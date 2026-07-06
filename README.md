# 🦠 Simulated Ransomware Attack Analysis

The following scenario is based on the captured ransomware execution logs. The malware was observed scanning directories, spawning encryption threads, and attempting to access files before encryption.

---

## 📄 Observed Execution Logs

The captured logs show the ransomware initializing its encryption engine and beginning file enumeration.

### Sample Log Entries

```text
[file_logger] [info] Number of thread to folder parsers = 2
[file_logger] [info] Number of thread to root folder parsers = 1
[file_logger] [info] Number of threads to encrypt = 5

[file_logger] [error] File handle not found!
(C:\Program Files\7-Zip\7-zip.chm)

[file_logger] [error] File handle not found!
(C:\Program Files\7-Zip\Lang\en.txt)

[file_logger] [error] File handle not found!
(C:\Program Files (x86)\Bonjour\Bonjour.Resources\About Bonjour.rtf)
```

---

# 🔍 Analysis of the Logs

The logs indicate the ransomware has already reached the encryption stage.

### Thread Initialization

The malware creates multiple worker threads to speed up encryption.

```
Folder Parser Threads : 2
Root Parser Threads   : 1
Encryption Threads    : 5
```

This indicates a multithreaded ransomware architecture designed to encrypt multiple files simultaneously.

---

### File Enumeration

The malware scans installed applications including:

- 7-Zip
- Apple Bonjour
- Language resource files
- Documentation files

Examples include:

```
C:\Program Files\7-Zip\
```

```
C:\Program Files (x86)\Bonjour\
```

This behavior suggests automated directory traversal before encryption.

---

### File Handle Errors

Numerous entries show:

```
File handle not found
```

Possible reasons include:

- File does not exist
- File is currently locked
- Access denied
- Unsupported file type
- Software already removed

Although these files could not be opened, the ransomware continued scanning additional directories.

---

# 📊 MITRE ATT&CK Mapping

| Attack Phase | MITRE Technique | Evidence from Logs | Response |
|--------------|----------------|--------------------|----------|
| Discovery | T1083 - File and Directory Discovery | Malware scans Program Files directories | Monitor abnormal directory enumeration |
| Execution | T1106 - Native API | Multiple encryption threads initialized | Terminate ransomware process immediately |
| Collection | T1005 - Data from Local System | Attempts to access numerous local files | Isolate endpoint |
| Impact | T1486 - Data Encrypted for Impact | Encryption worker threads detected | Restore from offline backup |

---

# 🚨 Indicators of Compromise (IoCs)

The following indicators were observed during execution:

- Multiple encryption threads created
- Automated directory traversal
- Repeated file access attempts
- High-speed filesystem enumeration
- Access attempts within Program Files
- Continuous file handle errors

---

# 🛡️ Recommended Incident Response

## Step 1 - Isolate the Host

Immediately disconnect the affected computer from:

- Ethernet
- Wi-Fi
- VPN

This prevents ransomware from spreading laterally.

---

## Step 2 - Preserve Evidence

Do **not** restart the computer.

Collect:

- Memory dump (if possible)
- Windows Event Logs
- Wazuh alerts
- Running processes
- Disk image

---

## Step 3 - Stop Malicious Processes

Terminate ransomware processes using an EDR solution or Task Manager before additional files are encrypted.

---

## Step 4 - Recover Systems

- Reimage affected endpoints.
- Restore verified offline backups.
- Scan restored systems before reconnecting them to the production network.

---

# 📌 Key Findings

✅ Multi-threaded ransomware execution detected.

✅ Automated filesystem scanning observed.

✅ Multiple file access attempts against installed software directories.

✅ Evidence supports active file enumeration prior to encryption.

✅ Immediate host isolation would significantly reduce further damage.
