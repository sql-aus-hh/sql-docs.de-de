---
description: sys.sysservers (Transact-SQL)
title: sys.sysServer (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.sysservers
- sysservers_TSQL
- sysservers
- sys.sysservers_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysservers system table
- sys.sysservers compatibility view
ms.assetid: d02f186f-c00f-44a6-b38d-dc78a3d2145b
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 1cf2a9649745eeeb5fbf10667b9849c0c1de5dec
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095328"
---
# <a name="syssysservers-transact-sql"></a>sys.sysservers (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Enthält eine Zeile für jeden Server, auf den eine Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] als OLE DB-Datenquelle zugreifen kann.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**srvid**|**smallint**|ID (nur für lokale Verwendung) des Remoteservers.|  
|**srvstatus**|**smallint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**srvname**|**sysname**|Name des Servers.|  
|**srvproduct**|**sysname**|Produktname für den Remoteserver.|  
|**Provider Name**|**nvarchar(128)**|Name des OLE DB-Anbieters für den Zugriff auf diesen Server.|  
|**DataSource**|**nvarchar(4000)**|OLE DB-Datenquellenwert.|  
|**location**|**nvarchar(4000)**|OLE DB-Speicherortwert.|  
|**Eigenschaften**|**nvarchar(4000)**|Zeichenfolgenwert des OLE DB-Anbieters.|  
|**schemadate**|**datetime**|Datum, an dem diese Zeile zuletzt aktualisiert wurde.|  
|**topologyx**|**int**|Wird nicht verwendet.|  
|**topologyy**|**int**|Wird nicht verwendet.|  
|**catalog**|**sysname**|Katalog, der beim Herstellen einer Verbindung mit einem OLE DB-Anbieter verwendet wird.|  
|**srvcollation**|**sysname**|Die Sortierung des Servers.|  
|**ConnectTimeout**|**int**|Timeouteinstellung für die Serververbindung.|  
|**QueryTimeout**|**int**|Timeouteinstellung für Abfragen auf den Server.|  
|**srvnetname**|**char (30)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**IsRemote**|**bit**|1 = Server ist ein Remoteserver.<br /><br /> 0 = Server ist ein Verbindungsserver.|  
|**RPC**|**bit**|1 = **sp_serveroption \@ RPC** ist auf **true** oder **on** festgelegt.<br /><br /> 0 = **sp_serveroption \@ RPC** ist auf **false** oder **Off** festgelegt.|  
|**knei**|**bit**|1 = **sp_serveroption \@ pub** ist auf **true** oder **on** festgelegt.<br /><br /> 0 = **sp_serveroption \@ pub** ist auf **false** oder **Off** festgelegt.|  
|**sub**|**bit**|1 = **sp_serveroption \@ Sub** ist auf **true** oder **on** festgelegt.<br /><br /> 0 = **sp_serveroption \@ Sub** ist auf **false** oder **Off** festgelegt.|  
|**dist**|**bit**|1 = **sp_serveroption \@ dist** ist auf **true** oder **on** festgelegt.<br /><br /> 0 = **sp_serveroption \@ dist** ist auf **false** oder **Off** festgelegt.|  
|**dpub**|**bit**|1 = **sp_serveroption \@ dpub ist** ist auf **true** oder **on** festgelegt.<br /><br /> 0 = **sp_serveroption \@ dpub ist** ist auf **false** oder **Off** festgelegt.|  
|**rpcout**|**bit**|1 = **sp_serveroption \@ RPC out** ist auf **true** oder **on** festgelegt.<br /><br /> 0 = **sp_serveroption \@ RPC out** ist auf **false** oder **Off** festgelegt.|  
|**DataAccess**|**bit**|1 = **sp_serveroption \@ Datenzugriff** ist auf **true** oder **on** festgelegt.<br /><br /> 0 = **sp_serveroption \@ Datenzugriff** ist auf **false** oder **Off** festgelegt.|  
|**collationcompatible**|**bit**|1 = **sp_serveroption \@ Sortierungs Kompatibilitäts** Wert ist auf **true** oder **on** festgelegt.<br /><br /> 0 = **sp_serveroption \@ Sortierungs Kompatibilitäts** Wert ist auf **false** oder **Off** festgelegt.|  
|**System**|**bit**|1 = **sp_serveroption \@ System** ist auf **true** oder **on** festgelegt.<br /><br /> 0 = **sp_serveroption \@ System** ist auf **false** oder **Off** festgelegt.|  
|**useremotecollation**|**bit**|1 = **sp_serveroption \@ Remote Sortierung** ist auf **true** oder **on** festgelegt.<br /><br /> 0 = **sp_serveroption \@ Remote Sortierung** ist auf **false** oder **Off** festgelegt.|  
|**lazyschemavalidation**|**bit**|1 = **sp_serveroption verzögerte \@ Schema Validierung** ist auf **true** oder **on** festgelegt.<br /><br /> 0 = **sp_serveroption verzögerte \@ Schema Validierung** ist auf **false** oder **Off** festgelegt.|  
|**Sortierung**|**sysname**|Die durch **sp_serveroption \@ Sortierungs Name** festgelegte Server Sortierung.|  
|**nonsqlsub**|bit|0 = Server ist eine Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]<br /><br /> 1 = Server ist keine Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Zuordnung von Systemtabellen zu System Sichten &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [Kompatibilitätssichten &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  
