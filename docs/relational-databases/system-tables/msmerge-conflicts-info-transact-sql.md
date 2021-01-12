---
description: MSmerge_conflicts_info (Transact-SQL)
title: MSmerge_conflicts_info (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSmerge_conflicts_info_TSQL
- MSmerge_conflicts_info
dev_langs:
- TSQL
helpviewer_keywords:
- MSmerge_conflicts_info system table
ms.assetid: 6b76ae96-737a-4000-a6b6-fcc8772c2af4
author: cawrites
ms.author: chadam
ms.openlocfilehash: da7de7b1c8c18a74fc5a255027507853d195813b
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091477"
---
# <a name="msmerge_conflicts_info-transact-sql"></a>MSmerge_conflicts_info (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  In der **MSmerge_conflicts_info** Tabelle werden Konflikte nachverfolgt, die beim Synchronisieren eines Abonnements mit einer Mergeveröffentlichung auftreten. Die Verlust von Zeilendaten für Konflikte wird in der [MSmerge_conflict_publication_article](../../relational-databases/system-tables/msmerge-conflict-publication-article-transact-sql.md) Tabelle für den Artikel gespeichert, in dem der Konflikt aufgetreten ist. Diese Tabelle wird auf dem Verleger in der Veröffentlichungsdatenbank und auf dem Abonnenten in der Abonnementdatenbank gespeichert.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**tablenick**|**int**|Der Spitzname der veröffentlichten Tabelle.|  
|**ROWGUID**|**uniqueidentifier**|Der Bezeichner für die Konfliktzeile.|  
|**origin_datasource**|**nvarchar(255)**|Der Name der Datenbank, aus der die konfliktverursachende Änderung stammt.|  
|**conflict_type**|**int**|Der Typ des Konflikts, der aufgetreten ist. Die folgenden Werte sind möglich.<br /><br /> **1** = Aktualisierungs Konflikt: der Konflikt wurde auf Zeilenebene erkannt.<br /><br /> **2** = Konflikt bei der Spalten Aktualisierung: der Konflikt wurde auf Spaltenebene erkannt.<br /><br /> **3** = Update-DELETE-Konflikt: der Löschvorgang gewinnt den Konflikt.<br /><br /> **4** = Update-DELETE-Konflikt: die gelöschte ROWGUID, die den Konflikt verliert, wird in dieser Tabelle aufgezeichnet.<br /><br /> **5** = Fehler beim Hochladen der Einfügung: der Einfügevorgang des Abonnenten konnte nicht auf dem Verleger angewendet werden.<br /><br /> **6** = Fehler beim Herunterladen der Einfügung: die Einfügung vom Verleger konnte nicht auf dem Abonnenten angewendet werden.<br /><br /> **7** = Fehler beim Hochladen der Löschung: der Löschvorgang auf dem Abonnenten konnte nicht auf den Verleger hochgeladen werden.<br /><br /> **8** = Fehler beim Herunterladen der Löschung: der Löschvorgang für den Verleger konnte nicht auf den Abonnenten heruntergeladen werden.<br /><br /> **9** = Fehler beim Hochladen des Updates: das Update auf dem Abonnenten konnte nicht auf dem Verleger angewendet werden.<br /><br /> **10** = Fehler beim Herunterladen des Updates: das Update auf dem Verleger konnte nicht auf den Abonnenten angewendet werden.<br /><br /> **11** = Auflösung<br /><br /> **12** = logischer Daten Satz Update WINS löschen: der gelöschte logische Datensatz, der den Konflikt verliert, wird in dieser Tabelle aufgezeichnet.<br /><br /> **13** = logischer Daten Satz Konflikt Insert-Update: das Einfügen in einen logischen Datensatz steht in Konflikt mit einem Update.<br /><br /> **14** = logischer Datensatz: Delete WINS-Update Konflikt: der aktualisierte logische Datensatz, der den Konflikt verliert, wird in dieser Tabelle aufgezeichnet.|  
|**reason_code**|**int**|Der Fehlercode, der kontextbezogen sein kann. Im Fall von Konflikten mit Update-Update und Update-Delete ist der Wert, der für diese Spalte verwendet wird, identisch mit dem **conflict_type**. Bei Konflikten, bei denen Fehler beim Ändern aufgetreten sind, wird als Ursachencode der bei der Änderung im Merge-Agent aufgetretene Fehler verwendet. Wenn die Merge-Agent beispielsweise aufgrund einer Primärschlüssel Verletzung keine Einfügung auf dem Abonnenten anwenden kann, protokolliert Sie die **conflict_type** 6 ("Fehler beim herunterladen herunterladen") und eine **reason_code** von 2627, bei der es sich [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] um die interne Fehlermeldung bei einer Primärschlüssel Verletzung handelt: "Verletzung der% ls-Einschränkung '%. * ls '. Ein doppelter Schlüssel kann nicht in das Objekt '% ' eingefügt werden. \* ls "."|  
|**reason_text**|**nvarchar (720)**|Die Fehlerbeschreibung, die kontextbezogen sein kann.|  
|**pubid**|**uniqueidentifier**|Der Bezeichner für die Veröffentlichung.|  
|**MSrepl_create_time**|**datetime**|Die Uhrzeit, zu der der Konflikt aufgetreten ist.|  
|**origin_datasource_id**|**uniqueidentifier**|Der Bezeichner der Datenbank, aus der die konfliktverursachende Änderung stammt.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Replikations Tabellen &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Replikationssichten &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
