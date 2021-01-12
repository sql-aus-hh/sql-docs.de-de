---
description: COL_LENGTH (Transact-SQL)
title: COL_LENGTH (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- COL_LENGTH
- COL_LENGTH_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- lengths [SQL Server], columns
- COL_LENGTH function
- column properties [SQL Server]
- column length [SQL Server]
ms.assetid: cf891206-c49f-40eb-858e-eefd2b638a33
author: cawrites
ms.author: chadam
ms.openlocfilehash: 3ad0d568f52fbec8d1feec0ec734b2722a0247e3
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102614"
---
# <a name="col_length-transact-sql"></a>COL_LENGTH (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Diese Funktion gibt die definierte Länge einer Spalte in Bytes zurück.
  
![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
COL_LENGTH ( 'table' , 'column' )   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
**'** *table* **'**  
Der Name der Tabelle, für die die Spaltenlängeninformationen bestimmt werden sollen. *table*: Ein Ausdruck vom Typ **nvarchar**.
  
**'** *column* **'**  
Der Name der Spalte, für die die Länge bestimmt werden soll. *column*: Ein Ausdruck vom Typ **nvarchar**.
  
## <a name="return-type"></a>Rückgabetyp
**smallint**
  
## <a name="exceptions"></a>Ausnahmen  
Gibt NULL zurück bei einem Fehler oder wenn ein Aufrufer nicht über die korrekte Berechtigung zum Anzeigen des Objekts verfügt.
  
In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] kann ein Benutzer nur die Metadaten sicherungsfähiger Elemente anzeigen, bei denen der Benutzer entweder der Besitzer ist oder für die dem Benutzer eine Berechtigung erteilt wurde. Dies bedeutet, dass Metadaten ausgebende integrierte Funktionen, z.B. COL_LENGTH, möglicherweise NULL zurückgeben, wenn dem Benutzer für das Objekt nicht die korrekte Berechtigung erteilt wurde. Weitere Informationen finden Sie unter [Konfigurieren der Sichtbarkeit von Metadaten](../../relational-databases/security/metadata-visibility-configuration.md).
  
## <a name="remarks"></a>Bemerkungen  
Für **varchar**-Spalten, die mit dem Bezeichner **max** deklariert wurden (**varchar(max)**), gibt COL_LENGTH den Wert –1 zurück.
  
## <a name="examples"></a>Beispiele  
Dieses Beispiel zeigt die Rückgabewerte für eine Spalte vom Typ `varchar(40)` und eine Spalte vom Typ `nvarchar(40)`:
  
```sql
USE AdventureWorks2012;  
GO  
CREATE TABLE t1(c1 VARCHAR(40), c2 NVARCHAR(40) );  
GO  
SELECT COL_LENGTH('t1','c1')AS 'VarChar',  
      COL_LENGTH('t1','c2')AS 'NVarChar';  
GO  
DROP TABLE t1;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
VarChar     NVarChar  
40          80  
```  
  
## <a name="see-also"></a>Weitere Informationen
[Ausdrücke &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)  
[Metadata Functions &#40;Transact-SQL&#41; (Metadatenfunktionen (Transact-SQL))](../../t-sql/functions/metadata-functions-transact-sql.md)  
[COL_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/col-name-transact-sql.md)  
[COLUMNPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/columnproperty-transact-sql.md)
  
  
