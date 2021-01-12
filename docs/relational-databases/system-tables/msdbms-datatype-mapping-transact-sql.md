---
description: MSdbms_datatype_mapping (Transact-SQL)
title: MSdbms_datatype_mapping (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSdbms_datatype_mapping_TSQL
- MSdbms_datatype_mapping
dev_langs:
- TSQL
helpviewer_keywords:
- MSdbms_datatype_mapping system table
ms.assetid: 13289a0b-dfb0-4771-ad80-4c5f83cded99
author: cawrites
ms.author: chadam
ms.openlocfilehash: 308e11275807ff11e3700bfcb443238c8c6ffeef
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098665"
---
# <a name="msdbms_datatype_mapping-transact-sql"></a>MSdbms_datatype_mapping (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Die **MSdbms_datatype_mapping** Tabelle enthält die zulässigen Datentyp Zuordnungen des Datentyps im Quelldatenbank-Management System (DBMS) mit einem oder mehreren spezifischen Datentypen im Ziel-DBMS. Diese Tabelle wird in der **msdb** -Datenbank gespeichert und für die heterogene Daten Bank Replikation verwendet.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**datatype_mapping_id**|**int**|Identifiziert jede eindeutige Datentypzuordnung.|  
|**map_id**|**int**|Identifiziert den Quelldatentyp.|  
|**dest_datatype_id**|**int**|Identifiziert den Zieldatentyp.|  
|**dest_precision**|**bigint**|Definiert die Genauigkeit des Ziel Datentyps, wobei der Wert NULL bedeutet, dass die Genauigkeit nicht verwendet wird, und der Wert **-1** bedeutet, dass die Genauigkeit des Quell Datentyps verwendet wird.|  
|**dest_scale**|**int**|Definiert die Skala des Ziel Datentyps, wobei der Wert NULL bedeutet, dass die Skala nicht verwendet wird, und der Wert **-1** bedeutet, dass die Skala des Quell Datentyps verwendet wird.|  
|**dest_length**|**bigint**|Definiert die Länge des Ziel Datentyps, wobei der Wert NULL bedeutet, dass die Länge nicht verwendet wird, und der Wert **-1** bedeutet, dass die Länge des Quell Datentyps verwendet wird.|  
|**dest_nullable**|**bit**|Gibt an, ob die Zielspalte in der Zuordnung NULL-Werte zulässt. Ein Wert von NULL bedeutet, dass diese Definition nicht erforderlich ist.|  
|**dest_createparams**|**int**|Die Bitmap, die beschreibt, welche Kombination aus Länge, Genauigkeit und Dezimalstellen für den jeweiligen Datentyp anwendbar sind. Sie beinhaltet Folgendes:<br /><br /> **0x1** = Genauigkeit.<br /><br /> **0x2** = skalieren.<br /><br /> **0x4** = Länge.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Heterogene Datenbankreplikation](../../relational-databases/replication/non-sql/heterogeneous-database-replication.md)   
 [Angeben von Datentyp Zuordnungen für einen Oracle-Verleger](../../relational-databases/replication/publish/specify-data-type-mappings-for-an-oracle-publisher.md)   
 [Replikations Tabellen &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Replikationssichten &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
