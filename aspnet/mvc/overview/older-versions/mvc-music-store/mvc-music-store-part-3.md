---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
title: 'Teil 3: Ansichten und ViewModels | Microsoft Docs'
author: jongalloway
description: Diese Reihe von Lernprogrammen details aller die Schritte zum Erstellen von ASP.NET MVC-Music Store-beispielanwendung. Teil 3 werden die Ansichten und ViewModels behandelt.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 94297aa0-1f2d-4d72-bbcb-63f64653e0c0
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
msc.type: authoredcontent
ms.openlocfilehash: 497c2898db2e03b0650982c3ad1e6b5ac5ca639d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874849"
---
<a name="part-3-views-and-viewmodels"></a>Teil 3: Ansichten und ViewModels
====================
durch [Jon Galloway](https://github.com/jongalloway)

> Das MVC-Music Store ist eine lernprogrammanwendung, die führt und erklärt schrittweise, wie mithilfe von ASP.NET MVC und Visual Studio für die Webentwicklung.  
>   
> Das MVC-Music Store handelt es sich um einfache Beispiel Store Implementierung, die online Musikalben verkauft und implementiert grundlegende standortverwaltung Benutzer anmelden und shopping Cart-Funktionalität.  
>   
> Diese Reihe von Lernprogrammen details aller die Schritte zum Erstellen von ASP.NET MVC-Music Store-beispielanwendung. Teil 3 werden die Ansichten und ViewModels behandelt.


Bisher haben wir nur Zeichenfolgen aus Controlleraktionen Zurückgeben von wurde. Dies ist eine gute Möglichkeit, erhalten einen Überblick über die Funktionsweise von Controllern allerdings handelt es sich nicht wie Sie möchten eine echte Webanwendung zu erstellen. Kegel zum eine bessere Möglichkeit zum Generieren von HTML zurück an den Browser besuchen Sie unsere Website soll – eines, in dem wir Vorlagendateien verwenden können, den HTML-Inhalt leichter anpassen, zurücksenden. Das ist genau vorgehen Ansichten.

## <a name="adding-a-view-template"></a>Hinzufügen einer Vorlage für eine Sicht

Um eine Ansicht-Vorlage verwenden, wir ändern die HomeController Index-Methode, um ein ActionResult zurückzugeben und View(), wie im folgenden zurückgeben lassen:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample1.cs)]

Die oben genannten Änderung gibt an, dass anstelle eine Zeichenfolge zurückgegeben, die wir stattdessen eine "View" zu verwenden, um ein Ergebnis zurück generieren möchten.

Jetzt müssen wir eine entsprechende Ansichtenvorlage unsere Projekt hinzufügen. Zu diesem Zweck wir positionieren Sie den Textcursor innerhalb der Index-Aktionsmethode, und klicken Sie dann mit der rechten Maustaste fort und wählen "Ansicht hinzufügen". Hierdurch wird das Dialogfeld "Ansicht hinzufügen" angezeigt:

![](mvc-music-store-part-3/_static/image1.jpg) ![](mvc-music-store-part-3/_static/image1.png)

Das Dialogfeld "Ansicht hinzufügen" kann wir schnell und einfach Vorlagendateien Ansicht zu generieren. "Ansicht hinzufügen" in der Standardeinstellung bereits im Vorfeld gefüllt Dialogfeld den Namen der Sicht Vorlage zum Erstellen, damit er die Aktionsmethode findet, die verwendet werden. Da wir die Aktionsmethode Index() von unseren HomeController im Kontextmenü "Ansicht hinzufügen" verwendet, hat das Dialogfeld "Ansicht hinzufügen" "Ressourcen" "Index", wie der Ansichtsname standardmäßig im Voraus ausgefüllt. Wir müssen eine der Optionen in diesem Dialogfeld ändern möchten, klicken Sie auf die Schaltfläche "hinzufügen".

Wenn wir die Schaltfläche "hinzufügen" klicken, erstellt Visual Web Developer ein neues Index.cshtml Vorlage anzeigen für uns im Verzeichnis \Views\Home, erstellen den Ordner aus, wenn nicht bereits vorhanden ist.

![](mvc-music-store-part-3/_static/image2.png)

Der Name und Ordner Speicherort der Datei "Index.cshtml" ist wichtig und folgt die Standard-ASP.NET-MVC-Benennungskonventionen. Der Name des Verzeichnisses, \Views\Home, den Controller - mit dem Namen HomeController übereinstimmt. Vorlage Ansichtsnamen Index entspricht der Aktionsmethode des Controllers, die Ansicht anzeigen werden.

ASP.NET MVC erlaubt es uns zu vermeiden, dass den Namen oder Speicherort der Vorlage für eine Sicht explizit angeben, wenn wir diese Benennungskonvention verwenden, um eine Sicht zurückzugeben. Standardmäßig wird die \Views\Home\Index.cshtml Ansichtenvorlage gerendert, wenn wir in unserer HomeController Code wie im folgenden schreiben:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample2.cs)]

Visual Web Developer erstellt und die View-Vorlage "Index.cshtml" geöffnet, nachdem es in das Dialogfeld "Ansicht hinzufügen" auf "Hinzufügen" geklickt. Der Inhalt des Index.cshtml werden unten gezeigt.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample3.cshtml)]

In dieser Ansicht wird die Razor-Syntax, wobei präziser als die Web Forms-Ansichtsmodul in ASP.NET Web Forms- und früheren Versionen von ASP.NET MVC verwendet wird. Das WebForms-Anzeigemodul weiterhin in ASP.NET MVC 3 verfügbar ist, aber viele Entwickler gefunden werden, dass das Razor-Ansichtsmodul ASP.NET MVC-Entwicklung sehr gut geeignet ist.

Die ersten drei Zeilen Festlegen der Seitenname ViewBag.Title verwenden. Wir sehen Sie sich ausführlicher bald jedoch zunächst sehen wir Update Funktionsweise der Überschriftentext Text und die Seite anzuzeigen. Update der &lt;h2&gt; Tag "Diese ist die Startseite" sagen, wie unten dargestellt.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample4.cshtml)]

Ausführen von die Anwendung zeigt, dass unsere neue Text auf der Startseite angezeigt wird.

![](mvc-music-store-part-3/_static/image3.png)

## <a name="using-a-layout-for-common-site-elements"></a>Verwenden ein Layout für allgemeine-Websiteelementen

Die meisten Websites haben Inhalte, die viele Seiten gemeinsam verwendet werden: Navigation, Fußzeilen, Logos, Stylesheet Verweise usw. Das Razor-Ansichtsmodul ist ganz einfach, um die Verwaltung mithilfe einer Seite aufgerufen \_Layout.cshtml die/Ansichten/freigegeben Ordner automatisch für uns erstellt wurde.

![](mvc-music-store-part-3/_static/image4.png)

Doppelklicken Sie auf diesen Ordner zum Anzeigen des Inhalts, sind unten aufgeführt.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample5.cshtml)]

Wird der Inhalt aus unserem einzelnen Ansichten angezeigt werden, durch die @RenderBodyBefehl (), und alle allgemeinen Inhalte, die wir außerhalb, die angezeigt werden sollen hinzugefügt werden die \_Layout.cshtml Markup. Sollten wir unsere MVC Music Store, einen allgemeinen Header mit Links in unsere Startseite und Store Bereich auf allen Seiten der Website, damit wir, die an der Vorlage, die direkt über hinzufügen @RenderBody()-Anweisung.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample6.cshtml)]

## <a name="updating-the-stylesheet"></a>Aktualisieren das StyleSheet

Die leere Projektvorlage enthält eine sehr optimierte CSS-Datei, die Stile, die zur Anzeige von validierungsmeldungen enthält nur. Unsere Designer bereitgestellt hat, einige zusätzliche CSS und Bilder um das Aussehen und Verhalten für unsere Site zu definieren, damit wir diese jetzt hinzufügen.

Die aktualisierte CSS-Datei und die Bilder befinden sich das Inhaltsverzeichnis der MvcMusicStore Assets.zip unter [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store). Wir wählen beide Zertifikate im Windows-Explorer und löschen sie in der Projektmappe Ordners "Content" in Visual Web Developer, wie unten dargestellt:

![](mvc-music-store-part-3/_static/image5.png)

Sie werden aufgefordert, die zu überprüfen, ob die vorhandene Datei "Site.CSS" überschrieben werden soll. Klicken Sie auf "Ja".

![](mvc-music-store-part-3/_static/image6.png)

Die Ordners "Content" der Anwendung wird nun wie folgt aussehen:

![](mvc-music-store-part-3/_static/image7.png)

Jetzt sehen wir die Anwendung auszuführen, und sehen, wie sich unsere Änderungen an der auf der Startseite angezeigt.

![](mvc-music-store-part-3/_static/image8.png)

- Betrachten wir, was geändert wird: der HomeController Index Aktionsmethode gefunden, und die Vorlage \Views\Home\Index.cshtmlView angezeigt, obwohl getesteten Codes "return View()" bezeichnet, da unsere Vorlage Anzeigen der standardmäßige Benennungskonvention folgt.
- Auf der Startseite ist eine einfache Begrüßungsnachricht anzeigen, die innerhalb der \Views\Home\Index.cshtml Ansichtenvorlage definiert ist.
- Auf der Startseite verwendeten unsere \_Layout.cshtml-Vorlage, und damit die Willkommensnachricht innerhalb der standardmäßigen Website HTML-Layout enthalten ist.

## <a name="using-a-model-to-pass-information-to-our-view"></a>Ein Modell verwenden, um Informationen an unsere Ansicht zu übergeben.

Eine Vorlage anzeigen, die hartcodierte HTML nur zeigt ist nicht im Begriff, eine Website besonders interessant machen. Um eine dynamische Website erstellen, sollten wir stattdessen Informationen aus unserer Controlleraktionen an unsere Ansichtsvorlagen zu übergeben.

In der Model-View-Controller-Muster Objekte der Begriff, dem Modell bezieht sich auf, die Daten in der Anwendung darstellt. Häufig Modellobjekte Tabellen in der Datenbank entsprechen, sie jedoch nicht.

Controlleraktionsmethoden ein Aktionsergebnis zurückgegeben können Model-Objekts an die Ansicht übergeben. Dies ermöglicht einem Controller ordnungsgemäß Paket die Informationen, die erforderlich, um eine Antwort zu generieren, und übergeben dann diese Informationen in eine Vorlage anzeigen, mit die entsprechenden HTML-Antwort generiert. Dies ist am einfachsten, Einblicke durch diese Aktion zu sehen, wir also beginnen.

Erstellen Sie zunächst einige Modellklassen zur Darstellung von Genres und Alben in unserem Store. Beginnen Sie durch Erstellen einer "Genre"-Klasse. Mit der rechten Maustaste in des Ordners "Modelle" in Ihrem Projekt, wählen Sie die Option "Klasse hinzufügen", und nennen Sie die Datei "Genre.cs".

![](mvc-music-store-part-3/_static/image2.jpg)

![](mvc-music-store-part-3/_static/image9.png)

Fügen Sie eine öffentliche Zeichenfolge Name-Eigenschaft klicken Sie dann auf die Klasse, die erstellt wurde:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample7.cs)]

*Hinweis: In Fällen, die Sie Fragen sich, die {abrufen; festlegen;} Notation macht # automatisch implementierte Eigenschaften-Funktion. Dadurch erhalten Sie die Vorteile einer Eigenschaft, ohne dass uns eine dahinter liegende Feld nicht deklariert werden.*

Führen Sie anschließend die Schritten, um eine Album-Klasse, die (mit dem Namen Album.cs) zu erstellen, die einen Titel und eine Eigenschaft "Genre" aufweist:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample8.cs)]

Jetzt können wir die StoreController um Sichten zu verwenden, die dynamische Informationen aus unserem Modell anzeigen, ändern. Wenn – zu Demonstrationszwecken Moment - wir unsere Alben basierend auf der Anforderungs-ID mit dem Namen, konnte die Informationen wie in der folgenden Ansicht angezeigt werden.

![](mvc-music-store-part-3/_static/image10.png)

Wir beginnen mit der Aktion Store Details ändern, damit sie die Informationen für eine einzelnes Album zeigt. Fügen Sie eine "using-Anweisung am Anfang der **StoreControllers** Klasse den Namespace MvcMusicStore.Models einbeziehen, daher brauchen wir MvcMusicStore.Models.Album Geben Sie jedes Mal, wenn die Album-Klasse verwendet werden soll. Im Abschnitt "Using-Direktiven" dieser Klasse sollte jetzt angezeigt werden folgendermaßen aus.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample9.cs)]

Als Nächstes werden wir Controlleraktion Details aktualisieren, sodass ein Aktionsergebnis dar, sondern eine Zeichenfolge zurückgegeben wie wir mit der HomeController Index-Methode.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample10.cs)]

Jetzt können wir die Logik zum Zurückgeben eines Album-Objekts auf die Ansicht ändern. Weiter unten in diesem Lernprogramm wird werden abgerufenen Daten aus einer Datenbank – für jetzt wir verwenden jedoch "dummy Daten" für den Einstieg.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample11.cs)]

*Hinweis: Wenn Sie nicht mit c# vertraut sind, können Sie voraussetzen, dass mit Var bedeutet, dass unsere Album Variable spät gebunden. Das ist nicht richtig – der C#-Compiler wird mithilfe von Typrückschluss basierend auf was wir zum Ermitteln des Typs Album Album wird der Variablen zuweisen und die lokale Album-Variable als einen Typ Album kompilieren, damit wir kompilierzeitüberprüfung und Visual Studio Code-Editors erhalten unterstützt.*

Jetzt erstellen wir eine Vorlage anzeigen, die unsere Album verwendet, um eine HTML-Antwort zu generieren. Bevor wir dies tun müssen wir das Projekt zu erstellen, sodass das Dialogfeld "Ansicht hinzufügen" zu unseren neu erstellten Album Klasse erkennt. Sie können das Projekt erstellen durch Auswählen der Debug⇨Build MvcMusicStore Menüelement (für zusätzliche Guthaben können Sie die STRG-UMSCHALT-B-Verknüpfung zum Erstellen des Projekts verwenden).

![](mvc-music-store-part-3/_static/image11.png)

Nachdem wir unsere unterstützenden Klassen eingerichtet haben, können wir unsere Ansicht-Vorlage erstellen. Mit der rechten Maustaste innerhalb der Methode Details, und wählen Sie "Ansicht hinzufügen" aus dem Kontextmenü.

![](mvc-music-store-part-3/_static/image12.png)

Kegel zum Erstellen einer neuen Ansichtenvorlage aus, wie wir mit HomeController hat sich nichts geändert. Da wir es aus der StoreController erstellen wird er standardmäßig in einer Datei \Views\Store\Index.cshtml erstellt werden.

Im Gegensatz zu werden, wir überprüfen Sie das Kontrollkästchen für "Erstellen, die einen stark typisierten" anzeigen. Dann werden wir unsere "Album"-Klasse in der Drop-Downlist von "-Datenklasse anzeigen" auswählen. Dadurch wird das Dialogfeld "Ansicht hinzufügen" erstellen eine Ansichtenvorlage, die der erwartet, dass ein Album Objekt hinzu, um verwenden übergeben wird.

![](mvc-music-store-part-3/_static/image13.png)

Beim Klicken auf die Schaltfläche "Hinzufügen" wird unsere \Views\Store\Details.cshtml Ansicht-Vorlage erstellt werden, mit dem folgenden Code.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample12.cshtml)]

Beachten Sie die erste Zeile, gibt an, dass diese Ansicht Album-Klasse stark typisiert ist. Das Razor-Ansichtsmodul erkennt, dass es ein Album-Objekt übergeben wurde, damit wir können Zugriff auf einfache Weise die Modelleigenschaften und auch die Vorteile von IntelliSense im Visual Web Developer-Editor.

Update der &lt;h2&gt; kennzeichnen, damit es dem Album Title-Eigenschaft zeigt an, durch Ändern dieser Zeile wie folgt angezeigt werden sollen.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample13.cshtml)]

Beachten Sie, dass IntelliSense ausgelöst wird, wenn Sie den Zeitraum nach dem Eingeben der @Model -Schlüsselwort, zeigt die Eigenschaften und Methoden, die die Album-Klasse unterstützt.

Wir nun unsere Projekt erneut ausführen und besuchen Sie diese Store/Informationen/5. Wir sehen die Details der ein Album wie unten.

![](mvc-music-store-part-3/_static/image14.png)

Jetzt stellen wir eine ähnliche Aktualisierung für die Aktionsmethode Store durchsuchen. Aktualisieren Sie die Methode, sodass ein Aktionsergebnis zurückgegeben, und ändern Sie die Logik der Methode, sodass er ein neues Objekt für "Genre erstellt" und gibt ihn an die Sicht zurück.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample14.cs)]

Mit der rechten Maustaste in der Methode zum Durchsuchen, und wählen Sie "Ansicht hinzufügen" Klicken Sie im Kontextmenü, fügen Sie dann eine Ansicht, stark typisiert ist, eine stark typisierte der "Genre" Klasse hinzufügen.

![](mvc-music-store-part-3/_static/image15.png)

Update der &lt;h2&gt; Element in der Ansicht code (im /Views/Store/Browse.cshtml), um "Genre" Informationen anzuzeigen.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample15.cshtml)]

Jetzt sehen wir führen Sie das Projekt erneut aus, und navigieren Sie zu der/Store/durchsuchen? "Genre" = Disco-URL. Wir sehen, dass die Seite "Durchsuchen" wie unten angezeigt.

![](mvc-music-store-part-3/_static/image16.png)

Zum Schluss stellen ein etwas komplexes Update für die **Index speichern** Aktionsmethode und können Sie eine Liste mit allen Genres in unserem Shop anzeigen. Wir müssen zu diesem Zweck verwenden eine Liste von Genres als unsere Modellobjekt, anstatt nur einen einzelnen "Genre".

[!code-csharp[Main](mvc-music-store-part-3/samples/sample16.cs)]

Mit der rechten Maustaste in den Columnstore-Index-Aktionsmethode, und wählen Sie die Ansicht hinzufügen, wie zuvor, wählen "Genre" als der Skriptobjektmodell-Klasse, und drücken Sie die Schaltfläche "hinzufügen".

![](mvc-music-store-part-3/_static/image17.png)

Ändern wir zunächst die @model Deklaration, um anzugeben, dass die Sicht mehrere "Genre" erwartet werden, werden Objekte, anstatt nur einen. Ändern Sie die erste Zeile der /Store/Index.cshtml wie folgt:

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample17.cshtml)]

Dies weist das Razor-Ansichtsmodul aus, dem sie einem Modellobjekt arbeiten, die mehrere "Genre"-Objekte enthalten kann. Wir verwenden eine IEnumerable&lt;"Genre"&gt; anstelle einer Liste&lt;"Genre"&gt; , da es mehr generisch ist, sodass wir unsere Modelltyp später auf einen beliebigen Objekttyp zu ändern, die IEnumerable-Schnittstelle unterstützt.

Als Nächstes müssen wir die "Genre"-Objekte im Modell wie im Ansichtscode unten abgeschlossenen dargestellt durchlaufen.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample18.cshtml)]

Beachten Sie, dass wir vollständige IntelliSense-Unterstützung haben wir diesen Code eingeben, damit es bei der Eingabe "@Model." sehen Sie alle Methoden und Eigenschaften von IEnumerable vom Typ "Genre" unterstützt.

![](mvc-music-store-part-3/_static/image18.png)

Innerhalb der Schleife "Foreach" weiß Visual Web Developer an, dass jedes Element vom Typ "Genre" ist, damit wir IntelliSense für die einzelnen finden Sie unter den Typ "Genre".

![](mvc-music-store-part-3/_static/image19.png)

Als Nächstes die Gerüstbau-Funktion überprüft das Objekt "Genre" und ermittelt, dass jede eine Name-Eigenschaft verfügen wird, damit durchläuft und Dateien schreibt. Es generiert auch bearbeiten, Details und Löschen von Verknüpfungen auf jedes einzelne Element. Wir führen, die weiter unten in unsere Speicher-Manager nutzen, aber vorläufig wir hätten gerne eine einfache Liste stattdessen.

Wenn wir führen Sie die Anwendung, und navigieren Sie zu/Store, sehen wir, dass die Anzahl und die Liste von Genres wird angezeigt.

![](mvc-music-store-part-3/_static/image20.png)

## <a name="adding-links-between-pages"></a>Hinzufügen von Verknüpfungen zwischen Seiten

Unsere/Store-URL, die derzeit Genres listet führt die "Genre"-Namen einfach als nur-Text. Wir ändern, damit anstelle von nur-Text wir stattdessen die "Genre" Namen Link zur entsprechenden Store/durchsuchen-URL enthalten, damit durch Klicken auf einen Musik "Genre" wie "Disco" / Store/durchsuchen navigieren soll? "Genre" = Disco-URL. Wir konnten unseren \Views\Store\Index.cshtml Ansichtenvorlage Ausgabe, die diese Links mit wie unten Code aktualisiert **(Geben Sie hierzu finden Sie unter: Wir werden darauf zu verbessern)**:

[!code-html[Main](mvc-music-store-part-3/samples/sample19.html)]

Dies funktioniert, aber dies könnte zu später behandeln, da er auf eine hartcodierte Zeichenfolge angewiesen ist. Wenn wir, benennen Sie den Controller möchten, müssten wir z. B. durchsuchen getesteten Codes, die Suche nach Links, die aktualisiert werden müssen.

Eine alternative Methode, die verwendet werden können, besteht darin ein HTML-Hilfsmethode nutzen. ASP.NET MVC umfasst HTML-Hilfsmethoden aus unserem Vorlagencode Ansicht zum Ausführen einer Vielzahl von allgemeinen Aufgaben wie folgt zur Verfügung stehen. Die Hilfsmethode Html.ActionLink() ist eine besonders nützlich, und erleichtert das Erstellen von HTML &lt;ein&gt; verknüpft und übernimmt die ärgerlich Details, wie Sie sicherstellen, dass URL-Pfade sind ordnungsgemäß URL-codiert.

Html.ActionLink() has several different overloads to allow specifying as much information as you need for your links. Im einfachsten Fall müssen Sie angeben, nur der Text des Links und die Aktionsmethode soll, wenn auf dem Client auf der Link geklickt wird. Beispielsweise können wir mit "/ Store /" Index()-Methode auf der Detailseite für Speicher mit der Text des Links "Wechseln Sie zu der Columnstore-Index" mithilfe des folgenden Aufrufs verknüpfen:

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample20.cshtml)]

*Hinweis: In diesem Fall haben nicht wir müssen den Controllernamen angeben, da es nur an eine andere Aktion in den gleichen Controller verknüpfen, die die aktuelle Ansicht gerendert wird.*

Unsere Links auf der Seite "Durchsuchen" müssen jedoch einen Parameter zu übergeben, damit wir eine andere Überladung der Methode Html.ActionLink verwenden, die drei Parameter akzeptiert:

- 1. Text des Links, der den Namen "Genre" angezeigt wird
- 2. Aktionsname "Controller" (Durchsuchen)
- 3. Routenparameterwerten angeben den Namen ("Genre") und der Wert ("Genre"-Name)

Platzieren, dass alle zusammen sieht wie wir diese Links in die Columnstore-Index Ansicht schreiben müssen:

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample21.cshtml)]

Wenn wir unsere Projekt erneut auszuführen, und greifen auf die URL /Store/ wird wir nun eine Liste von Genres angezeigt. Jedes Genres steht ein Hyperlink – aus, wenn geklickt dauert es uns, unsere/Store/durchsuchen? "Genre" =*["Genre"]* URL.

![](mvc-music-store-part-3/_static/image3.jpg)

Der HTML-Code für die Liste "Genre" sieht wie folgt:

[!code-html[Main](mvc-music-store-part-3/samples/sample22.html)]


> [!div class="step-by-step"]
> [Zurück](mvc-music-store-part-2.md)
> [Weiter](mvc-music-store-part-4.md)
