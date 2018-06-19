---
uid: aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline
title: OWIN-Middleware in der IIS-integrierten Pipeline | Microsoft Docs
author: Praburaj
description: Dieser Artikel zeigt, wie zum Ausführen von owin-Middleware-Komponenten (OMCs) in der integrierten IIS-Pipeline und zum Festlegen von der Pipeline-Ereignis ein OMC auf ausgeführt. Sie sollten...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2013
ms.topic: article
ms.assetid: d031c021-33c2-45a5-bf9f-98f8fa78c2ab
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline
msc.type: authoredcontent
ms.openlocfilehash: 5df70c80084a32c5f61ac9288c8cdbfaaa47f124
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871492"
---
<a name="owin-middleware-in-the-iis-integrated-pipeline"></a>OWIN-Middleware in der integrierten IIS-pipeline
====================
durch [Praburaj Thiagarajan](https://github.com/Praburaj), [Rick Anderson](https://github.com/Rick-Anderson)

> Dieser Artikel zeigt, wie zum Ausführen von owin-Middleware-Komponenten (OMCs) in der integrierten IIS-Pipeline und zum Festlegen von der Pipeline-Ereignis ein OMC auf ausgeführt. Lesen Sie [eine Übersicht über Project Katana](an-overview-of-project-katana.md) und [OWIN Start Klasse Erkennung](owin-startup-class-detection.md) vor dem Lesen dieses Lernprogramms. Dieses Lernprogramm wurde von Rick Anderson geschrieben ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ), Chris Ross Praburaj Thiagarajan und Howard Dierking ( [ @howard \_Dierking](https://twitter.com/howard_dierking) ).


Obwohl [OWIN](an-overview-of-project-katana.md) Middleware-Komponenten (OMCs) dienen in erster Linie in einer Pipeline Unabhängigkeit von der Server ausgeführt, es ist möglich, eine OMC in der integrierten IIS-Pipeline ebenfalls ausgeführt (**im klassischer Modus ist *nicht* unterstützt**). Ein OMC kann vorgenommen werden, in der integrierten IIS-Pipeline verwendet, installieren Sie das folgende Paket aus der Paket-Manager-Konsole (PMC):

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample1.cmd)]

Dies bedeutet, dass alle Anwendungsframeworks, auch solche, die noch nicht außerhalb von IIS und System.Web, ausgeführt werden, die von vorhandenen owin-Middleware Komponenten profitieren können. 

> [!NOTE]
> Alle von der `Microsoft.Owin.Security.*` Pakete Auslieferung mit der neuen Identitätssystem in Visual Studio 2013 (z. B.: Cookies, Microsoft Account, Google, Facebook, Twitter, [Trägertoken](http://self-issued.info/docs/draft-ietf-oauth-v2-bearer.html), OAuth, autorisierungsserver, JWT, Azure Active Directory und Active Directory-Verbunddienste) als OMCs erstellt wurden, und in selbst gehostet und IIS-gehosteten Szenarien verwendet werden können.

## <a name="how-owin-middleware-executes-in-the-iis-integrated-pipeline"></a>Wie der OWIN-Middleware in der integrierten IIS-Pipeline ausgeführt wird

Für die OWIN-konsolenanwendungen, mit der Pipeline der Anwendung erstellt die [Startkonfiguration](owin-startup-class-detection.md) wird festgelegt, indem die Reihenfolge der Komponenten hinzugefügt werden, mit der `IAppBuilder.Use` Methode. D. h. die OWIN-Pipeline in der [Katana](an-overview-of-project-katana.md) Laufzeit verarbeitet OMCs in der Reihenfolge, die sie registriert wurden, mithilfe von `IAppBuilder.Use`. In der integrierten IIS-Pipeline besteht die Anforderungspipeline [HttpModules](https://msdn.microsoft.com/library/ms178468(v=vs.85).aspx) wie z. B. auf einen vordefinierten Satz von der Pipelineereignisse abonniert [BeginRequest](https://msdn.microsoft.com/library/system.web.httpapplication.beginrequest.aspx), [AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx), [AuthorizeRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx)usw.

Wenn wir eine OMC mit Vergleichen einer [HttpModule](https://msdn.microsoft.com/library/zec9k340(v=vs.85).aspx) in der Welt ASP.NET muss ein OMC an das richtige vordefinierten Pipeline-Ereignis registriert werden. Beispielsweise HttpModule `MyModule` wird abrufen aufgerufen, wenn eine Anforderung, um eingeht die [AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx) Stufe der Pipeline:

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample2.cs?highlight=10)]

In der Reihenfolge für eine OMC zur Teilnahme an dieser Anordnung derselben, das eine ereignisbasierte Ausführung der [Katana](an-overview-of-project-katana.md) Laufzeitcode eingeschränkten Bereichsscans mithilfe von der [Startkonfiguration](owin-startup-class-detection.md) und abonniert die Middleware-Komponenten auf eine Ereignis der integrierten Pipeline. Beispielsweise können mit der folgende Code der OMC und Registrierung für die Standard-ereignisregistrierung, Middleware-Komponenten finden Sie unter. (Ausführlichere Informationen zum Erstellen einer OWIN-Startklasse finden Sie unter [OWIN Start Klasse Erkennung](owin-startup-class-detection.md).)

1. Erstellen Sie ein leere Webanwendungsprojekt, und nennen Sie sie **owin2**.
2. Führen Sie aus der Paket-Manager-Konsole (PMC) den folgenden Befehl: 

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample3.cmd)]
3. Hinzufügen einer `OWIN Startup Class` und nennen Sie sie `Startup`. Ersetzen Sie den generierten Code mit den folgenden (die Änderungen werden hervorgehoben):  

    [!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample4.cs?highlight=5-7,15-36)]
4. Drücken Sie F5, um die app auszuführen.

Die Konfiguration für den Dienststart richtet eine Pipeline mit drei middlewarekomponenten gemeinsam sind, die ersten beiden zum Anzeigen von Diagnoseinformationen und zum letzten Blatt reagieren auf Ereignisse (und auch anzeigen von Diagnoseinformationen). Die `PrintCurrentIntegratedPipelineStage` -Methode der integrierten Pipeline-Ereignis für diese Middleware aufgerufen wird und eine Meldung angezeigt. Das Ausgabefenster zeigt Folgendes an:

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample5.cmd)]

Die Katana-Laufzeit zugeordnet aller der OWIN-Middleware zu Komponenten [PreExecuteRequestHandler](https://msdn.microsoft.com/library/system.web.httpapplication.prerequesthandlerexecute.aspx) standardmäßig entspricht der IIS-Ereignis-Pipeline [PreRequestHandlerExecute](https://msdn.microsoft.com/library/system.web.httpapplication.prerequesthandlerexecute.aspx).

## <a name="stage-markers"></a>Phase-Marker

Können Sie kennzeichnen, OMCs auszuführende in bestimmten Phasen der Pipeline mithilfe der `IAppBuilder UseStageMarker()` -Erweiterungsmethode. Um eine Reihe von Komponenten der Middleware während einer bestimmten Stufe ausgeführt wurde, fügen eine phasenmarkierung direkt nach der letzten Komponente ist ein Satz während der Registrierung. Es sind Regeln, in welcher Phase der Pipeline können Sie die Middleware ausführen, und die Order-Komponenten ausgeführt werden müssen (später in diesem Lernprogramm werden die Regeln erläutert). Hinzufügen der `UseStageMarker` Methode, um die `Configuration` code wie unten dargestellt:

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample6.cs?highlight=13,19)]

Die `app.UseStageMarker(PipelineStage.Authenticate)` Aufruf so konfiguriert, dass alle zuvor registrierten Middleware-Komponenten (in diesem Fall unsere zwei diagnostischen Komponenten) auf die Authentifizierungsphase der Pipeline ausgeführt werden. Die letzte middlewarekomponente (der Diagnose angezeigt und auf Anforderungen antwortet) wird ausgeführt, auf die `ResolveCache` Stufe (der [ResolveRequestCache](https://msdn.microsoft.com/library/system.web.httpapplication.resolverequestcache.aspx) Ereignis).

Drücken Sie F5, um die app auszuführen. Das Fenster "Ausgabe" zeigt Folgendes:

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample7.cmd)]

## <a name="stage-marker-rules"></a>Phase Marker Regeln

Owin-Middleware-Komponenten (OMC) können zum Ausführen von auf owin-Pipeline-Phase Folgendes konfiguriert werden:

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample8.cs)]

1. Standardmäßig OMCs, führen Sie an das letzte Ereignis (`PreHandlerExecute`). Deshalb ist unsere erste Beispielcode angezeigt "PreExecuteRequestHandler".
2. Können Sie die einer `app.UseStageMarker` Methode, um eine OMC früher in keiner Phase der OWIN-Pipeline ausgeführt registrieren aufgeführt, der `PipelineStage` Enum.
3. Die OWIN-Pipeline und die IIS-Pipeline ist sortiert, daher Aufrufe `app.UseStageMarker` muss in der Reihenfolge. Den Ereignishandler kann nicht festgelegt werden, um ein Ereignis, das vor dem letzten Ereignis registriert mit `app.UseStageMarker`. Beispielsweise *nach* aufrufen:

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample9.cmd)]

   Aufrufe von `app.UseStageMarker` übergeben `Authenticate` oder `PostAuthenticate` nicht berücksichtigt werden, und es wird keine Ausnahme ausgelöst. Führen Sie in der letzten Phase, dies ist standardmäßig OMCs `PreHandlerExecute`. Phase Marker werden verwendet, damit sie zuvor ausgeführt werden können. Wenn Sie die Phase Marker außerhalb der Reihenfolge angeben, runden wir auf den früheren Marker. Hinzufügen eine phasenmarkierung sagt also "Phase X oder höher ausführen". Bei den frühesten phasenmarkierung hinzugefügt, nachdem sie in der OWIN-Pipeline des OMC-ausführen.
4. Der frühesten Stufe der Aufrufe an `app.UseStageMarker` Wins. Angenommen, wenn Sie die Reihenfolge der wechseln `app.UseStageMarker` Aufrufe aus unserer vorherigen Beispiel:

    [!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample10.cs?highlight=13,19)]

   Das Fenster "Ausgabe" wird angezeigt: 

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample11.cmd)]

   Die OMCs alle ausgeführt, in der `AuthenticateRequest` bereitstellen, da der letzte OMC registriert die `Authenticate` -Ereignis und die `Authenticate` Ereignis vorangeht, alle anderen Ereignisse.
