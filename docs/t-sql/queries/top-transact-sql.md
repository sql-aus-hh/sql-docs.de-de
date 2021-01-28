---
description: TOP (Transact-SQL)
title: TOP (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- TOP_TSQL
- TOP
dev_langs:
- TSQL
helpviewer_keywords:
- TOP clause
- first set of query result rows [SQL Server]
- TOP clause, about TOP clause
- queries [SQL Server], results
ms.assetid: da983c0a-06c5-4cf8-a6a4-7f9d66f34f2c
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: eb6024d0ad3ef6f34d170201c0fbacc3447dab26
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783589"
---
# <a name="top-transact-sql"></a>TOP (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Begrenzt die zurückgegebenen Zeilen in einem Abfrageresultset auf die in [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] angegebene Anzahl bzw. den darin angegebenen diesbezüglichen Prozentwert. Wenn TOP in Verbindung mit der ORDER BY-Klausel verwendet wird, ist das Resultset auf die ersten *N* sortierten Zeilen beschränkt. Andernfalls gibt TOP die ersten *N* Zeilen in zufälliger Reihenfolge zurück. Verwenden Sie diese Klausel, um die Anzahl der von einer SELECT-Anweisung zurückgegebenen Zeilen anzugeben. Oder verwenden Sie TOP, um die Zeilen anzugeben, die von einer INSERT-, UPDATE-, MERGE- oder DELETE-Anweisung betroffen sind.  
  
![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
 
 Die folgende Syntax gilt für SQL Server und Azure SQL-Datenbank:

```syntaxsql  
[   
    TOP (expression) [PERCENT]  
    [ WITH TIES ]  
]  
```  

Es folgt die Syntax für [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]:

```syntaxsql  
[   
    TOP ( expression )   
    [ WITH TIES ]  
]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
*expression*  
Der numerische Ausdruck, der die Anzahl der zurückzugebenden Zeilen angibt. *expression* wird implizit in einen **float**-Wert konvertiert, wenn PERCENT angegeben ist. Andernfalls wird *expression* in **bigint** konvertiert.  
  
PERCENT  
Gibt an, dass die Abfrage nur die ersten *expression* Prozent der Zeilen aus dem Resultset zurückgibt. Bruchwerte werden auf den nächsten ganzzahligen Wert aufgerundet.  
  
WITH TIES  
Gibt mindestens zwei Zeilen zurück, die zeitgleich den letzten Platz im beschränkten Resultset belegen. Sie müssen dieses Argument mit der **ORDER BY**-Klausel verwenden. Durch **WITH TIES** werden möglicherweise mehr Zeilen zurückgegeben, als der Wert in *expression* angibt. Wenn beispielsweise *expression* auf den Wert „5“ festgelegt ist, aber zwei weitere Zeilen den Werten der **ORDER BY**-Spalten in Zeile 5 entsprechen, enthält das Resultset sieben Zeilen.  
  
Sie können die TOP-Klausel mit dem WITH TIES-Argument nur in SELECT-Anweisungen und nur dann angeben, wenn Sie auch die ORDER BY-Klausel angegeben haben. Die zurückgegebene Reihenfolge beim Binden von Datensätzen ist willkürlich. ORDER BY wirkt sich nicht auf diese Regel aus.  
  
## <a name="best-practices"></a>Empfehlungen  
Verwenden Sie in einer SELECT-Anweisung immer eine ORDER BY-Klausel mit einer TOP-Klausel. Denn dies ist die einzige Möglichkeit, zuverlässig anzugeben, welche Zeilen von TOP betroffen sind.  
  
Verwenden Sie OFFSET und FETCH in der ORDER BY-Klausel anstelle der TOP-Klausel, um eine Abfrageauslagerung zu implementieren. Eine Abfrageauslagerung (d. h. das Senden von Abschnitten oder "Seiten" von Daten an den Client) kann leichter mit OFFSET- und FETCH-Klauseln implementiert werden. Weitere Informationen finden Sie unter [ORDER BY-Klausel &#40;Transact-SQL&#41;](../../t-sql/queries/select-order-by-clause-transact-sql.md).  
  
Schränken Sie die Anzahl der zurückgegebenen Zeilen mithilfe von TOP (oder OFFSET und FETCH) anstelle von SET ROWCOUNT ein. Diese Methoden werden SET ROWCOUNT aus folgenden Gründen vorgezogen:  
  
-   Im Rahmen einer SELECT-Anweisung kann der Wert von *expression* in der TOP- oder der FETCH-Klausel vom Abfrageoptimierer während der Abfrageoptimierung berücksichtigt werden. Da SET ROWCOUNT außerhalb einer Anweisung verwendet wird, die eine Abfrage ausführt, kann ihr Wert nicht in einem Abfrageplan berücksichtigt werden.  
  
## <a name="compatibility-support"></a>Kompatibilitätsunterstützung  
Aus Gründen der Abwärtskompatibilität sind die Klammern in SELECT-Anweisungen optional, wenn es sich bei dem Ausdruck um eine ganzzahlige Konstante handelt. Es empfiehlt sich, in SELECT-Anweisungen immer Klammern für TOP zu verwenden. Damit erzielen Sie Konsistenz mit der erforderlichen Verwendung in INSERT-, UPDATE-, MERGE- und DELETE-Anweisungen. 
  
## <a name="interoperability"></a>Interoperabilität  
Der TOP-Ausdruck wirkt sich nicht auf Anweisungen aus, die möglicherweise wegen eines Triggers ausgeführt werden. Die Tabellen **inserted** und **deleted** in den Triggern geben nur die Zeilen zurück, die tatsächlich von den INSERT-, UPDATE- MERGE- oder DELETE-Anweisungen betroffen sind. Beispiel: Ein INSERT TRIGGER, der aufgrund einer INSERT-Anweisung mit einer TOP-Klausel ausgelöst wird.  
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ermöglicht das Aktualisieren von Zeilen über Sichten. Weil die TOP-Klausel in die Sichtdefinition einbezogen werden kann, werden bestimmte Zeilen aufgrund eines Updates möglicherweise nicht mehr in der Sicht angezeigt, wenn die Zeilen die Anforderungen des TOP-Ausdrucks nicht mehr erfüllen.  
  
Bei Angabe in der MERGE-Anweisung wird die TOP-Klausel angewendet, *nachdem* die gesamte Quelltabelle und die gesamte Zieltabelle verknüpft wurden. Und die verknüpften Zeilen, die nicht für eine INSERT-, UPDATE- oder DELETE-Aktion infrage kommen, werden entfernt. Die TOP-Klausel verringert zudem die Anzahl der verknüpften Zeilen auf den angegebenen Wert, und die INSERT-, UPDATE- oder DELETE-Aktionen werden ungeordnet auf die verbliebenen verknüpften Zeilen angewendet. Dies bedeutet, dass für die Verteilung der Zeilen auf die in den WHEN-Klauseln definierten Aktionen keine bestimmte Reihenfolge gilt. Wenn beispielsweise die Angabe von „TOP (10)“ 10 Zeilen betrifft, werden sieben davon aktualisiert und drei eingefügt. Möglicherweise werden auch fünf Zeilen aktualisiert, vier eingefügt und eine gelöscht usw. Da die MERGE-Anweisung einen vollständigen Tabellenscan der Quell- und der Zieltabelle ausführt, kann die E/A-Leistung beeinträchtigt werden, wenn Sie mit der TOP-Klausel eine große Tabelle durch Erstellen mehrerer Batches ändern. In diesem Szenario muss unbedingt sichergestellt werden, dass alle aufeinanderfolgenden Batches auf neue Zeilen ausgerichtet sind.  
  
Geben Sie die TOP-Klausel in einer Abfrage mit einem UNION-, UNION ALL-, EXCEPT- oder INTERSECT-Operator mit Bedacht an. Es ist denkbar, dass eine Abfrage geschrieben wird, die unerwartete Ergebnisse zurückgibt, weil die Reihenfolge der logischen Verarbeitung für die TOP-Klausel und die ORDER BY-Klausel nicht immer intuitiv ist, wenn diese Operatoren in einem SELECT-Vorgang verwendet werden. Beispiel: Für die folgende Tabelle und die darin enthaltenen Daten sollen das günstigste rote Auto sowie das günstigste blaue Auto zurückgeben werden. Dies sind der rote PKW und der blaue LKW.  
  
```sql  
CREATE TABLE dbo.Cars(Model VARCHAR(15), Price MONEY, Color VARCHAR(10));  
INSERT dbo.Cars VALUES  
    ('sedan', 10000, 'red'), ('convertible', 15000, 'blue'),   
    ('coupe', 20000, 'red'), ('van', 8000, 'blue');  
```  
  
Um diese Ergebnisse zu erreichen, können Sie die folgende Abfrage schreiben:  
  
```sql  
SELECT TOP(1) Model, Color, Price  
FROM dbo.Cars  
WHERE Color = 'red'  
UNION ALL  
SELECT TOP(1) Model, Color, Price  
FROM dbo.Cars  
WHERE Color = 'blue'  
ORDER BY Price ASC;  
GO    
```  
  
Nachfolgend ist das Resultset dargestellt.  
  
 ```
 Model         Color      Price  
 ------------- ---------- -------  
 sedan         red        10000.00  
 convertible   blue       15000.00
 ```  
  
Die unerwarteten Ergebnisse werden zurückgegeben, weil die logische Ausführung der TOP-Klausel vor der logischen Ausführung der ORDER BY-Klausel erfolgt, mit der die Ergebnisse des Operators (in diesem Fall UNION ALL) sortiert werden. So werden von der vorherigen Abfrage ein beliebiges rotes und ein beliebiges blaues Auto zurückgegeben, und dann wird das Ergebnis dieser Union nach dem Preis sortiert. Im folgenden Beispiel wird veranschaulicht, wie eine Abfrage geschrieben wird, um das gewünschte Ergebnis zu erzielen.  
  
```sql  
SELECT Model, Color, Price  
FROM (SELECT TOP(1) Model, Color, Price  
      FROM dbo.Cars  
      WHERE Color = 'red'  
      ORDER BY Price ASC) AS a  
UNION ALL  
SELECT Model, Color, Price  
FROM (SELECT TOP(1) Model, Color, Price  
      FROM dbo.Cars  
      WHERE Color = 'blue'  
      ORDER BY Price ASC) AS b;  
GO    
```  
  
Durch die Verwendung von TOP und ORDER BY in einem untergeordneten SELECT-Vorgang stellen Sie sicher, dass die Ergebnisse der ORDER BY-Klausel auf die TOP-Klausel angewendet werden und damit nicht das Ergebnis des UNION-Vorgangs sortiert wird.  
  
 Hier ist das Resultset.  
  
 ```
 Model         Color      Price  
 ------------- ---------- -------  
 sedan         red        10000.00  
 van           blue        8000.00
 ```  
  
## <a name="limitations-and-restrictions"></a>Einschränkungen  
Wenn Sie TOP mit INSERT, UPDATE, MERGE oder DELETE verwenden, werden die referenzierten Zeilen in keiner bestimmten Reihenfolge angeordnet. Und Sie können die ORDER BY-Klausel nicht direkt in diesen Anweisungen angeben. Wenn Sie die TOP-Klausel verwenden müssen, um Zeilen in einer sinnvollen Reihenfolge einzufügen, zu löschen oder zu bearbeiten, verwenden Sie TOP mit einer ORDER BY-Klausel in einer untergeordneten SELECT-Anweisung. Beachten Sie in diesem Zusammenhang den folgenden Abschnitt „Beispiele“ in diesem Artikel.  
  
TOP kann nicht in UPDATE- und DELETE-Anweisungen für partitionierte Sichten verwendet werden.  
  
TOP kann nicht mit OFFSET und FETCH im gleichen Abfrageausdruck (im gleichen Abfragebereich) kombiniert werden. Weitere Informationen finden Sie unter [ORDER BY-Klausel &#40;Transact-SQL&#41;](../../t-sql/queries/select-order-by-clause-transact-sql.md).  
  
## <a name="examples"></a>Beispiele  
  
|Category|Funktionssyntaxelemente|  
|--------------|------------------------------|  
|[Grundlegende Syntax](#BasicSyntax)|TOP • PERCENT|  
|[Einschließen von gleichwertigen Werten](#tie)|WITH TIES|  
|[Beschränken der von DELETE, INSERT oder UPDATE betroffenen Zeilen](#DML)|DELETE • INSERT • UPDATE|  
  
###  <a name="basic-syntax"></a><a name="BasicSyntax"></a> Grundlegende Syntax  
Anhand von Beispielen in diesem Abschnitt wird die grundlegende Funktion der ORDER BY-Klausel mithilfe der mindestens erforderlichen Syntax veranschaulicht.  
  
#### <a name="a-using-top-with-a-constant-value"></a>A. Verwenden von TOP mit einem konstanten Wert  
In den folgenden Beispielen wird die Anzahl der Mitarbeiter, die in einem Abfrageresultset zurückgegeben werden, mit einem konstanten Wert angegeben. Im ersten Beispiel werden die ersten 10 nicht definierten Zeilen zurückgegeben, weil keine ORDER BY-Klausel verwendet wird. Im zweiten Beispiel wird eine ORDER BY-Klausel verwendet, um die ersten 10 Mitarbeiter zurückzugeben, die kürzlich eingestellt wurden.  
  
```sql  
USE AdventureWorks2012;  
GO  
-- Select the first 10 random employees.  
SELECT TOP(10)JobTitle, HireDate  
FROM HumanResources.Employee;  
GO  
-- Select the first 10 employees hired most recently.  
SELECT TOP(10)JobTitle, HireDate  
FROM HumanResources.Employee  
ORDER BY HireDate DESC;  
GO  
```  
  
#### <a name="b-using-top-with-a-variable"></a>B. Verwenden von TOP mit einer Variablen  
Im folgenden Beispiel wird die Anzahl der Mitarbeiter, die im Abfrageresultset zurückgegeben werden, mit einer Variablen angegeben.  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @p AS INT = 10;  
SELECT TOP(@p)JobTitle, HireDate, VacationHours  
FROM HumanResources.Employee  
ORDER BY VacationHours DESC;  
GO  
```  
  
#### <a name="c-specifying-a-percentage"></a>C. Angeben eines Prozentsatzes  
Im folgenden Beispiel wird die Anzahl der Mitarbeiter, die im Abfrageresultset zurückgegeben werden, mit PERCENT angegeben. Die Tabelle `HumanResources.Employee` enthält 290 Mitarbeiter. Da fünf Prozent von 290 ein Dezimalstellenwert ist, wird der Wert auf die nächste ganze Zahl aufgerundet.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT TOP(5)PERCENT JobTitle, HireDate  
FROM HumanResources.Employee  
ORDER BY HireDate DESC;  
GO    
```  
  
###  <a name="including-tie-values"></a><a name="tie"></a> Einschließen von gleichwertigen Werten  
  
#### <a name="a-using-with-ties-to-include-rows-that-match-the-values-in-the-last-row"></a>A. Einschließen von Zeilen mit identischen Werten wie vorangehende Zeilen mit WITH TIES  
Im folgenden Beispiel werden die obersten `10` Prozent aller Mitarbeiter mit dem höchsten Gehalt abgerufen und in absteigender Reihenfolge nach der Höhe des Gehalts zurückgegeben. Durch Angeben von `WITH TIES` wird sichergestellt, dass alle Mitarbeiter mit einem Gehalt, das dem niedrigsten zurückgegebenen Gehalt (der letzten Zeile) entspricht, ebenfalls im Resultset enthalten sind, auch wenn dadurch `10` Prozent der Mitarbeiter überschritten werden.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT TOP(10) PERCENT WITH TIES  
pp.FirstName, pp.LastName, e.JobTitle, e.Gender, r.Rate  
FROM Person.Person AS pp   
    INNER JOIN HumanResources.Employee AS e  
        ON pp.BusinessEntityID = e.BusinessEntityID  
    INNER JOIN HumanResources.EmployeePayHistory AS r  
        ON r.BusinessEntityID = e.BusinessEntityID  
ORDER BY Rate DESC;  
GO    
```  
  
###  <a name="limiting-the-rows-affected-by-delete-insert-or-update"></a><a name="DML"></a> Beschränken der von DELETE, INSERT oder UPDATE betroffenen Zeilen  
  
#### <a name="a-using-top-to-limit-the-number-of-rows-deleted"></a>A. Verwenden von TOP, um die Anzahl der zu löschenden Zeilen einzuschränken  
Wenn Sie eine TOP (*n*)-Klausel mit DELETE verwenden, wird der Löschvorgang für eine nicht definierte Auswahl von *n* Zeilen ausgeführt. Das heißt, die DELETE-Anweisung wählt eine beliebige Anzahl (*n*) von Zeilen aus, die die in der WHERE-Klausel definierten Kriterien erfüllen. Im folgenden Beispiel werden `20` Zeilen mit einem Fälligkeitsdatum vor dem 1. Juli 2002 aus der Tabelle `PurchaseOrderDetail` gelöscht.  
  
```sql  
USE AdventureWorks2012;  
GO  
DELETE TOP (20)   
FROM Purchasing.PurchaseOrderDetail  
WHERE DueDate < '20020701';  
GO  
```  
  
Wenn Sie die TOP-Klausel verwenden möchten, um Zeilen in einer sinnvollen Reihenfolge zu löschen, verwenden Sie TOP mit ORDER BY in einer untergeordneten SELECT-Anweisung. Die folgende Abfrage löscht die zehn Zeilen der `PurchaseOrderDetail` -Tabelle mit den frühesten Fälligkeitsdaten. Die in der untergeordneten SELECT-Anweisung angegebene Spalte (`PurchaseOrderID`) ist der Primärschlüssel der Tabelle, um sicherzustellen, dass nur 10 Zeilen gelöscht werden. Wird in der untergeordneten SELECT-Anweisung eine Nichtschlüsselspalte verwendet, werden möglicherweise mehr als 10 Zeilen gelöscht, wenn die angegebene Spalte doppelte Werte enthält.  
  
```sql  
USE AdventureWorks2012;  
GO  
DELETE FROM Purchasing.PurchaseOrderDetail  
WHERE PurchaseOrderDetailID IN  
   (SELECT TOP 10 PurchaseOrderDetailID   
    FROM Purchasing.PurchaseOrderDetail   
    ORDER BY DueDate ASC);  
GO  
```  
  
#### <a name="b-using-top-to-limit-the-number-of-rows-inserted"></a>B. Einschränken der Anzahl eingefügter Zeilen mit TOP  
Im folgenden Beispiel wird die Tabelle `EmployeeSales` erstellt, und der Name und die Verkaufszahlen des laufenden Jahres für die ersten fünf Mitarbeiter aus der Tabelle `HumanResources.Employee` werden eingefügt. Die INSERT-Anweisung wählt fünf Zeilen aus, die von der `SELECT`-Anweisung zurückgegeben werden, die die in der WHERE-Klausel definierten Kriterien erfüllen. Mit der OUTPUT-Klausel werden die Zeilen angezeigt, die in die `EmployeeSales`-Tabelle eingefügt werden. Beachten Sie, dass die ORDER BY-Klausel in der SELECT-Anweisung nicht zum Ermitteln der ersten fünf Mitarbeiter verwendet wird.  
  
```sql  
USE AdventureWorks2012 ;  
GO  
IF OBJECT_ID ('dbo.EmployeeSales', 'U') IS NOT NULL  
    DROP TABLE dbo.EmployeeSales;  
GO  
CREATE TABLE dbo.EmployeeSales  
( EmployeeID   NVARCHAR(11) NOT NULL,  
  LastName     NVARCHAR(20) NOT NULL,  
  FirstName    NVARCHAR(20) NOT NULL,  
  YearlySales  MONEY NOT NULL  
 );  
GO  
INSERT TOP(5)INTO dbo.EmployeeSales  
    OUTPUT inserted.EmployeeID, inserted.FirstName, inserted.LastName, inserted.YearlySales  
    SELECT sp.BusinessEntityID, c.LastName, c.FirstName, sp.SalesYTD   
    FROM Sales.SalesPerson AS sp  
    INNER JOIN Person.Person AS c  
        ON sp.BusinessEntityID = c.BusinessEntityID  
    WHERE sp.SalesYTD > 250000.00  
    ORDER BY sp.SalesYTD DESC;  
GO    
```  
  
Wenn Sie die TOP-Klausel verwenden möchten, um Zeilen in einer sinnvollen Reihenfolge einzufügen, verwenden Sie TOP mit ORDER BY in einer untergeordneten SELECT-Anweisung. Dies wird im folgenden Beispiel veranschaulicht. Mit der OUTPUT-Klausel werden die Zeilen angezeigt, die in die `EmployeeSales`-Tabelle eingefügt werden. Beachten Sie, dass die ersten fünf Mitarbeiter jetzt basierend auf den Ergebnissen der ORDER BY-Klausel und nicht anhand nicht definierter Zeilen eingefügt werden.  
  
```sql  
INSERT INTO dbo.EmployeeSales  
    OUTPUT inserted.EmployeeID, inserted.FirstName, inserted.LastName, inserted.YearlySales  
    SELECT TOP (5) sp.BusinessEntityID, c.LastName, c.FirstName, sp.SalesYTD   
    FROM Sales.SalesPerson AS sp  
    INNER JOIN Person.Person AS c  
        ON sp.BusinessEntityID = c.BusinessEntityID  
    WHERE sp.SalesYTD > 250000.00  
    ORDER BY sp.SalesYTD DESC;  
GO    
```  
  
#### <a name="c-using-top-to-limit-the-number-of-rows-updated"></a>C. Einschränken der Anzahl aktualisierter Zeilen mit TOP  
Im folgenden Beispiel wird die TOP-Klausel zur Aktualisierung von Zeilen in einer Tabelle verwendet. Wenn Sie eine TOP (*n*)-Klausel mit UPDATE verwenden, wird der Updatevorgang für eine nicht definierte Anzahl von Zeilen ausgeführt. Das heißt, die UPDATE-Anweisung wählt eine beliebige Anzahl (*n*) von Zeilen aus, die die in der WHERE-Klausel definierten Kriterien erfüllen. Im folgenden Beispiel werden 10 Kunden von einem Vertriebsmitarbeiter zu einem anderen zugewiesen.  
  
```sql  
USE AdventureWorks2012;  
UPDATE TOP (10) Sales.Store  
SET SalesPersonID = 276  
WHERE SalesPersonID = 275;  
GO  
```  
  
Wenn TOP verwendet werden muss, um Updates in einer sinnvollen Abfolge anzuwenden, muss in einer untergeordneten SELECT-Anweisung TOP gemeinsam mit ORDER BY verwendet werden. Mit dem nachfolgenden Beispiel werden die Urlaubsstunden der 10 Mitarbeiter mit dem frühesten Einstellungsdatum aktualisiert.  
  
```sql  
UPDATE HumanResources.Employee  
SET VacationHours = VacationHours + 8  
FROM (SELECT TOP 10 BusinessEntityID FROM HumanResources.Employee  
     ORDER BY HireDate ASC) AS th  
WHERE HumanResources.Employee.BusinessEntityID = th.BusinessEntityID;  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Beispiele: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
Im folgenden Beispiel werden die obersten 31 Zeilen zurückgegeben, die den Abfragekriterien entsprechen. Mit der **ORDER BY**-Klausel wird sichergestellt, dass es sich bei den 31 zurückgegebenen Zeilen um die ersten 31 Zeilen (basierend auf einer alphabetischen Sortierung der Spalte `LastName`) handelt.  
  
Verwenden Sie **TOP**, ohne WITH TIES anzugeben.  
  
```sql  
SELECT TOP (31) FirstName, LastName   
FROM DimEmployee ORDER BY LastName;  
```  
  
Ergebnis: 31 Zeilen werden zurückgegeben.  
  
Verwenden Sie TOP mit Angabe von WITH TIES.  
  
```sql  
SELECT TOP (31) WITH TIES FirstName, LastName   
FROM DimEmployee ORDER BY LastName;  
```  
  
Ergebnis: 33 Zeilen werden zurückgegeben, da drei Mitarbeiter namens „Brown“ in der 31. Zeile vorhanden sind.  
  
## <a name="see-also"></a>Weitere Informationen  
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)   
 [UPDATE (Transact-SQL)](../../t-sql/queries/update-transact-sql.md)   
 [DELETE &#40;Transact-SQL&#41;](../../t-sql/statements/delete-transact-sql.md)   
 [ORDER BY-Klausel &#40;Transact-SQL&#41;](../../t-sql/queries/select-order-by-clause-transact-sql.md)   
 [SET ROWCOUNT &#40;Transact-SQL&#41;](../../t-sql/statements/set-rowcount-transact-sql.md)   
 [MERGE &#40;Transact-SQL&#41;](../../t-sql/statements/merge-transact-sql.md)  
  
 
