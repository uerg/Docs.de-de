---
uid: mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-cs
title: Überprüfen mit der IDataErrorInfo-Schnittstelle (c#) | Microsoft Docs
author: StephenWalther
description: Stephen Walther wird gezeigt, wie benutzerdefinierte Überprüfungsfehlermeldungen durch Implementieren der IDataErrorInfo-Schnittstelle in einer Modellklasse angezeigt.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 4733b9f1-9999-48fb-8b73-6038fbcc5ecb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: b5028b2e07c4144efa59824885ce96cd8b037dff
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870517"
---
<a name="validating-with-the-idataerrorinfo-interface-c"></a>Überprüfen mit der IDataErrorInfo-Schnittstelle (c#)
====================
durch [Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther wird gezeigt, wie benutzerdefinierte Überprüfungsfehlermeldungen durch Implementieren der IDataErrorInfo-Schnittstelle in einer Modellklasse angezeigt.


Das Ziel dieses Lernprogramms wird ein Ansatz besteht darin zu erläutern, mit der Ausführung der Überprüfung in einer ASP.NET MVC-Anwendung. Erfahren Sie, wie Sie verhindern, dass jemand ein HTML-Formular ohne Bereitstellung von Werten für die erforderlichen Felder des Formulars zu senden. In diesem Lernprogramm erfahren Sie, wie die Validierung mithilfe der IErrorDataInfo-Schnittstelle.

## <a name="assumptions"></a>Annahmen

In diesem Lernprogramm wird die MoviesDB-Datenbank und der Datenbanktabelle Filme verwendet. Diese Tabelle weist die folgenden Spalten:

<a id="0.5_table01"></a>


| **Spaltenname** | **Datentyp** | **NULL-Werte zulassen** |
| --- | --- | --- |
| Id | Int | False |
| Titel | Nvarchar(100) | False |
| Director | Nvarchar(100) | False |
| DateReleased | DateTime | False |


In diesem Lernprogramm mit der ich Microsoft Entity Framework Meine Datenbank Modellklassen generiert. In Abbildung 1 wird die Film-Klasse, die vom Entity Framework generierten angezeigt.


[![Die Film-Entität](validating-with-the-idataerrorinfo-interface-cs/_static/image1.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image1.png)

**Abbildung 01**: die Film-Entität ([klicken Sie hier, um das Bild in voller Größe angezeigt](validating-with-the-idataerrorinfo-interface-cs/_static/image2.png))


> [!NOTE] 
> 
> Weitere Informationen zum Generieren von Ihren Modellklassen Datenbank mithilfe von Entity Framework finden Sie meine Lernprogramm Erstellen von Modellklassen mit dem Entity Framework.


## <a name="the-controller-class"></a>Der Controller-Klasse

Wir verwenden den Home-Controller zu Filmen Liste, und erstellen neue Kinofilmen. Der Code für diese Klasse ist im Codebeispiel 1 enthalten.

**1 – Controllers\HomeController. cs auflisten**

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample1.cs)]

Die Home-Controller-Klasse in Codebeispiel 1 enthält zwei Create()-Aktionen. Die erste Aktion wird das HTML-Formular für das Erstellen eines neuen Films angezeigt. Die zweite Create()-Aktion führt das eigentliche Einfügen des neuen Films in der Datenbank. Die zweite Create()-Aktion wird aufgerufen, wenn das Formular angezeigt, die von der ersten Create()--Aktion an den Server gesendet wird.

Beachten Sie, dass die zweite Create()--Aktion, die folgenden Codezeilen enthält ein:

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample2.cs)]

IsValid-Eigenschaft gibt false zurück, wenn ein Validierungsfehler vorliegt. In diesem Fall wird die Ansicht für das Erstellen, das das HTML-Formular für das Erstellen eines Films enthält erneut angezeigt.

## <a name="creating-a-partial-class"></a>Erstellen einer partiellen Klasse

Die Film-Klasse wird vom Entity Framework generiert. Sie können den Code für die Klasse Film festzustellen, ob Sie die MoviesDBModel.edmx-Datei im Projektmappen-Explorer-Fenster erweitern, und öffnen Sie die Datei MoviesDBModel.Designer.cs im Code-Editor (siehe Abbildung 2).


[![Der Code für die Film-Entität](validating-with-the-idataerrorinfo-interface-cs/_static/image2.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image3.png)

**Abbildung 02**: der Code für die Entität Film ([klicken Sie hier, um das Bild in voller Größe angezeigt](validating-with-the-idataerrorinfo-interface-cs/_static/image4.png))


Die Film-Klasse ist eine partielle Klasse. Das bedeutet, dass wir eine andere partielle Klasse mit dem gleichen Namen zum Erweitern der Funktionalität der Film-Klasse hinzufügen können. Wir fügen die Überprüfungslogik der neuen partiellen Klasse.

Fügen Sie die Klasse 2 Auflisten der Ordner Models.

**Auflisten von 2 – Models\Movie.cs**

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample3.cs)]

Beachten Sie, die die Klasse im Codebeispiel 2 enthält das *partielle* Modifizierer. Alle Methoden oder Eigenschaften, die diese Klasse hinzugefügt werden Teil der Film-Klasse, die vom Entity Framework generiert.

## <a name="adding-onchanging-and-onchanged-partial-methods"></a>Hinzufügen von OnChanging und OnChanged partielle Methoden

Wenn Entity Framework eine Entitätsklasse generiert, fügt das Entity Framework die partielle Methoden der Klasse automatisch aus. Das Entity Framework generiert OnChanging und OnChanged partielle Methoden, die jede Eigenschaft der Klasse entsprechen.

Im Fall der Film-Klasse erstellt das Entity Framework die folgenden Methoden:

- OnIdChanging
- OnIdChanged
- OnTitleChanging
- OnTitleChanged
- OnDirectorChanging
- OnDirectorChanged
- OnDateReleasedChanging
- OnDateReleasedChanged

Die OnChanging-Methode wird rechts aufgerufen, bevor die entsprechende Eigenschaft geändert wird. Die OnChanged-Methode wird rechts aufgerufen, nach dem Ändern der Eigenschaften.

Sie können diese partielle Methoden zum Hinzufügen von Validierungslogik auf die Film-Klasse nutzen. Das Update Film-Klasse in 3 auflisten überprüft, dass der Titel und Director-Eigenschaften nicht leere Werte zugewiesen werden.

> [!NOTE] 
> 
> Eine partielle Methode ist eine Methode definiert, die in einer Klasse, die nicht zum Implementieren erforderlich sind. Wenn Sie eine partielle Methode implementieren keine entfernt der Compiler die Signatur der Methode, und alle Aufrufe der Methode bringt werden keine Kosten zur Laufzeit die partielle Methode. Im Code-Editor von Visual Studio können Sie eine partielle Methode hinzufügen, indem Sie das Schlüsselwort *partielle* gefolgt von einem Leerzeichen zum Anzeigen einer Liste von Replikatsgruppenelemente implementieren.


**3 – Models\Movie.cs auflisten**

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample4.cs)]

Z. B., wenn Sie versuchen, die Title-Eigenschaft eine leere Zeichenfolge zuweisen, klicken Sie dann folgende Fehlermeldung wird zugewiesen ein Wörterbuch mit dem Namen \_Fehler.

An diesem Punkt nichts tatsächlich geschieht, wenn Sie die Title-Eigenschaft eine leere Zeichenfolge zuweisen und ein Fehler wird an die Private hinzugefügt \_Fehler Feld. Wir müssen implementieren die IDataErrorInfo-Schnittstelle, um diese Überprüfungsfehler auf, um das ASP.NET MVC-Framework verfügbar zu machen.

## <a name="implementing-the-idataerrorinfo-interface"></a>Implementieren der IDataErrorInfo-Schnittstelle

IDataErrorInfo-Schnittstelle ist Teil von .NET Framework seit der ersten Version. Diese Schnittstelle ist eine sehr einfache Schnittstelle:

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample5.cs)]

Wenn eine Klasse die IDataErrorInfo-Schnittstelle implementiert, wird diese Schnittstelle in ASP.NET MVC-Framework verwenden, beim Erstellen einer Instanz der Klasse. Die Home-Controller Create()-Aktion akzeptiert z. B. eine Instanz der Klasse Film:

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample6.cs)]

ASP.NET MVC-Framework erstellt die Instanz des Films an die Create()--Aktion übergeben wird, indem Sie einen Modellbinder (DefaultModelBinder). Der Modellbinder ist verantwortlich für das Erstellen einer Instanz des Objekts Film durch die HTML-Felder zu einer Instanz des Objekts Film binden.

Die DefaultModelBinder erkennt, und zwar unabhängig davon, ob eine Klasse die IDataErrorInfo-Schnittstelle implementiert. Wenn eine Klasse diese Schnittstelle implementiert, ruft der Modellbinder IDataErrorInfo.this Indexer für jede Eigenschaft der Klasse. Der Indexer auf eine Fehlermeldung zurück fügt der Modellbinder diese Fehlermeldung Status automatisch zu modellieren.

Die DefaultModelBinder überprüft auch die IDataErrorInfo.Error-Eigenschaft. Diese Eigenschaft dient zur Darstellung von bestimmten nicht-eigenschaftenvalidierung Fehler im Zusammenhang mit der Klasse. Beispielsweise empfiehlt es sich um eine Validierungsregel zu erzwingen, von die die Werte von mehreren Eigenschaften der Klasse Film abhängt. In diesem Fall würden Sie einen Validierungsfehler aus Fehlereigenschaft zurückgeben.

Die aktualisierte Film-Klasse in 4 auflisten implementiert die IDataErrorInfo-Schnittstelle.

**Auflisten von 4 – Models\Movie.cs (IDataErrorInfo implementiert)**

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample7.cs)]

Auflisten von 4 der Indexereigenschaft überprüft die \_Errors-Auflistung, um festzustellen, ob es sich um einen Schlüssel enthält, die den Namen der Eigenschaft entspricht dem Indexer übergeben. Wenn keine Validierungsfehler, die der Eigenschaft zugeordnet ist, wird eine leere Zeichenfolge zurückgegeben.

Sie müssen nicht den Home-Controller in keiner Weise verwenden Sie die geänderte Film-Klasse ändern. Die Seite wird angezeigt, die in Abbildung 3 wird veranschaulicht, was geschieht, wenn kein Wert für den Titel oder Director Formularfelder eingegeben wird.


[![Aktionsmethoden erstellen automatisch](validating-with-the-idataerrorinfo-interface-cs/_static/image3.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image5.png)

**Abbildung 03**: ein Formular mit fehlenden Werten ([klicken Sie hier, um das Bild in voller Größe angezeigt](validating-with-the-idataerrorinfo-interface-cs/_static/image6.png))


Beachten Sie, dass der Wert DateReleased automatisch überprüft wird. Da die DateReleased-Eigenschaft nicht NULL-Werte akzeptiert, generiert der DefaultModelBinder einen Validierungsfehler für diese Eigenschaft automatisch, wenn sie nicht über einen Wert verfügt. Wenn Sie die Fehlermeldung für die Eigenschaft DateReleased ändern möchten, müssen Sie einen benutzerdefinierten Modellbinder zu erstellen.

## <a name="summary"></a>Zusammenfassung

In diesem Lernprogramm haben Sie gelernt, wie die IDataErrorInfo-Schnittstelle verwenden, um Überprüfungsfehlermeldungen zu generieren. Zunächst haben wir eine partielle Film-Klasse, die die Funktionalität der partiellen Film-Klasse, die vom Entity Framework generierten erweitert. Als Nächstes haben wir die Film Klasse OnTitleChanging() und OnDirectorChanging() partiellen Methoden Validierungslogik hinzugefügt. Abschließend implementiert wir IDataErrorInfo-Schnittstelle, um diese validierungsmeldungen zu ASP.NET MVC-Framework verfügbar zu machen.

> [!div class="step-by-step"]
> [Zurück](performing-simple-validation-cs.md)
> [Weiter](validating-with-a-service-layer-cs.md)
