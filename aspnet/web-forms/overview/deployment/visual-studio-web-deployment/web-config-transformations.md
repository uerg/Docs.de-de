---
uid: web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations
title: "ASP.NET Web-Bereitstellung mit Visual Studio: Transformationen für die Datei \"Web.config\" | Microsoft Docs"
author: tdykstra
description: "Diese Reihe von Lernprogrammen wird gezeigt, wie bereitstellen (veröffentlichen) aus einer ASP.NET web-Anwendung auf Azure App Service-Web-Apps oder mit einem Hostinganbieter von Drittanbietern durch wählen..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 5a2a927b-14cb-40bc-867a-f0680f9febd7
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations
msc.type: authoredcontent
ms.openlocfilehash: a526275d76618c325a6b00f33cc550f28ab0cc00
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-web-deployment-using-visual-studio-webconfig-file-transformations"></a>ASP.NET Web-Bereitstellung mit Visual Studio: Transformationen für die Datei "Web.config"
====================
Durch [Tom Dykstra](https://github.com/tdykstra)

[Startprojekt herunterladen](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Diese Reihe von Lernprogrammen wird gezeigt, wie bereitstellen (veröffentlichen) aus einer ASP.NET web-Anwendung eines Drittanbieters Hostinganbieter oder Azure App Service-Web-Apps mithilfe von Visual Studio 2012 oder Visual Studio 2010. Informationen über die Reihen finden Sie unter [im ersten Lernprogramm, in der Reihe](introduction.md).


## <a name="overview"></a>Übersicht

Dieses Lernprogramm veranschaulicht das Automatisieren einer Änderung der *"Web.config"* Datei, wenn Sie unterschiedliche zielumgebungen bereitstellen. Die meisten Anwendungen gelten die Einstellungen der *"Web.config"* -Datei, die verschiedene sein muss, wenn die Anwendung bereitgestellt wird. Automatisieren des Prozesses, der diesen Änderungen verhindert, dass sie manuell jedes Mal, wenn Sie bereitstellen, die wäre mühsam und fehleranfällig ausgeführt werden müssen.

Hinweis: Wenn Sie eine Fehlermeldung erhalten, oder etwas funktioniert nicht, wenn Sie das Lernprogramm absolvieren, müssen Sie überprüfen die [Website](troubleshooting.md).

## <a name="webconfig-transformations-versus-web-deploy-parameters"></a>"Web.config" Transformationen im Vergleich zu Web Deploy-Parameter

Es gibt zwei Möglichkeiten zum Automatisieren von veränderlichen *"Web.config"* Settings-Datei: ["Web.config" Transformationen](https://msdn.microsoft.com/library/dd465326.aspx) und [Web Deploy-Parameter](https://msdn.microsoft.com/library/ff398068.aspx). Ein *"Web.config"* Transformationsdatei enthält XML-Markup, der angibt, wie Sie ändern die *"Web.config"* Datei bei der Bereitstellung. Sie können die verschiedene Änderungen für bestimmte Buildkonfigurationen und für bestimmte Veröffentlichungsprofile angeben. Die standardmäßige Buildkonfigurationen werden Debug- und, und Sie können benutzerdefinierte Buildkonfigurationen erstellen. Ein Veröffentlichungsprofil entspricht in der Regel eine zielumgebung. (Erfahren Sie mehr über Veröffentlichungsprofile in der [Bereitstellung in IIS als Testumgebung](deploying-to-iis.md) Lernprogramm.)

Web Deploy-Parameter können verwendet werden, können viele verschiedene Arten von Einstellungen angeben, die konfiguriert werden müssen, während der Bereitstellung, einschließlich der Einstellungen, die im gefunden *"Web.config"* Dateien. Wenn zur Angabe *"Web.config"* Datei ändert, Web Deploy-Parameter sind komplexer eingerichtet, aber sie sind hilfreich, wenn Sie den Wert festgelegt werden, bevor Sie bereitstellen, nicht kennen. Z. B. in einer unternehmensumgebung möglicherweise erstellen Sie eine *Bereitstellungspaket* und weisen Sie ihm eine Person in der IT-Abteilung in einer produktionsumgebung installieren und diese Person hat in der Lage, geben Verbindungszeichenfolgen oder Kennwörter, die Sie nicht kennen.

Für das Szenario, die diese Reihe von Lernprogrammen abdeckt, Sie Weiß im Voraus alle Elemente, die ausgeführt werden, zu der *"Web.config"* Datei, sodass Sie nicht benötigen, verwenden Sie Web Deploy-Parameter. Sie werden einige Transformationen, die unterscheiden sich abhängig von der Buildkonfiguration verwendet, und einige, unterscheiden sich je nach verwendete Veröffentlichungsprofil, konfigurieren.

<a id="watransforms"></a>

## <a name="specifying-webconfig-settings-in-azure"></a>Angeben von "Web.config"-Einstellungen in Azure

Wenn die *"Web.config"* dateieinstellungen, die Sie ändern möchten, sind in der `<connectionStrings>` oder `<appSettings>` -Element, und wenn Sie für die Web-Apps in Azure App Service bereitstellen, haben Sie eine weitere Option für die Automatisierung von Änderungen während Bereitstellung. Können Sie die Einstellungen, die in Azure im wirksam werden soll, Eingeben der **konfigurieren** auf der Registerkarte die Seite des Verwaltungsportals für Ihre Web-app (führen Sie einen Bildlauf nach unten, um die **app-Einstellungen** und **Verbindungszeichenfolgen**  Abschnitte). Wenn Sie das Projekt bereitstellen, die Änderungen automatisch von Azure angewendet. Weitere Informationen finden Sie unter [Windows Azure-Websites: wie Anwendungszeichenfolgen und Verbindung Zeichenfolgen können](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx).

## <a name="default-transformation-files"></a>Standard-Transformationsdateien

In **Projektmappen-Explorer**, erweitern Sie *"Web.config"* , finden Sie unter der *"Web.Debug.config"* und *Web.Release.config* Transformation-Dateien mit werden standardmäßig für die zwei standardmäßigen Projektmappenbuild-Konfigurationen erstellt werden.

![Web.config_transform_files](web-config-transformations/_static/image1.png)

Sie können Transformationsdateien für benutzerdefinierte Buildkonfigurationen erstellen, indem Sie mit der rechten Maustaste in der Datei "Web.config" und **hinzufügen Config transformiert** aus dem Kontextmenü. Für dieses Lernprogramm müssen Sie keine nachholen, und die Option ist deaktiviert, da Sie alle benutzerdefinierten Build-Konfigurationen erstellt haben.

Später erstellen Sie drei weitere Transformationsdateien, jeweils eine für den Test, staging und Produktion Veröffentlichungsprofile. Ein typisches Beispiel für eine Einstellung, die Sie in einer Transformation veröffentlichungsprofildatei verarbeiten würden, da es von der zielumgebung abhängt, ist ein WCF-Endpunkt, der für Tests und produktionsumgebung unterschiedlich ist. Sie erstellen veröffentlichen Transformation Profildateien in späteren Lernprogrammen nach der Erstellung der Veröffentlichungsprofile, denen sie mit aufgenommen werden.

## <a name="disable-debug-mode"></a>Deaktivieren Sie den Debugmodus

Ein Beispiel für eine Einstellung, die Buildkonfiguration abhängt, anstatt zielumgebung ist die `debug` Attribut. Für einen Releasebuild sollten Sie in der Regel Debuggen deaktiviert, unabhängig davon, welcher, denen Umgebung bereitgestellt werden. Daher standardmäßig der Visual Studio-Projektvorlagen erstellen *Web.Release.config* Dateien mit Code, der entfernt Transformieren der `debug` -Attribut aus der `compilation` Element. Hier ist die Standardeinstellung *Web.Release.config*: Zusätzlich zu einigen Transformation Beispielcode, der auskommentiert ist, enthält es Code in der `compilation` Element, das entfernt die `debug` Attribut:

[!code-xml[Main](web-config-transformations/samples/sample1.xml?highlight=18)]

Die `xdt:Transform="RemoveAttributes(debug)"` Attribut gibt an, dass Sie die `debug` Attribut aus zu entfernenden der `system.web/compilation` Element in der bereitgestellten *"Web.config"* Datei. Dies erfolgt jedes Mal, wenn Sie einen Releasebuild bereitstellen.

## <a name="limit-error-log-access-to-administrators"></a>Fehler-Protokolls den Zugriff auf Administratoren beschränken

Wenn ein Fehler aufgetreten ist, während die Anwendung ausgeführt wird, die Anwendung zeigt eine generische Fehlerseite anstelle der vom System generierte Fehlerseite an und verwendet die [Elmah NuGet-Paket](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) für Fehler, Protokollierung und-berichterstellung. Die `customErrors` Element in der Anwendung *"Web.config"* Datei gibt die Fehlerseite an:

[!code-xml[Main](web-config-transformations/samples/sample2.xml)]

Um die Seite "Fehler" angezeigt wird, ändern Sie vorübergehend die `mode` Attribut des der `customErrors` Element aus "RemoteOnly" auf "On" und führen Sie die Anwendung aus Visual Studio. Verursacht einen Fehler durch eine ungültige URL, z. B. anfordern *Studentsxxx.aspx*. Statt eines Fehlers IIS generiert "kann nicht die Ressource gefunden werden" Seite sehen Sie die *GenericErrorPage.aspx* Seite.

![Fehler (Seite)](web-config-transformations/_static/image2.png)

Um das Fehlerprotokoll angezeigt wird, ersetzen Sie alles in der URL die Portnummer mit *elmah.axd* (z. B. `http://localhost:51130/elmah.axd`), und drücken Sie die EINGABETASTE:

![Seite "ELMAH"](web-config-transformations/_static/image3.png)

Vergessen Sie nicht, legen Sie die `customErrors` Element wieder in den Modus "RemoteOnly", wenn Sie fertig sind.

Auf dem Entwicklungscomputer empfiehlt es sich um kostenlosen Zugriff auf die Fehlerseite-Protokoll, aber in der Produktion, die ein Sicherheitsrisiko zu ermöglichen. Für die Produktionsstandort soll So fügen Sie eine Autorisierungsregel hinzu, die Zugriff auf die Fehler-Ereignisprotokoll auf Administratoren beschränkt, und um sicherzustellen, dass die Beschränkung funktioniert Sie Test und staging auch hinzufügen möchten. Dies ist daher besteht eine weitere Änderung, die zu implementierenden jedes Mal, wenn Sie einen Releasebuild bereitstellen, weshalb es gehört der *Web.Release.config* Datei.

Open *Web.Release.config* und fügen Sie einen neuen `location` unmittelbar vor das schließende Element `configuration` kennzeichnen, wie hier gezeigt.

[!code-xml[Main](web-config-transformations/samples/sample3.xml?highlight=27-34)]

Die `Transform` Attribut-Wert von "Insert" bewirkt, dass `location` Element als ein gleichgeordnetes Element hinzugefügt werden, um alle vorhandenen `location` Elemente in der *"Web.config"* Datei. (Es ist bereits eine `location` Element, der angibt, Autorisierung von Regeln für die **Update Gutschriften** Seite.)

Jetzt können Sie eine Vorschau die Transformation, um sicherzustellen, dass sie ordnungsgemäß codiert.

In **Projektmappen-Explorer**, mit der rechten Maustaste *Web.Release.config* , und klicken Sie auf **Vorschau transformieren**.

![Transform-Menü in der Vorschau anzeigen](web-config-transformations/_static/image4.png)

Eine Seite geöffnet, die Aufschluss über die Entwicklung *"Web.config"* Datei auf der linken Seite und was der bereitgestellten *"Web.config"* Datei sieht wie folgt auf der rechten Seite mit Änderungen, die hervorgehoben.

![Vorschau der Debug-Transformation](web-config-transformations/_static/image5.png)

![Vorschau der Speicherort-Transformation](web-config-transformations/_static/image6.png)

(In der Vorschau, fallen Ihnen möglicherweise einige zusätzlichen Änderungen, die Sie schreiben, nicht für transformiert: Diese enthalten in der Regel das Entfernen von Leerzeichen, die nicht beeinträchtigt.)

Wenn Sie den Standort nach der Bereitstellung testen, testen Sie auch, ob die Autorisierungsregel wirksam ist.

> [!NOTE] 
> 
> **Sicherheitshinweis** nie Fehlerdetails an die Öffentlichkeit in einer produktionsanwendung anzuzeigen oder diese Informationen in einem öffentlichen Speicherort speichern. Fehlerinformationen können Angreifer um Schwachstellen in einem Standort zu ermitteln. Wenn Sie in Ihrer eigenen Anwendung ELMAH verwenden, konfigurieren Sie ELMAH um Sicherheitsrisiken zu minimieren. Das ELMAH-Beispiel in diesem Lernprogramm sollte nicht die empfohlene Konfiguration verwendet werden. Es ist ein Beispiel, das ausgewählt wurde, um zu veranschaulichen, wie einen Ordner behandeln zu können, die Anwendung auf die Dateien erstellt werden muss. Weitere Informationen finden Sie unter [sichern den Endpunkt ELMAH](https://code.google.com/p/elmah/wiki/SecuringErrorLogPages).


## <a name="a-setting-that-youll-handle-in-publish-profile-transformation-files"></a>Eine Einstellung, der Sie behandeln müssen veröffentlichen Profildateien-transformation

Ein häufiges Szenario ist, damit *"Web.config"* Datei Einstellungen, die in jeder Umgebung unterschiedlich sein müssen, das Sie bereitstellen. Z. B. möglicherweise eine Anwendung, die einen WCF-Dienst aufruft, einen anderen Endpunkt in Test-und produktionsumgebungen. Die Anwendung von Contoso University enthält auch eine Einstellung dieser Art. Diese Einstellung steuert auf den Seiten einer Website einen Indikator angezeigten, der Sie der Umgebung Sie informiert, wie z. B. Entwicklung, Test oder Produktion sind. Der Wert der Einstellung bestimmt, ob die Anwendung "(Dev)" angefügt werden, oder "(Test)", um die wichtigsten Überschrift in der *Site.Master* Gestaltungsvorlage:

![Indikator der Umgebung](web-config-transformations/_static/image7.png)

Der Indikator für die Umgebung wird ausgelassen, wenn die Anwendung in einer Staging- oder produktionsumgebung ausgeführt wird.

Contoso University Web Pages Lesen eines Werts, der festgelegt wird, in `appSettings` in der *"Web.config"* Datei, um zu bestimmen, welche Umgebung in die Anwendung ausgeführt wird:

[!code-xml[Main](web-config-transformations/samples/sample4.xml)]

Der Wert sollte in der testumgebung werden "Test" und "Prod" für das Staging und Produktion.

Der folgende Code in einer Transformationsdatei wird diese Transformation implementieren:

[!code-xml[Main](web-config-transformations/samples/sample5.xml)]

Die `xdt:Transform` -Attributwert "SetAttributes" gibt an, dass der Zweck dieser Transformation so ändern Sie die Attributwerte eines vorhandenen Elements in der *"Web.config"* Datei. Die `xdt:Locator` -Attributwert "Match(key)" gibt an, dass das Element geändert werden, die dem Zertifikat entspricht, dessen `key` Attribut entspricht der `key` hier angegebenen Attributs. Nur anderen Attributs der `add` Element ist `value`, und was geändert werden, wird in der bereitgestellten *"Web.config"* Datei. Die hier angezeigten Codes erzielt Ursachen der `value` Attribut des der `Environment` `appSettings` Element auf "Test" festgelegt werden, der *"Web.config"* -Datei, die bereitgestellt wird.

Diese Transformation gehört, in den Veröffentlichen Profil Transform-Dateien, die Sie noch nicht erstellt haben. Sie erstellen und aktualisieren Sie die Transformationsdateien, die diese Änderung zu implementieren, bei der Erstellung der Veröffentlichungsprofile für die Umgebungen für Tests, Staging und Produktion. Sie erledigen, die Sie der [für IIS bereitstellen](deploying-to-iis.md) und [für die Produktion bereitstellen](deploying-to-production.md) Lernprogramme.

> [!NOTE]
> Da diese Einstellung wird die `<appSettings>` Element, haben Sie eine Alternative zur Transformation für das angeben, wenn Sie auf Web-Apps in Azure App Service finden Sie unter Bereitstellen [angeben "Web.config"-Einstellungen in Azure](#watransforms) weiter oben in In diesem Thema.


## <a name="setting-connection-strings"></a>Festlegen von Verbindungszeichenfolgen

Obwohl die Standarddatei für die Transformation beispielhaft, das zum Aktualisieren einer Verbindungszeichenfolge veranschaulicht enthält, in den meisten Fällen müssen nicht Sie zum Einrichten der Verbindung Zeichenfolge Transformationen, da Sie die Verbindungszeichenfolgen im Veröffentlichungsprofil festlegen können. Sie erledigen, die Sie der [für IIS bereitstellen](deploying-to-iis.md) und [für die Produktion bereitstellen](deploying-to-production.md) Lernprogramme.

## <a name="summary"></a>Zusammenfassung

Sie haben nun ausgeführt, so viel wie Sie können *"Web.config"* Transformationen, bevor Sie die Veröffentlichungsprofile erstellen und eine Vorschau der in der bereitgestellten Datei "Web.config" wird angezeigt.

![Vorschau der Speicherort-Transformation](web-config-transformations/_static/image8.png)

Im folgenden Lernprogramm Sie müssen der Einrichtung Bereitstellungsaufgaben darauf achten, die erfordern, Festlegen von Projekteigenschaften.

## <a name="more-information"></a>Weitere Informationen

Weitere Informationen zu den von diesem Lernprogramm behandelten Themen finden Sie unter [mithilfe von "Web.config" Transformationen zum Ändern von Einstellungen in der Datei "Web.config" der Zieldatei oder die Datei "App.config" während der Bereitstellung](https://go.microsoft.com/fwlink/p/?LinkId=282413#transforms) in der Web Content Map für die Bereitstellung Visual Studio und ASP.NET.

>[!div class="step-by-step"]
[Zurück](preparing-databases.md)
[Weiter](project-properties.md)
