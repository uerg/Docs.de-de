---
title: Überlegungen zum Entwurf der SignalR-API
author: anurse
description: Erfahren Sie, wie SignalR-APIs für die Kompatibilität zwischen Versionen Ihrer App zu entwerfen.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 11/06/2018
uid: signalr/api-design
ms.openlocfilehash: 3f17bf055b793e8fc91fbcc15f668928ca261f77
ms.sourcegitcommit: 408921a932448f66cb46fd53c307a864f5323fe5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/12/2018
ms.locfileid: "51571552"
---
# <a name="signalr-api-design-considerations"></a><span data-ttu-id="1aa77-103">Überlegungen zum Entwurf der SignalR-API</span><span class="sxs-lookup"><span data-stu-id="1aa77-103">SignalR API design considerations</span></span>

<span data-ttu-id="1aa77-104">Durch [Andrew Stanton-Nurse](https://twitter.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="1aa77-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse)</span></span>

<span data-ttu-id="1aa77-105">Dieser Artikel enthält Anleitungen zum Erstellen von SignalR-basierten APIs.</span><span class="sxs-lookup"><span data-stu-id="1aa77-105">This article provides guidance for building SignalR-based APIs.</span></span>

## <a name="use-custom-object-parameters-to-ensure-backwards-compatibility"></a><span data-ttu-id="1aa77-106">Verwenden Sie benutzerdefiniertes Objekt-Parameter, um Abwärtskompatibilität zu gewährleisten.</span><span class="sxs-lookup"><span data-stu-id="1aa77-106">Use custom object parameters to ensure backwards-compatibility</span></span>

<span data-ttu-id="1aa77-107">Hinzufügen von Parametern an eine SignalR-Hub-Methode (auf dem Client oder Server) ist eine *bedeutende Änderung*.</span><span class="sxs-lookup"><span data-stu-id="1aa77-107">Adding parameters to a SignalR hub method (on either the client or the server) is a *breaking change*.</span></span> <span data-ttu-id="1aa77-108">Dies bedeutet, dass ältere Clients oder Server Fehler erhalten, wenn sie versuchen, die ohne die entsprechende Anzahl von Parametern für Methodenaufrufe.</span><span class="sxs-lookup"><span data-stu-id="1aa77-108">This means older clients/servers will get errors when they try to invoke the method without the appropriate number of parameters.</span></span> <span data-ttu-id="1aa77-109">Allerdings ist das Hinzufügen von Eigenschaften für einen Parameter des benutzerdefinierten Objekts **nicht** eine unterbrechende Änderung.</span><span class="sxs-lookup"><span data-stu-id="1aa77-109">However, adding properties to a custom object parameter is **not** a breaking change.</span></span> <span data-ttu-id="1aa77-110">Dies kann verwendet werden, zum Entwerfen von kompatiblen APIs, die auf Änderungen auf dem Client oder Server stabil sind.</span><span class="sxs-lookup"><span data-stu-id="1aa77-110">This can be used to design compatible APIs that are resilient to changes on the client or the server.</span></span>

<span data-ttu-id="1aa77-111">Betrachten Sie beispielsweise eine serverseitige API wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="1aa77-111">For example, consider a server-side API like the following:</span></span>

[!code-csharp[ParameterBasedOldVersion](api-design/sample/Samples.cs?name=ParameterBasedOldVersion)]

<span data-ttu-id="1aa77-112">Der JavaScript-Client ruft diese Methode mit `invoke` wie folgt:</span><span class="sxs-lookup"><span data-stu-id="1aa77-112">The JavaScript client calls this method using `invoke` as follows:</span></span>

[!code-typescript[CallWithOneParameter](api-design/sample/Samples.ts?name=CallWithOneParameter)]

<span data-ttu-id="1aa77-113">Wenn Sie später einen zweiten Parameter an die Servermethode hinzufügen, wird nicht ältere Clients diesen Parameterwert angeben.</span><span class="sxs-lookup"><span data-stu-id="1aa77-113">If you later add a second parameter to the server method, older clients won't provide this parameter value.</span></span> <span data-ttu-id="1aa77-114">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="1aa77-114">For example:</span></span>

[!code-csharp[ParameterBasedNewVersion](api-design/sample/Samples.cs?name=ParameterBasedNewVersion)]

<span data-ttu-id="1aa77-115">Wenn der alte Client versucht, diese Methode aufzurufen, erhalten sie eine Fehlermeldung wie folgt:</span><span class="sxs-lookup"><span data-stu-id="1aa77-115">When the old client tries to invoke this method, it will get an error like this:</span></span>

```
Microsoft.AspNetCore.SignalR.HubException: Failed to invoke 'GetTotalLength' due to an error on the server.
```

<span data-ttu-id="1aa77-116">Auf dem Server sehen Sie eine protokollmeldung wie folgt:</span><span class="sxs-lookup"><span data-stu-id="1aa77-116">On the server, you'll see a log message like this:</span></span>

```
System.IO.InvalidDataException: Invocation provides 1 argument(s) but target expects 2.
```

<span data-ttu-id="1aa77-117">Die alten Clients nur einen Parameter gesendet, aber der neuere API-Server erforderlich, zwei Parameter.</span><span class="sxs-lookup"><span data-stu-id="1aa77-117">The old client only sent one parameter, but the newer server API required two parameters.</span></span> <span data-ttu-id="1aa77-118">Verwenden von benutzerdefinierten Objekten als Parameter können Sie flexibler.</span><span class="sxs-lookup"><span data-stu-id="1aa77-118">Using custom objects as parameters gives you more flexibility.</span></span> <span data-ttu-id="1aa77-119">Wir ändern die original-API, um ein benutzerdefiniertes Objekt zu verwenden:</span><span class="sxs-lookup"><span data-stu-id="1aa77-119">Let's redesign the original API to use a custom object:</span></span>

[!code-csharp[ObjectBasedOldVersion](api-design/sample/Samples.cs?name=ObjectBasedOldVersion)]

<span data-ttu-id="1aa77-120">Nun verwendet der Client ein Objekt zum Aufrufen der Methode:</span><span class="sxs-lookup"><span data-stu-id="1aa77-120">Now, the client uses an object to call the method:</span></span>

[!code-typescript[CallWithObject](api-design/sample/Samples.ts?name=CallWithObject)]

<span data-ttu-id="1aa77-121">Anstatt einen Parameter hinzufügen, fügen Sie eine Eigenschaft, um die `TotalLengthRequest` Objekt:</span><span class="sxs-lookup"><span data-stu-id="1aa77-121">Instead of adding a parameter, add a property to the `TotalLengthRequest` object:</span></span>

[!code-csharp[ObjectBasedNewVersion](api-design/sample/Samples.cs?name=ObjectBasedNewVersion&highlight=4,9-13)]

<span data-ttu-id="1aa77-122">Wenn die alten Clients sendet einen einzelnen Parameter, die zusätzlichen `Param2` Eigenschaft wechselt `null`.</span><span class="sxs-lookup"><span data-stu-id="1aa77-122">When the old client sends a single parameter, the extra `Param2` property will be left `null`.</span></span> <span data-ttu-id="1aa77-123">Sie können erkennen, dass eine Nachricht von einem älteren Client gesendet werden, indem Sie überprüfen die `Param2` für `null` und Standardwert anwenden.</span><span class="sxs-lookup"><span data-stu-id="1aa77-123">You can detect a message sent by an older client by checking the `Param2` for `null` and apply a default value.</span></span> <span data-ttu-id="1aa77-124">Ein neuer Client kann beide Parameter senden.</span><span class="sxs-lookup"><span data-stu-id="1aa77-124">A new client can send both parameters.</span></span>

[!code-typescript[CallWithObjectNew](api-design/sample/Samples.ts?name=CallWithObjectNew)]

<span data-ttu-id="1aa77-125">Dieselbe Vorgehensweise funktioniert für Methoden, die auf dem Client definiert.</span><span class="sxs-lookup"><span data-stu-id="1aa77-125">The same technique works for methods defined on the client.</span></span> <span data-ttu-id="1aa77-126">Sie können ein benutzerdefiniertes Objekt von der Serverseite senden:</span><span class="sxs-lookup"><span data-stu-id="1aa77-126">You can send a custom object from the server side:</span></span>

[!code-csharp[ClientSideObjectBasedOld](api-design/sample/Samples.cs?name=ClientSideObjectBasedOld)]

<span data-ttu-id="1aa77-127">Klicken Sie auf der Clientseite, in dem Sie Zugriff auf die `Message` -Eigenschaft, anstatt einen Parameter:</span><span class="sxs-lookup"><span data-stu-id="1aa77-127">On the client side, you access the `Message` property rather than using a parameter:</span></span>

[!code-typescript[OnWithObjectOld](api-design/sample/Samples.ts?name=OnWithObjectOld)]

<span data-ttu-id="1aa77-128">Wenn Sie später die Nutzlast der Absender der Nachricht hinzu, fügen Sie auf das Objekt eine Eigenschaft hinzu:</span><span class="sxs-lookup"><span data-stu-id="1aa77-128">If you later decide to add the sender of the message to the payload, add a property to the object:</span></span>

[!code-csharp[ClientSideObjectBasedNew](api-design/sample/Samples.cs?name=ClientSideObjectBasedNew&highlight=5)]

<span data-ttu-id="1aa77-129">Die älteren Clients wird nicht erwartet werden die `Sender` Wert, sodass sie es ignoriert werden.</span><span class="sxs-lookup"><span data-stu-id="1aa77-129">The older clients won't be expecting the `Sender` value, so they'll ignore it.</span></span> <span data-ttu-id="1aa77-130">Ein neuer Client können sie übernehmen, indem Sie aktualisiert, um die neue Eigenschaft zu lesen:</span><span class="sxs-lookup"><span data-stu-id="1aa77-130">A new client can accept it by updating to read the new property:</span></span>

[!code-typescript[OnWithObjectNew](api-design/sample/Samples.ts?name=OnWithObjectNew&highlight=2-5)]

<span data-ttu-id="1aa77-131">Der neue Client ist in diesem Fall auch von einer alten Server, die nicht zur Verfügung stellt, fehlertolerante der `Sender` Wert.</span><span class="sxs-lookup"><span data-stu-id="1aa77-131">In this case, the new client is also tolerant of an old server that doesn't provide the `Sender` value.</span></span> <span data-ttu-id="1aa77-132">Da es sich bei der alte Server sind die `Sender` Wert, der Client überprüft, um festzustellen, ob es vorhanden ist, bevor Sie darauf zugreifen.</span><span class="sxs-lookup"><span data-stu-id="1aa77-132">Since the old server won't provide the `Sender` value, the client checks to see if it exists before accessing it.</span></span>
