# SOC Investigation: Repeated Failed Authentication Attempts

## 1. Investigation Summary

A high volume of failed Windows authentication attempts was identified on the monitored system. The activity originated from a single suspicious external source IP address and targeted multiple user accounts.

The investigation was performed using Windows Security Event logs collected in Azure Log Analytics.

---

## 2. Detection Details

| Field | Details |
|---|---|
| Detection Type | Repeated Failed Authentication Attempts |
| Windows Event ID | 4625 |
| Affected System | SecurityLab-VM |
| Suspicious Source IP | 80.94.95.83 |
| Logon Type | 3 |
| Status | 0xc000006d |
| SubStatus | 0xc0000064 |
| Failure Reason | %2313 |

The detection identified a high volume of failed authentication attempts originating from the same source IP address.

---

## 3. Affected Accounts

The suspicious source IP targeted multiple accounts, including:

- ADMIN
- USER
- BACKUP
- ADMINISTRADOR
- ADMINISTRATOR
- SYSTEM
- Test

This pattern indicates that the source was attempting authentication against multiple usernames rather than repeatedly targeting only a single account.

---

## 4. Failed Authentication Activity

The investigation identified thousands of failed Event ID 4625 authentication attempts.

Examples of failed attempts observed during the investigation included:

| Account | Failed Attempts |
|---|---:|
| ADMIN | 4,168 |
| USER | 4,167 |
| BACKUP | 4,167 |
| ADMINISTRADOR | 4,167 |
| ADMINISTRATOR | 4,167 |
| SYSTEM | 4,166 |

The failed authentication activity was grouped by account and source IP address to identify the accounts being targeted.

---

## 5. Logon Type Analysis

The observed failed authentication attempts used:

```text
Logon Type: 3
