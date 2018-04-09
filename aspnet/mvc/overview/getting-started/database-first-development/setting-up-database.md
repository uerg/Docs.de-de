---
uid: mvc/overview/getting-started/database-first-development/setting-up-database
title: Erste Schritte mit Entity Framework 6 Database First mit MVC 5 | Microsoft Docs
author: tfitzmac
description: MVC, Entity Framework und ASP.NET Gerüstbau verwenden, können Sie eine Webanwendung erstellen, die eine Schnittstelle zu einer vorhandenen Datenbank bereitstellt. Dieses Lernprogramm Seri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: 095abad4-3bfe-4f06-b092-ae6a735b7e49
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/setting-up-database
msc.type: authoredcontent
ms.openlocfilehash: ae60b5c808d2522c66dc17ccf7d16fefdc65d552
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="getting-started-with-entity-framework-6-database-first-using-mvc-5"></a>Erste Schritte mit Entity Framework 6 Database First mit MVC 5
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> MVC, Entity Framework und ASP.NET Gerüstbau verwenden, können Sie eine Webanwendung erstellen, die eine Schnittstelle zu einer vorhandenen Datenbank bereitstellt. Diese Reihe von Lernprogrammen wird gezeigt, wie automatisch generieren von Code, der ermöglicht Benutzern das anzeigen, bearbeiten, erstellen und Löschen von Daten, die in einer Datenbanktabelle gespeichert. Der generierte Code entspricht den Spalten in der Datenbanktabelle. Im letzten Teil der Reihe stellen Sie den Standort- und Datenbankserver in Azure bereit.
> 
> In diesem Teil der Reihe konzentriert sich auf eine Datenbank erstellen und mit Daten aufgefüllt.
> 
> Diese Reihe Schreibvorgangs mit Beiträgen aus Tom Dykstra und Rick Anderson. Es wurde basierend auf Feedback von Benutzern im Abschnitt "Kommentare" verbessert.


## <a name="introduction"></a>Einführung

In diesem Thema wird gezeigt, wie start mit einer vorhandenen Datenbank und erstellen Sie schnell eine Webanwendung, die Benutzern ermöglicht, die mit den Daten interagieren. Es verwendet die Entity Framework 6 und MVC 5, um die Webanwendung zu erstellen. Die ASP.NET Gerüstbau-Funktion können Sie Code zum Anzeigen, aktualisieren, erstellen und Löschen von Daten automatisch zu generieren. Verwenden die publishing Tools in Visual Studio, können Sie einfach die Standort- und Datenbankserver in Azure bereitstellen.

Dieses Thema behandelt die Situation, in dem Sie mit einer Datenbank und zum Generieren von Code für eine Webanwendung, die auf Basis der Felder in dieser Datenbank werden soll. Dieser Ansatz wird Database First-Entwicklung aufgerufen. Wenn Sie nicht bereits über eine vorhandene Datenbank verfügen, können Sie stattdessen eine Methode wird aufgerufen, was das Datenklassen zu definieren, und generieren die Datenbank in den Klasseneigenschaften umfasst Code First-Entwicklung verwenden.

Einführende beispielsweise der Code First-Entwicklung finden Sie unter [erste Schritte mit ASP.NET MVC 5](../introduction/getting-started.md). Ein komplexeres Beispiel finden Sie unter [Erstellen eines Entity Framework-Datenmodells für eine ASP.NET MVC 4-App](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

Anleitung zum Auswählen von Entity Framework Ansatz verwenden, finden Sie unter [Ansätze zur Entity Framework-Entwicklung](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).

## <a name="prerequisites"></a>Erforderliche Komponenten

Visual Studio 2013 oder Visual Studio Express 2013 für Web

## <a name="set-up-the-database"></a>Einrichten der Datenbank

Zum Imitieren der Umgebung von einer vorhandenen Datenbank müssen Sie zuerst erstellen Sie eine Datenbank mit Daten vorab mit Daten aufgefüllt, und erstellen Sie Ihre Webanwendung, die mit der Datenbank verbunden.

Dieses Lernprogramm wurde mit LocalDB mit Visual Studio 2013 oder Visual Studio Express 2013 für Web entwickelt. Sie können einen vorhandenen Datenbankserver anstelle von LocalDB verwenden, aber abhängig von Ihrer Version von Visual Studio und den Typ der Datenbank, aller Data Tools in Visual Studio möglicherweise nicht unterstützt. Wenn die Tools nicht für die Datenbank verfügbar sind, müssen Sie möglicherweise einige der Schritte in der Management Suite datenbankspezifischen für Ihre Datenbank ausführen.

Wenn Sie ein Problem mit der Datenbanktools in Ihrer Version von Visual Studio verfügen, stellen Sie sicher, dass Sie die neueste Version der Datenbanktools installiert haben. Informationen zum Aktualisieren oder installieren die Datenbanktools finden Sie unter [Microsoft SQL Server Data Tools](https://msdn.microsoft.com/data/hh297027).

Starten Sie Visual Studio und erstellen Sie eine **SQL Server-Datenbankprojekt**. Nennen Sie das Projekt **ContosoUniversityData**.

![Datenbankprojekt erstellen](setting-up-database/_static/image1.png)

Sie haben jetzt ein leeres Datenbankprojekt. Sie werden diese Datenbank in Azure weiter unten in diesem Lernprogramm bereitstellen, daher Sie Azure SQL-Datenbank als Zielplattform für das Projekt festlegen müssen. Festlegen der Zielplattform wird die Datenbank nicht tatsächlich bereitgestellt; Es bedeutet lediglich, dass das Datenbankprojekt prüfen, ob der Datenbankentwurf mit der Zielplattform kompatibel ist. Um die Zielplattform festzulegen, öffnen Sie die **Eigenschaften** für das Projekt und wählen Sie **Microsoft Azure SQL-Datenbank** für die Zielplattform.

![Set-Zielplattform](setting-up-database/_static/image2.png)

Sie können die Tabellen, die für dieses Lernprogramm erforderlich sind, durch Hinzufügen von SQL-Skripts, die definieren, die Tabellen erstellen. Mit der rechten Maustaste des Projekts, und fügen Sie ein neues Element hinzu.

![Neues Element hinzufügen](setting-up-database/_static/image3.png)

Fügen Sie eine neue Tabelle namens Student hinzu.

![Studententabelle hinzufügen](setting-up-database/_static/image4.png)

Ersetzen Sie den T-SQL-Befehl in der Tabelle durch den folgenden Code zum Erstellen der Tabelle.

[!code-sql[Main](setting-up-database/samples/sample1.sql)]

Beachten Sie, dass das Entwurfsfenster automatisch durch den Code synchronisiert wird. Sie können mit dem Code oder -Designer arbeiten.

![Anzeigen von Code und Entwurf](setting-up-database/_static/image5.png)

Fügen Sie einer anderen Tabelle hinzu. Dieses Mal nennen Sie sie Kurs und verwenden Sie den folgenden T-SQL-Befehl.

[!code-sql[Main](setting-up-database/samples/sample2.sql)]

Und wiederholen Sie noch einmal, eine Tabelle mit dem Namen Registrierung erstellen.

[!code-sql[Main](setting-up-database/samples/sample3.sql)]

Sie können die Datenbank mit Daten über ein Skript auffüllen, die ausgeführt wird, nachdem die Datenbank bereitgestellt wird. Fügen Sie ein Skript nach der Bereitstellung zum Projekt hinzu. Sie können den Standardnamen verwenden.

![Hinzufügen des Skripts nach der Bereitstellung](setting-up-database/_static/image6.png)

Fügen Sie den folgenden T-SQL-Code, um das Skript nach der Bereitstellung. Dieses Skript fügt Daten einfach mit der Datenbank, wenn keine übereinstimmenden Datensatz gefunden wird. Es werden keine überschreiben oder löschen alle Daten, die Sie in die Datenbank eingegeben haben.

[!code-sql[Main](setting-up-database/samples/sample4.sql)]

Es ist wichtig zu beachten, dass das Skript nach der Bereitstellung ausgeführt wird, jedes Mal, wenn Sie das Datenbankprojekt bereitstellen. Aus diesem Grund müssen Sie Ihren Anforderungen sorgfältig zu überlegen, wenn Sie dieses Skript schreiben. In einigen Fällen möchten Sie möglicherweise über Starten von einem bekannten Satz von Daten jedes Mal, wenn das Projekt bereitgestellt wird. In anderen Fällen möchten Sie möglicherweise nicht die vorhandenen Daten in irgendeiner Weise ändern. Je nach Ihren Anforderungen, können Sie entscheiden, ob Sie benötigen ein Skript nach der Bereitstellung oder in das Skript eingefügt werden sollen. Weitere Informationen zum Auffüllen der Datenbank mit einem Skript nach der Bereitstellung finden Sie unter [einschließlich Daten in einem SQL Server-Datenbankprojekt](https://blogs.msdn.com/b/ssdt/archive/2012/02/02/including-data-in-an-sql-server-database-project.aspx).

Sie verfügen jetzt über 4 SQL-Skriptdateien, aber keine tatsächlichen Tabellen. Sie sind bereit für die Bereitstellung des Datenbankprojekts auf "Localdb". Klicken Sie in Visual Studio auf die Schaltfläche "Start" (oder F5) zum Erstellen und Bereitstellen des Datenbankprojekts. Überprüfen Sie die Registerkarte "Ausgabe", um sicherzustellen, dass der Build- und Bereitstellungsprozess wurde erfolgreich abgeschlossen.

![Ausgabe anzeigen](setting-up-database/_static/image7.png)

Um zu sehen, dass die neue Datenbank erstellt wurde, öffnen Sie **Objekt-Explorer von SQL Server** und suchen Sie nach den Namen des Projekts in der richtigen lokalen Datenbankserver (in diesem Fall **(Localdb) \ProjectsV12**).

![Anzeigen der neuen Datenbank](setting-up-database/_static/image8.png)

Um zu sehen, dass die Tabellen mit Daten aufgefüllt werden, mit der rechten Maustaste in einer Tabellenstatus, und wählen **Ansichtsdaten**.

![Anzeigen von Tabellendaten](setting-up-database/_static/image9.png)

Eine bearbeitbare Ansicht der Tabellendaten wird angezeigt.

![Anzeigen der Ergebnisse der Tabelle](setting-up-database/_static/image10.png)

Ihre Datenbank weist nun eingerichtet und mit Daten aufgefüllt. In den nächsten Lernprogrammen erstellen Sie eine Webanwendung für die Datenbank.

> [!div class="step-by-step"]
> [Nächste](creating-the-web-application.md)
