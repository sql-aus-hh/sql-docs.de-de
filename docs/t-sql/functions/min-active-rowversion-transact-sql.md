---
description: MIN_ACTIVE_ROWVERSION (Transact-SQL)
title: MIN_ACTIVE_ROWVERSION (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- MIN_ACTIVE_ROWVERSION
- MIN_ACTIVE_ROWVERSION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MIN_ACTIVE_ROWVERSION function [Transact-SQL]
ms.assetid: 87c89547-8ea1-4820-b75e-36be683e4e10
author: cawrites
ms.author: chadam
ms.openlocfilehash: 415d86958972d059b0cb9a27b8a741ac7a4ed136
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102510"
---
# <a name="min_active_rowversion-transact-sql"></a>MIN_ACTIVE_ROWVERSION (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Gibt den niedrigsten aktiven **rowversion** -Wert in der aktuellen Datenbank zurück. Ein **rowversion** -Wert ist aktiv, wenn er in einer Transaktion verwendet wird, für die noch kein Commit ausgeführt wurde. Weitere Informationen finden Sie unter [rowversion &#40;Transact-SQL&#41;](../../t-sql/data-types/rowversion-transact-sql.md).  
  
> [!NOTE]  
>  Der **rowversion** -Datentyp wird auch als **timestamp** bezeichnet.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql  
MIN_ACTIVE_ROWVERSION ( ) 
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Rückgabetypen
 Gibt einen **binary(8)** -Wert zurück.  
  
## <a name="remarks"></a>Bemerkungen  
 MIN_ACTIVE_ROWVERSION ist eine nicht deterministische Funktion, die den niedrigsten aktiven **rowversion** -Wert in der aktuellen Datenbank zurückgibt. Ein neuer **rowversion** -Wert wird in der Regel generiert, wenn ein Einfüge- oder Updatevorgang für eine Tabelle ausgeführt wird, die eine Spalte vom Typ **rowversion** aufweist. Wenn in der Datenbank keine aktiven Werte vorhanden sind, gibt MIN_ACTIVE_ROWVERSION den gleichen Wert wie @@DBTS + 1 zurück.  
  
 MIN_ACTIVE_ROWVERSION ist hilfreich in Szenarien wie der Datensynchronisierung, in denen Änderungen mithilfe von **rowversion** -Werten gruppiert werden. Wenn eine Anwendung @@DBTS statt MIN_ACTIVE_ROWVERSION verwendet, werden möglicherweise Änderungen übergangen, die bei der Synchronisierung aktiv sind.  
  
 Die Funktion „MIN_ACTIVE_ROWVERSION“ ist nicht von Änderungen in den Transaktionsisolationsstufen betroffen.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel werden **rowversion** -Werte mithilfe von `MIN_ACTIVE_ROWVERSION` und `@@DBTS`. Beachten Sie, dass sich die Werte unterscheiden, wenn in der Datenbank keine aktiven Transaktionen vorhanden sind.  
  
```sql  
-- Create a table that has a ROWVERSION column in it.  
CREATE TABLE RowVersionTestTable (rv ROWVERSION)  
GO  
  
-- Print the current values for the database.  
PRINT ''  
PRINT 'DBTS'  
PRINT @@DBTS  
PRINT 'MIN_ACTIVE_ROWVERSION'  
PRINT MIN_ACTIVE_ROWVERSION()   
GO  
---------------- Results ----------------  
--DBTS  
--0x00000000000007E2  
--MIN_ACTIVE_ROWVERSION  
--0x00000000000007E3  
  
-- Insert a row.  
INSERT INTO RowVersionTestTable VALUES (DEFAULT)  
SELECT * FROM RowVersionTestTable  
GO  
---------------- Results ----------------  
--rv  
--0x00000000000007E3  
  
-- Print the current values for the database.  
PRINT ''  
PRINT 'DBTS'  
PRINT @@DBTS  
PRINT 'MIN_ACTIVE_ROWVERSION'  
PRINT MIN_ACTIVE_ROWVERSION()  
GO  
---------------- Results ----------------  
--DBTS  
--0x00000000000007E3  
--MIN_ACTIVE_ROWVERSION  
--0x00000000000007E4  
  
-- Insert a new row inside a transaction but do not commit.  
BEGIN TRAN  
INSERT INTO RowVersionTestTable VALUES (DEFAULT)  
SELECT * FROM RowVersionTestTable  
GO  
---------------- Results ----------------  
--rv  
--0x00000000000007E3  
--0x00000000000007E4  
  
-- Print the current values for the database.  
PRINT ''  
PRINT 'DBTS'  
PRINT @@DBTS  
PRINT 'MIN_ACTIVE_ROWVERSION'  
PRINT MIN_ACTIVE_ROWVERSION()   
GO  
---------------- Results ----------------  
--DBTS  
--0x00000000000007E4  
--MIN_ACTIVE_ROWVERSION  
--0x00000000000007E4  
  
-- Commit the transaction.  
COMMIT  
GO  
  
-- Print the current values for the database.  
PRINT ''  
PRINT 'DBTS'  
PRINT @@DBTS  
PRINT 'MIN_ACTIVE_ROWVERSION'  
PRINT MIN_ACTIVE_ROWVERSION()  
GO  
---------------- Results ----------------  
--DBTS  
--0x00000000000007E4  
--MIN_ACTIVE_ROWVERSION  
--0x00000000000007E5  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [@@DBTS &#40;Transact-SQL&#41;](../../t-sql/functions/dbts-transact-sql.md)   
 [rowversion &#40;Transact-SQL&#41;](../../t-sql/data-types/rowversion-transact-sql.md)  
  
  
