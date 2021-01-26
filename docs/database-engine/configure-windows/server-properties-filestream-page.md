---
title: SQL Server-Eigenschaften (Seite FILESTREAM) | Microsoft-Dokumentation
description: Informationen zu FILESTREAM-Einstellungen in SQL Server Hier erfahren Sie, wie Sie FILESTREAM aktivieren und den Remoteclientzugriff und andere Eigenschaften konfigurieren.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
f1_keywords:
- sql13.swb.serverproperties.filestream.f1
helpviewer_keywords:
- FILESTREAM [SQL Server], properties page
ms.assetid: 8a8d38d3-e97a-4b09-a40b-659b2e3a5c47
author: markingmyname
ms.author: maghan
ms.openlocfilehash: bde5d28db431bb99e1721ee3984496dd10e6cf1c
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/25/2021
ms.locfileid: "98764803"
---
# <a name="server-properties---filestream-page"></a>Servereigenschaften (Seite FILESTREAM)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Verwenden Sie diese Seite, um FILESTREAM für diese Installation von [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] zu aktivieren.  
  
## <a name="ui-element-list"></a>Liste der Benutzeroberflächenelemente  
 **FILESTREAM für Transact-SQL-Zugriff aktivieren**  
 Wählen Sie diese Option aus, um FILESTREAM für den [!INCLUDE[tsql](../../includes/tsql-md.md)] -Zugriff zu aktivieren. Dieses Steuerelement muss aktiviert werden, bevor die anderen Steuerelementoptionen verfügbar sind.  
  
 **FILESTREAM für E/A-Streamingzugriff auf Datei aktivieren**  
 Wählen Sie diese Option aus, um den Win32-Streamingzugriff für FILESTREAM zu aktivieren.  
  
 **Windows-Freigabename**  
 Verwenden Sie dieses Steuerelement, um den Namen der Windows-Freigabe einzugeben, in der die FILESTREAM-Daten gespeichert werden sollen.  
  
 **Streamingzugriff von Remoteclients auf FILESTREAM-Daten zulassen**  
 Aktivieren Sie dieses Steuerelement, damit Remoteclients auf diese FILESTREAM-Daten auf diesem Server zugreifen können.  
  
## <a name="see-also"></a>Weitere Informationen  
 [FILESTREAM &#40;SQL Server&#41;](../../relational-databases/blob/filestream-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
  
  
