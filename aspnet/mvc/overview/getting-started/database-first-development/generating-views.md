---
uid: mvc/overview/getting-started/database-first-development/generating-views
title: 'EF Database First mit ASP.NET MVC: Generieren von Sichten | Microsoft-Dokumentation'
author: tfitzmac
description: Verwenden MVC, Entity Framework und ASP.NET-Gerüstbau, können Sie eine Webanwendung erstellen, die eine Schnittstelle für eine vorhandene Datenbank bereitstellt. Dieses Tutorial Seri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/29/2014
ms.topic: article
ms.assetid: 669367cf-8e30-4eb6-821d-10a7d9bb906c
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/database-first-development/generating-views
msc.type: authoredcontent
ms.openlocfilehash: 58f8be181f80f5b27353e9ef5cafe48bcb370e29
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399609"
---
<a name="ef-database-first-with-aspnet-mvc-generating-views"></a>EF Database First mit ASP.NET MVC: Generieren von Sichten
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> Verwenden MVC, Entity Framework und ASP.NET-Gerüstbau, können Sie eine Webanwendung erstellen, die eine Schnittstelle für eine vorhandene Datenbank bereitstellt. Dieser tutorialreihe erfahren Sie, wie Sie automatisch generierter Code, der ermöglicht Benutzern das anzeigen, bearbeiten, erstellen und Löschen von Daten, die in einer Datenbanktabelle gespeichert. Der generierte Code entspricht die Spalten in der Datenbanktabelle.
> 
> Dieser Teil der Serie konzentriert sich auf ASP.NET-Gerüstbau verwenden, um die Controller und Ansichten zu generieren.


## <a name="add-scaffold"></a>Gerüst hinzufügen

Sie können zum Generieren von Code, die standard-Vorgänge für die Modellklassen bereitstellt. Sie können den Code durch Hinzufügen eines Elements Gerüst hinzufügen. Es gibt viele Optionen für den Typ der Gerüstbau, die Sie hinzufügen können. In diesem Tutorial enthält das Gerüst, einen Controller und Ansichten, die die Modelle "Student" und der Registrierung entsprechen, die Sie im vorherigen Abschnitt erstellt haben.

Um Konsistenz in Ihrem Projekt zu gewährleisten, fügen Sie den neuen Controller hinzu, die vorhandene **Controller** Ordner. Mit der rechten Maustaste die **Controller** Ordner, und wählen **hinzufügen** – **neues Gerüstelement**.

![Gerüst hinzufügen](generating-views/_static/image1.png)

Wählen Sie die **MVC 5-Controller mit Ansichten unter Verwendung von Entity Framework** Option. Diese Option wird die Controller und Ansichten zum Aktualisieren, löschen, erstellen und Anzeigen der Daten in Ihrem Modell generiert.

![Hinzufügen der Mvc-controller](generating-views/_static/image2.png)

Wählen Sie **für Schüler und Studenten** für die Model-Klasse, und wählen die **ContosoUniversityEntities** für der Context-Klasse. Behalten Sie den Namen der Controller als **StudentsController**,

![Geben Sie controller](generating-views/_static/image3.png)

Klicken Sie auf **Hinzufügen**.

Wenn Sie eine Fehlermeldung erhalten, kann es sein, da Sie nicht auf das Projekt im vorherigen Abschnitt erstellt haben. Wenn dies der Fall ist, versuchen Sie es beim Erstellen des Projekts, und klicken Sie dann das erstellte Element erneut hinzufügen.

Nachdem der Prozess der codegenerierung abgeschlossen ist, werden Sie einen neuen Controller und Ansichten in Ihrem Projekt angezeigt.

![Ansichten anzeigen](generating-views/_static/image4.png)

Führen Sie die gleichen Schritte erneut aus, aber fügen Sie ein Gerüst für die Registrierung-Klasse hinzu. Wenn Sie fertig sind, müssen Sie eine **EnrollmentsController.cs** Datei sowie einen Ordner unter **Ansichten** mit dem Namen **Registrierungen** mit der Create, Delete, Details, bearbeiten und Index Ansichten.

![Ansichten anzeigen](generating-views/_static/image5.png)

## <a name="add-links-to-new-views"></a>Hinzufügen von Links zu den neuen Ansichten

Um für die Navigation zu Ihrer neuen Ansichten vereinfachen, können Sie eine Reihe von Links für Schüler/Studenten und Registrierungen, die den Index Ansichten hinzufügen. Öffnen Sie die Datei unter **Views/Home/Index.cshtml**, dies ist die Startseite für Ihre Website. Fügen Sie den folgenden Code unter den Jumbotron hinzu.

[!code-cshtml[Main](generating-views/samples/sample1.cshtml)]

Für die ActionLink-Methode ist der erste Parameter der im Link anzuzeigende Text. Der zweite Parameter ist die Aktion aus, und der dritte Parameter ist der Name des Controllers. Zeigt beispielsweise der erste Link zu der Aktion Index StudentsController an. Der tatsächlichen Link wird unter folgenden Werten erstellt. Die erste Verknüpfung letztendlich gelangen Benutzer zum die **"Index.cshtml"** Datei innerhalb der **Ansichten/Schüler/Studenten** Ordner.

## <a name="display-student-views"></a>Anzeigen von für Schüler und Studenten-Ansichten

Sie werden überprüfen, dass der Code ordnungsgemäß zu Ihrem Projekt hinzugefügt wird eine Liste der Studenten angezeigt, und Benutzern ermöglicht das Bearbeiten, erstellen oder löschen Sie die Studentendatensätze für Schüler und in der Datenbank.

Mit der rechten Maustaste die **Views/Home/Index.cshtml** , und wählen Sie **in Browser anzeigen**. Klicken Sie auf dieser Seite auf den Link, um die Liste der Schüler/Studenten.

![](generating-views/_static/image6.png)

Beachten Sie auf dieser Seite die Liste der Schüler/Studenten und Links zum Ändern dieser Daten ein.

![Liste der Schüler/Studenten](generating-views/_static/image7.png)

Klicken Sie auf die **neu erstellen** verknüpfen, und geben Sie einige Werte für einen neuen Studenten.

![Erstellen Sie neue für Schüler und Studenten](generating-views/_static/image8.png)

Klicken Sie auf **erstellen**, und beachten Sie, dass die neue Studenten zur Liste hinzugefügt wird.

![Liste mit den neuen Studenten](generating-views/_static/image9.png)

Wählen Sie die **bearbeiten** verknüpfen, und einige der Werte für Schüler/Student ändern.

![Kursteilnehmer bearbeiten](generating-views/_static/image10.png)

Klicken Sie auf **speichern**, und beachten Sie, dass der Student-Datensatz geändert wurde.

Wählen Sie abschließend die **löschen** verknüpfen, und bestätigen Sie den Datensatz zu löschen, indem Sie auf die **löschen** Schaltfläche.

![Löschen Sie "Student"](generating-views/_static/image11.png)

Ohne Code schreiben zu müssen, haben Sie die Sichten hinzugefügt, die allgemeine Vorgänge auf die Daten in der Tabelle "Student" ausführen.

Ihnen möglicherweise aufgefallen, dass das die textbezeichnung für ein Feld auf die Datenbankeigenschaft (z. B. **"LastName"**) dem ist nicht unbedingt auf der Webseite anzeigen möchten. Z. B. möglicherweise bevorzugen Sie die Bezeichnung sein **Nachname**. Sie werden später im Tutorial dieses Anzeigeproblems behoben.

## <a name="display-enrollment-views"></a>Anzeigen von Ansichten der Registrierung

Ihre Datenbank enthält eine 1: n Beziehung zwischen den Tabellen "Student" und Registrierung, und eine 1: n Beziehung zwischen den Tabellen Kurs- und Registrierungsentitäten. Die Ansichten für die Registrierung zu diese Beziehungen verarbeiten. Navigieren Sie zur Startseite für Ihre Website, und wählen die **Liste der Registrierungen** Link und klicken Sie dann die **neu erstellen** Link. Die Ansicht zeigt ein Formular zum Erstellen eines neuen Eintrags für die Registrierung. Beachten Sie insbesondere, dass das Formular mit zwei Dropdownlisten enthält, die mit Werten aus den verknüpften Tabellen aufgefüllt werden.

![Erstellen Sie die Registrierung](generating-views/_static/image12.png)

Darüber hinaus wird die Überprüfung der bereitgestellten Werte automatisch basierend auf dem Datentyp des Felds angewendet. Professionelle erfordert eine Zahl ist, damit eine Fehlermeldung angezeigt wird, wenn Sie versuchen, einen nicht kompatiblen Wert angeben.

![validierungsmeldung](generating-views/_static/image13.png)

Sie haben sichergestellt, dass die automatisch generierte Ansichten für die Arbeit mit den Daten in der Datenbank ermöglichen. Im nächsten Tutorial dieser Reihe Sie die Datenbank aktualisieren, und nehmen die entsprechenden Änderungen in der Webanwendung.

> [!div class="step-by-step"]
> [Zurück](creating-the-web-application.md)
> [Weiter](changing-the-database.md)
