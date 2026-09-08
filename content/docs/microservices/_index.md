---
title: "microservices"
prev: "get-started"
next: "message-user"
weight: 2
---

<div class="hx-mb-12">
{{< hextra/hero-subtitle >}}
  microservices document
{{< /hextra/hero-subtitle >}}
</div>

{{< callout type="default" >}}
Important Info:

<div class="hx-mb-12">
{{< hextra/hero-subtitle >}}
  Each microservice is commiunicated to others using redis message broker,
  you can change it to mqtt, rabbit mq, tcp, ... in main.ts transport option
{{< /hextra/hero-subtitle >}}
</div>


{{< /callout >}}

{{< callout type="warning" >}}
Warning:

<div class="hx-mb-12">
{{< hextra/hero-subtitle >}}
  Be Carefull! If you change transport option, you will still need a redis client for pub sub and chaching, so you still have to init connection string to redis in .env file
{{< /hextra/hero-subtitle >}}
</div>


{{< /callout >}}

<br/>