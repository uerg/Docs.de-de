---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
title: Verwenden des HTML5 und jQuery UI Datepicker-Popupkalenders mit ASP.NET MVC – Teil 3 | Microsoft Docs
author: Rick-Anderson
description: In diesem Lernprogramm erfahren Sie die Grundlagen der Arbeit mit Editorvorlagen Anzeigevorlagen und dem jQuery UI Datepicker-Popupkalenders in einer ASP.NET MV...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/29/2011
ms.topic: article
ms.assetid: 8f5f91ae-12d7-4cf3-ac09-4bb53d07ee60
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
msc.type: authoredcontent
ms.openlocfilehash: fd1ae746f4f134b779c7eee50cf6c840bbb7068e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-3"></a>Verwenden des HTML5 und jQuery UI Datepicker-Popupkalenders mit ASP.NET MVC – Teil 3
====================
durch [Rick Anderson](https://github.com/Rick-Anderson)

> In diesem Lernprogramm erfahren Sie die Grundlagen der Arbeit mit Editorvorlagen Anzeigevorlagen und jQuery UI Datepicker-Popupkalenders in einer ASP.NET MVC-Webanwendung.


## <a name="working-with-complex-types"></a>Arbeiten mit komplexen Typen

In diesem Abschnitt Sie erstellen eine Adressklasse und erfahren Sie, wie beim Erstellen einer Vorlage aus, um es anzuzeigen.

In der *Modelle* Ordner, erstellen Sie eine neue Klassendatei mit dem Namen *Person.cs* dort legen Sie zwei Typen: ein `Person` Klasse und ein `Address` Klasse. Die `Person` Klasse enthält eine Eigenschaft, die als typisiert ist `Address`. Die `Address` Typ ist ein komplexer Typ, d. h., es ist keines der integrierten Typen wie `int`, `string`, oder `double`. Stattdessen hat es mehrere Eigenschaften. Der Code für den neuen Klassen sieht wie folgt:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample1.cs)]

In der `Movie` Controller, fügen Sie die folgenden `PersonDetail` Aktion aus, um eine Personeninstanz angezeigt:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample2.cs)]

Fügen Sie folgenden Code, der `Movie` Controller zum Auffüllen der `Person` Modell mit Beispieldaten:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample3.cs)]

Öffnen der *Views\Movies\PersonDetail.cshtml* Datei, und fügen Sie das folgende Markup für die `PersonDetail` anzeigen.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample4.cshtml)]

Drücken Sie STRG + F5, um die Anwendung auszuführen, und navigieren zu *Filme/PersonDetail*.

Die `PersonDetail` Ansicht enthält nicht die `Address` komplexer Typ, wie Sie sehen in diesem Screenshot. (Es ist keine Adresse dargestellt.)

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image1.png)

Die `Address` Modelldaten werden nicht angezeigt werden, da es sich um einen komplexen Typ handelt. Um die Adressinformationen anzuzeigen, öffnen die *Views\Movies\PersonDetail.cshtml* Datei erneut, und fügen Sie das folgende Markup hinzu.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample5.cshtml)]

Das vollständige Markup für die `PersonDetail` jetzt Ansicht wie folgt aussieht:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample6.cshtml)]

Die Anwendung erneut ausführen und Anzeigen der `PersonDetail` anzeigen. Die Adressinformationen werden nun angezeigt:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image2.png)

### <a name="creating-a-template-for-a-complex-type"></a>Erstellen einer Vorlage für einen komplexen Typ

In diesem Abschnitt erstellen Sie eine Vorlage, die verwendet werden soll, zum Rendern der `Address` komplexen Typ. Beim Erstellen einer Vorlage für die `Address` Typ ASP.NET-MVC automatisch verwenden, um eine Adressmodell an einer beliebigen Stelle in der Anwendung zu formatieren. Dies bietet Ihnen eine Möglichkeit zum Steuern des Renderns von der `Address` Typ von nur einem Ort in der Anwendung.

In der *Views\Shared\DisplayTemplates* Ordner, erstellen Sie eine stark typisierte Teilansicht mit dem Namen **Adresse**:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image3.png)

Klicken Sie auf **hinzufügen**, und öffnen Sie die neue *Views\Shared\DisplayTemplates\Address.cshtml* Datei. Die neue Ansicht enthält die folgenden generierten Markup:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample7.cshtml)]

Führen Sie die Anwendung und Anzeigen der `PersonDetail` anzeigen. Dieses Mal die `Address` die neu erstellte Vorlage dient zum Anzeigen der `Address` komplexer Typ, damit die Anzeige wie folgt aussieht:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image4.png)

### <a name="summary-ways-to-specify-the-model-display-format-and-template"></a>Zusammenfassung: Möglichkeiten zum Angeben der Modell-Anzeigeformat und Vorlage

Sie haben gesehen, dass das Format oder die Vorlage für eine Modelleigenschaft angeben können, legen Sie mithilfe der folgenden Vorgehensweisen:

- Anwenden der `DisplayFormat` -Attribut auf eine Eigenschaft im Modell. Beispielsweise verursacht der folgende Code das Datum ohne Uhrzeit angezeigt werden:

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample8.cs)]
- Anwenden einer [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) -Attribut auf eine Eigenschaft im Modell und die Angabe des Datentyps. Beispielsweise verursacht der folgende Code das Datum ohne Uhrzeit angezeigt werden.

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample9.cs)]

    Wenn die Anwendung enthält eine *date.cshtml* Vorlage in der *Views\Shared\DisplayTemplates* Ordner oder die *Views\Movies\DisplayTemplates* Ordner, die Vorlage wird verwendet, um das Rendern der `DateTime` Eigenschaft. Andernfalls zeigt die integrierten ASP.NET Datenvorlagen System die Eigenschaft als Datum.
- Erstellen einer Anzeigevorlage für die in der *Views\Shared\DisplayTemplates* Ordner oder die *Views\Movies\DisplayTemplates* Ordner, deren Namen übereinstimmen, den Datentyp, die Sie formatieren möchten. Beispielsweise haben Sie gesehen, die die *Views\Shared\DisplayTemplates\DateTime.cshtml* wurde zum Rendern verwendet `DateTime` Eigenschaften in einem Modell, ohne das Hinzufügen eines Attributs für das Modell und Ansichten Markup hinzugefügt.
- Mithilfe der [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) Attribut für das Modell auf die Vorlage zum Anzeigen der Modelleigenschaft angeben.
- Das explizite Hinzufügen der Anzeigename für die Vorlage, die [Html.DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) in einer Ansicht aufrufen.

Der Ansatz, den Sie verwenden, hängt davon ab, welche Schritte Sie in der Anwendung ausführen müssen. Es ist nicht ungewöhnlich, dass zum Mischen von dieser Ansätze, um genau die Art der Formatierung zu erhalten, die Sie benötigen.

Wechseln Sie im nächsten Abschnitt etwas TheBeerHouse und Verschieben von anpassen, wie Daten angezeigt werden, um anpassen, wie er eingegeben wird. Sie müssen die jQuery-Datepicker, die den Edit-Ansichten in der Anwendung um benutzerdefinierbaren Möglichkeit zum Angeben von Datumsangaben verknüpft.

> [!div class="step-by-step"]
> [Zurück](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
> [Weiter](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4.md)
