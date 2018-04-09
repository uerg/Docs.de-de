---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
title: 'Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mit Visual Studio oder Visual Web Developer: Bereitstellen eines Datenbankupdates - 9 von 12 | Microsoft Docs'
author: tdykstra
description: Diese Reihe von Lernprogrammen wird gezeigt, wie das Bereitstellen einer ASP.NET-Anwendung (veröffentlichen) Webanwendungsprojekt, die eine SQL Server Compact-Datenbank enthält, mithilfe von Visual das...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: a8d776af-4735-4612-87f6-9f326587f2d3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
msc.type: authoredcontent
ms.openlocfilehash: 0a00f9d3ed284ebbc1d83c1b5696436e5ba00f4b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-database-update---9-of-12"></a>Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mit Visual Studio oder Visual Web Developer: Bereitstellen eines Datenbankupdates - 9 von 12
====================
durch [Tom Dykstra](https://github.com/tdykstra)

[Startprojekt herunterladen](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Diese Reihe von Lernprogrammen wird gezeigt, wie das Bereitstellen einer ASP.NET-Anwendung (veröffentlichen) Webanwendungsprojekt, die eine SQL Server Compact-Datenbank mithilfe von Visual Studio 2012 RC oder Visual Studio Express 2012 RC für das Web enthält. Sie können auch Visual Studio 2010 verwenden, wenn Sie das Web veröffentlichen Update installieren. Eine Einführung in der Reihe, finden Sie unter [im ersten Lernprogramm, in der Reihe](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Ein Lernprogramm, die die Bereitstellungsfeatures nach der RC-Version von Visual Studio 2012 eingeführt wurde, wird gezeigt, wie SQL Server-Editionen als SQL Server Compact bereitstellen, und zeigt die Vorgehensweise beim Bereitstellen in Azure App Service-Web-Apps, finden Sie unter [ASP.NET Web Deploy Mithilfe von Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Übersicht

In diesem Lernprogramm stellen Sie eine Datenbank und zugehörigem codeänderungen, testen Sie die Änderungen in Visual Studio, und klicken Sie dann das Update für sowohl die Test-und produktionsumgebungen bereitstellen.

Hinweis: Wenn Sie eine Fehlermeldung erhalten, oder etwas funktioniert nicht, wenn Sie das Lernprogramm absolvieren, müssen Sie überprüfen die [Website](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="adding-a-new-column-to-a-table"></a>Hinzufügen einer neuen Spalte zu einer Tabelle

In diesem Abschnitt fügen Sie eine Birth Date-Spalte zu den `Person` Basisklasse für die `Student` und `Instructor` Entitäten. Klicken Sie dann aktualisieren Sie die Seite, die Instructor-Daten zeigt an, dass die neue Spalte angezeigt.

In der *ContosoUniversity.DAL* geöffneten Projekts *Person.cs* und fügen Sie die folgende Eigenschaft am Ende der `Person` Klasse (es darf zwei schließende geschweifte Klammern, die darauf folgende):

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample1.cs)]

Als Nächstes Aktualisieren der Seed-Methode, damit sie einen Wert für die neue Spalte bereitstellt. Open *Migrations\Configuration.cs* , und Ersetzen Sie den Codeblock, der beginnt, `var instructors = new List<Instructor>` mit den folgenden Codeblock das Geburtsdatum enthält:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample2.cs)]

Öffnen Sie im Projekt ContosoUniversity *Instructors.aspx* und Hinzufügen eines neuen Vorlage-Felds, um das Geburtsdatum anzuzeigen. Fügen Sie es zwischen den Vorgängen für Hire Date und Office-Zuordnung hinzu:

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample3.aspx)]

(Wenn Code Einzug synchron abgerufen werden, können Sie STRG-K und dann STRG + D, um die Datei automatisch neu zu formatieren drücken.)

Erstellen Sie die Projektmappe, und öffnen Sie die **Package Manager Console** Fenster. Sicherstellen, dass als ContosoUniversity.DAL ausgewählter der **Standardprojekt**.

In der **Package Manager Console** wählen **ContosoUniversity.DAL** als die **Standardprojekt**, und geben Sie dann den folgenden Befehl aus:

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample4.ps1)]

Wenn dieser Befehl abgeschlossen ist, handelt es sich bei Visual Studio öffnet die Klassendatei, die die neue definiert `DbMIgration` -Klasse, und klicken Sie in der `Up` Methode sehen Sie den Code, der die neue Spalte erstellt.

![AddBirthDate_migration_code](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image1.png)

Erstellen Sie die Projektmappe, und geben Sie dann den folgenden Befehl in der **Package Manager Console** Fenster (Stellen Sie sicher, dass das ContosoUniversity.DAL-Projekt ausgewählt ist):

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample5.ps1)]

Wenn der Befehl abgeschlossen ist, führen Sie die Anwendung, und wählen Sie der Seite "Lehrkräfte". Beim Laden der Seite angezeigt, dass die neue verfügt über Birth Date-Felds.

[![Instructors_page_with_birth_date](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image3.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image2.png)

## <a name="deploying-the-database-update-to-the-test-environment"></a>Das Datenbankupdate bereitstellen, in der Testumgebung

In **Projektmappen-Explorer** wählen Sie das Projekt ContosoUniversity.

In der **Web eine klicken Sie auf Publish** Symbolleiste, wählen die **Test** Veröffentlichungsprofil, und klicken Sie dann auf **Web veröffentlichen**. (Wenn die Symbolleiste deaktiviert ist, wählen Sie das Projekt ContosoUniversity **Projektmappen-Explorer**.)

Visual Studio wird die aktualisierte Anwendung bereitgestellt, und der Browser geöffnet wird, um zur Startseite. Führen Sie die Lehrkräfte-Seite, um sicherzustellen, dass das Update erfolgreich bereitgestellt wurde. Wenn die Anwendung versucht, den Zugriff auf die Datenbank für diese Seite, Code First aktualisiert das Datenbankschema und führt die `Seed` Methode. Wenn die Seite angezeigt wird, sehen Sie den erwarteten **Geburtsdatum** Spalte mit Datumsangaben darin.

[![Instructors_page_with_birth_date_Test](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image4.png)

## <a name="deploying-the-database-update-to-the-production-environment"></a>Das Datenbankupdate Bereitstellen in der Produktionsumgebung.

Sie können nun bis hin zur Produktion bereitstellen. Der einzige Unterschied ist, dass Sie gestützte *app\_offline.htm* zu verhindern, dass Benutzer Zugriff auf die Website und wodurch die Datenbank aktualisiert, während Sie Änderungen bereitstellen. Führen Sie die folgenden Schritte aus, für die produktionsbereitstellung:

- Hochladen der *app\_offline.htm* Datei des Produktionsstandorts.
- Wählen Sie in Visual Studio das Profil "Produktion" in der **Web eine klicken Sie auf Publish** Symbolleiste, und klicken Sie auf **Web veröffentlichen**.
- Löschen der *app\_offline.htm* Datei des Produktionsstandorts.

> [!NOTE]
> Während die Anwendung in der produktionsumgebung eingesetzt wird sollten Sie einen Sicherungsplan implementieren. D. h., Sie sollten werden in regelmäßigen Abständen Kopieren der *School-Prod.sdf* und *Aspnet-Prod.sdf* Dateien aus der Produktion-Standort an einen sicheren Speicherort und Generationen solcher beibehalten werden soll Sicherungen. Wenn Sie die Datenbank aktualisieren, sollten Sie eine Sicherungskopie von unmittelbar vor der Änderung. Klicken Sie dann, wenn Sie ein Fehler unterläuft und nicht erst ermitteln, nachdem Sie es in der produktionsumgebung bereitgestellt haben, noch werden Sie können zum Wiederherstellen der Datenbank in den Zustand, der vorlag, bevor er beschädigt wurde.


Wenn Visual Studio die URL der Startseite im Browser, öffnet der *app\_offline.htm* Seite wird angezeigt. Nach dem Löschen der *app\_offline.htm* -Datei, die Sie durchsuchen können, an Ihre Startseite erneut, um sicherzustellen, dass das Update erfolgreich bereitgestellt wurde.

[![Instructors_page_with_birth_date_Prod](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image7.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image6.png)

Sie haben jetzt ein Anwendungsupdate bereitgestellt, die Änderung an einer Datenbank zu Test- und produktionsumgebungen enthalten. Der nächste Lernprogrammen wird gezeigt, wie die Datenbank von SQL Server Compact, SQL Server Express und SQL Server migriert.

> [!div class="step-by-step"]
> [Zurück](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
> [Weiter](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
