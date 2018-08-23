---
uid: web-forms/overview/older-versions-getting-started/master-pages/master-pages-and-asp-net-ajax-cs
title: Masterseiten und ASP.NET AJAX (c#) | Microsoft-Dokumentation
author: rick-anderson
description: Erläutert die Optionen für die Verwendung von ASP.NET AJAX und Masterseiten. Untersucht, mit der Klasse "ScriptManagerProxy"; Erläutert, wie die verschiedenen JS-Dateien Dependi geladen werden...
ms.author: riande
ms.date: 07/11/2008
ms.assetid: 0c55eb66-ba44-4d49-98e8-5c87fd9b1111
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/master-pages-and-asp-net-ajax-cs
msc.type: authoredcontent
ms.openlocfilehash: 47201a0cfeb5d1e548721094d11488e9e804dc9c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838981"
---
<a name="master-pages-and-aspnet-ajax-c"></a>Masterseiten und ASP.NET AJAX (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](http://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_08_CS.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_08_CS.pdf)

> Erläutert die Optionen für die Verwendung von ASP.NET AJAX und Masterseiten. Untersucht, mit der Klasse "ScriptManagerProxy"; Erläutert, wie die verschiedenen JS-Dateien je nachdem, ob der ScriptManager, in der Masterdatenbank verwendet wird geladen werden, Seite oder die Seite "Inhalt".


## <a name="introduction"></a>Einführung

In den letzten Jahren immer mehr Entwickler erstellt haben [AJAX](http://en.wikipedia.org/wiki/Ajax_(programming))-fähigen Webanwendungen. Eine AJAX-fähige Website verwendet eine Reihe von verwandten webtechnologien um zu eine steigern der Reaktionsfähigkeit der Benutzeroberfläche zu bieten. Erstellen von AJAX-aktivierter ASP.NET-Anwendungen ist erstaunlich einfach Dank Microsoft [ASP.NET AJAX-Framework](../../../../ajax/index.md). ASP.NET AJAX ist in ASP.NET 3.5 und Visual Studio 2008 integriert. Es ist auch als separater Download für ASP.NET 2.0-Anwendungen verfügbar.

Wenn AJAX-aktivierten von Webseiten mit ASP.NET AJAX-Framework zu erstellen, müssen Sie hinzufügen, genau eine [ScriptManager-Steuerelement](https://msdn.microsoft.com/library/bb398863.aspx) auf jeder Seite, die das Framework verwendet. Wie der Name schon sagt, verwaltet der ScriptManager das clientseitige Skript in AJAX-fähige Webseiten verwendet. Zumindest gibt ScriptManager HTML, die den Browser anweist, zu den JavaScript-Dateien, Zusammensetzung der ASP.NET AJAX-Clientbibliothek herunterladen. Sie können auch verwendet werden, um benutzerdefinierte JavaScript-Dateien, Skript-fähige Webdienste und Dienstfunktionen für die benutzerdefinierte Anwendung zu registrieren.

Wenn Ihre Website verwendet Master-Seiten (wie gewünscht) die, müssen nicht unbedingt Sie jede einzelne Inhaltsseite ein ScriptManager-Steuerelement hinzu; Stattdessen können Sie die Masterseite ein ScriptManager-Steuerelement hinzufügen. Dieses Tutorial veranschaulicht, wie das ScriptManager-Steuerelement auf die Masterseite hinzufügen. Außerdem erläutert das wie des Steuerelements "ScriptManagerProxy" verwenden, um benutzerdefinierte Skripts und Skriptdienste in einer bestimmten Inhaltsseite zu registrieren.

> [!NOTE]
> In diesem Tutorial wird nicht untersuchen, entwerfen, oder Erstellen von AJAX-aktivierten Webanwendungen mit ASP.NET AJAX-Framework. Weitere Informationen zur Verwendung von AJAX finden Sie in der [ASP.NET AJAX-Videos](../../../videos/aspnet-ajax/index.md) und [Tutorials](../aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax.md)sowie wie diese Ressourcen, die im Abschnitt Weitere nützliche Informationen am Ende dieses Tutorials aufgeführt.


## <a name="examining-the-markup-emitted-by-the-scriptmanager-control"></a>Untersuchen das Markup ausgegeben, die vom ScriptManager-Steuerelement

Das ScriptManager-Steuerelement gibt Markup, das den Browser zum Herunterladen der JavaScript-Dateien, Zusammensetzung der ASP.NET AJAX-Clientbibliothek anweist. Es fügt auch etwas Inline-JavaScript auf der Seite, die diese Bibliothek initialisiert. Das folgende Markup zeigt den Inhalt, der die gerenderte Ausgabe einer Seite hinzugefügt wird, die ein ScriptManager-Steuerelement enthält:


[!code-html[Main](master-pages-and-asp-net-ajax-cs/samples/sample1.html)]

Die `<script src="url"></script>` Tags weisen Sie den Browser zum Herunterladen und Ausführen der JavaScript-Datei am *Url*. ScriptManager gibt drei solche Tags; eine verweist auf die Datei `WebResource.axd`, während die anderen beiden auf die Datei verweisen `ScriptResource.axd`. Diese Dateien als Dateien auf Ihrer Website eigentlich nicht vorhanden. Stattdessen, wenn eine Anforderung für eine dieser Dateien auf dem Webserver eingeht, das ASP.NET-Modul untersucht die Abfragezeichenfolge und gibt den entsprechenden JavaScript-Inhalt zurück. Das Skript, die von diesen drei externen JavaScript-Dateien bereitgestellten bilden die ASP.NET AJAX-Framework-Clientbibliothek. Die andere `<script>` Tags, die vom ScriptManager ausgegeben zählen Inline-Skript, die diese Bibliothek initialisiert.

Die externen Skript-Verweise und den Inline-Skript ausgegeben, die von ScriptManager sind entscheidend für eine Seite, die ASP.NET AJAX-Framework verwendet, aber ist nicht erforderlich, für Seiten, die das Framework nicht verwenden. Aus diesem Grund kann gedacht, dass sie ideal ist, nur ein ScriptManager-Steuerelement hinzufügen, die ASP.NET AJAX-Framework verwenden. Und dies ist ausreichend, aber wenn Sie über viele Seiten verfügen, mit denen das Framework erhalten Sie alle Seiten – eine sich wiederholende Aufgabe, gelinde gesagt das ScriptManager-Steuerelement hinzugefügt. Alternativ können Sie die Masterseite, die fügt dann diese notwendige Skript in alle Inhaltsseiten, ein ScriptManager-Steuerelement hinzufügen. Bei diesem Ansatz müssen Sie nicht Denken Sie daran, die ein ScriptManager-Steuerelement eine neue Seite hinzufügen, die ASP.NET AJAX-Framework verwendet werden, da er bereits von der Masterseite enthalten ist. Schritt 1 führt ein ScriptManager-Steuerelement auf die Masterseite hinzufügen.

> [!NOTE]
> Wenn Sie planen, auch eine AJAX-Funktion in der Benutzeroberfläche der Masterseite, dann haben Sie keine andere Wahl, die innerhalb – müssen Sie den ScriptManager auf der Masterseite einschließen.


Ein Nachteil von ScriptManager hinzufügen, auf die Masterseite ist, dass das obige Skript, im ausgegeben wird *jeder* Seite, unabhängig davon, ob er benötigt. Dies führt natürlich zu einer Verschwendung von Bandbreite für diese Seiten haben ScriptManager (über die Masterseite) enthalten, die noch keine Funktionen von ASP.NET AJAX-Framework verwenden. Aber wie viel Bandbreite wird verschwendet?

- Der eigentliche Inhalt von ScriptManager (siehe oben) ausgegebenen summiert etwas länger als 1KB.
- Die drei externen Skript-Dateien, die auf die verwiesen wird durch die `<script>` Element umfassen jedoch ungefähr 450 KB Daten nicht komprimiert, die auf einer Website, die Gzip-Komprimierung verwendet wird, diese gesamte Bandbreite reduziert werden kann, in der Nähe von 100 KB. Diese Skriptdateien wird jedoch vom Browser zwischengespeichert werden, ein Jahr lang, was bedeutet, dass sie nur einmal heruntergeladen werden müssen, und klicken Sie dann auf anderen Seiten auf der Website wiederverwendet werden können.

Im besten Fall wenn die Skriptdateien zwischengespeichert werden, erfolgt, die Gesamtkosten 1KB, die sind zu vernachlässigen. Im schlimmsten Fall jedoch – d. h. wenn die Skriptdateien nicht noch heruntergeladen wurden und der Webserver ist keine Art von Komprimierung verwenden – die Bandbreite erreicht wird, etwa 450KB, die aus einem oder zwei Sekunden einer Breitbandverbindung auf bis zu eine Minute für eine beliebige Stelle hinzufügen können  der Benutzer über DFÜ-Modems. Die gute Nachricht ist, da die externe Skriptdateien vom Browser zwischengespeichert werden, diese schlimmsten Fall nur selten auftritt.

> [!NOTE]
> Wenn Sie weiterhin Verlegenheit gebracht, platzieren das ScriptManager-Steuerelement auf der Masterseite glauben, sollten Sie das Web Form (die `<form runat="server">` Markup auf der Masterseite). Jeder ASP.NET-Seite, die das postback Modell verwendet, muss genau ein Web Form enthalten. Ein Web Form hinzufügen, fügt zusätzliche Inhalt hinzu: eine Reihe von ausgeblendeten Formularfeldern die `<form>` selbst markieren und, falls erforderlich, eine JavaScript-Funktion für das Initiieren eines Postbacks aus Skript. Dieses Markup ist für Seiten, die nicht von postback nicht erforderlich. Dieses überflüssige Markup kann ausgeschlossen werden, indem Sie das Web Form der Masterseite manuell es auf jeder Inhaltsseite, die dies erfordert. Die Vorteile der Nutzung der Web Form in die Masterseite überwiegen jedoch die Nachteile müssen sie bestimmte Inhaltsseiten unnötigerweise hinzugefügt.


## <a name="step-1-adding-a-scriptmanager-control-to-the-master-page"></a>Schritt 1: Hinzufügen ein ScriptManager-Steuerelement auf der Masterseite

Jede Webseite, die ASP.NET AJAX-Framework verwendet, muss genau ein ScriptManager-Steuerelement enthalten. Aufgrund dieser Anforderung ist es normalerweise sinnvoll, ein einzelnes ScriptManager-Steuerelement auf der Masterseite zu platzieren, sodass alle Inhaltsseiten über das ScriptManager-Steuerelement, das automatisch einbezogen haben. Darüber hinaus muss ScriptManager vor allen von der ASP.NET AJAX-Steuerelemente, z. B. den UpdatePanel- und UpdateProgress-Steuerelementen stammen. Aus diesem Grund empfiehlt es sich ScriptManager vor alle ContentPlaceHolder-Steuerelemente in das Web Form zu versetzen.

Öffnen der `Site.master` Masterseite, und fügen Sie ein ScriptManager-Steuerelement auf der Seite innerhalb der Web-Formulars, aber vor der `<div id="topContent">` Element (siehe Abbildung 1). Wenn Sie Visual Web Developer 2008 oder Visual Studio 2008 verwenden, befindet sich das ScriptManager-Steuerelement in der Toolbox auf die Registerkarte "AJAX-Erweiterungen". Wenn Sie Visual Studio 2005 verwenden, müssen Sie zunächst installieren ASP.NET AJAX-Framework und die Steuerelemente zur Toolbox hinzufügen. Besuchen Sie die [ASP.NET AJAX-Wiki](https://github.com/DevExpress/AjaxControlToolkit/wiki) um das Framework für ASP.NET 2.0 zu erhalten.

Ändern Sie nach dem Hinzufügen von ScriptManager auf der Seite, die `ID` aus `ScriptManager1` zu `MyManager`.


[![Hinzufügen von ScriptManager auf der Masterseite](master-pages-and-asp-net-ajax-cs/_static/image2.png)](master-pages-and-asp-net-ajax-cs/_static/image1.png)

**Abbildung 01**: Hinzufügen von ScriptManager auf der Masterseite ([klicken Sie, um das Bild in voller Größe anzeigen](master-pages-and-asp-net-ajax-cs/_static/image3.png))


## <a name="step-2-using-the-aspnet-ajax-framework-from-a-content-page"></a>Schritt 2: Verwenden der ASP.NET AJAX-Framework von einer Inhaltsseite

Beim ScriptManager-Steuerelement hinzugefügt, die Masterseite können wir jetzt ASP.NET AJAX-Framework-Funktionen einer Inhaltsseite hinzufügen. Wir erstellen eine neue ASP.NET-Seite, die ein zufällig ausgewähltes Produkt aus der Northwind-Datenbank angezeigt. Wir verwenden Timer-Steuerelement von ASP.NET AJAX-Framework, um diese Anzeige alle 15 Sekunden, die mit einem neuen Produkt zu aktualisieren.

Zunächst erstellen Sie eine neue Seite in das Stammverzeichnis, das mit dem Namen `ShowRandomProduct.aspx`. Vergessen Sie nicht, binden Sie diese neue Seite, um die `Site.master` Masterseite.


[![Fügen Sie eine neue ASP.NET-Seite auf der Website](master-pages-and-asp-net-ajax-cs/_static/image5.png)](master-pages-and-asp-net-ajax-cs/_static/image4.png)

**Abbildung 02**: Fügen Sie eine neue ASP.NET-Seite auf der Website ([klicken Sie, um das Bild in voller Größe anzeigen](master-pages-and-asp-net-ajax-cs/_static/image6.png))


Denken Sie daran, dass in der [ *Titel, Meta-Tags und anderer HTML-Header angeben, auf der Masterseite* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) Lernprogramm eine benutzerdefinierten Basisseite-Klasse, die mit dem Namen erstellten `BasePage` , die den Titel der Seite generiert, wenn es war nicht explizit festgelegt wurde. Wechseln Sie zu der `ShowRandomProduct.aspx` Seite des Code-Behind-Klasse und deren abgeleitet `BasePage` (anstelle von aus `System.Web.UI.Page`).

Aktualisieren Sie abschließend die `Web.sitemap` hinzu, um einen Eintrag in dieser Lektion einzubeziehen. Fügen Sie das folgende Markup unterhalb der `<siteMapNode>` für den Master zur Interaktion der Inhalt Seite Lektion:


[!code-xml[Main](master-pages-and-asp-net-ajax-cs/samples/sample2.xml)]

Das Hinzufügen dieses `<siteMapNode>` Elements in den Lektionen wiedergegeben wird (siehe Abbildung 5).

### <a name="displaying-a-randomly-selected-product"></a>Ein zufällig ausgewähltes Produkt anzeigen

Wechseln Sie zurück zur `ShowRandomProduct.aspx`. Ziehen Sie aus dem Designer, ein UpdatePanel-Steuerelement aus der Toolbox in die `MainContent` Inhaltssteuerelement aus, und legen Sie dessen `ID` Eigenschaft `ProductPanel`. UpdatePanel stellt dar, eine Region aus, auf dem Bildschirm, der asynchron über ein teilupdates für Seiten-Postback aktualisiert werden kann.

Unsere erste Aufgabe ist zum Anzeigen von Informationen zu einem zufällig ausgewählten Produkt innerhalb von UpdatePanel. Starten Sie von einem DetailsView-Steuerelement in UpdatePanel ziehen. Legen Sie das DetailsView-Steuerelement `ID` Eigenschaft `ProductInfo` und lösche die `Height` und `Width` Eigenschaften. Erweitern Sie DetailsViews-Smarttag und, wählen Sie aus der Dropdownliste wählen Sie die Datenquelle zum Binden von DetailsView an ein neues SqlDataSource-Steuerelement, das mit dem Namen `RandomProductDataSource`.


[![Binden von DetailsView an ein neues SqlDataSource-Steuerelement](master-pages-and-asp-net-ajax-cs/_static/image8.png)](master-pages-and-asp-net-ajax-cs/_static/image7.png)

**Abbildung 03**: DetailsView an ein neues SqlDataSource-Steuerelement zu binden ([klicken Sie, um das Bild in voller Größe anzeigen](master-pages-and-asp-net-ajax-cs/_static/image9.png))


Konfigurieren Sie die SqlDataSource-Steuerelement für die Verbindung zur Northwind-Datenbank über die `NorthwindConnectionString` (die wir in erstellt die [ *Interaktion mit der Masterseite der Inhaltsseite* ](interacting-with-the-content-page-from-the-master-page-cs.md) Tutorial). Wenn die select-Anweisung konfigurieren wählen Sie eine benutzerdefinierte SQL­Anweisung angeben, und geben die folgende Abfrage:


[!code-sql[Main](master-pages-and-asp-net-ajax-cs/samples/sample3.sql)]

Die `TOP 1` -Schlüsselwort in der `SELECT` die Klausel gibt nur den ersten von der Abfrage zurückgegebenen Datensatz zurück. Die [ `NEWID()` Funktion](https://msdn.microsoft.com/library/ms190348.aspx) generiert einen neuen [Wert globally unique Identifier (GUID)](http://en.wikipedia.org/wiki/Globally_Unique_Identifier) und kann verwendet werden, eine `ORDER BY` -Klausel, um der Tabelle Datensätze in zufälliger Reihenfolge zurückgegeben.


[![Konfigurieren von dem SqlDataSource-Steuerelement um einen einzelnen, zufällig ausgewählten Datensatz zurückgeben](master-pages-and-asp-net-ajax-cs/_static/image11.png)](master-pages-and-asp-net-ajax-cs/_static/image10.png)

**Abbildung 04**: Konfigurieren von dem SqlDataSource-Steuerelement zum Zurückgeben einer einzelnes, nach dem Zufallsprinzip ausgewählten Datensatz ([klicken Sie, um das Bild in voller Größe anzeigen](master-pages-and-asp-net-ajax-cs/_static/image12.png))


Nach Abschluss des Assistenten, erstellt Visual Studio eine BoundField für die zwei Spalten, die von der obigen Abfrage zurückgegeben. An diesem Punkt sollte deklaratives Markup Ihrer Seite etwa wie folgt aussehen:


[!code-aspx[Main](master-pages-and-asp-net-ajax-cs/samples/sample4.aspx)]

Abbildung 5 zeigt die `ShowRandomProduct.aspx` Seite, wenn Sie über einen Browser angezeigt. Klicken Sie auf die Aktualisierungsschaltfläche Ihres Browsers, um die Seite erneut zu laden. Daraufhin sollte die `ProductName` und `UnitPrice` Werte für einen neuen, zufällig ausgewählten Datensatz.


[![Name und Preis des Produkts, auf eine zufällige wird angezeigt.](master-pages-and-asp-net-ajax-cs/_static/image14.png)](master-pages-and-asp-net-ajax-cs/_static/image13.png)

**Abbildung 05**: eine zufällige Product Name und der Preis angezeigt ([klicken Sie, um das Bild in voller Größe anzeigen](master-pages-and-asp-net-ajax-cs/_static/image15.png))


### <a name="automatically-displaying-a-new-product-every-15-seconds"></a>Anzeigen von automatisch ein neues Produkt alle 15 Sekunden

ASP.NET AJAX-Framework umfasst ein Zeitgeber-Steuerelement, das zu einem bestimmten Zeitpunkt ein Postback ausführt. auf postback des Timers `Tick` Ereignis wird ausgelöst. Wenn das Timer-Steuerelement in einem UpdatePanel-Steuerelement platziert wird, löst er ein teilupdates für Seiten-Postback, während der wir die Daten zur DetailsView zum Anzeigen eines neuen, zufällig ausgewählten Produkts binden können.

Zu diesem Zweck ziehen Sie einen Timer aus der Toolbox, und legen Sie es in UpdatePanel. Ändern des Timers `ID` aus `Timer1` zu `ProductTimer` und die zugehörige `Interval` Eigenschaft aus 60000 und 15000 liegen. Die `Interval` Eigenschaft gibt die Anzahl der Millisekunden zwischen Postbacks; 15000 Einstellung bewirkt, dass des Timers ein teilupdates für Seiten-Postback alle 15 Sekunden ausgelöst wird. An diesem Punkt sollte des Timers deklarative Markup etwa wie folgt aussehen:


[!code-aspx[Main](master-pages-and-asp-net-ajax-cs/samples/sample5.aspx)]

Erstellen Sie einen Ereignishandler für des Timers `Tick` Ereignis. In diesem Ereignishandler müssen wir die Daten zur DetailsView erneut binden, durch den Aufruf des DetailsView `DataBind` Methode. Auf diese Weise weist die DetailsView, um die Daten aus der quellcodeverwaltung die Daten erneut abzurufen wird der auswählen und nach dem Zufallsprinzip einen neuen anzeigen ausgewählten Datensatz (genau wie bei der erneute Laden der Seite, indem Sie auf die im Browser auf die Schaltfläche "Aktualisieren").


[!code-csharp[Main](master-pages-and-asp-net-ajax-cs/samples/sample6.cs)]

Das ist alles schon! Rufen Sie die Seite über einen Browser ein. Zunächst wird eine zufällige Produktinformationen angezeigt. Wenn Sie geduldig auf den Bildschirm ansehen werden Sie feststellen, Informationen zu einem neuen Produkt nach 15 Sekunden, wie von Zauberhand die vorhandene Anzeige ersetzt.

Um besser zu sehen, was hier passiert ist, fügen Sie ein Label-Steuerelement, das UpdatePanel, das anzeigt, die die Anzeige zuletzt aktualisiert wurde. Fügen Sie ein Label-Steuerelement innerhalb von UpdatePanel hinzu, legen Sie dessen `ID` zu `LastUpdateTime`, und deaktivieren Sie die `Text` Eigenschaft. Als Nächstes erstellen Sie einen Ereignishandler für des UpdatePanel `Load` Ereignisses und die aktuelle Uhrzeit in der Bezeichnung. (Des UpdatePanel `Load` Ereignis wird ausgelöst, bei jedem seitenpostback vollständig oder teilweise-.)


[!code-csharp[Main](master-pages-and-asp-net-ajax-cs/samples/sample7.cs)]

Durch diese Änderung abgeschlossen ist enthält die Seite für die Zeit, die das aktuell angezeigte Produkt geladen wurde. Abbildung 6 zeigt die Seite beim ersten Mal besucht hat. Abbildung 7 zeigt die Seite 15 Sekunden später, nachdem das Timer-Steuerelement verfügt über "Perform" und UpdatePanel aktualisiert wurden, um Informationen über ein neues Produkt anzuzeigen.


[![Ein zufällig ausgewähltes Produkt wird beim Laden der Seite angezeigt.](master-pages-and-asp-net-ajax-cs/_static/image17.png)](master-pages-and-asp-net-ajax-cs/_static/image16.png)

**Abbildung 06**: nach dem Zufallsprinzip ausgewählte Produkt A wird beim Laden der Seite angezeigt ([klicken Sie, um das Bild in voller Größe anzeigen](master-pages-and-asp-net-ajax-cs/_static/image18.png))


[![Alle 15 Sekunden, die ein neues nach dem Zufallsprinzip ausgewählten Produkt angezeigt wird](master-pages-and-asp-net-ajax-cs/_static/image20.png)](master-pages-and-asp-net-ajax-cs/_static/image19.png)

**Abbildung 07**: alle 15 Sekunden wird ein neues nach dem Zufallsprinzip ausgewählten Produkt angezeigt ([klicken Sie, um das Bild in voller Größe anzeigen](master-pages-and-asp-net-ajax-cs/_static/image21.png))


## <a name="step-3-using-the-scriptmanagerproxy-control"></a>Schritt 3: Verwenden des Steuerelements "ScriptManagerProxy"

ScriptManager kann auch benutzerdefinierte JavaScript-Dateien, Verweise auf Webdienste Skripts aktiviert, und benutzerdefinierte Authentifizierung, Autorisierung und Profile-Diensten registrieren, zusammen mit, wie das notwendige Skript für das ASP.NET AJAX-Framework-Clientbibliothek. In der Regel sind solche Anpassungen für eine bestimmte Seite spezifisch. Jedoch, wenn die benutzerdefinierte Dateien, Webdienst-Verweise oder Authentifizierung Skript zu erstellen, Autorisierung oder Profildienste im ScriptManager auf der Masterseite verwiesen werden, und sie befinden sich im *alle* Seiten auf der Website.

Verwenden Sie zum Hinzufügen des Steuerelements "ScriptManagerProxy" ScriptManager Anpassungen pro Seite von Seite. Sie können eine Inhaltsseite eine "ScriptManagerProxy" hinzu, und registrieren Sie dann die benutzerdefinierte JavaScript-Datei, Webdienst-Verweis oder Authentifizierung, Autorisierung oder Profildienst von der "ScriptManagerProxy"; Dies hat den Effekt der diese Dienste für bestimmte Inhaltsseite registrieren.

> [!NOTE]
> Eine ASP.NET-Seite kann nur maximal ein ScriptManager-Steuerelement vorhanden verfügen. Sie können nicht aus diesem Grund ein ScriptManager-Steuerelement an eine Inhaltsseite hinzufügen, wenn das ScriptManager-Steuerelement auf der Masterseite bereits definiert ist. Der einzige Zweck der "ScriptManagerProxy" ist eine Möglichkeit für Entwickler im ScriptManager auf der Masterseite zu definieren, aber immer noch die Möglichkeit zum ScriptManager Anpassungen pro Seite von Seite hinzufügen.


Erweitern, um die ScriptManagerProxy-Steuerelement in Aktion sehen zu können, lassen Sie uns UpdatePanel in `ShowRandomProduct.aspx` , eine Schaltfläche aufzunehmen, die Client-seitige Skript zum Anhalten oder Fortsetzen der Timer-Steuerelement verwendet. Das Timer-Steuerelement verfügt über drei clientseitige-Methoden, die wir verwenden können, um diese gewünschte Funktionalität zu erzielen:

- `_startTimer()` : startet das Timer-Steuerelement
- `_raiseTick()` -führt dazu, dass das Timer-Steuerelement auf "Tick", wodurch Postback und Auslösen von dessen `Tick` Ereignis auf dem Server
- `_stopTimer()` – beendet den Timer-Steuerelement

Erstellen wir eine JavaScript-Datei mit einer Variablen namens `timerEnabled` und eine Funktion namens `ToggleTimer`. Die `timerEnabled` Variable gibt an, ob das Timer-Steuerelement derzeit aktiviert oder deaktiviert ist, wird standardmäßig auf "true". Die `ToggleTimer` -Funktion akzeptiert zwei Eingabeparameter: einen Verweis auf die Schaltfläche "Anhalten bzw. fortsetzen" sowie die clientseitige `id` Wert des Timer-Steuerelements. Diese Funktion wird der Wert von `timerEnabled`, ruft einen Verweis auf das Timer-Steuerelement, startet oder beendet den Timer (abhängig vom Wert `timerEnabled`), und der Text der Schaltfläche anzeigen "Pause" oder "Resume" aktualisiert. Diese Funktion wird aufgerufen, wenn der anhalten bzw. fortsetzen geklickt wird.

Zunächst erstellen Sie einen neuen Ordner auf der Website mit dem Namen `Scripts`. Fügen Sie eine neue Datei zum Ordner "Scripts" mit dem Namen `TimerScript.js` JScript-Datei.


[![Fügen Sie eine neue JavaScript-Datei zum Ordner "Scripts"](master-pages-and-asp-net-ajax-cs/_static/image23.png)](master-pages-and-asp-net-ajax-cs/_static/image22.png)

**Abbildung 08**: Hinzufügen einer neuen JavaScript-Datei, die `Scripts` Ordner ([klicken Sie, um das Bild in voller Größe anzeigen](master-pages-and-asp-net-ajax-cs/_static/image24.png))


[![Eine neue JavaScript-Datei wurde auf der Website hinzugefügt](master-pages-and-asp-net-ajax-cs/_static/image26.png)](master-pages-and-asp-net-ajax-cs/_static/image25.png)

**Abbildung 09**: die Website eine neue JavaScript-Datei hinzugefügt wurde ([klicken Sie, um das Bild in voller Größe anzeigen](master-pages-and-asp-net-ajax-cs/_static/image27.png))


Als Nächstes fügen Sie mit dem folgenden Skript der TimerScript.js-Datei hinzu:


[!code-csharp[Main](master-pages-and-asp-net-ajax-cs/samples/sample8.cs)]

Jetzt müssen wir diese benutzerdefinierten JavaScript-Datei in registrieren `ShowRandomProduct.aspx`. Wechseln Sie zurück zur `ShowRandomProduct.aspx` und ein ScriptManagerProxy-Steuerelement auf der Seite hinzufügen, legen Sie dessen `ID` zu `MyManagerProxy`. Registrieren Sie ein benutzerdefiniertes JavaScript Datei wählen Sie im Designer des Steuerelements "ScriptManagerProxy", und fahren Sie mit dem Fenster "Eigenschaften". Eine der Eigenschaften wird mit der Bezeichnung Skripts. Durch Aktivieren dieser Eigenschaft zeigt den ScriptReference-Auflistungs-Editor, die in Abbildung 10 dargestellt. Klicken Sie auf die Schaltfläche "hinzufügen", um einen neuen Skriptverweis und geben dann den Pfad zur Skriptdatei in die Path-Eigenschaft: `~/Scripts/TimerScript.js`.


[![Fügen Sie einen Skriptverweis auf das Steuerelement "ScriptManagerProxy"](master-pages-and-asp-net-ajax-cs/_static/image29.png)](master-pages-and-asp-net-ajax-cs/_static/image28.png)

**Abbildung 10**: einen Skriptverweis auf das Steuerelement "ScriptManagerProxy" hinzufügen ([klicken Sie, um das Bild in voller Größe anzeigen](master-pages-and-asp-net-ajax-cs/_static/image30.png))


Nach dem Hinzufügen des Skriptverweis des Steuerelements "ScriptManagerProxy" deklarative ist Markup wird aktualisiert eine `<Scripts>` Auflistung mit einem einzelnen `ScriptReference` Eintrag wie den folgenden Codeausschnitt von Markup veranschaulicht wird:


[!code-aspx[Main](master-pages-and-asp-net-ajax-cs/samples/sample9.aspx)]

Die `ScriptReference` Eintrag weist den "ScriptManagerProxy", um einen Verweis auf die JavaScript-Datei in der gerenderten Markup enthalten. In der "ScriptManagerProxy", also durch die Registrierung des benutzerdefiniertes Skripts die `ShowRandomProduct.aspx` gerenderte Ausgabe enthält nun eine weitere `<script src="url"></script>` Tag: `<script src="Scripts/TimerScript.js" type="text/javascript"></script>`.

Wir können jetzt Aufrufen der `ToggleTimer` in definierte Funktion `TimerScript.js` das vom Clientskript in die `ShowRandomProduct.aspx` Seite. Fügen Sie folgenden HTML-Code innerhalb von UpdatePanel hinzu:


[!code-aspx[Main](master-pages-and-asp-net-ajax-cs/samples/sample10.aspx)]

Dadurch werden eine Schaltfläche mit dem Text "Pause" angezeigt. Jedes Mal, wenn sie angeklickt wird, die JavaScript-Funktion `ToggleTimer` aufgerufen wird, wird die Übergabe eines Verweises auf die Schaltfläche und die Id-Wert, der das Timer-Steuerelement (`ProductTimer`). Beachten Sie die Syntax zum Abrufen der `id` Wert des Timer-Steuerelements. `<%=ProductTimer.ClientID%>` Gibt den Wert des der `ProductTimer` Timer-Steuerelement `ClientID` Eigenschaft. In der [ *Steuerelement-IDs auf Inhaltsseiten* ](control-id-naming-in-content-pages-cs.md) Tutorial erläutert die Unterschiede zwischen der Serverseite `ID` Wert und die resultierende clientseitige `id` Wert, und wie `ClientID` gibt zurück, die die clientseitige `id`.

Abbildung 11 zeigt diese Seite, wenn zunächst über einen Browser aufgerufen. Der Zeitgeber wird derzeit ausgeführt und aktualisiert die angezeigten Produktinformationen alle 15 Sekunden. Abbildung 12 zeigt den Bildschirm, nachdem auf die Schaltfläche "Anhalten" geklickt wurde. Klicken Sie auf die Schaltfläche "Pause" hält den Timer und der Text der Schaltfläche zum "Resume" aktualisiert. Die Produktinformationen aktualisieren (und und werden alle 15 Sekunden aktualisieren), sobald der Benutzer auf Fortsetzen klickt.


[![Klicken Sie auf die Schaltfläche "Pause" um das Timer-Steuerelement zu beenden.](master-pages-and-asp-net-ajax-cs/_static/image32.png)](master-pages-and-asp-net-ajax-cs/_static/image31.png)

**Abbildung 11**: Klicken Sie auf die Schaltfläche "Pause" um das Timer-Steuerelement zu beenden ([klicken Sie, um das Bild in voller Größe anzeigen](master-pages-and-asp-net-ajax-cs/_static/image33.png))


[![Klicken Sie auf die Schaltfläche "fortsetzen", um den Timer neu zu starten.](master-pages-and-asp-net-ajax-cs/_static/image35.png)](master-pages-and-asp-net-ajax-cs/_static/image34.png)

**Abbildung 12**: Klicken Sie auf die Schaltfläche "fortsetzen", um den Timer neu zu starten ([klicken Sie, um das Bild in voller Größe anzeigen](master-pages-and-asp-net-ajax-cs/_static/image36.png))


## <a name="summary"></a>Zusammenfassung

Beim Erstellen von AJAX-aktivierten Webanwendungen mit ASP.NET AJAX-Framework ist es zwingend erforderlich, dass alle AJAX-aktivierten Webseite ein ScriptManager-Steuerelement enthalten. Um diesen Prozess zu vereinfachen, können wir denken Sie daran, jede Inhaltsseite ein ScriptManager-Steuerelement hinzufügen müssen, anstatt die Masterseite ein ScriptManager-Steuerelement hinzufügen. Schritt 1 wurde gezeigt, wie Sie die Masterseite während Schritt2 Implementieren von AJAX-Funktionalität in einer Inhaltsseite betrachtet ScriptManager hinzufügen.

Wenn Sie zum Hinzufügen von benutzerdefinierten Skripts, Verweise auf Webdienste Skripts aktiviert, oder benutzerdefinierte Authentifizierung, Autorisierung und profilerstellung für Dienste auf eine bestimmte Inhaltsseite auf der Seite Inhalt ein ScriptManagerProxy-Steuerelement hinzufügen und konfigurieren Sie dann die Anpassungen vorhanden. Schritt 3 untersucht, wie Sie die "ScriptManagerProxy" verwenden, um eine benutzerdefinierte JavaScript-Datei in eine bestimmte Inhaltsseite zu registrieren.

Viel Spaß beim Programmieren!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Tutorial erläutert finden Sie in den folgenden Ressourcen:

- [ASP.NET AJAX-Frameworks](../../../../ajax/index.md)
- [ASP.NET AJAX-Lernprogramme](../aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax.md)
- [ASP.NET AJAX-Videos](../../../videos/aspnet-ajax/index.md)
- [Erstellen von interaktiven Benutzeroberfläche mit Microsoft ASP.NET AJAX](http://aspnet.4guysfromrolla.com/articles/101007-1.aspx)
- [Verwenden von NEWID, nach dem Zufallsprinzip Sortieren von Datensätzen](http://www.sqlteam.com/article/using-newid-to-randomly-sort-records)
- [Verwenden das Timer-Steuerelement](http://aspnet.4guysfromrolla.com/articles/061808-1.aspx)

### <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor mehrerer Büchern zu ASP/ASP.NET und Gründer von 4GuysFromRolla.com, arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neuestes Buch heißt [ *Sams Teach selbst ASP.NET 3.5 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott erreicht werden kann, zur [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog unter [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Besonderen Dank an

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](interacting-with-the-content-page-from-the-master-page-cs.md)
> [Weiter](specifying-the-master-page-programmatically-cs.md)
