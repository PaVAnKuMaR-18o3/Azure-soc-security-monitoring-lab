# Azure SOC Security Monitoring Lab

## Overview

This project demonstrates a hands-on Windows security monitoring lab built using Microsoft Azure.

The lab focuses on collecting, investigating, and monitoring Windows security events using:

- Microsoft Azure
- Azure Monitor
- Log Analytics Workspace
- Microsoft Sentinel
- KQL (Kusto Query Language)
- Azure Monitor Workbooks

The objective of this project is to simulate common SOC analyst activities such as security monitoring, log analysis, authentication investigation, account monitoring, and process creation investigation.

---

## Lab Architecture

Windows Virtual Machine
        ↓
Windows Security Events
        ↓
Azure Monitor Agent
        ↓
Log Analytics Workspace
        ↓
Microsoft Sentinel
        ↓
KQL Queries and Security Investigation
        ↓
Azure Monitor Workbook Dashboard

---

## Security Events Monitored

This lab monitors Windows security events including:

| Event ID | Security Event | Purpose |
|---|---|---|
| 4624 | Successful Logon | Monitor successful authentication activity |
| 4625 | Failed Logon | Investigate failed authentication attempts |
| 4688 | Process Creation | Monitor process execution activity |
| 4740 | Account Lockout | Detect locked user accounts |
| 4720 | User Account Created | Monitor new user account creation |

---

## KQL Detection Queries

The repository contains KQL queries used to investigate Windows security events.

### Successful Logins

File:

`queries/successful-logins.kql`

Monitors successful Windows logon events using Event ID 4624.

### Failed Logins

File:

`queries/failed-logins.kql`

Monitors failed authentication attempts using Event ID 4625.

### Process Creation

File:

`queries/process-creation.kql`

Monitors process creation events using Event ID 4688.

### Account Lockouts

File:

`queries/account-lockouts.kql`

Monitors account lockout events using Event ID 4740.

### User Account Creation

File:

`queries/account-created.kql`

Monitors new user account creation events using Event ID 4720.

---

## SOC Investigation Fields

During investigations, the following fields are analyzed where available:

- TimeGenerated
- Computer
- TargetUserName
- SubjectUserName
- IpAddress
- WorkstationName
- LogonType
- FailureReason
- Status
- SubStatus
- NewProcessName
- ParentProcessName

---

## Skills Demonstrated

- Security Monitoring
- Alert and Event Triage
- Windows Event Log Analysis
- Authentication Investigation
- KQL Query Development
- Microsoft Sentinel
- Azure Monitor
- Log Analytics
- Process Monitoring
- Account Activity Monitoring
- Security Dashboard Development

---

## Project Structure

```text
Azure-soc-security-monitoring-lab/
│
├── queries/
│   ├── successful-logins.kql
│   ├── failed-logins.kql
│   ├── process-creation.kql
│   ├── account-lockouts.kql
│   └── account-created.kql
│
└── README.md
