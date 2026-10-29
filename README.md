# Assignment 2 - Part 1 Docker Networks & Docker-Compose
### Create a new Docker network named "my_network" using the bridge driver.

`docker network ls` lists current network drivers

```
ubuntu@ubuntu:~/dicebootcamp26-assignment2-part1$
ubuntu@ubuntu:~/dicebootcamp26-assignment2-part1$ docker network ls
NETWORK ID     NAME      DRIVER    SCOPE
1d1057c86241   bridge    bridge    local
f6ec2e3dc1b6   host      host      local
b2938995de5f   none      null      local
ubuntu@ubuntu:~/dicebootcamp26-assignment2-part1$
```

`docker network create --driver bridge my_network` will create a bridge network with name ***my_network***
```
ubuntu@ubuntu:~/dicebootcamp26-assignment2-part1$ docker network create --driver bridge my_network
dada9b0d2f9a6fa083a150d767e2a9b071e320d7c5337351b99042bc4a030c44
ubuntu@ubuntu:~/dicebootcamp26-assignment2-part1$
```

`docker network ls` can be used again to verify that a bridge network is created.

```
ubuntu@ubuntu:~/dicebootcamp26-assignment2-part1$ docker network ls
NETWORK ID     NAME         DRIVER    SCOPE
1d1057c86241   bridge       bridge    local
f6ec2e3dc1b6   host         host      local
dada9b0d2f9a   my_network   bridge    local
b2938995de5f   none         null      local
ubuntu@ubuntu:~/dicebootcamp26-assignment2-part1$
```

### Create a new Docker container using the "nginx" image and connect it to the "my_network" network. Name the container "nginx_container".

`docker run -d --name nginx_container --network my_network nginx` will create a new container with name ***nginx_container*** and it will use previously created network bridge driver ***my_network***
	- Port 80 of container is exposed to the port number 8080 of main machine running the docker engine

```
ubuntu@ubuntu:~/dicebootcamp26-assignment2-part1$ docker run -d -p 8080:80 --name nginx_container --network my_network nginx
e6f240f486db39ad3a2ea5c27a494dbfdb449498ec474a58fbb6fc97cf1a50e1
ubuntu@ubuntu:~/dicebootcamp26-assignment2-part1$

```

### Verify that the "nginx" default page is accessible on your host machine at http://localhost:8080.

```
ubuntu@ubuntu:~/dicebootcamp26-assignment2-part1$
ubuntu@ubuntu:~/dicebootcamp26-assignment2-part1$ curl http://localhost:8080
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, nginx is successfully installed and working.
Further configuration is required for the web server, reverse proxy,
API gateway, load balancer, content cache, or other features.</p>

<p>For online documentation and support please refer to
<a href="https://nginx.org/">nginx.org</a>.<br/>
To engage with the community please visit
<a href="https://community.nginx.org/">community.nginx.org</a>.<br/>
For enterprise grade support, professional services, additional
security features and capabilities please refer to
<a href="https://f5.com/nginx">f5.com/nginx</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
ubuntu@ubuntu:~/dicebootcamp26-assignment2-part1$
```

### Create a new Docker container using the "httpd" image and connect it to the "my_network" network. Name the container "httpd_container".

```
ubuntu@ubuntu:~/dicebootcamp26-assignment2-part1$ docker run -d -p 8081:80 --name httpd_container --network my_network httpd
004ad5b3d7393c80729b337e182a51ef7c13e7ec6aa7abd2e0d8c0de631a2095
ubuntu@ubuntu:~/dicebootcamp26-assignment2-part1$
```

### Verify that the "httpd" default page is accessible on your host machine at http://localhost:8081.

```
ubuntu@ubuntu:~/dicebootcamp26-assignment2-part1$ curl http://localhost:8081
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
<html>
<head>
<title>It works! Apache httpd</title>
</head>
<body>
<p>It works!</p>
</body>
</html>
ubuntu@ubuntu:~/dicebootcamp26-assignment2-part1$
```

### Use the "docker network inspect" command to display information about the "my_network" network. Document your findings in the README.md file.

```
ubuntu@ubuntu:~/dicebootcamp26-assignment2-part1$ docker network inspect my_network
[
    {
        "Name": "my_network",
        "Id": "dada9b0d2f9a6fa083a150d767e2a9b071e320d7c5337351b99042bc4a030c44",
        "Created": "2026-10-29T14:08:55.042290104Z",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv4": true,
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "172.18.0.0/16",
                    "Gateway": "172.18.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Options": {},
        "Labels": {},
        "Containers": {
            "004ad5b3d7393c80729b337e182a51ef7c13e7ec6aa7abd2e0d8c0de631a2095": {
                "Name": "httpd_container",
                "EndpointID": "5a4f4aa872bd39ce295a266a209f38da355bb1987d868b15c9f21e6cce88c387",
                "MacAddress": "06:a9:1b:be:c7:bc",
                "IPv4Address": "172.18.0.3/16",
                "IPv6Address": ""
            },
            "e6f240f486db39ad3a2ea5c27a494dbfdb449498ec474a58fbb6fc97cf1a50e1": {
                "Name": "nginx_container",
                "EndpointID": "50c57f580027d524c12e4c33cd037f81344a0cbefd41a966df11cbccd31bad97",
                "MacAddress": "b6:76:1f:5e:59:38",
                "IPv4Address": "172.18.0.2/16",
                "IPv6Address": ""
            }
        },
        "Status": {
            "IPAM": {
                "Subnets": {
                    "172.18.0.0/16": {
                        "IPsInUse": 5,
                        "DynamicIPsAvailable": 65531
                    }
                }
            }
        }
    }
]
ubuntu@ubuntu:~/dicebootcamp26-assignment2-part1$
```

 ###### Key Findings :

- `EnableIPv4`: Set to *True*. IPv4 is enabled by default
- `EnableIPv6`: Set to *False*. IPv6 is disabled by default
- `Containers`: This section gives you details about containers using this driver
- `Subnet`: "172.18.0.0/16"
- `Gateway`: "172.18.0.1"
### Stop and remove the "nginx_container" container.

```
ubuntu@ubuntu:~/dicebootcamp26-assignment2-part1$ docker stop nginx_container && docker rm nginx_container
nginx_container
nginx_container
ubuntu@ubuntu:~/dicebootcamp26-assignment2-part1$ docker ps
CONTAINER ID   IMAGE     COMMAND              CREATED         STATUS         PORTS                                     NAMES
004ad5b3d739   httpd     "httpd-foreground"   7 minutes ago   Up 7 minutes   0.0.0.0:8081->80/tcp, [::]:8081->80/tcp   httpd_container
```

### Create a new Docker container using the "nginx" image and connect it to the "my_network" network. Name the container "nginx_container_2".

```
ubuntu@ubuntu:~/dicebootcamp26-assignment2-part1$ docker run -d -p 8082:80 --name nginx_container --network my_network nginx
d5632155434f07ef2318e32696f2116506be2066485cebbb215f6d3e39845f3d
ubuntu@ubuntu:~/dicebootcamp26-assignment2-part1$
ubuntu@ubuntu:~/dicebootcamp26-assignment2-part1$
ubuntu@ubuntu:~/dicebootcamp26-assignment2-part1$ docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS                                     NAMES
d5632155434f   nginx     "/docker-entrypoint.…"   3 seconds ago   Up 2 seconds   0.0.0.0:8082->80/tcp, [::]:8082->80/tcp   nginx_container
004ad5b3d739   httpd     "httpd-foreground"       9 minutes ago   Up 9 minutes   0.0.0.0:8081->80/tcp, [::]:8081->80/tcp   httpd_container
ubuntu@ubuntu:~/dicebootcamp26-assignment2-part1$
```

### Verify that the "nginx" default page is accessible on your host machine at http://localhost:8082.

```
ubuntu@ubuntu:~/dicebootcamp26-assignment2-part1$ curl http://localhost:8082
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, nginx is successfully installed and working.
Further configuration is required for the web server, reverse proxy,
API gateway, load balancer, content cache, or other features.</p>

<p>For online documentation and support please refer to
<a href="https://nginx.org/">nginx.org</a>.<br/>
To engage with the community please visit
<a href="https://community.nginx.org/">community.nginx.org</a>.<br/>
For enterprise grade support, professional services, additional
security features and capabilities please refer to
<a href="https://f5.com/nginx">f5.com/nginx</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
ubuntu@ubuntu:~/dicebootcamp26-assignment2-part1$

```

### Use the "docker container ls" command to display information about all running containers. Document your findings in the README.md file.

```
ubuntu@ubuntu:~/dicebootcamp26-assignment2-part1$
ubuntu@ubuntu:~/dicebootcamp26-assignment2-part1$ docker container ls
CONTAINER ID   IMAGE     COMMAND                  CREATED              STATUS              PORTS                                     NAMES
d5632155434f   nginx     "/docker-entrypoint.…"   About a minute ago   Up About a minute   0.0.0.0:8082->80/tcp, [::]:8082->80/tcp   nginx_container
004ad5b3d739   httpd     "httpd-foreground"       11 minutes ago       Up 11 minutes       0.0.0.0:8081->80/tcp, [::]:8081->80/tcp   httpd_container
ubuntu@ubuntu:~/dicebootcamp26-assignment2-part1$
```
###### Key Findings :

- `docker container ls` gives you the same output as a regular `docker ps` command
- It gives you information about currently running containers, their port mappings along with some other useful information.

### Stop and remove all containers.

```
ubuntu@ubuntu:~/dicebootcamp26-assignment2-part1$
ubuntu@ubuntu:~/dicebootcamp26-assignment2-part1$
ubuntu@ubuntu:~/dicebootcamp26-assignment2-part1$ docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS                                     NAMES
d5632155434f   nginx     "/docker-entrypoint.…"   4 minutes ago    Up 4 minutes    0.0.0.0:8082->80/tcp, [::]:8082->80/tcp   nginx_container
004ad5b3d739   httpd     "httpd-foreground"       13 minutes ago   Up 13 minutes   0.0.0.0:8081->80/tcp, [::]:8081->80/tcp   httpd_container
ubuntu@ubuntu:~/dicebootcamp26-assignment2-part1$
ubuntu@ubuntu:~/dicebootcamp26-assignment2-part1$ docker stop d5632155434f && docker rm d5632155434f
d5632155434f
d5632155434f
ubuntu@ubuntu:~/dicebootcamp26-assignment2-part1$ docker stop 004ad5b3d739 && docker rm 004ad5b3d739
004ad5b3d739
004ad5b3d739
ubuntu@ubuntu:~/dicebootcamp26-assignment2-part1$
ubuntu@ubuntu:~/dicebootcamp26-assignment2-part1$
ubuntu@ubuntu:~/dicebootcamp26-assignment2-part1$ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
ubuntu@ubuntu:~/dicebootcamp26-assignment2-part1$
```


### Cleanup: Remove the "my_network" network.

`docker network rm my_network` command will remove the **my_network** bridge adapter

```
ubuntu@ubuntu:~/dicebootcamp26-assignment2-part1$ docker network ls
NETWORK ID     NAME         DRIVER    SCOPE
1d1057c86241   bridge       bridge    local
f6ec2e3dc1b6   host         host      local
dada9b0d2f9a   my_network   bridge    local
b2938995de5f   none         null      local
ubuntu@ubuntu:~/dicebootcamp26-assignment2-part1$
ubuntu@ubuntu:~/dicebootcamp26-assignment2-part1$ docker network rm my_network
my_network
ubuntu@ubuntu:~/dicebootcamp26-assignment2-part1$
ubuntu@ubuntu:~/dicebootcamp26-assignment2-part1$ docker network ls
NETWORK ID     NAME      DRIVER    SCOPE
1d1057c86241   bridge    bridge    local
f6ec2e3dc1b6   host      host      local
b2938995de5f   none      null      local
ubuntu@ubuntu:~/dicebootcamp26-assignment2-part1$
```
