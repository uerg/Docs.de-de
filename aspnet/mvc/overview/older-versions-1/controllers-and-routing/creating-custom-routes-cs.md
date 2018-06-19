---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-cs
title: Erstellen von benutzerdefinierten Routen (c#) | Microsoft Docs
author: microsoft
description: Weitere Informationen Sie zum Hinzufügen von benutzerdefinierter Routen zu einer ASP.NET MVC-Anwendung. In diesem Lernprogramm erfahren Sie, wie die Tabelle mit den standardmäßigen Route in der Datei "Global.asax" ändern.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: 3cd08f02-8763-490a-b625-2ac96a24b73f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-cs
msc.type: authoredcontent
ms.openlocfilehash: 573b6a3360124feea92788ff7a3de363840fa1ef
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30867966"
---
<a name="creating-custom-routes-c"></a>Erstellen von benutzerdefinierten Routen (c#)
====================
durch [Microsoft](https://github.com/microsoft)

> Weitere Informationen Sie zum Hinzufügen von benutzerdefinierter Routen zu einer ASP.NET MVC-Anwendung. In diesem Lernprogramm erfahren Sie, wie die Tabelle mit den standardmäßigen Route in der Datei "Global.asax" ändern.


In diesem Lernprogramm erfahren Sie, wie eine benutzerdefinierte Route zu einer ASP.NET MVC-Anwendung hinzufügen. Erfahren Sie, wie die Tabelle mit den standardmäßigen Route in der Datei "Global.asax" mit einer benutzerdefinierten Route zu ändern.

Viele einfache ASP.NET MVC-Anwendungen funktioniert die Standard-Routingtabelle gut. Jedoch könnten Sie feststellen, dass Sie routing benötigt spezielle. In diesem Fall können Sie eine benutzerdefinierte Route erstellen.

Stellen Sie sich vor, z. B., dass Sie eine Bloganwendung erstellen. Möglicherweise möchten die eingehende Anforderungen zu verarbeiten, die wie folgt aussehen:

/ Archiv/12-25-2009

Wenn ein Benutzer diese Anforderung eingibt, zurückgegeben werden soll den Blogeintrag, entspricht dem Datum 12/25/2009. Um diese Art der Anforderung zu verarbeiten, müssen Sie eine benutzerdefinierte Route erstellen.

Die Datei "Global.asax" im Codebeispiel 1 enthält eine neue benutzerdefinierte Route, die mit dem Namen Blog verwenden, welche Handles-Anforderungen, die wie /Archive/ aussehen*Eingabedatum*.

**Auflisten von 1 – "Global.asax" (mit benutzerdefinierten Route)**

[!code-csharp[Main](creating-custom-routes-cs/samples/sample1.cs)]

Die Reihenfolge der Routen, die Sie, um die Routentabelle hinzufügen ist wichtig. Bevor Sie die vorhandene Standardroute wird unsere neue benutzerdefinierte Blog-Route hinzugefügt. Wenn Sie die Reihenfolge umgekehrt, und klicken Sie dann die Standardroute anstelle der benutzerdefinierten Route immer aufgerufen wird.

Die benutzerdefinierte Blog Route entspricht jede Anforderung, die mit/Archiv/beginnt. Deshalb liegt eine Übereinstimmung mit allen folgenden URLs:

- / Archiv/12-25-2009

- / Archiv/10-6-2004

- / Archiv/apple

Die benutzerdefinierte Route ordnet die eingehende Anforderung an einen Controller mit dem Namen Archiv und ruft die Entry()-Aktion. Wenn die Entry()-Methode aufgerufen wird, wird das Datum des Eintrags als einen Parameter namens EntryDate übergeben.

Sie können die benutzerdefinierte Blog-Route mit dem Codebeispiel 2-Controller verwenden.

**Auflisten von 2 – ArchiveController.cs**

[!code-csharp[Main](creating-custom-routes-cs/samples/sample2.cs)]

Beachten Sie, dass die Entry()-Methode im Codebeispiel 2 einen Parameter vom Typ "DateTime" akzeptiert. Das MVC-Framework ist intelligent genug, um das Eingabedatum aus der URL automatisch in einen DateTime-Wert konvertieren. Wenn der Eintrag Date-Parameter von der URL in einen datetime-Wert konvertiert werden kann, wird ein Fehler ausgelöst (siehe Abbildung 1).

**Abbildung 1: Fehler aus der Konvertierung der parameter**


[![Das Dialogfeld "Neues Projekt"](creating-custom-routes-cs/_static/image1.jpg)](creating-custom-routes-cs/_static/image1.png)

**Abbildung 01**: Fehler aus der Konvertierung der Parameter ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-custom-routes-cs/_static/image2.png))


## <a name="summary"></a>Zusammenfassung

Das Ziel dieses Lernprogramms wurde veranschaulicht, wie Sie eine benutzerdefinierte Route erstellen können. Sie haben gelernt, wie eine benutzerdefinierte Route die Routingtabelle in der Datei "Global.asax" hinzugefügt, die Blogeinträgen darstellt. Zuordnen von Anforderungen für Blogeinträgen zu einem Controller namens ArchiveController und eine Controlleraktion mit dem Namen Entry() besprochen.

> [!div class="step-by-step"]
> [Zurück](aspnet-mvc-controllers-overview-cs.md)
> [Weiter](creating-a-route-constraint-cs.md)
