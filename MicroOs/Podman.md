# Erfaringer med Podman

Podman er standard container-grensesnitt for MicroOS, og er kompatibel med docker images.
Podman CLI er veldig lik Docker i syntax. Største forskjellen er at Docker kjører pr Default som root,
mens podman er rootless.

## Tips

- mapping av volumer `-v /path/to/host/volume/or/file:/path/in/container:Z`.
- Z tillater non-root container UID tilganger - ellers `Access denied`

 ## Dokumentasjon
 - [Podman troubleshooting](https://github.com/containers/podman/blob/main/troubleshooting.md#33-container-creates-a-file-that-is-not-owned-by-the-users-regular-uid)
 - [Using volumes with rootless podman, explained](https://www.tutorialworks.com/podman-rootless-volumes/)
 - [Containers lifecycle explained 'docker'](https://k21academy.com/docker-kubernetes/docker-container-lifecycle-management/)
 - [Github -containers/podman-desktop ](https://github.com/containers/podman-desktop)
