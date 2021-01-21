---
title: Schreiben eines SQL Server-Komponententests, der im Gültigkeitsbereich einer einzelnen Transaktion ausgeführt wird
description: In diesem Artikel erfahren Sie, wie Sie mit der Verwendung des Distributed Transaction Coordinator-Diensts beginnen, einen SQL Server-Komponententest mit einzelner Transaktion schreiben und ein Rollback für die Teständerungen ausführen.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: cb241e94-d81c-40e9-a7ae-127762a6b855
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 95b91ffe542b4085dcdeca9f3e0dcfc4928b6564
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/20/2021
ms.locfileid: "98597007"
---
# <a name="how-to-write-a-sql-server-unit-test-that-runs-within-the-scope-of-a-single-transaction"></a>Gewusst wie: Schreiben eines SQL Server-Komponententests, der im Gültigkeitsbereich einer einzelnen Transaktion ausgeführt wird

Sie können Komponententests so anpassen, dass sie im Gültigkeitsbereich einer einzelnen Transaktion ausgeführt werden. Bei diesem Ansatz können Sie für alle vom Test durchgeführten Änderungen ein Rollback ausführen, nachdem der Test beendet wurde. Die Vorgehensweise wird in folgenden Verfahren erläutert:  
  
-   Erstellen einer Transaktion in Ihrem Transact\-SQL-Testskript, das **BEGIN TRANSACTION** und **ROLLBACK TRANSACTION** verwendet  
  
-   Erstellen einer Transaktion für eine einzelne Testmethode in einer Testklasse  
  
-   Erstellen einer Transaktion für alle Testmethoden in einer bestimmten Testklasse  
  
**Voraussetzungen**  
  
Bei einigen Verfahren in diesem Thema muss der Distributed Transaction Coordinator-Dienst auf dem Computer ausgeführt werden, auf dem die Komponententests ausgeführt werden. Weitere Informationen finden Sie im Verfahren am Ende dieses Themas.  
  
## <a name="to-create-a-transaction-using-transact-sql"></a>So erstellen Sie eine Transaktion mit Transact\-SQL  
  
#### <a name="to-create-a-transaction-using-transact-sql"></a>So erstellen Sie eine Transaktion mit Transact\-SQL  
  
1.  Öffnen Sie einen Komponententest im SQL Server-Komponententest-Designer. (Doppelklicken Sie auf die Quellcodedatei des Komponententests, um den Designer anzuzeigen.)  
  
2.  Geben Sie den Typ des Skripts an, für das Sie die Transaktion erstellen möchten. Sie können z. B. einen Vortest, Test oder Nachtest angeben.  
  
3.  Geben Sie ein Testskript im Transact\-SQL-Editor ein.  
  
4.  Fügen Sie eine `BEGIN TRANSACTION`-Anweisung und eine `ROLLBACK TRANSACTION`-Anweisung ein, wie in diesem einfachen Beispiel veranschaulicht. Im Beispiel wird eine Datenbanktabelle mit dem Namen "OrderDetails" und 50 Datenzeilen verwendet:  
  
    ```  
    BEGIN TRANSACTION TestTransaction  
    UPDATE "OrderDetails" set Quantity = Quantity + 10  
    IF @@ROWCOUNT!=50  
    RAISERROR('Row count does not equal 50',16,1)  
    ROLLBACK TRANSACTION TestTransaction  
    ```  
  
    > [!NOTE]  
    > Sie können kein Rollback für eine Transaktion ausführen, nachdem eine COMMIT TRANSACTION-Anweisung ausgeführt wurde.  
  
    Weitere Informationen zur Funktionsweise von ROLLBACK TRANSACTION mit gespeicherten Prozeduren und Triggern finden Sie auf der folgenden Seite auf der Microsoft-Website: [ROLLBACK TRANSACTION (Transact-SQL)](../t-sql/language-elements/rollback-transaction-transact-sql.md).  
  
## <a name="to-create-a-transaction-for-a-single-test-method"></a>So erstellen Sie eine Transaktion für eine einzelne Testmethode  
In diesem Beispiel verwenden Sie zusammen mit dem [System.Transactions.TransactionScope](/dotnet/api/system.transactions.transactionscope)-Typ eine Ambient-Transaktion. Für Verbindungen im Ausführungs- oder privilegierten Kontext wird die Ambient-Transaktion standardmäßig nicht verwendet, da die Verbindungen erstellt wurden, bevor die Methode ausgeführt wird. SqlConnection verfügt über eine [System.Data.SqlClient.SqlConnection.EnlistTransaction](/dotnet/api/system.data.sqlclient.sqlconnection.enlisttransaction)-Methode, durch die einer aktiven Verbindung eine Transaktion zugeordnet wird. Beim Erstellen einer Ambient-Transaktion wird diese als aktuelle Transaktion registriert. Mithilfe der [System.Transactions.Transaction.Current](/dotnet/api/system.transactions.transaction.current)-Eigenschaft können Sie auf die Transaktion zugreifen. In diesem Beispiel wird für die Transaktion ein Rollback ausgeführt, sobald die Ambient-Transaktion verworfen wird. Um für Änderungen bei der Ausführung des Komponententests ein Commit auszuführen, rufen Sie die [System.Transactions.TransactionScope.Complete](/dotnet/api/system.transactions.transactionscope.complete)-Methode auf.  
  
#### <a name="to-create-a-transaction-for-a-single-test-method"></a>So erstellen Sie eine Transaktion für eine einzelne Testmethode  
  
1.  Klicken Sie im **Projektmappen-Explorer** im Testprojekt mit der rechten Maustaste auf den Knoten **Verweise**, und klicken Sie auf **Verweis hinzufügen**.  
  
    Das Dialogfeld **Verweis hinzufügen** wird angezeigt.  
  
2.  Klicken Sie auf die Registerkarte **.NET**.  
  
3.  Klicken Sie in der Assemblyliste erst auf **System.Transactions** und dann auf **OK**.  
  
4.  Öffnen Sie die Visual Basic- oder C#-Datei für den Komponententest.  
  
5.  Umschließen Sie die Vortest-, Test- und Nachtestaktionen, wie im folgenden Visual Basic-Codebeispiel gezeigt:  
  
    ```  
    <TestMethod()> _  
    Public Sub dbo_InsertTable1Test()  
  
        Using ts as New System.Transactions.TransactionScope( System.Transactions.TransactionScopeOption.Required)  
            ExecutionContext.Connection.EnlistTransaction(Transaction.Current)  
            PrivilegedContext.Connection.EnlistTransaction(Transaction.Current)  
  
            Dim testActions As DatabaseTestActions = Me.dbo_InsertTable1TestData  
            'Execute the pre-test script  
            '  
            System.Diagnostics.Trace.WriteLineIf((Not (testActions.PretestAction) Is Nothing), "Executing pre-test script...")  
            Dim pretestResults() As ExecutionResult = TestService.Execute(Me.PrivilegedContext, Me.PrivilegedContext, testActions.PretestAction)  
            'Execute the test script  
  
            System.Diagnostics.Trace.WriteLineIf((Not (testActions.TestAction) Is Nothing), "Executing test script...")  
            Dim testResults() As ExecutionResult = TestService.Execute(ExecutionContext, Me.PrivilegedContext, testActions.TestAction)  
  
            'Execute the post-test script  
            '  
            System.Diagnostics.Trace.WriteLineIf((Not (testActions.PosttestAction) Is Nothing), "Executing post-test script...")  
            Dim posttestResults() As ExecutionResult = TestService.Execute(Me.PrivilegedContext, Me.PrivilegedContext, testActions.PosttestAction)  
  
            'Because the transaction is not explicitly committed, it  
            'is rolled back when the ambient transaction is   
            'disposed.  
            'To commit the transaction, remove the comment delimiter  
            'from the following statement:  
            'ts.Complete()  
  
    End Sub  
    Private dbo_InsertTable1TestData As DatabaseTestActions  
    ```  
  
    > [!NOTE]  
    > Bei Verwendung von Visual Basic müssen Sie (neben `Imports Microsoft.VisualStudio.TestTools.UnitTesting`, `Imports Microsoft.VisualStudio.TeamSystem.Data.UnitTesting` und `Imports Microsoft.VisualStudio.TeamSystem.Data.UnitTest.Conditions`) `Imports System.Transactions` hinzufügen. Bei Verwendung von Visual C# müssen Sie (neben den `using`-Anweisungen für Microsoft.VisualStudio.TestTools, Microsoft.VisualStudio.TeamSystem.Data.UnitTesting und Microsoft.VisualStudio.TeamSystem.Data.UnitTesting.Conditions) `using System.Transactions` hinzufügen. Außerdem müssen Sie diesen Assemblys einen Verweis auf das Projekt hinzufügen.  
  
## <a name="to-create-a-transaction-for-all-test-methods-in-a-test-class"></a>So erstellen Sie eine Transaktion für alle Testmethoden in einer Testklasse  
  
#### <a name="to-create-a-transaction-for-all-test-methods-in-a-test-class"></a>So erstellen Sie eine Transaktion für alle Testmethoden in einer Testklasse  
  
1.  Öffnen Sie die Visual Basic- oder C#-Datei für den Komponententest.  
  
2.  Erstellen Sie die Transaktion in TestInitialize, und verwerfen Sie sie in TestCleanup, wie im folgenden Visual C#-Codebeispiel gezeigt:  
  
    ```  
    TransactionScope _trans;  
  
            [TestInitialize()]  
            public void Init()  
            {  
                _trans = new TransactionScope();  
                base.InitializeTest();  
            }  
  
            [TestCleanup()]  
            public void Cleanup()  
            {  
                base.CleanupTest();  
                _trans.Dispose();  
            }  
  
            [TestMethod()]  
            public void TransactedTest()  
            {  
                DatabaseTestActions testActions = this.DatabaseTestMethod1Data;  
                // Execute the pre-test script  
                //   
                System.Diagnostics.Trace.WriteLineIf((testActions.PretestAction != null), "Executing pre-test script...");  
                ExecutionResult[] pretestResults = TestService.Execute(this.PrivilegedContext, this.PrivilegedContext, testActions.PretestAction);  
                // Execute the test script  
                //   
                System.Diagnostics.Trace.WriteLineIf((testActions.TestAction != null), "Executing test script...");  
                ExecutionResult[] testResults = TestService.Execute(this.ExecutionContext, this.PrivilegedContext, testActions.TestAction);  
                // Execute the post-test script  
                //   
                System.Diagnostics.Trace.WriteLineIf((testActions.PosttestAction != null), "Executing post-test script...");  
                ExecutionResult[] posttestResults = TestService.Execute(this.PrivilegedContext, this.PrivilegedContext, testActions.PosttestAction);  
  
            }  
    ```  
  
## <a name="to-start-the-distributed-transaction-coordinator-service"></a>So starten Sie den Distributed Transaction Coordinator-Dienst  
In einigen Verfahren in diesem Thema werden Typen in der System.Transactions-Assembly verwendet. Bevor Sie diese Verfahren ausführen, müssen Sie sicherstellen, dass der Distributed Transaction Coordinator-Dienst auf dem Computer ausgeführt wird, auf dem die Komponententests laufen. Andernfalls tritt bei den Tests ein Fehler mit folgender Fehlermeldung auf: „Die Testmethode *Projektname*.*Testname*.*Methodenname* hat eine Ausnahme ausgelöst: System.Data.SqlClient.SqlException: MS DTC auf dem Server "*Computername*" ist nicht verfügbar.“  
  
#### <a name="to-start-the-distributed-transaction-coordinator-service"></a>So starten Sie den Distributed Transaction Coordinator-Dienst  
  
1.  Öffnen Sie die **Systemsteuerung**.  
  
2.  Klicken Sie in der **Systemsteuerung** auf die Option **Verwaltung**.  
  
3.  Öffnen Sie unter **Verwaltung** die Option **Dienste**.  
  
4.  Klicken Sie im Bereich **Dienste** mit der rechten Maustaste auf den Dienst **Distributed Transaction Coordinator**, und klicken Sie auf **Starten**.  
  
    Der Status des Diensts sollte sich in **Gestartet** ändern. Jetzt sollten Sie in der Lage sein, Komponententests auszuführen, die System.Transactions verwenden.  
  
> [!IMPORTANT]  
> Folgender Fehler kann ausgegeben werden, obwohl der Distributed Transaction Controller-Dienst gestartet wurde: `System.Transactions.TransactionManagerCommunicationException: Network access for Distributed Transaction Manager (MSDTC) has been disabled. Please enable DTC for network access in the security configuration for MSDTC using the Component Services Administrative tool. ---> System.Runtime.InteropServices.COMException: The transaction manager has disabled its support for remote/network transactions. (Exception from HRESULT: 0x8004D024)`. In diesem Fall muss der Distributed Transaction Controller für den Netzwerkzugriff konfiguriert werden. Weitere Informationen finden Sie unter [Aktivieren des DTC-Netzwerkzugriffs](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753510(v=ws.10)).  
  
## <a name="see-also"></a>Weitere Informationen  
[Erstellen und Definieren von SQL Server-Komponententests](../ssdt/creating-and-defining-sql-server-unit-tests.md)  
