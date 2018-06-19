---
uid: visual-studio/overview/2013/using-browser-link
title: Browserlink in Visual Studio 2013 mit | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/04/2013
ms.topic: article
ms.assetid: 46cbfe20-b4dc-449b-9016-80657dd44fbe
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/using-browser-link
msc.type: authoredcontent
ms.openlocfilehash: e5a13405a303580ec8c1d4cdacafc26c6f8ff34a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
ms.locfileid: "28044109"
---
<a name="using-browser-link-in-visual-studio-2013"></a>Mithilfe der Browserlink in Visual Studio 2013
====================
durch [Mike Wasson](https://github.com/MikeWasson)

Browserlink ist ein neues Feature in Visual Studio 2013, der einen Kommunikationskanal zwischen der Entwicklungs- und eine oder mehrere Webbrowsern erstellt. Browser-Link, um Ihre Webanwendung gleichzeitig in mehreren Browsern aktualisieren können Sie eignet sich für browserübergreifende Tests.

- [Browser aktualisieren](#browser-refresh)
- [Die Browserlink-Dashboard anzeigen](#dashboard)
- [Browserlink für für statische HTML-Dateien aktivieren](#static-html)
- [Deaktivieren von Browserlink](#disabling)
- [Wie funktioniert es?](#how-it-works)

<a id="browser-refresh"></a>
## <a name="browser-refresh"></a>Browser aktualisieren

Mit den Browser zu aktualisieren können Sie mehreren Browsern aktualisieren, die mit Visual Studio mit Browser Link verbunden sind.

Verwendung von Browser zu aktualisieren, müssen Sie zunächst erstellen Sie eine ASP.NET-Anwendung mit einer der Projektvorlagen. Debuggen der Anwendung durch Drücken von F5 oder klicken Sie auf das Pfeilsymbol in der Symbolleiste:

![](using-browser-link/_static/image1.png)

Sie können auch die Dropdownliste zur Auswahl eines bestimmten Browsers zum Debuggen verwenden.

![](using-browser-link/_static/image2.png)

Wählen Sie zum Debuggen mit mehreren Browsern **Browserauswahl**. In der **Browserauswahl** Dialogfeld, halten die STRG-Taste, um mehr als ein Browser auszuwählen. Klicken Sie auf **Durchsuchen** mit den ausgewählten Browsern zu debuggen. Browserlink funktioniert auch, wenn Sie einen Browser außerhalb von Visual Studio starten und navigieren Sie zu der URL der Anwendung.

![](using-browser-link/_static/image3.png)

Die Browserlink-Steuerelemente befinden sich in der Dropdownliste mit den zirkuläre Pfeilsymbol. Das Symbol "Pfeil" ist die **aktualisieren** Schaltfläche.

![](using-browser-link/_static/image4.png)

Um festzustellen, welche Browser verbunden sind, zeigen Sie die Maus auf die **aktualisieren** Schaltfläche während des Debuggens. Die verbundenen Browser werden in einer QuickInfo-Fenster angezeigt.

![](using-browser-link/_static/image5.png)

Um die verbundenen Browser zu aktualisieren, klicken Sie auf die **aktualisieren** klicken, oder drücken Sie STRG + ALT + EINGABETASTE. Das folgende Bildschirmfoto zeigt z. B. ein ASP.NET-Projekt, die ich mit der MVC 5-Projektvorlage erstellt. Sie können die Anwendung ausgeführt wird, in zwei Browsern oben sehen. Unten ist das Projekt in Visual Studio geöffnet.

![](using-browser-link/_static/image6.png)

In Visual Studio geändert ich die &lt;h1&gt; Überschrift auf der Startseite ":

![](using-browser-link/_static/image7.png)

Ich beim Klicken auf die **aktualisieren** Schaltfläche, die Änderung wurde in beide Browserfenster angezeigt:

![](using-browser-link/_static/image8.png)

**Notizen**

- Legen Sie zum Aktivieren der Browserlink `debug=true` in der [ &lt;Kompilierung&gt; ](https://msdn.microsoft.com/library/s10awwz0(v=vs.85).aspx) Element in der Datei Web.config des Projekts.
- Die Anwendung muss auf "localhost" ausgeführt werden.
- Die Anwendung muss .NET 4.0 oder höher abzielen.

<a id="dashboard"></a>
## <a name="viewing-the-browser-link-dashboard"></a>Die Browserlink-Dashboard anzeigen

Die Browserlink-Dashboard zeigt Informationen zu den Browserlink-Verbindungen. Wählen Sie zum Anzeigen eines Dashboards, die Browserlink-Dropdownmenü (den kleinen Pfeil neben der **aktualisieren** Schaltfläche). Klicken Sie dann auf **Browserlink-Dashboard**.

![](using-browser-link/_static/image9.png)

Das Dashboard Listet den verbundenen Browsern und die URL, zu dem jeweiligen Browser navigiert ist.

![](using-browser-link/_static/image10.png)

Die **Voraussetzungen** Abschnitt zeigt alle Schritte zum Aktivieren der Browserlink für dieses Projekt. Das folgende Bildschirmfoto zeigt z. B. ein Projekt, "debug" auf "false" in der Datei "Web.config" festgelegt ist.

![](using-browser-link/_static/image11.png)

<a id="static-html"></a>
## <a name="enabling-browser-link-for-static-html-files"></a>Browserlink für für statische HTML-Dateien aktivieren

Um Browserlink für statische HTML-Dateien zu aktivieren, fügen Sie Folgendes in die Datei "Web.config" ein.

[!code-xml[Main](using-browser-link/samples/sample1.xml)]

Entfernen Sie diese Einstellung, wenn Sie Ihr Projekt veröffentlichen, aus Gründen der Leistung.

<a id="disabling"></a>
## <a name="disabling-browser-link"></a>Deaktivieren von Browserlink

Browserlink ist standardmäßig aktiviert. Es gibt mehrere Möglichkeiten, um ihn zu deaktivieren:

- Deaktivieren Sie auf dem Browserlink-Dropdown-Menü **Browserlink aktivieren**. 

    ![](using-browser-link/_static/image12.png)
- Fügen Sie einen Schlüssel namens "Vs: EnableBrowserLink" mit dem Wert "False" im Abschnitt "appSettings" in der Datei "Web.config" hinzu. 

    [!code-xml[Main](using-browser-link/samples/sample2.xml)]
- Festlegen von Debug-in der Datei "Web.config" auf "false". 

    [!code-xml[Main](using-browser-link/samples/sample3.xml)]

<a id="how-it-works"></a>
## <a name="how-does-it-work"></a>Wie funktioniert es?

Browser Replikationsverkehr [SignalR](../../../signalr/index.md) um einen Kommunikationskanal zwischen Visual Studio und den Browser zu erstellen. Wenn Browserlink aktiviert ist, verhält sich Visual Studio als SignalR-Server, dem mehrere Clients (Browser), eine Verbindung herstellen können. Browserlink registriert auch ein HTTP-Modul mit ASP.NET. Dieses Modul fügt spezielle &lt;Skript&gt; Verweise in jede Seitenanforderung vom Server. Sie können die Skriptverweise anzeigen, indem Sie Sie im Browser "Quelltext anzeigen" auswählen.

![](using-browser-link/_static/image13.png)

Die Quelldateien werden nicht geändert. Das HTTP-Modul fügt die Skriptverweise dynamisch.

Da der Browser clientseitigen Code alle JavaScript ist, funktioniert in allen Browsern, [SignalR unterstützt](../../../signalr/overview/getting-started/supported-platforms.md), ohne dass einen beliebigen Browser-Plug-in.
