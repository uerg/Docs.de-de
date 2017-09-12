---
title: Konfigurieren der Windowsauthentifizierung in ASP.NET Core
author: ardalis
description: 'Gewusst wie: Konfigurieren von Windows-Authentifizierung in ASP.NET Core'
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 7/5/2017
ms.topic: article
ms.assetid: cf119f21-1a2b-49a2-b052-548ccb66ee83
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/windowsauth
ms.openlocfilehash: aa401f956d74680efd3964203af3e8866b129887
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2017
---
# <a name="configure-windows-authentication-in-aspnet-core"></a>Konfigurieren der Windowsauthentifizierung in ASP.NET Core

Durch [Steve Smith](https://ardalis.com)

Windows-Authentifizierung kann für ASP.NET Core-apps, die mit IIS oder WebListener gehostet konfiguriert werden.

## <a name="what-is-windows-authentication"></a>Was ist Windows-Authentifizierung

Windows-Authentifizierung basiert auf dem Betriebssystem zur Authentifizierung von Benutzern von ASP.NET Core-apps. Sie können Windows-Authentifizierung verwenden, wenn Ihre Server in einem Unternehmensnetzwerk mithilfe von Active Directory-Domäne-Identitäten oder andere Windows-Konten zur Identifizierung von Benutzern ausgeführt wird. Windows-Authentifizierung ist eine sichere Form der Authentifizierung, die beste für Intranetumgebungen, in dem Benutzer, Clientanwendungen und Webservern auf derselben Windows-Domäne gehören, geeignet sind.

[Weitere Informationen zu Windows-Authentifizierung und die Installation für IIS](https://docs.microsoft.com/iis/configuration/system.webServer/security/authentication/windowsAuthentication/).

## <a name="enabling-windows-authentication-in-an-aspnet-core-application"></a>Aktivieren der Windows-Authentifizierung in einer ASP.NET Core-Anwendung

Der Visual Studio Web Application-Vorlage kann für die Unterstützung der Windows-Authentifizierung konfiguriert werden.

### <a name="using-the-windows-authentication-app-template"></a>Mithilfe der Windows-Authentifizierung-app-Vorlage

In Visual Studio:
* Erstellen Sie eine neue ASP.NET Core-Webanwendung. 
* Wählen Sie aus der Liste der Vorlagen-Webanwendung.
* Wählen Sie die Schaltfläche "Authentifizierung ändern" und **Windows-Authentifizierung**. 

Führen Sie die app an. Der Benutzername wird angezeigt, in der oberen rechten der app.

![Windows-Authentifizierung-Browser-Screenshot](windowsauth/_static/browser-screenshot.png)

Die Vorlage bereitstellt für Entwicklungen mit IIS Express die zur Verwendung der Windows-Authentifizierung erforderlich Konfiguration. Im nächste Abschnitt zeigt, wie eine ASP.NET Core-app für Windows-Authentifizierung manuell konfigurieren.

### <a name="visual-studio-settings-for-windows-and-anonymous-authentication"></a>Visual Studio-Einstellungen für Windows und anonyme Authentifizierung

Der Visual Studio-Eigenschaftenseite, Registerkarte "Debuggen" bietet das Kontrollkästchen für Windows-Authentifizierung und die anonyme Authentifizierung.

![Windows-Authentifizierung-Browser-Screenshot](windowsauth/_static/vs-auth-property-menu.png)

Sie können auch konfigurieren, diese Eigenschaften in der `launchSettings.json` Datei:

```json
{
  "iisSettings": {
    "windowsAuthentication": true,
    "anonymousAuthentication": false,
    "iisExpress": {
      "applicationUrl": "http://localhost:52171/",
      "sslPort": 0
    }
  } // additional options trimmed
}
```

## <a name="enabling-windows-authentication-with-iis"></a>Aktivieren der Windows-Authentifizierung mit IIS

IIS verwendet den [ASP.NET Core-Modul](xref:fundamentals/servers/aspnet-core-module) (ANCM) zum Hosten von ASP.NET Core-apps. Die ANCM Flüsse Windows-Authentifizierung in IIS standardmäßig. Konfiguration der Windows-Authentifizierung erfolgt innerhalb von IIS nicht auf das Anwendungsprojekt. Die folgenden Abschnitte zeigen, wie IIS-Manager verwenden, um eine ASP.NET Core-Anwendung, um die Windows-Authentifizierung zu konfigurieren:

### <a name="create-a-new-iis-site"></a>Erstellen einer neuen IIS-Website

Geben Sie einen Namen und den Ordner, und lassen sie einen neuen Anwendungspool zu erstellen.

### <a name="customize-authentication"></a>Anpassen der Authentifizierung

Öffnen Sie die Authentifizierung für den Standort.

![Menü für IIS-Authentifizierung](windowsauth/_static/iis-authentication-menu.png)

Deaktivieren Sie anonyme Authentifizierung und aktivieren Sie die Windows-Authentifizierung.

![IIS-Authentifizierungseinstellungen](windowsauth/_static/iis-auth-settings.png)

### <a name="publish-your-project-to-the-iis-site-folder"></a>Veröffentlichen Sie Ihr Projekt in den IIS-Website-Ordner

Mithilfe von Visual Studio oder die .NET Core-CLI *veröffentlichen* Ihrer app in den Zielordner.

![Visual Studio, Dialogfeld "Veröffentlichen"](windowsauth/_static/vs-publish-app.png)

Erfahren Sie mehr über [in IIS veröffentlichen](xref:publishing/iis).

Starten Sie die app aus, um sicherzustellen, dass die Windows-Authentifizierung funktionsfähig ist.

## <a name="enabling-windows-authentication-with-weblistener"></a>Aktivieren der Windows-Authentifizierung mit WebListener

Obwohl Kestrel Windows-Authentifizierung nicht unterstützt, können Sie [WebListener](xref:fundamentals/servers/weblistener) selbst gehostete Szenarien unter Windows unterstützt. Das folgende Beispiel konfiguriert die app Webhost um WebListener mit Windows-Authentifizierung zu verwenden:

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        var host = new WebHostBuilder()
            .UseWebListener(options =>
            {
                options.ListenerSettings.Authentication.Schemes = 
                    AuthenticationSchemes.Negotiate | AuthenticationSchemes.NTLM;
                options.ListenerSettings.Authentication.AllowAnonymous = false;
            })
            .UseContentRoot(Directory.GetCurrentDirectory())
            .UseStartup<Startup>()
            .Build();

        host.Run();
    }
}
```

## <a name="working-with-windows-authentication"></a>Arbeiten mit Windows-Authentifizierung

Wenn Ihre app auf Windows-Authentifizierung und anonymem Zugriff verwendet, können Sie die ``[Authorize]`` und ``[AllowAnonymous]`` Attribute. Apps, die keinen anonymen aktiviert stimmen nicht erfordern ``[Authorize]``; die app so behandelt, als eine Authentifizierung erforderlich ist, werden die anonyme Anforderungen abgelehnt. Beachten Sie, wenn die IIS-Site konfiguriert ist **nicht** anonymen Zugriff erlaubt die ``[AllowAnonymous]`` Attribut führt **nicht** anonyme Anforderungen zulassen. Die ``[AllowAnonymous]`` -Attribut überschreibt ``[Authorize]`` -Attribut Auslastung in apps, die den anonymen Zugriff zulassen.

### <a name="impersonation"></a>Identitätswechsel

ASP.NET Core implementiert Identitätswechsel nicht. Apps, die mit der Anwendungsidentität für alle Anforderungen, die mit app-Pool oder Prozess Identität ausgeführt werden. Wenn Sie explizit eine Aktion im Auftrag eines Benutzers ausführen müssen, verwenden Sie ``WindowsIdentity.RunImpersonated``. Eine einzelne Aktion in diesem Kontext ausgeführt, und schließen Sie dann den Kontext. Beachten Sie, dass ``RunImpersonated`` Async nicht unterstützt und sollte nicht für komplexe Szenarios verwendet werden. Z. B. wird wrapping gesamten Anfragen oder Middleware Ketten nicht unterstützt oder empfohlen.
