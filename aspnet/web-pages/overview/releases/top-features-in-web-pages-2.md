---
uid: web-pages/overview/releases/top-features-in-web-pages-2
title: Die wichtigsten Funktionen in ASP.NET Web Pages 2 | Microsoft-Dokumentation
author: microsoft
description: Dieses Thema enthält eine Übersicht über die wichtigsten neuen Funktionen in ASP.NET Web Pages 2, eine einfache Programmierung Webframework, das die WebMatr enthalten ist...
ms.author: aspnetcontent
ms.date: 02/13/2012
ms.assetid: cc712e72-c3d0-4e43-bc2d-28cc09cd8f71
msc.legacyurl: /web-pages/overview/releases/top-features-in-web-pages-2
msc.type: authoredcontent
ms.openlocfilehash: 6e20dedd19ae458b9881973570f23b5d77dda654
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37827400"
---
<a name="the-top-features-in-aspnet-web-pages-2"></a>Die wichtigsten Funktionen in ASP.NET Web Pages 2
====================
durch [Microsoft](https://github.com/microsoft)

> Dieser Artikel bietet einen Überblick über die wichtigsten neuen Features in ASP.NET Web Pages 2 RC, eine einfache Programmierung Webframework, das enthalten ist [Microsoft WebMatrix 2 RC](https://www.microsoft.com/web/).
> 
> **Was enthalten ist:** 
> 
> - [Installieren von WebMatrix](#install)
> - [Neue und verbesserte Funktionen](#New_and_Enhanced_Features)
> 
>     - [Änderungen für die RC-Version](#Changes_for_the_RC_Version)
>     - [Änderungen für die Beta-Version](#Changes_for_the_Beta_Version)
>     - [Verwenden die neue und aktualisierte Websitevorlagen](#templates)
>     - [Überprüfen der Benutzereingabe](#validation)
>     - [Aktivieren Anmeldungen von Facebook und andere Standorte mithilfe von OAuth und OpenID](#oauthsetup)
>     - [Hinzufügen von Zuordnungen mithilfe der Maps-Hilfe](#maphelper)
>     - [Web Pages-Anwendungen parallel ausführen](#sidebyside)
>     - [Rendern von Seiten für Mobile Geräte](#mobile)
> - [Additional Resources](#resources) (Zusätzliche MSBuild-Ressourcen)
> 
> > [!NOTE]
> > In diesem Thema wird davon ausgegangen, dass Sie WebMatrix zum Arbeiten mit dem ASP.NET Web Pages 2-Code verwenden. Allerdings als mit Web Pages 1, Sie auch Web Pages 2-Websites mithilfe von Visual Studio erstellen können, erweiterte können IntelliSense-Funktionen und Debuggen. Um mit Webseiten in Visual Studio arbeiten, müssen Sie zunächst Visual Studio 2010 SP1, Visual Web Developer Express 2010 SP1 oder Visual Studio 11 Beta installieren. Installieren Sie dann die ASP.NET MVC 4 Beta, Vorlagen und Tools zum Erstellen von ASP.NET MVC 4 und Web Pages 2-Anwendungen in Visual Studio enthält.
> 
> 
> *Letzte Aktualisierung: 18 Juni 2012*


<a id="install"></a>
## <a name="installing-webmatrix"></a>Installieren von WebMatrix

Um Webseiten zu installieren, können Sie Microsoft Web Platform Installer, dies ist eine kostenlose Anwendung, die ganz einfach installieren und Konfigurieren von Web-verwandten Technologien. Installieren Sie die WebMatrix 2-Betaversion, einschließlich Web Pages 2 Beta.

1. Navigieren Sie zu der Seite "Installation", für die neueste Version des Webplattform-Installer:

    [https://go.microsoft.com/fwlink/?LinkId=226883](https://go.microsoft.com/fwlink/?LinkId=226883)

    > [!NOTE]
    > Wenn Sie bereits über WebMatrix 1 installiert haben, wird es von diese Installation WebMatrix 2 Beta aktualisiert. Sie können Websites ausführen, die mit Version 1 oder 2 auf demselben Computer erstellt wurden. Weitere Informationen finden Sie im Abschnitt für [ausgeführten Web Pages-Anwendungen nebeneinander](#sidebyside).
2. Wählen Sie **jetzt installieren**. 

    Wenn Sie Internet Explorer verwenden, wechseln Sie mit dem nächsten Schritt. Wenn Sie einen anderen Browser wie Mozilla Firefox oder Google Chrome verwenden, werden Sie aufgefordert, speichern die *Webmatrix.exe* Datei auf Ihrem Computer. Speichern Sie die Datei, und klicken Sie dann auf, um den Installer zu starten.
3. Führen Sie das Installationsprogramm, und wählen Sie die **installieren** Schaltfläche. WebMatrix und ASP.NET-Webseiten wird installiert.

## <a id="New_and_Enhanced_Features"></a>  Neue und verbesserte Funktionen

### <a id="Changes_for_the_RC_Version"></a>  Änderungen für die RC-Version (Juni 2012)

Die RC-Version im Juni 2012 verfügt über einige Änderungen von der Aktualisierung der Beta-Version, die im März 2012 veröffentlicht wurde. Diese Änderungen sind:

- Ein `Validation.AddFormError` Methode wurde hinzugefügt, um die `Validation` Helper. Dies ist hilfreich, wenn Sie die Überprüfung manuell durchführen (z. B. Sie validieren einen Wert, der in der Abfragezeichenfolge übergeben wird) und eine Fehlermeldung angezeigt, die von angezeigt werden können, hinzugefügt werden soll die `Html.ValidationSummary` Methode. Weitere Informationen finden Sie im Abschnitt [überprüft Daten, nicht ist stammen direkt von Benutzern](https://go.microsoft.com/fwlink/?LinkId=253002#Validating_Data_That_Doesnt_Come_Directly_from_Users) in [Validating User Input in ASP.NET Web Pages (Razor) Sites](https://go.microsoft.com/fwlink/?LinkId=253002).
- Die Funktionalität für die Bündelung und Minimierung wurde aus der ASP.NET Web Pages 2-Kernassemblys entfernt. Infolgedessen wird der `Assets` weiter unten in diesem Dokument ist nicht verfügbar. Stattdessen müssen Sie installieren die [ASP.NET-Optimierung](http://nuget.org/packages/Microsoft.Web.Optimization/0.1) NuGet-Paket. Weitere Informationen finden Sie unter [Bündelung und Minimierung von Ressourcen in einer ASP.NET Web Pages (Razor) Website](https://go.microsoft.com/fwlink/?LinkId=255373).
- Zusätzliche Assemblys zur Unterstützung von ASP.NET Web Pages 2 wurden hinzugefügt. Der nur nennenswerte Auswirkungen dieser Änderung liegt darin, dass mehrere Assemblys in einer Website möglicherweise *Bin* Ordner nach dem Erstellen einer Website oder einer Website mit einem Hostinganbieter bereitstellen.

<a id="Changes_for_the_Beta_Version"></a>
### <a name="changes-for-the-beta-version-february-2012"></a>Änderungen für die Beta-Version (Februar 2012)

Die Betaversion veröffentlicht im Februar 2012 verfügt über nur wenige Änderungen aus der Beta-Version, die im Dezember 2011 veröffentlicht wurde. Diese Änderungen sind:

- Razor unterstützt jetzt bedingte Attribute. In einem HTML-Elements, sofern Sie ein Attribut auf einen Wert gesetzt, aufgelöst wird, im Servercode, um `false` oder `null`, ASP.NET rendert nicht das Attribut überhaupt. Beispiel: Angenommen Sie, dass Sie das folgende Markup für ein Kontrollkästchen haben:

    [!code-html[Main](top-features-in-web-pages-2/samples/sample1.html)]

    Wenn der Wert des `checked1` löst in `false` oder `null`, `checked` Attribut wird nicht gerendert. Dies ist eine unterbrechende Änderung.
- Die `Validation.GetHtml` Methode wurde umbenannt in `Validation.For`. Dies ist eine unterbrechende Änderung. `Validation.GetHtml` funktioniert nicht in der Betaversion.
- Sie können jetzt enthalten die `~` Operator im Markup zum Stammverzeichnis der Website zu verweisen, ohne die `Href` Funktion. (D. h. der Razor-Parser kann nun suchen und Auflösen der `~` Operator ohne einen expliziten Methodenaufruf `Href`.) Die `Href` Methode weiterhin funktioniert, damit diese sich nicht um eine unterbrechende Änderung ist.

    Angenommen, Sie bisher Markup wie folgt:

    `<a href="@Href("~/Default.cshtml")">Home</a>`

    Sie können jetzt Markup wie folgt verwenden:

    `<a href="~/Default.cshtml">Home</a>`
- Die `Scripts` Hilfsprogramm für die Verwaltung von Ressourcen (Ressource) wurde durch ersetzt die `Assets` Hilfsmethode, die andere Methoden, wie die folgende:

  - Für `Scripts.Add`, verwenden `Assets.AddScript`
  - Für `Scripts.GetScriptTags`, verwenden `Assets.GetScripts`

    Dies ist eine unterbrechende Änderung. die `Scripts` Klasse ist nicht in der Betaversion verfügbar. Die Codebeispiele in diesem Dokument, mit denen Asset-Management wurden mit dieser Änderung aktualisiert.

<a id="templates"></a>
### <a name="using-the-new-and-updated-site-templates"></a>Verwenden die neue und aktualisierte Websitevorlagen

Die **Starter Site** Vorlage wurde aktualisiert, damit sie auf die Web Pages 2 ausgeführt wird standardmäßig. Darüber hinaus die folgenden neuen Funktionen:

- Mobilgerätefreundliche Seitenrendering. Durch die Verwendung von CSS-Stile und die `@media` Auswahl der **Starter Site** verbessertes Rendering von Seiten auf kleineren Bildschirmen, einschließlich mobiler Geräte Bildschirme enthält.
- Verbesserte Optionen für Mitgliedschaft und Authentifizierung. Sie können Benutzern die Anmeldung in Ihre Website mithilfe ihrer Konten von anderen Websites für soziale Netzwerke, z. B. Twitter, Facebook und Windows Live. Weitere Informationen finden Sie unter den [Anmeldungen aktivieren, Facebook oder andere Standorte mithilfe von OAuth und OpenID](#oauthsetup) Abschnitt.
- HTML5-Elementen.

Die neue **persönliche Website** Vorlage können Sie eine Website erstellen, die einen persönlichen Blog, eine Seite "Fotos" und ein Twitter-Seite enthält. Sie können anpassen, dass eine Website auf Grundlage der **persönliche Website** Vorlage wie folgt:

- Ändern Sie das Aussehen des Standorts durch Bearbeiten der Layoutdatei (*\_SiteLayout.cshtml*) und die Stile-Datei (*"Site.CSS"*).
- Installieren Sie die NuGet-Pakete, die Funktionalität Ihrer Website hinzufügen. Informationen zum Installieren von Paketen, einschließlich der ASP.NET Web Helpers Library, finden Sie im Lernprogramm zu [Hilfsprogramme installieren](https://go.microsoft.com/fwlink/?LinkId=202889#webhelpers).

Für den Zugriff auf die **persönliche Website** Vorlage wählen **Vorlagen** auf die WebMatrix **Schnellstart** Bildschirm.

[![topseven-personalsite-1](top-features-in-web-pages-2/_static/image2.png)](top-features-in-web-pages-2/_static/image1.png)

In der **Vorlagen** Dialogfeld auf die **persönliche Website** Vorlage.

[![topseven-personalsite-2](top-features-in-web-pages-2/_static/image4.png)](top-features-in-web-pages-2/_static/image3.png)

Die Landing Page des der **persönliche Website** Vorlage können Sie das Verfolgen von Links zum Einrichten Ihres Blogs, Twitter-Seite und Fotos Seite.

[![topseven-personalsite-3](top-features-in-web-pages-2/_static/image6.png)](top-features-in-web-pages-2/_static/image5.png)

<a id="validation"></a>
### <a name="validating-user-input"></a>Überprüfen der Benutzereingabe

In Web Pages 1, zum Überprüfen von Benutzereingaben in Formularen übermittelte, verwenden Sie die `System.Web.WebPages.Html.ModelState` Klasse. (Dies wird in einigen Codebeispielen veranschaulicht, in der Web Pages 1-Lernprogramm, das mit dem Titel [arbeiten mit Daten](../data/5-working-with-data.md).) Sie können diesen Ansatz weiterhin in Web Pages 2 verwenden. Web Pages 2 bietet jedoch auch über verbesserte Tools zum Überprüfen der Benutzereingabe:

- Neue Validierungsklassen, einschließlich `System.Web.WebPages.ValidationHelper` und `System.Web.WebPages.Validator`, mit denen Sie die leistungsstarken Überprüfung Aufgaben mit wenigen Codezeilen ausführen können.
- Optional können die clientseitige Validierung, die unmittelbares Feedback, der zu überprüfende Benutzer statt einen Roundtrip zum Server Fehler bei der Validierung bereitstellt. (Aus Sicherheitsgründen wird Validierung auf dem Server ausgeführt, auch wenn die Prüfungen auf dem Client zuvor ausgeführt wurden.)

Um die neuen Validierungsfeatures verwenden möchten, führen Sie folgende Schritte aus:

Registrieren Sie in den Code der Seite, ein Element mithilfe der Methoden überprüft werden die `Validation` Helper: `Validation.RequireField`, `Validation.RequireFields` (um mehrere Elemente als erforderlich registrieren), oder `Validation.Add`. Die `Add` Methode können Sie angeben, andere Arten von Überprüfungen, z. B. den Datentyp überprüfen, vergleichen die Einträge in verschiedenen Feldern, die Länge der Zeichenfolge überprüft, und die Nachrichtenaustauschmuster (mit regulären Ausdrücken). Hier einige Beispiele:

[!code-html[Main](top-features-in-web-pages-2/samples/sample2.html)]

Um ein Feld-spezifischer Fehler anzuzeigen, rufen `Html.ValidationMessage` im Markup für jedes Element, das überprüft wird:

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample3.cshtml)]

Um eine Zusammenfassung anzuzeigen (`<ul>` Liste) auf der Seite aller Fehler `Html.ValidationSummary` im Markup:

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample4.cshtml)]

Diese Schritte sind ausreichend, um eine serverseitige Validierung zu implementieren. Wenn Sie die clientseitige Validierung hinzufügen möchten, gehen Sie darüber hinaus.

Fügen Sie die folgenden Skriptverweise für die Datei in die `<head>` Abschnitt einer Webseite. Die ersten beiden Skriptverweise zeigen auf Remotedateien auf einem Content Delivery Network (CDN)-Server. Der dritte Verweis zeigt auf eine lokale Skriptdatei. Produktions-apps sollten als Fallback implementieren, wenn das CDN nicht verfügbar ist. Testen Sie das Fallback.

[!code-html[Main](top-features-in-web-pages-2/samples/sample5.html)]

Die einfachste Möglichkeit zum Abrufen einer lokalen Kopie der *jquery.validate.unobtrusive.min.js* -Bibliothek ist eine neue Web Pages-Website, die basierend auf einer der Websitevorlagen (z. B. Starter Site) erstellen. Die Website, die von der Vorlage erstellten umfasst *jquery.validate.unobtrusive.js* -Datei in die Ordner "Scripts", Sie können es von dem auf Ihrer Website kopieren.

Wenn Ihre Website verwendet eine<em>\_SiteLayout</em> Seite, um zu steuern, Seitenlayouts, Sie können diese Skriptverweise in dieser Seite einschließen, damit die Überprüfung für alle Inhaltsseiten verfügbar ist. Wenn Sie die Überprüfung nur auf bestimmte Seiten ausführen möchten, können Sie den Ressourcen-Manager, um die Skripts auf nur die Seiten zu registrieren. Zu diesem Zweck rufen `Assets.AddScript(path)` auf der Seite, die Sie überprüfen möchten und auf jede der Skriptdateien verweisen. Fügen Sie dann auf einen Aufruf von `Assets.GetScripts` in die  <em>\_SiteLayout</em> Seite, um den registrierten Rendern `<script>` Tags. Weitere Informationen finden Sie im Abschnitt [Registrieren von Skripts mit dem Ressourcen-Manager](#resmanagement).

Rufen Sie in das Markup für ein einzelnes Element der `Validation.For` Methode. Diese Methode gibt die Attribute aus, dass jQuery kann verknüpfen, um die clientseitige Validierung bereitzustellen. Zum Beispiel:

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample6.cshtml)]

Das folgende Beispiel zeigt eine Seite, die überprüft, Benutzereingaben in einem Formular ob. Zum Ausführen und Testen dieses Codes für die Validierung, tun Sie dies:

1. Erstellen einer neuen Website mithilfe einer der WebMatrix 2-Websitevorlagen, die enthält eine *Skripts* Ordner, z. B. die **Starter Site** Vorlage.
2. Erstellen Sie in den neuen Standort, ein neues *.cshtml* Seite, und Ersetzen Sie den Inhalt der Seite mit den folgenden Code.
3. Führen Sie die Seite in einem Browser. Geben Sie gültige und ungültige Werte, um die Auswirkungen auf die Validierung finden Sie unter. Z. B. ein erforderliches Feld leer lassen, oder geben Sie einen Buchstaben in die **Gutschriften** Feld.


[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample7.cshtml)]

Dies ist die Seite auf, wenn ein Benutzer eine gültige Eingabe übermittelt:

[![topSeven-valid-1](top-features-in-web-pages-2/_static/image8.png)](top-features-in-web-pages-2/_static/image7.png)

Dies ist die Seite auf, wenn ein Benutzer mit einem erforderlichen Feld leer gelassen wird übermittelt:

[![topSeven-valid-2](top-features-in-web-pages-2/_static/image10.png)](top-features-in-web-pages-2/_static/image9.png)

Hier ist die Seite, wenn ein Benutzer mit etwas anderes als eine ganze Zahl im sendet die **Gutschriften** Feld:

[![topSeven-valid-3](top-features-in-web-pages-2/_static/image12.png)](top-features-in-web-pages-2/_static/image11.png)

Weitere Informationen finden Sie in den folgenden Blogbeiträgen:

- [Aktualisiert die Validierung in Web Pages 2](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2344) Grundlagen zum Hinzufügen der Validierung mithilfe der `Validation` Helper (nur Server-Seite)
- [Aktualisiert die Validierung in Web Pages 2, Teil 2](http://www.mikepope.com/blog/DisplayBlog.aspx?permalink=2347) die clientseitige Validierung hinzufügen.
- [Aktualisiert die Validierung in Web Pages 2, Teil 3](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2351) Formatierung Validierungsfehler.

<a id="resmanagement"></a>
### <a name="registering-scripts-using-the-assets-manager"></a>Registrieren von Skripts, die mit dem Ressourcen-Manager

Der Ressourcen-Manager ist ein neues Feature, mit denen Sie im Server-Code zum Registrieren und Rendern von Clientskripts. Diese Funktion ist hilfreich, wenn Sie mit Code aus mehreren Dateien (z. B. Layoutseiten, Inhaltsseiten, Hilfsprogramme usw.) arbeiten, die auf einer Seite zur Laufzeit kombiniert werden. Der Ressourcen-Manager koordiniert die Quelldateien aus, um sicherzustellen, dass Skriptdateien korrekt referenziert werden und effizient auf der gerenderten Seite, unabhängig davon, welche Codedateien, die sie von aufgerufen werden oder wie oft sie aufgerufen werden. Rendert der Ressourcen-Manager auch `<script>` tags am richtigen Ort, damit die Seite schnell (ohne Herunterladen von Skripts beim Rendering) zu laden und Fehler zu vermeiden, die auftreten können kann, wenn Skripts, vor dem Rendern aufgerufen werden abgeschlossen ist.

Nehmen wir beispielsweise an, dass Sie einen benutzerdefinierten Hilfsmethoden erstellen, die eine JavaScript-Datei aufruft, und Sie dieses Hilfsprogramm an drei verschiedenen Stellen im Code aufrufen, Seite "Inhalt". Wenn Sie nicht den Ressourcen-Manager verwenden, registrieren wird das Skript in das Hilfsprogramm, drei verschiedene ruft `<script>` Tags, die alle zeigen Sie auf die Skriptdatei in der gerenderten Seite angezeigt wird. Darüber hinaus je nachdem, wo die `<script>` Tags, die in der gerenderten Seite eingefügt werden, können Fehler auftreten, wenn das Skript versucht, die bestimmte Seitenelemente zugreifen, bevor die Seite vollständig geladen wurde. Wenn Sie den Ressourcen-Manager verwenden, um das Skript zu registrieren, vermeiden Sie diese Probleme.

Sie können ein Skript mit dem Ressourcen-Manager registrieren, auf diese Weise:

- Rufen Sie in den Code, der das Skript verweisen, muss die `Assets.AddScript` Methode.
- In einem  *\_SiteLayout* Seite, rufen Sie die `Assets.GetScripts` Methode zum Rendern der `<script>` Tags. 

    > [!NOTE]
    > Platzieren Aufrufe von `Assets.GetScripts` als das letzte Element in der `<body>` Element der  *\_SiteLayout* Seite. Dadurch wird die Seite schneller geladen, und können das Skriptfehler zu vermeiden.

Das folgende Beispiel zeigt die Funktionsweise des Ressourcen-Managers. Der Code enthält die folgenden Elemente:

- Eine benutzerdefinierte Hilfsprogramm mit dem Namen `MakeNote`. Dieses Hilfsprogramm wird eine Zeichenfolge in ein Feld gerendert, durch das wrapping eine `div` -Element, formatiert wurde, wird mit einem Rahmen und durch Hinzufügen &quot;Hinweis:&quot; zuzuweisen. Das Hilfsprogramm ruft auch eine JavaScript-Datei, die den Hinweis Laufzeitverhalten hinzufügt. Anstatt das Skript mit Verweisen auf eine `<script>` -Tag, das Hilfsprogramm registriert das Skript durch Aufrufen von `Assets.AddScript` .
- Eine JavaScript-Datei. Dies ist die Datei, die vom Hilfsprogramm aufgerufen wird, und vorübergehend erhöht den Schriftgrad des Elemente Beachten Sie, während eine `mouseover` Ereignis.
- Eine Inhaltsseite, die verweist auf die<em>\_SiteLayout</em> Seite einige Inhalte in Text gerendert, und ruft dann die `MakeNote` Helper.
- Ein  *\_SiteLayout* Seite. Diese Seite enthält ein allgemeiner Header- und eine Seite Layout-Struktur. Es enthält auch einen Aufruf von `Assets.GetScripts`, d.h. wie Skripts auf der Ressourcen-Manager gerendert wird auf einer Seite aufgerufen.

So führen Sie das Beispiel aus:

1. Erstellen Sie eine leere Web Pages 2-Website. Sie können die WebMatrix **leere Website** Vorlage dafür.
2. Erstellen Sie einen Ordner mit dem Namen *Skripts* am Standort.
3. In der *Skripts* Ordner, erstellen Sie eine Datei mit dem Namen *Test.js*, kopieren die *Test.js* Inhalt hinein, aus dem Beispiel, und speichern Sie die Datei...
4. Erstellen Sie einen Ordner mit dem Namen *App\_Code* am Standort.
5. In der *App\_Code* Ordner eine Datei namens *Helpers.cshtml*, kopieren Sie den Beispielcode hinein, und speichern Sie ihn in einen Ordner namens *App\_Code*im Stammordner.
6. Erstellen Sie im Stammordner der Website, eine Datei namens  *\_SiteLayout.cshtml,* kopieren Sie das Beispiel hinein, und speichern Sie die Datei.
7. Erstellen Sie im Stammverzeichnis Website, eine Datei namens *ContentPage.cshtml*, fügen Sie den Beispielcode, und speichern Sie sie.
8. Führen Sie *ContentPage* in einem Browser. Die Zeichenfolge, die Sie an der `MakeNote` Helper als geschachteltes sich gerendert wird.
9. Übergeben Sie den Mauszeiger über die Notiz ein. Das Skript wird vorübergehend erhöht den Schriftgrad des Hinweises.
10. Zeigen Sie die Quelle der gerenderten Seite. Aufgrund der Platzierung des Aufrufs von `Assets.GetScripts`, das gerenderte `<script>` Tag, das Aufrufe *Test.js* ist das letzte Element im Text der Seite.

*Test.js*

[!code-javascript[Main](top-features-in-web-pages-2/samples/sample8.js)]

*Helpers.cshtml*

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample9.cshtml)]

*\_SiteLayout.cshtml*

[!code-html[Main](top-features-in-web-pages-2/samples/sample10.html)]

*ContentPage.cshtml*

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample11.cshtml)]

Der folgende Screenshot zeigt *ContentPage.cshtml* in einem Browser aus, wenn Sie den Mauszeiger über den Hinweis halten:

[![topSeven-resmgr-1](top-features-in-web-pages-2/_static/image14.png)](top-features-in-web-pages-2/_static/image13.png)

<a id="oauthsetup"></a>
### <a name="enabling-logins-from-facebook-and-other-sites-using-oauth-and-openid"></a>Aktivieren Anmeldungen von Facebook und andere Standorte mithilfe von OAuth und OpenID

Web Pages 2 bietet verbesserte Optionen für Mitgliedschaft und Authentifizierung. Die wichtigste Verbesserung ist, dass es neue [OAuth](http://oauth.net/) und [OpenID](http://openid.net/) Anbieter. Verwenden diese Anbieter, können Sie Benutzern die Anmeldung an Ihrem Standort mit ihren vorhandenen Anmeldeinformationen aus Facebook, Twitter, Windows Live, Google und Yahoo lassen. Beispielsweise können um mit einer Facebook-Konto anzumelden, Benutzer nur ein Facebook-Symbol auswählen sie an die Facebook-Anmeldeseite umgeleitet, in dem sie ihre Benutzerinformationen eingeben. Sie können dann die Facebook-Anmeldung mit ihrem Konto auf der Website zuordnen. Eine ähnliche Verbesserung auf die Funktionen der Webseiten-Mitgliedschaft ist, dass Benutzer mehrere Anmeldungen (einschließlich Anmeldungen von Websites für soziale Netzwerke) zuordnen, können mit einem einzigen Konto auf Ihrer Website.

Diese Abbildung zeigt die Anmeldeseite von der **Starter Site** Vorlage, in dem ein Benutzer eine Facebook, Twitter und Windows Live-Symbol zum Aktivieren der Protokollierung mit einem externen Konto auswählen kann:

[![topSeven-oauth-1](top-features-in-web-pages-2/_static/image16.png)](top-features-in-web-pages-2/_static/image15.png)

Sie können OAuth und OpenID-Mitgliedschaft, die mit nur wenigen Codezeilen ermöglichen. Die Methoden und Eigenschaften können Sie arbeiten mit dem OAuth und OpenID-Anbieter sind in der `WebMatrix.Security.OAuthWebSecurity` Klasse.

Anstatt Code, um Anmeldungen von anderen Websites zu aktivieren, ein empfohlenes Verfahren zum Einstieg in die neuen Anbieter ist jedoch für die Verwendung der neuen **Starter Site** Vorlage, die mit WebMatrix 2 Beta enthalten ist. Die **Starter Site** Vorlage umfasst eine Infrastruktur der vollständigen, komplett mit einer Anmeldeseite eine Mitgliedschaftsdatenbank und der gesamte Code müssen Sie Benutzern die Anmeldung bei Ihrer Website, die mit lokalen Anmeldeinformationen oder die von einem anderen Standort kann .

#### <a name="how-to-enable-logins-using-the-oauth-and-openid-providers"></a>So aktivieren Sie Anmeldungen unter Verwendung des OAuth und OpenID-Anbietern

Dieser Abschnitt enthält ein Beispiel dafür, wie Benutzer, die von externen Websites (Facebook, Twitter, Windows Live-, Google- oder Yahoo) melden Sie sich informieren, an einem Standort auf der Grundlage der **Starter Site** Vorlage. Nach der Erstellung einer Startwebsite, führen Sie diese (Details folgen):

- Erstellen Sie für die Standorte, die einen OAuth-Anbieter (Facebook, Twitter und Windows Live) verwenden eine Anwendung auf der externen Website. Dadurch können Sie die Anwendungsschlüssel, die Sie benötigen, um das Feature "Anmeldung" für die Websites aufrufen. Für Standorte, die einen OpenID-Anbieter (Google, Yahoo) zu verwenden, müssen Sie keinen zum Erstellen einer Anwendung. Für alle diese Standorte müssen Sie ein Konto verfügen, um sich anmelden und entwickleranwendungen erstellen. 

    > [!NOTE]
    > Windows Live-Anwendungen akzeptieren nur eine live-URL für eine Arbeitswebsite, damit Sie eine lokale Website-URL für das Testen von Anmeldungen nicht verwenden können.
- Bearbeiten Sie einige Dateien auf Ihrer Website, um den entsprechenden Authentifizierungsanbieter anzugeben und um eine Anmeldung auf der Website zu übermitteln, die Sie verwenden möchten.

**Aktivieren von Google und Yahoo Anmeldungen**:

1. Bearbeiten Sie in Ihrer Website, die  *\_AppStart.cshtml* Seite aus, und fügen Sie die folgenden zwei Codezeilen in der Razor-Codeblock nach dem Aufruf von der `WebSecurity.InitializeDatabaseConnection` Methode. Dieser Code ermöglicht, sowohl die Google und Yahoo OpenID-Anbieter. 

    [!code-css[Main](top-features-in-web-pages-2/samples/sample12.css)]
2. In der *~/Account/Login.cshtml* Seite, entfernen Sie die Kommentare aus folgendem `<fieldset>` des Markupblocks am Ende der Seite. Um den Code zu entfernen, entfernen Sie die `@*` Zeichen, die vor und nach der `<fieldset>` Block. Die daraus resultierenden Befehlsblock Code sieht folgendermaßen aus:

    [!code-html[Main](top-features-in-web-pages-2/samples/sample13.html)]
3. Hinzufügen einer `<input>` -Element für den Anbieter Google oder Yahoo an die `<fieldset>` Gruppe der *~/Account/Login.cshtml* Seite. Die aktualisierte `<fieldset>` Gruppe mit `<input>` Elemente für sowohl Google und Yahoo sieht wie im folgenden Beispiel: 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample14.html)]
4. In der *~/Account/AssociateServiceAccount.cshtml* Seite, fügen Sie `<input>` Elemente für Google oder Yahoo, um die `<fieldset>` Gruppe am Ende der Datei. Sie können die gleichen kopieren `<input>` Elemente, die Sie gerade hinzugefügt der `<fieldset>` im Abschnitt der *~/Account/Login.cshtml* Seite. 

    Die *~/Account/AssociateServiceAccount.cshtml* Seite in der Vorlage Starter Site kann verwendet werden, wenn Sie eine Seite erstellen, auf dem Benutzer mehrere Anmeldungen von anderen Standorten mit einem einzigen Konto auf Ihrer Website zuordnen, möchten.

Sie können nun Google und Yahoo Anmeldungen testen.

1. Führen Sie die *default.cshtml* Seite Ihrer Website, und wählen Sie die **melden Sie sich bei** Schaltfläche.
2. Auf der *Anmeldung* auf der Seite die **verwenden Sie einen anderen Dienst anmelden** Abschnitt, und wählen Sie entweder die **Google** oder **Yahoo** Schaltfläche "Senden". Dieses Beispiel verwendet die Google-Anmeldung. 

    Die Webseite leitet die Anforderung an die Google-Anmeldeseite.

    [![topSeven-oauth-6](top-features-in-web-pages-2/_static/image18.png)](top-features-in-web-pages-2/_static/image17.png)
3. Geben Sie Anmeldeinformationen für ein vorhandenes Google-Konto ein.
4. Wenn Sie Google gefragt, ob Sie "localhost" Verwenden von Informationen aus dem Konto, klicken Sie auf zulassen möchten **zulassen**.

    Der Code verwendet das Google-Token zur Authentifizierung des Benutzers und gibt dann zurück zu dieser Seite auf Ihrer Website. Sie können ein neues Konto auf der Website so ordnen Sie die externe Anmeldung mit registrieren, oder auf dieser Seite können Benutzer ihre Google-Anmeldung mit einem vorhandenen Konto auf Ihrer Website zu verknüpfen.

    [![topSeven-oauth-5](top-features-in-web-pages-2/_static/image20.png)](top-features-in-web-pages-2/_static/image19.png)
5. Wählen Sie die **zuordnen** Schaltfläche. Gibt zurück, der Browser zur Startseite Ihrer Anwendung.

    [![topSeven-oauth-3](top-features-in-web-pages-2/_static/image22.png)](top-features-in-web-pages-2/_static/image21.png)

    [![topSeven-oauth-3](top-features-in-web-pages-2/_static/image24.png)](top-features-in-web-pages-2/_static/image23.png)

**So aktivieren Sie die Facebook-Logins**:

1. Wechseln Sie zu der [Facebook-Entwickler-Website](https://developers.facebook.com/apps) (Melden Sie sich, wenn Sie noch nicht angemeldet sind).
2. Wählen Sie die **Create New App** Schaltfläche aus, und klicken Sie dann führen Sie die aufforderungen zum Benennen und die neue Anwendung erstellen.
3. Im Abschnitt **wählen, wie Ihre app mit Facebook integrieren**, wählen Sie die **Website** Abschnitt.
4. Geben Sie die **Website-URL** Feld mit der URL Ihrer Website (z. B. [ `http://www.example.com` ](http://www.example.com)). Die **Domäne** Feld ist optional; Sie können dies verwenden, zur Authentifizierung für eine gesamte Domäne (z. B. *"example.com"*). 

    > [!NOTE]
    > Wenn Sie einen Standort, auf dem lokalen Computer mit einer URL ausgeführt werden wie `http://localhost:12345` (wobei die Anzahl ist eine lokale Portnummer), können Sie diesen Wert zum Hinzufügen der **Website-URL** Feld für das Testen Ihrer Website. Jedoch jedes Mal, wenn die Portnummer des lokalen standortänderungen, müssen Sie zum Aktualisieren der **Website-URL** Feld der Anwendung.
5. Wählen Sie die **Save Changes** Schaltfläche.
6. Wählen Sie die **Apps** Registerkarte erneut aus, und zeigen Sie dann auf die Startseite für Ihre Anwendung.
7. Kopieren der **App-ID** und **App-Geheimnis** Werte für Ihre Anwendung und fügen Sie sie in eine temporäre Textdatei. Sie werden diese Werte mit dem Facebook-Anbieter in Ihrem Websitecode übergeben.
8. Beenden Sie die Facebook-Entwickler-Website.

Nun können Sie Änderungen auf beiden Seiten auf Ihrer Website, damit Benutzer bei der Website mit ihren Facebook-Konten anmelden können.

1. Bearbeiten Sie in Ihrer Website, die  *\_AppStart.cshtml* Seite, und kommentieren Sie den Code für die Facebook-OAuth-Anbieter. Der auskommentierungen aufgehoben wurden Codeblock sieht folgendermaßen aus: 

    [!code-xml[Main](top-features-in-web-pages-2/samples/sample15.xml)]
2. Kopieren der **App-ID** Wert aus der Facebook-Anwendung als Wert für die `consumerKey` Parameter (innerhalb der Anführungszeichen).
3. Kopie **App-Geheimnis** Wert aus der Facebook-Anwendung als der `consumerSecret` Parameterwert.
4. Speichern und schließen Sie die Datei.
5. Bearbeiten der *~/Account/Login.cshtml* Seite, und entfernen Sie die Kommentare aus der `<fieldset>` Block am Ende der Seite. Um den Code zu entfernen, entfernen Sie die `@*` Zeichen, die vor und nach der `<fieldset>` Block. Der Codeblock mit Kommentaren entfernt sieht wie folgt: 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample16.html)]
6. Speichern und schließen Sie die Datei.

Jetzt können Sie die Facebook-Anmeldung testen.

1. Führen Sie die Website des *default.cshtml* Seite, und wählen Sie die **Anmeldung** Schaltfläche.
2. Auf der *Anmeldung* auf der Seite die **verwenden Sie einen anderen Dienst anmelden** Abschnitt der **Facebook** Symbol. 

    Die Webseite leitet die Anforderung an die Facebook-Anmeldeseite.

    [![topSeven-oauth-2](top-features-in-web-pages-2/_static/image26.png)](top-features-in-web-pages-2/_static/image25.png)
3. Melden Sie sich ein Facebook-Konto. 

    Der Code wird das Facebook-Token zur Authentifizierung verwendet und gibt dann zurück zu einer Seite in dem Sie Ihre Facebook-Anmeldung mit Ihrer Website zuordnen. Ihr Benutzername oder e-Mail-Adresse wird ausgefüllt, in der **-e-Mail** Feld im Formular.

    [![topSeven-oauth-5](top-features-in-web-pages-2/_static/image28.png)](top-features-in-web-pages-2/_static/image27.png)
4. Wählen Sie die **zuordnen** Schaltfläche. 

    Der Browser gibt, auf der Startseite, und Sie angemeldet sind.

    [![topSeven-oauth-3](top-features-in-web-pages-2/_static/image30.png)](top-features-in-web-pages-2/_static/image29.png)

**So aktivieren Sie die Twitter-Anmeldungen:** 

1. Navigieren Sie zu der [Twitter-Entwickler-Website](https://dev.twitter.com/).
2. Wählen Sie die **erstellen Sie eine App** verknüpfen, und melden Sie sich bei der Website.
3. Auf der **erstellen Sie eine Anwendung** bilden, füllen Sie die **Namen** und **Beschreibung** Felder.
4. In der **WebSite** Geben Sie die URL Ihrer Website (z. B. [ `http://www.example.com` ](http://www.example.com)). 

    > [!NOTE]
    > Wenn Sie Ihre Website lokal testen (über eine URL wie `http://localhost:12345`), ist die URL, Twitter nicht akzeptieren. Allerdings ist Sie möglicherweise die lokalen Loopback-IP-Adresse verwenden (z. B. `http://127.0.0.1:12345`). Dies vereinfacht das Testen der Anwendung lokal. Jedoch jedes Mal, wenn die Portnummer des lokalen Standorts geändert wird, Sie müssen beim Aktualisieren der **WebSite** Feld der Anwendung.
5. In der **Rückruf-URL** Geben Sie eine URL für die Seite Ihrer Website, die Benutzern, nach der Anmeldung bei Twitter zurückgegeben werden sollen. Geben Sie beispielsweise die gleiche URL, die Sie im eingegeben haben, um Benutzer zur Startseite der Starter Site zu senden (was deren Status angemeldeten erkannt wird), die **WebSite** Feld.
6. Akzeptieren Sie die Bedingungen, und wählen Sie die **Erstellen Ihrer Twitter-Anwendung** Schaltfläche.
7. Auf der **Meine Anwendungen** Startseite, wählen Sie die Anwendung, die Sie erstellt haben.
8. Auf der **Details** Registerkarte, scrollen Sie nach unten, und wählen Sie die **erstellen My Access Token** Schaltfläche.
9. Auf der **Details** Registerkarte, kopieren Sie die **Consumerschlüssel** und **Consumer Secret** Werte für Ihre Anwendung und fügen Sie sie in eine temporäre Textdatei. Sie müssen diese Werte mit dem Twitter-Anbieter in Ihrem Websitecode übergeben.
10. Die Twitter-Website zu beenden.

Nun können Sie Änderungen auf beiden Seiten auf Ihrer Website, damit Benutzer bei der Website mit ihren Twitter-Konten anmelden können.

1. Bearbeiten Sie in Ihrer Website, die  *\_AppStart.cshtml* Seite, und entfernen Sie den Code für den Twitter-OAuth-Anbieter. Der auskommentierungen aufgehoben wurden Codeblock sieht folgendermaßen aus: 

    [!code-csharp[Main](top-features-in-web-pages-2/samples/sample17.cs)]
2. Kopieren der **Consumerschlüssel** Wert aus der Twitter-Anwendung als Wert für die `consumerKey` Parameter (innerhalb der Anführungszeichen).
3. Kopieren der **Consumer Secret** Wert aus der Twitter-Anwendung als Wert für die `consumerSecret` Parameter.
4. Speichern und schließen Sie die Datei.
5. Bearbeiten der *~/Account/Login.cshtml* Seite, und entfernen Sie die Kommentare aus der `<fieldset>` Block am Ende der Seite. Um den Code zu entfernen, entfernen Sie die `@*` Zeichen, die vor und nach der `<fieldset>` Block. Der Codeblock mit Kommentaren entfernt sieht wie folgt: 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample18.html)]
6. Speichern und schließen Sie die Datei.

Jetzt können Sie die Twitter-Anmeldung testen.

1. Führen Sie die *default.cshtml* Seite Ihrer Website, und wählen Sie die **Anmeldung** Schaltfläche.
2. Auf der *Anmeldung* auf der Seite die **verwenden Sie einen anderen Dienst anmelden** Abschnitt der **Twitter** Symbol. 

    Die Webseite leitet die Anforderung an eine Twitter-Anmeldeseite für die Anwendung, die Sie erstellt haben.

    [![topSeven-oauth-4](top-features-in-web-pages-2/_static/image32.png)](top-features-in-web-pages-2/_static/image31.png)
3. Melden Sie sich ein Twitter-Konto.
4. Der Code verwendet das Twitter-Token zur Authentifizierung des Benutzers und wechselt zurück zu einer Seite, in dem Sie zuordnen können Ihre Anmeldung mit Ihrem Konto für die Website. Ihren Namen oder e-Mail-Adresse wird ausgefüllt, in der **-e-Mail** Feld im Formular.

    [![topSeven-oauth-5](top-features-in-web-pages-2/_static/image34.png)](top-features-in-web-pages-2/_static/image33.png)
5. Wählen Sie die **zuordnen** Schaltfläche. 

    Der Browser gibt, auf der Startseite, und Sie angemeldet sind.

    [![topSeven-oauth-3](top-features-in-web-pages-2/_static/image36.png)](top-features-in-web-pages-2/_static/image35.png)

<a id="maphelper"></a>
### <a name="adding-maps-using-the-maps-helper"></a>Hinzufügen von Zuordnungen mithilfe der Maps-Hilfe

Web Pages 2 enthält Erweiterungen, die ASP.NET Web Helpers Library, der ein Paket add-ins für eine Webseiten-Website. Einer davon ist eine Komponente von bereitgestellten der `Microsoft.Web.Helpers.Maps` Klasse. Sie können die `Maps` Klasse zum Generieren von Zuordnungen, die basierend auf einer Adresse oder auf einen Satz von Koordinaten für Längen- und Breitengrad. Die `Maps` -Klasse ermöglicht, die Sie direkt in beliebte Map-Engines wie Bing, Google, Yahoo und MapQuest aufrufen.

Für die Verwendung der neuen `Maps` Klasse in Ihrer Website, müssen Sie zuerst die Version 2 von der Web Helpers Library installieren. Wechseln Sie zu diesem Zweck zu den Anweisungen zum Installieren der derzeit veröffentlichten Version von der [ASP.NET Web Helpers Library](https://go.microsoft.com/fwlink/?LinkId=202889#webhelpers) und installieren Sie Version 2.

Die Schritte zum Hinzufügen der Zuordnung zu einer Seite sind identisch unabhängig davon, die welches die Map-Engines, die Sie aufrufen. Sie fügen einfach einen JavaScript-Dateiverweis auf Ihrer Seite "Zuordnung", und klicken Sie dann hinzufügen einen Aufruf, der gerendert wird die `<script>` tags auf der Seite. Klicken Sie dann auf der Zuordnungsseite rufen Sie die Map-Engine, die Sie verwenden möchten.

Das folgende Beispiel zeigt das Erstellen einer Seite, die eine Zuordnung, die basierend auf einer Adresse rendert und einer anderen Seite, die eine Zuordnung anhand von Längen- und Breitengrad Koordinaten rendert. Die Adresse Mapping-Beispiel verwendet, Google Maps und die Koordinaten-Zuordnung-Beispiel verwendet die Bing Maps. Beachten Sie die folgenden Elemente im Code ein:

- Der Aufruf von `Assets.AddScript` am oberen Rand die beiden Zuordnungsseiten. Diese Methode fügt einen Verweis auf die *Jquery-1.6.2.min.js* -Datei, die in enthalten ist die **Starter Site** Vorlage, die erforderlich ist, und die `Maps` Klasse.
- Der Aufruf der `Assets.GetScripts` -Methode in der Layoutdatei. Diese Methode rendert das `<script>` -Tag in die beiden Zuordnungsseiten.
- Der Aufruf der `@Maps.GetGoogleHtml` und `@Maps.GetBingHtml` Methoden auf den Zuordnungsseiten zur. Um eine Adresse zuzuordnen, müssen Sie eine Adresszeichenfolge übergeben. Um die Koordinaten zuordnet, müssen Sie die Längen- und Breitengrad übergeben Koordinaten. Für die Bing Maps-Engine, müssen Sie auch einen Schlüssel übergeben (die Sie kostenlos erhalten, durch die Registrierung auf die [Bing Maps-Entwickler-Website](https://www.microsoft.com/maps/developers/web.aspx)). Die Methoden für die anderen Module Karte funktionieren auf ähnliche Weise (`@Maps.GetYahooHtml`, `@Maps.GetMapQuestHtml`).

So erstellen Sie Zuordnungsseiten:

1. Erstellen Sie eine Website, die auf der Grundlage der **Starter Site** Vorlage.
2. Erstellen Sie eine Datei mit dem Namen *MapAddress.cshtml* im Stammverzeichnis der Website. Auf dieser Seite generiert eine Karte, die basierend auf einer Adresse, die an sie übergeben werden.
3. Kopieren Sie den folgenden Code in die Datei, die vorhandene Inhalte überschrieben. 

    [!code-cshtml[Main](top-features-in-web-pages-2/samples/sample19.cshtml)]
4. Erstellen Sie eine Datei mit dem Namen  *\_MapLayout.cshtml* im Stammverzeichnis der Website. Auf dieser Seite werden der Layoutseite für die beiden Zuordnungsseiten.
5. Kopieren Sie den folgenden Code in die Datei, die vorhandene Inhalte überschrieben. 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample20.html)]
6. Erstellen Sie eine Datei mit dem Namen *MapCoordinates.cshtml* im Stammverzeichnis der Website. Auf dieser Seite generiert eine Karte, die basierend auf einem Satz von Koordinaten, die an sie übergeben werden.
7. Kopieren Sie den folgenden Code in die Datei, die vorhandene Inhalte überschrieben. 

    [!code-cshtml[Main](top-features-in-web-pages-2/samples/sample21.cshtml)]

So testen Sie Ihre Zuordnungsseiten:

1. Führen Sie die Seite *MapAddress.cshtml* Datei.
2. Geben Sie eine vollständige Adresse-Zeichenfolge, einschließlich einer Straße und Hausnummer, Status oder Provinz und Postleitzahl, und wählen Sie dann die **Map It** Schaltfläche. Die Seite rendert eine Zuordnung von Google Maps: 

    [![topseven-maphelper-1](top-features-in-web-pages-2/_static/image38.png)](top-features-in-web-pages-2/_static/image37.png)
3. Suchen Sie einen Satz von Längen-und Breitengradkoordinaten für einen bestimmten Standort.
4. Führen Sie die Seite *MapCoordinates.cshtml*. Geben Sie die Koordinaten, und wählen Sie dann die **Map It** Schaltfläche. Die Seite rendert eine Zuordnung von Bing Maps: 

    [![topseven-maphelper-2](top-features-in-web-pages-2/_static/image40.png)](top-features-in-web-pages-2/_static/image39.png)

<a id="sidebyside"></a>
### <a name="running-web-pages-applications-side-by-side"></a>Web Pages-Anwendungen parallel ausführen

Web Pages 2 fügt die Möglichkeit, Anwendungen parallel auszuführen. Dadurch können Sie weiterhin neue Web Pages 2-Anwendungen erstellen, führen Sie Ihre Web Pages 1-Anwendungen und alle auf demselben Computer ausgeführt.

Hier sind einige Punkte zu beachten, wenn Sie die Web Pages 2 Beta mit WebMatrix installieren:

- Standardmäßig werden der vorhandene Web Pages-Anwendungen als Version 2-Anwendungen auf Ihrem Computer ausgeführt. (Die Assemblys für Version 2, die im GAC installiert werden und werden automatisch verwendet werden.)
- Wenn Sie einen Standort mithilfe von Web Pages-Version 1 (anstelle des Standardwerts, wie am vorherigen Punkt) ausführen möchten, können Sie den Standort zu diesem Zweck konfigurieren. Wenn Ihre Website noch keinem *"Web.config"* Datei im Stammverzeichnis der Website, eine neue erstellen, und kopieren Sie das folgende XML, die vorhandene Inhalte überschrieben. Wenn die Website bereits enthält eine *"Web.config"* hinzufügen. ein `<appSettings>` Element wie den folgenden Ausdruck ein, um die `<configuration>` Abschnitt.

    [!code-xml[Main](top-features-in-web-pages-2/samples/sample22.xml)]
  "– Wenn Sie eine Version in nicht angeben der *" Web.config "* -Datei, einen Standort als Standort der Version 2 bereitgestellt wird. (Die Assemblys der Version 2 kopiert werden, um die *Bin* Ordner auf der bereitgestellten Website.)
- Neue Anwendungen, die mit der Websitevorlagen im Web Matrix Version 2 Beta schließen Sie die Web Pages-Version 2-Assemblys in der Website erstellt *Bin* Ordner.

Im Allgemeinen, Sie können immer steuern, welche Version von Webseiten mit Ihrer Website verwendet werden soll, mithilfe von NuGet die entsprechenden Assemblys in der Website installieren *Bin* Ordner. Um Pakete zu finden, besuchen Sie [NuGet.org](http://NuGet.org).

<a id="mobile"></a>
### <a name="rendering-pages-for-mobile-devices"></a>Rendern von Seiten für Mobile Geräte

Web Pages 2 können Sie benutzerdefinierte Ansichten zum Rendern von Inhalt auf Mobilgeräten und anderen Geräten zu erstellen.

Die `System.Web.WebPages` -Namespace enthält die folgenden Klassen, mit denen Sie arbeiten mit den Anzeigemodi: `DefaultDisplayMode`, `DisplayInfo`, und `DisplayModes`. Sie können diese Klassen direkt verwenden und Code schreiben, der die Rechte Ausgabe für bestimmte Geräte gerendert wird.

Alternativ können Sie gerätespezifische Seiten erstellen, mithilfe eines Musters Dateibenennung wie folgt: <em>FileName.</em> <em>Mobile</em><em>.cshtml</em>. Sie können z. B. zwei Versionen einer Seite, einen benannten erstellen <em>MyFile.cshtml</em> und eine mit dem Namen <em>MyFile.Mobile.cshtml</em>. Zur Laufzeit, wenn ein mobiles Gerät fordert <em>MyFile.cshtml</em>, Webseiten rendert den Inhalt von <em>MyFile.Mobile.cshtml</em>. Andernfalls <em>MyFile.cshtml</em> gerendert wird.

Das folgende Beispiel zeigt, wie mobile renderingaufträge durch Hinzufügen einer Inhaltsseite für mobile Geräte. *Page1.cshtml* enthält Inhalte sowie eine navigationsrandleiste. *Page1.Mobile.cshtml* den gleichen Inhalt enthält, lässt aber die Randleiste.

Erstellen und Ausführen des Codebeispiels:

1. Erstellen Sie in eine Web Pages-Website, eine Datei namens *Page1.cshtml* , und kopieren Sie die *Page1.cshtml* Inhalt hinein, aus dem Beispiel.
2. Erstellen Sie eine Datei mit dem Namen *Page1.Mobile.cshtml* , und kopieren Sie die *Page1.Mobile.cshtml* Inhalt hinein, aus dem Beispiel. Beachten Sie, dass es sich bei die mobile Version der Seite der Navigationsbereich für eine bessere Rendern auf einer kleineren Bildschirm lässt.
3. Führen Sie einen desktop-Browser und navigieren Sie zu *Page1.cshtml*.
4. Führen Sie einen mobilen Browser (oder einen Emulator für mobile Geräte), und navigieren Sie zu *Page1.cshtml*. Beachten Sie, dass es sich bei, dieses Mal Webseiten die mobile Version der Seite rendert. 

    > [!NOTE]
    > Um mobiler Seiten zu testen, können Sie einen Simulator für mobile Geräte, der auf einem Desktopcomputer ausgeführt wird. Mit diesem Tool können Sie Webseiten zu testen, wie sie auf mobilen Geräten aussehen würde (d. h. in der Regel eine viel kleinere anzeigen mit Bereich). Ein Beispiel für einen Simulator ist die [Benutzer-Agent Switcher-Add-On](http://addons.mozilla.org/en-us/firefox/addon/user-agent-switcher/) für Mozilla Firefox, sodass Sie verschiedenen mobilen Browsern aus einer desktop-Version von Firefox zu emulieren.

*Page1.cshtml*

[!code-html[Main](top-features-in-web-pages-2/samples/sample23.html)]

*Page1.Mobile.cshtml*

[!code-html[Main](top-features-in-web-pages-2/samples/sample24.html)]

*Page1.cshtml* in einem Desktopbrowser gerendert:

[![topseven-displaymodes-1](top-features-in-web-pages-2/_static/image42.png)](top-features-in-web-pages-2/_static/image41.png)

*Page1.Mobile.cshtml* in einer Ansicht Apple iPhone-Simulator, im Firefox-Browser angezeigt. Obwohl die Anforderung ist *Page1.cshtml*, die Anwendung rendert *Page1.Mobile.cshtml*.

[![topseven-displaymodes-2](top-features-in-web-pages-2/_static/image44.png)](top-features-in-web-pages-2/_static/image43.png)

<a id="resources"></a>
## <a name="additional-resources"></a>Zusätzliche Ressourcen

### <a name="aspnet-web-pages-1-resources"></a>ASP.NET Web Pages 1 Ressourcen

> [!NOTE]
> Es gelten weiterhin die meisten Web Pages 1-Programmierung und API-Ressourcen in Web Pages 2.

- [Einführung zu ASP.NET Web Pages-Programmierung](https://go.microsoft.com/fwlink/?LinkId=202890)

### <a name="webmatrix-resources"></a>WebMatrix-Ressourcen

- [WebMatrix 2 Neuigkeiten](http://webmatrix.com/next)
- [Microsoft WebMatrix Site](https://go.microsoft.com/fwlink/?LinkID=195076)
- [Starten der Webentwicklung mit Microsoft WebMatrix](https://msdn.microsoft.com/en-us/library/hh145669(v=VS.99).aspx)(einschließlich eine schwertlilienmodells Webseiten-beispielanwendung)
