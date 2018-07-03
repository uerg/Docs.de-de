---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-vb
title: Erstellen von benutzerdefinierten Routen (VB) | Microsoft-Dokumentation
author: microsoft
description: Erfahren Sie, wie Sie benutzerdefinierte Routen zu einer ASP.NET MVC-Anwendung hinzufügen. In diesem Tutorial erfahren Sie, wie die Standardroutingtabelle in der Datei "Global.asax" zu ändern.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: 6ac5758b-6199-42af-adcb-21954b864951
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-vb
msc.type: authoredcontent
ms.openlocfilehash: b2bca002434274e334509d21f6857e094d391198
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362017"
---
<a name="creating-custom-routes-vb"></a>Erstellen von benutzerdefinierten Routen (VB)
====================
durch [Microsoft](https://github.com/microsoft)

> Erfahren Sie, wie Sie benutzerdefinierte Routen zu einer ASP.NET MVC-Anwendung hinzufügen. In diesem Tutorial erfahren Sie, wie die Standardroutingtabelle in der Datei "Global.asax" zu ändern.


In diesem Tutorial erfahren Sie, wie eine benutzerdefinierte Route zu einer ASP.NET MVC-Anwendung hinzufügen. Erfahren Sie, wie die Standardroutingtabelle in der Datei "Global.asax" mit einer benutzerdefinierten Route ändern.

In ASP.NET MVC-Anwendungen funktioniert die Standardroutingtabelle. Allerdings könnten Sie feststellen, dass Sie Routinganforderungen spezielle. In diesem Fall können Sie eine benutzerdefinierte Route erstellen.

Stellen Sie sich vor, z. B., dass Sie eine Bloganwendung erstellen. Möglicherweise möchten die eingehenden Anforderungen zu verarbeiten, die wie folgt aussehen:

/ Archive/12-25-2009

Wenn ein Benutzer diese Anforderung eingibt, zurückgegeben werden soll den Blogeintrag, entspricht das Datum 12/25/2009. Um diese Art von Anforderung zu verarbeiten, müssen Sie eine benutzerdefinierte Route erstellen.

Die Datei "Global.asax" in Codebeispiel 1 enthält eine neue benutzerdefinierte Route, die mit dem Namen Blog, welche verarbeitet Anforderungen, die wie /Archive/ aussehen*Eingabedatum*.

**Codebeispiel 1 – "Global.asax" (mit benutzerdefinierten Route)**

[!code-vb[Main](creating-custom-routes-vb/samples/sample1.vb)]

Die Reihenfolge der Routen, die Sie, um die Routentabelle hinzufügen ist wichtig. Unsere neuen benutzerdefinierten Blog-Route wird vor der vorhandenen Standardroute hinzugefügt. Wenn Sie die Reihenfolge umgekehrt, wird Klicken Sie dann die Standardroute immer anstelle der benutzerdefinierten Route aufgerufen werden.

Die benutzerdefinierte Route für den Blog entspricht jede Anforderung, die mit/Archive/beginnt. Daher liegt eine Übereinstimmung mit allen der folgenden URLs:

- / Archive/12-25-2009

- / Archive/10-6-2004

- / Archive/apple

Die benutzerdefinierte Route ordnet die eingehende Anforderung an einen Controller mit dem Namen Archiv und die Entry() Aktion aufruft. Wenn die Entry()-Methode aufgerufen wird, wird das Eingabedatum als einen Parameter namens EntryDate übergeben.

Sie können die benutzerdefinierte Blog-Route mit dem Controller im Codebeispiel 2 verwenden.

**Codebeispiel 2 - ArchiveController.vb**

[!code-vb[Main](creating-custom-routes-vb/samples/sample2.vb)]

Beachten Sie, dass die Methode Entry() Programmausdruck 2 einen Parameter vom Typ "DateTime" akzeptiert. Das MVC-Framework ist intelligent genug, um das Datum des Eintrags aus der URL automatisch in einen DateTime-Wert konvertieren. Wenn der Eintrag Date-Parameter aus der URL in einen datetime-Wert konvertiert werden kann, wird ein Fehler ausgelöst (siehe Abbildung 1).

**Abbildung 1: Fehler in Parameter konvertieren**


[![Das Dialogfeld "Neues Projekt"](creating-custom-routes-vb/_static/image1.jpg)](creating-custom-routes-vb/_static/image1.png)

**Abbildung 01**: Fehler durch Konvertieren der Parameter ([klicken Sie, um das Bild in voller Größe anzeigen](creating-custom-routes-vb/_static/image2.png))


## <a name="summary"></a>Zusammenfassung

Das Ziel dieses Lernprogramms wurde veranschaulicht, wie Sie eine benutzerdefinierte Route erstellen können. Sie haben gelernt, wie eine benutzerdefinierte Route der Routingtabelle, in der Datei "Global.asax" hinzugefügt, der Blog-Einträge darstellt. Wir erläutert, wie Anforderungen für Blogeinträge auf einen Controller, die mit dem Namen ArchiveController und eine Controlleraktion, die mit dem Namen Entry() zuordnen.

> [!div class="step-by-step"]
> [Zurück](asp-net-mvc-controller-overview-vb.md)
> [Weiter](creating-a-route-constraint-vb.md)
