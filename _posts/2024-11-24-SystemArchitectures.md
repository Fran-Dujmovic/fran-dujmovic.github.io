---
title: System Architectures
description: Ignition system architectures and their respective implementations.
author: Fran_Dujmovic
date: 2024-11-24 11:00:00 +0200
categories: [System Architectures]
pin: true
---
## Basic Architecture

### Single Server Install
Ignition's most typical architecture is a **basic single installation**. This is very simple to set up and it provides the powerful functionality of the Ignition Gateway and it can serve as a starting point for more complicated architectures. The **Basic Architecture** consists of a single Gateway that can connect to multiple PLCs, databases, and other devices, all with a single licensed install.


![Desktop View](https://www.docs.inductiveautomation.com/assets/images/basic-Architecture-Model-5dcf196d0ffa8d52a43a0b63fcd7bd21.png){: width="600" height="300" }
_Single Server Install_

### One Server, Multiple Networks
The Ignition Gateway supports **dual-NIC servers**, and can act as a bridge between multiple networks, or communicate with multiple sites over a corporate WAN. Since clients talk to databases and PLCs through the Gateway, **clients can be launched from both a corporate network and an isolated control network**, and provide full access to both.

![Desktop View](https://www.docs.inductiveautomation.com/assets/images/basic-Architecture-Multiple-Network-Model-71c8e62c37677d7a8fc6a27dad4bbafe.jpg){: width="600" height="300" }
_One Server Connected To Multiple Networks_

## Scale Out Architecture

In the Scale Out Architecture, we have at least one Gateway that handles back-end communications. The **back-end Gateway** deals with all PLC and device communications. The **front-end Gateway** handles all of the Clients, serving up the data pulled from the back-end Gateway. This is made possible through the **Gateway Network**, connecting Gateways to each other, and allowing tags to be shared through remote Tag Providers.

![Desktop View](https://www.docs.inductiveautomation.com/assets/images/Scale-Out-Architecture-Model-ce5739ddcb5a6b1053a92f1979e9071d.png){: width="600" height="300" }
_Scale Out Architecture Model_

### Load Balancing

The best thing about the Scale Out Architecture is that it is **easy to scale up Ignition as your system grows**. In the image below, more front-end Gateways are added to help handle an increase in clients, and a Load Balancer to automatically distribute the clients between them.

![Desktop View](https://www.docs.inductiveautomation.com/assets/images/Scale-Out-Architecture-Load-Balancer-e7c0151141963097beff58399f7ea0aa.png){: width="600" height="300" }
_Load Balancing Model_

### Gateway Types

When utilizing Scale Out, an Ignition Gateway is given focus due to its placement. There are two possible types:

1. **Back-End Gateway**: This gateway is responsible for communicating with the PLCs, and executing Tags. They share their local Tags with the Front-End Gateways via a **Remote Tag Provider**, and are generally responsible for recording history. Additionally, alarms can be added to Tags locally, and exposed elsewhere in the system. 
2. **Front-End Gateway**: This Gateway is responsible for **hosting the clients**, allowing the Back-End Gateways to focus solely on PLC communication and history.

With this type of architecture, both types of Gateways will need database access: the Back-End Gateways need to be able to store data, while the Front-End Gateways need to query the data out. Multiple on-site databases may also be utilized with this architecture. This typically involves configuring a database cluster.

### Benefits of using the Scale Out Architecture

1. **Decentralized Servers** - In this architecture, each server only deals with a small part of the overall system. Should a server fault, only that part of the system is hindered, allowing other parts in organization to continue uninterrupted. Applying Redundancy to the Back-End servers is ideal to maximize uptime.
2. **Pooled Resource Usage** - Since multiple servers are working together, bottlenecks are reduced. A massive amount of work can be performed in this system without slowing down other servers.
3. **Easy Growth** - Improving performance in this architecture is simple: add another Gateway where appropriate. Because the system is spread out on purpose, it's easy to determine which kind of Gateway to add. A Back-End Gateway would help if you're adding new PLCs, and a Front-End Gateway will help when more clients need to be active at a time.

## Hub and Spoke Architecture

The **Hub and Spoke Architecture** involves multiple Ignition installations (**the spokes**) communicating and forwarding data points to a centralized Ignition installation (**the Hub**). The Hub and Spoke Architecture is unique in that you start with a **single Ignition install at a central site**. Ignition can then be installed at as many other remote or local sites as needed. These sites can then be customized or configured to suit that specific site's needs.

![Desktop View](https://www.docs.inductiveautomation.com/assets/images/Hub-and-Spoke-Architecture-Model-88caa02f57bd5fd30531cdae117fb9f4.png){: width="600" height="300" }
_Hub and Spoke Architecture Model_

### Benefits of using the Hub and Spoke sytem architecture

1. **Reliable Remote Data Logging**: Because connections between a database and remote PLCs can be unreliable, we can install either the **SQL Bridge module** or **Tag History module**. The Store-and-Forward system then ensures that data is never lost. The system may still be accessed through the OPC-UA server built into ignition, or database-based Tags may be used to communicate current status. By making use of Ignition's Store-and-Forward system, data loss is prevented, ensuring that history always reaches its destination in the database. The Hub and Spoke Architecture is ideal for **centralizing historical data from multiple remote sites**, and allows access to locally collected data via Ignition's Gateway Network.
2. **Local and Remote Logging**: The Hub and Spoke Architecture can be modified to store data at each **spoke**, in addition to the hub by splitting the **Tag history**. Redundant data is stored at each site as well as the hub database to protect against hard drive failure.
3. **Local Client Feedback**: Typically, the Hub and Spoke Architecture does **not include** the Vision Module at the remote sites. You can add the Vision Module at each spoke to take advantage of local client fallback. This way, each site can continue to operate at full efficiency in the event communication with the hub Gateway drops. Users at each spoke will never lost visibility of the system. **Vision Limited** licenses help lower costs on the spokes.
4. **Remote Site Alarming**: By adding the **Alarming Notification module** to a remote site, you can notify users when a problem occurs at each spoke. It is best to handle alarms at each remote site rather than centrally, to avoid any problems in case the connection to the central location is lost.

## Edge Architectures

**Edge** is a smaller version of the standard Ignition install. Edge is used to sit at the edge of a network and do a few specific tasks like client failover or push **MQTT data**. Up to 35 days or 10 million data points of history can be stored locally with an Edge installation.

Each Edge product (**Panel and IIoT**) offers specific functionality that works best in certain scenarios. The diagrams show some architecture options using Edge. Additionally, these architecture diagrams show an Edge Gateway paired with an Ignition Gateway, which shows how an Edge system architecture can be added on to any of the other Ignition architectures.

### Panel

Edge with the **Panel product** can act as a stand-alone panel by simply connecting to a local PLC and building a basic screen. When paired with a central Ignition Gateway, the Edge can also act as a **local client fallback**, providing control on site in the event of a lost network connection.

![Desktop View](https://www.docs.inductiveautomation.com/assets/images/edge-panel-basic-architecture-f2e3a2c947cac9f228b11f6ec12200e1.png){: width="600" height="300" }
_Edge Panel Basic Architecture_

![Desktop View](https://www.docs.inductiveautomation.com/assets/images/edge-panel-with-eam-8c404e247641998e054352d978eca3ad.png){: width="600" height="300" }
_Edge Panel With Basic EAM_

### Edge IIOT

Edge with the IIoT product (formerly **MQTT**) provides a simple way of setting up an **MQTT Publisher** next to a PLC, allowing it to connect to a greater IIoT architecture. Here you can see multiple types of devices using Ignition Edge to connect in the network.

**Edge IIoT as a standalone Edge Gateway** to publish device data from the edge of the network to an MQTT broker:

![Desktop View](https://www.docs.inductiveautomation.com/assets/images/standalone-edge-iiot-with-mqtt-cb31f036fd98fd60ea572d50c642e7d1.png){: width="600" height="300" }
_Standalone Edge IIOT with MQTT Module_

**Edge IIoT as an Edge Gateway** to publish device data from the edge of the network to an MQTT broker:

![Desktop View](https://www.docs.inductiveautomation.com/assets/images/edge-gateway-iiot-with-mqtt-c81d63be04903fcf1f211e6ab7dbe54f.jpg){: width="600" height="300" }
_Edge Gateway IIOT with MQTT Module_

**Edge IIoT and Edge Panel together** to add a local client at the edge of the network and to publish data to an MQTT broker:

![Desktop View](https://www.docs.inductiveautomation.com/assets/images/edge-iiot-and-panel-6eb25bb8dfd51309f97b108597b9a72d.jpg){: width="600" height="300" }
_Edge IIOT and Panel Module_

Shared Functionality:

1. **Ignition Edge Sync Services**, included with either the Edge IIoT or Panel product, acts as a limited remote server that synchronizes data from the edge of the network to a central Ignition server.
2. **Edge EAM**, also included with either the Edge IIoT or Panel product, acts as a remote EAM Agent Gateway and can easily combine with any other Edge edition for central management.

## IIoT Architecture

Ignition can collect data from any devices at the edge of the network, publish that data to a central broker, and push that data to subscribed industrial and line-of-business applications. **Ignition IIoT** can connect to PLCs in the field through the use of the **MQTT Transmission module**, field devices with Ignition 

Edge MQTT installed, and/or MQTT-enabled edge gateways and field devices that use the Cirrus Link Sparkplug MQTT specification. This data is published to an **MQTT broker**, this broker can be located on-premise, in the cloud, or a hybrid of the two. The MQTT Engine module located on an Ignition Gateway can subscribe to any data published from the broker. This data can be used in any Ignition application.

### IIoT Architecture Example

IIoT architecture typically involves the use of the **MQTT protocol** to communicate with a large number of devices. MQTT is a lightweight and secure protocol that makes use of a unique **publish/subscribe transportation method**.

The **Basic Architecture** can easily be expanded with the MQTT modules to collect data from numerous Edge of Network devices. Data collected by the MQTT Engine can be historized and presented in a client with the **Vision module**, or via reports with the **Reporting module**.

![Desktop View](https://www.docs.inductiveautomation.com/assets/images/IIoT-Archictecture-Model-06d547375f0f1cebab47cf7d8b4c8fa1.png){: width="600" height="300" }
_IIoT Architecture Model_

The following are key pieces in the IIoT Architecture:

1. **Field Device**: A Field Device of some sort can be used to connect to a PLC. This device also needs to be able to publish Tag values to an MQTT server. Any of the following hardware can act as a Field Device:
   - Ignition Edge with the MQTT product.
   - Ignition Server with the MQTT Transmitter Module.
   - Third-Party MQTT device.
2. **MQTT Server (Broker)**: An MQTT Broker is a server that can communicate with your Field Devices via MQTT. This can be an Ignition Gateway with the MQTT Distributor Module installed, or a third-party server, such as a Chariot MQTT Server.
3. **Subscriber**: A subscriber is some application that subscribes to topics in the Broker. In regards to Ignition, this is the Gateway with the the MQTT Engine Module installed.

## Enterprise Architecture

**Enterprise Architecture** typically involves having a mechanism to monitor and manage multiple Ignition Gateways. This involves utilizing the **Enterprise Administration Module (EAM)**.

**EAM** specifies one Gateway as the **Controller**, and the others as **Agents**. Connections between these Gateways take advantage of the Gateway Network, which is a secure method multiple Gateways can use to share information.


![Desktop View](https://www.docs.inductiveautomation.com/assets/images/Enterprise-Architecture-Model-f992230d2c0602f2952924d7f42048aa.png){: width="600" height="300" }
_Enterprise Architecture Model_

### Benefits of using Enterprise Architecture

1. **System Management** - EAM streamlines system management by having each Gateway report to a single Controller Gateway. This Controller can then generate alarms when problems occur, or assist in recovering from a hardware failure. Agent events can be stored in a SQL database, and easily incorporated into an Ignition Report. EAM can easily be incorporated into any architecture where multiple Gateways are present.
2. **Project Synchronization** - When multiple Gateways are used to launch clients, such as in the Hub and Spoke Architecture, standardizing the look and feel of projects across both Gateways is important for a consistent organization. With EAM, the Controller can synchronize resources across multiple Agents, allowing development to occur on one Gateway, and updates pushed out to other Agent automatically.

## Redundancy Architecture

Using **Redundancy**, two Ignition installations can be linked together, so if one server fails, the other takes over and continues executing. All of the clients connected will be redirected to the backup machine, and historical data will continue to be logged. The transition is seamless, meaning **processes will never be prevented from executing due to one server going down**.

Any single installation in the System Architectures can be replaced with a redundant pair of Ignition Gateways. If you have a dedicated backup computer, Inductive Automation offers a cheaper Backup-specific license.

![Desktop View](https://www.docs.inductiveautomation.com/assets/images/Mission-Critical-Redundancy-Diagram-f1016442e958e28ac2b970c02f7afa9f.png){: width="600" height="300" }
_Redundancy Diagram_

### Examples

**Standard Architecture**

![Desktop View](https://www.docs.inductiveautomation.com/assets/images/Standard-Architecture-With-Redundancy-Model-fe766b42ccdf623452ff7bfdda879458.jpg){: width="600" height="300" }
_Standard Architecture with Redundancy_

**Hub and Spoke Architecture**

![Desktop View](https://www.docs.inductiveautomation.com/assets/images/Hub-And-Spoke-Architecture-With-Redundancy-Model-a577f445e8070dca1d949fb8f6071e70.jpg){: width="600" height="300" }
_Hub and Spoke Architecture with Redundancy_

**Scale Out Architecture**

![Desktop View](https://www.docs.inductiveautomation.com/assets/images/Scale-Out-Architecture-With-Redundancy-Model-d57bcfd378b6922f4328244bb2101d8e.jpg){: width="600" height="300" }
_Scale Out Architecture with Redundancy_

**Scale Out Architecture with Load Balancer**

![Desktop View](https://www.docs.inductiveautomation.com/assets/images/Scale-Out-Architecture-Load-Balancer-Redundancy-Model-4da6776b37a1271e167a972c5cd8c0a3.jpg){: width="600" height="300" }
_Scale Out Architecture with Load Balancer Redundancy_

## Cloud Based Architecture

Ignition server can be hosted in the **Cloud**. The server can connect to the PLCs at the remote or customer sites via Round Robin Poll, Cell Tower, Satellite, or Secure VPN. The clients are connected to the Ignition server via Internet.

In this example, we see the **Basic Architecture**, but with the **Ignition Server and Database both running on cloud servers**. Note, that the Database and Ignition are on different cloud servers. This is because cloud service providers generally have specialized services that are designed explicitly for hosting SQL databases, as well as more general purpose services such as running Ignition. An installation of **Ignition Edge** is placed local to the PLCs, and is responsible for forwarding data to the cloud.

![Desktop View](https://www.docs.inductiveautomation.com/assets/images/Cloud-Architecture-With-Edge-a7039b0b34e28f9b6adc4d9229afec95.png){: width="600" height="300" }
_Cloud Architecture with Edge_

This type of architecture is ideal in smaller organizations since you don't have to worry about maintaining a server. Additionally, there is no on-site IT staff required to monitor the system. 

If you are using redundancy or are concerned about uptime, you should be using Ignition redundancy regardless of whether your Ignition is in the cloud. While most cloud providers offer built-in redundancy, this is often much slower than Ignition's redundancy failover time. Typically Cloud Server redundancy has a new computer instance spun up (started) in the event the original fails.

If you are using Ignition's Redundancy with a Cloud Server, then typically we recommend that both of the servers are run side-by-side. This means either both in the cloud or both on site. One warning: If you have both instances of Ignition in the cloud but if your connection to the internet goes down, then you lose access to both instances of Ignition.

**Cloud Service Suggestions**: 
1. Amazon Web Services (AWS)
2. Microsoft Azure



























