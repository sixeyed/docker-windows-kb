# Docker 1.13 - Windows Issues

There are some situations where Docker on Windows doesn't behave as expected.

These are the known issues, with resolutions.

## Engine

### Publish port unavailable - HNS failed with error : Failed to create endpoint

I haven't reliably replicated this, but it seems to happen when you have a container running with a published port and the host exits ungracefully. 

When the host restarts, the port mapping hasn't been cleared. It doesn't get cleared if you `docker rm` the old container either. 

If you `docker run` a new container trying to publish to the same port, you get the error from HNS (Windows Host Networking Service), because the port is still mapped.

####WORKAROUND

Use PowerShell `NetNat` commands to clear the still-allocated port. E.g. for port 80:

```
Get-NetNatStaticMapping | ? ExternalPort -eq 80 | Remove-NetNatStaticMapping
```

[Credit to Kentico for the workaround](https://devnet.kentico.com/articles/running-kentico-in-a-docker-container).
