---
description: dbo.sysalerts (Transact-SQL)
title: dbo.sysWarnungen (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 10/24/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dbo.sysalerts
- sysalerts_TSQL
- dbo.sysalerts_TSQL
- sysalerts
dev_langs:
- TSQL
helpviewer_keywords:
- sysalerts system table
ms.assetid: a2c2f50d-61f3-4951-996a-add5ad092cc2
author: cawrites
ms.author: chadam
ms.openlocfilehash: ff1e70701882c1740ae91212c8d33ae5e7e3040b
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094870"
---
# <a name="dbosysalerts-transact-sql"></a>dbo.sysalerts (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Enthält eine Zeile für jede Warnung. Eine Warnung ist eine Nachricht, die als Reaktion auf ein Ereignis gesendet wird. Eine Warnung kann Nachrichten über die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Umgebung hinaus weiterleiten. Eine Warnung kann eine E-Mail- oder eine Pagernachricht sein. Außerdem kann eine Warnung einen Task generieren.  Diese Tabelle wird in der **msdb** -Datenbank gespeichert.
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**id**|**int**|Warnungs-ID.|  
|**name**|**sysname**|Name der Warnung.|  
|**event_source**|**nvarchar (100)**|Die Quelle des Ereignisses: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**event_category_id**|**int**|Für zukünftige Verwendung reserviert.|  
|**event_id**|**int**|Für zukünftige Verwendung reserviert.|  
|**message_id**|**int**|Benutzerdefinierte Nachrichten-ID oder Verweis auf die **sysmessages** -Nachricht, die diese Warnung auslöst.|  
|**severity**|**int**|Schweregrad, der diese Warnung auslöst.|  
|**enabled**|**tinyint**|Status der Warnung:<br /><br /> **0** = deaktiviert.<br /><br /> **1** = aktiviert.|  
|**delay_between_responses**|**int**|Wartezeit in Sekunden zwischen den Benachrichtigungen für diese Warnung.|  
|**last_occurrence_date**|**int**|Letztes Auftreten (Datum) der Warnung.|  
|**last_occurrence_time**|**int**|Letztes Auftreten (Tageszeit) der Warnung.|  
|**last_response_date**|**int**|Letzte Benachrichtigung (Datum) der Warnung.|  
|**last_response_time**|**int**|Letzte Benachrichtigung (Tageszeit) der Warnung.|  
|**notification_message**|**nvarchar(512)**|Zusätzliche, mit der Warnung gesendete Informationen.|  
|**include_event_description**|**tinyint**|Bitmaske, die angibt, ob die Ereignis Beschreibung per E-Mail, Pager oder net send gesendet wird. Werte finden Sie unten im Diagramm.|  
|**database_name**|**nvarchar(512)**|Datenbank, in der die Warnung auftreten muss, damit sie ausgelöst wird.|  
|**event_description_keyword**|**nvarchar (100)**|Muster, dem der Fehler entsprechen muss, damit die Warnung ausgelöst wird.|  
|**occurrence_count**|**int**|Anzahl der Warnungsauftritte.|  
|**count_reset_date**|**int**|Tag (Datum), an dem die Anzahl auf **0** zurückgesetzt wird.|  
|**count_reset_time**|**int**|Tageszeit, zu der die Anzahl auf **0** zurückgesetzt wird.|  
|**job_id**|**uniqueidentifier**|ID des Tasks, der ausgeführt wird, wenn die Warnung auftritt.|  
|**has_notification**|**int**|Anzahl der Operatoren, die eine E-Mail-Benachrichtigung erhalten, wenn die Warnung auftritt.|  
|**flags**|**int**|Reserviert.|  
|**performance_condition**|**nvarchar(512)**|Reserviert.|  
|**category_id**|**int**|Reserviert.|  
  
 ## <a name="remarks"></a>Bemerkungen

In der folgenden Tabelle werden die Werte für die include_event_description Bitmaske angezeigt. Der Dezimalwert wird von dbo.sysWarnungen zurückgegeben. 

|Decimal | BINARY | Bedeutung |
|------|------|------|
|0 |0000 |keine Meldung |
|1 |0001 |email |
|2 |0010 |pager |
|3 |0011 |Pager und e-Mail |
|4 |0100 |NET SEND |
|5 |0101 |NET SEND und e-Mail |
|6 |0110 |NET SEND und Pager |
|7 |0111 |NET SEND, Pager und e-Mail |
  
