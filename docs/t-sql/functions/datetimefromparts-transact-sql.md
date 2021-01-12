---
description: DATETIMEFROMPARTS (Transact-SQL)
title: DATETIMEFROMPARTS (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 07/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DATETIMEFROMPARTS_TSQL
- DATETIMEFROMPARTS
dev_langs:
- TSQL
helpviewer_keywords:
- DATETIMEFROMPARTS function
ms.assetid: 6008148b-bf75-4c98-9392-68a89fa0711c
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f86250de50575a37320684b237867c517991b0f7
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101234"
---
# <a name="datetimefromparts-transact-sql"></a>DATETIMEFROMPARTS (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Diese Funktion gibt einen **datetime**-Wert für die angegebenen Argumente für Datum und Zeit zurück.
  
![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
DATETIMEFROMPARTS ( year, month, day, hour, minute, seconds, milliseconds )  
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
Ein ganzzahliger Ausdruck, der Stunden angibt.
  
*minute*  
Ein ganzzahliger Ausdruck, der Minuten angibt.
  
*Sekunden*  
Ein ganzzahliger Ausdruck, der Sekunden angibt.
  
*milliseconds*  
Ein ganzzahliger Ausdruck, der Millisekunden angibt.
  
## <a name="return-types"></a>Rückgabetypen
**datetime**
  
## <a name="remarks"></a>Bemerkungen  
`DATETIMEFROMPARTS` gibt einen vollständig initialisierten **datetime**-Wert zurück. `DATETIMEFROMPARTS` löst einen Fehler aus, wenn mindestens ein erforderliches Argument über einen ungültigen Wert verfügt. `DATETIMEFROMPARTS` gibt NULL zurück, wenn mindestens ein erforderliches Argument den Wert NULL enthält.
  
Diese Funktion unterstützt das Remoting zu [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]-Servern und höher. Sie unterstützt nicht das Remoting zu Servern mit einer Version unter [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].
  
## <a name="examples"></a>Beispiele  
  
```sql
SELECT DATETIMEFROMPARTS ( 2010, 12, 31, 23, 59, 59, 0 ) AS Result;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
Result  
---------------------------  
2010-12-31 23:59:59.000  
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>Weitere Informationen
[datetime &#40;Transact-SQL&#41;](../../t-sql/data-types/datetime-transact-sql.md)
  
  

