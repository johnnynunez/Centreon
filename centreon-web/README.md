# Centreon Web v21.04
Centreon documentation : https://docs.centreon.com/21.04/en/installation/installation-of-a-central-server/using-packages.html

## Building Docker image

```bash
docker build -t centreon-web .
```

## Create container

```bash
docker run -d -p <SOURCE_PORT>:80 --name <CONTAINER_NAME> centreon-web
```

## Connect to container

```bash
docker exec -it <CONTAINER_NAME> /bin/bash
```

Once the container is started, you need to set a MySQL root password, and then connect to the Centreon web interface to finish the setup.
