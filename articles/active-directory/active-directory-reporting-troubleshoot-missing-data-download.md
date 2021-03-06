---
title: "Resolução de problemas: dados em falta nos registos de atividades transferidos do Azure Active Directory | Microsoft Docs"
description: "Fornece uma resolução para os dados em falta nos registos de atividades transferidos do Azure Active Directory."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: ffce7eb1-99da-4ea7-9c4d-2322b755c8ce
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/15/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.translationtype: Human Translation
ms.sourcegitcommit: 9ae7e129b381d3034433e29ac1f74cb843cb5aa6
ms.openlocfilehash: 9109c698e4e8b43eeb7534c338adc99476012a3f
ms.contentlocale: pt-pt
ms.lasthandoff: 05/08/2017

---

# Não consigo encontrar dados nos registos de atividades do Azure Active Directory que transferi
<a id="i-cant-find-any-data-in-the-azure-active-directory-activity-logs-i-have-downloaded" class="xliff"></a>


## Sintomas
<a id="symptoms" class="xliff"></a>

Transferi os registos de atividades (auditorias ou inícios de sessão) e não vejo todos os registos para o período de tempo que escolhi. Porquê? 

 ![Relatórios](./media/active-directory-reporting-troubleshoot-missing-data-download/01.png)
 

## Causa
<a id="cause" class="xliff"></a>

Quando transfere registos de atividades no portal do Azure, limitamos o tamanho a 120K registos, ordenados pelos mais recentes. 

## Resolução
<a id="resolution" class="xliff"></a>

Pode tirar partido das [APIs de Relatórios do Azure AD](active-directory-reporting-api-getting-started.md) para obter até um milhão de registos num determinado período. A nossa abordagem recomendada é executar um script agendado que liga às APIs de relatórios para obter registos de uma forma incremental durante um período de tempo (por exemplo, diária ou semanalmente).

## Passos seguintes
<a id="next-steps" class="xliff"></a>
Veja as [FAQ de relatórios do Azure Active Directory](active-directory-reporting-faq.md).


