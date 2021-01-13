---
title: Schützen von Verbindungsinformationen
description: Erfahren Sie mehr über Sicherheitsrisiken in Verbindungszeichenfolgen, die durch die Zusammensetzung und Persistenz von Verbindungszeichenfolgen und den Authentifizierungstyp verursacht werden können.
ms.date: 11/13/2020
ms.assetid: 1471f580-bcd4-4046-bdaf-d2541ecda2f4
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 1d170d712269bf169d069ef7b93f975de855f8ec
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771402"
---
# <a name="protecting-connection-information"></a>Schützen von Verbindungsinformationen

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Eines der wichtigsten Ziele beim Sichern einer Anwendung besteht darin, den Zugriff auf die Datenquelle zu schützen. Eine Verbindungszeichenfolge stellt ein potenzielles Sicherheitsrisiko dar, wenn sie nicht gesichert wird. Das Speichern von Verbindungsinformationen als Klartext oder das Aufbewahren dieser Informationen im Arbeitsspeicher gefährdet Ihr gesamtes System. Die in Ihrem Quellcode eingebetteten Verbindungszeichenfolgen können mit [Ildasm.exe (IL Disassembler)](/dotnet/framework/tools/ildasm-exe-il-disassembler) gelesen werden, um MSIL (Microsoft Intermediate Language) in einer kompilierten Assembly anzuzeigen.

Welche Sicherheitsrisiken sich im Zusammenhang mit Verbindungszeichenfolgen ergeben, hängt von der Art der verwendeten Authentifizierung, der Art und Weise, wie die Verbindungszeichenfolgen im Arbeitsspeicher und auf der Festplatte aufbewahrt werden, und den Verfahren ab, mit denen die Verbindungszeichenfolgen zur Laufzeit konstruiert werden.

## <a name="use-windows-authentication"></a>Windows-Authentifizierung verwenden

Damit der Zugriff auf die Datenquelle geschützt bleibt, müssen Sie Verbindungsinformationen wie Benutzer-ID, Kennwort und den Namen der Datenquelle sichern. Um zu vermeiden, dass Benutzerinformationen verfügbar gemacht werden, empfehlen wir nach Möglichkeit überall die Windows-Authentifizierung (auch als *integrierte Sicherheit* bezeichnet) zu verwenden. Die Windows-Authentifizierung wird in einer Verbindungszeichenfolge mit dem Schlüsselwort `Integrated Security` oder `Trusted_Connection` angegeben, sodass weder eine Benutzer-ID noch ein Kennwort verwendet werden muss. Bei Verwendung der Windows-Authentifizierung werden die Benutzer von Windows authentifiziert, und der Zugriff auf Server- und Datenbankressourcen wird durch die Erteilung von Berechtigungen für Windows-Benutzer und -Gruppen gesteuert.

Dort, wo die Windows-Authentifizierung nicht verwendet werden kann, müssen Sie besondere Vorsicht walten lassen, da die Verbindungszeichenfolge Benutzeranmeldeinformationen enthält, die offen zugänglich sind. In ASP.NET-Anwendungen können Sie ein Windows-Konto als feste Identität konfigurieren, die dann für Verbindungen mit Datenbanken und anderen Netzwerkressourcen verwendet wird. Sie aktivieren im Element „Identity“ in der Datei **web.config** den Identitätswechsel und geben einen Benutzernamen und ein Kennwort an.

```xml  
<identity impersonate="true"
        userName="MyDomain\UserAccount"
        password="*****" />  
```  

Das Konto mit der festen Identität sollte nur über die nötigsten Berechtigungen für die Datenbank verfügen. Außerdem sollten Sie die Konfigurationsdatei verschlüsseln, sodass der Benutzername und das Kennwort nicht als Klartext verfügbar gemacht werden.

## <a name="avoid-injection-attacks-with-connection-string-builders"></a>Vermeiden von Injection-Angriffen mit Verbindungszeichenfolgen-Generatoren

Zu einem Angriff durch Einschleusen von Verbindungszeichenfolgen kann es kommen, wenn die dynamische Zeichenfolgenverkettung verwendet wird, um auf Benutzereingabe basierende Verbindungszeichenfolgen zu konstruieren. Wenn die Benutzereingabe nicht validiert wird und schädlicher Text (oder schädliche Zeichen) nicht maskiert wird, kann ein Angreifer möglicherweise auf sicherheitsrelevante Daten oder andere Ressourcen auf dem Server zugreifen. Zur Vermeidung dieses Problems enthält der Microsoft SqlClient-Datenanbieter für SQL Server nun eine neue Verbindungszeichenfolgen-Generatorklasse, mit deren Hilfe die Syntax von Verbindungszeichenfolgen validiert und sichergestellt werden kann, dass keine zusätzlichen Parameter eingeschleust werden. Weitere Informationen finden Sie in [Connection String Builders (Verbindungszeichenfolgengeneratoren)](connection-string-builders.md).

## <a name="use-persist-security-infofalse"></a>Verwenden von „Persist Security Info=false“

Die Standardeinstellung für `Persist Security Info` ist false. Wir empfehlen, diesen Standardwert in allen Verbindungszeichenfolgen beizubehalten. Wenn Sie `Persist Security Info` auf `true` oder `yes` festlegen, können sicherheitsrelevante Informationen, wie die Benutzer-ID und das Kennwort, über die Verbindung abgerufen werden, nachdem diese geöffnet wurde. Wenn Sie `Persist Security Info` hingegen auf `false` oder `no` festlegen, werden die sicherheitsrelevanten Informationen verworfen, sobald die Verbindung mit ihrer Hilfe geöffnet wurde. Auf diese Weise wird dafür gesorgt, dass nicht vertrauenswürdige Quellen nicht auf sicherheitsrelevante Informationen zugreifen können.

## <a name="encrypt-configuration-files"></a>Verschlüsseln von Konfigurationsdateien

[!INCLUDE[appliesto-netfx-netcore-xxxx-md](../../includes/appliesto-netfx-netcore-xxxx-md.md)]

Sie können die Verbindungszeichenfolgen auch in Konfigurationsdateien speichern. Damit erübrigt sich die Notwendigkeit, die Verbindungszeichenfolgen in den Code Ihrer Anwendung einzubetten. Konfigurationsdateien sind Standard-XML-Dateien, für die .NET Framework einen allgemeinen Satz von Elementen definiert hat. Verbindungszeichenfolgen in Konfigurationsdateien werden bei Windows-Anwendungen in der Regel im Element **\<connectionStrings>** in der Datei **app.config** gespeichert. Bei ASP.NET-Anwendungen erfolgt die Speicherung normalerweise in der Datei **web.config**. Weitere Informationen zu den Grundlagen des Speicherns, Abrufens und Verschlüsselns von Verbindungszeichenfolgen in Konfigurationsdateien finden Sie unter [Verbindungszeichenfolgen und Konfigurationsdateien](connection-strings-and-configuration-files.md).

## <a name="see-also"></a>Weitere Informationen

- [Verschlüsseln von Konfigurationsinformationen mithilfe der geschützten Konfiguration](/previous-versions/aspnet/53tyfkaw(v=vs.100))
- [Microsoft ADO.NET für SQL Server](microsoft-ado-net-sql-server.md)
