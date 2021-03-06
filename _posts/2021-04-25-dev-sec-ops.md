---
layout: post
title: DevSecOps and Operationalizing Kubernetes
date: 2021-04-25 16:26
category: k8s
author: Anand Rao
tags: 
 - DevSecOps
 - k8s
 - cloudnative
summary: The DevSecOps Flow in the Cloud Native World
---
<img src="/images/Thehammer.png" width="350" height="175" />
Containers and specifically Kubernetes, are abuzz in the world of cloud technology. The buzz has led to a tremendous rush to adopt Kubernetes, and rightfully so. Much too often, you are in a state where one can ask you, you just installed/provisioned Kubernetes - “Now what? “. The Challenge of operationalizing Kubernetes is not tiny. There is a lot that goes into making Kubernetes useful for developers and operators alike. 

***DevSecOps***, We have heard this phrase quite a bit lately. I will attempt to explain what that means to me and kick off a series of blog posts to walk through the workflow. Let's try to paint a picture of the current software development and deployment. There has been constant evolution around development, build, deploy and manage the software with multiple constraints. The DevOps model or the flow has been adopted by the cloud-native software industry. The key tenets of DevOps have been the automation of the key tasks of the workflow that the use of a CI/CD process. The distributed and the exploded scale has brought a big emphasis on security to be a core part of this DevOps flow that brings us to the new mantra in the cloud-native world called the ***DevSecOps*** flow.


<img src="/images/Theask.png" width="250" height="500" />

***The big Ask***

The current cloud-native and mainly distributed world impose the developers and designers with multiple implicit asks.
1. **Secure** by default
1. Can **scale** at ease to the needs of the distributed user base
1. Allows users to use it **consistently**
1. **Highly available**

Let's run through a standard set of pit stops that a ***DevSecOps*** flow or the journey takes for an Idea to reach a user's hands.


1. **The Ideation and Development**
<img src="/images/Development.png" width="300" height="500" />

    In this phase, developers and the product designers join hands and come up with the product design. The effort will focus on the user requirements augmented with the four key constraints added. The constraints are added almost like a guard rail to the design process.
   
     The Shelf life of modern software is highly reduced based on the fast-paced change in customer or user needs that seem to evolve over a short time compared to the past. This reduced shelf life requires developers to show high productivity. To meet the heightened productivity needs, developers will need a modern development framework.

    A modern development framework is one that allows for rapid development and excellent reusability with libraries that can help take care of everyday needs for the developers. One can think of modern frameworks like the Spring Framework for Java,  .Net Framework, and or modern languages like Go, all of which have some common and expected traits that lead to the rapid development of software.  We will need to provide the developers a place to store the code developed. The frameworks offer critical features for managing code generated by multiple developers in a team or even various groups that may be located across different geographies and time zones. Here is where the need for an excellent modern source control system comes into the picture. Git made famous by GitHub, has become one of the most commonly used source control systems.
       
    The development phase also may bring the need for a very short and swift feedback loop at times referred to as the ***Inner Loop***. Think about looping through this multiple times in a single day. We will need to provide developers with development environments that allow for these fast-paced loops along with the potential of supporting distributed development teams. There could be some additional needs for data services and other middleware. We can almost envision a small dedicated or multi-tenant Kubernetes cluster that can be short living.


1. **Continuous Integration**

    Continuous Integration is part of the flow when multiple individual components are running in an integrated model for the first time. We will need to ensure the functionality when working together by running some automated tests as part of a Continuous Integration pipeline. Teams and companies typically use Jenkins, Tekton, Concourse, or other similar a CI tool-chain to perform this set of tasks.


1. **Source to Image packaging**
<img src="/images/Imagebuild.png" width="350" height="175" />

   This is when we need to find an efficient process bound by the vital guard rails from above. Building a strategy that can create a **secure** image and the **reproducible** images are top needs here. The latest OCI image formats allow for a lot of efficiencies when it comes to building the images. With the layered model where the layer can be a reference to a separate repository, changes can now be granular.


1. **The Image Registry**
<img src="/images/registry1.png" width="350" height="175" />

    TThe docker or OCI image registry is a repository for storing OCI images. The registries have become increasingly sophisticated as they build towards scale, security, consistency, and enterprise readiness. One such registry is [Harbor](https://goharbor.io/), an open-source project under CNCF. Harbor has a few powerful enterprise-ready features like automatic image scanning for vulnerabilities and control of images by clusters based on levels of exposures. Harbor also allows for image signing to help avoid man-in-the-middle attacks. Harbor has a sound RBAC system to allow for user access controls and support for a Helm repository. Docker-hub,  GCR, ACR, and ECR are also very popular docker registries.

1. **Third party and OSS apps/services** 
<img src="/images/Catalog.png" width="350" height="175" />

    There are not many real applications that do not need databases and other middleware like queuing systems. With the ever-evolving technology, there is a big concern around the security of the images. Common questions around security and consistency are a significant concern for security and compliance teams at enterprises. The need to provide a process to have the same OSS apps or third-party apps packaged on top of a known secure base image is a significant need for enterprises. They have to create processes that manage constant checking for vulnerabilities and updates when patches are ready. Bitnami, with its catalog of k8s ready packages, has been a leader at this. VMware Tanzu has taken this to the enterprise with an entire product under the Tanzu portfolio. The Tanzu Application Catalog allows enterprises to pick a secure, curated, and hardened base image on which customer can package their choice of the OSS or third-party applications. This product also recreates the images when any patches for the underlying base image or the software are available. That process elevates the security posture of an enterprise in this area significantly.
   
    Most of the public cloud providers have marketplace/catalogs being developed. The goal is that the developers and operators have an easy, secure, and consistent way to consume/provision apps and services on Kubernetes.

1. **The Continuous Deployment**

    Now that we have figured out how to build a custom application and bring in an OSS/Third-party app and store them in a trusted secure registry. We need to have an automated continuous deployment pipeline triggered by any change in the images that needs deployment into the clusters that the platform is managing. Enterprises use tools like Jenkins and concourse for their automated deployment needs. Argo CD is an up-and-coming open-source deployment tool.


1. **Network and routing integration, Service mesh ?**
<img src="/images/Networking.png" width="500" height="200" />

    Once the Applications are deployed into the clusters one of the major needs for the applications to run in conjunction with tother apps and services that may be running on other namespaces and clusters is their connectivity to those names spaces in other clusters. Connectivity comes with other challenges. How are we going to make sure the connections are encrypted and how we will be able to negotiate the potentially unknown number of disparate apps and networks. This is where a need for a consolidated and unified network comes into picture. Service meshes play a big role in solving this puzzle.  

    One example is VMware's Tanzu service mesh. TSM allows you to create an overall mesh connecting multiple istio installed k8s clusters. Its agents allow for a encrypted pipe to be created across all connected clusters and provides a unified view to the apps running in the connected names spaces via a Global Namespace. This helps realize a major requirement when the workloads for a given application may be spread across multiple clusters which may in turn be spread across multiple Geographies. A service mesh like this can enable multiple other features. Example: The service mesh has all the traffic related information allowing it to use its agents to trigger app autoscaling using traffic based metrics. 

    The Service Mesh secton of the industry has a good few players and options for the consumers. I will detail that in a subsequent blog post. 

1. **Cluster and application management at Scale** 
<img src="/images/Management.png" width="600" height="200" />

    Once the Applications are deployed into the k8s clusters, a significant need for the applications will be a seamless way to connect to other namespaces and clusters. Connectivity brings challenges. How are we going to make sure the connections are encrypted, and how will we negotiate the potentially unknown number of disparate apps and networks. All of this drives the need for a consolidated and unified network. Service meshes play a significant role in solving this puzzle.  

    One example is VMware's Tanzu service mesh. TSM allows you to create an overall mesh connecting multiple istio installed k8s clusters. Its agents allow for an encrypted pipe to be built across all connected clusters and provide a unified view to the apps running in the connected names spaces via a Global Namespace. This helps realize a major requirement when the workloads for a given application may be spread across multiple clusters, which may, in turn, be spread across multiple Geographies. A service mesh like this can enable multiple other features. Example: The service mesh has all the traffic-related information allowing it to use its agents to trigger app autoscaling using traffic-based metrics.

    The Service Mesh section of the industry has a good few players and options for the consumers. I will detail that in a subsequent blog post.


1. **Monitoring and Observability**
<img src="/images/Observability.png" width="400" height="200" />
 
   Monitoring and Observability is a big ask when it comes to managing applications and clusters. In the current cloud-native world,  there is a lot of the management and monitoring that is based on the SRE principles that use the SLO / SLA based management. Having a pure cloud-native observability tool like Tanzu observability helps have a good view into your entire set of workloads. 

***Putting it all together***

If I ask you, which part of the above key steps is it ok to skip ? My assumption is that you will say none. very step is important to make sure the key asks of security, speed, consistency and scale are taken care on every step on the workflow to fully operationalize a container platform like kubernetes. I will double click on these steps and talk about the details on each of those on the subsequent posts on this series. Until then I leave you with this picture ! ( Click the picture to see higher resolution.)

<a href="/images/Fullpicture.png" target="_blank">
<img src="/images/Fullpicture.png" width="1200" height="600" />
</a>
