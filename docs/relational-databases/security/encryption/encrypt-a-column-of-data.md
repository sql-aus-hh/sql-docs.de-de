---
title: Verschlüsseln einer Datenspalte | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie mithilfe von Transact-SQL eine Datenspalte mithilfe der symmetrischen Verschlüsselung in SQL Server verschlüsseln. Dies wird manchmal auch als Verschlüsselung auf Spaltenebene oder Verschlüsselung auf Zellenebene bezeichnet.
ms.custom: ''
titleSuffix: SQL Server & Azure Synapse Analytics & Azure SQL Database & SQL Managed Instance
ms.date: 12/15/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- encryption [SQL Server], columns
- cryptography [SQL Server], columns
- column level encryption
- cell level encryption
ms.assetid: 38e9bf58-10c6-46ed-83cb-e2d76cda0adc
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current||=azure-sqldw-latest
ms.openlocfilehash: c09f7f91edf2cada0464b6cfbc1922664a86866f
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641020"
---
# <a name="encrypt-a-column-of-data"></a>Verschlüsseln einer Datenspalte

[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]  

In diesem Artikel wird beschrieben, wie Sie eine Datenspalte mithilfe der symmetrischen Verschlüsselung in [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] mithilfe von [!INCLUDE[tsql](../../../includes/tsql-md.md)]verschlüsseln können. Dies wird manchmal als Verschlüsselung auf Spaltenebene oder Verschlüsselung auf Zellenebene bezeichnet. Dieses Feature befindet sich in der Vorschau für Azure Synapse Analytics.

Die Beispiele in diesem Artikel wurden anhand von AdventureWorks2017 überprüft. Informationen zum Abrufen von Beispieldatenbanken finden Sie unter [AdventureWorks-Beispieldatenbanken](../../../samples/adventureworks-install-configure.md).

## <a name="security"></a>Sicherheit  
  
### <a name="permissions"></a>Berechtigungen  

Die folgenden Berechtigungen sind notwendig, um die Schritte unten auszuführen:  
  
- `CONTROL`-Berechtigung für die Datenbank  
- `CREATE CERTIFICATE`-Berechtigung für die Datenbank Nur Windows-Anmeldenamen, SQL Server-Anmeldenamen und Anwendungsrollen können eigene Zertifikate besitzen. Gruppen und Rollen können keine Zertifikate besitzen.  
- `ALTER`-Berechtigung für die Tabelle  
- Einige Berechtigungen für den Schlüssel und die `VIEW DEFINITION`-Berechtigung dürfen nicht verweigert worden sein.  
  
## <a name="create-database-master-key"></a>Erstellen eines Datenbankhauptschlüssels  

Um die folgenden Beispiele verwenden zu können, müssen Sie über einen Datenbankhauptschlüssel verfügen. Wenn Ihre Datenbank nicht bereits über einen Datenbankhauptschlüssel verfügt, erstellen Sie einen. Stellen Sie eine Verbindung mit Ihrer Datenbank her, und führen Sie das folgende Skript aus, um einen zu erstellen. Achten Sie darauf, ein komplexes Kennwort zu verwenden.

Kopieren Sie das folgende Beispiel, und fügen Sie es in das Abfragefenster ein, das mit der AdventureWorks-Beispieldatenbank verbunden ist. Klicken Sie auf **Ausführen**.  

```sql  
CREATE MASTER KEY ENCRYPTION BY   
PASSWORD = '<complex password>';  
```  

Erstellen Sie immer eine Sicherung Ihres Datenbankhauptschlüssels. Weitere Informationen zum Erstellen von Datenbank-Hauptschlüsseln finden Sie unter [CREATE MASTER KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-master-key-transact-sql.md).

## <a name="example-encrypt-with-symmetric-encryption-and-authenticator"></a>Beispiel: Verschlüsseln mit symmetrischer Verschlüsselung und Authentifikator
  
1. Stellen Sie im **Objekt-Explorer** eine Verbindung mit einer [!INCLUDE[ssDE](../../../includes/ssde-md.md)]-Instanz her.  
  
2. Klicken Sie in der Standardleiste auf **Neue Abfrage**.  
  
3. Kopieren Sie das folgende Beispiel, und fügen Sie es in das Abfragefenster ein, das mit der AdventureWorks-Beispieldatenbank verbunden ist. Klicken Sie auf **Ausführen**.

    ```sql
    CREATE CERTIFICATE Sales09  
       WITH SUBJECT = 'Customer Credit Card Numbers';  
    GO  
  
    CREATE SYMMETRIC KEY CreditCards_Key11  
        WITH ALGORITHM = AES_256  
        ENCRYPTION BY CERTIFICATE Sales09;  
    GO  
  
    -- Create a column in which to store the encrypted data.  
    ALTER TABLE Sales.CreditCard   
        ADD CardNumber_Encrypted varbinary(160);   
    GO  
  
    -- Open the symmetric key with which to encrypt the data.  
    OPEN SYMMETRIC KEY CreditCards_Key11  
       DECRYPTION BY CERTIFICATE Sales09;  
  
    -- Encrypt the value in column CardNumber using the  
    -- symmetric key CreditCards_Key11.  
    -- Save the result in column CardNumber_Encrypted.    
    UPDATE Sales.CreditCard  
    SET CardNumber_Encrypted = EncryptByKey(Key_GUID('CreditCards_Key11')  
        , CardNumber, 1, HASHBYTES('SHA2_256', CONVERT( varbinary  
        , CreditCardID)));  
    GO  
  
    -- Verify the encryption.  
    -- First, open the symmetric key with which to decrypt the data.  
  
    OPEN SYMMETRIC KEY CreditCards_Key11  
       DECRYPTION BY CERTIFICATE Sales09;  
    GO  
  
    -- Now list the original card number, the encrypted card number,  
    -- and the decrypted ciphertext. If the decryption worked,  
    -- the original number will match the decrypted number.  
  
    SELECT CardNumber, CardNumber_Encrypted   
        AS 'Encrypted card number', CONVERT(nvarchar,  
        DecryptByKey(CardNumber_Encrypted, 1 ,   
        HASHBYTES('SHA2_256', CONVERT(varbinary, CreditCardID))))  
        AS 'Decrypted card number' FROM Sales.CreditCard;  
    GO  
    ```  
  
## <a name="encrypt-with-simple-symmetric-encryption"></a>Verschlüsseln mit einfacher symmetrischer Verschlüsselung  

1. Stellen Sie im **Objekt-Explorer** eine Verbindung mit einer [!INCLUDE[ssDE](../../../includes/ssde-md.md)]-Instanz her.  
  
2. Klicken Sie in der Standardleiste auf **Neue Abfrage**.  
  
3. Kopieren Sie das folgende Beispiel, und fügen Sie es in das Abfragefenster ein, das mit der AdventureWorks-Beispieldatenbank verbunden ist. Klicken Sie auf **Ausführen**.  
  
    ```sql
     CREATE CERTIFICATE HumanResources037  
       WITH SUBJECT = 'Employee Social Security Numbers';  
    GO  
  
    CREATE SYMMETRIC KEY SSN_Key_01  
        WITH ALGORITHM = AES_256  
        ENCRYPTION BY CERTIFICATE HumanResources037;  
    GO  
  
    USE [AdventureWorks2012];  
    GO  
  
    -- Create a column in which to store the encrypted data.  
    ALTER TABLE HumanResources.Employee  
        ADD EncryptedNationalIDNumber varbinary(128);   
    GO  
  
    -- Open the symmetric key with which to encrypt the data.  
    OPEN SYMMETRIC KEY SSN_Key_01  
       DECRYPTION BY CERTIFICATE HumanResources037;  
  
    -- Encrypt the value in column NationalIDNumber with symmetric   
    -- key SSN_Key_01. Save the result in column EncryptedNationalIDNumber.  
    UPDATE HumanResources.Employee  
    SET EncryptedNationalIDNumber = EncryptByKey(Key_GUID('SSN_Key_01'), NationalIDNumber);  
    GO  
  
    -- Verify the encryption.  
    -- First, open the symmetric key with which to decrypt the data.  
    OPEN SYMMETRIC KEY SSN_Key_01  
       DECRYPTION BY CERTIFICATE HumanResources037;  
    GO  
  
    -- Now list the original ID, the encrypted ID, and the   
    -- decrypted ciphertext. If the decryption worked, the original  
    -- and the decrypted ID will match.  
    SELECT NationalIDNumber, EncryptedNationalIDNumber   
        AS 'Encrypted ID Number',  
        CONVERT(nvarchar, DecryptByKey(EncryptedNationalIDNumber))   
        AS 'Decrypted ID Number'  
        FROM HumanResources.Employee;  
    GO  
    ```  
  
 Weitere Informationen finden Sie unter  
  
-   [CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../../t-sql/statements/create-certificate-transact-sql.md)  
  
-   [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-symmetric-key-transact-sql.md)  
  
-   [ALTER TABLE &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-table-transact-sql.md)  
  
-   [OPEN SYMMETRIC KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/open-symmetric-key-transact-sql.md)  
