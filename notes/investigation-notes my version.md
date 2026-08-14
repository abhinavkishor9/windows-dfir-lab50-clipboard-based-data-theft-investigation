# Investigation Notes — Lab 50: 

## Investigation Scenario


A user copies information from a document or application into the Windows clipboard during normal workstation activity. Shortly afterward, security telemetry shows process and file activity occurring around the same period, creating a possibility that the clipboard contents may have been accessed and collected.

The SOC analyst investigates the activity to determine:

Whether a process accessed the clipboard contents.
Which process was responsible for the activity.
Whether the clipboard data was stored in a local file.
Whether any network communication occurred afterward.
Whether the process and file activity occurred within the same timeframe.
Whether the activity represents normal user behavior or possible unauthorized data collection.

The investigation focuses on correlating the available endpoint evidence and determining whether there is enough evidence to classify the activity as clipboard-based data theft.

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


