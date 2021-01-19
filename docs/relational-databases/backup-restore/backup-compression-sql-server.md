---
title: Sicherungskomprimierung (SQL Server) | Microsoft-Dokumentation
description: In diesem Artikel erhalten Sie Informationen zur Komprimierung von SQL Server-Sicherungen, einschließlich der Einschränkungen, Leistungskompromisse, der Konfiguration der Sicherungskomprimierung und dem Komprimierungsverhältnis.
ms.custom: ''
ms.date: 07/08/2020
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- log shipping [SQL Server], backup compression
- backup compression [SQL Server], about backup compression
- compression [SQL Server], backup compression
- backups [SQL Server], compression
- backing up [SQL Server], backup compression
- backup compression [SQL Server]
ms.assetid: 05bc9c4f-3947-4dd4-b823-db77519bd4d2
author: cawrites
ms.author: chadam
ms.openlocfilehash: a98aaaea7d243853f31d19c0e7c3aa824ee2cc4e
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171472"
---
# <a name="backup-compression-sql-server"></a>Sicherungskomprimierung (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  In diesem Thema wird die Komprimierung von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Sicherungen beschrieben, einschließlich Einschränkungen und Leistungseinbußen bei der Sicherungskomprimierung, Konfiguration der Sicherungskomprimierung sowie Komprimierungsverhältnis.  Die Sicherungskomprimierung wird auf folgenden [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Editionen unterstützt: Enterprise, Standard und Developer.  Jede Edition von [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höhere Versionen können eine komprimierte Sicherung wiederherstellen. 
 
  
##  <a name="benefits"></a><a name="Benefits"></a> Vorteile  
  
-   Da eine komprimierte Sicherung kleiner als eine unkomprimierte Sicherung derselben Daten ist, wird für das Komprimieren einer Sicherung normalerweise weniger Geräte-E/A benötigt, und daher steigt die Sicherungsgeschwindigkeit in der Regel erheblich.  
  
     Weitere Informationen finden Sie unter [Auswirkungen der Sicherungskomprimierung auf die Leistung](#PerfImpact)weiter unten in diesem Thema.  
  
  
##  <a name="restrictions"></a><a name="Restrictions"></a> Einschränkungen  
 Für komprimierte Sicherungen gelten die folgenden Einschränkungen:  
  
-   Komprimierte und nicht komprimierte Sicherungen können nicht nebeneinander in einem Mediensatz bestehen.  
  
-   Frühere Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] können komprimierte Sicherungen nicht lesen.  
  
-   Mit "NTbackup" erstellte Sicherungen können nicht auf demselben Band gespeichert werden wie komprimierte [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Sicherungen.  
  
  
##  <a name="performance-impact-of-compressing-backups"></a><a name="PerfImpact"></a> Auswirkungen der Sicherungskomprimierung auf die Leistung  
 Standardmäßig steigt die CPU-Nutzung durch die Komprimierung erheblich, und die bei der Komprimierung zusätzlich verbrauchten CPU-Ressourcen können sich negativ auf gleichzeitige Vorgänge auswirken. Daher ist es u.U. sinnvoll, in einer Sitzung, bei der die CPU-Nutzung durch[Resource Governor](../../relational-databases/resource-governor/resource-governor.md)eingeschränkt ist, komprimierte Sicherungen mit niedriger Priorität zu erstellen. Weitere Informationen finden Sie unter [Einschränken der CPU-Nutzung durch die Sicherungskomprimierung mithilfe der Ressourcenkontrolle &#40;Transact-SQL&#41;](../../relational-databases/backup-restore/use-resource-governor-to-limit-cpu-usage-by-backup-compression-transact-sql.md).  
  
 Wenn Sie genau wissen möchten, welche Leistung die Sicherungs-E/A erbringt, können Sie die Sicherungs-E/A zu und von den Geräten beurteilen, indem Sie die folgenden Arten von Leistungsindikatoren analysieren:  
  
-   Windows-E/A-Leistungsindikatoren, wie die Indikatoren für physische Datenträger  
  
-   Der Leistungsindikator **Mediumsdurchsatz Bytes/Sekunde** des Objekts [SQLServer:Sicherungsmedium](../../relational-databases/performance-monitor/sql-server-backup-device-object.md)  
  
-   Der Leistungsindikator **Sicherungs-/Wiederherstellungsdurchsatz/Sekunde** des Objekts [SQLServer:Datenbanken](../../relational-databases/performance-monitor/sql-server-databases-object.md)  
  
 Informationen zu den Windows-Indikatoren finden Sie in der Windows-Hilfe. Informationen zur Arbeit mit SQL Server-Indikatoren finden Sie unter [Verwenden von SQL Server-Objekten](../../relational-databases/performance-monitor/use-sql-server-objects.md).  
  
   
##  <a name="calculate-the-compression-ratio-of-a-compressed-backup"></a><a name="CompressionRatio"></a> Berechnen des Komprimierungsverhältnisses einer komprimierten Sicherung  
 Zum Berechnen des Komprimierungsverhältnisses einer Sicherung verwenden Sie die Werte für die Sicherung in den Spalten **backup_size** und **compressed_backup_size** der Verlaufstabelle [backupset](../../relational-databases/system-tables/backupset-transact-sql.md) wie folgt:  
  
 **backup_size**:**compressed_backup_size**  
  
 So gibt ein Komprimierungsverhältnis von 3:1 beispielsweise an, dass Sie etwa 66 Prozent an Speicherplatz sparen. Für eine Abfrage in diesen Spalten können Sie die folgende Transact-SQL-Anweisung verwenden:  
  
```sql  
SELECT backup_size/compressed_backup_size FROM msdb..backupset;  
```  
  
 Das Komprimierungsverhältnis einer komprimierten Sicherung ist abhängig von den komprimierten Daten. Das erzielte Komprimierungsverhältnis kann durch eine Reihe von Faktoren beeinflusst werden. Zu den Hauptfaktoren gehören:  
  
-   Die Art der Daten.  
  
     Zeichendaten lassen sich stärker komprimieren als andere Arten von Daten.  
  
-   Die Einheitlichkeit der Daten unter den Zeilen einer Seite.  
  
     In der Regel enthält eine Seite mehrere Zeilen, in denen ein Feld den gleichen Wert aufweist. Dieser Wert kann möglicherweise sehr stark komprimiert werden. Dagegen ist bei Datenbanken mit Zufallsdaten oder nur einer großen Zeile pro Seite die komprimierte Sicherung beinahe so groß wie eine nicht komprimierte Sicherung.  
  
-   Verschlüsselungsstatus der Daten  
  
     Verschlüsselte Daten lassen sich deutlich weniger komprimieren als entsprechende unverschlüsselte Daten. Wenn die Daten beispielsweise auf Spaltenebene mit Always Encrypted oder einer anderen Verschlüsselung auf Anwendungsebene verschlüsselt wurden, wird die Größe von Sicherungen durch die Komprimierung möglicherweise nicht erheblich reduziert.

     Weitere Informationen zum Komprimieren von mit TDE (Transparent Data Encryption) verschlüsselten Datenbanken finden Sie unter [ mit TDE](#backup-compression-with-tde).

-   Komprimierungsstatus der Datenbank.  
  
     Falls die Datenbank bereits komprimiert ist, ändert sich ihre Größe bei einer komprimierten Sicherung unter Umständen kaum oder gar nicht.  

## <a name="backup-compression-with-tde"></a>Sicherungskomprimierung mit TDE

Ab [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] ermöglicht die Einstellung `MAXTRANSFERSIZE` **größer als 65.536 (64 KB)** einen optimierten Komprimierungsalgorithmus für TDE-verschlüsselte Datenbanken ([Transparent Data Encryption](../../relational-databases/security/encryption/transparent-data-encryption.md)), der eine Seite zuerst entschlüsselt, komprimiert und dann wieder verschlüsselt. Wenn `MAXTRANSFERSIZE` nicht angegeben ist, oder wenn `MAXTRANSFERSIZE = 65536` (64 KB) verwendet wird, komprimiert die Sicherungskomprimierung mit TDE-verschlüsselten Datenbanken die verschlüsselten Seiten direkt und gibt möglicherweise keine guten Komprimierungsraten aus. Weitere Informationen finden Sie unter [Backup Compression for TDE-enabled Databases (Sicherungskomprimierung für TDE-fähige Datenbanken)](/archive/blogs/sqlcat/sqlsweet16-episode-1-backup-compression-for-tde-enabled-databases).

Ab [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] CU5 muss `MAXTRANSFERSIZE` nicht mehr festgelegt werden, um diesen optimierten Komprimierungsalgorithmus mit TDE zu aktivieren. Wenn der Sicherungsbefehl `WITH COMPRESSION` angegeben ist, oder wenn die Serverkonfiguration *backup compression default* auf 1 festgelegt ist, wird `MAXTRANSFERSIZE` automatisch auf 128.000 erhöht, um den optimierten Algorithmus zu aktivieren. Wenn `MAXTRANSFERSIZE` für den Sicherungsbefehl mit einem Wert > 64.000 angegeben wird, wird der angegebene Wert berücksichtigt. Anders ausgedrückt: SQL Server verringert den Wert nie automatisch, sondern erhöht ihn nur. Wenn Sie eine TDE-verschlüsselte Datenbank mit `MAXTRANSFERSIZE = 65536` sichern müssen, müssen Sie `WITH NO_COMPRESSION` angeben oder sicherstellen, dass die Serverkonfiguration *backup compression default* auf 0 festgelegt ist.

Weitere Informationen finden Sie unter [BACKUP (Transact-SQL)](../../t-sql/statements/backup-transact-sql.md).

##  <a name="allocation-of-space-for-the-backup-file"></a><a name="Allocation"></a> Speicherplatzzuordnung für die Sicherungsdatei  
 Bei komprimierten Sicherungen ist die Größe der finalen Sicherungsdatei davon abhängig, wie stark die Daten komprimiert werden können, und dies ist erst bekannt, wenn der Sicherungsvorgang abgeschlossen ist.  Wenn eine Datenbank daher mithilfe einer Komprimierung gesichert wird, verwendet die Datenbank-Engine standardmäßig einen Vorabzuordnungsalgorithmus für die Sicherungsdatei. Dieser Algorithmus ordnet der Sicherungsdatei vorab einen vordefinierten Prozentsatz der Größe der Datenbank zu. Wenn während des Sicherungsvorgangs mehr Platz benötigt wird, lässt die Datenbank-Engine die Datei wachsen. Wenn die finale Größe kleiner als der reservierte Platz ist, verkleinert die Datenbank-Engine die Datei am Ende des Sicherungsvorgangs auf die tatsächliche finale Größe der Sicherung.  
  
 Verwenden Sie das Ablaufverfolgungsflag 3042, damit die Sicherungsdatei nur so viel größer werden kann, wie erforderlich, um die finale Größe zu erreichen. Durch das Ablaufverfolgungsflag 3042 wird bewirkt, dass der Sicherungsvorgang den standardmäßigen Vorabzuordnungsalgorithmus für die Sicherungskomprimierung umgeht. Dieses Ablaufverfolgungsflag ist nützlich, wenn Sie Speicherplatz einsparen müssen, indem Sie nur die tatsächliche für die komprimierte Sicherung benötigte Größe zuordnen. Dieses Ablaufverfolgungsflag kann jedoch eine leichte Leistungseinbuße (eine mögliche Verlängerung des Sicherungsvorgangs) verursachen.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Verwandte Aufgaben  
  
-   [Konfigurieren von Sicherungskomprimierung &#40;SQL Server&#41;](../../relational-databases/backup-restore/configure-backup-compression-sql-server.md)  
  
-   [Anzeigen oder Konfigurieren der Serverkonfigurationsoption Standardeinstellung für die Sicherungskomprimierung](../../database-engine/configure-windows/view-or-configure-the-backup-compression-default-server-configuration-option.md)  
  
-   [Einschränken der CPU-Nutzung durch die Sicherungskomprimierung mithilfe der Ressourcenkontrolle &#40;Transact-SQL&#41;](../../relational-databases/backup-restore/use-resource-governor-to-limit-cpu-usage-by-backup-compression-transact-sql.md)  
  
-   [DBCC TRACEON &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-traceon-transact-sql.md)  
  
-   [DBCC TRACEOFF &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-traceoff-transact-sql.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Übersicht über Sicherungen &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-overview-sql-server.md)   
 [Ablaufverfolgungsflags &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md)  
  
