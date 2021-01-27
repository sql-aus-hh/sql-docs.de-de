---
description: sp_cursor (Transact-SQL)
title: sp_cursor (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sp_cursor_TSQL
- sp_cursor
dev_langs:
- TSQL
helpviewer_keywords:
- sp_cursor
ms.assetid: 41ade0ca-5f11-469d-bd4d-c8302ccd93b3
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 07dcf9bfc42705f2ad49e3ce476ffcd8ad70f052
ms.sourcegitcommit: 00be343d0f53fe095a01ea2b9c1ace93cdcae724
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/26/2021
ms.locfileid: "98813281"
---
# <a name="sp_cursor-transact-sql"></a>sp_cursor (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Fordert positionierte Updates an. Mithilfe dieser Prozedur werden Vorgänge für mindestens eine Zeile im Fetchpuffer eines Cursors ausgeführt. sp_cursor wird aufgerufen, indem ID = 1 in einem Tabular Data Stream-Paket (TDS) angegeben wird.  
  
||  
|-|  
|**Gilt für**: SQL Server ( [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] bis [aktuelle Version](/troubleshoot/sql/general/determine-version-edition-update-level)).|  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sp_cursor  cursor, optype, rownum, table  
    [ , value[...n]]]  
```  
  
## <a name="arguments"></a>Argumente  
 *Cursor*  
 Das Cursorhandle. *Cursor* ist ein erforderlicher Parameter, der einen **int** -Eingabe Wert aufruft. *Cursor* ist der *handle* -Wert, der vom SQL Server generiert und von der sp_cursoropen Prozedur zurückgegeben wird.  
  
 *optype*  
 Ein erforderlicher Parameter, der festlegt, welcher Vorgang vom Cursor ausgeführt wird. *optype* erfordert einen der folgenden **int** -Eingabewerte.  
  
|Wert|Name|Beschreibung|  
|-----------|----------|-----------------|  
|0X0001|UPDATE|Wird zum Update mindestens einer Zeile im Fetchpuffer verwendet.  Die in *rowNum* angegebenen Zeilen werden erneut aufgerufen und aktualisiert.|  
|0x0002|DELETE|Wird zum Löschen mindestens einer Zeile im Fetchpuffer verwendet. Die in *rowNum* angegebenen Zeilen werden erneut aufgerufen und gelöscht.|  
|0X0004|INSERT|Fügt Daten ein, ohne eine SQL **Insert** -Anweisung zu entwickeln.|  
|0X0008|REFRESH|Wird verwendet, um den Puffer mithilfe zugrunde liegender Tabellen aufzufüllen, und kann zum Update der Zeile verwendet werden, wenn ein Update- oder Löschvorgang aufgrund der Steuerung durch vollständige Parallelität fehlerhaft ist, oder nachdem ein UPDATE-Vorgang ausgeführt wurde.|  
|0X10|LOCK|Bewirkt, dass eine SQL Server U-Sperre auf der Seite mit der angegebenen Zeile abgerufen wird. Diese Sperre ist mit S-Sperren kompatibel, jedoch nicht mit X-Sperren oder anderen U-Sperren. Kann verwendet werden, um eine kurzfristige Sperre zu implementieren.|  
|0X20|SETPOSITION|Wird nur verwendet, wenn das Programm eine nachfolgende SQL Server positionierte DELETE-oder Update-Anweisung ausgibt.|  
|0X40|ABSOLUTE|Kann nur in Verbindung mit UPDATE oder DELETE verwendet werden.  ABSOLUTE wird nur mit KEYSET-Cursorn verwendet (wird für DYNAMIC-Cursor ignoriert, und STATIC-Cursor können nicht aktualisiert werden).<br /><br /> Hinweis: Wenn absolute für eine Zeile im Keyset angegeben ist, die nicht abgerufen wurde, kann der Vorgang die Parallelitäts Überprüfung nicht durchführen, und das Rückgabe Ergebnis kann nicht garantiert werden.|  
  
 *rowNum*  
 Gibt an, welche Zeilen der Cursor im Fetchpuffer verwendet, aktualisiert oder löscht.  
  
> [!NOTE]  
>  Weder dieser Wert noch die über sp_cursor ausgeführten Update- oder Löschvorgänge wirken sich auf den Ausgangspunkt eines Abrufvorgangs RELATIVE, NEXT oder PREVIOUS aus.  
  
 *rowNum* ist ein erforderlicher Parameter, der einen **int** -Eingabe Wert aufruft.  
  
 1  
 Gibt die erste Zeile im Fetchpuffer an.  
  
 2  
 Gibt die zweite Zeile im Fetchpuffer an.  
  
 3, 4, 5  
 Gibt die dritte Zeile an usw.  
  
 n  
 Gibt die n-te Zeile im Fetchpuffer an.  
  
 0  
 Gibt alle Zeilen im Fetchpuffer an.  
  
> [!NOTE]  
>  Ist nur gültig für die Verwendung mit den *optype* -Werten Update, DELETE, Refresh oder Lock.  
  
 *Tabelle*  
 Tabellenname, der die Tabelle identifiziert, auf die sich *optype* bezieht, wenn die Cursor Definition einen Join oder mehrdeutige Spaltennamen enthält, die vom *value* -Parameter zurückgegeben werden. Wenn keine bestimmte Tabelle festgelegt wird, wird die erste Tabelle in der FROM-Klausel als Standard verwendet. *Table* ist ein optionaler Parameter, der einen Zeichen folgen-Eingabe Wert erfordert. Die Zeichenfolge kann als beliebiges Zeichen oder UNICODE-Datentyp angegeben werden. die *Tabelle* kann ein mehrteilige Tabellenname sein.  
  
 *value*  
 Wird zum Einfügen oder Aktualisieren von Werten verwendet. Der *Wert* Zeichen folgen Parameter wird nur bei Update-und INSERT- *optype* -Werten verwendet. Die Zeichenfolge kann als beliebiges Zeichen oder UNICODE-Datentyp angegeben werden.  
  
> [!NOTE]  
>  Die Parameternamen für *value* können vom Benutzer zugewiesen werden.  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 Bei Verwendung eines RPC gibt ein positionierter DELETE-oder Update-Vorgang mit der Puffer Nummer 0 eine done-Meldung mit einer Zeilen *Anzahl* von 0 (Fehler) oder 1 (Erfolg) für jede Zeile im Fetchpuffer zurück.  
  
## <a name="remarks"></a>Bemerkungen  
  
## <a name="optype-parameter"></a>optype-Parameter  
 Mit Ausnahme der Kombinationen von SetPosition mit Update, DELETE, Refresh oder Lock. oder absolut mit Update oder DELETE schließen sich die *optype* -Werte gegenseitig aus.  
  
 Die SET-Klausel des Update-Werts wird anhand des *value* -Parameters erstellt.  
  
 Ein Vorteil der Verwendung des einfügewerts *optype* ist, dass Sie vermeiden können, nicht-Zeichendaten in das Zeichenformat für Einfügungen umzuwandeln. Die Werte werden auf die gleiche Weise wie bei UPDATE angegeben. Wenn eine der erforderlichen Spalten nicht eingeschlossen wird, tritt ein INSERT-Fehler auf.  
  
-   Weder der SETPOSITION-Wert noch die über die sp_cursor-Schnittstelle ausgeführten Update- oder Löschvorgänge wirken sich auf den Ausgangspunkt eines Abrufvorgangs RELATIVE, NEXT oder PREVIOUS aus. Jede Zahl, die keine Zeile im Fetchpuffer angibt, führt dazu, dass die Position auf 1 festgelegt und kein Fehler zurückgegeben wird. Nachdem SetPosition ausgeführt wurde, bleibt die Position bis zum nächsten sp_cursorfetch Operation, T-SQL **Fetch** -Vorgang oder sp_cursor SetPosition-Vorgang über den gleichen Cursor wirksam. Durch einen nachfolgenden sp_cursorfetch-Vorgang wird die Cursorposition auf die erste Zeile im neuen Fetchpuffer festgelegt, während sich andere Cursoraufrufe nicht auf den Wert der Position auswirken. SETPOSITION kann von einer OR-Klausel mit REFRESH, UPDATE, DELETE oder LOCK verknüpft werden, um den Wert der Position auf die letzte geänderte Zeile festzulegen.  
  
 Wenn eine Zeile im Fetchpuffer nicht durch den *rowNum* -Parameter angegeben wird, wird die Position auf 1 festgelegt, ohne dass ein Fehler zurückgegeben wird. Nachdem die Position festgelegt wurde, bleibt sie so lange wirksam, bis der nächste sp_cursorfetch-Vorgang, T-SQL FETCH-Vorgang oder sp_cursor SETPOSITION-Vorgang für den gleichen Cursor ausgeführt wird.  
  
 SETPOSITION kann von einer OR-Klausel mit REFRESH, UPDATE, DELETE oder LOCK verknüpft werden, um die Cursorposition auf die letzte geänderte Zeile festzulegen.  
  
## <a name="rownum-parameter"></a>rownum-Parameter  
 Wenn angegeben, kann der *rowNum* -Parameter als Zeilennummer innerhalb des Keysets anstelle der Zeilennummer innerhalb des Fetchpuffers interpretiert werden. Der Benutzer ist für die Einhaltung der Parallelitätssteuerung verantwortlich. Dies bedeutet, dass bei SCROLL_LOCKS-Cursorn eine Sperre für die angegebene Zeile unabhängig verwaltet werden muss (dies kann über eine Transaktion erfolgen). Bei OPTIMISTIC-Cursorn müssen Sie zuvor die Zeile abgerufen haben, um diesen Vorgang auszuführen.  
  
## <a name="table-parameter"></a>table-Parameter  
 Wenn der *optype* -Wert "Update" oder "Insert" ist und eine vollständige Update-oder INSERT-Anweisung als *value* -Parameter übermittelt wird, wird der für die *Tabelle* angegebene Wert ignoriert.  
  
> [!NOTE]  
>  Bei Sichten kann nur eine zur Sicht gehörige Tabelle geändert werden. Die *Wert* Parameter-Spaltennamen müssen die Spaltennamen in der Sicht widerspiegeln, aber der Tabellenname kann der der zugrunde liegenden Basistabelle entsprechen (in diesem Fall wird sp_cursor durch den Sicht Namen ersetzt).  
  
## <a name="value-parameter"></a>value-Parameter  
 Es gibt zwei Alternativen zu den Regeln für die Verwendung des *Werts* , wie zuvor im Abschnitt "Arguments" angegeben:  
  
1.  Sie können einen Namen verwenden, der \@ dem Namen der Spalte in der SELECT-Liste für beliebige benannte *Wert* Parameter vorausgeht. Ein Vorteil dabei ist, dass möglicherweise keine Datenkonvertierung erforderlich ist.  
  
2.  Verwenden Sie einen Parameter, um entweder eine komplette Update-oder INSERT-Anweisung zu übermitteln, oder verwenden Sie mehrere Parameter, um Teile einer Update-oder INSERT-Anweisung zu übermitteln, die SQL Server dann in eine Complete-Anweisung Beispiele dafür finden Sie im Abschnitt "Beispiele" weiter unten in diesem Thema.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="alternative-value-parameter-uses"></a>Alternative Verwendung des value-Parameters  
 Für UPDATE:  
  
 Wenn ein einzelner Parameter verwendet wird, kann eine UPDATE-Anweisung mit der folgenden Syntax übermittelt werden:  
  
 `[ [ UPDATE <table name> ] SET ] {<column name> = expression} [,...n]`  
  
> [!NOTE]  
>  Wenn Update \<table name> angegeben ist, wird jeder für den *Table* -Parameter angegebene Wert ignoriert.  
  
 Wenn mehrere Parameter verwendet werden, muss der erste Parameter eine Zeichenfolge in der folgenden Form sein:  
  
 `[ SET ] <column name> = expression  [,...n]`  
  
 und die nachfolgenden Parameter müssen die folgende Form aufweisen:  
  
 `<column name> = expression  [,...n]`  
  
 In diesem Fall ist der \<table name> in der erstellten Update-Anweisung der Wert, der vom *Tabellen* Parameter entweder festgelegt oder standardmäßig festgelegt wurde.  
  
 Für INSERT:  
  
 Wenn ein einzelner Parameter verwendet wird, kann eine INSERT-Anweisung mit der folgenden Syntax übermittelt werden:  
  
 `[ [ INSERT [INTO] <table name> ] VALUES ] ( <expression> [,...n] )`  
  
> [!NOTE]  
>  Wenn INSERT *\<table name>* angegeben wird, wird jeder für den *Table* -Parameter angegebene Wert ignoriert.  
  
 Wenn mehrere Parameter verwendet werden, muss der erste Parameter eine Zeichenfolge in der folgenden Form sein:  
  
 `[ VALUES ( ] <expression>  [,...n]`  
  
 und die nachfolgenden Parameter müssen die folgende Form aufweisen:  
  
 `expression [,...n]`  
  
 es sei denn, VALUES wurde angegeben; in diesem Fall muss der letzte Ausdruck mit einer abschließenden ")" enden. In diesem Fall ist der *\<table name>* in der erstellten Update-Anweisung der Wert, der vom *Tabellen* Parameter entweder festgelegt oder standardmäßig festgelegt wurde.  
  
> [!NOTE]  
>  Es ist möglich, einen Parameter als benannten Parameter zu übermitteln, d. h. "`@VALUES`". In diesem Fall können keine weiteren benannten Parameter verwendet werden.  
  
## <a name="see-also"></a>Weitere Informationen  
 [sp_cursoropen &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-cursoropen-transact-sql.md)   
 [sp_cursorfetch &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-cursorfetch-transact-sql.md)   
 [Gespeicherte Systemprozeduren &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
