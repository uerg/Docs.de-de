---
uid: mvc/overview/older-versions-1/models-data/performing-simple-validation-vb
title: Ausführen einer einfachen Überprüfung (VB) | Microsoft-Dokumentation
author: StephenWalther
description: Erfahren Sie, wie die Validierung in ASP.NET MVC-Anwendungen. In diesem Tutorial führt Stephen Walther Sie Modellstatus und den überprüfungshelfer HTML...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: df6cf4b7-0bb3-4c4e-b17a-bd78a759a6bc
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/models-data/performing-simple-validation-vb
msc.type: authoredcontent
ms.openlocfilehash: 26900229630b25affe21a5bb801ef0247711d26b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399460"
---
<a name="performing-simple-validation-vb"></a>Ausführen einer einfachen Überprüfung (VB)
====================
durch [Stephen Walther](https://github.com/StephenWalther)

> Erfahren Sie, wie die Validierung in ASP.NET MVC-Anwendungen. In diesem Tutorial führt Stephen Walther Sie Modellzustand sowie die Validierung, HTML-Hilfsprogramme.


Das Ziel in diesem Tutorial wird beschrieben, wie Sie die Validierung in ASP.NET MVC-Anwendungen ausführen können. Beispielsweise erfahren Sie, wie Sie verhindern, dass eine Person senden eines Formulars, das nicht über einen Wert für ein erforderliches Feld enthält. Erfahren Sie, wie Sie mit der Modellzustand sowie die Überprüfung-HTML-Hilfsprogramme.

## <a name="understanding-model-state"></a>Grundlegendes zu Modellstatus

Können Sie Modellstatus – oder genauer gesagt, das Modellzustandswörterbuch - Validierungsfehler darstellen. Die Aktion Create() in Codebeispiel 1 überprüft beispielsweise die Eigenschaften der einer Produktklasse vor dem Hinzufügen der Product-Klasse zu einer Datenbank an.


Ich bin nicht empfehlen, dass Sie Ihre Logik zum Überprüfen oder die Datenbank auf einen Controller hinzufügen. Ein Controller sollte nur im Zusammenhang mit der Steuerung des Anwendungsflusses Logik enthalten. Wir werden eine Verknüpfung mit der Einfachheit halber dauert.


**1 – Controllers\ProductController.vb auflisten**

[!code-vb[Main](performing-simple-validation-vb/samples/sample1.vb)]

Im Codebeispiel 1 werden die Eigenschaften Name, Beschreibung und UnitsInStock der Product-Klasse überprüft. Wenn eine dieser Eigenschaften einen Überprüfungstest fehlschlagen wird ein Fehler auf das Modellzustandswörterbuch (dargestellt durch die ModelState-Eigenschaft der Controllerklasse) hinzugefügt.

Wenn Fehler vorhanden, im modellzustands sind gibt die Eigenschaft ModelState.IsValid "false" zurück. In diesem Fall wird das HTML-Formular zum Erstellen eines neuen Produkts erneut angezeigt. Andernfalls treten keine Validierungsfehler mehr auftreten, wird das neue Produkt mit der Datenbank hinzugefügt.

## <a name="using-the-validation-helpers"></a>Verwenden die Überprüfung-Hilfsprogramme

ASP.NET MVC-Framework enthält zwei Hilfsmethoden für Überprüfung: das Html.ValidationMessage()-Hilfsobjekt und der Html.ValidationSummary()-Hilfsprogramm. Verwenden Sie dieses beiden Hilfsprogramme in einer Ansicht, um Überprüfungsfehlermeldungen anzuzeigen.

Die Hilfsprogramme Html.ValidationMessage() und Html.ValidationSummary() werden in den Bearbeitungs- und änderungsansichten verwendet, die von der ASP.NET-MVC-Gerüstbau automatisch generiert werden. Um die Ansicht "erstellen" zu generieren, gehen Sie wie folgt vor:

1. Mit der rechten Maustaste der Create()-Aktion in der Product-Controller, und wählen Sie die Menüoption **Ansicht hinzufügen** (siehe Abbildung 1).
2. In der **Ansicht hinzufügen** Dialogfeld aktivieren Sie das Kontrollkästchen mit der Bezeichnung **eine stark typisierte Ansicht erstellen** (siehe Abbildung 2).
3. Von der **Datenklasse anzeigen** Dropdownliste wählen Sie die Product-Klasse.
4. Von der **Inhalt anzeigen** Dropdown-Liste wählen erstellen.
5. Klicken Sie auf die Schaltfläche **Hinzufügen**.


Stellen Sie sicher, dass Sie Ihre Anwendung vor dem Hinzufügen einer Ansicht zu erstellen. Andernfalls nicht die Liste der Klassen angezeigt, der **Datenklasse anzeigen** Dropdown-Liste.


[![Das Dialogfeld "Neues Projekt"](performing-simple-validation-vb/_static/image1.jpg)](performing-simple-validation-vb/_static/image1.png)

**Abbildung 01**: Hinzufügen einer Ansicht ([klicken Sie, um das Bild in voller Größe anzeigen](performing-simple-validation-vb/_static/image2.png))


[![Das Dialogfeld "Neues Projekt"](performing-simple-validation-vb/_static/image2.jpg)](performing-simple-validation-vb/_static/image3.png)

**Abbildung 02**: Erstellen einer stark typisierten Ansicht ([klicken Sie, um das Bild in voller Größe anzeigen](performing-simple-validation-vb/_static/image4.png))


Nachdem Sie diese Schritte abgeschlossen haben, erhalten Sie die Ansicht "erstellen" in Liste 2.

**Codebeispiel 2 - Views\Product\Create.aspx**

[!code-aspx[Main](performing-simple-validation-vb/samples/sample2.aspx)]

Programmausdruck 2 wird das Hilfsprogramm Html.ValidationSummary() sofort über das HTML-Formular aufgerufen. Dieses Hilfsprogramm wird verwendet, um eine Liste der Überprüfungsfehlermeldungen anzuzeigen. Das Hilfsprogramm Html.ValidationSummary() rendert die Fehler in einer Liste mit Aufzählungszeichen.

Das Hilfsprogramm Html.ValidationMessage() wird neben jeder HTML-Formular Felder bezeichnet. Dieses Hilfsprogramm wird verwendet, eine Fehlermeldung, die rechts neben einem Formularfeld angezeigt. Fall 2 auflisten werden die Html.ValidationMessage()-Hilfsprogramm ein Sternchen angezeigt, wenn ein Fehler auftritt.

Die Seite in Abbildung 3 zeigt die Fehlermeldungen, die durch die Überprüfung-Hilfsprogramme gerendert wird, wenn das Formular, fehlende Felder mit ungültigen Werten übermittelt wird.


[![Das Dialogfeld "Neues Projekt"](performing-simple-validation-vb/_static/image3.jpg)](performing-simple-validation-vb/_static/image5.png)

**Abbildung 03**: Erstellen Sie die Ansicht mit Problemen übermittelt ([klicken Sie, um das Bild in voller Größe anzeigen](performing-simple-validation-vb/_static/image6.png))


Beachten Sie, dass die Darstellung des HTML-Codes die Eingabe-Felder auch geändert werden, wenn ein Überprüfungsfehler vorliegt. Die Html.TextBox() Helper rendert eine *Klasse = "Eingabe-Validation-Error"* -Attribut, wenn ein Überprüfungsfehler vorliegt, die mit der Eigenschaft, die vom Hilfsprogramm Html.TextBox() gerendert verknüpft ist.

Es gibt drei Klassen cascading Stylesheet verwendet, um die Darstellung von Validierungsfehlern zu steuern:

- Eingabe-Überprüfungsfehlern - angewendet wird, um die &lt;Eingabe&gt; Tag von Html.TextBox() Helper gerendert.
- Feld-Überprüfungsfehlern - angewendet wird, um die &lt;umfassen&gt; Tag, die vom Hilfsprogramm Html.ValidationMessage() gerendert.
- Summary-Validierungsfehler: angewendet wird, um die &lt;Ul&gt; Tag, die vom Hilfsprogramm Html.ValidationSumamry() gerendert.

Sie können diese Klassen für cascading Stylesheet ändern und die Darstellung der Fehler bei der Validierung, daher durch Ändern der Datei "Site.CSS", die im Ordner "Content" ändern.

> [!NOTE] 
> 
> HtmlHelper-Klasse enthält schreibgeschützte statische Eigenschaften Abrufen der Namen der Überprüfung CSS zu verwandten Klassen. Diese statische Eigenschaften werden ValidationInputCssClassName ValidationFieldCssClassName und ValidationSummaryCssClassName benannt.


## <a name="prebinding-validation-and-postbinding-validation"></a>Prebinding Validierung und Überprüfung der Postbinding

Wenn Sie das HTML-Formular zum Erstellen eines Produkts senden und Sie einen ungültigen Wert für Feld für den Preis und keinen Wert für das UnitsInStock-Feld eingeben, klicken Sie dann die validierungsmeldungen werden angezeigt, die in Abbildung 4 erhalten Sie. Woher kommen diese Überprüfungsfehlermeldungen?


[![Das Dialogfeld "Neues Projekt"](performing-simple-validation-vb/_static/image4.jpg)](performing-simple-validation-vb/_static/image7.png)

**Abbildung 04**: Validierungsfehler Prebinding ([klicken Sie, um das Bild in voller Größe anzeigen](performing-simple-validation-vb/_static/image8.png))


Es gibt tatsächlich zwei Arten von Meldungen für Validierungsfehler: die generierten werden, bevor die HTML-Formular-Felder zu einer Klasse gebunden sind, und die generiert werden, nachdem die Formularfelder auf die Klasse gebunden sind. Das heißt, es sind Überprüfungsfehler prebinding und postbinding Validierungsfehler.

Die Create()-Aktion, die von der Produkt-Controller in Codebeispiel 1 verfügbar gemacht werden akzeptiert eine Instanz der Product-Klasse. Die Signatur der Create-Methode sieht folgendermaßen aus:

[!code-vb[Main](performing-simple-validation-vb/samples/sample3.vb)]

Die Werte der HTML-Formularfelder aus dem Formular erstellen, sind der ProductToCreate-Klasse einen Modellbinder vom gebunden. Der Standardmodellbinder hinzufügt, eine Fehlermeldung Modellstatus automatisch Wenn ein Formularfeld nicht auf eine Formulareigenschaft gebunden werden kann.

Die Zeichenfolge "Apple" kann nicht von der Standardmodellbinder der Preis-Eigenschaft der Product-Klasse gebunden werden. Sie können keinen decimal-Eigenschaft eine Zeichenfolge zuweisen. Daher fügt die modellbindung einen Fehler Modellzustand hinzu.

Der Standardmodellbinder darf nicht auch den Wert zuweisen "Nothing" einer Eigenschaft, die nicht dem Wert "Nothing" akzeptiert. Insbesondere kann die modellbindung dem Wert "Nothing" die UnitsInStock keiner Eigenschaft zugeordnet. Auch hier die modellbindung gibt und Modellzustand eine Fehlermeldung hinzugefügt.

Wenn Sie die Darstellung dieser anpassen, prebinding Fehlermeldungen möchten müssen Sie Ressourcenzeichenfolgen für diese Nachrichten zu erstellen.

## <a name="summary"></a>Zusammenfassung

Das Ziel in diesem Tutorial wurde die grundlegende Funktionsweise der Validierung in ASP.NET MVC-Framework beschrieben. Sie haben gelernt, wie Modellzustand sowie die Überprüfung-HTML-Hilfsprogramme verwenden. Erläutert auch die Unterscheidung zwischen prebinding und postbinding Überprüfung. In anderen Lernprogrammen besprechen wir verschiedene Strategien für Ihr Validierungscode aus Ihren Controllern und in Ihren Modellklassen verschieben.

> [!div class="step-by-step"]
> [Zurück](displaying-a-table-of-database-data-vb.md)
> [Weiter](validating-with-the-idataerrorinfo-interface-vb.md)
