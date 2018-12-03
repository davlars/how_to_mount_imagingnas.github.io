## How to mount imagingnas on your local computer

This is a short guide on how to mount the imagingnas windows share, such that data can be accessed directly from your terminal. The installation guide is given for linux, adapted directly from the [ubuntu help](https://help.ubuntu.com/community/MountWindowsSharesPermanently). 

### Install packages

Install package needed to access the samba windows share by typing

```
sudo apt-get install cifs-utils
```

### Create credentials file

Create a credential files ```.smbcredentials``` in your home directory. Typically, this can be done by editing the file using a text editor of your choice, e.g:

```
sudo vim /home/user/.smbcredentials
```

In this file, fill in the USERNAME and PASSWORD of a user (lcruser) on imagingnas:

```
username=USERNAME
password=PASSWORD
```

NOTE: Since this text file specifically gives the username and password, immediately change the user credentials such that it is restricted to admin only. THIS IS IMPORTANT. To do that run:

```
sudo chown admin .smbcredentials
sudo chmod 600 .smbcredentials
```

### Edit /etc/fstab

Go to your ```/etc/``` folder by:

```
cd /etc/
```

Make a backup copy of your fstab

```
sudo cp fstab fsatb.orig
```

Now edit fstab (using e.g. ```sudo vim fstab```) by adding the following at the end of the file:

#### For kernels 4.12 or earlier
```
# Settings for mounting the NAS
//imagingnas.math.kth.se/data /mnt/imagingnas/data cifs credentials=/home/admin/.smbcredentials,iocharset=utf8,gid=1001,uid=1001,file_mode=0777,dir_mode=0777 0 0
//imagingnas.math.kth.se/documents /mnt/imagingnas/documents cifs credentials=/home/admin/.smbcredentials,iocharset=utf8,gid=1001,uid=1001,file_mode=0777,dir_mode=0777 0 0
//imagingnas.math.kth.se/software /mnt/imagingnas/software cifs credentials=/home/admin/.smbcredentials,iocharset=utf8,gid=1001,uid=1001,file_mode=0777,dir_mode=0777 0 0
//imagingnas.math.kth.se/Reference /mnt/imagingnas/Reference cifs credentials=/home/admin/.smbcredentials,iocharset=utf8,gid=1001,uid=1001,file_mode=0777,dir_mode=0777 0 0 
```

### For kernels 4.13 or later
```
# Settings for mounting the NAS
//imagingnas.math.kth.se/data /mnt/imagingnas/data cifs credentials=/home/admin/.smbcredentials,iocharset=utf8,gid=1001,uid=1001,file_mode=0777,dir_mode=0777,vers=1.0 0 0
//imagingnas.math.kth.se/documents /mnt/imagingnas/documents cifs credentials=/home/admin/.smbcredentials,iocharset=utf8,gid=1001,uid=1001,file_mode=0777,dir_mode=0777,vers=1.0 0 0
//imagingnas.math.kth.se/software /mnt/imagingnas/software cifs credentials=/home/admin/.smbcredentials,iocharset=utf8,gid=1001,uid=1001,file_mode=0777,dir_mode=0777,vers=1.0 0 0
//imagingnas.math.kth.se/Reference /mnt/imagingnas/Reference cifs credentials=/home/admin/.smbcredentials,iocharset=utf8,gid=1001,uid=1001,file_mode=0777,dir_mode=0777,vers=1.0 0 0 
```

### Create mount point directories

Create the mount directories by running (you have to be ```sudo``` to do this):

```
sudo mkdir /mnt/imagingnas
sudo mkdir /mnt/imagingnas/data
sudo mkdir /mnt/imagingnas/documents
sudo mkdir /mnt/imagingnas/software
sudo mkdir /mnt/imagingnas/Reference
```

### Mount the samba share

With all steps above completed, simply run:

```
sudo mount -a
```
By so, you should have access to the imagingnas directories inside:
```
/mnt/imagingnas
```
