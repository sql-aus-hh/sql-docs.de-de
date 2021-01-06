---
description: CREATE WORKLOAD GROUP (Transact-SQL)
title: CREATE WORKLOAD GROUP (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 05/27/2020
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- WORKLOAD GROUP
- WORKLOAD_GROUP_TSQL
- CREATE WORKLOAD GROUP
- CREATE_WORKLOAD_GROUP_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE WORKLOAD GROUP statement
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=sql-server-2016||>=sql-server-linux-2017||=azure-sqldw-latest||=azuresqldb-mi-current'
ms.openlocfilehash: 1ad781bd554d7bcf506371ab71e3441e38f91f8c
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643977"
---
# <a name="create-workload-group-transact-sql"></a>CREATE WORKLOAD GROUP (Transact-SQL)

[!INCLUDE[select-product](../../includes/select-product.md)]

::: moniker range=">=sql-server-2016||>=sql-server-linux-2017"

:::row:::
    :::column:::
        **_\* SQL Server \*_** &nbsp;
    :::column-end:::
    :::column:::
        [SQL Managed Instance](create-workload-group-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](create-workload-group-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-server-and-sql-managed-instance"></a>SQL Server und SQL Managed Instance

[!INCLUDE [CREATE WORKLOAD GROUP](../../includes/create-workload-group.md)]
  
::: moniker-end
::: moniker range="=azuresqldb-mi-current"

:::row:::
    :::column:::
        [SQL Server](create-workload-group-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* SQL Managed Instance \*_** &nbsp;
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](create-workload-group-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-server-and-sql-managed-instance"></a>SQL Server und SQL Managed Instance

[!INCLUDE [CREATE WORKLOAD GROUP](../../includes/create-workload-group.md)]

::: moniker-end
::: moniker range="=azure-sqldw-latest"

:::row:::
    :::column:::
        [SQL Server](create-workload-group-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL Managed Instance](create-workload-group-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Azure Synapse<br />Analytics \*_** &nbsp;
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="azure-synapse-analytics"></a>Azure Synapse Analytics

Erstellt eine Arbeitsauslastungsgruppe Arbeitsauslastungsgruppen sind Container für eine Reihe von Anforderungen und die Grundlage für die Konfiguration der Workloadverwaltung auf einem System. Mit Arbeitsauslastungsgruppen können Sie Ressourcen für die Workloadisolation reservieren und Ressourcen beibehalten, pro Anforderung definieren oder Ausführungsregeln durchsetzen. Sobald die Anweisung abgeschlossen ist, sind die Einstellungen wirksam.

:::image type="icon" source="../../database-engine/configure-windows/media/topic-link.gif"::: [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

```syntaxsql
CREATE WORKLOAD GROUP group_name
 WITH
 (   MIN_PERCENTAGE_RESOURCE = value 
   , CAP_PERCENTAGE_RESOURCE = value 
   , REQUEST_MIN_RESOURCE_GRANT_PERCENT = value
  [ [ , ] REQUEST_MAX_RESOURCE_GRANT_PERCENT = value ]
  [ [ , ] IMPORTANCE = { LOW | BELOW_NORMAL | NORMAL | ABOVE_NORMAL | HIGH } ]
  [ [ , ] QUERY_EXECUTION_TIMEOUT_SEC = value ] )
  [ ; ]

```

*group_name*</br>
Gibt den Namen an, mit dem die Arbeitsauslastungsgruppe identifiziert werden kann. Der Name *group_name* ist vom Datentyp „sysname“. Dieses Argument kann bis zu 128 Zeichen lang sein und muss innerhalb der Instanz einen eindeutigen Namen haben.

*MIN_PERCENTAGE_RESOURCE* = value</br>
Diese Zeile gibt eine garantierte minimale Ressourcenzuordnung für diese Arbeitsauslastungsgruppe an, die nicht mit anderen Arbeitsauslastungsgruppen geteilt wird. Dabei entspricht *value* einem Integer zwischen 0 und 100. Die Summe von min_percentage_resource darf für alle Auslastungsgruppen nicht 100 überschreiten. Der Wert für min_percentage_resource darf den von cap_percentage_resource nicht übersteigen. Es gibt effektive Mindestwerte, die pro Dienstebene zulässig sind. Weitere Informationen finden Sie unter [Effektive Werte](#effective-values).

*CAP_PERCENTAGE_RESOURCE* = value</br>
Diese Zeile gibt die maximale Ressourcenverwendung für alle Anforderungen in einer Arbeitsauslastungsgruppe an. Der zulässige Integerbereich für value liegt zwischen 1 und 100. Der Wert für cap_percentage_resource muss den von min_percentage_resource übersteigen. Der effektive Wert für cap_percentage_resource kann reduziert werden, wenn min_percentage_resource in anderen Arbeitsauslastungsgruppen auf 0 oder höher festgelegt wird.

*REQUEST_MIN_RESOURCE_GRANT_PERCENT* = value</br>
Diese Zeile legt die Mindestmenge der Ressourcen fest, die pro Anforderung zugeordnet werden. Hierbei ist *value* ein erforderlicher Parameter mit einem Gleitkommawert zwischen 0.75 und 100.00. Der Wert für request_min_resource_grant_percent muss ein Vielfaches von 0,25, ein Faktor von min_percentage_resource und weniger als cap_percentage_resource sein. Es gibt effektive Mindestwerte, die pro Dienstebene zulässig sind. Weitere Informationen finden Sie unter [Effektive Werte](#effective-values).

Beispiel:

```sql
CREATE WORKLOAD GROUP wgSample 
WITH
  ( MIN_PERCENTAGE_RESOURCE = 26                -- integer value
    , REQUEST_MIN_RESOURCE_GRANT_PERCENT = 3.25 -- factor of 26 (guaranteed a minimum of 8 concurrency)
    , CAP_PERCENTAGE_RESOURCE = 100 )
```

Sehen Sie sich die Werte an, die für die Ressourcenklassen als Richtlinie für request_min_resource_grant_percent verwendet werden.  Die folgende Tabelle enthält die Ressourcenzuordnung für Gen2.

|Ressourcenklasse|Ressourcen in Prozent|
|---|---|
|Smallrc|3 %|
|Mediumrc|10 %|
|Largerc|22 %|
|Xlargerc|70 %|
|||

*REQUEST_MAX_RESOURCE_GRANT_PERCENT* = value</br>         
Diese Zeile legt die Maximalmenge der Ressourcen fest, die pro Anforderung zugeordnet werden. Dabei ist *value* ein optionaler Dezimalparameter mit einem Standardwert, der „request_min_resource_grant_percent“ entspricht. Der Wert von *value* muss größer oder gleich „request_min_resource_grant_percent“ sein. Wenn der Wert von request_max_resource_grant_percent größer als request_min_resource_grant_percent ist und Systemressourcen verfügbar sind, werden einer Anforderung zusätzliche Ressourcen zugeordnet.

*IMPORTANCE* = {LOW \| BELOW_NORMAL \| NORMAL \| ABOVE_NORMAL \| HIGH}</br>        
Diese Zeile gibt die Standardwichtigkeit einer Anforderung für die Arbeitsauslastungsgruppe an. Für die Wichtigkeit sind folgende Einstellungen möglich, wobei NORMAL die Standardeinstellung ist:

- LOW
- BELOW_NORMAL
- NORMAL (Standard)
- ABOVE_NORMAL
- HIGH

Die Wichtigkeit, die für eine Arbeitsauslastungsgruppe festgelegt wird, entspricht der Standardwichtigkeit für alle Anforderungen in der Arbeitsauslastungsgruppe. Benutzer können die Wichtigkeit auch auf Klassifizierungsebene festlegen und somit die festgelegte Wichtigkeit der Arbeitsauslastungsgruppe überschreiben. Dadurch wird eine Differenzierung der Wichtigkeit für Anforderungen innerhalb einer Arbeitsauslastungsgruppe ermöglicht, um schneller auf nicht reservierte Ressourcen zugreifen zu können. Wenn die Summe von min_percentage_resource in den Arbeitsauslastungsgruppen weniger als 100 ist, sind nicht reservierte Ressourcen vorhanden, die nach Wichtigkeit zugewiesen werden.

*QUERY_EXECUTION_TIMEOUT_SEC* = value</br>     
Diese Zeile gibt die maximale Zeit in Sekunden an, für deren Dauer eine Abfrage ausgeführt werden kann, bevor sie abgebrochen wird. *value* muss 0 (null) oder ein positiver Integer sein. Die Standardeinstellung für value ist 0 (null). Das bedeutet, dass für die Abfrage kein Timeout eintritt. QUERY_EXECUTION_TIMEOUT_SEC zählt, sobald die Abfrage als ausgeführt erkannt wird, nicht jedoch, wenn die Abfrage in die Warteschlange eingereiht wird.

## <a name="remarks"></a>Bemerkungen

Die entsprechenden Arbeitsauslastungsgruppen für Ressourcenklassen werden aufgrund der Abwärtskompatibilität automatisch generiert. Diese vom System definierten Arbeitsauslastungsgruppen können nicht gelöscht werden. Es können acht zusätzliche benutzerdefinierte Arbeitsauslastungsgruppen erstellt werden.

Wenn eine Arbeitsauslastungsgruppe mit `min_percentage_resource` größer als 0 (null) erstellt wird, wird die `CREATE WORKLOAD GROUP`-Anweisung in die Warteschlange eingereiht, bis genügend Ressourcen zum Erstellen der Arbeitsauslastungsgruppe vorhanden sind.

## <a name="effective-values"></a>Effektive Werte

Die Parameter `min_percentage_resource`, `cap_percentage_resource`, `request_min_resource_grant_percent` und `request_max_resource_grant_percent` haben effektive Werte, die basierend auf dem aktuellen Servicelevel und der Konfiguration anderer Arbeitsauslastungsgruppen angepasst werden.

Der `request_min_resource_grant_percent`-Parameter hat einen effektiven Wert, da je nach Servicelevel minimale Ressourcen pro Abfrage benötigt werden.  Beispielsweise sind auf dem niedrigsten Servicelevel (DW100c) mindestens 25 % der Ressourcen pro Anforderung erforderlich.  Wenn die Workloadgruppe mit 3 % `request_min_resource_grant_percent` und `request_max_resource_grant_percent` konfiguriert ist, passen sich die effektiven Werte für beide Parameter auf 25 % an, wenn die Instanz gestartet wird.  Wenn die Instanz zu DW1000c hochskaliert wird, sind die konfigurierten und effektiven Werte für beide Parameter 3 %, da 3 % der unterstützte Mindestwert für diesen Servicelevel ist.  Wenn die Instanz höher als DW1000c skaliert wird, bleiben die konfigurierten und effektiven Werte für beide Parameter bei 3 %.  In der folgenden Tabelle finden Sie weitere Informationen zu effektiven Werten bei verschiedenen Servicelevels.

|Dienstebene|Niedrigster effektiver Wert für REQUEST_MIN_RESOURCE_GRANT_PERCENT|Maximale Anzahl gleichzeitiger Abfragen|
|---|---|---|
|DW100c|25 %|4|
|DW200c|12,5 %|8|
|DW300c|8 %|12|
|DW400c|6,25 %|16|
|DW500c|5 %|20|
|DW1000c|3 %|32|
|DW1500c|3 %|32|
|DW2000c|2 %|48|
|DW2500c|2 %|48|
|DW3000c|1,5 %|64|
|DW5000c|1,5 %|64|
|DW6000c|0,75 %|128|
|DW7500c|0,75 %|128|
|DW10000c|0,75 %|128|
|DW15000c|0,75 %|128|
|DW30000c|0,75 %|128|
||||

Der `min_percentage_resource`-Parameter muss größer oder gleich dem effektiven `request_min_resource_grant_percent` sein. Bei einer Workloadgruppe, deren Wert für `min_percentage_resource` kleiner als der effektive Wert von `min_percentage_resource` ist, wird der Wert zur Laufzeit auf 0 (null) angepasst. In diesem Fall sind die für `min_percentage_resource` konfigurierten Ressourcen für alle Arbeitsauslastungsgruppen freigegeben. Beispielsweise hätte die Workloadgruppe `wgAdHoc` mit einem Wert für `min_percentage_resource` von 10 % unter DW1000c einen effektiven Wert für `min_percentage_resource` von 10 % (3 % ist der Mindestwert bei DW1000c). Bei DW100c hätte `wgAdhoc` einen effektiven Wert für min_percentage_resource von 0 %. Die für `wgAdhoc` konfigurierten 10 % würden für alle Arbeitsauslastungsgruppen freigegeben werden.

Der Parameter `cap_percentage_resource` hat ebenfalls einen effektiven Wert. Wenn eine Arbeitsauslastungsgruppe `wgAdhoc` mit einem Wert von 100 % für `cap_percentage_resource` konfiguriert wird und eine andere Arbeitsauslastungsgruppe `wgDashboards` mit einem Wert von 25 % für `min_percentage_resource`, wird der effektive Wert von `cap_percentage_resource` für `wgAdhoc` 75 %.

Die Laufzeitwerte für Ihre Arbeitsauslastungsgruppe lassen sich am einfachsten erklären, indem Sie die Systemsicht [sys.dm_workload_management_workload_groups_stats](../../relational-databases/system-dynamic-management-views/sys-dm-workload-management-workload-group-stats-transact-sql.md) abfragen.

## <a name="permissions"></a>Berechtigungen

Erfordert die `CONTROL DATABASE`-Berechtigung

## <a name="see-also"></a>Weitere Informationen

- [DROP WORKLOAD GROUP &#40;Transact-SQL&#41;](drop-workload-group-transact-sql.md)
- [ALTER WORKLOAD GROUP &#40;Transact-SQL&#41;](alter-workload-group-transact-sql.md)
- [sys.workload_management_workload_groups](../../relational-databases/system-catalog-views/sys-workload-management-workload-groups-transact-sql.md)
- [sys.dm_workload_management_workload_groups_stats](../../relational-databases/system-dynamic-management-views/sys-dm-workload-management-workload-group-stats-transact-sql.md)
- [Schnellstart: Konfigurieren der Arbeitsauslastungsisolation mithilfe von T-SQL](/azure/sql-data-warehouse/quickstart-configure-workload-isolation-tsql)

::: moniker-end
