---
title: Bereitstellen von SQL Server auf virtuellen Azure Computern
description: In diesem Tutorial erfahren Sie, wie Sie SQL Server auf Azure Virtual Machines erstellen können.
author: ninarn
ms.author: ninarn
ms.reviewer: alayu, maghan
ms.topic: tutorial
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.custom: ''
ms.date: 10/14/2020
ms.openlocfilehash: d73e4b95059dab183f9a0af4896c5a6f86b7e564
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091757"
---
# <a name="create-sql-server-on-azure-virtual-machines-using-azure-data-studio"></a>Erstellen von SQL Server auf Azure Virtual Machines mithilfe von Azure Data Studio

Sie können einen virtuellen SQL-Computer (VM) mit Azure Data Studio erstellen, indem Sie den Bereitstellungs-Assistenten und Notebooks verwenden.

## <a name="pre-requisites"></a>Voraussetzungen

- [Azure Data Studio](download-azure-data-studio.md) ist installiert
- Ein aktives Azure-Konto und ein Azure-Abonnement. Falls Sie nicht über ein Abonnement verfügen, können Sie ein [kostenloses Konto erstellen](https://azure.microsoft.com/free/).

## <a name="use-the-deployment-wizard"></a>Verwenden des Bereitstellungs-Assistenten

Führen Sie die folgenden Schritte aus, um den Bereitstellungs-Assistenten zu verwenden, der Sie mit einer einfachen Benutzeroberfläche durch die erforderlichen Einstellungen führt.

Suchen Sie zuerst eine Azure SQL VM im Bereitstellungs-Assistenten, und wählen Sie sie aus.

1. Wählen Sie in Azure Data Studio das Viewlet „Verbindungen“ im Navigationsbereich auf der linken Seite aus.

2. Wählen Sie oben im Bereich „Verbindungen“ die Schaltfläche mit den drei Auslassungspunkten **...** und dann **Neue Bereitstellung** aus.

3. Wählen Sie im Bereitstellungs-Assistenten die Kachel **Azure SQL VM** aus, und aktivieren Sie das Kontrollkästchen zum Akzeptieren der Bedingungen.

4. Wenn Sie dazu aufgefordert werden, installieren Sie die erforderlichen Tools, und wählen Sie dann unten die Schaltfläche **Auswählen** aus.

Geben Sie als nächstes alle erforderlichen Parameter im Azure SQL VM-Assistenten ein.

1. Melden Sie sich bei Ihrem Azure-Konto an, wenn dies noch nicht geschehen ist. Sie können Ihre Verbindung aktualisieren, wenn auf dieser Seite des Assistenten Probleme auftreten.

2. Wählen Sie das gewünschte Abonnement, die Ressourcengruppe und die Region aus. Wählen Sie **Weiter** aus.

3. Geben Sie einen eindeutigen Namen für den virtuellen Computer und Ihre Anmeldeinformationen in Form von Benutzername und Kennwort ein.

4. Wählen Sie Ihr bevorzugtes Image, die SKU und die Version aus, und wählen Sie dann die bevorzugte VM-Größe aus. Sie können mehr zu den [verfügbaren VM-Größen](/azure/virtual-machines/sizes) erfahren, damit Sie Ihre Auswahl treffen können. Wählen Sie **Weiter** aus.

5. Wählen Sie entweder in der Dropdownliste ein vorhandenes virtuelles Netzwerk aus, oder aktivieren Sie das Kontrollkästchen **Neues virtuelles Netzwerk**, um einen Namen für ein neues virtuelles Netzwerk einzugeben.

6. Gehen Sie in gleicher Weise vor, um ein Subnetz und eine öffentliche IP-Adresse zu erstellen oder auszuwählen.

7. Wenn Sie eine Verbindung mit Ihrer VM über Remotedesktop (RDP) herstellen möchten, aktivieren Sie das Kontrollkästchen **Enable RDP inbound port** (Eingehenden RDP-Port aktivieren). Wählen Sie **Weiter** aus.

8. Geben Sie Ihre bevorzugten SQL Server-Einstellungen ein. Die Empfehlung zum Testen dieser Benutzeroberfläche besteht darin, die SQL-Verbindung auf **Öffentlich (Internet)** , festzulegen, als Port 1433 einzugeben und die SQL-Authentifizierung mit Ihrem bevorzugten Benutzernamen und Kennwort zu aktivieren. Wählen Sie **Weiter** aus.

9. Überprüfen Sie die eingegebenen Parameter, und wählen Sie dann **Skript in Notebook schreiben** aus.

Sobald das Notebook geöffnet wird, können Sie die Inhalte und den Code überprüfen und ggf. Änderungen vornehmen. Das Vornehmen von Änderungen wird jedoch nicht empfohlen, da es zu Validierungsfehlern führen kann.

Wählen Sie als letzten Schritt **Run all** (Alle ausführen) aus, um alle Zellen im Notebook auszuführen. Sobald die Ausführung abgeschlossen ist, sollten diese Dinge erstellt worden sein und ausgeführt werden:

- Ein virtueller Azure-Computer
- Ein virtueller SQL-Computer
- Ein virtuelles Netzwerk, ein Subnetz und eine öffentliche IP-Adresse
- Eine Netzwerksicherheitsgruppe und eine Netzwerkschnittstelle
- Zugriff auf den virtuellen Computer per RDP
- Zugriff auf eine Vielzahl von SQL Server-Verwaltungsfunktionen zum Steuern des Sicherungszeitplans, des Patchzeitplans, der SQL Server-Edition und -Lizenzierung, der Konfigurationen zur Speicherleistung und vielem mehr.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Migrieren Ihrer Daten zur neuen SQL Server-VM finden Sie im folgenden Artikel:

> [!div class="nextstepaction"]
> [Migrieren einer SQL Server-Datenbank zu SQL Server auf einem virtuellen Azure-Computer](/azure/azure-sql/virtual-machines/windows/migrate-to-vm-from-sql-server)

Weitere Informationen zur Verwendung von SQL Server in Azure finden Sie unter [SQL Server auf virtuellen Azure-Computern](/azure/azure-sql/virtual-machines/windows/sql-server-on-azure-vm-iaas-what-is-overview) und [Häufig gestellte Fragen](/azure/azure-sql/virtual-machines/windows/frequently-asked-questions-faq).