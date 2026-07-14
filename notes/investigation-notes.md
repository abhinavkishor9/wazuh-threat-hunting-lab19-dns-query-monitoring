# Investigation Notes

## Incident Summary

A Windows endpoint generated multiple DNS queries through PowerShell.

Repeated DNS requests were observed within a short time interval and were successfully collected by Sysmon and forwarded to Wazuh for investigation.

---

## Timeline

### Initial Activity

Verified Sysmon and Wazuh Agent services.

---

### Simulation

Executed repeated DNS queries.

```powershell
Resolve-DnsName test1.example.com
Resolve-DnsName test2.example.com
...
Resolve-DnsName test20.example.com
```

---

### Detection

Sysmon generated Event ID 22 entries for each DNS lookup.

---

### Wazuh Investigation

Threat Hunting filter:

```
data.win.system.eventID:22
```

Observed:

- DNS query events
- Process information
- User account
- Event timestamp
- Query details

---

## Indicators Observed

- Event ID: 22
- Source: Microsoft-Windows-Sysmon
- Process: PowerShell
- Activity: DNS Query
- Multiple queries generated rapidly

---

## MITRE ATT&CK

### T1071.004

Application Layer Protocol: DNS

Reason:

DNS requests can be abused for command-and-control communication.

---

### T1001

Data Obfuscation

Reason:

DNS tunneling hides malicious traffic inside DNS requests.

---

## Investigation Outcome

The generated DNS requests were intentionally simulated.

No malicious payloads were downloaded.

No external communication beyond DNS resolution was observed.

This activity demonstrates how Wazuh can detect and investigate suspicious DNS behavior using Sysmon Event ID 22.

---

## Lessons Learned

- DNS telemetry is valuable for SOC investigations.
- High-frequency DNS requests may indicate beaconing.
- Sysmon Event ID 22 provides detailed DNS visibility.
- Wazuh effectively centralizes DNS telemetry for investigation.
