---
description: SET ANSI_NULL_DFLT_OFF (Transact-SQL)
title: SET ANSI_NULL_DFLT_OFF (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 12/04/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ANSI_NULL_DFLT_OFF_TSQL
- ANSI_NULL_DFLT_OFF
- SET ANSI_NULL_DFLT_OFF
- SET_ANSI_NULL_DFLT_OFF_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- default nullability
- ANSI_NULL_DFLT_OFF option
- null values [SQL Server], overriding
- overriding default nullability
- SET ANSI_NULL_DFLT_OFF statement
ms.assetid: 8ed5c512-f5de-4741-a18a-de85a3041295
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0141e6a854da807287fc1ab9b12fc8599489a9e1
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98080610"
---
# <a name="set-ansi_null_dflt_off-transact-sql"></a>SET ANSI_NULL_DFLT_OFF (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Ändert das Sitzungsverhalten, sodass die Standardeinstellung der NULL-Zulässigkeit für neue Spalten überschrieben wird, wenn die Option ANSI NULL Default für die Datenbank auf **TRUE** festgelegt ist. Weitere Informationen zum Festlegen eines Werts für ANSI NULL Default finden Sie unter [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md).  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  

## <a name="syntax"></a>Syntax

```syntaxsql
-- Syntax for SQL Server and Azure SQL Database
  
SET ANSI_NULL_DFLT_OFF { ON | OFF }
```

```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse

SET ANSI_NULL_DFLT_OFF OFF
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Hinweise
 Diese Einstellung betrifft die NULL-Zulässigkeit für neue Spalten nur, wenn die NULL-Zulässigkeit der Spalte nicht in den CREATE TABLE- und ALTER TABLE-Anweisungen angegeben wurde. Wenn für SET ANSI_NULL_DFLT_OFF die Einstellung ON festgelegt ist und mit ALTER TABLE- und CREATE TABLE-Anweisungen neue Spalten erstellt werden, sind diese standardmäßig NOT NULL, falls der NULL-Zulässigkeitsstatus der Spalte nicht explizit angegeben ist. SET ANSI_NULL_DFLT_OFF wirkt sich nicht auf Spalten aus, die mit einer expliziten Angabe von NULL oder NOT NULL erstellt wurden.  
  
 Für SET ANSI_NULL_DFLT_OFF und SET ANSI_NULL_DFLT_ON kann nicht gleichzeitig ON festgelegt werden. Wird eine der beiden Optionen aktiviert (ON), so wird die andere deaktiviert (OFF). Daher kann entweder ANSI_NULL_DFLT_OFF oder ANSI_NULL_DFLT_ON auf ON festgelegt werden, oder beide Optionen können auf OFF festgelegt werden. Wenn eine der beiden Optionen auf ON festgelegt ist, tritt die entsprechende Einstellung (SET ANSI_NULL_DFLT_OFF oder SET ANSI_NULL_DFLT_ON) in Kraft. Wird für beide Optionen OFF festgelegt, verwendet [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] den Wert der Spalte is_ansi_null_default_on in der Katalogsicht [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).  
  
 Am zuverlässigsten arbeiten [!INCLUDE[tsql](../../includes/tsql-md.md)]-Skripts, die in Datenbanken mit unterschiedlichen Einstellungen der NULL-Zulässigkeit verwendet werden, wenn in CREATE TABLE- und ALTER TABLE-Anweisungen immer NULL oder NOT NULL angegeben wird.  
  
 Die Einstellung von SET ANSI_NULL_DFLT_OFF wird zur Ausführungszeit und nicht zur Analysezeit festgelegt.  
  
 Um die aktuelle Einstellung anzuzeigen, führen Sie die folgende Abfrage aus.  
  
```sql  
DECLARE @ANSI_NULL_DFLT_OFF VARCHAR(3) = 'OFF';  
IF ( (2048 & @@OPTIONS) = 2048 ) SET @ANSI_NULL_DFLT_OFF = 'ON';  
SELECT @ANSI_NULL_DFLT_OFF AS ANSI_NULL_DFLT_OFF;  
```  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die Mitgliedschaft in der public-Rolle.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel werden die Auswirkungen der beiden Einstellungen von `SET ANSI_NULL_DFLT_OFF` für die Datenbankoption ANSI NULL Default gezeigt.  
  
```sql  
USE AdventureWorks2012;  
GO  
  
-- Set the 'ANSI null default' database option to true by executing   
-- ALTER DATABASE.  
GO  
ALTER DATABASE AdventureWorks2012 SET ANSI_NULL_DEFAULT ON;  
GO  
-- Create table t1.  
CREATE TABLE t1 (a TINYINT);  
GO  
-- NULL INSERT should succeed.  
INSERT INTO t1 (a) VALUES (NULL);  
GO  
  
-- SET ANSI_NULL_DFLT_OFF to ON and create table t2.  
SET ANSI_NULL_DFLT_OFF ON;  
GO  
CREATE TABLE t2 (a TINYINT);  
GO   
-- NULL INSERT should fail.  
INSERT INTO t2 (a) VALUES (NULL);  
GO  
  
-- SET ANSI_NULL_DFLT_OFF to OFF and create table t3.  
SET ANSI_NULL_DFLT_OFF OFF;  
GO  
CREATE TABLE t3 (a TINYINT) ;  
GO   
-- NULL INSERT should succeed.  
INSERT INTO t3 (a) VALUES (NULL);  
GO  
  
-- This illustrates the effect of having both the database  
-- option and SET option disabled.  
-- Set the 'ANSI null default' database option to false.  
ALTER DATABASE AdventureWorks2012 SET ANSI_NULL_DEFAULT OFF;  
GO  
-- Create table t4.  
CREATE TABLE t4 (a TINYINT) ;  
GO   
-- NULL INSERT should fail.  
INSERT INTO t4 (a) VALUES (NULL);  
GO  
  
-- SET ANSI_NULL_DFLT_OFF to ON and create table t5.  
SET ANSI_NULL_DFLT_OFF ON;  
GO  
CREATE TABLE t5 (a TINYINT);  
GO   
-- NULL insert should fail.  
INSERT INTO t5 (a) VALUES (NULL);  
GO  
  
-- SET ANSI_NULL_DFLT_OFF to OFF and create table t6.  
SET ANSI_NULL_DFLT_OFF OFF;  
GO  
CREATE TABLE t6 (a TINYINT);   
GO   
-- NULL insert should fail.  
INSERT INTO t6 (a) VALUES (NULL);  
GO  
  
-- Drop tables t1 through t6.  
DROP TABLE t1, t2, t3, t4, t5, t6;  
  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [SET-Anweisungen (Transact-SQL)](../../t-sql/statements/set-statements-transact-sql.md)   
 [SET ANSI_NULL_DFLT_ON &#40;Transact-SQL&#41;](../../t-sql/statements/set-ansi-null-dflt-on-transact-sql.md)  
  
  
