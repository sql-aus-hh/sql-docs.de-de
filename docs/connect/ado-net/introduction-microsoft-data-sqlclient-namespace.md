---
title: Einführung in den Namespace „Microsoft.Data.SqlClient“
description: Hier erfahren Sie mehr über den Namespace „Microsoft.Data.SqlClient“ sowie darüber, warum es sich dabei um die bevorzugte Methode für .NET-Anwendungen zum Herstellen einer Verbindung mit SQL handelt.
ms.date: 11/19/2020
ms.assetid: c18b1fb1-2af1-4de7-80a4-95e56fd976cb
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-jizho2
ms.openlocfilehash: e966e4f2f43ebe546d6baa0b757f682f3eca205b
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596365"
---
# <a name="introduction-to-microsoftdatasqlclient-namespace"></a>Einführung in den Namespace „Microsoft.Data.SqlClient“

[!INCLUDE [Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

## <a name="release-notes-for-microsoftdatasqlclient-21"></a>Versionshinweise für Microsoft.Data.SqlClient 2.1

Die Versionshinweise finden Sie auch im GitHub-Repository: [Hinweise zu Version 2.1](https://github.com/dotnet/SqlClient/tree/master/release-notes/2.1).

### <a name="new-features"></a>Neue Funktionen

### <a name="cross-platform-support-for-always-encrypted"></a>Plattformübergreifende Unterstützung für Always Encrypted
In Microsoft.Data.SqlClient 2.1 wird die Unterstützung für Always Encrypted auf folgenden Plattformen ausgedehnt:

| Unterstützung von Always Encrypted | Verwenden von Always Encrypted mit Secure Enclaves  | Zielframework | Microsoft.Data.SqlClient-Version | Betriebssystem |
|:--|:--|:--|:--:|:--:|
| Ja | Ja | .NET Framework 4.6+ | Ab 1.1.0 | Windows |
| Ja | Ja | .NET Core 2.1 oder höher | Ab 2.1.0<sup>1</sup> | Windows, Linux, macOS |
| Ja | Nein<sup>2</sup> | .NET-Standard 2.0 | Ab 2.1.0 | Windows, Linux, macOS |
| Ja | Ja | Ab .NET Standard 2.1 | Ab 2.1.0 | Windows, Linux, macOS |

> [!NOTE]
> <sup>1</sup> Vor Microsoft.Data.SqlClient-Version 2.1 wird Always Encrypted nur unter Windows unterstützt.
> <sup>2</sup> Always Encrypted mit Secure Enclaves wird nicht unter .NET Standard 2.0 unterstützt.

### <a name="azure-active-directory-device-code-flow-authentication"></a>Gerätecodeflow-Authentifizierung bei Azure Active Directory
Microsoft.Data.SqlClient 2.1 unterstützt die „Gerätecodeflow“-Authentifizierung mit MSAL.NET.
Referenzdokumentation: [Flow bei Gewährung der OAuth2.0-Geräteautorisierung](/azure/active-directory/develop/v2-oauth2-device-code)

Beispiel für Verbindungszeichenfolge:

`Server=<server>.database.windows.net; Authentication=Active Directory Device Code Flow; Database=Northwind;`

Die folgende API ermöglicht die Anpassung des Rückrufmechanismus des Gerätecodeflows:

```csharp
public class ActiveDirectoryAuthenticationProvider
{
    // For .NET Framework, .NET Core and .NET Standard targeted applications
    public void SetDeviceCodeFlowCallback(Func<DeviceCodeResult, Task> deviceCodeFlowCallbackMethod)
}
```

### <a name="azure-active-directory-managed-identity-authentication"></a>Azure Active Directory-Authentifizierung mit einer verwalteten Identität
Microsoft.Data.SqlClient 2.1 bietet Unterstützung für die Azure Active Directory-Authentifizierung mithilfe [verwalteter Identitäten](/azure/active-directory/managed-identities-azure-resources/overview).

Die folgenden Schlüsselwörter für den Authentifizierungsmodus werden unterstützt:
- Active Directory mit verwalteter Identität
- Active Directory MSI (für übergreifende Kompatibilität mit Microsoft SQL-Treibern)

Beispiele für Verbindungszeichenfolgen:

```cs
// For System Assigned Managed Identity
"Server={serverURL}; Authentication=Active Directory MSI; Initial Catalog={db};"

// For System Assigned Managed Identity
"Server={serverURL}; Authentication=Active Directory Managed Identity; Initial Catalog={db};"

// For User Assigned Managed Identity
"Server={serverURL}; Authentication=Active Directory MSI; User Id={ObjectIdOfManagedIdentity}; Initial Catalog={db};"

// For User Assigned Managed Identity
"Server={serverURL}; Authentication=Active Directory Managed Identity; User Id={ObjectIdOfManagedIdentity}; Initial Catalog={db};"
```

### <a name="azure-active-directory-interactive-authentication-enhancements"></a>Weiterentwicklungen der interaktiven Azure Active Directory-Authentifizierung
Microsoft.Data.SqlClient 2.1 fügt die folgenden APIs zum benutzerdefinierten Anpassen der Umgebung zur interaktiven Active Directory-Authentifizierung hinzu:

```csharp
public class ActiveDirectoryAuthenticationProvider
{
    // For .NET Framework targeted applications only
    public void SetIWin32WindowFunc(Func<IWin32Window> iWin32WindowFunc);

    // For .NET Standard targeted applications only
    public void SetParentActivityOrWindowFunc(Func<object> parentActivityOrWindowFunc);

    // For .NET Framework, .NET Core and .NET Standard targeted applications
    public void SetAcquireAuthorizationCodeAsyncCallback(Func<Uri, Uri, CancellationToken, Task<Uri>> acquireAuthorizationCodeAsyncCallback);

    // For .NET Framework, .NET Core and .NET Standard targeted applications
    public void ClearUserTokenCache();
}
```

### <a name="sqlclientauthenticationproviders-configuration-section"></a>Konfigurationsabschnitt `SqlClientAuthenticationProviders`
Microsoft.Data.SqlClient 2.1 enthält nun den neuen Konfigurationsabschnitt `SqlClientAuthenticationProviders` (ein Klon des vorhandenen Abschnitts `SqlAuthenticationProviders`). Der vorhandene Konfigurationsabschnitt `SqlAuthenticationProviders` wird weiterhin aus Gründen der Abwärtskompatibilität unterstützt, wenn der entsprechende Typ definiert ist.

Der neue Abschnitt ermöglicht es, dass Anwendungskonfigurationsdateien sowohl den Abschnitt SqlAuthenticationProviders für System.Data.SqlClient als auch den Abschnitt SqlClientAuthenticationProviders für Microsoft.Data.SqlClient enthalten.


### <a name="azure-active-directory-authentication-using-an-application-client-id"></a>Azure Active Directory-Authentifizierung mit Anwendungsclient-ID
Microsoft.Data.SqlClient 2.1 bietet Unterstützung für das Übergeben einer benutzerdefinierten Anwendungsclient-ID an die Microsoft-Authentifizierungsbibliothek. Die Anwendungsclient-ID wird bei der Authentifizierung bei Azure Active Directory verwendet.

Die folgenden neuen APIs werden eingeführt:

1. Ein neuer Konstruktor wurde in ActiveDirectoryAuthenticationProvider eingeführt:\
_[Gilt für alle .NET-Plattformen (.NET Framework, .NET Core und .NET Standard)]_

```csharp
public ActiveDirectoryAuthenticationProvider(string applicationClientId)
```

Syntax:
```csharp
string APP_CLIENT_ID = "<GUID>";
SqlAuthenticationProvider customAuthProvider = new ActiveDirectoryAuthenticationProvider(APP_CLIENT_ID);
SqlAuthenticationProvider.SetProvider(SqlAuthenticationMethod.ActiveDirectoryInteractive, customAuthProvider);

using (SqlConnection sqlConnection = new SqlConnection("<connection_string>")
{
    sqlConnection.Open();
}
```

2. Eine neue Konfigurationseigenschaft wurde unter `SqlAuthenticationProviderConfigurationSection` und `SqlClientAuthenticationProviderConfigurationSection` eingeführt:\
_[Gilt für .NET Framework und .NET Core]_

```csharp
internal class SqlAuthenticationProviderConfigurationSection : ConfigurationSection
{
    ...
    [ConfigurationProperty("applicationClientId", IsRequired = false)]
    public string ApplicationClientId => this["applicationClientId"] as string;
}

// Inheritance
internal class SqlClientAuthenticationProviderConfigurationSection : SqlAuthenticationProviderConfigurationSection
{ ... }
```

Syntax:
```xml
<configuration>
    <configSections>
        <section name="SqlClientAuthenticationProviders"
                         type="Microsoft.Data.SqlClient.SqlClientAuthenticationProviderConfigurationSection, Microsoft.Data.SqlClient" />
    </configSections>
    <SqlClientAuthenticationProviders applicationClientId ="<GUID>" />
</configuration>

<!--or-->

<configuration>
    <configSections>
        <section name="SqlAuthenticationProviders"
                         type="Microsoft.Data.SqlClient.SqlAuthenticationProviderConfigurationSection, Microsoft.Data.SqlClient" />
    </configSections>
    <SqlAuthenticationProviders applicationClientId ="<GUID>" />
</configuration>
```

### <a name="data-classification-v2-support"></a>Unterstützung von Datenklassifizierung, Version 2
Microsoft.Data.SqlClient 2.1 bietet jetzt Unterstützung von Informationen zur Vertraulichkeitsbewertung zur Datenklassifizierung. Die folgenden neuen APIs sind nun verfügbar:

```csharp
public class SensitivityClassification
{
    public SensitivityRank SensitivityRank;
}

public class SensitivityProperty
{
    public SensitivityRank SensitivityRank;
}

public enum SensitivityRank
{
    NOT_DEFINED = -1,
    NONE = 0,
    LOW = 10,
    MEDIUM = 20,
    HIGH = 30,
    CRITICAL = 40
}
```

### <a name="server-process-id-for-an-active-sqlconnection"></a>Serverprozess-ID für eine aktive `SqlConnection`
Microsoft.Data.SqlClient 2.1 bietet für eine aktive Verbindung die neue `SqlConnection`-Eigenschaft `ServerProcessId`.

```csharp
public class SqlConnection
{
    // Returns the server process Id (SPID) of the active connection.
    public int ServerProcessId;
}
```

### <a name="trace-logging-support-in-native-sni"></a>Unterstützung der Ablaufverfolgungsprotokollierung in nativer SNI-Datei
Microsoft.Data.SqlClient 2.1 dehnt die vorhandene Implementierung von `SqlClientEventSource` aus, um die Ereignisablaufverfolgung in „SNI.dll“ zu ermöglichen. Ereignisse müssen mithilfe eines Tools wie Xperf aufgezeichnet werden.

Die Ablaufverfolgung kann aktiviert werden, indem wie unten dargestellt ein Befehl an `SqlClientEventSource` gesendet wird:

```csharp
// Enables trace events:
EventSource.SendCommand(eventSource, (EventCommand)8192, null);

// Enables flow events:
EventSource.SendCommand(eventSource, (EventCommand)16384, null);

// Enables both trace and flow events:
EventSource.SendCommand(eventSource, (EventCommand)(8192 | 16384), null);
```


### <a name="command-timeout-connection-string-property"></a>Verbindungszeichenfolgen-Eigenschaft „Befehlstimeout“
Microsoft.Data.SqlClient 2.1 führt die Verbindungszeichenfolgen-Eigenschaft „Befehlstimeout“ ein, um den Standardwert von 30 Sekunden zu überschreiben. Das Zeitlimit für einzelne Befehle kann mit der `CommandTimeout`-Eigenschaft für SqlCommand überschrieben werden.

Beispiele für Verbindungszeichenfolgen:

`"Server={serverURL}; Initial Catalog={db}; Integrated Security=true; Command Timeout=60"`

### <a name="removal-of-symbols-from-native-sni"></a>Entfernung von Symbolen aus nativer SNI-Datei
In Microsoft.Data.SqlClient 2.1 haben wir die in [v2.0.0](https://www.nuget.org/packages/Microsoft.Data.SqlClient.SNI/2.0.0) eingeführten Symbole aus dem NuGet-Paket [Microsoft.Data.SqlClient.SNI.runtime](https://www.nuget.org/packages/Microsoft.Data.SqlClient.SNI.runtime) beginnend mit [v2.1.1](https://www.nuget.org/packages/Microsoft.Data.SqlClient.SNI.runtime/2.1.1) entfernt. Die öffentlichen Symbole werden jetzt für Tools wie BinSkim, die Zugriff auf öffentliche Symbole benötigen, auf dem Microsoft-Symbolserver veröffentlicht.

### <a name="source-linking-of-microsoftdatasqlclient-symbols"></a>Quellverknüpfung von Microsoft.Data.SqlClient-Symbolen
Ab Microsoft.Data.SqlClient 2.1 werden Microsoft.Data.SqlClient-Symbole mit Quellcode verknüpft und auf dem Microsoft-Symbolserver veröffentlicht, um das Debuggen zu erleichtern, ohne dass Quellcode heruntergeladen werden muss.


## <a name="release-notes-for-microsoftdatasqlclient-20"></a>Versionshinweise für Microsoft.Data.SqlClient 2.0

Die Versionshinweise finden Sie auch im GitHub-Repository: [Versionshinweise zu 2.0](https://github.com/dotnet/SqlClient/tree/master/release-notes/2.0)

### <a name="breaking-changes"></a>Aktuelle Änderungen

- Die Zugriffsmodifizierer für die Schnittstelle `SqlColumnEncryptionEnclaveProvider` des Enclave-Anbieters wurde von `public` in `internal` geändert.

- Konstanten in der `SqlClientMetaDataCollectionNames`-Klasse wurden aktualisiert, um die Änderungen in SQL Server widerzuspiegeln.

- Der Treiber führt nun eine Überprüfung des Serverzertifikats aus, wenn die SQL Server-Zielinstanz TLS-Verschlüsselung erzwingt. Dies ist die Standardeinstellung für Azure-Verbindungen.

- `SqlDataReader.GetSchemaTable()` gibt nun anstelle von `null` eine leere `DataTable`-Klasse zurück.

- Der Treiber führt nun eine Rundung von Dezimalzahlen durch, um dem Verhalten von SQL Server zu entsprechen. Wenn eine Abwärtskompatibilität erforderlich ist, kann das bisherige Verhalten (Kürzen) mithilfe einer AppContext-Option aktiviert werden.

- Bei .NET Framework-Anwendungen, die **Microsoft.Data.SqlClient** nutzen, heißen die zuvor in die Ordner `bin\x64` und `bin\x86` heruntergeladenen SNI.dll-Dateien nun `Microsoft.Data.SqlClient.SNI.x64.dll` und ` Microsoft.Data.SqlClient.SNI.x86.dll` und werden in das `bin`-Verzeichnis heruntergeladen.

- Neue Synonyme für Verbindungszeichenfolgeneigenschaften ersetzen alte Eigenschaften beim Abrufen von Verbindungszeichenfolgen aus `SqlConnectionStringBuilder` aus Gründen der Konsistenz. [Weitere Informationen](#new-connection-string-property-synonyms)

### <a name="new-features"></a>Neue Funktionen

#### <a name="dns-failure-resiliency"></a>DNS-Fehlerresilienz

Der Treiber speichert IP-Adressen ab sofort für jede erfolgreiche Verbindung zu einem SQL Server-Endpunkt zwischen, der das Feature unterstützt. Wenn während eines Verbindungsversuchs ein Fehler bei der DNS-Auflösung auftritt, versucht der Treiber, mithilfe einer zwischengespeicherten IP-Adresse für diesen Server (sofern vorhanden) eine Verbindung herzustellen.

#### <a name="eventsource-tracing"></a>EventSource-Nachverfolgung

Ab diesem Release wird das Erfassen von Ereignisablaufprotokollen unterstützt, um Anwendungen zu debuggen. Zur Erfassung dieser Ereignisse müssen Clientanwendungen auf Ereignisse aus der EventSource-Implementierung von SqlClient lauschen:

```
Microsoft.Data.SqlClient.EventSource
```

Weitere Informationen finden Sie unter [Aktivieren der Ereignisablaufverfolgung in SqlClient](enable-eventsource-tracing.md).

#### <a name="enabling-managed-networking-on-windows"></a>Aktivieren verwalteter Netzwerke unter Windows

Mithilfe der neuen AppContext-Option **Switch.Microsoft.Data.SqlClient.UseManagedNetworkingOnWindows** kann unter Windows zu Test- und Debuggingzwecken eine verwaltete SNI-Implementierung verwendet werden. Diese Option schaltet das Verhalten des Treibers so um, dass in Projekten in .NET Core 2.1 und höher und in .NET Standard 2.0 und höher unter Windows eine verwaltete SNI verwendet wird. Dies beseitigt alle Abhängigkeiten von nativen Bibliotheken für die Microsoft.Data.SqlClient-Bibliothek.

```csharp
AppContext.SetSwitch("Switch.Microsoft.Data.SqlClient.UseManagedNetworkingOnWindows", true);
```

Unter [AppContext-Optionen in SqlClient](appcontext-switches.md) finden Sie eine vollständige Liste verfügbarer Optionen für den Treiber.

#### <a name="enabling-decimal-truncation-behavior"></a>Aktivieren des Kürzungsverhaltens für Dezimalzahlen

decimal-Datentypen werden vom Treiber standardmäßig gerundet, wie das auch bei SQL Server der Fall ist. Aus Gründen der Abwärtskompatibilität können Sie die AppContext-Option **Switch.Microsoft.Data.SqlClient.TruncateScaledDecimal** auf **True** festlegen.

```csharp
AppContext.SetSwitch("Switch.Microsoft.Data.SqlClient.TruncateScaledDecimal", true);
```

#### <a name="new-connection-string-property-synonyms"></a>Neue Synonyme für Verbindungszeichenfolgeneigenschaften

Für die folgenden vorhandenen Verbindungszeichenfolgeneigenschaften wurden neue Synonyme hinzugefügt, um Verwirrung bei den Leerzeichen von Eigenschaften zu verhindern, die aus mehr als einem Wort bestehen. Aus Gründen der Abwärtskompatibilität werden alte Eigenschaftennamen weiterhin unterstützt, die neuen Verbindungszeichenfolgeneigenschaften werden ab sofort aber eingeschlossen, wenn Verbindungszeichenfolgen aus [SqlConnectionStringBuilder](/dotnet/api/microsoft.data.sqlclient.sqlconnectionstringbuilder) abgerufen werden.

|Vorhandene Verbindungszeichenfolgeneigenschaft|Neues Synonym|
|-----------------------------------|-----------|
| ApplicationIntent | Application Intent |
| ConnectRetryCount | Connect Retry Count (Anzahl der Verbindungsversuche) |
| ConnectRetryInterval | Connect Retry Interval (Verbindungswiederholungsintervall) |
| PoolBlockingPeriod | Pool Blocking Period (Blockierungszeitraum für einen Pool) |
| MultipleActiveResultSets | Multiple Active Result Sets (Mehrere aktive Resultsets) |
| MultiSubnetFailover | Multiple Subnet Failover (Failover für mehrere Subnetze) |
| TransparentNetworkIPResolution | Transparent Network IP Resolution (Transparente Netzwerk-IP-Adressauflösung) |
| TrustServerCertificate | TrustServerCertificate |

#### <a name="sqlbulkcopy-rowscopied-property"></a>SqlBulkCopy-Eigenschaft „RowsCopied“

Die RowsCopied-Eigenschaft bietet schreibgeschützten Zugriff auf die Anzahl der Datensätze, die von einem aktiven Massenkopiervorgang bereits verarbeitet wurden. Dieser Wert entspricht nicht unweigerlich der endgültigen Anzahl an Datensätzen, die der Zieltabelle hinzugefügt wurden.

#### <a name="connection-open-overrides"></a>Überschreibungen von SqlConnection.Open

Das Standardverhalten von SqlConnection.Open() kann überschrieben werden, um die Verzögerung von zehn Sekunden und automatische erneute Verbindungsversuche zu deaktivieren, die von vorübergehenden Fehlern ausgelöst werden.

```csharp
using SqlConnection sqlConnection = new SqlConnection("Data Source=(local);Integrated Security=true;Initial Catalog=AdventureWorks;");
sqlConnection.Open(SqlConnectionOverrides.OpenWithoutRetry);
```

> [!NOTE]
> Beachten Sie, dass diese Überschreibung nur auf SqlConnection.Open() angewendet werden kann, nicht auf SqlConnection.OpenAsync().

#### <a name="username-support-for-active-directory-interactive-mode"></a>Unterstützung für Benutzernamen bei der interaktiven Active Directory-Authentifizierung

Bei Verwendung der interaktiven Active Directory-Authentifizierung kann sowohl für .NET Framework als auch für .NET Core ein Benutzername in der Verbindungszeichenfolge angegeben werden.

Legen Sie einen Benutzernamen mithilfe der Verbindungszeichenfolgeneigenschaft **User ID** oder **UID** fest:

```
"Server=<server name>; Database=<db name>; Authentication=Active Directory Interactive; User Id=<username>;"
```

#### <a name="order-hints-for-sqlbulkcopy"></a>Hinweise für die Sortierung für SqlBulkCopy

Hinweise für die Sortierung können bereitgestellt werden, um die Leistung von Massenkopiervorgängen für Tabellen mit gruppierten Indizes zu verbessern. Weitere Informationen finden Sie unter [Hinweise für die Sortierung für Massenkopiervorgänge](sql/bulk-copy-order-hints.md).

#### <a name="sni-dependency-changes"></a>SNI-Abhängigkeitsänderungen

Microsoft.Data.SqlClient (.NET Core und .NET Standard) weisen unter Windows nun eine Abhängigkeit von **Microsoft.Data.SqlClient.SNI.runtime** auf. Dadurch wird die bisherige Abhängigkeit von **runtime.native.System.Data.SqlClient.SNI** ersetzt. Mit der neuen Abhängigkeit wird die ARM-Plattform zusammen mit den bereits unterstützten Plattformen ARM64, x64 und x86 unter Windows unterstützt.

## <a name="release-notes-for-microsoftdatasqlclient-110"></a>Versionshinweise zu Microsoft.Data.SqlClient 1.1.0

Die Versionshinweise finden Sie auch im GitHub-Repository: [Hinweise zur Version 1.1](https://github.com/dotnet/SqlClient/tree/master/release-notes/1.1).

### <a name="new-features"></a>Neue Funktionen

#### <a name="always-encrypted-with-secure-enclaves"></a>Always Encrypted mit Secure Enclaves

Always Encrypted ist ab Microsoft SQL Server 2016 verfügbar. Secure Enclaves sind ab Microsoft SQL Server 2019 verfügbar. Die Verbindungszeichenfolgen müssen das erforderliche Nachweisprotokoll und die Nachweis-URL enthalten, um das Enclave-Feature nutzen zu können. Beispiel:

```
Attestation Protocol=HGS;Enclave Attestation Url=<attestation_url_for_HGS>
```

Weitere Informationen finden Sie unter

- [SqlClient-Unterstützung für Always Encrypted](sql/sqlclient-support-always-encrypted.md)
- [Tutorial: Entwickeln einer .NET-Anwendung mithilfe von Always Encrypted mit Secure Enclaves](sql/tutorial-always-encrypted-enclaves-develop-net-apps.md)

## <a name="release-notes-for-microsoftdatasqlclient-10"></a>Versionshinweise für Microsoft.Data.SqlClient 1.0

Die erste Version des Namespace Microsoft.Data.SqlClient bietet gegenüber dem bestehenden Namespace System.Data.SqlClient zusätzliche Funktionalität.
Die Versionshinweise finden Sie auch im GitHub-Repository: [Hinweise zur Version 1.0](https://github.com/dotnet/SqlClient/tree/master/release-notes/1.0).

### <a name="new-features"></a>Neue Funktionen

#### <a name="new-features-over-net-framework-472-systemdatasqlclient"></a>Neue Features im Vergleich zu System.Data.SqlClient in .NET Framework 4.7.2

- **Datenklassifizierung**: verfügbar in Azure SQL-Datenbank und Microsoft SQL Server 2019.

- **UTF-8-Unterstützung**: verfügbar in Microsoft SQL Server 2019.

#### <a name="new-features-over-net-core-22-systemdatasqlclient"></a>Neue Features im Vergleich zu System.Data.SqlClient in .NET Core 2.2

- **Datenklassifizierung**: verfügbar in Azure SQL-Datenbank und Microsoft SQL Server 2019.

- **UTF-8-Unterstützung**: verfügbar in Microsoft SQL Server 2019.

- **Authentifizierung**: Active Directory-Kennwortauthentifizierungsmodus.

### <a name="data-classification"></a>Datenklassifizierung

Das Feature „Datenklassifizierung“ bietet eine neue Gruppe von APIs, die schreibgeschützte Informationen zur Datenvertraulichkeit und -klassifizierung von Objekten verfügbar machen. Diese werden über SqlDataReader abgerufen, wenn die zugrunde liegende Quelle das Feature unterstützt und Metadaten zu [Datenvertraulichkeit und -klassifizierung](../../relational-databases/security/sql-data-discovery-and-classification.md) enthält. Unter [Microsoft.Data.SqlClient 1.1-Releases](https://github.com/dotnet/SqlClient/tree/master/release-notes/1.1) finden Sie die Beispielanwendung.

```csharp
public class SqlDataReader
{
    public Microsoft.Data.SqlClient.DataClassification.SensitivityClassification SensitivityClassification
}

namespace Microsoft.Data.SqlClient.DataClassification
{
    public class ColumnSensitivity
    {
        public System.Collections.ObjectModel.ReadOnlyCollection<Microsoft.Data.SqlClient.DataClassification.SensitivityProperty> SensitivityProperties
    }
    public class InformationType
    {
        public string Id
        public string Name
    }
    public class Label
    {
        public string Id
        public string Name
    }
    public class SensitivityClassification
    {
        public System.Collections.ObjectModel.ReadOnlyCollection<Microsoft.Data.SqlClient.DataClassification.ColumnSensitivity> ColumnSensitivities
        public System.Collections.ObjectModel.ReadOnlyCollection<Microsoft.Data.SqlClient.DataClassification.InformationType> InformationTypes
        public System.Collections.ObjectModel.ReadOnlyCollection<Microsoft.Data.SqlClient.DataClassification.Label> Labels
    }
    public class SensitivityProperty
    {
        public Microsoft.Data.SqlClient.DataClassification.InformationType InformationType
        public Microsoft.Data.SqlClient.DataClassification.Label Label
    }
}
```

### <a name="utf-8-support"></a>Unterstützung für UTF-8

Für UTF-8-Unterstützung sind keine Änderungen am Anwendungscode erforderlich. Diese SqlClient-Änderungen optimieren die Kommunikation zwischen Client und Server, wenn der Server UTF-8 unterstützt und die zugrunde liegende Spaltensortierung UTF-8 ist. Weitere Informationen finden Sie im Abschnitt über UTF-8 unter [Neues in SQL Server 2019 (15.x)](../../sql-server/what-s-new-in-sql-server-ver15.md).

### <a name="always-encrypted-with-enclaves"></a>Always Encrypted mit Enclaves

Im Allgemeinen sollte die vorhandene Dokumentation zu System.Data.SqlClient im .NET Framework **und integrierten Anbietern von Speicher für Spaltenhauptschlüssel** nun auch für .NET Core gelten.

 [Entwickeln von Always Encrypted mit .NET Framework-Datenanbieter](../../relational-databases/security/encryption/develop-using-always-encrypted-with-net-framework-data-provider.md)

 [Always Encrypted: Schützen vertraulicher Daten und Speichern von Verschlüsselungsschlüsseln im Windows-Zertifikatspeicher](/azure/sql-database/sql-database-always-encrypted)

### <a name="authentication"></a>Authentifizierung

Unterschiedliche Authentifizierungsmodi können angegeben werden, indem in der Verbindungszeichenfolge die Option _Authentifizierung_ verwendet wird. Weitere Informationen finden Sie in der Dokumentation zu [SqlAuthenticationMethod](/dotnet/api/system.data.sqlclient.sqlauthenticationmethod?view=netframework-4.7.2&preserve-view=true).

> [!NOTE]
> Anbieter benutzerdefinierter Schlüsselspeicher wie der Azure Key Vault-Anbieter müssen aktualisiert werden, damit Microsoft.Data.SqlClient unterstützt wird. Ebenso müssen auch die Enclave-Anbieter aktualisiert werden, um Microsoft.Data.SqlClient zu unterstützen.
> Always Encrypted wird nur für .NET Framework- und .NET Core-Ziele unterstützt. Das Feature wird nicht für .NET Standard nicht unterstützt, da .NET Standard bestimmte Verschlüsselungsabhängigkeiten fehlen.