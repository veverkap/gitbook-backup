---
description: Deploy Memphis over Docker using Docker compose
---

# Docker

## Requirements

{% tabs %}
{% tab title="Docker" %}
**Requirements (No HA)**

| Resource | Quantity               |
| -------- | ---------------------- |
| OS       | Mac / Windows / Linux  |
| CPU      | 1 CPU                  |
| Memory   | 4GB                    |
| Storage  | 6GB                    |
{% endtab %}
{% endtabs %}

**Please make sure you have** [**docker-compose**](https://docs.docker.com/compose/) **installed.**

## Getting started

### Step 1: Download compose.yaml file

```
curl -s https://memphisdev.github.io/memphis-docker/docker-compose.yml -o docker-compose.yml
```

### Step 2: Run the compose file

```
docker compose -f docker-compose.yml -p memphis up
```

Output:

```
[+] Running 3/3
 ⠿ Container memphis-memphis-1        Creating                                                      0.2s                                                      0.2s                                                  0.2s
 ⠿ Container memphis-memphis-metadata-1          Creating                                                      0.2s
 ⠿ memphis-memphis-rest-gateway-1                                                  0.2s
```

#### Deployed Containers

* **memphis-1:** The broker itself which acts as the data storage layer. That is the component that stores and controls the ingested messages and their entire lifecycle management.
* **memphis-metadata-1:** Responsible for storing the platform metadata only, such as general information, monitoring, GUI state, and pointers to dead-letter messages. The metadata store uses Postgres.
* **memphis-rest-gateway-1:** Responsible for exposing Memphis management and data ingestion through REST requests.

### Step 3: Access via UI / CLI / SDK

{% tabs %}
{% tab title="UI" %}
The default port of the UI is 9000:

```
http://localhost:9000
```

**Default Username:** root

**Default Password**: memphis
{% endtab %}

{% tab title="SDK" %}
For more detailed information, head to the SDKs section below.

{% content-ref url="broken-reference" %}
[Broken link](broken-reference)
{% endcontent-ref %}

####

#### Memphis Node.JS SDK can be used to demonstrate the required parameters.

```
await memphis.connect({
            host: "broker.sandbox.memphis.dev",
            port: <port>, // defaults to 6666
            username: "<application type username>",
            connectionToken: "<connection_token>"
});
```

* **host:** Usually the control plane or through the UI URL. For example "https://memphis-ui.test.com/api".
* **username:** Usually "root". Head to the users' section via the UI or CLI to add more.
* **connectionToken:** Each app that produces and/or consumer data with Memphis uses token authentication. <mark style="color:green;">**The default value is "memphis".**</mark>
{% endtab %}
{% endtabs %}

## How to upgrade?

### Step 1: shutdown Memphis containers

```bash
docker rm -f $(docker ps -a | grep -i memphis | awk '{print $1}')
```

### Step 2: remove memphis docker images

```bash
docker image rm -f $(docker image ls | grep -i memphis)
```

### Step 3: Reinstall memphis

```bash
curl -s https://memphisdev.github.io/memphis-docker/docker-compose.yml -o docker-compose.yml && docker compose -f docker-compose.yml -p memphis up
```
