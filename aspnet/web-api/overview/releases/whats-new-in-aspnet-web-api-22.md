---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-22
title: Neuigkeiten in der ASP.NET Web API 2.2 | Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/25/2014
ms.topic: article
ms.assetid: 99c59ae4-167e-4f66-a6cd-d3f1098c4e4a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 400329dd852ca3c527387ee45e3e902b725e771b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
ms.locfileid: "26508399"
---
<a name="whats-new-in-aspnet-web-api-22"></a><span data-ttu-id="613c2-102">Neuigkeiten in der ASP.NET Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="613c2-102">What's New in ASP.NET Web API 2.2</span></span>
====================
<span data-ttu-id="613c2-103">durch [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="613c2-103">by [Microsoft](https://github.com/microsoft)</span></span>

<span data-ttu-id="613c2-104">Dieses Thema beschreibt die Neuigkeiten für ASP.NET Web API 2.2.</span><span class="sxs-lookup"><span data-stu-id="613c2-104">This topic describes what's new for ASP.NET Web API 2.2.</span></span>

- [<span data-ttu-id="613c2-105">Herunterladen</span><span class="sxs-lookup"><span data-stu-id="613c2-105">Download</span></span>](#download)
- [<span data-ttu-id="613c2-106">Dokumentation</span><span class="sxs-lookup"><span data-stu-id="613c2-106">Documentation</span></span>](#documentation)
- [<span data-ttu-id="613c2-107">Neue Funktionen in ASP.NET Web-API 2.2</span><span class="sxs-lookup"><span data-stu-id="613c2-107">New Features in ASP.NET Web API 2.2</span></span>](#newf)

    - [<span data-ttu-id="613c2-108">OData v4</span><span class="sxs-lookup"><span data-stu-id="613c2-108">OData v4</span></span>](#OData)
    - [<span data-ttu-id="613c2-109">-Attribut Routing Verbesserungen</span><span class="sxs-lookup"><span data-stu-id="613c2-109">Attribute Routing Improvements</span></span>](#ARI)
    - [<span data-ttu-id="613c2-110">Web-API-Client-Unterstützung für Windows Phone 8.1</span><span class="sxs-lookup"><span data-stu-id="613c2-110">Web API Client support for Windows Phone 8.1</span></span>](#phone)
- [<span data-ttu-id="613c2-111">Bekannte Probleme und aktueller Änderungen</span><span class="sxs-lookup"><span data-stu-id="613c2-111">Known Issues and Breaking Changes</span></span>](#known-issues)
- [<span data-ttu-id="613c2-112">Fehlerkorrekturen</span><span class="sxs-lookup"><span data-stu-id="613c2-112">Bug Fixes</span></span>](#bug-fixes)
- [<span data-ttu-id="613c2-113">Microsoft.AspNet.OData 5.2.1</span><span class="sxs-lookup"><span data-stu-id="613c2-113">Microsoft.AspNet.OData 5.2.1</span></span>](#odata521)
- [<span data-ttu-id="613c2-114">Microsoft.AspNet.WebAPI 5.2.2</span><span class="sxs-lookup"><span data-stu-id="613c2-114">Microsoft.AspNet.WebAPI 5.2.2</span></span>](#522RC)
- [<span data-ttu-id="613c2-115">Microsoft.AspNet.WebAPI 5.2.3 Beta</span><span class="sxs-lookup"><span data-stu-id="613c2-115">Microsoft.AspNet.WebAPI 5.2.3 Beta</span></span>](#523)

<a id="download"></a>
## <a name="download"></a><span data-ttu-id="613c2-116">Herunterladen</span><span class="sxs-lookup"><span data-stu-id="613c2-116">Download</span></span>

<span data-ttu-id="613c2-117">Die Common Language Runtime-Funktionen werden als NuGet-Pakete auf der NuGet Gallery zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="613c2-117">The runtime features are released as NuGet packages on the NuGet gallery.</span></span> <span data-ttu-id="613c2-118">Alle Pakete für die Laufzeit führen Sie die [Semantischer Versionsverwaltung](http://semver.org/) Spezifikation.</span><span class="sxs-lookup"><span data-stu-id="613c2-118">All the runtime packages follow the [Semantic Versioning](http://semver.org/) specification.</span></span> <span data-ttu-id="613c2-119">Das aktuellste ASP.NET Web API-2.2-Paket ist die folgende Version: "5.2.0".</span><span class="sxs-lookup"><span data-stu-id="613c2-119">The latest ASP.NET Web API 2.2 package has the following version: "5.2.0".</span></span> <span data-ttu-id="613c2-120">Installieren oder aktualisieren Sie diese Pakete über [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/).</span><span class="sxs-lookup"><span data-stu-id="613c2-120">You can install or update these packages through [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/).</span></span> <span data-ttu-id="613c2-121">Die Version umfasst auch die entsprechende lokalisierte Pakete für NuGet.</span><span class="sxs-lookup"><span data-stu-id="613c2-121">The release also includes corresponding localized packages on NuGet.</span></span>

<span data-ttu-id="613c2-122">Installieren und für die veröffentlichten NuGet-Pakete über NuGet-Paket-Manager-Konsole aktualisieren zu können:</span><span class="sxs-lookup"><span data-stu-id="613c2-122">You can install or update to the released NuGet packages by using the NuGet Package Manager Console:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-22/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a><span data-ttu-id="613c2-123">Dokumentation</span><span class="sxs-lookup"><span data-stu-id="613c2-123">Documentation</span></span>

<span data-ttu-id="613c2-124">Lernprogramme und Weitere Informationen zu ASP.NET Web API 2.2 stehen auf der Website für ASP.NET ([https://www.asp.net/web-api](../../index.md)).</span><span class="sxs-lookup"><span data-stu-id="613c2-124">Tutorials and other information about ASP.NET Web API 2.2 are available from the ASP.NET web site ([https://www.asp.net/web-api](../../index.md)).</span></span>

<a id="newf"></a>
## <a name="new-features-in-aspnet-web-api-22"></a><span data-ttu-id="613c2-125">Neue Funktionen in ASP.NET Web-API 2.2</span><span class="sxs-lookup"><span data-stu-id="613c2-125">New Features in ASP.NET Web API 2.2</span></span>

<a id="OData"></a>
### <a name="odata-v4"></a><span data-ttu-id="613c2-126">OData v4</span><span class="sxs-lookup"><span data-stu-id="613c2-126">OData v4</span></span>

<span data-ttu-id="613c2-127">Diese Version bietet Unterstützung für OData v4-Protokoll.</span><span class="sxs-lookup"><span data-stu-id="613c2-127">This release adds support for the OData v4 protocol.</span></span> <span data-ttu-id="613c2-128">Weitere Informationen finden Sie unter der [Web API OData v4-Dokumentation.](../odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint.md)</span><span class="sxs-lookup"><span data-stu-id="613c2-128">For more information, see the [Web API OData v4 documentation.](../odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint.md)</span></span>

<span data-ttu-id="613c2-129">Hier sind einige der wichtigsten Funktionen und Änderungen für OData v4:</span><span class="sxs-lookup"><span data-stu-id="613c2-129">Here are some of the key features and changes for OData v4:</span></span>

- [<span data-ttu-id="613c2-130">Unterstützung für Aliasing-Eigenschaften in OData-Modell</span><span class="sxs-lookup"><span data-stu-id="613c2-130">Support for aliasing properties in OData model</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataModelAliasingSample/)
- [<span data-ttu-id="613c2-131">Unterstützung für ComplexTypeAttribute, AssociationAttribute, TimesTampAttribute und ConcurrencyCheckAttribute in ODataConventionModelBuilder</span><span class="sxs-lookup"><span data-stu-id="613c2-131">Support for ComplexTypeAttribute, AssociationAttribute, TimesTampAttribute and ConcurrencyCheckAttribute in ODataConventionModelBuilder</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEtagSample/)
- [<span data-ttu-id="613c2-132">Geben Sie die angezeigten Titel für Aktionen ermöglichen</span><span class="sxs-lookup"><span data-stu-id="613c2-132">Provide ability to supply friendly Title for actions</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataActionsSample/)
- <span data-ttu-id="613c2-133">ODL-UriParser integriert</span><span class="sxs-lookup"><span data-stu-id="613c2-133">Integrate with ODL UriParser</span></span>
- <span data-ttu-id="613c2-134">Unterstützung für [Enum](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEnumTypeSample/ODataEnumTypeSample/), [Containment](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/) und [Singleton](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataSingletonSample/)</span><span class="sxs-lookup"><span data-stu-id="613c2-134">Support for [enum](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEnumTypeSample/ODataEnumTypeSample/), [containment](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/) and [singleton](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataSingletonSample/)</span></span>
- <span data-ttu-id="613c2-135">Unterstützung für primitive Typen umwandeln</span><span class="sxs-lookup"><span data-stu-id="613c2-135">Support cast for primitive types</span></span>
- [<span data-ttu-id="613c2-136">Unterstützung für OData-Funktion</span><span class="sxs-lookup"><span data-stu-id="613c2-136">Added OData function support</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [<span data-ttu-id="613c2-137">Parameteraliase für Funktionsaufrufe zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="613c2-137">Support parameter aliases for function calls</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [<span data-ttu-id="613c2-138">Camel-Case Benennungskonvention im Modell unterstützen</span><span class="sxs-lookup"><span data-stu-id="613c2-138">Support camel case naming convention in model</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataCamelCaseSample/)
- <span data-ttu-id="613c2-139">Unterstützung für cast() in $filter</span><span class="sxs-lookup"><span data-stu-id="613c2-139">Support for cast() in $filter</span></span>
- <span data-ttu-id="613c2-140">Unterstützung für offenen komplexer Typ</span><span class="sxs-lookup"><span data-stu-id="613c2-140">Support for open complex type</span></span>
- <span data-ttu-id="613c2-141">Entfernte EntitySetController und AsyncEntitySetController</span><span class="sxs-lookup"><span data-stu-id="613c2-141">Removed EntitySetController and AsyncEntitySetController</span></span>
- [<span data-ttu-id="613c2-142">Geänderte $link zu $ref</span><span class="sxs-lookup"><span data-stu-id="613c2-142">Changed $link to $ref</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataServiceSample/)
- [<span data-ttu-id="613c2-143">Hinzugefügte Attribut routing-Unterstützung</span><span class="sxs-lookup"><span data-stu-id="613c2-143">Added Attribute routing support</span></span>](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataAttributeRoutingSample/)
- <span data-ttu-id="613c2-144">OData-Core-Bibliotheken 6.4.0 verwendet</span><span class="sxs-lookup"><span data-stu-id="613c2-144">Uses OData Core Libraries 6.4.0</span></span>

<a id="ARI"></a>
### <a name="attribute-routing-improvements"></a><span data-ttu-id="613c2-145">-Attribut Routing Verbesserungen</span><span class="sxs-lookup"><span data-stu-id="613c2-145">Attribute Routing Improvements</span></span>

<span data-ttu-id="613c2-146">Routing-Attribut jetzt bietet einen Erweiterungspunkt aufgerufen IDirectRouteProvider, die vollständige Kontrolle über wie attributenrouten erkannt und konfiguriert werden kann.</span><span class="sxs-lookup"><span data-stu-id="613c2-146">Attribute Routing now provides an extensibility point called IDirectRouteProvider, which allows full control over how attribute routes are discovered and configured.</span></span> <span data-ttu-id="613c2-147">Ein IDirectRouteProvider ist verantwortlich für die Bereitstellung einer Liste von Aktionen und Controller zusammen mit zugehörigen Routeninformationen an genau welche Routingkonfiguration für diese Aktionen gewünscht wird.</span><span class="sxs-lookup"><span data-stu-id="613c2-147">An IDirectRouteProvider is responsible for providing a list of actions and controllers along with associated route information to specify exactly what routing configuration is desired for those actions.</span></span> <span data-ttu-id="613c2-148">Eine Implementierung IDirectRouteProvider kann angegeben werden, wenn MapAttributes/MapHttpAttributeRoutes aufrufen.</span><span class="sxs-lookup"><span data-stu-id="613c2-148">An IDirectRouteProvider implementation may be specified when calling MapAttributes/MapHttpAttributeRoutes.</span></span>

<span data-ttu-id="613c2-149">Anpassen von IDirectRouteProvider werden einfachste durch unsere Standardimplementierung DefaultDirectRouteProvider erweitern.</span><span class="sxs-lookup"><span data-stu-id="613c2-149">Customizing IDirectRouteProvider will be easiest by extending our default implementation, DefaultDirectRouteProvider.</span></span> <span data-ttu-id="613c2-150">Diese Klasse stellt separate überschreibbare virtuelle Methoden, um die Logik zum Erkennen von Attribute, routeneinträge erstellen und durch die Ermittlung Routenpräfix und Bereichspräfix zu ändern.</span><span class="sxs-lookup"><span data-stu-id="613c2-150">This class provides separate overridable virtual methods to change the logic for discovering attributes, creating route entries, and discovering route prefix and area prefix.</span></span>

<span data-ttu-id="613c2-151">Es folgen einige Beispiele für mit dieser neuen Erweiterungspunkt zugesagten:</span><span class="sxs-lookup"><span data-stu-id="613c2-151">Following are some examples on what you could do with this new extensibility point:</span></span>

1. <span data-ttu-id="613c2-152">Unterstützung von Vererbung Route-Attribute</span><span class="sxs-lookup"><span data-stu-id="613c2-152">Support inheritance of Route attributes</span></span>

    <span data-ttu-id="613c2-153">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="613c2-153">Example:</span></span>

    <span data-ttu-id="613c2-154">Hier würden like "/ api/Werte/10" erfolgreich "Erfolg: 10" zurück</span><span class="sxs-lookup"><span data-stu-id="613c2-154">Here a request like "/api/values/10" would successfully return "Success:10"</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample2.cs)]
2. <span data-ttu-id="613c2-155">Geben Sie einen Standardnamen für die Route für die attributenrouten anhand einiger Konvention, die Ihnen gefallen.</span><span class="sxs-lookup"><span data-stu-id="613c2-155">Provide a default route name for your attribute routes by following some convention you like.</span></span> <span data-ttu-id="613c2-156">Standardmäßig erstellt routing-Attribut nicht automatisch Namen für die attributenrouten.</span><span class="sxs-lookup"><span data-stu-id="613c2-156">By default, attribute routing doesn't automatically create names for attribute routes.</span></span>
3. <span data-ttu-id="613c2-157">Ändern Sie attributenrouten routenvorlage an einer zentralen Stelle aus, bevor sie in der Routingtabelle enden.</span><span class="sxs-lookup"><span data-stu-id="613c2-157">Modify attribute routes' route template at one central place before they end up in the route table.</span></span>

<a id="phone"></a>
### <a name="web-api-client-support-for-windows-phone-81"></a><span data-ttu-id="613c2-158">Web-API-Client-Unterstützung für Windows Phone 8.1</span><span class="sxs-lookup"><span data-stu-id="613c2-158">Web API Client Support for Windows Phone 8.1</span></span>

<span data-ttu-id="613c2-159">Sie können nun die Web-API-Client-NuGet-Paket verwenden, Ihre Web-API-Client-Logik implementieren, Windows Phone 8.1 abzielt oder vom in eine universelle App.</span><span class="sxs-lookup"><span data-stu-id="613c2-159">You can now use the Web API Client NuGet package to implement your Web API client logic when targeting Windows Phone 8.1 or from within a Universal App.</span></span>

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="613c2-160">Bekannte Probleme und aktueller Änderungen</span><span class="sxs-lookup"><span data-stu-id="613c2-160">Known Issues and Breaking Changes</span></span>

<span data-ttu-id="613c2-161">Dieser Abschnitt beschreibt bekannte Probleme und neueste Änderungen in der ASP.NET Web API-2.2.</span><span class="sxs-lookup"><span data-stu-id="613c2-161">This section describes known issues and breaking changes in the ASP.NET Web API 2.2.</span></span>

### <a name="odata-v4"></a><span data-ttu-id="613c2-162">OData v4</span><span class="sxs-lookup"><span data-stu-id="613c2-162">OData v4</span></span>

#### <a name="model-builder"></a><span data-ttu-id="613c2-163">Modellgenerator</span><span class="sxs-lookup"><span data-stu-id="613c2-163">Model builder</span></span>

<span data-ttu-id="613c2-164">Problem: Überladene Funktionen konnte nicht als FunctionImport verfügbar gemacht werden</span><span class="sxs-lookup"><span data-stu-id="613c2-164">Issue: Overloaded Functions could not be exposed as FunctionImport</span></span>

<span data-ttu-id="613c2-165">Wenn 2 überladene Funktionen vorhanden sind, und sie unterscheiden sich auch um FunctionImport wie unten Anfordern von ~/GetAllConventionCustomers(CustomerName={customerName}) führt System.InvalidOperationException.</span><span class="sxs-lookup"><span data-stu-id="613c2-165">If there are 2 overloaded functions and they are also FunctionImport as shown below then requesting ~/GetAllConventionCustomers(CustomerName={customerName}) results in System.InvalidOperationException.</span></span>

[!code-xml[Main](whats-new-in-aspnet-web-api-22/samples/sample3.xml)]

<span data-ttu-id="613c2-166">Problemumgehung: Die problemumgehung für dieses Problem ist sowohl die funktionsüberladungen als FunctionImports hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="613c2-166">Workaround: The workaround for this issue is to add both the function overloads as FunctionImports.</span></span>

#### <a name="odata-routing"></a><span data-ttu-id="613c2-167">OData-Routing</span><span class="sxs-lookup"><span data-stu-id="613c2-167">OData Routing</span></span>

<span data-ttu-id="613c2-168">Zeichenfolgenliterale, die die URL-codierte Schrägstrich (% 2F) und backslash(%5C) 404-Fehler verursachen, wenn sie in den Ressourcenpfaden OData verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="613c2-168">String literals that include the URL encoded slash (%2F), and backslash(%5C) cause a 404 error when they are used in the OData resource paths.</span></span>

<span data-ttu-id="613c2-169">Zeichenfolgenliterale können z. B. als Parameter von Funktionen oder Schlüsselwerte der angegebenen Entitätenmengen in den Ressourcenpfaden OData verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="613c2-169">For example, string literals can be used in the OData resource paths as parameters of functions or key values of entity sets.</span></span>

<span data-ttu-id="613c2-170">/Employees/Total.GetCount(Name='Name%2F')</span><span class="sxs-lookup"><span data-stu-id="613c2-170">/Employees/Total.GetCount(Name='Name%2F')</span></span>

<span data-ttu-id="613c2-171">/Employees('Name%5C')</span><span class="sxs-lookup"><span data-stu-id="613c2-171">/Employees('Name%5C')</span></span>

<span data-ttu-id="613c2-172">Erhalt Dienste solche Anforderungen Hosts wird Escapezeichen Escapesequenzen die fest, bevor sie an die Web-API-Runtime übergeben.</span><span class="sxs-lookup"><span data-stu-id="613c2-172">When services receive such requests the hosts will un-escape those escape sequences before passing them to the Web API runtime.</span></span> <span data-ttu-id="613c2-173">Dies schützt vor Angriffen wie folgt:</span><span class="sxs-lookup"><span data-stu-id="613c2-173">This protects against attacks like the following:</span></span>  
  
 <span data-ttu-id="613c2-174">http://www.contoso.com/..%2f..%2f/Windows/System32/cmd.exe?/c+dir+c:</span><span class="sxs-lookup"><span data-stu-id="613c2-174">http://www.contoso.com/..%2F..%2F/Windows/System32/cmd.exe?/c+dir+c:</span></span>

<span data-ttu-id="613c2-175">Dies bewirkt, dass der Stapel Web API OData-Fehler 404 (Nichtgefunden) zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="613c2-175">This causes the Web API OData stack to return a 404 error (Not Found).</span></span> <span data-ttu-id="613c2-176">Um diesen Fehler zu verhindern, sollte der Client verwenden, die doppelte Escapesequenzen für Schrägstrich ("% 252F") und umgekehrter Schrägstrich ("%" 255 C).</span><span class="sxs-lookup"><span data-stu-id="613c2-176">To prevent this error, your client should use the double escape sequences for slash (%252F) and backslash (%255C).</span></span> <span data-ttu-id="613c2-177">Dies geschieht nicht für Abfragezeichenfolgen, z. B. /Employees? $filter = Name Eq "Namen % 2F"</span><span class="sxs-lookup"><span data-stu-id="613c2-177">This does not happen for query strings such as /Employees?$filter=Name eq 'Name%2F'</span></span>

<span data-ttu-id="613c2-178">**Notieren Sie sich ohne Escapezeichen Schrägstriche ('/'), und umgekehrte Schrägstriche (") sind nicht zulässig in OData-Ressource Pfad Zeichenfolgenliteralen. Schrägstriche sollte nur als Pfadtrennzeichen angezeigt werden und umgekehrte Schrägstriche sollte nicht auf allen in den Pfad der OData-Ressource angezeigt. (Beide sind in einigen Teilen einer OData-Abfragezeichenfolge verwendet werden kann.)**</span><span class="sxs-lookup"><span data-stu-id="613c2-178">**Note un-escaped slashes ('/') and backslashes ('') are not legal in OData resource path string literals. Slashes should appear only as path separators and backslashes should not appear in the OData resource path at all. (Both are usable in some portions of an OData query string.)**</span></span>

<span data-ttu-id="613c2-179">Problemumgehung: Sie könnten die Parse-Methode der DefaultODataPathHandler mit dem Schrägstrich und den umgekehrten Schrägstrich in Zeichenfolgenliteralen Escapezeichen vor der Analyse tatsächlich diese überschreiben.</span><span class="sxs-lookup"><span data-stu-id="613c2-179">Workaround: You could override the Parse method of DefaultODataPathHandler to escape the slash and backslash in string literals before actually parsing them.</span></span> <span data-ttu-id="613c2-180">Sie finden ein Beispiel dieses Ansatzes besteht.</span><span class="sxs-lookup"><span data-stu-id="613c2-180">You can find a sample of this approach here.</span></span>

### <a name="odata-v3"></a><span data-ttu-id="613c2-181">OData v3</span><span class="sxs-lookup"><span data-stu-id="613c2-181">OData v3</span></span>

#### <a name="queryable"></a><span data-ttu-id="613c2-182">[Abfragbare]</span><span class="sxs-lookup"><span data-stu-id="613c2-182">[Queryable]</span></span>

<span data-ttu-id="613c2-183">Das Attribut [Queryable] ist veraltet.</span><span class="sxs-lookup"><span data-stu-id="613c2-183">The [Queryable] attribute is deprecated.</span></span> <span data-ttu-id="613c2-184">Neue OData v3-Anwendungen die zu verwendende **System.Web.Http.OData.EnableQueryAttribute**.</span><span class="sxs-lookup"><span data-stu-id="613c2-184">New OData v3 applications should use **System.Web.Http.OData.EnableQueryAttribute**.</span></span>

<span data-ttu-id="613c2-185">Die **ODataHttpConfigurationExtensions.EnableQuerySupport** Erweiterungsmethode fügt nun ein **EnableQueryAttribute** auf die globale filterauflistung.</span><span class="sxs-lookup"><span data-stu-id="613c2-185">The **ODataHttpConfigurationExtensions.EnableQuerySupport** extension method now adds an **EnableQueryAttribute** to the global filter collection.</span></span> <span data-ttu-id="613c2-186">Wenn alle Domänencontroller haben die **[Queryable]** Attribut aufrufen `config.EnableQuerySupport()` führt dazu, dass die **[Queryable]** Attribut fehlschlägt</span><span class="sxs-lookup"><span data-stu-id="613c2-186">If any controllers have the **[Queryable]** attribute, calling `config.EnableQuerySupport()` will cause the **[Queryable]** attribute to fail</span></span>

<span data-ttu-id="613c2-187">Die empfohlene Methode zum Beheben dieses Problems ist, zum Ersetzen aller Instanzen des **queryableattribute überprüft** mit **System.Web.Http.OData.EnableQueryAttribute**.</span><span class="sxs-lookup"><span data-stu-id="613c2-187">The recommended way to resolve this issue is to replace all instances of **QueryableAttribute** with **System.Web.Http.OData.EnableQueryAttribute**.</span></span>

<span data-ttu-id="613c2-188">Eine alternative problemumgehung besteht darin, den folgenden Code in Ihrer Web-API-Konfiguration verwenden:</span><span class="sxs-lookup"><span data-stu-id="613c2-188">An alternative workaround is to use the following code in your Web API configuration:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample4.cs)]

### <a name="attribute-routing"></a><span data-ttu-id="613c2-189">Routing-Attribut</span><span class="sxs-lookup"><span data-stu-id="613c2-189">Attribute Routing</span></span>

<span data-ttu-id="613c2-190">Problem: Wurden die modellbindung des komplexen Typs ab, mit FromUri-Attribut ergänzt wird, verhält sich anders bei Verwendung der Routing-Attribut.</span><span class="sxs-lookup"><span data-stu-id="613c2-190">Issue: Model binding of complex type which is decorated with FromUri attribute behaves differently when using Attribute Routing.</span></span>

<span data-ttu-id="613c2-191">Folgenden Link besteht im Nachverfolgen des Problems auch und Details für dieses Problem zu umgehen.</span><span class="sxs-lookup"><span data-stu-id="613c2-191">Following link is tracking the issue and also has details about a workaround.</span></span>  
[<span data-ttu-id="613c2-192">http://aspnetwebstack.codeplex.com/WorkItem/1944</span><span class="sxs-lookup"><span data-stu-id="613c2-192">http://aspnetwebstack.codeplex.com/workitem/1944</span></span>](http://aspnetwebstack.codeplex.com/workitem/1944)

<span data-ttu-id="613c2-193">Problem: Gerüstbau MVC/Web-API in einem Projekt mit 5.2.0 Pakete führt 5.1.2 Pakete für Argumente, die nicht bereits im Projekt vorhanden sind</span><span class="sxs-lookup"><span data-stu-id="613c2-193">Issue: Scaffolding MVC/Web API into a project with 5.2.0 packages results in 5.1.2 packages for ones that don't already exist in the project</span></span>

<span data-ttu-id="613c2-194">Aktualisieren von NuGet-Pakete für ASP.NET MVC 5.2 aktualisiert nicht Visual Studio-Tools wie z. B. ASP.NET Gerüstbau oder die Projektvorlage für ASP.NET Web-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="613c2-194">Updating NuGet packages for ASP.NET MVC 5.2 does not update the Visual Studio tools such as ASP.NET scaffolding or the ASP.NET Web Application project template.</span></span> <span data-ttu-id="613c2-195">Sie verwenden die vorherige Version von ASP.NET Common Language Runtime-Pakete (z. B. 5.1.2 in Update 2).</span><span class="sxs-lookup"><span data-stu-id="613c2-195">They use the previous version of the ASP.NET runtime packages (e.g. 5.1.2 in Update 2).</span></span> <span data-ttu-id="613c2-196">Daher wird das Gerüst ASP.NET die Vorgängerversion (z. B. 5.1.2 in Update 2) die erforderlichen Pakete installiert, wenn sie nicht bereits in Ihren Projekten vorhanden sind.</span><span class="sxs-lookup"><span data-stu-id="613c2-196">As a result, the ASP.NET scaffolding will install the previous version (e.g. 5.1.2 in Update 2) of the required packages, if they are not already available in your projects.</span></span> <span data-ttu-id="613c2-197">Das Gerüst ASP.NET in Visual Studio 2013 RTM oder Update 1 werden jedoch nicht die neuesten Pakete in Ihren Projekten überschrieben.</span><span class="sxs-lookup"><span data-stu-id="613c2-197">However, the ASP.NET scaffolding in Visual Studio 2013 RTM or Update 1 does not overwrite the latest packages in your projects.</span></span> <span data-ttu-id="613c2-198">Wenn Sie ASP.NET Gerüstbau nach dem Aktualisieren die Pakete der Projekte auf Web-API 2.2 oder ASP.NET MVC 5.2 verwenden, stellen Sie sicher, dass die Versionen der Web-API und ASP.NET MVC konsistent sind.</span><span class="sxs-lookup"><span data-stu-id="613c2-198">If you use ASP.NET scaffolding after updating the packages of your projects to Web API 2.2 or ASP.NET MVC 5.2, make sure the versions of Web API and ASP.NET MVC are consistent.</span></span>

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a><span data-ttu-id="613c2-199">Fehlerbehebungen und kleinere Feature-Updates</span><span class="sxs-lookup"><span data-stu-id="613c2-199">Bug Fixes and Minor Feature Updates</span></span>

<span data-ttu-id="613c2-200">Diese Version enthält auch mehrere Programmfehlerbehebungen und kleinere Funktion Updates.</span><span class="sxs-lookup"><span data-stu-id="613c2-200">This release also includes several bug fixes and minor feature updates.</span></span> <span data-ttu-id="613c2-201">Sie können die vollständige Liste hier finden:</span><span class="sxs-lookup"><span data-stu-id="613c2-201">You can find the complete list here:</span></span>

- [<span data-ttu-id="613c2-202">5.2-Paket</span><span class="sxs-lookup"><span data-stu-id="613c2-202">5.2 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.2%20RC|v5.2%20RTM&assignedTo=All&component=Web%20API|Web%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="odata521"></a>
## <a name="microsoftaspnetodata-521"></a><span data-ttu-id="613c2-203">Microsoft.AspNet.OData 5.2.1</span><span class="sxs-lookup"><span data-stu-id="613c2-203">Microsoft.AspNet.OData 5.2.1</span></span>

<span data-ttu-id="613c2-204">Das Microsoft.AspNet.OData 5.2.1-Paket enthält, aber keine Fehlerkorrekturen NuGet-Abhängigkeit Updates.</span><span class="sxs-lookup"><span data-stu-id="613c2-204">The Microsoft.AspNet.OData 5.2.1 package contains NuGet dependency updates but no bug fixes.</span></span> <span data-ttu-id="613c2-205">Mit diesem Update können auf Microsoft.OData.Core 6.4.0 mehr eine strikte Abhängigkeit vorhanden ist, aber eine kann auf eine beliebige Version zwischen 6.4.0 und 7.0.0 aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="613c2-205">With this update, there is no longer a strict dependency on Microsoft.OData.Core 6.4.0, but one can upgrade to any version between 6.4.0 and 7.0.0.</span></span>

<a id="522RC"></a>
## <a name="microsoftaspnetwebapi-522"></a><span data-ttu-id="613c2-206">Microsoft.AspNet.WebAPI 5.2.2</span><span class="sxs-lookup"><span data-stu-id="613c2-206">Microsoft.AspNet.WebAPI 5.2.2</span></span>

<span data-ttu-id="613c2-207">In dieser Version haben wir eine Abhängigkeit für ändern `Json.Net 6.0.4`.</span><span class="sxs-lookup"><span data-stu-id="613c2-207">In this release we have made a dependency change for `Json.Net 6.0.4`.</span></span> <span data-ttu-id="613c2-208">Weitere Informationen zu in dieser Version von Neuigkeiten `Json.NET`, finden Sie unter [Json.NET 6.0 Version 4: Zusammenführen von JSON-Abhängigkeitsinjektion](http://james.newtonking.com/archive/2014/08/04/json-net-6-0-release-4-json-merge-dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="613c2-208">For more information on what is new in this release of `Json.NET`, see [Json.NET 6.0 Release 4 - JSON Merge, Dependency Injection](http://james.newtonking.com/archive/2014/08/04/json-net-6-0-release-4-json-merge-dependency-injection).</span></span> <span data-ttu-id="613c2-209">Diese Version keine anderen neuen Features oder Fehlerkorrekturen in der Web-API.</span><span class="sxs-lookup"><span data-stu-id="613c2-209">This release doesn't have any other new features or bug fixes in Web API.</span></span> <span data-ttu-id="613c2-210">Wir haben anschließend alle anderen abhängigen Pakete aktualisiert, die wir besitzen, um diese neue Version von Web-API abhängig.</span><span class="sxs-lookup"><span data-stu-id="613c2-210">We have subsequently updated all other dependent packages we own to depend on this new version of Web API.</span></span>

<a id="523"></a>
## <a name="microsoftaspnetwebapi-523-beta"></a><span data-ttu-id="613c2-211">Microsoft.AspNet.WebAPI 5.2.3 Beta</span><span class="sxs-lookup"><span data-stu-id="613c2-211">Microsoft.AspNet.WebAPI 5.2.3 Beta</span></span>

<span data-ttu-id="613c2-212">Informieren Sie sich über die Version [hier](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx).</span><span class="sxs-lookup"><span data-stu-id="613c2-212">You can read about the release [here](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx).</span></span> <span data-ttu-id="613c2-213">Diese Version enthält nur Fehlerkorrekturen.</span><span class="sxs-lookup"><span data-stu-id="613c2-213">This release contains only bug fixes.</span></span> <span data-ttu-id="613c2-214">Sie können [dieser Abfrage](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=Web%20API&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) auf die Liste der in dieser Version behobene Probleme anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="613c2-214">You can use [this query](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=Web%20API&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) to see the list of issues fixed in this release.</span></span>
