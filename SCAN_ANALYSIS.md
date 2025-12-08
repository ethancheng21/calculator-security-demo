# Security Scan Analysis - [2/12/2025]

## DAST Results (ZAP Baseline)
- Total URLs scanned: 10
- PASS: 140 passed security checks  
- WARN-NEW: 7 new warnings  
- FAIL-NEW: 0 critical failures

### Warning Summary
- Medium risk:
10038,Content Security Policy (CSP) Header Not Set(5 instances)
URLs include:  
  - /  
  - /robots.txt  
  - /sitemap.xml  
  - /favicon.ico  
10106,HTTP Only Site(1 instance)
URL:  
  - http://localhost:5000

10020,Missing Anti-clickjacking Header(2 instance)
URLs include:  
  - http://localhost:5000/  
  - http://localhost:5000  
- Low risk: 
90004,Insufficient Site Isolation Against Spectre Vulnerability(6 instances)
URLs include:  
  - http://localhost:5000/  
10063,Permissions Policy Header Not Set(5 instances)
URLs include:  
  - /  
  - /robots.txt  
  - /sitemap.xml  
  - /favicon.ico  
10036,Server Leaks Version Information via "Server" HTTP Response Header Field(5 instances)
URLs include:  
  - http://localhost:5000/  
  - /robots.txt  
  - /sitemap.xml  
  - /favicon.ico  
10021,X-Content-Type-Options Header Missing(2 instances)
 URLs include:  
  - http://localhost:5000/  
  - http://localhost:5000  

## SCA Results (Dependency Check)
dependency-check version: 12.1.9
Report Generated On: Tue, 2 Dec 2025 07:24:19 GMT
Dependencies Scanned: 0 (0 unique)
Vulnerable Dependencies: 0
Vulnerabilities Found: 0
Vulnerabilities Suppressed: 0


### Vulnerable Packages
No vulnerable packages were detected.  
Dependency-Check scanned 0 dependencies and found **0 vulnerabilities** (0 CVEs).

## SAST Results (CodeQL)
- Total issues: 1
- By type: [breakdown]

## Recommendations
[Based on findings, what should be prioritized?]