---
title: SQL Server-Fehlerprotokoll (Always On-Verfügbarkeitsgruppen) (SQL Server) | Microsoft-Dokumentation
ms.custom: ag-guide
ms.date: 06/13/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: high-availability
ms.topic: conceptual
ms.assetid: 39d0c98d-75af-4dd1-b908-30d31af56f2a
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: 811b01d60e280e6424876195dfd15e900af912ba
ms.sourcegitcommit: 61381ef939415fe019285def9450d7583df1fed0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2018
ms.locfileid: "47599180"
---
# <a name="sql-server-error-log-always-on-availability-groups"></a>SQL Server-Fehlerprotokoll (Always On-Verfügbarkeitsgruppen)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Folgende Ereignisse von SQL Server-Fehlerprotokollberichten wirken sich auf Always On-Verfügbarkeitsgruppen aus:  
  
-   Kommunikation mit dem WSFC-Cluster (Windows Server Failover Clustering)  
  
-   Statusübergänge von Verfügbarkeitsreplikaten  
  
-   Statusübergänge von Verfügbarkeitsdatenbanken  
  
-   Konnektivitätsstatus von Verfügbarkeitsdatenbanken zwischen primären und sekundären Replikaten  
  
-   Status der Verfügbarkeitsgruppenendpunkte  
  
-   Status der Verfügbarkeitsgruppenlistener  
  
-   Status der Leasedauer zwischen der SQL Server-Ressourcen-DLL (die im WSFC-Cluster ausgeführt wird) und der SQL Server-Instanz (weitere Informationen finden Sie unter [How It Works: SQL Server Always On lease timeout (Funktionsweise: Ablauf der Leasedauer der SQL Server-Option „Always On“)](http://blogs.msdn.com/b/psssql/archive/2012/09/07/how-it-works-sql-server-alwayson-lease-timeout.aspx))  
  
-   Fehlerereignisse in der Verfügbarkeitsgruppe  


Wenn die folgenden Aussagen zutreffen, sollten Sie sich das SQL Server-Fehlerprotokoll ansehen:  

-   Der Zugriff auf Verfügbarkeitsdatenbanken ist nicht möglich.  
  
-   Es gab einen unerwarteten Failover einer Verfügbarkeitsgruppe.  
  
-   Eine Verfügbarkeitsgruppe befindet sich unerwartet im Status „Wird aufgelöst“.  
  
-   Eine Verfügbarkeitsgruppe befindet sich in einem unbestimmten Status.  
  
Weitere Informationen finden Sie unter [View the SQL Server error log &#40;SQL Server Management Studio&#41; (Anzeigen des SQL Server-Fehlerprotokolls &#40;SQL Server Management Studio&#41;)](~/relational-databases/performance/view-the-sql-server-error-log-sql-server-management-studio.md).  
  
  
