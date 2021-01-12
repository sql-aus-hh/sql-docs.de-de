---
description: Ausdrücke (Transact-SQL)
title: Ausdrücke (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- Boolean expressions
- expressions [SQL Server], about expressions
- combining expressions
- Transact-SQL expressions
- expressions [SQL Server], combining
- simple expressions [SQL Server]
- complex expressions [SQL Server]
ms.assetid: ee53c5c8-e36c-40f9-8cd1-d933791b98fa
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6db31166cf0931e7ace6a6f1366fed9776509568
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100380"
---
# <a name="expressions-transact-sql"></a>Ausdrücke (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Eine Kombination aus Symbolen und Operatoren, die [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] auswertet, um einen einzelnen Datenwert zu erhalten. Einfache Ausdrücke können aus einzelnen Konstanten, Variablen, Spalten oder Skalarfunktionen bestehen. Mithilfe von Operatoren können zwei oder mehrere einfache Ausdrücke zu einem komplexen Ausdruck verknüpft werden.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
{ constant | scalar_function | [ table_name. ] column | variable   
    | ( expression ) | ( scalar_subquery )   
    | { unary_operator } expression   
    | expression { binary_operator } expression   
    | ranking_windowed_function | aggregate_windowed_function  
}  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  

-- Expression in a SELECT statement  
<expression> ::=   
{  
    constant   
    | scalar_function   
    | column  
    | variable  
    | ( expression  )  
    | { unary_operator } expression   
    | expression { binary_operator } expression   
}  
[ COLLATE Windows_collation_name ]  
  
-- Scalar Expression in a DECLARE, SET, IF...ELSE, or WHILE statement  
<scalar_expression> ::=  
{  
    constant   
    | scalar_function   
    | variable  
    | ( expression  )  
    | (scalar_subquery )  
    | { unary_operator } expression   
    | expression { binary_operator } expression   
}  
[ COLLATE { Windows_collation_name ]  
  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
  
|Begriff|Definition|  
|----------|----------------|  
|*constant*|Ein Symbol, das einen einzelnen bestimmten Datenwert darstellt. Weitere Informationen finden Sie unter [Konstanten &#40;Transact-SQL&#41;](../../t-sql/data-types/constants-transact-sql.md).|  
|*scalar_function*|Eine Einheit der [!INCLUDE[tsql](../../includes/tsql-md.md)]-Syntax, die einen bestimmten Dienst bereitstellt und einen einzelnen Wert zurückgibt. Bei *scalar_function* kann es sich um integrierte Skalarfunktionen wie die SUM-, GETDATE- oder CAST-Funktionen oder um benutzerdefinierte Skalarfunktionen handeln.|  
|[ _table_name_ **.** ]|Der Name oder Alias einer Tabelle.|  
|*column*|Der Name einer Spalte. Nur der Name der Spalte ist in einem Ausdruck zulässig.|  
|*variable*|Der Name einer Variablen oder eines Parameters. Weitere Informationen finden Sie unter [DECLARE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md).|  
| **(** _Ausdruck_  **)**|Ein beliebiger gültiger Ausdruck, wie er in diesem Thema definiert ist. Die Klammern sind gruppierende Operatoren, die sicherstellen, dass alle Operatoren in dem in Klammern stehenden Ausdruck ausgewertet werden, bevor der resultierende Ausdruck mit einem weiteren Ausdruck kombiniert wird.|  
|**(** _skalare Unterabfrage_ **)**|Eine Unterabfrage, die einen Wert zurückgibt. Beispiel:<br /><br /> `SELECT MAX(UnitPrice)`<br /><br /> `FROM Products`|  
|{ *unary_operator* }|Unäre Operatoren können nur auf Ausdrücke angewendet werden, die zu einem der Datentypen in der numerischen Datentypkategorie ausgewertet werden. Ein Operator, der nur einen numerischen Operanden besitzt:<br /><br /> + zeigt eine positive Zahl an.<br /><br /> - zeigt eine negative Zahl an.<br /><br /> ~ zeigt den Einserkomplementoperator an.|  
|{ *binary_operator* }|Ein Operator, der die Art und Weise definiert, in der zwei Ausdrücke kombiniert werden, um ein einzelnes Resultat zu liefern. *binary_operator* kann ein arithmetischer Operator, der Zuweisungsoperator (=), ein bitweiser Operator, ein Vergleichsoperator, ein logischer Operator, der Operator für Zeichenfolgenverkettung (+) oder ein unärer Operator sein. Weitere Informationen zu Operatoren finden Sie unter [Operatoren &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md).|  
|*ranking_windowed_function*|Eine beliebige [!INCLUDE[tsql](../../includes/tsql-md.md)]-Rangfolgefunktion. Weitere Informationen finden Sie unter [Rangfolgefunktionen &#40;Transact-SQL&#41;](../../t-sql/functions/ranking-functions-transact-sql.md).|  
|*aggregate_windowed_function*|Eine beliebige [!INCLUDE[tsql](../../includes/tsql-md.md)]-Aggregatfunktion mit der OVER-Klausel. Weitere Informationen finden Sie unter [OVER-Klausel &#40;Transact-SQL&#41;](../../t-sql/queries/select-over-clause-transact-sql.md).|  
  
## <a name="expression-results"></a>Ergebnisse von Ausdrücken  
 Bei einfachen Ausdrücken, die aus einer einzelnen Konstante, Variablen, Skalarfunktion oder einem Spaltennamen bestehen, entsprechen Datentyp, Sortierung, Genauigkeit, Anzahl der Dezimalstellen und Wert des Ausdrucks den jeweiligen Eigenschaften (Datentyp, Sortierung, Genauigkeit usw.) des Elements, auf das verwiesen wird.  
  
 Werden zwei Ausdrücke mithilfe von Vergleichs- oder logischen Operatoren kombiniert, ist das Ergebnis vom booleschen Datentyp. Das Ergebnis nimmt einen der folgenden Werte an: TRUE, FALSE oder UNKNOWN. Weitere Informationen zu booleschen Datentypen finden Sie unter [Vergleichsoperatoren &#40;Transact-SQL&#41;](../../t-sql/language-elements/comparison-operators-transact-sql.md).  
  
 Werden zwei Ausdrücke mithilfe von arithmetischen Operatoren, bitweisen Operatoren oder Zeichenfolgenoperatoren kombiniert, bestimmt der Operator den resultierenden Datentyp.  
  
 Komplexe Ausdrücke, die aus vielen Symbolen und Operatoren bestehen, werden zu einem Ergebnis mit genau einem Wert ausgewertet. Der Datentyp, die Sortierung, die Genauigkeit und der Wert des resultierenden Ausdrucks werden bestimmt, indem immer jeweils zwei Teilausdrücke kombiniert und ausgewertet werden, bis ein Endergebnis erreicht ist. Die Reihenfolge, in der die Ausdrücke kombiniert werden, ist durch die Rangfolge der Operatoren im Ausdruck definiert.  
  
## <a name="remarks"></a>Hinweise  
 Zwei Ausdrücke können mit einem Operator kombiniert werden, wenn die Datentypen beider Ausdrücke vom Operator unterstützt werden und mindestens eine der folgenden Bedingungen erfüllt ist:  
  
-   Die Ausdrücke besitzen den gleichen Datentyp.  
  
-   Der Datentyp niedrigerer Rangfolge kann implizit in den Datentyp höherer Rangfolge konvertiert werden.  
  
 Falls die Ausdrücke diese Bedingungen nicht erfüllen, kann mit den Funktionen CAST oder CONVERT der Datentyp niedrigerer Rangfolge explizit in den Datentyp höherer Rangfolge konvertiert werden oder in einen dazwischen liegenden Datentyp, der implizit in den Datentyp höherer Rangfolge konvertiert werden kann.  
  
 Falls weder die implizite noch die explizite Konvertierung unterstützt wird, können die beiden Ausdrücke nicht kombiniert werden.  
  
 Die Sortierung eines Ausdrucks, der zu einer Zeichenfolge ausgewertet wird, wird entsprechend den Regeln zur Sortierungsrangfolge festgelegt. Weitere Informationen finden Sie unter [Rangfolge von Sortierungen &#40;Transact-SQL&#41;](../../t-sql/statements/collation-precedence-transact-sql.md).  
  
 In einer Programmiersprache wie C oder [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] wird ein Ausdruck immer zu einem einzelnen Ergebnis ausgewertet. Für Ausdrücke in einer [!INCLUDE[tsql](../../includes/tsql-md.md)]-Auswahlliste gilt eine Abwandlung dieser Regel: Der Ausdruck wird für jede Zeile des Resultsets einzeln ausgewertet. Ein einzelner Ausdruck kann für jede Zeile im Resultset einen anderen Wert annehmen, aber jede Zeile hat nur genau einen Wert für den Ausdruck. In der folgenden `SELECT`-Anweisung sind beispielsweise sowohl der Verweis auf `ProductID` als auch der Term `1+2` in der Auswahlliste Ausdrücke:  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT ProductID, 1+2  
FROM Production.Product;  
GO  
```  
  
 Der Ausdruck `1+2` wird für jede Zeile des Resultsets zu `3` ausgewertet. Obwohl der Ausdruck `ProductID` für jede Zeile des Resultsets einen eindeutigen Wert erzeugt, besitzt jede Zeile nur genau einen Wert für `ProductID`.  
 
- [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] ordnet jedem Thread eine maximale Menge an Arbeitsspeicher zu, sodass kein Thread den gesamten Arbeitsspeicher nutzen kann.  Ein Teil dieses Speichers dient zum Speichern von Abfrageausdrücken.  Wenn eine Abfrage zu viele Ausdrücke umfasst und der erforderliche Arbeitsspeicher den internen Grenzwert überschreitet, wird Sie von der Engine nicht ausgeführt.  Um dieses Problem zu vermeiden, können Benutzer die Abfrage in mehrere Abfragen mit einer geringeren Anzahl von Ausdrücken ändern. Beispiel: Sie haben eine Abfrage mit einer langen Liste von Ausdrücken in der WHERE-Klausel: 

```sql
DELETE FROM dbo.MyTable 
WHERE
(c1 = '0000001' AND c2 = 'A000001') or
(c1 = '0000002' AND c2 = 'A000002') or
(c1 = '0000003' AND c2 = 'A000003') or
...

```
Ändern Sie diese Abfrage in:

```sql
DELETE FROM dbo.MyTable WHERE (c1 = '0000001' AND c2 = 'A000001');
DELETE FROM dbo.MyTable WHERE (c1 = '0000002' AND c2 = 'A000002');
DELETE FROM dbo.MyTable WHERE (c1 = '0000003' AND c2 = 'A000003');
...
```

## <a name="see-also"></a>Siehe auch  
 [AT TIME ZONE &#40;Transact-SQL&#41;](../../t-sql/queries/at-time-zone-transact-sql.md)   
 [CASE &#40;Transact-SQL&#41;](../../t-sql/language-elements/case-transact-sql.md)   
 [CAST und CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)   
 [COALESCE &#40;Transact-SQL&#41;](../../t-sql/language-elements/coalesce-transact-sql.md)   
 [Datentypkonvertierung &#40;Datenbank-Engine&#41;](../../t-sql/data-types/data-type-conversion-database-engine.md)   
 [Rangfolge der Datentypen &#40;Transact-SQL&#41;](../../t-sql/data-types/data-type-precedence-transact-sql.md)   
 [Datentypen &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [Integrierte Funktionen &#40;Transact-SQL&#41;](~/t-sql/functions/functions.md)   
 [LIKE &#40;Transact-SQL&#41;](../../t-sql/language-elements/like-transact-sql.md)   
 [NULLIF &#40;Transact-SQL&#41;](../../t-sql/language-elements/nullif-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [WHERE &#40;Transact-SQL&#41;](../../t-sql/queries/where-transact-sql.md)  
  
  
