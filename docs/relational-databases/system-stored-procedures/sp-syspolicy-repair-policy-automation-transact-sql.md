---
title: Sp_syspolicy_repair_policy_automation (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sp_syspolicy_repair_policy_automation
- sp_syspolicy_repair_policy_automation_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_syspolicy_repair_policy_automation
ms.assetid: d81682e3-2444-4d66-ad00-1cf628632e8b
author: VanMSFT
ms.author: vanto
manager: craigg
ms.openlocfilehash: 67e73bd412299910b0fdd0927466ea953c7991fe
ms.sourcegitcommit: 61381ef939415fe019285def9450d7583df1fed0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2018
ms.locfileid: "47828158"
---
# <a name="spsyspolicyrepairpolicyautomation-transact-sql"></a>sp_syspolicy_repair_policy_automation (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Repariert in der richtlinienbasierten Verwaltung die Richtlinienautomatisierung. Sie können mit dieser gespeicherten Prozedur beispielsweise Trigger und Aufträge reparieren, denen Richtlinien zugeordnet sind, die für die Verwendung der Auswertungsmodi "Nach Zeitplan" oder "Bei Änderung" konfiguriert sind.  
  
 ![Themenlinksymbol](../../database-engine/configure-windows/media/topic-link.gif "Topic link icon") [Transact-SQL Syntax Conventions (Transact-SQL-Syntaxkonventionen)](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sp_syspolicy_repair_policy_automation  
```  
  
## <a name="arguments"></a>Argumente  
 Diese gespeicherte Prozedur weist keine Parameter auf.  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 **0** (Erfolg) oder **1** (Fehler)  
  
## <a name="remarks"></a>Hinweise  
 Sie müssen sp_syspolicy_repair_policy_automation im Kontext der Systemdatenbank msdb ausführen.  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die Mitgliedschaft in der festen Datenbankrolle PolicyAdministratorRole.  
  
> [!IMPORTANT]  
>  Mögliche Erweiterung der Anmeldeinformationen: Benutzer mit der Rolle PolicyAdministratorRole können Servertrigger erstellen und die Ausführung von Richtlinien planen. Dies kann sich auf die Arbeitsweise der [!INCLUDE[ssDE](../../includes/ssde-md.md)]-Instanz auswirken. Ein Benutzer mit der Rolle PolicyAdministratorRole kann beispielsweise eine Richtlinie erstellen, durch die das Erstellen der meisten Objekte in [!INCLUDE[ssDE](../../includes/ssde-md.md)] verhindert wird. Aufgrund dieser möglichen Erweiterung der Anmeldeinformationen, sollte die PolicyAdministratorRole-Rolle gewährt werden nur für Benutzer, die mit der Kontrolle der Konfiguration von vertrauenswürdigen sind die [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird die Richtlinienautomatisierung repariert.  
  
```  
EXEC msdb.dbo.sp_syspolicy_repair_policy_automation;  
  
GO  
```  
  
## <a name="see-also"></a>Siehe auch  
 [Gespeicherte Prozeduren für Richtlinie der richtlinienbasierten Verwaltung &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/policy-based-management-stored-procedures-transact-sql.md)  
  
  
