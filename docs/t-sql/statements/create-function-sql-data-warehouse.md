---
description: CREATE FUNCTION (Azure Synapse Analytics)
title: CREATE FUNCTION (Azure Synapse Analytics) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 09/17/2020
ms.prod: sql
ms.prod_service: sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: 8cad1b2c-5ea0-4001-9060-2f6832ccd057
author: julieMSFT
ms.author: jrasnick
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 7a7c60b45397f0a06cb7543dad9d15fff107a15f
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095765"
---
# <a name="create-function-azure-synapse-analytics"></a>CREATE FUNCTION (Azure Synapse Analytics)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Erstellt eine benutzerdefinierte Funktion in [!INCLUDE[ssSDW](../../includes/ssazuresynapse_md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]. Eine benutzerdefinierte Funktion ist eine [!INCLUDE[tsql](../../includes/tsql-md.md)]-Routine, die Parameter annimmt, eine Aktion ausführt (z.B. eine komplexe Berechnung) und das Ergebnis dieser Aktion als Wert zurückgeben kann. In [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] muss der Rückgabewert ein Skalarwert (Einzelwert) sein. In [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] kann CREATE FUNCTION durch Verwenden der Syntax für Inline-Tabellenwertfunktionen (Vorschau) eine Tabelle zurückgeben oder durch Verwenden der Syntax für Skalarfunktionen einen Einzelwert zurückgeben. Verwenden Sie diese Anweisung zum Erstellen einer wiederverwendbaren Routine, die auf folgende Weise verwendet werden kann:  
  
-   In [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisungen, wie z. B. SELECT  
  
-   In Anwendungen, die die Funktion aufrufen  
  
-   Bei der Definition einer anderen benutzerdefinierten Funktion  
  
-   Zum Definieren einer CHECK-Einschränkung für eine Spalte  
  
-   Zum Ersetzen einer gespeicherten Prozedur  
  
-   Zum Verwenden einer Inlinefunktion als Filterprädikat für eine Sicherheitsrichtlinie  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
-- Transact-SQL Scalar Function Syntax  (in Azure Synapse Analytics and Parallel Data Warehouse)
CREATE FUNCTION [ schema_name. ] function_name   
( [ { @parameter_name [ AS ] parameter_data_type   
    [ = default ] }   
    [ ,...n ]  
  ]  
)  
RETURNS return_data_type  
    [ WITH <function_option> [ ,...n ] ]  
    [ AS ]  
    BEGIN   
        function_body   
        RETURN scalar_expression  
    END  
[ ; ]  
  
<function_option>::=   
{  
    [ SCHEMABINDING ]  
  | [ RETURNS NULL ON NULL INPUT | CALLED ON NULL INPUT ]  
}  
```

```syntaxsql
-- Transact-SQL Inline Table-Valued Function Syntax (Preview in Azure Synapse Analytics only)
CREATE FUNCTION [ schema_name. ] function_name
( [ { @parameter_name [ AS ] parameter_data_type
    [ = default ] }
    [ ,...n ]
  ]
)
RETURNS TABLE
    [ WITH SCHEMABINDING ]
    [ AS ]
    RETURN [ ( ] select_stmt [ ) ]
[ ; ]
```

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]
  
## <a name="arguments"></a>Argumente  
 *schema_name*  
 Der Name des Schemas, zu dem die benutzerdefinierte Funktion gehört.  
  
 *function_name*  
 Der Name der benutzerdefinierten Funktion. Funktionsnamen müssen den Regeln für Bezeichner entsprechen und innerhalb der Datenbank und für jedes Schema eindeutig sein.  
  
> [!NOTE]  
>  Auf den Funktionsnamen müssen Klammern folgen, selbst wenn kein Parameter angegeben ist.  
  
 @*parameter_name*  
 Ein Parameter in der benutzerdefinierten Funktion. Ein oder mehrere Parameter können deklariert werden.  
  
 Eine Funktion kann maximal 2.100 Parameter haben. Der Benutzer muss beim Ausführen einer Funktion den Wert jedes deklarierten Parameters angeben (sofern kein Standardwert für den betreffenden Parameter definiert ist).  
  
 Geben Sie einen Parameternamen an, der mit dem Zeichen (@) beginnt. Der Parametername muss den Regeln für Bezeichner entsprechen. Parameter gelten lokal in der jeweiligen Funktion. Dieselben Parameternamen können in anderen Funktionen verwendet werden. Parameter können nur den Platz von Konstanten einnehmen. Sie können nicht anstelle von Tabellennamen, Spaltennamen oder Namen anderer Datenbankobjekte verwendet werden.  
  
> [!NOTE]  
>  ANSI_WARNINGS wird bei der Übergabe von Parametern in einer gespeicherten Prozedur oder in einer benutzerdefinierten Funktion oder beim Deklarieren und Festlegen von Variablen in einer Batchanweisung nicht berücksichtigt. Wird beispielsweise eine Variable als **char(3)** definiert und dann auf einen Wert festgelegt, der länger als drei Zeichen ist, werden die Daten auf die definierte Größe abgeschnitten, und die Anweisung INSERT oder UPDATE wird erfolgreich ausgeführt.  
  
 *parameter_data_type*  
 Der Parameterdatentyp. Für [!INCLUDE[tsql](../../includes/tsql-md.md)]-Funktionen sind alle skalaren Datentypen zulässig, die in [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] unterstützt werden. Der Datentyp timestamp (rowversion) wird nicht unterstützt.  
  
 [ =*default* ]  
 Ein Standardwert für den Parameter. Wenn ein *default*-Wert definiert ist, kann die Funktion ausgeführt werden, ohne dass ein Wert für diesen Parameter angegeben werden muss.  
  
 Wenn ein Parameter der Funktion über einen Standardwert verfügt, muss beim Aufrufen der Funktion das DEFAULT-Schlüsselwort angegeben werden, um den Standardwert abzurufen. In diesem Punkt gibt es einen Unterschied zum Verwenden von Parametern in einer gespeicherten Prozedur. Fehlt im Aufruf einer gespeicherten Prozedur ein Parameter, der einen Standardwert hat, wird automatisch dieser Standardwert verwendet.  
  
 *return_data_type*  
 Der Rückgabewert einer benutzerdefinierten Skalarfunktion. Für [!INCLUDE[tsql](../../includes/tsql-md.md)]-Funktionen sind alle skalaren Datentypen zulässig, die in [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] unterstützt werden. Der Datentyp timestamp (rowversion) wird nicht unterstützt. Der Cursor und die Tabelle von nicht skalaren Typen sind nicht zulässig.  
  
 *function_body*  
 Reihe von [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisungen.  function_body kann keine SELECT-Anweisung enthalten und kann nicht auf Datenbankdaten verweisen.  function_body kann nicht auf Tabellen oder Sichten verweisen. function_body kann andere deterministische Funktionen aufrufen, jedoch keine nicht deterministischen Funktionen aufrufen. 
  
 In Skalarfunktionen entspricht *function_body* einer Reihe von [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisungen, die zusammen einen Skalarwert ergeben.  
  
 *scalar_expression*  
 Gibt den skalaren Wert an, den die Skalarfunktion zurückgibt.  

 *select_stmt* **GILT FÜR**: Azure Synapse Analytics  
 Einzelne SELECT-Anweisung, die den Rückgabewert einer Inline-Tabellenwertfunktion (Vorschau) definiert.

 TABLE **GILT FÜR**: Azure Synapse Analytics  
 Gibt an, dass der Rückgabewert der Tabellenwertfunktion eine Tabelle ist. Nur Konstanten und @*local_variables* können an Tabellenwertfunktionen übergeben werden.

 In Inline-Tabellenwertfunktionen (Vorschau) wird der TABLE-Rückgabewert durch eine einzige SELECT-Anweisung definiert. Inlinefunktionen haben keine zugeordneten Rückgabevariablen.
  
 **\<function_option>::=** 
  
 Gibt an, dass die Funktion mindestens über eine der folgenden Optionen verfügen wird.  
  
 SCHEMABINDING  
 Gibt an, dass die Funktion an die Datenbankobjekte gebunden ist, auf die sie verweist. Wenn SCHEMABINDING angegeben ist, können an Basisobjekten keine Änderungen vorgenommen werden, die die Funktionsdefinition betreffen können. Zunächst muss die Funktionsdefinition selbst geändert oder gelöscht werden, um Abhängigkeiten in dem zu ändernden Objekt zu entfernen.  
  
 Die Bindung der Funktion an die Objekte, auf die sie verweist, wird nur bei einer der folgenden Aktionen entfernt:  
  
-   Die Funktion wird gelöscht.  
  
-   Die Funktion wird mithilfe der ALTER-Anweisung geändert, wobei die Option SCHEMABINDING nicht angegeben ist.  
  
 Eine Funktion kann nur dann schemagebunden sein, wenn die folgenden Bedingungen erfüllt sind:  
  
-   Alle benutzerdefinierten Funktionen, auf die die Funktion verweist, sind ebenfalls schemagebunden.  
  
-   Auf die Funktionen und die anderen benutzerdefinierten Funktionen, auf die die Funktion verweist, wird unter Verwendung eines einteiligen oder zweiteiligen Namens verwiesen.  
  
-   Nur auf integrierte Funktionen und andere benutzerdefinierte Funktionen in der gleichen Datenbank kann im Text der benutzerdefinierten Funktionen verwiesen werden.  
  
-   Der Benutzer, der die CREATE FUNCTION-Anweisung ausgeführt hat, besitzt REFERENCES-Berechtigungen für die Datenbankobjekte, auf die die Funktion verweist.  
  
 Zum Entfernen von SCHEMABINDING verwenden Sie ALTER  
  
 RETURNS NULL ON NULL INPUT | **CALLED ON NULL INPUT**  
 Gibt das **OnNULLCall**-Attribut einer Skalarwertfunktion an. Wenn das Attribut nicht angegeben ist, wird standardmäßig CALLED ON NULL INPUT verwendet. Dies bedeutet, dass der Hauptteil der Funktion ausgeführt wird, selbst wenn NULL als ein Argument übergeben wird.  
  
## <a name="best-practices"></a>Bewährte Methoden  
 Wenn eine benutzerdefinierte Funktion nicht mit der SCHEMABINDING-Klausel erstellt wurde, können sich die an zugrunde liegenden Objekten vorgenommenen Änderungen auf die Definition der Funktion auswirken und bei Aufruf der Funktion zu unerwarteten Ergebnissen führen. Es wird empfohlen, eine der folgenden Methoden zu implementieren, damit die Funktion aufgrund von Änderungen an den zugrunde liegenden Objekten nicht veraltet ist:  
  
-   Geben Sie beim Erstellen der Funktion die WITH SCHEMABINDING-Klausel an. Hiermit wird sichergestellt, dass die Objekte, auf die in der Funktionsdefinition verwiesen wird, nicht geändert werden können, es sei denn, die Funktion wird auch geändert.  
  
## <a name="interoperability"></a>Interoperabilität  
 Die folgenden Anweisungen sind in Skalarwertfunktionen gültig:  
  
-   Zuweisungsanweisungen  
  
-   Anweisungen zur Ablaufsteuerung, mit Ausnahme von TRY...CATCH-Anweisungen  
  
-   DECLARE-Anweisungen zum Definieren lokaler Datenbankvariablen.  

In einer Inline-Tabellenwertfunktion (Vorschau) ist nur eine einzelne SELECT-Anweisung zulässig.
  
## <a name="limitations-and-restrictions"></a>Einschränkungen  
 Mit benutzerdefinierten Funktionen können keine Aktionen ausgeführt werden, die den Status einer Datenbank ändern.  
  
 Benutzerdefinierte Funktionen können geschachtelt werden. Dies bedeutet, dass eine benutzerdefinierte Funktion eine andere aufrufen kann. Die Schachtelungsebene wird um eins erhöht, wenn die aufgerufene Funktion mit der Ausführung beginnt, und wird wieder um eins erniedrigt, wenn die aufgerufene Funktion die Ausführung beendet. Benutzerdefinierte Funktionen unterstützen bis zu 32 geschachtelte Ebenen. Ein Überschreiten der maximalen Schachtelungsebenen verursacht das Fehlschlagen der gesamten Funktionsaufrufskette.   
  
## <a name="metadata"></a>Metadaten  
 In diesem Abschnitt werden die Systemkatalogsichten aufgelistet, die Sie verwenden können, um Metadaten zu benutzerdefinierten Funktionen zurückzugeben.  
  
 [sys.sql_modules](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md): Zeigt die Definition von benutzerdefinierten [!INCLUDE[tsql](../../includes/tsql-md.md)]-Funktionen an. Beispiel:  
  
```sql  
SELECT definition, type   
FROM sys.sql_modules AS m  
JOIN sys.objects AS o   
    ON m.object_id = o.object_id   
    AND type = ('FN');  
GO  
  
```  
  
 [sys.parameters](../../relational-databases/system-catalog-views/sys-parameters-transact-sql.md): Zeigt Informationen zu den Parametern an, die in benutzerdefinierten Funktionen definiert sind.  
  
 [sys.sql_expression_dependencies](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md): Zeigt die zugrunde liegenden Objekte an, auf die von einer Funktion verwiesen wird.  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die CREATE FUNCTION-Berechtigung in der Datenbank und die ALTER-Berechtigung für das Schema, in dem die Funktion erstellt wird.  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Beispiele: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="a-using-a-scalar-valued-user-defined-function-to-change-a-data-type"></a>A. Verwenden einer benutzerdefinierten Skalarwertfunktion zum Ändern eines Datentyps  
 Diese einfache Funktion verwendet einen **int**-Datentyp als Eingabe und gibt einen **decimal(10,2)** -Datentyp als Ausgabe zurück.  
  
```sql  
CREATE FUNCTION dbo.ConvertInput (@MyValueIn int)  
RETURNS decimal(10,2)  
AS  
BEGIN  
    DECLARE @MyValueOut int;  
    SET @MyValueOut= CAST( @MyValueIn AS decimal(10,2));  
    RETURN(@MyValueOut);  
END;  
GO  
  
SELECT dbo.ConvertInput(15) AS 'ConvertedValue';  
```  

## <a name="examples-sssdwfull"></a>Beispiele: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)]  

### <a name="a-creating-an-inline-table-valued-function-preview"></a>A. Erstellen einer Inline-Tabellenwertfunktion (Vorschau)
 Im folgenden Beispiel wird eine Inline-Tabellenwertfunktion erstellt, um einige wichtige Informationen zu Modulen zurückzugeben, die nach dem `objectType`-Parameter gefiltert werden. Sie enthält einen Standardwert, durch den sie alle Module zurückgibt, wenn die Funktion mit dem DEFAULT-Parameter aufgerufen wird. In diesem Beispiel werden einige der Systemkatalogansichten verwendet, die in [Metadaten](#metadata) erwähnt werden.

```sql
CREATE FUNCTION dbo.ModulesByType(@objectType CHAR(2) = '%%')
RETURNS TABLE
AS
RETURN
(
    SELECT 
        sm.object_id AS 'Object Id',
        o.create_date AS 'Date Created',
        OBJECT_NAME(sm.object_id) AS 'Name',
        o.type AS 'Type',
        o.type_desc AS 'Type Description', 
        sm.definition AS 'Module Description'
    FROM sys.sql_modules AS sm  
    JOIN sys.objects AS o ON sm.object_id = o.object_id
    WHERE o.type like '%' + @objectType + '%'
);
GO
```
Anschließend kann die Funktion aufgerufen werden, um folgendermaßen alle Ansichtsobjekte (**V**) zurückzugeben:
```sql
select * from dbo.ModulesByType('V');
```

### <a name="b-combining-results-of-an-inline-table-valued-function-preview"></a>B. Kombinieren der Ergebnisse einer Inline-Tabellenwertfunktion (Vorschau)
 In diesem einfachen Beispiel wird die zuvor erstellte Tabellenwertfunktion verwendet, um zu zeigen, wie ihre Ergebnisse mithilfe von CROSS APPLY mit anderen Tabellen kombiniert werden können. Hier wählen wir alle Spalten aus beiden sys.objects und die Ergebnisse von `ModulesByType` für alle Zeilen aus, deren *type*-Spalte übereinstimmt. Weitere Details zum Verwenden von APPLY finden Sie unter [FROM-Klausel plus JOIN, APPLY, PIVOT](../../t-sql/queries/from-transact-sql.md).

```sql
SELECT * 
FROM sys.objects o
CROSS APPLY dbo.ModulesByType(o.type);
GO
```
  
## <a name="see-also"></a>Weitere Informationen  
 [ALTER FUNCTION (SQL Server PDW)](/previous-versions/sql/)   
 [DROP FUNCTION (SQL Server PDW)](/previous-versions/sql/)  
  
  

