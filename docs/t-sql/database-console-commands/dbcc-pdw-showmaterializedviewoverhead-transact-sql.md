---
description: DBCC PDW_SHOWMATERIALIZEDVIEWOVERHEAD (Transact-SQL)
title: DBCC PDW_SHOWMATERIALIZEDVIEWOVERHEAD (Transact-SQL)
ms.custom: seo-dt-2019
ms.date: 07/03/2019
ms.prod: sql
ms.technology: data-warehouse
ms.prod_service: sql-data-warehouse, pdw
ms.reviewer: ''
ms.topic: language-reference
dev_langs:
- TSQL
author: XiaoyuMSFT
ms.author: xiaoyul
monikerRange: = azure-sqldw-latest
ms.openlocfilehash: 87f02544512f4cd33385242101204e3d30c81ffe
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643938"
---
# <a name="dbcc-pdw_showmaterializedviewoverhead-transact-sql"></a>DBCC PDW_SHOWMATERIALIZEDVIEWOVERHEAD (Transact-SQL)  

[!INCLUDE [asa](../../includes/applies-to-version/asa.md)]

Zeigt die Anzahl inkrementeller Änderungen in den Basistabellen an, die für die materialisierten Sichten in [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] gespeichert werden. Das Overheadverhältnis wird als TOTAL_ROWS / MAX (1, BASE_VIEW_ROWS) berechnet.

![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen &#40;Transact-SQL&#41;](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntax

```syntaxsql
DBCC PDW_SHOWMATERIALIZEDVIEWOVERHEAD ( " [ schema_name .] materialized_view_name  " )
[;]
```

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

## <a name="arguments"></a>Argumente

 *schema_name*     
 Ist der Name des Schemas, zu dem die Sicht gehört.

*materialized_view_name*   
Der Name der materialisierten Sicht.

## <a name="remarks"></a>Hinweise

Die Data Warehouse-Engine fügt zu jeder betroffenen Sicht Nachverfolgungszeilen zur Darstellung der Änderungen hinzu, damit materialisierte Sichten stets mit Datenänderungen in Basistabellen aktualisiert werden. Die Auswahl einer materialisierten Sicht schließt das Überprüfen des gruppierten Columnstore-Index der Sicht und das Anwenden inkrementeller Änderungen ein.Die Nachverfolgungszeilen (TOTAL_ROWS - BASE_VIEW_ROWS) werden erst gelöscht, wenn Benutzer die materialisierte Sicht neu erstellen (REBUILD).  

Das Overheadverhältnis wird als TOTAL_ROWS / MAX (1, BASE_VIEW_ROWS) berechnet.  Wenn der Wert hoch ist, wird die SELECT-Leistung beeinträchtigt.Benutzer können die materialisierte Sicht neu erstellen, um das Overheadverhältnis zu verringern.

## <a name="permissions"></a>Berechtigungen  
  
Erfordert die VIEW DATABASE STATE-Berechtigung.  

## <a name="examples"></a>Beispiele  

### <a name="a-this-example-returns-the-overhead-ratio-of-a-materialized-view"></a>A. Dieses Beispiel gibt das Overheadverhältnis einer materialisierten Sicht zurück.

```sql
DBCC PDW_SHOWMATERIALIZEDVIEWOVERHEAD ( "dbo.MyIndexedView" )
```

Ausgabe:

|OBJECT_ID|BASE_VIEW_ROWS|TOTAL_ROWS|OVERHEAD_RATIO|
|--------|--------|--------|--------|  
|1234|1|3 |3.0 |

</br>

### <a name="b-this-example-shows-how-the-materialized-view-overhead-increases-as-data-changes-in-base-tables"></a>B. Dieses Beispiel zeigt, wie sich der Overhead der materialisierten Sicht vergrößert, wenn sich Daten in Basistabellen ändern.

Erstellen einer Tabelle
```sql
CREATE TABLE t1 (c1 INT NOT NULL, c2 INT NOT NULL, c3 INT NOT NULL)
```
Einfügen von fünf Zeilen in t1
```sql
INSERT INTO t1 VALUES (1, 1, 1)
INSERT INTO t1 VALUES (2, 2, 2) 
INSERT INTO t1 VALUES (3, 3, 3) 
INSERT INTO t1 VALUES (4, 4, 4) 
INSERT INTO t1 VALUES (5, 5, 5) 
```
Erstellen materialisierter Sichten (MV1)
```sql
CREATE MATERIALIZED VIEW MV1 
WITH (DISTRIBUTION = HASH(c1))  
AS
SELECT c1, COUNT(*) total_number 
FROM dbo.t1 WHERE c1 < 3
GROUP BY c1  
```
Bei der Auswahl aus der materialisierten Sicht werden zwei Zeilen zurückgegeben.

|c1|total_number|
|--------|--------| 
|1|1| 
|2|1|

Überprüfen Sie vor der Änderung von Daten in der Basistabelle den Overhead der materialisierten Sicht.
```sql
DBCC PDW_SHOWMATERIALIZEDVIEWOVERHEAD ("dbo.mv1")
```
Ausgabe:

|OBJECT_ID|BASE_VIEW_ROWS|TOTAL_ROWS|OVERHEAD_RATIO|
|--------|--------|--------|--------|  
|587149137|2|2 |1,00000000000000000 |

Aktualisieren Sie die Basistabelle.  Diese Abfrage aktualisiert dieselbe Spalte in derselben Zeile 100 mal mit demselben Wert.  Der Inhalt der materialisierten Sicht ändert sich nicht.
```sql
DECLARE @p INT
SELECT @p = 1
WHILE (@p < 101)
BEGIN
UPDATE t1 SET c1 = 1 WHERE c1 = 1
SELECT @p = @p+1
END  
```

Bei der Auswahl aus der materialisierten Sicht wird dasselbe Ergebnis wie zuvor zurückgegeben.  

|c1|total_number|
|--------|--------| 
|1|1| 
|2|1|

Nachstehend finden Sie die Ausgabe von DBCC PDW_SHOWMATERIALIZEDVIEWOVERHEAD ("dbo.mv1").  Der materialisierten Sicht werden 100 Zeilen hinzugefügt (total_row - base_view_rows), und das Overheadverhältnis (overhead_ratio) wird erhöht. 

|OBJECT_ID|BASE_VIEW_ROWS|TOTAL_ROWS|OVERHEAD_RATIO|
|--------|--------|--------|--------|  
|587149137|2|102 |51,00000000000000000 |

Nachdem die materialisierte Sicht neu erstellt wurde, werden alle Nachverfolgungszeilen für Änderungen inkrementeller Daten gelöscht, und das Overheadverhältnis der Sicht wird verringert.  

```sql
ALTER MATERIALIZED VIEW dbo.MV1 REBUILD
GO
DBCC PDW_SHOWMATERIALIZEDVIEWOVERHEAD ("dbo.mv1")
```
Output

|OBJECT_ID|BASE_VIEW_ROWS|TOTAL_ROWS|OVERHEAD_RATIO|
|--------|--------|--------|--------|  
|587149137|2|2 |1,00000000000000000 |

## <a name="see-also"></a>Weitere Informationen

[Leistungsoptimierung durch materialisierte Sicht](/azure/sql-data-warehouse/performance-tuning-materialized-views)   
[CREATE MATERIALIZED VIEW AS SELECT &#40;Transact-SQL&#41;](../statements/create-materialized-view-as-select-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)   
[ALTER MATERIALIZED VIEW &#40;Transact-SQL&#41;](../statements/alter-materialized-view-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)   
[EXPLAIN &#40;Transact-SQL&#41;](../queries/explain-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)   
[sys.pdw_materialized_view_column_distribution_properties &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-pdw-materialized-view-column-distribution-properties-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)   
[sys.pdw_materialized_view_distribution_properties &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-pdw-materialized-view-distribution-properties-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)   
[sys.pdw_materialized_view_mappings &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-pdw-materialized-view-mappings-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)   
[Katalogsichten von Azure Synapse Analytics und Parallel Data Warehouse Catalog](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)   
[In Azure Synapse Analytics unterstützte Systemsichten](/azure/sql-data-warehouse/sql-data-warehouse-reference-tsql-system-views)   
[In Azure Synapse Analytics unterstützte T-SQL-Anweisungen](/azure/sql-data-warehouse/sql-data-warehouse-reference-tsql-statements)