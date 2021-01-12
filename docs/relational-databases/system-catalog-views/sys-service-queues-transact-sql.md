---
description: sys.service_queues (Transact-SQL)
title: sys.service_queues (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.service_queues
- service_queues
- service_queues_TSQL
- sys.service_queues_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.service_queues catalog view
ms.assetid: 9fd9fa76-6128-410c-896f-741e6050143a
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 55a1037bc944c0ad27d60c02b39d26f0d7aa73b6
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096673"
---
# <a name="sysservice_queues-transact-sql"></a>sys.service_queues (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Enthält eine Zeile für jedes Objekt in der Datenbank, bei dem es sich um eine Dienstwarteschlange handelt, wobei **sys.objects.type** = SQ gilt.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**\<inherited columns>**||Eine Liste der Spalten, die diese Sicht erbt, finden Sie unter [sys. Objects &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md).|  
|**max_readers**|**smallint**|Die maximale Anzahl gleichzeitiger Leser, die in der Warteschlange zulässig sind.|  
|**activation_procedure**|**nvarchar (776)**|Dreiteiliger Name der Aktivierungsprozedur.|  
|**execute_as_principal_id**|**int**|Die ID des Datenbankprinzipals EXECUTE AS.<br /><br /> NULL als Standardwert oder bei Verwendung von EXECUTE AS CALLER.<br /><br /> ID des angegebenen Prinzipals, wenn EXECUTE AS Self EXECUTE AS \<principal> .<br /><br /> -2 = EXECUTE AS OWNER.|  
|**is_activation_enabled**|**bit**|1 = Aktivierung ist aktiviert.|  
|**is_receive_enabled**|**bit**|1 = Empfangen ist aktiviert.|  
|**is_enqueue_enabled**|**bit**|1 = Einreihen in Warteschlange ist aktiviert.|  
|**is_retention_enabled**|**bit**|1 = Nachrichten werden bis zum Ende des Dialogs beibehalten.|  
|**is_poison_message_handling_enabled**|**bit**|**Gilt für**:  [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und höher.<br /><br /> 1 = Behandlung nicht verarbeitbarer Nachrichten ist aktiviert.|  
  
## <a name="permissions"></a>Berechtigungen  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Weitere Informationen finden Sie unter [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Katalogsichten für Objekte &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Katalogsichten &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
