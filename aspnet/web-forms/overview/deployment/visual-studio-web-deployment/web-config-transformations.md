---
uid: web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations
title: 'ASP.NET-webbereitstellung mithilfe von Visual Studio: Umwandlungen für die Datei "Web.config" | Microsoft-Dokumentation'
author: tdykstra
description: Dieser tutorialreihe erfahren Sie, wie bereitzustellende (veröffentlichen) aus einer ASP.NET web-Anwendung auf Azure App Service-Web-Apps oder bei einem Hostinganbieter von Drittanbietern, indem Warnungsprovider...
ms.author: aspnetcontent
ms.date: 02/15/2013
ms.assetid: 5a2a927b-14cb-40bc-867a-f0680f9febd7
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations
msc.type: authoredcontent
ms.openlocfilehash: 0a52530396efa3612d19817eeaa0498cffdac38f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37842438"
---
<a name="aspnet-web-deployment-using-visual-studio-webconfig-file-transformations"></a>ASP.NET-webbereitstellung mithilfe von Visual Studio: Umwandlungen für die Datei "Web.config"
====================
durch [Tom Dykstra](https://github.com/tdykstra)

[Startprojekt herunterladen](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Dieser tutorialreihe erfahren Sie, wie bereitzustellende (veröffentlichen) aus einer ASP.NET web-Anwendung auf Azure App Service-Web-Apps oder bei einem Hostinganbieter von Drittanbietern, mithilfe von Visual Studio 2012 oder Visual Studio 2010. Weitere Informationen über die Reihe finden Sie unter [im ersten Tutorial der Reihe](introduction.md).


## <a name="overview"></a>Übersicht

In diesem Tutorial erfahren Sie, wie Sie den Prozess des Umschaltens automatisieren die *"Web.config"* -Datei, wenn Sie unterschiedliche zielumgebungen bereitstellen. Die meisten Anwendungen verfügen über Einstellungen der *"Web.config"* -Datei, die muss unterschiedlich sein, wenn die Anwendung bereitgestellt wird. Automatisierung des Prozesses, die diese Änderungen verhindert, dass sie manuell durchgeführt werden jedes Mal, wenn Sie bereitgestellt haben, die aufwendig sein würde und fehleranfällig.

Erinnerung: Wenn Sie eine Fehlermeldung erhalten, oder etwas nicht funktioniert, wie Sie das Lernprogramm durchzuarbeiten, sollten Sie unbedingt die [Problembehandlungsseite](troubleshooting.md).

## <a name="webconfig-transformations-versus-web-deploy-parameters"></a>Web.config-Transformationen im Vergleich zu Web Deploy-Parametern

Es gibt zwei Möglichkeiten zum Automatisieren des Prozess des Umschaltens *"Web.config"* Einstellungen der Datei: [Web.config-Transformationen](https://msdn.microsoft.com/library/dd465326.aspx) und [Web Deploy-Parametern](https://msdn.microsoft.com/library/ff398068.aspx). Ein *"Web.config"* Transformationsdatei enthält XML-Markup, der angibt, wie Sie ändern die *"Web.config"* -Datei, wenn er bereitgestellt wird. Sie können die verschiedene Änderungen für bestimmte Buildkonfigurationen und für bestimmte Veröffentlichungsprofile angeben. Die standardmäßige Buildkonfigurationen werden Debug- und, und Sie können benutzerdefinierte Konfigurationen erstellen. Ein Veröffentlichungsprofil entspricht in der Regel in einer zielumgebung. (Erfahren Sie mehr über Veröffentlichungsprofile in die [Bereitstellen in IIS als Testumgebung](deploying-to-iis.md) Tutorial.)

Web Deploy-Parameter können verwendet werden, um viele verschiedene Arten von Einstellungen anzugeben, die konfiguriert werden müssen, während der Bereitstellung, einschließlich Einstellungen, die in gefunden werden *"Web.config"* Dateien. Wenn an *"Web.config"* dateiänderungen, Web Deploy-Parameter sind zum Einrichten komplexer, aber sie sind hilfreich, wenn Sie den Wert festgelegt werden, bis Sie Sie bereitstellen, nicht kennen. Z. B. in einer unternehmensumgebung möglicherweise erstellen Sie eine *Bereitstellungspaket* und weisen Sie ihm eine Person in der IT-Abteilung in einer produktionsumgebung installieren und in der Lage, geben Verbindungszeichenfolgen oder Kennwörter, die Sie nicht der Fall ist, hat kennen.

Für das Szenario, das dieser tutorialreihe abdeckt, man im Voraus alle Elemente, die zu erfolgen hat die *"Web.config"* Datei, sodass Sie nicht benötigen, verwenden Sie Web Deploy-Parameter. Konfigurieren Sie einige Transformationen, die unterscheiden sich abhängig von der Buildkonfiguration verwendet und andere, unterscheiden sich je nach dem Veröffentlichungsprofil ein verwendet.

<a id="watransforms"></a>

## <a name="specifying-webconfig-settings-in-azure"></a>Angeben von Web.config-Einstellungen in Azure

Wenn die *"Web.config"* Einstellungen, die Sie ändern möchten, sind in der `<connectionStrings>` oder `<appSettings>` -Element, und wenn Sie für Web-Apps in Azure App Service bereitstellen, haben Sie eine weitere Option für die Automatisierung von Änderungen während der die Bereitstellung. Können Sie die Einstellungen, die in Azure in wirksam werden soll, Eingeben der **konfigurieren** Registerkarte die Seite des Verwaltungsportals für Ihre Web-app (Scrollen nach unten zu den **app-Einstellungen** und **Verbindungszeichenfolgen**  Abschnitten). Wenn Sie das Projekt bereitstellen, wendet Azure automatisch die Änderungen. Weitere Informationen finden Sie unter [Windows Azure-Websites: wie Anwendungszeichenfolgen und Verbindungszeichenfolgen](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx).

## <a name="default-transformation-files"></a>Standard-Transformationsdateien

In **Projektmappen-Explorer**, erweitern Sie *"Web.config"* , finden Sie unter den *"Web.Debug.config"* und *Datei "Web.Release.config"* Transformationsdateien aufgeführt, die werden standardmäßig für die zwei Standardkonfigurationen für den Build erstellt werden.

![Web.config_transform_files](web-config-transformations/_static/image1.png)

Sie können Transformationsdateien für benutzerdefinierte Build-Konfigurationen erstellen, indem Sie mit der rechten Maustaste in der Datei "Web.config", und wählen **Konfigurationstransformationen hinzufügen** aus dem Kontextmenü. Für dieses Tutorial müssen nicht dies tun, und die Menüoption "" ist deaktiviert, da Sie keine benutzerdefinierten Build-Konfigurationen erstellt haben.

Später erstellen Sie drei weitere Transformationsdateien, jeweils eine für den Test, staging und Produktion Veröffentlichungsprofile. Ein typisches Beispiel für eine Einstellung, die Sie in eine Transformation der veröffentlichungsprofildatei handhabten, da sie von der zielumgebung abhängt, ist ein WCF-Endpunkt, der für Tests und Produktion unterscheidet. Sie erstellen veröffentlichen Profildateien für die Transformation in späteren Tutorials nach der Erstellung die Veröffentlichungsprofile, denen sie mit aufgenommen werden.

## <a name="disable-debug-mode"></a>Deaktivieren des Debugmodus

Ein Beispiel für eine Einstellung, die Buildkonfiguration abhängt, anstatt die zielumgebung ist die `debug` Attribut. Für einen Releasebuild sollten Sie in der Regel debugging deaktiviert unabhängig davon, welche, denen Umgebung Sie bereitstellen. Aus diesem Grund wird standardmäßig der Visual Studio-Projektvorlagen erstellen *Datei "Web.Release.config"* Transformation von Dateien mit Code, der entfernt die `debug` -Attribut aus der `compilation` Element. Hier ist die Standardeinstellung *Datei "Web.Release.config"*: Zusätzlich zur Transformation für Beispielcode, der auskommentiert ist, enthält es Code in die `compilation` -Element, das entfernt die `debug` Attribut:

[!code-xml[Main](web-config-transformations/samples/sample1.xml?highlight=18)]

Die `xdt:Transform="RemoveAttributes(debug)"` Attribut gibt an, dass Sie möchten die `debug` Attribut von zu entfernenden der `system.web/compilation` Element in der bereitgestellten *"Web.config"* Datei. Dies geschieht jedes Mal, wenn Sie einen Releasebuild bereitstellen.

## <a name="limit-error-log-access-to-administrators"></a>Fehler-Log-Zugriff für Administratoren zu beschränken

Wenn ein Fehler vorliegt, wenn die Anwendung ausgeführt wird, die Anwendung eine generische Fehlerseite anstelle der Seite vom System generierte Fehler zeigt und verwendet die [Elmah-NuGet-Paket](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) für Fehler, Protokollierung und berichterstellung. Die `customErrors` Element in der Anwendung *"Web.config"* Datei gibt die Fehlerseite an:

[!code-xml[Main](web-config-transformations/samples/sample2.xml)]

Um die Seite "Fehler" zu sehen, ändern Sie vorübergehend die `mode` Attribut der `customErrors` Element von "RemoteOnly", "Auf" und führen Sie die Anwendung aus Visual Studio. Ein Fehler verursacht, indem Sie eine ungültige URL wie z. B. anfordern *Studentsxxx.aspx*. Anstelle eines Fehlers mit IIS-generierte "kann nicht die Ressource gefunden." Seite die *GenericErrorPage.aspx* Seite.

![Fehler (Seite)](web-config-transformations/_static/image2.png)

Um das Fehlerprotokoll anzuzeigen, ersetzen Sie alles in der URL nach der Portnummer *elmah.axd* (z. B. `http://localhost:51130/elmah.axd`), und drücken Sie die EINGABETASTE:

![ELMAH-Seite](web-config-transformations/_static/image3.png)

Vergessen Sie nicht, legen Sie die `customErrors` Element wieder in den Modus "RemoteOnly", wenn Sie fertig sind.

Auf dem Entwicklungscomputer ist es sinnvoll, kostenlosen Zugriff auf der Seite "Fehler", aber in der Produktion, die ein Sicherheitsrisiko dar. Für am Produktionsstandort werden sollen, fügen Sie eine Autorisierungsregel hinzu, die Fehler Protokoll Zugriff auf Administratoren beschränkt, und um sicherzustellen, dass die Einschränkung, die Sie funktioniert in Test- und auch bereitstellen möchten. Daher ist dies eine weitere Änderung, die Sie möchten, implementieren Sie jedes Mal, wenn Sie einen Releasebuild bereitstellen, weshalb er in gehört die *Datei "Web.Release.config"* Datei.

Open *Datei "Web.Release.config"* und fügen Sie einen neuen `location` Element unmittelbar vor dem schließenden `configuration` zu markieren, wie hier gezeigt.

[!code-xml[Main](web-config-transformations/samples/sample3.xml?highlight=27-34)]

Die `Transform` -Attributwert "Insert" bewirkt, dass dieser `location` Element als ein gleichgeordnetes Element hinzugefügt werden, um alle vorhandenen `location` Elemente in der *"Web.config"* Datei. (Es ist bereits eine `location` -Element, das gibt an, Autorisierung, Regeln für die **Update Gutschriften** Seite.)

Jetzt können Sie eine Vorschau die Transformation, um sicherzustellen, dass sie ordnungsgemäß codiert.

In **Projektmappen-Explorer**, mit der rechten Maustaste *Datei "Web.Release.config"* , und klicken Sie auf **Vorschau transformieren**.

![Transform-Menü "gerätevorschau"](web-config-transformations/_static/image4.png)

Eine Seite geöffnet wird, die Aufschluss über die Entwicklung *"Web.config"* -Datei auf der linken Seite und was die bereitgestellte *"Web.config"* Datei sieht auf der rechten Seite mit hervorgehobenen Änderungen.

![Vorschau der Debug-Transformation](web-config-transformations/_static/image5.png)

![Vorschau der Location-Transformation](web-config-transformations/_static/image6.png)

(In der Vorschauversion können Sie feststellen, einige zusätzlichen Änderungen, die Sie schreiben nicht transformiert werden, für die: Diese können in der Regel die Entfernung von Leerzeichen, die Auswirkungen auf die Funktionalität nicht.)

Wenn Sie die Website nach der Bereitstellung testen, testen Sie auch, ob die Autorisierungsregel effektiv ist.

> [!NOTE] 
> 
> **Sicherheitshinweis** nie Fehlerdetails für die Öffentlichkeit in einer produktionsanwendung anzuzeigen oder diese Informationen in einem öffentlichen Speicherort speichern. Angreifer können Fehlerinformationen verwenden, um Schwachstellen in einem Standort zu ermitteln. Wenn Sie ELMAH in Ihrer eigenen Anwendung verwenden, konfigurieren Sie ELMAH, um Sicherheitsrisiken zu minimieren. Das ELMAH-Beispiel in diesem Tutorial sollte nicht die empfohlene Konfiguration betrachtet werden. Es ist ein Beispiel, das ausgewählt wurde, um zu veranschaulichen, wie Sie einen Ordner zu behandeln, dass die Anwendung Dateien erstellt werden muss. Weitere Informationen finden Sie unter [Sicherung des Endpunkts ELMAH](https://code.google.com/p/elmah/wiki/SecuringErrorLogPages).


## <a name="a-setting-that-youll-handle-in-publish-profile-transformation-files"></a>Eine Einstellung, der Sie behandeln werde veröffentlichen Profildateien-transformation

Ein häufiges Szenario ist, damit *"Web.config"* Einstellungen, die in jeder Umgebung unterschiedlich, die Sie zum Bereitstellen sein müssen der Datei. Beispielsweise kann eine Anwendung, die einen WCF-Dienst aufruft, einen anderen Endpunkt in Test-und produktionsumgebungen benötigen. Die Contoso University-Anwendung enthält eine Einstellung dieser Art auch. Diese Einstellung steuert die auf einer Website. einen Indikator angezeigten, der anzeigt, welcher Umgebung Sie, wie z. B. Entwicklung, Test oder Produktion sind. Der Wert der Einstellung bestimmt, ob die Anwendung "(Dev)" angefügt wird oder "(Test)", um die wichtigsten Überschrift in der *Site.Master* Masterseite:

![Indikator der Umgebung](web-config-transformations/_static/image7.png)

Der Indikator für die Umgebung wird weggelassen, wenn die Anwendung in Staging oder Produktion ausgeführt wird.

Die Contoso University-Webseiten Lesen eines Werts, der festgelegt wird, im `appSettings` in die *"Web.config"* Datei, um zu ermitteln, welche Umgebung in die Anwendung ausgeführt wird:

[!code-xml[Main](web-config-transformations/samples/sample4.xml)]

Der Wert sollte in der testumgebung werden "Test" und "Prod" aus, für das Staging und Produktion.

Der folgende Code in einer Transformationsdatei wird diese Transformation implementieren:

[!code-xml[Main](web-config-transformations/samples/sample5.xml)]

Die `xdt:Transform` Attributwert "SetAttributes" gibt an, dass es sich bei der diese Transformation dient zum Ändern der Attributwerte von vorhandenen Elements in der *"Web.config"* Datei. Die `xdt:Locator` Attributwert "Match(key)" gibt an, dass das Element, das geändert werden, die eine, deren `key` Attribut entspricht der `key` dieser Stelle angegebenen Attributs. Nur anderen Attributs die `add` Element `value`, und das ist was geändert werden, wird in der bereitgestellten *"Web.config"* Datei. Den Code hier bewirkt, dass der `value` Attribut der `Environment` `appSettings` Element "Test" festgelegt werden, der *"Web.config"* -Datei, die bereitgestellt wird.

Diese Transformation gehört die veröffentlichen Profil Transformationsdateien aus dem Sie noch nicht erstellt haben. Sie erstellen und aktualisieren Sie die Transformationsdateien, die diese Änderung zu implementieren, wenn Sie die Veröffentlichungsprofile für die Umgebungen für Tests, Staging und Produktion erstellen. Sie müssen dies der [Bereitstellen in IIS](deploying-to-iis.md) und [für die Produktion bereitstellen](deploying-to-production.md) Tutorials.

> [!NOTE]
> Da diese Einstellung wird die `<appSettings>` -Element, Sie haben eine weitere Alternative zum Angeben der Transformations, beim Bereitstellen in Web-Apps in Azure App Service finden Sie unter [Einstellungen für die Angabe von "Web.config" in Azure](#watransforms) weiter oben in In diesem Thema.


## <a name="setting-connection-strings"></a>Festlegen von Verbindungszeichenfolgen

Obwohl die Standarddatei für die Transformation auf ein Beispiel für, die zeigt enthält, wie Sie eine Verbindungszeichenfolge zu aktualisieren, in den meisten Fällen müssen Sie nicht zum Einrichten der Verbindung Zeichenfolge Transformationen, da Sie die Verbindungszeichenfolgen im Veröffentlichungsprofil angeben können. Sie müssen dies der [Bereitstellen in IIS](deploying-to-iis.md) und [für die Produktion bereitstellen](deploying-to-production.md) Tutorials.

## <a name="summary"></a>Zusammenfassung

Sie haben jetzt fertig, so viel wie möglich mit *"Web.config"* Transformationen, bevor Sie die Veröffentlichungsprofile erstellen und Sie haben gesehen, dass eine Vorschau der in der bereitgestellten Web.config-Datei wird.

![Vorschau der Location-Transformation](web-config-transformations/_static/image8.png)

Im folgenden Tutorial: Sie richten Bereitstellungsaufgaben kümmern, die erfordern, Festlegen von Projekteigenschaften.

## <a name="more-information"></a>Weitere Informationen

Weitere Informationen zu Themen, die von diesem Lernprogramm behandelt, finden Sie unter [mithilfe von Web.config-Transformationen zum Ändern von Einstellungen in der Web.config-Zieldatei oder die Datei "App.config" während der Bereitstellung](https://go.microsoft.com/fwlink/p/?LinkId=282413#transforms) in den Einstieg in die Webbereitstellung für Visual Studio und ASP.NET.

> [!div class="step-by-step"]
> [Zurück](preparing-databases.md)
> [Weiter](project-properties.md)
