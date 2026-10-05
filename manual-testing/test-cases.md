# test-cases

# **Test Cases for CyberSenitnel AI**

1. ***Test Case ID: TC_01***

Title: Scan a known-safe URL returns a "safe" verdict

Preconditions: CyberSentinel is running, user is on the Scan URL page

Test Steps:

1. Paste a well-known safe URL (e.g. https://www.google.com) into the input field
2. Click "Scan Link"

Test Data: https://www.google.com

Expected Result: System returns a verdict of "Safe" / low risk score, no error shown

Actual Result: Safe, Risk Score 0/10

Status: Pass

1. ***Test Case ID: TC_02***

Title: Scan a known-unsafe URL returns a "Malicious" verdict

Preconditions: CyberSentinel is running, user is on the Scan URL page

Test Steps:

1. Paste an unsafe URL (e.g http://paypal-secure-verify-account-login.com/update/confirm) into the input field
2. Click "Scan Link"

Test Data: http://paypal-secure-verify-account-login.com/update/confirm

Expected Result: System returns a verdict of "Malicious" / high risk score, no error shown

Actual Result: Malicious, Risk Score 4/10

Status: Pass

1. ***Test Case ID: TC_03***

Title: Without pasting anything the system should handle it gracefully

Preconditions: CyberSentinel is running, user is on the Scan URL page

Test Steps:

1. Paste nothing into the input field
2. Click "Scan Link"

Test Data: empty

Expected Result: "System shows an error like 'Please enter a URL to scan' and does not attempt a scan.”

Actual Result: it shows” Please enter a URL to analyze.” and also did not scan anything.

Status: Pass

1. ***Test Case ID: TC_04***

Title: Pasting a paragraph and scanning the system should handle it gracefully

Preconditions: CyberSentinel is running, user is on the Scan URL page

Test Steps:

1. Paste a paragraph into the input field
2. Click "Scan Link"

Test Data: Any paragraph

Expected Result: "System shows an error like 'Please enter a URL to scan' and does not attempt a scan.”

Actual Result: scan it because in program I have added a thing where it added https to any text pasted in the URL scan box when it doesn’t have http by itself

Status: Fail

1. ***Test Case ID: TC_05***

Title: Scanning more than 10 times a minute should make the user stop for scanning for few moments

Preconditions: CyberSentinel is running, user is on the Scan URL page

Test Steps:

1. Paste anything into the input field
2. Click "Scan Link" more than 10 times in 1 minute

Test Data: Anything

Expected Result: System returns a verdict of “Limit Reached please try again after few minutes”

Actual Result: message comes “Rate limit exceeded. Try again later. “ and also did not scan.

Status: Pass

1. Test Case ID: TC_06

Title: Scan a legitimate email returns a "Safe" verdict
Precondition: CyberSentinel is running, user is on the Analyze Email page
Data: A genuine, benign email (e.g. a real newsletter or personal email, pasted as text)
Steps:

1. Paste the email text into the input field
2. Click "Scan Email"

Expected Result: System returns "Safe" verdict with a low/appropriate confidence score, no error

Actual Result: System returns "Safe" verdict with 94 %confidence score, no error
Priority: High

Type: Functional (Positive)
Status: Pass

1. Test Case ID: TC_07 

Title: Unsupported file upload should give warning
Precondition: CyberSentinel is running, user is on the Analyze Email page
Data: An unsupported file (e.g. a zip file)
Steps:

1. Click on “Upload .txt or .eml file” to find zip files
2. Click "Scan PDF or Word Doc” to find zip files

Expected Result: Cannot upload unsupported files

Actual Result: Unsupported files cannot be selected
Priority: Medium

Type: Negative
Status: Pass

1. Test Case ID: TC_08

Title: Submit an email/file exceeding the size limit 
Precondition: CyberSentinel is running, user is on the Analyze Email page
Data: An Email file (larger then 32mb size limit)
Steps:

1. Click on “Scan PDF or Word Doc”
2. Click on email file larger then 32 mb

Expected Result: Error showing “Email Size bigger than limit”

Actual Result: Error Showing “Scan Failed, please try again”
Priority: Medium

Type: Boundary
Status: Pass but error wording should be changed

1. Test Case ID: TC_09

Title: Try an obfuscated phishing email (e.g. 'p4ypal' style tricks)
Precondition: CyberSentinel is running, user is on the Analyze Email page
Data: Obfuscated phishing email
Steps:

1. Paste the email text into the input field
2. Click "Scan Email"

Expected Result: System returns "Phishing" verdict with a low/appropriate confidence score, no error

Actual Result: System returns "Phishing" verdict with 98% confidence score, no error
Priority: High

Type: Security
Status: Pass

1. Test Case ID: TC_10

Title: Scan a genuinely clean file returns a "Safe" verdict
Precondition: CyberSentinel is running, user is on the Scan File page
Data: Genuinely Clean File
Steps:

1. Select a clean file 
2. Click "Scan File"

Expected Result: System returns "Clean" verdict with no engine harmful detection, no error

Actual Result: System returns "Clean" verdict with no engine harmful detection, no error
Priority: High

Type: Functional
Status: Pass

1. Test Case ID: TC_11

Title: Scan the EICAR test file (a safe, industry-standard fake-malware test file used specifically for this purpose)
Precondition: CyberSentinel is running, user is on the Scan File page
Data: EICAR test file
Steps:

1. Select EICAR test file
2. Click "Scan File"

Expected Result: System returns "Malicious" verdict with many engines harmful detection, no error

Actual Result: System returns "Malicious" verdict with many engines harmful detection, no error
Priority: High

Type: Functional and Security
Status: Pass

1. Test Case ID: TC_12

Title: Upload an unsupported file type
Precondition: CyberSentinel is running, user is on the Scan File page
Data: Unsupported file type
Steps:

1. Select unsupported file type
2. Click "Scan File"

Expected Result: Files are not available to be selected, no error

Actual Result: The files that are unsupported are not available to be selected, no error
Priority: Medium

Type: Negative
Status: Pass

1. Test Case ID: TC_13

Title: Upload a file over your 32MB limit.
Precondition: CyberSentinel is running, user is on the Scan File page
Data: Supported file more than 32mb
Steps:

1. Select file more than 32mb
2. Click "Scan File"

Expected Result: Error shows that “File is too large (** MB). Maximum allowed size is 32 MB.

Actual Result: Error shows that “File is too large (** MB). Maximum allowed size is 32 MB.
Priority: Medium

Type: Boundry
Status: Pass

1. Test Case ID: TC_14

Title: Verify the file is actually deleted immediately after hashing
Precondition: CyberSentinel is running, user is on the Scan File page
Data: Any file
Steps:

1. Select any file
2. Click "Scan File"

Expected Result: File immediately deleted after hashing

Actual Result: It shows that file is deleted after hashing except for files that are new in which case file are uploaded to Virustotal which then scan it on many engines (I know this because I have made this thing myself)
Priority: Medium 

Type: Security
Status: Pass

1. Test Case ID: TC_15

Title: Submit a URL using an IP address instead of a domain
Precondition: CyberSentinel is running, user is on the Scan URL page
Data: IP Address instead of domain
Steps:

1. Paste an IP Address into the text field
2. Click "Scan URL"

Expected Result: Result show “Malicious” with clear warnings

Actual Result: Result show “Malicious” with clear warnings
Priority: High

Type: Security
Status: Pass

1. Test Case ID: TC_16

Title: Submit a URL using the @ symbol spoofing trick
Precondition: CyberSentinel is running, user is on the Scan URL page
Data: URL with @ symbol spoofing trick
Steps:

1. Paste an URL with @ symbol spoofing trick
2. Click "Scan URL"

Expected Result: Result show “Malicious” with clear warnings

Actual Result: Result show “Malicious” with clear warnings
Priority: High

Type: Security
Status: Pass

1. Test Case ID: TC_17

Title: Attempt more than a handful of failed login attempts in a row 
Precondition: CyberSentinel is running, user is on the login page
Data: Correct Gmail with incorrect password
Steps:

1. Write incorrect passwords multiple times
2. Click “Login”

Expected Result: Error show after few attempts “Rate limit exceeded. Try again later.”

Actual Result: Error show after few attempts “Rate limit exceeded. Try again later.”
Priority: High

Type: Security
Status: Pass

1. Test Case ID: TC_18

Title: Try creating a second account with the same email or details
Precondition: CyberSentinel is running, user is on the signup page
Data: Previously register email and password
Steps:

1. Write Previously register email and password
2. Click “Sign up”

Expected Result: Error show “Email already registered”

Actual Result: Error show “Email already registered”
Priority: High

Type: Negative and Security 
Status: Pass