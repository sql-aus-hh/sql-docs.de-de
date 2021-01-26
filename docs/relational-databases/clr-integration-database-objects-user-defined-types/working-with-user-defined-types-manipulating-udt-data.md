---
title: Bearbeiten von UDT-Daten | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie Daten in UDT-Spalten einer SQL Server-Datenbank einfügen, auswählen und aktualisieren.
ms.custom: ''
ms.date: 12/05/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: clr
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- CAST function
- data deletions [CLR integration]
- data insertions [CLR integration]
- CONVERT function
- data updates [CLR integration]
- comparing data
- updating data [CLR integration]
- removing data
- IsByteOrdered property
- variables [CLR integration]
- data manipulation [CLR integration]
- user-defined types [CLR integration], data manipulation
- deleting data
- UDTs [CLR integration], data manipulation
- invoking UDT methods
- inserting data
ms.assetid: 51b1a5f2-7591-4e11-bfe2-d88e0836403f
author: rothja
ms.author: jroth
ms.openlocfilehash: d23880a7ea6a1e8f4c1beccc5ec82f40303b9b76
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783632"
---
# <a name="working-with-user-defined-types---manipulating-udt-data"></a>Arbeiten mit benutzerdefinierten Typen: Bearbeiten von UDT-Daten
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  [!INCLUDE[tsql](../../includes/tsql-md.md)] stellt keine spezialisierte Syntax für INSERT-, UPDATE- oder DELETE-Anweisungen zum Ändern von Daten in Spalten vom benutzerdefinierten Typ (User-defined Type, UDT) bereit. Die [!INCLUDE[tsql](../../includes/tsql-md.md)]-Funktionen CAST oder CONVERT werden verwendet, um systemeigene Datentypen in den benutzerdefinierten Typ (UDT) umzuwandeln.  
  
## <a name="inserting-data-in-a-udt-column"></a>Einfügen von Daten in eine UDT-Spalte  
 Die folgenden [!INCLUDE[tsql](../../includes/tsql-md.md)] Anweisungen fügen drei Zeilen mit Beispiel Daten in die **Points** -Tabelle ein. Der **Point** -Datentyp besteht aus X-und Y-ganzzahligen Werten, die als Eigenschaften des UDT verfügbar gemacht werden. Sie müssen entweder die CAST-oder die Convert-Funktion verwenden, um die durch Kommas getrennten X-und Y-Werte in den **Punkttyp** umzuwandeln. Die ersten beiden Anweisungen verwenden die Convert-Funktion, um einen Zeichen folgen Wert in den **Punkttyp** zu konvertieren, und die dritte Anweisung verwendet die CAST-Funktion:  
  
```sql  
INSERT INTO dbo.Points (PointValue) VALUES (CONVERT(Point, '3,4'));  
INSERT INTO dbo.Points (PointValue) VALUES (CONVERT(Point, '1,5'));  
INSERT INTO dbo.Points (PointValue) VALUES (CAST ('1,99' AS Point));  
```  
  
## <a name="selecting-data"></a>Auswählen von Daten  
 Die folgende SELECT-Anweisung wählt den Binärwert des UDTs aus.  
  
```sql  
SELECT ID, PointValue FROM dbo.Points  
```  
  
 Um die Ausgabe in einem lesbaren Format anzuzeigen, müssen Sie die **ToString** -Methode des **Point** -UDT aufzurufen, der den Wert in seine Zeichen folgen Darstellung konvertiert.  
  
```sql  
SELECT ID, PointValue.ToString() AS PointValue   
FROM dbo.Points;  
```  
  
 Hierdurch werden folgende Ergebnisse erzielt.  
  
```  
ID PointValue  
-- ----------  
 1 3,4  
 2 1,5  
 3 1,99  
```  
  
 Sie können auch die [!INCLUDE[tsql](../../includes/tsql-md.md)]-Funktionen CAST und CONVERT verwenden, um dieselben Ergebnisse zu erreichen.  
  
```sql  
SELECT ID, CAST(PointValue AS varchar)   
FROM dbo.Points;  
  
SELECT ID, CONVERT(varchar, PointValue)   
FROM dbo.Points;  
```  
  
 Der **Point** -UDT macht seine X-und Y-Koordinaten als Eigenschaften verfügbar, die Sie dann einzeln auswählen können. Die folgende [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisung wählt die X- und die Y-Koordinate getrennt aus:  
  
```sql  
SELECT ID, PointValue.X AS xVal, PointValue.Y AS yVal   
FROM dbo.Points;  
```  
  
 Die X-Eigenschaft und die Y-Eigenschaft geben einen ganzzahligen Wert zurück, der im Resultset angezeigt wird.  
  
```  
ID xVal yVal  
-- ---- ----  
 1    3    4  
 2    1    5  
 3    1   99  
```  
  
## <a name="working-with-variables"></a>Arbeiten mit Variablen  
 Sie können mit Variablen arbeiten, indem Sie die DECLARE-Anweisung verwenden, um einem UDT eine Variable zuzuweisen. Die folgenden Anweisungen weisen mithilfe der [!INCLUDE[tsql](../../includes/tsql-md.md)] Set-Anweisung einen Wert zu und zeigen die Ergebnisse an, indem Sie die **ToString** -Methode des UDT in der Variablen aufrufen:  
  
```sql  
DECLARE @PointValue Point;  
SET @PointValue = (SELECT PointValue FROM dbo.Points  
    WHERE ID = 2);  
SELECT @PointValue.ToString() AS PointValue;  
```  
  
 Das Resultset zeigt den Wert der Variablen an:  
  
```  
PointValue  
----------  
-1,5  
```  
  
 Mit den folgenden [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisungen wird dasselbe Ergebnis erzielt. Dabei wird für die Variablenzuweisung SELECT statt SET verwendet:  
  
```sql  
DECLARE @PointValue Point;  
SELECT @PointValue = PointValue FROM dbo.Points  
    WHERE ID = 2;  
SELECT @PointValue.ToString() AS PointValue;  
```  
  
 Der Unterschied zwischen der Verwendung von SELECT statt SET für die Variablenzuweisung besteht darin, dass SELECT ermöglicht, mehrere Variable in einer SELECT-Anweisung zuzuweisen, während die SET-Syntax erfordert, dass jede Variable durch eine eigene SET-Anweisung zugewiesen wird.  
  
## <a name="comparing-data"></a>Vergleichen von Daten  
 Sie können Vergleichs Operatoren zum Vergleichen von Werten im UDT verwenden, wenn Sie die **isbyteorder** -Eigenschaft beim Definieren der Klasse auf **true** festgelegt haben. Weitere Informationen finden Sie unter [Erstellen eines User-Defined Typs](../../relational-databases/clr-integration-database-objects-user-defined-types/creating-user-defined-types.md).  
  
```sql  
SELECT ID, PointValue.ToString() AS Points   
FROM dbo.Points  
WHERE PointValue > CONVERT(Point, '2,2');  
```  
  
 Sie können interne Werte des UDT unabhängig von der **isbyteorder** -Einstellung vergleichen, wenn die Werte selbst vergleichbar sind. Die folgende [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisung wählt die Zeilen aus, in denen X größer ist als Y:  
  
```sql  
SELECT ID, PointValue.ToString() AS PointValue   
FROM dbo.Points  
WHERE PointValue.X < PointValue.Y;  
```  
  
 Vergleichsoperatoren können auch mit Variablen verwendet werden, wie in dieser Abfrage gezeigt, in der nach einem übereinstimmenden PointValue gesucht wird.  
  
```sql  
DECLARE @ComparePoint Point;  
SET @ComparePoint = CONVERT(Point, '3,4');  
SELECT ID, PointValue.ToString() AS MatchingPoint   
FROM dbo.Points  
WHERE PointValue = @ComparePoint;  
```  
  
## <a name="invoking-udt-methods"></a>Aufrufen von UDT-Methoden  
 Sie können auch Methoden aufrufen, die im UDT in [!INCLUDE[tsql](../../includes/tsql-md.md)] definiert sind. Die **Point** -Klasse enthält drei Methoden, **Distance**, **DistanceFrom** und **DistanceFromXY**. Die Code Auflistungen, die diese drei Methoden definieren, finden Sie unter [Programmieren User-Defined Typen](../../relational-databases/clr-integration-database-objects-user-defined-types/creating-user-defined-types-coding.md).  
  
 Die folgende [!INCLUDE[tsql](../../includes/tsql-md.md)] Anweisung ruft die **PointValue. Distance** -Methode auf:  
  
```sql  
SELECT ID, PointValue.X AS [Point.X],   
    PointValue.Y AS [Point.Y],  
    PointValue.Distance() AS DistanceFromZero   
FROM dbo.Points;  
```  
  
 Die Ergebnisse werden in der Spalte **Entfernung** angezeigt:  
  
```  
ID X  Y  Distance  
-- -- -- ----------------  
 1  3  4                5  
 2  1  5 5.09901951359278  
 3  1 99 99.0050503762308  
```  
  
 Die **DistanceFrom** -Methode nimmt ein Argument des **Point** -Datentyps an und zeigt den Abstand zwischen dem angegebenen Punkt und dem PointValue-Wert an:  
  
```sql  
SELECT ID, PointValue.ToString() AS Pnt,  
   PointValue.DistanceFrom(CONVERT(Point, '1,99')) AS DistanceFromPoint  
FROM dbo.Points;  
```  
  
 Die Ergebnisse zeigen die Ergebnisse der **DistanceFrom** -Methode für jede Zeile in der Tabelle an:  
  
```  
ID Pnt DistanceFromPoint  
-- --- -----------------  
 1 3,4  95.0210502993942  
 2 1,5                94  
 3 1,9                90  
```  
  
 Die **DistanceFromXY** -Methode nimmt die Punkte einzeln als Argumente an:  
  
```sql  
SELECT ID, PointValue.X as X, PointValue.Y as Y,   
PointValue.DistanceFromXY(1, 99) AS DistanceFromXY   
FROM dbo.Points  
```  
  
 Das Resultset ist identisch mit der **DistanceFrom** -Methode.  
  
## <a name="updating-data-in-a-udt-column"></a>Aktualisieren von Daten in einer UDT-Spalte  
 Verwenden Sie die [!INCLUDE[tsql](../../includes/tsql-md.md)]-UPDATE-Anweisung, um Daten in einer UDT-Spalte zu aktualisieren. Sie können auch eine Methode des UDTs verwenden, um den Status des Objekts zu aktualisieren. Die folgende [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisung aktualisiert eine einzelne Zeile in der Tabelle:  
  
```sql  
UPDATE dbo.Points  
SET PointValue = CAST('1,88' AS Point)  
WHERE ID = 3  
```  
  
 Sie können auch UDT-Elemente getrennt aktualisieren. Die folgende [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisung aktualisiert nur die Y-Koordinate:  
  
```sql  
UPDATE dbo.Points  
SET PointValue.Y = 99  
WHERE ID = 3  
```  
  
 Wenn der UDT mit Byte Reihenfolge auf **true** festgelegt wurde, [!INCLUDE[tsql](../../includes/tsql-md.md)] kann die UDT-Spalte in einer WHERE-Klausel auswerten.  
  
```sql  
UPDATE dbo.Points  
SET PointValue = '4,5'  
WHERE PointValue = '3,4';  
```  
  
### <a name="updating-limitations"></a>Einschränkungen für Updates  
 Mehrere Eigenschaften können mit [!INCLUDE[tsql](../../includes/tsql-md.md)] nicht gleichzeitig aktualisiert werden. Die folgende Update-Anweisung schlägt z. b. mit einem Fehler fehl, da Sie den gleichen Spaltennamen nicht zweimal in einer Update-Anweisung verwenden können.  
  
```sql  
UPDATE dbo.Points  
SET PointValue.X = 5, PointValue.Y = 99  
WHERE ID = 3  
```  
  
 Um jeden Punkt einzeln zu aktualisieren, müssen Sie in der Point-UDT-Assembly eine Mutatormethode erstellen. Anschließend können Sie die Mutatormethode aufrufen, um das Objekt in einer [!INCLUDE[tsql](../../includes/tsql-md.md)]-UPDATE-Anweisung zu aktualisieren, wie im Folgenden dargestellt:  
  
```sql  
UPDATE dbo.Points  
SET PointValue.SetXY(5, 99)  
WHERE ID = 3  
```  
  
## <a name="deleting-data-in-a-udt-column"></a>Löschen von Daten in einer UDT-Spalte  
 Um Daten in einem UDT zu löschen, verwenden Sie die [!INCLUDE[tsql](../../includes/tsql-md.md)]-DELETE-Anweisung. Die folgende Anweisung löscht alle Zeilen in der Tabelle, die den in der WHERE-Klausel angegebenen Kriterien entsprechen. Wenn Sie die WHERE-Klausel einer DELETE-Anweisung weglassen, werden alle Zeilen in der Tabelle gelöscht.  
  
```sql  
DELETE FROM dbo.Points  
WHERE PointValue = CAST('1,99' AS Point)  
```  
  
 Sollen die Werte aus einer UDT-Spalte gelöscht werden, andere Zeilenwerte jedoch intakt bleiben, verwenden Sie die UPDATE-Anweisung. In diesem Beispiel wird für PointValue NULL festgelegt.  
  
```sql  
UPDATE dbo.Points  
SET PointValue = null  
WHERE ID = 2  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Arbeiten mit benutzerdefinierten Typen in SQL Server](../../relational-databases/clr-integration-database-objects-user-defined-types/working-with-user-defined-types-in-sql-server.md)  
  
  
