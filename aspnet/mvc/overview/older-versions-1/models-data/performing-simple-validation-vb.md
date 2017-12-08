---
uid: mvc/overview/older-versions-1/models-data/performing-simple-validation-vb
title: "Ausführen von einfachen Überprüfung (VB) | Microsoft Docs"
author: StephenWalther
description: "Erfahren Sie, wie die Validierung in ASP.NET MVC-Anwendung. In diesem Lernprogramm führt Stephen Walther Sie Modellzustand und die Validierung, HTML-Hilfsobjekt..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: df6cf4b7-0bb3-4c4e-b17a-bd78a759a6bc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/performing-simple-validation-vb
msc.type: authoredcontent
ms.openlocfilehash: 2bc4cdbcd267bcdd3e71abc4c52664ae62c5c02e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="performing-simple-validation-vb"></a>Ausführen von einfachen Überprüfung (VB)
====================
durch [Stephen Walther](https://github.com/StephenWalther)

> Erfahren Sie, wie die Validierung in ASP.NET MVC-Anwendung. In diesem Lernprogramm führt Stephen Walther Sie Modellzustand sowie die Validierung, HTML-Hilfsprogramme.


Ziel dieses Lernprogramms wird erläutert, wie Sie die Validierung innerhalb einer ASP.NET MVC-Anwendung ausführen können. Beispielsweise erfahren Sie, wie Sie verhindern, dass eine Person senden eines Formulars, das einen Wert für ein Pflichtfeld nicht enthält. Erfahren Sie, wie Modellzustand und die Validierung HTML-Hilfsmethoden verwenden.

## <a name="understanding-model-state"></a>Grundlegendes zu Modellstatus

Sie mithilfe Modellstatus – oder genauer gesagt das Modellzustandswörterbuch - Validierungsfehler dar. Beispielsweise überprüft die Create()-Aktion im Codebeispiel 1 die Eigenschaften einer Klasse Product vor dem Hinzufügen der Product-Klasse zu einer Datenbank.


Ich bin nicht empfehlen, dass Sie die Überprüfung oder Datenbanklogik einen Controller hinzufügen. Ein Controller sollte nur im Zusammenhang mit der flusssteuerung für die Anwendung Logik enthalten. Wir machen eine Verknüpfung zum Dinge einfach zu halten.


**1 – Controllers\ProductController.vb auflisten**

[!code-vb[Main](performing-simple-validation-vb/samples/sample1.vb)]

Im Codebeispiel 1 werden die Eigenschaften Name, Beschreibung und "UnitsInStock" die Produktklasse überprüft. Wenn eine dieser Eigenschaften einen Überprüfungstest fehlschlagen wird ein Fehler auf das Modellzustandswörterbuch (dargestellt durch die ModelState-Eigenschaft der Controllerklasse) hinzugefügt.

Wenn Fehler, im Modellstatus vorliegen gibt die ModelState.IsValid-Eigenschaft "false" zurück. In diesem Fall wird das HTML-Formular für das Erstellen eines neuen Produkts erneut angezeigt. Wenn keine gültigkeitsprobleme bestehen, wird das neue Produkt andernfalls mit der Datenbank hinzugefügt.

## <a name="using-the-validation-helpers"></a>Verwenden die Hilfsprogramme für die Validierung

ASP.NET MVC-Framework umfasst zwei Hilfsmethoden der Überprüfung: das Html.ValidationMessage()-Hilfsobjekt und der Html.ValidationSummary()-Hilfsprogramm. Verwenden Sie diese zwei Hilfsmethoden in einer Ansicht, um Überprüfungsfehlermeldungen anzuzeigen.

Die Hilfsprogramme Html.ValidationMessage() und Html.ValidationSummary() dienen in den Ansichten erstellen und bearbeiten, die von ASP.NET-MVC-Gerüstbau automatisch generiert werden. Führen Sie diese Schritte aus, um die Erstellungsansicht generiert:

1. Mit der rechten Maustaste der Create()-Aktion des Produkts-Controllers ein, und wählen Sie die Menüoption **Ansicht hinzufügen** (siehe Abbildung 1).
2. In der **Ansicht hinzufügen** Dialogfeld, aktivieren Sie das Kontrollkästchen mit der Bezeichnung **eine stark typisierte Ansicht erstellen** (siehe Abbildung 2).
3. Aus der **Datenklasse anzeigen** Dropdown Liste, wählen Sie die Product-Klasse.
4. Aus der **Inhalt anzeigen** Dropdown-Liste wählen erstellen.
5. Klicken Sie auf die Schaltfläche **Hinzufügen**.


Stellen Sie sicher, dass Ihre Anwendung vor dem Hinzufügen einer Ansicht zu erstellen. Andernfalls, die Liste der Klassen erscheinen nicht in der **Datenklasse anzeigen** Dropdown-Liste.


[![Das Dialogfeld "Neues Projekt"](performing-simple-validation-vb/_static/image1.jpg)](performing-simple-validation-vb/_static/image1.png)

**Abbildung 01**: Hinzufügen einer Ansicht ([klicken Sie hier, um das Bild in voller Größe angezeigt](performing-simple-validation-vb/_static/image2.png))


[![Das Dialogfeld "Neues Projekt"](performing-simple-validation-vb/_static/image2.jpg)](performing-simple-validation-vb/_static/image3.png)

**Abbildung 02**: Erstellen einer stark typisierten Ansicht ([klicken Sie hier, um das Bild in voller Größe angezeigt](performing-simple-validation-vb/_static/image4.png))


Nachdem Sie diese Schritte abgeschlossen haben, erhalten Sie die Erstellungsansicht 2 aufgelistet.

**Auflisten von 2 – Views\Product\Create.aspx**

[!code-aspx[Main](performing-simple-validation-vb/samples/sample2.aspx)]

Auflisten von 2 wird das Hilfsobjekt Html.ValidationSummary() sofort über die HTML-Formular aufgerufen. Dieses Hilfsprogramm wird verwendet, um eine Liste der Überprüfungsfehlermeldungen anzuzeigen. Das Hilfsobjekt Html.ValidationSummary() rendert die Fehler in einer Liste mit Aufzählungszeichen aus.

Das Hilfsobjekt Html.ValidationMessage() wird neben jeder HTML-Formular Felder bezeichnet. Dieses Hilfsprogramm wird verwendet, um eine Fehlermeldung rechts neben einem Formularfeld anzuzeigen. In Fall 2 auflisten wird das Hilfsprogramm Html.ValidationMessage() ein Sternchen angezeigt, wenn ein Fehler vorliegt.

Die Seite in Abbildung 3 veranschaulicht die Fehlermeldungen, die durch die Überprüfung Hilfsprogramme gerendert wird, wenn fehlende Felder mit ungültigen Werten des Formulars senden.


[![Das Dialogfeld "Neues Projekt"](performing-simple-validation-vb/_static/image3.jpg)](performing-simple-validation-vb/_static/image5.png)

**Abbildung 03**: Erstellen Sie die Ansicht mit Problemen übermittelt ([klicken Sie hier, um das Bild in voller Größe angezeigt](performing-simple-validation-vb/_static/image6.png))


Beachten Sie, dass die Darstellung der HTML-Datei die Eingabe-Felder auch geändert werden, wenn ein Validierungsfehler vorliegt. Die Html.TextBox() Helper rendert eine *Klasse = "Eingabe-Validierungsfehler"* -Attribut, wenn es ein Validierungsfehler tritt verknüpft sind, mit der Eigenschaft, die von der Html.TextBox() Helper gerendert.

Es gibt drei Klassen cascading Stylesheet verwendet, um die Darstellung von Überprüfungsfehlern zu steuern:

- Eingabe-Überprüfung-Fehler - angewendet, um die &lt;input&gt; Tag von Html.TextBox() Helper gerendert.
- Feld-Überprüfung-Fehler - angewendet wird, um die &lt;umfassen&gt; Tag, die durch das Hilfsprogramm Html.ValidationMessage() gerendert.
- Summary-Validierungsfehler - angewendet wird, um die &lt;Ul&gt; Tag, die durch das Hilfsprogramm Html.ValidationSumamry() gerendert.

Sie können diese Klassen für cascading Stylesheet ändern und aus diesem Grund ändern die Darstellung von Validierungsfehlern, ändern die Datei "Site.CSS" im Ordner "Content".

> [!NOTE] 
> 
> HtmlHelper-Klasse enthält schreibgeschützte statische Eigenschaften aus, für das Abrufen der Namen der Überprüfung CSS verknüpften Klassen. Diese statische Eigenschaften werden ValidationInputCssClassName ValidationFieldCssClassName und ValidationSummaryCssClassName benannt.


## <a name="prebinding-validation-and-postbinding-validation"></a>Überprüfung und Postbinding Überprüfung Prebinding

Wenn Sie das HTML-Formular für das Erstellen eines Produkts senden und Sie einen ungültigen Wert für das Preisfeld und keinen Wert für das Feld "UnitsInStock" eingeben, klicken Sie dann erhalten die Überprüfung Nachrichten angezeigt, die in Abbildung 4 Sie. Woher stammen diese Überprüfungsfehlermeldungen?


[![Das Dialogfeld "Neues Projekt"](performing-simple-validation-vb/_static/image4.jpg)](performing-simple-validation-vb/_static/image7.png)

**Abbildung 04**: Validierungsfehler Prebinding ([klicken Sie hier, um das Bild in voller Größe angezeigt](performing-simple-validation-vb/_static/image8.png))


Es gibt tatsächlich zwei Arten von Validierungsfehlermeldungen - erzeugt werden, bevor die HTML-Felder auf eine Klasse gebunden sind und die generiert werden, nachdem die Felder der Klasse gebunden sind. Das heißt, es sind Überprüfungsfehler prebinding und postbinding Validierungsfehler.

Die Create()--Aktion, die von der Produkt-Controller im Codebeispiel 1 verfügbar gemacht werden akzeptiert eine Instanz der Product-Klasse. Die Signatur der Methode erstellen, sieht wie folgt:

[!code-vb[Main](performing-simple-validation-vb/samples/sample3.vb)]

Die Werte der HTML-Formularfelder aus dem Formular erstellen gebunden sind durch so genannte einen Modellbinder für die ProductToCreate-Klasse. Der Standardmodellbinder Fügt eine Fehlermeldung zum Modellzustand automatisch beim Formularfeld nicht auf eine Formulareigenschaft gebunden werden kann.

Der Standardmodellbinder kann nicht die Zeichenfolge "Apple" der Preis-Eigenschaft der Product-Klasse gebunden werden. Sie können kein decimal-Eigenschaft eine Zeichenfolge zuweisen. Daher fügt der Modellbinder Fehler Modellzustand hinzu.

Der Standardmodellbinder kann nicht auch den Wert zuzuweisen ' Nothing ' eine Eigenschaft, die nicht dem Wert ' Nothing ' akzeptiert. Insbesondere kann nicht der Modellbinder den Wert zuweisen ' Nothing ' der Eigenschaft "UnitsInStock". Erneut, der Modellbinder aufgibt und Modellzustand eine Fehlermeldung hinzugefügt.

Wenn Sie die Darstellung dieser anpassen, prebinding Fehlermeldungen möchten müssen Sie Ressourcenzeichenfolgen für diese Nachrichten zu erstellen.

## <a name="summary"></a>Zusammenfassung

Das Ziel dieses Lernprogramms wurde um die grundlegende Funktionsweise der Validierung in ASP.NET MVC-Framework zu beschreiben. Sie haben gelernt, wie Modellzustand und die Validierung HTML-Hilfsmethoden verwenden. Wir besprochen haben auch die Unterscheidung zwischen prebinding und postbinding Überprüfung. In anderen Lernprogrammen besprechen wir verschiedene Strategien zum Verschieben von Validierungscodes aus Ihrem Domänencontroller und in Ihren Modellklassen.

>[!div class="step-by-step"]
[Zurück](displaying-a-table-of-database-data-vb.md)
[Weiter](validating-with-the-idataerrorinfo-interface-vb.md)
