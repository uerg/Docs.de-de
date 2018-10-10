---
uid: web-forms/overview/deployment/visual-studio-web-deployment/preparing-databases
title: 'ASP.NET-webbereitstellung mithilfe von Visual Studio: Vorbereiten für die Datenbankbereitstellung | Microsoft-Dokumentation'
author: tdykstra
description: Dieser tutorialreihe erfahren Sie, wie bereitzustellende (veröffentlichen) aus einer ASP.NET web-Anwendung auf Azure App Service-Web-Apps oder bei einem Hostinganbieter von Drittanbietern, indem Warnungsprovider...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: ae4def81-fa37-4883-a13e-d9896cbf6c36
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/preparing-databases
msc.type: authoredcontent
ms.openlocfilehash: 67f44d9f23a2fe83c48e68328b1dee739056e32f
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912383"
---
<a name="aspnet-web-deployment-using-visual-studio-preparing-for-database-deployment"></a>ASP.NET-webbereitstellung mithilfe von Visual Studio: Vorbereiten für die Datenbankbereitstellung
====================
durch [Tom Dykstra](https://github.com/tdykstra)

[Startprojekt herunterladen](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Dieser tutorialreihe erfahren Sie, wie bereitzustellende (veröffentlichen) aus einer ASP.NET web-Anwendung auf Azure App Service-Web-Apps oder bei einem Hostinganbieter von Drittanbietern, mithilfe von Visual Studio 2012 oder Visual Studio 2010. Weitere Informationen über die Reihe finden Sie unter [im ersten Tutorial der Reihe](introduction.md).


## <a name="overview"></a>Übersicht

Dieses Tutorial veranschaulicht Abrufen des Projekts für die datenbankbereitstellung bereit. Die Struktur der Datenbank und einige (nicht alle) der Daten in der Anwendung zwei Datenbanken müssen für Test-, Staging- und produktionsumgebungen bereitgestellt werden.

In der Regel bei der Entwicklung eine Anwendung geben Sie Testdaten in eine Datenbank, die Sie nicht auf einer live-Website bereitstellen möchten. Jedoch, haben Sie möglicherweise auch einige Produktionsdaten, die Sie bereitstellen möchten. In diesem Tutorial Sie konfigurieren die Contoso University-Projekts und Vorbereiten von SQL-Skripts, damit die richtigen Daten enthalten ist, bei der Bereitstellung.

Erinnerung: Wenn Sie eine Fehlermeldung erhalten, oder etwas nicht funktioniert, wie Sie das Lernprogramm durchzuarbeiten, sollten Sie unbedingt die [Problembehandlungsseite](troubleshooting.md).

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

Die beispielanwendung verwendet die SQL Server Express LocalDB. SQL Server Express ist die kostenlose Edition von SQL Server. Es wird häufig bei der Entwicklung verwendet werden, da es auf die gleiche Datenbank-Engine als Vollversionen von SQL Server basiert. Sie können mit SQL Server Express testen und sicher sein, dass in der Produktion mit wenigen Ausnahmen für Features des Verhaltens der Anwendung, die zwischen SQL Server-Editionen variieren.

LocalDB ist einem speziellen Ausführungsmodus von SQL Server Express, die Ihnen ermöglicht, die Arbeit mit Datenbanken als *mdf* Dateien. LocalDB-Datenbankdateien bleiben in der Regel der *App\_Daten* Ordner eines Webprojekts. Das Feature "Instanz" in SQL Server Express ermöglicht Ihnen die Arbeit mit auch *mdf* Dateien, aber das Feature "Instanz" ist veraltet; LocalDB wird daher empfohlen, für die Arbeit mit *mdf* Dateien.

SQL Server Express wird in der Regel für Produktions-Web-Anwendungen nicht verwendet. LocalDB wird insbesondere nicht mit einer Webanwendung für die Produktion empfohlen, da es nicht ausgelegt ist, arbeiten Sie mit IIS.

In Visual Studio 2012 wird die LocalDB wird standardmäßig mit Visual Studio installiert. In Visual Studio 2010 und früheren Versionen ist das SQL Server Express (ohne LocalDB), die standardmäßig mit Visual Studio installiert. Warum Sie es als eines der erforderlichen Komponenten im installiert [im ersten Lernprogramm dieser Reihe](introduction.md).

Weitere Informationen zu SQL Server-Editionen, einschließlich LocalDB, finden Sie unter den folgenden Ressourcen [arbeiten mit SQL Server-Datenbanken](../../../../whitepapers/aspnet-data-access-content-map.md#sqlserver).

## <a name="entity-framework-and-universal-providers"></a>Entitätsframework und universelle Anbieter

Für den Datenbankzugriff erfordert die Contoso University-Anwendung die folgende Software, die mit der Anwendung bereitgestellt werden muss, weil es nicht in .NET Framework enthalten ist:

- [ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx) (ermöglicht das Mitgliedschaftssystem von ASP.NET mit Azure SQL-Datenbank)
- [Entity Framework](https://msdn.microsoft.com/library/gg696172.aspx)

Da diese Software in NuGet-Paketen enthalten ist, ist das Projekt bereits eingerichtet, sodass mit dem Projekt die erforderlichen Assemblys bereitgestellt werden. (Die Links zeigen Sie auf die aktuellen Versionen dieser Pakete, die möglicherweise höher als erwartet in das Startprojekt ist, die Sie für dieses Tutorial heruntergeladen haben.)

Wenn Sie mit einem Drittanbieter-Hostinganbieter statt Azure bereitstellen, stellen Sie sicher, dass Sie Entitätsframework 5.0 oder höher verwenden. Frühere Versionen von Code First-Migrationen erfordern volle Vertrauenswürdigkeit, und die meisten hosting-Anbieter werden Ihre Anwendung in der mittleren Vertrauensebene ausgeführt. Weitere Informationen zur mittleren Vertrauensebene finden Sie unter den [Bereitstellen in IIS als Testumgebung](deploying-to-iis.md) Tutorial.

## <a name="configure-code-first-migrations-for-application-database-deployment"></a>Konfigurieren von Code First-Migrationen zur anwendungsbereitstellung für die Datenbank

Die Contoso University-Datenbank wird durch Code First verwaltet, und Sie stellen es bereit, mit Code First-Migrationen. Einen Überblick über die Bereitstellung der Datenbank mithilfe von Code First-Migrationen finden Sie unter [im ersten Lernprogramm dieser Reihe](introduction.md).

Bei der Bereitstellung einer Datenbank bereitzustellen nicht in der Regel Sie einfach Ihrer Entwicklungsdatenbank mit allen enthaltenen Daten in die Produktion, da ein Großteil der Daten wahrscheinlich nur für Testzwecke. Beispielsweise sind die Namen "Student" in einer Testdatenbank fiktive. Sie können nicht nur die Struktur der Datenbank mit keine Daten auf der anderen Seite häufig überhaupt bereitstellen. Einige der Daten in der Testdatenbank kann sich um echte Daten sein und muss vorhanden sein, wenn der Benutzer zur Verwendung der Anwendung beginnen. Z. B. möglicherweise Ihre Datenbank eine Tabelle, die Werte für die gültige Dienstqualität oder real Abteilungsnamen enthält.

Um diesem häufigen Szenario zu simulieren, konfigurieren Sie einen Code-First-Migrationen `Seed` -Methode, die nur die Daten in die Datenbank einfügt, die in der produktionsumgebung vorhanden sein sollen. Dies `Seed` Methode sollte nicht Testdaten nicht eingefügt werden, da sie in der Produktion ausgeführt werden soll, nachdem der Code First die Datenbank in der produktionsumgebung erstellt.

In früheren Versionen von Code First, bevor Migrationen kam, war es üblich `Seed` Methoden zum Einfügen Testdaten auch, weil bei jeder modelländerung während der Entwicklung verwendet die Datenbank vollständig löschen und von Grund auf neu erstellt haben. Code First-Migrationen, Test, der Daten, nachdem Änderungen in der Datenbank beibehalten werden, also mit den Testdaten in die `Seed` Methode ist nicht erforderlich. Das Projekt, das Sie heruntergeladen haben verwendet die Methode der einschließlich aller Daten in die `Seed` Methode einer Klasse Initialisierer. In diesem Tutorial werden Sie diese Initialisiererklasse deaktivieren und `enable Migrations. Then you'll update the `Seed "-Methode in der Konfiguration von Migrationen Klasse, sodass nur Daten, die eingefügt, der in der Produktion eingefügt werden soll.

Das folgende Diagramm zeigt das Schema der Datenbank der dienstanwendung:

[![School_database_diagram](preparing-databases/_static/image2.png)](preparing-databases/_static/image1.png)

Für diese Tutorials Sie gehen davon aus, die die `Student` und `Enrollment` Tabellen sollte leer sein, wenn der Standort zuerst bereitgestellt wird. Die anderen Tabellen enthalten Daten, die vorab werden muss, geladen Wenn die Anwendung online geschaltet wird.

### <a name="disable-the-initializer"></a>Deaktivieren Sie den Initialisierer

Da Sie Code First-Migrationen verwenden möchten, müssen Sie nicht verwenden, die `DropCreateDatabaseIfModelChanges` Code First-Initialisierer. Der Code für diese Initialisierer ist in der *SchoolInitializer.cs* Datei im Projekt ContosoUniversity.DAL. Eine Einstellung in der `appSettings` Element der *"Web.config"* führt diese Initialisierer ausgeführt, sobald die Anwendung versucht, Zugriff auf die Datenbank zum ersten Mal:

[!code-xml[Main](preparing-databases/samples/sample1.xml?highlight=3)]

Öffnen Sie die Anwendung *"Web.config"* Datei und entfernen oder kommentieren Sie die `add` Element, das die Code First-Initialisierer-Klasse angibt. Die `appSettings` Element sieht nun wie folgt aus:

[!code-xml[Main](preparing-databases/samples/sample2.xml)]

> [!NOTE]
> Eine andere Möglichkeit, eine Initialisiererklasse anzugeben ist es durch den Aufruf erfolgen `Database.SetInitializer` in die `Application_Start` -Methode in der die *"Global.asax"* Datei. Wenn Sie Migrationen in einem Projekt aktivieren, die diese Methode verwendet wird, um den Initialisierer anzugeben, entfernen Sie diese Codezeile.

> [!NOTE]
> Wenn Sie Visual Studio 2013 verwenden, fügen Sie die folgenden Schritte aus, zwischen den Schritten 2 und 3: (a) In PMC Geben Sie "Update-Package Entityframework-Version 6.1.1" um die aktuelle Version von EF zu erhalten. Und (b) erstellen Sie das Projekt zum Abrufen einer Liste der Buildfehler, und beheben Sie sie. Löschen Sie die using-Anweisungen für Namespaces, die nicht mehr vorhanden sind, mit der rechten Maustaste und klicken Sie auf auflösen, Hinzufügen von using-Anweisungen, wo sie gebraucht werden, und System.Data.Entity.EntityState Vorkommen von System.Data.EntityState nun.

### <a name="enable-code-first-migrations"></a>Aktivieren Sie Code First-Migrationen

1. Stellen Sie sicher, dass das ContosoUniversity-Projekt (nicht ContosoUniversity.DAL) als Startprojekt festgelegt ist. In **Projektmappen-Explorer**mit der rechten Maustaste auf das ContosoUniversity-Projekt, und wählen Sie **als Startprojekt festlegen**. Code First-Migrationen sucht in der Startup-Projekt auf die Datenbank-Verbindungszeichenfolge zu ermitteln.
2. Von der **Tools** Menü wählen **NuGet Package Manager** > **-Paket-Manager-Konsole**.

    ![Selecting_Package_Manager_Console](preparing-databases/_static/image3.png)
3. Am oberen Rand der **-Paket-Manager-Konsole** Fenster auszuwählen ContosoUniversity.DAL als Standardprojekt, und klicken Sie dann an die `PM>` Eingabeaufforderung Geben Sie "Enable-Migrations".

    ![Enable-Migrations-Befehl](preparing-databases/_static/image4.png)

    (Wenn man ein Fehler mit dem Text der *Enable-Migrations-* Befehl nicht erkannt werden, geben Sie den Befehl *Update-Package EntityFramework-installieren* und versuchen Sie es erneut.)

    Dieser Befehl erstellt eine *Migrationen* Ordner im Projekt ContosoUniversity.DAL, und es wird in diesem Ordner zwei Dateien: eine *Configuration.cs* -Datei, die Sie, zum Konfigurieren von Migrationen und eine verwendenkönnen*InitialCreate.cs* -Datei für die erste Migration, der die Datenbank erstellt.

    ![Ordner "Migrations"](preparing-databases/_static/image5.png)

    Auswahl der DAL-Projekt in der **Standardprojekt** Dropdown-Liste von der **-Paket-Manager-Konsole** da die `enable-migrations` -Befehl muss ausgeführt werden, in das Projekt mit dem Code First Context-Klasse. Wenn ein Klassenbibliotheksprojekt diese Klasse ist, sucht Code First-Migrationen für die Datenbank-Verbindungszeichenfolge in das Startprojekt für die Lösung. In der Projektmappe ContosoUniversity wurde das Web-Projekt als Startprojekt festgelegt. Wenn Sie nicht das Projekt zu bestimmen, das die Verbindungszeichenfolge als Startprojekt in Visual Studio hat möchten, können Sie das Startprojekt in der PowerShell-Befehl angeben. Geben Sie den Befehl, um die Befehlssyntax anzuzeigen, `get-help enable-migrations`.

    Die `enable-migrations` Befehl automatisch die erste Migration erstellt, da die Datenbank bereits vorhanden ist. Eine Alternative ist damit für Migrationen, die die Datenbank zu erstellen. Verwenden Sie zu diesem Zweck **Server-Explorer** oder **Objekt-Explorer von SQL Server** , die ContosoUniversity-Datenbank zu löschen, bevor Sie die Migration aktivieren. Nach der Aktivierung von Migrationen erstellen Sie die erste Migration manuell mithilfe des Befehls "add-Migration InitialCreate". Sie können dann die Datenbank erstellen, durch den Befehl "Update-Database" eingeben.

### <a name="set-up-the-seed-method"></a>Richten Sie die Seed-Methode

Für dieses Tutorial werden Sie Festplattenlaufwerke durch Hinzufügen von Code zum Hinzufügen der `Seed` -Methode der Code-First-Migrationen `Configuration` Klasse. Code First-Migrationen Ruft die `Seed` -Methode auf, nachdem jede Migration.

Da die `Seed` Methode nach jeder Migration ausgeführt wird, gibt es Daten bereits in den Tabellen nach der ersten Migration. Um diese Situation zu behandeln, Sie verwenden, die `AddOrUpdate` Methode zum Aktualisieren von Zeilen, die bereits eingefügt wurde, oder fügen Sie sie an, wenn sie noch nicht vorhanden sind. Die `AddOrUpdate` Methode möglicherweise nicht die beste Wahl für Ihr Szenario. Weitere Informationen finden Sie unter [kümmern mit EF 4.3 AddOrUpdate Methode](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) Julie lermans Blog.

1. Öffnen der *Configuration.cs* Datei, und Ersetzen Sie die Kommentare in der `Seed` Methode durch den folgenden Code:

    [!code-csharp[Main](preparing-databases/samples/sample3.cs)]
2. Die Verweise auf `List` rote Wellenlinien unter diesen haben, da Sie keine `using` noch-Anweisung für den Namespace. Mit der rechten Maustaste eine der Instanzen des `List` , und klicken Sie auf **beheben**, und klicken Sie dann auf **using System.Collections.Generic**.

    ![Lösen Sie mit der using-Anweisung](preparing-databases/_static/image6.png)

    Diese Menüoption fügt den folgenden Code der `using` Anweisungen am oberen Rand der Datei.

    [!code-csharp[Main](preparing-databases/samples/sample4.cs)]
3. Drücken Sie STRG + UMSCHALT + B, um das Projekt erstellen.

Das Projekt ist jetzt bereit für die Bereitstellung der *ContosoUniversity* Datenbank. Nach der Bereitstellung der Anwendung, die zum ersten Mal ausführen, und navigieren Sie zu einer Seite, die Zugriff auf die Datenbank, Code First erstellt die Datenbank und führen Sie diesen `Seed` Methode.

> [!NOTE]
> Hinzufügen von Code, der `Seed` Methode ist eine von vielen Möglichkeiten, die Sie in die Datenbank "Festplattenlaufwerk" einfügen können. Eine Alternative zum Hinzufügen von Code ist die `Up` und `Down` Methoden jede Migration-Klasse. Die `Up` und `Down` Methoden enthalten Code, der Änderungen in der Datenbank implementiert. Sehen Sie Beispiele für die sie in der [Bereitstellen eines Datenbankupdates](deploying-a-database-update.md) Tutorial.
> 
> Sie können auch Schreiben Code, der SQL-Anweisungen, mithilfe ausführt der `Sql` Methode. Sie z. B., wenn Sie die Department-Tabelle eine Budgetspalte hinzugefügt wurden und alle Abteilung Budgets zu 1.000,00 $ als Teil einer Migration initialisieren möchten, können Hinzufügen der folgenden Codezeile den `Up` Methode für diese Migration:
> 
> `Sql("UPDATE Department SET Budget = 1000");`


## <a name="create-scripts-for-membership-database-deployment"></a>Erstellen von Skripts für die Bereitstellung der Mitgliedschaft

Die Contoso University-Anwendung verwendet die ASP.NET Membership-System und Forms-Authentifizierung zum Authentifizieren und Autorisieren von Benutzern. Die **Update Gutschriften** Seite nur für Benutzer, die in der Rolle "Administrator" möglich ist.

Führen Sie die Anwendung, und klicken Sie auf **Kurse**, und klicken Sie dann auf **Update Gutschriften**.

![Klicken Sie auf die Update-Guthaben](preparing-databases/_static/image7.png)

Die **melden Sie sich bei** Seite wird angezeigt, da die **Update Gutschriften** Seite sind Administratorrechte erforderlich.

Geben Sie *Admin* als Benutzernamen ein und *Devpwd* als Kennwort ein, und klicken Sie auf **melden Sie sich bei**.

![Anmeldeseite](preparing-databases/_static/image8.png)

Die **Update Gutschriften** Seite wird angezeigt.

![Seite zum Aktualisieren des Gutschriften](preparing-databases/_static/image9.png)

Benutzer-und Rolle befindet sich in der *Aspnet-ContosoUniversity* -Datenbank, die angegeben wird die **DefaultConnection** Verbindungszeichenfolge in der *"Web.config"* Datei.

Diese Datenbank wird nicht von Entity Framework Code First, verwaltet, sodass Sie Migrationen verwenden können, bereitgestellt. Verwenden Sie den DbDacFx-Anbieter, um das Datenbankschema bereitzustellen, und konfigurieren Sie das Veröffentlichungsprofil zum Ausführen eines Skripts, mit dem erste Daten in Datenbanktabellen eingefügt wird.

> [!NOTE]
> Eine neue ASP.NET-Mitgliedschaftssystem (jetzt als ASP.NET Identity) wurde mit Visual Studio 2013 eingeführt. Das neue System können Sie sowohl die Anwendung als auch die Mitgliedertabellen in der gleichen Datenbank zu speichern, und Sie können Code First-Migrationen verwenden, um beide bereitzustellen. Die beispielanwendung verwendet die früheren ASP.NET-Mitgliedschaftssystem, der nicht bereitgestellt werden kann, mithilfe von Code First-Migrationen. Die Verfahren zum Bereitstellen dieser Mitgliedschaftsdatenbank gelten auch für alle anderen Szenarien, in dem Ihre Anwendung, die zum Bereitstellen einer SQL Server-Datenbank, die nicht von Entity Framework Code First erstellt wird, muss, verwendet werden.


Hier nicht zu, in der Regel die gleichen Daten in der Produktion werden sollen, die Sie in der Entwicklung haben. Wenn Sie einen Standort zum ersten Mal bereitstellen, ist es üblich, schließen Sie die meisten oder alle der Benutzerkonten, die Sie für Testzwecke zu erstellen. Aus diesem Grund hat das heruntergeladene Projekt zwei Datenbankrollenmitgliedschaften: *Aspnet-ContosoUniversity.mdf* mit-entwicklungsbenutzer und *Aspnet-ContosoUniversity-Prod.mdf* mit aktiver Benutzer. In diesem Tutorial werden die Namen in beiden Datenbanken identisch: *Admin* und *Nonadmin*. Beide Benutzer haben Sie das Kennwort *Devpwd* in der Entwicklungsdatenbank und *Prodpwd* in der Produktionsdatenbank.

Sie müssen die entwicklungsbenutzer in der testumgebung und der Produktionsbenutzer zu Staging und Produktion bereitstellen. Dazu erstellen Sie zwei SQL-Skripts in diesem Tutorial, eine für die Entwicklung und eine für die Produktion, und konfigurieren Sie in späteren Tutorials des Veröffentlichungsprozesses, um sie auszuführen.

> [!NOTE]
> Die Mitgliedschaftsdatenbank speichert einen Hash der Kennwörter. Zum Bereitstellen von Konten, die von einem Computer zu einem anderen müssen Sie sicherstellen, dass hashing Routinen verschiedenen Hashes auf dem Zielserver kein generieren, als auf dem Quellcomputer. Sie werden die gleichen Hashwerte generieren, bei der Verwendung der ASP.NET Universal Providers, solange Sie nicht, dass den Standard-Algorithmus ändern. Der Standardalgorithmus ist HMACSHA256 und wird angegeben, der **Überprüfung** Attribut der **[MachineKey](https://msdn.microsoft.com/library/system.web.configuration.machinekeysection.aspx)** Element in der Datei "Web.config".


Sie können Daten von Bereitstellungsskripts manuell, mithilfe von SQL Server Management Studio (SSMS) oder mithilfe eines Tools von Drittanbietern erstellen. Diese weiteren Verlauf dieses Tutorials erfahren Sie, wie Sie ihn in SSMS, aber wenn Sie nicht möchten, installieren und Verwenden von SSMS können Sie das Abrufen der Skripts aus der abgeschlossenen Version des Projekts und fahren Sie mit Abschnitt, in dem Sie sie in den Projektmappenordner speichern.

Zum Installieren von SSMS, installieren Sie sie über [Download Center: Microsoft SQL Server 2012 Express](https://www.microsoft.com/download/details.aspx?id=29062) durch Klicken auf [ENU\x64\SQLManagementStudio\_X64\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x64/SQLManagementStudio_x64_ENU.exe) oder [ ENU\x86\SQLManagementStudio\_X86\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x86/SQLManagementStudio_x86_ENU.exe). Wenn Sie die falsche Version für Ihr System es kann nicht installiert und können Sie versuchen, der andere Controller.

(Beachten Sie, dass es sich um einen Download 600 MB handelt. Es dauert sehr lange installieren und macht einen Neustart des Computers erforderlich.)

Klicken Sie auf der ersten Seite des SQL Server-Installationscenter auf **eigenständige neue SQL Server-Installation oder Hinzufügen von Funktionen zu einer vorhandenen Installation**, und befolgen Sie die Anweisungen, übernehmen die Standardoptionen.

### <a name="create-the-development-database-script"></a>Erstellen von Datenbankskripts Entwicklung

1. Führen Sie SSMS.
2. In der **Herstellen einer Verbindung mit Server** Dialogfeld Geben Sie *(Localdb) \v11.0* als die **Servernamen**, lassen Sie **Authentifizierung** festgelegt**Windows-Authentifizierung**, und klicken Sie dann auf **Connect**.

    ![SSMS Verbindung mit Server herstellen.](preparing-databases/_static/image10.png)
3. In der **Objekt-Explorer** Fenster erweitern **Datenbanken**, mit der rechten Maustaste **Aspnet-ContosoUniversity**, klicken Sie auf **Aufgaben**, und klicken Sie dann auf **Generieren von Skripts**.

    ![SSMS-Skripts generieren](preparing-databases/_static/image11.png)
4. In der **generieren und Veröffentlichen von Skripts** Dialogfeld klicken Sie auf **Skripterstellungsoptionen festlegen**.

    Sie können fortfahren die **Objekte auswählen** Schritt, da die Standardeinstellung ist **Skripterstellung für gesamte Datenbank und alle Datenbankobjekte** erwünscht ist.
5. Klicken Sie auf **erweiterte**.

    ![SSMS Skripterstellungsoptionen festlegen](preparing-databases/_static/image12.png)
6. In der **erweiterte Skripterstellungsoptionen** führen Sie einen Bildlauf nach unten, um Sie im Dialogfeld **Typen Skript**, und klicken Sie auf die **nur Daten** Option in der Dropdown-Liste.
7. Änderung **Skripterstellung für USE DATABASE** zu **"false"**. USE-Anweisungen für Azure SQL-Datenbank nicht gültig und werden nicht für die Bereitstellung auf SQL Server Express in der testumgebung benötigt.

    ![SSMS Skript nur Daten keine USE-Anweisung](preparing-databases/_static/image13.png)
8. Klicken Sie auf **OK**.
9. In der **generieren und Veröffentlichen von Skripts** im Dialogfeld die **Dateiname** Feld gibt an, in dem das Skript erstellt wird. Ändern Sie den Pfad des Projektmappenordners gespeichert (der Ordner, die Ihre ContosoUniversity.sln-Datei) und der Dateiname, *Aspnet-Daten-dev.sql*.
10. Klicken Sie auf **Weiter** fahren Sie mit der **Zusammenfassung** Registerkarte, und klicken Sie dann auf **Weiter** erneut aus, um das Skript zu erstellen.

    ![SSMS-Skripts, die erstellt wurden](preparing-databases/_static/image14.png)
11. Klicken Sie auf **Fertig stellen**.

### <a name="create-the-production-database-script"></a>Erstellen Sie das Skript der Datenbank

Da Sie das Projekt mit der Produktionsdatenbank ausgeführt haben, ist nicht es noch die LocalDB-Instanz zugewiesen. Aus diesem Grund müssen Sie zuerst das Anfügen der Datenbank.

1. In SSMS **Objekt-Explorer**, mit der rechten Maustaste **Datenbanken** , und klicken Sie auf **Anfügen**.

    ![Anfügen von SSMS](preparing-databases/_static/image15.png)
2. In der **Datenbanken anfügen** Dialogfeld klicken Sie auf **hinzufügen** und navigieren Sie zu der *Aspnet-ContosoUniversity-Prod.mdf* Datei die *App\_ Daten* Ordner.

     ![Hinzufügen von SSMS MDF-Datei anfügen](preparing-databases/_static/image16.png)
3. Klicken Sie auf **OK**.
4. Führen Sie das gleiche Verfahren, die Sie zuvor ein Skript für die Produktion-Datei erstellt. Die Skriptdatei *Aspnet-Daten-prod.sql*.

## <a name="summary"></a>Zusammenfassung

Beide Datenbanken sind jetzt bereit für die Bereitstellung aus, und Sie haben zwei Bereitstellungsskripts für Daten im Projektmappenordner.

![Daten-Bereitstellungsskripts](preparing-databases/_static/image17.png)

In dem folgenden Tutorial konfigurieren Sie die projekteinstellungen, die Bereitstellung auswirken, und Sie richten Sie automatische *"Web.config"* konfigurationsdateitransformationen für Einstellungen, die in der bereitgestellten Anwendung unterscheiden müssen.

## <a name="more-information"></a>Weitere Informationen

Weitere Informationen zu NuGet, finden Sie unter [Verwalten von Projektbibliotheken mit NuGet](https://msdn.microsoft.com/magazine/hh547106.aspx) und [NuGet-Dokumentation](http://docs.nuget.org/docs/start-here/overview). Wenn Sie nicht NuGet verwenden möchten, müssen Sie erfahren, wie Sie ein NuGet-Paket, um zu bestimmen, welche Aktion er ausführt, bei der Installation zu analysieren. (sie können z. B. konfigurieren *"Web.config"* Transformationen, PowerShell-Skripts zur Ausführung auf den Zeitpunkt der Erstellung usw. konfigurieren.) Weitere Informationen zur Funktionsweise von NuGet finden Sie unter [erstellen und Veröffentlichen eines Pakets](http://docs.nuget.org/docs/creating-packages/creating-and-publishing-a-package) und [Konfigurationsdatei und Quellcodetransformationen](http://docs.nuget.org/docs/creating-packages/configuration-file-and-source-code-transformations).

> [!div class="step-by-step"]
> [Zurück](introduction.md)
> [Weiter](web-config-transformations.md)
