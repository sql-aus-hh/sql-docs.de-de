---
description: Sichern und Wiederherstellen von Tabellen (Transact-SQL)
title: Sichern und Wiederherstellen von Tabellen (Transact-SQL) | Microsoft-Dokumentation
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
- system tables [SQL Server], backup tables
- backup system tables [SQL Server]
- system tables [SQL Server], restore tables
- restore system tables [SQL Server]
ms.assetid: aa615add-54e6-40f5-8b55-3728b26884ee
author: cawrites
ms.author: chadam
ms.openlocfilehash: 1f82c8cbfcf2871912a2a83a02f3f29be294a7ba
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098758"
---
# <a name="backup-and-restore-tables-transact-sql"></a>Sichern und Wiederherstellen von Tabellen (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  In den Themen in diesem Abschnitt werden die Systemtabellen beschrieben, in denen die von Datenbank-Sicherungs- und Datenbank-Wiederherstellungsvorgängen verwendeten Informationen gespeichert werden.  
  
## <a name="in-this-section"></a>In diesem Abschnitt  
 [backupfile](../../relational-databases/system-tables/backupfile-transact-sql.md)  
 Enthält eine Zeile für jede Daten- oder Protokolldatei einer Datenbank.  
  
 [backupfilegroup](../../relational-databases/system-tables/backupfilegroup-transact-sql.md)  
 Enthält eine Zeile für jede Dateigruppe in einer Datenbank zum Zeitpunkt der Sicherung.  
  
 [backupmediafamily](../../relational-databases/system-tables/backupmediafamily-transact-sql.md)  
 Enthält eine Zeile für jede Medienfamilie.  
  
 [backupmediaset](../../relational-databases/system-tables/backupmediaset-transact-sql.md)  
 Enthält eine Zeile für jeden Sicherungsmediensatz.  
  
 [backupset](../../relational-databases/system-tables/backupset-transact-sql.md)  
 Enthält eine Zeile für jeden Sicherungssatz.  
  
 [logmarkhistory](../../relational-databases/system-tables/logmarkhistory-transact-sql.md)  
 Enthält eine Zeile für jede markierte Transaktion, für die ein Commit ausgeführt wurde.  
  
 [restorefile](../../relational-databases/system-tables/restorefile-transact-sql.md)  
 Enthält eine Zeile für jede wiederhergestellte Datei. Hierzu gehören Dateien, die indirekt nach Dateigruppen Namen wieder hergestellt wurden.  
  
 [restorefilegroup](../../relational-databases/system-tables/restorefilegroup-transact-sql.md)  
 Enthält eine Zeile für jede wiederhergestellte Dateigruppe.  
  
 [restorehistory](../../relational-databases/system-tables/restorehistory-transact-sql.md)  
 Enthält eine Zeile für jeden Wiederherstellungsvorgang.  
  
 [suspect_pages](../../relational-databases/system-tables/suspect-pages-transact-sql.md)  
 Enthält eine Zeile für jede Seite mit einem Fehler vom Typ 824 (maximal 1.000 Zeilen).  
  
 [sysopentapes](../../relational-databases/system-tables/sysopentapes-transact-sql.md)  
 Enthält eine Zeile für jedes aktuell geöffnete Bandmedium.  
  
  
