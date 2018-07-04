---
uid: web-forms/overview/older-versions-getting-started/master-pages/urls-in-master-pages-cs
title: URLs auf Masterseiten (c#) | Microsoft-Dokumentation
author: rick-anderson
description: Adressen, wie der URLs in die Masterseite aufgrund wird in einem anderen relativen Verzeichnis als die Inhaltsseite die Masterseitendatei umbrochen werden können. Untersucht die REBASE wird ausgeführt...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2008
ms.topic: article
ms.assetid: 48b58a18-5ea4-468c-b326-f35331b3e1e9
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/urls-in-master-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: dcbd066a55fed3dd6c79e8a091e61449f85531b0
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37398387"
---
<a name="urls-in-master-pages-c"></a>URLs auf Masterseiten (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_04_CS.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_04_CS.pdf)

> Adressen, wie der URLs in die Masterseite aufgrund wird in einem anderen relativen Verzeichnis als die Inhaltsseite die Masterseitendatei umbrochen werden können. Untersucht die neuen Basisadressen URLs über ~ in der deklarativen Syntax und ResolveUrl und ResolveClientUrl Programmgesteuertes verwenden. (Betrachten Sie auch


## <a name="introduction"></a>Einführung

In allen Beispielen haben wir bisher gesehen, die Master- und Inhaltsseiten in demselben Ordner (Stammordner der Website) gespeichert waren. Aber es gibt keinen Grund, warum die Master- und Inhaltsseiten in demselben Ordner befinden. Sie können sicherlich Inhaltsseiten in Unterordnern erstellen. Auf ähnliche Weise können Sie erstellen eine `~/MasterPages/` Ordner, in dem Sie Masterseiten Ihrer Website.

Ein mögliches Problem mit der Master- und Inhaltsseiten in unterschiedlichen Ordnern platzieren umfasst unterbrochen URLs. Wenn die Masterseite relative URLs in Hyperlinks, Bilder oder andere Elemente enthält, werden der Link für Inhaltsseiten ungültig, die sich in einem anderen Ordner befinden. In diesem Tutorial untersuchen wir die Quelle von diesem Problem sowie problemumgehungen.

## <a name="the-problem-with-relative-urls"></a>Das Problem mit relativen URLs

Eine URL auf einer Webseite gilt eine *relativen URL* ist der Speicherort der Ressource, die diese verweist auf relativ zum Speicherort der Webseite, in der Website-Ordnerstruktur. Jede URL, die nicht mit einem führenden Schrägstrich beginnen (`/`) oder ein Protokoll (z. B. `http://`) ist relativ, da er aufgelöst wird, vom Browser basierend auf den Speicherort der Website, die die URL enthält.

Unsere Website verfügt beispielsweise über eine `~/Images/` Ordner mit einer einzelnen Bilddatei, `PoweredByASPNET.gif`. Die Masterseitendatei `Site.master` verfügt über eine `<img>` Element in der `footerContent` Region durch das folgende Markup:


[!code-html[Main](urls-in-master-pages-cs/samples/sample1.html)]

Die `src` -Attributwert in der `<img>` Element ist eine relative URL, da er nicht mit beginnt `/` oder `http://`. Kurz gesagt: die `src` Attributwert weist den Browser gesucht werden soll, die `Images` Unterordner für eine Datei namens `PoweredByASPNET.gif`.

Wenn sich eine Inhaltsseite zu besuchen, wird das obenstehende Markup direkt an den Browser gesendet. Besuchen in Ruhe `About.aspx` und zeigen Sie die an den Browser gesendete HTML-Quelle. Sie werden feststellen, dass genaue dasselbe Markup auf der Masterseite an den Browser gesendet wurde.


[!code-html[Main](urls-in-master-pages-cs/samples/sample2.html)]

Ist die Seite Inhalte im Stammordner (wie `About.aspx`) alles funktioniert wie erwartet, da gibt es eine `Images` Unterordner relativ zum Stammordner. Allerdings unterteilen Aspekte ist von die Seite Inhalte in einem anderen Ordner als Masterseite. Um dies zu veranschaulichen, erstellen Sie einen Unterordner namens `Admin`. Fügen Sie eine Inhaltsseite, die mit dem Namen `Default.aspx` auf die `Admin` Ordner, sodass Sie sicher, dass die neue Seite binden die `Site.master` Masterseite.

> [!NOTE]
> In der [ *Titel, Meta-Tags und anderer HTML-Header angeben, auf der Masterseite* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) Lernprogramm eine benutzerdefinierten Basisseite-Klasse, die mit dem Namen erstellten `BasePage` , die Inhalte der Titel der Seite automatisch festgelegt (wenn es wurde nicht explizit zugewiesen). Vergessen Sie nicht, die neu erstellte Seite Code-Behind-Klasse abgeleitet haben `BasePage` , damit diese Funktion nutzen können.


Nachdem Sie diese Inhaltsseite erstellt haben, sollte der Projektmappen-Explorer wie in Abbildung 1 aussehen.


![Einen neuen Ordner und eine ASP.NET-Seite wurden dem Projekt hinzugefügt](urls-in-master-pages-cs/_static/image1.png)

**Abbildung 01**: einen neuen Ordner und eine ASP.NET-Seite wurde zum Projekt


Aktualisieren Sie als Nächstes die `Web.sitemap` hinzu, um ein neues einzubeziehen `<siteMapNode>` Eintrag in dieser Lektion. Das folgende XML zeigt die vollständige `Web.sitemap` Markup, die jetzt das Hinzufügen einer dritten `<siteMapNode>` Element.


[!code-xml[Main](urls-in-master-pages-cs/samples/sample3.xml)]

Die neu erstellte `Default.aspx` Seite müssen vier Inhaltssteuerelemente, die für die vier ContentPlaceHolder-Steuerelemente in `Site.master`. Fügen Sie Text hinzu, das Inhaltssteuerelement verweisen auf die `MainContent` ContentPlaceHolder und besuchen Sie dann auf die Seite über einen Browser. Der Browser kann nicht gefunden, wie in Abbildung 2 gezeigt, die `PoweredByASPNET.gif` Bilddatei. Was ist hier los?

Die `~/Admin/Default.aspx` Inhaltsseite erhält jedes Mal den gleichen HTML-Code für die `footerContent` Region wie der Fall war die `About.aspx` Seite:


[!code-html[Main](urls-in-master-pages-cs/samples/sample4.html)]

Da die `<img>` des Elements `src` -Attribut ist eine relative URL, die der Browser versucht, suchen Sie nach einer `Images` Ordner relativ zum Ordner für die Webseite. Das heißt, sucht der Browser für die Abbilddatei `Admin/Images/PoweredByASPNET.gif`.


[![Die PoweredByASPNET.gif Image-Datei wurde nicht gefunden](urls-in-master-pages-cs/_static/image3.png)](urls-in-master-pages-cs/_static/image2.png)

**Abbildung 02**: die `PoweredByASPNET.gif` Image-Datei wurde nicht gefunden ([klicken Sie, um das Bild in voller Größe anzeigen](urls-in-master-pages-cs/_static/image4.png))


### <a name="replacing-relative-urls-with-absolute-urls"></a>Ersetzen Relative URLs mit Absolute URLs

Das Gegenteil einer relativen URL ist ein *absolute URL*, einer, der mit einem Schrägstrich beginnt (`/`) oder ein Protokoll wie z. B. `http://`. Da eine absolute URL den Speicherort einer Ressource von einem bekannten festen Punkt angegeben wird, gilt die gleiche absolute URL in jede Webseite, unabhängig von der Webseite Standort in der Website-Ordnerstruktur.

Um die beschädigte Bilder, die in Abbildung 2 dargestellten zu beheben, müssen wir aktualisieren den `<img>` des Elements `src` Attribut, sodass sie eine absolute URL anstatt einer relativen verwendet. Um die richtige absolute URL zu bestimmen, finden Sie auf eine der Seiten auf Ihrer Website, Web, und überprüfen Sie die Adressleiste. Wie in die Adressleiste in Abbildung 2 gezeigt, ist der vollqualifizierte Pfad der Webanwendung `http://localhost:3908/ASPNET_MasterPages_Tutorial_04_CS/`. Aus diesem Grund können wir aktualisieren den `<img>` des Elements `src` -Attribut auf eine der beiden folgenden absolute URLs:

- `/ASPNET_MasterPages_Tutorial_04_CS/Images/PoweredByASPNET.gif`
- `http://localhost:3908/ASPNET_MasterPages_Tutorial_04_CS/Images/PoweredByASPNET.gif`

Aktualisieren Sie in Ruhe die `<img>` des Elements `src` -Attribut auf eine absolute URL mithilfe eines der oben gezeigten Formate ein, und rufen Sie die `~/Admin/Default.aspx` Seite über einen Browser. Dieses Mal der Browser wird ordnungsgemäß suchen und Anzeigen der `PoweredByASPNET.gif` Image-Datei (siehe Abbildung 3).


[![Das PoweredByASPNET.gif-Image ist jetzt angezeigt.](urls-in-master-pages-cs/_static/image6.png)](urls-in-master-pages-cs/_static/image5.png)

**Abbildung 03**: die `PoweredByASPNET.gif` Bild wird jetzt angezeigt ([klicken Sie, um das Bild in voller Größe anzeigen](urls-in-master-pages-cs/_static/image7.png))


Fest zu programmieren, in die absolute URL funktioniert, zwar verbindet er eng Ihren HTML-Code auf der Website-Server und Ordner, die sich ändern kann. Verwenden eine absolute URL des Formulars `http://localhost:3908/...` fehleranfällig ist da die Portnummer, die vorherigen `localhost` automatisch jedes Mal, die Visual Studio integrierten ASP.NET Development Web Server gestartet wird ausgewählt. Auf ähnliche Weise die `http://localhost` Teil ist nur gültig, wenn Sie lokal testen. Sobald der Code auf einem Produktionsserver bereitgestellt wird, die URL-Basis, ändert sich etwas anderes, wie z. B. `http://www.yourserver.com`. Die absolute URL im Format `/ASPNET_MasterPages_Tutorial_04_CS/...` auch aus der gleichen Fehleranfälligkeit beeinträchtigt, da es sich bei diesen Pfad der Anwendung häufig zwischen Entwicklungs- und produktionsumgebungen Servern unterscheidet.

Die gute Nachricht ist, dass ASP.NET eine Methode zum Generieren einer gültigen relativen URL zur Laufzeit bietet.

## <a name="usingandresolveclienturl"></a>Mithilfe von`~`und`ResolveClientUrl`

Stattdessen als eine absolute URL hart zu codieren, ASP.NET mit dem Seitenentwickler, verwenden Sie die Tilde (`~`) an den Stamm der Webanwendung. Weiter oben in diesem Tutorial verwendete ich z. B. die Notation `~/Admin/Default.aspx` im Text zum Verweisen auf die `Default.aspx` auf der Seite die `Admin` Ordner. Die `~` gibt an, dass die `Admin` Ordner ist ein Unterordner des Stammverzeichnisses für die Webanwendung.

Die `Control` Klasse [ `ResolveClientUrl` Methode](https://msdn.microsoft.com/library/system.web.ui.control.resolveclienturl.aspx) akzeptiert eine URL und ändert sie eine relative URL für die Webseite, die auf dem sich das Steuerelement befindet. Zum Beispiel der Aufruf `ResolveClientUrl("~/Images/PoweredByASPNET.gif")` aus `About.aspx` gibt `Images/PoweredByASPNET.gif`. Aufrufen von `~/Admin/Default.aspx`, dagegen die `../Images/PoweredByASPNET.gif`.

> [!NOTE]
> Da von allen ASP.NET-Serversteuerelementen abgeleitet sind die `Control` -Klasse, die alle Steuerelemente haben Zugriff auf die `ResolveClientUrl` Methode. Obwohl das `Page` Klasse leitet sich von der `Control` -Klasse, was bedeutet, dass Sie diese Methode direkt von Ihren ASP.NET-Seiten CodeBehind-Klassen verwenden können.


### <a name="usingin-the-declarative-markup"></a>Mithilfe von`~`im deklarativen Markup

Mehrere Steuerelemente von ASP.NET Web enthalten die URL-bezogene Eigenschaften: das Linksteuerelement ist eine `NavigateUrl` Eigenschaft: das Bild, das Steuerelement hat eine `ImageUrl` Eigenschaft; und So weiter. Beim Rendern, übergeben diese Steuerelemente zu ihrer URL bezogenen Eigenschaftswerte `ResolveClientUrl`. Daher enthalten diese Eigenschaften eine `~` um den Stamm der Webanwendung anzugeben, wird die URL in eine gültige relative URL geändert werden.

Denken Sie daran, die nur die ASP.NET-Serversteuerelemente Umwandeln der `~` in ihrer URL-bezogene Eigenschaften. Wenn eine `~` wird im statischen HTML-Markup, z. B. `<img src="~/Images/PoweredByASPNET.gif" />`, die ASP.NET-Engine sendet die `~` an den Browser zusammen mit dem Rest des HTML-Inhalts. Der Browser setzt voraus, dass die `~` ist Teil der URL. Wenn der Browser das Markup empfängt beispielsweise `<img src="~/Images/PoweredByASPNET.gif" />` es wird vorausgesetzt, es ist ein Unterordner, die mit dem Namen `~` mit einem Unterordner `Images` , enthält die Bilddatei `PoweredByASPNET.gif`.

Beheben Sie das Image-Markup in `Site.master`, ersetzen Sie die vorhandene `<img>` Element mit einem ASP.NET-Webanwendung-Image-Steuerelement. Legen Sie des Image-Websteuerelements `ID` zu `PoweredByImage`, dessen `ImageUrl` Eigenschaft, um `~/Images/PoweredByASPNET.gif`, und die zugehörige `AlternateText` Eigenschaft auf "Powered by ASP.NET!"


[!code-aspx[Main](urls-in-master-pages-cs/samples/sample5.aspx)]

Nach dieser Änderung auf die Masterseite, rufen Sie erneut die `~/Admin/Default.aspx` Seite erneut. Dieses Mal die `PoweredByASPNET.gif` Bilddatei, die auf der Seite angezeigt (siehe Abbildung 3). Wenn das Image-Web-Steuerelement ist gerendert es verwendet die `ResolveClientUrl` Verfahren zum Beheben der `ImageUrl` -Eigenschaftswert. In `~/Admin/Default.aspx` der `ImageUrl` in eine entsprechende relative URL wie im folgenden Codeausschnitt wird der HTML-Quelle konvertiert:


[!code-html[Main](urls-in-master-pages-cs/samples/sample6.html)]

> [!NOTE]
> Außer in URL-basierte Web-Steuerelementeigenschaften, die `~` kann auch verwendet werden, beim Aufrufen der `Response.Redirect` und `Server.MapPath` Methoden, u. a. Darüber hinaus die `ResolveClientUrl` Methode kann direkt aus einer ASP.NET- oder der Masterseite deklarativen Markup, bei Bedarf aufgerufen werden, finden Sie unter [Fritz Onion](https://www.pluralsight.com/blogs/fritz/)Blogeintrag [Using `ResolveClientUrl` im Markup](https://www.pluralsight.com/blogs/fritz/archive/2006/02/06/18596.aspx).


## <a name="fixing-the-master-pages-remaining-relative-urls"></a>Beheben die Masterseite verbleibende Relative URLs

Zusätzlich zu den `<img>` Element in der `footerContent` , dass wir gerade behoben, die Masterseite enthält eine weitere relativen URL, die unsere Aufmerksamkeit erfordert. Die `topContent` Region enthält, die Verknüpfung "Master-Seiten-Lernprogramme," verweist auf `Default.aspx`.


[!code-html[Main](urls-in-master-pages-cs/samples/sample7.html)]

Da diese URL relativ ist, sendet den Benutzer die `Default.aspx` Seite in den Ordner der Inhaltsseite besuchen. Haben Sie diesen Link, der immer auf zeigen `Default.aspx` im Stammordner wir ersetzen müssen die `<a>` Element mit einem HyperLink Web zu steuern, sodass wir verwenden können, die `~` Notation.

Entfernen Sie die `<a>` Elementknoten dem Elementmarkup und stattdessen ein Linksteuerelement hinzufügen. Legen Sie des Links des `ID` zu `lnkHome`, dessen `NavigateUrl` Eigenschaft `~/Default.aspx`, und die zugehörige `Text` Eigenschaft auf "Master-Seiten-Lernprogramme."


[!code-aspx[Main](urls-in-master-pages-cs/samples/sample8.aspx)]

Das ist alles! An diesem Punkt befinden sich alles, was die URLs in unserem Masterseite ordnungsgemäß basieren, wenn von einer Inhaltsseite, unabhängig davon, welche Ordner gerendert werden sollen, die Masterseite und die Seite "Inhalt" in.

### <a name="automatic-url-resolution-in-theheadsection"></a>URL für automatische Auflösung in die`<head>`Abschnitt

In der [ *erstellen eine standortweite Layout mithilfe von Master Pages* ](creating-a-site-wide-layout-using-master-pages-cs.md) Tutorial, die wir hinzugefügt, eine `<link>` auf die `Styles.css` Datei der `<head>` Region:


[!code-aspx[Main](urls-in-master-pages-cs/samples/sample9.aspx)]

Während der `<link>` des Elements `href` Attribut relativ ist, wird automatisch auf einen geeigneten Pfad zur Laufzeit konvertiert. Wie in erläutert die [ *Titel, Meta-Tags und anderer HTML-Header angeben, auf der Masterseite* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) Tutorial die `<head>` Region ist tatsächlich ein serverseitiges-Steuerelement, das die ändern, kann die der Inhalt der inneren Steuerelemente beim Rendern.

Um dies zu überprüfen, rufen Sie erneut die `~/Admin/Default.aspx` Seite, und zeigen Sie die an den Browser gesendete HTML-Quelle. Wie der folgende Codeausschnitt veranschaulicht, die `<link>` des Elements `href` Attribut automatisch so geändert wurde, eine entsprechende relative URL `../Styles.css`.


[!code-html[Main](urls-in-master-pages-cs/samples/sample10.html)]

## <a name="summary"></a>Zusammenfassung

Masterseiten enthalten sehr häufig, Links, Bilder und andere externen Ressourcen, die über eine URL angegeben werden müssen. Da der Masterseite und Inhaltsseiten nicht im selben Ordner vorhanden sein können, ist es wichtig, mithilfe von relativen URLs unterlassen. Es ist, zwar möglich, hartcodierte absolute URLs verwenden die absolute URL durch die enge ausführen an die Webanwendung verbindet. Wenn die absolute URL ändern, wie es häufig beim Verschieben oder zum Bereitstellen einer Web-Anwendung - - müssen Sie daran denken müssen, wechseln zurück und aktualisieren Sie die absolute URLs.

Der ideale Ansatz ist, verwenden Sie die Tilde (`~`) Stammverzeichnis der Anwendung an. Zuordnung der ASP.NET Web-Steuerelemente, die URL-bezogene Eigenschaften enthalten die `~` in das Stammverzeichnis der Anwendung zur Laufzeit. Intern wird die Web-Steuerelemente verwenden, die `Control` Klasse `ResolveClientUrl` Methode, um eine gültige relative URL zu generieren. Diese Methode ist, veröffentlichen und von jedem Serversteuerelement verfügbar (einschließlich der `Page` Klasse), damit Sie sie programmgesteuert aus Ihren Code-Behind-Klassen verwenden können bei Bedarf.

Viel Spaß beim Programmieren!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Tutorial erläutert finden Sie in den folgenden Ressourcen:

- [Masterseiten in ASP.NET](http://www.odetocode.com/Articles/419.aspx)
- [URL, die in einer Masterseite neuen Basisadressen](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/masterpages/default.aspx#urls)
- [Mithilfe von `ResolveClientUrl` im Markup](https://www.pluralsight.com/blogs/fritz/archive/2006/02/06/18596.aspx)

### <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor mehrerer Büchern zu ASP/ASP.NET und Gründer von 4GuysFromRolla.com, arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neuestes Buch heißt [ *Sams Teach selbst ASP.NET 3.5 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott erreicht werden kann, zur [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog unter [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Besonderen Dank an

Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Zurück](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md)
> [Weiter](control-id-naming-in-content-pages-cs.md)
