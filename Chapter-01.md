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


![figure 1.1 Components inside a monolithic application vs. standalone microservices](https://user-images.githubusercontent.com/95487264/183063949-e183dd45-24f8-4196-a015-790c7f14fcc0.png)


Microservices communicate through synchronous protocols such as HTTP, over which they usually expose RESTful (REpresentational State Transfer) APIs, or through asynchronous protocols such as AMQP (Advanced Message Queueing Protocol). These protocols are simple, well understood by most developers, and not tied to any specific programming language. Each microservice can be written in the language that’s most appropriate for implementing that specific microservice.

A change to one of them doesn’t require changes or redeployment of any other service, provided that the API doesn’t change or changes only in a backward-compatible way.


#### Scaling microservices

In microservice we can individually scale the services that require scaling and leave out those those can't or don't need to.

![Figure 1.2. Each microservice can be scaled individually.](https://user-images.githubusercontent.com/95487264/183064558-f4310c21-1b57-411b-9228-55106219c7ce.png)


#### Deploying microservices

When the number of those components increases, deployment-related decisions become increasingly difficult because not only does the number of deployment combinations increase, but the number of inter-dependencies between the components increases by an even greater factor.

 When deploying them, someone or something needs to configure all of them properly to enable them to work together as a single system. With increasing numbers of microservices, this becomes tedious and error-prone, especially when you consider what the ops/sysadmin teams need to do when a server fails.

 Microservices also bring other problems, such as making it hard to debug and trace execution calls, because they span multiple processes and machines. Luckily, these problems are now being addressed with distributed tracing systems such as Zipkin.


 ####  Understanding the divergence of environment requirements

 Deploying dynamically linked applications that require different versions of shared libraries, and/or require other environment specifics, can quickly become a nightmare for the ops team who deploys and manages them on production servers. The bigger the number of components you need to deploy on the same host, the harder it will be to manage all their dependencies to satisfy all their requirements.

 ![Figure 1.3. Multiple applications running on the same host may have conflicting dependencies.](https://user-images.githubusercontent.com/95487264/183065552-d1f76a75-a666-47df-bddd-348ba9661fb5.png)



### 1.1.2. Providing a consistent environment to applications


biggest problems that developers and operations teams always have to deal with is the differences in the environments they run their apps in. Not only is there a huge difference between development and production environments, differences even exist between individual production machines. Another unavoidable fact is that the environment of a single production machine will change over time.

 system administrators give much more emphasis on keeping the system up to date with the latest security patches, while a lot of developers don’t care about that as much, and sysadmin handles PROD environment whereas developers work on their local machine.

 A production system must provide the proper environment to all applications it hosts, even though they may require different, even conflicting, versions of libraries.

 To reduce the number of problems that only show up in production, it would be ideal if applications could run in the exact same environment during development and in production so they have the exact same operating system, libraries, system configuration, networking environment, and everything else. You also don’t want this environment to change too much over time, if at all. Also, if possible, you want the ability to add applications to the same server without affecting any of the existing applications on that server.


 ### 1.1.3. Moving to continuous delivery: DevOps and NoOps

organizations are realizing it’s better to have the same team that develops the application also take part in deploying it and taking care of it over its whole lifetime. This means the developer, QA, and operations teams now need to collaborate throughout the whole process. This practice is called DevOps.

#### Understanding the benefits

Having the developers more involved in running the application in production leads to them having a better understanding of both the users’ needs and issues and the problems faced by the ops team while maintaining the app. Application developers are now also much more inclined to give users the app earlier and then use their feedback to steer further development of the app.

To release newer versions of applications more often, you need to streamline the deployment process. Ideally, you want developers to deploy the applications themselves without having to wait for the ops people. But deploying an application often requires an understanding of the underlying infrastructure and the organization of the hardware in the datacenter. Developers don’t always know those details and, most of the time, don’t even want to know about them.


#### Letting developers and sysadmins do what they do best

Developers love creating new features and improving the user experience. They don’t normally want to be the ones making sure that the underlying operating system is up to date with all the security patches and things like that. They prefer to leave that up to the system administrators.

The ops team is in charge of the production deployments and the hardware infrastructure they run on. They care about system security, utilization, and other aspects that aren’t a high priority for developers. The ops people don’t want to deal with the implicit interdependencies of all the application components and don’t want to think about how changes to either the underlying operating system or the infrastructure can affect the operation of the application as a whole, but they must.

Ideally, you want the developers to deploy applications themselves without knowing anything about the hardware infrastructure and without dealing with the ops team. This is referred to as NoOps. Obviously, you still need someone to take care of the hardware infrastructure, but ideally, without having to deal with peculiarities of each application running on it.

As you’ll see, Kubernetes enables us to achieve all of this. By abstracting away the actual hardware and exposing it as a single platform for deploying and running apps, it allows developers to configure and deploy their applications without any help from the sysadmins and allows the sysadmins to focus on keeping the underlying infrastructure up and running, while not having to know anything about the actual applications running on top of it.



## 1.2. Introducing container technologies

Kubernetes uses Linux container technologies to provide isolation of running applications.
We need to become familiar with the basics of containers to understand what Kubernetes does itself, and what it offloads to container technologies like Docker or rkt (pronounced “rock-it”).

### 1.2.1. Understanding what containers are 

#### Isolating components with Linux container technologies

developers are turning to Linux container technologies. They allow you to run multiple services on the same host machine, while not only exposing a different environment to each of them, but also isolating them from each other, similarly to VMs, but with much less overhead.


####  Comparing virtual machines to containers
 each VM needs to run its own set of system processes, which requires additional compute resources in addition to those consumed by the component’s own process.

 A container, on the other hand, is nothing more than a single isolated process running in the host OS, consuming only the resources that the app consumes and without the overhead of any additional processes.

 ![Figure 1.4. Using VMs to isolate groups of applications vs. isolating individual apps with containers](https://user-images.githubusercontent.com/95487264/183083680-af81ad1f-db82-4eb7-aec7-c0349cc42ff3.png)


When you run three VMs on a host, you have three completely separate operating systems running on and sharing the same bare-metal hardware. Underneath those VMs is the host’s OS and a hypervisor, which divides the physical hardware resources into smaller sets of virtual resources that can be used by the operating system inside each VM. Applications running inside those VMs perform system calls to the guest OS’ kernel in the VM, and the kernel then performs x86 instructions on the host’s physical CPU through the hypervisor.

> Two types of hypervisors exist. Type 1 hypervisors don’t use a host OS, while Type 2 do.


Containers, on the other hand, all perform system calls on the exact same kernel running in the host OS. This single kernel is the only one performing x86 instructions on the host’s CPU. The CPU doesn’t need to do any kind of virtualization the way it does with VMs (see figure 1.5).

