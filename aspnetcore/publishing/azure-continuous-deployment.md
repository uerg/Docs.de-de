---
title: Continuous Deployment in Azure mit Visual Studio und Git
author: rick-anderson
description: "Erfahren Sie, wie Sie mit Visual Studio eine ASP.NET Core-Web-App erstellen und sie unter Verwenden von Git für Continuous Deployment in Azure App Service bereitstellen."
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 2707c7a8-2350-4304-9856-fda58e5c0a16
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/azure-continuous-deployment
ms.openlocfilehash: b576ef6bce3b211afe7465f33dfe62c25dac1f62
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/11/2017
---
# <a name="continuous-deployment-to-azure-for-aspnet-core-with-visual-studio-and-git"></a>Continuous Deployment in Azure für ASP.NET Core mit Visual Studio und Git

Von [Erik Reitan](https://github.com/Erikre)

Dieses Tutorial zeigt, wie Sie mit Visual Studio eine ASP.NET Core-Web-App erstellen und sie aus Visual Studio mithilfe von Continuous Deployment in Azure App Service bereitstellen. 

Siehe auch den Artikel [Use VSTS to Build and Publish to an Azure Web App with Continuous Deployment](https://www.visualstudio.com/docs/build/get-started/aspnet-4-ci-cd-azure-automatic), in dem gezeigt wird, wie Sie einen Continuous Delivery-Workflow für [Azure App Service](https://azure.microsoft.com/documentation/articles/app-service-changes-existing-services/) mithilfe von Visual Studio Team Services konfigurieren. Azure Continuous Delivery in Team Services vereinfacht das Einrichten einer zuverlässigen Bereitstellungspipeline zum Veröffentlichen von Updates für Ihre App in Azure App Service. Die Pipeline kann im Azure-Portal für die folgenden Aufgaben konfiguriert werden: Erstellen von Builds, Ausführen von Tests, Bereitstellen in einem Stagingslot und anschließendes Bereitstellen in der Produktion.

> [!NOTE]
> Für dieses Tutorial benötigen Sie ein Microsoft Azure-Konto. Wenn Sie kein Konto haben, können Sie [Ihre Leistungen für MSDN-Abonnenten aktivieren](http://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) oder [sich für eine kostenlose Testversion registrieren](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).

## <a name="prerequisites"></a>Voraussetzungen

In diesem Tutorial wird davon ausgegangen, dass Sie Folgendes bereits installiert haben:

* [Visual Studio](https://www.visualstudio.com)

* [ASP.NET Core](http://go.microsoft.com/fwlink/?LinkId=627627) (Laufzeit und Tools)

* [Git](http://git-scm.com/downloads) für Windows

## <a name="create-an-aspnet-core-web-app"></a>Erstellen einer ASP.NET Core-Web-App

1. Starten Sie Visual Studio.

2. Wählen Sie im Menü **Datei** den Befehl **Neu** > **Projekt** aus.

3. Wählen Sie die Projektvorlage **ASP.NET-Webanwendung** aus. Sie wird unter **Installierte** > **Vorlagen** > **Visual C#** > **Web** angezeigt. Benennen Sie das Projekt mit `SampleWebAppDemo`. Wählen Sie **Neues Git-Repository** aus, und klicken Sie auf **OK**.

   ![Dialogfeld „Neues Projekt“](azure-continuous-deployment/_static/01-new-project.png)

4. Wählen Sie im Dialogfeld **Neues ASP.NET-Projekt** die ASP.NET Core-Vorlage **Leer** aus, und klicken Sie dann auf **OK**.

   ![Dialogfeld „Neues ASP.NET-Projekt“](azure-continuous-deployment/_static/02-web-site-template.png)


### <a name="running-the-web-app-locally"></a>Lokales Ausführen der Web-App

1. Sobald Visual Studio das Erstellen der App abgeschlossen hat, führen Sie die App durch Auswählen von **Debuggen** -> **Debuggen starten** aus. Alternativ können Sie **F5** drücken.

   Das Initialisieren von Visual Studio und der neuen App dauert möglicherweise einen Moment. Sobald es erfolgt ist, wird die ausgeführte App im Browser angezeigt.

   ![Browserfenster mit der ausgeführten Anwendung, in der „Hello World!“ angezeigt wird](azure-continuous-deployment/_static/04-browser-runapp.png)

2. Nach Überprüfen der ausgeführten Web-App schließen Sie den Browser und klicken auf der Symbolleiste von Visual Studio auf das Symbol „Debuggen beenden“, um die App zu beenden.

## <a name="create-a-web-app-in-the-azure-portal"></a>Erstellen einer Web-App im Azure-Portal

Die folgenden Schritte führen Sie durch die Erstellung einer Web-App im Azure-Portal.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Tippen Sie links oben im Portal auf **NEU**.
3. Tippen Sie auf **Web + Mobil** > **Web-App**.

    ![Microsoft Azure-Portal: Schaltfläche „Neu“: „Web + Mobil“ unter Marketplace: Schaltfläche „Web-App“ unter „Ausgewählte Apps“](azure-continuous-deployment/_static/05-azure-newwebapp.png)

4.  Geben Sie auf dem Blatt **Web-App** für **App Service-Name** einen eindeutigen Wert ein.

    ![Blatt „Web-App“](azure-continuous-deployment/_static/06-azure-newappblade.png)

    >[!NOTE]
    >Der in **App Service-Name** eingegebene Name muss eindeutig sein. Das Portal erzwingt diese Regel, wenn Sie versuchen, den Namen einzugeben. Nachdem Sie einen anderen Wert eingegeben haben, müssen Sie jedes Vorkommen von **SampleWebAppDemo** in diesem Tutorial durch diesen Wert ersetzen.

    &nbsp;
    
    Wählen Sie außerdem auf dem Blatt **Web-App** einen vorhandenen **App Service-Plan/Standort** aus, oder erstellen Sie einen neuen. Wenn Sie einen neuen Plan erstellen, wählen Sie den Tarif, den Standort und andere Optionen aus. Weitere Informationen zu App Service-Plänen finden Sie unter [Azure App Service-Pläne – Detaillierte Übersicht](https://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/).

5.  Klicken Sie auf **Erstellen**. Azure stellt Ihre Web-App bereit und startet sie.

    ![Azure-Portal: Beispiel-Web-App „Demo 01“, Blatt „Zusammenfassung“](azure-continuous-deployment/_static/07-azure-webappblade.png)

## <a name="enable-git-publishing-for-the-new-web-app"></a>Aktivieren der Git-Veröffentlichung für die neue Web-App

Bei Git handelt es sich um ein verteiltes Versionskontrollsystem, mit dem Sie Ihre Azure App Service-Web-App bereitstellen können. Sie speichern den Code, den Sie für Ihre Web-App schreiben, in einem lokalen Git-Repository, und stellen Ihren Code in Azure bereit, indem Sie ihn mithilfe von Push in ein Remoterepository übertragen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, sofern Sie noch nicht angemeldet sind.

2. Klicken Sie unten im Navigationsbereich auf **Durchsuchen**.

3. Klicken Sie auf **Web-Apps**, um eine Liste von Web-Apps anzuzeigen, die Ihrem Azure-Abonnement zugeordnet sind.

4. Wählen Sie die Web-App aus, die Sie im vorherigen Abschnitt dieses Tutorials erstellt haben.

5. Wenn das Blatt **Einstellungen** nicht angezeigt wird, wählen Sie **Einstellungen** auf dem Blatt **Web-App** aus.

6. Klicken Sie auf dem Blatt **Einstellungen** auf **Bereitstellungsquelle** > **Quelle auswählen** > **Lokales Git-Repository**.

   ![Blatt „Einstellungen“: Blatt „Bereitstellungsquelle“: Blatt „Quelle auswählen“](azure-continuous-deployment/_static/08-azure-localrepository.png)

7. Klicken Sie auf **OK**.

8. Wenn Sie zuvor keine Anmeldeinformationen für die Bereitstellung zum Veröffentlichen einer Web-App oder anderen App Service-App festgelegt haben, richten Sie diese nun ein:

   * Klicken Sie auf **Einstellungen** > **Anmeldeinformationen für die Bereitstellung**. Das Blatt **Anmeldeinformationen für die Bereitstellung festlegen** wird angezeigt.

   * Erstellen Sie einen Benutzernamen und ein Kennwort.  Sie benötigen dieses Kennwort später bei der Einrichtung von Git.

   * Klicken Sie auf **Speichern**.

9. Klicken Sie auf dem Blatt **Web-App** auf **Einstellungen** > **Eigenschaften**. Die URL des Git-Remoterepositorys, in dem die Bereitstellung erfolgt, wird unter **GIT-URL** angezeigt.

10. Kopieren Sie den Wert **GIT-URL** zur späteren Verwendung in diesem Tutorial.

   ![Azure-Portal: Blatt „Eigenschaften“ der Anwendung](azure-continuous-deployment/_static/09-azure-giturl.png)

## <a name="publish-your-web-app-to-azure-app-service"></a>Veröffentlichen der App in Azure App Service

In diesem Abschnitt erstellen Sie ein lokales Git-Repository mit Visual Studio und führen einen Pushvorgang aus diesem Repository in Azure aus, um Ihre Web-App bereitzustellen. Folgende Schritte müssen ausgeführt werden:

   * Fügen Sie die Einstellung des Remoterepositorys mithilfe Ihres GIT-URL-Werts hinzu, damit Sie Ihr lokales Repository in Azure bereitstellen können.

   * Führen Sie für Ihre Projektänderungen einen Commit aus.

   * Übertragen Sie Ihre Projektänderungen mithilfe von Push aus Ihrem lokalen Repository in Ihr Remoterepository.

&nbsp;
   
1.  Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf **Projektmappe „SampleWebAppDemo“**, und wählen Sie **Commit ausführen** aus. Der **Team Explorer** wird angezeigt.

    ![Team Explorer-Registerkarte „Verbinden“](azure-continuous-deployment/_static/10-team-explorer.png)

2.  Klicken Sie in **Team Explorer** auf **Start** (Startsymbol) > **Einstellungen** > **Repositoryeinstellungen**.

3.  Klicken Sie in **Repositoryeinstellungen** im Abschnitt **Remotes** auf **Hinzufügen**. Das Dialogfeld **Remote hinzufügen** wird angezeigt.

4.  Legen Sie den **Namen** der Remoteinstanz auf **Azure SampleApp** fest.

5.  Legen Sie den Wert für **Fetch** auf die **Git-URL** fest, die Sie zuvor in diesem Tutorial aus Azure kopiert haben. Beachten Sie, dass dies die URL ist, die mit **.git** endet.

    ![Dialogfeld „Remote bearbeiten“](azure-continuous-deployment/_static/11-add-remote.png)

    >[!NOTE]
    >Als Alternative können Sie das Remoterepository im **Befehlsfenster** angeben, indem Sie das **Befehlsfenster** öffnen, zu Ihrem Projektverzeichnis wechseln und den Befehl eingeben. Beispiel: `git remote add Azure-SampleApp https://me@sampleapp.scm.azurewebsites.net:443/SampleApp.git`

6.  Klicken Sie auf **Start** (Startsymbol) > **Einstellungen** > **Globale Einstellungen**. Vergewissern Sie sicher, dass Sie Ihren Namen und Ihre E-Mail-Adresse festgelegt haben. Sie müssen möglicherweise auch **Aktualisieren** auswählen.

7.  Klicken Sie auf **Start** > **Änderungen**, um zur Ansicht **Änderungen** zurückzukehren.

8.  Geben Sie eine Commit-Nachricht ein, z.B. **Anfänglicher Push Nr. 1**, und klicken Sie auf **Commit ausführen**. Diese Aktion erstellt einen lokalen *Commit*. Als Nächstes müssen Sie eine *Synchronisierung* mit Azure durchführen.

    ![Team Explorer-Registerkarte „Verbinden“](azure-continuous-deployment/_static/12-initial-commit.png)

    >[!NOTE]
    >Als Alternative können Sie für Ihre Änderungen im **Befehlsfenster** einen Commit ausführen, indem Sie das **Befehlsfenster** öffnen, zu Ihrem Projektverzeichnis wechseln und die Git-Befehle eingeben. Beispiel:
    >
    >`git add .`
    >
    >`git commit -am "Initial Push #1"`

9.  Klicken Sie auf **Start** > **Synchronisieren** > **Aktionen** > **Eingabeaufforderung öffnen**. Die Eingabeaufforderung wird in Ihrem Projektverzeichnis geöffnet.

10.  Geben Sie im Befehlsfenster folgenden Befehl ein:

    `git push -u Azure-SampleApp master`

11.  Geben Sie das Kennwort Ihrer Azure-**Anmeldeinformationen für die Bereitstellung** ein, das Sie zuvor in Azure erstellt haben.

    >[!NOTE]
    >Ihr Kennwort wird bei der Eingabe nicht angezeigt.

    Über diesen Befehl startet das Übertragen Ihrer lokalen Projektdateien mithilfe von Push in Azure. Die Ausgabe des obigen Befehls endet mit der Meldung, dass die Bereitstellung erfolgreich war.
        
    ```
    remote: Finished successfully.
    remote: Running post deployment command(s)...
    remote: Deployment successful.
    To https://username@samplewebappdemo01.scm.azurewebsites.net:443/SampleWebAppDemo01.git
    * [new branch]      master -> master
    Branch master set up to track remote branch master from Azure-SampleApp.
    ```
    > [!NOTE]
    > Wenn Sie an einem Projekt mit anderen zusammenarbeiten müssen, sollten Sie zwischen den Pushübertragungen in Azure eine Pushübertragung auf [GitHub](https://github.com) erwägen.
 
### <a name="verify-the-active-deployment"></a>Überprüfen der aktiven Bereitstellung

Sie können überprüfen, ob Sie die Web-App erfolgreich aus Ihrer lokalen Umgebung in Azure übertragen haben. Sie sehen, dass die erfolgreiche Bereitstellung aufgeführt ist.

1. Wählen Sie Ihre Web-App im [Azure-Portal](https://portal.azure.com) aus. Klicken Sie dann auf **Einstellungen** > **Continuous Deployment**.

   ![Azure-Portal: Blatt „Einstellungen“: Blatt „Bereitstellungen“ mit erfolgreicher Bereitstellung](azure-continuous-deployment/_static/13-verify-deployment.png)

## <a name="run-the-app-in-azure"></a>Ausführen der App in Azure

Nachdem Sie Ihre Web-App in Azure bereitgestellt haben, können Sie die App ausführen.

Dazu gibt es zwei Möglichkeiten:

* Wechseln Sie im Azure-Portal zum Blatt „Web-App“ Ihrer Web-App, und klicken Sie auf **Durchsuchen**, um Ihre App in Ihrem Standardbrowser anzuzeigen.

* Öffnen Sie einen Browser, und geben Sie die URL Ihrer Web-App ein. Beispiel:

  `http://SampleWebAppDemo.azurewebsites.net`

## <a name="update-your-web-app-and-republish"></a>Aktualisieren Ihrer Web-App und erneutes Veröffentlichen

Nachdem Sie Änderungen am lokalen Code vorgenommen haben, können Sie sie erneut veröffentlichen.

1.  Öffnen Sie in Visual Studio im **Projektmappen-Explorer** die Datei *Startup.cs*.

2.  Ändern Sie in der `Configure`-Methode die `Response.WriteAsync`-Methode so, dass sie wie folgt angezeigt wird:

    ```aspx-cs
    await context.Response.WriteAsync("Hello World! Deploy to Azure.");
    ```
3.  Speichern Sie die Änderungen an *Startup.cs*.

4.  Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf **Projektmappe „SampleWebAppDemo“**, und wählen Sie **Commit ausführen** aus. Der **Team Explorer** wird angezeigt.

5.  Geben Sie eine Commit-Nachricht ein. Beispiel:

    ```none
    Update #2
    ```

6.  Klicken Sie auf die Schaltfläche **Commit ausführen**, um die Projektänderungen zu committen.

7.  Klicken Sie auf **Start** > **Synchronisieren** > **Aktionen** > **Push**.

>[!NOTE]
>Als Alternative können Sie Ihre Änderungen im **Befehlsfenster** mithilfe von Push übertragen, indem Sie das **Befehlsfenster** öffnen, zu Ihrem Projektverzeichnis wechseln und einen Git-Befehl eingeben. Beispiel:
>
>`git push -u Azure-SampleApp master`

## <a name="view-the-updated-web-app-in-azure"></a>Anzeigen der aktualisierten Web-App in Azure

Zeigen Sie Ihre aktualisierte Web-App an, indem Sie im Azure-Portal auf dem Blatt „Web-App“ **Durchsuchen** auswählen oder einen Browser öffnen und die URL der Web-App eingeben. Beispiel:

   `http://SampleWebAppDemo.azurewebsites.net`

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Veröffentlichung und Bereitstellung](index.md)

* [Projekt Kudu](https://github.com/projectkudu/kudu/wiki)
