# Using Podman med Portainer

Portainer er et kraftig og enkelt visuelt verktøy for å vedlikehlde containere.

Utdrag er hentet fra denne artikkelen [How to deploy the Portainer container manager with Podman](https://www.techrepublic.com/article/deploy-portainer-with-podman/)

Portainer was originally built for Docker, and with the rootless nature of Podman, Portainer had some serious issues and wouldn’t deploy. However, 
Podman can be run as either root or non-root, so it is actually possible to deploy the Portainer GUI for the Podman runtime.



```console
sudo systemctl enable --now podman.socket
sudo podman run -d -p 9443:9443 --privileged -v /run/podman/podman.sock:/var/run/docker.sock:Z portainer/portainer-ce
```
Connect to : (https://localhost:9443)

----

### Installing docker compatible CLI for podman

```console
sudo transactional-update pkg install docker-podman
sudo reboot
```
