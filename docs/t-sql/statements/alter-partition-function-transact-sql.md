---
description: ALTER PARTITION FUNCTION (Transact-SQL)
title: ALTER PARTITION FUNCTION (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ALTER PARTITION FUNCTION
- ALTER_PARTITION_FUNCTION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- splitting partitions [SQL Server]
- partitioned tables [SQL Server], splitting
- ALTER PARTITION FUNCTION statement
- merging partitions [SQL Server]
- partitioned indexes [SQL Server], merging
- partitioned indexes [SQL Server], splitting
- modifying partition functions
- partition functions [SQL Server], modifying
- partitioned tables [SQL Server], merging
ms.assetid: 70866dac-0a8f-4235-8108-51547949ada4
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: c3ae0c860bc31a228ce3a842d7e1ba8a861ea1e5
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092109"
---
# <a name="alter-partition-function-transact-sql"></a>ALTER PARTITION FUNCTION (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Ändert eine Partitionsfunktion durch Teilen oder Zusammenführen der Begrenzungswerte. Durch das Ausführen einer ALTER PARTITION FUNCTION-Anweisung kann eine Tabellenpartition oder ein Index, die/der die Partitionsfunktion verwendet, in zwei Partitionen geteilt werden. Die Anweisung kann auch zwei Partitionen zu einer kleineren Partition zusammenführen.  
  
> [!CAUTION]  
>  Mehrere Tabellen oder Indizes können dieselbe Partitionsfunktion verwenden. ALTER PARTITION FUNCTION wirkt sich auf alle Tabellen und Indizes in einer einzigen Transaktion aus.  
  
![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
  
ALTER PARTITION FUNCTION partition_function_name()  
{   
    SPLIT RANGE ( boundary_value )  
  | MERGE RANGE ( boundary_value )   
} [ ; ]  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
*partition_function_name*  
Der Name der Partitionsfunktion, die geändert werden soll.  
  
SPLIT RANGE ( *boundary_value* )  
Fügt der Partitionsfunktion eine Partition hinzu. *boundary_value* bestimmt den Bereich der neuen Partition und muss von den vorhandenen Begrenzungsbereichen der Partitionsfunktion abweichen. Basierend auf *boundary_value* teilt [!INCLUDE[ssDE](../../includes/ssde-md.md)] einen der vorhandenen Bereiche in zwei Bereiche auf. Von diesen beiden Bereichen ist der mit dem neuen *boundary_value*-Wert die neue Partition.  
  
Eine Dateigruppe muss online vorhanden sein. Außerdem muss das Partitionsschema, das die Partitionsfunktion verwendet, um die neue Partition aufzunehmen, die Dateigruppe als NEXT USED markieren. Eine CREATE PARTITION SCHEME-Anweisung weist Dateigruppen zu Partitionen zu. Die CREATE PARTITION FUNCTION-Anweisung erstellt weniger Partitionen als Dateigruppen zu deren Aufnahme. Eine CREATE PARTITION SCHEME-Anweisung legt möglicherweise mehr Dateigruppen an, als notwendig. In diesem Fall bleiben dann nicht zugewiesene Dateigruppen übrig. Das Partitionsschema markiert außerdem eine der Dateigruppen als NEXT USED. Diese Dateigruppe nimmt die neue Partition auf. Wenn keine Dateigruppen vorhanden sind, die vom Partitionsschema als NEXT USED markiert werden, müssen Sie eine ALTER PARTITION SCHEME-Anweisung verwenden. 

Die ALTER PARTITION SCHEME-Anweisung kann entweder eine Dateigruppe hinzufügen, oder eine vorhandene auswählen, um die neue Partition aufzunehmen. Sie können eine Dateigruppe zuweisen, die bereits Partitionen enthält, um zusätzliche Partitionen aufzunehmen. Eine Partitionsfunktion kann bei mehreren Partitionsschemas verwendet werden. Aus diesem Grund müssen alle Partitionsschemas, die die Partitionsfunktion verwenden, der Sie Partitionen hinzufügen, eine NEXT USED-Dateigruppe aufweisen. Andernfalls schlägt die ALTER PARTITION FUNCTION-Anweisung fehl, und es werden die Partitionsschemas angezeigt, für die eine NEXT USED-Dateigruppe fehlt.  
  
Wenn Sie alle Partitionen in derselben Dateigruppe erstellen, wird diese Dateigruppe anfänglich automatisch der NEXT USED-Dateigrupp zugewiesen. Nach der Ausführung eines Teilungsvorgangs gibt es jedoch keine ausgewählte NEXT USED-Dateigruppe mehr. Weisen Sie die Dateigruppe explizit mithilfe von ALTER PARTITION SCHEME als NEXT USED-Dateigruppe zu. Andernfalls schlägt ein bevorstehender Teilungsvorgang fehl.  
  
> [!NOTE]  
>  Einschränkungen im Zusammenhang mit dem Columnstore-Index: Wenn ein Columnstore-Index für die Tabelle vorhanden ist, können nur leere Partitionen aufgeteilt werden. Vor dem Ausführen dieses Vorgangs müssen Sie den Columnstore-Index löschen oder deaktivieren.  
  
MERGE [ RANGE ( *boundary_value*) ]  
Löscht eine Partition und führt alle in der Partition vorhandenen Werte in der verbleibenden Partition zusammen. RANGE (*boundary_value*) muss ein vorhandener Begrenzungswert der Partition sein, die gelöscht werden soll. Dieses Argument entfernt die Dateigruppe, in der *boundary_value* ursprünglich vorhanden war, aus dem Partitionsschema, wenn sie nicht von einer verbleibenden Partition verwendet wird, oder wird mit der NEXT USED-Eigenschaft markiert. Die zusammengeführte Partition ist in der Dateigruppe vorhanden, die anfänglich *boundary_value* nicht enthielt. *boundary_value* ist ein konstanter Ausdruck, der auf Variablen (einschließlich Variablen eines benutzerdefinierten Typs) oder Funktionen (einschließlich benutzerdefinierter Funktionen) verweisen kann. Er kann nicht auf einen [!INCLUDE[tsql](../../includes/tsql-md.md)]-Ausdruck verweisen. *boundary_value* muss entweder mit dem Datentyp der zugehörigen Partitionierungsspalte übereinstimmen oder implizit in diesen konvertierbar sein. Sie können *boundary_value* auch während der impliziten Konvertierung nicht so abschneiden, dass dessen Größe und Dezimalstellen mit denen des entsprechenden *input_parameter_type*-Werts nicht übereinstimmen.  
  
> [!NOTE]  
>  Einschränkungen für einen columnstore-Index: Zwei nicht leere Partitionen, die einen columnstore-Index enthalten, können nicht zusammengeführt werden. Vor dem Ausführen dieses Vorgangs müssen Sie den Columnstore-Index löschen oder deaktivieren.  
  
## <a name="best-practices"></a>Empfehlungen  
Bewahren Sie immer leere Partitionen an beiden Enden des Partitionsbereichs auf. Erhalten Sie die Partitionen an beiden Enden, um sicherzustellen, dass die Partitionsteilung und die Partitionszusammenführung keine Datenverschiebungen verursachen. Die Partitionsteilung erfolgt am Anfang und die Partitionszusammenführung am Ende. Vermeiden Sie das Aufteilen oder Zusammenführen gefüllter Partitionen. Das Teilen oder Zusammenführen aufgefüllter Partitionen kann ineffizient sein. Diese Vorgänge können ineffizient sein, da durch das Teilen oder Zusammenführen möglicherweise ein viermal größeres Protokoll generiert wird und es zudem zu ernsthaften Sperren kommen kann.  
  
## <a name="limitations-and-restrictions"></a>Einschränkungen  
ALTER PARTITION FUNCTION partitioniert Tabellen und Indizes neu, die die Funktion in einem einzelnen atomaren Vorgang verwenden. Dieser Vorgang erfolgt jedoch offline, und in Abhängigkeit vom Umfang kann die Neupartitionierung ressourcenintensiv sein.  
  
Verwenden Sie ALTER PARTITION FUNCTION nur zum Teilen einer Partition in zwei Partitionen oder zum Zusammenführen von zwei Partitionen zu einer Partition. Um die Partitionierung einer Tabelle anderweitig zu ändern (z. B. von 10 Partitionen in 5 Partitionen), führen Sie eine der folgenden Optionen aus. Der Ressourcenverbrauch dieser Optionen hängt von Ihrer Systemkonfiguration ab:  
  
-   Erstellen Sie eine neue partitionierte Tabelle mit der erforderlichen Partitionsfunktion. Fügen Sie dann mithilfe einer INSERT INTO...SELECT FROM-Anweisung die Daten aus der alten Tabelle in die neue Tabelle ein.  
  
-   Erstellen Sie einen partitionierten gruppierten Index für einen Heap.  
  
    > [!NOTE]  
    >  Das Löschen eines partitionierten gruppierten Index ergibt einen partitionierten Heap.  
  
-   Verwenden Sie die CREATE INDEX-Anweisung von [!INCLUDE[tsql](../../includes/tsql-md.md)] mit der DROP EXISTING = ON-Klausel, um einen vorhandenen partitionierten Index zu löschen und neu zu erstellen.  
  
-   Führen Sie eine Abfolge von ALTER PARTITION FUNCTION-Anweisungen aus.  
  
Alle von ALTER PARTITION FUNCTION betroffenen Dateigruppen müssen online sein.  
  
ALTER PARTITION FUNCTION schlägt fehl, wenn ein deaktivierter gruppierter Index für eine Tabelle vorhanden ist, die die Partitionsfunktion verwendet.  
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bietet keine Replikationsunterstützung für das Ändern einer Partitionsfunktion. Äderungen an einer Partitionsfunktion in der Veröffentlichungsdatenbank müssen in der Abonnementdatenbank manuell angewendet werden.  
  
## <a name="permissions"></a>Berechtigungen  
Die folgenden Berechtigungen können zum Ausführen von ALTER PARTITION FUNCTION verwendet werden:  
  
-   ALTER ANY DATASPACE-Berechtigung. Diese Berechtigung gilt standardmäßig für Mitglieder der festen Serverrolle **sysadmin** und für Mitglieder der festen Datenbankrollen **db_owner** und **db_ddladmin** .  
  
-   Die Berechtigung CONTROL oder ALTER für die Datenbank, in der die Partitionsfunktion erstellt wurde.  
  
-   Die Berechtigung CONTROL SERVER oder ALTER ANY DATABASE auf dem Server der Datenbank, in der die Partitionsfunktion erstellt wurde.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-splitting-a-partition-of-a-partitioned-table-or-index-into-two-partitions"></a>A. Teilen der Partition einer partitionierten Tabelle oder eines partitionierten Index in zwei Partitionen  
Im folgenden Beispiel wird eine Partitionsfunktion zum Partitionieren einer Tabelle oder eines Indexes in vier Partitionen erstellt. `ALTER PARTITION FUNCTION` teilt eine Partition in zwei, sodass insgesamt fünf Partitionen entstehen.  
  
```sql  
IF EXISTS (SELECT * FROM sys.partition_functions  
    WHERE name = 'myRangePF1')  
DROP PARTITION FUNCTION myRangePF1;  
GO  
CREATE PARTITION FUNCTION myRangePF1 (int)  
AS RANGE LEFT FOR VALUES ( 1, 100, 1000 );  
GO  
--Split the partition between boundary_values 100 and 1000  
--to create two partitions between boundary_values 100 and 500  
--and between boundary_values 500 and 1000.  
ALTER PARTITION FUNCTION myRangePF1 ()  
SPLIT RANGE (500);  
```  
  
### <a name="b-merging-two-partitions-of-a-partitioned-table-into-one-partition"></a>B. Zusammenführen von zwei Partitionen einer partitionierten Tabelle zu einer einzigen Partition  
Im folgenden Beispiel wird dieselbe Partitionsfunktion wie oben erstellt, und anschließend werden zwei Partitionen zu einer einzigen Partition zusammengeführt, sodass sich insgesamt drei Partitionen ergeben.  
  
```sql  
IF EXISTS (SELECT * FROM sys.partition_functions  
    WHERE name = 'myRangePF1')  
DROP PARTITION FUNCTION myRangePF1;  
GO  
CREATE PARTITION FUNCTION myRangePF1 (int)  
AS RANGE LEFT FOR VALUES ( 1, 100, 1000 );  
GO  
--Merge the partitions between boundary_values 1 and 100  
--and between boundary_values 100 and 1000 to create one partition  
--between boundary_values 1 and 1000.  
ALTER PARTITION FUNCTION myRangePF1 ()  
MERGE RANGE (100);  
```  
  
## <a name="see-also"></a>Siehe auch  
[Partitionierte Tabellen und Indizes](../../relational-databases/partitions/partitioned-tables-and-indexes.md)   
[CREATE PARTITION FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-partition-function-transact-sql.md)   
[DROP PARTITION FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-partition-function-transact-sql.md)   
[CREATE PARTITION SCHEME &#40;Transact-SQL&#41;](../../t-sql/statements/create-partition-scheme-transact-sql.md)   
[ALTER PARTITION SCHEME &#40;Transact-SQL&#41;](../../t-sql/statements/alter-partition-scheme-transact-sql.md)   
[DROP PARTITION SCHEME &#40;Transact-SQL&#41;](../../t-sql/statements/drop-partition-scheme-transact-sql.md)   
[CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)   
[ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md)   
[CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
[sys.partition_functions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-partition-functions-transact-sql.md)   
[sys.partition_parameters &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-partition-parameters-transact-sql.md) [sys.partition_range_values &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-partition-range-values-transact-sql.md)   
[sys.partitions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-partitions-transact-sql.md)   
[sys.tables &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-tables-transact-sql.md)   
[sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)   
[sys.index_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)  
  
  
