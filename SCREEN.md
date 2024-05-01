# Screen, X and NVIDIA insight



## Screen went blank after 20 seconds pause - turned off monitor

In Pop-OS, suddenly monitor shut off 20 seconds after no activity. Power settings was 5 minute before shut off.  Reason and fix found Googling.

[Reddit - Screen going black after 30 seconds](https://www.reddit.com/r/pop_os/comments/eln8bp/screen_going_black_after_30_seconds/?utm_source=share&utm_medium=web2x&context=3)

```console
$ xset -dpms
```

A bit more here: https://github.com/pop-os/default-settings/pull/123/files
and here https://github.com/pop-os/pop/issues/2104

