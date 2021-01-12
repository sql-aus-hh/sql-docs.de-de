---
description: sys.conversation_groups (Transact-SQL)
title: sys.conversation_groups (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- conversation_groups_TSQL
- conversation_groups
- sys.conversation_groups
- sys.conversation_groups_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.conversation_groups catalog view
ms.assetid: 3f35815e-2de4-42a2-a972-8f0141dad0b3
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 8efd718c9c5ab4d7c1186d3a85c70c1ced1c51ae
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100652"
---
# <a name="sysconversation_groups-transact-sql"></a>sys.conversation_groups (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Diese Katalogsicht enthält eine Zeile für jede Konversationsgruppe.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**conversation_group_id**|**uniqueidentifier**|Bezeichner für die Konversationsgruppe. Lässt keine NULL-Werte zu.|  
|**service_id**|**int**|Bezeichner für den von Konversationen in dieser Gruppe verwendeten Dienst. Lässt keine NULL-Werte zu.|  
|**is_system**|**bit**|Gibt an, ob dies eine Systeminstanz ist. Lässt NULL-Werte zu.|  
  
## <a name="permissions"></a>Berechtigungen  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Weitere Informationen finden Sie unter [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
  
