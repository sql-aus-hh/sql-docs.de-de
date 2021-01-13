---
description: SQLColAttribute-Funktion
title: SQLColAttribute-Funktion | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
apiname:
- SQLColAttribute
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLColAttribute
helpviewer_keywords:
- SQLColAttribute function [ODBC]
ms.assetid: 8c45c598-cb01-4789-a571-e93619a18ed9
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 43980caf7a592284aba87d57725c30ae9fe8e949
ms.sourcegitcommit: e40e75055c1435c5e3f9b6e3246be55526807b4c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/13/2021
ms.locfileid: "98151245"
---
# <a name="sqlcolattribute-function"></a>SQLColAttribute-Funktion
**Konformitäts**  
 Eingeführte Version: ODBC 3,0 Standards Compliance: ISO 92  
  
 **Zusammenfassung**  
 **SQLColAttribute** gibt Deskriptorinformationen für eine Spalte in einem Resultset zurück. Deskriptorinformationen werden als Zeichenfolge, ein vom Deskriptor abhängiger Wert oder ein ganzzahliger Wert zurückgegeben.  
  
> [!NOTE]  
>  Weitere Informationen dazu, wie der Treiber-Manager diese Funktion bei ODBC 3 zuordnet. die *x* -Anwendung arbeitet mit ODBC 2. *x* -Treiber finden Sie unter [Mapping Replace Functions for abwärts Compatibility of Applications](../../../odbc/reference/develop-app/mapping-replacement-functions-for-backward-compatibility-of-applications.md).  
  
## <a name="syntax"></a>Syntax  
  
```cpp  
  
SQLRETURN SQLColAttribute (  
      SQLHSTMT        StatementHandle,  
      SQLUSMALLINT    ColumnNumber,  
      SQLUSMALLINT    FieldIdentifier,  
      SQLPOINTER      CharacterAttributePtr,  
      SQLSMALLINT     BufferLength,  
      SQLSMALLINT *   StringLengthPtr,  
      SQLLEN *        NumericAttributePtr);  
```  
  
## <a name="arguments"></a>Argumente  
 *StatementHandle*  
 Der Anweisungs Handle.  
  
 *ColumnNumber*  
 Der Die Nummer des Datensatzes im IRD, von dem der Feldwert abgerufen werden soll. Dieses Argument entspricht der Spaltennummer der Ergebnisdaten, die sequenziell in der Reihenfolge der Spalten sortiert werden, beginnend bei 1. Spalten können in beliebiger Reihenfolge beschrieben werden.  
  
 In diesem Argument kann Spalte 0 angegeben werden, aber alle Werte außer SQL_DESC_TYPE und SQL_DESC_OCTET_LENGTH geben nicht definierte Werte zurück.  
  
 *FieldIdentifier*  
 Der Das Deskriptorhandle. Dieses Handle definiert, welches Feld im IRD abgefragt werden soll (z. b. SQL_COLUMN_TABLE_NAME).  
  
 *CharacterAttributePtr*  
 Ausgeben Ein Zeiger auf einen Puffer, in den der Wert im Feld " *fieldidentifier* " der *Spalte "ColumnNumber* " des IRD zurückgegeben werden soll, wenn das Feld eine Zeichenfolge ist. Andernfalls wird das Feld nicht verwendet.  
  
 Wenn " *charakteriattributeptr* " gleich NULL ist, gibt *stringlengthptr* weiterhin die Gesamtzahl der Bytes (ausgenommen des NULL-Beendigungs Zeichens für Zeichendaten) zurück, die im Puffer zurückgegeben werden können, auf den von " *charakteriattributeptr*" gezeigt wird.  
  
 *BufferLength*  
 Der Wenn *fieldidentifier* ein ODBC-definiertes Feld ist und " *attributeptr* " auf eine Zeichenfolge oder einen Binär Puffer zeigt, sollte dieses Argument die Länge von " \* *charakteriattributeptr*" sein. Wenn *fieldidentifier* ein ODBC-definiertes Feld und \* das *Attribut Attribut* PTR eine ganze Zahl ist, wird dieses Feld ignoriert. Wenn das *\* Attribut attributeptr* eine Unicode-Zeichenfolge ist (beim Aufrufen von **sqlcolattributew**), muss das *BufferLength* -Argument eine gerade Zahl sein. Wenn *fieldidentifier* ein Treiber definiertes Feld ist, gibt die Anwendung die Art des Felds für den Treiber-Manager an, indem das *BufferLength* -Argument festgelegt wird. *BufferLength* kann die folgenden Werte aufweisen:  
  
-   Wenn ' *charakteriattributeptr* ' ein Zeiger auf einen Zeiger ist, muss *BufferLength* den Wert SQL_IS_POINTER.  
  
-   Wenn ' *charakteriattributeptr* ' ein Zeiger auf eine Zeichenfolge ist, ist ' *BufferLength* ' die Länge des Puffers.  
  
-   Wenn " *charakteriattributeptr* " ein Zeiger auf einen binären Puffer ist, platziert die Anwendung das Ergebnis des Makros "SQL_LEN_BINARY_ATTR (*length*)" in " *BufferLength*". Dadurch wird ein negativer Wert in *BufferLength* platziert.  
  
-   Wenn ' *charakteriattributeptr* ' ein Zeiger auf einen Datentyp mit fester Länge ist, muss *BufferLength* eine der folgenden sein: SQL_IS_INTEGER, SQL_IS_UINTEGER, SQL_IS_SMALLINT oder SQL_IS_USMALLINT.  
  
 *Stringlengthptr*  
 Ausgeben Ein Zeiger auf einen Puffer, in dem die Gesamtzahl der Bytes (mit Ausnahme des NULL-Beendigungs Bytes für Zeichendaten) zurückgegeben werden soll, die in *-Wert *attributeptr* zurückgegeben werden können.  
  
 Wenn die Anzahl von Bytes, die für die Rückgabe verfügbar sind, größer oder gleich *BufferLength* ist, werden die Deskriptorinformationen in der Zeichenfolge für Zeichendaten \*  auf *BufferLength* abzüglich der Länge eines NULL-Beendigungs Zeichens gekürzt und vom Treiber auf Null enden.  
  
 Für alle anderen Datentypen wird der Wert von *BufferLength* ignoriert, und der Treiber geht davon aus, dass die Größe von **charakteriattributeptr* 32 Bits entspricht.  
  
 *Numeri| tributeptr*  
 Ausgeben Ein Zeiger auf einen ganzzahligen Puffer, in den der Wert im Feld " *fieldidentifier* " der *Spalte "ColumnNumber* " des IRD zurückgegeben werden soll, wenn das Feld ein numerischer Deskriptortyp ist, z. b. SQL_DESC_COLUMN_LENGTH. Andernfalls wird das Feld nicht verwendet. Beachten Sie, dass einige Treiber möglicherweise nur den unteren 32-Bit-oder 16-Bit-Puffer eines Puffers schreiben und das Bit höherer Ordnung unverändert lassen. Daher sollten Anwendungen den Wert vor dem Aufrufen dieser Funktion auf 0 initialisieren.  
  
## <a name="returns"></a>Gibt zurück  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_STILL_EXECUTING, SQL_ERROR oder SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnose  
 Wenn **SQLColAttribute** entweder SQL_ERROR oder SQL_SUCCESS_WITH_INFO zurückgibt, kann ein zugeordneter SQLSTATE-Wert durch Aufrufen von **SQLGetDiagRec** mit dem *Typ* SQL_HANDLE_STMT und einem *handle* von *StatementHandle* abgerufen werden. In der folgenden Tabelle sind die SQLSTATE-Werte aufgelistet, die häufig von **SQLColAttribute** zurückgegeben werden, und die einzelnen Werte werden im Kontext dieser Funktion erläutert. die Notation "(DM)" geht vor den Beschreibungen von Sqlstates vor, die vom Treiber-Manager zurückgegeben werden. Der Rückgabecode, der den einzelnen SQLSTATE-Werten zugeordnet ist, ist SQL_ERROR, sofern nichts anderes angegeben ist.  
  
|SQLSTATE|Fehler|BESCHREIBUNG|  
|--------------|-----------|-----------------|  
|01000|Allgemeine Warnung|Treiber spezifische Informations Meldung. (Die Funktion gibt SQL_SUCCESS_WITH_INFO zurück.)|  
|01004|Zeichen folgen Daten, rechts abgeschnitten|Der Puffer \* Wert *attributeptr* war nicht groß genug, um den gesamten Zeichen folgen Wert zurückzugeben, sodass der Zeichen folgen Wert abgeschnitten wurde. Die Länge des nicht abgeschnittene Zeichen folgen Werts wird in **stringlengthptr* zurückgegeben. (Die Funktion gibt SQL_SUCCESS_WITH_INFO zurück.)|  
|07005|Die vorbereitete Anweisung ist keine *Cursor Spezifikation* .|Die mit dem *StatementHandle* verknüpfte Anweisung hat kein Resultset zurückgegeben, und *fieldidentifier* wurde nicht SQL_DESC_COUNT. Es waren keine zu beschreiften Spalten vorhanden.|  
|07009|Ungültiger deskriptorindex.|(DM) der für *ColumnNumber* angegebene Wert war gleich 0, und das SQL_ATTR_USE_BOOKMARKS Statement-Attribut wurde SQL_UB_OFF.<br /><br /> Der für das Argument *ColumnNumber* angegebene Wert war größer als die Anzahl der Spalten im Resultset.|  
|HY000|Allgemeiner Fehler|Es ist ein Fehler aufgetreten, bei dem kein spezifischer SQLSTATE vorhanden war und für den kein Implementierungs spezifischer SQLSTATE definiert wurde. Die Fehlermeldung, die von **SQLGetDiagField** aus der Diagnosedaten Struktur zurückgegeben wird, beschreibt den Fehler und die Ursache.|  
|HY001|Fehler bei der Speicher Belegung|Der Treiber konnte keinen Arbeitsspeicher zuweisen, der zur Unterstützung der Ausführung oder Beendigung der Funktion erforderlich ist.|  
|HY008|Vorgang abgebrochen|Die asynchrone Verarbeitung wurde für " *StatementHandle*" aktiviert. Die Funktion wurde aufgerufen, und vor Abschluss der Ausführung wurde **SQLCancel** oder **sqlcancelhandle** für " *StatementHandle*" aufgerufen. Anschließend wurde die Funktion erneut für " *StatementHandle*" aufgerufen.<br /><br /> Die Funktion wurde aufgerufen, und vor Abschluss der Ausführung wurde **SQLCancel** oder **sqlcancelhandle** für das *StatementHandle* von einem anderen Thread in einer Multithread-Anwendung aufgerufen.|  
|HY010|Funktions Sequenz Fehler|(DM) eine asynchron ausgeführte Funktion wurde für das Verbindungs Handle aufgerufen, das mit dem *StatementHandle* verknüpft ist. Diese aynchronous-Funktion wurde noch ausgeführt, als "SQLColAttribute" aufgerufen wurde.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect** oder **SQLMoreResults** wurde für das *StatementHandle* aufgerufen und SQL_PARAM_DATA_AVAILABLE zurückgegeben. Diese Funktion wurde aufgerufen, bevor Daten für alle gestreuten Parameter abgerufen wurden.<br /><br /> (DM) die Funktion wurde vor dem Aufrufen von **SQLPrepare**, **SQLExecDirect** oder einer Katalog Funktion für das *StatementHandle* aufgerufen.<br /><br /> (DM) eine asynchron ausgeführte Funktion (nicht diese) wurde für das *StatementHandle* aufgerufen und wird noch ausgeführt, als diese Funktion aufgerufen wurde.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** oder **SQLSetPos** wurde für das *StatementHandle* aufgerufen und SQL_NEED_DATA zurückgegeben. Diese Funktion wurde aufgerufen, bevor Daten für alle Data-at-Execution-Parameter oder-Spalten gesendet wurden.|  
|HY013|Speicher Verwaltungsfehler|Der Funktions Aufrufwert konnte nicht verarbeitet werden, da auf die zugrunde liegenden Speicher Objekte nicht zugegriffen werden konnte, möglicherweise aufgrund von wenig Arbeitsspeicher.|  
|HY090|Ungültige Zeichen folgen-oder Pufferlänge|(DM) " *\* Merkmal attributeptr* " ist eine Zeichenfolge, und *BufferLength* war kleiner als 0 (null), aber nicht gleich SQL_NTS.|  
|HY091|Ungültiger Deskriptorfeldbezeichner.|Der für das Argument *fieldidentifier* angegebene Wert war keiner der definierten Werte und war kein von der Implementierung definierter Wert.|  
|HY117|Die Verbindung wurde aufgrund eines unbekannten Transaktions Zustands angehalten. Nur Disconnect-und Read-Only-Funktionen sind zulässig.|(DM) Weitere Informationen zum angehaltenen Status finden Sie unter [SQLEndTran Function](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Treiber nicht fähig|Der für das Argument " *fieldidentifier* " angegebene Wert wurde vom Treiber nicht unterstützt.|  
|HYT01|Verbindungs Timeout abgelaufen|Der Verbindungs Timeout Zeitraum ist abgelaufen, bevor die Datenquelle auf die Anforderung geantwortet hat. Der Timeout Zeitraum für die Verbindung wird über **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT festgelegt.|  
|IM001|Der Treiber unterstützt diese Funktion nicht.|(DM) der dem *StatementHandle* zugeordnete Treiber unterstützt die-Funktion nicht.|  
|IM017|Der Abruf ist im asynchronen Benachrichtigungs Modus deaktiviert.|Wenn das Benachrichtigungs Modell verwendet wird, ist das Abrufen deaktiviert.|  
|IM018|**Sqlcompleteasync** wurde nicht aufgerufen, um den vorherigen asynchronen Vorgang für dieses Handle abzuschließen.|Wenn der vorherige Funktionsaufruf für das Handle SQL_STILL_EXECUTING zurückgibt und der Benachrichtigungs Modus aktiviert ist, muss **sqlcompleteasync** für das Handle aufgerufen werden, um die Nachbearbeitung auszuführen und den Vorgang abzuschließen.|  
  
 Wenn nach **SQLPrepare** und vor **SQLExecute** aufgerufen wird, kann **SQLColAttribute** beliebige SQLSTATE-Werte zurückgeben, die von **SQLPrepare** oder **SQLExecute** zurückgegeben werden können, je nachdem, wann die Datenquelle die SQL-Anweisung auswertet, die dem *StatementHandle* zugeordnet ist.  
  
 Aus Leistungsgründen sollte eine Anwendung **SQLColAttribute** nicht vor dem Ausführen einer-Anweisung abrufen.  
  
## <a name="comments"></a>Kommentare  
 Informationen dazu, wie Anwendungen die von **SQLColAttribute** zurückgegebenen Informationen verwenden, finden Sie unter [Resultsetmetadaten](../../../odbc/reference/develop-app/result-set-metadata.md).  
  
 **SQLColAttribute** gibt Informationen entweder in \* *numerisintributeptr* oder in " \* *charakteriattributeptr*" zurück. Ganzzahlige Informationen werden in " \* *numeriingtributeptr* " als SQLLEN-Wert zurückgegeben. alle anderen Informationsformate werden in " \* *charakteriattributeptr*" zurückgegeben. Wenn Informationen in \* *numerisintributeptr* zurückgegeben werden, ignoriert der Treiber die *Attribute attributeptr*, *BufferLength* und *stringlengthptr*. Wenn Informationen in " \* *attributeptr*" zurückgegeben werden, ignoriert der Treiber " *numerisintributeptr*".  
  
 **SQLColAttribute** gibt Werte aus den Deskriptorfeldern der IRD zurück. Die-Funktion wird mit einem Anweisungs Handle anstelle eines Deskriptorhandles aufgerufen. Die von SQLColAttribute für die weiter unten in diesem Abschnitt *aufgeführten Werte,* die von **SQLColAttribute** zurückgegeben werden, können auch durch Aufrufen von **SQLGetDescField** mit dem entsprechenden IRD-Handle abgerufen werden.  
  
 Die aktuell definierten Deskriptorfelder, die Version von ODBC, in der Sie eingeführt wurden, und die Argumente, in denen Informationen für Sie zurückgegeben werden, werden weiter unten in diesem Abschnitt angezeigt. Weitere deskriptortypen können von Treibern definiert werden, um unterschiedliche Datenquellen zu nutzen.  
  
 Ein ODBC 3. der *x* -Treiber muss einen Wert für jedes der Deskriptorfelder zurückgeben. Wenn ein Deskriptorfeld nicht für einen Treiber oder eine Datenquelle gilt und sofern nicht anders angegeben, gibt der Treiber 0 in \* *stringlengthptr* oder eine leere Zeichenfolge in **charakteriattributeptr* zurück.  
  
## <a name="backward-compatibility"></a>Backward Compatibility  
 ODBC 3. die *x* -Funktion **SQLColAttribute** ersetzt den veralteten ODBC 2. *x* -Funktion **SQLColAttribute**. Beim Zuordnen von **SQLColAttribute** zu **SQLColAttribute** (bei ODBC 2.*die x* -Anwendung arbeitet mit ODBC 3. *x* -Treiber) oder **SQLColAttribute** **SQLColAttribute** zuordnen (bei ODBC 3.*die x* -Anwendung arbeitet mit ODBC 2. *x* -Treiber), der Treiber-Manager übergibt entweder den Wert von *fieldidentifier* über, ordnet ihn einem neuen Wert zu oder gibt wie folgt einen Fehler zurück:  
  
> [!NOTE]  
>  Das Präfix, das in den *fieldidentifier* -Werten in ODBC 3 verwendet wird. *x* wurde von der in ODBC 2 verwendeten geändert. *x*. Das neue Präfix ist "SQL_DESC"; das alte Präfix war "SQL_COLUMN".  
  
-   , Wenn der **#define** Wert von ODBC 2 ist. der Wert von " *x* *fieldidentifier* " ist mit dem **#define** Wert von ODBC 3 identisch. *x* *fieldidentifier*, der Wert im Funktions Aufrufwert wird gerade durchlaufen.  
  
-   Die **#define** Werte von ODBC 2. *x* *fieldidentifier* SQL_COLUMN_LENGTH, SQL_COLUMN_PRECISION und SQL_COLUMN_SCALE unterscheiden sich von den **#define** Werten von ODBC 3. *x* *fieldidentifier* SQL_DESC_PRECISION, SQL_DESC_SCALE und SQL_DESC_LENGTH. Ein ODBC 2. der *x* -Treiber muss nur ODBC 2 unterstützen. *x* -Werte. Ein ODBC 3. der *x* -Treiber muss sowohl die Werte "SQL_COLUMN" als auch "SQL_DESC" für diese drei *fieldidentifier* unterstützen. Diese Werte unterscheiden sich, da Genauigkeit, Dezimalstellen und Länge in ODBC 3 anders definiert sind. *x* als in ODBC 2. *x*. Weitere Informationen finden Sie unter [Spaltengröße, Dezimalstellen, Oktettlänge übertragen und Anzeige Größe](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md).  
  
-   , Wenn der **#define** Wert von ODBC 2 ist. der Wert von " *x* *fieldidentifier* " unterscheidet sich vom **#define** Wert von ODBC 3. *x* *fieldidentifier*, wie bei den Werten "count", "Name" und "Nullable", wird der Wert im Funktionsaufruf dem entsprechenden Wert zugeordnet. Beispielsweise wird SQL_COLUMN_COUNT SQL_DESC_COUNT zugeordnet, und SQL_DESC_COUNT wird SQL_COLUMN_COUNT zugeordnet, abhängig von der Richtung der Zuordnung.  
  
-   Wenn *fieldidentifier* ein neuer Wert in ODBC 3 ist. *x*, für den in ODBC 2 kein entsprechender Wert vorhanden war. *x*, wird es bei ODBC 3 nicht zugeordnet. die *x* -Anwendung verwendet Sie in einem **SQLColAttribute** -Befehl in einem ODBC 2. *x* -Treiber, und der-Rückruf gibt SQLSTATE HY091 (Ungültiger Deskriptorfeldbezeichner) zurück.  
  
 In der folgenden Tabelle sind die von **SQLColAttribute** zurückgegebenen deskriptortypen aufgeführt. Der Typ für numerische Werte von " *Numeri-tributeptr* " ist **sqllen \** _.  
  
|_FieldIdentifier *|Information<br /><br /> zurückgegeben in|BESCHREIBUNG|  
|-----------------------|---------------------------------|-----------------|  
|SQL_DESC_AUTO_UNIQUE_VALUE (ODBC 1,0)|*Numeri| tributeptr*|SQL_TRUE, wenn die Spalte eine autoincrementierung-Spalte ist.<br /><br /> SQL_FALSE, wenn die Spalte keine autoincrementierung-Spalte ist oder nicht numerisch ist.<br /><br /> Dieses Feld ist nur für numerische Datentyp Spalten gültig. Eine Anwendung kann Werte in eine Zeile einfügen, die eine AUTOINCREMENT-Spalte enthält, aber in der Regel keine Werte in der Spalte aktualisieren können.<br /><br /> Wenn eine Einfügung in eine AUTOINCREMENT-Spalte eingefügt wird, wird während der Einfügezeit ein eindeutiger Wert in die Spalte eingefügt. Das Inkrement ist nicht definiert, sondern ist Datenquellen spezifisch. Eine Anwendung sollte nicht davon ausgehen, dass eine AUTOINCREMENT-Spalte an einem bestimmten Punkt beginnt oder um einen bestimmten Wert erhöht.|  
|SQL_DESC_BASE_COLUMN_NAME (ODBC 3,0)|*CharacterAttributePtr*|Der Name der Basis Spalte für die Resultsetspalte. Wenn kein Basis Spaltenname vorhanden ist (wie bei Spalten, bei denen es sich um Ausdrücke handelt), enthält diese Variable eine leere Zeichenfolge.<br /><br /> Diese Informationen werden aus dem Feld SQL_DESC_BASE_COLUMN_NAME Datensatz von IRD zurückgegeben, das ein Schreib geschütztes Feld ist.|  
|SQL_DESC_BASE_TABLE_NAME (ODBC 3,0)|*CharacterAttributePtr*|Der Name der Basistabelle, die die Spalte enthält. Wenn der Name der Basistabelle nicht definiert werden kann oder nicht anwendbar ist, enthält diese Variable eine leere Zeichenfolge.<br /><br /> Diese Informationen werden aus dem Feld SQL_DESC_BASE_TABLE_NAME Datensatz von IRD zurückgegeben, das ein Schreib geschütztes Feld ist.|  
|SQL_DESC_CASE_SENSITIVE (ODBC 1,0)|*Numeri| tributeptr*|SQL_TRUE, wenn die Spalte bei Sortierungen und vergleichen bei der Groß-/Kleinschreibung beachtet wird.<br /><br /> SQL_FALSE, wenn die Spalte bei Sortierungen und vergleichen oder bei einem nicht-Zeichen nicht als Unterscheidung nach Groß-/Kleinschreibung behandelt wird.|  
|SQL_DESC_CATALOG_NAME (ODBC 2,0)|*CharacterAttributePtr*|Der Katalog der Tabelle, in der die Spalte enthalten ist. Der zurückgegebene Wert ist Implementierungs definiert, wenn es sich bei der Spalte um einen Ausdruck handelt oder wenn die Spalte Teil einer Sicht ist. Wenn die Datenquelle keine Kataloge unterstützt oder der Katalog Name nicht bestimmt werden kann, wird eine leere Zeichenfolge zurückgegeben. Dieses varchar-Daten Satz Feld ist nicht auf 128 Zeichen beschränkt.|  
|SQL_DESC_CONCISE_TYPE (ODBC 1,0)|*Numeri| tributeptr*|Der präzise Datentyp.<br /><br /> Für die DateTime-und interval-Datentypen gibt dieses Feld den präzisen Datentyp zurück. beispielsweise SQL_TYPE_TIME oder SQL_INTERVAL_YEAR. (Weitere Informationen finden Sie unter [Datentyp Bezeichner und Deskriptoren](../../../odbc/reference/appendixes/data-type-identifiers-and-descriptors.md) in Anhang D: Datentypen.)<br /><br /> Diese Informationen werden aus dem Feld SQL_DESC_CONCISE_TYPE Datensatz von IRD zurückgegeben.|  
|SQL_DESC_COUNT (ODBC 1,0)|*Numeri| tributeptr*|Die Anzahl der Spalten, die im Resultset verfügbar sind. Wenn keine Spalten im Resultset vorhanden sind, wird 0 zurückgegeben. Der Wert im *ColumnNumber* -Argument wird ignoriert.<br /><br /> Diese Informationen werden aus dem SQL_DESC_COUNT-Header Feld des IRD zurückgegeben.|  
|SQL_DESC_DISPLAY_SIZE (ODBC 1,0)|*Numeri| tributeptr*|Maximale Anzahl von Zeichen, die zum Anzeigen von Daten aus der Spalte erforderlich sind. Weitere Informationen zur Anzeige Größe finden Sie unter [Spaltengröße, Dezimalstellen, Länge von Oktett übertragen und Anzeige Größe](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) in Anhang D: Datentypen.|  
|SQL_DESC_FIXED_PREC_SCALE (ODBC 1,0)|*Numeri| tributeptr*|SQL_TRUE, wenn die Spalte eine Dezimal-und nicht-NULL-Skala hat, die Datenquellen spezifisch sind.<br /><br /> SQL_FALSE, wenn die Spalte nicht über eine festgelegte Genauigkeit mit fester Genauigkeit und ungleich 0 (null) verfügt, die Datenquellen spezifisch sind.|  
|SQL_DESC_LABEL (ODBC 2,0)|*CharacterAttributePtr*|Die Spalten Bezeichnung oder der Spaltentitel. Beispielsweise kann eine Spalte mit dem Namen EmpName als Mitarbeiter Name bezeichnet werden oder mit einem Alias versehen werden.<br /><br /> Wenn eine Spalte keine Bezeichnung hat, wird der Name der Spalte zurückgegeben. Wenn die Spalte nicht beschriftet und unbenannt ist, wird eine leere Zeichenfolge zurückgegeben.|  
|SQL_DESC_LENGTH (ODBC 3,0)|*Numeri| tributeptr*|Ein numerischer Wert, der entweder die maximale oder die tatsächliche Zeichen Länge einer Zeichenfolge oder eines Binär Datentyps ist. Dies ist die maximale Zeichen Länge für einen Datentyp mit fester Länge oder die tatsächliche Zeichen Länge für einen Datentyp mit variabler Länge. Der Wert schließt immer das NULL-Terminierungs Byte aus, das die Zeichenfolge beendet.<br /><br /> Diese Informationen werden aus dem Feld SQL_DESC_LENGTH Datensatz von IRD zurückgegeben.<br /><br /> Weitere Informationen über die Länge finden Sie unter [Spaltengröße, Dezimalstellen, Länge von Oktett übertragen und Anzeige Größe](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) in Anhang D: Datentypen.|  
|SQL_DESC_LITERAL_PREFIX (ODBC 3,0)|*CharacterAttributePtr*|Dieses varchar (128)-Daten Satz Feld enthält die Zeichen, die der Treiber als Präfix für einen Literalwert dieses Datentyps erkennt. Dieses Feld enthält eine leere Zeichenfolge für einen Datentyp, für den kein literalpräfix anwendbar ist. Weitere Informationen finden Sie unter [Literale Präfixe und Suffixe](../../../odbc/reference/develop-app/literal-prefixes-and-suffixes.md).|  
|SQL_DESC_LITERAL_SUFFIX (ODBC 3,0)|*CharacterAttributePtr*|Dieses varchar (128)-Daten Satz Feld enthält die Zeichen, die vom Treiber als Suffix für ein literaldieses Datentyps erkannt werden. Dieses Feld enthält eine leere Zeichenfolge für einen Datentyp, für den kein LiteralSuffix anwendbar ist. Weitere Informationen finden Sie unter [Literale Präfixe und Suffixe](../../../odbc/reference/develop-app/literal-prefixes-and-suffixes.md).|  
|SQL_DESC_LOCAL_TYPE_NAME (ODBC 3,0)|*CharacterAttributePtr*|Dieses varchar (128)-Daten Satz Feld enthält alle lokalisierten Namen (in der systemeigenen Sprache) für den Datentyp, die sich möglicherweise vom regulären Namen des Datentyps unterscheiden. Wenn kein lokalisierter Name vorhanden ist, wird eine leere Zeichenfolge zurückgegeben. Dieses Feld dient nur zu Anzeige Zwecken. Der Zeichensatz der Zeichenfolge ist vom Gebiets Schema abhängig und ist in der Regel der Standardzeichensatz des Servers.|  
|SQL_DESC_NAME (ODBC 3,0)|*CharacterAttributePtr*|Der Spaltenalias, falls zutreffend. Wenn der Spaltenalias nicht zutrifft, wird der Spaltenname zurückgegeben. In beiden Fällen wird SQL_DESC_UNNAMED auf SQL_NAMED festgelegt. Wenn kein Spaltenname oder Spaltenalias vorhanden ist, wird eine leere Zeichenfolge zurückgegeben und SQL_DESC_UNNAMED auf SQL_UNNAMED festgelegt.<br /><br /> Diese Informationen werden aus dem Feld SQL_DESC_NAME Datensatz von IRD zurückgegeben.|  
|SQL_DESC_NULLABLE (ODBC 3,0)|*Numeri| tributeptr*|SQL_ NULL-Werte zulassen, wenn die Spalte NULL-Werte aufweisen kann. SQL_NO_NULLS, wenn die Spalte keine NULL-Werte enthält. oder SQL_NULLABLE_UNKNOWN, wenn nicht bekannt ist, ob die Spalte NULL-Werte akzeptiert.<br /><br /> Diese Informationen werden aus dem Feld SQL_DESC_NULLABLE Datensatz von IRD zurückgegeben.|  
|SQL_DESC_NUM_PREC_RADIX (ODBC 3,0)|*Numeri| tributeptr*|Wenn der Datentyp im Feld SQL_DESC_TYPE ein ungefähren numerischer Datentyp ist, enthält dieses SQLINTEGER-Feld den Wert 2, da das SQL_DESC_PRECISION Feld die Anzahl der Bits enthält. Wenn der Datentyp im Feld SQL_DESC_TYPE ein genauer numerischer Datentyp ist, enthält dieses Feld den Wert 10, da das SQL_DESC_PRECISION Feld die Anzahl der Dezimalstellen enthält. Dieses Feld wird für alle nicht numerischen Datentypen auf 0 festgelegt.|  
|SQL_DESC_OCTET_LENGTH (ODBC 3,0)|*Numeri| tributeptr*|Die Länge einer Zeichenfolge oder eines Binär Datentyps in Bytes. Für Zeichen-oder Binär Typen mit fester Länge ist dies die tatsächliche Länge in Bytes. Für Zeichen-oder Binär Typen mit variabler Länge ist dies die maximale Länge in Bytes. Dieser Wert enthält nicht das NULL-Terminator.<br /><br /> Diese Informationen werden aus dem Feld SQL_DESC_OCTET_LENGTH Datensatz von IRD zurückgegeben.<br /><br /> Weitere Informationen über die Länge finden Sie unter [Spaltengröße, Dezimalstellen, Länge von Oktett übertragen und Anzeige Größe](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) in Anhang D: Datentypen.|  
|SQL_DESC_PRECISION (ODBC 3,0)|*Numeri| tributeptr*|Ein numerischer Wert, der für einen numerischen Datentyp die anwendbare Genauigkeit angibt. Bei Datentypen SQL_TYPE_TIME, SQL_TYPE_TIMESTAMP und allen Intervall Datentypen, die ein Zeitintervall darstellen, entspricht der Wert der anwendbaren Genauigkeit der Komponente für Sekundenbruchteile.<br /><br /> Diese Informationen werden aus dem Feld SQL_DESC_PRECISION Datensatz von IRD zurückgegeben.|  
|SQL_DESC_SCALE (ODBC 3,0)|*Numeri| tributeptr*|Ein numerischer Wert, der die anwendbare Skala für einen numerischen Datentyp ist. Bei Decimal-und numeric-Datentypen ist dies die definierte Skala. Es ist für alle anderen Datentypen nicht definiert.<br /><br /> Diese Informationen werden aus dem Feld "Skalierungs Daten Satz" von IRD zurückgegeben.|  
|SQL_DESC_SCHEMA_NAME (ODBC 2,0)|*CharacterAttributePtr*|Das Schema der Tabelle, in der die Spalte enthalten ist. Der zurückgegebene Wert ist Implementierungs definiert, wenn es sich bei der Spalte um einen Ausdruck handelt oder wenn die Spalte Teil einer Sicht ist. Wenn die Datenquelle keine Schemas unterstützt oder der Schema Name nicht bestimmt werden kann, wird eine leere Zeichenfolge zurückgegeben. Dieses varchar-Daten Satz Feld ist nicht auf 128 Zeichen beschränkt.|  
|SQL_DESC_SEARCHABLE (ODBC 1,0)|*Numeri| tributeptr*|SQL_PRED_NONE, wenn die Spalte nicht in einer WHERE-Klausel verwendet werden kann. (Dies entspricht dem SQL_UNSEARCHABLE Wert in ODBC 2. *x*.)<br /><br /> SQL_PRED_CHAR, wenn die Spalte in einer WHERE-Klausel verwendet werden kann, jedoch nur mit dem LIKE-Prädikat. (Dies entspricht dem SQL_LIKE_ONLY Wert in ODBC 2. *x*.)<br /><br /> SQL_PRED_BASIC, wenn die Spalte in einer WHERE-Klausel mit allen Vergleichs Operatoren mit Ausnahme von like verwendet werden kann. (Dies entspricht dem SQL_EXCEPT_LIKE Wert in ODBC 2. *x*.)<br /><br /> SQL_PRED_SEARCHABLE, wenn die Spalte in einer WHERE-Klausel mit einem beliebigen Vergleichs Operator verwendet werden kann.<br /><br /> Spalten vom Typ SQL_LONGVARCHAR und SQL_LONGVARBINARY geben normalerweise SQL_PRED_CHAR zurück.|  
|SQL_DESC_TABLE_NAME (ODBC 2,0)|*CharacterAttributePtr*|Der Name der Tabelle, die die Spalte enthält. Der zurückgegebene Wert ist Implementierungs definiert, wenn es sich bei der Spalte um einen Ausdruck handelt oder wenn die Spalte Teil einer Sicht ist.<br /><br /> Wenn der Tabellenname nicht bestimmt werden kann, wird eine leere Zeichenfolge zurückgegeben.|  
|SQL_DESC_TYPE (ODBC 3,0)|*Numeri| tributeptr*|Ein numerischer Wert, der den SQL-Datentyp angibt.<br /><br /> Wenn *ColumnNumber* gleich 0 ist, wird SQL_BINARY für Lesezeichen mit variabler Länge zurückgegeben, und SQL_INTEGER wird für Lesezeichen mit fester Länge zurückgegeben.<br /><br /> Für die DateTime-und interval-Datentypen gibt dieses Feld den ausführlichen Datentyp zurück: SQL_DATETIME oder SQL_INTERVAL. (Weitere Informationen finden Sie unter [Datentyp Bezeichner und Deskriptoren](../../../odbc/reference/appendixes/data-type-identifiers-and-descriptors.md) in Anhang D: Datentypen.<br /><br /> Diese Informationen werden aus dem Feld SQL_DESC_TYPE Datensatz von IRD zurückgegeben. **Hinweis:**  Zum Arbeiten mit ODBC 2. *x* -Treiber verwenden Sie stattdessen SQL_DESC_CONCISE_TYPE.|  
|SQL_DESC_TYPE_NAME (ODBC 1,0)|*CharacterAttributePtr*|Datenquellen abhängiger Datentyp Name; Beispiel: "char", "varchar", "Money", "Long varbinary" oder "char () for Bit Data".<br /><br /> Wenn der Typ unbekannt ist, wird eine leere Zeichenfolge zurückgegeben.|  
|SQL_DESC_UNNAMED (ODBC 3,0)|*Numeri| tributeptr*|SQL_NAMED oder SQL_UNNAMED. Wenn das SQL_DESC_NAME-Feld des IRD einen Spaltenalias oder einen Spaltennamen enthält, wird SQL_NAMED zurückgegeben. Wenn kein Spaltenname oder Spaltenalias vorhanden ist, wird SQL_UNNAMED zurückgegeben.<br /><br /> Diese Informationen werden aus dem Feld SQL_DESC_UNNAMED Datensatz von IRD zurückgegeben.|  
|SQL_DESC_UNSIGNED (ODBC 1,0)|*Numeri| tributeptr*|SQL_TRUE, wenn die Spalte nicht signiert (oder nicht numerisch) ist.<br /><br /> SQL_FALSE, wenn die Spalte signiert ist.|  
|SQL_DESC_UPDATABLE (ODBC 1,0)|*Numeri| tributeptr*|Die Spalte wird durch die Werte für die definierten Konstanten beschrieben:<br /><br /> SQL_ATTR_READONLY SQL_ATTR_WRITE SQL_ATTR_READWRITE_UNKNOWN<br /><br /> SQL_DESC_UPDATABLE beschreibt die Aktualisierbarkeit der Spalte im Resultset, nicht die Spalte in der Basistabelle. Die Aktualisierbarkeit der Basis Spalte, auf der die Resultsetspalte basiert, kann sich von dem Wert in diesem Feld unterscheiden. Ob eine Spalte aktualisierbar ist, kann auf dem Datentyp, den Benutzerrechten und der Definition des Resultsets basieren. Wenn unklar ist, ob eine Spalte aktualisierbar ist, sollten SQL_ATTR_READWRITE_UNKNOWN zurückgegeben werden.|  
  
 **SQLColAttribute** ist eine erweiterbare Alternative zu **SQLDescribeCol**. **SQLDescribeCol** gibt einen festgelegten Satz von Deskriptorinformationen zurück, die auf ANSI-89 SQL basieren. **SQLColAttribute** ermöglicht den Zugriff auf den umfangreicheren Satz von Deskriptorinformationen, die in den Lieferanten Erweiterungen von ANSI SQL-92 und DBMS verfügbar sind.  
  
## <a name="related-functions"></a>Verwandte Funktionen  
  
|Informationen über|Finden Sie unter|  
|---------------------------|---------|  
|Binden eines Puffers an eine Spalte in einem Resultset|[SQLBindCol-Funktion](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|Abbrechen der Anweisungs Verarbeitung|[SQLCancel-Funktion](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Zurückgeben von Informationen zu einer Spalte in einem Resultset|[SQLDescribeCol-Funktion](../../../odbc/reference/syntax/sqldescribecol-function.md)|  
|Abrufen eines Datenblocks oder Scrollen durch ein Resultset|[SQLFetchScroll-Funktion](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|Abrufen mehrerer Daten Zeilen|[SQLFetch-Funktion](../../../odbc/reference/syntax/sqlfetch-function.md)|  
  
## <a name="example"></a>Beispiel  
 Der folgende Beispielcode gibt keine Handles und Verbindungen frei. Codebeispiele zum Freigeben von Handles und Anweisungen finden Sie unter [SQLFreeHandle-Funktion](../../../odbc/reference/syntax/sqlfreehandle-function.md), [ODBC-Beispielprogramm](../../../odbc/reference/sample-odbc-program.md)und [SQLFreeStmt-Funktion](../../../odbc/reference/syntax/sqlfreestmt-function.md) .  
  
```cpp  
// SQLColAttibute.cpp  
// compile with: user32.lib odbc32.lib  
  
#define UNICODE  
  
#include <windows.h>  
#include <sqlext.h>  
#include <strsafe.h>  
  
struct DataBinding {  
   SQLSMALLINT TargetType;  
   SQLPOINTER TargetValuePtr;  
   SQLINTEGER BufferLength;  
   SQLLEN StrLen_or_Ind;  
};  
  
void printStatementResult(SQLHSTMT hstmt) {  
   int bufferSize = 1024, i;  
   SQLRETURN retCode;  
   SQLSMALLINT numColumn = 0, bufferLenUsed;
   
   retCode = SQLNumResultCols(hstmt, &numColumn);  
   
   SQLPOINTER* columnLabels = (SQLPOINTER *)malloc( numColumn * sizeof(SQLPOINTER*) );  
   struct DataBinding* columnData = (struct DataBinding*)malloc( numColumn * sizeof(struct DataBinding) );  
  
   printf( "Columns from that table:\n" );  
   for ( i = 0 ; i < numColumn ; i++ ) {  
      columnLabels[i] = (SQLPOINTER)malloc( bufferSize*sizeof(char) );  
  
      retCode = SQLColAttribute(hstmt, (SQLUSMALLINT)i + 1, SQL_DESC_LABEL, columnLabels[i], (SQLSMALLINT)bufferSize, &bufferLenUsed, NULL);  
      wprintf( L"Column %d: %s\n", i, (wchar_t*)columnLabels[i] );  
   }  
  
   // allocate memory for the binding  
   for ( i = 0 ; i < numColumn ; i++ ) {  
      columnData[i].TargetType = SQL_C_CHAR;  
      columnData[i].BufferLength = (bufferSize+1);  
      columnData[i].TargetValuePtr = malloc( sizeof(unsigned char)*columnData[i].BufferLength );  
   }  
  
   // setup the binding   
   for ( i = 0 ; i < numColumn ; i++ ) {  
      retCode = SQLBindCol(hstmt, (SQLUSMALLINT)i + 1, columnData[i].TargetType,   
         columnData[i].TargetValuePtr, columnData[i].BufferLength, &(columnData[i].StrLen_or_Ind));  
   }  
  
   printf( "Data from that table:\n" );  
   // fetch the data and print out the data  
   for ( retCode = SQLFetch(hstmt) ; retCode == SQL_SUCCESS || retCode == SQL_SUCCESS_WITH_INFO ; retCode = SQLFetch(hstmt) ) {  
      int j;  
      for ( j = 0 ; j < numColumn ; j++ )  
         wprintf( L"%s: %hs\n", columnLabels[j], columnData[j].TargetValuePtr );  
      printf( "\n" );  
   }  
   printf( "\n" );   
}  
  
int main() {  
   int bufferSize = 1024, i, count = 1, numCols = 5;  
   wchar_t firstTableName[1024], * dbName = (wchar_t *)malloc( sizeof(wchar_t)*bufferSize ), * userName = (wchar_t *)malloc( sizeof(wchar_t)*bufferSize );  
   HWND desktopHandle = GetDesktopWindow();   // desktop's window handle  
   SQLWCHAR connStrbuffer[1024];  
   SQLSMALLINT connStrBufferLen, bufferLen;  
   SQLRETURN retCode;  
  
   SQLHENV henv = NULL;   // Environment     
   SQLHDBC hdbc = NULL;   // Connection handle  
   SQLHSTMT hstmt = NULL;   // Statement handle  
  
   struct DataBinding* catalogResult = (struct DataBinding*) malloc( numCols * sizeof(struct DataBinding) );  
   SQLWCHAR* selectAllQuery = (SQLWCHAR *)malloc( sizeof(SQLWCHAR) * bufferSize );  
  
   // connect to database  
   retCode = SQLAllocHandle(SQL_HANDLE_ENV, SQL_NULL_HANDLE, &henv);  
   retCode = SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION, (SQLCHAR *)(void*)SQL_OV_ODBC3, -1);  
   retCode = SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc);  
   retCode = SQLSetConnectAttr(hdbc, SQL_LOGIN_TIMEOUT, (SQLPOINTER)10, 0);  
   retCode = SQLDriverConnect(hdbc, desktopHandle, L"Driver={SQL Server}", SQL_NTS, connStrbuffer, 1025, &connStrBufferLen, SQL_DRIVER_PROMPT);  
   retCode = SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmt);  
  
   // display the database information  
   retCode = SQLGetInfo(hdbc, SQL_DATABASE_NAME, dbName, (SQLSMALLINT)bufferSize, (SQLSMALLINT *)&bufferLen);  
   retCode = SQLGetInfo(hdbc, SQL_USER_NAME, userName, (SQLSMALLINT)bufferSize, &bufferLen);  
  
   for ( i = 0 ; i < numCols ; i++ ) {  
      catalogResult[i].TargetType = SQL_C_CHAR;  
      catalogResult[i].BufferLength = (bufferSize + 1);  
      catalogResult[i].TargetValuePtr = malloc( sizeof(unsigned char)*catalogResult[i].BufferLength );  
   }  
  
   // Set up the binding. This can be used even if the statement is closed by closeStatementHandle  
   for ( i = 0 ; i < numCols ; i++ )  
      retCode = SQLBindCol(hstmt, (SQLUSMALLINT)i + 1, catalogResult[i].TargetType, catalogResult[i].TargetValuePtr, catalogResult[i].BufferLength, &(catalogResult[i].StrLen_or_Ind));  
  
   retCode = SQLTables( hstmt, (SQLWCHAR*)SQL_ALL_CATALOGS, SQL_NTS, L"", SQL_NTS, L"", SQL_NTS, L"", SQL_NTS );  
   retCode = SQLFreeStmt(hstmt, SQL_CLOSE);  
  
   retCode = SQLTables( hstmt, dbName, SQL_NTS, userName, SQL_NTS, L"%", SQL_NTS, L"TABLE", SQL_NTS );  
  
   for ( retCode = SQLFetch(hstmt) ; retCode == SQL_SUCCESS || retCode == SQL_SUCCESS_WITH_INFO ; retCode = SQLFetch(hstmt), ++count )  
      if ( count == 1 )  
         StringCchPrintfW( firstTableName, 1024, L"%hs", catalogResult[2].TargetValuePtr );  
   retCode = SQLFreeStmt(hstmt, SQL_CLOSE);  
  
   wprintf( L"Select all data from the first table (%s)\n", firstTableName );  
   StringCchPrintfW( selectAllQuery, bufferSize, L"SELECT * FROM %s", firstTableName );  
  
   retCode = SQLExecDirect(hstmt, selectAllQuery, SQL_NTS);  
   printStatementResult(hstmt);  
}  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [ODBC-API-Referenz](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [ODBC-Header Dateien](../../../odbc/reference/install/odbc-header-files.md)   
 [ODBC-Beispielprogramm](../../../odbc/reference/sample-odbc-program.md)
