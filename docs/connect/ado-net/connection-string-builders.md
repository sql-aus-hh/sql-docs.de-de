---
title: Verbindungszeichenfolgen-Generatoren
description: Erfahren Sie mehr über die Verbindungszeichenfolgen-Generatorklassen, die für verschiedene Anbieter in ADO.NET verwendet werden und die alle von DbConnectionStringBuilder erben.
ms.date: 11/13/2020
dev_langs:
- csharp
ms.assetid: 8434b608-c4d3-43d3-8ae3-6d8c6b726759
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 38c1ad34fb4819460c08f00da4c1dbd31749b7cb
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771432"
---
# <a name="connection-string-builders"></a>Verbindungszeichenfolgen-Generatoren

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

In früheren ADO.NET-Versionen erfolgte bei der Kompilierung keine Syntaxprüfung für verkettete Zeichenfolgenwerte, sodass bei einem falschen Schlüsselwort zur Laufzeit eine <xref:System.ArgumentException> generiert wurde. Die Microsoft SqlClient-Datenanbieter für SQL Server enthält die stark typisierte Verbindungszeichenfolgen-Generatorklasse <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder?displayProperty=nameWithType>, die von <xref:System.Data.Common.DbConnectionStringBuilder> erbt.

## <a name="connection-string-injection-attacks"></a>Injection-Angriffe mit Verbindungszeichenfolgen

Zu einem Angriff durch Einschleusen von Verbindungszeichenfolgen kann es kommen, wenn die dynamische Zeichenfolgenverkettung verwendet wird, um auf Benutzereingabe basierende Verbindungszeichenfolgen zu konstruieren. Wenn die Zeichenfolge nicht validiert wird und schädlicher Text oder schädliche Zeichen nicht maskiert werden, kann ein Angreifer möglicherweise auf sicherheitsrelevante Daten oder andere Ressourcen auf dem Server zugreifen. So könnte ein Angreifer z. B. einen Angriff starten, indem er ein Semikolon, gefolgt von einem weiteren Wert, einfügt. Die Verbindungszeichenfolge wird mit einem **Last One Wins**-Algorithmus (der Letzte gewinnt) analysiert, wobei die feindliche Eingabe durch den legitimen Wert ersetzt wird.

Die ConnectionStringBuilder-Klassen sorgen dafür, dass das Rätselraten abgeschafft wird und die Verbindungszeichenfolgen vor Syntaxfehlern und Sicherheitslücken geschützt werden. Sie stellen Methoden und Eigenschaften bereit, die den vom jeweiligen Datenanbieter zugelassenen bekannten Schlüssel-Wert-Paaren entsprechen. Jede Klasse unterhält eine feste Sammlung von Synonymen und ist in der Lage, ein Synonym in den entsprechenden bekannten Schlüsselnamen zu übersetzen. Die Gültigkeit der Schlüssel/Wert-Paare wird geprüft, und im Falle eines ungültigen Paars wird eine Ausnahme ausgelöst. Außerdem werden eingeschleuste Werte auf sichere Weise behandelt.

Das folgende Beispiel zeigt, wie der <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder> einen eingefügten Zusatzwert für die `Initial Catalog`-Einstellung behandelt.

[!code-csharp[SqlConnectionStringBuilder_InjectionAttack#1](~/../sqlclient/doc/samples/SqlConnectionStringBuilder_InjectionAttack.cs#1)]

Die Ausgabe zeigt, dass <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder> ihn ordnungsgemäß behandelt hat, indem der zusätzliche Wert in doppelte Anführungszeichen gesetzt wurde, anstatt ihn als neues Schlüssel-Wert-Paar an die Verbindungszeichenfolge anzufügen.

```output
data source=(local);Integrated Security=True;
initial catalog="AdventureWorks;NewValue=Bad"
```

## <a name="build-connection-strings-from-configuration-files"></a>Erstellen von Verbindungszeichenfolgen aus Konfigurationsdateien

Wenn bestimmte Elemente einer Verbindungszeichenfolge vorab bekannt sind, können sie in einer Konfigurationsdatei gespeichert und zur Laufzeit zum Konstruieren einer vollständigen Verbindungszeichenfolge abgerufen werden. So ist es z. B. denkbar, dass zwar der Name der Datenbank, nicht aber der Name des Servers vorab bekannt ist. Vielleicht möchten Sie auch, dass die Benutzer zur Laufzeit zwar einen Namen und ein Kennwort bereitstellen, aber keine anderen Werte in die Verbindungszeichenfolge einfügen können.

Einer der überladenen Konstruktoren für einen Verbindungszeichenfolgen-Generator verwendet eine <xref:System.String> als Argument, sodass Sie eine Teilverbindungszeichenfolge bereitstellen können, die dann zur Vervollständigung der Benutzereingabe verwendet wird. Die Teilverbindungszeichenfolge kann in einer Konfigurationsdatei gespeichert und zur Laufzeit abgerufen werden.

> [!NOTE]
> Der <xref:System.Configuration>-Namespace ermöglicht den programmgesteuerten Zugriff auf Konfigurationsdateien unter Verwendung des <xref:System.Web.Configuration.WebConfigurationManager> bei Webanwendungen und des <xref:System.Configuration.ConfigurationManager> bei Windows-Anwendungen. Weitere Informationen zum Arbeiten mit Verbindungszeichenfolgen und Konfigurationsdateien finden Sie unter [Verbindungszeichenfolgen und Konfigurationsdateien](connection-strings-and-configuration-files.md).

### <a name="example"></a>Beispiel

Dieses Beispiel zeigt, wie Sie durch Festlegen der Eigenschaften <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder.DataSource%2A>, <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder.UserID%2A> und <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder.Password%2A> des <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder> eine Teilverbindungszeichenfolge aus einer Konfigurationsdatei abrufen und vervollständigen können. Die Konfigurationsdatei ist wie folgt definiert:

```xml
<connectionStrings>
  <clear/>
  <add name="partialConnectString"
    connectionString="Initial Catalog=Northwind;"
    providerName="Microsoft.Data.SqlClient" />
</connectionStrings>
```

> [!NOTE]
> Sie müssen in Ihrem Projekt einen Verweis auf `System.Configuration.dll` angeben, damit der Code ausgeführt wird.

[!code-csharp[DataWorks SqlConnectionStringBuilder.UserNamePwd#1](~/../sqlclient/doc/samples/SqlConnectionStringBuilder_UserNamePwd.cs#1)]
  
## <a name="see-also"></a>Weitere Informationen

- [Verbindungszeichenfolgen](connection-strings.md)
- [Microsoft ADO.NET für SQL Server](microsoft-ado-net-sql-server.md)
