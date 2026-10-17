# Incident Report: Repeated Failed Logins

## 1. Summary
A simulated authentication dataset was analyzed in Splunk to identify repeated failed login attempts and check for a subsequent successful login.

## 2. Evidence
- Username: `admin`
- Source IP: `203.0.113.50`
- Failed login attempts: 5
- Subsequent successful login: 1
- Host field in search results: `Aster`
- Data source: `auth_logs.csv`

## 3. Detection
A Splunk SPL query grouped failed login events by username and source IP, then identified combinations with at least five failures.

A second query identified a successful login after five or more recorded failures for the same username and source IP.

## 4. Assessment
**Priority: Medium — requires investigation.**

A successful login following repeated failures may indicate a brute-force attempt or legitimate user error. The simulated data alone does not prove account compromise.

## 5. Recommended Actions
1. Verify whether the successful login was authorized.
2. Review surrounding authentication events and any activity after login.
3. Check the account's normal login patterns.
4. Escalate if additional evidence indicates unauthorized access.

## 6. Limitations
This investigation used synthetic CSV data in a local Splunk lab. The results do not represent a confirmed real-world security incident.
