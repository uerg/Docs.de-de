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
# <a name="aspnet-webhooks-receivers"></a><span data-ttu-id="49126-103">ASP.NET WebHooks Empfänger</span><span class="sxs-lookup"><span data-stu-id="49126-103">ASP.NET WebHooks receivers</span></span>

<span data-ttu-id="49126-104">Empfangen von WebHooks, hängt davon ab, die der Absender ist.</span><span class="sxs-lookup"><span data-stu-id="49126-104">Receiving WebHooks depends on who the sender is.</span></span> <span data-ttu-id="49126-105">In einigen Fällen sind zusätzliche Schritte, die einen WebHook, überprüfen, dass der Abonnent eine wirklich Überwachung registrieren.</span><span class="sxs-lookup"><span data-stu-id="49126-105">Sometimes there are additional steps registering a WebHook verifying that the subscriber is really listening.</span></span> <span data-ttu-id="49126-106">Einige WebHooks bieten ein Push-Pull-Modell, in dem die HTTP-POST-Anforderung nur einen Verweis auf die Informationen enthält, die dann ist separat abgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="49126-106">Some WebHooks provide a push-to-pull model where the HTTP POST request only contains a reference to the event information which is then to be retrieved independently.</span></span> <span data-ttu-id="49126-107">Häufig variiert erheblich des Sicherheitsmodells.</span><span class="sxs-lookup"><span data-stu-id="49126-107">Often the security model varies quite a bit.</span></span>

<span data-ttu-id="49126-108">Microsoft ASP.NET WebHooks dient sie einfacher und konsistenter für die von Ihrer API von Netzwerkdaten ohne viel Zeit zu ermitteln, wie eine bestimmte Variante WebHooks behandelt machen.</span><span class="sxs-lookup"><span data-stu-id="49126-108">The purpose of Microsoft ASP.NET WebHooks is to make it both simpler and more consistent to wire up your API without spending a lot of time figuring out how to handle any particular variant of WebHooks.</span></span>

<span data-ttu-id="49126-109">Ein WebHook Empfänger ist für akzeptieren, und überprüfen WebHooks anhand bestimmter Absender verantwortlich.</span><span class="sxs-lookup"><span data-stu-id="49126-109">A WebHook receiver is responsible for accepting and verifying WebHooks from a particular sender.</span></span> <span data-ttu-id="49126-110">Ein WebHook Empfänger kann eine beliebige Anzahl von WebHooks, jeweils über seine eigene Konfiguration unterstützen.</span><span class="sxs-lookup"><span data-stu-id="49126-110">A WebHook receiver can support any number of WebHooks, each with their own configuration.</span></span> <span data-ttu-id="49126-111">Beispielsweise kann der Empfänger GitHub WebHook WebHooks, die von einer beliebigen Anzahl von GitHub-Repositorys akzeptieren.</span><span class="sxs-lookup"><span data-stu-id="49126-111">For example, the GitHub WebHook receiver can accept WebHooks from any number of GitHub repositories.</span></span>

## <a name="webhook-receiver-uris"></a><span data-ttu-id="49126-112">WebHook-Empfänger-URIs</span><span class="sxs-lookup"><span data-stu-id="49126-112">WebHook Receiver URIs</span></span>

<span data-ttu-id="49126-113">Durch die Installation von Microsoft ASP.NET WebHooks erhalten Sie einen allgemeine WebHook-Controller, der WebHook-Anforderungen von einer begrenzten Anzahl von Diensten akzeptiert.</span><span class="sxs-lookup"><span data-stu-id="49126-113">By installing Microsoft ASP.NET WebHooks you get a general WebHook controller which accepts WebHook requests from an open-ended number of services.</span></span> <span data-ttu-id="49126-114">Wenn eine Anforderung eingeht, wählt er den entsprechenden Empfänger, den Sie installiert haben, für die Behandlung bestimmten WebHook Absenders.</span><span class="sxs-lookup"><span data-stu-id="49126-114">When a request arrives, it picks the appropriate receiver that you have installed for handling a particular WebHook sender.</span></span>

<span data-ttu-id="49126-115">Der URI des diesem Controller ist die WebHook-URI, den Sie mit dem Dienst zu registrieren und das Format:</span><span class="sxs-lookup"><span data-stu-id="49126-115">The URI of this controller is the WebHook URI that you register with the service and is of the form:</span></span>

```
https://<host>/api/webhooks/incoming/<receiver>/{id}
```

<span data-ttu-id="49126-116">Aus Gründen der Sicherheit vieler WebHook Empfänger erfordern, dass der URI ist ein *Https* URI und in einigen Fällen müssen sie auch eine zusätzliche Abfrageparameter dient zum erzwingen, dass nur die vorgesehene Partei WebHooks an den oben genannten URI senden kann enthalten .</span><span class="sxs-lookup"><span data-stu-id="49126-116">For security reasons, many WebHook receivers require that the URI is an *https* URI and in some cases it must also contain an additional query parameter which is used to enforce that only the intended party can send WebHooks to the URI above.</span></span>

<span data-ttu-id="49126-117">Die  *<receiver>*  Komponente ist der Name des Empfängers, z. B. *Github* oder *Slack*.</span><span class="sxs-lookup"><span data-stu-id="49126-117">The *<receiver>* component is the name of the receiver, for example *github* or *slack*.</span></span>

<span data-ttu-id="49126-118">Die *{Id}* ist ein optionaler Bezeichner, der verwendet werden kann, um eine bestimmte Konfiguration von WebHook Empfänger zu identifizieren.</span><span class="sxs-lookup"><span data-stu-id="49126-118">The *{id}* is an optional identifier which can be used to identify a particular WebHook receiver configuration.</span></span> <span data-ttu-id="49126-119">Dies kann verwendet werden, um N WebHooks mit einem bestimmten Empfänger zu registrieren.</span><span class="sxs-lookup"><span data-stu-id="49126-119">This can be used to register N WebHooks with a particular receiver.</span></span> <span data-ttu-id="49126-120">Beispielsweise kann die folgenden drei URIs verwendet werden, für die drei unabhängige WebHooks registrieren:</span><span class="sxs-lookup"><span data-stu-id="49126-120">For example, the following three URIs can be used to register for three independent WebHooks:</span></span>

```
https://<host>/api/webhooks/incoming/github
https://<host>/api/webhooks/incoming/github/12345
https://<host>/api/webhooks/incoming/github/54321
```

## <a name="installing-a-webhook-receiver"></a><span data-ttu-id="49126-121">Installieren einen Empfänger WebHook</span><span class="sxs-lookup"><span data-stu-id="49126-121">Installing a WebHook Receiver</span></span>

<span data-ttu-id="49126-122">Zum Verwenden von Microsoft ASP.NET WebHooks WebHooks zu erhalten, installieren Sie zuerst das NuGet-Paket für den WebHook-Anbieter oder der Anbieter von WebHooks erhalten sollen.</span><span class="sxs-lookup"><span data-stu-id="49126-122">To receive WebHooks using Microsoft ASP.NET WebHooks, you first install the Nuget package for the WebHook provider or providers you want to receive WebHooks from.</span></span> <span data-ttu-id="49126-123">Die NuGet-Pakete werden mit dem Namen [Microsoft.AspNet.WebHooks.Receivers.*](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers) , in denen der letzte Teil gibt des Diensts unterstützt.</span><span class="sxs-lookup"><span data-stu-id="49126-123">The Nuget packages are named [Microsoft.AspNet.WebHooks.Receivers.*](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers) where the last part indicates the service supported.</span></span> <span data-ttu-id="49126-124">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="49126-124">For example</span></span>

<span data-ttu-id="49126-125">[Microsoft.AspNet.WebHooks.Receivers.GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) bietet Unterstützung für den Empfang von WebHooks von GitHub und [Microsoft.AspNet.WebHooks.Receivers.Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) bietet Unterstützung für den Empfang von WebHooks, die von ASP generiert. NET-WebHooks.</span><span class="sxs-lookup"><span data-stu-id="49126-125">[Microsoft.AspNet.WebHooks.Receivers.GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) provides support for receiving WebHooks from GitHub and [Microsoft.AspNet.WebHooks.Receivers.Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) provides support for receiving WebHooks generated by ASP.NET WebHooks.</span></span>

<span data-ttu-id="49126-126">Direktes finden Sie Unterstützung für Dropbox, GitHub, MailChimp, PayPal, Pusher, Salesforce, Slack, Stripe, Trello und WordPress, aber es ist möglich, eine beliebige Anzahl von anderen Anbietern unterstützt.</span><span class="sxs-lookup"><span data-stu-id="49126-126">Out of the box you can find support for Dropbox, GitHub, MailChimp, PayPal, Pusher, Salesforce, Slack, Stripe, Trello, and WordPress but it is possible to support any number of other providers.</span></span>

## <a name="configuring-a-webhook-receiver"></a><span data-ttu-id="49126-127">Konfigurieren einen Empfänger WebHook</span><span class="sxs-lookup"><span data-stu-id="49126-127">Configuring a WebHook Receiver</span></span>

<span data-ttu-id="49126-128">WebHook Empfänger werden konfiguriert, über die [IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) -Schnittstelle und bestimmte Implementierungen dieser Schnittstelle, können mithilfe von Dependency Injection-Modellen registriert werden.</span><span class="sxs-lookup"><span data-stu-id="49126-128">WebHook Receivers are configured through the [IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) inteface and particular implementations of that interface can be registered using any dependency injection model.</span></span> <span data-ttu-id="49126-129">Die Standardimplementierung verwendet die Anwendungseinstellungen die kann entweder in der Datei "Web.config" festgelegt werden, oder bei Verwendung von Azure-Web-Apps können durch Festlegen der [Azure-Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="49126-129">The default implementation uses Application Settings which can either be set in the Web.config file, or, if using Azure Web Apps, can be set through the [Azure Portal](https://portal.azure.com/).</span></span>

![Azure-App-Einstellungen](_static/AzureAppSettings.png)

<span data-ttu-id="49126-131">Das Format für die Anwendung Einstellungsschlüssel lautet wie folgt:</span><span class="sxs-lookup"><span data-stu-id="49126-131">The format for Application Setting keys is as follows:</span></span>

```
MS_WebHookReceiverSecret_<receiver>
```

<span data-ttu-id="49126-132">Der Wert ist eine durch Trennzeichen getrennte Liste der Werte, die mit der *{Id}* Werte für die WebHooks, beispielsweise erfasst wurden:</span><span class="sxs-lookup"><span data-stu-id="49126-132">The value is a comma-separated list of values matching the *{id}* values for which WebHooks have been registered, for example:</span></span>

```
MS_WebHookReceiverSecret_GitHub = <secret1>, 12345=<secret2>, 54321=<secret3>
```

## <a name="initializing-a-webhook-receiver"></a><span data-ttu-id="49126-133">Ein WebHook Empfänger initialisieren</span><span class="sxs-lookup"><span data-stu-id="49126-133">Initializing a WebHook Receiver</span></span>

<span data-ttu-id="49126-134">WebHook Empfänger werden initialisiert, durch die Registrierung, in der Regel in der *"webapiconfig"* statische Klasse, zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="49126-134">WebHook Receivers are initialized by registering them, typically in the *WebApiConfig* static class, for example:</span></span>

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
