---
description: sys.dm_clr_appdomains (Transact-SQL)
title: sys.dm_clr_appdomains (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_clr_appdomains
- sys.dm_clr_appdomains
- dm_clr_appdomains_TSQL
- sys.dm_clr_appdomains_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_clr_appdomains dynamic management dynamic management view
ms.assetid: 9fe0d4fd-950a-4274-a493-85e776278045
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: ae934fc3b9d8f7664e2542450ee1456298548c85
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097766"
---
# <a name="sysdm_clr_appdomains-transact-sql"></a>sys.dm_clr_appdomains (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Gibt eine Zeile für jede Anwendungsdomäne auf dem Server zurück. Die Anwendungsdomäne (**AppDomain**) ist ein Konstrukt im [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] Common Language Runtime (CLR), das die Isolationseinheit für eine Anwendung ist. Mit dieser Ansicht können Sie CLR-Integrations Objekte, die in ausgeführt werden, verstehen und beheben [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Es stehen mehrere Typen von CLR-Integrationsobjekten für verwaltete Datenbanken zur Verfügung. Allgemeine Informationen zu diesen Objekten finden Sie im Thema zum [aufbauen von Datenbankobjekten mit CLR-Integration (Common Language Runtime)](../../relational-databases/clr-integration/database-objects/building-database-objects-with-common-language-runtime-clr-integration.md). Wenn diese Objekte ausgeführt werden, wird von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] eine **AppDomain** erstellt, unter der der erforderliche Code geladen und ausgeführt werden kann. Die Isolationsstufe für eine **AppDomain** ist eine **AppDomain** pro Datenbank pro Besitzer. Das heißt, dass alle CLR-Objekte, die sich im Besitz eines Benutzers befinden, stets in derselben **AppDomain** pro Datenbank ausgeführt werden (wenn ein Benutzer CLR-Datenbankobjekte in verschiedenen Datenbanken registriert, werden die CLR-Datenbankobjekte in verschiedenen Anwendungs Domänen ausgeführt). Eine **AppDomain** wird nicht zerstört, nachdem die Ausführung des Codes abgeschlossen ist. Stattdessen wird sie für die zukünftige Ausführung im Arbeitsspeicher zwischengespeichert. Dadurch wird die Leistung verbessert.  
  
 Weitere Informationen finden Sie unter [Anwendungs Domänen](../../sql-server/what-s-new-in-sql-server-2016.md).  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**appdomain_address**|**varbinary(8)**|Adresse der **AppDomain**. Alle verwalteten Datenbankobjekte, die sich im Besitz eines Benutzers befinden, werden immer in derselben **AppDomain** geladen. Sie können diese Spalte verwenden, um alle zurzeit in dieser **AppDomain** geladenen Assemblys in **sys.dm_clr_loaded_assemblies** zu suchen.|  
|**appdomain_id**|**int**|Die ID der **AppDomain**. Jede **AppDomain** verfügt über eine eindeutige ID.|  
|**appdomain_name**|**varchar (386)**|Der Name der **AppDomain** , die von zugewiesen wird [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|**creation_time**|**datetime**|Uhrzeit, zu der die **AppDomain** erstellt wurde. Da **AppDomains** zwischengespeichert und wieder verwendet werden, um die Leistung zu verbessern, ist **creation_time** nicht notwendigerweise die Zeit, zu der der Code ausgeführt wurde.|  
|**db_id**|**int**|ID der Datenbank, in der diese **AppDomain** erstellt wurde. Code, der in zwei verschiedenen Datenbanken gespeichert ist, kann eine **AppDomain** nicht gemeinsam nutzen|  
|**user_id**|**int**|ID des Benutzers, dessen Objekte in dieser **AppDomain** ausgeführt werden können.|  
|**state**|**nvarchar(128)**|Ein Deskriptor für den aktuellen Status der **AppDomain**. Eine AppDomain kann sich in verschiedenen Zuständen befinden, von Erstellung bis Löschung. Weitere Informationen finden Sie im Abschnitt "Hinweise" in diesem Thema.|  
|**strong_refcount**|**int**|Anzahl der starken Verweise auf diese **AppDomain**. Dies gibt die Anzahl der zurzeit ausgeführten Batches an, die diese **AppDomain** verwenden. Beachten Sie, dass durch die Ausführung dieser Ansicht ein **starker Ref**-Wert erstellt wird. auch wenn derzeit kein Code ausgeführt wird, hat **strong_refcount** den Wert 1.|  
|**weak_refcount**|**int**|Anzahl der schwachen Verweise auf diese **AppDomain**. Gibt an, wie viele Objekte innerhalb der **AppDomain** zwischengespeichert werden. Wenn Sie ein verwaltetes Datenbankobjekt ausführen, wird [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] es zur späteren Wiederverwendung in der **AppDomain** zwischengespeichert. Dadurch wird die Leistung verbessert.|  
|**cost**|**int**|Kosten der **AppDomain**. Je höher die Kosten, desto wahrscheinlicher ist es, dass diese **AppDomain** bei ungenügendem Arbeitsspeicher entladen wird. Die Kosten hängen in der Regel davon ab, wie viel Arbeitsspeicher für die Neuerstellung dieser **AppDomain** benötigt wird.|  
|**value**|**int**|Der Wert der **AppDomain**. Je niedriger der Wert ist, desto wahrscheinlicher ist es, dass diese **AppDomain** bei ungenügendem Arbeitsspeicher entladen wird. Der Wert hängt in der Regel davon ab, wie viele Verbindungen oder Batches diese **AppDomain** verwenden.|  
|**total_processor_time_ms**|**bigint**|Gesamtprozessorzeit in Millisekunden, die von allen Threads beim Ausführen in der aktuellen Anwendungsdomäne ab dem Start des Prozesses verwendet wird. Dies entspricht **System. AppDomain. MonitoringTotalProcessorTime**.|  
|**total_allocated_memory_kb**|**bigint**|Gesamtgröße, in Kilobyte, aller Speicherbelegungen durch die Anwendungsdomäne seit der Erstellung, ohne Abzug des bei Sammlungsvorgängen freigegebenen Speichers. Dies entspricht **System. AppDomain. monitoringtotalzustellungs-Speicher MemorySize**.|  
|**survived_memory_kb**|**bigint**|Menge der Daten in KB, die die letzte vollständige Sammlung mit exklusivem Zugriff überdauert haben, und von denen bekannt ist, dass sie von der aktuellen Anwendungsdomäne referenziert werden. Dies entspricht **System. AppDomain. MonitoringSurvivedMemorySize**.|  
  
## <a name="remarks"></a>Bemerkungen  
 Es besteht eine 1: Mai-Beziehung zwischen **dm_clr_appdomains. appdomain_address** und **dm_clr_loaded_assemblies. appdomain_address**.  
  
 In den folgenden Tabellen sind mögliche **Zustands** Werte, deren Beschreibungen und deren Auftreten im **AppDomain** -Lebenszyklus aufgeführt. Sie können diese Informationen verwenden, um den Lebenszyklus einer **AppDomain** zu verfolgen und um zu überwachen, dass verdächtige oder sich wiederholende **AppDomain** -Instanzen entladen werden, ohne dass das Windows-Ereignisprotokoll analysiert werden muss.  
  
## <a name="appdomain-initialization"></a>AppDomain-Initialisierung  
  
|State|BESCHREIBUNG|  
|-----------|-----------------|  
|E_APPDOMAIN_CREATING|Die **AppDomain** wird erstellt.|  
  
## <a name="appdomain-usage"></a>AppDomain-Verwendung  
  
|State|BESCHREIBUNG|  
|-----------|-----------------|  
|E_APPDOMAIN_SHARED|Die **AppDomain** der Laufzeit kann von mehreren Benutzern verwendet werden.|  
|E_APPDOMAIN_SINGLEUSER|Die **AppDomain** ist bereit für die Verwendung in DDL-Vorgängen. Diese unterscheiden sich von E_APPDOMAIN_SHARED, indem im Gegensatz zu DDL-Vorgängen freigegebene AppDomains für die CLR-Integration verwendet werden, . Solche AppDomains werden von anderen gleichzeitigen Vorgängen isoliert.|  
|E_APPDOMAIN_DOOMED|Das Entladen der **AppDomain** ist geplant, aber zurzeit werden Threads darin ausgeführt.|  
  
## <a name="appdomain-cleanup"></a>AppDomain-Cleanup  
  
|State|BESCHREIBUNG|  
|-----------|-----------------|  
|E_APPDOMAIN_UNLOADING|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] hat angefordert, dass die CLR die **AppDomain** entladen soll, in der Regel, weil die Assembly, die die verwalteten Datenbankobjekte enthält, geändert oder gelöscht wurde.|  
|E_APPDOMAIN_UNLOADED|Die CLR hat die **AppDomain** entladen. Dies ist normalerweise das Ergebnis eines Eskalations Verfahrens aufgrund von **ThreadAbort**, **outhfmemory** oder einer nicht behandelten Ausnahme im Benutzercode.|  
|E_APPDOMAIN_ENQUEUE_DESTROY|Die **AppDomain** wurde in CLR entladen und so festgelegt, dass Sie von zerstört wird [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|E_APPDOMAIN_DESTROY|Die **AppDomain** wird gerade von zerstört [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|E_APPDOMAIN_ZOMBIE|Die **AppDomain** wurde von zerstört. es wurden [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] jedoch nicht alle Verweise auf die **AppDomain** bereinigt.|  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die VIEW SERVER STATE-Berechtigung in der Datenbank.  
  
## <a name="examples"></a>Beispiele  
 Das folgende Beispiel zeigt, wie Sie die Details einer **AppDomain** für eine bestimmte Assembly anzeigen:  
  
```  
select appdomain_id, creation_time, db_id, user_id, state  
from sys.dm_clr_appdomains a  
where appdomain_address =   
(select appdomain_address   
 from sys.dm_clr_loaded_assemblies  
   where assembly_id = 500);  
```  
  
 Im folgenden Beispiel wird gezeigt, wie alle Assemblys in einer bestimmten **AppDomain** angezeigt werden:  
  
```  
select a.name, a.assembly_id, a.permission_set_desc, a.is_visible, a.create_date, l.load_time   
from sys.dm_clr_loaded_assemblies as l   
inner join sys.assemblies as a  
on l.assembly_id = a.assembly_id  
where l.appdomain_address =   
(select appdomain_address   
from sys.dm_clr_appdomains  
where appdomain_id = 15);  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [sys.dm_clr_loaded_assemblies &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-clr-loaded-assemblies-transact-sql.md)   
 [Dynamische Verwaltungs Sichten im Zusammenhang mit der Common Language Runtime &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/common-language-runtime-related-dynamic-management-views-transact-sql.md)  
  
