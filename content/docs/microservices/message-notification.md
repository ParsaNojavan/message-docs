---
title: "Message Notification"
weight: 4
---

### Responsibility

- Handles message notification

- Handles ended and missed calls notifying  

- Handles seen notifications

- Handles send otp code for verification


### Setup

- You need to initial the value of access token secret keys in .env file

{{< callout type="info" >}}
  [User Microservice](/docs/microservices/message-user) is connected to notification ms for emmiting otp, it emmits otp code generated during login process to the event pattern: **notification.send-otp** 
{{< /callout >}} 

{{< callout type="default" >}}
  otp message service is and abstract class, so you can replace it with your favorite sms service that implements thisfollowing method:

  ```typescript
  abstract sendOtp(recipient: string, code: string, metadata?: Record<string, any>): Promise<void>;
  ```
{{< /callout >}} 


### Environments

```bash
REDIS_HOST=localhost
REDIS_PORT=6379
MONGO_STRING="mongodb://root:example@localhost:27017/notifdb?authSource=admin"
JWT_SECRET=adsfljewifsdfkljlio
JWT_EXPIRATION="1h"
JWT_REFRESH_SECRET=ldksjfsdl
JWT_REFRESH_EXPIRATION="1h"
```