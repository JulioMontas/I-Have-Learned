# Docker List


### Emulator JS
[Emulatorjs](https://github.com/linuxserver/emulatorjs) - In browser web based emulation portable to nearly any device for many retro consoles. A mix of emulators is used between Libretro and EmulatorJS.


`-v /path/to/nes/roms:/data/nes/roms`

The folder names are:
```
$ touch 3do arcade atari2600 atari7800 colecovision doom gb gba gbc jaguar lynx msx n64 nds nes ngp odyssey2 pce psx sega32x segaCD segaGG segaMD segaMS segaSaturn segaSG snes vb vectrex ws
```

#### docker-compose (recommended, [click here for more info](https://docs.linuxserver.io/general/docker-compose))

```yaml
---
version: "2.1"
services:
  emulatorjs:
    image: lscr.io/linuxserver/emulatorjs:latest
    container_name: emulatorjs
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/London
      - SUBFOLDER=/ #optional
    volumes:
      - /path/to/config:/config
      - /path/to/data:/data
    ports:
      - 3000:3000
      - 80:80
      - 4001:4001 #optional
    restart: unless-stopped
```

#### docker cli ([click here for more info](https://docs.docker.com/engine/reference/commandline/cli/))

```bash
docker run -d \
  --name=emulatorjs \
  -e PUID=1000 \
  -e PGID=1000 \
  -e TZ=Europe/London \
  -e SUBFOLDER=/ `#optional` \
  -p 3000:3000 \
  -p 80:80 \
  -p 4001:4001 `#optional` \
  -v /path/to/config:/config \
  -v /path/to/data:/data \
  --restart unless-stopped \
  lscr.io/linuxserver/emulatorjs:latest
```

#### Parameters

Container images are configured using parameters passed at runtime (such as those above). These parameters are separated by a colon and indicate `<external>:<internal>` respectively. For example, `-p 8080:80` would expose port `80` from inside the container to be accessible from the host's IP on port `8080` outside the container.

| Parameter | Function |
| :----: | --- |
| `-p 3000` | Rom/artwork management interface, used to generate/manage config files and download artwork |
| `-p 80` | Emulation frontend containing static web files used to browse and launch games |
| `-p 4001` | IPFS peering port, if you want to participate in the P2P network to distribute frontend artwork please forward this to the Internet |
| `-e PUID=1000` | for UserID - see below for explanation |
| `-e PGID=1000` | for GroupID - see below for explanation |
| `-e TZ=Europe/London` | Specify a timezone to use EG Europe/London |
| `-e SUBFOLDER=/` | Specify a subfolder for reverse proxies IE '/FOLDER/' |
| `-v /config` | Path to store user profiles |
| `-v /data` | Path to store roms/artwork |
