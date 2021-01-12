---
description: sys.fn_get_sql (Transact-SQL)
title: sys.fn_get_sql (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- fn_get_sql
- sys.fn_get_sql_TSQL
- fn_get_sql_TSQL
- sys.fn_get_sql
dev_langs:
- TSQL
helpviewer_keywords:
- fn_get_sql function
- text [SQL Server], SQL handles
- sys.fn_get_sql function
- valid SQL handles [SQL Server]
- SQL handles
ms.assetid: d5fe49b5-0813-48f2-9efb-9187716b2fd4
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 4b3e28e4c66d45f28c6239431e8e6d5440d5d4a0
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093820"
---
# <a name="sysfn_get_sql-transact-sql"></a>sys.fn_get_sql (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Gibt den Text der SQL-Anweisung für das angegebene SQL-Handle zurück.  
  
> [!IMPORTANT]  
>  Dieses Feature wird in einer künftigen Version von Microsoft SQL Server entfernt. Nutzen Sie diese Funktionen bei Neuentwicklungen nicht mehr, und planen Sie die Änderung von Anwendungen, die diese Funktion zurzeit verwenden. Verwenden Sie stattdessen sys.dm_exec_sql_text. Weitere Informationen finden Sie unter [sys.dm_exec_sql_text &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md).  
  
 
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sys.fn_get_sql ( SqlHandle )  
```  
  
## <a name="arguments"></a>Argumente  
 *SqlHandle*  
 Wert des Handles. *SqlHandle* ist vom Datentyp **varbinary (64)** und hat keinen Standardwert.  
  
## <a name="tables-returned"></a>Zurückgegebene Tabellen  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|dbid|**smallint**|Datenbank-ID Für Ad-hoc-Anweisungen und vorbereitete SQL-Anweisungen, die ID der Datenbank, in der die Anweisungen kompiliert wurden.|  
|objectid|**int**|ID des Datenbankobjekts. Dieser Wert ist für Ad-hoc-SQL-Anweisungen NULL.|  
|number|**smallint**|Gibt die Nummer der Gruppe an, wenn die Prozeduren gruppiert sind.<br /><br /> 0 = Einträge sind keine Prozeduren.<br /><br /> NULL = Ad-hoc-SQL-Anweisungen.|  
|encrypted|**bit**|Zeigt an, ob das Objekt verschlüsselt ist.<br /><br /> 0 = Nicht verschlüsselt<br /><br /> 1 = Verschlüsselt.|  
|Text|**text**|Der Text der SQL-Anweisung. Der Wert ist für verschlüsselte Objekte NULL.|  
  
## <a name="remarks"></a>Bemerkungen  
 Sie können ein gültiges SQL-Handle aus der Spalte sql_handle der [sys.dm_exec_requests &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md) dynamische Verwaltungs Sicht abrufen.  
  
 Wenn Sie ein Handle übergeben, das nicht mehr im Cache vorhanden ist, gibt fn_get_sq **l** ein leeres Resultset zurück. Wenn Sie ein ungültiges Handle übergeben, wird die Ausführung des Batches beendet und eine Fehlermeldung zurückgegeben.  
  
 Der [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] kann einige Anweisungen nicht zwischenspeichern [!INCLUDE[tsql](../../includes/tsql-md.md)] , z. b. Massen Kopier Anweisungen und Anweisungen mit Zeichenfolgenliteralen, die größer als 8 KB sind. Handles für diese Anweisungen können nicht mithilfe von fn_get_sql abgerufen werden.  
  
 Die **Text** -Spalte des Resultsets wird nach Text gefiltert, der möglicherweise Kenn Wörter enthält. Weitere Informationen zu sicherheitsbezogenen gespeicherten Prozeduren, die nicht überwacht werden, finden Sie unter [Filtern einer Ablauf Verfolgung](../../relational-databases/sql-trace/filter-a-trace.md).  
  
 Die fn_get_sql-Funktion gibt Informationen zurück, die dem [DBCC INPUTBUFFER](../../t-sql/database-console-commands/dbcc-inputbuffer-transact-sql.md) -Befehl ähneln. Im Folgenden handelt es sich um Beispiele, in denen die fn_get_sql-Funktion verwendet werden kann, weil DBCC INPUTBUFFER nicht verwendet werden kann:  
  
-   Ereignisse umfassen mehr als 255 Zeichen.  
  
-   Sie möchten die höchste aktuelle Schachtelungsebene einer gespeicherten Prozedur zurückgeben. Beispiel: Es gibt zwei gespeicherte Prozeduren namens sp_1 und sp_2. Wenn sp_1 die Prozedur sp_2 aufruft und Sie das Handle von der dynamischen Verwaltungssicht sys.dm_exec_requests erhalten, während sp_2 ausgeführt wird, gibt die fn_get_sql-Funktion Informationen zu sp_2 zurück. Außerdem gibt die fn_get_sql-Funktion den gesamten Text der gespeicherten Prozedur auf der höchsten aktuellen Schachtelungsebene zurück.  
  
## <a name="permissions"></a>Berechtigungen  
 Der Benutzer benötigt die View Server State-Berechtigung auf dem Server.  
  
## <a name="examples"></a>Beispiele  
 Datenbankadministratoren können die fn_get_sql-Funktion, wie im folgenden Beispiel gezeigt, für die Diagnose von Problemprozessen verwenden. Nachdem ein Administrator eine Problem Sitzungs-ID identifiziert hat, kann der Administrator das SQL-Handle für diese Sitzung abrufen, fn_get_sql mit dem Handle aufrufen und dann mithilfe der Start-und Endoffsets den SQL-Text der Problem Sitzungs-ID ermitteln.  
  
```  
DECLARE @Handle varbinary(64);  
SELECT @Handle = sql_handle   
FROM sys.dm_exec_requests   
WHERE session_id = 52 and request_id = 0;  
SELECT * FROM sys.fn_get_sql(@Handle);  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [DBCC INPUTBUFFER &#40;Transact-SQL-&#41;](../../t-sql/database-console-commands/dbcc-inputbuffer-transact-sql.md)   
 [sys.sysProzesse &#40;Transact-SQL-&#41;](../../relational-databases/system-compatibility-views/sys-sysprocesses-transact-sql.md)   
 [sys.dm_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)  
  
  
