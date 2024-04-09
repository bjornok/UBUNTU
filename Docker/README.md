# TIPS - Docker / Podman


`docker`og `podman`bruker de samme container og image spesifikasjonene.  Her legges det opp informasjon og tips til situasjoner jeg har vært oppe i.

### MicroOS - docker kompatibel cli

Med `sudo transactional-update pkg install docker-podman` får man docker cli i podman.


### Portainer

Portainer er et behagelig web-basert grensesnitt for å vedlikeholde og ha oversikt over docker kjøremiljøet.

**Oppdatere containere til nyeste image**
- Velg container i grensesnittet
- `Stop` container
- `Recreate` - du får da mulighet til [Re-pull image]

Portainer er bygget slik at den oppdaterer seg selv inne i imaget - ikke behov for å bruke docker til det :-)

### Unifi controller

Veiledningen dekker trinn-for-trinn-instruksjoner for å sette opp UniFi Controller, som er et administrasjonsverktøy for Ubiquiti-nettverksenheter, i en Docker-container.
Referanse: [Computing for Geeks - How To Run UniFi Controller in Docker Container](https://computingforgeeks.com/how-to-run-unifi-controller-in-docker-container/#google_vignette)

Har opprettet en folder `/unifi_data` som holder konfigurasjonen.
I tillegg - når man bruker en Docker container på en maskin som kan skifte IP-adresse - og vi ikke har name-resolution - så må man under "System/Advanced/Inform Host:" - oppgi IP-adresse på vertsmaskinen til containeren.  Ellers vil AP-punktets status bli stående som "Adopting"-

```console
docker run -d \
  --name=unifi-controller \
  -e PUID=1000 \
  -e PGID=1000 \
  -e MEM_LIMIT=1024 `#optional` \
  -e MEM_STARTUP=1024 `#optional` \
  -p 8443:8443 \
  -p 3478:3478/udp \
  -p 10001:10001/udp \
  -p 8080:8080 \
  -p 1900:1900/udp `#optional` \
  -p 8843:8843 `#optional` \
  -p 8880:8880 `#optional` \
  -p 6789:6789 `#optional` \
  -p 5514:5514/udp `#optional` \
  -v /unifi_data/config:/config \
  --restart unless-stopped \
  lscr.io/linuxserver/unifi-controller:latest
```
