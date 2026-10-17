# Splunk Brute-Force Detection and Investigation Lab

## Overview
A hands-on SOC Analyst lab using Splunk to analyze authentication events, detect repeated failed login attempts, and investigate a successful login following multiple failures.

**Project type:** Blue Team / SIEM Detection  
**Tools:** Splunk Enterprise, SPL, Fedora Linux, Git, GitHub  
**Data:** Synthetic authentication logs created for lab testing

## Objectives
- Ingest authentication logs into Splunk.
- Detect repeated failed login attempts by username and source IP.
- Identify successful logins following repeated failures.
- Investigate suspicious authentication patterns.
- Present findings in a Splunk dashboard and incident report.

## Detection Use Cases

### 1. Repeated Failed Logins
```spl
source="auth_logs.csv" event="failed"
| stats count by username src_ip
| where count >= 5
```

This query counts failed authentication events by username and source IP, then returns combinations with at least five failures.

### 2. Successful Login After Repeated Failures
```spl
source="auth_logs.csv"
| sort 0 _time
| streamstats count(eval(event="failed")) as failures by username src_ip
| where event="success" AND failures >= 5
| table _time username src_ip event failures host
```

This query identifies successful login events that occur after at least five recorded failures for the same username and source IP in the event sequence.

## Lab Findings
- **Account:** `admin`
- **Source IP:** `203.0.113.50`
- **Failed attempts before success:** 5
- **Subsequent successful login:** Detected
- **Assessment:** Requires investigation; the simulated evidence alone does not confirm account compromise.

The IP address belongs to a range reserved for documentation and examples. The dataset is synthetic and does not represent a real attacker.

## Dashboard
The Splunk dashboard contains panels for repeated failed logins and successful logins following multiple failures.

Screenshot: [`screenshots/brute-force-dashboard.png`](screenshots/brute-force-dashboard.png)

## Repository Structure
```text
sample-logs/   - Synthetic authentication events
spl-queries/   - SPL detection and investigation queries
screenshots/   - Dashboard evidence
reports/       - Incident investigation report
README.md      - Project documentation
```

## Limitations
This is a local lab using synthetic CSV data, not production authentication telemetry. Results depend on the indexed data, event timestamps, and selected Splunk time range. The detections are investigative indicators, not proof of compromise.

## Future Improvements
- Test with Linux SSH authentication logs.
- Add alerting for repeated failures.
- Improve detection thresholds and time-window logic.
- Expand the dashboard with additional investigation panels.

## Disclaimer
Created for educational and defensive cybersecurity practice. All events are simulated.
