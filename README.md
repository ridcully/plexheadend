# plexheadend

Proxy requests between PlexDVR and TVHeadend. This is a fork of github.com/wrboyce/plexheadend . All credits go to Will Boyce.

## Installation

### Build from Source

Download and build the project and its dependencies with the standard Go tooling, `go get github.com/ridcully/plexheadend`.

To cross compile for your target machine:

```GOOS="linux" GOARCH="arm64" go build```

## Usage

All configuration options can be specified as either a commandline parameter or an environment variable.

| Name               | Commandline                 | Environment                   |
|--------------------|-----------------------------|-------------------------------|
| Device ID          | `--device-id` `-i`          | `PLEXHEADEND_DEVICE_ID`       |
| Name               | `--name` `-n`               | `PLEXHEADEND_NAME`            |
| Proxy Bind         | `--proxy-bind` `-b`         | `PLEXHEADEND_PROXY_BIND`      |
| Proxy Hostname     | `--proxy-hostname` `-H`     | `PLEXHEADEND_PROXY_HOSTNAME`  |
| Proxy Listen       | `--proxy-listen` `-l`       | `PLEXHEADEND_PROXY_LISTEN`    |
| Filter Tag         | `--tag` `-f`                | `PLEXHEADEND_TAG`             |
| Tuners             | `--tuners` `-t`             | `PLEXHEADEND_TUNERS`          |
| TVHeadend Host     | `--tvh-host` `-h`           | `PLEXHEADEND_TVH_HOST`        |
| TVHeadend Pass     | `--tvh-pass` `-P`           | `PLEXHEADEND_TVH_PASS`        |
| TVHeadend Port     | `--tvh-port` `-p`           | `PLEXHEADEND_TVH_PORT`        |
| TVHeadend User     | `--tvh-user` `-u`           | `PLEXHEADEND_TVH_USER`        |

```
Usage of plexheadend:
  -i, --device-id string        Device ID reported to Plex (default "1")
  -n, --name string             Friendly name reported to Plex (default "plexHeadend")
  -b, --proxy-bind string       Bind address (default all)
  -H, --proxy-hostname string   Hostname reported to Plex (default "localhost")
  -l, --proxy-listen string     Listen port (default "80")
  -f, --tag string              TVHeadend tag to filter reported channels (default none)
  -t, --tuners int              Number of Tuners reported to Plex (default 1)
  -h, --tvh-host string         TVHeadend Host (default "localhost")
  -P, --tvh-pass string         TVHeadend Password
  -p, --tvh-port string         TVHeadend Port (default "9981")
  -u, --tvh-user string         TVHeadend Username
```

Please note, if your Plex Media Server ist not on the same host, as tvHeadend and plexhead, than you need to set the --proxy-hostname and -tvh-host parameter to IPs reachable by the Plex Media Server.

### Example `docker-compose` Usage

```yaml
services:
    tvheadend:
        image: linuxserver/tvheadend
        container_name: tvheadend
        environment:
            - VERSION=latest
            - TZ=UTC
        ports:
            - 9981:9981
            - 9982:9982
        restart: unless-stopped
        
    plexheadend:
        image: wrboyce/plexheadend
        container_name: plexheadend
        environment:
            - PLEXHEADEND_TVH_HOST=tvheadend
            - PLEXHEADEND_PROXY_HOSTNAME=plexheadend
        restart: unless-stopped
        
    plex:
        image: linuxserver/plex
        container_name: plex
        environment:
            - VERSION=latest
            - TZ=UTC
        ports:
            - 32400:32400
            - 32400:32400/udp
        restart: unless-stopped
```