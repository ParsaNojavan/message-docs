---
title: "Message Support"
next: "http gateway"
weight: 5
---

### Responsibility

- Hanled realtime support chat using socket io gateway

- List rooms for your api-key


### Setup

- You need to recieve an api-key from [user microservice](/docs/microservices/message-user)

- You need to send your api-key in every socket io request inside the handshake

- you will also need to generate a public key with open ssl from the private key you generated in [user microservice](/docs/microservices/message-user)

{{< callout type="warning" >}}
  To create the **public key** run the code below in terminal  

  ```bash
    openssl rsa -in <path/to/private.pem> -pubout -out public.pem
  ```
  put the generated file in this location :

  ```bash
    keys / public.pem
  ```

{{< /callout >}} 

### Connection / Auth

| Role | Auth | Room Joined |
| :--- | :--- | :--- |
| `admin` | `JWT` (handshake `auth.token` / `query.token`) | `admin_room_{ownerId}` |
| `visitor` | `API Key` (header `x-api-key` / `auth.apiKey` / `query.apiKey` + origin domain check) | `visitor_{visitorId}` |

### Client → Server Events

| Event | Auth | Message Body (Payload) | Response / Broadcast Event |
| :--- | :---: | :--- | :--- |
| `sendMessage` | `API Key (WsApiKeyGuard)` | `{ text: string }` | `newMessage` (To `admin_room_{ownerId}`) + `{ status: 'ok', message }` (Ack) |
| `adminReply` | `JWT (Admin)` | `{ roomId: string, visitorId: string, text: string }` | `newMessage` (To `visitor_{visitorId}`) + `{ status: 'ok', message }` (Ack) |

### Server → Client Broadcast Events (Redis)

| Event | Trigger Source (Redis Channel) | Target / Scope | Description / Payload |
| :--- | :--- | :--- | :--- |
| `newRoom` | `room:created` | `admin_room_{ownerId}` | New chat room created for the owner: `{ roomId, ownerId, visitorId, updatedAt }` |
| `newMessage` | — (emitted by `sendMessage`) | `admin_room_{ownerId}` | Visitor message saved: `{ roomId, message }` |
| `newMessage` | — (emitted by `adminReply`) | `visitor_{visitorId}` | Admin reply saved: `{ roomId, message }` |


### Environments

```bash
REDIS_HOST=localhost
REDIS_PORT=6379
MONGO_STRING="mongodb://root:example@localhost:27017/supportdb?authSource=admin"
JWT_SECRET=adsfljewifsdfkljlio
JWT_EXPIRATION="1h"
JWT_REFRESH_SECRET=ldksjfsdl
JWT_REFRESH_EXPIRATION="1h"
```