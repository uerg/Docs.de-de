---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
title: 'Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mit Visual Studio oder Visual Web Developer: Bereitstellen eines Datenbankupdates - 9 von 12 | Microsoft-Dokumentation'
author: tdykstra
description: In dieser tutorialreihe erfahren Sie, wie zum Bereitstellen einer ASP.NET-Anwendung (veröffentlichen) Webanwendungsprojekt, die eine SQL Server Compact-Datenbank enthält, mithilfe von Visual Stu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: a8d776af-4735-4612-87f6-9f326587f2d3
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
msc.type: authoredcontent
ms.openlocfilehash: e94281e36192c91a04392735af318bbc517b0521
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382458"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-database-update---9-of-12"></a>Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mit Visual Studio oder Visual Web Developer: Bereitstellen eines Datenbankupdates - 9 von 12
====================
durch [Tom Dykstra](https://github.com/tdykstra)

[Startprojekt herunterladen](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> In dieser tutorialreihe erfahren Sie, wie zum Bereitstellen einer ASP.NET-Anwendung (veröffentlichen) Webanwendungsprojekt, das eine SQL Server Compact-Datenbank mithilfe von Visual Studio 2012 RC oder Visual Studio Express 2012 RC für Web enthält. Sie können auch Visual Studio 2010 verwenden, wenn Sie die Web Publish Update installieren. Eine Einführung in die Reihe, finden Sie unter [im ersten Tutorial der Reihe](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Ein Lernprogramm, das zeigt, Bereitstellungsfunktionen, die nach der RC-Version von Visual Studio 2012 eingeführt wurden, zeigt, wie zum Bereitstellen von SQL Server-Editionen als SQL Server Compact und zeigt, wie Sie in Azure App Service-Web-Apps bereitstellen, finden Sie unter [ASP.NET-webbereitstellung Mithilfe von Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Übersicht

In diesem Tutorial stellen Sie eine Datenbank und zugehörigen Code-Änderungen, testen Sie die Änderungen in Visual Studio, und klicken Sie dann das Update bereitstellen, um sowohl die Test-und produktionsumgebungen.

Erinnerung: Wenn Sie eine Fehlermeldung erhalten, oder etwas nicht funktioniert, wie Sie das Lernprogramm durchzuarbeiten, sollten Sie unbedingt die [Problembehandlungsseite](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="adding-a-new-column-to-a-table"></a>Hinzufügen einer neuen Spalte in einer Tabelle

In diesem Abschnitt fügen Sie eine Birth Date-Spalte, die `Person` Basisklasse für die `Student` und `Instructor` Entitäten. Klicken Sie dann aktualisieren Sie die Seite mit Daten für "Instructor", damit die neue Spalte angezeigt.

In der *ContosoUniversity.DAL* -Projekt, öffnen *Person.cs* und fügen Sie die folgende Eigenschaft am Ende der `Person` Klasse (es muss zwei schließende geschweifte Klammern, die darauf folgende):

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample1.cs)]

Aktualisieren Sie als Nächstes die Seed-Methode auf, sodass sie einen Wert für die neue Spalte bereitstellt. Open *migrations\configuration. cs* , und Ersetzen Sie den Codeblock, der beginnt `var instructors = new List<Instructor>` mit den folgenden Codeblock, der Geburtsdatum enthält:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample2.cs)]

Öffnen Sie im Projekt ContosoUniversity *Instructors.aspx* und Hinzufügen eines neuen Vorlagenfelds zum Anzeigen des Geburtsdatums. Fügen Sie sie zwischen den Vorgängen für Datum und die Office-Zuweisung Hire hinzu:

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample3.aspx)]

(Wenn der codeeinzug nicht synchronisiert wird, können Sie STRG + K und dann STRG + D, um die Datei automatisch neu formatieren drücken.)

Erstellen Sie die Projektmappe, und öffnen Sie die **-Paket-Manager-Konsole** Fenster. Sicherstellen, dass ContosoUniversity.DAL noch, als ausgewählt ist die **Standardprojekt**.

In der **-Paket-Manager-Konsole** wählen Sie im Fenster **ContosoUniversity.DAL** als die **Standardprojekt**, und geben Sie dann den folgenden Befehl aus:

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample4.ps1)]

Wenn dieser Befehl abgeschlossen ist, handelt es sich bei Visual Studio öffnet die Klassendatei, die die neue definiert `DbMIgration` -Klasse, und klicken Sie in der `Up` Methode sehen Sie den Code, der die neue Spalte erstellt.

![AddBirthDate_migration_code](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image1.png)

Erstellen Sie die Projektmappe, und geben Sie dann den folgenden Befehl in der **-Paket-Manager-Konsole** Fenster (Stellen Sie sicher, dass das Projekt ContosoUniversity.DAL immer noch ausgewählt ist):

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample5.ps1)]

Wenn der Befehl abgeschlossen ist, führen Sie die Anwendung aus, und wählen Sie die dozentenseite. Beim Laden der Seite angezeigt, dass sie die neue hat Birth Date-Felds.

[![Instructors_page_with_birth_date](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image3.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image2.png)

## <a name="deploying-the-database-update-to-the-test-environment"></a>Bereitstellen des Datenbankupdates in der Testumgebung

In **Projektmappen-Explorer** wählen Sie das Projekt ContosoUniversity.

In der **klicken Sie auf Webveröffentlichung mit einem** -Symbolleiste auf die **Test** Veröffentlichungsprofil aus, und klicken Sie dann auf **Webveröffentlichung**. (Wenn die Symbolleiste deaktiviert ist, wählen Sie das Projekt "ContosoUniversity" im **Projektmappen-Explorer**.)

Visual Studio stellt die aktualisierte Anwendung bereit, und der Browser zur Startseite geöffnet wird. Führen Sie die "Dozenten", um sicherzustellen, dass das Update wurde erfolgreich bereitgestellt wurde. Wenn die Anwendung versucht, die Zugriff auf die Datenbank für diese Seite, Code First aktualisiert das Schema der Datenbank und führt die `Seed` Methode. Wenn die Seite angezeigt wird, sehen Sie den erwarteten **Birth Date** Spalte mit Datumsangaben in es.

[![Instructors_page_with_birth_date_Test](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image4.png)

## <a name="deploying-the-database-update-to-the-production-environment"></a>Das Datenbankupdate Bereitstellen in der Produktionsumgebung

Sie können jetzt in der produktionsumgebung bereitstellen. Der einzige Unterschied ist, dass Sie verwenden *app\_offline.htm* zu verhindern, dass Benutzer Zugriff auf die Website, und daher die Datenbank aktualisiert, während Sie Änderungen bereitstellen. Bereitstellung für die Produktion führen Sie die folgenden Schritte aus:

- Hochladen der *app\_offline.htm* Datei mit dem Produktionsstandort.
- Wählen Sie in Visual Studio das Profil "Produktion" in der **klicken Sie auf Webveröffentlichung mit einem** Symbolleiste, und klicken Sie auf **Webveröffentlichung**.
- Löschen der *app\_offline.htm* Datei auf der Produktionswebsite.

> [!NOTE]
> Während der Verwendung Ihrer Anwendung in der produktionsumgebung ist sollten Sie einen Sicherungsplan implementieren. Das heißt, Sie sollten werden in regelmäßigen Abständen Kopieren der *School-Prod.sdf* und *Aspnet-Prod.sdf* Dateien aus der Produktion Standort an einen sicheren Speicherort, und Sie sollten mehrere Generationen solcher beibehalten werden Sicherungen. Wenn Sie die Datenbank aktualisieren, sollten Sie eine Sicherungskopie von unmittelbar vor der Änderung. Klicken Sie dann, wenn Sie ein Fehler unterläuft und nicht erst erkennen, nachdem Sie es in der produktionsumgebung bereitgestellt haben, noch werden Sie die Datenbank in den Zustand wiederherstellen, die er sich befand, bevor er beschädigt.


Wenn Visual Studio die URL der Startseite im Browser öffnet die *app\_offline.htm* angezeigt wird. Nach dem Löschen der *app\_offline.htm* -Datei, die Sie durchsuchen können, an Ihre Startseite erneut aus, um sicherzustellen, dass das Update wurde erfolgreich bereitgestellt wurde.

[![Instructors_page_with_birth_date_Prod](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image7.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image6.png)

Sie haben jetzt ein Anwendungsupdate bereitgestellt, die Änderung an einer Datenbank zu Test- und produktionsumgebungen zu enthalten. Im nächste Tutorial erfahren Sie, wie Sie Ihre Datenbank aus SQL Server Compact, SQL Server Express und SQL Server zu migrieren.

> [!div class="step-by-step"]
> [Zurück](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
> [Weiter](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
