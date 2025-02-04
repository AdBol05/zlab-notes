`sudo apt install dpkg-dev`

```
~/projects/  
└── <projekt>  
    ├── DEBIAN  
    │   └── control  
    └── usr  
        └── uzlabina  
            └── <projekt>
```

`~/projects/DEBIAN/control`
```bash
Package: projekt  
Version: 1.0
Section: utils  
Priority: optional  
Architecture: all  
Maintainer: I3C <I3C@test.com>  
Description: popis
```

```bash
sudo dpkg-deb --build projetcs/<projekt>
sudo mkdir /usr/<uzlabina-repo>/
sudo mv projects/<project>.deb /usr/uzlabina-repo
cd /usr/uzlabina-repo/
dpkg-scanpackages . /dev/null > Packages
```

`/etc/apt/sources.list`
```bash
deb [trusetd=yes] file:///usr/uzlabina-repo ./
```

```bash
sudo apt udpate
sudo apt install <projekt>
```