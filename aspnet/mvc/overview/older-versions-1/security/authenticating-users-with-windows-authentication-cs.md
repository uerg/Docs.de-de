---
uid: mvc/overview/older-versions-1/security/authenticating-users-with-windows-authentication-cs
title: Authentifizieren von Benutzern mit der Windows-Authentifizierung (c#) | Microsoft-Dokumentation
author: microsoft
description: Erfahren Sie, wie Sie Windows-Authentifizierung im Rahmen einer MVC-Anwendung verwenden. Erfahren Sie, wie zum Aktivieren der Windows-Authentifizierung in Ihrer Anwendung Web co...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 418bb07e-f369-4119-b4b0-08f890f7abb2
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/security/authenticating-users-with-windows-authentication-cs
msc.type: authoredcontent
ms.openlocfilehash: 0261f3b989d721e6e9aae8fa5766e02c8bca7b63
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37397803"
---
<a name="authenticating-users-with-windows-authentication-c"></a>Authentifizieren von Benutzern mit der Windows-Authentifizierung (c#)
====================
durch [Microsoft](https://github.com/microsoft)

> Erfahren Sie, wie Sie Windows-Authentifizierung im Rahmen einer MVC-Anwendung verwenden. Erfahren Sie, wie Windows-Authentifizierung in Ihrer Anwendung-Web-Konfigurationsdatei zu aktivieren und Konfigurieren der Authentifizierung mit IIS. Schließlich erfahren Sie, wie Sie die [Authorize]-Attribut zu verwenden, um den Zugriff auf Controlleraktionen an bestimmte Windows-Benutzer oder Gruppen beschränken.


Das Ziel in diesem Tutorial wird beschrieben, wie Sie nutzen können Security Features in Internet Information Services integriert werden, um das Kennwort die Sichten in Ihre MVC-Anwendungen schützen. Erfahren Sie, wie um Controlleraktionen aufgerufen werden, nur von bestimmten Windows-Benutzer oder Benutzer, die Mitglieder von bestimmten Windows-Gruppen zu ermöglichen.

Mithilfe der Windows-Authentifizierung ist sinnvoll, wenn Sie eine unternehmensinterne-Website (eine Intranetsite) erstellen und Ihre Benutzer, um die standardmäßige Windows-Benutzernamen und Kennwörter zu verwenden, wenn die Website zugreifen zu können. Bei der Erstellung einer aus nach außen mit Internetzugriff-Website (eine Internetwebsite) erwägen Sie stattdessen unter Verwendung der Formularauthentifizierung.

#### <a name="enabling-windows-authentication"></a>Aktivieren der Windows-Authentifizierung

Wenn Sie eine neue ASP.NET MVC-Anwendung erstellen, ist die Windows-Authentifizierung nicht standardmäßig aktiviert. Formularauthentifizierung ist der standardauthentifizierungstyp für MVC-Anwendungen aktiviert. Sie müssen Windows-Authentifizierung aktivieren, durch Ändern der Webkonfigurationsdatei (web.config) für Ihre MVC-Anwendung. Suchen der &lt;Authentifizierung&gt; aus, und ändern, um Windows zu verwenden, anstatt der Formularauthentifizierung wie folgt:

[!code-xml[Main](authenticating-users-with-windows-authentication-cs/samples/sample1.xml)]

Wenn Sie Windows-Authentifizierung aktivieren, wird Ihre Webserver zum Authentifizieren von Benutzern verantwortlich. Es gibt in der Regel zwei verschiedene Arten von Webservern, mit denen Sie beim Erstellen und Bereitstellen einer ASP.NET MVC-Anwendung.

Zunächst verwenden Sie beim Entwickeln einer MVC-Anwendung kann den ASP.NET Development Web Server mit Visual Studio enthalten. Standardmäßig führt ASP.NET Development Web Server alle Seiten im Kontext des aktuellen Windows-Kontos (jeweils passende Konto, das Sie zum Anmelden verwendet in Windows).

ASP.NET Web Development Server unterstützt auch die NTLM-Authentifizierung. Sie können die NTLM-Authentifizierung aktivieren, indem Sie mit der rechten Maustaste in des Namens des Projekts im Projektmappen-Explorer-Fenster und Eigenschaften auswählen. Klicken Sie dann die Registerkarte "Web" und aktivieren Sie das Kontrollkästchen für die NTLM (siehe Abbildung 1).

**Abbildung 1: Aktivieren der NTLM-Authentifizierung für den ASP.NET Development Web Server**

![clip_image002](authenticating-users-with-windows-authentication-cs/_static/image1.jpg)

Für eine Produktions-Web-Anwendung verwenden Sie auf der Hand Sie IIS als Webserver. IIS unterstützt mehrere Typen der Authentifizierung, einschließlich:

- Basic-Authentifizierung –, die als Teil des Protokolls HTTP 1.0 definiert werden. Benutzernamen und Kennwörter im Klartext (Base64-codiert) über das Internet gesendet. -Digest-Authentifizierung – sendet einen Hashcode für ein Kennwort, anstatt das Kennwort, über das Internet. -Integrierte Windows (NTLM)-Authentifizierung – die beste Art der Authentifizierung in Intranetumgebungen, die mithilfe von Windows verwenden. -Authentifizierung – aktiviert die Authentifizierung mit einem Zertifikat für die clientseitige Zertifikat. Das Zertifikat wird ein Windows-Benutzerkonto zugeordnet.

> [!NOTE] 
> 
> Eine ausführlichere Übersicht über die unterschiedlichen Arten von Authentifizierung, finden Sie unter [ https://msdn.microsoft.com/library/aa292114(VS.71).aspx ](https://msdn.microsoft.com/library/aa292114(VS.71).aspx).


Sie können den Internetinformationsdienste-Manager verwenden, um einen bestimmten Typ von Authentifizierung zu aktivieren. Denken Sie daran, dass alle Arten der Authentifizierung nicht bei jedem Betriebssystem verfügbar sind. Wenn Sie IIS 7.0 mit Windows Vista verwenden, müssen Sie außerdem die verschiedenen Typen von Windows-Authentifizierung zu aktivieren, bevor sie in der Internetinformationsdienste-Manager angezeigt. Open **Systemsteuerung unter Programme, Programme und Funktionen, aktivieren Sie Windows-Funktionen ein- oder ausschalten**, und erweitern Sie den Knoten für die Internet Information Services (siehe Abbildung 2).

**Abbildung 2 – aktivieren Windows IIS-features**

![clip_image004](authenticating-users-with-windows-authentication-cs/_static/image2.jpg)

Verwenden Internet Information Services, können Sie aktivieren oder deaktivieren verschiedene Arten der Authentifizierung. Abbildung 3 zeigt beispielsweise, deaktiviert die anonyme Authentifizierung und integrierte Windows (NTLM) durch Aktivieren der Authentifizierung bei Verwendung von IIS 7.0.

**Abbildung 3: Aktivieren der integrierten Windows-Authentifizierung**

![clip_image006](authenticating-users-with-windows-authentication-cs/_static/image3.jpg)

#### <a name="authorizing-windows-users-and-groups"></a>Autorisieren des Windows-Benutzer und Gruppen

Nachdem Sie Windows-Authentifizierung aktiviert haben, können Sie die [Authorize]-Attribut, zum Steuern des Zugriffs auf Controller oder Controlleraktionen. Dieses Attribut kann auf einen gesamten MVC-Controller oder eine bestimmte Controlleraktion angewendet werden.

Beispielsweise macht der Home-Controller in Codebeispiel 1 drei Aktionen, die mit dem Namen Index() CompanySecrets() und StephenSecrets() verfügbar. Jeder Benutzer kann die Index() Aktion aufrufen. Allerdings können nur Mitglieder der lokalen Windows-Gruppe von Managers CompanySecrets() Aktion aufrufen. Schließlich kann nur die Windows Domänenbenutzer mit dem Namen Stephen (in der Domäne "Redmond") die StephenSecrets() Aktion aufrufen.

**Codebeispiel 1 – Controllers\HomeController. cs**

[!code-csharp[Main](authenticating-users-with-windows-authentication-cs/samples/sample2.cs)]

> [!NOTE] 
> 
> Aufgrund von Windows User Account Control (UAC), bei der Arbeit mit Windows Vista oder Windows Server 2008, verhält sich der lokalen Gruppe "Administratoren" anders als andere Gruppen. Die [Authorize]-Attribut wird nicht ordnungsgemäß ein Mitglied der lokalen Gruppe "Administratoren" erkennen, es sei denn, Sie UAC-Einstellungen Ihres Computers ändern.


Genau was geschieht, wenn Sie versuchen, eine Controlleraktion aufgerufen wird, ohne die richtigen Berechtigungen, hängt von den Typ der Authentifizierung aktiviert. Standardmäßig wird bei der ASP.NET Development Server verwenden erhalten Sie einfach eine leere Seite. Die Seite wird bereitgestellt, mit einem **401 nicht autorisiert** HTTP-Antwortstatus.

Wenn Sie auf der anderen Seite verwenden Sie IIS mit anonyme Authentifizierung deaktiviert und die Standardauthentifizierung aktiviert, und Sie behalten eine Aufforderung zur Benutzeranmeldung Dialogfeld jedes Mal, die Sie anfordern, dass die geschützte Seite abrufen (siehe Abbildung 4).

**Abbildung 4 – Standardauthentifizierung-Anmeldedialogfeld**

![clip_image008](authenticating-users-with-windows-authentication-cs/_static/image4.jpg)

#### <a name="summary"></a>Zusammenfassung

In diesem Tutorial wird erläutert, wie Sie Windows-Authentifizierung im Kontext einer ASP.NET MVC-Anwendung verwenden können. Sie haben gelernt, wie Windows-Authentifizierung in Ihrer Anwendung-Web-Konfigurationsdatei zu aktivieren und Konfigurieren der Authentifizierung mit IIS. Zum Schluss, wie Sie die [Authorize]-Attribut zu verwenden, um den Zugriff auf Controlleraktionen an bestimmte Windows-Benutzer oder Gruppen beschränken.

> [!div class="step-by-step"]
> [Zurück](authenticating-users-with-forms-authentication-cs.md)
> [Weiter](preventing-javascript-injection-attacks-cs.md)
