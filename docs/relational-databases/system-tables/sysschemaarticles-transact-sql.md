---
description: sysschemaarticles (Transact-SQL)
title: sysschemaarticles (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- sysschemaarticles_TSQL
- sysschemaarticles
dev_langs:
- TSQL
helpviewer_keywords:
- sysschemaarticles system table
ms.assetid: 67a1c039-c283-4a9c-bacc-b9b3973590c3
author: cawrites
ms.author: chadam
ms.openlocfilehash: ecefc7fe927f5013d5647c276cf4dc1e687c0ff0
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102702"
---
# <a name="sysschemaarticles-transact-sql"></a>sysschemaarticles (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Protokolliert Artikel vom Typ schema only für Momentaufnahme- und Transaktionsveröffentlichungen. Diese Tabelle wird in der Veröffentlichungsdatenbank gespeichert.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**artid**|**int**|Die Artikel-ID.|  
|**creation_script**|**nvarchar(255)**|Der Pfad und der Name eines Artikelschemaskripts, mit dem die Zieltabelle erstellt wird.|  
|**description**|**nvarchar(255)**|Der Beschreibungseintrag für den Artikel.|  
|**dest_object**|**sysname**|Der Name des Objekts in der Abonnementdatenbank, wenn der Artikel vom Typ schema only ist, wie z. B. eine gespeicherte Prozedur, eine Sicht oder eine UDF.|  
|**name**|**sysname**|Der Name des Artikels vom Typ schema only in einer Veröffentlichung.|  
|**objid**|**int**|Der Objektbezeichner des Basisobjekts des Artikels. Dies kann der Objektbezeichner einer Prozedur, einer Sicht, einer indizierten Sicht oder einer UDF sein.|  
|**pubid**|**int**|Die ID für die Veröffentlichung.|  
|**pre_creation_cmd**|**tinyint**|Gibt die vom System durchzuführenden Schritte an, wenn es beim Anwenden der Momentaufnahme für diesen Artikel ein vorhandenes Objekt mit demselben Namen beim Abonnenten erkennt:<br /><br /> **0** = nichts.<br /><br /> **1** = Ziel Tabelle löschen.<br /><br /> **2** = Ziel Tabelle löschen.<br /><br /> **3** = Abschneiden der Ziel Tabelle.|  
|**status**|**int**|Das Bitmuster, das zum Anzeigen des Artikelstatus verwendet wird.|  
|**type**|**tinyint**|Der Wert, der den Typ des schema only-Artikels anzeigt:<br /><br /> **32** = gespeicherte Prozedur.<br /><br /> **64** = Sicht oder indizierte Sicht. <br /><br /> **96** = Aggregatfunktion.<br /><br /> **128** = Funktion.|  
|**schema_option**|**Binär (8)**|Die Bitmaske der Option zur Schemagenerierung für den angegebenen Artikel. Sie gibt die automatische Erstellung der gespeicherten Prozedur in der Zieldatenbank für alle Anweisungen vom Typ CALL/MCALL/XCALL an. Hierbei kann es sich um das Ergebnis eines bitweisen logischen OR-Vorgangs von mindestens einem der folgenden Werte handeln:<br /><br /> **0x00** = deaktiviert die Skripterstellung durch den Momentaufnahmen-Agent und verwendet *creation_script*.<br /><br /> **0x01** = generiert die Objekt Erstellung (CREATE TABLE, CREATE PROCEDURE usw.). Dies ist der Standardwert für alle Artikel mit gespeicherten Prozeduren.<br /><br /> **0x02** = generiert benutzerdefinierte gespeicherte Prozeduren für den Artikel, falls definiert.<br /><br /> **0x10** = generiert einen entsprechenden gruppierten Index.<br /><br /> **0x20** = konvertiert benutzerdefinierte Datentypen in Basis Datentypen.<br /><br /> **0x40**= generiert entsprechende nicht gruppierte Indizes.<br /><br /> **0x80**= schließt die deklarierte referenzielle Integrität für die Primärschlüssel ein.<br /><br /> **0x73** = generiert die CREATE TABLE-Anweisung, erstellt gruppierte und nicht gruppierte Indizes, konvertiert benutzerdefinierte Datentypen in Basis Datentypen und generiert benutzerdefinierte Skripts für gespeicherte Prozeduren, die auf dem Abonnenten angewendet werden. Dies ist der Standardwert für alle Artikel außer für Artikel mit gespeicherten Prozeduren.<br /><br /> **0x100**= repliziert Benutzer Trigger für einen Tabellen Artikel, falls definiert.<br /><br /> **0x200**= repliziert Foreign Key-Einschränkungen. Wenn die Tabelle, auf die verwiesen wird, nicht Teil einer Veröffentlichung ist, werden alle FOREIGN KEY-Einschränkungen für eine veröffentlichte Tabelle nicht repliziert.<br /><br /> **0x400**= repliziert Check-Einschränkungen.<br /><br /> **0x800**= repliziert Standardwerte.<br /><br /> **0x1000**= repliziert die Sortierung auf Spaltenebene.<br /><br /> **0x2000**= repliziert erweiterte Eigenschaften, die dem veröffentlichten Artikel Quell Objekt zugeordnet sind.<br /><br /> **0x4000**= repliziert eindeutige Schlüssel, wenn Sie für einen Tabellen Artikel definiert sind.<br /><br /> **0X8000**= repliziert Primärschlüssel und eindeutige Schlüssel eines Tabellen Artikels als Einschränkungen mithilfe von ALTER TABLE-Anweisungen.|  
|**dest_owner**|**sysname**|Der Besitzer der Tabelle in der Zieldatenbank|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Replikations Tabellen &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Replikationssichten &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
