---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
title: Erstellen ein Teamprojekt in TFS | Microsoft Docs
author: jrjlee
description: Dieses Thema beschreibt die Erstellung ein neuen Teamprojekts in Team Foundation Server (TFS) 2010.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: b28d3e2d-0bb4-4e29-a780-af810b964722
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
msc.type: authoredcontent
ms.openlocfilehash: 4cb0d72330086ecb8cd9e6fb70ce0a57698dda5a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-team-project-in-tfs"></a>Erstellen ein Teamprojekt in TFS
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Dieses Thema beschreibt die Erstellung ein neuen Teamprojekts in Team Foundation Server (TFS) 2010.


Dieses Thema ist Teil einer Reihe von Lernprogrammen, die auf der Basis der Enterprise-bereitstellungsanforderungen eines fiktiven Unternehmens mit dem Namen Fabrikam, Inc. Diese Reihe von Lernprogrammen verwendet eine Beispielprojektmappe & #x 2014; die [Kontakt-Manager-Lösung](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; zum Darstellen einer Webanwendung mit einer realistischen Maß an Komplexität, einschließlich einer ASP.NET MVC 3-Anwendung, eine Windows Communication Foundation (WCF)-Dienst, und ein Datenbankprojekt.

## <a name="task-overview"></a>Übersicht über den Task

Zum Bereitstellen und ein neues Teamprojekt in TFS verwenden, müssen Sie die folgenden allgemeinen Schritte ausführen:

- Erteilen Sie Berechtigungen für den Benutzer, der das neue Teamprojekt erstellt wird.
- Erstellen Sie das Teamprojekt.
- Die Teammitglieder, die für das Projekt arbeiten, erteilen Sie Berechtigungen.
- Checken Sie Teile des Inhalts.

In diesem Thema erfahren Sie, wie Sie diese Verfahren ausführen, und es identifiziert die Benutzer und Rollen Auftrag, die wahrscheinlich für jede Prozedur zuständig sind. Denken Sie daran, dass abhängig von der Struktur Ihrer Organisation, jede dieser Aufgaben einer anderen Person verantwortlich sein kann.

Aufgaben und exemplarische Vorgehensweisen in diesem Thema wird davon ausgegangen, dass Sie installiert und TFS konfiguriert haben und Sie eine Teamprojektsammlung im Rahmen des Konfigurationsvorgangs erstellt haben. Weitere Informationen zu diesen Annahmen und weitere allgemeine Hintergrundinformationen für dieses Szenario finden Sie unter [eine TFS-Build-Server für die Bereitstellung konfigurieren](configuring-a-tfs-build-server-for-web-deployment.md).

## <a name="grant-permissions-to-the-team-project-creator"></a>Gewähren von Berechtigungen an den Teamprojektersteller

Um ein neues Teamprojekt erstellen, werden diese Berechtigungen erforderlich:

- Sie benötigen die **neue Projekte erstellen** -Berechtigung für die TFS-Anwendungsebene. Sie erteilen diese Berechtigung in der Regel durch Hinzufügen von Benutzern, die **Projektauflistungsadministratoren** TFS-Gruppe. Die **Team Foundation-Administratoren** globale Gruppe enthält auch diese Berechtigung.
- Sie benötigen die Berechtigung zum Erstellen neuer Team-Standorte innerhalb der SharePoint-Websitesammlung, die die TFS-Teamprojektsammlung entspricht. Sie erteilen diese Berechtigung in der Regel durch Hinzufügen des Benutzers zu einer SharePoint-Gruppe mit **Vollzugriff** -Rechte für die SharePoint-websiteauflistung.
- Wenn Sie SQL Server Reporting Services-Funktionen verwenden, muss Sie Mitglied der **Team Foundation-Inhalts-Manager** Rolle in Reporting Services.

### <a name="who-performs-these-procedures"></a>Wem diese Verfahren ausgeführt werden?

In der Regel führt die Person oder Gruppe, die die TFS-Bereitstellung verwaltet auch diese Verfahren aus.

Da dies einen mit weit reichenden Berechtigungen Satz von Berechtigungen ist, werden neue Teamprojekte mit Verantwortung für die Verwaltung einer TFS-Bereitstellung in der Regel durch eine kleine Teilmenge von Benutzern erstellt. Entwicklern werden nicht in der Regel die Berechtigungen erforderlich, um die Erstellung neuer Teamprojekte gewährt werden.

### <a name="grant-permissions-in-tfs"></a>Erteilen von Berechtigungen in TFS

Wenn Sie die Erstellung neuer Teamprojekte Benutzern ermöglichen möchten, ist die erste Aufgabe auf hoher Ebene beim Hinzufügen des Benutzers, der **Projektauflistungsadministratoren** Gruppe für die Teamprojektsammlung.

**Hinzufügen ein Benutzers zu der Gruppe "Projektauflistungsadministratoren" Projekt "**

1. Auf dem TFS-Server auf die **starten** Sie im Menü **Programme**, klicken Sie auf **Microsoft Team Foundation Server 2010**, und klicken Sie dann auf **Team Foundation -Verwaltungskonsole**.
2. Erweitern Sie in der Strukturansicht des Navigationsbereichs **Logikschicht**, und klicken Sie dann auf **Teamprojektsammlungen**.

    ![](creating-a-team-project-in-tfs/_static/image1.png)
3. In der **Teamprojektsammlungen** klicken Sie im Bereich der Teamprojektsammlung, die Sie verwalten möchten.

    ![](creating-a-team-project-in-tfs/_static/image2.png)
4. Auf der **allgemeine** auf **Gruppenmitgliedschaft**.

    ![](creating-a-team-project-in-tfs/_static/image3.png)
5. In der **globale Gruppen** wählen Sie im Dialogfeld die **Projektauflistungsadministratoren** Gruppe, und klicken Sie dann auf **Eigenschaften**.
6. In der **Eigenschaften für die Team Foundation Server-Gruppe** wählen Sie im Dialogfeld **Windows-Benutzer oder Gruppe**, und klicken Sie dann auf **hinzufügen**.

    ![](creating-a-team-project-in-tfs/_static/image4.png)
7. In der **Auswahl von Benutzern, Computern oder Gruppen** Dialogfeld geben der Benutzernamen des Benutzers aus, um neue Teamprojekte erstellen zu können. Klicken Sie auf **Namen überprüfen**, und klicken Sie dann auf **OK** .

    ![](creating-a-team-project-in-tfs/_static/image5.png)
8. In der **Eigenschaften für die Team Foundation Server-Gruppe** (Dialogfeld), klicken Sie auf **OK**.
9. In der **globale Gruppen** (Dialogfeld), klicken Sie auf **schließen**.

### <a name="grant-permissions-in-sharepoint-services"></a>Erteilen von Berechtigungen in SharePointServices

Als Nächstes müssen Sie neue Teamwebsites in der SharePoint-Websitesammlung zu erstellen, die TFS-Teamprojektsammlung entsprechen, die Benutzerberechtigung geben.

**Erteilen von Vollzugriffsberechtigungen für die SharePoint-Websitesammlung**

1. In der Team Foundation Server-Verwaltungskonsole auf der **Teamprojektsammlungen** Seite, wählen Sie die Teamprojektsammlung, die Sie verwalten möchten.
2. Auf der **SharePoint-Website** Registerkarte, notieren Sie den Wert von der **aktuelle Standard Standort** URL.

    ![](creating-a-team-project-in-tfs/_static/image6.png)
3. Öffnen Sie Internet Explorer, und fahren Sie mit der URL, die Sie in Schritt 2 notiert haben.

    > [!NOTE]
    > Wenn Sie nicht Windows als Benutzer, die die Teamprojektsammlung erstellt hat angemeldet sind, müssen Sie in SharePoint als dieser Benutzer anmelden, um den Vorgang fortzusetzen.
4. Auf der **Websiteaktionen** Menü klicken Sie auf **Standorteinstellungen**.

    ![](creating-a-team-project-in-tfs/_static/image7.png)
5. Auf der **Standorteinstellungen** Seite **Benutzer und Berechtigungen**, klicken Sie auf **Personen und Gruppen**.
6. Klicken Sie in den linken Navigationsbereich auf **Gruppen**.

    ![](creating-a-team-project-in-tfs/_static/image8.png)
7. Auf der **Personen und Gruppen: alle Gruppen** auf **Gruppen einrichten für diesen Standort**.

    ![](creating-a-team-project-in-tfs/_static/image9.png)

    > [!NOTE]
    > Möglicherweise erhalten Sie eine **HTTP 404 Not Found** Fehler aufgrund eines doppelten Codierung HTTP-Fehlers. In diesem Fall ersetzen Sie dabei die URL:   
    > [*URL Websitesammlung*] /\_layouts/permsetup.aspx  
    > Zum Beispiel:  
    > http://TFS/Sites/Fabrikam%20Web%20Projects/\_layouts/permsetup.aspx
8. Auf der **Gruppen einrichten für diesen Standort** Seite, fügen Sie den Benutzer, die Teamprojekte erstellen, wird die **Besitzer** Gruppe, und klicken Sie dann auf **OK**.

    ![](creating-a-team-project-in-tfs/_static/image10.png)

Weitere Informationen zum Aktivieren von Benutzern die Erstellung neuer Teamprojekte innerhalb einer Teamprojektsammlung finden Sie unter [Festlegen von Administratorberechtigungen für Teamprojektsammlungen](https://msdn.microsoft.com/en-us/library/dd547204.aspx).

## <a name="create-a-new-team-project-and-add-users"></a>Ein neues Teamprojekt erstellen und Hinzufügen von Benutzern

Nachdem Sie die erforderlichen Berechtigungen haben, können Sie die **Team Explorer** Fenster in Visual Studio 2010 zum Erstellen eines neuen Teamprojekts. Dieser Ansatz bietet einen Assistenten, der die erforderliche Informationen erfasst und führt die erforderlichen Aufgaben in TFS, SharePoint und SQL Server Reporting Services. Sie müssen auch das Erteilen von Berechtigungen für das neue Teamprojekt Mitgliedern der Developer-Team, zu ermöglichen, hinzufügen und Ändern von Inhalt.

### <a name="who-performs-these-procedures"></a>Wem diese Verfahren ausgeführt werden?

In der Regel führt eine TFS-Administrator oder ein Entwickler Teamleiter diese Verfahren.

### <a name="create-a-new-team-project"></a>Erstellen eines neuen Teamprojekts

Das nächste Verfahren beschreibt, wie ein neues Teamprojekt in TFS 2010 zu erstellen.

**Zum Erstellen eines neuen Teamprojekts**

1. Auf der **starten** Sie im Menü **Programme**, klicken Sie auf **Microsoft Visual Studio 2010**, mit der rechten Maustaste **Microsoft Visual Studio 2010**, und klicken Sie dann auf **als Administrator ausführen**.

    > [!NOTE]
    > Wenn Sie nicht Visual Studio 2010 als Administrator ausführen, schlägt der Assistent für neue Teamprojekte im letzten Schritt fehl.
2. Wenn die **User Account Control** Dialogfeld angezeigt wird, klicken Sie auf **Ja**.
3. In Visual Studio auf die **Team** Menü klicken Sie auf **mit Team Foundation Server verbinden**.

    > [!NOTE]
    > Wenn Sie bereits eine Verbindung mit einem TFS-Server konfiguriert haben, können Sie die Schritte 4 bis 7 weglassen.
4. In der **Verbindung mit Teamprojekt** (Dialogfeld), klicken Sie auf **Server**.
5. In der **Team Foundation Server hinzufügen/entfernen** (Dialogfeld), klicken Sie auf **hinzufügen**.
6. In der **Team Foundation Server hinzufügen** (Dialogfeld), geben Sie die Details Ihrer TFS-Instanz, und klicken Sie dann auf **OK**.

    ![](creating-a-team-project-in-tfs/_static/image11.png)
7. In der **Team Foundation Server hinzufügen/entfernen** (Dialogfeld), klicken Sie auf **schließen**.
8. In der **Verbindung mit Teamprojekt herstellen** wählen Sie im Dialogfeld die TFS-Instanz, die Sie verbinden möchten, markieren Sie das Team möchten Sie verwenden möchten, hinzu, und klicken Sie dann auf projektauflistung **verbinden**.

    ![](creating-a-team-project-in-tfs/_static/image12.png)
9. In der **Team Explorer** der rechten Maustaste auf die Teamprojektsammlung Sammlung, und klicken Sie dann auf **neues Teamprojekt**.

    ![](creating-a-team-project-in-tfs/_static/image13.png)
10. In der **neues Teamprojekt** (Dialogfeld), geben Sie einen Namen und eine Beschreibung für das Teamprojekt, und klicken Sie dann auf **Weiter**.

    > [!NOTE]
    > Wenn das Teamprojekt Leerzeichen enthält, können Sie einige Probleme stoßen, wenn Sie gelangen zu der Internetinformationsdienste (Internet Information Services, IIS)-Webbereitstellungstool (Web Deploy) zum Bereitstellen von Paketen aus den Ausgabepfad verwenden. Leerzeichen im Pfad können viel schwieriger zu Web Deploy-Befehle ausführen erleichtern.

    ![](creating-a-team-project-in-tfs/_static/image14.png)
11. Auf der **Auswählen einer Prozessvorlage** Seite, wählen Sie die Prozessvorlage, die Sie verwenden, um den Entwicklungsprozess zu verwalten, und klicken Sie dann auf möchten **Weiter**.

    > [!NOTE]
    > Weitere Informationen zu Prozessvorlagen für TFS, finden Sie unter [Prozessvorlagen und Tools](https://msdn.microsoft.com/en-us/vstudio/aa718795).
12. Auf der **Team Standorteinstellungen** Seite, übernehmen Sie die Standardeinstellungen unverändert, und klicken Sie dann auf **Weiter**.
13. Mit dieser Einstellung erstellt oder identifiziert wird, eine SharePoint-Teamwebsite, die die TFS-Teamprojekt zugeordnet ist. Ihr Entwicklungsteam können diese Website verwalten-Dokumentation, Diskussionsthemen teilnehmen, Wiki-Seiten erstellen und verschiedene andere Aufgaben ausführen, die nicht mit Code verknüpft sind. Weitere Informationen finden Sie unter [Interaktionen zwischen SharePoint-Produkte und Team Foundation Server](https://msdn.microsoft.com/en-us/library/ms253177.aspx).
14. Auf der **Quellcodeverwaltungseinstellungen geben** Seite, übernehmen Sie die Standardeinstellungen unverändert, und klicken Sie dann auf **Weiter**.
15. Diese Einstellung identifiziert, oder erstellt den Speicherort in der Hierarchie der TFS-Ordner, die als einen Stammordner für Ihre Inhalte fungiert.
16. Auf der **Teamprojekteinstellungen bestätigen** auf **Fertig stellen**.
17. Wenn das neue Teamprojekt wurde erfolgreich erstellt, auf die **Teamprojekt wurde erstellt** auf **schließen**.

### <a name="add-users-to-a-team-project"></a>Hinzufügen von Benutzern zu einem Teamprojekt

Nun, dass Sie das neue Teamprojekt erstellt haben, können Sie für Benutzer, damit sie beginnen, hinzufügen und das gemeinsame Bearbeiten von Inhalt können Berechtigungen gewähren.

**Zum Hinzufügen von Benutzern zu einem Teamprojekt**

1. In Visual Studio 2010 in der **Team Explorer** Fenster mit der rechten Maustaste in des Teamprojekts, zeigen Sie auf **Teamprojekteinstellungen**, und klicken Sie dann auf **Gruppenmitgliedschaft**.

    ![](creating-a-team-project-in-tfs/_static/image15.png)
2. Um einen Benutzer hinzufügen, ändern und Entfernen von Code in die quellcodeverwaltung zu aktivieren, müssen Sie ihn hinzufügen der **Contributors** Gruppe.
3. In der **Teamprojektgruppen** wählen Sie im Dialogfeld die **Contributors** Gruppe, und klicken Sie dann auf **Eigenschaften**.

    ![](creating-a-team-project-in-tfs/_static/image16.png)
4. In der **Eigenschaften für die Team Foundation Server-Gruppe** wählen Sie im Dialogfeld **Windows-Benutzer oder Gruppe**, und klicken Sie dann auf **hinzufügen**.

    ![](creating-a-team-project-in-tfs/_static/image17.png)
5. In der **Auswahl von Benutzern, Computern oder Gruppen** Dialogfeld geben der Benutzernamen des Benutzers mit dem Teamprojekt hinzufügen möchten. Klicken Sie auf **Namen überprüfen**, und klicken Sie dann auf **OK**.

    ![](creating-a-team-project-in-tfs/_static/image18.png)
6. In der **Eigenschaften für die Team Foundation Server-Gruppe** (Dialogfeld), klicken Sie auf **OK**.
7. In der **Teamprojektgruppen** (Dialogfeld), klicken Sie auf **schließen**.

## <a name="conclusion"></a>Schlussfolgerung

Zu diesem Zeitpunkt das neue Teamprojekt einsatzbereit ist, und Ihr Team Developer kann beginnen, Hinzufügen von Inhalt und Zusammenarbeit bei der Entwicklung.

Im nächsten Thema [Hinzufügen von Inhalt zur Quellcodeverwaltung](adding-content-to-source-control.md), beschreibt, wie Inhalt zur quellcodeverwaltung hinzufügen.

## <a name="further-reading"></a>Weiterführende Themen

Umfassendere Anleitung zum Erstellen von Teamprojekten in TFS finden Sie unter [Erstellen eines Teamprojekts](https://msdn.microsoft.com/en-us/library/ms181477(v=VS.100).aspx). Weitere Informationen zum Aktivieren von Benutzern die Erstellung neuer Teamprojekte innerhalb einer Teamprojektsammlung finden Sie unter [Festlegen von Administratorberechtigungen für Teamprojektsammlungen](https://msdn.microsoft.com/en-us/library/dd547204.aspx). Weitere Informationen zum Hinzufügen von Benutzern zu Teamprojekten finden Sie unter [Hinzufügen von Benutzern zu Teamprojekten](https://msdn.microsoft.com/en-us/library/bb558971.aspx).

>[!div class="step-by-step"]
[Zurück](configuring-team-foundation-server-for-web-deployment.md)
[Weiter](adding-content-to-source-control.md)
