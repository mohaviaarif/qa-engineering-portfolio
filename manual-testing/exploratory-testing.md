# Exploratory Testing

## Charter 1: Happy Path

Explore the app doing exactly what a normal, non-technical user will do - across all modules - and note everything unexpected, confusing or inconsistent, even if nothing technically breaks.

- Try to Sign up using new email and password - first it says can’t connect to server then when click on sign up again it shows email already registered - no code or confirmation message is sent to gmail.
- Try to Sign in using correct email and password - the account is login correctly.
- Scan 3 different emails through direct copy paste, “Upload .txt or .eml file” and “Scan PDF or Word Doc” - all emails are uploaded and scanned correctly - it give wrong result for the legitimate email body i uploaded it say it is spam( I think it is because it is train on checking the spam words and pattern which legitimate and spam emails have the same) - also when i upload email in live vercel app it spins the Analyze email button and then after some time it says “Can’t connect to backend. Make sure your backend is running”. So, I had then run the Cybersentinel Ai locally where it works correctly.
- Scan different links through URL scanner - all the URLs are scan correctly - as it checks with 10 rule base checks it somewhat give a legitimate link a suspicious ranking as some same links can also have same rules that it breaks such as too big of a link etc.
- Scan different files through “Scan file” - all the files are uploaded and checked correctly -only the files that are already present can be checked at the same time -the files that are newly made can’t be checked in first check (as it is uploaded to Virustotal server so it is a queue to be check) -we have to scan again to get the result back.
- Scan through extension -it doesn’t check the email- if we open any email and check “Scan Email” in extension it shows the email is from a trusted sender and say trusted however it gives the wrong sender address and says that all the emails have the same sender “google.com”.

## Charter 2: Malformed/adversarial inputs

Deliberately feed the system weird, broken or attack-like input across all modules, and note anything that behaves badly- not just “is this input rejected correctly”, but “does the system stay stable and safe even when handled roughly”.

- Sign up using a wrong @- signup using a wrong @ i.e @abv.com- it did not showed any error and was able to sign up.
- Sign up using any amount of letters password- it accepted it without showing error.
- Sign up using special characters/emojis in the email field- can sign up no errors.
- Tried logging in using SQL-injection-style strings in the email/password fields (`' OR '1'='1`)- was not able to login up.
- Pasted email of more than 5000 characters- message showed “Email text is too long (max 5000 chars).”
- Scanned text with html injections- scanned all of the scripts easily and give safe to all.
- Scanned “Mixed/non-English languages or mixed scripts”- it show safe without showing it contain urdu or emogi etc.
- Scanned random gibberish- it also showed safe for gibberish.
- Scanned a Url with unusual encoding (`%2E%2E%2F` style tricks, or heavily percent-encoded characters) it showed safe and even when it showed suspicous it showed it because of long URL and not unusual encoding.
- Scanned a URL with no scheme at all, just weird fragments (`javascript:alert(1)`)- cannot able to scan it- and error show that is “Something went wrong. Please try again.” which is not specific.
- Scanned an extremely long query string tacked onto an otherwise normal URL- scanned and give suspicious rating for medium long URL- cannot scan a very long URL and show this error “URL is too long.”
- Upload and scanned a file with a double/nested extension (`invoice.pdf.exe`)- Uploaded and scanned correctly.
- Upload a file with no extension at all- error shows “Unsupported file type (.invoice). Allowed: PDF, EXE, ZIP, DOCX, TXT, JS, PY, XLS.”.
- Uploaded a zero-byte file and click scan- error shows “File is empty. Please upload a file with content.”
- Upload and scan a file with a misleading name but wrong actual content (rename a .txt to .pdf and see what happens)- upload and was also able to scan.
- Scan through extension- it says every email is from trusted sender i.e.  [noreply-accounts@google.com](mailto:noreply-accounts@google.com) for all email scanned and say safe.

## Charter 3: Recovery/Error handling

Test what happens when something goes wrong mid-action and see if the system recovers gracefully.

- Refresh the page mid-scan- the page reloads, no error shows and the file is gone- the file is still scanned which we can see in “Scan History”.
- Submit a URL or email scan, and while it's showing a loading/processing state, click the "Scan" button again rapidly 3-4 times in a row (double/triple-submit)- can’t press the analyze email or check URL button again until one scan is complete and result is given.
- Start a file upload/scan, and while it's uploading or processing, **turn off your WiFi or disconnect your internet briefly**, then turn it back on- it shows and error “Unable to reach server. Make sure the backend is running.”- after reconnecting I have to “scan file” again.
- Log in successfully, then use your browser's **back button** a few times. See what happens — it reloads the main cybersentinel page and if I try it multiple time it goes back to browser main page.
- Log in, visit a page that requires being logged in (like a dashboard or scan history page), then **log out**, then click the browser's **back button once- it correctly stays on login page.**
- Hitting the browser back button after submitting- the scan is present in history to check.
- Closing and reopening the tab mid-analysis- present in history not on the same page.

---

## 🐞 Defects

1. Signup shows a false "can't connect to server" error on first attempt, even when the account is actually created successfully — misleading failure message masks a real success.
2. Chrome extension doesn't analyze emails at all — shows "trusted sender: [google.com](http://google.com/)" for every email regardless of actual sender.
3. Live deployment fails to process email-file uploads that work correctly locally ("Can't connect to backend" on Vercel).
4. Generic, unhelpful error ("Unable to reach server. Make sure the backend is running.") is used indiscriminately for both real backend outages and the user's own network dropping — not actionable for non-technical users.
5. Signup accepts malformed/invalid email addresses (wrong @, special characters, emojis) — no format validation.
6. Signup accepts arbitrarily weak passwords with no minimum length/complexity, and no maximum length either.
7. `javascript:` URL scheme produces a vague generic error ("Something went wrong") instead of a specific one.
8. File-type restriction is extension-only, not content-verified — renaming a file's extension bypasses the entire allowlist.
9. A file with no extension at all incorrectly displays ".filename" in the error message when the real filename had no dot/extension.
10. **[Critical] Scan history displays all users' scans, not filtered to the logged-in user** — a broken access control issue exposing potentially sensitive data across accounts.

## ❓ Questions

None remaining — both open items from Charter 2 (possible XSS via redisplayed text, the `.invoice` filename mystery) were investigated and resolved during testing.

## 💡 Enhancements

1. No independent heuristic for deceptive double-extensions (e.g. file.pdf.exe) beyond the full AV scan already run on new files.
2. No detection or flagging of non-English/mixed-script content (Urdu, emoji-heavy) — relevant given the Pakistani-localization claim.
3. URL-encoding attack patterns (e.g. %2E%2E%2F) aren't flagged as their own signal, only caught incidentally via unrelated rules.
4. No user-facing message confirming a scan continues in the background after a refresh or closed tab — confirmed via 2 separate tests that no data is lost, but the user isn't told that in the moment.

## ✅ Confirmed working correctly

- SQL injection blocked on login
- No XSS — raw scanned text is never redisplayed
- Rapid double/triple-submit correctly blocked by button disabling
- No cached authenticated content accessible via back button after logout
- Long-input limits correctly enforced with clear messages (in most, not all, places)