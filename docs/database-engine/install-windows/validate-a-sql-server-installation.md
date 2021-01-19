---
title: Überprüfen einer SQL Server-Installation | Microsoft-Dokumentation
description: Mithilfe des SQL Server-Ermittlungsberichts können Sie die Version und die auf dem Computer installierten Features von SQL Server überprüfen.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- validating installations [SQL Server]
ms.assetid: 1689af50-d2b8-4aa6-8f27-cc7127157fc8
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 2138be8c23aa6a9849ce19872d3c1b455223540e
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170122"
---
# <a name="validate-a-sql-server-installation"></a>Überprüfen einer SQL Server-Installation

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]
  
  Mithilfe des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Ermittlungsberichts können Sie die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Version sowie die auf dem Computer installierten [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Funktionen überprüfen. Der **Bericht zur Ermittlung installierter [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Funktionen** zeigt einen Bericht aller [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)]-, [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]-, [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]-, [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]-, [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]-, [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]-, [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] -und [!INCLUDE[ssSQL15](../../includes/sssqlv14-md.md)]-Produkte und -Funktionen an, die auf dem lokalen Server installiert sind. Der Bericht zur Ermittlung der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Funktionen steht im **-Installationscenter auf der Seite** Tools [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bereit.  
  
 ## <a name="run-ssnoversion-features-discovery-report"></a>Ausführen des Bericht zur Ermittlung der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Funktionen  
  
 Rufen Sie das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Installationscenter auf. Klicken Sie dazu im **Startmenü** auf **Alle Programme**, **[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] \<Version Name>** > **Konfigurationstools**, und klicken Sie auf **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Installationscenter**. Klicken Sie im linken Navigationsbereich des **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Installationscenter** auf **Tools** und anschließend auf **Bericht zur Ermittlung installierter [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Funktionen**, um den Bericht zur Ermittlung der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Funktionen auszuführen.  
  
 Der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Ermittlungsbericht wird unter %ProgramFiles%\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\\*nnn*\Setup Bootstrap\Log\\<letzte Setupsitzung\> gespeichert.  
  
 Sie können den Ermittlungsbericht auch über die Befehlszeile generieren. Führen Sie „Setup.exe /Action=RunDiscovery“ über eine Eingabeaufforderung aus. Wenn Sie der oben stehenden Befehlszeile den Parameter „/q“ hinzufügen, wird keine Benutzeroberfläche angezeigt, stattdessen wird der entsprechende Bericht unter %ProgramFiles%\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\\*nnn*\Setup Bootstrap\Log\\<letzte Setupsitzung\> erstellt.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Lesen und Anzeigen der Setupprotokolldateien von SQL Server](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)  
  
  
