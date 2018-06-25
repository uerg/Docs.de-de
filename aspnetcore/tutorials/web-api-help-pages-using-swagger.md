---
title: ASP.NET Core-Web-API-Hilfeseiten mit Swagger und Open API
author: rsuter
description: Dieses Tutorial enthält eine exemplarische Vorgehensweise für das Hinzufügen von Swagger, um Dokumentationen und Hilfeseiten für eine Web-API-App zu generieren.
ms.author: scaddie
ms.custom: mvc
ms.date: 03/09/2018
uid: tutorials/web-api-help-pages-using-swagger
ms.openlocfilehash: 56e146337ad9e94298f72abf5ede009eea65fb46
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272250"
---
# <a name="aspnet-core-web-api-help-pages-with-swagger--open-api"></a><span data-ttu-id="ff6b8-103">ASP.NET Core-Web-API-Hilfeseiten mit Swagger und Open API</span><span class="sxs-lookup"><span data-stu-id="ff6b8-103">ASP.NET Core Web API help pages with Swagger / Open API</span></span>

<span data-ttu-id="ff6b8-104">Von [Christoph Nienaber](https://twitter.com/zuckerthoben) und [Rico Suter](http://rsuter.com)</span><span class="sxs-lookup"><span data-stu-id="ff6b8-104">By [Christoph Nienaber](https://twitter.com/zuckerthoben) and [Rico Suter](http://rsuter.com)</span></span>

<span data-ttu-id="ff6b8-105">Wenn eine Web-API verwendet wird, fällt es dem Entwickler oft nicht leicht, die zahlreichen Methoden zu verstehen.</span><span class="sxs-lookup"><span data-stu-id="ff6b8-105">When consuming a Web API, understanding its various methods can be challenging for a developer.</span></span> <span data-ttu-id="ff6b8-106">[Swagger](https://swagger.io/), auch als Open API bekannt, löst das Problem, das das Generieren von nützlichen Dokumentationen und Hilfeseiten für Web-APIs darstellen kann.</span><span class="sxs-lookup"><span data-stu-id="ff6b8-106">[Swagger](https://swagger.io/), also known as Open API, solves the problem of generating useful documentation and help pages for Web APIs.</span></span> <span data-ttu-id="ff6b8-107">Das Programm bietet Vorteile wie die interaktive Dokumentation, die Generierung von Client SDKs und die Erkennbarkeit von APIs.</span><span class="sxs-lookup"><span data-stu-id="ff6b8-107">It provides benefits such as interactive documentation, client SDK generation, and API discoverability.</span></span>

<span data-ttu-id="ff6b8-108">In diesem Artikel werden die Swagger-Implementierungen [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) und [NSwag](https://github.com/RSuter/NSwag) von .NET veranschaulicht:</span><span class="sxs-lookup"><span data-stu-id="ff6b8-108">In this article, the [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) and [NSwag](https://github.com/RSuter/NSwag) .NET Swagger implementations are showcased:</span></span>

* <span data-ttu-id="ff6b8-109">**Swashbuckle.AspNetCore** ist ein Open Source-Projekt zum Generieren von Swagger-Dokumenten für Web-APIs von ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ff6b8-109">**Swashbuckle.AspNetCore** is an open source project for generating Swagger documents for ASP.NET Core Web APIs.</span></span>

* <span data-ttu-id="ff6b8-110">Bei **NSwag** handelt es sich um ein weiteres Open Source-Projekt zum Integrieren der [Swagger-Benutzeroberfläche](https://swagger.io/swagger-ui/) oder von [ReDoc](https://github.com/Rebilly/ReDoc) in ASP.NET Core-Web-APIs.</span><span class="sxs-lookup"><span data-stu-id="ff6b8-110">**NSwag** is another open source project for integrating [Swagger UI](https://swagger.io/swagger-ui/) or [ReDoc](https://github.com/Rebilly/ReDoc) into ASP.NET Core Web APIs.</span></span> <span data-ttu-id="ff6b8-111">Es bietet Methoden, um C#- und TypeScript-Clientcode für Ihre API zu generieren.</span><span class="sxs-lookup"><span data-stu-id="ff6b8-111">It offers approaches to generate C# and TypeScript client code for your API.</span></span>

## <a name="what-is-swagger--open-api"></a><span data-ttu-id="ff6b8-112">Was ist Swagger bzw. Open API?</span><span class="sxs-lookup"><span data-stu-id="ff6b8-112">What is Swagger / Open API?</span></span>

<span data-ttu-id="ff6b8-113">Bei Swagger handelt es sich um eine sprachunabhängige Spezifikation für das Beschreiben von [REST-APIs](https://en.wikipedia.org/wiki/Representational_state_transfer).</span><span class="sxs-lookup"><span data-stu-id="ff6b8-113">Swagger is a language-agnostic specification for describing [REST](https://en.wikipedia.org/wiki/Representational_state_transfer) APIs.</span></span> <span data-ttu-id="ff6b8-114">Das Swagger-Projekt wurde an die Initiative [OpenAPI](https://www.openapis.org/) gespendet. Dort wird es nun als „Open API“ bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="ff6b8-114">The Swagger project was donated to the [OpenAPI Initiative](https://www.openapis.org/), where it's now referred to as Open API.</span></span> <span data-ttu-id="ff6b8-115">Beide Namen werden synonym verwendet, Open API wird jedoch bevorzugt.</span><span class="sxs-lookup"><span data-stu-id="ff6b8-115">Both names are used interchangeably; however, Open API is preferred.</span></span> <span data-ttu-id="ff6b8-116">Dadurch können Computer und Benutzer die Funktionen eines Diensts ohne direkten Zugriff auf die Implementierung (Quellcode, Netzwerkzugriff, Dokumentation) nachvollziehen.</span><span class="sxs-lookup"><span data-stu-id="ff6b8-116">It allows both computers and humans to understand the capabilities of a service without any direct access to the implementation (source code, network access, documentation).</span></span> <span data-ttu-id="ff6b8-117">Ein Ziel besteht im Minimieren des Arbeitsaufwands, der zum Verbinden von getrennten Diensten erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="ff6b8-117">One goal is to minimize the amount of work needed to connect disassociated services.</span></span> <span data-ttu-id="ff6b8-118">Ein weiteres Ziel besteht darin, den Zeitaufwand zu verringern, der für die genaue Dokumentation eines Diensts erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="ff6b8-118">Another goal is to reduce the amount of time needed to accurately document a service.</span></span>

## <a name="swagger-specification-swaggerjson"></a><span data-ttu-id="ff6b8-119">Spezifikation von Swagger (swagger.json)</span><span class="sxs-lookup"><span data-stu-id="ff6b8-119">Swagger specification (swagger.json)</span></span>

<span data-ttu-id="ff6b8-120">Im Zentrum des Swagger-Flows steht die Swagger-Spezifikation. Diese ist standardmäßig ein Dokument namens *swagger.json*.</span><span class="sxs-lookup"><span data-stu-id="ff6b8-120">The core to the Swagger flow is the Swagger specification&mdash;by default, a document named *swagger.json*.</span></span> <span data-ttu-id="ff6b8-121">Dieses wird je nachdem, welchen Dienst Sie verwenden, von der Swagger-Toolkette (oder von einer Drittanbieterimplementierung davon) generiert.</span><span class="sxs-lookup"><span data-stu-id="ff6b8-121">It's generated by the Swagger tool chain (or third-party implementations of it) based on your service.</span></span> <span data-ttu-id="ff6b8-122">Es beschreibt die Funktionen Ihrer API und wie auf diese mit HTTP zugegriffen werden kann.</span><span class="sxs-lookup"><span data-stu-id="ff6b8-122">It describes the capabilities of your API and how to access it with HTTP.</span></span> <span data-ttu-id="ff6b8-123">Es führt die Swagger-Benutzeroberfläche aus und wird von der Toolkette verwendet, um die Ermittlung und die Generierung von Clientcode zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="ff6b8-123">It drives the Swagger UI and is used by the tool chain to enable discovery and client code generation.</span></span> <span data-ttu-id="ff6b8-124">Hier finden Sie ein Beispiel der Swagger-Spezifikation, das aus Gründen der Übersichtlichkeit reduziert wurde:</span><span class="sxs-lookup"><span data-stu-id="ff6b8-124">Here's an example of a Swagger specification, reduced for brevity:</span></span>

```json
{
   "swagger": "2.0",
   "info": {
       "version": "v1",
       "title": "API V1"
   },
   "basePath": "/",
   "paths": {
       "/api/Todo": {
           "get": {
               "tags": [
                   "Todo"
               ],
               "operationId": "ApiTodoGet",
               "consumes": [],
               "produces": [
                   "text/plain",
                   "application/json",
                   "text/json"
               ],
               "responses": {
                   "200": {
                       "description": "Success",
                       "schema": {
                           "type": "array",
                           "items": {
                               "$ref": "#/definitions/TodoItem"
                           }
                       }
                   }
                }
           },
           "post": {
               ...
           }
       },
       "/api/Todo/{id}": {
           "get": {
               ...
           },
           "put": {
               ...
           },
           "delete": {
               ...
   },
   "definitions": {
       "TodoItem": {
           "type": "object",
            "properties": {
                "id": {
                    "format": "int64",
                    "type": "integer"
                },
                "name": {
                    "type": "string"
                },
                "isComplete": {
                    "default": false,
                    "type": "boolean"
                }
            }
       }
   },
   "securityDefinitions": {}
}
```

## <a name="swagger-ui"></a><span data-ttu-id="ff6b8-125">Swagger-Benutzeroberfläche</span><span class="sxs-lookup"><span data-stu-id="ff6b8-125">Swagger UI</span></span>

<span data-ttu-id="ff6b8-126">Die [Swagger-Benutzeroberfläche](https://swagger.io/swagger-ui/) stellt eine webbasierte Benutzeroberfläche bereit, die Informationen über den Dienst enthält und die generierte Swagger-Spezifikation verwendet.</span><span class="sxs-lookup"><span data-stu-id="ff6b8-126">[Swagger UI](https://swagger.io/swagger-ui/) offers a web-based UI that provides information about the service, using the generated Swagger specification.</span></span> <span data-ttu-id="ff6b8-127">Swashbuckle und NSwag enthalten eine eingebettete Version der Swagger-Benutzeroberfläche, sodass diese in Ihrer ASP.NET Core-App mithilfe eines Registrierungsaufrufs für die Middleware gehostet werden kann.</span><span class="sxs-lookup"><span data-stu-id="ff6b8-127">Both Swashbuckle and NSwag include an embedded version of Swagger UI, so that it can be hosted in your ASP.NET Core app using a middleware registration call.</span></span> <span data-ttu-id="ff6b8-128">Die Webbenutzeroberfläche sieht folgendermaßen aus:</span><span class="sxs-lookup"><span data-stu-id="ff6b8-128">The web UI looks like this:</span></span>

![Swagger-Benutzeroberfläche](web-api-help-pages-using-swagger/_static/swagger-ui.png)

<span data-ttu-id="ff6b8-130">Jede öffentliche Aktionsmethode in Ihren Controllern kann über die Benutzeroberfläche getestet werden.</span><span class="sxs-lookup"><span data-stu-id="ff6b8-130">Each public action method in your controllers can be tested from the UI.</span></span> <span data-ttu-id="ff6b8-131">Klicken Sie auf einen Methodennamen, um den Abschnitt zu erweitern.</span><span class="sxs-lookup"><span data-stu-id="ff6b8-131">Click a method name to expand the section.</span></span> <span data-ttu-id="ff6b8-132">Fügen Sie die erforderlichen Parameter hinzu, und klicken Sie auf **Probieren Sie es aus!**.</span><span class="sxs-lookup"><span data-stu-id="ff6b8-132">Add any necessary parameters, and click **Try it out!**.</span></span>

![Beispiel für einen Swagger-GET-Test](web-api-help-pages-using-swagger/_static/get-try-it-out.png)

> [!NOTE]
> <span data-ttu-id="ff6b8-134">Die für den Screenshot verwendete Version der Swagger-Benutzeroberfläche ist Version 2.</span><span class="sxs-lookup"><span data-stu-id="ff6b8-134">The Swagger UI version used for the screenshots is version 2.</span></span> <span data-ttu-id="ff6b8-135">Ein Beispiel für Version 3 finden Sie unter [Pet store example (Beispiel für eine Tierhandlung)](http://petstore.swagger.io/).</span><span class="sxs-lookup"><span data-stu-id="ff6b8-135">For a version 3 example, see [Petstore example](http://petstore.swagger.io/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="ff6b8-136">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="ff6b8-136">Next steps</span></span>

* [<span data-ttu-id="ff6b8-137">Erste Schritte mit Swashbuckle</span><span class="sxs-lookup"><span data-stu-id="ff6b8-137">Get started with Swashbuckle</span></span>](xref:tutorials/get-started-with-swashbuckle)
* [<span data-ttu-id="ff6b8-138">Erste Schritte mit NSwag</span><span class="sxs-lookup"><span data-stu-id="ff6b8-138">Get started with NSwag</span></span>](xref:tutorials/get-started-with-nswag)
