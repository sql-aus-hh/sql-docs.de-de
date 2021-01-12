---
description: sys.fn_trace_gettable (Transact-SQL)
title: sys.fn_trace_gettable (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- fn_trace_gettable
- fn_trace_gettable_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- fn_trace_gettable function
- sys.fn_trace_gettable function
ms.assetid: c2590159-6ec5-4510-81ab-e935cc4216cd
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 1f1593e1d12621b5dbe858b0f012322f447111e4
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101300"
---
# <a name="sysfn_trace_gettable-transact-sql"></a>sys.fn_trace_gettable (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Gibt den Inhalt mindestens einer Ablaufverfolgungsdatei in Tabellenform zurück.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Verwenden Sie stattdessen erweiterte Ereignisse.  
   
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
fn_trace_gettable ( 'filename' , number_files )  
```  
  
## <a name="arguments"></a>Argumente  
 '*Dateiname*'  
 Gibt die erste Ablaufverfolgungsdatei an, die gelesen werden soll. *Dateiname ist vom Datentyp* **nvarchar (256)** und hat keinen Standardwert.  
  
 *number_files*  
 Gibt die Anzahl der zu lesenden Rolloverdateien an. Diese Zahl schließt die in *filename* angegebene anfängliche Datei ein. *number_files* ist vom Datentyp **int**.  
  
## <a name="remarks"></a>Bemerkungen  
 Wenn *number_files* als **Standard** angegeben wird, liest **fn_trace_gettable** Alle Rolloverdateien, bis das Ende der Ablauf Verfolgung erreicht ist. **fn_trace_gettable** gibt eine Tabelle mit allen für die angegebene Ablauf Verfolgung gültigen Spalten zurück. Weitere Informationen finden Sie unter [sp_trace_setevent &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md).  
  
 Beachten Sie, dass die fn_trace_gettable Funktion keine Rolloverdateien lädt (wenn diese Option mithilfe des *number_files* -Arguments angegeben wird), wobei der Name der ursprünglichen Ablauf Verfolgungs Datei mit einem Unterstrich und einem numerischen Wert endet. (Dies gilt nicht für den Unterstrich und die Zahl, die automatisch angefügt werden, wenn für eine Datei ein Rollover ausgeführt wird.) Um dieses Problem zu umgehen, können Sie die Ablauf Verfolgungs Dateien umbenennen, um die Unterstriche im ursprünglichen Dateinamen zu entfernen. Wenn die ursprüngliche Datei z **. b. den Namen Trace_Oct_5. trc** hat und die Rolloverdatei **Trace_Oct_5_1. trc** heißt, können Sie die Dateien in **TraceOct5. trc** und **TraceOct5_1. trc** umbenennen.  
  
 Diese Funktion kann eine Ablaufverfolgung lesen, die noch auf der Instanz aktiv ist, auf der sie ausgeführt wird.  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die ALTER TRACE-Berechtigung auf dem Server.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-using-fn_trace_gettable-to-import-rows-from-a-trace-file"></a>A. Verwenden von fn_trace_gettable zum Importieren von Zeilen aus einer Ablaufverfolgungsdatei  
 Im folgenden Beispiel wird `fn_trace_gettable` innerhalb der `FROM`-Klausel einer `SELECT...INTO`-Anweisung aufgerufen.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT * INTO temp_trc  
FROM fn_trace_gettable('c:\temp\mytrace.trc', default);  
GO  
```  
  
### <a name="b-using-fn_trace_gettable-to-return-a-table-with-an-identity-column-that-can-be-loaded-into-a-sql-server-table"></a>B. Verwenden von fn_trace_gettable zum Zurückgeben einer Tabelle mit einer IDENTITY-Spalte, die in eine SQL Server-Tabelle geladen werden kann  
 Im folgenden Beispiel wird die Funktion als Teil einer `SELECT...INTO`-Anweisung aufgerufen. Die Funktion gibt eine Tabelle mit einer `IDENTITY`-Spalte zurück, die in die `temp_trc`-Tabelle geladen werden kann.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT IDENTITY(int, 1, 1) AS RowNumber, * INTO temp_trc  
FROM fn_trace_gettable('c:\temp\mytrace.trc', default);  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [sp_trace_generateevent &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-trace-generateevent-transact-sql.md)   
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)   
 [sp_trace_setfilter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setfilter-transact-sql.md)   
 [sp_trace_setstatus &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setstatus-transact-sql.md)  
  
  
