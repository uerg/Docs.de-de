---
title: Fortlaufende Bereitstellung in Azure mit Visual Studio und Git mit ASP.NET Core
author: rick-anderson
description: "Erfahren Sie, wie Sie mit Visual Studio eine ASP.NET Core-Web-App erstellen und sie unter Verwendung von Git für Continuous Deployment in Azure App Service bereitstellen."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/azure-apps/azure-continuous-deployment
ms.openlocfilehash: 7302de1ace62dba53b317039aac7f4763314aa19
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/15/2018
---
# <a name="continuous-deployment-to-azure-with-visual-studio-and-git-with-aspnet-core"></a>Fortlaufende Bereitstellung in Azure mit Visual Studio und Git mit ASP.NET Core

Von [Erik Reitan](https://github.com/Erikre)

[!INCLUDE[Azure App Service Preview Notice](../../includes/azure-apps-preview-notice.md)]

Dieses Lernprogramm zeigt, wie eine ASP.NET Core-Web-app mit Visual Studio erstellen und Bereitstellen von Visual Studio nach Azure App Service mithilfe der kontinuierlichen Bereitstellung.

Siehe auch den Artikel [Use VSTS to Build and Publish to an Azure Web App with Continuous Deployment](/vsts/build-release/archive/apps/aspnet/aspnet-4-ci-cd-azure-automatic), in dem gezeigt wird, wie Sie einen Continuous Delivery-Workflow für [Azure App Service](/azure/app-service/app-service-web-overview) mithilfe von Visual Studio Team Services konfigurieren. Azure Continuous Delivery in Team Services vereinfacht das Einrichten einer robusten Bereitstellungspipeline zum Veröffentlichen von Updates für apps, die in Azure App Service gehostet. Die Pipeline kann konfiguriert werden, aus dem Azure-Portal erstellen, Ausführen von Tests, staging-Slot, und klicken Sie dann für die Produktion bereitstellen.

> [!NOTE]
> Um dieses Lernprogramm abzuschließen, ist ein Microsoft Azure-Konto erforderlich. Ein Konto abrufen [MSDN-abonnentenvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A261C142F) oder [für eine kostenlose Testversion registrieren](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="prerequisites"></a>Erforderliche Komponenten

In diesem Lernprogramm wird davon ausgegangen, dass die folgende Software installiert ist:

* [Visual Studio](https://www.visualstudio.com)
* [.NET Core SDK](https://www.microsoft.com/net/download/core) (Common Language Runtime und die Tools)
* [Git](https://git-scm.com/downloads) für Windows

## <a name="create-an-aspnet-core-web-app"></a>Erstellen einer ASP.NET Core-Web-App

1. Starten Sie Visual Studio.

1. Wählen Sie im Menü **Datei** den Befehl **Neu** > **Projekt** aus.

1. Wählen Sie die Projektvorlage **ASP.NET Core-Webanwendung** aus. Sie wird unter **Installierte** > **Vorlagen** > **Visual C#** > **.NET Core** angezeigt. Benennen Sie das Projekt mit `SampleWebAppDemo`. Wählen Sie die **erstellen neue Git-Repository** aus, und klicken Sie auf **OK**.

   ![Dialogfeld "Neues Projekt"](azure-continuous-deployment/_static/01-new-project.png)

1. Wählen Sie im Dialogfeld **Neues ASP.NET Core-Projekt** die ASP.NET Core-Vorlage **Leer** aus, und klicken Sie dann auf **OK**.

   ![Dialogfeld „Neues ASP.NET-Projekt“](azure-continuous-deployment/_static/02-web-site-template.png)

> [!NOTE]
> Der neueste Version von .NET Core ist 2.0.

### <a name="running-the-web-app-locally"></a>Lokales Ausführen der Web-App

1. Sobald Visual Studio das Erstellen der App abgeschlossen hat, führen Sie die App durch Auswählen von **Debuggen** > **Debuggen starten** aus. Als Alternative, drücken Sie die **F5**.

   Das Initialisieren von Visual Studio und der neuen App dauert möglicherweise einen Moment. Sobald er abgeschlossen ist, zeigt der Browser die ausgeführte app.

   ![Browserfenster mit der ausgeführten Anwendung, in der „Hello World!“ angezeigt wird](azure-continuous-deployment/_static/04-browser-runapp.png)

1. Nach dem Überprüfen der ausgeführten Web-app, schließen Sie den Browser, und wählen Sie das Symbol "Debuggen beenden" auf der Symbolleiste von Visual Studio die app zu beenden.

## <a name="create-a-web-app-in-the-azure-portal"></a>Erstellen einer Web-App im Azure-Portal

Die folgenden Schritte aus erstellen Web-app im Azure-Portal:

1. Melden Sie sich auf die [Azure-Portal](https://portal.azure.com).

1. Wählen Sie **neu** am oben links auf der Portal-Schnittstelle.

1. Wählen Sie **Web + Mobil** > **Web-App**.

   ![Microsoft Azure-Portal: Schaltfläche „Neu“: „Web + Mobil“ unter Marketplace: Schaltfläche „Web-App“ unter „Ausgewählte Apps“](azure-continuous-deployment/_static/05-azure-newwebapp.png)

1. Geben Sie auf dem Blatt **Web-App** für **App Service-Name** einen eindeutigen Wert ein.

   ![Blatt „Web-App“](azure-continuous-deployment/_static/06-azure-newappblade.png)

   > [!NOTE]
   > Die **Name des App Service** Name muss eindeutig sein. Das Portal erzwingt diese Regel aus, wenn der Name angegeben ist. Wenn Sie einen anderen Wert haben, ersetzen Sie diesen Wert für jedes Auftreten der **SampleWebAppDemo** in diesem Lernprogramm.

   Wählen Sie außerdem auf dem Blatt **Web-App** einen vorhandenen **App Service-Plan/Standort** aus, oder erstellen Sie einen neuen. Wenn Sie einen neuen Plan zu erstellen, wählen Sie den Tarif, den Speicherort und andere Optionen. Weitere Informationen zu App Service-Pläne, finden Sie unter [Azure App Service-Pläne detaillierte Übersicht über](/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview).

1. Wählen Sie **Erstellen** aus. Azure bereitstellen und starten Sie die Web-app.

   ![Azure-Portal: Beispiel-Web-App „Demo 01“, Blatt „Zusammenfassung“](azure-continuous-deployment/_static/07-azure-webappblade.png)

## <a name="enable-git-publishing-for-the-new-web-app"></a>Aktivieren der Git-Veröffentlichung für die neue Web-App

Bei Git handelt es sich um eine verteilte Versionskontrollsystem, die zum Bereitstellen einer Azure App Service-Web-app verwendet werden kann. Web-app-Code in einem lokalen Git-Repository gespeichert ist, und der Code in Azure bereitgestellt wird, indem Sie auf einem remote-Repository übertragen.

1. Melden Sie sich bei der [Azure-Portal](https://portal.azure.com).

1. Wählen Sie **Anwendungsdienste** zum Anzeigen einer Liste von der app-Dienste, die mit dem Azure-Abonnement verknüpft sind.

1. Wählen Sie die Web-app, die im vorherigen Abschnitt dieses Lernprogramms erstellt.

1. Klicken Sie auf dem Blatt **Bereitstellung** auf **Bereitstellungsoptionen** > **Quelle auswählen** > **Lokales Git-Repository**.

   ![Blatt „Einstellungen“: Blatt „Bereitstellungsquelle“: Blatt „Quelle auswählen“](azure-continuous-deployment/_static/deployment-options.png)

1. Klicken Sie auf **OK**.

1. Wenn Anmeldeinformationen für die Veröffentlichung von einer Web-app oder eine andere App Service-app für die Bereitstellung zuvor eingerichtet wurde dies nicht getan haben, richten sie nun:

   * Wählen Sie **Einstellungen** > **bereitstellungsanmeldeinformationen**. Die **legen Sie Anmeldeinformationen für die Bereitstellung** Blatt wird angezeigt.
   * Erstellen Sie einen Benutzernamen und ein Kennwort. Speichern Sie das Kennwort für die spätere Verwendung beim Einrichten von Git.
   * Klicken Sie auf **Speichern**.

1. In der **Web-App** Blatt select **Einstellungen** > **Eigenschaften**. Die URL der remote Git-Repositorys zur Bereitstellung auf unterhalb **GIT-URL**.

1. Kopieren Sie den Wert **GIT-URL** zur späteren Verwendung in diesem Tutorial.

   ![Azure-Portal: Blatt „Eigenschaften“ der Anwendung](azure-continuous-deployment/_static/09-azure-giturl.png)

## <a name="publish-the-web-app-to-azure-app-service"></a>Veröffentlichen Sie die Web-app in Azure App Service

Erstellen Sie in diesem Abschnitt ein lokales Git-Repository mit Visual Studio und Push aus diesem Repository in Azure zum Bereitstellen der Web-app. Folgende Schritte müssen ausgeführt werden:

* Fügen Sie die remote-Repository-Einstellung, die mit den GIT-URL-Wert, damit das lokale Repository in Azure bereitgestellt werden kann.
* Festgeschrieben Sie projektänderungen werden.
* Push projektänderungen aus dem lokalen Repository in das Remoterepository in Azure.

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf **Projektmappe „SampleWebAppDemo“**, und wählen Sie **Commit ausführen** aus. Die **Team Explorer** wird angezeigt.

   ![Team Explorer-Registerkarte „Verbinden“](azure-continuous-deployment/_static/10-team-explorer.png)

1. Klicken Sie in **Team Explorer** auf **Start** (Startsymbol) > **Einstellungen** > **Repositoryeinstellungen**.

1. In der **entfernten Datenbanken** Teil der **-Repositoryeinstellungen werden**Option **hinzufügen**. Die **Remote hinzufügen** Dialogfeld wird angezeigt.

1. Legen Sie den **Namen** der Remoteinstanz auf **Azure SampleApp** fest.

1. Legen Sie den Wert für **Fetch** auf die **Git-URL** , die zuvor in diesem Lernprogramm aus Azure kopiert. Beachten Sie, dass dies die URL ist, die mit **.git** endet.

   ![Dialogfeld „Remote bearbeiten“](azure-continuous-deployment/_static/11-add-remote.png)

   > [!NOTE]
   > Geben Sie als Alternative, die remote-Repository aus der **Befehlsfenster** durch Öffnen der **Befehlsfenster**, in das Projektverzeichnis ändern und Sie den Befehl eingeben. Beispiel:
   >
   > `git remote add Azure-SampleApp https://me@sampleapp.scm.azurewebsites.net:443/SampleApp.git`

1. Klicken Sie auf **Start** (Startsymbol) > **Einstellungen** > **Globale Einstellungen**. Vergewissern Sie sich, dass der Name und e-Mail-Adresse festgelegt werden. Wählen Sie **Update** bei Bedarf.

1. Klicken Sie auf **Start** > **Änderungen**, um zur Ansicht **Änderungen** zurückzukehren.

1. Geben Sie eine Commit-Nachricht, z. B. **anfängliche Push Nr. 1** , und wählen Sie **Commit**. Diese Aktion erstellt eine *Commit* lokal.

   ![Team Explorer-Registerkarte „Verbinden“](azure-continuous-deployment/_static/12-initial-commit.png)

   > [!NOTE]
   > Als Alternative können Commit ändert sich von der **Befehlsfenster** durch Öffnen der **Befehlsfenster**, in das Projektverzeichnis ändern und Sie die Git-Befehle eingeben. Beispiel:
   >
   > `git add .`
   >
   > `git commit -am "Initial Push #1"`

1. Klicken Sie auf **Start** > **Synchronisieren** > **Aktionen** > **Eingabeaufforderung öffnen**. Der Eingabeaufforderung in das Projektverzeichnis wird geöffnet.

1. Geben Sie im Befehlsfenster folgenden Befehl ein:

   `git push -u Azure-SampleApp master`

1. Geben Sie den Azure **bereitstellungsanmeldeinformationen** Kennwort, die zuvor in Azure erstellt wurden.

   Dieser Befehl startet den Prozess der lokalen Projektdateien auf Azure zu übertragen. Die Ausgabe der obige Befehl endet mit einer Meldung, dass die Bereitstellung erfolgreich war.

   ```
   remote: Finished successfully.
   remote: Running post deployment command(s)...
   remote: Deployment successful.
   To https://username@samplewebappdemo01.scm.azurewebsites.net:443/SampleWebAppDemo01.git
   * [new branch]      master -> master
   Branch master set up to track remote branch master from Azure-SampleApp.
   ```

   > [!NOTE]
   > Wenn Zusammenarbeit für das Projekt erforderlich ist, sollten Sie erwägen, pushen an [GitHub](https://github.com) vor dem auf Azure übertragen.
 
### <a name="verify-the-active-deployment"></a>Überprüfen der aktiven Bereitstellung

Stellen Sie sicher, dass die Web-app-Übertragung von der lokalen Umgebung in Azure erfolgreich ist.

In der [Azure-Portal](https://portal.azure.com), wählen Sie die Web-app. Wählen Sie **Bereitstellung** > **Bereitstellungsoptionen**.

![Azure-Portal: Blatt „Einstellungen“: Blatt „Bereitstellungen“ mit erfolgreicher Bereitstellung](azure-continuous-deployment/_static/13-verify-deployment.png)

## <a name="run-the-app-in-azure"></a>Ausführen der App in Azure

Nun, dass die Web-app in Azure bereitgestellt wird, führen Sie die app an.

Dies kann auf zwei Arten erfolgen:

* Suchen Sie im Azure-Portal Blatt für die Web-app für die Web-app ein. Wählen Sie **Durchsuchen** um die app in der Standard-Webbrowser anzuzeigen.
* Öffnen Sie einen Browser, und geben Sie die URL für die Web-app. Ein Beispiel: `http://SampleWebAppDemo.azurewebsites.net`

## <a name="update-the-web-app-and-republish"></a>Die Web-app aktualisieren und erneut veröffentlichen

Nach Änderungen an den lokalen Code zu veröffentlichen:

1. Öffnen Sie in Visual Studio im **Projektmappen-Explorer** die Datei *Startup.cs*.

1. Ändern Sie in der `Configure`-Methode die `Response.WriteAsync`-Methode so, dass sie wie folgt angezeigt wird:

   ```csharp
   await context.Response.WriteAsync("Hello World! Deploy to Azure.");
   ```

1. Speichern Sie die Änderungen zu *Startup.cs*.

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf **Projektmappe „SampleWebAppDemo“**, und wählen Sie **Commit ausführen** aus. Die **Team Explorer** wird angezeigt.

1. Geben Sie eine Commit-Nachricht, z. B. `Update #2`.

1. Klicken Sie auf die Schaltfläche **Commit ausführen**, um die Projektänderungen zu committen.

1. Klicken Sie auf **Start** > **Synchronisieren** > **Aktionen** > **Push**.

> [!NOTE]
> Als Alternative können mithilfe von Push übertragen die Änderungen aus der **Befehlsfenster** durch Öffnen der **Befehlsfenster**, in das Projektverzeichnis ändern und Sie einen Git-Befehl eingeben. Beispiel:
> 
> `git push -u Azure-SampleApp master`

## <a name="view-the-updated-web-app-in-azure"></a>Anzeigen der aktualisierten Web-App in Azure

Die aktualisierte Web-app anzeigen, indem er **Durchsuchen** aus dem Blade der Web-app in der Azure-Verwaltungsportal oder durch einen Browser öffnen und die URL für die Web-app eingeben. Ein Beispiel: `http://SampleWebAppDemo.azurewebsites.net`

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Verwenden Sie zum Erstellen und Veröffentlichen einer Azure-Web-App mit Continuous Deployment VSTS](/vsts/build-release/archive/apps/aspnet/aspnet-4-ci-cd-azure-automatic)
* [Projekt Kudu](https://github.com/projectkudu/kudu/wiki)
