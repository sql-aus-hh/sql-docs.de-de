---
description: logmarkhistory (Transact-SQL)
title: logmarkhistory (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- logmarkhistory
- logmarkhistory_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- logmarkhistory system table
ms.assetid: 5c1becc5-f34e-4869-bf69-dfafab684540
author: cawrites
ms.author: chadam
ms.openlocfilehash: ab2f877b99fdc15cc4543741999c20a9a4924cda
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098680"
---
# <a name="logmarkhistory-transact-sql"></a>logmarkhistory (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Enthält eine Zeile für jede markierte Transaktion, für die ein Commit ausgeführt wurde. Diese Tabelle wird in der **msdb** -Datenbank gespeichert.  
  

|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**database_name**|**nvarchar(128)**|Lokale Datenbank, in der die markierte Transaktion aufgetreten ist|  
|**mark_name**|**nvarchar(128)**|Vom Benutzer bereitgestellter Name für die markierte Transaktion|  
|**description**|**nvarchar(255)**|Vom Benutzer bereitgestellte Beschreibung für die markierte Transaktion Kann den Wert NULL haben.|  
|**user_name**|**nvarchar(128)**|Datenbank-Benutzername, der die markierte Transaktion durchgeführt hat. Kann den Wert NULL haben.|  
|**LSN**|**numeric(25,0)**|Protokollsequenznummer des Transaktionsdatensatzes, in dem die Markierung auftrat|  
|**mark_time**|**datetime**|Bestätigungszeitpunkt der markierten Transaktion (lokale Zeit)|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Wiederherstellen einer Datenbank bis zu einer markierten Transaktion &#40;SQL Server Management Studio&#41;](../../relational-databases/backup-restore/restore-a-database-to-a-marked-transaction-sql-server-management-studio.md)   
 [Wiederherstellen von verwandten Datenbanken mithilfe von markierten Transaktionen &#40;vollständiges Wiederherstellungsmodell&#41;](../../relational-databases/backup-restore/use-marked-transactions-to-recover-related-databases-consistently.md)   
 [Systemtabellen &#40;Transact-SQL&#41;](../../relational-databases/system-tables/system-tables-transact-sql.md)  
  
  
