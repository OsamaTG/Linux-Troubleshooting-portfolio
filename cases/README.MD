# Case Study: Disk Space Held by Deleted File

## Category
Storage / Filesystem

---

## Client Complaint
The client reported multiple issues across networks and services.  
Almost everything seemed affected, creating a complex troubleshooting scenario.

---

## Observed Symptoms
- Multiple services failing or behaving unpredictably  
- Disk space appearing completely full  
- Standard cleanup and log rotation had no effect  

---

## Initial Hypotheses
1. Large hidden files consuming disk space  
2. Deleted files still held open by running processes  
3. Filesystem corruption or misconfiguration  

---

## Investigation Steps

### 1. Check Disk Usage
```bash
df -h
```
- Confirmed the filesystem was at 100% usage.

### 2. Search for Large Files
```bash
du -sh /*
```
- No unusually large files were found to explain the full disk.

### 3. Identify Deleted but Open Files
```bash
lsof | grep deleted
```
**Result:**
```
<service>   <pid>  <user>  <fd>  REG  ...  <size>  <path-to-deleted-file> (deleted)
```
- Found a **very large deleted file** still held open by a running service.

---

## Resolution
- Restarted the affected service to release the file descriptor:
```bash
systemctl restart <affected-service>
```
- Disk space was immediately freed  
- All affected services returned to normal operation

---

## Verification
```bash
df -h
lsof | grep deleted
```
- Confirmed disk space reclaimed  
- No deleted-but-open files remained  
- Services returned to normal operation

---

## Tools Used
- `df`  
- `du`  
- `lsof`  
- `systemctl`  

---

## Outcome / Impact
- Services restored to normal  
- Disk space fully reclaimed  
- Prevented potential downtime and logging failures  

---

## Key Takeaways
> Deleted files can continue consuming disk space if held open by running processes.  
> Always check for deleted-but-open files using `lsof`.  
> Starting troubleshooting with **system resources** can help reach the root cause faster than immediately examining logs.
