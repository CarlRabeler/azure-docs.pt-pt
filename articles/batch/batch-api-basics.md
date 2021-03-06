---
title: "Descrição geral do Azure Batch para programadores | Microsoft Docs"
description: "Conheça as funcionalidades do serviço Batch e das respetivas APIs de um ponto de vista de programação."
services: batch
documentationcenter: .net
author: tamram
manager: timlt
editor: 
ms.assetid: 416b95f8-2d7b-4111-8012-679b0f60d204
ms.service: batch
ms.devlang: multiple
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-compute
ms.date: 06/28/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: c2f2a878414e4efd626d674ef9a182ae52eeb1ff
ms.contentlocale: pt-pt
ms.lasthandoff: 08/21/2017

---
# <a name="develop-large-scale-parallel-compute-solutions-with-batch"></a>Desenvolver soluções de computação paralelas em grande escala com o Batch

Nesta descrição geral dos componentes principais do serviço Azure Batch, vamos discutir as funcionalidades e os recursos principais do serviço que os programadores do Batch podem utilizar para criar soluções de computação paralelas em grande escala.

Se está a desenvolver uma aplicação computacional distribuída ou um serviço que emite chamadas à [API REST][batch_rest_api] diretas ou a utilizar os [SDKs do Batch](batch-apis-tools.md#azure-accounts-for-batch-development), vai utilizar muitos dos recursos e funcionalidades abordados neste artigo.

> [!TIP]
> Para obter uma introdução mais detalhada do serviço Batch, veja [Noções básicas do Azure Batch](batch-technical-overview.md).
>
>

## <a name="batch-service-workflow"></a>Fluxo de trabalho do serviço Batch
O fluxo de trabalho detalhado que se segue é típico de quase todos os serviços e aplicações que utilizam o serviço Batch para processar cargas de trabalho paralelas:

1. Carregue os **ficheiros de dados** que quer processar para uma conta do [Armazenamento do Azure][azure_storage]. O Batch inclui suporte incorporado para aceder ao Armazenamento de Blobs do Azure e as suas tarefas podem transferir estes ficheiros para [nós de computação](#compute-node) quando são executadas.
2. Carregue os **ficheiros de aplicação** que as aplicações vão executar. Estes ficheiros podem ser binários ou scripts e as respetivas dependências e são executados pelas tarefas dos seus trabalhos. As tarefas podem transferir estes ficheiros a partir da sua conta de Armazenamento ou pode utilizar a funcionalidade de [pacotes de aplicações](#application-packages) do Batch na gestão e implementação de aplicações.
3. Crie um [conjunto](#pool) de nós de computação. Quando cria um conjunto, especifica o número de nós de computação do mesmo e o tamanho e o sistema operativo desses nós. Quando cada tarefa dos seus trabalhos é executada, é atribuída a um dos nós do conjunto.
4. Crie um [trabalho](#job). Os trabalhos gerem uma coleção de tarefas. Cada trabalho é associado a um conjunto específico no qual as tarefas desses trabalhos vão ser executadas.
5. Adicione [tarefas](#task) ao trabalho. Cada tarefa executa a aplicação ou script que carregou para processar os ficheiros de dados que transfere a partir da sua conta de Armazenamento. À medida que cada tarefa é concluída, pode carregar o respetivo resultado para o Armazenamento do Azure.
6. Monitorize o progresso do trabalho e obtenha o resultado da tarefa no Armazenamento do Azure.

As secções seguintes abordam estes e os outros recursos do Batch permitem o seu cenário computacional distribuído.

> [!NOTE]
> Precisa de uma [conta do Batch](#account) para utilizar o serviço. Quase todas as soluções do Batch utilizam também uma conta de [Armazenamento do Azure][azure_storage] para armazenamento e obtenção de ficheiros. Atualmente, o Batch só suporta o tipo de conta de armazenamento para **fins gerais**, conforme descrito no passo 5 de [Criar uma conta de Armazenamento](../storage/common/storage-create-storage-account.md#create-a-storage-account), em [Acerca das contas de Armazenamento do Azure](../storage/common/storage-create-storage-account.md).
>
>

## <a name="batch-service-resources"></a>Recursos do serviço Batch
Todas as soluções que utilizam o serviço Batch precisam de alguns dos recursos seguintes, como contas, nós de computação, conjuntos, trabalhos e tarefas. Outros recursos, como as agendas de trabalhos e os pacotes de aplicações, são funcionalidades úteis, mas opcionais.

* [Conta](#account)
* [Nó de computação](#compute-node)
* [Conjunto](#pool)
* [Tarefa](#job)

  * [Agendas de tarefas](#scheduled-jobs)
* [Tarefa](#task)

  * [Tarefa de início](#start-task)
  * [Tarefa do gestor de tarefas](#job-manager-task)
  * [Tarefas de preparação e de lançamento da tarefa](#job-preparation-and-release-tasks)
  * [Tarefa de várias instâncias (Multi-instance task, MPI)](#multi-instance-tasks)
  * [Dependências de tarefas](#task-dependencies)
* [Pacotes de aplicações](#application-packages)

## <a name="account"></a>Conta
Uma conta do Batch é uma entidade identificada exclusivamente no âmbito do serviço Batch. Todo o processamento está associado a uma conta do Batch.

Pode criar uma conta do Azure Batch através do [portal do Azure](batch-account-create-portal.md) ou através de programação, como com a [Biblioteca .NET de Gestão de Batch](batch-management-dotnet.md). Ao criar a conta, pode associar uma conta de armazenamento do Azure.

### <a name="pool-allocation-mode"></a>Modo de alocação de conjuntos

Quando cria uma conta do Batch, pode especificar quantos [conjuntos](#pool) de nós de computação são alocados. Pode optar por alocar conjuntos de nós de computação numa subscrição gerida pelo Azure Batch ou pode alocá-los na sua própria subscrição. A propriedade *pool allocation mode* da conta determina onde é que os conjuntos são alocados. 

Para decidir que modo de alocação de conjuntos utilizar, considere qual destes melhor se adequa ao seu cenário:

* **Serviço Batch**: o Serviço Batch é o modo de alocação de conjuntos predefinido, no qual os conjuntos são aplicados em segundo plano nas subscrições geridas do Azure. Tenha em conta os aspetos seguintes relativamente ao modo de alocação de conjuntos do Serviço Batch.

    - O modo de alocação de conjuntos do Serviço Batch suporta conjuntos do Serviço Cloud e de Máquinas Virtuais.
    - O modo de alocação de conjuntos do Serviço Batch suporta a autenticação de chave partilhada ou a [autenticação do Azure Active Directory](batch-aad-auth.md) (Azure AD). 
    - Pode utilizar nós de computação dedicados ou de baixa prioridade nos conjuntos alocados com o modo de alocação de conjuntos do Serviço Batch.
    - Não utilize o modo de alocação de conjuntos se planear criar conjuntos de maquinas virtuais a partir de imagens de VMs personalizadas ou se planear utilizar uma rede virtual. Em alternativa, crie a sua conta com o modo de alocação de conjuntos Subscrição de Utilizador.
    - Os conjuntos de Máquinas Virtuais aprovisionados em contas criadas com o modo de alocação de conjuntos do Serviço Batch têm de ser criados a partir de imagens do [Marketplace das Máquinas Virtuais do Azure][vm_marketplace].

* **Subscrição de Utilizador**: com o modo de alocação de conjuntos Subscrição do Utilizador, os conjuntos do Batch são alocados na subscrição do Azure em que a conta é criada. Tenha em conta os aspetos seguintes relativamente ao modo de alocação de conjuntos Subscrição de Utilizador:
     
    - O modo de alocação de conjuntos Subscrição de Utilizador suporta apenas conjuntos de Máquinas Virtuais. Não suporta agrupamentos de Serviços Cloud.
    - Para criar conjuntos de Máquinas Virtuais a partir de imagens de VMs personalizadas ou para utilizar uma rede virtual com estes conjuntos, tem de utilizar o modo de alocação de conjuntos Subscrição de Utilizador.  
    - Nos conjuntos que são alocados na subscrição de utilizador, tem de utilizar a [autenticação do Azure Active Directory](batch-aad-auth.md). 
    - Tem de configurar um cofre de chaves do Azure para a sua conta do Batch, se o modo de alocação de conjuntos for definido como Subscrição de Utilizador. 
    - Só pode utilizar nós de computação dedicados em conjuntos numa conta criada com o modo de alocação de conjuntos Subscrição de Utilizador. Os nós de prioridade baixa não são suportados.
    - Os conjuntos de Máquinas Virtuais aprovisionados em contas com o modo de alocação de conjuntos Subscrição de Utilizador têm de ser criados a partir de imagens do [Marketplace das Máquinas Virtuais do Azure][vm_marketplace] ou de imagens personalizadas fornecidas por si.

A tabela seguinte compara os modos de alocação de conjuntos Serviço Batch e Subscrição de Utilizador.

| **Modo de alocação de conjuntos**                 | **Serviço Batch**                                                                                       | **Subscrição de Utilizador**                                                              |
|-------------------------------------------|---------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------|
| **Os conjuntos são alocados**               | Numa subscrição do Azure gerida                                                                           | Na subscrição do utilizador na qual a conta do Batch é criada                        |
| **Configurações suportadas**             | <ul><li>Configuração do Serviço Cloud</li><li>Configuração de Máquina Virtual (Linux e Windows)</li></ul> | <ul><li>Configuração de Máquina Virtual (Linux e Windows)</li></ul>                |
| **Imagens de VM suportadas**                  | <ul><li>Imagens do Azure Marketplace</li></ul>                                                              | <ul><li>Imagens do Azure Marketplace</li><li>Imagens personalizadas</li></ul>                   |
| **Tipos de nós de computação suportados**         | <ul><li>Nós dedicados</li><li>Nós de baixa prioridade</li></ul>                                            | <ul><li>Nós dedicados</li></ul>                                                  |
| **Autenticação suportada**             | <ul><li>Chave Partilhada</li><li>Azure AD</li></ul>                                                           | <ul><li>Azure AD</li></ul>                                                         |
| **Azure Key Vault necessário**             | Não                                                                                                      | Sim                                                                                |
| **Quota de núcleos**                           | Determinada pela quota de núcleos do Batch                                                                          | Determinada pela quota de núcleos da subscrição                                              |
| **Suporte da Rede Virtual do Azure (Vnet)** | Conjuntos criados com a Configuração Serviço Cloud                                                      | Conjuntos criados com a Configuração de Máquina Virtual                               |
| **Modelo de implementação de Vnet suportado**      | Vnets criadas com o modelo de implementação clássica                                                             | Não são suportadas VNets criadas com o modelo de implementação clássica ou Azure Resource Manager |

## <a name="azure-storage-account"></a>Conta de armazenamento do Azure

A maioria das soluções do Batch utilizam o Armazenamento do Azure para armazenar ficheiros de recursos e ficheiros de saída.  

Atualmente, o Batch só suporta o tipo de conta de armazenamento para fins gerais, conforme descrito no passo 5 de [Criar uma conta de Armazenamento](../storage/common/storage-create-storage-account.md#create-a-storage-account), em [Acerca das contas de Armazenamento do Azure](../storage/common/storage-create-storage-account.md). As suas tarefas do Batch (incluindo tarefas standard, tarefas de início, tarefas de preparação de trabalhos e tarefas de lançamento de trabalhos) têm de especificar os ficheiros de recursos que residem apenas nas contas de armazenamento para fins gerais.


## <a name="compute-node"></a>Nó de computação
Um nó de computação é uma máquina virtual (VM) ou uma VM de um serviço cloud do Azure dedicada ao processamento de uma parte da carga de trabalho da sua aplicação. O tamanho de um nó determina o número de núcleos de CPU, a capacidade da memória e o tamanho do sistema de ficheiros local que está alocado ao nó. Pode criar conjuntos de nós do Windows ou Linux com imagens dos Serviços Cloud do Azure, imagens do [Marketplace das Máquinas Virtuais do Azure][vm_marketplace] ou imagens personalizadas preparadas por si. Veja a secção [Conjunto](#pool), abaixo, para obter mais informações sobre estas opções.

Os nós podem executar qualquer executável ou script que seja suportado pelo ambiente dos respetivos sistemas operativos. Incluem-se \*.exe, \*. cmd, \*. bat e scripts do PowerShell para o Windows, e binários, shell e scripts Python para Linux.

Todos os nós de computação do Batch incluem também:

* Uma [estrutura de pastas](#files-and-directories) padrão e [variáveis de ambiente](#environment-settings-for-tasks) associadas que também estão disponíveis para referência por parte das tarefas.
* Definições de **Firewall** que são configuradas para controlar o acesso.
* [Acesso remoto](#connecting-to-compute-nodes) para Windows (protocolo RDP (Remote Desktop Protocol)) e nós Linux (Secure Shell (SSH)).

## <a name="pool"></a>Conjunto
Um conjunto é uma coleção de nós na qual a sua aplicação é executada. O conjunto pode ser criado manualmente por si ou pelo serviço Batch automaticamente quando especifica o trabalho a ser realizado. Pode criar e gerir um conjunto que cumpra os requisitos de recursos da sua aplicação. Os conjuntos só podem ser utilizados pela conta do Batch em que foram criados. Uma conta do Batch pode ter mais do que um conjunto.

Conjuntos do Azure Batch criados com base na plataforma de computação principal do Azure. Eles proporcionam alocação em grande escala, instalação de aplicações, distribuição de dados, monitorização de estado de funcionamento e o ajuste flexível do número de nós de computação dentro de um conjunto ([dimensionamento](#scaling-compute-resources)).

Um nome e um endereço IP exclusivos são atribuídos a cada nó que seja adicionado a um conjunto. Quando um nó é removido de um conjunto, as alterações feitas ao sistema operativo ou aos ficheiros perdem-se e o respetivo nome e endereço IP são libertados para utilização futura. Quando um nó deixa um conjunto, a sua duração termina.

Quando cria um conjunto, pode especificar os seguintes atributos. Algumas definições variam, consoante o modo de alocação do agrupamento da [conta](#account) do Batch:

- Sistema operativo e versão do nó de computação
- Tipo de nó de comutação e número de nós de destino
- Tamanho dos nós de computação
- Política de dimensionamento
- Política de agendamento de tarefas
- Estado de comunicação dos nós de computação
- Tarefas de início dos nós de computação
- Pacotes de aplicações
- Configuração da rede

Cada uma destas definições está descrita mais detalhadamente nas secções seguintes.

> [!IMPORTANT]
> As contas do Batch criadas com o modo de alocação de conjuntos Serviço Batch têm uma quota predefinida que limita o número de núcleos nas mesmas. O número de núcleos corresponde ao número de nós de computação. Pode encontrar as quotas predefinidas e instruções sobre como [aumentar uma quota](batch-quota-limit.md#increase-a-quota) em [Quotas and limits for the Azure Batch service](batch-quota-limit.md) (Quotas e limites para o serviço Azure Batch). Se o seu agrupamento não estiver a obter o número de destino de nós, tal poderá dever-se à quota de núcleos.
>
>As contas do Batch criadas com o modo de alocação de conjuntos Subscrição do Utilizador não observam as quotas de serviço do Batch. Em vez disso, partilham a quota de núcleos da subscrição especificada. Para obter mais informações, veja [Virtual Machines limits](../azure-subscription-service-limits.md#virtual-machines-limits) (Limites das Máquinas Virtuais), em [Azure subscription and service limits, quotas, and constraints](../azure-subscription-service-limits.md) (Limites, quotas e limitações das subscrições e serviços do Azure).
>
>

### <a name="compute-node-operating-system-and-version"></a>Sistema operativo e versão do nó de computação

Quando cria um agrupamento do Batch, pode especificar a configuração da máquina virtual do Azure e o tipo de sistema operativo que quer executar em cada nó de computação do agrupamento. Os dois tipos de configurações disponíveis no Batch são:

- A **Configuração de Máquina Virtual**, que especifica que o agrupamento é composto por máquinas virtuais do Azure. Estas VMs podem ser criadas a partir de imagens do Linux ou do Windows. 

    Quando cria um agrupamento com base na Configuração de Máquina Virtual, tem de especificar não só o tamanho dos nós e a origem das imagens utilizadas para criá-los, como também a **referência da imagem da máquina virtual** e o **SKU do agente de nó** do Batch a instalar nos nós. Para obter mais informações sobre como especificar estas propriedades dos conjuntos, veja [Provision Linux compute nodes in Azure Batch pools (Aprovisionar nós de computação do Linux em conjuntos do Azure Batch)](batch-linux-nodes.md).

- A **Configuração de Serviços Cloud**, que especifica que o agrupamento é composto por nós de Serviços Cloud do Azure. Os Serviços Cloud *só* disponibilizam nós de computação do Windows.

    Os sistemas operativos disponíveis para os conjuntos de Configuração de Serviços Cloud estão listados em [Azure Guest OS releases and SDK compatibility matrix (Versões de SO Convidado do Azure e matriz de compatibilidade de SDK)](../cloud-services/cloud-services-guestos-update-matrix.md). Quando cria um agrupamento que contém nós de Serviços Cloud, tem de especificar o tamanho do nó e a respetiva *Família de SO*. Os Serviços Cloud são implementados no Azure mais rapidamente do que as máquinas virtuais que executem o Windows. Se quiser agrupamentos de nós de computação do Windows, vai chegar à conclusão de que os Serviços Cloud são mais vantajosos no que diz respeito ao tempo de implementação.

    * A *Família do SO* também determina quais as versões do .NET que estão instaladas no SO.
    * Tal como acontece com as funções de trabalho nos Serviços Cloud, pode especificar uma *Versão de SO* (para obter mais informações sobre as funções de trabalho, veja a secção [Tell me about cloud services (Saber mais sobre os serviços em nuvem)](../cloud-services/cloud-services-choose-me.md#tell-me-about-cloud-services), na [Descrição geral dos Serviços Cloud](../cloud-services/cloud-services-choose-me.md)).
    * Tal como com as funções de trabalho, recomendamos que especifique `*` para a *Versão do SO*, para que os nós sejam atualizados automaticamente e não seja necessário fazer nada para fornecer versões lançadas recentemente. O principal motivo para selecionar uma versão de SO específica é garantir a compatibilidade da aplicação, o que permite fazer testes de retrocompatibilidade antes de permitir a atualização da versão. Após a validação, a *Versão do SO* do conjunto pode ser atualizada e a nova imagem do SO instalada. As tarefas que estejam em execução são interrompidas e colocadas novamente em fila.

Quando cria um agrupamento, tem de selecionar o **nodeAgentSkuId** adequado, consoante o SO da imagem de base do VHD. Pode chamar a operação [List Supported Node Agent SKUs](https://docs.microsoft.com/rest/api/batchservice/list-supported-node-agent-skus) para ver um mapeamento dos IDs de SKUs de agente de nós disponíveis para as respetivas referências de Imagem do SO.

Veja a secção [Conta](#account) para obter mais informações sobre como definir o modo de alocação de agrupamentos durante a criação de uma conta do Batch.

#### <a name="custom-images-for-virtual-machine-pools"></a>Imagens personalizadas para agrupamentos de Máquinas Virtuais

Para utilizar uma imagem personalizada para aprovisionar os seus conjuntos de Máquinas Virtuais, crie a sua conta do Batch com o modo de alocação de conjuntos Subscrição de Utilizador. Com este modo, os conjuntos do Batch são alocados à subscrição na qual a conta reside. Veja a secção [Conta](#account) para obter mais informações sobre como definir o modo de alocação de agrupamentos durante a criação de uma conta do Batch.

Para utilizar uma imagem personalizada, terá de generalizar a imagem para prepará-la. Para obter informações sobre como preparar imagens do Linux personalizadas a partir de VMs do Azure, veja [Capture an Azure Linux VM to use as a template](../virtual-machines/linux/capture-image-nodejs.md) (Capturar uma VM do Azure do Linux e utilizá-la como modelo). Para obter informações sobre como preparar imagens do Windows personalizadas a partir de VMs do Azure, veja [Create custom VM images with Azure PowerShell](../virtual-machines/windows/tutorial-custom-images.md) (Criar imagens de VM personalizadas com o Azure PowerShell). 

> [!IMPORTANT]
> Quando preparar a imagem personalizada, tenha em atenção o seguinte:
> - Confirme que a imagem de SO base que vai utilizar para aprovisionar o conjunto do Batch não tem nenhuma extensão do Azure pré-instalada, como a extensão Custom Script. Se a imagem tiver uma extensão pré-instalada, o Azure poderá encontrar problemas durante a implementação da VM.
> - Certifique-se de que a imagem de SO base que indicar utiliza a unidade temp predefinida, pois o agente de nós do Batch espera, atualmente, a unidade temp.
>
>

Para criar um agrupamento de Configuração de Máquina Virtual com uma imagem personalizada, precisa de uma ou mais contas do Armazenamento do Azure standard, para armazenar as imagens VHD personalizadas. As imagens personalizadas são armazenadas como blobs. Para fazer referência às imagens personalizadas quando cria um agrupamento, especifique os URIs dos blobs de VHD da imagem personalizada para a propriedade [osDisk](https://docs.microsoft.com/rest/api/batchservice/add-a-pool-to-an-account#bk_osdisk) da propriedade [virtualMachineConfiguration](https://docs.microsoft.com/rest/api/batchservice/add-a-pool-to-an-account#bk_vmconf).

Certifique-se de que as suas contas de armazenamento cumprem os critérios seguintes:   

- As contas de armazenamento que contêm os blobs de VHD da imagem personalizada têm de estar na mesma subscrição que a conta do Batch (a subscrição do utilizador).
- As contas de armazenamento especificadas têm de estar na mesma região que a conta do Batch.
- Atualmente, só são suportadas as contas de armazenamento para fins gerais. O Armazenamento premium do Azure será suportado no futuro.
- Pode especificar uma conta de armazenamento com vários blobs de VHD personalizados ou múltiplas contas de armazenamento, contendo cada uma um único blob. Para obter um desempenho melhor, recomendamos utilizar várias contas de armazenamento.
- Um blob de VHD de imagem personalizada individual pode suportar até 40 instâncias de VMs do Linux ou 20 instâncias de VM do Windows. Para criar agrupamentos com mais VMs, tem de criar cópias do blob de VHD. Por exemplo, um agrupamento com 200 VMs do Windows precisa de dez blobs de VHD individuais especificados na propriedade **osDisk**.

Para criar um agrupamento a partir de uma imagem personalizada através do portal do Azure:

1. No portal do Azure, navegue para a sua conta do Batch.
2. No painel **Definições**, selecione o item de menu **Agrupamentos**.
3. No painel **Agrupamentos**, selecione o comando **Adicionar**; é apresentado o painel **Adicionar agrupamento**.
4. Selecione **Imagem Personalizada (Linux/Windows)**, no menu pendente **Tipo de Imagem**. O portal apresenta o seletor **Imagem Personalizada**. Escolha um ou mais VHDs no mesmo contentor e clique no botão **Selecionar**. 
    Será adicionado no futuro suporte para vários VHDs de diferentes contas de armazenamento e diferentes contentores.
5. Selecione o **Editor/Oferta/Sku** correto para os VHDs personalizados, selecione o modo de **Colocação em Cache** pretendido e preencha todos os outros parâmetros do agrupamento.
6. Para verificar se um agrupamento se baseia numa imagem personalizada, veja a propriedade **Operating System** na secção de resumo do recurso do painel **Agrupamento**. O valor desta propriedade deve ser **Imagem de VM personalizada**.
7. São apresentados no painel **Propriedades** do agrupamento todos os VHDs personalizados associados a esse agrupamento.

### <a name="compute-node-type-and-target-number-of-nodes"></a>Tipo de nó de comutação e número de nós de destino

Quando cria um conjunto, pode especificar quais os tipos de nós de computação que pretende e o número de destino de cada um. Os dois tipos de nós de computação são:

- **Nós de computação dedicados.** Os nós de computação dedicados estão reservados para as suas cargas de trabalho. São mais dispendiosos do que os nós de baixa prioridade, mas é garantido que nunca serão substituídos.

- **Nós de computação de baixa prioridade.** Os nós de baixa prioridade tiram partido da capacidade excedente do Azure para executar as cargas de trabalho do Batch. Os nós de baixa prioridade são mais económicos por hora do que os nós dedicados e permitem cargas de trabalho que exigem muito poder de computação. Para obter mais informações, veja [Use low-priority VMs with Batch](batch-low-pri-vms.md) (Utilizar VMs de baixa prioridade com o Batch).

    Se o Azure não tiver capacidade excedente suficiente, os nós de computação de baixa prioridade podem ser substituídos. Se um nó for substituído enquanto está a executar tarefas, estas são colocadas novamente na fila e executadas assim que esteja disponível um nó de computação. Os nós de baixa prioridade são uma boa opção para cargas de trabalho em que o tempo de conclusão do trabalho é flexível e este pode ser distribuído por muitos nós. Antes de decidir se vai utilizar nós de baixa prioridade no seu cenário, confirme que eventuais trabalhos perdidos devido à interrupção são mínimos e fáceis de recriar.

    Os nós de computação de baixa prioridade só estão disponíveis para contas do Batch criadas com o modo de alocação de conjuntos definido como **Serviço do Batch**.

Pode ter nós de computação de baixa prioridade e dedicados no mesmo conjunto. Cada tipo e nó &mdash; baixa prioridade e dedicado &mdash; tem a sua própria definição de destino, de acordo com a qual pode especificar o número pretendido de nós. 
    
O número de nós de computação é referido com *destino* porque, em algumas situações, é possível que o seu conjunto não atinja o número de nós pretendido. Por exemplo, um conjunto pode não alcançar o destino se atingir primeiro a [quota de núcleos](batch-quota-limit.md) da sua conta do Batch. Ou o conjunto poderá não alcançar o destino se tiver aplicado uma fórmula de dimensionamento automático ao conjunto que limite o número máximo de nós.

Para obter informações sobre os preços dos nós de computação de baixa prioridade e dedicados, veja [Preços de Batch](https://azure.microsoft.com/pricing/details/batch/).

### <a name="size-of-the-compute-nodes"></a>Tamanho dos nós de computação

Os tamanhos dos nós de computação de **Configuração de Serviços Cloud** estão listados em [Sizes for Cloud Services (Tamanhos dos Serviços Cloud)](../cloud-services/cloud-services-sizes-specs.md). O Batch suporta todos os tamanhos de Serviços Cloud, exceto `ExtraSmall`, `STANDARD_A1_V2`, e `STANDARD_A2_V2`.

Os tamanhos dos nós de computação de **Configuração de Máquina Virtual** estão listados em [Sizes for virtual machines in Azure (Tamanhos das máquinas virtuais no Azure)](../virtual-machines/linux/sizes.md) (Linux) e em [Sizes for virtual machines in Azure (Tamanhos das máquinas virtuais no Azure)](../virtual-machines/windows/sizes.md) (Windows). O Batch suporta todos os tamanhos de VM do Azure, exceto `STANDARD_A0` e os do armazenamento premium (séries `STANDARD_GS`, `STANDARD_DS` e `STANDARD_DSV2`).

Ao selecionar um tamanho de nó de computação, considere as características e os requisitos das aplicações que vai executar nos nós. Alguns aspetos, como se a aplicação tem vários threads e a quantidade de memória que consome, podem ajudar a determinar o tamanho de nó mais adequado e económico. Normalmente, o tamanho dos nós é selecionado ao partir do princípio de que será executada uma tarefa de cada vez num nó. Contudo, é possível que várias tarefas (e, consequentemente, múltiplas instâncias da aplicação) sejam [executadas em paralelo](batch-parallel-node-tasks.md) nos nós de computação durante a execução do trabalho. Neste caso, é comum escolher um tamanho de nó maior para acomodar a maior necessidade de execução de tarefas em paralelo. Veja [Task scheduling policy (Política de agendamento de tarefas)](#task-scheduling-policy) para obter mais informações.

Todos os nós num conjunto têm de ter o mesmo tamanho. Se quiser executar aplicações com requisitos de sistema e/ou níveis de carga diferentes, recomendamos utilizar conjuntos separados.

### <a name="scaling-policy"></a>Política de dimensionamento

Para cargas de trabalho dinâmicas, pode escrever e aplicar uma [fórmula de dimensionamento automático](#scaling-compute-resources) num conjunto. O serviço Batch avalia periodicamente a fórmula e ajusta o número de nós dentro do conjunto com base em vários parâmetros de conjuntos, trabalhos e tarefas que pode especificar.

### <a name="task-scheduling-policy"></a>Política de agendamento de tarefas

A opção de configuração [máximo de tarefas por nó](batch-parallel-node-tasks.md) determina o número máximo de tarefas que podem ser executadas em paralelo em cada nó de computação dentro do conjunto.

A configuração predefinida especifica que uma tarefa seja executada num nó de computação de cada vez, mas existem cenários onde é vantajoso ter duas ou mais tarefas executadas num nó em simultâneo. Veja o [cenário de exemplo](batch-parallel-node-tasks.md#example-scenario) no artigo [Tarefas de nó simultâneas](batch-parallel-node-tasks.md) para saber como tirar partido de várias tarefas por nó.

Também pode especificar um *tipo de preenchimento*, que determina se o Batch propaga as tarefas uniformemente em todos os nós de um conjunto ou se preenche cada nó com o número máximo de tarefas antes de atribuir tarefas a outro nó.

### <a name="communication-status-for-compute-nodes"></a>Estado de comunicação dos nós de computação

Na maioria dos cenários, as tarefas funcionam de forma independente e não têm de comunicar entre si. Contudo, poderão existir algumas aplicações nas quais as tarefas têm de comunicar, como em [cenários de MPI](batch-mpi.md).

Pode configurar um conjunto para permitir a **comunicação entre os nós**, para que os nós do conjunto possam comunicar no runtime. Se a comunicação internós estiver ativada, os nós nos conjuntos de Configuração de Serviços Cloud podem comunicar entre si em portas maiores do que a 1.100 e os conjuntos de Configuração de Máquina Virtual não restringem o tráfego em nenhuma porta.

Tenha em conta que a comunicação internós também tem impacto na colocação dos nós dentro dos clusters e pode limitar o número máximo de nós num conjunto, devido a restrições de implementação. Se a comunicação entre nós não for necessária para a sua aplicação, o serviço Batch pode alocar um número de nós potencialmente maior ao conjunto a partir de muitos clusters e datacenters diferentes, para proporcionar um poder de processamento paralelo superior.

### <a name="start-tasks-for-compute-nodes"></a>Tarefas de início dos nós de computação

A *tarefa inicial* opcional é executada em cada nó à medida que é adicionado ao conjunto, e sempre que é reiniciado ou sempre que a respetiva imagem for recriada. A tarefa inicial é especialmente útil para preparar nós de computação para a execução de tarefas, como instalar as aplicações que as suas tarefas executam nos nós de computação.

### <a name="application-packages"></a>Pacotes de aplicações

Pode especificar [pacotes de aplicações](#application-packages) para implementar os nós de computação no conjunto. Os pacotes de aplicações fornecem uma implementação simplificada e o controlo de versões das aplicações que as suas tarefas executam. Os pacotes de aplicações que especificou para um conjunto são instalados em cada nó associado a esse conjunto, e sempre que um nó é reiniciado ou sempre que a respetiva imagem for recriada.

> [!NOTE]
> Os pacotes de aplicações são suportados em todos os conjuntos do Batch criados após 5 de Julho de 2017. Só são suportados em conjuntos do Batch criados entre 10 de Março de 2016 e 5 de Julho de 2017 se o conjunto tiver sido criado com uma configuração de Serviço Cloud. Os conjuntos do Batch criados antes de 10 de Março de 2016 não suportam pacotes de aplicações. Para obter mais informações sobre a utilização de pacotes de aplicações para implementar as aplicações em nós do Batch, veja [Implementar aplicações em nós de computação com pacotes de aplicações do Batch](batch-application-packages.md).
>
>

### <a name="network-configuration"></a>Configuração da rede

Pode especificar a sub-rede de uma [rede virtual (VNet)](../virtual-network/virtual-networks-overview.md) do Azure na qual devem ser criados os nós de computação do agrupamento. Veja a secção [Configuração de rede de conjunto](#pool-network-configuration) para obter mais informações.


## <a name="job"></a>Tarefa
Um trabalho é uma coleção de tarefas. Gere de que forma é que a computação é realizada pelas respetivas tarefas nos nós de computação de um conjunto.

* O trabalho especifica o **conjunto** no qual o trabalho será executado. Pode criar um novo conjunto para cada trabalho ou utilizar um conjunto para muitos trabalhos. Pode criar um conjunto para cada trabalho associado a uma agenda de trabalho ou para todos os trabalhos associados a uma agenda de trabalho.
* Pode especificar uma **prioridade de trabalho** opcional. Quando um trabalho for submetido com uma prioridade superior à dos trabalhos atualmente em curso, as tarefas do trabalho de prioridade superior são introduzidas na fila à frente das tarefas dos trabalhos de prioridade inferior. As tarefas de trabalhos de prioridade inferior que já estão em execução não serão substituídas.
* Pode utilizar **restrições** de trabalhos para especificar determinados limites para os trabalhos:

    Pode definir um **tempo máximo cronometrado**, para que, se um trabalho for executado durante mais tempo do que o tempo máximo cronometrado especificado, o trabalho e as respetivas tarefas são terminados.

    O Batch pode detetar e, depois, repetir as tarefas falhadas. Pode especificar o **número máximo de repetições de tarefas** como uma restrição, incluindo o facto de uma tarefa ser *sempre* ou *nunca* repetida. Repetir uma tarefa significa que a tarefa é recolocada na fila para ser executada novamente.
* A aplicação cliente pode adicionar tarefas a um trabalho ou pode especificar uma [tarefa de gestão de trabalhos](#job-manager-task). As tarefas de gestão de trabalhos contêm as informações necessárias para criar as tarefas necessárias para um trabalho, sendo a tarefa de gestão de trabalhos executada num dos nós de computação do conjunto. A tarefa do gestor de trabalhos é processada especificamente pelo Batch – é colocada na fila após a criação do trabalho e é reiniciada se falhar. A tarefa de gestão de trabalhos é *obrigatória* para os trabalhos criados por uma [agenda de trabalhos](#scheduled-jobs), já que é a única forma de definir as tarefas antes de o trabalho ser instanciado.
* Por predefinição, os trabalhos permanecem no estado ativo quando todas as tarefas dentro do trabalho estiverem concluídas. Pode alterar este comportamento para que o trabalho seja automaticamente terminado quando todas as tarefas no trabalho estiverem concluídas. Defina a propriedade **onAllTasksComplete** da tarefa ([OnAllTasksComplete][net_onalltaskscomplete] no Batch .NET) para *terminatejob*, para terminar automaticamente o trabalho quando todas as respetivas tarefas estiverem no estado de conclusão.

    Tenha em atenção que o serviço Batch considera um trabalho *sem* tarefas como tendo todas as tarefas concluídas. Por conseguinte, esta opção é frequentemente utilizada com uma [tarefa de gestor de trabalhos](#job-manager-task). Se pretender utilizar a terminação automática do trabalho sem um gestor de trabalhos, deve definir inicialmente uma nova propriedade **onAllTasksComplete** do trabalho como *noaction* e, em seguida, configurá-la para *terminatejob* (terminar o trabalho) apenas depois de terminar de adicionar tarefas ao trabalho.

### <a name="job-priority"></a>Prioridade dos trabalhos
Pode atribuir uma prioridade aos trabalhos que cria no Batch. O serviço Batch utiliza o valor de prioridade do trabalho para determinar a ordem de agendamento dos trabalhos numa conta (não deve ser confundido com [trabalhos agendados](#scheduled-jobs)). Os valores de prioridade variam entre -1000 a 1000, sendo -1000 a prioridade mais baixa e 1000 a prioridade mais alta. Para atualizar a prioridade de uma tarefa, realize a operação [Atualizar as propriedades de uma tarefa][rest_update_job] (REST do Batch) ou ao modifique a propriedade [CloudJob.Priority][net_cloudjob_priority] (NET do Batch).

Dentro da mesma conta, os trabalhos de prioridade mais alta têm precedência de agendamento sobre os de prioridade mais baixa. Um trabalho com um valor de prioridade superior numa conta não tem precedência de agendamento sobre outro trabalho com um valor de prioridade inferior numa conta diferente.

O agendamento de tarefas no âmbito dos conjuntos é independente. Entre conjuntos diferentes, não é garantido que um trabalho de prioridade superior seja agendado primeiro se o respetivo conjunto associado tiver poucos nós inativos. No mesmo conjunto, as tarefas com o mesmo nível de prioridade têm as mesmas hipóteses de serem agendadas.

### <a name="scheduled-jobs"></a>Tarefas agendadas
[Agendas de tarefas][rest_job_schedules] permitem-lhe criar tarefas recorrentes no âmbito do serviço Batch. Uma agenda de tarefas especifica quando executar tarefas e inclui as especificações das tarefas a executar. Pode especificar a duração da agenda – quando e durante quanto tempo está em vigor – e com que frequência os trabalhos são criados durante o período agendado.

## <a name="task"></a>Tarefa
As tarefas são uma unidade de computação que estão associadas a um trabalho. São executadas num nó. As tarefas são atribuídas a um nó para execução ou são colocadas na fila até que um nó fique livre. Resumindo, as tarefas executam um ou mais programas ou scripts num nó de computação para fazer os trabalhos necessários.

Quando cria uma tarefa, pode especificar:

* A **linha de comandos** da tarefa. Esta é a linha de comandos que executa a aplicação ou script no nó de computação.

    É importante ter em conta que a linha de comandos não é, efetivamente, executada sob uma shell. Por conseguinte,-não é possível tirar partido nativamente das funcionalidades de shell, como a expansão da [variável de ambiente](#environment-settings-for-tasks) expansão (inclui `PATH`). Para tirar partido destas funcionalidades, tem de invocar a shell na linha de comandos, por exemplo, ao iniciar `cmd.exe` nos nós do Windows ou `/bin/sh` no Linux:

    `cmd /c MyTaskApplication.exe %MY_ENV_VAR%`

    `/bin/sh -c MyTaskApplication $MY_ENV_VAR`

    Se as suas tarefas tiverem de executar uma aplicação ou script que não esteja no `PATH` ou de referenciar variáveis de ambiente, invoque a shell explicitamente na linha de comandos das tarefas.
* Os **ficheiros de recursos** que contêm os dados a serem processados. Estes ficheiros são copiados automaticamente para o nó a partir do Armazenamento de blobs numa conta de Armazenamento do Azure para fins gerais antes de a linha de comando da tarefa ser executada. Para obter mais informações, veja as secções [Tarefa de início](#start-task) e [Ficheiros e diretórios](#files-and-directories).
* As **variáveis de ambiente** de que a aplicação precisa. Para obter mais informações, veja a secção [Definições de ambiente das tarefas](#environment-settings-for-tasks).
* As **restrições** sob as quais a computação deve ocorrer. Por exemplo, as restrições incluem o tempo máximo dentro do qual a tarefa pode ser executada, o número máximo de vezes que uma tarefa falhada deve ser repetida e o tempo máximo durante o qual os ficheiros no diretório de trabalho da tarefa são retidos.
* **Pacotes de aplicações** para implementar o nó de computação no qual a tarefa está agendada para ser executada. Os [pacotes de aplicações](#application-packages) fornecem uma implementação simplificada e o controlo de versões das aplicações que as suas tarefas executam. Os pacotes de aplicações ao nível das tarefas são particularmente úteis em ambientes de conjunto partilhado, em que as diferentes tarefas são executadas num conjunto, e o conjunto não é eliminado quando um trabalho estiver concluído. Se o trabalho tiver menos tarefas do que nós no conjunto, os pacotes de aplicações de tarefas podem minimizar a transferência de dados, uma vez que a aplicação é implementada apenas nos nós que executam tarefas.

Além das tarefas que define para realizar a computação num nó, também são fornecidas pelo serviço Batch as seguintes tarefas especiais:

* [Tarefa de início](#start-task)
* [Tarefa do gestor de tarefas](#job-manager-task)
* [Tarefas de preparação e de lançamento da tarefa](#job-preparation-and-release-tasks)
* [Tarefas de várias instâncias (MPI)](#multi-instance-tasks)
* [Dependências de tarefas](#task-dependencies)

### <a name="start-task"></a>Tarefa de início
Ao associar uma **tarefa de início** a um conjunto, pode preparar o ambiente de funcionamento dos respetivos nós. Por exemplo, pode realizar ações como instalar as aplicações que as suas tarefas vão executar ou começar processos em segundo plano. A tarefa de início é executada sempre que um nó é iniciado enquanto permanecer no conjunto, incluindo quando o nó for adicionado pela primeira vez ao conjunto e quando é reiniciado ou a respetiva imagem recriada.

Uma das principais vantagens da tarefa de início é o facto de poder conter todas as informações necessárias para configurar um nó de computação e instalar as aplicações de que as tarefas precisam para serem executadas. Por conseguinte, aumentar o número de nós num conjunto é tão simples como especificar a nova contagem de nó de destino. A tarefa de início fornece ao serviço do Batch as informações necessárias para configurar novos nós e prepará-los para aceitar tarefas.

À semelhança de qualquer tarefa do Batch, pode especificar uma lista de **ficheiros de recursos** no [Armazenamento do Azure][azure_storage], além de uma **linha de comandos** para ser executada. O serviço do Batch começa por copiar os ficheiros de recursos para o nó a partir do Armazenamento do Azure e, depois, executa a linha de comandos. Numa tarefa de início de conjunto, a lista de ficheiros contém, geralmente, a aplicação de tarefa e as respetivas dependências.

Contudo, a tarefa de início também pode incluir dados de referência que todas as tarefas em execução no nó de computação podem utilizar. Por exemplo, a linha de comandos de uma tarefa de início pode fazer uma operação `robocopy` para copiar os ficheiros de aplicação (que foram especificados como ficheiros de recursos e transferidos para o nó) do [diretório de trabalho](#files-and-directories) da tarefa de início para a [pasta partilhada](#files-and-directories) e, em seguida, executar um MSI ou `setup.exe`.

Normalmente, é aconselhável que o serviço Batch aguarde pela conclusão da tarefa de início antes de considerar o nó como estando pronto para receber aplicações, mas esta definição pode ser configurada.

Se uma tarefa inicial falhar num nó de computação, o estado do nó é atualizado para refletir a falha e não são atribuídas tarefas ao nó. Uma tarefa de início pode falhar se houver um problema ao copiar os respetivos ficheiros de recursos do armazenamento ou se o processo executado pela respetiva linha de comandos devolver um código de saída diferente de zero.

Se adicionar ou atualizar a tarefa de início para um agrupamento existente, tem de reiniciar os nós de computação para que a tarefa de início seja aplicada aos mesmos.

>[!NOTE]
> O tamanho total de uma tarefa de início tem de ser inferior ou igual a 32 768 carateres, incluindo ficheiros de recursos e variáveis de ambiente. Para garantir que a sua tarefa de início cumpre este requisito, pode seguir uma de duas abordagens:
>
> 1. Pode utilizar pacotes de aplicações para distribuir aplicações ou dados para todos os nós do agrupamento do Batch. Para obter mais informações sobre os pacotes de aplicações, veja [Implementar aplicações em nós de computação com pacotes de aplicações do Batch](batch-application-packages.md).
> 2. Pode criar manualmente um arquivo zipado que contenha os seus ficheiros de aplicação. Carregue o arquivo zipado para o Armazenamento do Azure como um blob. Especifique este arquivo como um ficheiro de recursos para a tarefa de início. Antes de executar a linha de comandos da tarefa de início, descomprima o arquivo a partir da linha de comandos. 
>
>    Para descomprimir o arquivo, pode utilizar uma ferramenta de arquivos à sua escolha. Tem de incluir a ferramenta utilizada para descomprimir o arquivo como um ficheiro de recursos para a tarefa de início.
>
>

### <a name="job-manager-task"></a>Tarefa do gestor de tarefas
Geralmente, é utilizada uma **tarefa de gestor de trabalhos** para controlar e/ou monitorizar a execução de trabalhos, por exemplo, para criar e submeter as tarefas de um trabalho, determinar tarefas adicionais a executar e determinar quando o trabalho está concluído. No entanto, as tarefas de gestor de trabalhos não estão confinadas a estas atividades. São tarefas completas que podem realizar quaisquer ações necessárias no âmbito do trabalho. Por exemplo, uma tarefa de gestor de trabalhos pode transferir um ficheiro especificado como um parâmetro, analisar o conteúdo desse ficheiro e submeter mais tarefas com base nesse conteúdo.

A tarefa de gestor de trabalhos é iniciada antes de todas as outras tarefas. Proporciona as seguintes funcionalidades:

* É submetida automaticamente como uma tarefa pelo serviço de Batch quando a tarefa é criada.
* É agendada para execução antes das outras tarefas numa tarefa.
* O seu nó associado é o último a ser removido de um conjunto quando este está a ser reduzido.
* A sua terminação pode ser vinculada à terminação de todas as tarefas na tarefa.
* A tarefa do gestor de trabalhos recebe a prioridade mais elevada quando tem de ser reiniciada. Se um nó inativo não estiver disponível, o serviço Batch pode terminar uma das outras tarefas em execução no conjunto, de modo a criar espaço para a tarefa do gestor de trabalhos ser executada.
* Uma tarefa do gestor de tarefas numa tarefa não tem prioridade sobre as tarefas de outras tarefas. No âmbito das tarefas, apenas as prioridades ao nível da tarefa são respeitadas.

### <a name="job-preparation-and-release-tasks"></a>Tarefas de preparação e de lançamento da tarefa
O Batch fornece tarefas de preparação de trabalhos para a configuração de execução de trabalho prévia. As tarefas de libertação de trabalhos destinam-se a manutenção ou limpeza pós-trabalho.

* **Tarefa de preparação de trabalho**: as tarefas de preparação de trabalhos são executadas em todos os nós de computação agendados para executar tarefas antes de qualquer uma das outras tarefas de trabalho serem executadas. Pode utilizar uma tarefa de preparação de trabalhos para copiar dados partilhados por todas as tarefas, mas que são exclusivos do trabalho, por exemplo.
* **Tarefa de libertação de trabalhos**: quando um trabalho tiver sido concluído, é executada uma tarefa de libertação de trabalhos em cada nó no conjunto que executou, pelo menos, uma tarefa. Pode utilizar uma tarefa de libertação de trabalhos para eliminar dados copiados pela tarefa de preparação de trabalhos ou para comprimir e carregar dados de registos de diagnósticos, por exemplo.

Quer a tarefa de preparação de trabalhos, quer a de libertação de trabalhos, permitem-lhe especificar uma linha de comandos a ser executada quando a tarefa é invocada. Oferecem funcionalidades como, por exemplo, transferência de ficheiros, execução elevada, variáveis de ambiente personalizadas, duração de execução máxima e tempo de retenção de ficheiros

Para obter mais informações sobre as tarefas de lançamento e de preparação da tarefa, consulte [Executar tarefas de preparação e de conclusão da tarefa em nós de computação do Azure Batch](batch-job-prep-release.md).

### <a name="multi-instance-task"></a>Tarefa de várias instâncias
Um [tarefa de várias instâncias](batch-mpi.md) é uma tarefa que está configurada para ser executada simultaneamente em mais do que um nó de computação. Com as tarefas de várias instâncias, pode ativar cenários de computação de alto desempenho que precisam de um grupo de nós de computação alocados em conjunto para processar uma carga de trabalho única (como a Interface de Passagem de Mensagens (Message Passing Interface, MPI)).

Para ver um debate detalhado sobre a execução de trabalhos de MPI no Batch com a biblioteca .NET do Batch, veja [Use multi-instance tasks to run Message Passing Interface (MPI) applications in Azure Batch (Utilizar tarefas de várias instâncias para executar aplicações de Interface de Passagem de Mensagens (MPI) no Azure Batch)](batch-mpi.md).

### <a name="task-dependencies"></a>Dependências de tarefas
As [dependências de tarefas](batch-task-dependencies.md), como o nome indica, permitem-lhe especificar que uma tarefa depende da conclusão de outras tarefas antes de ser executada. Esta funcionalidade proporciona suporte para situações em que uma tarefa “a jusante” consome o resultado de uma tarefa “a montante” ou quando uma tarefa a montante efetua alguma inicialização que seja necessária para uma tarefa a jusante. Para utilizar esta funcionalidade, tem primeiro de ativar as dependências de tarefas na sua tarefa do Batch. Em seguida, para cada tarefa que dependa de outra (ou de muitas outras), especifique as tarefas das quais essa tarefa depende.

Com as dependências de tarefas, pode configurar cenários como os seguintes:

* A *tarefaB* depende da *tarefaA* (a execução da *tarefaB* não é iniciada enquanto a *tarefaA* não tiver sido concluída).
* A *tarefaC* depende da *tarefaA* e da *tarefaB*.
* A *tarefaD* depende de um intervalo de tarefas, como as tarefas *1* à *10*, antes de ser executada.

Veja [Task dependencies in Azure Batch (Dependências de tarefas no Azure Batch)](batch-task-dependencies.md) e o exemplo de código [TaskDependencies][github_sample_taskdeps] no repositório do GitHub [azure-batch-samples][github_samples] para obter detalhes mais aprofundados sobre esta funcionalidade.

## <a name="environment-settings-for-tasks"></a>Definições de ambiente para tarefas
Cada tarefa executada pelo serviço Batch tem acesso às variáveis de ambiente que define nos nós de computação. Estas incluem as variáveis de ambiente definidas pelo serviço Batch ([definidas pelo serviço][msdn_env_vars]) e as variáveis de ambiente personalizadas que pode definir para as suas tarefas. As aplicações e os scripts que as tarefas executam têm acesso a estas variáveis de ambiente durante a execução.

Pode definir variáveis de ambiente personalizadas ao nível da tarefa ou do trabalho ao preencher a propriedade *definições de ambiente* dessas entidades. Por exemplo, veja a operação [Add a task to a job (Adicionar uma tarefa a um trabalho)][rest_add_task] (API REST do Batch) ou as propriedades [CloudTask.EnvironmentSettings][net_cloudtask_env] e [CloudJob.CommonEnvironmentSettings][net_job_env] no .NET do Batch.

O serviço ou aplicação cliente pode obter as variáveis de ambiente de uma tarefa, quer definidas pelo serviço, quer personalizadas, através da operação [Get information about a task][rest_get_task_info] (REST do Batch) (Obter informações sobre uma tarefa) ou ao aceder à propriedade [CloudTask.EnvironmentSettings][net_cloudtask_env] (.NET do Batch). Os processos em execução num nó de computação podem aceder a estas e outras variáveis de ambiente no nó, por exemplo, com a sintaxe familiar `%VARIABLE_NAME%` (Windows) ou `$VARIABLE_NAME` (Linux) .

Pode obter uma lista completa de todas as variáveis de ambiente definidas pelo serviço em [Compute node environment variables (Variáveis de ambiente de nó de computação)][msdn_env_vars].

## <a name="files-and-directories"></a>Ficheiros e diretórios
Cada tarefa tem um *diretório de trabalho* no qual cria zero ou mais ficheiros e diretórios. Este diretório de trabalho pode ser utilizado para armazenar o programa que é executado pela tarefa, os dados que processa e o resultado do processamento feito. Todos os ficheiros e diretórios de uma tarefa são propriedade do utilizador da tarefa.

O serviço Batch expõe uma parte do sistema de ficheiros num nó como o *diretório de raiz*. As tarefas podem consultar a variável de ambiente `AZ_BATCH_NODE_ROOT_DIR` para aceder ao diretório de raiz. Para obter mais informações sobre como utilizar as variáveis de ambiente, veja [Definições de ambiente das tarefas](#environment-settings-for-tasks).

O diretório de raiz contém a seguinte estrutura de diretórios:

![Estrutura de diretórios do nó de computação][1]

* **partilhado**: este diretório proporciona acesso de leitura/escrita a *todas* as tarefas executadas num nó. Qualquer tarefa executada no nó pode criar, ler, atualizar e eliminar ficheiros neste diretório. As tarefas podem consultar a variável de ambiente `AZ_BATCH_NODE_SHARED_DIR` para aceder a este diretório.
* **arranque**: este diretório é utilizado pelas tarefas de início como o diretório de trabalho. Todos os ficheiros que tenham sido transferidos para o nó pela tarefa de início são armazenados aqui. A tarefa de início pode criar, ler, atualizar e eliminar ficheiros neste diretório. As tarefas podem consultar a variável de ambiente `AZ_BATCH_NODE_STARTUP_DIR` para aceder a este diretório.
* **Tarefas**: é criado um diretório para cada tarefa executada no nó. O diretório é acedido ao consultar a variável de ambiente `AZ_BATCH_TASK_DIR`.

    Dentro do diretório de cada tarefa, o serviço Batch cria um diretório de trabalho (`wd`) cujo caminho exclusivo é especificado pela variável de ambiente `AZ_BATCH_TASK_WORKING_DIR`. Este diretório proporciona acesso de leitura/escrita à tarefa. A tarefa pode criar, ler, atualizar e eliminar ficheiros neste diretório. Este diretório é mantido com base na restrição *RetentionTime* especificada para a tarefa.

    `stdout.txt` e `stderr.txt`: estes ficheiros são escritos na pasta da tarefa durante a execução da mesma.

> [!IMPORTANT]
> Quando um nó é removido do conjunto, *todos* os ficheiros que estão armazenados no mesmo são removidos.
>
>

## <a name="application-packages"></a>Pacotes de aplicações
A funcionalidade [pacotes de aplicações](batch-application-packages.md) facilita a gestão e a implementação de aplicações nos nós de computação nos seus conjuntos. Pode carregar e gerir facilmente várias versões das aplicações executadas pelas suas tarefas, incluindo os respetivos ficheiros binários e de suporte. Depois, pode implementar automaticamente uma ou mais destas aplicações nos nós de computação do seu conjunto.

Pode especificar os pacotes de aplicações ao nível do conjunto e tarefas. Quando especificar os pacotes de aplicações do conjunto, a aplicação é implementada em todos os nós no conjunto. Quando especificar os pacotes de aplicações de tarefas, a aplicação é implementada apenas nos nós que estão agendados para executarem, pelo menos, uma das tarefas do trabalho, imediatamente antes de a linha de comandos da tarefa ser executada.

O Batch processa os detalhes do trabalho com o Armazenamento do Azure para armazenar os seus pacotes de aplicações e implementá-los em nós de computação, para que o seu código e os custos de gestão possam ser simplificados.

Para saber mais sobre a funcionalidade de pacote de aplicação, veja [Implementar aplicações em nós de computação com pacotes de aplicações do Batch](batch-application-packages.md).

> [!NOTE]
> Se adicionar pacotes de aplicações de conjunto a um conjunto *existente*, terá de reiniciar os nós de computação para os pacotes de aplicações a serem implementados nos nós.
>
>

## <a name="pool-and-compute-node-lifetime"></a>Duração do nó de computação e de conjunto
Quando conceber a sua solução do Azure Batch, tem de tomar uma decisão de design em relação a como e quando são criados os conjuntos e durante quanto tempo os nós de computação são mantidos disponíveis dentro desses conjuntos.

Num extremo do espetro, pode criar um conjunto para cada trabalho que submeter e eliminá-lo assim que a execução das respetivas tarefas terminar. Desta forma, maximiza a utilização, já que os nós só são alocados quando é necessário e são encerrados logo que fiquem inativos. Apesar de isto significar que o trabalho tem de aguardar que os nós sejam alocados, é importante ter em atenção que as tarefas são agendadas para execução assim que os nós estejam individualmente disponíveis e alocados e que a tarefa inicial esteja concluída. O Batch *não* espera que todos os nós de um conjunto estejam disponíveis para lhes atribuir tarefas. Assim, garante-se a máxima utilização de todos os nós disponíveis.

No outro extremo do espetro, se a prioridade mais elevada for dar início aos trabalhos, pode criar um conjunto antecipadamente e disponibilizar os respetivos nós antes da submissão dos trabalhos. Neste cenário, as tarefas podem ser iniciadas de imediato, mas os nós podem manter-se inativos enquanto aguardam que as tarefas sejam atribuídas.

Geralmente, é utilizada uma abordagem combinada para lidar com cargas variáveis, mas contínuas. Pode ter um conjunto para o qual são submetidos vários trabalhos, mas pode aumentar e reduzir verticalmente o número de nós de acordo com a carga dos trabalhos (veja [Dimensionar os recursos de computação](#scaling-compute-resources) na secção seguinte). Pode fazer estes ajustes reativamente, com base na carga atual, ou pró-ativamente, se for possível prever a carga.

## <a name="virtual-network-vnet-and-firewall-configuration"></a>Configurar a rede virtual (VNet) e a firewall 

Quando aprovisiona um conjunto de nós de computação no Azure Batch, pode associá-lo a uma sub-rede de uma [rede virtual (VNet)](../virtual-network/virtual-networks-overview.md) do Azure. Para saber mais sobre como criar uma VNet com sub-redes, veja [Criar uma rede virtual do Azure com sub-redes](../virtual-network/virtual-networks-create-vnet-arm-pportal.md). 

 * A VNet associada a um conjunto tem de:

   * Estar na mesma **região** do Azure que a conta do Azure Batch.
   * Estar na mesma **subscrição** do Azure que a conta do Azure Batch.

* O tipo de VNet suportada depende da forma como os agrupamentos estão a ser alocados na conta do Batch:

    - Se o modo de alocação de conjuntos da sua conta do Batch estiver definido como Serviço Batch, só pode atribuir VNets a conjuntos criados com a **Configuração de Serviços Cloud**. Além disso, a VNet especificada tem de ser criada com o modelo de implementação clássica. Não são suportadas VNets criadas com o modelo de implementação Azure Resource Manager.
 
    - Se o modo de alocação de conjuntos da sua conta do Batch estiver definido como Subscrição de Utilizador, só pode atribuir VNets a conjuntos criados com a **Configuração de Máquina Virtual**. Não há suporte para conjuntos criados com a **Configuração de Serviço Cloud**. A VNet associada pode ser criada com o modelo de implementação Azure Resource Manager ou o modelo de implementação clássica.

    Para obter uma tabela que resume o suporte de VNet de acordo com o modo de alocação de conjuntos, veja a secção [Modo de alocação de conjuntos](#pool-allocation-mode).

* Se o modo de alocação de conjuntos da sua conta do Batch estiver definido como Serviço Batch, tem de fornecer permissões para que o principal de serviço do Batch aceda à VNet. A VNet tem de atribuir a função [Controlo de Acesso Baseado em Funções (RBAC) Contribuidor de Máquina Virtual Clássica](https://azure.microsoft.com/documentation/articles/role-based-access-built-in-roles/#classic-virtual-machine-contributor) ao principal de serviço do Batch. Se a função RBAC especificada não for fornecida, o serviço Batch devolve 400 (Pedido Inválido). Para adicionar a função no portal do Azure:

    1. Selecione a **VNet**, selecione **Controlo de acesso (IAM)** > **Funções** > **Contribuidor de Máquina Virtual** > **Adicionar**.
    2. No painel **Adicionar permissões**, selecione a função **Contribuidor de Máquina Virtual**.
    3. No painel **Adicionar permissões**, procure a API do Batch. Procure cada uma destas cadeias por ordem até encontrar a API:
        1. **MicrosoftAzureBatch**.
        2. **Batch do Microsoft Azure**. Os inquilinos mais recentes podem utilizar este nome.
        3. **ddbf3205-c6bd-46ae-8127-60eb93363864** é o ID para a API do Batch. 
    3. Selecione o principal de serviço da API do Batch. 
    4. Clique em **Guardar**.

        ![Atribuir a função Contribuidor de VM ao principal de serviço do Batch](./media/batch-api-basics/iam-add-role.png)


* A sub-rede indicada deve ter **endereços IP** livres suficientes para acomodar o número de nós de destino, ou seja, a soma das propriedades `targetDedicatedNodes` e `targetLowPriorityNodes` do agrupamento. Se a sub-rede não tiver endereços IP livres suficientes, o serviço Batch aloca, parcialmente, os nós de computação do conjunto e devolve um erro de redimensionamento.

* A sub-rede especificada tem de permitir a comunicação do serviço Batch para conseguir agendar tarefas nos nós de computação. Se a comunicação com os nós de computação for recusada por um **Grupo de Segurança de Rede (NSG)** associado à VNet, o serviço Batch define o estado dos nós de computação como **inutilizável**.

* Se a VNet especificada tiver **Grupos de Segurança de Rede (NSG)** associados e/ou uma **firewall**, têm de ser ativadas algumas portas do sistema reservadas para a comunicação de entrada:

- Nos agrupamentos criados com a configuração de máquina virtual, ative as portas 29876 e 29877, bem como a porta 22 para Linux e a 3389 para Windows. 
- Nos agrupamentos criados com uma configuração de serviço cloud, ative as portas 10100, 20100 e 30100. 
- Ative as ligações de saída para o Armazenamento do Azure na porta 443. Confirme também que o ponto final do Armazenamento do Azure pode ser resolvido por qualquer servidor DNS personalizado que sirva a sua VNet. Mais concretamente, deve ser resolvível um URL no formato `<account>.table.core.windows.net`.

    A tabela seguinte descreve as portas de entrada que tem de ativar nos agrupamentos que criou com a configuração de máquina virtual:

    |    Porta(s) de Destino    |    Endereço IP de origem      |    O Batch adiciona NSGs?    |    Necessário para a VM seja utilizável?    |    Ação do utilizador   |
    |---------------------------|---------------------------|----------------------------|-------------------------------------|-----------------------|
    |    <ul><li>Em agrupamentos criados com a configuração de máquina virtual: 29876, 29877</li><li>Nos agrupamentos criados com a configuração de serviço cloud: 10100, 20100, 30100, 30100</li></ul>         |    Apenas endereços IP de função do serviço Batch |    Sim. O Batch adiciona NSGs ao nível das interfaces de rede (NIC) anexadas a VMs. Estes NSGs permitem tráfego apenas de endereços IP da função do serviço Batch. Mesmo que abra estas portas a todo o tráfego da Internet, este será bloqueado na NIC. |    Sim  |  Não tem de especificar um NSG, porque o Batch só permite endereços IP do Batch. <br /><br /> No entanto, se especificar um NSG, certifique-se que estas portas estão abertas para tráfego de entrada. <br /><br /> Se especificar * como o IP de origem no NSG, o Batch continua a adicionar NSGs ao nível da NIC anexada às VMs. |
    |    3389, 22               |    Máquinas de utilizador, utilizadas para fins de depuração, para que possa aceder remotamente à VM.    |    Não                                    |    Não                     |    Adicione NSGs se quiser permitir o acesso remoto (RDP/SSH) à VM.   |                 

    A tabela seguinte descreve a porta de saída que tem de ativar para permitir o acesso ao Armazenamento do Azure:

    |    Porta(s) de Saída    |    Destino    |    O Batch adiciona NSGs?    |    Necessário para a VM seja utilizável?    |    Ação do utilizador    |
    |------------------------|-------------------|----------------------------|-------------------------------------|------------------------|
    |    443    |    Storage do Azure    |    Não    |    Sim    |    Se adicionar NSGs, confirme que esta porta está aberta ao tráfego de saída.    |


## <a name="scaling-compute-resources"></a>Dimensionar os recursos de computação
Com o [dimensionamento automático](batch-automatic-scaling.md), pode fazer com que o serviço Batch ajuste dinamicamente o número de nós de computação num conjunto de acordo com a carga de trabalho e a utilização de recursos atual do seu cenário de computação. Esta funcionalidade permite-lhe reduzir o custo global de execução da sua aplicação ao utilizar apenas os recursos de que precisa e libertar aqueles de que não precisa.

A ativação do dimensionamento automático é feita ao escrever uma [fórmula de dimensionamento automático](batch-automatic-scaling.md#automatic-scaling-formulas) e associá-la a um conjunto. O serviço Batch utiliza esta fórmula para determinar o número de destino de nós no conjunto para o próximo intervalo de dimensionamento (um intervalo que pode configurar). Pode especificar as definições de dimensionamento automático de um conjunto quando o criar ou ativar o dimensionamento num conjunto mais tarde. Também pode atualizar as definições de dimensionamento num conjunto preparado para dimensionamento.

Por exemplo, talvez um trabalho que requer que submeta um grande número de tarefas para execução. Pode atribuir uma fórmula de dimensionamento ao conjunto que ajusta o número de nós no conjunto com base no número atual de tarefas na fila, bem como na taxa de conclusão das tarefas desse trabalho. O serviço Batch avalia periodicamente a fórmula e redimensiona o conjunto com base na carga de trabalho e nas outras definições de fórmula. O serviço adiciona nós conforme necessário quando existe um grande número de tarefas em fila e remove nós quando não existem tarefas em execução ou em fila.

Uma fórmula de dimensionamento pode basear-se nas métricas seguintes:

* **Métricas de tempo**: baseadas em estatísticas recolhidas a cada cinco minutos no número de horas especificado.
* **Métricas de recurso**: baseadas na utilização da CPU, da largura de banda, da memória e no número de nós.
* **Métricas de tarefas**: baseadas no estado da tarefa, como *Ativa* (em fila), *Em Execução* ou *Concluída*.

Quando o dimensionamento automático diminuir o número de nós de computação de um conjunto, tem de pensar como vai processar as tarefas que estão a ser executadas no momento da operação de diminuição. Para dar resposta a esta situação, o Batch disponibiliza uma *opção de desalocação de nós*, a qual pode incluir nas suas fórmulas. Por exemplo, pode especificar que as tarefas em execução são paradas imediatamente, paradas imediatamente e recolocadas em fila para execução noutro nó ou que podem ser concluídas antes de o nó ser removido do conjunto.

Para obter mais informações sobre o dimensionamento automático de uma aplicação, consulte [Dimensionar automaticamente nós de computação num conjunto do Azure Batch](batch-automatic-scaling.md).

> [!TIP]
> Para maximizar a utilização de recursos de computação, defina o número de destino de nós para zero no final de uma tarefa, mas permita que as tarefas em execução sejam concluídas.
>
>

## <a name="security-with-certificates"></a>Segurança com certificados
Normalmente, tem de utilizar certificados quando encriptar ou desencriptar informações confidenciais relativas a tarefas, como a chave de uma [conta de Armazenamento do Azure][azure_storage]. Para suportar esta situação, pode instalar certificados em nós. Os segredos encriptados são transmitidos para as tarefas através dos parâmetros da linha de comandos ou incorporados num dos recursos da tarefa, sendo que os certificados instalados podem ser utilizados para desencriptá-los.

Utilize a operação [Adicionar certificado][rest_add_cert] (REST do Batch) ou o método [CertificateOperations.CreateCertificate][net_create_cert] (.NET do Batch) para adicionar um certificado a uma conta do Batch. Em seguida, pode associar o certificado a um conjunto novo ou existente. Quando um certificado é associado a um conjunto, o serviço Batch instala o certificado em cada nó no conjunto. O serviço Batch instala os certificados adequados quando o nó é iniciado, antes de iniciar qualquer tarefa (incluindo a tarefa de início e a tarefa do gestor de trabalhos).

Se adicionar certificados a um conjunto *existente*, terá de reiniciar os nós de computação para os certificados a serem implementados nos nós.

## <a name="error-handling"></a>Processamento de erros
Pode considerar necessário processar falhas de tarefas e de aplicações dentro da sua solução do Batch.

### <a name="task-failure-handling"></a>Processamento de falhas de tarefas
As falhas de tarefas enquadram-se nestas categorias:

* **Falha de pré-processamento**

    Se ocorrer uma falha ao iniciar uma tarefa, é definido um erro de pré-processamento para a mesma.  

    Os erros de pré-processamento podem dever-se ao facto de os ficheiros de recursos da tarefa terem sido movidos, de a conta de Armazenamento já não estar disponível ou de ter ocorrido outro problema que impediu a cópia bem-sucedida dos ficheiros para o nó.

* **Falhas de carregamento de ficheiros**

    Se o carregamento de ficheiros especificados para uma tarefa falhar por algum motivo, é definido um erro de carregamento para a mesma.

    Os erros de carregamento de ficheiros podem ocorrer se a SAS fornecida para aceder ao Armazenamento do Azure for inválida ou não fornecer permissões de escrita, se a conta de utilizador já não estiver disponíveis ou se tiver ocorrido outro problema que impediu a cópia bem-sucedida dos ficheiros para o nós.    

* **Falhas de aplicações**

    O processo especificado pela linha de comandos da tarefa também pode falhar. Considera-se que o processo falhou quando o processo que a tarefa executa devolve um código de saída diferente de zero (veja *Códigos de saída de tarefas* na secção seguinte).

    Relativamente às falhas de aplicações, pode configurar o Batch para repetir automaticamente a tarefa até um número de vezes especificado.

* **Falhas de restrições**

    Pode definir uma restrição que especifica a duração de execução máxima de um trabalho, a *maxWallClockTime*. Isto pode ser útil para terminar tarefas que não progridem.

    Quando o tempo máximo tiver sido excedido, a tarefa é marcada como *concluída*, mas o código de saída é definido como `0xC000013A` e o campo *schedulingError* é marcado como `{ category:"ServerError", code="TaskEnded"}`.

### <a name="debugging-application-failures"></a>Depurar falhas de aplicações
* `stderr` e `stdout`

    Durante a execução, uma aplicação poderá produzir resultados de diagnóstico que pode utilizar para resolver problemas. Como foi mencionado na secção [Ficheiros e diretórios](#files-and-directories) acima, o serviço Batch escreve resultados padrão e resultados de erro padrão nos ficheiros `stdout.txt` e `stderr.txt` no diretório da tarefa no nó de computação. Pode utilizar o portal do Azure ou um dos SDKs do Batch para transferir estes ficheiros. Por exemplo, pode utilizar [ComputeNode.GetNodeFile][net_getfile_node] e [CloudTask.GetNodeFile][net_getfile_task] na biblioteca .NET do Batch, para obter estes e outros ficheiros para fins de resolução de problemas.

* **Códigos de saída de tarefa**

    Conforme mencionado anteriormente, o serviço Batch marca uma tarefa como falhada se o processo executado por essa tarefa devolver um código de saída diferente de zero. Quando uma tarefa executa um processo, o Batch preenche a propriedade do código de saída dessa tarefa com o *código de devolução do processo*. É importante que tenha em atenção que o código de saída de uma tarefa **não** é determinado pelo serviço do Batch. É determinado um código de saída de tarefas pelo próprio processo ou o sistema operativo no qual o processo foi executado.

### <a name="accounting-for-task-failures-or-interruptions"></a>Explicação de falhas ou interrupções de tarefas
Por vezes, as tarefas podem falhar ou ser interrompidas. A própria aplicação da tarefa pode falhar, o nó no qual a tarefa está a ser executada pode ser reiniciado ou o nó pode ser removido do conjunto durante uma operação de redimensionamento se política de desalocação do conjunto estiver definida para remover nós imediatamente sem aguardar que as tarefas estejam concluídas. Em todos os casos, o Batch pode recolocar automaticamente a tarefa na fila para execução noutro nó.

Também é possível que um problema intermitente faça com que uma tarefa fique suspensa ou demore demasiado tempo a ser executada. Pode definir o intervalo de execução máxima de uma tarefa. Se o intervalo de execução máxima for excedido, o serviço de Batch interrompe a aplicação de tarefas.

### <a name="connecting-to-compute-nodes"></a>Ligar a nós de computação
Pode iniciar sessão remotamente num nó de computação para realizar depurações e resolução de problemas adicionais. Pode utilizar o portal do Azure para transferir um ficheiro do protocolo RDP (Remote Desktop Protocol) para nós do Windows e obter informações de ligação Secure Shell (SSH) para nós do Linux. Também o pode fazer com as APIs do Batch, como, por exemplo, [.NET do Batch][net_rdpfile] ou [Python do Batch](batch-linux-nodes.md#connect-to-linux-nodes-using-ssh).

> [!IMPORTANT]
> Para ligar a um nó através de DRP ou SSH, tem de criar, primeiro, um utilizador no nó. Para tal, pode utilizar o portal do Azure, [adicionar uma conta de utilizador a um nó][rest_create_user] com a API REST do Batch, chamar o método [ComputeNode.CreateComputeNodeUser][net_create_user] no .NET do Batch ou chamar o método [add_user][py_add_user] no módulo Python do Batch.
>
>

### <a name="troubleshooting-problematic-compute-nodes"></a>Resolução de problemas de nós de computação problemáticos
Em situações onde algumas das suas tarefas estejam a falhar, a aplicação cliente ou o serviço Batch podem examinar os metadados das tarefas com falhas para identificar um nó a funcionar incorretamente. Cada nó num conjunto recebe um ID exclusivo e o nó no qual é executada uma tarefa está incluído nos metadados da tarefa. Depois de ter identificado um nó de problema, pode tomar várias medidas no mesmo:

* **Reiniciar o nó** ([REST][rest_reboot] | [.NET][net_reboot])

    Reiniciar o nó pode, por vezes, limpar problemas latentes, como processos bloqueados ou falhados. Tenha em atenção que, se o conjunto utilizar uma tarefa de início ou o seu trabalho utilizar uma tarefa de preparação de trabalho, estas são executados quando o nó é reiniciado.
* **Recriar imagem do nó** ([REST][rest_reimage] | [.NET][net_reimage])

    Isto reinstala o sistema operativo no nó. Tal como acontece com o reinício de um nó, as tarefas de início e as tarefas de preparação da tarefa são novamente executadas depois de ter sido recriada a imagem do nó.
* **Remover o nó do conjunto** ([REST][rest_remove] | [.NET][net_remove])

    Por vezes, é necessário remover completamente o nó do conjunto.
* **Desativar o agendamento de tarefas no nó** ([REST][rest_offline] | [.NET][net_offline])

    Esta ação coloca o nó offline de forma eficaz, para que não lhe sejam atribuídas mais tarefas, mas permite que o nó permaneça em execução e no conjunto. Isto permite-lhe investigar melhor a causa das falhas sem perder os dados da tarefa com falhas e sem que o nó cause mais falhas nas tarefas. Por exemplo, pode desativar o agendamento de tarefas no nó e, em seguida, [iniciar sessão remotamente](#connecting-to-compute-nodes) para examinar os registos de eventos do nó ou realizar outras ações de resolução de problemas. Depois de terminar a investigação, pode colocar novamente o nó online ao ativar o agendamento de tarefas ([REST][rest_online] | [.NET][net_online]) ou realizar uma das outras ações descritas acima.

> [!IMPORTANT]
> Em cada ação descrita nesta secção -- reiniciar, recriar imagem, remover e desativar o agendamento de tarefas --, pode especificar a forma como as tarefas atualmente em execução no nó são processadas quando realizar a ação. Por exemplo, quando desativar o agendamento de tarefas num nó com a biblioteca de cliente .NET do Batch, pode especificar um valor de enumeração [DisableComputeNodeSchedulingOption][net_offline_option] para especificar se pretende **Terminar** as tarefas em execução, **Recolocá-las na fila** para agendamento noutros nós ou permitir que as tarefas em execução sejam concluídas antes de realizar a ação (**TaskCompletion**).
>
>

## <a name="next-steps"></a>Passos seguintes
* Saiba mais sobre o [Ferramentas e APIs do Batch](batch-apis-tools.md) disponíveis para criação de soluções para o Batch.
* Ver um guia passo a passo para um exemplo de aplicação do Batch, em [Introdução à biblioteca do Azure Batch para .NET](batch-dotnet-get-started.md). Também existe uma [versão para Python](batch-python-tutorial.md) do tutorial que executa uma carga de trabalho em nós de computação do Linux.
* Transfira e crie o projeto de exemplo [Batch Explorer][github_batchexplorer] para utilização enquanto programa as suas soluções do Batch. Ao utilizar o Batch Explorer, pode realizar o seguinte e muito mais:

  * Monitorizar e manipular conjuntos e tarefas na sua conta do Batch
  * Transferir `stdout.txt`, `stderr.txt` e outros ficheiros a partir de nós
  * Criar utilizadores em nós e transferir ficheiros RDP para início de sessão remoto
* Saiba como [criar conjuntos de nós de computação do Linux](batch-linux-nodes.md).
* Visite o [fórum do Azure Batch][batch_forum] no MSDN. O fórum é o local certo para fazer perguntas, quer esteja apenas a aprender ou seja já perito no Batch.

[1]: ./media/batch-api-basics/node-folder-structure.png

[azure_storage]: https://azure.microsoft.com/services/storage/
[batch_forum]: https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurebatch
[cloud_service_sizes]: ../cloud-services/cloud-services-sizes-specs.md
[msmpi]: https://msdn.microsoft.com/library/bb524831.aspx
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_sample_taskdeps]:  https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/TaskDependencies
[github_batchexplorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[batch_net_api]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[msdn_env_vars]: https://msdn.microsoft.com/library/azure/mt743623.aspx
[net_cloudjob_jobmanagertask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobmanagertask.aspx
[net_cloudjob_priority]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.priority.aspx
[net_cloudpool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx
[net_cloudtask_env]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.environmentsettings.aspx
[net_create_cert]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificateoperations.createcertificate.aspx
[net_create_user]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.createcomputenodeuser.aspx
[net_getfile_node]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.getnodefile.aspx
[net_getfile_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.getnodefile.aspx
[net_job_env]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.commonenvironmentsettings.aspx
[net_multiinstancesettings]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.aspx
[net_onalltaskscomplete]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.onalltaskscomplete.aspx
[net_rdp]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.getrdpfile.aspx
[net_reboot]: https://msdn.microsoft.com/library/azure/mt631495.aspx
[net_reimage]: https://msdn.microsoft.com/library/azure/mt631496.aspx
[net_remove]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.removefrompoolasync.aspx
[net_offline]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.disableschedulingasync.aspx
[net_online]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.enableschedulingasync.aspx
[net_offline_option]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.common.disablecomputenodeschedulingoption.aspx
[net_rdpfile]: https://msdn.microsoft.com/library/azure/Mt272127.aspx
[vnet]: https://msdn.microsoft.com/library/azure/dn820174.aspx#bk_netconf

[py_add_user]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.operations.html#azure.batch.operations.ComputeNodeOperations.add_user

[batch_rest_api]: https://msdn.microsoft.com/library/azure/Dn820158.aspx
[rest_add_job]: https://msdn.microsoft.com/library/azure/mt282178.aspx
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[rest_add_cert]: https://msdn.microsoft.com/library/azure/dn820169.aspx
[rest_add_task]: https://msdn.microsoft.com/library/azure/dn820105.aspx
[rest_create_user]: https://msdn.microsoft.com/library/azure/dn820137.aspx
[rest_get_task_info]: https://msdn.microsoft.com/library/azure/dn820133.aspx
[rest_job_schedules]: https://msdn.microsoft.com/library/azure/mt282179.aspx
[rest_multiinstance]: https://msdn.microsoft.com/library/azure/mt637905.aspx
[rest_multiinstancesettings]: https://msdn.microsoft.com/library/azure/dn820105.aspx#multiInstanceSettings
[rest_update_job]: https://msdn.microsoft.com/library/azure/dn820162.aspx
[rest_rdp]: https://msdn.microsoft.com/library/azure/dn820120.aspx
[rest_reboot]: https://msdn.microsoft.com/library/azure/dn820171.aspx
[rest_reimage]: https://msdn.microsoft.com/library/azure/dn820157.aspx
[rest_remove]: https://msdn.microsoft.com/library/azure/dn820194.aspx
[rest_offline]: https://msdn.microsoft.com/library/azure/mt637904.aspx
[rest_online]: https://msdn.microsoft.com/library/azure/mt637907.aspx

[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/

