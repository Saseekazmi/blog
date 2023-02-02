# How to connect  the localhost to the docker container

If you are debugging an application inside a docker container and want to connect to the SQL server running on your localhost:3000, you cannot give the localhost:3000 as a connection string. Docker doesn't know how to connect to your local host. To solve that, you can follow the below methods.

If you are a mac or windows user, you could simply replace the **localhost:3000** with \*\*host.docker.internal:3000 \*\* inside your container.

If you are a Linux user, you could add \*\*--add-host \*\*flag for docker run. Start your containers with this flag to expose the host string:

```plaintext
docker run --add-host host.docker.internal:host-gateway containername:latest
```

The --add-host flag adds an entry to the container’s /etc/hosts file. The value shown above maps host.docker.internal to the container’s host gateway, which matches the real localhost value. You could replace host.docker.internal with your own string if you prefer.

To get the same behaviour in docker-compose, you need to specify the host.docker.internal:host-gateway using the **extra\_hosts** parameter.

```plaintext
version: "3.8"
services:

  ubuntu:
    image: ubuntu
    container_name: ubuntu
    extra_hosts:
      - "host.docker.internal:host-gateway"
    command: sleep infinity
```

**Note:** host.docker.internal which resolves to the internal IP address used by the host. This is for development purposes and will not work in a production environment outside of Docker Desktop for Windows.