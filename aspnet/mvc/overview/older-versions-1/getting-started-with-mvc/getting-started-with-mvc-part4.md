---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
title: Erstellen einer Datenbank | Microsoft-Dokumentation
author: shanselman
description: Dies ist ein Tutorial für Anfänger, die die Grundlagen von ASP.NET MVC eingeführt werden. Erstellen Sie eine einfache Webanwendung, die aus einer Datenbank liest und schreibt.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 742df67f-484d-4ef3-af6b-8c791e556b43
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
msc.type: authoredcontent
ms.openlocfilehash: 0e72359095e4c40ef7e56f1290a45ded257143c9
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362355"
---
<a name="creating-a-database"></a>Erstellen einer Datenbank
====================
durch [Scott Hanselman](https://github.com/shanselman)

> Dies ist ein Tutorial für Anfänger, die die Grundlagen von ASP.NET MVC eingeführt werden. Sie erstellen eine einfache Webanwendung, die aus einer Datenbank liest und schreibt. Besuchen Sie die [ASP.NET MVC-Informationscenter](../../../index.md) anderen ASP.NET MVC anhand von Tutorials und Beispiele finden.


In diesem Abschnitt werden wir eine neue SQL Express-Datenbank zu erstellen, die wir zum Speichern und Abrufen von unserem Movie-Daten verwenden. Wählen Sie aus der Visual Web Developer-IDE auf die Registerkarte Ansicht | Server-Explorer. Klicken Sie mit der rechten Maustaste auf Data Connections, und klicken Sie auf die Verbindung hinzufügen...

![AddConnection](getting-started-with-mvc-part4/_static/image1.png)

Klicken Sie im Dialogfeld "Datenquelle auswählen" Wählen Sie Microsoft SQL Server, und wählen Sie weiter.

![](getting-started-with-mvc-part4/_static/image2.png)

Geben Sie im Dialogfeld Verbindung hinzufügen ". \SQLEXPRESS" für den Servernamen, und geben Sie "Movies" als Namen für Ihre neue Datenbank.

[![Dialogfeld "Verbindung" hinzufügen](getting-started-with-mvc-part4/_static/image4.png)](getting-started-with-mvc-part4/_static/image3.png)

Klicken Sie auf OK, und Sie werden aufgefordert, wenn die Datenbank erstellt werden soll. Wählen Sie Ja.

[![Erstellen von Filmen?](getting-started-with-mvc-part4/_static/image6.png)](getting-started-with-mvc-part4/_static/image5.png)

Jetzt haben Sie eine leere Datenbank im Server-Explorer.

![Neue Tabelle hinzufügen](getting-started-with-mvc-part4/_static/image7.png)

Klicken Sie mit der rechten Maustaste auf Tabellen, und klicken Sie auf die Tabelle hinzufügen. Tabellen-Designer wird angezeigt. Hinzufügen von Spalten für Id, Titel, ReleaseDate, "Genre" und Preis. Klicken Sie mit der rechten Maustaste auf die ID-Spalte, und klicken Sie auf den Primärschlüssel festlegen. Hier ist welche mein Entwurf Bereiche wie folgt aussieht.

[![Datenbank-Editor](getting-started-with-mvc-part4/_static/image9.png)](getting-started-with-mvc-part4/_static/image8.png)

Wählen Sie die Id-Spalte, und ändern Sie unter den unten aufgeführten Eigenschaften der Spalte "Die Spezifikation der Spaltenidentität" auf "Ja".

[![IsIdentity - Spalteneigenschaften](getting-started-with-mvc-part4/_static/image11.png)](getting-started-with-mvc-part4/_static/image10.png)

Wenn Sie abgeschlossen haben, klicken Sie auf das Symbol "Speichern" in der Symbolleiste, oder wählen Sie Datei | Speichern Sie im Menü, und nennen Sie die Tabelle "**Film**" (im singular). Wir haben eine Datenbank und eine Tabelle.

[![Wählen Sie Namen](getting-started-with-mvc-part4/_static/image13.png)](getting-started-with-mvc-part4/_static/image12.png)

Wechseln Sie zurück zum Server-Explorer, und klicken Sie mit der rechten Maustaste auf die Tabelle "Movie", und wählen Sie dann "Tabellendaten anzeigen". Geben Sie einige Filme, damit unsere Datenbank einige Daten haben.

[![Datenbank-Tabellenbearbeitung](getting-started-with-mvc-part4/_static/image15.png)](getting-started-with-mvc-part4/_static/image14.png)

## <a name="creating-a-model"></a>Erstellen eines Modells

Nun wechseln Sie zurück zum Projektmappen-Explorer auf der rechten Seite der IDE mit der rechten Maustaste auf den Ordner "Models", und wählen Add | Neues Element.

[![addnewmodelitem](getting-started-with-mvc-part4/_static/image17.png)](getting-started-with-mvc-part4/_static/image16.png)

Wir werden ein Entitätsmodell aus der neuen Datenbank zu erstellen. Dadurch wird eine Reihe von Klassen zu Ihrem Projekt hinzugefügt, die erleichtert es uns, Abfragen und bearbeiten die Daten in der Datenbank. Wählen Sie den Knoten "Daten" auf der linken Seite des Dialogfelds, und wählen Sie dann den ADO.NET Entity Data Model-Elementvorlage. Nennen Sie ihn Movies.edmx.

[![AddNewDataModel](getting-started-with-mvc-part4/_static/image19.png)](getting-started-with-mvc-part4/_static/image18.png)

Klicken Sie auf die Schaltfläche "Hinzufügen". Dadurch wird dann "Entity Data Model-Assistenten" gestartet.

Wählen Sie in der neue Dialog, der angezeigt wird, generieren aus Datenbank ein. Da wir nur eine Datenbank vorgenommen haben, müssen wir nur das Entity Framework über unsere neue Datenbank und die Tabelle zu informieren. Klicken Sie auf "neben", speichern Sie die datenbankverbindung in unserer Webanwendung-Konfiguration. Prüfen Sie jetzt die Tabellen und Film Kontrollkästchen und klicken Sie auf Fertig stellen.

[![Assistent für Entity Data Model](getting-started-with-mvc-part4/_static/image21.png)](getting-started-with-mvc-part4/_static/image20.png)

Nun können wir unsere neue Tabelle "Movie" im Entity Framework Designer angezeigt und in Code darauf zugreifen.

[![Filme – Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part4/_static/image23.png)](getting-started-with-mvc-part4/_static/image22.png)

Auf der Entwurfsoberfläche können Sie eine "Movie"-Klasse sehen. Diese Klasse wird der Tabelle "Movie" in der Datenbank, und jede Eigenschaft darin ordnet einer Spalte mit der Tabelle. Jede Instanz einer Klasse "Movie" wird eine Zeile in der Tabelle "Movie" entsprechen.

Wenn Sie den standardmäßigen benennungs- und Zuordnen von Konventionen, die vom Entity Framework verwendet nicht zufrieden sind, können Sie den Entity Framework Designer ändern, oder passen Sie sie. Für diese Anwendung wir die Standardeinstellungen verwenden und speichern Sie die Datei als-ist.

Nun arbeiten wir mit ein paar echten Daten!

> [!div class="step-by-step"]
> [Zurück](getting-started-with-mvc-part3.md)
> [Weiter](getting-started-with-mvc-part5.md)
