---
uid: web-forms/overview/deployment/visual-studio-web-deployment/preparing-databases
title: "ASP.NET Web-Bereitstellung mit Visual Studio: Vorbereiten für die Bereitstellung der Datenbank | Microsoft Docs"
author: tdykstra
description: "Diese Reihe von Lernprogrammen wird gezeigt, wie bereitstellen (veröffentlichen) aus einer ASP.NET web-Anwendung auf Azure App Service-Web-Apps oder mit einem Hostinganbieter von Drittanbietern durch wählen..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: ae4def81-fa37-4883-a13e-d9896cbf6c36
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/preparing-databases
msc.type: authoredcontent
ms.openlocfilehash: caa79725ede320c4bd3e87ac246966c57175eb8e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-web-deployment-using-visual-studio-preparing-for-database-deployment"></a>ASP.NET Web-Bereitstellung mit Visual Studio: Vorbereiten für die Bereitstellung der Datenbank
====================
Durch [Tom Dykstra](https://github.com/tdykstra)

[Startprojekt herunterladen](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Diese Reihe von Lernprogrammen wird gezeigt, wie bereitstellen (veröffentlichen) aus einer ASP.NET web-Anwendung eines Drittanbieters Hostinganbieter oder Azure App Service-Web-Apps mithilfe von Visual Studio 2012 oder Visual Studio 2010. Informationen über die Reihen finden Sie unter [im ersten Lernprogramm, in der Reihe](introduction.md).


## <a name="overview"></a>Übersicht

In diesem Lernprogramm wird gezeigt, wie zum Abrufen des Projekts für die datenbankbereitstellung bereit. Der Datenbankstruktur und einige (nicht alle) der Daten in der Anwendung zwei Datenbanken müssen für Test-, Staging- und produktionsumgebungen bereitgestellt werden.

In der Regel, wie Sie eine Anwendung entwickeln, geben Sie Testdaten in eine Datenbank, die Sie nicht auf einem live-Website bereitstellen möchten. Jedoch, müssen Sie möglicherweise auch einige Produktionsdaten, die Sie bereitstellen möchten. In diesem Lernprogramm Sie konfigurieren Sie das Projekt Contoso University und Vorbereiten von SQL-Skripts so, dass die richtigen Daten enthalten sind, bei der Bereitstellung.

Hinweis: Wenn Sie eine Fehlermeldung erhalten, oder etwas funktioniert nicht, wenn Sie das Lernprogramm absolvieren, müssen Sie überprüfen die [Website](troubleshooting.md).

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

Die beispielanwendung verwendet SQL Server Express LocalDB. SQL Server Express ist die kostenlose Edition von SQL Server. Sie wird häufig während der Entwicklung verwendet werden, da er auf das gleiche Datenbankmodul als Vollversionen von SQL Server basiert. Sie können mit SQL Server Express testen und sicher sein, dass es sich bei dem in der Produktion mit wenigen Ausnahmen, die für Features des Verhaltens der Anwendung, die zwischen SQL Server-Editionen variieren.

LocalDB ist eine spezielle Ausführungsmodus von SQL Server Express, die Ihnen ermöglicht, die Arbeit mit Datenbanken als *mdf* Dateien. LocalDB-Datenbankdateien bleiben in der Regel der *App\_Daten* Ordner eines Webprojekts. Die Benutzerinstanzfunktion in SQL Server Express können Sie zur Bearbeitung auch *mdf* Dateien, aber der Benutzerinstanzfunktion wird als veraltet markiert; daher LocalDB wird empfohlen, für die Arbeit mit *mdf* Dateien.

SQL Server Express wird in der Regel für die Produktion Webanwendungen nicht verwendet. LocalDB ist mit einer Webanwendung für die Produktion insbesondere nicht empfohlen, da es nicht zum Arbeiten mit IIS ausgelegt ist.

In Visual Studio 2012 ist LocalDB, die standardmäßig mit Visual Studio installiert. In Visual Studio 2010 und frühere Versionen ist das SQL Server Express (ohne LocalDB) standardmäßig mit Visual Studio installiert. d. h. warum Sie es als eine der Voraussetzungen im installiert [im ersten Lernprogramm dieser Reihe](introduction.md).

Weitere Informationen zu SQL Server-Editionen, einschließlich LocalDB, finden Sie unter den folgenden Ressourcen [arbeiten mit SQL Server-Datenbanken](../../../../whitepapers/aspnet-data-access-content-map.md#sqlserver).

## <a name="entity-framework-and-universal-providers"></a>Entity Framework und Universal Providers

Für den Datenbankzugriff erfordert die Universität von Contoso-Anwendung die folgende Software, die mit der Anwendung bereitgestellt werden, da er nicht in .NET Framework enthalten ist:

- [ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx) (ermöglicht das ASP.NET-Mitgliedschaftssystem mit Azure SQL-Datenbank)
- [Entity Framework](https://msdn.microsoft.com/library/gg696172.aspx)

Da diese Software in NuGet-Pakete enthalten ist, wird das Projekt bereits eingerichtet, damit, dass die erforderlichen Assemblys mit dem Projekt bereitgestellt werden. (Die Links zeigen Sie auf die aktuellen Versionen dieser Pakete, die möglicherweise aktuellere, was im Startprojekt installiert ist, die Sie für dieses Lernprogramm heruntergeladen haben.)

Wenn Sie mit einem Drittanbieter-Hostinganbieter anstelle von Azure bereitstellen, stellen Sie sicher, dass Sie Entity Framework 5.0 oder höher verwenden. Frühere Versionen von Code First-Migrationen erfordern vollständige Vertrauenswürdigkeit aus, und die meisten Hostinganbieter werden Ihre Anwendung in mit mittlerer Vertrauenswürdigkeit ausgeführt. Weitere Informationen zu mit mittlerer Vertrauenswürdigkeit, finden Sie unter der [in IIS als Testumgebung bereitstellen](deploying-to-iis.md) Lernprogramm.

## <a name="configure-code-first-migrations-for-application-database-deployment"></a>Konfigurieren von Code First-Migrationen für die Bereitstellung der Datenbank

Die Anwendungsdatenbank Contoso Universität von Code First verwaltet wird, und stellen Sie es mithilfe von Code First-Migrationen. Einen Überblick über die Bereitstellung mithilfe von Code First-Migrationen finden Sie unter [im ersten Lernprogramm dieser Reihe](introduction.md).

Beim Bereitstellen einer Anwendungsdatenbank nicht in der Regel Sie einfach Ihre Entwicklungsdatenbank mit allen enthaltenen Daten bis hin zur Produktion, bereitgestellt werden, da ein Großteil der Daten wahrscheinlich besteht ausschließlich für Testzwecke vorgesehen. Beispielsweise sind die Student-Namen in eine Testdatenbank fiktive. Sie können nicht nur die Datenbankstruktur ohne Daten darin andererseits, häufig überhaupt bereitstellen. Einige der Daten in der Testdatenbank möglicherweise um echte Daten und muss vorhanden sein, wenn der Benutzer zur Verwendung der Anwendung zu starten. Z. B. möglicherweise die Datenbank eine Tabelle, die gültige Grade Werten oder echte Abteilungsnamen enthält.

Um dieses gängige Szenario zu simulieren, konfigurieren Sie ein Code First-Migrationen `Seed` Methode, die nur die Daten in die Datenbank einfügt, die in der Produktion vorhanden sein sollen. Dies `Seed` Methode darf keine Testdaten nicht eingefügt werden, da es in der produktionsumgebung ausgeführt wird, nachdem die Datenbank in der Produktion Code First erstellt.

In früheren Versionen von Code First bevor Migrationen freigegeben wurde, war es üblich `Seed` einzufügende Testmethoden Daten auch, da bei jeder Änderung Modell während der Entwicklung die Datenbank mussten vollständig gelöscht und von Grund auf neu erstellt werden. Mit Code First-Migrationen Test Daten, nachdem Änderungen an der Datenbank beibehalten werden, einschließlich also Testdaten in die `Seed` Methode ist nicht erforderlich. Das Projekt, das Sie heruntergeladen haben verwendet die Methode der einschließlich aller Daten in der `Seed` Methode einer Klasse Initialisierer. In diesem Lernprogramm deaktivieren Sie diese Initialisiererklasse und `enable Migrations. Then you'll update the `Ausgangswert '-Methode in der migrationskonfiguration Klasse, sodass es nur Daten einfügt, die in der Produktion eingefügt werden soll.

Das folgende Diagramm zeigt das Schema der Datenbank der dienstanwendung:

[![School_database_diagram](preparing-databases/_static/image2.png)](preparing-databases/_static/image1.png)

Für diese Lernprogramme, Sie gehen davon aus, die die `Student` und `Enrollment` Tabellen sollte leer sein, wenn der Standort zuerst bereitgestellt wird. Die anderen Tabellen enthalten Daten, die vorab geladen werden, wenn die Anwendung online geschaltet wird.

### <a name="disable-the-initializer"></a>Deaktivieren Sie den Initialisierer

Da Sie Code First-Migrationen verwenden, müssen Sie nicht verwenden die `DropCreateDatabaseIfModelChanges` Code First Initialisierer. Der Code für diese Initialisierer ist in der *SchoolInitializer.cs* Datei im Projekt ContosoUniversity.DAL. Eine Einstellung in der `appSettings` Element von der *"Web.config"* Datei führt dazu, dass diese Initialisierer ausgeführt, sobald die Anwendung versucht, Zugriff auf die Datenbank zum ersten Mal:

[!code-xml[Main](preparing-databases/samples/sample1.xml?highlight=3)]

Öffnen Sie die Anwendung *"Web.config"* Datei, und entfernen, oder kommentieren Sie Sie aus der `add` Element, das die Code First Initialisiererklasse angibt. Die `appSettings` Element sieht nun wie folgt:

[!code-xml[Main](preparing-databases/samples/sample2.xml)]

> [!NOTE]
> Alternative Möglichkeit zum Angeben einer Initialisiererklasse ist, führen Sie es durch den Aufruf `Database.SetInitializer` in der `Application_Start` Methode in der *"Global.asax"* Datei. Wenn Sie Migrationen in einem Projekt aktivieren, die diese Methode verwendet wird, um den Initialisierer anzugeben, entfernen Sie diese Codezeile.


> [!NOTE]
> Wenn Sie Visual Studio 2013 verwenden, fügen Sie die folgenden Schritte aus, zwischen den Schritten 2 und 3: (a) In PMC Geben Sie "Update-Paket Entityframework-Version 6.1.1" zum Abrufen der aktuellen Version von EF. Und (b) Build des Projekts zum Abrufen einer Liste der Buildfehler und zu korrigieren. Löschen Sie die using-Anweisungen für Namespaces, die nicht mehr vorhanden sind, mit der rechten Maustaste und klicken Sie auf auflösen, Hinzufügen von using-Anweisungen, in denen sie erforderlich sind, und Ändern von Vorkommen des System.Data.EntityState in System.Data.Entity.EntityState.


### <a name="enable-code-first-migrations"></a>Aktivieren Sie die Code First-Migrationen

1. Stellen Sie sicher, dass das ContosoUniversity-Projekt (nicht ContosoUniversity.DAL) als Startprojekt festgelegt ist. In **Projektmappen-Explorer**mit der rechten Maustaste auf das ContosoUniversity-Projekt, und wählen Sie **als Startprojekt festlegen**. Code First-Migrationen sieht in das Startup-Projekt auf die Datenbank-Verbindungszeichenfolge gefunden.
2. Aus der **Tools** Menü klicken Sie auf **Bibliothekspaket-Manager** (oder **NuGet Package Manager**) und dann **Package Manager Console**.

    ![Selecting_Package_Manager_Console](preparing-databases/_static/image3.png)
3. Am oberen Rand der **Package Manager Console** Fenster auswählen ContosoUniversity.DAL als Standardprojekt und anschließend an die `PM>` Eingabeaufforderung Geben Sie "Enable-Migrations".

    ![Enable-Migrations-Befehl](preparing-databases/_static/image4.png)

    (Wenn Sie die Fehlermeldung erhalten die *Enable-Migrations-* Befehl nicht erkannt wird, geben Sie den Befehl *Updatepaket EntityFramework-Neuinstallation* und versuchen Sie es erneut.)

    Dieser Befehl erstellt eine *Migrationen* Ordner im Projekt ContosoUniversity.DAL, und es wird in diesem Ordner zwei Dateien: einer *"Configuration.cs"* -Datei, die Sie, zum Konfigurieren von Migrationen und ein verwendenkönnen*InitialCreate.cs* -Datei für die erste Migration, die die Datenbank erstellt.

    ![Ordner](preparing-databases/_static/image5.png)

    Auswahl der DAL-Projekt in der **Standardprojekt** Dropdown-Liste der **Package Manager Console** da die `enable-migrations` -Befehl muss ausgeführt werden, in das Projekt mit Code First Context-Klasse. Wenn diese Klasse in ein Klassenbibliotheksprojekt, gesucht Code First-Migrationen für die Datenbank-Verbindungszeichenfolge in das Startprojekt für die Projektmappe. In der Projektmappe ContosoUniversity wurde das Projekt als Startprojekt festgelegt. Wenn Sie das Projekt festlegen, der die Verbindungszeichenfolge wie das Startup-Projekt in Visual Studio verwenden möchten, können Sie das Startprojekt in der PowerShell-Befehl angeben. Um die Befehlssyntax anzuzeigen, geben Sie den Befehl `get-help enable-migrations`.

    Die `enable-migrations` Befehl wird automatisch die erste Migration erstellt, da die Datenbank bereits vorhanden ist. Eine Alternative besteht darin Migrationen, die die Datenbank erstellt haben. Verwenden Sie zu diesem Zweck **Server-Explorer** oder **Objekt-Explorer von SQL Server** ContosoUniversity-Datenbank zu löschen, bevor Sie Migrationen aktivieren. Nach der Aktivierung von Migrationen erstellen Sie die erste Migration manuell durch Eingabe des Befehls "hinzufügen Migration InitialCreate". Klicken Sie dann können Sie die Datenbank erstellen, indem Sie den Befehl "Update-Database" eingeben.

### <a name="set-up-the-seed-method"></a>Einrichten der Seed-Methode

Für dieses Lernprogramm fügen Sie eingebauten durch Hinzufügen von Code, der `Seed` Methode von Code First-Migrationen `Configuration` Klasse. Code First-Migrationen Ruft die `Seed` Methode nach jeder Migration.

Da die `Seed` Methode wird nach jeder Migration ausgeführt, es befinden sich Daten bereits in den Tabellen nach der ersten Migration. Um diese Situation zu behandeln, Sie verwenden, die `AddOrUpdate` Methode zum Aktualisieren von Zeilen, die bereits eingefügt wurde, oder fügen Sie sie an, wenn sie noch nicht vorhanden sind. Die `AddOrUpdate` Methode ist möglicherweise nicht die beste Wahl für Ihr Szenario. Weitere Informationen finden Sie unter [Achten Sie darauf mit EF 4.3 AddOrUpdate Methode](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) Julie Lerman Blog.

1. Öffnen der *"Configuration.cs"* -Datei und Ersetzen Sie die Kommentare in der `Seed` -Methode durch folgenden Code:

    [!code-csharp[Main](preparing-databases/samples/sample3.cs)]
2. Die Verweise auf `List` rote Wellenlinien unter sich haben, da Sie keine `using` noch-Anweisung für den Namespace. Mit der rechten Maustaste eine der Instanzen des `List` , und klicken Sie auf **beheben**, und klicken Sie dann auf **using System.Collections.Generic**.

    ![Mit der Anweisung beheben](preparing-databases/_static/image6.png)

    Diese Menüauswahl fügt den folgenden Code, der `using` Anweisungen am oberen Rand der Datei.

    [!code-csharp[Main](preparing-databases/samples/sample4.cs)]
3. Drücken Sie STRG-UMSCHALT + B, um das Projekt zu erstellen.

Das Projekt ist nun bereit für die Bereitstellung der *ContosoUniversity* Datenbank. Nachdem Sie die Anwendung zum ersten Mal bereitstellen, führen Sie ihn, und navigieren Sie zu einer Seite, die Zugriff auf die Datenbank, Code First wird die Datenbank erstellen und führen Sie diesen `Seed` Methode.

> [!NOTE]
> Hinzufügen von Code, der `Seed` Methode ist eine von vielen Möglichkeiten, die Sie in der Datenbank eingebauten einfügen können. Eine Alternative besteht darin, zum Hinzufügen von Code die `Up` und `Down` Methoden jeder Migration-Klasse. Die `Up` und `Down` Methoden Code enthalten, der Änderungen an der Datenbank implementiert. Sie finden Beispiele dafür, in der [Bereitstellen eines Datenbankupdates](deploying-a-database-update.md) Lernprogramm.
> 
> Kann auch Code schreiben, der SQL-Anweisungen mit führt die `Sql` Methode. Z. B. Wenn Sie die Department-Tabelle eine Budgetspalte hinzugefügt wurden und alle Abteilung Budgets 1.000,00 $ als Teil der Migration eines initialisieren möchten, können Sie hinzufügen die folgenden Zeile des Codes für die `Up` Methode für die diese Migration:
> 
> `Sql("UPDATE Department SET Budget = 1000");`


## <a name="create-scripts-for-membership-database-deployment"></a>Erstellen von Skripts für die Bereitstellung des Mitgliedschaft

Die University Contoso-Anwendung verwendet die ASP.NET Membership System- und Forms-Authentifizierung zum Authentifizieren und Autorisieren von Benutzern. Die **Update Gutschriften** Seite ist nur für Benutzer, die in der Rolle "Administrator" sind verfügbar.

Führen Sie die Anwendung, und klicken Sie auf **Kurse**, und klicken Sie dann auf **Update Gutschriften**.

![Klicken Sie auf die Update-Gutschriften](preparing-databases/_static/image7.png)

Die **melden Sie sich** Seite wird angezeigt, da die **Update Gutschriften** Seite sind Administratorrechte erforderlich.

Geben Sie *Admin* als Benutzernamen ein und *Devpwd* als Kennwort und auf **melden Sie sich**.

![Melden Sie sich auf der Seite](preparing-databases/_static/image8.png)

Die **Update Gutschriften** Seite wird angezeigt.

![Guthaben-Seite "Aktualisieren"](preparing-databases/_static/image9.png)

Benutzer-und Rolle befindet sich in der *Aspnet-ContosoUniversity* Datenbank, die von angegeben wird die **DefaultConnection** Verbindungszeichenfolge in der *"Web.config"* Datei.

Diese Datenbank wird nicht von Entity Framework Code First verwaltet, damit Sie Migrationen nutzen können, um es bereitzustellen. Den DbDacFx-Anbieter verwenden, um das Datenbankschema bereitzustellen, und Sie konfigurieren das Veröffentlichungsprofil zum Ausführen eines Skripts, das ursprüngliche Daten in Datenbanktabellen eingefügt wird.

> [!NOTE]
> Eine neue ASP.NET-Mitgliedschaftssystem (jetzt unter dem Namen ASP.NET Identity) wurde zusammen mit Visual Studio 2013 eingeführt. Das neue System ermöglicht es Ihnen, Anwendung und Mitgliedertabellen in derselben Datenbank beibehalten und können Sie Code First-Migrationen sowohl bereitstellen. Die beispielanwendung verwendet die frühere ASP.NET-Mitgliedschaftssystem, die mithilfe von Code First-Migrationen bereitgestellt werden kann. Die Schritte zum Bereitstellen dieser Mitgliedschaftsdatenbank gelten auch für allen anderen Szenarien, in dem Ihre Anwendung muss eine SQL Server-Datenbank bereitstellen, die von Entity Framework Code First erstellt wird.


Hier nicht zu, in der Regel die gleichen Daten in der Produktion werden sollen, die Ihnen bei der Entwicklung. Wenn Sie einen Standort zum ersten Mal bereitstellen, wird häufig ausschließen, die meisten oder alle Benutzerkonten, die Sie zum Testen erstellen. Deshalb hat die heruntergeladene Projekt zwei Mitgliedschaft Datenbanken: *Aspnet-ContosoUniversity.mdf* mit entwicklungsbenutzer und *Aspnet-ContosoUniversity-Prod.mdf* mit produktionsbenutzern. Für dieses Lernprogramm die Benutzernamen in beiden Datenbanken identisch sind: *Admin* und *Nonadmin*. Beide Benutzer haben Sie das Kennwort *Devpwd* in der Entwicklungsdatenbank und *Prodpwd* in der Produktionsdatenbank.

Die Development-Benutzer stellen Sie die Umgebung und die Produktionsbenutzer in Staging und Produktion. Dazu erstellen Sie zwei SQL-Skripts in diesem Lernprogramm: eine für die Entwicklung und eine für die Produktion und in späteren Lernprogrammen konfigurieren Sie des Veröffentlichungsprozesses, um sie auszuführen.

> [!NOTE]
> Die Mitgliedschaftsdatenbank speichert einen Hash der Kennwörter. Zum Bereitstellen von Konten auf einem Computer zu einem anderen müssen Sie sicherstellen, dass hashing Routinen unterschiedliche Hashes auf dem Zielserver kein generieren, als auf dem Quellcomputer ausgeführt. Sie gleichen Hashes generiert bei der Verwendung der ASP.NET Universal Providers, solange Sie nicht den standardmäßigen Algorithmus ändern. Der Standardalgorithmus ist HMACSHA256 und angegeben wird, der **Überprüfung** Attribut von der  **[MachineKey](https://msdn.microsoft.com/library/system.web.configuration.machinekeysection.aspx)**  Element in der Datei "Web.config".


Sie können Skripts für die Bereitstellung von Daten manuell, mithilfe von SQL Server Management Studio (SSMS) oder mithilfe eines Drittanbietertools erstellen. Diese Rest dieses Lernprogramms erfahren Sie, wie es in SSMS, aber wenn Sie nicht möchten, installieren und Verwenden von SSMS können Sie die Skripts aus die abgeschlossene Version des Projekts abrufen und fahren Sie mit Abschnitt, in dem Sie sie im Projektmappenordner gespeichert.

Zum Installieren von SSMS, installieren Sie ihn von [Download Center: Microsoft SQL Server 2012 Express](https://www.microsoft.com/download/details.aspx?id=29062) durch Klicken auf [ENU\x64\SQLManagementStudio\_X64\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x64/SQLManagementStudio_x64_ENU.exe) oder [ ENU\x86\SQLManagementStudio\_X86\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x86/SQLManagementStudio_x86_ENU.exe). Falls die falsche Datei gewünscht für Ihr System schlägt fehl installieren und Sie können versuchen, eine andere.

(Beachten Sie, dass dies ein Download 600 MB ist. Es kann sehr lange dauern installiert und erfordern einen Neustart des Computers.)

Klicken Sie auf der ersten Seite des SQL Server-Installationscenter auf **eigenständige neue SQL Server-Installation oder Hinzufügen von Funktionen zu einer vorhandenen Installation**, und befolgen Sie die Anweisungen, akzeptieren die Standardoptionen.

### <a name="create-the-development-database-script"></a>Erstellen von Datenbankskripts Entwicklung

1. Führen Sie SSMS.
2. In der **Verbindung mit Server herstellen** Dialogfeld Geben Sie *(Localdb) \v11.0* als die **Servernamen**, lassen Sie **Authentifizierung** festgelegt**Windows-Authentifizierung**, und klicken Sie dann auf **verbinden**.

    ![SSMS eine Verbindung mit Server herstellen.](preparing-databases/_static/image10.png)
3. In der **Objekt-Explorer** Fenster, erweitern Sie **Datenbanken**, mit der rechten Maustaste **Aspnet-ContosoUniversity**, klicken Sie auf **Aufgaben**, und klicken Sie dann auf **Generieren von Skripts**.

    ![SSMS-Skripts generieren](preparing-databases/_static/image11.png)
4. In der **generieren und Veröffentlichen von Skripts** (Dialogfeld), klicken Sie auf **Skripterstellungsoptionen festlegen**.

    Sie können fortfahren die **Objekte auswählen** Schritt, da der Standardwert ist **Skript für gesamte Datenbank und alle Datenbankobjekte** und erwünscht ist.
5. Klicken Sie auf **erweiterte**.

    ![SSMS Skripterstellungsoptionen festlegen](preparing-databases/_static/image12.png)
6. In der **erweiterte Skripterstellungsoptionen** (Dialogfeld), einen Bildlauf bis zum **Typen Skript**, und klicken Sie auf die **nur Daten** Option in der Dropdown-Liste.
7. Änderung **Skripterstellung für USE DATABASE** auf **"false"**. USE-Anweisungen sind für Azure SQL-Datenbank ungültig und nicht für die Bereitstellung in SQL Server Express in der testumgebung benötigt werden.

    ![SSMS Skript nur Daten keine USE-Anweisung](preparing-databases/_static/image13.png)
8. Klicken Sie auf **OK**.
9. In der **generieren und Veröffentlichen von Skripts** (Dialogfeld), die **Dateiname** Feld gibt an, in dem das Skript erstellt werden soll. Wechseln Sie zur Projektmappenordner (im Ordner, die Ihre ContosoUniversity.sln-Datei) und den Dateinamen an *Aspnet-Daten-dev.sql*.
10. Klicken Sie auf **Weiter** fahren Sie mit der **Zusammenfassung** Registerkarte, und klicken Sie dann auf **Weiter** erneut aus, um das Skript zu erstellen.

    ![SSMS-Skripts erstellt](preparing-databases/_static/image14.png)
11. Klicken Sie auf **Fertig stellen**.

### <a name="create-the-production-database-script"></a>Erstellen von Datenbankskripts Produktion

Da das Projekt mit der Produktionsdatenbank noch nicht ausgeführt werden, wird nicht noch die LocalDB-Instanz zugeordnet. Aus diesem Grund müssen Sie zuerst die Datenbank anfügen.

1. In SSMS **Objektexplorer**, mit der rechten Maustaste **Datenbanken** , und klicken Sie auf **Anfügen**.

    ![Anfügen von SSMS](preparing-databases/_static/image15.png)
- In der **Datenbanken anfügen** (Dialogfeld), klicken Sie auf **hinzufügen** und navigieren Sie zu der *Aspnet-ContosoUniversity-Prod.mdf* in der Datei die *App\_ Daten* Ordner.

    ![Hinzufügen von SSMS MDF-Datei anfügen](preparing-databases/_static/image16.png)
- Klicken Sie auf **OK**.
- Halten Sie die gleiche Prozedur, die Sie zuvor verwendet, um ein Skript für die Produktion-Datei erstellen. Benennen Sie die Skriptdatei *Aspnet-Daten-prod.sql*.

## <a name="summary"></a>Zusammenfassung

Beide Datenbanken befinden sich jetzt bereitgestellt werden, und Sie haben zwei Data-Bereitstellungsskripts im Projektmappenordner.

![Daten-Bereitstellungsskripts](preparing-databases/_static/image17.png)

Im folgenden Lernprogramm konfigurieren Sie Einstellungen für Projektdateien, die Bereitstellung auswirken, und Sie richten Sie automatische *"Web.config"* Datei Transformationen für Einstellungen, die in der bereitgestellten Anwendung unterschiedlich sein muss.

## <a name="more-information"></a>Weitere Informationen

Weitere Informationen zu NuGet, finden Sie unter [verwalten Projektbibliotheken mit NuGet](https://msdn.microsoft.com/magazine/hh547106.aspx) und [NuGet-Dokumentation](http://docs.nuget.org/docs/start-here/overview). Wenn Sie NuGet verwenden möchten, müssen Sie Informationen zum Analysieren eines NuGet-Pakets, um zu bestimmen, welche Aktion er ausführt, wenn er installiert ist. (sie können z. B. konfigurieren *"Web.config"* Transformationen, konfigurieren Sie PowerShell-Skripts zur Buildzeit usw. ausführen.) Weitere Informationen zur Funktionsweise von NuGet finden Sie unter [erstellen und veröffentlichen ein Paket](http://docs.nuget.org/docs/creating-packages/creating-and-publishing-a-package) und [Konfigurationsdatei und Source Codetransformationen](http://docs.nuget.org/docs/creating-packages/configuration-file-and-source-code-transformations).

>[!div class="step-by-step"]
[Zurück](introduction.md)
[Weiter](web-config-transformations.md)
