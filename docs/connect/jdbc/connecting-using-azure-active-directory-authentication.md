---
title: Herstellen einer Verbindung mithilfe der Azure Active Directory-Authentifizierung
description: Erfahren Sie, wie Sie Java-Anwendungen entwickeln können, die dieses Azure Active Directory-Authentifizierungsfeature mit dem Microsoft-JDBC-Treiber für SQL Server verwenden.
ms.custom: ''
ms.date: 01/04/2020
ms.reviewer: ''
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 9c9d97be-de1d-412f-901d-5d9860c3df8c
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 68d8b2a131fa6ab1c9e287f70cb584db3aeedacc
ms.sourcegitcommit: 6154ee7f20bccce9d458ac7f3b0a21b9613d1131
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/05/2021
ms.locfileid: "97902638"
---
# <a name="connecting-using-azure-active-directory-authentication"></a>Herstellen einer Verbindung mithilfe der Azure Active Directory-Authentifizierung

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

Dieser Artikel enthält Informationen zum Entwickeln von Java-Anwendungen, die das Azure Active Directory-Authentifizierungsfeature mit dem Microsoft-JDBC-Treiber für SQL Server verwenden.

Sie können die Azure Active Directory-Authentifizierung (Azure AD) verwenden. Hierbei handelt es sich um einen Mechanismus zum Herstellen einer Verbindung mit einer Azure SQL-Datenbank v12 unter Verwendung von Identitäten in Azure Active Directory. Verwenden Sie AAD-Authentifizierung zur zentralen Verwaltung von Identitäten von Datenbankbenutzern und als Alternative zur SQL Server-Authentifizierung. Mit dem JDBC-Treiber können Sie Ihre Azure Active Directory-Anmeldeinformationen in der JDBC-Verbindungszeichenfolge angeben, um eine Verbindung mit Azure SQL-Datenbank herzustellen. Informationen zum Konfigurieren der Azure Active Directory-Authentifizierung finden Sie unter [Herstellen einer Verbindung mit SQL-Datenbank unter Verwendung der Azure Active Directory-Authentifizierung](/azure/azure-sql/database/authentication-aad-overview). 

Folgende Verbindungseigenschaften im Microsoft-JDBC-Treiber für SQL Server unterstützen die Azure Active Directory-Authentifizierung:
*   **authentication**:  Verwenden Sie diese Eigenschaft, um anzugeben, welche SQL-Authentifizierungsmethode für die Verbindung verwendet werden soll. Mögliche Werte: 
    * **ActiveDirectoryMSI**
        * `authentication=ActiveDirectoryMSI` wird ab Treiberversion **v7.2** unterstützt und kann verwendet werden, um aus einer Azure-Ressource mit aktivierter Identitätsunterstützung heraus eine Verbindung mit einer Azure SQL-Datenbank- oder Data Warehouse-Instanz herzustellen. Optional kann in den Verbindungs-/DataSource-Eigenschafen zusätzlich zu diesem Authentifizierungsmodus auch **msiClientId** angegeben werden. Diese Eigenschaft muss die Client-ID einer verwalteten Identität enthalten, mit der das **accessToken** zum Herstellen der Verbindung abgerufen wird.
    * **ActiveDirectoryIntegrated**
        * `authentication=ActiveDirectoryIntegrated` wird ab Treiberversion **v6.0** unterstützt und kann verwendet werden, um mithilfe der integrierten Authentifizierung eine Verbindung mit einer Azure SQL-Datenbank- oder Data Warehouse-Instanz herzustellen. Um diesen Authentifizierungsmodus zu verwenden, müssen Sie einen Verbund zwischen den lokalen Active Directory-Verbunddiensten (AD FS) und Azure Active Directory in der Cloud einrichten. Sobald dieser Verbund eingerichtet ist, können Sie eine Verbindung herstellen, indem Sie entweder die native Bibliothek „mssql-jdbc_auth-\<version>-\<arch>.dll“ zum Anwendungsklassenpfad im Windows-Betriebssystem hinzufügen oder ein Kerberos-Ticket einrichten, um eine plattformübergreifende Authentifizierung zu unterstützen. Wenn Sie bei einem in die Domäne eingebundenen Computer angemeldet sind, können Sie auf Azure SQL-Datenbank bzw. Azure Synapse Analytics zugreifen, ohne zur Eingabe von Anmeldeinformationen aufgefordert zu werden.
    * **ActiveDirectoryPassword**
        * `authentication=ActiveDirectoryPassword` wird ab Treiberversion **v6.0** unterstützt und kann verwendet werden, um mithilfe eines Azure AD-Benutzernamens und des zugehörigen Kennworts eine Verbindung mit einer Azure SQL-Datenbank- oder Data Warehouse-Instanz herzustellen.
    * **SqlPassword**
        * Verwenden Sie `authentication=SqlPassword`, um unter Verwendung der Eigenschaften „userName/user“ und „password“ eine Verbindung mit einer SQL Server-Instanz herzustellen.
    * **NotSpecified**
        * Verwenden Sie `authentication=NotSpecified`, oder behalten Sie den Standardwert bei, wenn keine dieser Authentifizierungsmethoden benötigt wird.

*   **accessToken**: Verwenden Sie diese Verbindungseigenschaft, um mithilfe eines Zugriffstokens eine Verbindung mit einer SQL-Datenbank herzustellen. accessToken kann nur über den Properties-Parameter der getConnection()-Methode in der DriverManager-Klasse festgelegt werden. Eine Verwendung in der Verbindungs-URL ist nicht möglich.  

Weitere Informationen finden Sie in der Beschreibung der Authentifizierungseigenschaft auf der Seite [Festlegen von Verbindungseigenschaften](setting-the-connection-properties.md).  


## <a name="client-setup-requirements"></a>Anforderungen an das Clientsetup
Für die **ActiveDirectoryMSI**-Authentifizierung müssen die folgenden Komponenten auf dem Clientcomputer installiert sein:
* Java 8 oder höher
* Microsoft-JDBC-Treiber 7.2 (oder höher) für SQL Server
* Bei der Clientumgebung muss es sich um eine Azure-Ressource handeln, und die Unterstützung für das Identitätsfeature muss aktiviert sein.
* Ein Benutzer einer eigenständigen Datenbank, der die systemseitig oder benutzerseitig zugewiesene verwaltete Identität der Azure-Ressource oder eine der Gruppen darstellt, denen Ihre verwaltete Identität angehört, muss in der Zieldatenbank vorhanden sein und über die Berechtigung CONNECT verfügen.

Für andere Authentifizierungsmodi müssen die folgenden Komponenten auf dem Clientcomputer installiert sein:
* Java 7 oder höher
* Microsoft-JDBC-Treiber 6.0 (oder höher) für SQL Server
* Wenn Sie den Authentifizierungsmodus mit Zugriffstoken verwenden, benötigen Sie die Bibliothek [azure-activedirectory-library-for-java](https://github.com/AzureAD/azure-activedirectory-library-for-java) und die zugehörigen Abhängigkeiten, um die Beispiele in diesem Artikel auszuführen. Weitere Informationen finden Sie im Abschnitt **Herstellen einer Verbindung über ein Zugriffstoken**.
* Wenn Sie den Authentifizierungsmodus **ActiveDirectoryPassword** verwenden, benötigen Sie die Bibliothek [azure-activedirectory-library-for-java](https://github.com/AzureAD/azure-activedirectory-library-for-java) und die zugehörigen Abhängigkeiten. Weitere Informationen finden Sie im Abschnitt **Herstellen einer Verbindung im ActiveDirectoryPassword-Authentifizierungsmodus**.
* Wenn Sie den Authentifizierungsmodus **ActiveDirectoryIntegrated** verwenden, benötigen Sie die Bibliothek „azure-activedirectory-library-for-java“ und die zugehörigen Abhängigkeiten. Weitere Informationen finden Sie im Abschnitt **Herstellen einer Verbindung im ActiveDirectoryIntegrated-Authentifizierungsmodus**.

## <a name="connecting-using-activedirectorymsi-authentication-mode"></a>Herstellen einer Verbindung im ActiveDirectoryMSI-Authentifizierungsmodus
Im folgenden Beispiel wird veranschaulicht, wie der Modus `authentication=ActiveDirectoryMSI` verwendet wird. Führen Sie dieses Beispiel aus einer Azure-Ressource heraus aus, z. B. einer Azure-VM, einer App Service- oder einer Funktions-App, die sich in einem Verbund mit Azure Active Directory befindet.

Ersetzen Sie die Server- und Datenbanknamen in den folgenden Zeilen durch die Namen Ihres Servers und Ihrer Datenbank, bevor Sie das Beispiel ausführen:

```java
ds.setServerName("aad-managed-demo.database.windows.net"); // replace 'aad-managed-demo' with your server name
ds.setDatabaseName("demo"); // replace with your database name
//Optional
ds.setMSIClientId("94de34e9-8e8c-470a-96df-08110924b814"); // Replace with Client ID of User-Assigned Managed Identity to be used
```

Beispiel zur Verwendung des ActiveDirectoryMSI-Authentifizierungsmodus:

```java
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.Statement;

import com.microsoft.sqlserver.jdbc.SQLServerDataSource;

public class AAD_MSI {
    public static void main(String[] args) throws Exception {

        SQLServerDataSource ds = new SQLServerDataSource();
        ds.setServerName("aad-managed-demo.database.windows.net"); // Replace with your server name
        ds.setDatabaseName("demo"); // Replace with your database name
        ds.setAuthentication("ActiveDirectoryMSI");
        // Optional
        ds.setMSIClientId("94de34e9-8e8c-470a-96df-08110924b814"); // Replace with Client ID of User-Assigned Managed Identity to be used

        try (Connection connection = ds.getConnection(); 
                Statement stmt = connection.createStatement();
                ResultSet rs = stmt.executeQuery("SELECT SUSER_SNAME()")) {
            if (rs.next()) {
                System.out.println("You have successfully logged on as: " + rs.getString(1));
            }
        }
    }
}
```

Die Ausführung dieses Beispiels auf einer Azure-VM ruft ein Zugriffstoken aus einer _systemseitig zugewiesenen verwalteten Identität_ oder einer _benutzerseitig zugewiesenen verwalteten Identität_ (wenn **msiClientId** angegeben ist) ab und stellt mithilfe dieses Tokens eine Verbindung her. Wenn eine Verbindung hergestellt wurde, sollte die folgende Meldung angezeigt werden:

```bash
You have successfully logged on as: <your Managed Identity username>
```

## <a name="connecting-using-activedirectoryintegrated-authentication-mode"></a>Herstellen einer Verbindung im ActiveDirectoryIntegrated-Authentifizierungsmodus
Ab Version 6.4. fügt der Microsoft-JDBC-Treiber Unterstützung für die ActiveDirectoryIntegrated-Authentifizierung mithilfe eines Kerberos-Tickets auf verschiedenen Plattformen hinzu (Windows, Linux und macOS).
Weitere Informationen finden Sie unter [Einrichten eines Kerberos-Tickets unter Windows, Linux und macOS](#set-kerberos-ticket-on-windows-linux-and-macos). Alternativ kann unter Windows auch „mssql-jdbc_auth-\<version>-\<arch>.dll“ für die ActiveDirectoryIntegrated-Authentifizierung mit dem JDBC-Treiber verwendet werden.

> [!NOTE]
>  Wenn Sie eine ältere Version des Treibers verwenden, finden Sie unter diesem [Link](feature-dependencies-of-microsoft-jdbc-driver-for-sql-server.md) Informationen zu den jeweiligen Abhängigkeiten, die zur Verwendung dieses Authentifizierungsmodus erforderlich sind. 

Im folgenden Beispiel wird veranschaulicht, wie der Modus `authentication=ActiveDirectoryIntegrated` verwendet wird. Führen Sie dieses Beispiel auf einem Computer aus, der in die Domäne eingebunden ist und sich in einem Verbund mit Azure Active Directory befindet. Eine eigenständige Datenbank, die Ihren Azure AD-Benutzer oder eine der Gruppen repräsentiert, denen Sie angehören, muss in der Datenbank vorhanden sein und über die Berechtigung CONNECT verfügen. 

Bevor Sie das Beispiel erstellen und ausführen, laden Sie auf dem Clientcomputer, auf dem Sie das Beispiel ausführen möchten, die Bibliothek [azure-activedirectory-library-for-java](https://github.com/AzureAD/azure-activedirectory-library-for-java) und die zugehörigen Abhängigkeiten herunter, und schließen Sie diese in den Java-Buildpfad ein.

Ersetzen Sie die Server- und Datenbanknamen in den folgenden Zeilen durch die Namen Ihres Servers und Ihrer Datenbank, bevor Sie das Beispiel ausführen:

```java
ds.setServerName("aad-managed-demo.database.windows.net"); // replace 'aad-managed-demo' with your server name
ds.setDatabaseName("demo"); // replace with your database name
```

Beispiel zur Verwendung des ActiveDirectoryIntegrated-Authentifizierungsmodus:
```java
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.Statement;

import com.microsoft.sqlserver.jdbc.SQLServerDataSource;

public class AADIntegrated {
    public static void main(String[] args) throws Exception {

        SQLServerDataSource ds = new SQLServerDataSource();
        ds.setServerName("aad-managed-demo.database.windows.net"); // Replace with your server name
        ds.setDatabaseName("demo"); // Replace with your database name
        ds.setAuthentication("ActiveDirectoryIntegrated");

        try (Connection connection = ds.getConnection(); 
                Statement stmt = connection.createStatement();
                ResultSet rs = stmt.executeQuery("SELECT SUSER_SNAME()")) {
            if (rs.next()) {
                System.out.println("You have successfully logged on as: " + rs.getString(1));
            }
        }
    }
}
```

Bei Ausführung dieses Beispiels auf einem Clientcomputer wird automatisch Ihr Kerberos-Ticket verwendet; ein Kennwort ist nicht erforderlich. Wenn eine Verbindung hergestellt wurde, sollte die folgende Meldung angezeigt werden:

```
You have successfully logged on as: <your domain user name>
```

### <a name="set-kerberos-ticket-on-windows-linux-and-macos"></a>Einrichten eines Kerberos-Tickets unter Windows, Linux und macOS

Sie müssen ein Kerberos-Ticket einrichten, das den aktuellen Benutzer mit einem Windows-Domänenkonto verknüpft. Im Folgenden finden Sie die wichtigsten Schritte.

#### <a name="windows"></a>Windows
Das JDK enthält `kinit` – hiermit können Sie auf einem in die Domäne eingebundenen Computer, der sich in einem Verbund mit Azure Active Directory befindet, ein TGT (Ticket-Granting Ticket) aus dem KDC (Key Distribution Center) abrufen.

##### <a name="step-1-ticket-granting-ticket-retrieval"></a>Schritt 1: Abruf des TGT
- **Ausführung**: Windows
- **Aktion:**
  - Verwenden Sie den Befehl `kinit username@DOMAIN.COMPANY.COM`, um ein TGT aus dem KDC abzurufen. Sie werden zur Eingabe Ihres Domänenkennworts aufgefordert.
  - Verwenden Sie `klist`, um die verfügbaren Tickets anzuzeigen. Wenn der kinit-Befehl erfolgreich war, sollte ein Ticket von krbtgt/DOMAIN.COMPANY.COM@ DOMAIN.COMPANY.COM angezeigt werden.

> [!NOTE]
>  Möglicherweise müssen Sie eine `.ini`-Datei mit `-Djava.security.krb5.conf` angeben, damit Ihre Anwendung das KDC findet.

#### <a name="linux-and-macos"></a>Linux und macOS

##### <a name="requirements"></a>Requirements (Anforderungen)
Zugriff auf einen in die Windows-Domäne eingebundenen Computer, um den Kerberos-Domänencontroller abzufragen.

##### <a name="step-1-find-kerberos-kdc"></a>Schritt 1: Suchen des Kerberos-KDC
- **Ausführung**: Windows-Befehlszeile
- **Aktion**: `nltest /dsgetdc:DOMAIN.COMPANY.COM` (wobei „DOMAIN.COMPANY.COM“ dem Namen Ihrer Domäne entspricht)
- **Beispielausgabe**
  ```
  DC: \\co1-red-dc-33.domain.company.com
  Address: \\2111:4444:2111:33:1111:ecff:ffff:3333
  ...
  The command completed successfully
  ```
- **Zu extrahierende Informationen**: Der Name des Domänencontrollers, in diesem Fall `co1-red-dc-33.domain.company.com`

##### <a name="step-2-configuring-kdc-in-krb5conf"></a>Schritt 2: Konfigurieren des KDC in „krb5.conf“
- **Ausführung**: Linux/macOS
- **Aktion:** Bearbeiten Sie „/etc/krb5.con“ in einem Editor Ihrer Wahl. Konfigurieren Sie die folgenden Schlüssel
  ```
  [libdefaults]
    default_realm = DOMAIN.COMPANY.COM
   
  [realms]
  DOMAIN.COMPANY.COM = {
     kdc = co1-red-dc-28.domain.company.com
  }
  ```
  Speichern Sie dann die Datei „krb5.conf“, und beenden Sie den Editor.

> [!NOTE]
>  Die Domäne muss VOLLSTÄNDIG IN GROSSBUCHSTABEN angegeben werden.

##### <a name="step-3-testing-the-ticket-granting-ticket-retrieval"></a>Schritt 3: Testen des TGT-Abrufs
- **Ausführung**: Linux/macOS
- **Aktion:**
  - Verwenden Sie den Befehl `kinit username@DOMAIN.COMPANY.COM`, um ein TGT aus dem KDC abzurufen. Sie werden zur Eingabe Ihres Domänenkennworts aufgefordert.
  - Verwenden Sie `klist`, um die verfügbaren Tickets anzuzeigen. Wenn der kinit-Befehl erfolgreich war, sollte ein Ticket von krbtgt/DOMAIN.COMPANY.COM@ DOMAIN.COMPANY.COM angezeigt werden.

## <a name="connecting-using-activedirectorypassword-authentication-mode"></a>Herstellen einer Verbindung im ActiveDirectoryPassword-Authentifizierungsmodus
Im folgenden Beispiel wird veranschaulicht, wie der Modus `authentication=ActiveDirectoryPassword` verwendet wird.

Gehen Sie vor dem Erstellen und Ausführen des Beispiels folgendermaßen vor:
1.  Laden Sie auf dem Clientcomputer, auf dem Sie das Beispiel ausführen möchten, die Bibliothek [azure-activedirectory-library-for-java](https://github.com/AzureAD/azure-activedirectory-library-for-java) und die zugehörigen Abhängigkeiten herunter, und schließen Sie diese in den Java-Buildpfad ein.
2.  Suchen Sie die folgenden Codezeilen, und ersetzen Sie die Server- und Datenbanknamen durch die Namen Ihres Servers und Ihrer Datenbank.
    ```java
    ds.setServerName("aad-managed-demo.database.windows.net"); // replace 'aad-managed-demo' with your server name
    ds.setDatabaseName("demo"); // replace with your database name
    ```
3.  Suchen Sie die folgenden Codezeilen, und ersetzen Sie den Benutzernamen durch den Namen des Azure AD-Benutzers, mit dessen Identität Sie die Verbindung herstellen möchten.
    ```java
    ds.setUser("bob@cqclinic.onmicrosoft.com"); // replace with your user name
    ds.setPassword("password");     // replace with your password
    ```

Beispiel zur Verwendung des ActiveDirectoryPassword-Authentifizierungsmodus:
```java
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.Statement;

import com.microsoft.sqlserver.jdbc.SQLServerDataSource;

public class AADUserPassword {
    
    public static void main(String[] args) throws Exception{
        
        SQLServerDataSource ds = new SQLServerDataSource();
        ds.setServerName("aad-managed-demo.database.windows.net"); // Replace with your server name
        ds.setDatabaseName("demo"); // Replace with your database
        ds.setUser("bob@cqclinic.onmicrosoft.com"); // Replace with your user name
        ds.setPassword("password"); // Replace with your password
        ds.setAuthentication("ActiveDirectoryPassword");
        
        try (Connection connection = ds.getConnection(); 
                Statement stmt = connection.createStatement();
                ResultSet rs = stmt.executeQuery("SELECT SUSER_SNAME()")) {
            if (rs.next()) {
                System.out.println("You have successfully logged on as: " + rs.getString(1));
            }
        }
    }
}
```
Wenn eine Verbindung hergestellt wurde, sollte die folgende Meldung als Ausgabe angezeigt werden:
```
You have successfully logged on as: <your user name>
```

> [!NOTE]  
> Es muss eine eigenständige Benutzerdatenbank vorhanden sein, und in dieser Datenbank muss ein Benutzer vorhanden sein, der den angegebenen Azure AD-Benutzer oder eine der Gruppen repräsentiert, zu denen der angegebene Azure AD-Benutzer gehört. Diese Datenbank muss über die Berechtigung CONNECT verfügen (außer, es handelt sich um den Azure Active Directory-Serveradministrator oder eine Azure Active Directory-Gruppe).

## <a name="connecting-using-access-token"></a>Herstellen einer Verbindung über ein Zugriffstoken
Anwendungen und Dienste können ein Zugriffstoken aus Azure Active Directory abrufen und mit diesem eine Verbindung mit einer Azure SQL-Datenbank- oder Data Warehouse-Instanz herstellen.

> [!NOTE] 
> **accessToken** kann nur über den Properties-Parameter der getConnection()-Methode in der DriverManager-Klasse festgelegt werden. Eine Verwendung in der Verbindungszeichenfolge ist nicht möglich.

Das folgende Beispiel enthält eine einfache Java-Anwendung, die mithilfe der zugriffstokenbasierten Authentifizierung eine Verbindung mit Azure SQL-Datenbank/Data Warehouse herstellt. Bevor Sie das Beispiel erstellen und ausführen, führen Sie die folgenden Schritte aus:
1.  Erstellen Sie in Azure Active Directory ein Anwendungskonto für Ihren Dienst.
    1. Melden Sie sich beim Azure-Portal an.
    2. Klicken Sie im linken Navigationsbereich auf „Azure Active Directory“.
    3. Klicken Sie auf die Registerkarte „App-Registrierungen“.
    4. Klicken Sie im Drawer auf „Registrierung einer neuen Anwendung“.
    5. Geben Sie „mytokentest“ als Anzeigenamen für die Anwendung ein, und wählen Sie „Web-App/API“ aus.
    6. Die Angabe einer Anmelde-URL ist nicht erforderlich. Geben Sie einfach irgendetwas ein: https://mytokentest.
    7. Klicken Sie unten auf „Erstellen“.
    9. Klicken Sie im Azure-Portal auf die Registerkarte „Einstellungen“ Ihrer Anwendung, und öffnen Sie die Registerkarte „Eigenschaften“.
    10. Suchen Sie nach dem Wert der Anwendungs-ID (Client-ID), und kopieren Sie ihn. Sie benötigen den ID-Wert später beim Konfigurieren Ihrer Anwendung (Beispiel: 1846943b-ad04-4808-aa13-4702d908b5c1). Siehe folgenden Screenshot.
    11. Erstellen Sie im Abschnitt „Schlüssel“ einen Schlüssel, indem Sie das Namensfeld ausfüllen, die Dauer des Schlüssels auswählen und die Konfiguration speichern (lassen Sie das Wertfeld leer). Nach dem Speichern ist das Wertfeld automatisch ausgefüllt. Kopieren Sie den generierten Wert. Dies ist der geheime Clientschlüssel.
    12. Klicken Sie im linken Navigationsbereich auf „Azure Active Directory“. Suchen Sie die Registerkarte „Endpunkte“ unter „App-Registrierungen“. Kopieren Sie die URL unter „OATH 2.0-TOKENENDPUNKT“ – dies ist Ihre STS-URL.
    
    ![Endpunkt für App-Registrierungen im Azure-Portal – STS-URL](media/jdbc_aad_token.png)  
2. Melden Sie sich als Azure Active Directory-Administrator bei Ihrer Azure-SQL Server-Benutzerdatenbank an, und geben Sie mithilfe eines T-SQL-Befehls einen Benutzer der eigenständigen Datenbank als Anwendungsprinzipal an. Weitere Informationen zum Erstellen eines Azure Active Directory-Administrators und eines Benutzers einer eigenständigen Datenbank finden Sie unter [Herstellen einer Verbindung mit SQL-Datenbank oder Azure Synapse Analytics unter Verwendung der Azure Active Directory-Authentifizierung](/azure/azure-sql/database/authentication-aad-overview).

    ```
    CREATE USER [mytokentest] FROM EXTERNAL PROVIDER
    ```

3.  Laden Sie auf dem Clientcomputer, auf dem Sie das Beispiel ausführen möchten, die Bibliothek [azure-activedirectory-library-for-java](https://github.com/AzureAD/azure-activedirectory-library-for-java) und die zugehörigen Abhängigkeiten herunter, und schließen Sie diese in den Java-Buildpfad ein. Beachten Sie, dass die Bibliothek „azure-activedirectory-library-for-java“ nur zur Ausführung dieses speziellen Beispiels benötigt wird. Im Beispiel werden die APIs aus dieser Bibliothek verwendet, um das Zugriffstoken aus Azure AD abzurufen. Wenn Sie bereits über ein Zugriffstoken verfügen, können Sie diesen Schritt überspringen. Beachten Sie, dass Sie dann auch den Abschnitt aus dem Beispiel entfernen müssen, der ein Zugriffstoken abruft.

Ersetzen Sie im folgenden Beispiel die Werte für die STS-URL, die Client-ID, das Clientgeheimnis, sowie den Server und den Datenbanknamen durch Ihre eigenen Werte.

```java
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.Statement;
import java.util.concurrent.Executors;
import java.util.concurrent.Future;
import com.microsoft.sqlserver.jdbc.SQLServerDataSource;

// The azure-activedirectory-library-for-java is needed to retrieve the access token from the AD.
import com.microsoft.aad.adal4j.AuthenticationContext;
import com.microsoft.aad.adal4j.AuthenticationResult;
import com.microsoft.aad.adal4j.ClientCredential;

public class AADTokenBased {

    public static void main(String[] args) throws Exception {

        // Retrieve the access token from the AD.
        String spn = "https://database.windows.net/";
        String stsurl = "https://login.microsoftonline.com/..."; // Replace with your STS URL.
        String clientId = "1846943b-ad04-4808-aa13-4702d908b5c1"; // Replace with your client ID.
        String clientSecret = "..."; // Replace with your client secret.

        AuthenticationContext context = new AuthenticationContext(stsurl, false, Executors.newFixedThreadPool(1));
        ClientCredential cred = new ClientCredential(clientId, clientSecret);

        Future<AuthenticationResult> future = context.acquireToken(spn, cred, null);
        String accessToken = future.get().getAccessToken();

        System.out.println("Access Token: " + accessToken);

        // Connect with the access token.
        SQLServerDataSource ds = new SQLServerDataSource();

        ds.setServerName("aad-managed-demo.database.windows.net"); // Replace with your server name.
        ds.setDatabaseName("demo"); // Replace with your database name.
        ds.setAccessToken(accessToken);

        try (Connection connection = ds.getConnection(); 
                Statement stmt = connection.createStatement();
                ResultSet rs = stmt.executeQuery("SELECT SUSER_SNAME()")) {
            if (rs.next()) {
                System.out.println("You have successfully logged on as: " + rs.getString(1));
            }
        }
    }
}
``` 

Wenn die Verbindung erfolgreich hergestellt wurde, sollte die folgende Meldung als Ausgabe angezeigt werden:

```bash
Access Token: <your access token>
You have successfully logged on as: <your client ID>    
```
