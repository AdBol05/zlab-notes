![[RAID konfigurace 2023-11-07 11.35.09.excalidraw]]

```bash
sudo mdadm --detail --scan

sudo mdadm --create --verbose /dev/md0 --level=mirror --raid-devices=2 /dev/sdc1 /dev/sdd1 --spare-devices=1 /dev/sdb70 --name=mujmilacek

sudo mdadm --stop /dev/md1

sudo mdadm --assemble /dev/md2 /dev/sdc3 /dev/sdd3 --update=name --name=pavlasek
```

`/etc/mdadm/mdadm.conf`
- konfigurace všech RAID sestav
- sestavuje při startu
```bash
ARRAY /dev/md0 metadata=1.2 spares=1 name=client:0 UUID=e7032549:b551aea5
```

![[RAID konfigurace 2023-12-11 08.18.45.excalidraw]]
