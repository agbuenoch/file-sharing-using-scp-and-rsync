### Step 1: Settings/Configurations of “SSH and rsync” on Ubuntu Server VM.
Install and enable SSH and rsync on the Ubuntu server.<br>
**The list of all commands used in Step 1:<br>**
**Ubuntu server Bash:**
```bash
ip a
ssh --version
wireshark --version
rsync --version
sudo systemctl status ssh
sudo systemctl status rsync
ssh <ubuntu_server_username>@<ubuntu_server_IPv4>
```

### Step 2: Settings/Configurations of “SSH and rsync” on Windows Client Machine.  
**OPTION A: Using Windows Subsystem for Linux (WSL) (PREFERRED/RECOMMENDED):** Using the Windows Subsystem for Linux (WSL), install and enable SSH and rsync.<br>
**The list of all commands used in Step 2, Option A.<br>**
**Windows PowerShell:**
```powershell
wsl --install
```

**Ubuntu server Bash:**
```bash
sudo apt install ssh
ssh --version
rsync --help
sudo apt install rsync
cd /mnt/c/
ls -l | head -n 4
whoami
ip a
```

**OPTION B: Using PowerShell (OPTIONAL).**: Using PowerShell, install and enable SSH. Note that rsync cannot be installed directly using PowerShell because it is a Linux tool.<br>
**The list of all commands used in Step 2, Option B:<br>**
**Windows PowerShell:**
```powershell
Add-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0
Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
Get-Service sshd
Get-Service -Name sshd -StartupType 'Automatic'
Start-Service sshd
ipconfig
whoami
exit
```

**Ubuntu server Bash:**
```bash
ping <windows_IPv4>
ssh <windows_username>@<windows_IPv4>
```

### Step 3: Copy Files using "scp".
**OPTION A: Copy from Ubuntu server VM to Windows client machine via WSL<br>**
**The list of all commands used in Step 3, Option A:<br>**
**Ubuntu server Bash:**
```bash
sudo scp /ubuntu-server/path/to/filename <WSL-username>@<WSL-ip>:"/mnt/c/Users/windows-nameofuser/.../destination/"
```
  
**OPTION B: Copy file from Ubuntu server VM directly to Windows machine (NOT via WSL).<br>**
**The list of all commands used in Step 3, Option B:<br>**
**Ubuntu server Bash:**
```bash
ls -l
sudo scp /ubuntu-server/path/to/filename <windows-username>@windows-ip:"C:\\Users\\windows-nameofuser\\...\\destination\\"
```
  
**OPTION C: Copy file from Windows client machine (via WSL) to Ubuntu server VM.<br>**
**The list of all commands used in Step 3, Option C:<br>**
**Ubuntu server Bash:**
```bash
pwd
ls -l
sudo scp <WSL-username>@<WSL-ip>:"/mnt/c/Users/windows-nameofuser/.../filename" /ubuntu-server/path/to/destination/
```

### Step 4: Copy files using "rsync" (Fast Sync for Large Files).
**OPTION I: Copy from Ubuntu server VM to Windows machine via WSL.<br>**
**The list of all commands used in Step 4, Option I:<br>**
**Ubuntu server Bash:**
```bash
pwd
ls -l
sudo rsync -avz /ubuntu-server/path/to/filename <WSL-username>@<WSL-ip>:"/mnt/c/Users/windows-nameofuser/.../destination/"
```

> The -avz are **options** (also called "**flags**") we passed to rsync, and they control how the file transfer happens.<br>
**-a => Archive mode**. It preserves important file properties (permissions, timestamps, symbolic links, etc.). It ensures the copied file    is a true replica of the original.<br>
**-v => Verbose**. It makes the command show you more details during the transfer. You’ll see what files are being copied in real-time, 
which is helpful for monitoring.<br>
**-z = Compression**. This compresses file data during transfer. Especially useful for transferring over a network (like Ubuntu VM to WSL) to speed up the process, especially for large files.<br>

**OPTION II: Copy file from Ubuntu server VM directly to Windows machine (NOT via WSL).<br>**
“rsync” is a Linux Tool. You cannot directly “rsync” from Ubuntu to a pure native Windows client machine without some Linux-like layer (WSL or Cygwin) on the Windows side. Therefore,     this option is not achievable, except you directly install “rsync” on the native Windows client machine (which is rare and complicated).<br>

**OPTION III: Copy file from Windows machine (via WSL) to Ubuntu server VM.<br>**
**The list of all commands used in Step 4, Option III:<br>**
**Ubuntu server Bash:**
```bash
pwd
ls -l
sudo rsync -avz <WSL-username>@<WSL-ip>:"/mnt/c/Users/windows-nameofuser/.../filename" /ubuntu-server/path/to/destination
```
