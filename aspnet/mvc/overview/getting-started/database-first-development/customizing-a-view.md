---
uid: mvc/overview/getting-started/database-first-development/customizing-a-view
title: 'EF Database First mit ASP.NET MVC: Anpassen einer Ansicht | Microsoft-Dokumentation'
author: tfitzmac
description: Verwenden MVC, Entity Framework und ASP.NET-Gerüstbau, können Sie eine Webanwendung erstellen, die eine Schnittstelle für eine vorhandene Datenbank bereitstellt. Dieses Tutorial Seri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: 269380ff-d7e1-4035-8ad1-fe1316a25f76
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/database-first-development/customizing-a-view
msc.type: authoredcontent
ms.openlocfilehash: bfbcfd39dd1cf0abe89a00d2958ca010f0e5e109
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37376497"
---
<a name="ef-database-first-with-aspnet-mvc-customizing-a-view"></a>EF Database First mit ASP.NET MVC: Anpassen einer Ansicht
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> Verwenden MVC, Entity Framework und ASP.NET-Gerüstbau, können Sie eine Webanwendung erstellen, die eine Schnittstelle für eine vorhandene Datenbank bereitstellt. Dieser tutorialreihe erfahren Sie, wie Sie automatisch generierter Code, der ermöglicht Benutzern das anzeigen, bearbeiten, erstellen und Löschen von Daten, die in einer Datenbanktabelle gespeichert. Der generierte Code entspricht die Spalten in der Datenbanktabelle.
> 
> Dieser Teil der Serie konzentriert sich auf die automatisch generierten Ansichten zur Verbesserung der Präsentation zu ändern.


## <a name="add-enrolled-courses-to-student-details"></a>Registrierte Kurse für Schüler und Studenten Details hinzufügen

Der generierte Code bietet einen guten Ausgangspunkt für Ihre Anwendung bietet jedoch nicht unbedingt alle Funktionen, die Sie in Ihrer Anwendung benötigen. Sie können den Code, um die bestimmten Anforderungen Ihrer Anwendung anpassen. Die Anwendung wird derzeit nicht die registrierten Kurse für den ausgewählten Studenten angezeigt. In diesem Abschnitt fügen Sie die registrierten Kurse für jeden Kursteilnehmer auf die **Details** Ansicht für den Studenten.

Öffnen **Students/Details.cshtml**, und klicken Sie unterhalb der letzten &lt;/DL&gt; Registerkarte aber vor dem schließenden &lt;/div&gt; markieren, fügen Sie den folgenden Code hinzu.

[!code-cshtml[Main](customizing-a-view/samples/sample1.cshtml)]

Dieser Code erstellt eine Tabelle, die eine Zeile für jeden Datensatz in der Enrollment-Tabelle für den ausgewählten Studenten angezeigt. Die **Anzeige** -Methode rendert die HTML für das Objekt (ModelItem), die den Ausdruck darstellt. Verwenden Sie die Anzeige-Methode (anstatt einfach den Wert der Eigenschaft im Code) um sicherzustellen, dass der Wert ist ordnungsgemäß basierend auf den Typ und die Vorlage für diesen Typ formatiert. In diesem Beispiel jeder Ausdruck gibt eine einzelne Eigenschaft des aktuellen Datensatzes in der Schleife und die Werte sind primitive Typen, die als Text gerendert werden.

Navigieren Sie erneut an die Schüler/Studenten/Index-Ansicht aus, und wählen Sie **Details** für eines der Schüler/Studenten. Sie sehen, dass die registrierten Kurse in der Ansicht hinzugefügt wurden.

!["Student" mit Registrierung](customizing-a-view/_static/image1.png)

> [!div class="step-by-step"]
> [Zurück](changing-the-database.md)
> [Weiter](enhancing-data-validation.md)
