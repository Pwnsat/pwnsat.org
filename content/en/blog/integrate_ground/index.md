---
title: "2. Integrate Radio Board"
description: "Integrate Radio Board to CosmosC3."
excerpt: "Integrate Radio Board to CosmosC3."
draft: false
weight: 50
images: ["code.png"]
categories: ["software"]
tags: ["security", "software", "flatsat", "environment"]
contributors: ["Kevin Leon"]
pinned: false
homepage: true
---

# Docker commands
```bash

$> docker network ls # <- List networks
NETWORK ID     NAME                          DRIVER    SCOPE
NETWORK ID     NAME             DRIVER    SCOPE
2aa352aa5636   bridge           bridge    local
1942b0ae6254   cosmos_default   bridge    local # <- Network
7cb3767096dc   dvwa_dvwa        bridge    local
a8ab87acd038   host             host      local
ab6b593c457d   none             null      local

$> docker network inspect cosmos_default
[
    {
        "Name": "cosmos_default",
        "Id": "1942b0ae6254c5da20d03e642dca016cb5216026bc04b546efdc08338265caba",
        "Created": "2025-07-04T19:36:46.580091002-06:00",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv4": true,
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": null,
            "Config": [
                {
                    "Subnet": "172.19.0.0/16",
                    "Gateway": "172.19.0.1"
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
        "Containers": {
            "18a064f6c112bb3672916cdda79aea1b755e990ad1430e9d7768b538617f9d87": {
                "Name": "cosmos-openc3-cosmos-cmd-tlm-api-1",
                "EndpointID": "afd4066e9922e388d5b33529615a5bf3ece1eec9f973a65897a5ccb42d17c377",
                "MacAddress": "f2:a1:ca:0a:9d:84",
                "IPv4Address": "172.19.0.8/16",
                "IPv6Address": ""
            },
            "2635e8de098304fab485cbbd0adaec515e53ce1196aa3f7fd044e07a6b7b346a": {
                "Name": "cosmos-openc3-cosmos-init-1",
                "EndpointID": "baacb107964cfd3babe88089ea4fcd97c1a95da12e73dc28a8e1645eb01a9361",
                "MacAddress": "86:d4:9e:99:de:31",
                "IPv4Address": "172.19.0.9/16",
                "IPv6Address": ""
            },
            "2bc8de7eaa367ab79db295da0610d46f6f6f86be13dc91816c45cd7692960e53": {
                "Name": "cosmos-openc3-cosmos-script-runner-api-1",
                "EndpointID": "a977724d5bfeb524e2ed5cdd6c47b5198ea48deb10428a95a703515990429d95",
                "MacAddress": "2e:99:90:b0:c6:d4",
                "IPv4Address": "172.19.0.6/16",
                "IPv6Address": ""
            },
            "8c4facfba8a99e3e780f6962dbfeb7e7f83129c620fd7ca1f481ec9c7d88952e": {
                "Name": "cosmos-openc3-redis-1",
                "EndpointID": "327505545a51d22e9604ed330e61e8a9cf44fa8ef3705b70dde760c7ae294fb6",
                "MacAddress": "12:35:6c:86:e5:d3",
                "IPv4Address": "172.19.0.4/16",
                "IPv6Address": ""
            },
            "a1efafa8e21649ffc2a42d602b460e2df7528cfc6d7ff8813b98e82740e5c242": {
                "Name": "cosmos-openc3-minio-1",
                "EndpointID": "6f6aa820e59c43c871707e56a886c691ef1a43b7b909443b53990a1e9545f8d8",
                "MacAddress": "82:cd:58:7f:b9:a7",
                "IPv4Address": "172.19.0.3/16",
                "IPv6Address": ""
            },
            "a4e2185882f1e4cff0461e8b3a5fe941f0def33b46fc639fe57663c09dab3539": {
                "Name": "cosmos-openc3-operator-1",
                "EndpointID": "9ab54075341ac74e2622436df2457ed877126911589901753d85a6496895f875",
                "MacAddress": "fe:a7:d6:ae:6d:bc",
                "IPv4Address": "172.19.0.7/16",
                "IPv6Address": ""
            },
            "bd96c9827ef8731ad6a06afda1da0ca30cf1314e5ac118118a00caebf7f4975c": {
                "Name": "cosmos-openc3-traefik-1",
                "EndpointID": "44dbf9791befdb910a1a6920a3e4c64e73808e94c33b9fc33cb6f1fd13e9c9b0",
                "MacAddress": "32:42:fe:20:f5:5f",
                "IPv4Address": "172.19.0.5/16",
                "IPv6Address": ""
            },
            "e03203d1e89e4f45b8913d055e7f5f6e1bba099fe9a1efb78e9090d9ab101e82": {
                "Name": "cosmos-openc3-redis-ephemeral-1",
                "EndpointID": "0ee4054e491df2995c9121dc1d9d38dbd9c9790aee8febee739dc72c0f38faa4",
                "MacAddress": "a6:55:55:68:11:b1",
                "IPv4Address": "172.19.0.2/16",
                "IPv6Address": ""
            }
        },
        "Options": {},
        "Labels": {
            "com.docker.compose.config-hash": "ef043b69aa48efebec763d45f0d58bbceef1de6a4e6015460921753bb469a64c",
            "com.docker.compose.network": "default",
            "com.docker.compose.project": "cosmos",
            "com.docker.compose.version": "2.35.1"
        }
    }
]
```

# Running our server Connector
```bash
pwnsat@ubuntu:~/groundStation$ docker build -t radio .
[+] Building 20.6s (8/8) FINISHED                            docker:default
 => [internal] load build definition from Dockerfile                   0.0s
 => => transferring dockerfile: 127B                                   0.0s
 => [internal] load metadata for docker.io/library/python:3.8-slim     2.3s
 => [internal] load .dockerignore                                      0.0s
 => => transferring context: 2B                                        0.0s
 => [1/3] FROM docker.io/library/python:3.8-slim@sha256:1d52838af602  17.9s
 => => resolve docker.io/library/python:3.8-slim@sha256:1d52838af602b  0.0s
 => => sha256:c37419b5a020178521c2e451f44424a2c0bdf34 1.75kB / 1.75kB  0.0s
 => => sha256:989bb9480c980a0e6722f366bf7dbf68bf6c097 5.25kB / 5.25kB  0.0s
 => => sha256:14c9d9d199323cbf0a4c2347a8af85f2875c 29.16MB / 29.16MB  12.6s
 => => sha256:d847ad1879f2202660d37d9bedf1b8986116079 3.33MB / 3.33MB  3.3s
 => => sha256:56d45b8e140401cb0bb97c76a062697a0502f 14.51MB / 14.51MB  8.9s
 => => sha256:1d52838af602b4b5a831beb13a0e4d0732806 10.41kB / 10.41kB  0.0s
 => => sha256:0cd45dda32d5387b73cdb7755ea8f31eca0e250f286 249B / 249B  3.5s
 => => extracting sha256:14c9d9d199323cbf0a4c2347a8af85f2875c1f2c26a1  3.1s
 => => extracting sha256:d847ad1879f2202660d37d9bedf1b8986116079d578b  0.3s
 => => extracting sha256:56d45b8e140401cb0bb97c76a062697a0502ff1928da  1.5s
 => => extracting sha256:0cd45dda32d5387b73cdb7755ea8f31eca0e250f2860  0.0s
 => [internal] load build context                                      0.0s
 => => transferring context: 1.59kB                                    0.0s
 => [2/3] WORKDIR /usr/src/app                                         0.2s
 => [3/3] COPY . .                                                     0.0s
 => exporting to image                                                 0.0s
 => => exporting layers                                                0.0s
 => => writing image sha256:83fdcd80cd8478d09312b1940dfd64e3bb6d01a2a  0.0s
 => => naming to docker.io/library/radio                     0.0s
```

```bash
pwnsat@ubuntu:~/groundStation$ docker run --net=cosmos_default --name radio -p1234:1234/udp -p1235:1235 --rm radio
Satellite Ground Station Connector
Version: 0.0.1
Waiting for commands...
```

# Building our Plugin Connector

```bash
pwnsat@ubuntu:~/cosmos$ ./openc3.sh cli generate plugin RADIO
Plugin openc3-cosmos-radio successfully generated!
pwnsat@ubuntu:~/cosmos$ ls
cacert.pem    openc3.bat               openc3.sh       README.md
compose.yaml  openc3-cosmos-radio  openc3-traefik  scripts
LICENSE.txt   openc3-redis             plugins
```
Generate the target
```bash
pwnsat@ubuntu:~/cosmos$ cd openc3-cosmos-radio
pwnsat@ubuntu:~/cosmos/openc3-cosmos-radio$ ../openc3.sh cli generate target RADIO
Target CONNECTOR successfully generated!
pwnsat@ubuntu:~/cosmos/openc3-cosmos-radio$ ls
LICENSE.txt                      plugin.txt  README.md
openc3-cosmos-radio.gemspec  Rakefile    targets
```
Edit the `plugin.txt`
```bash
VARIABLE ip 127.0.0.1
VARIABLE port_tm 1234
VARIABLE port_tc 1235
VARIABLE radio_target_name RADIO

TARGET RADIO <%= radio_target_name %>
INTERFACE <%= radio_target_name %>_INT udp_interface.rb <%= ip %> <%= port_tc %> <%= port_tm %> nil nil 128 nil nil MAP_TARGET <%= radio_target_name %>
```
Now edit the `tml` files, first the `cmd.txt`:
```bash
COMMAND RADIO PING BIG_ENDIAN "Ping Satellite"
  APPEND_PARAMETER CMD_STRING 0 STRING "PING" "Ping Command"

COMMAND RADIO STATUS BIG_ENDIAN "Get Status"
  APPEND_PARAMETER CMD_STRING 0 STRING "STATUS" "Command to Query Status"

COMMAND RADIO GET_TEMP BIG_ENDIAN "Get TEMP"
  APPEND_PARAMETER CMD_STRING 0 STRING "GET_TEMP" "Command to Query Temp"

COMMAND RADIO SET_MODE BIG_ENDIAN "Set Satellite Mode"
  APPEND_PARAMETER CMD_STRING 0 STRING "SET_MODE" "Command to Set Mode of Satellite"

COMMAND RADIO REBOOT BIG_ENDIAN "REBOOT Satellite"
  APPEND_PARAMETER CMD_STRING 0 STRING "REBOOT" "Reboot Satellite"
```

``` bash
TELEMETRY RADIO STAUS BIG_ENDIAN "STATUS PKT"
  APPEND_ID_ITEM PACKET_ID 16 UINT 0x4320 "PACKET ID"
    FORMAT_STRING "0X%04X"
  APPEND_ITEM RESULT 96 STRING "RESPONSE"

TELEMETRY RADIO PING BIG_ENDIAN "PING PKT"
  APPEND_ID_ITEM PACKET_ID 16 UINT 0x4321 "PACKET ID"
    FORMAT_STRING "0X%04X"
  APPEND_ITEM RESULT 96 STRING "RESPONSE"

TELEMETRY RADIO TEMP BIG_ENDIAN "TEMP PKT"
  APPEND_ID_ITEM PACKET_ID 16 UINT 0x4322 "PACKET ID"
    FORMAT_STRING "0X%04X"
  APPEND_ITEM RESULT 96 STRING "RESPONSE"

TELEMETRY RADIO MODE BIG_ENDIAN "MODE PKT"
  APPEND_ID_ITEM PACKET_ID 16 UINT 0x4323 "PACKET ID"
    FORMAT_STRING "0X%04X"
  APPEND_ITEM RESULT 96 STRING "RESPONSE"

TELEMETRY RADIO REBOOT BIG_ENDIAN "REBOOT PKT"
  APPEND_ID_ITEM PACKET_ID 16 UINT 0x4324 "PACKET ID"
    FORMAT_STRING "0X%04X"
  APPEND_ITEM RESULT 96 STRING "RESPONSE"

TELEMETRY RADIO ERROR BIG_ENDIAN "ERROR PKT"
  APPEND_ID_ITEM PACKET_ID 16 UINT 0x4325 "PACKET ID"
    FORMAT_STRING "0X%04X"
  APPEND_ITEM RESULT 96 STRING "RESPONSE"
```
Now that your packet definition files are done, the next step is to build the plugin. To do this, you need to navigate back a few directories using the command ```cd ../../../``` and then issuing the following command to build your plugin file:
```bash
pwnsat@ubuntu:~/cosmos/openc3-cosmos-radio$ ../openc3.sh cli rake build VERSION=1.0.0 .
gem build openc3-cosmos-radio
WARNING:  make sure you specify the oldest ruby version constraint (like ">= 3.0") that you want your gem to support by setting the `required_ruby_version` gemspec attribute
WARNING:  See https://guides.rubygems.org/specification-reference/ for help
openc3cli validate openc3-cosmos-radio-1.0.0.gem
  Successfully built RubyGem
  Name: openc3-cosmos-radio
  Version: 1.0.0
  File: openc3-cosmos-radio-1.0.0.gem
Installing openc3-cosmos-radio-1.0.0.gem
Successfully validated openc3-cosmos-radio-1.0.0.gem
```

