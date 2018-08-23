---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
title: 'Teil 3: Ansichten und ViewModels | Microsoft-Dokumentation'
author: jongalloway
description: Dieser tutorialreihe werden alle Schritte ausgeführt, um die ASP.NET MVC Music Store-beispielanwendung zu erstellen. Teil 3 sind die Ansichten und ViewModels.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 94297aa0-1f2d-4d72-bbcb-63f64653e0c0
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
msc.type: authoredcontent
ms.openlocfilehash: 828ff18abcc5932f82be71a45ebde589eeb051fa
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832702"
---
<a name="part-3-views-and-viewmodels"></a>Teil 3: Ansichten und ViewModels
====================
durch [Jon Galloway](https://github.com/jongalloway)

> Die MVC Music Store ist ein lernprogrammanwendung, die eingeführt und erläutert Schritt für Schritt, wie ASP.NET MVC und Visual Studio für die Webentwicklung verwenden.  
>   
> Die MVC Music Store ist eine Implementierung eines einfachen Beispiels die Alben online verkauft und implementiert grundlegende Verwaltung, Benutzeranmeldung und shopping Cart-Funktionalität.  
>   
> Dieser tutorialreihe werden alle Schritte ausgeführt, um die ASP.NET MVC Music Store-beispielanwendung zu erstellen. Teil 3 sind die Ansichten und ViewModels.


Bisher haben wir nur Zeichenfolgen aus Controlleraktionen Zurückgeben von wurde. Das ist eine gute Möglichkeit, Sie erhalten eine Vorstellung der Funktionsweise von Controllern, aber es ist nicht wie Sie möchten eine echte Webanwendung erstellen. Wir werden eine bessere Möglichkeit zum Generieren von HTML zurück zum Browser besuchen unsere Website möchten – ein, in dem wir Vorlagendateien verwenden können, um den HTML-Inhalt leichter anpassen, zurücksenden. Das ist genau das, was Ansichten tun.

## <a name="adding-a-view-template"></a>Hinzufügen einer Vorlage anzeigen

Um eine ansichtsvorlage zu verwenden, wir ändern die HomeController-Index-Methode, um ein Aktionsergebnis zurückgegeben und View(), wie unten zurückgegeben haben:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample1.cs)]

Die obige Änderung gibt an, dass anstelle eine Zeichenfolge zurückgegeben, wir möchten stattdessen eine "Ansicht" verwenden, um ein Ergebnis zurück zu generieren.

Wir werden unser Projekt nun eine geeignete ansichtsvorlage hinzufügen. Zu diesem Zweck wir positionieren Sie den Textcursor in den Index-Aktionsmethode, und klicken Sie dann mit der rechten Maustaste und wählen Sie "Ansicht hinzufügen". Hierdurch wird das Dialogfeld "Ansicht hinzufügen" angezeigt:

![](mvc-music-store-part-3/_static/image1.jpg) ![](mvc-music-store-part-3/_static/image1.png)

Das Dialogfeld "Ansicht hinzufügen" kann wir schnell und einfach Vorlagendateien für die Sicht zu generieren. Standardmäßig ist die "Ansicht hinzufügen" füllt Dialogfeld den Namen der Vorlage anzeigen, erstellen, damit diese die Aktionsmethode entspricht, die dazu verwendet werden. Da wir im Kontextmenü "Ansicht hinzufügen" innerhalb der Aktionsmethode Index() von unserem HomeController verwendet, hat das Dialogfeld "Ansicht hinzufügen" oben "Index" wie der Ansichtsname, der standardmäßig aufgefüllt. Wir müssen keine der Optionen in diesem Dialogfeld ändern möchten, klicken Sie daher auf die Schaltfläche "hinzufügen".

Wenn wir auf die Schaltfläche "hinzufügen" klicken, erstellt Visual Web Developer eine neue Datei "Index.cshtml" ansichtsvorlage für uns im Verzeichnis \Views\Home, erstellen den Ordner aus, wenn nicht bereits vorhanden ist.

![](mvc-music-store-part-3/_static/image2.png)

Der Name und Ordner Speicherort der Datei "Index.cshtml" ist wichtig, und die standardmäßige ASP.NET-MVC-Namenskonventionen entspricht. Der Name des Verzeichnisses, \Views\Home, die Controller - mit dem HomeController Namen entspricht. Vorlage Ansichtsnamen Index entspricht der Aktionsmethode des Controllers, die Ansicht anzeigen lassen.

ASP.NET MVC ermöglicht uns, zu vermeiden, dass den Namen oder Speicherort, der eine ansichtsvorlage explizit angeben, wenn wir diese Namenskonvention verwenden, um eine Ansicht zurückzugeben. Es werden standardmäßig die ansichtsvorlage \Views\Home\Index.cshtml gerendert, wenn wir in unserer HomeController z. B. folgenden Code schreiben:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample2.cs)]

Visual Web Developer erstellt und die ansichtsvorlage "Index.cshtml" geöffnet, nachdem wir die Schaltfläche "Hinzufügen" im Dialogfeld "Ansicht hinzufügen" geklickt wurde. Der Inhalt der Datei "Index.cshtml" werden unten angezeigt.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample3.cshtml)]

In dieser Ansicht wird die Razor-Syntax verwendet, die präziser als die Web Forms-Ansichts-Engine, die in ASP.NET Web Forms und früheren Versionen von ASP.NET MVC verwendet wird. Die Web Forms-Ansichts-Engine ist in ASP.NET MVC 3 weiterhin verfügbar, aber viele Entwickler finden Sie, dass die Razor-ansichtsengine ASP.NET MVC-Entwicklung wirklich gut geeignet ist.

Die ersten drei Zeilen fest mit ViewBag.Title Seitentitel Wir sehen Sie sich ausführlicher in Kürze, aber zuerst sehen wir Update Funktionsweise Text Überschrift und zeigen Sie die Seite. Update der &lt;h2&gt; Tag "Diese ist die Homepage" zu sagen, wie unten dargestellt.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample4.cshtml)]

Ausführen von der Anwendung zeigt, dass der neue Text auf der Startseite angezeigt wird.

![](mvc-music-store-part-3/_static/image3.png)

## <a name="using-a-layout-for-common-site-elements"></a>Verwenden ein Layout für allgemeine Website-Elemente

Die meisten Websites verfügen, Inhalt, der viele Seiten gemeinsam verwendet werden: Navigation, Fußzeilen, Logos, Stylesheet Verweise usw. Die Razor-ansichtsengine macht dies einfach zu verwalten, verwenden eine Seite mit dem \_Layout.cshtml, die im Ordner "/ Views/Shared" automatisch für uns erstellt wurde.

![](mvc-music-store-part-3/_static/image4.png)

Doppelklicken Sie auf diesen Ordner zum Anzeigen des Inhalts, sind unten aufgeführt.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample5.cshtml)]

Der Inhalt aus die einzelnen Ansichten angezeigt, auf die @RenderBody()-Befehl, und alle allgemeinen Inhalte, die wir außerhalb dieser angezeigt werden sollen können hinzugefügt werden die \_Layout.cshtml Markup. Sollten wir unsere MVC Music Store, um einen allgemeinen Header mit Links auf unserer Homepage und Store-Bereich auf allen Seiten der Website haben, damit wir, die für die Vorlage, die direkt über hinzufügen @RenderBody()-Anweisung.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample6.cshtml)]

## <a name="updating-the-stylesheet"></a>Das StyleSheet wird aktualisiert.

Die leere Projektvorlage enthält eine sehr optimierte CSS-Datei nur enthält Stile verwendet, um Nachrichten zur inhaltsprüfung anzuzeigen. Einige zusätzliche CSS- und Images um das Aussehen und Verhalten für unsere Website, zu definieren, damit wir diese jetzt hinzufügen, hat unsere Designer bereitgestellt.

Die aktualisierte CSS-Datei und die Bilder befinden sich im Verzeichnis mit Inhalt der MvcMusicStore Assets.zip, verfügbar unter [MVC-Musik-Store](https://github.com/evilDave/MVC-Music-Store). Wir wählen beide im Windows-Explorer und legen Sie sie in unserer Lösung für den Ordner "Content" in Visual Web Developer, wie unten dargestellt:

![](mvc-music-store-part-3/_static/image5.png)

Sie werden aufgefordert zu bestätigen, dass die vorhandene Datei "Site.CSS" überschrieben werden soll. Klicken Sie auf "Ja".

![](mvc-music-store-part-3/_static/image6.png)

Der Ordner "Content" der Anwendung wird nun wie folgt aussehen:

![](mvc-music-store-part-3/_static/image7.png)

Jetzt sehen wir die Anwendung auszuführen, und sehen, wie unsere Änderungen auf der Startseite aussieht.

![](mvc-music-store-part-3/_static/image8.png)

- Betrachten Sie die Änderungen: die HomeController-Index-Aktionsmethode gefunden und die Vorlage \Views\Home\Index.cshtmlView angezeigt, obwohl unseres Codes "return View()" bezeichnet, da es sich bei unserer Vorlage anzeigen, die standardmäßige Namenskonvention folgen.
- Auf der Startseite wird eine einfache Willkommensnachricht angezeigt, die die ansichtsvorlage \Views\Home\Index.cshtml definiert ist.
- Auf der Startseite verwendet unsere \_Layout.cshtml-Vorlage, und daher die Willkommensnachricht in der Standardwebsite HTML-Layout enthalten ist.

## <a name="using-a-model-to-pass-information-to-our-view"></a>Ein Modell verwenden, um Informationen an unsere Ansicht zu übergeben.

Eine ansichtsvorlage, die hartcodierte HTML nur zeigt ist nicht die eine Website sehr interessant machen soll. Um eine dynamische Website erstellen, sollten wir stattdessen zum Übergeben von Informationen über unsere Controlleraktionen an unsere Vorlagen anzeigen.

Im Model-View-Controller-Muster Objekte der Begriff, dem bezieht sich das Modell auf, die die Daten in der Anwendung darstellen. Häufig Modellobjekte Tabellen in der Datenbank entsprechen, aber sie müssen keine.

Controlleraktionsmethoden, die ein ActionResult zurückgeben können einem Modellobjekt an die Ansicht übergeben. Dies ermöglicht einem Controller sauber Paket alle Informationen erforderlich, um eine Antwort zu generieren und diese Informationen dann in eine ansichtsvorlage übergeben, mit die entsprechenden HTML-Antwort generiert. Dies ist am einfachsten zu verstehen, indem Sie diese in Aktion zu sehen, also lassen Sie uns loslegen.

Zunächst erstellen wir einige Modellklassen Genres und Alben innerhalb unseres neuen Speichers darstellen. Wir erstellen zunächst eine Klasse "Genre". Mit der rechten Maustaste in des Ordners "Models" in Ihrem Projekt, wählen Sie die Option "Klasse hinzufügen", und nennen Sie die Datei "Genre.cs".

![](mvc-music-store-part-3/_static/image2.jpg)

![](mvc-music-store-part-3/_static/image9.png)

Fügen Sie eine public String Name-Eigenschaft klicken Sie dann auf die Klasse, die erstellt wurde:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample7.cs)]

*Hinweis: In Fällen, die Sie Fragen sich, den {get; festlegen;} Notation besteht darin, Verwendung der Funktion für # automatisch implementierten Eigenschaften. Dadurch erhalten wir die Vorteile einer Eigenschaft, ohne uns um ein dahinter liegendes Feld zu deklarieren.*

Als Nächstes führen Sie die gleichen Schritte aus, um ein Album-Klasse, die (mit dem Namen Album.cs) zu erstellen, die einen Titel und eine Eigenschaft "Genre" verfügt:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample8.cs)]

Jetzt können wir durch Ändern der StoreController um Sichten zu verwenden, die dynamische Informationen von unserem Modell anzuzeigen. Wenn – zu Demonstrationszwecken jetzt – wir unsere Alben basierend auf die Anforderungs-ID mit dem Namen, können wir diese Informationen, wie in der Ansicht unten darstellen.

![](mvc-music-store-part-3/_static/image10.png)

Zunächst werden die Details der Store-Aktion ändern, sodass die Informationen für ein einzelnes Album angezeigt. Fügen Sie eine "using"-Anweisung am Anfang der **StoreControllers** Klasse den Namespace MvcMusicStore.Models einbeziehen, daher müssen wir nicht MvcMusicStore.Models.Album Geben Sie jedes Mal, wenn die Album-Klasse verwendet werden soll. Im Abschnitt "Using-Direktiven" dieser Klasse sollte jetzt angezeigt werden wie folgt.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample9.cs)]

Als Nächstes aktualisieren die Controlleraktion Details wir damit, dass ein ActionResult anstelle einer Zeichenfolge zurückgegeben wie wir die HomeController-Index-Methode haben.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample10.cs)]

Jetzt können wir die Logik zum Zurückgeben einer Album-Objekts an die Ansicht ändern. Weiter unten in diesem Tutorial wir wird werden zum Abrufen der Daten aus einer Datenbank – aber jetzt verwenden wir "Pseudoupdate von Daten" für den Einstieg.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample11.cs)]

*Hinweis: Wenn Sie nicht mit c# vertraut sind, können Sie davon ausgehen, dass es sich bei Verwenden von Var bedeutet, dass die Variable Album spät gebunden ist. Das ist nicht richtig – c#-Compiler verwendet den Typrückschluss basierend auf was wir die Variable zuweisen, um zu bestimmen, dass der Typ Album jeweiligen Album zu sehen ist und die lokalen Album-Variable als einen Typ Album, kompilieren, sodass kompilierzeitüberprüfung und Visual Studio Code-Editors abrufen unterstützt.*

Nun erstellen wir eine Vorlage anzeigen, die unsere Album verwendet, um eine HTML-Antwort zu generieren. Zunächst müssen wir das Projekt zu erstellen, damit das Dialogfeld "Ansicht hinzufügen" unserer Klasse der neu erstellten Album bekannt sind. Sie können das Projekt erstellen durch Auswählen der Debug⇨Build MvcMusicStore Menüelement (zusätzliche Anerkennung, können Sie die Tastenkombination STRG + UMSCHALT + B zum Erstellen des Projekts verwenden).

![](mvc-music-store-part-3/_static/image11.png)

Nun, da wir unsere unterstützenden Klassen eingerichtet haben, können wir unsere Vorlage für die Ansicht zu erstellen. Mit der rechten Maustaste in die Details-Methode, und wählen Sie "Ansicht hinzufügen" aus dem Kontextmenü aus.

![](mvc-music-store-part-3/_static/image12.png)

Wir werden eine neue, wie zuvor mit dem HomeController anzeigen-Vorlage erstellen. Da wir es aus der StoreController erstellen wird er standardmäßig in einer Datei \Views\Store\Index.cshtml erstellt werden.

Im Gegensatz zu werden, wir das "Erstellen, die einen stark typisierten" Ansicht-Kontrollkästchen aktivieren. Dann werden wir unsere Klasse "Album" in der "Ansicht-Datenklasse" Drop-Downlist auswählen. Dadurch wird das Dialogfeld "Ansicht hinzufügen" zum Erstellen einer Vorlage anzeigen, die von der erwartet, dass ein Album, die Objekt an ihn zur Verwendung übergeben wird.

![](mvc-music-store-part-3/_static/image13.png)

Beim Klicken auf die Schaltfläche "Hinzufügen" wird die Vorlage unserer \Views\Store\Details.cshtml anzeigen erstellt werden, mit dem folgenden Code.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample12.cshtml)]

Beachten Sie, dass der ersten Zeile gibt an, dass diese Ansicht auf unsere Album-Klasse stark typisiert ist. Die Razor-Ansichts-Engine erkennt, dass es ein Album-Objekt, übergeben wurde also können wir ganz einfach Zugriff auf Eigenschaften des Modells und sogar den Vorteil von IntelliSense im Visual Web Developer-Editor haben.

Update der &lt;h2&gt; markieren, damit es zeigt jedoch das Album des Title-Eigenschaft ändern diese Zeile folgendermaßen angezeigt werden.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample13.cshtml)]

Beachten Sie, dass IntelliSense ausgelöst wird, wenn Sie den Zeitraum nach dem Eingeben der @Model -Schlüsselwort, mit den Eigenschaften und Methoden, die die Album-Klasse unterstützt.

Lassen Sie uns nun führen Sie das Projekt erneut aus und rufen Sie die URL der Store/Details/5. Wir sehen die Details eines Albums wie unten.

![](mvc-music-store-part-3/_static/image14.png)

Stellen Sie jetzt eine ähnliche Aktualisierung für die Aktionsmethode Store durchsuchen. Aktualisieren Sie die Methode so, dass ein Aktionsergebnis zurückgegeben, und ändern Sie die Logik der Methode aus, damit es ein neues Objekt für "Genre erstellt" und gibt ihn an die Ansicht zurück.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample14.cs)]

Mit der rechten Maustaste in der durchsuchen-Methode, und wählen "Ansicht hinzufügen" aus dem Kontextmenü, klicken Sie dann eine Ansicht, die stark typisierte Hinzufügen der Klasse "Genre" eine stark typisierte hinzufügen.

![](mvc-music-store-part-3/_static/image15.png)

Update der &lt;h2&gt; Element in der Ansicht code (in /Views/Store/Browse.cshtml) zum Anzeigen der Informationen für "Genre".

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample15.cshtml)]

Jetzt sehen wir führen Sie das Projekt erneut aus, und navigieren Sie zu dem/Store/durchsuchen? Genre = Disco-URL. Wir sehen, dass die Seite "Durchsuchen" wie unten angezeigt.

![](mvc-music-store-part-3/_static/image16.png)

Abschließend soll ein etwas komplexes Update für die **Store Index** Aktionsmethode und können Sie eine Liste mit allen Genres in unseres neuen Speichers anzeigen. Wir werden dies über eine Liste der Genres als das Modellobjekt, anstatt nur eine einzelne "Genre".

[!code-csharp[Main](mvc-music-store-part-3/samples/sample16.cs)]

Mit der rechten Maustaste in der Store-Index-Aktionsmethode, und wählen Sie die Ansicht hinzufügen, wie zuvor, wählen "Genre" als die Model-Klasse, und drücken Sie die Schaltfläche "hinzufügen".

![](mvc-music-store-part-3/_static/image17.png)

Zuerst ändern wir die @model Deklaration, um anzugeben, dass die Ansicht, verschiedene "Genre" erwartet werden Objekte, anstatt nur eine. Ändern Sie die erste Zeile der /Store/Index.cshtml wie folgt:

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample17.cshtml)]

Dadurch wird der Razor-ansichtsengine, die es einem Modellobjekt verwendet werden, die mehrere "Genre"-Objekte enthalten kann. Wir verwenden eine IEnumerable&lt;"Genre"&gt; anstelle einer Liste&lt;"Genre"&gt; , da es mehr generisch ist, ermöglicht uns, unsere Modelltyp später auf einen beliebigen Objekttyp ändern, die die IEnumerable-Schnittstelle unterstützt.

Als Nächstes müssen wir die Objekte "Genre" in das Modell, wie in den abgeschlossen-Code unten gezeigt durchlaufen.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample18.cshtml)]

Beachten Sie, dass die vollständige IntelliSense-Unterstützung haben wir diesem Code eingeben, damit wir bei der Eingabe "@Model." sehen Sie alle Methoden und Eigenschaften, die durch ein IEnumerable vom Typ "Genre" unterstützt.

![](mvc-music-store-part-3/_static/image18.png)

In unserem "Foreach"-Schleife weiß Visual Web Developer an, dass jedes Element des Typs "Genre", sodass wir IntelliSense für die einzelnen finden Sie unter den Typ "Genre".

![](mvc-music-store-part-3/_static/image19.png)

Als Nächstes gerüstfeature untersucht das Genre-Objekt und bestimmt, von denen jeder eine Name-Eigenschaft, wird damit durchläuft und Dateien schreibt. Es generiert auch bearbeiten, Details und Löschen von Links auf jedes einzelne Element. Werfen wir nutzen, die später in unsere Geschäftsführer, aber jetzt möchten wir eine einfache Liste stattdessen zu haben.

Wenn wir die Anwendung auszuführen, und navigieren Sie zu/Store, sehen wir, dass die Anzahl und die Liste der Genres wird angezeigt.

![](mvc-music-store-part-3/_static/image20.png)

## <a name="adding-links-between-pages"></a>Hinzufügen von Verknüpfungen zwischen Seiten

Unsere/Store-URL, die Genres derzeit listet Listet die "Genre"-Namen als nur-Text. Lassen Sie uns dies ändern, damit anstelle von nur-Text wir stattdessen den Link "Genre" Namen, auf die entsprechende URL Store/durchsuchen, haben und durch Klicken auf eine Musik "Genre" wie "Disco" zu der/Store/durchsuchen navigiert? Genre = Disco-URL. Wir konnten unsere \Views\Store\Index.cshtml ansichtsvorlage aktualisieren, Ausgabe, die diesen Links, mit wie unten Code **(nicht in Typ – wir werden diese verbessern)**:

[!code-html[Main](mvc-music-store-part-3/samples/sample19.html)]

Dies funktioniert, jedoch kann dies zu einem späteren Zeitpunkt Problemen, da dabei auf eine hartcodierte Zeichenfolge. Wenn wir den Controller umbenennen möchten, müssten wir z. B. durchsuchen Sie unser Code nach Links, die aktualisiert werden müssen.

Eine alternative Methode, die wir verwenden können, ist eine HTML-Hilfsmethode nutzen. ASP.NET MVC enthält HTML-Hilfsmethoden, die über unsere Vorlagencode anzeigen, um eine Vielzahl von Aufgaben wie folgt ausführen verfügbar sind. Die Hilfsmethode Html.ActionLink() ist besonders nützlich, und erleichtert Ihnen die Erstellung von HTML &lt;eine&gt; verknüpft und übernimmt die lästigen Details wie das sicherstellen, dass URL-Pfade sind ordnungsgemäß URL-codiert.

Html.ActionLink() verfügt über mehrere verschiedene Überladungen zur Angabe so viele Informationen, wie Sie für Ihre Links benötigen. Im einfachsten Fall müssen Sie angeben nur der Text des Links und die Aktionsmethode auf, wenn auf den Link geklickt wird, auf dem Client. Beispielsweise können wir mit "/ Store /" Index()-Methode auf der Seite "Store-Details" mit der Text des Links "Wechseln Sie zu der Store Index" mit dem folgenden Aufruf verknüpfen:

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample20.cshtml)]

*Hinweis: In diesem Fall nicht haben wir müssen den Namen des Controllers an, da wir nur zu einer anderen Aktion im selben Controller erstellen, die die aktuelle Ansicht rendert.*

Unsere Links auf der Seite "Durchsuchen" müssen jedoch einen Parameter übergeben, deshalb verwenden wir eine andere Überladung der Html.ActionLink-Methode, die drei Parameter akzeptiert:

- 1. Text des Links, der den Namen "Genre" angezeigt wird
- 2. Aktionsname "Controller" (Durchsuchen)
- 3. Routenparameterwerten, sowohl den Namen ("Genre") und den Wert ("Genre"-Name) angeben

Platzieren, dass alle zusammen sieht wie wir diese Links an den Store Index schreiben:

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample21.cshtml)]

Jetzt Wenn wir führen das Projekt erneut aus, und greifen Sie auf die URL /Store/ sehen wir eine Liste von Genres. Jedes Genre wird ein Link – beim Klicken auf dauert es uns, unsere/Store/durchsuchen? Genre =*["Genre"]* URL.

![](mvc-music-store-part-3/_static/image3.jpg)

Der HTML-Code für die Liste "Genre" sieht folgendermaßen aus:

[!code-html[Main](mvc-music-store-part-3/samples/sample22.html)]


> [!div class="step-by-step"]
> [Zurück](mvc-music-store-part-2.md)
> [Weiter](mvc-music-store-part-4.md)
