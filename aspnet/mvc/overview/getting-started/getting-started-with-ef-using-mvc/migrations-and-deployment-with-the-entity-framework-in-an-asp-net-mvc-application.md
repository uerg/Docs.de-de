---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application
title: Code First-Migrationen und Bereitstellung mit Entitätsframework in einer ASP.NET MVC-Anwendung | Microsoft-Dokumentation
author: tdykstra
description: Die Contoso University-Beispielwebanwendung veranschaulicht, wie ASP.NET MVC 5-Anwendungen, die mit dem Entity Framework 6 Code First "und" Visual Studio...
ms.author: riande
ms.date: 11/07/2014
ms.assetid: d4dfc435-bda6-4621-9762-9ba270f8de4e
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 4c1f852bab5e8f77b35239c356d11b5058cbdaef
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832413"
---
<a name="code-first-migrations-and-deployment-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Code First-Migrationen und Bereitstellung mit Entitätsframework in einer ASP.NET MVC-Anwendung
====================
durch [Tom Dykstra](https://github.com/tdykstra)

[Herunterladen des abgeschlossenen Projekts](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Die Contoso University-Beispielwebanwendung veranschaulicht, wie ASP.NET MVC 5-Anwendungen, die mit dem Entity Framework 6 Code First "und" Visual Studio 2013. Informationen zu dieser Tutorialreihe finden Sie im [ersten Tutorial der Reihe](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

Bisher wurde die Anwendung lokal in IIS Express auf Ihrem Entwicklungscomputer ausgeführt wurde. Um eine reale Anwendung für andere Benutzer über das Internet verfügbar zu machen, müssen Sie es auf einen Webhostinganbieter bereitstellen. In diesem Tutorial stellen Sie die Contoso University-Anwendung in der Cloud in Azure bereit.

Das Tutorial enthält die folgenden Abschnitte:

- Aktivieren Sie Code First-Migrationen. Das Migrationsfeature können Sie das Datenmodell ändern und die Änderungen für die Produktion bereitstellen, indem Sie das Datenbankschema ohne löschen und Neuerstellen der Datenbank aktualisieren.
- In Azure bereitgestellt. Dieser Schritt ist optional. Sie können mit den restlichen Tutorials fortfahren, ohne dass das Projekt bereitgestellt.

Es wird empfohlen, dass Sie eine continuous Integration-Prozesses mit der quellcodeverwaltung für die Bereitstellung verwenden, aber diese Themen in diesem Tutorial nicht behandelt. Weitere Informationen finden Sie unter den [quellcodeverwaltung](xref:aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control) und [fortlaufende Integration](xref:aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery) Kapiteln die [Building Real-World Cloud-Apps mit Azure](xref:aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction) e-Book.

## <a name="enable-code-first-migrations"></a>Aktivieren Sie Code First-Migrationen

Wenn Sie eine neue Anwendung entwickeln, ändert sich Ihr Datenmodell häufig. Jedes Mal, wenn das Datenmodell geändert wird, ist es nicht mehr synchron mit der Datenbank. Sie haben konfiguriert, dass das Entity Framework für das automatische Löschen und Neuerstellen der Datenbank jedes Mal, die Sie das Datenmodell ändern. Wenn Sie hinzufügen, entfernen oder Ändern von Entitätsklassen oder Ändern Ihrer `DbContext` Klasse, die beim nächsten der Anwendung ausführen es automatisch die vorhandene Datenbank gelöscht wird, erstellt eine neue Ressourcengruppe, die dem Modell entspricht, und startet diese mit Testdaten.

Diese Methode, bei der die Datenbank mit dem Datenmodell synchron gehalten wird, funktioniert so lange, bis Sie die Anwendung für die Produktion bereitstellen. Wenn die Anwendung in der Produktion ausgeführt wird, wird es in der Regel Speichern von Daten, die Sie beibehalten möchten, und nicht alles, was jedes Mal verloren gehen, die Sie z. B. das Hinzufügen einer neuen Spalteninhalts ändern möchten. Die [Code First-Migrationen](https://msdn.microsoft.com/data/jj591621) löst dieses Problem durch Code First zum Aktualisieren des Datenbankschemas löschen und Neuerstellen der Datenbank aktivieren. In diesem Tutorial stellen Sie die Anwendung bereit, und aktivieren, die Sie die Vorbereitung Migrationen.

1. Deaktivieren Sie den Initialisierer, die Sie zuvor eingerichtet haben durch auskommentieren oder Löschen der `contexts` -Element, das Sie die Datei Web.config der Anwendung hinzugefügt.

    [!code-xml[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.xml?highlight=2,6)]
2. Auch in der Anwendung *"Web.config"* Datei, ändern Sie den Namen der Datenbank in der Verbindungszeichenfolge in "contosouniversity2".

    [!code-xml[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.xml?highlight=2)]

    Durch diese Änderung wird das Projekt eingerichtet, sodass bei der ersten Migration eine neue Datenbank erstellt wird. Dies ist nicht erforderlich, aber Sie werden später sehen, warum es eine gute Idee ist.
3. Von der **Tools** Menü klicken Sie auf **Bibliothekspaket-Manager** und dann **-Paket-Manager-Konsole**.

    ![Selecting_Package_Manager_Console](https://asp.net/media/4336350/1pm.png)
4. Auf der `PM>` Eingabeaufforderung Geben Sie die folgenden Befehle aus:

    `enable-migrations`  
    `add-migration InitialCreate`

    ![Enable-Migrations-Befehl](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

    Die `enable-migrations` Befehl erstellt eine *Migrationen* Ordner im Projekt ContosoUniversity, und es wird in diesem Ordner eine *Configuration.cs* -Datei, die Sie bearbeiten können, um die Migrationen zu konfigurieren.

    (Falls Sie die oben genannten Schritt, die Sie zum Ändern des Datenbanknamens weiterleitet übersehen, Migrationen findet die vorhandene Datenbank und automatisch die `add-migration` Befehl. Das ist in Ordnung, es bedeutet lediglich, dass Sie einen Test des Codes Migrationen ausführen wird nicht, bevor Sie die Datenbank bereitstellen. Später beim Ausführen der `update-database` Befehls geschieht nichts, da die Datenbank bereits vorhanden sind.)

    ![Ordner "Migrations"](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

    Initialisiererklasse, die Sie schon kennen wie die `Configuration` Klasse enthält eine `Seed` Methode.

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

    Der Zweck der [Ausgangswert](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) Methode besteht darin, ermöglichen es Ihnen, einfügen oder aktualisieren Testdaten aus, nach dem Code First erstellt oder die Datenbank aktualisiert. Die Methode wird aufgerufen, wenn die Datenbank erstellt wird, und jedes Mal, wenn das Datenbankschema aktualisiert wird, nachdem ein Datenmodell ändern.

### <a name="set-up-the-seed-method"></a>Richten Sie die Seed-Methode

Wenn Sie gelöscht werden und Erstellung der Datenbank für jede datenmodelländerung, Sie verwenden der Initialisiererklasse `Seed` Methode, um Testdaten einzufügen, da nach jeder modelländerung die Datenbank gelöscht wird und die Testdaten geht verloren. Code First-Migrationen, Test, der Daten, nachdem Änderungen in der Datenbank beibehalten werden, also mit den Testdaten in die [Ausgangswert](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) Methode ist in der Regel nicht erforderlich. In der Tat nicht sollen die `Seed` Methode, um Testdaten einzufügen, wenn Sie Migrationen zum Bereitstellen der Datenbank für die Produktion, verwenden da die `Seed` Methode in der Produktion ausgeführt. In diesem Fall möchten Sie die `Seed` Methode, um in die Datenbank einfügen nur die Daten, die Sie benötigen in der Produktion. Sie können z. B. die Datenbank im tatsächlichen Abteilungsnamen enthalten die `Department` Tabelle, wenn die Anwendung in der Produktion verfügbar ist.

In diesem Tutorial verwenden Sie Migrationen für die Bereitstellung, aber das `Seed` Methode fügt Testdaten ohnehin um leichter sehen, wie die Funktionalität der Anwendung funktioniert, ohne eine große Datenmenge manuell einfügen.

1. Ersetzen Sie den Inhalt von der *Configuration.cs* -Datei mit den folgenden Code, der Testdaten in die neue Datenbank geladen werden. 

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

    Die [Ausgangswert](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) Methode nimmt das Datenbank-Context-Objekt als Eingabeparameter und der Code in der Methode verwendet das Objekt auf der Datenbank neue Entitäten hinzugefügt. Für jeden Entitätstyp, der Code erstellt eine Auflistung von neuen Entitäten, fügt diese an die entsprechende ["DbSet"](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx) Eigenschaft, und klicken Sie dann speichert die Änderungen an der Datenbank. Es ist nicht notwendig, die ["SaveChanges"](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) Methode nach jeder Gruppe von Entitäten, wie hier geschehen ist, aber dies hilft Ihnen, die Ursache eines Problems zu suchen, wenn eine Ausnahme auftritt, während der Code in die Datenbank geschrieben werden.

    Verwenden Sie einige der Anweisungen, die Daten einfügen der [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) Methode zum Ausführen eines Vorgangs "Upsert". Da die `Seed` Methode wird bei jedem Ausführen der `update-database` Befehl nach der jede Migration in der Regel Daten, können nicht nur eingefügt werden, da die Zeilen, die Sie hinzufügen möchten es nach der ersten Migration sein werden, der die Datenbank erstellt. Der Vorgang "Upsert" wird verhindert, dass Fehler, die passieren würde, wenn Sie versuchen, fügen Sie eine Zeile, die bereits vorhanden ist, aber es ***überschreibt*** Änderungen an Daten, die Sie möglicherweise beim Testen der Anwendung vorgenommen haben. Mit Testdaten in einigen Tabellen möchten Sie vielleicht nicht so: in einigen Fällen, wenn Sie Daten während des Tests ändern, sollen die Änderungen, um nach Datenbankupdates bleiben. In diesem Fall einen bedingte Insert-Vorgang ausgeführt werden soll: Fügen Sie eine Zeile nur dann, wenn er nicht bereits vorhanden. Die Seed-Methode verwendet beide Ansätze.

    Der erste Parameter zu übergeben, um die [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) Methode gibt die Eigenschaft zu verwenden, um zu überprüfen, ob eine Zeile bereits vorhanden ist. Für die Testdaten für "Student", die Sie bereitstellen, die `LastName` Eigenschaft kann für diesen Zweck verwendet werden, da jedes Nachnamens in der Liste eindeutig ist:

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

    Dieser Code wird davon ausgegangen, dass die Nachnamen eindeutig sind. Wenn Sie manuell ein Schüler/Student mit einem doppelten Nachnamen hinzufügen, erhalten Sie die folgende Ausnahme beim nächsten einer Migration ausführen.

    Die Sequenz enthält mehr als ein element

    Weitere Informationen zur Behandlung von redundanter Daten wie z. B. zwei Schüler/Studenten, die mit dem Namen "Alexander Carson" finden Sie unter [Seeding and Debugging Entity Framework (EF) DBs](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) andersons Blog. Weitere Informationen zu den `AddOrUpdate` -Methode finden Sie unter [kümmern mit EF 4.3 AddOrUpdate Methode](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) Julie lermans Blog.

    Der Code, der erstellt `Enrollment` Entitäten wird vorausgesetzt, dass die `ID` Wert in den Entitäten der `students` -Auflistung, obwohl Sie diese Eigenschaft im Code festgelegt haben, die die Auflistung erstellt.

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs?highlight=2)]

    Können Sie die `ID` hier Eigenschaft da die `ID` Wert wird festgelegt, wenn Sie aufrufen `SaveChanges` für die `students` Auflistung. EF ruft automatisch der primäre Schlüsselwert ab, wenn eine Entität in die Datenbank eingefügt und aktualisiert die `ID` Eigenschaft der Entität im Speicher.

    Der Code, der den hinzugefügt `Enrollment` Entität, die die `Enrollments` Entitätenmenge verwendet nicht die `AddOrUpdate` Methode. Es überprüft, ob eine Entität bereits vorhanden ist und die Entität eingefügt, wenn er nicht vorhanden ist. Dieser Ansatz wird vorgenommene Änderungen beizubehalten, die Sie an einer Registrierung auf Unternehmensniveau mit der Benutzeroberfläche der Anwendung. Der Code durchläuft jedes Element der `Enrollment` [Liste](https://msdn.microsoft.com/library/6sh2ey19.aspx) und wenn die Registrierung in der Datenbank nicht gefunden wird, wird die Registrierung mit der Datenbank hinzugefügt. Beim ersten der Datenbank Aktualisierung, wird die Datenbank leer sein, sodass jede Registrierung hinzugefügt werden.

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]
2. Erstellen Sie das Projekt.

### <a name="execute-the-first-migration"></a>Führen Sie die erste Migration

Beim Ausführen der `add-migration` Befehl Migrationen generiert den Code, der die Datenbank von Grund auf neu erstellen würden. Dieser Code befindet sich auch in der *Migrationen* Ordner, in der Datei, die mit dem Namen  *&lt;Zeitstempel&gt;\_InitialCreate.cs*. Die `Up` Methode der `InitialCreate` Klasse erstellt, die Datenbanktabellen, die den datenmodellentitätenmengen entsprechen, und die `Down` Methode löscht sie.

[!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Die Migrationsfunktion ruft die Methode `Up` auf, um die Datenmodelländerungen für eine Migration zu implementieren. Wenn Sie einen Befehl eingeben, um ein Rollback für das Update auszuführen, ruft die Migrationsfunktion die Methode `Down` auf.

Dies ist die ursprüngliche Migration erstellt wurde, als Sie eingegeben haben, die die `add-migration InitialCreate` Befehl. Der Parameter (`InitialCreate` im Beispiel) wird verwendet, für die Datei benennen und kann beliebig, die Sie in der Regel wählen Sie ein Wort oder Ausdruck, der zusammengefasst, was bei der Migration erfolgt. Sie können z. B. eine spätere Migration nennen &quot;AddDepartmentTable&quot;.

Wenn Sie die ursprüngliche Migration erstellt haben, als die Datenbank bereits vorhanden war, wird der Code für die Datenbankerstellung zwar generiert, allerdings muss er nicht ausgeführt werden, da die Datenbank bereits dem Datenmodell entspricht. Wenn Sie die App in einer anderen Umgebung bereitstellen, in der die Datenbank noch nicht vorhanden ist, wird dieser Code ausgeführt, um Ihre Datenbank zu erstellen. Daher sollte dieser zunächst getestet werden. Aus diesem Grund haben Sie den Namen der Datenbank zuvor in der Verbindungszeichenfolge geändert – damit eine Datenbank bei Migrationen von Grund auf neu erstellt werden kann.

1. In der **-Paket-Manager-Konsole** Fenster, geben Sie den folgenden Befehl aus:

    `update-database`

    ![](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

    Die `update-database` Befehl führt die `Up` Methode zum Erstellen der Datenbank aus, und klicken Sie dann es ausgeführt wird die `Seed` Methode, um die Datenbank zu füllen. Der gleiche Prozess wird automatisch dann in der Produktion ausgeführt, nach der Bereitstellung der Anwendung, wie Sie im folgenden Abschnitt sehen werden.
2. Verwendung **Server-Explorer** auf die Datenbank zu untersuchen, wie im ersten Tutorial aus, und führen Sie die Anwendung aus, um sicherzustellen, dass alles weiterhin wie zuvor gleichermaßen funktioniert.

## <a name="deploy-to-azure"></a>Bereitstellung in Azure

Bisher wurde die Anwendung lokal in IIS Express auf Ihrem Entwicklungscomputer ausgeführt wurde. Um es für andere Benutzer über das Internet verfügbar zu machen, müssen Sie es auf einen Webhostinganbieter bereitstellen. In diesem Abschnitt des Tutorials stellen Sie es in Azure bereit. In diesem Abschnitt ist optional. Sie können diesen Schritt überspringen, und fahren Sie mit dem folgenden Tutorial fort, oder können Sie die Anweisungen in diesem Abschnitt für einen anderen Hostinganbieter Ihrer Wahl anpassen.

### <a name="using-code-first-migrations-to-deploy-the-database"></a>Mithilfe von Code First-Migrationen zum Bereitstellen der Datenbank

Um die Datenbank bereitstellen, verwenden Sie Code First-Migrationen. Wenn Sie das Veröffentlichungsprofil, die Sie zum Konfigurieren von Einstellungen erstellen für die Bereitstellung aus Visual Studio verwenden, wählen Sie das Kontrollkästchen mit der Bezeichnung **Update Database**. Diese Einstellung bewirkt, dass den Bereitstellungsprozess so konfigurieren Sie die Anwendung automatisch *"Web.config"* Datei auf dem Zielserver so, dass Code First verwendet die `MigrateDatabaseToLatestVersion` datenbankinitialisiererklasse.

Visual Studio nichts mit der Datenbank während des Bereitstellungsprozesses, während sie Ihr Projekt auf den Zielserver kopiert. Wenn Sie die bereitgestellte Anwendung ausführen, und greift auf die Datenbank zum ersten Mal nach der Bereitstellung, überprüft Code First, wenn die Datenbank mit dem Datenmodell übereinstimmt. Wenn ein Konflikt vorliegt, Code First erstellt automatisch die Datenbank (sofern noch keiner vorhanden ist) oder das Datenbankschema auf die neueste Version aktualisiert wird, (Wenn eine Datenbank vorhanden ist, aber es nicht das Modell entspricht). Wenn die Anwendung eine Migrationen implementiert `Seed` -Methode, die Methode ausgeführt wird, nachdem die Datenbank wird erstellt, oder das Schema wird aktualisiert.

Ihre Migrationen `Seed` -Methode fügt die Testdaten. Wenn Sie in einer produktionsumgebung bereitstellen, müssten Sie ändern die `Seed` Methode, sodass die It nur Daten einfügt, die in der-Datenbank eingefügt werden sollen. Z. B. in Ihrem aktuellen Datenmodell empfiehlt um echte Kurse, aber fiktive Schüler/Studenten in der Entwicklungsdatenbank zu erhalten. Können Sie schreiben eine `Seed` Methode, um sowohl in der Entwicklung zu laden und kommentieren Sie die fiktiven Schüler/Studenten, bevor Sie in der produktionsumgebung bereitstellen. Oder Sie schreiben eine `Seed` Methode, um nur Kurse zu laden, und geben die fiktiven Schüler/Studenten in der Testdatenbank manuell mithilfe der Benutzeroberfläche der Anwendung.

### <a name="get-an-azure-account"></a>Ein Azure-Konto erhalten

Sie benötigen ein Azure-Konto. Wenn diese nicht bereits vorhanden, aber Sie Visual Studio-Abonnement müssen, können Sie [aktivieren Sie die Vorteile der Subscription](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/). Andernfalls können Sie in wenigen Minuten ein kostenloses Testkonto erstellen. Weitere Informationen finden Sie unter [kostenlose Azure-Testversion](https://azure.microsoft.com/free/).

### <a name="create-a-web-site-and-a-sql-database-in-azure"></a>Erstellen Sie eine Website und einer SQL-Datenbank in Azure

Ihre Web-app in Azure wird in einer freigegebenen Hostingumgebung ausgeführt, was bedeutet, dass sie auf virtuellen Computern (VMs) ausgeführt wird, die gemeinsam mit anderen Azure-Clients verwendet werden. Eine freigegebene Hostingumgebung ist eine kostengünstige Möglichkeit für den Einstieg in die Cloud. Wenn der Webdatenverkehr zunimmt, kann später die Anwendung skalieren, um die Anforderungen zu erfüllen, indem Sie auf dedizierten virtuellen Computern ausführen. Weitere Informationen zu den Preisoptionen für Azure App Service finden unter der Dokumentation auf [Azure-Dokumentation](https://azure.microsoft.com/pricing/details/app-service/)

Sie stellen die Datenbank in Azure SQL-Datenbank bereit. SQL-Datenbank ist eine cloudbasierte relationale Datenbank-Dienst, der auf SQL Server-Technologien basiert. Tools und Anwendungen, die Arbeit mit SQL Server funktionieren auch mit SQL-Datenbank.

1. In der [Azure-Verwaltungsportal](https://portal.azure.com), klicken Sie auf **neu** klicken Sie auf der linken Registerkarte auf **alle** im neuen Blatt, und klicken Sie dann auf **Web-App und SQL** in der **Web** Abschnitt und schließlich **erstellen**.

    ![Schaltfläche "Neu" im Verwaltungsportal](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/CreateWeb-Sql.png)

   Die **neue Web-App und SQL - erstellen** -Assistent wird geöffnet.

2. Geben Sie auf dem Blatt, die eine Zeichenfolge in die **App-Namen** Feld als eindeutige URL für Ihre Anwendung verwenden. Die vollständige URL besteht aus der hier sowie die Standarddomäne von Azure App Services eingegebenen (. azurewebsites.net). Wenn die **Anwendungsnamen** bereits erstellt ist, wird der Assistent benachrichtigt Sie dieses mit einem roten *der app-Name ist nicht verfügbar* Nachricht. Wenn die **Anwendungsnamen** ist verfügbar, Sie erhalten ein grünes Häkchen.

    ![Erstellen Sie mit der Datenbank-Link im Verwaltungsportal](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Create-WebApp.png)

3. In der **Abonnement** Dropdownliste, wählen Sie das Azure-Abonnement, in denen Sie möchten, die **App Service** befinden.

4. In der **Ressourcengruppe** Textfeld, wählen Sie eine Ressourcengruppe oder ein neues erstellen. Diese Einstellung gibt an, welches Datencenter Ihrer Website ausgeführt wird. Weitere Informationen zu Ressourcengruppen finden Sie in der Dokumentation auf [Azure Docs](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups).
5. Erstellen Sie ein neues **App Service-Plan** durch Klicken auf die *App Service-Abschnitt*, **neu erstellen**, und geben Sie **App Service-Plan** (kann denselben Namen wie sein App Service), **Speicherort**, und **Tarif** (besteht eine Option "free").

    ![](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Create-AppService.png)
6. Klicken Sie auf die **SQL-Datenbank**, und wählen Sie *neu erstellen* oder eine vorhandene Datenbank auswählen

    ![](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Create-Database.png)

7. In der **Namen** Geben Sie einen Namen für Ihre Datenbank.
8. Klicken Sie auf die **Zielserver** Kontrollkästchen **Erstellen eines neuen Servers**. Wenn Sie einen Server zuvor erstellt haben, können Sie auch diesen Server aus der Liste der verfügbaren Server auswählen.
9. Wählen Sie **Tarif** Abschnitt *Free*. Wenn zusätzliche Ressourcen erforderlich sind, kann die Datenbank bis zu einem beliebigen Zeitpunkt hochskaliert werden. Informationen zu Azure SQL-Preisen finden unter der Dokumentation auf [Azure Docs](https://azure.microsoft.com/pricing/details/sql-database/).
10. Ändern Sie [Sortierreihenfolge](https://docs.microsoft.com/sql/relational-databases/collations/collation-and-unicode-support) je nach Bedarf.
11. Geben Sie einen Administrator **SQL-Administratorname** und **SQL-Administratorkennwort**. Wenn Sie ausgewählt haben **neue SQL-Datenbankserver**, Sie sind nicht keinen vorhandenen Namen und das Kennwort hier eingeben, geben Sie einen neuen Namen und das Kennwort, das stattdessen Sie jetzt definieren zur späteren Verwendung, wenn Sie die Datenbank zugreifen. Wenn Sie einen Server, den Sie zuvor erstellt haben ausgewählt, müssen Sie Anmeldeinformationen für diesen Server eingeben.
12. Erfassung von Telemetriedaten kann für App Service mithilfe von Application Insights aktiviert werden. Application Insights mit nur wenig Konfigurationsaufwand erfasst wertvolle-Ereignis, Ausnahme, Abhängigkeit, Anforderung und Ablaufverfolgungsinformationen. Weitere Informationen zu Application Insights, erste Schritte mit [Azure Docs](https://azure.microsoft.com/services/application-insights/).
13. Klicken Sie auf **erstellen** am unteren Rand des Blatts, um anzugeben, dass Sie fertig sind.
  
    Das Verwaltungsportal auf der Seite "Dashboards" zurückgibt und die **Benachrichtigungen** Blatt am oberen Rand der Seite zeigt, dass die Website erstellt wird. Nach einer Weile (in der Regel weniger als einer Minute) wird eine Benachrichtigung, die die Bereitstellung erfolgreich war. In der Navigationsleiste auf der linken Seite der neuen **App Service** wird in der *App Services* Abschnitt und der neue **SQL-Datenbank** wird in der *SQL-Datenbanken*  Abschnitt.

### <a name="deploy-the-application-to-azure"></a>Bereitstellen der Anwendung in Azure

1. In Visual Studio mit der Maustaste des Projekts im **Projektmappen-Explorer** , und wählen Sie **veröffentlichen** aus dem Kontextmenü.
  
    ![Veröffentlichen Sie im Kontextmenü "Projekt"](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)
2. In der **Profil** Registerkarte die **Webveröffentlichung** -Assistenten klicken Sie auf **Microsoft Azure App Service**.
  
    ![Importieren der veröffentlichungseinstellungen](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-ChooseTarget.png)
3. Wenn Sie zuvor nicht Ihrem Azure-Abonnement in Visual Studio hinzugefügt haben, führen Sie die Schritte aus, auf dem Bildschirm. Die folgenden Schritte ermöglichen die Visual Studio für Azure-Abonnement, also, die mit die Liste der **Anwendungsdienste** Ihrer Website enthalten.
 
4. Wählen Sie die **Abonnement** Hinzufügen der App-Dienst, klicken Sie dann die **App Service-Plan** Ordner, die Ihre App Service ein Teil ist, und schließlich die **App Service** selbst gefolgt von **Ok**.

    ![App Service auswählen](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-AppService.png)
5. Nachdem das Profil konfiguriert wurde, die **Verbindung** Registerkarte wird angezeigt. Klicken Sie auf **Verbindung überprüfen** um sicherzustellen, dass die Einstellungen richtig sind.

    ![Verbindung überprüfen](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-Connection.png)
6. Wenn die Verbindung validiert wurde, wird ein grünes Häkchen neben gezeigt die **Verbindung überprüfen** Schaltfläche. Klicken Sie auf **Weiter**.
  
    ![Erfolgreich überprüfte Verbindung](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-SettingsValidated.png)
7. Öffnen der **remoteverbindungszeichenfolge** Dropdown-Liste unter **SchoolContext** , und wählen Sie die Verbindungszeichenfolge für die Datenbank, die Sie erstellt haben.
8. Wählen Sie **Aktualisieren einer Datenbank**.

    ![Registerkarte "Einstellungen"](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-Settings.png)

    Diese Einstellung bewirkt, dass den Bereitstellungsprozess so konfigurieren Sie die Anwendung automatisch *"Web.config"* Datei auf dem Zielserver so, dass Code First verwendet die `MigrateDatabaseToLatestVersion` datenbankinitialisiererklasse.
9. Klicken Sie auf **Weiter**.
10. In der **Vorschau** auf **Vorschau starten**.
  
    ![StartPreview-Schaltfläche in der Registerkarte "Vorschau"](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-Preview.png)
  
    Die Registerkarte zeigt eine Liste der Dateien, die an den Server kopiert werden. Anzeigen der Vorschau nicht zum Veröffentlichen der Anwendung erforderlich, aber es ist eine nützliche Funktion berücksichtigen. In diesem Fall müssen Sie mit der Liste der Dateien nichts, das angezeigt wird. Bei der nächsten Bereitstellung dieser Anwendung werden nur die geänderten Dateien in dieser Liste.
    ![StartPreview DateiAusgabe](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-PreviewLoaded.png)

11. Klicken Sie auf **Veröffentlichen**.
    Visual Studio startet den Prozess des Kopierens von Dateien auf dem Azure-Server.
12. Die **Ausgabe** Fenster zeigt, welche Bereitstellungsaktionen ausgeführt wurden, und meldet die erfolgreiche Durchführung der Bereitstellung.
  
    ![Fenster "Ausgabe" Meldung zur erfolgreichen Bereitstellung](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-BuildOutput.png)
13. Nach der erfolgreichen Bereitstellung wird der Standardbrowser automatisch an die URL der bereitgestellten Website geöffnet.
    Die Anwendung, die Sie erstellt haben, wird jetzt in der Cloud ausgeführt. 
  
    ![Students_index_page_with_paging](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-Site.png)

An diesem Punkt Ihrer *SchoolContext* Datenbank wurde in der Azure SQL-Datenbank erstellt wurde, da Sie ausgewählt haben **Code First-Migrationen ausführen (wird beim Anwendungsstart ausgeführt)**. Die *"Web.config"* -Datei in die bereitgestellte Website geändert wurde, damit die [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) Initialisierer ausgeführt wird, zum ersten Mal Code liest oder schreibt Daten in der Datenbank (die aufgetreten sind Bei Auswahl der **Schüler/Studenten** Registerkarte "):

![](https://asp.net/media/4367421/mig.png)

Der Bereitstellungsprozess erstellt auch eine neue Verbindungszeichenfolge *(SchoolContext\_DatabasePublish*) für Code First-Migrationen zum Aktualisieren des Datenbankschemas und das seeding der Datenbank.

![Database_Publish-Verbindungszeichenfolge](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image26.png)

Sie finden die bereitgestellte Version der Datei "Web.config" auf dem lokalen Computer in *ContosoUniversity\obj\Release\Package\PackageTmp\Web.config*. Sie können die bereitgestellte zugreifen *"Web.config"* Datei selbst unter Verwendung von FTP. Anweisungen hierzu finden Sie unter [ASP.NET-webbereitstellung mithilfe von Visual Studio: Bereitstellen eines Codeupdates](xref:web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update). Befolgen Sie die Anweisungen, die mit beginnen "um ein FTP-Tool verwenden zu können, benötigen Sie drei Dinge: der FTP-URL, den Benutzernamen und das Kennwort."

> [!NOTE]
> Die Web-app implementieren nicht Sicherheit, damit jeder, der die URL findet die Daten ändern kann. Anweisungen dazu, wie Sie die Website zu schützen, finden Sie unter [bereitstellen eine sicheren ASP.NET MVC-app mit Mitgliedschaft, OAuth und SQL-Datenbank in Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Sie können verhindern, dass andere Personen über die Website mithilfe von Azure-Verwaltungsportal oder **Server-Explorer** in Visual Studio für den Standort zu beenden.


![](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Stop-Service.png)

## <a name="advanced-migrations-scenarios"></a>Erweiterte Migrationsszenarien

Wenn Sie eine Datenbank durch Ausführen von Migrationen automatisch durchführen, wie in diesem Tutorial gezeigt bereitstellen, und Sie bereitstellen auf einer Website, die auf mehreren Servern ausgeführt wird, konnte Sie mehrere Server, die beim Ausführen von Migrationen zur gleichen Zeit abgerufen werden. Migrationen sind atomisch –, also wenn zwei Server versuchen, die die gleiche Migration ausführen, eine erfolgreich ausgeführt, und die andere fehl schlägt, (vorausgesetzt, dass die Vorgänge können nicht zweimal ausgeführt werden). In diesem Szenario sollten Sie diese Probleme zu vermeiden können Sie Migrationen manuell aufrufen, und richten Ihren eigenen Code, sodass es nur einmal durchgeführt. Weitere Informationen finden Sie unter [ausgeführt wird und Scripting von Migrationen aus Code](http://romiller.com/2012/02/09/running-scripting-migrations-from-code/) Blog von Rowan Miller des und [Migrate.exe](https://msdn.microsoft.com/data/jj618307) (für das Ausführen von Migrationen über die Befehlszeile) auf MSDN.

Weitere Informationen zu anderen Migrationsszenarien finden Sie unter [Migrationen Screencast-Reihe](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx).

## <a name="code-first-initializers"></a>Code First-Initialisierer

In den Abschnitt zur Bereitstellung, in dem Sie gesehen haben die [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) Initialisierer, die verwendet wird. Code stellt zunächst auch weitere Initialisierungen, einschließlich ["createDatabaseIfNotExists"](https://msdn.microsoft.com/library/gg679221(v=vs.103).aspx) (Standardeinstellung), [DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/library/gg679604(v=VS.103).aspx) (die Sie zuvor verwendet haben) und [ DropCreateDatabaseAlways](https://msdn.microsoft.com/library/gg679506(v=VS.103).aspx). Die `DropCreateAlways` Initialisierer für das Einrichten von Bedingungen für die Komponententests nützlich sein kann. Sie können auch eigene Initialisierer schreiben, und Sie können keinen Initialisierer explizit aufrufen, wenn Sie nicht warten, bis die Anwendung liest bzw. in die Datenbank geschrieben werden sollen. Zu dem Zeitpunkt, die in diesem Tutorial im November 2013 geschrieben wird nur können das Erstellen und DropCreate Initialisierer Sie vor dem Aktivieren von Migrationen. Das Entity Framework-Team arbeitet daran, diese Initialisierer mit Migrationen auch verwendet werden.

Weitere Informationen zu Initialisierern finden Sie unter [Grundlegendes zu Datenbank-Initialisierer in Entity Framework Code First](http://www.codeguru.com/csharp/article.php/c19999/Understanding-Database-Initializers-in-Entity-Framework-Code-First.htm) und Kapitel 6 des Buchs [Programming Entity Framework: Code First](http://shop.oreilly.com/product/0636920022220.do) von Julie Lerman und Rowan Miller.

## <a name="summary"></a>Zusammenfassung

In diesem Tutorial haben Sie erfahren, wie zum Aktivieren von Migrationen erstellen und bereitstellen können. Im nächsten Tutorial beginnen Sie Themen für fortgeschrittenere Benutzer ansehen, indem Sie das Datenmodell erweitern.

Lassen Sie Feedback auf, wie Sie in diesem Tutorial gefallen hat und was wir verbessern können. Sie können auch neue Themen anfordern [anzeigen Me How With Code](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Links zu anderen Ressourcen des Entity Framework finden Sie im [ASP.NET-Datenzugriff – empfohlene Ressourcen](xref:whitepapers/aspnet-data-access-content-map).

> [!div class="step-by-step"]
> [Zurück](xref:mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application)
> [Weiter](xref:mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-a-more-complex-data-model-for-an-asp-net-mvc-application)
