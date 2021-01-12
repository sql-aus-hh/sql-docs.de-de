---
description: IHarticles (Transact-SQL)
title: IHarticles (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- IHarticles
- IHarticles_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- IHarticles system table
ms.assetid: 773ef9b7-c993-4629-9516-70c47b9dcf65
author: cawrites
ms.author: chadam
ms.openlocfilehash: 00e32678fd1df185607f3e02c5bf1f21780c5d79
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091520"
---
# <a name="iharticles-transact-sql"></a>IHarticles (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Die **IHarticles** -Systemtabelle enthält eine Zeile für jeden Artikel, der von einem nicht-SQL Server Verleger mithilfe des aktuellen Verteilers repliziert wird. Diese Tabelle wird in der Verteilungsdatenbank gespeichert.  
  
## <a name="definition"></a>Definition  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**article_id**|**int**|Die Identitätsspalte, die eine eindeutige ID für den Artikel bereitstellt.|  
|**name**|**sysname**|Der mit dem Artikel verknüpfte Name, der innerhalb der Veröffentlichung eindeutig ist.|  
|**publication_id**|**smallint**|Die ID der Veröffentlichung, zu der der Artikel gehört.|  
|**table_id**|**int**|Die ID der Tabelle, die aus [IHpublishertables](../../relational-databases/system-tables/ihpublishertables-transact-sql.md)veröffentlicht wird.|  
|**publisher_id**|**smallint**|Die ID des Nicht-SQL Server-Verlegers.|  
|**creation_script**|**nvarchar(255)**|Das Schemaskript für den Artikel.|  
|**del_cmd**|**nvarchar(255)**|Der Replikationsbefehlstyp, der zur Replikation von Löschungen bei Tabellenartikeln verwendet wird. Weitere Informationen finden Sie unter [Angeben der Weitergabemethode für Änderungen bei Transaktionsartikeln](../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md).|  
|**filter**|**int**|Diese Spalte wird nicht verwendet und ist nur enthalten, um die [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) -Sicht der **IHarticles** -Tabelle mit der [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) -Sicht, die für SQL Server Artikel ([sysarticles](../../relational-databases/system-tables/sysarticles-transact-sql.md)) verwendet wird, kompatibel zu machen.|  
|**filter_clause**|**ntext**|Die WHERE-Klausel des Artikels, die zum horizontalen Filtern verwendet wird und in einem standardmäßigen Transact-SQL-Code geschrieben ist, der von anderen als SQL Server-Verlegern interpretiert werden kann.|  
|**ins_cmd**|**nvarchar(255)**|Der Replikationsbefehlstyp, der zur Replikation von Einfügungen bei Tabellenartikeln verwendet wird. Weitere Informationen finden Sie unter [Angeben der Weitergabemethode für Änderungen bei Transaktionsartikeln](../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md).|  
|**pre_creation_cmd**|**tinyint**|Der Befehl, der vor dem Anwenden der Anfangsmomentaufnahme ausgeführt wird, wenn auf dem Abonnenten bereits ein Objekt mit dem gleichen Namen vorhanden ist.<br /><br /> **0** = None-ein Befehl wird nicht ausgeführt.<br /><br /> **1** = Drop-Drop der Ziel Tabelle.<br /><br /> **2** = DELETE-Daten aus der Ziel Tabelle löschen.<br /><br /> **3** = abschneiden-die Ziel Tabelle wird abgeschnitten.|  
|**status**|**tinyint**|Die Bitmaske der Artikeloptionen und der Status, die das Ergebnis des bitweisen logischen OR von mindestens einem der folgenden Werte sein können:<br /><br /> **0** = keine zusätzlichen Eigenschaften.<br /><br /> **1** = aktiv.<br /><br /> **8** = Den Spaltennamen in INSERT-Anweisungen einschließen.<br /><br /> **16** = Parametrisierte Anweisungen verwenden.<br /><br /> Ein aktiver Artikel, der parametrisierte Anweisungen verwendet, würde in dieser Spalte beispielsweise den Wert 17 anzeigen. Der Wert 0 gibt an, dass der Artikel inaktiv ist und keine zusätzlichen Eigenschaften definiert wurden.|  
|**type**|**tinyint**|Der Artikeltyp:<br /><br /> **1** = Protokollbasierter Artikel.|  
|**upd_cmd**|**nvarchar(255)**|Der Replikationsbefehlstyp, der zur Replikation von Updates bei Tabellenartikeln verwendet wird. Weitere Informationen finden Sie unter [Angeben der Weitergabemethode für Änderungen bei Transaktionsartikeln](../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md).|  
|**schema_option**|**Binär (8)**|Das Bitmuster der Schemagenerierungsoption für den angegebenen Artikel, die das Ergebnis des bitweisen logischen OR von mindestens einem der folgenden Werte sein kann:<br /><br /> **0x00** = Skripterstellung durch die Momentaufnahmen-Agent deaktivieren und das bereitgestellte Skript für die Erstellung von Skripts verwenden.<br /><br /> **0x01** = generiert die Objekt Erstellung (CREATE TABLE, CREATE PROCEDURE usw.).<br /><br /> **0x10** = generiert einen entsprechenden gruppierten Index.<br /><br /> **0x40** = generieren Sie entsprechende nicht gruppierte Indizes.<br /><br /> **0x80** = deklarierte referenzielle Integrität für die Primärschlüssel einschließen.<br /><br /> **0x1000** = repliziert die Sortierung auf Spaltenebene. Hinweis: diese Option ist standardmäßig für Oracle-Verleger festgelegt, um Vergleiche zwischen Groß-und Kleinschreibung zu aktivieren.<br /><br /> **0x4000** = repliziert eindeutige Schlüssel, wenn Sie für einen Tabellen Artikel definiert sind.<br /><br /> **0X8000** = repliziert einen Primärschlüssel und eindeutige Schlüssel für einen Tabellen Artikel als Einschränkungen mithilfe von ALTER TABLE-Anweisungen.|  
|**dest_owner**|**sysname**|Der Besitzer der Tabelle in der Zieldatenbank|  
|**dest_table**|**sysname**|Der Name der Zieltabelle|  
|**tablespace_name**|**nvarchar(255)**|Identifiziert den von der Protokollierungstabelle für den Artikel verwendeten Tabellenbereich.|  
|**objid**|**int**|Diese Spalte wird nicht verwendet und ist nur enthalten, um die [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) -Sicht der **IHarticles** -Tabelle mit der [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) -Sicht, die für SQL Server Artikel ([sysarticles](../../relational-databases/system-tables/sysarticles-transact-sql.md)) verwendet wird, kompatibel zu machen.|  
|**sync_objid**|**int**|Diese Spalte wird nicht verwendet und ist nur enthalten, um die [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) -Sicht der **IHarticles** -Tabelle mit der [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) -Sicht, die für SQL Server Artikel ([sysarticles](../../relational-databases/system-tables/sysarticles-transact-sql.md)) verwendet wird, kompatibel zu machen.|  
|**description**|**nvarchar(255)**|Der Beschreibungseintrag für den Artikel.|  
|**publisher_status**|**int**|Wird verwendet, um anzugeben, ob die Sicht, die den veröffentlichten Artikel definiert, durch Aufrufen von [sp_articleview](../../relational-databases/system-stored-procedures/sp-articleview-transact-sql.md)definiert wurde.<br /><br /> **0**  =  [sp_articleview](../../relational-databases/system-stored-procedures/sp-articleview-transact-sql.md) wurde aufgerufen.<br /><br /> **1**  =  [sp_articleview](../../relational-databases/system-stored-procedures/sp-articleview-transact-sql.md) wurde nicht aufgerufen.|  
|**article_view_owner**|**nvarchar(255)**|Der Besitzer des Synchronisierungsobjekts auf dem Verleger, das vom Protokolllese-Agent verwendet wird.|  
|**article_view**|**nvarchar(255)**|Das Synchronisierungsobjekts auf dem Verleger, das vom Protokolllese-Agent verwendet wird.|  
|**ins_scripting_proc**|**int**|Diese Spalte wird nicht verwendet und ist nur enthalten, um die [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) -Sicht der **IHarticles** -Tabelle mit der [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) -Sicht, die für SQL Server Artikel ([sysarticles](../../relational-databases/system-tables/sysarticles-transact-sql.md)) verwendet wird, kompatibel zu machen.|  
|**del_scripting_proc**|**int**|Diese Spalte wird nicht verwendet und ist nur enthalten, um die [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) -Sicht der **IHarticles** -Tabelle mit der [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) -Sicht, die für SQL Server Artikel ([sysarticles](../../relational-databases/system-tables/sysarticles-transact-sql.md)) verwendet wird, kompatibel zu machen.|  
|**upd_scripting_proc**|**int**|Diese Spalte wird nicht verwendet und ist nur enthalten, um die [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) -Sicht der **IHarticles** -Tabelle mit der [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) -Sicht, die für SQL Server Artikel ([sysarticles](../../relational-databases/system-tables/sysarticles-transact-sql.md)) verwendet wird, kompatibel zu machen.|  
|**custom_script**|**int**|Diese Spalte wird nicht verwendet und ist nur enthalten, um die [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) -Sicht der **IHarticles** -Tabelle mit der [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) -Sicht, die für SQL Server Artikel ([sysarticles](../../relational-databases/system-tables/sysarticles-transact-sql.md)) verwendet wird, kompatibel zu machen.|  
|**fire_triggers_on_snapshot**|**bit**|Diese Spalte wird nicht verwendet und ist nur enthalten, um die [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) -Sicht der **IHarticles** -Tabelle mit der [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) -Sicht, die für SQL Server Artikel ([sysarticles](../../relational-databases/system-tables/sysarticles-transact-sql.md)) verwendet wird, kompatibel zu machen.|  
|**instance_id**|**int**|Identifiziert die aktuelle Instanz des Artikelprotokolls für die veröffentlichte Tabelle.|  
|**use_default_datatypes**|**bit**|Gibt an, ob der Artikel standardmäßige Datentyp Zuordnungen verwendet. der Wert **1** gibt an, dass standardmäßige Datentyp Zuordnungen verwendet werden.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Heterogene Datenbankreplikation](../../relational-databases/replication/non-sql/heterogeneous-database-replication.md)   
 [Replikations Tabellen &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Replikations Sichten &#40;Transact-SQL-&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sp_addarticle &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md)   
 [sp_changearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changearticle-transact-sql.md)  
  
  
