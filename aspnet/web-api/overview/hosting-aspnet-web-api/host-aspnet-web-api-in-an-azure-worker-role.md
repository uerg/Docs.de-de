---
uid: web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
title: Hosten von ASP.NET Web API 2 in einer Azure-Workerrolle | Microsoft Docs
author: MikeWasson
description: "Dieses Lernprogramm zeigt, wie zum Hosten von ASP.NET Web-API in einer Azure-Workerrolle mit OWIN Selbsthosting der Web-API-Framework. Öffnen Sie die Weboberfläche für .NET (OWIN) de..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/02/2014
ms.topic: article
ms.assetid: 6980ee2e-d6b0-4a08-8fb6-ab96362dd0e3
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: 326c4a4e274dbc1aa6e09f1d07c4d135e4304484
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="host-aspnet-web-api-2-in-an-azure-worker-role"></a>Hosten von ASP.NET Web API 2 in einer Azure-Workerrolle
====================
durch [Mike Wasson](https://github.com/MikeWasson)

> Dieses Lernprogramm zeigt, wie zum Hosten von ASP.NET Web-API in einer Azure-Workerrolle mit OWIN Selbsthosting der Web-API-Framework.
> 
> [Öffnen Sie die Weboberfläche für .NET](http://owin.org/) (OWIN) definiert eine Abstraktion zwischen .NET Webservern und Webanwendungen. OWIN entkoppelt die Webanwendung aus dem Server, wodurch OWIN ideal für eine Webanwendung in Ihrem eigenen Prozess, außerhalb von IIS-Self-hosting – z. B. in einer Azure-workerrolle.
> 
> In diesem Lernprogramm verwenden Sie das Paket Microsoft.Owin.Host.HttpListener stellt einem HTTP-Server, die verwendet werden, um Selbsthosting OWIN-Anwendungen.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>In diesem Lernprogramm verwendeten Versionen der Software
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - Web-API 2
> - [Azure SDK für .NET 2.3](https://azure.microsoft.com/en-us/downloads/)


## <a name="create-a-microsoft-azure-project"></a>Erstellen Sie ein Microsoft Azure-Projekt

Starten Sie Visual Studio mit Administratorrechten aus. Administratorrechte sind erforderlich, um die Anwendung lokal debuggen, mit dem Azure-Serveremulator.

Auf der **Datei** Menü klicken Sie auf **neu**, klicken Sie dann auf **Projekt**. Von **installierte Vorlagen**, klicken Sie unter Visual C#- **Cloud** , und klicken Sie dann auf **Windows Azure-Cloud-Dienst**. Nennen Sie das Projekt "AzureApp", und klicken Sie auf **OK**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image2.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image1.png)

In der **neuen Windows Azure-Cloud-Dienst** Dialogfeld, doppelklicken Sie auf **Workerrolle**. Lassen Sie den Standardnamen ("WorkerRole1"). Dieser Schritt wird der Projektmappe eine workerrolle hinzugefügt. Klicken Sie auf **OK**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image4.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image3.png)

Visual Studio-Projektmappe, die erstellt wird, enthält zwei Projekte:

- &quot;AzureApp&quot; definiert die Rollen und die Konfiguration für die Azure-Anwendung.
- &quot;WorkerRole1&quot; enthält den Code für die workerrolle.

Im Allgemeinen kann eine Azure-Anwendung mehrere Rollen enthalten, obwohl in diesem Lernprogramm eine einzige Rolle verwendet wird.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-web-api-and-owin-packages"></a>Hinzufügen der Web-API und die OWIN-Pakete

Aus der **Tools** Menü klicken Sie auf **Bibliothekspaket-Manager**, klicken Sie dann auf **Package Manager Console**.

Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl aus:

[!code-console[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a>Fügen Sie einen HTTP-Endpunkt

Erweitern Sie im Projektmappen-Explorer das Projekt AzureApp. Erweitern Sie den Knoten Rollen, mit der rechten Maustaste WorkerRole1, und wählen **Eigenschaften**.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image6.png)

Klicken Sie auf **Endpunkte**, und klicken Sie dann auf **Add Endpoint**.

In der **Protokoll** Dropdown-Liste, wählen Sie "http". In **öffentlicher Port** und **privater Port**, geben den Wert 80. Diese Portnummern können unterschiedlich sein. Der öffentliche Port wird von Clients verwendet, beim Versand einer Anforderungs in die Rolle.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image8.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image7.png)

## <a name="configure-web-api-for-self-host"></a>Konfigurieren von Web-API für Selbsthosting

Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste, klicken Sie auf das Projekt WorkerRole1, und wählen **hinzufügen** / **Klasse** So fügen Sie eine neue Klasse hinzu. Nennen Sie die Klasse `Startup`.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image9.png)

Ersetzen Sie alle in dieser Datei den Standardcode durch Folgendes:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample2.cs)]

## <a name="add-a-web-api-controller"></a>Fügen Sie eine Web-API-Controller hinzu

Als Nächstes fügen Sie eine Web-API-Controllerklasse hinzu. Mit der rechten Maustaste des WorkerRole1-Projekts, und wählen Sie **hinzufügen** / **Klasse**. Benennen Sie die Klasse TestController. Ersetzen Sie alle in dieser Datei den Standardcode durch Folgendes:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample3.cs)]

Der Einfachheit halber definiert diesen Controller nur zwei GET-Methoden, die nur-Text zurückgeben.

## <a name="start-the-owin-host"></a>Starten Sie den OWIN-Host

Öffnen der Datei WorkerRole.cs hinzu. Diese Klasse definiert den Code, der ausgeführt wird, wenn die workerrolle gestartet oder beendet wird.

Fügen Sie die folgende Anweisung:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample4.cs)]

Hinzufügen einer **IDisposable** Member der `WorkerRole` Klasse:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample5.cs)]

In der `OnStart` -Methode, fügen Sie den folgenden Code zum Starten des Hosts hinzu:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample6.cs?highlight=5)]

Die **WebApp.Start** Methode startet den OWIN-Host. Der Name des der `Startup` Klasse ist ein Typparameter der Methode. Gemäß der Konvention werden der Host Ruft die `Configure` Methode dieser Klasse.

Überschreiben Sie die `OnStop` Löschen der  *\_app* Instanz:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample7.cs)]

Hier wird der vollständige Code für WorkerRole.cs:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample8.cs)]

Erstellen Sie die Projektmappe, und drücken Sie F5, um die Anwendung lokal im Azure-Serveremulator auszuführen. Abhängig von Ihren Firewalleinstellungen müssen Sie möglicherweise den Emulator über die Firewall zulassen.

> [!NOTE]
> Wenn Sie eine der folgenden vergleichbare Ausnahme angezeigt werden, finden Sie unter [diesem Blogbeitrag](https://blogs.msdn.com/b/praburaj/archive/2013/11/20/fileloadexception-on-microsoft-owin-when-running-on-worker-role.aspx) für dieses Problem zu umgehen. "Konnte nicht geladen werden, Datei oder Assembly" "Microsoft.owin", Version = 2.0.2.0, Culture = Neutral, PublicKeyToken = 31bf3856ad364e35' oder eine ihrer Abhängigkeiten. Das manifest der gefunden Assemblydefinition entspricht nicht der Assemblyverweis verweist. (Ausnahme von HRESULT: 0x80131040) "


Der Serveremulator weist eine lokale IP-Adresse an den Endpunkt an. Sie können die IP-Adresse finden, indem Sie den Serveremulator-Benutzeroberfläche anzeigen. Maustaste auf das Symbol "Emulator" in den Infobereich der Taskleiste, und wählen Sie **Serveremulator-Benutzeroberfläche anzeigen**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image11.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image10.png)

Suchen Sie die IP-Adresse unter Dienstbereitstellungen, Bereitstellung [Id], Dienstdetails. Öffnen Sie einen Webbrowser, und navigieren Sie zu http://*Adresse*/Test/1, in dem *Adresse* ist die IP-Adresse zugewiesen, die vom Serveremulator; z. B. `http://127.0.0.1:80/test/1`. Die Antwort aus dem Web-API-Controller sollte angezeigt werden:

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image12.png)

## <a name="deploy-to-azure"></a>In Azure bereitstellen

Für diesen Schritt müssen Sie ein Azure-Konto verfügen. Wenn Sie einen noch nicht haben, können Sie in wenigen Minuten ein kostenloses Testkonto erstellen. Weitere Informationen finden Sie unter [kostenlosen Microsoft Azure-Testversion](https://azure.microsoft.com/en-us/pricing/free-trial/?WT.mc_id=A261C142F).

Im Projektmappen-Explorer mit der rechten Maustaste des Projekts AzureApp. Wählen Sie **Veröffentlichen**.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image13.png)

Wenn Sie nicht beim Azure-Konto angemeldet sind, klicken Sie auf **anmelden**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image15.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image14.png)

Nachdem Sie sich angemeldet haben, wählen Sie ein Abonnement, und klicken Sie auf **Weiter**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image17.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image16.png)

Geben Sie einen Namen für den Clouddienst, und wählen Sie eine Region aus. Klicken Sie auf **Erstellen**.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image18.png)

Klicken Sie auf **Veröffentlichen**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image20.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image19.png)

Das Azure-Aktivitätsprotokoll-Fenster wird der Fortschritt der Bereitstellung. Wenn die app bereitgestellt wurde, navigieren Sie zu http://appname.cloudapp.net/test/1.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image21.png)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Eine Übersicht über Project Katana](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)
- [Katana-Projekt auf GitHub](https://github.com/aspnet/AspNetKatana)
