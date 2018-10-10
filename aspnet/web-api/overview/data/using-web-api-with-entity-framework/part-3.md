---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-3
title: Verwenden Sie Code First-Migrationen, um die Datenbank ein Seeding | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 76e2013a-65b7-488c-834d-9448ecea378e
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-3
msc.type: authoredcontent
ms.openlocfilehash: 733b1c343d774e5fa8757808be07a9ae67481d84
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2018
ms.locfileid: "48910498"
---
<a name="use-code-first-migrations-to-seed-the-database"></a>Verwenden Sie Code First-Migrationen, um die Datenbank ein Seeding
====================
durch [Mike Wasson](https://github.com/MikeWasson)

[Abgeschlossenes Projekt herunterladen](https://github.com/MikeWasson/BookService)

In diesem Abschnitt verwenden Sie [Code First-Migrationen](https://msdn.microsoft.com/data/jj591621) in EF das Seeding der Datenbank mit Testdaten.

Von der **Tools** , wählen Sie im Menü **NuGet Package Manager**, und wählen Sie dann **-Paket-Manager-Konsole**. Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl aus:

[!code-console[Main](part-3/samples/sample1.cmd)]

Dieser Befehl fügt einen Ordner namens Migrationen zu Ihrem Projekt sowie eine Codedatei namens Configuration.cs in den Ordner "Migrations".

![](part-3/_static/image1.png)

Öffnen Sie die Configuration.cs-Datei. Fügen Sie die folgenden **mit** Anweisung.

[!code-csharp[Main](part-3/samples/sample2.cs)]

Klicken Sie dann fügen Sie den folgenden Code der **Configuration.Seed** Methode:

[!code-csharp[Main](part-3/samples/sample3.cs)]

Geben Sie im Fenster Paket-Manager-Konsole die folgenden Befehle aus:

[!code-console[Main](part-3/samples/sample4.cmd)]

Der erste Befehl generiert Code, der die Datenbank erstellt, und mit dem zweite Befehl wird dieser Code ausgeführt. Die Datenbank wird lokal mit erstellt [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx).

![](part-3/_static/image2.png)

## <a name="explore-the-api-optional"></a>Erkunden der API (Optional)

Drücken Sie F5, um die Anwendung im Debugmodus auszuführen. Visual Studio startet IIS Express und Ihrer Web-app ausgeführt. Visual Studio klicken Sie dann einen Browser und der app-Startseite wird geöffnet.

Wenn Visual Studio ein Webprojekt ausgeführt wird, weist sie eine Portnummer an. In der folgenden Abbildung ist die Nummer des Ports 50524. Wenn Sie die Anwendung ausführen, sehen Sie eine andere Portnummer an.

![](part-3/_static/image3.png)

Auf der Startseite wird die Verwendung von ASP.NET MVC implementiert. Bei den oberen Rand der Seite wird eine Verknüpfung mit dem Text "API". Diesen Link gelangen Sie zu einer automatisch generierten Hilfeseite an, für die Web-API. (Um zu erfahren, wie diese Hilfeseite generiert wird und wie Sie Ihre eigene Dokumentation auf der Seite hinzufügen können, finden Sie unter [für ASP.NET Web API Help Pages erstellen](../../getting-started-with-aspnet-web-api/creating-api-help-pages.md).) Sie können Seite enthält Links, um die Details der API, einschließlich der Anforderung und Antwort-Format finden auf die Hilfe klicken.

![](part-3/_static/image4.png)

Der API können CRUD-Vorgänge in der Datenbank. Im folgenden werden die API zusammengefasst.

| Authors |  |
| --- | -- |
| Abrufen der api/authors | Rufen Sie alle Autoren. |
| GET-api/Authors / {Id} | Erhalten Sie einen Autor anhand der ID. |
| POST/api/authors | Erstellen Sie einen neuen Autor. |
| PUT/API/Authors / {Id} | Aktualisieren eines vorhandenen Autors an. |
| Löschen Sie/API/Authors / {Id} | Löschen eines Autors an. |

| Bücher |  |
| --- | -- |
| /Api/books abrufen | Alle Bücher zu erhalten. |
| Abrufen Sie/API/Books / {Id} | Erhalten Sie ein Buch anhand der ID. |
| POST/api/Bücher | Erstellen Sie ein neues Buch. |
| PUT/API/Books / {Id} | Aktualisieren Sie ein vorhandenes Buch. |
| Löschen Sie/API/Books / {Id} | Löschen Sie ein Buch. |

## <a name="view-the-database-optional"></a>Zeigen Sie die Datenbank (Optional)

Wenn Sie den Update-Database-Befehl ausgeführt haben, EF die Datenbank erstellt und wird aufgerufen, die `Seed` Methode. Wenn Sie die Anwendung lokal ausführen, verwendet EF [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx). Sie können die Datenbank in Visual Studio anzeigen. Von der **Ansicht** , wählen Sie im Menü **Objekt-Explorer von SQL Server**.

![](part-3/_static/image5.png)

In der **Herstellen einer Verbindung mit Server** Dialogfeld, in der **Servernamen** "Bearbeiten", geben Sie "(Localdb) \v11.0". Lassen Sie die **Authentifizierung** Option als "Windows-Authentifizierung". Klicken Sie auf **Verbinden**.

![](part-3/_static/image6.png)

Visual Studio eine Verbindung mit LocalDB her und zeigt Ihre vorhandenen Datenbanken im Objekt-Explorer von SQL Server-Fenster. Sie können die Knoten, um die Tabellen anzuzeigen, die EF erstellt erweitern.

![](part-3/_static/image7.png)

Klicken Sie zum Anzeigen der Daten, mit der rechten Maustaste in einer Tabelle, und wählen Sie **Ansichtsdaten**.

![](part-3/_static/image8.png)

Der folgende Screenshot zeigt die Ergebnisse für die Bücher-Tabelle. Beachten Sie, dass EF, die Datenbank mit der Seed-Daten aufgefüllt, und die Tabelle die Fremdschlüssel für die Authors-Tabelle enthält.

![](part-3/_static/image9.png)

> [!div class="step-by-step"]
> [Zurück](part-2.md)
> [Weiter](part-4.md)
