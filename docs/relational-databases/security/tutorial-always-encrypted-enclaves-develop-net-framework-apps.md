---
description: 'Tutorial: Entwickeln einer .NET Framework-Anwendung mithilfe von Always Encrypted mit Secure Enclaves'
title: 'Tutorial: Entwickeln einer .NET Framework-Anwendung mithilfe von Always Encrypted mit Secure Enclaves | Microsoft-Dokumentation'
ms.custom: ''
ms.date: 01/15/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: vanto
ms.suite: sql
ms.technology: security
ms.tgt_pltfrm: ''
ms.topic: tutorial
author: jaszymas
ms.author: jaszymas
monikerRange: '>= sql-server-ver15'
ms.openlocfilehash: 092220a2ef2715b8d5cd414ab5a4bc3e3493acb5
ms.sourcegitcommit: 8ca4b1398e090337ded64840bcb8d6c92d65c29e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/16/2021
ms.locfileid: "98534739"
---
# <a name="tutorial-develop-a-net-framework-application-using-always-encrypted-with-secure-enclaves"></a>Tutorial: Entwickeln einer .NET Framework-Anwendung mithilfe von Always Encrypted mit Secure Enclaves

[!INCLUDE [sqlserver2019-windows-only-asdb](../../includes/applies-to-version/sqlserver2019-windows-only-asdb.md)]

In diesem Tutorial erfahren Sie, wie Sie eine Anwendung entwickeln, die Datenbankabfragen ausgibt, die eine serverseitige Secure Enclave für [Always Encrypted mit Secure Enclaves](encryption/always-encrypted-enclaves.md) verwendet. 

## <a name="prerequisites"></a>Voraussetzungen
Stellen Sie sicher, dass Sie eines der folgenden Tutorials abgeschlossen haben, bevor Sie die darauffolgenden Schritte in diesem Tutorial ausführen:

- [Tutorial: Erste Schritte mit Always Encrypted mit Secure Enclaves in SQL Server](tutorial-getting-started-with-always-encrypted-enclaves.md)
- [Tutorial: Erste Schritte mit Always Encrypted mit Secure Enclaves in Azure SQL-Datenbank](/azure/azure-sql/database/always-encrypted-enclaves-getting-started)

Darüber hinaus benötigen Sie Visual Studio (Version 2019 wird empfohlen). Den Download finden Sie unter [https://visualstudio.microsoft.com/](https://visualstudio.microsoft.com). Auf Ihrem Anwendungsentwicklungscomputer muss .NET Framework 4.7.2 oder höher ausgeführt werden.

## <a name="step-1-set-up-your-visual-studio-project"></a>Schritt 1: Einrichten Ihres Visual Studio-Projekts

Damit Sie Always Encrypted mit Secure Enclaves in einer.NET Framework-Anwendung verwenden können, müssen Sie sicherstellen, dass Ihre Anwendung auf .NET Framework 4.7.2 basiert und mit dem [Microsoft.SqlServer.Management.AlwaysEncrypted.EnclaveProviders-NuGet-Paket](https://www.nuget.org/packages/Microsoft.SqlServer.Management.AlwaysEncrypted.EnclaveProviders) integriert ist. Wenn Sie Ihren Spaltenhauptschlüssel im Azure Key Vault speichern, müssen Sie Ihre Anwendung außerdem mit Version 2.2.0 oder höher des [Microsoft.SqlServer.Management.AlwaysEncrypted.AzureKeyVaultProvider -NuGet-Pakets](https://www.nuget.org/packages/Microsoft.SqlServer.Management.AlwaysEncrypted.AzureKeyVaultProvider) integrieren. 

1. Öffnen Sie Visual Studio.

2. Erstellen Sie ein neues C\#-Konsolen-App-Projekt (.NET Framework).

3. Stellen Sie sicher, dass das Projekt mindestens auf .NET Framework 4.7.2 ausgerichtet ist. Klicken Sie mit der rechten Maustaste auf das Projekt im Projektmappen-Explorer, wählen Sie **Eigenschaften** aus und legen Sie das **Zielframework** auf .NET Framework 4.7.2 fest.

4. Installieren Sie das folgende NuGet-Paket, indem Sie zu **Tools** (Hauptmenü) > **NuGet-Paket-Manager** > **Paket-Manager-Konsole** gehen. Führen Sie den folgenden Code in der Paket-Manager-Konsole aus.

   ```powershell
   Install-Package Microsoft.SqlServer.Management.AlwaysEncrypted.EnclaveProviders -IncludePrerelease
   ```

5. Wenn Sie Azure Key Vault für die Speicherung Ihrer Spaltenhauptschlüssel verwenden, installieren Sie die folgenden NuGet-Pakete, indem Sie zu **Tools** (Hauptmenü) > **NuGet-Paket-Manager** > **Paket-Manager-Konsole** gehen. Führen Sie den folgenden Code in der Paket-Manager-Konsole aus.

   ```powershell
   Install-Package Microsoft.SqlServer.Management.AlwaysEncrypted.AzureKeyVaultProvider -IncludePrerelease -Version 2.2.0
   Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
   ```

6. Öffnen Sie die Datei „App.config“ für Ihr Projekt.

7. Suchen Sie nach dem Abschnitt `<configuration>`, und führen Sie die `<configSections>`-Abschnitte hinzu oder aktualisieren Sie diese.

   1. Wenn der Abschnitt `<configuration>` **nicht** den Abschnitt `<configSections>` enthält, fügen Sie den folgenden Inhalt direkt unter `<configuration>` hinzu.

      ```xml
      <configSections>
        <section name="SqlColumnEncryptionEnclaveProviders" type="System.Data.SqlClient.SqlColumnEncryptionEnclaveProviderConfigurationSection, System.Data, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" />
      </configSections>
      ```

   2. Wenn der Abschnitt `<configuration>` den Abschnitt `<configSections>` bereits enthält, fügen Sie die folgende Zeile im `<configSections>`-Abschnitt hinzu:

      ```xml
      <section name="SqlColumnEncryptionEnclaveProviders"  type="System.   Data.SqlClient.   SqlColumnEncryptionEnclaveProviderConfigurationSection, System.   Data,  Version=4.0.0.0, Culture=neutral,    PublicKeyToken=b77a5c561934e089" />
      ```

8. Fügen Sie im Abschnitt `<configuration>` unter `</configSections>` einen neuen Abschnitt hinzu, um einen Enclave-Anbieter festzulegen, der für die Bestätigung und Interaktion mit Ihren serverseitigen sicheren Enclaves verwendet werden soll.

   1. Wenn Sie [!INCLUDE [ssnoversion-md](../../includes/ssnoversion-md.md)] und den Host-Überwachungsdienst (Host Guardian Service, HGS) verwenden (Sie verwenden die Datenbank aus [Tutorial: Erste Schritte mit Always Encrypted mit Secure Enclaves in SQL Server](tutorial-getting-started-with-always-encrypted-enclaves.md)), fügen Sie den Abschnitt unten hinzu.

      ```xml
      <SqlColumnEncryptionEnclaveProviders>
        <providers>
          <add name="VBS"  type="Microsoft.SqlServer.Management.AlwaysEncrypted.EnclaveProviders.HostGuardianServiceEnclaveProvider,  Microsoft.SqlServer.Management.AlwaysEncrypted.EnclaveProviders,    Version=15.0.0.0, Culture=neutral, PublicKeyToken=89845dcd8080cc91"/>
        </providers>
      </SqlColumnEncryptionEnclaveProviders>
      ```

      Hier ist ein vollständiges Beispiel für eine app.config-Datei für eine einfache Konsolenanwendung.

      ```xml
      <?xml version="1.0" encoding="utf-8" ?>
      <configuration>
        <configSections>
          <section name="SqlColumnEncryptionEnclaveProviders" type="System.Data.SqlClient.SqlColumnEncryptionEnclaveProviderConfigurationSection, System.Data, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" />
        </configSections>
        <SqlColumnEncryptionEnclaveProviders>
          <providers>
            <add name="VBS"  type="Microsoft.SqlServer.Management.AlwaysEncrypted.EnclaveProviders.HostGuardianServiceEnclaveProvider,  Microsoft.SqlServer.Management.AlwaysEncrypted.EnclaveProviders,    Version=15.0.0.0, Culture=neutral, PublicKeyToken=89845dcd8080cc91"/>
          </providers>
        </SqlColumnEncryptionEnclaveProviders>
        <startup> 
         <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.7.2" />
        </startup>
      </configuration>
      ```

   1. Wenn Sie [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] und Microsoft Azure Attestation verwenden (Sie verwenden die Datenbank aus [Tutorial: Erste Schritte mit Always Encrypted mit Secure Enclaves in Azure SQL-Datenbank](/azure/azure-sql/database/always-encrypted-enclaves-getting-started)), fügen Sie den Abschnitt unten hinzu.

      ```xml
      <SqlColumnEncryptionEnclaveProviders>
        <providers>
          <add name="SGX" type="Microsoft.SqlServer.Management.AlwaysEncrypted.EnclaveProviders.AzureAttestationEnclaveProvider, Microsoft.SqlServer.Management.AlwaysEncrypted.EnclaveProviders, Version=15.0.0.0, Culture=neutral, PublicKeyToken=89845dcd8080cc91" />
        </providers>
      </SqlColumnEncryptionEnclaveProviders>
      ```

      Hier ist ein vollständiges Beispiel für eine app.config-Datei für eine einfache Konsolenanwendung.

      ```xml
      <?xml version="1.0" encoding="utf-8" ?>
      <configuration>
        <configSections>
          <section name="SqlColumnEncryptionEnclaveProviders" type="System.Data.SqlClient.SqlColumnEncryptionEnclaveProviderConfigurationSection, System.Data, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" />
        </configSections>
        <SqlColumnEncryptionEnclaveProviders>
          <providers>
            <add name="SGX" type="Microsoft.SqlServer.Management.AlwaysEncrypted.EnclaveProviders.AzureAttestationEnclaveProvider, Microsoft.SqlServer.Management.AlwaysEncrypted.EnclaveProviders, Version=15.0.0.0, Culture=neutral, PublicKeyToken=89845dcd8080cc91" />
          </providers>
        </SqlColumnEncryptionEnclaveProviders>
        <startup> 
         <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.7.2" />
        </startup>
      </configuration>
      ```

## <a name="step-2-implement-your-application-logic"></a>Schritt 2: Implementieren Ihrer Anwendungslogik

Ihre Anwendung stellt eine Verbindung mit der **ContosoHR**-Datenbank aus dem [Tutorial: Erste Schritte mit Always Encrypted mit Secure Enclaves in SQL Server](tutorial-getting-started-with-always-encrypted-enclaves.md) oder [Tutorial: Erste Schritte mit Always Encrypted mit Secure Enclaves in Azure SQL-Datenbank](/azure/azure-sql/database/always-encrypted-enclaves-getting-started) her und führt eine Abfrage aus, die das `LIKE`-Prädikat in der Spalte **SSN** sowie einen Bereichsvergleich in der Spalte **Salary** enthält.

1. Ersetzen Sie mit dem folgenden Code den Inhalt der Datei „Program.cs“ (wird von Visual Studio generiert). Aktualisieren Sie die Datenverbindungszeichenfolge mit Ihrem Servernamen, den Einstellungen zur Datenbankauthentifizierung und die Enclave-Nachweis-URL für Ihre Umgebung.

    ```cs
    using System;
    using System.Data.SqlClient;
    using System.Data;

    namespace ConsoleApp1
    {
        class Program
        {
            static void Main(string[] args)
            {
    
                string connectionString = "Data Source = myserver; Initial Catalog = ContosoHR; Column Encryption Setting = Enabled;Enclave Attestation Url = http://hgs.bastion.local/Attestation; Integrated Security = true";

                //string connectionString = "Data Source = myserver.database.windows.net; Initial Catalog = ContosoHR; Column Encryption Setting = Enabled;Enclave Attestation Url = https://myattestationprovider.uks.attest.azure.net/attest/SgxEnclave; User ID=user; Password=password";

                using (SqlConnection connection = new SqlConnection(connectionString))
                {
                    connection.Open();

                    SqlCommand cmd = connection.CreateCommand();
                    cmd.CommandText = @"SELECT [SSN], [FirstName], [LastName], [Salary] FROM [HR].[Employees] WHERE [SSN] LIKE @SSNPattern AND [Salary] > @MinSalary;";

                    SqlParameter paramSSNPattern = cmd.CreateParameter();

                    paramSSNPattern.ParameterName = @"@SSNPattern";
                    paramSSNPattern.DbType = DbType.AnsiStringFixedLength;
                    paramSSNPattern.Direction = ParameterDirection.Input;
                    paramSSNPattern.Value = "%9838";
                    paramSSNPattern.Size = 11;

                    cmd.Parameters.Add(paramSSNPattern);

                    SqlParameter MinSalary = cmd.CreateParameter();

                    MinSalary.ParameterName = @"@MinSalary";
                    MinSalary.DbType = DbType.Currency;
                    MinSalary.Direction = ParameterDirection.Input;
                    MinSalary.Value = 20000;

                    cmd.Parameters.Add(MinSalary);
                    cmd.ExecuteNonQuery();
    
                    SqlDataReader reader = cmd.ExecuteReader();
                    while (reader.Read())

                    {
                        Console.WriteLine(reader[0] + ", " + reader[1] + ", " + reader[2] + ", " + reader[3]);
                    }   
                    Console.ReadKey();
                }
            }
        }
    }
    ```

2. Erstellen Sie die Anwendung, und führen Sie sie aus.  

## <a name="see-also"></a>Weitere Informationen

- [Verwenden von Always Encrypted mit dem .NET Framework-Datenanbieter für SQL Server](encryption/develop-using-always-encrypted-with-net-framework-data-provider.md)
- [Tutorial: Entwickeln einer .NET-Anwendung mithilfe von Always Encrypted mit Secure Enclaves](../../connect/ado-net/sql/tutorial-always-encrypted-enclaves-develop-net-apps.md)
