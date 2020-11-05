# Cloud-Native SD-WAN (CNWAN) Project

The Cloud-Native SD-WAN project (CNWAN) bridges the gap between SD-WAN technologies and Cloud-Native applications running on Kubernetes.

The documentation for the CNWAN project is a work in progress. Please contact us at [cnwan@cisco.com](mailto:cnwan@cisco.com) with any questions.

If you are new to the project a good place to start is by reading the [Overview](#overview) and/or checking some of the resources below. Once you are familiar with the project, feel free to explore the different components described in the [Architecture](#architecture). There is also some [Automation](https://github.com/CloudNativeSDWAN/cnwan-automation) available that should help bootstraping things.

* "Cloud-Native SD-WAN: The WAN Your Kubernetes Applications Deserve" - Cisco Blogs [[post](https://blogs.cisco.com/networking/introducing-the-cloud-native-sd-wan-project)]
* "Network, Please Evolve: Chapter 3, Stretching Out" - Cisco Keynote @ KubeCon EU 2020 [[video](https://www.cisco.com/c/en/us/training-events/events/kubecon-europe.html?socialshare=lightbox_video1_keynote)]
* CNWAN demo @ KubeCon EU 2020 [[video](https://www.cisco.com/c/en/us/training-events/events/kubecon-europe.html#~demos-and-presentations)]
* "CN-WAN: a Cloud Native (SD-)WAN for Microservice Applications" - Presentation at NSMCon EU 2020 [[video](https://www.youtube.com/watch?v=C28_WTyT-KI)]


## Overview

Nowadays, access to applications hosted in Kubernetes across Wide Area Networks (WANs) is a standard pattern for Enterprise apps. At the same time, Software-Defined WAN (SD-WAN) technologies are becoming popular since they democratize WAN access patterns across the Internet through latency reduction, throughput improvement, and packet loss prevention. Unfortunately, there is not much integration between SD-WAN and Kubernetes, in most cases (if not all) Kubernetes and SD-WAN are like ships in the night.

Interestingly, most SD-WAN solutions offer APIs that allow to programmatically influence how the traffic is handled over the WAN. This enables interesting opportunities for automation and application optimization. There is an opportunity to pair the declarative nature of Kubernetes with the programmable nature of modern SD-WAN solutions to automatically optimize application experience over the WAN.

![CNWAN Integration](img/cnwan-overview.png)

With that goal, the Cloud-Native SD-WAN (CNWAN) project offers a reference implementation for how SD-WAN controllers can use Kubernetes application metadata to optimize application WAN traffic and link Kubernetes application attributes with SD-WAN network capabilities. 

CNWAN focuses on enabling Kubernetes-SDWAN integration while minimizing disruptions to existing DevOps and NetOps workflows. In a typical enterprise today, a DevOps team configures and operates the Kubernetes infra and another NetOps team setups and maintains the SD-WAN connectivity. Because today the two infrastructures are agnostic to one another, the two teams need to go through manual co-ordination to deliver optimal application experience. By using CNWAN that is no longer the case.

The CNWAN project presents DevOps teams with the possibility to adapt their workflows when deploying Kubernetes-hosted apps to define WAN attributes along with the rest of the app configuration and metadata. At the same time, CNWAN offers patterns for publishing those apps via service discovery systems and connect those to SD-WAN controllers. NetOps can then configure the SDWAN controller (along with a CNWAN adaptation layer) to make it to automatically receive the application metadata and optimize the application flows as they traverse the WAN. This reduces the need for manual coordination between the two teams and creates a more dynamic and efficient app experience across WAN connections.

## Architecture

![CNWAN Architecture](img/cnwan-arch.png)

The CNWAN project is composed of three main components that together enable the integration between Kubernetes and SD-WAN:

- **[CNWAN Operator](https://github.com/CloudNativeSDWAN/cnwan-operator)**: A Kubernetes operator that monitors externally exposed services deployed in the Kubernetes cluster looking for WAN related metadata. DevOps deploying services on the cluster can use annotations in the form of (for instance) "cnwan.dev/traffic=video" to specify the type of traffic that the SD-WAN can expect for that particular service. The CNWAN Operator extracts the externally exposed IP address and port for the services as well as the associated WAN metadata and makes it all available through an external Service Registry. 

- **[CNWAN Reader](https://github.com/CloudNativeSDWAN/cnwan-reader)**: The Reader connects to the Service Registry to learn about how Kubernetes is exposing the services and their associated WAN metadata, as extracted by the CNWAN operator. The CNWAN Reader periodically polls the Service Registry and keeps track of updates regarding the services exposed and/or the metadata associated. When it detects new (or updated) services or metadata, it sends a message towards the CNWAN Adaptor so SD-WAN policies can be updated.

- **[CNWAN Adaptor](https://github.com/CloudNativeSDWAN/cnwan-adaptor)**: The Adaptor is listening for updates from the CNWAN Reader and connects with a given SD-WAN controller to translate the service metadata into SD-WAN policies. NetOps team can configure policies in the SD-WAN controller regarding the WAN metadata for the services (e.g. "services with video traffic should go through this link") and then let the CNWAN Adaptor automatically populate the IP address and port for each of the services that should be treated by the policy.