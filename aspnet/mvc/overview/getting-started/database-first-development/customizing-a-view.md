---
uid: mvc/overview/getting-started/database-first-development/customizing-a-view
title: 'EF Datenbank zuerst mit ASP.NET MVC: Anpassen einer Ansicht | Microsoft Docs'
author: tfitzmac
description: "MVC, Entity Framework und ASP.NET Gerüstbau verwenden, können Sie eine Webanwendung erstellen, die eine Schnittstelle zu einer vorhandenen Datenbank bereitstellt. Dieses Lernprogramm Seri..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: 269380ff-d7e1-4035-8ad1-fe1316a25f76
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/customizing-a-view
msc.type: authoredcontent
ms.openlocfilehash: af9609396cff18b08824732731ddb9c5cca578fa
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="ef-database-first-with-aspnet-mvc-customizing-a-view"></a>EF Datenbank zuerst mit ASP.NET MVC: Anpassen einer Ansicht
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> MVC, Entity Framework und ASP.NET Gerüstbau verwenden, können Sie eine Webanwendung erstellen, die eine Schnittstelle zu einer vorhandenen Datenbank bereitstellt. Diese Reihe von Lernprogrammen wird gezeigt, wie automatisch generieren von Code, der ermöglicht Benutzern das anzeigen, bearbeiten, erstellen und Löschen von Daten, die in einer Datenbanktabelle gespeichert. Der generierte Code entspricht den Spalten in der Datenbanktabelle.
> 
> In diesem Teil der Reihe konzentriert sich auf das Ändern der Ansichten automatisch generiert, um die Lesbarkeit zu verbessern.


## <a name="add-enrolled-courses-to-student-details"></a>Registrierte Kurse zu Student Details hinzufügen

Der generierte Code bietet einen guten Ausgangspunkt für Ihre Anwendung jedoch nicht notwendigerweise bietet alle Funktionen, die Sie in Ihrer Anwendung benötigen. Sie können den Code, um die bestimmte Anforderungen der Anwendung anpassen. Die Anwendung wird derzeit nicht registrierte Kurse für den ausgewählten Schüler angezeigt. In diesem Abschnitt fügen Sie die registrierten Kurse für Studenten, die **Details** Ansicht für die Studenten.

Open **Students/Details.cshtml**, und unterhalb der letzten &lt;/DL&gt; Registerkarte, aber vor dem schließenden &lt;/div&gt; kennzeichnen, fügen Sie den folgenden Code hinzu.

[!code-cshtml[Main](customizing-a-view/samples/sample1.cshtml)]

Dieser Code erstellt eine Tabelle, die eine Zeile für jeden Datensatz in der Enrollment-Tabelle für die ausgewählten Studenten angezeigt. Die **Anzeige** Methode rendert die HTML für das Objekt (ModelItem), die den Ausdruck darstellt. Verwenden Sie die Anzeige-Methode (statt einfach einbetten den Eigenschaftswert im Code) um sicherzustellen, dass der Wert wird formatiert ordnungsgemäß basierend auf den Typ und die Vorlage für diesen Typ. In diesem Beispiel wird jeder Ausdruck gibt eine einzelne Eigenschaft aus dem aktuellen Datensatz in der Schleife und die Werte sind primitive Typen, die als Text gerendert werden.

Navigieren Sie zu der Studenten/Indexansicht erneut aus, und wählen Sie **Details** für eines der Studenten. Sie sehen, dass die registrierten Kurse in der Ansicht enthalten sind.

![Studenten, die bei der Registrierung](customizing-a-view/_static/image1.png)

>[!div class="step-by-step"]
[Zurück](changing-the-database.md)
[Weiter](enhancing-data-validation.md)
