### Driver options are not supported for local volumes

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