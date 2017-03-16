# Docker 1.13 - Windows Limitations

Docker on Windows does not have full feature parity with Docker on Linux for version 1.13. 

These are the known limitations running on Windows.

## Engine

- No Docker-in-Docker
- Windows Server containers can't be paused

## Swarm mode

- [Swarm initialization needs IP addresses specified](limitations/engine/swarm-init-needs-ip.md)
- Secrets not available
- Services need to run with `--endpoint-mode=dnsrr`

## Volumes

- [Driver options are not supported for local volumes](limitations/volumes/driver-opts-not-supported.md)
- Some app platforms cannot access mounted volumes in the container


## Docker Compose

- [Only one 'default' NAT network can be created](limitations/compose/one-default-nat-net.md)
