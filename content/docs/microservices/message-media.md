---
title: "Message Media"
weight: 3
---



### Responsibility

- Handles media upload

- Returns metadata of uploaded blob and stores in message model in chat ms

- creates thumbnails for uploaded files using sharp and ffmpeg

- streams files and thumbnails for live access


### Setup

- You need to initial the value of access token secret keys in .env file

- You should also create a folder for storing files and introduce it in .env

{{< callout type="info" >}}
  The file upload service is and abstract class, this means you can change it to your own blob storage service that iplements these 3 following methods:

```typescript
abstract upload(file : Express.Multer.File): Promise<DataResultDto<any>>;
abstract download(filePath : string): Promise<any>;
abstract delete(filePath: string): Promise<DataResultDto<any>>;
```

{{< /callout >}} 

{{< callout type="default" >}}
  This microservice is also an API, so you need to cinfigure a free port to run on it in .env file
{{< /callout >}} 

### Media Controller (`/media`)

| Method | Endpoint | Auth | Params / Query |
| :--- | :--- | :---: | :--- |
| `POST` | `/media/upload` | `JWT` | — | — |
| `GET` | `	/media/download/:id` | `JWT` | `id` (Param) |
| `GET` | `/media/:id/stream` | `JWT (Token Query)` | `id` (Param), `token` (Query), `range` (Header) |


### Environments

```bash
REDIS_HOST=localhost
REDIS_PORT=6379
MONGO_STRING="mongodb://root:example@localhost:27017/mediadb?authSource=admin"
JWT_SECRET=adsfljewifsdfkljlio
JWT_EXPIRATION="1h"
LOCAL_STORAGE_ROOT_DIR="./storage"
PORT=3001
```

### Flow

```mermaid
flowchart TD
A[Client Upload] --> B{Media Service}
B -->|valid?| C[Save to Local Storage]
B -->|invalid| Z[400 Error]
C --> D[Create thumbnail via sharp/ ffmpeg]
D --> F[Store metadata in MongoDB]
F --> G[Return DataResultDto]
G --> A
```