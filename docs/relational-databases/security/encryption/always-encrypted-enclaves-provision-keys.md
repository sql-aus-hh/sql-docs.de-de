---
description: Bereitstellen Enclave-fähiger Schlüssel
title: Bereitstellen Enclave-fähiger Schlüssel | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/15/2021
ms.prod: sql
ms.reviewer: vanto
ms.prod_service: database-engine, sql-database
ms.technology: security
ms.topic: conceptual
author: jaszymas
ms.author: jaszymas
monikerRange: '>= sql-server-ver15'
ms.openlocfilehash: e28b6d18b5fe466aa239164b18ebdfe5fef0895c
ms.sourcegitcommit: 8ca4b1398e090337ded64840bcb8d6c92d65c29e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/16/2021
ms.locfileid: "98534759"
---
# <a name="provision-enclave-enabled-keys"></a>Bereitstellen Enclave-fähiger Schlüssel

[!INCLUDE [sqlserver2019-windows-only-asdb](../../../includes/applies-to-version/sqlserver2019-windows-only-asdb.md)]

In diesem Artikel wird beschrieben, wie Sie Enclave-fähige Schlüssel bereitstellen, die Berechnungen in serverseitigen Secure Enclaves unterstützen, die für [Always Encrypted mit Secure Enclaves](always-encrypted-enclaves.md) verwendet werden. 

Die allgemeinen Richtlinien und Prozesse für die [Verwaltung von Always Encrypted-Schlüsseln](overview-of-key-management-for-always-encrypted.md) gelten für die Bereitstellung von Enclave-fähigen Schlüsseln. Dieser Artikel befasst sich mit Details zu Always Encrypted mit Secure Enclaves.

Stellen Sie sicher, dass der neue Schlüssel Enclave-Berechnungen unterstützt, um einen Enclave-fähigen Spaltenhauptschlüssel mithilfe von SQL Server Management Studio (SSMS) oder PowerShell bereitzustellen. Das führt dazu, dass das Tool (SSMS oder PowerShell) die `CREATE COLUMN MASTER KEY`-Anweisung generiert, die `ENCLAVE_COMPUTATIONS` in den Metadaten des Spaltenhauptschlüssels in der Datenbank festlegt. Weitere Informationen finden Sie unter [CREATE COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-column-master-key-transact-sql.md). 

Das Tool signiert die Spaltenhaupteigenschaften digital mit dem Spaltenhauptschlüssen und speichert die Signatur in den Datenbankmetadaten. Die Signatur verhindert böswillige Änderungen mit der Einstellung `ENCLAVE_COMPUTATIONS`. Der SQL-Clienttreiber überprüft die Signaturen, bevor die Enclave-Nutzung zugelassen wird. Dadurch haben Sicherheitsadministratoren die Kontrolle darüber, welche Spaltendaten innerhalb der Enclave berechnet werden können.

`ENCLAVE_COMPUTATIONS` ist unveränderlich, d. h. Sie können keine Änderungen vornehmen, nachdem Sie den Spaltenhauptschlüssel in den Metadaten definiert haben. Sie müssen den Spaltenhauptschlüssel rotieren und durch einen Enclave-fähigen Spaltenhauptschlüssel ersetzen, um Enclave-Berechnungen mithilfe eines Spaltenverschlüsselungsschlüssels zu aktivieren, der von einem angegebenen Spaltenhauptschlüssel verschlüsselt wird. Informationen hierzu finden Sie unter [Rotieren Enclave-fähiger Schlüssel](always-encrypted-enclaves-rotate-keys.md).

> [!NOTE]
> Derzeit unterstützen sowohl SSMS als auch PowerShell Enclave-fähige Spaltenhauptschlüssel, die in Azure Key Vault oder im Windows-Zertifikatspeicher gespeichert werden. Hardwaresicherheitsmodule (mit CNG oder CAPI) werden nicht unterstützt.

Sie müssen sicherstellen, dass Sie einen Enclave-fähigen Spaltenhauptschlüssel zum Verschlüsseln des neuen Schlüssels auswählen, um einen Enclave-fähigen Spaltenverschlüsselungsschlüssel zu erstellen. 

In den folgenden Abschnitten finden Sie weitere Informationen zum Bereitstellen Enclave-fähiger Schlüssel mithilfe von SSMS und PowerShell.

## <a name="provision-enclave-enabled-keys-using-sql-server-management-studio"></a>Bereitstellen von Enclave-fähigen-Schlüsseln mithilfe von SQL Server Management Studio
In SQL Server Management Studio können Sie Folgendes bereitstellen:
- Einen Enclave-fähigen Spaltenhauptschlüssel über das Dialogfeld **Neuer Spaltenhauptschlüssel**
- Einen Enclave-fähigen Spaltenverschlüsselungsschlüssel über das Dialogfeld **Neuer Spaltenverschlüsselungsschlüssel**

> [!NOTE]
> Der [Always Encrypted-Assistent](always-encrypted-wizard.md) unterstützt das Erstellen von Enclave-fähigen Schlüsseln derzeit nicht. Sie können Enclave-fähige Schlüssel jedoch zuerst mithilfe der oben genannten Dialogfelder erstellen und anschließend, wenn Sie den Assistenten ausführen, eine bereits vorhandene Enclave-fähige Spaltenverschlüsselung für Spalten auswählen, die Sie verschlüsseln möchten.

Minimale Anforderungen an die SSMS-Version:

- SSMS 18.3 bei Verwendung von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]
- SSMS 18.8 bei Verwendung von [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)]

### <a name="provision-enclave-enabled-column-master-keys-with-the-new-column-master-key-dialog"></a>Bereitstellen Enclave-fähiger Spaltenhauptschlüssel mit dem Dialogfeld „Neuer Spaltenhauptschlüssel“
Führen Sie die Schritte unter [Bereitstellen von Spaltenhauptschlüsseln mit dem Dialogfeld „Neuer Spaltenhauptschlüssel“](configure-always-encrypted-keys-using-ssms.md#provision-column-master-keys-with-the-new-column-master-key-dialog) aus, um einen Enclave-fähigen Spaltenhauptschlüssel bereitzustellen. Stellen Sie sicher, dass Sie die Option **Enclave-Berechnungen zulassen** aktivieren. Diese Option sehen Sie im folgenden Screenshot:

![Enclave-Berechnungen zulassen](./media/always-encrypted-enclaves/allow-enclave-computations.png)

> [!NOTE]
> Das Kontrollkästchen **Enclave-Berechnungen zulassen** wird nur angezeigt, wenn eine Secure Enclave für Ihre Datenbank konfiguriert ist. Wenn Sie [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] verwenden, finden Sie weitere Informationen unter [Konfigurieren von Secure Enclaves in SQL Server](always-encrypted-enclaves-configure-enclave-type.md). Wenn Sie [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] verwenden, finden Sie weitere Informationen unter [Aktivieren von Intel SGX für die Azure SQL-Datenbank](/azure/azure-sql/database/always-encrypted-enclaves-enable-sgx).

> [!TIP]
> Klicken Sie im Objekt-Explorer mit der rechten Maustaste auf den Spaltenhauptschlüssel, und wählen Sie dann **Eigenschaften** aus, um zu überprüfen, ob er Enclave-fähig ist. Wenn der Schlüssel Enclave-fähig ist, wird **Enclave Computations: Allowed** (Enclave-Berechnungen: Zugelassen) im Fenster mit den Eigenschaften des Schlüssels angezeigt. Alternativ können Sie die Ansicht [sys.column_master_keys (Transact-SQL)](../../system-catalog-views/sys-column-master-keys-transact-sql.md) verwenden.

### <a name="provision-enclave-enabled-column-encryption-keys-with-the-new-column-encryption-key-dialog"></a>Bereitstellen Enclave-fähiger Spaltenverschlüsselungsschlüssel mithilfe des Dialogfelds „Neuer Spaltenverschlüsselungsschlüssel“
Führen Sie die Schritte unter [Bereitstellen von Spaltenverschlüsselungsschlüsseln mit dem Dialogfeld „Neuer Spaltenverschlüsselungsschlüssel“](configure-always-encrypted-keys-using-ssms.md#provision-column-encryption-keys-with-the-new-column-encryption-key-dialog) aus, um einen Enclave-fähigen Spaltenverschlüsselungsschlüssel bereitzustellen. Stellen Sie bei der Auswahl eines Spaltenhauptschlüssels sicher, dass dieser Enclave-fähig ist.

> [!TIP]
> Klicken Sie im Objekt-Explorer mit der rechten Maustaste auf den Spaltenverschlüsselungsschlüssel, und wählen Sie dann **Eigenschaften** aus, um zu überprüfen, ob er Enclave-fähig ist. Wenn der Schlüssel Enclave-fähig ist, wird **Enclave Computations: Allowed** (Enclave-Berechnungen: Zugelassen) im Fenster mit den Eigenschaften des Schlüssels angezeigt.

## <a name="provision-enclave-enabled-keys-using-powershell"></a>Bereitstellen Enclave-fähiger Schlüssel mit PowerShell
Für die Bereitstellung Enclave-fähiger Schlüssel mithilfe von PowerShell benötigen Sie mindestens die Version 21.1.18179 des PowerShell-Moduls „SqlServer“.

Im Allgemeinen gelten die Bereitstellungsworkflows für PowerShell-Schlüssel (mit und ohne Rollentrennung), die unter [Bereitstellen von Always Encrypted-Schlüsseln mithilfe von PowerShell](configure-always-encrypted-keys-using-powershell.md) beschrieben werden, auch für Enclave-fähige Schlüssel. In diesem Abschnitt werden spezifische Details zu Enclave-fähigen Schlüsseln beschrieben.

Das PowerShell-Modul „SqlServer“ erweitert die Cmdlets [**New-SqlCertificateStoreColumnMasterKeySettings**](/powershell/module/sqlserver/new-sqlcertificatestorecolumnmasterkeysettings) und [**New-SqlAzureKeyVaultColumnMasterKeySettings**](/powershell/module/sqlserver/new-sqlazurekeyvaultcolumnmasterkeysettings) mit dem Parameter `-AllowEnclaveComputations`, um Ihnen während des Bereitstellungsprozesses das Festlegen eines Enclave-fähigen Spaltenhauptschlüssels zu ermöglichen. Beide Cmdlets erstellen ein lokales Objekt, das Eigenschaften eines Spaltenhauptschlüssels enthält (der in Azure Key Vault oder im Windows-Zertifikatspeicher gespeichert wird). Die Eigenschaft `-AllowEnclaveComputations` kennzeichnet den Schlüssel im lokalen Objekt als Enclave-fähig, wenn sie festgelegt wird. Dies bewirkt außerdem, dass das Cmdlet auf den referenzierten Spaltenhauptschlüssel zugreift (der sich in Azure Key Vault oder im Windows-Zertifikatspeicher befindet), um die Eigenschaften des Schlüssels digital zu signieren. Sobald Sie ein Einstellungsobjekt für einen neuen Enclave-fähigen Spaltenhauptschlüssel erstellt haben, können Sie diesen in anschließenden Aufrufen des Cmdlets [**New-SqlColumnMasterKey**](/powershell/module/sqlserver/new-sqlcolumnmasterkey) verwenden, um ein Metadatenobjekt zu erstellen, das den neuen Schlüssel in der Datenbank beschreibt.

Die Bereitstellung Enclave-fähiger Spaltenverschlüsselungsschlüssel unterscheidet sich nicht von der Bereitstellung nicht Enclave-fähiger Spaltenverschlüsselungsschlüssel. Sie müssen lediglich sicherstellen, dass der Spaltenhauptschlüssel, der zum Verschlüsseln des neuen Spaltenverschlüsselungsschlüssel verwendet wird, Enclave-fähig ist.

> [!NOTE]
> Das PowerShell-Modul „SqlServer“ unterstützt die Bereitstellung von Enclave-fähigen Schlüsseln, die in Hardwaresicherheitsmodulen (mit CNG oder CAPI) gespeichert werden, derzeit nicht.

### <a name="example---provision-enclave-enabled-keys-using-windows-certificate-store"></a>Beispiel: Bereitstellen Enclave-fähiger Schlüssel mithilfe des Windows-Zertifikatspeichers
Im folgenden vollumfassenden Beispiel wird veranschaulicht, wie Enclave-fähige Schlüssel bereitgestellt werden, wenn der Spaltenhauptschlüssel im Windows-Zertifikatspeicher gespeichert wird. Das Skript basiert auf dem Beispiel unter [Windows-Zertifikatspeicher ohne Rollentrennung (Beispiel)](configure-always-encrypted-keys-using-powershell.md#windows-certificate-store-without-role-separation-example). Dabei sollte unbedingt die Verwendung des `-AllowEnclaveComputations`-Parameters im Cmdlet [**New-SqlCertificateStoreColumnMasterKeySettings**](/powershell/module/sqlserver/new-sqlcertificatestorecolumnmasterkeysettings) beachtet werden, was den einzigen Unterschied zwischen den Workflows der beiden Beispiele darstellt.

```powershell
# Create a column master key in Windows Certificate Store.
$cert = New-SelfSignedCertificate -Subject "AlwaysEncryptedCert" -CertStoreLocation Cert:CurrentUser\My -KeyExportPolicy Exportable -Type DocumentEncryptionCert -KeyUsage DataEncipherment -KeySpec KeyExchange

# Import the SqlServer module.
Import-Module "SqlServer"

# Connect to your database.
$serverName = "<server name>"
$databaseName = "<database name>"
# Change the authentication method in the connection string, if needed.
$connStr = "Server = " + $serverName + "; Database = " + $databaseName + "; Integrated Security = True"
$database = Get-SqlDatabase -ConnectionString $connStr

# Create a SqlColumnMasterKeySettings object for your column master key
# using the -AllowEnclaveComputations parameter.
$cmkSettings = New-SqlCertificateStoreColumnMasterKeySettings -CertificateStoreLocation "CurrentUser" -Thumbprint $cert.Thumbprint -AllowEnclaveComputations

# Create column master key metadata in the database.
$cmkName = "CMK1"
New-SqlColumnMasterKey -Name $cmkName -InputObject $database -ColumnMasterKeySettings $cmkSettings

# Generate a column encryption key, encrypt it with the column master key and create column encryption key metadata in the database. 
$cekName = "CEK1"
New-SqlColumnEncryptionKey -Name $cekName  -InputObject $database -ColumnMasterKey $cmkName
```

### <a name="example---provision-enclave-enabled-keys-using-azure-key-vault"></a>Beispiel: Bereitstellen Enclave-fähiger Schlüssel mithilfe von Azure Key Vault
Im folgenden vollumfassenden Beispiel wird veranschaulicht, wie Enclave-fähige Schlüssel bereitgestellt werden, wenn der Spaltenhauptschlüssel in Azure Key Vault gespeichert wird. Das Skript basiert auf dem Beispiel unter [Azure Key Vault ohne Rollentrennung (Beispiel)](configure-always-encrypted-keys-using-powershell.md#azure-key-vault-without-role-separation-example). Hier sollten Sie unbedingt die zwei Unterschiede zwischen den Workflows für Enclave-fähige und nicht Enclave-fähige Schlüssel beachten. 
- Im folgenden Skript wird im Cmdlet [**New-SqlCertificateStoreColumnMasterKeySettings**](/powershell/module/sqlserver/new-sqlcertificatestorecolumnmasterkeysettings) der Parameter `-AllowEnclaveComputations` verwendet, damit der neue Spaltenhauptschlüssel Enclave-fähig ist. 
- Im folgenden Skript wird das Cmdlet [**Add-SqlAzureAuthenticationContext**](/powershell/module/sqlserver/add-sqlazureauthenticationcontext) für die Anmeldung bei Azure aufgerufen, bevor das Cmdlet [**New-SqlAzureKeyVaultColumnMasterKeySettings**](/powershell/module/sqlserver/new-sqlazurekeyvaultcolumnmasterkeysettings) aufgerufen wird. Die Anmeldung bei Azure ist erforderlich, da der Parameter `-AllowEnclaveComputations` bewirkt, dass das Cmdlet **New-SqlAzureKeyVaultColumnMasterKeySettings** Azure Key Vault aufruft, um die Eigenschaften des Spaltenhauptschlüssels zu signieren.

```powershell
# Create a column master key in Azure Key Vault.
Import-Module Az
Connect-AzAccount
$SubscriptionId = "<Azure SubscriptionId>"
$resourceGroup = "<resource group name>"
$azureLocation = "<datacenter location>"
$akvName = "<key vault name>"
$akvKeyName = "<key name>"
$azureCtx = Set-AzConteXt -SubscriptionId $SubscriptionId # Sets the context for the below cmdlets to the specified subscription.
New-AzResourceGroup -Name $resourceGroup -Location $azureLocation # Creates a new resource group - skip, if your desired group already exists.
New-AzKeyVault -VaultName $akvName -ResourceGroupName $resourceGroup -Location $azureLocation # Creates a new key vault - skip if your vault already exists.
Set-AzKeyVaultAccessPolicy -VaultName $akvName -ResourceGroupName $resourceGroup -PermissionsToKeys get, create, delete, list, wrapKey,unwrapKey, sign, verify -UserPrincipalName $azureCtx.Account
$akvKey = Add-AzureKeyVaultKey -VaultName $akvName -Name $akvKeyName -Destination "Software"

# Connect to your database.
$serverName = "<server name>"
$databaseName = "<database name>"
# Change the authentication method in the connection string, if needed.
$connStr = "Server = " + $serverName + "; Database = " + $databaseName + "; Integrated Security = True"
$database = Get-SqlDatabase -ConnectionString $connStr

# Authenticate to Azure - it is required before calling the next cmdlet.
Add-SqlAzureAuthenticationContext -Interactive

# Create a SqlColumnMasterKeySettings object for your column master key. 
$cmkSettings = New-SqlAzureKeyVaultColumnMasterKeySettings -KeyURL $akvKey.ID -AllowEnclaveComputations

# Create column master key metadata in the database.
$cmkName = "CMK1"
New-SqlColumnMasterKey -Name $cmkName -InputObject $database -ColumnMasterKeySettings $cmkSettings

# Generate a column encryption key, encrypt it with the column master key and create column encryption key metadata in the database. 
$cekName = "CEK1"
New-SqlColumnEncryptionKey -Name $cekName -InputObject $database -ColumnMasterKey $cmkName
```

## <a name="next-steps"></a>Nächste Schritte
- [Ausführen von Transact-SQL-Anweisungen mit Secure Enclaves](always-encrypted-enclaves-query-columns.md)
- [Konfigurieren einer direkten Spaltenverschlüsselung mithilfe von Always Encrypted mit Secure Enclaves](always-encrypted-enclaves-configure-encryption.md)
- [Aktivieren von Always Encrypted mit Secure Enclaves für vorhandene verschlüsselte Spalten](always-encrypted-enclaves-enable-for-encrypted-columns.md)
- [Entwickeln von Anwendungen mithilfe von Always Encrypted mit Secure Enclaves](always-encrypted-enclaves-client-development.md) 

## <a name="see-also"></a>Weitere Informationen  
- [Tutorial: Erste Schritte mit Always Encrypted mit Secure Enclaves in SQL Server](../tutorial-getting-started-with-always-encrypted-enclaves.md)
- [Tutorial: Erste Schritte mit Always Encrypted mit Secure Enclaves in Azure SQL-Datenbank](/azure/azure-sql/database/always-encrypted-enclaves-getting-started)
- [Verwalten von Schlüsseln für Always Encrypted mit Secure Enclaves](always-encrypted-enclaves-manage-keys.md)
- [CREATE COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-column-master-key-transact-sql.md)