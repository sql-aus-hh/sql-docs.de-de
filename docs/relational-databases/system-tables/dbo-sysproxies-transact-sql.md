---
description: dbo.sysproxies (Transact-SQL)
title: dbo.sysProxys (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dbo.sysproxies_TSQL
- sysproxies_TSQL
- dbo.sysproxies
- sysproxies
dev_langs:
- TSQL
helpviewer_keywords:
- sysproxies system table
ms.assetid: a73da875-be22-45fc-b5e2-ea7ebd48e2d6
author: cawrites
ms.author: chadam
ms.openlocfilehash: 3d04cd1822a9c7e0edc309a3500e800b2df58fd0
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098317"
---
# <a name="dbosysproxies-transact-sql"></a>dbo.sysproxies (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Definiert Attribute eines [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent-Proxykontos. Diese Tabelle wird in der **msdb** -Datenbank gespeichert.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**proxy_id**|**int**|ID des Proxykontos.|  
|**name**|**sysname**|Name des Proxykontos.|  
|**credential_id**|**int**|ID der vom Proxykonto verwendeten Anmeldeinformationen.|  
|**enabled**|**tinyint**|Status des Proxykontos:<br /><br /> **0** = deaktiviert. **1** = aktiviert.|  
|**description**|**nvarchar(512)**|Beschreibung, die der Benutzer bei Erstellung des Proxykontos eingegeben hat.|  
|**user_sid**|**varbinary(85)**|Microsoft Windows-Sicherheits-ID ( *security_identifier* ) des Benutzers oder der Gruppe, der bzw. die den Proxyanmeldeinformationen zugeordnet ist.|  
|**credential_date_created**|**datetime**|Datum und Uhrzeit des Zeitpunkts, an dem die Anmeldeinformationen erstellt wurden.|  
  
## <a name="remarks"></a>Bemerkungen  
 Nur Mitglieder der festen Serverrolle **sysadmin** können auf die **sysproxies** -Tabelle zugreifen.  
  
## <a name="see-also"></a>Weitere Informationen  
 [dbo.sysproxylogin &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/dbo-sysproxylogin-transact-sql.md)   
 [dbo.sysproxysubsystem &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/dbo-sysproxysubsystem-transact-sql.md)   
 [dbo.sysSubsysteme &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/dbo-syssubsystems-transact-sql.md)  
  
  
