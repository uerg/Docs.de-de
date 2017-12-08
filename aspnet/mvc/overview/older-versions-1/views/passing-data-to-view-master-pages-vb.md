---
uid: mvc/overview/older-versions-1/views/passing-data-to-view-master-pages-vb
title: "Übergeben von Daten an View Master Pages (VB) | Microsoft Docs"
author: microsoft
description: "Ziel dieses Lernprogramms wird erläutert, wie Sie Daten von einem Controller an eine Ansicht Gestaltungsvorlage übergeben können. Untersuchen wir zwei Strategien für die Übergabe von Daten an eine Ansicht m..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/16/2008
ms.topic: article
ms.assetid: 37a1ebae-8773-408f-8645-d21da7ff9ae1
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/passing-data-to-view-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: d92a1afe46af124e835b3d59f2b2093402742bbd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="passing-data-to-view-master-pages-vb"></a>Übergeben von Daten an View Master Pages (VB)
====================
durch [Microsoft](https://github.com/microsoft)

[PDF herunterladen](http://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_13_VB.pdf)

> Ziel dieses Lernprogramms wird erläutert, wie Sie Daten von einem Controller an eine Ansicht Gestaltungsvorlage übergeben können. Untersuchen wir zwei Strategien für die Übergabe von Daten auf einer Masterseite anzeigen. Zuerst wird erläutert, eine einfache Lösung, die in einer Anwendung, die schwer führt zu verwalten ist. Als Nächstes untersuchen wir eine viel bessere Lösung, die etwas mehr anfänglichen arbeiten, aber die Ergebnisse in einer Anwendung sind wesentlich besser verwaltbar erforderlich sind.


## <a name="passing-data-to-view-master-pages"></a>Übergeben von Daten an View Master Pages

Ziel dieses Lernprogramms wird erläutert, wie Sie Daten von einem Controller an eine Ansicht Gestaltungsvorlage übergeben können. Untersuchen wir zwei Strategien für die Übergabe von Daten auf einer Masterseite anzeigen. Zuerst wird erläutert, eine einfache Lösung, die in einer Anwendung, die schwer führt zu verwalten ist. Als Nächstes untersuchen wir eine viel bessere Lösung, die etwas mehr anfänglichen arbeiten, aber die Ergebnisse in einer Anwendung sind wesentlich besser verwaltbar erforderlich sind.

### <a name="the-problem"></a>Das Problem

Angenommen, Sie eine Anwendung für eine Film erstellen und die Liste der Kategorien Film auf jeder Seite in der Anwendung angezeigt werden soll (siehe Abbildung 1). Angenommen Sie, darüber hinaus, dass die Liste der Film Kategorien in einer Datenbanktabelle gespeichert werden. In diesem Fall wäre es sinnvoll, die Kategorien aus der Datenbank abrufen und die Liste der Film Kategorien innerhalb einer Masterseite Ansicht zu rendern.


[![Anzeigen von Film-Kategorien auf einer Masterseite anzeigen](passing-data-to-view-master-pages-vb/_static/image2.png)](passing-data-to-view-master-pages-vb/_static/image1.png)

**Abbildung 01**: Anzeigen von Film-Kategorien auf einer Masterseite anzeigen ([klicken Sie hier, um das Bild in voller Größe angezeigt](passing-data-to-view-master-pages-vb/_static/image3.png))


Hier ist das Problem. Wie werden die Liste der Film Kategorien auf der Masterseite abgerufen? Es ist verführerisch Methoden von Ihren Modellklassen in die Masterseite direkt aufrufen. Das heißt, ist es verlockend, den Code zum Abrufen der Daten aus der Datenbank rechts auf der Masterseite einschließen. Allerdings würde umgehen die MVC-Controller für den Zugriff auf die Datenbank die klare Trennung von Anliegen verletzt werden, die eine der größten Vorteile bei der Erstellung einer MVC-Anwendung ist.

In einer MVC-Anwendung werden alle Interaktionen zwischen Ihren MVC-Ansichten und das MVC-Modell vom MVC-Controller behandelt werden sollen. Diese Trennung von Anliegen führt zu einer Anwendung leicht verwaltbare, anpassbar und getestet werden können.

In einer MVC-Anwendung sollten alle Daten, die an eine Ansicht – z. B. eine Gestaltungsvorlage Ansicht – übergeben einer Ansicht durch eine Controlleraktion übergeben werden. Darüber hinaus müssen die Daten übergeben werden, durch die Nutzung von Anzeigedaten. Im weiteren Verlauf dieses Lernprogramms untersuchen ich zwei Methoden zum Anzeigen von Daten auf einer Masterseite Ansicht übergeben.

### <a name="the-simple-solution"></a>Die einfache Lösung

Beginnen Sie mit der einfachsten Lösung zum Anzeigen von Daten von einem Controller an einer Masterseite Ansicht übergeben. Die einfachste Lösung besteht darin, die Ansichtsdaten für die Gestaltungsvorlage in jede Controlleraktion übergeben.

Betrachten Sie den Controller im Codebeispiel 1 aus. Macht zwei Aktionen, die mit dem Namen `Index()` und `Details()`. Die `Index()` Aktionsmethode gibt jeder Film in der Datenbanktabelle Filme zurück. Die `Details()` Aktionsmethode gibt jeder Film in einer bestimmten Film-Kategorie.

**Auflisten von 1 –`Controllers\HomeController.vb`**

[!code-vb[Main](passing-data-to-view-master-pages-vb/samples/sample1.vb)]

Beachten Sie, dass sowohl die `Index()` und `Details()` Aktionen zwei Elemente zum Anzeigen von Daten hinzufügen. Die `Index()` Aktion fügt zwei Schlüssel: Kategorien und Filme. Der Kategorien-Schlüssel stellt die Liste der Film-Kategorien, die von der Sicht Masterseite angezeigt. Der Schlüssel Filme stellt die Liste von Filmen, die durch die Index-Ansichtsseite angezeigt.

Die `Details()` Aktion fügt auch zwei Schlüssel mit dem Namen Kategorien und Filme. Der Schlüssel "Categories" repräsentiert noch einmal: die Liste der Film-Kategorien, die von der Sicht Masterseite angezeigt. Der Filme Schlüssel stellt die Liste der Kinofilmen in einer bestimmten Kategorie angezeigt, die Details-Ansichtsseite (siehe Abbildung 2).


[![Die Detailansicht](passing-data-to-view-master-pages-vb/_static/image5.png)](passing-data-to-view-master-pages-vb/_static/image4.png)

**Abbildung 02**: Details anzeigen ([klicken Sie hier, um das Bild in voller Größe angezeigt](passing-data-to-view-master-pages-vb/_static/image6.png))


Auflisten von 2 enthalten die Indexansicht. Es durchläuft einfach die Liste von Filmen dargestellt, die vom Element "Filme" in den Anzeigedaten.

**Auflisten von 2 –`Views\Home\Index.aspx`**

[!code-aspx[Main](passing-data-to-view-master-pages-vb/samples/sample2.aspx)]

Die Ansicht, die Gestaltungsvorlage in 3 auflisten enthalten ist. Die master-Ansichtsseite durchläuft und rendert alle Film-Kategorien, die vom Element "Kategorien" dargestellt, aus der Anzeigedaten.

**Auflisten von 3:`Views\Shared\Site.master`**

[!code-aspx[Main](passing-data-to-view-master-pages-vb/samples/sample3.aspx)]

Alle Daten wird auf die Ansicht und der Gestaltungsvorlage Ansicht über Ansichtsdaten übergeben. Das ist die korrekte Methode zum Übergeben von Daten an die Gestaltungsvorlage.

Was ist mit dieser Lösung gibt es Probleme? Das Problem ist, dass diese Lösung das Prinzip trocken (wiederhole Dich nicht) verletzt. Jede Controlleraktion muss sehr dieselbe Liste von Film-Kategorien zum Anzeigen von Daten hinzufügen. Mit doppelten Code in Ihrer Anwendung macht Ihre Anwendung viel schwieriger zu verwalten, anzupassen und zu ändern.

### <a name="the-good-solution"></a>Die geeignete Lösung

In diesem Abschnitt untersuchen wir eine alternative und bessere Lösung zum Übergeben von Daten aus einer Controlleraktion auf einer Masterseite anzeigen. Statt die Film-Kategorien für die Gestaltungsvorlage in jede Controlleraktion zu addieren, wir die Film Kategorien hinzufügen die Ansichtsdaten nur einmal. Alle Daten anzeigen, durch die Sicht Gestaltungsvorlage verwendet wird in einer Anwendung-Controller hinzugefügt.

Die ApplicationController-Klasse ist im Codebeispiel 4 enthalten.

Die ApplicationController-Klasse ist im Codebeispiel 4 enthalten.

**Auflisten von 4 –`Controllers\ApplicationController.vb`**

[!code-vb[Main](passing-data-to-view-master-pages-vb/samples/sample4.vb)]

Es gibt drei Dinge, die Sie zum Auflisten von 4 Controller Anwendung beachten sollten. Erstens ist zu beachten, dass die Klasse von der System.Web.Mvc.Controller Basisklasse erbt. Die Anwendung ist eine Controllerklasse.

Zweitens feststellen Sie, dass die Anwendung Controllerklasse eine MustInherit-Klasse. MustInherit-Klasse ist eine Klasse, die durch eine konkrete Klasse implementiert werden muss. Da die Anwendung Controller eine MustInherit-Klasse ist, können nicht Sie nicht direkt in der Klasse definiert Methoden aufrufen. Wenn Sie versuchen, direkt aufrufen, die Anwendungsklasse dann erhalten eine Ressource wurde nicht gefunden-Fehlermeldung Sie.

Dritte, beachten Sie, dass der Anwendung-Controller einen Konstruktor enthält, der von der Liste der Film Kategorien zum Anzeigen von Daten hinzugefügt. Jeder Controllerklasse, die von der Anwendung Controller erbt Ruft die Anwendung des Controllers-Konstruktor automatisch. Wenn Sie alle Aktionen auf jedem Controller aufrufen, die von der Anwendung Controller erbt, die Film-Kategorien wird in die Ansichtsdaten automatisch eingeschlossen.

Der Controller Filme auflisten 5 erbt von der Anwendung-Controller.

**Auflisten von 5 –`Controllers\MoviesController.vb`**

[!code-vb[Main](passing-data-to-view-master-pages-vb/samples/sample5.vb)]

Der Controller Filme genau wie im vorherigen Abschnitt erläutert den Home-Controller macht zwei Aktionsmethoden `Index()` und `Details()`. Beachten Sie, dass die Liste der Film Kategorien angezeigt, die für die Gestaltungsvorlage Ansicht nicht hinzugefügt werden, zum Anzeigen von Daten in einem der `Index()` oder `Details()` Methode. Da der Filme Controller vom Controller Anwendung erbt, wird die Liste der Film Kategorien zum Anzeigen von Daten automatisch hinzugefügt.

Beachten Sie, dass diese Lösung zum Anzeigen von Daten für eine Sicht Gestaltungsvorlage hinzufügen nicht das Prinzip trocken (wiederhole Dich nicht verletzt). Der Code zum Hinzufügen der Liste der Film Kategorien zum Anzeigen von Daten in nur ein Standort enthalten ist: der Konstruktor für den Controller für die Anwendung.

### <a name="summary"></a>Zusammenfassung

In diesem Lernprogramm erläutert wir zwei Ansätze zum Anzeigen von Daten von einem Controller an einer Masterseite Ansicht übergeben. Zuerst überprüft wir eine einfache, aber schwer zu Ansatz zu verwalten. Im ersten Abschnitt wird erläutert, wie Sie die Ansichtsdaten für eine Sicht Gestaltungsvorlage in jeder jeder Controlleraktion in Ihrer Anwendung hinzufügen können. Es hat ergeben, dass dies eine ungültige Ansatz war, da er das Prinzip trocken (wiederhole Dich nicht) verletzt.

Als Nächstes untersucht wir eine viel bessere Strategie zum Hinzufügen von Daten, die von einer Masterseite Ansicht zum Anzeigen von Daten erforderlich. Statt die Ansichtsdaten in jede Controlleraktion zu addieren, haben wir die Ansichtsdaten nur einmal innerhalb eines Controllers Anwendung hinzugefügt. Auf diese Weise können Sie doppelte Code vermeiden, bei der Übergabe von Daten an eine Ansicht Masterseite in einer ASP.NET MVC-Anwendung.

>[!div class="step-by-step"]
[Zurück](creating-page-layouts-with-view-master-pages-vb.md)
