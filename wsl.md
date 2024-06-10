# Fix hyperwisor issues/ How to enable WSL on windows
Sometimes wsl will not install/run due to hypervisor issues. If the hypervisor are enabled in bios, the commands below should fix the problem. For more info see this link https://learn.microsoft.com/en-us/windows/wsl/install-manual  
  
Open a cmd/powershell in admin mode
```
bcdedit /set hypervisorlaunchtype auto 
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```  
Restart and profit

## For vmware/virtualbox
If the commands above breaks vmware/virtualbox virtual machines run the following command
```
bcdedit /set hypervisorlaunchtype off
```

# Connection usb devices
To connect usb devices you WSL supports a method call usb over ethernet. You need to install a program called usbipd-win, which is not developed by MS.  
Here is the MS guide for how-to attach usb devices to WSL. 
https://learn.microsoft.com/en-us/windows/wsl/connect-usb

The summery is:
### Install
Requires a shell with admin right
```
winget install --interactive --exact dorssel.usbipd-win
```

### List, bind and attach a device
The bind command requires a terminal in admin mode.
```
# List all devices
usbipd list
# Bind a device, requires admin right
usbipd bind --busid <busid>
# Attach a usb device to WSL
usbipd attach --wsl --busid <busid>
```

# Enabling FTDI devices
There have been some issues connecting FTDI devices to WSL. As described in this ticket https://github.com/dorssel/usbipd-win/issues/948, enabling systemd and restarting wsl should fix it.

Edit `/etc/wsl.conf` and add the following
```
[boot]
systemd=true
```

Alternatively you can recompile the wsl kernel with FTDI enabled as described here.
https://www.ralf-lang.de/2024/04/02/run-ftdi-usb-devices-on-windows-10-11-wsl2/

# Install new WSL to another drive

- Follow the guide here 
```
https://helloglenn.me/blog/installing-wsl-distro-in-different-drive-win/
```
- The images can be found here 
```
https://github.com/microsoft/WSL/blob/master/distributions/DistributionInfo.json
```
- The newer images comes as a `.AppxBundle` file. Open it with 7zip and extract the wanted `.appx` file and use that file in the guide

# Move and existing WSL intallation to another drive

If you want to move wsl to another drive, a good guide is in the following link.

https://superuser.com/questions/1701175/installing-ubuntu-on-mnt-d-with-wsl

Here is the gist of that post

## Make necessary paths
```
mkdir D:\WSL\instances\Ubuntu2004
mkdir D:\WSL\images
cd D:\WSL\images
 
```
## Export and import on new location
```
wsl --export Ubuntu-20.04 ubuntu.tar
wsl --import Ubuntu2004 D:\WSL\instances\Ubuntu2004 ubuntu.tar --version 2
```
 
## Set default user
```
wsl ~ -d Ubuntu2004
sudo vim /etc/wsl.conf
 
```
Add the following to /etc/wsl.conf
```
[user]
default=<your_username>
```
 
## Exit wsl

```
exit
 
```
## Terminate and restart wsl
```
wsl --terminate Ubuntu2004
wsl ~ -d Ubuntu2004
```
 
## Set as default wsl if wanted
```
wsl --set-default Ubuntu2004
```
 
## Remove old wsl instance
```
wsl --unregister Ubuntu-20.04
```
