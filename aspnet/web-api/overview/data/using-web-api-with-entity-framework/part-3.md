---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-3
title: Code First-Migrationen verwenden, um das Seeding der Datenbank | Microsoft Docs
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 76e2013a-65b7-488c-834d-9448ecea378e
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-3
msc.type: authoredcontent
ms.openlocfilehash: 1ca627397f0f100d13388f9afc27ff481886e098
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="use-code-first-migrations-to-seed-the-database"></a>Verwenden Sie Code First-Migrationen, um das Seeding der Datenbank
====================
durch [Mike Wasson](https://github.com/MikeWasson)

[Herunterladen des abgeschlossenen Projekts](https://github.com/MikeWasson/BookService)

In diesem Abschnitt verwenden Sie [Code First-Migrationen](https://msdn.microsoft.com/data/jj591621) in EF zum Ausgangswert für der Datenbank mit Testdaten.

Aus der **Tools** klicken Sie im Menü **Bibliothekspaket-Manager**, und wählen Sie dann **Package Manager Console**. Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl aus:

[!code-console[Main](part-3/samples/sample1.cmd)]

Dieser Befehl fügt einen Ordner namens Migrationen zu Ihrem Projekt plus eine Codedatei mit dem Namen "Configuration.cs" in den Ordner.

![](part-3/_static/image1.png)

Öffnen Sie die Datei "Configuration.cs". Fügen Sie die folgenden **mit** Anweisung.

[!code-csharp[Main](part-3/samples/sample2.cs)]

Fügen Sie folgenden Code, der **Configuration.Seed** Methode:

[!code-csharp[Main](part-3/samples/sample3.cs)]

Geben Sie im Fenster Paket-Manager-Konsole die folgenden Befehle ein:

[!code-console[Main](part-3/samples/sample4.cmd)]

Der erste Befehl generiert Code, der die Datenbank erstellt und mit dem zweite Befehl wird dieser Code ausgeführt. Die Datenbank wird lokal mit erstellt [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx).

![](part-3/_static/image2.png)

## <a name="explore-the-api-optional"></a>Untersuchen Sie die API (Optional)

Drücken Sie F5, um die Anwendung im Debugmodus auszuführen. Visual Studio startet IIS Express und Ihre Web-app ausführt. Visual Studio dann öffnet einen Browser und Startseite der app geöffnet.

Wenn Visual Studio ein Webprojekt ausgeführt wird, weist es eine Portnummer an. In der folgenden Abbildung ist die Nummer des Ports 50524. Wenn Sie die Anwendung ausführen, sehen Sie eine andere Portnummer an.

![](part-3/_static/image3.png)

Die Startseite wird mithilfe von ASP.NET MVC implementiert. Klicken Sie oben auf der Seite besteht eine Verknüpfung, die besagt, dass "API" zur Verfügung. Diesen Link gelangen Sie zur einer automatisch generierten Hilfeseite an, für die Web-API. (Um zu erfahren, wie diese Hilfeseite generiert wird und wie Sie Ihre eigenen Dokumentation auf der Seite hinzufügen können, finden Sie unter [Hilfeseiten für ASP.NET Web-API erstellen](../../getting-started-with-aspnet-web-api/creating-api-help-pages.md).) Sie können auf "Hilfe" die Seite enthält Links, um Details über die API, einschließlich der Anforderung und Antwort-Format anzuzeigen klicken.

![](part-3/_static/image4.png)

Die API ermöglicht CRUD-Vorgänge in der Datenbank. Im folgenden werden die API zusammengefasst.

| Authors |  |
| --- | -- |
| Api-Autoren abrufen | Rufen Sie aller Autoren ab. |
| GET api/authors/{id} | Einen Autor-ID abrufen |
| POST/api/Autoren | Erstellen Sie eine neue erstellen. |
| PUT /api/authors/{id} | Aktualisieren eines vorhandenen Autors. |
| DELETE /api/authors/{id} | Löschen eines Autors an. |

| Bücher |  |
| --- | -- |
| /Api/books abrufen | Rufen Sie aller Bücher an ab. |
| GET /api/books/{id} | Ein Buch-ID abrufen |
| Bereitstellen Sie/api/Bücher | Erstellen Sie ein neues Buch. |
| PUT /api/books/{id} | Aktualisieren Sie ein vorhandenes Buch. |
| DELETE /api/books/{id} | Löschen Sie ein Buch. |

## <a name="view-the-database-optional"></a>Anzeigen der Datenbank (Optional)

Beim Ausführen den Update-Database-Befehl wurde die Datenbank erstellt und aufgerufen EF die `Seed` Methode. Wenn Sie die Anwendung lokal ausführen, EF verwendet [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx). Sie können die Datenbank in Visual Studio anzeigen. Aus der **Ansicht** klicken Sie im Menü **Objekt-Explorer von SQL Server**.

![](part-3/_static/image5.png)

In der **Verbindung mit Server herstellen** Dialogfeld in der **Servernamen** Bearbeitungsfeld, geben Sie "(Localdb) \v11.0". Lassen Sie die **Authentifizierung** Option als "Windows-Authentifizierung". Klicken Sie auf **verbinden**.

![](part-3/_static/image6.png)

Visual Studio eine Verbindung mit LocalDB her und zeigt Ihre vorhandenen Datenbanken im Objekt-Explorer von SQL Server-Fenster. Sie können die Knoten, um die Tabellen anzuzeigen, die EF erstellt erweitern.

![](part-3/_static/image7.png)

Klicken Sie zum Anzeigen der Daten mit der rechten Maustaste in einer Tabellenstatus, und wählen Sie **Ansichtsdaten**.

![](part-3/_static/image8.png)

Der folgende Screenshot zeigt die Ergebnisse für die Bücher-Tabelle. Beachten Sie, dass EF, die Datenbank mit den Daten Ausgangswert aufgefüllt und die Tabelle die Fremdschlüssel für die Authors-Tabelle enthält.

![](part-3/_static/image9.png)

>[!div class="step-by-step"]
[Zurück](part-2.md)
[Weiter](part-4.md)
