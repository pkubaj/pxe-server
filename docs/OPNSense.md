In order to boot OPNSense via PXE,
you need to do the following:

1. Download ISO from http://opnsense.mirrorhost.pw/releases/17.7/OPNsense-17.7-OpenSSL-serial-amd64.img.bz2

   Optionally, you can verify signature of the file, to make sure
   it wasn't tampered:
   https://opnsense.org/opnsense-17-7-released/
   
   You should also check the SHA256 hash:
   `sha256 OPNsense-17.7-OpenSSL-serial-amd64.img.bz2`
   and compare it to the one in http://opnsense.mirrorhost.pw/releases/17.7/OPNsense-17.7-OpenSSL-checksums-amd64.sha256

2. Extract the ISO contents to a directory called
    `images/opsense` in TFTP root:
    `bzip2 -d OPNsense-17.7-OpenSSL-serial-amd64.img.bz2`
    
   In order to extract the contents, you need to mount
   the ISO with following commands (example from FreeBSD):
   `mount /dev/$(mdconfig -a -t vnode -f OPNsense-17.7-OpenSSL-serial-amd64.img)a /mnt`
   Then:
   `rsync -avvP /mnt/ /srv/tftp/images/opnsense/`
   You need to have `rsync` installed.

2. Relevant entry in DHCP server config:
    `option root-path /srv/tftp/images/opnsense;`
   `/srv/tftp/images/opnsense` is the path to the extracted ISO.

3. Relevant entries in PXELINUX config:
```
label OPNSense
 menu label OPNSense
 pxe images/OPNSense/boot/pxeboot
```

The path `images/pfsense/boot/OPNSense` is relative to TFTP root directory.
It's a path to the PXE loader in the extracted OPNSense installation image.

Unfortunately, there's no mechanism for unattended installation of OPNSense.