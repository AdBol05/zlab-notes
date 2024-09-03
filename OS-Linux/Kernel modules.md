
```bash
sudo lsmod

sudo modprobe <module name>
sudo modprobe -r <module name>

sudo insmod <module path>
sudo rmmod <module path>


sudo systool -v -m joydev
```

## Blacklist
`/etc/modprobe.d/blacklist.conf`

```bash
blacklist <module name>
```


## Ovladače při bootu
`/etc/initramfs-tools/modules`

```
lz4
z3fold
```


```bash
sudo update-initramfs -u
```