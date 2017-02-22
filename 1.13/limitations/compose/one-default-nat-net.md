### Only one 'default' NAT network can be created

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
``