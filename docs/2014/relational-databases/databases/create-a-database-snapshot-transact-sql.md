---
title: Erstellen einer Datenbankmomentaufnahme (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/13/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.technology:
- database-engine
ms.topic: conceptual
helpviewer_keywords:
- database snapshots [SQL Server], creating
ms.assetid: 187fbba3-c555-4030-9bdf-0f01994c5230
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 7bb53467361cec415b95f2fe3477f3b0730f33b8
ms.sourcegitcommit: 3da2edf82763852cff6772a1a282ace3034b4936
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/02/2018
ms.locfileid: "48212200"
---
# <a name="create-a-database-snapshot-transact-sql"></a>Erstellen einer Datenbankmomentaufnahme (Transact-SQL)
  Eine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Datenbankmomentaufnahme kann nur mithilfe von [!INCLUDE[tsql](../../includes/tsql-md.md)]erstellt werden. [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] nicht unterstützt.  
  
-   **Vorbereitungen:**  
  
     [Erforderliche Komponenten](#Prerequisites)  
  
     [Security](#Security)  
  
     [Bewährte Methode: Benennen von Datenbankmomentaufnahmen](#Naming)  
  
-   **So erstellen Sie eine Datenbank-Momentaufnahme mit:**  [Transact-SQL](#TsqlProcedure)  
  
##  <a name="BeforeYouBegin"></a> Vorbereitungsmaßnahmen  
  
###  <a name="Prerequisites"></a> Erforderliche Komponenten  
 Die Quelldatenbank, die ein Wiederherstellungsmodell verwenden kann, muss die folgenden Voraussetzungen erfüllen:  
  
-   Die Serverinstanz muss eine Edition von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ausführen, die Datenbankmomentaufnahmen unterstützt. Informationen zur Unterstützung von datenbankmomentaufnahmen in [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], finden Sie unter [von den SQL Server 2014-Editionen unterstützte Funktionen](../../getting-started/features-supported-by-the-editions-of-sql-server-2014.md).  
  
-   Die Quelldatenbank muss online sein, es sei denn, bei der Datenbank handelt es sich um eine Spiegeldatenbank innerhalb einer Datenbank-Spiegelungssitzung.  
  
-   Zum Erstellen einer Datenbank-Momentaufnahme für die Spiegeldatenbank muss sich die Datenbank im synchronisierten[Spiegelungsstatus](../../database-engine/database-mirroring/mirroring-states-sql-server.md).  
  
-   Die Quelldatenbank kann nicht als skalierbare freigegebene Datenbank konfiguriert werden.  
  
> [!IMPORTANT]  
>  Informationen zu anderen bedeutenden Überlegungen finden Sie unter [Datenbank-Momentaufnahmen &#40;SQL Server&#41;](database-snapshots-sql-server.md)erstellt werden.  
  
###  <a name="Recommendations"></a> Empfehlungen  
 In diesem Abschnitt werden die folgenden bewährten Methoden erläutert:  
  
-   [Bewährte Methode: Benennen von Datenbankmomentaufnahmen](#Naming)  
  
-   [Bewährte Methode: Beschränken der Anzahl von Datenbankmomentaufnahmen](#Limiting_Number)  
  
-   [Bewährte Methode: Clientverbindungen mit einer Datenbankmomentaufnahme](#Client_Connections)  
  
####  <a name="Naming"></a> Bewährte Methode: Benennen von Datenbankmomentaufnahmen  
 Vor dem Erstellen von Momentaufnahmen müssen Sie unbedingt überlegen, wie Sie diese benennen. Jede Datenbankmomentaufnahme erfordert einen eindeutigen Datenbanknamen. Um den Verwaltungsaufwand zu reduzieren, kann der Name einer Momentaufnahme Informationen enthalten, mit denen die Datenbank identifiziert wird:  
  
-   Der Name der Quelldatenbank.  
  
-   Einen Hinweis, dass der neue Name für eine Momentaufnahme ist  
  
-   Das Erstellungsdatum und die Erstellungszeit der Momentaufnahme, eine Sequenznummer oder sonstige Informationen, wie z. B. die Tageszeit, um sequenzielle Momentaufnahmen in einer bestimmten Datenbank zu unterscheiden  
  
 Angenommen, Sie haben eine Reihe von Momentaufnahmen für die [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] -Datenbank. Täglich werden drei Momentaufnahmen im Abstand von sechs Stunden zwischen 6:00 Uhr und 18:00 Uhr erstellt (24-Stunden-System). Jede tägliche Momentaufnahme wird nach 24 Stunden gelöscht und durch eine neue gleichnamige Momentaufnahme ersetzt. Beachten Sie, dass jeder Momentaufnahmename einen Hinweis auf die Uhrzeit, aber nicht auf den Tag enthält:  
  
```  
AdventureWorks_snapshot_0600  
AdventureWorks_snapshot_1200  
AdventureWorks_snapshot_1800  
```  
  
 Falls alternativ die Erstellungszeit dieser täglichen Momentaufnahmen von Tag zu Tag variiert, ist möglicherweise eine weniger präzise Benennungskonvention vorzuziehen, wie beispielsweise:  
  
```  
AdventureWorks_snapshot_morning  
AdventureWorks_snapshot_noon  
AdventureWorks_snapshot_evening  
```  
  
####  <a name="Limiting_Number"></a> Bewährte Methode: Beschränken der Anzahl von Datenbankmomentaufnahmen  
 Durch das Erstellen einer Reihe von Momentaufnahmen werden im Laufe der Zeit sequenzielle Momentaufnahmen der Quelldatenbank aufgezeichnet. Jede Momentaufnahme wird so lange persistent gespeichert, bis sie explizit gelöscht wird. Durch jede Momentaufnahme nehmen die ursprünglichen Seiten beim Aktualisieren an Größe zu. Deshalb sollten Sie Speicherplatz freigeben, indem Sie eine ältere Momentaufnahme löschen, nachdem eine neue Momentaufnahme erstellt wurde.  
  
> [!NOTE]  
>  Wenn Sie eine Datenbankmomentaufnahme wiederherstellen möchten, müssen Sie andere Momentaufnahmen in dieser Datenbank löschen.  
  
####  <a name="Client_Connections"></a> Bewährte Methode: Clientverbindungen mit einer Datenbankmomentaufnahme  
 Zur Verwendung einer Datenbankmomentaufnahme müssen die Clients wissen, wo sie diese finden. Die Benutzer können aus einer Datenbankmomentaufnahme lesen, während eine andere Datenbankmomentaufnahme erstellt oder gelöscht wird. Wenn Sie jedoch eine vorhandenen Momentaufnahme durch eine neue Momentaufnahme ersetzen, müssen Sie Clients an die neue Momentaufnahme umleiten. Die Benutzer können mithilfe von [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]manuell eine Verbindung mit einer Datenbankmomentaufnahme herstellen. Für die Unterstützung einer Produktionsumgebung sollten Sie jedoch eine programmatische Lösung erstellen, die Berichterstellungsclients transparent an die neueste Momentaufnahme der Datenbank weiterleitet.  
  
###  <a name="Security"></a> Sicherheit  
  
####  <a name="Permissions"></a> Berechtigungen  
 Jeder Benutzer, der eine Datenbank erstellen kann, kann auch eine Datenbankmomentaufnahme erstellen. Eine Momentaufnahme einer Spiegeldatenbank kann jedoch nur von Mitgliedern der festen Serverrolle **sysadmin** erstellt werden.  
  
##  <a name="TsqlProcedure"></a> So erstellen Sie eine Datenbankmomentaufnahme (mit Transact-SQL)  
 **So erstellen Sie eine Datenbankmomentaufnahme**  
  
> [!NOTE]  
>  Ein Beispiel für diese Prozedur finden Sie in [Beispiele (Transact-SQL)](#TsqlExample)an späterer Stelle in diesem Abschnitt.  
  
1.  Prüfen Sie die aktuelle Größe der Quelldatenbank, um sicherzustellen, dass der verfügbare Festplattenspeicher zum Speichern der Datenbankmomentaufnahme ausreicht. Die maximale Größe einer Datenbankmomentaufnahme beläuft sich auf die Größe der Quelldatenbank zum Zeitpunkt der Momentaufnahmeerstellung. Weitere Informationen finden Sie unter [Anzeigen der Größe der Datei mit geringer Dichte einer Datenbank-Momentaufnahme &#40;Transact-SQL&#41;](view-the-size-of-the-sparse-file-of-a-database-snapshot-transact-sql.md).  
  
2.  Geben Sie eine CREATE DATABASE-Anweisung für die Dateien aus, und verwenden Sie dabei die AS SNAPSHOT OF-Klausel. Bei der Erstellung einer Momentaufnahme müssen die logischen Namen aller in der Quelldatenbank enthaltenen Datenbankdateien angegeben werden. Die Syntax lautet wie folgt:  
  
     CREATE DATABASE *Name der Datenbank-Momentaufnahme*  
  
     ON  
  
     (  
  
     NAME =*logischer Dateiname*,  
  
     FILENAME = '*physischer Dateiname*'  
  
     ) [ ,...*n* ]  
  
     ALS SNAPSHOT OF *Name der Quelldatenbank*  
  
     [;]  
  
     Dabei ist der *Quelle_**Datenbankname* die Quelldatenbank, der *logische_Dateiname* ist der in SQL Server beim Verweis auf die Datei verwendete logische Name, der *Betriebssystem_Dateiname* ist der vom Betriebssystem beim Erstellen der Datei verwendete Pfad- und Dateiname, und der *Name_der_Datenbank_Momentaufnahme* ist der Name der Momentaufnahme, aus der die Datenbank wiederhergestellt werden soll. Eine vollständige Beschreibung dieser Syntax finden Sie unter [CREATE DATABASE &#40;SQL Server Transact-SQL&#41;](/sql/t-sql/statements/create-database-sql-server-transact-sql).  
  
    > [!NOTE]  
    >  Wenn Sie eine Datenbankmomentaufnahme erstellen, darf die CREATE DATABASE-Anweisung weder Protokolldateien noch Offlinedateien, Wiederherstellungsdateien oder außer Kraft gesetzte Dateien enthalten.  
  
###  <a name="TsqlExample"></a> Beispiele (Transact-SQL)  
  
> [!NOTE]  
>  Die in den Beispielen verwendete Erweiterung `.ss` ist willkürlich.  
  
 Dieser Abschnitt enthält die folgenden Beispiele:  
  
-   A. [Erstellen einer Momentaufnahme für die AdventureWorks-Datenbank](#Creating_on_AW)  
  
-   B. [Erstellen einer Momentaufnahme für die Sales-Datenbank](#Creating_on_Sales)  
  
####  <a name="Creating_on_AW"></a> A. Erstellen einer Momentaufnahme für die AdventureWorks-Datenbank  
 In diesem Beispiel wird eine Datenbankmomentaufnahme für die `AdventureWorks` -Datenbank erstellt. Der Momentaufnahmename, `AdventureWorks_dbss_1800`, und der Dateiname der entsprechenden Sparsedatei, `AdventureWorks_data_1800.ss`, geben als Erstellungszeit 18:00 Uhr an.  
  
```  
CREATE DATABASE AdventureWorks_dbss1800 ON  
( NAME = AdventureWorks_Data, FILENAME =   
'C:\Program Files\Microsoft SQL Server\MSSQL12.MSSQLSERVER\MSSQL\Data\AdventureWorks_data_1800.ss' )  
AS SNAPSHOT OF AdventureWorks;  
GO  
```  
  
####  <a name="Creating_on_Sales"></a> B. Erstellen einer Momentaufnahme für die Sales-Datenbank  
 In diesem Beispiel wird eine Datenbankmomentaufnahme, `sales_snapshot1200`, für die `Sales` -Datenbank erstellt. Diese Datenbank wurde im Beispiel "Erstellen einer Datenbank mit Dateigruppen" erstellt, [CREATE DATABASE &#40;SQL Server Transact-SQL&#41;](/sql/t-sql/statements/create-database-sql-server-transact-sql).  
  
```  
--Creating sales_snapshot1200 as snapshot of the  
--Sales database:  
CREATE DATABASE sales_snapshot1200 ON  
( NAME = SPri1_dat, FILENAME =   
'C:\Program Files\Microsoft SQL Server\MSSQL12.MSSQLSERVER\MSSQL\data\SPri1dat_1200.ss'),  
( NAME = SPri2_dat, FILENAME =   
'C:\Program Files\Microsoft SQL Server\MSSQL12.MSSQLSERVER\MSSQL\data\SPri2dt_1200.ss'),  
( NAME = SGrp1Fi1_dat, FILENAME =   
'C:\Program Files\Microsoft SQL Server\MSSQL12.MSSQLSERVER\mssql\data\SG1Fi1dt_1200.ss'),  
( NAME = SGrp1Fi2_dat, FILENAME =   
'C:\Program Files\Microsoft SQL Server\MSSQL12.MSSQLSERVER\MSSQL\data\SG1Fi2dt_1200.ss'),  
( NAME = SGrp2Fi1_dat, FILENAME =   
'C:\Program Files\Microsoft SQL Server\MSSQL12.MSSQLSERVER\MSSQL\data\SG2Fi1dt_1200.ss'),  
( NAME = SGrp2Fi2_dat, FILENAME =   
'C:\Program Files\Microsoft SQL Server\MSSQL12.MSSQLSERVER\MSSQL\data\SG2Fi2dt_1200.ss')  
AS SNAPSHOT OF Sales;  
GO  
```  
  
##  <a name="RelatedTasks"></a> Verwandte Aufgaben  
  
-   [Anzeigen einer Datenbank-Momentaufnahme &#40;SQL Server&#41;](view-a-database-snapshot-sql-server.md)  
  
-   [Wiederherstellen einer Datenbank zu einer Datenbank-Momentaufnahme](revert-a-database-to-a-database-snapshot.md)  
  
-   [Löschen einer Datenbankmomentaufnahme &#40;Transact-SQL&#41;](drop-a-database-snapshot-transact-sql.md)  
  
## <a name="see-also"></a>Siehe auch  
 [CREATE DATABASE &#40;SQL Server Transact-SQL&#41;](/sql/t-sql/statements/create-database-sql-server-transact-sql)   
 [Datenbank-Momentaufnahmen &#40;SQL Server&#41;](database-snapshots-sql-server.md)  
  
  
