---
title: 'Datenbank-Engine: Breaking Changes | Microsoft-Dokumentation'
titleSuffix: SQL Server 2017
description: Informieren Sie sich über Änderungen, die zur Funktionsunfähigkeit von Anwendungen, Skripts oder Funktionen führen können, die auf früheren Versionen von SQL Server basieren.
ms.custom: seo-lt-2019
ms.date: 07/22/2020
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: release-landing
ms.topic: conceptual
helpviewer_keywords:
- breaking changes 2017 [SQL Server]
ms.assetid: ''
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=sql-server-2017||>=sql-server-linux-2017'
ms.openlocfilehash: ddb25e9fe9e4463d47b595c77cd6e24c7e0d0976
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171032"
---
# <a name="breaking-changes-to-database-engine-features-in-sssqlv14-md"></a>Wichtige Änderungen an Funktionen der Datenbank-Engine in [!INCLUDE[sssqlv14-md](../includes/sssqlv14-md.md)]
[!INCLUDE[SQL Server 2017](../includes/applies-to-version/sqlserver2017.md)]


  In diesem Artikel werden Breaking Changes in [!INCLUDE[sssqlv14-md](../includes/sssqlv14-md.md)] [!INCLUDE[ssDE](../includes/ssde-md.md)] beschrieben. Diese Änderungen können u. U. zur Funktionsunfähigkeit von Anwendungen, Skripts oder Funktionen führen, die auf früheren Versionen von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]basieren. Diese Probleme können nach einem Upgrade auftreten.  
  
## <a name="breaking-changes-in-sssqlv14-md-ssde"></a>Breaking Changes in [!INCLUDE[sssqlv14-md](../includes/sssqlv14-md.md)] [!INCLUDE[ssDE](../includes/ssde-md.md)]  
  
-  CLR verwendet die Codezugriffssicherheit (Code Access Security, CAS) im .NET Framework, die nicht länger als Sicherheitsbegrenzung unterstützt wird. Ab [!INCLUDE[sssqlv14-md](../includes/sssqlv14-md.md)][!INCLUDE[ssDE](../includes/ssde-md.md)] wird eine `sp_configure`-Option mit der Bezeichnung `clr strict security` eingeführt, um die Sicherheit von CLR-Assemblys zu erhöhen. CLR Strict Security ist standardmäßig aktiviert und behandelt `SAFE`- und `EXTERNAL_ACCESS`-Assemblys so, als wären Sie als `UNSAFE` gekennzeichnet. Die Option `clr strict security` kann für die Abwärtskompatibilität deaktiviert werden, es wird jedoch nicht empfohlen. Wenn `clr strict security` deaktiviert ist, kann eine CLR-Assembly, die mit `PERMISSION_SET = SAFE` erstellt wurde, möglicherweise auf externe Systemressourcen zugreifen, nicht verwalteten Code aufrufen und **sysadmin**-Privilegien erwerben. Nachdem Sie Strict Security aktiviert haben, können Assemblys, die nicht signiert sind, nicht geladen werden. Wenn eine Datenbank über `SAFE`- oder `EXTERNAL_ACCESS`-Assemblys verfügt, können `RESTORE`- oder `ATTACH DATABASE`-Anweisungen abgeschlossen werden, aber die Assemblys können möglicherweise nicht geladen werden.   
  Um die Assemblys zu laden, müssen Sie jede Assembly entweder bearbeiten, ablegen oder neu erstellen, damit sie mit einem Zertifikat oder asymmetrischen Schlüssel signiert ist, der über einen entsprechenden Anmeldenamen mit der `UNSAFE ASSEMBLY`-Berechtigung auf dem Server verfügt. Weitere Informationen finden Sie unter [CLR Strict Security](../database-engine/configure-windows/clr-strict-security.md). 
  
-  Die Algorithmen MD2, MD4, MD5, SHA und SHA-1 sind ab [!INCLUDE[ssSQL15](../includes/sssql16-md.md)] veraltet. Bis [!INCLUDE[ssSQL15](../includes/sssql16-md.md)] werden selbstsignierte Zertifikate mit SHA-1 erstellt. Ab [!INCLUDE[ssSQL17](../includes/sssql17-md.md)] werden selbstsignierte Zertifikate mit SHA-256 erstellt.

## <a name="previous-versions"></a><a name="previous-versions"></a> Vorgängerversionen  

- [Fehlerhafte Änderungen an Funktionen der Datenbank-Engine in SQL Server 2016](../database-engine/breaking-changes-to-database-engine-features-in-sql-server-2016.md)

- [Fehlerhafte Änderungen an Funktionen der Datenbank-Engine in SQL Server 2014](/previous-versions/sql/2014/database-engine/breaking-changes-to-database-engine-features-in-sql-server-2016?view=sql-server-2014&preserve-view=true#SQL14)

#### <a name="archived-documentation-for-very-old-versions-of-sql-server"></a>Archivierte Dokumentationen von sehr alten Versionen von SQL Server

[!INCLUDE[Archived documentation for very old versions of SQL Server](../includes/paragraph-content/previous-versions-archive-documentation-sql-server.md)]

## <a name="see-also"></a>Weitere Informationen  
 [Als veraltet markierte Funktionen der Datenbank-Engine in SQL Server 2016](../database-engine/deprecated-database-engine-features-in-sql-server-2016.md)   
 [Nicht mehr unterstützte Datenbank-Engine-Funktionalität in SQL Server 2016](./discontinued-database-engine-functionality-in-sql-server.md)   
 [Abwärtskompatibilität der SQL Server-Datenbank-Engine](./discontinued-database-engine-functionality-in-sql-server.md)   
 [ALTER DATABASE-Kompatibilitätsgrad &#40;Transact-SQL&#41;](../t-sql/statements/alter-database-transact-sql-compatibility-level.md)  
  
