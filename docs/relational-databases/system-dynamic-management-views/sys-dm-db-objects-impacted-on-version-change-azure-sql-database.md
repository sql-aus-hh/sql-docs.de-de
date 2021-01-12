---
description: sys.dm_db_objects_impacted_on_version_change (Azure SQL-Datenbank)
title: sys.dm_db_objects_impacted_on_version_change
titleSuffix: Azure SQL Database
ms.date: 03/03/2017
ms.service: sql-database
ms.reviewer: ''
ms.topic: language-reference
f1_keywords:
- sys.dm_db_objects_impacted_on_version_change_TSQL
- dm_db_objects_impacted_on_version_change
- dm_db_objects_impacted_on_version_change_TSQL
- sys.dm_db_objects_impacted_on_version_change
dev_langs:
- TSQL
helpviewer_keywords:
- dm_db_objects_impacted_on_version_change
- sys.dm_db_objects_impacted_on_version_change
ms.assetid: b94af834-c4f6-4a27-80a6-e8e71fa8793a
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: = azuresqldb-current
ms.custom: seo-dt-2019
ms.openlocfilehash: f7830e874026b9d5af250fde294c8c72294d013d
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097700"
---
# <a name="sysdm_db_objects_impacted_on_version_change-azure-sql-database"></a>sys.dm_db_objects_impacted_on_version_change (Azure SQL-Datenbank)
[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

  Diese datenbankbezogene Systemsicht wurde entworfen, um ein Frühwarnsystem bereitzustellen, mit dem Objekte ermittelt werden sollen, die von einem Upgrade der Hauptversion in [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] betroffen sein werden. Sie können die Sicht entweder vor oder nach dem Upgrade verwenden, um eine vollständige Enumeration der betroffenen Objekte abzurufen. Sie müssen diese Sicht in jeder Datenbank abfragen, damit der gesamte Server berücksichtigt wird.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|class|**int** nicht NULL|Die Klasse des Objekts, das betroffen sein wird:<br /><br /> **1** = Einschränkung<br /><br /> **7** = Indizes und Heaps|  
|class_desc|**nvarchar (60)** nicht NULL|Beschreibung der Klasse:<br /><br /> **OBJECT_OR_COLUMN**<br /><br /> **INDEX**|  
|major_id|**int** nicht NULL|Objekt-ID der Einschränkung oder Objekt-ID der Tabelle, die den Index oder Heap enthält.|  
|minor_id|**int** Normal|**NULL** für Einschränkungen<br /><br /> Index_id für Indizes und Heaps|  
|dependency|**nvarchar (60)** nicht NULL|Beschreibung der Abhängigkeit, die bewirkt, dass die Einschränkung oder der Index betroffen sind. Derselbe Wert wird auch für Warnungen verwendet, die während des Upgrades generiert werden.<br /><br /> Beispiele:<br /><br /> **Speicherplatz** (für systeminterne Objekte)<br /><br /> **Geometrie** (für System-UDT)<br /><br /> **Geografie: Analysieren** (für System-UDT-Methode)|  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die VIEW DATABASE STATE-Berechtigung.  
  
## <a name="example"></a>Beispiel  
 Das folgende Beispiel zeigt eine Abfrage bei **sys.dm_db_objects_impacted_on_version_change**, um die Objekte zu finden, die von einem Upgrade zur nächstgrößeren Serverversion betroffen wären.  
  
```  
SELECT * FROM sys.dm_db_objects_disabled_on_version_change;  
GO  
```  
  
```  
class  class_desc        major_id    minor_id    dependency                       
------ ----------------- ----------- ----------- ----------   
1      OBJECT_OR_COLUMN  181575685   NULL        geometry                        
7      INDEX             37575172    1           geometry                        
7      INDEX             2121058592  1           geometry                        
1      OBJECT_OR_COLUMN  101575400   NULL        geometry     
```  
  
## <a name="remarks"></a>Bemerkungen  
  
### <a name="how-to-update-impacted-objects"></a>Aktualisieren betroffener Objekte  
 Die folgenden Schritte beschreiben die Korrekturmaßnahmen, die Sie nach dem bevorstehenden Serviceupgrade im Juni durchführen sollten.  
  
|Order|Betroffenes Objekt|Korrekturmaßnahme|  
|-----------|---------------------|-----------------------|  
|1|**Indizes**|Erstellen Sie einen Index neu, der durch **sys.dm_db_objects_impacted_on_version_change** identifiziert wird. Beispiel:  `ALTER INDEX ALL ON <table> REBUILD`<br />oder<br />`ALTER TABLE <table> REBUILD`|  
|2|**Object**|Alle Einschränkungen, die durch **sys.dm_db_objects_impacted_on_version_change** gekennzeichnet sind, müssen nach der Neuberechnung der Geometrie- und Geografiedaten in der zugrunde liegenden Tabelle erneut überprüft werden. Führen Sie die erneute Überprüfung für Einschränkungen mithilfe von ALTER TABLE durch. <br />Beispiel: <br />`ALTER TABLE <tab> WITH CHECK CHECK CONSTRAINT <constraint name>`<br />oder<br />`ALTER TABLE <tab> WITH CHECK CONSTRAINT ALL`|  
  
  
