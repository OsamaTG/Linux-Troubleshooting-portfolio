# Disk Space Full Despite File Deletion

## Category
Storage / Filesystem

---

## Problem
The filesystem reported 100% disk usage even after deleting large files, which caused multiple services to fail due to lack of available disk space.

---

## Symptoms
- `df -h` shows the filesystem at 100% usage
- `du -sh` does not report large files
- Deleting files does not reclaim disk space
- Services fail to write logs or temporary files

---

## Initial Hypotheses
- Hidden large files consuming disk space
- Deleted files still held open by running processes
- Filesystem inconsistency or corruption

---

## Investigation
```bash
df -h
du -sh /var/log
lsof | grep deleted
