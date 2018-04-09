---
uid: mvc/overview/getting-started/database-first-development/creating-the-web-application
title: 'EF Datenbank zuerst mit ASP.NET MVC: Erstellen der Webanwendung und Datenmodelle | Microsoft Docs'
author: tfitzmac
description: MVC, Entity Framework und ASP.NET Gerüstbau verwenden, können Sie eine Webanwendung erstellen, die eine Schnittstelle zu einer vorhandenen Datenbank bereitstellt. Dieses Lernprogramm Seri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: bc8f2bd5-ff57-4dcd-8418-a5bd517d8953
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/creating-the-web-application
msc.type: authoredcontent
ms.openlocfilehash: 04ccc00fa48702608fdc7b5b00d73778985852f9
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="ef-database-first-with-aspnet-mvc-creating-the-web-application-and-data-models"></a>EF Datenbank zuerst mit ASP.NET MVC: Erstellen der Webanwendung und Datenmodelle
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> MVC, Entity Framework und ASP.NET Gerüstbau verwenden, können Sie eine Webanwendung erstellen, die eine Schnittstelle zu einer vorhandenen Datenbank bereitstellt. Diese Reihe von Lernprogrammen wird gezeigt, wie automatisch generieren von Code, der ermöglicht Benutzern das anzeigen, bearbeiten, erstellen und Löschen von Daten, die in einer Datenbanktabelle gespeichert. Der generierte Code entspricht den Spalten in der Datenbanktabelle.
> 
> In diesem Teil der Reihe konzentriert sich auf die Webanwendung erstellen und das Generieren von Datenmodelle auf Grundlage von Datenbanktabellen.


## <a name="create-a-new-aspnet-web-application"></a>Erstellen Sie eine neue ASP.NET-Webanwendung

Erstellen Sie in einer neuen Projektmappe oder in der gleichen Lösung wie das Datenbankprojekt ein neues Projekt in Visual Studio, und wählen Sie die **ASP.NET-Webanwendung** Vorlage. Nennen Sie das Projekt **ContosoSite**.

![Projekt erstellen](creating-the-web-application/_static/image1.png)

Klicken Sie auf **OK**.

Wählen Sie im Fenster Neues ASP.NET-Projekt den **MVC** Vorlage. Können Sie löschen die **Host in der Cloud** option jetzt, da Sie später, die Anwendung in der Cloud bereitgestellt werden. Klicken Sie auf **OK** zum Erstellen der Anwendung.

![Mvc-Vorlage auswählen](creating-the-web-application/_static/image2.png)

Das Projekt wird mit den Standarddateien und Ordner erstellt.

In diesem Lernprogramm verwenden Sie Entity Framework 6. Sie können die Version von Entity Framework in Ihrem Projekt über das Fenster "NuGet-Pakete verwalten" überprüfen. Aktualisieren Sie bei Bedarf Ihrer Version von Entity Framework.

![Anzeigen der version](creating-the-web-application/_static/image3.png)

## <a name="generate-the-models"></a>Die Modelle generieren

Entity Framework-Modelle wird nun aus den Datenbanktabellen erstellt werden. Diese Modelle sind Klassen, die Sie verwenden, um mit den Daten arbeiten. Jedes Modell eine Tabelle in der Datenbank spiegelt und enthaltenen Eigenschaften entsprechen den Spalten in der Tabelle.

Mit der rechten Maustaste die **Modelle** Ordner, und wählen **hinzufügen** und **neues Element**.

![Neues Element hinzufügen](creating-the-web-application/_static/image4.png)

Wählen Sie im Fenster Neues Element hinzufügen **Daten** im linken Bereich und **ADO.NET Entity Data Model** aus den Optionen in der Mitte. Benennen Sie die neue Modelldatei **ContosoModel**.

![Modell erstellen](creating-the-web-application/_static/image5.png)

Klicken Sie auf **Hinzufügen**.

Wählen Sie in der Entity Data Model-Assistenten **EF Designer aus Datenbank**.

![aus Datenbank generieren](creating-the-web-application/_static/image6.png)

Klicken Sie auf **Weiter**.

Wenn Sie Datenbankverbindungen, die innerhalb Ihrer Entwicklungsumgebung definiert haben, wird möglicherweise diese Verbindungen vorausgewählt angezeigt. Sie möchten jedoch eine neue Verbindung mit der Datenbank zu erstellen, die Sie in den ersten Teil dieses Lernprogramms erstellt haben. Klicken Sie auf die **neue Verbindung** Schaltfläche.

![mit Datenbank verbinden](creating-the-web-application/_static/image7.png)

Geben Sie im Fenster Eigenschaften von Verbindung den Namen des lokalen Servers, auf die Datenbank erstellt wurde (in diesem Fall **(Localdb) \ProjectsV12**). Wählen Sie nachdem Sie den Namen des Servers haben die ContosoUniversityData aus den verfügbaren Datenbanken aus.

![Festlegen von Verbindungseigenschaften](creating-the-web-application/_static/image8.png)

Klicken Sie auf **OK**.

Die richtige Verbindungseigenschaften werden nun angezeigt. Sie können den Standardnamen für die Verbindung in der Datei "Web.config" verwenden.

![Verbindungseinstellungen](creating-the-web-application/_static/image9.png)

Klicken Sie auf **Weiter**.

Wählen Sie **Tabellen** zum Generieren von Modellen für alle drei Tabellen.

![Tabellen auswählen](creating-the-web-application/_static/image10.png)

Klicken Sie auf **Fertig stellen**.

Wenn eine sicherheitswarnung angezeigt wird, wählen Sie **OK** weiter ausgeführt wird, die Vorlage.

Die Modelle werden aus den Datenbanktabellen generiert und ein Diagramm wird angezeigt, die Eigenschaften und Beziehungen zwischen den Tabellen angezeigt, werden.

![Diagramm des Modells](creating-the-web-application/_static/image11.png)

Der Ordner Models enthält jetzt viele neue Dateien, die im Zusammenhang mit der Modelle, die aus der Datenbank generiert wurden.

![neue Modelldateien anzeigen](creating-the-web-application/_static/image12.png)

Die **ContosoModel.Context.cs** -Datei enthält eine Klasse, die von abgeleitet ist die **DbContext** Klasse und enthält eine Eigenschaft für jedes Modellklasse, die in einer Datenbanktabelle entspricht. Die **Course.cs**, **Enrollment.cs**, und **Student.cs** Dateien enthalten die Modellklassen, die Datenbanken Tabellen darstellen. Sie werden der Context-Klasse und der Modellklassen verwenden, bei der Arbeit mit Gerüstbau.

Erstellen Sie bevor Sie mit diesem Lernprogramm fortfahren das Projekt aus. Im nächsten Abschnitt generieren Sie Code basiert auf Datenmodellen, aber dieser Abschnitt nicht funktioniert, wenn das Projekt nicht erstellt wurde.

> [!div class="step-by-step"]
> [Zurück](setting-up-database.md)
> [Weiter](generating-views.md)
