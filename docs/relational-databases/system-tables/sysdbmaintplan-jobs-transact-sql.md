---
description: sysdbmaintplan_jobs (Transact-SQL)
title: sysdbmaintplan_jobs (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sysdbmaintplan_jobs
- sysdbmaintplan_jobs_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysdbmaintplan_jobs system table
ms.assetid: bc65cd70-6ef2-4c17-be11-877ecf4efe50
author: cawrites
ms.author: chadam
ms.openlocfilehash: 3c1c925fca9ae3fab98e0cc3579a9105db855f71
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098081"
---
# <a name="sysdbmaintplan_jobs-transact-sql"></a>sysdbmaintplan_jobs (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Diese Tabelle ist in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] enthalten, um vorhandene Informationen für Instanzen beizubehalten, für die ein Update von einer früheren Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]durchgeführt wurde. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ändert den Inhalt dieser Tabelle nicht. Diese Tabelle wird in der **msdb** -Datenbank gespeichert.  
  
 [!INCLUDE[ssNoteDepNextAvoid](../../includes/ssnotedepnextavoid-md.md)]  
  

  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**plan_id**|**uniqueidentifier**|ID des Datenbankwartungsplans.|  
|**job_id**|**uniqueidentifier**|ID eines Auftrags, der dem Datenbank-Wartungsplan zugeordnet ist.|  
  
  
