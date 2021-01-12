---
description: sysmergeschemachange (Transact-SQL)
title: sysmergeschemachange (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- sysmergeschemachange_TSQL
- sysmergeschemachange
dev_langs:
- TSQL
helpviewer_keywords:
- sysmergeschemachange system table
ms.assetid: ae9ce16e-6ee9-4c7c-8210-a9bf2c7efdf0
author: cawrites
ms.author: chadam
ms.openlocfilehash: 2a0315c49710ac324234710d30c223dfb9b27b12
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102007"
---
# <a name="sysmergeschemachange-transact-sql"></a>sysmergeschemachange (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Enthält Informationen zu den vom Momentaufnahme-Agent generierten veröffentlichten Artikeln. Diese Tabelle wird in der Veröffentlichungs- und in der Abonnementdatenbank gespeichert.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**pubid**|**uniqueidentifier**|Die ID der Veröffentlichung.|  
|**artid**|**uniqueidentifier**|Die ID des Artikels.|  
|**Schema Version**|**int**|Die Nummer der letzten Schemaänderung|  
|**schemaguid**|**uniqueidentifier**|Die eindeutige ID des letzten Schemas|  
|**schematype**|**int**|Der Typ der Schemaänderung:<br /><br /> **-1** = ungültig.<br /><br /> **1** = SQL-Befehl.<br /><br /> **2** = Schema Skript.<br /><br /> **3** = System eigenes Hilfsprogramm zum Massen kopieren (BCP).<br /><br /> **4** = Zeichen bcp.<br /><br /> **5** = zuletzt erfasste Generierung.<br /><br /> **6** = zuletzt gesendete Generierung.<br /><br /> **7** = Verzeichnis.<br /><br /> **8** = Priorität.<br /><br /> **9** = Beibehaltungs Dauer.<br /><br /> **10** = auslöserskript.<br /><br /> **11** = ALTER TABLE.<br /><br /> **12** = alle erneut initialisieren.<br /><br /> **13** = ALTER TABLE (Non- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ).<br /><br /> **14** = mit Upload erneut initialisieren.<br /><br /> **15** = Einschränkungs-und Index Skript.<br /><br /> **16** = Metadatenbereinigung.<br /><br /> **17** = zuletzt gesendete Generierung aktualisieren.<br /><br /> **18** = abwärts Kompatibilitäts Grad.<br /><br /> **19** = Abonnenten Informationen überprüfen.<br /><br /> **20** = gut partitioniert.<br /><br /> **21** = benutzerdefinierter Konflikt Löser.<br /><br /> **22** = Artikel Verarbeitungsreihenfolge.<br /><br /> **23** = in Transaktions Veröffentlichung veröffentlicht.<br /><br /> **24** = Fehler kompensieren.<br /><br /> **25** = alternativer Momentaufnahme Ordner.<br /><br /> **26** = nur herunterladbar.<br /><br /> **27** = Lösch Nachverfolgung.<br /><br /> **40** = Momentaufnahme Skript vor der Erstellung.<br /><br /> **45** = Momentaufnahme Skript nach der Erstellung.<br /><br /> **46** = Bedarfs gesteuertes Benutzer Skript.<br /><br /> **50** = Momentaufnahme-Header Anfang.<br /><br /> **51** = Snapshot-Header Ende.<br /><br /> **52** = Snapshot-Nachspann.<br /><br /> **53** = Dateiübertragungsprotokoll (FTP)-Adresse.<br /><br /> **54** = FTP-Port.<br /><br /> **55** = FTP-Unterverzeichnis.<br /><br /> **56** = komprimierte Momentaufnahme.<br /><br /> **57** = FTP-Anmeldung.<br /><br /> **58** = FTP-Kennwort.<br /><br /> **60** = Skript für vorab Erstellung des Systems.<br /><br /> **61** = Schema der gespeicherten Prozedur.<br /><br /> **62** = Schema anzeigen.<br /><br /> **63** = benutzerdefiniertes Funktions Schema.<br /><br /> **64** = Indizes anzeigen.<br /><br /> **65** = erweiterte Eigenschaften.<br /><br /> **66** = Validate.<br /><br /> **67** = SQL-Befehl vor der Momentaufnahme.<br /><br /> **71** = dynamische Momentaufnahme Validierung.<br /><br /> **80** = System Tabelle, System eigenes BCP 9,0.<br /><br /> **81** = System Tabellen Zeichen BCP 9,0.<br /><br /> **82** = System Tabelle, System eigenes BCP 9,0 (nur global).<br /><br /> **83** = System Tabellen Zeichen BCP 9,0 (nur global).<br /><br /> **84** = System Tabelle, System eigenes BCP 9,0 (Lightweight).<br /><br /> **85** = System Tabellen Zeichen BCP 9,0 (Lightweight).<br /><br /> **128** = dynamisches bcp (Bit).<br /><br /> **131** = dynamisches natives bcp.<br /><br /> **132** = dynamisches Zeichen bcp.<br /><br /> **208** = dynamische Systemtabelle, System eigenes BCP 9,0.<br /><br /> **209** = dynamisches Systemtabellen Zeichen BCP 9,0.<br /><br /> **210** = dynamische Systemtabelle, System eigenes BCP 9,0 (nur global).<br /><br /> **211** = dynamisches Systemtabellen Zeichen BCP 9,0 (nur global).<br /><br /> **212** = dynamische Systemtabelle, System eigenes BCP 9,0 (Lightweight).<br /><br /> **213** = dynamisches Systemtabellen Zeichen BCP 9,0 (Lightweight).<br /><br /> **300** = DDL-Aktionen (Data Definition Language, Datendefinitionssprache).<br /><br /> **1024** = Inkrementelles Snapshot-Steuerelement<br /><br /> **1049** = inkrementeller Momentaufnahme Ordner.<br /><br /> **1074** = inkrementeller Snapshot-BEGIN-Header.<br /><br /> **1075** = inkrementeller Snapshot-endheader.<br /><br /> **1076** = inkrementeller Snapshot.<br /><br /> **1077** = inkrementelle FTP-Adresse.<br /><br /> **1078** = Inkrementeller FTP-Port.<br /><br /> **1079** = Inkrementelles FTP-Unterverzeichnis.<br /><br /> **1080** = inkrementelle komprimierte Momentaufnahme<br /><br /> **1081** = inkrementelle FTP-Anmeldung.<br /><br /> **1082** = Inkrementelles FTP-Kennwort|  
|**schematext**|**nvarchar (2000)**|Der Name der Skriptdatei oder eines Befehls, der einen Dateinamen einschließt.|  
|**schemastatus**|**tinyint**|Gibt ab, ob eine Schemaänderung für den Artikel aussteht, die einen der folgenden Werte haben kann:<br /><br /> **0** = inaktiv.<br /><br /> **1** = aktiv.<br /><br /> Wenn eine Schema Änderung aussteht, wird dieser Wert auf **1** festgelegt.|  
|**schemasubtype**|**int**|Der Untertyp der Schemaänderung:<br /><br /> **1** = AddColumn<br /><br /> **2** = DropColumn<br /><br /> **3** = AlterColumn<br /><br /> **4** = addprimarykey<br /><br /> **5** = AddUnique<br /><br /> **6** = adressenz<br /><br /> **7** = dropeinschränkung<br /><br /> **8** = adddefault<br /><br /> **9** = ADDCHECK<br /><br /> **10** = disableauslöst<br /><br /> **11** = enableauslöst<br /><br /> **12** = disableauslöst<br /><br /> **13** = enableauslöst<br /><br /> **14** = enableeinschränkung<br /><br /> **15** = disableeinschränkung<br /><br /> **16** = enableeinschränkung<br /><br /> **17** = disableeinschränkung|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Replikations Tabellen &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Replikationssichten &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
