# Setup SAP DP AGent - Linux

As of SLES 15.6, libnsl.so.1 is no longer delived as base package. See SAP Note [3310210 - Linux: Installation with SDBINST requires library libnsl.so.1](https://me.sap.com/notes/3310210/E)

See SAP Help: [Install the Agent on Linux](https://help.sap.com/docs/HANA_SMART_DATA_INTEGRATION/f4a54d019f624c799cbc276a798e588b/a0ad8b579e5c4c4b9149fa1ea46fe67e.html)

### Linux version

* Use OpenSuse Leap or OpenSuse Tumbleweed

```console
sudo zypper install libnsl1
cd ./INSTALL/HANA_DP_AGENT_20_LIN_X86_64
./hdbinst
```

* Goto dataprovagent/bin

```console
./agencli.sh --configAGent
```

