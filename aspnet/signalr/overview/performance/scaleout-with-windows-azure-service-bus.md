---
uid: signalr/overview/performance/scaleout-with-windows-azure-service-bus
title: SignalR mit horizontaler Skalierung mit Azure Servicebus | Microsoft Docs
author: MikeWasson
description: Versionen der Software verwendet in diesem Thema Visual Studio 2013 .NET 4.5 SignalR Version 2 frühere Versionen dieser Thema für SignalR 1.x-Version dieses Themas...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: ce1305f9-30fd-49e3-bf38-d0a78dfb06c3
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/scaleout-with-windows-azure-service-bus
msc.type: authoredcontent
ms.openlocfilehash: e6d9e4e6ba2040aa2c6e453aacf0ddca38c4a1a9
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/03/2018
---
<a name="signalr-scaleout-with-azure-service-bus"></a>SignalR mit horizontaler Skalierung mit Azure Servicebus
====================
durch [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

In diesem Lernprogramm werden Sie eine SignalR-Anwendung in einer Windows Azure-Webrolle, mit der Service Bus-Rückwandplatine zum Verteilen von Nachrichten für jede Rolleninstanz bereitstellen. (Sie können auch mit die Service Bus-Rückwandplatine [web-apps in Azure App Service](https://docs.microsoft.com/azure/app-service-web/).)

![](scaleout-with-windows-azure-service-bus/_static/image1.png)

Erforderliche Komponenten:

- Ein Windows Azure-Konto.
- Die [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).
- Visual Studio 2012 oder 2013.

Die Servicebus-Rückwandplatine ist kompatibel mit [Service Bus for Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), Version 1.1. Allerdings ist es nicht mit Version 1.0 der Service Bus for Windows Server kompatibel.

## <a name="pricing"></a>Preise

Die Service Bus-Rückwandplatine werden Themen zum Senden von Nachrichten verwendet. Die neuesten Preisinformationen finden Sie unter [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/). Zum Zeitpunkt der Erstellung dieses Dokuments können Sie für weniger als 1 $ 1.000.000 Nachrichten pro Monat senden. Der Rückwand sendet eine Servicebus-Nachricht für jeden Aufruf einer SignalR-Hub-Methode. Es gibt auch einige Steuerungsnachrichten für Verbindungen, Trennvorgänge beitreten oder verlassen Gruppen und So weiter. In den meisten Anwendungen wird die Mehrheit der dem Volumen des Nachrichtenverkehrs hubmethodenaufrufe sein.

## <a name="overview"></a>Übersicht

Bevor wir auf die ausführliches Lernprogramm erhalten, ist hier ein schnellen Überblick darüber, was Sie tun können.

1. Verwenden Sie die Windows Azure-Portal, um einen neuen Service Bus-Namespace zu erstellen.
2. Ihre Anwendung diese NuGet-Pakete hinzugefügt werden: 

    - [Microsoft.AspNet.SignalR](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft.AspNet.SignalR.ServiceBus3](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus3) oder [Microsoft.AspNet.SignalR.ServiceBus](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus)
3. Erstellen einer SignalR-Anwendung.
4. Fügen Sie den folgenden Code, um tritt an der Rückwand zu konfigurieren: 

    [!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample1.cs)]

Dieser Code dient zum Konfigurieren der Rückwand mit den Standardwerten für [TopicCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.servicebusscaleoutconfiguration.topiccount(v=vs.118).aspx) und [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx). Informationen zum Ändern dieser Werte finden Sie unter [SignalR-Leistung: Warteschlangen für horizontale Skalierung Metriken](signalr-performance.md#scaleout_metrics).

Wählen Sie für jede Anwendung einen anderen Wert für "Nameihrerapp" ein. Verwenden Sie nicht den gleichen Wert für mehrere Anwendungen.

## <a name="create-the-azure-services"></a>Erstellen der Azure-Dienste

Erstellen Sie einen Cloud-Dienst aus, wie beschrieben in [zum Erstellen und Bereitstellen eines Cloud-Diensts](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy). Führen Sie die Schritte im Abschnitt "Vorgehensweise: erstellen ein Cloud-Diensts verwenden der Schnellerfassung". Für dieses Lernprogramm müssen Sie sich nicht um ein Zertifikat hochzuladen.

![](scaleout-with-windows-azure-service-bus/_static/image2.png)

Erstellen Sie einen neuen Service Bus-Namespace ein, wie beschrieben in [wie auf verwenden Service Bus-Themen/-Abonnements](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions). Führen Sie die Schritte im Abschnitt "Erstellen einer Dienst-Namespace" aus.

![](scaleout-with-windows-azure-service-bus/_static/image3.png)

> [!NOTE]
> Stellen Sie sicher, dass die gleiche Region für den Clouddienst und Service Bus-Namespace.


## <a name="create-the-visual-studio-project"></a>Visual Studio-Projekt erstellen

Starten Sie Visual Studio. Aus der **Datei** Menü klicken Sie auf **neues Projekt**.

In der **neues Projekt** Dialogfeld erweitern Sie **Visual C#-**. Klicken Sie unter **installierte Vorlagen**Option **Cloud** und wählen Sie dann **Windows Azure-Cloud-Dienst**. Behalten Sie die Standardeinstellung .NET Framework 4.5. Benennen Sie die Anwendung ChatService, und klicken Sie auf **OK**.

![](scaleout-with-windows-azure-service-bus/_static/image4.png)

In der **neuen Windows Azure-Cloud-Dienst** ASP.NET-Webrolle wählen Sie im Dialogfeld. Klicken Sie auf den Pfeil nach rechts (**&gt;**), die der Projektmappe hinzuzufügen.

Bewegen Sie die Maus über die neue Rolle, also das Stiftsymbol angezeigt. Klicken Sie auf dieses Symbol, um die Rolle umbenennen. Benennen Sie die Rolle "SignalRChat", und klicken Sie auf **OK**.

![](scaleout-with-windows-azure-service-bus/_static/image5.png)

In der **neues ASP.NET-Projekt** wählen Sie im Dialogfeld **MVC**, und klicken Sie auf OK.

![](scaleout-with-windows-azure-service-bus/_static/image6.png)

Der Assistent erstellt zwei Projekte:

- ChatService: Dieses Projekt ist das Windows Azure-Anwendung. Er definiert den Azure-Rollen und weitere Konfigurationsoptionen.
- SignalRChat: Dieses Projekt ist das ASP.NET MVC 5-Projekt.

## <a name="create-the-signalr-chat-application"></a>Die SignalR-Chat-Anwendung erstellen

Um die chatanwendung zu erstellen, führen Sie die Schritte im Lernprogramm [erste Schritte mit SignalR und MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).

Installieren Sie die erforderlichen Bibliotheken mithilfe von NuGet. Aus der **Tools** klicken Sie im Menü **Bibliothekspaket-Manager**, und wählen Sie dann **Package Manager Console**. In der **Package Manager Console** Fenster, geben Sie die folgenden Befehle:

[!code-powershell[Main](scaleout-with-windows-azure-service-bus/samples/sample2.ps1)]

Verwenden der `-ProjectName` Option zum Installieren der Pakete anstelle der Windows Azure-Projekt das ASP.NET MVC-Projekt zur Verfügung.

## <a name="configure-the-backplane"></a>Konfigurieren der Rückwand

Fügen Sie in Ihrer Anwendung Startup.cs Datei den folgenden Code hinzu:

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample3.cs)]

Nun müssen Sie die Servicebus-Verbindungszeichenfolge abzurufen. Wählen Sie im Azure-Portal den Servicebus-Namespace, den Sie erstellt haben, und klicken Sie auf das Symbol "Zugriffsschlüssel".

![](scaleout-with-windows-azure-service-bus/_static/image7.png)

Kopieren Sie die Verbindungszeichenfolge in die Zwischenablage, und fügen Sie ihn in die *"ConnectionString"* Variable.

![](scaleout-with-windows-azure-service-bus/_static/image8.png)

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample4.cs)]

## <a name="deploy-to-azure"></a>In Azure bereitstellen

Erweitern Sie im Projektmappen-Explorer die **Rollen** Ordner im Projekt ChatService.

![](scaleout-with-windows-azure-service-bus/_static/image9.png)

Mit der rechten Maustaste in der Rolle "SignalRChat", und wählen Sie **Eigenschaften**. Wählen Sie die **Konfiguration** Registerkarte. Klicken Sie unter **Instanzen** 2 auswählen. Sie können auch die Größe des virtuellen Computers festlegen, um **sehr klein**.

![](scaleout-with-windows-azure-service-bus/_static/image10.png)

Speichern Sie die Änderungen.

Im Projektmappen-Explorer mit der rechten Maustaste des Projekts ChatService. Wählen Sie **Veröffentlichen**.

![](scaleout-with-windows-azure-service-bus/_static/image11.png)

Wenn dies Ihre erste Zeit Veröffentlichungsvorgang in Windows Azure ist, müssen Sie Ihre Anmeldeinformationen herunterladen. In der **veröffentlichen** -Assistenten klicken Sie auf "Sign in to download Credentials". Dadurch werden Sie aufgefordert, sich bei Windows Azure-Portal anmelden und eine Datei mit veröffentlichungseinstellungen herunterladen.

![](scaleout-with-windows-azure-service-bus/_static/image12.png)

Klicken Sie auf **Import** , und wählen Sie die heruntergeladene Datei mit den veröffentlichungseinstellungen.

Klicken Sie auf **Weiter**. In der **Veröffentlichungseinstellungen** Dialogfeld unter **Cloud-Dienst**, wählen Sie den Clouddienst, die Sie zuvor erstellt haben.

![](scaleout-with-windows-azure-service-bus/_static/image13.png)

Klicken Sie auf **Veröffentlichen**. Es dauert einige Minuten, bis die Anwendung bereitstellen und starten die virtuellen Computern.

Beim Ausführen der chatanwendung kommunizieren die Rolleninstanzen jetzt über Azure Service Bus mithilfe eines Service Bus-Themas. Ein Thema ist eine Warteschlange, die mehrere Abonnenten ermöglicht.

Der Rückwand erstellt automatisch das Thema und die Abonnements. Um das Abonnement und die Nachrichtenaktivität anzuzeigen, öffnen Sie das Azure Portal, wählen Sie den Service Bus-Namespace, und klicken Sie auf "Themen".

![](scaleout-with-windows-azure-service-bus/_static/image14.png)

Dafür können einige Minuten, bis die Nachrichtenaktivität im Dashboard angezeigt werden.

![](scaleout-with-windows-azure-service-bus/_static/image15.png)

SignalR verwaltet die Lebensdauer des Themas. Solange die Anwendung bereitgestellt wurde, versuchen Sie nicht manuell Themen löschen oder Ändern von Einstellungen für das Thema.

## <a name="troubleshooting"></a>Problembehandlung

**System.InvalidOperationException "Die einzige unterstützte IsolationLevel ist"IsolationLevel.Serializable"."**

Dieser Fehler kann auftreten, wenn die Transaktionsebene für einen Vorgang auf einen anderen Wert, außer festgelegt ist `Serializable`. Stellen Sie sicher, dass keine Vorgänge mit anderen Ebenen für die Transaktion ausgeführt werden.
