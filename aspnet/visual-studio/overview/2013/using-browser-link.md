---
uid: visual-studio/overview/2013/using-browser-link
title: Verwenden einer Browserverknüpfung in Visual Studio 2013 | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 10/04/2013
ms.assetid: 46cbfe20-b4dc-449b-9016-80657dd44fbe
msc.legacyurl: /visual-studio/overview/2013/using-browser-link
msc.type: authoredcontent
ms.openlocfilehash: 9da93c279bfa2af614733e3234ba62429abf688a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37822194"
---
<a name="using-browser-link-in-visual-studio-2013"></a>Verwenden einer Browserverknüpfung in Visual Studio 2013
====================
durch [Mike Wasson](https://github.com/MikeWasson)

Browserverknüpfung ist ein neues Feature in Visual Studio 2013, der einen Kommunikationskanal zwischen der Entwicklungs- und eine oder mehrere Webbrowser erstellt. Sie können Browser Link, um Ihre Webanwendung in mehreren Browsern gleichzeitig zu aktualisieren, dies ist hilfreich für browserübergreifende Tests verwenden.

- [Aufrufbrowser aktualisieren](#browser-refresh)
- [Die Browserlink-Dashboard anzeigen](#dashboard)
- [Aktivieren von Browserlink, um statische HTML-Dateien](#static-html)
- [Deaktivieren von Browserlink](#disabling)
- [Wie funktioniert es?](#how-it-works)

<a id="browser-refresh"></a>
## <a name="browser-refresh"></a>Aufrufbrowser aktualisieren

Mit den Browser zu aktualisieren können Sie mehrere Browser aktualisieren, die in Visual Studio über Browser Link verbunden sind.

Um den Browser aktualisieren, verwenden zu können, müssen Sie zuerst erstellen Sie eine ASP.NET-Anwendung mit einer der Projektvorlagen. Debuggen der Anwendung durch Drücken von F5 oder durch Klicken auf das Pfeilsymbol in der Symbolleiste:

![](using-browser-link/_static/image1.png)

Sie können auch die Dropdownliste zur Auswahl eines bestimmten Browsers zum Debuggen verwenden.

![](using-browser-link/_static/image2.png)

Wählen Sie zum Debuggen mit mehreren Browsern **Browserauswahl**. In der **Browserauswahl** Dialogfeld, halten Sie die STRG-Taste mehrere Browser auswählen. Klicken Sie auf **Durchsuchen** zum Debuggen mit den ausgewählten Browsern. Browserverknüpfung funktioniert auch, wenn Sie einen Browser außerhalb von Visual Studio startet, und navigieren Sie zu der Anwendungs-URL.

![](using-browser-link/_static/image3.png)

Die Browserlink-Steuerelemente befinden sich in der Dropdownliste mit dem kreisförmigen Pfeil-Symbol. Das Symbol "Pfeil" ist die **aktualisieren** Schaltfläche.

![](using-browser-link/_static/image4.png)

Um anzuzeigen, welche Browser verbunden sind, zeigen Sie auf den die **aktualisieren** Schaltfläche während des Debuggens. Die verbundenen Browser werden in einer QuickInfo-Fenster angezeigt.

![](using-browser-link/_static/image5.png)

Um die verbundenen Browser zu aktualisieren, klicken Sie auf die **aktualisieren** Schaltfläche oder drücken Sie STRG + ALT + Eingabe. Der folgende Screenshot zeigt beispielsweise ein ASP.NET-Projekt, den ich erstellt, mit der MVC 5-Projektvorlage. Sie sehen, dass die Anwendung in beiden Browsern oben ausgeführt wird. Unten ist das Projekt in Visual Studio geöffnet.

![](using-browser-link/_static/image6.png)

In Visual Studio geändert ich die &lt;h1&gt; Überschrift auf der Startseite:

![](using-browser-link/_static/image7.png)

Ich beim Klicken auf die **aktualisieren** Schaltfläche, die Änderung in beide Browserfenster angezeigt:

![](using-browser-link/_static/image8.png)

**Notizen**

- Legen Sie zum Aktivieren des Browserlinks `debug=true` in die [ &lt;Kompilierung&gt; ](https://msdn.microsoft.com/library/s10awwz0(v=vs.85).aspx) Element in der Datei Web.config des Projekts.
- Die Anwendung muss auf "localhost" ausgeführt werden.
- Die Anwendung muss .NET 4.0 oder höher verweisen können.

<a id="dashboard"></a>
## <a name="viewing-the-browser-link-dashboard"></a>Die Browserlink-Dashboard anzeigen

Die Browserlink-Dashboard zeigt Informationen zu den Browserlink-Verbindungen. Um das Dashboard anzuzeigen, wählen Sie im Dropdownmenü mit den Browser Link (den kleinen Pfeil neben der **aktualisieren** Schaltfläche). Klicken Sie dann auf **Browserlink-Dashboard**.

![](using-browser-link/_static/image9.png)

Die verbundenen Browser und die URL, zu dem jeweiligen Browser navigiert ist, werden im Dashboard aufgeführt.

![](using-browser-link/_static/image10.png)

Die **Voraussetzungen** Abschnitt wird gezeigt, alle erforderlichen Schritte zum Aktivieren von Browser Link für das Projekt. Der folgende Screenshot zeigt beispielsweise ein Projekt, in denen "debug" auf "false" in der Datei "Web.config" festgelegt ist.

![](using-browser-link/_static/image11.png)

<a id="static-html"></a>
## <a name="enabling-browser-link-for-static-html-files"></a>Aktivieren von Browserlink, um statische HTML-Dateien

Fügen Sie Folgendes zum Aktivieren von Browser Link für statische HTML-Dateien zu Ihrer Datei "Web.config".

[!code-xml[Main](using-browser-link/samples/sample1.xml)]

Zur Verbesserung der Leistung entfernen Sie diese Einstellung, wenn Sie Ihr Projekt veröffentlichen.

<a id="disabling"></a>
## <a name="disabling-browser-link"></a>Deaktivieren von Browserlink

Browserverknüpfung ist standardmäßig aktiviert. Es gibt mehrere Möglichkeiten, um ihn zu deaktivieren:

- Deaktivieren Sie im Dropdownmenü mit den Browserlink **Aktivieren des Browserlinks**. 

    ![](using-browser-link/_static/image12.png)
- Fügen Sie in der Datei "Web.config" einen Schlüssel namens "Vs: EnableBrowserLink" durch den Wert "False" im Abschnitt "appSettings" hinzu. 

    [!code-xml[Main](using-browser-link/samples/sample2.xml)]
- Festlegen von Debug-in der Datei "Web.config" auf "false". 

    [!code-xml[Main](using-browser-link/samples/sample3.xml)]

<a id="how-it-works"></a>
## <a name="how-does-it-work"></a>Wie funktioniert es?

Browserverknüpfung verwendet [SignalR](../../../signalr/index.md) um einen Kommunikationskanal zwischen Visual Studio und den Browser zu erstellen. Wenn Browser Link aktiviert ist, fungiert Visual Studio als SignalR-Server, dem mehrere Clients (Browser) herstellen können. Browserverknüpfung registriert ein HTTP-Modul auch mit ASP.NET. Dieses Modul fügt spezielle &lt;Skript&gt; verweisen in jede Anforderung einer Seite vom Server. Sie können die Skriptverweise sehen, indem Sie Sie im Browser "Quelltext anzeigen" auswählen.

![](using-browser-link/_static/image13.png)

Die Quelldateien werden nicht geändert werden. Das HTTP-Modul fügt die Skriptverweise dynamisch.

Da der Browser-Side-Code alle JavaScript ist, funktioniert in allen Browsern, [SignalR unterstützt](../../../signalr/overview/getting-started/supported-platforms.md), ohne dass einen beliebigen Browser-Plug-in.
