---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-9
title: "Hinzufügen eines neuen Elements in der Datenbank | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 0967c29e-e124-4db0-a788-c45d0ff5aff2
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-9
msc.type: authoredcontent
ms.openlocfilehash: d33355b1bd286513958f71ce5521942a6cbb584f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="add-a-new-item-to-the-database"></a>Hinzufügen eines neuen Elements in der Datenbank
====================
durch [Mike Wasson](https://github.com/MikeWasson)

[Herunterladen des abgeschlossenen Projekts](https://github.com/MikeWasson/BookService)

In diesem Abschnitt fügen Sie die Möglichkeit für Benutzer zum Erstellen eines neuen Buchs. Fügen Sie den folgenden Code in "App.js" um das Modell anzeigen:

[!code-javascript[Main](part-9/samples/sample1.js)]

Ersetzen Sie im Index.cshtml das folgende Markup:

[!code-html[Main](part-9/samples/sample2.html)]

Mit:

[!code-html[Main](part-9/samples/sample3.html)]

Dieses Markup erstellt ein Formular zum Senden der Autors eines neuen. Die Werte für den Autor Dropdown-Liste werden einer Datenbindung zu den `authors` Observable im Modell anzeigen. Für die andere Formulareingaben die Werte werden einer Datenbindung zu den `newBook` Eigenschaft des Modells anzeigen.

Der Submit-Ereignishandler auf dem Formular gebunden ist die `addBook` Funktion:

[!code-html[Main](part-9/samples/sample4.html)]

Die `addBook` Funktion liest die aktuellen Werten der datengebundenen Formulareingaben für ein JSON-Objekt zu erstellen. Und es das JSON-Objekt, das zurückgesendet `/api/books`.

>[!div class="step-by-step"]
[Zurück](part-8.md)
[Weiter](part-10.md)
