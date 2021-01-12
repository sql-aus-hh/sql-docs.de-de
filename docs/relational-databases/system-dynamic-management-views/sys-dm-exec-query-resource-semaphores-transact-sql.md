---
description: sys.dm_exec_query_resource_semaphores (Transact-SQL)
title: sys.dm_exec_query_resource_semaphores (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_exec_query_resource_semaphores_TSQL
- dm_exec_query_resource_semaphores_TSQL
- sys.dm_exec_query_resource_semaphores
- dm_exec_query_resource_semaphores
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_query_resource_semaphores dynamic management view
ms.assetid: e43a2aa9-dd52-4c89-911e-1a7d05f7ffbb
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0a9ec322e5f19cab5e8506e5e0dbc05a3e55e900
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096561"
---
# <a name="sysdm_exec_query_resource_semaphores-transact-sql"></a>sys.dm_exec_query_resource_semaphores (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Gibt Informationen zum aktuellen Status des Abfrageressourcensemaphors in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] zurück. **sys.dm_exec_query_resource_semaphores** stellt den allgemeinen Speicherstatus der Abfrage Ausführung bereit und ermöglicht es Ihnen zu bestimmen, ob das System auf genügend Arbeitsspeicher zugreifen kann. Diese Ansicht ergänzt die Arbeitsspeicher Informationen, die von [sys.dm_os_memory_clerks](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-clerks-transact-sql.md) abgerufen wurden, um ein umfassendes Bild des Server Speicherstatus bereitzustellen. **sys.dm_exec_query_resource_semaphores** gibt eine Zeile für das reguläre Ressourcen Semaphor und eine weitere Zeile für das Ressourcen Semaphor für kleine Abfragen zurück. Es gibt zwei Anforderungen an ein kleines Abfrage-Semaphor:  
  
-   Die angeforderte Arbeitsspeicher Zuweisung sollte weniger als 5 MB betragen.  
  
-   Die Abfrage Kosten sollten weniger als drei Kosteneinheiten betragen.  
  
> [!NOTE]  
>  Um dies von oder aus aufzurufen [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , verwenden Sie den Namen **sys.dm_pdw_nodes_exec_query_resource_semaphores**.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**resource_semaphore_id**|**smallint**|Nicht eindeutige ID des Ressourcensemaphors. 0 für das normale Ressourcensemaphor und 1 für das Ressourcensemaphor für kleine Abfragen.|  
|**target_memory_kb**|**bigint**|Zuweisungsziel in Kilobytes.|  
|**max_target_memory_kb**|**bigint**|Maximal mögliches Ziel in Kilobytes. NULL für das Ressourcensemaphor für kleine Abfragen.|  
|**total_memory_kb**|**bigint**|Der vom Ressourcensemaphor belegte Arbeitsspeicher in Kilobytes. Wenn das System nicht genügend Arbeitsspeicher aufweist oder häufig erzwungener minimaler Arbeitsspeicher gewährt wird, kann dieser Wert größer als die Werte für **target_memory_kb** oder **max_target_memory_kb** sein. Der Gesamtarbeitsspeicher ist die Summe aus dem verfügbaren und dem zugewiesenen Arbeitsspeicher.|  
|**available_memory_kb**|**bigint**|Der für eine neue Zuweisung verfügbare Arbeitsspeicher in Kilobytes.|  
|**granted_memory_kb**|**bigint**|Der insgesamt zugewiesene Arbeitsspeicher in Kilobytes.|  
|**used_memory_kb**|**bigint**|Der physisch verwendete Teil des zugewiesenen Arbeitsspeichers in Kilobytes.|  
|**grantee_count**|**int**|Die Anzahl aktiver Abfragen, deren Zuweisungen erfüllt wurden.|  
|**waiter_count**|**int**|Die Anzahl von Abfragen, die auf die Erfüllung ihrer Zuweisungen warten.|  
|**timeout_error_count**|**bigint**|Die Gesamtanzahl von Timeoutfehlern seit dem Start des Servers. NULL für das Ressourcensemaphor für kleine Abfragen.|  
|**forced_grant_count**|**bigint**|Die Gesamtanzahl erzwungener Zuweisungen des minimalen Arbeitsspeichers seit dem Start des Servers. NULL für das Ressourcensemaphor für kleine Abfragen.|  
|**pool_id**|**int**|ID des Ressourcenpools, zu dem dieses Ressourcensemaphor gehört.|  
|**pdw_node_id**|**int**|**Gilt für**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Der Bezeichner für den Knoten, auf dem sich diese Distribution befindet.|  
  
## <a name="permissions"></a>Berechtigungen  

In [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ist die- `VIEW SERVER STATE` Berechtigung erforderlich.   
Bei den Dienst Zielen "Basic", "S0" und "S1" in SQL-Datenbank ist für Datenbanken in Pools für elastische Datenbanken `Server admin` oder ein `Azure Active Directory admin` Konto erforderlich. Für alle anderen SQL-Datenbank-Dienst Ziele `VIEW DATABASE STATE` ist die Berechtigung in der Datenbank erforderlich.   
  
## <a name="remarks"></a>Bemerkungen  
 Abfragen mithilfe dynamischer Verwaltungssichten, die ORDER BY oder Aggregate enthalten, können die Arbeitsspeichernutzung erhöhen und so zu dem Problem beitragen, das mit ihnen behandelt werden soll.  
  
 Verwenden Sie **sys.dm_exec_query_resource_semaphores** zur Problembehandlung, aber schließen Sie es nicht in Anwendungen ein, die zukünftige Versionen von verwenden werden [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Mit der Ressourcenkontrollen-Funktion kann ein Datenbankadministrator Serverressourcen auf Ressourcenpools verteilen, bis zu maximal 64 Pools. In [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und höher verhält sich jeder Pool wie eine kleine unabhängige Serverinstanz und erfordert 2 Semaphore.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Dynamische Verwaltungs Sichten und-Funktionen im Zusammenhang mit der Ausführung &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)   
 [sys.dm_exec_query_memory_grants &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-memory-grants-transact-sql.md)  
  
  


