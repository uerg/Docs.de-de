---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
title: Erstellen eines Teamprojekts in TFS | Microsoft-Dokumentation
author: jrjlee
description: In diesem Thema wird beschrieben, wie Sie ein neues Teamprojekt in Team Foundation Server (TFS) 2010 erstellen.
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: b28d3e2d-0bb4-4e29-a780-af810b964722
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
msc.type: authoredcontent
ms.openlocfilehash: 98725b9d2f669e6520f24c3a8122be086002e96a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37810223"
---
<a name="creating-a-team-project-in-tfs"></a>Erstellen eines Teamprojekts in TFS
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In diesem Thema wird beschrieben, wie Sie ein neues Teamprojekt in Team Foundation Server (TFS) 2010 erstellen.


In diesem Thema ist Teil einer Reihe von Tutorials, die auf der Basis der bereitstellungsanforderungen Enterprise ein fiktives Unternehmen, die mit dem Namen Fabrikam, Inc. Dieser tutorialreihe verwendet eine beispiellösung&#x2014;der [Contact Manager-Lösung](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;zur Darstellung einer Webanwendung mit einem realistischen Maß an Komplexität, einschließlich einer ASP.NET MVC 3-Anwendung, eine Windows-Kommunikation Foundation (WCF)-Dienst und ein Datenbankprojekt.

## <a name="task-overview"></a>Übersicht über den Task

Für das Bereitstellen und ein neues Teamprojekt in TFS verwenden, müssen Sie die folgenden allgemeinen Schritte ausführen:

- Erteilen Sie Berechtigungen für den Benutzer, der das neue Teamprojekt erstellen sollen.
- Erstellen Sie das Teamprojekt.
- Gewähren von Berechtigungen Sie für die Teammitglieder, die auf das Projekt zu funktionieren.
- Überprüfen Sie in Teil des Inhalts.

In diesem Thema wird gezeigt, wie Sie diese Schritte ausführen, und es erkennt, die Benutzer und die Auftrags-Rollen, die wahrscheinlich für jede Prozedur verantwortlich sind. Denken Sie daran, dass Sie abhängig von der Struktur Ihrer Organisation, diese Aufgaben die Verantwortung für eine andere Person sein kann.

Die Aufgaben und exemplarische Vorgehensweisen in diesem Thema wird davon ausgegangen, die Sie installiert und konfiguriert TFS, und Sie eine Teamprojektsammlung im Rahmen des Konfigurationsvorgangs erstellt haben. Weitere Informationen zu diesen Annahmen und weitere allgemeine Informationen zu dem Szenario, finden Sie unter [konfigurieren Sie eine TFS-Build-Server für die Webbereitstellung](configuring-a-tfs-build-server-for-web-deployment.md).

## <a name="grant-permissions-to-the-team-project-creator"></a>Gewähren von Berechtigungen für den Teamprojektersteller

Um ein neues Teamprojekt erstellen zu können, benötigen Sie diese Berechtigungen:

- Sie benötigen die **neue Projekte erstellen** -Berechtigung für die TFS-Anwendungsebene. Sie erteilen diese Berechtigung in der Regel durch Hinzufügen von Benutzern, die **Projektauflistungsadministratoren** TFS-Gruppe. Die **Team Foundation-Administratoren** globale Gruppe enthält auch diese Berechtigung.
- Sie benötigen die Berechtigung zum Erstellen neuer Team-Standorte innerhalb der SharePoint-Websitesammlung, die die TFS-Teamprojektsammlung entspricht. Sie erteilen diese Berechtigung in der Regel durch Hinzufügen des Benutzers zu einer SharePoint-Gruppe mit **Vollzugriff** Rechte auf dem SharePoint-websiteauflistung.
- Wenn Sie SQL Server Reporting Services-Funktionen verwenden, müssen Sie Mitglied werden die **Team Foundation-Inhalts-Manager** Rolle in Reporting Services.

### <a name="who-performs-these-procedures"></a>Wem diese Verfahren werden?

In der Regel führt die Person oder Gruppe, die die TFS-Bereitstellung verwaltet auch diese Verfahren aus.

Da dies einen weitreichenden Satz von Berechtigungen ist, werden neue Teamprojekte in der Regel von einer kleinen Gruppe von Benutzern mit Verantwortung für die Verwaltung einer TFS-Bereitstellung erstellt. Entwickler nicht in der Regel die erforderlichen Berechtigungen zum Erstellen neuer Teamprojekte erhält.

### <a name="grant-permissions-in-tfs"></a>Erteilen von Berechtigungen in TFS

Wenn Sie einen Benutzer zum Erstellen neuer Teamprojekte aktivieren möchten, ist die allgemeine erste Aufgabe beim Hinzufügen des Benutzers, der **Projektauflistungsadministratoren** für die Teamprojektsammlung.

**Einen Benutzer der Gruppe Administratoren der Projektsammlung hinzufügen**

1. Auf dem TFS-Server auf die **starten** , zeigen Sie auf **Programme**, klicken Sie auf **Microsoft Team Foundation Server 2010**, und klicken Sie dann auf **Team Foundation -Verwaltungskonsole**.
2. Erweitern Sie in der Strukturansicht Navigation **Anwendungsebene**, und klicken Sie dann auf **Team Project Collections**.

    ![](creating-a-team-project-in-tfs/_static/image1.png)
3. In der **Team Project Collections** wählen Sie im Bereich der teamprojektauflistung, die Sie verwalten möchten.

    ![](creating-a-team-project-in-tfs/_static/image2.png)
4. Auf der **allgemeine** auf **Gruppenmitgliedschaft**.

    ![](creating-a-team-project-in-tfs/_static/image3.png)
5. In der **globale Gruppen** wählen Sie im Dialogfeld die **Projektauflistungsadministratoren** gruppieren, und klicken Sie dann auf **Eigenschaften**.
6. In der **Team Foundation Server-Gruppeneigenschaften** wählen Sie im Dialogfeld **Windows-Benutzer oder Gruppe**, und klicken Sie dann auf **hinzufügen**.

    ![](creating-a-team-project-in-tfs/_static/image4.png)
7. In der **Auswahl von Benutzern, Computern oder Gruppen** Dialogfeld geben der Benutzernamen des Benutzers neue Teamprojekte erstellt werden sollen. Klicken Sie auf **Namen überprüfen**, und klicken Sie dann auf **OK** .

    ![](creating-a-team-project-in-tfs/_static/image5.png)
8. In der **Team Foundation Server-Gruppeneigenschaften** Dialogfeld klicken Sie auf **OK**.
9. In der **globale Gruppen** Dialogfeld klicken Sie auf **schließen**.

### <a name="grant-permissions-in-sharepoint-services"></a>Erteilen von Berechtigungen in SharePointServices

Als Nächstes müssen Sie neue Teamwebsites in der SharePoint-Websitesammlung zu erstellen, die die TFS-Teamprojektsammlung entspricht, die Benutzer die Berechtigung gewähren.

**Erteilen von Vollzugriffsberechtigungen für die SharePoint-Websitesammlung**

1. In der Team Foundation Server-Verwaltungskonsole auf der **Team Project Collections** Seite, wählen Sie die Teamprojektsammlung, die Sie verwalten möchten.
2. Auf der **SharePoint-Website** Registerkarte, beachten Sie den Wert der **aktueller Standardort der Website** URL.

    ![](creating-a-team-project-in-tfs/_static/image6.png)
3. Öffnen Sie Internet Explorer, und fahren Sie mit der URL, die Sie in Schritt 2 notiert haben.

    > [!NOTE]
    > Wenn Sie nicht auf Windows als Benutzer, die die Teamprojektsammlung erstellt hat angemeldet sind, müssen Sie in SharePoint als dieser Benutzer melden Sie sich um fortzufahren.
4. Auf der **Websiteaktionen** Menü klicken Sie auf **Standorteinstellungen**.

    ![](creating-a-team-project-in-tfs/_static/image7.png)
5. Auf der **Standorteinstellungen** Seite **Benutzern und Berechtigungen**, klicken Sie auf **Personen und Gruppen**.
6. Klicken Sie im linken Bereich auf **Gruppen**.

    ![](creating-a-team-project-in-tfs/_static/image8.png)
7. Auf der **Personen und Gruppen: alle Gruppen** auf **Gruppen einrichten für diesen Standort**.

    ![](creating-a-team-project-in-tfs/_static/image9.png)

   > [!NOTE]
   > Erhalten Sie möglicherweise eine <strong>HTTP 404 Not Found</strong> Fehler aufgrund eines doppelten Codierung HTTP-Fehlers. In diesem Fall ersetzen Sie dabei die URL:   
   > `[site_collection_URL]/_layouts/permsetup.aspx` Zum Beispiel:  
   > `http://tfs/sites/Fabrikam%20Web%20Projects/_layouts/permsetup.aspx` 
8. Auf der **Gruppen einrichten für diesen Standort** Seite fügen Sie den Benutzer, die Teamprojekte zu erstellen, wird die **Besitzer** gruppieren, und klicken Sie dann auf **OK**.

    ![](creating-a-team-project-in-tfs/_static/image10.png)

Weitere Informationen zum Aktivieren von Benutzern die Erstellung neuer Teamprojekte innerhalb einer Teamprojektsammlung finden Sie unter [Festlegen von Administratorberechtigungen für Teamprojektsammlungen](https://msdn.microsoft.com/library/dd547204.aspx).

## <a name="create-a-new-team-project-and-add-users"></a>Ein neues Teamprojekt erstellen und Hinzufügen von Benutzern

Nachdem Sie die erforderlichen Berechtigungen verfügen, können Sie mithilfe der **Team Explorer** Fenster in Visual Studio 2010 zum Erstellen eines neuen Teamprojekts. Dieser Ansatz bietet einen Assistenten, der alle erforderlichen Informationen erfasst und führt die erforderlichen Aufgaben in TFS, SharePoint und SQL Server Reporting Services. Sie müssen auch zum Gewähren von Berechtigungen für das neue Teamprojekt für Mitglied des Entwicklerteams, aktivieren sie zum Hinzufügen und Ändern des Inhalts.

### <a name="who-performs-these-procedures"></a>Wem diese Verfahren werden?

In der Regel führt ein TFS-Administrator oder Teamleiter sind Entwickler diese Verfahren aus.

### <a name="create-a-new-team-project"></a>Erstellen Sie ein neues Teamprojekt

Im nächste Verfahren wird beschrieben, erstellen Sie ein neues Teamprojekt in TFS 2010.

**Um ein neues Teamprojekt zu erstellen.**

1. Auf der **starten** Startmenü **Programme**, klicken Sie auf **Microsoft Visual Studio 2010**, mit der rechten Maustaste **Microsoft Visual Studio 2010**, und klicken Sie dann auf **als Administrator ausführen**.

    > [!NOTE]
    > Wenn Sie nicht Visual Studio 2010 als Administrator ausführen, schlägt der Assistent für neue Teamprojekte im letzten Schritt fehl.
2. Wenn die **User Account Control** klicken Sie im angezeigten Dialogfeld **Ja**.
3. In Visual Studio auf die **Team** Menü klicken Sie auf **Herstellen einer Verbindung mit Team Foundation Server**.

    > [!NOTE]
    > Wenn Sie bereits eine Verbindung mit einem TFS-Server konfiguriert haben, können Sie die Schritte 4 bis 7 weglassen.
4. In der **Verbindung mit Teamprojekt** Dialogfeld klicken Sie auf **Server**.
5. In der **Team Foundation Server hinzufügen/entfernen** Dialogfeld klicken Sie auf **hinzufügen**.
6. In der **Team Foundation Server hinzufügen** Dialogfeld Geben Sie die Details Ihrer TFS-Instanz, und klicken Sie dann auf **OK**.

    ![](creating-a-team-project-in-tfs/_static/image11.png)
7. In der **Team Foundation Server hinzufügen/entfernen** Dialogfeld klicken Sie auf **schließen**.
8. In der **Herstellen einer Verbindung mit Teamprojekt** wählen Sie im Dialogfeld die TFS-Instanz, die Sie eine Verbindung herstellen, wählen Sie das Team möchten die projektauflistung zu hinzu, und klicken Sie dann auf **Connect**.

    ![](creating-a-team-project-in-tfs/_static/image12.png)
9. In der **Team Explorer** der rechten Maustaste auf das Team die Projektsammlung, und klicken Sie dann auf **neues Teamprojekt**.

    ![](creating-a-team-project-in-tfs/_static/image13.png)
10. In der **neues Teamprojekt** Dialogfeld Geben Sie einen Namen und eine Beschreibung für das Teamprojekt, und klicken Sie dann auf **Weiter**.

    > [!NOTE]
    > Wenn Ihr Teamprojekt Leerzeichen enthält, können Sie einige Probleme auftreten, auf das Internet Information Services (IIS)-Webbereitstellungstool (Web Deploy) verwenden Sie zum Bereitstellen von Paketen aus den Ausgabepfad. Leerzeichen im Pfad können viel zum Ausführen von Befehlen mit Web Deploy erschweren.

    ![](creating-a-team-project-in-tfs/_static/image14.png)
11. Auf der **Auswählen einer Prozessvorlage** Seite, wählen Sie eine Prozessvorlage, die Sie verwenden, um den Entwicklungsprozess zu verwalten, und klicken Sie dann auf möchten **Weiter**.

    > [!NOTE]
    > Weitere Informationen zu Prozessvorlagen für TFS finden Sie unter [Prozessvorlagen und Tools](https://msdn.microsoft.com/vstudio/aa718795).
12. Auf der **Teamwebsiteeinstellungen** Seite, übernehmen Sie die Standardeinstellungen unverändert, und klicken Sie dann auf **Weiter**.
13. Mit dieser Einstellung erstellt oder identifiziert wird, eine SharePoint-Teamwebsite, die mit dem TFS-Teamprojekt zugeordnet ist. Ihr Entwicklungsteam können diese Website verwalten Dokumentation, Diskussionsthemen teilnehmen, Wiki-Seiten erstellen und verschiedene andere Aufgaben, die nicht mit Code verknüpft sind. Weitere Informationen finden Sie unter [Interaktionen zwischen SharePoint-Produkte und Team Foundation Server](https://msdn.microsoft.com/library/ms253177.aspx).
14. Auf der **Quellcode-Verwaltungseinstellungen geben** Seite, übernehmen Sie die Standardeinstellungen unverändert, und klicken Sie dann auf **Weiter**.
15. Diese Einstellung identifiziert, oder die Position in der TFS-Ordnerhierarchie, die als einen Stammordner für Ihre Inhalte fungieren wird erstellt.
16. Auf der **Teamprojekteinstellungen bestätigen** auf **Fertig stellen**.
17. Wenn das neue Teamprojekt wird erfolgreich erstellt, auf die **Teamprojekt wurde erstellt** auf **schließen**.

### <a name="add-users-to-a-team-project"></a>Hinzufügen von Benutzern zu einem Teamprojekt

Nun, dass Sie das neue Teamprojekt erstellt haben, können Sie Berechtigungen für Benutzer, damit sie beginnen, hinzufügen und die Zusammenarbeit an Inhalten können gewähren.

**Hinzufügen von Benutzern zu einem Teamprojekt**

1. In Visual Studio 2010 in der **Team Explorer** Fenster mit der rechten Maustaste in des Teamprojekts, zeigen Sie auf **Teamprojekteinstellungen**, und klicken Sie dann auf **Gruppenmitgliedschaft**.

    ![](creating-a-team-project-in-tfs/_static/image15.png)
2. Um einen Benutzer hinzufügen, ändern und Entfernen von Code in der quellcodeverwaltung zu aktivieren, müssen Sie ihn hinzufügen der **Mitwirkende** Gruppe.
3. In der **Projektgruppen** wählen Sie im Dialogfeld die **Mitwirkende** gruppieren, und klicken Sie dann auf **Eigenschaften**.

    ![](creating-a-team-project-in-tfs/_static/image16.png)
4. In der **Team Foundation Server-Gruppeneigenschaften** wählen Sie im Dialogfeld **Windows-Benutzer oder Gruppe**, und klicken Sie dann auf **hinzufügen**.

    ![](creating-a-team-project-in-tfs/_static/image17.png)
5. In der **Auswahl von Benutzern, Computern oder Gruppen** Dialogfeld geben der Benutzernamen des Benutzers mit dem Teamprojekt hinzufügen möchten. Klicken Sie auf **Namen überprüfen**, und klicken Sie dann auf **OK**.

    ![](creating-a-team-project-in-tfs/_static/image18.png)
6. In der **Team Foundation Server-Gruppeneigenschaften** Dialogfeld klicken Sie auf **OK**.
7. In der **Projektgruppen** Dialogfeld klicken Sie auf **schließen**.

## <a name="conclusion"></a>Schlussbemerkung

An diesem Punkt das neue Teamprojekt ist einsatzbereit, und Ihr Entwicklerteam kann beginnen, Hinzufügen von Inhalt und die Zusammenarbeit bei der Entwicklung.

Im nächsten Thema, [Inhalten zur Quellcodeverwaltung hinzufügen](adding-content-to-source-control.md), beschreibt, wie Sie Inhalte zur quellcodeverwaltung hinzufügen.

## <a name="further-reading"></a>Weiterführende Themen

Umfassendere Anleitung zum Erstellen von Teamprojekten in TFS finden Sie [Erstellen eines Teamprojekts](https://msdn.microsoft.com/library/ms181477(v=VS.100).aspx). Weitere Informationen zum Aktivieren von Benutzern die Erstellung neuer Teamprojekte innerhalb einer Teamprojektsammlung finden Sie unter [Festlegen von Administratorberechtigungen für Teamprojektsammlungen](https://msdn.microsoft.com/library/dd547204.aspx). Weitere Informationen zum Hinzufügen von Benutzern zu Teamprojekten finden Sie unter [Hinzufügen von Benutzern zu Teamprojekten](https://msdn.microsoft.com/library/bb558971.aspx).

> [!div class="step-by-step"]
> [Zurück](configuring-team-foundation-server-for-web-deployment.md)
> [Weiter](adding-content-to-source-control.md)
