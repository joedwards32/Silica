[![Docker Image CI](https://github.com/joedwards32/Silica/actions/workflows/docker-image.yml/badge.svg?branch=main)](https://github.com/joedwards32/Silica/actions/workflows/docker-image.yml) [![Docker Build and Publish](https://github.com/joedwards32/Silica/actions/workflows/docker-publish.yml/badge.svg)](https://github.com/joedwards32/Silica/actions/workflows/docker-publish.yml)

# What is Silica?

Silica is a crossover of FPS and RTS, where you take part in epic photorealistic battles either as humans or aliens. Lead as the commander, or join the assault from the ground - the choice is yours! 
This Docker image contains the dedicated server for the game.

>  [Silica](https://store.steampowered.com/app/1494420/Silica/)

<img src="https://cdn.cloudflare.steamstatic.com/steam/apps/1494420/header.jpg?t=1696011820" alt="logo" width="300"/></img>

# How to use this image
## Hosting a simple game server

Running using Docker:
```console
$ docker run -d --name=silica -p 27015:27015/tcp -p 27016:27016/tcp joedwards32/Silica
```

Running using a bind mount for data persistence on container recreation:
```console
$ mkdir -p $(pwd)/silica-data
$ chown 1000:1000 $(pwd)/silica-data # Makes sure the directory is writeable by the unprivileged container user with uid 1000, known as steam
$ docker run -d --name=silica -v $(pwd)/cs2-data:/home/steam/silica-dedicated/ -p 27015:27015/tcp -p 27016:27016/tcp joedwards32/silica
```

or using docker-compose, see [examples](https://github.com/joedwards32/Silica/blob/main/examples/docker-compose.yml):
```console
# Remember to update passwords and SRCDS_TOKEN in your compose file
$ docker compose --file examples/docker-compose.yml up -d silica
```

You must have at least **40GB** of free disk space! See [System Requirements](./#system-requirements).

**The container will automatically update the game on startup, so if there is a game update just restart the container.**

# Configuration

## System Requirements

Minimum system requirements are:

* 4 CPUs
* 4GiB RAM
* 40GB of disk space for the container or mounted as a persistent volume on `/home/steam/silica-dedicated/`

## Environment Variables
Feel free to overwrite these environment variables, using -e (--env): 

### Server Configuration

```dockerfile
```

# Customizing this Container

## Debug Logging

If you want to increase the verbosity of log output set the `DEBUG` environment variable:

```dockerfile
DEBUG=0                    (0=none, 1=steamcmd, 2=cs2, 3=all)
```

## Validating Game Files

If you break the game through your customisations and want steamcmd to validate and redownload then set the `STEAMAPPVALIDATE` environment variable to `1`:

```dockerfile
STEAMAPPVALIDATE=0          (0=skip validation, 1=validate game files)
```

# Credits

This container leans heavily on the work of [CM2Walki](https://github.com/CM2Walki/), especially his [SteamCMD](https://github.com/CM2Walki/steamcmd) container image. GG!

The work of [Casraw](https://github.com/Casraw/silica-docker-server) was also instructive in crafting the server configuration file.
