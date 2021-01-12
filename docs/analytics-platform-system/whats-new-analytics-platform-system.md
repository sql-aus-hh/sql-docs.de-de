---
title: Neues
description: Sehen Sie sich die Neuerungen in Microsoft Analytics Platform System an, eine lokale Appliance für horizontales Skalieren, die MPP SQL Server parallel Data Warehouse hostet.
author: mzaman1
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: murshedz
ms.reviewer: martinle
ms.custom: seo-dt-2019
ms.openlocfilehash: 004550d95ca69bae45c518b8054886e635e829df
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091783"
---
# <a name="whats-new-in-analytics-platform-system-a-scale-out-mpp-data-warehouse"></a>Neues in Analytics Platform System, ein Data Warehouse für horizontales Skalieren
Weitere Informationen finden Sie unter What es New in the latest Appliance Updates for Microsoft Analytics Platform System (APS). APS ist eine lokale Appliance für horizontales Skalieren, die MPP SQL Server parallele Data Warehouse hostet. 

::: moniker range=">= aps-pdw-2016-au7 "
<a name="h2-aps-cu7.7"></a>
## <a name="aps-cu77"></a>APS CU7.7
Veröffentlichungsdatum-2020

### <a name="scvmm2016"></a>SCVMM2016
APS Cu 7.7 Software aktualisiert VMM-VM auf Windows Server 2016 und installiert SCVMM2016. SCVMM 2012 R2, das zurzeit verwendet wird, hat das Ende des Lebenszyklus von Juli 2022. Das neuere SCVMM muss unterstützt werden, um Cu 7.7 zu einem obligatorischen Upgrade zu führen. Kunden müssen so bald wie möglich ein Upgrade auf Cu 7.7 durchführen.

### <a name="ssis-destination-adapter-for-sql-server-2019-as-target"></a>SSIS-Ziel Adapter für SQL Server 2019 als Ziel
Der neue APS SSIS-Ziel Adapter, der SQL Server 2019 als Bereitstellungs Ziel unterstützt, kann von der [Download Website](https://www.microsoft.com/download/details.aspx?id=57472)heruntergeladen werden.

<a name="h2-aps-cu7.6"></a>
## <a name="aps-cu76"></a>APS CU7.6
Veröffentlichungsdatum-2020

### <a name="rename-column"></a>Rename Column (Spalte umbenennen)
Nach dem Upgrade auf Cu 7.6 können Kunden eine Spalte einer vom Benutzer erstellten Tabelle umbenennen. Syntax, Beispiele, Einschränkungen und weitere Informationen finden Sie unter [Rename (Transact-SQL)](../t-sql/statements/rename-transact-sql.md) .

### <a name="alter-view"></a>Alter View
Kunden können nun Ansichten ändern. Weitere Informationen finden Sie unter [Alter View (Transact-SQL)](../t-sql/statements/alter-view-transact-sql.md) .

<a name="h2-aps-cu7.5"></a>
## <a name="aps-cu75"></a>APS CU7.5
Veröffentlichungsdatum-September 2019

### <a name="alter-external-data-source"></a>Externe Datenquelle ändern
Kunden können die Definition externer Datenquellen mit dem Cu 7.5-Update ändern. Kunden mit hoher Verfügbarkeit von Hadoop-namens Knoten können nun die Datenquelle ändern, um die Argumente zu ändern, wenn ein Failover auftritt. Für APS können nur der Speicherort, RESOURCE_MANAGER_LOCATION und die Anmelde Informationen geändert werden. Weitere Informationen finden Sie unter [Alter extern Data Source](../t-sql/statements/alter-external-data-source-transact-sql.md?view=sql-server-2017&preserve-view=true) .

### <a name="cdh-515-and-516-support-with-polybase"></a>CDH 5,15-und 5,16-Unterstützung mit polybase
Polybase on APS mit Cu 7.5 Update unterstützt jetzt CDH 5,15-und 5,16-Versionen der Hadoop-Distribution von cloudera. Verwenden Sie Option 6 für CDH 5. x-Versionen. 

### <a name="try_convert-and-try_cast-support"></a>Unterstützung für Try_Convert und Try_Cast
Cu 7.5 APS unterstützt jetzt die Funktionen [TRY_CAST](../t-sql/functions/try-cast-transact-sql.md?view=sql-server-2017&preserve-view=true) und [TRY_CONVERT](../t-sql/functions/try-convert-transact-sql.md?view=sql-server-2017&preserve-view=true) . Beide Funktionen geben einen Wert zurück, der in den angegebenen Datentyp konvertiert wird, wenn die Konvertierung erfolgreich ist. Andernfalls wird NULL zurückgegeben.

<a name="h2-aps-cu7.4"></a>
## <a name="aps-cu74"></a>APS CU7.4
Veröffentlichungsdatum-Mai 2019

### <a name="loading-large-rows-with-dwloader"></a>Laden von großen Zeilen mit "dwloader
Ab APS Cu 7.4 können Kunden mit einem neuen "dwloader Zeilen in Tabellen laden, die größer als 32 KB (32.768 Bytes) sind. Das neue "dwloader unterstützt den Schalter-l, der einen ganzzahligen Wert zwischen 32768 und 33554432 (in Bytes) einnimmt, um Zeilen zu laden, die größer sind als 32 KB. Verwenden Sie diese Option nur, wenn Sie große Zeilen laden (mehr als 32 KB), da dieser Switch mehr Arbeitsspeicher auf dem Client und dem Server zuweist und die Auslastung möglicherweise verlangsamt. Sie können das neue "dwloader von der [Download Website](https://www.microsoft.com/download/details.aspx?id=57472)herunterladen.  

### <a name="hdp-30-and-31-support-with-polybase"></a>Unterstützung von HDP 3,0 und 3,1 mit polybase
Polybase on APS unterstützt jetzt HDP 3,0 und 3,1 mit diesem Update. Verwenden Sie Option 7 für HDP 3. x-Versionen. Weitere Informationen finden Sie auf der Seite [polybase-Konnektivität](../database-engine/configure-windows/polybase-connectivity-configuration-transact-sql.md) .

### <a name="utf16-file-support-with-polybase"></a>Unterstützung von UTF16-Dateien mit polybase
Polybase unterstützt jetzt das Lesen von durch Trennzeichen getrennten Textdateien, die in der UTF16-Codierung (Le) vorliegen Setup Details finden Sie unter [Erstellen eines externen Datei Formats](../t-sql/statements/create-external-file-format-transact-sql.md) . 

<a name="h2-aps-cu7.3"></a>
## <a name="aps-cu73"></a>APS CU7.3
Veröffentlichungsdatum-Dezember 2018

### <a name="common-subexpression-elimination"></a>Entfernen gemeinsamer Teilausdrücke
APS Cu 7.3 verbessert die Abfrageleistung mit allgemeiner Teil Ausdrucks Löschung im SQL-Abfrageoptimierer. Die Verbesserung verbessert Abfragen auf zwei Arten. Der erste Vorteil ist die Möglichkeit, solche Ausdrücke zu identifizieren und zu entfernen, um die SQL-Kompilierungszeit zu verringern. Der zweite und wichtigere Vorteil besteht darin, dass Daten Verschiebungs Vorgänge für diese redundanten Teil Ausdrücke eliminiert werden, sodass die Ausführungszeit für Abfragen schneller wird. Ausführliche Erläuterungen zu diesem Feature finden Sie [hier](common-sub-expression-elimination.md).

### <a name="aps-informatica-connector-for-informatica-1020-published"></a>APS Informatica-Connector für Informatica 10.2.0 veröffentlicht
Wir haben eine neue Version von Informatica-Connectors für APS veröffentlicht, die mit Informatica Version 10.2.0 und 10.2.0 Hotfix 1 zusammenarbeitet. Die neuen Connectors können von der [Download Website](https://www.microsoft.com/download/details.aspx?id=57472)heruntergeladen werden.
> [!NOTE]
> Der APS Informatica-Connector für Informatica 10.2.0 oder 10.2.0 Hotfix 1 funktioniert nicht bei Strict TLS 1.2 und erfordert, dass TLS 1.0 und 1,1 voll funktionsfähig sind.

#### <a name="supported-versions"></a>Unterstützte Versionen

| APS-Version | Informatica PowerCenter | Treiber |
|:---|:---|:---|
| APS 2016 | 9.6.1 | SQL Server Native Client 11. x |
| APS 2016 und höher | 10.2.0, 10.2.0 Hotfix 1 | SQL Server Native Client 11. x |

<a name="h2-aps-cu7.2"></a>
## <a name="aps-cu72"></a>APS CU7.2
Veröffentlichungsdatum-Oktober 2018

### <a name="support-for-tls-12"></a>Unterstützung für TLS 1.2
APS Cu 7.2 unterstützt TLS 1,2. Die Kommunikation zwischen Client Computern und APS und der Kommunikation zwischen Knoten kann jetzt so festgelegt werden, dass Sie nur über TLS 1.2 kommuniziert. Tools wie SSDT, SSIS und dwloader, die auf Client Computern installiert sind, die nur für die Kommunikation über TLS 1,2 festgelegt sind, können jetzt mithilfe von TLS 1,2 eine Verbindung mit APS herstellen. Standardmäßig werden von APS alle TLS-Versionen (1,0, 1,1 und 1,2) für die Abwärtskompatibilität unterstützt. Wenn Sie festlegen möchten, dass ihr APS-Gerät ausschließlich TLS 1,2 verwendet, können Sie die Registrierungs Einstellungen ändern. 

Weitere Informationen finden Sie unter [Konfigurieren von TLS 1.2 auf APS](configure-tls12-aps.md).

### <a name="hadoop-encryption-zone-support-for-polybase"></a>Unterstützung von Hadoop-Verschlüsselungs Zonen für polybase
Polybase kann jetzt mit Hadoop-Verschlüsselungs Zonen kommunizieren. Siehe APS-Konfigurationsänderungen, die zum [Konfigurieren der Hadoop-Sicherheit](polybase-configure-hadoop-security.md#encryptionzone)erforderlich sind.

### <a name="insert-select-maxdop-options"></a>Insert-Select MAXDOP-Optionen
Wir haben einen [Funktions Schalter](appliance-feature-switch.md) hinzugefügt, mit dem Sie MAXDOP-Einstellungen für INSERT-SELECT-Vorgänge über 1 auswählen können. Sie können jetzt die MAXDOP-Einstellung auf 0, 1, 2 oder 4 festlegen. Der Standardwert ist 1.

> [!IMPORTANT]  
> Das erhöhen von MAXDOP kann manchmal zu langsameren Vorgängen oder Deadlockfehlern führen. Wenn dies der Fall ist, ändern Sie die Einstellung wieder in MAXDOP 1, und wiederholen Sie den Vorgang.

### <a name="columnstore-index-health-dmv"></a>Integritäts-DMV für columnstore-Index
Integritäts Informationen zum columnstore--Index können Sie mithilfe **dm_pdw_nodes_db_column_store_row_group_physical_stats** DMV anzeigen. Verwenden Sie die folgende Ansicht, um die Fragmentierung zu bestimmen und zu entscheiden, wann ein columnstore--Index neu erstellt oder neu organisiert wird.

```sql
create view dbo.vCS_rg_physical_stats
as 
with cte
as
(
select   tb.[name]                    AS [logical_table_name]
,        rg.[row_group_id]            AS [row_group_id]
,        rg.[state]                   AS [state]
,        rg.[state_desc]              AS [state_desc]
,        rg.[total_rows]              AS [total_rows]
,        rg.[trim_reason_desc]        AS trim_reason_desc
,        mp.[physical_name]           AS physical_name
FROM    sys.[schemas] sm
JOIN    sys.[tables] tb               ON  sm.[schema_id]          = tb.[schema_id]                             
JOIN    sys.[pdw_table_mappings] mp   ON  tb.[object_id]          = mp.[object_id]
JOIN    sys.[pdw_nodes_tables] nt     ON  nt.[name]               = mp.[physical_name]
JOIN    sys.[dm_pdw_nodes_db_column_store_row_group_physical_stats] rg      ON  rg.[object_id]     = nt.[object_id]
                                                                            AND rg.[pdw_node_id]   = nt.[pdw_node_id]
                                        AND rg.[pdw_node_id]    = nt.[pdw_node_id]                                          
)
select *
from cte;
```

### <a name="polybase-date-range-increase-for-orc-and-parquet-files"></a>Zunahme des polybase-Datums Bereichs für Orc-und Parkett Dateien
Das Lesen, importieren und Exportieren von Datums Datentypen mit polybase unterstützt jetzt Datumsangaben vor 1970-01-01 und 2038-01-20 für Orc-und Parkett Dateitypen.

### <a name="ssis-destination-adapter-for-sql-server-2017-as-target"></a>SSIS-Ziel Adapter für SQL Server 2017 als Ziel
Der neue APS SSIS-Ziel Adapter, der SQL Server 2017 als Bereitstellungs Ziel unterstützt, kann von der [Download Website](https://www.microsoft.com/download/details.aspx?id=57472)heruntergeladen werden.

<a name="h2-aps-cu7.1"></a>
## <a name="aps-cu71"></a>APS CU7.1
Veröffentlichungsdatum-2018

### <a name="dbcc-commands-do-not-consume-concurrency-slots-behavior-change"></a>DBCC-Befehle verbrauchen keine Parallelitäts Slots (Behavior Change)
APS unterstützt eine Teilmenge der T-SQL- [DBCC-Befehle](../t-sql/database-console-commands/dbcc-transact-sql.md) wie z. b. [DBCC DROPCLEANBUFFERS](../t-sql/database-console-commands/dbcc-dropcleanbuffers-transact-sql.md). Bis jetzt haben diese Befehle einen [Parallelitätsslot](./workload-management.md?view=aps-pdw-2016-au7&preserve-view=true&#concurrency-slots) belegt, wodurch sich die Zahl der ausführbaren Benutzerauslastungen/Abfragen verringert hat. Die `DBCC` Befehle werden nun in einer lokalen Warteschlange ausgeführt, die keinen benutzerparallelitäts Slot verbraucht, um die Gesamtleistung der Abfrage Ausführung zu verbessern.

### <a name="replaces-some-metadata-calls-with-catalog-objects"></a>Ersetzt einige Metadatenaufrufe durch Katalog Objekte.
Durch die Verwendung von Katalog Objekten für Metadatenaufrufe anstelle von SMO wurde in APS eine Leistungsverbesserung angezeigt. Ab Cu 7.1 verwenden einige dieser Metadatenaufrufe jetzt standardmäßig Katalog Objekte. Dieses Verhalten kann durch [Funktions Wechsel](appliance-feature-switch.md) deaktiviert werden, wenn Kunden, die Metadatenabfragen verwenden, Probleme haben.

### <a name="bug-fixes"></a>Behebung von Programmfehlern
Wir haben ein Upgrade auf SQL Server 2016 SP2 Cu2 mit APS Cu 7.1 durchgeführt. Das Upgrade korrigiert einige Probleme, die unten beschrieben werden.

| Titel | Beschreibung |
|:---|:---|
| **Möglicher Deadlock von tupelbewegsvorgang** |Das Upgrade korrigiert eine langfristige Möglichkeit eines Deadlocks in einer verteilten Transaktion und einem tupelverschiebungshintergrundthread. Nach der Installation von Cu 7.1 können Kunden, die TF634 verwendet haben, um den tupelverschiebungsvorgang als SQL Server Startparameter oder globales Ablaufverfolgungsflag zu unterbinden | 
| **Bestimmte Verzögerung/führende Abfrage schlägt fehl** |Bestimmte Abfragen in CCI-Tabellen mit in der Fehlermeldung gedebuggten Funktionen mit verzögertem verzögertem Rückstand | 


<a name="h2-aps-au7"></a>
## <a name="aps-au7"></a>APS AU7
Veröffentlichungsdatum-Mai 2018

APS 2016 ist eine Voraussetzung für ein Upgrade auf AU7. Im folgenden finden Sie neue Features in APS AU7:

### <a name="auto-create-and-auto-update-statistics"></a>Statistiken automatisch erstellen und automatisch aktualisieren
APS AU7 standardmäßig werden Statistiken automatisch erstellt und aktualisiert. Zum Aktualisieren der Statistik Einstellungen können Administratoren ein neues Menü Element für Funktions Switches in der [Configuration Manager](appliance-configuration.md#CMTasks)verwenden. Der [Funktions Wechsel](appliance-feature-switch.md) steuert das automatische erstellen, das automatische Aktualisieren und das asynchrone Update Verhalten von Statistiken. Sie können auch Statistik Einstellungen mit der Anweisung [ALTER DATABASE (Parallel Data Warehouse)](../t-sql/statements/alter-database-transact-sql.md?tabs=sqlpdw) aktualisieren.

### <a name="t-sql"></a>T-SQL
Select @var wird jetzt unterstützt. Weitere Informationen finden Sie unter [Select Local Variable](../t-sql/language-elements/select-local-variable-transact-sql.md) . 

Die Abfrage Hinweise Hash und Order Group werden jetzt unterstützt. Weitere Informationen finden Sie unter [Hinweise (Transact-SQL)-Query](../t-sql/queries/hints-transact-sql-query.md)

### <a name="feature-switch"></a>Funktions Wechsel
APS AU7 führt einen Funktions Wechsel in [Configuration Manager](launch-the-configuration-manager.md)ein. Autostatus Abled und dmsprocessstopmessagetimeoutinseconds sind nun konfigurierbare Optionen, die von Administratoren geändert werden können.

### <a name="known-issues"></a>Bekannte Probleme
Mit APS AU7 Software wird ein Intel BIOS-Update bereitgestellt, das ein Problem behebt, das als *spekulativer Ausführungs Seitenkanal-Angriffe* beschrieben wird. Die Angriffe zielen darauf ab, was als *Spectre-und Meltdown-Sicherheits* Risiken bezeichnet werden. Obwohl das BIOS-Update zusammen mit APS verpackt ist, wird es manuell installiert und nicht als Teil der APS AU7 Software Installation.

Microsoft rät allen Kunden, das aktualisierte BIOS zu installieren. Microsoft hat die Auswirkungen von Kernel-VMS (Virtual Address Shadowings, Kvas), Kernel Page Table deretion (kpti) und indirekter Verzweigungs Entschärfung (IBP) für verschiedene SQL-Arbeits Auslastungen in verschiedenen Umgebungen gemessen. Die Messungen haben bei einigen Arbeits Auslastungen eine erhebliche Beeinträchtigung festgestellt. Basierend auf den Ergebnissen wird empfohlen, dass Sie die Leistungs Auswirkung der Aktivierung von BIOS-Updates testen, bevor Sie Sie in einer Produktionsumgebung bereitstellen. Weitere [Informationen finden Sie unter SQL Server Anleitung.](https://support.microsoft.com/help/4073225/guidance-protect-sql-server-against-spectre-meltdown)

::: moniker-end
::: moniker range=">= aps-pdw-2016 "
<a name="h2-aps-au6"></a>
## <a name="aps-2016"></a>APS 2016
In diesem Abschnitt wurden die neuen Features für APS 2016-AU6 beschrieben.

### <a name="sql-server-2016"></a>SQL Server 2016

APS AU6 wird auf der neuesten Version von SQL Server 2016 ausgeführt und verwendet den Standard Kompatibilitäts Grad der Datenbank 130. SQL Server 2016 aktiviert die Unterstützung für neue Features wie z. b.:

- Sekundäre Indizes für gruppierte columnstore--Indizes.
- Kerberos für polybase.

### <a name="t-sql"></a>T-SQL
APS AU6 unterstützt diese Verbesserungen der T-SQL-Kompatibilität.  Diese zusätzlichen Sprachelemente vereinfachen die Migration von SQL Server und anderen Datenquellen. 

- [SQL-Sortierungen auf Spaltenebene][] werden nun zusätzlich zu Windows-Sortierungen unterstützt.
- [Nicht gruppierte Indizes für gruppierte columnstore--Indizes][] verbessern die Leistung von Abfragen, die im gruppierten columnstore--Index nach bestimmten Werten suchen. 
- [Wählen Sie... In][] 
- [sp_spaceused ()][] zeigt den Speicherplatz an, der in einer Tabelle oder Datenbank verwendet oder reserviert wurde.
- Die Unterstützung von [breiten Tabellen][] ist identisch mit SQL Server 2016. Das vorherige Limit von 32 K für die Zeilengröße ist nicht mehr vorhanden. 

**Datentypen**

- [Varchar (max)][], [nvarchar (max)][] und [varbinary (max)][]. Diese LOB-Datentypen haben eine maximale Größe von 2 GB. Um diese Objekte zu laden, verwenden Sie das [Hilfsprogramm bcp][]. Polybase und "dwloader unterstützen diese Datentypen zurzeit nicht. 
- [Vom Datentyp sysname][]
- [UNIQUEIDENTIFIER][]
- [Numerische][] und Dezimal Datentypen.

**Fensterfunktionen**

- [Zeilen oder Bereich][] in der OVER-Klausel der SELECT-Anweisung.
- [FIRST_VALUE][]
- [LAST_VALUE][]
- [CUME_DIST][]
- [PERCENT_RANK][]

**Sicherheitsfunktionen**

- [CHECKSUM ()][] und [BINARY_CHECKSUM ()][]
- [HAS_PERMS_BY_NAME ()][]

**Zusätzliche Funktionen**

- ["Nwid ()"][]
- [Rand ()][]

### <a name="polybasehadoop-enhancements"></a>Polybase-/Hadoop-Erweiterungen

- Kompatibilität mit hortonworks HDP 2,4 und HDP 2,5
- Kerberos-Unterstützung über Daten Bank weit gültige Anmelde Informationen
- Unterstützung von Anmelde Informationen mit Azure Storage-BLOB

### <a name="install-and-upgrade-enhancements"></a>Verbesserungen bei Installation und Upgrade

**Updates der Unternehmensarchitektur** Aktualisieren Ihrer vorhandenen Appliance auf APS AU6 installiert die neuesten Firmware-und Treiber Updates, einschließlich Sicherheitskorrekturen. 

Ein neues Gerät von HPE oder Dell enthält alle neuesten Updates und bietet folgende Informationen:

- Prozessorunterstützung der neuesten Generation (Broadwell)
- Aktualisieren auf DDR4 DIMMs
- Verbesserter DIMM-Durchsatz

**Integration**

- Der voll qualifizierte Domänen Name (Fully Qualified Domain Name, FQDN) ermöglicht das Einrichten einer Domänen Vertrauensstellung für das Gerät. 
- Um FQDN verwenden zu können, müssen Sie während des Upgrades ein vollständiges Upgrade und ein Upgrade durchführen. 

**Reduzierte Ausfallzeiten** Das Installieren oder Aktualisieren auf APS AU6 ist schneller und erfordert weniger Ausfallzeiten als vorherige Versionen. Um Ausfallzeiten zu reduzieren, installieren oder aktualisieren Sie Folgendes: 

 - Optimiert das Anwenden von WSUS-Updates mithilfe eines Images, das alle Updates bis Juni 2016 enthält.
 - Anwenden von Sicherheitsupdates mit den Treiber-und Firmwareupdates
 - Platziert die neuesten Hotfixes und das geräterüberprüfungs-Hilfsprogramm (Appliance Verification Utility, PV) auf Ihrer Appliance, damit Sie installiert werden können, sodass Sie nicht heruntergeladen werden müssen.

::: moniker-end

<!--
Link references to other articles in this same GitHub repo.

The link format that starts with '/sql/what-ever/my-artlcle' is not appropriate for common links within the same repo (as most of these link are).  The first couple links have been edited to show the proper syntax, but all other links in this article need to be similarly edited.
The proper formats have at least two big advantages.  One big advantage is that the proper formats enable the OPS Build system to detect broken links at Pull Request build time, instead of only later during run time.
-->
[database compatibility level 130]: ../t-sql/statements/alter-database-transact-sql-compatibility-level.md
[SQL-Sortierungen auf Spaltenebene]: ~/relational-databases/collations/collation-and-unicode-support.md

[Nicht gruppierte Indizes für gruppierte columnstore--Indizes]:/sql/t-sql/statements/create-index-transact-sql
[varchar (max)]:/sql/t-sql/data-types/char-and-varchar-transact-sql
[nvarchar (max)]:/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql
[varbinary (max)]:/sql/t-sql/data-types/binary-and-varbinary-transact-sql
[Vom Datentyp sysname]:/sql/relational-databases/system-catalog-views/sys-types-transact-sql
[Wählen Sie... In]:/sql/t-sql/queries/select-into-clause-transact-sql
[sp_spaceused ()]:/sql/relational-databases/system-stored-procedures/sp-spaceused-transact-sql
[Breite Tabellen]:/sql/sql-server/maximum-capacity-specifications-for-sql-server
[BULK INSERT]:/sql/t-sql/statements/bulk-insert-transact-sql
[bcp (Hilfsprogramm)]:/sql/tools/bcp-utility
[UNIQUEIDENTIFIER]:/sql/t-sql/data-types/uniqueidentifier-transact-sql
[NUMERIC]:/sql/t-sql/data-types/decimal-and-numeric-transact-sql
[Zeilen oder Bereich]:/sql/t-sql/queries/select-over-clause-transact-sql
[FIRST_VALUE]:/sql/t-sql/functions/first-value-transact-sql
[LAST_VALUE]:/sql/t-sql/functions/last-value-transact-sql
[CUME_DIST]:/sql/t-sql/functions/cume-dist-transact-sql
[PERCENT_RANK]:/sql/t-sql/functions/percent-rank-transact-sql
[Prüfsumme ()]:/sql/t-sql/functions/checksum-transact-sql
[BINARY_CHECKSUM ()]:/sql/t-sql/functions/binary-checksum-transact-sql
[HAS_PERMS_BY_NAME ()]:/sql/t-sql/functions/has-perms-by-name-transact-sql
["Nwid ()"]:/sql/t-sql/functions/newid-transact-sql
[Rand ()]:/sql/t-sql/functions/rand-transact-sql


  

