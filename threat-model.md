# threat-model

## **Core Module 1: Analyze Email**

**Questions**

1. Users — who interacts with it (a visitor, an admin)?

Visitor/admin

1. Inputs — what data goes in (a URL, an uploaded file, an email)?

Email(Copy Pasted), file upload(.txt, .eml, pdf, word doc)

1. Outputs — what comes back (a verdict, a score, a report)?

Verdict, Confidence level, Risk level, Indicators, Recommendation, Extracted link (Link malicious verdict), Report

1. Dependencies — what does it rely on to work (your ML model, an external API, a database)?

ML Model, Written Code

**Trust Boundary**

Uploaded file or pasted text and clicked scan email.

**What's the worst realistic thing that could go wrong here?**

1. [Bug]Wrong uploaded file type
2. [Bug]Wrong ML model result
3. [Bug]Email size bigger then limit
4. [Bug]Rate limiting does not applied
5. [Bug]Text does not extract from the uploaded file
6. [Bug]Link is not extracted from email text
7. [Bug]Link does not pass correctly to the rule checking program
8. [Bug]Email wording in other language
9. [Security]Feeding inputs designed to fool your ML model into a wrong verdict
10. [Security]Attack on our backend using hidden xml or other ways

## **Core Module 2: Scan URL**

**Questions**

1. Users — who interacts with it (a visitor, an admin)?

Visitor/admin

1. Inputs — what data goes in (a URL, an uploaded file, an email)?

URL(Copy Pasted)

1. Outputs — what comes back (a verdict, a score, a report)?

Verdict, Risk level, Thread score, Confidence Score, AbuseIPDB Score, Indicators,                                                                                                      Recommendation, Report

1. Dependencies — what does it rely on to work (your ML model, an external API, a database)?

External API( AbuseIPDB), Database(already scanned urls), Written Code, Internet Connection

**Trust Boundary**

Pasted Link and clicked scan link.

**What's the worst realistic thing that could go wrong here?**

1. [Bug]Backend crash
2. [Bug]Link is bigger than acceptable limit
3. [Bug]Server down
4. [Bug]API did not work
5. [Bug]Link have something else other than the 10 rules we check
6. [Bug]Rate limiting not implied
7. [Bug]Wrong prediction
8. [Bug]After prediction link is not stored in database
9. [Security]A crafted url for causing un expected behavior
10. [Bug]10 rule check didn’t work properly
11. [Security]**URL module — SSRF risk.** If your backend fetches/visits the URL server-side to check it (rather than only querying AbuseIPDB about it), an attacker could submit a URL pointing at your own internal network/localhost and probe things they shouldn't reach. Worth a line even if you're not sure — flag it as "needs verification."

## **Core Module 3: Scan File**

**Questions**

1. Users — who interacts with it (a visitor, an admin)

Visitor/admin

1. Inputs — what data goes in (a URL, an uploaded file, an email)?

Uploaded file(txt, pdf, doc, docx, exe, js, zip. py, xls, xlsx, csv, json, dll, bat, ps1, vbs)

1. Outputs — what comes back (a verdict, a score, a report)?

Verdict, Engine Detection, SHA-256 Hash, Indicators, Recommendation, Report

1. Dependencies — what does it rely on to work (your ML model, an external API, a database)?

API(VirusTotal), Written Code, Internet Connection

**Trust Boundary**

Uploaded file clicked scan file.

**What's the worst realistic thing that could go wrong here?**

1. [Bug]did not work
2. [Bug]Wrong file type
3. [Bug]File did not delete immediately after converting to hash
4. [Bug]More file size
5. [Bug]No internet connection
6. [Bug]Backend do not convert the file to hash
7. [Bug]Rate limiting isn’t applied
8. [Bug]Download report does not work
9. [Bug]Recommendation and indicators are not correct
10. [Security]File not validated
11. [Bug]**File module — executable payload risk.** You accept .exe, .js, .py, .bat, .ps1, .vbs — if anything in your pipeline ever opens/runs these rather than just hashing them, that's a real code-execution risk, not just a "wrong file type" bug.

## **Login Module**

**Trust Boundary**

Write Credential and click login.

**What's the worst realistic thing that could go wrong here?**

1. [Bug]BRUTE FORCE attack due to improper rate limiting
2. [Bug]Broken Access control risk( a non admin accessing only admin data)
3. [Bug]No account lockout after repeated failed attempts
4. [Bug]No session tokens save seperately

## **Signup Module**

**Trust Boundary**

Write Credential and click Sign up.

**What's the worst realistic thing that could go wrong here?**

1. [Bug]Multiple account from same person
2. [Bug]No rate limiting applied

## Requirements (derived from the system map)

### Email Module

- REQ-E1: Classify submitted email text/files as Safe or Phishing with a confidence score
- REQ-E2: Accept .txt, .eml, PDF, and Word document uploads
- REQ-E3: Reject unsupported file types
- REQ-E4: Reject emails/files exceeding the size limit, with a clear, specific error message
- REQ-E5: Scan any URLs found within email content through the URL analyzer

### URL Module

- REQ-U1: Classify submitted URLs as Safe, Suspicious, or Malicious using the 10-feature score plus AbuseIPDB reputation
- REQ-U2: Detect structural red flags (IP-as-domain, @ symbol spoofing, suspicious TLDs, etc.)
- REQ-U3: Store scanned URLs and verdicts for future lookups

### File Module

- REQ-F1: Compute a SHA-256 hash of any uploaded file
- REQ-F2: Delete the uploaded file after hashing, except when a first-time VirusTotal analysis requires the actual file
- REQ-F3: Reject files exceeding 32MB, with a specific size-limit error message
- REQ-F4: Reject unsupported file types
- REQ-F5: Return a verdict (Clean/Suspicious/Malicious/Pending) based on VirusTotal's 70+ engine results

### Login / Signup

- REQ-L1: Rate-limit repeated failed login attempts
- REQ-L2: Prevent duplicate account registration with the same email