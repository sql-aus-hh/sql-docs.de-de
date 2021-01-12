---
description: SET STATISTICS IO (Transact-SQL)
title: SET STATISTICS IO (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 11/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- SET_STATISTICS_IO_TSQL
- IO
- IO_TSQL
- SET STATISTICS IO
dev_langs:
- TSQL
helpviewer_keywords:
- disk I/O statistics [SQL Server]
- I/O [SQL Server], disk activity information
- disks [SQL Server], statement statistics
- STATISTICS IO option
- statements [SQL Server], statistical information
- SET STATISTICS IO statement
- statistical information [SQL Server], disk activity
ms.assetid: 7033aac9-a944-4156-9ff4-6ef65717a28b
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 7b1f3fac5ab29e44dd3150cba5882cd1dfc4e271
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98079560"
---
# <a name="set-statistics-io-transact-sql"></a>SET STATISTICS IO (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Bewirkt, dass [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Informationen zum Umfang der Datenträgeraktivitäten anzeigt, die durch [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisungen generiert werden.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
  
SET STATISTICS IO { ON | OFF }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Bemerkungen
 Wenn STATISTICS IO auf ON festgelegt ist, werden statistische Informationen angezeigt. Bei OFF werden die Informationen nicht angezeigt.   
  
 Wenn diese Option auf ON festgelegt wird, geben alle [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisungen so lange statistische Informationen zurück, bis die Option auf OFF festgelegt wird.  
  
 Die folgende Tabelle enthält eine Auflistung der Ausgabeelemente sowie entsprechende Beschreibungen.  
  
|Ausgabeelement|Bedeutung|  
|-----------------|-------------|  
|**Table**|Der Name der Tabelle.|  
|**Scananzahl**|Die Anzahl von Suchen oder Scans, die nach Erreichen der Blattebene in beliebiger Richtung gestartet wurden, um alle Werte zum Erstellen des letzten Datasets für die Ausgabe abzurufen.<br /><br /> Die Scananzahl beträgt „0“ (null), wenn der verwendete Index ein eindeutiger Index oder ein gruppierter Index für eine Primärschlüsselspalte ist und Sie nur einen Wert suchen. Beispiel: `WHERE Primary_Key_Column = <value>`.<br /><br /> Die Scananzahl beträgt „1“, wenn Sie anhand eines nicht eindeutig gruppierten Index, der für eine Nicht-Primärschlüsselspalte definiert ist, nach einem Wert suchen. Auf diese Weise wird nach doppelten Werten eines Schlüsselwerts gesucht, der als Suchwert verwendet wird. Beispiel: `WHERE Clustered_Index_Key_Column = <value>`.<br /><br /> Die Scananzahl ist „N“, wenn „N“ der Anzahl unterschiedlicher Suchen oder Scans entspricht, die auf der Blattebene nach links oder rechts gestartet wurden, nachdem ein Schlüsselwert anhand des Indexschlüssels ermittelt wurde.|  
|**Logische Lesevorgänge**|Anzahl der aus dem Datencache gelesenen Seiten|  
|**Physische Lesevorgänge**|Anzahl der vom Datenträger gelesenen Seiten|  
|**Read-Ahead-Lesevorgänge**|Anzahl der Seiten, die für die Abfrage im Cache platziert wurden|  
|**Logische LOB-Lesevorgänge**|Anzahl der aus dem Datencache gelesenen Seiten Umfasst **text**, **ntext**, **image**, **varchar(max)**, **nvarchar(max)**, **varbinary(max)** oder Columnstore-Indexseiten.|  
|**Physische LOB-Lesevorgänge**|Anzahl der vom Datenträger gelesenen Seiten Umfasst **text**, **ntext**, **image**, **varchar(max)**, **nvarchar(max)**, **varbinary(max)** oder Columnstore-Indexseiten.|  
|**Read-Ahead-LOB-Lesevorgänge**|Anzahl der Seiten, die für die Abfrage im Cache platziert wurden Umfasst **text**, **ntext**, **image**, **varchar(max)**, **nvarchar(max)**, **varbinary(max)** oder Columnstore-Indexseiten.|

 Die Einstellung von SET STATISTICS IO wird zur Ausführungszeit und nicht zur Analysezeit festgelegt.

> [!NOTE]  
> Wenn Transact-SQL-Anweisungen LOB-Spalten abrufen, kann es vorkommen, dass bestimmte LOB-Abrufvorgänge die LOB-Struktur mehrere Male durchlaufen müssen. Dadurch meldet SET STATISTICS IO möglicherweise mehr logische Lesevorgänge als erwartet.

## <a name="permissions"></a>Berechtigungen  
 Für die Verwendung von SET STATISTICS IO müssen die Benutzer über die geeigneten Berechtigungen zum Ausführen der [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisung verfügen. Die SHOWPLAN-Berechtigung ist nicht erforderlich.  
  
## <a name="examples"></a>Beispiele  
 In diesem Beispiel wird gezeigt, wie viele logische und physische Lesevorgänge [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] während der Verarbeitung der Anweisungen verwendet.  
  
```sql
USE AdventureWorks2012;  
GO         
SET STATISTICS IO ON;  
GO  
SELECT *   
FROM Production.ProductCostHistory  
WHERE StandardCost < 500.00;  
GO  
SET STATISTICS IO OFF;  
GO  
```  
  
 Im Folgenden wird das Resultset aufgeführt:  
  
```  
Table 'ProductCostHistory'. Scan count 1, logical reads 5, physical   
reads 0, read-ahead reads 0, lob logical reads 0, lob physical reads 0,   
lob read-ahead reads 0.  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [SET-Anweisungen (Transact-SQL)](../../t-sql/statements/set-statements-transact-sql.md)   
 [SET SHOWPLAN_ALL &#40;Transact-SQL&#41;](../../t-sql/statements/set-showplan-all-transact-sql.md)   
 [SET STATISTICS TIME &#40;Transact-SQL&#41;](../../t-sql/statements/set-statistics-time-transact-sql.md)  
  
  
