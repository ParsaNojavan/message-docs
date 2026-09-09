---
title: "Message Chat"
weight: 2
---

### Responsibility

- Handling realtime chat through socket io gateway

- Handling group and dm creations and operations

- Creating tokens for livekit rooms


### Setup

- You need to initial the value of access token secret keys in .env file

- you will also need livekit server for handling realtime rtc calls for this you can use a docker container in your server(livekit container should only be in the chat microservice server)

{{< callout type="info" >}}
  To create the **livekit container**
  you can use the **docker compose** below

  ```yml
  services:
    livekit:
      image: livekit/livekit-server:latest
      command:
        - "--dev"
        - "--bind"
        - "0.0.0.0"
        - "--node-ip"
        - "127.0.0.1"
      ports:
        - "7880:7880"
        - "7881:7881"
        - "7882:7882/udp"
      restart: unless-stopped
  ```

  then in the same directory, run this command:

  ```bash
  docker-compose up -d
  ```

{{< /callout >}} 

### Environments

```bash
REDIS_HOST=localhost
REDIS_PORT=6379
MONGO_STRING="mongodb://root:example@localhost:27017/chatdb?authSource=admin"
JWT_SECRET=adsfljewifsdfkljlio
JWT_EXPIRATION="1h"
LIVEKIT_API_KEY=devkey
LIVEKIT_API_SECRET=secret
LIVEKIT_URL=ws://127.0.0.1:7880

```

- for more info about livekit sdk, you can visit [this link](https://docs.livekit.io/intro/overview/)
- you can also clone a test client for livekit sdk from [this link](https://github.com/livekit-examples/agent-starter-react)