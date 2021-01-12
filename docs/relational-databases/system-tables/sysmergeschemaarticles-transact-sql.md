---
description: sysmergeschemaarticles (Transact-SQL)
title: sysmergeschemaarticles (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- sysmergeschemaarticles_TSQL
- sysmergeschemaarticles
dev_langs:
- TSQL
helpviewer_keywords:
- sysmergeschemaarticles system table
ms.assetid: b5085979-2f76-48e1-bf3b-765a84003dd9
author: cawrites
ms.author: chadam
ms.openlocfilehash: 98dc783b78d26d040789cc3dc46f2f53aaa4dc7c
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097382"
---
# <a name="sysmergeschemaarticles-transact-sql"></a>sysmergeschemaarticles (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Verfolgt Artikel vom Typ schema only für die Mergereplikation nach. Diese Tabelle wird in der Veröffentlichungs- und in der Abonnementdatenbank gespeichert.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**name**|**sysname**|Der Name des Artikels vom Typ schema only in der Mergeveröffentlichung.|  
|**type**|**tinyint**|Zeigt den Typ des Artikels vom Typ schema only an, der einen der folgenden Werte annehmen kann:<br /><br /> **0x20** = Schema Only-Artikel für gespeicherte Prozeduren.<br /><br /> **0x40** = nur Schema-oder indizierte Sicht Artikel anzeigen.|  
|**objid**|**int**|Der Objektbezeichner des Basisobjekts des Artikels. Kann der Objektbezeichner einer Prozedur, einer Sicht, einer indizierten Sicht oder einer benutzerdefinierten Funktion sein.|  
|**artid**|**uniqueidentifier**|Die Artikel-ID.|  
|**description**|**nvarchar(255)**|Die Beschreibung des Artikels.|  
|**pre_creation_command**|**tinyint**|Standardaktion, die ausgeführt wird, wenn der Artikel in der Abonnementdatenbank erstellt wird:<br /><br /> **0 =** Keine: Wenn die Tabelle bereits auf dem Abonnenten vorhanden ist, wird keine Aktion ausgeführt.<br /><br /> **1** = Drop-löscht die Tabelle, bevor Sie neu erstellt wird.<br /><br /> **2** = DELETE: gibt einen Löschvorgang basierend auf der WHERE-Klausel im Teilmengen Filter aus.<br /><br /> **3** = Abschneiden-identisch mit **2**, löscht jedoch Seiten anstelle von Zeilen. Eine WHERE-Klausel wird jedoch nicht verwendet.|  
|**pubid**|**uniqueidentifier**|Der eindeutige Bezeichner der Veröffentlichung.|  
|**status**|**tinyint**|Gibt den Status des Artikels vom Typ schema only an, der einen der folgenden Werte annehmen kann:<br /><br /> **1** = nicht synchronisiert-das Anfangs Verarbeitungs Skript zum Veröffentlichen der Tabelle wird ausgeführt, wenn die Momentaufnahmen-Agent das nächste Mal ausgeführt wird.<br /><br /> **2** = aktiv: das Anfangs Verarbeitungs Skript zum Veröffentlichen der Tabelle wurde ausgeführt.<br /><br /> **5** = New_inactive hinzuzufügen.<br /><br /> **6** = New_active hinzuzufügen.|  
|**creation_script**|**nvarchar(255)**|Der Pfad und Name eines optionalen Artikel-Schemavorabskripts, mit dem die Zieltabelle erstellt wird.|  
|**schema_option**|**Binär (8)**|Das Bitmuster der Option zur Schemaerstellung für den angegebenen Artikel vom Typ schema only, das das bitweise logische OR-Ergebnis von mindestens einer dieser Werte sein kann:<br /><br /> **0x00** = Skripterstellung durch die Momentaufnahmen-Agent deaktivieren und das bereitgestellte Skript für die Erstellung von Skripts verwenden.<br /><br /> **0x01** = generiert die Objekt Erstellung (CREATE TABLE, CREATE PROCEDURE usw.).<br /><br /> **0x10** = generiert einen entsprechenden gruppierten Index.<br /><br /> **0x20** = konvertieren Sie benutzerdefinierte Datentypen in Basis Datentypen.<br /><br /> **0x40** = generieren Sie den entsprechenden nicht gruppierten Index oder die Indizes.<br /><br /> **0x80** = deklarierte referenzielle Integrität für die Primärschlüssel einschließen.<br /><br /> **0x100** = repliziert Benutzer Trigger für einen Tabellen Artikel, falls definiert.<br /><br /> **0x200** = Replizieren von Foreign Key-Einschränkungen. Wenn die Tabelle, auf die verwiesen wird, nicht Teil einer Veröffentlichung ist, werden keine Fremdschlüsseleinschränkungen für eine veröffentlichte Tabelle repliziert.<br /><br /> **0x400** = replizieren Sie Check-Einschränkungen.<br /><br /> **0x800** = replizieren Sie die Standardwerte.<br /><br /> **0x1000** = replizieren Sie die Sortierung auf Spaltenebene.<br /><br /> **0x2000** = replizieren Sie erweiterte Eigenschaften, die dem veröffentlichten Artikel Quell Objekt zugeordnet sind.<br /><br /> **0x4000** = repliziert eindeutige Schlüssel, wenn Sie für einen Tabellen Artikel definiert sind.<br /><br /> **0X8000** = repliziert einen Primärschlüssel und eindeutige Schlüssel für einen Tabellen Artikel als Einschränkungen mithilfe von ALTER TABLE-Anweisungen.<br /><br /> Weitere Informationen zu möglichen Werten für **schema_option** finden Sie unter [sp_addmergearticle](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md).|  
|**destination_object**|**sysname**|Der Name des Zielobjekts in der Abonnementdatenbank. Dieser Wert gilt nur für Artikel vom Typ schema only, wie z. B. gespeicherte Prozeduren, Sichten und UDFs.|  
|**destination_owner**|**sysname**|Der Besitzer des Objekts in der Abonnement Datenbank, wenn es sich nicht um **dbo** handelt.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Replikations Tabellen &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Replikationssichten &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
