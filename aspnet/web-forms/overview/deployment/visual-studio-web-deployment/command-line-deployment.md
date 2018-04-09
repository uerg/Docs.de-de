---
uid: web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
title: 'ASP.NET Web-Bereitstellung mit Visual Studio: Befehlszeile Bereitstellung | Microsoft Docs'
author: tdykstra
description: Diese Reihe von Lernprogrammen wird gezeigt, wie bereitstellen (veröffentlichen) aus einer ASP.NET web-Anwendung auf Azure App Service-Web-Apps oder mit einem Hostinganbieter von Drittanbietern durch wählen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 82b8dea0-f062-4ee4-8784-3ffa30fbb1ca
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
msc.type: authoredcontent
ms.openlocfilehash: acc4a0e7f4744a3759b90e0f1b159da68b7c7362
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-web-deployment-using-visual-studio-command-line-deployment"></a>ASP.NET Web-Bereitstellung mit Visual Studio: Bereitstellung über die Befehlszeile
====================
durch [Tom Dykstra](https://github.com/tdykstra)

[Startprojekt herunterladen](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Diese Reihe von Lernprogrammen wird gezeigt, wie bereitstellen (veröffentlichen) aus einer ASP.NET web-Anwendung eines Drittanbieters Hostinganbieter oder Azure App Service-Web-Apps mithilfe von Visual Studio 2012 oder Visual Studio 2010. Informationen über die Reihen finden Sie unter [im ersten Lernprogramm, in der Reihe](introduction.md).


## <a name="overview"></a>Übersicht

Dieses Lernprogramm veranschaulicht das Aufrufen der Visual Studio Web veröffentlichen Pipeline über die Befehlszeile. Dies ist hilfreich, wenn Sie möchten [automatisieren Sie den Bereitstellungsprozess](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) anstatt dies manuell in Visual Studio, in der Regel durch die Verwendung einer [source Code Versionskontrollsystem](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md).

## <a name="make-a-change-to-deploy"></a>Nehmen Sie eine Änderung bereitstellen

Derzeit zeigt die Seite "Info" im Vorlagencode.

![Zu den Seiten mit Vorlagencode](command-line-deployment/_static/image1.png)

Sie werden, die durch Code ersetzen, die eine Zusammenfassung der Student-Anmeldung angezeigt.

Öffnen der *About.aspx* Seite, löschen Sie alle Markups in der `MainContent` `Content` -Element, und legen Sie das folgende Markup an seiner Stelle:

[!code-aspx[Main](command-line-deployment/samples/sample1.aspx)]

Führen Sie das Projekt, und wählen Sie die **zu** Seite.

![Seite „Info“](command-line-deployment/_static/image2.png)

## <a name="deploy-to-test-by-using-the-command-line"></a>Bereitstellen Sie in Test mithilfe der Befehlszeile

Sie wird nicht einer anderen Datenbank ändern, daher deaktivieren DbDacFx-datenbankbereitstellung für das Aspnet-ContosoUniversity Datenbank bereitstellen. Öffnen der **Web veröffentlichen** Assistenten, und in jeder der drei Veröffentlichungsprofile, Deaktivieren der **Update Database** Kontrollkästchen auf der **Einstellungen** Registerkarte.

Suchen Sie in der Windows 8-Startbildschirm **Developer-Eingabeaufforderung für VS2012**.

Mit der rechten Maustaste des Symbol für **Developer-Eingabeaufforderung für VS2012** , und klicken Sie auf **als Administrator ausführen**.

Geben Sie an der Befehlszeile aus, und Ersetzen Sie dabei den Pfad zur Projektmappendatei mit dem Pfad zu Ihrer Projektmappendatei den folgenden Befehl aus:

[!code-console[Main](command-line-deployment/samples/sample2.cmd)]

MSBuild erstellt die Lösung und stellt sie in der testumgebung bereit.

![Befehlszeilenausgabe](command-line-deployment/_static/image3.png)

Öffnen Sie einen Browser, und wechseln Sie zu `http://localhost/ContosoUniversity`, klicken Sie dann auf die **zu** Seite, um sicherzustellen, dass die Bereitstellung erfolgreich war.

Wenn Sie alle Studenten im Test erstellt haben, sehen Sie eine leere Seite unter das **Student Text Statistiken** Überschrift. Wechseln Sie zu der **Studenten** auf **Student hinzufügen**, und einige Studenten hinzufügen, und fahren Sie dann mit der **zu** Seite Student-Statistik anzuzeigen.

![Zu den Seiten in der testumgebung](command-line-deployment/_static/image4.png)

## <a name="key-command-line-options"></a>Key-Befehlszeilenoptionen

Der Befehl, den eingegebene Pfad der Projektmappe und zwei Eigenschaften an MSBuild übergeben:

[!code-console[Main](command-line-deployment/samples/sample3.cmd)]

### <a name="deploying-the-solution-versus-deploying-individual-projects"></a>Bereitstellen der Lösung im Vergleich zur Bereitstellung von einzelnen Projekten

Die Projektmappendatei Angabe bewirkt, dass alle Projekte in der Projektmappe erstellt werden sollen. Wenn Sie mehrere Webprojekte in der Projektmappe haben, gilt MSBuild Folgendes ein:

- Die Eigenschaften, die Sie in der Befehlszeile angeben, werden auf jedes Projekt übergeben. Daher muss jedes Webprojekt ein Veröffentlichungsprofil mit dem Namen verfügen, die Sie angeben. Bei Angabe von `/p:PublishProfile=Test`, jedes Webprojekt benötigen ein Veröffentlichungsprofil mit dem Namen *Test*.
- Sie können ein Projekt erfolgreich veröffentlichen, wenn eine andere nicht selbst erstellt werden kann. Weitere Informationen finden Sie im Stackoverflow-Thread [MSBuild schlägt fehl mit zwei Pakete](http://stackoverflow.com/questions/14226451/msbuild-fails-with-two-packages).

Wenn Sie ein einzelnes Projekt anstelle einer Projektmappe angeben, müssen Sie einen Parameter hinzufügen, der das Visual Studio-Version angibt. Bei Verwendung von Visual Studio 2012 wäre die Befehlszeile im folgenden Beispiel ähnelt:

[!code-console[Main](command-line-deployment/samples/sample4.cmd?highlight=1)]

Die Versionsnummer für Visual Studio 2010 ist 10,0. Weitere Informationen finden Sie unter [Visual Studio-Projekt zur Anwendungskompatibilität und VisualStudioVersion](http://sedodream.com/2012/08/19/VisualStudioProjectCompatabilityAndVisualStudioVersion.aspx) Sayed Hashimi-Blog.

### <a name="specifying-the-publish-profile"></a>Das Veröffentlichungsprofil angeben

Sie können das Veröffentlichungsprofil angeben, nach Namen oder den vollständigen Pfad zu der *pubxml* Datei, wie im folgenden Beispiel gezeigt:

[!code-console[Main](command-line-deployment/samples/sample5.cmd?highlight=1)]

### <a name="web-publish-methods-supported-for-command-line-publishing"></a>Web veröffentlichen für Befehlszeilen Veröffentlichung unterstützte Methoden

Veröffentlichen von drei Methoden werden für die Veröffentlichung über die Befehlszeile unterstützt:

- `MSDeploy` -Mithilfe von Web Deploy veröffentlichen.
- `Package` -Veröffentlichen Sie, indem Sie ein Web Deploy-Paket erstellen. Sie müssen das Paket getrennt von der MSBuild-Befehl installieren, die es erstellt.
- `FileSystem` -Durch Kopieren von Dateien in einem angegebenen Ordner veröffentlichen.

### <a name="specifying-the-build-configuration-and-platform"></a>Angeben der Build-Konfiguration und Plattform

Die Buildkonfiguration und die Plattform müssen in Visual Studio oder in der Befehlszeile festgelegt werden. Veröffentlichungsprofile enthalten Eigenschaften, die mit dem Namen sind `LastUsedBuildConfiguration` und `LastUsedPlatform`, aber diese Eigenschaften können nicht festgelegt werden, um zu bestimmen, wie das Projekt erstellt wird. Weitere Informationen finden Sie unter [MSBuild: Gewusst wie: festlegen die Konfigurationseigenschaft](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) Sayed Hashimi-Blog.

## <a name="deploy-to-staging"></a>In die Stagingumgebung bereitstellen

Um in Azure bereitstellen, müssen Sie das Kennwort in der Befehlszeile hinzufügen. Wenn Sie das Kennwort in das Veröffentlichungsprofil in Visual Studio gespeichert, es wurde in verschlüsselter Form gespeichert in der Ihre *. pubxml.user* Datei. Diese Datei wird von MSBuild nicht zugegriffen werden, wenn Sie eine Bereitstellung über die Befehlszeile ausführen, müssen Sie das Kennwort in einem Befehlszeilenparameter übergeben.

1. Kopieren Sie das Kennwort, die Sie benötigen die *publishsettings* die heruntergeladene Datei zuvor für das staging-Website. Das Kennwort ist der Wert der `userPWD` -Attribut für das Web Deploy `publishProfile` Element.

    ![Web Deploy-Kennwort](command-line-deployment/_static/image5.png)
2. Suchen Sie in der Windows 8-Startbildschirm **Developer-Eingabeaufforderung für VS2012**, und klicken Sie auf das Symbol, um das Eingabeaufforderungsfenster zu öffnen. (Sie müssen als Administrator dieses Mal öffnen, da Sie für IIS auf dem lokalen Computer bereitstellen werden nicht.)
3. Geben Sie den folgenden Befehl an der Eingabeaufforderung, und Ersetzen Sie dabei den Pfad zur Projektmappendatei mit dem Pfad in der Projektmappendatei und das Kennwort durch Ihr Kennwort ein:

    [!code-console[Main](command-line-deployment/samples/sample6.cmd)]

    Beachten Sie, dass dieser über die Befehlszeile einen zusätzlichen Parameter enthält: `/p:AllowUntrustedCertificate=true`. Wie in diesem Lernprogramm geschrieben wird, die `AllowUntrustedCertificate` Eigenschaft muss festgelegt werden, wenn Sie über die Befehlszeile in Azure veröffentlichen. Wenn diese Fehlerkorrektur veröffentlicht wird, brauchen Sie keine Parameter.
4. Öffnen Sie einen Browser, und wechseln Sie zu der URL der staging-Website und klicken Sie dann auf die **zu** Seite, um sicherzustellen, dass die Bereitstellung erfolgreich war.

    Wie Sie zuvor für die testumgebung gesehen haben, müssen Sie möglicherweise einige Studenten Statistiken für erhalten Sie erstellen die **zu** Seite.

## <a name="deploy-to-production"></a>Für die Produktion bereitstellen

Der Prozess für die Bereitstellung bis hin zur Produktion ähnelt der Prozess für das Staging.

1. Kopieren Sie das Kennwort, die Sie benötigen die *publishsettings* die heruntergeladene Datei zuvor für die Produktion-Website.
2. Open **Developer-Eingabeaufforderung für VS2012**.
3. Geben Sie den folgenden Befehl an der Eingabeaufforderung, und Ersetzen Sie dabei den Pfad zur Projektmappendatei mit dem Pfad in der Projektmappendatei und das Kennwort durch Ihr Kennwort ein:

    [!code-console[Main](command-line-deployment/samples/sample7.cmd)]

    Für einen Standort Produktion gäbe auch Änderung an einer Datenbank, in der Regel kopieren Sie die *app\_offline.htm* Datei wird in der Website vor der Bereitstellung und nach der erfolgreichen Bereitstellung zu löschen.
4. Öffnen Sie einen Browser, und wechseln Sie zu der URL der staging-Website und klicken Sie dann auf die **zu** Seite, um sicherzustellen, dass die Bereitstellung erfolgreich war.

## <a name="summary"></a>Zusammenfassung

Sie haben jetzt ein Anwendungsupdate bereitgestellt, über die Befehlszeile.

![Zu den Seiten in der testumgebung](command-line-deployment/_static/image6.png)

In den nächsten Lernprogrammen sehen Sie ein Beispiel zum Erweitern Sie im Web veröffentlichen Pipeline. Im Beispiel wird gezeigt, wie Dateien bereitstellen, die nicht im Projekt enthalten sind.

> [!div class="step-by-step"]
> [Zurück](deploying-a-database-update.md)
> [Weiter](deploying-extra-files.md)
