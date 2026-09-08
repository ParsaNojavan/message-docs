---
title: "Message User"
prev: "microservices"
weight: 1
---


### Responsibility

- User authentication and generating jwt tokens

- User profile and editing user details

- Creating API Keys for support service

- Managing user's contacts and blocked users


### Setup

- You need to initial the value of refresh token and access token secret keys in .env file

- you will also need to generate a private key with open ssl in keys folder

{{< callout type="warning" >}}
  To create the **private key** run the code below in terminal  

  ```bash
    openssl genrsa -out private.key 2048
  ```
  put the generated file in this location :

  ```bash
    keys / private.pem
  ```

{{< /callout >}} 

### Environments

```bash
REDIS_HOST= localhost
REDIS_PORT= 6379
MONGO_STRING= "mongodb://root:example@localhost:27017/usersdb?authSource=admin"
JWT_SECRET= adsfljewifsdfkljlio
JWT_EXPIRATION= "1h"
JWT_REFRESH_SECRET= ldksjfsdl
JWT_REFRESH_EXPIRATION= "1h"
```