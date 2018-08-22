---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
title: Zugreifen auf Modelldaten anhand eines Controllers | Microsoft-Dokumentation
author: shanselman
description: Dies ist ein Tutorial für Anfänger, die die Grundlagen von ASP.NET MVC eingeführt werden. Erstellen Sie eine einfache Webanwendung, die aus einer Datenbank liest und schreibt.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: 004703cd-e0e9-4ba7-9974-1b0475c71222
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
msc.type: authoredcontent
ms.openlocfilehash: 76dc324134dc93c9552741fea9f1136abdc9184a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827361"
---
<a name="accessing-your-models-data-from-a-controller"></a>Zugreifen auf Modelldaten anhand eines Controllers
====================
durch [Scott Hanselman](https://github.com/shanselman)

> Dies ist ein Tutorial für Anfänger, die die Grundlagen von ASP.NET MVC eingeführt werden. Sie erstellen eine einfache Webanwendung, die aus einer Datenbank liest und schreibt. Besuchen Sie die [ASP.NET MVC-Informationscenter](../../../index.md) anderen ASP.NET MVC anhand von Tutorials und Beispiele finden.


In diesem Abschnitt werden wir erstellen eine neue MoviesController-Klasse, und Schreiben von Code, der unsere Daten abgerufen und wieder an den Browser, die mithilfe einer ansichtsvorlage anzeigt.

Klicken Sie mit der rechten Maustaste auf den Ordner "Controllers", und stellen Sie eine neue MoviesController.

[![Controller hinzufügen](getting-started-with-mvc-part5/_static/image2.png)](getting-started-with-mvc-part5/_static/image1.png)

Dadurch wird eine neue "MoviesController.cs"-Datei unterhalb unserer \Controllers-Ordner, in unserem Projekt erstellt. Aktualisieren wir die MovieController Abrufen einer Liste von Filmen aus unserer Datenbank neu aufgefüllt.

[!code-csharp[Main](getting-started-with-mvc-part5/samples/sample1.cs)]

Wir werden eine LINQ-Abfrage ausführen, so, dass wir nur freigegeben, nachdem der Sommer 1984 Filme abgerufen. Wir benötigen eine ansichtsvorlage zum Rendern dieser Liste von Filmen zurück, also mit der rechten Maustaste in der Methode und wählen "Ansicht hinzufügen", um es zu erstellen.

Wir müssen das Dialogfeld "Ansicht hinzufügen" um anzugeben, dass wir eine Liste übergeben&lt;Movies.Models.Movie&gt; , unsere Vorlage anzeigen. Im Gegensatz zu den älteren Zeiten, die wir verwendet das Dialogfeld "Ansicht hinzufügen", und wählen Sie eine "Leere" Vorlage erstellen, möchten wir werden anzugeben, dass diesmal wir Visual Studio automatisch "eine ansichtsvorlage für uns mit einigen Standardinhalt Gerüst für". Wir werden dies durch Auswählen des Elements "List" in "Content-Dropdownliste im Menü Ansicht.

Denken Sie daran: Wenn Sie ein erstelltes haben eine neue Klasse Sie Kompilieren Ihrer Anwendung für die es in das Dialogfeld "Ansicht-" angezeigt wird müssen.

![Ansicht hinzufügen](getting-started-with-mvc-part5/_static/image3.png)

Klicken Sie auf Hinzufügen und das System generiert automatisch Code für eine Ansicht für uns, in dem unsere Liste von Filmen angezeigt. Dies ist ein guter Zeitpunkt zum Ändern der &lt;h2&gt; Überschrift z. B. "Meine Movie List", wie zuvor mit der "Hello World" angezeigt.

[![Filme – Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part5/_static/image5.png)](getting-started-with-mvc-part5/_static/image4.png)

Führen Sie die Anwendung, und finden Sie in der Adressleiste auf /Movies. Jetzt haben wir die Daten mit der eine einfache Abfrage in den Controller aus der Datenbank abgerufen und zurückgegeben, die Daten an eine Ansicht, die über Filme bekannt sind. Diese Ansicht, und klicken Sie dann führt Spin-Vorgänge über die Liste der Filme und wird eine Tabelle mit Daten für uns erstellt.

[![Filmliste – Windows InternetExplorer](getting-started-with-mvc-part5/_static/image7.png)](getting-started-with-mvc-part5/_static/image6.png)

Es wird nicht implementiert bearbeiten "," Details "und" Delete-Funktionen mit dieser Anwendung -, sodass wir nicht die Standardlinks benötigen, die die Vorlage Gerüst für uns erstellt. Öffnen Sie die /Movies/Index.aspx-Datei, und entfernen Sie sie.

Hier ist der Quellcode für die wie unsere aktuellen ansichtsvorlage aussehen soll, sobald wir diese Änderungen vornehmen:

[!code-aspx[Main](getting-started-with-mvc-part5/samples/sample2.aspx)]

Es ist die Links, die wir nicht brauchen, Erstellung, daher wird sie für dieses Beispiel entfernt werden. Wir bleiben unseren neu erstellen Link jedoch, wie das nächste ist! So sieht unsere app wie mit dieser Spalte entfernt.

[![Filmliste – Windows InternetExplorer](getting-started-with-mvc-part5/_static/image9.png)](getting-started-with-mvc-part5/_static/image8.png)

Wir haben jetzt eine einfache Liste von unserem Movie-Daten. Jedoch, wenn wir auf den Link "Neu erstellen" klicken, wir erhalten eine Fehlermeldung, da es nicht verknüpft ist! Lassen Sie uns eine Create Action-Methode implementieren, und ermöglichen einem Benutzer zur Eingabe neuer Filme in der Datenbank.

> [!div class="step-by-step"]
> [Zurück](getting-started-with-mvc-part4.md)
> [Weiter](getting-started-with-mvc-part6.md)
