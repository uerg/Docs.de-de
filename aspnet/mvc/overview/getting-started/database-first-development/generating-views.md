---
uid: mvc/overview/getting-started/database-first-development/generating-views
title: 'EF Datenbank zuerst mit ASP.NET MVC: Generieren von Sichten | Microsoft Docs'
author: tfitzmac
description: MVC, Entity Framework und ASP.NET Gerüstbau verwenden, können Sie eine Webanwendung erstellen, die eine Schnittstelle zu einer vorhandenen Datenbank bereitstellt. Dieses Lernprogramm Seri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/29/2014
ms.topic: article
ms.assetid: 669367cf-8e30-4eb6-821d-10a7d9bb906c
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/generating-views
msc.type: authoredcontent
ms.openlocfilehash: b60e89a187a879255eb051dc87241714cef6fa63
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879750"
---
<a name="ef-database-first-with-aspnet-mvc-generating-views"></a>EF Datenbank zuerst mit ASP.NET MVC: Generieren von Sichten
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> MVC, Entity Framework und ASP.NET Gerüstbau verwenden, können Sie eine Webanwendung erstellen, die eine Schnittstelle zu einer vorhandenen Datenbank bereitstellt. Diese Reihe von Lernprogrammen wird gezeigt, wie automatisch generieren von Code, der ermöglicht Benutzern das anzeigen, bearbeiten, erstellen und Löschen von Daten, die in einer Datenbanktabelle gespeichert. Der generierte Code entspricht den Spalten in der Datenbanktabelle.
> 
> In diesem Teil der Reihe konzentriert sich auf die mithilfe von ASP.NET Gerüstbau der Controller und Ansichten generieren.


## <a name="add-scaffold"></a>Gerüst hinzufügen

Sie sind bereit, um Code zu generieren, die Standarddaten Vorgänge für das Modellklassen bereitstellt. Sie fügen Code hinzu, indem Sie ein Gerüst-Element. Es gibt viele Optionen für den Typ des Gerüstbau, die Sie hinzufügen können. In diesem Lernprogramm umfasst das Gerüst einen Controller und Ansichten, die den Modellen Student "und" Registrierung entsprechen, die Sie im vorherigen Abschnitt erstellt haben.

Um die Konsistenz in Ihrem Projekt zu wahren, werden Sie Hinzufügen eines neuen Controllers zur vorhandenen **Controller** Ordner. Mit der rechten Maustaste die **Controller** Ordner, und wählen **hinzufügen** – **neues Gerüstelement**.

![Gerüst hinzufügen](generating-views/_static/image1.png)

Wählen Sie die **MVC 5-Controller mit Ansichten unter Verwendung von Entity Framework** Option. Diese Option wird der Controller und Ansichten zum Aktualisieren, löschen, erstellen und Anzeigen der Daten in Ihrem Modell generiert.

![Hinzufügen von Mvc-controller](generating-views/_static/image2.png)

Wählen Sie **Student** der Skriptobjektmodell-Klasse, und wählen die **ContosoUniversityEntities** für der Context-Klasse. Behalten Sie den Namen des Controllers als **StudentsController**,

![Geben Sie die controller](generating-views/_static/image3.png)

Klicken Sie auf **Hinzufügen**.

Wenn Sie eine Fehlermeldung erhalten, möglicherweise darauf zurückzuführen, bis Sie nicht auf das Projekt im vorherigen Abschnitt erstellt. Wenn dies der Fall ist, versuchen Sie es beim Erstellen des Projekts, und fügen Sie das Gerüst erneut hinzu.

Nachdem der Code-Generierungsprozess abgeschlossen ist, werden Sie einen neuen Controller und Ansichten in Ihrem Projekt angezeigt.

![Ansichten anzeigen](generating-views/_static/image4.png)

Führen Sie dieselben Schritte erneut aus, aber fügen Sie ein Gerüst für die Registrierung Klasse hinzu. Wenn Sie fertig sind, müssen Sie eine **EnrollmentsController.cs** Datei sowie einen Ordner unter **Ansichten** mit dem Namen **Registrierung** mit der Create, Delete, Details, bearbeiten und Index Ansichten.

![Ansichten anzeigen](generating-views/_static/image5.png)

## <a name="add-links-to-new-views"></a>Hinzufügen von Links mit neuen Ansichten

Um Sie zu Ihrer neuen Ansichten navigieren erleichtern, können Sie eine Reihe von Hyperlinks, die den Index Ansichten für Studenten und Registrierung hinzufügen. Öffnen Sie die Datei am **Views/Home/Index.cshtml**, also auf der Startseite für die Website. Fügen Sie den folgenden Code unter der Jumbotron hinzu.

[!code-cshtml[Main](generating-views/samples/sample1.cshtml)]

Für die Methode ActionLink ist der erste Parameter im Bereich anzuzeigende Text. Der zweite Parameter ist die Aktion, und der dritte Parameter ist der Name des Controllers. Zeigt beispielsweise der erste Link, um die Index-Aktion in StudentsController. Die tatsächliche Hyperlink wird aus dieser Werte erstellt. Der erste Link gelangen letztlich Benutzer auf die **Index.cshtml** innerhalb der **Ansichten/Studenten** Ordner.

## <a name="display-student-views"></a>Student-Ansichten angezeigt

Sie überprüft, ob der Code ordnungsgemäß zu Ihrem Projekt hinzugefügt wird eine Liste der Studenten angezeigt, und ermöglicht Benutzern das Bearbeiten, erstellen oder löschen Sie die Studentendatensätze in der Datenbank.

Mit der rechten Maustaste die **Views/Home/Index.cshtml** Datei, und wählen Sie **in Browser anzeigen**. Klicken Sie auf dieser Seite auf den Link, um die Liste der Schüler.

![](generating-views/_static/image6.png)

Beachten Sie auf dieser Seite die Liste der Schüler und Links zum Ändern dieser Daten ein.

![Liste der Schüler](generating-views/_static/image7.png)

Klicken Sie auf die **neu erstellen** verknüpfen, und geben Sie einige Werte für einen neuen Studenten.

![Erstellen von neuen Studenten](generating-views/_static/image8.png)

Klicken Sie auf **erstellen**, und beachten Sie die neue Studenten wird zur Liste hinzugefügt.

![Liste mit neuen Studenten](generating-views/_static/image9.png)

Wählen Sie die **bearbeiten** verknüpfen, und einige der Werte für ein Student ändern.

![Student bearbeiten](generating-views/_static/image10.png)

Klicken Sie auf **speichern**, und beachten Sie die Student-Datensatz wurde geändert.

Wählen Sie abschließend die **löschen** verknüpfen, und vergewissern Sie sich, dass den Datensatz gelöscht werden sollen die **löschen** Schaltfläche.

![Schüler löschen](generating-views/_static/image11.png)

Ohne Code schreiben zu müssen, haben Sie Sichten hinzugefügt, die allgemeine Vorgänge auf die Daten in der Tabelle Student ausführen.

Ihnen möglicherweise aufgefallen, dass die Beschriftung für ein Feld auf die Datenbankeigenschaft basiert (z. B. **LastName**) die ist nicht notwendigerweise was auf der Webseite angezeigt werden soll. Z. B. möglicherweise bevorzugen Sie die Bezeichnung sein **Nachname**. Dieses Problem korrigieren Sie später in diesem Lernprogramm.

## <a name="display-enrollment-views"></a>Registrierung Ansichten angezeigt

Ihre Datenbank enthält eine 1: n-Beziehung zwischen den Tabellen Student "und" Registrierung, und eine 1: n-Beziehung zwischen den Tabellen Kurs und Registrierung. Die Ansichten für die Registrierung zu verarbeiten diese Beziehungen. Navigieren Sie zu der Homepage Ihrer Website, und wählen die **Liste der Bereitstellungen** Verknüpfung und dann die **neu erstellen** Link. Die Ansicht zeigt ein Formular für das Erstellen eines neuen Datensatzes für die Registrierung. Beachten Sie insbesondere, dass das Formular zwei Dropdownlisten enthält, die mit Werten aus den verknüpften Tabellen aufgefüllt werden.

![Erstellen Sie die Registrierung](generating-views/_static/image12.png)

Darüber hinaus wird die Überprüfung der bereitgestellten Werte automatisch basierend auf den Datentyp des Felds angewendet. Grade fordert eine Nummer an, damit eine Fehlermeldung angezeigt wird, wenn Sie versuchen, einen inkompatiblen Wert angeben.

![überprüfungsmeldung](generating-views/_static/image13.png)

Sie haben sichergestellt, dass die automatisch generierten Sichten ermöglichen einen Benutzer mit den Daten in der Datenbank arbeiten. In der nächsten Lernprogramm dieser Reihe Sie die Datenbank aktualisieren, und nehmen Sie die entsprechenden Änderungen in der Webanwendung.

> [!div class="step-by-step"]
> [Zurück](creating-the-web-application.md)
> [Weiter](changing-the-database.md)
