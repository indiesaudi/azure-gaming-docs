---
title: Multiplayer Game Server Hosting Using AKS
description: You can opt to manage containerized dedicated game servers using the Kubernetes orchestrator on Azure using the managed Azure Kubernetes Service (AKS).
author: BrianPeek
keywords: 
ms.topic: reference-architecture
ms.date: 3/14/2019
ms.author: brpeek
ms.prod: azure-gaming
---

# Synchronous Multiplayer Using Azure Kubernetes Service (AKS) and Thundernetes

You can choose to manage containerized dedicated game servers using the Kubernetes orchestrator on Azure with the managed Azure Kubernetes Service (AKS).

This article will describe the architecture used in [project Thundernetes](https://playfab.github.io/thundernetes).

## Architecture diagram

[![Synchronous multiplayer using Azure Kubernetes Service and Thundernetes](media/multiplayer/multiplayer-aks-hosting.png)](media/multiplayer/multiplayer-aks-hosting.png)

## Relevant services

- [Azure Kubernetes Service](https://azure.microsoft.com/services/kubernetes-service/) - Simplifies the deployment and operations of Kubernetes.
- [Azure Container Registry](https://azure.microsoft.com/services/container-registry/) - Allows storing images for all types of container deployments.
- [Thundernetes](https://playfab.github.io/thundernetes) - Thundernetes makes it easy to run your game servers on Kubernetes.

## Architecture considerations

### Custom resource definitions (CRDs)

In this reference architecture, Kubernetes is extended by using [Custom Resource Definition (CRDs)](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/) objects. These objects will be used to represent the dedicated game server entities.

Specifically, there are 2 core entities, which are represented by 2 respective CRDs:

- **GameServer**: represents the game server itself. Each GameServer has a single corresponding child [Pod](https://kubernetes.io/docs/concepts/workloads/pods/pod/) which will run the container image with the game server executable.
- **GameServerBuild**: represents a collection/set of related GameServers that will run the same Pod template and can be scaled in/out within the GameServerBuild (i.e. add or remove more instances of them).

    GameServers that are members of the same GameServerBuild have a lot of similarities in their execution environment, e.g. all of them could launch the same multiplayer map or the same type of game. So, you could have one collection for a "Capture the flag" mode of your game and another collection for a "Conquest" mode. Or, a collection for players playing on map "X" and a collection for players playing on map "Y".

## Deployment template

Follow the [instructions on the repository](https://github.com/PlayFab/thundernetes) to create the Azure Kubernetes Cluster and install Thundernetes.

## Additional resources and samples

[Official Kubernetes documentation](https://kubernetes.io/docs/concepts/overview/what-is-kubernetes/)

## Pricing

If you don't have an Azure subscription, create a [free account](https://aka.ms/azfreegamedev) to get started with 12 months of free services. You're not charged for services included for free with Azure free account, unless you exceed the limits of these services. Learn how to check usage through the [Azure Portal](/azure/billing/billing-check-free-service-usage#check-usage-on-the-azure-portal) or through the [usage file](/azure/billing/billing-check-free-service-usage#check-usage-through-the-usage-file).

You are responsible for the cost of the Azure services used while running these reference architectures.  The total amount will vary based on usage. See the pricing webpages for each of the services that were used in the reference architecture:

- [Azure Kubernetes Service](https://azure.microsoft.com/pricing/details/kubernetes-service/)
- [Azure Container Registry](https://azure.microsoft.com/pricing/details/container-registry/)

You can also use the [Azure pricing calculator](https://azure.microsoft.com/pricing/calculator/) to configure and estimate the costs for the Azure services that you are planning to use.