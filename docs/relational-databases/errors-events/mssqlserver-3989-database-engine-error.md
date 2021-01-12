---
description: MSSQLSERVER_3989
title: MSSQLSERVER_3989
ms.custom: ''
ms.date: 12/25/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, vencher, tejasaks, docast
ms.technology: supportability
ms.topic: language-reference
helpviewer_keywords:
- 3989 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: 11862d9f21556dd5039024c225b58f5f0f05f9a4
ms.sourcegitcommit: d819173fb91af6f20ca6ee59686c35c71b060fbc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/28/2020
ms.locfileid: "97797760"
---
# <a name="mssqlserver_3989"></a>MSSQLSERVER_3989
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Details

|attribute|Wert|
|---|---|
|Produktname|SQL Server|
|Ereignis-ID|3989|
|Ereignisquelle|MSSQLSERVER|
|Komponente|SQLEngine|
|Symbolischer Name|XACT_UNSUPPORT_PARALLEL_TRAN3|
|Meldungstext|Die neue Anforderung kann nicht gestartet werden, weil sie einen gültigen Transaktionsdeskriptor aufweisen sollte.|
||

## <a name="explanation"></a>Erklärung

Dieser Fehler tritt auf, wenn Sie eine verteilte Abfrage zum Verknüpfen mehrerer Tabellen ausführen, die von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Remoteinstanzen gehostet werden, während die Einstellung der `XACT_ABORT`-Sitzung auf **ON** festgelegt ist. Dem Benutzer wird eine Fehlermeldung angezeigt, die der folgenden ähnelt:

> Meldung 3989, Ebene 16, Status 1, Zeile #  
Die neue Anforderung kann nicht gestartet werden, weil sie einen gültigen Transaktionsdeskriptor aufweisen sollte.

## <a name="cause"></a>Ursache

Unter den folgenden Bedingungen gelten einige Einschränkungen bei der Art und Weise, wie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verteilte Abfragen behandelt:

- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verknüpft mehrere Tabellen aus einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Remotedatenquelle.
- Die Sitzung, in der die Abfrage ausgeführt wird, ist nicht in einer verteilten Transaktion eingetragen.

In dieser Situation wird beim Versuch, die Abfrage auszuführen, möglicherweise einer der beiden Fehler ausgelöst, die im Abschnitt **Erklärung** erwähnt werden.

## <a name="user-action"></a>Benutzeraktion

Um dieses Problem zu umgehen, schließen Sie die verteilte Abfrage in einer „BEGIN DISTRIBUTED TRANSACTION“-Anweisung ein:

```sql
BEGIN DISTRIBUTED TRANSACTION <Distributed Query> COMMIT TRANSACTION
```
