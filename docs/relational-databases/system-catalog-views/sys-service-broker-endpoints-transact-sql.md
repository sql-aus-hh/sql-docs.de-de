---
description: sys.service_broker_endpoints (Transact-SQL)
title: sys.service_broker_endpoints (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.service_broker_endpoints_TSQL
- service_broker_endpoints
- service_broker_endpoints_TSQL
- sys.service_broker_endpoints
dev_langs:
- TSQL
helpviewer_keywords:
- sys.service_broker_endpoints catalog view
ms.assetid: 6979ec9b-0043-411e-aafb-0226fa26c5ba
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: d8e8a6590dedf9c43da14147c503d1e62423e595
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096689"
---
# <a name="sysservice_broker_endpoints-transact-sql"></a>sys.service_broker_endpoints (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Diese Katalogsicht enthält eine Zeile für den Service Broker-Endpunkt. Für jede Zeile in dieser Sicht gibt es eine entsprechende Zeile mit demselben **endpoint_id** in der **sys.tcp_endpoints** Sicht, die die TCP-Konfigurations Metadaten enthält. TCP ist das einzige für Service Broker zulässige Protokoll.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**\<inherited columns>**|**--**|Erbt Spalten von [sys. Endpoints &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-endpoints-transact-sql.md).|  
|**is_message_forwarding_enabled**|**bit**|Gibt an, ob die Nachrichtenweiterleitung vom Endpunkt unterstützt wird. Dies wird anfänglich auf **0** (deaktiviert) festgelegt. Lässt keine NULL-Werte zu.|  
|**message_forwarding_size**|**int**|Die maximale Anzahl von Megabyte des **tempdb** -Speicherplatzes, der für weitergeleitete Nachrichten verwendet werden darf. Dies ist anfänglich auf **10** festgelegt. Lässt keine NULL-Werte zu.|  
|**connection_auth**|**tinyint**|Der Typ von Verbindungsauthentifizierung, der für Verbindungen mit diesem Endpunkt erforderlich ist. Folgende Werte sind möglich:<br /><br /> **1** -NTLM<br /><br /> **2** : Kerberos<br /><br /> **3** -aushandeln<br /><br /> **4** : Zertifikat<br /><br /> **5** -NTLM, Zertifikat<br /><br /> **6** -Kerberos, Zertifikat<br /><br /> **7** -aushandeln, Zertifikat<br /><br /> **8** : Zertifikat, NTLM<br /><br /> **9** : Zertifikat, Kerberos<br /><br /> **10** -Zertifikat, Aushandlung<br /><br /> Lässt keine NULL-Werte zu.|  
|**connection_auth_desc**|**nvarchar(60)**|Die Beschreibung des Types von Verbindungsauthentifizierung, der für Verbindungen mit diesem Endpunkt erforderlich ist. Folgende Werte sind möglich:<br /><br /> NTLM<br /><br /> KERBEROS<br /><br /> NEGOTIATE<br /><br /> CERTIFICATE<br /><br /> NTLM, CERTIFICATE<br /><br /> KERBEROS, CERTIFICATE<br /><br /> NEGOTIATE, CERTIFICATE<br /><br /> CERTIFICATE, NTLM<br /><br /> CERTIFICATE, KERBEROS<br /><br /> CERTIFICATE, NEGOTIATE<br /><br /> Lässt NULL-Werte zu.|  
|**certificate_id**|**int**|Gegebenenfalls ID des für die Authentifizierung verwendeten Zertifikats.<br /><br /> 0 = Windows-Authentifizierung wird verwendet.|  
|**encryption_algorithm**|**tinyint**|Der Verschlüsselungsalgorithmus. Im folgenden sind die möglichen Werte mit ihren Beschreibungen und entsprechenden DDL-Optionen aufgeführt.<br /><br /> **0** : keine. Entsprechende DDL-Option: deaktiviert.<br /><br /> **1** : RC4. Entsprechende DDL-Option: {Required &#124; erforderlichen Algorithmus RC4}.<br /><br /> **2** : AES. Entsprechende DDL-Option: erforderlicher Algorithmus AES.<br /><br /> **3** : keine, RC4. Entsprechende DDL-Option: {Supported &#124; unterstützter Algorithmus RC4}.<br /><br /> **4** : keine, AES. Entsprechende DDL-Option: unterstützter Algorithmus AES.<br /><br /> **5** : RC4, AES. Entsprechende DDL-Option: erforderlicher Algorithmus RC4 AES.<br /><br /> **6** : AES, RC4. Entsprechende DDL-Option: erforderlicher Algorithmus AES RC4.<br /><br /> **7** : keine, RC4, AES. Entsprechende DDL-Option: unterstützter Algorithmus RC4 AES.<br /><br /> **8** : keine, AES, RC4. Entsprechende DDL-Option: unterstützter Algorithmus AES RC4.<br /><br /> Lässt keine NULL-Werte zu.|  
|**encryption_algorithm_desc**|**nvarchar(60)**|Beschreibung des Verschlüsselungsalgorithmus. Die möglichen Werte und ihre entsprechenden DDL-Optionen sind im folgenden aufgeführt:<br /><br /> Keine: deaktiviert<br /><br /> RC4: {Required &#124; erforderlichen Algorithmus RC4}<br /><br /> AES: erforderlicher Algorithmus AES<br /><br /> None, RC4: {Supported &#124; unterstützter Algorithmus RC4}<br /><br /> None, AES: unterstützter Algorithmus AES<br /><br /> RC4, AES: erforderlicher Algorithmus RC4 AES<br /><br /> AES, RC4: erforderlicher Algorithmus AES RC4<br /><br /> None, RC4, AES: unterstützter Algorithmus RC4 AES<br /><br /> None, AES, RC4: unterstützter Algorithmus AES RC4<br /><br /> Lässt NULL-Werte zu.|  
  
## <a name="remarks"></a>Bemerkungen  
  
> [!NOTE]  
>  Der RC4-Algorithmus wird nur aus Gründen der Abwärtskompatibilität unterstützt. Neues Material kann nur mit RC4 oder RC4_128 verschlüsselt werden, wenn die Datenbank den Kompatibilitätsgrad 90 oder 100 besitzt. (Nicht empfohlen.) Verwenden Sie stattdessen einen neueren Algorithmus, z. B. einen der AES-Algorithmen. In [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und höheren Versionen kann mit RC4 oder RC4_128 verschlüsseltes Material in jedem Kompatibilitätsgrad entschlüsselt werden.  
  
## <a name="permissions"></a>Berechtigungen  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Weitere Informationen finden Sie unter [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [ALTER ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/alter-endpoint-transact-sql.md)   
 [CREATE ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/create-endpoint-transact-sql.md)  
  
  
