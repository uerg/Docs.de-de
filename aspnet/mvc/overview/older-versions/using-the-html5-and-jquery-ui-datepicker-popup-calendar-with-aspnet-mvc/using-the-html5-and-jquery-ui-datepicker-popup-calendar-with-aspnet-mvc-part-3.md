---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
title: Verwenden des HTML5 und jQuery UI Datepicker-Popupkalenders mit ASP.NET MVC – Teil 3 | Microsoft-Dokumentation
author: Rick-Anderson
description: In diesem Tutorial lernen Sie die Grundlagen der Arbeit mit Editor-Vorlagen, Anzeigevorlagen und dem jQuery UI Datepicker-Popupkalenders in einer ASP.NET MV...
ms.author: riande
ms.date: 08/29/2011
ms.assetid: 8f5f91ae-12d7-4cf3-ac09-4bb53d07ee60
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
msc.type: authoredcontent
ms.openlocfilehash: baaca74d584a6d5028824e877c561494cdff044e
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577222"
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-3"></a>Verwenden des HTML5 und jQuery UI Datepicker-Popupkalenders mit ASP.NET MVC – Teil 3
====================
durch [Rick Anderson]((https://twitter.com/RickAndMSFT))

> In diesem Tutorial lernen Sie die Grundlagen der Arbeit mit Editor-Vorlagen, Anzeigevorlagen und dem jQuery UI Datepicker-Popupkalenders in einer ASP.NET MVC-Webanwendung.


## <a name="working-with-complex-types"></a>Arbeiten mit komplexen Typen

In diesem Abschnitt Sie erstellen Sie eine Adressklasse und erfahren Sie, wie zum Erstellen einer Vorlage aus, um es anzuzeigen.

In der *Modelle* Ordner, erstellen Sie eine neue Klassendatei mit dem Namen *Person.cs* , fügen Sie zwei Typen: ein `Person` Klasse und ein `Address` Klasse. Die `Person` Klasse enthält eine Eigenschaft, die als typisiert ist `Address`. Die `Address` Typ ist ein komplexer Typ, d. h., es ist keiner der integrierten Typen wie `int`, `string`, oder `double`. Stattdessen verfügt er über mehrere Eigenschaften. Der Code für die neuen Klassen sieht folgendermaßen aus:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample1.cs)]

In der `Movie` Controller, fügen Sie die folgenden `PersonDetail` Aktion aus, um eine Personeninstanz angezeigt:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample2.cs)]

Klicken Sie dann fügen Sie den folgenden Code der `Movie` Controller zum Auffüllen der `Person` Modell mit einigen Beispieldaten:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample3.cs)]

Öffnen der *Views\Movies\PersonDetail.cshtml* Datei, und fügen Sie das folgende Markup für die `PersonDetail` anzeigen.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample4.cshtml)]

Drücken Sie STRG + F5, um die Anwendung auszuführen, und navigieren zu *Filme/PersonDetail*.

Die `PersonDetail` Ansicht enthält nicht die `Address` komplexer Typ sein, wie Sie sehen in diesem Screenshot. (Es ist keine Adresse angezeigt.)

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image1.png)

Die `Address` Modellieren von Daten werden nicht angezeigt werden, da es sich um einen komplexen Typ handelt. Um die Informationen zur Adresse anzuzeigen, öffnen Sie die *Views\Movies\PersonDetail.cshtml* -Datei erneut, und fügen Sie das folgende Markup hinzu.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample5.cshtml)]

Das vollständige Markup für die `PersonDetail` nun Ansicht folgendermaßen aussieht:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample6.cshtml)]

Die Anwendung erneut ausführen und Anzeigen der `PersonDetail` anzeigen. Die Adressinformationen wird jetzt angezeigt werden:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image2.png)

### <a name="creating-a-template-for-a-complex-type"></a>Erstellen einer Vorlage für einen komplexen Typ

In diesem Abschnitt erstellen Sie eine Vorlage, die zum Rendern verwendet werden wird die `Address` komplexen Typ. Wenn Sie eine Vorlage zum Erstellen der `Address` Typ ASP.NET MVC können automatisch verwenden, um ein Adressmodell, an einer beliebigen Stelle in der Anwendung formatieren. Dies bietet Ihnen eine Möglichkeit zum Steuern des Renderns der `Address` Typ aus einer Stelle in der Anwendung.

In der *Views\Shared\DisplayTemplates* Ordner, erstellen Sie eine stark typisierte Teilansicht mit dem Namen **Adresse**:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image3.png)

Klicken Sie auf **hinzufügen**, und öffnen Sie dann auf die neue *Views\Shared\DisplayTemplates\Address.cshtml* Datei. Die neue Ansicht enthält die folgende generierte Markup:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample7.cshtml)]

Führen Sie die Anwendung und Anzeigen der `PersonDetail` anzeigen. Dieses Mal die `Address` Vorlage, die Sie gerade erstellt haben dient zum Anzeigen der `Address` komplexer Typ sein, damit die Anzeige wie folgt aussieht:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image4.png)

### <a name="summary-ways-to-specify-the-model-display-format-and-template"></a>Zusammenfassung:-Möglichkeiten, um das Anzeigeformat für Modell und die Vorlage anzugeben.

Sie haben gesehen, dass Sie das Format oder die Vorlage für eine Modelleigenschaft angeben können, mithilfe der folgenden Ansätze:

- Anwenden der `DisplayFormat` -Attribut auf eine Eigenschaft im Modell. Beispielsweise verursacht der folgende Code das Datum ohne Uhrzeit angezeigt werden:

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample8.cs)]
- Anwenden einer [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) -Attribut auf eine Eigenschaft im Modell und die Angabe des Datentyps. Beispielsweise verursacht der folgende Code das Datum ohne Uhrzeit angezeigt werden.

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample9.cs)]

    Wenn die Anwendung enthält eine *date.cshtml* Vorlage in der *Views\Shared\DisplayTemplates* Ordner oder die *Views\Movies\DisplayTemplates* -Ordner, die Vorlage wird verwendet, um das Rendern der `DateTime` Eigenschaft. Andernfalls zeigt die integrierten ASP.NET vorlagensystem die Eigenschaft als Datum.
- Erstellen einer Anzeigevorlage für die in der *Views\Shared\DisplayTemplates* Ordner oder die *Views\Movies\DisplayTemplates* Ordner, deren Name entspricht den Datentyp, der formatiert werden soll. Beispielsweise haben Sie gesehen, die die *Views\Shared\DisplayTemplates\DateTime.cshtml* wurde verwendet, um das Rendern `DateTime` Eigenschaften in einem Modell, ohne dass das Hinzufügen eines Attributs mit dem Modell oder Ansichten Markup hinzugefügt.
- Mithilfe der ["UIHint"](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) Attribut für das Modell, das die Vorlage zum Anzeigen der Modelleigenschaft angeben.
- Explizite Hinzufügen der Anzeigename der Vorlage, die [Html.DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) in einer Ansicht aufrufen.

Der Ansatz, den Sie verwenden, hängt davon ab, was Sie in Ihrer Anwendung tun müssen. Es ist nicht ungewöhnlich, kombinieren diese Ansätze, um genau die Art der Formatierung zu erhalten, die Sie benötigen.

Wechseln Sie im nächsten Abschnitt etwas TheBeerHouse und Verschieben von anpassen, wie Daten angezeigt werden, zum Anpassen, wie er eingegeben wird. Sie müssen den bearbeiten-Ansichten in der Anwendung aus, um eine elegante Möglichkeit, die Datumsangaben festlegen, geben Sie "DatePicker" jQuery einbinden.

> [!div class="step-by-step"]
> [Zurück](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
> [Weiter](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4.md)
