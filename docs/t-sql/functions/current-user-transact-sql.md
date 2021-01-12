---
description: CURRENT_USER (Transact-SQL)
title: CURRENT_USER (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CURRENT_USER
- CURRENT_USER_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- usernames [SQL Server]
- current user names
- niladic functions
- CURRENT_USER
- users [SQL Server], names
ms.assetid: 29248949-325b-4063-9f55-5a445fb35c6e
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 16c55b4a28ce58e62b4af19fa9e54c4bf780170f
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097219"
---
# <a name="current_user-transact-sql"></a>CURRENT_USER (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Diese Funktion gibt den Namen des aktuellen Benutzers zurück. Diese Funktion ist gleichbedeutend mit `USER_NAME()`.
  
![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
CURRENT_USER  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Rückgabetypen
**sysname**
  
## <a name="remarks"></a>Bemerkungen  
`CURRENT_USER` gibt den Namen des aktuellen Sicherheitskontexts zurück. Wird `CURRENT_USER` ausgeführt, nachdem der Kontext über einen Aufruf von `EXECUTE AS` gewechselt wurde, gibt `CURRENT_USER` den Namen des gewechselten Identitätskontexts zurück. Falls ein Windows-Prinzipal über eine Mitgliedschaft in einer Gruppe auf die Datenbank zugegriffen hat, gibt `CURRENT_USER` den Namen des Windows-Prinzipals anstelle des Gruppennamens zurück.
  
Weitere Informationen dazu, wie der Anmeldename des aktuellen Benutzers zurückgegeben werden kann, finden Sie unter [SUSER_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/suser-name-transact-sql.md) und [SYSTEM_USER &#40;Transact-SQL&#41;](../../t-sql/functions/system-user-transact-sql.md).
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-using-current_user-to-return-the-current-user-name"></a>A. Verwenden von CURRENT_USER zur Rückgabe des aktuellen Benutzernamens  
In diesem Beispiel wird der Name des aktuellen Benutzers zurückgegeben.
  
```sql
SELECT CURRENT_USER;  
GO  
```  
  
### <a name="b-using-current_user-as-a-default-constraint"></a>B. Verwenden von CURRENT_USER als DEFAULT-Einschränkung  
In diesem Beispiel wird eine Tabelle erstellt, die `CURRENT_USER` als `DEFAULT`-Einschränkung für die `order_person`-Spalte in einer Zeile mit Verkaufszahlen verwendet.
  
```sql
USE AdventureWorks2012;  
GO  
IF EXISTS (SELECT TABLE_NAME FROM INFORMATION_SCHEMA.TABLES  
      WHERE TABLE_NAME = 'orders22')  
   DROP TABLE orders22;  
GO  
SET NOCOUNT ON;  
CREATE TABLE orders22  
(  
order_id int IDENTITY(1000, 1) NOT NULL,
cust_id  int NOT NULL,
order_date smalldatetime NOT NULL DEFAULT GETDATE(),
order_amt money NOT NULL,
order_person char(30) NOT NULL DEFAULT CURRENT_USER
);  
GO  
```  
  
In diesem Beispiel wird ein Datensatz in die Tabelle eingefügt. Die Benutzerin mit dem Namen `Wanida` führt diese Anweisungen aus.
  
```sql
INSERT orders22 (cust_id, order_amt)  
VALUES (5105, 577.95);  
GO  
SET NOCOUNT OFF;  
GO  
```  
  
In dieser Abfrage werden alle Informationen in der `orders22`-Tabelle ausgewählt.
  
```sql
SELECT * FROM orders22;  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
order_id    cust_id     order_date           order_amt    order_person
----------- ----------- -------------------- ------------ ------------
1000        5105        2005-04-03 23:34:00  577.95       Wanida
  
(1 row(s) affected)
```
  
### <a name="c-using-current_user-from-an-impersonated-context"></a>C. Verwenden von CURRENT_USER aus einem Kontext, dessen Identität angenommen wurde  
In diesem Beispiel führt die Benutzerin `Wanida` den folgenden [!INCLUDE[tsql](../../includes/tsql-md.md)]-Code aus, um sich als Benutzer „Arnalfo“ auszugeben.
  
```sql
SELECT CURRENT_USER;  
GO  
EXECUTE AS USER = 'Arnalfo';  
GO  
SELECT CURRENT_USER;  
GO  
REVERT;  
GO  
SELECT CURRENT_USER;  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
Wanida
Arnalfo
Wanida
```
  
## <a name="see-also"></a>Weitere Informationen
[USER_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/user-name-transact-sql.md)  
[SYSTEM_USER &#40;Transact-SQL&#41;](../../t-sql/functions/system-user-transact-sql.md)  
[sys.database_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)  
[ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)  
[CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)  
[Systemfunktionen &#40;Transact-SQL&#41;](../../relational-databases/system-functions/system-functions-category-transact-sql.md)
  
  

