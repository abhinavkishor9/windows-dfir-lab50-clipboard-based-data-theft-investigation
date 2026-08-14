# windows-dfir-lab50-clipboard-based-data-theft-investigation

## Overview

The Windows clipboard is commonly used to temporarily store information copied by a user, including text, configuration data, source code, and other potentially sensitive information. While clipboard usage is normal, unauthorized access to clipboard contents can become a data-theft concern when a process reads the data and subsequently stores or transmits it.

In this hands-on DFIR lab, a controlled fake API key was placed into the Windows clipboard and accessed using PowerShell. The clipboard contents were then written to a local file to simulate a controlled collection and staging scenario. Windows Event Viewer, Sysmon telemetry, Windows Security Event ID 4688, file metadata, SHA-256 hashing, and digital-signature validation were used to investigate the resulting activity.

---

# Executive Summary

This investigation examined how clipboard data could be accessed and locally staged by a process. A completely artificial API-key-like string was placed into the clipboard and retrieved using PowerShell. The retrieved data was then written to a controlled investigation file to simulate a potential collection scenario without using real credentials or sensitive information.

Sysmon Event IDs 3 and 11 and Windows Security Event ID 4688 were available for investigation through Event Viewer. File metadata and SHA-256 hashing were also used to identify the staged artifact. The investigation demonstrated that clipboard access does not necessarily generate a dedicated Windows event proving clipboard theft, so process, file, network, and timestamp evidence must be correlated before making a data-theft determination.

---

# Investigation Objectives

- Understand the Windows clipboard as a potential source of sensitive data.
- Simulate clipboard access using completely controlled test data.
- Observe how clipboard data can be locally staged.
- Identify processes associated with clipboard-related activity.
- Examine Windows Security Event ID 4688.
- Investigate Sysmon Event ID 3 network telemetry.
- Investigate Sysmon Event ID 11 file-creation telemetry.
- Examine PowerShell activity related to clipboard access.
- Collect metadata from the staged clipboard file.
- Calculate a SHA-256 hash for the staged artifact.
- Validate digital-signature information where applicable.
- Correlate process, file, network, and timestamp evidence.
- Distinguish clipboard access from confirmed data theft.
- Document telemetry limitations.
- Produce an evidence-supported DFIR conclusion.

---

# Skills Demonstrated

- Clipboard Activity Investigation
- Windows DFIR
- PowerShell Investigation
- Sysmon Analysis
- Sysmon Event ID 3 Analysis
- Sysmon Event ID 11 Analysis
- Windows Security Event ID 4688 Analysis
- Process Investigation
- File Creation Analysis
- File Metadata Analysis
- SHA-256 Hashing
- Digital Signature Validation
- Event Viewer Investigation
- Evidence Correlation
- Timeline Construction
- Data Collection Analysis
- Forensic Documentation

---

# Tools Used

- Windows
- PowerShell
- Windows Event Viewer
- Sysmon
- File Explorer

---

# Lab Environment

| Component | Details |
| --- | --- |
| Operating System | Windows |
| Investigation Type | Host-Based DFIR |
| Primary Activity | Controlled Clipboard Access |
| Primary Tool | PowerShell |
| Supporting Telemetry | Sysmon |
| Process Creation Evidence | Windows Security Event ID 4688 |
| Network Telemetry | Sysmon Event ID 3 |
| File Telemetry | Sysmon Event ID 11 |
| Investigation Workspace | `C:\ClipboardTheftLab\` |

---

# Investigation Scenario

A user copies information into the Windows clipboard. Shortly afterward, a process accesses the clipboard contents and writes the retrieved information to a local file. This type of behavior could become suspicious if the data represented credentials, tokens, API keys, or other sensitive information and the process was not authorized to access it.

For this lab, the scenario was simulated safely using a completely fake API-key-like string. The data was placed into the clipboard, retrieved using PowerShell, and written to a controlled investigation file.

The investigation focused on determining what activity could be proven from available Windows telemetry.

---

# Investigation Workflow

1. Verify clipboard functionality.
2. Create the investigation workspace.
3. Create controlled fake sensitive data.
4. Place the test data into the clipboard.
5. Record the activity timestamp.
6. Review running processes.
7. Perform controlled clipboard access.
8. Write clipboard contents to a local staging file.
9. Examine the staged file.
10. Calculate its SHA-256 hash.
11. Investigate PowerShell process activity.
12. Review Sysmon Event ID 1.
13. Review Windows Security Event ID 4688.
14. Review Sysmon Event ID 3.
15. Review Sysmon Event ID 11.
16. Examine PowerShell Operational logging.
17. Validate the digital signature where applicable.
18. Correlate process, file, network, and timestamp evidence.
19. Assess whether the activity supports a data-theft conclusion.
20. Clean up the controlled investigation artifacts.

---

# Controlled Test Data

A fake API-key-like value was used:

    DEMO_API_KEY=LAB50-1234567890-FAKE

This value was intentionally created for the investigation.

No real:

- Passwords
- API keys
- Authentication tokens
- Credentials
- Company information
- Personal information

were used.

---

# Clipboard Activity

The test data was placed into the clipboard using PowerShell.

The clipboard was then read using:

    Get-Clipboard

This created the following controlled activity chain:

    Fake Test Data
          |
          v
      Clipboard
          |
          v
       PowerShell
          |
          v
    Clipboard Contents

The activity was intentionally performed in a controlled environment.

---

# Local Staging

To simulate a potential collection-and-staging behavior, the clipboard contents were written to:

    C:\ClipboardTheftLab\clipboard_capture.txt

The controlled activity therefore became:

    Clipboard
        |
        v
    PowerShell
        |
        v
    clipboard_capture.txt

The resulting file was examined as an investigation artifact.

---

# Sysmon Event ID 11

Sysmon Event ID 11 was investigated as file-creation telemetry.

The event can provide information about files created by processes.

The investigation focused on whether the controlled staging file was represented in the available Sysmon telemetry.

The relevant artifact was:

    C:\ClipboardTheftLab\clipboard_capture.txt

Event ID 11 was treated as supporting evidence of local staging rather than proof of malicious activity.

---

# Sysmon Event ID 3

Sysmon Event ID 3 was reviewed through Event Viewer to investigate network activity.

Relevant fields can include:

- Process
- Process ID
- Source IP
- Destination IP
- Destination Port
- Protocol
- Timestamp

The purpose was to determine whether network activity occurred around the clipboard-access and staging timeframe.

Network activity was treated as supporting evidence and was not automatically classified as malicious.

---

# Windows Security Event ID 4688

Windows Security Event ID 4688 was observed through Event Viewer.

Event ID 4688 provides process-creation information that can be used to correlate application activity.

The investigation considered:

- Process name
- Process ID
- Parent process
- Command line
- User
- Timestamp

The event was used as an additional source of process evidence.

---

# PowerShell Investigation

PowerShell was used to perform the controlled clipboard operations.

The relevant operations included:

    Set-Clipboard
    Get-Clipboard
    Out-File

PowerShell activity was investigated to understand the relationship between:

    Clipboard
        |
        v
    PowerShell
        |
        v
    Local File

PowerShell was not treated as malicious simply because it was involved in the controlled activity.

---

# File Metadata Analysis

The staged file was examined for:

- Filename
- Full path
- File size
- Creation time
- Last write time
- Last access time

These properties were used to establish when the file was created and modified.

Filesystem timestamps were correlated with process and security telemetry.

---

# SHA-256 Analysis

The staged file was hashed using:

    Get-FileHash "C:\ClipboardTheftLab\clipboard_capture.txt" -Algorithm SHA256

The resulting SHA-256 value provides a deterministic identifier for the artifact.

The hash can be used for:

- Evidence tracking
- Artifact correlation
- Future comparison
- Threat intelligence
- Malware analysis

---

# Digital Signature Analysis

Digital-signature validation was performed where applicable using:

    Get-AuthenticodeSignature "<path-to-file>"

The observed result was documented exactly as returned by Windows.

A digital-signature result should not be interpreted independently.

Forensic interpretation should consider:

- Signature status
- Signer
- File location
- File hash
- Process behavior
- Command line
- Network activity
- Timeline

---

# Evidence Correlation

The investigation correlated the following evidence sources:

    User
      |
      v
    Clipboard
      |
      v
    PowerShell
      |
      +---- Process Creation
      |        |
      |        +---- Security Event 4688
      |        +---- Sysmon process telemetry
      |
      +---- Local Staging
      |        |
      |        +---- Sysmon Event ID 11
      |        +---- File Metadata
      |        +---- SHA-256
      |
      +---- Network Activity
               |
               +---- Sysmon Event ID 3

This correlation approach helps distinguish simple clipboard usage from a potentially suspicious collection chain.

---

# Investigation Findings

The investigation successfully demonstrated controlled clipboard access and local staging using PowerShell.

A fake API-key-like value was placed into the Windows clipboard, retrieved using PowerShell, and written to a controlled file inside the investigation workspace.

Sysmon Event IDs 3 and 11 and Windows Security Event ID 4688 were available for investigation through Event Viewer. These telemetry sources provided supporting evidence for network, file, and process activity.

The investigation demonstrated that clipboard access itself may not produce a dedicated Windows event explicitly stating that clipboard data was stolen. Therefore, clipboard-related investigations require correlation between process behavior, file activity, network activity, and timestamps.

---

# Suspicious Indicators in a Real Investigation

The following combination would increase concern during a real incident:

    User copies sensitive information
            |
            v
    Unknown process accesses clipboard
            |
            v
    Data written to temporary file
            |
            v
    File compressed or encoded
            |
            v
    Process establishes network connection
            |
            v
    Data transmitted externally

Additional suspicious indicators could include:

- Unsigned executable
- Executable from Downloads
- Executable from Temp
- Unknown application
- Suspicious command line
- PowerShell execution
- Unexpected outbound connection
- Newly created executable
- Unusual parent-child relationship
- Data staging immediately before network communication

These indicators would require further investigation and should not automatically be treated as proof of data theft.

---

# MITRE ATT&CK Mapping

| Technique | Description |
| --- | --- |
| T1059.001 | PowerShell |
| T1114 | Email Collection |
| T1005 | Data from Local System |
| T1071.001 | Web Protocols |

The mappings represent techniques that may become relevant in a real clipboard-data-theft investigation. The controlled lab itself does not establish malicious use of these techniques.

---

# Evidence Collected

- Controlled clipboard test data
- Clipboard access timestamp
- PowerShell process information
- Windows Security Event ID 4688
- Sysmon Event ID 3
- Sysmon Event ID 11
- Staged clipboard file
- File metadata
- SHA-256 hash
- Digital-signature information
- PowerShell logging observations
- Investigation timeline

---

# Limitations

The investigation used completely controlled and artificial data.

The fake API-key-like value was not a real credential.

The activity was intentionally performed using PowerShell, so the presence of PowerShell does not establish malicious behavior.

The presence of Sysmon Event ID 3 does not automatically indicate data exfiltration.

The presence of Sysmon Event ID 11 does not automatically indicate malicious file creation.

Windows Security Event ID 4688 establishes process creation but does not independently establish malicious intent.

Most importantly, clipboard access does not necessarily generate a dedicated event that directly proves clipboard theft.

A real investigation would require additional evidence such as:

- EDR telemetry
- Malware analysis
- Memory analysis
- Network packet analysis
- Proxy logs
- DNS logs
- Data Loss Prevention alerts
- File-system artifacts
- Process-access telemetry
- Threat intelligence
- Authentication telemetry

---

# Key Takeaway

Clipboard usage is normal Windows behavior, but unauthorized clipboard access can become a serious data-theft concern when sensitive information is collected and subsequently staged or transmitted.

The investigation demonstrated how a controlled clipboard-access scenario can be reconstructed using process, file, network, and security telemetry.

The most important DFIR lesson is that **clipboard access, file creation, or network activity alone is not enough to prove data theft**.

A defensible conclusion requires correlation of the complete activity chain:

    Clipboard Access
          +
    Process Evidence
          +
    File Staging
          +
    Network Activity
          +
    Timeline
          =
    Evidence-Based Assessment
