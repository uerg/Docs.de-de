---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-22
title: Neuigkeiten in der ASP.NET Web API 2.2 | Microsoft-Dokumentation
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/25/2014
ms.topic: article
ms.assetid: 99c59ae4-167e-4f66-a6cd-d3f1098c4e4a
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 51b01c4b9ee8d12e4e3925193e308d3ca6c5f2b6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377440"
---
<a name="whats-new-in-aspnet-web-api-22"></a><span data-ttu-id="5f6ab-102">Neuerungen in ASP.NET Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="5f6ab-102">What's New in ASP.NET Web API 2.2</span></span>
====================
<span data-ttu-id="5f6ab-103">durch [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="5f6ab-103">by [Microsoft](https://github.com/microsoft)</span></span>

<span data-ttu-id="5f6ab-104">Dieses Thema beschreibt, welche neuerungen für ASP.NET Web API 2.2.</span><span class="sxs-lookup"><span data-stu-id="5f6ab-104">This topic describes what's new for ASP.NET Web API 2.2.</span></span>

- [<span data-ttu-id="5f6ab-105">Herunterladen</span><span class="sxs-lookup"><span data-stu-id="5f6ab-105">Download</span></span>](#download)
- [<span data-ttu-id="5f6ab-106">Dokumentation</span><span class="sxs-lookup"><span data-stu-id="5f6ab-106">Documentation</span></span>](#documentation)
- [<span data-ttu-id="5f6ab-107">Neue Features in der ASP.NET Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="5f6ab-107">New Features in ASP.NET Web API 2.2</span></span>](#newf)

    - [<span data-ttu-id="5f6ab-108">OData v4</span><span class="sxs-lookup"><span data-stu-id="5f6ab-108">OData v4</span></span>](#OData)
    - [<span data-ttu-id="5f6ab-109">Attribut-Routing-Verbesserungen</span><span class="sxs-lookup"><span data-stu-id="5f6ab-109">Attribute Routing Improvements</span></span>](#ARI)
    - [<span data-ttu-id="5f6ab-110">Web-API-Client-Unterstützung für Windows Phone 8.1</span><span class="sxs-lookup"><span data-stu-id="5f6ab-110">Web API Client support for Windows Phone 8.1</span></span>](#phone)
- [<span data-ttu-id="5f6ab-111">Bekannte Probleme und aktueller Änderungen</span><span class="sxs-lookup"><span data-stu-id="5f6ab-111">Known Issues and Breaking Changes</span></span>](#known-issues)
- [<span data-ttu-id="5f6ab-112">Fehlerbehebungen</span><span class="sxs-lookup"><span data-stu-id="5f6ab-112">Bug Fixes</span></span>](#bug-fixes)
- [<span data-ttu-id="5f6ab-113">Microsoft.AspNet.OData 5.2.1</span><span class="sxs-lookup"><span data-stu-id="5f6ab-113">Microsoft.AspNet.OData 5.2.1</span></span>](#odata521)
- [<span data-ttu-id="5f6ab-114">Microsoft.AspNet.WebAPI 5.2.2</span><span class="sxs-lookup"><span data-stu-id="5f6ab-114">Microsoft.AspNet.WebAPI 5.2.2</span></span>](#522RC)
- [<span data-ttu-id="5f6ab-115">Microsoft.AspNet.WebAPI vom Typ 5.2.3 Beta</span><span class="sxs-lookup"><span data-stu-id="5f6ab-115">Microsoft.AspNet.WebAPI 5.2.3 Beta</span></span>](#523)

<a id="download"></a>
## <a name="download"></a><span data-ttu-id="5f6ab-116">Herunterladen</span><span class="sxs-lookup"><span data-stu-id="5f6ab-116">Download</span></span>

<span data-ttu-id="5f6ab-117">Die CLR-Funktionen werden als NuGet-Pakete im NuGet-Katalog veröffentlicht.</span><span class="sxs-lookup"><span data-stu-id="5f6ab-117">The runtime features are released as NuGet packages on the NuGet gallery.</span></span> <span data-ttu-id="5f6ab-118">Die Common Language Runtime-Pakete führen Sie die [semantische Versionierung](http://semver.org/) Spezifikation.</span><span class="sxs-lookup"><span data-stu-id="5f6ab-118">All the runtime packages follow the [Semantic Versioning](http://semver.org/) specification.</span></span> <span data-ttu-id="5f6ab-119">Das neueste Paket mit ASP.NET Web API 2.2 hat die folgende Version: "5.2.0".</span><span class="sxs-lookup"><span data-stu-id="5f6ab-119">The latest ASP.NET Web API 2.2 package has the following version: "5.2.0".</span></span> <span data-ttu-id="5f6ab-120">Können Sie zu installieren oder aktualisieren Sie diese Pakete über [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/).</span><span class="sxs-lookup"><span data-stu-id="5f6ab-120">You can install or update these packages through [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/).</span></span> <span data-ttu-id="5f6ab-121">Die Version enthält auch entsprechende lokalisierte Pakete für NuGet.</span><span class="sxs-lookup"><span data-stu-id="5f6ab-121">The release also includes corresponding localized packages on NuGet.</span></span>

<span data-ttu-id="5f6ab-122">Sie können die installieren oder aktualisieren Sie auf die veröffentlichte NuGet-Pakete mithilfe der NuGet-Paket-Manager-Konsole:</span><span class="sxs-lookup"><span data-stu-id="5f6ab-122">You can install or update to the released NuGet packages by using the NuGet Package Manager Console:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-22/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a><span data-ttu-id="5f6ab-123">Dokumentation</span><span class="sxs-lookup"><span data-stu-id="5f6ab-123">Documentation</span></span>

<span data-ttu-id="5f6ab-124">Lernprogramme und Weitere Informationen zu ASP.NET Web API 2.2 stehen auf der Website für ASP.NET ([https://www.asp.net/web-api](../../index.md)).</span><span class="sxs-lookup"><span data-stu-id="5f6ab-124">Tutorials and other information about ASP.NET Web API 2.2 are available from the ASP.NET web site ([https://www.asp.net/web-api](../../index.md)).</span></span>

<a id="newf"></a>
## <a name="new-features-in-aspnet-web-api-22"></a><span data-ttu-id="5f6ab-125">Neue Features in der ASP.NET Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="5f6ab-125">New Features in ASP.NET Web API 2.2</span></span>

<a id="OData"></a>
### <a name="odata-v4"></a><span data-ttu-id="5f6ab-126">OData v4</span><span class="sxs-lookup"><span data-stu-id="5f6ab-126">OData v4</span></span>

<span data-ttu-id="5f6ab-127">Diese Version bietet Unterstützung für OData v4-Protokolls.</span><span class="sxs-lookup"><span data-stu-id="5f6ab-127">This release adds support for the OData v4 protocol.</span></span> <span data-ttu-id="5f6ab-128">Weitere Informationen finden Sie unter den [Web-API OData v4-Dokumentation.](../odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint.md)</span><span class="sxs-lookup"><span data-stu-id="5f6ab-128">For more information, see the [Web API OData v4 documentation.](../odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint.md)</span></span>

<span data-ttu-id="5f6ab-129">Hier sind einige der wichtigsten Features und Änderungen für OData v4:</span><span class="sxs-lookup"><span data-stu-id="5f6ab-129">Here are some of the key features and changes for OData v4:</span></span>

- [<span data-ttu-id="5f6ab-130">Unterstützung von Aliasing-Eigenschaften in der OData-Modell</span><span class="sxs-lookup"><span data-stu-id="5f6ab-130">Support for aliasing properties in OData model</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataModelAliasingSample/)
- [<span data-ttu-id="5f6ab-131">Unterstützung für ComplexTypeAttribute "," AssociationAttribute "," TimesTampAttribute "und" ConcurrencyCheckAttribute in ODataConventionModelBuilder</span><span class="sxs-lookup"><span data-stu-id="5f6ab-131">Support for ComplexTypeAttribute, AssociationAttribute, TimesTampAttribute and ConcurrencyCheckAttribute in ODataConventionModelBuilder</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEtagSample/)
- [<span data-ttu-id="5f6ab-132">Geben Sie die Möglichkeit, die benutzerfreundliche Titel für Aktionen angeben</span><span class="sxs-lookup"><span data-stu-id="5f6ab-132">Provide ability to supply friendly Title for actions</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataActionsSample/)
- <span data-ttu-id="5f6ab-133">ODL-UriParser integrieren</span><span class="sxs-lookup"><span data-stu-id="5f6ab-133">Integrate with ODL UriParser</span></span>
- <span data-ttu-id="5f6ab-134">Unterstützung für [Enum](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEnumTypeSample/ODataEnumTypeSample/), [Kapselung](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/) und [Singleton](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataSingletonSample/)</span><span class="sxs-lookup"><span data-stu-id="5f6ab-134">Support for [enum](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEnumTypeSample/ODataEnumTypeSample/), [containment](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/) and [singleton](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataSingletonSample/)</span></span>
- <span data-ttu-id="5f6ab-135">Unterstützung für primitive Typen umwandeln</span><span class="sxs-lookup"><span data-stu-id="5f6ab-135">Support cast for primitive types</span></span>
- [<span data-ttu-id="5f6ab-136">Unterstützung für OData-Funktion</span><span class="sxs-lookup"><span data-stu-id="5f6ab-136">Added OData function support</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [<span data-ttu-id="5f6ab-137">Unterstützen Sie parameteraliase für Funktionsaufrufe</span><span class="sxs-lookup"><span data-stu-id="5f6ab-137">Support parameter aliases for function calls</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [<span data-ttu-id="5f6ab-138">Camel Case Benennungskonvention im Modell zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="5f6ab-138">Support camel case naming convention in model</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataCamelCaseSample/)
- <span data-ttu-id="5f6ab-139">Unterstützung für cast() in $filter</span><span class="sxs-lookup"><span data-stu-id="5f6ab-139">Support for cast() in $filter</span></span>
- <span data-ttu-id="5f6ab-140">Unterstützung für open komplexen Typ</span><span class="sxs-lookup"><span data-stu-id="5f6ab-140">Support for open complex type</span></span>
- <span data-ttu-id="5f6ab-141">Entfernte EntitySetController und AsyncEntitySetController</span><span class="sxs-lookup"><span data-stu-id="5f6ab-141">Removed EntitySetController and AsyncEntitySetController</span></span>
- [<span data-ttu-id="5f6ab-142">Geänderte $link zu $ref</span><span class="sxs-lookup"><span data-stu-id="5f6ab-142">Changed $link to $ref</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataServiceSample/)
- [<span data-ttu-id="5f6ab-143">Hinzugefügte Attribut-routing-Unterstützung</span><span class="sxs-lookup"><span data-stu-id="5f6ab-143">Added Attribute routing support</span></span>](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataAttributeRoutingSample/)
- <span data-ttu-id="5f6ab-144">OData-Kernbibliotheken 6.4.0 verwendet</span><span class="sxs-lookup"><span data-stu-id="5f6ab-144">Uses OData Core Libraries 6.4.0</span></span>

<a id="ARI"></a>
### <a name="attribute-routing-improvements"></a><span data-ttu-id="5f6ab-145">Attribut-Routing-Verbesserungen</span><span class="sxs-lookup"><span data-stu-id="5f6ab-145">Attribute Routing Improvements</span></span>

<span data-ttu-id="5f6ab-146">Attributrouting jetzt bietet einen Erweiterungspunkt aufgerufen IDirectRouteProvider, die ermöglicht die vollständige Kontrolle darüber, wie attributrouten erkannt und konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="5f6ab-146">Attribute Routing now provides an extensibility point called IDirectRouteProvider, which allows full control over how attribute routes are discovered and configured.</span></span> <span data-ttu-id="5f6ab-147">Ein IDirectRouteProvider ist verantwortlich für die Angabe einer Liste von Aktionen und Controller zusammen mit zugehörigen Routeninformationen an genau welche Routingkonfiguration für diese Aktionen erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="5f6ab-147">An IDirectRouteProvider is responsible for providing a list of actions and controllers along with associated route information to specify exactly what routing configuration is desired for those actions.</span></span> <span data-ttu-id="5f6ab-148">Eine IDirectRouteProvider-Implementierung kann angegeben werden, wenn MapAttributes/MapHttpAttributeRoutes aufrufen.</span><span class="sxs-lookup"><span data-stu-id="5f6ab-148">An IDirectRouteProvider implementation may be specified when calling MapAttributes/MapHttpAttributeRoutes.</span></span>

<span data-ttu-id="5f6ab-149">Anpassen von IDirectRouteProvider werden am einfachsten durch die Erweiterung unserer Standardimplementierung DefaultDirectRouteProvider.</span><span class="sxs-lookup"><span data-stu-id="5f6ab-149">Customizing IDirectRouteProvider will be easiest by extending our default implementation, DefaultDirectRouteProvider.</span></span> <span data-ttu-id="5f6ab-150">Diese Klasse stellt separate überschreibbare virtuelle Methoden, um die Logik zum Ermitteln von Attributen, routeneinträge erstellen und Ermitteln von Routenpräfix und Bereichspräfix zu ändern.</span><span class="sxs-lookup"><span data-stu-id="5f6ab-150">This class provides separate overridable virtual methods to change the logic for discovering attributes, creating route entries, and discovering route prefix and area prefix.</span></span>

<span data-ttu-id="5f6ab-151">Es folgen einige Beispiele dazu, wie Sie dieser neue Erweiterungspunkt verwenden können:</span><span class="sxs-lookup"><span data-stu-id="5f6ab-151">Following are some examples on what you could do with this new extensibility point:</span></span>

1. <span data-ttu-id="5f6ab-152">Unterstützung von Vererbung von routenattribute</span><span class="sxs-lookup"><span data-stu-id="5f6ab-152">Support inheritance of Route attributes</span></span>

    <span data-ttu-id="5f6ab-153">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="5f6ab-153">Example:</span></span>

    <span data-ttu-id="5f6ab-154">Hier würde die Anforderung wie "/ api/Werte/10" erfolgreich "Erfolg: 10" zurückgeben</span><span class="sxs-lookup"><span data-stu-id="5f6ab-154">Here a request like "/api/values/10" would successfully return "Success:10"</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample2.cs)]
2. <span data-ttu-id="5f6ab-155">Geben Sie einen Standardnamen für die Route für die attributrouten, durch eine Konvention, die Sie wie folgt.</span><span class="sxs-lookup"><span data-stu-id="5f6ab-155">Provide a default route name for your attribute routes by following some convention you like.</span></span> <span data-ttu-id="5f6ab-156">In der Standardeinstellung erstellt attributrouting nicht automatisch Namen für die attributrouten.</span><span class="sxs-lookup"><span data-stu-id="5f6ab-156">By default, attribute routing doesn't automatically create names for attribute routes.</span></span>
3. <span data-ttu-id="5f6ab-157">Ändern Sie attributrouten routenvorlage an einem zentralen Ort aus, bevor sie in der Routentabelle enden.</span><span class="sxs-lookup"><span data-stu-id="5f6ab-157">Modify attribute routes' route template at one central place before they end up in the route table.</span></span>

<a id="phone"></a>
### <a name="web-api-client-support-for-windows-phone-81"></a><span data-ttu-id="5f6ab-158">Web-API-Client-Unterstützung für Windows Phone 8.1</span><span class="sxs-lookup"><span data-stu-id="5f6ab-158">Web API Client Support for Windows Phone 8.1</span></span>

<span data-ttu-id="5f6ab-159">Jetzt können Sie das Web-API-Client-NuGet-Paket implementieren Sie Ihre Web-API-Client-Logik beim Windows Phone 8.1 als Ziel oder von einer universellen App.</span><span class="sxs-lookup"><span data-stu-id="5f6ab-159">You can now use the Web API Client NuGet package to implement your Web API client logic when targeting Windows Phone 8.1 or from within a Universal App.</span></span>

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="5f6ab-160">Bekannte Probleme und aktueller Änderungen</span><span class="sxs-lookup"><span data-stu-id="5f6ab-160">Known Issues and Breaking Changes</span></span>

<span data-ttu-id="5f6ab-161">Dieser Abschnitt beschreibt bekannte Probleme und wichtige Änderungen in der ASP.NET Web API 2.2.</span><span class="sxs-lookup"><span data-stu-id="5f6ab-161">This section describes known issues and breaking changes in the ASP.NET Web API 2.2.</span></span>

### <a name="odata-v4"></a><span data-ttu-id="5f6ab-162">OData v4</span><span class="sxs-lookup"><span data-stu-id="5f6ab-162">OData v4</span></span>

#### <a name="model-builder"></a><span data-ttu-id="5f6ab-163">Modell-Generator</span><span class="sxs-lookup"><span data-stu-id="5f6ab-163">Model builder</span></span>

<span data-ttu-id="5f6ab-164">Problem: Überladene Funktionen können nicht als FunctionImport verfügbar gemacht werden</span><span class="sxs-lookup"><span data-stu-id="5f6ab-164">Issue: Overloaded Functions could not be exposed as FunctionImport</span></span>

<span data-ttu-id="5f6ab-165">Wenn 2 überladene Funktionen vorhanden sind, und sie auch FunctionImport sind wie unten, klicken Sie dann anfordern ~/GetAllConventionCustomers(CustomerName={customerName}) führt System.InvalidOperationException.</span><span class="sxs-lookup"><span data-stu-id="5f6ab-165">If there are 2 overloaded functions and they are also FunctionImport as shown below then requesting ~/GetAllConventionCustomers(CustomerName={customerName}) results in System.InvalidOperationException.</span></span>

[!code-xml[Main](whats-new-in-aspnet-web-api-22/samples/sample3.xml)]

<span data-ttu-id="5f6ab-166">Problemumgehung: Die problemumgehung für dieses Problem ist, um sowohl die funktionsüberladungen als FunctionImports hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="5f6ab-166">Workaround: The workaround for this issue is to add both the function overloads as FunctionImports.</span></span>

#### <a name="odata-routing"></a><span data-ttu-id="5f6ab-167">OData-Routing</span><span class="sxs-lookup"><span data-stu-id="5f6ab-167">OData Routing</span></span>

<span data-ttu-id="5f6ab-168">Zeichenfolgenliterale, die die URL-codierte Schrägstrich (% 2F), und backslash(%5C) verursachen einen 404-Fehler auf, wenn sie in den Pfaden der OData-Ressource verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="5f6ab-168">String literals that include the URL encoded slash (%2F), and backslash(%5C) cause a 404 error when they are used in the OData resource paths.</span></span>

<span data-ttu-id="5f6ab-169">Zeichenfolgenliterale können z. B. als Parameter von Funktionen oder Schlüsselwerte der Entitätenmengen in den Pfaden der OData-Ressource verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="5f6ab-169">For example, string literals can be used in the OData resource paths as parameters of functions or key values of entity sets.</span></span>

<span data-ttu-id="5f6ab-170">/Employees/Total.GetCount(Name='Name%2F')</span><span class="sxs-lookup"><span data-stu-id="5f6ab-170">/Employees/Total.GetCount(Name='Name%2F')</span></span>

<span data-ttu-id="5f6ab-171">/Employees('Name%5C')</span><span class="sxs-lookup"><span data-stu-id="5f6ab-171">/Employees('Name%5C')</span></span>

<span data-ttu-id="5f6ab-172">Wenn Dienste diese Anforderungen den Hosts werden Escapezeichen erhalten Escapesequenzen die fest, bevor sie an die Web-API-Laufzeit übergeben.</span><span class="sxs-lookup"><span data-stu-id="5f6ab-172">When services receive such requests the hosts will un-escape those escape sequences before passing them to the Web API runtime.</span></span> <span data-ttu-id="5f6ab-173">Dies schützt vor Angriffen wie folgt:</span><span class="sxs-lookup"><span data-stu-id="5f6ab-173">This protects against attacks like the following:</span></span>  
  
`http://www.contoso.com/..%2F..%2F/Windows/System32/cmd.exe?/c+dir+c:`

<span data-ttu-id="5f6ab-174">Dies bewirkt, dass den Web-API OData-Stapel einen 404-Fehler (nicht gefunden) zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="5f6ab-174">This causes the Web API OData stack to return a 404 error (Not Found).</span></span> <span data-ttu-id="5f6ab-175">Um diesen Fehler zu vermeiden, sollte der Client die doppelte Escapesequenzen für Schrägstrich (% 252F) und umgekehrter Schrägstrich (% 255-C) verwenden.</span><span class="sxs-lookup"><span data-stu-id="5f6ab-175">To prevent this error, your client should use the double escape sequences for slash (%252F) and backslash (%255C).</span></span> <span data-ttu-id="5f6ab-176">Dies geschieht nicht für Abfragezeichenfolgen, z. B. /Employees? $filter = Name Eq "Name % 2F"</span><span class="sxs-lookup"><span data-stu-id="5f6ab-176">This does not happen for query strings such as /Employees?$filter=Name eq 'Name%2F'</span></span>

<span data-ttu-id="5f6ab-177">**Beachten Sie ohne Escapezeichen Schrägstriche ("/"), und umgekehrte Schrägstriche (") sind nicht in OData-Ressource Pfad Zeichenfolgenliterale zulässig. Schrägstriche sollte angezeigt werden, nur als Pfadtrennzeichen und umgekehrte Schrägstriche sollten nicht alle in der OData-Ressourcenpfad angezeigt. (Beide sind in einigen Teilen des OData-Abfragezeichenfolge verwendet werden.)**</span><span class="sxs-lookup"><span data-stu-id="5f6ab-177">**Note un-escaped slashes ('/') and backslashes ('') are not legal in OData resource path string literals. Slashes should appear only as path separators and backslashes should not appear in the OData resource path at all. (Both are usable in some portions of an OData query string.)**</span></span>

<span data-ttu-id="5f6ab-178">Problemumgehung: Sie könnten die Parse-Methode der DefaultODataPathHandler der Schrägstrich und umgekehrte Schrägstriche sind bei Zeichenfolgenliteralen, die vor der Analyse tatsächlich als Escapezeichen überschreiben.</span><span class="sxs-lookup"><span data-stu-id="5f6ab-178">Workaround: You could override the Parse method of DefaultODataPathHandler to escape the slash and backslash in string literals before actually parsing them.</span></span> <span data-ttu-id="5f6ab-179">Sie finden ein Beispiel dieses Ansatzes besteht.</span><span class="sxs-lookup"><span data-stu-id="5f6ab-179">You can find a sample of this approach here.</span></span>

### <a name="odata-v3"></a><span data-ttu-id="5f6ab-180">OData v3</span><span class="sxs-lookup"><span data-stu-id="5f6ab-180">OData v3</span></span>

#### <a name="queryable"></a><span data-ttu-id="5f6ab-181">[Queryable]</span><span class="sxs-lookup"><span data-stu-id="5f6ab-181">[Queryable]</span></span>

<span data-ttu-id="5f6ab-182">Das Attribut [Queryable] ist veraltet.</span><span class="sxs-lookup"><span data-stu-id="5f6ab-182">The [Queryable] attribute is deprecated.</span></span> <span data-ttu-id="5f6ab-183">Neue OData v3-Anwendungen sollten verwenden **System.Web.Http.OData.EnableQueryAttribute**.</span><span class="sxs-lookup"><span data-stu-id="5f6ab-183">New OData v3 applications should use **System.Web.Http.OData.EnableQueryAttribute**.</span></span>

<span data-ttu-id="5f6ab-184">Die **ODataHttpConfigurationExtensions.EnableQuerySupport** Erweiterungsmethode bietet nun eine **EnableQueryAttribute** auf die globale filterauflistung.</span><span class="sxs-lookup"><span data-stu-id="5f6ab-184">The **ODataHttpConfigurationExtensions.EnableQuerySupport** extension method now adds an **EnableQueryAttribute** to the global filter collection.</span></span> <span data-ttu-id="5f6ab-185">Wenn alle Controller haben die **[Queryable]** Attribut aufrufen `config.EnableQuerySupport()` führt dazu, dass die **[Queryable]** Attribut fehlschlägt</span><span class="sxs-lookup"><span data-stu-id="5f6ab-185">If any controllers have the **[Queryable]** attribute, calling `config.EnableQuerySupport()` will cause the **[Queryable]** attribute to fail</span></span>

<span data-ttu-id="5f6ab-186">Die empfohlene Methode zum Beheben dieses Problems ist, zum Ersetzen aller Instanzen des **queryableattribute überprüft** mit **System.Web.Http.OData.EnableQueryAttribute**.</span><span class="sxs-lookup"><span data-stu-id="5f6ab-186">The recommended way to resolve this issue is to replace all instances of **QueryableAttribute** with **System.Web.Http.OData.EnableQueryAttribute**.</span></span>

<span data-ttu-id="5f6ab-187">Eine alternative problemumgehung besteht darin, den folgenden Code in Ihrer Web-API-Konfiguration verwenden:</span><span class="sxs-lookup"><span data-stu-id="5f6ab-187">An alternative workaround is to use the following code in your Web API configuration:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample4.cs)]

### <a name="attribute-routing"></a><span data-ttu-id="5f6ab-188">Attribut-Routing</span><span class="sxs-lookup"><span data-stu-id="5f6ab-188">Attribute Routing</span></span>

<span data-ttu-id="5f6ab-189">Problem: Modellbindung des komplexen Typs ab, mit FromUri-Attribut versehen ist, verhält sich anders bei Verwendung der Attribut-Routing.</span><span class="sxs-lookup"><span data-stu-id="5f6ab-189">Issue: Model binding of complex type which is decorated with FromUri attribute behaves differently when using Attribute Routing.</span></span>

<span data-ttu-id="5f6ab-190">Folgenden Link verfolgt das Problem auch und Details zu der dieses Problem zu umgehen.</span><span class="sxs-lookup"><span data-stu-id="5f6ab-190">Following link is tracking the issue and also has details about a workaround.</span></span>  
[http://aspnetwebstack.codeplex.com/workitem/1944](http://aspnetwebstack.codeplex.com/workitem/1944)

<span data-ttu-id="5f6ab-191">Problem: Gerüstbau MVC-Web-API in einem Projekt mit 5.2.0 Pakete führt 5.1.2 Pakete, die nicht bereits im Projekt vorhanden sind</span><span class="sxs-lookup"><span data-stu-id="5f6ab-191">Issue: Scaffolding MVC/Web API into a project with 5.2.0 packages results in 5.1.2 packages for ones that don't already exist in the project</span></span>

<span data-ttu-id="5f6ab-192">Aktualisieren von NuGet-Pakete für ASP.NET MVC 5.2 wird Visual Studio-Tools wie z. B. ASP.NET-Gerüstbau oder die Projektvorlage der ASP.NET Web-Anwendung nicht aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="5f6ab-192">Updating NuGet packages for ASP.NET MVC 5.2 does not update the Visual Studio tools such as ASP.NET scaffolding or the ASP.NET Web Application project template.</span></span> <span data-ttu-id="5f6ab-193">Sie verwenden die Vorgängerversion von die Laufzeitpakete für ASP.NET (z. B. 5.1.2 in Update 2).</span><span class="sxs-lookup"><span data-stu-id="5f6ab-193">They use the previous version of the ASP.NET runtime packages (e.g. 5.1.2 in Update 2).</span></span> <span data-ttu-id="5f6ab-194">Daher wird das Gerüst ASP.NET die vorherige Version (z. B. 5.1.2 in Update 2) die erforderlichen Pakete, installiert, wenn sie nicht bereits in Ihren Projekten verfügbar sind.</span><span class="sxs-lookup"><span data-stu-id="5f6ab-194">As a result, the ASP.NET scaffolding will install the previous version (e.g. 5.1.2 in Update 2) of the required packages, if they are not already available in your projects.</span></span> <span data-ttu-id="5f6ab-195">Die ASP.NET-Gerüstbau in Visual Studio 2013 RTM oder Update 1 überschreibt jedoch nicht die neuesten Pakete in Ihren Projekten.</span><span class="sxs-lookup"><span data-stu-id="5f6ab-195">However, the ASP.NET scaffolding in Visual Studio 2013 RTM or Update 1 does not overwrite the latest packages in your projects.</span></span> <span data-ttu-id="5f6ab-196">Bei Verwendung von ASP.NET-Gerüstbau nach der Aktualisierung der Pakete Ihrer Projekte zu Web-API 2.2 oder ASP.NET MVC 5.2 Stellen Sie sicher, dass die Versionen von Web-API und ASP.NET MVC konsistent sind.</span><span class="sxs-lookup"><span data-stu-id="5f6ab-196">If you use ASP.NET scaffolding after updating the packages of your projects to Web API 2.2 or ASP.NET MVC 5.2, make sure the versions of Web API and ASP.NET MVC are consistent.</span></span>

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a><span data-ttu-id="5f6ab-197">Fehlerbehebungen und kleinere Feature-Updates</span><span class="sxs-lookup"><span data-stu-id="5f6ab-197">Bug Fixes and Minor Feature Updates</span></span>

<span data-ttu-id="5f6ab-198">Diese Version umfasst auch mehrere Fehlerbehebungen und kleinere Feature Updates.</span><span class="sxs-lookup"><span data-stu-id="5f6ab-198">This release also includes several bug fixes and minor feature updates.</span></span> <span data-ttu-id="5f6ab-199">Sie können hier die vollständige Liste finden:</span><span class="sxs-lookup"><span data-stu-id="5f6ab-199">You can find the complete list here:</span></span>

- [<span data-ttu-id="5f6ab-200">5.2-Paket</span><span class="sxs-lookup"><span data-stu-id="5f6ab-200">5.2 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.2%20RC|v5.2%20RTM&assignedTo=All&component=Web%20API|Web%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="odata521"></a>
## <a name="microsoftaspnetodata-521"></a><span data-ttu-id="5f6ab-201">Microsoft.AspNet.OData 5.2.1</span><span class="sxs-lookup"><span data-stu-id="5f6ab-201">Microsoft.AspNet.OData 5.2.1</span></span>

<span data-ttu-id="5f6ab-202">Das Microsoft.AspNet.OData 5.2.1-Paket enthält die Aktualisierung von NuGet-Abhängigkeiten, aber keine Programmfehlerbehebungen bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="5f6ab-202">The Microsoft.AspNet.OData 5.2.1 package contains NuGet dependency updates but no bug fixes.</span></span> <span data-ttu-id="5f6ab-203">Mit diesem Update kann steht nicht mehr eine strenge Abhängigkeit auf Microsoft.OData.Core 6.4.0 jedoch ein einem upgrade auf eine beliebige Version 7.0.0 bis 6.4.0.</span><span class="sxs-lookup"><span data-stu-id="5f6ab-203">With this update, there is no longer a strict dependency on Microsoft.OData.Core 6.4.0, but one can upgrade to any version between 6.4.0 and 7.0.0.</span></span>

<a id="522RC"></a>
## <a name="microsoftaspnetwebapi-522"></a><span data-ttu-id="5f6ab-204">Microsoft.AspNet.WebAPI 5.2.2</span><span class="sxs-lookup"><span data-stu-id="5f6ab-204">Microsoft.AspNet.WebAPI 5.2.2</span></span>

<span data-ttu-id="5f6ab-205">In dieser Version haben wir eine abhängigkeitsänderung für `Json.Net 6.0.4`.</span><span class="sxs-lookup"><span data-stu-id="5f6ab-205">In this release we have made a dependency change for `Json.Net 6.0.4`.</span></span> <span data-ttu-id="5f6ab-206">Weitere Informationen zu neuerungen in dieser Version von `Json.NET`, finden Sie unter [Json.NET 6.0 Release 4 – JSON-Zusammenführung, Dependency Injection](http://james.newtonking.com/archive/2014/08/04/json-net-6-0-release-4-json-merge-dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="5f6ab-206">For more information on what is new in this release of `Json.NET`, see [Json.NET 6.0 Release 4 - JSON Merge, Dependency Injection](http://james.newtonking.com/archive/2014/08/04/json-net-6-0-release-4-json-merge-dependency-injection).</span></span> <span data-ttu-id="5f6ab-207">Diese Version keine anderen neuen Features oder Programmfehlerbehebungen in Web-API.</span><span class="sxs-lookup"><span data-stu-id="5f6ab-207">This release doesn't have any other new features or bug fixes in Web API.</span></span> <span data-ttu-id="5f6ab-208">Wir haben anschließend alle anderen abhängigen Pakete aktualisiert, die wir besitzen, um diese neue Version von Web-API abhängen.</span><span class="sxs-lookup"><span data-stu-id="5f6ab-208">We have subsequently updated all other dependent packages we own to depend on this new version of Web API.</span></span>

<a id="523"></a>
## <a name="microsoftaspnetwebapi-523-beta"></a><span data-ttu-id="5f6ab-209">Microsoft.AspNet.WebAPI vom Typ 5.2.3 Beta</span><span class="sxs-lookup"><span data-stu-id="5f6ab-209">Microsoft.AspNet.WebAPI 5.2.3 Beta</span></span>

<span data-ttu-id="5f6ab-210">Informieren Sie sich über die Version [hier](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx).</span><span class="sxs-lookup"><span data-stu-id="5f6ab-210">You can read about the release [here](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx).</span></span> <span data-ttu-id="5f6ab-211">Diese Version enthält nur Fehler behoben.</span><span class="sxs-lookup"><span data-stu-id="5f6ab-211">This release contains only bug fixes.</span></span> <span data-ttu-id="5f6ab-212">Sie können [dieser Abfrage](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=Web%20API&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) um die Liste der in diesem Release behobene Probleme anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="5f6ab-212">You can use [this query](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=Web%20API&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) to see the list of issues fixed in this release.</span></span>
