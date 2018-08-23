---
uid: mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-vb
title: Überprüfen mit der IDataErrorInfo-Schnittstelle (VB) | Microsoft-Dokumentation
author: StephenWalther
description: Stephen Walther erfahren Sie, wie benutzerdefinierte Überprüfungsfehlermeldungen durch Implementieren der IDataErrorInfo-Schnittstelle in einer Modellklasse angezeigt.
ms.author: riande
ms.date: 03/02/2009
ms.assetid: 3a8a9d9f-82dd-4959-b7c6-960e9ce95df1
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: e9b989d0110c3947583fd70bd38b29dcb2bb5c31
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833367"
---
<a name="validating-with-the-idataerrorinfo-interface-vb"></a>Überprüfen mit der IDataErrorInfo-Schnittstelle (VB)
====================
durch [Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther erfahren Sie, wie benutzerdefinierte Überprüfungsfehlermeldungen durch Implementieren der IDataErrorInfo-Schnittstelle in einer Modellklasse angezeigt.


Das Ziel in diesem Tutorial wird ein Ansatz zum Ausführen der Validierung in ASP.NET MVC-Anwendungen beschrieben. Erfahren Sie, wie Sie verhindern, dass eine Person ein HTML-Formular ohne Angabe von Werten für die erforderlichen Felder des Formulars zu senden. In diesem Tutorial erfahren Sie, wie die Validierung mithilfe der IErrorDataInfo-Schnittstelle.

## <a name="assumptions"></a>Annahmen

In diesem Tutorial verwende ich die MoviesDB-Datenbank und der Tabelle der Datenbank Filme. Diese Tabelle weist die folgenden Spalten:

<a id="0.6_table01"></a>


| **Name der Spalte** | **Datentyp** | **NULL-Werte zulassen** |
| --- | --- | --- |
| Id | Int | False |
| Titel | nvarchar(100) | False |
| Director | nvarchar(100) | False |
| DateReleased | DateTime | False |


In diesem Tutorial verwende ich das Microsoft Entity Framework, meine Datenbank Modellklassen generiert werden. In Abbildung 1 ist die Movie-Klasse, die vom Entity Framework generiert wird.


[![Die Movie-Entität](validating-with-the-idataerrorinfo-interface-vb/_static/image1.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image1.png)

**Abbildung 01**: der Movie-Entität ([klicken Sie, um das Bild in voller Größe anzeigen](validating-with-the-idataerrorinfo-interface-vb/_static/image2.png))


> [!NOTE] 
> 
> Weitere Informationen zum mithilfe von Entity Framework auf um Ihren Modellklassen für die Datenbank zu generieren, finden Sie meinen Tutorial Erstellen von Modellklassen mit dem Entity Framework.


## <a name="the-controller-class"></a>Der Controller-Klasse

Wir verwenden den Home-Controller auf Liste Filme und Erstellen neuer Filme. Der Code für diese Klasse ist in Codebeispiel 1 enthalten.

**1 – Controllers\HomeController.vb auflisten**

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample1.vb)]

Die Home-Controller-Klasse in Codebeispiel 1 enthält zwei Create() Aktionen. Die erste Aktion zeigt das HTML-Formular zum Erstellen eines neuen Films. Die zweite Create()-Aktion führt das eigentliche Einfügen des neuen Film in der Datenbank. Die zweite Create()-Aktion wird aufgerufen, wenn das Formular die erste Create()-Aktion an den Server gesendet wird.

Beachten Sie, dass die zweite Create()-Aktion die folgenden Codezeilen enthält:

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample2.vb)]

Die Eigenschaft "IsValid" gibt false zurück, wenn ein Überprüfungsfehler vorliegt. In diesem Fall wird die Ansicht erstellen, die das HTML-Formular zum Erstellen eines Films enthält erneut angezeigt.

## <a name="creating-a-partial-class"></a>Erstellen einer partiellen Klasse

Die Movie-Klasse wird vom Entity Framework generiert. Sie können den Code für die Movie-Klasse sehen, wenn Sie die MoviesDBModel.edmx-Datei im Projektmappen-Explorer-Fenster zu erweitern, und öffnen Sie die MoviesDBModel.Designer.vb-Datei im Code-Editor (siehe Abbildung 2).


[![Der Code für die Movie-Entität](validating-with-the-idataerrorinfo-interface-vb/_static/image2.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image3.png)

**Abbildung 02**: der Code für die Movie-Entität ([klicken Sie, um das Bild in voller Größe anzeigen](validating-with-the-idataerrorinfo-interface-vb/_static/image4.png))


Die Movie-Klasse ist eine partielle Klasse. Das bedeutet, dass wir eine andere partielle Klasse mit dem gleichen Namen zum Erweitern der Funktionalität der Movie-Klasse hinzufügen können. Wir fügen unserer Validierungslogik auf die neue partielle Klasse.

Fügen Sie der Klasse in Liste 2 auf den Ordner "Models".

**Codebeispiel 2 - Models\Movie.vb**

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample3.vb)]

Beachten Sie, die die Klasse im Codebeispiel 2 enthält die *teilweise* Modifizierer. Alle Methoden oder Eigenschaften, die Sie für diese Klasse hinzufügen, werden die Movie-Klasse, die vom Entity Framework generiert.

## <a name="adding-onchanging-and-onchanged-partial-methods"></a>Hinzufügen von OnChanging und OnChanged partielle Methoden

Wenn Entity Framework eine Entitätsklasse generiert, fügt Entity Framework die partielle Methoden der Klasse automatisch aus. Das Entity Framework generiert OnChanging und OnChanged partielle Methoden, die jede Eigenschaft der Klasse entsprechen.

Im Fall von die Movie-Klasse erstellt Entity Framework die folgenden Methoden:

- OnIdChanging
- OnIdChanged
- OnTitleChanging
- OnTitleChanged
- OnDirectorChanging
- OnDirectorChanged
- OnDateReleasedChanging
- OnDateReleasedChanged

Die OnChanging-Methode wird rechts aufgerufen, bevor die entsprechende Eigenschaft geändert wird. Die OnChanged-Methode wird rechts aufgerufen, nachdem die Eigenschaft geändert wird.

Sie können diese partiellen Methoden, um die Movie-Klasse Validierungslogik hinzufügen nutzen. Das Update Movie-Klasse in Programmausdruck 3 stellt sicher, dass die Eigenschaften für Titel und Director nicht leeren Werte zugewiesen werden.

> [!NOTE] 
> 
> Eine partielle Methode ist eine Methode, die in einer Klasse, die Sie nicht erforderlich, um die Implementierung definiert. Wenn Sie eine partielle Methode implementieren, nicht der Compiler entfernt die Signatur der Methode, und alle Aufrufe an die Methode also es werden keine Laufzeit-Kosten im Zusammenhang mit der partiellen Methode. In Visual Studio Code-Editor können Sie eine partielle Methode hinzufügen, durch das Schlüsselwort *teilweise* gefolgt von einem Leerzeichen zum Anzeigen einer Liste von Teilansichten implementieren.


**Codebeispiel 3 - Models\Movie.vb**

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample4.vb)]

Z. B. Wenn Sie versuchen, eine leere Zeichenfolge an die Title-Eigenschaft zuweisen, klicken Sie dann eine Fehlermeldung erhält in ein Wörterbuch mit dem Namen \_Fehler.

An diesem Punkt nichts tatsächlich geschieht, wenn Sie eine leere Zeichenfolge an die Title-Eigenschaft zuweisen, und ein Fehler, an die Private hinzugefügt wird \_Fehler Feld. Wir müssen zum Implementieren der IDataErrorInfo-Schnittstelle, um diese Überprüfungsfehler auf, um das ASP.NET MVC-Framework verfügbar zu machen.

## <a name="implementing-the-idataerrorinfo-interface"></a>Implementieren der IDataErrorInfo-Schnittstelle

Die IDataErrorInfo-Schnittstelle ist Teil von .NET Framework seit der ersten Version. Diese Schnittstelle ist eine sehr einfache Schnittstelle:

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample5.vb)]

Wenn eine Klasse die IDataErrorInfo-Schnittstelle implementiert, wird diese Schnittstelle in ASP.NET MVC-Framework verwenden, beim Erstellen einer Instanz der Klasse. Die Home-Controller Create() Aktion akzeptiert z. B. eine Instanz der Movie-Klasse:

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample6.vb)]

ASP.NET MVC-Framework erstellt, die Instanz des Films an die Create()-Aktion mithilfe eines Modellbinders (der DefaultModelBinder) übergeben wird. Die modellbindung ist dafür verantwortlich, erstellen eine Instanz von der Movie-Objekt, durch die Bindung der HTML-Formularfelder mit einer Instanz von der Movie-Objekt.

Der DefaultModelBinder erkennt, und zwar unabhängig davon, ob eine Klasse die IDataErrorInfo-Schnittstelle implementiert. Wenn eine Klasse diese Schnittstelle implementiert, ruft die modellbindung IDataErrorInfo.this Indexer für jede Eigenschaft der Klasse. Wenn der Indexer eine Fehlermeldung zurückgegeben, fügt die modellbindung diese Fehlermeldung Zustand automatisch zu modellieren.

Der DefaultModelBinder überprüft auch die IDataErrorInfo.Error-Eigenschaft. Diese Eigenschaft sollte keine Eigenschaften spezifische Validierungsfehler im Zusammenhang mit der Klasse darstellen. Beispielsweise empfiehlt es sich um eine Validierungsregel zu erzwingen, von die die Werte aus mehreren Eigenschaften der Movie-Klasse abhängt. In diesem Fall müsste einen Validierungsfehler aus die Error-Eigenschaft zurückgegeben werden.

In Listing 4 die aktualisierte Movie-Klasse implementiert die IDataErrorInfo-Schnittstelle.

**Codebeispiel 4: Models\Movie.vb (IDataErrorInfo implementiert)**

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample7.vb)]

In Listing 4, die Indexereigenschaft überprüft die \_Errors-Auflistung, um festzustellen, ob es sich um einen Schlüssel enthält, die Namen der Eigenschaft entspricht, dem Indexer übergeben. Wenn kein Validierungsfehler, die der Eigenschaft zugeordnet ist, wird eine leere Zeichenfolge zurückgegeben.

Sie müssen nicht den Home-Controller in keiner Weise verwenden Sie die geänderte Movie-Klasse zu ändern. Die Seite angezeigt, die in Abbildung 3 wird veranschaulicht, was geschieht, wenn kein Wert für die Felder für Titel oder Director Formular eingegeben wird.


[![Aktionsmethoden erstellen automatisch](validating-with-the-idataerrorinfo-interface-vb/_static/image3.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image5.png)

**Abbildung 03**: ein Formular mit fehlenden Werten ([klicken Sie, um das Bild in voller Größe anzeigen](validating-with-the-idataerrorinfo-interface-vb/_static/image6.png))


Beachten Sie, dass der Wert DateReleased automatisch überprüft wird. Da die DateReleased-Eigenschaft keine NULL-Werte akzeptiert, generiert der DefaultModelBinder ein Überprüfungsfehler für diese Eigenschaft automatisch, wenn sie nicht über einen Wert verfügt. Wenn Sie die Fehlermeldung für die DateReleased-Eigenschaft zu ändern, müssen Sie eine benutzerdefinierte modellbindung erstellen möchten.

## <a name="summary"></a>Zusammenfassung

In diesem Tutorial haben Sie gelernt, wie die IDataErrorInfo-Schnittstelle verwenden, um Meldungen für Validierungsfehler zu generieren. Zunächst haben wir eine partielle Movie-Klasse, die die Funktionalität von der partiellen Movie-Klasse, die von Entity Framework generierten erweitert. Als Nächstes haben wir die Movie-Klasse OnTitleChanging() und OnDirectorChanging() partiellen Methoden Validierungslogik hinzugefügt. Schließlich implementiert haben wir die IDataErrorInfo-Schnittstelle um diese Nachrichten zur inhaltsprüfung zu ASP.NET MVC-Framework verfügbar zu machen.

> [!div class="step-by-step"]
> [Zurück](performing-simple-validation-vb.md)
> [Weiter](validating-with-a-service-layer-vb.md)
