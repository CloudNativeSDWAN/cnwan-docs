# Cloud-Native SD-WAN (CNWAN) Project

The Cloud-Native SD-WAN project (CNWAN) bridges the gap between SD-WAN technologies and Cloud-Native applications running on Kubernetes

## Overview

Nowadays, access to applications hosted in Kubernetes across Wide Area Networks (WANs) is a standard pattern for Enterprise apps. At the same time, Software-Defined WAN (SD-WAN) technologies are becoming popular since they democratize WAN access patterns across the Internet through latency reduction, throughput improvement, and packet loss prevention. Unfortunately, there is not much integration between SD-WAN and Kubernetes, in most cases (if not all) Kubernetes and SD-WAN are like ships in the night.

Interestingly, most SD-WAN solutions offer APIs that allow to programmatically influence how the traffic is handled over the WAN. This enables interesting opportunities for automation and application optimization. There is an opportunity to pair the declarative nature of Kubernetes with the programmable nature of modern SD-WAN solutions to automatically optimize application experience over the WAN.

With that goal, the Cloud-Native SD-WAN (CNWAN) project offers a reference implementation for how SD-WAN controllers can use Kubernetes application metadata to optimize application WAN traffic and link Kubernetes application attributes with SD-WAN network capabilities. 

CNWAN focuses on enabling Kubernetes-SDWAN integration while minimizing disruptions to existing DevOps and NetOps workflows. In a typical enterprise today, a DevOps team configures and operates the Kubernetes infra and another NetOps team setups and maintains the SD-WAN connectivity. Because today the two infrastructures are agnostic to one another, the two teams need to go through manual co-ordination to deliver optimal application experience. By using CNWAN that is no longer the case.

The CNWAN project presents DevOps teams with the possibility to adapt their workflows when deploying Kubernetes-hosted apps to define WAN attributes along with the rest of the app configuration and metadata. At the same time, CNWAN offers patterns for publishing those apps via service discovery systems and connect those to SD-WAN controllers. NetOps can then configure the SDWAN controller (along with a CNWAN adaptation layer) to make it to automatically receive the application metadata and optimize the application flows as they traverse the WAN. This reduces the need for manual coordination between the two teams and creates a more dynamic and efficient app experience across WAN connections.

