---
description: SET FMTONLY (Transact-SQL)
title: SET FMTONLY (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/03/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- FMTONLY_TSQL
- FMTONLY
- SET FMTONLY
- SET_FMTONLY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- metadata [SQL Server], only metadata returned
- SET FMTONLY statement
- FMTONLY option
ms.assetid: 02a1d9ac-2e58-433c-9a07-2c5a4a2214e1
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6bb9fb1cf9b10f27bcf90b9d4deb56bcd6484044
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093300"
---
# <a name="set-fmtonly-transact-sql"></a>SET FMTONLY (Transact-SQL)

[!INCLUDE[tsql-appliesto-ss-all-md](../../includes/tsql-appliesto-ss-all-md.md)]

  Gibt nur Metadaten an den Client zurück. Kann verwendet werden, um das Format der Antwort zu testen, ohne die Abfrage tatsächlich auszuführen.  

> [!NOTE]
> Verwenden Sie diese Funktion nicht. Diese Funktion wurde durch folgende Elemente ersetzt:
>
> - [sp_describe_first_result_set (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-describe-first-result-set-transact-sql.md)
> - [sp_describe_undeclared_parameters (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-describe-undeclared-parameters-transact-sql.md)
> - [sys.dm_exec_describe_first_result_set (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-describe-first-result-set-transact-sql.md)
> - [sys.dm_exec_describe_first_result_set_for_object (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-describe-first-result-set-for-object-transact-sql.md)

 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
SET FMTONLY { ON | OFF }   
```  

## <a name="remarks"></a>Hinweise

Wenn `FMTONLY` auf `ON` gesetzt ist, wird ein Rowset mit den Spaltennamen zurückgeben, jedoch ohne jegliche Datenzeilen.

`SET FMTONLY ON` hat beim Analysieren des Transact-SQL-Batches keine Auswirkungen. Der Wirkung tritt während der Ausführungslaufzeit auf.

Der Standardwert ist `OFF`.

## <a name="permissions"></a>Berechtigungen  
 Erfordert die Mitgliedschaft in der public-Rolle.  

## <a name="examples"></a>Beispiele

Im folgenden Transact-SQL-Codebeispiel wird `FMTONLY` auf `ON` gesetzt. Aufgrund dieser Einstellung gibt SQL Server nur Metadateninformationen zu den ausgewählten Spalten zurück. Insbesondere die Spaltennamen werden zurückgegeben. Es werden keine Datenzeilen zurückgegeben.

Im Beispiel gibt die Testausführung der gespeicherten Prozedur `prc_gm29` Folgendes zurück:

- Mehrere Rowsets
- Spalten aus mehreren Tabellen in einer ihrer `SELECT`-Anweisungen

<!--
Issue 2246 inspired this code example, and the replacement of the two pre-existing examples. 2019/June/03, GM.
-->

```sql
SET NOCOUNT ON;
GO

DROP PROCEDURE IF EXISTS prc_gm29;

DROP TABLE IF EXISTS #tabTemp41;
DROP TABLE IF EXISTS #tabTemp42;
GO

CREATE TABLE #tabTemp41
(
   KeyInt41        INT           NOT NULL,
   Name41          NVARCHAR(16)  NOT NULL,
   TargetDateTime  DATETIME      NOT NULL  DEFAULT GetDate()
);

CREATE TABLE #tabTemp42
(
   KeyInt42 INT          NOT NULL,   -- JOIN-able to KeyInt41.
   Name42   NVARCHAR(16) NOT NULL
);
GO

INSERT INTO #tabTemp41 (KeyInt41, Name41) VALUES (10, 't41-c');
INSERT INTO #tabTemp42 (KeyInt42, Name42) VALUES (10, 't42-p');
GO

CREATE PROCEDURE prc_gm29
AS
BEGIN
SELECT * FROM #tabTemp41;
SELECT * FROM #tabTemp42;

SELECT t41.KeyInt41, t41.TargetDateTime, t41.Name41, t42.Name42
   FROM
                 #tabTemp41 AS t41
      INNER JOIN #tabTemp42 AS t42 on t42.KeyInt42 = t41.KeyInt41
END;
GO

SET DATEFORMAT mdy;

SET FMTONLY ON;
EXECUTE prc_gm29;   -- Returns multiple tables.
SET FMTONLY OFF;
GO

DROP PROCEDURE IF EXISTS prc_gm29;

DROP TABLE IF EXISTS #tabTemp41;
DROP TABLE IF EXISTS #tabTemp42;
GO

/****  Actual Output:
[C:\JunkM\]
>> osql.exe -S myazuresqldb.database.windows.net -U somebody -P secret -d MyDatabase -i C:\JunkM\Issue-2246-a.SQL 

 KeyInt41    Name41           TargetDateTime
 ----------- ---------------- -----------------------

 KeyInt42    Name42
 ----------- ----------------

 KeyInt41    TargetDateTime          Name41           Name42
 ----------- ----------------------- ---------------- ----------------


[C:\JunkM\]
>>
****/
```

## <a name="see-also"></a>Siehe auch  
 [SET-Anweisungen &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)  
  
  

