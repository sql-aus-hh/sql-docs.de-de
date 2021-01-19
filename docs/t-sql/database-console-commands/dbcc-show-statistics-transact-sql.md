---
description: DBCC SHOW_STATISTICS (Transact-SQL)
title: DBCC SHOW_STATISTICS (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 12/18/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- SHOW_STATISTICS_TSQL
- DBCC SHOW_STATISTICS
- SHOW_STATISTICS
- DBCC_SHOW_STATISTICS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- query optimization statistics [SQL Server], densities
- histograms [SQL Server]
- statistical information [SQL Server], viewing statistics objects
- current distribution statistics
- query optimization statistics [SQL Server], histograms
- DBCC SHOW_STATISTICS statement
- distribution statistics
- statistical information [SQL Server], densities
- statistics objects
- statistical information [SQL Server], histograms
- query optimization statistics [SQL Server], viewing statistics objects
- statistical information [SQL Server], distribution
- densities [SQL Server]
- displaying distribution statistics
ms.assetid: 12be2923-7289-4150-b497-f17e76a50b2e
author: pmasl
ms.author: umajay
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 710a6211b588ab0dd4745b41af1f330c81241294
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170502"
---
# <a name="dbcc-show_statistics-transact-sql"></a>DBCC SHOW_STATISTICS (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

DBCC SHOW_STATISTICS zeigt die aktuelle Abfrageoptimierungsstatistik für eine Tabelle oder eine indizierte Sicht an. Der Abfrageoptimierer verwendet Statistiken zur Schätzung der Kardinalität oder Zeilenanzahl im Abfrageergebnis und ermöglicht somit dem Abfrageoptimierer, einen Abfrageplan von hoher Qualität zu erstellen. Beispielsweise kann der Abfrageoptimierer Kardinalitätsschätzungen verwenden, um im Abfrageplan statt des Index Scan-Operators den Index Seek-Operator auszuwählen und so die Abfrageleistung zu verbessern, indem ein ressourcenintensiver Indexscan vermieden wird.
  
Der Abfrageoptimierer speichert die Statistiken für eine Tabelle oder indizierte Sicht in einem Statistikobjekt. Für eine Tabelle wird das Statistikobjekt entweder für einen Index oder eine Liste mit Tabellenspalten erstellt. Das Statistikobjekt enthält einen Header mit Metadaten über die Statistik, ein Histogramm mit der Verteilung der Werte in der ersten Schlüsselspalte des Statistikobjekts sowie einen Dichtevektor zum Messen der Korrelation zwischen Spalten. [!INCLUDE[ssDE](../../includes/ssde-md.md)] kann Kardinalitätsschätzungen mit beliebigen Daten des Statistikobjekts berechnen. Weitere Informationen finden Sie unter [Statistik](../../relational-databases/statistics/statistics.md) und [Kardinalitätsschätzung (SQL Server)](../../relational-databases/performance/cardinality-estimation-sql-server.md).
  
DBCC SHOW_STATISTICS zeigt den Header, das Histogramm und den Dichtevektor auf der Grundlage von Daten an, die im Statistikobjekt gespeichert sind. Die Syntax ermöglicht es Ihnen, eine Tabelle oder indizierte Sicht zusammen mit einem Zielindexnamen, Statistiknamen oder Spaltennamen anzugeben. In diesem Thema wird beschrieben, wie die Statistik angezeigt und die angezeigten Ergebnisse interpretiert werden.

> [!IMPORTANT]
> Ab [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP1 steht die dynamische Verwaltungssicht [sys.dm_db_stats_properties](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-properties-transact-sql.md) zur Verfügung, um die im Statistikobjekt enthaltenen Headerinformationen für nicht inkrementelle Statistiken programmgesteuert abzurufen.

> [!IMPORTANT]
> Ab [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 und [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP1 steht die dynamische Verwaltungssicht [sys.dm_db_incremental_stats_properties](../../relational-databases/system-dynamic-management-views/sys-dm-db-incremental-stats-properties-transact-sql.md) zur Verfügung, um die im Statistikobjekt enthaltenen Headerinformationen für inkrementelle Statistiken programmgesteuert abzurufen.

> [!IMPORTANT]
> Ab [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP1 CU2 steht die dynamische Verwaltungssicht [sys.dm_db_stats_histogram](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-histogram-transact-sql.md) zur Verfügung, um Histogramminformationen, die im Statistikobjekt enthalten sind, programmgesteuert abzurufen.

![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntax

```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
DBCC SHOW_STATISTICS ( table_or_indexed_view_name , target )   
[ WITH [ NO_INFOMSGS ] < option > [ , n ] ]  
< option > :: =  
    STAT_HEADER | DENSITY_VECTOR | HISTOGRAM | STATS_STREAM  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  

DBCC SHOW_STATISTICS ( table_name , target )   
    [ WITH { STAT_HEADER | DENSITY_VECTOR | HISTOGRAM } [ ,...n ] ]  
[;]
```

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente

 *table_or_indexed_view_name*  
 Name der Tabelle oder der indizierten Sicht, für die statistische Informationen angezeigt werden sollen.  
  
 *table_name*  
 Der Name der Tabelle, die die anzuzeigenden Statistiken enthält. Die Tabelle kann keine externe Tabelle sein.  
  
 *Ziel*  
 Der Name des Indexes, der Statistik oder der Spalte, für die Statistikinformationen angezeigt werden sollen. *target* wird in Klammern, einzelnen Anführungszeichen oder doppelten Anführungszeichen gesetzt, bzw. es werden keine Anführungszeichen verwendet. Wenn *target* ein Name eines vorhandenen Indexes oder einer vorhandenen Statistik für eine Tabelle oder eine indizierte Sicht ist, werden die Statistikinformationen zu diesem Ziel zurückgegeben. Wenn *target* der Name einer vorhandenen Spalte ist und eine automatisch erstellte Statistik für diese Spalte vorhanden ist, werden Informationen zu dieser automatisch erstellten Statistik zurückgegeben. Wenn keine automatisch erstellte Statistik für ein Spaltenziel vorhanden ist, wird die Fehlermeldung 2767 zurückgegeben.  
 *target* kann in [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] kein Spaltenname sein.  
  
 NO_INFOMSGS  
 Unterdrückt alle Informationsmeldungen mit einem Schweregrad von 0 bis 10.  
  
 STAT_HEADER \| DENSITY_VECTOR \| HISTOGRAM \| STATS_STREAM [ **,** _n_]: Wenn mindestens eine dieser Optionen angegeben wird, schränkt dies die Resultsets ein, die von der Anweisung an die angegebene Option oder die angegebenen Optionen zurückgegeben werden. Wenn keine Optionen angegeben sind, werden alle Statistikinformationen zurückgegeben.  

 STATS_STREAM entspricht [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)].  
  
## <a name="result-sets"></a>Resultsets

In der folgenden Tabelle werden die Spalten beschrieben, die im Resultset zurückgegeben werden, wenn STAT_HEADER angegeben wird.
  
|Spaltenname|BESCHREIBUNG|  
|-----------------|-----------------|  
|Name|Name des Statistikobjekts.|  
|Aktualisiert|Datum und Uhrzeit des letzten Updates der Statistik. Die Funktion [STATS_DATE](../../t-sql/functions/stats-date-transact-sql.md) ist eine alternative Möglichkeit zum Abrufen dieser Informationen. Weitere Informationen finden Sie im Abschnitt [Hinweise](#Remarks) dieses Artikels.|  
|Zeilen|Gesamtanzahl der Zeilen in der Tabelle oder indizierten Sicht zum Zeitpunkt des letzten Updates der Statistik. Wenn die Statistik gefiltert wird oder einem gefilterten Index entspricht, kann die Anzahl der Zeilen geringer als die Anzahl der Zeilen in der Tabelle sein. Weitere Informationen finden Sie unter [Statistiken](../../relational-databases/statistics/statistics.md).|  
|Rows Sampled|Gesamtzahl der Zeilen, die für die statistischen Berechnungen in die Stichprobe aufgenommen wurden. Wenn Rows Sampled < Rows, sind das angezeigte Histogramm und die Dichteergebnisse Schätzungen auf Grundlage der als Stichprobe entnommenen Zeilen.|  
|Schritte|Anzahl der Schritte im Histogramm. Jeder Schritt umfasst einen Bereich von Spaltenwerten gefolgt von einem oberen Spaltengrenzwert. Die Histogrammschritte werden in der Statistik in der ersten Schlüsselspalte definiert. Die maximale Anzahl von Schritten ist 200.|  
|Dichte|Berechnet als 1 / *verschiedene Werte* für alle Werte in der ersten Schlüsselspalte des Statistikobjekts mit Ausnahme der Begrenzungswerte des Histogramms. Dieser Dichtewert wird vom Abfrageoptimierer nicht verwendet und für die Abwärtskompatibilität mit Versionen vor [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] angezeigt.|  
|Average Key Length|Durchschnittliche Anzahl von Bytes pro Wert für alle Schlüsselspalten im Statistikobjekt.|  
|String Index|"Ja" gibt an, dass das Statistikobjekt Statistiken über Zusammenfassungen von Zeichenfolgen enthält, um die Kardinalitätsschätzungen für Abfrageprädikate, die den LIKE-Operator verwenden, zu verbessern, z. B. `WHERE ProductName LIKE '%Bike'`. Statistiken über Zusammenfassungen von Zeichenfolgen werden getrennt vom Histogramm gespeichert und in der ersten Schlüsselspalte des Statistikobjekts erstellt, wenn dieses vom Typ **char**, **varchar**, **nchar**, **nvarchar**, **varchar(max)** , **nvarchar(max)** , **text** oder **ntext** ist.|  
|Filterausdruck|Prädikat für die Teilmenge von Tabellenzeilen, die im Statistikobjekt enthalten sind. NULL = Nicht gefilterte Statistik. Weitere Informationen zu gefilterten Prädikaten finden Sie unter [Erstellen gefilterter Indizes](../../relational-databases/indexes/create-filtered-indexes.md). Weitere Informationen zu gefilterten Statistiken finden Sie unter [Statistiken](../../relational-databases/statistics/statistics.md).|  
|Unfiltered Rows|Gesamtzahl von Zeilen in der Tabelle vor dem Anwenden des Filterausdrucks. Wenn Filter Expression NULL ist, ist Unfiltered Rows gleich Rows.|  
|Persistierter Beispielprozentwert|Der persistierte Prozentwert für die Stichprobe wird für Aktualisierungen von Statistiken verwendet, die keinen expliziten Prozentwert für die Stichprobenentnahme angibt. Wenn der Wert 0 (null) ist, wird kein persistierter Prozentwert für diese Statistik festgelegt.<br /><br /> **Anwendungsbereich:** [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP1 CU4| 
  
In der folgenden Tabelle werden die Spalten beschrieben, die beim Angeben von DENSITY_VECTOR im Resultset zurückgegeben werden.
  
|Spaltenname|BESCHREIBUNG|  
|-----------------|-----------------|  
|All Density|Die Dichte ist 1 / *verschiedene Werte*. Die Ergebnisse zeigen die Dichte für jedes Präfix von Spalten im Statistikobjekt mit einer Zeile pro Dichte an. Bei einem unterschiedlichen Wert handelt es sich um eine unterschiedliche Liste der Spaltenwerte pro Zeile und pro Spaltenpräfix. Wenn das Statistikobjekt beispielsweise Schlüsselspalten (A, B, C) enthält, geben die Ergebnisse die Dichte der unterschiedlichen Wertelisten jedes dieser Spaltenpräfixe an: (A), (A,B) und (A, B, C). Mit dem Präfix (A, B, C) ist jede dieser Listen eine Liste unterschiedlicher Werte: (3, 5, 6), (4, 4, 6), (4, 5, 6), (4, 5, 7). Mit dem Präfix (A, B) weisen dieselben Spaltenwerte diese unterschiedlichen Wertelisten auf: (3, 5), (4, 4) und (4, 5)|  
|Average Length|Durchschnittliche Länge in Bytes zum Speichern einer Liste der Spaltenwerte für das Spaltenpräfix. Wenn die Werte in der Liste (3, 5, 6) beispielsweise jeweils 4 Bytes erfordern, beträgt die Länge 12 Bytes.|  
|Spalten|Namen der Spalten im Präfix, für die All Density und Average Length angezeigt werden.|  
  
Die folgende Tabelle beschreibt die Spalten, die im Resultset zurückgegeben werden, wenn die HISTOGRAM-Option angegeben wird.
  
|Spaltenname|BESCHREIBUNG|  
|---|---|
|RANGE_HI_KEY|Oberer Spaltengrenzwert für einen Histogrammschritt. Der Spaltenwert wird auch als Schlüsselwert bezeichnet.|  
|RANGE_ROWS|Geschätzte Anzahl von Zeilen, deren Spaltenwerte innerhalb eines Histogrammschritts liegen, ohne den oberen Grenzwert.|  
|EQ_ROWS|Geschätzte Anzahl von Zeilen, deren Spaltenwerte der Obergrenze des Histogrammschritts entsprechen.|  
|DISTINCT_RANGE_ROWS|Geschätzte Anzahl von Zeilen mit einem unterschiedlichen Spaltenwert innerhalb eines Histogrammschritts ohne den oberen Grenzwert.|  
|AVG_RANGE_ROWS|Durchschnittliche Anzahl von Zeilen mit einem duplizierten Spaltenwert innerhalb eines Histogrammschritts ohne den oberen Grenzwert. Wenn DISTINCT_RANGE_ROWS größer als 0 ist, wird AVG_RANGE_ROWS berechnet, indem RANGE_ROWS durch DISTINCT_RANGE_ROWS dividiert wird. Wenn DISTINCT_RANGE_ROWS 0 ist, gibt AVG_RANGE_ROWS 1 für den Histogrammschritt zurück.| 
  
## <a name="remarks"></a><a name="Remarks"></a> Hinweise 

Das Aktualisierungsdatum für die Statistiken befindet sich gemeinsam mit dem [Histogramm](#histogram) und [Dichtevektor](#density) nicht in den Metadaten, sondern im [Statistik-Blobobjekt](../../relational-databases/statistics/statistics.md#DefinitionQOStatistics). Wenn für das Generieren von Statistikdaten keine Daten gelesen werden, wird das Statistik-Blob nicht erstellt, das Datum nicht verfügbar und die Spalte *aktualisiert* ist NULL. Dies ist der Fall bei gefilterten Statistiken oder neuen und leeren Tabellen, für die das Prädikat keine Zeilen zurückgibt.
  
## <a name="histogram"></a><a name="histogram"></a> Histogramm

Ein Histogramm misst die Häufigkeit des Vorkommens für jeden unterschiedlichen Wert in einem Dataset. Der Abfrageoptimierer berechnet ein Histogramm für die Spaltenwerte in der ersten Schlüsselspalte des Statistikobjekts und wählt die Spaltenwerte aus, indem statistische Zeilenstichproben entnommen werden oder indem ein vollständiger Scan aller Zeilen in der Tabelle oder Sicht ausgeführt wird. Wenn das Histogramm anhand einer Gruppe von Zeilenstichproben erstellt wird, handelt es sich bei der gespeicherten Gesamtzahl von Zeilen und unterschiedlichen Werten um Schätzungen, die keine ganzen Zahlen sein müssen.
  
Zum Erstellen des Histogramms sortiert der Abfrageoptimierer die Spaltenwerte, berechnet die Anzahl der Werte, die den einzelnen unterschiedlichen Spaltenwerten entsprechen, und aggregiert die Spaltenwerte dann in maximal 200 zusammenhängenden Histogrammschritten. Jeder Schritt enthält einen Bereich von Spaltenwerten gefolgt von einem oberen Spaltengrenzwert. Der Bereich enthält alle möglichen Spaltenwerte zwischen den Begrenzungswerten, ohne die Begrenzungswerte selbst. Der niedrigste der sortierten Spaltenwerte ist der obere Grenzwert für den ersten Histogrammschritt.
  
Das folgende Diagramm zeigt ein Histogramm mit sechs Schritten. Der Bereich links vom ersten oberen Grenzwert ist der erste Schritt.
  
![Histogramm](../../relational-databases/system-dynamic-management-views/media/a0ce6714-01f4-4943-a083-8cbd2d6f617a.gif "a0ce6714-01f4-4943-a083-8cbd2d6f617a")
  
Für jeden Histogrammschritt gilt:
-   Eine fett formatierte Zeile stellt den oberen Grenzwert (RANGE_HI_KEY) und die Häufigkeit des Vorkommens dar (EQ_ROWS).  
-   Der einfarbige Bereich links von RANGE_HI_KEY stellt den Bereich der Spaltenwerte und die durchschnittliche Häufigkeit des Vorkommens der einzelnen Spaltenwerte (AVG_RANGE_ROWS) dar. AVG_RANGE_ROWS ist für den ersten Histogrammschritt immer 0.  
-   Gepunktete Linien stellen die als Stichprobe entnommenen Werte dar, die zum Schätzen der Gesamtanzahl der unterschiedlichen Werte im Bereich (DISTINCT_RANGE_ROWS) verwendet werden, sowie die Gesamtanzahl der Werte im Bereich (RANGE_ROWS). Der Abfrageoptimierer verwendet RANGE_ROWS und DISTINCT_RANGE_ROWS, um AVG_RANGE_ROWS zu berechnen. Die als Stichprobe entnommenen Werte werden nicht gespeichert.  
  
Der Abfrageoptimierer definiert die Histogrammschritte gemäß ihrer statistischen Bedeutung. Dabei wird ein Algorithmus für die maximale Differenz verwendet, um die Anzahl der Schritte im Histogramm zu minimieren und gleichzeitig die Differenz zwischen den Begrenzungswerten zu maximieren. Die maximale Anzahl von Schritten ist 200. Die Anzahl von Histogrammschritten kann geringer sein als die Anzahl unterschiedlicher Werte, auch bei Spalten mit weniger als 200 Grenzpunkten. Beispielsweise kann eine Spalte mit 100 unterschiedlichen Werten ein Histogramm mit weniger als 100 Grenzpunkten aufweisen.
  
## <a name="density-vector"></a><a name="density"></a> Dichtevektor  
Der Abfrageoptimierer verwendet Dichten, um Kardinalitätsschätzungen für Abfragen zu erweitern, die mehrere Spalten aus derselben Tabelle oder indizierten Sicht zurückgeben. Der Dichtevektor enthält eine Dichte für jedes Präfix von Spalten im Statistikobjekt. Wenn ein Statistikobjekt beispielsweise die Schlüsselspalten `CustomerId`, `ItemId` und `Price` enthält, wird die Dichte für jedes der folgenden Spaltenpräfixe berechnet:
  
|Spaltenpräfix|Dichte berechnet für|  
|---|---|
|(CustomerId)|Zeilen mit übereinstimmenden Werten für CustomerId|  
|(CustomerId, ItemId)|Zeilen mit übereinstimmenden Werten für CustomerId und ItemId|  
|(CustomerId, ItemId, Price)|Zeilen mit übereinstimmenden Werten für CustomerId, ItemId und Price|  
  
## <a name="restrictions"></a>Beschränkungen  
 DBCC SHOW_STATISTICS stellt keine Statistik für räumliche Indizes oder speicheroptimierte columnstore-Indizes bereit.  
  
## <a name="permissions-for-ssnoversion-and-sssds"></a>Berechtigungen für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und [!INCLUDE[ssSDS](../../includes/sssds-md.md)]  
Zum Anzeigen des Statistikobjekts muss der Benutzer über die `SELECT`-Berechtigung für die Tabelle verfügen.
Die folgenden Voraussetzungen müssen erfüllt sein, damit der Befehl erfolgreich mit SELECT-Berechtigung ausgeführt werden kann:
-   Die Benutzer benötigen eine Zugriffsberechtigung für alle Spalten im Statistikobjekt.  
-   Die Benutzer benötigen eine Zugriffsberechtigung für alle Spalten in einer Filterbedingung (falls vorhanden).  
-   Die Tabelle kann keine Sicherheitsrichtlinie auf Zeilenebene haben.
-   Wenn eine der Spalten innerhalb eines Statistikobjekts mit dynamischen Datenmaskierungsregeln maskiert ist, muss der Benutzer neben der `SELECT`-Berechtigung über die `UNMASK`-Berechtigung verfügen.

In Versionen vor [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP1 muss der Benutzer die Tabelle besitzen oder Mitglied der festen Serverrollen `sysadmin`, `db_owner` oder `db_ddladmin` sein.

 > [!NOTE]
 > Verwenden Sie das Ablaufverfolgungsflag 9485, um das Verhalten in das Verhalten vor [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP1 zu ändern.
  
## <a name="permissions-for-sssdw-and-sspdw"></a>Berechtigungen für [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
DBCC SHOW_STATISTICS erfordert die `SELECT`-Berechtigung für die Tabelle oder Mitgliedschaft in der festen Serverrolle `sysadmin`, der festen Datenbankrolle `db_owner` oder der festen Datenbankrolle `db_ddladmin`.  
  
## <a name="limitations-and-restrictions-for-sssdw-and-sspdw"></a>Einschränkungen für [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
DBCC SHOW_STATISTICS zeigt Statistiken an, die in der Shell-Datenbank auf der Ebene des Steuerelements gespeichert sind. Statistiken die automatisch von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] auf den Computeknoten erstellt wurden, werden nicht angezeigt.
  
DBCC SHOW_STATISTICS wird nicht auf externen Tabellen unterstützt.
  
## <a name="examples-ssnoversion-and-sssds"></a>Beispiele: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und [!INCLUDE[ssSDS](../../includes/sssds-md.md)]  
### <a name="a-returning-all-statistics-information"></a>A. Zurückgeben aller Statistikinformationen  
Im folgenden Beispiel werden alle Statistikinformationen für den Index `AK_Address_rowguid` der Tabelle `Person.Address` in der Datenbank [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] angezeigt.
  
```sql
DBCC SHOW_STATISTICS ("Person.Address", AK_Address_rowguid);  
GO  
```  
  
### <a name="b-specifying-the-histogram-option"></a>B. Angeben der HISTOGRAM-Option  
Dies beschränkt die für Customer_LastName angezeigten Statistikinformationen auf die HISTOGRAM-Daten.
  
```sql
DBCC SHOW_STATISTICS ("dbo.DimCustomer",Customer_LastName) WITH HISTOGRAM;  
GO  
```  
  
## <a name="examples-sssdw-and-sspdw"></a>Beispiele: [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
### <a name="c-display-the-contents-of-one-statistics-object"></a>C. Anzeigen der Inhalte eine Statistikobjekts  
 Im folgenden Beispiel werden die Inhalte der Customer_LastName-Statistiken in der Tabelle „DimCustomer“ angezeigt.  
  
```sql
-- Uses AdventureWorks  
--First, create a statistics object  
CREATE STATISTICS Customer_LastName   
ON AdventureWorksPDW2012.dbo.DimCustomer (LastName);  
GO  
DBCC SHOW_STATISTICS ("dbo.DimCustomer",Customer_LastName);  
GO  
```  
  
Die Ergebnisse zeigen den Header, den Dichtevektor und einen Teil des Histogramms an.
  
![Ergebnisse zu DBCC SHOW_STATISTICS](../../t-sql/database-console-commands/media/aps-sql-dbccshow-statistics.JPG "Ergebnisse zu DBCC SHOW_STATISTICS")
  
## <a name="see-also"></a>Weitere Informationen  
[Statistik](../../relational-databases/statistics/statistics.md)  
[CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)  
[CREATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/create-statistics-transact-sql.md)  
[DROP STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/drop-statistics-transact-sql.md)  
[sp_autostats &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-autostats-transact-sql.md)  
[sp_createstats &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-createstats-transact-sql.md)  
[STATS_DATE &#40;Transact-SQL&#41;](../../t-sql/functions/stats-date-transact-sql.md)  
[UPDATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/update-statistics-transact-sql.md)  
[sys.dm_db_stats_properties (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-properties-transact-sql.md)  
[sys.dm_db_stats_histogram (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-histogram-transact-sql.md)   
[sys.dm_db_incremental_stats_properties (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-incremental-stats-properties-transact-sql.md)   
