---
description: DECOMPRESS (Transact-SQL)
title: DECOMPRESS (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 10/11/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DECOMPRESS
- DECOMPRESS_TSQL
helpviewer_keywords:
- DECOMPRESS function
ms.assetid: 738d56be-3870-4774-b112-3dce27becc11
author: cawrites
ms.author: chadam
ms.openlocfilehash: 35d503318f71750691d8dccabfc60284f57f51ee
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101160"
---
# <a name="decompress-transact-sql"></a>DECOMPRESS (Transact-SQL)
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

Diese Funktion dekomprimiert einen Eingabeausdruckswert mit dem GZIP-Algorithmus. `DECOMPRESS` gibt ein Bytearray zurück (Typ VARBINARY(MAX)).  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql  
DECOMPRESS ( expression )  
```  
  
## <a name="arguments"></a>Argumente
 *expression*  
Ein Wert vom Typ **varbinary(** _n_ **)** , **varbinary(max)** oder **binary(** _n_ **)** . Weitere Informationen finden Sie unter [Ausdrücke &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md).  
  
## <a name="return-types"></a>Rückgabetypen  
Ein Wert vom Datentyp **varbinary(max)**. `DECOMPRESS` verwendet den ZIP-Algorithmus, um das Eingabeargument zu dekomprimieren. Der Benutzer sollte das Ergebnis wenn nötig explizit in einen Zieltyp umwandeln.  
  
## <a name="remarks"></a>Bemerkungen  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-decompress-data-at-query-time"></a>A. Dekomprimieren von Daten zur Abfragezeit  
In diesem Beispiel wird gezeigt, wie Sie komprimierte Tabellendaten zurückgegeben:  
  
```sql  
SELECT _id, name, surname, datemodified,  
             CAST(DECOMPRESS(info) AS NVARCHAR(MAX)) AS info  
FROM player;  
```  
  
### <a name="b-display-compressed-data-using-computed-column"></a>B. Anzeigen von komprimierten Daten unter Verwendung einer berechneten Spalte  
In diesem Beispiel wird gezeigt, wie Sie eine Tabelle erstellen, in der dekomprimierte Daten gespeichert werden können:  
  
```sql  
CREATE TABLE example_table (  
    _id INT PRIMARY KEY IDENTITY,  
    name NVARCHAR(max),  
    surname NVARCHAR(max),  
    info VARBINARY(max),  
    info_json as CAST(DECOMPRESS(info) as NVARCHAR(max))  
);  
```  
  
## <a name="see-also"></a>Siehe auch  
 [String Functions &#40;Transact-SQL&#41; (Zeichenfolgenfunktionen (Transact-SQL))](../../t-sql/functions/string-functions-transact-sql.md)   
 [COMPRESS &#40;Transact-SQL&#41;](../../t-sql/functions/compress-transact-sql.md)  
  
  
