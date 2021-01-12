---
description: sysmergepartitioninfo (Transact-SQL)
title: sysmergepartitioninfo (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- sysmergepartitioninfo_TSQL
- sysmergepartitioninfo
dev_langs:
- TSQL
helpviewer_keywords:
- sysmergepartitioninfo system table
ms.assetid: 7429ad2c-dd33-4f7d-89cc-700e083af518
author: cawrites
ms.author: chadam
ms.openlocfilehash: 8a94f14212f2603639c1d272da4d35e9bea9fb29
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100494"
---
# <a name="sysmergepartitioninfo-transact-sql"></a>sysmergepartitioninfo (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Stellt Informationen zu Partitionen für jeden Artikel bereit. Enthält eine Zeile für jeden in der lokalen Datenbank definierten Mergeartikel. Diese Tabelle wird in der Veröffentlichungs- und in der Abonnementdatenbank gespeichert.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**artid**|**uniqueidentifier**|Die eindeutige ID des angegebenen Artikels.|  
|**pubid**|**uniqueidentifier**|Die eindeutige ID für diese Veröffentlichung. Sie wird generiert, wenn die Veröffentlichung hinzugefügt wird.|  
|**partition_view_id**|**int**|Die ID der Partitionssicht für diese Tabelle. In dieser Sicht wird eine Zuordnung jeder Zeile im Artikel zu den verschiedenen zugehörigen Partitions-IDs angezeigt.|  
|**repl_view_id**|**int**|Muss hinzugefügt werden.|  
|**partition_deleted_view_rule**|**nvarchar(4000)**|Die SQL-Anweisung, mit der in einem Mergereplikationstrigger die Partitions-ID für jede gelöschte oder aktualisierte Zeile basierend auf den alten Spaltenwerten abgerufen wird.|  
|**partition_inserted_view_rule**|**nvarchar(4000)**|Die SQL-Anweisung, mit der in einem Mergereplikationstrigger die Partitions-ID für jede eingefügte oder aktualisierte Zeile basierend auf den neuen Spaltenwerten abgerufen wird.|  
|**membership_eval_proc_name**|**sysname**|Der Name der Prozedur, die die aktuellen Partitions-IDs von Zeilen in **MSmerge_contents** auswertet.|  
|**column_list**|**nvarchar(4000)**|Die durch Trennzeichen getrennte Liste mit in einem Artikel replizierten Spalten.|  
|**column_list_blob**|**nvarchar(4000)**|Die durch Trennzeichen getrennte Liste mit in einem Artikel replizierten Spalten, einschließlich BLOB-Spalten (Binary Large Object).|  
|**expand_proc**|**sysname**|Der Name der Prozedur, mit der Partitions-IDs für alle untergeordneten Zeilen einer neu eingefügten übergeordneten Zeile sowie für übergeordnete Zeilen, die einer Partitionsänderung unterzogen oder gelöscht wurden, neu ausgewertet werden.|  
|**logical_record_parent_nickname**|**int**|Der Spitzname des übergeordneten Elements der obersten Ebene eines Artikels in einem logischen Datensatz.|  
|**logical_record_view**|**int**|Eine Sicht, die den rowguid-Wert des übergeordneten Artikels der obersten Ebene ausgibt, der jedem untergeordneten rowguid-Wert entspricht.|  
|**logical_record_deleted_view_rule**|**nvarchar(4000)**|Vergleichbar mit **logical_record_view**, mit dem Unterschied, dass untergeordnete Zeilen in der Tabelle "Deleted" in Update-und DELETE-Triggern angezeigt werden.|  
|**logical_record_level_conflict_detection**|**bit**|Gibt an, ob Konflikte auf der logischen Datensatzebene oder auf der Zeilen- oder Spaltenebene erkannt werden sollen.<br /><br /> **0** = Konflikterkennung auf Zeilen-oder Spaltenebene wird verwendet.<br /><br /> **1** = die Konflikterkennung logischer Datensätze wird verwendet, wobei eine Änderung in einer Zeile auf dem Verleger und eine Änderung in einer separaten Zeile denselben logischen Datensatz auf dem Abonnenten als Konflikt behandelt.<br /><br /> Wenn dieser Wert **1** ist, kann nur die Konflikt Auflösung auf der logischen Datensatzebene verwendet werden.|  
|**logical_record_level_conflict_resolution**|**bit**|Gibt an, ob Konflikte auf der logischen Datensatzebene oder auf der Zeilen- oder Spaltenebene aufgelöst werden sollen.<br /><br /> **0** = Auflösung auf Zeilen-oder Spaltenebene wird verwendet.<br /><br /> **1** = bei einem Konflikt überschreibt der gesamte logische Datensatz aus dem Gewinner den gesamten logischen Datensatz auf der Verliererseite.<br /><br /> Der Wert **1** kann sowohl mit der Erkennung auf der logischen Datensatzebene als auch mit der Erkennung auf Zeilen-oder Spaltenebene verwendet werden.|  
|**partition_options**|**tinyint**|Definiert die Art und Weise, wie Daten im Artikel partitioniert werden. Dies ermöglicht Leistungsoptimierungen, wenn alle Zeilen nur zu einer einzigen Partition oder zu einem einzigen Abonnement gehören. *partition_options* kann einen der folgenden Werte aufweisen.<br /><br /> **0** = das Filtern für den Artikel ist entweder statisch oder ergibt keine eindeutige Teilmenge von Daten für jede Partition, d. h. eine "überlappende" Partition.<br /><br /> **1** = die Partitionen überlappen sich, und auf dem Abonnenten vorgenommene DML-Updates können nicht die Partition ändern, zu der eine Zeile gehört.<br /><br /> **2** = das Filtern für den Artikel ergibt nicht überlappende Partitionen, mehrere Abonnenten können jedoch die gleiche Partition erhalten.<br /><br /> **3** = das Filtern für den Artikel ergibt nicht überlappende Partitionen, die für jedes Abonnement eindeutig sind.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Replikations Tabellen &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Replikationssichten &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
