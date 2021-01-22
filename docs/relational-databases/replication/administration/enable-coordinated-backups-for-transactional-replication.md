---
title: Aktivieren koordinierter Sicherungen (transaktional)
description: Erfahren Sie, wie Sie auf der Verteilungsdatenbank die koordinierte Sicherung aktivieren, sodass das Transaktionsprotokoll der Veröffentlichungsdatenbank der Transaktionsreplikation erst dann abgeschnitten wird, nachdem die an den Verteiler weitergegebenen Transaktionen gesichert wurden.
ms.custom: seo-lt-2019
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
dev_langs:
- TSQL
helpviewer_keywords:
- transactional replication, backup and restore
- sp_replicationdboption
- sync with backup [SQL Server replication]
- coordinated backups [SQL Server replication]
- backups [SQL Server replication], transactional replication
ms.assetid: 73a914ba-8b2d-4f4d-ac1b-db9bac676a30
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 34911188162ac5b63f5a43d510d10d503820d200
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170472"
---
# <a name="enable-coordinated-backups-for-transactional-replication"></a>Aktivieren koordinierter Sicherungen für die Transaktionsreplikation
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Wenn Sie eine Datenbank für die Transaktionsreplikation aktivieren, können Sie angeben, dass alle Transaktionen gesichert werden müssen, bevor sie an die Verteilungsdatenbank übermittelt werden. Zudem können Sie auf der Verteilungsdatenbank die koordinierte Sicherung aktivieren, sodass das Transaktionsprotokoll der Veröffentlichungsdatenbank erst dann abgeschnitten wird, nachdem die an den Verteiler weitergegebenen Transaktionen gesichert wurden. Weitere Informationen finden Sie unter [Strategien zum Sichern und Wiederherstellen einer Momentaufnahme- und Transaktionsreplikation](../../../relational-databases/replication/administration/strategies-for-backing-up-and-restoring-snapshot-and-transactional-replication.md).  
  
### <a name="to-enable-coordinated-backups-for-a-database-published-with-transactional-replication"></a>So aktivieren Sie koordinierte Sicherungen für eine mit der Transaktionsreplikation veröffentlichte Datenbank  
  
1.  Verwenden Sie auf dem Verleger die `SELECT DATABASEPROPERTYEX(DB_NAME(),'IsSyncWithBackup')` [DATABASEPROPERTYEX &#40;Transact-SQL&#41;](../../../t-sql/functions/databasepropertyex-transact-sql.md)-Funktion, um die **IsSyncWithBackup**-Eigenschaft der Veröffentlichungsdatenbank zurückzugeben. Wenn die Funktion **1** zurückgibt, sind bereits koordinierte Sicherungen für die veröffentlichte Datenbank aktiviert.  
  
2.  Wenn die Funktion in Schritt 1 **0** zurückgibt, führen Sie [sp_replicationdboption &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-replicationdboption-transact-sql.md) auf dem Verleger für die Veröffentlichungsdatenbank aus. Geben Sie einen Wert **sync with backup** für **\@optname** und **TRUE** für **\@value** an.  
  
    > [!NOTE]  
    >  Wenn Sie die Option **sync with backup** in **false** ändern, wird der Abschneidepunkt der Veröffentlichungsdatenbank nach Ausführen des Protokolllese-Agents aktualisiert oder, wenn der Protokolllese-Agent fortlaufend ausgeführt wird, nach einem bestimmten Zeitintervall. Das Maximalintervall wird vom **–MessageInterval** -Agentparameter gesteuert (standardmäßig 30 Sekunden).  
  
### <a name="to-enable-coordinated-backups-for-a-distribution-database"></a>So aktivieren Sie koordinierte Sicherungen für eine Verteilungsdatenbank  
  
1.  Verwenden Sie auf dem Verteiler die [DATABASEPROPERTYEX &#40;Transact-SQL&#41;](../../../t-sql/functions/databasepropertyex-transact-sql.md)-Funktion, um die **IsSyncWithBackup**-Eigenschaft der Verteilungsdatenbank zurückzugeben. Wenn die Funktion **1** zurückgibt, sind bereits koordinierte Sicherungen für die Verteilungsdatenbank aktiviert.  
  
2.  Wenn die Funktion in Schritt 1 **0** zurückgibt, führen Sie [sp_replicationdboption &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-replicationdboption-transact-sql.md) auf dem Verteiler für die Verteilungsdatenbank aus. Geben Sie einen Wert **sync with backup** für **\@optname** und **TRUE** für **\@value** an.  
  
### <a name="to-disable-coordinated-backups"></a>So deaktivieren Sie koordinierte Sicherungen  
  
1.  Führen Sie [sp_replicationdboption &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-replicationdboption-transact-sql.md) auf dem Verleger für die Veröffentlichungsdatenbank oder auf dem Verteiler für die Verteilungsdatenbank aus. Geben Sie einen Wert **sync with backup** für **\@optname** und **FALSE** für **\@value** an.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-retrieve-the-issyncwithbackup-property-for-the-current-database"></a>A. Rufen Sie die `IsSyncWithBackup`-Eigenschaft für die aktuelle Datenbank ab.

In diesem Beispiel wird die `IsSyncWithBackup`-Eigenschaft für die aktuelle Datenbank zurückgegeben:
  
```sql
SELECT DATABASEPROPERTYEX(DB_NAME(),'IsSyncWithBackup')`
```

### <a name="b-retrieve-the-issyncwithbackup-property-for-a-specific-database"></a>B. Abrufen der `IsSyncWithBackup`-Eigenschaft für eine bestimmte Datenbank

In diesem Beispiel wird die `IsSyncWithBackup`-Eigenschaft für die `NameOfDatabaseToCheck`-Datenbank zurückgegeben:
  
```sql
SELECT DATABASEPROPERTYEX('NameOfDatabaseToCheck','IsSyncWithBackup')`
```
