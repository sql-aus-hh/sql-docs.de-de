---
description: sys.sysusers (Transact-SQL)
title: sys.sysBenutzer (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.sysusers
- sys.sysusers_TSQL
- sysusers_TSQL
- sysusers
dev_langs:
- TSQL
helpviewer_keywords:
- sysusers system table
- sys.sysusers compatibility view
ms.assetid: 5f0e6a8d-c983-44f6-97e9-aab5bff67d18
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 812aca2816b9df05596509e4a61b50d2e4e59d56
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095325"
---
# <a name="syssysusers-transact-sql"></a>sys.sysusers (Transact-SQL)
[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

  Enthält eine Zeile für jeden [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows-Benutzer, jede Windows-Gruppe, jeden [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Benutzer oder jede [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Rolle in der Datenbank.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**uid**|**smallint**|In dieser Datenbank eindeutiger Benutzername.<br /><br /> 1 = Datenbankbesitzer<br /><br /> Führt zu einem Überlauf oder gibt NULL zurück, wenn die Anzahl von Benutzern und Rollen 32.767 übersteigt.|  
|**status**|**smallint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**name**|**sysname**|Der in dieser Datenbank eindeutige Benutzer- oder Gruppenname.|  
|**sid**|**varbinary(85)**|Sicherheitsbezeichner für diesen Eintrag.|  
|**Rollen**|**varbinary(2048)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**mit "up"**|**datetime**|Datum, an dem das Konto hinzugefügt wurde.|  
|**updatedate**|**datetime**|Datum, an dem das Konto zuletzt geändert wurde.|  
|**altuid**|**smallint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]<br /><br /> Führt zu einem Überlauf oder gibt NULL zurück, wenn die Anzahl von Benutzern und Rollen 32.767 übersteigt.|  
|**password**|**varbinary (256)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**gid**|**smallint**|ID der Gruppe, zu der dieser Benutzer gehört. Wenn **uid** mit **gid** identisch ist, definiert dieser Eintrag eine Gruppe. Führt zu einem Überlauf oder gibt NULL zurück, wenn die kombinierte Anzahl von Gruppen und Benutzern 32.767 übersteigt.|  
|**Environ**|**varchar (255)**|Reserviert.|  
|**hasdbaccess**|**int**|1 = Konto hat Datenbankzugriff.|  
|**islogin**|**int**|1 = Konto ist eine Windows-Gruppe, ein Windows-Benutzer oder ein [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Benutzer mit einem Anmeldekonto.|  
|**isntname**|**int**|1 = Konto ist eine Windows-Gruppe oder ein Windows-Benutzer.|  
|**isntgroup**|**int**|1 = Konto ist eine Windows-Gruppe.|  
|**isntuser**|**int**|1 = Konto ist ein Windows-Benutzer.|  
|**issqluser**|**int**|1 = Konto ist ein [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Benutzer.|  
|**isaliased**|**int**|1 = Konto wird als Alias für einen anderen Benutzer verwendet.|  
|**issqlrole**|**int**|1 = Konto ist eine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Rolle.|  
|**isapprole**|**int**|1 = Konto ist eine Anwendungsrolle.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Zuordnung von Systemtabellen zu System Sichten &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [Kompatibilitätssichten &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  
