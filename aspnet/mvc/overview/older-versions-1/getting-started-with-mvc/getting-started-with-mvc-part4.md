---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
title: Erstellen einer Datenbank | Microsoft Docs
author: shanselman
description: Dies ist ein Anfänger-Lernprogramm, das die Grundlagen von ASP.NET MVC. Erstellen Sie eine einfache Web-Anwendung, die Lese- und Schreibvorgänge aus einer Datenbank.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 742df67f-484d-4ef3-af6b-8c791e556b43
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
msc.type: authoredcontent
ms.openlocfilehash: ff2a41803cd31ce50bbf79e630d827b6de441ba3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868073"
---
<a name="creating-a-database"></a>Erstellen einer Datenbank
====================
durch [Scott Hanselman](https://github.com/shanselman)

> Dies ist ein Anfänger-Lernprogramm, das die Grundlagen von ASP.NET MVC. Erstellen Sie eine einfache Web-Anwendung, die Lese- und Schreibvorgänge aus einer Datenbank. Besuchen Sie die [ASP.NET MVC-Trainingscenter](../../../index.md) andere ASP.NET-MVC Lernprogramme und Beispiele finden.


In diesem Abschnitt werden wir eine neue SQL Express-Datenbank zu erstellen, die zum Speichern und Abrufen von unseren Filmdaten verwendet wird. Wählen Sie aus der Visual Web Developer-IDE auf die Registerkarte Ansicht | Server-Explorer. Klicken Sie mit der rechten Maustaste auf Datenverbindungen, und klicken Sie auf die Verbindung hinzufügen...

![AddConnection](getting-started-with-mvc-part4/_static/image1.png)

Klicken Sie im Dialogfeld "Datenquelle auswählen" Wählen Sie Microsoft SQL Server, und wählen Sie fortfahren.

![](getting-started-with-mvc-part4/_static/image2.png)

Klicken Sie im Dialogfeld "Verbindung hinzufügen" Geben Sie ". \SQLEXPRESS" für den Servernamen, und geben Sie "Filme" als Namen für die neue Datenbank.

[![Dialogfeld "Verbindung" hinzufügen](getting-started-with-mvc-part4/_static/image4.png)](getting-started-with-mvc-part4/_static/image3.png)

Klicken Sie auf OK, und Sie werden gefragt, ob die Datenbank erstellt werden soll. Wählen Sie Ja.

[![Erstellen Filme?](getting-started-with-mvc-part4/_static/image6.png)](getting-started-with-mvc-part4/_static/image5.png)

Jetzt haben Sie eine leere Datenbank im Server-Explorer.

![Fügen Sie neue Tabelle hinzu](getting-started-with-mvc-part4/_static/image7.png)

Klicken Sie mit der rechten Maustaste auf Tabellen, und klicken Sie auf die Tabelle hinzufügen. Tabellen-Designer wird angezeigt. Hinzufügen von Spalten für Id, Titel, ReleaseDate, "Genre" und Preis. Klicken Sie mit der rechten Maustaste auf die ID-Spalte, und klicken Sie auf den Primärschlüssel festlegen. Hier ist welche Meine Entwurf Bereiche sieht wie ein.

[![Datenbank-Tabellen-Editor](getting-started-with-mvc-part4/_static/image9.png)](getting-started-with-mvc-part4/_static/image8.png)

Wählen Sie die Id-Spalte, und ändern Sie unter folgenden Spalteneigenschaften "Identitätsspezifikation" auf "Yes".

[![IsIdentity - Spalteneigenschaften](getting-started-with-mvc-part4/_static/image11.png)](getting-started-with-mvc-part4/_static/image10.png)

Wenn Sie vorgenommen haben, klicken Sie auf das Symbol "Speichern" in der Symbolleiste, oder wählen Sie Datei | Speichern Sie aus dem Menü aus, und nennen Sie die Tabelle "**Film**" (im singular). Wir haben eine Datenbank und eine Tabelle!

[![Namen auswählen](getting-started-with-mvc-part4/_static/image13.png)](getting-started-with-mvc-part4/_static/image12.png)

Wechseln Sie zurück zum Server-Explorer mit der rechten Maustaste, klicken Sie auf die Film-Tabelle, und wählen Sie "Tabellendaten anzeigen". Geben Sie ein paar Filme aus, damit unsere Datenbank einige Daten enthält.

[![Bearbeiten von Datenbank-Tabelle](getting-started-with-mvc-part4/_static/image15.png)](getting-started-with-mvc-part4/_static/image14.png)

## <a name="creating-a-model"></a>Erstellen eines Modells

Jetzt, wechseln Sie zurück zum Projektmappen-Explorer auf der rechten Seite der IDE und mit der rechten Maustaste auf den Ordner Models und wählen Sie hinzufügen | Neues Element.

[![addnewmodelitem](getting-started-with-mvc-part4/_static/image17.png)](getting-started-with-mvc-part4/_static/image16.png)

Wir werden ein Entitätenmodell aus unserem neuen Datenbank zu erstellen. Dadurch wird eine Reihe von Klassen zum Projekt hinzugefügt, die erleichtert es uns abgefragt und bearbeitet die Daten innerhalb der Datenbank. Wählen Sie den Knoten Daten auf der linken Seite des Dialogfelds, und wählen Sie dann das ADO.NET Entity Data Model-Elementvorlage. Nennen sie Movies.edmx.

[![AddNewDataModel](getting-started-with-mvc-part4/_static/image19.png)](getting-started-with-mvc-part4/_static/image18.png)

Klicken Sie auf die Schaltfläche "Hinzufügen". Klicken Sie dann wird "Entity Data Model-Assistenten" gestartet.

Wählen Sie in der angezeigten Dialogfeld neue generieren aus Datenbank ein. Da wir nur eine Datenbank vorgenommen haben, müssen wir nur die Entity Framework über die neue Datenbank und die Tabelle mitteilen. Klicken Sie auf neben Speichern unserer datenbankverbindung in unserem Webanwendung-Konfiguration. Prüfen Sie jetzt die Tabellen und Film Kontrollkästchen und klicken Sie auf Fertig stellen.

[![Assistent für Entity Data Model](getting-started-with-mvc-part4/_static/image21.png)](getting-started-with-mvc-part4/_static/image20.png)

Jetzt können wir finden Sie in unserer neuen Film-Tabelle im Entity Framework-Designer und Code zugreifen.

[![Filme – Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part4/_static/image23.png)](getting-started-with-mvc-part4/_static/image22.png)

Auf der Entwurfsoberfläche sehen Sie eine "Movie"-Klasse. Diese Klasse ist der "Movie" Tabelle in der Datenbank zugeordnet, und jede Eigenschaft darin ist eine Spalte mit der Tabelle zugeordnet. Jede Instanz einer Klasse "Movie" wird eine Zeile in der Tabelle "Movie" entsprechen.

Wenn Sie die Standardnamen und Zuordnen von Entity Framework verwendete Konventionen nicht gefällt, können Sie die Entity Framework-Designer ändern, oder passen Sie sie. Für diese Anwendung wir die Standardeinstellungen verwenden und speichern Sie die Datei als nur-ist.

Jetzt sehen wir mit ein paar echten Daten funktioniert!

> [!div class="step-by-step"]
> [Zurück](getting-started-with-mvc-part3.md)
> [Weiter](getting-started-with-mvc-part5.md)
