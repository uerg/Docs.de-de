---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
title: Hinzufügen von Inhalten zur Quellcodeverwaltung | Microsoft-Dokumentation
author: jrjlee
description: In diesem Thema wird erläutert, wie der quellcodeverwaltung in Team Foundation Server (TFS) 2010 Inhalt hinzugefügt wird. Es wird beschrieben, Hinzufügen von Projektmappen und Projekte zu einem Team-Projekt hinzufügen...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 86c14aab-c2dd-4f73-b40c-c6d52fa44950
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
msc.type: authoredcontent
ms.openlocfilehash: 9fdb2e37f2925273b457157b634865d93e865098
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37838370"
---
<a name="adding-content-to-source-control"></a>Hinzufügen von Inhalten zur Quellcodeverwaltung
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In diesem Thema wird erläutert, wie der quellcodeverwaltung in Team Foundation Server (TFS) 2010 Inhalt hinzugefügt wird. Es beschreibt das Hinzufügen von Projektmappen und Projekte zu einem Teamprojekt in TFS, und es wird erläutert, wie externe Abhängigkeiten wie Frameworks oder Assemblys zur quellcodeverwaltung hinzufügen.


In diesem Thema ist Teil einer Reihe von Tutorials, die auf der Basis der bereitstellungsanforderungen Enterprise ein fiktives Unternehmen, die mit dem Namen Fabrikam, Inc. Dieser tutorialreihe verwendet eine beispiellösung&#x2014;der [Contact Manager-Lösung](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;zur Darstellung einer Webanwendung mit einem realistischen Maß an Komplexität, einschließlich einer ASP.NET MVC 3-Anwendung, eine Windows-Kommunikation Foundation (WCF)-Dienst und ein Datenbankprojekt.

## <a name="task-overview"></a>Übersicht über den Task

In den meisten Fällen sollte jedes Mitglied des Entwicklerteams Inhalten zur quellcodeverwaltung hinzufügen können. Um eine Projektmappe zur quellcodeverwaltung in TFS hinzufügen möchten, müssen Sie die folgenden allgemeinen Schritte ausführen:

- Verbinden Sie mit einem Teamprojekt.
- Ordnen Sie die Team Project-Ordnerstruktur auf dem Server, auf eine Ordnerstruktur auf dem lokalen Computer.
- Die Lösung und seinen Inhalt zur quellcodeverwaltung hinzufügen.
- Externen Abhängigkeiten zur quellcodeverwaltung hinzufügen.

In diesem Thema zeigt Sie, wie Sie diese Schritte ausführen.

Die Aufgaben und exemplarische Vorgehensweisen in diesem Thema wird davon ausgegangen, dass Sie ein neues Teamprojekt von TFS, um die Inhalte verwalten, bereits erstellt haben. Weitere Informationen zum Erstellen eines neuen Teamprojekts finden Sie unter [Erstellen eines Teamprojekts in TFS](creating-a-team-project-in-tfs.md).

### <a name="who-performs-these-procedures"></a>Wem diese Verfahren werden?

In den meisten Fällen sollte jedes Mitglied des Entwicklerteams möglicherweise hinzufügen und ändern die Inhalte in bestimmten Teamprojekten.

## <a name="connect-to-a-team-project-and-create-a-folder-mapping"></a>Verbinden mit einem Teamprojekt und erstellen Sie eine Ordnerzuordnung

Bevor Sie alle Inhalte zur quellcodeverwaltung hinzufügen, müssen Sie eine Verbindung mit einem Teamprojekt, und erstellen Sie eine Zuordnung zwischen der Ordnerstruktur auf dem Server und dem Dateisystem auf dem lokalen Computer.

**Eine Verbindung mit einem Teamprojekt, und ordnen einen lokalen Pfad**

1. Öffnen Sie Visual Studio 2010, auf der Entwicklerarbeitsstation.
2. In Visual Studio auf die **Team** Menü klicken Sie auf **Herstellen einer Verbindung mit Team Foundation Server**.

    > [!NOTE]
    > Wenn Sie bereits eine Verbindung mit einem TFS-Server konfiguriert haben, können Sie die Schritte 3 bis 6 auslassen.
3. In der **Verbindung mit Teamprojekt** Dialogfeld klicken Sie auf **Server**.
4. In der **Team Foundation Server hinzufügen/entfernen** Dialogfeld klicken Sie auf **hinzufügen**.
5. In der **Team Foundation Server hinzufügen** Dialogfeld Geben Sie die Details Ihrer TFS-Instanz, und klicken Sie dann auf **OK**.

    ![](adding-content-to-source-control/_static/image1.png)
6. In der **Team Foundation Server hinzufügen/entfernen** Dialogfeld klicken Sie auf **schließen**.
7. In der **Herstellen einer Verbindung mit Teamprojekt** Dialogfeld Wählen Sie die TFS-Instanz, die Sie eine Verbindung herstellen möchten, markieren Sie das Team möchten project, Sammlung, wählen Sie das Teamprojekt, das Sie hinzufügen möchten, und klicken Sie dann auf **Connect**.

    ![](adding-content-to-source-control/_static/image2.png)
8. In der **Team Explorer** , erweitern Sie Ihr Teamprojekt, und doppelklicken Sie dann auf **Quellcodeverwaltung**.

    ![](adding-content-to-source-control/_static/image3.png)
9. Auf der **Quellcodeverwaltungs-Explorer** auf **nicht zugeordnet**.

    ![](adding-content-to-source-control/_static/image4.png)
10. In der **Zuordnung** Dialogfeld die **lokalen Ordner** Feld, navigieren Sie zu (oder erstellen) fungieren als Stammordner für das Teamprojekt, und klicken Sie auf einen lokalen Ordner **Zuordnung**.

    ![](adding-content-to-source-control/_static/image5.png)
11. Wenn Sie zum Herunterladen der Quelldateien aufgefordert werden, klicken Sie auf **Ja**.

    ![](adding-content-to-source-control/_static/image6.png)

An diesem Punkt haben Sie die serverseitigen Ordner für das Teamprojekt in einen lokalen Ordner auf der Entwicklerarbeitsstation zugeordnet. Sie haben außerdem jeglichen vorhandenen Inhalt aus dem Teamprojekt auf Ihrem lokalen Ordnerstruktur heruntergeladen. Sie können jetzt starten, um Ihre eigenen Inhalte zur quellcodeverwaltung hinzufügen.

## <a name="add-projects-and-solutions-to-source-control"></a>Projekte und Projektmappen zur Quellcodeverwaltung hinzufügen

Um Projekte und Projektmappen zur quellcodeverwaltung hinzufügen möchten, müssen Sie zunächst in den zugeordneten Ordner für das Teamprojekt auf dem lokalen Computer verschieben. Sie können Inhalte in die Erweiterungen mit dem Server synchronisiert. Klicken Sie dann überprüfen.

**Projekte zur quellcodeverwaltung hinzufügen**

1. Verschieben Sie auf der Entwicklerarbeitsstation die Projekte und Projektmappen auf einen geeigneten Speicherort innerhalb der Struktur zugeordneten Ordner für das Teamprojekt aus.

    > [!NOTE]
    > Viele Organisationen haben einen bevorzugten Ansatz wie Projekte und Projektmappen in der quellcodeverwaltung angeordnet werden soll. Anleitungen zum Ordner der Struktur finden Sie unter [so wird's gemacht: Struktur der Quellcode-Verwaltungsordnern in Team Foundation Server](https://msdn.microsoft.com/library/bb668992.aspx).
2. Öffnen Sie die Projektmappe in Visual Studio 2010 ein.
3. In der **Projektmappen-Explorer** rechten Maustaste auf die Projektmappe, und klicken Sie dann auf **Projektmappe zur Quellcodeverwaltung hinzufügen**.

    ![](adding-content-to-source-control/_static/image7.png)

    > [!NOTE]
    > In einigen Fällen, je nachdem, wie Ihre Organisation zum Strukturieren von Inhalt in TFS, gerne ganz von vorne müssen Sie zum Hinzufügen von Projekten zur quellcodeverwaltung durchzugehen, um eine präzisere Kontrolle über den Aufbau von Quellcode bereitzustellen.
4. Überprüfen Sie, ob die **Quellcodeverwaltungs-Explorer** Registerkarte zeigt den Inhalt, die Sie in der Ordnerstruktur für das Teamprojekt hinzugefügt haben.

    ![](adding-content-to-source-control/_static/image8.png)

    > [!NOTE]
    > Die **Quellcodeverwaltungs-Explorer** Registerkarte zeigt Ihre Inhalte mit keine weiteren eingabeaufforderungen, da Sie einen zugeordneten Ordner auf dem lokalen Dateisystem Ihrer Projektmappe hinzugefügt. Wenn Ihre Lösung an einem nicht zugeordneten Speicherort war, würden Sie an Speicherorte von Ordnern in TFS und Ihrem lokalen Dateisystem aufgefordert.
5. Auf der **Quellcodeverwaltungs-Explorer** Registerkarte die **Ordner** Bereich mit der rechten Maustaste in des Teamprojekts (z. B. **ContactManager**), und klicken Sie dann auf **Einchecken Ausstehende Änderungen**.
6. In der **Einchecken – Quelldateien** Dialogfeld Geben Sie einen Kommentar, und klicken Sie dann auf **Einchecken**.

    ![](adding-content-to-source-control/_static/image9.png)

An diesem Punkt haben Sie die Projektmappe zur quellcodeverwaltung in TFS hinzugefügt.

## <a name="add-external-dependencies-to-source-control"></a>Externe Abhängigkeiten zur Quellcodeverwaltung hinzufügen

Wenn Sie ein Projekt oder eine Projektmappe zur quellcodeverwaltung hinzufügen, werden alle Dateien und Ordner in Ihrem Projekt oder Projektmappe auch hinzugefügt werden. Allerdings basieren in vielen Fällen, Projekte und Projektmappen auch auf externe Abhängigkeiten, ebenso wie bei lokalen Assemblys ordnungsgemäß funktioniert. In diesem Fall müssen Sie eine hinzufügen, dass solche Ressourcen zur quellcodeverwaltung, Team Build und anderen Mitgliedern des Entwicklerteams können Ihren Code erfolgreich zu erstellen.

Die Ordnerstruktur für die beispiellösung Contact Manager enthält beispielsweise einen Ordner namens Pakete. Die Assembly und verschiedene unterstützende Ressourcen enthält für das ADO.NET Entity Framework 4.1. Der Ordner "Pakete" ist nicht Teil der Contact Manager-Lösung, aber die Projektmappe ohne nicht erfolgreich erstellt. Um Team Build zum Erstellen der Projektmappe zu aktivieren, müssen Sie den Ordner "Pakete" zur quellcodeverwaltung hinzufügen.

> [!NOTE]
> Die Einbeziehung von einem Ordner "Pakete" ist typisch für was geschieht, wenn Sie der Projektmappe mit der NuGet-Erweiterung für Visual Studio 2010 die Entity Framework oder ähnliche Ressourcen hinzufügen.


**Inhalt der nicht Teil des Projekts zur quellcodeverwaltung hinzufügen**

1. Stellen Sie sicher, dass die Elemente, die Sie hinzufügen möchten (z. B. den Ordner "Pakete") in einen geeigneten Speicherort in einen zugeordneten Ordner auf Ihrem lokalen Dateisystem befinden.
2. In Visual Studio 2010 In der **Team Explorer** , erweitern Sie Ihr Teamprojekt, und doppelklicken Sie dann auf **Quellcodeverwaltung**.

    ![](adding-content-to-source-control/_static/image10.png)
3. Auf der **Quellcodeverwaltungs-Explorer** Registerkarte die **Ordner** wählen Sie im Bereich der Ordner, der das Element enthält, oder Sie Elemente hinzufügen möchten.
4. Klicken Sie auf die **Elemente zu Ordner hinzufügen** Schaltfläche.

    ![](adding-content-to-source-control/_static/image11.png)
5. In der **zur Quellcodeverwaltung hinzufügen** Dialogfeld wählen den Ordner oder die Elemente, die Sie hinzufügen möchten, und klicken Sie dann auf **Weiter**.

    ![](adding-content-to-source-control/_static/image12.png)
6. Auf der **ausgeschlossene Elemente** Registerkarte, wählen Sie alle erforderlichen Elemente, die wurden (z. B. Assemblys) automatisch ausgeschlossen, und klicken Sie dann auf **Element(e) einschließen**.

    ![](adding-content-to-source-control/_static/image13.png)
7. Auf der **hinzuzufügenden Elemente** Registerkarte, stellen Sie sicher, dass alle Dateien, die Sie einschließen möchten, finden Sie, und klicken Sie dann auf **Fertig stellen**.

    ![](adding-content-to-source-control/_static/image14.png)
8. In der **Quellcodeverwaltungs-Explorer** Fenster, klicken Sie auf die **Einchecken** Schaltfläche.

    ![](adding-content-to-source-control/_static/image15.png)
9. In der **Einchecken – Quelldateien** Dialogfeld Geben Sie einen Kommentar, und klicken Sie dann auf **Einchecken**.

An diesem Punkt haben Sie die externen Abhängigkeiten für Ihre Projektmappe zur quellcodeverwaltung hinzugefügt.

## <a name="conclusion"></a>Schlussbemerkung

In diesem Thema beschrieben, wie Sie eine Verbindung mit einem Teamprojekt herstellen, ordnen eine Ordnerstruktur und Inhalten zur quellcodeverwaltung hinzufügen. Weitere Informationen zum Arbeiten mit Elementen unter quellcodeverwaltung finden Sie unter [verwenden der Versionskontrolle](https://msdn.microsoft.com/library/ms181368.aspx).

Im nächsten Thema, [konfigurieren eine TFS-Build-Server für die Webbereitstellung](configuring-a-tfs-build-server-for-web-deployment.md), beschreibt, wie Sie einen TFS Team Build-Server zum Erstellen und Bereitstellen Ihrer Lösung vorbereiten.

## <a name="further-reading"></a>Weiterführende Themen

Ausführlichere Informationen zum Arbeiten mit Datenquellen-Steuerelement in TFS finden Sie unter [verwenden der Versionskontrolle](https://msdn.microsoft.com/library/ms181368.aspx).

> [!div class="step-by-step"]
> [Zurück](creating-a-team-project-in-tfs.md)
> [Weiter](configuring-a-tfs-build-server-for-web-deployment.md)
