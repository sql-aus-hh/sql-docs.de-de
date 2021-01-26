---
title: Aktualisieren von SQLCLR-Assemblys nach .NET Framework Update
description: Erfahren Sie, wie Sie die von SQL Server Data Quality Services (DQS) verwendeten SQLCLR-Assemblys Aktualisieren, nachdem Sie .NET Framework aktualisiert haben.
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: data-quality-services
ms.reviewer: ''
ms.technology: data-quality-services
ms.topic: conceptual
ms.assetid: b1a008cc-7e6b-4655-a869-bd429f986400
author: swinarko
ms.author: sawinark
ms.openlocfilehash: 4b835c60da2d2848b33dd453605290a1fb056180
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/25/2021
ms.locfileid: "98765480"
---
# <a name="upgrade-sqlclr-assemblies-after-net-framework-update"></a>Aktualisieren von SQLCLR-Assemblys nach .NET Framework-Update

[!INCLUDE [SQL Server - Windows only ](../../includes/applies-to-version/sql-windows-only.md)]

  [!INCLUDE[ssDQSnoversion](../../includes/ssdqsnoversion-md.md)] (DQS) ist eine Auflistung der SQL CLR-Routinen (SQL Common Language Runtime), die auf Microsoft .NET Framework 4-Assemblys verweisen. Wenn Sie ein .NET Framework-Update auf dem Computer installieren, das sich auf eine der .NET Framework-Assemblys, auf die verwiesen wird, auswirkt, wird infolgedessen die MVID (Modul Version ID) der Assembly im globalen Assemblycache (GAC) geändert. Dadurch wird ein Konflikt zwischen den MVIDs der Assembly im GAC, auf die verwiesen wird, und der Assembly in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]verursacht.  
  
 Wenn es beim .NET Framework-Update erforderlich ist, den [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] -Computer neu zu starten, werden die betroffenen SQLCLR-Assemblys automatisch aktualisiert, um den MVID-Konflikt beim Neustart des [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] -Computers zu beheben. Bei .NET Framework-Updates jedoch, bei denen kein Neustart des [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] -Computers erforderlich ist, tritt ein Fehler aufgrund des Konflikts in den MVIDs der Assemblys auf, wenn Sie versuchen, eine Verbindung mit einem [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] herzustellen, der einen [!INCLUDE[ssDQSClient](../../includes/ssdqsclient-md.md)]verwendet:  
  
```  
A new version of .NET was installed on this machine. In order to continue to work with DQS please run dqsinstaller.exe -upgradedlls.  
```  
  
 Um dieses Problem zu beheben, müssen die betroffenen SQLCLR-Assemblys in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] aktualisiert werden. Sie können zu diesem Zweck die Datei DQSInstaller.exe mit dem **upgradedlls** -Befehlszeilenparameter ausführen, um das Neuerstellen der DQS-Datenbanken zu überspringen und nur die betroffenen Assemblys zu aktualisieren. Dadurch wird sichergestellt, dass die Knowledge Bases, Datenqualitätsprojekte und alle anderen Daten in DQS beibehalten werden.  
  
## <a name="prerequisites"></a>Voraussetzungen  
  
-   Sie müssen als Mitglied der Administratorgruppe auf dem [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] -Computer angemeldet sein.  
  
-   Ihr Windows-Benutzerkonto muss Mitglied der festen Serverrolle sysadmin auf der SQL Server-Instanz sein, auf der der [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] installiert ist.  
  
### <a name="to-upgrade-sqlclr-assemblies"></a>So aktualisieren Sie SQLCLR-Assemblys  
  
1.  Öffnen Sie die Eingabeaufforderung.  
  
2.  Wechseln Sie an der Eingabeaufforderung zu dem Verzeichnis, in dem DQSInstaller.exe enthalten ist. Wenn Sie z. B. die Standardinstanz von SQL Server installiert haben, steht die Datei DQSInstaller.exe in der Regel unter C:\Programme\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Binn zur Verfügung:  
  
    ```  
    cd C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Binn  
    ```  
  
3.  Geben Sie an der Eingabeaufforderung den folgenden Befehl ein, und drücken Sie die EINGABETASTE:  
  
    ```  
    dqsinstaller.exe -upgradedlls  
    ```  
  
4.  Die restlichen Schritte entsprechen den Schritten 2–6 im Abschnitt [Ausführen von "DQSInstaller.exe" über den Startbildschirm, das Startmenü oder Windows-Explorer](../../data-quality-services/install-windows/run-dqsinstaller-exe-to-complete-data-quality-server-installation.md#WindowsExplorer) des Artikels [Ausführen von DQSInstaller.exe zum Abschließen der Installation von Data Quality Server](../../data-quality-services/install-windows/run-dqsinstaller-exe-to-complete-data-quality-server-installation.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Installieren von Data Quality Services](../../data-quality-services/install-windows/install-data-quality-services.md)   
 [Aktualisieren des DQS-Datenbankschemas nach der Installation eines SQL Server-Updates](../../data-quality-services/install-windows/upgrade-dqs-databases-schema-after-installing-sql-server-update.md)  
  
  
