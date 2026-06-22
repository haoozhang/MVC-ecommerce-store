# Security Assessment Report

**Generated:** 2026-06-22T10:06:03.0000000Z

## Summary

| Metric | Count |
|--------|-------|
| Total Findings | 6 |
| CVE Vulnerabilities | 1 |
| CWE Vulnerabilities | 5 |
| Total Rules Assessed | 59 |
| Rules Passed | 54 |

### By Severity

| Severity | Count |
|----------|-------|
| mandatory | 1 |
| optional | 2 |
| potential | 3 |

### By Category

| Category | Count |
|----------|-------|
| CVE | 1 |
| Code Quality | 2 |
| Credentials & Secrets | 3 |

## CVE Findings (Dependency Vulnerabilities)

### CVE-2021-21252: Regular Expression Denial of Service in jquery-validation
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** EStore.WebUI/packages.config:7

[CVE-2021-21252](https://github.com/advisories/GHSA-jxwx-85vp-gvwm): Regular Expression Denial of Service in jquery-validation

Severity: HIGH

Affected dependencies:
  - jQuery.Validation:1.13.1 (declared at EStore.WebUI/packages.config:7)

Recommended fix:
  - Upgrade jQuery.Validation to 1.19.3 or later

## CWE Findings (Code-Level Vulnerabilities)

### CWE-259: Use of Hard-coded Password
- **Category:** Credentials & Secrets
- **Severity:** optional
- **Story Points:** 5
- **Files:** EStore.WebUI/Web.config:29, EStore.Domain/Concrete/EmailOrderProcessor.cs:91

Two hard-coded passwords found: (1) Web.config line 29 contains a plaintext admin password in the Forms Authentication credentials section with passwordFormat="Clear". (2) EmailOrderProcessor.cs line 91 in the EmailSettings class defines a hard-coded SMTP password.

---

### CWE-778: Insufficient Logging
- **Category:** Credentials & Secrets
- **Severity:** potential
- **Story Points:** 3
- **Files:** EStore.WebUI/Controllers/AccountController.cs

No logging framework (ILogger, NLog, log4net, Serilog, or System.Diagnostics) is present anywhere in the codebase. The AccountController.Login method handles authentication failures but does not log failed attempts, usernames, or remote IPs. Security-critical events such as failed logins, unauthorized access attempts, and admin operations are not recorded.

---

### CWE-798: Use of Hard-coded Credentials
- **Category:** Credentials & Secrets
- **Severity:** optional
- **Story Points:** 5
- **Files:** EStore.WebUI/Web.config:28, EStore.Domain/Concrete/EmailOrderProcessor.cs:88

Hard-coded credentials found in two locations: (1) Web.config lines 28-30 contain hard-coded admin credentials stored in clear text (passwordFormat="Clear"). (2) EmailOrderProcessor.cs EmailSettings class (lines 88-91) contains hard-coded SMTP username and password.

---

### CWE-772: Missing Release of Resource after Effective Lifetime
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** EStore.Domain/Concrete/EFProductRepository.cs

In EFProductRepository (line 13), an EFDbContext instance is created as a class field but EFProductRepository does not implement IDisposable and never calls context.Dispose(). EFDbContext extends DbContext which implements IDisposable, so the underlying database connection and resources are never explicitly released.

---

### CWE-775: Missing Release of File Descriptor or Handle after Effective Lifetime
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** EStore.Domain/Concrete/EmailOrderProcessor.cs

In EmailOrderProcessor.ProcessOrder (line 73), a MailMessage instance is created but never disposed. MailMessage implements IDisposable and holds internal stream handles. The MailMessage is used inside a using(smtpClient) block but is not itself wrapped in a using statement or explicitly disposed before the method returns.
