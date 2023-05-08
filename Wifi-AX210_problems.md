### All info samlet om hvordan jeg håndterer AX210 og dropouts

BK: 2023-05-08 (fra Lauvlia 404)

### Håndtere WIFI-pci bussen

For å finne hvilken PCI-port Wifi-kort er på:

``` console
$ lspci | grep AX
07:00.0 Network controller: Intel Corporation Wi-Fi 6 AX210/AX211/AX411 160MHz (rev 1a)
## Wifi-kort ligger da på 0000:07:00:0 
$ sudo bash -c "echo 1 > /sys/bus/pci/devices/0000:07:00.0/remove"
$ sudo bash -c "echo 1 > /sys/bus/pci/rescan" # Kan vel også gjøre rescan nede på 0000:07:00:0 også???
```
NB!  Muligens må bruke escape karakterer som vist her `echo 1 > /sys/bus/pci/devices/XXXX\:XX\:XX.X/remove && echo 1 > /sys/bus/pci/rescan`


For å hente ut info fra dmesg bruk `sudo dmesg -w -T | grep -i iwlwifi`, -T er lesbar dato, -w er vent på nye meldinger.
Grep på 'iwlwifi' som er driveren eller 'wlp7s0' som er porten


### WIFI fixer shell script (Ubuntu versjon)
Her er en som har laget et shellscript for å fixe wifi-problemer:  [wi-fi-fixer Ubuntu Edition](https://github.com/ctsdownloads/wi-fi-fixer)

**Detects and fixes common issues with Intel wifi on GNOME environments:**

- Checks if the wifi radio is off, turns it on.
- Restarts NetworkManager if down, then fixes it by restarting NetworkManager (GNOME).
- Checks for Airplane mode. turns it off.
- Checks for Intel wifi modules, if not loaded - detects and loads them.
- NEW! Is a VPN causing problems? Checks for VPNs in case it's blocking something.
- Once setup, this little script will tell you if you're connected to the wrong Wi-Fi WAP.

### Bruk av util `iw` for å sjekke interface

Kjør `$ iw dev | grep Interface | awk '{print $2}' | xargs -I {} iw {} get power_save`  og få `Power save: on|off`
 i retur.
 
```console
$ iw dev
phy#0
	Unnamed/non-netdev interface
		wdev 0x2
		addr 3c:e9:f7:dd:8c:37
		type P2P-device
		txpower 0.00 dBm
	Interface wlp7s0
		ifindex 3
		wdev 0x1
		addr 3c:e9:f7:dd:8c:36
		type managed
		channel 6 (2437 MHz), width: 20 MHz, center1: 2437 MHz
		txpower 22.00 dBm
		multicast TXQ:
			qsz-byt	qsz-pkt	flows	drops	marks	overlmt	hashcol	tx-bytes	tx-packets
			0	0	0	0	0	0	0	0		0

```

