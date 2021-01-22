---
description: DBCC SHRINKFILE (Transact-SQL)
title: DBCC SHRINKFILE (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 11/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- SHRINKFILE
- DBCC_SHRINKFILE_TSQL
- DBCC SHRINKFILE
- SHRINKFILE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- data shrinking [SQL Server]
- TRUNCATEONLY option
- shrinking files
- NOTRUNCATE option
- shrinking databases
- decreasing database size
- file shrinking [SQL Server]
- database shrinking [SQL Server]
- logs [SQL Server], shrinking
- reducing database size
- DBCC SHRINKFILE statement
ms.assetid: e02b2318-bee9-4d84-a61f-2fddcf268c9f
author: pmasl
ms.author: umajay
ms.openlocfilehash: 478c7c784c25b1b71bcafd2279ac93bc7545c188
ms.sourcegitcommit: e40e75055c1435c5e3f9b6e3246be55526807b4c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/13/2021
ms.locfileid: "98151310"
---
# <a name="dbcc-shrinkfile-transact-sql"></a>DBCC SHRINKFILE (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Reduziert die Größe der angegebenen Daten oder Protokolldateien der aktuellen Datenbank. Damit können Sie Daten von einer Datei in andere Dateien derselben Dateigruppe verschieben. So wird die Datei geleert und die Entfernung der Datenbank ermöglicht. Sie können eine Datei auf weniger als die Größe bei der Erstellung verkleinern, um die minimale Dateigröße auf den neuen Wert zurücksetzen.
  
![Artikellinksymbol](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
DBCC SHRINKFILE   
(  
    { file_name | file_id }   
    { [ , EMPTYFILE ]   
    | [ [ , target_size ] [ , { NOTRUNCATE | TRUNCATEONLY } ] ]  
    }  
)  
[ WITH NO_INFOMSGS ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
*file_name*  
Der logische Name der Datei, die verkleinert werden soll.
  
*file_id*  
Die Datei-ID der Datei, die verkleinert werden soll. Verwenden Sie zum Ermitteln einer Datei-ID die [FILE_IDEX](../../t-sql/functions/file-idex-transact-sql.md)-Systemfunktion, oder fragen Sie die [sys.database_files](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)-Katalogsicht in der aktuellen Datenbank ab.
  
*target_size*  
Ein Integer – die Größe der neuen Datei in Megabyte. Wenn nichts angegeben wird oder der Wert 0 ist, wird die Größe von DBCC SHRINKFILE auf die Dateigröße bei der Erstellung reduziert.
  
> [!NOTE]  
>  Sie können die Standardgröße einer leeren Datei mit DBCC SHRINKFILE *target_size* verringern. Wenn Sie z. B. eine 5 MB große Datei erstellen und die Dateigröße dann auf 3 MB herabsetzen, während die Datei noch leer ist, wird die Standarddateigröße auf 3 MB festgelegt. Dies gilt nur für leere Dateien, die nie Daten enthalten haben.  
  
Diese Option wird in FILESTREAM-Dateigruppencontainern nicht unterstützt.  
Wenn angegeben, versucht DBCC SHRINKFILE, die Datei auf *target_size* zu verkleinern. Verwendete Seiten im Bereich der freizugebenden Datei werden auf freien Speicherplatz in den beibehalten Bereichen der Datei verschoben. Beispielsweise verschiebt ein DBCC SHRINKFILE-Vorgang mit einer 8 *target_size* bei einer 10-MB-Datendatei alle verwendeten Seiten in den letzten 2 MB der Datei in alle nicht zugeordneten Seiten in den ersten 8 MB der Datei. DBCC SHRINKFILE verkleinert eine Datei nicht über die erforderliche Speichergröße hinaus. Werden beispielsweise 7 MB einer 10 MB großen Datendatei verwendet, verkleinert eine DBCC SHRINKFILE-Anweisung mit einem *target_size*-Wert von 6 die Datei lediglich auf 7 MB statt auf 6 MB.
  
EMPTYFILE  
Verlagert alle Daten aus der angegebenen Datei in andere Dateien in **derselben Dateigruppe**. EMPTYFILE migriert die Daten also aus der angegebenen Datei zu anderen Dateien in derselben Dateigruppe. EMPTYFILE stellt sicher, dass keine neuen Daten zur Datei hinzugefügt werden, obwohl diese Datei nicht schreibgeschützt ist. Sie könne eine [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md)-Anweisung verwenden, um eine Datei zu entfernen. Wenn zum Ändern der Dateigröße die [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md)-Anweisung verwenden, wird das Flag „schreibgeschützt“ zurückgesetzt, und Daten können hinzugefügt werden.

Die Datei kann aus FILESTREAM-Dateigruppencontainern erst dann mit ALTER DATABASE entfernt werden, nachdem der FILESTREAM-Garbage Collector ausgeführt und alle nicht benötigten Dateigruppen-Containerdateien gelöscht wurden, die von EMPTYFILE in einen anderen Container kopiert wurden. Weitere Informationen finden Sie unter [sp_filestream_force_garbage_collection &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/filestream-and-filetable-sp-filestream-force-garbage-collection.md).
  
> [!NOTE]  
>  Informationen zum Entfernen eines FILESTREAM-Containers finden Sie im entsprechenden Abschnitt [ALTER DATABASE-Optionen für Dateien und Dateigruppen &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md).  
  
NOTRUNCATE  
Verschiebt zugeordnete Seiten vom Ende einer Datendatei in nicht zugeordnete Seiten am Dateianfang, und zwar mit oder ohne Angabe von *target_percent*. Der freie Speicherplatz am Dateiende wird nicht an das Betriebssystem zurückgegeben, und die physische Größe der Datei bleibt unverändert. Daher scheint die Datei bei Angabe von NOTRUNCATE nicht verkleinert zu werden.
NOTRUNCATE gilt nur für Datendateien. Die Protokolldateien sind nicht betroffen.   Diese Option wird in FILESTREAM-Dateigruppencontainern nicht unterstützt.
  
TRUNCATEONLY  
Gibt den gesamten freien Speicherplatz am Dateiende an das Betriebssystem frei, es werden jedoch keine Seiten innerhalb der Datei verschoben. Die Datendatei wird nur bis zum letzten zugeordneten Block verkleinert.
Der Parameter *target_size* wird ignoriert, wenn er mit TRUNCATEONLY angegeben wird.  
Die Option TRUNCATEONLY verschiebt Informationen nicht in das Protokoll, entfernt jedoch inaktive VLFs am Ende der Protokolldatei. Diese Option wird in FILESTREAM-Dateigruppencontainern nicht unterstützt.
  
WITH NO_INFOMSGS  
Alle Informationsmeldungen werden unterdrückt.
  
## <a name="result-sets"></a>Resultsets  
In der folgenden Tabelle sind die Resultsetspalten beschrieben.
  
|Spaltenname|BESCHREIBUNG|  
|---|---|
|**DbId**|Die Datenbank-ID der Datei, die das [!INCLUDE[ssDE](../../includes/ssde-md.md)] zu verkleinern versuchte.|  
|**FileId**|Die Datei-ID der Datei, die [!INCLUDE[ssDE](../../includes/ssde-md.md)] zu verkleinern versuchte.|  
|**CurrentSize**|Die Anzahl von 8-KB-Seiten, die die Datei derzeit belegt.|  
|**MinimumSize**|Die Anzahl von 8-KB-Seiten, die die Datei minimal belegen könnte. Diese Zahl entspricht der Mindestgröße bzw. der ursprünglich erzeugten Dateigröße.|  
|**UsedPages**|Die Anzahl von 8-KB-Seiten, die derzeit von der Datei verwendet werden.|  
|**EstimatedPages**|Die Anzahl an 8-KB-Seiten, auf die die Datei wahrscheinlich vom [!INCLUDE[ssDE](../../includes/ssde-md.md)] verkleinert werden kann.|  
  
## <a name="remarks"></a>Hinweise  
DBCC SHRINKFILE gilt für die Dateien der aktuellen Datenbank. Weitere Informationen zum Ändern der aktuellen Datenbank finden Sie unter [USE &#40;Transact-SQL&#41;](../../t-sql/language-elements/use-transact-sql.md).
  
Sie können den DBCC SHRINKFILE-Vorgang jederzeit beenden, und alle abgeschlossenen Vorgänge bleiben erhalten. Wenn Sie den Parameter EMPTYFILE verwenden und den Vorgang abbrechen, wird die Datei nicht markiert, um zu verhindern, dass zusätzliche Daten hinzugefügt werden.
  
Wenn ein DBCC SHRINKFILE-Vorgang fehlschlägt, wird ein Fehler ausgelöst.
  
 Andere Benutzer können während der Dateiverkleinerung in der Datenbank arbeiten – die Datenbank muss nicht im Einzelbenutzermodus sein. Um die Systemdatenbanken zu verkleinern, muss auch [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nicht im Einzelbenutzermodus ausgeführt werden.  
  
## <a name="shrinking-a-log-file"></a>Verkleinern einer Protokolldatei  

Für Protokolldateien verwendet [!INCLUDE[ssDE](../../includes/ssde-md.md)]*target_size*, um die Zielgröße des gesamten Protokolls zu berechnen. Daher ist *target_size* der freie Speicherplatz des Protokolls nach dem Verkleinern. Die Zielgröße des gesamten Protokolls wird dann in die Zielgröße der einzelnen Protokolldateien umgewandelt. DBCC SHRINKFILE versucht, jede physische Protokolldatei sofort auf ihre Zielgröße zu verkleinern. Wenn sich dagegen ein Teil des logischen Protokolls in den virtuellen Protokollen befindet, die außerhalb der Zielgröße liegen, gibt das [!INCLUDE[ssDE](../../includes/ssde-md.md)] so viel Speicherplatz frei wie möglich und gibt dann eine Informationsmeldung aus. Die Meldung beschreibt, welche Aktionen erforderlich sind, um das logische Protokoll aus den virtuellen Protokollen am Ende der Datei zu verschieben. Nachdem diese Aktionen ausgeführt wurden, kann der verbleibende Speicherplatz mit DBCC SHRINKFILE freigegeben werden.
  
Da eine Protokolldatei nur auf eine Grenze einer virtuellen Protokolldatei verkleinert werden kann, ist eine Verkleinerung der Protokolldatei auf eine geringere Größe als die einer virtuellen Protokolldatei u. U. nicht möglich, selbst wenn die Protokolldatei nicht verwendet wird. [!INCLUDE[ssDE](../../includes/ssde-md.md)] wählt dynamisch die Größe des virtuellen Dateiprotokolls, wenn Protokolldateien erstellt oder erweitert werden.
  
## <a name="best-practices"></a>Bewährte Methoden 
 
Berücksichtigen Sie die folgenden Informationen, wenn Sie eine Datei verkleinern möchten:
-   Ein Verkleinerungsvorgang ist am effektivsten nach einem Vorgang, durch den umfangreicher nicht verwendeter Speicherplatz bereitgestellt wird, z. B. das Abschneiden oder Löschen einer Tabelle.  

-   Die meisten Datenbanken benötigen für die täglichen Routinevorgänge eine gewisse Menge an verfügbarem freiem Speicherplatz. Wenn Sie eine Datenbank wiederholt verkleinern und ihre Größe wieder zunimmt, dann ist es wahrscheinlich, dass normale Vorgänge den verkleinerten Platz benötigen. In diesem Fall ist das Verkleinern der Datenbank vergeblich.  

-   Bei einem Verkleinerungsvorgang bleibt der Fragmentierungszustand der Indizes in der Datenbank nicht erhalten. Im Allgemeinen wird die Fragmentierung zu einem gewissen Grad verstärkt. Dies Fragmentierung ist ein weiterer Grund, die Datenbank nicht wiederholt zu verkleinern.  

-   Verkleinern Sie mehrere Dateien in der gleichen Datenbank sequenziell statt gleichzeitig. Konflikte bei Systemtabellen können zu einer Blockierung führen und Verzögerungen verursachen.  
  
## <a name="troubleshooting"></a>Problembehandlung  
In diesem Abschnitt wird beschrieben, wie Probleme, die beim Ausführen des DBCC SHRINKFILE-Befehls auftreten können, diagnostiziert und behoben werden.
  
### <a name="the-file-doesnt-shrink"></a>Die Datei wird nicht verkleinert.
  
Wenn sich die Dateigröße nach einem fehlerfreien Verkleinerungsvorgang nicht ändert, versuchen Sie Folgendes, um sicherzustellen, dass die Datei über ausreichend freien Speicherplatz verfügt:

- Führen Sie die folgende Abfrage aus.  
  
```sql
SELECT name ,size/128.0 - CAST(FILEPROPERTY(name, 'SpaceUsed') AS int)/128.0 AS AvailableSpaceInMB
FROM sys.database_files;
```

-   Führen Sie den Befehl [DBCC SQLPERF](../../t-sql/database-console-commands/dbcc-sqlperf-transact-sql.md) aus, um den vom Transaktionsprotokoll verwendeten Speicherplatz zurückzugeben.  

Falls nicht ausreichend Speicherplatz verfügbar ist, kann die Dateigröße durch den Verkleinerungsvorgang nicht weiter reduziert werden.
  
In der Regel ist es die Protokolldatei, die scheinbar nicht verkleinert wurde. Ist dies der Fall, liegt es gewöhnlich daran, dass die Protokolldatei nicht abgeschnitten wurde. Um das Protokoll abzuschneiden, können Sie das Datenbankwiederherstellungsmodell auf SIMPLE setzen, oder das Protokoll sichern und dann den DBCC SHRINKFILE-Vorgang erneut ausführen.
  
### <a name="the-shrink-operation-is-blocked"></a>Der Verkleinerungsvorgang ist blockiert  

Eine Transaktion, die unter einer [auf Zeilenversionsverwaltung basierenden Isolationsstufe](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md) ausgeführt wird, kann Verkleinerungsvorgänge blockieren. Erfolgt z. B. während eines DBCC SHRINKDATABASE-Vorgangs gleichzeitig ein umfangreicher Löschvorgang, der auf einer auf Zeilenversionsverwaltung basierenden Isolationsstufe ausgeführt wird, wird auf den Abschluss des Löschvorgangs gewartet, bevor die Dateien weiter verkleinert werden. Wenn diese Blockierung auftritt, wird von den DBCC SHRINKFILE- und DBCC SHRINKDATABASE-Vorgängen eine Informationsmeldung (5202 für SHRINKDATABASE und 5203 für SHRINKFILE) an das SQL Server-Fehlerprotokoll ausgegeben. Diese Meldung wird in der ersten Stunde alle fünf Minuten und dann jede Stunde protokolliert. Wenn das Fehlerprotokoll beispielsweise folgende Fehlermeldung enthält, tritt folgender Fehler auf:
  
```
DBCC SHRINKFILE for file ID 1 is waiting for the snapshot   
transaction with timestamp 15 and other snapshot transactions linked to   
timestamp 15 or with timestamps older than 109 to finish.  
```  
  
Diese Meldung bedeutet, dass Momentaufnahmentransaktionen mit Zeitstempeln, die älter als 109 sind (die letzte Transaktion, die der Verkleinerungsvorgang abgeschlossen hat), den Verkleinerungsvorgang blockieren. Außerdem zeigt es an, dass die **transaction_sequence_num**-Spalte oder die **first_snapshot_sequence_num**-Spalte in der dynamischen Verwaltungssicht [sys.dm_tran_active_snapshot_database_transactions](../../relational-databases/system-dynamic-management-views/sys-dm-tran-active-snapshot-database-transactions-transact-sql.md) einen Wert von 15 enthält. Wenn entweder die Ansichtsspalte **transaction_sequence_num** oder **first_snapshot_sequence_num** eine Zahl enthält, die kleiner ist als die zuletzt abgeschlossene Transaktion (109) eines Verkleinerungsvorgangs ist, wartet der Verkleinerungsvorgang darauf, dass diese Transaktionen abgeschlossen sind.
  
Führen Sie eine der folgenden Aufgaben aus, um das Problem zu beheben:
-   Beenden Sie die Transaktion, die den Verkleinerungsvorgang blockiert.
-   Beenden Sie den Verkleinerungsvorgang. Wenn der Verkleinerungsvorgang beendet wird, bleibt der bereits abgeschlossene Teil erhalten.  
-   Führen Sie keine besonderen Aktionen aus, und lassen Sie zu, dass mit dem Verkleinerungsvorgang gewartet wird, bis die blockierende Transaktion abgeschlossen ist.  
  
## <a name="permissions"></a>Berechtigungen  
Erfordert die Mitgliedschaft in der festen Serverrolle **sysadmin** oder der festen Datenbankrolle **db_owner** .
  
## <a name="examples"></a>Beispiele  
  
### <a name="shrinking-a-data-file-to-a-specified-target-size"></a>Verkleinern einer Datendatei auf eine angegebene Zielgröße  
Im folgenden Beispiel wird die Größe der Datendatei `DataFile1` in der `UserDB`-Benutzerdatenbank auf 7 MB verkleinert.
  
```sql  
USE UserDB;  
GO  
DBCC SHRINKFILE (DataFile1, 7);  
GO  
```  
  
### <a name="shrinking-a-log-file-to-a-specified-target-size"></a>Verkleinern einer Protokolldatei auf eine angegebene Zielgröße  
Im folgenden Beispiel wird die Protokolldatei in der `AdventureWorks`-Datenbank auf 1 MB verkleinert. Damit die Datei mit dem DBCC SHRINKFILE-Befehl verkleinert werden kann, wird die Datei zunächst abgeschnitten, indem das Wiederherstellungsmodell für die Datenbank auf SIMPLE festgelegt wird.
  
```sql  
USE AdventureWorks2012;  
GO  
-- Truncate the log by changing the database recovery model to SIMPLE.  
ALTER DATABASE AdventureWorks2012  
SET RECOVERY SIMPLE;  
GO  
-- Shrink the truncated log file to 1 MB.  
DBCC SHRINKFILE (AdventureWorks2012_Log, 1);  
GO  
-- Reset the database recovery model.  
ALTER DATABASE AdventureWorks2012  
SET RECOVERY FULL;  
GO  
```  
  
### <a name="c-truncating-a-data-file"></a>C. Abschneiden einer Datendatei  
Im folgenden Beispiel wird die primäre Datendatei in der `AdventureWorks`-Datenbank abgeschnitten. Die `sys.database_files`-Katalogsicht wird abgefragt, um den `file_id`-Wert für die Datendatei zu erhalten.
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT file_id, name  
FROM sys.database_files;  
GO  
DBCC SHRINKFILE (1, TRUNCATEONLY);  
```  
  
### <a name="d-emptying-a-file"></a>D: Leeren einer Datei  
Das folgende Beispiel veranschaulicht das Leeren einer Datei, sodass sie aus der Datenbank entfernt werden kann. Für dieses Beispiel wird zunächst eine Datei mit Daten erstellt.
  
```sql  
USE AdventureWorks2012;  
GO  
-- Create a data file and assume it contains data.  
ALTER DATABASE AdventureWorks2012   
ADD FILE (  
    NAME = Test1data,  
    FILENAME = 'C:\t1data.ndf',  
    SIZE = 5MB  
    );  
GO  
-- Empty the data file.  
DBCC SHRINKFILE (Test1data, EMPTYFILE);  
GO  
-- Remove the data file from the database.  
ALTER DATABASE AdventureWorks2012  
REMOVE FILE Test1data;  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
[ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)  
[DBCC &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-transact-sql.md)  
[DBCC SHRINKDATABASE &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-shrinkdatabase-transact-sql.md)  
[FILE_ID &#40;Transact-SQL&#41;](../../t-sql/functions/file-id-transact-sql.md)  
[sys.database_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)  
[Verkleinern einer Datei](../../relational-databases/databases/shrink-a-file.md)
  
  
