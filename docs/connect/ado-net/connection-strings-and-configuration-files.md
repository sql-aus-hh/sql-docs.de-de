---
title: Verbindungszeichenfolgen und Konfigurationsdateien
description: Erfahren Sie, wie Verbindungszeichenfolgen für ADO.NET-Anwendungen als bewährte Methode für Sicherheit und Wartung in einer Anwendungskonfigurationsdatei gespeichert werden können.
ms.date: 11/13/2020
dev_langs:
- csharp
ms.assetid: 37df2641-661e-407a-a3fb-7bf9540f01e8
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 8f1e0ac5fc52576f0e47ccb8be4b8e90899c6e68
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771457"
---
# <a name="connection-strings-and-configuration-files"></a>Verbindungszeichenfolgen und Konfigurationsdateien

[!INCLUDE[appliesto-netfx-netcore-xxxx-md](../../includes/appliesto-netfx-netcore-xxxx-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Das Einbetten von Verbindungszeichenfolgen in den Code Ihrer Anwendung kann zu Sicherheitslücken und Wartungsproblemen führen. Unverschlüsselte Verbindungszeichenfolgen, die in den Quellcode einer Anwendung kompiliert wurden, können mit dem Tool [Ildasm.exe (IL Disassembler)](/dotnet/framework/tools/ildasm-exe-il-disassembler) angezeigt werden. Hinzu kommt, dass die Anwendung neu kompiliert werden muss, wenn sich die Verbindungszeichenfolge irgendwann einmal ändert. Aus diesen Gründen empfehlen wir, Verbindungszeichenfolgen in einer Anwendungskonfigurationsdatei zu speichern.

## <a name="work-with-application-configuration-files"></a>Arbeiten mit Anwendungskonfigurationsdateien

Anwendungskonfigurationsdateien enthalten anwendungsspezifische Einstellungen. Eine ASP.NET-Anwendung kann beispielsweise mindestens eine **web.config**-Datei enthalten, und eine Windows-Anwendung kann eine optionale **app.config**-Datei enthalten. Konfigurationsdateien haben etliche Elemente gemein, auch wenn sich der Name und der Speicherort der Konfigurationsdateien abhängig vom jeweiligen Host der Anwendung ändert.

### <a name="the-connectionstrings-section"></a>connectionStrings-Abschnitt

Verbindungszeichenfolgen können als Schlüssel/Wert-Paare im Abschnitt **connectionStrings** des **configuration**-Elements einer Anwendungskonfigurationsdatei gespeichert werden. Zu den untergeordneten Elementen gehören **add**, **clear** und **remove**.

Das folgende Konfigurationsdateifragment zeigt das Schema und die Syntax für das Speichern einer Verbindungszeichenfolge. Das **name**-Attribut ist ein Name, den Sie zum eindeutigen Identifizieren einer Verbindungszeichenfolge angeben, damit diese zur Laufzeit abgerufen werden kann. **providerName** ist **Microsoft.Data.SqlClient** mit dem Microsoft SqlClient-Datenanbieter für SQL Server.

```xml  
<?xml version='1.0' encoding='utf-8'?>  
  <configuration>  
    <connectionStrings>  
      <clear />  
      <add name="Name"
       providerName="Microsoft.Data.SqlClient"
       connectionString="Valid Connection String;" />  
    </connectionStrings>  
  </configuration>  
```  

> [!NOTE]
> Sie können einen Teil der Verbindungszeichenfolge in einer Konfigurationsdatei speichern und zur Vervollständigung zur Laufzeit die <xref:System.Data.Common.DbConnectionStringBuilder>-Klasse verwenden. Diese Vorgehensweise empfiehlt sich in Szenarien, in denen Ihnen die Elemente der Verbindungszeichenfolge vorab nicht bekannt sind oder wenn Sie sicherheitsrelevante Informationen nicht in einer Konfigurationsdatei speichern möchten. Weitere Informationen finden Sie in [Connection String Builders (Verbindungszeichenfolgengeneratoren)](connection-string-builders.md).

### <a name="use-external-configuration-files"></a>Verwenden externer Konfigurationsdateien

Externe Konfigurationsdateien sind separate Dateien, die ein aus einem einzigen Abschnitt bestehendes Fragment einer Konfigurationsdatei enthalten. Auf die externe Konfigurationsdatei wird dann von der Hauptkonfigurationsdatei verwiesen. Das Speichern des Abschnitts **connectionStrings** in einer separaten Datei empfiehlt sich, wenn die Verbindungszeichenfolgen auch nach der Bereitstellung der Anwendung noch geändert werden können. ASP.NET verhält sich z. B. standardmäßig so, dass nach Änderungen an Konfigurationsdateien eine Anwendung neu gestartet wird, wodurch Statusinformationen verloren gehen. Änderungen an einer externen Konfigurationsdatei hingegen führen nicht zum Neustart der Anwendung. Externe Konfigurationsdateien sind nicht auf ASP.NET beschränkt und können auch von Windows-Anwendungen verwendet werden. Außerdem kann der Zugriff auf externe Konfigurationsdateien auch durch Dateizugriffssicherheit und Berechtigungen eingeschränkt werden. Der Einsatz externer Konfigurationsdateien zur Laufzeit ist transparent und erfordert keine besondere Codierung.

Wenn Sie Verbindungszeichenfolgen in einer externen Konfigurationsdatei speichern möchten, erstellen Sie eine separate Datei, die ausschließlich den **connectionStrings**-Abschnitt enthält. Nehmen Sie in diese Datei keine zusätzlichen Elemente, Abschnitte oder Attribute auf. Das folgende Beispiel zeigt die Syntax für eine externe Konfigurationsdatei.

```xml  
<connectionStrings>  
  <add name="Name"
   providerName="Microsoft.Data.SqlClient"
   connectionString="Valid Connection String;" />  
</connectionStrings>  
```  

 Verwenden Sie in der Hauptkonfigurationsdatei der Anwendung das **configSource**-Attribut, um den vollqualifizierten Namen und den Speicherort der externen Datei anzugeben. Das folgende Beispiel verweist auf eine Konfigurationsdatei mit dem Namen `connections.config`.

```xml  
<?xml version='1.0' encoding='utf-8'?>  
<configuration>  
    <connectionStrings configSource="connections.config"/>  
</configuration>  
```  

## <a name="retrieving-connection-strings-at-run-time"></a>Abrufen von Verbindungszeichenfolgen zur Laufzeit

Mit .NET Framework 2.0 wurden neue Klassen im <xref:System.Configuration>-Namespace eingeführt, um das Abrufen von Verbindungszeichenfolgen aus Konfigurationsdateien zur Laufzeit zu vereinfachen. Sie können programmgesteuert eine Verbindungszeichenfolge nach Namen oder Anbieternamen abrufen.

> [!NOTE]
> Die Datei **machine.config** enthält auch einen **connectionStrings**-Abschnitt, der von Visual Studio verwendete Verbindungszeichenfolgen enthält. Beim Abrufen von Verbindungszeichenfolgen anhand des Anbieternamens aus der Datei **app.config** in einer Windows-Anwendung werden zuerst die in **machine.config** vorhandenen Verbindungszeichenfolgen und dann die in **app.config** vorhandenen Verbindungszeichenfolgen geladen. Wenn unmittelbar hinter dem **connectionStrings**-Element **clear** hinzugefügt wird, werden alle geerbten Verweise aus der Datenstruktur im Arbeitsspeicher entfernt, sodass nur die in der lokalen Datei **app.config** definierten Verbindungszeichenfolgen berücksichtigt werden.

### <a name="work-with-the-configuration-files"></a>Arbeiten mit den Konfigurationsdateien

Ab .NET Framework 2.0 wird bei der Arbeit mit Konfigurationsdateien auf dem lokalen Computer der <xref:System.Configuration.ConfigurationManager> verwendet, der die <xref:System.Configuration.ConfigurationSettings> ersetzt. Für die Arbeit mit ASP.NET-Konfigurationsdateien kommt der <xref:System.Web.Configuration.WebConfigurationManager> zum Einsatz. Er wurde für die Verwendung mit den auf einem Webserver befindlichen Konfigurationsdateien entwickelt und erlaubt den programmgesteuerten Zugriff auf Konfigurationsdateiabschnitte, wie z.B. **system.web**.

> [!NOTE]
> Wenn ein Aufrufer zur Laufzeit auf Konfigurationsdateien zugreifen können soll, benötigt er Berechtigungen. Welche Berechtigungen notwendig sind, hängt von der Art der Anwendung, der Konfigurationsdatei und dem Speicherort ab. Weitere Informationen finden Sie unter [Verwenden der Konfigurationsklassen](/previous-versions/aspnet/ms228063(v=vs.100)) und <xref:System.Web.Configuration.WebConfigurationManager> (für ASP.NET-Anwendungen) und unter <xref:System.Configuration.ConfigurationManager> (für Windows-Anwendungen).

Zum Abrufen von Verbindungszeichenfolgen aus Anwendungskonfigurationsdateien können Sie die <xref:System.Configuration.ConnectionStringSettingsCollection> verwenden. Sie enthält eine Auflistung von <xref:System.Configuration.ConnectionStringSettings>-Objekten, wobei jedes Objekt für einen einzelnen Eintrag im **connectionStrings**-Abschnitt steht. Ihre Eigenschaften werden entsprechenden Verbindungszeichenfolgenattributen zugeordnet, sodass es möglich ist, Verbindungszeichenfolgen nach dem Namen oder dem Anbieternamen abzurufen.

|Eigenschaft|BESCHREIBUNG|  
|--------------|-----------------|  
|<xref:System.Configuration.ConnectionStringSettings.Name%2A>|Name der Verbindungszeichenfolge: Wird dem **name**-Attribut zugeordnet.|  
|<xref:System.Configuration.ConnectionStringSettings.ProviderName%2A>|Vollqualifizierter Anbietername: Wird dem **providerName**-Attribut zugeordnet.|  
|<xref:System.Configuration.ConnectionStringSettings.ConnectionString%2A>|Verbindungszeichenfolge. Wird dem **connectionString**-Attribut zugeordnet.|  

### <a name="example-listing-all-connection-strings"></a>Beispiel: Auflisten aller Verbindungszeichenfolgen

Dieses Beispiel iteriert durch <xref:System.Configuration.ConnectionStringSettingsCollection> und zeigt die Eigenschaften <xref:System.Configuration.ConnectionStringSettings.Name%2A?displayProperty=nameWithType>, <xref:System.Configuration.ConnectionStringSettings.ProviderName%2A?displayProperty=nameWithType> und <xref:System.Configuration.ConnectionStringSettings.ConnectionString%2A?displayProperty=nameWithType> im Konsolenfenster an.

> [!NOTE]
> &lt;legacyBold&gt;System.Configuration.dll&lt;/legacyBold&gt; ist nicht in allen Projekttypen enthalten, sodass Sie u. U. einen Verweis einfügen müssen, um die Konfigurationsklassen zu verwenden. Wie die jeweilige Anwendungskonfigurationsdatei heißt und wo sie gespeichert ist, hängt von der Art der Anwendung und dem Hostingprozess ab.

[!code-csharp[DataWorks ConnectionStringSettings.RetrieveFromConfig#1](~/../SqlClient/doc/samples/ConnectionStringSettings_RetrieveFromConfig.cs#1)]

### <a name="example-retrieving-a-connection-string-by-name"></a>Beispiel: Abrufen einer Verbindungszeichenfolge über den Namen

In diesem Beispiel wird gezeigt, wie eine Verbindungszeichenfolge aus einer Konfigurationsdatei durch Angabe ihres Namens abgerufen werden kann. Der Code erstellt ein <xref:System.Configuration.ConnectionStringSettings>-Objekt, das den bereitgestellten Eingabeparameter mit dem <xref:System.Configuration.ConfigurationManager.ConnectionStrings%2A>-Namen abgleicht. Wird kein übereinstimmender Name gefunden, gibt die Funktion `null` (`Nothing` in Visual Basic) zurück.

[!code-csharp[DataWorks ConnectionStringSettings.RetrieveFromConfigByName#1](~/../SqlClient/doc/samples/ConnectionStringSettings_RetrieveFromConfigByName.cs#1)]

### <a name="example-retrieving-a-connection-string-by-provider-name"></a>Beispiel: Abrufen einer Verbindungszeichenfolge über den Anbieternamen

In diesem Beispiel wird gezeigt, wie eine Verbindungszeichenfolge durch Angabe des Anbieternamens im Format *Microsoft.Data.SqlClient* abgerufen werden kann. Der Code durchläuft die <xref:System.Configuration.ConnectionStringSettingsCollection> und gibt die Verbindungszeichenfolge für den ersten gefundenen <xref:System.Configuration.ConnectionStringSettings.ProviderName%2A>-Eintrag zurück. Wird kein Anbietername gefunden, gibt die Funktion `null` (`Nothing` in Visual Basic) zurück.

[!code-csharp[DataWorks ConnectionStringSettings.RetrieveFromConfigByProvider#1](~/../SqlClient/doc/samples/ConnectionStringSettings_RetrieveFromConfigByProvider.cs#1)]

## <a name="encrypt-configuration-file-sections-using-protected-configuration"></a>Verschlüsseln von Konfigurationsdateiabschnitten mithilfe der geschützten Konfiguration

In ASP.NET 2.0 wurde mit der *geschützten Konfiguration* ein neues Feature eingeführt, mit dem Sie sicherheitsrelevante Informationen in einer Konfigurationsdatei verschlüsseln können. Die geschützte Konfiguration wurde zwar primär für ASP.NET entwickelt, sie kann aber auch zum Verschlüsseln von Konfigurationsdateiabschnitten in Windows-Anwendungen verwendet werden. Eine ausführliche Beschreibung der Funktionen der geschützten Konfiguration finden Sie unter [Verschlüsseln von Konfigurationsinformationen mithilfe der geschützten Konfiguration](/previous-versions/aspnet/53tyfkaw(v=vs.100)).

Das folgende Konfigurationsdateifragment zeigt den **connectionStrings**-Abschnitt nach der Verschlüsselung. **configProtectionProvider** gibt den Anbieter für die geschützte Konfiguration an, der zum Verschlüsseln und Entschlüsseln der Verbindungszeichenfolgen verwendet wird. Der **EncryptedData**-Abschnitt enthält den Verschlüsselungstext.

```xml  
<connectionStrings configProtectionProvider="DataProtectionConfigurationProvider">  
  <EncryptedData>  
    <CipherData>  
      <CipherValue>AQAAANCMnd8BFdERjHoAwE/Cl+sBAAAAH2... </CipherValue>  
    </CipherData>  
  </EncryptedData>  
</connectionStrings>  
```  

Wenn die verschlüsselte Verbindungszeichenfolge zur Laufzeit abgerufen wird, verwendet .NET Framework den angegebenen Anbieter, um den **CipherValue** zu entschlüsseln und ihn für Ihre Anwendung zur Verfügung zu stellen. Sie müssen für die Verwaltung des Entschlüsselungsprozesses keinen zusätzlichen Code schreiben.

### <a name="protected-configuration-providers"></a>Anbieter geschützter Konfigurationen

Die Anbieter für die geschützte Konfiguration werden im **configProtectedData**-Abschnitt der Datei **machine.config** auf dem lokalen Computer registriert, wie dies im folgenden Fragment dargestellt ist. Das Fragment enthält die beiden Anbieter für die geschützte Konfiguration in .NET Framework. Die hier gezeigten Werte wurden aus Gründen der besseren Lesbarkeit gekürzt.

```xml  
<configProtectedData defaultProvider="RsaProtectedConfigurationProvider">  
  <providers>  
    <add name="RsaProtectedConfigurationProvider"
      type="System.Configuration.RsaProtectedConfigurationProvider" />  
    <add name="DataProtectionConfigurationProvider"
      type="System.Configuration.DpapiProtectedConfigurationProvider" />  
  </providers>  
</configProtectedData>  
```  

Sie können zusätzliche Anbieter für die geschützte Konfiguration konfigurieren, indem Sie sie der Datei **machine.config** hinzufügen. Sie können auch einen eigenen Anbieter für die geschützte Konfiguration erstellen, indem Sie von der abstrakten Basisklasse <xref:System.Configuration.ProtectedConfigurationProvider> erben. In der folgenden Tabelle werden die zwei in .NET Framework enthaltenen Anbieter für die geschützte Konfiguration beschrieben.

|Anbieter|Beschreibung|  
|--------------|-----------------|  
|<xref:System.Configuration.RsaProtectedConfigurationProvider>|Verwendet zum Verschlüsseln und Entschlüsseln der Daten den RSA-Verschlüsselungsalgorithmus. Der RSA-Algorithmus kann sowohl für die Verschlüsselung mit öffentlichem Schlüssel als auch für digitale Signaturen verwendet werden. Er wird auch als "öffentlicher Schlüssel" oder asymmetrische Verschlüsselung bezeichnet, da bei dieser Art der Verschlüsselung zwei verschiedene Schlüssel eingesetzt werden. Mit dem [ASP.NET IIS-Registrierungstool (Aspnet_regiis.exe)](/previous-versions/dotnet/netframework-3.5/k6h9cz8h(v=vs.90)) können Sie die Abschnitte in einer Web.config-Datei verschlüsseln und die Verschlüsselungsschlüssel verwalten. ASP.NET entschlüsselt die Konfigurationsdatei, wenn die Datei verarbeitet wird. Die Identität der ASP.NET-Anwendung muss berechtigt sein, den Verschlüsselungsschlüssel zu lesen, mit dem die verschlüsselten Abschnitte verschlüsselt und entschlüsselt werden.|  
|<xref:System.Configuration.DpapiProtectedConfigurationProvider>|Verwendet zum Verschlüsseln der Konfigurationsabschnitte die Windows-Datenschutz-API (DPAPI). Die DPAPI verwendet die in Windows integrierten Kryptografiedienste, und sie kann für den computerspezifischen oder den benutzerkontospezifischen Schutz konfiguriert werden. Der computerspezifische Schutz bietet sich an, wenn auf demselben Server mehrere Anwendungen vorhanden sind, die Informationen gemeinsam nutzen müssen. Die Verwendung des benutzerkontospezifischen Schutzes empfiehlt sich bei Diensten, die mit einer bestimmten Benutzeridentität, z. B. einer freigegebenen gehosteten Umgebung, ausgeführt werden. Jede Anwendung wird unter einer separaten Identität ausgeführt, die den Zugriff auf Ressourcen wie z. B. Dateien und Datenbanken einschränkt.|

Beide Anbieter bieten eine starke Verschlüsselung der Daten. Wenn Sie aber beabsichtigen, ein und dieselbe verschlüsselte Konfigurationsdatei auf mehreren Servern, z. B. in einer Webfarm, zu verwenden, müssen Sie den <xref:System.Configuration.RsaProtectedConfigurationProvider> verwenden, da nur er die Möglichkeit bietet, die zum Verschlüsseln der Daten verwendeten Verschlüsselungsschlüssel zu exportieren und sie auf einem anderen Server zu importieren. Weitere Informationen finden Sie unter [Importieren und Exportieren von RSA-Schlüsselcontainern mit geschützter Konfiguration](/previous-versions/aspnet/yxw286t2(v=vs.100)).

### <a name="use-the-configuration-classes"></a>Verwenden der Konfigurationsklassen

Der <xref:System.Configuration>-Namespace stellt Klassen zum programmgesteuerten Arbeiten mit Konfigurationseinstellungen bereit. Die <xref:System.Configuration.ConfigurationManager>-Klasse ermöglicht den Zugriff auf Computer-, Anwendungs- und Benutzerkonfigurationsdateien. Beim Erstellen einer ASP.NET-Anwendung können Sie die <xref:System.Web.Configuration.WebConfigurationManager>-Klasse verwenden, die dieselbe Funktionalität bietet, Ihnen gleichzeitig aber auch Zugriff auf Einstellungen erlaubt, die es so nur in ASP.NET-Anwendungen gibt, beispielsweise die Einstellungen in **\<system.web>** .

> [!NOTE]
> Der <xref:System.Security.Cryptography>-Namespace enthält Klassen, die zusätzliche Optionen zum Verschlüsseln und Entschlüsseln von Daten bereitstellen. Verwenden Sie diese Klassen, wenn Sie Kryptografiedienste benötigen, die bei Verwendung der geschützten Konfiguration nicht verfügbar sind. Einige dieser Klassen sind Wrapper für die nicht verwaltete Microsoft CryptoAPI, während es sich bei anderen Klassen um verwaltete Implementierungen handelt. Weitere Informationen finden Sie unter [Kryptografiedienste](/previous-versions/visualstudio/visual-studio-2008/93bskf9z(v=vs.90)).

### <a name="appconfig-example"></a>Beispiel für app.config

In diesem Beispiel wird gezeigt, wie Sie die Verschlüsselung des Abschnitts **connectionStrings** in der Datei **app.config** einer Windows-Anwendung aktivieren und deaktivieren können. In diesem Beispiel übernimmt die Prozedur den Namen der Anwendung, z. B. MyApplication.exe, als Argument. Die Datei **app.config** wird dann verschlüsselt und in den Ordner kopiert, der die ausführbare Datei mit dem Namen „MyApplication.exe.config“ enthält.

> [!NOTE]
> Die Verbindungszeichenfolge kann nur auf dem Computer entschlüsselt werden, auf dem sie verschlüsselt wurde.

Der Code öffnet die Datei **app.config** mit der <xref:System.Configuration.ConfigurationManager.OpenExeConfiguration%2A>-Methode, um sie bearbeiten zu können, und die <xref:System.Configuration.ConfigurationManager.GetSection%2A>-Methode gibt den **connectionStrings**-Abschnitt zurück. Der Code überprüft nun die <xref:System.Configuration.SectionInformation.IsProtected%2A>-Eigenschaft, indem er die <xref:System.Configuration.SectionInformation.ProtectSection%2A>-Methode aufruft, um den Abschnitt zu verschlüsseln, sofern dieser nicht bereits verschlüsselt ist. Die <xref:System.Configuration.SectionInformation.UnprotectSection%2A>-Methode wird aufgerufen, um den Abschnitt zu entschlüsseln. Die <xref:System.Configuration.Configuration.Save%2A>-Methode schließt den Vorgang ab und speichert die Änderungen.

> [!NOTE]
> Sie müssen in Ihrem Projekt einen Verweis auf `System.Configuration.dll` angeben, damit der Code ausgeführt wird.

[!code-csharp[DataWorks ConnectionStrings.Encrypt#1](~/../SqlClient/doc/samples/ConnectionStrings_Encrypt.cs#1)]

### <a name="webconfig-example"></a>Beispiel für web.config

Das folgende Beispiel verwendet die <xref:System.Web.Configuration.WebConfigurationManager.OpenWebConfiguration%2A>-Methode `WebConfigurationManager`. Beachten Sie, dass Sie in diesem Fall den relativen Pfad zur Datei **Web.config** mit einer Tilde angeben können. Der Code benötigt einen Verweis auf die `System.Web.Configuration`-Klasse.  
  
[!code-csharp[DataWorks ConnectionStringsWeb.Encrypt#1](~/../SqlClient/doc/samples/ConnectionStringsWeb_Encrypt.cs#1)]

Weitere Informationen zum Absichern von ASP.NET-Anwendungen finden Sie unter [Absichern von ASP.NET-Websites](/previous-versions/aspnet/91f66yxt(v=vs.100)).

## <a name="see-also"></a>Weitere Informationen

- [Verbindungszeichenfolgen-Generatoren](connection-string-builders.md)
- [Schützen von Verbindungsinformationen](protecting-connection-information.md)
- [Verwenden der Konfigurationsklassen](/previous-versions/visualstudio/visual-studio-2008/ms228063(v=vs.90))
- [Konfigurieren von Apps](/dotnet/framework/configure-apps/index)
- [ASP.NET-Websiteverwaltung](/previous-versions/aspnet/6hy1xzbw(v=vs.100))
- [Microsoft ADO.NET für SQL Server](microsoft-ado-net-sql-server.md)
