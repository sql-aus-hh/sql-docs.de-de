---
title: 'Powershell: Datenbankspiegelungsendpunkt für Verfügbarkeitsgruppen'
description: In diesem Artikel wird das Erstellen eines Datenbankspiegelungsendpunkts für eine Always On-Verfügbarkeitsgruppe mithilfe von PowerShell beschrieben.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- Availability Groups [SQL Server], server instance
- Availability Groups [SQL Server], deploying
- Availability Groups [SQL Server], endpoint
ms.assetid: 6197bbe7-67d4-446d-ba5f-cabfa5df77f1
author: cawrites
ms.author: chadam
ms.openlocfilehash: b9b1be7e6cbfead5bf0fa14bf60f294ad852d9ad
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643236"
---
# <a name="create-a-database-mirroring-endpoint-for-an-availability-group-using-powershell"></a>Erstellen eines Datenbankspiegelungsendpunkts für eine Verfügbarkeitsgruppe mithilfe von PowerShell
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  In diesem Thema wird beschrieben, wie ein Datenbankspiegelungs-Endpunkt zur Verwendung durch [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] in [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] mit PowerShell erstellt wird.  
  

  
##  <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
 Erfordert die CREATE ENDPOINT-Berechtigung oder die Mitgliedschaft in der festen Serverrolle "sysadmin". Weitere Informationen finden Sie unter [GRANT (Endpunktberechtigungen) &#40;Transact-SQL&#41;](../../../t-sql/statements/grant-endpoint-permissions-transact-sql.md).  

> [!IMPORTANT]  
>  Der RC4-Algorithmus ist veraltet. [!INCLUDE[ssNoteDepFutureDontUse](../../../includes/ssnotedepfuturedontuse-md.md)] Stattdessen wird die Verwendung von AES empfohlen.  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> PowerShell  
 **So erstellen Sie einen Datenbankspiegelungs-Endpunkt**  
  
1.  Wechseln Sie mit **cd** in das Verzeichnis der Serverinstanz, für die Sie den Datenbankspiegelungs-Endpunkt erstellen möchten.  
  
2.  Verwenden Sie das Cmdlet **New-SqlHadrEndpoint** , um den Endpunkt zu erstellen und verwenden Sie dann **Set-SqlHadrEndpoint** , um den Endpunkt zu starten.  
  
###  <a name="example-powershell"></a><a name="PShellExample"></a> Beispiel (PowerShell)  
 Mit den folgenden PowerShell-Befehlen wird ein Datenbankspiegelungs-Endpunkt auf einer Instanz von SQL Server erstellt (*Computer*\\*Instanz*). Der Endpunkt verwendet Port 5022.  
  
> [!IMPORTANT]  
>  Dieses Beispiel funktioniert nur auf einer Serverinstanz, die derzeit keinen Datenbankspiegelungs-Endpunkt hat.  
  
```  
# Create the endpoint.  
$endpoint = New-SqlHadrEndpoint MyMirroringEndpoint -Port 5022 -Path SQLSERVER:\SQL\Machine\Instance  
  
# Start the endpoint  
Set-SqlHadrEndpoint -InputObject $endpoint -State "Started"  
  
```  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Verwandte Aufgaben  
 **So konfigurieren Sie einen Datenbankspiegelungs-Endpunkt**  
  
-   [Erstellen eines Endpunkts der Datenbankspiegelung für Windows-Authentifizierung &#40;Transact-SQL&#41;](../../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md)  
  
-   [Verwenden von Zertifikaten für einen Datenbankspiegelungs-Endpunkt &#40;Transact-SQL&#41;](../../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md)  
  
    -   [Ermöglichen des Verwendens von Zertifikaten für ausgehende Verbindungen für einen Datenbankspiegelungs-Endpunkt &#40;Transact-SQL&#41;](../../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md)  
  
    -   [Ermöglichen des Verwendens von Zertifikaten für eingehende Verbindungen für einen Datenbankspiegelungs-Endpunkt &#40;Transact-SQL&#41;](../../../database-engine/database-mirroring/database-mirroring-use-certificates-for-inbound-connections.md)  
  
-   [Angeben einer Servernetzwerkadresse &#40;Datenbankspiegelung&#41;](../../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md)  
  
-   [Angeben der Endpunkt-URL beim Hinzufügen oder Ändern eines Verfügbarkeitsreplikats &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/specify-endpoint-url-adding-or-modifying-availability-replica.md)  
  
 **So zeigen Sie Informationen zum Datenbankspiegelungs-Endpunkt an**  
  
-   [sys.database_mirroring_endpoints &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Erstellen einer Verfügbarkeitsgruppe &#40;Transact-SQL&#41;](../../../database-engine/availability-groups/windows/create-an-availability-group-transact-sql.md)   
 [Übersicht zu AlwaysOn-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)  
  
  
