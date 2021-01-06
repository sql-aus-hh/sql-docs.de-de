---
title: Konfigurationsoptionen für den Serverarbeitsspeicher | Microsoft-Dokumentation
ms.custom: contperf-fy20q4
ms.date: 08/14/2019
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
description: Hier erfahren Sie, wie Sie die Arbeitsspeichermenge konfigurieren, die SQL Server-Prozessen vom Speicher-Manager von SQL Server zugewiesen wird. Sehen Sie sich Speicherverwaltungsansätze und -beispiele an.
helpviewer_keywords:
- Virtual Memory Manager
- max server memory option
- virtual memory [SQL Server]
- VMM
- server memory options [SQL Server]
- servers [SQL Server], memory
- buffer pools [SQL Server]
- min server memory option
- manual memory options [SQL Server]
- memory [SQL Server], servers
ms.assetid: 29ce373e-18f8-46ff-aea6-15bbb10fb9c2
author: pmasl
ms.author: pelopes
ms.openlocfilehash: 71cab9c11aaa4f3dc569761fdfef407601162b1c
ms.sourcegitcommit: cb8e2ce950d8199470ff1259c9430f0560f0dc1d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/05/2021
ms.locfileid: "97878907"
---
# <a name="server-memory-configuration-options"></a>Konfigurationsoptionen für den Serverarbeitsspeicher
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Konfigurieren Sie die von einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verwendete Arbeitsspeichermenge (in Megabyte) für einen SQL Server-Prozess neu.  Es gibt die beiden Serverarbeitsspeicheroptionen **min server memory** und **max server memory**. Durch diese Optionen wird die Arbeitsspeichermenge geändert, die SQL Server-Prozessen vom Speicher-Manager von SQL Server zugewiesen wird.
  
Die Standardeinstellungen und die minimal zulässigen Werte für diese Optionen sind folgende:

|Option  |  Standard | Minimal zulässig  |
|---------|---------|---------|
|**Min. Serverarbeitsspeicher**     |    0     |    0     |
|**Max. Serverarbeitsspeicher**     |     2\.147.483.647 Megabytes (MB).     |  128 MB       |

Standardmäßig können die Arbeitsspeicheranforderungen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] anhand der verfügbaren Systemressourcen dynamisch geändert werden. Weitere Informationen finden Sie unter [Dynamische Arbeitsspeicherverwaltung](../../relational-databases/memory-management-architecture-guide.md#dynamic-memory-management).

> [!IMPORTANT]  
> Die Festlegung von **max server memory** auf einen zu hohen Wert kann dazu führen, dass eine einzelne Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] möglicherweise mit anderen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanzen um Arbeitsspeicher konkurrieren muss, die auf dem gleichen Host aufgeführt werden. Die Festlegung auf einen zu niedrigen Wert kann jedoch zu erheblichem Arbeitsspeichermangel und entsprechenden Leistungsproblemen führen. Das Festlegen von **Max. Serverarbeitsspeicher** auf den Minimalwert kann sogar den Start von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verhindern. Wenn [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nach dem Ändern dieser Option nicht gestartet werden kann, müssen Sie den Start mithilfe der Startoption **_-f_** durchführen und die Option **Max. Serverarbeitsspeicher** auf ihren vorherigen Wert zurücksetzen. Weitere Informationen finden Sie unter [Startoptionen für den Datenbank-Engine-Dienst](../../database-engine/configure-windows/database-engine-service-startup-options.md).  
    
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] kann Arbeitsspeicher dynamisch verwenden. Sie können die Speicheroptionen jedoch auch manuell festlegen und den Umfang des für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] zugreifbaren Arbeitsspeichers einschränken. Bevor Sie den Umfang des Arbeitsspeichers für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] festlegen, sollten Sie die geeignete Arbeitsspeichereinstellung ermitteln. Ziehen Sie dazu vom gesamten physischen Speicher den Arbeitsspeicher ab, der für das Betriebssystem, für nicht durch die Einstellung „max_server_memory“ gesteuerte Speicherbelegungen und für alle weiteren Instanzen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] erforderlich ist. (Falls der Computer nicht vollständig für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] reserviert ist, müssen Sie zusätzlich auch den für andere Verwendungen des Systems benötigten Arbeitsspeicher abziehen.) Die Differenz entspricht der maximalen Arbeitsspeichergröße, die Sie der aktuellen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz zuweisen können.  
 
## <a name="set-options-manually"></a><a name="manually"></a> Manuelles Festlegen der Optionen  
Sie können die Serveroptionen **Min. Serverarbeitsspeicher** und **Max. Serverarbeitsspeicher** so festlegen, dass ein großer Bereich von Arbeitsspeicherwerten überdeckt wird. Diese Methode ist vor allem dann sinnvoll, wenn der System- oder Datenbankadministrator eine Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in Abhängigkeit von den Arbeitsspeicheranforderungen anderer Anwendungen oder weiterer Instanzen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], die auf demselben Computer ausgeführt werden, konfigurieren möchte.

> [!NOTE]
> Bei **min server memory** und **max server memory** handelt es sich um erweiterte Optionen. Wenn Sie diese Einstellungen mithilfe der gespeicherten Systemprozedur **sp_configure** ändern, können Sie diese nur ändern, wenn **Erweiterte Optionen anzeigen** auf 1 festgelegt ist. Diese Einstellungen treten sofort ohne Neustart des Servers in Kraft.  
  
<a name="min_server_memory"></a> Mithilfe der Konfigurationsoption **min_server_memory** wird sichergestellt, dass für den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Speicher-Manager einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] eine Mindestmenge an Arbeitsspeicher verfügbar ist. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Allerdings wird die unter **Min. Serverarbeitsspeicher** angegebene Arbeitsspeichermenge von nicht gleich beim Start zugeordnet. Sobald der Wert für die Speicherauslastung aufgrund der Clientauslastung erreicht ist, kann [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nur dann Arbeitsspeicher freigeben, wenn der Wert für **Min. Serverarbeitsspeicher** reduziert wird. Wenn beispielsweise mehrere Instanzen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gleichzeitig auf dem gleichen Host ausgeführt werden können, legen Sie den Parameter „min_server_memory“ anstelle von „max_server_memory“ fest, um Arbeitsspeicher für eine Instanz zu reservieren. Ferner ist das Festlegen eines Werts für „min_server_memory“ in einer virtualisierten Umgebung entscheidend, um sicherzustellen, dass Arbeitsspeichermangel beim zugrundeliegenden Host nicht zu dem Versuch führt, Arbeitsspeicher aus dem Pufferpool eines virtuellen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Gastcomputers jenseits dessen abzuzweigen, was für eine vertretbare Leistung erforderlich ist.

>[!NOTE]
>Allerdings kann nicht sichergestellt werden, dass [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] die in **min server memory** angegebene Arbeitsspeichermenge zuordnet. Wenn die in **min server memory** angegebene Arbeitsspeichermenge aufgrund der Serverlast zu keinem Zeitpunkt zugeordnet werden muss, wird [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mit weniger Arbeitsspeicher ausgeführt.

<a name="max_server_memory"></a> Verwenden Sie **max_server_memory**, um sicherzustellen, dass beim Betriebssystem kein nachteiliger Arbeitsspeichermangel eintritt. Um den maximalen Serverarbeitsspeicher zu konfigurieren, überwachen Sie den Gesamtverbrauch des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Prozesses, um die Arbeitsspeicheranforderungen zu bestimmen. Hier folgen genauere Angaben für diese Berechnungen für eine Einzelinstanz:
- Reservieren Sie vom gesamten Arbeitsspeicher des Betriebssystems 1 GB–4 GB für das Betriebssystem selbst.
- Subtrahieren Sie anschließend das Äquivalent der potenziellen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Arbeitsspeicherbelegungen außerhalb des Steuerelements **Max. Serverarbeitsspeicher**, bestehend aus **Stapelgröße <sup>1</sup> \* max. Anzahl von berechneten Arbeitsthreads <sup>2</sup>** . Der Rest sollte die Einstellung „max_server_memory“ für die Einrichtung einer einzelnen Instanz bilden.

<sup>1</sup> Informationen zu den Threadstapelgrößen der einzelnen Architekturen finden Sie im [Handbuch zur Architektur der Speicherverwaltung](../../relational-databases/memory-management-architecture-guide.md#stacksizes).

<sup>2</sup> Informationen zu den standardmäßig berechneten Arbeitsthreads für eine bestimmte Anzahl kategorisierter CPUs auf dem aktuellen Host finden Sie auf der Dokumentationsseite zum [Konfigurieren der Serverkonfigurationsoption Maximale Anzahl von Arbeitsthreads](../../database-engine/configure-windows/configure-the-max-worker-threads-server-configuration-option.md).

## <a name="use-ssmanstudiofull"></a>Verwenden Sie [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]  
Mit den beiden Arbeitsspeicheroptionen für den Server, **Min. Serverarbeitsspeicher** und **Max. Serverarbeitsspeicher**, können Sie den vom [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Speicher-Manager für eine Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verwalteten Umfang des Arbeitsspeichers (in MB) umkonfigurieren. Standardmäßig können die Arbeitsspeicheranforderungen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] anhand der verfügbaren Systemressourcen dynamisch geändert werden.  
  
### <a name="procedure-for-configuring-a-fixed-amount-of-memory-not-recommended"></a>Vorgehensweise beim Konfigurieren eines festen Arbeitsspeichers (nicht empfohlen)  
So legen Sie eine feste Arbeitsspeichergröße fest:  
  
1.  Klicken Sie im Objekt-Explorer mit der rechten Maustaste auf einen Server, und wählen Sie **Eigenschaften** aus.  
  
2.  Klicken Sie auf den **Speicher** -Knoten.  
  
3.  Geben Sie unter **Arbeitsspeicheroptionen für den Server** den gewünschten Wert für **Minimaler Serverarbeitsspeicher** und **Maximaler Serverarbeitsspeicher** ein.  
  
     Verwenden Sie die Standardeinstellungen, damit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] die Arbeitsspeicheranforderungen auf der Grundlage der verfügbaren Systemressourcen dynamisch ändert. Es wird empfohlen, einen Wert für **Max. Serverarbeitsspeicher** festzulegen, wie [oben ausführlich beschrieben](#max_server_memory). 

Der folgende Screenshot zeigt alle drei Schritte: 

:::image type="content" source="media/server-memory-server-configuration-options/configure-memory-in-ssms.png" alt-text="Konfigurieren des Arbeitsspeichers in SSMS":::

  
## <a name="lock-pages-in-memory-lpim"></a>Sperren von Seiten im Speicher (LPIM) 
Mit dieser Windows-Richtlinie werden die Konten bestimmt, die einen Prozess zum Speichern von Daten im physischen Speicher verwenden können, um das systemgesteuerte Auslagern der Daten in den virtuellen Arbeitsspeicher zu vermeiden. Durch Sperren von Seiten im Arbeitsspeicher können Sie die Reaktionsfähigkeit des Servers möglicherweise auch nach Auslagerung von Arbeitsspeicherdaten auf die Festplatte aufrechterhalten. Die Option **Sperren von Seiten im Speicher** wird für Instanzen der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Standard Edition und höher auf ON gesetzt, wenn dem Konto mit den Privilegien zum Ausführen von "sqlserver.exe" das Windows-Benutzerrecht *Sperren von Seiten im Speicher* (Lock Pages in Memory, LPIM) erteilt wurde.  
  
Entfernen Sie zum Deaktivieren der Option **Sperren von Seiten im Speicher** für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] das Benutzerrecht *Sperren von Seiten im Speicher* für das Konto mit der Ausführungsberechtigung für sqlserver.exe (das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Startkonto).  
 
Das Festlegen dieser Option wirkt sich nicht auf die [dynamische Arbeitsspeicherverwaltung](../../relational-databases/memory-management-architecture-guide.md#dynamic-memory-management) von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] aus und ermöglicht ein Erweitern oder Verkleinern aufgrund der Anforderungen anderer Arbeitsspeicherclerks. Bei der Verwendung des Benutzerrechts *Sperren von Seiten im Speicher* empfiehlt es sich, einen oberen Grenzwert für **Max. Serverarbeitsspeicher** festzulegen, wie [oben ausführlich beschrieben](#max_server_memory).

> [!IMPORTANT]
> Das Festlegen dieser Option sollte nur bei Bedarf erfolgen, nämlich wenn es Anzeichen gibt, dass der sqlservr-Prozess ausgelagert wird. In diesem Fall wird im Fehlerprotokoll der Fehler 17890 gemeldet, ähnlich wie im folgenden Beispiel: `A significant part of sql server process memory has been paged out. This may result in a performance degradation. Duration: #### seconds. Working set (KB): ####, committed (KB): ####, memory utilization: ##%.`
> Ab [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] wird das [Ablaufverfolgungsflag 845](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) nicht von der Standard Edition benötigt, um gesperrte Seiten zu verwenden. 
  
### <a name="to-enable-lock-pages-in-memory"></a>Aktivieren des Sperrens von Seiten im Speicher  
So aktivieren Sie die Option "Sperren von Seiten im Speicher":  
  
1.  Klicken Sie im Menü **Start** auf **Ausführen**. Geben Sie **gpedit.msc** im Feld **Öffnen** ein.  
  
     Das Dialogfeld **Gruppenrichtlinie** wird geöffnet.  
  
2.  Erweitern Sie in der Konsole **Gruppenrichtlinie** die Option **Computerkonfiguration** und dann **Windows-Einstellungen**.  
  
3.  Erweitern Sie **Sicherheitseinstellungen** und dann **Lokale Richtlinien**.  
  
4.  Wählen Sie den Ordner **Zuweisen von Benutzerrechten** aus.  
  
     Die Richtlinien werden im Detailbereich angezeigt.  
  
5.  Doppelklicken Sie im Detailbereich auf **Sperren von Seiten im Speicher**.  
  
6.  Fügen Sie im Dialogfeld **Lokale Sicherheitseinstellung** das Konto mit Privilegien zum Ausführen von „sqlservr.exe“ (das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Startkonto) hinzu.  
  
## <a name="multiple-instances-of-ssnoversion"></a>Mehrere Instanzen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
 Wenn Sie mehrere Instanzen von [!INCLUDE[ssDE](../../includes/ssde-md.md)]ausführen, stehen Ihnen zum Verwalten des Arbeitsspeichers drei Möglichkeiten zur Verfügung:  
  
-   Verwenden Sie **Max. Serverarbeitsspeicher**, um die Speicherbelegung zu steuern, wie [oben ausführlich dargestellt](#max_server_memory). Richten Sie für jede Instanz Maximaleinstellungen ein, und achten Sie darauf, dass der gesamte zugeordnete Arbeitsspeicher nicht größer ist als der insgesamt auf dem Computer verfügbare physische Speicher. Es empfiehlt sich, den jeder Instanz zugeordneten Arbeitsspeicher proportional zur erwarteten Arbeitsauslastung oder Datenbankgröße zu bemessen. Dieser Ansatz hat den Vorteil, dass beim Starten neuer Prozesse oder Instanzen sofort freier Arbeitsspeicher für die Prozesse oder Instanzen zur Verfügung steht. Der Nachteil ist, wenn nicht alle Instanzen ausgeführt werden, dass keine der laufenden Instanzen den verbleibenden freien Arbeitsspeicher nutzen kann.  
  
-   Verwenden Sie **Min. Serverarbeitsspeicher**, um die Speicherbelegung zu steuern, wie [oben ausführlich dargestellt](#min_server_memory). Richten Sie für jede Instanz Minimaleinstellungen ein, sodass die Summe dieser Mindestwerte 1 bis 2 GB unterhalb des gesamten physischen Speichers auf dem Computer liegt. Auch bei dieser Methode empfiehlt es sich, die Werte proportional zu der für die jeweilige Instanz erwarteten Arbeitsauslastung zu bemessen. Dieser Ansatz hat den Vorteil, dass die laufenden Instanzen den verbleibenden freien Arbeitsspeicher nutzen können, wenn nicht alle Instanzen gleichzeitig ausgeführt werden. Diese Vorgehensweise ist auch dann sinnvoll, wenn auf dem Computer ein weiterer speicherintensiver Prozess vorhanden ist, da sichergestellt ist, dass [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] zumindest eine angemessene Menge an Arbeitsspeicher erhält. Der Nachteil besteht darin, dass es beim Starten einer neuen Instanz (oder eines anderen Prozesses) ggf. etwas dauern kann, bis die laufenden Instanzen Speicher freigeben. Dies trifft vor allem dann zu, wenn die Instanzen zuerst noch geänderte Seiten in ihre jeweiligen Datenbanken zurückschreiben müssen.  
  
-   Unternehmen Sie nichts (dies wird nicht empfohlen). Die ersten Instanzen, denen eine Arbeitslast zugewiesen wird, weisen sich den gesamten Arbeitsspeicher zu. Instanzen im Leerlauf oder Instanzen, die später gestartet werden, müssen in dieser Situation u. U. mit einer minimalen Menge an Arbeitsspeicher auskommen. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] versucht nicht, die Speicherauslastung über mehrere Instanzen hinweg auszugleichen. Alle Instanzen antworten jedoch auf Signale der Windows-Arbeitsspeicherbenachrichtigung, um ihren Speicherbedarf anzupassen. Windows nimmt keinen Speicherausgleich bei Anwendungen vor, die über eine Arbeitsspeicherbenachrichtigungs-API verfügen. Es erfolgt lediglich eine globale Rückmeldung über die Verfügbarkeit von Arbeitsspeicher auf dem System.  
  
 Sie können diese Einstellungen ohne Neustart der Instanzen ändern. Dadurch können Sie problemlos mit verschiedenen Einstellungen experimentieren, um die für Ihr Nutzungsmuster am besten geeigneten Einstellungen herauszufinden.  
  
## <a name="provide-the-maximum-amount-of-memory"></a>Bereitstellen der maximalen Arbeitsspeichermenge

In allen Editionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] kann der Arbeitsspeicher bis zum Speicherplatzlimit des virtuellen Adressraums des Prozesses konfiguriert werden. Weitere Informationen finden Sie unter [Memory Limits for Windows and Windows Server Releases](/windows/desktop/Memory/memory-limits-for-windows-releases#physical-memory-limits-windows-server-2016) (Grenzwerte für den Arbeitsspeicher für Versionen von Windows und Windows Server).

## <a name="examples"></a>Beispiele

### <a name="example-a-set-the-max-server-memory-option-to-4-gb"></a>Beispiel A: Festlegen der Option „Max. Serverarbeitsspeicher“ auf 4 GB.
 Im folgenden Beispiel wird die Option `max server memory` auf 4 GB festgelegt.  Beachten Sie, dass im Beispiel `(MB)` ausgelassen wird, obwohl `sp_configure` den Namen der Option als `max server memory (MB)` festlegt.

```sql
sp_configure 'show advanced options', 1;
GO
RECONFIGURE;
GO
sp_configure 'max server memory', 4096;
GO
RECONFIGURE;
GO
```
Dadurch wird eine Anweisung ausgegeben ähnlich wie:

> Die Konfigurationsoption 'max server memory (MB)' (Max. Serverarbeitsspeicher (MB)) wurde von 2147483647 in 4096 geändert. Führen Sie zum Installieren die RECONFIGURE-Anweisung aus.

### <a name="example-b-determining-current-memory-allocation"></a>Beispiel B: Bestimmen der aktuellen Speicherbelegung  
 Mit der folgenden Abfrage werden Informationen zur aktuellen Speicherbelegung zurückgegeben.  

```sql  
SELECT 
  physical_memory_in_use_kb/1024 AS sql_physical_memory_in_use_MB, 
   large_page_allocations_kb/1024 AS sql_large_page_allocations_MB, 
   locked_page_allocations_kb/1024 AS sql_locked_page_allocations_MB,
   virtual_address_space_reserved_kb/1024 AS sql_VAS_reserved_MB, 
   virtual_address_space_committed_kb/1024 AS sql_VAS_committed_MB, 
   virtual_address_space_available_kb/1024 AS sql_VAS_available_MB,
   page_fault_count AS sql_page_fault_count,
   memory_utilization_percentage AS sql_memory_utilization_percentage, 
   process_physical_memory_low AS sql_process_physical_memory_low, 
   process_virtual_memory_low AS sql_process_virtual_memory_low
FROM sys.dm_os_process_memory;  
```  

### <a name="example-c-determining-value-for-max-server-memory-mb"></a>Beispiel C: Bestimmen eines Werts für die Option „Max. Serverarbeitsspeicher (MB)“
Die folgende Abfrage gibt Informationen zum aktuell konfigurierten Wert und dem von SQL Server verwendeten Wert zurück.  Diese Abfrage gibt Ergebnisse zurück, unabhängig davon, ob für „Erweiterte Optionen anzeigen“ „true“ gilt.

```sql
SELECT c.value, c.value_in_use
FROM sys.configurations c WHERE c.[name] = 'max server memory (MB)'
```
  
## <a name="next-steps"></a>Nächste Schritte
 [Handbuch zur Architektur der Speicherverwaltung](../../relational-databases/memory-management-architecture-guide.md)   
 [Überwachen und Optimieren der Leistung](../../relational-databases/performance/monitor-and-tune-for-performance.md)   
 [RECONFIGURE &#40;Transact-SQL&#41;](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [Serverkonfigurationsoptionen &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)   
 [Startoptionen für den Datenbank-Engine-Dienst](../../database-engine/configure-windows/database-engine-service-startup-options.md)   
 [Editionen und unterstützten Funktionen von SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md#Cross-BoxScaleLimits)   
 [Editionen und unterstützten Funktionen von SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md#Cross-BoxScaleLimits)   
 [Editionen und unterstützte Funktionen von SQL Server 2017 unter Linux](../../linux/sql-server-linux-editions-and-components-2017.md#Cross-BoxScaleLimits)   
 [Memory Limits for Windows and Windows Server Releases](/windows/desktop/Memory/memory-limits-for-windows-releases) (Grenzwerte für den Arbeitsspeicher für Versionen von Windows und Windows Server).
