---
description: IHcolumns (Transact-SQL)
title: IHcolumns (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- IHcolumns_TSQL
- IHcolumns
dev_langs:
- TSQL
helpviewer_keywords:
- IHcolumns system table
ms.assetid: 5bb027e5-5279-487b-9c33-5f402987253c
author: cawrites
ms.author: chadam
ms.openlocfilehash: e9d264dd2c3199527a763f5c2c8afce01dad17e3
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094823"
---
# <a name="ihcolumns-transact-sql"></a>IHcolumns (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Die **IHcolumns** -Systemtabelle enthält eine Zeile für jede veröffentlichte Spalte. Mit dieser Tabelle wird definiert, wie Spaltendatentypen von einem Nicht-SQL Server-Verleger beim Veröffentlichen dargestellt werden. Im Prinzip werden Datentypen zwischen einem Nicht-SQL Server-Datenbank-Managementsystem (DBMS) und SQL Server zugeordnet. Diese Tabelle wird in der Verteilungsdatenbank gespeichert.  
  
## <a name="definition"></a>Definition  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**column_id**|**int**|Identifiziert eine veröffentlichte Spalte.|  
|**publishercolumn_id**|**int**|Ordnet eine veröffentlichte Spalte Spalten Metadaten zu, die in der [IHpublishercolumns](../../relational-databases/system-tables/ihpublishercolumns-transact-sql.md) -Systemtabelle gespeichert sind.|  
|**name**|**sysname**|Gibt den Spaltennamen an.|  
|**article_id**|**int**|Identifiziert den Artikel, zu dem die Spalte gehört.|  
|**column_ordinal**|**int**|Identifiziert die Spalte nach Reihenfolge.|  
|**mapped_type**|**tinyint**|Der Spaltendatentyp der Zielspalte auf dem Abonnenten.|  
|**mapped_length**|**bigint**|Die Länge der Spalte auf dem Abonnenten.|  
|**mapped_prec**|**int**|Die Genauigkeit der Spalte auf dem Abonnenten.|  
|**mapped_scale**|**int**|Die Dezimalstellen der Spalte auf dem Abonnenten.|  
|**mapped_nullable**|**bit**|Gibt an, ob die Spalte auf dem Abonnenten NULL-Werte akzeptiert, wobei **1** bedeutet, dass NULL-Werte akzeptiert werden.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Heterogene Datenbankreplikation](../../relational-databases/replication/non-sql/heterogeneous-database-replication.md)   
 [Replikations Tabellen &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Replikations Sichten &#40;Transact-SQL-&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sp_articlecolumn &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md)   
 [sysarticlecolumns &#40;System Ansicht&#41; &#40;Transact-SQL-&#41;](../../relational-databases/system-views/sysarticlecolumns-system-view-transact-sql.md)   
 [sysarticlecolumns &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/sysarticlecolumns-transact-sql.md)  
  
  
