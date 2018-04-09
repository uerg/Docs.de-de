---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-debugging-capabilities
title: Grundlegendes zu ASP.NET-AJAX Debugfunktionen | Microsoft Docs
author: scottcate
description: So debuggen Sie Code ist eine Fähigkeit, die jeder Entwickler in ihren Arsenal unabhängig von der Technologie enthalten sein sollen, das sie verwenden. Während viele Entwickler sind...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/28/2008
ms.topic: article
ms.assetid: 7f9380c6-19f7-4c82-a019-916ec6dffc9c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-debugging-capabilities
msc.type: authoredcontent
ms.openlocfilehash: f082e2206f5e691579670e42634f30b57e3b3593
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="understanding-aspnet-ajax-debugging-capabilities"></a>Grundlegendes zu ASP.NET-AJAX Debugfunktionen
====================
durch [Scott Kate](https://github.com/scottcate)

[PDF herunterladen](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial06_Debugging_MS_Ajax_Applications_cs.pdf)

> So debuggen Sie Code ist eine Fähigkeit, die jeder Entwickler in ihren Arsenal unabhängig von der Technologie enthalten sein sollen, das sie verwenden. Während viele Entwickler daran gewöhnt sind, verwenden Visual Studio .NET oder Web Developer Express zum Debuggen von ASP.NET-Anwendungen, die VB.NET oder C#-Code verwenden, sind einige nicht beachten Sie, dass sie äußerst nützlich für das Debuggen von clientseitigem Code wie z. B. JavaScript entspricht. Die gleiche Art von Techniken zum Debuggen von Anwendungen für .NET kann auch für AJAX-aktivierten Anwendungen und genauer gesagt ASP.NET AJAX-Anwendungen angewendet werden.


## <a name="debugging-aspnet-ajax-applications"></a>Debuggen von ASP.NET AJAX-Anwendungen

Dan Wahlin

So debuggen Sie Code ist eine Fähigkeit, die jeder Entwickler in ihren Arsenal unabhängig von der Technologie enthalten sein sollen, das sie verwenden. Selbstverständlich Grundlegendes zu den verschiedenen Optionen für das Debuggen, die verfügbar sind eine enorme Zeit auf ein Projekt und vielleicht sogar einige Headaches speichern kann. Während viele Entwickler daran gewöhnt sind, verwenden Visual Studio .NET oder Web Developer Express zum Debuggen von ASP.NET-Anwendungen, die VB.NET oder C#-Code verwenden, sind einige nicht beachten Sie, dass sie äußerst nützlich für das Debuggen von clientseitigem Code wie z. B. JavaScript entspricht. Die gleiche Art von Techniken zum Debuggen von Anwendungen für .NET kann auch für AJAX-aktivierten Anwendungen und genauer gesagt ASP.NET AJAX-Anwendungen angewendet werden.

In diesem Artikel sehen Sie, wie Visual Studio 2008 und mehrere andere Tools zum Debuggen von ASP.NET AJAX-Anwendungen auf Fehler und andere Probleme schnell verwendet werden können. In dieser Diskussion werden Informationen zum Aktivieren von InternetExplorer 6 oder höher für das Debuggen, mithilfe von Visual Studio 2008 und Skript-Explorer Code schrittweise durchlaufen sowie für die Verwendung anderer kostenlose Tools wie z. B. Web Development Helper enthalten. Außerdem erfahren, wie das Debuggen von ASP.NET AJAX-Anwendungen in Firefox mithilfe eine Erweiterung Firebug verwenden und Sie können mit dem Namen Sie JavaScript-Code direkt im Browser ohne beliebige andere Tools durchlaufen schrittweise. Sie müssen schließlich für Klassen in der ASP.NET AJAX-Bibliothek eingeführt werden, die mit verschiedenen Debugaufgaben wie Protokollierung und assertionsanweisungen Code helfen.

Bevor Sie versuchen, das Debuggen in Internet Explorer angezeigten Seiten stehen einige grundlegende Schritte, die Sie ausführen, um sie für das Debuggen zu aktivieren müssen. Werfen wir einen Blick auf einige einfache Einrichtung-Anforderungen, die müssen ausgeführt werden, um zu beginnen.

## <a name="configuring-internet-explorer-for-debugging"></a>Konfigurieren von InternetExplorer für das Debuggen

Die meisten Personen nicht von Interesse sind, sehen Sie die JavaScript-Problemen, die auf einer Website mit Internet Explorer angezeigt. Tatsächlich würde nicht die durchschnittliche Benutzer auch wissen, was zu tun, wenn sie eine Fehlermeldung angezeigt wurde. Daher werden Optionen für das Debuggen im Browser standardmäßig deaktiviert. Allerdings ist es sehr einfach, aktivieren Sie das Debuggen, und fügen Sie sie verwenden, wie Sie neue AJAX-Anwendungen zu entwickeln.

Aktivieren von Debugfunktionen, wechseln in den Internetoptionen Internet Explorer im Menü Extras, und wählen Sie die Registerkarte "Erweitert". Stellen Sie sicher, dass die folgenden Elemente deaktiviert sind, in dem Abschnitt zum Durchsuchen:

- Deaktivieren Sie Skriptdebugging (Internet Explorer)
- Deaktivieren Sie Skriptdebugging (andere)

Obwohl nicht erforderlich, wenn versuchen Sie zum Debuggen einer Anwendung sollten Sie wahrscheinlich eine JavaScript-Fehler auf der Seite sofort sichtbar und unverkennbar sein. Sie können erzwingen, dass alle Fehler in einem Meldungsfeld angezeigt werden, durch Aktivieren des Kontrollkästchens "Anzeige eine Benachrichtigung zu jedem Skriptfehler". Dies ist, zwar eine sinnvolle Option zu aktivieren, während Sie eine Anwendung entwickeln können sie schnell werden ärgerlich, wenn Sie andere Websites nur beispieldatasets sind, da die Wahrscheinlichkeit zu senken JavaScript-Fehler ziemlich guten sind.

Abbildung 1 zeigt, welche das Dialogfeld Erweiterte Internet Explorer sollte aussehen, nachdem es für das Debuggen ordnungsgemäß konfiguriert wurde.


[![Konfigurieren von Internet Explorer für das Debuggen.](understanding-asp-net-ajax-debugging-capabilities/_static/image2.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image1.png)

**Abbildung 1**: Konfigurieren von Internet Explorer für das Debuggen.  ([Klicken Sie hier, um das Bild in voller Größe angezeigt](understanding-asp-net-ajax-debugging-capabilities/_static/image3.png))


Sobald das Debuggen aktiviert wurde, sehen Sie ein neues Menüelement im Menü "Ansicht" mit dem Namen Skriptdebugger angezeigt werden. Es gibt zwei Optionen zur Verfügung, einschließlich öffnen und Seitenumbruch, an die nächste Anweisung. Beim Öffnen aktiviert ist, werden Sie aufgefordert werden so Debuggen Sie die Seite in Visual Studio 2008 (Beachten Sie, dass Visual Web Developer Express für das Debuggen auch verwendet werden kann). Wenn Visual Studio .NET zurzeit ausgeführt wird, können Sie diese Instanz verwenden oder Erstellen einer neuen Instanz auswählen. Bei aktiviertem Unterbrechung am nächsten Anweisung werden Sie aufgefordert, auf die Seite zu debuggen, wenn JavaScript-Code ausgeführt wird. Wenn JavaScript-Code in das OnLoad-Ereignis der Seite ausgeführt wird, können Sie die Seite, um das Auslösen einer Debugsitzung aktualisieren. Wenn JavaScript-Code ausgeführt wird, nach dem Klicken auf eine Schaltfläche wird der Debugger sofort nach dem Klicken auf die Schaltfläche ausgeführt.

> *> [!NOTE] Wenn Sie die unter Windows Vista mit der Benutzerkontensteuerung (UAC) aktiviert ausgeführt werden, und Sie haben Visual Studio 2008 als Administrator ausführen, kann Visual Studio nicht an den Prozess anfügen, wenn Sie aufgefordert werden, für die Verbindung. Um dieses Problem zu umgehen, starten Sie Visual Studio zunächst, und verwenden Sie diese Instanz zum Debuggen.*


Obwohl im nächste Abschnitt So debuggen Sie eine ASP.NET AJAX-Seite in Visual Studio 2008 direkt aus veranschaulicht wird, ist mit der Option Script-Debugger von Internet Explorer nützlich, wenn eine Seite bereits geöffnet ist und Sie möchte ausführlicher untersuchen sie.

## <a name="debugging-with-visual-studio-2008"></a>Debuggen mit Visual Studio 2008

Visual Studio 2008 bietet Debugfunktionen, die Entwickler auf der ganzen Welt auf täglich um Debug .NET Applications basieren. Der integrierte Debugger können Sie Code, Anzeigen von Objektdaten, Überwachung für bestimmte Variablen, die Aufrufliste und vieles mehr überwachen schrittweise durchlaufen. Zusätzlich zum Debuggen von VB.NET oder C#-Code, der Debugger ist auch nützlich für das Debuggen von ASP.NET AJAX-Anwendungen und lässt Sie JavaScript-Code zeilenweise durchlaufen. Die Details, die Fokus von Verfahren, die verwendet werden kann folgen, so Debuggen Sie die clientseitige Skriptdateien, anstatt eine Discourse zum Gesamtprozess der Debuggen mit Visual Studio 2008-Anwendungen bereitstellen.

Eine Seite in Visual Studio 2008 debuggt kann auf verschiedene Weise gestartet werden. Zunächst können Sie die im vorherigen Abschnitt erwähnten Script-Debugger von Internet Explorer-Option verwenden. Dies funktioniert gut, wenn eine Seite im Browser bereits geladen ist und Sie es zu Debuggen starten möchten. Alternativ können Sie mit der rechten Maustaste auf eine ASPX-Seite im Projektmappen-Explorer, und wählen aus dem Menü als Startseite festlegen. Wenn Sie daran gewöhnt sind, Debuggen von ASP.NET-Seiten haben dann wahrscheinlich vor dem einschließen. Sobald F5 gedrückt wird, kann die Seite gedebuggt werden. Allerdings während Sie in der Regel einen Haltepunkt an einer beliebigen Stelle festlegen können möchte Sie im VB.NET oder C#-Code, die nicht immer bei JavaScript der Fall ist, da Sie als Nächstes sehen.

*Eingebettete und externe Skripts*

Der Visual Studio 2008-Debugger behandelt JavaScript in eine externe JavaScript-Dateien unterscheidet Seite eingebettet. Sie können mit externen Skriptdateien öffnen Sie die Datei und legen Sie einen Haltepunkt für jede beliebige Zeile, die Sie auswählen. Haltepunkte können festgelegt werden, indem Sie in den grauen Bereich links neben dem Code-Editor-Fenster auf. Wenn JavaScript eingebettet wird, direkt in eine Seite mit den `<script>` Tag, einen Haltepunkt festlegen, indem Sie in den grauen Bereich auf keine Option. Versuche zum Festlegen eines Haltepunkts in einer Zeile des eingebetteten Skripts führt zu einer Warnung, aus der hervorgeht, "This keinem gültigen Speicherort für einen Breakpoint is".

Sie können dieses Problem abrufen, indem Verschieben des Codes in eine externe JS-Datei, und verweisen auf das Src-Attribut mit der &lt;Skript&gt; Tag:


[!code-html[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample1.html)]

Was geschieht, wenn das Verschieben des Codes in einer externen Datei oder eine Option ist nicht mehr Arbeit als erforderlich ist Folgendes zu? Während Sie einen Haltepunkt mit dem Editor festlegen können, können Sie die Debuggeranweisung direkt in den Code hinzufügen, in dem Sie debuggen möchten. Sie können auch Sys.Debug-Klasse in der ASP.NET AJAX-Bibliothek verwenden, zum Starten des Debuggens erzwingen. Erfahren Sie mehr über die Klasse Sys.Debug weiter unten in diesem Artikel.

Ein Beispiel zur Verwendung der `debugger` -Schlüsselwort wird in Codebeispiel 1 angezeigt. Dieses Beispiel zwingt den Debugger an den rechten unterbrochen, bevor Sie einen Aufruf an eine Update-Funktion ausgeführt wird.

**Programmbeispiel 1. Verwenden die Debugger-Schlüsselwort, um Debugger von Visual Studio .NET unterbrechen zu gewährleisten.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample2.js)]

Nachdem die Debuggeranweisung erreicht wird, die Sie werden aufgefordert, die Seite mithilfe von Visual Studio .NET debug und können beginnen, den Code schrittweise durchlaufen. Während auf diese Weise Sie diese ein Problem mit den Zugriff auf ASP.NET AJAX-Bibliothek-Skriptdateien, die wir also auf der Seite verwendeten auftreten, sehen Sie sich mit Visual Studio. NET Skript-Explorer.

## <a name="using-visual-studio-net-windows-to-debug"></a>Verwenden zum Debuggen von Fenstern in Visual Studio .NET

Sobald eine Debugsitzung gestartet wird und Sie beginnen mit dem F11 Standardschlüssel Code durchlaufen, treten ggf. gezeigt im Fehlerdialogfeld finden Sie unter Abbildung 2, es sei denn, alle Skriptdateien, die auf der Seite verwendeten öffnen und zum Debuggen verfügbar sind.


[![Fehlerdialogfeld angezeigt, wenn kein Quellcode für das Debuggen verfügbar ist.](understanding-asp-net-ajax-debugging-capabilities/_static/image5.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image4.png)

**Abbildung 2**: Fehlerdialogfeld angezeigt, wenn kein Quellcode für das Debuggen verfügbar ist.  ([Klicken Sie hier, um das Bild in voller Größe angezeigt](understanding-asp-net-ajax-debugging-capabilities/_static/image6.png))


Dieses Dialogfeld wird angezeigt, da Visual Studio .NET nicht sicher wie auf den Quellcode einiger Skripts verwiesen wird, von der Seite abgerufen ist. Dies kann recht frustrierend sein zunächst eine einfache Lösung vorhanden ist. Sobald Sie eine Debugsitzung gestartet und einen Haltepunkt erreicht, wechseln Sie zum Debuggen von Windows-Skript-Explorer-Fenster der Visual Studio 2008-Menü, oder verwenden Sie den Hotkey Strg + Alt + N.

> *> [!NOTE] Wenn Sie das Skript-Explorer-Menü aufgeführten sehen können, wechseln Sie zu Extras* *anpassen* *Befehle in diesem Menü für Visual Studio .NET. Suchen Sie den Debug-Eintrag im Abschnitt Kategorien aus, und klicken Sie auf diese Option, um alle im Kontextmenü verfügbaren Einträge anzuzeigen. Klicken Sie in der Liste der Befehle Bildlauf nach unten zur Skript-Explorer, und ziehen Sie sie dann oben auf der Debug* *Windows-Menü im zuvor erwähnten. Dadurch wird den Eintrag der Skript-Explorer-Menü jedes Mal zur Verfügung, wenn Sie Visual Studio .NET ausführen.*


Skript-Explorer kann verwendet werden, um alle Skripts, die verwendet werden, auf einer Seite anzeigen und im Code-Editor zu öffnen. Sobald der Skript-Explorer geöffnet ist, doppelklicken Sie auf der ASPX-Seite, die gerade gedebuggt werden, um sie im Code-Editor-Fenster zu öffnen. Führen Sie die gleiche Aktion für alle anderen Skripts im Skript-Explorer. Sobald alle Skripts im Code-Fenster geöffnet sind Sie können drücken Sie F11 (und verwenden die Hotkeys in anderen Debug), um den Code schrittweise. Abbildung 3 zeigt ein Beispiel für die Skript-Explorer. Die aktuelle Datei gedebuggten (Demo.aspx) aufgeführt sowie zwei benutzerdefinierte Skripts und zwei Skripts, die dynamisch von ASP.NET AJAX ScriptManager in die Seite eingefügt.


[![Der Skript-Explorer bietet einfachen Zugriff auf Skripts, die auf einer Seite verwendet.](understanding-asp-net-ajax-debugging-capabilities/_static/image8.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image7.png)

**Abbildung 3**. Der Skript-Explorer bietet einfachen Zugriff auf Skripts, die auf einer Seite verwendet.  ([Klicken Sie hier, um das Bild in voller Größe angezeigt](understanding-asp-net-ajax-debugging-capabilities/_static/image9.png))


Einigen weiteren Bildschirmen Windows auch verwendet werden, können um nützliche Informationen liefern, wie Sie Code auf einer Seite durchlaufen schrittweise. Fenster "lokal" können Sie z. B. die Werte von verschiedenen Variablen verwendet, die auf der Seite das "Direktfenster" bestimmte Variablen oder Bedingungen ausgewertet, und zeigen Sie die Ausgabe angezeigt. Das Fenster "Ausgabe" können auch Trace-Anweisungen geschrieben, mit der Sys.Debug.trace-Funktion (die weiter unten in diesem Artikel behandelt werden) oder Internet Explorer Debug.writeln-Funktion anzeigen.

Wie Sie Code mit dem Debugger schrittweise durchlaufen können sich die Maus über der Variablen im Code den Wert an, dem ihnen zugewiesen wurden. Allerdings zeigen nicht der Skriptdebugger gelegentlich nichts an, wie sich die Maus über einen bestimmten JavaScript-Variable. Um den Wert anzuzeigen, markieren Sie die Anweisung oder eine Variable, die Sie im Code-Editor-Fenster angezeigt, und klicken Sie dann die Maus über diese möchten. Obwohl diese Technik nicht in jeder Situation funktioniert, werden oft Sie den Wert anzuzeigen, ohne in einem anderen Debugfenster wie z. B. das Fenster "lokal" suchen können.

Ein video-Lernprogramm zur Veranschaulichung einiger der hier beschriebenen Features angezeigt werden kann, am [ http://www.xmlforasp.net ](http://www.xmlforasp.net).

## <a name="debugging-with-web-development-helper"></a>Debuggen mit Web Development Helper

Obwohl Visual Studio 2008 (und Visual Web Developer Express 2008) sehr leistungsfähig debugging-Tools sind, stehen zusätzliche Optionen, die auch verwendet werden können die mehr ressourcenschonende sind. Zu den neuesten Tools freizugebenden ist der Web Development Helper. Microsofts Nikhil Kothari (eines der wichtigsten ASP.NET AJAX-Architekten bei Microsoft) hat dieses ausgezeichnete Tools, die viele verschiedene Tasks ausführen kann, von einfachen debugging bis hin zur HTTP-Anforderung und Antwort-Nachrichten anzeigen. Können Web Development Helper heruntergeladen werden [ http://projects.nikhilk.net/Projects/WebDevHelper.aspx ](http://projects.nikhilk.net/Projects/WebDevHelper.aspx).

Web Development Helper kann direkt in Internet Explorer verwendet werden, wodurch es praktisch, zu verwenden. Er wird gestartet, durch die Tools Web Development Helper im Internet Explorer-Menü auswählen. Dadurch wird das Tool im unteren Bereich des Browsers geöffnet nice also, da Sie nicht, um den Browser, um einige Aufgaben wie z. B. HTTP-Anforderung und Antwort nachrichtenprotokollierung ausführen zu lassen. Abbildung 4 zeigt, wie Web Development Helper in Aktion aussieht.


[![Web Development Helper](understanding-asp-net-ajax-debugging-capabilities/_static/image11.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image10.png)

**Abbildung 4**: Web Development Helper ([klicken Sie hier, um das Bild in voller Größe angezeigt](understanding-asp-net-ajax-debugging-capabilities/_static/image12.png))


Web Development Helper ist Sie verwenden dieses Tool nicht auf den Code zeilenweise, wie mit Visual Studio 2008 schrittweise durchlaufen. Allerdings kann zum Anzeigen von Ablaufverfolgungsausgabe, leicht Auswerten von Variablen in einem Skript oder Durchsuchen verwendet werden Daten in ein JSON-Objekt ist. Es ist auch sehr nützlich für die Anzeige von Daten, die an und von einer ASP.NET AJAX-Seite und einen Server übergeben werden.

Nachdem Web Development Helper in Internet Explorer geöffnet ist, muss das Skriptdebuggen aktiviert werden, indem Skriptdebugging aktivieren Skript aus dem Web Development Helper-Menü auswählen, wie weiter oben in Abbildung 4 dargestellt. Dies ermöglicht das Tool zum Abfangen von Fehlern, die auftreten, wenn eine Seite ausgeführt wird. Darüber hinaus können einfachen Zugriff auf die Ablaufverfolgungsmeldungen, die auf der Seite ausgegeben werden. Wählen Sie zum Anzeigen von Ablaufverfolgungsinformationen aus, oder führen Sie Befehle des Skripts So testen Sie verschiedene Funktionen innerhalb einer Seite, Skript Skript-Konsole anzeigen, aus dem Web Development Helper-Menü. Dies ermöglicht den Zugriff auf ein Befehlsfenster, und eine einfache "Direktfenster".

*Anzeigen von Ablaufverfolgungsmeldungen und JSON-Objekt-Daten*

Das "Direktfenster" dient, Skriptbefehle ausführen oder sogar zu laden oder Speichern von Skripts, die verwendet werden, um verschiedene Funktionen auf einer Seite zu testen. Im Befehlsfenster angezeigt, Trace oder Debug geschrieben, die von der Seite angezeigt wird. Auflisten von 2 zeigt, wie eine Ablaufverfolgungsmeldung mit Internet Explorer Debug.writeln-Funktion zu schreiben.

**Auflisten von 2. Schreiben eine clientseitige Ablaufverfolgungsmeldung mithilfe der Debug-Klasse.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample3.js)]

Wenn die LastName-Eigenschaft auf einen Wert von Doe enthält, zeigt Web Development Helper an "Personennamen: Doe" in der Skriptkonsole Befehlsfenster (vorausgesetzt, dass Debuggen aktiviert wurde). Web Development Helper Fügt ein Objekt der obersten Ebene DebugService ebenfalls in Seiten, die zum Schreiben von Ablaufverfolgungsinformationen oder Anzeigen des Inhalts von JSON-Objekten verwendet werden können. Auflisten von 3 zeigt ein Beispiel der Verwendung der DebugService Klasse Trace-Funktion.

**Auflisten von 3. Verwenden Web Development Helper DebugService-Klasse, um eine Ablaufverfolgungsmeldung zu schreiben.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample4.js)]

Ein nützliches Feature der DebugService-Klasse ist, dass er funktioniert, auch wenn das Debuggen aktiviert ist, in Internet Explorer vereinfacht die Ablaufverfolgungsdaten stets zugreifen, wenn Web Development Helper ausgeführt wird. Wenn das Tool zum Debuggen einer Seite verwendet wird, werden ablaufverfolgungsanweisungen seit der Aufruf von window.debugService "false" zurückgeben, wird ignoriert.

Die Klasse DebugService macht es möglich, JSON-Objektdaten mit dem Web Development Helper Inspektor-Fenster angezeigt werden. Auflisten von 4 wird ein einfaches JSON-Objekt, das mit Daten der Person erstellt. Nachdem das Objekt erstellt wurde, erfolgt ein Aufruf an die DebugService überprüfen Klasse Funktion, um das JSON-Objekt, das visuell überprüft werden können.

**Auflisten von 4. Verwenden die debugService.inspect-Funktion zum Anzeigen von Daten der JSON-Objekt.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample5.js)]

Die GetPerson()-Funktion aufrufen, auf der Seite oder über das "Direktfenster" führt im Dialogfenster Objektinspektor angezeigt wird, wie in Abbildung 5 dargestellt. Eigenschaften, die innerhalb des Objekts können dynamisch geändert werden, indem hervorzuheben, ändern den Wert im Textfeld Wert angezeigt, und klicken Sie dann auf den Link aktualisieren. Mithilfe der Objekt-Inspektor erleichtert das Anzeigen von Daten der JSON-Objekts und experimentieren Sie mit unterschiedliche Werten auf Eigenschaften anwenden.

*Remotedebuggen – Fehler*

Zusätzlich zum Zulassen von Ablaufverfolgungsdaten und JSON-Objekten, die angezeigt werden, kann Web Development Helper auch helfen, Debuggen von Fehlern auf einer Seite. Wenn ein Fehler aufgetreten ist, werden Sie aufgefordert, auf der nächsten Codezeile fortzufahren, oder das Skript debuggen (siehe Abbildung 6). Das Skriptfehler Dialogfeld Fenster zeigt die vollständige Aufrufliste sowie Zeilennummern, sodass Sie problemlos erkennen können, in dem Probleme innerhalb eines Skripts werden.


[![Verwenden von Objektinspektor-Fenster zum Anzeigen einer JSON-Objekts.](understanding-asp-net-ajax-debugging-capabilities/_static/image14.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image13.png)

**Abbildung 5**: mit dem Objektinspektor-Fenster zum Anzeigen einer JSON-Objekts.  ([Klicken Sie hier, um das Bild in voller Größe angezeigt](understanding-asp-net-ajax-debugging-capabilities/_static/image15.png))


Durch Auswählen der Debugoption ermöglicht das auszuführende skriptanweisungen direkt in Web Development Helper "Direktfenster", zeigen Sie den Wert der Variablen, Schreiben von JSON-Objekten sowie weitere. Wenn die gleiche Aktion, die den Fehler ausgelöst wird erneut ausgeführt, und Visual Studio 2008 auf dem Computer verfügbar ist, werden Sie aufgefordert, eine Debugsitzung starten, damit Sie den Code zeilenweise, wie im vorherigen Abschnitt erläuterten durchlaufen können.


[![Web-Development-Helper-Skript Fehlerdialogfeld](understanding-asp-net-ajax-debugging-capabilities/_static/image17.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image16.png)

**Abbildung 6**: Fehlerdialogfeld für Web Development Helper-Skript ([klicken Sie hier, um das Bild in voller Größe angezeigt](understanding-asp-net-ajax-debugging-capabilities/_static/image18.png))


*Überprüfen von Anforderungs- und Antwortnachrichten verwendet werden*

Beim Debuggen von ASP.NET AJAX-Seiten ist es oft nützlich, um die Anforderung und Antwort-Nachrichten, die zwischen einer Seite und den Server gesendeten finden Sie unter. Anzeigen des Inhalts in Nachrichten ermöglicht Ihnen, festzustellen, ob die richtigen Daten sowie die Größe der Nachrichten übergeben werden. Web Development Helper bietet eine hervorragende HTTP-Nachricht Logger-Funktion, die zum Anzeigen von Daten als unformatierten Text oder in einem besser lesbaren Format erleichtert.

Um ASP.NET AJAX-Anforderung und Antwort-Nachrichten anzeigen möchten, muss die HTTP-Protokollierung aktiviert werden, indem HTTP-HTTP-Protokollierung im Web Development Helper Menü auswählen. Nach der Aktivierung können alle Nachrichten aus der aktuellen Seite im HTTP-Protokoll-Viewer angezeigt werden die durch Auswahl von HTTP-anzeigen HTTP-Protokolle zugegriffen werden kann.

Zwar sicherlich nützlich Anzeigen der unformatierten Texts, der in jeder Anforderung/Antwort-Nachricht gesendet ist (und eine Option im Web Development Helper), ist es oft einfacher, Nachrichtendaten in einem Grafikformat mehr anzuzeigen. Nachdem die HTTP-Protokollierung aktiviert wurde, und Meldungen protokolliert haben, können Nachrichtendaten durch Doppelklicken auf die Nachricht in der HTTP-Protokoll-Viewer angezeigt werden. Auf diese Weise können Sie alle zugeordneten eine Nachricht als auch die eigentliche Nachricht Header anzeigen Inhalt. Abbildung 7 zeigt ein Beispiel für eine Anforderungsnachricht und die Antwortnachricht, die im HTTP-Protokoll-Viewer-Fenster angezeigt.


[![Der Protokollanzeige HTTP-Anforderung und Antwort Nachrichtendaten anzeigen.](understanding-asp-net-ajax-debugging-capabilities/_static/image20.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image19.png)

**Abbildung 7**: mit dem HTTP-Protokoll-Viewer zum Anzeigen von Daten von Anforderung und Antwort Nachricht.  ([Klicken Sie hier, um das Bild in voller Größe angezeigt](understanding-asp-net-ajax-debugging-capabilities/_static/image21.png))


Die HTTP-Protokoll-Viewer wird automatisch analysiert JSON-Objekten und mit einer Strukturansicht somit schnell und einfach die Objektdaten Eigenschaft anzeigen angezeigt. UpdatePanel in einer ASP.NET AJAX-Seite verwendet wird, wird der Viewer, jeden Teil der Nachricht in einzelne Teile unterbrochen, wie in Abbildung 8 gezeigt. Dies ist eine großartige Funktion, die kann einfacher zu erkennen und zu verstehen, was die Nachricht im Vergleich zu Anzeigen der unformatierte Nachrichtendaten enthält.


[![Eine UpdatePanel-Antwortnachricht mit dem HTTP-Protokoll-Viewer angezeigt werden soll.](understanding-asp-net-ajax-debugging-capabilities/_static/image23.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image22.png)

**Abbildung 8**: ein UpdatePanel Antwortnachricht mithilfe von HTTP-Protokoll-Viewer angezeigt.  ([Klicken Sie hier, um das Bild in voller Größe angezeigt](understanding-asp-net-ajax-debugging-capabilities/_static/image24.png))


Es gibt mehrere Tools, die zum Anzeigen von Anforderung und Antwort-Nachrichten zusätzlich zu Web Development Helper verwendet werden können. Eine weitere gute Möglichkeit besteht Fiddler kostenlos unter [ http://www.fiddlertool.com ](http://www.fiddlertool.com). Obwohl Fiddler hier nicht erläutert werden, ist es auch eine gute Wahl, wenn Sie gründlich überprüfen von Nachrichten-Header und Daten müssen.

## <a name="debugging-with-firefox-and-firebug"></a>Debuggen mit Firefox und Firebug

Während Internet Explorer immer noch die am häufigsten verwendeten Browser ist, wird anderen Browsern, z. B. Firefox sehr beliebte geworden, und mehr verwendet werden. Daher sollten Sie zum Anzeigen und Debuggen der ASP.NET AJAX-Seiten in Firefox als auch Internet Explorer stellen Sie sicher, dass die Anwendung ordnungsgemäß funktioniert. Firefox direkt in Visual Studio 2008 tie nicht möglich, für das Debuggen, hat aber eine Erweiterung mit dem Namen Firebug, die zum Debuggen von Seiten verwendet werden kann. Firebug kann kostenlos heruntergeladen werden, navigieren Sie zu [ http://www.getfirebug.com ](http://www.getfirebug.com).

Firebug bietet eine voll ausgestattete debugging-Umgebung, mit denen Code zeilenweise durchlaufen, Zugriff auf alle Skripts, die innerhalb einer Seite verwendet werden, Anzeigen von DOM-Strukturen, CSS-Formatvorlagen und sogar überwachen Ereignisse, die auftreten, auf einer Seite angezeigt werden kann. Nach Abschluss der Installation kann Firebug Tools Firebug öffnen Firebug im Menü Firefox auswählen zugegriffen werden. Wie Web Development-Helper ist Firebug direkt im Browser verwendet, obwohl er auch als eigenständige Anwendung verwendet werden kann.

Sobald Firebug ausgeführt wird, können Haltepunkte auf eine beliebige Zeile einer JavaScript-Datei festgelegt werden, ob das Skript auf einer Seite eingebettet ist. Um einen Haltepunkt festzulegen, laden Sie zuerst die entsprechende Seite in Firefox debuggen möchten. Sobald die Seite geladen ist, wählen Sie das Skript aus der Dropdown-Liste von Firebug des Skripts zu debuggen. Alle Skripts, die verwendet werden, von der Seite werden angezeigt. Ein Haltepunkt ist festgelegt, durch Klicken auf die Firebug des grauen Bereich in der Zeile, in denen der Haltepunkt muss ebenso wie in Visual Studio 2008 wechseln sollte.

Sobald ein Haltepunkt im Firebug festgelegt wurde, können Sie die Aktion erforderlich, um das Skript auszuführen, das debuggt werden, z. B. auf eine Schaltfläche oder aktualisieren den Browser, um das OnLoad-Ereignis auslösen muss ausführen. Ausführung unterbrochen automatisch auf die Zeile mit dem Haltepunkt. Abbildung 9 zeigt ein Beispiel für einen Haltepunkt aus, der ausgelöst wurde, ist in Firebug verwenden.


[![Behandlung von Haltepunkten in Firebug verwenden.](understanding-asp-net-ajax-debugging-capabilities/_static/image26.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image25.png)

**Abbildung 9**: Behandlung von Haltepunkten in Firebug verwenden.  ([Klicken Sie hier, um das Bild in voller Größe angezeigt](understanding-asp-net-ajax-debugging-capabilities/_static/image27.png))


Sobald ein Haltepunkt erreicht wird können Einzelschritt, Prozedurschritt oder bis zum Rücksprung Code mit den Pfeilschaltflächen. Wie Sie Code durchlaufen schrittweise, werden Skriptvariablen angezeigt, in der rechte Bereich des Debuggers können Sie Werte und Drilldown in Objekte. Firebug enthält auch eine Aufrufliste Dropdown-Liste, um das Skript Ausführungsschritte Folgen anzuzeigen, die zur aktuellen Zeile angezeigt, der debuggt wird geführt haben.

Firebug enthält auch ein Konsolenfenster, das zum Testen verschiedene skriptanweisungen, Variablen auswerten und Trace-Ausgabe anzeigen verwendet werden kann. Es wird durch Klicken auf die Registerkarte "Console", am oberen Rand des Fensters Firebug zugegriffen. Die zu debuggende Seite kann auch "überprüft werden" um DOM-Struktur und Inhalt anzuzeigen, indem Sie auf der Registerkarte "prüfen". Wie werden die Maus über die verschiedenen DOM-Elemente, die im Inspektor-Fenster angezeigt, die entsprechenden Teil der Seite hervorgehoben vereinfacht von anzuzeigen, wo das Element auf der Seite verwendet wird. Attributwerte, die mit einem bestimmten Element verknüpft sind, können "live" Experimentieren mit verschiedenen Breiten, Stile usw. auf ein Element anwenden geändert werden. Dies ist ein nützliches Feature, das Sie erspart von ständig Wechseln zwischen dem Quellcode-Editor und den Firefox-Browser, wie einfache Änderungen Auswirkungen einer Seite anzeigen.

Abbildung 10 zeigt ein Beispiel der Verwendung des DOM-Inspektors um ein Textfeld mit dem Namen TxtCountry auf der Seite zu suchen. Der Inspektor Firebug kann auch verwendet werden, an der CSS-Formatvorlagen, die in einer Seite sowie Ereignisse, die auftreten, z. B. Verfolgen von mausbewegungen, Schaltflächenklicks sowie weitere verwendet.


[![Mithilfe des Firebug DOM Inspektor an.](understanding-asp-net-ajax-debugging-capabilities/_static/image29.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image28.png)

**Abbildung 10**: Verwenden von Firebug DOM Inspektor.  ([Klicken Sie hier, um das Bild in voller Größe angezeigt](understanding-asp-net-ajax-debugging-capabilities/_static/image30.png))


Firebug bietet eine ressourcenschonende Weise schnell eine Seite direkt in Firefox als auch ein hervorragendes Tool zum Überprüfen von anderer Elementen auf der Seite Debuggen.

## <a name="debugging-support-in-aspnet-ajax"></a>Debugging-Unterstützung in ASP.NET-AJAX

Die ASP.NET AJAX-Bibliothek enthält viele verschiedene Objektklassen, die verwendet werden können, um den Vorgang des Hinzufügens von AJAX-Funktionen in einer Webseite zu vereinfachen. Verwenden Sie diese Klassen zum Suchen von Elementen innerhalb einer Seite und bearbeitet werden, neue Steuerelemente hinzufügen, Aufrufen von Webdiensten und sogar Ereignisse behandeln. Die ASP.NET AJAX-Bibliothek enthält auch Klassen, die verwendet werden können, um den Prozess der debugging-Seiten zu verbessern. In diesem Abschnitt wird eine Einführung in die Klasse Sys.Debug und finden Sie, wie es in Anwendungen verwendet werden kann.

*Verwenden der Sys.Debug-Klasse*

Die Sys.Debug-Klasse (eine JavaScript-Klasse befindet sich im Sys-Namespace) kann verwendet werden, einschließlich Ablaufverfolgungsausgabe schreiben, Ausführen von Code Assertionen und das Erzwingen eines zu einem Fehler, damit er debuggt werden kann unterschiedliche Funktionen ausführen. Es ist sehr häufig in der ASP.NET AJAX-Bibliothek Debugdateien (standardmäßig unter c:\Programme\Microsoft c:\Programme\Microsoft ASP.NET\ASP.NET 2.0 AJAX Extensions\v1.0.61025\MicrosoftAjaxLibrary\System.Web.Extensions\1.0.61025.0 installiert) verwendet zum Ausführen von bedingten Tests ( wird aufgerufen, Assertionen) sicherstellen, dass ordnungsgemäß Parameter übergeben werden, um Funktionen, Objekte, die erwarteten Daten enthalten und Trace-Anweisungen zu schreiben.

Sys.Debug-Klasse stellt mehrere unterschiedliche Funktionen, die zum Behandeln der Ablaufverfolgung, Code Assertionen oder Fehlern in Tabelle 1 dargestellt verwendet werden können.

**Tabelle 1. Funktionen der Sys.Debug-Klasse.**

| **Funktionsname** | **Beschreibung** |
| --- | --- |
| assert(condition, message, displayCaller) | Bestätigt, dass der Bedingungsparameter "true" ist. Wenn die getestete Bedingung auf "false" festgelegt ist, wird ein Meldungsfeld anzuzeigende Meldung der Wert des Parameters verwendet werden. Wenn der DisplayCaller-Parameter auf "true" festgelegt ist, zeigt die Methode auch Informationen zu den Aufrufer. |
| clearTrace() | Löscht die Ausgabe von Anweisungen aus möglich. |
| fail(message) | Bewirkt, dass das Programm beendet und der Debugger. Message-Parameter kann verwendet werden, um einen Grund für den Fehler bereitzustellen. |
| trace(message) | Message-Parameter in der Ablaufverfolgungsausgabe geschrieben. |
| traceDump(object, name) | Gibt die Daten eines Objekts in einem lesbaren Format an. Die Name-Parameter kann verwendet werden, um eine Bezeichnung für das Trace-Speicherabbild bereitzustellen. Alle untergeordneten Objekte innerhalb des Objekts wird als Dump ausgegeben werden standardmäßig geschrieben werden. |

Die clientseitige Protokollierung kann in großen und ganzen genauso wie der Ablaufverfolgung Funktionalität in ASP.NET verwendet werden. Es kann verschiedene Nachrichten problemlos angezeigt werden, ohne den Fluss der Anwendung zu unterbrechen. Auflisten von 5 zeigt ein Beispiel mit der Sys.Debug.trace-Funktion in das Ablaufverfolgungsprotokoll geschrieben. Diese Funktion nimmt einfach die Meldung, die als Parameter geschrieben werden soll.

**Auflisten von 5. Mithilfe der Sys.Debug.trace-Funktion.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample6.js)]

Wenn Sie den Code im Codebeispiel 5 ausführen wird Ablaufverfolgungsausgabe auf der Seite nicht angezeigt werden. Die einzige Möglichkeit zur Anzeige ist die Verwendung ein Konsolenfensters, die in Visual Studio .NET, Web Development Helper oder Firebug verfügbar. Wenn Sie die Ausgabe der Ablaufverfolgung auf der Seite anzeigen möchten müssen Sie ein TextArea-Tag hinzufügen, und geben Sie ihm eine TextArea-Id, wie nachfolgend dargestellt:


[!code-html[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample7.html)]

Sys.Debug.trace Anweisungen auf der Seite werden in der TraceConsole TextArea geschrieben werden.

In Fällen, in ein JSON-Objekt enthaltenen Daten angezeigt werden soll, können Sie die Sys.Debug Klasse traceDump-Funktion verwenden. Diese Funktion akzeptiert zwei Parameter einschließlich des Objekts, die an die Konsole Trace gesichert werden sollte und ein Name, der zur Identifizierung des Objekts in der Ablaufverfolgungsausgabe verwendet werden kann. Auflisten von 6 zeigt ein Beispiel der Verwendung der traceDump-Funktion.

**Auflisten von 6. Mithilfe der Sys.Debug.traceDump-Funktion.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample8.js)]

Abbildung 11 zeigt die Ausgabe von Aufrufen der Sys.Debug.traceDump-Funktion. Beachten Sie, dass zusätzlich zum Schreiben von Daten für das Person-Objekt, er auch die Adresse Sub-Object-Daten schreibt.

Zusätzlich zu den an Ablaufverfolgungsdaten einschließen, kann auch Sys.Debug-Klasse zum Ausführen von Code Assertionen verwendet werden. Assertionen werden verwendet, um zu testen, dass bestimmte Bedingungen erfüllt sind, während eine Anwendung ausgeführt wird. Die Debugversion von ASP.NET AJAX-Bibliothekskripts enthalten mehrere assert-Anweisungen, um eine Vielzahl von Bedingungen zu testen.

Auflisten von 7 zeigt ein Beispiel mit der Sys.Debug.assert-Funktion zum Testen einer Bedingung. Der Code wird getestet, ob die Adressobjekt null ist, vor dem Aktualisieren eines Person-Objekts.


[![Die Ausgabe der Funktion Sys.Debug.traceDump.](understanding-asp-net-ajax-debugging-capabilities/_static/image32.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image31.png)

**Abbildung 11**: Ausgabe der Sys.Debug.traceDump-Funktion.  ([Klicken Sie hier, um das Bild in voller Größe angezeigt](understanding-asp-net-ajax-debugging-capabilities/_static/image33.png))


**Auflisten von 7. Mithilfe der debug.assert-Funktion.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample9.js)]

Drei Parameter übergeben werden, einschließlich der auszuwertende Bedingung, die Meldung angezeigt, wenn die Assertion gibt "false", und davon, ob Informationen zu den Aufrufer angezeigt werden soll. In Fällen, in denen eine Assertion fehlschlägt, wird die Meldung sowie Aufruferinformationen angezeigt werden, wenn der dritte Parameter auf "true" festgelegt wurde. Abbildung 12 zeigt ein Beispiel für das Dialogfeld "Fehler", das angezeigt wird, schlägt die Assertion in auflisten 7 angezeigt.

Die letzte Funktion abdecken ist Sys.Debug.fail. Wenn Sie Code in einer bestimmten Zeile in einem Skript erzeugen erzwingen möchten können Sie einen Aufruf Sys.Debug.fail statt der Debuggeranweisung, die in JavaScript-Anwendungen in der Regel verwendet hinzufügen. Die Sys.Debug.fail-Funktion akzeptiert einen einzelnen Zeichenfolgenparameter, der die Ursache des Fehlers darstellt, wie nachfolgend dargestellt:


[!code-css[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample10.css)]


[![Eine Sys.Debug.assert-Fehlermeldung.](understanding-asp-net-ajax-debugging-capabilities/_static/image35.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image34.png)

**Abbildung 12**: ein Sys.Debug.assert-Fehlermeldung.  ([Klicken Sie hier, um das Bild in voller Größe angezeigt](understanding-asp-net-ajax-debugging-capabilities/_static/image36.png))


Bei eine Sys.Debug.fail-Anweisung erreicht wird, während ein Skript ausgeführt wird, der Wert des Parameters Nachricht wird in der eine Anwendung debuggen, z. B. Visual Studio 2008-Konsole angezeigt werden, und Sie werden aufgefordert, um die Anwendung zu debuggen. Einen Fall, in denen dies sehr hilfreich sein kann, ist, wenn Sie einen Haltepunkt mit Visual Studio 2008 nicht werden, auf ein Inline-Skript festgelegt können, sondern soll den Code auf bestimmten Zeile zu beenden, damit Sie den Wert von Variablen überprüfen können.

*Grundlegendes zur ScriptMode-Eigenschaft für das ScriptManager-Steuerelement*

Die ASP.NET AJAX-Bibliothek umfasst debug und release von Skriptversionen, die standardmäßig unter c:\Programme\Microsoft c:\Programme\Microsoft ASP.NET\ASP.NET 2.0 AJAX Extensions\v1.0.61025\MicrosoftAjaxLibrary\System.Web.Extensions\1.0.61025.0 installiert sind. Debug-Skripts sind wohlformatierte, leicht zu lesen und haben mehrere Sys.Debug.assert Aufrufe verteilt diese während der Release-Skripts Leerzeichen entfernt wurden und Sys.Debug-Klasse nur selten, um deren Gesamtgröße zu minimieren.

Die ScriptManager-Steuerelement hinzugefügt, um ASP.NET AJAX-Seiten liest das Compilation-Element-Debug-Attribut in der Datei "Web.config", um zu bestimmen, welche Versionen von Bibliothekskripts zu laden. Sie können jedoch steuern, wenn Debug- oder release Skripts sind geladen (Bibliothekskripts oder Ihre eigenen benutzerdefinierten Skripts) durch Ändern der ScriptMode-Eigenschaft. ScriptMode akzeptiert eine ScriptMode-Enumeration, deren Mitglieder, automatisch "," Debug "," Release "und" erben enthalten.

ScriptMode standardmäßig den Wert von "Auto" Was bedeutet, dass die ScriptManager das Debug-Attribut in der Datei "Web.config" wird geprüft. Wenn Debug "false" ist wird die veröffentlichte Version von ASP.NET AJAX-Bibliothekskripts ScriptManager geladen werden. Beim Debuggen auf "true" festgelegt ist, wird die Debugversion der Skripts geladen werden. Ändern die Eigenschaft ScriptMode zum Freigeben oder zum Debuggen erzwingen ScriptManager das Debug-Attribut in der Datei "Web.config" verfügt über die entsprechenden Skripts unabhängig davon, welcher Wert geladen. Auflisten von 8 zeigt ein Beispiel für das ScriptManager-Steuerelement zum Debug-Skripts aus der ASP.NET AJAX-Bibliothek laden.

**Auflisten von 8. Laden die Debug-Skripts, die über das ScriptManager**.


[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample11.aspx)]

Sie können auch verschiedene Versionen (Debug oder Release) für Ihre eigenen benutzerdefinierten Skripts laden, mithilfe der ScriptManager Skripts Eigenschaft unterstützt zusammen mit der ScriptReference-Komponente, wie im Codebeispiel 9 gezeigt.

**Auflisten von 9. Beim Laden des benutzerdefinierten Skripts, die über das ScriptManager.**


[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample12.aspx)]

> [!NOTE]
> Wenn Sie benutzerdefinierte Skripts mithilfe der Komponente zur ScriptReference laden müssen Sie ScriptManager benachrichtigen, wenn das Skript vollständig geladen wurde, indem Sie den folgenden Code am Ende des Skripts hinzufügen:


[!code-csharp[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample13.cs)]

Der Code in auflisten 9 weist ScriptManager für eine Debugversion des Skripts Person gesucht werden soll, damit sie automatisch nach Person.debug.js statt Person.js gesucht wird. Wenn die Person.debug.js-Datei nicht gefunden wird, dass ein Fehler ausgelöst wird.

In Fällen, in dem Sie möchten eine Debug oder Release-Version des benutzerdefinierten Skripts geladen werden basierend auf dem Wert der Eigenschaft ScriptMode legen Sie für das ScriptManager-Steuerelement können Sie die ScriptReference Steuerelementeigenschaft ScriptMode zu erben festlegen. Dies bewirkt die richtige Version des benutzerdefinierten Skripts geladen werden soll, basierend auf den ScriptManager ScriptMode Eigenschaft wie im Codebeispiel 10 gezeigt. Da die ScriptMode-Eigenschaft des ScriptManager-Steuerelement zum Debuggen festgelegt ist, wird das Skript Person.debug.js geladen und auf der Seite verwendet.

**Auflisten von 10. Die ScriptManager für benutzerdefinierte Skripts Vererben ScriptMode.**


[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample14.aspx)]

Mithilfe der Eigenschaft ScriptMode entsprechend können Sie leichter Debuggen von Anwendungen und vereinfachen das generelle Verfahren. Der ASP.NET AJAX-Bibliothek Versionsskripts sind schwer zu durchlaufen und lesbar, da er codeformatierung entfernt wurde, während die Debugskripts speziell für Debugzwecke formatiert sind.

## <a name="conclusion"></a>Schlussbemerkung

Microsoft ASP.NET AJAX-Technologie bietet eine solide Grundlage zum Erstellen von AJAX-fähigen Anwendungen, die der Endbenutzer durchgängig verbessern können. Jedoch werden als mit jeder Programmiersprache Technologie, Fehlern und anderen Anwendungsprobleme sicherlich entstehen. Über die verschiedenen Optionen für das Debuggen verfügbar wissen, kann viel Zeit und das Ergebnis in einem Produkt stabiler und ausgereifter speichern.

In diesem Artikel haben Sie mehrere verschiedene Verfahren zum Debuggen von ASP.NET AJAX-Webseiten, einschließlich Internet Explorer mit Visual Studio 2008, Web Development Helper und Firebug eingeführt wurde. Diese Tools können insgesamt Debugprozesses vereinfachen, da die Variable den Datenzugriff, Code zeilenweise durchlaufen und Anzeigen von ablaufverfolgungsanweisungen. Zusätzlich zu den verschiedenen debugging-Tools erläutert haben Sie auch, wie der ASP.NET AJAX-Bibliothek Sys.Debug-Klasse in einer Anwendung verwendet werden kann und wie die ScriptManager-Klasse verwendet werden kann, laden, Debug- oder Releaseversionen von Skripts.

## <a name="bio"></a>Lebenslauf

Dan Wahlin (Microsoft Most Valuable Professional für ASP.NET und XML-Webdienste) ist .NET Development Dozenten und Architektur Berater zur technischen Training-Schnittstelle ([www.interfacett.com)](http://www.interfacett.com). Dan gegründet XML für Entwickler ASP.NET-Website ([www.XMLforASP.NET](http://www.XMLforASP.NET)), befindet sich auf die INETA Referent und spricht auf mehrere Konferenzen. Dan Mitautor Professional Windows DNA (Wrox), ASP.NET: Tipps, Lernprogramme und Code (Sams), ASP.NET 1.1 Insider-Lösungen, Professional ASP.NET 2.0 AJAX (Wrox), ASP.NET 2.0 MVP Hacks und erstellte XML für ASP.NET-Entwickler, (Sams). Wenn er nicht Code "," Artikel "oder" Bücher geschrieben wird, hat Dan schreiben und Musik aufzeichnen und Wiedergeben von Golf und Basketball mit seiner Frau und Kinder.

Scott Cate arbeitet mit Microsoft-Web-Technologien seit 1997 und Präsidenten des myKB.com ist ([www.myKB.com](http://www.myKB.com)), in dem er zum Schreiben von ASP.NET spezialisiert-basierten Anwendungen, die Wissensdatenbank softwarelösungen konzentriert. Scott hergestellt werden kann, per e-Mail an [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) oder seinen Blog unter [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Vorherige](understanding-asp-net-ajax-web-services.md)
