---
title: Azure Arc-fähige SQL Server-Instanz
titleSuffix: ''
description: Verwalten Sie SQL Server-Instanzen mit einer Azure Arc-fähigen SQL Server-Instanz.
author: anosov1960
ms.author: sashan
ms.reviewer: mikeray
ms.date: 12/08/2020
ms.topic: conceptual
ms.prod: sql
ms.custom: references_regions
ms.openlocfilehash: c1ba7f7552b5050e3c1fa7bc765acfa431f3df30
ms.sourcegitcommit: 18e2f0706e03d0b2b6324845244fbafaa077a8dd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/11/2020
ms.locfileid: "97103145"
---
# <a name="azure-arc-enabled-sql-server-preview"></a>Azure Arc-fähige SQL-Server-Instanz (Vorschauversion)

Azure Arc-fähige SQL Server-Instanzen sind Teil von Azure Arc für Server. Sie weiten Azure-Dienste auf SQL Server-Instanzen aus, die außerhalb von Azure im Rechenzentrum des Kunden, am Edge oder in einer Multicloudumgebung gehostet werden.

Damit Azure-Dienste aktiviert werden können, muss eine ausgeführte SQL Server-Instanz mithilfe des Azure-Portals und eines Registrierungsskripts bei Azure Arc registriert werden. Nach der Registrierung wird die Instanz in Azure als Ressource vom Typ __SQL Server – Azure Arc__ dargestellt. Die Eigenschaften dieser Ressource spiegeln einen Teil der Konfigurationseinstellungen in SQL Server wider.

SQL Server kann auf einem virtuellen oder physischen Computer installiert werden, auf dem Windows oder Linux ausgeführt wird und der über den Connected Machine-Agent mit Azure Arc verbunden ist. Im Rahmen der Registrierung der SQL Server-Instanz wird der Agent installiert und der Computer automatisch registriert. Der Connected Machine-Agent kommuniziert ausgehend sicher über den TCP-Port 443 mit Azure Arc. Wenn der Computer bei der Kommunikation über das Internet die Verbindung durch eine Firewall oder über einen HTTP-Proxyserver herstellt, sehen Sie sich die [Anforderungen für die Netzwerkkonfiguration für den Connected Machine-Agent](/azure/azure-arc/servers/agent-overview#prerequisites) an.

Die Public Preview von Azure Arc-fähigen SQL Server-Instanzen unterstützt eine Reihe von Lösungen, bei denen die Microsoft Monitoring Agent-Servererweiterung (MMA) installiert und mit einem Azure Log Analytics-Arbeitsbereich für die Datensammlung und Berichterstellung verbunden sein muss. Zu diesen Lösungen gehören erweiterte Datensicherheit mit dem Azure Security Center und Azure Sentinel sowie die Prüfung der SQL-Umgebungsintegrität mithilfe des Features zur bedarfsgesteuerten SQL-Bewertung.

Das folgende Diagramm zeigt die Architektur von Azure Arc-fähigen SQL Server-Instanzen.

![Architektur der Public Preview](media/overview/pubic-preview-architecture.png)

## <a name="prerequisites"></a>Voraussetzungen

### <a name="supported-sql-versions-and-operating-systems"></a>Unterstützte SQL-Versionen und Betriebssysteme

Azure Arc-fähige SQL Server-Instanzen unterstützen SQL Server 2012 und höher auf den folgenden Windows- und Linux-Betriebssystemen:

- Windows Server 2012 R2 und höhere Versionen
- Ubuntu 16.04 und 18.04 (x64)
- Red Hat Enterprise Linux (RHEL) 7 (x64) 
- SUSE Linux Enterprise Server (SLES) 15 (x64)

### <a name="required-permissions"></a>Erforderliche Berechtigungen

Sie benötigen ein Konto mit Berechtigungen zur Ausführung der folgenden Aktionen, um eine Verbindung zwischen den SQL Server-Instanzen, dem Hostcomputer und Azure Arc herzustellen:
   * Microsoft.AzureArcData/sqlServerInstances/read
   * Microsoft.AzureArcData/sqlServerInstances/write
   * Microsoft.HybridCompute/machines/read
   * Microsoft.HybridCompute/machines/write
   * Microsoft.GuestConfiguration/guestConfigurationAssignments/read

Für optimale Sicherheit wird empfohlen, eine benutzerdefinierte Rolle in Azure zu erstellen, die über die aufgeführten Mindestberechtigungen verfügt. Informationen zum Erstellen einer benutzerdefinierten Rolle in Azure mit diesen Berechtigungen finden Sie unter [Benutzerdefinierte Administratorrollen in Azure Active Directory (Vorschau)](/azure/active-directory/users-groups-roles/roles-custom-overview). Informationen zum Hinzufügen von Rollenzuweisungen finden Sie unter [Hinzufügen oder Entfernen von Azure-Rollenzuweisungen über das Azure-Portal](/azure/role-based-access-control/role-assignments-portal) oder [Hinzufügen oder Entfernen von Azure-Rollenzuweisungen mithilfe der Azure-Befehlszeilenschnittstelle](/azure/role-based-access-control/role-assignments-cli).

### <a name="azure-subscription-and-service-limits"></a>Einschränkungen von Azure-Abonnements und -Diensten

Machen Sie sich vor dem Konfigurieren Ihrer SQL Server-Instanzen und Computer mit Azure Arc mit den [Grenzwerten für Abonnements](/azure/azure-resource-manager/management/azure-subscription-service-limits#subscription-limits) sowie den [Grenzwerten für Ressourcengruppen](/azure/azure-resource-manager/management/azure-subscription-service-limits#resource-group-limits) im Azure Resource Manager vertraut, um die Anzahl von zu verbindenden Computern zu planen.

### <a name="networking-configuration-and-resource-providers"></a>Netzwerkkonfiguration und Ressourcenanbieter

Sehen Sie sich die [Netzwerkkonfiguration, Transport Layer Security und Ressourcenanbieter](/azure/azure-arc/servers/agent-overview#prerequisites) an, die für den Connected Machine-Agent erforderlich sind.

Der Ressourcenanbieter `Microsoft.AzureArcData` ist zum Herstellen einer Verbindung zwischen SQL Server-Instanzen und Azure Arc erforderlich. Weitere Informationen finden Sie in den Anweisungen für die Ressourcenanbieterregistrierung im Abschnitt [Voraussetzungen](connect.md#prerequisites).

Wenn Sie bereits eine Verbindung zwischen den SQL Server-Instanzen und Azure Arc eingerichtet haben, führen Sie die Schritte zum Migrieren der vorhandenen **SQL Server – Azure Arc**-Ressourcen zum neuen Namespace durch.

### <a name="supported-azure-regions"></a>Unterstützte Azure-Regionen

Die Public Preview ist in den folgenden Regionen verfügbar:
- East US
- USA (Ost) 2
- USA, Westen 2
- Australien (Osten)
- Asien, Südosten
- Nordeuropa
- Europa, Westen
- UK, Süden

## <a name="next-steps"></a>Nächste Schritte

- [Herstellen einer Verbindung zwischen Ihrer SQL Server-Instanz und Azure Arc](connect.md)
- [Konfigurieren Ihrer SQL Server-Instanz für eine regelmäßige Umgebungsintegritätsprüfung mithilfe der bedarfsgesteuerten SQL-Bewertung](assess.md)
- [Konfigurieren von erweiterter Datensicherheit für Ihre SQL Server-Instanz](configure-advanced-data-security.md)