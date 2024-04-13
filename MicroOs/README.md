# Oppsett av OpenSUSE MicroOS Desktop m Gnome (Tumbleweed rolling release)

## Introduction

```console
$ sudo transactional-update dup
$ reboot
---
$ sudo transactional-update pkg install systemd-zram-service
$ reboot
---
$ free -h
$ sudo systemctl enable --now zramswap.service
$ free -h # Should show swap enabled
---
$ sudo transactional-update pkg install neofetch htop
$ reboot
```
 
## Useful applications (from Flatpak)

Podman is a powerful utility - and has similar functionality as Docker - runs Docker images and is a good way to add addiditionally functionality
like a "Unifi controller" etc...

- Podman-Desktop
- Document Scanner
- Document Viewer (needed to view pdf-files etc...)
- Microsoft Edge
- Celluloid
- OnlyOffice
- Flatseal (handling rights - give Firefox print rights)


## Useful extensions to Gnome

Using Extension Manager - tou will be able to customiuze the Desktop. Here are my favorites.

- Bing Wallpaper
- IP Finder
- Apps Menu
- Launch new instance
- Workspace indicator


## Using Distrobox

Distrobox is an utility to run mutiple container images integrated with the Host. See [github.com: Distrobox](https://github.com/89luca89/distrobox/tree/main)

### Enabling a OpenSuse Leap distrobox with systemd and install SQL Server

You will be able to install SQL Server another servies that require SystemD/Init inside the container to work. 
See [Using init system inside a distrobox](https://github.com/89luca89/distrobox/blob/main/docs/useful_tips.md#using-init-system-inside-a-distrobox)

See [Quickstart: Install SQL Server and create a database on SUSE Linux Enterprise Server](https://learn.microsoft.com/en-us/sql/linux/quickstart-install-connect-suse?view=sql-server-linux-ver15)

```shell
$ distrobox create -i registry.opensuse.org/opensuse/leap:latest --init --additional-packages "systemd" -n leapsql
$ distrobox enter leapsql
----
$$ sudo zypper addrepo -fc https://packages.microsoft.com/config/sles/15/mssql-server-2019.repo
$$ sudo zypper --gpg-auto-import-keys refresh
$$ sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
$$ sudo zypper install -y mssql-server
----
$$ sudo /opt/mssql/bin/mssql-conf setup
----
$$ sudo systemctl status mssql-server
---
$$ exit
$
```
