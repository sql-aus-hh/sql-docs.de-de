---
title: DATETIME2FROMPARTS (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 07/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DATETIME2FROMPARTS_TSQL
- DATETIME2FROMPARTS
dev_langs:
- TSQL
helpviewer_keywords:
- DATETIME2FROMPARTS function
ms.assetid: 632b757d-d2d1-43a5-b870-792a779ae204
author: MashaMSFT
ms.author: mathoma
manager: craigg
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d923e65f55edc56b2201d455567c924619dc0ad3
ms.sourcegitcommit: 61381ef939415fe019285def9450d7583df1fed0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2018
ms.locfileid: "47713538"
---
# <a name="datetime2fromparts-transact-sql"></a>DATETIME2FROMPARTS (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2012-all-md](../../includes/tsql-appliesto-ss2012-all-md.md)]

Diese Funktion gibt einen **datetime2**-Wert für die angegebenen Argumente für Datum und Zeit zurück. Der zurückgegebene Wert verfügt über eine Genauigkeit, die durch das precision-Argument angegeben wird.
  
![Themenlinksymbol](../../database-engine/configure-windows/media/topic-link.gif "Topic link icon") [Transact-SQL Syntax Conventions (Transact-SQL-Syntaxkonventionen)](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntax  
  
```sql
DATETIME2FROMPARTS ( year, month, day, hour, minute, seconds, fractions, precision )  
```  
  
## <a name="arguments"></a>Argumente  
*year*  
Ein ganzzahliger Ausdruck, der ein Jahr angibt.
  
*month*  
Ein ganzzahliger Ausdruck, der einen Monat angibt.
  
*day*  
Ein ganzzahliger Ausdruck, der einen Tag angibt.
  
*hour*  
Ein ganzzahliger Ausdruck, der die Stunden angibt.
  
*minute*  
Ein ganzzahliger Ausdruck, der die Minuten angibt.
  
*Sekunden*  
Ein ganzzahliger Ausdruck, der die Sekunden angibt.
  
*fractions*  
Ein ganzzahliger Ausdruck, der einen Wert für Sekundenbruchteile angibt.
  
*precision*  
Ein ganzzahliger Ausdruck, der die Genauigkeit des **datetime2**-Werts angibt, der von `DATETIME2FROMPARTS` zurückgegeben wird.
  
## <a name="return-types"></a>Rückgabetypen
**datetime2(** *precision* **)**
  
## <a name="remarks"></a>Remarks  
`DATETIME2FROMPARTS` gibt einen vollständig initialisierten **datetime2**-Wert zurück. `DATETIME2FROMPARTS` löst einen Fehler aus, wenn mindestens ein erforderliches Argument über einen ungültigen Wert verfügt. `DATETIME2FROMPARTS` gibt NULL zurück, wenn mindestens ein erforderliches Argument den Wert NULL enthält. Wenn das *precision*-Argument jedoch einen NULL-Wert enthält, löst `DATETIME2FROMPARTS` einen Fehler aus.

Das *fractions*-Argument ist vom *precision*-Argument abhängig. Wenn *precision* beispielsweise den Wert 7 aufweist, stellt jeder Bruchteil 100 Nanosekunden dar. Wenn *precision* jedoch den Wert 3 aufweist, stellt jeder Bruchteil eine Millisekunde dar. Wenn der Wert von *precision* 0 (null) ist, muss auch der Wert von *fractions* 0 (null) sein; andernfalls löst `DATETIME2FROMPARTS` einen Fehler aus.
  
Diese Funktion unterstützt das Remoting zu [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]-Servern und höher. Sie unterstützt nicht das Remoting zu Servern mit einer Version unter [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-an-example-without-fractions-of-a-second"></a>A. Ein Beispiel ohne Sekundenbruchteile  
  
```sql
SELECT DATETIME2FROMPARTS ( 2010, 12, 31, 23, 59, 59, 0, 0 ) AS Result;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```sql
Result  
---------------------------  
2010-12-31 23:59:59.0000000  
  
(1 row(s) affected)  
```  
  
### <a name="b-example-with-fractions-of-a-second"></a>B. Beispiel mit Sekundenbruchteilen  
Dieses Beispiel zeigt die Verwendung der Parameter *fractions* und *precision*:
  
1.  Wenn *fractions* den Wert 5 und *precision* den Wert 1 hat, stellt der Wert von *fractions* 5/10 einer Sekunde dar.  
  
2.  Wenn *fractions* den Wert 50 und *precision* den Wert 2 hat, stellt der Wert von *fractions* 50/100 einer Sekunde dar.  
  
3.  Wenn *fractions* den Wert 500 und *precision* den Wert 3 hat, stellt der Wert von *fractions* 500/1000 einer Sekunde dar.  
  
```sql
SELECT DATETIME2FROMPARTS ( 2011, 8, 15, 14, 23, 44, 5, 1 );  
SELECT DATETIME2FROMPARTS ( 2011, 8, 15, 14, 23, 44, 50, 2 );  
SELECT DATETIME2FROMPARTS ( 2011, 8, 15, 14, 23, 44, 500, 3 );  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```sql
----------------------  
2011-08-15 14:23:44.5  
  
(1 row(s) affected)  
  
----------------------  
2011-08-15 14:23:44.50  
  
(1 row(s) affected)  
  
----------------------  
2011-08-15 14:23:44.500  
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>Siehe auch
[datetime2 &#40;Transact-SQL&#41;](../../t-sql/data-types/datetime2-transact-sql.md)
  
  

