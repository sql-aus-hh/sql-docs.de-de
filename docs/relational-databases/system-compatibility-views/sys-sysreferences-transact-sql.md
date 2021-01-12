---
description: sys.sysreferences (Transact-SQL)
title: sys.sysVerweise (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.sysreferences
- sys.sysreferences_TSQL
- sysreferences
- sysreferences_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sysreferences compatibility view
- sysreferences system table
ms.assetid: 81276f13-202e-4e74-962d-46eb98c98d2e
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 16d1cdff10581925afd703ddd5b76260964dd225
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095381"
---
# <a name="syssysreferences-transact-sql"></a>sys.sysreferences (Transact-SQL)
[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

  Enthält Zuordnungen von FOREIGN KEY-Einschränkungsdefinitionen zu den Spalten, auf die in der Datenbank verwiesen wird.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**constid**|**int**|ID der FOREIGN KEY-Einschränkung.|  
|**fkeyid**|**int**|ID der verweisenden Tabelle|  
|**rkeyid**|**int**|ID der Tabelle, auf die verwiesen wird|  
|**rkeyindid**|**smallint**|Die Index-ID des eindeutigen Indexes in der Tabelle, auf die verwiesen wird, der die Schlüsselspalten abdeckt, auf die verwiesen wird.|  
|**keycnt**|**smallint**|Anzahl von Spalten im Schlüssel|  
|**forkeys**|**varbinary(32)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**refkeys**|**varbinary(32)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**fkeydbid**|**smallint**|Reserviert.|  
|**rkeydbid**|**smallint**|Reserviert.|  
|**fkey1**|**smallint**|Spalten-ID der verweisenden Spalte|  
|**fkey2**|**smallint**|Spalten-ID der verweisenden Spalte|  
|**fkey3**|**smallint**|Spalten-ID der verweisenden Spalte|  
|**fkey4**|**smallint**|Spalten-ID der verweisenden Spalte|  
|**fkey5**|**smallint**|Spalten-ID der verweisenden Spalte|  
|**fkey6**|**smallint**|Spalten-ID der verweisenden Spalte|  
|**fkey7**|**smallint**|Spalten-ID der verweisenden Spalte|  
|**fkey8**|**smallint**|Spalten-ID der verweisenden Spalte|  
|**fkey9**|**smallint**|Spalten-ID der verweisenden Spalte|  
|**fkey10**|**smallint**|Spalten-ID der verweisenden Spalte|  
|**fkey11**|**smallint**|Spalten-ID der verweisenden Spalte|  
|**fkey12**|**smallint**|Spalten-ID der verweisenden Spalte|  
|**fkey13**|**smallint**|Spalten-ID der verweisenden Spalte|  
|**fkey14**|**smallint**|Spalten-ID der verweisenden Spalte|  
|**fkey15**|**smallint**|Spalten-ID der verweisenden Spalte|  
|**fkey16**|**smallint**|Spalten-ID der verweisenden Spalte|  
|**rkey1**|**smallint**|Spalten-ID der Spalte, auf die verwiesen wird|  
|**rkey2**|**smallint**|Spalten-ID der Spalte, auf die verwiesen wird|  
|**rkey3**|**smallint**|Spalten-ID der Spalte, auf die verwiesen wird|  
|**rkey4**|**smallint**|Spalten-ID der Spalte, auf die verwiesen wird|  
|**rkey5**|**smallint**|Spalten-ID der Spalte, auf die verwiesen wird|  
|**rkey6**|**smallint**|Spalten-ID der Spalte, auf die verwiesen wird|  
|**rkey7**|**smallint**|Spalten-ID der Spalte, auf die verwiesen wird|  
|**rkey8**|**smallint**|Spalten-ID der Spalte, auf die verwiesen wird|  
|**rkey9**|**smallint**|Spalten-ID der Spalte, auf die verwiesen wird|  
|**rkey10**|**smallint**|Spalten-ID der Spalte, auf die verwiesen wird|  
|**rkey11**|**smallint**|Spalten-ID der Spalte, auf die verwiesen wird|  
|**rkey12**|**smallint**|Spalten-ID der Spalte, auf die verwiesen wird|  
|**rkey13**|**smallint**|Spalten-ID der Spalte, auf die verwiesen wird|  
|**rkey14**|**smallint**|Spalten-ID der Spalte, auf die verwiesen wird|  
|**rkey15**|**smallint**|Spalten-ID der Spalte, auf die verwiesen wird|  
|**rkey16**|**smallint**|Spalten-ID der Spalte, auf die verwiesen wird|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Zuordnung von Systemtabellen zu System Sichten &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [Kompatibilitätssichten &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  
