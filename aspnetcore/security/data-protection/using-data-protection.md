---
title: Erste Schritte mit Data Protection-APIs
author: rick-anderson
description: "Dieses Dokument erläutert, wie Sie die ASP.NET Core Datenschutz-APIs zum Schützen und Aufheben des Schutzes Daten in eine app."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/using-data-protection
ms.openlocfilehash: c6a631b6dc4a7855b11031dfcef42b17906754b0
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
# <a name="getting-started-with-the-data-protection-apis"></a><span data-ttu-id="91f2d-103">Erste Schritte mit Data Protection-APIs</span><span class="sxs-lookup"><span data-stu-id="91f2d-103">Getting Started with the Data Protection APIs</span></span>

<a name="security-data-protection-getting-started"></a>

<span data-ttu-id="91f2d-104">Die einfachste und schützen Daten besteht aus folgenden Schritten:</span><span class="sxs-lookup"><span data-stu-id="91f2d-104">At its simplest, protecting data consists of the following steps:</span></span>

1. <span data-ttu-id="91f2d-105">Erstellen Sie eine Schutzvorrichtung aus einen Datenschutzanbieter.</span><span class="sxs-lookup"><span data-stu-id="91f2d-105">Create a data protector from a data protection provider.</span></span>

2. <span data-ttu-id="91f2d-106">Rufen Sie die `Protect` Methode mit den Daten, die Sie schützen möchten.</span><span class="sxs-lookup"><span data-stu-id="91f2d-106">Call the `Protect` method with the data you want to protect.</span></span>

3. <span data-ttu-id="91f2d-107">Rufen Sie die `Unprotect` Methode mit den Daten zurück in Klartext umgewandelt werden sollen.</span><span class="sxs-lookup"><span data-stu-id="91f2d-107">Call the `Unprotect` method with the data you want to turn back into plain text.</span></span>

<span data-ttu-id="91f2d-108">Die meisten Frameworks und app-Modelle, z. B. ASP.NET oder SignalR, bereits das Data Protection-System konfigurieren und einen Dienstcontainer, die den Zugriff über Abhängigkeitsinjektion hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="91f2d-108">Most frameworks and app models, such as ASP.NET or SignalR, already configure the data protection system and add it to a service container you access via dependency injection.</span></span> <span data-ttu-id="91f2d-109">Das folgende Beispiel zeigt einen Dienstcontainer für zielabhängigkeit konfigurieren und registrieren den Data Protection-Stapel, der Datenschutzanbieter über DI empfangen, erstellen eine Schutzvorrichtung und schützen und Aufheben des Schutzes von Daten</span><span class="sxs-lookup"><span data-stu-id="91f2d-109">The following sample demonstrates configuring a service container for dependency injection and registering the data protection stack, receiving the data protection provider via DI, creating a protector and protecting then unprotecting data</span></span>

[!code-csharp[Main](../../security/data-protection/using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

<span data-ttu-id="91f2d-110">Wenn Sie eine Schutzvorrichtung erstellen müssen Sie angeben, eine oder mehrere [Zweck Zeichenfolgen](consumer-apis/purpose-strings.md).</span><span class="sxs-lookup"><span data-stu-id="91f2d-110">When you create a protector you must provide one or more [Purpose Strings](consumer-apis/purpose-strings.md).</span></span> <span data-ttu-id="91f2d-111">Eine Zeichenfolge Zweck wird eine Isolation zwischen Consumer.</span><span class="sxs-lookup"><span data-stu-id="91f2d-111">A purpose string provides isolation between consumers.</span></span> <span data-ttu-id="91f2d-112">Beispielsweise wäre eine Schutzvorrichtung, die durch die Zeichenfolge "Grün" Zweck erstellt Aufheben des Schutzes von Daten, die durch eine Schutzvorrichtung bereitgestellt, mit dem Zweck "Violett" können nicht.</span><span class="sxs-lookup"><span data-stu-id="91f2d-112">For example, a protector created with a purpose string of "green" wouldn't be able to unprotect data provided by a protector with a purpose of "purple".</span></span>

>[!TIP]
> <span data-ttu-id="91f2d-113">Instanzen von `IDataProtectionProvider` und `IDataProtector` für mehrere Aufrufer threadsicher sind.</span><span class="sxs-lookup"><span data-stu-id="91f2d-113">Instances of `IDataProtectionProvider` and `IDataProtector` are thread-safe for multiple callers.</span></span> <span data-ttu-id="91f2d-114">Es ist vorgesehen, die nach eine Komponente einen Verweis auf Ruft eine `IDataProtector` über einen Aufruf an `CreateProtector`, verwenden sie diesen Verweis für mehrere Aufrufe von `Protect` und `Unprotect`.</span><span class="sxs-lookup"><span data-stu-id="91f2d-114">It's intended that once a component gets a reference to an `IDataProtector` via a call to `CreateProtector`, it will use that reference for multiple calls to `Protect` and `Unprotect`.</span></span>
>
><span data-ttu-id="91f2d-115">Ein Aufruf von `Unprotect` löst CryptographicException aus, wenn die geschützte Nutzlast überprüft oder entschlüsselt werden kann.</span><span class="sxs-lookup"><span data-stu-id="91f2d-115">A call to `Unprotect` will throw CryptographicException if the protected payload cannot be verified or deciphered.</span></span> <span data-ttu-id="91f2d-116">Einige Komponenten Fehler ignorieren möchten Aufheben des Schutzes von während der Vorgänge eine Komponente, die Authentifizierungscookies liest möglicherweise diesen Fehler zu behandeln und die Anforderung zu behandeln, als ob es überhaupt kein Cookie hatte anstelle der sofortiges Anforderung schlägt fehl.</span><span class="sxs-lookup"><span data-stu-id="91f2d-116">Some components may wish to ignore errors during unprotect operations; a component which reads authentication cookies might handle this error and treat the request as if it had no cookie at all rather than fail the request outright.</span></span> <span data-ttu-id="91f2d-117">Komponenten, die über dieses Verhalten soll sollten CryptographicException speziell statt Angriffspunkt alle Ausnahmen abfangen.</span><span class="sxs-lookup"><span data-stu-id="91f2d-117">Components which want this behavior should specifically catch CryptographicException instead of swallowing all exceptions.</span></span>
