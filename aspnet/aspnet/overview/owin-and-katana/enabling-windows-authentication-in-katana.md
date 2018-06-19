---
uid: aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
title: Aktivieren der Windows-Authentifizierung in Katana | Microsoft Docs
author: MikeWasson
description: 'In diesem Artikel wird gezeigt, wie die Windows-Authentifizierung in Katana aktiviert. Es werden zwei Szenarien behandelt: Verwenden von IIS zum Host Katana und HttpListener Kat Selbsthosting mithilfe...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 82324ef0-3b75-4f63-a217-76ef4036ec93
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
msc.type: authoredcontent
ms.openlocfilehash: 8a26d356f7abafba021199761f9a49dcb81765c5
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
ms.locfileid: "28033969"
---
<a name="enabling-windows-authentication-in-katana"></a>Aktivieren der Windows-Authentifizierung in Katana
====================
durch [Mike Wasson](https://github.com/MikeWasson)

> In diesem Artikel wird gezeigt, wie die Windows-Authentifizierung in Katana aktiviert. Es werden zwei Szenarien behandelt: Verwenden von IIS zum Host Katana und Verwenden von HttpListener Katana selbst in einem benutzerdefinierten Prozess gehostet. Unser Dank gilt Barry Dorrans, David Matson und Chris Ross zum Überprüfen des in diesem Artikel.


Katana ist Microsoft Implementierung des [OWIN](http://owin.org/), die offene Webschnittstelle für .NET. Informieren Sie sich eine Einführung in die OWIN und Katana [hier](an-overview-of-project-katana.md). Die OWIN-Architektur besteht aus mehreren Ebenen:

- Host: Verwaltet den Prozess, in dem die OWIN-Pipeline ausgeführt wird.
- Server: Ein Netzwerksocket geöffnet und aktiv abgehört.
- Middleware: Verarbeitet die HTTP-Anforderung und Antwort.

Katana bietet derzeit zwei Servern, die beide die integrierte Windows-Authentifizierung unterstützen:

- **Microsoft.Owin.Host.SystemWeb**. Wird IIS mit der ASP.NET-Pipeline verwendet.
- **Microsoft.Owin.Host.HttpListener**. Verwendet [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx). Dieser Server ist derzeit die Standardoption für Selbsthosting Katana.

> [!NOTE]
> Katana bietet derzeit OWIN-Middleware für die Windows-Authentifizierung keine, da diese Funktion bereits in der Server verfügbar ist.


## <a name="windows-authentication-in-iis"></a>Windows-Authentifizierung in IIS

Verwenden "Microsoft.owin.Host.systemweb", können Sie einfach Windows-Authentifizierung in IIS aktivieren.

Zunächst erstellen eine ASP.NET-Anwendung mithilfe der Projektvorlage "Leere ASP.NET-Webanwendung".

![](enabling-windows-authentication-in-katana/_static/image1.png)

Als Nächstes fügen Sie NuGet-Pakete. Aus der **Tools** klicken Sie im Menü **Bibliothekspaket-Manager**, und wählen Sie dann **Package Manager Console**. Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl aus:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample1.cmd)]

Nun fügen Sie eine Klasse, die mit dem Namen `Startup` durch den folgenden Code:

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample2.cs)]

Das ist alles, die müssen Sie eine Anwendung "Hello World" für OWIN, die unter IIS zu erstellen. Drücken Sie F5, um die Anwendung zu debuggen. "Hello World!" sollte angezeigt werden. im Browserfenster.

![](enabling-windows-authentication-in-katana/_static/image2.png)

Aktivieren Sie als Nächstes Windows-Authentifizierung in IIS Express. Aus der **Ansicht** klicken Sie im Menü **Eigenschaften**. Klicken Sie auf den Projektnamen im Projektmappen-Explorer, um die Projekteigenschaften anzuzeigen.

In der **Eigenschaften** legen **anonyme Authentifizierung** auf **deaktiviert** und legen Sie **Windows-Authentifizierung** auf  **Aktiviert**.

![](enabling-windows-authentication-in-katana/_static/image3.png)

Wenn Sie die Anwendung aus Visual Studio ausführen, müssen die IIS Express, dass Windows-Anmeldeinformationen des Benutzers. Sie können dies verschaffen, indem [Fiddler](http://fiddler2.com/home) oder eine andere HTTP-Tool zum Debuggen. Hier ist ein Beispiel für HTTP-Antwort:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample3.cmd?highlight=1,5-6)]

Der WWW-Authenticate-Header in dieser Antwort anzugeben, dass der Server unterstützt die [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) -Protokoll, das entweder Kerberos oder NTLM verwendet.

Später, wenn Sie die Anwendung auf einem Server bereitstellen, führen Sie [Schritte](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication) auf Windows-Authentifizierung in IIS auf diesem Server zu aktivieren.

## <a name="windows-authentication-in-httplistener"></a>Windows-Authentifizierung in HttpListener

Wenn Sie zum Katana Selbsthosting Microsoft.Owin.Host.HttpListener verwenden, können Sie die Windows-Authentifizierung aktivieren, direkt auf die **HttpListener** Instanz.

Erstellen Sie zunächst eine neue Konsolenanwendung. Als Nächstes fügen Sie NuGet-Pakete. Aus der **Tools** klicken Sie im Menü **Bibliothekspaket-Manager**, und wählen Sie dann **Package Manager Console**. Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl aus:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample4.cmd)]

Nun fügen Sie eine Klasse, die mit dem Namen `Startup` durch den folgenden Code:

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample5.cs)]

Diese Klasse implementiert die gleichen "Hello World"-Beispiel vorher, aber Windows-Authentifizierung wird auch als das Authentifizierungsschema festgelegt.

Innerhalb der `Main` -Funktion, die OWIN-Pipeline starten:

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample6.cs)]

Sie können eine Anforderung senden, in Fiddler, um sicherzustellen, dass die Anwendung Windows-Authentifizierung verwendet:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample7.cmd?highlight=1,4-5)]

## <a name="related-topics"></a>Verwandte Themen

[Übersicht über das Katana-Projekt](an-overview-of-project-katana.md)

[System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx)

[Grundlegendes zur Formularauthentifizierung in MVC 5 OWIN](https://blogs.msdn.com/b/webdev/archive/2013/07/03/understanding-owin-forms-authentication-in-mvc-5.aspx)
