---
description: DATETIMEOFFSETFROMPARTS (Transact-SQL)
title: DATETIMEOFFSETFROMPARTS (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 07/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DATETIMEOFFSETFROMPARTS_TSQL
- DATETIMEOFFSETFROMPARTS
dev_langs:
- TSQL
helpviewer_keywords:
- DATETIMEOFFSETFROMPARTS function
ms.assetid: 463da1f4-b4b6-45a3-9a95-ea1f99575542
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ab37520d60c482afccb40bde7392de2515a79753
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101191"
---
# <a name="datetimeoffsetfromparts-transact-sql"></a>DATETIMEOFFSETFROMPARTS (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Gibt einen **datetimeoffset**-Wert für die angegebenen Argumente für Datum und Zeit zurück. Die Genauigkeit des Rückgabewerts wird vom precision-Argument angegeben, und Offsets werden von den Offsetargumenten bestimmt.  
  
![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
DATETIMEOFFSETFROMPARTS ( year, month, day, hour, minute, seconds, fractions, hour_offset, minute_offset, precision )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

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
  
*hour_offset*  
Ein ganzzahliger Ausdruck, der den Stundenteil des Zeitzonenoffsets angibt.  
  
*minute_offset*  
Ein ganzzahliger Ausdruck, der den Minutenteil des Zeitzonenoffsets angibt.  
  
*precision*  
Ein ganzzahliger literaler Wert, der die Genauigkeit des **datetimeoffset**-Werts angibt, der von `DATETIMEOFFSETFROMPARTS` zurückgegeben wird.  
  
## <a name="return-types"></a>Rückgabetypen
**datetimeoffset(** *Genauigkeit* **)**  
  
## <a name="remarks"></a>Bemerkungen  

`DATETIMEOFFSETFROMPARTS` gibt einen vollständig initialisierten **datetimeoffset**-Datentyp zurück. Die Offsetargumente stellen den Zeitzonenoffset dar. Bei ausgelassenen Offsetargumenten geht `DATETIMEOFFSETFROMPARTS` von einem Zeitzonenoffset von `00:00` (also kein Zeitzonenoffset) aus. Bei angegebenen Offsetargumenten erwartet `DATETIMEOFFSETFROMPARTS` Werte für beide Argumente, und beide Werte müssen entweder positiv oder negativ sein. Wenn *minute_offset* einen Wert und *hour_offset* keinen Wert aufweist, löst `DATETIMEOFFSETFROMPARTS` einen Fehler aus. `DATETIMEOFFSETFROMPARTS` löst einen Fehler aus, wenn andere Argumente über ungültige Werte verfügen. Wenn mindestens ein erforderliches Argument über einen `NULL`-Wert verfügt, dann gibt `DATETIMEOFFSETFROMPARTS``NULL` zurück. Wenn das *precision*-Argument jedoch einen `NULL`-Wert enthält, löst `DATETIMEOFFSETFROMPARTS` einen Fehler aus.  
  
Das *fractions*-Argument ist vom precision-Argument abhängig. Wenn „precision“ beispielsweise den Wert 7 aufweist, stellt jeder Bruchteil 100 Nanosekunden dar. Wenn „precision“ jedoch den Wert 3 aufweist, stellt jeder Bruchteil eine Millisekunde dar. Wenn der Wert von „precision“ 0 (null) ist, muss auch der Wert von „fractions“ 0 (null) sein; andernfalls löst `DATETIMEOFFSETFROMPARTS` einen Fehler aus.  
  
Diese Funktion unterstützt das Remoting zu [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]-Servern und höher. Sie unterstützt nicht das Remoting zu Servern mit einer Version unter [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-an-example-without-fractions-of-a-second"></a>A. Ein Beispiel ohne Sekundenbruchteile  
  
```sql
SELECT DATETIMEOFFSETFROMPARTS ( 2010, 12, 31, 14, 23, 23, 0, 12, 0, 7 ) AS Result;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
Result  
----------------------------------
2010-12-31 14:23:23.0000000 +12:00  
  
(1 row(s) affected)  
```  
  
### <a name="b-example-with-fractions-of-a-second"></a>B. Beispiel mit Sekundenbruchteilen  

Dieses Beispiel zeigt die Verwendung der Parameter *fractions* und *precision*:  

1. Wenn *fractions* den Wert 5 und *precision* den Wert 1 hat, stellt der Wert von *fractions* 5/10 einer Sekunde dar.  

2. Wenn *fractions* den Wert 50 und *precision* den Wert 2 hat, stellt der Wert von *fractions* 50/100 einer Sekunde dar.  

3. Wenn *fractions* den Wert 500 und *precision* den Wert 3 hat, stellt der Wert von *fractions* 500/1000 einer Sekunde dar.  
  
```sql
SELECT DATETIMEOFFSETFROMPARTS ( 2011, 8, 15, 14, 30, 00, 5, 12, 30, 1 );  
SELECT DATETIMEOFFSETFROMPARTS ( 2011, 8, 15, 14, 30, 00, 50, 12, 30, 2 );  
SELECT DATETIMEOFFSETFROMPARTS ( 2011, 8, 15, 14, 30, 00, 500, 12, 30, 3 );  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
----------------------------------  
2011-08-15 14:30:00.5 +12:30  
  
(1 row(s) affected)  
  
----------------------------------  
2011-08-15 14:30:00.50 +12:30  
  
(1 row(s) affected)  
  
----------------------------------  
2011-08-15 14:30:00.500 +12:30  
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>Weitere Informationen
[datetimeoffset &#40;Transact-SQL&#41;](../../t-sql/data-types/datetimeoffset-transact-sql.md)  
[AT TIME ZONE &#40;Transact-SQL&#41;](../../t-sql/queries/at-time-zone-transact-sql.md)
  
  


