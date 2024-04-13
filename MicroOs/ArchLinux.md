# Arch Linux spesifics (distrobox)


## Install yay

yay is a utility to install from AUR. Use `yay` without sudo

```console
$ sudo pacman -S --needed base-devel git
$ git clone https://aur.archlinux.org/yay.git
$ cd yay         # step into cloned directory
$ makepkg -si    # build and install yay
```

## Updating and installing

Use `sudo -i` or remember to run pacman with sudo. 


```console
pacman -Ss firefox # search for firefox package in archlinux package repos
sudo pacman -S firefox # install firefox
sudo pacman -Rns firefox # uninstall firefom and it's dependencies from the system
sudo pacman -Syyyu # Update package list and upgrade if available

yay -Ss popcorntime # find popcorntime package in archlinux package repos including AUR
yay -S popcorntime # install popcorntime from AUR
yay -Rns popcorntime # uninstall popcorntime and it's dependencies from the system
yay -Syyyu # Update package list and upgrade if available including AUR packages
```

## Reset keyring

Remove or reset all the keys installed in your system by removing the `/etc/pacman.d/gnupg` directory (as root) and by 
rerunning `pacman-key --init` followed by `pacman-key --populate` to re-add the default keys. 

