---
uid: mvc/overview/older-versions-1/nerddinner/create-a-database
title: Erstellen Sie eine Datenbank | Microsoft Docs
author: microsoft
description: Schritt 2 zeigt die Schritte zum Erstellen der Datenbank, die alle von der Dinner und RSVP Daten für unsere NerdDinner-Anwendung.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 983f3ffa-08b8-4868-b8c9-aa34593fc683
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-database
msc.type: authoredcontent
ms.openlocfilehash: ba28d671bf13ec54b83b876462e2c23f90310037
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869139"
---
<a name="create-a-database"></a>Erstellen Sie eine Datenbank
====================
durch [Microsoft](https://github.com/microsoft)

[PDF herunterladen](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Dies ist Schritt 2 mit einem kostenlosen ["NerdDinner" Anwendung Lernprogramm](introducing-the-nerddinner-tutorial.md) , die Durchläufe-durch die Schritte zum Erstellen einer kleinen, aber abgeschlossen haben, Webanwendung, die mithilfe von ASP.NET MVC-1.
> 
> Schritt 2 zeigt die Schritte zum Erstellen der Datenbank, die alle von der Dinner und RSVP Daten für unsere NerdDinner-Anwendung.
> 
> Bei Verwendung von ASP.NET MVC 3 empfehlen wir führen Sie die [erste Schritte mit MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) oder [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) Lernprogramme.


## <a name="nerddinner-step-2-creating-the-database"></a>NerdDinner, Schritt 2: Erstellen der Datenbank

Wir müssen eine Datenbank verwenden, um alle Daten Dinner und die Antwort für unsere NerdDinner-Anwendung speichern.

Die folgenden Schritte veranschaulichen das Erstellen der Datenbank mithilfe der kostenlosen SQL Server Express Edition (die Sie leicht installieren können mithilfe von V2 der [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)). Der gesamte Code wir schreiben funktioniert mit SQL Server Express und die Vollversion von SQL Server.

### <a name="creating-a-new-sql-server-express-database"></a>Erstellen einer neuen SQL Server Express-Datenbank

Wir beginnen, indem Sie mit der rechten Maustaste auf unserem Webprojekt und wählen Sie dann die **Add -&gt;neues Element** Menübefehl:

![](create-a-database/_static/image1.png)

Hierdurch wird Visual Studio das Dialogfeld "Neues Element hinzufügen" "angezeigt. Wir filtern, indem Sie die "Datenkategorie" fort und wählen die Elementvorlage "SQL Server-Datenbank":

![](create-a-database/_static/image2.png)

Namen die SQL Server Express-Datenbank zu erstellen "NerdDinner.mdf", und klicken Sie auf ok. Visual Studio wird dann Fragen Sie uns, wenn wir unsere \App dieser Datei hinzufügen möchten\_Datenverzeichnis (durch die bereits ein Verzeichnis ist setup mit Lese- und Sicherheits-ACLs schreiben):

![](create-a-database/_static/image3.png)

Wir müssen auf "Ja" klicken und die neue Datenbank erstellt und unsere Projektmappen-Explorer hinzugefügt werden:

![](create-a-database/_static/image4.png)

### <a name="creating-tables-within-our-database"></a>Erstellen von Tabellen in der Datenbank

Wir haben jetzt eine neue leere Datenbank. Fügen Sie nun einige Tabellen hinzu.

Zu diesem Zweck wechseln wir zu "Verwaltung" die "Server-Explorer" in Visual Studio die uns zum Verwalten von Datenbanken und Servern ermöglicht. SQL Server Express-Datenbanken, die in der \App gespeichert\_Datenordner der Anwendung wird automatisch innerhalb des Server-Explorers angezeigt. Wir können optional das Symbol "Verbindung mit Datenbank herstellen" oben auf das Fenster "Server-Explorer" verwenden, um zusätzliche SQL Server-Datenbanken (lokal und remote) als auch der Liste hinzuzufügen:

![](create-a-database/_static/image5.png)

Wir werden unsere NerdDinner-Datenbank – einen zum Speichern von unseren Abendessen angezeigt, und die andere um Antwort Akzepte nachzuverfolgen zwei Tabellen hinzufügen. Wir können neue Tabellen erstellen, indem Sie mit der rechten Maustaste auf den Ordner "Tables" innerhalb der Datenbank und des Menübefehls "Neue Tabelle hinzufügen":

![](create-a-database/_static/image6.png)

Dadurch wird eine Tabellen-Designer geöffnet, die uns so konfigurieren Sie das Schema der Tabelle ermöglicht. Für unsere Tabelle "Abendessen" werden wir 10 Datenspalten hinzufügen:

![](create-a-database/_static/image7.png)

Wir möchten die Spalte "DinnerID" zu einem eindeutigen Primärschlüssel für die Tabelle. Wir können dies konfigurieren, indem Sie mit der rechten Maustaste auf die Spalte "DinnerID" und das Menüelement "Primärschlüssel festlegen":

![](create-a-database/_static/image8.png)

Zusätzlich zu den vorgenommen DinnerID ein Primärschlüssel ist, möchten wir auch ebenfalls eine Spalte "Identity" zu konfigurieren, deren Wert wird automatisch erhöht, wenn neue Zeilen von Daten zur Tabelle hinzugefügt werden (d. h., die erste Zeile der eingefügte Dinner müssen eine DinnerID 1, das zweite eingefügte Zeile weist eine DinnerID 2 usw.).

Wir können dazu die Spalte "DinnerID" und dann mithilfe des "Spalteneigenschaften"-Editors die Eigenschaft "(ist Identity)" für die Spalte auf "Ja" festgelegt. Wir verwenden die Standardidentität Standardwerte (beginnen bei 1 und 1 für jede neue Dinner Zeile erhöht):

![](create-a-database/_static/image9.png)

Wir werden unsere Tabelle dann speichern, durch Eingabe von STRG + S oder mithilfe der **Datei -&gt;speichern** Menübefehl. Daraufhin werden wir den Namen der Tabelle aufgefordert werden. Er "Abendessen" den Namen:

![](create-a-database/_static/image10.png)

Unsere neue Abendessen Tabelle wird dann in die Datenbank im Server-Explorer angezeigt.

Wir dann wiederholen Sie die oben genannten Schritte und erstellen Sie eine Tabelle "Antwort". Diese Tabelle mit 3 Spalten erhalten Wir werden die RsvpID-Spalte als Primärschlüssel einrichten, und auch eine Identity-Spalte:

![](create-a-database/_static/image11.png)

Wir müssen speichern, und geben Sie ihm den Namen "Antwort".

### <a name="setting-up-a-foreign-key-relationship-between-tables"></a>Einrichten einer Foreign Key-Beziehung zwischen Tabellen

Wir haben jetzt die beiden Tabellen in der Datenbank. Unsere letzte Schritt der Schema-Entwurf werden eine "eins zu viele"-Beziehung zwischen diesen beiden Tabellen – einrichten, damit wir jede Zeile Dinner NULL oder mehr Zeilen von Antwort zuordnen können, die für sie gelten. Es wird dazu konfigurieren die Antwort Spalte der Tabelle "DinnerID" um einen Fremdschlüssel-Beziehung auf die Spalte "DinnerID" in der Tabelle "Abendessen" haben.

Zu diesem Zweck müssen wir die Antwort-Tabelle im Tabellen-Designer öffnen, durch Doppelklick im Server-Explorer. Wir wählen dann die Spalte "DinnerID" darin, mit der rechten Maustaste, und wählen Sie "Relationshps..." Der Kontextmenübefehl:

![](create-a-database/_static/image12.png)

Hierdurch wird ein Dialogfeld angezeigt, die wir zum Setup-Beziehungen zwischen Tabellen verwendet werden können:

![](create-a-database/_static/image13.png)

Die Schaltfläche "Hinzufügen", um das Dialogfeld eine neue Beziehung hinzufügen klicken. Sobald eine Beziehung hinzugefügt wurde, müssen wir Knoten "Tabellen und Spaltenspezifikation" Strukturansicht im Eigenschaftenraster auf der rechten Seite des Dialogfelds und klicken Sie dann auf die Schaltfläche "…" rechts davon:

![](create-a-database/_static/image14.png)

Klicken auf die Schaltfläche "…" wird ein weiteres Dialogfeld angezeigt, die uns ermöglicht, die angeben, welche Tabellen und Spalten in der Beziehung beteiligt sind, sowie ermöglichen es uns, um die Beziehung zu benennen.

Wir ändern die Primärschlüsseltabelle "Abendessen" angezeigt werden, und wählen Sie die Spalte "DinnerID" innerhalb der Tabelle Abendessen als Primärschlüssel. Unsere Antwort-Tabelle werden die Fremdschlüssel-Tabelle und die Antwort. DinnerID-Spalte wird als der Fremdschlüssel zugeordnet werden:

![](create-a-database/_static/image15.png)

Jetzt wird jede Zeile in der Antwort-Tabelle eine Zeile in der Tabelle Dinner zugeordnet werden. SQL Server wird die referenziellen Integrität für uns – verwalten und verhindern, dass wir eine neue Antwort Zeile hinzufügen, wenn er nicht auf eine gültige Dinner Zeile verweist. Es wird auch verhindert, dass uns Löschen einer Zeile Dinner treten weiterhin RSVP von Zeilen, die darauf verweisen.

### <a name="adding-data-to-our-tables"></a>Hinzufügen von Daten zu den Tabellen

Fertig stellen wir einige Beispieldaten mit unserer Tabelle Abendessen hinzufügen. Wir können die Daten in eine Tabelle hinzufügen, indem Sie mit der rechten Maustaste auf die sie in Server-Explorer und den Befehl "Tabellendaten anzeigen":

![](create-a-database/_static/image16.png)

Wir fügen einige Dinner Datenzeilen, die wir später wie beginnen wir implementieren die Anwendung verwenden können:

![](create-a-database/_static/image17.png)

### <a name="next-step"></a>Nächster Schritt

Wir haben die Erstellung der Datenbank ist abgeschlossen. Jetzt erstellen wir Modellklassen, die wir zum Abfragen und aktualisieren Sie ihn verwenden können.

> [!div class="step-by-step"]
> [Zurück](create-a-new-aspnet-mvc-project.md)
> [Weiter](build-a-model-with-business-rule-validations.md)
