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

### Client → Server Events

| Event | Auth | Message Body (Payload) | Response / Broadcast Event |
| :--- | :---: | :--- | :--- |
| `ping` | `JWT` | — | `pong` (Ack) |
| `server.message` | `JWT (Admin)` | `{ text: string }` | `server.message.result` (Ack) |
| `room.join` | `JWT` | `{ roomId: string }` | `room.join.result` / error |
| `room.leave` | `JWT` | `{ roomId: string }` | `room.leave.result` |
| `room.typing` | `JWT` | `{ roomId: string, isTyping: boolean }` | `room.typing.event` (To Room) / error |
| `room.message` | `JWT` | `{ roomId: string, message: string, media?: Array, replyTo?: string, isForwarded?: boolean, ... }` | `room.message.new` (To Room) / error |
| `room.message.react` | `JWT` | `{ reaction: ReactionDto }` | `room.message.reaction.updated` (To Room) |
| `call.accept` | `JWT` | `{ roomId: string }` | `{ status, token, url }` (Ack) + Publishes to RTC |
| `call.decline` | `JWT` | `{ roomId: string }` | `{ status: 'success' \| 'error' }` (Ack) |
| `call.leave` | `JWT` | `{ roomId: string }` | `{ status: 'success' \| 'error' }` (Ack) |

### Server → Client Broadcast Events (Redis)

| Event | Trigger Source (Redis Channel) | Target / Scope | Description / Payload |
| :--- | :--- | :--- | :--- |
| `presence.update` | `presence:events` | All Connected Users | Users' online/offline status |
| `new_notification` | `notifications:event` (type: send) | Specific User Room | Sends a new notification to recipients |
| `seen_notification` | `notifications:event` (type: read) | Specific Room | Room notifications marked as seen |
| `seen_messages` | `messages:event` | Specific Room | Room messages marked as seen |
| `room.kicked` | `user:*:blocks` (Pattern) | Blocked User Socket | Force-kicks a blocked user from DM |
| `call.incoming` | `rtc:channel` (incoming_call) | Target User Rooms | Incoming call for target users |
| `call.user_joined` | `rtc `(call.user_acuser_joining_call) | Specific Room | A user joined the call |
| `call.user_accepted` | `rtc:channel` (user_accepted) | Specific Room | A user accepted the call |
| `call.user_declined` | `rtc:channel` (user_declined) | Specific Room | A specific user declined the call |
| `call.declined` | `rtc:channel` (call_declined) | Specific Room | The call was declined entirely |
| `call.user_left` | `rtc:channel` (user_left_call) | Specific Room | A user left the voice/video call |
| `call.ended` | `rtc:channel` (call_ended) | Specific Room | The call fully ended |
| `call.missed` | `rtc:channel` (call_missed) / Expired | Specific Room | A missed call is recorded |


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