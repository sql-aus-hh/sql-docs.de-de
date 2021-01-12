---
description: FILE_ID (Transact-SQL)
title: FILE_ID (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- FILE_ID
- FILE_ID_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- IDs [SQL Server], files
- file IDs [SQL Server]
- FILE_ID function
- names [SQL Server], files
- identification numbers [SQL Server], files
- file names [SQL Server], FILE_ID
ms.assetid: 6a7382cf-a360-4d62-b9d2-5d747f56f076
author: cawrites
ms.author: chadam
ms.openlocfilehash: 780ca2746f1370a84c72334b6676c6d32e71d577
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092311"
---
# <a name="file_id-transact-sql"></a>FILE_ID (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Für den angegebenen logischen Namen für eine Komponentendatei der aktuellen Datenbank gibt diese Funktion die Datei-ID zurück.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Verwenden Sie stattdessen [FILE_IDEX](../../t-sql/functions/file-idex-transact-sql.md).  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql  
FILE_ID ( file_name )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
*file_name*  
Ein Ausdruck vom Typ **sysname**, der den logischen Namen der Datei darstellt, deren Datei-ID-Wert `FILE_ID` zurückgegeben wird.  
  
## <a name="return-types"></a>Rückgabetypen  
**smallint**  
  
## <a name="remarks"></a>Hinweise  
*file_name* entspricht dem logischen Dateinamen, der in der Namensspalte in den Katalogsichten „sys.master_files“ oder „sys.database_files“ angezeigt wird.  

`FILE_ID` gibt `NULL` zurück, wenn *file_name* nicht mit dem logischen Namen einer Komponentendatei der aktuellen Datenbank übereinstimmt.
  
In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ist die Volltextkatalogen zugewiesene Datei-ID größer als 32767. Da die `FILE_ID`-Funktion den Rückgabetyp **smallint** aufweist, wird `FILE_ID` keine Volltextdateien unterstützen. Verwenden Sie stattdessen [FILE_IDEX](../../t-sql/functions/file-idex-transact-sql.md).  
  
## <a name="examples"></a>Beispiele  
Dieses Beispiel gibt den Datei-ID-Wert für die `AdventureWorks_Data`-Datei zurück, bei der es sich um eine Komponentendatei der `ADVENTUREWORKS2012`-Datenbank handelt.  

```sql  
USE AdventureWorks2012;  
GO  
SELECT FILE_ID('AdventureWorks2012_Data')AS 'File ID';  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
File ID   
-------   
1  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Als veraltet markierte Funktionen der Datenbank-Engine in SQL Server 2016](../../database-engine/deprecated-database-engine-features-in-sql-server-2016.md)   
 [FILE_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/file-name-transact-sql.md)   
 [Metadata Functions &#40;Transact-SQL&#41; (Metadatenfunktionen &#40;Transact-SQL&#41;)](../../t-sql/functions/metadata-functions-transact-sql.md)   
 [sys.database_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)   
 [sys.master_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)  
  
  
