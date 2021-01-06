---
title: Ändern der Transaktionssicherheit (gespiegelte Datenbank)
description: Ändern des Attributs für Transaktionssicherheit für eine SQL Server-Datenbankspiegelungssitzung mithilfe von Transact-SQL
ms.custom: seo-lt-2019
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- transaction safety [SQL Server database mirroring]
ms.assetid: 8b03bb82-8589-4558-8545-9942fe008391
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 98865c826e9ee4c0cf0da05f5668feaa41fd6b44
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643631"
---
# <a name="change-transaction-safety-in-a-database-mirroring-session-transact-sql"></a>Ändern der Transaktionssicherheit in einer Datenbank-Spiegelungssitzung (Transact-SQL)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Die Transaktionssicherheit ist das Attribut, das den Betriebsmodus der Sitzung steuert. Der Datenbankbesitzer kann die Transaktionssicherheit jedoch jederzeit ändern. Standardmäßig ist die Sicherheitsstufe für Transaktionen auf FULL (synchroner Betriebsmodus) festgelegt.  
  
 Wird die Transaktionssicherheit deaktiviert, geht die Sitzung in den asynchronen Betriebsmodus über, in dem die Leistung maximiert wird. Falls der Prinzipal nicht mehr verfügbar ist, wird der Spiegel beendet, ist jedoch als betriebsbereit verfügbar (ein Failover erfordert das Erzwingen des Diensts bei möglichem Datenverlust).  
  
### <a name="to-turn-on-transaction-safety"></a>So aktivieren Sie die Transaktionssicherheit  
  
1.  Stellen Sie eine Verbindung mit dem Prinzipalserver her.  
  
2.  Führen Sie die folgende Transact-SQL-Anweisung aus:  
  
    ```  
    ALTER DATABASE <database> SET PARTNER SAFETY FULL  
    ```  
  
     Dabei ist *\<database>* der Name der gespiegelten Datenbank.  
  
### <a name="to-turn-off-transaction-safety"></a>So deaktivieren Sie die Transaktionssicherheit  
  
1.  Stellen Sie eine Verbindung mit dem Prinzipalserver her.  
  
2.  Führen Sie die folgende Anweisung aus:  
  
    ```  
    ALTER DATABASE <database> SET PARTNER SAFETY OFF  
    ```  
  
     Dabei ist *\<database>* die gespiegelte Datenbank.  
  
## <a name="see-also"></a>Weitere Informationen  
 [ALTER DATABASE-Datenbankspiegelung &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-database-mirroring.md)   
 [Database Mirroring Operating Modes](../../database-engine/database-mirroring/database-mirroring-operating-modes.md)  
  
  
