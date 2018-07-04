---
uid: mvc/overview/older-versions-1/nerddinner/create-a-database
title: Erstellen Sie eine Datenbank | Microsoft-Dokumentation
author: microsoft
description: Schritt 2 zeigt die Schritte zum Erstellen der Datenbank enthält alle von der Dinner und Antworten Sie Daten für die NerdDinner-Anwendung.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 983f3ffa-08b8-4868-b8c9-aa34593fc683
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-database
msc.type: authoredcontent
ms.openlocfilehash: 01c6d3c63780e492b97aa54a92f3982d4c18f9e5
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371748"
---
<a name="create-a-database"></a>Erstellen Sie eine Datenbank
====================
durch [Microsoft](https://github.com/microsoft)

[PDF herunterladen](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Dies ist Schritt 2, der ein kostenloses ["NerdDinner"-webanwendungstutorial](introducing-the-nerddinner-tutorial.md) , die führt – Exemplarische Vorgehensweise erstellen eine kleine, jedoch abgeschlossen haben, Web-Anwendung mithilfe von ASP.NET MVC-1.
> 
> Schritt 2 zeigt die Schritte zum Erstellen der Datenbank enthält alle von der Dinner und Antworten Sie Daten für die NerdDinner-Anwendung.
> 
> Wenn Sie ASP.NET MVC 3 verwenden, sollten Sie Sie folgen den [erste Schritte mit MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) oder [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) Tutorials.


## <a name="nerddinner-step-2-creating-the-database"></a>NerdDinner, Schritt 2: Erstellen der Datenbank

Wir werden eine Datenbank verwenden, um alle Dinner und RSVP Daten für die NerdDinner-Anwendung zu speichern.

Die folgenden Schritte zeigen, der mit der kostenlosen SQL Server Express Edition-Datenbank erstellen (die Sie ganz einfach mit Version 2 der installieren können die [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)). Der gesamte wir schreiben Code funktioniert mit SQL Server Express und die vollständige SQL Server.

### <a name="creating-a-new-sql-server-express-database"></a>Erstellen einer neuen SQL Server Express-Datenbank

Wir beginnen, indem Sie mit der rechten Maustaste auf unsere Webprojekt, und wählen Sie dann die **Add-&gt;neues Element** Menübefehl:

![](create-a-database/_static/image1.png)

Dialogfeld für Visual Studio-"Neues Element hinzufügen" wird angezeigt. Wir filtern, indem Sie die Kategorie "Data" und wählen die Elementvorlage "SQL-Datenbank":

![](create-a-database/_static/image2.png)

Namen die SQL Server Express-Datenbank, die zum Erstellen von "NerdDinner.mdf", und drücken hier also Okay werden sollten. Visual Studio wird dann Fragen Sie uns, wenn wir unsere \App dieser Datei hinzufügen möchten\_Verzeichnis "Data" (durch die bereits ein Verzeichnis ist Einrichtung mit Lese- und Schreiben von Sicherheits-ACLs):

![](create-a-database/_static/image3.png)

"Ja" klicken, und der neuen Datenbank wird erstellt und unsere Projektmappen-Explorer hinzugefügt:

![](create-a-database/_static/image4.png)

### <a name="creating-tables-within-our-database"></a>Erstellen von Tabellen in unserer Datenbank

Wir haben jetzt eine neue leere Datenbank. Wir fügen Sie einige Tabellen hinzu.

Zu diesem Zweck navigieren wir zu der Registerkarte "Server-Explorer" im Fenster in Visual Studio, die wir zum Verwalten von Datenbanken und Server ermöglicht. SQL Server Express-Datenbanken, die in der \App gespeichert\_Datenordner der Anwendung wird automatisch im Server-Explorer angezeigt. Wir können optional das Symbol "Verbindung mit Datenbank herstellen" oben auf das Fenster "Server-Explorer" verwenden, um zusätzliche SQL Server-Datenbanken (lokal und remote) sowie der Liste hinzuzufügen:

![](create-a-database/_static/image5.png)

Wir werden unsere NerdDinner-Datenbank – einen zum Speichern unserer Dinner, und die andere zum Nachverfolgen von RSVP Annahme werden zwei Tabellen hinzufügen. Wir können neue Tabellen erstellen, indem Sie mit der rechten Maustaste auf den Ordner "Tables" in unserer Datenbank und Auswählen des Menübefehls "Neue Tabelle hinzufügen":

![](create-a-database/_static/image6.png)

Dadurch wird eine Tabellen-Designer geöffnet, die wir das Schema der Tabelle konfigurieren zu können. Fügen wir für unsere Tabelle "Dinner" 10 Datenspalten hinzu:

![](create-a-database/_static/image7.png)

Wir möchten die Spalte "DinnerID" einem eindeutigen Primärschlüssel für die Tabelle zu sein. Wir können dies konfigurieren, indem Sie mit der rechten Maustaste auf die Spalte "DinnerID" und das Menüelement "Primärschlüssel festlegen" auswählen:

![](create-a-database/_static/image8.png)

Zusätzlich zum DinnerID einen Primärschlüssel, wir sollten es als eine Spalte "Identity" konfigurieren, deren Wert wird automatisch erhöht werden, in der Tabelle neue Datenzeilen hinzugefügt werden (d. h. die erste Zeile der eingefügte Dinner müssen eine DinnerID 1 aus, die zweite eingefügte Zeile hat eine DinnerID 2 usw.).

Dazu die Spalte "DinnerID" Sie können und dann mithilfe des "Spalteneigenschaften"-Editors die Eigenschaft "(ist Identity)" in der Spalte auf "Ja" festlegen. Wir verwenden die Standardwerte für die standard-Identität (beginnen bei 1 und 1 für jede neue Zeile mit Dinner erhöht):

![](create-a-database/_static/image9.png)

Speichern wir auch dann der Tabelle durch Eingabe von STRG + S oder mithilfe der **Dateien&gt;speichern** Menübefehl. Dies fordert uns, die den Namen der Tabelle. Es "Dinner" nennen:

![](create-a-database/_static/image10.png)

Unsere neue Dinner-Tabelle wird dann in unserer Datenbank im Server-Explorer angezeigt.

Wir dann wiederholen Sie die oben genannten Schritte und erstellen Sie eine Tabelle "Bestätigung". Diese Tabelle mit haben 3 Spalten. Wir richten die RsvpID-Spalte als Primärschlüssel, und stellen Sie sie außerdem eine Identity-Spalte:

![](create-a-database/_static/image11.png)

Wir speichern Sie sie und geben Sie ihm den Namen "Bestätigung".

### <a name="setting-up-a-foreign-key-relationship-between-tables"></a>Das Einrichten einer Foreign Key-Beziehung zwischen Tabellen

Wir haben jetzt zwei Tabellen in unserer Datenbank. Im letzten Schritt der Schema Design werden Sie eine "1-n-Beziehung zwischen diesen beiden Tabellen – einrichten, damit wir jede Dinner-Zeile mit NULL oder mehr Zeilen von RSVP zuordnen können, die für sie gelten. Wir tun dies, indem RSVP Spalte der Tabelle "DinnerID" um einen Fremdschlüssel-Beziehung auf die Spalte "DinnerID" in der Tabelle "Dinner" konfigurieren.

Zu diesem Zweck werden wir der RSVP-Tabelle im Tabellen-Designer aktivieren, indem sie im Server-Explorer darauf doppelklicken. Ich wähle klicken Sie dann die Spalte "DinnerID" darin, mit der rechten Maustaste, und wählen Sie den Befehl im Kontextmenü "Relationshps...":

![](create-a-database/_static/image12.png)

Hierdurch wird ein Dialogfeld angezeigt, die wir zum Setup von Beziehungen zwischen Tabellen verwenden können:

![](create-a-database/_static/image13.png)

Wir werden auf die Schaltfläche "Hinzufügen", um das Dialogfeld eine neue Beziehung hinzufügen klicken. Nachdem Sie eine Beziehung hinzugefügt wurde, wir erweitern den Knoten "Tabellen und Spaltenspezifikation" Strukturansicht, innerhalb des eigenschaftsrasters auf der rechten Seite des Dialogfelds, und klicken Sie dann auf die Schaltfläche "...", die sich rechts von:

![](create-a-database/_static/image14.png)

Klicken Sie auf die Schaltfläche "..." wird ein weiteres Dialogfeld angezeigt, mit der wir angeben, welche Tabellen und Spalten in der Beziehung beteiligt sind, sowie können wir nennen Sie die Beziehung.

Wir ändern die Primärschlüsseltabelle "Dinner" werden, und wählen Sie die Spalte "DinnerID" innerhalb der Dinner-Tabelle als Primärschlüssel. Unsere RSVP-Tabelle werden die Fremdschlüsseltabelle, und der Bestätigung. DinnerID-Spalte wird als der fremdschlüsselbeziehung zugeordnet werden:

![](create-a-database/_static/image15.png)

Jetzt wird jede Zeile in der RSVP-Tabelle eine Zeile in die Dinner-Tabelle zugeordnet sein. SQL Server wird referenziellen Integrität für uns – verwalten und zu verhindern, dass wir eine neue RSVP Zeile hinzufügen, wenn es sich nicht auf eine gültige Dinner Zeile verweist. Es wird auch verhindern, dass uns das Löschen einer Dinner-Zeile, treten immer noch Antworten von Zeilen, die darauf verweist.

### <a name="adding-data-to-our-tables"></a>Hinzufügen von Daten zu den Tabellen

Lassen Sie uns durch das Hinzufügen von Beispieldaten mit unserer Tabelle Dinner abgeschlossen. Wir können Daten in eine Tabelle hinzufügen, indem Sie mit der rechten Maustaste auf die sie im Server-Explorer, und wählen den Befehl "Tabellendaten anzeigen":

![](create-a-database/_static/image16.png)

Wir fügen einige Zeilen der Dinner-Daten, die wir später dem Implementieren der Anwendungs verwenden können:

![](create-a-database/_static/image17.png)

### <a name="next-step"></a>Nächster Schritt

Wir haben die Erstellung der Datenbank abgeschlossen. Als Nächstes erstellen wir ViewModel-Klassen, die wir zum Abfragen und aktualisieren Sie sie verwenden können.

> [!div class="step-by-step"]
> [Zurück](create-a-new-aspnet-mvc-project.md)
> [Weiter](build-a-model-with-business-rule-validations.md)
