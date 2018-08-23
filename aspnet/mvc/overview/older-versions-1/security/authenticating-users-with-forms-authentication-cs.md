---
uid: mvc/overview/older-versions-1/security/authenticating-users-with-forms-authentication-cs
title: Authentifizieren von Benutzern bei der Formularauthentifizierung (c#) | Microsoft-Dokumentation
author: microsoft
description: Erfahren Sie, wie Sie mit dem [Authorize]-Attribut mit einem Kennwort geschützt bestimmte Seiten in der MVC-Anwendung. Erfahren Sie, wie Sie mit der Web Site Administration zu...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: 239fd3ca-5630-4b8d-bc4b-2f906b1d3504
msc.legacyurl: /mvc/overview/older-versions-1/security/authenticating-users-with-forms-authentication-cs
msc.type: authoredcontent
ms.openlocfilehash: d69ae977b3e6a323d1dff1443f09ac40e8f9a449
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836458"
---
<a name="authenticating-users-with-forms-authentication-c"></a>Authentifizieren von Benutzern bei der Formularauthentifizierung (c#)
====================
durch [Microsoft](https://github.com/microsoft)

> Erfahren Sie, wie Sie mit dem [Authorize]-Attribut mit einem Kennwort geschützt bestimmte Seiten in der MVC-Anwendung. Erfahren Sie, wie Sie mit der Web Site Administration Tool zum Erstellen und Verwalten von Benutzern und Rollen. Außerdem erfahren Sie, wie so konfigurieren, in dem Konto und die entsprechende Benutzerinformationen gespeichert ist.


Das Ziel in diesem Tutorial wird beschrieben, wie Sie Formulare verwenden können Kennwort-Authentifizierung schützen, die Ansichten in ASP.NET MVC-Anwendungen. Erfahren Sie, wie das Websiteverwaltungs-Tool verwenden, um Benutzer und Rollen zu erstellen. Sie erfahren außerdem, wie Sie verhindern, dass nicht autorisierte Benutzer Controlleraktionen aufgerufen. Schließlich erfahren Sie, wie Sie konfigurieren, in dem Benutzernamen und Kennwörter gespeichert werden.

#### <a name="using-the-web-site-administration-tool"></a>Verwenden das Websiteverwaltungs-Tool

Bevor wir nichts weiter tun, sollten wir zunächst erstellen einige Benutzer und Rollen. Die einfachste Möglichkeit zum Erstellen neuer Benutzer und Rollen ist der Visual Studio 2008 Websiteverwaltungs-Tool nutzen. Sie können dieses Tool starten, indem Sie durch Auswählen der Menüoption **-Projekt, ASP.NET-Konfiguration**. Alternativ können Sie die Web Site Administration Tool starten, indem Sie auf das Symbol "(ein wenig furchterregend)", der der Hammer, die auf der ganzen Welt, die im oberen Bereich des Projektmappen-Explorer-Fensters angezeigt wird (siehe Abbildung 1).

**Abbildung 1 – starten das Websiteverwaltungs-Tool**

![clip_image002](authenticating-users-with-forms-authentication-cs/_static/image1.jpg)

In der Web Site Administration Tool erstellen Sie neue Benutzer und Rollen durch Auswählen der Registerkarte "Sicherheit". Klicken Sie auf die **Benutzer erstellen** Link zum Erstellen eines neuen Benutzers mit dem Namen Stephen (siehe Abbildung 2). Erhält der Benutzer Stephen das gewünschte Kennwort ein (z. B. *Geheimnis*).

**Abbildung 2 – Erstellen eines neuen Benutzers**

![clip_image004](authenticating-users-with-forms-authentication-cs/_static/image2.jpg)

Sie erstellen neue Rollen erste Aktivieren von Rollen und eine oder mehrere Rollen definieren. Rollen zu aktivieren, indem Sie auf die **Rollen aktivieren** Link. Als Nächstes erstellen Sie eine Rolle, die mit dem Namen *Administratoren* durch Klicken auf die **erstellen oder Verwalten von Rollen** verknüpfen (siehe Abbildung 3).

**Abbildung 3 – Erstellen einer neuen Rolle**

![clip_image006](authenticating-users-with-forms-authentication-cs/_static/image3.jpg)

Abschließend erstellen Sie einen neuen Benutzer, die mit dem Namen Sally und die Rolle "Administratoren" Sally zuordnen, indem Sie auf den Link "Benutzer erstellen", und Administratoren auswählen, wenn Sally erstellen (siehe Abbildung 4).

**Abbildung 4 – Hinzufügen eines Benutzers zu einer Rolle**

![clip_image008](authenticating-users-with-forms-authentication-cs/_static/image4.jpg)

Wenn alle gesagt und getan ist, sollten Sie zwei neue Benutzer, die mit dem Namen Stephen und Sally verfügen. Sie sollten auch eine neue Rolle mit dem Namen Administrators verfügen. Sally ist ein Mitglied der Rolle "Administratoren" und Stephen nicht.

#### <a name="requiring-authorization"></a>Eine Autorisierung erfordert

Sie können anfordern, dass einen Benutzer authentifiziert werden, bevor der Benutzer eine Controlleraktion aufgerufen, durch die Aktion, die [Authorize]-Attribut hinzugefügt. Sie können die [Authorize]-Attribut auf eine einzelne Controlleraktion anwenden, oder Sie können dieses Attribut anwenden, um eine gesamte Controllerklasse.

Der Controller in Codebeispiel 1 stellt beispielsweise eine Aktion, die mit dem Namen CompanySecrets(). Da diese Aktion mit dem [Authorize]-Attribut ergänzt wird, wird diese Aktion kann nicht aufgerufen werden, wenn ein Benutzer authentifiziert wurde.

**Codebeispiel 1 – Controllers\HomeController. cs**

[!code-csharp[Main](authenticating-users-with-forms-authentication-cs/samples/sample1.cs)]

Wenn Sie CompanySecrets() Aktion aufrufen, indem Sie die URL/Home/CompanySecrets in der Adressleiste des Browsers eingeben und Sie kein authentifizierter Benutzer sind, werden Sie umgeleitet an die Ansicht für die Anmeldung automatisch (siehe Abbildung 5).

**Abbildung 5 – die Anmeldeansicht**

![clip_image010](authenticating-users-with-forms-authentication-cs/_static/image5.jpg)

Sie können die Anmeldeansicht verwenden, Ihren Benutzernamen und Kennwort eingeben. Wenn Sie sind sich nicht um einen registrierten Benutzer, und klicken Sie auf die **registrieren** Link zu der Registrierung navigieren anzeigen (siehe Abbildung 6). Sie können die Register-Ansicht verwenden, um ein neues Benutzerkonto zu erstellen.

**Abbildung 6 – die Register-Ansicht**

![clip_image012](authenticating-users-with-forms-authentication-cs/_static/image6.jpg)

Nachdem Sie sich erfolgreich angemeldet haben, sehen Sie die CompanySecrets anzeigen (siehe Abbildung 7). Standardmäßig werden Sie weiterhin angemeldet sein, bis Sie Ihr Browserfenster schließen.

**Abbildung 7 – die CompanySecrets-Ansicht**

![clip_image014](authenticating-users-with-forms-authentication-cs/_static/image7.jpg)

#### <a name="authorizing-by-user-name-or-user-role"></a>Autorisieren von Benutzer- oder Benutzerrolle

Sie können die [Authorize]-Attribut verwenden, um den Zugriff auf eine Controlleraktion auf eine bestimmte Gruppe von Benutzern oder einen bestimmten Satz von Benutzerrollen einschränken. Die geänderte Home-Controller im Codebeispiel 2 enthält beispielsweise zwei neue Aktionen, die mit dem Namen StephenSecrets() und AdministratorSecrets().

**Codebeispiel 2 – Controllers\HomeController. cs**

[!code-csharp[Main](authenticating-users-with-forms-authentication-cs/samples/sample2.cs)]

Nur ein Benutzer mit dem Benutzernamen Stephen kann die StephenSecrets() Aktion aufrufen. Alle anderen Benutzer werden auf die Ansicht für die Anmeldung umgeleitet. Die Users-Eigenschaft akzeptiert eine durch Trennzeichen getrennte Liste von Benutzerkontonamen.

Nur Benutzer in der Rolle "Administratoren" können die AdministratorSecrets() Aktion aufrufen. Da Sally Mitglied der Gruppe "Administratoren" ist, können sie z. B. die AdministratorSecrets() Aktion aufrufen. Alle anderen Benutzer werden auf die Ansicht für die Anmeldung umgeleitet. Die Roles-Eigenschaft akzeptiert eine durch Trennzeichen getrennte Liste von Rollennamen.

#### <a name="configuring-authentication"></a>Konfigurieren der Authentifizierung

An diesem Punkt können Sie Fragen sich, in denen die Benutzer-Konto und die entsprechende Informationen gespeichert wird. Standardmäßig befindet sich die Informationen in einer benannten ASPNETDB.mdf befindet sich in Ihrer Anwendung MVC-App (ranu, Run) SQL Express-Datenbank\_Datenordner. Diese Datenbank wird automatisch von ASP.NET-Framework generiert, wenn Sie beginnen, mit der Mitgliedschaft.

Um die ASPNETDB.mdf-Datenbank im Projektmappen-Explorer-Fenster anzuzeigen, müssen Sie zuerst auf die Menüoption ""-Projekt, alle Dateien anzeigen.

Unter Verwendung der standardmäßigen SQL Express-Datenbank ist in Ordnung, wenn eine Anwendung entwickeln. In den meisten Fällen wird nicht jedoch, Sie die standardmäßige ASPNETDB.mdf-Datenbank für eine produktionsanwendung verwenden möchten. In diesem Fall können Sie die Benutzerkontoinformationen Speicherort wird mithilfe der folgenden zwei Schritte ändern:

1. Fügen Sie die Anwendungsdienste Datenbankobjekte für die Produktionsdatenbank: Ändern der Verbindungszeichenfolge der Anwendung, um auf die Produktionsdatenbank zu verweisen

Im ersten Schritt werden alle erforderlichen Datenbankobjekte (Tabellen und gespeicherte Prozeduren) hinzufügen, die Produktionsdatenbank. Die einfachste Möglichkeit zum Hinzufügen dieser Objekte in eine neue Datenbank wird die ASP.NET SQL Server-Setup-Assistenten nutzen (siehe Abbildung 8). Sie können dieses Tool starten, öffnen die Visual Studio 2008-Eingabeaufforderung aus der Programmgruppe Microsoft Visual Studio 2008 und den folgenden Befehl an der Eingabeaufforderung ausführen:

aspnet\_regsql

**Abbildung 8 – die ASP.NET SQL Server-Setup-Assistenten**

![clip_image016](authenticating-users-with-forms-authentication-cs/_static/image8.jpg)

Die ASP.NET SQL Server-Setup-Assistenten können Sie wählen Sie eine SQL Server-Datenbank in Ihrem Netzwerk, und installieren Sie alle Datenbankobjekte, die durch die Anwendungsdienste für ASP.NET erforderlich. Der Datenbankserver ist nicht erforderlich, auf dem lokalen Computer befinden.

> [!NOTE] 
> 
> Wenn Sie nicht die ASP.NET SQL Server-Setup-Assistenten verwenden möchten, finden Sie bei SQL-Skripts zum Hinzufügen von Application Services Datenbankobjekte im folgenden Ordner:
> 
> > C:\Windows\Microsoft.NET\Framework\v2.0.50727


Nachdem Sie die erforderlichen Datenbankobjekte erstellen, müssen Sie Ihre MVC-Anwendung ein, die Verbindung mit der Datenbank zu ändern. Ändern Sie die ApplicationServices-Verbindungszeichenfolge in Ihrer Web-Konfigurationsdatei (web.config), damit er in der Produktionsdatenbank verweist. Beispielsweise verweist die geänderte Verbindung in Programmausdruck 3 mit einer Datenbank mit dem Namen MyProductionDB (die ursprüngliche ApplicationServices-Verbindungszeichenfolge wurde auskommentiert wurde).

**Codebeispiel 3 – Datei "Web.config"**

[!code-xml[Main](authenticating-users-with-forms-authentication-cs/samples/sample3.xml)]

#### <a name="configuring-database-permissions"></a>Konfigurieren von Berechtigungen für die Datenbank

Wenn Sie die integrierte Sicherheit verwenden, um die Verbindung mit der Datenbank müssen Sie die richtigen Windows-Benutzerkonto als Anmeldekonto mit Ihrer Datenbank hinzufügen. Das richtige Konto hängt davon ab, ob Sie den ASP.NET Development Server oder der Internet Information Services als Webserver verwenden. Das ordnungsgemäße Benutzerkonto hängt auch von Ihrem Betriebssystem ab.

Bei Verwendung der ASP.NET Development Server (den Standard-Webserver, die von Visual Studio verwendet) wird die Anwendung innerhalb des Kontexts Ihres Windows-Benutzerkontos ausgeführt. In diesem Fall müssen Sie Ihr Windows-Benutzerkonto als eine Datenbank-Server-Anmeldung hinzufügen.

Sie können auch bei Verwendung von Internet Information Services müssen Sie entweder das ASPNET-Konto oder das Konto NT AUTHORITY/NETWORK-Dienst als eine Datenbank-Server-Anmeldung hinzugefügt. Wenn Sie Windows XP verwenden das ASPNET-Konto als Anmeldung zu Ihrer Datenbank hinzuzufügen. Bei Verwendung von einem neueren Betriebssystem wie Windows Vista oder Windows Server 2008, fügen Sie das Konto NT AUTHORITY/NETWORK-Dienst als die datenbankanmeldung hinzu.

Sie können ein neues Benutzerkonto mit Ihrer Datenbank mithilfe von Microsoft SQL Server Management Studio hinzufügen (siehe Abbildung 9).

**Abbildung 9 – Erstellen einer neuen Microsoft SQL Server-Anmeldung**

![clip_image018](authenticating-users-with-forms-authentication-cs/_static/image9.jpg)

Nachdem Sie die erforderliche Anmeldung zu erstellen, müssen Sie die Anmeldung einen Datenbankbenutzer mit den richtigen Datenbankrollen zuzuordnen. Doppelklicken Sie auf die Anmeldung, und wählen Sie die Registerkarte "Benutzerzuordnung". Wählen Sie eine oder mehrere Rollen der Anwendung-Dienste. Beispielsweise, um Benutzer zu authentifizieren, müssen Sie das Aspnet aktivieren\_Mitgliedschaft\_BasicAccess-Datenbankrolle. Um neue Benutzer zu erstellen, müssen Sie das Aspnet aktivieren\_Mitgliedschaft\_FullAccess Datenbankrolle (siehe Abbildung 10).

**Abbildung 10 – Hinzufügen von Rollen für Anwendungsdienste-Datenbank**

![clip_image020](authenticating-users-with-forms-authentication-cs/_static/image10.jpg)

#### <a name="summary"></a>Zusammenfassung

In diesem Tutorial haben Sie gelernt, wie die Formularauthentifizierung verwenden, beim Erstellen einer ASP.NET MVC-Anwendung. Zuerst, wie Sie neue Benutzer und Rollen erstellen, durch das Websiteverwaltungs-Tool nutzen. Als Nächstes, wie Sie die [Authorize]-Attribut zu verwenden, um zu verhindern, dass nicht autorisierte Benutzer Controlleraktionen aufgerufen. Schließlich haben Sie Ihre MVC-Anwendung zum Speichern von Rolleninformationen zu Benutzern und in einer Produktionsdatenbank zu konfigurieren.

> [!div class="step-by-step"]
> [Nächste](authenticating-users-with-windows-authentication-cs.md)
