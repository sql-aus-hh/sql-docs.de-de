---
description: COL_NAME (Transact-SQL)
title: COL_NAME (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- COL_NAME
- COL_NAME_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- column properties [SQL Server]
- COL_NAME function
- column names [SQL Server]
- names [SQL Server], columns
ms.assetid: 214144ab-f2bc-4052-83cf-caf0a85c4cc6
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 003629030dfd9fdaeab4f98ced539609c5a4cabc
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101353"
---
# <a name="col_name-transact-sql"></a>COL_NAME (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Diese Funktion gibt den Namen der Tabellenspalte basierend auf den Werten der Tabellen-ID und der Spalten-ID der entsprechenden Tabellenspalte zurück.
  
![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
COL_NAME ( table_id , column_id )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
*table_id*  
Die ID der Tabelle, die diese Spalte enthält. Das Argument *table_id* weist den Datentyp **int** auf.
  
*column_id*  
Die ID der Spalte. Das Argument *column_id* weist den Datentyp **int** auf.
  
## <a name="return-types"></a>Rückgabetypen
**sysname**
  
## <a name="exceptions"></a>Ausnahmen  
Gibt NULL zurück bei einem Fehler oder wenn ein Aufrufer nicht über die korrekte Berechtigung zum Anzeigen des Objekts verfügt.
  
In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] kann ein Benutzer nur die Metadaten sicherungsfähiger Elemente anzeigen, bei denen der Benutzer entweder der Besitzer ist oder für die dem Benutzer eine Berechtigung erteilt wurde. Dies bedeutet, dass Metadaten ausgebende integrierte Funktionen, z.B. `COL_NAME`, möglicherweise NULL zurückgeben, wenn dem Benutzer für das Objekt nicht die korrekten Berechtigungen erteilt wurden. Weitere Informationen finden Sie unter [Konfigurieren der Sichtbarkeit von Metadaten](../../relational-databases/security/metadata-visibility-configuration.md).
  
## <a name="remarks"></a>Bemerkungen  
Der *table_id*- und der *column_id*-Parameter erzeugen zusammen eine Spaltennamenzeichenfolge.
  
Weitere Informationen zum Abrufen von Tabellen- und Spalten-IDs finden Sie unter [OBJECT_ID &#40;Transact-SQL&#41;](../../t-sql/functions/object-id-transact-sql.md).
  
## <a name="examples"></a>Beispiele  
Dieses Beispiel gibt den Namen der ersten Spalte in der Beispieltabelle `Employee` zurück.
  
```sql
-- Uses AdventureWorks  
  
SELECT COL_NAME(OBJECT_ID('dbo.FactResellerSales'), 1) AS FirstColumnName,  
COL_NAME(OBJECT_ID('dbo.FactResellerSales'), 2) AS SecondColumnName;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
ColumnName          
------------   
BusinessEntityID  
```  
  
## <a name="see-also"></a>Weitere Informationen
[Ausdrücke &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)  
[Metadata Functions &#40;Transact-SQL&#41; (Metadatenfunktionen (Transact-SQL))](../../t-sql/functions/metadata-functions-transact-sql.md)  
[COLUMNPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/columnproperty-transact-sql.md)  
[COL_LENGTH &#40;Transact-SQL&#41;](../../t-sql/functions/col-length-transact-sql.md)
  
  

