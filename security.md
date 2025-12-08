Security Scanning Overview

This repository uses three layers of automated security scanning to identify vulnerabilities in the source code, dependencies, and the running application. All scans run through GitHub Actions.

1. SAST – Static Application Security Testing (CodeQL)

Purpose:

Analyzes source code to detect security flaws and unsafe coding patterns.

Trigger:

Runs automatically on every push and pull request to the main/default branch.

Checks for:

Use of insecure functions

Flask debug mode enabled

Data flow vulnerabilities

Code injection risks

Where to view results:

GitHub → Security → Code scanning alerts

Workflow file:

.github/workflows/codeql.yml

2. SCA – Software Composition Analysis (Dependency-Check)

Purpose:

Scans third-party libraries to identify known vulnerabilities (CVEs).

Trigger:

Runs on every push to the main branch.

Checks for:

Vulnerable dependencies

Outdated packages

Known CVEs from NVD, GitHub Advisories, and OSS Index

Important note:

GitHub does not automatically scan Python requirements.txt in a default setup. Reports may show 0 dependencies unless configured.

Artifacts generated:

sca-reports.zip (HTML, JSON, XML, SARIF)

Workflow file:

.github/workflows/dependency-check.yml

3. DAST – Dynamic Application Security Testing (OWASP ZAP)

Two types of OWASP ZAP scans are used.

3.1 ZAP Baseline Scan

Performs passive analysis

Does not actively attack the application

Detects missing headers and configuration issues

3.2 ZAP Full Scan

Performs active scanning and attack probes

Conducts spidering and fuzzing

Requires the application to be running in the CI environment

Checks for:

Missing security headers (CSP, X-Frame-Options, Permissions-Policy)

Runtime misconfigurations

Information leakage

Insecure endpoints

Input validation issues

Artifacts generated:

zap-baseline-reports.zip

zap-fullscan-reports.zip

Workflow files:

.github/workflows/zap-baseline.yml

.github/workflows/zap-fullscan.yml

3.Understanding Results


This section explains how to interpret the results produced by the three security tools used in this project: CodeQL (SAST), Dependency-Check (SCA), and OWASP ZAP (DAST).
Each tool focuses on a different security layer, so understanding their output is essential for accurate analysis.

Interpreting DAST Scan Results (OWASP ZAP)

OWASP ZAP performs dynamic testing by interacting with the running application. ZAP reports findings using several categories:

DAST Result Categories

PASS — The security check passed with no issues.
Example:
PASS: Directory Browsing [10033]

WARN-NEW — A new warning found in the current scan that needs attention.
Example:
WARN-NEW: Missing Anti-clickjacking Header [10020]

WARN-INPROG — A warning that already existed in previous scans and remains unresolved.

FAIL-NEW — A newly detected critical vulnerability (e.g., SQL injection, command execution).

FAIL-INPROG — A previously detected critical issue that is still present.

INFO — Informational findings that do not require action.

Common DAST Findings Explained

ZAP frequently identifies missing or misconfigured security headers, which affect how browsers protect the application.

Issue Code	Meaning	Why It Matters
10020	Missing X-Frame-Options	Enables clickjacking attacks
10021	X-Content-Type-Options missing	Allows MIME-sniffing attacks
10035	Missing HSTS header	HTTPS downgrade / MITM risk
10038	No Content Security Policy (CSP)	Higher risk of XSS
10049	Cacheable content	Sensitive data may be stored
10063	Permissions-Policy missing	Browser features could be abused
90004	Spectre vulnerability	Side-channel leakage
10036	Server version exposure	Helps attackers identify exploits
Functional Issues Detected by DAST
Issue Code	Meaning	Example
40018	SQL Injection	Database manipulation possible
40019	XSS	Script injection into pages
90005	Broken authentication	Weak login protection
90006	CORS misconfiguration	Unauthorised cross-origin access
 
 Example ZAP Scan Interpretation

Example DAST scan summary:

Total URLs scanned: 3
PASS: 61 checks passed
WARN-NEW: 9 warnings
FAIL-NEW: 0 failures
FAIL-INPROG: 0 issues

What This Means

61 PASS → Most checks passed

9 WARN-NEW → Issues requiring review

0 FAIL-NEW → No critical vulnerabilities

404 URLs → ZAP tests common paths (robots.txt, sitemap.xml), so 404 is normal

What To Do After a DAST Scan

Understand the risk — What attack is enabled?

Check affected URLs — Where is the issue located?

Assess severity — Prioritise FAIL → WARN → INFO

Plan the fix — Add headers, patch configuration, etc.

Re-scan — Confirm the issue is resolved.

Common questions

Q: Why do I see “Permission denied” in ZAP logs?
A: This comes from harmless file-write attempts. The scan still works.

Q: Why does my baseline scan change over time?
A: ZAP discovers more URLs as it learns the application.

Q: Do I fix INFO findings?
A: No. They are not vulnerabilities.

Q: Why does ZAP scan pages that return 404?
A: These are standard checks for common files (robots.txt, sitemap.xml).

Reporting Vulnerabilities

If you discover a security issue, contact:

s10266834@connect.np.edu.sg

Include:

Description of the issue

Steps to reproduce

Supporting evidence (logs, screenshots, etc.)