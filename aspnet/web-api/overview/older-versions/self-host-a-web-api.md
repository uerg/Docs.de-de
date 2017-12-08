---
uid: web-api/overview/older-versions/self-host-a-web-api
title: Selbsthosting ASP.NET Web-API 1 (c#) | Microsoft Docs
author: MikeWasson
description: "ASP.NET Web-API erfordert keine IIS. Eine Web-API können in Ihren eigenen Hostprozess selbst gehostet werden. In diesem Lernprogramm wird gezeigt, wie eine Web-API in einer Konsole applic hosten wird..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2012
ms.topic: article
ms.assetid: be5ab1e2-4140-4275-ac59-ca82a1bac0c1
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/self-host-a-web-api
msc.type: authoredcontent
ms.openlocfilehash: b308ee9ec209ba8bbb021827655c83443dd149e6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="self-host-aspnet-web-api-1-c"></a>Selbsthosting ASP.NET Web-API 1 (c#)
====================
durch [Mike Wasson](https://github.com/MikeWasson)

> ASP.NET Web-API erfordert keine IIS. Eine Web-API können in Ihren eigenen Hostprozess selbst gehostet werden. In diesem Lernprogramm wird gezeigt, wie eine Web-API in einer Konsolenanwendung gehostet wird.
> 
> **Neue Anwendungen sollten mithilfe von OWIN Web API Self host.** Finden Sie unter [mit der ASP.NET Web API 2 Selbsthosting OWIN](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>In diesem Lernprogramm verwendeten Versionen der Software
> 
> 
> - Web-API 1
> - Visual Studio 2012


## <a name="create-the-console-application-project"></a>Erstellen Sie das Konsolenanwendungsprojekt

Starten Sie Visual Studio, und wählen Sie **neues Projekt** aus der **starten** Seite. Oder von der **Datei** klicken Sie im Menü **neu** und dann **Projekt**.

In der **Vorlagen** klicken Sie im Bereich **installierte Vorlagen** und erweitern Sie die **Visual C#-** Knoten. Klicken Sie unter **Visual C#-**Option **Windows**. Wählen Sie in der Liste der Projektvorlagen **Konsolenanwendung**. Nennen Sie das Projekt &quot;SelfHost&quot; , und klicken Sie auf **OK**.

![](self-host-a-web-api/_static/image1.png)

## <a name="set-the-target-framework-visual-studio-2010"></a>Legen Sie das Zielframework (Visual Studio 2010)

Wenn Sie Visual Studio 2010 verwenden, ändern Sie das Zielframework in .NET Framework 4.0. (Standardmäßig wird die Projektvorlage abzielt die [.Net Framework Client Profile](https://msdn.microsoft.com/en-us/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile).)

Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste des Projekts, und wählen **Eigenschaften**. In der **Zielframework** Dropdownliste ändern das Zielframework auf .NET Framework 4.0. Wenn Sie dazu aufgefordert werden, um die Änderung zu übernehmen, klicken Sie auf **Ja**.

![](self-host-a-web-api/_static/image2.png)

## <a name="install-nuget-package-manager"></a>Installieren von NuGet-Paket-Manager

Die NuGet-Paket-Manager ist die einfachste Möglichkeit, die Web-API-Assemblys zu einem ASP.NET-Projekt hinzufügen.

Um festzustellen, ob NuGet Package Manager installiert ist, klicken Sie auf die **Tools** Menü in Visual Studio. Wenn Sie ein Datenelement namens Menü finden Sie unter **Bibliothekspaket-Manager**, dann verfügen Sie über NuGet-Paket-Manager.

So installieren Sie das NuGet-Paket-Manager:

1. Starten Sie Visual Studio.
2. Aus der **Tools** klicken Sie im Menü **Erweiterungen und Updates**.
3. In der **Erweiterungen und Updates** wählen Sie im Dialogfeld **Online**.
4. Wenn Sie "NuGet-Paket-Manager" nicht angezeigt wird, geben Sie "NuGet-Paket-Manager" in das Suchfeld ein.
5. Wählen Sie den NuGet-Paket-Manager, und klicken Sie auf **herunterladen**.
6. Nachdem der Download abgeschlossen ist, werden Sie aufgefordert, diese zu installieren.
7. Nach Abschluss der Installation werden Sie möglicherweise aufgefordert, Visual Studio neu zu starten.

![](self-host-a-web-api/_static/image3.png)

## <a name="add-the-web-api-nuget-package"></a>Fügen Sie das Web-API-NuGet-Paket hinzu

Nach der Installation von NuGet-Paket-Manager fügen Sie hinzu, zum Projekt Web-API Self-Host.

1. Aus der **Tools** klicken Sie im Menü **Bibliothekspaket-Manager**. *Hinweis*: Wenn Sie nicht dieses Menü Element sehen, stellen Sie sicher, dass dieses NuGet-Paket-Manager ordnungsgemäß installiert.
2. Wählen Sie **NuGet-Pakete für Projektmappe verwalten...**
3. In der **NugGet-Pakete verwalten** wählen Sie im Dialogfeld **Online**.
4. Geben Sie in das Suchfeld &quot;Microsoft.AspNet.WebApi.SelfHost&quot;.
5. Wählen Sie die ASP.NET Web API Self Host-Paket, und klicken Sie auf **installieren**.
6. Nachdem das Paket installiert wurde, klicken Sie auf **schließen** um das Dialogfeld zu schließen.

> [!NOTE]
> Stellen Sie sicher, dass das Paket mit dem Namen Microsoft.AspNet.WebApi.SelfHost nicht AspNetWebApi.SelfHost installieren.


![](self-host-a-web-api/_static/image4.png)

## <a name="create-the-model-and-controller"></a>Erstellen Sie das Modell und der Controller

Dieses Lernprogramm verwendet die gleichen Modell und Controller-Klassen wie der [Einstieg](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) Lernprogramm.

Fügen Sie eine öffentliche Klasse, die mit dem Namen `Product`.

[!code-csharp[Main](self-host-a-web-api/samples/sample1.cs)]

Fügen Sie eine öffentliche Klasse, die mit dem Namen `ProductsController`. Leiten Sie diese Klasse von **System.Web.Http.ApiController**.

[!code-csharp[Main](self-host-a-web-api/samples/sample2.cs)]

Weitere Informationen zu den Code in diesem Controller, finden Sie unter der [Einstieg](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) Lernprogramm. Dieser Controller definiert drei GET-Aktionen:

| URI | Beschreibung |
| --- | --- |
| / api /-Produkte | Ruft eine Liste aller Produkte. |
| /API/Produkte/*Id* | Abrufen eines Produkts nach ID auf. |
| / API/Produkte /? Kategorie =*Kategorie* | Abrufen einer Liste der Produkte nach Kategorie. |

## <a name="host-the-web-api"></a>Hosten Sie die Web-API

Öffnen Sie die Datei "Program.cs", und fügen Sie die folgenden using-Anweisungen:

[!code-csharp[Main](self-host-a-web-api/samples/sample3.cs)]

Fügen Sie folgenden Code, der **Programm** Klasse.

[!code-csharp[Main](self-host-a-web-api/samples/sample4.cs)]

## <a name="optional-add-an-http-url-namespace-reservation"></a>(Optional) Fügen Sie eine HTTP-URL-Namespace-Reservierung hinzu.

Diese Anwendung überwacht `http://localhost:8080/`. Standardmäßig sind Administratorrechte erforderlich, wenn Sie an einer bestimmten HTTP-Adresse lauschen. Wenn Sie das Lernprogramm ausführen, daher erhalten Sie möglicherweise diesen Fehler: "HTTP konnte URL http://+:8080/ nicht registriert" Es gibt zwei Möglichkeiten, um diesen Fehler zu vermeiden:

- Ausführen von Visual Studio mit erweiterten Administratorberechtigungen, oder
- Verwenden Sie Netsh.exe, um den Kontoberechtigungen reserviert die URL zu gewähren.

Um Netsh.exe verwenden möchten, öffnen Sie eine Eingabeaufforderung mit Administratorrechten, und geben Sie den folgenden Befehl: folgenden Befehl:

[!code-console[Main](self-host-a-web-api/samples/sample5.cmd)]

wobei *Computer\Benutzername* Ihr Benutzerkonto ist.

Wenn Sie Selbsthosting abgeschlossen sind, achten Sie darauf, dass Sie die Reservierung zu löschen:

[!code-console[Main](self-host-a-web-api/samples/sample6.cmd)]

## <a name="call-the-web-api-from-a-client-application-c"></a>Rufen Sie die Web-API aus einer Clientanwendung (c#)

Schreiben Sie eine einfache Konsolenanwendung, die die Web-API aufruft.

Fügen Sie der Projektmappe ein neues Konsolenanwendungsprojekt hinzu:

- Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste in der Projektmappe, und wählen **neues Projekt hinzufügen**.
- Erstellen Sie eine neue Konsolenanwendung mit dem Namen &quot;ClientApp&quot;.

![](self-host-a-web-api/_static/image5.png)

Verwenden Sie NuGet Package Manager, die ASP.NET Web API Core Libraries Paket hinzuzufügen:

- Wählen Sie im Menü Extras **Bibliothekspaket-Manager**.
- Wählen Sie **NuGet-Pakete für Projektmappe verwalten...**
- In der **NuGet-Pakete verwalten** wählen Sie im Dialogfeld **Online**.
- Geben Sie in das Suchfeld &quot;Microsoft.AspNet.WebApi.Client&quot;.
- Wählen Sie das Paket für Microsoft ASP.NET Web API Client Libraries, und klicken Sie auf **installieren**.

Fügen Sie einen Verweis auf das Projekt SelfHost in ClientApp:

- Im Projektmappen-Explorer mit der rechten Maustaste ClientApp-Projekt.
- Wählen Sie **Verweis hinzufügen**.
- In der **Verweis-Manager** Dialogfeld unter **Lösung**Option **Projekte**.
- Wählen Sie das Projekt SelfHost.
- Klicken Sie auf **OK**.

![](self-host-a-web-api/_static/image6.png)

Öffnen Sie die Client/Program.cs-Datei. Fügen Sie die folgenden **mit** Anweisung:

[!code-csharp[Main](self-host-a-web-api/samples/sample7.cs)]

Fügen Sie einen statischen **"HttpClient"** Instanz:

[!code-csharp[Main](self-host-a-web-api/samples/sample8.cs)]

Fügen Sie die folgenden Methoden zum Auflisten aller Produkte, ein Produkt nach ID-Liste und Liste Produkte nach Kategorie.

[!code-csharp[Main](self-host-a-web-api/samples/sample9.cs)]

Jede dieser Methoden erfolgt nach dem gleichen Muster:

1. Rufen Sie **HttpClient.GetAsync** zum Senden einer GET-Anforderung zum entsprechenden URI.
2. Rufen Sie **HttpResponseMessage.EnsureSuccessStatusCode**. Diese Methode löst eine Ausnahme aus, der HTTP-Antwort-Status ist ein Fehlercode.
3. Rufen Sie **ReadAsAsync&lt;T&gt;**  um einen CLR-Typ der HTTP-Antwort zu deserialisieren. Diese Methode ist eine Erweiterungsmethode, die in definierten **System.Net.Http.HttpContentExtensions**.

Die **GetAsync** und **ReadAsAsync** Methoden sind sowohl asynchrone. Diese zurückgeben **Aufgabe** Objekte, die den asynchronen Vorgang darstellen. Abrufen der **Ergebnis** -Eigenschaft sperrt den Thread, bis der Vorgang abgeschlossen ist.

Weitere Informationen zum Verwenden von "HttpClient", einschließlich der Erstellung nicht blockierende Aufrufe, finden Sie unter [ein Web-API aus einer .NET Client aufrufen](../advanced/calling-a-web-api-from-a-net-client.md).

Vor dem Aufrufen dieser Methoden, legen Sie die BaseAddress-Eigenschaft auf die Instanz "HttpClient", "`http://localhost:8080`". Zum Beispiel:

[!code-csharp[Main](self-host-a-web-api/samples/sample10.cs)]

Diese sollte folgende Ausgabe. (Denken Sie daran, führen Sie die Anwendung SelfHost zuerst).

[!code-console[Main](self-host-a-web-api/samples/sample11.cmd)]

![](self-host-a-web-api/_static/image7.png)
