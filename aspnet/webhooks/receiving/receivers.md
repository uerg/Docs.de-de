---
uid: webhooks/receiving/receivers
title: ASP.NET WebHooks Empfänger | Microsoft-Dokumentation
author: rick-anderson
description: ASP.NET WebHooks Empfänger
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 6cdea089-15b2-4732-8c68-921ca561a8f1
ms.technology: ''
ms.openlocfilehash: be1f7dcef60642231af9c593eb7329ede7c2094f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374742"
---
# <a name="aspnet-webhooks-receivers"></a>ASP.NET WebHooks Empfänger

Empfangen von WebHooks, hängt davon ab, die der Absender ist. Manchmal sind zusätzliche Schritte, die einen WebHook, überprüfen, dass der Abonnent wirklich eine Überwachung zu registrieren. Einige WebHooks Geben Sie ein Push-Pull-Modell, in denen die HTTP-POST-Anforderung nur einen Verweis auf die Informationen enthält, klicken Sie dann mit separat abgerufen werden. Häufig variiert ein wenig des Sicherheitsmodells.

Microsoft ASP.NET WebHooks dient sowohl einfacher und konsistenter, um Ihre API zu verknüpfen, ohne viel Zeit, um herauszufinden, wie eine bestimmte Variante von WebHooks behandelt.

Webhookempfänger ist verantwortlich für das akzeptieren und WebHooks von einem bestimmten Absender überprüfen. Webhookempfänger kann eine beliebige Anzahl von WebHooks, mit jeweils eigener Konfiguration unterstützen. Beispielsweise kann der Empfänger des GitHub-WebHook WebHooks von eine beliebige Anzahl von GitHub-Repositorys akzeptieren.

## <a name="webhook-receiver-uris"></a>WebHook-Empfänger-URIs

Durch die Installation von Microsoft ASP.NET WebHooks erhalten Sie einen allgemeinen WebHook-Controller, der WebHook-Anforderungen über eine offene Anzahl von Services akzeptiert. Wenn eine Anforderung eingeht, wählt er den entsprechenden Empfänger, den Sie installiert haben, für die Behandlung von eines bestimmten Absenders für den WebHook.

Der URI des diesem Controller ist der WebHook-URI, den Sie mit dem Dienst zu registrieren und hat folgendes Format:

```
https://<host>/api/webhooks/incoming/<receiver>/{id}
```

Aus Gründen der Sicherheit erfordern viele WebHook-Empfänger, dass der URI eine *Https* URI und in einigen Fällen müssen sie auch einen zusätzliche Abfragezeichenfolgen-Parameter dient zum erzwingen, dass nur die vorgesehene Partei WebHooks an den oben aufgeführten URI senden kann enthalten .

Die <em> <receiver> </em> Komponente ist der Name des Empfängers, z. B. <em>Github</em> oder <em>Slack</em>.

Die *{Id}* ist ein optionaler Bezeichner, die verwendet werden kann, um eine bestimmte Konfiguration des WebHook-Empfänger zu identifizieren. Dies kann verwendet werden, um N WebHooks mit einem bestimmten Empfänger zu registrieren. Beispielsweise kann die folgenden drei URIs verwendet werden, für die drei unabhängige WebHooks registrieren:

```
https://<host>/api/webhooks/incoming/github
https://<host>/api/webhooks/incoming/github/12345
https://<host>/api/webhooks/incoming/github/54321
```

## <a name="installing-a-webhook-receiver"></a>Installieren einen WebHook-Empfänger

Um mithilfe von Microsoft ASP.NET WebHooks WebHooks zu erhalten, installieren Sie zunächst das Nuget-Paket für die WebHook-Anbieter oder der Anbieter, denen Sie WebHooks von erhalten möchten. Die Nuget-Pakete werden mit dem Namen [Microsoft.AspNet.WebHooks.Receivers.*](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers) , in denen der letzte Teil gibt des Diensts unterstützt. Beispiel:

[Microsoft.AspNet.WebHooks.Receivers.GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) bietet Unterstützung für den Empfang von WebHooks über GitHub und [Microsoft.AspNet.WebHooks.Receivers.Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) bietet Unterstützung für den Empfang von WebHooks, die von ASP generiert. NET-WebHooks.

Standardmäßig finden Sie Unterstützung für Dropbox, GitHub, MailChimp, PayPal, Pusher, Salesforce, Slack, Stripe, Trello und WordPress, aber es ist möglich, eine beliebige Anzahl von anderen Anbietern unterstützt.

## <a name="configuring-a-webhook-receiver"></a>Konfigurieren einen WebHook-Empfänger

WebHook-Empfänger konfiguriert sind, über die [IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) -Schnittstelle und bestimmte Implementierungen dieser Schnittstelle registriert werden können mit jedem Dependency Injection-Modell. Die Standardimplementierung verwendet die Einstellungen für die Anwendung kann entweder in der Datei "Web.config" festgelegt werden, oder bei Verwendung von Azure-Web-Apps über festgelegt werden können die [Azure-Portal](https://portal.azure.com/).

![Azure-App-Einstellungen](_static/AzureAppSettings.png)

Das Format für die Schlüssel der Anwendungseinstellung lautet wie folgt aus:

```
MS_WebHookReceiverSecret_<receiver>
```

Der Wert ist eine durch Trennzeichen getrennte Liste von Werten der *{Id}* Werte, die für die WebHooks, z.B. erfasst wurden:

```
MS_WebHookReceiverSecret_GitHub = <secret1>, 12345=<secret2>, 54321=<secret3>
```

## <a name="initializing-a-webhook-receiver"></a>Initialisieren einen WebHook-Empfänger

WebHook-Empfänger werden initialisiert, durch die Registrierung, in der Regel in der *WebApiConfig* statische Klasse, zum Beispiel:

```csharp
namespace WebHookReceivers
{
    public static class WebApiConfig
    {
        public static void Register(HttpConfiguration config)
        {
            // Web API configuration and services

            // Web API routes
            config.MapHttpAttributeRoutes();

            config.Routes.MapHttpRoute(
                name: "DefaultApi",
                routeTemplate: "api/{controller}/{id}",
                defaults: new { id = RouteParameter.Optional }
            );

            // Load receivers
            config.InitializeReceiveGitHubWebHooks();
        }
    }
}
```
