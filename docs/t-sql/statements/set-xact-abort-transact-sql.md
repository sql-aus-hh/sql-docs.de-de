---
description: SET XACT_ABORT (Transact-SQL)
title: SET XACT_ABORT (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 10/03/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- XACT_ABORT_TSQL
- XACT_ABORT
- SET XACT_ABORT
- SET_XACT_ABORT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- transaction rollbacks [SQL Server]
- XACT_ABORT option
- automatic transaction roll backs
- transactions [SQL Server], rolling back
- rolling back transactions, SET XACT_ABORT
- roll back transactions [SQL Server]
- SET XACT_ABORT statement
ms.assetid: cbcaa433-58f2-4dc3-a077-27273bef65b5
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 29737fa62f99028ebf35bca5d8e9c0e94fa1af21
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98079907"
---
# <a name="set-xact_abort-transact-sql"></a>SET XACT_ABORT (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

> [!NOTE]
> **SET XACT_ABORT** wird von der **THROW**-Anweisung berücksichtigt. **RAISERROR** wird nicht berücksichtigt. In neuen Anwendungen sollte **THROW** statt **RAISERROR** verwendet werden.

Gibt an, ob [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] für die aktuelle Transaktion automatisch ein Rollback ausführt, wenn eine [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisung einen Laufzeitfehler ausgibt.

![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Syntax

```syntaxsql
SET XACT_ABORT { ON | OFF }
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Bemerkungen

Wenn SET XACT_ABORT auf ON festgelegt ist und eine [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisung einen Laufzeitfehler auslöst, wird die gesamte Transaktion beendet, und es wird ein Rollback für sie ausgeführt.

Wenn SET XACT_ABORT auf OFF festgelegt ist, wird in einigen Fällen nur für die [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisung, die den Fehler ausgelöst hat, ein Rollback ausgeführt, und die Transaktion wird fortgesetzt. Abhängig vom Schweregrad des Fehlers wird möglicherweise für die gesamte Transaktion ein Rollback ausgeführt, wenn SET XACT_ABORT auf OFF festgelegt ist. OFF ist die Standardeinstellung in einer T-SQL-Anweisung, während ON die Standardeinstellung in einem Trigger ist.

Kompilierungsfehler, wie z. B. Syntaxfehler, sind von SET XACT_ABORT nicht betroffen.

Es ist erforderlich, dass XACT_ABORT für Anweisungen zur Datenänderung in einer impliziten oder expliziten Transaktion für die meisten OLE DB-Anbieter, einschließlich [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], auf ON festgelegt ist. Nur wenn der Anbieter geschachtelte Transaktionen unterstützt, ist diese Option nicht erforderlich.

Bei ANSI_WARNINGS=OFF werden Transaktionen aufgrund von Berechtigungsverletzungen abgebrochen.

Die Einstellung von SET XACT_ABORT wird zur Ausführungszeit und nicht zur Analysezeit festgelegt.

Um die aktuelle Einstellung anzuzeigen, führen Sie die folgende Abfrage aus.

```sql
DECLARE @XACT_ABORT VARCHAR(3) = 'OFF';
IF ( (16384 & @@OPTIONS) = 16384 ) SET @XACT_ABORT = 'ON';
SELECT @XACT_ABORT AS XACT_ABORT;

```

## <a name="examples"></a>Beispiele

Im folgenden Codebeispiel wird eine Fremdschlüsselverletzung in einer Transaktion verursacht, die weitere [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisungen enthält. In der ersten Anweisungsgruppe wird der Fehler generiert; die anderen Anweisungen werden jedoch erfolgreich ausgeführt, und für die Transaktion wird erfolgreich ein Commit ausgeführt. Im zweiten Anweisungssatz ist `SET XACT_ABORT` auf `ON` festgelegt. Der Anweisungsfehler bewirkt daher, dass der Batch abgebrochen und für die Transaktion ein Rollback ausgeführt wird.

```sql
IF OBJECT_ID(N't2', N'U') IS NOT NULL
    DROP TABLE t2;
GO
IF OBJECT_ID(N't1', N'U') IS NOT NULL
    DROP TABLE t1;
GO  
CREATE TABLE t1
    (a INT NOT NULL PRIMARY KEY);
CREATE TABLE t2
    (a INT NOT NULL REFERENCES t1(a));
GO
INSERT INTO t1 VALUES (1);
INSERT INTO t1 VALUES (3);
INSERT INTO t1 VALUES (4);
INSERT INTO t1 VALUES (6);
GO
SET XACT_ABORT OFF;
GO
BEGIN TRANSACTION;
INSERT INTO t2 VALUES (1);
INSERT INTO t2 VALUES (2); -- Foreign key error.
INSERT INTO t2 VALUES (3);
COMMIT TRANSACTION;
GO
SET XACT_ABORT ON;
GO
BEGIN TRANSACTION;
INSERT INTO t2 VALUES (4);
INSERT INTO t2 VALUES (5); -- Foreign key error.
INSERT INTO t2 VALUES (6);
COMMIT TRANSACTION;
GO
-- SELECT shows only keys 1 and 3 added.
-- Key 2 insert failed and was rolled back, but
-- XACT_ABORT was OFF and rest of transaction
-- succeeded.
-- Key 5 insert error with XACT_ABORT ON caused
-- all of the second transaction to roll back.
SELECT *
 FROM t2;
GO
```

## <a name="see-also"></a>Weitere Informationen

- [THROW &#40;Transact-SQL&#41;](../../t-sql/language-elements/throw-transact-sql.md)
- [BEGIN TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/begin-transaction-transact-sql.md)
- [COMMIT TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/commit-transaction-transact-sql.md)
- [ROLLBACK TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/rollback-transaction-transact-sql.md)
- [SET-Anweisungen &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)
- [@@TRANCOUNT &#40;Transact-SQL&#41;](../../t-sql/functions/trancount-transact-sql.md)  
