---
description: BINARY_CHECKSUM (Transact-SQL)
title: BINARY_CHECKSUM (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: sql-data-warehouse, database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- BINARY_CHECKSUM
- BINARY_CHECKSUM_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- BINARY_CHECKSUM function
- binary [SQL Server], checksum values
ms.assetid: 07fece4d-58e3-446e-a3b5-92fe24d2d1fb
author: cawrites
ms.author: chadam
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 33dbfaeaeef8e6505ee5c90e1a0b4712b0895669
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98088523"
---
# <a name="binary_checksum--transact-sql"></a>BINARY_CHECKSUM (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

Gibt den binären Prüfsummenwert zurück, der für eine Zeile einer Tabelle oder eine Liste von Ausdrücken berechnet wurde.
  
![Artikellinksymbol](../../database-engine/configure-windows/media/topic-link.gif "Artikellinksymbol") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
BINARY_CHECKSUM ( * | expression [ ,...n ] )   
```  
  
[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
*\**  
Gibt an, dass die Berechnung für alle Spalten der Tabelle erfolgt. BINARY_CHECKSUM ignoriert in seiner Berechnung jede Spalte, die einen nicht vergleichbaren Datentyp hat. Die folgenden Datentypen sind z.B. nicht vergleichbar:  
* **Cursor**  
* **image**  
* **ntext**  
* **text**  
* **xml**  

Außerdem gehören auch benutzerdefinierte CLR-Typen (Common Language Runtime) zu den nicht vergleichbaren Datentypen.
  
*expression*  
Ein [Ausdruck](../../t-sql/language-elements/expressions-transact-sql.md) beliebigen Typs. BINARY_CHECKSUM ignoriert in seiner Berechnung jeden Ausdruck, der einen nicht vergleichbaren Datentyp hat.

## <a name="return-types"></a>Rückgabetypen  
 **int**
  
## <a name="remarks"></a>Bemerkungen  
Wird `BINARY_CHECKSUM(*)` für eine beliebige Zeile einer Tabelle berechnet, gibt die Anweisung den gleichen Wert zurück, solange die Zeile nicht später geändert wird. `BINARY_CHECKSUM` erfüllt die Eigenschaften einer Hashfunktion: Wenn es auf zwei beliebige Listen mit Ausdrücken angewendet wird, wird immer derselbe Wert zurückgegeben, falls die entsprechenden Elemente der beiden Listen vom gleichen Typ sind und bezüglich des Vergleichs mit dem Gleichheitsoperator (=) gleich sind. Bei dieser Definition werden die NULL-Werte eines angegebenen Typs bei einem Vergleich als gleiche Werte angesehen. Wenn sich einer der Werte in der Liste mit Ausdrücken ändert, ändert sich gewöhnlich auch die Prüfsumme des Ausdrucks. Diese Änderung ist jedoch nicht gewährleistet. Daher wird empfohlen, `BINARY_CHECKSUM` nur zur Ermittlung von Änderungen der Daten zu verwenden, wenn Ihre Anwendung auch bei gelegentlich nicht ausgeführten Änderungen weiter ausgeführt werden kann. Sie sollten andernfalls in Betracht ziehen, stattdessen `HASHBYTES` zu verwenden. Ist ein MD5-Hashalgorithmus angegeben, ist die Wahrscheinlichkeit, dass `HASHBYTES` für zwei verschiedene Eingaben dasselbe Ergebnis zurückgibt, wesentlich geringer als bei der Verwendung von `BINARY_CHECKSUM`.
  
`BINARY_CHECKSUM` kann auf eine Liste von Ausdrücken angewendet werden und gibt den gleichen Wert für eine angegebene Liste zurück. Wenn `BINARY_CHECKSUM` auf zwei beliebige Listen von Ausdrücken angewendet wird, wird stets der gleiche Wert zurückgegeben, wenn die entsprechenden Elemente der beiden Listen denselben Typ und dieselbe Bytedarstellung haben. Für diese Definition wird bei NULL-Werten eines angegebenen Datentyps angenommen, dass sie dieselbe Bytedarstellung haben.
  
`BINARY_CHECKSUM` und `CHECKSUM` sind ähnliche Funktionen. Sie können zur Berechnung eines Prüfsummenwerts für eine Liste mit Ausdrücken verwendet werden, wobei der berechnete Wert von der Reihenfolge der Ausdrücke abhängt. Die Spaltenreihenfolge, die bei `BINARY_CHECKSUM(*)` verwendet wird, ist die Spaltenreihenfolge, die in der Tabellen- oder Sichtdefinition angegeben ist. Diese Reihenfolge die berechneten Spalten ein.
  
`BINARY_CHECKSUM` und `CHECKSUM` geben unterschiedliche Werte für die String-Datentypen zurück, bei denen aufgrund des Gebietsschemas unterschiedliche Repräsentationen als gleich betrachtet werden können. String-Datentypen sind z.B.:  

* **char**  
* **nchar**  
* **nvarchar**  
* **varchar**  

oder  

* **sql_variant** (wenn der Basistyp von **sql_variant** ein String-Datentyp ist).  
  
Die `BINARY_CHECKSUM`-Werte für die Zeichenfolgen „McCavity“ und „Mccavity“ sind z. B. unterschiedlich. Bei einem Server ohne Unterscheidung nach Groß-/Kleinschreibung gibt `CHECKSUM` jedoch für diese Zeichenfolgen dieselben Prüfsummenwerte zurück. Sie sollten Vergleiche zwischen `CHECKSUM`-Werten und `BINARY_CHECKSUM`-Werten vermeiden.
 
`BINARY_CHECKSUM` unterstützt für den Typ **varbinary(max)** beliebige Längen und bis zu 255 Zeichen für den Typ **nvarchar(max)**.
  
## <a name="examples"></a>Beispiele  
Im folgenden Beispiel wird `BINARY_CHECKSUM` verwendet, um Änderungen in einer Zeile einer Tabelle zu erkennen.
  
```sql
USE AdventureWorks2012;  
GO  
CREATE TABLE myTable (column1 INT, column2 VARCHAR(256));  
GO  
INSERT INTO myTable VALUES (1, 'test');  
GO  
SELECT BINARY_CHECKSUM(*) from myTable;  
GO  
UPDATE myTable set column2 = 'TEST';  
GO  
SELECT BINARY_CHECKSUM(*) from myTable;  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen
[Aggregate Functions &#40;Transact-SQL&#41; (Aggregatfunktionen (Transact-SQL))](../../t-sql/functions/aggregate-functions-transact-sql.md)  
[CHECKSUM_AGG &#40;Transact-SQL&#41;](../../t-sql/functions/checksum-agg-transact-sql.md)  
[CHECKSUM &#40;Transact-SQL&#41;](../../t-sql/functions/checksum-transact-sql.md)  
[HASHBYTES &#40;Transact-SQL&#41;](../../t-sql/functions/hashbytes-transact-sql.md)  
  
  
