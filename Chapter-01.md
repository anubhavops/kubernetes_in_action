# CH-01 Introducing Kubernetes

POINTS COVERED - 

- Understanding how software development and deployment has changed over recent years
- Isolating applications and reducing environment differences using containers
- Understanding how containers and Docker are used by Kubernetes
- Making developers’ and sysadmins’ jobs easier with Kubernetes 

> Kubernetes is Greek for pilot or helmsman (the person holding the ship’s steering wheel)

Years ago, most software applications were big monoliths, running either as a single process or as a small number of processes spread across a handful of servers. They have slow release cycles hence updates infrequently. Dev. packages up the whole system and hands ovet to the OPS team which then deploys and monitor it, in case of hardware failure ops team manually migrated it to the remaining healthy server.

Now monolithic apps are being broken down in to smaller, indepedently running components called microservices, as they are decoupled from each other they can be developed, deployed, updated and scaled individually. 


Chapter 1. Introducing Kubernetes
This chapter covers

Understanding how software development and deployment has changed over recent years
Isolating applications and reducing environment differences using containers
Understanding how containers and Docker are used by Kubernetes
Making developers’ and sysadmins’ jobs easier with Kubernetes
Years ago, most software applications were big monoliths, running either as a single process or as a small number of processes spread across a handful of servers. These legacy systems are still widespread today. They have slow release cycles and are updated relatively infrequently. At the end of every release cycle, developers package up the whole system and hand it over to the ops team, who then deploys and monitors it. In case of hardware failures, the ops team manually migrates it to the remaining healthy servers.

Today, these big monolithic legacy applications are slowly being broken down into smaller, independently running components called microservices. Because microservices are decoupled from each other, they can be developed, deployed, updated, and scaled individually. This enables you to change components quickly and as often as necessary to keep up with today’s rapidly changing business requirements.

But with bigger numbers of deployable components and increasingly larger datacenters, it becomes increasingly difficult to configure, manage, and keep the whole system running smoothly. It’s much harder to figure out where to put each of those components to achieve high resource utilization and thereby keep the hardware costs down. Doing all this manually is hard work. We need automation, which includes automatic scheduling of those components to our servers, automatic configuration, supervision, and failure-handling. This is where Kubernetes comes in.

Kubernetes enables developers to deploy their applications themselves and as often as they want, without requiring any assistance from the operations (ops) team. But Kubernetes doesn’t benefit only developers. It also helps the ops team by automatically monitoring and rescheduling those apps in the event of a hardware failure. The focus for system administrators (sysadmins) shifts from supervising individual apps to mostly supervising and managing Kubernetes and the rest of the infrastructure, while Kubernetes itself takes care of the apps.

Kubernetes abstracts away the hardware infrastructure and exposes your whole datacenter as a single enormous computational resource. It allows you to deploy and run your software components without having to know about the actual servers underneath. When deploying a multi-component application through Kubernetes, it selects a server for each component, deploys it, and enables it to easily find and communicate with all the other components of your application.


## 1.1  Understanding the need for a system like Kubernetes 

### 1.1.1 Moving from monolithic apps to microservices

Monolithic application has components tightly bound which are developed, deployed and managed as one entity running as one single OS Process. Changes in one components leads to deployment of whole application and hence increases the complexity of application and leads to deterioration of quality as inter-dependencies betweens the parts are increased over the time.

TO run monolithic apps we need few servers with powerful configuration and as load increases we can either SCALE UP os SCALE OUT, SCALE Up overtime increases the cost whereas SCALE OUT is cheap but for that we need to run multiple replicas of the components on servers which sometimes becomes impossible due to the components like RELATIONAL DB can't be scale horizontally easily leading to whole monolithic application can't be scale out.

#### Splitting apps into microservices 

Hence splitting of complex monolithic application in to smaller independently deployable components called microservices has started. 

Each microservice runs as an independent process (see figure 1.1) and communicates with other microservices through simple, well-defined interfaces (APIs).




