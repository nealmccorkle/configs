# Portainer Media Stack

Info on portainer at (portainer.io)[https://www.portainer.io]. The below uses portainer with docker-compose. With some additional modifcation to the docker-compose.yml to define default network it would be possible to use without portainer.

1. Setup *media* user if not previously done
    - Run `adduser --system --no-create-home --uid 2001 --group media`
1. Setup config directory if not previously done
    - Run `mkdir /config`
    - Run `chown media /config`
    - Run `chgrp media /config`
1. Setup data directory if not previously done. This is where all downloads, shows, and moviews will live.
    - Run `mkdir -p /var/data/shows`
    - Run `mkdir -p /var/data/movies`
    - Run `mkdir -p /var/data/sabnzbd`
    - Run `chown -R media /var/data`
    - Run `chgrp -R media /var/data`
1. Setup docker networks if not previously done
    - Run `docker network create media_default`
    - Run `docker network create utils_default`
1. In the portaniner gui, create a new stack named *media*
    - The stack name *media* will automatically use the *media_default* network
1. Add the provided docker-compose.yml
1. Adjust values in swag and all volumes as needed

# About Volumes
All apps store configs in the `/config` directory. All downloaded data, movies, and shows will live in `/var/data`. Both of these paths can be adjusted by editing the `docker-compose.yml` file in all the needed places. (I should use vars!)

Containers that will share access to the `/var/data` directory:
- Plex writes reads from `shows` and `movies`
- Sonarr writes and reads to `shows` and `sabnzbd`
- Radarr writes and reads to `movies` and `sabnzbd`
- Sabnzbd writes and reads to `sabnzbd`
- Bazarr writes and reads to `shows` and `movies`

# File Owner and Group
The config and data dirs must be owned by the UID 2001 and GID 2001. This can be changed accordingly in the `docker-compose.yml`. file (I should use vars!) The containers that need access to these dirs will ensure that the `PUID` and `PGID` docker env vars are used to avoid owner and group read/write issues. See [Understanding PUID and PGID](https://docs.linuxserver.io/general/understanding-puid-and-pgid) for more info.

# External Access and Homepage
The *Heimdall* image provides and easy to use home page for the media stack. You can optionally expose the home page with password protection using the *SWAG* proxy server and port forwarding on your router. If you do not need a home page and do not plan to expose these outside your network, remove the *Heimdall* and *SWAG* containers from the `docker-compose.yml`.

# Container Docs
Links below provide applicaiton docs and docker container setup docs. Images are provided by the most excelent [linuxserver.io](https://www.linuxserver.io) project. See the [fleet](https://fleet.linuxserver.io).

## swag
Docs: https://docs.linuxserver.io/general/swag
Docker: https://hub.docker.com/r/linuxserver/swag

## plex
Docs: https://support.plex.tv/articles/
Docker: https://hub.docker.com/r/linuxserver/plex

## heimdall
Docs: https://heimdall.site
Docker: https://hub.docker.com/r/linuxserver/heimdall

## ombi
Docs: https://ombi.io
Docker: https://hub.docker.com/r/linuxserver/ombi

## tautulli
Docs: https://tautulli.com/#heading
Docker: https://hub.docker.com/r/linuxserver/tautulli

## sabnzbd
Docs: https://sabnzbd.org/
Docker: https://hub.docker.com/r/linuxserver/sabnzbd

## nzbhydra2
Docs: https://github.com/theotherp/nzbhydra2/
Docker: https://hub.docker.com/r/linuxserver/nzbhydra2

## sonarr
Docs: https://sonarr.tv
Docker: https://hub.docker.com/r/linuxserver/sonarr

## radarr
Docs: https://radarr.video
Docker: https://hub.docker.com/r/linuxserver/radarr

## bazarr
Docs: https://www.bazarr.media
Docker: https://hub.docker.com/r/linuxserver/bazarr
