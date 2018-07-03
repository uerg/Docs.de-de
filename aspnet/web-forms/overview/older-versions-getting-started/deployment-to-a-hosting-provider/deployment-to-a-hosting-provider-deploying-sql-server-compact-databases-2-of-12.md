---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12
title: 'Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mit Visual Studio oder Visual Web Developer: Bereitstellen von SQL Server Compact-Datenbanken – 2 von 12 | Microsoft-Dokumentation'
author: tdykstra
description: In dieser tutorialreihe erfahren Sie, wie zum Bereitstellen einer ASP.NET-Anwendung (veröffentlichen) Webanwendungsprojekt, die eine SQL Server Compact-Datenbank enthält, mithilfe von Visual Stu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: c3c76516-4c48-4153-bd03-d70e3a3edbb0
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12
msc.type: authoredcontent
ms.openlocfilehash: d52a8b8555f2f4accd628a5e24f2e1705fe8d9aa
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37364775"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-sql-server-compact-databases---2-of-12"></a>Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mit Visual Studio oder Visual Web Developer: Bereitstellen von SQL Server Compact-Datenbanken – 2 von 12
====================
durch [Tom Dykstra](https://github.com/tdykstra)

[Startprojekt herunterladen](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> In dieser tutorialreihe erfahren Sie, wie zum Bereitstellen einer ASP.NET-Anwendung (veröffentlichen) Webanwendungsprojekt, das eine SQL Server Compact-Datenbank mithilfe von Visual Studio 2012 RC oder Visual Studio Express 2012 RC für Web enthält. Sie können auch Visual Studio 2010 verwenden, wenn Sie die Web Publish Update installieren. Eine Einführung in die Reihe, finden Sie unter [im ersten Tutorial der Reihe](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Ein Lernprogramm, das zeigt, Bereitstellungsfunktionen, die nach der RC-Version von Visual Studio 2012 eingeführt wurden, zeigt, wie zum Bereitstellen von SQL Server-Editionen als SQL Server Compact und zeigt, wie Sie in Azure App Service-Web-Apps bereitstellen, finden Sie unter [ASP.NET-webbereitstellung Mithilfe von Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Übersicht

Dieses Tutorial zeigt, wie zwei SQL Server Compact-Datenbanken und die Datenbank-Engine für die Bereitstellung eingerichtet wird.

Für den Datenbankzugriff erfordert die Contoso University-Anwendung die folgende Software, die mit der Anwendung bereitgestellt werden muss, weil es nicht in .NET Framework enthalten ist:

- [SQL Server Compact](https://www.microsoft.com/sqlserver/en/us/editions/compact.aspx) (Datenbank-Engine).
- [ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx) (die ermöglichen des ASP.NET-Mitgliedschaftssystem mit SQL Server Compact)
- [Entitätsframework 5.0](https://msdn.microsoft.com/library/gg696172(d=lightweight,v=vs.103).aspx)(Code First mit Migrationen).

Die Struktur der Datenbank und einige (nicht alle) der Daten in der Anwendung zwei Datenbanken müssen ebenfalls bereitgestellt werden. In der Regel bei der Entwicklung eine Anwendung geben Sie Testdaten in eine Datenbank, die Sie nicht auf einer live-Website bereitstellen möchten. Sie können jedoch auch einige Produktionsdaten eingeben, die Sie bereitstellen möchten. In diesem Tutorial konfigurieren Sie die Contoso University-Projekt, damit die erforderliche Software und die richtigen Daten enthalten sind, bei der Bereitstellung.

Erinnerung: Wenn Sie eine Fehlermeldung erhalten, oder etwas nicht funktioniert, wie Sie das Lernprogramm durchzuarbeiten, sollten Sie unbedingt die [Problembehandlungsseite](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="sql-server-compact-versus-sql-server-express"></a>SQL Server Compact im Vergleich zu SQL Server Express

Die beispielanwendung verwendet die SQL Server Compact 4.0. Diese Datenbank-Engine ist eine relativ neue Option für Websites. frühere Versionen von SQL Server Compact funktionieren nicht in einer hostumgebung Web. SQL Server Compact bietet einige Vorteile im Vergleich zum Szenario der Entwicklung mit SQL Server Express und Bereitstellung in der Vollversion von SQL Server. Abhängig von der gewählten Hostinganbieter möglicherweise SQL Server Compact für die Bereitstellung, kostengünstiger, da einige Anbieter stellen zusätzliche zur Unterstützung einer vollständigen SQL Server-Datenbank in Rechnung. Es ist ohne zusätzliche Kosten für SQL Server Compact an, da Sie die Datenbank-Engine selbst als Teil Ihrer Webanwendung bereitstellen können.

Allerdings sollten Sie auch die Einschränkungen bewusst sein. SQL Server Compact unterstützt gespeicherte Prozeduren, Trigger, Ansichten oder Replikation nicht. (Eine vollständige Liste der SQL Server-Funktionen, die nicht von SQL Server Compact unterstützt werden, finden Sie unter [Unterschiede zwischen SQL Server Compact und SQL Server](https://msdn.microsoft.com/library/bb896140.aspx).) Darüber hinaus funktionieren einige der Tools, mit denen Sie zum Bearbeiten von Schemas und Daten in SQL Server Express und SQL Server-Datenbanken nicht mit SQL Server Compact. Sie können keine z. B. SQL Server Management Studio oder SQL Server Data Tools in Visual Studio mit SQL Server Compact-Datenbanken verwenden. Sie haben andere Optionen für die Arbeit mit SQL Server Compact-Datenbanken:

- Sie können Server-Explorer in Visual Studio verwenden, die begrenzte Bearbeitung Datenbankfunktionalität für SQL Server Compact bietet.
- Können Sie die Datenbank-Manipulation-Funktion von [WebMatrix](https://www.microsoft.com/web/webmatrix/), die über mehr Funktionen als Server-Explorer verfügt.
- Sie können relativ mit vollem Funktionsumfang von Drittanbietern verwenden oder öffnen Sie die Source-Tools, z. B. die [SQL Server Compact Toolbox](https://github.com/ErikEJ/SqlCeToolbox) und [SQL Compact Daten- und Schemadateien Skript Hilfsprogramm](https://github.com/ErikEJ/SqlCeToolbox).
- Sie können zu schreiben, und führen Ihre eigenen Skripts DDL (Data Definition Language), um das Datenbankschema zu bearbeiten.

Sie können mit SQL Server Compact beginnen und aktualisieren Sie dann später Ihre Bedürfnisse entwickeln. In weiteren Tutorials dieser Serie gezeigt, wie zum Migrieren von SQL Server Compact, SQL Server Express und SQL Server. Wenn Sie eine neue Anwendung erstellen und SQL Server in der nahen Zukunft mehr benötigen, ist es jedoch wahrscheinlich am besten mit SQL Server oder SQL Server Express zu starten.

## <a name="configuring-the-sql-server-compact-database-engine-for-deployment"></a>Konfigurieren die SQL Server Compact-Datenbank-Engine für die Bereitstellung

Die softwareanforderungen für den Datenzugriff in der Contoso University-Anwendung wurde hinzugefügt, indem Sie die folgenden NuGet-Pakete installieren:

- [SqlServerCompact](http://nuget.org/List/Packages/SqlServerCompact)
- [System.Web.Providers](http://nuget.org/List/Packages/System.Web.Providers) (ASP.NET universal Providers)
- [EntityFramework](http://nuget.org/List/Packages/EntityFramework)
- [EntityFramework.SqlServerCompact](http://nuget.org/List/Packages/EntityFramework.sqlservercompact)

Die Links zeigen Sie auf die aktuellen Versionen dieser Pakete, die möglicherweise höher als erwartet in das Startprojekt ist, die Sie für dieses Tutorial heruntergeladen. Stellen Sie für die Bereitstellung für einen Hostinganbieter sicher, dass Sie Entitätsframework 5.0 oder höher verwenden. Frühere Versionen von Code First-Migrationen erfordern volle Vertrauenswürdigkeit und an viele Hostinganbieter Ihrer Anwendung in der mittleren Vertrauensebene ausgeführt wird. Weitere Informationen zur mittleren Vertrauensebene finden Sie unter den [Bereitstellen in IIS als Testumgebung](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) Tutorial.

NuGet-Paketinstallation übernimmt in der Regel alle Elemente, die Sie benötigen, um diese Software mit der Anwendung bereitstellen. In einigen Fällen beinhaltet dies Aufgaben wie die Datei "Web.config" ändern und Hinzufügen von PowerShell-Skripts, die ausgeführt werden, wenn Sie die Projektmappe zu erstellen. **Wenn Sie Unterstützung für eines dieser Features (z. B. SQL Server Compact und Entity Framework) ohne Verwendung von NuGet hinzufügen möchten, stellen Sie sicher, dass Sie wissen, was NuGet-Paketinstallation ausführt, damit Sie die gleiche Arbeit manuell durchführen können.**

Es gibt eine Ausnahme, in dem NuGet Betreuung der alles geht, was man tun, um die erfolgreiche Bereitstellung sicherzustellen. Die SqlServerCompact-NuGet-Paket Fügt ein Skript nach der Erstellung zu Ihrem Projekt, das die systemeigenen Assemblys kopiert *X86* und *amd64* Unterordner unter dem Projekt *Bin* Ordner, aber das Skript enthält keine diese Ordner im Projekt. Daher wird Web Deploy nicht sie in der Ziel-Website kopieren, wenn Sie manuell in das Projekt einschließen. (Dieses Verhalten führt, aus der Standardkonfiguration für die Bereitstellung ist eine weitere Option, die Sie in diesen Tutorials nicht verwenden möchten, um die Einstellung zu ändern, die steuert dieses Verhalten. Die Einstellung, die Sie ändern können, ist **nur Dateien, die zum Ausführen der Anwendung benötigt** unter **bereitzustellenden Elemente** auf die **Web packen/veröffentlichen** Registerkarte die **Projekt Eigenschaften** Fenster. Das Ändern dieser Einstellung ist nicht in der Regel empfohlen, da es bei der Bereitstellung des viele weitere Dateien führen kann, in der produktionsumgebung, als es erforderlich sind. Weitere Informationen zu alternativen, finden Sie unter den [Konfigurieren von Projekteigenschaften](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md) Tutorial.)

Erstellen Sie das Projekt, und klicken Sie dann im **Projektmappen-Explorer** klicken Sie auf **alle Dateien anzeigen** , wenn Sie nicht bereits geschehen. Sie möglicherweise auch auf **aktualisieren**.

![Solution_Explorer_Show_All_Files](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image1.png)

Erweitern Sie die **Bin** Ordner finden Sie unter den **amd64** und **X86** Ordner, und wählen Sie dann die Ordner, mit der rechten Maustaste und wählen Sie **zu Projekt**.

![amd64_and_x86_in_Solution_Explorer.png](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image2.png)

Die Symbole ändern, um anzugeben, dass der Ordner im Projekt enthalten ist.

![Solution_Explorer_amd64_included.png](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image3.png)

## <a name="configuring-code-first-migrations-for-application-database-deployment"></a>Konfigurieren von Code First-Migrationen zur Anwendungsbereitstellung für die Datenbank

Bei der Bereitstellung einer Datenbank bereitzustellen nicht in der Regel Sie einfach Ihrer Entwicklungsdatenbank mit allen enthaltenen Daten in die Produktion, da ein Großteil der Daten wahrscheinlich nur für Testzwecke. Beispielsweise sind die Namen "Student" in einer Testdatenbank fiktive. Sie können nicht nur die Struktur der Datenbank mit keine Daten auf der anderen Seite häufig überhaupt bereitstellen. Einige der Daten in der Testdatenbank kann sich um echte Daten sein und muss vorhanden sein, wenn der Benutzer zur Verwendung der Anwendung beginnen. Z. B. möglicherweise Ihre Datenbank eine Tabelle, die Werte für die gültige Dienstqualität oder real Abteilungsnamen enthält.

Um diesem häufigen Szenario zu simulieren, konfigurieren Sie eine Code erste Migrationen Seed-Methode, die nur die Daten in die Datenbank einfügt, die in der produktionsumgebung vorhanden sein sollen. Testdaten kann von diesem Seed-Methode wird nicht eingefügt werden, da sie in der Produktion ausgeführt werden soll, nachdem der Code First die Datenbank in der produktionsumgebung erstellt.

In früheren Versionen von Code First bevor Migrationen kam, war es üblich Seed-Methoden, die Testdaten auch einfügen, da bei jeder modelländerung während der Entwicklung der Datenbank, die vollständig löschen und von Grund auf neu erstellt wurde. Mit Code First-Migrationen werden nach Änderungen in der Datenbank, Testdaten beibehalten, sodass einschließlich Testdaten in die Seed-Methode nicht erforderlich ist. Das Projekt, die, das Sie heruntergeladen, verwendet die Methode vor der Migration einschließlich der alle Daten in die Seed-Methode einer Klasse Initialisierer. In diesem Tutorial werden Sie Initialisiererklasse deaktivieren und Aktivieren von Migrationen. Klicken Sie dann aktualisieren Sie die Seed-Methode in der Konfigurationsklasse für Migrationen, damit nur die Daten eingefügt, die in der Produktion eingefügt werden soll.

Das folgende Diagramm zeigt das Schema der Datenbank der dienstanwendung:

[![School_database_diagram](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image4.png)

Für diese Tutorials Sie gehen davon aus, die die `Student` und `Enrollment` Tabellen sollte leer sein, wenn der Standort zuerst bereitgestellt wird. Die anderen Tabellen enthalten Daten, die vorab werden muss, geladen Wenn die Anwendung online geschaltet wird.

Da Sie Code First-Migrationen verwenden möchten, nicht mehr müssen Sie mit der **DropCreateDatabaseIfModelChanges** Code First-Initialisierer. Der Code für diese Initialisierer ist in der Datei SchoolInitializer.cs im ContosoUniversity.DAL-Projekt. Eine Einstellung in der **"appSettings"** Element der Datei "Web.config" bewirkt, dass diese Initialisierer ausgeführt, sobald die Anwendung versucht, Zugriff auf die Datenbank zum ersten Mal:

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample1.xml?highlight=3)]

Öffnen Sie die Datei Web.config der Anwendung, und entfernen Sie das Element, das angibt, die Code First-Initialisierer-Klasse aus dem Element "appSettings". Das Element "appSettings" sieht nun folgendermaßen aus:

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample2.xml)]

> [!NOTE]
> Eine andere Möglichkeit, eine Initialisiererklasse anzugeben ist es durch den Aufruf erfolgen `Database.SetInitializer` in die `Application_Start` -Methode in der die *"Global.asax"* Datei. Wenn Sie Migrationen in einem Projekt aktivieren, die diese Methode verwendet wird, um den Initialisierer anzugeben, entfernen Sie diese Codezeile.


Aktivieren Sie als Nächstes Code First-Migrationen.

Der erste Schritt ist, um sicherzustellen, dass das ContosoUniversity-Projekt als Startprojekt festgelegt ist. In **Projektmappen-Explorer**mit der rechten Maustaste auf das ContosoUniversity-Projekt, und wählen Sie **als Startprojekt festlegen**. Code First-Migrationen sucht in der Startup-Projekt auf die Datenbank-Verbindungszeichenfolge zu ermitteln.

Von der **Tools** Menü klicken Sie auf **Bibliothekspaket-Manager** und dann **-Paket-Manager-Konsole**.

![Selecting_Package_Manager_Console](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image6.png)

Am oberen Rand der **-Paket-Manager-Konsole** Fenster auszuwählen ContosoUniversity.DAL als Standardprojekt, und klicken Sie dann an die `PM>` Eingabeaufforderung Geben Sie "Enable-Migrations".

![Enable-migrations_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image7.png)

Dieser Befehl erstellt eine *Configuration.cs* Datei in einem neuen *Migrationen* Ordner im Projekt ContosoUniversity.DAL.

![Migrations_folder_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image8.png)

Die DAL-Projekt wird ausgewählt, da der Befehl "Enable-Migrations" im Projekt ausgeführt werden muss, die die Code First Context-Klasse enthält. Wenn ein Klassenbibliotheksprojekt diese Klasse ist, sucht Code First-Migrationen für die Datenbank-Verbindungszeichenfolge in das Startprojekt für die Lösung. In der Projektmappe ContosoUniversity wurde das Web-Projekt als Startprojekt festgelegt. (Wenn Sie nicht das Projekt zu bestimmen, das die Verbindungszeichenfolge als Startprojekt in Visual Studio hat möchten, können Sie das Startprojekt in der PowerShell-Befehl angeben. Um die Befehlssyntax für das Enable-Migrations-Befehl zu sehen, können Sie den Befehl "Get-Help Enable-Migrations" eingeben.)

Öffnen Sie die Configuration.cs-Datei, und Ersetzen Sie die Kommentare in der `Seed` Methode durch den folgenden Code:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample3.cs)]

Die Verweise auf `List` rote Wellenlinien unter diesen haben, da Sie keine `using` noch-Anweisung für den Namespace. Mit der rechten Maustaste eine der Instanzen des `List` , und klicken Sie auf **beheben**, und klicken Sie dann auf **using System.Collections.Generic**.

![Lösen Sie mit der using-Anweisung](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image9.png)

Diese Menüoption fügt den folgenden Code der `using` Anweisungen am oberen Rand der Datei.

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample4.cs)]

> [!NOTE]
> Hinzufügen von Code, der `Seed` Methode ist eine von vielen Möglichkeiten, die Sie in die Datenbank "Festplattenlaufwerk" einfügen können. Eine Alternative zum Hinzufügen von Code ist die `Up` und `Down` Methoden jede Migration-Klasse. Die `Up` und `Down` Methoden enthalten Code, der Änderungen in der Datenbank implementiert. Sehen Sie Beispiele für die sie in der [Bereitstellen eines Datenbankupdates](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) Tutorial.
> 
> Sie können auch Schreiben Code, der SQL-Anweisungen, mithilfe ausführt der `Sql` Methode. Sie z. B., wenn Sie die Department-Tabelle eine Budgetspalte hinzugefügt wurden und alle Abteilung Budgets zu 1.000,00 $ als Teil einer Migration initialisieren möchten, können Hinzufügen der folgenden Codezeile den `Up` Methode für diese Migration:
> 
> `Sql("UPDATE Department SET Budget = 1000");`
> 
> Dieses Beispiel für dieses Tutorial verwendet die `AddOrUpdate` -Methode in der die `Seed` -Methode der Code-First-Migrationen `Configuration` Klasse. Code First-Migrationen Ruft die `Seed` -Methode auf, nachdem jede Migration, und diese Methode aktualisiert die Zeilen, die bereits eingefügt wurde, oder fügt sie, wenn sie noch nicht vorhanden sind. Die `AddOrUpdate` Methode möglicherweise nicht die beste Wahl für Ihr Szenario. Weitere Informationen finden Sie unter [kümmern mit EF 4.3 AddOrUpdate Methode](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) Julie lermans Blog.


Drücken Sie STRG + UMSCHALT + B, um das Projekt erstellen.

Der nächste Schritt ist die Erstellung einer `DbMigration` -Klasse für die anfängliche Migration. Sie möchten diese Migration auf eine neue Datenbank erstellen, müssen Sie die Datenbank zu löschen, die bereits vorhanden ist. SQL Server Compact-Datenbanken befinden sich *.sdf* Dateien in die *App\_Daten* Ordner. In **Projektmappen-Explorer**, erweitern Sie *App\_Daten* im ContosoUniversity-Projekt auf die beiden SQL Server Compact-Datenbanken finden Sie unter dem durch dargestellt *.sdf*Dateien.

Mit der rechten Maustaste die *School.sdf* Datei, und klicken Sie auf **löschen**.

![sdf_files_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image10.png)

In der **-Paket-Manager-Konsole** Fenster, geben Sie den Befehl "add-Migration Initial" zum Erstellen der anfänglichen Migrations, und nennen Sie sie "Initial".

![Hinzufügen migration_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image11.png)

Code First-Migrationen wird eine weitere Klassendatei in der *Migrationen* Ordner, und diese Klasse enthält Code, der das Schema der Datenbank erstellt.

In der **-Paket-Manager-Konsole**, geben Sie den Befehl "Update-Database" zum Erstellen der Datenbank und Ausführen der **Ausgangswert** Methode.

![update-database_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image12.png)

(Wenn Sie eine Fehlermeldung, der angibt erhalten, eine Tabelle ist bereits vorhanden und kann nicht erstellt werden, es ist wahrscheinlich, da Sie die Anwendung ausgeführt, nachdem Sie die Datenbank gelöscht und bevor Sie ausgeführt `update-database`. -Verarbeitung, löschen Sie die *School.sdf* -Datei erneut, und wiederholen Sie den `update-database` Befehl.)

Führen Sie die Anwendung aus. Jetzt wird Seite für Studenten ist leer, aber die dozentenseite enthält Dozenten. Dies ist in der Produktion erhalten Sie nach der Bereitstellung der Anwendung.

![Empty_Students_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image13.png)

![Instructors_page_after_initial_migration](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image14.png)

Das Projekt ist jetzt bereit für die Bereitstellung der *School* Datenbank.

## <a name="creating-a-membership-database-for-deployment"></a>Erstellen eine Mitgliedschaftsdatenbank für die Bereitstellung

Die Contoso University-Anwendung verwendet die ASP.NET Membership-System und Forms-Authentifizierung zum Authentifizieren und Autorisieren von Benutzern. Einer der darin enthaltenen Seiten ist nur für Administratoren zugegriffen werden kann. Um diese Seite anzuzeigen, führen Sie die Anwendung, und wählen Sie **Update Gutschriften** im Flyout-Menü unter **Kurse**. Zeigt die Anwendung die **anmelden** Seite, da nur Administratoren autorisiert sind, verwenden die **Update Gutschriften** Seite.

[![Log_in_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image15.png)

Melden Sie sich als "Admin" mit dem Kennwort "Pas$ w0rd" (Beachten Sie die Anzahl 0 (null) anstelle von den Buchstaben "o" in "w0rd"). Nachdem Sie sich angemeldet haben, die **Update Gutschriften** angezeigt wird.

[![Update_Credits_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image17.png)

Wenn Sie einen Standort zum ersten Mal bereitstellen, ist es üblich, schließen Sie die meisten oder alle der Benutzerkonten, die Sie für Testzwecke zu erstellen. In diesem Fall stellen Sie ein Administratorkonto und keine Benutzerkonten bereit. Anstatt Testkonten manuell zu löschen, erstellen Sie eine neue Mitgliedschaftsdatenbank, die nur das Benutzerkonto ein Administrator verfügt, das Sie in der Produktion müssen.

> [!NOTE]
> Die Mitgliedschaftsdatenbank speichert einen Hash der Kennwörter. Zum Bereitstellen von Konten, die von einem Computer zu einem anderen müssen Sie sicherstellen, dass hashing Routinen verschiedenen Hashes auf dem Zielserver kein generieren, als auf dem Quellcomputer. Sie werden die gleichen Hashwerte generieren, bei der Verwendung der ASP.NET Universal Providers, solange Sie nicht, dass den Standard-Algorithmus ändern. Der Standardalgorithmus ist HMACSHA256 und wird angegeben, der **Überprüfung** Attribut der **[MachineKey](https://msdn.microsoft.com/library/w8h3skw9.aspx)** Element in der Datei "Web.config".


Die Mitgliedschaftsdatenbank wird nicht beibehalten, indem Sie Code First-Migrationen, und es gibt keine automatische Initialisierung, die die Datenbank mit Testkonten startet (wie bei der Datenbank "School"). Aus diesem Grund zum Beibehalten von Testdaten zur Verfügung stellen Sie eine Kopie der Datenbank, bevor Sie eine neue Ressourcengruppe erstellen.

In **Projektmappen-Explorer**, benennen Sie die *aspnet.sdf* Datei die *App\_Daten* Ordner *Aspnet-Dev.sdf*. (Erstellen Sie eine Kopie, benennen Sie sie einfach nicht, erstellen Sie eine neue Datenbank in Kürze.)

In **Projektmappen-Explorer**, stellen Sie sicher, dass das Webprojekt ("ContosoUniversity", "nicht ContosoUniversity.DAL") ausgewählt ist. Klicken Sie dann in der **Projekt** , wählen Sie im Menü **ASP.NET-Konfiguration** zum Ausführen der **Websiteverwaltungs-Tool**(WAT).

Wählen Sie die **Sicherheit** Registerkarte.

[![WAT_Security_tab](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image19.png)

Klicken Sie auf **erstellen oder Verwalten von Rollen** und Hinzufügen einer **Administrator** Rolle.

[![WAT_Create_New_Role](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image22.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image21.png)

Navigieren Sie zurück zu den **Sicherheit** auf **Create User**, und Benutzer "Admin" als Administrator hinzufügen. Bevor Sie auf der **Create User** Schaltfläche der **Create User** stellen Sie sicher, dass Sie auswählen, die **Administrator** Kontrollkästchen. In diesem Tutorial verwendete Kennwort ist "Pas$ w0rd", und Sie können eine beliebige e-Mail-Adresse eingeben.

[![WAT_Create_User](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image24.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image23.png)

Schließen Sie den Browser. In **Projektmappen-Explorer**, klicken Sie auf die Schaltfläche "Aktualisieren", um die neuen *aspnet.sdf* Datei.

![New_aspnet.sdf_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image25.png)

Mit der rechten Maustaste **aspnet.sdf** , und wählen Sie **zu Projekt**.

## <a name="distinguishing-development-from-production-databases"></a>Entwicklung von Produktionsdatenbanken unterscheiden

In diesem Abschnitt müssen Sie die Datenbanken umbenennen, damit die Development-Versionen "School"-Dev.sdf sind und Aspnet-Dev.sdf und die Produktionsversionen "School"-Prod.sdf und Aspnet-Prod.sdf sind. Dies ist nicht erforderlich, aber dies also können Sie zu verhindern, dass Test- und produktionsumgebungen Versionen der Datenbanken verwirrt.

In **Projektmappen-Explorer**, klicken Sie auf **aktualisieren** und erweitern Sie die App\_Datenordner finden in der Datenbank "School", die Sie zuvor erstellt haben, der rechten Maustaste darauf und wählen Sie **in Projekt einschließen** .

![Including_School.sdf_in_project](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image26.png)

Benennen Sie *aspnet.sdf* zu *Aspnet-Prod.sdf*.

Benennen Sie *School.sdf* zu *School-Dev.sdf*.

Beim Ausführen der Anwendung in Visual Studio, die Sie nicht möchten, verwenden Sie die *-Prod* Versionen der Datenbankdateien, die Sie verwenden möchten *- Dev* Versionen. Aus diesem Grund müssen Sie die Verbindungszeichenfolgen in der Datei "Web.config" ändern, damit sie es auf die *- Dev* Versionen der Datenbanken. (Sie noch nicht School-Prod.sdf-Datei erstellt, aber das ist in Ordnung, da Code First die Datenbank in der ersten Produktionszeit erstellen, die Sie Ihre app ausführen, gibt es.)

Öffnen Sie die Datei Web.config der Anwendung, und suchen Sie die Verbindungszeichenfolgen:

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample5.xml)]

Ändern Sie "aspnet.sdf" in "Aspnet-Dev.sdf" und ändern Sie "School.sdf" in "Schule-Dev.sdf":

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample6.xml?highlight=4-5)]

Die SQL Server Compact-Datenbank-Engine und die beiden Datenbanken können nun bereitgestellt werden. In dem folgenden Tutorial richten Sie automatische *"Web.config"* konfigurationsdateitransformationen für Einstellungen, die in den Umgebungen für Entwicklungs-, Test- und produktionsumgebungen unterscheiden müssen. (Für die Einstellungen, die geändert werden müssen, werden die Verbindungszeichenfolgen, aber richten Sie diese Änderungen später beim Erstellen eines Veröffentlichungsprofils.)

## <a name="more-information"></a>Weitere Informationen

Weitere Informationen zu NuGet, finden Sie unter [Verwalten von Projektbibliotheken mit NuGet](https://msdn.microsoft.com/magazine/hh547106.aspx) und [NuGet-Dokumentation](http://docs.nuget.org/docs/start-here/overview). Wenn Sie nicht NuGet verwenden möchten, müssen Sie erfahren, wie Sie ein NuGet-Paket, um zu bestimmen, welche Aktion er ausführt, bei der Installation zu analysieren. (sie können z. B. konfigurieren *"Web.config"* Transformationen, PowerShell-Skripts zur Ausführung auf den Zeitpunkt der Erstellung usw. konfigurieren.) Weitere Informationen zur Funktionsweise von NuGet finden Sie unter besonders [erstellen und Veröffentlichen eines Pakets](http://docs.nuget.org/docs/creating-packages/creating-and-publishing-a-package) und [Konfigurationsdatei und Quellcodetransformationen](http://docs.nuget.org/docs/creating-packages/configuration-file-and-source-code-transformations).

> [!div class="step-by-step"]
> [Zurück](deployment-to-a-hosting-provider-introduction-1-of-12.md)
> [Weiter](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12.md)
