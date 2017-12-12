---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12
title: "Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mit Visual Studio oder Visual Web Developer: Bereitstellen von SQL Server Compact-Datenbanken – 2 12 | Microsoft Docs"
author: tdykstra
description: "Diese Reihe von Lernprogrammen wird gezeigt, wie das Bereitstellen einer ASP.NET-Anwendung (veröffentlichen) Webanwendungsprojekt, die eine SQL Server Compact-Datenbank enthält, mithilfe von Visual das..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: c3c76516-4c48-4153-bd03-d70e3a3edbb0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12
msc.type: authoredcontent
ms.openlocfilehash: d0b76c06495c51df3ed0f61cd318507a05240392
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-sql-server-compact-databases---2-of-12"></a>Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mit Visual Studio oder Visual Web Developer: Bereitstellen von SQL Server Compact-Datenbanken – 2 von 12
====================
Durch [Tom Dykstra](https://github.com/tdykstra)

[Startprojekt herunterladen](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Diese Reihe von Lernprogrammen wird gezeigt, wie das Bereitstellen einer ASP.NET-Anwendung (veröffentlichen) Webanwendungsprojekt, die eine SQL Server Compact-Datenbank mithilfe von Visual Studio 2012 RC oder Visual Studio Express 2012 RC für das Web enthält. Sie können auch Visual Studio 2010 verwenden, wenn Sie das Web veröffentlichen Update installieren. Eine Einführung in der Reihe, finden Sie unter [im ersten Lernprogramm, in der Reihe](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Ein Lernprogramm, die die Bereitstellungsfeatures nach der RC-Version von Visual Studio 2012 eingeführt wurde, wird gezeigt, wie SQL Server-Editionen als SQL Server Compact bereitstellen, und zeigt die Vorgehensweise beim Bereitstellen in Azure App Service-Web-Apps, finden Sie unter [ASP.NET Web Deploy Mithilfe von Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Übersicht

Dieses Lernprogramm zeigt, wie zwei SQL Server Compact-Datenbanken und das Datenbankmodul für die Bereitstellung einrichten.

Für den Datenbankzugriff erfordert die Universität von Contoso-Anwendung die folgende Software, die mit der Anwendung bereitgestellt werden, da er nicht in .NET Framework enthalten ist:

- [SQL Server Compact](https://www.microsoft.com/sqlserver/en/us/editions/compact.aspx) (Datenbankmodul).
- [ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx) (die ermöglichen des ASP.NET-Mitgliedschaftssystem mit SQL Server Compact)
- [Entity Framework 5.0](https://msdn.microsoft.com/en-us/library/gg696172(d=lightweight,v=vs.103).aspx)(Code First mit Migrationen).

Der Datenbankstruktur und einige (nicht alle) der Daten in der Anwendung zwei Datenbanken müssen ebenfalls bereitgestellt werden. In der Regel, wie Sie eine Anwendung entwickeln, geben Sie Testdaten in eine Datenbank, die Sie nicht auf einem live-Website bereitstellen möchten. Sie können jedoch auch einige Produktionsdaten eingeben, die Sie bereitstellen möchten. In diesem Lernprogramm konfigurieren Sie die University Contoso-Projekt, damit die erforderliche Software und die richtigen Daten enthalten sind, bei der Bereitstellung.

Hinweis: Wenn Sie eine Fehlermeldung erhalten, oder etwas funktioniert nicht, wenn Sie das Lernprogramm absolvieren, müssen Sie überprüfen die [Website](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="sql-server-compact-versus-sql-server-express"></a>SQL Server Compact im Vergleich zu SQL Server Express

Die beispielanwendung verwendet SQL Server Compact 4.0. Diese Datenbank-Engine ist eine relativ neue Option für Websites. frühere Versionen von SQL Server Compact funktionieren nicht in einer Web-hosting-Umgebung. SQL Server Compact bietet einige Vorteile im Vergleich zu den gängigeren Szenario mit SQL Server Express Entwicklung und Bereitstellung in der Vollversion von SQL Server. Je nach den gewählten Hostinganbieter SQL Server Compact günstiger bereitstellen, möglicherweise einige Anbieter zusätzlicher zur Unterstützung einer vollständigen SQL Server-Datenbank in Rechnung zu stellen. Es gibt keine zusätzlichen Gebühren für SQL Server Compact auf, da das Datenbankmodul selbst als Teil der Webanwendung bereitgestellt werden kann.

Allerdings sollten Sie auch Einschränkungen bewusst sein. SQL Server Compact unterstützt gespeicherte Prozeduren, Trigger, Sichten oder Replikation nicht. (Eine vollständige Liste der SQL Server-Funktionen, die von SQL Server Compact nicht unterstützt werden, finden Sie unter [Unterschiede zwischen SQL Server Compact und SQL Server](https://msdn.microsoft.com/en-us/library/bb896140.aspx).) Darüber hinaus funktionieren einige der Tools, die Sie, zum Bearbeiten von Schemas und Daten in SQL Server Express und SQL Server-Datenbanken verwenden können nicht mit SQL Server Compact. Sie können keine z. B. SQL Server Management Studio oder SQL Server Data Tools in Visual Studio mit SQL Server Compact-Datenbanken verwenden. Sie haben die anderen Optionen für die Arbeit mit SQL Server Compact-Datenbanken:

- Sie können Server-Explorer in Visual Studio die begrenzte Bearbeitung-Funktionalität für SQL Server Compact bietet.
- Können Sie die Manipulation Datenbankfunktion von [WebMatrix](https://www.microsoft.com/web/webmatrix/), die über mehr Funktionen als Server-Explorer verfügt.
- Können Sie relativ mit umfassenden eines Drittanbieters verwenden oder öffnen Sie die Source-Tools, z. B. die [SQL Server Compact Toolbox](https://github.com/ErikEJ/SqlCeToolbox) und [SQL Compact Daten- und Schemadateien Skript Hilfsprogramm](https://github.com/ErikEJ/SqlCeToolbox).
- Schreiben, und führen Sie eine eigene DDL (Data Definition Language)-Skripts, um das Datenbankschema zu bearbeiten.

Sie können mit SQL Server Compact beginnen und aktualisieren Sie dann später mit steigenden Ihren Anforderungen. In dieser Serie spätere Lernprogrammen zeigen, wie SQL Server Express und SQL Server Migration von SQL Server Compact. Wenn Sie eine neue Anwendung erstellen und erwarten, dass SQL Server in naher Zukunft zu benötigen, ist es jedoch ratsam, die mit SQL Server oder SQL Server Express zu starten.

## <a name="configuring-the-sql-server-compact-database-engine-for-deployment"></a>Konfigurieren von SQL Server Compact-Datenbankmodul für die Bereitstellung

Die für den Datenzugriff in der Contoso-University Anwendung erforderliche Software wurde hinzugefügt, indem Sie die folgenden NuGet-Pakete installieren:

- [SqlServerCompact](http://nuget.org/List/Packages/SqlServerCompact)
- [System.Web.Providers](http://nuget.org/List/Packages/System.Web.Providers) (ASP.NET universal Providers)
- [EntityFramework](http://nuget.org/List/Packages/EntityFramework)
- [EntityFramework.SqlServerCompact](http://nuget.org/List/Packages/EntityFramework.sqlservercompact)

Die Links zeigen Sie auf die aktuellen Versionen dieser Pakete, die möglicherweise aktuellere, was im Startprojekt installiert ist, die Sie für dieses Lernprogramm heruntergeladen haben. Bereitstellung mit einem Hostinganbieter stellen Sie sicher, dass Sie Entity Framework 5.0 oder höher verwenden. Frühere Versionen von Code First-Migrationen erfordern vollständige Vertrauenswürdigkeit aus, und viele Hostinganbietern die Anwendung in mit mittlerer Vertrauenswürdigkeit ausgeführt wird. Weitere Informationen zu mit mittlerer Vertrauenswürdigkeit, finden Sie unter der [Bereitstellung in IIS als Testumgebung](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) Lernprogramm.

NuGet-Paketinstallation übernimmt im Allgemeinen alles, was Sie benötigen, um diese Software mit der Anwendung bereitstellen. In einigen Fällen umfasst dies Aufgaben wie das Ändern der Datei "Web.config" und das Hinzufügen von PowerShell-Skripts, die ausgeführt werden, sobald Sie die Projektmappe zu erstellen. **Wenn Sie Unterstützung für diese Funktionen (z. B. SQL Server Compact und Entity Framework) ohne mithilfe von NuGet hinzufügen möchten, stellen Sie sicher, dass Sie wissen Wirkungsweise NuGet-Paketinstallation, damit Sie die gleiche Aktionen manuell ausführen können.**

Eine Ausnahme besteht, in denen NuGet Sorgfalt alles akzeptiert haben Sie erforderlich ist, um die erfolgreiche Bereitstellung sicherzustellen. Das SqlServerCompact NuGet-Paket Fügt ein Skript nach der Erstellung zu Ihrem Projekt, das die systemeigenen Assemblys kopiert *X86* und *amd64* Unterordner unter dem Projekt *"bin"* Ordner, sondern das Skript enthält keine diese Ordner im Projekt. Daher wird Web Deploy nicht an den Zielstandort für die Web kopieren, wenn Sie manuell in das Projekt einschließen. (Dieses Verhalten führt die Standardkonfiguration für die Bereitstellung ist eine weitere Option, die Sie in diesen Lernprogrammen verwenden, wird nicht zum Ändern der Einstellung, die dieses Verhalten steuert. Die Einstellung, die Sie ändern können, ist **nur Dateien, die zum Ausführen der Anwendung benötigt** unter **bereitzustellenden Elemente** auf die **Web packen/veröffentlichen** auf der Registerkarte die **Projekt Eigenschaften** Fenster. Das Ändern dieser Einstellung wird nicht im Allgemeinen empfohlen, da es in der Bereitstellung viele weitere Dateien führen kann, in der produktionsumgebung, als es erforderlich sind. Weitere Informationen zu alternativen, finden Sie unter der [Konfigurieren von Projekteigenschaften](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md) Lernprogramm.)

Erstellen Sie das Projekt, und klicken Sie dann im **Projektmappen-Explorer** klicken Sie auf **alle Dateien anzeigen** , wenn Sie nicht bereits geschehen. Sie möglicherweise auch auf **aktualisieren**.

![Solution_Explorer_Show_All_Files](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image1.png)

Erweitern Sie die **"bin"** sehen die **amd64** und **X86** Ordner, und wählen Sie dann die Ordner, mit der rechten Maustaste und wählen Sie **Projekt**.

![amd64_and_x86_in_Solution_Explorer.PNG](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image2.png)

Die Symbole ändern, um anzuzeigen, dass der Ordner in das Projekt eingeschlossen wurde.

![Solution_Explorer_amd64_included.PNG](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image3.png)

## <a name="configuring-code-first-migrations-for-application-database-deployment"></a>Konfigurieren von Code First-Migrationen für die Bereitstellung der Datenbank

Beim Bereitstellen einer Anwendungsdatenbank nicht in der Regel Sie einfach Ihre Entwicklungsdatenbank mit allen enthaltenen Daten bis hin zur Produktion, bereitgestellt werden, da ein Großteil der Daten wahrscheinlich besteht ausschließlich für Testzwecke vorgesehen. Beispielsweise sind die Student-Namen in eine Testdatenbank fiktive. Sie können nicht nur die Datenbankstruktur ohne Daten darin andererseits, häufig überhaupt bereitstellen. Einige der Daten in der Testdatenbank möglicherweise um echte Daten und muss vorhanden sein, wenn der Benutzer zur Verwendung der Anwendung zu starten. Z. B. möglicherweise die Datenbank eine Tabelle, die gültige Grade Werten oder echte Abteilungsnamen enthält.

Um dieses gängige Szenario zu simulieren, konfigurieren Sie eine Code erste Migrationen Seed-Methode, die nur die Daten in die Datenbank einfügt, die in der Produktion vorhanden sein sollen. Testdaten kann von diesem Seed-Methode wird nicht eingefügt werden, da es in der produktionsumgebung ausgeführt wird, nachdem die Datenbank in der Produktion Code First erstellt.

In früheren Versionen von Code First bevor Migrationen freigegeben wurde, war es üblich Seed-Methoden, um die Testdaten auch einfügen, da bei jeder Änderung Modell während der Entwicklung die Datenbank mussten vollständig gelöscht und von Grund auf neu erstellt werden. Mit Code First-Migrationen werden Testdaten nach Änderungen an der Datenbank beibehalten, damit einschließlich Testdaten in der Seed-Methode nicht erforderlich ist. Das Projekt, das Sie heruntergeladen haben verwendet die Methode vor der Migration einschließlich aller Daten in der Seed-Methode einer Klasse Initialisierer. In diesem Lernprogramm Sie deaktivieren die Initialisiererklasse und Migrationen zu aktivieren. Klicken Sie dann aktualisieren Sie die Seed-Methode in der zielmigrationen, damit sie nur Daten einfügt, die in der Produktion eingefügt werden soll.

Das folgende Diagramm zeigt das Schema der Datenbank der dienstanwendung:

[![School_database_diagram](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image4.png)

Für diese Lernprogramme, Sie gehen davon aus, die die `Student` und `Enrollment` Tabellen sollte leer sein, wenn der Standort zuerst bereitgestellt wird. Die anderen Tabellen enthalten Daten, die vorab geladen werden, wenn die Anwendung online geschaltet wird.

Da Sie Code First-Migrationen verwenden, nicht mehr müssen Sie die **DropCreateDatabaseIfModelChanges** Code First Initialisierer. Der Code für diese Initialisierer ist in der Datei SchoolInitializer.cs im ContosoUniversity.DAL-Projekt. Eine Einstellung in der **"appSettings"** -Element der Datei "Web.config" bewirkt, dass diese Initialisierer ausgeführt, sobald die Anwendung versucht, Zugriff auf die Datenbank zum ersten Mal:

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample1.xml?highlight=3)]

Öffnen Sie die Anwendungsdatei "Web.config", und entfernen Sie das Element, das die Code First Initialisierer-Klasse aus dem Element "appSettings" angibt. Das Element "appSettings" sieht nun wie folgt:

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample2.xml)]

> [!NOTE]
> Alternative Möglichkeit zum Angeben einer Initialisiererklasse ist, führen Sie es durch den Aufruf `Database.SetInitializer` in der `Application_Start` Methode in der *"Global.asax"* Datei. Wenn Sie Migrationen in einem Projekt aktivieren, die diese Methode verwendet wird, um den Initialisierer anzugeben, entfernen Sie diese Codezeile.


Als Nächstes Code First-Migrationen zu aktivieren.

Der erste Schritt besteht, um sicherzustellen, dass das ContosoUniversity-Projekt als Startprojekt festgelegt ist. In **Projektmappen-Explorer**mit der rechten Maustaste auf das ContosoUniversity-Projekt, und wählen Sie **als Startprojekt festlegen**. Code First-Migrationen sieht in das Startup-Projekt auf die Datenbank-Verbindungszeichenfolge gefunden.

Aus der **Tools** Menü klicken Sie auf **Bibliothekspaket-Manager** und dann **Package Manager Console**.

![Selecting_Package_Manager_Console](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image6.png)

Am oberen Rand der **Package Manager Console** Fenster auswählen ContosoUniversity.DAL als Standardprojekt und anschließend an die `PM>` Eingabeaufforderung Geben Sie "Enable-Migrations".

![Enable-migrations_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image7.png)

Dieser Befehl erstellt eine *"Configuration.cs"* Datei in einem neuen *Migrationen* Ordner des Projekts ContosoUniversity.DAL.

![Migrations_folder_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image8.png)

Die DAL-Projekt wird ausgewählt, da der Befehl "Enable-Migrations" im Projekt ausgeführt werden muss, die die Code First Context-Klasse enthält. Wenn diese Klasse in ein Klassenbibliotheksprojekt, gesucht Code First-Migrationen für die Datenbank-Verbindungszeichenfolge in das Startprojekt für die Projektmappe. In der Projektmappe ContosoUniversity wurde das Projekt als Startprojekt festgelegt. (Wenn Sie nicht das Projekt festlegen, der die Verbindungszeichenfolge wie das Startup-Projekt in Visual Studio möchten, können Sie das Startprojekt in der PowerShell-Befehl angeben. Um die Befehlssyntax für den Enable-Migrations-Befehl angezeigt wird, können Sie den Befehl "Get-Help Enable-Migrations" eingeben.)

Öffnen Sie die Datei "Configuration.cs", und Ersetzen Sie die Kommentare in der `Seed` -Methode durch folgenden Code:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample3.cs)]

Die Verweise auf `List` rote Wellenlinien unter sich haben, da Sie keine `using` noch-Anweisung für den Namespace. Mit der rechten Maustaste eine der Instanzen des `List` , und klicken Sie auf **beheben**, und klicken Sie dann auf **using System.Collections.Generic**.

![Mit der Anweisung beheben](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image9.png)

Diese Menüauswahl fügt den folgenden Code, der `using` Anweisungen am oberen Rand der Datei.

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample4.cs)]

> [!NOTE]
> Hinzufügen von Code, der `Seed` Methode ist eine von vielen Möglichkeiten, die Sie in der Datenbank eingebauten einfügen können. Eine Alternative besteht darin, zum Hinzufügen von Code die `Up` und `Down` Methoden jeder Migration-Klasse. Die `Up` und `Down` Methoden Code enthalten, der Änderungen an der Datenbank implementiert. Sie finden Beispiele dafür, in der [Bereitstellen eines Datenbankupdates](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) Lernprogramm.
> 
> Kann auch Code schreiben, der SQL-Anweisungen mit führt die `Sql` Methode. Z. B. Wenn Sie die Department-Tabelle eine Budgetspalte hinzugefügt wurden und alle Abteilung Budgets 1.000,00 $ als Teil der Migration eines initialisieren möchten, können Sie hinzufügen die folgenden Zeile des Codes für die `Up` Methode für die diese Migration:
> 
> `Sql("UPDATE Department SET Budget = 1000");`
> 
> Dieses Beispiel für dieses Lernprogramm verwendet den `AddOrUpdate` Methode in der `Seed` Methode von Code First-Migrationen `Configuration` Klasse. Code First-Migrationen Ruft die `Seed` nach jeder Migration, und diese Methode aktualisiert die Zeilen, die bereits eingefügt wurde, oder fügt sie, wenn sie noch nicht vorhanden sind. Die `AddOrUpdate` Methode ist möglicherweise nicht die beste Wahl für Ihr Szenario. Weitere Informationen finden Sie unter [Achten Sie darauf mit EF 4.3 AddOrUpdate Methode](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) Julie Lerman Blog.


Drücken Sie STRG-UMSCHALT + B, um das Projekt zu erstellen.

Der nächste Schritt ist die Erstellung einer `DbMigration` der anfänglichen Migration-Klasse. Sie möchten diese Migration, um eine neue Datenbank zu erstellen, müssen Sie die Datenbank zu löschen, die bereits vorhanden ist. SQL Server Compact-Datenbanken sind in enthalten *.sdf* Dateien in der *App\_Daten* Ordner. In **Projektmappen-Explorer**, erweitern Sie *App\_Daten* im ContosoUniversity-Projekt, das zwei SQL Server Compact-Datenbanken finden Sie unter dem sind dargestellte *.sdf*Dateien.

Mit der rechten Maustaste die *School.sdf* Datei, und klicken Sie auf **löschen**.

![sdf_files_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image10.png)

In der **Package Manager Console** Fenster, geben Sie den Befehl "hinzufügen Migration anfängliche" zum Erstellen der anfänglichen Migrations, und nennen es "Initial".

![Hinzufügen migration_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image11.png)

Code First-Migrationen erstellt einen anderen Klassendatei in der *Migrationen* Ordner, und diese Klasse enthält Code, der das Schema der Datenbank erstellt.

In der **Package Manager Console**, geben Sie den Befehl "Update-Database" zum Erstellen der Datenbank, und führen Sie die **Ausgangswert** Methode.

![Update-database_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image12.png)

(Wenn Sie eine Fehlermeldung, der angibt erhalten, eine Tabelle ist bereits vorhanden und kann nicht erstellt werden, es ist wahrscheinlich, da die Anwendung ausgeführt wird, nachdem Sie die Datenbank gelöscht und bevor Sie ausgeführt `update-database`. -Verarbeitung, löschen Sie die *School.sdf* Datei erneut, und wiederholen Sie den `update-database` Befehl.)

Führen Sie die Anwendung aus. Jetzt der Seite "Students" ist leer, aber der Seite "Lehrkräfte" enthält Lehrkräfte. Dies ist in der Produktion erhalten Sie nach der Bereitstellung der Anwendung.

![Empty_Students_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image13.png)

![Instructors_page_after_initial_migration](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image14.png)

Das Projekt ist nun bereit für die Bereitstellung der *School* Datenbank.

## <a name="creating-a-membership-database-for-deployment"></a>Erstellen eine Mitgliedschaftsdatenbank für die Bereitstellung

Die University Contoso-Anwendung verwendet die ASP.NET Membership System- und Forms-Authentifizierung zum Authentifizieren und Autorisieren von Benutzern. Eine der Seiten ist nur für Administratoren zugegriffen werden kann. Um diese Seite anzuzeigen, führen Sie die Anwendung, und wählen Sie **Update Gutschriften** aus der Dropdown-Menü unter **Kurse**. Zeigt die Anwendung die **anmelden** Seite, da nur Administratoren autorisiert sind, verwenden die **Update Gutschriften** Seite.

[![Log_in_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image15.png)

Melden Sie sich als "Admin" mit dem Kennwort "Pas$ w0rd" (Beachten Sie die Zahl 0 anstelle der Buchstaben "o" in "w0rd"). Nach dem Anmelden die **Update Gutschriften** angezeigt wird.

[![Update_Credits_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image17.png)

Wenn Sie einen Standort zum ersten Mal bereitstellen, wird häufig ausschließen, die meisten oder alle Benutzerkonten, die Sie zum Testen erstellen. In diesem Fall stellen Sie ein Administratorkonto und keine Benutzerkonten bereit. Anstelle von Testkonten manuell zu löschen, erstellen Sie eine neue Mitgliedschaftsdatenbank, die das Benutzerkonto ein Administrator hat, das Sie in der Produktion zu müssen.

> [!NOTE]
> Die Mitgliedschaftsdatenbank speichert einen Hash der Kennwörter. Zum Bereitstellen von Konten auf einem Computer zu einem anderen müssen Sie sicherstellen, dass hashing Routinen unterschiedliche Hashes auf dem Zielserver kein generieren, als auf dem Quellcomputer ausgeführt. Sie gleichen Hashes generiert bei der Verwendung der ASP.NET Universal Providers, solange Sie nicht den standardmäßigen Algorithmus ändern. Der Standardalgorithmus ist HMACSHA256 und angegeben wird, der **Überprüfung** Attribut von der  **[MachineKey](https://msdn.microsoft.com/en-us/library/w8h3skw9.aspx)**  Element in der Datei "Web.config".


Die Mitgliedschaftsdatenbank wird nicht beibehalten, indem Sie Code First-Migrationen, und es gibt keine automatische Initialisierung, die die Datenbank mit Testkonten Ausgangswerte (wie es für die Datenbank "School"). Um Testdaten verfügbar zu halten müssen Sie daher eine Kopie der Datenbank vornehmen, bevor Sie eine neue Domäne erstellen.

In **Projektmappen-Explorer**, benennen Sie die *aspnet.sdf* in der Datei die *App\_Daten* Ordner *Aspnet-Dev.sdf*. (Erstellen Sie eine Kopie, benennen Sie sie einfach nicht – erstellen Sie eine neue Datenbank in wenigen Augenblicken.)

In **Projektmappen-Explorer**, stellen Sie sicher, dass das Webprojekt (ContosoUniversity, nicht ContosoUniversity.DAL) ausgewählt ist. Klicken Sie dann in der **Projekt** klicken Sie im Menü **ASP.NET-Konfiguration** zum Ausführen der **Websiteverwaltungs-Tool**(WAT).

Wählen Sie die **Sicherheit** Registerkarte.

[![WAT_Security_tab](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image19.png)

Klicken Sie auf **erstellen oder Verwalten von Rollen** und Hinzufügen einer **Administrator** Rolle.

[![WAT_Create_New_Role](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image22.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image21.png)

Navigieren Sie zurück zu den **Sicherheit** auf **Create User**, und Benutzer "Admin" als Administrator hinzufügen. Bevor Sie auf die **Create User** auf die Schaltfläche der **Create User** Seite, stellen Sie sicher, dass die Auswahl der **Administrator** Kontrollkästchen. In diesem Lernprogramm verwendete Kennwort wird "Pas$ w0rd", und Sie können eine beliebige e-Mail-Adresse eingeben.

[![WAT_Create_User](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image24.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image23.png)

Schließen Sie den Browser. In **Projektmappen-Explorer**, klicken Sie auf die Aktualisierungsschaltfläche, um die neue finden Sie unter *aspnet.sdf* Datei.

![New_aspnet.sdf_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image25.png)

Mit der rechten Maustaste **aspnet.sdf** , und wählen Sie **Projekt**.

## <a name="distinguishing-development-from-production-databases"></a>Entwicklung von Produktionsdatenbanken unterscheiden

In diesem Abschnitt müssen Sie die Datenbanken umbenennen, damit die Development-Versionen "School" Dev.sdf sind und Aspnet-Dev.sdf und die Produktionsversionen "School" Prod.sdf und Aspnet-Prod.sdf sind. Dies ist nicht erforderlich, aber dies daher hilft zu verhindern, dass Sie erste Test- und Versionen der Datenbanken zu verwechseln.

In **Projektmappen-Explorer**, klicken Sie auf **aktualisieren** und erweitern Sie die App\_Datenordner finden in der Datenbank "School", die Sie zuvor erstellt haben, der rechten Maustaste darauf und wählen Sie **zu Projekt hinzufügen** .

![Including_School.sdf_in_project](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image26.png)

Benennen Sie *aspnet.sdf* auf *Aspnet-Prod.sdf*.

Benennen Sie *School.sdf* auf *School-Dev.sdf*.

Bei der Ausführung der Anwendung im Visual Studio, die Sie nicht möchten, verwenden Sie die *-Prod* Versionen der Datenbankdateien, die Sie verwenden möchten *- Dev* Versionen. Aus diesem Grund müssen Sie die Verbindungszeichenfolgen in der Datei "Web.config" zu ändern, sodass sie es auf die *- Dev* Versionen der Datenbanken. (Sie noch nicht School-Prod.sdf-Datei erstellt, aber das ist in Ordnung, da Code First, dass die Datenbank in Produktion das erste Mal erstellt wird, die Sie dort Ihre app ausgeführt.)

Öffnen Sie die Anwendungsdatei "Web.config", und suchen Sie die Verbindungszeichenfolgen:

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample5.xml)]

Ändern Sie "aspnet.sdf" in "Aspnet-Dev.sdf", und ändern Sie "School.sdf" in "School-Dev.sdf":

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample6.xml?highlight=4-5)]

Das SQL Server Compact-Datenbankmodul und beide Datenbanken können jetzt bereitgestellt werden. In der folgenden Lernprogramm richten Sie automatische *"Web.config"* Datei Transformationen für Einstellungen, die in die Entwicklungs-, Test- und produktionsumgebung Umgebungen unterschiedlich sein muss. (Zwischen den Einstellungen, die geändert werden müssen, werden die Verbindungszeichenfolgen, aber legen Sie diese Änderungen später bei der Erstellung eines Veröffentlichungsprofils.)

## <a name="more-information"></a>Weitere Informationen

Weitere Informationen zu NuGet, finden Sie unter [verwalten Projektbibliotheken mit NuGet](https://msdn.microsoft.com/en-us/magazine/hh547106.aspx) und [NuGet-Dokumentation](http://docs.nuget.org/docs/start-here/overview). Wenn Sie NuGet verwenden möchten, müssen Sie Informationen zum Analysieren eines NuGet-Pakets, um zu bestimmen, welche Aktion er ausführt, wenn er installiert ist. (sie können z. B. konfigurieren *"Web.config"* Transformationen, konfigurieren Sie PowerShell-Skripts zur Buildzeit usw. ausführen.) Weitere Informationen zur Funktionsweise von NuGet finden Sie unter besonders [erstellen und veröffentlichen ein Paket](http://docs.nuget.org/docs/creating-packages/creating-and-publishing-a-package) und [Konfigurationsdatei und Source Codetransformationen](http://docs.nuget.org/docs/creating-packages/configuration-file-and-source-code-transformations).

>[!div class="step-by-step"]
[Zurück](deployment-to-a-hosting-provider-introduction-1-of-12.md)
[Weiter](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12.md)
