---
title: Verbindungszeichenfolgen
description: Informieren Sie sich über Verbindungszeichenfolgen im Microsoft SqlClient-Datenanbieter für SQL Server mit Initialisierungsinformationen, die als Parameter von einem Datenanbieter an eine Datenquelle übergeben werden.
ms.date: 11/13/2020
ms.assetid: 745c5f95-2f02-4674-b378-6d51a7ec2490
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 257ed7d43f8ab204c7c7e7575c69251be6f2efdf
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771626"
---
# <a name="connection-strings-in-adonet"></a>Verbindungszeichenfolgen in ADO.NET

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Eine Verbindungszeichenfolge enthält Initialisierungsinformationen, die als Parameter von einem Datenanbieter an eine Datenquelle übergeben werden. Der Datenanbieter empfängt die Verbindungszeichenfolge als Wert der <xref:System.Data.Common.DbConnection.ConnectionString?displayProperty=nameWithType>-Eigenschaft. Der Anbieter analysiert die Verbindungszeichenfolge und stellt sicher, dass die Syntax stimmt und die Schlüsselwörter unterstützt werden. Anschließend übergibt die <xref:System.Data.Common.DbConnection.Open?displayProperty=nameWithType>-Methode die analysierten Verbindungsparameter an die Datenquelle. Die Datenquelle führt eine weitere Prüfung durch und stellt eine Verbindung her.

## <a name="connection-string-syntax"></a>Syntax von Verbindungszeichenfolgen

Eine Verbindungszeichenfolge besteht aus einer durch Semikolons getrennten Liste mit Paaren von Schlüssel-Wert-Parametern:

```csharp
keyword1=value; keyword2=value;
```

Bei Schlüsselwörtern muss die Groß-/Kleinschreibung nicht beachtet werden. Allerdings muss bei Werten je nach Datenquelle möglicherweise Groß-/Kleinschreibung beachtet werden. Sowohl Schlüsselwörter als auch Werte können [Leerzeichen](https://en.wikipedia.org/wiki/Whitespace_character#Unicode) enthalten. Führende und nachstehende Leerzeichen werden in Schlüsselwörtern und Werten ohne Anführungszeichen ignoriert.

Wenn ein Wert das Semikolon, [Unicode-Steuerzeichen](https://en.wikipedia.org/wiki/Unicode_control_characters) oder führende oder nachstehende Leerzeichen enthält, muss er von einfachen oder doppelten Anführungszeichen umschlossen werden. Beispiel:

```csharp
Keyword=" whitespace  ";
Keyword='special;character';
```

Das umschließende Zeichen darf nicht im Wert vorkommen, den es umschließt. Daher kann ein Wert, der einfache Anführungszeichen enthält, nur von doppelten Anführungszeichen umschlossen werden und umgekehrt:

```csharp
Keyword='double"quotation;mark';
Keyword="single'quotation;mark";
```

Sie können das umschließende Zeichen auch mit Escapezeichen versehen, indem Sie zwei davon zusammen verwenden:

```csharp
Keyword="double""quotation";
Keyword='single''quotation';
```

Sowohl die Anführungszeichen selbst als auch das Gleichheitszeichen müssen nicht mit Escapezeichen versehen werden, sodass die folgenden Verbindungszeichenfolgen gültig sind:

```csharp
Keyword=no "escaping" 'required';
Keyword=a=b=c
```

Da jeder Wert bis zum nächsten Semikolon bzw. bis zum Ende einer Zeichenfolge gelesen wird, ist der Wert im letzteren Beispiel `a=b=c` und das abschließende Semikolon optional.

Alle Verbindungszeichenfolgen haben dieselbe grundlegende Syntax, die zuvor beschrieben wurde. Die Gruppe erkannter Schlüsselwörter hängt vom Anbieter ab. Der *Microsoft SqlClient*-Datenanbieter für *SQL Server* unterstützt viele Schlüsselwörter älterer APIs, ist jedoch im Allgemeinen flexibler und akzeptiert Synonyme für viele der gängigen Schlüsselwörter in Verbindungszeichenfolgen.

Tippfehler können Fehler verursachen. Beispielsweise ist `Integrated Security=true` gültig, aber `IntegratedSecurity=true` verursacht einen Fehler.

Verbindungszeichenfolgen, die zur Laufzeit manuell anhand nicht überprüfter Benutzereingaben erstellt werden, sind anfällig für Zeichenfolgeneinschleusungs-Angriffe und gefährden die Sicherheit der Datenquelle. Um diese Probleme zu lösen, wurde die <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder>-Klasse erstellt. Dieser [Verbindungszeichenfolgen-Generator](connection-string-builders.md)-Klasse macht Parameter als stark typisierte Eigenschaften verfügbar und ermöglicht es, die Verbindungszeichenfolge zu überprüfen, bevor sie zur Datenquelle gesendet wird.

## <a name="in-this-section"></a>In diesem Abschnitt

[Verbindungszeichenfolgen-Generator](connection-string-builders.md)\
Zeigt, wie zur Laufzeit mithilfe der `ConnectionStringBuilder`-Klasse gültige Verbindungszeichenfolgen erstellt werden können.

[Verbindungszeichenfolgen und Konfigurationsdateien](connection-strings-and-configuration-files.md)\
Zeigt, wie Verbindungszeichenfolgen in Konfigurationsdateien gespeichert und abgerufen werden können.

[Syntax der Verbindungszeichenfolge](connection-string-syntax.md)\
Beschreibt, wie anbieterspezifische Verbindungszeichenfolgen für `SqlClient`konfiguriert werden.

[Schützen von Verbindungsinformationen](protecting-connection-information.md)\
Demonstriert Verfahren zum Schützen von Informationen, die beim Herstellen von Verbindungen mit einer Datenquelle verwendet werden.

## <a name="see-also"></a>Weitere Informationen:

- [Microsoft ADO.NET für SQL Server](microsoft-ado-net-sql-server.md)
