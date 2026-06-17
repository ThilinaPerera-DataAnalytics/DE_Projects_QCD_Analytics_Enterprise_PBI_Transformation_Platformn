# Oracle APEX Local Development Stack
## Complete Setup Guide — Windows 10
### From Zero to Working in One Document

---

> **Who this is for:** </br>
Anyone starting from scratch with no prior knowledge of Docker, Oracle, or APEX.</br>
> **What you will have at the end:**</br>
A fully working Oracle Database + APEX web form builder running on your personal laptop, accessible from your browser.</br>
> **Time required:**</br>
2–3 hours (most of it waiting for downloads and installs).

---

![setup](/oracle_setup.png)

## Section 0 — What Are We Building and Why?

Before touching anything, understand what each piece does:

| Technology | Plain English Explanation |
|---|---|
| **Oracle Database Free** | The database — where all your data lives. Think of it as the filing cabinet. |
| **ORDS** (Oracle REST Data Services) | The web gateway — connects your browser to the database. Without it, you can't open APEX in a browser. |
| **APEX** (Oracle Application Express) | The form builder — a web app that runs inside Oracle DB. You use it to build forms, reports, and data entry screens. |
| **Docker** | A container system — runs Oracle and ORDS inside isolated mini-computers on your laptop. No messy installation directly on Windows. |
| **Docker Desktop** | The Windows app that manages Docker containers. |
| **VS Code** | Code editor — used to write and run SQL queries against your database. |

### How Everything Connects

```
Your Browser
     |
     | http://localhost:8181
     |
  ORDS Container (port 8181 → 8080 inside)
     |
     | port 1521
     |
  Oracle DB Container (port 1521)
     |
  APEX (lives inside Oracle DB)

VS Code → connects directly to Oracle DB on port 1521
```

### What the End Result Looks Like

- Open browser → go to `http://localhost:8181/ords/apex` → APEX login page
- Login → build web forms that write data to Oracle DB
- Open VS Code → run SQL queries directly against the same DB
- Power BI → connects to Oracle DB via port 1521

---

## Section 1 — System Requirements Check

> Run all checks BEFORE installing anything. Open PowerShell as Administrator for every command in this guide.

### How to Open PowerShell as Administrator

1. Click the Windows Start button
2. Type `PowerShell`
3. Right-click `Windows PowerShell`
4. Click `Run as administrator`
5. Click `Yes` on the User Account Control prompt

> **IMPORTANT:** Always open PowerShell as Administrator. Regular PowerShell will cause permission errors.

### Check 1 — Windows Version

```powershell
Get-ComputerInfo | Select-Object WindowsVersion, OsBuildNumber, OsArchitecture
```

Required: Build number **19041 or higher** (Windows 10 version 2004 or later).

### Check 2 — Virtualization

```powershell
(Get-WmiObject Win32_ComputerSystem).HypervisorPresent
```

Must return `True`. If it returns `False`, you need to enable virtualization in your laptop BIOS before proceeding.

### Check 3 — Available RAM and Disk

```powershell
# RAM
(Get-CimInstance Win32_PhysicalMemory | Measure-Object -Property capacity -Sum).sum / 1GB

# Free disk space on D drive
(Get-PSDrive D).Free / 1GB
```

Minimum required: **8 GB RAM**, **20 GB free disk space** on D drive.

---

## Section 2 — One-Time Software Installation

### 2.1 — Docker Desktop 4.28.0

> **CRITICAL: Do NOT install the latest Docker Desktop version.** Version 4.76+ has a known bug on Windows 10 that prevents the engine from starting. Use version 4.28.0 specifically.

**Download:**
```
https://desktop.docker.com/win/main/amd64/139021/Docker%20Desktop%20Installer.exe
```

Or download via PowerShell:

```powershell
# Create project folder first
mkdir D:\oracle-local
cd D:\oracle-local

# Download Docker Desktop 4.28.0
Invoke-WebRequest -Uri "https://desktop.docker.com/win/main/amd64/139021/Docker%20Desktop%20Installer.exe" -OutFile "D:\oracle-local\DockerDesktopInstaller-4.28.0.exe"
```

**Install with Hyper-V backend (critical flag):**

```powershell
Start-Process "D:\oracle-local\DockerDesktopInstaller-4.28.0.exe" -ArgumentList "install --backend=hyper-v" -Wait
```

**After install completes — before launching Docker — edit the config file:**

```powershell
# Create the config with WSL2 disabled
$config = @{
    AutoStart = $false
    DisplayedOnboarding = $false
    LicenseTermsVersion = 2
    SettingsVersion = 44
    UseWSL2 = $false
} | ConvertTo-Json

$config | Set-Content "$env:APPDATA\Docker\settings-store.json"
```

**Install WSL2 Ubuntu (required by Docker even with Hyper-V backend):**

```powershell
wsl --install -d Ubuntu
```

This downloads Ubuntu (~500 MB). When prompted, create a Linux username and password — keep it simple. After it completes, restart your laptop.

**After restart — add firewall rules for Docker and Oracle:**

```powershell
# Open PowerShell as Administrator, then run:

New-NetFirewallRule -DisplayName "Docker Daemon" -Direction Inbound -Action Allow -Program "C:\Program Files\Docker\Docker\resources\dockerd.exe"
New-NetFirewallRule -DisplayName "Docker Desktop" -Direction Inbound -Action Allow -Program "C:\Program Files\Docker\Docker\Docker Desktop.exe"
New-NetFirewallRule -DisplayName "Docker Backend" -Direction Inbound -Action Allow -Program "C:\Program Files\Docker\Docker\resources\com.docker.backend.exe"
New-NetFirewallRule -DisplayName "Docker Port 2375" -Direction Inbound -Action Allow -Protocol TCP -LocalPort 2375
New-NetFirewallRule -DisplayName "Docker Port 2376" -Direction Inbound -Action Allow -Protocol TCP -LocalPort 2376
New-NetFirewallRule -DisplayName "Oracle DB 1521" -Direction Inbound -Action Allow -Protocol TCP -LocalPort 1521
New-NetFirewallRule -DisplayName "ORDS 8181" -Direction Inbound -Action Allow -Protocol TCP -LocalPort 8181
```

**Launch Docker Desktop:**

```powershell
Start-Process "C:\Program Files\Docker\Docker\Docker Desktop.exe"
```

Wait 2–3 minutes for the whale icon in the system tray (bottom right) to stop animating. Then verify:

```powershell
docker ps
```

Expected output — empty table with no errors:
```
CONTAINER ID   IMAGE   COMMAND   CREATED   STATUS   PORTS   NAMES
```

If you get an error, wait another minute and try again.

---

### 2.2 — VS Code

**Download:** `https://code.visualstudio.com/download`

Install normally — all default options are fine.

**Install the Oracle SQL extension:**

```powershell
code --install-extension Oracle.oracledevtools
```

Or search `Oracle SQL Developer Extension for VS Code` in the VS Code Extensions panel (published by Oracle).

---

### 2.3 — Oracle Container Registry Account

You need a free Oracle account to pull the Docker images.

**Step 1 — Create account:**
Go to: `https://profile.oracle.com/myprofile/account/create-account.jspx`

Fill in your name, email, password, and country (Qatar). Verify your email when the confirmation arrives.

**Step 2 — Accept licenses:**

1. Go to `https://container-registry.oracle.com`
2. Sign in with your new account
3. Search for `database`
4. Click the **Database** category
5. Click **free** in the left panel
6. Click **Continue** to accept the license
7. Go back, find **ords** in the left panel
8. Click **Continue** to accept that license too

Both must be accepted or the image pulls will fail with a 403 error.

**Step 3 — Generate Auth Token:**

1. Click your name/email at the top right of the registry website
2. Click **Auth Token**
3. Click **Generate Auth Token**
4. Copy the token immediately — it is shown only once
5. Save it somewhere temporarily (Notepad is fine)

**Step 4 — Login from PowerShell:**

```powershell
docker login container-registry.oracle.com
```

When prompted:
- Username: your Oracle account email
- Password: paste the Auth Token (NOT your account password)

Should return: `Login Succeeded`

---

## Section 3 — Project Folder Structure

Create the complete folder structure:

```powershell
mkdir D:\oracle-local
mkdir D:\oracle-local\sql
mkdir D:\oracle-local\sql\schema
mkdir D:\oracle-local\sql\seed
mkdir D:\oracle-local\sql\queries
```

Final structure:

```
D:\oracle-local\
├── docker-compose.yml       ← Stack configuration (created in Section 4)
├── settings.xml             ← ORDS static files config (created in Section 4)
├── apex\                    ← APEX installer files (downloaded in Section 6)
│   └── images\              ← APEX static files (used in Section 8)
└── sql\
    ├── schema\              ← Your table creation scripts
    ├── seed\                ← Sample data scripts
    └── queries\             ← Ad-hoc queries
```

---

## Section 4 — Create Configuration Files

### 4.1 — docker-compose.yml

Open Notepad:

```powershell
notepad D:\oracle-local\docker-compose.yml
```

Paste this exact content and save:

```yaml
version: '3.8'

services:

  oracle-db:
    image: container-registry.oracle.com/database/free:latest
    container_name: oracle-free
    ports:
      - "1521:1521"
    environment:
      - ORACLE_PWD=Oracle_123
    volumes:
      - oracle-data:/opt/oracle/oradata
    healthcheck:
      test: ["CMD-SHELL", "echo 'SELECT 1 FROM DUAL;' | sqlplus -s system/Oracle_123@localhost:1521/FREE || exit 1"]
      interval: 30s
      timeout: 10s
      retries: 15
      start_period: 180s

  ords:
    image: container-registry.oracle.com/database/ords:latest
    container_name: ords-free
    ports:
      - "8181:8080"
    environment:
      - DBHOST=oracle-db
      - DBPORT=1521
      - DBSERVICENAME=FREEPDB1
      - ORACLE_PWD=Oracle_123
    volumes:
      - ords-config:/etc/ords/config
    depends_on:
      oracle-db:
        condition: service_healthy

volumes:
  oracle-data:
  ords-config:
```

Save with `Ctrl+S` and close Notepad.

### 4.2 — settings.xml

Open Notepad:

```powershell
notepad D:\oracle-local\settings.xml
```

Paste this exact content and save:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE properties SYSTEM "http://java.sun.com/dtd/properties.dtd">
<properties>
<comment>ORDS Configuration</comment>
<entry key="database.api.enabled">true</entry>
<entry key="mongo.enabled">true</entry>
<entry key="standalone.access.log">/tmp/ords_access_logs/</entry>
<entry key="standalone.doc.root">/etc/ords/config/global/doc_root</entry>
<entry key="standalone.apex.doc.root">/etc/ords/config/global/doc_root</entry>
</properties>
```

Save with `Ctrl+S` and close Notepad.

---

## Section 5 — Pull Docker Images

> Docker Desktop must be running before these commands. Check the system tray whale icon is static (not animating).

```powershell
# Pull Oracle Database Free (~3 GB)
docker pull container-registry.oracle.com/database/free:latest

# Pull ORDS (~1 GB)
docker pull container-registry.oracle.com/database/ords:latest
```

Each pull will show progress bars. Wait for both to complete.

Verify both images downloaded:

```powershell
docker images
```

You should see both images listed.

---

## Section 6 — Download and Prepare APEX Installer

**Download APEX 24.1:**

Go to: `https://www.oracle.com/tools/downloads/apex-241-downloads/`

Download: **Oracle APEX 24.1 - English language only** (~400 MB zip file)

**Extract the zip:**

Extract the zip file to: `D:\oracle-local\`

After extraction you should have: `D:\oracle-local\apex\` folder containing many `.sql` files and an `images` subfolder.

Verify:

```powershell
Test-Path "D:\oracle-local\apex\apexins.sql"
Test-Path "D:\oracle-local\apex\images"
```

Both must return `True`. If either returns `False`, the extraction path is wrong — move the apex folder to `D:\oracle-local\apex\`.

---

## Section 7 — First Time Stack Setup (One Time Only)

> This section is done ONCE. After this, use Section 9 (Daily Startup) for all future sessions.

### Step 1 — Start Oracle DB Only

```powershell
cd D:\oracle-local
docker compose up oracle-db -d
```

### Step 2 — Wait for Database Ready

```powershell
docker logs -f oracle-free
```

Watch the output. Wait until you see this exact line:

```
#########################
DATABASE IS READY TO USE!
#########################
```

This takes 5–10 minutes on first run. Press `Ctrl+C` to stop following logs once you see this message.

### Step 3 — Copy APEX Installer into Container

```powershell
docker cp D:\oracle-local\apex oracle-free:/tmp/apex
```

This copies ~579 MB. Takes 1–2 minutes. No output until complete.

### Step 4 — Install APEX into the Database

```powershell
docker exec -u oracle -it oracle-free bash -c "cd /tmp/apex && sqlplus sys/Oracle_123@localhost:1521/FREEPDB1 as sysdba @apexins.sql SYSAUX SYSAUX TEMP /i/"
```

> **CRITICAL:** Do NOT interrupt this command. It runs for 10–20 minutes and streams a lot of output. Let it run completely.

Wait for this completion message at the end:

```
Thank you for installing Oracle APEX 24.1.0
timing for: Complete Installation
Elapsed: XX.XX
```

### Step 5 — Start ORDS

```powershell
cd D:\oracle-local
docker compose up ords-free -d
```

Wait 2 minutes then watch ORDS logs:

```powershell
docker logs -f ords-free
```

Wait for:
```
Oracle REST Data Services initialized
```

Press `Ctrl+C` to stop following logs.

### Step 6 — Copy APEX Static Files into ORDS

Run these commands in exact order:

```powershell
# Create the static files directory inside ORDS
docker exec -it ords-free bash -c "mkdir -p /etc/ords/config/global/doc_root"

# Copy APEX images into it (~444 MB, takes 2-3 minutes)
docker cp D:\oracle-local\apex\images ords-free:/etc/ords/config/global/doc_root/i

# Copy the settings file
docker cp D:\oracle-local\settings.xml ords-free:/etc/ords/config/global/settings.xml

# Restart ORDS to pick up new settings
docker restart ords-free
```

Wait 2 minutes after the restart.

### Step 7 — Unlock APEX Database Accounts

Open VS Code, connect to Oracle (see Section 10 for VS Code setup), then run:

```sql
ALTER USER APEX_PUBLIC_USER ACCOUNT UNLOCK;
ALTER USER APEX_PUBLIC_USER IDENTIFIED BY Oracle_123;
ALTER USER FLOWS_FILES ACCOUNT UNLOCK;
ALTER USER FLOWS_FILES IDENTIFIED BY Oracle_123;
```

### Step 8 — Set APEX Admin Password

Still in VS Code, run:

```sql
BEGIN
    APEX_UTIL.SET_SECURITY_GROUP_ID(10);
    APEX_UTIL.CREATE_USER(
        p_user_name       => 'ADMIN',
        p_email_address   => 'admin@local.com',
        p_web_password    => 'Oracle_123',
        p_developer_privs => 'ADMIN');
    APEX_UTIL.SET_SECURITY_GROUP_ID(null);
    COMMIT;
END;
/
```

If you get an error saying user already exists, reset the password instead:

```sql
BEGIN
    APEX_UTIL.SET_SECURITY_GROUP_ID(10);
    APEX_UTIL.CHANGE_PASSWORD_ON_BEHALF(
        p_user_name    => 'ADMIN',
        p_new_password => 'Oracle_123');
    APEX_UTIL.SET_SECURITY_GROUP_ID(null);
    COMMIT;
END;
/
```

---

## Section 8 — APEX First Login and Workspace Setup

### Step 1 — Open APEX

Open your browser and go to:

```
http://localhost:8181/ords/apex
```

You should see the Oracle APEX login screen.

### Step 2 — Login as Admin

| Field | Value |
|---|---|
| Workspace | `INTERNAL` |
| Username | `ADMIN` |
| Password | `Oracle_123` |

### Step 3 — Create Your Development Workspace

Click **Create Workspace** and fill in:

| Field | Value |
|---|---|
| Workspace Name | `QCDEV` |
| Schema Name | `QCDEV` |
| Schema Password | `Oracle_123` |
| Workspace Admin Username | `QCADMIN` |
| Workspace Admin Password | `Oracle_123` |
| Email | your email address |

Click **Create Workspace**.

### Step 4 — Login to Your Workspace

1. Click your profile at top right → Sign Out
2. At the login screen enter:

| Field | Value |
|---|---|
| Workspace | `QCDEV` |
| Username | `QCADMIN` |
| Password | `Oracle_123#` |

You are now in your development workspace and ready to build applications.

---

## Section 9 (was Section 10) — VS Code Oracle Connection Setup

### Install Extension

Search in VS Code Extensions:
```
Oracle SQL Developer Extension for VS Code
```

Publisher: Oracle. Install it.

### Create Connection

1. Click the Oracle icon in the VS Code left sidebar
2. Click **+** to add a new connection
3. Fill in:

| Field | Value |
|---|---|
| Connection Name | `oracle-local` |
| Host | `localhost` |
| Port | `1521` |
| Service Name | `FREEPDB1` |
| Username | `sys` |
| Role | `SYSDBA` |
| Password | `Oracle_123` |

4. Click **Test Connection** — should say success
5. Click **Save**

### Run Your First Query

Create a new file `D:\oracle-local\sql\queries\test.sql` and paste:

```sql
SELECT 'Stack is working' AS STATUS FROM DUAL;
```

Highlight the query and press `Ctrl+Enter` to run. Should return `Stack is working`.

---

## Section 10 — Daily Startup Procedure

> Do this every time you want to use Oracle APEX on your laptop.

### Step 1 — Open PowerShell as Administrator

Start → search PowerShell → Right click → Run as Administrator

### Step 2 — Start Docker Desktop (if not already running)

```powershell
Start-Process "C:\Program Files\Docker\Docker\Docker Desktop.exe"
```

Wait 2–3 minutes for the whale icon in the system tray to stop animating.

Verify Docker is ready:

```powershell
docker ps
```

No error = Docker is ready.

### Step 3 — Start the Oracle Stack

```powershell
cd D:\oracle-local
docker compose up -d
```

Wait about 2 minutes. Verify both containers are running:

```powershell
docker ps
```

Expected output:
```
NAMES          STATUS
ords-free      Up X minutes
oracle-free    Up X minutes (healthy)
```

### Step 4 — Open APEX in Browser

```
http://localhost:8181/ords/apex
```

Login:
- Workspace: `QCDEV`
- Username: `QCADMIN`
- Password: `Oracle_123#`

### Step 5 — Open VS Code

Open VS Code, click the Oracle icon in the sidebar, click `oracle-local` to connect.

You are ready to work.

---

## Section 11 — Daily Shutdown Procedure

### Step 1 — Save Your Work

Save any open files in VS Code and APEX.

### Step 2 — Stop the Stack

```powershell
cd D:\oracle-local
docker compose down
```

Wait for confirmation:
```
Container ords-free    Removed
Container oracle-free  Removed
Network oracle-local_default  Removed
```

> **WARNING: NEVER run `docker compose down -v`** — the `-v` flag deletes all your database data permanently. Only use plain `docker compose down`.

### Step 3 — Optional — Close Docker Desktop

Right-click the whale icon in the system tray → Quit Docker Desktop.

Your data is safely stored in the Docker volume `oracle-data` and will be there next time you start up.

---

## Section 12 — Troubleshooting

### Docker Won't Start / Takes Too Long

**Symptom:** Docker Desktop opens but never becomes ready after 5+ minutes.

**Fix:**
```powershell
# Kill all Docker processes
taskkill /IM "Docker Desktop.exe" /F 2>$null
taskkill /IM "dockerd.exe" /F 2>$null
wsl --shutdown
Start-Sleep -Seconds 5

# Relaunch
Start-Process "C:\Program Files\Docker\Docker\Docker Desktop.exe"
```

Wait 3 minutes and test `docker ps`.

---

### ORDS Container Not Starting

**Symptom:** `docker ps` shows oracle-free healthy but ords-free is missing or exited.

**Fix:**
```powershell
docker logs ords-free --tail 20
```

If logs show `config directory is empty`:
```powershell
docker exec -it ords-free bash -c "mkdir -p /etc/ords/config/global/doc_root"
docker cp D:\oracle-local\apex\images ords-free:/etc/ords/config/global/doc_root/i
docker cp D:\oracle-local\settings.xml ords-free:/etc/ords/config/global/settings.xml
docker restart ords-free
```

---

### APEX Shows "Account Is Locked" Error

**Symptom:** Browser shows HTTP 570 Account Is Locked when accessing APEX.

**Fix — run in VS Code:**
```sql
ALTER USER APEX_PUBLIC_USER ACCOUNT UNLOCK;
ALTER USER APEX_PUBLIC_USER IDENTIFIED BY Oracle_123;
ALTER USER FLOWS_FILES ACCOUNT UNLOCK;
ALTER USER FLOWS_FILES IDENTIFIED BY Oracle_123;
```

---

### APEX Shows "App Unavailable" on Landing Page

**Symptom:** ORDS landing page shows Oracle APEX tile as "App Unavailable".

**Fix — run in VS Code:**
```sql
UPDATE SYS.REGISTRY$
SET STATUS = 1
WHERE CNAME = 'Oracle APEX';
COMMIT;

ALTER USER APEX_PUBLIC_USER ACCOUNT UNLOCK;
ALTER USER APEX_PUBLIC_USER IDENTIFIED BY Oracle_123;
COMMIT;
```

Then restart ORDS:
```powershell
docker restart ords-free
```

---

### Port Already In Use

**Symptom:** Container fails to start with port binding error.

**Fix:**
```powershell
# Find what is using port 1521
netstat -ano | findstr :1521

# Kill the process (replace XXXX with the PID number from above)
taskkill /PID XXXX /F
```

---

### Forgot APEX Password

**Fix — run in VS Code:**
```sql
BEGIN
    APEX_UTIL.SET_SECURITY_GROUP_ID(10);
    APEX_UTIL.CHANGE_PASSWORD_ON_BEHALF(
        p_user_name    => 'ADMIN',
        p_new_password => 'Oracle_123');
    APEX_UTIL.SET_SECURITY_GROUP_ID(null);
    COMMIT;
END;
/
```

---

### Container Data Lost After Restart

**Cause:** You accidentally ran `docker compose down -v` which deletes volumes.

**Prevention:** Never use `-v` flag. Always use `docker compose down` only.

**Recovery:** If data is lost, you must repeat Section 7 (First Time Stack Setup) from Step 1.

---

## Section 13 — Quick Reference Card

### All URLs

| What | URL |
|---|---|
| APEX Login | `http://localhost:8181/ords/apex` |
| ORDS Landing Page | `http://localhost:8181/ords` |
| SQL Developer Web | `http://localhost:8181/ords` → click Go under SQL Developer Web |

### All Credentials

| What | Value |
|---|---|
| Oracle DB Password (SYS/SYSTEM) | `Oracle_123` |
| ORDS Internal Password | `Ords_123` |
| APEX Internal Admin | Workspace: `INTERNAL` / User: `ADMIN` / Password: `Oracle_123` |
| APEX Dev Workspace | Workspace: `QCDEV` / User: `QCADMIN` / Password: `Oracle_123#` |
| VS Code DB Connection | Host: `localhost` / Port: `1521` / Service: `FREEPDB1` / User: `sys` / Role: `SYSDBA` |

### Key Commands

```powershell
# Start stack
cd D:\oracle-local
docker compose up -d

# Stop stack (SAFE)
cd D:\oracle-local
docker compose down

# Check container status
docker ps

# Watch Oracle DB logs
docker logs -f oracle-free

# Watch ORDS logs
docker logs -f ords-free

# Restart ORDS only
docker restart ords-free

# Connect to Oracle DB directly
docker exec -it oracle-free sqlplus sys/Oracle_123@localhost:1521/FREEPDB1 as sysdba
```

### Folder Structure

```
D:\oracle-local\
├── docker-compose.yml
├── settings.xml
├── apex\
│   └── images\
└── sql\
    ├── schema\
    ├── seed\
    └── queries\
```

### Docker Volumes (Where Your Data Lives)

| Volume | What It Stores |
|---|---|
| `oracle-data` | All Oracle database files — your tables, data, APEX config |
| `ords-config` | ORDS configuration and static file references |

These volumes survive `docker compose down`. They are only deleted by `docker compose down -v`.

---

## Appendix — Technology Version Reference

| Component | Version Used | Notes |
|---|---|---|
| Windows 10 | 22H2 (Build 19045) | Minimum build 19041 |
| Docker Desktop | 4.28.0 | Do NOT use 4.76+ on Windows 10 |
| Oracle Database Free | 23ai (23.26.2) | Latest as of setup date |
| ORDS | 26.1.2 | Bundled with Oracle ORDS image |
| APEX | 24.1.0 | Manually installed — English only |
| VS Code Extension | Oracle SQL Developer Extension for VS Code | Published by Oracle |
| Docker Backend | Hyper-V | WSL2 backend has known bug on Windows 10 |