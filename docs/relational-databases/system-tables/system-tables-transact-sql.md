---
description: Systemtabellen (Transact-SQL)
title: System Tabellen (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- status information [SQL Server]
- tables [SQL Server], system tables
- information retrieval [SQL Server]
- status information [SQL Server], system tables
- system information [SQL Server]
- system tables [SQL Server]
- system tables [SQL Server], about system tables
- system tables [SQL Server], retrieving information from
- retrieving system table information
ms.assetid: 56b8ad51-930c-4e5c-8d99-8c939d5b70ac
author: cawrites
ms.author: chadam
ms.openlocfilehash: a753d453572534373be1b18149a41ea822a3f1f6
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102662"
---
# <a name="system-tables-transact-sql"></a>Systemtabellen (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  In den Themen in diesem Abschnitt werden die Systemtabellen in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] beschrieben.  
  
 Die Systemtabellen sollten von keinem Benutzer direkt geändert werden. Versuchen Sie z. B. nicht, Systemtabellen mit den Anweisungen DELETE, UPDATE oder INSERT oder benutzerdefinierten Triggern zu ändern.  
  
 Das Verweisen auf dokumentierte Spalten in Systemtabellen ist zulässig. Viele der Spalten in Systemtabellen sind jedoch nicht dokumentiert. Anwendungen sollten nicht so geschrieben werden, dass sie undokumentierte Spalten direkt abfragen. Zum Abrufen von Informationen aus den Systemtabellen sollten Anwendungen vielmehr die folgenden Komponenten verwenden:  
  
-   Gespeicherte Systemprozeduren  
  
-   [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisungen und -Funktionen  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Management Objects (SMO)  
  
-   Replikationsverwaltungsobjekte (RMO)  
  
-   Datenbank-API-Katalogfunktionen  
  
 Die Komponenten bilden eine veröffentlichte API zum Abrufen von Systeminformationen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. [!INCLUDE[msCoName](../../includes/msconame-md.md)] erhält die Kompatibilität dieser Komponenten von Version zu Version aufrecht. Das Format der Systemtabellen hängt von der internen Architektur von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ab und kann sich von Version zu Version ändern. Daher müssen Anwendungen, die direkt auf die undokumentierten Spalten der Systemtabellen zugreifen, möglicherweise geändert werden, bevor sie auf eine spätere Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] zugreifen können.  
  
## <a name="in-this-section"></a>In diesem Abschnitt  
 Die Themen im Zusammenhang mit Systemtabellen sind nach den folgenden Funktionen unterteilt:  

:::row:::
    :::column:::
        [Sichern und Wiederherstellen von Tabellen &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/backup-and-restore-tables-transact-sql.md)

        [Change Data Capture-Tabellen &#40;Transact-SQL&#41;](../../relational-databases/system-tables/change-data-capture-tables-transact-sql.md)

        [Tabellen für Datenbank-Wartungspläne &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/database-maintenance-plan-tables-transact-sql.md)

        [Tabellen für erweiterte Ereignisse von SQL Server &#40;Transact-SQL&#41;](../../relational-databases/extended-events/xevents-references-system-objects.md#system-tables)

        [Integration Services Tabellen &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/integration-services-tables-transact-sql.md)
    :::column-end:::
    :::column:::
        [Protokollversandtabellen &#40;Transact-SQL&#41;](../../relational-databases/system-tables/log-shipping-tables-transact-sql.md)

        [Replikationstabellen &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)

        [SQL Server-Agent Tabellen &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/sql-server-agent-tables-transact-sql.md)

        [sys.sysoledbusers &#40;Transact-SQL-&#41;](../../relational-databases/system-compatibility-views/sys-sysoledbusers-transact-sql.md)

        [systranschemas &#40;Transact-SQL-&#41;](../../relational-databases/system-views/systranschemas-transact-sql.md)
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>Weitere Informationen  
 [Kompatibilitäts Sichten &#40;Transact-SQL-&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)   
 [Katalogsichten &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
