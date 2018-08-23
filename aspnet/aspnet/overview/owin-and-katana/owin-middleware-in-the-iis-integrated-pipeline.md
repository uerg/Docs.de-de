---
uid: aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline
title: OWIN-Middleware in der IIS integrierte Pipeline | Microsoft-Dokumentation
author: Praburaj
description: In diesem Artikel zeigt, wie zum Ausführen von OWIN-Middleware-Komponenten (OMCs) in der integrierten IIS-Pipeline, und wie der Pipeline Ereignissatz ein OMC ausgeführt wird. Sie sollten...
ms.author: riande
ms.date: 11/07/2013
ms.assetid: d031c021-33c2-45a5-bf9f-98f8fa78c2ab
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline
msc.type: authoredcontent
ms.openlocfilehash: 56bd145688e1ab0a70710cc80cb8f5fa10ee8968
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826782"
---
<a name="owin-middleware-in-the-iis-integrated-pipeline"></a>OWIN-Middleware in der integrierten IIS-pipeline
====================
durch [Praburaj Thiagarajan](https://github.com/Praburaj), [Rick Anderson](https://github.com/Rick-Anderson)

> In diesem Artikel zeigt, wie zum Ausführen von OWIN-Middleware-Komponenten (OMCs) in der integrierten IIS-Pipeline, und wie der Pipeline Ereignissatz ein OMC ausgeführt wird. Sie sollten überprüfen, [eine Übersicht des Katana-Projekt](an-overview-of-project-katana.md) und [OWIN-Startup-Klasse Erkennung](owin-startup-class-detection.md) vor dem Lesen in diesem Tutorial. In diesem Tutorial wurde von Rick Anderson geschrieben ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ), Chris Ross und Praburaj Thiagarajan Howard Dierking ( [ @howard \_Dierking](https://twitter.com/howard_dierking) ).


Obwohl [OWIN](an-overview-of-project-katana.md) Middleware-Komponenten (OMCs) dienen in erster Linie in einer Pipeline Unabhängigkeit von der Server ausgeführt, es ist möglich, führen Sie in der integrierten IIS-Pipeline sowie eine OMC (**wird im klassischen Modus *nicht* unterstützt**). Ein OMC kann vorgenommen werden, in der integrierten IIS-Pipeline zu arbeiten, installieren Sie das folgende Paket aus dem Paket-Manager-Konsole (PMC):

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample1.cmd)]

Dies bedeutet, dass alle Anwendungsframeworks, auch solche, die noch nicht ausführen außerhalb von IIS und "System.Web", sind vorhandene OWIN-middlewarekomponenten profitieren können. 

> [!NOTE]
> Alle der `Microsoft.Owin.Security.*` Auslieferung mit der neuen Identität-System in Visual Studio 2013-Pakete (z. B.: Cookies, Microsoft Account, Google, Facebook, Twitter, [Bearertoken](http://self-issued.info/docs/draft-ietf-oauth-v2-bearer.html), OAuth, autorisierungsserver, JWT, Azure Active Directory und Active Directory-Verbunddienste) werden als OMCs erstellt und können in selbstgehostete und IIS-gehosteten Szenarien verwendet werden.

## <a name="how-owin-middleware-executes-in-the-iis-integrated-pipeline"></a>Wie der OWIN-Middleware in der integrierten IIS-Pipeline ausgeführt wird

Für konsolenanwendungen OWIN-Pipeline der Anwendung erstellt mithilfe der [Startkonfiguration](owin-startup-class-detection.md) wird festgelegt, indem die Reihenfolge die Komponenten hinzugefügt werden, mithilfe der `IAppBuilder.Use` Methode. D. h. die OWIN-Pipeline in der [Katana](an-overview-of-project-katana.md) Runtime verarbeitet OMCs in der Reihenfolge, die sie registriert wurden, mithilfe von `IAppBuilder.Use`. In der integrierten IIS-Pipeline die Anforderungspipeline besteht aus [HttpModules](https://msdn.microsoft.com/library/ms178468(v=vs.85).aspx) einen vordefinierten Satz von der Pipelineereignisse abonniert sind, z. B. [BeginRequest](https://msdn.microsoft.com/library/system.web.httpapplication.beginrequest.aspx), [AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx), [AuthorizeRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx)usw.

Wenn wir eine OMC mit Vergleichen einer [HttpModule](https://msdn.microsoft.com/library/zec9k340(v=vs.85).aspx) in der Welt ASP.NET eine OMC muss mit dem richtigen vordefinierten Pipelineereignis registriert werden. Z. B. das HttpModule `MyModule` wird abrufen aufgerufen, wenn eine Anforderung, um eingeht die [AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx) Phase in der Pipeline:

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample2.cs?highlight=10)]

In der Reihenfolge für eine OMC zur Teilnahme an diesem gleichen, die das ereignisbasierte Ausführungsreihenfolge, die [Katana](an-overview-of-project-katana.md) Laufzeitcode durchsucht die [Startkonfiguration](owin-startup-class-detection.md) und jede der middlewarekomponenten zum abonniert ein integrierte Pipeline-Ereignis. Beispielsweise können mit der folgende Code der OMC und registrieren Sie die Standard-ereignisregistrierung Middleware-Komponenten finden Sie unter. (Weitere Informationen zum Erstellen einer OWIN-Startklasse finden Sie unter [OWIN-Startup-Klasse Erkennung](owin-startup-class-detection.md).)

1. Erstellen Sie eine leere Web-Anwendungsprojekt, und nennen Sie sie **owin2**.
2. Führen Sie über die Paket-Manager-Konsole (PMC), den folgenden Befehl aus: 

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample3.cmd)]
3. Hinzufügen einer `OWIN Startup Class` und nennen Sie sie `Startup`. Ersetzen Sie den generierten Code durch den folgenden (die Änderungen sind hervorgehoben):  

    [!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample4.cs?highlight=5-7,15-36)]
4. Drücken Sie F5, um die app auszuführen.

Einrichten einer Pipeline mit drei Middleware-Komponenten, die ersten beiden zum Anzeigen von Diagnoseinformationen und der letzte Suchvorgang reagieren auf Ereignisse (und auch Anzeige von Diagnoseinformationen) legt die Start-Konfiguration fest. Die `PrintCurrentIntegratedPipelineStage` -Methode der integrierten Pipeline-Ereignis, das für diese Middleware aufgerufen wird und eine Meldung angezeigt. Das Ausgabefenster zeigt Folgendes an:

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample5.cmd)]

Die Katana-Laufzeit zugeordnet wird jede der OWIN-Middleware-Komponenten für [PreExecuteRequestHandler](https://msdn.microsoft.com/library/system.web.httpapplication.prerequesthandlerexecute.aspx) entspricht standardmäßig dem IIS-Pipeline-Ereignis [PreRequestHandlerExecute](https://msdn.microsoft.com/library/system.web.httpapplication.prerequesthandlerexecute.aspx).

## <a name="stage-markers"></a>Phase Marker

Können Sie kennzeichnen, OMCs auszuführende in bestimmten Phasen der Pipeline mithilfe der `IAppBuilder UseStageMarker()` -Erweiterungsmethode. Führen Sie eine Reihe von middlewarekomponenten in einer bestimmten Phase, fügen Sie eine phasenmarkierung direkt nach der letzten Komponente ist ein Satz während der Registrierung. Sind Regeln, die auf die Phase der Pipeline Sie, Middleware ausführen können und müssen die Komponenten ausführen (später in diesem Tutorial werden die Regeln erläutert). Hinzufügen der `UseStageMarker` Methode, um die `Configuration` code wie folgt:

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample6.cs?highlight=13,19)]

Die `app.UseStageMarker(PipelineStage.Authenticate)` Aufruf so konfiguriert, dass alle zuvor registrierten middlewarekomponenten (in diesem Fall unsere zwei diagnostischen Komponenten) auf die Authentifizierungsphase der Pipeline ausgeführt werden. Die letzte middlewarekomponente (die Diagnose angezeigt, und auf Anforderungen antwortet) wird ausgeführt, auf die `ResolveCache` Stufe (der [ResolveRequestCache](https://msdn.microsoft.com/library/system.web.httpapplication.resolverequestcache.aspx) Ereignis).

Drücken Sie F5, um die app auszuführen. Das Fenster "Ausgabe" zeigt Folgendes:

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample7.cmd)]

## <a name="stage-marker-rules"></a>Phase Marker-Regeln

Owin-Middleware-Komponenten (OMC) können konfiguriert werden, wenn Sie die folgenden Phase Ereignisse für die OWIN-Pipeline ausführen:

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample8.cs)]

1. In der Standardeinstellung OMCs, führen Sie an das letzte Ereignis (`PreHandlerExecute`). Daher unsere erste Beispielcode angezeigt "PreExecuteRequestHandler".
2. Können Sie die einem `app.UseStageMarker` Methode, um eine OMC oben in jedem Stadium der OWIN-Pipeline ausführen registrieren aufgeführt, der `PipelineStage` Enum.
3. Die OWIN-Pipeline und die IIS-Pipeline sortiert wurde, daher Aufrufe `app.UseStageMarker` muss in der Reihenfolge. Können Sie den Ereignishandler auf ein Ereignis, das vor das letzte Ereignis, das mit registriert festlegen `app.UseStageMarker`. Z. B. *nach* aufrufen:

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample9.cmd)]

   Aufrufe von `app.UseStageMarker` übergeben `Authenticate` oder `PostAuthenticate` werden nicht berücksichtigt, und es wird keine Ausnahme ausgelöst werden. OMCs führen Sie in der aktuellen Phase, die in der Standardeinstellung ist `PreHandlerExecute`. Marker aus Phase stellen sie zuvor ausgeführt wird. Wenn Sie die Phase Marker außerhalb der Reihenfolge angeben, runden es auf den früheren Marker. Das heißt, sagt eine phasenmarkierung hinzufügen "Nicht später als Phase X ausführen". Führen Sie die OMC der frühesten Stufe Markierung hinzugefügt, nachdem sie in der OWIN-Pipeline.
4. Der frühesten Stufe Aufrufe `app.UseStageMarker` Wins. Wenn Sie die Reihenfolge der wechseln, z. B. `app.UseStageMarker` Aufrufe aus unserem vorherigen Beispiel:

    [!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample10.cs?highlight=13,19)]

   Das Fenster "Ausgabe" wird angezeigt: 

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample11.cmd)]

   Die OMCs jeweils ausgeführt, in der `AuthenticateRequest` bereitstellen, da bei der letzten OMC registriert die `Authenticate` Ereignis und die `Authenticate` Ereignis vorangestellt ist, alle anderen Ereignisse.
