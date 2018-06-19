---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
title: 'Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mit Visual Studio oder Visual Web Developer: Bereitstellen einer SQL Server-Datenbankupdate - 11 12 | Microsoft Docs'
author: tdykstra
description: Diese Reihe von Lernprogrammen wird gezeigt, wie das Bereitstellen einer ASP.NET-Anwendung (veröffentlichen) Webanwendungsprojekt, die eine SQL Server Compact-Datenbank enthält, mithilfe von Visual das...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 5e2bb092-cb22-4511-ad0a-22ae12dd99b3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
msc.type: authoredcontent
ms.openlocfilehash: d8cc072c631900937f31c8f2637869b53d46cf54
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30887329"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-sql-server-database-update---11-of-12"></a>Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mit Visual Studio oder Visual Web Developer: Bereitstellen einer SQL Server-Datenbankupdate - 11 12
====================
durch [Tom Dykstra](https://github.com/tdykstra)

[Startprojekt herunterladen](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Diese Reihe von Lernprogrammen wird gezeigt, wie das Bereitstellen einer ASP.NET-Anwendung (veröffentlichen) Webanwendungsprojekt, die eine SQL Server Compact-Datenbank mithilfe von Visual Studio 2012 RC oder Visual Studio Express 2012 RC für das Web enthält. Sie können auch Visual Studio 2010 verwenden, wenn Sie das Web veröffentlichen Update installieren. Eine Einführung in der Reihe, finden Sie unter [im ersten Lernprogramm, in der Reihe](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Ein Lernprogramm, das zeigt Bereitstellungsfeatures nach der RC-Version von Visual Studio 2012 eingeführt und wird gezeigt, wie zum Bereitstellen von SQL Server-Editionen als SQL Server Compact wird gezeigt, wie Windows Azure-Websites bereitstellen, finden Sie unter [ASP.NET Web Deploy Mithilfe von Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Übersicht

In diesem Lernprogramm wird gezeigt, wie eine Aktualisierung der Datenbank auf einer vollständigen SQL Server-Datenbank bereitstellen. Da Code First-Migrationen auf die gesamte Arbeit aktualisieren die Datenbank verfügt, ist der Prozess nahezu identisch mit was Sie für SQL Server Compact in haben der [Bereitstellen eines Datenbankupdates](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) Lernprogramm.

Hinweis: Wenn Sie eine Fehlermeldung erhalten, oder etwas funktioniert nicht, wenn Sie das Lernprogramm absolvieren, müssen Sie überprüfen die [Website](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="adding-a-new-column-to-a-table"></a>Hinzufügen einer neuen Spalte zu einer Tabelle

In diesem Abschnitt des Lernprogramms treffen Sie eine Datenbank ändern und entsprechende Änderungen am Code, klicken Sie dann in Visual Studio als Vorbereitung für das Bereitstellen in den Test- und produktionsumgebungen Umgebungen testen. Die Änderung umfasst das Hinzufügen einer `OfficeHours` Spalte die `Instructor` Entität und die neuen Informationen in den **Lehrkräfte** Webseite.

Öffnen Sie im Projekt ContosoUniversity.DAL *Instructor.cs* und fügen Sie die folgende Eigenschaft zwischen den `HireDate` und `Courses` Eigenschaften:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample1.cs)]

Aktualisieren Sie die Initialisiererklasse, sodass er die neue Spalte mit Testdaten Ausgangswerte. Open *Migrations\Configuration.cs* , und Ersetzen Sie den Codeblock, der beginnt, `var instructors = new List<Instructor>` mit den folgenden Codeblock enthält die neue Spalte:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample2.cs)]

Öffnen Sie im Projekt ContosoUniversity *Instructors.aspx* und fügen Sie ein neues Vorlagenfeld für Geschäftszeiten unmittelbar vor dem schließenden `</Columns>` Tag in der ersten `GridView` Steuerelement:

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample3.aspx)]

Erstellen Sie die Projektmappe.

Öffnen der **Package Manager Console** Fenster, und wählen ContosoUniversity.DAL als die **Standardprojekt**.

Geben Sie die folgenden Befehle aus:

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample4.ps1)]

Führen Sie die Anwendung, und wählen Sie die **Lehrkräfte** Seite. Dauert es etwas länger als üblich, geladen werden, da das Entity Framework die Datenbank neu erstellt und ihn mit Testdaten startet, die Seite.

[![Instructors_page_with_office_hours](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image1.png)

## <a name="deploying-the-database-update-to-the-test-environment"></a>Das Datenbankupdate bereitstellen, in der Testumgebung

Wenn Sie Code First-Migrationen verwenden, ist die Methode für die Bereitstellung der Änderung an einer Datenbank mit SQL Server genauso wie bei SQL Server Compact. Sie müssen jedoch den Test ändern Veröffentlichungsprofil, da er weiterhin so eingerichtet ist, von SQL Server Compact zu SQL Server zu migrieren.

Der erste Schritt ist die Verbindung Zeichenfolge Transformationen zu entfernen, die Sie im vorherigen Lernprogramm erstellt haben. Diese sind nicht mehr erforderlich, da Verbindung Zeichenfolge Transformationen im Veröffentlichungsprofil, angeben müssen, wie Sie ausgeführt haben, bevor Sie konfiguriert die **SQL packen/veröffentlichen** Registerkarte für die Migration zu SQL Server.

Öffnen der *Web.Test.config* Datei, und entfernen Sie die `connectionStrings` Element. Die einzige verbleibende Transformation in der *Web.Test.config* -Datei ist für die `Environment` Wert in der `appSettings` Element.

Jetzt können Sie das Veröffentlichungsprofil und Veröffentlichen in der testumgebung aktualisieren.

Öffnen der **Web veröffentlichen** -Assistenten, und wechseln Sie dann auf die **Profil** Registerkarte.

Wählen Sie die **Test** Veröffentlichungsprofil.

Wählen Sie die **Einstellungen** Registerkarte.

Klicken Sie auf **aktivieren Sie die neue Datenbank, die publishing Verbesserungen**.

In das Feld die Verbindungszeichenfolge für **SchoolContext**, geben Sie den gleichen Wert, der in verwendet das *Web.Test.config* Transformationsdatei im vorherigen Lernprogramm:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample5.cmd)]

Wählen Sie **führen Sie Code First-Migrationen (wird beim Anwendungsstart ausgeführt)**. (In Ihrer Version von Visual Studio, können Sie das Kontrollkästchen Beschriftung **gelten Code First-Migrationen**.)

In das Feld die Verbindungszeichenfolge für **DefaultConnection**, geben Sie den gleichen Wert, der in verwendet das *Web.Test.config* Transformationsdatei im vorherigen Lernprogramm:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample6.cmd)]

Lassen Sie **Datenbank aktualisieren** deaktiviert.

Klicken Sie auf **Veröffentlichen**.

Visual Studio stellt die codeänderungen in der testumgebung und öffnet den Browser, um die Startseite des Contoso-Universität.

Wählen Sie die Seite "Lehrkräfte".

Wenn die Anwendung auf dieser Seite ausgeführt wird, wird versucht, Zugriff auf die Datenbank. Code First-Migrationen wird überprüft, ob die Datenbank aktuell und feststellt, dass die neueste Migration noch nicht angewendet wurden. Code First-Migrationen die letzte Migration gilt, führt die `Seed` -Methode, und klicken Sie dann auf die Seite normal ausgeführt wird. Sie finden die neue Geschäftszeiten-Spalte mit den per Seeding hinzugefügte Daten.

[![Instructors_page_with_OfficeHours_Test](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image3.png)

## <a name="deploying-the-database-update-to-the-production-environment"></a>Das Datenbankupdate Bereitstellen in der Produktionsumgebung.

Sie müssen auch das Veröffentlichungsprofil für die produktionsumgebung zu ändern. In diesem Fall müssen Sie das vorhandene Profil entfernen und ein neues erstellen, indem Sie eine aktualisierte PUBLISHSETTINGS-Datei importieren. Die aktualisierte Datei wird die Verbindungszeichenfolge für die SQL Server-Datenbank am Cytanium enthalten.

Wie Sie gesehen haben, wenn Sie in der testumgebung bereitgestellt haben, nicht mehr benötigte Verbindung Zeichenfolge Transformationen in der *Web.Production.config* Transformationsdatei. Öffnen, die Datei, und Entfernen der `connectionStrings` Element. Die verbleibenden-Transformationen sind für die `Environment` Wert in der `appSettings` Element und die `location` -Element, das Zugriff auf die Fehlerberichte Elmah beschränkt.

Vor der Erstellung eines neues Veröffentlichungsprofils für die Produktion herunterladen eine aktualisierten PUBLISHSETTINGS-Datei die gleiche Weise wie Sie zuvor in war der [in der Produktionsumgebung bereitstellen](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) Lernprogramm. (Klicken Sie in der Systemsteuerung Cytanium auf **Websites**, und klicken Sie dann auf die **contosouniversity.com** Website. Wählen Sie die **Webveröffentlichung** Registerkarte, und klicken Sie dann auf **Veröffentlichungsprofil herunterladen, für diese Website**.) Der Grund dafür ist, damit die Datenbank-Verbindungszeichenfolge in der PUBLISHSETTINGS-Datei übernommen. Die Verbindungszeichenfolge nicht beim ersten Verwenden Sie die Datei heruntergeladen haben, da weiterhin von SQL Server Compact mithilfe wurden und SQL Server-Datenbank am Cytanium hadn't noch erstellt.

Jetzt können Sie das Veröffentlichungsprofil und Veröffentlichen in der produktionsumgebung aktualisieren.

Öffnen der **Web veröffentlichen** -Assistenten, und wechseln Sie dann auf die **Profil** Registerkarte.

Klicken Sie auf **Profile verwalten**, und klicken Sie dann das Produktions-Profil löschen.

Schließen der **Web veröffentlichen** Assistenten, um diese Änderung zu speichern.

Öffnen der **Web veröffentlichen** Assistenten erneut aus, und klicken Sie dann auf **Import**.

Auf der **Verbindung** Registerkarte **Ziel-URL** auf den entsprechenden Wert, wenn Sie eine temporäre URL verwenden.

Klicken Sie auf **Weiter**.

Auf der **Einstellungen** auf **aktivieren Sie die neue Datenbank, die publishing Verbesserungen**.

In der Verbindung Zeichenfolge Dropdown-Liste für **SchoolContext**, wählen Sie die Verbindungszeichenfolge Cytanium.

![Selecting_Cytanium_connection_string](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image5.png)

Wählen Sie **führen Sie Code First-Migrationen (wird beim Anwendungsstart ausgeführt)**.

In der Verbindung Zeichenfolge Dropdown-Liste für **DefaultConnection**, wählen Sie die Verbindungszeichenfolge Cytanium.

Wählen Sie die **Profil** auf **Profile verwalten**, und benennen Sie das Profil aus "contosouniversity.com - Web Deploy" in "Produktion".

Schließen Sie das Veröffentlichungsprofil, um die Änderung zu speichern, und öffnen Sie es erneut.

Klicken Sie auf **Veröffentlichen**. (Für eine echte Produktionswebsite, kopieren Sie *app\_offline.htm* in der Produktions- und Put es in den Projektordner vor der Veröffentlichung, entfernen Sie ihn dann bei der Bereitstellung abgeschlossen ist.)

Visual Studio stellt die codeänderungen in der testumgebung und öffnet den Browser, um die Startseite des Contoso-Universität.

Wählen Sie die Seite "Lehrkräfte".

Code First-Migrationen aktualisiert die Datenbank die gleiche Weise, wie in der testumgebung. Sie finden die neue Geschäftszeiten-Spalte mit den per Seeding hinzugefügte Daten.

![Instructors_page_with_OfficeHours_Prod](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image6.png)

Sie haben jetzt erfolgreich bereitgestellt eine Anwendung aktualisieren, die eine datenbankänderung enthalten mithilfe einer SQL Server-Datenbank.

## <a name="more-information"></a>Weitere Informationen

Dies schließt diese Reihe von Lernprogrammen zum Bereitstellen einer ASP.NET-WEBANWENDUNG zwischen einem Hostinganbieter eines Drittanbieters. Weitere Informationen zu den Themen in diesen Lernprogrammen behandelt, finden Sie unter der [ASP.NET Deployment Content Map](https://msdn.microsoft.com/library/bb386521(v=vs.110).aspx) auf der MSDN-Website.

## <a name="acknowledgements"></a>Bestätigungen

Ich möchte die vielen Dank, dass die folgenden Personen, die bedeutende Beiträge auf den Inhalt dieser Reihe von Lernprogrammen vorgenommen:

- [Alberto Poblacion, MVP &amp; MCT, Spanien](https://mvp.support.microsoft.com/profile/Alberto)
- Jarod Ferguson, Plattform die Entwicklung von Data-MVP, USA
- Harsh Mittal, Microsoft
- [Kristina Olson, Microsoft](https://blogs.iis.net/krolson/default.aspx)
- [Mike PPPoE, Microsoft](http://www.mikepope.com/blog/DisplayBlog.aspx)
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
