# Investigation Notes — Lab 50: Clipboard-Based Data Theft Investigation

## Investigation Scenario

A user copies information into the Windows clipboard. A process subsequently accesses the clipboard contents and stores the retrieved information in a local file. This type of behavior could indicate data collection if the clipboard contained sensitive information and the process was unauthorized.

The lab used a completely fake API-key-like value and a controlled PowerShell workflow to simulate the activity safely.

---

# Initial Hypothesis

The investigation began with the possibility that clipboard contents could be accessed by a process and subsequently staged on the local system.

The objective was to determine what evidence would be available to support such a hypothesis.

---

# Controlled Data

The following artificial value was used:

    DEMO_API_KEY=LAB50-1234567890-FAKE

This value was intentionally created for the lab.

No real sensitive information was used.

---

# Investigation Workspace

The investigation workspace was:

    C:\ClipboardTheftLab\

The workspace was used to store the controlled staging artifact.

---

# Clipboard Preparation

The test data was placed into the Windows clipboard using PowerShell.

The clipboard contents were then verified using:

    Get-Clipboard

This confirmed that the controlled data was available in the clipboard.

---

# Controlled Clipboard Access

PowerShell was used to retrieve the clipboard contents.

The controlled activity was:

    Clipboard
        |
        v
    PowerShell
        |
        v
    Clipboard Contents

This demonstrated the basic mechanism that an application could use to access clipboard data.

---

# Local Staging

The retrieved clipboard contents were written to:

    C:\ClipboardTheftLab\clipboard_capture.txt

This created a controlled collection-and-staging scenario:

    Clipboard
        |
        v
    PowerShell
        |
        v
    clipboard_capture.txt

The file was then examined as forensic evidence.

---

# File Metadata

The staged file was examined for:

- Name
- Full path
- Size
- Creation time
- Last write time
- Last access time

The timestamps were useful for correlating file creation with the clipboard-access activity.

---

# SHA-256

The staged file was hashed using:

    Get-FileHash "C:\ClipboardTheftLab\clipboard_capture.txt" -Algorithm SHA256

The resulting SHA-256 hash was treated as the unique identifier for the evidence file.

---

# Process Investigation

PowerShell process information was reviewed to determine the process responsible for the controlled clipboard operations.

The investigation considered:

- Process name
- Process ID
- Executable path
- Parent process
- Command line
- Timestamp

The purpose was to establish process context around the clipboard access.

---

# Windows Security Event ID 4688

Windows Security Event ID 4688 was observed through Event Viewer.

The event was reviewed as process-creation evidence.

The investigation considered:

- Process
- Process ID
- Parent process
- Command line
- User
- Timestamp

The event provided an additional source for correlating PowerShell activity.

---

# Sysmon Event ID 11

Sysmon Event ID 11 was available and reviewed through Event Viewer.

The event was relevant because the controlled clipboard contents were written to a local file.

The investigation looked for evidence associated with:

    C:\ClipboardTheftLab\clipboard_capture.txt

Event ID 11 was treated as supporting evidence of file creation and local staging.

---

# Sysmon Event ID 3

Sysmon Event ID 3 was available and reviewed through Event Viewer.

The purpose was to determine whether network activity occurred around the clipboard-access and staging timeframe.

The investigation considered:

- Process
- Source address
- Destination address
- Destination port
- Protocol
- Timestamp

Network activity was treated as supporting evidence.

---

# PowerShell Logging

PowerShell Operational logging was examined to determine whether clipboard-related commands were recorded.

The investigation considered commands such as:

    Get-Clipboard
    Set-Clipboard
    Out-File

The availability of these details depends on the PowerShell logging configuration.

If a specific command was not present in the logs, that absence was documented as a telemetry limitation.

---

# Digital Signature

Digital-signature validation was performed using:

    Get-AuthenticodeSignature "<path-to-file>"

The exact result returned by Windows was recorded.

The signature result was not treated as an independent maliciousness verdict.

---

# Evidence Correlation

The investigation correlated:

| Evidence | Purpose |
| --- | --- |
| Clipboard data | Establish controlled input |
| PowerShell | Identify clipboard-access process |
| Event ID 4688 | Process creation correlation |
| Sysmon Event ID 11 | File creation and staging |
| Sysmon Event ID 3 | Network activity |
| File metadata | Establish file timeline |
| SHA-256 | Identify artifact |
| Digital signature | Assess file trust |
| Timestamps | Correlate activity |

The overall analytical model was:

    Clipboard
        |
        v
    Process Access
        |
        +---- Process Creation
        |
        +---- File Creation
        |
        +---- Network Activity
        |
        v
    Evidence Correlation

---

# Findings

The controlled investigation successfully demonstrated that clipboard contents can be accessed through PowerShell and written to a local file.

The resulting file provided a tangible artifact that could be examined using metadata and hashing.

Sysmon Event IDs 3 and 11 and Windows Security Event ID 4688 were available through Event Viewer and were used as supporting telemetry sources.

The investigation did not treat the controlled activity as malicious because the activity was intentionally performed by the investigator.

---

# What the Evidence Can Prove

The controlled scenario can establish:

- The test data existed in the clipboard.
- PowerShell retrieved the clipboard contents.
- The contents were written to a local file.
- The file existed at a known path.
- The file had identifiable filesystem timestamps.
- The file had a SHA-256 hash.
- Process and security telemetry could be reviewed.
- Network telemetry could be checked for related communication.

---

# What the Evidence Cannot Prove by Itself

The evidence does not automatically prove:

- Malware was responsible.
- A real credential was stolen.
- Data was transmitted externally.
- An attacker controlled the system.
- The clipboard access was unauthorized.

Those conclusions require additional evidence.

---

# Real-World Investigation Comparison

A real suspicious scenario might look like:

    User copies credential
            |
            v
    Unknown process
            |
            v
    Clipboard access
            |
            v
    Data staging
            |
            v
    Network connection
            |
            v
    External destination

The analyst would then correlate:

- Process lineage
- File creation
- Network connection
- Destination reputation
- User activity
- Endpoint telemetry
- Authentication activity
- Threat intelligence

---

# Final Assessment

The lab demonstrated the difference between **clipboard access** and **confirmed clipboard-based data theft**.

Clipboard access becomes more concerning when it is performed by an unexpected process, followed by local staging or network communication, and supported by additional evidence.

A strong DFIR conclusion must therefore describe exactly what was observed and avoid claiming data theft when the available evidence only demonstrates clipboard access or file staging.
