---
description: sys.database_service_objectives (Azure SQL-Datenbank)
title: sys.database_service_objectives
titleSuffix: Azure SQL Database
ms.date: 03/21/2018
ms.service: sql-database
ms.prod_service: sql-database, sql-data-warehouse
ms.reviewer: ''
ms.topic: conceptual
keywords:
- elastischer Pool
- Pool für elastische Datenbanken, Verwaltung
f1_keywords:
- DATABASE_SERVICE_OBJECTIVES_TSQL
ms.assetid: cecd8c31-06c0-4aa7-85d3-ac590e6874fa
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.custom: seo-dt-2019
monikerRange: = azuresqldb-current || = azure-sqldw-latest
ms.openlocfilehash: 861e8297a54cff399425e521591cd983afbb0cc6
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098010"
---
# <a name="sysdatabase_service_objectives-azure-sql-database"></a>sys.database_service_objectives (Azure SQL-Datenbank)
[!INCLUDE [asdb-asdbmi-asa](../../includes/applies-to-version/asdb-asdbmi-asa.md)]

Gibt die Edition (Dienst Ebene), das Dienst Ziel (Tarif) und den Namen des elastischen Pools (sofern vorhanden) für eine Azure SQL-Datenbank oder eine Azure-Synapse-Analyse zurück. Wenn eine Anmeldung an der Masterdatenbank in einem Azure SQL-Datenbank-Server besteht, werden Informationen zu allen Datenbanken zurückgegeben. Für Azure Synapse Analytics müssen Sie mit der Masterdatenbank verbunden sein, um eine Anmeldung erstellen zu können.  
  
  
 Informationen zu den Preisen finden Sie unter [SQL-Daten Bankoptionen und-Leistung:](https://azure.microsoft.com/pricing/details/sql-database/) Preise für SQL-Datenbank und Preise für [Azure Synapse-Analysen](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).  
  
 Informationen zum Ändern der Dienst Einstellungen finden Sie unter [ALTER DATABASE (Azure SQL-Datenbank)](../../t-sql/statements/alter-database-transact-sql.md) und [ALTER DATABASE (Azure Synapse Analytics)](../../t-sql/statements/alter-database-transact-sql.md?view=azure-sqldw-latest&preserve-view=true).  
  
 Die sys.database_service_objectives Sicht enthält die folgenden Spalten.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|database_id|INT|Die ID der Datenbank, die innerhalb einer Instanz von Azure SQL-Datenbankserver eindeutig ist. Joinfähig mit [sys.-Datenbanken &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).|  
|Edition|sysname|Die Dienst Ebene für die Datenbank oder Data Warehouse: **Basic**, **Standard**, **Premium** oder **Data Warehouse**.|  
|service_objective|sysname|Der Tarif der Datenbank. Wenn sich die Datenbank in einem Pool für elastische Datenbanken befindet, wird **elasticpool** zurückgegeben.<br /><br /> Auf dem **Basic** -Level gibt **Basic** zurück.<br /><br /> Eine **einzelne Datenbank in einer Standard Dienst Ebene** gibt eine der folgenden zurück: S0, S1, S2, S3, S4, S6, S7, S9 oder S12.<br /><br /> **Eine Einzel Datenbank in einem Premium** -Tarif gibt Folgendes zurück: P1, P2, P4, P6, P11 oder P15.<br /><br /> **Azure Synapse Analytics** gibt DW100 über DW30000c zurück.<br /><br /> Weitere Informationen finden Sie unter [Einzel Datenbanken](/azure/sql-database/sql-database-dtu-resource-limits-single-databases/), [Pools für elastische](/azure/sql-database/sql-database-dtu-resource-limits-elastic-pools/)Datenbanken, [Data](/azure/sql-data-warehouse/what-is-a-data-warehouse-unit-dwu-cdwu/) Warehouse|  
|elastic_pool_name|sysname|Der Name des [Pools für elastische](/azure/azure-sql/database/elastic-pool-overview) Datenbanken, zu dem die Datenbank gehört. Gibt **null** zurück, wenn die Datenbank eine einzelne Datenbank oder eine Data Warehouse ist.|  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die **DBManager** -Berechtigung für die Master-Datenbank.  Auf Datenbankebene muss der Benutzer der Ersteller oder Besitzer sein.  
  
## <a name="examples"></a>Beispiele  
 Dieses Beispiel kann in der Master-Datenbank oder in Azure SQL-Datenbank-Benutzer Datenbanken ausgeführt werden. Die Abfrage gibt die Informationen zu Name, Dienst und Leistungs Ebene der Datenbank (en) zurück.  
  
```sql  
SELECT  d.name,   
     slo.*    
FROM sys.databases d   
JOIN sys.database_service_objectives slo    
ON d.database_id = slo.database_id;  
  
```  
  
