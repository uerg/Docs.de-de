---
uid: web-api/overview/older-versions/self-host-a-web-api
title: Selbst gehostete ASP.NET Web-API 1 (c#) | Microsoft-Dokumentation
author: MikeWasson
description: ASP.NET Web-API ist IIS nicht erforderlich. Sie können eine Web-API selbst in Ihrem eigenen Hostprozess hosten. In diesem Tutorial wird gezeigt, wie eine Web-API in einer Anwendung auf dem Konsole gehostet wird...
ms.author: aspnetcontent
ms.date: 01/26/2012
ms.assetid: be5ab1e2-4140-4275-ac59-ca82a1bac0c1
msc.legacyurl: /web-api/overview/older-versions/self-host-a-web-api
msc.type: authoredcontent
ms.openlocfilehash: 50681dcd89dfed480cf343f753371af384fd3e68
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37811736"
---
<a name="self-host-aspnet-web-api-1-c"></a>Selbst gehostete ASP.NET Web-API 1 (c#)
====================
durch [Mike Wasson](https://github.com/MikeWasson)

> ASP.NET Web-API ist IIS nicht erforderlich. Sie können eine Web-API selbst in Ihrem eigenen Hostprozess hosten. In diesem Tutorial wird gezeigt, wie eine Web-API in einer Konsolenanwendung gehostet wird.
> 
> **Neue Anwendungen sollten OWIN zum selfhosten der Web-API verwenden.** Finden Sie unter [verwenden Sie OWIN zum Selfhosten von ASP.NET-Web-API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Softwareversionen, die in diesem Tutorial verwendet werden.
> 
> 
> - Web-API 1
> - Visual Studio 2012


## <a name="create-the-console-application-project"></a>Erstellen Sie das Projekt der Konsolenanwendung

Starten Sie Visual Studio, und wählen Sie **neues Projekt** aus der **starten** Seite. Alternativ wählen Sie in der **Datei** , wählen Sie im Menü **neu** und dann **Projekt**.

In der **Vorlagen** wählen Sie im Bereich **installierte Vorlagen** und erweitern Sie die **Visual C#-** Knoten. Klicken Sie unter **Visual C#-** Option **Windows**. Wählen Sie in der Liste der Projektvorlagen das Projekt **Konsolenanwendung**. Nennen Sie das Projekt &quot;SelfHost&quot; , und klicken Sie auf **OK**.

![](self-host-a-web-api/_static/image1.png)

## <a name="set-the-target-framework-visual-studio-2010"></a>Legen Sie das Zielframework (Visual Studio 2010)

Wenn Sie Visual Studio 2010 verwenden, ändern Sie das Zielframework auf .NET Framework 4.0. (Standardmäßig wird die Projektvorlage abzielt der [.Net Framework-Clientprofil](https://msdn.microsoft.com/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile).)

Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste in des Projekts, und wählen **Eigenschaften**. In der **Zielframework** Dropdownliste, ändern Sie das Zielframework auf .NET Framework 4.0. Wenn Sie dazu aufgefordert werden, um die Änderung zu übernehmen, klicken Sie auf **Ja**.

![](self-host-a-web-api/_static/image2.png)

## <a name="install-nuget-package-manager"></a>Installieren von NuGet-Paket-Manager

Die NuGet-Paket-Manager ist die einfachste Möglichkeit, um die Web-API-Assemblys zu einem ASP.NET-Projekt hinzufügen.

Um festzustellen, ob NuGet-Paket-Manager installiert ist, klicken Sie auf die **Tools** Menü in Visual Studio. Wenn Sie ein Menüelement mit dem Namen finden Sie unter **Bibliothekspaket-Manager**, müssen Sie die NuGet-Paket-Manager.

So installieren Sie die NuGet-Paket-Manager:

1. Starten Sie Visual Studio.
2. Von der **Tools** , wählen Sie im Menü **Erweiterungen und Updates**.
3. In der **Erweiterungen und Updates** wählen Sie im Dialogfeld **Online**.
4. Wenn Sie "NuGet-Paket-Manager" nicht angezeigt wird, geben Sie "Nuget-Paket-Manager" in das Suchfeld ein.
5. Wählen Sie den NuGet-Paket-Manager aus, und klicken Sie auf **herunterladen**.
6. Nachdem der Download abgeschlossen ist, werden Sie aufgefordert werden, installieren.
7. Nach Abschluss der Installation werden Sie möglicherweise aufgefordert, Visual Studio neu zu starten.

![](self-host-a-web-api/_static/image3.png)

## <a name="add-the-web-api-nuget-package"></a>Hinzufügen der Web-API-NuGet-Pakets

Nach der Installation von NuGet-Paket-Manager fügen Sie das selfhosten der Web-API-Paket dem Projekt hinzu.

1. Von der **Tools** , wählen Sie im Menü **Bibliothekspaket-Manager**. *Beachten Sie*: Wenn Sie nicht dieses Menü sehen-Element, stellen Sie sicher, dass dieses NuGet-Paket-Manager ordnungsgemäß installiert.
2. Wählen Sie **NuGet-Pakete für Projektmappe verwalten...**
3. In der **NuGet-Pakete verwalten** wählen Sie im Dialogfeld **Online**.
4. Geben Sie in das Suchfeld &quot;Microsoft.AspNet.WebApi.SelfHost&quot;.
5. Wählen Sie das ASP.NET Web API-Self-Host-Paket, und klicken Sie auf **installieren**.
6. Nachdem das Paket installiert wurde, klicken Sie auf **schließen** um das Dialogfeld zu schließen.

> [!NOTE]
> Stellen Sie sicher, dass das Paket mit dem Namen Microsoft.AspNet.WebApi.SelfHost, nicht AspNetWebApi.SelfHost zu installieren.


![](self-host-a-web-api/_static/image4.png)

## <a name="create-the-model-and-controller"></a>Erstellen Sie das Modell und Controller

In diesem Tutorial verwendet die gleichen Modell und Controller-Klassen als die [Einstieg](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) Tutorial.

Fügen Sie eine öffentliche Klasse, die mit dem Namen `Product`.

[!code-csharp[Main](self-host-a-web-api/samples/sample1.cs)]

Fügen Sie eine öffentliche Klasse, die mit dem Namen `ProductsController`. Leiten Sie diese Klasse von **"System.Web.http.ApiController"**.

[!code-csharp[Main](self-host-a-web-api/samples/sample2.cs)]

Weitere Informationen zu den Code in diesem Controller, finden Sie unter den [Einstieg](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) Tutorial. Dieser Controller definiert drei GET-Aktionen:

| URI | Beschreibung |
| --- | --- |
| /api/products | Ruft eine Liste aller Produkte. |
| /api/products/*id* | Abrufen eines Produkts nach ID auf. |
| /api/products/?category=*category* | Erhalten Sie eine Liste der Produkte nach Kategorie. |

## <a name="host-the-web-api"></a>Hosten der Web-API

Öffnen Sie die Datei "Program.cs", und fügen Sie die folgenden using-Anweisungen:

[!code-csharp[Main](self-host-a-web-api/samples/sample3.cs)]

Fügen Sie den folgenden Code der **Programm** Klasse.

[!code-csharp[Main](self-host-a-web-api/samples/sample4.cs)]

## <a name="optional-add-an-http-url-namespace-reservation"></a>(Optional) Fügen Sie eine HTTP-URL-Namespace-Reservierung hinzu.

Diese Anwendung Lauscht auf `http://localhost:8080/`. Standardmäßig sind Administratorrechte erforderlich, wenn Sie in einer bestimmten HTTP-Adresse lauschen. Wenn Sie das Tutorial ausführen, daher kann diese Fehlermeldung erhalten: "HTTP konnte URL nicht registrieren http://+:8080/" Es gibt zwei Möglichkeiten, um diesen Fehler zu vermeiden:

- Führen Sie Visual Studio mit Administratorberechtigungen, oder
- Nutzen Sie Netsh.exe, um die Berechtigung, reservieren die URL für Ihr Konto zu ermöglichen.

Um Netsh.exe verwenden zu können, öffnen Sie eine Eingabeaufforderung mit Administratorrechten aus, und geben Sie den folgenden Befehl: folgenden Befehl aus:

[!code-console[Main](self-host-a-web-api/samples/sample5.cmd)]

wo *"Computer\Benutzername"* ist Ihr Benutzerkonto.

Wenn Sie Selbsthosting abgeschlossen sind, achten Sie darauf, dass Sie die Reservierung zu löschen:

[!code-console[Main](self-host-a-web-api/samples/sample6.cmd)]

## <a name="call-the-web-api-from-a-client-application-c"></a>Rufen Sie die Web-API von einer Clientanwendung (c#)

Schreiben wir nun eine einfache Konsolenanwendung, die die Web-API aufruft.

Fügen Sie der Projektmappe ein neues Konsolenanwendungsprojekt hinzu:

- Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste in der Projektmappe, und wählen **neues Projekt hinzufügen**.
- Erstellen Sie eine neue Konsolenanwendung, die mit dem Namen &quot;ClientApp&quot;.

![](self-host-a-web-api/_static/image5.png)

Verwenden Sie NuGet Package Manager, das Paket für die ASP.NET Web-API-Core-Bibliotheken hinzuzufügen:

- Wählen Sie im Menü Extras **Bibliothekspaket-Manager**.
- Wählen Sie **NuGet-Pakete für Projektmappe verwalten...**
- In der **NuGet-Pakete verwalten** wählen Sie im Dialogfeld **Online**.
- Geben Sie in das Suchfeld &quot;Microsoft.AspNet.WebApi.Client&quot;.
- Wählen Sie das Paket Microsoft ASP.NET Web-API-Client-Bibliotheken, und klicken Sie auf **installieren**.

Fügen Sie einen Verweis auf das Projekt SelfHost in ClientApp:

- Klicken Sie im Projektmappen-Explorer das Projekt ClientApp.
- Klicken Sie auf **Verweis hinzufügen**.
- In der **Verweis-Manager** Dialogfeld unter **Lösung**Option **Projekte**.
- Wählen Sie das SelfHost-Projekt.
- Klicken Sie auf **OK**.

![](self-host-a-web-api/_static/image6.png)

Öffnen Sie die Client/Program.cs-Datei. Fügen Sie die folgenden **mit** Anweisung:

[!code-csharp[Main](self-host-a-web-api/samples/sample7.cs)]

Fügen Sie einen statischen **"HttpClient"** Instanz:

[!code-csharp[Main](self-host-a-web-api/samples/sample8.cs)]

Fügen Sie die folgenden Methoden zum Auflisten aller Produkte, Liste eines Produkts nach ID und Liste Produkte nach Kategorie hinzu.

[!code-csharp[Main](self-host-a-web-api/samples/sample9.cs)]

Jede dieser Methoden folgt demselben Muster:

1. Rufen Sie **HttpClient.GetAsync** eine GET-Anforderung an den entsprechenden URI senden.
2. Rufen Sie **HttpResponseMessage.EnsureSuccessStatusCode**. Diese Methode löst eine Ausnahme aus, wenn der HTTP-Antwortstatus ein Fehlercode ist.
3. Rufen Sie **ReadAsAsync&lt;T&gt;**  um einen CLR-Typ der HTTP-Antwort zu deserialisieren. Diese Methode ist eine Erweiterungsmethode, die in definierten **System.Net.Http.HttpContentExtensions**.

Die **"getasync"** und **ReadAsAsync** Methoden sind zwar beide asynchron. Geben sie zurück **Aufgabe** Objekte, die den asynchronen Vorgang darstellen. Abrufen der **Ergebnis** -Eigenschaft sperrt den Thread, bis der Vorgang abgeschlossen ist.

Weitere Informationen zur Verwendung von "HttpClient", wie Sie diesen nicht blockierende Aufrufe finden Sie unter [Aufrufen einer Web-API aus einer .NET Client](../advanced/calling-a-web-api-from-a-net-client.md).

Legen Sie vor dem Aufrufen dieser Methoden, auf die HttpClient-Instanz, um die BaseAddress-Eigenschaft "`http://localhost:8080`". Zum Beispiel:

[!code-csharp[Main](self-host-a-web-api/samples/sample10.cs)]

Diese sollte folgende Ausgabe. (Denken Sie daran, die ersten Ausführen der Anwendung SelfHost).

[!code-console[Main](self-host-a-web-api/samples/sample11.cmd)]

![](self-host-a-web-api/_static/image7.png)
