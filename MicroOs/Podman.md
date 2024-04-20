# Erfaringer med Podman

Podman er standard container-grensesnitt for MicroOS, og er kompatibel med docker images.
Podman CLI er veldig lik Docker i syntax. Største forskjellen er at Docker kjører pr Default som root,
mens podman er rootless.

## Tips

- mapping av volumer `-v /path/to/host/volume/or/file:/path/in/container:Z`.
- Z tillater non-root container UID tilganger - ellers `Access denied`

 ## Dokumentasjon
 - [Github -containers/podman-desktop ](https://github.com/containers/podman-desktop)
