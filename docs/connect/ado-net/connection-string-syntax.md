---
title: Syntax von Verbindungszeichenfolgen
description: Erfahren Sie mehr über die Syntax von Verbindungszeichenfolgen im Microsoft SqlClient-Datenanbieter für SQL Server. Die Syntax für jeden Anbieter ist in dessen ConnectionString-Eigenschaft dokumentiert.
ms.date: 11/13/2020
ms.assetid: 0977aeee-04d1-4cce-bbed-750c77fce06e
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 42b1f8704be721c0b52b3c42946c9abbf7efd7db
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771627"
---
# <a name="connection-string-syntax"></a>Syntax von Verbindungszeichenfolgen

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

<xref:Microsoft.Data.SqlClient> hat ein `Connection`-Objekt, das von <xref:System.Data.Common.DbConnection> sowie einer anbieterspezifischen <xref:System.Data.Common.DbConnection.ConnectionString%2A>-Eigenschaft erbt. Die spezifische Syntax der Verbindungszeichenfolge für den SqlClient-Anbieter ist in dessen `ConnectionString`-Eigenschaft dokumentiert. Weitere Informationen zur Verbindungszeichenfolgensyntax finden Sie unter <xref:Microsoft.Data.SqlClient.SqlConnection.ConnectionString%2A>.

## <a name="connection-string-builders"></a>Verbindungszeichenfolgen-Generatoren

 Der Microsoft SqlClient-Datenanbieter für SQL Server bietet nun den folgenden Verbindungszeichenfolgen-Generator.

- <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder>

Mit den Verbindungszeichenfolgen-Generatoren können Sie zur Laufzeit syntaktisch gültige Verbindungszeichenfolgen erstellen, sodass Sie die Werte der Verbindungszeichenfolgen nicht manuell im Code verketten müssen. Weitere Informationen finden Sie in [Connection String Builders (Verbindungszeichenfolgengeneratoren)](connection-string-builders.md).

## <a name="windows-authentication"></a>Windows-Authentifizierung

Wir empfehlen die Verwendung der Windows-Authentifizierung (mitunter als *integrierte Sicherheit* bezeichnet) zum Herstellen einer Verbindung mit Datenquellen, die diese unterstützen. In der folgenden Tabelle wird die Syntax der Windows-Authentifizierung gezeigt, die für den Microsoft SqlClient-Datenanbieter für SQL Server genutzt wird.

|Anbieter|Syntax|  
|--------------|------------|  
|`SqlClient`|`Integrated Security=true;`<br /><br /> `-- or --`<br /><br /> `Integrated Security=SSPI;`|  

## <a name="sqlclient-connection-strings"></a>SqlClient-Verbindungszeichenfolgen

Die Syntax für eine <xref:Microsoft.Data.SqlClient.SqlConnection>-Verbindungszeichenfolge wird in der <xref:Microsoft.Data.SqlClient.SqlConnection.ConnectionString%2A?displayProperty=nameWithType>-Eigenschaft dokumentiert. Mit der <xref:Microsoft.Data.SqlClient.SqlConnection.ConnectionString%2A>-Eigenschaft können Sie eine Verbindungszeichenfolge für eine SQL Server-Datenbank abrufen oder festlegen. Die Schlüsselwörter der Verbindungszeichenfolge werden außerdem Eigenschaften in <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder> zugeordnet.

> [!IMPORTANT]
> Die Standardeinstellung für das Schlüsselwort `Persist Security Info` ist `false`. Wenn die Einstellung in `true` bzw. `yes` geändert wird, sind sicherheitsrelevante Informationen, die über diese Verbindung übertragen werden, wie Benutzer-ID und Kennwort, nicht mehr sicher. Lassen Sie `Persist Security Info` auf `false` festgelegt, um sicherzustellen, dass eine nicht vertrauenswürdige Quelle keinen Zugriff auf sensible Informationen in Verbindungszeichenfolgen hat.

### <a name="windows-authentication-with-sqlclient"></a>Windows-Authentifizierung mit SqlClient

Alle folgenden Syntaxformen verwenden beim Herstellen einer Verbindung mit der Datenbank **AdventureWorks** auf einem lokalen Server die Windows-Authentifizierung.

```csharp  
"Persist Security Info=False;Integrated Security=true;  
    Initial Catalog=AdventureWorks;Server=MSSQL1"  
"Persist Security Info=False;Integrated Security=SSPI;  
    database=AdventureWorks;server=(local)"  
"Persist Security Info=False;Trusted_Connection=True;  
    database=AdventureWorks;server=(local)"  
```  

### <a name="sql-server-authentication-with-sqlclient"></a>SQL Server-Authentifizierung mit SqlClient

Zum Herstellen einer Verbindung mit SQL Server ist vorzugsweise die Windows-Authentifizierung zu verwenden. Wenn jedoch die SQL Server-Authentifizierung erforderlich ist, verwenden Sie zum Angeben eines Benutzernamens und Kennworts die im Folgenden beschriebene Syntax. In diesem Beispiel werden der Benutzername und das Kennwort durch Sternchen dargestellt.

```csharp  
"Persist Security Info=False;User ID=*****;Password=*****;Initial Catalog=AdventureWorks;Server=MySqlServer"  
```  

Wenn Sie eine Verbindung mit Azure SQL-Datenbank oder Azure Synapse Analytics herstellen und die Anmeldeinformationen im Format `user@servername` eingeben, stellen Sie sicher, dass der Wert `servername` in der Anmeldung mit dem für `Server=` angegebenen Wert übereinstimmt.

> [!NOTE]
> Das Angeben der Windows-Authentifizierung hat Vorrang vor SQL Server-Anmeldungen. Wenn Sie sowohl die integrierte Sicherheit aktivieren als auch einen Benutzernamen und ein Kennwort angeben, werden Benutzername und Kennwort ignoriert, und die Windows-Authentifizierung wird verwendet.

### <a name="connect-to-a-named-instance-of-sql-server"></a>Herstellen einer Verbindung mit einer benannten Instanz von SQL Server

Wenn Sie eine Verbindung mit einer benannten Instanz von SQL Server herstellen möchten, verwenden Sie die Syntax *Servername\Instanzname*.

```csharp  
"Data Source=MySqlServer\MSSQL1;"  
```  

Sie können beim Erstellen einer Verbindungszeichenfolge auch die <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder.DataSource%2A>-Eigenschaft von `SqlConnectionStringBuilder` auf den Instanznamen festlegen. Die <xref:Microsoft.Data.SqlClient.SqlConnection.DataSource%2A>-Eigenschaft eines <xref:Microsoft.Data.SqlClient.SqlConnection>-Objekts ist schreibgeschützt.

### <a name="type-system-version-changes"></a>Eingeben von Änderungen der Systemversion

Das Schlüsselwort `Type System Version` in <xref:Microsoft.Data.SqlClient.SqlConnection.ConnectionString%2A?displayProperty=nameWithType> gibt die clientseitige Darstellung von SQL Server-Typen an. Weitere Informationen zum <xref:Microsoft.Data.SqlClient.SqlConnection.ConnectionString%2A?displayProperty=nameWithType>-Schlüsselwort finden Sie unter `Type System Version`.

## <a name="connect-and-attach-to-sql-server-express-user-instances"></a>Herstellen einer Verbindung und Anfügen an SQL Server Express-Benutzerinstanzen

Benutzerinstanzen sind eine Funktion in SQL Server Express. Mit ihrer Hilfe können Benutzer, die mit einem lokalen Windows-Konto der untersten Berechtigungsebene (LUA) arbeiten, eine SQL Server-Datenbank anfügen und ausführen, ohne dass dafür Administratorrechte erforderlich sind. Eine Benutzerinstanz wird mit den Windows-Anmeldeinformationen des Benutzers und nicht als Dienst ausgeführt.

Weitere Informationen zum Arbeiten mit Benutzerinstanzen finden Sie unter [SQL Server Express-Benutzerinstanzen](./sql/sql-server-express-user-instances.md).

## <a name="use-trustservercertificate"></a>Verwenden von TrustServerCertificate

Das Schlüsselwort `TrustServerCertificate` ist nur gültig, wenn unter Verwendung eines gültigen Zertifikats eine Verbindung mit einer SQL Server-Instanz hergestellt wird. Wenn `TrustServerCertificate` auf `true` festgelegt ist, verwendet die Transportschicht TLS/SSL zur Verschlüsselung des Kanals und umgeht das Durchlaufen der Zertifikatskette zur Überprüfung der Vertrauenswürdigkeit.

```csharp  
"TrustServerCertificate=true;"
```  

> [!NOTE]
> Wenn `TrustServerCertificate` auf `true` gesetzt wird und die Verschlüsselung aktiviert ist, wird die auf dem Server angegebene Verschlüsselungsstufe auch dann verwendet, wenn `Encrypt` in der Verbindungszeichenfolge auf `false` gesetzt ist. Andernfalls schlägt die Verbindung fehl.

### <a name="enable-encryption"></a>Aktivieren der Verschlüsselung

Wenn auf dem Server kein Zertifikat bereitgestellt wurde und Sie die Verschlüsselung aktivieren möchten, müssen Sie im SQL Server-Konfigurations-Manager die Optionen **Protokollverschlüsselung erzwingen** und **Dem Serverzertifikat vertrauen** aktivieren. In diesem Fall wird bei der Verschlüsselung ein selbstsigniertes Serverzertifikat ohne Überprüfung verwendet, wenn kein überprüfbares Zertifikat auf dem Server bereitgestellt wurde.

Die Anwendungseinstellungen können nicht zu einer Einschränkung der in SQL Server konfigurierten Sicherheitsstufe führen, sondern verstärken sie sogar möglicherweise. Eine Anwendung kann die Verschlüsselung anfordern, indem die Schlüsselwörter `TrustServerCertificate` und `Encrypt` auf `true` festgelegt werden. Dadurch wird sichergestellt, dass die Verschlüsselung auch dann erfolgt, wenn kein Serverzertifikat bereitgestellt wurde und die Option **Protokollverschlüsselung erzwingen** für den Client nicht konfiguriert wurde. Wenn jedoch `TrustServerCertificate` in der Clientkonfiguration nicht aktiviert wird, ist immer noch ein bereitgestelltes Serverzertifikat erforderlich.

In der folgenden Tabelle werden alle Fälle beschrieben.

|Protokollverschlüsselung erzwingen - Clienteinstellung|Dem Serverzertifikat vertrauen|Verbindungszeichenfolge/Attribut "Encrypt"/"Use Encryption for Data"|Verbindungszeichenfolge/Attribut "TrustServerCertificate"|Ergebnis|  
|----------------------------------------------|---------------------------------------------|-------------------------------------------------------------------|-----------------------------------------------------------|------------|  
|Nein|N/V|Nein (Standard)|Wird ignoriert.|Keine Verschlüsselung.|  
|Nein|N/V|Ja|Nein (Standard)|Eine Verschlüsselung findet nur statt, wenn ein überprüfbares Serverzertifikat vorliegt, anderenfalls schlägt der Verbindungsversuch fehl.|  
|Nein|N/V|Ja|Ja|Verschlüsselung wird immer durchgeführt, es wird jedoch z. B. ein selbstsigniertes Serverzertifikat verwendet.|  
|Ja|Nein|Wird ignoriert.|Wird ignoriert.|Die Verschlüsselung erfolgt nur, wenn ein überprüfbares Serverzertifikat vorhanden ist. Andernfalls schlägt der Verbindungsversuch fehl.|  
|Ja|Ja|Nein (Standard)|Wird ignoriert.|Verschlüsselung wird immer durchgeführt, es wird jedoch z. B. ein selbstsigniertes Serverzertifikat verwendet.|  
|Ja|Ja|Ja|Nein (Standard)|Die Verschlüsselung erfolgt nur, wenn ein überprüfbares Serverzertifikat vorhanden ist. Andernfalls schlägt der Verbindungsversuch fehl.|  
|Ja|Ja|Ja|Ja|Verschlüsselung wird immer durchgeführt, es wird jedoch z. B. ein selbstsigniertes Serverzertifikat verwendet.|  

Weitere Informationen finden Sie unter [Verwenden von Verschlüsselung ohne Überprüfung](/sql/relational-databases/native-client/features/using-encryption-without-validation).

## <a name="see-also"></a>Siehe auch

- [Verbindungszeichenfolgen](connection-strings.md)
- [Herstellen einer Verbindung mit Datenquellen](connecting-to-data-source.md)
- [Microsoft ADO.NET für SQL Server](microsoft-ado-net-sql-server.md)
