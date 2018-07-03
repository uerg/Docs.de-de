---
title: Erste Schritte mit Razor Pages in ASP.NET Core
author: rick-anderson
description: Erfahren Sie Grundlegendes zur Erstellung einer ASP.NET Core-Web-App mit Razor-Seiten. Razor-Seiten werden für Web-Workloads in ASP.NET Core empfohlen.
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 7148f2d944bd1978b1a83278dfed9051f192e4dd
ms.sourcegitcommit: 08f1a9baa97060da5168840b332c9c0805b5f901
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2018
ms.locfileid: "37144936"
---
# <a name="get-started-with-razor-pages-in-aspnet-core"></a>Erste Schritte mit Razor Pages in ASP.NET Core

Von [Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range="= aspnetcore-2.0"

Es wird empfohlen, die Version ASP.NET Core 2.1 dieses Tutorials zu verwenden. Es ist **deutlich** einfacher, dieser Version zu folgen; darüber hinaus behandelt diese mehr Features. Wählen Sie in der Versionsauswahl **ASP.NET Core 2.1** aus.

![Versionsauswahl im Inhaltsverzeichnis](razor-pages-start/_static/v21.png)

::: moniker-end

In diesem Tutorial lernen Sie Grundlegendes zur Erstellung einer ASP.NET Core-Webapp mit Razor Pages. Razor Pages sind der empfohlene Weg für die Erstellung von Benutzeroberflächen für Web-Apps in ASP.NET Core.

Es gibt drei Versionen dieses Tutorials:

* Windows: Dieses Tutorial
* macOS: [Erste Schritte mit Razor-Seiten in ASP.NET Core mit Visual Studio für Mac](xref:tutorials/razor-pages-mac/razor-pages-start)
* macOS, Linux und Windows: [Erste Schritte mit Razor-Seiten in ASP.NET Core mit Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))

::: moniker range=">= aspnetcore-2.1"

## <a name="prerequisites"></a>Erforderliche Komponenten

[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-razor-web-app"></a>Erstellen einer Razor-Web-App

* Wählen Sie in Visual Studio im Menü **Datei** die Option **Neu** > **Projekt** aus.
* Erstellen Sie eine neue ASP.NET Core-Webanwendung. Nennen Sie das Projekt **RazorPagesMovie**. Es ist wichtig, den Namen *RazorPagesMovie* zu verwenden, damit die Namespaces übereinstimmen, wenn Sie Code kopieren/einfügen.
 ![neue ASP.NET Core-Webanwendung](razor-pages-start/_static/np_2.1.png)
* Wählen Sie in der Dropdownliste **ASP.NET Core 2.1** und anschließend **Webanwendung** aus.

 ![neue ASP.NET Core-Webanwendung](razor-pages-start/_static/np_2_2.1.png)

Die Visual Studio-Vorlage erstellt ein Startprojekt:

![Projektmappen-Explorer](razor-pages-start/_static/se2.1.png)

Drücken Sie **F5**, um die App im Debugmodus auszuführen, oder **STRG + F5**, um die App ohne Anfügen des Debuggers auszuführen. Wählen Sie **Akzeptieren** aus, um der Nachverfolgung zuzustimmen. Diese App verfolgt keine personenbezogenen Informationen nach. Der generierte Vorlagencode enthält Objekte, die bei der Erfüllung der [Datenschutz-Grundverordnung (DSGVO)](xref:security/gdpr) als Unterstützung dienen sollen.

![Start- oder Indexseite](razor-pages-start/_static/homeGDPR.png)

Die folgende Abbildung zeigt die App, nachdem die Nachverfolgung akzeptiert wurde:

![Start- oder Indexseite](razor-pages-start/_static/home2.1.png)

* Visual Studio startet [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) und führt die App aus. Die Adressleiste zeigt `localhost:port#` an, nicht `example.com`. Das liegt daran, dass es sich bei `localhost` um den Standardhostnamen für Ihren lokalen Computer handelt. „Localhost“ dient nur Webanforderungen vom lokalen Computer. Wenn in Visual Studio ein Webprojekt erstellt wird, wird für den Webserver ein zufälliger Port verwendet. In der vorherigen Abbildung ist die Nummer des Ports 5000. Wenn Sie die App ausführen, wird eine andere Portnummer angezeigt.
* Das Starten der App mit **STRG+F5** (Nicht-Debugmodus) ermöglicht die Änderung des Codes, das Speichern der Datei, das Aktualisieren des Browsers und das Anzeigen von Codeänderungen. Viele Entwickler bevorzugen den Nicht-Debugmodus, um die App schnell zu starten und Änderungen anzuzeigen.

[!INCLUDE [razor-pages-start](~/includes/RP/2.1/razor-pages-start.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

## <a name="prerequisites"></a>Erforderliche Komponenten

[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-razor-web-app"></a>Erstellen einer Razor-Web-App

* Wählen Sie in Visual Studio im Menü **Datei** die Option **Neu** > **Projekt** aus.
* Erstellen Sie eine neue ASP.NET Core-Webanwendung. Nennen Sie das Projekt **RazorPagesMovie**. Es ist wichtig, den Namen *RazorPagesMovie* zu verwenden, damit die Namespaces übereinstimmen, wenn Sie Code kopieren/einfügen.
  ![neue ASP.NET Core-Webanwendung](../../razor-pages/index/_static/np.png)
* Wählen Sie in der Dropdownliste **ASP.NET Core 2.0** aus, und klicken Sie anschließend auf **Webanwendung**.

  [!INCLUDE [install 2.0](~/includes/dotnetcore-on-dotnetfx-vs.md)]

Die Visual Studio-Vorlage erstellt ein Startprojekt:

![Projektmappen-Explorer](razor-pages-start/_static/se.png)

Drücken Sie **F5**, um die App im Debugmodus auszuführen, oder **STRG+F5** zur Ausführung ohne Anfügen des Debuggers.

![Start- oder Indexseite](razor-pages-start/_static/home.png)

* Visual Studio startet [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) und führt Ihre App aus. Die Adressleiste zeigt `localhost:port#` an, nicht `example.com`. Das liegt daran, dass es sich bei `localhost` um den Standardhostnamen für Ihren lokalen Computer handelt. „Localhost“ dient nur Webanforderungen vom lokalen Computer. Wenn in Visual Studio ein Webprojekt erstellt wird, wird für den Webserver ein zufälliger Port verwendet. In der vorherigen Abbildung ist die Nummer des Ports 5000. Wenn Sie die App ausführen, wird eine andere Portnummer angezeigt.
* Das Starten der App mit **STRG+F5** (Nicht-Debugmodus) ermöglicht die Änderung des Codes, das Speichern der Datei, das Aktualisieren des Browsers und das Anzeigen von Codeänderungen. Viele Entwickler bevorzugen den Nicht-Debugmodus, um die App schnell zu starten und Änderungen anzuzeigen.

[!INCLUDE [razor-pages-start](~/includes/RP/razor-pages-start.md)]

::: moniker-end

> [!div class="step-by-step"]
> [Nächstes Tutorial: Adding a model to a Razor Pages app in ASP.NET Core with Visual Studio for Mac (Hinzufügen eines Modells zu einer App mit Razor Pages in ASP.NET Core mit Visual Studio für Mac)](xref:tutorials/razor-pages/model)
