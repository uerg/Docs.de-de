---
uid: web-api/overview/advanced/configuring-aspnet-web-api
title: Konfigurieren von ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2014
ms.topic: article
ms.assetid: 9e10a700-8d91-4d2e-a31e-b8b569fe867c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/configuring-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: f9b471fe2afdce278869a2e4d9b693a78030324b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="configuring-aspnet-web-api-2"></a>Konfigurieren von ASP.NET Web API 2
====================
durch [Mike Wasson](https://github.com/MikeWasson)

Dieses Thema beschreibt, wie ASP.NET Web-API zu konfigurieren.

- [Konfigurationseinstellungen](#settings)
- [Konfigurieren von Web-API mit ASP.NET hostet](#webhost)
- [Konfigurieren von Web-API mit OWIN Selbsthosting](#selfhost)
- [Globale Web API-Dienste](#services)
- [Pro Controller-Konfiguration](#percontrollerconfig)

<a id="settings"></a>
## <a name="configuration-settings"></a>Konfigurationseinstellungen

Einstellungen zum Web-API-Konfiguration Benutzergerät werden definiert, der [HttpConfiguration](https://msdn.microsoft.com/library/system.web.http.httpconfiguration.aspx) Klasse.

| Member | Beschreibung |
| --- | --- |
| **DependencyResolver** | Ermöglicht die Abhängigkeitsinjektion für Controller. Finden Sie unter [mithilfe des Web-API-Abhängigkeitskonfliktlösers](dependency-injection.md). |
| **Filter** | Aktionsfilter verwendet werden. |
| **Formatters** | [Medientypformatierer](../formats-and-model-binding/media-formatters.md). |
| **IncludeErrorDetailPolicy** | Gibt an, ob der Server Fehlerdetails wie ausnahmemeldungen und stapelüberwachungen in HTTP-Antwortnachrichten werden sollen. Finden Sie unter [IncludeErrorDetailPolicy](https://msdn.microsoft.com/library/system.web.http.includeerrordetailpolicy(v=vs.108)). |
| **Initializer** | Eine Funktion, die endgültige Initialisierung führt die **HttpConfiguration**. |
| **MessageHandlers** | [HTTP-Meldungshandler](http-message-handlers.md). |
| **ParameterBindingRules** | Eine Auflistung von Regeln zum Binden von Parametern für Controlleraktionen. |
| **Eigenschaften** | Eine generische Eigenschaftensammlung. |
| **Routen** | Die Auflistung der Routen. Finden Sie unter [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md). |
| **Dienste** | Die Auflistung von Diensten. Finden Sie unter [Services](#services). |


## <a name="prerequisites"></a>Erforderliche Komponenten

[Visual Studio-2017](https://www.visualstudio.com/vs/) Community, Professional oder Enterprise Edition.

<a id="webhost"></a>
## <a name="configuring-web-api-with-aspnet-hosting"></a>Konfigurieren von Web-API mit ASP.NET hostet

Konfigurieren Sie in einer ASP.NET-Anwendung Web-API, durch den Aufruf [GlobalConfiguration.Configure](https://msdn.microsoft.com/library/system.web.http.globalconfiguration.configure.aspx) in der **Anwendung\_starten** Methode. Die **konfigurieren** Methode nimmt einen Delegaten mit einem einzelnen Parameter vom Typ **HttpConfiguration**. Führen Sie alle Ihre Konfiguration innerhalb des Delegaten.

Hier ist ein Beispiel für die Verwendung eines anonymen Delegaten:

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample1.cs)]

In Visual Studio 2017 die Projektvorlage "ASP.NET-Webanwendung" automatisch richtet den Konfigurationscode bei Auswahl von "Web-API" in der **neues ASP.NET-Projekt** Dialogfeld.

[![](configuring-aspnet-web-api/_static/image2.png)](configuring-aspnet-web-api/_static/image1.png)

Die Projektvorlage erstellt eine Datei namens WebApiConfig.cs innerhalb der App\_Startordner. Diese Codedatei definiert den Delegaten, in denen Ihre Web-API-Konfigurationscode eingefügt werden soll.

![](configuring-aspnet-web-api/_static/image3.png)

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample2.cs?highlight=12)]

Die Projektvorlage fügt außerdem den Code, der die Delegatenaufrufe aus **Anwendung\_starten**.

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample3.cs?highlight=5)]

<a id="selfhost"></a>
## <a name="configuring-web-api-with-owin-self-hosting"></a>Konfigurieren von Web-API mit OWIN Selbsthosting

Wenn Sie Selbsthosting mit OWIN sind, erstellen Sie ein neues **HttpConfiguration** Instanz. Führen Sie eine Konfiguration auf dieser Instanz, und übergeben Sie die Instanz, die die **Owin.UseWebApi** Erweiterungsmethode.

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample4.cs)]

Das Lernprogramm [verwenden OWIN Self-Host ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md) zeigt die vollständige Schritte.

<a id="services"></a>
## <a name="global-web-api-services"></a>Globale Web API-Dienste

Die **HttpConfiguration.Services** Auflistung enthält einen Satz von globalen Diensten, die Web-API zum Ausführen verschiedener Aufgaben, z. B. Controller Auswahl und Content-Aushandlung verwendet.

> [!NOTE]
> Die **Services** Auflistung ist keinen allgemeinen Mechanismus für die Dienst-Ermittlung oder die Abhängigkeit Injection. Er speichert nur Diensttypen, die für die Web-API-Framework bekannt sind.


Die **Services** Auflistung einen Standardsatz von Diensten initialisiert wird und Sie können eigene benutzerdefinierten Implementierungen bereitstellen. Mehrere Instanzen verwenden, wird von allen Diensten unterstützt, während andere nur eine Instanz haben darf. (Allerdings können Sie auch Dienste auf Controllerebene angeben, denn finden Sie unter [pro Controller-Konfiguration](#percontrollerconfig).

Einzelinstanz-Dienste


| Dienst | Beschreibung |
| --- | --- |
| **IActionValueBinder** | Ruft eine Bindung für einen Parameter ab. |
| **IApiExplorer** | Ruft eine Beschreibung der APIs, die von der Anwendung verfügbar gemacht werden. Finden Sie unter [erstellen eine Hilfeseite für eine Web-API](../getting-started-with-aspnet-web-api/creating-api-help-pages.md). |
| **IAssembliesResolver** | Ruft eine Liste der Assemblys für die Anwendung ab. Finden Sie unter [Routing und Aktionsauswahl](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IBodyModelValidator** | Überprüft ein Modell, die von einem medientypformatierer aus dem Anforderungstext gelesen wird. |
| **IContentNegotiator** | Führt inhaltsaushandlung aus. |
| **IDocumentationProvider** | Dokumentation für APIs enthält. Die Standardeinstellung ist **null**. Finden Sie unter [erstellen eine Hilfeseite für eine Web-API](../getting-started-with-aspnet-web-api/creating-api-help-pages.md). |
| **IHostBufferPolicySelector** | Gibt an, ob der Host die HTTP-Entität Nachrichtentexte Puffern soll. |
| **IHttpActionInvoker** | Ruft eine Controlleraktion. Finden Sie unter [Routing und Aktionsauswahl](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpActionSelector** | Wählt eine Controlleraktion. Finden Sie unter [Routing und Aktionsauswahl](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpControllerActivator** | Aktiviert einen Controller. Finden Sie unter [Routing und Aktionsauswahl](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpControllerSelector** | Wählt einen Controller aus. Finden Sie unter [Routing und Aktionsauswahl](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpControllerTypeResolver** | Enthält eine Liste der Web-API-Controller-Typen in der Anwendung. Finden Sie unter [Routing und Aktionsauswahl](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **ITraceManager** | Initialisiert die ablaufverfolgungsframeworks an. Finden Sie unter [in der ASP.NET Web API Tracing](../testing-and-debugging/tracing-in-aspnet-web-api.md). |
| **ITraceWriter** | Stellt eine ablaufverfolgungswriter bereit. Der Standardwert ist "No-Op" Trace-Writer. Finden Sie unter [in der ASP.NET Web API Tracing](../testing-and-debugging/tracing-in-aspnet-web-api.md). |
| **IModelValidatorCache** | Bietet einen Cache der Modellvalidierungssteuerelemente. |

Mehrfachinstanz-Dienste


| Dienst | Beschreibung |
| --- | --- |
| **IFilterProvider** | Gibt eine Liste von Filtern für eine Controlleraktion. |
| **ModelBinderProvider** | Gibt einen Modellbinder für einen angegebenen Typ zurück. |
| **ModelMetadataProvider** | Stellt Metadaten für ein Modell bereit. |
| **ModelValidatorProvider** | Stellt ein Validierungssteuerelement für ein Modell bereit. |
| **ValueProviderFactory** | Erstellt einen Wertanbieter. Weitere Informationen finden Sie unter Mike Stall des Blogbeitrag [erstellen Sie einen benutzerdefinierten Wertanbieter in WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx) |sein.

Um eine benutzerdefinierte Implementierung an einen Dienst mit mehreren Instanzen hinzuzufügen, rufen Sie **hinzufügen** oder **einfügen** auf die **Services** Auflistung:

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample5.cs)]

Aufrufen, um einen Diensttyp mit einer Instanz durch eine benutzerdefinierte Implementierung ersetzen, **ersetzen** auf die **Services** Auflistung:

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample6.cs)]

<a id="percontrollerconfig"></a>
## <a name="per-controller-configuration"></a>Pro Controller-Konfiguration

Sie können die folgenden Einstellungen auf Basis pro Controller überschreiben:

- Medientypformatierer
- Bindungsregeln für Parameter
- Dienste

Zu diesem Zweck definieren Sie ein benutzerdefiniertes Attribut, das implementiert die **IControllerConfiguration** Schnittstelle. Wenden Sie dann das Attribut mit dem Controller an.

Im folgende Beispiel ersetzt die standardmäßige medientypformatierer durch ein benutzerdefiniertes Formatierungsprogramm.

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample7.cs)]

Die **IControllerConfiguration.Initialize** Methode akzeptiert zwei Parameter:

- Ein **HttpControllerSettings** Objekt
- Ein **HttpControllerDescriptor** Objekt

Die **HttpControllerDescriptor** enthält eine Beschreibung des Controllers an, die Sie, zu Informationszwecken (z. B. prüfen können zu Unterschieden zwischen zwei Controller).

Verwenden der **HttpControllerSettings** Objekt so konfigurieren Sie den Controller. Dieses Objekt enthält die Teilmenge der Konfigurationsparameter, die Sie regelmäßig pro Controller überschreiben können. Alle Einstellungen, die Sie nicht ändern, standardmäßig auf die globale **HttpConfiguration** Objekt.
