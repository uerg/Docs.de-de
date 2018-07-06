---
uid: aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
title: Hosten von OWIN in einer Azure-Workerrolle | Microsoft-Dokumentation
author: MikeWasson
description: In diesem Tutorial wird gezeigt, wie OWIN selbst in einer Microsoft Azure-workerrolle gehostet wird. Open Web Interface for .NET (OWIN) definiert eine Abstraktion zwischen .NET Webserver...
ms.author: aspnetcontent
ms.date: 04/11/2014
ms.assetid: 07aa855a-92ee-4d43-ba66-5bfd7de20ee6
msc.legacyurl: /aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: f62b9299a4e369ae3a938c85e60dd6a79108548d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37826480"
---
<a name="host-owin-in-an-azure-worker-role"></a>Hosten von OWIN in einer Azure-Workerrolle
====================
durch [Mike Wasson](https://github.com/MikeWasson)

> In diesem Tutorial wird gezeigt, wie OWIN selbst in einer Microsoft Azure-workerrolle gehostet wird.
> 
> [Öffnen von Weboberfläche für .NET](http://owin.org/) (OWIN) definiert eine Abstraktion zwischen Webservern für .NET und Webanwendungen. OWIN entkoppelt die Webanwendung aus dem Server, wodurch OWIN ideal für eine Webanwendung in Ihrem eigenen Prozess, außerhalb von IIS Self-hosting – z. B. in einer Azure-workerrolle.
> 
> In diesem Tutorial erfahren Sie, wie Sie Self-Hosting einer OWIN-Anwendung in einer Microsoft Azure-workerrolle. Weitere Informationen zu Workerrollen finden Sie unter [Azure-Ausführungsmodelle](https://azure.microsoft.com/documentation/articles/fundamentals-application-models/#CloudServices).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Softwareversionen, die in diesem Tutorial verwendet werden.
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - [Azure SDK für .NET 2.3](https://azure.microsoft.com/downloads/)
> - [Microsoft.Owin.Selfhost 2.1.0](http://www.nuget.org/packages/Microsoft.Owin.SelfHost/2.1.0)


## <a name="create-a-microsoft-azure-project"></a>Erstellen Sie ein Microsoft Azure-Projekt

Starten Sie Visual Studio mit Administratorrechten aus. Administratorberechtigungen sind erforderlich, um die Anwendung lokal zu debuggen, mit dem Azure-Serveremulator.

Auf der **Datei** Menü klicken Sie auf **neu**, klicken Sie dann auf **Projekt**. Von **installierte Vorlagen**, klicken Sie unter Visual c# **Cloud** , und klicken Sie dann auf **Windows Azure Cloud Service**. Nennen Sie das Projekt "AzureApp", und klicken Sie auf **OK**.

[![](host-owin-in-an-azure-worker-role/_static/image2.png)](host-owin-in-an-azure-worker-role/_static/image1.png)

In der **neuen Windows Azure-Cloud-Dienst** Dialogfeld, doppelklicken Sie auf **Workerrolle**. Übernehmen Sie den Standardnamen ("WorkerRole1") aus. Dieser Schritt wird der Projektmappe eine Worker-Rolle hinzugefügt. Klicken Sie auf **OK**.

[![](host-owin-in-an-azure-worker-role/_static/image4.png)](host-owin-in-an-azure-worker-role/_static/image3.png)

Visual Studio-Projektmappe, die erstellt wird, enthält zwei Projekte:

- &quot;AzureApp&quot; definiert die Rollen und die Konfiguration für die Azure-Anwendung.
- &quot;WorkerRole1&quot; enthält den Code für die Worker-Rolle.

Im Allgemeinen kann eine Azure-Anwendung mehrere Rollen enthalten, obwohl in diesem Tutorial eine einzelne Rolle verwendet.

![](host-owin-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-owin-self-host-packages"></a>Fügen Sie die Self-Hosting-Pakete für OWIN

Von der **Tools** Menü klicken Sie auf **Bibliothekspaket-Manager**, klicken Sie dann auf **-Paket-Manager-Konsole**.

Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl aus:

[!code-console[Main](host-owin-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a>Fügen Sie einen HTTP-Endpunkt hinzu.

Erweitern Sie im Projektmappen-Explorer das Projekt AzureApp aus. Erweitern Sie den Knoten "Rollen", mit der rechten Maustaste WorkerRole1, und wählen **Eigenschaften**.

![](host-owin-in-an-azure-worker-role/_static/image6.png)

Klicken Sie auf **Endpunkte**, und klicken Sie dann auf **Add Endpoint**.

In der **Protokoll** Dropdown-Liste, Option "http". In **öffentlicher Port** und **privater Port**, geben Sie 80. Diese Portnummern können sich unterscheiden. Der öffentliche Port ist, welche Clients verwenden, wenn sie eine Anforderung an die Rolle zu senden.

[![](host-owin-in-an-azure-worker-role/_static/image8.png)](host-owin-in-an-azure-worker-role/_static/image7.png)

## <a name="create-the-owin-startup-class"></a>Erstellen der OWIN-Startup-Klasse

Klicken Sie im Projektmappen-Explorer klicken Sie mit der rechten Maustaste auf das Projekt WorkerRole1, und wählen Sie **hinzufügen** / **Klasse** eine neue Klasse hinzufügen. Nennen Sie die Klasse `Startup`.

Ersetzen Sie alle den Standardcode durch Folgendes:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample2.cs)]

Die `UseWelcomePage` Erweiterungsmethode Fügt eine einfache HTML-Seite Ihrer Anwendung, um zu überprüfen, ob die Website funktioniert.

## <a name="start-the-owin-host"></a>Starten Sie die OWIN-Host

Öffnen Sie die Datei "WorkerRole.cs". Diese Klasse definiert den Code, der ausgeführt wird, wenn die workerrolle gestartet oder beendet wird.

Fügen Sie die folgenden using-Anweisung:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample3.cs)]

Hinzufügen einer **"IDisposable"** Member der `WorkerRole` Klasse:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample4.cs)]

In der `OnStart` -Methode, fügen Sie den folgenden Code zum Starten des Hosts hinzu:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample5.cs?highlight=5)]

Die **WebApp.Start** Methode startet den OWIN-Host. Der Name des der `Startup` Klasse ist ein Typparameter der Methode. Gemäß der Konvention der Host Ruft die `Configure` Methode dieser Klasse.

Überschreiben der `OnStop` zum Verwerfen der  *\_app* Instanz:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample6.cs)]

Hier ist der vollständige Code für "WorkerRole.cs":

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample7.cs)]

Erstellen Sie die Projektmappe, und drücken Sie F5, um die Anwendung lokal im Azure-Serveremulator ausführen. Abhängig von Ihren Firewalleinstellungen müssen Sie den Emulator über die Firewall zulassen.

Der Serveremulator weist eine lokale IP-Adresse an den Endpunkt an. Sie finden die IP-Adresse, indem Sie die Serveremulator-Benutzeroberfläche anzeigen. Mit der rechten Maustaste in des Emulator-Symbols im Infobereich der Taskleiste und, und wählen **Serveremulator-Benutzeroberfläche anzeigen**.

[![](host-owin-in-an-azure-worker-role/_static/image10.png)](host-owin-in-an-azure-worker-role/_static/image9.png)

Suchen Sie die IP-Adresse unter Dienstbereitstellungen, die Bereitstellung [Id], Dienstdetails. Öffnen Sie einen Webbrowser, und navigieren Sie zu http://<em>Adresse</em>, wobei <em>Adresse</em> ist die IP-Adresse vom Serveremulator; z. B. `http://127.0.0.1:80`. Der OWIN-Startseite sollte angezeigt werden:

![](host-owin-in-an-azure-worker-role/_static/image11.png)

## <a name="deploy-to-azure"></a>Bereitstellung in Azure

Für diesen Schritt müssen Sie ein Azure-Konto verfügen. Wenn Sie noch nicht haben, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Weitere Informationen finden Sie unter [kostenlose Microsoft Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).

Klicken Sie im Projektmappen-Explorer das Projekt AzureApp. Wählen Sie **Veröffentlichen**.

![](host-owin-in-an-azure-worker-role/_static/image12.png)

Wenn Sie nicht beim Azure-Konto angemeldet sind, klicken Sie auf **Anmeldung**.

[![](host-owin-in-an-azure-worker-role/_static/image14.png)](host-owin-in-an-azure-worker-role/_static/image13.png)

Nachdem Sie angemeldet sind, wählen Sie ein Abonnement, und klicken Sie auf **Weiter**.

[![](host-owin-in-an-azure-worker-role/_static/image16.png)](host-owin-in-an-azure-worker-role/_static/image15.png)

Geben Sie einen Namen für den Clouddienst, und wählen Sie eine Region aus. Klicken Sie auf **Erstellen**.

![](host-owin-in-an-azure-worker-role/_static/image17.png)

Klicken Sie auf **Veröffentlichen**.

[![](host-owin-in-an-azure-worker-role/_static/image19.png)](host-owin-in-an-azure-worker-role/_static/image18.png)

Das Fenster "Azure Activity Log" zeigt den Fortschritt der Bereitstellung. Wenn die app bereitgestellt wurde, navigieren Sie zur `http://appname.cloudapp.net/`, wobei *Appname* ist der Name des Cloud-Diensts.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Übersicht über das Katana-Projekt](an-overview-of-project-katana.md)
- [Katana-Projekt auf GitHub](https://github.com/aspnet/AspNetKatana/)
