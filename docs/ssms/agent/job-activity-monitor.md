---
description: Auftragsaktivitätsmonitor
title: Auftragsaktivitätsmonitor
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- sql13.ag.jobactivitymonitor.alljobs.f1
- SQL13.SWB.ACTIVITYMON.F1
ms.assetid: 11f2182c-5f71-46f8-8d2b-74f0fc48f2d6
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 96a19ea760409d354390c171965f494f59cdff42
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97477031"
---
# <a name="job-activity-monitor"></a>Auftragsaktivitätsmonitor
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> In [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance) werden derzeit die meisten, aber nicht alle, SQL Server-Agent-Features unterstützt. Details dazu finden Sie unter [T-SQL-Unterschiede zwischen Azure SQL Managed Instance und SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Mithilfe dieser Seite können Sie die aktuelle Aktivität von Aufträgen des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agents anzeigen. Klicken Sie auf **Filter** , um die Anzahl der angezeigten Aufträge zu beschränken. Das Raster **Agentauftragsaktivität** ist schreibgeschützt. Klicken Sie auf die Spaltenheader, um das Raster zu sortieren. Sie können einen Auftrag ändern, indem Sie auf den betreffenden Auftrag doppelklicken und das Dialogfeld **Auftragseigenschaften** öffnen. Klicken Sie mit der rechten Maustaste auf einen Auftrag im Raster, um die Ausführung aller Auftragsschritte zu starten, einen speziellen Auftragsschritt zu starten, den Auftrag zu aktivieren oder zu deaktivieren, den Auftrag zu aktualisieren, den Auftrag zu löschen, den Verlauf des Auftrags anzuzeigen oder die Auftragseigenschaften anzuzeigen. Klicken Sie auf **Aktualisieren** , um das Raster mit den aktuellen Informationen zu aktualisieren.  
  
## <a name="options"></a>Tastatur  
**Name**  
Der Name des Auftrags.  
  
**Enabled**  
Zeigt an, ob der Auftrag aktiviert (**Ja**) oder nicht aktiviert (**Nein**) ist.  
  
**Status**  
Aktueller Status des Auftrags  
  
*Ergebnis der letzten Ausführung**  
Auftragsstatus bei der letzten Ausführung  
  
**Zuletzt ausgeführt**  
Datum und Uhrzeit, an dem bzw. zu der der Auftrag zuletzt ausgeführt wurde (ausgehend vom lokalen Datum und der lokalen Uhrzeit des Servers).  
  
**Nächste Ausführung**  
Datum und Uhrzeit des Zeitpunkts, an dem die nächste Ausführung des Auftrags geplant ist (ausgehend vom lokalen Datum und der lokalen Uhrzeit des Servers).  
  
*Kategorie**  
Die dem Auftrag zugewiesene Auftragskategorie.  
  
**Ausführbar**  
**Ja** , wenn der Auftrag ausgeführt werden kann; **Nein** , wenn der Auftrag nicht ausgeführt werden kann. Ein Auftrag kann nicht ausgeführt werden, wenn er keine Schritte oder keinen Zielserver aufweist.  
  
**Geplant**  
**Ja** , wenn der Auftrag einem Auftragszeitplan zugewiesen ist; **Nein** , wenn für den Auftrag kein Zeitplan vorhanden ist.  
  
* Nur Mitglieder der festen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Serverrolle „sysadmin“ und die Serveradministratorgruppe können Werte in dieser Spalte sehen. Mitglieder der SQLAgentOperatorRole-Rolle können keine Werte in dieser Spalte sehen.  
  
#### <a name="to-open-the-job-activity-monitor"></a>So öffnen Sie den Auftragsaktivitätsmonitor  
  
-   Erweitern Sie in **Objekt-Explorer** den verwendeten Server und anschließend **SQL Server-Agent**. Klicken Sie mit der rechten Maustaste auf **Auftragsaktivitätsmonitor**, und klicken Sie dann auf **Auftragsaktivitäten anzeigen**.  
  
## <a name="see-also"></a>Weitere Informationen  
[Überwachen der Auftragsaktivität](../../ssms/agent/monitor-job-activity.md)  
