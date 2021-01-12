---
description: MSsubscription_properties (Transact-SQL)
title: MSsubscription_properties (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSsubscription_properties
- MSsubscription_properties_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSsubscription_properties system table
ms.assetid: f96fc1ae-b798-4b05-82a7-564ae6ef23b8
author: cawrites
ms.author: chadam
ms.openlocfilehash: 9927dd1e15f04a073d0fb3cf7f95e13b1f425e0c
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100493"
---
# <a name="mssubscription_properties-transact-sql"></a>MSsubscription_properties (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Die **MSsubscription_properties** Tabelle enthält Zeilen für die Parameterinformationen, die zum Ausführen von Replikations-Agents auf dem Abonnenten erforderlich sind. Diese Tabelle wird für ein Pullabonnement auf dem Abonnenten in der Abonnementdatenbank und für ein Pushabonnement auf dem Verteiler in der Verteilungsdatenbank gespeichert.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**publisher**|**sysname**|Der Name des Verlegers.|  
|**publisher_db**|**sysname**|Der Name der Verlegerdatenbank.|  
|**ung**|**sysname**|Der Name der Veröffentlichung.|  
|**publication_type**|**int**|Der Typ der Veröffentlichung:<br /><br /> **0** = transaktional.<br /><br /> **2** = Merge.|  
|**publisher_login**|**sysname**|Die Anmelde-ID, die auf dem Verleger für die Authentifizierung verwendet wird [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|**publisher_password**|**nvarchar (524)**|Das (verschlüsselte) Kennwort, das auf dem Verleger für die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Authentifizierung verwendet wird.|  
|**publisher_security_mode**|**int**|Auf dem Verleger implementierter Sicherheitsmodus:<br /><br /> **0**  =  [!INCLUDE[msCoName](../../includes/msconame-md.md)] SQL Server-Authentifizierung.<br /><br /> **1**  =  [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows-Authentifizierung.<br /><br /> **2** = die Synchronisierungs Trigger verwenden einen statischen **sysservers** -Eintrag für einen Remote Prozedur Aufruf (RPC), und der *Herausgeber* muss in der **sysservers** -Tabelle als Remote Server oder Verbindungs Server definiert sein.|  
|**Verleih**|**sysname**|Der Name des Verteilers.|  
|**distributor_login**|**sysname**|Die Anmelde-ID, die auf dem Verteiler für die SQL Server-Authentifizierung verwendet wird.|  
|**distributor_password**|**nvarchar (524)**|Das Kennwort (verschlüsselt), das auf dem Verteiler für die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Authentifizierung verwendet wird.|  
|**distributor_security_mode**|**int**|Der auf dem Verteiler implementierte Sicherheitsmodus:<br /><br /> **0** -  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Authentifizierung.<br /><br /> **1** = Windows-Authentifizierung.|  
|**ftp_address**|**sysname**|Die Netzwerkadresse des FTP-Diensts (File Transfer Protocol) für den Verteiler.|  
|**ftp_port**|**int**|Die Portnummer des FTP-Dienstanbieter für den Verteiler.|  
|**ftp_login**|**sysname**|Der zum Herstellen einer Verbindung mit dem FTP-Dienst verwendete Benutzername.|  
|**ftp_password**|**nvarchar (524)**|Das zum Herstellen der Verbindung mit dem FTP-Dienst verwendete u. User-Kennwort.|  
|**alt_snapshot_folder**|**nvarchar(255)**|Gibt den Speicherort des anderen Ordners für die Momentaufnahme an.|  
|**working_directory**|**nvarchar(255)**|Der Name des Arbeitsverzeichnisses, das zum Speichern von Daten-und Schema Dateien verwendet wird.|  
|**use_ftp**|**bit**|Gibt anstelle des normalen Protokolls FTP an, um Momentaufnahmen abzurufen. Bei **einem** Wert von 1 wird FTP verwendet.|  
|**dts_package_name**|**sysname**|Gibt den Namen des DTS-Pakets (Data Transformation Services) an.|  
|**dts_package_password**|**nvarchar (524)**|Gibt das Kennwort für das Paket an.|  
|**dts_package_location**|**int**|Der Speicherort des DTS-Pakets.|  
|**enabled_for_syncmgr**|**bit**|Gibt an, ob das Abonnement über die [!INCLUDE[msCoName](../../includes/msconame-md.md)] Synchronisierungsverwaltung synchronisiert werden kann.<br /><br /> **0** = Abonnement ist nicht beim Synchronisierungs-Manager registriert.<br /><br /> **1** = das Abonnement wird bei der Synchronisierungs Verwaltung registriert und kann synchronisiert werden, ohne zu starten [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] .|  
|**offload_agent**|**bit**|Gibt an, ob der Agent Remote aktiviert werden kann. Wenn der Wert **0** ist, kann der Agent nicht remote aktiviert werden.|  
|**offload_server**|**sysname**|Gibt den Netzwerknamen des Servers an, der für die Remoteaktivierung verwendet wird.|  
|**dynamic_snapshot_location**|**nvarchar(255)**|Gibt den Pfad zum Ordner an, in dem die Momentaufnahmedateien gespeichert werden.|  
|**use_web_sync**|**bit**|Gibt an, ob das Abonnement über HTTP synchronisiert werden kann. Der Wert **1** bedeutet, dass diese Funktion aktiviert ist.|  
|**internet_url**|**nvarchar(260)**|Die URL, die den Speicherort der Replikationsüberwachung für die Websynchronisierung darstellt.|  
|**internet_login**|**sysname**|Die Anmeldung, die der Merge-Agent verwendet, wenn eine Verbindung mit dem Webserver hergestellt wird, der die Websynchronisierung mithilfe der- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Authentifizierung verwendet.|  
|**internet_password**|**nvarchar (524)**|Das Kennwort für die Anmeldung, die das Merge-Agent verwendet, wenn eine Verbindung mit dem Webserver hergestellt wird, der die Websynchronisierung mithilfe der- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Authentifizierung verwendet.|  
|**internet_security_mode**|**int**|Der Authentifizierungsmodus, der beim Herstellen einer Verbindung mit dem Webserver verwendet wird, der die Websynchronisierung verwendet. der Wert **1** steht für die Windows-Authentifizierung und der Wert **0** für die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Authentifizierung.|  
|**internet_timeout**|**int**|Die Zeit in Sekunden, nach der eine Websynchronisierungsanforderung abläuft.|  
|**hostname**|**sysname**|Gibt den Wert für **HOST_NAME** an, wenn diese Funktion in der **Where** -Klausel eines Joinfilters oder einer logischen Daten Satz Beziehung verwendet wird.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Replikations Tabellen &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Replikations Sichten &#40;Transact-SQL-&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sp_helppullsubscription &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-helppullsubscription-transact-sql.md)   
 [sp_helpsubscription &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-helpsubscription-transact-sql.md)   
 [sp_helpsubscription_properties &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpsubscription-properties-transact-sql.md)  
  
  
