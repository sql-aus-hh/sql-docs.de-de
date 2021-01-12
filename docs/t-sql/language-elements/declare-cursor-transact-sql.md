---
description: DECLARE CURSOR (Transact-SQL)
title: DECLARE CURSOR (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DECLARE_CURSOR_TSQL
- CURSOR_TSQL
- DECLARE CURSOR
- CURSOR
dev_langs:
- TSQL
helpviewer_keywords:
- DECLARE CURSOR statement
- cursors [SQL Server], attributes
- local cursors [SQL Server]
- nesting cursors
- Transact-SQL cursors, attributes
- global cursors [SQL Server]
ms.assetid: 5a3a27aa-03e8-4c98-a27e-809282379b21
author: cawrites
ms.author: chadam
ms.openlocfilehash: 5fcd8a9ca3239d368b25f7444c11c311e4b1d306
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102483"
---
# <a name="declare-cursor-transact-sql"></a>DECLARE CURSOR (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Definiert die Attribute eines [!INCLUDE[tsql](../../includes/tsql-md.md)]-Servercursors, wie z. B. dessen Scrollverhalten, sowie die Abfrage, die zum Erstellen des Resultsets verwendet wird, auf das der Cursor ausgeführt wird. `DECLARE CURSOR` unterstützt sowohl die Syntax basierend auf dem ISO-Standard als auch eine Syntax, für die eine Teilmenge der [!INCLUDE[tsql](../../includes/tsql-md.md)]-Erweiterungen verwendet wird.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
ISO Syntax  
DECLARE cursor_name [ INSENSITIVE ] [ SCROLL ] CURSOR   
     FOR select_statement   
     [ FOR { READ ONLY | UPDATE [ OF column_name [ ,...n ] ] } ]  
[;]  
Transact-SQL Extended Syntax  
DECLARE cursor_name CURSOR [ LOCAL | GLOBAL ]   
     [ FORWARD_ONLY | SCROLL ]   
     [ STATIC | KEYSET | DYNAMIC | FAST_FORWARD ]   
     [ READ_ONLY | SCROLL_LOCKS | OPTIMISTIC ]   
     [ TYPE_WARNING ]   
     FOR select_statement   
     [ FOR UPDATE [ OF column_name [ ,...n ] ] ]  
[;]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *cursor_name*  
 Der Name des definierten Servercursors von [!INCLUDE[tsql](../../includes/tsql-md.md)]. *cursor_name* muss den Regeln für Bezeichner entsprechen.  
  
 INSENSITIVE  
 Definiert einen Cursor, der eine temporäre Kopie der von ihm zu verwendenden Daten erzeugt. Sämtliche Anforderungen an den Cursor werden von dieser temporären Tabelle in **tempdb** beantwortet; Änderungen an den Basistabellen werden nicht in den Daten wiedergegeben, die durch Abrufvorgänge an diesen Cursor zurückgegeben wurden. Darüber hinaus lässt dieser Cursor keine Änderungen zu. Bei Verwendung der ISO-Syntax ohne `INSENSITIVE` werden ausgeführte Löschvorgänge und Updates an den zugrunde liegenden Tabellen (von einem beliebigen Benutzer) in späteren Abrufvorgängen wiedergegeben.  
  
 SCROLL  
 Gibt an, dass alle Abrufoptionen (`FIRST`, `LAST`, `PRIOR`, `NEXT`, `RELATIVE`, `ABSOLUTE`) zur Verfügung stehen. Wird `SCROLL` in einer ISO-Anweisung des Typs `DECLARE CURSOR` nicht angegeben, wird nur die Abrufoption `NEXT` unterstützt. `SCROLL` kann nicht angegeben werden, wenn `FAST_FORWARD` ebenfalls angegeben ist. Wenn `SCROLL` nicht angegeben wird, ist nur die Abrufoption `NEXT` verfügbar, und der Cursor wird ein `FORWARD_ONLY`-Cursor.
  
 *select_statement*  
 Eine standardmäßige `SELECT`-Anweisung, die das Resultset des Cursors definiert. Die Schlüsselwörter `FOR BROWSE` und `INTO` sind in *select_statement* einer Cursordeklaration nicht zulässig.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] konvertiert den Cursor implizit in einen anderen Typ, wenn die Klauseln in *select_statement* in Konflikt mit der Funktionalität des angeforderten Cursortyps stehen.  
  
 READ ONLY  
 Verhindert, dass über diesen Cursor Updates vorgenommen werden. Auf den Cursor kann in einer `UPDATE`- oder `DELETE`-Anweisung nicht in einer `WHERE CURRENT OF`-Klausel verwiesen werden. Diese Option überschreibt die Standardeinstellung, nach der ein Cursor aktualisiert werden kann.  
  
 UPDATE [OF *column_name* [**,**...*n*]]  
 Definiert aktualisierbare Spalten innerhalb des Cursors. Wenn OF <column_name> [, <... n>] angegeben wird, können nur in den aufgeführten Spalten Änderungen vorgenommen werden. Wenn `UPDATE` ohne Spaltenliste angegeben wird, können alle Spalten aktualisiert werden.  
  
*cursor_name*  
Der Name des definierten Servercursors von [!INCLUDE[tsql](../../includes/tsql-md.md)]. *cursor_name* muss den Regeln für Bezeichner entsprechen.  
  
LOCAL  
Gibt an, dass der Gültigkeitsbereich des Cursors lokal zu dem Batch, der gespeicherten Prozedur oder dem Trigger ist, in dem bzw. in der er erstellt wurde. Der Cursorname ist nur innerhalb dieses Bereichs gültig. Auf den Cursor kann durch lokale Cursorvariablen im Batch, in der gespeicherten Prozedur, im Trigger oder im `OUTPUT`-Parameter einer gespeicherten Prozedur verwiesen werden. Ein `OUTPUT`-Parameter kann den lokalen Cursor an den aufrufenden Batch, die aufrufende gespeicherte Prozedur oder den aufrufenden Trigger zurückgeben. Diese können den Parameter einer Cursorvariablen zuweisen, um nach dem Beenden der gespeicherten Prozedur auf den Cursor zu verweisen. Die Zuordnung des Cursors wird implizit aufgehoben, wenn der Batch, die gespeicherte Prozedur oder der Trigger beendet wird, es sei denn, der Cursor wurde in einem `OUTPUT`-Parameter zurückgegeben. Wenn die Rückgabe in einem `OUTPUT`-Parameter erfolgt, wird die Zuordnung des Cursors aufgehoben, wenn die Zuordnung der letzten auf ihn verweisenden Variablen aufgehoben wird oder wenn der Cursor den Gültigkeitsbereich verlässt.  
  
GLOBAL  
Gibt an, dass der Bereich des Cursors global zur Verbindung ist. Auf den Cursornamen kann in jeder gespeicherten Prozedur und in jedem Batch verwiesen werden, die bzw. der von der Verbindung ausgeführt wird. Die Zuordnung des Cursors wird nur implizit aufgehoben, wenn die Verbindung getrennt wird.  
  
> [!NOTE]  
>  Wird weder `GLOBAL` noch `LOCAL` angegeben, wird der Standard durch die Einstellung der Datenbankoption **default to local cursor** gesteuert.  
  
FORWARD_ONLY  
Gibt an, dass der Cursor von der ersten bis zur letzten Zeile nur vorwärts bewegt werden kann. `FETCH NEXT` ist die einzige unterstützte Abrufoption. Alle INSERT-, UPDATE- und DELETE-Anweisungen, die vom aktuellen Benutzer ausgeführt werden (oder von anderen Benutzern committet werden), die sich auf Zeilen im Resultset auswirken, sind beim Abrufen der Zeilen sichtbar. Da mit dem Cursor nicht zurück gescrollt werden kann, sind Änderungen, die an Zeilen in der Datenbank vorgenommen wurden, nachdem die jeweilige Zeile abgerufen wurde, über den Cursor nicht sichtbar. Vorwärtscursor sind standardmäßig dynamisch, d. h. dass alle Änderungen ermittelt werden, während die aktuelle Zeile verarbeitet wird. Damit kann der Cursor schneller gestartet werden, und Updates an den zugrunde liegenden Tabellen können im Resultset angezeigt werden. Obwohl Vorwärtscursor das Zurückscrollen nicht unterstützen, können Anwendungen zum Anfang des Resultsets zurückkehren, indem der Cursor beendet und erneut gestartet wird. Wenn `FORWARD_ONLY` ohne eines der Schlüsselwörter `STATIC`, `KEYSET` oder `DYNAMIC` angegeben wird, ist der Cursor ein dynamischer Cursor. Wenn weder `FORWARD_ONLY` noch `SCROLL` angegeben wird, wird standardmäßig `FORWARD_ONLY` verwendet, es sei denn, die Schlüsselwörter `STATIC`, `KEYSET` oder `DYNAMIC` werden angegeben. Die Cursor `STATIC`, `KEYSET` und `DYNAMIC` sind standardmäßig auf `SCROLL` festgelegt. Anders als bei Datenbank-APIs wie ODBC und ADO, wird `FORWARD_ONLY` für die [!INCLUDE[tsql](../../includes/tsql-md.md)]-Cursor `STATIC`, `KEYSET` und `DYNAMIC` unterstützt.  
   
 STATIC  
Legt fest, dass der Cursor das Resultset immer so anzeigt, wie es war, als der Cursor gestartet wurde, und erstellt eine temporäre Kopie der Daten, die vom Cursor verwendet werden. Alle Anforderungen an den Cursor werden von dieser temporären Tabelle in der **tempdb** beantwortet. Aus diesem Grund werden INSERT-, UPDATE- und DELETE-Anweisungen für Basistabellen nicht in den Daten widergespiegelt, die durch Abrufvorgänge an diesen Cursor zurückgegeben werden, und der Cursor erkennt keine Änderungen an der Mitgliedschaft, der Reihenfolge oder den Werten des Resultsets, nachdem er geöffnet wurde. Statische Cursor können ihre eigenen UPDATE-, DELETE- und INSERT-Anweisungen erkennen, obwohl dies nicht erforderlich ist. Angenommen ein statischer Cursor ruft eine Zeile ab, und eine andere Anwendung aktualisiert diese Zeile dann. Wenn die Anwendung die Zeile erneut vom statischen Cursor abruft, sind die erkannten Werte unverändert, obwohl die andere Anwendung Änderungen vorgenommen hat. Alle Arten des Scrollens werden unterstützt. 
  
KEYSET  
Gibt an, dass im Cursor die Mitgliedschaft und Reihenfolge der Zeilen fest ist, wenn der Cursor geöffnet wird. Die Menge der Schlüssel, welche die Zeilen eindeutig identifizieren, wird in einer Tabelle in **tempdb** erstellt, die als **keyset** bezeichnet wird. Mit seiner Fähigkeit, Änderungen zu erkennen, bietet dieser Cursor Funktionalität zwischen statischen und dynamischen Cursor. Wie ein statischer Cursor ermittelt er nicht immer Änderungen an der Mitgliedschaft und Reihenfolge des Resultsets. Wie ein dynamischer Cursor ermittelt er Änderungen an den Werten der Zeilen im Resultset. Keysetgesteuerte Cursor werden von einer Reihe von eindeutigen Bezeichnern (Schlüssel) gesteuert, die als das Keyset bezeichnet werden. Die Schlüssel werden anhand einer Reihe von Spalten erstellt, die die Zeilen im Resultset eindeutig identifizieren. Das Keyset besteht aus Schlüsselwerten aus allen Zeilen, die von der Abfrageanweisung zurückgegeben werden. Mit einem keysetgesteuerten Cursor wird für jede Zeile im Cursor ein Schlüssel erstellt und gespeichert, der wiederum entweder auf der Clientarbeitsstation oder dem Server gespeichert wird. Wenn Sie auf eine beliebige Zeile zugreifen, wird der gespeicherte Schlüssel zum Abrufen der aktuellen Datenwerte aus der Datenquelle verwendet. In einem keysetgesteuerter Cursor wird die Mitgliedschaft des Resultsets fixiert, wenn das Keyset vollständig gefüllt wurde. Daher sind Ergänzungen und Updates, die die Mitgliedschaft betreffen, nicht Teil des Resultsets, bis dieses neu geöffnet wird. Änderungen an Datenwerten (entweder durch den Besitzer des Keysets oder andere Prozesse), die sichtbar sind, während der Benutzer durch das Resultset scrollt:
-  Wenn eine Zeile gelöscht wird, wird beim Abrufen der Zeile der Wert „-2“ für `@@FETCH_STATUS` zurückgegeben, da die gelöschte Zeile als Lücke im Resultset angezeigt wird. Der Schlüssel für die Zeile ist im Keyset enthalten, aber die Zeile ist nicht mehr im Resultset vorhanden. 
-  INSERTs außerhalb des Cursors (durch andere Prozesse) sind nur sichtbar, wenn der Cursor beendet und neu gestartet wird. INSERTs innerhalb des Cursors werden am Ende des Resultsets angezeigt.
-  Updates von Schlüsselwerten von außerhalb des Cursors sind vergleichbar mit einem Löschen der alten Zeile, gefolgt von einem Einfügen der neuen Zeile. Die Zeile mit den neuen Werten ist nicht sichtbar, und bei Versuchen, die Zeile mit den alten Werten abzurufen, wird `@@FETCH_STATUS` mit dem Wert -2 zurückgegeben. Die neuen Werte sind sichtbar, wenn das Update über den Cursor durch Angeben der `WHERE CURRENT OF`-Klausel durchgeführt wird. 

> [!NOTE]  
> Wenn die Abfrage auf mindestens eine Tabelle ohne einen eindeutigen Index verweist, wird der Keysetcursor in einen statischen Cursor konvertiert.  
  
DYNAMIC  
Definiert einen Cursor, der alle Datenänderungen an den Zeilen im Resultset widerspiegelt, während Sie im Cursor scrollen und einen neuen Datensatz abrufen, unabhängig davon, ob die Änderungen innerhalb oder außerhalb (durch andere Benutzer) des Cursors vorgenommen werden. Daher sind alle INSERT-, UPDATE- und -DELETE-Anweisungen von allen Benutzern über den Cursor sichtbar. Datenwerte, Reihenfolge und Mitgliedschaft der Zeilen können sich bei jedem Abrufvorgang ändern. Die Abrufoption `ABSOLUTE` wird für dynamische Cursor nicht unterstützt. Updates, die außerhalb des Cursors ausgeführt werden, sind nur dann sichtbar, wenn ein Commit für sie ausgeführt wurde (es sei denn, die Transaktionsisolationsstufe des Cursors wurde auf `UNCOMMITTED` festgelegt). Angenommen ein dynamischer Cursor ruft zwei Zeilen ab, und eine andere Anwendung aktualisiert daraufhin eine dieser Zeilen und löscht die andere. Wenn der dynamische Cursor diese Zeilen dann abruft, findet er die gelöschte Zeile nicht, die neuen Werte der aktualisierten Zeile werden jedoch angezeigt. 
  
FAST_FORWARD  
Gibt einen `FORWARD_ONLY`-, `READ_ONLY`-Cursor mit aktivierten Leistungsoptimierungen an. `FAST_FORWARD` kann nicht angegeben werden, wenn `SCROLL` oder `FOR_UPDATE` ebenfalls angegeben ist. Diese Art von Cursor lässt keine Änderungen an den Daten innerhalb des Cursors zu.  
  
> [!NOTE]  
> `FAST_FORWARD` und `FORWARD_ONLY` können nicht in der gleichen `DECLARE CURSOR`-Anweisung verwendet werden.  
  
READ_ONLY  
Verhindert, dass über diesen Cursor Updates vorgenommen werden. Auf den Cursor kann in einer `UPDATE`- oder `DELETE`-Anweisung nicht in einer `WHERE CURRENT OF`-Klausel verwiesen werden. Diese Option überschreibt die Standardeinstellung, nach der ein Cursor aktualisiert werden kann.  
  
SCROLL_LOCKS  
Gibt an, dass positionierte Updates oder Löschungen durch den Cursor garantiert erfolgreich sind. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sperrt die Zeilen, während sie in den Cursor eingelesen werden, um ihre Verfügbarkeit für spätere Änderungen sicherzustellen. `SCROLL_LOCKS` kann nicht angegeben werden, wenn `FAST_FORWARD` oder `STATIC` ebenfalls angegeben ist.  
  
OPTIMISTIC  
Gibt an, dass positionierte Updates oder Löschungen durch den Cursor nicht erfolgreich sind, wenn die Zeile seit dem letzten Einlesen in den Cursor aktualisiert wurde. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sperrt keine Zeilen, während sie in den Cursor eingelesen werden. Stattdessen wird durch Vergleiche von **timestamp**-Spaltenwerten oder einen Prüfsummenwert, wenn die Tabelle keine **timestamp**-Spalte aufweist, bestimmt, ob die Zeile nach dem Einlesen in den Cursor geändert wurde. Wurde die Zeile geändert, so schlägt der versuchte positionierte Update- oder Löschvorgang fehl. `OPTIMISTIC` kann nicht angegeben werden, wenn `FAST_FORWARD` ebenfalls angegeben ist.  
  
 TYPE_WARNING  
 Gibt an, dass dem Client eine Warnmeldung gesendet wird, wenn der Cursor vom angeforderten Typ in einen anderen Typ implizit konvertiert wird.  
  
 *select_statement*  
 Eine standardmäßige SELECT-Anweisung, die das Resultset des Cursors definiert. Die Schlüsselwörter `COMPUTE`, `COMPUTE BY`, `FOR BROWSE` und `INTO` sind in *select_statement* einer Cursordeklaration nicht zulässig.  
  
> [!NOTE]  
> Sie können einen Abfragehinweis in einer Cursordeklaration verwenden. Wenn Sie jedoch auch die `FOR UPDATE OF`-Klausel verwenden, geben Sie `OPTION (<query_hint>)` nach `FOR UPDATE OF` an.  
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] konvertiert den Cursor implizit in einen anderen Typ, wenn die Klauseln in *select_statement* in Konflikt mit der Funktionalität des angeforderten Cursortyps stehen. Weitere Informationen finden Sie unter Implizite Cursorkonvertierungen.  
  
FOR UPDATE [OF *column_name* [**,**...*n*]]  
Definiert aktualisierbare Spalten innerhalb des Cursors. Wenn `OF <column_name> [, <... n>]` angegeben wird, können Änderungen nur in den aufgelisteten Spalten vorgenommen werden. Wenn `UPDATE` ohne Spaltenliste angegeben wird, können alle Spalten aktualisiert werden, sofern nicht die Parallelitätsoption `READ_ONLY` angegeben wurde.  
  
## <a name="remarks"></a>Bemerkungen  
`DECLARE CURSOR` definiert die Attribute eines [!INCLUDE[tsql](../../includes/tsql-md.md)]-Servercursors, wie z. B. dessen Scrollverhalten, sowie die Abfrage, die zum Erstellen des Resultsets verwendet wird, auf das der Cursor ausgeführt wird. Die `OPEN`-Anweisung füllt das Resultset auf, `FETCH` gibt eine Zeile aus dem Resultset zurück. Die `CLOSE`-Anweisung gibt das aktuelle Resultset frei, das dem Cursor zugeordnet ist. Die `DEALLOCATE`-Anweisung gibt die vom Cursor verwendeten Ressourcen frei.  
  
Die erste Form der `DECLARE CURSOR`-Anweisung verwendet zum Deklarieren des Cursorverhaltens die ISO-Syntax. Die zweite Form der `DECLARE CURSOR`-Anweisung verwendet [!INCLUDE[tsql](../../includes/tsql-md.md)]-Erweiterungen, die die Definition von Cursorn mithilfe der gleichen Cursortypen zulassen, die in den Datenbank-API-Cursor-Funktionen von ODBC oder ADO verwendet werden.  
  
Die beiden Formen können nicht gleichzeitig verwendet werden. Wenn Sie das Schlüsselwort `SCROLL` oder `INSENSITIVE` vor dem Schlüsselwort `CURSOR` angeben, können Sie zwischen den Schlüsselwörtern `CURSOR` und `FOR <select_statement>` keine Schlüsselwörter verwenden. Wenn Sie Schlüsselwörter zwischen den Schlüsselwörtern `CURSOR` und `FOR <select_statement>` angeben, können Sie `SCROLL` oder `INSENSITIVE` nicht vor dem Schlüsselwort `CURSOR` angeben.  
  
Wird in einer `DECLARE CURSOR`-Anweisung mit der [!INCLUDE[tsql](../../includes/tsql-md.md)]-Syntax weder `READ_ONLY`, `OPTIMISTIC` noch `SCROLL_LOCKS` angegeben, ist der Standard folgendermaßen:  
  
-   Wenn die `SELECT`-Anweisung keine Updates (wegen fehlender Berechtigungen, Zugriff auf Remotetabellen, die keine Updates unterstützen usw.) unterstützt, ist der Cursor `READ_ONLY`.  
  
-   Die Cursor `STATIC` und `FAST_FORWARD` sind standardmäßig auf `READ_ONLY` festgelegt.  
  
-   Die Cursor `DYNAMIC` und `KEYSET` sind standardmäßig auf `OPTIMISTIC` festgelegt.  
  
Ein Verweis auf Cursornamen ist nur von anderen [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisungen aus möglich. Auf sie kann nicht von Datenbank-API-Funktionen verwiesen werden. Nach der Deklaration eines Cursors besteht beispielsweise keine Möglichkeit, in einer OLE DB-, ODBC- oder ADO-Funktion oder -Methode auf den Cursornamen zu verweisen. Die Zeilen des Cursors können nicht mithilfe von API-Funktionen oder API-Methoden abgerufen werden. Ein Abrufen der Zeilen ist lediglich mit den FETCH-Anweisungen von [!INCLUDE[tsql](../../includes/tsql-md.md)] möglich.  
  
Nach der Deklaration eines Cursors können die Eigenschaften des Cursors mithilfe der folgenden gespeicherten Systemprozeduren bestimmt werden:  
  
|Gespeicherte Systemprozeduren|BESCHREIBUNG|  
|------------------------------|-----------------|  
|**sp_cursor_list**|Gibt eine Liste der in der Verbindung aktuell sichtbaren Cursor und ihrer Attribute zurück.|  
|**sp_describe_cursor**|Beschreibt die Attribute eines Cursors, z. B. ob es sich um einen Vorwärtscursor oder einen Scrollcursor handelt.|  
|**sp_describe_cursor_columns**|Beschreibt die Spaltenattribute im Resultset des Cursors.|  
|**sp_describe_cursor_tables**|Beschreibt die Basistabellen, auf die der Cursor zugreift.|  
  
 Variablen können möglicherweise als Teil von *select_statement* verwendet werden, die einen Cursor deklariert. Die Werte von Cursorvariablen ändern sich nach dem Deklarieren eines Cursors nicht.  
  
## <a name="permissions"></a>Berechtigungen  
 In der Standardeinstellung haben alle Benutzer die `DECLARE CURSOR`-Berechtigung, wenn sie über die `SELECT`-Berechtigungen für die im Cursor verwendeten Sichten, Tabellen und Spalten verfügen.
 
## <a name="limitations-and-restrictions"></a>Einschränkungen

In einer Tabelle mit einem gruppierten Columnstore-Index können keine Cursor oder Trigger verwendet werden. Diese Einschränkung gilt nicht für nicht gruppierte Columnstore-Indizes; in einer Tabelle mit einem nicht gruppierten Columnstore-Index können Cursor und Trigger verwendet werden. 
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-using-simple-cursor-and-syntax"></a>A. Verwenden des einfachen Cursors und der einfachen Syntax  

Das beim Öffnen dieses Cursors generierte Resultset enthält alle Zeilen und Spalten in der Tabelle. Dieser Cursor kann aktualisiert werden. Alle Updates und Löschungen werden in Abrufen dieses Cursors dargestellt. `FETCH NEXT` ist der einzige verfügbare Abruf, da die `SCROLL`-Option nicht angegeben worden ist.  
 
```sql  
DECLARE vend_cursor CURSOR  
    FOR SELECT * FROM Purchasing.Vendor  
OPEN vend_cursor  
FETCH NEXT FROM vend_cursor;  
```  
  
### <a name="b-using-nested-cursors-to-produce-report-output"></a>B. Verwenden von geschachtelten Cursorn, um eine Berichtsausgabe zu erzeugen  
 Im folgenden Beispiel wird gezeigt, wie Cursor zum Erzeugen komplexer Berichte geschachtelt werden können. Der innere Cursor wird für jeden Anbieter deklariert.  
  
```sql  
SET NOCOUNT ON;  
  
DECLARE @vendor_id INT, @vendor_name NVARCHAR(50),  
    @message VARCHAR(80), @product NVARCHAR(50);  
  
PRINT '-------- Vendor Products Report --------';  
  
DECLARE vendor_cursor CURSOR FOR   
SELECT VendorID, Name  
FROM Purchasing.Vendor  
WHERE PreferredVendorStatus = 1  
ORDER BY VendorID;  
  
OPEN vendor_cursor  
  
FETCH NEXT FROM vendor_cursor   
INTO @vendor_id, @vendor_name  
  
WHILE @@FETCH_STATUS = 0  
BEGIN  
    PRINT ' '  
    SELECT @message = '----- Products From Vendor: ' +   
        @vendor_name  
  
    PRINT @message  
  
    -- Declare an inner cursor based     
    -- on vendor_id from the outer cursor.  
  
    DECLARE product_cursor CURSOR FOR   
    SELECT v.Name  
    FROM Purchasing.ProductVendor pv, Production.Product v  
    WHERE pv.ProductID = v.ProductID AND  
    pv.VendorID = @vendor_id  -- Variable value from the outer cursor  
  
    OPEN product_cursor  
    FETCH NEXT FROM product_cursor INTO @product  
  
    IF @@FETCH_STATUS <> 0   
        PRINT '         <<None>>'       
  
    WHILE @@FETCH_STATUS = 0  
    BEGIN  
  
        SELECT @message = '         ' + @product  
        PRINT @message  
        FETCH NEXT FROM product_cursor INTO @product  
        END  
  
    CLOSE product_cursor  
    DEALLOCATE product_cursor  
        -- Get the next vendor.  
    FETCH NEXT FROM vendor_cursor   
    INTO @vendor_id, @vendor_name  
END   
CLOSE vendor_cursor;  
DEALLOCATE vendor_cursor;  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [@@FETCH_STATUS &#40;Transact-SQL&#41;](../../t-sql/functions/fetch-status-transact-sql.md)   
 [CLOSE &#40;Transact-SQL&#41;](../../t-sql/language-elements/close-transact-sql.md)   
 [Cursors &#40;Transact-SQL&#41;](../../t-sql/language-elements/cursors-transact-sql.md)   
 [DEALLOCATE &#40;Transact-SQL&#41;](../../t-sql/language-elements/deallocate-transact-sql.md)   
 [FETCH &#40;Transact-SQL&#41;](../../t-sql/language-elements/fetch-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
  
  
