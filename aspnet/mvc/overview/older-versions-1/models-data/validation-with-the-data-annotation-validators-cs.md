---
uid: mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-cs
title: Die Überprüfung der Daten Anmerkung Validierungssteuerelemente (c#) | Microsoft Docs
author: microsoft
description: Nutzen Sie die Daten Anmerkung Modellbinder validiert innerhalb einer ASP.NET MVC-Anwendung. Erfahren Sie, wie die verschiedenen Typen von Validierungssteuerelement verwendet...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/29/2009
ms.topic: article
ms.assetid: 7ca8013e-9dfc-4e33-8336-cdccfd5f9414
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-cs
msc.type: authoredcontent
ms.openlocfilehash: 0aca9472094e6a54c7b7cb4ad4f12df64fe12db2
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="validation-with-the-data-annotation-validators-c"></a>Die Überprüfung der Daten Anmerkung Validierungssteuerelemente (c#)
====================
durch [Microsoft](https://github.com/microsoft)

> Nutzen Sie die Daten Anmerkung Modellbinder validiert innerhalb einer ASP.NET MVC-Anwendung. Erfahren Sie, wie die verschiedenen Typen von Validierungssteuerelement-Attribute verwenden und mit ihnen arbeiten, im Microsoft Entity Framework.


In diesem Lernprogramm erfahren Sie, wie die Validierungssteuerelemente-Datenanmerkung die Validierung in ASP.NET MVC-Anwendung verwenden. Der Vorteil der Verwendung der Validierungssteuerelemente-Datenanmerkung ist die Möglichkeit, einfach durch Hinzufügen von einem oder mehreren Attributen – z. B. die erforderliche oder StringLength-Attribut – an Klasseneigenschaften-Objekt validiert.

Bevor Sie die Validierungssteuerelemente-Datenanmerkung verwenden können, müssen Sie den Daten Anmerkungen Modellbinder herunterladen. Sie können Anmerkungen Modell Binder Datenbeispiel von der CodePlex-Website herunterladen, indem Sie auf [hier](http://aspnet.codeplex.com/Release/ProjectReleases.aspx?ReleaseId=24471).


Es ist wichtig zu verstehen, dass die Daten Anmerkungen Modellbinder nicht offizieller Bestandteil des Microsoft ASP.NET MVC-Framework ist. Obwohl die Daten Anmerkungen Modellbinder vom Microsoft ASP.NET MVC-Team erstellt wurde, bietet Microsoft keine offizielle Microsoft-Support für den Modellbinder für Daten Anmerkungen beschrieben und in diesem Lernprogramm verwendet.


## <a name="using-the-data-annotation-model-binder"></a>Mithilfe der Modellbindung des Daten-Anmerkung

Um die Daten Anmerkungen Modellbinder in einer ASP.NET MVC-Anwendung verwenden zu können, müssen Sie zunächst einen Verweis auf die Microsoft.Web.Mvc.DataAnnotations.dll-Assembly und die System.ComponentModel.DataAnnotations.dll-Assembly hinzufügen. Wählen Sie die Menüoption **Projekt "," Verweis hinzufügen**. Klicken Sie anschließend auf die **Durchsuchen** Registerkarte, und navigieren Sie zum Speicherort, in dem heruntergeladen (und entzippt) von Daten Anmerkungen Modellbinder-Beispiel (finden Sie unter **Abbildung 1**).

[![](validation-with-the-data-annotation-validators-cs/_static/image2.png)](validation-with-the-data-annotation-validators-cs/_static/image1.png)

**Abbildung 1**: Hinzufügen eines Verweises auf den Daten Anmerkungen Modellbinder ([klicken Sie hier, um das Bild in voller Größe angezeigt](validation-with-the-data-annotation-validators-cs/_static/image3.png))

Wählen Sie die Microsoft.Web.Mvc.DataAnnotations.dll-Assembly und die System.ComponentModel.DataAnnotations.dll-Assembly, und klicken Sie auf die **OK** Schaltfläche.


Sie können keine System.ComponentModel.DataAnnotations.dll-Assembly, die mit .NET Framework Service Pack 1 mit den Daten Anmerkungen Modellbinder enthalten. Sie müssen die Version der Assembly System.ComponentModel.DataAnnotations.dll im Anmerkungen Modell Binder Datenbeispiel Download verwenden.


Schließlich müssen Sie die DataAnnotations-Modellbindung in der Datei "Global.asax" zu registrieren. Fügen Sie die folgende Zeile des Codes der Anwendung\_Start()-Ereignishandler, damit die Anwendung\_Methode Start() sieht wie folgt:

[!code-csharp[Main](validation-with-the-data-annotation-validators-cs/samples/sample1.cs)]

Diese Codezeile registriert die AtaAnnotationsModelBinder als den Standardmodellbinder für die gesamte ASP.NET MVC-Anwendung.

## <a name="using-the-data-annotation-validator-attributes"></a>Hierbei werden die Daten Anmerkung Validierungssteuerelement-Attribute

Wenn Sie den Modellbinder für Daten Anmerkungen verwenden, verwenden Sie die Validierungssteuerelement Attribute validiert. System.ComponentModel.DataAnnotations-Namespace enthält die folgenden Validierungssteuerelement-Attribute:

- Im Bereich – können Sie überprüfen, ob der Wert einer Eigenschaft zwischen einem angegebenen Bereich von Werten liegt.
- Der reguläre Ausdruck – können Sie überprüfen, ob der Wert einer Eigenschaft ein angegebenes reguläres Ausdrucksmuster übereinstimmt.
- Erforderlich – ermöglicht es Ihnen, eine Eigenschaft als erforderlich markieren.
- StringLength – ermöglicht Ihnen die Angabe eine maximale Länge für eine Zeichenfolgeneigenschaft.
- Überprüfung: die Basisklasse für alle Validierungssteuerelement-Attribute.

> [!NOTE] 
> 
> Wenn Ihre Anforderungen für die Validierung mithilfe einer standardmäßigen Validierungssteuerelemente nicht erfüllt werden müssen Sie immer die Möglichkeit, erstellen ein benutzerdefiniertes Validierungssteuerelement-Attribut durch ein neues Validierungssteuerelement-Attribut aus der Überprüfung Basisattribut erben.


Die Produktklasse in **Codebeispiel 1** wird veranschaulicht, wie diese Attribute Validierungssteuerelement verwenden. Die Eigenschaften Name, Beschreibung und UnitPrice gekennzeichnet werden nach Bedarf. Die Name-Eigenschaft muss ein String-length-Funktion haben, die weniger als 10 Zeichen lang ist. Die UnitPrice-Eigenschaft muss schließlich Muster eines regulären Ausdrucks übereinstimmen, das einen Währungsbetrag darstellt.

[!code-csharp[Main](validation-with-the-data-annotation-validators-cs/samples/sample2.cs)]

**Auflisten von 1**: Models\Product.cs

Die Produktklasse zeigt, wie ein zusätzliches Attribut: das DisplayName-Attribut. Das DisplayName-Attribut können Sie den Namen der Eigenschaft ändern, wenn die Eigenschaft in einer Fehlermeldung angezeigt wird. Anstelle der Anzeige der Fehlermeldung "das UnitPrice-Feld ist erforderlich" können Sie anzeigen, der Fehlermeldung "das Price-Feld ist erforderlich".

> [!NOTE] 
> 
> Wenn die Fehlermeldung angezeigt, indem ein Validator vollständig angepasst werden soll, können Sie eine benutzerdefinierte Fehlermeldung an das Validierungssteuerelement "ErrorMessage"-Eigenschaft wie folgt zuweisen: `<Required(ErrorMessage:="This field needs a value!")>`


Können Sie die Produktklasse in **Codebeispiel 1** mit Create()-Controlleraktion in **auflisten 2**. Diese Controlleraktion erneut die Erstellungsansicht an, wenn Modellstatus alle Fehler enthält.

[!code-csharp[Main](validation-with-the-data-annotation-validators-cs/samples/sample3.cs)]

**Auflisten von 2**: Controllers\ProductController.vb

Schließlich erstellen Sie die Ansicht im **auflisten 3** durch die Create()--Aktion mit der rechten Maustaste, und wählen die Menüoption **Ansicht hinzufügen**. Erstellen Sie eine stark typisierte Ansicht mit der Produkt-Klasse, wie die Modellklasse. Wählen Sie **erstellen** aus der Dropdownliste der Ansicht Inhalt (finden Sie unter **Abbildung 2**).

[![](validation-with-the-data-annotation-validators-cs/_static/image5.png)](validation-with-the-data-annotation-validators-cs/_static/image4.png)

**Abbildung 2**: die Erstellungsansicht hinzufügen

[!code-aspx[Main](validation-with-the-data-annotation-validators-cs/samples/sample4.aspx)]

**Auflisten von 3**: Views\Product\Create.aspx

> [!NOTE] 
> 
> Entfernen Sie das Feld "Id" aus dem Formular erstellen, die generiert werden, indem Sie die **Ansicht hinzufügen** Option des Menüs. Da das Feld "Id" eine Identity-Spalte entspricht, möchten Sie Benutzern ermöglichen, einen Wert für dieses Feld einzugeben.


Wenn Sie das Formular für das Erstellen eines Produkts übermitteln und Werte für die erforderlichen Felder eingeben, der Validierungsfehler Nachrichten **Abbildung 3** werden angezeigt.

[![](validation-with-the-data-annotation-validators-cs/_static/image7.png)](validation-with-the-data-annotation-validators-cs/_static/image6.png)

**Abbildung 3**: Pflichtfelder fehlt

Bei der Eingabe eine ungültige Währungsbetrag, und klicken Sie dann auf die Fehlermeldung in **Abbildung 4** wird angezeigt.

[![](validation-with-the-data-annotation-validators-cs/_static/image9.png)](validation-with-the-data-annotation-validators-cs/_static/image8.png)

**Abbildung 4**: Ungültige Währungsbetrag

## <a name="using-data-annotation-validators-with-the-entity-framework"></a>Verwenden von Daten Anmerkung Validierungssteuerelemente mit Entity Framework

Wenn Sie Microsoft Entity Framework verwenden, um Ihren Modellklassen Daten generieren können nicht Sie das Validierungssteuerelement Attribute direkt in Ihren Klassen anwenden. Da Entity Framework-Designer die Modellklassen generiert werden, werden alle vorgenommenen Änderungen an der Modellklassen das nächste Mal überschrieben werden vorgenommenen Änderungen im Designer.

Wenn Sie mit den vom Entity Framework generierten Klassen die Validierungssteuerelemente verwenden möchten, müssen Sie Meta-Datenklassen zu erstellen. Sie gelten die Validierungssteuerelemente für Meta Datenklasse statt der Validierungssteuerelemente auf die tatsächliche Klasse anzuwenden.

Angenommen, dass Sie eine Film-Klasse, die mit dem Entity Framework erstellt haben (siehe **Abbildung 5**). Angenommen Sie, darüber hinaus, dass der Filmtitel und Director erforderlichen Eigenschaften machen möchten. In diesem Fall können Sie erstellen die partielle Klasse und die Datenklasse in Meta **auflisten 4**.

[![](validation-with-the-data-annotation-validators-cs/_static/image11.png)](validation-with-the-data-annotation-validators-cs/_static/image10.png)

**Abbildung 5**: Film-Klasse, die vom Entity Framework generierten

[!code-csharp[Main](validation-with-the-data-annotation-validators-cs/samples/sample5.cs)]

**Auflisten von 4**: Models\Movie.cs

Die Datei im **auflisten 4** enthält zwei Klassen, die mit dem Namen Film und MovieMetaData aus. Die Film-Klasse ist eine partielle Klasse. Er entspricht der partiellen Klasse generiert, die vom Entity Framework, die in der Datei DataModel.Designer.vb enthalten ist.

Derzeit werden teilweise Eigenschaften von .NET Framework nicht unterstützt. Daher besteht keine Möglichkeit, auf die Eigenschaften der Film-Klasse, die in der Datei DataModel.Designer.vb definiert, durch Anwenden der Validierungssteuerelement-Attribute auf die Eigenschaften der Film-Klasse, die in der Datei definiert die Validierungssteuerelement-Attribute gelten **auflisten 4**.

Beachten Sie, dass die Teilklasse Film mit einem MetadataType-Attribut ergänzt wird, der auf die Klasse MovieMetaData aus zeigt. Die MovieMetaData aus-Klasse enthält Eigenschaften von Netzwerkproxy für die Eigenschaften der Film-Klasse.

Die Validierungssteuerelement-Attribute werden den Eigenschaften der Klasse MovieMetaData aus angewendet. Die Eigenschaften für Titel, Director und DateReleased, die alle als erforderlichen Eigenschaften gekennzeichnet sind. Die Director-Eigenschaft muss eine Zeichenfolge zugewiesen werden, die weniger als 5 Zeichen enthält. Schließlich das DisplayName-Attribut angewendet wird, und die DateReleased-Eigenschaft zeigt eine Fehlermeldung wie "das Feld Date freigegeben ist erforderlich." statt den Fehler ist"das Feld DateReleased erforderlich."

> [!NOTE] 
> 
> Beachten Sie, dass die Proxyeigenschaften in der Klasse MovieMetaData aus nicht benötigen, um dieselben Typen wie die entsprechenden Eigenschaften in der Klasse Film darzustellen. Beispielsweise ist die Director-Eigenschaft eine Zeichenfolgeneigenschaft, in der Film-Klasse und eine Eigenschaft des Objekts in der Klasse MovieMetaData aus.


Die Seite in **Abbildung 6** veranschaulicht die Fehlermeldungen zurückgegeben, wenn Sie ungültige Werte für die Film-Eigenschaften eingeben.

[![](validation-with-the-data-annotation-validators-cs/_static/image13.png)](validation-with-the-data-annotation-validators-cs/_static/image12.png)

**Abbildung 6**: Entity Framework mit Validierungssteuerelemente ([klicken Sie hier, um das Bild in voller Größe angezeigt](validation-with-the-data-annotation-validators-cs/_static/image14.png))

## <a name="summary"></a>Zusammenfassung

In diesem Lernprogramm haben Sie gelernt, wie die Daten Anmerkung Modellbinder validiert innerhalb einer ASP.NET MVC-Anwendung nutzen. Sie haben gelernt, wie die verschiedenen Typen von Validierungssteuerelement-Attribute, z. B. die erforderlichen und StringLength-Attribute verwendet. Außerdem haben Sie gelernt, diese Attribute bei der Arbeit mit Microsoft Entity Framework nicht verwenden.

> [!div class="step-by-step"]
> [Zurück](validating-with-a-service-layer-cs.md)
> [Weiter](creating-model-classes-with-the-entity-framework-vb.md)
