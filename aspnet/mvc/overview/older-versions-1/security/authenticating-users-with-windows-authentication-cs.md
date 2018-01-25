---
uid: mvc/overview/older-versions-1/security/authenticating-users-with-windows-authentication-cs
title: Authentifizieren von Benutzern mit Windows-Authentifizierung (c#) | Microsoft Docs
author: microsoft
description: Erfahren Sie, wie Windows-Authentifizierung im Rahmen einer MVC-Anwendung verwenden. Erfahren Sie, wie die Windows-Authentifizierung in Ihre Anwendung Web co aktiviert...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 418bb07e-f369-4119-b4b0-08f890f7abb2
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/security/authenticating-users-with-windows-authentication-cs
msc.type: authoredcontent
ms.openlocfilehash: d52597e65272fa202ef4980924f669dcc4cec593
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="authenticating-users-with-windows-authentication-c"></a>Authentifizieren von Benutzern mit Windows-Authentifizierung (c#)
====================
durch [Microsoft](https://github.com/microsoft)

> Erfahren Sie, wie Windows-Authentifizierung im Rahmen einer MVC-Anwendung verwenden. Erfahren Sie, wie die Windows-Authentifizierung innerhalb Ihrer Anwendung Webkonfigurationsdatei aktiviert und wie die Authentifizierung mit IIS konfiguriert. Schließlich erfahren Sie, wie Sie das Attribut [Authorize] zu verwenden, um den Zugriff auf Controlleraktionen auf bestimmte Windows-Benutzer oder Gruppen beschränken.


Das Ziel dieses Lernprogramms wird erläutert, wie Sie nutzen können die Sicherheit-Funktionen, die in Internetinformationsdienste (IIS) mit einem Kennwort schützen der Sichten in einer MVC-Anwendung. Erfahren Sie, wie ermöglichen Controlleraktionen, nur von bestimmten Windows-Benutzer oder Mitglieder von bestimmten Windows-Gruppen aufgerufen werden soll.

Mithilfe der Windows-Authentifizierung ist sinnvoll, wenn Sie eine interne Unternehmenswebsite (einer Intranetsite) erstellen und Benutzer zu ihren standardmäßigen Windows-Benutzernamen und Kennwörter zu verwenden, wenn die Website zugreifen können sollen. Wenn Sie eine nach außen zugängliche Website (Internet-Website) erstellen sollten Sie die Formularauthentifizierung verwenden.

#### <a name="enabling-windows-authentication"></a>Aktivieren der Windowsauthentifizierung

Wenn Sie eine neue ASP.NET MVC-Anwendung erstellen, ist die Windows-Authentifizierung nicht standardmäßig aktiviert. Formularauthentifizierung ist der standardauthentifizierungstyp für MVC-Anwendungen aktiviert. Sie müssen Windows-Authentifizierung aktivieren, indem die Webkonfigurationsdatei (web.config) für Ihre MVC-Anwendung ändern. Suchen der &lt;Authentifizierung&gt; Abschnitt, und ändern, um Windows anstatt der Formularauthentifizierung wie folgt verwenden:

[!code-xml[Main](authenticating-users-with-windows-authentication-cs/samples/sample1.xml)]

Wenn Sie Windows-Authentifizierung aktivieren, wird Ihre Webserver zum Authentifizieren von Benutzern verantwortlich. Es gibt in der Regel zwei verschiedene Arten von Webservern, die Sie beim Erstellen und Bereitstellen einer ASP.NET MVC-Anwendung verwenden.

Zunächst verwenden Sie beim Entwickeln einer MVC-Anwendung kann den ASP.NET Development Web Server mit Visual Studio enthalten. Standardmäßig führt den ASP.NET Development Web Server alle Seiten im Kontext des aktuellen Windows-Kontos (alle Konten, die Sie zum Anmelden verwendet in Windows).

Der ASP.NET Development Web Server unterstützt auch die NTLM-Authentifizierung. Sie können die NTLM-Authentifizierung aktivieren, durch den Namen des Projekts im Projektmappen-Explorer-Fenster mit der rechten Maustaste klicken und Eigenschaften auswählen. Als nächstes wählen Sie die Registerkarte "Web", und aktivieren das Kontrollkästchen "NTLM" (siehe Abbildung 1).

**Abbildung 1: Aktivieren der NTLM-Authentifizierung für den ASP.NET Development Web Server**

![clip_image002](authenticating-users-with-windows-authentication-cs/_static/image1.jpg)

Für eine produktionsanwendung Web bei Verwendung auf der Seite IIS als Webserver. IIS unterstützt mehrere Typen von Authentifizierung einschließlich:

- Basic-Authentifizierung –, die als Teil des Protokolls HTTP 1.0 definiert werden. Benutzernamen und Kennwörter im Klartext (Base64-codiert) über das Internet gesendet. -Digest-Authentifizierung – sendet einen Hash ein Kennwort anstelle des Kennworts selbst, über das Internet. -Integrierte Windows (NTLM)-Authentifizierung – die beste Art der Authentifizierung in Intranetumgebungen mithilfe von Windows verwenden. -Authentifizierung – aktiviert die Authentifizierung mit einem Zertifikat für die clientseitige Zertifikat. Das Zertifikat einem Windows-Benutzerkonto zugeordnet wird.

> [!NOTE] 
> 
> Eine ausführlichere Übersicht über diese verschiedenen Typen der Authentifizierung finden Sie unter [https://msdn.microsoft.com/library/aa292114(VS.71).aspx](https://msdn.microsoft.com/library/aa292114(VS.71).aspx).


Sie können Internetinformationsdienste-Manager verwenden, um eine bestimmte Art von Authentifizierung zu aktivieren. Denken Sie daran, dass alle Typen der Authentifizierung nicht bei jedem Betriebssystem verfügbar sind. Wenn Sie IIS 7.0 mit Windows Vista verwenden, müssen Sie darüber hinaus die verschiedenen Typen von Windows-Authentifizierung aktivieren, bevor sie in der Internetinformationsdienste-Manager angezeigt werden. Open **der Systemsteuerung unter Programme, Programme und Funktionen, die Windows-Funktionen ein- oder ausschalten**, und erweitern Sie den Internetinformationsdienste (IIS)-Knoten (siehe Abbildung 2).

**Abbildung 2: Aktivieren der Windows IIS-Funktionen**

![clip_image004](authenticating-users-with-windows-authentication-cs/_static/image2.jpg)

Verwenden Internetinformationsdienste (IIS), können Sie aktivieren oder deaktivieren verschiedene Arten der Authentifizierung. Abbildung 3 veranschaulicht beispielsweise deaktiviert die anonyme Authentifizierung und ermöglicht integrierte Windows Authentifizierung (NTLM) bei Verwendung von IIS 7.0.

**Abbildung 3: Aktivieren der integrierten Windows-Authentifizierung**

![clip_image006](authenticating-users-with-windows-authentication-cs/_static/image3.jpg)

#### <a name="authorizing-windows-users-and-groups"></a>Autorisieren des Windows-Benutzer und Gruppen

Nachdem Sie Windows-Authentifizierung aktiviert haben, können Sie das Attribut [Authorize] zum Steuern des Zugriffs auf Domänencontrollern oder Controlleraktionen verwenden. Dieses Attribut kann eine gesamte MVC-Controller oder eine bestimmte Controlleraktion angewendet werden.

Beispielsweise macht der Home-Controller 1 Auflisten von drei Aktionen, die mit dem Namen Index() CompanySecrets() und StephenSecrets() verfügbar. Jeder Benutzer kann die Index() Aktion aufrufen. Allerdings können nur Mitglieder der Windows-Manager-Gruppe die CompanySecrets() Aktion aufrufen. Schließlich kann nur die Windows-Domänenbenutzer mit dem Namen Stephen (in der Domäne "Redmond") die StephenSecrets() Aktion aufrufen.

**1 – Controllers\HomeController. cs auflisten**

[!code-csharp[Main](authenticating-users-with-windows-authentication-cs/samples/sample2.cs)]

> [!NOTE] 
> 
> Aufgrund von Windows Benutzerkontensteuerung (UAC), bei der Arbeit mit Windows Vista oder Windows Server 2008, verhält sich der lokalen Administratorgruppe anders als andere Gruppen. Das Attribut [Authorize] erkannt nicht Mitglied der lokalen Administratorgruppe ordnungsgemäß, sofern Sie UAC-Einstellungen des Computers ändern.


Genau was geschieht, wenn Sie versuchen, eine Controlleraktion aufrufen, ohne dass die richtigen Berechtigungen werden, hängt von den Typ der Authentifizierung aktiviert. Standardmäßig, wenn Sie den ASP.NET Development Server verwenden erhalten Sie einfach eine leere Seite. Seite wird angezeigt, mit einem **401 nicht autorisiert** HTTP-Antwort-Status.

Wenn Sie andererseits, verwenden Sie IIS mit anonyme Authentifizierung deaktiviert und die Standardauthentifizierung aktiviert, und Sie wiederholt ein Dialogfeld-anmeldeaufforderung jedes Mal, die Sie anfordern, dass die geschützte Seite (siehe Abbildung 4).

**Abbildung 4 – Standardauthentifizierung Anmeldedialogfeld**

![clip_image008](authenticating-users-with-windows-authentication-cs/_static/image4.jpg)

#### <a name="summary"></a>Zusammenfassung

In diesem Lernprogramm wird erläutert, wie Sie die Windows-Authentifizierung im Kontext einer ASP.NET MVC-Anwendung verwenden können. Sie haben gelernt, wie die Windows-Authentifizierung innerhalb Ihrer Anwendung Webkonfigurationsdatei aktiviert und wie die Authentifizierung mit IIS konfiguriert. Schließlich haben Sie gelernt, wie Sie das Attribut [Authorize] zu verwenden, um den Zugriff auf Controlleraktionen auf bestimmte Windows-Benutzer oder Gruppen beschränken.

>[!div class="step-by-step"]
[Zurück](authenticating-users-with-forms-authentication-cs.md)
[Weiter](preventing-javascript-injection-attacks-cs.md)
