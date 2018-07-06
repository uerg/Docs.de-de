---
uid: mvc/overview/getting-started/database-first-development/changing-the-database
title: 'EF Database First mit ASP.NET MVC: Ändern der Datenbank | Microsoft-Dokumentation'
author: tfitzmac
description: Verwenden MVC, Entity Framework und ASP.NET-Gerüstbau, können Sie eine Webanwendung erstellen, die eine Schnittstelle für eine vorhandene Datenbank bereitstellt. Dieses Tutorial Seri...
ms.author: aspnetcontent
ms.date: 10/01/2014
ms.assetid: cfd5c083-a319-482e-8f25-5b38caa93954
msc.legacyurl: /mvc/overview/getting-started/database-first-development/changing-the-database
msc.type: authoredcontent
ms.openlocfilehash: 0176274694426a527ed0862b5138919d4cf5c319
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37840939"
---
<a name="ef-database-first-with-aspnet-mvc-changing-the-database"></a>EF Database First mit ASP.NET MVC: Ändern der Datenbank
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> Verwenden MVC, Entity Framework und ASP.NET-Gerüstbau, können Sie eine Webanwendung erstellen, die eine Schnittstelle für eine vorhandene Datenbank bereitstellt. Dieser tutorialreihe erfahren Sie, wie Sie automatisch generierter Code, der ermöglicht Benutzern das anzeigen, bearbeiten, erstellen und Löschen von Daten, die in einer Datenbanktabelle gespeichert. Der generierte Code entspricht die Spalten in der Datenbanktabelle.
> 
> Dieser Teil der Serie konzentriert sich auf eine Aktualisierung vornehmen, um die Struktur der Datenbank und das Weitergeben von dieser Änderung in der Webanwendung.


## <a name="add-a-column"></a>Hinzufügen einer Spalte

Wenn Sie die Struktur einer Tabelle in der Datenbank aktualisieren, müssen Sie sicherstellen, dass die Änderung an das Datenmodell, Ansichten und Controller weitergegeben wird.

In diesem Tutorial werden Sie eine neue Spalte der Tabelle "Student", um den zweiten Vornamen der Student aufzuzeichnen hinzufügen. Um diese Spalte hinzuzufügen, öffnen Sie das Datenbankprojekt, und öffnen Sie die Datei Student.sql. Über Designer "oder" T-SQL-Code, fügen Sie eine Spalte, die mit dem Namen **MiddleName** , ein NVARCHAR(50) und NULL-Werte zulässt.

![Zweiter Vorname hinzufügen](changing-the-database/_static/image1.png)

Ihr Datenbankprojekt (oder F5) starten, um bereitstellen Sie diese Änderung in Ihre lokale Datenbank. Das neue Feld wird in der Tabelle hinzugefügt. Wenn Sie es in der Objekt-Explorer von SQL Server nicht angezeigt werden, klicken Sie auf die Schaltfläche "Aktualisieren", klicken Sie im Bereich.

![neue Spalte anzeigen](changing-the-database/_static/image2.png)

Die neue Spalte, die in der Tabelle der Datenbank vorhanden ist, aber derzeit nicht in der Modellklasse für Daten vorhanden. Sie müssen das Modell, um Ihre neue Spalte enthalten aktualisieren. In der **Modelle** Ordner die **ContosoModel.edmx** Datei in das Modelldiagramm anzuzeigen. Beachten Sie, dass das Modell "Student" nicht die MiddleName-Eigenschaft enthält. Mit der rechten Maustaste auf die Entwurfsoberfläche, und wählen **Modell aus der Datenbank aktualisieren**.

![Aktualisierungsmodell](changing-the-database/_static/image3.png)

Wählen Sie in der Update-Assistenten die **aktualisieren** Registerkarte und der **für Schüler und Studenten** Tabelle.

![Update-Assistenten](changing-the-database/_static/image4.png)

Klicken Sie auf **Fertig stellen**.

Nachdem der Aktualisierungsvorgang abgeschlossen ist, enthält das Datenbankdiagramm neuen **MiddleName** Eigenschaft. Speichern Sie die **ContosoModel.edmx** Datei. Sie müssen diese Datei für die neue Eigenschaft an weitergegeben speichern die **Student.cs** Klasse. Sie haben jetzt die Datenbank und das Modell aktualisiert.

Erstellen Sie die Projektmappe.

Die Ansichten enthalten noch leider nicht die neue Eigenschaft. Aktualisieren Sie die Ansichten haben Sie zwei Optionen: können Sie entweder erneut generieren die Ansichten von wieder hinzufügen Gerüst für die Klasse "Student", oder Sie können die neue Eigenschaft manuell hinzufügen, um Ihre vorhandenen Ansichten. In diesem Tutorial fügen Sie das Gerüst erneut aus, da Sie nicht benutzerdefinierten Änderungen an den automatisch generierten Ansichten vorgenommen haben. Sie sollten erwägen, die Eigenschaft manuell hinzufügen, wenn Sie Änderungen, in den Ansichten vorgenommen haben und nicht, diese Änderungen verloren möchten.

Um sicherzustellen, die Ansichten werden neu erstellt, löschen Sie die **Schüler/Studenten** Ordner unter **Ansichten**, und Löschen der **StudentsController**. Klicken Sie dann mit der rechten Maustaste die **Controller** Ordner und Hinzufügen der Gerüstbau für die **für Schüler und Studenten** Modell. Nennen Sie den Controller wieder **StudentsController**. Klicken Sie auf **OK**.

Die Ansichten enthalten jetzt die MiddleName-Eigenschaft.

![Zweiter Vorname anzeigen](changing-the-database/_static/image5.png)

Im nächsten Abschnitt fügen Sie Code zum Anpassen der Ansicht zum Anzeigen von Details zu einem Datensatz für Schüler und Studenten.

> [!div class="step-by-step"]
> [Zurück](generating-views.md)
> [Weiter](customizing-a-view.md)
