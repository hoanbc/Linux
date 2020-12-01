# Resize root partition (or how to remove the default /home partition) on CentOS 7 online
This requires you to be able to ssh into the instance using the root user account and that no services be running as users out of /home on the target machine.

The examples are from a default installation with no customation-you NEED to know what you're working with for volumes/partitions to not horribly break things.

By default, CentOS 7 uses XFS for the file system and Logical Volume Manager (LVM), creating 3 partitions: `/`,`/home` and 

## Step 1 - Copy /home Contents
To backup the contents of /home, do the following:
```bash
mkdir /temp
cp -a /home /temp/
```
Once that is finished at your back at the prompt, you can proceed to step 2.  

## Step 2 - Unmount the /home directory
```bash
umount -fl /home
```

## Step 3 - Note the size of the home LVM volume
We run the `lvs` command to display the attributes of the LVM volumes
```bash
lvs
```
Sample output:  
```bash
  LV   VG Attr       LSize   Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  home cl -wi-a----- 406.94g
  root cl -wi-ao----  50.00g
  swap cl -wi-ao----   7.81g
```

## Step 4 - Remove the home LVM volume
```bash
lvremove /dev/cl/home
```

## Step 5 - Resize the root LVM volume
Based on the output of `lvs` above, I can safely extend the root LVM by 406GiB.  
```bash
lvextend -L+406G /dev/cl/root
```

## Step 6 - Resize the root partition
```bash
xfs_growfs /dev/mapper/cl-root
```

## Step 7 - Copy the /home contents back into the /home directory
```bash
cp -a /temp/home /
```

## Step 8 - Remove the temporary location
```bash
rm -rf /temp
```

## Step 9 - Remove the entry from /etc/fstab
Using your preferred text editor, ensure you open `/etc/fstab` and remove the line for /dev/mapper/cl-home.

## Step 10 - Don't miss this!
Run the following command to sync systemd up with the changes.
```bash
dracut --regenerate-all --force
```