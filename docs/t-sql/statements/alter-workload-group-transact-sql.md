---
description: ALTER WORKLOAD GROUP (Transact-SQL)
title: ALTER WORKLOAD GROUP (Transact-SQL)
ms.custom: ''
ms.date: 05/05/2020
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ALTER_WORKLOAD_GROUP_TSQL
- ALTER WORKLOAD GROUP
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER WORKLOAD GROUP statement
ms.assetid: 957addce-feb0-4e54-893e-5faca3cd184c
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=sql-server-2016||>=sql-server-linux-2017||=azure-sqldw-latest||=azuresqldb-mi-current'
ms.openlocfilehash: 2c9c90cb1107d33f6ecbb42e0974371555d9b9aa
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098420"
---
# <a name="alter-workload-group-transact-sql"></a>ALTER WORKLOAD GROUP (Transact-SQL)

[!INCLUDE[select-product](../../includes/select-product.md)]

::: moniker range=">=sql-server-2016||>=sql-server-linux-2017"

:::row:::
    :::column:::
        **_\* SQL Server \*_** &nbsp;
    :::column-end:::
    :::column:::
        [SQL Managed Instance](alter-workload-group-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-workload-group-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-server-and-sql-managed-instance"></a>SQL Server und SQL Managed Instance

[!INCLUDE [ALTER WORKLOAD GROUP](../../includes/alter-workload-group.md)]
  
::: moniker-end
::: moniker range="=azuresqldb-mi-current"

:::row:::
    :::column:::
        [SQL Server](alter-workload-group-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* SQL Managed Instance \*_** &nbsp;
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-workload-group-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-server-and-sql-managed-instance"></a>SQL Server und SQL Managed Instance

[!INCLUDE [ALTER WORKLOAD GROUP](../../includes/alter-workload-group.md)]

::: moniker-end
::: moniker range="=azure-sqldw-latest"

:::row:::
    :::column:::
        [SQL Server](alter-workload-group-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL Managed Instance](alter-workload-group-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Azure Synapse<br />Analytics \*_** &nbsp;
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="azure-synapse-analytics"></a>Azure Synapse Analytics

Ändert eine vorhandene Arbeitsauslastungsgruppe.

Im Abschnitt zum `ALTER WORKLOAD GROUP`-Verhalten weiter unten erhalten Sie weitere Informationen dazu, wie `ALTER WORKLOAD GROUP` sich auf einem System mit laufenden und sich in der Warteschlange befindenden Anforderungen verhält. 

Einschränkungen für [CREAT WORKLOAD GROUP](create-workload-group-transact-sql.md) gelten auch für `ALTER WORKLOAD GROUP`.  Fragen Sie vor dem Ändern der Parameter [sys.workload_management_workload_groups](../../relational-databases/system-catalog-views/sys-workload-management-workload-groups-transact-sql.md) ab, um sicherzustellen, dass sich die Werte innerhalb akzeptabler Bereiche befinden.

## <a name="syntax"></a>Syntax

```syntaxsql
ALTER WORKLOAD GROUP group_name
 WITH
 ([       MIN_PERCENTAGE_RESOURCE = value ]
  [ [ , ] CAP_PERCENTAGE_RESOURCE = value ]
  [ [ , ] REQUEST_MIN_RESOURCE_GRANT_PERCENT = value ]
  [ [ , ] REQUEST_MAX_RESOURCE_GRANT_PERCENT = value ] 
  [ [ , ] IMPORTANCE = { LOW | BELOW_NORMAL | NORMAL | ABOVE_NORMAL | HIGH }]
  [ [ , ] QUERY_EXECUTION_TIMEOUT_SEC = value ] )
  [ ; ]
  ```

## <a name="arguments"></a>Argumente

group_name  
Der Name einer vorhandenen benutzerdefinierten Arbeitsauslastungsgruppe, die geändert wird.  Die group-name-Gruppe kann nicht verändert werden. 

MIN_PERCENTAGE_RESOURCE = value  
Dabei entspricht „value“ einem Integer zwischen 0 und 100.  Wenn Sie „min_percentage_resource“ bearbeiten, darf die Summe von „min_percentage_resource“ für alle Auslastungsgruppen nicht 100 überschreiten.  Zum Ändern von „min_percentage-resource“ müssen alle laufenden Abfragen in der Arbeitsauslastungsgruppe abgeschlossen werden, bevor der Befehl ausgeführt wird.  Im Abschnitt „Verhalten von ALTER WORKLOAD GROUP“ in diesem Dokument erhalten Sie weitere Informationen.

CAP_PERCENTAGE_RESOURCE = value  
Dabei entspricht „value“ einem Integer von 1 bis 100.  Der Wert für cap_percentage_resource muss den von min_percentage_resource übersteigen.  Zum Ändern von „cap_percentage_resource“ müssen alle laufenden Abfragen in der Arbeitsauslastungsgruppe abgeschlossen werden, bevor der Befehl ausgeführt wird.  Im Abschnitt „Verhalten von ALTER WORKLOAD GROUP“ in diesem Dokument erhalten Sie weitere Informationen. 

REQUEST_MIN_RESOURCE_GRANT_PERCENT = value  
Dabei entspricht „value“ einem Dezimalwert mit einem Bereich zwischen 0,75 und 100,00.  Der Wert für „request_min_resource_grant_percent“ muss ein Faktor von „min_percentage_resource“ und weniger als „cap_percentage_resource“ sein. 
  
REQUEST_MAX_RESOURCE_GRANT_PERCENT = value  
Value ist eine Dezimalzahl und muss größer als „request_min_resource_grant_percent“ sein.

IMPORTANCE = {LOW \| BELOW_NORMAL \| NORMAL \| ABOVE_NORMAL \| HIGH}  
Diese Zeile ändert die Standardwichtigkeit einer Anforderung für die Arbeitsauslastungsgruppe.

QUERY_EXECUTION_TIMEOUT_SEC = value  
Diese Zeile ändert die maximale Zeit in Sekunden, für deren Dauer eine Abfrage ausgeführt werden kann, bevor sie abgebrochen wird. Hierbei muss value 0 (Null) oder ein positiver Integer sein. Die Standardeinstellung für value ist 0 (null), also unbegrenzt.   

## <a name="permissions"></a>Berechtigungen

Erfordert die CONTROL DATABASE-Berechtigung

## <a name="example"></a>Beispiel

Im folgenden Beispiel werden die Werte in der Katalogansicht für „wgDataLoads“ überprüft, und die Werte werden geändert.

```sql
SELECT *
FROM sys.workload_management_workload_groups  
WHERE [name] = 'wgDataLoads'

ALTER WORKLOAD GROUP wgDataLoads WITH
( MIN_PERCENTAGE_RESOURCE            = 40
 ,CAP_PERCENTAGE_RESOURCE            = 80
 ,REQUEST_MIN_RESOURCE_GRANT_PERCENT = 10 )
 ```

## <a name="alter-workload-group-behavior"></a>Verhalten von ALTER WORKLOAD GROUP

Zu jedem Zeitpunkt gibt es drei Verhaltensarten im System:
- Anforderung, die noch nicht klassifiziert wurden
- Anforderungen, die klassifiziert wurden und auf Objektsperren oder Systemressourcen warten
- Anforderungen, die klassifiziert wurden und ausgeführt werden

Auf Grundlage der Eigenschaften einer Arbeitsauslastungsgruppe, die geändert wird, unterscheidet sich der Zeitpunkt, zu dem die Einstellungen wirksam werden.

**Relevanz oder query_execution_timeout** Für die Relevanz und die Eigenschaften von „query_execution_timeout“ übernehmen nicht klassifizierte Anforderungen die neuen Konfigurationswerte.  Wartende und laufende Anforderungen werden mit der alten Konfiguration ausgeführt.  Die `ALTER WORKLOAD GROUP`-Anforderung wird sofort ausgeführt, unabhängig davon, ob in der Arbeitsauslastungsgruppe Abfragen ausgeführt werden.

**Request_min_resource_grant_percent oder request_max_resource_grant_percent** Für „request_min_resource_grant_percent“ und „request_max_resource_grant_percent“ werden laufende Anforderungen mit der alten Konfiguration ausgeführt.  Wartende Anforderungen und nicht klassifizierte Anforderungen übernehmen die neuen Konfigurationswerte.  Die `ALTER WORKLOAD GROUP`-Anforderung wird sofort ausgeführt, unabhängig davon, ob in der Arbeitsauslastungsgruppe Abfragen ausgeführt werden.

**Min_percentage_resource oder cap_percentage_resource** Für „min_percentage_resource“ und „cap_percentage_resource“ werden laufende Anforderungen mit der alten Konfiguration ausgeführt.  Wartende Anforderungen und nicht klassifizierte Anforderungen übernehmen die neuen Konfigurationswerte. 

Das Ändern von „min_percentage_resource“ und „cap_percentage_resource“ erfordert die Ableitung laufender Anforderungen in der Arbeitsauslastungsgruppe, die geändert wird.  Wenn „min_percentage_resource“ verringert wird, werden die freigegebenen Ressourcen an den Freigabepool zurückgegeben. So können Anforderungen aus anderen Arbeitsauslastungsgruppen diese auch verwenden.  Umgekehrt wird durch die Erhöhung von „min_percentage_resource“ gewartet, bis Anforderungen, die nur die benötigten Ressourcen aus dem Freigabepool nutzen, verarbeitet werden.  Der ALTER WORKLOAD GROUP-Vorgang hat noch vor allen anderen Anforderungen, die auf Ausführung in einem freigegebenen Pool warten, priorisierten Zugriff auf freigegebene Ressourcen.  Wenn die Summe von „min_percentage_resource“ 100 % überschreitet, schlägt die ALTER WORKLOAD GROUP-Anforderung sofort fehl. 

**Sperrverhalten** Die Änderung einer Arbeitsauslastungsgruppe erfordert eine globale Sperre für alle Arbeitsauslastungsgruppen.  Eine Anforderung zum Ändern einer Arbeitsauslastungsgruppe würde sich in einer Warteschlange hinter den bereits übermittelten CREATE- oder DROP-Anforderungen der Arbeitsauslastungsgruppe befinden.  Wenn ein Batch von ALTER-Anweisungen gleichzeitig übermittelt wird, werden sie in der Reihenfolge verarbeitet, in der sie eingegangen sind.  

## <a name="see-also"></a>Weitere Informationen

- [CREATE WORKLOAD GROUP &#40;Transact-SQL&#41;](create-workload-group-transact-sql.md)
- [DROP WORKLOAD GROUP &#40;Transact-SQL&#41;](drop-workload-group-transact-sql.md)
- [sys.workload_management_workload_groups](../../relational-databases/system-catalog-views/sys-workload-management-workload-groups-transact-sql.md)
- [sys.dm_workload_management_workload_groups_stats](../../relational-databases/system-dynamic-management-views/sys-dm-workload-management-workload-group-stats-transact-sql.md)
- [Schnellstart: Konfigurieren der Arbeitsauslastungsisolation mithilfe von T-SQL](/azure/sql-data-warehouse/quickstart-configure-workload-isolation-tsql)

::: moniker-end
