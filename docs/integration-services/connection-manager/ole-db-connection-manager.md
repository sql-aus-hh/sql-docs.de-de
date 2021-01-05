---
title: OLE DB-Verbindungs-Manager | Microsoft-Dokumentation
description: Durch einen OLE DB-Verbindungs-Manager kann ein Paket mithilfe eines OLE DB-Anbieters eine Verbindung mit einer Datenquelle herstellen.
ms.custom: ''
ms.date: 05/24/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.oledbconnection.f1
helpviewer_keywords:
- OLE DB connection manager
- data sources [Integration Services], connections
- connection managers [Integration Services], OLE DB
- connections [Integration Services], OLE DB
ms.assetid: 91e3622e-4b1a-439a-80c7-a00b90d66979
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 7c50a2e201bcba99299998dc232e1246005eb168
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/17/2020
ms.locfileid: "97639316"
---
# <a name="ole-db-connection-manager"></a>Teilcache

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


Durch einen OLE DB-Verbindungs-Manager kann ein Paket mithilfe eines OLE DB-Anbieters eine Verbindung mit einer Datenquelle herstellen. Beispielsweise kann ein OLE DB-Verbindungs-Manager, der eine Verbindung mit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] herstellt, den [!INCLUDE[msCoName](../../includes/msconame-md.md)] OLE DB-Anbieter für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]verwenden.    
    
> [!NOTE]
>  Der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 11.0 OLEDB-Anbieter unterstützt die neuen Schlüsselwörter für Verbindungszeichenfolgen (MultiSubnetFailover=True) für Multisubnetz-Failoverclustering nicht. Weitere Informationen finden Sie unter [Versionshinweise zu SQL Server](https://go.microsoft.com/fwlink/?LinkId=247824).    
> 
> [!NOTE]
>  Wenn es sich bei der Datenquelle um [!INCLUDE[msCoName](../../includes/msconame-md.md)] Office Excel 2007 oder [!INCLUDE[msCoName](../../includes/msconame-md.md)] Office Access 2007 handelt, erfordert die Datenquelle einen anderen Datenanbieter als frühere Versionen von Excel oder Access. Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit einer Excel-Arbeitsmappe](../load-data-to-from-excel-with-ssis.md) und [Herstellen einer Verbindung mit einer Access-Datenbank](./integration-services-ssis-connections.md).    
    
Mehrere Tasks und Datenflusskomponenten von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] verwenden einen OLE DB-Verbindungs-Manager. Beispielsweise verwenden die OLE DB-Quelle und das OLE DB-Ziel diesen Verbindungs-Manager zum Extrahieren und Laden von Daten. Ein Paket mit einem Task „SQL ausführen“ kann diesen Verbindungs-Manager zum Herstellen einer Verbindung mit einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Datenbank zum Ausführen von Abfragen verwenden.    
    
Sie können den OLE DB-Verbindungs-Manager auch für den Zugriff auf OLE DB-Datenquellen in benutzerdefinierten Tasks verwendet, die in nicht verwaltetem Code in einer Programmiersprache wie z. B. C++ geschrieben sind.    
    
Wenn Sie einem Paket einen OLE DB-Verbindungs-Manager hinzufügen, erstellt [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] einen Verbindungs-Manager, der zur Runtime in eine OLE DB-Verbindung aufgelöst wird, die Eigenschaften des Verbindungs-Managers festlegt und den Verbindungs-Manager der Sammlung **Verbindungen** im Paket hinzufügt.    
    
Die `ConnectionManagerType`-Eigenschaft des Verbindungs-Managers ist auf `OLEDB` festgelegt.    
    
Sie haben folgende Möglichkeiten, um den OLE DB-Verbindungs-Manager zu konfigurieren:    
    
-   Stellen Sie eine Verbindungszeichenfolge bereit, die die Anforderungen des ausgewählten Anbieters erfüllt.    
    
-   Schließen Sie in Abhängigkeit vom Anbieter den Namen der Datenquelle ein, mit der eine Verbindung hergestellt werden soll.    
    
-   Stellen Sie entsprechende Sicherheitsanmeldeinformationen für den ausgewählten Anbieter bereit.    
    
-   Geben Sie an, ob die im Verbindungs-Manager erstellte Verbindung zur Runtime beibehalten wird.    
    
## <a name="log-calls-and-troubleshoot-connections"></a>Protokollieren von Aufrufen und Behandeln von Verbindungsproblemen    
 Sie können die vom OLE DB-Verbindungs-Manager an externe Datenanbieter gerichteten Aufrufe protokollieren. Anschließend können Sie Probleme bei Verbindungen behandeln, die vom OLE DB-Verbindungs-Manager mit externen Datenquellen hergestellt werden. Aktivieren Sie zum Protokollieren der vom OLE DB-Verbindungs-Manager an externe Datenanbieter gerichteten Aufrufe die Paketprotokollierung, und wählen Sie das **Diagnostic**-Ereignis auf Paketebene aus. Weitere Informationen finden Sie unter [Behandeln von Problemen mit Paketausführungstools](../../integration-services/troubleshooting/troubleshooting-tools-for-package-execution.md).    
    
## <a name="configure-the-ole-db-connection-manager"></a>Konfigurieren des OLE DB-Verbindungs-Managers    
 Sie können Eigenschaften mit dem [!INCLUDE[ssIS](../../includes/ssis-md.md)]-Designer oder programmgesteuert festlegen. Weitere Informationen zu den Eigenschaften, die Sie im [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Designer festlegen können, finden Sie unter [Konfigurieren des OLE DB-Verbindungs-Managers](). Informationen zum programmgesteuerten Konfigurieren eines Verbindungs-Managers finden Sie in der Dokumentation zur **T:Microsoft.SqlServer.Dts.Runtime.ConnectionManager** -Klasse im Entwicklerhandbuch.    
    
### <a name="configure-ole-db-connection-manager"></a>Konfigurieren des OLE DB-Verbindungs-Managers

Über das Dialogfeld **OLE DB-Verbindungs-Manager konfigurieren** können Sie eine Verbindung zu einer Datenquelle hinzufügen. Dies kann eine neue Verbindung oder eine Kopie einer vorhandenen Verbindung sein.  
  
> [!NOTE]  
>  Wenn es sich bei der Datenquelle um [!INCLUDE[msCoName](../../includes/msconame-md.md)] Office Excel 2007 handelt, erfordert die Datenquelle einen anderen Verbindungs-Manager als frühere Versionen von Excel. Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit einer Excel-Arbeitsmappe](../load-data-to-from-excel-with-ssis.md).  
>   
>  Wenn es sich bei der Datenquelle um [!INCLUDE[msCoName](../../includes/msconame-md.md)] Office Access 2007 handelt, erfordert die Datenquelle einen anderen OLE DB-Anbieter als frühere Versionen von Access. Weitere Informationen finden Sie unter [Herstellen einer Verbindung zu einer Access-Datenbank](./integration-services-ssis-connections.md).  
  
 Weitere Informationen zum OLE DB-Verbindungs-Manager finden Sie unter [OLE DB Connection Manager](../../integration-services/connection-manager/ole-db-connection-manager.md).  
  
#### <a name="options"></a>Tastatur  
 **Datenverbindungen**  
 Wählen Sie aus der Liste eine vorhandene OLE DB-Datenverbindung aus.  
  
 **Datenverbindungseigenschaften**  
 Zeigt die Eigenschaften und Werte der ausgewählten OLE DB-Datenverbindung an.  
  
 **Neu**  
 Erstellen Sie mithilfe des Dialogfelds **Verbindungs-Manager** eine OLE DB-Datenverbindung.  
  
 **Löschen**  
 Wählen Sie eine Datenverbindung aus, und löschen Sie sie mithilfe von **Löschen**.  
  
#### <a name="managed-identities-for-azure-resources-authentication"></a>Verwaltete Identitäten für die Authentifizierung von Azure-Ressourcen
Beim Ausführen von SSIS-Paketen auf der [Azure-SSIS Integration Runtime in Azure Data Factory](/azure/data-factory/concepts-integration-runtime#azure-ssis-integration-runtime) verwenden Sie die [verwaltete Identität](/azure/data-factory/connector-azure-sql-database#managed-identity), die der Data Factory für die Authentifizierung mit Azure SQL-Datenbank oder Managed Instance zugeordnet ist. Die angegebene Factory kann mithilfe dieser Identität auf Daten zugreifen und Daten aus der oder in die Datenbank kopieren.

> [!NOTE]
>  Wenn Sie die Azure Active Directory-Authentifizierung (einschließlich der Authentifizierung der verwalteten Identität) zum Herstellen einer Verbindung mit der Azure SQL-Datenbank-Instanz (oder Managed Instance) verwenden, kann es zu einem Problem bei der Paketausführung oder zu unerwarteten Behavior Changes kommen. Weitere Informationen finden Sie unter [Features und Einschränkungen von Azure AD](/azure/sql-database/sql-database-aad-authentication#azure-ad-features-and-limitations).

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

Dann konfigurieren Sie den OLE DB-Anbieter für den OLE DB-Verbindungs-Manager. Es gibt dafür folgende Optionen:
    
- **Konfigurieren zur Entwurfszeit.** Doppelklicken Sie im SSIS-Designer auf den OLE DB-Verbindungs-Manager, um das Fenster **Verbindungs-Manager** zu öffnen. Wählen Sie in der Dropdownliste **Anbieter** die Option [**Microsoft OLE DB-Treiber für SQL Server**](https://go.microsoft.com/fwlink/?linkid=871294) aus.
    > [!NOTE]
    >  Andere Anbieter in der Dropdownliste bieten möglicherweise keine Unterstützung für die Authentifizierung der verwalteten Identität.
    
- **Konfigurieren zur Runtime.** Suchen Sie beim Ausführen des Pakets über [SQL Server Management Studio (SSMS)](../ssis-quickstart-run-ssms.md) oder die [Aktivität „SSIS-Paket ausführen“ von Azure Data Factory](/azure/data-factory/how-to-invoke-ssis-package-ssis-activity) die Verbindungs-Manager-Eigenschaft **ConnectionString** für den OLE DB-Verbindungs-Manager. Aktualisieren Sie die Verbindungseigenschaft `Provider` auf `MSOLEDBSQL` (d. h. OLE DB-Treiber für SQL Server).
    ```vb
    Data Source=serverName;Initial Catalog=databaseName;Provider=MSOLEDBSQL;...
    ```

Abschließend konfigurieren Sie die Authentifizierung der verwalteten Identität für den OLE DB-Verbindungs-Manager. Es gibt dafür folgende Optionen:
    
- **Konfigurieren zur Entwurfszeit.** Klicken Sie im SSIS-Designer mit der rechten Maustaste auf den OLE DB-Verbindungs-Manager, und wählen Sie **Eigenschaften** aus. Aktualisieren Sie die Eigenschaft `ConnectUsingManagedIdentity` auf `True`.
    > [!NOTE]
    >  Derzeit hat die Verbindungs-Manager-Eigenschaft `ConnectUsingManagedIdentity` keine Auswirkung (bedeutet, dass die Authentifizierung der verwalteten Identität nicht funktioniert) beim Ausführen des SSIS-Pakets im SSIS-Designer oder [!INCLUDE[msCoName](../../includes/msconame-md.md)] SQL Server.

- **Konfigurieren zur Runtime.** Suchen Sie beim Ausführen des Pakets über SSMS oder die Aktivität **SQL-Paket ausführen** den OLE DB-Verbindungs-Manager, und aktualisieren Sie dessen Eigenschaft `ConnectUsingManagedIdentity` auf `True`.
    > [!NOTE]
    >  In Azure-SSIS Integration Runtime werden alle anderen Authentifizierungsmethoden (z.B. integrierte Sicherheit, Kennwort), die im OLE DB-Verbindungs-Manager vorkonfiguriert sind, überschrieben, wenn die Authentifizierung der verwalteten Identität zum Herstellen einer Datenbankverbindung verwendet wird.

> [!NOTE]
>  Um die Authentifizierung von verwalteten Identitäten für vorhandene Pakete zu konfigurieren, besteht die bevorzugte Methode darin, das SSIS-Projekt mindestens einmal mit dem [neuesten SSIS-Designer](../../ssdt/download-sql-server-data-tools-ssdt.md) neu zu erstellen. Stellen Sie dieses SSIS-Projekt erneut in ihrer Azure-SSIS Integration Runtime bereit, sodass die neue Eigenschaft `ConnectUsingManagedIdentity` des Verbindungs-Managers automatisch allen OLE DB-Verbindungs-Managern in Ihrem SSIS-Projekt hinzugefügt wird. Die alternative Methode besteht darin, eine Eigenschaftsüberschreibung direkt mit dem Eigenschaftenpfad **\Package.Connections[{Name Ihres Verbindungs-Managers}].Properties[ConnectUsingManagedIdentity]** zur Runtime zu verwenden.

## <a name="see-also"></a>Weitere Informationen    
 [OLE DB-Quelle](../../integration-services/data-flow/ole-db-source.md)     
 [OLE DB-Ziel](../../integration-services/data-flow/ole-db-destination.md)     
 [SQL ausführen (Task)](../../integration-services/control-flow/execute-sql-task.md)     
 [Integration Services-Verbindungen &#40;SSIS&#41;](../../integration-services/connection-manager/integration-services-ssis-connections.md)    
    
