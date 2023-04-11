This usually fixes Flatpak issues and slims it down:

```
sudo flatpak repair
flatpak repair --user
flatpak remove --unused
flatpak update
```

PS: never user sudo with Flatpak on Pop!_OS unless you are directed to do so.
