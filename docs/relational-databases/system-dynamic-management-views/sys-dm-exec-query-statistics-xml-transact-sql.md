---
description: sys.dm_exec_query_statistics_xml (Transact-SQL)
title: sys.dm_exec_query_statistics_xml (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 11/16/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: conceptual
f1_keywords:
- sys.dm_exec_query_statistics_xml
- sys.dm_exec_query_statistics_xml_TSQL
- dm_exec_query_statistics_xml_TSQL
- dm_exec_query_statistics_xml
helpviewer_keywords:
- sys.dm_exec_query_statistics_xml management view
ms.assetid: fdc7659e-df41-488e-b2b5-0d79734dfecb
author: pmasl
ms.author: pelopes
ms.openlocfilehash: 9bfce6f56bdf39597def73884f12ca06f6eb56b7
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170272"
---
# <a name="sysdm_exec_query_statistics_xml-transact-sql"></a>sys.dm_exec_query_statistics_xml (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

Gibt den Abfrage Ausführungsplan für in-Flight-Anforderungen zurück. Verwenden Sie diese DMV zum Abrufen von Showplan XML mit vorübergehenden Statistiken. 

## <a name="syntax"></a>Syntax

```
sys.dm_exec_query_statistics_xml(session_id)  
``` 

## <a name="arguments"></a>Argumente 
*session_id*  
 Die Sitzungs-ID, die den Batch ausführt, der gesucht werden soll. *session_id* ist vom Datentyp **smallint**. *session_id* können aus den folgenden dynamischen Verwaltungs Objekten abgerufen werden:  
  
-   [sys.dm_exec_requests](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)  
  
-   [sys.dm_exec_sessions](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sessions-transact-sql.md)  
  
-   [sys.dm_exec_connections](../../relational-databases/system-dynamic-management-views/sys-dm-exec-connections-transact-sql.md)  

## <a name="table-returned"></a>Zurückgegebene Tabelle

|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|
|session_id|**smallint**|ID der Sitzung. Lässt keine NULL-Werte zu.|
|request_id|**int**|ID der Anforderung. Lässt keine NULL-Werte zu.|
|sql_handle|**varbinary(64)**|Ein Token, das den Batch oder die gespeicherte Prozedur eindeutig identifiziert, zu der die Abfrage gehört. NULL-Werte sind zulässig.|
|plan_handle|**varbinary(64)**|Ein Token, das einen Abfrage Ausführungsplan für einen momentan ausgeführten Batch eindeutig identifiziert. NULL-Werte sind zulässig.|
|query_plan|**xml**|Enthält die Showplan-Lauf Zeit Darstellung des Abfrage Ausführungs Plans, der mit *plan_handle* angegeben wird, die partielle Statistiken enthält. Der Showplan liegt im XML-Format vor. Für jeden Batch, der z. B. Ad-hoc- [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisungen, Aufrufe von gespeicherten Prozeduren und benutzerdefinierten Funktionen enthält, wird jeweils ein Plan generiert. NULL-Werte sind zulässig.|

## <a name="remarks"></a>Bemerkungen
Diese Systemfunktion ist ab SP1 verfügbar [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] . Siehe KB [3190871](https://support.microsoft.com/help/3190871)

Diese Systemfunktion funktioniert sowohl mit der **standardmäßigen** als auch der **Lightweight** -Abfrage für die Abfrage Ausführungs Statistik. Weitere Informationen finden Sie unter [Profilerstellungsinfrastruktur für Abfragen](../../relational-databases/performance/query-profiling-infrastructure.md).  

Unter den folgenden Bedingungen wird keine Show Plan Ausgabe in der **query_plan** -Spalte der zurückgegebenen Tabelle für **sys.dm_exec_query_statistics_xml** zurückgegeben:  
  
-   Wenn der Abfrageplan, der dem angegebenen *session_id* entspricht, nicht mehr ausgeführt wird, ist die **query_plan** -Spalte der zurückgegebenen Tabelle NULL. Diese Bedingung kann z. b. auftreten, wenn es eine Zeitverzögerung zwischen der Erfassung des Plan Handles und der Verwendung mit **sys.dm_exec_query_statistics_xml** gibt.  
    
Aufgrund einer Einschränkung in der Anzahl der zulässigen, im **XML** -Datentyp zulässigen Werte können **sys.dm_exec_query_statistics_xml** keine Abfrage Pläne zurückgeben, die 128 Ebenen von geschachtelte-Elementen erfüllen oder überschreiten. In früheren Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]verhinderte diese Bedingung das Zurückgeben des Abfrageplans, wobei der Fehler 6335 generiert wurde. In [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] Service Pack 2 und höheren Versionen gibt die **query_plan** -Spalte NULL zurück.   

## <a name="permissions"></a>Berechtigungen  
Unter [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ist `VIEW SERVER STATE` die-Berechtigung auf dem Server erforderlich.  
Bei [!INCLUDE[ssSDS](../../includes/sssds-md.md)] Premium-Tarifen ist die- `VIEW DATABASE STATE` Berechtigung in der Datenbank erforderlich. In [!INCLUDE[ssSDS](../../includes/sssds-md.md)] den Tarifen "Standard" und "Basic" ist der **Server Administrator** oder ein **Azure Active Directory Administrator** Konto erforderlich.

## <a name="examples"></a>Beispiele  
  
### <a name="a-looking-at-live-query-plan-and-execution-statistics-for-a-running-batch"></a>A. Überprüfen des Live-Abfrage Plans und der Ausführungs Statistik für einen laufenden Batch  
 Im folgenden Beispiel wird **sys.dm_exec_requests** abgefragt, um die interessante Abfrage zu suchen und deren `session_id` aus der Ausgabe zu kopieren.  
  
```sql  
SELECT * FROM sys.dm_exec_requests;  
GO  
```  
  
 Verwenden Sie dann zum Abrufen des Live-Abfrage Plans und der Ausführungs Statistik die kopierten `session_id` mit der Systemfunktion **sys.dm_exec_query_statistics_xml**.  
  
```sql  
--Run this in a different session than the session in which your query is running.
SELECT * FROM sys.dm_exec_query_statistics_xml(< copied session_id >);  
GO  
```   

 Oder kombiniert für alle laufenden Anforderungen.  
  
```sql  
--Run this in a different session than the session in which your query is running.
SELECT * FROM sys.dm_exec_requests
CROSS APPLY sys.dm_exec_query_statistics_xml(session_id);  
GO  
```   
  
## <a name="see-also"></a>Weitere Informationen
  [Ablaufverfolgungsflags](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md)  
 [Dynamische Verwaltungssichten und -funktionen &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Dynamische Verwaltungs Sichten im Zusammenhang mit der Datenbank &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)  

