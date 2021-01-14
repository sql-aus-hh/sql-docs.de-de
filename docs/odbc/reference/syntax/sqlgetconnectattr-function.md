---
description: SQLGetConnectAttr-Funktion
title: SQLGetConnectAttr-Funktion | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
apiname:
- SQLGetConnectOption
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLGetConnectAttr
helpviewer_keywords:
- SQLGetConnectAttr function [ODBC]
ms.assetid: 2cb4ffa8-19d3-4664-8c2f-6682cdcc3f33
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 5da83146d0c6167a08f36a648749470830795703
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171862"
---
# <a name="sqlgetconnectattr-function"></a>SQLGetConnectAttr-Funktion
**Konformitäts**  
 Eingeführte Version: ODBC 3,0 Standards Compliance: ISO 92  
  
 **Zusammenfassung**  
 **SQLGetConnectAttr** gibt die aktuelle Einstellung eines Connection-Attributs zurück.  
  
> [!NOTE]
>  Weitere Informationen dazu, wie der Treiber-Manager diese Funktion zuordnet, wenn eine ODBC 3 *. x* -Anwendung mit einem ODBC 2 *. x* -Treiber arbeitet, finden Sie unterzuordnen [von Ersetzungs Funktionen für die Abwärtskompatibilität von Anwendungen](../../../odbc/reference/develop-app/mapping-replacement-functions-for-backward-compatibility-of-applications.md).  
  
## <a name="syntax"></a>Syntax  
  
```cpp  
  
SQLRETURN SQLGetConnectAttr(  
     SQLHDBC        ConnectionHandle,  
     SQLINTEGER     Attribute,  
     SQLPOINTER     ValuePtr,  
     SQLINTEGER     BufferLength,  
     SQLINTEGER *   StringLengthPtr);  
```  
  
## <a name="arguments"></a>Argumente  
 *ConnectionHandle*  
 [Eingabe] Verbindungshandle.  
  
 *Attribut*  
 Der Das abzurufende Attribut.  
  
 *ValuePtr*  
 Ausgeben Ein Zeiger auf den Arbeitsspeicher, in dem der aktuelle Wert des durch das *Attribut* angegebenen Attributs zurückgegeben werden soll. Bei Attributen vom Typ "Integer" können einige Treiber nur den unteren 32-Bit-oder 16-Bit-Puffer eines Puffers schreiben und das Bit höherer Ordnung unverändert lassen. Daher sollten Anwendungen einen Puffer von SQLULEN verwenden und den Wert auf 0 initialisieren, bevor Sie diese Funktion aufrufen.  
  
 Wenn *ValuePtr* den Wert NULL hat, gibt *stringlengthptr* weiterhin die Gesamtzahl der Bytes (ausgenommen des NULL-Beendigungs Zeichens für Zeichendaten) zurück, die im Puffer zurückgegeben werden können, auf den *ValuePtr* zeigt.  
  
 *BufferLength*  
 Der Wenn *Attribute* ein ODBC-definiertes Attribut ist und *ValuePtr* auf eine Zeichenfolge oder einen binären Puffer zeigt, sollte dieses Argument der Länge von \* *ValuePtr* entsprechen. Wenn *Attribute* ein ODBC-definiertes Attribut und *\* ValuePtr* eine ganze Zahl ist, wird *BufferLength* ignoriert. Wenn der Wert in *\* ValuePtr* eine Unicode-Zeichenfolge ist (beim Aufrufen von **sqlgetconnectattrw**), muss das *BufferLength* -Argument eine gerade Zahl sein.  
  
 Wenn *Attribute* ein Treiber definiertes Attribut ist, gibt die Anwendung die Art des Attributs für den Treiber-Manager an, indem das *BufferLength* -Argument festgelegt wird. *BufferLength* kann die folgenden Werte aufweisen:  
  
-   Wenn *\* ValuePtr* ein Zeiger auf eine Zeichenfolge ist, entspricht *BufferLength* der Länge der Zeichenfolge.  
  
-   Wenn *\* ValuePtr* ein Zeiger auf einen binären Puffer ist, platziert die Anwendung das Ergebnis des Makros SQL_LEN_BINARY_ATTR (*length*) in *BufferLength*. Dadurch wird ein negativer Wert in *BufferLength* platziert.  
  
-   Wenn *\* ValuePtr* ein Zeiger auf einen anderen Wert als eine Zeichenfolge oder eine binäre Zeichenfolge ist, muss *BufferLength* den Wert SQL_IS_POINTER.  
  
-   Wenn *\* ValuePtr* einen Datentyp mit fester Länge enthält, ist *BufferLength* entweder SQL_IS_INTEGER oder SQL_IS_UINTEGER.  
  
 *Stringlengthptr*  
 Ausgeben Ein Zeiger auf einen Puffer, in dem die Gesamtzahl der Bytes (ausgenommen des NULL-Beendigungs Zeichens) zurückgegeben werden soll, die in \* *ValuePtr* zurückgegeben werden können. Wenn der Attribut Wert eine Zeichenfolge ist und die Anzahl von Bytes, die zurückgegeben werden können, größer als *BufferLength* abzüglich der Länge des NULL-Beendigungs Zeichens ist, werden die Daten in *\* ValuePtr* auf *BufferLength* abzüglich der Länge des NULL-Beendigungs Zeichens gekürzt und vom Treiber auf Null beendet.  
  
## <a name="returns"></a>Gibt zurück  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_NO_DATA, SQL_ERROR oder SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnose  
 Wenn **SQLGetConnectAttr** SQL_ERROR oder SQL_SUCCESS_WITH_INFO zurückgibt, kann ein zugeordneter SQLSTATE-Wert aus der Diagnosedaten Struktur abgerufen werden, indem **SQLGetDiagRec** mit dem *Handlertyp* SQL_HANDLE_DBC und einem *handle* von *connectionHandle* aufgerufen wird. In der folgenden Tabelle sind die SQLSTATE-Werte aufgelistet, die normalerweise von **SQLGetConnectAttr** zurückgegeben werden, und die einzelnen Werte werden im Kontext dieser Funktion erläutert. die Notation "(DM)" geht vor den Beschreibungen von Sqlstates vor, die vom Treiber-Manager zurückgegeben werden. Der Rückgabecode, der den einzelnen SQLSTATE-Werten zugeordnet ist, ist SQL_ERROR, sofern nichts anderes angegeben ist.  
  
|SQLSTATE|Fehler|Beschreibung|  
|--------------|-----------|-----------------|  
|01000|Allgemeine Warnung|Treiber spezifische Informations Meldung. (Die Funktion gibt SQL_SUCCESS_WITH_INFO zurück.)|  
|01004|Zeichen folgen Daten, rechts abgeschnitten|Die in \* *ValuePtr* zurückgegebenen Daten wurden als *BufferLength* abzüglich der Länge eines NULL-Beendigungs Zeichens abgeschnitten. Die Länge des nicht abgeschnittene Zeichen folgen Werts wird in *\* stringlengthptr* zurückgegeben. (Die Funktion gibt SQL_SUCCESS_WITH_INFO zurück.)|  
|08003|Verbindung nicht geöffnet|(DM) ein *Attribut* Wert, der eine geöffnete Verbindung erforderte, wurde angegeben.|  
|08S01|Kommunikations Verbindungsfehler|Die Kommunikationsverbindung zwischen dem Treiber und der Datenquelle, mit der der Treiber verbunden war, ist fehlgeschlagen, bevor die Funktion die Verarbeitung abgeschlossen hat.|  
|HY000|Allgemeiner Fehler|Es ist ein Fehler aufgetreten, bei dem kein spezifischer SQLSTATE vorhanden war und für den kein Implementierungs spezifischer SQLSTATE definiert wurde. Die Fehlermeldung, die von der Diagnosedaten Struktur durch das Argument *MessageText* in **SQLGetDiagField** zurückgegeben wird, beschreibt den Fehler und die Ursache.|  
|HY001|Fehler bei der Speicher Belegung|Der Treiber konnte keinen Arbeitsspeicher zuweisen, der zur Unterstützung der Ausführung oder Beendigung der Funktion erforderlich ist.|  
|HY010|Funktions Sequenz Fehler|(DM) **sqlbrowseconnetct** wurde für *connectionHandle* aufgerufen und SQL_NEED_DATA zurückgegeben. Diese Funktion wurde aufgerufen, bevor **sqlbrowseconnct** SQL_SUCCESS_WITH_INFO oder SQL_SUCCESS zurückgegeben hat.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect** oder **SQLMoreResults** wurde für *connectionHandle* aufgerufen und SQL_PARAM_DATA_AVAILABLE zurückgegeben. Diese Funktion wurde aufgerufen, bevor Daten für alle gestreuten Parameter abgerufen wurden.|  
|HY013|Speicher Verwaltungsfehler|Der Funktions Aufrufwert konnte nicht verarbeitet werden, da auf die zugrunde liegenden Speicher Objekte nicht zugegriffen werden konnte, möglicherweise aufgrund von wenig Arbeitsspeicher.|  
|HY090|Ungültige Zeichen folgen-oder Pufferlänge|(DM) *\* ValuePtr* ist eine Zeichenfolge, und BufferLength war kleiner als 0 (null), aber nicht gleich SQL_NTS.|  
|HY092|Ungültiger Attribut/Options Bezeichner|Der für das Argument- *Attribut* angegebene Wert war nicht gültig für die Version von ODBC, die vom Treiber unterstützt wird.|  
|HY114|Der Treiber unterstützt keine asynchrone Funktions Ausführung auf Verbindungs Ebene.|(DM) eine Anwendung hat versucht, die asynchrone Funktions Ausführung mit SQL_ATTR_ASYNC_DBC_FUNCTIONS_ENABLE für einen Treiber zu aktivieren, der asynchrone Verbindungs Vorgänge nicht unterstützt.|  
|HY117|Die Verbindung wurde aufgrund eines unbekannten Transaktions Zustands angehalten. Nur Disconnect-und Read-Only-Funktionen sind zulässig.|(DM) Weitere Informationen zum angehaltenen Status finden Sie unter [SQLEndTran Function](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Optionales Feature nicht implementiert|Der für das Argument- *Attribut* angegebene Wert war ein gültiges ODBC-Verbindungs Attribut für die Version von ODBC, die vom Treiber unterstützt, jedoch nicht vom Treiber unterstützt wurde.|  
|HYT01|Verbindungs Timeout abgelaufen|Der Verbindungs Timeout Zeitraum ist abgelaufen, bevor die Datenquelle auf die Anforderung geantwortet hat. Der Timeout Zeitraum für die Verbindung wird über **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT festgelegt.|  
|IM001|Der Treiber unterstützt diese Funktion nicht.|(DM) der Treiber, der dem *connectionHandle* entspricht, unterstützt die-Funktion nicht.|  
  
## <a name="comments"></a>Kommentare  
 Allgemeine Informationen zu Verbindungs Attributen finden Sie unter [Verbindungs Attribute](../../../odbc/reference/develop-app/connection-attributes.md).  
  
 Eine Liste der Attribute, die festgelegt werden können, finden Sie unter [SQLSetConnectAttr](../../../odbc/reference/syntax/sqlsetconnectattr-function.md). Beachten Sie Folgendes: Wenn *Attribute* ein Attribut angibt, das eine Zeichenfolge zurückgibt, muss *ValuePtr* ein Zeiger auf einen Puffer für die Zeichenfolge sein. Die maximale Länge der zurückgegebenen Zeichenfolge, einschließlich des NULL-Beendigungs Zeichens, wird als *BufferLength* -Byte verwendet.  
  
 Abhängig vom-Attribut muss eine Anwendung keine Verbindung herstellen, bevor **SQLGetConnectAttr** aufgerufen wird. Wenn jedoch **SQLGetConnectAttr** aufgerufen wird und das angegebene Attribut nicht über einen Standardwert verfügt und nicht durch einen vorherigen Aufruf von **SQLSetConnectAttr** festgelegt wurde, gibt **SQLGetConnectAttr** SQL_NO_DATA zurück.  
  
 Wenn *Attribute* SQL_ATTR_ Trace oder SQL_ATTR_ Tracefile ist, muss *connectionHandle* nicht gültig sein, und **SQLGetConnectAttr** gibt nicht SQL_ERROR oder SQL_INVALID_HANDLE zurück, wenn *connectionHandle* ungültig ist. Diese Attribute gelten für alle Verbindungen. **SQLGetConnectAttr** gibt SQL_ERROR oder SQL_INVALID_HANDLE zurück, wenn ein anderes Argument ungültig ist.  
  
 Obwohl eine Anwendung Anweisungs Attribute mithilfe von **SQLSetConnectAttr** festlegen kann, kann eine Anwendung **SQLGetConnectAttr** nicht verwenden, um Anweisungs Attributwerte abzurufen. Er muss **SQLGetStmtAttr** aufrufen, um die Einstellung der Anweisungs Attribute abzurufen.  
  
 Sowohl SQL_ATTR_AUTO_IPD als auch SQL_ATTR_CONNECTION_DEAD Verbindungs Attribute können durch einen **SQLGetConnectAttr** -Befehl zurückgegeben werden, können aber nicht durch einen-Befehl von **SQLSetConnectAttr** festgelegt werden.  
  
> [!NOTE]  
>  Es gibt keine asynchrone Unterstützung für **SQLGetConnectAttr**. Bei der Implementierung von **SQLGetConnectAttr** kann ein Treiber die Leistung verbessern, indem die Anzahl der vom Server gesendeten oder angeforderten Informationen minimiert wird.  
  
## <a name="related-functions"></a>Verwandte Funktionen  
  
|Informationen über|Finden Sie unter|  
|---------------------------|---------|  
|Zurückgeben der Einstellung eines Anweisungs Attributs|[SQLGetStmtAttr-Funktion](../../../odbc/reference/syntax/sqlgetstmtattr-function.md)|  
|Festlegen eines Verbindungs Attributs|[SQLSetConnectAttr-Funktion](../../../odbc/reference/syntax/sqlsetconnectattr-function.md)|  
|Festlegen eines Umgebungs Attributs|[SQLSetEnvAttr-Funktion](../../../odbc/reference/syntax/sqlsetenvattr-function.md)|  
|Festlegen eines Anweisungs Attributs|[SQLSetStmtAttr-Funktion](../../../odbc/reference/syntax/sqlsetstmtattr-function.md)|  
  
## <a name="see-also"></a>Weitere Informationen  
 [ODBC-API-Referenz](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [ODBC-Headerdateien](../../../odbc/reference/install/odbc-header-files.md)
