### Reduce FS from 12G to 10G (if LV)
1. unmount /FS (ex: /dev/mapper/vgroot_lvhome)
2. e2fsck -f /dev/mapper/vgroot_lvhome
3. resize2fs -p /dev/mapper/vgroot_lvhome 10G
4. lvreduce -L 10G /dev/mapper/vgroot_lvhome
5. e2fsck -f /dev/mapper/vgroot_lvhome
6. mount /FS
7. df -h /FS
