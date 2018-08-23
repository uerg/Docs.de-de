---
uid: mvc/overview/older-versions-1/views/passing-data-to-view-master-pages-vb
title: Übergeben von Daten an Ansichtsmasterseiten (VB) | Microsoft-Dokumentation
author: microsoft
description: Das Ziel in diesem Tutorial wird beschrieben, wie Sie Daten von einem Controller an eine Masterseite für die Ansicht übergeben können. Betrachten wir zwei Strategien für die Übergabe von Daten an eine Ansicht m...
ms.author: riande
ms.date: 10/16/2008
ms.assetid: 37a1ebae-8773-408f-8645-d21da7ff9ae1
msc.legacyurl: /mvc/overview/older-versions-1/views/passing-data-to-view-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: 9b840e0a5cc325a043ae88c10f52cca418589119
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836206"
---
<a name="passing-data-to-view-master-pages-vb"></a>Übergeben von Daten an Ansichtsmasterseiten (VB)
====================
durch [Microsoft](https://github.com/microsoft)

[PDF herunterladen](http://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_13_VB.pdf)

> Das Ziel in diesem Tutorial wird beschrieben, wie Sie Daten von einem Controller an eine Masterseite für die Ansicht übergeben können. Untersuchen wir zwei Strategien für die Übergabe von Daten an eine Masterseite anzeigen. Zuerst geht es sich um eine einfache Lösung, die sich in einer Anwendung, die schwierig ergibt zu verwalten ist. Als Nächstes sehen wir eine viel bessere Lösung, die ein wenig anfängliche aufwendiger, aber die Ergebnisse in einer wesentlich besser zu verwaltender-Anwendung erforderlich sind.


## <a name="passing-data-to-view-master-pages"></a>Übergeben von Daten an Ansichtsmasterseiten

Das Ziel in diesem Tutorial wird beschrieben, wie Sie Daten von einem Controller an eine Masterseite für die Ansicht übergeben können. Untersuchen wir zwei Strategien für die Übergabe von Daten an eine Masterseite anzeigen. Zuerst geht es sich um eine einfache Lösung, die sich in einer Anwendung, die schwierig ergibt zu verwalten ist. Als Nächstes sehen wir eine viel bessere Lösung, die ein wenig anfängliche aufwendiger, aber die Ergebnisse in einer wesentlich besser zu verwaltender-Anwendung erforderlich sind.

### <a name="the-problem"></a>Das Problem

Stellen Sie sich vor, dass Sie beim Erstellen einer filmdatenbankanwendung und Sie die Liste der Kategorien des Films auf jeder Seite in Ihrer Anwendung anzeigen möchten (siehe Abbildung 1). Stellen Sie sich vor, darüber hinaus, dass die Liste der Movie-Kategorien in einer Datenbanktabelle gespeichert werden. In diesem Fall wäre es sinnvoll, die Kategorien aus der Datenbank abgerufen und die Liste der Film Kategorien in einer Masterseite für die Ansicht zu rendern.


[![Anzeigen von Film-Kategorien in einer Masterseite anzeigen](passing-data-to-view-master-pages-vb/_static/image2.png)](passing-data-to-view-master-pages-vb/_static/image1.png)

**Abbildung 01**: Anzeigen von Film-Kategorien in einer Masterseite anzeigen ([klicken Sie, um das Bild in voller Größe anzeigen](passing-data-to-view-master-pages-vb/_static/image3.png))


Hier ist das Problem. Wie abrufen Sie die Liste der Film Kategorien auf der Masterseite? Es ist verlockend, Methoden, die von Ihren Modellklassen in die Masterseite direkt aufzurufen. Das heißt, ist es verlockend, den Code zum Abrufen der Daten aus der Datenbank-direkt in Ihrer Masterseite einschließen. Allerdings würde Ihre MVC-Controller für den Zugriff auf die Datenbank umgehen die klare Trennung von Anliegen verletzt werden, die eine der größten Vorteile bei der Erstellung einer MVC-Anwendung ist.

In einer MVC-Anwendung werden alle Interaktionen zwischen Ihren MVC-Ansichten und das MVC-Modell von Ihre MVC-Controller verarbeitet werden sollen. Diese Trennung von Belangen führt zu einer Anwendung besser verwaltbaren, anpassbar und getestet werden.

In einer MVC-Anwendung sollten alle Daten, die an eine Ansicht – z. B. eine Gestaltungsvorlage Ansicht – übergeben durch eine Controlleraktion auf eine Ansicht übergeben werden. Darüber hinaus sollten die Daten übergeben werden, durch die Nutzung der Anzeigedaten. Überprüfen Sie im weiteren Verlauf dieses Lernprogramms ich zwei Methoden zum Anzeigen von Daten an eine Masterseite für die Ansicht übergeben.

### <a name="the-simple-solution"></a>Eine einfache Lösung

Wir beginnen mit die einfachste Lösung für das Anzeigen von Daten von einem Controller an eine Masterseite für die Ansicht übergeben. Die einfachste Lösung ist die Anzeigen von Daten für die Gestaltungsvorlage jede Controlleraktion übergeben.

Betrachten Sie den Controller in Codebeispiel 1. Es macht zwei Aktionen, die mit dem Namen `Index()` und `Details()`. Die `Index()` Aktionsmethode gibt jeder Film in der Tabelle der Datenbank Filme. Die `Details()` Aktionsmethode gibt jeder Film in einer bestimmten Film-Kategorie zurück.

**Codebeispiel 1: `Controllers\HomeController.vb`**

[!code-vb[Main](passing-data-to-view-master-pages-vb/samples/sample1.vb)]

Beachten Sie, dass sowohl die `Index()` und `Details()` Aktionen hinzufügen, zwei Elemente zum Anzeigen von Daten. Die `Index()` Aktion fügt zwei Schlüssel: Kategorien und Filme. Die Kategorien-Schlüssel darstellt, die Liste der Movie-Kategorien, die von der Masterseite Ansicht angezeigt wird. Die Filme-Schlüssel darstellt, die Liste der Filme, die von der Indexseite der Ansicht angezeigt wird.

Die `Details()` Aktion fügt außerdem zwei Schlüssel namens "Kategorien" und "Filme hinzu. Der Schlüssel Kategorien darstellt wieder die Liste der Movie-Kategorien, die von der Masterseite Ansicht angezeigt. Die Filme-Schlüssel darstellt, die Liste von Filmen in einer bestimmten Kategorie angezeigt, die von der Seite Details anzeigen (siehe Abbildung 2).


[![Die Detailansicht](passing-data-to-view-master-pages-vb/_static/image5.png)](passing-data-to-view-master-pages-vb/_static/image4.png)

**Abbildung 02**: die Details anzuzeigen ([klicken Sie, um das Bild in voller Größe anzeigen](passing-data-to-view-master-pages-vb/_static/image6.png))


Ansicht "Index" ist im Codebeispiel 2 enthalten. Es durchläuft einfach die Liste von Filmen, die durch das Movies-Element im Anzeigen von Daten dargestellt wird.

**Codebeispiel 2: `Views\Home\Index.aspx`**

[!code-aspx[Main](passing-data-to-view-master-pages-vb/samples/sample2.aspx)]

Die Ansicht, die Masterseite in Programmausdruck 3 enthalten ist. Die master-Ansichtsseite durchläuft und rendert alle Kategorien Film anzeigen von Daten durch die Kategorien-Element dargestellt.

**Codebeispiel 3: `Views\Shared\Site.master`**

[!code-aspx[Main](passing-data-to-view-master-pages-vb/samples/sample3.aspx)]

Alle Daten wird an die Ansicht und die Masterseite für die Ansicht übergeben, über das Anzeigen von Daten. Das ist die richtige Vorgehensweise zum Übergeben von Daten an die Masterseite.

Was ist also falsch an dieser Lösung? Das Problem ist, dass diese Lösung das DRY (wiederhole Dich nicht)-Prinzip verletzt. Jede Controlleraktion muss sehr dieselbe Liste von Film-Kategorien zum Anzeigen von Daten hinzufügen. Mit duplizierten Code in Ihrer Anwendung macht Ihre Anwendung sehr viel schwieriger zu verwalten, anpassen und ändern.

### <a name="the-good-solution"></a>Die gute Lösung.

In diesem Abschnitt untersuchen wir eine alternative und bessere Lösung zum Übergeben von Daten über eine Controlleraktion auf eine Gestaltungsvorlage anzeigen. Anstatt die Movie-Kategorien für die Gestaltungsvorlage in jede Controlleraktion hinzuzufügen, fügen wir die Movie-Kategorien hinzu das Anzeigen von Daten nur einmal. Alle Daten anzeigen, ein, die die Masterseite für die Sicht wird in einem Application Controller hinzugefügt.

Die ApplicationController-Klasse ist in Listing 4 enthalten.

Die ApplicationController-Klasse ist in Listing 4 enthalten.

**Codebeispiel 4: `Controllers\ApplicationController.vb`**

[!code-vb[Main](passing-data-to-view-master-pages-vb/samples/sample4.vb)]

Es gibt drei Dinge, die Sie zum Application Controller in Listing 4 sehen. Erstens ist zu beachten, dass die Klasse von System.Web.Mvc.Controller Basisklasse erbt. Der Controller der Anwendung ist eine Controller-Klasse.

Zweitens: Beachten Sie, dass die Application Controller-Klasse eine MustInherit-Klasse. MustInherit-Klasse ist eine Klasse, die von einer konkreten Klasse implementiert werden muss. Da der Controller der Anwendung eine MustInherit-Klasse ist, können nicht Sie nicht direkt in der Klasse definierten Methoden aufrufen. Wenn Sie versuchen, die Anwendungsserver-Klasse direkt aufzurufen. Klicken Sie dann erhalten eine Ressource wurde nicht gefunden-Fehlermeldung Sie.

Drittens: Beachten Sie, dass der Controller der Anwendung einen Konstruktor enthält, der die Liste der Film Kategorien zum Anzeigen von Daten hinzufügt. Jede Controllerklasse, die von der Anwendungscontroller erbt ruft Konstruktor des Controllers der Anwendung automatisch. Sobald Sie alle Aktionen auf jedem Controller aufrufen, die von der Anwendungscontroller erbt, die Film-Kategorien ist in der Ansicht automatisch eingeschlossen.

"Movies"-Controllers in Listing 5 erbt von der Anwendung-Controller.

**Codebeispiel 5: `Controllers\MoviesController.vb`**

[!code-vb[Main](passing-data-to-view-master-pages-vb/samples/sample5.vb)]

"Movies"-Controllers, genau wie im vorherigen Abschnitt erläutert den Home-Controller stellt zwei Aktionsmethoden `Index()` und `Details()`. Beachten Sie, dass die Liste der Film Kategorien angezeigt, die von der Masterseite für die Sicht nicht hinzugefügt werden, zum Anzeigen von Daten in einem der `Index()` oder `Details()` Methode. Da "Movies"-Controllers auf dem Controller für die Anwendung erbt, wird die Liste der Film Kategorien zum Anzeigen von Daten automatisch hinzugefügt.

Beachten Sie, dass diese Lösung zum Anzeigen von Daten für eine Sicht Masterseite hinzufügen, das Prinzip DRY (wiederhole Dich nicht) nicht verletzt. Der Code zum Hinzufügen der Liste der Film Kategorien zum Anzeigen von Daten an einem Speicherort enthalten ist: der Konstruktor für den Controller der Anwendung.

### <a name="summary"></a>Zusammenfassung

In diesem Tutorial erläutert zwei Ansätze zum Anzeigen von Daten von einem Controller an eine Masterseite für die Ansicht übergeben. Zunächst untersucht wir eine einfache, aber schwierig zu Ansatz zu verwalten. Im ersten Abschnitt wird erläutert, wie Sie Daten für eine Masterseite Ansicht anzeigen in jeder jede Controlleraktion in Ihrer Anwendung hinzufügen können. Wir kamen zu dem Schluss, dass dies einen ungültigen Ansatz war, weil es das DRY (wiederhole Dich nicht)-Prinzip verletzt.

Als Nächstes untersucht wir eine viel bessere Strategie zum Hinzufügen von Daten, die von einer Masterseite Ansicht zum Anzeigen von Daten erforderlich. Anstatt die Ansichtsdaten in jede Controlleraktion hinzugefügt haben, haben wir die Ansichtsdaten nur einmal in einem Application Controller hinzugefügt. Auf diese Weise können Sie doppelt vorhandenem Code vermeiden, für das Übergeben von Daten an eine Ansicht Masterseite in ASP.NET MVC-Anwendungen.

> [!div class="step-by-step"]
> [Vorherige](creating-page-layouts-with-view-master-pages-vb.md)
