---
title: "http gateway"
prev: "message-support"
weight: 3
---

### Responsibility

- passing request to the related microservice using client proxy

- Checking access in auth guards


### Setup

- You need to initial the value of access token secret keys in .env file

### User Controller (`/user`)

| Method | Endpoint | Auth | Params / Query | Body | Event |
| :--- | :--- | :---: | :--- | :--- | :--- |
| `POST` | `/user/login` | Public | — | `{ ... }` *(userDto)* | `user.login` |
| `POST` | `/user/register` | Public | — | `{ ... }` *(userDto)* | `user.register` |
| `POST` | `/user/verify-code` | Public | — | `{ ... }` *(userDto)* | `user.verify-code` |
| `POST` | `/user/refresh-token` | Public | — | `{ refreshToken }` | `user.refresh-token` |
| `POST` | `/user/reset-password` | `JWT` | — | `{ ... }` *(resetPassword)* | `user.reset-password` |
| `GET` | `/user/user-profile` | `JWT` | — | — | `user.profile` |
| `PATCH` | `/user/user-update` | `JWT` | — | `{ ... }` *(userDto)* | `user.update` |
| `POST` | `/user/block-user` | `JWT` | — | `{ blockedId }` | `user.block` |
| `POST` | `/user/contacts` | `JWT` | — | `{ query, customFirstName, customLastName }` | `contact.add` |
| `GET` | `/user/contacts` | `JWT` | `search`, `cursor`, `limit` (Query) | — | `contacts.list` |
| `PATCH` | `/user/contacts/:contactUserId` | `JWT` | `contactUserId` (Param) | `{ ... }` *(editContactDto)* | `contacts.edit` |
| `DELETE` | `/user/contacts/:contactUserId` | `JWT` | `contactUserId` (Param) | — | `contacts.remove` |

### Notification Controller (`/notifications`)

| Method | Endpoint | Auth | Params / Query | Body | Event |
| :--- | :--- | :---: | :--- | :--- | :--- |
| `GET` | `/notifications/notifications` | `JWT` | `page`, `limit` (Query) | — | `notifications.check` |

### Chat Controller (`/chat`)

| Method | Endpoint | Auth | Params / Query | Body | Event |
| :--- | :--- | :---: | :--- | :--- | :--- |
| `POST` | `/chat/create-group` | `JWT` | — | `groupDto` | `group.create` |
| `POST` | `/chat/create-direct` | `JWT` | — | `{ userId }` | `direct.create` |
| `POST` | `/chat/add-member` | `JWT` | — | `{ roomId, memberId }` | `group.add` |
| `POST` | `/chat/remove-member` | `JWT` | — | memberDto | `group.remove` |
| `POST` | `/chat/users-status` | `JWT` | — | `{ userIds: string[] }` | `users-status.check` |
| `POST` | `/chat/message-seen` | `JWT` | — | `{ roomId, messageIds: string[] }` | `message.seen` |
| `PUT` | `/chat/room-mute` | `JWT` | — | `{ roomId, durationMinutes }` | `room.mute` |
| `GET` | `/chat/user-rooms` | `JWT` | — | — | `rooms.fetch` |
| `POST` | `/chat/join-room` | **Public** ⚠️ | — | `{ roomId }` | `room.join` |
| `GET` | `/chat/rpc-token` | `JWT` | `roomId` (Query) | — | `group-rtc.token` |
| `GET` | `/chat/user-calls` | `JWT` | `page`, `limit` (Query) | — | `calls.list` |
| `GET` | `/chat/:roomId/messages` | `JWT` | `roomId` (Param)<br>`messageId`, `limit` (Query) | — | `room.messages` |
| `GET` | `/chat/:roomId/messages/search` | `JWT` | `roomId` (Param)<br>`q`, `limit`, `cursor` (Query) | — | `room.messages.search` |
| `GET` | `/chat/messages/search` | `JWT` | `q`, `limit` (Query) | — | `user.messages.search` |
| `GET` | `/chat/rooms/search` | `JWT` | `q`, `limit` (Query) | — | `rooms.search` |
| `POST` | `/chat/channel/create` | `JWT` | — | `{ name, avatar }` | `channel.create` |
| `POST` | `/chat/channel/add-member` | `JWT` | — | `{ roomId, memberId }` | `channel.add` |
| `DELETE` | `/chat/channel/remove-member` | `JWT` | — | `{ roomId, memberId }` | `channel.remove` |

### API Key Controller (`/api-key`)

| Method | Endpoint | Auth | Params / Query | Body | Event |
| :--- | :--- | :---: | :--- | :--- | :--- |
| `POST` | `/api-key/create-key` | `JWT` | — | `{ title, allowedDomain }` | `api_key.create` |

### Support Controller (`/support`)

| Method | Endpoint | Auth | Params / Query | Body | Event |
| :--- | :--- | :---: | :--- | :--- | :--- |
| `GET` | `/support/messages` | `API Key` | `roomId`, `visitorId` (Query) | — | `support.messages` |
| `GET` | `/support/admin/messages` | `JWT` | `roomId` (Query) | — | `support.messages` |
| `GET` | `/support/admin/rooms` | `JWT` | `page`, `limit` (Query) | — | `support.rooms` |


### Environments

```bash
REDIS_HOST=localhost
REDIS_PORT=6379
JWT_SECRET=adsfljewifsdfkljlio
JWT_EXPIRATION="1h"
```