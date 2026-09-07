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


---

# Alert Logic and Investigation Thresholds

The following thresholds provide investigation guidance for the SOC detections implemented in this lab. These thresholds are intended to help prioritize suspicious activity and should be adjusted based on the environment.

## Failed Login Activity

**Detection:** Windows Event ID 4625

### Investigation Threshold

Investigate when multiple failed login attempts are observed for the same user account or from the same source within a short period of time.

### Potential Indicators

- Repeated failed authentication attempts
- Multiple failures against the same account
- Failures from an unusual IP address
- Failures from an unfamiliar workstation
- A successful login occurring shortly after multiple failed attempts

### Analyst Action

Review the affected account, source IP address, workstation, logon type, failure status, and substatus to determine whether the activity may indicate password guessing or unauthorized access attempts.

---

## Successful Login Activity

**Detection:** Windows Event ID 4624

### Investigation Threshold

Investigate successful logins that occur after multiple failed authentication attempts or originate from unusual sources.

### Potential Indicators

- Successful login following repeated failed logins
- Unusual source IP address
- Unfamiliar workstation
- Unexpected account activity
- Suspicious logon type

### Analyst Action

Review the authentication history for the account and correlate successful login activity with previous failed authentication events.

---

## Process Creation Activity

**Detection:** Windows Event ID 4688

### Investigation Threshold

Investigate process creation events involving unusual, unexpected, or suspicious process activity.

### Potential Indicators

- Unexpected processes
- Unusual process execution patterns
- Processes associated with suspicious activity
- Process activity occurring during investigation of another alert

### Analyst Action

Review the process creation event details and correlate the activity with other security events from the same computer or user.

---

## User Account Creation Activity

**Detection:** Windows Event ID 4720

### Investigation Threshold

Investigate newly created user accounts unless the account creation is known and authorized.

### Potential Indicators

- Unexpected account creation
- Unknown user accounts
- Account creation outside normal administrative activity
- Multiple accounts created within a short period

### Analyst Action

Verify whether the account creation was authorized and review the account and system involved.

---

## Account Lockout Activity

**Detection:** Windows Event ID 4740

### Investigation Threshold

Investigate account lockouts, especially when repeated lockouts occur for the same account.

### Potential Indicators

- Repeated account lockouts
- Multiple accounts being locked
- Lockouts associated with repeated failed logins
- Unexpected caller computer

### Analyst Action

Review the affected account, caller computer, and related failed login events to identify the cause of the lockout.

---
