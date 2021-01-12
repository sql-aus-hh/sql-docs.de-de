---
description: sys.fn_trace_getinfo (Transact-SQL)
title: sys.fn_trace_getinfo (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- fn_trace_getinfo
- fn_trace_getinfo_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- traces [SQL Server], status information
- status information [SQL Server], traces
- sys.fn_trace_getinfo function
- fn_trace_getinfo function
ms.assetid: 04b140fe-110a-47b8-98b5-e4c161beb6c9
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: c7426552dd52732b6b22f9947862b8607418f482
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093810"
---
# <a name="sysfn_trace_getinfo-transact-sql"></a>sys.fn_trace_getinfo (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Gibt Informationen zu einer angegebene Ablaufverfolgung oder zu alle vorhandenen Ablaufverfolgungen zurück.  
  
> **WICHTIG!** [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Verwenden Sie stattdessen erweiterte Ereignisse.    
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sys.fn_trace_getinfo ( { trace_id | NULL | 0 | DEFAULT } )  
```  
  
## <a name="arguments"></a>Argumente  
 *trace_id*  
 Die ID der Ablaufverfolgung. *trace_id* ist vom Datentyp **int**.  Gültige Eingaben sind die ID-Nummer einer Ablauf Verfolgung, NULL, 0 oder default. NULL, 0 und DEFAULT sind in diesem Kontext gleichwertig. Geben Sie NULL, 0 oder DEFAULT an, wenn Informationen zu allen Ablaufverfolgungen in der Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] zurückgegeben werden sollen.  
  
## <a name="tables-returned"></a>Zurückgegebene Tabellen  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|traceid|**int**|ID der Ablaufverfolgung.|  
|property|**int**|Eigenschaft der Ablaufverfolgung:<br /><br /> 1= Ablaufverfolgungsoptionen. Weitere Informationen finden Sie unter @options in [sp_trace_create &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-trace-create-transact-sql.md).<br /><br /> 2 = Dateiname<br /><br /> 3 = Maximale Größe<br /><br /> 4 = Beendigungszeit<br /><br /> 5 = Aktueller Status der Ablaufverfolgung 0 = beendet. 1 = aktiv|  
|value|**sql_variant**|Informationen zur Eigenschaft der angegebenen Ablaufverfolgung.|  
  
## <a name="remarks"></a>Bemerkungen  
 Wird die ID einer bestimmten Ablaufverfolgung übergeben, gibt fn_trace_getinfo Informationen zu dieser Ablaufverfolgung zurück. Wird eine ungültige ID übergeben, gibt die Funktion ein leeres Rowset zurück.  
  
 fn_trace_getinfo fügt die Erweiterung *.trc an den Namen jeder Ablaufverfolgungsdatei an, die im Resultset enthalten ist. Weitere Informationen zum Definieren einer Ablauf Verfolgung finden Sie unter [sp_trace_create &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-trace-create-transact-sql.md). Ähnliche Informationen zu Ablauf Verfolgungs Filtern finden Sie unter [sys.fn_trace_getfilterinfo &#40;Transact-SQL-&#41;](../../relational-databases/system-functions/sys-fn-trace-getfilterinfo-transact-sql.md).  
  
 Ein umfassendes Beispiel für die Verwendung von gespeicherten Prozeduren für die Ablauf Verfolgung finden Sie unter [Erstellen einer Ablauf Verfolgung &#40;Transact-SQL&#41;](../../relational-databases/sql-trace/create-a-trace-transact-sql.md).  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die ALTER TRACE-Berechtigung auf dem Server.  
  
## <a name="examples"></a>Beispiele  
 Das folgende Beispiel gibt Informationen zu allen aktiven Ablaufverfolgungen zurück.  
  
```  
SELECT * FROM sys.fn_trace_getinfo(0) ;  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Erstellen einer Ablaufverfolgung &#40;Transact-SQL&#41;](../../relational-databases/sql-trace/create-a-trace-transact-sql.md)   
 [sp_trace_create &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-create-transact-sql.md)   
 [sp_trace_generateevent &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-trace-generateevent-transact-sql.md)   
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)   
 [sp_trace_setfilter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setfilter-transact-sql.md)   
 [sp_trace_setstatus &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setstatus-transact-sql.md)   
 [sys.fn_trace_getfilterinfo &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-trace-getfilterinfo-transact-sql.md)   
 [sys.fn_trace_geteventinfo &#40;Transact-SQL-&#41;](../../relational-databases/system-functions/sys-fn-trace-geteventinfo-transact-sql.md)   
 [sys.fn_trace_gettable &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-trace-gettable-transact-sql.md)  
  
  
