If you want to move wsl to another drive, a good guide is in the following link.

https://superuser.com/questions/1701175/installing-ubuntu-on-mnt-d-with-wsl

Here is the gist of that post

# Make necessary paths
```
mkdir D:\WSL\instances\Ubuntu2004
mkdir D:\WSL\images
cd D:\WSL\images
 
```
# Export and import on new location
```
wsl --export Ubuntu-20.04 ubuntu.tar
wsl --import Ubuntu2004 D:\WSL\instances\Ubuntu2004 ubuntu.tar --version 2
```
 
# Set default user
```
wsl ~ -d Ubuntu2004
sudo vim /etc/wsl.conf
 
```
#add the following to /etc/wsl.conf
```
[user]
default=<your_username>
```
 
# Exit wsl

```
exit
 
```
# Terminate and restart wsl
```
wsl --terminate Ubuntu2004
wsl ~ -d Ubuntu2004
```
 
# Set as default wsl if wanted
```
wsl --set-default Ubuntu2004
```
 
# Remove old wsl instance
```
wsl --unregister Ubuntu-20.04
```
