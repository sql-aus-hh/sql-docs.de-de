---
description: sys.syscacheobjects (Transact-SQL)
title: sys.syscacheobjects (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.syscacheobjects_TSQL
- sys.syscacheobjects
- syscacheobjects
- syscacheobjects_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- syscacheobjects system table
- sys.syscacheobjects compatibility view
ms.assetid: 9b14f37c-b7f5-4f71-b070-cce89a83f69e
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 4b33b97a5b3753d63e1df4759d26970cf6359ee2
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097805"
---
# <a name="syssyscacheobjects-transact-sql"></a>sys.syscacheobjects (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Enthält Informationen zur Verwendung des Caches.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**bucketid**|**int**|Bucket-ID. Der Wert liegt im Bereich von 0 bis (Verzeichnisgröße - 1). Die Verzeichnisgröße ist die Größe der Hashtabelle.|  
|**cacheobjtype**|**nvarchar (17)**|Typ des Objekts im Cache:<br /><br /> Kompilierter Plan<br /><br /> Ausführbarer Plan<br /><br /> Analysestruktur<br /><br /> Cursor<br /><br /> Erweiterte gespeicherte Prozedur|  
|**objtype**|**nvarchar (8)**|Typ des Objekts:<br /><br /> Gespeicherte Prozedur<br /><br /> Vorbereitete Anweisung<br /><br /> Ad-hoc-Abfrage ([!INCLUDE[tsql](../../includes/tsql-md.md)] -Abfragen, die als Sprachereignisse von den Hilfsprogrammen **sqlcmd** oder **osql** aus übermittelt wurden, im Gegensatz zu Remoteprozeduraufrufen)<br /><br /> ReplProc (Replikationsprozedur)<br /><br /> Trigger<br /><br /> Sicht<br /><br /> Standard<br /><br /> Benutzertabelle<br /><br /> Systemtabelle<br /><br /> Prüfen<br /><br /> Regel|  
|**objid**|**int**|Einer der Hauptschlüssel zur Suche nach einem Objekt im Cache. Für Datenbankobjekte (Prozeduren, Sichten, Trigger usw.) ist dies die Objekt-ID, die in **sysobjects** gespeichert wird. Bei Cacheobjekten, wie Ad-hoc-SQL-Code oder vorbereiteter SQL-Code, ist **objid** ein intern generierter Wert.|  
|**dbid**|**smallint**|ID der Datenbank, in der das Cacheobjekt kompiliert wurde.|  
|**dbidexec**|**smallint**|Datenbank-ID, von der die Abfrage ausgeführt wird.<br /><br /> Bei den meisten Objekten besitzt **dbidexec** denselben Wert wie **dbid**.<br /><br /> Bei Systemsichten ist **dbidexec** die Datenbank-ID, von der die Abfrage ausgeführt wird.<br /><br /> Für Ad-hoc-Abfragen ist **dbidexec** 0. Dies bedeutet, dass **dbidexec** denselben Wert besitzt wie **dbid**.|  
|**UID**|**smallint**|Bei Ad-hoc-Abfrageplänen und vorbereiteten Plänen zeigt diese ID den Ersteller des Plans an.<br /><br /> -2 = Der abgesendete Batch hängt nicht von der impliziten Namensauflösung ab und kann von verschiedenen Benutzern gemeinsam genutzt werden. Dies ist die bevorzugte Methode. Jeder andere Wert stellt den Benutzernamen des Benutzers dar, der die Abfrage in der Datenbank absendet.<br /><br /> Führt zu einem Überlauf oder gibt NULL zurück, wenn die Anzahl von Benutzern und Rollen 32.767 übersteigt.|  
|**refcounts**|**int**|Anzahl von anderen Cacheobjekten, die auf dieses Cacheobjekt verweisen. Eine Anzahl von 1 ist die Basis.|  
|**usecounts**|**int**|Anzahl von Verwendungen dieses Cacheobjekts seit Beginn.|  
|**pagesused**|**int**|Anzahl der Seiten, die vom Cacheobjekt belegt werden.|  
|**setopts**|**int**|Einstellungen von SET-Optionen, die sich auf einen kompilierten Plan auswirken. Diese Einstellungen sind Teil des Cacheschlüssels. Änderungen an Werten in dieser Spalte weisen darauf hin, dass Benutzer SET-Optionen geändert haben. Dazu gehören die folgenden Optionen:<br /><br /> **ANSI_PADDING**<br /><br /> **FORCEPLAN**<br /><br /> **CONCAT_NULL_YIELDS_NULL**<br /><br /> **ANSI_WARNINGS**<br /><br /> **ANSI_NULLS**<br /><br /> **QUOTED_IDENTIFIER**<br /><br /> **ANSI_NULL_DFLT_ON**<br /><br /> **ANSI_NULL_DFLT_OFF**|  
|**langid**|**smallint**|Sprach-ID. ID der Sprache der Verbindung, die das Cacheobjekt erstellt hat.|  
|**DATEFORMAT**|**smallint**|Datumsformat der Verbindung, die das Cacheobjekt erstellt hat.|  
|**status**|**int**|Zeigt an, ob das Cacheobjekt ein Cursorplan ist. Derzeit wird nur das niederwertigste Bit verwendet.|  
|**lasttime**|**bigint**|Nur aus Gründen der Abwärtskompatibilität beibehalten Es wird immer 0 zurückgegeben.|  
|**maxexectime**|**bigint**|Nur aus Gründen der Abwärtskompatibilität beibehalten Es wird immer 0 zurückgegeben.|  
|**avgexectime**|**bigint**|Nur aus Gründen der Abwärtskompatibilität beibehalten Es wird immer 0 zurückgegeben.|  
|**lastreads**|**bigint**|Nur aus Gründen der Abwärtskompatibilität beibehalten Es wird immer 0 zurückgegeben.|  
|**lastwrites**|**bigint**|Nur aus Gründen der Abwärtskompatibilität beibehalten Es wird immer 0 zurückgegeben.|  
|**SqlBytes**|**int**|Länge in Byte der Prozedurdefinition oder des übermittelten Batches.|  
|**sql**|**nvarchar (3900)**|Moduldefinition oder die ersten 3.900 Zeichen des übermittelten Batches.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Kompatibilitätssichten &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  

