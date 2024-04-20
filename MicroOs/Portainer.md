# Using Podman med Portainer

Oppdatert 2024-04-20: Istedet for Portainer - bruk Podman-Desktop (Portainer er best mot Docker)

Portainer er et kraftig og enkelt visuelt verktøy for å vedlikehlde containere.

Utdrag er hentet fra denne artikkelen [How to deploy the Portainer container manager with Podman](https://www.techrepublic.com/article/deploy-portainer-with-podman/)

Portainer was originally built for Docker, and with the rootless nature of Podman, Portainer had some serious issues and wouldn’t deploy. However, 
Podman can be run as either root or non-root, so it is actually possible to deploy the Portainer GUI for the Podman runtime.

NB! fungerer ikke med MicroOS - finner ikke portainer i registry
```console
bjorn@localhost:~> docker pull portainer/portainer-ce
Emulate Docker CLI using podman. Create /etc/containers/nodocker to quiet msg.
✔ registry.opensuse.org/portainer/portainer-ce:latest
Trying to pull registry.opensuse.org/portainer/portainer-ce:latest...
Error: initializing source docker://registry.opensuse.org/portainer/portainer-ce:latest: reading manifest latest in registry.opensuse.org/portainer/portainer-ce: name unknown

bjorn@localhost:~> podman pull portainer/portainer-ce:latest
✔ docker.io/portainer/portainer-ce:latest
Trying to pull docker.io/portainer/portainer-ce:latest...
Getting image source signatures
Copying blob 379538b6d68e done   | 
Copying blob c1cad9f5200f done   | 
Copying blob 4ea3e2c3a39b done   | 
Copying blob 5171176db7f2 done   | 
Copying blob 52e9438966a5 done   | 
Copying blob 43d4775415ac done   | 
Copying blob 22eab514564f done   | 
Copying blob 962b9fa821a2 done   | 
Copying blob c153fefda5ce done   | 
Copying blob bed990c4615b done   | 
Copying blob 4f4fb700ef54 done   | 
Copying config 1a0fb356ea done   | 
Writing manifest to image destination
1a0fb356ea35830bad2a93aea5a72c88385b3505490cf035a575122bbe094a81


bjorn@localhost:~> docker run hello-world
Emulate Docker CLI using podman. Create /etc/containers/nodocker to quiet msg.
Resolved "hello-world" as an alias (/etc/containers/registries.conf.d/000-shortnames.conf)
Trying to pull quay.io/podman/hello:latest...
Getting image source signatures
Copying blob 1aa8a143ab41 done   | 
Copying config faee435989 done   | 
Writing manifest to image destination
!... Hello Podman World ...!

         .--"--.           
       / -     - \         
      / (O)   (O) \        
   ~~~| -=(,Y,)=- |         
    .---. /`  \   |~~      
 ~/  o  o \~~~~.----. ~~   
  | =(X)= |~  / (O (O) \   
   ~~~~~~~  ~| =(Y_)=-  |   
  ~~~~    ~~~|   U      |~~ 


```


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
