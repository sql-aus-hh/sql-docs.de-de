---
title: SQL Server-Sicherheit
description: Bietet eine Übersicht über die SQL Server-Sicherheitsfunktionen und Anwendungsszenarios zum Erstellen sicherer ADO.NET-Anwendungen, die auf SQL Server zugreifen.
ms.date: 09/26/2019
ms.assetid: 9053724d-a1fb-4f0f-b9dc-7f6dd893e8ff
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-kaywon
ms.openlocfilehash: e70f6da7755b28c400cc80a5ee6de79a004e4e42
ms.sourcegitcommit: 2add15a99df7b85d271adb261523689984dfd134
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2020
ms.locfileid: "97038918"
---
# <a name="sql-server-security"></a>SQL Server-Sicherheit

[!INCLUDE[Driver_ADONET_Download](../../../includes/driver_adonet_download.md)]

SQL Server besitzt viele Funktionen, die das Erstellen sicherer Datenbankanwendungen unterstützen.  
  
Allgemeine Sicherheitsfragen, wie der Schutz vor Datendiebstahl oder Vandalismus, bleiben jedoch ungeachtet der jeweils verwendeten SQL Server-Version immer aktuell. Datenintegrität muss auch als Sicherheitsaspekt berücksichtigt werden. Wenn Daten nicht geschützt sind, können sie möglicherweise wertlos werden, sobald eine Ad-hoc-Manipulation von Daten erlaubt ist und die Daten versehentlich oder böswillig mit falschen Werten versehen oder ganz gelöscht werden. Darüber hinaus gibt es häufig gesetzliche Bestimmungen, die eingehalten werden müssen, wie z. B. die ordnungsgemäße Speicherung vertraulicher Informationen. Die Speicherung einiger Arten personenbezogener Daten ist je nach den in einem bestimmten Rechtsraum geltenden Gesetzen gänzlich verboten.  
  
Genau wie jede Windows-Version besitzt auch jede SQL Server-Version andere Sicherheitsfunktionen, wobei der Funktionsumfang in höheren Versionen dem früherer Versionen überlegen ist. Wichtig ist zu verstehen, dass Sicherheitsvorkehrungen allein keine sichere Datenbankanwendung garantieren können. Jede Datenbankanwendung ist hinsichtlich ihrer Anforderungen, der Ausführungsumgebung, des Bereitstellungsmodells, des physischen Standorts und der Benutzergruppe einzigartig. Einige Anwendungen, die lokal begrenzt sind, benötigen nur minimale Sicherheit, während andere lokale Anwendungen oder Anwendungen, die über das Internet bereitgestellt werden, möglicherweise strenge Sicherheitsvorkehrungen und eine ständige Überwachung und Einschätzung erfordern.  
  
Welche Sicherheitsanforderungen eine SQL Server-Datenbankanwendung erfüllen muss, sollte bereits in der Entwurfsphase geklärt werden. Die Einschätzung von Bedrohungen in einem frühen Stadium des Entwicklungszyklus gibt Ihnen die Möglichkeit, potenzielle Schäden einzudämmen, wo auch immer ein Sicherheitsrisiko entdeckt wird.  
  
Selbst wenn der anfängliche Entwurf einer Anwendung einwandfrei ist, können bei der Weiterentwicklung des Systems neue Bedrohungen auftauchen. Durch die Schaffung mehrerer Verteidigungslinien rund um Ihre Datenbank können Sie den durch eine Sicherheitsverletzung verursachten Schaden minimieren. Ihre erste Verteidigungslinie besteht darin, die Angriffsfläche zu reduzieren, indem Sie auf keinen Fall mehr Berechtigungen als unbedingt notwendig erteilen.  
  
In den Themen in diesem Abschnitt werden die für Entwickler relevanten Sicherheitsfunktionen in SQL Server kurz umrissen. Außerdem finden Sie hier Links zu entsprechenden Themen in der SQL Server-Onlinedokumentation und anderen Ressourcen mit ausführlicheren Informationen.  
  
## <a name="in-this-section"></a>In diesem Abschnitt  
[Authentifizierung in SQL Server](authentication-sql-server.md)  
Beschreibt die Anmeldung und Authentifizierung in SQL Server und bietet Links zu weiteren Ressourcen. 

[Authentifizierung über Azure Active Directory](azure-active-directory-authentication.md)  
In diesem Artikel wird die Verwendung unterstützter Azure Active Directory-Authentifizierungsmodi zum Herstellen einer Verbindung mit Azure SQL-Datenquellen mit SqlClient beschrieben.
  
[Anwendungssicherheitsszenarios in SQL Server](application-security-scenarios-sql-server.md)  
Enthält Themen, in denen verschiedene Anwendungssicherheitsszenarien für ADO.NET- und SQL Server-Anwendungen erläutert werden.  
  
[SQL Server Express-Sicherheit](sql-server-express-security.md)  
Beschreibt Sicherheitsüberlegungen für SQL Server Express.  
  
## <a name="related-sections"></a>Verwandte Abschnitte  
[Sicherheitscenter für SQL Server-Datenbank-Engine und Azure SQL-Datenbank](../../../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md)  
Beschreibt Sicherheitsüberlegungen für SQL Server und Azure SQL-Datenbank.

[Überlegungen zur Sicherheit bei SQL Server-Installationen](../../../sql-server/install/security-considerations-for-a-sql-server-installation.md)  
Erörtert Sicherheitsbedenken, die vor der Installation von SQL Server zu berücksichtigen sind.

## <a name="next-steps"></a>Nächste Schritte
- [SQL Server und ADO.NET](index.md)
