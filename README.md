# UBUNTU

### Setup MS repositories in UbUntu
```
## Setup
curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
sudo install -o root -g root -m 644 microsoft.gpg /etc/apt/trusted.gpg.d/
sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/edge stable main" > /etc/apt/sources.list.d/microsoft-edge-dev.list'
sudo rm microsoft.gpg
## Install
sudo apt update
sudo apt install microsoft-edge-dev
```


## Find my SHELL version
I need to use the correct shell, running SAP DP Agent.  It works well in SUSE Linux, but not in Ubuntu


***UPDATE***  just edited: *agentcli.sh*, *dpagent_env.sh* and *dpagent_servicedaemon.sh* - changed #/bin/sh to #/bin/bash using NANO

* $SHELL --version
```
user2@dbserver2:/usr/sap/dataprovagent/bin$ $SHELL --version
GNU bash, version 4.4.20(1)-release (x86_64-pc-linux-gnu)
Copyright (C) 2016 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>

This is free software; you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
```
