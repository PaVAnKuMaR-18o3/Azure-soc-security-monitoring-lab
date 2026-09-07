# SOC Detection Use Cases

This document describes the detection use cases implemented in the Azure SOC Security Monitoring Lab.

---

## 1. Successful Login Detection

**Windows Security Event ID:** 4624

### Description

Detects successful user authentication events on monitored Windows systems.

### Key Investigation Fields

- TimeGenerated
- Computer
- TargetUserName
- TargetDomainName
- IpAddress
- WorkstationName
- LogonType
- RenderedDescription

### Investigation Guidance

Investigate unusual successful logins by checking:

- Whether the user account is expected.
- Whether the source IP address is known.
- Whether the workstation is expected.
- Whether the logon type matches normal user behavior.
- Whether the login occurred at an unusual time.

---

## 2. Failed Login Detection

**Windows Security Event ID:** 4625

### Description

Detects failed authentication attempts on monitored Windows systems.

### Key Investigation Fields

- TimeGenerated
- Computer
- TargetUserName
- TargetDomainName
- IpAddress
- WorkstationName
- LogonType
- Status
- SubStatus
- RenderedDescription

### Investigation Guidance

Investigate:

- Repeated failed login attempts.
- Multiple failed attempts from the same IP address.
- Failed attempts against multiple user accounts.
- Unusual source IP addresses.
- Failure Status and SubStatus values.

---

## 3. Process Creation Detection

**Windows Security Event ID:** 4688

### Description

Detects process creation events on monitored Windows systems.

### Key Investigation Fields

- TimeGenerated
- Computer
- NewProcessName
- ParentProcessName
- CommandLine
- SubjectUserName
- RenderedDescription

### Investigation Guidance

Investigate:

- Unusual or suspicious processes.
- Unexpected parent-child process relationships.
- Suspicious command-line arguments.
- Processes launched by unexpected user accounts.

---

## 4. User Account Creation Detection

**Windows Security Event ID:** 4720

### Description

Detects the creation of new user accounts.

### Key Investigation Fields

- TimeGenerated
- Computer
- TargetUserName
- TargetDomainName
- SubjectUserName
- SubjectDomainName
- RenderedDescription

### Investigation Guidance

Investigate:

- Whether the account creation was authorized.
- Which user created the account.
- Whether the new account name appears suspicious.
- Whether the account was created outside normal administrative activity.

---

## 5. Account Lockout Detection

**Windows Security Event ID:** 4740

### Description

Detects user account lockout events.

### Key Investigation Fields

- TimeGenerated
- Computer
- TargetUserName
- TargetDomainName
- SubjectUserName
- SubjectDomainName
- CallerComputerName
- RenderedDescription

### Investigation Guidance

Investigate:

- Repeated account lockouts.
- The computer generating the lockout.
- Whether multiple accounts are being locked.
- Possible password guessing or brute-force activity.
- Whether the affected account belongs to a privileged user.

---

## Detection Summary

| Detection | Windows Event ID | Primary Purpose |
|---|---:|---|
| Successful Login | 4624 | Monitor successful authentication |
| Failed Login | 4625 | Detect failed authentication attempts |
| Process Creation | 4688 | Monitor process execution |
| User Account Creation | 4720 | Detect creation of user accounts |
| Account Lockout | 4740 | Detect account lockouts |
