# file-sharing-using-scp-and-rsync

One essential requirement for building a powerful home cybersecurity lab is the ability to securely and efficiently transfer files between your Ubuntu Server VM and your Windows client machine.

This guide provides two methods to transfer files, all from the Ubuntu server VM terminal, covering:
1. SCP (Secure Copy over SSH)
2. Rsync (Fast file sync)

## The following steps were carried out sequentially.
Check out the **screenshot folder** for the screenshots of the entire process.
> Anything inside the symbol <> is a placeholder; replace it with your system's values or argument.<br>
  When copying files, always include the file extension when writing file names, for example, myfile.html, packet.pcap, test.txt. etc.<br>
  "username" is different from "nameofuser". The username could be "agbuenoch", while the name of the user is "Agbu Enoch".

### Step 1: Settings/Configurations of “SSH and rsync” on Ubuntu Server VM.
Install and enable SSH and rsync on the Ubuntu server.<br>
**The list of all commands used in Step 1:**<br>
**Ubuntu server Bash**<br>
ip a <br>
ssh --version<br>
wireshark --version<br>
rsync --version<br>
sudo systemctl status ssh<br>
sudo systemctl status rsync<br>
ssh <ubuntu_server_username>@<ubuntu_server_IPv4>

### Step 2: Settings/Configurations of “SSH and rsync” on Windows Client Machine.  
**OPTION A: Using Windows Subsystem for Linux (WSL) (PREFERRED/RECOMMENDED):** Using the Windows Subsystem for Linux (WSL), install and enable SSH and rsync.<br>
**The list of all commands used in Step 2, Option A<br>**
**Windows PowerShell**<br>
wsl --install<br>
**Ubuntu server Bash**<br>
sudo apt install ssh<br>
ssh --version<br>
rsync --help<br>
sudo apt install rsync<br>
cd /mnt/c/<br>
ls -l | head -n 4<br>
whoami<br>
ip a<br>

**OPTION B: Using PowerShell (OPTIONAL).**: Using PowerShell, install and enable SSH. Note that rsync cannot be installed directly using PowerShell because it is a Linux tool.<br>
**The list of all commands used in Step 2, Option B:<br>**
**Windows PowerShell**<br>
Add-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0<br>

Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0<br>
Get-Service sshd<br>
Get-Service -Name sshd -StartupType 'Automatic'<br>
Start-Service sshd<br>
ipconfig<br>
whoami<br>
exit<br>
**Ubuntu server Bash**<br>
ping <windows_IPv4>
ssh <windows_username>@<windows_IPv4>

### Step 3: Copy Files using "scp".
**OPTION A: Copy from Ubuntu server VM to Windows client machine via WSL<br>**
**The list of all commands used in Step 3, Option A:<br>**
**Ubuntu server Bash**<br>
_sudo scp /ubuntu-server/path/to/filename <WSL-username>@<WSL-ip>:"/mnt/c/Users/windows-nameofuser/.../destination/"_<br>
  
**OPTION B: Copy file from Ubuntu server VM directly to Windows machine (NOT via WSL).<br>**
**The list of all commands used in Step 3, Option B:<br>**
**Ubuntu server Bash**<br>
ls -l<br>
_sudo scp /ubuntu-server/path/to/filename <windows-username>@windows-ip:"C:\\Users\\windows-nameofuser\\...\\destination\\"_<br>
  
**OPTION C: Copy file from Windows client machine (via WSL) to Ubuntu server VM.<br>**
**The list of all commands used in Step 3, Option C:<br>**
**Ubuntu server Bash**<br>
pwd<br>
ls -l<br>
_sudo scp <WSL-username>@<WSL-ip>:"/mnt/c/Users/windows-nameofuser/.../filename" /ubuntu-server/path/to/destination_
  
### Step 4: Copy files using "rsync" (Fast Sync for Large Files).
**OPTION I: Copy from Ubuntu server VM to Windows machine via WSL.<br>**
**The list of all commands used in Step 4, Option I:<br>**
**Ubuntu server Bash**<br>
pwd<br>
ls -l<br>
_sudo rsync -avz /ubuntu-server/path/to/filename <WSL-username>@<WSL-ip>:"/mnt/c/Users/windows-nameofuser/.../destination/"_<br>

> The -avz are **options** (also called "**flags**") we passed to rsync, and they control how the file transfer happens.<br>
  -a => Archive mode. It preserves important file properties (permissions, timestamps, symbolic links, etc.). It ensures the copied file  
  is a true replica of the original.<br>
  -v => Verbose. It makes the command show you more details during the transfer. You’ll see what files are being copied in real-time, 
  which is helpful for monitoring.<br>
  -z = Compression. This compresses file data during transfer. Especially useful for transferring over a network (like Ubuntu VM to WSL) 
  to speed up the process, especially for large files.<br>
  
**OPTION II: Copy file from Ubuntu server VM directly to Windows machine (NOT via WSL).<br>**
“rsync” is a Linux Tool. You cannot directly “rsync” from Ubuntu to a pure native Windows client machine without some Linux-like layer (WSL or Cygwin) on the Windows side. Therefore,     this option is not achievable, except you directly install “rsync” on the native Windows client machine (which is rare and complicated).<br>

**OPTION III: Copy file from Windows machine (via WSL) to Ubuntu server VM.<br>**
**The list of all commands used in Step 4, Option III:<br>**
**Ubuntu server Bash**<br>
pwd<br>
ls -l<br>
_sudo rsync -avz <WSL-username>@<WSL-ip>:"/mnt/c/Users/windows-nameofuser/.../filename" /ubuntu-server/path/to/destination_

## Which Method to Use?
1. For one-time transfers: **scp**. 
2. For large files: **rsync**.
