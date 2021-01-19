---
description: DBCC CHECKCONSTRAINTS (Transact-SQL)
title: DBCC CHECKCONSTRAINTS (Transact-SQL)
ms.custom: ''
ms.date: 11/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DBCC CHECKCONSTRAINTS
- DBCC_CHECKCONSTRAINTS_TSQL
- CHECKCONSTRAINTS
- CHECKCONSTRAINTS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- DBCC CHECKCONSTRAINTS statement
- consistency [SQL Server], constraints
- checking constraint consistency
- constraints [SQL Server], consistency checks
- integrity [SQL Server], constraints
ms.assetid: da6c9cee-6687-46e8-b504-738551f9068b
author: pmasl
ms.author: umajay
ms.openlocfilehash: a0257d403980bcb869a7ac1e06471cd1699dd16d
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170602"
---
# <a name="dbcc-checkconstraints-transact-sql"></a>DBCC CHECKCONSTRAINTS (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Überprüft die Integrität einer angegebenen Einschränkung oder aller Einschränkungen einer angegebenen Tabelle in der aktuellen Datenbank.
  
![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
DBCC CHECKCONSTRAINTS  
[   
    (   
    table_name | table_id | constraint_name | constraint_id   
    )  
]  
    [ WITH   
    [ { ALL_CONSTRAINTS | ALL_ERRORMSGS } ]  
    [ , ] [ NO_INFOMSGS ]   
    ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *table_name* \| *table_id* \| *constraint_name* \| *constraint_id*  
 Die zu überprüfende Tabelle oder Einschränkung. Wenn *table_name* oder *table_id* angegeben ist, werden alle aktivierten Einschränkungen der Tabelle überprüft. Wenn *constraint_name* oder *constraint_id* angegeben wird, wird nur diese Einschränkung überprüft. Wenn weder ein Tabellenbezeichner noch ein Einschränkungsbezeichner angegeben ist, werden alle aktivierten Einschränkungen für alle Tabellen in der aktuellen Datenbank überprüft.  
 Durch den Einschränkungsnamen wird die zugehörige Tabelle eindeutig identifiziert. Weitere Informationen finden Sie unter [Datenbankbezeichner](../../relational-databases/databases/database-identifiers.md).  
  
 WITH  
 Aktiviert anzugebende Optionen.  
  
 ALL_CONSTRAINTS  
 Überprüft alle aktivierten und deaktivierten Einschränkungen der Tabelle, wenn der Tabellenname angegeben ist oder wenn alle Tabellen überprüft werden. Andernfalls wird nur die aktivierte Einschränkung überprüft. ALL_CONSTRAINTS hat keine Wirkung, wenn ein Einschränkungsname angegeben ist.  
  
 ALL_ERRORMSGS  
 Gibt alle Zeilen zurück, die den Einschränkungen der überprüften Tabelle nicht entsprechen. Standardmäßig sind dies die ersten 200 Zeilen.  
  
 NO_INFOMSGS  
 Alle Informationsmeldungen werden unterdrückt.  
  
## <a name="remarks"></a>Bemerkungen  
DBCC CHECKCONSTRAINTS erstellt für alle FOREIGN KEY- und CHECK-Einschränkungen einer Tabelle eine Abfrage und führt sie aus.
  
Eine FOREIGN KEY-Abfrage sieht beispielsweise folgendermaßen aus:
  
```sql
SELECT <columns>  
FROM <table_being_checked> LEFT JOIN <referenced_table>  
    ON <table_being_checked.fkey1> = <referenced_table.pkey1>   
    AND <table_being_checked.fkey2> = <referenced_table.pkey2>  
WHERE <table_being_checked.fkey1> IS NOT NULL   
    AND <referenced_table.pkey1> IS NULL  
    AND <table_being_checked.fkey2> IS NOT NULL  
    AND <referenced_table.pkey2> IS NULL  
```  
  
Die Abfragedaten werden in einer temporären Tabelle gespeichert. Wenn alle geforderten Tabellen oder Einschränkungen überprüft wurden, wird das Resultset zurückgegeben.
DBCC CHECKCONSTRAINTS prüft die Integrität von FOREIGN KEY- und CHECK-Einschränkungen, aber nicht die Integrität der auf dem Datenträger gespeicherten Datenstrukturen einer Tabelle. Solche Datenstrukturprüfungen können mit [DBCC CHECKDB](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md) und [DBCC CHECKTABLE](../../t-sql/database-console-commands/dbcc-checktable-transact-sql.md) ausgeführt werden.
  
**Gilt für**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] und höher
  
Wenn *table_name* oder *table_id* angegeben wird und DBCC CHECKCONSTRAINTS für die Versionsverwaltung durch das System aktiviert ist, führt der Befehl außerdem Konsistenzprüfungen temporärer Daten für die angegebene Tabelle aus. Wenn *NO_INFOMSGS* nicht angegeben ist, gibt dieser Befehl jeden Konsistenzverstoß in der Ausgabe in einer separaten Zeile zurück. Das Format der Ausgabe ist ([pkcol1], [pkcol2]..) = (\<pkcol1_value>, \<pkcol2_value>...) AND \<what is wrong with temporal table record>.
  
|Prüfen|Zusätzliche Informationen in der Ausgabe, wenn die Prüfung einen Fehler zurückgibt|  
|-----------|-----------------------------------------------|  
|PeriodEndColumn ≥ PeriodStartColumn (aktuell)|[sys_end] = '{0}' AND MAX(DATETIME2) = '9999-12-31 23:59:59.99999'|  
|PeriodEndColumn ≥ PeriodStartColumn (aktuell, Vergangenheit)|[sys_start] = '{0}' AND [sys_end] = '{1}'|  
|PeriodStartColumn < current_utc_time (aktuell)|[sys_start] = '{0}' AND SYSUTCTIME|  
|PeriodEndColumn < current_utc_time (Vergangenheit)|[sys_end] = '{0}' AND SYSUTCTIME|  
|Überschneidungen|(sys_start1, sys_end1) , (sys_start2, sys_end2) für zwei sich überschneidende Einträge.<br /><br /> Wenn sich mehr als 2 Einträge überschneiden, enthält die Ausgabe mehrere Zeilen, die sich überschneiden.|  
  
Es ist nicht möglich, constraint_name oder constraint_id angeben, um nur temporäre Konsistenzprüfungen auszuführen.
  
## <a name="result-sets"></a>Resultsets  
DBCC CHECKCONSTRAINTS gibt ein Rowset mit folgenden Spalten zurück.
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|Tabellenname|**varchar**|Der Name der Tabelle.|  
|Constraint Name|**varchar**|Name der verletzten Einschränkung.|  
|Hierbei gilt:|**varchar**|Spaltenwertzuweisungen, die die Zeile bzw. die Zeilen, die die Einschränkung verletzen, identifizieren.<br /><br /> Der Wert in dieser Spalte kann in einer WHERE-Klausel einer SELECT-Anweisung verwendet werden, die hinsichtlich Zeilen abfragt, die die Einschränkung verletzen.|  
  
## <a name="permissions"></a>Berechtigungen  
Erfordert die Mitgliedschaft in der festen Serverrolle **sysadmin** oder der festen Datenbankrolle **db_owner** .
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-checking-a-table"></a>A. Überprüfen einer Tabelle  
Im folgenden Beispiel wird die Einschränkungsintegrität der `Table1`-Tabelle in der `AdventureWorks`-Datenbank überprüft.
  
```sql  
USE AdventureWorks2012;  
GO  
CREATE TABLE Table1 (Col1 INT, Col2 CHAR(30));  
GO  
INSERT INTO Table1 VALUES (100, 'Hello');  
GO  
ALTER TABLE Table1 WITH NOCHECK ADD CONSTRAINT chkTab1 CHECK (Col1 > 100);  
GO  
DBCC CHECKCONSTRAINTS(Table1);  
GO  
```  
  
### <a name="b-checking-a-specific-constraint"></a>B. Überprüfen einer bestimmten Einschränkung  
Im folgenden Beispiel wird die Integrität der `CK_ProductCostHistory_EndDate`-Einschränkung überprüft.
  
```sql  
USE AdventureWorks2012;  
GO  
DBCC CHECKCONSTRAINTS ('Production.CK_ProductCostHistory_EndDate');  
GO  
```  
  
### <a name="c-checking-all-enabled-and-disabled-constraints-on-all-tables"></a>C. Überprüfen aller aktivierten und deaktivierten Einschränkungen für alle Tabellen  
 Im folgenden Beispiel wird die Integrität aller aktivierten und deaktivierten Einschränkungen für alle Tabellen in der aktuellen Datenbank überprüft.  
  
```sql  
DBCC CHECKCONSTRAINTS WITH ALL_CONSTRAINTS;  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
[DBCC CHECKDB &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md)  
[DBCC CHECKTABLE &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-checktable-transact-sql.md)  
[DBCC &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-transact-sql.md)
  
  
