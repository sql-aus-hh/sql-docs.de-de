---
title: SQL Server Migration Assistant | Microsoft-Dokumentation
description: Hier finden Sie Informationen zu SQL Server Migration Assistant, einem Tool, das die Daten Bank Migration zu SQL Server von Microsoft Access, DB2, MySQL, Oracle und SAP ASE automatisiert.
ms.custom: ''
ms.date: 10/10/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: d0233525-a83b-4279-813e-c554042abd0e
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 2de9fc7fdc730cb09f96bff6633cab29521e175e
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596977"
---
# <a name="sql-server-migration-assistant"></a>SQL Server Migration Assistant

Microsoft SQL Server Migration Assistant (SSMA) ist ein Tool zur Automatisierung der Datenbankmigration von Microsoft Access, DB2, MySQL, Oracle und SAP ASE zu SQL Server.  
  
## <a name="migration-sources"></a>Migration von Datenquellen  
  
- [SQL Server Migration Assistant for Access](../ssma/access/sql-server-migration-assistant-for-access-accesstosql.md)  
  
- [SQL Server Migration Assistant for DB2](../ssma/db2/sql-server-migration-assistant-for-db2-db2tosql.md)  
  
- [SQL Server Migration Assistant for MySQL](../ssma/mysql/sql-server-migration-assistant-for-mysql-mysqltosql.md)  
  
- [SQL Server Migration Assistant for Oracle](../ssma/oracle/sql-server-migration-assistant-for-oracle-oracletosql.md)  
  
- [SQL Server Migration Assistant für SAP ASE](../ssma/sybase/sql-server-migration-assistant-for-sybase-sybasetosql.md)  

## <a name="supported-sources-and-target-versions"></a>Unterstützte Quellen und Ziel Versionen

Informationen zu unterstützten Quellen finden Sie im Download Center für den SSMA-Download.

Die folgenden Ziel Versionen werden für SSMA unterstützt.

- SQL Server 2012
- SQL Server 2014
- SQL Server 2016
- SQL Server 2017 unter Windows und Linux
- SQL Server 2019 unter Windows und Linux
- Azure SQL-Datenbank
- Verwaltete Azure SQL-Instanz
- Azure-Synapse-Analysen * *

* * Dieses Ziel wird nur von SSMA für Oracle unterstützt.

## <a name="downloads"></a>Downloads

- [SSMA für Access](https://aka.ms/ssmaforaccess)
- [SSMA für DB2](https://aka.ms/ssmafordb2)
- [SSMA für MySql](https://aka.ms/ssmaformysql)
- [SSMA für die Oracle](https://aka.ms/ssmafororacle)
- [SSMA für SAP ASE](https://aka.ms/ssmaforsybase)
 
## <a name="getting-ssma-support"></a>Erhalten von SSMA-Unterstützung  

**Hilfe und Unterstützung für Microsoft SQL Server Migration Assistant (SSMA):**  
  
- **Produkt Hilfe** : um auf den Produktsupport zuzugreifen, starten Sie SSMA, und wählen Sie das Menü Hilfe aus, oder drücken Sie die F1-Taste.  
  
- **SQL Server Community-Foren** : Stellen Sie eine Frage in der SQL Server Community  
  
  - [SQL Server Community](../sql-server/index.yml) -Newsgroups und Foren, die von der SQL Server Community überwacht werden. Auf dieser Seite sind auch Community-Informationsquellen aufgeführt, z. B. Blogs und Websites.  
  
  - [SQL Server Developer Center Community](../sql-server/index.yml) -Newsgroups, Foren und andere Communityressourcen, die für SQL Server Entwickler nützlich sind.  
  
- **Unterstützter Support** : Wechseln Sie zu, [https://support.microsoft.com/assistedsupportproducts](https://support.microsoft.com/assistedsupportproducts) und suchen Sie nach "SQL Server Migration Assistant".  Wählen Sie Ihre Version aus, und wählen Sie dann Anforderung starten aus.  Der unterstützte Support ist im SQL Server Migration Assistant Tool enthalten.  
  
- **Premier-Support** : Wenn Sie einen Premier-Vertrag haben, können Sie Premier Support im [Premier Online-Portal](https://premier.microsoft.com/)erhalten.  
  
- **Beratungsdienste** : Informationen zu den Partner gestützten Migrationen finden Sie im [Leitfaden zur Azure-Daten Bank Migration](https://datamigration.microsoft.com/).
  
## <a name="legal-notice-ssma"></a>Rechtliche Hinweise (SSMA)

Diese Dokumentation, einschließlich der darin enthaltenen Beispielanwendungen, wird nur zu Informationszwecken zur Verfügung gestellt. Es werden keine Gewährleistungen übernommen, weder ausdrücklich noch konkludent. Die in dieser Dokumentation enthaltenen Angaben und Daten, einschließlich URLs und anderen Verweisen auf Internetwebsites, können ohne vorherige Ankündigung geändert werden. Das vollständige Risiko der Nutzung oder der Ergebnisse der Nutzung dieser Dokumentation liegt bei dem Benutzer.  
  
Die in dieser Dokumentation enthaltenen Beispiele dienen in erster Linie der Veranschaulichung eines Konzepts oder der sinnvollen Verwendung einer bestimmten Anweisung oder Klausel. Die meisten Beispiele enthalten nicht den gesamten Code, der normalerweise in einem vollständigen Produktionssystem zu finden ist, da einige der üblichen Datenvalidierung und Fehlerbehandlung entfernt werden, um das Beispiel auf ein bestimmtes Konzept oder eine bestimmte Anweisung zu konzentrieren. Technischer Support ist für diese Beispiele oder den enthaltenen Quellcode nicht verfügbar.  
  
Die in den Beispielen verwendeten Namen von Firmen, Organisationen, Produkten, Domänen, Personen, Orten, Ereignissen sowie E-Mail-Adressen und Logos sind frei erfunden, soweit nichts anderes angegeben ist. Jede Ähnlichkeit mit tatsächlichen Firmen, Organisationen, Produkten, Domänen, Personen, Orten, Ereignissen, E-Mail-Adressen und Logos ist rein zufällig. Die Benutzer/innen sind verpflichtet, sich an alle anwendbaren Urheberrechtsgesetze zu halten. Unabhängig von der Anwendbarkeit der entsprechenden Urheberrechtsgesetze darf ohne ausdrückliche schriftliche Erlaubnis der Microsoft Corporation kein Teil dieser Dokumentation für irgendwelche Zwecke vervielfältigt oder in einem Datenempfangssystem gespeichert oder eingelesen werden, unabhängig davon, auf welche Art und Weise oder mit welchen Mitteln (elektronisch, mechanisch, durch Fotokopieren, Aufzeichnen usw.) dies geschieht.  
  
Es ist möglich, dass Microsoft Rechte an Patenten bzw. angemeldeten Patenten, an Marken, Urheberrechten oder sonstigem geistigen Eigentum besitzt, die sich auf den fachlichen Inhalt dieser Dokumentation beziehen. Das Bereitstellen dieser Dokumentation gibt Ihnen jedoch keinen Anspruch auf diese Patente, Marken, Urheberrechte oder auf sonstiges geistiges Eigentum, es sei denn, dies wird ausdrücklich in den schriftlichen Lizenzverträgen von Microsoft eingeräumt.  
  
© 2019 Microsoft Corporation. Alle Rechte vorbehalten.  
  
Microsoft, Windows, Windows NT, Windows Server, Active Directory, ActiveX, BackOffice, bCentral, BizTalk, DirectX, Excel, Hotmail, IntelliSense, J/Direct, Jscript, Microsoft Press, MSDN, MS-DOS, Outlook, PivotChart, PivotTable, PowerPoint, SharePoint, SQL Server, Visual Basic, Visual C#, Visual C++, Visual FoxPro, Visual InterDev, Visual J#, Visual J++, Visual SourceSafe, Visual Studio, Win32, Win32s, Windows Mobile, Windows Server System und WinFX sind entweder eingetragene Marken oder Marken der Microsoft Corporation in den USA und/oder anderen Ländern und Regionen.  
  
SAP NetWeaver ist eine eingetragene Marke der SAP AG in Deutschland und/oder anderen Ländern und Regionen.  
  
Alle anderen Marken sind Eigentum ihrer jeweiligen Inhaber.  
  
## <a name="documentation-policy-for-sql-server-support-and-upgrade"></a>Dokumentationsrichtlinie für den Support von und das Upgrade auf SQL Server

In der SQL Server-Dokumentation werden Inhalte erst veröffentlicht, nachdem ausreichend Tests durchgeführt wurden. Produktdokumentation-SQL Server-Onlinedokumentation, Info Dateien, Dokumente zu bekannten Problemen und Knowledge Base-Artikel: enthält Inhalte zu SQL Server Features und Funktionen, die robust genug sind, um für die allgemeine Verwendung durch alle Kunden sicher zu sein. Diese Richtlinie gilt für alle SQL Server-Dokumentationen, einschließlich der Infodateien zu Versionen und Service Packs. Eine Infodatei wird als Erweiterung der Onlinedokumentation betrachtet.  
  
Es gibt Fälle, in denen bestimmte Funktionen von Kunden nicht direkt verwendet werden sollten, weshalb diese nicht dokumentiert sind. Sofern ein Feature nicht auch in der SQL Server-Dokumentation von Microsoft beschrieben wird, werden Inhalte in Dokumentationen oder auf Websites von Drittanbietern vom Microsoft-Kundendienst nicht unterstützt und sollte nicht in Produktionsdatenbanken oder Anwendungen verwendet werden.  
  
Kunden sollten keine nicht dokumentierten APIs nutzen, was u. a. für folgende Elemente gilt: gespeicherte Prozeduren, erweiterte gespeicherte Prozeduren, Funktionen, Ansichten, Tabellen, Spalten, Eigenschaften und Metadaten. Der Microsoft-Kundensupport unterstützt keine Datenbanken oder Anwendungen, die nicht dokumentierte Einstiegspunkte nutzen oder verwenden.  
  
Server- und Datenbankupgrades auf zukünftige Versionen von SQL Server werden nicht für Anwendungen und Datenbanken garantiert, die nicht dokumentierte Einstiegspunkte nutzen und verwenden. Die Verwendung von SQL Server-Features und -Funktionen muss auf solche beschränkt sein, die in der Microsoft SQL Server-Dokumentation enthalten sind. Wenn eine Funktion nicht in der Microsoft SQL Server-Dokumentation erfasst ist, zählt sie nicht zum unterstützten Teil von SQL Server.