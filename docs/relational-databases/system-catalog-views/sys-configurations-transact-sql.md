---
description: sys.configurations (Transact-SQL)
title: sys.configurationen (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.configurations_TSQL
- configurations
- sys.configurations
- configurations_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.configurations catalog view
ms.assetid: c4709ed1-bf88-4458-9e98-8e9b78150441
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 10956e1c3738c64829ec18d92a05b9f427b1833d
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100677"
---
# <a name="sysconfigurations-transact-sql"></a>sys.configurations (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Enthält eine Zeile für jeden serverweiten Konfigurations Optionswert im System.  

|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**configuration_id**|**int**|Eindeutige ID des Konfigurationswerts.|  
|**name**|**nvarchar(35)**|Der Name der Konfigurationsoption.|  
|**value**|**sql_variant**|Der für diese Option konfigurierte Wert.|  
|**minimum**|**sql_variant**|Der Mindestwert für die Konfigurationsoption.|  
|**maximum**|**sql_variant**|Der Höchstwert für die Konfigurationsoption.|  
|**value_in_use**|**sql_variant**|Ausgeführter Wert, der derzeit für diese Option wirksam ist.|  
|**description**|**nvarchar(255)**|Beschreibung der Konfigurationsoption.|  
|**is_dynamic**|**bit**|1 = Variable, die bei Ausführung der RECONFIGURE-Anweisung wirksam wird.|  
|**is_advanced**|**bit**|1 = die Variable wird nur angezeigt, wenn die **Option advancedoption anzeigen** festgelegt ist.|  
  
 ## <a name="remarks"></a>Bemerkungen
  Eine Liste aller Server Konfigurationsoptionen finden Sie unter [Server Configuration options &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md).  
  
> [!NOTE]  
>  Informationen zu den Konfigurationsoptionen auf Datenbankebene finden Sie unter [ALTER DATABASE scoped Configuration &#40;Transact-SQL-&#41;](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md). Informationen zum Konfigurieren von Soft-NUMA finden Sie unter [Soft-NUMA &#40;SQL Server&#41;](../../database-engine/configure-windows/soft-numa-sql-server.md).  
 
Die sys.configurations-Katalog Sicht kann verwendet werden, um die config_value (die Wert Spalte), den run_value (die value_in_use Spalte) und die Option zu bestimmen, ob es sich um eine dynamische Konfigurationsoption handelt (erfordert keine Server-Engine-Neustarts oder die is_dynamic Spalte).

> [!NOTE]
> Die config_value im Resultset von sp_configure entspricht der **sys.configurations. Value** -Spalte. Der **run_value** entspricht der Spalte **sys.configurations.value_in_use** .

Die folgende Abfrage kann verwendet werden, um zu bestimmen, ob konfigurierte Werte nicht installiert wurden:

```SQL
select * from sys.configurations where value != value_in_use
```

Wenn der Wert der Änderung für die von Ihnen vorgenommene Konfigurationsoption entspricht, der **value_in_use** jedoch nicht identisch ist, wurde entweder der RECONFIGURE-Befehl nicht ausgeführt, oder es ist ein Fehler aufgetreten, oder die Server-Engine muss neu gestartet werden.

Es gibt Konfigurationsoptionen, bei denen der Wert und die value_in_use möglicherweise nicht identisch sind und dieses Verhalten erwartet wird. Beispiel:

"Max. Server Arbeitsspeicher (MB)": der konfigurierte Standardwert von 0 wird als **value_in_use** = 2147483647 angezeigt.<br>

"min. Server Arbeitsspeicher (MB)": der konfigurierte Standardwert "0" wird möglicherweise als **value_in_use** = 8 (32-Bit) oder 16 (64 Bit) angezeigt. In einigen Fällen ist der **value_in_use** 0. In dieser Situation ist der Wert "true" **value_in_use** 8 (32-Bit) oder 16 (64 Bit).


Mithilfe der Spalte **is_dynamic** kann bestimmt werden, ob für die Konfigurationsoption ein Neustart erforderlich ist. is_dynamic = 1 bedeutet, dass der neue Wert beim Ausführen der RECONFIGURE-Befehls (T-SQL) sofort wirksam wird (in einigen Fällen wertet die Server-Engine den neuen Wert möglicherweise nicht sofort aus, führt dies jedoch im normalen Verlauf der Ausführung aus). is_dynamic = 0 bedeutet, dass der geänderte Konfigurations Wert erst wirksam wird, wenn der Server neu gestartet wird, obwohl der Befehl RECONFIGURE (T-SQL) ausgeführt wurde.

Für eine Konfigurationsoption, die nicht dynamisch ist, gibt es keine Möglichkeit, zu ermitteln, ob der Befehl RECONFIGURE (T-SQL) ausgeführt wurde, um den ersten Schritt bei der Installation der Konfigurationsänderung auszuführen. Bevor Sie SQL Server neu starten, um eine Konfigurationsänderung zu installieren, führen Sie den Befehl RECONFIGURE (T-SQL) aus, um sicherzustellen, dass alle Konfigurationsänderungen nach einem SQL Server Neustart wirksam werden. 
 
 
## <a name="permissions"></a>Berechtigungen  
 Erfordert die Mitgliedschaft in der **public** -Rolle. Weitere Informationen finden Sie unter [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Server weite Konfigurations Katalog Sichten &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/server-wide-configuration-catalog-views-transact-sql.md)   
 [Katalogsichten &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
