# wazuh-threat-hunting-lab19-dns-query-monitoring
## Objective

Detect suspicious DNS activity using Sysmon Event ID 22 and investigate DNS query events inside Wazuh Threat Hunting.

This lab simulates repetitive DNS lookups that resemble the early stages of DNS tunneling or command-and-control communication.

---

## Lab Environment

- Windows 11
- Wazuh Manager
- Wazuh Agent
- Sysmon
- PowerShell

---

## MITRE ATT&CK Mapping

| Tactic | Technique | ID |
|---------|-----------|----|
| Command and Control | Application Layer Protocol: DNS | T1071.004 |
| Command and Control | Data Obfuscation (Potential DNS Tunneling) | T1001 |

---

## Steps Performed

### Step 1 – Verify Required Services

Verified Sysmon and Wazuh Agent services were running.

Screenshot:
- Verify Services Running

---

### Step 2 – Generate Normal DNS Queries

Executed normal DNS lookups.

```powershell
Resolve-DnsName google.com
Resolve-DnsName microsoft.com
Resolve-DnsName github.com
```

Screenshot:
- Generate Normal DNS Activity

---

### Step 3 – Simulate Suspicious DNS Activity

Executed multiple DNS requests using PowerShell.

```powershell
1..20 | ForEach-Object {
    Resolve-DnsName "test$_.example.com" -ErrorAction SilentlyContinue
}
```

This generates repeated DNS queries that resemble suspicious DNS beaconing behavior.

Screenshot:
- Simulate Suspicious DNS Queries

---

### Step 4 – Verify Sysmon Logging

Confirmed Sysmon Event ID 22 events.

```powershell
Get-WinEvent `
-LogName "Microsoft-Windows-Sysmon/Operational" `
-FilterXPath "*[System[(EventID=22)]]" `
-MaxEvents 20
```

Screenshot:
- Verify DNS Queries Logging

---

### Step 5 – Investigate in Wazuh

Opened:

Threat Hunting

Filtered for:

```
data.win.system.eventID:22
```

Reviewed:

- DNS Query Name
- Process
- User
- Timestamp
- Image
- Query Status

Screenshot:
- Locate DNS Events in Wazuh Threat Hunting

---

### Step 6 – Review Event Details

Opened an individual event.

Verified:

- Event ID
- Process Image
- Query Name
- User
- Timestamp
- Sysmon Channel

Screenshot:
- View Event Details in Wazuh Threat Hunting

---

## Detection Workflow

PowerShell

↓

DNS Query Generated

↓

Sysmon Event ID 22

↓

Windows Event Log

↓

Wazuh Agent

↓

Wazuh Manager

↓

Threat Hunting Investigation

---

## Detection Opportunities

SOC analysts can identify:

- Excessive DNS requests
- Repeated failed lookups
- Beaconing activity
- Suspicious subdomains
- Potential DNS tunneling
- Malware communication
- Command-and-control traffic

---

## Skills Demonstrated

- Sysmon Event ID 22
- Windows DNS Monitoring
- Wazuh Threat Hunting
- PowerShell Simulation
- IOC Investigation
- DNS Analysis
- SOC Investigation Workflow
