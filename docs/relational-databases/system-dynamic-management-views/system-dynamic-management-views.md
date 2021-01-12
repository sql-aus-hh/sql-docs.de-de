---
description: Dynamische Verwaltungs Sichten (Transact-SQL)
title: Dynamische Verwaltungs Sichten (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- database scoped dynamic management objects [SQL Server]
- dynamic management objects [SQL Server], about dynamic management objects
- server state information [SQL Server]
- dynamic management functions [SQL Server]
- metadata [SQL Server], dynamic management objects
- dynamic management views [SQL Server]
- DMVs [SQL Server]
- functions [SQL Server], dynamic management
- server scoped dynamic management objects [SQL Server]
- dynamic management objects [SQL Server]
ms.assetid: cf893ecb-0bf6-4cbf-ac00-8a1099e405b1
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 422984471491d402102942304c06f37ef1a14727
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098865"
---
# <a name="system-dynamic-management-views"></a>Dynamische Systemverwaltungssichten
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Dynamische Verwaltungssichten (DMVs) und -funktionen geben Serverstatusinformationen zurück, mit denen der Zustand einer Serverinstanz überwacht, Probleme diagnostiziert und die Leistung optimiert werden kann.  
  
> [!IMPORTANT]  
>  Dynamische Verwaltungssichten und -funktionen geben interne, implementierungsspezifische Statusdaten zurück. Die Schemas und die zurückgegebenen Daten können in zukünftigen Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] geändert werden. Deshalb kann es sein, dass dynamische Verwaltungssichten und -funktionen in zukünftigen Versionen nicht mit den dynamischen Verwaltungssichten und -funktionen in dieser Version kompatibel sind. In zukünftigen Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] erweitert Microsoft möglicherweise die Definition der dynamischen Verwaltungssichten, indem am Ende der Spaltenliste z.B. Spalten hinzugefügt werden. Von der Verwendung der Syntax `SELECT * FROM dynamic_management_view_name` im Produktionscode wird abgeraten, da sich die Anzahl der zurückgegebenen Spalten möglicherweise ändert und Ihre Anwendung dadurch beschädigt werden kann.  
  
 Es gibt zwei Arten von dynamischen Verwaltungssichten und -funktionen:  
  
-   Dynamische Verwaltungssichten und -funktionen mit Serverbereich. Sie erfordern die VIEW SERVER STATE-Berechtigung auf dem Server.  
  
-   Dynamische Verwaltungssichten und -funktionen mit Datenbankbereich. Sie erfordern die VIEW DATABASE STATE-Berechtigung für die Datenbank.  
  
## <a name="querying-dynamic-management-views"></a>Abfragen von dynamischen Verwaltungssichten  
 Auf dynamische Verwaltungssichten kann in [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisungen mithilfe zweiteiliger, dreiteiliger oder vierteiliger Namen verwiesen werden. Auf dynamische Verwaltungsfunktionen kann andererseits in [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisungen mithilfe zweiteiliger oder dreiteiliger Namen verwiesen werden. Auf dynamische Verwaltungssichten und -funktionen kann in [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisungen nicht mithilfe einteiliger Namen verwiesen werden.  
  
 Alle dynamischen Verwaltungssichten und -funktionen sind im sys-Schema vorhanden und verwenden die Benennungskonvention dm_*. Wenn Sie eine dynamische Verwaltungssicht oder -funktion verwenden, müssen Sie vor dem Namen der Sicht oder Funktion das sys-Schema als Präfix einfügen. Führen Sie z. B. die folgende Abfrage aus, um die dynamische Verwaltungssicht dm_os_wait_stats abzufragen:  
  
 ```sql
SELECT wait_type, wait_time_ms  
FROM sys.dm_os_wait_stats;  
```  
  
### <a name="required-permissions"></a>Erforderliche Berechtigungen  
 Zum Abfragen einer dynamischen Verwaltungssicht oder -funktion sind die SELECT-Berechtigung für das Objekt und die VIEW SERVER STATE- oder VIEW DATABASE STATE-Berechtigung erforderlich. Auf diese Weise können Sie den Zugriff eines Benutzers oder eines Anmeldenamens selektiv auf dynamische Verwaltungssichten und -funktionen einschränken. Dazu erstellen Sie zunächst den Benutzer in master und verweigern dem Benutzer dann die SELECT-Berechtigung für die dynamischen Verwaltungssichten oder -funktionen, auf die er keinen Zugriff haben soll. Der Benutzer kann dann diese dynamischen Verwaltungssichten oder -funktionen unabhängig vom Datenbankkontext des Benutzers nicht auswählen.  
  
> [!NOTE]  
>  DENY hat Vorrang. Daher kann ein Benutzer, dem die VIEW SERVER STATE-Berechtigung erteilt, aber die VIEW DATABASE STATE-Berechtigung verweigert wurde, zwar Informationen auf Serverebene, aber keine Informationen auf Datenbankebene anzeigen.  
  
## <a name="in-this-section"></a>In diesem Abschnitt  
 Dynamische Verwaltungssichten und -funktionen wurden in die folgenden Kategorien unterteilt.  

:::row:::
    :::column:::
        [Dynamische Verwaltungs Sichten und-Funktionen für Always on Verfügbarkeits Gruppen (Transact-SQL)](../../relational-databases/system-dynamic-management-views/always-on-availability-groups-dynamic-management-views-functions.md)

        [Dynamische Verwaltungssichten in Bezug auf Change Data Capture &#40;Transact-SQL&#41;](change-data-capture-sys-dm-cdc-errors.md)

        [Dynamische Verwaltungssichten für Änderungsnachverfolgung](change-tracking-sys-dm-tran-commit-table.md)

        [Dynamische Verwaltungs Sichten im Zusammenhang mit der Common Language Runtime &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/common-language-runtime-related-dynamic-management-views-transact-sql.md)

        [Dynamische Verwaltungs Sichten in Verbindung mit der Daten Bank Spiegelung &#40;Transact-SQL-&#41;](database-mirroring-sys-dm-db-mirroring-auto-page-repair.md)

        [Dynamische Verwaltungs Sichten im Zusammenhang mit der Datenbank &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)

        [Execution Related Dynamic Management Views and Functions &#40;Transact-SQL&#41; (Dynamische Verwaltungssichten und Funktionen im Zusammenhang mit der Ausführung (Transact-SQL))](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)

        [Dynamische Verwaltungssichten für erweiterte Ereignisse](../../relational-databases/system-dynamic-management-views/extended-events-dynamic-management-views.md)

        [Dynamische Verwaltungs Sichten für FILESTREAM und FILETABLE &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/filestream-and-filetable-dynamic-management-views-transact-sql.md)

        [Dynamische Verwaltungs Sichten und Funktionen für die voll Text Suche und die semantische Suche &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/full-text-and-semantic-search-dynamic-management-views-functions.md)

        [Dynamische Verwaltungs Sichten und-Funktionen für die georeplikation &#40;Azure SQL-Datenbank&#41;](../../relational-databases/system-dynamic-management-views/geo-replication-dynamic-management-views-and-functions-azure-sql-database.md)

        [Index Related Dynamic Management Views and Functions (Transact-SQL) (Indexbezogene dynamische Verwaltungssichten und -funktionen (Transact-SQL))](../../relational-databases/system-dynamic-management-views/index-related-dynamic-management-views-and-functions-transact-sql.md)

        [In Beziehung stehende dynamische Verwaltungs Sichten und Funktionen &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/i-o-related-dynamic-management-views-and-functions-transact-sql.md)
    :::column-end:::
    :::column:::
        [Dynamische Verwaltungssichten für speicheroptimierte Tabellen (Transact-SQL)](../../relational-databases/system-dynamic-management-views/memory-optimized-table-dynamic-management-views-transact-sql.md)

        [Objektbezogene dynamische Verwaltungssichten und -funktionen &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/object-related-dynamic-management-views-and-functions-transact-sql.md)

        [Abfrage Benachrichtigungen mit dynamischen Verwaltungs Sichten &#40;Transact-SQL-&#41;](query-notifications-sys-dm-qn-subscriptions.md)

        [Dynamische Verwaltungs Sichten im Zusammenhang mit der Replikation &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/replication-related-dynamic-management-views-transact-sql.md)

        [Dynamische Verwaltungssichten in Verbindung mit dem Resource Governor &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/resource-governor-related-dynamic-management-views-transact-sql.md)

        [Sicherheitsbezogene dynamische Verwaltungssichten und -funktionen &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/security-related-dynamic-management-views-and-functions-transact-sql.md)

        [Serverbezogene dynamische Verwaltungssichten und -funktionen (Transact-SQL)](../../relational-databases/system-dynamic-management-views/server-related-dynamic-management-views-and-functions-transact-sql.md)

        [Dynamische Verwaltungssichten in Verbindung mit Service Broker &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/service-broker-related-dynamic-management-views-transact-sql.md)

        [Dynamische Verwaltungs Sichten und Funktionen in Bezug auf räumliche Daten &#40;Transact-SQL-&#41;](./spatial-data-sys-dm-db-objects-disabled-on-compatibility-level-change.md)

        [Azure Synapse Analytics und parallele Data Warehouse dynamische Verwaltungs Sichten &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)

        [SQL Server dynamischen Verwaltungs Sichten im Zusammenhang mit dem Betriebs System &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)

        [Stretch Database dynamischen Verwaltungs Sichten &#40;Transact-SQL-&#41;]()

        [Dynamische Verwaltungssichten und Funktionen in Verbindung mit Transaktionen &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/transaction-related-dynamic-management-views-and-functions-transact-sql.md)
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>Weitere Informationen  
 [Erteilen von Server Berechtigungen &#40;Transact-SQL-&#41;](../../t-sql/statements/grant-server-permissions-transact-sql.md)   
 [GRANT (Datenbankberechtigungen) &#40;&#40;Transact-SQL&#41;](../../t-sql/statements/grant-database-permissions-transact-sql.md)   
 [System Sichten &#40;Transact-SQL-&#41;](../../t-sql/language-reference.md)  
  
