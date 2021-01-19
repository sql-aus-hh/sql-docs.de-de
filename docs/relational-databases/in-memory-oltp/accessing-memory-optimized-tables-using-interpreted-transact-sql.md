---
title: Speicheroptimierte Tabellen mit interpretiertem T-SQL
description: Erfahren Sie, wie Sie mithilfe von interpretiertem Transact-SQL (Transact-SQL-Batches oder gespeicherte Prozeduren in SQL Server) auf speicheroptimierte Tabellen zugreifen.
ms.custom: seo-dt-2019
ms.date: 05/31/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 92a44d4d-0e53-4fb0-b890-de264c65c95a
author: MightyPen
ms.author: genemi
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: c88fd07e9af86a3c12561935364514b23bc2ea91
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172282"
---
# <a name="accessing-memory-optimized-tables-using-interpreted-transact-sql"></a>Zugreifen auf speicheroptimierte Tabellen mit interpretiertem Transact-SQL
[!INCLUDE[tsql-appliesto-ss2014-asdb-xxxx-xxx_md](../../includes/tsql-appliesto-ss2014-asdb-xxxx-xxx-md.md)]

 Der Zugriff auf speicheroptimierte Tabellen ist bis auf einige Ausnahmen über beliebige [!INCLUDE[tsql](../../includes/tsql-md.md)] -Abfragen, DML-Vorgänge (SELECT, INSERT, UPDATE oder DELETE), Ad-hoc-Batches sowie über SQL-Module wie gespeicherte Prozeduren, Tabellenwertfunktionen, Trigger und Sichten möglich.  
  
Interpretiertes [!INCLUDE[tsql](../../includes/tsql-md.md)] bezieht sich auf [!INCLUDE[tsql](../../includes/tsql-md.md)] -Batches oder gespeicherte Prozeduren, die keine systemintern kompilierten gespeicherten Prozeduren sind. Der Zugriff auf speicheroptimierte Tabellen mittels interpretiertem [!INCLUDE[tsql](../../includes/tsql-md.md)] wird als Interopzugriff bezeichnet.  

Ab [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)]können Abfragen in interpretiertem [!INCLUDE[tsql](../../includes/tsql-md.md)] speicheroptimierte Tabellen parallel scannen, nicht nur im seriellen Modus.

Auf speicheroptimierte Tabellen kann auch mithilfe einer systemintern kompilierten gespeicherten Prozedur zugegriffen werden. Für leistungskritische OLTP-Vorgänge werden systemintern kompilierte gespeicherte Prozeduren empfohlen.  
  
Interpretierter [!INCLUDE[tsql](../../includes/tsql-md.md)] -Zugriff wird für die folgenden Szenarien empfohlen:  
  
- Ad-hoc-Abfragen und Verwaltungsaufgaben  
  
- Berichtsabfragen, die in der Regel Konstrukte verwenden, die in nativ kompilierten gespeicherten Prozeduren nicht verfügbar sind (z.B. *window* -Funktionen, die auch als [OVER](../../t-sql/queries/select-over-clause-transact-sql.md) -Funktionen bezeichnet werden).  
  
- Zum Migrieren leistungskritischer Teile der Anwendung zu speicheroptimierten Tabellen mit minimalen (oder ohne) Änderungen am Anwendungscode. Durch das Migrieren von Tabellen können u. U. Leistungsverbesserungen erzielt werden. Durch die anschließende Migration gespeicherter Prozeduren zu systemintern kompilierten gespeicherten Prozeduren lässt sich die Leistung möglicherweise weiter optimieren.  
  
- Wenn keine [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisung für systemintern kompilierte gespeicherte Prozeduren verfügbar ist.  
  
Die folgenden [!INCLUDE[tsql](../../includes/tsql-md.md)] -Konstrukte werden in gespeicherten Prozeduren mit interpretiertem [!INCLUDE[tsql](../../includes/tsql-md.md)] , die auf Daten in einer speicheroptimierten Tabelle zugreifen, jedoch nicht unterstützt.  
  
|Bereich|Nicht unterstützt|  
|----------|-----------------|  
|Zugriff auf Tabellen|TRUNCATE TABLE<br /><br /> MERGE (speicheroptimierte Tabelle als Ziel)<br /><br /> Dynamische und KEYSET-Cursor (diese werden automatisch auf "statisch" herabgestuft).<br /><br /> Zugriff aus CLR-Modulen mithilfe der Kontextverbindung.<br /><br /> Das Verweisen auf eine speicheroptimierte Tabelle aus einer indizierten Sicht.|  
|Datenbankübergreifend|Datenbankübergreifende Abfragen<br /><br /> Datenbankübergreifende Transaktionen<br /><br /> Verbindungsserver|  
  
## <a name="table-hints"></a>Tabellenhinweise

Weitere Informationen zu Tabellenhinweisen finden Sie unter [Tabellenhinweise &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-table.md). Zur Unterstützung von [!INCLUDE[hek_2](../../includes/hek-2-md.md)]wurde SNAPSHOT hinzugefügt.  
  
Die folgenden Tabellenhinweise werden nicht unterstützt, wenn mit interpretiertem [!INCLUDE[tsql](../../includes/tsql-md.md)]auf eine speicheroptimierte Tabelle zugegriffen wird.  

:::row:::
    :::column:::
        HOLDLOCK

        PAGLOCK

        READUNCOMMITTED

        TABLOCKXX
    :::column-end:::
    :::column:::
        IGNORE_CONSTRAINTS

        READCOMMITTED

        ROWLOCK

        UPDLOCK
    :::column-end:::
    :::column:::
        IGNORE_TRIGGERS

        READCOMMITTEDLOCK

        SPATIAL_WINDOW_MAX_CELLS = *integer*

        XLOCK
    :::column-end:::
    :::column:::
        NOWAIT

        READPAST

        TABLOCK
    :::column-end:::
:::row-end:::

Wenn der Zugriff auf eine speicheroptimierte Tabelle von einer expliziten oder impliziten Transaktion mittels interpretiertem [!INCLUDE[tsql](../../includes/tsql-md.md)]erfolgt, müssen Sie mindestens einen der folgenden Schritte ausführen:  
  
- Geben Sie einen [Tabellenhinweis auf die Isolationsstufe](../../relational-databases/in-memory-oltp/transactions-with-memory-optimized-tables.md) ein, z.B. SNAPSHOT, REPEATABLEREAD oder SERIALIZABLE.  
  
- Legt Sie die Datenbankoption [MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT](../../t-sql/statements/alter-database-transact-sql-set-options.md) auf ON fest.  
  
Ein Tabellenhinweis auf die Isolationsstufe ist bei speicheroptimierten Tabellen, auf die der Zugriff mit Abfragen im [Autocommitmodus](../../odbc/reference/develop-app/auto-commit-mode.md)erfolgt, nicht erforderlich.  
  
## <a name="see-also"></a>Weitere Informationen

[Transact-SQL-Unterstützung für OLTP im Arbeitsspeicher](../../relational-databases/in-memory-oltp/transact-sql-support-for-in-memory-oltp.md)   

[Migrieren zu In-Memory OLTP](./plan-your-adoption-of-in-memory-oltp-features-in-sql-server.md)