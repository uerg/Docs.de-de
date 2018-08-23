---
uid: mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-vb
title: Die Überprüfung der Validierungssteuerelemente für Datenanmerkungen (VB) | Microsoft-Dokumentation
author: microsoft
description: Die Modellbindung des Daten-Anmerkung für die Validierung in ASP.NET MVC-Anwendungen nutzen. Erfahren Sie, wie die verschiedenen Typen von Validierungssteuerelement verwenden...
ms.author: riande
ms.date: 05/29/2009
ms.assetid: 0d23ff2b-f2ec-434a-be3b-1180beeccba3
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-vb
msc.type: authoredcontent
ms.openlocfilehash: 8159693adced7f102f6fe1457d7b103f8596d231
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831950"
---
<a name="validation-with-the-data-annotation-validators-vb"></a>Die Überprüfung der Validierungssteuerelemente für Datenanmerkungen (VB)
====================
durch [Microsoft](https://github.com/microsoft)

> Die Modellbindung des Daten-Anmerkung für die Validierung in ASP.NET MVC-Anwendungen nutzen. Erfahren Sie, wie die verschiedenen Typen von Validierungssteuerelement-Attribute und mit ihnen im Microsoft Entity Framework arbeiten.


In diesem Tutorial erfahren Sie, wie die Validierungssteuerelemente für die Datenanmerkung verwenden, für die Validierung in ASP.NET MVC-Anwendungen. Der Vorteil der Verwendung der Validierungssteuerelemente für die Datenanmerkung ist, dass Sie eine Überprüfung durchführen, indem Sie einfach ein oder mehrere Attribute – z. B. die erforderliche oder StringLength-Attribut hinzufügen – einer Klasseneigenschaft bieten.

Bevor Sie die Validierungssteuerelemente für die Datenanmerkung verwenden können, müssen Sie die Daten Anmerkungen Modellbindung herunterladen. Sie können Anmerkungen Modell Binder Datenbeispiel, das von der CodePlex-Website herunterladen, indem Sie auf [hier](http://aspnet.codeplex.com/Release/ProjectReleases.aspx?ReleaseId=24471).


Es ist wichtig zu verstehen, dass die Modellbindung für Daten Anmerkungen keine offizielle Microsoft ASP.NET MVC-Framework gehört. Obwohl die Modellbindung für Daten Anmerkungen von den Microsoft ASP.NET MVC-Team erstellt wurde, bietet Microsoft keine offizielle Microsoft-Support für die Modellbindung des Daten-Anmerkungen beschrieben und in diesem Tutorial verwendet.


## <a name="using-the-data-annotation-model-binder"></a>Mithilfe der Modellbindung für Daten-Anmerkung

Um die Daten Anmerkungen Modellbindung in ASP.NET MVC-Anwendungen zu verwenden, müssen Sie zuerst einen Verweis auf die Microsoft.Web.Mvc.DataAnnotations.dll-Assembly und die System.ComponentModel.DataAnnotations.dll-Assembly hinzufügen. Wählen Sie die Menüoption **-Projekt "," Verweis hinzufügen**. Klicken Sie dann auf die **Durchsuchen** Registerkarte, und navigieren Sie zum Speicherort, in dem Sie heruntergeladen (und entzippt) im Beispiel Daten Anmerkungen Modellbinder (finden Sie unter **Abbildung1**).

[![](validation-with-the-data-annotation-validators-vb/_static/image2.png)](validation-with-the-data-annotation-validators-vb/_static/image1.png)

**Abbildung 1**: Hinzufügen eines Verweises auf die Modellbindung für Data Annotations ([klicken Sie, um das Bild in voller Größe anzeigen](validation-with-the-data-annotation-validators-vb/_static/image3.png))

Wählen Sie zuerst die Microsoft.Web.Mvc.DataAnnotations.dll-Assembly und die System.ComponentModel.DataAnnotations.dll-Assembly, und klicken Sie auf die **OK** Schaltfläche.


Sie können keine die System.ComponentModel.DataAnnotations.dll-Assembly, die in .NET Framework Service Pack 1 mit der Modellbindung für Daten-Anmerkungen enthalten. Sie müssen die Version der im Beispiel für Daten Anmerkungen Binder-Download enthalten System.ComponentModel.DataAnnotations.dll Assembly verwenden.


Abschließend müssen Sie die Modellbindung "DataAnnotations" in der Datei "Global.asax" zu registrieren. Fügen Sie die folgende Zeile des Codes der Anwendung\_Start()-Ereignishandler so, dass die Anwendung\_Start()-Methode sieht wie folgt aus:

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample1.vb)]

Diese Codezeile wird der DataAnnotationsModelBinder als der Standardmodellbinder für die gesamte ASP.NET MVC-Anwendung registriert.

## <a name="using-the-data-annotation-validator-attributes"></a>Verwenden die Datenanmerkungsattribute-Validierungssteuerelement

Wenn Sie die Daten Anmerkungen Modellbindung verwenden, verwenden Sie die Validierungssteuerelement-Attribute für die Validierung. System.ComponentModel.DataAnnotations-Namespace enthält die folgenden Attribute für den systemintegritätsprüfungspunkt:

- Bereich – können Sie überprüfen, ob der Wert einer Eigenschaft zwischen einem angegebenen Bereich von Werten liegt.
- ReqularExpression – können Sie überprüfen, ob der Wert einer Eigenschaft mit einem angegebenen regulären Ausdruck-Muster übereinstimmt.
- Erforderlich: ermöglicht es Ihnen, eine Eigenschaft als erforderlich markieren.
- StringLength – können Sie eine maximale Länge für eine Zeichenfolgeneigenschaft angeben.
- Überprüfung: die Basisklasse für alle Validierungssteuerelement-Attribute.

> [!NOTE] 
> 
> Wenn bedarfsgerechte Überprüfung von einem standard-Validierungssteuerelemente nicht erfüllt werden müssen Sie immer die Möglichkeit, erstellen ein benutzerdefiniertes Validierungssteuerelement-Attribut durch ein neues Validierungssteuerelement-Attribut aus der Überprüfung Basisattribut erben.


Die Product-Klasse in **Codebeispiel 1** wird veranschaulicht, wie diese Attribute Validierungssteuerelement verwenden. Die Eigenschaften Name, Beschreibung und UnitPrice markiert werden nach Bedarf. Die Name-Eigenschaft müssen eine Länge der Zeichenfolge, die weniger als 10 Zeichen ist. Die UnitPrice-Eigenschaft muss schließlich Muster eines regulären Ausdrucks übereinstimmen, das einen Währungsbetrag darstellt.

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample2.vb)]

**Codebeispiel 1**: Models\Product.vb

Die Product-Klasse veranschaulicht, wie ein zusätzliches Attribut: das DisplayName-Attribut. Das DisplayName-Attribut können Sie den Namen der Eigenschaft ändern, wenn die Eigenschaft in einer Fehlermeldung angezeigt wird. Anstelle der Anzeige der Fehlermeldung "der UnitPrice-Feld ist erforderlich" können Sie anzeigen, der Fehlermeldung "das Preisfeld ist erforderlich".

> [!NOTE] 
> 
> Wenn Sie die Fehlermeldung wird angezeigt, die durch ein Validierungssteuerelement vollständig anpassen möchten, können Sie eine benutzerdefinierte Fehlermeldung, des Validierungssteuerelements "ErrorMessage"-Eigenschaft wie folgt zuweisen: `<Required(ErrorMessage:="This field needs a value!")>`


Können Sie die Product-Klasse in **Codebeispiel 1** mit Create() Controlleraktion in **Codebeispiel 2**. Diese Controlleraktion Ansicht "erstellen" wird erneut angezeigt, wenn Modellstatus alle Fehler enthält.

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample3.vb)]

**Codebeispiel 2**: Controllers\ProductController.vb

Schließlich können Sie die Ansicht erstellen **Codebeispiel 3** durch einen Rechtsklick auf die Aktion Create() und durch Auswählen der Menüoption **Ansicht hinzufügen**. Erstellen Sie eine stark typisierte Ansicht mit der Product-Klasse, wie die Model-Klasse. Wählen Sie **erstellen** aus der Dropdownliste der Ansicht Inhalt (finden Sie unter **abbildung2**).

[![](validation-with-the-data-annotation-validators-vb/_static/image5.png)](validation-with-the-data-annotation-validators-vb/_static/image4.png)

**Abbildung 2**: Hinzufügen der Ansicht "erstellen"

[!code-aspx[Main](validation-with-the-data-annotation-validators-vb/samples/sample4.aspx)]

**Codebeispiel 3**: Views\Product\Create.aspx

> [!NOTE] 
> 
> Entfernen Sie das Id-Feld von der erstellen-Form von generiert die **Ansicht hinzufügen** Option des Menüs. Da das Id-Feld eine Identity-Spalte entspricht, möchten keine Benutzer einen Wert für dieses Feld eingeben können.


Wenn Sie das Formular zum Erstellen eines Produkts senden und Sie werden keine Werte für die erforderlichen Felder eingeben, die Überprüfung von Fehlermeldungen, in **Abbildung 3** werden angezeigt.

[![](validation-with-the-data-annotation-validators-vb/_static/image7.png)](validation-with-the-data-annotation-validators-vb/_static/image6.png)

**Abbildung 3**: Fehlende erforderliche Felder

Wenn Sie eine ungültige Währungsbetrag, und klicken Sie dann auf die Fehlermeldung im eingeben **Abbildung 4** wird angezeigt.

[![](validation-with-the-data-annotation-validators-vb/_static/image9.png)](validation-with-the-data-annotation-validators-vb/_static/image8.png)

**Abbildung 4**: Ungültige Währungsbetrag

## <a name="using-data-annotation-validators-with-the-entity-framework"></a>Verwenden von Validierungssteuerelementen für Datenanmerkungen mit Entitätsframework

Wenn Sie Microsoft Entity Framework verwenden, um Ihre Data Model-Klassen zu generieren, können nicht Sie die Validierungssteuerelement-Attribute direkt auf Ihre Klassen anwenden. Da Entity Framework Designer der ViewModel-Klassen generiert, werden alle Änderungen, die Sie an die Modellklassen das nächste Mal überschrieben werden, die, das Sie im Designer Änderungen vornehmen.

Wenn Sie die Validierungssteuerelemente mit den Klassen generiert, die vom Entity Framework verwenden möchten, müssen Sie Meta Datenklassen erstellen. Sie haben die Validierungssteuerelemente der Meta-Data-Klasse anstelle der Validierungssteuerelemente auf die tatsächliche Klasse anwenden.

Beispiel: Angenommen, dass Sie eine Movie-Klasse, die mithilfe von Entity Framework erstellt haben (siehe **Abbildung 5**). Stellen Sie sich vor, darüber hinaus, dass Sie den Filmtitel und Director Eigenschaften erforderlichen Eigenschaften machen möchten. In diesem Fall können Sie erstellen die partielle Klasse und Meta-Data-Klasse in **Listing 4**.

[![](validation-with-the-data-annotation-validators-vb/_static/image11.png)](validation-with-the-data-annotation-validators-vb/_static/image10.png)

**Abbildung 5**: Movie-Klasse, die von Entity Framework generierten

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample5.vb)]

**Programmausdruck 4**: Models\Movie.vb

Die Datei im **Listing 4** enthält zwei Klassen namens "Film" und "MovieMetaData aus. Die Movie-Klasse ist eine partielle Klasse. Er entspricht der partiellen Klasse, die vom Entity Framework, das in der Datei DataModel.Designer.vb enthalten ist.

Derzeit werden teilweise Eigenschaften von .NET Framework nicht unterstützt. Daher besteht keine Möglichkeit, die Validierungssteuerelement-Attribute für die Movie-Klasse, die durch die Validierungssteuerelement-Attribute anwenden, auf die Movie-Klasse, die in der Datei definiert die Eigenschaften in der DataModel.Designer.vb-Datei definiert die Eigenschaften gelten **Listing 4**.

Beachten Sie, dass die partielle Movie-Klasse mit dem Attribut "metadatatype" versehen ist, der auf die Klasse MovieMetaData aus zeigt. Die MovieMetaData aus-Klasse enthält Eigenschaften von Netzwerkproxy für die Eigenschaften der Movie-Klasse.

Die Validierungssteuerelement-Attribute gelten für die Eigenschaften der Klasse MovieMetaData aus. Die Eigenschaften für Titel, Regisseur und DateReleased, die alle als erforderlichen Eigenschaften gekennzeichnet sind. Die Director-Eigenschaft muss eine Zeichenfolge zugewiesen werden, die weniger als 5 Zeichen enthält. Schließlich wird das DisplayName-Attribut angewendet, auf die DateReleased-Eigenschaft zum Anzeigen einer Fehlermeldung wie "das Feld Date freigegeben ist erforderlich". statt den Fehler ist"das Feld" DateReleased"erforderlich".

> [!NOTE] 
> 
> Beachten Sie, dass die Proxyeigenschaften in der Klasse MovieMetaData aus nicht dieselben Typen wie die entsprechenden Eigenschaften in die Movie-Klasse darstellen müssen. Beispielsweise ist die Director-Eigenschaft einer Zeichenfolgeneigenschaft in die Movie-Klasse und eine Eigenschaft des Objekts in der Klasse MovieMetaData aus.


Die Seite im **Abbildung 6** veranschaulicht die Fehlermeldungen zurückgegeben, wenn Sie ungültige Werte für die Movie-Eigenschaften eingeben.

[![](validation-with-the-data-annotation-validators-vb/_static/image13.png)](validation-with-the-data-annotation-validators-vb/_static/image12.png)

**Abbildung 6**: Entity Framework mit Validierungssteuerelementen ([klicken Sie, um das Bild in voller Größe anzeigen](validation-with-the-data-annotation-validators-vb/_static/image14.png))

## <a name="summary"></a>Zusammenfassung

In diesem Tutorial haben Sie gelernt, wie die Modellbindung des Daten-Anmerkung für die Validierung in ASP.NET MVC-Anwendungen nutzen. Sie haben gelernt, wie die verschiedenen Typen von Validierungssteuerelement-Attribute wie z. B. die erforderlichen und StringLength-Attribute verwendet. Sie haben zudem, wie Sie diese Attribute verwenden, bei der Arbeit mit Microsoft Entity Framework.

> [!div class="step-by-step"]
> [Vorherige](validating-with-a-service-layer-vb.md)
