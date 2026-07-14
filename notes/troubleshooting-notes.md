# Troubleshooting Notes

## Issue 1

### No Sysmon Event ID 22 generated

Cause

Sysmon DNS Query logging not enabled.

Resolution

Verify Sysmon configuration includes:

```
<DnsQuery onmatch="include"/>
```

Restart Sysmon after updating the configuration.

---

## Issue 2

### No DNS queries in Wazuh

Cause

Wazuh Agent not forwarding Sysmon Operational logs.

Resolution

Verify:

- Wazuh Agent service running
- Sysmon Operational log collection enabled
- Agent connected to Wazuh Manager

---

## Issue 3

### Resolve-DnsName returns errors

Cause

Non-existent domains generate DNS failures.

Resolution

This is expected.

Sysmon records both successful and failed DNS queries.

---

## Issue 4

### No results in Threat Hunting

Cause

Incorrect search filter or time range.

Resolution

Use:

```
data.win.system.eventID:22
```

Expand the time range to:

Last 15 Minutes

or

Last 1 Hour

---

## Issue 5

### Too few DNS events

Cause

Only a small number of queries executed.

Resolution

Generate additional queries.

```powershell
1..20 | ForEach-Object {
    Resolve-DnsName "test$_.example.com" -ErrorAction SilentlyContinue
}
```

---

## Issue 6

### DNS events delayed

Cause

Normal Wazuh ingestion delay.

Resolution

Wait 10–30 seconds and refresh Threat Hunting.

---

## Validation Checklist

✓ Sysmon service running

✓ Wazuh Agent running

✓ DNS Query events generated

✓ Sysmon Event ID 22 present

✓ Wazuh Threat Hunting displays Event ID 22

✓ Event details include DNS query information

✓ Investigation successfully completed
