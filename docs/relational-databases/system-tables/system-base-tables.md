---
description: Systembasistabellen
title: System Basistabellen | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- system base tables [SQL Server]
- hobt [SQL Server]
- base tables
ms.assetid: 31f2df90-651f-4699-8067-19f59b60904f
author: cawrites
ms.author: chadam
ms.openlocfilehash: c5805fd84eb9bce1092cdfae59a9711c2d63edb8
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102067"
---
# <a name="system-base-tables"></a>Systembasistabellen
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Systembasistabellen sind die zugrunde liegenden Tabellen, in denen die Metadaten für eine bestimmte Datenbank gespeichert werden. In dieser Hinsicht ist die **Master** -Datenbank besonders spezifisch, da Sie einige zusätzliche Tabellen enthält, die in keiner der anderen Datenbanken gefunden werden. Diese Tabellen enthalten permanente Metadaten, deren Bereich serverweit ist.  
  
> [!IMPORTANT]  
>  Die Systembasistabellen werden nur in [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] verwendet und dienen nicht der allgemeinen Verwendung durch Kunden. Sie können geändert werden, und ihre Kompatibilität wird nicht garantiert.  
  
## <a name="system-base-table-metadata"></a>Metadaten-Systembasistabelle  
 Ein Empfänger, der über die Control-, Alter-oder View Definition-Berechtigung für eine Datenbank verfügt, kann die Metadaten der Systembasis Tabelle in der **sys. Objects** -Katalog Sicht sehen. Der Empfänger kann die Namen und Objekt-IDs von Systembasis Tabellen auch mithilfe integrierter Funktionen wie [object_name](../../t-sql/functions/object-name-transact-sql.md) und [object_id](../../t-sql/functions/object-id-transact-sql.md)auflösen.  
  
 Um eine Bindung zu einer Systembasistabelle herzustellen, muss ein Benutzer eine dedizierte Administratorverbindung (Dedicated Administrator Connection, DAC) zu einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] aufbauen. Der Versuch, eine SELECT-Abfrage aus einer Systembasistabelle auszuführen, ohne dass mithilfe von DAC eine Verbindung hergestellt wurde, löst einen Fehler aus.  
  
> [!IMPORTANT]  
>  Zugriff auf Systembasistabellen mithilfe von DAC ist nur für [!INCLUDE[msCoName](../../includes/msconame-md.md)]-Mitarbeiter und nicht für die Verwendung durch Kunden vorgesehen.  
  
## <a name="system-base-tables"></a>Systembasistabellen  
 In der folgenden Tabelle werden die Systembasistabellen in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] aufgeführt und beschrieben.  
  
|Basistabelle|BESCHREIBUNG|  
|----------------|-----------------|  
|**sys.sysschobjs**|Ist in jeder Datenbank vorhanden. Jede Zeile stellt ein Objekt in der Datenbank dar.|  
|**sys.sysschobjs**|Ist in jeder Datenbank vorhanden. Enthält eine Zeile für jede Service Broker-Entität in der Datenbank. Service Broker-Entitäten schließen Folgendes ein:<br /><br /> Nachrichtentyp<br /><br /> Dienstvertrag<br /><br /> Dienst<br /><br /> Die Namen und Typen verwenden binäre Sortierung, die nicht geändert wird.|  
|**sys.sysclsobjs**|Ist in jeder Datenbank vorhanden. Enthält eine Zeile für jede klassifizierte Entität, für die die gleichen allgemeinen Eigenschaften gelten, darunter die folgenden:<br /><br /> Assembly<br /><br /> Sicherungsmedium<br /><br /> Volltextkatalog<br /><br /> Partitionsfunktion<br /><br /> Partitionsschema<br /><br /> Dateigruppe<br /><br /> Verbergungsschlüssel|  
|**sys.sysnsobjs**|Ist in jeder Datenbank vorhanden. Enthält eine Zeile für jede Namespace-bezogene Entität. Diese Tabelle wird zum Speichern von XML-Auflistungsentitäten verwendet.|  
|**sys.syscolpars**|Ist in jeder Datenbank vorhanden. Enthält eine Zeile für jede Spalte in einer Tabelle, Sicht oder Tabellenwertfunktion. Sie enthält auch Zeilen für jeden Parameter einer Prozedur oder einer Funktion.|  
|**sys.systypedsubobjs**|Ist in jeder Datenbank vorhanden. Enthält eine Zeile für jede typisierte untergeordnete Entität. Nur Parameter für Partitionsfunktionen fallen in diese Kategorie.|  
|**sys.sysidxstats**|Ist in jeder Datenbank vorhanden. Enthält eine Zeile für jeden Index oder Statistiken für Tabellen und indizierte Sichten<br /><br /> Hinweis: jeder Index (mit Ausnahme von Heap) ist mit einer Statistik verknüpft, die den gleichen Namen wie der Index aufweist.|  
|**sys.sysiscols**|Ist in jeder Datenbank vorhanden. Enthält eine Zeile für jeden permanenten Index und jede Statistikspalte.|  
|**sys.sysscalartypes**|Ist in jeder Datenbank vorhanden. Enthält eine Zeile für jeden benutzerdefinierten oder Systemtyp.|  
|**sys.sysdbreg**|Ist nur in der **Master** -Datenbank vorhanden. Enthält eine Zeile für jede registrierte Datenbank.|  
|**sys.sysxsrvs**|Ist nur in der **Master** -Datenbank vorhanden. Enthält eine Zeile für jeden lokalen, Verbindungs- oder Remoteserver.|  
|**sys.sysrmtlgns**|Diese Systembasis Tabelle ist nur in der **Master** -Datenbank vorhanden. Enthält eine Zeile für jede Remoteanmeldungszuordnung. Sie wird für die Zuordnung von eingehenden lokalen Anmeldungen, die vorgeben, von einem entsprechenden Server zu stammen, an eine tatsächliche lokale Anmeldung verwendet.|  
|**sys.syslnklgns**|Ist nur in der **Master** -Datenbank vorhanden. Enthält eine Zeile für jede verknüpfte Anmeldungszuordnung. Verknüpfte Anmeldungszuordnungen werden von Remoteprozeduraufrufen und verteilten Abfragen vom lokalen Server zum entsprechenden Verbindungsserver verwendet.|  
|**sys.sysxlgns**|Ist nur in der **Master** -Datenbank vorhanden. Enthält eine Zeile für jeden Serverprinzipal.|  
|**sys.sysdbfiles**|Ist in jeder Datenbank vorhanden. Wenn die **DBID** -Spalte 0 (null) ist, stellt die Zeile eine Datei dar, die zu dieser Datenbank gehört. In der **Master** -Datenbank kann die **DBID** -Spalte nicht NULL sein. Wenn dies der Fall ist, steht die Zeile für eine Masterdatei.|  
|**sys.sysusermsg**|Ist nur in der **Master** -Datenbank vorhanden. Jede Zeile stellt eine benutzerdefinierte Fehlermeldung dar.|  
|**sys.sysprivs**|Ist in jeder Datenbank vorhanden. Enthält eine Zeile für jede Berechtigung auf Datenbank- oder Serverebene.<br /><br /> Hinweis: Berechtigungen auf Server Ebene werden in der **Master** -Datenbank gespeichert.|  
|**sys.sysowners**|Ist in jeder Datenbank vorhanden. Jede Zeile stellt einen Datenbankprinzipal dar.|  
|**sys.sysobjkeycrypts**|Ist in jeder Datenbank vorhanden. Enthält eine Zeile für jeden symmetrischen Schlüssel, jede Verschlüsselung oder kryptografische Eigenschaft, die einem Objekt zugeordnet ist.|  
|**sys.syscerts**|Ist in jeder Datenbank vorhanden. Enthält eine Zeile für jedes Zertifikat in einer Datenbank.|  
|**sys.sysasymkeys**|Ist in jeder Datenbank vorhanden. Jede Zeile stellt einen asymmetrischen Schlüssel dar.|  
|**sys.ftinds**|Ist in jeder Datenbank vorhanden. Enthält eine Zeile für jeden Volltextindex in der Datenbank.|  
|**sys.sysxprops**|Ist in jeder Datenbank vorhanden. Enthält eine Zeile für jede erweiterte Eigenschaft.|  
|**sys.sysallocunits**|Ist in jeder Datenbank vorhanden. Enthält eine Zeile für jede Speicherzuordnungseinheit.|  
|**sys.sysrowsets**|Ist in jeder Datenbank vorhanden. Enthält eine Zeile für jedes Partitionsrowset für einen Index oder einen Heap.|  
|**sys.sysrowsetrefs**|Ist in jeder Datenbank vorhanden. Enthält eine Zeile für jeden Index eines Rowsetverweises.|  
|**sys.syslogshippers**|Ist nur in der **Master** -Datenbank vorhanden. Enthält eine Zeile für jeden Datenbankspiegelungszeugen.|  
|**sys.sysremsvcbinds**|Ist in jeder Datenbank vorhanden. Enthält eine Zeile für jede Remotedienstbindung.|  
|**sys.sysconvgroup**|Ist in jeder Datenbank vorhanden. Enthält eine Zeile für jede Dienstinstanz von Service Broker.|  
|**sys.sysxmitqueue**|Ist in jeder Datenbank vorhanden. Enthält eine Zeile für jede Übertragungswarteschlange von Service Broker.|  
|**sys.sysdesend**|Ist in jeder Datenbank vorhanden. Enthält eine Zeile für jeden sendenden Endpunkt einer Service Broker-Konversation.|  
|**sys.sysdercv**|Ist in jeder Datenbank vorhanden. Enthält eine Zeile für jeden empfangenden Endpunkt einer Service Broker-Konversation.|  
|**sys.sysendpts**|Ist nur in der **Master** -Datenbank vorhanden. Enthält eine Zeile für jeden im Server erstellten Endpunkt.|  
|**sys.syswebmethods**|Ist nur in der **Master** -Datenbank vorhanden. Enthält eine Zeile für jede für einen SOAP-aktivierten HTTP-Endpunkt definierte SOAP-Methode, die auf dem Server erstellt wird.|  
|**sys.sysqnames**|Ist in jeder Datenbank vorhanden. Enthält eine Zeile für jeden Namespace oder jeden qualifizierten Namen für ein 4-Byte-ID-Token.|  
|**sys.sysxmlcomponent**|Ist in jeder Datenbank vorhanden. Jede Zeile stellt eine XML-Schema-Komponente dar.|  
|**sys.sysxmlfacet**|Ist in jeder Datenbank vorhanden. Enthält eine Zeile für jedes XML-Facet (Beschränkung) der XML-Typdefinition.|  
|**sys.sysxmlplacement**|Ist in jeder Datenbank vorhanden. Enthält eine Zeile für jede XML-Platzierung für XML-Komponenten.|  
|**sys.syssingleobjrefs**|Ist in jeder Datenbank vorhanden. Enthält eine Zeile für jeden allgemeinen N-zu-1-Verweis.|  
|**sys.sysmultiobjrefs**|Ist in jeder Datenbank vorhanden. Enthält eine Zeile für jeden allgemeinen N-zu-N-Verweis.|  
|**sys.sysobjvalues**|Ist in jeder Datenbank vorhanden. Enthält eine Zeile für jede allgemeine Werteigenschaft einer Entität.|  
|**sys.sysguidrefs**|Ist in jeder Datenbank vorhanden. Enthält eine Zeile für jeden GUID-klassifizierten ID-Verweis.|  
  
## <a name="updating-system-base-tables"></a>Aktualisieren von System Basistabellen    
Sie können die Daten in den Systemtabellen über die Systemkatalogsichten abrufen. Um die Metadaten in einer Systembasis Tabelle zu aktualisieren, verwenden Sie die entsprechende-Schnittstelle (z. b. DDL-Anweisungen). Systemtabellen können nicht manuell aktualisiert werden. SQL Server meldet die folgenden Meldungen, wenn Sie direkte Updates für Systemtabellen ausführen.

### <a name="a-system-table-is-manually-updated"></a>Eine Systemtabelle wird manuell aktualisiert.
Fehlermeldung 17659: Warnung: Die Systemtabelle mit der ID <id> in der Datenbank mit der ID <id> wurde direkt aktualisiert. Die Cachekohärenz ist möglicherweise verloren gegangen. SQL Server sollte neu gestartet werden.

### <a name="starting-a-database-with-a-system-table-that-was-manually-updated"></a>Beim Starten einer Datenbank mit einer manuell aktualisierten Systemtabelle
Meldung 3859: Warnung: der System Katalog wurde direkt in der Datenbank mit der ID 17 aktualisiert, zuletzt bei date_time.

### <a name="executing-the-dbcc_checkdb-command-after-a-system-table-is-manually-updated"></a>Ausführen des DBCC_CHECKDB Befehls nach dem manuellen Aktualisieren einer Systemtabelle
Meldung 3859: Warnung: der System Katalog wurde direkt in der Datenbank mit der ID 17 aktualisiert, zuletzt bei date_time.

Wenn Sie manuelle Updates für eine Systemtabelle ausführen und ein Problem auftritt, werden Sie möglicherweise aufgefordert, eine Wiederherstellung aus einer Sicherung durchzuführen oder die Daten aus der betroffenen Datenbank in eine neue Datenbank zu kopieren. Weitere Informationen zu [Benutzer Aktions Fehlermeldungen](../errors-events/mssqlserver-8992-database-engine-error.md#user-action).