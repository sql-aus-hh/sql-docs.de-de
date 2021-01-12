---
description: sys.tcp_endpoints (Transact-SQL)
title: sys.tcp_endpoints (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.tcp_endpoints
- sys.tcp_endpoints_TSQL
- tcp_endpoints
- tcp_endpoints_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.tcp_endpoints catalog view
ms.assetid: 43cc3afa-cced-4463-8e97-fbfdaf2e4fa8
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: f5717389606bb3b73dc95c6a7eaedd6e03312cfd
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094423"
---
# <a name="systcp_endpoints-transact-sql"></a>sys.tcp_endpoints (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Enthält eine Zeile für jeden TCP-Endpunkt im System. Die durch **sys.tcp_endpoints** beschriebenen Endpunkte stellen ein Objekt für das Erteilen und Widerrufen des Verbindungsprivilegs bereit. Die zu Ports und IP-Adressen angezeigten Informationen werden nicht zum Konfigurieren der Protokolle verwendet und stimmen möglicherweise nicht mit der tatsächlichen Protokollkonfiguration überein. Verwenden Sie zum Anzeigen und Konfigurieren von Protokollen den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Konfigurations-Manager.  
  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**< geerbte Spalten>**||Erbt Spalten von [sys.endpoints](../../relational-databases/system-catalog-views/sys-endpoints-transact-sql.md).|  
|**port**|INT|Die Nummer des Ports, den der Endpunkt überwacht. Lässt keine NULL-Werte zu.|  
|**is_dynamic_port**|bit|1 = Portnummer wurde dynamisch zugewiesen.<br /><br /> Lässt keine NULL-Werte zu.|  
|**ip_address**|**nvarchar (45)**|Von der LISTENER_IP-Klausel angegebene Überwachungs-IP-Adresse. Lässt NULL-Werte zu.|  
  
## <a name="remarks"></a>Bemerkungen  
 Führen Sie die folgende Abfrage aus, um Informationen über Endpunkte und Verbindungen zu sammeln. Endpunkte ohne aktuelle oder TCP-Verbindungen werden als NULL-Werte angezeigt. Fügen Sie die **WHERE** -Klausel `WHERE des.session_id = @@SPID` hinzu, um Informationen zur aktuellen Verbindung zurückzugeben.  
  
```  
SELECT des.login_name, des.host_name, program_name,  dec.net_transport, des.login_time,   
e.name AS endpoint_name, e.protocol_desc, e.state_desc, e.is_admin_endpoint,   
t.port, t.is_dynamic_port, dec.local_net_address, dec.local_tcp_port   
FROM sys.endpoints AS e  
LEFT JOIN sys.tcp_endpoints AS t  
   ON e.endpoint_id = t.endpoint_id  
LEFT JOIN sys.dm_exec_sessions AS des  
   ON e.endpoint_id = des.endpoint_id  
LEFT JOIN sys.dm_exec_connections AS dec  
   ON des.session_id = dec.session_id;  
```  
  
## <a name="permissions"></a>Berechtigungen  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Weitere Informationen finden Sie unter [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Katalogsichten &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Endpoints Catalog Views &#40;Transact-SQL&#41; (Katalogsichten für Endpunkte &#40;Transact-SQL&#41;)](../../relational-databases/system-catalog-views/endpoints-catalog-views-transact-sql.md)  
  
  
