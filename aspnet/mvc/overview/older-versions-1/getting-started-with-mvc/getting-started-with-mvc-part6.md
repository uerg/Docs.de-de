---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part6
title: Hinzufügen einer Methode erstellen, und erstellen Sie Ansicht | Microsoft-Dokumentation
author: shanselman
description: Dies ist ein Tutorial für Anfänger, die die Grundlagen von ASP.NET MVC eingeführt werden. Erstellen Sie eine einfache Webanwendung, die aus einer Datenbank liest und schreibt.
ms.author: aspnetcontent
ms.date: 08/14/2010
ms.assetid: a3a90963-0286-4fa0-9b3d-c230cc18b0a3
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part6
msc.type: authoredcontent
ms.openlocfilehash: cf0d721b551c38e8c38e35f82b73ee1b14cd068f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37840965"
---
<a name="adding-a-create-method-and-create-view"></a>Hinzufügen einer Methode erstellen und die Ansicht erstellen
====================
durch [Scott Hanselman](https://github.com/shanselman)

> Dies ist ein Tutorial für Anfänger, die die Grundlagen von ASP.NET MVC eingeführt werden. Sie erstellen eine einfache Webanwendung, die aus einer Datenbank liest und schreibt. Besuchen Sie die [ASP.NET MVC-Informationscenter](../../../index.md) anderen ASP.NET MVC anhand von Tutorials und Beispiele finden.


In diesem Abschnitt werden wir die Unterstützung zum Aktivieren von Benutzern die Erstellung neuer Filme in der Datenbank erforderlich zu implementieren. Dazu müssen wir implementieren die URL der Filme/Create-Aktion.

Implementieren die Filme/Create-URL ist ein zweistufiger Vorgang. Wenn ein Benutzer zuerst die URL der Filme/Create besucht möchten wir sie ein HTML-Formular angezeigt, die sie ausfüllen können, um einen neuen Film einzugeben. Klicken Sie dann, wenn der Benutzer sendet das Formular und Beiträge, die die Daten an den Server wieder, möchten wir den bereitgestellten Inhalt abrufen und speichern Sie sie in der Datenbank.

Wir werden diese beiden Schritte innerhalb von zwei Create()-Methoden in unserer MoviesController-Klasse implementieren. Eine Methode zeigt die &lt;Formular&gt; , dass der Benutzer zum Erstellen eines neuen Films ausfüllen soll. Die zweite Methode übernimmt die bereitgestellten Daten verarbeiten, wenn der Benutzer sendet die &lt;Formular&gt; zurück an den Server, und speichern Sie einen neuen Film in unserer Datenbank.

Im folgenden finden Sie der Code unserer MoviesController-Klasse zur Implementierung hinzugefügt:

[!code-csharp[Main](getting-started-with-mvc-part6/samples/sample1.cs)]

Der obige Code enthält alle des Codes, den wir in unserem Controller benötigen.

Nun implementieren wir die Create View-Vorlage, mit denen ein Formular für den Benutzer angezeigt. Wir klicken Sie mit der rechten Maustaste in der ersten Create-Methode und wählen "Ansicht hinzufügen", um die ansichtsvorlage für unsere Movie-Formular zu erstellen.

Ich wähle, die der ansichtsvorlage eine "Movie" übergeben als die Datenklasse anzeigen soll, und wir anzugeben, dass "Vorlage"Erstellen"erstellen" werden soll.

[![Ansicht hinzufügen](getting-started-with-mvc-part6/_static/image2.png)](getting-started-with-mvc-part6/_static/image1.png)

Nachdem Sie auf die Schaltfläche "hinzufügen" klicken, wird für Sie \Movies\Create.aspx ansichtsvorlage erstellt. Da wir in der Dropdownliste "Inhalt anzeigen" "Erstellen" ausgewählt haben, "Gerüst erstellt das Dialogfeld" Ansicht hinzufügen"automatisch" einige Standardinhalt für uns. Das Gerüst erstellt eine HTML &lt;Formular&gt;, ein Ort für Validierungsfehler zu Nachrichten und da Filme Gerüstbau kennt, Erstellung Bezeichnung und Felder für jede Eigenschaft unserer Klasse.

[!code-aspx[Main](getting-started-with-mvc-part6/samples/sample2.aspx)]

Da unsere Datenbank automatisch einem Film ID erhalten, wir entfernen diese Felder, Referenzmodell. Die ID aus unserer Ansicht erstellen. Entfernen Sie die 7 Zeilen nach &lt;Legende&gt;Felder&lt;/legend&gt; wie darin, dass wir nicht möchten, wird dem ID-Feld veranschaulicht.

Lassen Sie uns nun erstellen Sie einen neuen Film und der Datenbank hinzugefügt. Wir zu diesem Zweck die Anwendung erneut ausführen und finden Sie auf der "/ Movies"-URL, und klicken Sie auf "Erstellen" verknüpfen, um einen neuen Film hinzuzufügen.

[![Erstellen Sie – Windows InternetExplorer](getting-started-with-mvc-part6/_static/image4.png)](getting-started-with-mvc-part6/_static/image3.png)

Wenn wir auf die Schaltfläche "erstellen" klicken, werden wir (per HTTP POST) die Daten in diesem Formular an die Methode /Movies/Create veröffentlicht werden, die wir gerade erstellt haben. Genau wie das System wird bei automatisch hat den Parameter "NumTimes" und "Name" nicht in der URL, und der Parameter für eine Methode zuvor zugewiesen das System automatisch die Formularfelder aus einem Beitrag nehmen und ordnen sie ein Objekt. In diesem Fall werden die Werte aus Feldern in HTML, z. B. "ReleaseDate" und "Title" automatisch in die richtigen Eigenschaften einer neuen Instanz eines Films platziert.

Sehen wir uns die zweite Create-Methode aus unserer MoviesController erneut aus. Beachten Sie, wie sie ein "Movie"-Objekt als Argument akzeptiert:

[!code-csharp[Main](getting-started-with-mvc-part6/samples/sample3.cs)]

Diese Movie-Objekt klicken Sie dann auf die Version [HttpPost] unsere Create-Aktion-Methode übergeben wurde, und wir in der Datenbank gespeichert ist, und klicken Sie dann den Benutzer wieder an die Index() Aktionsmethode das gespeicherte Ergebnis in der Movie-Liste aufgeführt werden umgeleitet:

[![Filmliste – Windows InternetExplorer](getting-started-with-mvc-part6/_static/image6.png)](getting-started-with-mvc-part6/_static/image5.png)

Wir werden nicht überprüft, ob unsere Filme jedoch richtig ist sind, und die Datenbank wird nicht erlauben, einen Film mit kein Titel zu speichern. Es wäre schön, wenn wir den Benutzer mitteilen könnten, der vor der Datenbank einen Fehler ausgelöst hat. Dies als Nächstes machen wir durch Hinzufügen von validierungsunterstützung unserer Anwendung.

> [!div class="step-by-step"]
> [Zurück](getting-started-with-mvc-part5.md)
> [Weiter](getting-started-with-mvc-part7.md)
