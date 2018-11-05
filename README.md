# docker-plexdrive

Docker image for running [plexdrv](https://github.com/kilhaasi/plexdrv)


## Usage

```yaml
version: '2'

services:
  plexdrive:
    container_name: plexdrv
    image: kilhaasi/plexdrv
    restart: always
    network_mode: "bridge"
    volumes:
      - /root/.plexdrive:/config
      - /mnt/test:/data:shared
    privileged: true
    devices:
      - /dev/fuse
    cap_add:
      - MKNOD
      - SYS_ADMIN
    environment:
      - PUID=0
      - PGID=0
      - RUN_OPTS=<additional arguments for running plexdrive>
```

First, up and run your container as above. It will be waiting for two plexdrive configuration files to be ready. You can create those files using built-in script by

```bash
docker-compose exec <service_name> plexdrive_setup
```

Once you finish typing your API token, shell stops responding. No worries, it is expected. Simply escape by ```Ctrl+C```, and go to ```/config```. You will find two json files generated. Container running in background proceeds to execute mounting command for plexdrive. You can now access google drive contents via volume-mapped ```/your/mounting/point```.
