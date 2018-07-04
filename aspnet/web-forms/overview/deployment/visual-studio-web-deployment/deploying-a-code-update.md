---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
title: 'ASP.NET-webbereitstellung mithilfe von Visual Studio: Bereitstellen eines Codeupdates | Microsoft-Dokumentation'
author: tdykstra
description: Dieser tutorialreihe erfahren Sie, wie bereitzustellende (veröffentlichen) aus einer ASP.NET web-Anwendung auf Azure App Service-Web-Apps oder bei einem Hostinganbieter von Drittanbietern, indem Warnungsprovider...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: c76dbc35-a914-4ee3-919c-4f4d1fa05104
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
msc.type: authoredcontent
ms.openlocfilehash: 3649f0250e830b76c42d832e51038c2c52ed4561
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368213"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-code-update"></a>ASP.NET-webbereitstellung mithilfe von Visual Studio: Bereitstellen eines Codeupdates
====================
durch [Tom Dykstra](https://github.com/tdykstra)

[Startprojekt herunterladen](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Dieser tutorialreihe erfahren Sie, wie bereitzustellende (veröffentlichen) aus einer ASP.NET web-Anwendung auf Azure App Service-Web-Apps oder bei einem Hostinganbieter von Drittanbietern, mithilfe von Visual Studio 2012 oder Visual Studio 2010. Weitere Informationen über die Reihe finden Sie unter [im ersten Tutorial der Reihe](introduction.md).


## <a name="overview"></a>Übersicht

Nach der erstbereitstellung Ihre Arbeit verwalten und entwickeln Ihre Website wird fortgesetzt, und vor dem Long-Wert wird ein Update bereitstellen möchten. Dieses Tutorial führt Sie durch den Prozess der Bereitstellung eines Updates an Ihrem Anwendungscode. Das Update, das Implementieren und bereitstellen, die in diesem Tutorial muss keine Änderung an einer Datenbank werden; sehen Sie, ob Sie zum Bereitstellen der Änderung an einer Datenbank im nächsten Tutorial unterschiedlich ist.

Erinnerung: Wenn Sie eine Fehlermeldung erhalten, oder etwas nicht funktioniert, wie Sie das Lernprogramm durchzuarbeiten, sollten Sie unbedingt die [Problembehandlungsseite](troubleshooting.md).

## <a name="make-a-code-change"></a>Einer codeänderung

Fügen Sie ein einfaches Beispiel eines Updates für Ihre Anwendung, um die **Dozenten** Seite eine Liste der Kurse, die von den ausgewählten Dozenten unterrichtet.

Wenn das Ausführen der **Dozenten** Seite werden Sie feststellen, dass es gibt **wählen** Verknüpfungen im Raster, aber nicht der Fall ist etwas anderes als stellen die Zeile Background grau dargestellt.

!["Dozenten" mit Auswahl](deploying-a-code-update/_static/image1.png)

Nachdem Sie Code hinzufügen, die ausgeführt, wenn wird die **wählen** Link geklickt wird, und zeigt eine Liste der Kurse, die von den ausgewählten Dozenten unterrichtet.

1. In *Instructors.aspx*, fügen Sie das folgende Markup direkt nach der **ErrorMessageLabel** `Label` Steuerelement:

    [!code-aspx[Main](deploying-a-code-update/samples/sample1.aspx)]
2. Führen Sie die Seite, und wählen Sie einen Dozenten. Sie sehen eine Liste der Kurse, die durch dieses Dozenten unterrichtet.

    ![Dozentenseite mit Schulungskurse](deploying-a-code-update/_static/image2.png)
3. Schließen Sie den Browser.

## <a name="deploy-the-code-update-to-the-test-environment"></a>Bereitstellen des Code-Updates für die testumgebung

Bevor Sie Ihre Veröffentlichungsprofile verwenden können, für Test-, Staging- und produktionsumgebungen bereitstellen, müssen Sie Optionen für die Veröffentlichung der Datenbank zu ändern. Sie müssen nicht mehr die Bereitstellungsskripts erteilen und die Daten für die Mitgliedschaftsdatenbank ausführen.

1. Öffnen der **Webveröffentlichung** Assistenten, indem Sie mit der rechten Maustaste in des Projekts ContosoUniversity und auf **veröffentlichen**.
2. Klicken Sie auf die **Test** Profil in der **Profil** Dropdown-Liste.
3. Klicken Sie auf die **Einstellungen** Registerkarte.
4. Klicken Sie unter **DefaultConnection** in die **Datenbanken** deaktivieren Sie im Abschnitt der **Aktualisieren einer Datenbank** Kontrollkästchen.
5. Klicken Sie auf die **Profil** Registerkarte, und klicken Sie dann auf die **Staging** Profil in der **Profil** Dropdown-Liste.
6. Wenn Sie gefragt werden, wenn Sie die Änderungen speichern möchten die **Test** möchten, klicken Sie auf **Ja**.
7. Stellen Sie die gleiche Änderung in der Staging-Profil ein.
8. Wiederholen Sie den Vorgang, um die gleiche Änderung im produktionsprofil vornehmen.
9. Schließen der **Webveröffentlichung** Assistenten.

Bereitstellung in der testumgebung ist nun einfach der Ausführung von nur einem Klick erneut veröffentlichen. Um diesen Prozess zu beschleunigen, können Sie die **klicken Sie auf Webveröffentlichung mit einem** Symbolleiste.

1. In der **Ansicht** Menü wählen **Symbolleisten** und wählen Sie dann **klicken Sie auf Webveröffentlichung mit einem**.

    ![Selecting_One_Click_Publish_toolbar](deploying-a-code-update/_static/image3.png)
2. In **Projektmappen-Explorer**, wählen Sie das Projekt ContosoUniversity.
3. die **klicken Sie auf Webveröffentlichung mit einem** Symbolleiste wählen Sie die **Test** Veröffentlichungsprofil aus, und klicken Sie dann auf **Webveröffentlichung** (das Symbol mit den Pfeilen nach links und rechts).

    ![Web_One_Click_Publish_toolbar](deploying-a-code-update/_static/image4.png)
4. Visual Studio stellt die aktualisierte Anwendung bereit, und der Browser automatisch geöffnet wird, auf der Startseite.
5. Führen Sie die dozentenseite, und wählen Sie einen Dozenten aus, um sicherzustellen, dass das Update wurde erfolgreich bereitgestellt wurde.

Sie würden normalerweise auch tun Regressionstests (d. h. den Rest des Standorts, um sicherzustellen, dass die neue Änderung keine vorhandene Funktionalität nicht beeinträchtigt haben testen). Aber für dieses Tutorial überspringen Sie diesen Schritt und fahren Sie mit der Bereitstellung des Updates zu Staging und Produktion.

Wenn Sie erneut bereitstellen, Web Deploy bestimmt automatisch, welche Dateien geändert wurden, und nur Kopien geänderte Dateien auf dem Server. Standardmäßig verwendet Web Deploy zuletzt geänderten Datumsangaben für Dateien um zu bestimmen, welche geändert haben. Einige Quellcodeverwaltungssysteme Ändern der Datei Datumsangaben sogar wenn Sie den Inhalt der Datei nicht ändern. In diesem Fall empfiehlt es sich so konfigurieren Sie Web Deploy, um die Dateiprüfsummen zu verwenden, um zu bestimmen, welche Dateien geändert wurden. Weitere Informationen finden Sie unter [Warum alle meine Dateien erneut bereitgestellt, obwohl ich nicht ändern?](https://msdn.microsoft.com/library/ee942158.aspx#use_checksum) in die ASP.NET Bereitstellung – häufig gestellte Fragen.

## <a name="take-the-application-offline-during-deployment"></a>Nutzen Sie die Anwendung offline schalten, während der Bereitstellung

Die Änderung, die Sie bereitstellen, ist jetzt eine einfache Änderung an einer einzelnen Seite. Aber manchmal Sie größere Änderungen bereitstellen oder Bereitstellen von Anwendungscode und datenbankänderungen, und die Website möglicherweise fehlerhaften Verhalten, wenn ein Benutzer eine Seite anfordert, bevor die Bereitstellung abgeschlossen ist. Um zu verhindern, dass Benutzer auf den Standort zugreifen, während der Bereitstellung ausgeführt wird, können Sie eine *app\_offline.htm* Datei. Wenn Sie eine Datei namens einfügen *app\_offline.htm* im Stammordner Ihrer Anwendung, IIS automatisch die Datei statt der Ausführung Ihrer Anwendung angezeigt. Damit um den Zugriff während der Bereitstellung zu verhindern, Sie fügen *app\_offline.htm* im Stammordner, den Bereitstellungsvorgang ausführen, und entfernen Sie *app\_offline.htm* nach dem erfolgreichen die Bereitstellung.

Sie können konfigurieren, Web Deploy, um automatisch einen Standardwert versetzen *app\_offline.htm* Datei auf dem Server beim Start bereitstellen und entfernen Sie sie an, wenn er abgeschlossen ist. Zu diesem Zweck müssen Sie lediglich ist Ihre veröffentlichungsprofildatei (.pubxml) das folgende XML-Element hinzugefügt:

[!code-xml[Main](deploying-a-code-update/samples/sample2.xml)]

In diesem Tutorial erfahren Sie zum Erstellen und verwenden ein benutzerdefinierten *app\_offline.htm* Datei.

Mithilfe von *app\_offline.htm* auf der Stagingsite ist nicht erforderlich, da Sie nicht über Benutzer, die Zugriff auf die Stagingsite verfügen. Aber es wird empfohlen, Sie verwenden Test die Möglichkeit, die Sie in der Produktion bereitstellen möchten.

### <a name="create-appofflinehtm"></a>Erstellen Sie app\_offline.htm

1. In **Projektmappen-Explorer**mit der rechten Maustaste auf die Projektmappe, und klicken Sie auf **hinzufügen**, und klicken Sie dann auf **neues Element**.
2. Erstellen Sie eine **HTML-Seite** mit dem Namen *app\_offline.htm* (löschen Sie die endgültige "l" in der *.html* -Erweiterung, die Visual Studio wird standardmäßig erstellt).
3. Ersetzen Sie das Vorlagenmarkup, durch das folgende Markup:

    [!code-html[Main](deploying-a-code-update/samples/sample3.html)]
4. Speichern und schließen Sie die Datei.

### <a name="copy-appofflinehtm-to-the-root-folder-of-the-web-site"></a>App kopieren\_offline.htm in den Stammordner der Website

Sie können alle FTP-Tool verwenden, zum Kopieren von Dateien mit der Website. [FileZilla](http://filezilla-project.org/) ist ein beliebtes Tool für die FTP und wird in den Screenshots gezeigt.

Um ein FTP-Tool verwenden zu können, benötigen Sie drei Dinge: der FTP-URL, den Benutzernamen und das Kennwort.

Die URL wird auf der Website-Dashboard-Seite im Azure-Verwaltungsportal angezeigt, und den Benutzernamen und das Kennwort für FTP finden Sie in der *.publishsettings* -Datei, die Sie zuvor heruntergeladen haben. Die folgenden Schritte zeigen, wie Sie diese Werte zu erhalten.

1. Klicken Sie in der Azure-Verwaltungsportal auf **Websites** Registerkarte, und klicken Sie dann auf die staging-Website.
2. Auf der **Dashboard** Seite Scrollen Sie zu suchen, die der FTP-Hostname angegeben wird, in der **Blick** Abschnitt.

    ![FTP-Hostname](deploying-a-code-update/_static/image5.png)
3. Öffnen Sie das Staging *.publishsettings* Datei in Notepad oder einem anderen Texteditor.
4. Suchen der `publishProfile` -Element für das FTP-Profil.
5. Kopieren der `userName` und `userPWD` Werte.

    ![FTP-Anmeldeinformationen](deploying-a-code-update/_static/image6.png)
6. Öffnen Sie Ihren FTP-Tool, und melden Sie sich bei der FTP-URL.
7. Kopie *app\_offline.htm* im Projektmappenordner, den *"/ Site/wwwroot"* Ordner auf der staging-Website.

    ![Kopieren Sie "App_offline"](deploying-a-code-update/_static/image7.png)
8. Navigieren Sie zu Ihrer staging-Website-URL. Sie sehen, dass die *app\_offline.htm* Seite wird nun anstelle Ihrer Startseite angezeigt.

    ![App_offline.htm im Browserfenster](deploying-a-code-update/_static/image8.png)

Sie können nun in der Stagingumgebung bereitgestellt.

## <a name="deploy-the-code-update-to-staging-and-production"></a>Bereitstellen des Code-Updates für Staging und Produktion

1. In der **klicken Sie auf Webveröffentlichung mit einem** Symbolleiste wählen Sie die **Staging** Veröffentlichungsprofil aus, und klicken Sie dann auf **Webveröffentlichung**.

    Visual Studio stellt die aktualisierte Anwendung bereit und öffnet der Browser zur Startseite der Website. Die *app\_offline.htm* Datei wird angezeigt. Bevor Sie zum Überprüfen der erfolgreichen Bereitstellung testen können, müssen Sie entfernen die *app\_offline.htm* Datei.
2. Zurück zu Ihrem FTP-Tool, und Löschen von **app\_offline.htm** von der staging-Website.
3. Klicken Sie im Browser öffnen Sie die dozentenseite auf der Stagingsite, und wählen Sie einen Dozenten aus, um sicherzustellen, dass das Update wurde erfolgreich bereitgestellt wurde.
4. Führen Sie das gleiche Verfahren für die Produktion, ebenso wie für staging.

<a id="specificfiles"></a>

## <a name="reviewing-changes-and-deploying-specific-files"></a>Überprüfen von Änderungen und Bereitstellen von Dateien

Visual Studio 2012 gibt Ihnen außerdem die Möglichkeit, einzelne Dateien bereitstellen. Für eine ausgewählte Datei können Sie Unterschiede zwischen der lokalen Version und die bereitgestellte Version anzeigen, die Datei in die zielumgebung bereitstellen oder kopieren Sie die Datei von der zielumgebung für das lokale Projekt. In diesem Abschnitt des Tutorials erfahren Sie, wie diese Funktionen verwendet.

### <a name="make-a-change-to-deploy"></a>Nehmen Sie eine Änderung bereitstellen

1. Open *Content/Site.css*, und suchen Sie den Block für die `body` Tag.
2. Ändern Sie den Wert für `background-color` aus `#fff` zu `darkblue`.

    [!code-css[Main](deploying-a-code-update/samples/sample4.css?highlight=2)]

### <a name="view-the-change-in-the-publish-preview-window"></a>Überprüfen Sie die Änderung im Fenster Vorschau veröffentlichen

Bei Verwendung der **Web veröffentlichen** Assistenten zum Veröffentlichen des Projekts, sehen Sie, welche Änderungen ausgeführt werden soll veröffentlicht werden, durch Doppelklicken auf die Datei in die **Vorschau** Fenster.

1. Mit der rechten Maustaste in des ContosoUniversity-Projekts, und klicken Sie auf **veröffentlichen**.
2. Von der **Profil** Dropdown-Liste die **Test** Veröffentlichungsprofil.
3. Klicken Sie auf **Vorschau**, und klicken Sie dann auf **Vorschau starten**.
4. In der **Vorschau** Bereich doppelklicken Sie auf **"Site.CSS"**.

    ![Doppelklicken Sie auf "Site.CSS"](deploying-a-code-update/_static/image9.png)

    Die **Vorschau der Änderungen** Dialogfeld zeigt eine Vorschau der Änderungen, die bereitgestellt werden.

    ![Vorschau der Änderungen auf "Site.CSS"](deploying-a-code-update/_static/image10.png)

    Wenn Sie doppelklicken Sie auf die *"Web.config"* -Datei, die **Vorschau der Änderungen** Dialogfeld zeigt die Auswirkungen des Builds Konfiguration von Transformationen, und veröffentlichen Sie die Profil-Transformationen. An diesem Punkt haben dies nicht etwas, das würde dazu führen, dass die *"Web.config"* Datei auf dem Server zu ändern, sodass Sie erwarten, dass keine Änderungen sehen,. Allerdings die **Vorschau der Änderungen** Fenster zeigt fälschlicherweise zwei Änderungen. Offenbar zwei XML-Elemente werden entfernt. Diese Elemente werden durch die Veröffentlichung hinzugefügt, bei der Auswahl **Code First-Migrationen ausführen beim Start der Anwendung** für ein Code First Context-Klasse. Der Vergleich erfolgt, bevor der Veröffentlichungsprozess diese Elemente hinzugefügt, sodass sie aussieht, wie sie entfernt werden, obwohl sie nicht entfernt werden. Dieser Fehler wird in einer zukünftigen Version behoben werden.
5. Klicken Sie auf **Schließen**.
6. Klicken Sie auf **Veröffentlichen**.
7. Wenn der Browser zur Startseite der Test-Website geöffnet wird, drücken Sie STRG + F5, um eine Aktualisierung schwierig zu bewirken, um die Auswirkungen der Änderung CSS finden Sie unter.

    ![Auswirkung der CSS-Änderung](deploying-a-code-update/_static/image11.png)
8. Schließen Sie den Browser.

### <a name="publish-specific-files-from-solution-explorer"></a>Veröffentlichen Sie bestimmte Dateien im Projektmappen-Explorer

Angenommen Sie, Sie nicht wie die blauen Hintergrund und die ursprüngliche Farbe wiederherstellen möchten. In diesem Abschnitt müssen Sie die ursprünglichen Einstellungen wiederherstellen, indem Sie eine bestimmte Datei direkt aus veröffentlichen **Projektmappen-Explorer**.

1. Open *Content/Site.css* und Wiederherstellen der `background-color` auf `#fff`.
2. In **Projektmappen-Explorer**, mit der rechten Maustaste die *Content/Site.css* Datei.

    Das Kontextmenü enthält, dass drei Optionen veröffentlichen.

    ![Optionen im Projektmappen-Explorer veröffentlichen](deploying-a-code-update/_static/image12.png)
3. Klicken Sie auf **Vorschau der Änderungen auf "Site.CSS"**.

    Ein Fenster wird geöffnet, um die Unterschiede zwischen der lokalen Datei und die Version in der zielumgebung anzuzeigen.

    ![Diff-Content / "Site.CSS"](deploying-a-code-update/_static/image13.png)
4. In **Projektmappen-Explorer**, mit der rechten Maustaste **"Site.CSS"** erneut aus, und klicken Sie auf **veröffentlichen "Site.CSS"**.

    Die **Webveröffentlichungsaktivität** zeigt an, dass die Datei veröffentlicht wurde.

    ![Fenster "Web-Aktivität veröffentlichen"](deploying-a-code-update/_static/image14.png)
5. Öffnen Sie einen Browser auf die `http://localhost/contosouniversity` URL, und drücken Sie dann STRG + F5, um einen hartcodierten dazu führen, dass aktualisieren, um die Auswirkungen des CSS, ändern, finden Sie unter.

    ![Auf der Startseite mit normalen CSS](deploying-a-code-update/_static/image15.png)
6. Schließen Sie den Browser.

## <a name="summary"></a>Zusammenfassung

Sie haben nun gesehen, mehrere Möglichkeiten, ein Anwendungsupdate bereitstellen, die keine Änderung an einer Datenbank verbunden ist, und Sie haben gesehen, wie Sie eine Vorschau der Änderungen, um sicherzustellen, dass was aktualisiert werden, was Sie erwarten. Die dozentenseite verfügt jetzt über eine **Kurse unterrichtet** Abschnitt.

![Dozentenseite mit Schulungskurse](deploying-a-code-update/_static/image16.png)

Im nächste Tutorial erfahren Sie, wie zum Bereitstellen der Änderung an einer Datenbank: Fügen Sie ein Feld "BirthDate" in der Datenbank und zur dozentenseite.

> [!div class="step-by-step"]
> [Zurück](deploying-to-production.md)
> [Weiter](deploying-a-database-update.md)
