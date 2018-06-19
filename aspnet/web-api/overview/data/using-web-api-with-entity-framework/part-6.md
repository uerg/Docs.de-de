---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-6
title: Erstellen der JavaScript-Client | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 20360326-b123-4b1e-abae-1d350edf4ce4
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-6
msc.type: authoredcontent
ms.openlocfilehash: 29d50e448e6d282c7db06b9d1946ac221347e1ea
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879308"
---
<a name="create-the-javascript-client"></a>Erstellen der JavaScript-Client
====================
durch [Mike Wasson](https://github.com/MikeWasson)

[Herunterladen des abgeschlossenen Projekts](https://github.com/MikeWasson/BookService)

In diesem Abschnitt erstellen Sie den Client für die Anwendung mit HTML, JavaScript, und die [Knockout.js](http://knockoutjs.com/) Bibliothek. Wir müssen die Client-app in Phasen entwerfen:

- Zeigt eine Liste von Büchern.
- Ein Buch Details angezeigt.
- Hinzufügen eines neuen Buchs an.

Die Bibliothek Knockout verwendet das Model-View-ViewModel (MVVM)-Muster:

- Die **Modell** ist die serverseitige Darstellung der Daten in der Business-Domäne (in unserem Groß-/Kleinschreibung, Bücher und Autoren).
- Die **Ansicht** ist die Darstellungsschicht (HTML).
- Die **Ansichtsmodell** ist ein JavaScript-Objekt, das die Modelle enthält. Das Ansichtsmodell ist eine Abstraktion der Code der Benutzeroberfläche. Sie hat keine Kenntnis der HTML-Darstellung. Stattdessen dar abstrakte Funktionen in der Ansicht, z. B. &quot;eine Liste von Büchern&quot;.

Die Ansicht für das Ansichtsmodell datengebunden ist. Updates für das ViewModel werden automatisch in der Sicht widergespiegelt. Das Ansichtsmodell ruft auch Ereignisse aus der Ansicht ab, wie z. B. Schaltfläche klickt.

![](part-6/_static/image1.png)

Dieser Ansatz erleichtert so ändern Sie das Layout und die Benutzeroberfläche der app, da Sie die Bindungen ändern können, ohne Code umzuschreiben. Sie können z. B. eine Liste von Elementen als Anzeigen einer `<ul>`, ändern Sie ihn später in einer Tabelle.

## <a name="add-the-knockout-library"></a>Fügen Sie der Bibliothek Knockout hinzu

In Visual Studio aus der **Tools** klicken Sie im Menü **Bibliothekspaket-Manager**. Wählen Sie dann **Package Manager Console**. Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl aus:

[!code-console[Main](part-6/samples/sample1.cmd)]

Mit diesem Befehl wird der Ordner "Skripts" Knockout Dateien hinzugefügt.

## <a name="create-the-view-model"></a>Erstellen des Modells anzeigen

Fügen Sie eine JavaScript-Datei mit dem Namen "App.js", um den Ordner "Skripts" hinzu. (Im Projektmappen-Explorer mit der Maustaste den Ordner "Skripts", wählen Sie **hinzufügen**, und wählen Sie dann **JavaScript-Datei**.) Fügen Sie in den folgenden Code:

[!code-javascript[Main](part-6/samples/sample2.js)]

In Knockout die `observable` Klasse ermöglicht die Datenbindung. Wenn der Inhalt der Observable-Objekt ändern, benachrichtigt der Observable-Objekt alle datengebundenen Steuerelemente, damit selbst aktualisiert werden können. (Die `observableArray` Klasse ist die Array-Version des *Observable*.) Zunächst weist unsere Ansichtsmodell zwei Wahrnehmbare Elemente:

- `books` enthält die Liste der Bücher an.
- `error` enthält eine Fehlermeldung, wenn ein AJAX-Aufruf ein Fehler auftritt.

Die `getAllBooks` Methode ist einen AJAX-Aufruf zum Abrufen der Liste von Büchern. Und es sich um das Ergebnis auf verlagert die `books` Array.

Die `ko.applyBindings` Methode ist Teil der Knockout-Bibliothek. Wird das Ansichtsmodell als Parameter, die dem Datenbindung eingerichtet.

## <a name="add-a-script-bundle"></a>Fügen Sie ein Skript-Paket hinzu

Bundling ist ein Feature in ASP.NET 4.5, die ganz einfach kombinieren oder mehrere Dateien in einer einzelnen Datei bündeln. Bundling reduziert die Anzahl der Anforderungen an den Server, der Seitenladezeit verbessert werden kann.

Öffnen Sie die Datei App\_Start/BundleConfig.cs. Fügen Sie den folgenden Code, um die RegisterBundles-Methode.

[!code-csharp[Main](part-6/samples/sample3.cs)]

> [!div class="step-by-step"]
> [Zurück](part-5.md)
> [Weiter](part-7.md)
