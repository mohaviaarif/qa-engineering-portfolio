# bugs

## **Bugs found in CyberSentinel AI**

Bug ID: BUG_01

Found in: TC_04 — Scan URL module

Summary: Non-URL text input (paragraph) is auto-prefixed with https:// and scanned as a URL instead of being rejected.

Severity: Medium — input validation gap; scanner processes malformed/non-URL input silently.

Status: Open — logged, not yet fixed.

Bug ID: BUG_02
Found in: TC_08 — Analyze Email module
Summary: Oversized file upload shows a generic "Scan Failed" error instead of a specific size-limit message (unlike the File module, which handles this correctly).
Severity: Low — cosmetic/usability, not a functional failure.
Status: Open — logged, not yet fixed.