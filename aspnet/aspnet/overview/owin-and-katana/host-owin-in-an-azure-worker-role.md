---
uid: aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
title: Hosten in einer Azure-Workerrolle OWIN | Microsoft Docs
author: MikeWasson
description: Dieses Lernprogramm zeigt, wie OWIN selbst in einer Microsoft Azure-workerrolle hosten. Open Web-Schnittstelle für .NET (OWIN) definiert eine Abstraktion zwischen .NET Webserver...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/11/2014
ms.topic: article
ms.assetid: 07aa855a-92ee-4d43-ba66-5bfd7de20ee6
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: 13bccc4b2d6f1b22c94446deaf6795dab766275b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868424"
---
<a name="host-owin-in-an-azure-worker-role"></a>Host in einer Azure-Workerrolle OWIN
====================
durch [Mike Wasson](https://github.com/MikeWasson)

> Dieses Lernprogramm zeigt, wie OWIN selbst in einer Microsoft Azure-workerrolle hosten.
> 
> [Öffnen Sie die Weboberfläche für .NET](http://owin.org/) (OWIN) definiert eine Abstraktion zwischen .NET Webservern und Webanwendungen. OWIN entkoppelt die Webanwendung aus dem Server, wodurch OWIN ideal für eine Webanwendung in Ihrem eigenen Prozess, außerhalb von IIS-Self-hosting – z. B. in einer Azure-workerrolle.
> 
> In diesem Lernprogramm erfahren Sie, wie einer OWIN-Anwendung in eine Microsoft Azure-Worker-Rolle selbst gehostet. Weitere Informationen zu Workerrollen finden Sie unter [Azure-Ausführungsmodelle](https://azure.microsoft.com/documentation/articles/fundamentals-application-models/#CloudServices).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>In diesem Lernprogramm verwendeten Versionen der Software
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - [Azure SDK für .NET 2.3](https://azure.microsoft.com/downloads/)
> - [Microsoft.Owin.Selfhost 2.1.0](http://www.nuget.org/packages/Microsoft.Owin.SelfHost/2.1.0)


## <a name="create-a-microsoft-azure-project"></a>Erstellen Sie ein Microsoft Azure-Projekt

Starten Sie Visual Studio mit Administratorrechten aus. Administratorrechte sind erforderlich, um die Anwendung lokal debuggen, mit dem Azure-Serveremulator.

Auf der **Datei** Menü klicken Sie auf **neu**, klicken Sie dann auf **Projekt**. Von **installierte Vorlagen**, klicken Sie unter Visual C#- **Cloud** , und klicken Sie dann auf **Windows Azure-Cloud-Dienst**. Nennen Sie das Projekt "AzureApp", und klicken Sie auf **OK**.

[![](host-owin-in-an-azure-worker-role/_static/image2.png)](host-owin-in-an-azure-worker-role/_static/image1.png)

In der **neuen Windows Azure-Cloud-Dienst** Dialogfeld, doppelklicken Sie auf **Workerrolle**. Lassen Sie den Standardnamen ("WorkerRole1"). Dieser Schritt wird der Projektmappe eine workerrolle hinzugefügt. Klicken Sie auf **OK**.

[![](host-owin-in-an-azure-worker-role/_static/image4.png)](host-owin-in-an-azure-worker-role/_static/image3.png)

Visual Studio-Projektmappe, die erstellt wird, enthält zwei Projekte:

- &quot;AzureApp&quot; definiert die Rollen und die Konfiguration für die Azure-Anwendung.
- &quot;WorkerRole1&quot; enthält den Code für die workerrolle.

Im Allgemeinen kann eine Azure-Anwendung mehrere Rollen enthalten, obwohl in diesem Lernprogramm eine einzige Rolle verwendet wird.

![](host-owin-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-owin-self-host-packages"></a>Hinzufügen der Pakete für die OWIN-Selbsthosting

Aus der **Tools** Menü klicken Sie auf **Bibliothekspaket-Manager**, klicken Sie dann auf **Package Manager Console**.

Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl aus:

[!code-console[Main](host-owin-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a>Fügen Sie einen HTTP-Endpunkt

Erweitern Sie im Projektmappen-Explorer das Projekt AzureApp. Erweitern Sie den Knoten Rollen, mit der rechten Maustaste WorkerRole1, und wählen **Eigenschaften**.

![](host-owin-in-an-azure-worker-role/_static/image6.png)

Klicken Sie auf **Endpunkte**, und klicken Sie dann auf **Add Endpoint**.

In der **Protokoll** Dropdown-Liste, wählen Sie "http". In **öffentlicher Port** und **privater Port**, geben den Wert 80. Diese Portnummern können unterschiedlich sein. Der öffentliche Port wird von Clients verwendet, beim Versand einer Anforderungs in die Rolle.

[![](host-owin-in-an-azure-worker-role/_static/image8.png)](host-owin-in-an-azure-worker-role/_static/image7.png)

## <a name="create-the-owin-startup-class"></a>Erstellen Sie die OWIN-Start-Klasse

Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste, klicken Sie auf das Projekt WorkerRole1, und wählen **hinzufügen** / **Klasse** So fügen Sie eine neue Klasse hinzu. Nennen Sie die Klasse `Startup`.

Ersetzen Sie alle den Standardcode durch Folgendes:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample2.cs)]

Die `UseWelcomePage` Erweiterungsmethode Fügt eine einfache HTML-Seite für Ihre Anwendung, um zu überprüfen, ob der Standort funktioniert.

## <a name="start-the-owin-host"></a>Starten Sie den OWIN-Host

Öffnen der Datei WorkerRole.cs hinzu. Diese Klasse definiert den Code, der ausgeführt wird, wenn die workerrolle gestartet oder beendet wird.

Fügen Sie die folgende Anweisung:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample3.cs)]

Hinzufügen einer **IDisposable** Member der `WorkerRole` Klasse:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample4.cs)]

In der `OnStart` -Methode, fügen Sie den folgenden Code zum Starten des Hosts hinzu:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample5.cs?highlight=5)]

Die **WebApp.Start** Methode startet den OWIN-Host. Der Name des der `Startup` Klasse ist ein Typparameter der Methode. Gemäß der Konvention werden der Host Ruft die `Configure` Methode dieser Klasse.

Überschreiben Sie die `OnStop` Löschen der  *\_app* Instanz:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample6.cs)]

Hier wird der vollständige Code für WorkerRole.cs:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample7.cs)]

Erstellen Sie die Projektmappe, und drücken Sie F5, um die Anwendung lokal im Azure-Serveremulator auszuführen. Abhängig von Ihren Firewalleinstellungen müssen Sie möglicherweise den Emulator über die Firewall zulassen.

Der Serveremulator weist eine lokale IP-Adresse an den Endpunkt an. Sie können die IP-Adresse finden, indem Sie den Serveremulator-Benutzeroberfläche anzeigen. Maustaste auf das Symbol "Emulator" in den Infobereich der Taskleiste, und wählen Sie **Serveremulator-Benutzeroberfläche anzeigen**.

[![](host-owin-in-an-azure-worker-role/_static/image10.png)](host-owin-in-an-azure-worker-role/_static/image9.png)

Suchen Sie die IP-Adresse unter Dienstbereitstellungen, Bereitstellung [Id], Dienstdetails. Öffnen Sie einen Webbrowser, und navigieren Sie zu http://<em>Adresse</em>, wobei <em>Adresse</em> ist die IP-Adresse zugewiesen, die vom Serveremulator; z. B. `http://127.0.0.1:80`. Die OWIN-Willkommensseite sollte angezeigt werden:

![](host-owin-in-an-azure-worker-role/_static/image11.png)

## <a name="deploy-to-azure"></a>In Azure bereitstellen

Für diesen Schritt müssen Sie ein Azure-Konto verfügen. Wenn Sie einen noch nicht haben, können Sie in wenigen Minuten ein kostenloses Testkonto erstellen. Weitere Informationen finden Sie unter [kostenlosen Microsoft Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).

Im Projektmappen-Explorer mit der rechten Maustaste des Projekts AzureApp. Wählen Sie **Veröffentlichen**.

![](host-owin-in-an-azure-worker-role/_static/image12.png)

Wenn Sie nicht beim Azure-Konto angemeldet sind, klicken Sie auf **anmelden**.

[![](host-owin-in-an-azure-worker-role/_static/image14.png)](host-owin-in-an-azure-worker-role/_static/image13.png)

Nachdem Sie sich angemeldet haben, wählen Sie ein Abonnement, und klicken Sie auf **Weiter**.

[![](host-owin-in-an-azure-worker-role/_static/image16.png)](host-owin-in-an-azure-worker-role/_static/image15.png)

Geben Sie einen Namen für den Clouddienst, und wählen Sie eine Region aus. Klicken Sie auf **Erstellen**.

![](host-owin-in-an-azure-worker-role/_static/image17.png)

Klicken Sie auf **Veröffentlichen**.

[![](host-owin-in-an-azure-worker-role/_static/image19.png)](host-owin-in-an-azure-worker-role/_static/image18.png)

Das Azure-Aktivitätsprotokoll-Fenster wird der Fortschritt der Bereitstellung. Wenn die app bereitgestellt wurde, navigieren Sie zu `http://appname.cloudapp.net/`, wobei *Appname* ist der Name des Cloud-Diensts.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Übersicht über das Katana-Projekt](an-overview-of-project-katana.md)
- [Katana-Projekt auf GitHub](https://github.com/aspnet/AspNetKatana/)
