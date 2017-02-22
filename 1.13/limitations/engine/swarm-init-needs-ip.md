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