### All info samlet om hvordan jeg håndterer AX210 og dropouts

BK:2023-07-07 - fortsatt problemer - endret disable_11ax=Y
```console
sudo nano /etc/modprobe.d/iwlwifi.conf

options iwlwifi disable_11ax=Y
```
Gjør søk på ```iwlwifi 0000:07:00.0: Error sending STATISTICS_CMD```


This is the subject of a well-known bug report: [https://bugzilla.kernel.org/show_bug](https://bugzilla.kernel.org/show_bug.cgi?id=212371#c13)

Let's try the suggested fix. From the terminal: ------BK: Dette fungerer ikke - får ikke lastet iwlwifi når denne ikke er der. -----

`sudo mv /usr/lib/firmware/iwlwifi-ty-a0-gf-a0.pnvm  /usr/lib/firmware/iwlwifi-ty-a0-gf-a0.bak`

Reboot and tell us if there is any improvement.

EDIT: It appears that periodic updates to the package linux-firmware will install a new version of the offending file iwlwifi-ty-a0-gf-a0.pnvm and so this process will need to be repeated.

```console
(base) bjorn@pop-os:~$ grep [[:alnum:]] /sys/module/iwl*/parameters/*
/sys/module/iwlmvm/parameters/init_dbg:N
/sys/module/iwlmvm/parameters/power_scheme:2
/sys/module/iwlwifi/parameters/11n_disable:0
/sys/module/iwlwifi/parameters/amsdu_size:0
/sys/module/iwlwifi/parameters/bt_coex_active:Y
/sys/module/iwlwifi/parameters/disable_11ac:N
/sys/module/iwlwifi/parameters/disable_11ax:N
/sys/module/iwlwifi/parameters/disable_11be:N
grep: /sys/module/iwlwifi/parameters/enable_ini: Operation not permitted
/sys/module/iwlwifi/parameters/fw_restart:Y
/sys/module/iwlwifi/parameters/led_mode:0
/sys/module/iwlwifi/parameters/nvm_file:(null)
/sys/module/iwlwifi/parameters/power_level:0
/sys/module/iwlwifi/parameters/power_save:N
/sys/module/iwlwifi/parameters/remove_when_gone:N
/sys/module/iwlwifi/parameters/swcrypto:0
/sys/module/iwlwifi/parameters/uapsd_disable:3
```
Sjekk også [SOLVED Using the AX210 with Linux on the Framework Laptop](https://community.frame.work/t/solved-using-the-ax210-with-linux-on-the-framework-laptop/1844/51?page=2)

BK: 2023-05-08 (fra Lauvlia 404)

Pr dags dato, harjeg endret powersave fra 3 til 2 og installert iwd, slik atwlan0 ikke blir renamet. iwd servicen er disablet for øyeblikket.  Skal se om powersave fikset problemet. Ucode 72 er den som er lastet i kernel release 6.2


* [Kernel.org - Linux Wireless (iwlwifi)](https://wireless.wiki.kernel.org/en/users/drivers/iwlwifi)
* [Kernel.org - index : kernel/git/iwlwifi/linux-firmware.git](https://git.kernel.org/pub/scm/linux/kernel/git/iwlwifi/linux-firmware.git/)

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

### Når adapteret feiler får jeg: Timeout waiting for hardware access (CSR_GP_CNTRL 0xffffffff)

Utdrag fra dmesg
```console
[ 1049.042543] ------------[ cut here ]------------
[ 1049.042545] Timeout waiting for hardware access (CSR_GP_CNTRL 0xffffffff)
[ 1049.042581] WARNING: CPU: 16 PID: 843 at drivers/net/wireless/intel/iwlwifi/pcie/trans.c:2172 __iwl_trans_pcie_grab_nic_access+0x192/0x1a0 [iwlwifi]
....
[ 1049.042732] CPU: 16 PID: 843 Comm: NetworkManager Tainted: P           OE      6.2.6-76060206-generic #202303130630~1680814622~22.04~3850312
[ 1049.042735] Hardware name: To be filled by O.E.M. To be filled by O.E.M./Intel X79, BIOS X79GA00C 12/21/2019
[ 1049.042737] RIP: 0010:__iwl_trans_pcie_grab_nic_access+0x192/0x1a0 [iwlwifi]
....
```
### Gå via modprobe

Sjekk output fra `$ m̀odinfo iwlwifi`

```console
# snippets fra kommandoen 
$ modinfo iwlwifi
filename:       /lib/modules/6.2.6-76060206-generic/kernel/drivers/net/wireless/intel/iwlwifi/iwlwifi.ko
license:        GPL
description:    Intel(R) Wireless WiFi driver for Linux
...
firmware:       iwlwifi-ty-a0-gf-a0-72.ucode
firmware:       iwlwifi-so-a0-gf-a0-72.ucode
firmware:       iwlwifi-so-a0-hr-b0-72.ucode
firmware:       iwlwifi-so-a0-jf-b0-72.ucode
firmware:       iwlwifi-cc-a0-72.ucode
...
srcversion:     F230833AC4C04A291C86468
...
name:           iwlwifi
vermagic:       6.2.6-76060206-generic SMP preempt mod_unload modversions 
sig_id:         PKCS#7
signer:         Build time autogenerated kernel key
...
parm:           swcrypto:using crypto in software (default 0 [hardware]) (int)
parm:           11n_disable:disable 11n functionality, bitmap: 1: full, 2: disable agg TX, 4: disable agg RX, 8 enable agg TX (uint)
parm:           amsdu_size:amsdu size 0: 12K for multi Rx queue devices, 2K for AX210 devices, 4K for other devices 1:4K 2:8K 3:12K (16K buffers) 4: 2K (default 0) (int)
parm:           fw_restart:restart firmware in case of error (default true) (bool)
parm:           nvm_file:NVM file name (charp)
parm:           uapsd_disable:disable U-APSD functionality bitmap 1: BSS 2: P2P Client (default: 3) (uint)
parm:           enable_ini:0:disable, 1-15:FW_DBG_PRESET Values, 16:enabled without preset value defined,Debug INI TLV FW debug infrastructure (default: 16)
parm:           bt_coex_active:enable wifi/bt co-exist (default: enable) (bool)
parm:           led_mode:0=system default, 1=On(RF On)/Off(RF Off), 2=blinking, 3=Off (default: 0) (int)
parm:           power_save:enable WiFi power management (default: disable) (bool)
parm:           power_level:default power save level (range from 1 - 5, default: 1) (int)
parm:           disable_11ac:Disable VHT capabilities (default: false) (bool)
parm:           remove_when_gone:Remove dev from PCIe bus if it is deemed inaccessible (default: false) (bool)
parm:           disable_11ax:Disable HE capabilities (default: false) (bool)
parm:           disable_11be:Disable EHT capabilities (default: false) (bool
```

***Du kan også resette driveren via modprobe***
```console
$ sudo modprobe -r iwlwifi
$ sudo modprobe iwlwifi
$ sudo dmesg
```

***Sjekker verdier i parameterlisten ***

```console
$ grep [[:alnum:]] /sys/module/iwlwifi/parameters/* | cut -d '/' -f6
 grep: /sys/module/iwlwifi/parameters/enable_ini11n_disable:0
amsdu_size:0
bt_coex_active:Y
disable_11ac:N
disable_11ax:N
disable_11be:N
: Operation not permitted
fw_restart:Y
led_mode:0
nvm_file:(null)
power_level:0
power_save:N
remove_when_gone:N
swcrypto:0
uapsd_disable:3
$
```


NetworkManager WiFi Power Saving
================================

From gist: [jcberthon/networkmanager-wifi-powersave.md](https://gist.github.com/jcberthon/ea8cfe278998968ba7c5a95344bc8b55)

NetworkManager supports WiFi powersaving but the function is rather undocumented. Standard value in Pop_OS 22.04 was `wifi.powersave = 3`
I changed it to 2.

```console
$ ls /etc/NetworkManager/conf.d/
10-globally-managed-devices.conf  10-ubuntu-fan.conf  default-wifi-powersave-on.conf
$ cat /etc/NetworkManager/conf.d/default-wifi-powersave-on.conf
[connection]
wifi.powersave = 3
$ 
```


From the source code: wifi.powersave can have the following value:

  - NM_SETTING_WIRELESS_POWERSAVE_DEFAULT (0): use the default value
  - NM_SETTING_WIRELESS_POWERSAVE_IGNORE  (1): don't touch existing setting
  - NM_SETTING_WIRELESS_POWERSAVE_DISABLE (2): disable powersave
  - NM_SETTING_WIRELESS_POWERSAVE_ENABLE  (3): enable powersave

Then I propose 2 files, only one of them needs to be put under `/etc/NetworkManager/conf.d/`.  
One is forcing to disable powersaving, while the other one enable it.

Once you have put the file in the right folder, simply restart NetworkManager:

    sudo systemctl restart NetworkManager


Intel har skrevet en utility for å håndtere Wireless
====================================================

Sjekk [How to manage wireless connections using iwd on Linux](https://linuxconfig.org/how-to-manage-wireless-connections-using-iwd-on-linux)

Installasjon
```console
$ sudo apt install iwd
```
Har installert iwd - som en konsekvens blir ikke **wlan0** endret til **wlp7s0** og jeg måtte reconnecte Wifi

The iwd package provides:

* The `iwd` daemon
* The `iwctl` command line utility
* The `iwmon` monitoring tool


Note to self: *Skal avinstallere denne*
