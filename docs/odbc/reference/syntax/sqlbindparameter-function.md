---
description: SQLBindParameter-Funktion
title: SQLBindParameter-Funktion | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
apiname:
- SQLBindParameter
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLBindParameter
helpviewer_keywords:
- SQLBindParameter function [ODBC]
ms.assetid: 38349d4b-be03-46f9-9d6a-e50dd144e225
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 44b8b59722b790685b7fad47300039913d7294ae
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170152"
---
# <a name="sqlbindparameter-function"></a>SQLBindParameter-Funktion

**Konformitäts**  
 Eingeführte Version: ODBC 2,0 Standards Compliance: ODBC  
  
 **Zusammenfassung**  
 **SQLBindParameter** bindet einen Puffer an eine Parameter Markierung in einer SQL-Anweisung. **SQLBindParameter** unterstützt die Bindung an einen Unicode-C-Datentyp, auch wenn der zugrunde liegende Treiber keine Unicode-Daten unterstützt.  
  
> [!NOTE]  
>  Diese Funktion ersetzt die ODBC 1,0-Funktion **SQLSetParam**. Weitere Informationen finden Sie unter "comments".  
  
## <a name="syntax"></a>Syntax  
  
```cpp
  
SQLRETURN SQLBindParameter(  
      SQLHSTMT        StatementHandle,  
      SQLUSMALLINT    ParameterNumber,  
      SQLSMALLINT     InputOutputType,  
      SQLSMALLINT     ValueType,  
      SQLSMALLINT     ParameterType,  
      SQLULEN         ColumnSize,  
      SQLSMALLINT     DecimalDigits,  
      SQLPOINTER      ParameterValuePtr,  
      SQLLEN          BufferLength,  
      SQLLEN *        StrLen_or_IndPtr);  
```
  
## <a name="arguments"></a>Argumente

 *StatementHandle*  
 Der Anweisungs Handle.  
  
 *ParameterNumber*  
 Der Parameter Zahl, geordnet nach Reihenfolge in steigender Parameterreihenfolge, beginnend bei 1.  
  
 *InputOutputType*  
 Der Der Typ des Parameters. Weitere Informationen finden Sie unter "*inputoutputtype* -Argument" in "comments".  
  
 *ValueType*  
 Der Der C-Datentyp des Parameters. Weitere Informationen finden Sie unter "*ValueType* -Argument" in "comments".  
  
 *ParameterType*  
 Der Der SQL-Datentyp des Parameters. Weitere Informationen finden Sie unter "*ParameterType* -Argument" in "comments".  
  
 *ColumnSize*  
 Der Die Größe der Spalte oder des Ausdrucks der entsprechenden Parameter Markierung. Weitere Informationen finden Sie unter "*ColumnSize* -Argument" in "comments".  
  
 Wenn Ihre Anwendung unter einem 64-Bit-Windows-Betriebssystem ausgeführt wird, finden Sie weitere [Informationen unter ODBC 64-Bit-Informationen](../../../odbc/reference/odbc-64-bit-information.md).  
  
 *DecimalDigits*  
 Der Die Dezimalziffern der Spalte oder des Ausdrucks der entsprechenden Parameter Markierung. Weitere Informationen zur Spaltengröße finden Sie unter [Spaltengröße, Dezimalstellen, Länge von Oktett übertragen und Anzeige Größe](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md).  
  
 *ParameterValuePtr*  
 [Verzögerte Eingabe] Ein Zeiger auf einen Puffer für die Daten des Parameters. Weitere Informationen finden Sie unter "*ParameterValuePtr* -Argument" in "comments".  
  
 *BufferLength*  
 [Eingabe/Ausgabe] Länge des *ParameterValuePtr* -Puffers in Bytes. Weitere Informationen finden Sie unter "*BufferLength* -Argument" in "comments".  
  
 Weitere Informationen finden Sie unter [ODBC 64-Bit-Informationen](../../../odbc/reference/odbc-64-bit-information.md), wenn Ihre Anwendung unter einem 64-Bit-Betriebssystem ausgeführt wird.  
  
 *StrLen_or_IndPtr*  
 [Verzögerte Eingabe] Ein Zeiger auf einen Puffer für die Länge des Parameters. Weitere Informationen finden Sie unter "*StrLen_or_IndPtr* -Argument" in "comments".  
  
## <a name="returns"></a>Gibt zurück

 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR oder SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnose

 Wenn **SQLBindParameter** SQL_ERROR oder SQL_SUCCESS_WITH_INFO zurückgibt, kann ein zugeordneter SQLSTATE-Wert durch Aufrufen von **SQLGetDiagRec** mit dem *Handlertyp* SQL_HANDLE_STMT und einem *handle* von *StatementHandle* abgerufen werden. In der folgenden Tabelle sind die SQLSTATE-Werte aufgelistet, die normalerweise von **SQLBindParameter** zurückgegeben werden, und die einzelnen Werte werden im Kontext dieser Funktion erläutert. die Notation "(DM)" geht vor den Beschreibungen von Sqlstates vor, die vom Treiber-Manager zurückgegeben werden. Der Rückgabecode, der den einzelnen SQLSTATE-Werten zugeordnet ist, ist SQL_ERROR, sofern nichts anderes angegeben ist.  

|SQLSTATE|Fehler|Beschreibung|  
|--------------|-----------|-----------------|  
|01000|Allgemeine Warnung|Treiber spezifische Informations Meldung. (Die Funktion gibt SQL_SUCCESS_WITH_INFO zurück.)|  
|07006|Verletzung des Attributs für eingeschränkte Datentypen|Der vom *ValueType* -Argument identifizierte Datentyp kann nicht in den Datentyp konvertiert werden, der durch das *Parameter Type* -Argument identifiziert wird. Beachten Sie, dass dieser Fehler von **SQLExecDirect**, **SQLExecute** oder **SQLPutData** zur Ausführungszeit anstelle von **SQLBindParameter** zurückgegeben werden kann.|  
|07009|Ungültiger deskriptorindex.|(DM) der für das Argument *ParameterNumber* angegebene Wert ist kleiner als 1.|  
|HY000|Allgemeiner Fehler|Es ist ein Fehler aufgetreten, bei dem kein spezifischer SQLSTATE vorhanden war und für den kein Implementierungs spezifischer SQLSTATE definiert wurde. Die von **SQLGetDiagRec** im **MessageText* -Puffer zurückgegebene Fehlermeldung beschreibt den Fehler und seine Ursache.|  
|HY001|Fehler bei der Speicher Belegung|Der Treiber konnte keinen Arbeitsspeicher zuweisen, der zur Unterstützung der Ausführung oder Beendigung der Funktion erforderlich ist.|  
|HY003|Ungültiger Anwendungs Puffertyp.|Der durch das Argument *ValueType* angegebene Wert war kein gültiger C-Datentyp oder SQL_C_DEFAULT.|  
|HY004|Ungültiger SQL-Datentyp.|Der für den Argument *ParameterType* angegebene Wert war weder ein gültiger ODBC-SQL-Datentyp Bezeichner noch ein Treiber spezifischer SQL-Datentyp Bezeichner, der vom Treiber unterstützt wird.|  
|HY009|Ungültiger Argument Wert.|(DM) das Argument *ParameterValuePtr* war ein NULL-Zeiger, das Argument *StrLen_or_IndPtr* war ein NULL-Zeiger, und das *inputoutputtype* -Argument war nicht SQL_PARAM_OUTPUT.<br /><br /> (DM) SQL_PARAM_OUTPUT, wobei das Argument *ParameterValuePtr* ein NULL-Zeiger war, der C-Typ char oder Binary war und die BufferLength (*cbvaluemax*) größer als 0 ist.|  
|HY010|Funktions Sequenz Fehler|(DM) eine asynchron ausgeführte Funktion wurde für das Verbindungs Handle aufgerufen, das mit dem *StatementHandle* verknüpft ist. Diese asynchrone Funktion wurde noch ausgeführt, als **SQLBindParameter** aufgerufen wurde.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect** oder **SQLMoreResults** wurde für das *StatementHandle* aufgerufen und SQL_PARAM_DATA_AVAILABLE zurückgegeben. Diese Funktion wurde aufgerufen, bevor Daten für alle gestreuten Parameter abgerufen wurden.<br /><br /> (DM) eine asynchron ausgeführte Funktion wurde für das *StatementHandle* aufgerufen und ausgeführt, als diese Funktion aufgerufen wurde.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** oder **SQLSetPos** wurde für das *StatementHandle* aufgerufen und SQL_NEED_DATA zurückgegeben. Diese Funktion wurde aufgerufen, bevor Daten für alle Data-at-Execution-Parameter oder-Spalten gesendet wurden.|  
|HY013|Speicher Verwaltungsfehler|Der Funktions Aufrufwert konnte nicht verarbeitet werden, da auf die zugrunde liegenden Speicher Objekte nicht zugegriffen werden konnte, möglicherweise aufgrund von wenig Arbeitsspeicher.|  
|HY021|Inkonsistente Deskriptorinformationen|Die während einer Konsistenzprüfung überprüften Deskriptorinformationen waren nicht konsistent. (Weitere Informationen finden Sie im Abschnitt "Konsistenzprüfungen" in **SQLSetDescField**.)<br /><br /> Der für das Argument *DecimalDigits* angegebene Wert lag außerhalb des Bereichs der Werte, die von der Datenquelle für eine Spalte des SQL-Datentyps, der durch das *Parameter Type* -Argument angegeben wird, unterstützt wird.|  
|HY090|Ungültige Zeichen folgen-oder Pufferlänge|(DM) der Wert in *BufferLength* war kleiner als 0 (null). (Weitere Informationen finden Sie in der Beschreibung des SQL_DESC_DATA_PTR Felds in **SQLSetDescField**.)|  
|HY104|Ungültiger Wert für Genauigkeit oder Dezimalwert|Der für das Argument *ColumnSize* oder *DecimalDigits* angegebene Wert lag außerhalb des Bereichs der Werte, die von der Datenquelle für eine Spalte des SQL-Datentyps, der durch das *Parameter Type* -Argument angegeben wird, unterstützt wird.|  
|HY105|Ungültiger Parametertyp|(DM) der für das *inputoutputtype* -Argument angegebene Wert ist ungültig. (Siehe "Kommentare")|  
|HY117|Die Verbindung wurde aufgrund eines unbekannten Transaktions Zustands angehalten. Nur Disconnect-und Read-Only-Funktionen sind zulässig.|(DM) Weitere Informationen zum angehaltenen Status finden Sie unter [SQLEndTran Function](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Optionales Feature nicht implementiert|Der Treiber oder die Datenquelle unterstützt nicht die durch die Kombination des Werts, der für das Argument *ValueType* angegebene ist, und den treiberspezifischen Wert, der für das Argument *ParameterType* angegeben wurde.<br /><br /> Der für das Argument *ParameterType* angegebene Wert war ein gültiger ODBC-SQL-Datentyp Bezeichner für die Version von ODBC, die vom Treiber unterstützt, jedoch nicht vom Treiber oder von der Datenquelle unterstützt wurde.<br /><br /> Der Treiber unterstützt nur ODBC 2. *x* und das Argument *ValueType* waren eine der folgenden:<br /><br /> SQL_C_NUMERIC SQL_C_SBIGINT SQL_C_UBIGINT<br /><br /> und alle in [c-Datentypen](../../../odbc/reference/appendixes/c-data-types.md) aufgelisteten Interval c-Datentypen in Anhang D: Datentypen.<br /><br /> Der Treiber unterstützt nur ODBC-Versionen vor 3,50, und das Argument *ValueType* war SQL_C_GUID.|  
|HYT01|Verbindungs Timeout abgelaufen|Der Verbindungs Timeout Zeitraum ist abgelaufen, bevor die Datenquelle auf die Anforderung geantwortet hat. Der Timeout Zeitraum für die Verbindung wird über **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT festgelegt.|  
|IM001|Der Treiber unterstützt diese Funktion nicht.|(DM) der dem *StatementHandle* zugeordnete Treiber unterstützt die-Funktion nicht.|  
  
## <a name="comments"></a>Kommentare

 Eine Anwendung ruft **SQLBindParameter** auf, um jede Parameter Markierung in einer SQL-Anweisung zu binden. Bindungen bleiben wirksam, bis die Anwendung **SQLBindParameter** erneut aufruft, **SQLFreeStmt** mit der SQL_RESET_PARAMS-Option aufruft oder **SQLSetDescField** aufruft, um das SQL_DESC_COUNT Header-Feld der APD auf 0 festzulegen.  
  
 Weitere Informationen zu Parametern finden Sie unter [Anweisungs Parameter](../../../odbc/reference/develop-app/statement-parameters.md). Weitere Informationen zu Parameter Datentypen und Parameter Markierungen finden Sie unter [Parameter Datentypen](../../../odbc/reference/appendixes/parameter-data-types.md) und [Parameter Markierungen](../../../odbc/reference/appendixes/parameter-markers.md) in Anhang C: SQL-Grammatik.  
  
## <a name="parameternumber-argument"></a>ParameterNumber-Argument  
 Wenn *ParameterNumber* im Aufruf von **SQLBindParameter** größer als der Wert von SQL_DESC_COUNT ist, wird **SQLSetDescField** aufgerufen, um den Wert von SQL_DESC_COUNT auf *ParameterNumber* zu erhöhen.  
  
## <a name="inputoutputtype-argument"></a>Inputoutputtype-Argument  
 Das *inputoutputtype* -Argument gibt den Typ des Parameters an. Dieses Argument legt das SQL_DESC_PARAMETER_TYPE-Feld der IPD fest. Alle Parameter in SQL-Anweisungen, die keine Prozeduren (z. b. **Insert** -Anweisungen) aufzurufen, sind *Eingabe* *Parameter*. Parameter in Prozedur aufrufen können Eingabe-, Eingabe-/Ausgabe-oder Ausgabeparameter sein. (Eine Anwendung ruft **sqlprocedurecolrens** auf, um den Typ eines Parameters in einem Prozedur Aufruf zu bestimmen; Parameter, deren Typ nicht bestimmt werden kann, werden als Eingabeparameter angenommen.)  
  
 Das *InputOutputType*-Argument ist einer der folgenden Werte:  
  
-   SQL_PARAM_INPUT. Der-Parameter markiert einen Parameter in einer SQL-Anweisung, die keine Prozedur aufruft, wie z. b. eine **Insert** -Anweisung, oder markiert einen Eingabeparameter in einer Prozedur. Beispielsweise sind die Parameter in **INSERT INTO Employee Values (?,?,?)** Eingabeparameter, wohingegen die Parameter in **{calladdemp (?,?,?)}** , aber nicht notwendigerweise, Eingabeparameter sein können.  
  
     Wenn die-Anweisung ausgeführt wird, sendet der Treiber Daten für den Parameter an die Datenquelle. der \* *ParameterValuePtr* -Puffer muss einen gültigen Eingabe Wert enthalten, oder der **StrLen_or_IndPtr* Puffer muss SQL_NULL_DATA, SQL_DATA_AT_EXEC oder das Ergebnis des SQL_LEN_DATA_AT_EXEC Makros enthalten.  
  
     Wenn eine Anwendung den Typ eines Parameters in einem Prozedur Aufrufsatz nicht ermitteln kann, wird *inputoutputtype* auf SQL_PARAM_INPUT festgelegt. Wenn die Datenquelle einen Wert für den Parameter zurückgibt, verwirft der Treiber den Wert.  
  
-   SQL_PARAM_INPUT_OUTPUT. Der-Parameter markiert einen Eingabe-/Ausgabeparameter in einer Prozedur. Beispielsweise ist der Parameter in **{callgetempdept (?)}** ein Eingabe-/Ausgabeparameter, der den Namen eines Mitarbeiters annimmt und den Namen der Abteilung des Mitarbeiters zurückgibt.  
  
     Wenn die-Anweisung ausgeführt wird, sendet der Treiber Daten für den Parameter an die Datenquelle. der \* *ParameterValuePtr* -Puffer muss einen gültigen Eingabe Wert enthalten, oder der \* *StrLen_or_IndPtr* Puffer muss SQL_NULL_DATA, SQL_DATA_AT_EXEC oder das Ergebnis des SQL_LEN_DATA_AT_EXEC Makros enthalten. Nachdem die-Anweisung ausgeführt wurde, gibt der Treiber Daten für den Parameter an die Anwendung zurück. Wenn die Datenquelle keinen Wert für einen Eingabe-/Ausgabeparameter zurückgibt, legt der Treiber den **StrLen_or_IndPtr* Puffer auf SQL_NULL_DATA fest.  
  
    > [!NOTE]  
    >  Wenn eine ODBC 1,0-Anwendung **SQLSetParam** in einem ODBC 2,0-Treiber aufruft, konvertiert der Treiber-Manager diesen in einen Aufruf von **SQLBindParameter** , bei dem das *inputoutputtype* -Argument auf SQL_PARAM_INPUT_OUTPUT festgelegt ist.  
  
-   SQL_PARAM_OUTPUT. Der-Parameter markiert den Rückgabewert einer Prozedur oder eines Output-Parameters in einer Prozedur. in beiden Fällen werden diese als *Ausgabeparameter* bezeichnet. Beispielsweise ist der-Parameter in **{? = getnextempid}** ein Ausgabeparameter, der die nächste Mitarbeiter-ID zurückgibt.  
  
     Nachdem die-Anweisung ausgeführt wurde, gibt der Treiber Daten für den Parameter an die Anwendung zurück, es sei denn, die Parameter " *ParameterValuePtr* " und " *StrLen_or_IndPtr* " sind beide NULL-Zeiger. in diesem Fall verwirft der Treiber den Ausgabewert. Wenn die Datenquelle keinen Wert für einen Output-Parameter zurückgibt, legt der Treiber den **StrLen_or_IndPtr* Puffer auf SQL_NULL_DATA fest.  
  
-   SQL_PARAM_INPUT_OUTPUT_STREAM. Gibt an, dass ein Eingabe-/Ausgabeparameter gestreamt werden soll **SQLGetData** kann Parameterwerte in Teilen lesen. *BufferLength* wird ignoriert, da die Pufferlänge beim Aufrufen von **SQLGetData** bestimmt wird. Der Wert des *StrLen_or_IndPtr* Puffers muss SQL_NULL_DATA, SQL_DEFAULT_PARAM, SQL_DATA_AT_EXEC oder das Ergebnis des SQL_LEN_DATA_AT_EXEC Makros enthalten. Ein Parameter muss bei der Eingabe als ein Data-at-Execution-Parameter (DAE) gebunden werden, wenn er bei der Ausgabe gestreamt wird. *ParameterValuePtr* kann ein beliebiger nicht-NULL-Zeiger Wert sein, der von **SQLParamData** als benutzerdefiniertes Token zurückgegeben wird, dessen Wert sowohl für die Eingabe als auch für die Ausgabe mit *ParameterValuePtr* übermittelt wurde. Weitere Informationen finden Sie unter [Abrufen von Ausgabeparametern mit SQLGetData](../../../odbc/reference/develop-app/retrieving-output-parameters-using-sqlgetdata.md).  
  
-   SQL_PARAM_OUTPUT_STREAM. Identisch mit SQL_PARAM_INPUT_OUTPUT_STREAM für einen Output-Parameter. **StrLen_or_IndPtr* wird bei der Eingabe ignoriert.  
  
 In der folgenden Tabelle sind die verschiedenen Kombinationen von *inputoutputtype* und **StrLen_or_IndPtr* aufgeführt:  
  
|*InputOutputType*|**StrLen_or_IndPtr*|Ergebnis|Anmerkung zu ParameterValuePtr|  
|-----------------------|----------------------------|-------------|---------------------------------|  
|SQL_PARAM_INPUT|SQL_LEN_DATA_AT_EXEC (*len*) oder SQL_DATA_AT_EXEC|Eingabe in Teilen|*ParameterValuePtr* kann ein beliebiger Zeiger Wert sein, der von **SQLParamData** als benutzerdefiniertes Token zurückgegeben wird, dessen Wert mit *ParameterValuePtr* übermittelt wurde.|  
|SQL_PARAM_INPUT|Nicht SQL_LEN_DATA_AT_EXEC (*len*) oder SQL_DATA_AT_EXEC|Eingabe gebundener Puffer|*ParameterValuePtr* ist die Adresse des Eingabe Puffers.|  
|SQL_PARAM_OUTPUT|Bei Eingabe ignoriert.|Ausgabe gebundener Puffer|*ParameterValuePtr* ist die Adresse des Ausgabepuffers.|  
|SQL_PARAM_OUTPUT_STREAM|Bei Eingabe ignoriert.|Streaming-Ausgabe|*ParameterValuePtr* kann ein beliebiger Zeiger Wert sein, der von **SQLParamData** als benutzerdefiniertes Token zurückgegeben wird, dessen Wert mit *ParameterValuePtr* übermittelt wurde.|  
|SQL_PARAM_INPUT_OUTPUT|SQL_LEN_DATA_AT_EXEC (*len*) oder SQL_DATA_AT_EXEC|Eingabe in Teile und Ausgabe gebundenen Puffer|*ParameterValuePtr* ist die Adresse des Ausgabepuffers, die von **SQLParamData** auch als benutzerdefiniertes Token zurückgegeben wird, dessen Wert mit *ParameterValuePtr* übermittelt wurde.|  
|SQL_PARAM_INPUT_OUTPUT|Nicht SQL_LEN_DATA_AT_EXEC (*len*) oder SQL_DATA_AT_EXEC|Eingabe-und Ausgabe gebundener Puffer|*ParameterValuePtr* ist die Adresse des freigegebenen Eingabe-/Ausgabepuffers.|
|SQL_PARAM_INPUT_OUTPUT_STREAM|SQL_LEN_DATA_AT_EXEC (*len*) oder SQL_DATA_AT_EXEC|Eingabe in Teilen und Streamen der Ausgabe|*ParameterValuePtr* kann ein beliebiger nicht-NULL-Zeiger Wert sein, der von **SQLParamData** als benutzerdefiniertes Token zurückgegeben wird, dessen Wert sowohl für die Eingabe als auch für die Ausgabe mit *ParameterValuePtr* übermittelt wurde.|  
  
> [!NOTE]  
>  Der Treiber muss entscheiden, welche SQL-Typen zulässig sind, wenn eine Anwendung einen Output-oder Input-Output-Parameter als gestreamt bindet. Der Treiber-Manager generiert keinen Fehler für einen ungültigen SQL-Typ.  
  
## <a name="valuetype-argument"></a>ValueType-Argument

 Das *ValueType* -Argument gibt den C-Datentyp des Parameters an. Mit diesem Argument werden die Felder SQL_DESC_TYPE, SQL_DESC_CONCISE_TYPE und SQL_DESC_DATETIME_INTERVAL_CODE der APD festgelegt. Dies muss einer der Werte im Abschnitt C- [Datentypen](../../../odbc/reference/appendixes/c-data-types.md) von Anhang D: Datentypen sein.  
  
 Wenn das *ValueType* -Argument einer der Intervall Datentypen ist, wird das SQL_DESC_TYPE-Feld des *ParameterNumber* -Datensatzes der APD auf SQL_INTERVAL festgelegt. das SQL_DESC_CONCISE_TYPE-Feld des APD wird auf den Datentyp "präziser Interval" festgelegt, und das SQL_DESC_DATETIME_INTERVAL_CODE Feld des *ParameterNumber* -Datensatzes wird auf einen Subcode für den spezifischen Interval-Datentyp festgelegt (Siehe [Anhang D: Datentypen](../../../odbc/reference/appendixes/appendix-d-data-types.md).) Die standardmäßige Intervall Genauigkeit (2) und Default interval seconds (6), wie in den Feldern "SQL_DESC_DATETIME_INTERVAL_PRECISION" und "SQL_DESC_PRECISION" des APD festgelegt, werden für die Daten verwendet. Wenn eine der beiden Standardgenauigkeit nicht geeignet ist, sollte die Anwendung das Deskriptorfeld explizit durch einen **SQLSetDescField** -oder **SQLSetDescRec**-Befehl festlegen.  
  
 Wenn das *ValueType* -Argument einer der DateTime-Datentypen ist, das SQL_DESC_TYPE-Feld des *ParameterNumber* -Datensatzes der APD ist auf SQL_DATETIME festgelegt, das SQL_DESC_CONCISE_TYPE Feld des *ParameterNumber* -Datensatzes der APD wird auf den präzisen DateTime-C-Datentyp festgelegt, und das SQL_DESC_DATETIME_INTERVAL_CODE-Feld des *ParameterNumber* -Datensatzes wird auf einen Subcode für den spezifischen DateTime-Datentyp festgelegt. (Siehe [Anhang D: Datentypen](../../../odbc/reference/appendixes/appendix-d-data-types.md).)  
  
 Wenn das *ValueType* -Argument ein SQL_C_NUMERIC-Datentyp ist, werden die Standardgenauigkeit (die vom Treiber definiert ist) und die Standardskala (0), die in den SQL_DESC_PRECISION-und SQL_DESC_SCALE-Feldern der APD festgelegt sind, für die Daten verwendet. Wenn die Standardgenauigkeit oder-Skala nicht geeignet ist, sollte die Anwendung das Deskriptorfeld explizit durch einen **SQLSetDescField** -oder **SQLSetDescRec**-Befehl festlegen.  
  
 SQL_C_DEFAULT gibt an, dass der Parameterwert aus dem C-Standard Datentyp für den mit *ParameterType* angegebenen SQL-Datentyp übertragen werden soll.  
  
 Sie können auch einen erweiterten C-Datentyp angeben. Weitere Informationen finden Sie unter [C-Datentypen in ODBC](../../../odbc/reference/develop-app/c-data-types-in-odbc.md).  
  
 Weitere Informationen finden Sie unter [Standard-c-Datentypen](../../../odbc/reference/appendixes/default-c-data-types.md), [wandeln von Daten aus C in SQL-Datentypen](../../../odbc/reference/appendixes/converting-data-from-c-to-sql-data-types.md)und umrechnen [von Daten aus SQL in C-Daten](../../../odbc/reference/appendixes/converting-data-from-sql-to-c-data-types.md) Typen in Anhang D: Datentypen.  
  
## <a name="parametertype-argument"></a>ParameterType-Argument

 Dies muss einer der Werte sein, die im Abschnitt [SQL-Datentypen](../../../odbc/reference/appendixes/sql-data-types.md) von Anhang D: Datentypen aufgeführt sind, oder es muss sich um einen treiberspezifischen Wert handeln. Dieses Argument legt die Felder SQL_DESC_TYPE, SQL_DESC_CONCISE_TYPE und SQL_DESC_DATETIME_INTERVAL_CODE der IPD fest.  
  
 Wenn das *ParameterType* -Argument einer der DateTime-Bezeichner ist, wird das SQL_DESC_TYPE-Feld der IPD auf SQL_DATETIME festgelegt, das SQL_DESC_CONCISE_TYPE-Feld der IPD auf den präzisen DateTime-SQL-Datentyp festgelegt, und das SQL_DESC_DATETIME_INTERVAL_CODE Feld wird auf den entsprechenden DateTime-Subcodewert festgelegt.  
  
 Wenn *ParameterType* einer der Intervall Bezeichner ist, wird das SQL_DESC_TYPE-Feld der IPD auf SQL_INTERVAL festgelegt. das SQL_DESC_CONCISE_TYPE-Feld der IPD wird auf den Datentyp "präziser SQL Interval" festgelegt, und das SQL_DESC_DATETIME_INTERVAL_CODE-Feld der IPD wird auf den entsprechenden intervallsubcode festgelegt. Das SQL_DESC_DATETIME_INTERVAL_PRECISION-Feld der IPD wird auf die angegebene Intervall Genauigkeit festgelegt, und das SQL_DESC_PRECISION Feld wird ggf. auf die Genauigkeit des Intervalls Sekunden festgelegt. Wenn der Standardwert SQL_DESC_DATETIME_INTERVAL_PRECISION oder SQL_DESC_PRECISION nicht angemessen ist, sollte die Anwendung diese explizit festlegen, indem Sie **SQLSetDescField** aufruft. Weitere Informationen zu diesen Feldern finden Sie unter [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md).  
  
 Wenn das *ValueType* -Argument ein SQL_NUMERIC-Datentyp ist, werden die Standardgenauigkeit (die vom Treiber definiert ist) und die Standardskala (0), die in den SQL_DESC_PRECISION-und SQL_DESC_SCALE-Feldern der IPD festgelegt sind, für die Daten verwendet. Wenn die Standardgenauigkeit oder-Skala nicht geeignet ist, sollte die Anwendung das Deskriptorfeld explizit durch einen **SQLSetDescField** -oder **SQLSetDescRec**-Befehl festlegen.  
  
 Informationen zum Konvertieren von Daten finden [Sie unter Konvertieren von Daten aus C-in SQL-Datentypen](../../../odbc/reference/appendixes/converting-data-from-c-to-sql-data-types.md) und [Konvertieren von Daten aus SQL](../../../odbc/reference/appendixes/converting-data-from-sql-to-c-data-types.md) -in c-Datentypen in Anhang D: Datentypen.  
  
## <a name="columnsize-argument"></a>ColumnSize-Argument

 Das *ColumnSize* -Argument gibt die Größe der Spalte oder des Ausdrucks an, die der Parameter Markierung, der Länge der Daten oder beiden entspricht. Dieses Argument legt abhängig vom SQL-Datentyp (dem *ParameterType* -Argument) verschiedene Felder der IPD fest. Die folgenden Regeln gelten für diese Zuordnung:  
  
-   Wenn *ParameterType* SQL_CHAR, SQL_VARCHAR, SQL_LONGVARCHAR, SQL_BINARY, SQL_VARBINARY, SQL_LONGVARBINARY oder einer der präzisen SQL DateTime-oder interval-Datentypen ist, wird das SQL_DESC_LENGTH-Feld der IPD auf den Wert von *ColumnSize* festgelegt. (Weitere Informationen finden Sie im Abschnitt [Spaltengröße, Dezimalziffern, Länge von Übertragungs Oktett und Anzeige Größe](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) in Anhang D: Datentypen.)  
  
-   Wenn *ParameterType* SQL_DECIMAL, SQL_NUMERIC, SQL_FLOAT, SQL_REAL oder SQL_DOUBLE ist, wird das SQL_DESC_PRECISION-Feld der IPD auf den Wert von *ColumnSize* festgelegt.  
  
-   Bei anderen Datentypen wird das *ColumnSize* -Argument ignoriert.  
  
 Weitere Informationen finden Sie unter "übergeben von Parameter Werten" und SQL_DATA_AT_EXEC im Argument "*StrLen_or_IndPtr* ".  
  
## <a name="decimaldigits-argument"></a>DecimalDigits-Argument

 Wenn *ParameterType* SQL_TYPE_TIME, SQL_TYPE_TIMESTAMP, SQL_INTERVAL_SECOND, SQL_INTERVAL_DAY_TO_SECOND, SQL_INTERVAL_HOUR_TO_SECOND oder SQL_INTERVAL_MINUTE_TO_SECOND ist, wird das SQL_DESC_PRECISION-Feld der IPD auf *DecimalDigits* festgelegt. Wenn *ParameterType* SQL_NUMERIC oder SQL_DECIMAL ist, wird das Feld SQL_DESC_SCALE der IPD auf *DecimalDigits* festgelegt. Für alle anderen Datentypen wird das *DecimalDigits* -Argument ignoriert.  
  
## <a name="parametervalueptr-argument"></a>ParameterValuePtr-Argument

 Das *ParameterValuePtr* -Argument verweist auf einen Puffer, der, wenn **SQLExecute** oder **SQLExecDirect** aufgerufen wird, die eigentlichen Daten für den Parameter enthält. Die Daten müssen in der Form vorliegen, die vom *ValueType* -Argument angegeben wird. Mit diesem Argument wird das SQL_DESC_DATA_PTR-Feld der APD festgelegt. Eine Anwendung kann das *ParameterValuePtr* -Argument auf einen NULL-Zeiger festlegen, sofern *\* StrLen_or_IndPtr* SQL_NULL_DATA oder SQL_DATA_AT_EXEC ist. (Dies gilt nur für Eingabe-oder Eingabe-/Ausgabeparameter.)  
  
 Wenn \* *StrLen_or_IndPtr* das Ergebnis des SQL_LEN_DATA_AT_EXEC (*length*)-Makros oder SQL_DATA_AT_EXEC ist, dann ist *ParameterValuePtr* ein Anwendungs definierter Zeiger Wert, der dem-Parameter zugeordnet ist. Sie wird über **SQLParamData** an die Anwendung zurückgegeben. *ParameterValuePtr* kann z. b. ein Token ungleich 0 (null) sein, z. b. eine Parameter Nummer, ein Zeiger auf Daten oder ein Zeiger auf eine-Struktur, die von der Anwendung verwendet wurde, um Eingabeparameter zu binden. Beachten Sie jedoch Folgendes: Wenn der Parameter ein Eingabe-/Ausgabeparameter ist, muss *ParameterValuePtr* ein Zeiger auf einen Puffer sein, in dem der Ausgabewert gespeichert wird. Wenn der Wert im SQL_ATTR_PARAMSET_SIZE Statement-Attribut größer als 1 ist, kann die Anwendung den Wert, auf den das SQL_ATTR_PARAMS_PROCESSED_PTR Statement-Attribut zeigt, mit dem *ParameterValuePtr* -Argument verwenden. *ParameterValuePtr* könnte z. b. auf ein Array von-Werten verweisen, und die Anwendung verwendet möglicherweise den Wert, auf den SQL_ATTR_PARAMS_PROCESSED_PTR zeigt, um den korrekten Wert aus dem Array abzurufen. Weitere Informationen finden Sie weiter unten in diesem Abschnitt unter "übergeben von Parameter Werten".  
  
 Wenn das *inputoutputtype* -Argument SQL_PARAM_INPUT_OUTPUT oder SQL_PARAM_OUTPUT ist, verweist *ParameterValuePtr* auf einen Puffer, in dem der Treiber den Ausgabewert zurückgibt. Wenn die Prozedur ein oder mehrere Resultsets zurückgibt, wird der \* *ParameterValuePtr* -Puffer nicht garantiert festgelegt, bis alle Resultsets/Zeilen Anzahlen verarbeitet wurden. Wenn der Puffer erst festgelegt wird, wenn die Verarbeitung beendet ist, sind die Ausgabeparameter und Rückgabewerte nicht verfügbar, bis **SQLMoreResults** SQL_NO_DATA zurückgibt. Das Aufrufen von **SQLCloseCursor** oder **SQLFreeStmt** mit der Option SQL_CLOSE bewirkt, dass diese Werte verworfen werden.  
  
 Wenn der Wert im SQL_ATTR_PARAMSET_SIZE Statement-Attribut größer als 1 ist, verweist *ParameterValuePtr* auf ein Array. Eine einzelne SQL-Anweisung verarbeitet das gesamte Array von Eingabe Werten für einen Eingabe-oder Eingabe-/Ausgabeparameter und gibt ein Array von Ausgabe Werten für einen Eingabe-/Ausgabe-oder Ausgabeparameter zurück.  
  
## <a name="bufferlength-argument"></a>BufferLength-Argument

 Für Zeichen-und binäre C-Daten gibt das *BufferLength* -Argument die Länge des \* *ParameterValuePtr* -Puffers (wenn es sich um ein einzelnes Element handelt) oder die Länge eines Elements im \* *ParameterValuePtr* -Array an (wenn der Wert im SQL_ATTR_PARAMSET_SIZE Statement-Attribut größer als 1 ist). Mit diesem Argument wird das SQL_DESC_OCTET_LENGTH Datensatz-Feld der APD festgelegt. Wenn die Anwendung mehrere Werte angibt, wird *BufferLength* verwendet, um den Speicherort der Werte im **ParameterValuePtr* -Array sowohl bei der Eingabe als auch bei der Ausgabe zu bestimmen. Für Eingabe-/Ausgabe-und Ausgabeparameter wird verwendet, um zu bestimmen, ob Zeichen-und binäre C-Daten bei der Ausgabe abgeschnitten werden sollen:  
  
-   Wenn die Anzahl von Bytes, die für die Rückgabe verfügbar sind, größer oder gleich *BufferLength* ist, werden die Daten in \* *ParameterValuePtr* auf die Länge eines NULL  -Beendigungs Zeichens gekürzt und vom Treiber auf NULL-terminierte Zeichen gekürzt.  
  
-   Wenn die Anzahl von Bytes, die für die Rückgabe verfügbar sind, größer als *BufferLength* ist, werden die Daten in \* *ParameterValuePtr* für binäre C-Daten auf *BufferLength* -Bytes gekürzt.  
  
 Für alle anderen Typen von C-Daten wird das *BufferLength* -Argument ignoriert. Die Länge des \* *ParameterValuePtr* -Puffers (wenn es sich um ein einzelnes Element handelt) oder die Länge eines Elements im \* *ParameterValuePtr* -Array (wenn die Anwendung **SQLSetStmtAttr** mit einem *Attribut* Argument SQL_ATTR_PARAMSET_SIZE von aufruft, um mehrere Werte für jeden Parameter anzugeben), wird angenommen, dass es sich um die Länge des C-Datentyps handelt.  
  
 Für streamingausgaben oder per Streaming übertragene Eingabe-/Ausgabeparameter wird das *BufferLength* -Argument ignoriert, da die Pufferlänge in **SQLGetData** angegeben wird.  
  
> [!NOTE]  
>  Wenn eine ODBC 1,0-Anwendung **SQLSetParam** in einem ODBC 3 aufruft. *x* -Treiber: der Treiber-Manager konvertiert diesen in einen **SQLBindParameter** -Befehl, in dem das *BufferLength* -Argument immer SQL_SETPARAM_VALUE_MAX ist. Der Treiber-Manager gibt bei ODBC 3 einen Fehler zurück. in der *x* -Anwendung wird *BufferLength* auf SQL_SETPARAM_VALUE_MAX festgelegt, ein ODBC 3. der *x* -Treiber kann dies verwenden, um zu bestimmen, wann er von einer ODBC 1,0-Anwendung aufgerufen wird.  
  
> [!NOTE]  
>  In **SQLSetParam** gibt die Art und Weise, in der eine Anwendung die Länge des **ParameterValuePtr* -Puffers angibt, damit der Treiber Zeichen-oder Binärdaten zurückgeben kann, und die Art und Weise, wie eine Anwendung ein Array von Zeichen-oder binären Parameterwerten an den Treiber sendet, Treiber definiert.  
  
## <a name="strlen_or_indptr-argument"></a>StrLen_or_IndPtr-Argument

 Das *StrLen_or_IndPtr* -Argument verweist auf einen Puffer, der, wenn **SQLExecute** oder **SQLExecDirect** aufgerufen wird, eine der folgenden enthält. (Dieses Argument legt die Felder SQL_DESC_OCTET_LENGTH_PTR und SQL_DESC_INDICATOR_PTR Datensatz der Anwendungsparameter Zeiger fest.)  
  
-   Die Länge des in **ParameterValuePtr* gespeicherten Parameter Werts. Dies wird ignoriert, außer für Zeichen-oder binäre C-Daten.  
  
-   SQL_NTS. Der Parameterwert ist eine NULL-terminierte Zeichenfolge.  
  
-   SQL_NULL_DATA. Der Parameterwert ist NULL.  
  
-   SQL_DEFAULT_PARAM. Eine Prozedur besteht darin, den Standardwert eines Parameters anstelle eines Werts zu verwenden, der aus der Anwendung abgerufen wird. Dieser Wert ist nur in einer in der kanonischen ODBC-Syntax aufgerufenen Prozedur und dann nur dann gültig, wenn das *inputoutputtype* -Argument SQL_PARAM_INPUT, SQL_PARAM_INPUT_OUTPUT oder SQL_PARAM_INPUT_OUTPUT_STREAM ist. Wenn \* *StrLen_or_IndPtr* SQL_DEFAULT_PARAM ist, werden die Argumente *ValueType*, *ParameterType*, *ColumnSize*, *DecimalDigits*, *BufferLength* und *ParameterValuePtr* bei Eingabe Parametern ignoriert und nur zum Definieren des Ausgabeparameter Werts für Eingabe-/Ausgabeparameter verwendet.  
  
-   Das Ergebnis des SQL_LEN_DATA_AT_EXEC (*length*)-Makros. Die Daten für den Parameter werden mit **SQLPutData** gesendet. Wenn das *Parameter Type* -Argument SQL_LONGVARBINARY, SQL_LONGVARCHAR oder ein langer Datenquellen spezifischer Datentyp ist und der Treiber "Y" für den SQL_NEED_LONG_DATA_LEN-Informationstyp in **SQLGetInfo** zurückgibt, ist *length* die Anzahl der Daten bytes, die für den Parameter gesendet werden sollen. Andernfalls muss *length* ein nicht negativer Wert sein und wird ignoriert. Weitere Informationen finden Sie unter "übergeben von Parameter Werten" weiter unten in diesem Abschnitt.  
  
     Um z. b. anzugeben, dass 10.000 Bytes an Daten mit **SQLPutData** in einem oder mehreren Aufrufen gesendet werden, legt eine Anwendung für einen SQL_LONGVARCHAR-Parameter **StrLen_or_IndPtr* auf SQL_LEN_DATA_AT_EXEC (10000) fest.  
  
-   SQL_DATA_AT_EXEC. Die Daten für den Parameter werden mit **SQLPutData** gesendet. Dieser Wert wird von ODBC 1,0-Anwendungen verwendet, wenn er ODBC 3 aufruft. *x* -Treiber. Weitere Informationen finden Sie unter "übergeben von Parameter Werten" weiter unten in diesem Abschnitt.  
  
 Wenn *StrLen_or_IndPtr* ein NULL-Zeiger ist, geht der Treiber davon aus, dass alle Eingabeparameter Werte ungleich NULL sind und dass Zeichen-und Binärdaten mit Null enden. Wenn *inputoutputtype* SQL_PARAM_OUTPUT oder SQL_PARAM_OUTPUT_STREAM und *ParameterValuePtr* und *StrLen_or_IndPtr* beide NULL-Zeiger sind, verwirft der Treiber den Ausgabewert.  
  
> [!NOTE]  
>  Anwendungsentwickler werden dringend davon abgeraten, einen NULL-Zeiger für *StrLen_or_IndPtr* anzugeben, wenn der Datentyp des Parameters SQL_C_BINARY ist. Um sicherzustellen, dass ein Treiber SQL_C_BINARY Daten nicht unerwartet abschneidet, sollte *StrLen_or_IndPtr* einen Zeiger auf einen gültigen Längen Wert enthalten.  
  
 Wenn das *inputoutputtype* -Argument SQL_PARAM_INPUT_OUTPUT, SQL_PARAM_OUTPUT, SQL_PARAM_INPUT_OUTPUT_STREAM oder SQL_PARAM_OUTPUT_STREAM ist, zeigt *StrLen_or_IndPtr* auf einen Puffer, in dem der Treiber SQL_NULL_DATA zurückgibt, die Anzahl der Bytes, die in \* *ParameterValuePtr* (mit Ausnahme des NULL-Terminierungs Byte der Zeichendaten) zurückgegeben werden können, oder SQL_NO_TOTAL (wenn die Anzahl der verfügbaren Bytes nicht bestimmt werden kann). Wenn die Prozedur ein oder mehrere Resultsets zurückgibt, wird der **StrLen_or_IndPtr* Puffer nicht garantiert festgelegt, bis alle Ergebnisse abgerufen wurden.  
  
 Wenn der Wert im SQL_ATTR_PARAMSET_SIZE Statement-Attribut größer als 1 ist, zeigt *StrLen_or_IndPtr* auf ein Array von sqllen-Werten. Dabei kann es sich um einen der weiter oben in diesem Abschnitt aufgeführten Werte handeln, der mit einer einzelnen SQL-Anweisung verarbeitet wird.  
  
## <a name="passing-parameter-values"></a>Übergeben von Parameter Werten

 Eine Anwendung kann den Wert für einen Parameter entweder im \* *ParameterValuePtr* -Puffer oder mit einem oder mehreren Aufrufen von **SQLPutData** übergeben. Parameter, deren Daten mit **SQLPutData** übermittelt werden, werden als *Data-at-Execution-* Parameter bezeichnet. Diese werden in der Regel zum Senden von Daten für SQL_LONGVARBINARY-und SQL_LONGVARCHAR-Parameter verwendet und können mit anderen Parametern gemischt werden.  
  
 Zum Übergeben von Parameterwerten führt eine Anwendung die folgenden Schritte aus:  
  
1.  Ruft **SQLBindParameter** für jeden Parameter auf, um Puffer für den Wert des Parameters (*ParameterValuePtr* -Argument) und length/Indicator (*StrLen_or_IndPtr* -Argument) zu binden. Bei Data-at-Execution-Parametern ist *ParameterValuePtr* ein von der Anwendung definierter Zeiger Wert, z. b. eine Parameter Nummer oder ein Zeiger auf Daten. Der Wert wird später zurückgegeben und kann verwendet werden, um den Parameter zu identifizieren.  
  
2.  Legt Werte für Eingabe-und Eingabe-/Ausgabeparameter im \* *ParameterValuePtr* -und **StrLen_or_IndPtr* -Puffer ab:  
  
    -   Bei normalen Parametern platziert die Anwendung den Parameterwert im \* *ParameterValuePtr* -Puffer und die Länge dieses Werts im **StrLen_or_IndPtr* Puffer. Weitere Informationen finden Sie unter [Festlegen von Parameter Werten](../../../odbc/reference/develop-app/setting-parameter-values.md).  
  
    -   Bei Data-at-Execution-Parametern legt die Anwendung das Ergebnis des SQL_LEN_DATA_AT_EXEC (*length*)-Makros (beim Aufrufen eines ODBC 2,0-Treibers) im **StrLen_or_IndPtr* Puffer ab.  
  
3.  Ruft **SQLExecute** oder **SQLExecDirect** auf, um die SQL-Anweisung auszuführen.  
  
    -   Wenn keine Data-at-Execution-Parameter vorhanden sind, ist der Prozess vollständig.  
  
    -   Wenn Data-at-Execution-Parameter vorhanden sind, gibt die Funktion SQL_NEED_DATA zurück.  
  
4.  Ruft **SQLParamData** auf, um den von der Anwendung definierten Wert abzurufen, der im *ParameterValuePtr* -Argument von **SQLBindParameter** angegeben ist, für den ersten Data-at-Execution-Parameter, der verarbeitet werden soll. **SQLParamData** gibt SQL_NEED_DATA zurück.  
  
    > [!NOTE]  
    >  Obwohl Data-at-Execution-Parameter mit Data-at-Execution-Spalten vergleichbar sind, ist der von **SQLParamData** zurückgegebene Wert für jeden Wert anders. Data-at-Execution-Parameter sind Parameter in einer SQL-Anweisung, für die Daten mit **SQLPutData** gesendet werden, wenn die Anweisung mit **SQLExecDirect** oder **SQLExecute** ausgeführt wird. Sie sind mit **SQLBindParameter** gebunden. Der von **SQLParamData** zurückgegebene Wert ist ein Zeiger Wert, der im *ParameterValuePtr* -Argument an **SQLBindParameter** übergeben wird. Data-at-Execution-Spalten sind Spalten in einem Rowset, für die Daten mit **SQLPutData** gesendet werden, wenn eine Zeile mit **SQLBulkOperations** aktualisiert oder hinzugefügt oder mit **SQLSetPos** aktualisiert wird. Sie sind mit **SQLBindCol** gebunden. Der von **SQLParamData** zurückgegebene Wert ist die Adresse der Zeile im **targetvalueptr* -Puffer (durch einen **SQLBindCol**-Befehl festgelegt), der verarbeitet wird.  
  
5.  Ruft **SQLPutData** ein oder mehrere Male auf, um Daten für den Parameter zu senden. Wenn der Datenwert größer ist als der \* in **SQLPutData** angegebene *ParameterValuePtr* -Puffer, ist mehr als ein-Rückruf erforderlich. mehrere Aufrufe von **SQLPutData** für denselben Parameter sind nur zulässig, wenn Zeichen-c-Daten an eine Spalte mit einem Zeichen-, Binär-oder Datenquellen spezifischen Datentyp gesendet werden oder wenn binäre C-Daten an eine Spalte mit einem Zeichen-, Binär-oder Datenquellen spezifischen Datentyp gesendet werden.  
  
6.  Ruft **SQLParamData** erneut auf, um zu signalisieren, dass alle Daten für den Parameter gesendet wurden.  
  
    -   Wenn weitere Data-at-Execution-Parameter vorhanden sind, gibt **SQLParamData** SQL_NEED_DATA und den von der Anwendung definierten Wert für den nächsten Data-at-Execution-Parameter zurück, der verarbeitet werden soll. Die Anwendung wiederholt die Schritte 4 und 5.  
  
    -   Wenn keine weiteren Data-at-Execution-Parameter vorhanden sind, ist der Prozess vollständig. Wenn die Anweisung erfolgreich ausgeführt wurde, gibt **SQLParamData** SQL_SUCCESS oder SQL_SUCCESS_WITH_INFO zurück. Wenn bei der Ausführung ein Fehler aufgetreten ist, wird SQL_ERROR zurückgegeben. An diesem Punkt können **SQLParamData** beliebige SQLSTATE-Zeichen zurückgeben, die von der Funktion zurückgegeben werden können, die zum Ausführen der Anweisung (**SQLExecDirect** oder **SQLExecute**) verwendet wird.  
  
         Ausgabewerte für Eingabe-/Ausgabe-oder Ausgabeparameter sind in den \* *Parametern ParameterValuePtr* und **StrLen_or_IndPtr* verfügbar, nachdem die Anwendung alle von der-Anweisung generierten Resultsets abgerufen hat.  
  
 Durch Aufrufen von **SQLExecute** oder **SQLExecDirect** wird die Anweisung in einen SQL_NEED_DATA Zustand versetzt. An diesem Punkt kann die Anwendung nur **SQLCancel**, **SQLGetDiagField**, **SQLGetDiagRec**, **SQLGetFunctions**, **SQLParamData** oder **SQLPutData** mit der-Anweisung oder dem *Verbindungs Handle* aufrufen, das mit der-Anweisung verknüpft ist. Wenn eine andere Funktion mit der-Anweisung oder der Verbindung aufgerufen wird, die der Anweisung zugeordnet ist, gibt die Funktion SQLSTATE HY010 (Funktions Sequenz Fehler) zurück. Die-Anweisung verlässt den SQL_NEED_DATA Zustand, wenn **SQLParamData** oder **SQLPutData** einen Fehler zurückgibt, **SQLParamData** SQL_SUCCESS oder SQL_SUCCESS_WITH_INFO zurückgibt oder die Anweisung abgebrochen wird.  
  
 Wenn die Anwendung **SQLCancel** aufruft, während der Treiber weiterhin Daten für Data-at-Execution-Parameter benötigt, bricht der Treiber die Anweisungs Ausführung ab. die Anwendung kann dann **SQLExecute** oder **SQLExecDirect** erneut aufzurufen.  
  
## <a name="retrieving-streamed-output-parameters"></a>Abrufen von gestreuten Ausgabeparametern

 Wenn eine Anwendung *inputoutputtype* auf SQL_PARAM_INPUT_OUTPUT_STREAM oder SQL_PARAM_OUTPUT_STREAM festlegt, muss der Ausgabeparameter Wert von einem oder mehreren Aufrufen von **SQLGetData** abgerufen werden. Wenn der Treiber über einen Stream-Ausgabeparameter Wert verfügt, der an die Anwendung zurückgegeben wird, gibt er SQL_PARAM_DATA_AVAILABLE als Reaktion auf einen Aufrufen der folgenden Funktionen zurück: **SQLMoreResults**, **SQLExecute** und **SQLExecDirect**. Eine Anwendung ruft **SQLParamData** auf, um zu bestimmen, welcher Parameterwert verfügbar ist.  
  
 Weitere Informationen zu SQL_PARAM_DATA_AVAILABLE und gestreamt-Ausgabeparametern finden [Sie unter Abrufen von Ausgabeparametern mit SQLGetData](../../../odbc/reference/develop-app/retrieving-output-parameters-using-sqlgetdata.md).  
  
## <a name="using-arrays-of-parameters"></a>Verwenden von Parameterarrays

 Wenn eine Anwendung eine-Anweisung mit Parameter Markierungen vorbereitet und ein Array von Parametern übergibt, gibt es zwei Möglichkeiten, wie dies ausgeführt werden kann. Eine Möglichkeit besteht darin, dass sich der Treiber auf die Array Verarbeitungsfunktionen des Back-Ends verlässt. in diesem Fall wird die gesamte Anweisung mit dem Array von Parametern als eine unteilbare Einheit behandelt. Oracle ist ein Beispiel für eine Datenquelle, die Array Verarbeitungsfunktionen unterstützt. Eine andere Möglichkeit zum Implementieren dieses Features besteht darin, dass der Treiber einen Batch von SQL-Anweisungen generiert, eine SQL-Anweisung für jeden Satz von Parametern im Parameter Array und den Batch auszuführen. Arrays von Parametern können nicht mit einer **Update WHERE CURRENT of** -Anweisung verwendet werden.  
  
 Wenn ein Array von Parametern verarbeitet wird, können einzelne Resultsets/Zeilen Anzahlen (eines für jeden Parametersatz) verfügbar sein, oder die Anzahl der Resultsets/Zeilen kann in eins umgewandelt werden. Die Option SQL_PARAM_ARRAY_ROW_COUNTS in **SQLGetInfo** gibt an, ob die Zeilen Anzahl für jeden Parametersatz (SQL_PARC_BATCH) verfügbar ist oder nur eine Zeilen Anzahl verfügbar ist (SQL_PARC_NO_BATCH).  
  
 Die Option SQL_PARAM_ARRAY_SELECTS in **SQLGetInfo** gibt an, ob ein Resultset für jeden Parametersatz (SQL_PAS_BATCH) verfügbar ist oder nur ein Resultset verfügbar ist (SQL_PAS_NO_BATCH). Wenn der Treiber nicht zulässt, dass eine Resultset-generierende Anweisung mit einem Array von Parametern ausgeführt wird, gibt SQL_PARAM_ARRAY_SELECTS SQL_PAS_NO_SELECT zurück.  
  
 Weitere Informationen finden Sie unter [SQLGetInfo-Funktion](../../../odbc/reference/syntax/sqlgetinfo-function.md).  
  
 Zur Unterstützung von Arrays von Parametern wird das SQL_ATTR_PARAMSET_SIZE-Anweisungs Attribut festgelegt, um die Anzahl der Werte für jeden Parameter anzugeben. Wenn das Feld größer als 1 ist, müssen die Felder SQL_DESC_DATA_PTR, SQL_DESC_INDICATOR_PTR und SQL_DESC_OCTET_LENGTH_PTR der APD auf Arrays zeigen. Die Kardinalität der einzelnen Arrays entspricht dem Wert SQL_ATTR_PARAMSET_SIZE.  
  
 Das SQL_DESC_ROWS_PROCESSED_PTR-Feld des APD verweist auf einen Puffer, der die Anzahl der verarbeiteten Parametersätze enthält, einschließlich der Fehler Sätze. Wenn jeder Satz von Parametern verarbeitet wird, speichert der Treiber einen neuen Wert im Puffer. Wenn dies ein NULL-Zeiger ist, wird keine Zahl zurückgegeben. Wenn Arrays von Parametern verwendet werden, wird der Wert, auf den das SQL_DESC_ROWS_PROCESSED_PTR-Feld der APD zeigt, auch dann aufgefüllt, wenn SQL_ERROR von der Setting-Funktion zurückgegeben wird. Wenn SQL_NEED_DATA zurückgegeben wird, wird der Wert, auf den das SQL_DESC_ROWS_PROCESSED_PTR-Feld der APD zeigt, auf den Satz von Parametern festgelegt, der verarbeitet wird.  
  
 Was geschieht, wenn ein Array von Parametern gebunden wird und ein **Update, bei dem die Current of** -Anweisung ausgeführt wird, Treiber definiert ist.  
  
## <a name="column-wise-parameter-binding"></a>Column-Wise Parameter Bindung

 In Spalten weiser Bindung bindet die Anwendung separate Parameter-und Längen-/indikatorenarrays an jeden Parameter.  
  
 Um die spaltenweise Bindung zu verwenden, legt die Anwendung zuerst das SQL_ATTR_PARAM_BIND_TYPE Statement-Attribut auf SQL_PARAM_BIND_BY_COLUMN fest. (Dies ist die Standardeinstellung.) Für jede Spalte, die gebunden werden soll, führt die Anwendung die folgenden Schritte aus:  
  
1.  Ordnet ein Parameter Puffer Array zu.  
  
2.  Ordnet ein Array von Längen-/indikatorenpuffern zu.  
  
    > [!NOTE]  
    >  Wenn die Anwendung bei Verwendung der Spalten bezogenen Bindung direkt in Deskriptoren schreibt, können separate Arrays für Längen-und Indikator Daten verwendet werden.  
  
3.  Ruft **SQLBindParameter** mit den folgenden Argumenten auf:  
  
    -   *ValueType* ist der C-Typ eines einzelnen Elements im Parameter Puffer Array.  
  
    -   *ParameterType* ist der SQL-Typ des Parameters.  
  
    -   *ParameterValuePtr* ist die Adresse des Parameter Puffer Arrays.  
  
    -   *BufferLength* ist die Größe eines einzelnen Elements im Parameter Puffer Array. Das *BufferLength* -Argument wird ignoriert, wenn die Daten Daten fester Länge haben.  
  
    -   *StrLen_or_IndPtr* ist die Adresse des Längen-/indikatorenarrays.  
  
 Weitere Informationen zur Verwendung dieser Informationen finden Sie unter "ParameterValuePtr-Argument" in "comments" weiter unten in diesem Abschnitt. Weitere Informationen zur Spalten bezogenen Bindung von Parametern finden Sie unter [Binden von Arrays von Parametern](../../../odbc/reference/develop-app/binding-arrays-of-parameters.md).  
  
## <a name="row-wise-parameter-binding"></a>Row-Wise Parameter Bindung

 In Zeilen weiser Bindung definiert die Anwendung eine Struktur, die Parameter-und Längen-/Indikatorpuffer für jeden zu gebundenen Parameter enthält.  
  
 Die Anwendung führt die folgenden Schritte aus, um die zeilenweise Bindung zu verwenden:  
  
1.  Definiert eine-Struktur, die einen einzelnen Satz von Parametern (einschließlich der Parameter-und Längen-/Indikatorpuffer) enthält und ein Array dieser Strukturen zuordnet.  
  
    > [!NOTE]  
    >  Wenn die Anwendung bei Verwendung der Zeilen bezogenen Bindung direkt in Deskriptoren schreibt, können separate Felder für Längen-und Indikator Daten verwendet werden.  
  
2.  Legt das SQL_ATTR_PARAM_BIND_TYPE Anweisungs Attribut auf die Größe der Struktur fest, die einen einzelnen Satz von Parametern enthält, oder auf die Größe einer Instanz eines Puffers, in den die Parameter gebunden werden. Die Länge muss Leerzeichen für alle gebundenen Parameter und alle Auffüll Zeichen der Struktur oder des Puffers enthalten, um sicherzustellen, dass das Ergebnis, wenn die Adresse eines gebundenen Parameters mit der angegebenen Länge inkrementiert wird, auf den Anfang desselben Parameters in der nächsten Zeile zeigt. Wenn Sie den *sizeof* -Operator in ANSI C verwenden, wird dieses Verhalten garantiert.  
  
3.  Ruft **SQLBindParameter** mit den folgenden Argumenten für jeden Parameter auf, der gebunden werden soll:  
  
    -   *ValueType* ist der Typ des Parameter Puffer Elements, das an die Spalte gebunden werden soll.  
  
    -   *ParameterType* ist der SQL-Typ des Parameters.  
  
    -   *ParameterValuePtr* ist die Adresse des Parameter Puffer Elements im ersten Array Element.  
  
    -   *BufferLength* ist die Größe des Parameter Puffer Elements.  
  
    -   *StrLen_or_IndPtr* ist die Adresse des Längen-/indikatorenmembers, der gebunden werden soll.  
  
 Weitere Informationen zur Verwendung dieser Informationen finden Sie unter "*ParameterValuePtr* -Argument" weiter unten in diesem Abschnitt. Weitere Informationen über die zeilenweise Bindung von Parametern finden Sie in den [Bindungs Arrays von Parametern](../../../odbc/reference/develop-app/binding-arrays-of-parameters.md).  
  
## <a name="error-information"></a>Fehlerinformationen

 Wenn ein Treiber Parameter Arrays nicht als Batches implementiert (die SQL_PARAM_ARRAY_ROW_COUNTS Option ist gleich SQL_PARC_NO_BATCH), werden Fehlersituationen wie bei Ausführung einer Anweisung behandelt. Wenn der Treiber Parameter Arrays als Batches implementiert, kann eine Anwendung das SQL_DESC_ARRAY_STATUS_PTR Header-Feld der IPD verwenden, um zu bestimmen, welcher Parameter einer SQL-Anweisung oder welcher Parameter in einem Array von Parametern bewirkt hat, dass **SQLExecDirect** oder **SQLExecute** einen Fehler zurückgibt. Dieses Feld enthält Statusinformationen für jede Zeile von Parameterwerten. Wenn das Feld anzeigt, dass ein Fehler aufgetreten ist, geben Felder in der Diagnosedaten Struktur die Zeilen-und Parameter Nummer des fehlgeschlagenen Parameters an. Die Anzahl der Elemente im Array wird durch das SQL_DESC_ARRAY_SIZE Header-Feld in der APD definiert, das vom Attribut der SQL_ATTR_PARAMSET_SIZE Anweisung festgelegt werden kann.  
  
> [!NOTE]  
>  Das SQL_DESC_ARRAY_STATUS_PTR Header-Feld in der APD wird verwendet, um Parameter zu ignorieren. Weitere Informationen zum Ignorieren von Parametern finden Sie im nächsten Abschnitt "Ignorieren eines Satzes von Parametern".  
  
 Wenn **SQLExecute** oder **SQLExecDirect** SQL_ERROR zurückgibt, enthalten die Elemente im Array, auf das das SQL_DESC_ARRAY_STATUS_PTR Feld in der IPD zeigt, SQL_PARAM_ERROR, SQL_PARAM_SUCCESS, SQL_PARAM_SUCCESS_WITH_INFO, SQL_PARAM_UNUSED oder SQL_PARAM_DIAG_UNAVAILABLE.  
  
 Für jedes Element in diesem Array enthält die Diagnosedaten Struktur mindestens einen Statusdaten Satz. Das SQL_DIAG_ROW_NUMBER-Feld der-Struktur gibt die Zeilennummer der Parameterwerte an, die den Fehler verursacht haben. Wenn es möglich ist, den jeweiligen Parameter in einer Zeile von Parametern zu ermitteln, die den Fehler verursacht hat, wird die Parameter Nummer in das SQL_DIAG_COLUMN_NUMBER Feld eingegeben.  
  
 SQL_PARAM_UNUSED wird eingegeben, wenn ein Parameter nicht verwendet wurde, weil ein Fehler in einem früheren Parameter aufgetreten ist, durch den **SQLExecute** oder **SQLExecDirect** abgebrochen wurde. Wenn z. b. 50 Parameter vorhanden sind und ein Fehler aufgetreten ist, während der Basis-Satz von Parametern ausgeführt wurde, der das Abbrechen von **SQLExecute** oder **SQLExecDirect** bewirkt hat, wird SQL_PARAM_UNUSED im Status Array für die Parameter 41 bis 50 eingegeben.  
  
 SQL_PARAM_DIAG_UNAVAILABLE wird eingegeben, wenn der Treiber Arrays von Parametern als monolithische Einheit behandelt, sodass diese einzelne Parameter Ebene der Fehlerinformationen nicht generiert wird.  
  
 Einige Fehler bei der Verarbeitung eines einzelnen Parameter Satzes bewirken, dass die nachfolgenden Sätze von Parametern im Array beendet werden. Andere Fehler wirken sich nicht auf die Verarbeitung von nachfolgenden Parametern aus. Welche Fehler die Verarbeitung beendet, wird vom Treiber definiert. Wenn die Verarbeitung nicht beendet wird, werden alle Parameter im-Array verarbeitet, SQL_SUCCESS_WITH_INFO werden als Ergebnis des Fehlers zurückgegeben, und der von SQL_ATTR_PARAMS_PROCESSED_PTR definierte Puffer wird auf die Gesamtanzahl der verarbeiteten Parametersätze festgelegt (wie vom SQL_ATTR_PARAMSET_SIZE Statement-Attribut definiert), die Fehler Sätze einschließt.  
  
> [!CAUTION]  
>  ODBC-Verhalten, wenn ein Fehler bei der Verarbeitung eines Arrays von Parametern auftritt, ist in ODBC 3 anders. *x* als in ODBC 2. *x*. In ODBC 2. *x*, die zurückgegebene Funktion SQL_ERROR und die Verarbeitung wurde beendet. Der Puffer, auf den das *Pirow* -Argument von **SQLParamOptions** zeigt, enthält die Nummer der Fehler Zeile. In ODBC 3. *x*, die Funktion gibt SQL_SUCCESS_WITH_INFO zurück, und die Verarbeitung kann entweder beendet oder fortgesetzt werden. Wenn er fortgesetzt wird, wird der durch SQL_ATTR_PARAMS_PROCESSED_PTR angegebene Puffer auf den Wert aller verarbeiteten Parameter festgelegt, einschließlich derjenigen, die zu einem Fehler geführt haben. Diese Änderung des Verhaltens kann Probleme für vorhandene Anwendungen verursachen.  
  
 Wenn **SQLExecute** oder **SQLExecDirect** zurückgegeben wird, bevor die Verarbeitung aller Parametersätze in einem Parameter Array abgeschlossen wird, z. b. wenn SQL_ERROR oder SQL_NEED_DATA zurückgegeben wird, enthält das Statusarray Status für die Parameter, die bereits verarbeitet wurden. Der Speicherort, auf den das SQL_DESC_ROWS_PROCESSED_PTR-Feld in der IPD verweist, enthält die Zeilennummer im Parameter Array, die den SQL_ERROR-oder SQL_NEED_DATA Fehlercode verursacht hat. Wenn ein Array von Parametern an eine SELECT-Anweisung gesendet wird, ist die Verfügbarkeit von Status Array Werten vom Treiber definiert. Sie sind möglicherweise verfügbar, nachdem die-Anweisung ausgeführt wurde oder wenn Resultsets abgerufen werden.  
  
## <a name="ignoring-a-set-of-parameters"></a>Ignorieren eines Satzes von Parametern

 Das SQL_DESC_ARRAY_STATUS_PTR-Feld des APD (wie vom SQL_ATTR_PARAM_STATUS_PTR Statement-Attribut festgelegt) kann verwendet werden, um anzugeben, dass ein Satz gebundener Parameter in einer SQL-Anweisung ignoriert werden soll. Um den Treiber anzuweisen, mindestens eine Reihe von Parametern während der Ausführung zu ignorieren, sollte eine Anwendung die folgenden Schritte ausführen:  
  
1.  Aufrufen von **SQLSetDescField** , um das SQL_DESC_ARRAY_STATUS_PTR Header-Feld der APD so festzulegen, dass es auf ein Array von sqlusmallint-Werten verweist, die Status Informationen enthalten sollen. Dieses Feld kann auch durch Aufrufen von **SQLSetStmtAttr** mit dem *Attribut* SQL_ATTR_PARAM_OPERATION_PTR festgelegt werden, das es einer Anwendung ermöglicht, das Feld festzulegen, ohne ein Deskriptorhandle zu erhalten.  
  
2.  Legen Sie jedes Element des Arrays, das durch das SQL_DESC_ARRAY_STATUS_PTR-Feld der APD definiert ist, auf einen von zwei Werten fest:  
  
    -   SQL_PARAM_IGNORE, um anzugeben, dass die Zeile von der Anweisungs Ausführung ausgeschlossen wird.  
  
    -   SQL_PARAM_PROCEED, um anzugeben, dass die Zeile in der Anweisungs Ausführung enthalten ist.  
  
3.  Führen Sie **SQLExecDirect** oder **SQLExecute** zum Ausführen der vorbereiteten Anweisung aus.  
  
 Die folgenden Regeln gelten für das Array, das durch das SQL_DESC_ARRAY_STATUS_PTR-Feld der APD definiert wird:  
  
-   Der Zeiger ist standardmäßig auf NULL festgelegt.  
  
-   Wenn der Zeiger NULL ist, werden alle Sätze von Parametern verwendet, als wären alle Elemente auf SQL_ROW_PROCEED festgelegt.  
  
-   Wenn ein Element auf SQL_PARAM_PROCEED festgelegt wird, wird nicht garantiert, dass der Vorgang diesen speziellen Satz von Parametern verwendet.  
  
-   SQL_PARAM_PROCEED ist in der Header Datei als 0 definiert.  
  
 Eine Anwendung kann das SQL_DESC_ARRAY_STATUS_PTR-Feld in der APD so festlegen, dass Sie auf dasselbe Array verweist, das auf das SQL_DESC_ARRAY_STATUS_PTR Feld in der IRD zeigt. Dies ist nützlich, wenn Parameter an Zeilendaten gebunden werden. Parameter können dann gemäß dem Status der Zeilendaten ignoriert werden. Zusätzlich zu SQL_PARAM_IGNORE bewirken die folgenden Codes, dass ein Parameter in einer SQL-Anweisung ignoriert wird: SQL_ROW_DELETED, SQL_ROW_UPDATED und SQL_ROW_ERROR. Zusätzlich zu SQL_PARAM_PROCEED bewirken die folgenden Codes, dass eine SQL-Anweisung fortgesetzt wird: SQL_ROW_SUCCESS, SQL_ROW_SUCCESS_WITH_INFO und SQL_ROW_ADDED.  
  
## <a name="rebinding-parameters"></a>Parameter zum erneuten Binden

 Eine Anwendung kann eine der beiden Vorgänge ausführen, um eine Bindung zu ändern:  
  
-   Aufrufen von **SQLBindParameter** , um eine neue Bindung für eine Spalte anzugeben, die bereits gebunden ist. Der Treiber überschreibt die alte Bindung mit der neuen.  
  
-   Geben Sie einen Offset an, der der Puffer Adresse hinzugefügt werden soll, die durch den Bindungs Befehl **SQLBindParameter** angegeben wurde. Weitere Informationen finden Sie im nächsten Abschnitt "Rebinding with Offsets".  
  
## <a name="rebinding-with-offsets"></a>Erneute Bindung mit Offsets

 Die erneute Bindung von Parametern ist besonders nützlich, wenn eine Anwendung über ein Pufferbereich-Setup verfügt, das viele Parameter enthalten kann, aber ein-Befehl von **SQLExecDirect** oder **SQLExecute** nur einige wenige Parameter verwendet. Der verbleibende Speicherplatz im Pufferbereich kann für die nächste Gruppe von Parametern verwendet werden, indem die vorhandene Bindung durch einen Offset geändert wird.  
  
 Das SQL_DESC_BIND_OFFSET_PTR Header-Feld im APD verweist auf den Bindungs Offset. Wenn das Feld ungleich NULL ist, dereferenziert der Treiber den Zeiger. Wenn keiner der Werte in den Feldern SQL_DESC_DATA_PTR, SQL_DESC_INDICATOR_PTR und SQL_DESC_OCTET_LENGTH_PTR ein NULL-Zeiger ist, wird der Dereferenzierte Wert den Feldern in den deskriptordaten Sätzen zur Ausführungszeit hinzugefügt. Die neuen Zeiger Werte werden verwendet, wenn die SQL-Anweisungen ausgeführt werden. Der Offset bleibt nach der erneuten Bindung gültig. Da SQL_DESC_BIND_OFFSET_PTR ein Zeiger auf den Offset anstelle des Offsets selbst ist, kann eine Anwendung den Offset direkt ändern, ohne [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md) oder [SQLSetDescRec](../../../odbc/reference/syntax/sqlsetdescrec-function.md) aufrufen zu müssen, um das Deskriptorfeld zu ändern. Der Zeiger ist standardmäßig auf NULL festgelegt. Das SQL_DESC_BIND_OFFSET_PTR-Feld der ARD kann durch einen [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md) -Befehl oder durch einen [SQLSetStmtAttr](../../../odbc/reference/syntax/sqlsetstmtattr-function.md)-Befehl mit einem *fattribute* von SQL_ATTR_PARAM_BIND_OFFSET_PTR festgelegt werden.  
  
 Der Bindungs Offset wird immer direkt den Werten in den Feldern SQL_DESC_DATA_PTR, SQL_DESC_INDICATOR_PTR und SQL_DESC_OCTET_LENGTH_PTR hinzugefügt. Wenn der Offset in einen anderen Wert geändert wird, wird der neue Wert dem Wert in jedem Deskriptorfeld weiterhin direkt hinzugefügt. Der neue Offset wird der Summe des Feldwerts und aller früheren Offsets nicht hinzugefügt.  
  
## <a name="descriptors"></a>Deskriptoren

 Die Art und Weise, wie ein Parameter gebunden wird, wird durch Felder der APDS und IPDS bestimmt. Die Argumente in **SQLBindParameter** werden verwendet, um diese Deskriptorfelder festzulegen. Die Felder können auch durch die **SQLSetDescField** -Funktionen festgelegt werden, obwohl **SQLBindParameter** effizienter zu verwenden ist, da die Anwendung kein Deskriptorhandle zum Aufrufen von **SQLBindParameter** abrufen muss.  
  
> [!CAUTION]  
>  Das Aufrufen von **SQLBindParameter** für eine Anweisung kann sich auf andere-Anweisungen auswirken. Dies tritt auf, wenn der mit der-Anweisung verknüpfte ARD explizit zugeordnet und auch anderen-Anweisungen zugeordnet wird. Da **SQLBindParameter** die Felder der APD ändert, gelten die Änderungen für alle-Anweisungen, die diesem Deskriptor zugeordnet sind. Wenn dies nicht das erforderliche Verhalten ist, muss die Anwendung diesen Deskriptor von den anderen Anweisungen trennen, bevor **SQLBindParameter** aufgerufen wird.  
  
 Konzeptionell führt **SQLBindParameter** die folgenden Schritte nacheinander aus:  
  
1.  Ruft [SQLGetStmtAttr](../../../odbc/reference/syntax/sqlgetstmtattr-function.md) auf, um das APD-Handle abzurufen.  
  
2.  Ruft [SQLGetDescField](../../../odbc/reference/syntax/sqlgetdescfield-function.md) auf, um das SQL_DESC_COUNT Feld der APD zu erhalten, und wenn der Wert des *ColumnNumber* -Arguments den Wert von SQL_DESC_COUNT überschreitet, ruft **SQLSetDescField** auf, um den Wert von SQL_DESC_COUNT auf *ColumnNumber* zu erhöhen.  
  
3.  Ruft [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md) mehrmals auf, um den folgenden Feldern der APD Werte zuzuweisen:  
  
    -   Legt SQL_DESC_TYPE und SQL_DESC_CONCISE_TYPE auf den Wert von *ValueType* fest, mit dem Unterschied, dass, wenn *ValueType* einer der präzisen Bezeichner eines DateTime-oder Interval-unter Typs ist, SQL_DESC_TYPE auf SQL_DATETIME oder SQL_INTERVAL festgelegt wird, SQL_DESC_CONCISE_TYPE auf den präzisen Bezeichner festgelegt und SQL_DESC_DATETIME_INTERVAL_CODE auf den entsprechenden DateTime-oder Interval-Subcode festgelegt wird.  
  
    -   Legt das SQL_DESC_OCTET_LENGTH-Feld auf den Wert von *BufferLength* fest.  
  
    -   Legt das SQL_DESC_DATA_PTR-Feld auf den Wert von *ParameterValue* fest.  
  
    -   Legt das SQL_DESC_OCTET_LENGTH_PTR-Feld auf den Wert *StrLen_Or_Ind* fest.  
  
    -   Legt für das SQL_DESC_INDICATOR_PTR-Feld ebenfalls den Wert *StrLen_Or_Ind* fest.  
  
     Der *StrLen_Or_Ind* -Parameter gibt sowohl die Indikator Informationen als auch die Länge des Parameter Werts an.  
  
4.  Ruft [SQLGetStmtAttr](../../../odbc/reference/syntax/sqlgetstmtattr-function.md) auf, um das IPD-Handle abzurufen.  
  
5.  Ruft [SQLGetDescField](../../../odbc/reference/syntax/sqlgetdescfield-function.md) auf, um das SQL_DESC_COUNT Feld der IPD zu erhalten, und wenn der Wert des *ColumnNumber* -Arguments den Wert von SQL_DESC_COUNT überschreitet, ruft **SQLSetDescField** auf, um den Wert von SQL_DESC_COUNT auf *ColumnNumber* zu erhöhen.  
  
6.  Ruft [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md) mehrmals auf, um den folgenden Feldern der IPD Werte zuzuweisen:  
  
    -   Legt SQL_DESC_TYPE und SQL_DESC_CONCISE_TYPE auf den Wert von *ParameterType* fest, mit dem Unterschied, dass, wenn *ParameterType* einer der präziseren Bezeichner eines DateTime-oder Interval-unter Typs ist, SQL_DESC_TYPE auf SQL_DATETIME oder SQL_INTERVAL festgelegt wird, SQL_DESC_CONCISE_TYPE auf den präzisen Bezeichner festgelegt und SQL_DESC_DATETIME_INTERVAL_CODE auf den entsprechenden DateTime-oder Interval-Subcode festgelegt wird.  
  
    -   Legt eine oder mehrere SQL_DESC_LENGTH, SQL_DESC_PRECISION und SQL_DESC_DATETIME_INTERVAL_PRECISION entsprechend für *ParameterType* fest.  
  
    -   Legt SQL_DESC_SCALE auf den Wert von *DecimalDigits* fest.  
  
 Wenn beim Aufrufen von **SQLBindParameter** ein Fehler auftritt, sind die Inhalte der Deskriptorfelder, die in der APD festgelegt wurden, nicht definiert, und das SQL_DESC_COUNT-Feld der APD bleibt unverändert. Außerdem sind die Felder "SQL_DESC_LENGTH", "SQL_DESC_PRECISION", "SQL_DESC_SCALE" und "SQL_DESC_TYPE" des entsprechenden Datensatzes in der IPD nicht definiert, und das Feld "SQL_DESC_COUNT" der IPD bleibt unverändert.  
  
## <a name="conversion-of-calls-to-and-from-sqlsetparam"></a>Konvertierung von Aufrufen von und von SQLSetParam

 Wenn eine ODBC 1,0-Anwendung **SQLSetParam** in einem ODBC 3 aufruft. *x* -Treiber, ODBC 3. der *x* -Treiber-Manager ordnet den-Befehl zu, wie in der folgenden Tabelle gezeigt.  
  
|Von der Anwendung ODBC 1,0 anrufen|ODBC 3-Rückruf. *x* -Treiber|  
|----------------------------------|-------------------------------|  
|SQLSetParam (StatementHandle, ParameterNumber, ValueType, ParameterType, Längen Genauigkeit, parameterscale, ParameterValuePtr, StrLen_or_IndPtr);|SQLBindParameter (StatementHandle, ParameterNumber, SQL_PARAM_INPUT_OUTPUT, ValueType, ParameterType,      *ColumnSize*,      *DecimalDigits*, ParameterValuePtr, SQL_SETPARAM_VALUE_MAX, StrLen_or_IndPtr);|  
  
## <a name="examples"></a>Beispiele  

### <a name="a-use-sqlbindparameter-function"></a>A. Verwenden der SQLBindParameter-Funktion

 Im folgenden Beispiel bereitet eine Anwendung eine SQL-Anweisung vor, um Daten in die Tabelle Orders einzufügen. Für jeden Parameter in der-Anweisung ruft die Anwendung **SQLBindParameter** auf, um den ODBC-C-Datentyp und den SQL-Datentyp des Parameters anzugeben, und um einen Puffer an jeden Parameter zu binden. Für jede Daten Zeile weist die Anwendung jedem Parameterdaten Werte zu und ruft **SQLExecute** auf, um die Anweisung auszuführen.  
  
 Im folgenden Beispiel wird davon ausgegangen, dass Sie über eine ODBC-Datenquelle mit dem Namen "Northwind" verfügen, die der Northwind-Datenbank zugeordnet ist.  
  
 Weitere Codebeispiele finden Sie unter [SQLBulkOperations-Funktion](../../../odbc/reference/syntax/sqlbulkoperations-function.md), [SQLProcedures](../../../odbc/reference/syntax/sqlprocedures-function.md)-Funktion, [SQLPutData-Funktion](../../../odbc/reference/syntax/sqlputdata-function.md)und [SQLSetPos-Funktion](../../../odbc/reference/syntax/sqlsetpos-function.md).  
  
```cpp
// SQLBindParameter_Function.cpp  
// compile with: ODBC32.lib  
#include <windows.h>  
#include <sqltypes.h>  
#include <sqlext.h>  
  
#define EMPLOYEE_ID_LEN 10  
  
SQLHENV henv = NULL;  
SQLHDBC hdbc = NULL;  
SQLRETURN retcode;  
SQLHSTMT hstmt = NULL;  
SQLSMALLINT sCustID;  
  
SQLCHAR szEmployeeID[EMPLOYEE_ID_LEN];  
SQL_DATE_STRUCT dsOrderDate;  
SQLINTEGER cbCustID = 0, cbOrderDate = 0, cbEmployeeID = SQL_NTS;  
  
int main() {  
   retcode = SQLAllocHandle(SQL_HANDLE_ENV, SQL_NULL_HANDLE, &henv);  
   retcode = SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION, (SQLPOINTER*)SQL_OV_ODBC3, 0);   
  
   retcode = SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc);   
   retcode = SQLSetConnectAttr(hdbc, SQL_LOGIN_TIMEOUT, (SQLPOINTER)5, 0);  
  
   retcode = SQLConnect(hdbc, (SQLCHAR*) "Northwind", SQL_NTS, (SQLCHAR*) NULL, 0, NULL, 0);  
   retcode = SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmt);  
  
   retcode = SQLBindParameter(hstmt, 1, SQL_PARAM_INPUT, SQL_C_CHAR, SQL_CHAR, EMPLOYEE_ID_LEN, 0, szEmployeeID, 0, &cbEmployeeID);  
   retcode = SQLBindParameter(hstmt, 2, SQL_PARAM_INPUT, SQL_C_SSHORT, SQL_INTEGER, 0, 0, &sCustID, 0, &cbCustID);  
   retcode = SQLBindParameter(hstmt, 3, SQL_PARAM_INPUT, SQL_C_TYPE_DATE, SQL_TIMESTAMP, sizeof(dsOrderDate), 0, &dsOrderDate, 0, &cbOrderDate);  
  
   retcode = SQLPrepare(hstmt, (SQLCHAR*)"INSERT INTO Orders(CustomerID, EmployeeID, OrderDate) VALUES (?, ?, ?)", SQL_NTS);  
  
   strcpy_s((char*)szEmployeeID, _countof(szEmployeeID), "BERGS");  
   sCustID = 5;  
   dsOrderDate.year = 2006;  
   dsOrderDate.month = 3;  
   dsOrderDate.day = 17;  
  
   retcode = SQLExecute(hstmt);  
}  
```  
  
### <a name="b-execute-a-stored-procedure-using-a-named-parameter"></a>B. Ausführen einer gespeicherten Prozedur mit einem benannten Parameter

 Im folgenden Beispiel führt eine Anwendung eine gespeicherte Prozedur SQL Server mithilfe eines benannten Parameters aus.  
  
```cpp
// SQLBindParameter_Function_2.cpp  
// compile with: ODBC32.lib  
// sample assumes the following stored procedure:  
// use northwind  
// DROP PROCEDURE SQLBindParameter  
// GO  
//   
// CREATE PROCEDURE SQLBindParameter @quote int  
// AS  
// delete from orders where OrderID >= @quote  
// GO  
#include <windows.h>  
#include <sqltypes.h>  
#include <sqlext.h>  
  
SQLHDESC hIpd = NULL;  
SQLHENV henv = NULL;  
SQLHDBC hdbc = NULL;  
SQLRETURN retcode;  
SQLHSTMT hstmt = NULL;  
SQLCHAR szQuote[50] = "100084";  
SQLINTEGER cbValue = SQL_NTS;  
  
int main() {  
   retcode = SQLAllocHandle(SQL_HANDLE_ENV, SQL_NULL_HANDLE, &henv);  
   retcode = SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION, (SQLPOINTER*)SQL_OV_ODBC3, 0);   
  
   retcode = SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc);   
   retcode = SQLSetConnectAttr(hdbc, SQL_LOGIN_TIMEOUT, (SQLPOINTER)5, 0);  
  
   retcode = SQLConnect(hdbc, (SQLCHAR*) "Northwind", SQL_NTS, (SQLCHAR*) NULL, 0, NULL, 0);  
   retcode = SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmt);  
  
   retcode = SQLPrepare(hstmt, (SQLCHAR*)"{call SQLBindParameter(?)}", SQL_NTS);  
   retcode = SQLBindParameter(hstmt, 1, SQL_PARAM_INPUT, SQL_C_CHAR, SQL_CHAR, 50, 0, szQuote, 0, &cbValue);  
   retcode = SQLGetStmtAttr(hstmt, SQL_ATTR_IMP_PARAM_DESC, &hIpd, 0, 0);  
   retcode = SQLSetDescField(hIpd, 1, SQL_DESC_NAME, "@quote", SQL_NTS);  
  
   retcode = SQLExecute(hstmt);  
}  
```  
  
## <a name="related-functions"></a>Verwandte Funktionen  
  
|Informationen über|Finden Sie unter|  
|---------------------------|---------|  
|Zurückgeben von Informationen zu einem Parameter in einer-Anweisung|[SQLDescribeParam-Funktion](../../../odbc/reference/syntax/sqldescribeparam-function.md)|  
|Ausführen einer SQL-Anweisung|[SQLExecDirect-Funktion](../../../odbc/reference/syntax/sqlexecdirect-function.md)|  
|Ausführen einer vorbereiteten SQL-Anweisung|[SQLExecute-Funktion](../../../odbc/reference/syntax/sqlexecute-function.md)|  
|Freigeben von Parameter Puffern in der Anweisung|[SQLFreeStmt-Funktion](../../../odbc/reference/syntax/sqlfreestmt-function.md)|  
|Zurückgeben der Anzahl von Anweisungs Parametern|[SQLNumParams-Funktion](../../../odbc/reference/syntax/sqlnumparams-function.md)|  
|Zurückgeben des nächsten Parameters zum Senden von Daten|[SQLParamData-Funktion](../../../odbc/reference/syntax/sqlparamdata-function.md)|  
|Angeben mehrerer Parameterwerte|[SQLParamOptions-Funktion](../../../odbc/reference/syntax/sqlparamoptions-function.md)|  
|Senden von Parameterdaten zur Ausführungszeit|[SQLPutData-Funktion](../../../odbc/reference/syntax/sqlputdata-function.md)|  
  
## <a name="see-also"></a>Weitere Informationen

 [ODBC-API-Referenz](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [ODBC-Header Dateien](../../../odbc/reference/install/odbc-header-files.md)   
 [Abrufen von Ausgabeparametern mithilfe von SQLGetData](../../../odbc/reference/develop-app/retrieving-output-parameters-using-sqlgetdata.md)
