---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
title: 'Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mit Visual Studio oder Visual Web Developer: Bereitstellen einer SQL Server-Datenbank-Update - 11 12 | Microsoft-Dokumentation'
author: tdykstra
description: In dieser tutorialreihe erfahren Sie, wie zum Bereitstellen einer ASP.NET-Anwendung (veröffentlichen) Webanwendungsprojekt, die eine SQL Server Compact-Datenbank enthält, mithilfe von Visual Stu...
ms.author: aspnetcontent
ms.date: 11/17/2011
ms.assetid: 5e2bb092-cb22-4511-ad0a-22ae12dd99b3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
msc.type: authoredcontent
ms.openlocfilehash: c744bc54c8ce12820d1e1388ac7ab2db814ff031
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37816627"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-sql-server-database-update---11-of-12"></a>Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mit Visual Studio oder Visual Web Developer: Bereitstellen einer SQL Server-Datenbank-Update - 11 12
====================
durch [Tom Dykstra](https://github.com/tdykstra)

[Startprojekt herunterladen](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> In dieser tutorialreihe erfahren Sie, wie zum Bereitstellen einer ASP.NET-Anwendung (veröffentlichen) Webanwendungsprojekt, das eine SQL Server Compact-Datenbank mithilfe von Visual Studio 2012 RC oder Visual Studio Express 2012 RC für Web enthält. Sie können auch Visual Studio 2010 verwenden, wenn Sie die Web Publish Update installieren. Eine Einführung in die Reihe, finden Sie unter [im ersten Tutorial der Reihe](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Ein Lernprogramm, das zeigt, Bereitstellungsfunktionen, die nach der RC-Version von Visual Studio 2012 eingeführt wurden, zeigt, wie zum Bereitstellen von SQL Server-Editionen als SQL Server Compact und zeigt, wie in Windows Azure-Websites bereitstellen, finden Sie unter [ASP.NET-webbereitstellung Mithilfe von Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Übersicht

In diesem Tutorial erfahren Sie, wie Sie eine Aktualisierung der Datenbank auf einer vollständigen SQL Server-Datenbank bereitstellen. Der Prozess ist fast identisch mit für SQL Server Compact in bisheriges vorgehen, da der Code First-Migrationen Aktualisieren der Datenbank alle Aufgaben durchführt, die [Bereitstellen eines Datenbankupdates](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) Tutorial.

Erinnerung: Wenn Sie eine Fehlermeldung erhalten, oder etwas nicht funktioniert, wie Sie das Lernprogramm durchzuarbeiten, sollten Sie unbedingt die [Problembehandlungsseite](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="adding-a-new-column-to-a-table"></a>Hinzufügen einer neuen Spalte in einer Tabelle

In diesem Abschnitt des Tutorials stellen Sie eine Datenbank zu ändern und entsprechende Änderungen am Code, klicken Sie dann diese testen in Visual Studio als Vorbereitung für die sie auf die Test- und produktionsumgebungen Umgebungen bereitzustellen. Die Änderung umfasst das Hinzufügen einer `OfficeHours` Spalte die `Instructor` Entität und Anzeigen von den neuen Informationen in der **Dozenten** Webseite.

Öffnen Sie im Projekt ContosoUniversity.DAL *Instructor.cs* und fügen Sie die folgende Eigenschaft zwischen den `HireDate` und `Courses` Eigenschaften:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample1.cs)]

Aktualisieren Sie Initialisiererklasse, sodass die neue Spalte mit Testdaten datengeneratoranwendung gestartet. Open *migrations\configuration. cs* , und Ersetzen Sie den Codeblock, der beginnt `var instructors = new List<Instructor>` mit den folgenden Codeblock enthält die neue Spalte:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample2.cs)]

Öffnen Sie im Projekt ContosoUniversity *Instructors.aspx* und fügen Sie ein neues Vorlagenfeld für Geschäftszeiten direkt vor dem abschließenden `</Columns>` Tag in der ersten `GridView` Steuerelement:

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample3.aspx)]

Erstellen Sie die Projektmappe.

Öffnen der **-Paket-Manager-Konsole** angezeigt, und wählen ContosoUniversity.DAL als die **Standardprojekt**.

Geben Sie die folgenden Befehle ein:

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample4.ps1)]

Führen Sie die Anwendung, und wählen Sie die **Dozenten** Seite. Es dauert etwas länger als gewöhnlich zum geladen werden, da das Entity Framework die Datenbank neu erstellt und diese mit Testdaten startet, die Seite.

[![Instructors_page_with_office_hours](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image1.png)

## <a name="deploying-the-database-update-to-the-test-environment"></a>Bereitstellen des Datenbankupdates in der Testumgebung

Wenn Sie Code First-Migrationen verwenden, ist die Methode für die Bereitstellung der Änderung an einer Datenbank in SQL Server genauso wie bei SQL Server Compact. Allerdings müssen Sie so ändern den Test Veröffentlichungsprofil aus, da es immer noch so eingerichtet ist, Migrieren von SQL Server Compact auf SQL Server.

Der erste Schritt ist die Transformationen für die Verbindung Zeichenfolge zu entfernen, die Sie im vorherigen Tutorial erstellt haben. Diese sind nicht mehr erforderlich, da Connection String Transformationen im Veröffentlichungsprofil, angeben wird wie vor dem Konfigurieren der Sie die **SQL packen/veröffentlichen** Registerkarte für die Migration zu SQL Server.

Öffnen der *Web.Test.config* Datei, und entfernen Sie die `connectionStrings` Element. Die einzige verbleibende Transformation in der *Web.Test.config* -Datei ist für die `Environment` Wert in der `appSettings` Element.

Jetzt können Sie das Veröffentlichungsprofil und Veröffentlichen in die testumgebung aktualisieren.

Öffnen der **Webveröffentlichung** -Assistenten, und klicken Sie dann wechseln Sie zu der **Profil** Registerkarte.

Wählen Sie die **Test** Veröffentlichungsprofil.

Wählen Sie die **Einstellungen** Registerkarte.

Klicken Sie auf **aktivieren Sie die neue Datenbank veröffentlichen Verbesserungen**.

Klicken Sie in das Feld die Verbindungszeichenfolge für **SchoolContext**, geben Sie den gleichen Wert, der in verwendet die *Web.Test.config* Transformationsdatei im vorherigen Tutorial:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample5.cmd)]

Wählen Sie **Code First-Migrationen ausführen (wird beim Anwendungsstart ausgeführt)**. (In Ihrer Version von Visual Studio, das Kontrollkästchen kann mit der Bezeichnung **anwenden Code First-Migrationen**.)

Klicken Sie in das Feld die Verbindungszeichenfolge für **DefaultConnection**, geben Sie den gleichen Wert, der in verwendet die *Web.Test.config* Transformationsdatei im vorherigen Tutorial:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample6.cmd)]

Lassen Sie **Aktualisieren einer Datenbank** gelöscht.

Klicken Sie auf **Veröffentlichen**.

Visual Studio stellt die codeänderungen in die testumgebung bereit und öffnet den Browser mit der Contoso University-Startseite.

Wählen Sie die dozentenseite.

Wenn die Anwendung auf dieser Seite ausgeführt wird, versucht, Zugriff auf die Datenbank. Code First-Migrationen wird überprüft, ob die Datenbank aktuell ist und feststellt, dass die neueste Migration noch nicht angewendet wurde. Code First-Migrationen die neueste Migration gilt, führt die `Seed` -Methode, und klicken Sie dann die Seite wird normal ausgeführt. Daraufhin wird die neue Office-Stunden-Spalte mit den per Seeding hinzugefügten Daten.

[![Instructors_page_with_OfficeHours_Test](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image3.png)

## <a name="deploying-the-database-update-to-the-production-environment"></a>Das Datenbankupdate Bereitstellen in der Produktionsumgebung

Sie müssen auch das Veröffentlichungsprofil für die produktionsumgebung zu ändern. In diesem Fall müssen Sie das vorhandene Profil zu entfernen und erstellen eine neue, indem Sie eine aktualisierte PUBLISHSETTINGS-Datei importieren. Die aktualisierte Datei enthält die Verbindungszeichenfolge für die SQL Server-Datenbank am Cytanium.

Wie Sie gesehen haben, wenn Sie in der testumgebung bereitgestellt, nicht mehr benötigten Connection String Transformationen in der *Web.Production.config* Transformationsdatei. Öffnen Sie die Datei, und entfernen Sie die `connectionStrings` Element. Die restlichen Transformationen sind für die `Environment` Wert in der `appSettings` Element und die `location` -Element, das Zugriff auf Fehlerberichte von Elmah beschränkt.

Bevor Sie ein neues Veröffentlichungsprofil für die Produktion erstellen, laden eine aktualisierten PUBLISHSETTINGS-Datei die gleiche Weise wie weiter oben in der [in der Produktionsumgebung bereitstellen](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) Tutorial. (Klicken Sie in der Systemsteuerung Cytanium **Websites**, und klicken Sie dann auf die **contosouniversity.com** Website. Wählen der **Webveröffentlichung** Registerkarte, und klicken Sie dann auf **Veröffentlichungsprofil herunterladen, für diese Website**.) Der Grund dafür ist, um die Datenbank-Verbindungszeichenfolge in der Datei ".publishsettings" zu übernehmen. Die Verbindungszeichenfolge nicht beim ersten der Datei, herunterladen da Sie immer noch SQL Server Compact verwendet haben und SQL Server-Datenbank auf Cytanium noch nicht noch erstellt.

Jetzt können Sie das Veröffentlichungsprofil und Veröffentlichen in der produktionsumgebung aktualisieren.

Öffnen der **Webveröffentlichung** -Assistenten, und klicken Sie dann wechseln Sie zu der **Profil** Registerkarte.

Klicken Sie auf **Profile verwalten**, und löschen Sie dann das Profil für die Produktion.

Schließen der **Webveröffentlichung** Assistenten, um diese Änderung zu speichern.

Öffnen der **Webveröffentlichung** Assistenten erneut aus, und klicken Sie dann auf **Import**.

Auf der **Verbindung** Registerkarte **Ziel-URL** auf den entsprechenden Wert bei Verwendung eine temporäre URL.

Klicken Sie auf **Weiter**.

Auf der **Einstellungen** auf **aktivieren Sie die neue Datenbank veröffentlichen Verbesserungen**.

In der Verbindung Zeichenfolge Dropdown-Liste für **SchoolContext**, wählen Sie die Verbindungszeichenfolge Cytanium.

![Selecting_Cytanium_connection_string](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image5.png)

Wählen Sie **führen Sie Code First-Migrationen (wird beim Anwendungsstart ausgeführt)**.

In der Verbindung Zeichenfolge Dropdown-Liste für **DefaultConnection**, wählen Sie die Verbindungszeichenfolge Cytanium.

Wählen Sie die **Profil** auf **Profile verwalten**, und benennen Sie das Profil aus "contosouniversity.com – Web Deploy" in "Produktion".

Schließen Sie das Veröffentlichungsprofil, um die Änderung zu speichern, und öffnen Sie es erneut.

Klicken Sie auf **Veröffentlichen**. (Kopieren Sie für eine Website Produktion *app\_offline.htm* für Produktions- und Put in Ihrem Projektordner vor der Veröffentlichung, entfernen sie Sie dann bei der Bereitstellung abgeschlossen ist.)

Visual Studio stellt die codeänderungen in die testumgebung bereit und öffnet den Browser mit der Contoso University-Startseite.

Wählen Sie die dozentenseite.

Code First-Migrationen aktualisiert die Datenbank die gleiche Weise wie in der testumgebung zuvor. Daraufhin wird die neue Office-Stunden-Spalte mit den per Seeding hinzugefügten Daten.

![Instructors_page_with_OfficeHours_Prod](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image6.png)

Sie haben jetzt erfolgreich bereitgestellt ein Anwendungsupdates, das Änderung an einer Datenbank, enthalten mithilfe einer SQL Server-Datenbank.

## <a name="more-information"></a>Weitere Informationen

Dies schließt diese tutorialreihe zum Bereitstellen einer ASP.NET-Webanwendung zu einem Hostinganbieter von Drittanbietern. Weitere Informationen zu den Themen in diesen Tutorials behandelt, finden Sie unter den [ASP.NET die ASP.NET-Bereitstellung](https://msdn.microsoft.com/library/bb386521(v=vs.110).aspx) auf der MSDN-Website.

## <a name="acknowledgements"></a>Bestätigungen

Ich möchte die vielen Dank, dass die folgenden Personen, die wichtige Beiträge zu den Inhalt dieser tutorialreihe vorgenommen:

- [Alberto Poblacion, MVP &amp; MCT, Spanien](https://mvp.support.microsoft.com/profile/Alberto)
- Jarod Ferguson, Entwicklung von Data Platform MVP, Vereinigte Staaten
- Harsh Mittal, Microsoft
- [Kristina Olson, Microsoft](https://blogs.iis.net/krolson/default.aspx)
- [Mike Papst, Microsoft](http://www.mikepope.com/blog/DisplayBlog.aspx)
- Mohit Srivastava, Microsoft
- [Raffaele Rialdi, Italien](http://www.iamraf.net/)
- [Rick Anderson, Microsoft](https://blogs.msdn.com/b/rickandy/)
- [Sayed Hashimi, Microsoft](http://sedodream.com/default.aspx)(twitter: [ @sayedihashimi ](http://twitter.com/sayedihashimi))
- [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](http://twitter.com/shanselman))
- [Scott Hunter, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [ @coolcsh ](http://twitter.com/coolcsh))
- [Srđan Božović, Serbia](http://msforge.net/blogs/zmajcek/)
- [Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [ @vishalrjoshi ](http://twitter.com/vishalrjoshi))

> [!div class="step-by-step"]
> [Zurück](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
> [Weiter](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)
