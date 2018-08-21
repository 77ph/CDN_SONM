CDN step by step
=====================

**SONM** :  decentralized distributed network of computer resources that has its own marketplace. You can use SONM to sell free resources or purchase the ones you need. Resources are traded for SONM tokens. SONM resources can be rented at any time and attractive in price, so it&#39;s convenient for them, for example, to connect edge CDN servers to the CDN control panel, in case you need additional servers for a limited time.

     

**CDN** : Content distribution network. The best practices to construct CDN you need is Lego approach. You choose components and solutions  most suitable for your tasks.

For the all the components of the CDN, virtualization in any form makes sense only for edge servers, that distribute traffic to end users. The core and origins are best to have as bare metal  in the property or long-term lease.

The key component of the system is the streaming server (we are talking only about audio or video delivery), there are two well-known solutions on the market and both are not open source, unfortunately. Open source solutions also exist (ffmpeg for example), but they are not normally used in real business projects. In the pilot, we used the[nimble server](https://wmspanel.com/nimble).

    

Infrastructure Elements
=======================

- [**WMS Panel**](https://wmspanel.com/) - control panel CDN (Statistics, clients, services, flows, etc.)

- **Origin** - Nimble server or wowza. Storage server content. Distributes traffic only for Edges.
- **Redirector** -  Manages scenarios of traffic distribution to end users according to specified criteria.
- **Edge** - distributes traffic to end users.

    
 ![Standart CDN Scheme](https://lh6.googleusercontent.com/c8XmTH4G2oLfNvu9dNiEpnk7MKnpgQQ3ycea5YG3iXOv6xdGFJWok-YkftyWXGjA_yGD-cJjog0rqtlqQ6FXvyKovbeJTbBg4yplsAZ14y9YiPOv7bscaPn1WS_sTV7_xQcGwXy6)
   
Step by step inatructions
=========================
1. Register in [**WMS Panel**](https://wmspanel.com/). Registration is easy and you get two weeks free trial. **WMS Panel** is software for control all proucesses of CDN bussines and CDN network infrastrucure.
2. Create at least one **Origin**. Origin or ingest server needed for store audio and video content for streaming to users. You can use [**nimble server**](https://wmspanel.com/nimble) or [**Wowza**](https://www.wowza.com) for that. Origins often used as a transcoders and transmuxsers. Because of that you need very stable server or two (for redundance).
3. Setup network of **Edges**. Edge is the key element of the non critical CDN infrastructure. Usually Edges are geographically distributed to provide high video and audio quality for users. You may use nimble server or wowza for edges too. Edges are not so demanding on the hardware and you use any suitable VPS of course. As a new solution of infrastructure for edges network we represent SONM - another poossibility to scale your CDN network. We prepare [**docker contreiner with nimble**](https://github.com/77ph/docker-nimble) especially  for this case.


**How to install and use Nimble streamer on SONM network for use in your  WMS panel.**

You must install the customer software according to [SONM instructions](https://docs.sonm.com/getting-started/as-a-consumer).

- Go to the SONM marketplace and select the appropriate ask orders for sale. All further actions are performed using SONM CLI

- Make a purchase using the command **sonmcli deal quick-buy <ask_id> [duration] [flags].** Check that the resources match the parameters of the tape drive and your tasks (the presence of an external IP, the necessary bandwidth).

- Install the corresponding docker container using the task.yaml file with the command **sonmcli task start <deal_id> <task.yaml> [flags].** Example of a task.yaml file for a container docker with Nimble streamer:

      container:

      image: docker.io/77phnet/nimble:latest

      expose:

        - 8081: 8081

        - 1935: 1935

        - 8082: 8082

      env:

        WMSPANEL\_USER: user e-mail

        WMSPANEL\_PASS: user password

        WMSPANEL\_TOKEN: token

Here you must insert the details, corresponding to your panel.

Once launched, Nimble streamer should appear in your panel and is available for use. Make additional settings for addresses and ports in the panel and you can connect streaming routes.

To complete the task or change the settings, run the command **sonmcli task stop <deal_id> <task_id> [flags].** In this case, you can run a new task.yaml file

To stop working with the resource, run the command **sonmcli deal close <deal_id> [flags].** In this case, the lease of the resource will cancel.

4. Install [**Redirector**](https://github.com/77ph/load-balancer). Redirector or load or Geo or ...  balacncer is the special CDN sofrware for traffic distribution to provide fastest content delivery to user. At the beginning can use this or create your own. Represented redirector is just simple PHP code. Do not forget to add all your edges to code according instructions.

