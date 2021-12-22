# Shairport Sync Docker Image

Available at: https://hub.docker.com/r/mikebrady/shairport-sync

Please note if you want the development version of the image including Airplay 2 support, please pull the image with the ```unstable development``` tag using the following command:
```
docker pull mikebrady/shairport-sync:unstable-development
```

When using the below commands, you should replace mikebrady/shairport-sync with mikbrady/shairport-sync:unstable-development.

## Example Docker Compose File
See the `docker-compose.yaml` file in this folder for an example.

## Docker Run

```
$ docker run -d --restart unless-stopped --net host --device /dev/snd \
    mikebrady/shairport-sync
```

### Options

Any options you add to the command above will be passed to Shairport Sync. Here is an example:
```
$ docker run -d --restart unless-stopped --net host --device /dev/snd \
    mikebrady/shairport-sync -a DenSystem -- -d hw:0 -c PCM
```
This will sent audio to alsa hardware device `hw:0` and make use of the that device's mixer control called `PCM`. The service will be visible as `DenSystem` on the network.

## Configuration File

To get access to the full range of configuration options, pass the configuration file to `/etc/shairport-sync.conf` in the container using the `-v` option or docker compose.

## Building
### Build Example (for arm7 devices)

```
docker buildx build --platform linux/arm/v7 -f ./docker/Dockerfile --build-arg SHAIRPORT_SYNC_BRANCH=development --build-arg NQPTP_BRANCH=development -t shairport-sync:unstable-development .
```

`SHAIRPORT_SYNC_BRANCH` and `NQPTP_BRANCH` are required to ensure the image is built using the expected branch.

### AirPlay 1 Only

The AirPlay1 only dockerfile is in the `airplay1` folder. This also includes the `start.sh` script used by the container. Please note that the AirPlay 1 image built via the AirPlay 2 branch has missing functionality, e.g. it does not work with multiple instances on the same hardware; does not support iTunes for Windows etc.

### GitHub Action Builds

Requires the following secrets to be set in the repo:
- `DOCKER_REGISTRY` - docker.io if using Docker Hub, else set to your registry URL.
- `DOCKER_REGISTRY_TOKEN` - Access token for your registry.
- `DOCKER_REGISTRY_USER` - Login user for your registry.
- `DOCKER_IMAGE_NAME` - The name of the image, for example `your-registry.com/shairport-sync` or just `your-username/shairport-sync` if using Docker Hub.
