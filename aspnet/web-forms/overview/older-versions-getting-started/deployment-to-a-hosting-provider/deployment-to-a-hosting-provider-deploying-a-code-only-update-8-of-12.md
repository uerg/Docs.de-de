---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12
title: 'Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mit Visual Studio oder Visual Web Developer: Bereitstellen von einem Code-Only-Update - 8 12 | Microsoft Docs'
author: tdykstra
description: Diese Reihe von Lernprogrammen wird gezeigt, wie das Bereitstellen einer ASP.NET-Anwendung (veröffentlichen) Webanwendungsprojekt, die eine SQL Server Compact-Datenbank enthält, mithilfe von Visual das...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: ddf6252f-9413-4c0c-a360-2cef8d231717
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12
msc.type: authoredcontent
ms.openlocfilehash: 6305a8cb87d9b00b6bb4c4f8fa6114bddc4eb89f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30886913"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-code-only-update---8-of-12"></a>Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mit Visual Studio oder Visual Web Developer: Bereitstellen von einem Code-Only-Update - 8 von 12
====================
durch [Tom Dykstra](https://github.com/tdykstra)

[Startprojekt herunterladen](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Diese Reihe von Lernprogrammen wird gezeigt, wie das Bereitstellen einer ASP.NET-Anwendung (veröffentlichen) Webanwendungsprojekt, die eine SQL Server Compact-Datenbank mithilfe von Visual Studio 2012 RC oder Visual Studio Express 2012 RC für das Web enthält. Sie können auch Visual Studio 2010 verwenden, wenn Sie das Web veröffentlichen Update installieren. Eine Einführung in der Reihe, finden Sie unter [im ersten Lernprogramm, in der Reihe](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Ein Lernprogramm, die die Bereitstellungsfeatures nach der RC-Version von Visual Studio 2012 eingeführt wurde, wird gezeigt, wie SQL Server-Editionen als SQL Server Compact bereitstellen, und zeigt die Vorgehensweise beim Bereitstellen in Azure App Service-Web-Apps, finden Sie unter [ASP.NET Web Deploy Mithilfe von Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Übersicht

Nach der anfänglichen Bereitstellung können Ihre Arbeit verwalten und entwickeln Ihre Website fortgesetzt und lange wird eine Aktualisierung bereitstellen möchten. Dieses Lernprogramm führt Sie durch den Prozess der Bereitstellung von Aktualisierungen an den Anwendungscode. Dieses Update beinhaltet keine Änderung an einer Datenbank; sehen Sie, ob Sie zum Bereitstellen der Änderung an einer Datenbank in den nächsten Lernprogrammen unterschiedlich ist.

Hinweis: Wenn Sie eine Fehlermeldung erhalten, oder etwas funktioniert nicht, wenn Sie das Lernprogramm absolvieren, müssen Sie überprüfen die [Website](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="making-a-code-change"></a>Eine codeänderung

Als einfaches Beispiel eines Updates für Ihre Anwendung, fügen Sie auf der **Lehrkräfte** Seite eine Liste der ausgewählten Dozent unterrichtet Kurse.

Wenn das Ausführen der **Lehrkräfte** Seite werden Sie feststellen, dass es gibt **wählen** Links im Raster, aber nicht der Fall ist etwas anderes als stellen die Zeile Background grau dargestellt.

[![Instructors_page](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image1.png)

Nachdem Sie Code hinzufügen, die ausgeführt, wenn wird die **wählen** Link geklickt wird, und zeigt eine Liste der ausgewählten Dozent unterrichtet Kurse.

In *Instructors.aspx*, fügen Sie das folgende Markup unmittelbar nach der **ErrorMessageLabel** `Label` Steuerelement:

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/samples/sample1.aspx)]

Führen Sie die Seite, und wählen Sie einen Kursleiter. Sie sehen eine Liste der Kurse, Dozent unterrichtet.

[![Instructors_page_with_courses](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image3.png)

## <a name="deploying-the-code-update-to-the-test-environment"></a>Bereitstellen des Code-Updates in der Testumgebung

Bereitstellung in der testumgebung wird problemlos der Ausführung nur einem Klick erneut veröffentlichen. Um diesen Prozess zu beschleunigen, können Sie die **Web eine klicken Sie auf Publish** Symbolleiste.

In der **Ansicht** Menü Wählen Sie **Symbolleisten** und wählen Sie dann **Web eine klicken Sie auf Publish**.

![Selecting_One_Click_Publish_toolbar](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image5.png)

In **Projektmappen-Explorer**, wählen Sie das Projekt ContosoUniversity.

die **Web eine klicken Sie auf Publish** Symbolleiste, wählen Sie die **Test** Veröffentlichungsprofil, und klicken Sie dann auf **Web veröffentlichen** (das Symbol mit den Pfeilen nach links und rechts).

![Web_One_Click_Publish_toolbar](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image6.png)

Visual Studio wird die aktualisierte Anwendung bereitgestellt, und der Browser automatisch geöffnet wird, um zur Startseite. Führen Sie die Seite Lehrkräfte, und wählen Sie einen Kursleiter, um sicherzustellen, dass das Update erfolgreich bereitgestellt wurde.

[![Instructors_page_with_courses_Test](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image7.png)

Sie würden normalerweise Gleiches Regressionstests (d. h. den Rest des Standorts, um sicherzustellen, dass die neue Änderung vorhandene Funktionalität unterbrechen nicht testen). Aber für dieses Lernprogramm überspringen Sie diesen Schritt und fahren Sie mit das Update für die Produktion bereitstellen.

## <a name="preventing-redeployment-of-the-initial-database-state-to-production"></a>Verhindern, dass für die erneute Bereitstellung des ersten Datenbankstatus bis hin zur Produktion

In einer realen Anwendung Benutzer interagieren mit Ihren Produktionsstandort nach Ihrer anfänglichen Bereitstellung und die Datenbanken mit live-Daten aufgefüllt werden. Daher möchten Sie die Mitgliedschaftsdatenbank seinen anfänglichen aktiven Status erneut bereitstellen, die alle die Livedaten zurücksetzen würden. Da SQL Server Compact-Datenbanken sind, die Dateien in den *App\_Daten* Ordner, müssen Sie dies verhindern, indem Sie die bereitstellungseinstellungen ändern, damit, die in Dateien der *App\_Daten* Ordner werden nicht bereitgestellt.

Öffnen der **Projekteigenschaften** Fenster für die ContosoUniversity-Projekt, und wählen die **Web packen/veröffentlichen** Registerkarte. Stellen Sie sicher, dass die **Konfiguration** Dropdown-Menü wurde entweder **aktiv (Release)** oder **Release** ausgewählt ist, wählen Sie **Ausschließen von Dateien aus der App\_Datenordner**.

![Exclude_files_from_the_App_Data_folder](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image9.png)

Für den Fall, dass Sie einen Debugbuild in der Zukunft bereitstellen möchten, ist es eine gute Idee, machen die gleiche Änderung für die Debug-Buildkonfiguration: Ändern Sie **Konfiguration** auf **Debuggen** und wählen Sie dann **ausschließen Dateien aus der App\_Datenordner**.

Speichern und schließen Sie die **Web packen/veröffentlichen** Registerkarte.

> [!NOTE] 
> 
> [!IMPORTANT]
> Stellen Sie sicher, dass Sie keine **entfernen weiterer Dateien am Ziel** in Ihre Veröffentlichungsprofile ausgewählt. Wenn Sie diese Option auswählen, wird der Bereitstellungsprozess die Datenbanken, die App verwendet löschen\_Daten in den bereitgestellten Standort aus, und löschen Sie die App werden\_Datenordner selbst.


## <a name="preventing-user-access-to-the-production-site-during-update"></a>Benutzerzugriff auf die Produktionsstandort zu verhindern, während der Aktualisierung

Die Änderung, die Sie bereitstellen, ist jetzt eine einfache Änderung an einer einzelnen Seite. Aber manchmal Sie größere Änderungen bereitstellen und in diesem Fall die Website seltsames Verhalten kann, wenn ein Benutzer eine Seite anfordert, bevor die Bereitstellung abgeschlossen ist. Um dies zu verhindern, können Sie eine *app\_offline.htm* Datei. Wenn Sie eine Datei namens einfügen *app\_offline.htm* im Stammordner der Anwendung, IIS automatisch diese Datei statt der Anwendungsstatus angezeigt. Damit während der Bereitstellung den Zugriff verhindern möchten, Sie platzieren *app\_offline.htm* im Stammordner, den Bereitstellungsvorgang ausführen, und entfernen Sie *app\_offline.htm*.

In **Projektmappen-Explorer**mit der rechten Maustaste auf die Projektmappe (nicht eines der Projekte), und wählen Sie **neuen Projektmappenordner**.

![Creating_a_solution_folder](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image10.png)

Benennen Sie den Ordner *SolutionFiles*.

In den neuen Ordner erstellen Sie eine HTML-Seite mit dem Namen *app\_offline.htm*. Ersetzen Sie den vorhandenen Inhalt durch Folgendes Markup:

[!code-html[Main](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/samples/sample2.html)]

Sie kopieren können die *app\_offline.htm* Datei mit dem Standort mithilfe von FTP-Verbindung oder die **Manager für Dateiserver** Hilfsprogramm in der Systemsteuerung des Hostinganbieters. Für dieses Lernprogramm verwenden Sie die **Manager für Dateiserver**.

Öffnen Sie die Systemsteuerung, und wählen Sie **Manager für Dateiserver** wie in der [in der Produktionsumgebung bereitstellen](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) Lernprogramm. Wählen Sie **contosouniversity.com** und dann **"Wwwroot"** zum Stammordner der Anwendung erhalten haben, und klicken Sie auf **hochladen**.

[![Upload_button_in_File_Manager](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image12.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image11.png)

In der **Datei hochladen** wählen Sie im Dialogfeld die *app\_offline.htm* Datei, und klicken Sie dann auf **hochladen**.

[![Upload_dialog_box_in_File_Manager](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image14.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image13.png)

Navigieren Sie zu Ihrer Website-URL. Sie sehen, dass die *app\_offline.htm* Seite wird jetzt anstatt Ihre Startseite angezeigt.

[![App_offline.htm_page_in_production](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image15.png)

Sie sind jetzt bereit für die Bereitstellung bis hin zur Produktion.

## <a name="deploying-the-code-update-to-the-production-environment"></a>Das Update Code bereitstellen in der Produktionsumgebung

In der **Web eine klicken Sie auf Publish** Symbolleiste, wählen Sie die **Produktion** Veröffentlichungsprofil, und klicken Sie dann auf **Web veröffentlichen**.

Visual Studio stellt die aktualisierte Anwendung bereit und öffnet den Browser, um die Homepage der Website. Die *app\_offline.htm* Datei wird angezeigt. Sie testen können, um die erfolgreiche Bereitstellung zu überprüfen, müssen Sie Sie entfernen die *app\_offline.htm* Datei.

Zurück zu den **Manager für Dateiserver** Anwendung in der Systemsteuerung. Wählen Sie **contosouniversity.com** und **"Wwwroot"** Option **app\_offline.htm**, und klicken Sie dann auf **löschen**.

[![Deleting_app_offline.htm](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image17.png)

Klicken Sie im Browser öffnen Sie die Lehrkräfte-Seite in der öffentlichen Website, und wählen Sie einen Kursleiter, um sicherzustellen, dass das Update erfolgreich bereitgestellt wurde.

[![Instructors_page_with_courses_Prod](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image19.png)

Sie haben jetzt ein Anwendungsupdate bereitgestellt, die keine Änderung an einer Datenbank umfassen. Die nächste Lernprogramm veranschaulicht die Änderung an einer Datenbank bereitstellen.

> [!div class="step-by-step"]
> [Zurück](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)
> [Weiter](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)
