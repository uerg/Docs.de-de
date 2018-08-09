---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-debugging-capabilities
title: Grundlegendes zu Debuggingfunktionen von ASP.NET-AJAX | Microsoft-Dokumentation
author: scottcate
description: Die Möglichkeit, Code zu debuggen, ist eine Qualifikation, die jeder Entwickler in ihren Arsenal unabhängig von der Technologie haben soll, das sie verwenden. Während viele Entwickler sind...
ms.author: aspnetcontent
ms.date: 03/28/2008
ms.assetid: 7f9380c6-19f7-4c82-a019-916ec6dffc9c
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-debugging-capabilities
msc.type: authoredcontent
ms.openlocfilehash: 95c2487f26109cbdd8c76dc6f269f37264f5e34b
ms.sourcegitcommit: 028ad28c546de706ace98066c76774de33e4ad20
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/08/2018
ms.locfileid: "39655445"
---
<a name="understanding-aspnet-ajax-debugging-capabilities"></a>Grundlegendes zu Debuggingfunktionen von ASP.NET-AJAX
====================
durch [Scott Cate](https://github.com/scottcate)

[PDF herunterladen](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial06_Debugging_MS_Ajax_Applications_cs.pdf)

> Die Möglichkeit, Code zu debuggen, ist eine Qualifikation, die jeder Entwickler in ihren Arsenal unabhängig von der Technologie haben soll, das sie verwenden. Während viele Entwickler daran gewöhnt sind, für die Verwendung von Visual Studio .NET oder die Web Developer Express, um ASP.NET-Anwendungen zu debuggen, die VB.NET oder C#-Code verwenden, sind einige nicht bewusst, dass es auch äußerst nützlich für das Debuggen von clientseitigen Code wie z.B. JavaScript. Die gleiche Art von .NET-Anwendungen Debuggen verwendeten Techniken kann auch auf AJAX-aktivierter Anwendungen und genauer gesagt die ASP.NET AJAX-Anwendungen angewendet werden.


## <a name="debugging-aspnet-ajax-applications"></a>Debuggen von ASP.NET AJAX-Anwendungen

Dan Wahlin

Die Möglichkeit, Code zu debuggen, ist eine Qualifikation, die jeder Entwickler in ihren Arsenal unabhängig von der Technologie haben soll, das sie verwenden. Es versteht die Erläuterung der verschiedenen debugging-Optionen, die verfügbar sind eine enorme Menge Zeit auf ein Projekt, und vielleicht sogar einige Kopfschmerzen speichern kann. Während viele Entwickler daran gewöhnt sind, für die Verwendung von Visual Studio .NET oder die Web Developer Express, um ASP.NET-Anwendungen zu debuggen, die VB.NET oder C#-Code verwenden, sind einige nicht bewusst, dass es auch äußerst nützlich für das Debuggen von clientseitigen Code wie z.B. JavaScript. Die gleiche Art von .NET-Anwendungen Debuggen verwendeten Techniken kann auch auf AJAX-aktivierter Anwendungen und genauer gesagt die ASP.NET AJAX-Anwendungen angewendet werden.

In diesem Artikel sehen Sie, wie Visual Studio 2008 und einige andere Tools können, zum Debuggen von ASP.NET AJAX-Anwendungen verwendet werden, um Fehler und andere Probleme schnell zu finden. Diese Diskussion enthalten Informationen zum Aktivieren von InternetExplorer 6 oder höher für das Debuggen mithilfe von Visual Studio 2008 und den Skript-Explorer Code schrittweise durchlaufen sowie und andere kostenlose Tools, z. B. Web Development Helper verwenden. Sie erfahren auch Gewusst wie: Debuggen von ASP.NET AJAX-Anwendungen in Firefox verwenden, eine Erweiterung Firebug und Sie können mit dem Namen Sie JavaScript-Code direkt im Browser ohne von anderen Tools zu durchlaufen. Sie werden schließlich für Klassen in ASP.NET AJAX-Bibliothek eingeführt werden, die mit verschiedenen Debugaufgaben wie Ablaufverfolgung und assertionsanweisungen Code helfen können.

Bevor Sie versuchen, das Debuggen von Seiten, die in Internet Explorer, es einige grundlegende Schritte benötigen Sie zum Ausführen gibt, um sie für das Debuggen zu aktivieren, angezeigt werden soll. Werfen wir einen Blick auf einige grundlegende Einrichtung von Anforderungen, die ausgeführt werden, um zu beginnen.

## <a name="configuring-internet-explorer-for-debugging"></a>Konfigurieren von InternetExplorer für das Debuggen

Die meisten Benutzer nicht von Interesse, sehen die JavaScript-Probleme auf einer Website mit Internet Explorer angezeigt. In der Tat wäre nicht der durchschnittliche Benutzer auch wissen, was zu tun, wenn sie eine Fehlermeldung gesehen haben. Daher werden Optionen für das Debuggen im Browser standardmäßig deaktiviert. Allerdings ist es sehr einfach, aktivieren Sie das Debuggen, und fügen Sie sie verwenden, wie Sie neue AJAX-Anwendungen zu entwickeln.

Aktivieren von Debugfunktionen, wechseln zu "Tools Internetoptionen" auf das Internet Explorer-Menü, und wählen Sie die Registerkarte "Erweitert". Stellen Sie sicher, dass die folgenden Elemente deaktiviert sind, in dem Abschnitt durchsuchen:

- Deaktivieren Sie Skriptdebugging (Internet Explorer)
- Deaktivieren Sie Skriptdebugging (andere)

Obwohl nicht erforderlich, wenn versuchen Sie, die Debuggen einer Anwendung, die Sie wahrscheinlich alle JavaScript-Fehler auf der Seite unmittelbar sichtbar und unverkennbar sein sollten. Sie können erzwingen, dass alle Fehler, durch Aktivieren des Kontrollkästchens "Display eine Benachrichtigung zu jedem Skriptfehler," ein Meldungsfeld angezeigt werden soll. Dies ist, zwar eine hervorragende Möglichkeit, die während der Entwicklung einer Anwendungs aktivieren können sie schnell werden ärgerlich, wenn Sie andere Websites, da die Wahrscheinlichkeit, dass der JavaScript-Fehler auftreten, recht gut sind nur durchlesen sind.

Abbildung 1 zeigt, welche im erweiterten Dialogfeld Internet Explorer sollte aussehen, nachdem es für das Debuggen ordnungsgemäß konfiguriert wurde.


[![Konfigurieren von Internet Explorer für das Debuggen.](understanding-asp-net-ajax-debugging-capabilities/_static/image2.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image1.png)

**Abbildung 1**: Konfigurieren von Internet Explorer für das Debuggen.  ([Klicken Sie, um das Bild in voller Größe anzeigen](understanding-asp-net-ajax-debugging-capabilities/_static/image3.png))


Sobald das Debuggen aktiviert wurde, sehen Sie ein neues Menüelement im Menü "Ansicht" mit dem Namen Script-Debugger angezeigt werden. Es hat zwei Optionen zur Verfügung, einschließlich öffnen und die Unterbrechung an die nächste Anweisung. Beim Öffnen aktiviert ist, werden Sie aufgefordert werden zum Debuggen der Seite in Visual Studio 2008 (Beachten Sie, dass Visual Web Developer Express für das Debuggen auch verwendet werden kann). Wenn Visual Studio .NET ausgeführt wird, können Sie diese Instanz verwenden oder erstellen eine neue Instanz auswählen. Wenn Unterbrechung am nächsten Anweisung ausgewählt ist, werden Sie aufgefordert werden auf die Seite zu debuggen, wenn JavaScript-Code ausgeführt wird. Wenn JavaScript-Code in das OnLoad-Ereignis der Seite ausgeführt wird, können Sie die Seite, um das Auslösen einer Debugsitzung aktualisieren. Wenn JavaScript-Code ausgeführt wird, nachdem eine Schaltfläche geklickt wird führt der Debugger dann sofort, nachdem Sie die Schaltfläche geklickt wird.

> [!NOTE]
> Wenn Sie in Windows Vista mit User Access Control (UAC) aktiviert, und Sie haben Visual Studio 2008, die zur Ausführung als Administrator, kann Visual Studio nicht an den Prozess angefügt werden soll, wenn Sie aufgefordert werden, um anzufügen. Um dieses Problem zu umgehen, starten Sie Visual Studio zuerst, und verwenden Sie diese Instanz zum Debuggen.

Obwohl im nächste Abschnitt zum Debuggen einer ASP.NET AJAX-Seite direkt aus Visual Studio 2008 veranschaulicht wird, ist mit der Script-Debugger von Internet Explorer-Option hilfreich, wenn eine Seite bereits geöffnet ist, und Sie es genauer zu untersuchen möchten.

## <a name="debugging-with-visual-studio-2008"></a>Debuggen mit Visual Studio 2008

Visual Studio 2008 bietet Debugfunktionen, die Entwickler auf der ganzen Welt auf täglich für .NET-Anwendungen Debuggen basieren. Der integrierte Debugger können Sie Code, anzeigen, dass Objektdaten, sehen Sie sich für bestimmte Variablen, die Aufrufliste und vieles mehr überwachen schrittweise durchlaufen. Zusätzlich zum Debuggen von VB.NET oder C#-Code, der Debugger ist auch nützlich für das Debuggen von ASP.NET AJAX-Anwendungen und lässt Sie JavaScript-Code Zeile für Zeile durchlaufen. Die Details, die Fokus auf Techniken, die verwendet werden kann befolgen, um eine clientseitige Skriptdateien, anstatt eine Discourse auf Gesamtprozesses zum Debuggen von Anwendungen mit Visual Studio 2008 zu debuggen.

Der Prozess des Debuggens einer Seite in Visual Studio 2008 kann auf verschiedene Weise gestartet werden. Erstens können Sie die im vorherigen Abschnitt beschriebenen Option für den Script-Debugger von Internet Explorer. Dies funktioniert gut, wenn eine Seite bereits im Browser geladen wird, und starten Sie debuggen möchten. Alternativ können Sie mit der rechten Maustaste auf eine ASPX-Seite im Projektmappen-Explorer, und wählen als Startseite festlegen, aus dem Menü. Wenn Sie das Debuggen von ASP.NET-Seiten gewöhnt haben dann noch wahrscheinlich erfolgt. Wenn F5 gedrückt wird, kann die Seite gedebuggt werden. Allerdings während Sie in der Regel einen Haltepunkt an einer beliebigen Stelle festlegen können in VB.NET oder C#-Code werden soll, die nicht immer der Fall mit JavaScript ist Sie als Nächstes sehen.

*Eingebettete und externe Skripts im Vergleich*

Der Debugger von Visual Studio 2008 behandelt JavaScript auf einer Seite, die sich von externen JavaScript-Dateien eingebettet. Sie können mit externen Skript-Dateien öffnen Sie die Datei und legen einen Haltepunkt auf eine beliebige Zeile, die Sie auswählen. Haltepunkte können festgelegt werden, indem Sie auf den grauen Bereich auf der linken Seite des Code-Editor-Fensters. Wenn JavaScript eingebettet ist, direkt in eine Seite mit den `<script>` -Tag, einen Haltepunkt festlegen, indem Sie auf den grauen Bereich ist keine Option. Versucht, einen Haltepunkt in einer Zeile des eingebetteten Skripts festlegen führt zu einer Warnung, aus der hervorgeht, "Dieser einen gültigen Speicherort für einen Haltepunkt is not".

Sie können Umgehung dieses Problems abrufen, indem Sie sich, den Code in eine externe JS-Datei und verweisen auf das Src-Attribut mit dem &lt;Skript&gt; Tag:


[!code-html[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample1.html)]

Was geschieht, wenn den Code in einer externen Datei verschieben oder ist keine Option erfordert mehr Arbeit als lohnt? Während Sie einen Haltepunkt mit dem Editor festlegen können, können Sie die Debugger-Anweisung direkt in den Code hinzufügen, die, in dem Sie debuggen möchten. Sie können auch die Sys.Debug-Klasse verfügbar in der ASP.NET AJAX-Bibliothek verwenden, zum Starten des Debuggings erzwingen. Erfahren Sie mehr über die Sys.Debug-Klasse weiter unten in diesem Artikel.

Ein Beispiel der Verwendung der `debugger` Schlüsselwort wird in Programmausdruck 1 gezeigt. Dieses Beispiel zwingt den Debugger an die richtige unterbrochen, bevor eine eine Update-Funktion aufgerufen wird.

**Codebeispiel 1. Verwenden das Debugger-Schlüsselwort, um die erzwingen, dass den Visual Studio .NET Debugger unterbrochen.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample2.js)]

Nachdem der Debugger-Anweisung erreicht wird, die Sie werden aufgefordert, die Seite mit Visual Studio .NET zu debuggen und können beginnen, den Code schrittweise durchlaufen. Während Sie diese durchführen, ein Problem mit dem Zugriff auf ASP.NET AJAX-Bibliothek-Skriptdateien, die wir auf der Seite verwendeten auftreten kann sehen Sie sich mithilfe von Visual Studio. NET Skript-Explorer.

## <a name="using-visual-studio-net-windows-to-debug"></a>Verwenden zum Debuggen von Visual Studio .NET Windows

Nach eine Debugsitzung gestartet wird und Sie beginnen mit der F11-Taste standardmäßig Code durchlaufen, treten ggf. das Fehlerdialogfeld in finden Sie unter Abbildung 2, es sei denn, alle Skriptdateien, die verwendet werden, auf der Seite geöffnet und für das Debuggen verfügbar sind.


[![Das Fehlerdialogfeld angezeigt, wenn kein Quellcode für das Debuggen verfügbar ist.](understanding-asp-net-ajax-debugging-capabilities/_static/image5.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image4.png)

**Abbildung 2**: Dialogfeld "Fehler" angezeigt, wenn kein Quellcode für das Debuggen verfügbar ist.  ([Klicken Sie, um das Bild in voller Größe anzeigen](understanding-asp-net-ajax-debugging-capabilities/_static/image6.png))


Dieses Dialogfeld wird angezeigt, da Sie sicher, dass Visual Studio .NET-Gewusst wie: Abrufen des Quellcodes einige Skripts verwiesen wird, von der Seite nicht. Dies kann sehr frustrierend sein zunächst eine einfache Lösung besteht. Nachdem Sie eine Debugsitzung gestartet und einen Haltepunkt erreicht haben, wechseln Sie zum Debuggen von Windows-Skript-Explorer-Fenster auf das Menü "Visual Studio 2008", oder verwenden Sie den Hotkey Strg + Alt + N.

> [!NOTE]
> Wenn Sie nicht, dass die Skript-Explorer im Menü aufgelistet sehen, fahren Sie mit **Tools** > **anpassen** > **Befehle** im Visual Studio .NET auf. Suchen Sie die **Debuggen** Eintrag in die Kategorien aus, und klicken Sie darauf, um alle im Kontextmenü verfügbaren Einträge anzuzeigen. Scrollen Sie zum Skript-Explorer, und ziehen sie Sie auf das Debuggen von Windows im Menü oben erwähnt, in der Liste der Befehle. Dadurch wird den Eintrag der Skript-Explorer-Menü jedes Mal zur Verfügung, wenn Sie Visual Studio .NET ausführen.

Die Skript-Explorer kann verwendet werden, zeigen Sie alle Skripts, die auf einer Seite verwendet, und sie im Code-Editor zu öffnen. Doppelklicken Sie auf der ASPX-Seite, die gerade gedebuggt werden, um sie im Code-Editor-Fenster zu öffnen, nach dem Skript-Explorer öffnen. Führen Sie die gleiche Aktion für alle anderen Skripts in den Skript-Explorer angezeigt. Nachdem alle Skripts im Code-Fenster geöffnet sind Sie können drücken Sie F11 (und verwenden Sie die anderen Debug-Hotkeys), um den Code schrittweise. Abbildung 3 zeigt ein Beispiel für die Skript-Explorer. Die aktuelle Datei im Debugmodus befindlichen (Demo.aspx) aufgeführt sowie zwei benutzerdefinierte Skripts und zwei Skripts, die dynamisch vom ASP.NET AJAX ScriptManager auf der Seite eingefügt.


[![Die Skript-Explorer bietet einfachen Zugriff auf Skripts, die auf einer Seite verwendet.](understanding-asp-net-ajax-debugging-capabilities/_static/image8.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image7.png)

**Abbildung 3**. Die Skript-Explorer bietet einfachen Zugriff auf Skripts, die auf einer Seite verwendet.  ([Klicken Sie, um das Bild in voller Größe anzeigen](understanding-asp-net-ajax-debugging-capabilities/_static/image9.png))


Andere Windows auch verwendet werden, können um nützliche Informationen liefern, wie Sie Code auf einer Seite durchlaufen schrittweise. Fenster "lokal" können Sie z. B. die Werte verschiedener Variablen, die verwendet werden, die auf der Seite das Direktfenster ein, um bestimmte Variablen oder Bedingungen auswerten und die Ausgabe anzuzeigen. Das Fenster "Ausgabe" können auch zum Anzeigen von Trace-Anweisungen, die Verwendung der Sys.Debug.trace-Funktion (die weiter unten in diesem Artikel behandelt werden) oder Internet Explorer Debug.writeln-Funktion geschrieben.

Wie Sie Code mit dem Debugger schrittweise durchlaufen können Sie die Maus über Variablen in den Code, um den Wert anzuzeigen, den sie zugewiesen sind. Allerdings nicht der Skriptdebugger gelegentlich nichts angezeigt, wie Sie den Mauszeiger über einem bestimmten JavaScript-Variable. Um den Wert anzuzeigen, markieren Sie die Anweisung oder eine Variable, die Sie versuchen, finden im Code-Editor-Fenster, und klicken Sie dann mit der Maus darauf. Aber diese Technik in jedem Fall funktionieren nicht, werden oft können Sie den Wert ohne suchen Sie in einem anderen Debugfenster, z. B. das Fenster "lokal" Sie.

Ein Videotutorial zur Veranschaulichung einiger der hier beschriebenen Features angezeigt werden kann, am [ http://www.xmlforasp.net ](http://www.xmlforasp.net).

## <a name="debugging-with-web-development-helper"></a>Debuggen mit Web Development Helper

Obwohl Visual Studio 2008 (und Visual Web Developer Express 2008) sehr leistungsstarken debugging-Tools sind, stehen zusätzliche Optionen, die auch verwendet werden, können mehr geringfügiger sind. Zu den neuesten Tools veröffentlicht werden ist das Web Development Helper. Microsofts Nikhil Kothari (eine der wichtigsten ASP.NET AJAX-Architekten bei Microsoft), schrieb diese hervorragende Tool, das viele verschiedene Aufgaben ausführen können, von einfachen Debuggen zur Anzeige der HTTP-Anforderung und Antwort-Nachrichten. Web Development Helper heruntergeladen werden kann, auf [ http://projects.nikhilk.net/Projects/WebDevHelper.aspx ](http://projects.nikhilk.net/Projects/WebDevHelper.aspx).

Web Development Helper kann direkt in Internet Explorer verwendet werden, dadurch wird es praktisch, zu verwenden. Es wird durch Auswählen von Tools Web Development Helper im Internet Explorer-Menü gestartet. Dadurch wird das Tool im unteren Bereich des Browsers geöffnet, die eine gute Sache, da Sie den Browser, um verschiedene Aufgaben wie z. B. HTTP-Anforderung und Antwort nachrichtenprotokollierung lassen keine. Abbildung 4 zeigt, wie Web Development Helper in Aktion aussieht.


[![Web Development Helper](understanding-asp-net-ajax-debugging-capabilities/_static/image11.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image10.png)

**Abbildung 4**: Web Development Helper ([klicken Sie, um das Bild in voller Größe anzeigen](understanding-asp-net-ajax-debugging-capabilities/_static/image12.png))


Web Development Helper ist ein Tool, das Sie verwenden nicht auf den Code zeilenweise, wie mit Visual Studio 2008 zu durchlaufen. Es kann jedoch verwendet werden, zum Anzeigen der Ausgabe der Ablaufverfolgung, leicht zu Variablen in einem Skript auswerten oder untersuchen Daten in ein JSON-Objekt ist. Es ist auch sehr nützlich für das Anzeigen von Daten, die in und aus einer ASP.NET AJAX-Seite und einem Server übergeben wird.

Nach Web Development Helper in Internet Explorer geöffnet ist, muss das Skriptdebuggen aktiviert werden, durch Skriptdebugging aktivieren-Skript aus dem Web Development Helper-Menü auswählen, wie weiter oben in Abbildung 4 dargestellt. Dies ermöglicht das Tool zum Abfangen von Fehlern, die auftreten, wenn eine Seite ausgeführt wird. Außerdem können ganz einfach auf Meldungen zur Ablaufverfolgung, die in der Seite ausgegeben werden. Wählen Sie zum Anzeigen von Ablaufverfolgungsinformationen aus, oder führen Sie die Skriptbefehle zum Testen von anderer Funktionen innerhalb einer Seite, Skript-Skript-Konsole anzeigen, über das Menü "Web Development Helper". Dies ermöglicht den Zugriff auf ein Befehlsfenster, und eine einfache Direktfenster.

*Anzeigen von Meldungen zur Ablaufverfolgung und JSON-Objektdaten*

Das "Direktfenster" dienen zum Ausführen der Befehle des Skripts oder sogar zu laden und speichern Skripts, die zum Testen von anderer Funktionen auf einer Seite verwendet werden. Das Befehlsfenster zeigt Trace oder Debug-Nachrichten, die von der Seite angezeigt wird geschrieben. Codebeispiel 2 gezeigt, wie eine Ablaufverfolgungsmeldung, die mithilfe von Internet Explorer Debug.writeln-Funktion zu schreiben.

**Codebeispiel 2. Schreiben eine der clientseitigen Ablaufverfolgung-Nachricht mithilfe der Debug-Klasse.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample3.js)]

Wenn die LastName-Eigenschaft einen Wert von Doe enthält, zeigt Web Development Helper an "Name der Person: Doe" in der Skriptkonsole-Befehlsfenster (vorausgesetzt, dass Debuggen aktiviert wurde). Web Development Helper Fügt ein Objekt der obersten Ebene DebugService auch Seiten, die verwendet werden können, um Ablaufverfolgungsinformationen zu schreiben, oder zeigen den Inhalt der JSON-Objekte hinzu. Codebeispiel 3 zeigt ein Beispiel der Verwendung der DebugService Klasse Trace-Funktion.

**Codebeispiel 3. Verwenden Web Development Helper DebugService-Klasse, eine Ablaufverfolgungsmeldung zu schreiben.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample4.js)]

Ein nützliches Feature der DebugService-Klasse ist, dass es funktioniert auch, wenn debugging nicht aktiviert ist in Internet Explorer, die Dies erleichtert immer für die Ablaufverfolgung zugreifen, wenn Web Development Helper ausgeführt wird. Wenn das Tool zum Debuggen einer Seite verwendet wird, werden Trace-Anweisungen, da der Aufruf window.debugService "false" zurückgibt, wird ignoriert.

Die DebugService-Klasse ermöglicht es auch Daten von JSON-Objekts, mit dem Web Development Helper-Inspektor-Fenster angezeigt werden. Programmausdruck 4 erstellt ein einfaches jsonobjekt, das Person-Daten enthält. Nachdem das Objekt erstellt wurde, erfolgt ein Aufruf an die DebugService überprüfen Klasse Funktion auf, um das JSON-Objekt, visuell überprüft werden können.

**Programmausdruck 4. Verwenden die debugService.inspect-Funktion, um die JSON-Objektdaten anzuzeigen.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample5.js)]

Die GetPerson()-Funktion aufrufen, auf der Seite oder über das "Direktfenster" führt das Objekt Inspector-Dialogfeld angezeigt wird, wie in Abbildung 5 dargestellt. Eigenschaften in das Objekt können dynamisch geändert werden, hervorgehoben, indem sie an, das Ändern des Werts in das Textfeld Wert angezeigt, und klicken Sie dann auf den Link "Aktualisieren". Verwenden der Seitenprüfung Objekt können sie ganz leicht zum Anzeigen von Daten von JSON-Objekts, und experimentieren Sie mit unterschiedliche Werten auf Eigenschaften anwenden.

*Debuggen von Fehlern*

Zusätzlich zum Zulassen der Ablaufverfolgung und JSON-Objekte, die angezeigt werden, kann Web Development Helper auch dabei helfen, Debuggen von Fehlern in einer Seite. Wenn ein Fehler auftritt, werden Sie aufgefordert, weiterhin die nächste Zeile des Codes oder das Skript debuggen (siehe Abbildung 6). Das Skriptfehler Dialogfeld Fenster zeigt die vollständige Aufrufliste sowie Zeilennummern, sodass Sie problemlos erkennen können, in denen sich Probleme innerhalb eines Skripts sind.


[![Mithilfe von Objekt-Inspektor-Fenster an ein JSON-Objekt.](understanding-asp-net-ajax-debugging-capabilities/_static/image14.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image13.png)

**Abbildung 5**: mithilfe des Objekt-Inspektor-Fenster an ein JSON-Objekt.  ([Klicken Sie, um das Bild in voller Größe anzeigen](understanding-asp-net-ajax-debugging-capabilities/_static/image15.png))


Die debuggingoption auswählen, können Sie Ausführen von skriptanweisungen direkt im Web Development Helper "Direktfenster" den Wert der Variablen anzeigen, sowie weitere JSON-Objekte zu schreiben. Wenn die gleiche Aktion, die den Fehler ausgelöst wird erneut ausgeführt, und Visual Studio 2008 auf dem Computer verfügbar ist, werden Sie aufgefordert, eine Debugsitzung starten, sodass Sie den Code zeilenweise, wie im vorherigen Abschnitt erläutert schrittweise durchlaufen können.


[![Web Development Helper Skript-Fehlerdialogfeld](understanding-asp-net-ajax-debugging-capabilities/_static/image17.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image16.png)

**Abbildung 6**: Web Development Helper-Skript-Fehlerdialogfeld ([klicken Sie, um das Bild in voller Größe anzeigen](understanding-asp-net-ajax-debugging-capabilities/_static/image18.png))


*Anforderung und Antwort-Nachrichten überprüfen*

Während des Debuggens von ASP.NET AJAX-Seiten ist es oft nützlich, um die Anforderung und Antwort-Nachrichten, die zwischen einer Seite und den Server gesendet werden, finden Sie unter. Anzeigen des Inhalts in Nachrichten, können Sie ermitteln, ob die richtigen Daten sowie die Größe der Nachrichten übergeben wird. Web Development Helper bietet eine hervorragende HTTP-Nachricht Logger-Funktion, die es einfach macht, die Daten als unformatierter Text oder in einem besser lesbaren Format anzuzeigen.

Um ASP.NET AJAX-Anforderung und Antwort-Nachrichten anzuzeigen, muss die HTTP-Protokollierung aktiviert werden, durch Auswahl von HTTP-Aktivieren der HTTP-Protokollierung über das Menü "Web Development Helper". Nach der Aktivierung können alle Nachrichten aus der aktuellen Seite im HTTP-Protokoll-Viewer angezeigt werden die hierzu zeigen HTTP-Protokolle HTTP zugegriffen werden kann.

Obwohl sicherlich nützlich Anzeigen des unformatierten Texts in den einzelnen Anforderung/Antwort-Nachrichten gesendet ist (und eine Option im Web Development Helper), ist es oft einfacher, um Nachrichtendaten in einer grafisch angezeigt. Nachdem die HTTP-Protokollierung aktiviert wurde und die Nachrichten wurden angemeldet, können die Nachrichtendaten durch Doppelklicken auf die Nachricht in der HTTP-Protokoll-Viewer angezeigt werden. Dies ermöglicht es Ihnen an alle Header, die eine Nachricht als auch die eigentliche Nachricht zugeordnete Inhalt. Abbildung 7 zeigt ein Beispiel für eine Anforderungsnachricht und die Antwortnachricht, die im HTTP-Protokoll-Viewer-Fenster angezeigt.


[![Verwenden die HTTP-Protokoll-Viewer zum Anzeigen von Daten von Anforderungs- und Antwortnachrichten Nachricht.](understanding-asp-net-ajax-debugging-capabilities/_static/image20.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image19.png)

**Abbildung 7**: mithilfe der HTTP-Protokoll-Viewer zum Anzeigen von Daten von Anforderungs- und Antwortnachrichten Nachricht.  ([Klicken Sie, um das Bild in voller Größe anzeigen](understanding-asp-net-ajax-debugging-capabilities/_static/image21.png))


Die HTTP-Protokoll-Viewer wird automatisch analysiert JSON-Objekten und zeigt sie die mit einer Strukturansicht somit schnell und einfach die Daten des Objekts anzeigen. Bei einem UpdatePanel-Steuerelement in einer ASP.NET AJAX-Seite verwendet wird, bricht der Viewer jeder Teil der Nachricht in einzelne Teile wie in Abbildung 8 gezeigt. Dies ist eine großartige Funktion, die es anzeigen und verstehen, was in der Nachricht im Vergleich zum Anzeigen der Daten für die unformatierte Nachricht ist wesentlich einfacher macht.


[![Ein UpdatePanel-Antwortnachricht mit dem HTTP-Protokoll-Viewer angezeigt werden soll.](understanding-asp-net-ajax-debugging-capabilities/_static/image23.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image22.png)

**Abbildung 8**: ein UpdatePanel-Response-Nachricht angezeigt, mit dem HTTP-Protokoll-Viewer.  ([Klicken Sie, um das Bild in voller Größe anzeigen](understanding-asp-net-ajax-debugging-capabilities/_static/image24.png))


Es gibt mehrere andere Tools, die zum Anzeigen von Anforderung und Antwort-Nachrichten zusätzlich zu Web Development Helper verwendet werden können. Eine weitere gute Option ist Fiddler handelt es sich kostenlos erhältlich unter [ http://www.fiddlertool.com ](http://www.fiddlertool.com). Zwar Fiddler nicht hier erläutert wird, ist es auch eine gute Option, wenn Sie Nachrichten-Header und Daten gründlich untersuchen möchten.

## <a name="debugging-with-firefox-and-firebug"></a>Debuggen mit Firefox und Firebug

Während der Internet Explorer immer noch den am häufigsten verwendeten Browser ist, anderen Browsern wie Firefox sind sehr beliebt geworden, und Sie werden mehr und mehr verwendet werden. Daher sollten Sie zum Anzeigen und Debuggen Ihrer ASP.NET AJAX-Seiten in Firefox und Internet Explorer, stellen Sie sicher, dass Ihre Anwendungen ordnungsgemäß funktioniert. Firefox direkt in Visual Studio 2008 zu verknüpfen kann nicht für das Debuggen, hat aber-Erweiterung Firebug, die zum Debuggen von Seiten verwendet werden kann. Firebug kann kostenlos heruntergeladen werden, indem Sie zu [ http://www.getfirebug.com ](http://www.getfirebug.com).

Firebug bietet eine umfassende Debugumgebung, mit denen Code Zeile für Zeile durchlaufen, das Zugriff auf alle Skripts, die innerhalb einer Seite verwendet, das Anzeigen von DOM-Strukturen, CSS-Stile und sogar Nachverfolgen von Ereignissen, die auftreten, auf einer Seite angezeigt werden können. Nach Abschluss der Installation ist Firebug möglich durch Tools Firebug öffnen Firebug im Firefox-Menü auswählen. Firebug ist wie Web Development Helper direkt im Browser verwendet, obwohl es auch als eigenständige Anwendung verwendet werden kann.

Sobald Firebug ausgeführt wird, können Haltepunkte auf beliebige Zeile in einer JavaScript-Datei festgelegt werden, ob das Skript auf einer Seite oder nicht eingebettet ist. Um einen Haltepunkt festzulegen, laden Sie zuerst die entsprechende Seite, die Sie in Firefox debuggen möchten. Sobald die Seite geladen wird, wählen Sie das Skript zum Debuggen von Firebug des Skripts Dropdown-Liste ein. Alle Skripts, die verwendet werden, indem Sie die Seite werden angezeigt. Ein Haltepunkt ist festgelegt, indem Sie auf die Firebug grauen Taskleistenbereich in der Zeile, in dem der Haltepunkt muss wie in Visual Studio 2008 landen sollen.

Sobald ein Haltepunkt in Firebug festgelegt wurde, können Sie die Aktion erforderlich, um das Skript auszuführen, das debuggt werden, z. B. auf eine Schaltfläche oder aktualisieren den Browser, um das OnLoad-Ereignis auslösen muss ausführen. Ausführung unterbrochen automatisch in der Zeile mit dem Haltepunkt. Abbildung 9 zeigt ein Beispiel für einen Haltepunkt an, der ausgelöst wurde, in Firebug.


[![Behandeln Haltepunkte in Firebug.](understanding-asp-net-ajax-debugging-capabilities/_static/image26.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image25.png)

**Abbildung 9**: Haltepunkte in Firebug behandeln.  ([Klicken Sie, um das Bild in voller Größe anzeigen](understanding-asp-net-ajax-debugging-capabilities/_static/image27.png))


Sobald ein Haltepunkt erreicht wird können schrittweise, überspringen oder Prozedurschritt aus Code, die mit den Pfeilschaltflächen. Wie Sie Code durchlaufen schrittweise, werden Skriptvariablen in der rechte Bereich des Debuggers können Sie Werte und Detailinformationen zu den Objekten angezeigt. Firebug enthält auch eine Aufrufliste Dropdown-Liste, um Ausführungsschritte des Skripts anzuzeigen, die auf die aktuelle Zeile, die im Debugmodus befindlichen geführt haben.

Firebug enthält außerdem ein Konsolenfenster, das zum Testen der verschiedenen skriptanweisungen Variablen und Anzeigen der Ausgabe der Ablaufverfolgung verwendet werden kann. Der Zugriff erfolgt durch Klicken auf die Registerkarte "Konsole" am oberen Rand des Fensters Firebug. Die Seite im Debugmodus befindlichen kann auch "überprüft werden" um die DOM-Struktur und Inhalt finden in der Registerkarte "prüfen". Wie werden die verschiedenen DOM-Elemente, die im Inspektor-Fenster angezeigt, den entsprechenden Teil der Seite für die Mouseover hervorgehoben somit leicht zu erkennen, in dem das Element auf der Seite verwendet wird. Ein angegebenes Element zugeordneten Attributwerte können geändert werden, "live" Experimentieren mit verschiedenen Breiten, Stile usw. auf ein Element anwenden. Dies ist ein nützliches Feature, das Sie ständig zwischen dem Quellcode-Editor und den Firefox-Browser, wie einfache Änderungen Auswirkungen einer Seite anzeigen wechseln erspart.

Abbildung 10 zeigt ein Beispiel für das DOM-Inspektor zu verwenden, um ein Textfeld mit dem Namen TxtCountry auf der Seite zu suchen. Der Inspektor Firebug kann auch verwendet werden, an CSS-Formatvorlagen, die in einer Seite sowie Ereignisse, die auftreten, z.B. für die nachverfolgung von mausbewegungen, Mausklicks und mehr verwendet.


[![Verwenden Firebug des DOM-Inspektors.](understanding-asp-net-ajax-debugging-capabilities/_static/image29.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image28.png)

**Abbildung 10**: mithilfe von Firebug DOM-Inspektors.  ([Klicken Sie, um das Bild in voller Größe anzeigen](understanding-asp-net-ajax-debugging-capabilities/_static/image30.png))


Firebug bietet einen einfachen Weg auf eine Seite direkt in Firefox als auch ein hervorragendes Tool für das Prüfen von verschiedenen Elementen auf der Seite schnell Debuggen.

## <a name="debugging-support-in-aspnet-ajax"></a>Debugging-Unterstützung in ASP.NET-AJAX

Die ASP.NET AJAX-Bibliothek enthält viele verschiedene Objektklassen, die verwendet werden können, um den Prozess des Hinzufügens von AJAX-Funktionen in einer Webseite zu vereinfachen. Sie können diese Klassen verwenden, Suchen von Elementen innerhalb einer Seite und Bearbeiten dieser, fügen neue Steuerelemente, Webdienste aufrufen und sogar die Ereignisse behandeln. Die ASP.NET AJAX-Bibliothek enthält außerdem Klassen, die verwendet werden können, um den Prozess der debugging-Seiten zu verbessern. In diesem Abschnitt Einführung in die Klasse Sys.Debug und wie es in Anwendungen verwendet werden kann.

*Verwenden der Sys.Debug-Klasse*

Die Sys.Debug-Klasse (eine JavaScript-Klasse befindet sich im Sys-Namespace) kann verwendet werden, um führen mehrere verschiedene Funktionen, einschließlich der Schreiben der Ablaufverfolgungsausgabe, Ausführen von Code Assertionen und erzwingen die zu einem Fehler, damit er debuggt werden kann. Es ist häufig in der ASP.NET AJAX-Bibliothek-Debug-Dateien (standardmäßig unter c:\Programme\Microsoft c:\Programme\Microsoft ASP.NET\ASP.NET 2.0 AJAX Extensions\v1.0.61025\MicrosoftAjaxLibrary\System.Web.Extensions\1.0.61025.0 installiert) verwendet, um bedingte Tests (Ausführen wird aufgerufen, Assertionen) sicherzustellen, dass die Parameter werden ordnungsgemäß übergeben, um Funktionen, dass Objekte die erwarteten Daten enthalten und Trace-Anweisungen zu schreiben.

Sys.Debug-Klasse stellt mehrere unterschiedliche Funktionen, die zum Verarbeiten der Ablaufverfolgung, Code Assertionen oder Fehler, wie in Tabelle 1 dargestellt verwendet werden können.

**Tabelle 1. Sys.Debug Klassenfunktionen.**

| **Funktionsname** | **Beschreibung** |
| --- | --- |
| assert(condition, message, displayCaller) | Bestätigt, dass der Bedingungsparameter true ist. Wenn die getestete Bedingung false ist, wird ein Meldungsfeld zur Anzeige des Werts der Message-Parameter verwendet werden. Wenn der DisplayCaller-Parameter auf "true" festgelegt ist, zeigt die Methode auch Informationen über den Aufrufer. |
| clearTrace() | Löscht die Anweisungen aus der Ablaufverfolgung von Vorgängen. |
| fail(message) | Bewirkt, dass das Programm beenden und den Debugger unterbrechen. Der Meldungsparameter kann verwendet werden, um einen Grund für den Fehler angeben. |
| Trace(Message) | Schreibt den Message-Parameter, um die Ausgabe der Ablaufverfolgung. |
| TraceDump ("Object", "Name") | Gibt die Daten eines Objekts in einem lesbaren Format an. Der Name-Parameter kann verwendet werden, um eine Bezeichnung für die Ablaufverfolgung Dump bereitzustellen. Alle untergeordneten Objekte innerhalb des Objekts wird gesichert werden standardmäßig geschrieben werden. |

Der clientseitigen Ablaufverfolgung kann in wesentlich auf die gleiche Weise wie die Funktionalität für die datenablaufverfolgung in ASP.NET verfügbaren verwendet werden. Sie können verschiedene Nachrichten an problemlos ohne Unterbrechung des Ablaufs der Anwendung angezeigt werden. Programmausdruck 5 zeigt ein Beispiel der Verwendung der Sys.Debug.trace-Funktion in das Ablaufverfolgungsprotokoll geschrieben. Diese Funktion nimmt einfach die Meldung, die als Parameter geschrieben werden soll.

**Programmausdruck 5 an. Mithilfe der Sys.Debug.trace-Funktion.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample6.js)]

Wenn Sie den Code in Listing 5 ausführen, wird Ausgabe der Ablaufverfolgung auf der Seite nicht angezeigt. Die einzige Möglichkeit zum Anzeigen ist ein Konsolenfenster, verfügbar in Visual Studio .NET, Web Development Helper oder Firebug verwenden. Wenn Sie die Ausgabe der Ablaufverfolgung auf der Seite finden Sie unter möchten müssen Sie ein TextArea-Tag hinzufügen, und geben Sie ihm eine Id von TraceConsole, wie im folgenden gezeigt:


[!code-html[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample7.html)]

Alle Sys.Debug.trace Anweisungen auf der Seite werden in der TraceConsole TextArea geschrieben werden.

In Fällen, in der Sie Daten, die in ein JSON-Objekt anzeigen möchten, können Sie der Klasse Sys.Debug traceDump-Funktion verwenden. Diese Funktion akzeptiert zwei Parameter, wie das Objekt, das an die Ablaufverfolgungskonsole-diagnosesicherungsmechanismus gesichert werden sollte und ein Name, der zur Identifizierung des Objekts in die Ausgabe der Ablaufverfolgung verwendet werden kann. Codebeispiel 6 zeigt ein Beispiel der Verwendung der traceDump-Funktion.

**Codebeispiel 6. Mithilfe der Sys.Debug.traceDump-Funktion.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample8.js)]

Abbildung 11 zeigt die Ausgabe von Aufrufen der Sys.Debug.traceDump-Funktion. Beachten Sie, dass zusätzlich zum Schreiben von Daten für das Person-Objekt auch das Sub-Adressobjekt die Daten geschrieben.

Zusätzlich zur Ablaufverfolgung werden Sys.Debug-Klasse auch zum Ausführen von Code Assertionen verwendet. Assertionen werden verwendet, zu testen, ob bestimmte Bedingungen erfüllt sind, während eine Anwendung ausgeführt wird. Die Debugversion der ASP.NET AJAX-Bibliothek-Skripts enthalten mehrere assert-Anweisungen, um eine Vielzahl von Bedingungen zu testen.

Codebeispiel 7 zeigt ein Beispiel der Verwendung der Sys.Debug.assert-Funktion zum Testen einer Bedingung. Der Code überprüft, ob das Adressobjekt null ist, bevor Sie ein Person-Objekt zu aktualisieren.


[![Die Ausgabe der Funktion Sys.Debug.traceDump.](understanding-asp-net-ajax-debugging-capabilities/_static/image32.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image31.png)

**Abbildung 11**: Ausgabe der Funktion Sys.Debug.traceDump.  ([Klicken Sie, um das Bild in voller Größe anzeigen](understanding-asp-net-ajax-debugging-capabilities/_static/image33.png))


**Codebeispiel 7 an. Mithilfe der debug.assert-Funktion.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample9.js)]

Drei Parameter übergeben werden, einschließlich die auszuwertende Bedingung, die Meldung angezeigt, wenn die Assertion gibt "false", und davon, ob die Informationen über den Aufrufer angezeigt werden soll. In Fällen, in denen eine Assertion fehlschlägt, wird die Meldung sowie Aufruferinformationen angezeigt werden, wenn der dritte Parameter auf "true" festgelegt wurde. Abbildung 12 zeigt ein Beispiel für das Dialogfeld "Fehler", das angezeigt wird, schlägt die Assertion, die in Codebeispiel 7 gezeigt.

Die letzte Funktion abgedeckt ist Sys.Debug.fail. Wenn Sie Code in einer bestimmten Zeile in einem Skript nicht erzwingen möchten, können Sie die Debuggeranweisung, die normalerweise in JavaScript-Anwendungen verwendet werden, statt einen Aufruf Sys.Debug.fail hinzufügen. Die Sys.Debug.fail-Funktion akzeptiert einen einzelnen Zeichenfolgenparameter, der die Ursache des Fehlers darstellt, wie im folgenden gezeigt:


[!code-css[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample10.css)]


[![Eine Sys.Debug.assert-Fehlermeldung.](understanding-asp-net-ajax-debugging-capabilities/_static/image35.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image34.png)

**Abbildung 12**: ein Sys.Debug.assert-Fehlermeldung.  ([Klicken Sie, um das Bild in voller Größe anzeigen](understanding-asp-net-ajax-debugging-capabilities/_static/image36.png))


Wenn eine Anweisung Sys.Debug.fail gefunden wird, während ein Skript ausgeführt wird, der Wert des Nachrichtenparameters wird in der eine Anwendung debuggen, z. B. Visual Studio 2008-Konsole angezeigt werden und werden Sie aufgefordert, die die Anwendung zu debuggen. Ein Fall, in denen dies sehr nützlich sein kann, ist, wenn Sie einen Haltepunkt in Visual Studio 2008 können für ein Inline-Skript nicht festgelegt, aber möchten den Code auf bestimmten Zeile zu beenden, damit Sie den Wert der Variablen überprüfen können.

*Grundlegendes zur ScriptMode-Eigenschaft des ScriptManager-Steuerelements*

Die ASP.NET AJAX-Bibliothek enthält debug und release Skriptversionen, die standardmäßig unter c:\Programme\Microsoft c:\Programme\Microsoft ASP.NET\ASP.NET 2.0 AJAX Extensions\v1.0.61025\MicrosoftAjaxLibrary\System.Web.Extensions\1.0.61025.0 installiert werden. Die Debug-Skripts sind ordentlich formatierte, leicht zu lesen und haben mehrere Sys.Debug.assert Aufrufe verteilt diese während Versionsskripts Leerzeichen entfernt haben, und verwenden die Sys.Debug-Klasse nur selten, um die Gesamtgröße der zu minimieren.

Das ScriptManager-Steuerelement, das ASP.NET AJAX-Seiten hinzugefügt, liest das Compilation-Element-Debug-Attribut in der Datei "Web.config", um zu bestimmen, welche Versionen von Bibliothekskripts zu laden. Sie können jedoch steuern, wenn Debug- oder Versionsskripts sind geladen (Bibliothekskripts oder Ihre eigenen benutzerdefinierten Skripts) durch Ändern der ScriptMode-Eigenschaft. ScriptMode akzeptiert eine ScriptMode-Enumeration, deren Mitglieder, automatisch "," Debug "," Release "und" erben enthalten.

ScriptMode standardmäßig den Wert von "Auto" Was bedeutet, dass ScriptManager das Debug-Attribut in der Datei "Web.config" überprüft. Beim Debuggen auf "false" festgelegt ist, lädt ScriptManager die Releaseversion von ASP.NET AJAX-Bibliothekskripts. Beim Debuggen auf "true" festgelegt ist, wird die Debugversion der Skripts geladen werden. Ändern die Eigenschaft ScriptMode zum Freigeben oder zum Debuggen, erzwingt den ScriptManager die entsprechenden Skripts unabhängig davon, welcher Wert zu laden, die das Debug-Attribut in der Datei "Web.config" verfügt. Codebeispiel 8 zeigt ein Beispiel für das ScriptManager-Steuerelement zum Debug-Skripts aus der ASP.NET AJAX-Bibliothek laden.

**Codebeispiel 8. Laden mithilfe von ScriptManager Debugskripts**.


[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample11.aspx)]

Sie können auch verschiedene Versionen von Ihren eigenen benutzerdefinierten Skripts (Debug oder Release) laden, mithilfe ScriptManagers-Scripts-Eigenschaft zusammen mit der ScriptReference-Komponente wie im Codebeispiel 9 gezeigt.

**Codebeispiel 9. Laden benutzerdefinierte Skripts, die mithilfe von ScriptManager.**


[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample12.aspx)]

> [!NOTE]
> Wenn Sie benutzerdefinierte Skripts, die mit der ScriptReference-Komponente laden, müssen Sie ScriptManager benachrichtigen, wenn das Skript geladen wurde, indem Sie den folgenden Code am Ende des Skripts hinzufügen:


[!code-csharp[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample13.cs)]

Der Code in Codebeispiel 9 weist im ScriptManager auf die Suche nach eine Debugversion des Person-Skripts, damit sie automatisch für Person.debug.js statt Person.js aussehen wird. Wenn die Person.debug.js-Datei nicht gefunden wird, dass ein Fehler ausgelöst wird.

In Fällen, in dem Sie einen Debugbuild oder Releaseversion von einem benutzerdefinierten Skript geladen werden basierend auf dem Wert der Eigenschaft ScriptMode für das ScriptManager-Steuerelement festgelegt, wird können Sie zum erben ScriptMode-Eigenschaft des ScriptReference-Steuerelements festlegen. Dies bewirkt die richtige Version des benutzerdefinierten Skripts geladen werden, basierend auf das ScriptModes ScriptMode Eigenschaft wie in Codebeispiel 10 dargestellt. Da die Eigenschaft ScriptMode des ScriptManager-Steuerelements auf Debuggen festgelegt ist, wird das Skript Person.debug.js geladen und auf der Seite verwendet.

**Codebeispiel 10. Erben das ScriptMode von ScriptManager für benutzerdefinierte Skripts an.**


[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample14.aspx)]

Mithilfe der Eigenschaft ScriptMode entsprechend können einfacher Debuggen von Anwendungen und den gesamten Prozess zu vereinfachen. Die ASP.NET AJAX-Bibliothek Versionsskripts sind schwer zu durchlaufen und lesbar, da der Code-Formatierung entfernt wurde, während die Debugskripts formatiert sind, insbesondere zu Debugzwecken.

## <a name="conclusion"></a>Schlussbemerkung

Die Technologie von Microsoft ASP.NET AJAX bietet eine solide Grundlage für das Erstellen von AJAX-fähigen Anwendungen, die allgemeine Erfahrung des Endbenutzers verbessern können. Allerdings werden als mit jeder Programmiersprache-Technologie, Fehler und andere Anwendungsprobleme sicherlich auftreten. Zu wissen, über die verschiedenen verfügbaren debugging-Optionen kann viel Zeit und das Ergebnis in einem Produkt stabiler speichern.

In diesem Artikel haben Sie mehrere verschiedene Verfahren zum Debuggen von ASP.NET AJAX-Seiten, einschließlich Internet Explorer mit Visual Studio 2008, Web Development Helper und Firebug eingeführt wurde. Diese Tools können den Gesamtprozess Debuggen vereinfachen, da können Sie Variable Daten zugreifen, Code zeilenweise durchlaufen und Anzeigen von Trace-Anweisungen. Zusätzlich zu den verschiedenen debugging-Tools erläutert haben Sie auch, wie der ASP.NET AJAX-Bibliothek Sys.Debug-Klasse in einer Anwendung verwendet werden kann und wie die ScriptManager-Klasse zum Laden verwendet werden kann, debug oder release Debugversionen der Skripts.

## <a name="bio"></a>Biografie

Dan Wahlin (Microsoft Most Valuable Professional für ASP.NET und XML Web Services) ist eine Entwicklung "Instructor" und Architektur .NET-Berater auf technische Schulungen-Schnittstelle ([www.interfacett.com)](http://www.interfacett.com). Dan gegründet wurde, den XML-Code für ASP.NET Entwickler-Website ([www.XMLforASP.NET](http://www.XMLforASP.NET)), befindet sich auf die INETA-Sprecher Bureau und hält Vorträge auf Konferenzen. Dan Co-Autor Professional Windows DNA (Wrox), ASP.NET: Tipps, Tutorials und Code (Sams), ASP.NET 1.1-Insider-Lösungen, Professional ASP.NET 2.0 AJAX (Wrox), ASP.NET 2.0-MVP-Hacks und erstellte XML für ASP.NET-Entwickler (Sams). Wenn er nicht Code, Artikel oder Bücher schreiben wird, profitiert Dan schreiben und Musik Aufzeichnung und Wiedergabe Golf und Basketball mit seiner Frau und Kinder an.

Scott Cate arbeitet mit Microsoft-Web-Technologien seit 1997 und ist Vorsitzender der myKB.com ([www.myKB.com](http://www.myKB.com)), in dem er spezialisiert sich auf das Schreiben von ASP.NET basierende Anwendungen, die mit dem Schwerpunkt Knowledge Base-softwarelösungen. Scott hergestellt werden kann, per e-Mail unter [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) oder seinen Blog unter [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Vorherige](understanding-asp-net-ajax-web-services.md)
