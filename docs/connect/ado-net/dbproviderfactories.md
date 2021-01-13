---
title: DbProviderFactories
description: Beschreibt das Anbietermodell für Zuordnungsinstanzen und veranschaulicht, wie die Basisklassen im `System.Data.Common`-Namespace verwendet werden.
ms.date: 12/22/2020
ms.assetid: 2a8e2640-3a49-42a1-a3a9-b43026907ae1
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 1a373e7bde1e93d8dc92294f983a96de64e28ae1
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771770"
---
# <a name="dbproviderfactories"></a>DbProviderFactories

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Der <xref:System.Data.Common>-Namespace stellt Klassen zum Erstellen von <xref:System.Data.Common.DbProviderFactory>-Instanzen für die Arbeit mit bestimmten Datenquellen bereit. Wenn Sie eine <xref:System.Data.Common.DbProviderFactory>-Instanz erstellen und Informationen zum Anbieter an die Instanz übergeben, kann die `DbProviderFactory` auf der Grundlage der ihr bereitgestellten Informationen das korrekte, stark typisierte Verbindungsobjekt bestimmen, das zurückgegeben werden soll.  

Der Datenanbieter <xref:Microsoft.Data.SqlClient> wird nicht mehr in der Datei „computer.config“ aufgeführt, aber benutzerdefinierte Anbieter werden dort weiterhin aufgeführt.  

## <a name="in-this-section"></a>In diesem Abschnitt  

[Abrufen von SqlClientFactory](obtain-sqlclientfactory.md)  
Veranschaulicht, wie eine `SqlClientFactory` von der `DbProviderFactories`-Klasse abgerufen wird, um mit bestimmten Datenquellen in .NET zu arbeiten.  

## <a name="see-also"></a>Weitere Informationen:

- [Abrufen und Ändern von Daten in ADO.NET](retrieving-modifying-data.md)
- [Microsoft ADO.NET für SQL Server](microsoft-ado-net-sql-server.md)
