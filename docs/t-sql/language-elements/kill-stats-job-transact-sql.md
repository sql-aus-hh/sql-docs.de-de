---
description: KILL STATS JOB (Transact-SQL)
title: KILL STATS JOB (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 07/27/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- KILL STATS JOB
- KILL_STATS_JOB_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- ending statistics update jobs [SQL Server]
- stopping statistics update jobs
- terminating statistics update jobs
- asynchronous statistics updates [SQL Server]
- KILL STATS JOB statement
- statistics update jobs [SQL Server]
ms.assetid: 55a8f9f1-3259-45c0-8ab9-60b9c088b4b4
author: cawrites
ms.author: chadam
ms.openlocfilehash: c862b3f53654278aeb1a6e69cd30ebb52dca3732
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98084558"
---
# <a name="kill-stats-job-transact-sql"></a>KILL STATS JOB (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Beendet einen asynchronen Statistikupdateauftrag in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Update.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
KILL STATS JOB job_id   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *job_id*  
 Ist das job_id-Feld, das von der dynamischen Verwaltungssicht sys.dm_exec_background_job_queue für den Auftrag zurückgegeben wird.  
  
## <a name="remarks"></a>Bemerkungen  
 job_id hängt nicht mit session_id oder der in anderen Formen der KILL-Anweisung verwendeten Arbeitseinheit zusammen.  
  
## <a name="permissions"></a>Berechtigungen  
 Benutzer benötigen die VIEW SERVER STATE-Berechtigung, um auf Informationen in der dynamischen Verwaltungssicht sys.dm_exec_background_job_queue zuzugreifen.  
  
 KILL STATS JOB-Berechtigungen erhalten standardmäßig die Mitglieder der festen Datenbankrollen sysadmin und processadmin; sie sind nicht übertragbar.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird veranschaulicht, wie das mit einem Auftrag verknüpfte Statistikupdate beendet wird, wobei die *job_id* = `53` ist.  
  
```sql  
KILL STATS JOB 53;  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [KILL &#40;Transact-SQL&#41;](../../t-sql/language-elements/kill-transact-sql.md)   
 [KILL QUERY NOTIFICATION SUBSCRIPTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/kill-query-notification-subscription-transact-sql.md)   
 [sys.dm_exec_background_job_queue &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-background-job-queue-transact-sql.md)   
 [Statistik](../../relational-databases/statistics/statistics.md)  
  
  
