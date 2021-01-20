---
title: Herstellen einer Verbindung mit Oracle (oracleto SQL) | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine Verbindung mit einer Oracle-Datenbank herstellen, um die Migration mit SSMA für Oracle zu starten Verwenden Sie das Dialogfeld Verbindung mit Oracle herstellen.
author: nahk-ivanov
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 23a48cb6-ff30-49bb-b4a7-603ebcab336f
ms.author: alexiva
ms.openlocfilehash: 8e13b9ea8bcd264d1668cf0979dc2956bd11bef7
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596721"
---
# <a name="connect-to-oracle-oracletosql"></a>Herstellen einer Verbindung mit Oracle (oracleto SQL)

Verwenden Sie das Dialogfeld **Verbindung mit Oracle herstellen** , um eine Verbindung mit der Oracle-Datenbank herzustellen, die Sie migrieren möchten.

Um auf dieses Dialogfeld zuzugreifen, wählen Sie im Menü **Datei** die Option **mit Oracle verbinden** aus. Wenn Sie bereits eine Verbindung hergestellt haben, stellt der Befehl die **Verbindung mit Oracle wieder** her.

## <a name="options"></a>Optionen

**Anbieter**  
Wählen Sie den Datenzugriffs Anbieter für die Verbindung mit der Oracle-Datenbank aus. Verfügbare Anbieter sind der Oracle-Client Anbieter und der OLE DB-Anbieter. Der Standardwert ist der Oracle-Client Anbieter.

**Mode**  
Wählen Sie entweder Standard, TNSNAME oder Verbindungs Zeichen folgen Modus aus.

- Im Standard Modus geben Sie Werte für den Anbieter, den Servernamen, den Serverport, die Oracle-sid, den Benutzernamen und das Kennwort ein bzw. wählen diese aus.
- Im TNSNAME-Modus geben Sie den Verbindungs Bezeichner (TNS-Alias) der Oracle-Datenbank, den Benutzernamen und das Kennwort ein.
- Im Verbindungs Zeichen folgen Modus geben Sie eine Verbindungs Zeichenfolge an.

  > [!IMPORTANT]
  > Es wird nicht empfohlen, den Verbindungs Zeichen folgen Modus zu verwenden, da der Text Kenn Wörter enthalten kann, und er wird als Klartext gesendet.

Der Standardmodus ist der Standardmodus.

**Servername**  
Geben Sie den Namen des Oracle-Servers ein. Der Standard Servername ist identisch mit dem Computernamen. Dies ist eine Standard Modus-Option.

**Serverport**  
Wenn Sie für Verbindungen mit Oracle eine andere Portnummer als 1521 (Standard) verwenden, geben Sie die Portnummer ein. Dies ist eine Standard Modus-Option.

**Verbindungs Bezeichner**  
Geben Sie den Oracle Connect-Bezeichner ein. Dies ist der Alias der Datenbank, wie in der lokalen Datei "tnsnames. Ora" definiert.

Dies ist eine Option für den TNSNAME-Modus.

**Oracle SID**  
Geben Sie die sid für die Datenbank ein. Die SID ist ein Bezeichner, der die Oracle-Datenbank auf einem Computer unterscheidet. Die Standard-sid für eine Datenbank ist die ersten acht Zeichen des Daten Banknamens.

Dies ist eine Standard Modus-Option.

**Benutzername**  
Geben Sie den Benutzernamen ein, der von SSMA für die Verbindung mit der Oracle-Datenbank verwendet werden soll.

**Kennwort**  
Geben Sie das Kennwort für den Benutzernamen ein.

**Verbindungszeichenfolge**  
> [!IMPORTANT]
> Es wird nicht empfohlen, den Verbindungs Zeichen folgen Modus zu verwenden, da der Text Kenn Wörter enthalten kann, und er wird als Klartext gesendet.

Wenn Sie den Verbindungs Zeichen folgen Modus verwenden, geben Sie die vollständige Verbindungs Zeichenfolge für die Verbindung mit Oracle ein.

Verbindungs Zeichenfolgen bestehen aus Parameter Name-Wert-Paaren.

- Informationen OLE DB Verbindungs Zeichenfolgen finden Sie im Artikel [Microsoft OLE DB-Anbieter für Oracle](../../ado/guide/appendixes/microsoft-ole-db-provider-for-oracle.md) in der MSDN Library.

Fügen Sie für SSMA-Verbindungs Zeichenfolgen immer den Provider-Parameter ein. Stellen Sie außerdem sicher, dass Sie den Port-Parameter einschließen, wenn Sie eine Verbindung mit Oracle herstellen.

## <a name="next-steps"></a>Nächste Schritte

Der nächste Schritt des Migrations Vorgangs besteht darin, eine [Verbindung mit SQL Server herzustellen](connect-to-sql-server-oracletosql.md).