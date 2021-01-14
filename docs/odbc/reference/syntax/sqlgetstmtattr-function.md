---
description: SQLGetStmtAttr-Funktion
title: SQLGetStmtAttr-Funktion | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
apiname:
- SQLGetStmtAttr
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLGetStmtAttr
helpviewer_keywords:
- SQLGetStmtAttr function [ODBC]
ms.assetid: e321d460-e997-4527-aee6-207cf5a498e9
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f6af1813068b51780c9181f3e3e98559769495fe
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172372"
---
# <a name="sqlgetstmtattr-function"></a>SQLGetStmtAttr-Funktion
**Konformitäts**  
 Eingeführte Version: ODBC 3,0 Standards Compliance: ISO 92  
  
 **Zusammenfassung**  
 **SQLGetStmtAttr** gibt die aktuelle Einstellung eines Anweisungs Attributs zurück.  
  
> [!NOTE]  
>  Weitere Informationen dazu, wie der Treiber-Manager diese Funktion bei ODBC 3 zuordnet. die *x* -Anwendung arbeitet mit ODBC 2. *x* -Treiber finden Sie unter [Mapping Replace Functions for abwärts Compatibility of Applications](../../../odbc/reference/develop-app/mapping-replacement-functions-for-backward-compatibility-of-applications.md).  
  
## <a name="syntax"></a>Syntax  
  
```cpp  
  
SQLRETURN SQLGetStmtAttr(  
     SQLHSTMT        StatementHandle,  
     SQLINTEGER      Attribute,  
     SQLPOINTER      ValuePtr,  
     SQLINTEGER      BufferLength,  
     SQLINTEGER *    StringLengthPtr);  
```  
  
## <a name="arguments"></a>Argumente  
 *StatementHandle*  
 Der Anweisungs Handle.  
  
 *Attribut*  
 Der Das abzurufende Attribut.  
  
 *ValuePtr*  
 Ausgeben Ein Zeiger auf einen Puffer, in den der Wert des im *Attribut* angegebenen Attributs zurückgegeben werden soll.  
  
 Wenn *ValuePtr* den Wert NULL hat, gibt *stringlengthptr* weiterhin die Gesamtzahl der Bytes (ausgenommen des NULL-Beendigungs Zeichens für Zeichendaten) zurück, die im Puffer zurückgegeben werden können, auf den *ValuePtr* zeigt.  
  
 *BufferLength*  
 Der Wenn *Attribute* ein ODBC-definiertes Attribut ist und *ValuePtr* auf eine Zeichenfolge oder einen binären Puffer zeigt, sollte dieses Argument der Länge von \* *ValuePtr* entsprechen. Wenn *Attribute* ein ODBC-definiertes Attribut und \* *ValuePtr* eine ganze Zahl ist, wird *BufferLength* ignoriert. Wenn der in *\* ValuePtr* zurückgegebene Wert eine Unicode-Zeichenfolge ist (beim Aufrufen von **sqlgetstmtattrw**), muss das *BufferLength* -Argument eine gerade Zahl sein.  
  
 Wenn *Attribute* ein Treiber definiertes Attribut ist, gibt die Anwendung die Art des Attributs für den Treiber-Manager an, indem das *BufferLength* -Argument festgelegt wird. *BufferLength* kann die folgenden Werte aufweisen:  
  
-   Wenn *\* ValuePtr* ein Zeiger auf eine Zeichenfolge ist, entspricht *BufferLength* der Länge der Zeichenfolge oder SQL_NTS.  
  
-   Wenn *\* ValuePtr* ein Zeiger auf einen binären Puffer ist, platziert die Anwendung das Ergebnis des Makros SQL_LEN_BINARY_ATTR (*length*) in *BufferLength*. Dadurch wird ein negativer Wert in *BufferLength* platziert.  
  
-   Wenn *\* ValuePtr* ein Zeiger auf einen anderen Wert als eine Zeichenfolge oder eine binäre Zeichenfolge ist, sollte *BufferLength* den Wert SQL_IS_POINTER.  
  
-   Wenn *\* ValuePtr* einen Datentyp mit fester Länge enthält, ist *BufferLength* entweder SQL_IS_INTEGER oder SQL_IS_UINTEGER.  
  
 *Stringlengthptr*  
 Ausgeben Ein Zeiger auf einen Puffer, in dem die Gesamtzahl der Bytes (ausgenommen des NULL-Beendigungs Zeichens) zurückgegeben werden soll, die in *\* ValuePtr* zurückgegeben werden können. Wenn der Attribut Wert eine Zeichenfolge ist und die Anzahl von Bytes, die zurückgegeben werden können, größer oder gleich *BufferLength* ist, werden die Daten in *\* ValuePtr* auf *BufferLength* abzüglich der Länge eines NULL-Beendigungs Zeichens gekürzt und vom Treiber auf NULL-terminiert.  
  
## <a name="returns"></a>Gibt zurück  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR oder SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnose  
 Wenn **SQLGetStmtAttr** SQL_ERROR oder SQL_SUCCESS_WITH_INFO zurückgibt, kann ein zugeordneter SQLSTATE-Wert durch Aufrufen von **SQLGetDiagRec** mit dem *Handlertyp* SQL_HANDLE_STMT und einem *handle* von *StatementHandle* abgerufen werden. In der folgenden Tabelle sind die SQLSTATE-Werte aufgelistet, die häufig von **SQLGetStmtAttr** zurückgegeben werden, und die einzelnen Werte werden im Kontext dieser Funktion erläutert. die Notation "(DM)" geht vor den Beschreibungen von Sqlstates vor, die vom Treiber-Manager zurückgegeben werden. Der Rückgabecode, der den einzelnen SQLSTATE-Werten zugeordnet ist, ist SQL_ERROR, sofern nichts anderes angegeben ist.  
  
|SQLSTATE|Fehler|Beschreibung|  
|--------------|-----------|-----------------|  
|01000|Allgemeine Warnung|Treiber spezifische Informations Meldung. (Die Funktion gibt SQL_SUCCESS_WITH_INFO zurück.)|  
|01004|Zeichen folgen Daten, rechts abgeschnitten|Die in *\* ValuePtr* zurückgegebenen Daten wurden als *BufferLength* abzüglich der Länge eines NULL-Beendigungs Zeichens abgeschnitten. Die Länge des nicht abgeschnittene Zeichen folgen Werts wird in **stringlengthptr* zurückgegeben. (Die Funktion gibt SQL_SUCCESS_WITH_INFO zurück.)|  
|24.000|Ungültiger Cursorstatus|Das Argument *Attribut* wurde SQL_ATTR_ROW_NUMBER, und der Cursor war nicht geöffnet, oder der Cursor befand sich vor dem Anfang des Resultsets oder nach dem Ende des Resultsets.|  
|HY000|Allgemeiner Fehler|Es ist ein Fehler aufgetreten, bei dem kein spezifischer SQLSTATE vorhanden war und für den kein Implementierungs spezifischer SQLSTATE definiert wurde. Die von **SQLGetDiagRec** im Argument *MessageText* zurückgegebene Fehlermeldung beschreibt den Fehler und seine Ursache.|  
|HY001|Fehler bei der Speicher Belegung|Der Treiber konnte keinen Arbeitsspeicher zuweisen, der zur Unterstützung der Ausführung oder Beendigung der Funktion erforderlich ist.|  
|HY010|Funktions Sequenz Fehler|(DM) eine asynchron ausgeführte Funktion wurde für das Verbindungs Handle aufgerufen, das mit dem *StatementHandle* verknüpft ist. Diese asynchrone Funktion wurde noch ausgeführt, als die **SQLGetStmtAttr** -Funktion aufgerufen wurde.<br /><br /> (DM) eine asynchron ausgeführte Funktion wurde für das *StatementHandle* aufgerufen und ausgeführt, als diese Funktion aufgerufen wurde.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** oder **SQLSetPos** wurde für das *StatementHandle* aufgerufen und SQL_NEED_DATA zurückgegeben. Diese Funktion wurde aufgerufen, bevor Daten für alle Data-at-Execution-Parameter oder-Spalten gesendet wurden.|  
|HY013|Speicher Verwaltungsfehler|Der Funktions Aufrufwert konnte nicht verarbeitet werden, da auf die zugrunde liegenden Speicher Objekte nicht zugegriffen werden konnte, möglicherweise aufgrund von wenig Arbeitsspeicher.|  
|HY090|Ungültige Zeichen folgen-oder Pufferlänge|*(DM) \* ValuePtr* ist eine Zeichenfolge, und BufferLength war kleiner als 0 (null), aber nicht gleich SQL_NTS.|  
|HY092|Ungültiger Attribut/Options Bezeichner|Der für das Argument- *Attribut* angegebene Wert war nicht gültig für die Version von ODBC, die vom Treiber unterstützt wird.|  
|HY109|Ungültige Cursorposition|Das *Attribut* Argument wurde SQL_ATTR_ROW_NUMBER, und die Zeile wurde gelöscht oder konnte nicht abgerufen werden.|  
|HY117|Die Verbindung wurde aufgrund eines unbekannten Transaktions Zustands angehalten. Nur Disconnect-und Read-Only-Funktionen sind zulässig.|(DM) Weitere Informationen zum angehaltenen Status finden Sie unter [SQLEndTran Function](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Optionales Feature nicht implementiert|Der für das Argument- *Attribut* angegebene Wert war ein gültiges ODBC-Anweisungs Attribut für die Version von ODBC, die vom Treiber unterstützt, jedoch nicht vom Treiber unterstützt wurde.|  
|HYT01|Verbindungs Timeout abgelaufen|Der Verbindungs Timeout Zeitraum ist abgelaufen, bevor die Datenquelle auf die Anforderung geantwortet hat. Der Timeout Zeitraum für die Verbindung wird über **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT festgelegt.|  
|IM001|Der Treiber unterstützt diese Funktion nicht.|(DM) der der *StatementHandle* entsprechende Treiber unterstützt die-Funktion nicht.|  
  
## <a name="comments"></a>Kommentare  
 Allgemeine Informationen zu Anweisungs Attributen finden Sie unter [Anweisungs Attribute](../../../odbc/reference/develop-app/statement-attributes.md).  
  
 Ein-Befehl von **SQLGetStmtAttr** gibt in *\* ValuePtr* den Wert des Anweisungs Attributs zurück, das im- *Attribut* angegeben ist. Dieser Wert kann entweder ein SQLULEN-Wert oder eine auf NULL endende Zeichenfolge sein. Wenn es sich bei dem Wert um einen SQLULEN-Wert handelt, können einige Treiber nur den unteren 32-Bit-oder 16-Bit-Puffer eines Puffers schreiben und das Bit höherer Ordnung unverändert lassen. Daher sollten Anwendungen einen Puffer von SQLULEN verwenden und den Wert auf 0 initialisieren, bevor Sie diese Funktion aufrufen. Außerdem werden die Argumente *BufferLength* und *stringlengthptr* nicht verwendet. Wenn der Wert eine NULL-terminierte Zeichenfolge ist, gibt die Anwendung die maximale Länge dieser Zeichenfolge im *BufferLength* -Argument an, und der Treiber gibt die Länge dieser Zeichenfolge im *\* stringlengthptr* -Puffer zurück.  
  
 , Damit Anwendungen, die **SQLGetStmtAttr** aufrufen, mit ODBC 2 funktionieren. *x* -Treiber: der **SQLGetStmtAttr** -Befehl wird im Treiber-Manager **SQLGetStmtOption** zugeordnet.  
  
 Die folgenden Anweisungs Attribute sind schreibgeschützt, sodass Sie von **SQLGetStmtAttr** abgerufen, aber nicht von **SQLSetStmtAttr** festgelegt werden können:  
  
-   SQL_ATTR_IMP_PARAM_DESC  
  
-   SQL_ATTR_IMP_ROW_DESC  
  
-   SQL_ATTR_ROW_NUMBER  
  
 Eine Liste der Attribute, die festgelegt und abgerufen werden können, finden Sie unter [SQLSetStmtAttr](../../../odbc/reference/syntax/sqlsetstmtattr-function.md).  
  
## <a name="related-functions"></a>Verwandte Funktionen  
  
|Informationen über|Finden Sie unter|  
|---------------------------|---------|  
|Zurückgeben der Einstellung eines Verbindungs Attributs|[SQLGetConnectAttr-Funktion](../../../odbc/reference/syntax/sqlgetconnectattr-function.md)|  
|Festlegen eines Verbindungs Attributs|[SQLSetConnectAttr-Funktion](../../../odbc/reference/syntax/sqlsetconnectattr-function.md)|  
|Festlegen eines Anweisungs Attributs|[SQLSetStmtAttr-Funktion](../../../odbc/reference/syntax/sqlsetstmtattr-function.md)|  
  
## <a name="see-also"></a>Weitere Informationen  
 [ODBC-API-Referenz](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [ODBC-Headerdateien](../../../odbc/reference/install/odbc-header-files.md)
