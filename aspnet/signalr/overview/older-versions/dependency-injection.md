---
uid: signalr/overview/older-versions/dependency-injection
title: Abhängigkeitsinjektion in SignalR 1.x | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/15/2013
ms.topic: article
ms.assetid: eaa206c4-edb3-487e-8fcb-54a3261fed36
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 6fd155adc9a0aa71b66db7a51062a51fb0c1feca
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
ms.locfileid: "26505489"
---
<a name="dependency-injection-in-signalr-1x"></a>Abhängigkeitsinjektion in SignalR 1.x
====================
durch [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

Abhängigkeitsinjektion ist eine Möglichkeit zum Entfernen von hartcodierten Abhängigkeiten zwischen Objekten, die leichter auf ein Objekt Abhängigkeiten, entweder für das Testen (unter Verwendung von Pseudoobjekten) zu ersetzen oder Laufzeitverhalten zu ändern. In diesem Lernprogramm wird gezeigt, wie Abhängigkeitsinjektion für SignalR-Hubs ausgeführt wird. Es wird gezeigt, wie mit SignalR IoC-Container verwenden. Ein IoC-Container ist ein allgemeines Framework für zielabhängigkeit.

## <a name="what-is-dependency-injection"></a>Was ist die Abhängigkeitsinjektion?

Überspringen Sie diesen Abschnitt, wenn Sie bereits mit Abhängigkeitsinjektion vertraut sind.

*Abhängigkeitsinjektion* (DI) ist ein Muster, in denen sind Objekte nicht verantwortlich für das Erstellen ihrer eigenen Abhängigkeiten. Hier ist ein einfaches Beispiel zum DI angeregt. Nehmen Sie an, dass Sie ein Objekt haben, die benötigt werden, um Nachrichten zu protokollieren. Sie können eine protokollierungsschnittstelle definieren:

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

In Ihr Objekt erstellen Sie eine `ILogger` um Nachrichten zu protokollieren:

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

Dies funktioniert, aber es ist nicht zum optimalen Entwurf. Wenn Sie ersetzen möchten `FileLogger` mit einem anderen `ILogger` Implementierung müssen Sie ändern `SomeComponent`. Kommunale, die Verwendung auf viele andere Objekte `FileLogger`, müssen Sie alle von ihnen zu ändern. Oder wenn Sie sich entscheiden, stellen `FileLogger` ein Singleton, Sie müssen auch Änderungen in der gesamten Anwendung vornehmen.

Ein besserer Ansatz "Einfügen" ist ein `ILogger` in das Objekt – z. B. durch einen Konstruktor mit dem Argument:

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

Nachdem das Objekt nicht für die Auswahl der zuständig ist `ILogger` verwenden. Sie können Swich `ILogger` Implementierungen ohne Änderung der Objekte, die von ihm abhängig sind.

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

Dieses Muster wird aufgerufen, [Konstruktoreinfügung](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection). Ein anderes Muster ist Setter-Injection, in dem Sie die Abhängigkeit über einen Setter-Methode oder Eigenschaft festgelegt wird.

## <a name="simple-dependency-injection-in-signalr"></a>Einfache Abhängigkeitsinjektion in SignalR

Betrachten Sie die Chat-Anwendung aus dem Lernprogramm [erste Schritte mit SignalR](../getting-started/tutorial-getting-started-with-signalr.md). So sieht die hubklasse über diese Anwendung aus:

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

Nehmen Sie an, dass Sie Chat-Nachrichten auf dem Server speichern, vor dem senden möchten. Sie definieren eine Schnittstelle, die diese Funktionalität abstrahiert und DI zum Einfügen von der Schnittstelle für verwenden möglicherweise die `ChatHub` Klasse.

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

Das einzige Problem besteht darin, dass eine Anwendung SignalR Hubs nicht direkt erstellt; SignalR erstellt für Sie. Standardmäßig erwartet SignalR eine hubklasse über einen parameterlosen Konstruktor verfügt. Allerdings kann problemlos eine Funktion zur Erstellung der hubinstanzen registrieren, und mit dieser Funktion können DI ausführen. Registrieren Sie die Funktion durch Aufrufen von **GlobalHost.DependencyResolver.Register**.

[!code-csharp[Main](dependency-injection/samples/sample7.cs)]

Jetzt SignalR dieser anonymen Funktion immer dann wird wird benötigt aufgerufen, um erstellen eine `ChatHub` Instanz.

## <a name="ioc-containers"></a>IoC-Container

Der vorhergehende Code ist in einfachen Fällen gut. Aber weiterhin mussten Sie Folgendes schreiben:

[!code-css[Main](dependency-injection/samples/sample8.css)]

In einer komplexen Anwendung mit vielen Abhängigkeiten müssen Sie möglicherweise ein Großteil dieser "verknüpfen" Code schreiben. Dieser Code kann schwierig zu verwalten sein, insbesondere dann, wenn Abhängigkeiten geschachtelt sind. Es ist auch schwer zu Komponententest.

Eine Lösung besteht darin einen IoC-Container zu verwenden. Ein IoC-Container ist eine Softwarekomponente, die zum Verwalten von Abhängigkeiten zuständig ist. Registrieren von Typen mit dem Container, und klicken Sie dann den Container verwenden, um Objekte zu erstellen. Der Container ermittelt automatisch die Abhängigkeit Beziehungen. Viele IoC-Container können Sie steuern, z. B. Objektlebensdauer und Bereich.

> [!NOTE]
> "IoC" steht für "Inversion des Steuerelements" Dies ist ein allgemeines Muster, in denen ein Framework Anwendungscode aufruft. Ein IoC-Container erstellt die Objekte, die "die übliche ablaufsteuerung kehrt".


## <a name="using-ioc-containers-in-signalr"></a>Mithilfe von IoC-Container in SignalR

Chat-Anwendung ist wahrscheinlich zu einfach einen IoC-Container profitieren. Stattdessen sehen wir uns die [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) Beispiel.

Im Beispiel StockTicker definiert die beiden Hauptklassen:

- `StockTickerHub`: Der hubklasse, die Clientverbindungen verwaltet.
- `StockTicker`: Ein Singleton, der Aktienkurse enthält und in regelmäßigen Abständen aktualisiert werden.

`StockTickerHub`enthält einen Verweis auf die `StockTicker` Singleton, während `StockTicker` enthält einen Verweis auf die **IHubConnectionContext** für die `StockTickerHub`. Er verwendet diese Schnittstelle für die Kommunikation mit `StockTickerHub` Instanzen. (Weitere Informationen finden Sie unter [Broadcast-Server mit ASP.NET SignalR](index.md).)

Wir können einen IoC-Container verwenden, um diese Abhängigkeiten etwas beurteilt. Zunächst sehen wir vereinfachen die `StockTickerHub` und `StockTicker` Klassen. Im folgenden Code habe ich die Teile auskommentiert, dass wir nicht brauchen.

Entfernen des parameterlosen Konstruktors von `StockTicker`. Es wird stattdessen immer DI verwenden, um den Hub zu erstellen.

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

StockTicker entfernen Sie die Singleton-Instanz. Später verwenden wir den IoC-Container zur Steuerung der Lebensdauer StockTicker. Darüber hinaus öffentlich des Konstruktors.

[!code-csharp[Main](dependency-injection/samples/sample10.cs?highlight=7)]

Als Nächstes können wir den Code Umgestalten, erstellen Sie eine Schnittstelle für `StockTicker`. Wir verwenden diese Schnittstelle zum Entkoppeln der `StockTickerHub` aus der `StockTicker` Klasse.

Visual Studio legt diese Art von Umgestaltung einfach. Öffnen Sie die Datei StockTicker.cs der rechten Maustaste auf die `StockTicker` Klassendeklaration, und wählen Sie **Umgestalten** ... **Schnittstelle extrahieren**.

![](dependency-injection/_static/image1.png)

In der **Schnittstelle extrahieren** Dialogfeld klicken Sie auf **Alles markieren**. Lassen Sie die anderen Standardwerte. Klicken Sie auf **OK**.

![](dependency-injection/_static/image2.png)

Visual Studio erstellt eine neue Schnittstelle namens `IStockTicker`, und ändert sich auch `StockTicker` Ableitung `IStockTicker`.

Öffnen Sie die Datei IStockTicker.cs und ändern Sie die Schnittstelle zum **öffentlichen**.

[!code-csharp[Main](dependency-injection/samples/sample11.cs?highlight=1)]

In der `StockTickerHub` Klasse, ändern Sie die zwei Instanzen des `StockTicker` auf `IStockTicker`:

[!code-csharp[Main](dependency-injection/samples/sample12.cs?highlight=4,6)]

Erstellen einer `IStockTicker` Schnittstelle nicht unbedingt erforderlich ist, aber ich soll demonstriert werden, wie DI helfen kann, um Kopplung zwischen Komponenten in Ihrer Anwendung zu reduzieren.

## <a name="add-the-ninject-library"></a>Fügen Sie der Bibliothek Ninject hinzu

Es gibt viele Open Source-IoC-Container für .NET. Für dieses Lernprogramm verwende ich [Ninject](http://www.ninject.org/). (Andere beliebten Bibliotheken enthalten [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), und [StructureMap ](http://docs.structuremap.net).)

Verwenden Sie NuGet Package Manager zum Installieren der [Ninject Bibliothek](https://nuget.org/packages/Ninject/3.0.1.10). In Visual Studio aus der **Tools** Menü die Option **Bibliothekspaket-Manager** | **Package Manager Console**. Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl aus:

[!code-powershell[Main](dependency-injection/samples/sample13.ps1)]

## <a name="replace-the-signalr-dependency-resolver"></a>Ersetzen Sie den Abhängigkeitskonfliktlöser für SignalR

Um Ninject in SignalR verwenden zu können, erstellen Sie eine Klasse, die abgeleitet **DefaultDependencyResolver**.

[!code-csharp[Main](dependency-injection/samples/sample14.cs)]

Diese Klasse überschreibt die **GetService** und **"GetServices" auf** Methoden der **DefaultDependencyResolver**. SignalR ruft diese Methoden zum Erstellen von verschiedenen Objekten zur Laufzeit, einschließlich hubinstanzen sowie verschiedene Dienste, die intern vom SignalR verwendet.

- Die **GetService** Methode erstellt eine einzelne Instanz eines Typs. Überschreiben Sie diese Methode zum Aufrufen des Ninject Kernels **TryGet** Methode. Wenn diese Methode null zurückgibt, ein Fallback auf den Standardkonfliktlöser.
- Die **"GetServices" auf** Methode erstellt eine Auflistung von Objekten eines angegebenen Typs. Überschreiben Sie diese Methode, um die Ergebnisse von Ninject mit den Ergebnissen aus den Standardkonfliktlöser verketten.

## <a name="configure-ninject-bindings"></a>Ninject Bindungen konfigurieren

Jetzt müssen wir Ninject verwenden, um die Bindungen des Typs deklarieren.

Öffnen Sie die Datei RegisterHubs.cs. In der `RegisterHubs.Start` -Methode, erstellen Sie den Ninject-Container, der aufgerufen wird, Ninject der *Kernel*.

[!code-csharp[Main](dependency-injection/samples/sample15.cs)]

Erstellen Sie eine Instanz des unsere benutzerdefinierte Abhängigkeitskonfliktlöser:

[!code-csharp[Main](dependency-injection/samples/sample16.cs)]

Erstellen Sie eine Bindung für `IStockTicker` wie folgt:

[!code-html[Main](dependency-injection/samples/sample17.html)]

Dieser Code ist zwei Dinge darauf hingewiesen werden. Erste, wenn die Anwendung muss eine `IStockTicker`, Kernel sollten erstellen Sie eine Instanz des `StockTicker`. Zweitens stellt die `StockTicker` sollten Klasse als Singleton-Objekt erstellt werden. Ninject erstellt eine Instanz des Objekts, und die gleiche Instanz für jede Anforderung zurück.

Erstellen Sie eine Bindung für **IHubConnectionContext** wie folgt:

[!code-csharp[Main](dependency-injection/samples/sample18.cs)]

Dieser Code Creatres einer anonymen Funktion, die gibt eine **IHubConnection**. Die **WhenInjectedInto** Methode teilt Ninject diese Funktion verwenden, nur beim Erstellen `IStockTicker` Instanzen. Der Grund ist, dass SignalR erstellt **IHubConnectionContext** Instanzen intern, und wir möchten nicht überschreiben, wie Sie SignalR erstellt. Diese Funktion gilt nur für unsere `StockTicker` Klasse.

Übergeben Sie den Abhängigkeitskonfliktlöser in der **MapHubs** Methode:

[!code-csharp[Main](dependency-injection/samples/sample19.cs)]

Nun SignalR verwendet die im angegebenen Konfliktlösers **MapHubs**, anstatt des Standardkonfliktlösers.

Hier wird die vollständige codeauflistung für `RegisterHubs.Start`.

[!code-csharp[Main](dependency-injection/samples/sample20.cs)]

Drücken Sie F5, um die StockTicker-Anwendung in Visual Studio auszuführen. Wechseln Sie in das Browserfenster zu `http://localhost:*port*/SignalR.Sample/StockTicker.html`.

![](dependency-injection/_static/image3.png)

Die Anwendung muss genau die gleiche Funktionalität wie vor. (Eine Beschreibung finden Sie unter [Broadcast-Server mit ASP.NET SignalR](index.md).) Wir noch nicht das Verhalten geändert werden. nur vereinfacht den Code testen, verwalten und entwickeln.
