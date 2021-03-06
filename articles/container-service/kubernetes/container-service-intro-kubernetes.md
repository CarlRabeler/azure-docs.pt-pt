---
title: "Introdução ao Azure Container Service para Kubernetes | Microsoft Docs"
description: "O Azure Container Service para Kubernetes permite a implementação e gestão fácil de aplicações baseadas em contentores no Azure."
services: container-service
documentationcenter: 
author: gabrtv
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Kubernetes, Docker, Containers, Microsserviços, Azure"
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/21/2017
ms.author: gamonroy
ms.custom: 
ms.translationtype: HT
ms.sourcegitcommit: bfd49ea68c597b109a2c6823b7a8115608fa26c3
ms.openlocfilehash: ed3193cce078b4b81f269fe01a40d3aa4465782d
ms.contentlocale: pt-pt
ms.lasthandoff: 07/25/2017

---
# <a name="introduction-to-azure-container-service-for-kubernetes"></a>Introdução ao Azure Container Service para Kubernetes
O Azure Container Service para Kubernetes simplifica a criação, configuração e gestão de um cluster de máquinas virtuais que estão pré-configuradas para executar aplicações em contentores. Isto permite-lhe utilizar as suas competências ou basear-se num corpo extenso e crescente de conhecimentos da comunidade, para implementar e gerir aplicações baseadas em contentores do Microsoft Azure.

Ao utilizar o Azure Container Service, pode tirar partido das funcionalidades de nível empresarial do Azure, mantendo, ao mesmo tempo, a portabilidade da aplicação através de Kubernetes e do formato de imagens do Docker.

## <a name="using-azure-container-service-for-kubernetes"></a>Utilizar o Azure Container Service para Kubernetes
O nosso objetivo para o Azure Container Service é fornecer um ambiente de alojamento de contentores, utilizando ferramentas e tecnologias open-source que são atualmente populares entre os nossos clientes. Para este fim, expomos os pontos finais da API do Kubernetes padrão. Ao utilizar estes pontos finais padrão, pode tirar partido de qualquer software que consiga comunicar com um cluster do Kubernetes. Por exemplo, pode escolher [kubectl](https://kubernetes.io/docs/user-guide/kubectl-overview/), [helm](https://helm.sh/) ou [draft](https://github.com/Azure/draft).

## <a name="creating-a-kubernetes-cluster-using-azure-container-service"></a>Criar um cluster do Kubernetes com o Azure Container Service
Para começar a utilizar o Azure Container Service, implemente um cluster deste serviço com a [CLI 2.0 do Azure](container-service-kubernetes-walkthrough.md) ou através do portal (procure **Azure Container Service** no Marketplace). Se for utilizador avançado e precisar de mais controlo sobre os modelos do Azure Resource Manager, pode utilizar o projeto [acs-engine](https://github.com/Azure/acs-engine) para criar o seu próprio cluster do Kubernetes personalizado e implementá-lo através da CLI `az`.

### <a name="using-kubernetes"></a>Utilizar Kubernetes
O Kubernetes automatiza a implementação, o dimensionamento e a gestão de aplicações no contentor. Tem um conjunto avançado de funcionalidades, incluindo:
* Empacotamento automático
* Autorrecuperação
* Dimensionamento horizontal
* Deteção do serviço e balanceamento de carga
* Implementações e reversões automáticas
* Gestão de segredos e de configurações
* Orquestração de armazenamento
* Execução de lotes

Diagrama da arquitetura de Kubernetes implementado através do Azure Container Service:

![Azure Container Service configurado para utilizar o Kubernetes.](media/acs-intro/kubernetes.png)

## <a name="videos"></a>Vídeos

Suporte de Kubernetes no Azure Container Service (Azure Friday, janeiro de 2017):

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Kubernetes-Support-in-Azure-Container-Services/player]
>
>

Ferramentas para Programar e Implementar Aplicações no Kubernetes (Azure OpenDev, junho de 2017):

> [!VIDEO https://channel9.msdn.com/Events/AzureOpenDev/June2017/Tools-for-Developing-and-Deploying-Applications-on-Kubernetes/player]
>
>

## <a name="next-steps"></a>Passos seguintes

Explore o [Kubernetes Quickstart](container-service-kubernetes-walkthrough.md) (Início Rápido do Kubernetes) para começar a explorar o Azure Container Service.
