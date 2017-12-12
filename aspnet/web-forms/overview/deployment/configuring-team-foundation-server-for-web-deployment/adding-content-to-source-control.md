---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
title: "Hinzufügen von Inhalt zur Quellcodeverwaltung | Microsoft Docs"
author: jrjlee
description: "In diesem Thema wird erläutert, wie Datenquellen-Steuerelements in Team Foundation Server (TFS) 2010 Inhalt hinzugefügt wird. Es wird beschrieben, wie ein Team cht aus Projektmappen und Projekte hinzufügen..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 86c14aab-c2dd-4f73-b40c-c6d52fa44950
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
msc.type: authoredcontent
ms.openlocfilehash: a6a90a03674cfe7565da0ed56148186ee9525707
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="adding-content-to-source-control"></a>Hinzufügen von Inhalt zur Quellcodeverwaltung
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In diesem Thema wird erläutert, wie Datenquellen-Steuerelements in Team Foundation Server (TFS) 2010 Inhalt hinzugefügt wird. Es wird beschrieben, wie Hinzufügen von Projektmappen und Projekten zu einem Teamprojekt in TFS, und es wird erläutert, wie externe Abhängigkeiten wie Frameworks oder Assemblys zur quellcodeverwaltung hinzufügen.


Dieses Thema ist Teil einer Reihe von Lernprogrammen, die auf der Basis der Enterprise-bereitstellungsanforderungen eines fiktiven Unternehmens mit dem Namen Fabrikam, Inc. Diese Reihe von Lernprogrammen verwendet eine Beispielprojektmappe & #x 2014; die [Kontakt-Manager-Lösung](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; zum Darstellen einer Webanwendung mit einer realistischen Maß an Komplexität, einschließlich einer ASP.NET MVC 3-Anwendung, eine Windows Communication Foundation (WCF)-Dienst, und ein Datenbankprojekt.

## <a name="task-overview"></a>Übersicht über den Task

In den meisten Fällen sollte jedes Mitglied des Entwickler-Teams Inhalt zur quellcodeverwaltung hinzufügen können. Um eine Projektmappe zur quellcodeverwaltung in TFS hinzufügen möchten, müssen Sie die folgenden allgemeinen Schritte ausführen:

- Verbinden Sie mit einem Teamprojekt.
- Ordnen Sie die Ordnerstruktur des Team-Projekts auf dem Server eine Ordnerstruktur auf dem lokalen Computer an.
- Die Lösung und dessen Inhalt zur quellcodeverwaltung hinzufügen.
- Externe Abhängigkeiten zur quellcodeverwaltung hinzufügen.

In diesem Thema erfahren Sie, wie Sie diese Verfahren ausführen.

Aufgaben und exemplarische Vorgehensweisen in diesem Thema wird davon ausgegangen, dass Sie ein neues Teamprojekts von TFS, um Ihren Inhalt verwalten bereits erstellt haben. Weitere Informationen zum Erstellen eines neuen Teamprojekts finden Sie unter [erstellen ein Teamprojekt in TFS](creating-a-team-project-in-tfs.md).

### <a name="who-performs-these-procedures"></a>Wem diese Verfahren ausgeführt werden?

In den meisten Fällen sollte jedes Mitglied des Entwickler-Teams hinzuzufügen und zu Inhalten innerhalb einer bestimmten Teamprojekten ändern können.

## <a name="connect-to-a-team-project-and-create-a-folder-mapping"></a>Herstellen einer Verbindung mit einem Teamprojekt und erstellen Sie eine Zuordnung Ordner

Bevor Sie alle Inhalte, die zur quellcodeverwaltung hinzufügen, müssen Sie eine Verbindung mit einem Teamprojekt und erstellen Sie eine Zuordnung zwischen der Ordnerstruktur auf dem Server und dem Dateisystem auf dem lokalen Computer.

**Zum Herstellen einer Verbindung mit einem Teamprojekt und zum Zuordnen eines lokalen Pfads**

1. Öffnen Sie auf der Entwicklerarbeitsstation Visual Studio 2010 ein.
2. In Visual Studio auf die **Team** Menü klicken Sie auf **mit Team Foundation Server verbinden**.

    > [!NOTE]
    > Wenn Sie bereits eine Verbindung mit einem TFS-Server konfiguriert haben, können Sie die Schritte 3 bis 6 weglassen.
3. In der **Verbindung mit Teamprojekt** (Dialogfeld), klicken Sie auf **Server**.
4. In der **Team Foundation Server hinzufügen/entfernen** (Dialogfeld), klicken Sie auf **hinzufügen**.
5. In der **Team Foundation Server hinzufügen** (Dialogfeld), geben Sie die Details Ihrer TFS-Instanz, und klicken Sie dann auf **OK**.

    ![](adding-content-to-source-control/_static/image1.png)
6. In der **Team Foundation Server hinzufügen/entfernen** (Dialogfeld), klicken Sie auf **schließen**.
7. In der **Verbindung mit Teamprojekt herstellen** (Dialogfeld), wählen Sie die TFS-Instanz, die Sie verbinden möchten, markieren Sie das Team möchten Projekt Sammlung, wählen Sie das Teamprojekt, das Sie hinzufügen möchten, und klicken Sie dann auf **verbinden**.

    ![](adding-content-to-source-control/_static/image2.png)
8. In der **Team Explorer** , erweitern Sie das Teamprojekt, und doppelklicken Sie dann auf **Quellcodeverwaltung**.

    ![](adding-content-to-source-control/_static/image3.png)
9. Auf der **Quellcodeverwaltungs-Explorer** auf **nicht zugeordnet**.

    ![](adding-content-to-source-control/_static/image4.png)
10. In der **Zuordnung** Dialogfeld die **lokalen Ordner** Feld, navigieren Sie zu (oder erstellen) fungieren als Stammordner für das Teamprojekt, und klicken Sie auf einen lokalen Ordner **Zuordnung**.

    ![](adding-content-to-source-control/_static/image5.png)
11. Wenn Sie zum Herunterladen der Quelldateien aufgefordert werden, klicken Sie auf **Ja**.

    ![](adding-content-to-source-control/_static/image6.png)

An diesem Punkt haben Sie den serverseitigen-Ordner für das Teamprojekt in einen lokalen Ordner auf der Arbeitsstation Developer zugeordnet. Sie haben auch jeglichen vorhandenen Inhalt aus dem Teamprojekt den Projektplan in Ihre lokale Ordnerstruktur heruntergeladen. Sie können jetzt starten, um Ihre eigenen Inhalte zur quellcodeverwaltung hinzufügen.

## <a name="add-projects-and-solutions-to-source-control"></a>Projekte und Projektmappen zur Quellcodeverwaltung hinzufügen

Um Projekte und Projektmappen zur quellcodeverwaltung hinzufügen, müssen Sie sie in der zugeordneten Ordner für das Teamprojekt auf dem lokalen Computer zu verschieben. Sie können dann das Kontrollkästchen in den Inhalt, der die Erweiterungen mit dem Server zu synchronisieren.

**Projekte zur quellcodeverwaltung hinzufügen**

1. Verschieben Sie auf der Arbeitsstation Developer die Projekte und Projektmappen auf einen geeigneten Speicherort innerhalb der Struktur zugeordneten Ordner für das Teamprojekt aus.

    > [!NOTE]
    > Viele Organisationen haben einen bevorzugten Ansatz, wie Projekte und Projektmappen in der quellcodeverwaltung strukturiert sein sollte. Anleitungen zum Struktur Ordnern finden Sie unter [How To: Struktur des Steuerelements Quellordnern in Team Foundation Server](https://msdn.microsoft.com/en-us/library/bb668992.aspx).
2. Öffnen Sie die Projektmappe in Visual Studio 2010.
3. In der **Projektmappen-Explorer** rechten Maustaste auf die Projektmappe, und klicken Sie dann auf **Projektmappe zur Quellcodeverwaltung hinzufügen**.

    ![](adding-content-to-source-control/_static/image7.png)

    > [!NOTE]
    > In einigen Fällen, abhängig von Ihrer Organisation zum Strukturieren von Inhalt in TFS wie schätzt müssen Sie zum Hinzufügen von Projekten zur quellcodeverwaltung durchzugehen, um bieten eine präzisere Steuerung wie Quellcode organisiert ist.
4. Überprüfen Sie, ob die **Quellcodeverwaltungs-Explorer** Registerkarte zeigt den Inhalt, die Sie in der Ordnerstruktur für das Teamprojekt hinzugefügt haben.

    ![](adding-content-to-source-control/_static/image8.png)

    > [!NOTE]
    > Die **Quellcodeverwaltungs-Explorer** Registerkarte zeigt Ihre Inhalte mit keine weiteren aufgefordert werden, da Sie einen zugeordneten Ordner im lokalen Dateisystem die Projektmappe hinzugefügt. War die Projektmappe in einem nicht zugeordneten Speicherort, würden Sie aufgefordert, Speicherorte von Ordnern in TFS und Ihrem lokalen Dateisystem angeben.
5. Auf der **Quellcodeverwaltungs-Explorer** Registerkarte die **Ordner** Bereich mit der rechten Maustaste in des Teamprojekts (z. B. **ContactManager**), und klicken Sie dann auf **Einchecken Ausstehende Änderungen**.
6. In der **Einchecken – Quelldateien** (Dialogfeld), geben Sie einen Kommentar ein, und klicken Sie dann auf **Einchecken**.

    ![](adding-content-to-source-control/_static/image9.png)

An diesem Punkt haben Sie Datenquellen-Steuerelements in TFS die Projektmappe hinzugefügt.

## <a name="add-external-dependencies-to-source-control"></a>Externe Abhängigkeiten zur Quellcodeverwaltung hinzufügen

Wenn Sie ein Projekt oder eine Projektmappe zur quellcodeverwaltung hinzufügen, werden alle Dateien und Ordner in Ihrem Projekt oder eine Projektmappe auch hinzugefügt werden. Allerdings basieren in vielen Fällen, Projekte und Projektmappen auch auf externe Abhängigkeiten, ebenso wie bei lokalen Assemblys ordnungsgemäß funktioniert. Sie müssen das Hinzufügen eines solchen Ressourcen zur quellcodeverwaltung, Team Build und anderen Mitgliedern des Teams Entwickler können die Code erfolgreich erstellt.

Die Ordnerstruktur für die Projektmappe Contact Manager enthält beispielsweise einen Ordner namens Pakete. Er enthält die Assembly und verschiedene unterstützende Ressourcen für das ADO.NET Entity Framework 4.1. Der Ordner "Pakete" ist nicht Teil der Projektmappe Contact Manager allerdings Erstellen der Projektmappe nicht erfolgreich ohne sie. Damit kann Team Build, um die Projektmappe zu erstellen, müssen Sie den Ordner "Pakete" zur quellcodeverwaltung hinzufügen.

> [!NOTE]
> Das Einschließen eines Ordners Pakete ist typisch für was geschieht, wenn Sie der Projektmappe mit der NuGet-Erweiterung für Visual Studio 2010 die Entity Framework oder ähnliche Ressourcen hinzufügen.


**Nicht projizieren von Inhalten zur quellcodeverwaltung hinzufügen**

1. Stellen Sie sicher, dass die Elemente, die Sie hinzufügen möchten (z. B. den Ordner "Pakete") in einen geeigneten Speicherort innerhalb einer zugeordneten Ordner auf Ihrem lokalen Dateisystem befinden.
2. In Visual Studio 2010 In der **Team Explorer** , erweitern Sie das Teamprojekt, und doppelklicken Sie dann auf **Quellcodeverwaltung**.

    ![](adding-content-to-source-control/_static/image10.png)
3. Auf der **Quellcodeverwaltungs-Explorer** Registerkarte die **Ordner** Bereich auf der Ordner, der das Element enthält, oder Elemente hinzufügen möchten.
4. Klicken Sie auf die **Elemente hinzufügen, um Ordner** Schaltfläche.

    ![](adding-content-to-source-control/_static/image11.png)
5. In der **zur Quellcodeverwaltung hinzufügen** Dialogfeld Feld, wählen Sie den Ordner oder die Elemente, die Sie hinzufügen möchten, und klicken Sie dann auf **Weiter**.

    ![](adding-content-to-source-control/_static/image12.png)
6. Auf der **ausgeschlossene Elemente** Registerkarte, wählen Sie alle erforderlichen Elemente, die wurden (z. B. Assemblys) automatisch ausgeschlossen werden soll, und klicken Sie dann auf **Element(e) einschließen**.

    ![](adding-content-to-source-control/_static/image13.png)
7. Auf der **hinzuzufügenden Elemente** Registerkarte, stellen Sie sicher, dass alle Dateien, die Sie einschließen möchten aufgelistet sind, und klicken Sie dann auf **Fertig stellen**.

    ![](adding-content-to-source-control/_static/image14.png)
8. In der **Quellcodeverwaltungs-Explorer** Fenster, klicken Sie auf die **Einchecken** Schaltfläche.

    ![](adding-content-to-source-control/_static/image15.png)
9. In der **Einchecken – Quelldateien** (Dialogfeld), geben Sie einen Kommentar ein, und klicken Sie dann auf **Einchecken**.

An diesem Punkt haben Sie die externen Abhängigkeiten für Ihre Projektmappe zur quellcodeverwaltung hinzugefügt.

## <a name="conclusion"></a>Schlussfolgerung

In diesem Thema beschrieben, wie eine Verbindung mit einem Teamprojekt herstellen, ordnen Sie eine Ordnerstruktur und Inhalte zur quellcodeverwaltung hinzufügen. Weitere Informationen zum Arbeiten mit Elementen in der quellcodeverwaltung finden Sie unter [verwenden der Versionskontrolle](https://msdn.microsoft.com/en-us/library/ms181368.aspx).

Im nächsten Thema [eine TFS-Build-Server für die Bereitstellung konfigurieren](configuring-a-tfs-build-server-for-web-deployment.md), enthält Informationen zum Vorbereiten eines TFS Team Build-Servers zum Erstellen und Bereitstellen der Projektmappe.

## <a name="further-reading"></a>Weiterführende Themen

Umfassendere Informationen zum Arbeiten mit Datenquellen-Steuerelements in TFS finden Sie unter [verwenden der Versionskontrolle](https://msdn.microsoft.com/en-us/library/ms181368.aspx).

>[!div class="step-by-step"]
[Zurück](creating-a-team-project-in-tfs.md)
[Weiter](configuring-a-tfs-build-server-for-web-deployment.md)
