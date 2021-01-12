---
description: ALTER DATABASE-Datenbankspiegelung (Transact-SQL)
title: ALTER DATABASE-Datenbankspiegelung (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 02/21/2019
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- witness [SQL Server], establishing
- manual failover [SQL Server]
- ALTER DATABASE statement, database mirroring
- database mirroring [SQL Server], Transact-SQL
ms.assetid: 27a032ef-1cf6-4959-8e67-03d28c4b3465
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 60dcccf63cb8ca8116f5fdbfcfe7482e0a1560a4
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094684"
---
# <a name="alter-database-transact-sql-database-mirroring"></a>ALTER DATABASE- (Transact-SQL)-Datenbankspiegelung

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

> [!NOTE]
> [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Verwenden Sie stattdessen [!INCLUDE[ssHADR](../../includes/sshadr-md.md)].

Steuert die Datenbankspiegelung für eine Datenbank. Werte, die mit den Optionen für die Datenbankspiegelung angegeben werden, gelten für Kopien der Datenbank sowie für die gesamte Sitzung zur Datenbankspiegelung. Pro ALTER DATABASE-Anweisung ist nur eine \<database_mirroring_option>-Option erlaubt.

> [!NOTE]
> Es empfiehlt sich, die Konfiguration der Datenbankspiegelung außerhalb der Spitzenbetriebszeiten durchzuführen, da sich die Konfiguration auf die Leistung auswirken kann.

Informationen zu ALTER DATABASE-Optionen finden Sie unter [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md). Informationen zu ALTER DATABASE SET-Optionen finden Sie unter [ALTER DATABASE SET-Optionen](../../t-sql/statements/alter-database-transact-sql-set-options.md).

![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Syntax

```syntaxsql

ALTER DATABASE database_name
SET { <partner_option> | <witness_option> }
  <partner_option> ::=
    PARTNER { = 'partner_server'
            | FAILOVER
            | FORCE_SERVICE_ALLOW_DATA_LOSS
            | OFF
            | RESUME
            | SAFETY { FULL | OFF }
            | SUSPEND
            | TIMEOUT integer
            }
  <witness_option> ::=
    WITNESS { = 'witness_server'
            | OFF
            }
  
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente

> [!IMPORTANT]
> Ein SET PARTNER- oder SET WITNESS-Befehl kann nach der Eingabe erfolgreich abgeschlossen werden und dennoch später einen Fehler generieren.
> [!NOTE]
> Die Optionen der ALTER DATABASE-Datenbankspiegelung sind für enthaltene Datenbanken nicht verfügbar.

*database_name* Der Name der Datenbank, die geändert werden soll.

PARTNER \<partner_option> steuert die Datenbankeigenschaften, die die Failoverpartner einer Datenbankspiegelungssitzung und deren Verhalten definieren. Einige Optionen von SET PARTNER können auf einem beliebigen der Partner festgelegt werden, andere sind auf den Prinzipal- oder den Spiegelserver beschränkt. Weitere Informationen finden Sie unter den jeweiligen Optionen von PARTNER weiter unten. Eine SET PARTNER-Klausel wirkt sich auf beide Kopien der Datenbank aus, unabhängig davon, für welchen Partner sie angegeben ist.

Zum Ausführen der SET PARTNER-Anweisung muss die Option STATE der Endpunkte beider Partner auf STARTED festgelegt sein. Außerdem muss ROLE für den Datenbank-Spiegelungsendpunkt jeder Partnerserverinstanz auf PARTNER oder ALL festgelegt sein. Weitere Informationen zum Angeben eines Endpunkts finden Sie unter [Erstellen eines Endpunkts der Datenbankspiegelung für Windows-Authentifizierung](../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md). Verwenden Sie folgende [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisung, um die Rolle und den Status des Datenbank-Spiegelungsendpunkts einer Serverinstanz zu erfahren:

```sql
SELECT role_desc, state_desc FROM sys.database_mirroring_endpoints
```

**\<partner_option> ::=**

> [!NOTE]
> Pro SET PARTNER-Klausel wird nur eine \<partner_option>-Option zugelassen.

**'** _partner_server_ **'** Gibt die Netzwerkadresse des Servers einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz an, die als Failoverpartner in einer neuen Datenbankspiegelungs-Sitzung fungiert. Jede Sitzung erfordert zwei Partner, von denen einer als Prinzipalserver, der andere als Spiegelserver beginnt. Die beiden Partner sollten sich auf unterschiedlichen Computern befinden.

Diese Option wird einmal pro Sitzung für jeden Partner angegeben. Für das Starten einer Datenbank-Spiegelungssitzung sind zwei ALTER DATABASE *Datenbank* SET PARTNER **='** _partner_server_ **'** -Anweisungen erforderlich. Ihre Reihenfolge ist wichtig. Stellen Sie zuerst eine Verbindung mit dem Spiegelserver her, und geben Sie die Prinzipalserverinstanz als *partner_server* (SET PARTNER **='** _principal_server_ **'** ) an. Stellen Sie anschließend eine Verbindung mit dem Prinzipalserver her, und geben Sie die Spiegelserverinstanz als *partner_server* (SET PARTNER **='** _mirror_server_ **'** ) an; auf diese Weise wird eine Datenbank-Spiegelungssitzung zwischen diesen beiden Partnern gestartet. Weitere Informationen finden Sie weiter unten in diesem Thema unter [Einrichten der Datenbankspiegelung](../../database-engine/database-mirroring/setting-up-database-mirroring-sql-server.md).

Der Wert von *partner_server* ist eine Server-Netzwerkadresse. Diese hat folgende Syntax:

TCP **://** _\<system-address>_ **:** _\<port>_

Hierbei gilt:

- *\<system-address>* ist eine Zeichenfolge, beispielsweise ein Systemname, ein vollqualifizierter Domänenname oder eine IP-Adresse, die das Zielcomputersystem eindeutig identifiziert.
- *\<port>* ist eine Portnummer, die dem Spiegelungsendpunkt der Partnerserverinstanz zugeordnet ist.

Weitere Informationen finden Sie unter [Angeben einer Servernetzwerkadresse – Datenbankspiegelung](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md).

Das folgende Beispiel veranschaulicht die SET PARTNER **='** _partner_server_ **'** -Klausel:

```
'TCP://MYSERVER.mydomain.Adventure-Works.com:7777'
```

> [!IMPORTANT]
> Wird eine Sitzung mithilfe der ALTER DATABASE-Anweisung statt mit [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] eingerichtet, wird die Sitzung standardmäßig auf vollständige Transaktionssicherheit festgelegt (SAFETY wird auf FULL festgelegt) und im Modus für hohe Sicherheit ohne automatisches Failover ausgeführt. Konfigurieren Sie einen Zeugen, um automatisches Failover zuzulassen. Für die Ausführung im Modus für hohe Leistung deaktivieren Sie die Transaktionssicherheit (SAFETY OFF).

FAILOVER Führt ein manuelles Failover vom Prinzipalserver zum Spiegelserver aus. Sie können FAILOVER nur auf dem Prinzipalserver angeben. Diese Option ist nur dann gültig, wenn die Einstellung SAFETY auf FULL (Standard) festgelegt ist.

Die Option FAILOVER erfordert **master** als Datenbankkontext.

FORCE_SERVICE_ALLOW_DATA_LOSS Erzwingt ein Failover des Datenbankdiensts auf die Spiegeldatenbank, wenn auf dem Prinzipalserver ein Fehler auftritt, wobei sich die Datenbank im nicht synchronisierten Zustand oder im synchronisierten Zustand ohne automatisches Failover befindet.

Es wird dringend empfohlen, den Dienst nur dann zu erzwingen, wenn der Prinzipalserver nicht mehr ausgeführt wird. Ansonsten können einige Clients weiter versuchen, auf die ursprüngliche Prinzipaldatenbank zuzugreifen statt auf die neue Prinzipaldatenbank.
FORCE_SERVICE_ALLOW_DATA_LOSS ist nur auf dem Spiegelserver verfügbar und nur unter allen folgenden Bedingungen:

- Der Prinzipalserver ist ausgefallen.
- WITNESS ist auf OFF festgelegt, oder der Zeuge ist mit dem Spiegelserver verbunden.

Sie sollten den Dienst nur erzwingen, wenn Sie bereit sind, Datenverluste in Kauf zu nehmen, um den Dienst für die Datenbank unverzüglich wiederherzustellen.

Durch das Erzwingen des Diensts wird die Sitzung ausgesetzt, und alle Daten werden vorübergehend in der ursprünglichen Prinzipaldatenbank beibehalten. Sobald der ursprüngliche Prinzipalserver in Betrieb und zur Kommunikation mit dem neuen Prinzipalserver in der Lage ist, kann der Datenbankadministrator den Dienst fortsetzen. Wenn die Sitzung fortgesetzt wird, gehen alle nicht gesendeten Protokolldatensätze sowie die entsprechenden Updates verloren.

OFF Entfernt eine Datenbank-Spiegelungssitzung und entfernt die Spiegelung von der Datenbank. Sie können OFF auf beiden Partnern festlegen. Weitere Informationen zu den Auswirkungen des Entfernens der Datenbankspiegelung finden Sie unter [Entfernen der Datenbankspiegelung](../../database-engine/database-mirroring/removing-database-mirroring-sql-server.md).

RESUME Setzt eine ausgesetzte Datenbank-Spiegelungssitzung fort. Sie können RESUME nur auf dem Hauptserver angeben.

SAFETY { FULL | OFF } Legt die Sicherheitsstufe für Transaktionen fest. Sie können SAFETY nur auf dem Hauptserver angeben.

Der Standardwert ist FULL. Bei vollständiger Sicherheit (FULL) wird die Datenbank-Spiegelungssitzung synchron (im *Modus für hohe Sicherheit*) ausgeführt. Ist SAFETY auf OFF festgelegt, wird die Datenbank-Spiegelungssitzung asynchron (im *Modus für hohe Leistung*) ausgeführt.

Das Verhalten des Modus für hohe Sicherheit hängt teilweise folgendermaßen vom Zeugen ab:

- Wenn die Sicherheit auf FULL und ein Zeuge für die Sitzung festgelegt wurde, wird die Sitzung im Modus für hohe Sicherheit mit automatischem Failover ausgeführt. Wenn der Prinzipalserver ausfällt, findet automatisch ein Failover der Sitzung statt, sofern die Datenbank synchronisiert ist und die Spiegelserverinstanz und der Zeuge noch miteinander verbunden sind (das heißt, sie verfügen über ein Quorum). Weitere Informationen finden Sie unter [Quorum: Auswirkungen eines Zeugen auf die Datenbankverfügbarkeit – Datenbankspiegelung](../../database-engine/database-mirroring/quorum-how-a-witness-affects-database-availability-database-mirroring.md).

  Wenn ein Zeuge für die Sitzung festgelegt wurde, der zurzeit nicht verbunden ist, bewirkt der Verlust des Spiegelservers, dass der Prinzipalserver ausfällt

- Wenn die Sicherheit auf FULL und der Zeuge auf OFF festgelegt wurde, wird die Sitzung im Modus für hohe Sicherheit ohne automatisches Failover ausgeführt. Wenn die Spiegelserverinstanz ausfällt, hat dies keine Auswirkungen auf die Prinzipalserverinstanz. Wenn der Prinzipalserver ausfällt, können Sie den Dienst (mit möglichem Datenverlust) auf dem Spiegelserver erzwingen.

- Ist SAFETY auf OFF festgelegt, wird die Sitzung im Modus für hohe Leistung ausgeführt, und weder automatisches noch manuelles Failover werden unterstützt. Probleme auf dem Spiegelserver wirken sich jedoch nicht auf den Prinzipalserver aus. Wenn die Prinzipalserverinstanz ausfällt, können Sie ggf. das Failover des Diensts (mit möglichem Datenverlust) auf die Spiegelserverinstanz erzwingen – wenn WITNESS auf OFF festgelegt wurde oder der Zeuge aktuell mit dem Spiegelserver verbunden ist. Weitere Informationen zum Erzwingen des Diensts finden Sie unter FORCE_SERVICE_ALLOW_DATA_LOSS weiter oben in diesem Abschnitt.

> [!IMPORTANT]
> Der Modus für hohe Leistung ist nicht für die Verwendung eines Zeugen konzipiert. Beim Festlegen von SAFETY auf OFF sollte unbedingt sichergestellt werden, dass WITNESS ebenfalls auf OFF festgelegt ist.

SUSPEND Hält eine Datenbank-Spiegelungssitzung an.

Sie können SUSPEND auf beiden Partnern angeben.

TIMEOUT *ganze Zahl* Gibt das Zeitlimit in Sekunden an. Der Timeoutzeitraum ist die Zeit, die eine Serverinstanz maximal auf den Empfang einer PING-Nachricht von einer anderen Instanz in der Spiegelungssitzung wartet, bevor davon ausgegangen wird, dass die Verbindung der anderen Instanz getrennt wurde.

Sie können die Option TIMEOUT nur auf dem Prinzipalserver angeben. Wenn Sie die Option nicht angeben, beträgt der Timeoutzeitraum standardmäßig 10 Sekunden. Wenn Sie 5 oder höher angeben, wird der Timeoutzeitraum auf die angegebene Anzahl von Sekunden festgelegt. Wenn Sie einen Timeoutzeitwert von 0 bis 4 Sekunden angeben, wird der Timeoutzeitraum automatisch auf 5 Sekunden festgelegt.

> [!IMPORTANT]
> Es wird empfohlen, einen Timeoutzeitraum von 10 Sekunden oder mehr zu wählen. Wenn Sie diesen Wert auf weniger als 10 Sekunden festlegen, verpasst ein stark ausgelastetes System möglicherweise PINGs und meldet einen falschen Fehler.

Weitere Informationen finden Sie unter [Possible Failures During Database Mirroring](../../database-engine/database-mirroring/possible-failures-during-database-mirroring.md).

WITNESS \<witness_option> steuert die Datenbankeigenschaften, die einen Datenbankspiegelungszeugen definieren. Eine SET WITNESS-Klausel wirkt sich auf beide Kopien der Datenbank aus, Sie können SET WITNESS jedoch nur auf dem Prinzipalserver angeben. Wenn ein Zeuge für eine Sitzung festgelegt wird, ist unabhängig von der SAFETY-Einstellung ein Quorum zum Anbieten der Datenbank erforderlich; weitere Informationen finden Sie unter [Quorum: Auswirkungen eines Zeugen auf die Datenbankverfügbarkeit – Datenbankspiegelung](../../database-engine/database-mirroring/quorum-how-a-witness-affects-database-availability-database-mirroring.md).

Zeuge und Failoverpartner sollten sich auf separaten Computern befinden. Informationen zum Hinzufügen oder Entfernen eines Zeugen finden Sie unter [Datenbank-Spiegelungszeuge](../../database-engine/database-mirroring/database-mirroring-witness.md).

Für die Endpunkte von Prinzipal- und Zeugenserverinstanz muss STATE auf STARTED festgelegt werden, um eine SET WITNESS-Anweisung auszuführen. Außerdem muss ROLE für den Datenbank-Spiegelungsendpunkt einer Zeugenserverinstanz auf WITNESS oder ALL festgelegt sein. Informationen zum Angeben eines Endpunkts finden Sie unter [der Datenbankspiegelungs-Endpunkt](../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md).

Verwenden Sie folgende [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisung, um die Rolle und den Status des Datenbank-Spiegelungsendpunkts einer Serverinstanz zu erfahren:

```sql
SELECT role_desc, state_desc FROM sys.database_mirroring_endpoints
```

> [!NOTE]
> Auf dem Zeugen können keine Datenbankeigenschaften festgelegt werden.

 **\<witness_option> ::=**

> [!NOTE]
> Pro SET WITNESS-Klausel wird nur eine \<witness_option>-Option zugelassen.

 **'** _witness_server_ **'** Gibt eine [!INCLUDE[ssDE](../../includes/ssde-md.md)]-Instanz an, die als Zeugenserver für die Datenbankspiegelungs-Sitzung agiert. Sie können SET WITNESS-Anweisungen nur auf dem Prinzipalserver angeben.

In einer SET WITNESS **='** _witness_server_ **'** -Anweisung ist die Syntax von *witness_server* mit der Syntax von *partner_server* identisch.

OFF Entfernt den Zeugen aus einer Datenbank-Spiegelungssitzung. Durch das Festlegen des Zeugen auf OFF wird das automatische Failover deaktiviert. Ist die Datenbank auf FULL SAFETY festgelegt und der Zeuge auf OFF, führt ein Fehler auf dem Spiegelserver dazu, dass der Prinzipalserver die Datenbank nicht verfügbar macht.

## <a name="remarks"></a>Bemerkungen

## <a name="examples"></a>Beispiele

### <a name="a-creating-a-database-mirroring-session-with-a-witness"></a>A. Erstellen einer Datenbank-Spiegelungssitzung mit einem Zeugen

Das Einrichten einer Datenbankspiegelung mit einem Zeugen erfordert das Konfigurieren von Sicherheit und Vorbereiten der Spiegeldatenbank sowie das Verwenden von ALTER DATABASE zum Festlegen der Partner. Ein Beispiel des vollständigen Setupprozesses finden Sie unter [Einrichten der Datenbankspiegelung](../../database-engine/database-mirroring/setting-up-database-mirroring-sql-server.md).

### <a name="b-manually-failing-over-a-database-mirroring-session"></a>B. Manuelles Ausführen eines Failovers für eine Datenbank-Spiegelungssitzung

Ein manuelles Failover kann von beiden Datenbank-Spiegelungspartnern initiiert werden. Vor dem Failover sollten Sie sicherstellen, dass es sich bei dem aktuellen Prinzipalserver auch tatsächlich um den Prinzipalserver handelt. Führen Sie z. B. für die [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]-Datenbank auf der Serverinstanz, die Sie für den aktuellen Prinzipalserver halten, die folgende Abfrage aus:

```sql
SELECT db.name, m.mirroring_role_desc
FROM sys.database_mirroring m
JOIN sys.databases db
ON db.database_id = m.database_id
WHERE db.name = N'AdventureWorks2012';
GO
```

Handelt es sich bei der Serverinstanz tatsächlich um den Prinzipalserver, hat `mirroring_role_desc` den Wert `Principal`. Handelt es sich bei der Serverinstanz aber um den Spiegelserver, gibt die `SELECT`-Anweisung `Mirror` zurück.

Im folgenden Beispiel wird vorausgesetzt, dass es sich bei dem Server um den aktuellen Prinzipalserver handelt.

1. Manuelles Failover an den Datenbank-Spiegelungspartner:

    ```sql
    ALTER DATABASE AdventureWorks2012 SET PARTNER FAILOVER;
    GO
    ```
  
2. Führen Sie die folgende Abfrage aus, um die Ergebnisse des Failovers auf dem neuen Spiegelserver zu überprüfen:
  
    ```sql
    SELECT db.name, m.mirroring_role_desc
    FROM sys.database_mirroring m
    JOIN sys.databases db
    ON db.database_id = m.database_id
    WHERE db.name = N'AdventureWorks2012';
    GO
    ```
  Der aktuelle Wert von `mirroring_role_desc` ist jetzt `Mirror`.

## <a name="see-also"></a>Weitere Informationen

- [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md)
- [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)
- [sys.database_mirroring_witnesses](../../relational-databases/system-catalog-views/database-mirroring-witness-catalog-views-sys-database-mirroring-witnesses.md)
