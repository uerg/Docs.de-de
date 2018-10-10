---
uid: web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
title: Hosten von ASP.NET Web-API 2 in ein Azure-Workerrolle | Microsoft-Dokumentation
author: MikeWasson
description: Dieses Tutorial zeigt, wie zum Hosten von ASP.NET Web-API in einer Azure-Workerrolle mit OWIN zum selfhosten der Web-API-Framework. Öffnen von Weboberfläche für .NET (OWIN) de...
ms.author: riande
ms.date: 04/02/2014
ms.assetid: 6980ee2e-d6b0-4a08-8fb6-ab96362dd0e3
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: 40cb1a4514beaf81e7ed75bbd3e478f2ba146fe5
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2018
ms.locfileid: "48910745"
---
<a name="host-aspnet-web-api-2-in-an-azure-worker-role"></a>Hosten von ASP.NET Web-API 2 in ein Azure-Workerrolle
====================
durch [Mike Wasson](https://github.com/MikeWasson)

> Dieses Tutorial zeigt, wie zum Hosten von ASP.NET Web-API in einer Azure-Workerrolle mit OWIN zum selfhosten der Web-API-Framework.
>
> [Öffnen von Weboberfläche für .NET](http://owin.org/) (OWIN) definiert eine Abstraktion zwischen Webservern für .NET und Webanwendungen. OWIN entkoppelt die Webanwendung aus dem Server, wodurch OWIN ideal für eine Webanwendung in Ihrem eigenen Prozess, außerhalb von IIS Self-hosting – z. B. in einer Azure-workerrolle.
>
> In diesem Tutorial verwenden Sie das Paket Microsoft.Owin.Host.HttpListener, die einem HTTP-Server enthält, die verwendet werden, um die OWIN-Anwendungen Self-Hosting.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Softwareversionen, die in diesem Tutorial verwendet werden.
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - Web-API 2
> - [Azure SDK für .NET 2.3](https://azure.microsoft.com/downloads/)


## <a name="create-a-microsoft-azure-project"></a>Erstellen Sie ein Microsoft Azure-Projekt

Starten Sie Visual Studio mit Administratorrechten aus. Administratorberechtigungen sind erforderlich, um die Anwendung lokal zu debuggen, mit dem Azure-Serveremulator.

Auf der **Datei** Menü klicken Sie auf **neu**, klicken Sie dann auf **Projekt**. Von **installierte Vorlagen**, klicken Sie unter Visual c# **Cloud** , und klicken Sie dann auf **Windows Azure Cloud Service**. Nennen Sie das Projekt "AzureApp", und klicken Sie auf **OK**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image2.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image1.png)

In der **neuen Windows Azure-Cloud-Dienst** Dialogfeld, doppelklicken Sie auf **Workerrolle**. Übernehmen Sie den Standardnamen ("WorkerRole1") aus. Dieser Schritt wird der Projektmappe eine Worker-Rolle hinzugefügt. Klicken Sie auf **OK**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image4.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image3.png)

Visual Studio-Projektmappe, die erstellt wird, enthält zwei Projekte:

- &quot;AzureApp&quot; definiert die Rollen und die Konfiguration für die Azure-Anwendung.
- &quot;WorkerRole1&quot; enthält den Code für die Worker-Rolle.

Im Allgemeinen kann eine Azure-Anwendung mehrere Rollen enthalten, obwohl in diesem Tutorial eine einzelne Rolle verwendet.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-web-api-and-owin-packages"></a>Fügen Sie die Web-API und OWIN-Pakete

Von der **Tools** Menü klicken Sie auf **NuGet Package Manager**, klicken Sie dann auf **-Paket-Manager-Konsole**.

Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl aus:

[!code-console[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a>Fügen Sie einen HTTP-Endpunkt hinzu.

Erweitern Sie im Projektmappen-Explorer das Projekt AzureApp aus. Erweitern Sie den Knoten "Rollen", mit der rechten Maustaste WorkerRole1, und wählen **Eigenschaften**.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image6.png)

Klicken Sie auf **Endpunkte**, und klicken Sie dann auf **Add Endpoint**.

In der **Protokoll** Dropdown-Liste, Option "http". In **öffentlicher Port** und **privater Port**, geben Sie 80. Diese Portnummern können sich unterscheiden. Der öffentliche Port ist, welche Clients verwenden, wenn sie eine Anforderung an die Rolle zu senden.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image8.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image7.png)

## <a name="configure-web-api-for-self-host"></a>Konfigurieren von Web-API für selbst gehostete Dienste

Klicken Sie im Projektmappen-Explorer klicken Sie mit der rechten Maustaste auf das Projekt WorkerRole1, und wählen Sie **hinzufügen** / **Klasse** eine neue Klasse hinzufügen. Nennen Sie die Klasse `Startup`.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image9.png)

Ersetzen Sie alle die Codebausteine in dieser Datei durch Folgendes:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample2.cs)]

## <a name="add-a-web-api-controller"></a>Fügen Sie eine Web-API-Controller hinzu.

Als Nächstes fügen Sie eine Web-API-Controller-Klasse hinzu. Mit der rechten Maustaste in des WorkerRole1-Projekts, und wählen Sie **hinzufügen** / **Klasse**. Der Name der TestController-Klasse. Ersetzen Sie alle die Codebausteine in dieser Datei durch Folgendes:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample3.cs)]

Der Einfachheit halber definiert diesen Controller lediglich zwei GET-Methoden, die nur-Text zurückgeben.

## <a name="start-the-owin-host"></a>Starten Sie die OWIN-Host

Öffnen Sie die Datei "WorkerRole.cs". Diese Klasse definiert den Code, der ausgeführt wird, wenn die workerrolle gestartet oder beendet wird.

Fügen Sie die folgenden using-Anweisung:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample4.cs)]

Hinzufügen einer **"IDisposable"** Member der `WorkerRole` Klasse:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample5.cs)]

In der `OnStart` -Methode, fügen Sie den folgenden Code zum Starten des Hosts hinzu:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample6.cs?highlight=5)]

Die **WebApp.Start** Methode startet den OWIN-Host. Der Name des der `Startup` Klasse ist ein Typparameter der Methode. Gemäß der Konvention der Host Ruft die `Configure` Methode dieser Klasse.

Überschreiben der `OnStop` zum Verwerfen der  *\_app* Instanz:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample7.cs)]

Hier ist der vollständige Code für "WorkerRole.cs":

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample8.cs)]

Erstellen Sie die Projektmappe, und drücken Sie F5, um die Anwendung lokal im Azure-Serveremulator ausführen. Abhängig von Ihren Firewalleinstellungen müssen Sie den Emulator über die Firewall zulassen.

> [!NOTE]
> Wenn Sie eine Ausnahme wie die folgende erhalten, finden Sie unter [in diesem Blogbeitrag](https://blogs.msdn.com/b/praburaj/archive/2013/11/20/fileloadexception-on-microsoft-owin-when-running-on-worker-role.aspx) für dieses Problem zu umgehen. "Konnte nicht geladen werden, Datei oder Assembly ' Microsoft.Owin, Version = 2.0.2.0, Kultur = Neutral, PublicKeyToken = 31bf3856ad364e35' oder eine ihrer Abhängigkeiten. Das manifest der sich Assemblydefinition entspricht nicht den der Assemblyverweis verweist. (Ausnahme von HRESULT: 0x80131040) "


Der Serveremulator weist eine lokale IP-Adresse an den Endpunkt an. Sie finden die IP-Adresse, indem Sie die Serveremulator-Benutzeroberfläche anzeigen. Mit der rechten Maustaste in des Emulator-Symbols im Infobereich der Taskleiste und, und wählen **Serveremulator-Benutzeroberfläche anzeigen**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image11.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image10.png)

Suchen Sie die IP-Adresse unter Dienstbereitstellungen, die Bereitstellung [Id], Dienstdetails. Öffnen Sie einen Webbrowser, und navigieren Sie zu http://<em>Adresse</em>/Test/1, in denen <em>Adresse</em> ist die IP-Adresse vom Serveremulator; z. B. `http://127.0.0.1:80/test/1`. Die Antwort von der Web-API-Controller sollte angezeigt werden:

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image12.png)

## <a name="deploy-to-azure"></a>Bereitstellung in Azure

Für diesen Schritt müssen Sie ein Azure-Konto verfügen. Wenn Sie noch nicht haben, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Weitere Informationen finden Sie unter [kostenlose Microsoft Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).

Klicken Sie im Projektmappen-Explorer das Projekt AzureApp. Wählen Sie **Veröffentlichen**.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image13.png)

Wenn Sie nicht beim Azure-Konto angemeldet sind, klicken Sie auf **Anmeldung**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image15.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image14.png)

Nachdem Sie angemeldet sind, wählen Sie ein Abonnement, und klicken Sie auf **Weiter**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image17.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image16.png)

Geben Sie einen Namen für den Clouddienst, und wählen Sie eine Region aus. Klicken Sie auf **Erstellen**.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image18.png)

Klicken Sie auf **Veröffentlichen**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image20.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image19.png)

Das Fenster "Azure Activity Log" zeigt den Fortschritt der Bereitstellung. Wenn die app bereitgestellt wurde, navigieren Sie zur http://appname.cloudapp.net/test/1.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image21.png)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Übersicht über das Katana-Projekt](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)
- [Katana-Projekt auf GitHub](https://github.com/aspnet/AspNetKatana)
