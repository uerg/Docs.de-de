---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
title: 'Teil 6: Mithilfe von Datenanmerkungen zur Modellvalidierung | Microsoft Docs'
author: jongalloway
description: Diese Reihe von Lernprogrammen details aller die Schritte zum Erstellen von ASP.NET MVC-Music Store-beispielanwendung. Teil 6 deckt mithilfe von Datenanmerkungen für Modell V...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: b3193d33-2d0b-4d98-9712-58bd897c62ec
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
msc.type: authoredcontent
ms.openlocfilehash: 328eccb4324bb10a7e8dec819a70129fc14c42c4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="part-6-using-data-annotations-for-model-validation"></a>Teil 6: Mithilfe von Datenanmerkungen bei der Modellüberprüfung
====================
durch [Jon Galloway](https://github.com/jongalloway)

> Das MVC-Music Store ist eine lernprogrammanwendung, die führt und erklärt schrittweise, wie mithilfe von ASP.NET MVC und Visual Studio für die Webentwicklung.  
>   
> Das MVC-Music Store handelt es sich um einfache Beispiel Store Implementierung, die online Musikalben verkauft und implementiert grundlegende standortverwaltung Benutzer anmelden und shopping Cart-Funktionalität.  
>   
> Diese Reihe von Lernprogrammen details aller die Schritte zum Erstellen von ASP.NET MVC-Music Store-beispielanwendung. Teil 6 werden die mithilfe von Datenanmerkungen zur Modellvalidierung behandelt.


Wir haben ein ernstes Problem mit unserem erstellen und Bearbeiten von Formularen: machen sie keine Validierung. Staus z. B. für Pflichtfelder leer oder Typ Buchstaben im Feld "Preis" lassen und der erste Fehler, die, den wir sehen, aus der Datenbank ist.

Wir können unsere Anwendung einfach durch Hinzufügen von Datenanmerkungen zu unserem Modellklassen Validierung hinzufügen. Datenanmerkungen ermöglichen es uns zur Beschreibung der Regeln, die wir möchten unsere Modelleigenschaften angewendet, und ASP.NET MVC erzwungen werden und Anzeige von entsprechenden Meldungen unseren Benutzern übernimmt.

## <a name="adding-validation-to-our-album-forms"></a>Unsere Album Formularen Validierungen hinzugefügt

Wir verwenden die folgenden Datenanmerkung Attribute:

- **Erforderliche** – gibt an, dass die Eigenschaft ein Pflichtfeld ist.
- **DisplayName** – definiert den Text, der es verwendet werden auf Formularfelder und validierungsmeldungen soll
- **StringLength** – definiert die maximale Länge für ein Zeichenfolgenfeld eingegeben
- **Bereich** – weist den maximalen und minimalen Wert für ein numerisches Feld
- **Binden Sie** – Listet die Felder zum Ausschließen oder einschließen beim Binden der Parameter oder Form Werte an Modelleigenschaften
- **ScaffoldColumn** – ermöglicht das Ausblenden von Feldern aus Editor Formularen

*Hinweis: Weitere Informationen zum Verwenden von Attributen-Datenanmerkung Modellvalidierung, finden Sie auf die MSDN-Dokumentation*[`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)

Öffnen Sie die Album-Klasse, und fügen Sie die folgenden *mit* -Anweisungen an den Anfang.

[!code-csharp[Main](mvc-music-store-part-6/samples/sample1.cs)]

Als Nächstes aktualisieren Sie die Eigenschaften, um die Anzeige und Validierung Attribute hinzufügen, wie unten dargestellt.

[!code-csharp[Main](mvc-music-store-part-6/samples/sample2.cs)]

Während wir vorhanden sind, haben wir die "Genre" und Interpreten virtuelle Eigenschaften geändert. Dies kann Entity Framework lazy sie nach Bedarf laden.

[!code-csharp[Main](mvc-music-store-part-6/samples/sample3.cs)]

Nach müssen, werden diese Attribute unsere Album-Modell hinzugefügt, unsere erstellen und bearbeiten-Bildschirm beginnt sofort mit dem Überprüfen von Feldern, und wir haben mit dem Anzeigenamen (z. B. Album Art Url statt AlbumArtUrl) ausgewählt. Führen Sie die Anwendung, und navigieren Sie zu /StoreManager/Create.

![](mvc-music-store-part-6/_static/image1.png)

Als Nächstes müssen wir einige Überprüfungsregeln unterbrochen. Geben Sie einen Preis von 0, und lassen Sie den Titel leer. Wenn wir auf die Schaltfläche "erstellen" klicken, sehen wir das Formular angezeigt, wobei Überprüfungsfehlermeldungen anzeigt, welche Felder der Überprüfungsregeln nicht entsprochen haben, dass wir definiert haben.

![](mvc-music-store-part-6/_static/image2.png)

## <a name="testing-the-client-side-validation"></a>Testen die clientseitige Validierung

Serverseitige Validierung ist sehr wichtig, aus der Perspektive einer Anwendung, da Benutzer die clientseitige Überprüfung umgehen können. Allerdings weisen Formulare, die nur die serverseitige Validierung implementiert, drei wesentlichen Probleme auf.

1. Der Benutzer muss warten, für das Formular gebucht, auf dem Server überprüft werden, und für die Antwort an den Browser gesendet wird.
2. Der Benutzer erhalten nicht unmittelbares Feedback, wenn sie ein Feld korrigieren, sodass sie jetzt den setupüberprüfungsregeln entspricht.
3. Wir sind Serverressourcen zum Ausführen der Validierungslogik, statt den Browser des Benutzers nutzt verschwenden.

Glücklicherweise müssen die ASP.NET MVC 3-Vorlagen Gerüst clientseitige Validierung integriert, erfordern keine zusätzliche Arbeit übernehmen.

Eingabe aus einen einzelnen Buchstaben in das Feld "Titel" erfüllt die überprüfungsanforderungen zu erfüllen, weshalb der validierungsmeldung sofort entfernt wird.

![](mvc-music-store-part-6/_static/image3.png)


> [!div class="step-by-step"]
> [Zurück](mvc-music-store-part-5.md)
> [Weiter](mvc-music-store-part-7.md)
