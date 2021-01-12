---
description: Bitweise Operatoren (Transact-SQL)
title: Bitweise Operatoren (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/04/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- operators [Transact-SQL], bitwise
- bitwise operators
- bit manipulations
ms.assetid: 2b994cf5-2daa-438a-b8c7-4bd8d451ac8d
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e41fff219addb547d9706819a9fd5d9327f24e36
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100957"
---
# <a name="bitwise-operators-transact-sql"></a>Bitweise Operatoren (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Bitweise Operatoren bearbeiten Bits aus zwei Ausdrücken eines Datentyps der ganzzahligen Datentypkategorie.  
  Bitweise Operatoren konvertieren zwei ganzzahlige Werte in binäre Bitwerte und führen die Operationen AND, OR oder NOT für jedes Bit aus, wodurch ein Ergebnis ausgegeben wird. Dieses Ergebnis wird anschließend in einen Integer konvertiert.  
  
  Beispielsweise wird der Integer 170 in die Binärzahl 1010 1010 konvertiert.
Der Integer 75 wird in die Binärzahl 0100 1011 konvertiert.

|Operator|Bitweise Mathematik|
|---- |---- |
|UND <br> Wenn zwei Bits an einem beliebigen Speicherort 1 ergeben, wird auch 1 als Ergebnis ausgegeben. |1010 1010 = 170 <br>0100 1011 = 75 <br>-----------------  <br> 0000 1010 = 10 |
|oder <br> Wenn irgendein Bit an einem beliebigen Speicherort 1 ergibt, wird auch 1 als Ergebnis zurückgegeben. |1010 1010 = 170 <br>0100 1011 = 75 <br>-----------------  <br> 1110 1011 = 235|
|NICHT  <br> Kehrt den Bitwert an jedem Bitspeicherort um. |1010 1010 = 170 <br>----------------- <br>  0101 0101 = 85 |
  
Weitere Informationen finden Sie in den folgenden Artikeln:   
* [& &#40;Bitwise AND&#41; (Bitweises UND (Operator))](../../t-sql/language-elements/bitwise-and-transact-sql.md)  
* [&= &#40;Bitwise AND Assignment&#41; (Bitweises UND und Zuweisung)](../../t-sql/language-elements/bitwise-and-equals-transact-sql.md)   
* [&#124; &#40;Bitwise OR&#41; (Bitweiser OR-Operator)](../../t-sql/language-elements/bitwise-or-transact-sql.md)  
* [&#124;= &#40;Bitwise OR Assignment&#41; (Zuweisung von bitweisem OR)](../../t-sql/language-elements/bitwise-or-equals-transact-sql.md)   
* [^ &#40;Bitwise Exclusive OR&#41; (Bitweiser exklusiver OR-Operator)](../../t-sql/language-elements/bitwise-exclusive-or-transact-sql.md)  
* [^= &#40;Bitwise Exclusive OR Assignment&#41; (Zuweisung von bitweisem OR)](../../t-sql/language-elements/bitwise-exclusive-or-equals-transact-sql.md)  
* [~ &#40;Bitwise NOT&#41; (Bitweiser NOT-Operator)](../../t-sql/language-elements/bitwise-not-transact-sql.md)  
  
 Die Operanden für bitweise Operatoren können beliebige Datentypen aus den Datentypkategorien des Integers oder der binären Zeichenfolgen aufweisen (außer dem **image**-Datentyp). Ausnahme ist, dass keiner der Operanden aus der Datentypkategorie der binären Zeichenfolgen stammen können. In der folgenden Tabelle sind alle Datentypen aufgeführt, die für Operanden unterstützt werden.  
  
|Linker Operand|Rechter Operand|  
|------------------|-------------------|  
|[binary](../../t-sql/data-types/binary-and-varbinary-transact-sql.md)|**int**, **smallint** oder **tinyint**|  
|[bit](../../t-sql/data-types/bit-transact-sql.md)|**int**, **smallint**, **tinyint** oder **bit**|  
|[bigint](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md)|**bigint**, **int**, **smallint**, **tinyint**, **binary** oder **varbinary**|  
|[int](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md)|**int**, **smallint**, **tinyint**, **binary** oder **varbinary**|  
|[smallint](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md)|**int**, **smallint**, **tinyint**, **binary** oder **varbinary**|  
|[tinyint](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md)|**int**, **smallint**, **tinyint**, **binary** oder **varbinary**|  
|[varbinary](../../t-sql/data-types/binary-and-varbinary-transact-sql.md)|**int**, **smallint** oder **tinyint**|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Operatoren &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)   
 [Datentypen &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [Verbundoperatoren &#40;Transact-SQL&#41;](../../t-sql/language-elements/compound-operators-transact-sql.md)
