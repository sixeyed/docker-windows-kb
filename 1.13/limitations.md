# Docker 1.13 - Windows Limitations

Docker on Windows does not have full feature parity with Docker on Linux for version 1.13. 

These are the known limitations running on Windows.

## Engine

### Swarm initialization needs IP addresses specified

Overlay networking is [available in Windows 10](https://blogs.technet.microsoft.com/virtualization/2017/02/09/overlay-network-driver-with-support-for-docker-swarm-mode-now-available-to-windows-insiders-on-windows-10/) and coming soon to Server 2016. On Windows 10 you can run Docker in swarm mode, but the default initialization fails:

```
> docker swarm init
Error response from daemon: could not choose an IP address to advertise since this system has multiple addresses on different interfaces (...) - specify one with --advertise-addr
```

If you follow the advice in the error, the `init` command just hangs:

```
> docker swarm init --advertise-addr 192.168.2.214
# NEVER RETURNS #
```

####WORKAROUND

Specify both the advertise and listen addresses:

```
> docker swarm init --advertise-addr 192.168.2.214 --listen-addr 192.168.2.214
Swarm initialized: current node (...) is now a manager.
```

## Volumes

### Driver options are not supported for local volumes. 

You can't create a volume at a specific location, you have to use the defaults.

```
docker volume create vol1
```

Creates a volume where the mountpoint is something like `C:\ProgramData\docker\columes\vol1\_data`. 

If you have a RAID array on `E:` you can't use that for the mountpoint as you could in Linux, because options aren't supported:

```
> docker volume create vol2 --opt device=e:\data
Error response from daemon: create vol2: options are not supported on this platform
```

####WORKAROUND

None.

## Docker Compose

### Only one 'default' NAT network can be created.

If you don't specify a network in the compose file, Docker Compose will try to create a network for the app using the default settings. Windows only allows one default NAT network (you can create other, but you need to explicitly specify the IP address range).

```
version: '3'

services:
  
  web:
    image: microsoft/iis:windowsservercore
```
This compose file won't run because of the network issue:

```
> docker-compose up -d
Creating network "temp_default" with the default driver
ERROR: HNS failed with error : The parameter is incorrect.
```

####WORKAROUND

You need to explicitly configure an external network, which can be the default NAT or a custom one you create for the app:

```
version: '3'

services:
  
  web:
    image: microsoft/iis:windowsservercore
    networks:
      - app-net

networks:
  app-net:
    external:
      name: nat
```


