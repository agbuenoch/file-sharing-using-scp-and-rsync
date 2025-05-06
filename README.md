# file-sharing-using-scp-and-rsync

One essential requirement for building a powerful home cybersecurity lab is the ability to securely and efficiently transfer files between your Ubuntu Server VM and your Windows client machine.

This guide provides two methods to transfer files, all from the Ubuntu server VM terminal, covering:
1. SCP (Secure Copy over SSH)
2. Rsync (Fast file sync)

## The following steps were carried out sequentially.
Check out the screenshot folder for the screenshots of the entire process.

### Step 1: Settings/Configurations of “SSH and rsync” on Ubuntu Server VM

### Step 2: Settings/Configurations of “SSH and rsync” on Windows Client Machine.  
OPTION A: Using Windows Subsystem for Linux (WSL) (PREFERRED/RECOMMENDED).
OPTION B: Using PowerShell (OPTIONAL).
  
### Step 3: Copy Files using "scp".
**OPTION A: Copy from Ubuntu server VM to Windows client machine via WSL.<br>**
_sudo scp /ubuntu-server/path/to/filename WSL-username@WSL-ip:"/mnt/c/Users/windows-nameofuser/.../destination/"_
  
**OPTION B: Copy file from Ubuntu server VM directly to Windows machine (NOT via WSL).<br>**
_sudo scp /ubuntu-server/path/to/filename <windows-username>@windows-ip:"C:\\Users\\windows-nameofuser\\...\\destination\\"_
  
**OPTION C: Copy file from Windows client machine (via WSL) to Ubuntu server VM.<br>**
_sudo scp WSL-username@<WSL-ip>:"/mnt/c/Users/windows-nameofuser/.../filename" /ubuntu-server/path/to/destination_
  
### Step 4: Copy files using "rsync" (Fast Sync for Large Files).
**OPTION I: Copy from Ubuntu server VM to Windows machine via WSL.<br>**
_sudo rsync -avz /ubuntu-server/path/to/filename WSL-username@WSL-ip:"/mnt/c/Users/windows-nameofuser/.../destination/"_
  
**OPTION II: Copy file from Ubuntu server VM directly to Windows machine (NOT via WSL).**<br>
“rsync” is a Linux Tool. You cannot directly “rsync” from Ubuntu to a pure native Windows client machine without some Linux-like layer (WSL or Cygwin) on the Windows side. Therefore,     this option is not achievable, except you directly install “rsync” on the native Windows client machine (which is rare and complicated).
  
**OPTION III: Copy file from Windows machine (via WSL) to Ubuntu server VM.**<br>
  _sudo rsync -avz WSL-username@WSL-ip:"/mnt/c/Users/windows-nameofuser/.../filename" /ubuntu-server/path/to/destination_

## Which Method to Use?
1. For one-time transfers: **scp**. 
2. For large files: **rsync**.
