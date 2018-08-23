---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
title: 'Teil 6: Verwenden von Datenanmerkungen zur Modellvalidierung | Microsoft-Dokumentation'
author: jongalloway
description: Dieser tutorialreihe werden alle Schritte ausgeführt, um die ASP.NET MVC Music Store-beispielanwendung zu erstellen. Teil 6 wird die Verwendung von Datenanmerkungen für Modell V behandelt...
ms.author: riande
ms.date: 04/21/2011
ms.assetid: b3193d33-2d0b-4d98-9712-58bd897c62ec
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
msc.type: authoredcontent
ms.openlocfilehash: b666c3cef0b09c6d68cee581a3c27c08e3357cca
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835206"
---
<a name="part-6-using-data-annotations-for-model-validation"></a>Teil 6: Verwenden von Datenanmerkungen bei der Modellüberprüfung
====================
durch [Jon Galloway](https://github.com/jongalloway)

> Die MVC Music Store ist ein lernprogrammanwendung, die eingeführt und erläutert Schritt für Schritt, wie ASP.NET MVC und Visual Studio für die Webentwicklung verwenden.  
>   
> Die MVC Music Store ist eine Implementierung eines einfachen Beispiels die Alben online verkauft und implementiert grundlegende Verwaltung, Benutzeranmeldung und shopping Cart-Funktionalität.  
>   
> Dieser tutorialreihe werden alle Schritte ausgeführt, um die ASP.NET MVC Music Store-beispielanwendung zu erstellen. Teil 6 wird die Verwendung von Datenanmerkungen zur Modellvalidierung behandelt.


Wir haben ein schwerwiegendes Problem mit unserem erstellen und Bearbeiten von Formularen: sie noch keine Überprüfung nicht möglich. Können wir z. B. für Pflichtfelder leer "und" Typ Buchstaben in das Feld für den Preis zu lassen, und der erste Fehler, die, den wir sehen werden, aus der Datenbank ist.

Wir können unsere Anwendung einfach durch Hinzufügen von Datenanmerkungen auf unserem Modellklassen Validierung hinzufügen. Datenanmerkungen können wir beschreiben die gewünschten auf unsere Modelleigenschaften angewendet, und ASP.NET MVC übernimmt erzwungen werden und die entsprechende Nachrichten für unsere Benutzer anzeigen.

## <a name="adding-validation-to-our-album-forms"></a>Hinzufügen einer Validierung zu unseren Album-Formularen

Wir verwenden die folgenden Attribute für die Datenanmerkung:

- **Erforderliche** – gibt an, dass die Eigenschaft ein erforderliches Feld.
- **"DisplayName"** – definiert den Text, der es verwendet werden auf Formularfelder und validierungsmeldungen soll
- **StringLength** – definiert die maximale Länge für ein Zeichenfolgenfeld eingegeben
- **Bereich** – weist den maximalen und minimalen Wert für ein numerisches Feld
- **Binden Sie** – Listet die Felder zum Ausschließen oder einschließen, wenn die Werte für Parameter oder Formular eine Bindung an Eigenschaften des Modells
- **ScaffoldColumn** – ermöglicht das Ausblenden von Feldern von Editor-Formularen

*Hinweis: Weitere Informationen zu Modellvalidierung Datenanmerkung Attribute verwenden, finden Sie auf die MSDN-Dokumentation*[`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)

Öffnen Sie die Album-Klasse, und fügen Sie die folgenden *mit* -Anweisungen an den Anfang.

[!code-csharp[Main](mvc-music-store-part-6/samples/sample1.cs)]

Als Nächstes aktualisieren Sie die Eigenschaften, um die Anzeige und Validierung-Attribute hinzufügen, wie unten dargestellt.

[!code-csharp[Main](mvc-music-store-part-6/samples/sample2.cs)]

Während wir damit beschäftigt sind, haben wir auch das Genre und Interpreteninformationen virtuelle Eigenschaften geändert. Dies kann Entity Framework lazy sie bei Bedarf laden.

[!code-csharp[Main](mvc-music-store-part-6/samples/sample3.cs)]

Klicken Sie nach dem müssen, werden diese Attribute unseres Modells Album hinzugefügt, unsere erstellen und bearbeiten-Bildschirm beginnt sofort mit dem Überprüfen von Feldern, und verwenden die Anzeigenamen haben wir uns entschieden, (z. B. Album Art Url anstelle von AlbumArtUrl). Führen Sie die Anwendung, und navigieren Sie zu /StoreManager/Create.

![](mvc-music-store-part-6/_static/image1.png)

Als Nächstes müssen wir einige Validierungsregeln unterbrochen. Geben Sie einen Preis von 0, und lassen Sie den Titel leer. Wenn wir auf die Schaltfläche "erstellen" klicken, sehen wir das Formular mit Meldungen für Validierungsfehler angezeigt, welche Felder die Validierungsregeln nicht erfüllt, dass wir definiert haben.

![](mvc-music-store-part-6/_static/image2.png)

## <a name="testing-the-client-side-validation"></a>Testen die clientseitige Validierung

Eine serverseitige Validierung ist sehr wichtig ist, aus Anwendungssicht, da der Benutzer die clientseitige Überprüfung umgehen können. Allerdings eine Webseite Formulare, die nur eine serverseitige Validierung implementieren drei schwerwiegende Probleme auf.

1. Der Benutzer muss warten, gesendet werden, auf dem Server überprüft das Formular, und klicken Sie für die Antwort an den Browser gesendet werden.
2. Der Benutzer erhalten nicht sofort ein Feedback, wenn sie ein Feld, damit sie jetzt die Validierungsregeln erfolgreich zu beheben.
3. Wir müssen die Serverressourcen zum Ausführen von Validierungslogik, statt den Browser des Benutzers nutzen verschwenden.

Sie müssen jedoch die ASP.NET MVC 3-gerüstvorlagen die clientseitige Validierung integriert, erfordert keine zusätzliche Arbeit jeglicher Art.

Geben einen einzelnen Buchstaben in das Feld "Titel" erfüllt die überprüfungsanforderungen zu erfüllen, damit der validierungsmeldung sofort entfernt wird.

![](mvc-music-store-part-6/_static/image3.png)


> [!div class="step-by-step"]
> [Zurück](mvc-music-store-part-5.md)
> [Weiter](mvc-music-store-part-7.md)
