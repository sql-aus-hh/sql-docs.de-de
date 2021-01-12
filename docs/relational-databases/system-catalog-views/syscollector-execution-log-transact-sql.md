---
description: syscollector_execution_log (Transact-SQL)
title: syscollector_execution_log (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- syscollector_execution_log_TSQL
- syscollector_execution_log
dev_langs:
- TSQL
helpviewer_keywords:
- data collector view
- syscollector_execution_log view
ms.assetid: 11554d64-0426-42ce-b7ce-5591f67864d2
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: f6c9cc330c87debc0a80a9d282ea8c40c904c59c
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094244"
---
# <a name="syscollector_execution_log-transact-sql"></a>syscollector_execution_log (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Stellt Informationen aus dem Ausführungsprotokoll für einen Sammlungssatz oder ein Paket bereit.   
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|log_id|**bigint**|Identifiziert jede Sammlungssatzausführung. Wird verwendet, um diese Sicht mit anderen ausführlichen Protokollen zu verknüpfen. Lässt keine NULL-Werte zu.|  
|parent_log_id|**bigint**|Identifiziert das übergeordnete Paket oder den Sammlungssatz. Lässt keine NULL-Werte zu. Die IDs werden in der Über-/Unterordnungsbeziehung verkettet. Auf diese Weise können Sie bestimmen, welches Paket von welchem Sammlungssatz gestartet wurde. Diese Sicht gruppiert die Protokolleinträge nach ihren über- und untergeordneten Verknüpfungen und rückt die Namen der Pakete ein, sodass die Aufrufkette übersichtlich angezeigt wird.|  
|collection_set_id|**int**|Gibt den Sammlungssatz oder das Paket an, der bzw. das von diesem Protokolleintrag dargestellt wird. Lässt keine NULL-Werte zu.|  
|collection_item_id|**int**|Identifiziert ein Sammlungselement. Lässt NULL-Werte zu.|  
|start_time|**datetime**|Die Startzeit für den Sammlungssatz oder das Paket. Lässt keine NULL-Werte zu.|  
|last_iteration_time|**datetime**|Für kontinuierlich ausgeführte Pakete der letzte Zeitpunkt, zu dem das Paket eine Momentaufnahme aufgezeichnet hat. Lässt NULL-Werte zu.|  
|finish_time|**datetime**|Der Zeitpunkt, zu dem der Testlauf für abgeschlossene Pakete und Sammlungssätze beendet wurde. Lässt NULL-Werte zu.|  
|runtime_execution_mode|**smallint**|Gibt an, ob die Sammlungs Satz Aktivität Daten gesammelt oder Daten hochgeladen hat. Lässt NULL-Werte zu.<br /><br /> Gültige Werte:<br /><br /> 0 = Sammlung<br /><br /> 1 = Upload|  
|status|**smallint**|Gibt den aktuellen Status des Sammlungssatzes oder des Pakets an. Lässt keine NULL-Werte zu.<br /><br /> Gültige Werte:<br /><br /> 0 = Wird ausgeführt<br /><br /> 1 = Beendet<br /><br /> 2 = fehlgeschlagen|  
|Operator|**nvarchar(128)**|Gibt an, wer den Sammlungssatz oder das Paket gestartet hat. Lässt keine NULL-Werte zu.|  
|package_id|**uniqueidentifier**|Gibt den Sammlungssatz oder das Paket an, der bzw. das das Protokoll generiert hat. Lässt NULL-Werte zu.|  
|package_name|**nvarchar(4000)**|Der Name des Pakets, von dem dieses Protokoll generiert wurde. Lässt NULL-Werte zu.|  
|package_execution_id|**uniqueidentifier**|Stellt einen Link zur [!INCLUDE[ssIS](../../includes/ssis-md.md)]-Protokolltabelle bereit. Lässt NULL-Werte zu.|  
|failure_message|**nvarchar (2048)**|Wenn bei einem Sammlungssatz oder einem Paket ein Fehler aufgetreten ist, die aktuelle Fehlermeldung für die jeweilige Komponente. Lässt NULL-Werte zu. Um ausführlichere Fehlerinformationen zu erhalten, verwenden Sie die [fn_syscollector_get_execution_details &#40;Transact-SQL-&#41;](../../relational-databases/system-functions/fn-syscollector-get-execution-details-transact-sql.md) -Funktion.|  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert SELECT für dc_operator.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Gespeicherte Prozeduren für den Datensammler &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/data-collector-stored-procedures-transact-sql.md)   
 [Sichten des Datensammlers &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/data-collector-views-transact-sql.md)   
 [Datensammlung](../../relational-databases/data-collection/data-collection.md)  
  
  
