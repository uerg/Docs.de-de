---
uid: web-pages/overview/releases/top-features-in-web-pages-2
title: Die wichtigsten Funktionen in ASP.NET Web Pages 2 | Microsoft Docs
author: microsoft
description: Dieses Thema bietet eine Übersicht über die wichtigsten neuen Funktionen in ASP.NET Web Pages 2, ein Programmierframework für einfache Web, die mit der WebMatr ist...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/13/2012
ms.topic: article
ms.assetid: cc712e72-c3d0-4e43-bc2d-28cc09cd8f71
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/releases/top-features-in-web-pages-2
msc.type: authoredcontent
ms.openlocfilehash: f0d32edd3ab54c55aa06c803cd91e01cbbb8f08a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30899381"
---
<a name="the-top-features-in-aspnet-web-pages-2"></a>Die wichtigsten Funktionen in ASP.NET Web Pages 2
====================
durch [Microsoft](https://github.com/microsoft)

> Dieser Artikel bietet einen Überblick über die wichtigsten neuen Funktionen in ASP.NET Web Pages 2 RC, ein Programmierframework für einfache Web die enthaltene [Microsoft WebMatrix 2 RC](https://www.microsoft.com/web/).
> 
> **Inhalt:** 
> 
> - [Installieren von WebMatrix](#install)
> - [Neue und verbesserte Funktionen](#New_and_Enhanced_Features)
> 
>     - [Änderungen für die RC-Version](#Changes_for_the_RC_Version)
>     - [Änderungen für die Betaversion](#Changes_for_the_Beta_Version)
>     - [Mithilfe der Vorlagen für neue und aktualisierte Standort](#templates)
>     - [Validieren von Benutzereingaben](#validation)
>     - [Anmeldungen von Facebook und andere Standorte mithilfe von OAuth und OpenID aktivieren](#oauthsetup)
>     - [Hinzufügen von Zuordnungen mithilfe der Maps-Hilfsmethode](#maphelper)
>     - [Ausführen von Webseiten Anwendungen nebeneinander angezeigt werden](#sidebyside)
>     - [Rendern von Seiten für Mobile Geräte](#mobile)
> - [Additional Resources](#resources) (Zusätzliche MSBuild-Ressourcen)
> 
> > [!NOTE]
> > In diesem Thema wird davon ausgegangen, dass Sie WebMatrix verwenden, mit der ASP.NET Web Pages 2-Code arbeiten. Allerdings als mit Web Pages-1, Sie auch Web Pages 2-Websites, die mit Visual Studio erstellen können, verbesserte wo Sie IntelliSense-Funktionen und Debuggen. Um mit Webseiten in Visual Studio arbeiten, müssen Sie zuerst Visual Studio 2010 SP1, Visual Web Developer Express 2010 SP1 oder Visual Studio 11 Beta installieren. Installieren Sie ASP.NET MVC 4 Beta, darunter Vorlagen und Tools zum Erstellen von ASP.NET MVC 4 und Web Pages 2-Anwendungen in Visual Studio.
> 
> 
> *Letzte Aktualisierung: 18 Juni 2012*


<a id="install"></a>
## <a name="installing-webmatrix"></a>Installieren von WebMatrix

Um Webseiten zu installieren, können Sie Microsoft Web Platform Installer, dies ist eine kostenlose Anwendung, die zum Installieren und Konfigurieren von Web-bezogene Technologien erleichtert. Installieren Sie die WebMatrix 2 Beta, die Web Pages 2 Beta umfasst.

1. Navigieren Sie zu die Installationsseite für die neueste Version von Webplattform-Installer:

    [https://go.microsoft.com/fwlink/?LinkId=226883](https://go.microsoft.com/fwlink/?LinkId=226883)

    > [!NOTE]
    > Wenn Sie bereits WebMatrix 1 installiert haben, von dieser Installation zu WebMatrix 2 Beta aktualisiert. Sie können Websites ausführen, die mit Version 1 oder 2 auf demselben Computer erstellt wurden. Weitere Informationen finden Sie im Abschnitt auf [Ausführung Web Pages-Anwendungen parallel dazu](#sidebyside).
2. Wählen Sie **jetzt installieren**. 

    Wenn Sie Internet Explorer verwenden, fahren Sie mit dem nächsten Schritt fort. Wenn Sie einen anderen Browser wie Mozilla Firefox oder Google Chrome verwenden, werden Sie aufgefordert, speichern die *Webmatrix.exe* Datei auf Ihrem Computer. Speichern Sie die Datei, und klicken Sie dann auf, um das Installationsprogramm zu starten.
3. Führen Sie das Installationsprogramm, und wählen Sie die **installieren** Schaltfläche. Hiermit werden installiert WebMatrix- und Web Pages.

## <a id="New_and_Enhanced_Features"></a>  Neue und verbesserte Funktionen

### <a id="Changes_for_the_RC_Version"></a>  Änderungen für die RC-Version (Juni 2012)

Die RC-Version im Juni 2012 verfügt über ein paar Änderungen über die Beta-Version aktualisieren, die im März 2012 veröffentlicht wurde. Diese Änderungen sind:

- Ein `Validation.AddFormError` Methode wurde hinzugefügt, um die `Validation` Helper. Dies ist hilfreich, wenn Sie die Überprüfung manuell durchführen (z. B. Sie validieren einen Wert, der in der Abfragezeichenfolge übergeben wird) und eine Fehlermeldung angezeigt, die angezeigt werden können, hinzugefügt werden soll die `Html.ValidationSummary` Methode. Weitere Informationen finden Sie im Abschnitt [überprüft Daten, ist nicht stammen direkt vom Benutzer](https://go.microsoft.com/fwlink/?LinkId=253002#Validating_Data_That_Doesnt_Come_Directly_from_Users) in [Validieren von Benutzereingaben in ASP.NET Web Pages (Razor)-Websites](https://go.microsoft.com/fwlink/?LinkId=253002).
- Die Funktionalität für Bündelung und Minimierung wurde von Kernassemblys ASP.NET Web Pages 2 entfernt. Daher die `Assets` weiter unten in diesem Dokument ist nicht verfügbar. Stattdessen müssen Sie installieren die [ASP.NET-Optimierung](http://nuget.org/packages/Microsoft.Web.Optimization/0.1) NuGet-Paket. Weitere Informationen finden Sie unter [Bündelung und Minimierung mit Ressourcen in einer ASP.NET Web Pages (Razor) Standort](https://go.microsoft.com/fwlink/?LinkId=255373).
- Zusätzliche Assemblys zur Unterstützung von ASP.NET Web Pages 2 wurden hinzugefügt. Die nur merkliche Auswirkungen dieser Änderung ist, dass Sie möglicherweise mehrere Assemblys an eines Standorts angezeigt *"bin"* Ordner nach dem Erstellen einer Website oder einen Standort mit einem Hostinganbieter bereitstellen.

<a id="Changes_for_the_Beta_Version"></a>
### <a name="changes-for-the-beta-version-february-2012"></a>Änderungen für die Beta-Version (Version von Februar 2012)

Die Beta-Version, die im Februar 2012 veröffentlicht wurde, nur wenige Änderungen über die Beta-Version, die im Dezember 2011 veröffentlicht wurde. Diese Änderungen sind:

- Razor unterstützt jetzt bedingte Attribute. In einem HTML-Element, wenn Sie ein Attribut, das auf einen Wert festgelegt, die in Servercode zu aufgelöst `false` oder `null`, ASP.NET rendert nicht das Attribut an. Angenommen Sie, dass Sie das folgende Markup für ein Kontrollkästchen haben:

    [!code-html[Main](top-features-in-web-pages-2/samples/sample1.html)]

    Wenn der Wert der `checked1` löst in `false` oder `null`, die `checked` Attribut wird nicht gerendert. Dies ist eine unterbrechende Änderung.
- Die `Validation.GetHtml` Methode wurde umbenannt in `Validation.For`. Dies ist eine unterbrechende Änderung. `Validation.GetHtml` funktionieren nicht in der Betaversion.
- Sie können jetzt enthalten die `~` Operator im Markup zum Stammverzeichnis der Website ohne verweisen die `Href` Funktion. (D. h. der Razor-Parser kann jetzt suchen und Beheben der `~` Operator ohne einen expliziten Methodenaufruf zu `Href`.) Die `Href` Methode weiterhin funktioniert, damit es sich nicht um eine unterbrechende Änderung handelt.

    Wenn beispielsweise zuvor verfügbaren Markup wie folgt:

    `<a href="@Href("~/Default.cshtml")">Home</a>`

    Sie können jetzt Markup wie folgt verwenden:

    `<a href="~/Default.cshtml">Home</a>`
- Die `Scripts` Helfer für die Verwaltung von Ressourcen (Ressource) wurde durch ersetzt die `Assets` Helper, verfügt über etwas andere Methoden, z. B. die folgenden:

  - Für `Scripts.Add`, verwenden `Assets.AddScript`
  - Für `Scripts.GetScriptTags`, verwenden `Assets.GetScripts`

    Dies ist eine unterbrechende Änderung. die `Scripts` Klasse ist nicht in der Betaversion verfügbar. Die Codebeispiele in diesem Dokument, mit denen Asset Management wurden mit dieser Änderung aktualisiert.

<a id="templates"></a>
### <a name="using-the-new-and-updated-site-templates"></a>Mithilfe der Vorlagen für neue und aktualisierte Standort

Die **Starter Site** Vorlage wurde aktualisiert, damit sie auf die Web Pages 2 ausgeführt wird standardmäßig. Es umfasst außerdem die folgenden neuen Funktionen:

- Mobilgerätefreundliche Seite rendern. Durch die Verwendung von CSS-Formatvorlagen und der `@media` -Auswahl, die **Starter Site** bietet verbesserte Rendern von Seiten auf kleineren Bildschirmen, einschließlich mobiler Geräte Bildschirme.
- Verbesserte Optionen für Mitgliedschaft und Authentifizierung. Sie können sich Benutzer in Ihrer Website mithilfe ihrer Konten von anderen social Network-Sites, z. B. Twitter, Facebook und Windows Live lassen. Weitere Informationen finden Sie unter der [Anmeldungen aktivieren, Facebook und andere Websites, die mithilfe von OAuth und OpenID](#oauthsetup) Abschnitt.
- HTML5-Elemente.

Die neue **persönliche Website** Vorlage können Sie eine Website zu erstellen, die einen persönlichen Blog, eine Seite Fotos und Twitter-Seite enthält. Sie können anpassen, einen Standort auf der Grundlage der **persönliche Website** Vorlage mithilfe der folgenden Schritte:

- Ändern des Aussehens von der Website durch Bearbeiten der Layoutdatei (*\_SiteLayout.cshtml*) und die Stile-Datei (*"Site.CSS" ändern*).
- Installieren von NuGet-Pakete, die Funktionalität Ihrer Website hinzufügen. Informationen zum Installieren der Pakete, einschließlich der ASP.NET Web Helpers Library finden Sie im Lernprogramm zu [installieren Hilfsprogramme](https://go.microsoft.com/fwlink/?LinkId=202889#webhelpers).

Für den Zugriff auf die **persönliche Website** Vorlage, wählen Sie **Vorlagen** auf die WebMatrix **Schnellstart** Bildschirm.

[![topseven-personalsite-1](top-features-in-web-pages-2/_static/image2.png)](top-features-in-web-pages-2/_static/image1.png)

In der **Vorlagen** Dialogfeld Wählen Sie die **persönliche Website** Vorlage.

[![topseven-personalsite-2](top-features-in-web-pages-2/_static/image4.png)](top-features-in-web-pages-2/_static/image3.png)

Die Zielseite der der **persönliche Website** Vorlage können Sie die Links zum Einrichten Ihrer Blog verwenden, führen Sie die Twitter-Seite und Fotos Seite.

[![topseven-personalsite-3](top-features-in-web-pages-2/_static/image6.png)](top-features-in-web-pages-2/_static/image5.png)

<a id="validation"></a>
### <a name="validating-user-input"></a>Validieren von Benutzereingaben

In Web Pages-1, zum Überprüfen von Benutzereingaben für übermittelte Formulare, verwenden Sie die `System.Web.WebPages.Html.ModelState` Klasse. (Dies wird in einige Codebeispiele veranschaulicht, in der Web Pages-1-Lernprogramm, das mit dem Titel [arbeiten mit Daten](../data/5-working-with-data.md).) Sie können diesen Ansatz in Web Pages 2 weiterhin verwenden. Web Pages 2 bietet jedoch außerdem eine verbesserte Tools zum Überprüfen von Benutzereingaben:

- Neue Überprüfung-Klassen, einschließlich `System.Web.WebPages.ValidationHelper` und `System.Web.WebPages.Validator`, mit denen Sie die leistungsstarken Überprüfung Aufgaben mit wenigen Codezeilen ausführen können.
- Optional, clientseitige Validierung, die Validierungsfehler unmittelbar Feedback zu der zu überprüfende Benutzer anstatt von einem Roundtrip zum Server bereit. (Aus Gründen der Sicherheit wird Validierung auf dem Server ausgeführt, selbst wenn die Überprüfungen in der Client zuvor ausgeführt wurden.)

Um die neuen Validierungsfunktionen verwenden möchten, führen Sie folgende Schritte aus:

In der Seitencode registrieren ein Elements, das überprüft werden, mithilfe der Methoden der `Validation` Helper: `Validation.RequireField`, `Validation.RequireFields` (um mehrere Elemente als erforderlich registrieren zu können), oder `Validation.Add`. Die `Add` Methode können Sie andere Arten von Überprüfungen, z. B. Datentyp überprüfen, Vergleichen von Einträgen in verschiedenen Bereichen, String-length-Funktion überprüft, angeben und Nachrichtenaustauschmuster (mit regulären Ausdrücken). Hier einige Beispiele:

[!code-html[Main](top-features-in-web-pages-2/samples/sample2.html)]

Rufen Sie zum Anzeigen eines Fehlers feldspezifischen `Html.ValidationMessage` im Markup für jedes Element, das überprüft wird:

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample3.cshtml)]

Eine Zusammenfassung anzeigen (`<ul>` Liste) aller Fehler, die sich auf der Seite `Html.ValidationSummary` im Markup:

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample4.cshtml)]

Diese Schritte sind ausreichend, um die serverseitige Validierung zu implementieren. Wenn Sie die clientseitige Validierung hinzufügen möchten, gehen Sie darüber hinaus.

Fügen Sie die Datei die folgenden Skriptverweise innerhalb der `<head>` Abschnitt einer Webseite. Die ersten beiden Skriptverweise zeigen auf remote-Dateien auf einem Content Delivery Network (CDN)-Server. Die dritte Verweis zeigt auf eine lokale Skriptdatei. Produktions-apps müssen als Fallback implementieren, wenn das CDN nicht verfügbar ist. Das Fallback zu testen.

[!code-html[Main](top-features-in-web-pages-2/samples/sample5.html)]

Die einfachste Möglichkeit, erhalten eine lokale Kopie der *jquery.validate.unobtrusive.min.js* Bibliothek zum Erstellen einer neuen Web Pages-Website, die basierend auf einer Site-Vorlagen (z. B. Starter Site) ist. Die Website, die von der Vorlage erstellten stellt *jquery.validate.unobtrusive.js* -Datei in die Ordner "Skripts", von dem Sie sie auf Ihrer Website kopieren können.

Wenn Ihre Website verwendet eine<em>\_SiteLayout</em> zum Steuern des Seitenlayouts Seite, Sie können diese Skriptverweise in dieser Seite einschließen, damit die Überprüfung für alle Inhaltsseiten verfügbar ist. Wenn Sie die Überprüfung nur auf bestimmte Seiten ausführen möchten, können Sie den Ressourcen-Manager, um die Skripts auf nur die Seiten zu registrieren. Zu diesem Zweck rufen `Assets.AddScript(path)` auf der Seite, die Sie überprüfen möchten und jede der Skriptdateien verweisen. Fügen Sie dann auf einen Aufruf von `Assets.GetScripts` in der  <em>\_SiteLayout</em> Seite, um den registrierten Rendern `<script>` Tags. Weitere Informationen finden Sie im Abschnitt [Registrieren von Skripts mit dem Ressourcen-Manager](#resmanagement).

Rufen Sie in das Markup für ein einzelnes Element, das `Validation.For` Methode. Diese Methode gibt Attribute jQuery kann verknüpfen, um die clientseitige Validierung bereitzustellen. Zum Beispiel:

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample6.cshtml)]

Das folgende Beispiel zeigt eine Seite, die Benutzereingaben in einem Formular überprüft. Zum Ausführen und Testen Sie den Validierungscode, tun Sie Folgendes aus:

1. Erstellen einer neuen Website mithilfe einer der WebMatrix 2-Vorlagen, die enthält eine *Skripts* Ordner, z. B. die **Starter Site** Vorlage.
2. In den neuen Standort erstellen Sie ein neues *cshtml* Seite, und Ersetzen Sie den Inhalt der Seite mit den folgenden Code.
3. Führen Sie die Seite in einem Browser aus. Geben Sie gültige und ungültige Werte, um die Auswirkungen auf die Validierung finden Sie unter. Z. B. ein erforderliches Feld leer lassen, oder geben Sie einen Buchstaben in die **Gutschriften** Feld.


[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample7.cshtml)]

Hier ist die Seite auf, wenn ein Benutzer mit gültigen Eingaben übermittelt:

[![topSeven-valid-1](top-features-in-web-pages-2/_static/image8.png)](top-features-in-web-pages-2/_static/image7.png)

Hier ist die Seite auf, wenn ein Benutzer, die sie ein erforderliches Feld leer gelassen übermittelt:

[![topSeven-valid-2](top-features-in-web-pages-2/_static/image10.png)](top-features-in-web-pages-2/_static/image9.png)

Hier ist die Seite, wenn ein Benutzer mit einem anderen Knotentyp als eine ganze Zahl im übermittelt die **Gutschriften** Feld:

[![topSeven-valid-3](top-features-in-web-pages-2/_static/image12.png)](top-features-in-web-pages-2/_static/image11.png)

Weitere Informationen finden Sie unter den folgenden Blogbeiträgen:

- [Aktualisiert die Validierung in Webseiten v2](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2344) Grundlagen zum Hinzufügen von Validierung mithilfe der `Validation` Helper (nur Server-Seite)
- [Aktualisiert die Validierung in Webseiten v2, Teil 2](http://www.mikepope.com/blog/DisplayBlog.aspx?permalink=2347) clientseitige Validierung hinzufügen.
- [Aktualisiert die Validierung in Webseiten v2, Teil 3](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2351) Formatierung Validierungsfehler.

<a id="resmanagement"></a>
### <a name="registering-scripts-using-the-assets-manager"></a>Registrieren von Skripts, die mit dem Ressourcen-Manager

Der Ressourcen-Manager ist eine neue Funktion, die Sie verwenden können, im Servercode zum Registrieren und Rendern von Clientskripts. Diese Funktion ist hilfreich, wenn Sie mit Code von mehreren Dateien (z. B. Layoutseiten, Inhaltsseiten, Hilfsprogramme, usw.) arbeiten, die in einer einzelnen Seite zur Laufzeit kombiniert werden. Der Ressourcen-Manager koordiniert, die Quelldateien, um sicherzustellen, dass Skriptdateien auf die verwiesen ordnungsgemäß wird sind und effizient auf der gerenderten Seite unabhängig davon, welche Codedateien, die sie aus aufgerufen werden oder wie oft werden sie aufgerufen wird. Der Ressourcen-Manager auch rendert `<script>` tags am richtigen Ort, damit die Seite schnell (ohne Herunterladen von Skripts während des Renderns) zu laden und Fehler zu vermeiden, die auftreten können kann, wenn Skripts vor dem Rendern aufgerufen werden abgeschlossen ist.

Nehmen wir beispielsweise an, dass Sie einen benutzerdefinierten Hilfsmethoden erstellen, die eine JavaScript-Datei aufruft, und Sie dieses Hilfsprogramm an drei verschiedenen Stellen im Code Inhaltsseite aufrufen. Wenn Sie nicht die Ressourcen-Manager verwenden, registrieren Ruft das Skript in das Hilfsprogramm, drei verschiedene `<script>` Transpondern, die alle, zeigen Sie auf die Skriptdatei in der gerenderten Seite angezeigt wird. Plus, je nachdem, wo die `<script>` Tags in der gerenderten Seite eingefügt werden, können Fehler auftreten, wenn das Skript versucht, auf bestimmte Elemente zugreifen, bevor die Seite vollständig geladen. Wenn Sie den Ressourcen-Manager verwenden, um das Skript zu registrieren, vermeiden Sie diese Probleme.

Auf diese Weise können Sie ein Skript mit dem Ressourcen-Manager registrieren:

- Rufen Sie im Code, der Verweis auf das Skript muss die `Assets.AddScript` Methode.
- In einem  *\_SiteLayout* Seite, rufen Sie die `Assets.GetScripts` Methode zum Rendern der `<script>` Tags. 

    > [!NOTE]
    > Aufrufe von PUT `Assets.GetScripts` als das letzte Element in der `<body>` Element von der  *\_SiteLayout* Seite. Dadurch wird die Seite schneller geladen und kann dabei helfen, die Skriptfehler zu vermeiden.

Das folgende Beispiel zeigt die Funktionsweise des Ressourcen-Managers. Der Code enthält die folgenden Elemente:

- Einen benutzerdefinierten Hilfsmethoden, die mit dem Namen `MakeNote`. Dieses Hilfsprogramm wrapping rendert eine Zeichenfolge in einem Feld eine `div` Element herum, formatiert wurde, mit einem Rahmen und durch Hinzufügen von &quot;Hinweis:&quot; darauf. Das Hilfsprogramm ruft außerdem eine JavaScript-Datei, die den Hinweis Laufzeitverhalten hinzufügt. Anstatt das Skript mit Verweisen eine `<script>` Tag, das Hilfsprogramm registriert das Skript durch Aufrufen von `Assets.AddScript` .
- Eine JavaScript-Datei. Dies ist die Datei, die durch das Hilfsprogramm aufgerufen wird, und vorübergehend erhöht den Schriftgrad des Elemente Beachten Sie, während gleichzeitig eine `mouseover` Ereignis.
- Eine Inhaltsseite, verweist auf die<em>\_SiteLayout</em> Seite rendert Manche Inhalte in den Text, und ruft dann die `MakeNote` Helper.
- Ein  *\_SiteLayout* Seite. Diese Seite enthält eine allgemeine Header und ein Layout Seitenstruktur. Es enthält auch einen Aufruf an `Assets.GetScripts`, das Rendering von Skript in des Ressourcen-Managers ist auf einer Seite aufruft.

Um das Beispiel auszuführen:

1. Erstellen Sie eine leere Web Pages 2-Website. Sie können die WebMatrix **leere Website** für diese Vorlage.
2. Erstellen Sie einen Ordner mit dem Namen *Skripts* am Standort.
3. In der *Skripts* Ordner, erstellen Sie eine Datei mit dem Namen *Test.js*, kopieren die *Test.js* Inhalt hinein aus dem Beispiel, und speichern Sie die Datei...
4. Erstellen Sie einen Ordner mit dem Namen *App\_Code* am Standort.
5. In der *App\_Code* Ordner, erstellen Sie eine Datei mit dem Namen *Helpers.cshtml*, kopieren Sie den Beispielcode hinein und speichern Sie sie in einem Ordner namens *App\_Code*im Stammordner.
6. Erstellen Sie im Stammordner der Website, eine Datei namens  *\_SiteLayout.cshtml,* kopieren Sie das Beispiel hinein, und speichern Sie die Datei.
7. Erstellen Sie im Stammverzeichnis Website eine Datei namens *ContentPage.cshtml*, fügen Sie den Beispielcode, und speichern Sie sie.
8. Führen Sie *ContentPage verwendet* in einem Browser. Die Zeichenfolge, die Sie an der `MakeNote` Helper wird als geschachtelter Knoten gerendert.
9. Übergeben Sie den Mauszeiger über den Hinweis. Das Skript wird vorübergehend erhöht den Schriftgrad der Anmerkung.
10. Zeigen Sie die Quelle der gerenderten Seite. Aufgrund der Platzierung des Aufrufs von `Assets.GetScripts`, das gerenderte `<script>` -Tags, das Aufrufe *Test.js* ist das letzte Element im Text der Seite.

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
### <a name="enabling-logins-from-facebook-and-other-sites-using-oauth-and-openid"></a>Anmeldungen von Facebook und andere Standorte mithilfe von OAuth und OpenID aktivieren

Web Pages 2 bietet verbesserte Optionen für die Mitgliedschaft und Authentifizierung. Die Haupt-Erweiterung ist, dass es neue [OAuth](http://oauth.net/) und [OpenID](http://openid.net/) Anbieter. Diese Anbieter können Sie Benutzern die Anmeldung in Ihre Website mit ihren vorhandenen Anmeldeinformationen von Facebook, Twitter, Windows Live, Google und Yahoo lassen. Beispielsweise können zur Anmeldung mit Facebook-Konto Benutzer eine Facebook-Symbol, wählen Sie dem diese an die Facebook-Anmeldeseite weitergeleitet, wo er ihre Benutzerinformationen ein. Sie können dann die Facebook-Anmeldung mit ihrem Konto auf der Website zuordnen. Eine verwandte-Erweiterung für die Web Pages-Mitgliedschaft-Funktionen ist, dass Benutzer mehrere Anmeldungen (einschließlich Anmeldungen über social Network-Sites) zuordnen, können mit einem einzelnen Konto auf Ihrer Website.

Dieses Bild zeigt die Anmeldeseite von der **Starter Site** Vorlage, in dem ein Benutzer eine Facebook, Twitter oder Windows Live-Symbol zum Aktivieren der Protokollierung mit einem externen Konto auswählen kann:

[![topSeven-oauth-1](top-features-in-web-pages-2/_static/image16.png)](top-features-in-web-pages-2/_static/image15.png)

Sie können OAuth- und OpenID-Mitgliedschaft mit nur wenigen Codezeilen aktivieren. Die Methoden und Eigenschaften verwenden Sie zum Arbeiten mit dem OAuth und OpenID-Anbieter sind in der `WebMatrix.Security.OAuthWebSecurity` Klasse.

Anstatt Code, um Anmeldungen von anderen Standorten zu ermöglichen, eine empfohlene Vorgehensweise zum Einstieg in die neue Anbieter ist jedoch der neuen **Starter Site** Vorlage, die mit WebMatrix 2 Beta enthalten ist. Die **Starter Site** eine vollständige Mitgliedschaft-Infrastruktur umfasst die Vorlage, mit einer Anmeldeseite, eine Mitgliedschaftsdatenbank und der gesamte Code müssen Sie Benutzern die Anmeldung in Ihre Website mithilfe von lokalen Anmeldeinformationen oder die von einem anderen Standort zu ermöglichen .

#### <a name="how-to-enable-logins-using-the-oauth-and-openid-providers"></a>So aktivieren Sie Anmeldungen unter Verwendung des OAuth- und OpenID-Anbietern

Dieser Abschnitt enthält ein Beispiel dafür, wie können Benutzer, die von externen Standorten (Facebook, Twitter, Windows Live, Google und Yahoo) melden Sie sich mit einem Standort auf der Grundlage der **Starter Site** Vorlage. Nach dem Erstellen einer Startwebsite, führen Sie diese (Details folgen):

- Erstellen Sie für die Standorte, die einen OAuth-Anbieter (Facebook, Twitter und Windows Live) verwenden eine Anwendung auf der externen Website ein. Dies gibt Ihnen die Anwendungsschlüssel, die Sie benötigen, um die Login-Funktion für diese Standorte aufzurufen. Für Standorte, die einen OpenID-Anbieter (Google, Yahoo) verwenden, müssen Sie keine Anwendung zu erstellen. Für alle diese Standorte müssen Sie ein Konto verfügen, um anzumelden und entwickleranwendungen zu erstellen. 

    > [!NOTE]
    > Windows Live-Anwendungen akzeptieren nur eine live-URL für eine Website verwenden, damit Sie eine lokale Website-URL für das Testen von Benutzernamen verwenden können.
- Bearbeiten Sie einige Dateien in Ihrer Website, um den entsprechenden Authentifizierungsanbieter anzugeben und eine Anmeldung auf der Website übermitteln, die Sie verwenden möchten.

**So aktivieren Sie Google und Yahoo Anmeldungen**:

1. Bearbeiten Sie Ihre Website die  *\_AppStart.cshtml* Seite, und fügen Sie die folgenden zwei Codezeilen in der Razor-Codeblock, nach dem Aufruf von der `WebSecurity.InitializeDatabaseConnection` Methode. Dieser Code kann sowohl die Google und Yahoo OpenID-Anbieter. 

    [!code-css[Main](top-features-in-web-pages-2/samples/sample12.css)]
2. In der *~/Account/Login.cshtml* Seite, entfernen Sie die Kommentare auf der folgenden `<fieldset>` Block des Markups am Ende der Seite. Um den Code zu kommentieren, entfernen Sie die `@*` Zeichen, die vor und nach der `<fieldset>` Block. Der resultierende Codeblock sieht wie folgt:

    [!code-html[Main](top-features-in-web-pages-2/samples/sample13.html)]
3. Hinzufügen einer `<input>` -Element für den Google oder Yahoo-Anbieter auf die `<fieldset>` Gruppe in der *~/Account/Login.cshtml* Seite. Die aktualisierte `<fieldset>` Gruppe mit `<input>` Elemente für Google und Yahoo sieht wie im folgende Beispiel: 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample14.html)]
4. In der *~/Account/AssociateServiceAccount.cshtml* Seite, fügen Sie `<input>` Elemente für Google oder Yahoo, um die `<fieldset>` Gruppe am Ende der Datei. Kopieren Sie die gleiche `<input>` Elemente, die Sie gerade hinzugefügt haben, um die `<fieldset>` im Abschnitt der *~/Account/Login.cshtml* Seite. 

    Die *~/Account/AssociateServiceAccount.cshtml* Seite in der Vorlage Starter Site kann verwendet werden, wenn Sie eine Seite zu erstellen, auf dem Benutzer mehrere Anmeldungen von anderen Standorten mit einem einzelnen Konto auf Ihrer Website zuordnen, möchten.

Sie können jetzt Google und Yahoo Anmeldungen testen.

1. Führen Sie die *default.cshtml* Seite Ihrer Website, und wählen Sie die **melden Sie sich** Schaltfläche.
2. Auf der *Anmeldung* Seite in der **einen anderen Dienst zum Anmelden verwenden** Abschnitt, und wählen Sie entweder die **Google** oder **Yahoo** Schaltfläche zum Absenden. Dieses Beispiel verwendet die Google-Anmeldung. 

    Die Webseite leitet die Anforderung an der Google-Anmeldeseite.

    [![topSeven-oauth-6](top-features-in-web-pages-2/_static/image18.png)](top-features-in-web-pages-2/_static/image17.png)
3. Geben Sie Anmeldeinformationen für ein vorhandenes Google-Konto ein.
4. Wenn Google Sie fragt, ob Sie "localhost", klicken Sie zum Verwenden von Informationen aus dem Konto zulassen möchten **zulassen**.

    Der Code verwendet das Google-Token zur Authentifizierung des Benutzers, und klicken Sie dann auf Ihrer Website auf diese Seite gibt. Auf dieser Seite können Benutzer ihre Google-Anmeldung mit einem vorhandenen Konto auf Ihrer Website zu verknüpfen, oder sie können ein neues Konto an Ihrem Standort ordnen Sie die externe Anmeldung mit registrieren.

    [![topSeven-oauth-5](top-features-in-web-pages-2/_static/image20.png)](top-features-in-web-pages-2/_static/image19.png)
5. Wählen Sie die **zuordnen** Schaltfläche. Der Browser gibt zur Startseite der Anwendung.

    [![topSeven-oauth-3](top-features-in-web-pages-2/_static/image22.png)](top-features-in-web-pages-2/_static/image21.png)

    [![topSeven-oauth-3](top-features-in-web-pages-2/_static/image24.png)](top-features-in-web-pages-2/_static/image23.png)

**Facebook-Anmeldungen aktivieren**:

1. Wechseln Sie zu der [Facebook-Entwickler-Website](https://developers.facebook.com/apps) (Melden Sie sich, wenn Sie noch nicht angemeldet sind).
2. Wählen Sie die **neue App** Schaltfläche und befolgen Sie dann die Anweisungen, die neue Anwendung erstellen und benennen.
3. Im Abschnitt **wählen Sie die Integration Ihrer app mit Facebook**, wählen Sie die **Website** Abschnitt.
4. Füllen Sie die **Website-URL** Feld mit der URL Ihrer Website (z. B. [ `http://www.example.com` ](http://www.example.com)). Die **Domäne** Feld ist optional; Sie können Hiermit können Sie die Bereitstellung der Authentifizierung für eine gesamte Domäne (z. B. *example.com*). 

    > [!NOTE]
    > Wenn Sie einen Standort, auf dem lokalen Computer mit einer URL ausgeführt werden wie `http://localhost:12345` (wobei die Anzahl ist eine lokale Portnummer), können Sie diesen Wert zum Hinzufügen der **Website-URL** bei Testen Ihrer Website im Feld. Jedoch jedes Mal, wenn die Portnummer des lokalen standortänderungen, müssen Sie beim Aktualisieren der **Website-URL** Feld Ihrer Anwendung.
5. Wählen Sie die **Änderungen speichern** Schaltfläche.
6. Wählen Sie die **Apps** Registerkarte erneut aus, und zeigen Sie dann auf die Startseite für die Anwendung.
7. Kopieren der **App-ID** und **App-Geheimnis** Werte für Ihre Anwendung und in eine temporäre Textdatei einfügen. Diese Werte werden an dem Facebook-Anbieter in Ihrem Websitecode übergeben werden.
8. Beenden Sie die Facebook-Entwicklerwebsite ein.

Jetzt nehmen Sie Änderungen an zwei Seiten in Ihrer Website, damit Benutzer können sich bei der Website mithilfe ihrer Facebook-Konten anmelden können.

1. Bearbeiten Sie Ihre Website die  *\_AppStart.cshtml* Seite und die auskommentierung des Codes für die Facebook-OAuth-Anbieter. Der unkommentierten Codeblock sieht wie folgt aus: 

    [!code-xml[Main](top-features-in-web-pages-2/samples/sample15.xml)]
2. Kopieren der **App-ID** Wert aus der Facebook-Anwendung als Wert für die `consumerKey` Parameter (in Anführungszeichen).
3. Kopie **App-Geheimnis** Wert aus der Facebook-Anwendung als der `consumerSecret` Parameterwert.
4. Speichern und schließen Sie die Datei.
5. Bearbeiten der *~/Account/Login.cshtml* Seite, und entfernen Sie die Kommentare aus der `<fieldset>` Block am Ende der Seite. Um den Code zu kommentieren, entfernen Sie die `@*` Zeichen, die vor und nach der `<fieldset>` Block. Der Codeblock mit Kommentaren entfernt sieht wie folgt: 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample16.html)]
6. Speichern und schließen Sie die Datei.

Sie können jetzt die Facebook-Anmeldung testen.

1. Führen Sie des Standorts *default.cshtml* Seite, und wählen Sie die **Anmeldung** Schaltfläche.
2. Auf der *Anmeldung* Seite in der **einen anderen Dienst zum Anmelden verwenden** Abschnitt der **Facebook** Symbol. 

    Die Webseite leitet die Anforderung an die Facebook-Anmeldeseite.

    [![topSeven-oauth-2](top-features-in-web-pages-2/_static/image26.png)](top-features-in-web-pages-2/_static/image25.png)
3. Melden Sie sich eine Facebook-Konto. 

    Der Code verwendet das Facebook-Token, um Sie zu authentifizieren und gibt dann zurück auf eine Seite in dem Sie Ihre Facebook-Anmeldung mit Ihrer Website Anmeldung zuordnen. Der Name oder e-Mail-Adresse eines Benutzers in gefüllt wird die **E-Mail** Feld im Formular.

    [![topSeven-oauth-5](top-features-in-web-pages-2/_static/image28.png)](top-features-in-web-pages-2/_static/image27.png)
4. Wählen Sie die **zuordnen** Schaltfläche. 

    Der Browser gibt, um zur Startseite, und Sie angemeldet sind.

    [![topSeven-oauth-3](top-features-in-web-pages-2/_static/image30.png)](top-features-in-web-pages-2/_static/image29.png)

**Twitter-Anmeldungen zu aktivieren:** 

1. Navigieren Sie zu der [Twitter-Entwickler-Website](https://dev.twitter.com/).
2. Wählen Sie die **Erstellen einer App** verknüpfen, und melden Sie sich dann bei der Website.
3. Auf der **erstellen Sie eine Anwendung** bilden, "füllen" der **Namen** und **Beschreibung** Felder.
4. In der **WebSite** Feld, geben Sie die URL Ihrer Website (z. B. [ `http://www.example.com` ](http://www.example.com)). 

    > [!NOTE]
    > Wenn Sie Ihre Website lokal testen (über eine URL wie `http://localhost:12345`), ist die URL, Twitter nicht akzeptieren. Allerdings können Sie möglicherweise die lokalen Loopback-IP-Adresse verwenden (z. B. `http://127.0.0.1:12345`). Dies vereinfacht das Testen Ihrer Anwendung lokal. Jedoch jedes Mal, wenn die Portnummer Ihres lokalen Standorts geändert wird, Sie müssen zum Aktualisieren der **WebSite** Feld Ihrer Anwendung.
5. In der **Rückruf-URL** Feld, geben Sie eine URL für die Seite in Ihrer Website, die Benutzern, nach der Anmeldung in Twitter zurückgegeben werden sollen. Geben Sie beispielsweise die gleiche URL, die Sie in eingegeben haben, um Benutzer zur Startseite des Starter-Site zu senden (die ihre angemeldeten Status erkannt wird), die **WebSite** Feld.
6. Akzeptieren Sie die Bestimmungen ein, und wählen Sie die **die Twitter-Anwendung erstellen** Schaltfläche.
7. Auf der **Meine Anwendungen** Angebotsseite, wählen Sie die Anwendung, die Sie erstellt haben.
8. Auf der **Details** Registerkarte, führen Sie einen Bildlauf nach unten aus, und wählen Sie die **erstellen Sie eigene Zugriffstoken** Schaltfläche.
9. Auf der **Details** Registerkarte, kopieren Sie die **Consumerschlüssel** und **Consumerschlüssel** Werte für Ihre Anwendung und in eine temporäre Textdatei einfügen. Sie müssen diese Werte in Ihrem Websitecode auf Twitter-Anbieter übergeben.
10. Die Twitter-Website zu beenden.

Jetzt nehmen Sie Änderungen an zwei Seiten in Ihrer Website, damit Benutzer werden bei der Website mithilfe ihrer Konten Twitter anmelden können.

1. Bearbeiten Sie Ihre Website die  *\_AppStart.cshtml* Seite und die auskommentierung des Codes für den Twitter-OAuth-Anbieter. Der unkommentierten Codeblock sieht wie folgt: 

    [!code-csharp[Main](top-features-in-web-pages-2/samples/sample17.cs)]
2. Kopieren der **Consumerschlüssel** Wert aus der Twitter-Anwendung als Wert für die `consumerKey` Parameter (in Anführungszeichen).
3. Kopieren der **Consumerschlüssel** Wert aus der Twitter-Anwendung als Wert für die `consumerSecret` Parameter.
4. Speichern und schließen Sie die Datei.
5. Bearbeiten der *~/Account/Login.cshtml* Seite, und entfernen Sie die Kommentare aus der `<fieldset>` Block am Ende der Seite. Um den Code zu kommentieren, entfernen Sie die `@*` Zeichen, die vor und nach der `<fieldset>` Block. Der Codeblock mit Kommentaren entfernt sieht wie folgt: 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample18.html)]
6. Speichern und schließen Sie die Datei.

Sie können jetzt die Twitter-Anmeldung testen.

1. Führen Sie die *default.cshtml* Seite Ihrer Website, und wählen Sie die **Anmeldung** Schaltfläche.
2. Auf der *Anmeldung* Seite in der **einen anderen Dienst zum Anmelden verwenden** Abschnitt der **Twitter** Symbol. 

    Die Webseite leitet die Anforderung an einen Twitter-Anmeldeseite für die Anwendung, die Sie erstellt haben.

    [![topSeven-oauth-4](top-features-in-web-pages-2/_static/image32.png)](top-features-in-web-pages-2/_static/image31.png)
3. Melden Sie sich ein Twitter-Konto.
4. Der Code verwendet das Twitter-Token zur Authentifizierung des Benutzers und wechselt zurück zu einer Seite, in dem Sie zuordnen können Ihre Anmeldung mit Ihrem Konto für die Website. Der Name oder e-Mail-Adresse in gefüllt wird die **E-Mail** Feld im Formular.

    [![topSeven-oauth-5](top-features-in-web-pages-2/_static/image34.png)](top-features-in-web-pages-2/_static/image33.png)
5. Wählen Sie die **zuordnen** Schaltfläche. 

    Der Browser gibt, um zur Startseite, und Sie angemeldet sind.

    [![topSeven-oauth-3](top-features-in-web-pages-2/_static/image36.png)](top-features-in-web-pages-2/_static/image35.png)

<a id="maphelper"></a>
### <a name="adding-maps-using-the-maps-helper"></a>Hinzufügen von Zuordnungen mithilfe der Maps-Hilfsmethode

Web Pages 2 enthält Ergänzungen der ASP.NET Web Helpers Library wird ein Paket von add-ins für eine Web Pages-Website. Einer davon ist eine Komponente von bereitgestellten der `Microsoft.Web.Helpers.Maps` Klasse. Sie können die `Maps` Klasse, um basierend auf einer Adresse oder einen Satz von Längen- und Breitengrad Koordinaten Zuordnungen zu generieren. Die `Maps` -Klasse ermöglicht, die Sie direkt in beliebte Zuordnung Module, einschließlich Bing, Google, MapQuest und Yahoo aufrufen.

Um den neuen `Maps` Klasse in Ihrer Website müssen Sie zuerst die Version 2 von der Web Helpers Library installieren. Wechseln Sie zu diesem Zweck zu den Anweisungen zum Installieren der derzeit veröffentlichten Version der [ASP.NET Web Helpers Library](https://go.microsoft.com/fwlink/?LinkId=202889#webhelpers) Version 2, und installieren.

Die Schritte zum Hinzufügen der Zuordnung zu einer Seite sind identisch, unabhängig davon, die welche die Zuordnung Module, die Sie aufrufen. Sie nur einen JavaScript-Dateiverweis auf der Seite "Zuordnung" hinzufügen und dann einen Aufruf, der rendert die `<script>` tags auf der Seite. Klicken Sie dann auf der Seite "Zuordnung" rufen Sie das Modul für die Zuordnung, die, das Sie verwenden möchten.

Das folgende Beispiel zeigt, wie eine Seite, die eine Zuordnung basierend auf einer Adresse gerendert wird, und eine andere Seite, die eine Zuordnung basierend auf Längen- und Breitengrad Koordinaten rendert zu erstellen. Die Adresse Mapping-Beispiel verwendet, Google Maps und im-Koordinate Mapping-Beispiel verwendet die Bing Maps. Beachten Sie die folgenden Elemente im Code aus:

- Der Aufruf von `Assets.AddScript` am oberen Rand die beiden Zuordnungsseiten. Diese Methode fügt einen Verweis auf die *Jquery-1.6.2.min.js* -Datei, die enthalten ist das **Starter Site** Vorlage und, verlangt der `Maps` Klasse.
- Der Aufruf der `Assets.GetScripts` Methode in der Layoutdatei. Diese Methode rendert das `<script>` auf beiden Zuordnungsseiten zu kennzeichnen.
- Der Aufruf der `@Maps.GetGoogleHtml` und `@Maps.GetBingHtml` Methoden auf den Zuordnungsseiten. Um eine Adresse zuordnen, müssen Sie eine Adresszeichenfolge übergeben. Um Koordinaten zuzuordnen, müssen Sie die Längen- und Breitengrad übergeben Koordinaten. Für den Bing Maps-Modul, müssen Sie auch einen Schlüssel übergeben (die erhalten Sie kostenlos durch registrieren sich an den [Entwickler von Bing Maps-Website](https://www.microsoft.com/maps/developers/web.aspx)). Die Methoden für die andere Module der Karte funktionieren auf ähnliche Weise (`@Maps.GetYahooHtml`, `@Maps.GetMapQuestHtml`).

So erstellen Sie Zuordnungsseiten:

1. Erstellen Sie eine Website auf der Grundlage der **Starter Site** Vorlage.
2. Erstellen Sie eine Datei mit dem Namen *MapAddress.cshtml* im Stammverzeichnis der Website. Auf dieser Seite generiert eine Zuordnung basierend auf eine Adresse, die Sie übergeben.
3. Kopieren Sie den folgenden Code in die Datei überschreiben der vorhandenen Inhalts. 

    [!code-cshtml[Main](top-features-in-web-pages-2/samples/sample19.cshtml)]
4. Erstellen Sie eine Datei mit dem Namen  *\_MapLayout.cshtml* im Stammverzeichnis der Website. Auf dieser Seite werden Layoutseite für die beiden Zuordnungsseiten.
5. Kopieren Sie den folgenden Code in die Datei überschreiben der vorhandenen Inhalts. 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample20.html)]
6. Erstellen Sie eine Datei mit dem Namen *MapCoordinates.cshtml* im Stammverzeichnis der Website. Auf dieser Seite generiert eine Zuordnung basierend auf einem Satz von Koordinaten, die Sie übergeben.
7. Kopieren Sie den folgenden Code in die Datei überschreiben der vorhandenen Inhalts. 

    [!code-cshtml[Main](top-features-in-web-pages-2/samples/sample21.cshtml)]

So testen Sie Ihre Zuordnungsseiten:

1. Führen Sie die Seite *MapAddress.cshtml* Datei.
2. Geben Sie eine vollständige Adresszeichenfolge, einschließlich einer Straße, Bundesland oder Kanton und Postleitzahl, und wählen Sie dann die **Map It** Schaltfläche. Die Seite wird eine Zuordnung von Google Maps gerendert: 

    [![topseven-maphelper-1](top-features-in-web-pages-2/_static/image38.png)](top-features-in-web-pages-2/_static/image37.png)
3. Suchen Sie einen Satz von Breiten-und Längenkoordinaten für einen bestimmten Standort.
4. Führen Sie die Seite *MapCoordinates.cshtml*. Geben Sie die Koordinaten, und wählen Sie dann die **Map It** Schaltfläche. Die Seite wird eine Zuordnung von Bing Maps gerendert: 

    [![topseven-maphelper-2](top-features-in-web-pages-2/_static/image40.png)](top-features-in-web-pages-2/_static/image39.png)

<a id="sidebyside"></a>
### <a name="running-web-pages-applications-side-by-side"></a>Ausführen von Webseiten Anwendungen nebeneinander angezeigt werden

Web Pages 2 bietet die Möglichkeit zum Ausführen von Anwendungen nebeneinander angezeigt werden. Dadurch können Sie weiterhin die Web Pages-1-Anwendungen ausführen, neue Web Pages 2-Anwendungen erstellen und alle von ihnen auf dem gleichen Computer ausführen.

Hier sind einige Punkte zu beachten, wenn Sie die Web Pages 2-Betaversion mit WebMatrix installieren:

- Standardmäßig werden die vorhandene Web Pages-Anwendungen als Version 2-Anwendungen auf Ihrem Computer ausgeführt. (Die Assemblys für Version 2 im GAC installiert werden und werden automatisch verwendet werden.)
- Wenn Sie einen Standort mithilfe von Web Pages, Version 1 (anstelle des Standardwerts, wie am vorherigen Punkt) ausführen möchten, können Sie den Standort zu diesem Zweck konfigurieren. Wenn Ihre Website noch keinem *"Web.config"* Datei im Stammverzeichnis der Site, ein neues erstellen und kopieren Sie die folgenden XML-Code hinein, Überschreiben der vorhandenen Inhalts. Wenn der Standort bereits enthält eine *"Web.config"* hinzufügen. ein `<appSettings>` Element wie den folgenden Ausdruck ein, um die `<configuration>` Abschnitt.

    [!code-xml[Main](top-features-in-web-pages-2/samples/sample22.xml)]
  "– Wenn Sie eine Version im nicht angeben der *" Web.config "* Datei, einem Standort wird als Version 2 Standort bereitgestellt. (Assemblys der Version 2 werden kopiert, um die *"bin"* Ordner auf der Website bereitgestellt.)
- Neue Anwendungen, die mit der Websitevorlagen im Web Matrix Version 2 Beta enthalten die Web Pages-Version 2-Assemblys in der Website erstellt *"bin"* Ordner.

Im Allgemeinen können Sie immer steuern, welche Version von Webseiten für Ihre Website verwendet werden, mithilfe von NuGet die entsprechenden Assemblys in der Website installieren *"bin"* Ordner. Um Pakete zu suchen, besuchen Sie [NuGet.org](http://NuGet.org).

<a id="mobile"></a>
### <a name="rendering-pages-for-mobile-devices"></a>Rendern von Seiten für Mobile Geräte

Web Pages 2 können Sie benutzerdefinierte zeigt zum Rendern von Inhalt auf Mobile oder anderen Geräten zu erstellen.

Die `System.Web.WebPages` -Namespace enthält die folgenden Klassen, mit denen Sie arbeiten mit Anzeigemodi: `DefaultDisplayMode`, `DisplayInfo`, und `DisplayModes`. Sie können diese Klassen direkt verwenden und Code schreiben, der die Rechte Ausgabe für bestimmte Geräte gerendert wird.

Alternativ können Sie gerätespezifische Seiten erstellen, mit einem Dateibenennung Muster wie folgt: <em>FileName.</em> <em>Mobile</em><em>cshtml</em>. Sie können z. B. zwei Versionen einer Seite, eine namens erstellen <em>MyFile.cshtml</em> und dasjenige mit dem Namen <em>MyFile.Mobile.cshtml</em>. Zur Laufzeit, wenn ein mobiles Gerät anfordert <em>MyFile.cshtml</em>, Webseiten rendert den Inhalt von <em>MyFile.Mobile.cshtml</em>. Andernfalls <em>MyFile.cshtml</em> gerendert wird.

Das folgende Beispiel zeigt, wie mobile Rendering durch Hinzufügen einer Inhaltsseite für mobile Geräte aktivieren. *Page1.cshtml* enthält Inhalte sowie eine navigationsrandleiste. *Page1.Mobile.cshtml* den gleichen Inhalt enthält, lässt aber die Randleiste.

So erstellen und Ausführen des Codebeispiels:

1. In einer Web Pages-Website erstellen Sie eine Datei namens *Page1.cshtml* , und kopieren Sie die *Page1.cshtml* Inhalt hinein aus dem Beispiel.
2. Erstellen Sie eine Datei mit dem Namen *Page1.Mobile.cshtml* , und kopieren Sie die *Page1.Mobile.cshtml* Inhalt hinein aus dem Beispiel. Beachten Sie, dass die mobile Version der Seite im Abschnitt Navigation besser Rendern auf einen kleineren Bildschirm wird ausgelassen.
3. Führen Sie einen Desktopbrowser, und navigieren Sie zu *Page1.cshtml*.
4. Führen Sie einen mobilen Browser (oder einen Emulator für mobile Geräte), und navigieren Sie zu *Page1.cshtml*. Beachten Sie, dass, diesmal Webseiten die mobile Version der Seite rendert. 

    > [!NOTE]
    > Um mobiler Seiten zu testen, können Sie einen Mobilgerät-Simulator aus, der auf einem desktop-Computer ausgeführt wird. Mit diesem Tool können Sie die Webseiten zu testen, wie sie auf mobilen Geräten aussehen würde (d. h. in der Regel mit einer wesentlich kleineren anzeigen Bereich). Ein Beispiel für einen Simulator ist die [Benutzer-Agent Switcher Add-on](http://addons.mozilla.org/en-us/firefox/addon/user-agent-switcher/) für Mozilla Firefox, dem können Sie die verschiedenen mobile Browsern aus einer desktop-Version von Firefox emulieren.

*Page1.cshtml*

[!code-html[Main](top-features-in-web-pages-2/samples/sample23.html)]

*Page1.Mobile.cshtml*

[!code-html[Main](top-features-in-web-pages-2/samples/sample24.html)]

*Page1.cshtml* in einen Desktopbrowser gerendert:

[![topseven-displaymodes-1](top-features-in-web-pages-2/_static/image42.png)](top-features-in-web-pages-2/_static/image41.png)

*Page1.Mobile.cshtml* in einer Sicht Apple iPhone-Simulator, in den Firefox-Browser angezeigt. Obwohl die Anforderung ist *Page1.cshtml*, die Anwendung rendert *Page1.Mobile.cshtml*.

[![topseven-displaymodes-2](top-features-in-web-pages-2/_static/image44.png)](top-features-in-web-pages-2/_static/image43.png)

<a id="resources"></a>
## <a name="additional-resources"></a>Zusätzliche Ressourcen

### <a name="aspnet-web-pages-1-resources"></a>1-Ressourcen für ASP.NET Web Pages

> [!NOTE]
> Die meisten Web Pages-1-Programmierung und -API-Ressourcen gelten aber weiterhin zu Web Pages 2.

- [Einführung in ASP.NET Web Pages-Programmierung](https://go.microsoft.com/fwlink/?LinkId=202890)

### <a name="webmatrix-resources"></a>WebMatrix-Ressourcen

- [WebMatrix 2 Neuigkeiten](http://webmatrix.com/next)
- [Microsoft WebMatrix Site](https://go.microsoft.com/fwlink/?LinkID=195076)
- [Starten der Webentwicklung mit Microsoft WebMatrix](https://msdn.microsoft.com/en-us/library/hh145669(v=VS.99).aspx)(einschließlich eine beispielanwendung voller Länge Webseiten)
