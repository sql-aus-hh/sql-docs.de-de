---
description: sys.dm_exec_query_plan (Transact-SQL)
title: sys.dm_exec_query_plan (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 08/02/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_exec_query_plan_TSQL
- sys.dm_exec_query_plan
- dm_exec_query_plan
- sys.dm_exec_query_plan_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_query_plan dynamic management function
ms.assetid: e26f0867-9be3-4b2e-969e-7f2840230770
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 18d6e4e8e3c6fe097676d1bf65f337610127566a
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094113"
---
# <a name="sysdm_exec_query_plan-transact-sql"></a>sys.dm_exec_query_plan (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Gibt für den vom Planhandle angegebenen Batch den Showplan im XML-Format zurück. Der vom Planhandle angegebene Plan ist möglicherweise zwischengespeichert oder wird gerade ausgeführt.  
  
Das XML-Schema für den Showplan ist veröffentlicht und auf [dieser Microsoft-Website](https://go.microsoft.com/fwlink/?linkid=43100&clcid=0x409)verfügbar. Es ist auch in dem Verzeichnis verfügbar, in dem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] installiert ist.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
sys.dm_exec_query_plan(plan_handle)  
```  
  
## <a name="arguments"></a>Argumente  
*plan_handle*  
Ein Token, das einen Abfrage Ausführungsplan für einen Batch eindeutig identifiziert, der ausgeführt wurde und dessen Plan sich im Plancache befindet oder gerade ausgeführt wird. *plan_handle* ist **varbinary(64)**   

*plan_handle* kann aus den folgenden dynamischen Verwaltungsobjekten abgerufen werden:
  
-   [sys.dm_exec_cached_plans &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md)  
  
-   [sys.dm_exec_query_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)  
  
-   [sys.dm_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)  

-   [sys.dm_exec_procedure_stats &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-procedure-stats-transact-sql.md)  

-   [sys.dm_exec_trigger_stats &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-trigger-stats-transact-sql.md)  
  
## <a name="table-returned"></a>Zurückgegebene Tabelle  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**DBID**|**smallint**|ID der Kontextdatenbank, die gültig war, als die diesem Plan entsprechende [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisung kompiliert wurde. Für Ad-hoc-Anweisungen und vorbereitete SQL-Anweisungen, die ID der Datenbank, in der die Anweisungen kompiliert wurden.<br /><br /> Die Spalte lässt NULL-Werte zu.|  
|**objectid**|**int**|ID des Objekts (z. B. gespeicherte Prozedur oder benutzerdefinierte Funktion) für diesen Abfrageplan. Für Ad-hoc- und vorbereitete Batches entspricht diese Spalte dem Wert **NULL**.<br /><br /> Die Spalte lässt NULL-Werte zu.|  
|**Zahl**|**smallint**|Gespeicherte Prozedur mit ganzer Zahl. Eine Gruppe von Prozeduren für die **orders**-Anwendung kann z. B. die Namen **orderproc;1**, **orderproc;2** usw. haben. Für Ad-hoc- und vorbereitete Batches entspricht diese Spalte dem Wert **NULL**.<br /><br /> Die Spalte lässt NULL-Werte zu.|  
|**.**|**bit**|Zeigt an, ob die entsprechende Prozedur verschlüsselt ist.<br /><br /> 0 = nicht verschlüsselt<br /><br /> 1 = verschlüsselt<br /><br /> NULL-Werte sind in der Spalte nicht zulässig.|  
|**query_plan**|**xml**|Enthält eine zur Kompilierzeit erstellte Showplandarstellung des Abfrageausführungsplans, der mit *plan_handle* angegeben ist. Der Showplan liegt im XML-Format vor. Für jeden Batch, der z. B. Ad-hoc- [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisungen, Aufrufe von gespeicherten Prozeduren und benutzerdefinierten Funktionen enthält, wird jeweils ein Plan generiert.<br /><br /> Die Spalte lässt NULL-Werte zu.|  
  
## <a name="remarks"></a>Bemerkungen  
 Unter den folgenden Bedingungen wird keine Showplanausgabe in der **query_plan**-Spalte der zurückgegebenen Tabelle für **sys.dm_exec_query_plan** zurückgegeben:  
  
-   Falls der mit *plan_handle* angegebene Abfrageplan aus dem Plancache entfernt wurde, enthält die **query_plan** -Spalte der zurückgegebenen Tabelle den Wert NULL. Diese Bedingung kann z. B. auftreten, wenn es eine Zeitverzögerung zwischen der Erfassung des Planhandles und seiner Verwendung mit **sys.dm_exec_query_plan** gibt.  
  
-   Einige [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisungen werden nicht zwischengespeichert. Beispiele hierfür sind Anweisungen für Massenvorgänge oder Anweisungen mit Zeichenfolgenliteralen, die größer als 8 KB sind. XML-Showpläne für diese Anweisungen können mit **sys.dm_exec_query_plan** nur abgerufen werden, wenn der Batch gerade ausgeführt wird, da sie im Cache nicht vorhanden sind.  
  
-   Wenn ein [!INCLUDE[tsql](../../includes/tsql-md.md)] -Batch oder eine gespeicherte Prozedur einen Aufruf für eine benutzerdefinierte Funktion oder einen Aufruf für eine dynamische SQL-Anweisung z. B. mit EXEC (*string*) enthält, ist der kompilierte XML-Showplan für die benutzerdefinierte Funktion nicht in der Tabelle enthalten, die von **sys.dm_exec_query_plan** für den Batch oder die gespeicherte Prozedur zurückgegeben wird. Stattdessen müssen Sie einen separaten Aufruf von **sys.dm_exec_query_plan** für das Planhandle erstellen, das der benutzerdefinierten Funktion entspricht.  
  
 Wenn bei einer Ad-hoc-Abfrage einfache oder erzwungene Parametrisierung verwendet wird, ist in der Spalte **query_plan** nur der Anweisungstext enthalten, nicht der tatsächliche Abfrageplan. Rufen Sie zum Zurückgeben des Abfrageplans **sys.dm_exec_query_plan** für das Planhandle der vorbereiteten parametrisierten Abfrage auf. Sie können anhand der **sql**-Spalte der [sys.syscacheobjects](../../relational-databases/system-compatibility-views/sys-syscacheobjects-transact-sql.md)-Sicht oder anhand der Textspalte der dynamischen [sys.dm_exec_sql_text](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md)-Verwaltungssicht ermitteln, ob die Abfrage parametrisiert wurde.  
  
> [!NOTE] 
> Aufgrund einer Beschränkung der im **xml** -Datentyp zulässigen Anzahl geschachtelter Ebenen kann **sys.dm_exec_query_plan** keine Abfragepläne zurückgeben, die 128 oder mehr Ebenen geschachtelter Elemente aufweisen. In früheren Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]verhinderte diese Bedingung das Zurückgeben des Abfrageplans, wobei der Fehler 6335 generiert wurde. In [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] Service Pack 2 und höheren Versionen gibt die **query_plan** -Spalte NULL zurück.   
> Mithilfe der [sys.dm_exec_text_query_plan &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-text-query-plan-transact-sql.md) dynamischen Verwaltungsfunktion können Sie die Ausgabe des Abfrage Plans im Textformat zurückgeben.  
  
## <a name="permissions"></a>Berechtigungen  
 Zum Ausführen **sys.dm_exec_query_plan** muss ein Benutzer Mitglied der festen Server Rolle **sysadmin** sein oder `VIEW SERVER STATE` über die Berechtigung auf dem Server verfügen.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird die Verwendung der dynamischen Verwaltungssicht **sys.dm_exec_query_plan** gezeigt.  
  
 Führen Sie zum Anzeigen der XML-Showpläne die folgenden Abfragen im Abfrage-Editor von [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] aus, und klicken Sie dann in der **query_plan**-Spalte der von **sys.dm_exec_query_plan** zurückgegebenen Tabelle auf **ShowPlanXML**. Der XML-Showplan wird im [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] -Zusammenfassungsbereich angezeigt. Um den XML-Showplan in einer Datei zu speichern, klicken Sie mit der rechten Maustaste in der Spalte **query_plan** auf **ShowPlanXML** , klicken Sie auf **Ergebnisse speichern** unter, und benennen Sie die Datei im Format \<*file_name*> . sqlplan, z. b. MyXMLShowplan. sqlplan.  
  
### <a name="a-retrieve-the-cached-query-plan-for-a-slow-running-transact-sql-query-or-batch"></a>A. Abrufen des zwischengespeicherten Abfrageplans für eine langsam ausgeführte Transact-SQL-Abfrage oder einen langsam ausgeführten Transact-SQL-Batch  
 Abfragepläne für unterschiedliche Typen von [!INCLUDE[tsql](../../includes/tsql-md.md)] -Batches z. B. Ad-hoc-Batches, gespeicherte Prozeduren und benutzerdefinierte Funktionen, werden in einem Bereich des Arbeitsspeichers zwischengespeichert, der als Plancache bezeichnet wird. Jeder zwischengespeicherte Abfrageplan wird durch einen eindeutigen Bezeichner identifiziert, der Planhandle genannt wird. Dieses Planhandle kann mit der dynamischen Verwaltungssicht **sys.dm_exec_query_plan** angegeben werden, um den Ausführungsplan für bestimmte [!INCLUDE[tsql](../../includes/tsql-md.md)]-Abfragen oder -Batches abzurufen.  
  
 Wenn eine [!INCLUDE[tsql](../../includes/tsql-md.md)] -Abfrage oder ein -Batch für lange Zeit mit einer bestimmten Verbindung mit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]ausgeführt wird, können Sie den Ausführungsplan für diese Abfrage oder diesen Batch abrufen, um die Ursache der Verzögerung zu ermitteln. Im folgenden Beispiel wird das Abrufen des XML-Showplans für eine langsam ausgeführte Abfrage oder einen Batch gezeigt.  
  
> [!NOTE]  
>  Ersetzen Sie zum Ausführen dieses Beispiels die Werte für *session_id* und *plan_handle* mit den Werten Ihres Servers.  
  
 Rufen Sie zunächst die Serverprozess-ID (SPID) für den Prozess ab, der die Abfrage oder den Batch ausführt, indem Sie die gespeicherte Prozedur `sp_who` verwenden:  
  
```sql  
USE master;  
GO  
exec sp_who;  
GO  
```  
  
 Das von `sp_who` zurückgegebene Resultset gibt die SPID `54`an. Verwenden Sie die SPID nun mit der dynamischen Verwaltungssicht `sys.dm_exec_requests` , um das Planhandle mithilfe der folgenden Abfrage abzurufen:  
  
```sql  
USE master;  
GO  
SELECT * FROM sys.dm_exec_requests  
WHERE session_id = 54;  
GO  
```  
  
 In der von **sys.dm_exec_requests** zurückgegebenen Tabelle wird angezeigt, dass das Planhandle für die langsam ausgeführte Abfrage oder den Batch `0x06000100A27E7C1FA821B10600`lautet. Geben Sie diesen Wert als *plan_handle* -Argument mit `sys.dm_exec_query_plan` an, um den Ausführungsplan im XML-Format wie folgt abzurufen. Der Ausführungsplan im XML-Format für die langsam ausgeführte Abfrage oder den Batch ist in der **query_plan**-Spalte der von `sys.dm_exec_query_plan` zurückgegebenen Tabelle enthalten.  
  
```sql  
USE master;  
GO  
SELECT * 
FROM sys.dm_exec_query_plan (0x06000100A27E7C1FA821B10600);  
GO  
```  
  
### <a name="b-retrieve-every-query-plan-from-the-plan-cache"></a>B. Abrufen jedes einzelnen Abfrageplans aus dem Plancache  
 Wenn Sie eine Momentaufnahme aller im Plancache gespeicherten Abfragen abrufen möchten, rufen Sie die Planhandles aller Abfragepläne im Cache ab, indem Sie die dynamische Verwaltungssicht `sys.dm_exec_cached_plans` abfragen. Die Planhandles sind in der `plan_handle` -Spalte von `sys.dm_exec_cached_plans`gespeichert. Verwenden Sie dann den CROSS APPLY-Operator, um die Planhandles wie folgt an `sys.dm_exec_query_plan` zu übergeben. Die XML-Showplanausgabe für jeden aktuell im Plancache gespeicherten Plan wird in der `query_plan` -Spalte der zurückgegebenen Tabelle angezeigt.  
  
```sql  
USE master;  
GO  
SELECT * 
FROM sys.dm_exec_cached_plans AS cp 
CROSS APPLY sys.dm_exec_query_plan(cp.plan_handle);  
GO  
```  
  
### <a name="c-retrieve-every-query-plan-for-which-the-server-has-gathered-query-statistics-from-the-plan-cache"></a>C. Abrufen jedes einzelnen Abfrageplans aus dem Plancache, für den der Server eine Abfragestatistik erfasst hat  
 Wenn Sie eine Momentaufnahme aller im Plancache gespeicherten Abfragen abrufen möchten, für die der Server eine Statistik erfasst hat, rufen Sie die Planhandles dieser Pläne im Cache ab, indem Sie die dynamische Verwaltungssicht `sys.dm_exec_query_stats` abfragen. Die Planhandles sind in der `plan_handle` -Spalte von `sys.dm_exec_query_stats`gespeichert. Verwenden Sie dann den CROSS APPLY-Operator, um die Planhandles wie folgt an `sys.dm_exec_query_plan` zu übergeben. Die XML-Showplanausgabe für jeden aktuell im Plancache gespeicherten Plan, für den der Server eine Statistik erfasst hat, wird in der `query_plan` -Spalte der zurückgegebenen Tabelle angezeigt.  
  
```sql  
USE master;  
GO  
SELECT * 
FROM sys.dm_exec_query_stats AS qs 
CROSS APPLY sys.dm_exec_query_plan(qs.plan_handle);  
GO  
```  
  
### <a name="d-retrieve-information-about-the-top-five-queries-by-average-cpu-time"></a>D: Abrufen von Informationen zu den fünf Abfragen mit dem höchsten durchschnittlichen CPU-Zeitaufwand  
 Im folgenden Beispiel werden die Pläne und die durchschnittliche CPU-Zeit für die fünf Abfragen mit der höchsten durchschnittlichen CPU-Zeit zurückgegeben.  
  
```sql  
SELECT TOP 5 total_worker_time/execution_count AS [Avg CPU Time],  
   plan_handle, query_plan   
FROM sys.dm_exec_query_stats AS qs  
CROSS APPLY sys.dm_exec_query_plan(qs.plan_handle)  
ORDER BY total_worker_time/execution_count DESC;  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Dynamische Verwaltungssichten und -funktionen &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [sys.dm_exec_cached_plans &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md)   
 [sys.dm_exec_query_stats &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)   
 [sys.dm_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)   
 [sp_who &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-who-transact-sql.md)   
 [Referenz zu logischen und physischen Showplanoperatoren](../../relational-databases/showplan-logical-and-physical-operators-reference.md)   
 [sys.dm_exec_text_query_plan &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-text-query-plan-transact-sql.md)  
  
  
