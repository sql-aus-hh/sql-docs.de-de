---
description: Daten Bank Spiegelung-sys.dm_db_mirroring_connections
title: sys.dm_db_mirroring_connections (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_db_mirroring_connections
- dm_db_mirroring_connections
- sys.dm_db_mirroring_connections_TSQL
- dm_db_mirroring_connections_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_mirroring_connections dynamic management view
ms.assetid: e4df91b6-0240-45d0-ae22-cb2c0d52e0b3
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: e0cd194ef04063bcd1500d4c3be59c352e905114
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101705"
---
# <a name="database-mirroring---sysdm_db_mirroring_connections"></a>Daten Bank Spiegelung-sys.dm_db_mirroring_connections
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Gibt für jede für die Datenbankspiegelung hergestellte Verbindung eine Zeile zurück.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**connection_id**|**uniqueidentifier**|Bezeichner der Verbindung.|  
|**transport_stream_id**|**uniqueidentifier**|Der Bezeichner der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Netzwerkschnittstellen Verbindung (SNI), die von dieser Verbindung für die TCP/IP-Kommunikation verwendet wird.|  
|**state**|**smallint**|Aktueller Verbindungsstatus. Mögliche Werte:<br /><br /> 1 = NEW<br /><br /> 2 = CONNECTING<br /><br /> 3 = CONNECTED<br /><br /> 4 = LOGGED_IN<br /><br /> 5 = geschlossen|  
|**state_desc**|**nvarchar(60)**|Aktueller Verbindungsstatus. Mögliche Werte:<br /><br /> NEW<br /><br /> CONNECTING<br /><br /> CONNECTED<br /><br /> LOGGED_IN<br /><br /> CLOSED|  
|**connect_time**|**datetime**|Datum und Uhrzeit der Verbindungseröffnung.|  
|**login_time**|**datetime**|Datum und Uhrzeit der erfolgreichen Verbindungsanmeldung.|  
|**authentication_method**|**nvarchar(128)**|Name der Windows-Authentifizierungsmethode, z. B. NTLM oder KERBEROS. Dieser Wert stammt von Windows.|  
|**principal_name**|**nvarchar(128)**|Anmeldename, der auf Verbindungsberechtigungen überprüft wurde. Bei der Windows-Authentifizierung handelt es sich bei diesem Wert um den Remotebenutzernamen. Bei der Zertifikatsauthentifizierung handelt es sich bei diesem Wert um den Besitzer des Zertifikats.|  
|**remote_user_name**|**nvarchar(128)**|Name des Peer-Benutzers aus der anderen Datenbank, die von der Windows-Authentifizierung verwendet wird.|  
|**last_activity_time**|**datetime**|Datum und Uhrzeit für die letzte Verwendung der Verbindung zum Senden oder Empfangen von Informationen.|  
|**is_accept**|**bit**|Gibt an, ob die Verbindung ursprünglich von der Remoteseite stammt.<br /><br /> 1 = Die Verbindung ist eine Anforderung, die von der Remoteinstanz angenommen wurde.<br /><br /> 0 = Die Verbindung wurde von der lokalen Instanz gestartet.|  
|**login_state**|**smallint**|Status des Anmeldeprozesses für diese Verbindung. Mögliche Werte:<br /><br /> 0 = INITIAL<br /><br /> 1 = WAIT LOGIN NEGOTIATE<br /><br /> 2 = ONE ISC<br /><br /> 3 = ONE ASC<br /><br /> 4 = TWO ISC<br /><br /> 5 = TWO ASC<br /><br /> 6 = WAIT ISC Confirm<br /><br /> 7 = WAIT ASC Confirm<br /><br /> 8 = WAIT REJECT<br /><br /> 9 = WAIT PRE-MASTER SECRET<br /><br /> 10 = WAIT VALIDATION<br /><br /> 11 = WAIT ARBITRATION<br /><br /> 12 = online<br /><br /> 13 = ERROR|  
|**login_state_desc**|**nvarchar(60)**|Aktueller Anmeldestatus des Remotecomputers. Mögliche Werte:<br /><br /> Verbindungshandshake wird initialisiert.<br /><br /> Verbindungshandshake wartet auf Anmeldungsaushandlungs-Meldung.<br /><br /> Verbindungshandshake hat Sicherheitskontext zur Authentifizierung initialisiert und gesendet.<br /><br /> Verbindungshandshake hat Sicherheitskontext zur Authentifizierung empfangen und akzeptiert.<br /><br /> Verbindungshandshake hat Sicherheitskontext zur Authentifizierung initialisiert und gesendet. Ein optionaler Mechanismus ist für das Authentifizieren der Peers verfügbar.<br /><br /> Verbindungshandshake hat Sicherheitskontext zur Authentifizierung empfangen und gesendet. Ein optionaler Mechanismus ist für das Authentifizieren der Peers verfügbar.<br /><br /> Verbindungshandshake wartet auf Meldung zur Bestätigung der Sicherheitskontextinitialisierung.<br /><br /> Verbindungshandshake wartet auf Meldung zur Bestätigung der Sicherheitskontextannahme.<br /><br /> Verbindungshandshake wartet auf SSPI-Ablehnungsmeldung zur fehlgeschlagenen Authentifizierung.<br /><br /> Verbindungshandshake wartet auf Meldung für Vorstufe des geheimen Hauptschlüssels.<br /><br /> Verbindungshandshake wartet auf Überprüfungsmeldung.<br /><br /> Verbindungshandshake wartet auf Vermittlungsmeldung.<br /><br /> Verbindungshandshake wurde abgeschlossen und ist online (bereit) für Nachrichtenaustausch.<br /><br /> Verbindungsfehler.|  
|**peer_certificate_id**|**int**|Die lokale Objekt-ID des Zertifikats, das von der Remote Instanz für die Authentifizierung verwendet wird. Der Besitzer dieses Zertifikats muss über CONNECT-Berechtigungen für den Endpunkt der Datenbankspiegelung verfügen.|  
|**encryption_algorithm**|**smallint**|Der für diese Verbindung verwendete Verschlüsselungsalgorithmus. Lässt NULL-Werte zu. Mögliche Werte:<br /><br /> **Wert:** 0<br /><br /> **Beschreibung:** Gar<br /><br /> **DDL-Option:** Deaktiviert<br /><br /> **Wert:** 1<br /><br /> **Beschreibung:** RC4<br /><br /> **DDL-Option:** {Required &#124; erforderlichen Algorithmus RC4}<br /><br /> **Wert:** 2<br /><br /> **Beschreibung:** Kak<br /><br /> **DDL-Option:** Erforderlicher Algorithmus AES<br /><br /> **Wert:** 3<br /><br /> **Beschreibung:** Keine, RC4<br /><br /> **DDL-Option:** {supported &#124; unterstützter Algorithmus RC4}<br /><br /> **Wert:** 4<br /><br /> **Beschreibung:** None, AES<br /><br /> **DDL-Option:** Unterstützter Algorithmus RC4<br /><br /> **Wert:** 5<br /><br /> **Beschreibung:** RC4, AES<br /><br /> **DDL-Option:** Erforderlicher Algorithmus RC4 AES<br /><br /> **Wert:** 6<br /><br /> **Beschreibung:** AES, RC4<br /><br /> **DDL-Option:** Erforderlicher Algorithmus AES RC4<br /><br /> **Wert:** 7<br /><br /> **Beschreibung:** None, RC4, AES<br /><br /> **DDL-Option:** Unterstützter Algorithmus RC4 AES<br /><br /> **Wert:** 8<br /><br /> **Beschreibung:** None, AES, RC4<br /><br /> **DDL-Option:** Unterstützter Algorithmus AES RC4<br /><br /> **Hinweis:** Der RC4-Algorithmus wird nur aus Gründen der Abwärtskompatibilität unterstützt. Neues Material kann nur mit RC4 oder RC4_128 verschlüsselt werden, wenn die Datenbank den Kompatibilitätsgrad 90 oder 100 besitzt. (Nicht empfohlen.) Verwenden Sie stattdessen einen neueren Algorithmus, z. B. einen der AES-Algorithmen. In [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und höheren Versionen können mithilfe von RC4 oder RC4_128 verschlüsselte Materialien in jedem Kompatibilitäts Grad entschlüsselt werden.|  
|**encryption_algorithm_desc**|**nvarchar(60)**|Textdarstellung des Verschlüsselungsalgorithmus. Lässt NULL-Werte zu. Mögliche Werte:<br /><br /> **Beschreibung:** Gar<br /><br /> **DDL-Option:** Deaktiviert<br /><br /> **Beschreibung:** RC4<br /><br /> **DDL-Option:** {Required &#124; erforderlichen Algorithmus RC4}<br /><br /> **Beschreibung:** Kak<br /><br /> **DDL-Option:** Erforderlicher Algorithmus AES<br /><br /> **Beschreibung:** Keine, RC4<br /><br /> **DDL-Option:** {supported &#124; unterstützter Algorithmus RC4}<br /><br /> **Beschreibung:** keine, AES<br /><br /> **DDL-Option:** Unterstützter Algorithmus RC4<br /><br /> **Beschreibung:** RC4, AES<br /><br /> **DDL-Option:** Erforderlicher Algorithmus RC4 AES<br /><br /> **Beschreibung:** AES, RC4<br /><br /> **DDL-Option:** Erforderlicher Algorithmus AES RC4<br /><br /> **Beschreibung:** None, RC4, AES<br /><br /> **DDL-Option:** Unterstützter Algorithmus RC4 AES<br /><br /> **Beschreibung:** None, AES, RC4<br /><br /> **DDL-Option:** Unterstützter Algorithmus AES RC4|  
|**receives_posted**|**smallint**|Die Anzahl asynchroner Netzwerkempfangsvorgänge, die für diese Verbindung noch nicht abgeschlossen wurden.|  
|**is_receive_flow_controlled**|**bit**|Angabe, ob Netzwerkempfangsvorgänge aus Gründen der Datenflusskontrolle verschoben wurden, da das Netzwerk ausgelastet ist.<br /><br /> 1 = True|  
|**sends_posted**|**smallint**|Die Anzahl asynchroner Netzwerksendevorgänge, die für diese Verbindung noch nicht abgeschlossen wurden.|  
|**is_send_flow_controlled**|**bit**|Angabe, ob Netzwerksendevorgänge aus Gründen der Datenflusskontrolle verschoben wurden, da das Netzwerk ausgelastet ist.<br /><br /> 1 = True|  
|**total_bytes_sent**|**bigint**|Die Gesamtanzahl der von dieser Verbindung gesendeten Bytes.|  
|**total_bytes_received**|**bigint**|Die Gesamtanzahl der von dieser Verbindung empfangenen Bytes.|  
|**total_fragments_sent**|**bigint**|Die Gesamtanzahl der von dieser Verbindung gesendeten Datenspiegelungs-Nachrichtenfragmente.|  
|**total_fragments_received**|**bigint**|Die Gesamtanzahl der von dieser Verbindung empfangenen Datenspiegelungs-Nachrichtenfragmente.|  
|**total_sends**|**bigint**|Die Gesamtanzahl der von dieser Verbindung ausgegebenen Netzwerk Sende Anforderungen.|  
|**total_receives**|**bigint**|Die Gesamtanzahl der von dieser Verbindung ausgegebenen Netzwerkempfangsanforderungen.|  
|**peer_arbitration_id**|**uniqueidentifier**|Interner Bezeichner für den Endpunkt. Lässt NULL-Werte zu.|  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die VIEW SERVER STATE-Berechtigung auf dem Server.  
  
## <a name="physical-joins"></a>Physische Joins  
 ![Join für sys.join_dm_db_mirroring_connections](../../relational-databases/system-dynamic-management-views/media/join-dm-db-mirroring-connections.gif "Join für sys.join_dm_db_mirroring_connections")  
  
## <a name="relationship-cardinalities"></a>Kardinalität der Beziehungen  
  
|Von|Beschreibung|Relationship|  
|----------|--------|------------------|  
|**dm_db_mirroring_connections.connection_id**|**dm_exec_connections.connection_id**|1:1|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Dynamische Verwaltungssichten und -funktionen &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Überwachen der Datenbankspiegelung &#40;SQL Server&#41;](../../database-engine/database-mirroring/monitoring-database-mirroring-sql-server.md)  
  
  
