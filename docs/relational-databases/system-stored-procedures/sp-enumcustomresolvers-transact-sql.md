---
title: Sp_enumcustomresolvers (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology:
- replication
ms.topic: language-reference
f1_keywords:
- sp_enumcustomresolvers
- sp_enumcustomresolvers_TSQL
helpviewer_keywords:
- sp_enumcustomresolvers
ms.assetid: 81bd0d3a-48dc-42b1-b662-c630f61fc630
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: d23b5ddef3f2eebfe69974364c0a112140b68da0
ms.sourcegitcommit: 61381ef939415fe019285def9450d7583df1fed0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2018
ms.locfileid: "47599428"
---
# <a name="spenumcustomresolvers-transact-sql"></a>sp_enumcustomresolvers (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Gibt eine Liste aller verfügbaren Geschäftslogikhandler und benutzerdefinierten Konfliktlöser zurück, die auf dem Verteiler registriert sind. Diese gespeicherte Prozedur wird auf dem Verleger für jede Datenbank ausgeführt.  
  
 ![Themenlinksymbol](../../database-engine/configure-windows/media/topic-link.gif "Topic link icon") [Transact-SQL Syntax Conventions (Transact-SQL-Syntaxkonventionen)](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sp_enumcustomresolvers [ [ @distributor =] 'distributor']  
```  
  
## <a name="arguments"></a>Argumente  
 [  **@distributor =**] **"***Verteiler***"**  
 Der Name des Verteilers, auf dem sich der benutzerdefinierte Konfliktlöser befindet. *Verteiler* ist **Sysname**, hat den Standardwert NULL. *Dieser Parameter ist veraltet und wird in einer zukünftigen Version entfernt.*  
  
## <a name="result-sets"></a>Resultsets  
  
|Spaltenname|Datentyp|Description|  
|-----------------|---------------|-----------------|  
|**article_resolver**|**nvarchar(255)**|Anzeigename für den Geschäftslogikhandler oder Konfliktlöser|  
|**resolver_clsid**|**nvarchar(50)**|Die Klassen-ID (CLSID, Class ID) des COM-basierten Konfliktlösers. Für einen Geschäftslogikhandler gibt diese Spalte einen CLSID-Wert von Null zurück.|  
|**is_dotnet_assembly**|**bit**|Gibt an, ob die Registrierung für einen Geschäftslogikhandler ist.<br /><br /> **0** = COM-basierte Konfliktlöser<br /><br /> **1** = Geschäftslogikhandler|  
|**dotnet_assembly_name**|**nvarchar(255)**|Der Name der [!INCLUDE[msCoName](../../includes/msconame-md.md)] .NET Framework-Assembly, die den Geschäftslogikhandler implementiert.|  
|**dotnet_class_name**|**nvarchar(255)**|Der Name der Klasse, die <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule> überschreibt, um den Geschäftslogikhandler zu implementieren|  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 **0** (Erfolg) oder **1** (Fehler)  
  
## <a name="remarks"></a>Hinweise  
 **Sp_enumcustomresolvers** wird bei der Mergereplikation verwendet.  
  
## <a name="permissions"></a>Berechtigungen  
 Nur Mitglieder der der **Sysadmin** Serverrolle und die **Db_owner** feste Datenbankrolle können ausführen **Sp_enumcustomresolvers**.  
  
## <a name="see-also"></a>Siehe auch  
 [Implementieren eines Geschäftslogikhandlers für einen Mergeartikel](../../relational-databases/replication/implement-a-business-logic-handler-for-a-merge-article.md)   
 [Implementieren eines benutzerdefinierten Konfliktlösers für einen Mergeartikel](../../relational-databases/replication/implement-a-custom-conflict-resolver-for-a-merge-article.md)   
 [Sp_lookupcustomresolver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-lookupcustomresolver-transact-sql.md)   
 [Sp_unregistercustomresolver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-unregistercustomresolver-transact-sql.md)   
 [Gespeicherte Systemprozeduren &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
