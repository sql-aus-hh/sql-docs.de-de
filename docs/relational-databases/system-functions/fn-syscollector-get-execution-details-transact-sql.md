---
description: fn_syscollector_get_execution_details (Transact-SQL)
title: fn_syscollector_get_execution_details (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- fn_syscollector_get_execution_details_TSQL
- fn_syscollector_get_execution_details
dev_langs:
- TSQL
helpviewer_keywords:
- fn_syscollector_get_execution_details function
ms.assetid: d59ddf0c-72c0-4c57-bc83-aef260e4e105
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 40201cb1fed3938da075b3fd5e97380eb0dddfc5
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093848"
---
# <a name="fn_syscollector_get_execution_details-transact-sql"></a>fn_syscollector_get_execution_details (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Gibt einen Teil des [!INCLUDE[ssIS](../../includes/ssis-md.md)] Protokolls (sysssislog) zurück, der mit dem package_execution_id für das angegebene Paket übereinstimmt. Die Tabelle enthält eine Zeile für jeden Protokollierungseintrag, der zur Laufzeit von Paketen oder deren Tasks und Containern generiert wird.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
fn_syscollector_get_execution_details ( log_id )  
```  
  
## <a name="arguments"></a>Argumente  
 *log_id*  
 Der lokale eindeutige Bezeichner für das Ausführungsprotokoll. *log_id* ist **int**  
  
## <a name="table-returned"></a>Zurückgegebene Tabelle  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|id|**int**|Der eindeutige Bezeichner des Protokollierungseintrags.|  
|Ereignis|**sysname**|Der Name des Ereignisses, das den Protokollierungseintrag generiert hat.|  
|computer|**nvarchar**|Der Computer, auf dem das Paket ausgeführt wurde, als der Protokollierungseintrag generiert wurde.|  
|Operator|**nvarchar**|Der Benutzername der Person oder des Agents, die bzw. der das Paket ausgeführt hat, das den Protokollierungseintrag generiert hat.|  
|source|**nvarchar**|Der Name der ausführbaren Datei, die den Protokollierungseintrag generiert hat.|  
|sourceid|**uniqueidentifier**|GUID der ausführbaren Datei, die den Protokollierungseintrag generiert hat.|  
|executionid|**uniqueidentifier**|GUID der Ausführungsinstanz der ausführbaren Datei, die den Protokollierungseintrag generiert hat.|  
|starttime|**datetime**|Die Uhrzeit, zu der die Paketausführung gestartet wurde.|  
|endtime|**datetime**|Die Uhrzeit, zu der das Paket abgeschlossen wurde.|  
|datacode|**int**|Ein ganzzahliger Wert, der das dem Protokolleintrag zugeordnete Ereignis angibt. "0" gibt an, dass das Ereignis keinen Bezeichner bereitgestellt hat.|  
|databytes|**image**|Ein Bytearray, das einen Rückgabewert identifiziert.|  
|message|**nvarchar**|Eine Beschreibung des Ereignisses sowie die mit dem Ereignis verknüpften Informationen.|  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die SELECT-Berechtigung für **dc_operator**.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Aktivieren der Paket Protokollierung in SQL Server Data Tools](../../integration-services/performance/integration-services-ssis-logging.md#server_logging)   
 [Datensammlung](../../relational-databases/data-collection/data-collection.md)  
  
  
