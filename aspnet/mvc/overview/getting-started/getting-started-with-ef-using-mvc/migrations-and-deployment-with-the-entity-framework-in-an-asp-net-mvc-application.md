---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application
title: Code First-Migrationen und Bereitstellung mit dem Entity Framework in einer ASP.NET MVC-Anwendung | Microsoft Docs
author: tdykstra
description: Die Contoso-University Beispielwebanwendung veranschaulicht, wie ASP.NET MVC 5-Anwendungen, die mit dem Entity Framework 6 Code First und Visual Studio erstellen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2014
ms.topic: article
ms.assetid: d4dfc435-bda6-4621-9762-9ba270f8de4e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 04d393edca0469df140f06a7d083a48aa8f84b65
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879555"
---
<a name="code-first-migrations-and-deployment-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Code First-Migrationen und Bereitstellung mit dem Entity Framework in einer ASP.NET MVC-Anwendung
====================
durch [Tom Dykstra](https://github.com/tdykstra)

[Herunterladen des abgeschlossenen Projekts](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) oder [PDF herunterladen](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Die Contoso-University Beispielwebanwendung veranschaulicht, wie ASP.NET MVC 5-Anwendungen, die mit dem Entity Framework 6 Code First und Visual Studio 2013. Informationen zu dieser Tutorialreihe finden Sie im [ersten Tutorial der Reihe](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

Bisher hat die Anwendung lokal in IIS Express auf dem Entwicklungscomputer ausgeführt wurde. Um einer realen Anwendung für andere Benutzer über das Internet verfügbar zu machen, müssen Sie es auf einen Webhostinganbieter bereitstellen. In diesem Lernprogramm stellen Sie die University Contoso-Anwendung in der Cloud in Azure bereit.

Das Lernprogramm enthält folgende Abschnitte:

- Code First-Migrationen zu aktivieren. Die Migrationen-Funktion können Sie das Datenmodell zu ändern und die Änderungen für die Produktion bereitstellen, indem Sie das Datenbankschema aktualisieren, ohne Sie zu löschen und Neuerstellen die Datenbank.
- In Azure bereitstellen. Dieser Schritt ist optional; Sie können die verbleibenden Lernprogramme fortsetzen, ohne dass das Projekt bereitgestellt.

Es wird empfohlen, dass Sie einen Prozess für die fortlaufende Integration mit der quellcodeverwaltung für die Bereitstellung verwenden, aber dieses Lernprogramm diesen Themen nicht behandelt. Weitere Informationen finden Sie unter der [quellcodeverwaltung](xref:aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control) und [fortlaufende Integration](xref:aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery) Kapiteln der [Building Real-World Cloud Apps with Azure](xref:aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction) e-Book.

## <a name="enable-code-first-migrations"></a>Aktivieren Sie die Code First-Migrationen

Wenn Sie eine neue Anwendung entwickeln, ändert sich Ihr Datenmodell häufig. Jedes Mal, wenn das Datenmodell geändert wird, ist es nicht mehr synchron mit der Datenbank. Sie haben das Entity Framework automatisch löschen und Neuerstellen der Datenbank bei jeder Änderung des Datenmodells konfiguriert. Wenn Sie hinzufügen, entfernen, Entitätsklassen oder Ändern Ihrer `DbContext` Klasse, das nächste Mal führen Sie die Anwendung es automatisch die vorhandene Datenbank gelöscht wird, ein neues Zertifikat an, die das Modell entspricht und startet ihn mit Testdaten erstellt.

Diese Methode, bei der die Datenbank mit dem Datenmodell synchron gehalten wird, funktioniert so lange, bis Sie die Anwendung für die Produktion bereitstellen. Wenn die Anwendung in der produktionsumgebung ausgeführt wird, wird es in der Regel Speichern von Daten, die Sie beibehalten möchten, und nicht alles, was jedes Mal nicht mehr verändert z. B. eine neue Spalte hinzufügen möchten. Die [Code First-Migrationen](https://msdn.microsoft.com/data/jj591621) löst dieses Problem durch Code First zum Aktualisieren des Datenbankschemas anstatt durch Löschen und Neuerstellen der Datenbank aktivieren. In diesem Lernprogramm stellen Sie die Anwendung, und um für die Vorbereitung der Migration aktivieren.

1. Deaktivieren Sie den Initialisierer, die Sie zuvor eingerichtet haben auskommentieren oder das Löschen der `contexts` Element, das Sie die Anwendungsdatei "Web.config" hinzugefügt.

    [!code-xml[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.xml?highlight=2,6)]
2. Auch in der Anwendung *"Web.config"* file, ändern Sie den Namen der Datenbank in der Verbindungszeichenfolge auf ContosoUniversity2.

    [!code-xml[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.xml?highlight=2)]

    Durch diese Änderung wird das Projekt eingerichtet, sodass bei der ersten Migration eine neue Datenbank erstellt wird. Ist dies nicht erforderlich, aber Sie werden später angezeigt, daher ist es eine gute Idee bleiben.
3. Aus der **Tools** Menü klicken Sie auf **Bibliothekspaket-Manager** und dann **Package Manager Console**.

    ![Selecting_Package_Manager_Console](https://asp.net/media/4336350/1pm.png)
4. Bei der `PM>` Eingabeaufforderung Geben Sie die folgenden Befehle:

    `enable-migrations`  
    `add-migration InitialCreate`

    ![Enable-Migrations-Befehl](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

    Die `enable-migrations` Befehl erstellt eine *Migrationen* Ordner im Projekt ContosoUniversity, und es wird in diesem Ordner eine *"Configuration.cs"* -Datei, die Sie bearbeiten können, um Migrationen zu konfigurieren.

    (Wenn Sie die vorherigen Schritte, die Sie zum Ändern des Datenbanknamens weiterleitet übersehen, Migrationen finden Sie die vorhandene Datenbank und automatisch lassen, werden die `add-migration` Befehl. Das ist in Ordnung, es bedeutet lediglich, dass Sie einen Test des Codes Migrationen ausgeführt werden, bevor Sie die Datenbank bereitstellen. Später beim Ausführen der `update-database` Befehl keine Aktion ausgeführt, da die Datenbank bereits vorhanden ist.)

    ![Ordner](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

    Wie bei der Initialisierung-Klasse, die Sie zuvor gesehen haben die `Configuration` Klasse enthält eine `Seed` Methode.

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

    Der Zweck der [Ausgangswert](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) Methode besteht darin, ermöglichen es Ihnen, einfügen oder aktualisieren Testdaten aus, nach dem Code First erstellt oder die Datenbank aktualisiert. Die Methode wird aufgerufen, wenn die Datenbank erstellt wird, und jedes Mal, wenn das Datenbankschema aktualisiert wird, nachdem ein Datenmodell ändern.

### <a name="set-up-the-seed-method"></a>Einrichten der Seed-Methode

Wenn der Datenbank für jede Änderung des Dienstdaten-Modell neu erstellen, der Initialisiererklasse verwenden und Löschen von sind `Seed` Methode, um Testdaten, eingefügt werden, da nach jeder Änderung Modell die Datenbank gelöscht wird und die Testdaten verloren gegangen ist. Mit Code First-Migrationen Test Daten, nachdem Änderungen an der Datenbank beibehalten werden, einschließlich also Testdaten in die [Ausgangswert](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) Methode ist in der Regel nicht erforderlich. Tatsächlich sollen die `Seed` Methode, um Testdaten einzufügen, wenn Sie Migrationen zum Bereitstellen der Datenbank bis hin zur Produktion, verwenden da die `Seed` Methode wird ausgeführt, in der Produktion. In diesem Fall sollen die `Seed` aufzurufende Methode fügen Sie in die Datenbank nur die Daten, die Sie benötigen in der Produktion. Beispielsweise sollten Sie die Datenbank im tatsächlichen Abteilungsnamen enthalten die `Department` Tabelle, wenn die Anwendung in der Produktion zur Verfügung steht.

Für dieses Lernprogramm müssen verwenden Sie Migrationen für die Bereitstellung, aber das `Seed` einfügen Methode Testdaten trotzdem um erleichtern es zu sehen, wie die Funktionalität der Anwendung funktioniert, ohne dass manuell eine große Datenmenge eingefügt.

1. Ersetzen Sie den Inhalt von der *"Configuration.cs"* -Datei mit den folgenden Code, der Testdaten in die neue Datenbank geladen werden. 

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

    Die [Ausgangswert](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) Methode nimmt das Datenbankkontextobjekt als Eingabeparameter und der Code in der Methode verwendet das Objekt, das Hinzufügen neuer Entitäten auf die Datenbank. Für jeden Entitätstyp im Code erstellt eine Auflistung von neuen Entitäten, fügt diese an die entsprechende [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx) -Eigenschaft, und klicken Sie dann speichert die Änderungen an der Datenbank. Es ist nicht erforderlich, rufen Sie die [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) Methode nach jeder Gruppe von Entitäten, wie hier erfolgt, aber auf diese Weise können Sie die Ursache eines Problems zu suchen, wenn eine Ausnahme auftritt, während der Code in die Datenbank geschrieben werden.

    Einige der Anweisungen, das Einfügen von Daten verwendet werden, die [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) Methode zum Ausführen eines Vorgangs "Upsert". Da die `Seed` Methode wird bei jedem Ausführen der `update-database` Befehl, in der Regel nach jeder Migration Daten können nicht nur eingefügt werden, da die Zeilen, die Sie hinzufügen möchten bereits es nach der ersten Migration entsprechen, die die Datenbank erstellt. Der Vorgang "Upsert" wird verhindert, dass Fehler, die eintreten würden, wenn Sie versuchen, fügen Sie eine Zeile, die bereits vorhanden ist, aber es ***überschreibt*** Änderungen an Daten, die Sie möglicherweise beim Testen der Anwendung vorgenommen haben. Mit Testdaten in einigen Tabellen nicht empfiehlt, die durchgeführt werden soll: in einigen Fällen beim Ändern von Daten beim Testen soll Ihre Änderungen bleiben nach einer Aktualisierung der Datenbank. In diesem Fall einen bedingten Einfügevorgang erstellt werden sollen: Fügen Sie eine Zeile nur dann, wenn er nicht bereits vorhanden. Die Seed-Methode verwendet beide Ansätze.

    Der erste Parameter übergeben wird, um die [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) Methode gibt die Eigenschaft zu verwenden, um zu überprüfen, ob bereits eine Zeile vorhanden. Für die Testdaten für Studenten, die Sie bereitstellen, die `LastName` Eigenschaft kann für diesen Zweck verwendet werden, da jeder letzte Name in der Liste eindeutig ist:

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

    Mit diesem Code wird davon ausgegangen, dass die Nachnamen eindeutig sind. Wenn Sie einen Studenten mit einem doppelten Nachnamen manuell hinzufügen, erhalten Sie die folgende Ausnahme beim nächsten einer Migration durchführen.

    Die Sequenz enthält mehr als ein element

    Informationen zum Umgang mit redundanten Daten wie z. B. zwei Studenten, die mit dem Namen "Alexander Carson" finden Sie unter [Seeding und Debuggen von Entity Framework (EF) Datenbanken](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) in Rick andersons Blog. Weitere Informationen zu den `AddOrUpdate` -Methode finden Sie unter [Achten Sie darauf mit EF 4.3 AddOrUpdate-Methode](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) Julie Lerman Blog.

    Der Code, der erstellt `Enrollment` Entitäten davon ausgegangen, dass die `ID` Wert in den Entitäten in der `students` -Auflistung, auch wenn Sie diese Eigenschaft im Code festgelegt haben nicht, die die Auflistung erstellt.

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs?highlight=2)]

    Können Sie die `ID` hier Eigenschaft da die `ID` Wert wird festgelegt, wenn Sie aufrufen `SaveChanges` für die `students` Auflistung. EF ruft des Primärschlüsselwerts automatisch ab, wenn eine Entität in der Datenbank eingefügt und aktualisiert die `ID` -Eigenschaft der Entität im Arbeitsspeicher.

    Der Code, der jede fügt `Enrollment` Entität, die die `Enrollments` Entitätssammlung verwendet nicht die `AddOrUpdate` Methode. Er überprüft, ob eine Entität bereits vorhanden ist, und fügt die Entität aus, wenn er nicht vorhanden ist. Dieser Ansatz wird Änderungen beizubehalten, die Sie eine Registrierung Grade mithilfe der Benutzeroberfläche der Anwendung vornehmen. Der Code durchläuft jedes Mitglied der `Enrollment` [Liste](https://msdn.microsoft.com/library/6sh2ey19.aspx) und wenn die Registrierung in der Datenbank nicht gefunden wird, wird die Registrierung mit der Datenbank hinzugefügt. Beim ersten der Datenbank Aktualisierung, wird die Datenbank leer sein, sodass jede Registrierung hinzugefügt wird.

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]
2. Erstellen Sie das Projekt.

### <a name="execute-the-first-migration"></a>Führen Sie die erste Migration

Wenn Sie ausgeführt haben die `add-migration` Befehl Migrationen generiert den Code, der die Datenbank von Grund auf erstellen würden. Dieser Code wird auch in der *Migrationen* Ordner, in der Datei mit dem Namen  *&lt;Zeitstempel&gt;\_InitialCreate.cs*. Die `Up` Methode der `InitialCreate` Klasse erstellt, die Datenbanktabellen, die die Daten Modell Entitätenmengen, entsprechen und die `Down` Methode löscht sie.

[!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Die Migrationsfunktion ruft die Methode `Up` auf, um die Datenmodelländerungen für eine Migration zu implementieren. Wenn Sie einen Befehl eingeben, um ein Rollback für das Update auszuführen, ruft die Migrationsfunktion die Methode `Down` auf.

Dies ist der anfänglichen Migration, die erstellt wurde, wenn Sie eingegeben haben die `add-migration InitialCreate` Befehl. Der Parameter (`InitialCreate` im Beispiel) wird verwendet, für die Datei benennen und kann beliebig sein; Sie wird in der Regel wählen Sie ein Wort oder Ausdruck, die zusammengefasst, was bei der Migration durchgeführt wird. Sie können z. B. eine spätere Migrationen Namen &quot;AddDepartmentTable&quot;.

Wenn Sie die ursprüngliche Migration erstellt haben, als die Datenbank bereits vorhanden war, wird der Code für die Datenbankerstellung zwar generiert, allerdings muss er nicht ausgeführt werden, da die Datenbank bereits dem Datenmodell entspricht. Wenn Sie die App in einer anderen Umgebung bereitstellen, in der die Datenbank noch nicht vorhanden ist, wird dieser Code ausgeführt, um Ihre Datenbank zu erstellen. Daher sollte dieser zunächst getestet werden. Aus diesem Grund haben Sie den Namen der Datenbank zuvor in der Verbindungszeichenfolge geändert – damit eine Datenbank bei Migrationen von Grund auf neu erstellt werden kann.

1. In der **Package Manager Console** Fenster, geben Sie den folgenden Befehl aus:

    `update-database`

    ![](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

    Die `update-database` -Befehl ausgeführt wird die `Up` Methode zum Erstellen der Datenbank aus, und klicken Sie dann es führt die `Seed` Methode zum Auffüllen der Datenbank. Dasselbe Verfahren wird automatisch dann in der Produktion ausgeführt, nach der Bereitstellung der Anwendung, wie Sie im folgenden Abschnitt sehen.
2. Verwendung **Server-Explorer** auf die Datenbank zu überprüfen, wie Sie im ersten Lernprogramm ausgeführt haben, und führen Sie die Anwendung aus, um sicherzustellen, dass alles noch die gleiche wie zuvor.

## <a name="deploy-to-azure"></a>In Azure bereitstellen

Bisher hat die Anwendung lokal in IIS Express auf dem Entwicklungscomputer ausgeführt wurde. Um ihn für andere Benutzer über das Internet verfügbar machen, müssen Sie es auf einen Webhostinganbieter bereitstellen. In diesem Abschnitt des Lernprogramms müssen Sie es in Azure bereitstellen. In diesem Abschnitt ist optional. Sie können diesen Schritt überspringen und mit dem folgenden Lernprogramm fortgesetzt, oder können Sie die Anweisungen in diesem Abschnitt für einen anderen Hostinganbieter Ihrer Wahl anpassen.

### <a name="using-code-first-migrations-to-deploy-the-database"></a>Mithilfe von Code First-Migrationen zum Bereitstellen der Datenbank

Um die Datenbank bereitzustellen, verwenden Sie Code First-Migrationen. Wenn Sie das Veröffentlichungsprofil, mit denen Sie Einstellungen erstellen für die Bereitstellung von Visual Studio konfigurieren, wählen Sie ein Kontrollkästchen mit der Bezeichnung **Update Database**. Diese Einstellung bewirkt, dass der Bereitstellungsprozess so konfigurieren Sie die Anwendung automatisch *"Web.config"* Datei auf dem Zielserver, sodass der Code First verwendet die `MigrateDatabaseToLatestVersion` Initialisiererklasse.

Visual Studio keine Reaktion hervor mit der Datenbank während des Bereitstellungsvorgangs während sie das Projekt auf den Zielserver kopiert werden. Wenn Sie die bereitgestellte Anwendung ausführen und greift auf die Datenbank zum ersten Mal nach der Bereitstellung, überprüft Code First, wenn die Datenbank das Datenmodell entspricht. Wenn es ein Konflikt besteht, Code First erstellt automatisch die Datenbank (sofern noch nicht vorhanden) oder das Datenbankschema auf die neueste Version aktualisiert wird, (Wenn eine Datenbank vorhanden ist, aber es stimmt nicht mit das Modell überein). Wenn die Anwendung eine Migrationen implementiert `Seed` -Methode, die Methode ausgeführt wird, nachdem die Datenbank erstellt wird, oder das Schema aktualisiert.

Migrationen `Seed` Methode fügt Testdaten. Wenn Sie in einer produktionsumgebung bereitgestellt wurden, müssten Sie ändern die `Seed` Methode, sodass die It nur Daten einfügt, die in der-Datenbank eingefügt werden sollen. Angenommen, in Ihrem aktuellen Datenmodell empfiehlt real Kurse aber fiktive Studenten in der Entwicklungsdatenbank verfügen. Sie schreiben eine `Seed` Methode zum Laden sowohl in der Entwicklung, und klicken Sie dann die fiktiven Studenten kommentieren Sie vor der Bereitstellung bis hin zur Produktion. Oder Sie können Schreiben einer `Seed` Methode, um nur Kurse zu laden, und geben die fiktiven Studenten in der Testdatenbank manuell mithilfe der Benutzeroberfläche der Anwendung.

### <a name="get-an-azure-account"></a>Ein Azure-Konto abrufen

Sie benötigen ein Azure-Konto. Wenn Sie nicht bereits vorhanden, aber Sie ein Visual Studio-Abonnement verfügen, können Sie [Vorteile Ihres Abonnements aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/). Andernfalls können Sie in wenigen Minuten ein kostenloses Testkonto erstellen. Weitere Informationen finden Sie unter [kostenlose Azure-Testversion](https://azure.microsoft.com/free/).

### <a name="create-a-web-site-and-a-sql-database-in-azure"></a>Erstellen Sie eine Website und eine SQL-Datenbank in Azure

Ihre Web-app in Azure wird in einer freigegebenen Hostingumgebung ausgeführt, was bedeutet, dass es auf virtuellen Computern (VMs) ausgeführt wird, die mit anderen Azure-Clients freigegeben werden. Eine freigegebene Hostingumgebung ist eine kostengünstige Möglichkeit, in der Cloud zu beginnen. Ihres Webdatenverkehrs zunimmt, kann später die Anwendung skalieren, um die Anforderungen zu erfüllen, indem auf dedizierten virtuellen Computern ausführen. Weitere Informationen zu Optionen Preise für Azure App Service, lesen Sie die Dokumentation auf [Azure Docs](https://azure.microsoft.com/pricing/details/app-service/)

Sie müssen die Datenbank in Azure SQL-Datenbank bereitstellen. SQL-Datenbank ist eine cloudbasierte relationale Datenbankdienst, der auf SQL Server-Technologien integriert ist. Tools und Anwendungen, die Arbeit mit SQL Server funktionieren auch mit SQL-Datenbank.

1. In der [Azure-Verwaltungsportal](https://portal.azure.com), klicken Sie auf **neu** klicken Sie in der linken Registerkarte auf **alle** neues Blatt, und klicken Sie auf **Web-App & SQL** in der **Web** Abschnitt und schließlich **erstellen**.

    ![Schaltfläche "Neu" im Verwaltungsportal](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/CreateWeb-Sql.png)

   Die **neue Web-App und SQL - erstellen** -Assistent wird geöffnet.

2. Das Blatt, geben Sie eine Zeichenfolge in der **Anwendungsnamen** Feld eindeutige URL für Ihre Anwendung verwenden. Die vollständige URL besteht hier plus die Standarddomäne von Azure-App-Dienste Eingabe aus (. azurewebsites.net). Wenn die **Anwendungsnamen** bereits verwendet wird, wird der Assistent benachrichtigt Sie dieses mit einem roten *der app-Name ist nicht verfügbar* Nachricht. Wenn die **Anwendungsnamen** ist verfügbar, Sie erhalten ein grünes Häkchen.

    ![Mit Datenbank-Link im Verwaltungsportal erstellen](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Create-WebApp.png)

3. In der **Abonnement** Dropdownliste wählen Sie das Azure-Abonnement in der sollen die **App Service** befinden.

4. In der **Ressourcengruppe** Textfeld, wählen Sie eine Ressourcengruppe oder einen neuen erstellen. Diese Einstellung gibt an, das Rechenzentrum, in der Website ausgeführt werden. Weitere Informationen zu Ressourcengruppen finden Sie in der Dokumentation auf [Azure Docs](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups).
5. Erstellen Sie ein neues **App Service-Plan** durch Klicken auf die *Abschnitt "App-Dienst"*, **neu erstellen**, und geben Sie **App Service-Plan** (kann denselben Namen wie sein App Service), **Speicherort**, und **Tarif** (es ist eine kostenlose-Option).

    ![](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Create-AppService.png)
6. Klicken Sie auf die **SQL-Datenbank**, und wählen Sie *neu erstellen* oder eine vorhandene Datenbank auswählen

    ![](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Create-Database.png)

7. In der **Namen** Geben Sie einen Namen für Ihre Datenbank.
8. Klicken Sie auf die **Zielserver** wählen Sie im **Erstellen eines neuen Servers**. Wenn Sie einen Server zuvor erstellt haben, können Sie Alternativ können Sie diesen Server aus der Liste der verfügbaren Server auswählen.
9. Wählen Sie **Tarif** Abschnitt *frei*. Wenn zusätzliche Ressourcen benötigt werden, kann die Datenbank zu einem beliebigen Zeitpunkt skaliert. Lesen Sie die Dokumentation weitere auf SQL Azure-Preismodell auf [Azure Docs](https://azure.microsoft.com/pricing/details/sql-database/).
10. Ändern Sie [Sortierung](https://docs.microsoft.com/sql/relational-databases/collations/collation-and-unicode-support) nach Bedarf.
11. Geben Sie einen Administrator **SQL-Administratorbenutzername** und **SQL-Administratorkennwort**. Wenn Sie ausgewählt haben **neue SQL-Datenbankserver**, Sie sind nicht Sie einen vorhandenen Namen und Kennwort hier eingeben, wird Sie einen neuen Namen und ein Kennwort, die Sie jetzt definieren, zur späteren Verwendung beim Zugriff auf die Datenbank eingeben. Wenn Sie einen Server ausgewählt haben, den Sie zuvor erstellt haben, müssen Sie Anmeldeinformationen für diesen Server eingeben.
12. Telemetrie-Erfassung kann für die App Service mithilfe von Application Insights aktiviert werden. Application Insights mit wenig Konfiguration erfasst wertvolle Ereignis, Ausnahme, Abhängigkeits-, Anforderung und Ablaufverfolgungsinformationen. Weitere Informationen zu Application Insights Einstieg [Azure Docs](https://azure.microsoft.com/services/application-insights/).
13. Klicken Sie auf **erstellen** unten auf dem Blatt ", um anzugeben, dass Sie fertig sind.
  
    Das Verwaltungsportal auf der Seite "Dashboards" zurückgibt und die **Benachrichtigungen** Blatt am oberen Rand der Seite zeigt an, dass die Website erstellt wird. Nach einer Weile (in der Regel weniger als eine Minute) wird eine Benachrichtigung, die die Bereitstellung erfolgreich war. In der Navigationsleiste auf der linken Seite der neuen **App Service** wird angezeigt, der *Anwendungsdienste* Abschnitt und der neuen **SQL-Datenbank** wird angezeigt, der *SQL-Datenbanken*  Abschnitt.

### <a name="deploy-the-application-to-azure"></a>Bereitstellen der Anwendung in Azure

1. In Visual Studio mit der Maustaste des Projekts im **Projektmappen-Explorer** , und wählen Sie **veröffentlichen** aus dem Kontextmenü.
  
    ![Im Projektkontextmenü veröffentlichen](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)
2. In der **Profil** auf der Registerkarte die **Web veröffentlichen** -Assistenten klicken Sie auf **Microsoft Azure App Service**.
  
    ![Veröffentlichungseinstellungen importieren](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-ChooseTarget.png)
3. Wenn Sie zuvor nicht Ihrem Azure-Abonnement in Visual Studio hinzugefügt haben, führen Sie die Schritte auf dem Bildschirm. Diese Schritte aktivieren Visual Studio die Verbindung mit Ihrem Azure-Abonnement so, die die Liste der **Anwendungsdienste** Ihrer Website enthalten.
 
4. Wählen Sie die **Abonnement** Sie das App Service-hinzugefügt haben, klicken Sie dann die **App Service-Plan** Ihren App Service ein Teil ist, Ordner und schließlich die **Anwendungsdienst** selbst gefolgt von **Ok**.

    ![Wählen Sie die App Service](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-AppService.png)
5. Nachdem Sie das Profil konfiguriert wurde, die **Verbindung** Registerkarte wird angezeigt. Klicken Sie auf **Validate Connection** um sicherzustellen, dass die Einstellungen richtig sind.

    ![Überprüfen der Verbindung](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-Connection.png)
6. Wenn die Verbindung erfolgreich überprüft wurde, wird ein grünes Häkchen neben gezeigt die **Validate Connection** Schaltfläche. Klicken Sie auf **Weiter**.
  
    ![Erfolgreich überprüfte Verbindung](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-SettingsValidated.png)
7. Öffnen der **Remote Verbindungszeichenfolge** Dropdown-Liste unter **SchoolContext** , und wählen Sie die Verbindungszeichenfolge für die Datenbank, die Sie erstellt haben.
8. Wählen Sie **Datenbank aktualisieren**.

    ![Registerkarte "Einstellungen"](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-Settings.png)

    Diese Einstellung bewirkt, dass der Bereitstellungsprozess so konfigurieren Sie die Anwendung automatisch *"Web.config"* Datei auf dem Zielserver, sodass der Code First verwendet die `MigrateDatabaseToLatestVersion` Initialisiererklasse.
9. Klicken Sie auf **Weiter**.
10. In der **Vorschau** auf **starten Preview**.
  
    ![Schaltfläche "StartPreview" auf der Registerkarte "Vorschau"](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-Preview.png)
  
    Die Registerkarte zeigt eine Liste der Dateien, die an den Server kopiert werden. Anzeigen der Vorschau ist nicht erforderlich, um die Anwendung zu veröffentlichen, aber es ist eine nützliche Funktion zu berücksichtigen. In diesem Fall müssen Sie mit der Liste der Dateien keine Wirkung, die angezeigt wird. Das nächste Mal mit das Sie diese Anwendung bereitstellen, werden in dieser Liste nur die Dateien, die geändert wurden.
    ![Dateiausgabe StartPreview](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-PreviewLoaded.png)

11. Klicken Sie auf **Veröffentlichen**.
    Visual Studio startet den Prozess der die Dateien auf dem Azure-Server kopieren.
12. Die **Ausgabe** Fenster zeigt, welche Bereitstellungsaktionen ausgeführt wurden, und gibt erfolgreichen Abschluss der Bereitstellung.
  
    ![Fenster "Ausgabe" erfolgreich berichtsbereitstellung](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-BuildOutput.png)
13. Nach der erfolgreichen Bereitstellung wird der Standardbrowser automatisch an die URL der bereitgestellten-Website geöffnet.
    Die Anwendung, die Sie erstellt haben, wird jetzt in der Cloud ausgeführt. 
  
    ![Students_index_page_with_paging](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-Site.png)

Zu diesem Zeitpunkt Ihrer *SchoolContext* Datenbank wurde in der Azure SQL-Datenbank erstellt, da Sie ausgewählte **führen Sie Code First-Migrationen (wird beim Anwendungsstart ausgeführt)**. Die *"Web.config"* -Datei in die bereitgestellte Website geändert wurde, damit die [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) Initialisierer ausgeführt wird, zum ersten Mal Code liest oder schreibt Daten in der Datenbank (die aufgetreten sind Wenn Sie aktiviert das **Studenten** Registerkarte "):

![](https://asp.net/media/4367421/mig.png)

Der Bereitstellungsprozess erstellt auch eine neue Verbindungszeichenfolge *(SchoolContext\_DatabasePublish*) für die Code First-Migrationen zum Aktualisieren des Datenbankschemas und das seeding der Datenbank verwendet werden soll.

![Database_Publish-Verbindungszeichenfolge](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image26.png)

Sie finden die bereitgestellte Version der Datei "Web.config" auf dem lokalen Computer in *ContosoUniversity\obj\Release\Package\PackageTmp\Web.config*. Sie erreichen die bereitgestellte *"Web.config"* Datei selbst unter Verwendung von FTP. Anweisungen hierzu finden Sie unter [ASP.NET Web-Bereitstellung mit Visual Studio: Bereitstellen eines Updates Code](xref:web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update). Befolgen Sie die Anweisungen, die beginnen mit "um ein FTP-Tool verwenden zu können, benötigen Sie drei Dinge: der FTP-URL, den Benutzernamen und das Kennwort."

> [!NOTE]
> Die Web-app implementieren nicht Sicherheit, damit Personen die URL findet die Daten ändern kann. Informationen zum Sichern der Website finden Sie unter [eine sichere ASP.NET MVC-app mit Mitgliedschaft, OAuth und SQL-Datenbank in Azure bereitstellen](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Sie können verhindern, dass andere Personen mithilfe der Website mit dem Azure-Verwaltungsportal oder **Server-Explorer** in Visual Studio zum Beenden der Website.


![](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Stop-Service.png)

## <a name="advanced-migrations-scenarios"></a>Erweiterte Migrationsszenarien

Wenn Sie eine Datenbank durch Ausführen von Migrationen automatisch, wie in diesem Lernprogramm gezeigt bereitstellen und Sie die Bereitstellung für eine Website, die auf mehreren Servern ausgeführt wird, konnte mehreren Servern, die versuchen, gleichzeitig Migrationen auszuführen abgerufen werden. Migrationen sind atomar, sodass zwei Server versuchen, die zur selben Migration auszuführen, eine funktionieren und die andere fehl schlägt (vorausgesetzt, dass zwei Mal die Vorgänge erfolgen können). In diesem Szenario ggf. diese Probleme zu vermeiden. können Sie Migrationen manuell aufrufen und richten Ihren eigenen Code, sodass es nur einmal ausgeführt wird. Weitere Informationen finden Sie unter [ausführen und Skripts Migrationen aus Code](http://romiller.com/2012/02/09/running-scripting-migrations-from-code/) Rowan Miller Blog und [Migrate.exe](https://msdn.microsoft.com/data/jj618307) (für Migrationen wird über die Befehlszeile ausgeführt) auf MSDN.

Informationen zu anderen Migrationsszenarien finden Sie unter [Migrationen Screencast Reihe](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx).

## <a name="code-first-initializers"></a>Erste Initialisierer Code

Im Abschnitt Bereitstellung Sie gesehen haben die [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) Initialisierer verwendet wird. Code enthält zunächst auch andere Initialisierer, einschließlich [CreateDatabaseIfNotExists](https://msdn.microsoft.com/library/gg679221(v=vs.103).aspx) (Standard), [DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/library/gg679604(v=VS.103).aspx) (die Sie zuvor verwendet haben) und [ DropCreateDatabaseAlways](https://msdn.microsoft.com/library/gg679506(v=VS.103).aspx). Die `DropCreateAlways` Initialisierer kann nützlich zum Einrichten von Bedingungen für die Komponententests werden. Sie können auch eigene Initialisierer schreiben, und Sie können einen Initialisierer explizit aufrufen, wenn Sie nicht warten, bis die Anwendung liest aus oder in die Datenbank geschrieben werden sollen. Zu dem Zeitpunkt, die in diesem Lernprogramm im November 2013 geschrieben wird können Sie nur die Initialisierer der Create "und" DropCreate verwenden, bevor Sie Migrationen aktivieren. Das Entity Framework-Team arbeitet an machen diese Initialisierer mit Migrationen auch verwendet werden kann.

Weitere Informationen zu den Initialisierer, finden Sie unter [Grundlegendes zu Datenbank-Initialisierer in Entity Framework Code First](http://www.codeguru.com/csharp/article.php/c19999/Understanding-Database-Initializers-in-Entity-Framework-Code-First.htm) und Kapitel 6 des Buchs [Entity Framework-Programmierung: Code First](http://shop.oreilly.com/product/0636920022220.do) von Julie Lerman und Rowan Miller.

## <a name="summary"></a>Zusammenfassung

In diesem Lernprogramm haben Sie gesehen, wie Migrationen zu aktivieren und die Anwendung bereitstellen. In den nächsten Lernprogrammen beginnen Sie erweiterte Themen ansehen, erweitern Sie das Datenmodell.

Lassen Sie Sie Feedback auf wie in diesem Lernprogramm mögen und was wir weiter verbessern können. Sie können auch neue Themen am anfordern [Me wie mit Code anzeigen](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Links zu anderen Entity Framework-Ressourcen finden Sie im [ASP.NET Data Access - Ressourcen empfohlen](xref:whitepapers/aspnet-data-access-content-map).

> [!div class="step-by-step"]
> [Zurück](xref:mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application)
> [Weiter](xref:mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-a-more-complex-data-model-for-an-asp-net-mvc-application)
