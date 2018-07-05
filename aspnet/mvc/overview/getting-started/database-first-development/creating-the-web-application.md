---
uid: mvc/overview/getting-started/database-first-development/creating-the-web-application
title: 'EF Database First mit ASP.NET MVC: Erstellen der Webanwendung und Datenmodelle | Microsoft-Dokumentation'
author: tfitzmac
description: Verwenden MVC, Entity Framework und ASP.NET-Gerüstbau, können Sie eine Webanwendung erstellen, die eine Schnittstelle für eine vorhandene Datenbank bereitstellt. Dieses Tutorial Seri...
ms.author: aspnetcontent
ms.date: 10/01/2014
ms.assetid: bc8f2bd5-ff57-4dcd-8418-a5bd517d8953
msc.legacyurl: /mvc/overview/getting-started/database-first-development/creating-the-web-application
msc.type: authoredcontent
ms.openlocfilehash: b4c30b12d2b62b71c0f80546d8a537c80d514d79
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37802895"
---
<a name="ef-database-first-with-aspnet-mvc-creating-the-web-application-and-data-models"></a>EF Database First mit ASP.NET MVC: Erstellen der Webanwendung und Datenmodelle
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> Verwenden MVC, Entity Framework und ASP.NET-Gerüstbau, können Sie eine Webanwendung erstellen, die eine Schnittstelle für eine vorhandene Datenbank bereitstellt. Dieser tutorialreihe erfahren Sie, wie Sie automatisch generierter Code, der ermöglicht Benutzern das anzeigen, bearbeiten, erstellen und Löschen von Daten, die in einer Datenbanktabelle gespeichert. Der generierte Code entspricht die Spalten in der Datenbanktabelle.
> 
> Dieser Teil der Serie konzentriert sich auf die Webanwendung erstellen und das Generieren von Datenmodelle auf Grundlage von Datenbanktabellen.


## <a name="create-a-new-aspnet-web-application"></a>Erstellen Sie eine neue ASP.NET-Webanwendung

Erstellen Sie ein neues Projekt in Visual Studio in einer neuen Projektmappe oder der gleichen Projektmappe wie das Projekt, und wählen Sie die **ASP.NET-Webanwendung** Vorlage. Nennen Sie das Projekt **ContosoSite**.

![Projekt erstellen](creating-the-web-application/_static/image1.png)

Klicken Sie auf **OK**.

Wählen Sie im Fenster Neues ASP.NET-Projekt die **MVC** Vorlage. Sie können löschen, die **in der Cloud hosten** option jetzt, da Sie später die Anwendung in der Cloud bereitstellen. Klicken Sie auf **OK** zum Erstellen der Anwendung.

![Mvc-Vorlage auswählen](creating-the-web-application/_static/image2.png)

Das Projekt wird mit der Standarddateien und Ordner erstellt.

In diesem Tutorial verwenden Sie Entity Framework 6. Sie können die Version von Entity Framework Vergewissern Sie sich in Ihrem Projekt über das Fenster "NuGet-Pakete verwalten". Aktualisieren Sie Ihre Version von Entity Framework, bei Bedarf.

![Version anzeigen](creating-the-web-application/_static/image3.png)

## <a name="generate-the-models"></a>Die Modelle generieren

Erstellen Sie jetzt Entity Framework-Modellen aus den Datenbanktabellen. Diese Modelle sind Klassen, die Sie verwenden werden, um mit den Daten arbeiten. Jedes Modell entspricht eine Tabelle in der Datenbank und enthält Eigenschaften, die die Spalten in der Tabelle entsprechen.

Mit der rechten Maustaste die **Modelle** Ordner, und wählen **hinzufügen** und **neues Element**.

![Neues Element hinzufügen](creating-the-web-application/_static/image4.png)

Wählen Sie im Fenster "Neues Element hinzufügen", **Daten** im linken Bereich und **ADO.NET Entity Data Model** aus den Optionen im mittleren Bereich. Nennen Sie die neue Modelldatei **ContosoModel**.

![Modell erstellen](creating-the-web-application/_static/image5.png)

Klicken Sie auf **Hinzufügen**.

Wählen Sie in der Assistent für Entity Data Model **EF Designer aus Datenbank**.

![aus Datenbank generieren](creating-the-web-application/_static/image6.png)

Klicken Sie auf **Weiter**.

Wenn Sie Verbindungen mit der Datenbank in Ihrer Entwicklungsumgebung definiert haben, sehen Sie diese Verbindungen vorab ausgewählt. Sie möchten jedoch eine neue Verbindung mit der Datenbank zu erstellen, die Sie im ersten Teil dieses Tutorials erstellt haben. Klicken Sie auf die **neue Verbindung** Schaltfläche.

![mit Datenbank verbinden](creating-the-web-application/_static/image7.png)

Geben Sie im Fenster Eigenschaften der Verbindung den Namen des lokalen Servers, in die Datenbank erstellt wurde (in diesem Fall **(Localdb) \ProjectsV12**). Wählen Sie nachdem Sie den Namen des Servers haben die ContosoUniversityData aus den verfügbaren Datenbanken aus.

![Set-Verbindungseigenschaften](creating-the-web-application/_static/image8.png)

Klicken Sie auf **OK**.

Die richtige Verbindungseigenschaften werden nun angezeigt. Sie können den Standardnamen für die Verbindung in der Datei "Web.config" verwenden.

![Verbindungseinstellungen](creating-the-web-application/_static/image9.png)

Klicken Sie auf **Weiter**.

Wählen Sie **Tabellen** zum Generieren von Modellen für alle drei Tabellen.

![Tabellen auswählen](creating-the-web-application/_static/image10.png)

Klicken Sie auf **Fertig stellen**.

Wenn eine sicherheitswarnung angezeigt wird, wählen Sie **OK** zum Fortsetzen der Ausführung der Vorlage.

Die Modelle aus Tabellen der Datenbank generiert wird, und ein Diagramm wird mit den Eigenschaften und Beziehungen zwischen den Tabellen angezeigt.

![Diagramm des Modells](creating-the-web-application/_static/image11.png)

Der Ordner "Models" enthält jetzt viele neue Dateien, die im Zusammenhang mit der die Modelle, die aus der Datenbank generiert wurden.

![Neues Modelldateien anzeigen](creating-the-web-application/_static/image12.png)

Die **ContosoModel.Context.cs** -Datei enthält eine abgeleitete Klasse die **"DbContext"** Klasse, und stellt eine Eigenschaft für jede Modellklasse, die einer Datenbanktabelle entspricht. Die **Course.cs**, **Enrollment.cs**, und **Student.cs** Dateien enthalten die Modellklassen, die in den Datenbanken Tabellen darstellen. Sie werden sowohl der Context-Klasse und die Modellklassen verwenden, bei der Arbeit mit Gerüstbau.

Erstellen Sie bevor Sie mit diesem Tutorial Fortfahren das Projekt ein. Im nächsten Abschnitt generieren Sie Code auf Grundlage der Datenmodelle, aber dieser Abschnitt funktioniert nicht, wenn das Projekt noch nicht erstellt wurde.

> [!div class="step-by-step"]
> [Zurück](setting-up-database.md)
> [Weiter](generating-views.md)
