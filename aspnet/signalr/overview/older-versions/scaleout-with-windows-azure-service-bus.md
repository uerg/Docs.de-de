---
uid: signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
title: SignalR mit horizontaler Skalierung mit Azure Servicebus (SignalR 1.x) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/01/2013
ms.topic: article
ms.assetid: 501db899-e68c-49ff-81b2-1dc561bfe908
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
msc.type: authoredcontent
ms.openlocfilehash: b48a7b04701b69f68a492c0f7e08da4a37a92a48
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="signalr-scaleout-with-azure-service-bus-signalr-1x"></a>SignalR mit horizontaler Skalierung mit Azure Servicebus (SignalR 1.x)
====================
durch [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

In diesem Lernprogramm werden Sie eine SignalR-Anwendung in einer Windows Azure-Webrolle, mit der Service Bus-Rückwandplatine zum Verteilen von Nachrichten für jede Rolleninstanz bereitstellen.

![](scaleout-with-windows-azure-service-bus/_static/image1.png)

Erforderliche Komponenten:

- Ein Windows Azure-Konto.
- Die [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).
- Visual Studio 2012.

Die Servicebus-Rückwandplatine ist kompatibel mit [Service Bus for Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), Version 1.1. Allerdings ist es nicht mit Version 1.0 der Service Bus for Windows Server kompatibel.

## <a name="pricing"></a>Preise

Die Service Bus-Rückwandplatine werden Themen zum Senden von Nachrichten verwendet. Die neuesten Preisinformationen finden Sie unter [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/). Zum Zeitpunkt der Erstellung dieses Dokuments können Sie für weniger als 1 $ 1.000.000 Nachrichten pro Monat senden. Der Rückwand sendet eine Servicebus-Nachricht für jeden Aufruf einer SignalR-Hub-Methode. Es gibt auch einige Steuerungsnachrichten für Verbindungen, Trennvorgänge beitreten oder verlassen Gruppen und So weiter. In den meisten Anwendungen wird die Mehrheit der dem Volumen des Nachrichtenverkehrs hubmethodenaufrufe sein.

## <a name="overview"></a>Übersicht

Bevor wir auf die ausführliches Lernprogramm erhalten, ist hier ein schnellen Überblick darüber, was Sie tun können.

1. Verwenden Sie die Windows Azure-Portal, um einen neuen Service Bus-Namespace zu erstellen.
2. Ihre Anwendung diese NuGet-Pakete hinzugefügt werden: 

    - [Microsoft.AspNet.SignalR](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft.AspNet.SignalR.ServiceBus](http://www.nuget.org/packages/SignalR.WindowsAzureServiceBus)
3. Erstellen einer SignalR-Anwendung.
4. Fügen Sie den folgenden Code, um "Global.asax" So konfigurieren Sie die Backplane: 

    [!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample1.cs)]

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

In der **neuen Windows Azure-Cloud-Dienst** Dialogfeld Wählen Sie ASP.NET MVC 4-Webrolle. Klicken Sie auf den Pfeil nach rechts (**&gt;**), die der Projektmappe hinzuzufügen.

Bewegen Sie die Maus über die neue Rolle, also das Stiftsymbol angezeigt. Klicken Sie auf dieses Symbol, um die Rolle umbenennen. Benennen Sie die Rolle "SignalRChat", und klicken Sie auf **OK**.

![](scaleout-with-windows-azure-service-bus/_static/image5.png)

In der **neues ASP.NET MVC 4-Projekt** Assistenten **Internetanwendung**. Klicken Sie auf **OK**. Der Assistent erstellt zwei Projekte:

- ChatService: Dieses Projekt ist das Windows Azure-Anwendung. Er definiert den Azure-Rollen und weitere Konfigurationsoptionen.
- SignalRChat: Dieses Projekt ist das ASP.NET MVC 4-Projekt.

## <a name="create-the-signalr-chat-application"></a>Die SignalR-Chat-Anwendung erstellen

Um die chatanwendung zu erstellen, führen Sie die Schritte im Lernprogramm [erste Schritte mit SignalR und MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).

Installieren Sie die erforderlichen Bibliotheken mithilfe von NuGet. Aus der **Tools** klicken Sie im Menü **Bibliothekspaket-Manager**, und wählen Sie dann **Package Manager Console**. In der **Package Manager Console** Fenster, geben Sie die folgenden Befehle:

[!code-powershell[Main](scaleout-with-windows-azure-service-bus/samples/sample2.ps1)]

Verwenden der `-ProjectName` Option zum Installieren der Pakete anstelle der Windows Azure-Projekt das ASP.NET MVC-Projekt zur Verfügung.

## <a name="configure-the-backplane"></a>Konfigurieren der Rückwand

Fügen Sie in Ihrer Anwendung Global.asax-Datei den folgenden Code hinzu:

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample3.cs)]

Nun müssen Sie die Servicebus-Verbindungszeichenfolge abzurufen. Wählen Sie im Azure-Portal den Servicebus-Namespace, den Sie erstellt haben, und klicken Sie auf das Symbol "Zugriffsschlüssel".

![](scaleout-with-windows-azure-service-bus/_static/image6.png)

Kopieren Sie die Verbindungszeichenfolge in die Zwischenablage, und fügen Sie ihn in die *"ConnectionString"* Variable.

![](scaleout-with-windows-azure-service-bus/_static/image7.png)

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample4.cs)]

## <a name="deploy-to-azure"></a>In Azure bereitstellen

Erweitern Sie im Projektmappen-Explorer die **Rollen** Ordner im Projekt ChatService.

![](scaleout-with-windows-azure-service-bus/_static/image8.png)

Mit der rechten Maustaste in der Rolle "SignalRChat", und wählen Sie **Eigenschaften**. Wählen Sie die **Konfiguration** Registerkarte. Klicken Sie unter **Instanzen** 2 auswählen. Sie können auch die Größe des virtuellen Computers festlegen, um **sehr klein**.

![](scaleout-with-windows-azure-service-bus/_static/image9.png)

Speichern Sie die Änderungen.

Im Projektmappen-Explorer mit der rechten Maustaste des Projekts ChatService. Wählen Sie **Veröffentlichen**.

![](scaleout-with-windows-azure-service-bus/_static/image10.png)

Wenn dies Ihre erste Zeit Veröffentlichungsvorgang in Windows Azure ist, müssen Sie Ihre Anmeldeinformationen herunterladen. In der **veröffentlichen** -Assistenten klicken Sie auf "Sign in to download Credentials". Dadurch werden Sie aufgefordert, sich bei Windows Azure-Portal anmelden und eine Datei mit veröffentlichungseinstellungen herunterladen.

![](scaleout-with-windows-azure-service-bus/_static/image11.png)

Klicken Sie auf **Import** , und wählen Sie die heruntergeladene Datei mit den veröffentlichungseinstellungen.

Klicken Sie auf **Weiter**. In der **Veröffentlichungseinstellungen** Dialogfeld unter **Cloud-Dienst**, wählen Sie den Clouddienst, die Sie zuvor erstellt haben.

![](scaleout-with-windows-azure-service-bus/_static/image12.png)

Klicken Sie auf **Veröffentlichen**. Es dauert einige Minuten, bis die Anwendung bereitstellen und starten die virtuellen Computern.

Beim Ausführen der chatanwendung kommunizieren die Rolleninstanzen jetzt über Azure Service Bus mithilfe eines Service Bus-Themas. Ein Thema ist eine Warteschlange, die mehrere Abonnenten ermöglicht.

Der Rückwand erstellt automatisch das Thema und die Abonnements. Um das Abonnement und die Nachrichtenaktivität anzuzeigen, öffnen Sie das Azure Portal, wählen Sie den Service Bus-Namespace, und klicken Sie auf "Themen".

![](scaleout-with-windows-azure-service-bus/_static/image13.png)

Dafür können einige Minuten, bis die Nachrichtenaktivität im Dashboard angezeigt werden.

![](scaleout-with-windows-azure-service-bus/_static/image14.png)

SignalR verwaltet die Lebensdauer des Themas. Solange die Anwendung bereitgestellt wurde, versuchen Sie nicht manuell Themen löschen oder Ändern von Einstellungen für das Thema.
