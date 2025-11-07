# DVWA Deployment & HTTP Recon — Report

Target: 192.168.211.129:8080
Date: $(date -I)

## Purpose
Deploy DVWA and run HTTP reconnaissance and session tests.

## Summary of Findings
- Session Fixation: NOT VULNERABLE (attacker-supplied PHPSESSID did not become authenticated).
- Logout invalidation: OK (pre-logout cookie did not grant access post-logout).
- See `scans/` and `evidence/` for raw output files.

## Recommendations
- Regenerate session ID at login: `session_regenerate_id(true)`.
- Destroy session on logout: `session_unset(); session_destroy();` and expire cookie.
- Add cookie flags: HttpOnly, Secure, SameSite.
