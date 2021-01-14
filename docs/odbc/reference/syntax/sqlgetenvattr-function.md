---
description: SQLGetEnvAttr-Funktion
title: SQLGetEnvAttr-Funktion | Microsoft-Dokumentation
ms.custom: ''
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
apiname:
- SQLGetEnvAttr
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLGetEnvAttr
helpviewer_keywords:
- SQLGetEnvAttr function [ODBC]
ms.assetid: 01f4590f-427a-4280-a1c3-18de9f7d86c1
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 457896a5c845bc42e446f1a115ee47863f7fced3
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171852"
---
# <a name="sqlgetenvattr-function"></a>SQLGetEnvAttr-Funktion
**Konformitäts**  
 Eingeführte Version: ODBC 3,0 Standards Compliance: ISO 92  
  
 **Zusammenfassung**  
 **SQLGetEnvAttr** gibt die aktuelle Einstellung eines Umgebungs Attributs zurück.  
  
## <a name="syntax"></a>Syntax  
  
```cpp  
  
SQLRETURN SQLGetEnvAttr(  
     SQLHENV        EnvironmentHandle,  
     SQLINTEGER     Attribute,  
     SQLPOINTER     ValuePtr,  
     SQLINTEGER     BufferLength,  
     SQLINTEGER *   StringLengthPtr);  
```  
  
## <a name="arguments"></a>Argumente  
 *Environment-THandle*  
 Der Umgebungs Handle.  
  
 *Attribut*  
 Der Das abzurufende Attribut.  
  
 *ValuePtr*  
 Ausgeben Ein Zeiger auf einen Puffer, in dem der aktuelle Wert des durch das *Attribut* angegebenen Attributs zurückgegeben werden soll.  
  
 Wenn *ValuePtr* den Wert NULL hat, gibt *stringlengthptr* weiterhin die Gesamtzahl der Bytes (ausgenommen des NULL-Beendigungs Zeichens für Zeichendaten) zurück, die im Puffer zurückgegeben werden können, auf den *ValuePtr* zeigt.  
  
 *BufferLength*  
 Der Wenn *ValuePtr* auf eine Zeichenfolge verweist, sollte dieses Argument der Länge von \* *ValuePtr* entsprechen. Wenn \* *ValuePtr* eine ganze Zahl ist, wird *BufferLength* ignoriert. Wenn *\* ValuePtr* eine Unicode-Zeichenfolge ist (beim Aufrufen von **sqlgetenvattrw**), muss das *BufferLength* -Argument eine gerade Zahl sein. Wenn der Attribut Wert keine Zeichenfolge ist, wird *BufferLength* nicht verwendet.  
  
 *Stringlengthptr*  
 Ausgeben Ein Zeiger auf einen Puffer, in dem die Gesamtzahl der Bytes (ausgenommen des NULL-Beendigungs Zeichens) zurückgegeben werden soll, die in *\* ValuePtr* zurückgegeben werden können. Wenn der Attribut Wert eine Zeichenfolge ist und die Anzahl von Bytes, die zurückgegeben werden können, größer oder gleich *BufferLength* ist, werden die Daten in \* *ValuePtr* auf *BufferLength* abzüglich der Länge eines NULL-Beendigungs Zeichens gekürzt und vom Treiber auf Null beendet.  
  
## <a name="returns"></a>Gibt zurück  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_NO_DATA, SQL_ERROR oder SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnose  
 Wenn **SQLGetEnvAttr** SQL_ERROR oder SQL_SUCCESS_WITH_INFO zurückgibt, kann ein zugeordneter SQLSTATE-Wert durch Aufrufen von **SQLGetDiagRec** mit dem *Handlertyp* SQL_HANDLE_ENV und einem *handle* von *environmenthandle* abgerufen werden. In der folgenden Tabelle sind die SQLSTATE-Werte aufgelistet, die von **SQLGetEnvAttr** häufig zurückgegeben werden, und die einzelnen Werte werden im Kontext dieser Funktion erläutert. die Notation "(DM)" geht vor den Beschreibungen von Sqlstates vor, die vom Treiber-Manager zurückgegeben werden. Der Rückgabecode, der den einzelnen SQLSTATE-Werten zugeordnet ist, ist SQL_ERROR, sofern nichts anderes angegeben ist.  
  
|SQLSTATE|Fehler|Beschreibung|  
|--------------|-----------|-----------------|  
|01000|Allgemeine Warnung|Treiber spezifische Informations Meldung. (Die Funktion gibt SQL_SUCCESS_WITH_INFO zurück.)|  
|01004|Zeichen folgen Daten, rechts abgeschnitten|Die in \* *ValuePtr* zurückgegebenen Daten wurden als *BufferLength* abzüglich des NULL-Beendigungs Zeichens abgeschnitten. Die Länge des nicht abgeschnittene Zeichen folgen Werts wird in **stringlengthptr* zurückgegeben. (Die Funktion gibt SQL_SUCCESS_WITH_INFO zurück.)|  
|HY000|Allgemeiner Fehler|Es ist ein Fehler aufgetreten, bei dem kein spezifischer SQLSTATE vorhanden war und für den kein Implementierungs spezifischer SQLSTATE definiert wurde. Die von **SQLGetDiagRec** im *\* MessageText* -Puffer zurückgegebene Fehlermeldung beschreibt den Fehler und die Ursache.|  
|HY001|Fehler bei der Speicher Belegung|Der Treiber konnte keinen Arbeitsspeicher zuweisen, der zur Unterstützung der Ausführung oder Beendigung der Funktion erforderlich ist.|  
|HY010|Funktions Sequenz Fehler|(DM) **SQL_ATTR_ODBC_VERSION** noch nicht über **SQLSetEnvAttr** festgelegt. Sie müssen **SQL_ATTR_ODBC_VERSION** nicht explizit festlegen, wenn Sie **sqlzuzugchandlestd** verwenden.|  
|HY013|Speicher Verwaltungsfehler|Der Funktions Aufrufwert konnte nicht verarbeitet werden, da auf die zugrunde liegenden Speicher Objekte nicht zugegriffen werden konnte, möglicherweise aufgrund von wenig Arbeitsspeicher.|  
|HY092|Ungültiger Attribut/Options Bezeichner|Der für das Argument- *Attribut* angegebene Wert war nicht gültig für die Version von ODBC, die vom Treiber unterstützt wird.|  
|HY117|Die Verbindung wurde aufgrund eines unbekannten Transaktions Zustands angehalten. Nur Disconnect-und Read-Only-Funktionen sind zulässig.|(DM) Weitere Informationen zum angehaltenen Status finden Sie unter [SQLEndTran Function](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Optionales Feature nicht implementiert|Der für das Argument- *Attribut* angegebene Wert war ein gültiges ODBC-Umgebungs Attribut für die Version von ODBC, die vom Treiber unterstützt, jedoch nicht vom Treiber unterstützt wurde.|  
|IM001|Der Treiber unterstützt diese Funktion nicht.|(DM) der Treiber, der dem *environmenthandle* entspricht, unterstützt die-Funktion nicht.|  
  
## <a name="comments"></a>Kommentare  
 Eine Liste der Attribute finden Sie unter [sqlsettenvattr](../../../odbc/reference/syntax/sqlsetenvattr-function.md). Es sind keine treiberspezifischen Umgebungs Attribute vorhanden. Wenn das *Attribut* ein Attribut angibt, das eine Zeichenfolge zurückgibt, muss *ValuePtr* ein Zeiger auf einen Puffer sein, in dem die Zeichenfolge zurückgegeben werden soll. Die maximale Länge der Zeichenfolge, einschließlich des NULL-Terminierungs Byte, ist *BufferLength* -bytes.  
  
 **SQLGetEnvAttr** kann jederzeit zwischen der Zuordnung und der Freigabe eines Umgebungs Handles aufgerufen werden. Alle Umgebungs Attribute, die von der Anwendung für die Umgebung erfolgreich festgelegt wurden, bleiben so lange erhalten, bis **SQLFreeHandle** für das *Environment-THandle* -Attribut mit dem *Typ* "SQL_HANDLE_ENV" aufgerufen wird. In ODBC 3 *. x* kann gleichzeitig mehr als ein Umgebungs Handle zugeordnet werden. Ein Umgebungs Attribut in einer Umgebung ist nicht betroffen, wenn eine andere Umgebung zugeordnet wurde.  
  
> [!NOTE]
>  Das SQL_ATTR_OUTPUT_NTS-Umgebungs Attribut wird von Standard kompatiblen Anwendungen unterstützt. Wenn **SQLGetEnvAttr** aufgerufen wird, gibt der ODBC 3 *. x* -Treiber-Manager immer SQL_TRUE für dieses Attribut zurück. SQL_ATTR_OUTPUT_NTS kann nur durch einen **SQLSetEnvAttr**-Befehl SQL_TRUE festgelegt werden.  
  
## <a name="related-functions"></a>Verwandte Funktionen  
  
|Informationen über|Finden Sie unter|  
|---------------------------|---------|  
|Zurückgeben der Einstellung eines Verbindungs Attributs|[SQLGetConnectAttr-Funktion](../../../odbc/reference/syntax/sqlgetconnectattr-function.md)|  
|Zurückgeben der Einstellung eines Anweisungs Attributs|[SQLGetStmtAttr-Funktion](../../../odbc/reference/syntax/sqlgetstmtattr-function.md)|  
|Festlegen eines Verbindungs Attributs|[SQLSetConnectAttr-Funktion](../../../odbc/reference/syntax/sqlsetconnectattr-function.md)|  
|Festlegen eines Umgebungs Attributs|[SQLSetEnvAttr-Funktion](../../../odbc/reference/syntax/sqlsetenvattr-function.md)|  
|Festlegen eines Anweisungs Attributs|[SQLSetStmtAttr-Funktion](../../../odbc/reference/syntax/sqlsetstmtattr-function.md)|  
  
## <a name="see-also"></a>Weitere Informationen  
 [ODBC-API-Referenz](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [ODBC-Headerdateien](../../../odbc/reference/install/odbc-header-files.md)
