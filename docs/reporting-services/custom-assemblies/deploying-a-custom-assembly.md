---
title: Bereitstellen einer benutzerdefinierten Assembly | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine benutzerdefinierte Assembly in SQL Server Reporting Services bereitstellen. Außerdem erfahren Sie, wie Sie benutzerdefinierten Assemblys Berechtigungen erteilen, die über Ausführungsberechtigungen hinausgehen.
ms.date: 11/23/2020
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: custom-assemblies
ms.topic: reference
helpviewer_keywords:
- deploying custom assemblies [Reporting Services]
- custom assemblies [Reporting Services], deploying
- custom assemblies [Reporting Services], updating
- updating custom assemblies
ms.assetid: c6674cd8-0de7-4a5a-9e7c-12ffa49f6fd2
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 9aa3a01e88b699f184ea05dfcb3650b4653837cb
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/20/2021
ms.locfileid: "98595498"
---
# <a name="deploying-a-custom-assembly"></a>Bereitstellen einer benutzerdefinierten Assembly
  Platzieren Sie die Assembly in den Anwendungsordnern des Berichtsservers und des Berichts-Designers, um eine benutzerdefinierte Assembly in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] bereitzustellen. Standardmäßig wird benutzerdefinierten Assemblys die **Execution**-Berechtigung in [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] erteilt. Sie müssen die Konfigurationsdatei „rssrvpolicy.config“ für den Berichtsserver und die Konfigurationsdatei „rspreviewpolicy.config“ für das Vorschaufenster des Berichts-Designers bearbeiten, um benutzerdefinierten Assemblys Privilegien über die Execute-Berechtigung hinaus zu erteilen. Alternativ dazu können Sie die benutzerdefinierte Assembly auch im globalen Assemblycache (Global Assembly Cache, GAC) installieren.  
  
> [!NOTE]  
>  Es stehen zwei Vorschaumodi für den Berichts-Designer zur Verfügung: die Registerkarte „Vorschau“ und das Popup-Vorschaufenster, das beim Start Ihres Berichtsprojekts im **DebugLocal**-Modus aufgerufen wird. Die Vorschauregisterkarte führt alle Berichtsausdrücke mithilfe des **FullTrust**-Berechtigungssatzes aus und wendet keine Sicherheitsrichtlinieneinstellungen an. Im Popup-Vorschaufenster sollen die Berichtsserverfunktionen simuliert werden. Es enthält daher eine Richtlinienkonfigurationsdatei, die von Ihnen oder einem Administrator verändert werden muss, damit benutzerdefinierte Assemblys im Berichts-Designer verwendet werden können. Diese automatische Vorschau sperrt auch die benutzerdefinierte Assembly. Daher müssen Sie das Vorschaufenster schließen, um den Code der benutzerdefinierten Assembly ändern oder aktualisieren zu können.  
  
## <a name="to-deploy-a-custom-assembly-in-reporting-services"></a>So stellen Sie benutzerdefinierte Assemblys in Reporting Services bereit  
  
1.  Kopieren Sie die benutzerdefinierte Assembly aus dem Build-Verzeichnis in das BIN-Verzeichnis des Berichtsservers oder in den Ordner des Berichts-Designers. 

     Wenn Sie Ihre benutzerdefinierte Assembly im Ordner „Bin“ des Berichtsservers speichern, können Sie Berichte veröffentlichen, die auf die benutzerdefinierte Assembly verweisen. Der Standardspeicherort des Ordners „bin“ für den Berichtsserver ist wie folgt:

     Reporting Services 2016
     ```
     %ProgramFiles%\Microsoft SQL Server\MSRS13.MSSQLSERVER\Reporting Services\ReportServer\bin
     ```
     Ab Reporting Services 2017
     ```
     %ProgramFiles%\Microsoft SQL Server Reporting Services\SSRS\ReportServer\bin
     ```
     Wenn Sie ihn im Ordner „Berichts-Designer“ ablegen, können Sie Berichte ausführen und debuggen, die auf die benutzerdefinierte Assembly im Berichts-Designer verweisen. Der Standardspeicherort des Berichts-Designers ist wie folgt:

     Visual Studio 2012
     ```
     C:\Program Files (x86)\Microsoft Visual Studio 11.0\Common7\IDE\PrivateAssemblies
     ```
     Visual Studio 2013
     ```
     C:\Program Files (x86)\Microsoft Visual Studio 12.0\Common7\IDE\PrivateAssemblies
     ```
     Visual Studio 2015
     ```
     C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\PrivateAssemblies
     ```
     Visual Studio 2017
     ```
     C:\Program Files (x86)\Microsoft Visual Studio\2017\Enterprise\Common7\IDE\CommonExtensions\Microsoft\SSRS
     ```
     Visual Studio 2019
     ```
     C:\Program Files (x86)\Microsoft Visual Studio\2019\Enterprise\Common7\IDE\CommonExtensions\Microsoft\SSRS
     ```  
2.  Öffnen Sie die entsprechende Konfigurationsdatei. Der Standardspeicherort der Datei „rssrvpolicy.config“ für den Berichtsserver ist wie folgt:

     Reporting Services 2016
     ```
     %ProgramFiles%\Microsoft SQL Server\MSRS13.MSSQLSERVER\Reporting Services\ReportServer
     ```
     Ab Reporting Services 2017
     ```
     %ProgramFiles%\Microsoft SQL Server Reporting Services\SSRS\ReportServer
     ```
     Die für den Berichts-Designer zu aktualisierenden Dateien sind wie folgt:

     Visual Studio 2012
     ```
     C:\Program Files (x86)\Microsoft Visual Studio 11.0\Common7\IDE\PrivateAssemblies\PreviewProcessingService.exe.config
     C:\Program Files (x86)\Microsoft Visual Studio 11.0\Common7\IDE\PrivateAssemblies\RSPreviewPolicy.config
     C:\Program Files (x86)\Microsoft Visual Studio 11.0\Common7\IDE\PrivateAssemblies\RSReportDesigner.config
     C:\Program Files (x86)\Microsoft Visual Studio 11.0\Common7\IDE\PrivateAssemblies\RSReportHost11.exe.config
     ```
     Visual Studio 2013
     ```
     C:\Program Files (x86)\Microsoft Visual Studio 12.0\Common7\IDE\PrivateAssemblies\PreviewProcessingService.exe.config
     C:\Program Files (x86)\Microsoft Visual Studio 12.0\Common7\IDE\PrivateAssemblies\RSPreviewPolicy.config
     C:\Program Files (x86)\Microsoft Visual Studio 12.0\Common7\IDE\PrivateAssemblies\RSReportDesigner.config
     C:\Program Files (x86)\Microsoft Visual Studio 12.0\Common7\IDE\PrivateAssemblies\RSReportHost.exe.config
     ```
     Visual Studio 2015
     ```
     C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\PrivateAssemblies\PreviewProcessingService.exe.config
     C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\PrivateAssemblies\RSPreviewPolicy.config
     C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\PrivateAssemblies\RSReportDesigner.config
     C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\PrivateAssemblies\RSReportHost.exe.config
     ```
     Visual Studio 2017
     ```
     C:\Program Files (x86)\Microsoft Visual Studio\2017\Enterprise\Common7\IDE\CommonExtensions\Microsoft\SSRS\PreviewProcessingService.exe.config
     C:\Program Files (x86)\Microsoft Visual Studio\2017\Enterprise\Common7\IDE\CommonExtensions\Microsoft\SSRS\RSPreviewPolicy.config
     C:\Program Files (x86)\Microsoft Visual Studio\2017\Enterprise\Common7\IDE\CommonExtensions\Microsoft\SSRS\RSReportDesigner.config
     C:\Program Files (x86)\Microsoft Visual Studio\2017\Enterprise\Common7\IDE\CommonExtensions\Microsoft\SSRS\RSReportHost.exe.config
     ```
     Visual Studio 2019
     ```
     C:\Program Files (x86)\Microsoft Visual Studio\2019\Enterprise\Common7\IDE\CommonExtensions\Microsoft\SSRS\PreviewProcessingService.exe.config
     C:\Program Files (x86)\Microsoft Visual Studio\2019\Enterprise\Common7\IDE\CommonExtensions\Microsoft\SSRS\RSPreviewPolicy.config
     C:\Program Files (x86)\Microsoft Visual Studio\2019\Enterprise\Common7\IDE\CommonExtensions\Microsoft\SSRS\RSReportDesigner.config
     C:\Program Files (x86)\Microsoft Visual Studio\2019\Enterprise\Common7\IDE\CommonExtensions\Microsoft\SSRS\RSReportHost.exe.config
     ```    
3.  Fügen Sie eine Codegruppe für die benutzerdefinierte Assembly hinzu. Weitere Informationen finden Sie unter [Sichere Entwicklung (Reporting Services)](../../reporting-services/extensions/secure-development/secure-development-reporting-services.md).  
  
## <a name="updating-custom-assemblies"></a>Aktualisieren von benutzerdefinierten Assemblys  
 Irgendwann kann es sein, dass Sie eine Version einer benutzerdefinierten Assembly aktualisieren müssen, auf die derzeit diverse veröffentlichten Berichte verweisen. Wenn die Assembly bereits im Bin-Verzeichnis des Berichtsservers oder des Berichts-Designers vorhanden ist und die Versionsnummer der Assembly erhöht oder geändert wird, funktionieren die aktuell veröffentlichten Berichte nicht mehr ordnungsgemäß. Sie müssen die Version der Assembly, auf die im **CodeModules**-Element der Berichtsdefinition verwiesen wird, aktualisieren und die Berichte erneut veröffentlichen. Wenn Sie wissen, dass Sie eine benutzerdefinierte Assembly häufig aktualisieren müssen und wenn Ihre derzeit veröffentlichten Berichte auf die neue Assembly verweisen müssen, sollten Sie überlegen, ob es nicht besser ist, dieselbe Versionsnummer in allen Updates einer bestimmten Assembly zu verwenden.  
  
 Wenn in den derzeit veröffentlichten Berichten nicht auf die neue Versionsnummer verwiesen werden muss, können Sie die benutzerdefinierte Assembly im globalen Assemblycache bereitstellen. Im globalen Assemblycache können mehrere Versionen derselben Assembly verwaltet werden, sodass aktuelle Berichte auf frühere Versionen der Assembly und neu veröffentliche Berichte auf die aktualisierte Assembly verweisen können. Ein weiteres Konzept wäre es, die bindende Umleitung des Berichtsservers so einzustellen, dass eine Umleitung aller Anforderungen für die alte Assembly zur neuen Assembly durchgesetzt würde. Sie müssten dann die Berichtsserver-Dateien „Web.config“ und „ReportingServicesService.exe.config“ ändern. Der Eintrag könnte folgendermaßen aussehen:  
  
```  
<configuration>  
   <runtime>  
      <assemblyBinding xmlns="urn:schemas-microsoft-com:asm.v1">  
         <dependentAssembly>  
            <assemblyIdentity name="myAssembly"  
                              publicKeyToken="32ab4ba45e0a69a1"  
                              culture="neutral" />  
            <bindingRedirect oldVersion="1.0.0.0"  
                             newVersion="2.0.0.0"/>  
         </dependentAssembly>  
      </assemblyBinding>  
   </runtime>  
</configuration>  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Verwenden benutzerdefinierter Assemblys mit Berichten](../../reporting-services/custom-assemblies/using-custom-assemblies-with-reports.md)   
 [Arbeiten mit Assemblys und dem globalen Assemblychache](/dotnet/framework/app-domains/working-with-assemblies-and-the-gac)  
  
