---
title: ADO.NET-Verbindungs-Manager | Microsoft-Dokumentation
description: Ein ADO.NET-Verbindungs-Manager ermöglicht einem Paket den Zugriff auf Datenquellen mithilfe eines .NET-Anbieters.
ms.custom: ''
ms.date: 05/24/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.adonetconnection.f1
helpviewer_keywords:
- connection managers [Integration Services], ADO.NET
- ADO.NET connection manager [Integration Services]
- connections [Integration Services], ADO.NET
ms.assetid: fc5daa2f-0159-4bda-9402-c87f1035a96f
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 8c2efe0145643f3f9e969c298e76115967477019
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/17/2020
ms.locfileid: "97637564"
---
# <a name="adonet-connection-manager"></a>ADO.NET-Verbindungs-Manager

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


Ein [!INCLUDE[vstecado](../../includes/vstecado-md.md)] -Verbindungs-Manager ermöglicht einem Paket den Zugriff auf Datenquellen mithilfe eines .NET-Anbieters. In der Regel verwenden Sie diesen Verbindungs-Manager für den Zugriff auf Datenquellen wie [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Sie können darüber hinaus auch auf Datenquellen zugreifen, die durch OLE DB und XML in benutzerdefinierten Tasks verfügbar gemacht werden, die mit einer Programmiersprache wie C# in verwaltetem Code geschrieben sind.  
  
Wenn Sie einem Paket einen [!INCLUDE[vstecado](../../includes/vstecado-md.md)]-Verbindungs-Manager hinzufügen, erstellt [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] einen Verbindungs-Manager, der zur Runtime als [!INCLUDE[vstecado](../../includes/vstecado-md.md)]-Verbindung aufgelöst wird. Es werden die Eigenschaften des Verbindungs-Managers festgelegt und der Verbindungs-Manager wird der Sammlung **Verbindungen** im Paket hinzugefügt.  
  
Die `ConnectionManagerType`-Eigenschaft des Verbindungs-Managers ist auf `ADO.NET` festgelegt. Der Wert von `ConnectionManagerType` ist qualifiziert, um den Namen des .NET-Anbieters einzuschließen, der vom Verbindungs-Manager verwendet wird.  
  
## <a name="adonet-connection-manager-troubleshooting"></a>Problembehandlung für den ADO.NET-Verbindungs-Manager  
Sie können die vom [!INCLUDE[vstecado](../../includes/vstecado-md.md)] -Verbindungs-Manager an externe Datenanbieter gerichteten Aufrufe protokollieren. Anschließend können Sie Probleme bei Verbindungen behandeln, die vom [!INCLUDE[vstecado](../../includes/vstecado-md.md)]-Verbindungs-Manager mit externen Datenquellen hergestellt werden. Aktivieren Sie zum Protokollieren der vom [!INCLUDE[vstecado](../../includes/vstecado-md.md)]-Verbindungs-Manager an externe Datenprovider gerichteten Aufrufe die Paketprotokollierung, und wählen Sie das **Diagnostic**-Ereignis auf Paketebene aus. Weitere Informationen finden Sie unter [Behandeln von Problemen mit Paketausführungstools](../../integration-services/troubleshooting/troubleshooting-tools-for-package-execution.md).  
  
Daten bestimmter [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Datumsdatentypen generieren beim Lesen durch einen [!INCLUDE[vstecado](../../includes/vstecado-md.md)]-Verbindungs-Manager die in der folgenden Tabelle dargestellten Ergebnisse.  
  
|SQL Server-Datentyp|Ergebnis|  
|--------------------------|------------|  
|**time**, **datetimeoffset**|Das Paket erzeugt einen Fehler, sofern das Paket keine parametrisierten SQL-Befehle verwendet. Um parametrisierte SQL-Befehle zu verwenden, verwenden Sie den Task SQL ausführen im Paket. Weitere Informationen finden Sie unter [SQL ausführen (Task)](../../integration-services/control-flow/execute-sql-task.md) und [Parameter und Rückgabecodes im Task „SQL ausführen“](../control-flow/execute-sql-task.md).|  
|**datetime2**|Der [!INCLUDE[vstecado](../../includes/vstecado-md.md)] -Verbindungs-Manager schneidet den Millisekundenwert ab.|  
  
> [!NOTE]  
>  Weitere Informationen zu [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Datentypen sowie deren Zuordnung zu [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]-Datentypen finden Sie unter [Datentypen &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md) und [SQL Server Integration Services-Datentypen](../../integration-services/data-flow/integration-services-data-types.md).  
  
## <a name="adonet-connection-manager-configuration"></a>Konfiguration des ADO.NET-Verbindungs-Managers  
  
Sie können Eigenschaften mit dem [!INCLUDE[ssIS](../../includes/ssis-md.md)]-Designer oder programmgesteuert festlegen.  
  
-   Stellen Sie eine Verbindungszeichenfolge bereit, die die Anforderungen des ausgewählten .NET-Anbieters erfüllt.  
  
-   Schließen Sie in Abhängigkeit vom Anbieter den Namen der Datenquelle ein, mit der eine Verbindung hergestellt werden soll.  
  
-   Stellen Sie entsprechende Sicherheitsanmeldeinformationen für den ausgewählten Anbieter bereit.  
  
-   Geben Sie an, ob die im Verbindungs-Manager erstellte Verbindung zur Runtime beibehalten wird.  
  
Viele der Konfigurationsoptionen des [!INCLUDE[vstecado](../../includes/vstecado-md.md)] -Verbindungs-Managers hängen vom .NET-Anbieter ab, der vom Verbindungs-Manager verwendet wird.  
  
Weitere Informationen zu den Eigenschaften, die Sie im [!INCLUDE[ssIS](../../includes/ssis-md.md)]-Designer festlegen können, finden Sie unter [Konfigurieren des ADO.NET-Verbindungs-Managers]().  
  
 Weitere Informationen zum programmgesteuerten Konfigurieren eines Verbindungs-Managers finden Sie unter <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager> und [Programmgesteuertes Hinzufügen von Verbindungen](../../integration-services/building-packages-programmatically/adding-connections-programmatically.md)festgelegt.  
  
### <a name="configure-adonet-connection-manager"></a>Konfigurieren des ADO.NET-Verbindungs-Managers
Verwenden Sie das Dialogfeld **ADO.NET-Verbindungs-Manager konfigurieren**, um eine Verbindung zu einer Datenquelle hinzuzufügen, auf die mithilfe eines .NET Framework-Datenanbieter zugegriffen werden kann. Ein solcher Anbieter ist beispielsweise der SqlClient-Anbieter. Der Verbindungs-Manager kann eine vorhandene Verbindung verwenden, oder Sie können eine neue erstellen.  
  
 Weitere Informationen zum ADO.NET-Verbindungs-Manager finden Sie unter [ADO.NET Connection Manager](../../integration-services/connection-manager/ado-net-connection-manager.md).  
  
#### <a name="options"></a>Tastatur  
**Datenverbindungen**  
Wählen Sie in der Liste eine vorhandene ADO.NET.Datenverbindung aus.  
  
**Datenverbindungseigenschaften**  
Zeigt Eigenschaften und Werte für die ausgewählte ADO.NET-Datenverbindung an.  
  
**Neu**  
Erstellen Sie mithilfe des Dialogfelds **Verbindungs-Manager** eine ADO.NET-Datenverbindung.  
  
**Löschen**  
Wählen Sie eine Verbindung aus, und löschen Sie sie mithilfe von **Löschen**.  
  
#### <a name="managed-identities-for-azure-resources-authentication"></a>Verwaltete Identitäten für die Authentifizierung von Azure-Ressourcen
Beim Ausführen von SSIS-Paketen in [Azure-SSIS Integration Runtime in Azure Data Factory](/azure/data-factory/concepts-integration-runtime#azure-ssis-integration-runtime) können Sie die [verwaltete Identität](/azure/data-factory/connector-azure-sql-database#managed-identity), die Ihrer Data Factory zugeordnet ist, zur Authentifizierung von Azure SQL-Datenbank oder Azure SQL Managed Instance verwenden. Die angegebene Factory kann mithilfe dieser Identität auf Daten zugreifen und Daten aus der oder in die Datenbank kopieren.

> [!NOTE]
>  Wenn Sie die Azure Active Directory-Authentifizierung (einschließlich der Authentifizierung der verwalteten Identität) zum Herstellen einer Verbindung mit der Azure SQL-Datenbank-Instanz (oder Azure SQL Managed Instance) verwenden, kann es zu einem Problem bei der Paketausführung oder zu unerwarteten Behavior Changes kommen. Weitere Informationen finden Sie unter [Features und Einschränkungen von Azure AD](/azure/sql-database/sql-database-aad-authentication#azure-ad-features-and-limitations).

Damit Sie die Authentifizierung der verwalteten Identität für die Azure SQL-Datenbank verwenden können, führen Sie die folgenden Schritte zum Konfigurieren der Datenbank aus:

1. [Stellen Sie einen Azure Active Directory-Administrator](/azure/sql-database/sql-database-aad-authentication-configure#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server) für Ihren Azure SQL-Server im Azure-Portal bereit, wenn dies noch nicht geschehen ist. Der Azure AD-Administrator kann ein Azure AD-Benutzer oder eine Azure AD-Gruppe sein. Wenn Sie der Gruppe mit der verwalteten Identität eine Administratorrolle zuweisen, überspringen Sie die Schritte 2 und 3. Der Administrator hat vollen Zugriff auf die Datenbank.

1. [Erstellen Sie für eine eigenständige Datenbank](/azure/sql-database/sql-database-aad-authentication-configure#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities) Benutzer für die verwaltete Data Factory-Identität. Stellen Sie eine Verbindung mit der Datenbank her, aus der oder in die Sie Daten kopieren möchten. Verwenden Sie dazu Tools wie SSMS und eine Azure AD-Identität, die mindestens über ALTER ANY USER-Berechtigung verfügt. Führen Sie folgenden T-SQL-Code aus: 
    
    ```sql
    CREATE USER [your data factory name] FROM EXTERNAL PROVIDER;
    ```

1. Gewähren Sie der verwalteten Data Factory-Identität die notwendigen Berechtigungen, und gehen Sie dabei so vor wie für SQL-Benutzer und andere Benutzer. Informationen zu den entsprechenden Rollen finden Sie unter [Rollen auf Datenbankebene](../../relational-databases/security/authentication-access/database-level-roles.md). Führen Sie den folgenden Code aus. Weitere Optionen finden Sie in [diesem Dokument](../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md).

    ```sql
    EXEC sp_addrolemember [role name], [your data factory name];
    ```

Führen Sie die folgenden Schritte zum Konfigurieren Ihrer Datenbank aus, um die Authentifizierung der verwalteten Identität für Azure SQL Managed Instance zu verwenden:
    
1. [Stellen Sie einen Azure Active Directory-Administrator](/azure/sql-database/sql-database-aad-authentication-configure#provision-an-azure-active-directory-administrator-for-your-managed-instance) für Ihre verwaltete Instanz im Azure-Portal bereit, wenn dies noch nicht geschehen ist. Der Azure AD-Administrator kann ein Azure AD-Benutzer oder eine Azure AD-Gruppe sein. Wenn Sie der Gruppe mit der verwalteten Identität eine Administratorrolle zuweisen, überspringen Sie die Schritte 2 bis 4. Der Administrator hat vollen Zugriff auf die Datenbank.

1. [Erstellen Sie Anmeldungen](../../t-sql/statements/create-login-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true) für die verwaltete Data Factory-Identität. Stellen Sie in SQL Server Management Studio (SSMS) über ein SQL Server-Konto (ein **Sysadmin**-Konto) eine Verbindung mit Ihrer verwalteten Instanz her. Führen Sie in der **Masterdatenbank** den folgenden T-SQL-Befehl aus:

    ```sql
    CREATE LOGIN [your data factory name] FROM EXTERNAL PROVIDER;
    ```

1. [Erstellen Sie für eine eigenständige Datenbank](/azure/sql-database/sql-database-aad-authentication-configure#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities) Benutzer für die verwaltete Data Factory-Identität. Stellen Sie eine Verbindung mit der Datenbank her, aus der bzw. in die Sie Daten kopieren möchten, und führen Sie den folgenden T-SQL-Befehl aus: 
  
    ```sql
    CREATE USER [your data factory name] FROM EXTERNAL PROVIDER;
    ```

1. Gewähren Sie der verwalteten Data Factory-Identität die notwendigen Berechtigungen, und gehen Sie dabei so vor wie für SQL-Benutzer und andere Benutzer. Führen Sie den folgenden Code aus. Weitere Optionen finden Sie in [diesem Dokument](../../t-sql/statements/alter-role-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true).

    ```sql
    ALTER ROLE [role name e.g., db_owner] ADD MEMBER [your data factory name];
    ```

Abschließend konfigurieren Sie die Authentifizierung der verwalteten Identität für den ADO.NET-Verbindungs-Manager. Es gibt dafür folgende Optionen:
    
- **Konfigurieren zur Entwurfszeit.** Klicken Sie im SSIS-Designer mit der rechten Maustaste auf den ADO.NET-Verbindungs-Manager, und wählen Sie **Eigenschaften** aus. Aktualisieren Sie die Eigenschaft `ConnectUsingManagedIdentity` auf `True`.
    > [!NOTE]
    >  Derzeit hat die Verbindungs-Manager-Eigenschaft `ConnectUsingManagedIdentity` keine Auswirkung (bedeutet, dass die Authentifizierung der verwalteten Identität nicht funktioniert) beim Ausführen des SSIS-Pakets im SSIS-Designer oder [!INCLUDE[msCoName](../../includes/msconame-md.md)] SQL Server.
    
- **Konfigurieren zur Runtime.** Suchen Sie beim Ausführen des Pakets über [SQL Server Management Studio (SSMS)](../ssis-quickstart-run-ssms.md) oder die [Aktivität „SSIS-Paket ausführen“ von Azure Data Factory](/azure/data-factory/how-to-invoke-ssis-package-ssis-activity) den ADO.NET-Verbindungs-Manager. Aktualisieren Sie dessen Eigenschaft `ConnectUsingManagedIdentity` auf `True`.
    > [!NOTE]
    >  In Azure-SSIS Integration Runtime werden alle anderen Authentifizierungsmethoden (z.B. integrierte Authentifizierung, Kennwort), die im ADO.NET-Verbindungs-Manager vorkonfiguriert sind, überschrieben, wenn die Authentifizierung der verwalteten Identität zum Herstellen einer Datenbankverbindung verwendet wird.

> [!NOTE]
>  Um die Authentifizierung von verwalteten Identitäten für vorhandene Pakete zu konfigurieren, besteht die bevorzugte Methode darin, das SSIS-Projekt mindestens einmal mit dem [neuesten SSIS-Designer](../../ssdt/download-sql-server-data-tools-ssdt.md) neu zu erstellen. Stellen Sie dieses SSIS-Projekt erneut in ihrer Azure-SSIS Integration Runtime bereit, sodass die neue Eigenschaft `ConnectUsingManagedIdentity` des Verbindungs-Managers automatisch allen ADO.NET-Verbindungs-Managern in Ihrem SSIS-Projekt hinzugefügt wird. Die alternative Methode besteht darin, eine Eigenschaftsüberschreibung direkt mit dem Eigenschaftenpfad **\Package.Connections[{Name Ihres Verbindungs-Managers}].Properties[ConnectUsingManagedIdentity]** zur Runtime zu verwenden.

## <a name="see-also"></a>Weitere Informationen  
 [Integration Services-Verbindungen &#40;SSIS&#41;](../../integration-services/connection-manager/integration-services-ssis-connections.md)  
  
