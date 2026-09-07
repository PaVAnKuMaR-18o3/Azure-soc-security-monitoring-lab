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


---

# MITRE ATT&CK Mapping

The following mappings provide context for how the monitored Windows security events may relate to MITRE ATT&CK techniques. Some events do not directly represent an ATT&CK technique and require additional investigation and correlation before assigning a technique.

## Failed Login Activity

**Windows Event ID:** 4625

### Relevant MITRE ATT&CK Technique

- **T1110 – Brute Force**

### Detection Context

Multiple failed authentication attempts against the same account or from the same source may indicate password guessing or brute-force activity.

---

## Successful Login Activity

**Windows Event ID:** 4624

### Relevant MITRE ATT&CK Technique

- **T1078 – Valid Accounts**

### Detection Context

A successful login event alone does not indicate malicious activity. However, successful authentication following suspicious failed login activity or from an unusual source may require investigation for potential misuse of valid credentials.

---

## Process Creation Activity

**Windows Event ID:** 4688

### MITRE ATT&CK Context

Process creation events provide visibility into process execution and can support investigation of multiple MITRE ATT&CK techniques depending on the process, command line, parent process, and surrounding activity.

### Detection Context

Suspicious process execution should be investigated and mapped to the relevant ATT&CK technique based on the observed behavior.

---

## User Account Creation Activity

**Windows Event ID:** 4720

### Relevant MITRE ATT&CK Technique

- **T1136 – Create Account**

### Detection Context

Unexpected or unauthorized account creation may indicate an attacker attempting to establish or maintain access within an environment.

---

## Account Lockout Activity

**Windows Event ID:** 4740

### MITRE ATT&CK Context

Account lockout events do not directly map to a single MITRE ATT&CK technique. However, repeated lockouts may be correlated with failed authentication activity that could indicate:

- **T1110 – Brute Force**

### Detection Context

Repeated account lockouts should be correlated with Windows Event ID 4625 events to identify potential password guessing, brute-force attempts, or misconfigured credentials.

---

## MITRE ATT&CK Summary

| Detection | Windows Event ID | Relevant MITRE ATT&CK Technique |
|---|---:|---|
| Failed Login | 4625 | T1110 – Brute Force |
| Successful Login | 4624 | T1078 – Valid Accounts (context-dependent) |
| Process Creation | 4688 | Context-dependent |
| User Account Creation | 4720 | T1136 – Create Account |
| Account Lockout | 4740 | T1110 – Brute Force (when correlated with failed logins) |


---

# Dashboard and Visualization Documentation

## Monitoring Overview

The SOC monitoring lab uses Azure Log Analytics to investigate Windows security events collected from the monitored virtual machine. KQL queries are used to identify and review authentication activity, process creation, user account changes, and account lockout events.

The primary monitored Windows Security Event IDs are:

| Detection | Windows Event ID | Monitoring Purpose |
|---|---:|---|
| Successful Login | 4624 | Monitor successful authentication activity |
| Failed Login | 4625 | Detect failed authentication attempts |
| Process Creation | 4688 | Monitor process execution activity |
| User Account Creation | 4720 | Detect creation of new user accounts |
| Account Lockout | 4740 | Detect account lockout activity |

---

## Log Analytics Visualization Approach

KQL queries can be used as the foundation for SOC monitoring visualizations within Azure Monitor and Log Analytics.

Useful visualization views include:

- Authentication activity over time.
- Failed login trends.
- Successful logins following repeated authentication failures.
- Process creation activity by computer.
- New user account creation events.
- Account lockout activity.

These visualizations help analysts identify unusual patterns and investigate changes in security activity over time.

---

## Recommended SOC Monitoring Views

### Authentication Monitoring

Monitor successful and failed authentication activity to identify:

- Repeated failed login attempts.
- Authentication failures followed by successful logins.
- Multiple accounts targeted from the same source.
- Unusual login activity.

### Process Monitoring

Monitor process creation events to identify:

- Unexpected process execution.
- Repeated process activity.
- Unusual activity on monitored systems.
- Process execution associated with privileged accounts.

### Account Monitoring

Monitor account-related events to identify:

- New user account creation.
- Unexpected administrative activity.
- Repeated account lockouts.
- Multiple accounts affected within a short period.

---

## SOC Investigation Workflow

1. Review the detection alert or suspicious event.
2. Identify the affected user account and computer.
3. Review the event timestamp and related activity.
4. Investigate relevant fields such as source IP address, workstation, process details, or account information.
5. Correlate the event with related Windows Security Events.
6. Determine whether the activity is expected or suspicious.
7. Document findings and escalate suspicious activity when required.

---

## Future Improvements

Future versions of this lab could include:

- Azure Monitor Workbooks for interactive visualizations.
- Microsoft Sentinel analytics rules.
- Automated alert generation.
- Incident creation and investigation workflows.
- Additional Windows Security Event monitoring.
- Correlation rules across multiple event types.
