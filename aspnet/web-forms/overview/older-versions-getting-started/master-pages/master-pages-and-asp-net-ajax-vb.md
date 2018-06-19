---
uid: web-forms/overview/older-versions-getting-started/master-pages/master-pages-and-asp-net-ajax-vb
title: Masterseiten und ASP.NET AJAX (VB) | Microsoft Docs
author: rick-anderson
description: Erläutert die Optionen für die Verwendung von ASP.NET AJAX und Masterseiten. Prüft, mit der Klasse ScriptManagerProxy; Erläutert, wie die verschiedenen JS-Dateien Dependi geladen werden...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/11/2008
ms.topic: article
ms.assetid: 0ee9318c-29bb-4d58-b1dc-94e575b8ae10
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/master-pages-and-asp-net-ajax-vb
msc.type: authoredcontent
ms.openlocfilehash: 2c7d8477d6d9d235749d88d0b657d60454298e53
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30891138"
---
<a name="master-pages-and-aspnet-ajax-vb"></a>Masterseiten und ASP.NET AJAX (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Herunterladen von Code](http://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_08_VB.zip) oder [PDF herunterladen](http://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_08_VB.pdf)

> Erläutert die Optionen für die Verwendung von ASP.NET AJAX und Masterseiten. Prüft, mit der Klasse ScriptManagerProxy; Erläutert, wie die verschiedenen JS-Dateien abhängig davon, ob die ScriptManager, in der Masterdatenbank verwendet wird geladen werden, Seite oder Inhalt.


## <a name="introduction"></a>Einführung

In den vergangenen Jahren haben mehr Entwickler AJAX-fähige Webanwendungen erstellen, wurden. Eine AJAX-aktivierten Website verwendet eine Reihe von verwandten webtechnologien, um eine Reaktionsgeschwindigkeit benutzererfahrung zu ermöglichen. Erstellen von ASP.NET AJAX-aktivierten Anwendungen ist erstaunlich einfach Dank des Microsoft ASP.NET AJAX-Framework. ASP.NET AJAX wird in ASP.NET 3.5 und Visual Studio 2008 erstellt. Es ist auch als separater Download für ASP.NET 2.0-Anwendungen verfügbar.

Beim Erstellen von AJAX-fähige Webseiten mit ASP.NET AJAX-Framework müssen Sie jede Seite, die das Framework verwendet, die genau ein ScriptManager-Steuerelement hinzufügen. Wie der Name schon sagt, verwaltet das ScriptManager die Skriptdatei auf Clientseite in AJAX-fähige Webseiten verwendet. Mindestens ausgibt ScriptManager HTML, der anweist, den Browser anzuweisen, den JavaScript-Dateien, Zusammensetzung der ASP.NET AJAX-Clientbibliothek herunterladen. Auch kann verwendet werden, um benutzerdefinierte JavaScript-Dateien, die Skript-aktivierten Webdiensten und Dienstfunktionalität benutzerdefinierte Anwendung zu registrieren.

Wenn Ihre Website verwendet Masterseiten (wie es sollten) müssen nicht unbedingt Sie jeder einzelnen Inhaltsseite ein ScriptManager-Steuerelement hinzu; Stattdessen können Sie die Gestaltungsvorlage ein ScriptManager-Steuerelement hinzufügen. Dieses Lernprogramm zeigt, wie die Gestaltungsvorlage ScriptManager-Steuerelement hinzu. Es sucht auch an, wie das ScriptManagerProxy-Steuerelement verwenden, um benutzerdefinierte Skripts und Skriptdienste in einer bestimmten Inhaltsseite zu registrieren.

> [!NOTE]
> In diesem Lernprogramm wird nicht untersuchen, entwerfen, oder Erstellen von AJAX-fähige Webanwendungen mit ASP.NET AJAX-Framework. Weitere Informationen zur Verwendung von AJAX finden Sie in der ASP.NET AJAX-Videos und Lernprogramme sowie die Ressourcen, die im Abschnitt Weitere Themen am Ende dieses Lernprogramms aufgeführt.


## <a name="examining-the-markup-emitted-by-the-scriptmanager-control"></a>Untersuchen das Markup ausgegeben, die vom ScriptManager-Steuerelement

Die ScriptManager-Steuerelement gibt Markup, das dem Browser anzuweisen, die JavaScript-Dateien herunterladen, Zusammensetzung der ASP.NET AJAX-Client-Bibliothek weist. Außerdem werden die Seite, die diese Bibliothek initialisiert einem gewissen Grad Inline-JavaScript hinzugefügt. Das folgende Markup zeigt den Inhalt, der die gerenderte Ausgabe einer Seite hinzugefügt wird, die ein ScriptManager-Steuerelement enthält:


[!code-html[Main](master-pages-and-asp-net-ajax-vb/samples/sample1.html)]

Die `<script src="url"></script>` Tags anzuweisen, den Browser zum Herunterladen und Ausführen der JavaScript-Datei am *Url*. Die ScriptManager gibt drei solche Tags; eine verweist auf die Datei `WebResource.axd`, während die anderen beiden die Datei verweisen `ScriptResource.axd`. Diese Dateien sind als Dateien in Ihrer Website nicht tatsächlich vorhanden. Stattdessen, wenn eine Anforderung für eine dieser Dateien auf dem Webserver eingeht, das Modul für ASP.NET überprüft Querystring und gibt den entsprechenden JavaScript-Inhalt. Das Skript, das durch diese drei externe JavaScript-Dateien bereitgestellt bilden des ASP.NET AJAX-Frameworks-Clientbibliothek. Die andere `<script>` Tags von ScriptManager ausgegeben, umfassen Inlineskript, das diese Bibliothek initialisiert.

Die externe Skriptverweise und Inline-Skript ausgegeben, die für die ScriptManager sind wichtig für eine Seite, die das ASP.NET AJAX-Framework verwendet, aber wird nicht benötigt, um Seiten, die das Framework nicht verwenden. Sie können daher Grund, dass er eignet sich nur ein ScriptManager auf diesen Seiten hinzufügen, die ASP.NET AJAX-Framework verwenden. Und dies ist ausreichend, allerdings haben viele Seiten, die das Framework verwenden. Sie müssen am Ende alle Seiten - eine sich wiederholende Aufgabe, den geringsten sagen: das ScriptManager-Steuerelement hinzugefügt. Alternativ können Sie ein ScriptManager der Masterseite hinzufügen, die diese notwendige Skript dann in allen Inhaltsseiten injiziert. Bei diesem Ansatz müssen Sie nicht vergessen nie eine ScriptManager auf eine neue Seite hinzufügen, die das ASP.NET AJAX-Framework verwendet, da er bereits von der Masterseite enthalten ist. Schritt 1 Durchläufe durch Hinzufügen ein ScriptManager der Masterseite.

> [!NOTE]
> Wenn Sie z. B. AJAX-Funktionen der Benutzeroberfläche der Masterseite möchten, haben Sie keine andere Möglichkeit, in die Angelegenheit, – müssen Sie die ScriptManager in die Masterseite einschließen.


Ein Nachteil der Masterseite ScriptManager hinzugefügt ist, dass das obige Skript, in ausgegeben wird *jeder* Seite, unabhängig davon, ob das benötigte. Dies führt klar zu ungenutzten Netzwerkbandbreite für diese Seiten, die haben die ScriptManager (über die Gestaltungsvorlage) enthalten, aber keine Funktionen von ASP.NET AJAX-Framework verwenden. Aber wie viel Bandbreite verschwendet wird?

- Der tatsächliche Inhalt ausgegeben, die für die ScriptManager (siehe oben) addiert etwas mehr als 1KB.
- Die drei externe Skriptdateien verweist die `<script>` Element, bilden jedoch ungefähr 450 KB Daten, die nicht komprimiert; in einer Website, die Gzip-Komprimierung verwendet, kann diese Gesamtbandbreite in der Nähe von 100 KB verringert werden. Allerdings wird diese Skriptdateien vom Browser zwischengespeichert werden, die ein Jahr lang, was bedeutet, dass sie nur einmal heruntergeladen werden müssen, und klicken Sie dann auf anderen Seiten auf der Website wiederverwendet werden können.

Im besten Fall wenn die Skriptdateien zwischengespeichert werden, ist dann die Gesamtkosten 1KB unerheblich. Im schlimmsten Fall jedoch - d. h., wenn die Skriptdateien nicht noch heruntergeladen wurden und den Webserver ist eine Art der Komprimierung nicht verwenden – der Bandbreite Treffer werden etwa 450KB, die von einer Sekunde oder zwei über eine Breitbandverbindung mit bis zu einer Minute an einer beliebigen Stelle hinzufügen können  der Benutzer über DFÜ-Modem. Die gute Nachricht ist, da die externe Skriptdateien vom Browser zwischengespeichert werden, diese schlimmsten Fall selten auftritt.

> [!NOTE]
> Falls Sie noch unbequem platzieren das ScriptManager-Steuerelement in die Masterseite unsicher sind, sollten Sie das Web Form (die `<form runat="server">` Markup in die Masterseite). Jeder ASP.NET-Seite, der Postbacks Objektmodell verwendet, muss genau ein Webformular enthalten. Ein Webformular hinzufügen, fügt zusätzliche Inhalt hinzu: eine Reihe von ausgeblendeten Formularfelder, die `<form>` tag selbst, und bei Bedarf eine JavaScript-Funktion zum Initiieren eines Postbacks aus einem Skript. Dieses Markup ist nicht erforderlich für Seiten, die nicht von postback. Diese unnötige Markup konnte durch entfernen das Web Form der Masterseite und manuell hinzufügen zu jeder Inhaltsseite, die entfernt werden. Ergeben sich folgende Vorteile das Web Form in die Masterseite überwiegen jedoch die Nachteile von ihm bestimmten Inhaltsseiten unnötigerweise hinzugefügt.


## <a name="step-1-adding-a-scriptmanager-control-to-the-master-page"></a>Schritt 1: Hinzufügen einer ScriptManager-Steuerelement auf der Seite "Master"

Jede Webseite, die ASP.NET AJAX-Framework verwendet, muss genau ein ScriptManager-Steuerelement enthalten. Aufgrund dieser Anforderung sinnvoll es in der Regel auf ein einzelnes ScriptManager-Steuerelement auf der Seite "master" zu setzen, sodass alle Inhaltsseiten das ScriptManager-Steuerelement, das automatisch eingeschlossen haben. Darüber hinaus muss die ScriptManager vor einem ASP.NET AJAX-Steuerelement, z. B. die Steuerelemente UpdatePanel und UpdateProgress stammen. Aus diesem Grund empfiehlt es sich um ScriptManager vor alle ContentPlaceHolder-Steuerelemente in das Web Form zu versetzen.

Öffnen der `Site.master` Masterseite und fügen Sie ein ScriptManager-Steuerelement auf der Seite in das Web Form jedoch vor der `<div id="topContent">` Element (siehe Abbildung 1). Wenn Sie Visual Web Developer 2008 oder Visual Studio 2008 verwenden, befindet sich das ScriptManager-Steuerelement in der Toolbox auf die Registerkarte "AJAX-Erweiterungen". Wenn Sie Visual Studio 2005 verwenden, müssen Sie zuerst das ASP.NET AJAX-Framework zu installieren und die Steuerelemente zur Toolbox hinzufügen. Besuchen Sie die ASP.NET AJAX-Download-Seite, um das Framework für ASP.NET 2.0 zu erhalten.

Ändern Sie nach dem Hinzufügen der ScriptManager auf der Seite an, dessen `ID` aus `ScriptManager1` auf `MyManager`.


[![Die ScriptManager auf der Seite "Master" hinzufügen](master-pages-and-asp-net-ajax-vb/_static/image2.png)](master-pages-and-asp-net-ajax-vb/_static/image1.png)

**Abbildung 01**: ScriptManager der Master-Seite hinzufügen ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-pages-and-asp-net-ajax-vb/_static/image3.png))


## <a name="step-2-using-the-aspnet-ajax-framework-from-a-content-page"></a>Schritt 2: Verwenden des ASP.NET AJAX-Frameworks Inhaltsseite

Mit dem ScriptManager-Steuerelement hinzugefügt, die Gestaltungsvorlage können wir jetzt jeder Inhaltsseite ASP.NET AJAX-Framework-Funktionen hinzufügen. Erstellen wir eine neue ASP.NET-Seite, in dem ein zufällig ausgewähltes Produkt aus der Northwind-Datenbank angezeigt. Wir verwenden das ASP.NET AJAX-Framework Timer-Steuerelement zum Aktualisieren dieser Anzeige alle 15 Sekunden, die ein neues Produkt angezeigt.

Zunächst erstellen eine neue Seite in das Stammverzeichnis, das mit dem Namen `ShowRandomProduct.aspx`. Vergessen Sie nicht, binden Sie diese neue Seite, um die `Site.master` Masterseite.


[![Fügen Sie eine neue ASP.NET-Seite auf der Website](master-pages-and-asp-net-ajax-vb/_static/image5.png)](master-pages-and-asp-net-ajax-vb/_static/image4.png)

**Abbildung 02**: Fügen Sie eine neue ASP.NET-Seite auf der Website ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-pages-and-asp-net-ajax-vb/_static/image6.png))


Bedenken Sie, dass in der Angabe der Titel, Meta-Tags und andere HTML-Header in der Gestaltungsvorlage [SKM1] Lernprogramm erstellt wir eine benutzerdefinierte Basisseite-Klasse, die mit dem Namen `BasePage` , die den Titel der Seite generiert, wenn sie nicht explizit festgelegt wurde. Wechseln Sie zu der `ShowRandomProduct.aspx` Seite des Code-Behind-Klasse, und Ihr abgeleitet sein `BasePage` (anstelle von aus `System.Web.UI.Page`).

Aktualisieren Sie schließlich die `Web.sitemap` Datei, die einen Eintrag in dieser Lektion hinzugefügt. Fügen Sie das folgende Markup unterhalb der `<siteMapNode>` für die Masterdatenbank Content Seite Interaktion Lektion:


[!code-xml[Main](master-pages-and-asp-net-ajax-vb/samples/sample2.xml)]

Das Hinzufügen dieses `<siteMapNode>` Elements in den Lektionen wiedergegeben wird (siehe Abbildung 5).

### <a name="displaying-a-randomly-selected-product"></a>Anzeigen eines zufällig ausgewählten Produkts

Zurück zu `ShowRandomProduct.aspx`. Ziehen Sie aus der Designer ein UpdatePanel-Steuerelement aus der Toolbox in die `MainContent` Inhaltssteuerelement aus, und legen Sie dessen `ID` Eigenschaft `ProductPanel`. UpdatePanel stellt einen Bereich auf dem Bildschirm, der über ein Postback Teilseite asynchron aktualisiert werden kann.

Unsere erste Aufgabe besteht darin Informationen zu einem zufällig ausgewählten Produkt innerhalb der UpdatePanel anzuzeigen. Starten Sie durch UpdatePanel, DetailsView-Steuerelement ziehen. Legen Sie das DetailsView-Steuerelement `ID` Eigenschaft, um `ProductInfo` und löschen, dessen `Height` und `Width` Eigenschaften. Erweitern Sie die DetailsView Smarttag und aus der Datenquelle auswählen Dropdown-Liste auswählen, DetailsView an ein neues SqlDataSource-Steuerelement mit dem Namen binden `RandomProductDataSource`.


[![Binden von DetailsView an ein neues SqlDataSource-Steuerelement](master-pages-and-asp-net-ajax-vb/_static/image8.png)](master-pages-and-asp-net-ajax-vb/_static/image7.png)

**Abbildung 03**: Binden von DetailsView an ein neues SqlDataSource-Steuerelement ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-pages-and-asp-net-ajax-vb/_static/image9.png))


Konfigurieren Sie die SqlDataSource-Steuerelement für die Verbindung zur Northwind-Datenbank über die `NorthwindConnectionString` (die in der Interaktion mit der Seite "Master" aus dem Lernprogramm Inhaltsseite [SKM2] erstellt haben). Wenn die select-Anweisung konfigurieren wählen, ob eine benutzerdefinierte SQL­Anweisung angeben, und geben dann die folgende Abfrage:


[!code-sql[Main](master-pages-and-asp-net-ajax-vb/samples/sample3.sql)]

Die `TOP 1` -Schlüsselwort in der `SELECT` -Klausel gibt nur den ersten Datensatz von der Abfrage zurückgegebenen zurück. Die `NEWID()` Funktion generiert einen neuen Wert für den globally unique Identifier (GUID) und kann verwendet werden, eine `ORDER BY` -Klausel, um die Tabelle Datensätze in zufälliger Reihenfolge zurück.


[![Konfigurieren Sie die SqlDataSource um einen einzelnen, zufällig ausgewählten Datensatz zurückgeben](master-pages-and-asp-net-ajax-vb/_static/image11.png)](master-pages-and-asp-net-ajax-vb/_static/image10.png)

**Abbildung 04**: Konfigurieren der SqlDataSource zum Zurückgeben einer einzelnen, zufällig ausgewählten Datensatz ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-pages-and-asp-net-ajax-vb/_static/image12.png))


Nach Abschluss des Assistenten, erstellt Visual Studio ein BoundField für die zwei Spalten, die von der obigen Abfrage zurückgegeben. An diesem Punkt sollte deklarativem Markup Ihrer Seite etwa wie folgt aussehen:


[!code-aspx[Main](master-pages-and-asp-net-ajax-vb/samples/sample4.aspx)]

Abbildung 5 zeigt die `ShowRandomProduct.aspx` Seite, wenn Sie über einen Browser angezeigt. Klicken Sie auf die Aktualisierungsschaltfläche Ihres Browsers, um die Seite erneut zu laden. Daraufhin sollte die `ProductName` und `UnitPrice` Werte für einen neuen zufällig ausgewählten Datensatz.


[![Eine zufällige Product Name und Preis wird angezeigt.](master-pages-and-asp-net-ajax-vb/_static/image14.png)](master-pages-and-asp-net-ajax-vb/_static/image13.png)

**Abbildung 05**: ein zufälliger Product Name und der Preis angezeigt ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-pages-and-asp-net-ajax-vb/_static/image15.png))


### <a name="automatically-displaying-a-new-product-every-15-seconds"></a>Anzeigen von automatisch ein neues Produkt alle 15 Sekunden

Das ASP.NET AJAX-Framework umfasst ein Zeitgeber-Steuerelement, das zu einem bestimmten Zeitpunkt einen Postback ausführt. auf postback des Timers `Tick` Ereignis wird ausgelöst. Wenn das Timer-Steuerelement innerhalb eines UpdatePanel platziert wird, löst er Teilseite Postback, denen wir die Daten, DetailsView ein neues, zufällig ausgewähltes Produkt angezeigt binden können.

Zu diesem Zweck ziehen Sie einen Zeitgeber aus der Toolbox und legen Sie UpdatePanel ab. Ändern des Timers `ID` aus `Timer1` auf `ProductTimer` und dessen `Interval` Eigenschaft von 60000 und 15000 liegen. Die `Interval` Eigenschaft gibt die Anzahl der Millisekunden zwischen Postbacks; festlegen und 15000 liegen, wird des Timers-Teilseite Postback alle 15 Sekunden ausgelöst. An diesem Punkt sollte des Timers deklarative Markup etwa wie folgt aussehen:


[!code-aspx[Main](master-pages-and-asp-net-ajax-vb/samples/sample5.aspx)]

Erstellen Sie einen Ereignishandler für des Timers `Tick` Ereignis. In diesem Ereignishandler müssen wir die Daten, DetailsView erneut binden, durch Aufrufen der DetailsView `DataBind` Methode. Auf diese Weise wird angewiesen, DetailsView, die Daten aus der Datenquellen-Steuerelements, erneut abzurufen, wählen Sie zu nach dem Zufallsprinzip einen neuen anzeigen ausgewählten Datensatz (genau wie beim erneuten Laden der Seite durch Klicken auf die Schaltfläche "Aktualisieren" des Browsers ein).


[!code-vb[Main](master-pages-and-asp-net-ajax-vb/samples/sample6.vb)]

Das ist alles vorhanden ist! Rufen Sie die Seite über einen Browser. Zunächst wird eine zufällige Produktinformationen angezeigt. Wenn Sie den Bildschirm geduldig beobachten, sehen Sie, dass nach 15 Sekunden die vorhandene Anzeige von Informationen zu einem neuen Produkt wie von Zauberhand ersetzt.

Um besser zu sehen, hier geschieht, fügen Sie ein Bezeichnungsfeld-Steuerelement zu UpdatePanel, in dem die Zeit angezeigt, an die Anzeige zuletzt aktualisiert wurde. Fügen Sie eine Bezeichnung Websteuerelement in UpdatePanel hinzu, legen Sie seine `ID` auf `LastUpdateTime`, und deaktivieren Sie seine `Text` Eigenschaft. Als Nächstes erstellen Sie einen Ereignishandler für das UpdatePanel `Load` Ereignisses und die aktuelle Uhrzeit in der Bezeichnung. (Die UpdatePanel `Load` Ereignis für jedes Postback vollständig oder teilweise Seite ausgelöst wird.)


[!code-vb[Main](master-pages-and-asp-net-ajax-vb/samples/sample7.vb)]

Durch diese Änderung abgeschlossen ist enthält die Seite die Zeit, die das aktuell angezeigte Produkt geladen wurde. Abbildung 6 zeigt die Seite beim ersten Mal besucht hat. Abbildung 7 zeigt die Seite 15 Sekunden später, nachdem das Timer-Steuerelement "Konfigurationsdaten hat" und UpdatePanel aktualisiert wurde, um Informationen zu einem neuen Produkt anzuzeigen.


[![Ein zufällig ausgewähltes Produkt wird beim Laden der Seite angezeigt.](master-pages-and-asp-net-ajax-vb/_static/image17.png)](master-pages-and-asp-net-ajax-vb/_static/image16.png)

**Abbildung 06**: nach dem Zufallsprinzip ausgewählte Produkt A wird beim Laden der Seite angezeigt ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-pages-and-asp-net-ajax-vb/_static/image18.png))


[![Alle 15 Sekunden wird ein neues zufällig ausgewählten Produkt angezeigt](master-pages-and-asp-net-ajax-vb/_static/image20.png)](master-pages-and-asp-net-ajax-vb/_static/image19.png)

**Abbildung 07**: alle 15 Sekunden wird ein neues zufällig ausgewählten Produkt angezeigt ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-pages-and-asp-net-ajax-vb/_static/image21.png))


## <a name="step-3-using-the-scriptmanagerproxy-control"></a>Schritt 3: Verwenden des ScriptManagerProxy-Steuerelements

ScriptManager kann zusammen mit einschließlich das notwendige Skript für die ASP.NET AJAX-Framework-Clientbibliothek, benutzerdefinierte JavaScript-Dateien, Verweise auf Webdienste Skripts aktiviert und benutzerdefinierte Authentifizierung, Autorisierung und Profildienste registriert. In der Regel sind solche Anpassungen für eine bestimmte Seite spezifisch. Jedoch wenn die benutzerdefinierten Skriptdateien, Webdienst-Verweise oder Authentifizierung, Autorisierung oder Profildienste, in der ScriptManager in die Masterseite verwiesen werden werden sie auf allen Seiten der Website eingeschlossen.

Zum Hinzufügen verwenden ScriptManager Anpassungen in Abständen von Seite ScriptManagerProxy-Steuerelement. Sie können einer Inhaltsseite ein ScriptManagerProxy hinzu, und registrieren Sie die benutzerdefinierte JavaScript-Datei, Webdienst-Verweis oder Authentifizierung, Autorisierung oder Profildienst aus der ScriptManagerProxy; Dies wirkt sich diese Dienste für bestimmte Inhaltsseite registriert.

> [!NOTE]
> Eine ASP.NET-Seite können nur maximal ein ScriptManager-Steuerelement vorhanden. Sie können nicht aus diesem Grund ein ScriptManager-Steuerelement auf einer Inhaltsseite hinzufügen, wenn das ScriptManager-Steuerelement in die Masterseite bereits definiert ist. Der einzige Zweck der ScriptManagerProxy ist, stellt eine Möglichkeit für Entwickler ScriptManager in die Masterseite definieren, jedoch weiterhin die Möglichkeit, Anpassungen ScriptManager auf Basis von Seite hinzuzufügen.


Erweitern, um das Steuerelement ScriptManagerProxy in Aktion zu sehen, wir UpdatePanel in `ShowRandomProduct.aspx` eine Schaltfläche enthalten, die mit clientseitigem Skript anhalten oder Fortsetzen der Timer-Steuerelement. Das Timer-Steuerelement verfügt über drei clientseitige-Methoden, die es verwenden können, um diese gewünschte Funktionalität zu erzielen:

- `_startTimer()` – Startet das Timer-Steuerelement
- `_raiseTick()` – bewirkt, dass das Timer-Steuerelement, "Takt" und Zurücksenden und Auslösen von das Tick-Ereignis auf dem Server
- `_stopTimer()` – beendet den Timer-Steuerelement

Erstellen wir eine JavaScript-Datei mit einer Variablen namens `timerEnabled` und eine Funktion namens `ToggleTimer`. Die `timerEnabled` Variable gibt an, ob das Timer-Steuerelement derzeit aktiviert oder deaktiviert ist, wird standardmäßig auf "true". Die `ToggleTimer` -Funktion akzeptiert zwei Eingabeparameter: ein Verweis auf die Schaltfläche "anhalten/fortsetzen" und die clientseitige `id` Wert, der das Timer-Steuerelement. Diese Funktion wird der Wert von `timerEnabled`, ruft einen Verweis auf das Timer-Steuerelement, startet oder beendet den Zeitgeber (abhängig vom Wert der `timerEnabled`), und aktualisiert die Anzeige des Schaltflächentext in "Pause" oder "Resume". Diese Funktion wird aufgerufen werden, wenn das anhalten/fortsetzen geklickt wird.

Starten, indem Sie einen neuen Ordner erstellen, auf der Website mit dem Namen `Scripts`. Fügen Sie eine neue Datei in den Ordner Scripts, die mit dem Namen `TimerScript.js` des Typs JScript-Datei.


[![Fügen Sie eine neue JavaScript-Datei auf den Ordner "Skripts"](master-pages-and-asp-net-ajax-vb/_static/image23.png)](master-pages-and-asp-net-ajax-vb/_static/image22.png)

**Abbildung 08**: Hinzufügen einer neuen JavaScript-Datei, die `Scripts` Ordner ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-pages-and-asp-net-ajax-vb/_static/image24.png))


[![Auf der Website wurde eine neue JavaScript-Datei hinzugefügt.](master-pages-and-asp-net-ajax-vb/_static/image26.png)](master-pages-and-asp-net-ajax-vb/_static/image25.png)

**Abbildung 09**: ein neues JavaScript-Datei der Website hinzugefügt wurde ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-pages-and-asp-net-ajax-vb/_static/image27.png))


Als Nächstes fügen Sie das folgende Skript, um die `TimerScript.js` Datei:


[!code-csharp[Main](master-pages-and-asp-net-ajax-vb/samples/sample8.cs)]

Jetzt müssen wir diese benutzerdefinierte JavaScript-Datei im registrieren `ShowRandomProduct.aspx`. Zurück zu `ShowRandomProduct.aspx` und Hinzufügen eines ScriptManagerProxy-Steuerelements auf der Seite "; legen Sie dessen `ID` auf `MyManagerProxy`. So registrieren ein benutzerdefinierte JavaScript Datei ScriptManagerProxy-Steuerelement im Designer auswählen, und fahren Sie dann auf Eigenschaftenfenster. Eine der Eigenschaften wird mit der Bezeichnung Skripts. Durch Aktivieren dieser Eigenschaft zeigt den ScriptReference-Auflistungs-Editor, die in Abbildung 10 dargestellt. Klicken Sie auf die Schaltfläche "hinzufügen", um einen neuen Skriptverweis enthalten, und geben dann den Pfad zur Skriptdatei in der Path-Eigenschaft: `~/Scripts/TimerScript.js`.


[![Fügen Sie einen Skriptverweis auf das ScriptManagerProxy-Steuerelement hinzu](master-pages-and-asp-net-ajax-vb/_static/image29.png)](master-pages-and-asp-net-ajax-vb/_static/image28.png)

**Abbildung 10**: Fügen Sie einen Skriptverweis auf das Steuerelement ScriptManagerProxy ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-pages-and-asp-net-ajax-vb/_static/image30.png))


Nach dem deklarativen's Skriptverweis ScriptManagerProxy-Steuerelement hinzufügen Markup so aktualisiert wird, eine `<Scripts>` Auflistung mit einem einzelnen `ScriptReference` Eintrag, der folgende Ausschnitt aus Markup veranschaulicht:


[!code-aspx[Main](master-pages-and-asp-net-ajax-vb/samples/sample9.aspx)]

Die `ScriptReference` Eintrag weist den ScriptManagerProxy einen Verweis auf die JavaScript-Datei in der gerenderten Markups eingeschlossen. D. h. durch die Registrierung der benutzerdefinierten Skript in die ScriptManagerProxy der `ShowRandomProduct.aspx` gerenderten Seitenausgabe enthält jetzt eine weitere `<script src="url"></script>` Tag: `<script src="Scripts/TimerScript.js" type="text/javascript"></script>`.

Wir können nun Aufrufe der `ToggleTimer` in definierte Funktion `TimerScript.js` aus dem Clientskript in die `ShowRandomProduct.aspx` Seite. Fügen Sie folgenden HTML-Code in UpdatePanel hinzu:


[!code-aspx[Main](master-pages-and-asp-net-ajax-vb/samples/sample10.aspx)]

Dies zeigt eine Schaltfläche mit dem Text "Anhalten" an. Bei jedem geklickt wird, die JavaScript-Funktion `ToggleTimer` aufgerufen wird, übergibt einen Verweis auf die Schaltfläche "" und die `id` Wert, der das Timer-Steuerelement (`ProductTimer`). Beachten Sie die Syntax zum Abrufen der `id` Wert, der das Timer-Steuerelement. `<%=ProductTimer.ClientID%>` Gibt den Wert der `ProductTimer` Timer-Steuerelement `ClientID` Eigenschaft. Die Steuerelement-ID-Benennung in Inhaltsseiten [SKM3] Lernprogramm erläutert wir die Unterschiede zwischen den serverseitigen `ID` Wert und die resultierende clientseitige `id` Wert, und wie `ClientID` gibt zurück, die die clientseitige `id`.

Abbildung 11 zeigt diese Seite, wenn zunächst über einen Browser zugegriffen werden. Der Zeitgeber wird derzeit ausgeführt und die angezeigten Produktinformationen alle 15 Sekunden aktualisiert. Abbildung 12 zeigt den Bildschirm, nachdem die entsprechende Schaltfläche geklickt wurde. Klicken auf die Schaltfläche "Anhalten" hält den Zeitgeber und aktualisiert den Schaltflächentext in "Fortsetzen". Die Produktinformationen aktualisieren (und wird fortgesetzt, aktualisieren Sie alle 15 Sekunden), sobald der Benutzer fortsetzen klickt.


[![Klicken Sie auf die entsprechende Schaltfläche derart das Timer-Steuerelement](master-pages-and-asp-net-ajax-vb/_static/image32.png)](master-pages-and-asp-net-ajax-vb/_static/image31.png)

**Abbildung 11**: Klicken Sie auf die Pause-Schaltfläche, um das Timer-Steuerelement zu beenden ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-pages-and-asp-net-ajax-vb/_static/image33.png))


[![Klicken Sie auf die Schaltfläche mit den fortsetzen, um den Zeitgeber neu zu starten](master-pages-and-asp-net-ajax-vb/_static/image35.png)](master-pages-and-asp-net-ajax-vb/_static/image34.png)

**Abbildung 12**: Klicken Sie auf die Schaltfläche mit den fortsetzen, um den Zeitgeber neu zu starten ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-pages-and-asp-net-ajax-vb/_static/image36.png))


## <a name="summary"></a>Zusammenfassung

Beim Erstellen von AJAX-fähige Webanwendungen unter Verwendung von ASP.NET AJAX-Framework ist es erforderlich, dass jeder AJAX-aktivierten Webseite ein ScriptManager-Steuerelement enthalten. Um diesen Prozess zu vereinfachen, können wir die Gestaltungsvorlage anstatt Denken Sie daran, jede Inhaltsseite ein ScriptManager hinzufügen ein ScriptManager hinzufügen. Schritt 1 wurde gezeigt, wie die Gestaltungsvorlage bei Schritt2 erläutert, Implementieren von AJAX-Funktionen in einer Inhaltsseite ScriptManager hinzugefügt.

Wenn Sie zum Hinzufügen von benutzerdefinierten Skripts, Verweise auf Webdienste Skripts aktiviert oder benutzerdefinierte Authentifizierung, Autorisierung oder Profildienste an eine bestimmte Inhaltsseite ein ScriptManagerProxy-Steuerelement auf der Seite Inhalt hinzufügen und konfigurieren Sie dann die Anpassungen vorhanden. Schritt 3 untersucht, wie die ScriptManagerProxy verwenden, um eine benutzerdefinierte JavaScript-Datei in einer bestimmten Inhaltsseite zu registrieren.

Viel Spaß beim Programmieren!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Lernprogramm erläutert finden Sie in den folgenden Ressourcen:

- [ASP.NET AJAX Framework](../../../../ajax/index.md)
- [ASP.NET AJAX-Lernprogramme](../aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax.md)
- [ASP.NET AJAX-Videos](../../../videos/aspnet-ajax/index.md)
- [Erstellen von interaktiven Benutzeroberfläche mit Microsoft ASP.NET AJAX](http://aspnet.4guysfromrolla.com/articles/101007-1.aspx)
- [Verwenden von NEWID, nach dem Zufallsprinzip Sortieren von Datensätzen](http://www.sqlteam.com/article/using-newid-to-randomly-sort-records)
- [Verwenden das Timer-Steuerelement](http://aspnet.4guysfromrolla.com/articles/061808-1.aspx)

### <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor mehrerer ASP/ASP.NET-Büchern und Gründer von 4GuysFromRolla.com arbeitet mit Microsoft-Web-Technologien seit 1998. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 3.5 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott erreicht werden kann, zur [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog unter [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Besonderen Dank an

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](interacting-with-the-content-page-from-the-master-page-vb.md)
> [Weiter](specifying-the-master-page-programmatically-vb.md)
