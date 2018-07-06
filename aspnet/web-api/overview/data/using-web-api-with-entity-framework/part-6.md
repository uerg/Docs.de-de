---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-6
title: Erstellen Sie den JavaScript-Client | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 06/16/2014
ms.assetid: 20360326-b123-4b1e-abae-1d350edf4ce4
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-6
msc.type: authoredcontent
ms.openlocfilehash: acadd8100b2698b4b17a750a02f875d4fd51c414
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37826513"
---
<a name="create-the-javascript-client"></a>Erstellen Sie den JavaScript-Client
====================
durch [Mike Wasson](https://github.com/MikeWasson)

[Abgeschlossenes Projekt herunterladen](https://github.com/MikeWasson/BookService)

In diesem Abschnitt erstellen Sie den Client für die Anwendung, die mit HTML, JavaScript, und die ["Knockout.js"](http://knockoutjs.com/) Bibliothek. Wir erstellen die Client-app in Phasen:

- Zeigt eine Liste von Büchern.
- Eine Book-Details werden angezeigt.
- Ein neues Buch wird hinzugefügt.

Die Knockout-Bibliothek wird das Model-View-ViewModel (MVVM)-Muster verwendet:

- Die **Modell** ist die serverseitige Darstellung der Daten in der Business-Domäne (in unserem Fall Büchern und Autoren).
- Die **Ansicht** spielt die Darstellungsschicht (HTML).
- Die **Ansichtsmodell** ist ein JavaScript-Objekt, das die Modelle enthält. Das Ansichtsmodell ist eine Abstraktion der Code der Benutzeroberfläche. Es wurde keine Kenntnisse über die HTML-Darstellung. Stattdessen es repräsentiert die abstrakte Funktionen in der Ansicht, wie z. B. &quot;eine Liste von Büchern&quot;.

Die Ansicht an das Ansichtsmodell datengebunden ist. Updates an das Ansichtsmodell werden in der Ansicht automatisch wiedergegeben. Das Ansichtsmodell ruft Ereignisse wie Schaltflächenklicks ebenfalls aus der Ansicht ab.

![](part-6/_static/image1.png)

Dieser Ansatz erleichtert Ihnen so ändern Sie das Layout und die Benutzeroberfläche der app, da Sie die Bindungen ändern können, ohne Code umzuschreiben. Beispielsweise kann eine Liste von Elementen als veranschaulicht eine `<ul>`, ändern Sie ihn später in einer Tabelle.

## <a name="add-the-knockout-library"></a>Fügen Sie die Knockout-Bibliothek

In Visual Studio aus der **Tools** , wählen Sie im Menü **Bibliothekspaket-Manager**. Wählen Sie dann **-Paket-Manager-Konsole**. Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl aus:

[!code-console[Main](part-6/samples/sample1.cmd)]

Dieser Befehl fügt die Knockout-Dateien zum Ordner "Scripts" an.

## <a name="create-the-view-model"></a>Erstellen des Ansichtsmodells

Fügen Sie eine JavaScript-Datei mit dem Namen "App.js", um den Ordner "Scripts" hinzu. (Klicken Sie im Projektmappen-Explorer mit der Maustaste Ordner "Scripts", wählen Sie **hinzufügen**, und wählen Sie dann **JavaScript-Datei**.) Fügen Sie in den folgenden Code:

[!code-javascript[Main](part-6/samples/sample2.js)]

In Knockout die `observable` Klasse ermöglicht die Datenbindung. Wenn der Inhalt der Observable-Objekt ändern, benachrichtigt das beobachtbare Objekt alle datengebundenen Steuerelemente, damit sie sich selbst aktualisiert werden können. (Die `observableArray` Klasse ist die Array-Version von *Observable*.) Zunächst hat unser Ansichtsmodell zwei wahrnehmbare Elemente:

- `books` enthält die Liste der Bücher.
- `error` enthält eine Fehlermeldung angezeigt, wenn ein AJAX-Aufruf ein Fehler auftritt.

Die `getAllBooks` Weise wird einen AJAX-Aufruf, um die Liste der Bücher zu erhalten. Und sie das Ergebnis auf überträgt die `books` Array.

Die `ko.applyBindings` Methode ist Teil der die Knockout-Bibliothek. Es dauert das Ansichtsmodell als Parameter und richtet die Datenbindung.

## <a name="add-a-script-bundle"></a>Fügen Sie ein Skript-Paket hinzu

Bündelung ist ein Feature in ASP.NET 4.5, die ganz einfach kombinieren oder mehrere Dateien in einer einzelnen Datei gebündelt. Bündelung reduziert die Anzahl der Anforderungen an den Server, der Seitenladezeit verbessert werden kann.

Öffnen Sie die Datei App\_Start/BundleConfig.cs. Fügen Sie den folgenden Code, der RegisterBundles-Methode.

[!code-csharp[Main](part-6/samples/sample3.cs)]

> [!div class="step-by-step"]
> [Zurück](part-5.md)
> [Weiter](part-7.md)
