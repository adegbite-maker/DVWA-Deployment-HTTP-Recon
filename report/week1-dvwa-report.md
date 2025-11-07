# Week 1 — Recon & HTTP fundamentals — DVWA

Target: 192.168.211.129:8080
Date: $(date -I)

## Purpose
Set up DVWA, perform HTTP recon, capture login flow, and test session management.

## Tools used
docker, curl, nmap, gobuster, nikto, Burp Suite

## Scans (saved in scans/)
- curl headers: scans/curl_headers.txt
- full nmap: scans/dvwa_full.nmap (and .gnmap, .xml)
- focused http nmap: scans/dvwa_http_8080.txt
- directory discovery: scans/gobuster_root.txt
- nikto scan: scans/nikto_dvwa.txt
- setup & login pages: scans/setup_preview.html, scans/login_page.html
- (optional) reflection test: scans/reflection_test.html

## Session & Cookie Tests (evidence in evidence/)
- Login capture: evidence/login_request.txt
- Session fixation simulation: evidence/login_post_response_headers.txt and evidence/protected_with_attacker_headers_after_login.txt
  - Result: **NOT VULNERABLE** — attacker-supplied PHPSESSID did not become authenticated; server redirected to login (HTTP 302).
- Normal login: evidence/login_normal_response_headers.txt
- Logout invalidation: evidence/logout_response_headers.txt and evidence/protected_after_logout_headers.txt
  - Result: **OK** — after logout, pre-logout cookie no longer grants access (redirect to login).

## Summary findings
- Open ports and service: 8080 (Apache/PHP DVWA). See scans/dvwa_http_8080.txt.
- Cookies: PHPSESSID and security=low observed. `HttpOnly`/`Secure` were not set in the observed headers (note this as a security recommendation).
- Session Fixation: NOT VULNERABLE in our tests.
- Logout Invalidation: PASS — sessions invalidated on logout.

## Recommendations (brief)
1. Regenerate session id on successful login (PHP: session_regenerate_id(true)).  
2. Ensure server sets cookies with `HttpOnly` and `Secure` (when using HTTPS), and `SameSite` as appropriate.  
3. Destroy server-side session on logout (session_unset(); session_destroy(); expire cookie).  
4. Continue to Week-2: test SQLi on `/vulnerabilities/sqli/` and XSS candidates from scans/reflection_test.html.

## Evidence files
See the `scans/` and `evidence/` folders under $BASE for raw outputs.
