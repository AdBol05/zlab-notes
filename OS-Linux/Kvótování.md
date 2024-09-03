
`/etc/fstab`
```bash
UUID="asdjfhalskdfhad" /mnt/kvotovani-vysvetleni ext4 usrquota,grpquota 0 1
```

```bash
sudo quotacheck -cug /mnt/kvotovani-vyvsvetleni # -(Create User Group)
sudo edquota -u patrik # nastavení pro užeivatele
sudo edquota -ut # nastavení časových limitů (globálně)

sudo quotaon /mnt/kvotovani-vysvetleni
sudo quotaoff /mnt/kvotovani-vysvetleni

sudo repquota -u /mnt/kvotovani-vysvetleni
```

`edquota`
```bash
Disk quotas for user patrik (uid 1001):  
 Filesystem           blocks       soft       hard     inodes     soft     hard  
 /dev/sdb80               16        50M       100M          4       60       80
```