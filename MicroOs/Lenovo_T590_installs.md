## Lenovo Thinkpad T590

### Epson printer install

Man må først finn skriveren i "Printers" applikasjonen. Deretter må det eventuelt installeres driverpakke for Epson skrivere om den ikke støttes automatisk.

```console
zypper search epson
sudo transactional-update pkg install epson-inkjet-printer-escpr
sudo reboot
```

link til epson oppdaterte drivere  - pakker - må legges på /root for å kunne installeres via shell
https://download.ebz.epson.net/dsc/search/01/search/searchModule


### Problemer med heng i Gnome pålogging

DEt viser seg via `dmesg -HT` at nouveau driveren får timeout og dermed blir det heng i pålogging.  Kombinasjonen med noveau og Wayland er ikke god for eldre laptops.
Denne PC har sekundært grafikkkort fra NVIDIA som heter GXM250 eller lign.

```console
zypper search xf86-video 
  | xf86-video-intel         | Intel video driver for the Xorg X ser-> | package
  | xf86-video-intel-32bit   | Intel video driver for the Xorg X ser-> | package
  | xf86-video-mach64        | ATI Mach64 series video driver for th-> | package
  | xf86-video-mga           | Matrox video driver for the Xorg X se-> | package
  | xf86-video-neomagic      | Neomagic video driver for the Xorg X -> | package
  | xf86-video-nouveau       | Accelerated Open Source driver for nV-> | package
  | xf86-video-nv            | NVIDIA video driver for the Xorg X se-> | package
  | xf86-video-openchrome    | Openchrome driver (VIA GPUs) for the -> | package
  | xf86-video-qxl           | QXL virtual GPU video driver for the -> | package
  | xf86-video-r128          | ATI Rage 128 video driver for the Xor-> | package
  | xf86-video-savage        | S3 Savage video driver for the Xorg X-> | package
  | xf86-video-siliconmotion | Silicon Motion video driver for the X-> | package
sudo transactional-update install xf86-video-nv xf86-video-intel nano
sudo reboot
```

Link til info:
https://support.huawei.com/enterprise/en/doc/EDOC1100212210/45139458/how-to-disable-the-nouveau-driver-for-different-linux-systems

for å blackliste noveau driveren:
* Create the `/etc/modprobe.d/blacklist-nouveau.conf` file and add the following information to the file:
```
blacklist nouveau
options nouveau modeset=0
```
Deretter må initrd kjøres

```console
sudo transactional-update initrd
sudo reboot
```

Tilslutt - ved pålogging, velg tannhjulet og velg `Gnome on Xorg`



