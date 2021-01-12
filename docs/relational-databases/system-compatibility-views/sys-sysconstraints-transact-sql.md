---
description: sys.sysconstraints (Transact-SQL)
title: sys.sysEinschränkungen (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sysconstraints
- sys.sysconstraints
- sysconstraints_TSQL
- sys.sysconstraints_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sysconstraints compatibility view
- sysconstraints system table
ms.assetid: f2b2e2ad-ba24-48a1-913c-8ee4e0895dc4
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 3014430aad20c74d22a72e1756503baadbe7cb68
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099131"
---
# <a name="syssysconstraints-transact-sql"></a>sys.sysconstraints (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Enthält Zuordnungen von Einschränkungen zu den Objekten, die diese Einschränkungen in der Datenbank besitzen.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**constid**|**int**|Nummer der Einschränkung.|  
|**id**|**int**|ID der Tabelle, die die Einschränkung besitzt.|  
|**ColId**|**smallint**|ID der Spalte, für die die Einschränkung definiert ist.<br /><br /> 0 = Tabelleneinschränkung|  
|**spare1**|**tinyint**|Reserviert|  
|**status**|**int**|Pseudobitmaske zur Anzeige des Status. Folgende Werte sind möglich:<br /><br /> 1 = PRIMARY KEY-Einschränkung<br /><br /> 2 = UNIQUE KEY-Einschränkung<br /><br /> 3 = FOREIGN KEY-Einschränkung<br /><br /> 4 = CHECK-Einschränkung<br /><br /> 5 = DEFAULT-Einschränkung<br /><br /> 16 = Einschränkung auf Spaltenebene<br /><br /> 32 = Einschränkung auf Tabellenebene|  
|**actions**|**int**|Reserviert|  
|**error**|**int**|Reserviert|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Zuordnung von Systemtabellen zu System Sichten &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [Kompatibilitätssichten &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  
