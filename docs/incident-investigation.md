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
```

Logon Type 3 represents a network logon.

The repeated network authentication attempts from the same external source IP were therefore investigated as potential unauthorized remote authentication activity.

---

## 6. Failed-to-Successful Authentication Correlation

A correlation investigation was performed between:

```text
Event ID 4625 — Failed Logon
```

and:

```text
Event ID 4624 — Successful Logon
```

The suspicious source IP investigated was:

```text
80.94.95.83
```

Successful Event ID 4624 logons were present in the environment; however, the successful authentication activity identified during the investigation was associated with different source activity, including the user account `Pavankumar`.

No confirmed successful Event ID 4624 authentication from source IP `80.94.95.83` was identified within the investigated telemetry and time range.

---

## 7. Analyst Assessment

The observed activity is consistent with a possible:

- Brute-force authentication attempt, or
- Password-spraying activity

Indicators supporting this assessment include:

- Thousands of failed authentication attempts
- A single suspicious source IP address
- Multiple targeted usernames
- Repeated Event ID 4625 events
- Network logon activity
- No confirmed successful authentication from the suspicious source

The available telemetry does not confirm that the authentication attempts resulted in a successful account compromise.

---

## 8. Severity Assessment

### Severity: Medium

The activity was assigned a Medium severity because:

- The authentication volume was unusually high.
- Multiple accounts were targeted.
- The activity originated from a suspicious external IP address.
- No successful authentication from the suspicious IP was confirmed.

The absence of a confirmed successful login reduces the immediate impact, but the volume and targeting pattern require investigation and continued monitoring.

---

## 9. Recommended Response

Recommended actions include:

1. Review and validate the source IP address `80.94.95.83`.
2. Block the source IP if it is confirmed to be malicious and blocking is appropriate for the environment.
3. Review activity associated with all targeted accounts.
4. Check for successful authentication attempts after the investigated time period.
5. Review account lockouts or authentication anomalies.
6. Continue monitoring for additional attempts from the same source IP.
7. Monitor for similar authentication patterns from other source IP addresses.

---

## 10. Final Conclusion

Repeated Windows failed authentication attempts were identified from source IP address `80.94.95.83` against multiple accounts on `SecurityLab-VM`.

The activity generated thousands of Event ID 4625 events and used Logon Type 3 network authentication. Multiple account names were targeted, which is consistent with brute-force or password-spraying behavior.

A correlation investigation was performed to identify whether the failed authentication activity was followed by a successful Event ID 4624 logon from the same suspicious source IP.

No confirmed successful authentication from `80.94.95.83` was identified within the investigated telemetry.

Based on the available evidence, the activity is assessed as suspicious authentication activity with no confirmed successful compromise.

---

## Evidence

The investigation was supported by the following Azure Log Analytics queries and screenshots:

- Failed authentication Event ID 4625 investigation
- Failed login attempts grouped by targeted account
- Source IP address analysis
- Logon type analysis
- Successful authentication Event ID 4624 investigation
- Failed-to-successful authentication correlation

Screenshots are available in the repository under:

```text
screenshots/
```
