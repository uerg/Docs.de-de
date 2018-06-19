---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
title: 'ASP.NET Web-Bereitstellung mit Visual Studio: Bereitstellen von einem Code-Update | Microsoft Docs'
author: tdykstra
description: Diese Reihe von Lernprogrammen wird gezeigt, wie bereitstellen (veröffentlichen) aus einer ASP.NET web-Anwendung auf Azure App Service-Web-Apps oder mit einem Hostinganbieter von Drittanbietern durch wählen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: c76dbc35-a914-4ee3-919c-4f4d1fa05104
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
msc.type: authoredcontent
ms.openlocfilehash: dd02b5c627fbfbb0034030f4c21207d24f6aabce
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30881333"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-code-update"></a>ASP.NET Web-Bereitstellung mit Visual Studio: Bereitstellen von einem Code-Update
====================
durch [Tom Dykstra](https://github.com/tdykstra)

[Startprojekt herunterladen](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Diese Reihe von Lernprogrammen wird gezeigt, wie bereitstellen (veröffentlichen) aus einer ASP.NET web-Anwendung eines Drittanbieters Hostinganbieter oder Azure App Service-Web-Apps mithilfe von Visual Studio 2012 oder Visual Studio 2010. Informationen über die Reihen finden Sie unter [im ersten Lernprogramm, in der Reihe](introduction.md).


## <a name="overview"></a>Übersicht

Nach der anfänglichen Bereitstellung können Ihre Arbeit verwalten und entwickeln Ihre Website fortgesetzt und lange wird eine Aktualisierung bereitstellen möchten. Dieses Lernprogramm führt Sie durch den Prozess der Bereitstellung von Aktualisierungen an den Anwendungscode. Das Update, das Sie implementieren und bereitstellen, die in diesem Lernprogramm beinhaltet keine Änderung an einer Datenbank; sehen Sie, ob Sie zum Bereitstellen der Änderung an einer Datenbank in den nächsten Lernprogrammen unterschiedlich ist.

Hinweis: Wenn Sie eine Fehlermeldung erhalten, oder etwas funktioniert nicht, wenn Sie das Lernprogramm absolvieren, müssen Sie überprüfen die [Website](troubleshooting.md).

## <a name="make-a-code-change"></a>Eine codeänderung

Als einfaches Beispiel eines Updates für Ihre Anwendung, fügen Sie auf der **Lehrkräfte** Seite eine Liste der ausgewählten Dozent unterrichtet Kurse.

Wenn das Ausführen der **Lehrkräfte** Seite werden Sie feststellen, dass es gibt **wählen** Links im Raster, aber nicht der Fall ist etwas anderes als stellen die Zeile Background grau dargestellt.

![Seite "Lehrkräfte" mit der Auswahl](deploying-a-code-update/_static/image1.png)

Nachdem Sie Code hinzufügen, die ausgeführt, wenn wird die **wählen** Link geklickt wird, und zeigt eine Liste der ausgewählten Dozent unterrichtet Kurse.

1. In *Instructors.aspx*, fügen Sie das folgende Markup unmittelbar nach der **ErrorMessageLabel** `Label` Steuerelement:

    [!code-aspx[Main](deploying-a-code-update/samples/sample1.aspx)]
2. Führen Sie die Seite, und wählen Sie einen Kursleiter. Sie sehen eine Liste der Kurse, Dozent unterrichtet.

    ![Trainer-Seite mit Kurse vermittelten](deploying-a-code-update/_static/image2.png)
3. Schließen Sie den Browser.

## <a name="deploy-the-code-update-to-the-test-environment"></a>Bereitstellen des Code-Updates in der testumgebung

Bevor Sie Ihre Veröffentlichungsprofile verwenden können, um auf Test-, Staging- und produktionsumgebung bereitzustellen, müssen Sie Optionen für die Veröffentlichung der Datenbank zu ändern. Sie müssen nicht mehr die Bereitstellungsskripts erteilen und die Daten für die Mitgliedschaftsdatenbank ausführen.

1. Öffnen der **Web veröffentlichen** Assistenten, indem Sie mit der rechten Maustaste des Projekts ContosoUniversity und auf **veröffentlichen**.
2. Klicken Sie auf die **Test** im Profil der **Profil** Dropdown-Liste.
3. Klicken Sie auf die **Einstellungen** Registerkarte.
4. Klicken Sie unter **DefaultConnection** in der **Datenbanken** deaktivieren Sie im Abschnitt der **Datenbank aktualisieren** Kontrollkästchen.
5. Klicken Sie auf die **Profil** Registerkarte, und klicken Sie dann auf die **Staging** im Profil der **Profil** Dropdown-Liste.
6. Wenn Sie gefragt werden, wenn Sie die vorgenommenen Änderungen speichern möchten die **Test** möchten, klicken Sie auf **Ja**.
7. Stellen Sie die gleiche Änderung in den Staging-Profil ein.
8. Wiederholen Sie den Vorgang, um die gleiche Änderung in der Produktion-Profil vornehmen.
9. Schließen der **Web veröffentlichen** Assistenten.

Bereitstellung in der testumgebung ist nun problemlos der Ausführung nur einem Klick erneut veröffentlichen. Um diesen Prozess zu beschleunigen, können Sie die **Web eine klicken Sie auf Publish** Symbolleiste.

1. In der **Ansicht** Menü Wählen Sie **Symbolleisten** und wählen Sie dann **Web eine klicken Sie auf Publish**.

    ![Selecting_One_Click_Publish_toolbar](deploying-a-code-update/_static/image3.png)
2. In **Projektmappen-Explorer**, wählen Sie das Projekt ContosoUniversity.
3. die **Web eine klicken Sie auf Publish** Symbolleiste, wählen Sie die **Test** Veröffentlichungsprofil, und klicken Sie dann auf **Web veröffentlichen** (das Symbol mit den Pfeilen nach links und rechts).

    ![Web_One_Click_Publish_toolbar](deploying-a-code-update/_static/image4.png)
4. Visual Studio wird die aktualisierte Anwendung bereitgestellt, und der Browser automatisch geöffnet wird, um zur Startseite.
5. Führen Sie die Seite Lehrkräfte, und wählen Sie einen Kursleiter, um sicherzustellen, dass das Update erfolgreich bereitgestellt wurde.

Sie würden normalerweise Gleiches Regressionstests (d. h. den Rest des Standorts, um sicherzustellen, dass die neue Änderung vorhandene Funktionalität unterbrechen nicht testen). Aber für dieses Lernprogramm überspringen Sie diesen Schritt und fahren Sie mit das Update auf Staging- und produktionsumgebung bereitstellen.

Wenn Sie erneut bereitstellen, Web Deploy automatisch bestimmt, welche Dateien geändert wurden, und nur Kopien Dateien auf dem Server geändert. Standardmäßig verwendet Web Deploy zuletzt geändert von Datumsangaben auf Dateien um zu bestimmen, welche geändert haben. Einige Quellcode-Verwaltungssysteme ändern Datei Datumsangaben auch, wenn Sie den Inhalt der Datei nicht ändern. In diesem Fall empfiehlt es sich so konfigurieren Sie Web Deploy, um die Dateiprüfsummen verwenden, um zu bestimmen, welche Dateien geändert wurden. Weitere Informationen finden Sie unter [Warum alle Dateien abrufen erneut bereitgestellt, obwohl ich sie geändert haben?](https://msdn.microsoft.com/library/ee942158.aspx#use_checksum) in der ASP.NET Bereitstellung – häufig gestellte Fragen.

## <a name="take-the-application-offline-during-deployment"></a>Nehmen Sie die Anwendung offline, während der Bereitstellung

Die Änderung, die Sie bereitstellen, ist jetzt eine einfache Änderung an einer einzelnen Seite. Aber manchmal Sie größere Änderungen bereitstellen oder Bereitstellen von Code und Datenbank ändert, und die Website möglicherweise falsch verhält, wenn ein Benutzer eine Seite anfordert, bevor die Bereitstellung abgeschlossen ist. Um zu verhindern, dass Benutzer auf den Standort zugreifen, während der Bereitstellung ausgeführt wird, können Sie eine *app\_offline.htm* Datei. Wenn Sie eine Datei namens einfügen *app\_offline.htm* im Stammordner der Anwendung, IIS automatisch diese Datei statt der Anwendungsstatus angezeigt. Damit während der Bereitstellung den Zugriff verhindern möchten, Sie platzieren *app\_offline.htm* im Stammordner, den Bereitstellungsvorgang ausführen, und entfernen Sie *app\_offline.htm* nach dem erfolgreichen Bereitstellung.

Sie können konfigurieren, Web Deploy, um automatisch einen Standardwert einzufügen *app\_offline.htm* Datei auf dem Server beim Start bereitstellen und sie nach dem Abschluss zu entfernen. Dazu, dass alle Sie lediglich müssen ist das folgende XML-Element die veröffentlichungsprofildatei (.pubxml) hinzufügen:

[!code-xml[Main](deploying-a-code-update/samples/sample2.xml)]

In diesem Lernprogramm erfahren Sie, wie zum Erstellen und verwenden ein benutzerdefinierten *app\_offline.htm* Datei.

Mit *app\_offline.htm* in die Stagingsite ist nicht erforderlich, da Sie nicht über Benutzer, die Zugriff auf die Stagingsite verfügen. Aber es wird empfohlen, staging verwenden, um alles, was die Möglichkeit zu testen, die Sie in der Produktion bereitstellen möchten.

### <a name="create-appofflinehtm"></a>Erstellen Sie die app\_offline.htm

1. In **Projektmappen-Explorer**mit der rechten Maustaste auf die Projektmappe, und klicken Sie auf **hinzufügen**, und klicken Sie dann auf **neues Element**.
2. Erstellen einer **HTML-Seite** mit dem Namen *app\_offline.htm* (löschen Sie die endgültige "l" in der *.html* -Erweiterung, die Visual Studio standardmäßig erstellt).
3. Ersetzen Sie das Vorlagenmarkup durch Folgendes Markup:

    [!code-html[Main](deploying-a-code-update/samples/sample3.html)]
4. Speichern und schließen Sie die Datei.

### <a name="copy-appofflinehtm-to-the-root-folder-of-the-web-site"></a>Kopie app\_offline.htm in den Stammordner der Website

Sie können alle FTP-Tool verwenden, zum Kopieren von Dateien mit der Website. [FileZilla](http://filezilla-project.org/) ist ein gängiges Tool für die FTP- und in die Screenshots angezeigt wird.

Um ein FTP-Tool verwenden zu können, benötigen Sie drei Dinge: der FTP-URL, den Benutzernamen und das Kennwort.

Die URL wird auf der Website-Dashboard-Seite im Azure-Verwaltungsportal angezeigt, und den Benutzernamen und das Kennwort für FTP finden Sie in der *publishsettings* -Datei, die Sie zuvor heruntergeladen haben. Die folgenden Schritte zeigen, wie Sie diese Werte abrufen.

1. Klicken Sie in der Azure-Verwaltungsportal auf **Websites** Registerkarte, und klicken Sie dann auf den staging-Website.
2. Auf der **Dashboard** Seite, scrollen Sie zu suchen, die der FTP-in Hostname der **Blick** Abschnitt.

    ![FTP-Hostname](deploying-a-code-update/_static/image5.png)
3. Öffnen Sie das Staging *publishsettings* Datei in Editor oder einem anderen Texteditor.
4. Suchen der `publishProfile` -Element für das FTP-Profil.
5. Kopieren der `userName` und `userPWD` Werte.

    ![FTP-Anmeldeinformationen](deploying-a-code-update/_static/image6.png)
6. Öffnen Sie Ihren FTP-Tool, und melden Sie sich bei der FTP-URL.
7. Kopie *app\_offline.htm* im Projektmappenordner, den */Site / "Wwwroot"* Ordner auf der staging-Website.

    ![Kopieren Sie app_offline](deploying-a-code-update/_static/image7.png)
8. Navigieren Sie zu Ihrer staging-Website-URL. Sie sehen, dass die *app\_offline.htm* Seite wird jetzt anstatt Ihre Startseite angezeigt.

    ![App_offline.htm im Browserfenster](deploying-a-code-update/_static/image8.png)

Sie können nun in die Stagingumgebung bereitstellen.

## <a name="deploy-the-code-update-to-staging-and-production"></a>Bereitstellen des Code-Updates für Staging und Produktion

1. In der **Web eine klicken Sie auf Publish** Symbolleiste, wählen Sie die **Staging** Veröffentlichungsprofil, und klicken Sie dann auf **Web veröffentlichen**.

    Visual Studio stellt die aktualisierte Anwendung bereit und öffnet den Browser, um die Homepage der Website. Die *app\_offline.htm* Datei wird angezeigt. Sie testen können, um die erfolgreiche Bereitstellung zu überprüfen, müssen Sie Sie entfernen die *app\_offline.htm* Datei.
2. In Ihrem FTP-Tool zurück, und löschen **app\_offline.htm** von der staging-Website.
3. Klicken Sie im Browser öffnen Sie die Seite Lehrkräfte in die Stagingsite, und wählen Sie einen Kursleiter, um sicherzustellen, dass das Update erfolgreich bereitgestellt wurde.
4. Halten Sie die gleiche Prozedur für die Produktion aus, wie Sie für die Stagingtabelle.

<a id="specificfiles"></a>

## <a name="reviewing-changes-and-deploying-specific-files"></a>Überprüfen der Änderungen und Bereitstellen von Dateien

Visual Studio 2012 bietet Ihnen außerdem die Möglichkeit, einzelne Dateien bereitzustellen. Für eine ausgewählte Datei können Sie Unterschiede zwischen der lokalen Version und der bereitgestellten Version anzeigen, die Datei in die zielumgebung bereitstellen oder kopieren Sie die Datei aus der zielumgebung zum lokalen Projekt. In diesem Abschnitt des Lernprogramms finden Sie unter Verwendung dieser Funktionen.

### <a name="make-a-change-to-deploy"></a>Nehmen Sie eine Änderung bereitstellen

1. Open *Content/Site.css*, und suchen Sie den Block für die `body` Tag.
2. Ändern Sie den Wert für `background-color` aus `#fff` auf `darkblue`.

    [!code-css[Main](deploying-a-code-update/samples/sample4.css?highlight=2)]

### <a name="view-the-change-in-the-publish-preview-window"></a>Die Änderung im Vorschaufenster veröffentlichen anzeigen

Bei Verwendung der **Web veröffentlichen** Assistenten zum Veröffentlichen des Projekts können Sie sehen, welche Schritte durchlaufen Änderungen veröffentlicht werden, durch Doppelklicken auf die Datei in die **Vorschau** Fenster.

1. Mit der rechten Maustaste des ContosoUniversity-Projekts, und klicken Sie auf **veröffentlichen**.
2. Aus der **Profil** Dropdown-Liste der **Test** Veröffentlichungsprofil.
3. Klicken Sie auf **Vorschau**, und klicken Sie dann auf **starten Preview**.
4. In der **Vorschau** Bereich doppelklicken Sie auf **"Site.CSS" ändern**.

    ![Doppelklicken Sie auf "Site.CSS" ändern](deploying-a-code-update/_static/image9.png)

    Die **Vorschau der Änderungen** Dialogfeld zeigt eine Vorschau der Änderungen, die bereitgestellt werden.

    ![Vorschau der Änderungen an "Site.CSS" ändern](deploying-a-code-update/_static/image10.png)

    Doppelklicken Sie auf die *"Web.config"* Datei, die **Vorschau der Änderungen** Dialogfeld zeigt die Auswirkung des Builds Konfiguration Transformationen und Transformationen Profil veröffentlichen. An diesem Punkt Sie nicht getan haben, die dazu führen würde, dass die *"Web.config"* Datei auf dem Server zu ändern, sodass Sie erwarten, dass keine Änderungen sehen,. Allerdings die **Vorschau der Änderungen** Fenster sind zwei Änderungen falsch dargestellt. Anscheinend zwei XML-Elemente entfernt werden sollen. Diese Elemente werden durch die Veröffentlichung hinzugefügt, bei der Auswahl **führen Sie Code First-Migrationen beim Anwendungsstart** für ein Code First Context-Klasse. Der Vergleich erfolgt, bevor der Veröffentlichungsprozess dieser Elemente hinzugefügt, damit sie scheinen sie entfernt werden, obwohl sie nicht entfernt werden. Dieser Fehler wird in einer zukünftigen Version behoben werden.
5. Klicken Sie auf **Schließen**.
6. Klicken Sie auf **Veröffentlichen**.
7. Wenn der Browser an die Startseite der der Test-Website geöffnet wird, drücken Sie STRG + F5, um eine feste Aktualisierung verursachen, um die Auswirkung der Änderung CSS finden Sie unter.

    ![Auswirkung der Änderung von CSS](deploying-a-code-update/_static/image11.png)
8. Schließen Sie den Browser.

### <a name="publish-specific-files-from-solution-explorer"></a>Bestimmte Dateien im Projektmappen-Explorer veröffentlichen

Angenommen Sie, Sie nicht wie die blauen Hintergrund und die ursprüngliche Farbe wiederherstellen möchten. In diesem Abschnitt müssen Sie die ursprünglichen Einstellungen wiederherstellen, indem Sie eine bestimmte Datei direkt aus veröffentlichen **Projektmappen-Explorer**.

1. Open *Content/Site.css* und Wiederherstellen der `background-color` auf `#fff`.
2. In **Projektmappen-Explorer**, mit der rechten Maustaste die *Content/Site.css* Datei.

    Das Kontextmenü enthält drei Optionen veröffentlichen.

    ![Optionen im Projektmappen-Explorer veröffentlichen](deploying-a-code-update/_static/image12.png)
3. Klicken Sie auf **Vorschau ändert sich in "Site.CSS" ändern**.

    Ein Fenster wird geöffnet, um die Unterschiede zwischen der lokalen Datei und die Version des Zertifikats in der zielumgebung anzeigen.

    ![Diff-Content/Site.css](deploying-a-code-update/_static/image13.png)
4. In **Projektmappen-Explorer**, mit der rechten Maustaste **"Site.CSS" ändern** erneut aus, und klicken Sie auf **veröffentlichen "Site.CSS" ändern**.

    Die **Webaktivität veröffentlichen** Fenster angezeigt wird, dass die Datei veröffentlicht wurde.

    ![Web veröffentlichen Aktivitätsfenster](deploying-a-code-update/_static/image14.png)
5. Öffnen Sie einen Browser, um die `http://localhost/contosouniversity` URL, und drücken Sie STRG + F5, um dazu führen, dass eine harte aktualisiert, damit die Auswirkungen von den CSS-Code ändern, finden Sie unter.

    ![Startseite mit normalen CSS](deploying-a-code-update/_static/image15.png)
6. Schließen Sie den Browser.

## <a name="summary"></a>Zusammenfassung

Sie haben nun gesehen, mehrere Möglichkeiten, ein Anwendungsupdate bereitstellen, die keine Änderung an einer Datenbank beinhaltet, und haben Sie gesehen, wie die Änderungen aus, um sicherzustellen, dass was aktualisiert werden, was Sie erwarten wird in der Vorschau anzeigen. Die Seite Lehrkräfte verfügt jetzt über eine **Kurse vermittelten** Abschnitt.

![Trainer-Seite mit Kurse vermittelten](deploying-a-code-update/_static/image16.png)

Im nächste Lernprogramm, in dem Sie erfahren, wie Sie eine Änderung einer bereitstellen: Fügen Sie ein Feld "BirthDate" in der Datenbank und auf der Seite "Lehrkräfte".

> [!div class="step-by-step"]
> [Zurück](deploying-to-production.md)
> [Weiter](deploying-a-database-update.md)
