---
title: Datenbankeigenschaften (Seite Änderungsnachverfolgung) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/07/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology:
- database-engine
ms.topic: conceptual
f1_keywords:
- sql13.swb.databaseproperties.changetracking.f1
ms.assetid: 9b929640-bc62-449b-9b06-b5a77b8cf372
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 9cdd4fba26cf337647f0afa5ace87ea04e54c136
ms.sourcegitcommit: 61381ef939415fe019285def9450d7583df1fed0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2018
ms.locfileid: "47750898"
---
# <a name="database-properties-changetracking-page"></a>Datenbankeigenschaften (Seite Änderungsnachverfolgung)
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  Mithilfe dieser Seite können Sie die Einstellungen der Änderungsnachverfolgung für die ausgewählte Datenbank anzeigen und ändern. Informationen zu den auf dieser Seite verfügbaren Optionen finden Sie unter [Aktivieren und Deaktivieren der Änderungsnachverfolgung &#40;SQL Server&#41;](../../relational-databases/track-changes/enable-and-disable-change-tracking-sql-server.md).  
  
## <a name="options"></a>Tastatur  
 **Änderungsnachverfolgung**  
 Mithilfe dieser Option können Sie die Änderungsnachverfolgung für die Datenbank aktivieren und deaktivieren.  
  
 Um die Änderungsnachverfolgung aktivieren zu können, müssen Sie über die Berechtigung zur Änderung der Datenbank verfügen.  
  
 Durch Festlegen des Werts auf **True** wird eine Datenbankoption festgelegt, die eine Aktivierung der Änderungsnachverfolgung für einzelne Tabellen ermöglicht.  
  
 Sie können die Änderungsnachverfolgung mithilfe von [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md)konfigurieren.  
  
 **Beibehaltungsdauer**  
 Gibt die Mindestdauer für die Beibehaltung von Änderungsnachverfolgungsinformationen in der Datenbank an. Die Daten werden nur dann entfernt, wenn der Wert für **Automatisches Cleanup** auf **True** festgelegt ist.  
  
 Der Standardwert ist 2.  
  
 **Einheiten für die Beibehaltungsdauer**  
 Gibt die Einheiten für den Beibehaltungsdauerwert an. Sie können **Tage**, **Stunden**oder **Minuten**auswählen. Der Standardwert ist **Tage**.  
  
 Die Mindestbeibehaltungsdauer ist 1 Minute. Es gibt keine maximale Beibehaltungsdauer.  
  
 **Automatisches Cleanup**  
 Gibt an, ob Änderungsnachverfolgungsinformationen nach der angegebenen Beibehaltungsdauer automatisch entfernt werden.  
  
 Durch Aktivieren von **Automatisches Cleanup** wird eine vorhandene benutzerdefinierte Beibehaltungsdauer auf die Standardbeibehaltungsdauer von 2 Tagen zurückgesetzt.  
  
## <a name="see-also"></a>Weitere Informationen finden Sie unter  
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [CREATE DATABASE &#40;SQL Server Transact-SQL&#41;](../../t-sql/statements/create-database-sql-server-transact-sql.md)  
  
  
