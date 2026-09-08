---
title: "get started"
next: microservices
weight: 1
---

Realtime chat, live calls and support services

### Setup

- You need to create a mongodb connection and copy the connection string in microservices environment.

- you will also need a redis connection for caching and call managment

{{< callout type="warning" >}}
  To manage the **call timeouts** run the code below in terminal

  redis-cli :
  ```bash
    CONFIG SET notify-keyspace-events Ex
  ```
  docker :
  ```bash
    docker exec -it <container_name> redis-cli CONFIG SET notify-keyspace-events Ex
  ```
{{< /callout >}} 

- Now you need to define all the variables in microservices env file. 
You need to replace both mongo string and redis string.


| Microservice | <span style="display: inline-flex; align-items: center; gap: 6px;">{{< icon name="github" >}} GitHub Link</span> |
| :--- | :--- |
| `http-gateway` | http gateway ([github](https://github.com/ParsaNojavan/message-http-gateway)) |
| `user` | user and auth microservice ([github](https://github.com/ParsaNojavan/message-user)) |
| `notification` | notification and otp microservice ([github](https://github.com/ParsaNojavan/message-notification)) |
| `chat` | realtime chat and call core ([github](https://github.com/ParsaNojavan/message-chat)) |
| `media` | upload media microservice ([github](https://github.com/ParsaNojavan/message-chat)) |
| `support` | support service managment microservice ([github](https://github.com/ParsaNojavan/message-support)) |
| `payment` | payment managment microservice ([github](https://github.com/ParsaNojavan/message-payment)) |
| `libs` | shared libs git submodule ([github](https://github.com/ParsaNojavan/message-libs)) |
| `frontend` | realtime chat frontend ([github](https://github.com/ParsaNojavan/message-frontend)) |
