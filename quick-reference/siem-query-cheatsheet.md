# SIEM Query Cheatsheet

Quick-reference for the most common SOC investigation queries. Syntax shown is Splunk SPL — adapt field names to your SIEM.

---

## Authentication & Account Activity

### Failed logins — single user
```
index=windows EventCode=4625 user=USERNAME
| stats count by src_ip, dest, _time
| where count > 5
```

### Brute force — many failed logins across users
```
index=windows EventCode=4625 earliest=-1h
| stats dc(user) as unique_users, count by src_ip
| where unique_users > 10
```

### Successful login after multiple failures (spray success)
```
index=windows (EventCode=4625 OR EventCode=4624)
| transaction user maxspan=10m
| where EventCode=4624 AND eventcount > 5
```

### Account created outside business hours
```
index=windows EventCode=4720
| eval hour=strftime(_time, "%H")
| where hour < 8 OR hour > 18
```

### Privileged group membership change
```
index=windows EventCode IN (4728, 4732, 4756)
| table _time, user, src_user, group_name, host
```

---

## Process Execution

### PowerShell execution with encoded command
```
index=windows EventCode=4688 process=powershell*
  (CommandLine="*-enc*" OR CommandLine="*-EncodedCommand*")
| table _time, host, user, CommandLine
```

### Suspicious child process from Office app
```
index=edr parent_process IN (winword.exe, excel.exe, powerpnt.exe)
  child_process IN (cmd.exe, powershell.exe, wscript.exe, mshta.exe)
| table _time, host, user, parent_process, child_process, command_line
```

### Living-off-the-land binaries (LOLBins)
```
index=edr process IN (certutil.exe, mshta.exe, rundll32.exe, regsvr32.exe,
  wmic.exe, cmstp.exe, msiexec.exe) earliest=-24h
| stats count by host, user, process, command_line
| sort -count
```

### Process creating a new scheduled task
```
index=windows EventCode=4698
| rex field=TaskContent "<Command>(?P<cmd>[^<]+)"
| table _time, host, user, TaskName, cmd
```

---

## Network & Lateral Movement

### Pass-the-Hash indicator (NTLM lateral movement)
```
index=windows EventCode=4624 LogonType=3 AuthenticationPackageName=NTLM
| stats count dc(dest) as targets by src_ip, user
| where targets > 3
```

### Internal port scan (many unique dest ports)
```
index=network src_zone=internal earliest=-1h
| stats dc(dest_port) as ports by src_ip
| where ports > 50
```

### Beaconing — regular outbound connections
```
index=proxy earliest=-6h
| bucket _time span=1m
| stats count by src_ip, dest_host, _time
| eventstats stdev(count) as sd, avg(count) as avg by src_ip, dest_host
| where sd < 2 AND avg > 0
```

### DNS requests to newly registered domains
```
index=dns earliest=-24h
| lookup domain_age_lookup domain AS query OUTPUT age_days
| where age_days < 30
| stats count by query, src_ip
```

---

## Data Exfiltration

### Large outbound transfer
```
index=network direction=outbound earliest=-24h
| stats sum(bytes_out) as total_bytes by src_ip, dest_ip
| where total_bytes > 104857600
| eval total_mb = round(total_bytes/1024/1024,2)
```

### Compressed file created then sent
```
index=edr (file_name="*.zip" OR file_name="*.rar" OR file_name="*.7z")
  event_type=file_create earliest=-2h
| join type=inner src_ip [search index=proxy earliest=-2h]
```

---

## Defence Evasion

### Windows event log cleared
```
index=windows EventCode IN (1102, 104)
| table _time, host, user, Message
```

### Security tool disabled (service stopped)
```
index=windows EventCode=7036
  (Message="*Windows Defender*" OR Message="*Carbon Black*" OR Message="*CrowdStrike*")
  Message="*stopped*"
| table _time, host, Message
```

### Timestomping (file modification time changed)
```
index=edr event_type=file_modify
| eval created_epoch=strptime(file_created, "%Y-%m-%dT%H:%M:%S")
| eval modified_epoch=strptime(file_modified, "%Y-%m-%dT%H:%M:%S")
| where modified_epoch < created_epoch
```

---

## Ransomware Indicators

### Mass file rename (encryption in progress)
```
index=edr event_type=file_rename earliest=-30m
| stats count by host, user
| where count > 100
```

### Shadow copy deletion
```
index=windows EventCode=4688
  CommandLine="*vssadmin*delete*shadows*"
  OR CommandLine="*wmic*shadowcopy*delete*"
| table _time, host, user, CommandLine
```

---

## General Utilities

### Top talkers (busiest source IPs)
```
index=network earliest=-1h
| stats sum(bytes) as vol by src_ip
| sort -vol | head 20
```

### Timeline for a specific host
```
index=* host=HOSTNAME earliest=-4h latest=now
| sort _time
| table _time, source, EventCode, user, process, CommandLine
```

### Correlate user across sources
```
index=* user=USERNAME earliest=-24h
| stats count by index, source, EventCode
| sort -count
```

---

> **Reminder:** Defang IOCs in tickets and reports using [.] notation (e.g., `evil[.]com`) to prevent accidental clicks.
