---
description: dbo.sysnotifications (Transact-SQL)
title: dbo.sysBenachrichtigungen (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dbo.sysnotifications_TSQL
- sysnotifications
- sysnotifications_TSQL
- dbo.sysnotifications
dev_langs:
- TSQL
helpviewer_keywords:
- sysnotifications system table
ms.assetid: c5150d18-e8b7-48a7-ada7-77c583af6e41
author: cawrites
ms.author: chadam
ms.openlocfilehash: 59d02308629621e36044eb7272dcfe1ba40116b6
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096311"
---
# <a name="dbosysnotifications-transact-sql"></a>dbo.sysnotifications (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Enthält eine Zeile für jede Benachrichtigung.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**alert_id**|**int**|ID der Warnung.|  
|**operator_id**|**int**|ID des Operators, an den diese Benachrichtigung gesendet werden soll.|  
|**notification_method**|**tinyint**|Benachrichtigungsmethode:<br /><br /> **1** = E-Mail<br /><br /> **2** = Pager<br /><br /> **4**  =  " **nend** "<br /><br /> **7** = alle|  
  
  
