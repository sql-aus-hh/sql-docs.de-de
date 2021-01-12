---
description: CREATE SYNONYM (Transact-SQL)
title: CREATE SYNONYM (Transact-SQL)
ms.custom: ''
ms.date: 04/11/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CREATE_SYNONYM_TSQL
- SYNONYM_TSQL
- SYNONYM
- CREATE SYNONYM
dev_langs:
- TSQL
helpviewer_keywords:
- alternate names [SQL Server]
- names [SQL Server], synonyms
- CREATE SYNONYM statement
- synonyms [SQL Server], creating
ms.assetid: 41313809-e970-449c-bc35-85da2ef96e48
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: ec09397653e497ff06283aed467fcfd18c6267fd
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102340"
---
# <a name="create-synonym-transact-sql"></a>CREATE SYNONYM (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Erstellt ein neues Synonym.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql  
-- SQL Server Syntax  
  
CREATE SYNONYM [ schema_name_1. ] synonym_name FOR <object>  
  
<object> :: =  
{  
    [ server_name.[ database_name ] . [ schema_name_2 ]. object_name   
  | database_name . [ schema_name_2 ].| schema_name_2. ] object_name  
}  
```  
  
```  
-- Azure SQL Database Syntax  
  
CREATE SYNONYM [ schema_name_1. ] synonym_name FOR < object >  
  
< object > :: =  
{  
    [database_name. [ schema_name_2 ].| schema_name_2. ] object_name  
}  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *schema_name_1*  
 Gibt das Schema an, in dem das Synonym erstellt wird. Wird *schema* nicht angegeben ist, verwendet [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] das Standardschema des aktuellen Benutzers.  
  
 *synonym_name*  
 Der Name des neuen Synonyms.  
  
 *server_name*  
 **Gilt für**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher.  
  
 Der Name des Servers, auf dem sich das Basisobjekt befindet.  
  
 *database_name*  
 Der Name der Datenbank, auf der sich das Basisobjekt befindet. Wenn *database_name* nicht angegeben ist, wird der Name der aktuellen Datenbank verwendet.  
  
 *schema_name_2*  
 Der Name des Schemas des Basisobjekts. Wenn *schema_name* nicht angegeben ist, wird das Standardschema des aktuellen Benutzers verwendet.  
  
 *object_name*  
 Der Name des Basisobjekts, auf das das Synonym verweist.  
  
 Die Azure SQL-Datenbank unterstützt das aus drei Teilen bestehende Format „database_name.[schema_name].object_name“, wenn „database_name“ die aktuelle Datenbank bzw. „database_name tempdb“ ist und „object_name“ mit „#“ beginnt.  
  
## <a name="remarks"></a>Bemerkungen  
 Das Basisobjekt muss zur Erstellungszeit des Synonyms nicht notwendigerweise vorhanden sein. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] überprüft das Vorhandensein des Basisobjekts zur Laufzeit.  
  
 Synonyme können für die folgenden Objekttypen erstellt werden:  
  
- Assembly (CLR)-gespeicherte Prozeduren
- Assembly (CLR)-Tabellenwertfunktionen
- Assembly (CLR)-Skalarfunktionen
- Assembly (CLR)-Aggregatfunktionen
- Replikationsfilterprozedur
- Erweiterte gespeicherte Prozeduren
- SQL-Skalarfunktionen
- SQL-Tabellenwertfunktion
- SQL-Inline-Tabellenwertfunktion
- Gespeicherte SQL-Prozeduren
- Tabelle<sup>1</sup> (Benutzerdefiniert)
- Sicht

 <sup>1 Enthält lokale und globale temporäre Tabellen</sup>  
  
 Vierteilige Namen für Funktionsbasisobjekte werden nicht unterstützt.  
  
 In dynamischem SQL können Synonyme erstellt und gelöscht werden. Außerdem kann auf sie verwiesen werden.
 
 > [!NOTE]
 > Synonyme sind datenbankspezifisch, andere Datenbanken können also nicht auf sie zugreifen.
  
## <a name="permissions"></a>Berechtigungen  
 Zum Erstellen eines Synonyms in einem Schema muss ein Benutzer über die CREATE SYNONYM-Berechtigung verfügen und entweder der Besitzer des Schemas sein oder über die ALTER SCHEMA-Berechtigung verfügen.  
  
 Die CREATE SYNONYM-Berechtigung ist eine erteilbare Berechtigung.  
  
> [!NOTE]  
>  Sie benötigen keine Berechtigung für das Basisobjekt, um die CREATE SYNONYM-Anweisung erfolgreich auszuführen, da alle Berechtigungsüberprüfungen für das Basisobjekt bis zur Laufzeit verzögert werden.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-creating-a-synonym-for-a-local-object"></a>A. Erstellen eines Synonyms für ein lokales Objekt  
 Im folgenden Beispiel wird zunächst ein Synonym für das Basisobjekt, `Product` in der `AdventureWorks2012`-Datenbank, erstellt und dann das Synonym abgefragt.  
  
```sql 
-- Create a synonym for the Product table in AdventureWorks2012.  
CREATE SYNONYM MyProduct  
FOR AdventureWorks2012.Production.Product;  
GO  
  
-- Query the Product table by using the synonym.  
SELECT ProductID, Name   
FROM MyProduct  
WHERE ProductID < 5;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 ----------------------- 
 ProductID   Name 
 ----------- -------------------------- 
 1           Adjustable Race 
 2           Bearing Ball 
 3           BB Ball Bearing 
 4           Headset Ball Bearings 

 (4 row(s) affected)
``` 
  
### <a name="b-creating-a-synonym-to-remote-object"></a>B. Erstellen eines Synonyms zu einem Remoteobjekt  
 Im folgenden Beispiel befindet sich das Basisobjekt, `Contact`, auf einem Remoteserver namens `Server_Remote`.  
  
**Gilt für**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher.  
  
```sql 
EXEC sp_addlinkedserver Server_Remote;  
GO  
USE tempdb;  
GO  
CREATE SYNONYM MyEmployee FOR Server_Remote.AdventureWorks2012.HumanResources.Employee;  
GO  
```  
  
### <a name="c-creating-a-synonym-for-a-user-defined-function"></a>C. Erstellen eines Synonyms für eine benutzerdefinierte Funktion  
 Im folgenden Beispiel wird eine Funktion mit dem Namen `dbo.OrderDozen` erstellt, durch die Bestellmengen auf ein Dutzend erhöht werden. Im Beispiel wird dann das Synonym `dbo.CorrectOrder` für die `dbo.OrderDozen`-Funktion erstellt.  
  
```sql  
-- Creating the dbo.OrderDozen function  
CREATE FUNCTION dbo.OrderDozen (@OrderAmt INT)  
RETURNS INT  
WITH EXECUTE AS CALLER  
AS  
BEGIN  
IF @OrderAmt % 12 <> 0  
BEGIN  
    SET @OrderAmt +=  12 - (@OrderAmt % 12)  
END  
RETURN(@OrderAmt);  
END;  
GO  
  
-- Using the dbo.OrderDozen function  
DECLARE @Amt INT;  
SET @Amt = 15;  
SELECT @Amt AS OriginalOrder, dbo.OrderDozen(@Amt) AS ModifiedOrder;  
  
-- Create a synonym dbo.CorrectOrder for the dbo.OrderDozen function.  
CREATE SYNONYM dbo.CorrectOrder  
FOR dbo.OrderDozen;  
GO  
  
-- Using the dbo.CorrectOrder synonym.  
DECLARE @Amt INT;  
SET @Amt = 15;  
SELECT @Amt AS OriginalOrder, dbo.CorrectOrder(@Amt) AS ModifiedOrder;  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [DROP SYNONYM &#40;Transact-SQL&#41;](../../t-sql/statements/drop-synonym-transact-sql.md)   
 [GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)  
  
  
