---
title: Verwenden der Betriebssystemauthentifizierung | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- ODBC driver for Oracle [ODBC], authentication
- authentication [ODBC]
ms.assetid: 613daef7-3171-42d0-b7e3-3879280f864d
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 5a532c253ea2204fa3636c24c503cbefd3fa6311
ms.sourcegitcommit: 61381ef939415fe019285def9450d7583df1fed0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2018
ms.locfileid: "47686498"
---
# <a name="using-operating-system-authentication"></a>Verwenden der Betriebssystemauthentifizierung
> [!IMPORTANT]  
>  Dieses Feature wird in einer zukünftigen Version von Windows entfernt werden. Nutzen Sie diese Funktionen bei Neuentwicklungen nicht mehr, und planen Sie die Änderung von Anwendungen, die diese Funktion zurzeit verwenden. Verwenden Sie stattdessen den ODBC-Treiber, die von Oracle bereitgestellt.  
  
 Oracle-betriebssystemauthentifizierung basiert auf das zugrunde liegende Betriebssystem, zum Steuern des Zugriffs für Datenbankkonten. Benutzer müssen kein Kennwort eingibt, wenn dieser Typ des Anmeldenamens zu verwenden.  
  
 Um dieses Feature nutzen zu können, geben Sie "/" als die Benutzer-ID und ein Kennwort nicht angeben, beim Herstellen einer Verbindung mit einer der folgenden Verbindung APIs: [SQLBrowseConnect](../../odbc/microsoft/level-2-api-functions-odbc-driver-for-oracle.md), [SQLConnect](../../odbc/microsoft/core-level-api-functions-odbc-driver-for-oracle.md), oder [ SQLDriverConnect](../../odbc/microsoft/level-1-api-functions-odbc-driver-for-oracle.md).  
  
 Oracle-Datenbanken verwenden Sie SQL * Net Authentifizierungsdienste zum Authentifizieren von Benutzern, die angemeldet sind. Dieser Dienst funktioniert gut, wenn sich Benutzer bei Oracle über "sqlplus" angemeldet sind. Wenn der angemeldete Benutzer ein Dienst z. B. Internetinformationsdienste ist, schlägt die Authentifizierung jedoch fehl. Dies ist eine bekannte Einschränkung SQL\*Net-Authentifizierung und tritt folgender Fehler auf: "[Microsoft] [ODBC-Treiber für Oracle] [Oracle] ORA-12641: TNS:authentication-Dienst konnte nicht initialisiert werden."  
  
 Sie können dieses Problem beheben, indem Sie die Datei Sqlnet.ora bearbeiten. Diese Konfigurationsdatei wird in der Regel im Unterverzeichnis Network\Admin das Oracle home-Verzeichnis gespeichert. Fügen Sie die folgende Zeile hinzu, um Sqlnet.ora:  
  
```  
SQLNET.AUTHENTICATION_SERVICES = (none)  
```
