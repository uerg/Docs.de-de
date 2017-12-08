---
uid: mvc/overview/older-versions-1/security/authenticating-users-with-forms-authentication-vb
title: Authentifizieren von Benutzern mit der Formularauthentifizierung (VB) | Microsoft Docs
author: microsoft
description: "Erfahren Sie, wie Sie das Attribut [Authorize] verwenden bestimmte Seiten in der MVC-Anwendung mit einem Kennwort zu schützen. Erfahren Sie, wie mit der Web Site Administration zu..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 4341f5b1-6fe5-44c5-8b8a-18fa84f80177
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/security/authenticating-users-with-forms-authentication-vb
msc.type: authoredcontent
ms.openlocfilehash: c7d52e51158575c674264efd19c81de9b077d27b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="authenticating-users-with-forms-authentication-vb"></a>Authentifizieren von Benutzern bei der Formularauthentifizierung (VB)
====================
durch [Microsoft](https://github.com/microsoft)

> Erfahren Sie, wie Sie das Attribut [Authorize] verwenden bestimmte Seiten in der MVC-Anwendung mit einem Kennwort zu schützen. Sie erfahren Sie, wie die Websiteverwaltungs-Tool zum Erstellen und Verwalten von Benutzern und Rollen verwenden. Sie erfahren außerdem konfigurieren, in dem Konto und die entsprechende Benutzerinformationen gespeichert ist.


Das Ziel dieses Lernprogramms wird zur Erläuterung der Verwendung von Formularen Authentifizierung mit einem Kennwort schützen, die Ansichten in Ihre ASP.NET MVC-Anwendungen. Sie erfahren Sie, wie die Websiteverwaltungs-Tool verwenden, um Benutzer und Rollen zu erstellen. Sie erfahren außerdem, wie Sie verhindern, dass nicht autorisierte Benutzer Controlleraktionen aufrufen. Schließlich erfahren Sie, wie so konfigurieren, wo die Benutzernamen und Kennwörter gespeichert werden.

#### <a name="using-the-web-site-administration-tool"></a>Verwenden die Websiteverwaltungs-Tool

Bevor wir weitere Schritte sind, sollten wir zunächst einige Benutzer und Rollen erstellen. Die einfachste Möglichkeit zum Erstellen neuer Benutzer und Rollen ist Visual Studio 2008 Websiteverwaltungs-Tool nutzen. Sie können dieses Tool starten, indem Sie im Menü die Option **Projekt, ASP.NET-Konfiguration**. Sie können alternativ die Websiteverwaltungs-Tool starten, indem Sie auf das Symbol "(etwas furchterregend)" von der Hammer Erreichens der ganzen Welt, die am oberen Rand der Projektmappen-Explorer-Fenster angezeigt wird (siehe Abbildung 1).

**Abbildung 1: Starten die Websiteverwaltungs-Tool**

![clip_image002 [4]](authenticating-users-with-forms-authentication-vb/_static/image1.jpg)

Innerhalb der Websiteverwaltungs-Tool erstellen Sie neue Benutzer und Rollen durch Auswählen der Registerkarte "Sicherheit". Klicken Sie auf die **Benutzer erstellen** Link zum Erstellen eines neuen Benutzers mit dem Namen Stephen (siehe Abbildung 2). Geben Sie den Benutzer Stephen mit jedes Kennwort, das gewünschte (z. B. *geheimen*).

**Abbildung 2 – Erstellen eines neuen Benutzers**

![clip_image004 [4]](authenticating-users-with-forms-authentication-vb/_static/image2.jpg)

Neue Rollen wird vom ersten Aktivieren der Rollen und definieren eine oder mehrere Rollen erstellen. Rollen aktivieren, indem Sie auf die **Rollen aktivieren** Link. Als Nächstes erstellen Sie eine Rolle mit dem Namen *Administratoren* durch Klicken auf die **erstellen oder Verwalten von Rollen** verknüpfen (siehe Abbildung 3).

**Abbildung 3: Erstellen einer neuen Rolle**

![clip_image006 [4]](authenticating-users-with-forms-authentication-vb/_static/image3.jpg)

Schließlich erstellen Sie einen neuen Benutzer mit dem Namen Sally und "Administratoren" Sally zuordnen, indem Sie auf den Link für die Create User und Administratoren beim Sally erstellen (siehe Abbildung 4).

**Abbildung 4 – Hinzufügen eines Benutzers zu einer Rolle**

![clip_image008 [4]](authenticating-users-with-forms-authentication-vb/_static/image4.jpg)

Wenn alle als und ausgeführt, sollten Sie zwei neue Benutzer, die mit dem Namen Stephen und Sally haben. Sie sollten auch eine neue Rolle mit dem Namen Administrators verfügen. Sally ist ein Mitglied der Rolle "Administratoren" und Stephen nicht.

#### <a name="requiring-authorization"></a>Die notwendige Autorisierung

Sie können anfordern, dass einen Benutzer authentifiziert werden, bevor der Benutzer eine Controlleraktion aufgerufen wird, durch die Aktion der [Authorize]-Attribut hinzugefügt. Wenden Sie das [Authorize]-Attribut, um eine einzelne Controlleraktion, oder Sie können dieses Attribut auf eine gesamte Controllerklasse anwenden.

Der Controller im Codebeispiel 1 gibt z. B. eine Aktion, die mit dem Namen CompanySecrets(). Da diese Aktion mit dem [Authorize]-Attribut ergänzt wird, wird diese Aktion kann nicht aufgerufen werden, wenn ein Benutzer authentifiziert wurde.

**1 – Controllers\HomeController.vb auflisten**

[!code-vb[Main](authenticating-users-with-forms-authentication-vb/samples/sample1.vb)]

Wenn Sie die Aktion CompanySecrets() durch Eingabe der URL/Home/CompanySecrets in der Adressleiste des Browsers, aufrufen und Sie nicht authentifizierten Benutzers sind, dann Sie anzuzeigende Anmeldung automatisch umgeleitet werden (siehe Abbildung 5).

**Abbildung 5 – die Login-Sicht**

![clip_image010 [4]](authenticating-users-with-forms-authentication-vb/_static/image5.jpg)

Die Sicht für die Anmeldung können Ihren Benutzernamen und Ihr Kennwort eingeben. Wenn Sie kein registrierter Benutzer klicken Sie auf die **registrieren** Link zum Navigieren zu dem Register anzeigen (siehe Abbildung 6). Die Register-Ansicht können Sie um ein neues Benutzerkonto zu erstellen.

**Abbildung 6 – die Register-Ansicht**

![clip_image012](authenticating-users-with-forms-authentication-vb/_static/image6.jpg)

Nachdem Sie sich erfolgreich anzumelden, sehen Sie die CompanySecrets anzeigen (siehe Abbildung 7). Standardmäßig werden Sie weiterhin angemeldet sein, bis Sie das Browserfenster schließen.

**Abbildung 7 – die CompanySecrets-Sicht**

![clip_image014](authenticating-users-with-forms-authentication-vb/_static/image7.jpg)

#### <a name="authorizing-by-user-name-or-user-role"></a>Autorisieren von Benutzernamen oder eine Benutzerrolle

Sie können das Attribut [Authorize] verwenden, den Zugriff auf eine Controlleraktion zu einem bestimmten Benutzer oder eine bestimmte Gruppe von Benutzerrollen einschränken. Die geänderte Home-Controller in 2 auflisten enthält beispielsweise zwei neue Aktionen, die mit dem Namen StephenSecrets() und AdministratorSecrets().

**Auflisten von 2 – Controllers\HomeController.vb**

[!code-vb[Main](authenticating-users-with-forms-authentication-vb/samples/sample2.vb)]

Nur ein Benutzer mit dem Benutzernamen Stephen kann die StephenSecrets() Aktion aufrufen. Alle anderen Benutzer abrufen anzuzeigende Anmeldung umgeleitet. Die Benutzer-Eigenschaft akzeptiert eine durch Trennzeichen getrennte Liste der Namen von Benutzerkonten.

Nur Benutzer in der "Administratoren" können die AdministratorSecrets() Aktion aufrufen. Da Sally ein Mitglied der Gruppe "Administratoren" ist, können sie z. B. die AdministratorSecrets() Aktion aufrufen. Alle anderen Benutzer abrufen anzuzeigende Anmeldung umgeleitet. Die Rollen-Eigenschaft akzeptiert eine durch Trennzeichen getrennte Liste von Rollennamen.

#### <a name="configuring-authentication"></a>Konfigurieren der Authentifizierung

An diesem Punkt vielleicht Sie, in denen das Konto und die entsprechende Benutzerinformationen gespeichert werden. Standardmäßig befindet sich die Informationen in einer benannten ASPNETDB.mdf befindet sich in Ihrer MVC-Anwendung-App (RANU) SQL Express-Datenbank\_Datenordner. Diese Datenbank wird automatisch von ASP.NET-Framework generiert, wenn Sie beginnen, mit der Mitgliedschaft.

Um die Datenbank ASPNETDB.mdf im Projektmappen-Explorer-Fenster angezeigt wird, müssen Sie zunächst wählen Sie die Menüoption Projekt, alle Dateien anzeigen.

Verwenden die standardmäßige SQL Express-Datenbank ist problemlos, beim Entwickeln einer Anwendung. In den meisten Fällen wird nicht jedoch Sie ASPNETDB.mdf Standarddatenbank für eine produktionsanwendung verwenden möchten. In diesem Fall können Sie ändern, in denen Benutzerkontoinformationen gespeichert ist, indem Sie die folgenden beiden Schritte abschließen:

1. Fügen Sie die Datenbankobjekte Anwendungsdienste an der Produktionsdatenbank - ändern Sie Ihre Anwendung-Verbindungszeichenfolge, um auf die Produktionsdatenbank zu verweisen

Der erste Schritt besteht, alle erforderlichen Datenbankobjekte (Tabellen und gespeicherte Prozeduren) hinzugefügt werden, an der Produktionsdatenbank. Die einfachste Möglichkeit, diese Objekte in eine neue Datenbank hinzufügen, die ASP.NET SQL Server-Setup-Assistenten nutzen ist (siehe Abbildung 8). Sie können dieses Tool starten, öffnen die Visual Studio 2008-Eingabeaufforderung aus der Programmgruppe Microsoft Visual Studio 2008 und den folgenden Befehl von der Befehlszeile aus ausführen:

ASPNET\_Regsql

**Abbildung 8 – die ASP.NET SQL Server-Setup-Assistenten**

![clip_image016](authenticating-users-with-forms-authentication-vb/_static/image8.jpg)

Die ASP.NET SQL Server-Setup-Assistenten können Sie wählen Sie eine SQL Server-Datenbank in Ihrem Netzwerk und installieren Sie alle Datenbankobjekte, die ASP.NET-Anwendungsdiensten erforderlich. Der Datenbankserver ist nicht erforderlich, auf dem lokalen Computer gesucht werden soll.

> [!NOTE]
> Wenn Sie nicht die ASP.NET SQL Server-Setup-Assistenten verwenden möchten, können Sie SQL-Skripts suchen, für die Datenbankobjekte der Anwendung Dienste in den folgenden Ordner hinzufügen:


> C:\WINDOWS\Microsoft.NET\Framework\v2.0.50727


Nachdem Sie die erforderlichen Datenbankobjekte erstellt haben, müssen Sie so ändern Sie die datenbankverbindung, die von der MVC-Anwendung verwendet. Ändern Sie die ApplicationServices-Verbindungszeichenfolge in der Web-Konfigurationsdatei (web.config), sodass er in der Produktionsdatenbank zeigt. Beispielsweise verweist die geänderte Verbindung im Codebeispiel 3 mit einer Datenbank mit dem Namen MyProductionDB (die ursprüngliche ApplicationServices-Verbindungszeichenfolge wurde auskommentiert).

**Auflisten von 3 – "Web.config"**

[!code-xml[Main](authenticating-users-with-forms-authentication-vb/samples/sample3.xml)]

#### <a name="configuring-database-permissions"></a>Konfigurieren von Datenbankberechtigungen

Wenn Sie die integrierte Sicherheit verwenden, für die Verbindung mit Ihrer Datenbank müssen Sie die richtigen Windows-Benutzerkonto als eine Anmeldung mit Ihrer Datenbank hinzufügen. Das richtige Konto hängt davon ab, ob Sie den ASP.NET Development Server oder die Internetinformationsdienste (IIS) als Webserver verwenden. Das ordnungsgemäße Benutzerkonto hängt außerdem auf dem neuesten Stand.

Bei Verwendung von ASP.NET Development Server (der Standardwebserver von Visual Studio verwendet) führt die Anwendung im Kontext Ihres Windows-Benutzerkontos. In diesem Fall müssen Sie Ihr Windows-Benutzerkonto als eine Datenbank-Server-Anmeldung hinzufügen.

Alternativ können Sie bei Verwendung von Internetinformationsdienste (IIS) müssen Sie das ASPNET-Konto oder das Konto NT-Autorität/Netzwerkdienst als eine Datenbank-Server-Anmeldung hinzufügen. Wenn Sie Windows XP verwenden Sie das ASPNET-Konto als Anmeldung mit Ihrer Datenbank hinzufügen. Bei Verwendung von einem neueren Betriebssystem z. B. Windows Vista oder Windows Server 2008, fügen Sie das NT AUTHORITY/NETZWERKDIENST-Konto mit dem Datenbank-Anmeldenamen an.

Sie können ein neues Benutzerkonto mit Ihrer Datenbank hinzufügen, indem Sie mit Microsoft SQL Server Management Studio (siehe Abbildung 9).

**Abbildung 9 – Erstellen einer neuen Microsoft SQL Server-Anmeldung**

![clip_image018](authenticating-users-with-forms-authentication-vb/_static/image9.jpg)

Nachdem Sie die erforderlichen Anmeldung zu erstellen, müssen Sie die Anmeldung einem Datenbankbenutzer mit den richtigen Datenbankrollen zuzuordnen. Doppelklicken Sie auf die Anmeldung, und wählen Sie die Registerkarte Benutzer zuordnen. Wählen Sie eine oder mehrere Rollen der Anwendung-Dienste. Angenommen, um Benutzer zu authentifizieren, müssen Sie das Aspnet aktivieren\_Mitgliedschaft\_BasicAccess-Datenbankrolle. Um neue Benutzer zu erstellen, müssen Sie das Aspnet aktivieren\_Mitgliedschaft\_FullAccess Datenbankrolle (siehe Abbildung 10).

**Abbildung 10 – Hinzufügen von Datenbankrollen Anwendungsdienste**

![clip_image020](authenticating-users-with-forms-authentication-vb/_static/image10.jpg)

#### <a name="summary"></a>Zusammenfassung

In diesem Lernprogramm haben Sie gelernt, wie Sie die Formularauthentifizierung verwenden, beim Erstellen einer ASP.NET MVC-Anwendung. Zunächst haben Sie gelernt, wie Sie neue Benutzer und Rollen erstellen, indem Sie nutzen die Websiteverwaltungs-Tool. Als Nächstes haben Sie gelernt, wie Sie das Attribut [Authorize] zu verwenden, um zu verhindern, dass nicht autorisierte Benutzer Controlleraktionen aufrufen. Schließlich haben Sie gelernt, wie zum Konfigurieren Ihrer MVC-Anwendung zum Speichern von Rolleninformationen zu Benutzern und in einer Produktionsdatenbank.

>[!div class="step-by-step"]
[Zurück](preventing-javascript-injection-attacks-cs.md)
[Weiter](authenticating-users-with-windows-authentication-vb.md)
