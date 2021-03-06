---
title: "Introdução à autenticação na Automatização do Azure | Microsoft Docs"
description: "Este artigo fornece uma descrição geral da segurança de Automatização e os métodos de autenticação diferentes disponíveis para as Contas de Automatização na Automatização do Azure."
services: automation
documentationcenter: 
author: MGoedtel
manager: jwhit
editor: tysonn
keywords: "segurança de automatização, automatização segura; autenticação de automatização"
ms.assetid: 4a6bc2f5-c5a2-4dfb-b10d-7950d750dee8
ms.service: automation
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/01/2017
ms.author: magoedte
ROBOTS: NOINDEX
ms.translationtype: Human Translation
ms.sourcegitcommit: 0d6f6fb24f1f01d703104f925dcd03ee1ff46062
ms.openlocfilehash: 1a0258e872449bf2f2f08345cbe86564e28d964e
ms.contentlocale: pt-pt
ms.lasthandoff: 04/17/2017

---
# <a name="introduction-to-authentication-in-azure-automation"></a>Introdução à Autenticação na Automatização do Azure  
A Automatização do Azure permite-lhe automatizar tarefas relativamente aos recursos no Azure, no local e outros fornecedores de serviços em nuvem, tais como os Amazon Web Services (AWS).  Para um runbook efetuar as ações necessárias, tem de ter permissões para aceder de forma segura aos recursos com os direitos mínimos necessários dentro da subscrição.

Este artigo irá cobrir os vários cenários de autenticação suportados pela Automatização do Azure e irá mostrar como começar com base no ambiente ou ambientes que tem de gerir.  

## <a name="automation-account-overview"></a>Descrição Geral da Conta de Automatização
Quando inicia a Automatização do Azure pela primeira vez, tem de criar pelo menos uma conta de Automatização. As contas de automatização permitem-lhe a isolar os recursos de Automatização (runbooks, recursos, configurações) a partir dos recursos contidos noutras contas de Automatização. Pode utilizar contas de Automatização para separar recursos em ambientes lógicos separados. Por exemplo, poderá utilizar uma conta para o desenvolvimento, outra para a produção e outra para o seu ambiente no local.  Uma conta de automatização do Azure é diferente da sua conta Microsoft ou contas criadas na sua subscrição do Azure.

Os recursos de Automatização de cada conta de Automatização estão associados a uma única região do Azure, mas as contas de Automatização podem gerir todos os recursos na sua subscrição. A razão principal para criar contas de Automatização em diferentes regiões seria se tiver políticas que exigem que os dados e os recursos estejam isolados numa região específica.

> [!NOTE]
> As contas de Automatização e os recursos que contêm que são criados no portal do Azure não podem ser acedidos no portal clássico do Azure. Se pretende gerir estas contas ou os respetivos recursos com o Windows PowerShell, tem de utilizar os módulos do Azure Resource Manager.
>

Todas as tarefas que executa relativamente aos recursos que utilizam o Azure Resource Manager e os cmdlets do Azure na Automatização do Azure têm de ser autenticados no Azure utilizando a autenticação com base em credenciais de identidade organizacional do Azure Active Directory.  A autenticação baseada em certificados foi o método de autenticação original com o modo de Gestão de Serviço do Azure, mas foi difícil de configurar.  A autenticação no Azure com o utilizador do Azure AD foi introduzida em 2014 não só para simplificar o processo de configuração de uma conta de Autenticação, mas também para suportar a capacidade de autenticar de forma não interativa no Azure com uma única conta de utilizador que funcionou com o Azure Resource Manager e os recursos clássicos.   

Atualmente, quando cria uma nova conta de Automatização no portal do Azure, este cria automaticamente:

* Uma conta Run As, que cria um novo principal de serviço no Azure Active Directory, um certificado e atribui o controlo de acesso baseado em funções (RBAC) de Contribuidor, que será utilizado para gerir recursos do Gestor de Recursos através de runbooks.
* A conta Run As clássica através do carregamento de um certificado de gestão, que será utilizada para gerir a Gestão de Serviço do Azure ou recursos clássicos através de runbooks.  

O controlo de acesso baseado em funções está disponível no Azure Resource Manager para conceder ações permitidas a uma conta de utilizador do Azure AD e uma conta Run As e autenticar esse principal de serviço.  Leia [o artigo Controlo de acesso baseado em funções na Automatização do Azure](automation-role-based-access-control.md) para obter mais informações ajudar a desenvolver o seu modelo para a gestão de permissões de Automatização.  

Os Runbooks em execução numa Função de Trabalho de Runbook Híbrida no seu datacenter ou em relação a serviços de computação no AWS não podem utilizar o mesmo método que é normalmente utilizado para a autenticação de runbooks nos recursos do Azure.  Isto acontece porque esses recursos estão a ser executados fora do Azure e, como tal, necessitarão das suas próprias credenciais de segurança definidas na Automatização para autenticar em recursos que acedem localmente.  

## <a name="authentication-methods"></a>Métodos de autenticação
A tabela seguinte resume os métodos de autenticação diferentes para cada ambiente suportado pela Automatização do Azure e o artigo que descreve como configurar a autenticação para os runbooks.

| Método | Ambiente | Artigo |
| --- | --- | --- |
| Conta de Utilizador do Azure AD |Azure Resource Manager e Gestão do Serviço do Azure |[Autenticar Runbooks com a conta de Utilizador do Azure AD](automation-create-aduser-account.md) |
| Conta Run As do Azure |Azure Resource Manager |[Autenticar Runbooks com a conta Run As do Azure](automation-sec-configure-azure-runas-account.md) |
| Conta Run As Clássica do Azure |Gestão do Serviço do Azure |[Autenticar Runbooks com a conta Run As do Azure](automation-sec-configure-azure-runas-account.md) |
| Autenticação do Windows |Datacenter No Local |[Autenticar Runbooks para Funções de Trabalho de Runbook Híbridas](automation-hybrid-runbook-worker.md) |
| Credenciais AWS |Amazon Web Services |[Autenticar Runbooks com os Amazon Web Services (AWS)](automation-config-aws-account.md) |

