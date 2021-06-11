# Centreon Web v21.04
Centreon documentation : https://docs.centreon.com/21.04/en/installation/installation-of-a-central-server/using-packages.html

## Building Docker image

```bash
docker build -t centreon-web .
```

## Create container

```bash
	
docker run -d --name centreon --privileged -p 8080:80 -p 3306:3306 -p 10022:22 -v /sys/fs/cgroup:/sys/fs/cgroup:ro centreon-web
```

## Connect to container

```bash
docker exec -it centreon bash
```

Once the container is started, you need to set a MySQL root password in terminal with:
```bash
mysql_secure_installation
```
, and then connect to the Centreon web interface to finish the setup.

It is optional if monitoring is not running
Then remove centcore.cmd
```bash
rm -rf /var/lib/centreon/centcore.cmd
```
