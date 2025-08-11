---
title: "1. OpenC3 Installation"
description: "Introduction to OpenC3, installation and configuration for our hacking environment."
excerpt: "Introduction to OpenC3, installation and configuration for our hacking environment."
draft: false
weight: 50
images: ["code.png"]
categories: ["software"]
tags: ["security", "software", "flatsat", "environment"]
contributors: ["Kevin Leon"]
pinned: false
homepage: true
---

COSMOS is a suite of applications that can be used to control a set of embedded systems. These systems can be anything from test equipment (power supplies, oscilloscopes, switched power strips, UPS devices, etc), to development boards (Arduinos, Raspberry Pi, Beaglebone, etc), to satellites.

For more information about the project check this [link.](https://docs.openc3.com/docs)

# Prerequisites
- Docker
- Docker Compose
- Git
# Clone the project
First we need to clone the next repository [cosmos-project](https://github.com/OpenC3/cosmos-project).

```bash
git clone https://github.com/OpenC3/cosmos-project.git
```

# Open to the network using http
Edit the `composer.yaml` with the following changes:
1. Comment the line: `"./openc3-traefik/traefik.yaml:/etc/traefik/traefik.yaml:z"`
2. Uncomment the line: `"./openc3-traefik/traefik-ssl.yaml:/etc/traefik/traefik.yaml:z"`

```yaml
  [...]
  openc3-traefik:
    user: "${OPENC3_USER_ID:-1001}:${OPENC3_GROUP_ID:-1001}"
    image: "${OPENC3_REGISTRY}/${OPENC3_NAMESPACE}/openc3-traefik${OPENC3_IMAGE_SUFFIX}:${OPENC3_TAG}"
    volumes:
      - "./cacert.pem:/devel/cacert.pem:z"
      # - "./openc3-traefik/traefik.yaml:/etc/traefik/traefik.yaml:z"           # Comment this line
      - "./openc3-traefik/traefik-allow-http.yaml:/etc/traefik/traefik.yaml:z"  # Uncomment this line
      # - "./openc3-traefik/traefik-ssl.yaml:/etc/traefik/traefik.yaml:z"
      # - "./openc3-traefik/traefik-letsencrypt.yaml:/etc/traefik/traefik.yaml:z"
      # - "./openc3-traefik/cert.key:/etc/traefik/cert.key:z"
      # - "./openc3-traefik/cert.crt:/etc/traefik/cert.crt:z"
    ports:
      - "127.0.0.1:2900:2900"
      - "127.0.0.1:2943:2943"
      # - "80:2900"
      # - "443:2943"
    [...]
```

# Configuring the .env
We need to configure the `.env` file to use our instance without any extra service, to do this we edit the env file as the folling:
```yaml
# Which TAG to deploy, latest or specific version, e.g. 5.4.2
OPENC3_TAG=6.5.0
[...]
# OPENC3_LOCAL_MODE=1 <- Comment this to disable the local mode
# OPENC3_DEMO=1 <- Comment this to disable the Demo mode
[...]
OPENC3_LANGUAGE=ruby # <- Uncomment to set the default language for plugin generation
```

# Running the docker
Once we modify the `composer.yaml` and the `.env` we can enable the services (you need to have already running the docker service). The `run.sh` is a CLI script to interact with some services, if you run the script alone `./openc3.sh` the script will show the information help.
```bash
$> ./openc3.sh
Usage: ./openc3.sh [cli, cliroot, start, stop, cleanup, run, util]
*  cli: run a cli command as the default user ('cli help' for more info)
*  cliroot: run a cli command as the root user ('cli help' for more info)
*  start: start the docker-compose openc3
*  stop: stop the running dockers for openc3
*  cleanup: cleanup network and volumes for openc3
*  run: run the prebuilt containers for openc3
*  util: various helper commands
```

We use the command `./openc3.sh run` to build and run the services. After some minutes the services will setup and you are able to use OpenC3 using the IP of the server or using the localhost url:
`http://localhost:2900/`


# Posible Errors
If you already running the OpenC3 instance before the changes, you can run the `./openc3.sh cleanup` command to delete all the informacion and then run `./openc3.sh run` to enable the services.

> Note: This will delete all the information