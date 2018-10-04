---
uid: web-api/overview/advanced/configuring-aspnet-web-api
title: Konfigurieren von ASP.NET-Web-API 2 | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: riande
ms.date: 03/31/2014
ms.assetid: 9e10a700-8d91-4d2e-a31e-b8b569fe867c
msc.legacyurl: /web-api/overview/advanced/configuring-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 57066b8ce3254caf59cf927d16d96f8bc22a8acd
ms.sourcegitcommit: 7890dfb5a8f8c07d813f166d3ab0c263f893d0c6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/04/2018
ms.locfileid: "48795250"
---
<a name="configuring-aspnet-web-api-2"></a>Konfigurieren von ASP.NET Web-API 2
====================
durch [Mike Wasson](https://github.com/MikeWasson)

Dieses Thema beschreibt, wie Sie ASP.NET Web-API zu konfigurieren.

- [Konfigurationseinstellungen](#settings)
- [Konfigurieren von Web-API mit Hosten von ASP.NET](#webhost)
- [Konfigurieren von Web-API mit Selbstgehostetem](#selfhost)
- [Globale Web-API-Dienste](#services)
- [Pro Controller-Konfiguration](#percontrollerconfig)

<a id="settings"></a>
## <a name="configuration-settings"></a>Konfigurationseinstellungen

Einstellungen der Web-API-Konfiguration werden definiert, der [HttpConfiguration](https://msdn.microsoft.com/library/system.web.http.httpconfiguration.aspx) Klasse.

| Member | Beschreibung |
| --- | --- |
| **DependencyResolver** | Ermöglicht Dependency Injection für Controller. Finden Sie unter [mithilfe des Web-API-Abhängigkeitskonfliktlösers](dependency-injection.md). |
| **Filter** | Aktionsfilter verwendet werden. |
| **Formatierungsprogramme** | [Medientypformatierer](../formats-and-model-binding/media-formatters.md). |
| **IncludeErrorDetailPolicy** | Gibt an, ob der Server Fehlerdetails wie ausnahmemeldungen und stapelüberwachungen in HTTP-Antwortnachrichten enthalten soll. Finden Sie unter [IncludeErrorDetailPolicy](https://msdn.microsoft.com/library/system.web.http.includeerrordetailpolicy(v=vs.108)). |
| **Initializer** | Eine Funktion, die endgültige Initialisierung führt die **HttpConfiguration**. |
| **MessageHandlers** | [HTTP-Meldungshandler](http-message-handlers.md). |
| **ParameterBindingRules** | Eine Auflistung von Regeln für das Binden von Parametern in Controlleraktionen. |
| **Eigenschaften** | Eine generische Eigenschaftensammlung. |
| **Routen** | Die Auflistung der Routen. Finden Sie unter [Routing in ASP.NET Web-API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md). |
| **Dienste** | Die Auflistung von Diensten. Finden Sie unter [Services](#services). |


## <a name="prerequisites"></a>Vorraussetzungen

[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional oder Enterprise Edition.

<a id="webhost"></a>
## <a name="configuring-web-api-with-aspnet-hosting"></a>Konfigurieren von Web-API mit Hosten von ASP.NET

Konfigurieren Sie in einer ASP.NET-Anwendung durch Aufrufen von Web-API- [GlobalConfiguration.Configure](https://msdn.microsoft.com/library/system.web.http.globalconfiguration.configure.aspx) in die **Anwendung\_starten** Methode. Die **konfigurieren** Methode verwendet einen Delegaten mit einem einzelnen Parameter vom Typ **HttpConfiguration**. Führen Sie alle Ihre Remotedebugger innerhalb des Delegaten.

Hier ist ein Beispiel mit einem anonymen Delegaten:

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample1.cs)]

In Visual Studio 2017 die Projektvorlage "ASP.NET Web Application" richtet automatisch den Konfigurationscode bei Auswahl von "Web-API" in der **neues ASP.NET-Projekt** Dialogfeld.

[![](configuring-aspnet-web-api/_static/image2.png)](configuring-aspnet-web-api/_static/image1.png)

Die Projektvorlage erstellt eine Datei namens WebApiConfig.cs innerhalb der App\_Startordner. Diese Codedatei definiert den Delegaten, in denen Ihre Web-API-Konfigurationscode eingefügt werden soll.

![](configuring-aspnet-web-api/_static/image3.png)

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample2.cs?highlight=12)]

Die Projektvorlage fügt auch den Code, der Delegat, aus aufruft **Anwendung\_starten**.

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample3.cs?highlight=5)]

<a id="selfhost"></a>
## <a name="configuring-web-api-with-owin-self-hosting"></a>Konfigurieren von Web-API mit Selbstgehostetem

Wenn Sie Selbsthosting mit OWIN sind, erstellen Sie ein neues **HttpConfiguration** Instanz. Führen Sie eine Konfiguration für diese Instanz, und übergeben Sie dann die Instanz, die die **Owin.UseWebApi** -Erweiterungsmethode.

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample4.cs)]

Das Tutorial [verwenden OWIN zum selfhosten ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md) zeigt die vollständige Schritte.

<a id="services"></a>
## <a name="global-web-api-services"></a>Globale Web-API-Dienste

Die **HttpConfiguration.Services** Auflistung enthält einen Satz von globalen Diensten, die Web-API verwendet, um verschiedene Aufgaben, wie Controller Auswahl und inhaltsaushandlung übernehmen.

> [!NOTE]
> Die **Services** Auflistung ist keinen allgemeinen Mechanismus für Service Discovery oder Dependency Injection. Es speichert nur Diensttypen, die für das Web-API-Framework bezeichnet werden.


Die **Services** Auflistung wird mit einer Reihe von Diensten initialisiert, und Sie können eigene benutzerdefinierten Implementierungen bereitstellen. Einige Dienste unterstützen mehrere Instanzen, aus, während andere nur eine Instanz aufweisen können. (Allerdings können Sie auch Dienste auf der Controllerebene bereitstellen, finden Sie unter [pro Controller-Konfiguration](#percontrollerconfig).

Einzelinstanz-Dienste


| Dienst | Beschreibung |
| --- | --- |
| **IActionValueBinder** | Ruft eine Bindung für einen Parameter ab. |
| **IApiExplorer** | Ruft eine Beschreibung der APIs verfügbar gemacht, von der Anwendung ab. Finden Sie unter [erstellen eine Hilfeseite einer Web-API](../getting-started-with-aspnet-web-api/creating-api-help-pages.md). |
| **IAssembliesResolver** | Ruft eine Liste der Assemblys für die Anwendung. Finden Sie unter [Routing und Aktionsauswahl](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IBodyModelValidator** | Überprüft ein Modell, das von einem medientypformatierer aus dem Anforderungstext gelesen wird. |
| **IContentNegotiator** | Inhaltsaushandlung ausführt. |
| **IDocumentationProvider** | Stellt eine Dokumentation für APIs. Der Standardwert ist **null**. Finden Sie unter [erstellen eine Hilfeseite einer Web-API](../getting-started-with-aspnet-web-api/creating-api-help-pages.md). |
| **IHostBufferPolicySelector** | Gibt an, ob der Host die HTTP-Nachrichtentexte Entität Puffern soll. |
| **IHttpActionInvoker** | Ruft eine Controlleraktion. Finden Sie unter [Routing und Aktionsauswahl](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpActionSelector** | Wählt eine Controlleraktion. Finden Sie unter [Routing und Aktionsauswahl](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpControllerActivator** | Aktiviert einen Controller. Finden Sie unter [Routing und Aktionsauswahl](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpControllerSelector** | Wählt einen Controller. Finden Sie unter [Routing und Aktionsauswahl](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpControllerTypeResolver** | Enthält eine Liste der Web-API-Controllertypen in der Anwendung. Finden Sie unter [Routing und Aktionsauswahl](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **ITraceManager** | Initialisiert das Framework für die Ablaufverfolgung an. Finden Sie unter [Ablaufverfolgung in ASP.NET Web-API](../testing-and-debugging/tracing-in-aspnet-web-api.md). |
| **ITraceWriter** | Stellt einen Ablaufverfolgungs-Writer bereit. Der Standardwert ist eine "No-Op" Ablaufverfolgungs-Writer. Finden Sie unter [Ablaufverfolgung in ASP.NET Web-API](../testing-and-debugging/tracing-in-aspnet-web-api.md). |
| **IModelValidatorCache** | Stellt einen Cache der Modellvalidierungssteuerelemente bereit. |

Dienste mit mehreren Instanzen


|                 Dienst                 |                                                                                                              Beschreibung                                                                                                               |
|-----------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    <strong>IFilterProvider</strong>     |                                                                                           Gibt eine Liste von Filtern für eine Controlleraktion.                                                                                           |
|  <strong>ModelBinderProvider</strong>   |                                                                                                Gibt einen Modellbinder für einen angegebenen Typ zurück.                                                                                                |
| <strong>ModelMetadataProvider</strong>  |                                                                                                     Stellt Metadaten für ein Modell bereit.                                                                                                     |
| <strong>ModelValidatorProvider</strong> |                                                                                                   Stellt eine Validierungsadapterklasse für ein Modell bereit.                                                                                                    |
|  <strong>ValueProviderFactory</strong>  | Erstellt einen Wertanbieter. Weitere Informationen finden Sie unter der Mike Stall Blogbeitrag [erstellen Sie einen benutzerdefinierten Wertanbieter in Web-API](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx) |

Um eine benutzerdefinierte Implementierung an einen Dienst mit mehreren Instanzen hinzuzufügen, rufen Sie **hinzufügen** oder **einfügen** auf die **Services** Auflistung:

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample5.cs)]

Um einen Diensttyp mit einer Instanz durch eine benutzerdefinierte Implementierung ersetzen möchten, rufen Sie **ersetzen** auf die **Services** Auflistung:

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample6.cs)]

<a id="percontrollerconfig"></a>
## <a name="per-controller-configuration"></a>Pro Controller-Konfiguration

Sie können die folgenden Einstellungen auf einer Basis pro Controller überschreiben:

- Medientypformatierer
- Parameter-Bindungsregeln
- Dienste

Zu diesem Zweck definieren Sie ein benutzerdefiniertes Attribut, das implementiert die **IControllerConfiguration** Schnittstelle. Wenden Sie dann das Attribut mit dem Controller.

Im folgende Beispiel ersetzt die standardmäßige medientypformatierer durch einen benutzerdefinierten Formatierer.

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample7.cs)]

Die **IControllerConfiguration.Initialize** Methode akzeptiert zwei Parameter:

- Ein **HttpControllerSettings** Objekt
- Ein **HttpControllerDescriptor** Objekt

Die **HttpControllerDescriptor** enthält eine Beschreibung des Controllers, den Sie, zu Informationszwecken (z. B. zur Unterscheidung zwischen zwei Controller untersuchen können).

Verwenden der **HttpControllerSettings** Objekt, das den Testcontroller zu konfigurieren. Dieses Objekt enthält die Teilmenge der Konfigurationsparameter, die auf einer Basis pro Controller überschrieben werden kann. Alle Einstellungen, die Sie ändern nicht standardmäßig auf die globale **HttpConfiguration** Objekt.
