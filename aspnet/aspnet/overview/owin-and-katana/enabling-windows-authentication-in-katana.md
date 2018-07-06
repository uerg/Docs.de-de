---
uid: aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
title: Aktivieren der Windows-Authentifizierung in Katana | Microsoft-Dokumentation
author: MikeWasson
description: 'In diesem Artikel wird das Aktivieren der Windows-Authentifizierung in Katana veranschaulicht. Es werden zwei Szenarien behandelt: von IIS für das Katana-Host, und "HttpListener" verwendet, um Kat selbst hosten...'
ms.author: aspnetcontent
ms.date: 07/30/2013
ms.assetid: 82324ef0-3b75-4f63-a217-76ef4036ec93
msc.legacyurl: /aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
msc.type: authoredcontent
ms.openlocfilehash: 80bdc3c76c8867dc559e80a794ac8bee84b47646
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37826325"
---
<a name="enabling-windows-authentication-in-katana"></a>Aktivieren der Windows-Authentifizierung in Katana
====================
durch [Mike Wasson](https://github.com/MikeWasson)

> In diesem Artikel wird das Aktivieren der Windows-Authentifizierung in Katana veranschaulicht. Es werden zwei Szenarien behandelt: von IIS für das Katana-Host, und "HttpListener" verwendet, um Katana selbst in einem benutzerdefinierten Prozess hosten. Nochmals vielen Dank an David Matson, Barry Dorrans und Chris Ross für die Durchsicht dieses Artikels.


Katana ist Microsofts Umsetzung dieser [OWIN](http://owin.org/), Open Web Interface for .NET. Sie erhalten eine Einführung in die OWIN und Katana [hier](an-overview-of-project-katana.md). Die OWIN-Architektur besteht aus mehreren Ebenen:

- Host: Verwaltet den Prozess, in dem die OWIN-Pipeline ausgeführt wird.
- Server: Ein Netzwerksocket geöffnet und Lauscht auf Anforderungen.
- Middleware: Verarbeitet HTTP-Anforderung und Antwort.

Katana bietet derzeit zwei Server, die beide über integrierte Windows-Authentifizierung unterstützt auf:

- **Microsoft.Owin.Host.SystemWeb**. IIS wird mit der ASP.NET-Pipeline verwendet.
- **Microsoft.Owin.Host.HttpListener**. Verwendet [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx). Dieser Server ist zurzeit die Standardoption, wenn Sie Selbsthosting Katana.

> [!NOTE]
> Katana bietet derzeit OWIN-Middleware für die Windows-Authentifizierung keine, da diese Funktion bereits in der Server verfügbar sind.


## <a name="windows-authentication-in-iis"></a>Windows-Authentifizierung in IIS

Verwenden "Microsoft.owin.Host.systemweb", können Sie einfach Windows-Authentifizierung in IIS aktivieren.

Wir erstellen zunächst eine neue ASP.NET-Anwendung mit mithilfe der Projektvorlage "Leere ASP.NET-Webanwendung".

![](enabling-windows-authentication-in-katana/_static/image1.png)

Als Nächstes fügen Sie die NuGet-Pakete hinzu. Von der **Tools** , wählen Sie im Menü **Bibliothekspaket-Manager**, und wählen Sie dann **-Paket-Manager-Konsole**. Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl aus:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample1.cmd)]

Nun fügen Sie eine Klasse, die mit dem Namen `Startup` durch den folgenden Code:

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample2.cs)]

Das ist alles, was, die Sie benötigen zum Erstellen einer "Hello World"-Anwendung für OWIN, die unter IIS. Drücken Sie F5, um die Anwendung zu debuggen. "Hello World!" sollte angezeigt werden. im Browserfenster.

![](enabling-windows-authentication-in-katana/_static/image2.png)

Aktivieren Sie als Nächstes Windows-Authentifizierung in IIS Express. Von der **Ansicht** , wählen Sie im Menü **Eigenschaften**. Klicken Sie auf den Projektnamen im Projektmappen-Explorer auf die Projekteigenschaften anzuzeigen.

In der **Eigenschaften** legen **anonyme Authentifizierung** zu **deaktiviert** und **Windows-Authentifizierung** zu  **Aktiviert**.

![](enabling-windows-authentication-in-katana/_static/image3.png)

Wenn Sie die Anwendung aus Visual Studio ausführen, müssen die IIS Express, dass Windows-Anmeldeinformationen des Benutzers. Dies sehen Sie mithilfe von [Fiddler](http://fiddler2.com/home) oder einem anderen HTTP-debugging-Tools. Hier ist ein Beispiel für HTTP-Antwort:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample3.cmd?highlight=1,5-6)]

Der WWW-Authenticate-Header in dieser Antwort anzugeben, dass der Server unterstützt die [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) -Protokoll, das entweder Kerberos oder NTLM verwendet.

Später, wenn Sie die Anwendung auf einem Server bereitstellen, führen Sie [folgendermaßen](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication) um Windows-Authentifizierung in IIS auf diesem Server zu aktivieren.

## <a name="windows-authentication-in-httplistener"></a>Windows-Authentifizierung in HttpListener

Wenn Sie zum selfhosten von Katana Microsoft.Owin.Host.HttpListener verwenden, können Sie die Windows-Authentifizierung aktivieren, direkt auf die **HttpListener** Instanz.

Erstellen Sie zunächst eine neue Konsolenanwendung. Als Nächstes fügen Sie die NuGet-Pakete hinzu. Von der **Tools** , wählen Sie im Menü **Bibliothekspaket-Manager**, und wählen Sie dann **-Paket-Manager-Konsole**. Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl aus:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample4.cmd)]

Nun fügen Sie eine Klasse, die mit dem Namen `Startup` durch den folgenden Code:

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample5.cs)]

Diese Klasse implementiert die gleichen "Hello World"-Beispiel vorher, aber sie setzt auch die Windows-Authentifizierung, als Authentifizierungsschema.

In der `Main` funktioniert, starten Sie die OWIN-Pipeline:

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample6.cs)]

Sie können eine Anforderung senden, in Fiddler ausführen, um sicherzustellen, dass die Anwendung Windows-Authentifizierung verwendet:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample7.cmd?highlight=1,4-5)]

## <a name="related-topics"></a>Verwandte Themen

[Übersicht über das Katana-Projekt](an-overview-of-project-katana.md)

[System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx)

[Grundlegendes zur OWIN-Formularauthentifizierung in MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/07/03/understanding-owin-forms-authentication-in-mvc-5.aspx)
