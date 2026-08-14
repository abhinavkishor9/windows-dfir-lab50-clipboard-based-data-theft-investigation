# Timeline — Lab 50: Clipboard-Based Data Theft Investigation

## Investigation Timeline

| Step | Activity | Evidence / Result |
| --- | --- | --- |
| 1 | Investigation started | Clipboard-based data-theft scenario established |
| 2 | Workspace created | `C:\ClipboardTheftLab\` created |
| 3 | Fake sensitive data created | Artificial API-key-like value generated |
| 4 | Clipboard populated | Controlled test data placed into clipboard |
| 5 | Timestamp recorded | Investigation reference time established |
| 6 | Clipboard verified | Test data successfully retrieved |
| 7 | PowerShell accessed clipboard | Controlled clipboard-access activity performed |
| 8 | Clipboard contents staged | Data written to `clipboard_capture.txt` |
| 9 | Staged file examined | Metadata collected |
| 10 | SHA-256 calculated | Artifact hash generated |
| 11 | PowerShell process examined | Process information collected |
| 12 | Sysmon Event ID 1 reviewed | Process creation telemetry investigated |
| 13 | Security Event ID 4688 observed | Process creation evidence available in Event Viewer |
| 14 | Sysmon Event ID 11 observed | File-creation telemetry available |
| 15 | Sysmon Event ID 3 observed | Network telemetry available |
| 16 | PowerShell logging reviewed | Clipboard-related logging investigated |
| 17 | Digital signature checked | File trust information evaluated |
| 18 | Evidence correlated | Process, file, network and timeline evidence compared |
| 19 | Data-theft hypothesis assessed | Collection and exfiltration claims separated |
| 20 | Investigation completed | Findings and limitations documented |
| 21 | Cleanup performed | Controlled artifacts removed and clipboard cleared |

---

# Detailed Timeline

## Phase 1 — Preparation

### T+00 — Investigation Started

The Clipboard-Based Data Theft Investigation was initiated.

The objective was to understand how clipboard data could be accessed, locally staged, and potentially investigated through Windows endpoint telemetry.

### T+05 — Workspace Created

The investigation directory was created:

    C:\ClipboardTheftLab\

The directory was used to contain controlled investigation artifacts.

### T+10 — Controlled Test Data Created

A fake API-key-like string was created:

    DEMO_API_KEY=LAB50-1234567890-FAKE

No real sensitive information was used.

---

# Phase 2 — Clipboard Activity

### T+15 — Clipboard Populated

The controlled test data was placed into the Windows clipboard.

The clipboard was then verified using:

    Get-Clipboard

The expected test value was returned.

### T+20 — Activity Timestamp Recorded

The current time was recorded using PowerShell.

This timestamp was used as a reference point when reviewing subsequent endpoint telemetry.

### T+25 — Clipboard Access Performed

PowerShell retrieved the clipboard contents.

The controlled process chain was:

    Clipboard
        |
        v
    PowerShell
        |
        v
    Clipboard Contents

---

# Phase 3 — Local Staging

### T+30 — Clipboard Contents Written to File

The retrieved clipboard data was written to:

    C:\ClipboardTheftLab\clipboard_capture.txt

The resulting activity was:

    Clipboard
        |
        v
    PowerShell
        |
        v
    clipboard_capture.txt

### T+35 — Staged File Examined

The staged file was examined for:

- Filename
- Full path
- Size
- Creation time
- Last write time
- Last access time

### T+40 — SHA-256 Calculated

A SHA-256 hash was generated for:

    clipboard_capture.txt

The hash was recorded as an artifact identifier.

---

# Phase 4 — Process Investigation

### T+45 — PowerShell Process Examined

PowerShell process information was reviewed.

The investigation considered:

- Process ID
- Process name
- Executable path
- Parent process
- Command line

### T+50 — Sysmon Process Telemetry Reviewed

Sysmon process-creation telemetry was reviewed to establish process context around the controlled clipboard activity.

Event ID 1 was considered where available.

---

# Phase 5 — Windows Security Investigation

### T+55 — Security Event ID 4688 Observed

Windows Security Event ID 4688 was observed through Event Viewer.

The event was reviewed as process-creation evidence.

Relevant information included:

- Process
- PID
- Parent process
- Command line
- User
- Timestamp

---

# Phase 6 — File Investigation

### T+60 — Sysmon Event ID 11 Observed

Sysmon Event ID 11 was observed through Event Viewer.

The event was relevant to the investigation because the controlled clipboard data was written to a local file.

The investigation looked for activity associated with:

    C:\ClipboardTheftLab\clipboard_capture.txt

### T+65 — File Timeline Correlated

The file creation timestamp was compared with the clipboard-access and process activity timestamps.

The objective was to establish whether the events occurred within the expected investigation window.

---

# Phase 7 — Network Investigation

### T+70 — Sysmon Event ID 3 Observed

Sysmon Event ID 3 was observed through Event Viewer.

The investigation considered:

- Process
- Destination
- Port
- Protocol
- Timestamp

### T+75 — Network Activity Assessed

Network telemetry was evaluated in relation to the clipboard-access and file-staging activity.

The presence of a network connection was not automatically interpreted as data exfiltration.

---

# Phase 8 — PowerShell Investigation

### T+80 — PowerShell Operational Logging Reviewed

PowerShell Operational logging was checked for evidence associated with clipboard activity.

The investigation considered commands such as:

    Set-Clipboard
    Get-Clipboard
    Out-File

The availability of detailed command information depended on the endpoint's PowerShell logging configuration.

---

# Phase 9 — File Trust Investigation

### T+85 — Digital Signature Checked

Digital-signature validation was performed using:

    Get-AuthenticodeSignature "<path-to-file>"

The result was recorded exactly as returned by Windows.

### T+90 — File Evidence Correlated

The following file evidence was considered together:

    Filename
        +
    Path
        +
    Metadata
        +
    SHA-256
        +
    Signature
        +
    Process Context

---

# Phase 10 — Evidence Correlation

### T+95 — Process Evidence Correlated

Windows Security Event ID 4688 and available Sysmon process telemetry were considered together.

### T+100 — File Evidence Correlated

Sysmon Event ID 11 and filesystem metadata were compared with the staging timestamp.

### T+105 — Network Evidence Correlated

Sysmon Event ID 3 was reviewed against the same time window.

The analytical model was:

    Clipboard Access
          |
          v
       Process
          |
          +---- File Creation
          |
          +---- Network Activity
          |
          v
       Timeline

---

# Phase 11 — Final Assessment

### T+110 — Collection Assessment

The investigation established that controlled clipboard contents could be accessed and written to a local file.

### T+115 — Exfiltration Assessment

Network telemetry was reviewed to determine whether evidence supported external transmission.

Clipboard access and local staging were not automatically interpreted as exfiltration.

### T+120 — Evidence Limitations Recorded

The investigation documented that Windows may not provide a dedicated event explicitly stating:

    Process read clipboard contents

Therefore, process, file, network, and timeline correlation remained essential.

### T+125 — Investigation Completed

The investigation was completed after reviewing the available evidence and documenting the findings and limitations.

---

# Final Timeline Summary

| Phase | Key Activity | Evidence |
| --- | --- | --- |
| Preparation | Workspace created | Filesystem |
| Preparation | Fake data generated | Controlled test data |
| Clipboard | Clipboard populated | `Get-Clipboard` |
| Clipboard | Clipboard accessed | PowerShell |
| Collection | Data staged locally | `clipboard_capture.txt` |
| File | File metadata examined | Filesystem |
| File | Hash calculated | SHA-256 |
| Process | Process activity reviewed | Sysmon |
| Process | Process creation observed | Security Event ID 4688 |
| File | File creation telemetry observed | Sysmon Event ID 11 |
| Network | Network telemetry observed | Sysmon Event ID 3 |
| PowerShell | Clipboard commands investigated | PowerShell logging |
| Trust | Signature validated | Authenticode |
| Correlation | Evidence combined | DFIR timeline |
| Assessment | Collection vs exfiltration separated | Evidence analysis |
| Cleanup | Controlled artifacts removed | Filesystem / Clipboard |

---

# Investigation Conclusion

The timeline demonstrates how a controlled clipboard-access scenario can be reconstructed through multiple Windows evidence sources.

The investigation connected clipboard access to PowerShell activity and local file staging while using Sysmon Event IDs 3 and 11 and Windows Security Event ID 4688 as supporting telemetry. The evidence was evaluated as a sequence of related activities rather than treating any individual event as proof of data theft.

The key forensic distinction is between **clipboard access, local collection, and confirmed external exfiltration**. Each requires progressively stronger evidence before it can be reported as an established finding.
