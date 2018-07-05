---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-9
title: Hinzufügen eines neuen Elements in der Datenbank | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 06/16/2014
ms.assetid: 0967c29e-e124-4db0-a788-c45d0ff5aff2
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-9
msc.type: authoredcontent
ms.openlocfilehash: 36251ba907a6f580b63f0fded0591c26b6ff879e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818507"
---
<a name="add-a-new-item-to-the-database"></a>Hinzufügen eines neuen Elements in der Datenbank
====================
durch [Mike Wasson](https://github.com/MikeWasson)

[Abgeschlossenes Projekt herunterladen](https://github.com/MikeWasson/BookService)

In diesem Abschnitt fügen Sie die Möglichkeit für Benutzer, um ein neues Buch zu erstellen. Fügen Sie den folgenden Code in "app.js" an das Ansichtsmodell:

[!code-javascript[Main](part-9/samples/sample1.js)]

Ersetzen Sie in "Index.cshtml" das folgende Markup:

[!code-html[Main](part-9/samples/sample2.html)]

Durch:

[!code-html[Main](part-9/samples/sample3.html)]

Dieses Markup erstellt ein Formular zum Senden eines neuen Autors. Die Werte für den Autor Dropdown-Liste sind eine Datenbindung an die `authors` Observable im Ansichtsmodell. Für die andere formeingaben die Werte sind eine Datenbindung an die `newBook` -Eigenschaft des Ansichtsmodells.

Der Submit-Handler für das Formular gebunden ist, um die `addBook` Funktion:

[!code-html[Main](part-9/samples/sample4.html)]

Die `addBook` Funktion liest die aktuellen Werten der datengebundenen Formulareingaben für ein jsonobjekt zu erstellen. Und es sich um das JSON-Objekt, das zurücksendet `/api/books`.

> [!div class="step-by-step"]
> [Zurück](part-8.md)
> [Weiter](part-10.md)
