---
title: Installation
description: SQL Server Native Client 11,0 wird mit SQL Server 2016 installiert. Erfahren Sie, wo Komponenten installiert sind. Es ist auch ein verteilbares Installationsprogramm vorhanden.
ms.custom: ''
ms.date: 07/15/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: native-client
author: markingmyname
ms.author: maghan
ms.topic: reference
helpviewer_keywords:
- SQL Server Native Client, uninstalling
- SQLNCLI, installing
- SQLNCLI, uninstalling
- Setup [SQL Server Native Client]
- uninstalling SQL Server Native Client
- data access [SQL Server Native Client], uninstalling SQL Server Native Client
- installing SQL Server Native Client
- SQL Server Native Client, installing
- data access [SQL Server Native Client], installing SQL Server Native Client
- removing SQL Server Native Client
ms.assetid: c6abeab2-0052-49c9-be79-cfbc50bff5c1
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 73c8e7f7656c927f65a6fb1b1989f14110e4e7b4
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171302"
---
# <a name="installing-sql-server-native-client"></a>Installieren von SQL Server Native Client
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]


  Microsoft [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 11.0 wird bei der Installation von [!INCLUDE[ssSQL15](../../../includes/sssql16-md.md)] installiert. 
 
 Es ist kein SQL Server 2016 Native Client vorhanden. Weitere Informationen finden Sie unter [SQL Server Native Client](../../../relational-databases/native-client/sql-server-native-client.md). 
 
Sie können sqlncli.msi auch von der SQL Server 2012 Feature Pack-Webseite herunterladen. Wenn Sie die neueste Version des SQL Server Native Client herunterladen möchten, besuchen Sie das [Microsoft® SQL Server® 2012 Feature Pack](https://www.microsoft.com/download/details.aspx?id=56041). Wenn eine frühere Version von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client vor SQL Server 2012 auch auf dem Computer installiert ist, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] wird Native Client 11,0 parallel zur früheren Version installiert.  
  
 Die [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client-Dateien (sqlncli11.dll, sqlnclir11.rll und s11ch_sqlncli.chm) werden in folgendem Verzeichnis installiert:  
  
 `%SYSTEMROOT%\system32\`  
  
> [!NOTE]  
>  Während des Installationsvorgangs werden alle erforderlichen Registrierungseinstellungen für den OLE DB-Anbieter von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client und den ODBC-Treiber von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client vorgenommen.  
  
 Die [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client-Header- und -Bibliotheksdateien (sqlncli.h und sqlncli11.lib) werden in folgendem Verzeichnis installiert:  
  
 `%PROGRAMFILES%\Microsoft SQL Server\110\SDK`  
  
 Neben der Installation von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client im Rahmen der Installation von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] gibt es die Möglichkeit der Installation mithilfe des verteilbaren Installationsprogramms sqlncli.msi, das sich auf dem Installationsdatenträger von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] in folgendem Verzeichnis befindet: `%CD%\Setup\`.  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client können Sie über sqlncli.msi verteilen. Möglicherweise müssen Sie [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client bei der Bereitstellung einer Anwendung installieren. Eine Möglichkeit, mehrere Pakete in mehreren Installationen, die für den Benutzer wie eine Installation aussehen, zu installieren, besteht in der Verwendung der Chainer- und Bootstrappertechnologie. Weitere Informationen finden Sie unter [Authoring a Custom Bootstrapper Package for Visual Studio 2005 (Erstellen eines benutzerdefinierten Bootstrapper-Pakets für Visual Studio 2005)](/previous-versions/aa730839(v=vs.80)) und [Adding Custom Prerequisites (Hinzufügen benutzerdefinierter Voraussetzungen)](/visualstudio/deployment/creating-bootstrapper-packages).  
  
 Mit der x64-Version und der Itanium-Version von sqlncli.msi wird auch die 32-Bit-Version von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client installiert. Wenn Ihre Anwendung für eine andere Plattform als für diejenige vorgesehen ist, auf der sie entwickelt wurde, können Sie Versionen von sqlncli.msi für x64, Itanium und x86 vom Microsoft Download Center herunterladen.  
  
 Wenn Sie sqlncli.msi aufrufen, werden nur die Clientkomponenten standardmäßig installiert. Die Client Komponenten sind Dateien, die das Ausführen einer Anwendung unterstützen, die mit [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client entwickelt wurde. Um auch die SDK-Komponenten zu installieren, geben Sie `ADDLOCAL=All` in der Befehlszeile an. Beispiel:  
  
 `msiexec /i sqlncli.msi ADDLOCAL=ALL APPGUID={0CC618CE-F36A-415E-84B4-FB1BFF6967E1}`  
  
## <a name="silent-install"></a>Installation ohne Eingreifen  
 Wenn Sie die /passiv, /qn, /qb, oder /qr-Option mit msiexec verwenden, müssen Sie auch IACCEPTSQLNCLILICENSETERMS=YES angeben, um explizit anzuzeigen, dass Sie den Bestimmungen der Endbenutzerlizenz zustimmen. Diese Option muss in Großbuchstaben angegeben werden.  
  
## <a name="uninstalling-sql-server-native-client"></a>Deinstallieren von SQL Server Native Client  
 Da Anwendungen wie [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Server und die [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Tools von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client abhängig sind, ist es wichtig, Native Client erst zu deinstallieren, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Wenn alle abhängigen Anwendungen deinstalliert wurden. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]Verwenden Sie die APPGUID-Installationsoption in ihrer MSI wie folgt, um Benutzer mit einer Warnung zu erhalten, dass Ihre Anwendung von Native Client abhängig ist:  
  
 `msiexec /i sqlncli.msi APPGUID={0CC618CE-F36A-415E-84B4-FB1BFF6967E1}`  
  
 Der an APPGUID übergebene Wert ist Ihr spezifischer Produktcode. Der Produktcode muss beim Packen des Setupprogramms für die Anwendung mit Microsoft Installer erstellt werden.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Entwickeln von Anwendungen mit SQL Server Native Client](../../../relational-databases/native-client/applications/installing-sql-server-native-client.md)   
 [Themen zu Vorgehensweisen für die Installation](/previous-versions/sql/)  
  
