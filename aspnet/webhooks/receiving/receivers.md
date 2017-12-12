---
uid: webhooks/receiving/receivers
title: "ASP.NET WebHooks Empfänger | Microsoft Docs"
author: rick-anderson
description: "ASP.NET WebHooks Empfänger"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 6cdea089-15b2-4732-8c68-921ca561a8f1
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: 8c42db4056dd7a6ef77c7bcbc0eca3b5bf7c87e9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-webhooks-receivers"></a>ASP.NET WebHooks Empfänger

Empfangen von WebHooks, hängt davon ab, die der Absender ist. In einigen Fällen sind zusätzliche Schritte, die einen WebHook, überprüfen, dass der Abonnent eine wirklich Überwachung registrieren. Einige WebHooks bieten ein Push-Pull-Modell, in dem die HTTP-POST-Anforderung nur einen Verweis auf die Informationen enthält, die dann ist separat abgerufen werden. Häufig variiert erheblich des Sicherheitsmodells.

Microsoft ASP.NET WebHooks dient sie einfacher und konsistenter für die von Ihrer API von Netzwerkdaten ohne viel Zeit zu ermitteln, wie eine bestimmte Variante WebHooks behandelt machen.

Ein WebHook Empfänger ist für akzeptieren, und überprüfen WebHooks anhand bestimmter Absender verantwortlich. Ein WebHook Empfänger kann eine beliebige Anzahl von WebHooks, jeweils über seine eigene Konfiguration unterstützen. Beispielsweise kann der Empfänger GitHub WebHook WebHooks, die von einer beliebigen Anzahl von GitHub-Repositorys akzeptieren.

## <a name="webhook-receiver-uris"></a>WebHook-Empfänger-URIs

Durch die Installation von Microsoft ASP.NET WebHooks erhalten Sie einen allgemeine WebHook-Controller, der WebHook-Anforderungen von einer begrenzten Anzahl von Diensten akzeptiert. Wenn eine Anforderung eingeht, wählt er den entsprechenden Empfänger, den Sie installiert haben, für die Behandlung bestimmten WebHook Absenders.

Der URI des diesem Controller ist die WebHook-URI, den Sie mit dem Dienst zu registrieren und das Format:

```
https://<host>/api/webhooks/incoming/<receiver>/{id}
```

Aus Gründen der Sicherheit vieler WebHook Empfänger erfordern, dass der URI ist ein *Https* URI und in einigen Fällen müssen sie auch eine zusätzliche Abfrageparameter dient zum erzwingen, dass nur die vorgesehene Partei WebHooks an den oben genannten URI senden kann enthalten .

Die  *<receiver>*  Komponente ist der Name des Empfängers, z. B. *Github* oder *Slack*.

Die *{Id}* ist ein optionaler Bezeichner, der verwendet werden kann, um eine bestimmte Konfiguration von WebHook Empfänger zu identifizieren. Dies kann verwendet werden, um N WebHooks mit einem bestimmten Empfänger zu registrieren. Beispielsweise kann die folgenden drei URIs verwendet werden, für die drei unabhängige WebHooks registrieren:

```
https://<host>/api/webhooks/incoming/github
https://<host>/api/webhooks/incoming/github/12345
https://<host>/api/webhooks/incoming/github/54321
```

## <a name="installing-a-webhook-receiver"></a>Installieren einen Empfänger WebHook

Zum Verwenden von Microsoft ASP.NET WebHooks WebHooks zu erhalten, installieren Sie zuerst das NuGet-Paket für den WebHook-Anbieter oder der Anbieter von WebHooks erhalten sollen. Die NuGet-Pakete werden mit dem Namen [Microsoft.AspNet.WebHooks.Receivers.*](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers) , in denen der letzte Teil gibt des Diensts unterstützt. Beispiel:

[Microsoft.AspNet.WebHooks.Receivers.GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) bietet Unterstützung für den Empfang von WebHooks von GitHub und [Microsoft.AspNet.WebHooks.Receivers.Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) bietet Unterstützung für den Empfang von WebHooks, die von ASP generiert. NET-WebHooks.

Direktes finden Sie Unterstützung für Dropbox, GitHub, MailChimp, PayPal, Pusher, Salesforce, Slack, Stripe, Trello und WordPress, aber es ist möglich, eine beliebige Anzahl von anderen Anbietern unterstützt.

## <a name="configuring-a-webhook-receiver"></a>Konfigurieren einen Empfänger WebHook

WebHook Empfänger werden konfiguriert, über die [IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) -Schnittstelle und bestimmte Implementierungen dieser Schnittstelle, können mithilfe von Dependency Injection-Modellen registriert werden. Die Standardimplementierung verwendet die Anwendungseinstellungen die kann entweder in der Datei "Web.config" festgelegt werden, oder bei Verwendung von Azure-Web-Apps können durch Festlegen der [Azure-Portal](https://portal.azure.com/).

![Azure-App-Einstellungen](_static/AzureAppSettings.png)

Das Format für die Anwendung Einstellungsschlüssel lautet wie folgt:

```
MS_WebHookReceiverSecret_<receiver>
```

Der Wert ist eine durch Trennzeichen getrennte Liste der Werte, die mit der *{Id}* Werte für die WebHooks, beispielsweise erfasst wurden:

```
MS_WebHookReceiverSecret_GitHub = <secret1>, 12345=<secret2>, 54321=<secret3>
```

## <a name="initializing-a-webhook-receiver"></a>Ein WebHook Empfänger initialisieren

WebHook Empfänger werden initialisiert, durch die Registrierung, in der Regel in der *"webapiconfig"* statische Klasse, zum Beispiel:

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
