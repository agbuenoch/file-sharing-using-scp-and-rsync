# file-sharing-using-scp-and-rsync

One essential requirement for building a powerful home cybersecurity lab is the ability to securely and efficiently transfer files between your Ubuntu Server VM and your Windows client machine.

This guide provides two methods to transfer files, all from the Ubuntu server VM terminal, covering:
- SCP (Secure Copy over SSH)
- Rsync (Fast file sync)

**IMPORTANT**:
- For best security practices, use virtual machines for the Ubuntu Server and Windows Client Machines.
- Ensure the systems have a means of authenticating users, that is username and password for login to the system. This is because, during file sharing, you will be prompted to provide both the source and destination system passwords.
- `SSH` and `rsync` must be installed and enabled on the two systems that intend to share/copy files.
- To confirm if a command/tool is installed on a terminal `Bash, zsh, Powershell, or Windows Subsystem for Linux–WSL`, type `commandName` or `commandName --version` or `commandName> --help`, if it returns the version or usage information, it means that the command/tool is installed, otherwise it will output that the `command <commandName>` not found or is not recognise, and it will provide you with the command to install that specific command/tool.
- When copying files, always include the file extension when writing file names, for example `myfile.html, packet.pcap, test.txt` etc.
- Disable your `SSH` service when not in use.
- Anything inside the symbol `<>` is a placeholder; replace it with your system's values or argument.<br>
- When copying files, always include the file extension when writing file names, for example, myfile.html, packet.pcap, test.txt. etc.<br>
- "username" is different from "nameofuser". The username could be "agbuenoch", while the name of the user is "Agbu Enoch".

## Step 1: Settings/Configurations of “SSH and rsync” on Ubuntu Server VM.
First, view the IP address of your Ubuntu server VM. Look for an entry under the "inet" section usually for the interface `eth0`, or `ens33`. This will show the IP address pointed to by the 1st and 2nd arrows.<br>
**View: [step1A](screenshots/step1A)**

Run `ssh --version` to verify SSH is installed. The usage information is returned, which means SSH is already installed on the Ubuntu server VM as pointed to by the 2nd arrow.<br>
**View: [step1A2](screenshots/step1A2)**

In an instance where a command is not yet installed, the terminal will not return the version/usage information, and it will provide you with a guide or command to install the specific command. For example, Wireshark is not yet installed, as pointed to by the 1st arrow. The 2nd arrow points to the command provided by the terminal to install the Wireshark tool.<br>
**View: [step1A3](screenshots/step1A3)**

Let's verify that “rsync” is installed on our Ubuntu server VM. As pointed out by the 1st arrow, the command returned the version and usage information.<br>
**View: [step1A4](screenshots/step1A4)**

We can also verify the status of the SSH service, as pointed out by the 2nd arrow below, the “SSH” service on the Ubuntu server VM is enabled and actively running.<br>
**View: [step1B](screenshots/step1B)**

Unlike “SSH” as shown above, “rsync” does not need to be enabled or running as a service (daemon) to work for file copying. So you can ignore the rsync “inactive (dead)” service status as pointed to by the 1st arrow, and use “rsync” normally for all your file transfer tasks.<br>
**View: [step1B2](screenshots/step1B2)**

> The `rsync` service shows `inactive (dead)` because by default, the `rsync` service (daemon mode) is not automatically used unless we configure it explicitly with a valid `rsyncd.conf` file and start it as a daemon.

Verify **SSH access** to see if we can SSH into the Ubuntu server VM manually. You will be prompted for the Ubuntu server VM password as pointed to by the 1st arrow. If successful, you will get a “**Welcome to Ubuntu ...**” message as underlined below.

Run: `ssh <Ubuntu_Username>@<Ubuntu_IPv4>`<br>
**View: [step1C](screenshots/step1C)**
We have ensured `SSH` is enabled/installed on the Ubuntu server VM, and “rsync” is also installed.

## Step 2: Settings/Configurations of “SSH and rsync” on Windows Client Machine.
The Windows client machine is also required to have an SSH server installed and running. Therefore, let's install “SSH” on the Windows client machine and ensure “rsync” (via WSL) is also installed.

### OPTION A: Using Windows Subsystem for Linux (WSL) (PREFERRED/RECOMMENDED).
We will be using WSL alongside the Ubuntu server VM terminal for a seamless, consistent Linux environment for file sharing and copying, with full access to Linux tools like “rsync”, as well as integration with the Windows system. This will help keep everything simple and efficient.

If you do not have Windows Subsystem for Linux (WSL) installed in your Windows client machine, run the command pointed to by the 1st arrow using PowerShell to begin the download of “Ubuntu”, but if you already have WSL, skip the WSL installation step to the next step. The 2nd arrow points to the downloading progress of “Ubuntu”, which is currently at 1.5% as shown below.<br>
**View: [step2A1](screenshots/step2A1)**

After successfully installing the WSL (Ubuntu), search for “Ubuntu or Terminal” on your Windows client machine and open it. By default, you will be presented with the PowerShell interface first. Click on the caret symbol for a drop-down of other shells and click on “Ubuntu” as pointed to by the 1st and 2nd arrows, respectively.<br>
**View: [step2A1i](screenshots/step2A1i)**

Install `SSH` (A package that contains both openssh-client and openssh-server) on the WSL using the command pointed to by the 1st arrow, to enable `WSL` to accept both inbound and outbound `SSH` connections.<br>
**View: [step2A2](screenshots/step2A2)**

Let's now verify if “SSH” and “rsync” are installed. Run the command pointed to by the 1st arrow, “SSH” usage information is returned, therefore it is installed.<br>
**View: [step2A3](screenshots/step2A3)**

Run the command pointed to by the 1st arrow; it is verified that `rsync` is installed, hence the return of the version and usage information as pointed to by the 2nd arrow.<br>
**View: [step2A4](screenshots/step2A4)**

But if the “rsync” command is not installed,  run the command pointed to by the 1st arrow to install it. Enter your user password as pointed out by the 2nd arrow. I already have `rsync` installed as pointed out by the 3rd arrow.<br>
**View: [step2A5](screenshots/step2A5)**

Therefore, we now have both `SSH` and `rsync` installed on both the Ubuntu server VM and the Windows client machine (via WSL).

**WSL Path Conventions:**
`Windows drives` are mounted inside `WSL` at `/mnt/`, for example:<br>
The Windows path/directory `C:\Users\YourName\Downloads\` Is written in WSL as `/mnt/c/Users/YourName/Downloads/`

Also, if you are copying/sharing files to a Windows client machine from Ubuntu server VM terminal using the `SSH` and `rsync` running on the WSL, use the Linux path convention for Windows client machine which is /mnt/c/ and NOT C:/ because in this scenario we are NOT using the “SSH” running directly on the Windows client machine through Powershell but the “SSH” running on WSL.

The file will land in the Windows client machine filesystem `C:\...`, but the connection goes through the Linux environment of WSL.

From the screenshot below, I want to access the Windows client machine `C:\` drive from the `WSL`. I changed the directory to `/mnt/c/` as underlined below. I listed the files and directory as pointed to by the 2nd arrow. The lists are long, so pass the `ls -l` output to `head -n 4` to print four lines of the output.<br>
**View: [step2A6](screenshots/step2A6)**

> You can run similar commands on both the Ubuntu server VM terminal and the WSL running on the Windows machine.
Another alternative for using the Windows Subsystem for Linux (WSL) described above is to install Cygwin or Git Bash; they both have “rsync” included. Cygwin provide “Linux feelings” on Windows client machines just like WSL; it provides Windows client machines with similar Linux functionality, but note that this does not mean you can run Linux native apps on Windows. But NOTE that WSL provide more complete/native Linux-like capabilities on Windows machines compared to Cygwin's lightweight/limited capabilities.

**Gather WSL details.**<br>
Let's check for the WSL username using the command pointed to by the 1st arrow. The 2nd arrow points to the username returned.<br>
**View: [step2A7](screenshots/step2A7)**

Check for the WSL IP address. Run the command pointed to by the 1st arrow to view the IPv4 address pointed to by the 3rd arrow of your network interface pointed to by the 2nd arrow.<br>
**View: [step2A8](screenshots/step2A8)<br>**
Take notes of these details, they will be required when we want to copy or share a file with another system.

### OPTION B: Using PowerShell (OPTIONAL).
“rsync” cannot be directly installed using PowerShell; it will have to be installed using the WSL, just as demonstrated above. Unlike `rsync`, even without the WSL, `SSH` can be installed directly on a Windows client machine and used to share/copy files with remote systems. This is why it is recommended to use `WSL`, which can enable the use of both `SSH` and `rsync`.

Let's enable `SSH` directly on the Windows client machine. Open a PowerShell on your Windows client machine and run the commands below as pointed to by the 1st arrow, as an administrator:

Installing OpenSSH client, made for outbound SSH connections to remote systems.<br>
**View: [step2B1](screenshots/step2B1)**

Installing OpenSSH server, made for inbound SSH connections from remote systems.<br>
**View: [step2B2](screenshots/step2B2)**

The Openssh-client and Openssh-server have been successfully installed as pointed to by the arrows below.<br>
**View: [step2B3](screenshots/step2B3)<br>**
**View: [step2B4](screenshots/step2B4)**

We would set the Openssh server to automatically start each time we boot our system. Run the command pointed out by the 1st arrow to show the current status of the server, which is “Stopped” as pointed out by the 2nd arrow. Run the command pointed out by the 3rd arrow to automatically start the Openssh server each time we boot the system. The command pointed out by the 4th arrow will start the Openssh server. When we view the Openssh server this time using the command pointed to by the 5th arrow, the status has changed to “Running” as pointed to by the 6th arrow.<br>
**View: [step2B5](screenshots/step2B5)**

**IMPORTANT:<br>**
`Stop-Service sshd`  =>Run this command to stop the openssh server.<br>
`Start-Service sshd` =>Run this command to start the openssh server.

**Get the Windows Client Machine details.<br>**
Note down these details, it will be required when sharing/copying files with another system.

When you run the command `ipconfig` in Windows PowerShell, you may see multiple IPv4 addresses—especially if you have: 
- WiFi and Ethernet (wired) connections
- VPNs (like OpenVPN, NordVPN), or
- Virtual network adapters (VMware, VirtualBox, WSL).

**Pick the IP based on your network setup:<br>**
- If you are connected via WiFi: Use the IP under Wireless LAN adapter Wi-Fi. Example: 192.168.1.100
- If connected via Ethernet (via a cable): Use the IP under Ethernet adapter Ethernet. Example: 10.0.0.5
- Ignore these IPs:172.x.x.x (usually Windows Subsystem Linux–WSL or Docker). 192.168.152.1 (VMware NAT interface—only for VM-internal traffic). These IPs are NOT used for the transfer/sharing of files.

Therefore, let's find the Windows client machine `IPs`. Run the command pointed to by the 1st arrow. The 2nd, 3rd and 4th arrows point to other available network interface adapters.<br>
**View: [step2C1](screenshots/step2C1)**

I am connected via WiFi as underlined below. If the file sharing/copying will be directly with the Windows client machine (NOT via WSL), the IPv4 address under it shall be used.<br>
**View: [step2C2](screenshots/step2C2)**

**Verify Connectivity:<br>**
From your Ubuntu VM terminal, ping the chosen Windows client machine IP as pointed to by the 1st arrow. If ping succeeds, this is the correct IP. If ping fails: Try another IPv4 address from the list. As pointed out by the 2nd arrow, we have successfully pinged the Windows IP.<br>
**View: [step2C3](screenshots/step2C3)**

**NOTE: VMware Network Mode.<br>**
- Bridged: Your VM shares the host’s WiFi/Ethernet IP (e.g., `192.168.1.100`).
- NAT: Your VM gets a separate IP (e.g., `192.168.152.x`), but the host’s IP is still `192.168.1.100`.

**Check the Windows client username:<br>**
Run the command pointed to by the 1st arrow. Your Windows client machine username will be returned as underlined and pointed to by the 2nd arrow.<br>
**View: [step2D1](screenshots/step2D1)**

**Verify “SSH” Connectivity to the Windows Client Machine:**
From the Ubuntu VM terminal, let's verify we can SSH into the Windows client machine after we have enabled the `OpenSSH server` and identified the `Windows client machine's IP`. If your username has a space in it, remember to wrap it in a quote as demonstrated below. Run the command pointed to by the 1st arrow and provide the Windows client machine password, i.e the machine you want to remotely connect to.<br>
**View: [step2D2](screenshots/step2D2)**

Voila, we are remotely connected to the Windows client machine, as shown below. You have total and complete control of the Windows client machine remotely, directly through the command prompt. As pointed out by the 3rd arrow, the prompt has changed from the Ubuntu server prompt `$` to a Windows command prompt `>`.<br>
**View: [step2D3](screenshots/step2D3)**

To disconnect from the remote Windows client machine, run the command pointed to by the 1st arrow. Immediately, the connection to the Windows client machine closed as pointed to by the 2nd arrow. We are back to the Ubuntu server VM terminal as pointed to by the 3rd, 4th and 5th arrows.<br>
**View: [step2D3i](screenshots/step2D3i)**

We are now set and ready to share/copy files between the Ubuntu server VM and the Windows client machine. We can decide to share/copy files to the Windows machine via WSL (using the SSH server running on the WSL) or directly to the Windows machine (using the SSH server running on the Windows client machine installed through PowerShell).

## Step 3: Copy Files using "scp".
**IMPORTANT:** 
- All commands will be run/executed from the Ubuntu server VM terminal for both copying from the Ubuntu server VM to the Windows client machine and copying from the Windows client machine to the Ubuntu server VM.
- Ensure both `SSH` and `rsync` are installed on the `WSL` and `SSH` on the Windows client machine (through PowerShell).
- If your username or a file path/directory has a space in it, ensure you enclose it with a double quotation mark like this: `agbu enoch amachundi` or `C:\\Users\\Agbu Enoch Amachundi` or `/mnt/c/Users/Agbu Enoch Amachundi`.
- The Windows `C:\` drive is located at `/mnt/c/` on the WSL or any Linux machine. For example, file path in Windows client machine:- `C:\\Users\\Agbu Enoch Amachundi\\`. File path in Linnux machine `/mnt/c/Users/Agbu Enoch Amachundi/`
- Copying/sharing files between the Ubuntu server VM and the Windows client machine is best recommended to use `Windows Subsystem for Linux (WSL)` running on the Windows machine, while `SSH` and/or `rsync` are installed/running on `WSL`.
- "**username**" is different from "**nameofuser**". The username could be "**agbuenoch**", while the name of the user is "**Agbu Enoch**".

### OPTION A: Copy from Ubuntu server VM to Windows client machine via WSL.
```bash
sudo scp /ubuntu-server/path/to/filename <WSL-username>@<WSL-ip>:"/mnt/c/Users/windows-nameofuser/.../destination/"
```
Run the command highlighted in green on the screenshot. I already have these files residing in my Ubuntu server's current directory.

After running the command in green to copy the file, provide your Ubuntu server password as pointed to by the 1st arrow, also provide the password for the WLS user as pointed to by the 2nd arrow. The 3rd arrow points to the file being copied, and the 4th arrow points to the progress percentage of the copied file. It was copied to the Windows client machine via the WSL 100%.<br>
**View: [step3A1](screenshots/step3A1)**

`scp` Is the command used to copy the file.<br>
`/ubuntu-server/path/to/filename` Is the source path.<br>
`<WSL-username>@<WSL-ip>:"/mnt/c/Users/windows-nameofuser/.../destination/` Is the destination path.

### OPTION B: Copy file from Ubuntu server VM directly to Windows machine (NOT via WSL):
```bash
sudo scp /ubuntu-server/path/to/filename <windows-username>@<windows-ip>:"C:\\Users\\windows-nameofuser\\...\\destination\\"
```
We will copy a file directly to a Windows client machine (not through WSL) via OpenSSH installed on Windows using PowerShell, just as demonstrated in `OPTION B of Step 2`:

The first command in green lists out the files and directories in the current folder, among which are two files we shall be copying, as pointed to by the 1st and 2nd arrows.

You will be authenticated to provide a password for both the source system `Ubuntu server` and the destination system `Windows client machine` you intend to copy to, as pointed to by the 3rd and 5th arrow. The 6th arrow shows the files were 100% copied.<br>
**View: [step3A2](screenshots/step3A2)**

`./*.*` Means, from the current path/directory `./`, copy all files with zero or more character length `*`, followed by a dot `.` and then extension (e.g `.html`, `.txt`) having zero or more character length `*`.

Note: The file transfer is directly with the native Windows client machine; therefore, the Windows machine path convention of `C:\` must be strictly followed. One of the double backwards slashes is made to escape the other backwards slash because the Windows path is enclosed within a quotation mark.

Voila, we have successfully and securely copied the two files to our Windows client machine.<br>
**View: [step3A3](screenshots/step3A3)**

### OPTION C: Copy file from Windows client machine (via WSL) to Ubuntu server VM.
```bash
sudo scp <WSL-username>@<WSL-ip>:"/mnt/c/Users/windows-nameofuser/.../filename" /ubuntu-server/path/to/destination
```
Let's copy the file `ARP+Storm.pcap` to the Ubuntu server VM's current path/directory `./`.

Provide the password for the `WSL user` as pointed to by the 2nd arrow. The 3rd and 4th arrows point to the file name and the percentage of the file copied, respectively.

The 5th arrow shows that the file has been successfully copied to the Ubuntu server by listing all the files in the current path.<br>
**View: [step3A4](screenshots/step3A4)**

Therefore:<br>
`<WSL-username>@<WSL-ip>:"/mnt/c/Users/windows-nameofuser/.../filename"` Is the source path.<br>
`/ubuntu-server/path/to/destination` Is the destination path.

## Step 4: Copy files using "rsync" (Remote SYNCchronisation).
`rsync` refers to "Remote SYNChronization". This is similar to the `scp` command; replace the `scp` command with the `rsync` command.

### OPTION I: Copy from Ubuntu server VM to Windows machine via WSL.
```bash
sudo rsync -avz /ubuntu-server/path/to/filename <WSL-username>@<WSL-ip>:"/mnt/c/Users/windows-nameofuser/.../destination/"
```
Copy the file pointed to by the 1st arrow from the Ubuntu server to the Windows client machine using the `rsync`.

We break the command into `two lines` by using the backslash `\` as pointed to by the 4th arrow, then press Enter. Provide the Ubuntu server user password as pointed to by the 2nd arrow. We have successfully copied the file to the Windows client machine using rsync, as pointed out by the 3rd arrow, we are provided with additional details regarding the copying of the files, including size and speed.<br>
**View: [step4A1](screenshots/step4A1)**

The `-avz` are options (also called "flags") we passed to `rsync`, and they control how the file transfer happens.<br>
`-a` **Archive mode**. It preserves important file properties (permissions, timestamps, symbolic links, etc.). It ensures the copied file is a true replica of the original.<br>
`-v` **Verbose**. It makes the command show you more details during the transfer. You’ll see what files are being copied in real-time, which is helpful for monitoring.<br>
`-z` **Compression**. This compresses file data during transfer. Especially useful for transferring over a network (like Ubuntu VM to WSL) to speed up the process, especially for large files.<br>

### OPTION II: Copy file from Ubuntu server VM directly to Windows machine (NOT via WSL).
First of all, `rsync` is a Linux Tool. You cannot directly `rsync` from Ubuntu to a pure native Windows client machine without some Linux-like layer `WSL` or `Cygwin`, on the Windows side. Therefore, this option is not achievable, except you directly install `rsync` on the native Windows client machine (which is rare and can be very complicated).

To prove to you that `rsync` is not running on a native Windows client machine, let's type the `rsync` command on PowerShell, as pointed by the 2nd and 3rd arrow, the command `rsync` is not recognised as a command in the PowerShell.<br>
**View: [step4A2](screenshots/step4A2)**
> I deliberately brought out this OPTION II, so you can understand when and where `scp` and `rsync` can be used.

### OPTION III: Copy file from Windows machine (via WSL) to Ubuntu server VM.
```bash
sudo rsync -avz <WSL-username>@<WSL-ip>:"/mnt/c/Users/windows-nameofuser/.../filename" /ubuntu-server/path/to/destination
```
Let’s copy a file `testing.html` from the Windows client machine via the `WSL` to the Ubuntu server VM at the current directory/path `./` as pointed to by the 2nd arrow.

Provide the `WSL user password` as pointed to by the 3rd arrow. The 4th arrow points to the copied file details during transit.

The 5th arrow points to the copied file after successfully copying it to the Ubuntu server VM.<br>
**View: [step4A3](screenshots/step4A3)**

**NOTE:** The `OPTION III` worked because `WSL` is standing as an intermediary between the Windows client machine and the Ubuntu server VM, and because `WSL` is an Ubuntu system, `rsync` was installed and can be used to communicate with another Linux system (Ubuntu server VM).

**Which Method to Use?**
- For one-time transfers: `scp`. 
- For large files: `rsync`

## LinkedIn Article.
- [File Sharing Using scp and rsync](https://www.linkedin.com/pulse/file-sharing-using-scp-rsync-enoch-agbu-yeynf/)

## Connect with me.
[🔗 LinkedIn](https://www.linkedin.com/in/agbuenoch)<br>
[🔗 X](https://www.x.com/agbuenoch)
