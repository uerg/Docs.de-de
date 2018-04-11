---
uid: web-forms/overview/older-versions-getting-started/master-pages/urls-in-master-pages-cs
title: URLs in Masterseiten (c#) | Microsoft Docs
author: rick-anderson
description: Erläutert, wie URLs in die Masterseite aufgrund wird in einem anderen relativen Verzeichnis als Inhaltsseite Masterseitendatei unterbrochen werden können. Untersucht die Zurücksetzung...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2008
ms.topic: article
ms.assetid: 48b58a18-5ea4-468c-b326-f35331b3e1e9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/urls-in-master-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: 494338f1a0705c8d7e15bc693ae1ec6362b26d64
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="urls-in-master-pages-c"></a>URLs in Masterseiten (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Herunterladen von Code](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_04_CS.zip) oder [PDF herunterladen](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_04_CS.pdf)

> Erläutert, wie URLs in die Masterseite aufgrund wird in einem anderen relativen Verzeichnis als Inhaltsseite Masterseitendatei unterbrochen werden können. Sucht nach der Zurücksetzung URLs über ~ in die deklarative Syntax und programmgesteuert mithilfe von ResolveUrl und ResolveClientUrl. (Auch betrachten


## <a name="introduction"></a>Einführung

In allen Beispielen haben wir bisher, gesehen, die Master- und Inhaltsseiten im selben Ordner (Stammordner der Website) gespeichert waren. Aber es besteht kein Grund, warum die Master- und Inhaltsseiten im gleichen Ordner befinden. Sie können sicherlich Inhaltsseiten in Unterordnern erstellen. Auf ähnliche Weise können Sie erstellen eine `~/MasterPages/` Ordner, in dem Sie Ihre Website Masterseiten.

Ein mögliches Problem bei der Integration von Master- und Inhaltsseiten in unterschiedlichen Ordnern umfasst unterbrochen URLs. Wenn die Gestaltungsvorlage relativen URLs in Hyperlinks, Bilder oder andere Elemente enthält, werden der Link für Inhaltsseiten ungültig, die sich in einem anderen Ordner befinden. In diesem Lernprogramm untersuchen wir die Quelle von diesem Problem sowie problemumgehungen.

## <a name="the-problem-with-relative-urls"></a>Das Problem mit dem relativen URLs

Eine URL auf einer Webseite gilt als eine *relative URL* ist der Speicherort der Ressource, die er zeigt, relativ zum Speicherort der Webseite, in der Website-Ordnerstruktur. Jede URL, die nicht mit einem führenden Schrägstrich beginnt (`/`) oder ein Protokoll (z. B. `http://`) relativ ist, da er nicht behoben wird, vom Browser basierend auf den Speicherort der Webseite, die die URL enthält.

Unsere Website hat z. B. ein `~/Images/` Ordner mit einer einzelnen Bilddatei `PoweredByASPNET.gif`. Die Masterseitendatei `Site.master` verfügt über eine `<img>` Element in der `footerContent` Region durch Folgendes Markup:


[!code-html[Main](urls-in-master-pages-cs/samples/sample1.html)]

Die `src` -Attributwert in der `<img>` Element ist eine relative URL auf, da er nicht startet `/` oder `http://`. Kurz gesagt, der `src` Attributwert weist den Browser anzuweisen, suchen Sie in der `Images` Unterordner für eine Datei namens `PoweredByASPNET.gif`.

Beim Zugriff auf einer Inhaltsseite, wird die oben genannten Markup direkt an den Browser gesendet. Erkundet besuchen `About.aspx` und zeigen Sie die HTML-Quelle, die an den Browser gesendet. Sie werden feststellen, dass die genaue gleiche Markup in die Masterseite an den Browser gesendet wurde.


[!code-html[Main](urls-in-master-pages-cs/samples/sample2.html)]

Ist der Inhaltsseite im Stammordner (Istzustand `About.aspx`) alles funktioniert wie erwartet, da es ist ein `Images` Unterordner relativ zum Stammverzeichnis. Allerdings aufgliedern Dinge, wenn die Seite Inhalte in einem anderen Ordner als die Gestaltungsvorlage ist. Um dies zu veranschaulichen, erstellen Sie einen Unterordner namens `Admin`. Fügen Sie eine Seite mit dem Namen `Default.aspx` auf die `Admin` Ordner, und stellen Sie sicher, binden Sie die neue Seite, um die `Site.master` Masterseite.

> [!NOTE]
> In der [ *Titel, Meta-Tags und andere HTML-Header angeben, der Gestaltungsvorlage* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) Lernprogramm wir eine benutzerdefinierte Basisseite-Klasse, die mit dem Namen erstellt `BasePage` , die mit dem Titel der Inhaltsseite automatisch festgelegt (wenn es wurde nicht explizit zugewiesen). Vergessen Sie nicht, die neu erstellte Seite Code-Behind-Klasse abgeleitet haben `BasePage` , damit sie diese Funktionalität nutzen kann.


Nachdem Sie diese Inhaltsseite erstellt haben, sollte Ihre Projektmappen-Explorer in Abbildung 1 ähneln.


![Ein neuer Ordner und ASP.NET-Seite wurden dem Projekt hinzugefügt](urls-in-master-pages-cs/_static/image1.png)

**Abbildung 01**: einen neuen Ordner und ASP.NET-Seite zum Projekt hinzugefügt wurde


Aktualisieren Sie als Nächstes die `Web.sitemap` zu einer neuen Datei `<siteMapNode>` Eintrag für diese Lektion. Das folgende XML zeigt die vollständige `Web.sitemap` Markup, die nun das Hinzufügen einer dritten enthält `<siteMapNode>` Element.


[!code-xml[Main](urls-in-master-pages-cs/samples/sample3.xml)]

Das neu erstellte `Default.aspx` Seite sollte für die vier ContentPlaceHolders in vier Content-Steuerelemente haben `Site.master`. Text des Inhaltssteuerelements verweisenden Hinzufügen der `MainContent` ContentPlaceHolder und besuchen dann die Seite über einen Browser. Der Browser kann nicht gefunden, wie in Abbildung 2 gezeigt, die `PoweredByASPNET.gif` Bilddatei. Was ist hier los?

Die `~/Admin/Default.aspx` Inhaltsseite wird das gleiche HTML gesendet, für die `footerContent` wurde der Region der `About.aspx` Seite:


[!code-html[Main](urls-in-master-pages-cs/samples/sample4.html)]

Da die `<img>` des Elements `src` -Attribut ist eine relative URL, die der Browser versucht, suchen Sie nach einer `Images` Ordner relativ zum Speicherort der Webseite. Das heißt, sucht der Browser für die Abbilddatei `Admin/Images/PoweredByASPNET.gif`.


[![Die Bilddatei PoweredByASPNET.gif wurde nicht gefunden](urls-in-master-pages-cs/_static/image3.png)](urls-in-master-pages-cs/_static/image2.png)

**Abbildung 02**: die `PoweredByASPNET.gif` Image-Datei kann nicht gefunden werden ([klicken Sie hier, um das Bild in voller Größe angezeigt](urls-in-master-pages-cs/_static/image4.png))


### <a name="replacing-relative-urls-with-absolute-urls"></a>Ersetzen Relative URLs mit Absolute URLs

Das Gegenteil von eine relative URL ist ein *absolute URL*, also eine, die mit einem Schrägstrich beginnt (`/`) oder ein Protokoll wie z. B. `http://`. Da eine absolute URL den Speicherort einer Ressource von einem bekannten festen Punkt angegeben wird, gilt die gleiche absolute URL in einer Webseite, unabhängig von der Webseite Position in der Website-Ordnerstruktur.

Um die beschädigte Bilder in Abbildung 2 veranschaulichte zu beheben, müssen wir aktualisieren das `<img>` des Elements `src` Attribut, damit sie eine absolute URL anstelle einer relativen verwendet. Um die richtige absolute URL zu ermitteln, finden Sie auf der Webseiten in Ihrer Website, und untersuchen Sie die Adressleiste. Wie in die Adressleiste in Abbildung 2 gezeigt, wird der vollqualifizierte Pfad für die Webanwendung `http://localhost:3908/ASPNET_MasterPages_Tutorial_04_CS/`. Deshalb konnten wir aktualisiert die `<img>` des Elements `src` -Attribut auf eine der folgenden absolute URLs:

- `/ASPNET_MasterPages_Tutorial_04_CS/Images/PoweredByASPNET.gif`
- `http://localhost:3908/ASPNET_MasterPages_Tutorial_04_CS/Images/PoweredByASPNET.gif`

Erkundet zum Aktualisieren der `<img>` des Elements `src` -Attribut auf eine absolute URL mithilfe eines der oben gezeigten Formate ein, und besuchen dann die `~/Admin/Default.aspx` Seite über einen Browser. Dieses Mal wird der Browser ordnungsgemäß suchen und Anzeigen der `PoweredByASPNET.gif` Bilddatei (siehe Abbildung 3).


[![Das Bild PoweredByASPNET.gif wird jetzt angezeigt.](urls-in-master-pages-cs/_static/image6.png)](urls-in-master-pages-cs/_static/image5.png)

**Abbildung 03**: die `PoweredByASPNET.gif` Image wird jetzt angezeigt ([klicken Sie hier, um das Bild in voller Größe angezeigt](urls-in-master-pages-cs/_static/image7.png))


Hartcodierung in die absolute URL funktioniert, zwar verbindet sie eng HTML-Server für die Website und Ordner ändern kann. Verwenden eine absolute URL des Formulars `http://localhost:3908/...` ist jedoch fehleranfällig, da die vorhergehende Portnummer `localhost` wird automatisch ausgewählt, jedes Mal, die Visual Studio integrierten ASP.NET Development Web Server gestartet wird. Auf ähnliche Weise die `http://localhost` Teil ist nur gültig, wenn lokal testen. Sobald der Code auf einem Produktionsserver bereitgestellt wird, die URL-Basis ändert sich in einen Text wie z. B. `http://www.yourserver.com`. Die absolute URL im Format `/ASPNET_MasterPages_Tutorial_04_CS/...` auch aus der gleichen unzulänglichen leidet, da der Pfad der Anwendung häufig zwischen Entwicklung und dem Produktionsserver unterscheidet.

Die gute Nachricht ist, dass ASP.NET, eine Methode bietet für eine gültige relative URL zur Laufzeit generieren.

## <a name="usingandresolveclienturl"></a>Mithilfe von`~`und`ResolveClientUrl`

Stattdessen als eine absolute URL hartcodieren, ASP.NET erlaubt Seite verwenden, die Tilde (`~`) an, dass der Stamm der Webanwendung. Zum Beispiel weiter oben in diesem Lernprogramm verwendet die Notation `~/Admin/Default.aspx` in den Text zum Verweisen auf die `Default.aspx` auf der Seite der `Admin` Ordner. Die `~` gibt an, dass die `Admin` Ordner ist ein Unterordner des Stamms der Webanwendung.

Die `Control` Klasse [ `ResolveClientUrl` Methode](https://msdn.microsoft.com/library/system.web.ui.control.resolveclienturl.aspx) eine URL verwendet, und ändert sie eine relative URL für die Webseite, auf dem sich das Steuerelement befindet. Beispielsweise Aufrufen `ResolveClientUrl("~/Images/PoweredByASPNET.gif")` aus `About.aspx` gibt `Images/PoweredByASPNET.gif`. Aufrufen von `~/Admin/Default.aspx`, dagegen `../Images/PoweredByASPNET.gif`.

> [!NOTE]
> Da alle ASP.NET-Serversteuerelemente Ableiten der `Control` -Klasse, die alle Serversteuerelemente haben Zugriff auf die `ResolveClientUrl` Methode. Auch das `Page` Klasse leitet sich von der `Control` -Klasse, was bedeutet, dass Sie diese Methode, direkt aus Ihrem ASP.NET-Seiten Code-Behind-Klassen verwenden können.


### <a name="usingin-the-declarative-markup"></a>Mit`~`im deklarativen Markup

Mehrere ASP.NET Web-Steuerelemente enthalten, URL-bezogene Eigenschaften: das Linksteuerelement verfügt über eine `NavigateUrl` Eigenschaft; das Bild, das Steuerelement besitzt eine `ImageUrl` Eigenschafts- und So weiter. Beim Rendern, übergeben diese Steuerelemente zu deren URL-bezogene Eigenschaftswerte `ResolveClientUrl`. Daher, wenn diese Eigenschaften enthalten eine `~` um den Stamm der Webanwendung anzugeben, wird die URL eine gültige relative URL geändert werden.

Beachten Sie, bei denen nur ASP.NET-Serversteuerelemente transformiert die `~` in deren URL-bezogene Eigenschaften. Wenn eine `~` in statische HTML-Markup wird angezeigt, wie z. B. `<img src="~/Images/PoweredByASPNET.gif" />`, sendet das Modul für ASP.NET die `~` an den Browser zusammen mit dem Rest des HTML-Inhalts. Der Browser wird vorausgesetzt, dass die `~` ist Teil der URL. Wenn der Browser das Markup empfängt beispielsweise `<img src="~/Images/PoweredByASPNET.gif" />` es wird vorausgesetzt, es ist ein Unterordner mit dem Namen `~` mit einem Unterordner `Images` , enthält die Bilddatei `PoweredByASPNET.gif`.

Beheben Sie das Image-Markup in `Site.master`, ersetzen Sie die vorhandene `<img>` Element mit einem ASP.NET-Webseiten-Image-Steuerelement. Legen Sie des Image-Websteuerelements `ID` auf `PoweredByImage`, dessen `ImageUrl` Eigenschaft `~/Images/PoweredByASPNET.gif`, und die zugehörige `AlternateText` -Eigenschaft auf "Powered by ASP.NET!"


[!code-aspx[Main](urls-in-master-pages-cs/samples/sample5.aspx)]

Nach dieser Änderung der Masterseite, rufen Sie erneut die `~/Admin/Default.aspx` Seite erneut. Dieses Mal die `PoweredByASPNET.gif` Bilddatei, die auf der Seite angezeigt (siehe Abbildung 3). Wenn die Image-Web-Steuerelement ist gerendert verwendet die `ResolveClientUrl` Verfahren zum Beheben der `ImageUrl` Eigenschaftswert. In `~/Admin/Default.aspx` der `ImageUrl` in einen geeigneten relative URL konvertiert wird, die als HTML-Quelle wird im folgenden Codeausschnitt:


[!code-html[Main](urls-in-master-pages-cs/samples/sample6.html)]

> [!NOTE]
> Zusätzlich zu den in Web-URL-basierte Steuerelementeigenschaften, verwendet der `~` kann auch verwendet werden, beim Aufrufen der `Response.Redirect` und `Server.MapPath` Methoden, u. a. Darüber hinaus die `ResolveClientUrl` Methode kann direkt von einer ASP.NET-Anwendung oder der Masterseite deklarativem Markup, bei Bedarf aufgerufen werden, finden Sie unter [Fritz Onion](https://www.pluralsight.com/blogs/fritz/)des Blogeintrag [Using `ResolveClientUrl` im Markup](https://www.pluralsight.com/blogs/fritz/archive/2006/02/06/18596.aspx).


## <a name="fixing-the-master-pages-remaining-relative-urls"></a>Korrigieren die Gestaltungsvorlage verbleibende relativen URLs

Zusätzlich zu den `<img>` Element in der `footerContent` , dass wir gerade behoben, die Gestaltungsvorlage enthält eine mehr relativen URL, die unsere seine Aufmerksamkeit erforderlich ist. Die `topContent` Region enthält den Link "Master Pages-Lernprogramme," verweist auf `Default.aspx`.


[!code-html[Main](urls-in-master-pages-cs/samples/sample7.html)]

Da diese URL relativ ist, senden Sie den Benutzer die `Default.aspx` Seite in den Ordner der Inhaltsseite besuchen. Auf diesen Link, der immer auf zeigen haben `Default.aspx` im Stammordner wir ersetzen müssen die `<a>` Element mit einem HyperLink zu steuern, sodass wir verwenden können, die `~` Notation.

Entfernen Sie die `<a>` Elementknoten dem Elementmarkup und fügen Sie ein Linksteuerelement an seiner Stelle. Legen Sie des Links `ID` auf `lnkHome`, dessen `NavigateUrl` Eigenschaft, um `~/Default.aspx`, und die zugehörige `Text` -Eigenschaft auf "Master Pages-Lernprogramme".


[!code-aspx[Main](urls-in-master-pages-cs/samples/sample8.aspx)]

Das ist alles! An diesem Punkt, auf denen die URLs in unserer Masterseite ordnungsgemäß basieren, einer Inhaltsseite unabhängig davon, welche Ordner der Masterseite und Inhaltsseite gerendert in befinden.

### <a name="automatic-url-resolution-in-theheadsection"></a>URL für automatische Auflösung in der`<head>`Abschnitt

In der [ *erstellen eine standortweite Layout mithilfe von Master Pages* ](creating-a-site-wide-layout-using-master-pages-cs.md) Lernprogramm, die wir hinzugefügt eine `<link>` auf die `Styles.css` in der Datei die `<head>` Region:


[!code-aspx[Main](urls-in-master-pages-cs/samples/sample9.aspx)]

Während der `<link>` des Elements `href` Attribut relativ ist, wird automatisch auf einen geeigneten Pfad zur Laufzeit konvertiert. Wie in erläutert die [ *Titel, Meta-Tags und andere HTML-Header in die Masterseite angeben* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) Lernprogramm die `<head>` Region ist tatsächlich eine serverseitige-Steuerelement, das dadurch so ändern Sie die Inhalt der entsprechenden innere Steuerelemente, wenn er gerendert wird.

Um dies zu überprüfen, rufen Sie erneut die `~/Admin/Default.aspx` Seite, und zeigen Sie die HTML-Quelle, die an den Browser gesendet. Wie der folgende Codeausschnitt veranschaulicht, die `<link>` des Elements `href` Attribut automatisch so geändert wurde, einen geeigneten relative URL `../Styles.css`.


[!code-html[Main](urls-in-master-pages-cs/samples/sample10.html)]

## <a name="summary"></a>Zusammenfassung

Masterseiten gehören sehr häufig, Links, Bilder und andere externen Ressourcen, die über eine URL angegeben werden müssen. Da die Gestaltungsvorlage und Inhaltsseiten nicht im selben Ordner vorhanden sein können, ist es wichtig, unterlassen, verwenden relative URLs. Es zwar möglich, die hartcodierte absolute URLs verwenden, die absolute URL durch die auf diese Weise so eng an die Webanwendung verbindet. Wenn die absolute URL ändert, wie häufig beim Verschieben oder zum Bereitstellen einer Webanwendung - - müssen Sie daran denken, wechseln zurück, und aktualisieren Sie die absolute URLs.

Der optimale Ansatz ist die Verwendung die Tilde (`~`) an, dass das Stammverzeichnis der Anwendung. Zuordnen von ASP.NET Web-Steuerelemente, die URL-bezogene Eigenschaften enthalten die `~` zum Stammverzeichnis Anwendung zur Laufzeit. Intern, Websteuerelemente verwenden die `Control` Klasse `ResolveClientUrl` Methode, um eine gültige relative URL zu generieren. Diese Methode ist öffentlich und für jedes Steuerelement verfügbar (einschließlich der `Page` Klasse), sodass Sie programmgesteuert aus Ihrem Code-Behind-Klassen verwenden können, wenn erforderlich.

Viel Spaß beim Programmieren!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Lernprogramm erläutert finden Sie in den folgenden Ressourcen:

- [Masterseiten in ASP.NET](http://www.odetocode.com/Articles/419.aspx)
- [URL in einer Masterseite Zurücksetzung](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/masterpages/default.aspx#urls)
- [Mit `ResolveClientUrl` im Markup](https://www.pluralsight.com/blogs/fritz/archive/2006/02/06/18596.aspx)

### <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor mehrerer ASP/ASP.NET-Büchern und Gründer von 4GuysFromRolla.com arbeitet mit Microsoft-Web-Technologien seit 1998. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 3.5 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott erreicht werden kann, zur [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog unter [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Besonderen Dank an

Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Zurück](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md)
> [Weiter](control-id-naming-in-content-pages-cs.md)
