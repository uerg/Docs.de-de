---
title: "Abhängigkeitsinjektion in Sichten"
author: ardalis
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 80fb9e43-e4db-4af2-b2a8-e1364a712f69
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/dependency-injection
ms.openlocfilehash: ff3ded36a04fdbba0628dc5f223bfd865d58612a
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2017
---
# <a name="dependency-injection-into-views"></a>Abhängigkeitsinjektion in Sichten

Durch [Steve Smith](https://ardalis.com/)

ASP.NET Core unterstützt [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection) in Sichten. Dies kann hilfreich für Ansicht-spezifische Dienste, z. B. Lokalisierung oder Daten, die nur für das Auffüllen der Ansichtselemente erforderlich sein. Sollten Sie versuchen, verwalten [Trennung von Anliegen](http://deviq.com/separation-of-concerns/) zwischen dem Controller und Ansichten. Die meisten Daten, die Ihre Ansichten anzeigen sollte auf dem Controller übergeben werden.

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/dependency-injection/sample)

## <a name="a-simple-example"></a>Ein einfaches Beispiel

Können Sie einen Dienst einfügen, in einer Ansicht mit der `@inject` Richtlinie. Sie können sich vorstellen `@inject` als Hinzufügen einer Eigenschaft, um die Ansicht aus, und füllen die Eigenschaft, die mithilfe der Abhängigkeitsinjektion.

Die Syntax für `@inject`:`@inject <type> <name>`

Ein Beispiel für `@inject` in Aktion:

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/ToDo/Index.cshtml?highlight=4,5,15,16,17)]

Diese Ansicht zeigt eine Liste von `ToDoItem` zusammen mit einer Zusammenfassung darüber angezeigt, Allgemeine Statistik-Instanzen. Die Zusammenfassung wird aufgefüllt, von der injizierten `StatisticsService`. Dieser Dienst ist für die Abhängigkeitsinjektion in registriert `ConfigureServices` in *Startup.cs*:

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Startup.cs?highlight=6,7&range=15-22)]

Die `StatisticsService` führt einige Berechnungen auf den Satz von `ToDoItem` Instanzen, die sie über ein Repository zugreift:

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/StatisticsService.cs?highlight=15,20,26)]

Das beispielrepository verwendet eine speicherinterne Auflistung. Die Implementierung, die oben gezeigte (das auf alle Daten im Arbeitsspeicher ausgeführt wird) wird nicht empfohlen für große, Remote zugegriffen Datasets.

Das Beispiel zeigt Daten aus das Modell, das an die Sicht gebunden ist und der Dienst in der Sicht eingefügt:

![Zum Auflisten von insgesamt Elementen anzeigen abgeschlossen Elemente, die durchschnittliche Priorität und eine Liste von Aufgaben mit den jeweiligen Prioritätsstufen und boolesche Werte, der angibt, Abschluss.](dependency-injection/_static/screenshot.png)

## <a name="populating-lookup-data"></a>Suchen von Daten auffüllen

Ansicht Injection kann zum Auffüllen der Optionen im UI-Elemente wie Dropdownlisten hilfreich sein. Betrachten Sie ein Benutzerformular für das Profil, das Optionen zum Angeben von Geschlecht, Status und andere Einstellungen enthält. Rendern eines Formulars, die von mit einer standard-MVC-Ansatz, müsste den Controller anfordern Datenzugriffsdienste für jede dieser Gruppen von Optionen, und füllen Sie ein Modell oder `ViewBag` mit jeder Satz von Optionen zum gebunden werden.

Ein alternativer Ansatz fügt Dienste direkt in der Ansicht, um die Optionen zu erhalten. Dies minimiert den Umfang des Codes, die vom Controller, verschieben diese Ansicht Element Konstruktion Logik in die Sicht selbst erforderlich sind. Controlleraktion anzuzeigenden ein Formular zum Bearbeiten des Profils muss nur dem Formular die Profilinstanz übergeben:

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Controllers/ProfileController.cs?highlight=9,19)]

Das HTML-Formular verwendet, um diese Einstellungen aktualisieren enthält Dropdownlisten für drei Eigenschaften:

![Aktualisieren Sie Profil-Ansicht mit einem Formular die Eingabe von Name, Geschlecht, Status und Lieblingsfarbe.](dependency-injection/_static/updateprofile.png)

Diese Listen werden von einem Dienst aufgefüllt, die in der Sicht eingefügt wurde:

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Profile/Index.cshtml?highlight=4,16,17,21,22,26,27)]

Die `ProfileOptionsService` ist ein UI-Level-Dienst bieten nur die Daten, die für dieses Formular benötigt:

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/ProfileOptionsService.cs?highlight=7,13,24)]

>[!TIP]
> Vergessen Sie nicht, um Typen zu registrieren, Sie über die Abhängigkeitsinjektion in fordert, der `ConfigureServices` Methode im *Startup.cs*.

## <a name="overriding-services"></a>Überschreiben von Diensten

Zusätzlich zu den Räumen neue Dienste, werden dieses Verfahren auch zum Überschreiben von zuvor eingefügte Services auf einer Seite verwendet. Die folgende Abbildung zeigt alle verfügbaren Felder auf der Seite, die im ersten Beispiel verwendet:

![IntelliSense-Kontextmenü für eine typisierte @-Symbol Html, Komponente, StatsService und Url-Felder auflisten](dependency-injection/_static/razor-fields.png)

Wie Sie sehen können, sind die Standardfelder `Html`, `Component`, und `Url` (als auch die `StatsService` , die wir eingefügten). Wenn Sie z. B. der Standard-HTML-Hilfsmethoden durch eigene ersetzen möchten, Sie können problemlos dazu `@inject`:

[!code-html[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Helper/Index.cshtml?highlight=3,11)]

Wenn Sie vorhandene Dienste erweitern möchten, können Sie dieses Verfahren einfach beim erben von oder die vorhandene Implementierung durch eigene wrapping verwenden.

## <a name="see-also"></a>Siehe auch

* Simon Timms Blog: [Suchdaten in Ihrer Ansicht abrufen](http://blog.simontimms.com/2015/06/09/getting-lookup-data-into-you-view/)
