---
description: 'Tutorial: Entwickeln einer .NET-Anwendung mithilfe von Always Encrypted mit Secure Enclaves'
title: 'Tutorial: Entwickeln einer .NET-Anwendung mithilfe von Always Encrypted mit Secure Enclaves | Microsoft-Dokumentation'
ms.custom: ''
ms.date: 07/09/2020
ms.reviewer: v-kaywon
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: tutorial
author: karinazhou
ms.author: v-jizho2
ms.openlocfilehash: 59c377d5055e8eb3858e1005c50d9c4dfb8334ab
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771543"
---
# <a name="tutorial-develop-a-net-application-using-always-encrypted-with-secure-enclaves"></a>Tutorial: Entwickeln einer .NET-Anwendung mithilfe von Always Encrypted mit Secure Enclaves

[!INCLUDE [sqlserver2019-windows-only](../../../includes/applies-to-version/sqlserver2019-windows-only.md)]

[!INCLUDE [appliesto-netfx-netcore-xxxx-md](../../../includes/appliesto-netfx-netcore-xxxx-md.md)]

In diesem Tutorial erfahren Sie, wie Sie eine einfache Anwendung entwickeln, die Datenbankabfragen ausgibt, die eine serverseitige Secure Enclave für [Always Encrypted mit Secure Enclaves](../../../relational-databases/security/encryption/always-encrypted-enclaves.md) verwendet.

> [!NOTE]
> Always Encrypted mit Secure Enclaves wird nur unter Windows unterstützt.

## <a name="prerequisites"></a>Voraussetzungen

Dieses Tutorial ist die Fortsetzung von [Tutorial: Erste Schritte mit Always Encrypted mit Secure Enclaves mithilfe von SSMS](../../../relational-databases/security/tutorial-getting-started-with-always-encrypted-enclaves.md). Schließen Sie es zunächst ab, bevor Sie mit den folgenden Schritten fortfahren.

Darüber hinaus benötigen Sie Visual Studio (Version 2019 wird empfohlen). Den Download finden Sie unter [https://visualstudio.microsoft.com/](https://visualstudio.microsoft.com). In Ihrer Anwendungsentwicklungsumgebung muss .NET Framework 4.6 oder höher oder .NET Core 2.1 oder höher verwendet werden.

## <a name="step-1-set-up-your-visual-studio-project"></a>Schritt 1: Einrichten Ihres Visual Studio-Projekts

Damit Sie Always Encrypted mit Secure Enclaves in einer .NET Framework-Anwendung verwenden können, müssen Sie sicherstellen, dass Ihre Anwendung auf .NET Framework 4.6 oder höher ausgerichtet ist. Damit Sie Always Encrypted mit Secure Enclaves in einer .NET Core-Anwendung verwenden können, müssen Sie sicherstellen, dass Ihre Anwendung auf .NET Core 2.1 oder ausgerichtet ist.

Wenn Sie Ihren Spaltenhauptschlüssel im Azure Key Vault speichern, müssen Sie außerdem Ihre Anwendung und das NuGet-Paket [Microsoft.Data.SqlClient.AlwaysEncrypted.AzureKeyVaultProvider](https://www.nuget.org/packages/Microsoft.Data.SqlClient.AlwaysEncrypted.AzureKeyVaultProvider) integrieren.

1. Öffnen Sie Visual Studio.

2. Erstellen Sie ein neues C\#-Konsolen-App-Projekt (.NET Framework/Core).

3. Stellen Sie sicher, dass das Projekt mindestens auf .NET Framework 4.6 oder .NET Core 2.1 ausgerichtet ist. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, wählen Sie „Eigenschaften“ aus, und legen Sie das Zielframework fest.

4. Installieren Sie das folgende NuGet-Paket, indem Sie zu **Tools** (Hauptmenü) > **NuGet-Paket-Manager** > **Paket-Manager-Konsole** gehen. Führen Sie den folgenden Code in der Paket-Manager-Konsole aus.

   ```powershell
   Install-Package Microsoft.Data.SqlClient -Version 1.1.0
   ```

5. Wenn Sie Azure Key Vault für die Speicherung Ihrer Spaltenhauptschlüssel verwenden, installieren Sie die folgenden NuGet-Pakete, indem Sie zu **Tools** (Hauptmenü) > **NuGet-Paket-Manager** > **Paket-Manager-Konsole** gehen. Führen Sie den folgenden Code in der Paket-Manager-Konsole aus.

   ```powershell
   Install-Package Microsoft.Data.SqlClient.AlwaysEncrypted.AzureKeyVaultProvider -Version 1.0.0
   Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
   ```

6. Geben Sie `Attestation Protocol` und `Enclave Attestation Url` in der Verbindungszeichenfolge an, die in Ihrer Anwendung zur Kommunikation mit der SQL Server-Instanz verwendet werden soll.

  ```cs
   Attestation Protocol = HGS; Enclave Attestation Url = http://hgs.bastion.local/Attestation; Column Encryption Setting = Enabled
   ```

## <a name="step-2-implement-your-application-logic"></a>Schritt 2: Implementieren Ihrer Anwendungslogik

Ihre Anwendung stellt eine Verbindung mit der Datenbank **ContosoHR** aus dem [Tutorial: Erste Schritte mit Always Encrypted mit Secure Enclaves mithilfe von SSMS](../../../relational-databases/security/tutorial-getting-started-with-always-encrypted-enclaves.md) her und führt eine Abfrage aus, die das `LIKE`-Prädikat in der Spalte **SSN** sowie einen Bereichsvergleich in der Spalte **Salary** enthält.

1. Ersetzen Sie den Inhalt der (von Visual Studio generierten) Datei „Program.cs“ durch folgenden Code. Aktualisieren Sie die Datenverbindungszeichenfolge mit Ihrem Servernamen und die Enclave-Nachweis-URL für Ihre Umgebung. Sie können auch die Datenbankauthentifizierungseinstellungen aktualisieren.

    ```cs
    using System;
    using Microsoft.Data.SqlClient;
    using System.Data;

    namespace ConsoleApp1
    {
        class Program
        {
            static void Main(string[] args)
            {

                string connectionString = "Data Source = myserver; Initial Catalog = ContosoHR; Column Encryption Setting = Enabled;Attestation Protocol = HGS; Enclave Attestation Url = http://hgs.bastion.local/Attestation; Integrated Security = true";

                using (SqlConnection connection = new SqlConnection(connectionString))
                {
                    connection.Open();

                    SqlCommand cmd = connection.CreateCommand();
                    cmd.CommandText = @"SELECT [SSN], [FirstName], [LastName], [Salary] FROM [dbo].[Employees] WHERE [SSN] LIKE @SSNPattern AND [Salary] > @MinSalary;";

                    SqlParameter paramSSNPattern = cmd.CreateParameter();

                    paramSSNPattern.ParameterName = @"@SSNPattern";
                    paramSSNPattern.DbType = DbType.AnsiStringFixedLength;
                    paramSSNPattern.Direction = ParameterDirection.Input;
                    paramSSNPattern.Value = "%1111";
                    paramSSNPattern.Size = 11;

                    cmd.Parameters.Add(paramSSNPattern);

                    SqlParameter MinSalary = cmd.CreateParameter();

                    MinSalary.ParameterName = @"@MinSalary";
                    MinSalary.DbType = DbType.Currency;
                    MinSalary.Direction = ParameterDirection.Input;
                    MinSalary.Value = 900;

                    cmd.Parameters.Add(MinSalary);
                    cmd.ExecuteNonQuery();

                    SqlDataReader reader = cmd.ExecuteReader();
                    while (reader.Read())

                    {
                        Console.WriteLine(reader);
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

- [Verwenden von Always Encrypted mit dem Microsoft .NET-Datenanbieter für SQL Server](sqlclient-support-always-encrypted.md)
- [Beispiel zur Veranschaulichung der Verwendung von Always Encrypted mit einem Azure Key Vault-Anbieter](azure-key-vault-example.md)
- [Beispiel zur Veranschaulichung der Verwendung von Always Encrypted mit Secure Enclaves mit einem Azure Key Vault-Anbieter](azure-key-vault-enclave-example.md)
