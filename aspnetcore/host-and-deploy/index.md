---
title: Hosten und Bereitstellen von ASP.NET Core
author: rick-anderson
description: Erfahren Sie, wie Sie Hostingumgebungen einrichten und ASP.NET Core-Apps bereitstellen.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 08/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/index
ms.openlocfilehash: 1ffc7f9f2dc2a06dddb629d2d2553964b56cec05
ms.sourcegitcommit: 1b94305cc79843e2b0866dae811dab61c21980ad
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/24/2018
ms.locfileid: "34472933"
---
# <a name="host-and-deploy-aspnet-core"></a>Hosten und Bereitstellen von ASP.NET Core

Um eine ASP.NET Core-App in einer Hostingumgebung bereitzustellen, müssen Sie allgemein folgende Schritte durchführen:

* Veröffentlichen Sie die App in einem Ordner auf dem Hostserver.
* Richten Sie einen Prozess-Manager ein, der die App startet, wenn Anforderungen eingehen und die App nach einem Absturz oder Serverneustart neu startet.
* Wenn die Konfiguration eines Reverseproxys gewünscht ist, richten Sie einen Reverseproxy ein, der Anforderungen an die App weiterleitet.

## <a name="publish-to-a-folder"></a>Veröffentlichen in einem Ordner

Der CLI-Befehl [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) kompiliert App-Code und kopiert die Dateien, die zum Ausführen der App benötigt werden, in den Ordner *publish*. Bei der Bereitstellung von Visual Studio erfolgt der Schritt [dotnet publish](/dotnet/core/tools/dotnet-publish) automatisch, bevor die Dateien in das Bereitstellungsziel kopiert werden.

### <a name="folder-contents"></a>Ordnerinhalte

Der Ordner *publish* enthält *EXE*- und *DLL*-Dateien für die App, ihre Abhängigkeiten und optional die .NET-Runtime.

Eine .NET Core-App kann *eigenständig* oder *Framework-abhängig* veröffentlicht werden. Wenn die App eigenständig ist, sind die *DLL*-Dateien, die die .NET-Runtime enthalten, im Ordner *publish* enthalten. Wenn die App frameworkabhängig ist, sind die Dateien für die .NET-Runtime nicht enthalten, da die App über einen Verweis auf eine auf dem Server installierte .NET-Version verfügt. Das Standardmodell für die Bereitstellung ist Framework-abhängig. Weitere Informationen finden Sie unter [.NET Core application deployment (.NET Core-Anwendungsbereitstellung)](/dotnet/articles/core/deploying/index).

Zusätzlich zu den *EXE*- und *DLL*-Dateien enthält der Ordner *publish* für eine ASP.NET Core-App üblicherweise noch Konfigurationsdateien, statische Objekte und MVC-Ansichten. Weitere Informationen finden Sie unter [Directory structure (Verzeichnisstruktur)](xref:host-and-deploy/directory-structure).

## <a name="set-up-a-process-manager"></a>Einrichten eines Prozessmanagers

Bei einer ASP.NET Core-App handelt es sich um eine Konsolen-App, die gestartet werden muss, wenn ein Server startet und nach Abstürzen neu startet. Sie benötigen einen Prozess-Manager, um die Starts und Neustarts zu automatisieren. Die gängigsten Prozess-Manager für ASP.NET Core sind Folgende:

* Linux
  * [Nginx](xref:host-and-deploy/linux-nginx)
  * [Apache](xref:host-and-deploy/linux-apache)
* Windows
  * [IIS](xref:host-and-deploy/iis/index)
  * [Windows-Dienst](xref:host-and-deploy/windows-service)

## <a name="set-up-a-reverse-proxy"></a>Einrichten eines Reverseproxys

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Wenn die App den [Kestrel](xref:fundamentals/servers/kestrel)-Webserver verwendet, können [Nginx](xref:host-and-deploy/linux-nginx), [Apache](xref:host-and-deploy/linux-apache) oder [IIS](xref:host-and-deploy/iis/index) als Reverseproxyserver verwendet werden. Ein Reverseproxyserver empfängt HTTP-Anforderungen aus dem Internet und leitet diese nach einer vorbereitenden Verarbeitung an Kestrel weiter.

Jede der beiden Konfigurationen &mdash;mit oder ohne einen Reverseproxyserver&mdash; ist eine gültige und unterstützte Hostingkonfiguration für ASP.NET Core 2.0 oder neuere Apps. Weitere Informationen finden Sie unter [When to use Kestrel with a reverse proxy (Verwenden von Kestrel mit einem Reverseproxy)](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Wenn die App den [Kestrel](xref:fundamentals/servers/kestrel)-Webserver verwendet und ins Internet gestellt wird, müssen Sie [Nginx](xref:host-and-deploy/linux-nginx), [Apache](xref:host-and-deploy/linux-apache) oder [IIS](xref:host-and-deploy/iis/index) als Reverseproxyserver verwenden. Ein Reverseproxyserver empfängt HTTP-Anforderungen aus dem Internet und leitet diese nach einer vorbereitenden Verarbeitung an Kestrel weiter. Der Hauptgrund für die Verwendung eines Reverseproxys ist Sicherheit. Weitere Informationen finden Sie unter [When to use Kestrel with a reverse proxy (Verwenden von Kestrel mit einem Reverseproxy)](xref:fundamentals/servers/kestrel?tabs=aspnetcore1x#when-to-use-kestrel-with-a-reverse-proxy).

---

## <a name="proxy-server-and-load-balancer-scenarios"></a>Proxyserver und Lastenausgleichsszenarien

Möglicherweise ist zusätzliche Konfiguration für Apps erforderlich, die hinter Proxyservern und Lastenausgleichsmodulen (Load Balancer) gehostet werden. Ohne zusätzliche Konfiguration hat eine App möglicherweise keinen Zugriff auf das Schema (HTTP/HTTPS) und die Remote-IP-Adresse, von der eine Anforderung stammte. Weitere Informationen hierzu feinden Sie unter [Konfigurieren von ASP.NET Core zur Verwendung mit Proxyservern und Lastenausgleich](xref:host-and-deploy/proxy-load-balancer).

## <a name="using-visual-studio-and-msbuild-to-automate-deployment"></a>Verwenden von Visual Studio und MSBuild zum Automatisieren der Bereitstellung

Die Bereitstellung erfordert neben dem Kopieren der Ausgabe von [dotnet publish](/dotnet/core/tools/dotnet-publish) auf einen Server oft zusätzliche Aufgaben. Beispielsweise können zusätzliche Dateien aus dem Ordner *publish* erforderlich oder ausgeschlossen sein. Visual Studio verwendet MSBuild für die Webbereitstellung, und MSBuild kann für die Ausführung vieler weiterer Tasks während der Bereitstellung angepasst werden. Weitere Informationen finden Sie unter [Publish profiles in Visual Studio (Veröffentlichen von Profilen in Visual Studio)](xref:host-and-deploy/visual-studio-publish-profiles) und im Buch [Using MSBuild and Team Foundation Build (Verwenden von MSBuild und Team Foundation Build)](http://msbuildbook.com/).

Mithilfe des [Features zum Veröffentlichen einer Web-App](xref:tutorials/publish-to-azure-webapp-using-vs) oder der [integrierten Git-Unterstützung](xref:host-and-deploy/azure-apps/azure-continuous-deployment) können Apps direkt von Visual Studio in Azure App Service bereitgestellt werden. Visual Studio Team Services unterstützt die [continuous deployment to Azure App Service (fortlaufende Bereitstellung in Azure App Service)](/vsts/build-release/apps/cd/azure/aspnet-core-to-azure-webapp?tabs=vsts).

## <a name="publishing-to-azure"></a>Veröffentlichen in Azure

Anweisungen zum Veröffentlichen einer App in Azure mit Visual Studio finden Sie unter [Veröffentlichen einer ASP.NET Core-Web-App in Azure App Service mit Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs). Die App kann auch über die [Befehlszeile](xref:tutorials/publish-to-azure-webapp-using-cli) in Azure veröffentlicht werden.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

Weitere Informationen zum Verwenden von Docker als Hostingumgebung finden Sie unter [Host ASP.NET Core apps in Docker (Hosten von ASP.NET Core-Apps in Docker)](xref:host-and-deploy/docker/index).
