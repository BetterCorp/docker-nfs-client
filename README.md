[![Docker Pulls](https://badgen.net/docker/pulls/betterweb/nfs-client?icon=docker&label=pulls&cache=600)](https://hub.docker.com/r/betterweb/nfs-client/tags) [![Docker Image Size](https://badgen.net/docker/size/betterweb/nfs-client/latest?icon=docker&label=image%20size&cache=600)](https://hub.docker.com/r/betterweb/nfs-client/tags) [![Docker build](https://img.shields.io/badge/automated-automated?style=flat&logo=docker&logoColor=blue&label=build&color=green&cacheSeconds=600)](https://hub.docker.com/r/betterweb/nfs-client/tags) [![Docker Stars](https://badgen.net/docker/stars/betterweb/nfs-client?icon=docker&label=stars&color=green&cache=600)](https://hub.docker.com/r/betterweb/nfs-client) [![Github Stars](https://img.shields.io/github/stars/BetterCorp/docker-nfs-client?label=stars&logo=github&color=green&style=flat&cacheSeconds=600)](https://github.com/BetterCorp/docker-nfs-client) [![Github forks](https://img.shields.io/github/forks/BetterCorp/docker-nfs-client?logo=github&style=flat&cacheSeconds=600)](https://github.com/BetterCorp/docker-nfs-client/fork) [![Github open issues](https://img.shields.io/github/issues-raw/BetterCorp/docker-nfs-client?logo=github&color=yellow&cacheSeconds=600)](https://github.com/BetterCorp/docker-nfs-client/issues) [![Github closed issues](https://img.shields.io/github/issues-closed-raw/BetterCorp/docker-nfs-client?logo=github&color=green&cacheSeconds=600)](https://github.com/BetterCorp/docker-nfs-client/issues?q=is%3Aissue+is%3Aclosed) [![GitHub license](https://img.shields.io/github/license/BetterCorp/docker-nfs-client)](https://github.com/BetterCorp/docker-nfs-client/blob/master/LICENSE)

# A small docker NFS client (multi-arch). Perfect for enabling whatever to NFS, compatible with databases (BetterCorp> betterweb/nfs-client). 

# This specific docker nfs image is for running actions on an nfs endpoint - published v3 only


This is a Docker multi-arch image for a light NFS client ([![Docker Image Size](https://badgen.net/docker/size/betterweb/nfs-client/latest?icon=docker&label=compressed)](https://hub.docker.com/r/betterweb/nfs-client/tags)) compatible with database usage.

We provide 2 tags for this image
- **latest**: which is the historical tag that is set for **NFS 3** by default but the ENVs enable you to change easily to **NFS 4** or any other filesystem types supported.
- **v4**: which is set by default to **NFS 4** as a ready to go nfs4 client.



## Docker image

pre-build as multi-arch from Docker hub with "automated build" option.

image name **betterweb/nfs-client**

`docker pull betterweb/nfs-client`

Docker hub repository: https://hub.docker.com/r/betterweb/nfs-client/

### Image TAGS

- **betterweb/nfs-client:latest** which is the default image when using betterweb/nfs-client but is set with nfs3 (nfs4 and other supported fstypes available by setting ENVs)
- **betterweb/nfs-client:v4** which is ready to go as nfs4 client (ENVs still permit to change the default fstype).

[![DockerHub Badge](https://dockeri.co/image/betterweb/nfs-client)](https://hub.docker.com/r/betterweb/nfs-client)

## Origin
Based on https://github.com/flaccid/docker-nfs-client
Extended from https://github.com/Angatar/docker-nfs-client

### Initial evolutions
*The image is now built from the original Alpine with automated build.
Default NFS type modified to NFS3 for local IT requirements. 
The entry script was adapted to be compatible with using this NFS client with database (mariadb, mysql...) and to permit running the container without setting the SERVER and SHARE env parameters, simply to share on the host's network the NFS client capabilities for mounting any NFS shared path on the host (quite useful with small os)* 

### Last evolutions

**The container is now made available as multi-arch image**, build from Docker Hub nodes dedicated to automated builds.
This multi-arch image will fit most of architectures:

- linux/amd64
- linux/386
- linux/arm/v6
- linux/arm/v7
- linux/arm64/v8
- linux/ppc64le
- linux/s390x 

As this container is quite widely used [![Docker Pulls](https://badgen.net/docker/pulls/betterweb/nfs-client?icon=docker&label=pulls)](https://hub.docker.com/r/betterweb/nfs-client/tags), **we'll remain with NFS3 as default FSTYPE on the historical tag "latest"** to avoid to create any anoying breaking issue, but the ENVIRONMENT variables enable you to change easily to NFS 4 or any other filesystem types supported: The filesystem types which are currently supported include adfs, affs, autofs, cifs, coda, coherent, cramfs, debugfs, devpts, efs, ext, ext2, ext3, ext4, hfs, hfsplus, hpfs, iso9660, jfs, minix, msdos, ncpfs, nfs, nfs4, ntfs, proc, qnx4, ramfs, reiserfs, romfs, squashfs, smbfs, sysv, tmpfs, ubifs, udf, ufs, umsdos, usbfs, vfat, xenix, xfs, xiafs.

**A new image tag was created "v4"** for the users who needed a ready to go nfs4 client without setting additional ENVIRONMENT variables.

## ENVIRONMENT

- `SERVER` - the hostname or IP of the NFS server to connect to
- `SHARE` - the NFS shared path to mount
- `MOUNT_OPTIONS` - mount options to mount the NFS share with; the default is `nfsvers=3` on betterweb/nfs-client:latest and `nfsvers=4` on betterweb/nfs-client:v4
- `FSTYPE` - the filesystem type; default is `nfs` in betterweb/nfs-client:latest for NFS3 mount, and default is `nfs4` in betterweb/nfs-client:v4
- `MOUNTPOINT` - the mount point for the NFS share within the container (default is /mnt/nfs-1)

## Usage

Several possibilities:
### 1. Mount your NFS mount-point **manually** on your host

Run the container

`docker run -itd --privileged=true --net=host betterweb/nfs-client`

then you can use NFS to mount all your mountpoints on your host

`sudo mount -t nfs SERVER_IP:/shared_path /mount_point`

### 2. Mount the mount-point **into** the nfs-client container

Basic command
`docker run -itd --privileged=true --net=host  -e SERVER=nfs_server_ip -e SHARE=shared_path betterweb/nfs-client`

**It is more convenient to set a volume**

Simply add a volume if you need to share the volume with other containers or mount it directly on your host (take care to add the **:shared** mention on the volume option)
`docker run -itd --privileged=true --name nfs --net=host -v /mnt/shared_nfs:/mnt/nfs-1:shared -e SERVER=nfs_server_ip -e SHARE=shared_path betterweb/nfs-client`

Then, using the `--volume-from nfs` option when runing another container will also made available the nfs shared content in this new container   
*Alternatively if you are not using a "named volume" but a "shared volume" you could also directly mount the host's directory that mounts the nfs in the new container*



### 3.  For [RancherOS](https://github.com/rancher/os/) users it is possible to run this nfs-client container **at [RancherOS](https://github.com/rancher/os/) startup** 

by adding the nfs service to one of your cloud-config.yml, user-config.yml or enabled service.yml... 

i.e: see the file [rancheros-cloud-config.yml](https://github.com/BetterCorp/docker-nfs-client/blob/master/rancheros-cloud-config.yml)


You could also use the [additional mount syntax](https://rancher.com/docs/os/v1.x/en/storage/additional-mounts/) addapted to NFS (since you now have a nfs-client started at os startup). 
ie:

```
#cloud-config
        mounts:
             - ["SERVER_IP:/shared_path", "/mnt/nfs-1", "nfs", ""]
```



[![GitHub license](https://img.shields.io/github/license/BetterCorp/docker-nfs-client)](https://github.com/BetterCorp/docker-nfs-client/blob/master/LICENSE)


License
-------------------

```text
The MIT License (MIT)

Copyright (c) 2015 Evey Quirk
Copyright (c) 2015 Chris Fordham
Copyright (c) 2016 betterweb

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```
