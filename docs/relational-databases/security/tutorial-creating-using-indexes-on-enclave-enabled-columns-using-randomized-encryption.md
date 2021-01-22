---
title: Indizes für Enclave-fähige Spalten mit zufälliger Verschlüsselung (Tutorial)
description: In diesem Tutorial erfahren Sie, wie Sie Indizes für Enclave-fähige Spalten erstellen und verwenden, indem Sie eine zufällige Verschlüsselung verwenden, die in Always Encrypted mit Secure Enclaves für SQL Server und Azure SQL-Datenbank unterstützt wird.
ms.custom: seo-lt-2019
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
ms.openlocfilehash: 5e08a42f9112379951c9fcff27693ab3700b69fd
ms.sourcegitcommit: 8ca4b1398e090337ded64840bcb8d6c92d65c29e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/16/2021
ms.locfileid: "98534819"
---
# <a name="tutorial-create-and-use-indexes-on-enclave-enabled-columns-using-randomized-encryption"></a>Tutorial: Erstellen und Verwenden von Indizes für Enclave-fähige Spalten mit zufälliger Verschlüsselung

[!INCLUDE [sqlserver2019-windows-only-asdb](../../includes/applies-to-version/sqlserver2019-windows-only-asdb.md)]

In diesem Tutorial erfahren Sie, wie Sie Indizes auf Enclave-fähigen Spalten erstellen und verwenden, indem Sie eine zufällige Verschlüsselung verwenden, die in [Always Encrypted mit Secure Enclaves](encryption/always-encrypted-enclaves.md) unterstützt wird. Es wird Folgendes gezeigt:

> [!div class="checklist"]
> - Wie Sie einen Index erstellen, wenn Sie Zugriff auf die Schlüssel (den Spaltenhauptschlüssel und den Spaltenverschlüsselungsschlüssel) haben, die die Spalte schützen.
> - Wie Sie einen Index erstellen, wenn Sie keinen Zugriff auf die Schlüssel haben, die die Spalte schützen.

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie sicher, dass Sie eines der folgenden Tutorials abgeschlossen haben, bevor Sie die darauffolgenden Schritte in diesem Tutorial ausführen:

- [Tutorial: Erste Schritte mit Always Encrypted mit Secure Enclaves in SQL Server](tutorial-getting-started-with-always-encrypted-enclaves.md)
- [Tutorial: Erste Schritte mit Always Encrypted mit Secure Enclaves in Azure SQL-Datenbank](/azure/azure-sql/database/always-encrypted-enclaves-getting-started)

## <a name="step-1-enable-accelerated-database-recovery-adr-in-your-database"></a>Schritt 1: Aktivieren von verbesserten Wiederherstellung von Datenbanken (ADR) in Ihrer Datenbank

> [!NOTE]
> Dieser Schritt gilt nur für [!INCLUDE [ssnoversion-md](../../includes/ssnoversion-md.md)]. Überspringen Sie diesen Schritt, wenn Sie [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] verwenden. Die beschleunigte Datenbankwiederherstellung (ADR) ist in [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] automatisch aktiviert. Ein Deaktivieren wird nicht unterstützt.

Microsoft empfiehlt dringend, ADR in Ihrer Datenbank zu aktivieren, bevor Sie den ersten Index in einer Enclave-fähigen Spalte mit zufälliger Verschlüsselung erstellen. Weitere Informationen finden Sie im Abschnitt [Datenbankwiederherstellung](./encryption/always-encrypted-enclaves.md#database-recovery) in [Always Encrypted mit Secure Enclaves](./encryption/always-encrypted-enclaves.md).



1. Schließen Sie alle SSMS-Instanzen, die Sie im vorherigen Tutorial verwendet haben. Dadurch werden die von Ihnen geöffneten Datenbankverbindungen geschlossen. Dies ist für die Aktivierung von ADR erforderlich.
1. Öffnen Sie eine neue SSMS-Instanz, und stellen Sie als „sysadmin“ eine Verbindung zu Ihrer [!INCLUDE [ssnoversion-md](../../includes/ssnoversion-md.md)]-Instanz her, **ohne** dass Always Encrypted für die Datenverbindung aktiviert ist.
    1. Starten Sie SSMS.
    1. Geben Sie im Dialogfeld **Mit Server verbinden** Ihren Servernamen, eine Authentifizierungsmethode und Ihre Anmeldeinformationen an.
    1. Klicken Sie auf **Optionen >>** , und wählen Sie die Registerkarte **Always Encrypted** aus.
    1. Stellen Sie sicher, dass das Kontrollkästchen **Always Encrypted aktivieren (Spaltenverschlüsselung)** **nicht** ausgewählt ist.
    1. Wählen Sie **Verbinden**.
1. Öffnen Sie ein neues Abfragefenster und führen Sie die folgenden Anweisung zum Aktivieren der ADR aus.

   ```sql
   ALTER DATABASE ContosoHR SET ACCELERATED_DATABASE_RECOVERY = ON;
   ```

## <a name="step-2-create-and-test-an-index-without-role-separation"></a>Schritt 2: Erstellen und Testen eines Index ohne Rollentrennung

In diesem Schritt werden Sie einen Index für eine verschlüsselte Spalte erstellen und testen. Sie agieren als Einzelbenutzer, der die Rollen eines DBA übernimmt, der die Datenbank verwaltet, und des Datenbesitzers, der Zugriff auf die Schlüssel hat, der die Daten schützt.

1. Öffnen Sie eine neue SSMS-Instanz, und stellen Sie eine Verbindung zu Ihrer [!INCLUDE [ssnoversion-md](../../includes/ssnoversion-md.md)]-Instanz her. Always Encrypted muss dabei für die Datenverbindung **aktiviert** sein.
   1. Starten Sie eine neue SSMS-Instanz.
   1. Geben Sie im Dialogfeld **Mit Server verbinden** Ihren Servernamen, eine Authentifizierungsmethode und Ihre Anmeldeinformationen an.
   1. Klicken Sie auf **Optionen >>** , und wählen Sie die Registerkarte **Always Encrypted** aus.
   1. Aktivieren Sie das Kontrollkästchen **Always Encrypted (Spaltenverschlüsselung) aktivieren**, und geben Sie die Enclave-Nachweis-URL an (z. B. `http://hgs.bastion.local/Attestation` oder `https://MyAttestationProvider.us.attest.azure.net/attest/SgxEnclave`).
   1. Klicken Sie auf **Verbinden**.
   1. Wenn Sie aufgefordert werden, die Parametrisierung für Always Encrypted zu aktivieren, klicken Sie auf **Aktivieren**.
1. Wenn Sie nicht aufgefordert werden, die Parametrisierung für Always Encrypted zu aktivieren, überprüfen Sie, ob die Option aktiviert ist.
   1. Wählen Sie im Hauptmenü von SSMS **Tools** aus.
   2. Wählen Sie **Optionen...** aus.
   3. Navigieren Sie zu **Abfrageausführung** > **SQL Server** > **Erweitert**.
   4. Stellen Sie sicher, dass **Parametrisierung für Always Encrypted aktivieren** aktiviert ist.
   5. Klicken Sie auf **OK**.
1. Öffnen Sie ein Abfragefenster, und führen Sie die folgenden Anweisungen aus, um die Spalte **LastName** in der Tabelle **Employees** zu verschlüsseln. Sie werden in späteren Schritten einen Index auf dieser Spalte erstellen und verwenden.

   ```sql  
   ALTER TABLE [HR].[Employees]
   ALTER COLUMN [LastName] [nvarchar](50) COLLATE Latin1_General_BIN2 
   ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK1], ENCRYPTION_TYPE = Randomized, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL;
   GO   
   ALTER DATABASE SCOPED CONFIGURATION CLEAR PROCEDURE_CACHE;
   GO
   ```

1. Erstellen Sie einen Index für die Spalte **LastName**. Da Sie mit der Datenbank verbunden sind, und Always Encrypted aktiviert ist, stellt der Clienttreiber im SSMS transparent **CEK1** (der Spaltenverschlüsselungsschlüssel, der die Spalte **LastName** schützt) für die Enclave bereit, der für die Erstellung des Index benötigt wird.

   ```sql
   CREATE INDEX IX_LastName ON [HR].[Employees] ([LastName])
   INCLUDE ([EmployeeID], [FirstName], [SSN], [Salary]);
   GO
   ```

1. Führen Sie eine umfangreiche Abfrage für die Spalte **LastName** aus, und überprüfen Sie, ob SQL Server beim Ausführen der Abfrage den Index verwendet.
   1. Stellen Sie im gleichen oder einem neuen Abfragefenster sicher, dass die Schaltfläche **Live-Abfragestatistik einschließen** in der Symbolleiste aktiviert ist.
   1. Führen Sie die folgenden Abfrage aus.

       ```sql
       DECLARE @LastNamePrefix NVARCHAR(50) = 'Aber%';
       SELECT * FROM [HR].[Employees] WHERE [LastName] LIKE @LastNamePrefix;
       GO
      ```

   1. Achten Sie auf der Registerkarte **Live-Abfragestatistik einschließen** (im unteren Teil des Abfragefensters) darauf, dass die Abfrage den Index verwendet.

## <a name="step-3-create-an-index-with-role-separation"></a>Schritt 3: Erstellen eines Index mit Rollentrennung

In diesem Schritt erstellen Sie einen Index auf einer verschlüsselten Spalte, der vorgibt, zwei verschiedene Benutzer zu sein. Ein Benutzer ist ein DBA, der einen Index erstellen muss, aber keinen Zugriff auf die Schlüssel hat. Der andere Benutzer ist ein Datenbesitzer, der Zugriff auf die Schlüssel hat.

1. Führen Sie unter Verwendung der SSMS-Instanz, **ohne** dass Always Encrypted aktiviert ist, die folgende Anweisung aus, um den Index auf der Spalte **LastName** zu löschen.

   ```sql
   DROP INDEX IX_LastName ON [HR].[Employees]; 
   GO
   ```

1. Als Datenbesitzer (oder eine Anwendung, die Zugriff auf die Schlüssel hat) füllen Sie dann den Cache innerhalb der Enclave mit **CEK1**.

   > [!NOTE]
   > Wenn Sie Ihre SQL Server-Instanz nach **Schritt 2: Erstellen und Testen eines Index ohne Rollentrennung** neu gestartet haben, ist dieser Schritt überflüssig, da **CEK1** bereits im Cache vorhanden ist. Wir haben diesen Schritt hinzugefügt, um zu zeigen, wie ein Datenbesitzer einen Schlüssel für die Enclave bereitstellen kann, wenn er nicht bereits in der Enclave vorhanden ist.

   1. Führen Sie in einer SSMS-Instanz **mit** aktiviertem Always Encrypted die folgenden Anweisungen in einem Abfragefenster aus. Die Anweisung sendet alle Enclave-fähigen Spaltenverschlüsselungsschlüssel an die Enclave. Weitere Informationen finden Sie unter [sp_enclave_send_keys](../system-stored-procedures/sp-enclave-send-keys-sql.md).

        ```sql
        EXEC sp_enclave_send_keys;
        GO
        ```

   1. Alternativ zur Ausführung der oben genannten gespeicherten Prozedur können Sie eine DML-Abfrage ausführen, die die Enklave in der Spalte **LastName** verwendet. Damit wird nur **CEK1** in die Enclave eingetragen.

        ```sql
        DECLARE @LastNamePrefix NVARCHAR(50) = 'Aber%';
        SELECT * FROM [HR].[Employees] WHERE [LastName] LIKE @LastNamePrefix;
        GO
        ```

1. Erstellen Sie einen Index als DBA.
    1. Führen Sie in einer SSMS-Instanz **ohne** aktiviertem Always Encrypted die folgenden Anweisungen in einem Abfragefenster aus.

        ```sql
        CREATE INDEX IX_LastName ON [HR].[Employees] ([LastName])
        INCLUDE ([EmployeeID], [FirstName], [SSN], [Salary]);
        GO
        ```

1. Führen Sie als Datenbesitzer eine umfangreiche Abfrage für die Spalte **LastName** aus, und überprüfen Sie, ob SQL Server beim Ausführen der Abfrage den Index verwendet.
   1. Wählen Sie in der SSMS-Instanz **mit** aktiviertem Always Encrypted ein vorhandenes Abfragefenster aus oder öffnen Sie ein neues, und stellen Sie sicher, dass die Schaltfläche **Live-Abfragestatistik einschließen** in der Symbolleiste aktiviert ist.
   1. Führen Sie die folgenden Abfrage aus. 

        ```sql
        DECLARE @LastNamePrefix NVARCHAR(50) = 'Aber%';
        SELECT * FROM [HR].[Employees] WHERE [LastName] LIKE @LastNamePrefix;
        GO
        ```

   1. Achten Sie auf der Registerkarte **Live-Abfragestatistik einschließen** (im unteren Teil des Abfragefensters) darauf, dass die Abfrage den Index verwendet.

## <a name="next-steps"></a>Nächste Schritte
- [Tutorial: Entwickeln einer .NET-Anwendung mithilfe von Always Encrypted mit Secure Enclaves](../../connect/ado-net/sql/tutorial-always-encrypted-enclaves-develop-net-apps.md)
- [Tutorial: Entwickeln einer .NET Framework-Anwendung mithilfe von Always Encrypted mit Secure Enclaves](tutorial-always-encrypted-enclaves-develop-net-framework-apps.md)

## <a name="see-also"></a>Weitere Informationen
- [Erstellen und Verwenden von Indizes in Spalten mithilfe von Always Encrypted mit Secure Enclaves](encryption/always-encrypted-enclaves-create-use-indexes.md)
