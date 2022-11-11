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

### Change /bin/sh to use /bin/bash and not /bin/dash.
SAP DP Agent is run and configured using shell scripts.  These requires to invike classic 'sh', however on Ubuntu it points to /bin/dash which is not compatible. We have two options, a) to edit all shell scripts in catalog /usr/sap/dataprovagent and change from #!/bin/sh to #!/bin/bash or b) create a symlink to bash instead of dash.

```
user2@dbserver2:~$ cd /bin
user2@dbserver2:/bin$ ls -la *sh
-rwxr-xr-x 1 root root 1113504 Apr 18  2022 bash
-rwxr-xr-x 1 root root  170456 Sep 21  2019 bsd-csh
lrwxrwxrwx 1 root root      21 Jan 28  2021 csh -> /etc/alternatives/csh
-rwxr-xr-x 1 root root  121432 Jan 25  2018 dash
lrwxrwxrwx 1 root root       4 Apr 18  2022 rbash -> bash
```
lrwxrwxrwx 1 root root       4 Aug  6  2020 **sh -> dash**
```
lrwxrwxrwx 1 root root       7 Nov 24  2021 static-sh -> busybox
user2@dbserver2:/bin$ sudo ln -sf bash sh
```

### Find my SHELL version
I need to use the correct shell, running SAP DP Agent.  It works well in SUSE Linux, but not in Ubuntu


***UPDATE***  just edited: *agentcli.sh*, *dpagent_env.sh* and *dpagent_servicedaemon.sh* - changed #/bin/sh to #/bin/bash using NANO

`user2@dbserver2:/usr/sap/dataprovagent/bin$ $SHELL --version`

```
user2@dbserver2:/usr/sap/dataprovagent/bin$ $SHELL --version
GNU bash, version 4.4.20(1)-release (x86_64-pc-linux-gnu)
Copyright (C) 2016 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>

This is free software; you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
```
