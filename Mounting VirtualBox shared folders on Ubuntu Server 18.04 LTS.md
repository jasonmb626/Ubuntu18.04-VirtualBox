# Mounting VirtualBox shared folders on Ubuntu Server 18.04 LTS

(Adapted from https://gist.github.com/estorgio/1d679f962e8209f8a9232f7593683265)

This guide will walk you through steps on how to setup a VirtualBox shared folder inside your Ubuntu Server guest. Tested on Ubuntu Server 18.04.2 LTS (Bionic Beaver)

## Steps:
1. Open VirtualBox
2. Right-click your VM, then click **Settings**
3. Go to **Shared Folders** section
4. Add a new shared folder
5. On **Add Share** prompt, select the **Folder Path** in your host that you want to be accessible inside your VM.
6. In the **Folder Name** field, type `shared`
7. Uncheck **Read-only** and **Auto-mount**, and check **Make Permanent**
8. Start your VM

9. Once your VM is up and running, go to **Devices** menu -> **Insert Guest Additions CD image menu**

10. Use the following commands to mount the CD:
```
sudo mkdir /media/cdrom
sudo mount /dev/cdrom /media/cdrom
```
11. Install dependencies for VirtualBox guest additions:
```
sudo apt-get update
sudo apt-get install build-essential
```
12. Run installation script for the guest additions:
```
sudo /media/cdrom/./VBoxLinuxAdditions.run
```
13. Reboot VM
```
sudo shutdown -r now
```
14. Create "shared" directory in your home
```
mkdir ~/shared
```
15. Mount the shared folder from the host to your ~/shared directory
```
sudo mount -t vboxsf shared ~/shared
```
16. The host folder should now be accessible inside the VM.
```
cd ~/shared
```
## Make the mount folder persistent
This directory mount we just made is temporary and it will disappear on next reboot. To make this permanent, we'll set it so that it will mount our `~/shared` directory on system startup. UPDATE: I tried these steps and upon reboot the server went into emergency mode. Something is not right.

1. Edit fstab file in /etc directory
```
sudo nano /etc/fstab
```
2. Add the following line to fstab (separated by tabs) and press Ctrl+O to Save.
```
shared	/home/<username>/shared	vboxsf	defaults	0	0
```
3. Edit modules
```
sudo nano /etc/modules
```
4. Add the following line to `/etc/modules` and save
```
vboxsf
```
5. Reboot the vm and log-in again
```
sudo shutdown -r now
```
6. Go to your home directory and check to see if the file is highlighted in green. 
```
cd ~
ls
```
If it is then congratulations! You successfully linked the directory within your vm with your host folder.
