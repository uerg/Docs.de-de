---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12
title: 'Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mit Visual Studio oder Visual Web Developer: Bereitstellen einer Code-Only-Update – 8 von 12 | Microsoft-Dokumentation'
author: tdykstra
description: In dieser tutorialreihe erfahren Sie, wie zum Bereitstellen einer ASP.NET-Anwendung (veröffentlichen) Webanwendungsprojekt, die eine SQL Server Compact-Datenbank enthält, mithilfe von Visual Stu...
ms.author: aspnetcontent
ms.date: 11/17/2011
ms.assetid: ddf6252f-9413-4c0c-a360-2cef8d231717
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12
msc.type: authoredcontent
ms.openlocfilehash: 16a9e48034536964d526717ecde2a9b813818963
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37816083"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-code-only-update---8-of-12"></a>Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mit Visual Studio oder Visual Web Developer: Bereitstellen einer Code-Only-Update – 8 von 12
====================
durch [Tom Dykstra](https://github.com/tdykstra)

[Startprojekt herunterladen](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> In dieser tutorialreihe erfahren Sie, wie zum Bereitstellen einer ASP.NET-Anwendung (veröffentlichen) Webanwendungsprojekt, das eine SQL Server Compact-Datenbank mithilfe von Visual Studio 2012 RC oder Visual Studio Express 2012 RC für Web enthält. Sie können auch Visual Studio 2010 verwenden, wenn Sie die Web Publish Update installieren. Eine Einführung in die Reihe, finden Sie unter [im ersten Tutorial der Reihe](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Ein Lernprogramm, das zeigt, Bereitstellungsfunktionen, die nach der RC-Version von Visual Studio 2012 eingeführt wurden, zeigt, wie zum Bereitstellen von SQL Server-Editionen als SQL Server Compact und zeigt, wie Sie in Azure App Service-Web-Apps bereitstellen, finden Sie unter [ASP.NET-webbereitstellung Mithilfe von Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Übersicht

Nach der erstbereitstellung Ihre Arbeit verwalten und entwickeln Ihre Website wird fortgesetzt, und vor dem Long-Wert wird ein Update bereitstellen möchten. Dieses Tutorial führt Sie durch den Prozess der Bereitstellung eines Updates an Ihrem Anwendungscode. Dieses Update muss keine Änderung an einer Datenbank werden; sehen Sie, ob Sie zum Bereitstellen der Änderung an einer Datenbank im nächsten Tutorial unterschiedlich ist.

Erinnerung: Wenn Sie eine Fehlermeldung erhalten, oder etwas nicht funktioniert, wie Sie das Lernprogramm durchzuarbeiten, sollten Sie unbedingt die [Problembehandlungsseite](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="making-a-code-change"></a>Eine Codeänderung vornimmt

Fügen Sie ein einfaches Beispiel eines Updates für Ihre Anwendung, um die **Dozenten** Seite eine Liste der Kurse, die von den ausgewählten Dozenten unterrichtet.

Wenn das Ausführen der **Dozenten** Seite werden Sie feststellen, dass es gibt **wählen** Verknüpfungen im Raster, aber nicht der Fall ist etwas anderes als stellen die Zeile Background grau dargestellt.

[![Instructors_page](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image1.png)

Nachdem Sie Code hinzufügen, die ausgeführt, wenn wird die **wählen** Link geklickt wird, und zeigt eine Liste der Kurse, die von den ausgewählten Dozenten unterrichtet.

In *Instructors.aspx*, fügen Sie das folgende Markup direkt nach der **ErrorMessageLabel** `Label` Steuerelement:

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/samples/sample1.aspx)]

Führen Sie die Seite, und wählen Sie einen Dozenten. Sie sehen eine Liste der Kurse, die durch dieses Dozenten unterrichtet.

[![Instructors_page_with_courses](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image3.png)

## <a name="deploying-the-code-update-to-the-test-environment"></a>Bereitstellen des Code-Updates in der Testumgebung

Bereitstellung in der testumgebung ist einfach der Ausführung von nur einem Klick erneut veröffentlichen. Um diesen Prozess zu beschleunigen, können Sie die **klicken Sie auf Webveröffentlichung mit einem** Symbolleiste.

In der **Ansicht** Menü wählen **Symbolleisten** und wählen Sie dann **klicken Sie auf Webveröffentlichung mit einem**.

![Selecting_One_Click_Publish_toolbar](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image5.png)

In **Projektmappen-Explorer**, wählen Sie das Projekt ContosoUniversity.

die **klicken Sie auf Webveröffentlichung mit einem** Symbolleiste wählen Sie die **Test** Veröffentlichungsprofil aus, und klicken Sie dann auf **Webveröffentlichung** (das Symbol mit den Pfeilen nach links und rechts).

![Web_One_Click_Publish_toolbar](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image6.png)

Visual Studio stellt die aktualisierte Anwendung bereit, und der Browser automatisch geöffnet wird, auf der Startseite. Führen Sie die dozentenseite, und wählen Sie einen Dozenten aus, um sicherzustellen, dass das Update wurde erfolgreich bereitgestellt wurde.

[![Instructors_page_with_courses_Test](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image7.png)

Sie würden normalerweise auch tun Regressionstests (d. h. den Rest des Standorts, um sicherzustellen, dass die neue Änderung keine vorhandene Funktionalität nicht beeinträchtigt haben testen). Aber für dieses Tutorial überspringen Sie diesen Schritt und fahren Sie mit der Bereitstellung des Updates für die Produktion.

## <a name="preventing-redeployment-of-the-initial-database-state-to-production"></a>Verhindern, dass für die erneute Bereitstellung des ersten Datenbankstatus in der Produktion

In einer echten Anwendung Benutzer interagieren mit Ihren Produktionsstandort nach Ihrer anfänglichen Bereitstellung und die Datenbanken mit live-Daten aufgefüllt werden. Aus diesem Grund möchten nicht die Mitgliedschaftsdatenbank in den Anfangszustand, erneut bereitstellen, das alle live-Daten zurücksetzen würden. Da die Dateien in SQL Server Compact-Datenbanken sind die *App\_Daten* Ordner müssen Sie dies verhindern, indem Sie die bereitstellungseinstellungen ändern, damit, die in Dateien der *App\_Daten* Ordner werden nicht bereitgestellt werden.

Öffnen der **Projekteigenschaften** Fenster für das ContosoUniversity-Projekt, und wählen Sie die **Web packen/veröffentlichen** Registerkarte. Stellen Sie sicher, dass die **Konfiguration** Dropdown-Listenfeld wurde entweder **aktiv (Release)** oder **Version** ausgewählt haben, wählen Sie **Ausschließen von Dateien aus der App\_Datenordner**.

![Exclude_files_from_the_App_Data_folder](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image9.png)

Für den Fall, dass Sie einen Debugbuild in Zukunft bereitstellen möchten, ist es eine gute Idee, die die gleiche Änderung für die Debug-Buildkonfiguration vornehmen: Ändern Sie **Konfiguration** zu **Debuggen** und wählen Sie dann **ausschließen Dateien aus der App\_Datenordner**.

Speichern und schließen Sie die **Web packen/veröffentlichen** Registerkarte.

> [!NOTE] 
> 
> [!IMPORTANT]
> Stellen Sie sicher, dass Sie keine **entfernen weiterer Dateien am Ziel** in Ihren Profilen veröffentlichen ausgewählt. Wenn Sie diese Option auswählen, werden während des Bereitstellungsvorgangs die Datenbanken, die Sie in der App gelöscht\_löschen Sie Daten in der bereitgestellten Website, und es werden die App\_Datenordner selbst.


## <a name="preventing-user-access-to-the-production-site-during-update"></a>Verhindern, dass Benutzerzugriff auf den Produktionsstandort während des Updates

Die Änderung, die Sie bereitstellen, ist jetzt eine einfache Änderung an einer einzelnen Seite. Aber manchmal Sie größere Änderungen bereitstellen und in diesem Fall die Website kann sich seltsam Verhalten, wenn ein Benutzer eine Seite anfordert, bevor die Bereitstellung abgeschlossen ist. Um dies zu verhindern, können Sie eine *app\_offline.htm* Datei. Wenn Sie eine Datei namens einfügen *app\_offline.htm* im Stammordner Ihrer Anwendung, IIS automatisch die Datei statt der Ausführung Ihrer Anwendung angezeigt. Damit um den Zugriff während der Bereitstellung zu verhindern, Sie fügen *app\_offline.htm* im Stammordner, den Bereitstellungsvorgang ausführen, und entfernen Sie *app\_offline.htm*.

In **Projektmappen-Explorer**mit der rechten Maustaste auf die Projektmappe (nicht auf eines der Projekte), und wählen Sie **Neuer Projektmappenordner**.

![Creating_a_solution_folder](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image10.png)

Nennen Sie den Ordner *SolutionFiles*.

In den neuen Ordner erstellen Sie eine HTML-Seite, die mit dem Namen *app\_offline.htm*. Ersetzen Sie den vorhandenen Inhalt durch Folgendes Markup:

[!code-html[Main](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/samples/sample2.html)]

Sie kopieren können die *app\_offline.htm* Datei mit dem Standort mithilfe von FTP-Verbindung oder die **Manager für Dateiserver** Hilfsprogramm in der Systemsteuerung des Hostinganbieters. In diesem Tutorial verwenden Sie die **Manager für Dateiserver**.

Öffnen Sie die Systemsteuerung, und wählen **Manager für Dateiserver** wie in der [in der Produktionsumgebung bereitstellen](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) Tutorial. Wählen Sie **contosouniversity.com** und dann **"Wwwroot"** zum Stammordner Ihrer Anwendung zu erhalten, und klicken Sie auf **hochladen**.

[![Upload_button_in_File_Manager](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image12.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image11.png)

In der **Datei hochladen** wählen Sie im Dialogfeld die *app\_offline.htm* Datei, und klicken Sie dann auf **hochladen**.

[![Upload_dialog_box_in_File_Manager](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image14.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image13.png)

Navigieren Sie zu Ihrer Website-URL. Sie sehen, dass die *app\_offline.htm* Seite wird nun anstelle Ihrer Startseite angezeigt.

[![App_offline.htm_page_in_production](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image15.png)

Sie können nun in der produktionsumgebung bereitstellen.

## <a name="deploying-the-code-update-to-the-production-environment"></a>Die Codeaktualisierung Bereitstellen in der Produktionsumgebung

In der **klicken Sie auf Webveröffentlichung mit einem** Symbolleiste wählen Sie die **Produktion** Veröffentlichungsprofil aus, und klicken Sie dann auf **Webveröffentlichung**.

Visual Studio stellt die aktualisierte Anwendung bereit und öffnet der Browser zur Startseite der Website. Die *app\_offline.htm* Datei wird angezeigt. Bevor Sie zum Überprüfen der erfolgreichen Bereitstellung testen können, müssen Sie entfernen die *app\_offline.htm* Datei.

Wechseln Sie zurück zur der **Manager für Dateiserver** Anwendung in der Systemsteuerung. Wählen Sie **contosouniversity.com** und **"Wwwroot"** Option **app\_offline.htm**, und klicken Sie dann auf **löschen**.

[![Deleting_app_offline.htm](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image17.png)

Klicken Sie im Browser öffnen Sie die dozentenseite in der öffentlichen Website, und wählen Sie einen Dozenten aus, um sicherzustellen, dass das Update wurde erfolgreich bereitgestellt wurde.

[![Instructors_page_with_courses_Prod](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image19.png)

Sie haben jetzt ein Anwendungsupdate bereitgestellt, die keine Änderung an einer Datenbank einschließen. Im nächste Tutorial erfahren Sie, wie Sie die Änderung an einer Datenbank bereitstellen.

> [!div class="step-by-step"]
> [Zurück](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)
> [Weiter](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)
