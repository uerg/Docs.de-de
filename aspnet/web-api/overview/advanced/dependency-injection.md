---
uid: web-api/overview/advanced/dependency-injection
title: Abhängigkeitsinjektion in ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: Dieses Lernprogramm zeigt, wie Abhängigkeiten in der ASP.NET Web API-Controller einfügen. Softwareversionen, die im Lernprogramm Web API 2 Unity-Anwendungsblock verwendet...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: e3d3e7ba-87f0-4032-bdd3-31f3c1aa9d9c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 7f64cc83e36c80b0ffd53edfc629557c0847b200
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
ms.locfileid: "28036514"
---
<a name="dependency-injection-in-aspnet-web-api-2"></a>Abhängigkeitsinjektion in ASP.NET Web API 2
====================
durch [Mike Wasson](https://github.com/MikeWasson)

[Herunterladen des abgeschlossenen Projekts](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-468ee148)

> Dieses Lernprogramm zeigt, wie Abhängigkeiten in der ASP.NET Web API-Controller einfügen.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>In diesem Lernprogramm verwendeten Versionen der Software
> 
> 
> - Web-API 2
> - [Unity-Anwendungsblock](https://www.nuget.org/packages/Unity/)
> - Entity Framework 6 (Version 5 funktioniert auch)


## <a name="what-is-dependency-injection"></a>Was ist die Abhängigkeitsinjektion?

Ein *Abhängigkeit* ist jedes Objekt, das ein anderes Objekt ist erforderlich. Angenommen, es ist üblich, definieren Sie eine [Repository](http://martinfowler.com/eaaCatalog/repository.html) , die Zugriff auf Daten verarbeitet. Wir mit einem Beispiel veranschaulichen. Zuerst definieren wir ein Domänenmodell:

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

Hier ist eine einfaches Repository-Klasse, die Elemente in einer Datenbank, die Verwendung von Entity Framework speichert.

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

Jetzt sehen wir definieren einen Web-API-Controller, der für die GET-Anforderungen unterstützt `Product` Entitäten. (Ich bin, POST und andere Methoden aus Gründen der Einfachheit verlassen.) Hier wird ein erster Versuch:

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

Beachten Sie, von denen die Controllerklasse abhängt `ProductRepository`, und wir sind den Controller erstellen lassen die `ProductRepository` Instanz. Allerdings ist es nicht ratsam, hartcodieren die Abhängigkeit auf diese Weise verschiedene Ursachen.

- Wenn Sie ersetzen möchten `ProductRepository` mit einer anderen Implementierung müssen Sie auch die Controllerklasse ändern.
- Wenn die `ProductRepository` über Abhängigkeiten verfügt, müssen Sie diese in den Controller konfigurieren. Für ein großes Projekt mit mehreren Domänencontrollern wird der Konfigurationscode in Ihrem Projekt verschoben.
- Es ist schwierig, Komponententest, da der Controller fest programmiert, dass die Datenbank abzufragen ist. Für einen Komponententest sollten Sie Mock oder Stub-Repository verwenden, die nicht mit dem Entwurf Benutzerverzeichnis möglich ist.

Wir können diese Probleme durch *Räumen* das Repository in den Controller. Gestalten Sie zunächst die `ProductRepository` Klasse in einer Schnittstelle:

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

Geben Sie dann die `IProductRepository` als Konstruktorparameter:

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

Dieses Beispiel verwendet [Konstruktoreinfügung](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection). Sie können auch *Setter-Injektion*, in dem Sie die Abhängigkeit über einen Setter-Methode oder Eigenschaft festlegen.

Aber jetzt ein Problem vorliegt, da Ihre Anwendung keine den Controller direkt erstellt. Web-API erstellt den Controller aus, wenn er die Anforderung weitergeleitet und Web-API weiß nicht, alles zu `IProductRepository`. Dies kommt der Web-API-Abhängigkeitskonfliktlöser.

## <a name="the-web-api-dependency-resolver"></a>Die Web-API-Abhängigkeitskonfliktlöser

Web-API definiert die **IDependencyResolver** Schnittstelle zum Auflösen von Abhängigkeiten. Hier ist die Definition der Schnittstelle:

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

Die **IDependencyScope** Schnittstelle verfügt über zwei Methoden:

- **GetService** erstellt eine Instanz eines Typs.
- **"GetServices" auf** erstellt eine Auflistung von Objekten eines angegebenen Typs.

Die **IDependencyResolver** Methode erbt **IDependencyScope** und fügt die **BeginScope** Methode. Ich werde über Bereiche weiter unten in diesem Lernprogramm.

Wenn die Web-API eine Controllerinstanz erstellt wird, ruft er zuerst **IDependencyResolver.GetService**, und der Typ des Controllers übergeben. Diese Erweiterbarkeit Hook können Sie den Controller, erstellen Sie alle Abhängigkeiten werden aufgelöst. Wenn **GetService** gibt null zurück, sieht der Web-API für einen parameterlosen Konstruktor für die Controllerklasse.

## <a name="dependency-resolution-with-the-unity-container"></a>Abhängigkeit Auflösung mit der Unity-Container

Obwohl Sie eine vollständige schreiben **IDependencyResolver** Implementierung von Grund auf neu, die Schnittstelle ist, fungiert als Brücke zwischen Web-API und vorhandenen IoC-Container wirklich ausgelegt.

Ein IoC-Container ist eine Softwarekomponente, die zum Verwalten von Abhängigkeiten zuständig ist. Registrieren von Typen mit dem Container, und klicken Sie dann den Container verwenden, um Objekte zu erstellen. Der Container ermittelt automatisch die Abhängigkeit Beziehungen. Viele IoC-Container können Sie steuern, z. B. Objektlebensdauer und Bereich.

> [!NOTE]
> "IoC" steht für "Inversion des Steuerelements" Dies ist ein allgemeines Muster, in denen ein Framework Anwendungscode aufruft. Ein IoC-Container erstellt die Objekte, die "die übliche ablaufsteuerung kehrt".


Für dieses Lernprogramm verwenden wir [Unity](https://msdn.microsoft.com/library/ff647202.aspx) aus Microsoft Patterns &amp; Methoden. (Andere beliebten Bibliotheken enthalten [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Ninject](http://www.ninject.org/), und [StructureMap ](http://docs.structuremap.net/).) NuGet-Paket-Manager können Sie Unity installieren. Aus der **Tools** in Visual Studio, wählen Sie im Menü **Bibliothekspaket-Manager**, und wählen Sie dann **Package Manager Console**. Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl ein:

[!code-console[Main](dependency-injection/samples/sample7.cmd)]

Hier ist eine Implementierung von **IDependencyResolver** , ein Unity-Containers einschließt.

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

> [!NOTE]
> Wenn die **GetService** Methode einen Typ kann nicht aufgelöst werden, sollte zurückgegeben **null**. Wenn die **"GetServices" auf** Methode einen Typ kann nicht aufgelöst werden, sollten sie ein leeres Auflistungsobjekt zurück. Lösen Sie keine Ausnahmen für die unbekannten Typen.


## <a name="configuring-the-dependency-resolver"></a>Konfigurieren den Abhängigkeitskonfliktlöser

Legen Sie den Abhängigkeitskonfliktlöser für die **DependencyResolver** Eigenschaft des globalen **HttpConfiguration** Objekt.

Im folgenden Codebeispiel Register der `IProductRepository` Schnittstelle mit Unity, und erstellt dann eine `UnityResolver`.

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

## <a name="dependency-scope-and-controller-lifetime"></a>Abhängigkeitsbereich und Controller-Lebensdauer

Domänencontroller werden pro Anforderung erstellt. Zum Verwalten der Objektlebensdauer, **IDependencyResolver** verwendet das Konzept einer *Bereich*.

Der Abhängigkeitskonfliktlöser angefügt, um die **HttpConfiguration** Objekt hat globalen Gültigkeitsbereich. Wenn die Web-API einen Controller erstellt, ruft er **BeginScope**. Diese Methode gibt ein **IDependencyScope** , der einen untergeordneten Bereich darstellt.

Web-API ruft dann **GetService** im untergeordneten Bereich den Controller zu erstellen. Wenn die Anforderung abgeschlossen ist, Web-API-Aufrufe **Dispose** auf untergeordneten Bereich. Verwenden der **Dispose** Methode, um den Controller Abhängigkeiten zu verwerfen.

Zum Implementieren von **BeginScope** richtet sich nach der IoC-Container. Für Unity und entspricht Bereich ein untergeordneter Container:

[!code-csharp[Main](dependency-injection/samples/sample10.cs)]

Die meisten IoC-Container haben ähnliche Entsprechungen.
