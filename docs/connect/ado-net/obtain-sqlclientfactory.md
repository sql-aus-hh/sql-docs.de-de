---
title: Abrufen von SqlClientFactory
description: Erfahren Sie, wie Sie SqlClientFactory aus der DbProviderFactories-Klasse abrufen, um mit bestimmten Datenquellen in .NET zu arbeiten.
ms.date: 12/22/2020
ms.assetid: a16e4a4d-6a5b-45db-8635-19570e4572ae
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 891cabf04bc8e63537983a91d8526a5cbdf238bf
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771762"
---
# <a name="obtain-a-sqlclientfactory"></a>Abrufen von SqlClientFactory

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Der Prozess des Abrufens einer <xref:System.Data.Common.DbProviderFactory> beinhaltet die Übergabe von Informationen zu einem Datenanbieter an die <xref:System.Data.Common.DbProviderFactories>-Klasse. Auf der Grundlage dieser Informationen erstellt die <xref:System.Data.Common.DbProviderFactories.GetFactory%2A>-Methode eine stark typisierte Anbieterfactory. Zum Erstellen von <xref:Microsoft.Data.SqlClient.SqlClientFactory> können Sie z. B. eine Zeichenfolge an `GetFactory` übergeben, in der der Name des Anbieters als „**Microsoft.Data.SqlClient**“ angegeben ist.

Die andere Überladung von `GetFactory` verwendet eine <xref:System.Data.DataRow>. Nach dem Erstellen der Anbieterfactory können Sie deren Methoden zum Erstellen zusätzlicher Objekte verwenden. Zu den Methoden einer `SqlClientFactory` gehören u. a. <xref:Microsoft.Data.SqlClient.SqlClientFactory.CreateConnection%2A>, <xref:Microsoft.Data.SqlClient.SqlClientFactory.CreateCommand%2A> und <xref:Microsoft.Data.SqlClient.SqlClientFactory.CreateDataAdapter%2A>.

## <a name="register-sqlclientfactory"></a>Registrieren von SqlClientFactory

Zum Abrufen des <xref:Microsoft.Data.SqlClient.SqlClientFactory>-Objekts durch die <xref:System.Data.Common.DbProviderFactories>-Klasse in .NET Framework ist es erforderlich, es in einer **App.config**- oder **web.config**-Datei zu registrieren. Das folgende Konfigurationsdateifragment zeigt die Syntax und das Format für <xref:Microsoft.Data.SqlClient>.  

```xml  
<system.data>
  <DbProviderFactories>
    <add name="Microsoft SqlClient Data Provider"
      invariant="Microsoft.Data.SqlClient"
      description="Microsoft SqlClient Data Provider for SQL Server"
      type="Microsoft.Data.SqlClient.SqlClientFactory, Microsoft.Data.SqlClient, Version=2.0.20168.4, Culture=neutral, PublicKeyToken=23ec7fc2d6eaa4a5"/>
  </DbProviderFactories>
</system.data>  
```  

Das **invariant**-Attribut identifiziert den zugrunde liegenden Datenanbieter. Diese dreiteilige Benennungssyntax wird auch zum Erstellen einer neuen Factory und zum Identifizieren des Anbieters in einer Anwendungskonfigurationsdatei verwendet, sodass der Anbietername und die damit verknüpfte Verbindungszeichenfolge zur Laufzeit abgerufen werden können.  

> [!NOTE]  
> In .NET Core sollte das <xref:Microsoft.Data.SqlClient.SqlClientFactory>-Objekt durch den Aufruf einer <xref:System.Data.Common.DbProviderFactories.RegisterFactory%2A>-Methode im Projekt registriert werden, da es keine GAC- oder globale Konfigurationsunterstützung gibt.

Im folgenden Beispiel wird gezeigt, wie <xref:Microsoft.Data.SqlClient.SqlClientFactory> in einer .NET Core-Anwendung verwendet wird.

[!code-csharp[SqlClientFactory_Netcoreapp#1](~/../sqlclient/doc/samples/SqlClientFactory_Netcoreapp.cs#1)]

## <a name="see-also"></a>Weitere Informationen

- [DbProviderFactories](dbproviderfactories.md)
- [Verbindungszeichenfolgen](connection-strings.md)
- [Verwenden der Konfigurationsklassen](/previous-versions/aspnet/ms228063(v=vs.100))
- [Microsoft ADO.NET für SQL Server](microsoft-ado-net-sql-server.md)
