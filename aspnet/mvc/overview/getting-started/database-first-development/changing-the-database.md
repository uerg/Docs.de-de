---
uid: mvc/overview/getting-started/database-first-development/changing-the-database
title: 'EF Datenbank zuerst mit ASP.NET MVC: Ändern der Datenbank | Microsoft Docs'
author: tfitzmac
description: MVC, Entity Framework und ASP.NET Gerüstbau verwenden, können Sie eine Webanwendung erstellen, die eine Schnittstelle zu einer vorhandenen Datenbank bereitstellt. Dieses Lernprogramm Seri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: cfd5c083-a319-482e-8f25-5b38caa93954
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/changing-the-database
msc.type: authoredcontent
ms.openlocfilehash: 63ee8768a43dbdac80922e3adbedd3378c10da73
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879321"
---
<a name="ef-database-first-with-aspnet-mvc-changing-the-database"></a>EF Datenbank zuerst mit ASP.NET MVC: Ändern der Datenbank
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> MVC, Entity Framework und ASP.NET Gerüstbau verwenden, können Sie eine Webanwendung erstellen, die eine Schnittstelle zu einer vorhandenen Datenbank bereitstellt. Diese Reihe von Lernprogrammen wird gezeigt, wie automatisch generieren von Code, der ermöglicht Benutzern das anzeigen, bearbeiten, erstellen und Löschen von Daten, die in einer Datenbanktabelle gespeichert. Der generierte Code entspricht den Spalten in der Datenbanktabelle.
> 
> In diesem Teil der Reihe konzentriert sich auf ein Update auf die Struktur der Datenbank herstellen und das Weitergeben von dieser Änderung in der gesamten Anwendung.


## <a name="add-a-column"></a>Hinzufügen einer Spalte

Wenn Sie die Struktur einer Tabelle in der Datenbank aktualisieren, müssen Sie sicherstellen, dass die Änderung an das Datenmodell, Ansichten und Controller weitergegeben wird.

Für dieses Lernprogramm fügen Sie eine neue Spalte der Tabelle Student zum Aufzeichnen der zweite Vorname des Studenten hinzu. Um diese Spalte hinzuzufügen, öffnen Sie das Datenbankprojekt, und öffnen Sie die Datei Student.sql. Über den Designer oder den T-SQL-Code, fügen Sie eine Spalte mit dem Namen **MiddleName** , ist ein NVARCHAR(50) und NULL-Werte zulässt.

![Zweiter Vorname hinzufügen](changing-the-database/_static/image1.png)

Bereitstellen Sie diese Änderung können Sie auf die lokale Datenbank, indem Sie Ihr Datenbankprojekt (oder F5) starten. Die Tabelle wird das neue Feld hinzugefügt. Wenn Sie sie im Objekt-Explorer von SQL Server nicht angezeigt werden, klicken Sie auf die Schaltfläche "Aktualisieren" im Bereich.

![neue Spalte anzeigen](changing-the-database/_static/image2.png)

Die neue Spalte in der Datenbanktabelle vorhanden ist, aber derzeit nicht in der Datenmodellklasse vorhanden. Sie müssen das Modell, um die neue Spalte berücksichtigt aktualisieren. In der **Modelle** Ordner die **ContosoModel.edmx** Datei, um das Modelldiagramm anzuzeigen. Beachten Sie, dass das Modell Student nicht die MiddleName-Eigenschaft enthält. Mit der rechten Maustaste auf die Entwurfsoberfläche, und wählen Sie **Modell aus der Datenbank aktualisieren**.

![Modell aktualisieren](changing-the-database/_static/image3.png)

Wählen Sie in der Update-Assistent die **aktualisieren** Registerkarte und der **Student** Tabelle.

![Assistent zum Aktualisieren](changing-the-database/_static/image4.png)

Klicken Sie auf **Fertig stellen**.

Nach Abschluss des Updatevorgangs Datenbankdiagramm enthält das neue **MiddleName** Eigenschaft. Speichern Sie die **ContosoModel.edmx** Datei. Sie speichern müssen Sie diese Datei für die neue Eigenschaft an die **Student.cs** Klasse. Sie haben jetzt die Datenbank und das Modell aktualisiert.

Erstellen Sie die Projektmappe.

Die Ansichten enthalten noch leider nicht die neue Eigenschaft. Aktualisiert die Ansichten stehen Ihnen zwei Optionen zur Verfügung: Sie können entweder erneut generieren die Ansichten des Gerüstbaus für die Klasse Student erneut hinzufügen, oder Sie können die neue Eigenschaft manuell hinzufügen, um Ihre vorhandenen Ansichten. In diesem Lernprogramm fügen Sie das Gerüst erneut aus, da Sie keine benutzerdefinierten Änderungen an den automatisch generierten Sichten vorgenommen haben. Sie sollten erwägen, die Eigenschaft manuell hinzufügen, wenn Sie Änderungen, auf die Ansichten vorgenommen haben und nicht diese Änderungen verlieren möchten.

Um sicherzustellen, dass die Ansichten neu erstellt wurde, löschen Sie die **Studenten** Unterordner **Ansichten**, und löschen die **StudentsController**. Klicken Sie dann mit der rechten Maustaste die **Controller** Ordner und Hinzufügen des Gerüstbaus für die **Student** Modell. Nennen Sie den Controller wieder **StudentsController**. Klicken Sie auf **OK**.

Die Ansichten enthalten jetzt die MiddleName-Eigenschaft.

![Zweiter Vorname anzeigen](changing-the-database/_static/image5.png)

Im nächsten Abschnitt fügen Sie Code aus, um die Ansicht zum Anzeigen von Details zu einem Datensatz Student anzupassen.

> [!div class="step-by-step"]
> [Zurück](generating-views.md)
> [Weiter](customizing-a-view.md)
