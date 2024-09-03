- Spouštěna systémem
	- systém init
	- nezávislá na uživateli

```bash
sudo systemctl <operace> <služba>
sudo service <služba> <operace>

sudo journalctl -xeu <služba> # zvýraznění errorů a skok na konec

sudo systemctl isolate <target> <služba> # run level

sudo systemctl set-default <target> <služba>
sudo systemctl get-default <target> <služba>
```

### Run levels
0. System halt
1. Single user
2. Multi-user (bez NFS)
3. Multi-user (bez GUI)
4. User-definable
5. Multi-user GUI
6. Reboot