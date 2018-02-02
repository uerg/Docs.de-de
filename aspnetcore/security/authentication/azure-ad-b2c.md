---
title: Cloud-Authentifizierung mit Azure Active Directory B2C
author: camsoper
description: Erfahren Sie, wie Azure Active Directory B2C-Authentifizierung mit ASP.NET Core einrichten.
manager: wpickett
ms.date: 01/25/2018
ms.topic: tutorial
ms.technology: aspnet
ms.prod: asp.net-core
ms.custom: mvc
uid: security/authentication/azure-ad-b2c
ms.openlocfilehash: 4815155ad238c31316e00471cf87beb3dd262613
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2018
---
# <a name="cloud-authentication-with-azure-active-directory-b2c"></a>Cloud-Authentifizierung mit Azure Active Directory B2C

Von [Cam Soper](https://twitter.com/camsoper)

[Azure Active Directory B2C](/azure/active-directory-b2c/active-directory-b2c-overview) (Azure AD B2C) ist eine Cloud Identity Management-Lösung für Web- und mobilen apps. Der Dienst ermöglicht die Authentifizierung bei apps, die in der Cloud und lokal gehostet. Mögliche Authentifizierungstypen gehören einzelkonten, soziale Netzwerke-Konten und verbundene Unternehmen-Konten. Darüber hinaus können Azure AD B2C Multi-Factor Authentication mit minimaler Konfiguration bereitstellen.

> [!TIP]
> Azure Active Directory (Azure AD) Azure AD B2C sind separate Produktangeboten. Azure AD-Mandanten repräsentiert eine Organisation ein Azure AD B2C-Mandanten eine Auflistung von Identitäten, die mit den Anwendungen der vertrauenden Seite verwendet werden. Weitere Informationen finden Sie unter [Azure AD B2C: häufig gestellte Fragen (FAQ)](/azure/active-directory-b2c/active-directory-b2c-faqs).

In diesem Lernprogramm erfahren Sie, wie Sie:

> [!div class="checklist"]
> * Erstellen eines Azure Active Directory B2C-Mandanten
> * Registrieren einer app in Azure AD B2C
> * Verwenden von Visual Studio zum Erstellen einer ASP.NET Core Web-app, die so konfiguriert, dass die Azure AD B2C-Mandanten für die Authentifizierung verwenden
> * Konfigurieren von Richtlinien, die Steuerung des Verhaltens des Azure AD B2C-Mandanten

## <a name="prerequisites"></a>Erforderliche Komponenten

Im folgenden sind für diese exemplarische Vorgehensweise erforderlich:

* [Microsoft Azure-Abonnement](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
* [Visual Studio-2017](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs) (beliebige Edition)

## <a name="create-the-azure-active-directory-b2c-tenant"></a>Erstellen der Azure Active Directory B2C-Mandanten

Erstellen eines Azure Active Directory B2C-Mandanten [in der Dokumentation beschriebenen](/azure/active-directory-b2c/active-directory-b2c-get-started). Wenn Sie aufgefordert werden, ist ein Azure-Abonnement Zuordnen des Mandanten optional für dieses Lernprogramm.

## <a name="register-the-app-in-azure-ad-b2c"></a>Registrieren der app in Azure AD B2C

In der neu erstellte Azure AD B2C-Mandanten Registrieren Ihrer app mit [die Schritte in der Dokumentation](/azure/active-directory-b2c/active-directory-b2c-app-registration#register-a-web-app) unter der **Registrieren einer WebApp** Abschnitt. Beenden Sie die **erstellen Sie eine Web-app-Clientschlüssel** Abschnitt. Ein geheimen Client ist nicht für dieses Lernprogramm erforderlich. 

Verwenden Sie die folgenden Werte an:

| Einstellung                       | Wert                     | Hinweise                                                                                                                                                                                              |
|-------------------------------|---------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**                      | *&lt;App-name&gt;*        | Geben Sie einen **Namen** für die app, die die app Consumer beschrieben wird.                                                                                                                                 |
| **Einschließen von Web-app / web-API** | Ja                       |                                                                                                                                                                                                    |
| **Impliziten Datenfluss zulassen**       | Ja                       |                                                                                                                                                                                                    |
| **Antwort-URL**                 | `https://localhost:44300` | Antwort-URLs sind die Endpunkte zurück, bei Azure AD B2C alle Token, die Ihre app anfordert. Visual Studio stellt die Antwort-URL verwenden. Geben Sie vorerst `https://localhost:44300` zum Ausfüllen des Formulars. |
| **App-ID-URI**                | Leer lassen               | Für dieses Lernprogramm erforderlich nicht.                                                                                                                                                                    |
| **Native Client enthalten**     | Nein                        |                                                                                                                                                                                                    |

> [!WARNING]
> Wenn eine Antwort-URL nicht "localhost" Einrichten der berücksichtigen die [Beschränkungen in der Antwort-URL-Liste zulässigen](/azure/active-directory-b2c/active-directory-b2c-app-registration#choosing-a-web-app-or-api-reply-url). 

Nachdem die app registriert wurde, wird die Liste der apps im Mandanten angezeigt. Wählen Sie die app, die gerade registriert wurde. Wählen Sie die **Kopie** Symbol rechts neben der **Anwendungs-ID** Feld in die Zwischenablage kopieren.

Nichts mehr zu diesem Zeitpunkt in der Azure AD B2C-Mandanten konfiguriert werden können, aber lassen Sie das Browserfenster geöffnet. Nach die app ASP.NET Core erstellt wird, entsteht mehr Konfigurationsaufwand.

## <a name="create-an-aspnet-core-app-in-visual-studio-2017"></a>Erstellen einer ASP.NET Core-app in Visual Studio 2017

Der Visual Studio Web Application-Vorlage kann für die Azure AD B2C-Mandanten für die Authentifizierung verwenden konfiguriert werden.

In Visual Studio:

1. Erstellen Sie eine neue ASP.NET Core-Webanwendung. 
2. Wählen Sie **Webanwendung** aus der Liste der Vorlagen.
3. Wählen Sie die **Authentifizierung ändern** Schaltfläche.
    
    ![Authentifizierung-Schaltfläche "ändern"](./azure-ad-b2c/_static/changeauth.png)

4. In der **Authentifizierung ändern** wählen Sie im Dialogfeld **einzelne Benutzerkonten**, und wählen Sie dann **Herstellen einer Verbindung mit einer vorhandenen Speicher des Benutzers in der Cloud** in der Dropdownliste aus. 
    
    ![Authentifizierungsdialog ändern](./azure-ad-b2c/_static/changeauthdialog.png)

5. Füllen Sie das Formular mit den folgenden Werten:
    
    | Einstellung                       | Wert                                                 |
    |-------------------------------|-------------------------------------------------------|
    | **Domänenname**               | *&lt;der Domänenname Ihres B2C-Mandanten&gt;*          |
    | **Anwendungs-ID**            | *&lt;die Anwendungs-ID aus der Zwischenablage einfügen&gt;* |
    | **Rückruf-Pfad**             | *&lt;Verwenden Sie den Standardwert&gt;*                       |
    | **Registrieren oder anmelden Richtlinie** | `B2C_1_SiUpIn`                                        |
    | **Zurücksetzen der Kennwortrichtlinie**     | `B2C_1_SSPR`                                          |
    | **Richtlinie bearbeiten**       | *&lt;leer lassen&gt;*                                 |
    
    Wählen Sie die **Kopie** neben verknüpfen **Antwort-URI** an die Antwort-URI in die Zwischenablage zu kopieren. Wählen Sie **OK** schließen die **Authentifizierung ändern** Dialogfeld. Wählen Sie **OK** zum Erstellen der Web-app.

## <a name="finish-the-b2c-app-registration"></a>Fertig stellen Sie die B2C-app-Registrierung

Zurück zum Browserfenster mit der B2C-app-Eigenschaften noch geöffnet. Ändern Sie die temporäre **-Antwort-URL** früher auf den Wert kopiert aus Visual Studio angegeben. Wählen Sie **speichern** am oberen Rand des Fensters.

> [!TIP]
> Wenn Sie die Antwort-URL kopieren haben nicht, verwenden Sie die SSL-Adresse aus der Registerkarte "Debuggen" in den Projekteigenschaften Web und Anfügen der **CallbackPath** Wert aus der *appsettings.json*.

## <a name="configure-policies"></a>Konfigurieren von Richtlinien

Anhand der Schritte in der Azure AD B2C Dokumentation [erstellen Sie eine Richtlinie registrieren oder anmelden](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-sign-up-or-sign-in-policy), und klicken Sie dann [erstellen Sie eine Richtlinie zum Zurücksetzen des](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-password-reset-policy). Verwenden Sie die Beispielwerte finden Sie in der Dokumentation für **Identitätsanbieter**, **Anmeldung Attribute**, und **Anwendung Ansprüche**. Mithilfe der **jetzt ausführen** Schaltfläche, um die Richtlinien zu testen, wie in der Dokumentation beschrieben ist optional.

> [!WARNING]
> Stellen Sie sicher, Richtliniennamen werden genau wie in der Dokumentation beschrieben, wie diese Richtlinien wurden in die **Authentifizierung ändern** Dialogfeld in Visual Studio. Richtliniennamen können überprüft werden, *appsettings.json*.

## <a name="run-the-app"></a>Ausführen der App

Drücken Sie in Visual Studio **F5** erstellen und Ausführen der app. Nachdem die Web-app gestartet wird, wählen Sie **anmelden**.

![Melden Sie sich die app](./azure-ad-b2c/_static/signin.png)

Der Browser leitet an die Azure AD B2C-Mandanten. Melden Sie sich mit einem vorhandenen Konto, (wenn die Richtlinien testen erstellt wurde), oder wählen Sie **jetzt registrieren** um ein neues Konto zu erstellen. Die **haben Sie Ihr Kennwort vergessen?** Link wird verwendet, um ein vergessenes Kennwort zurückzusetzen.

![Azure AD B2C-Anmeldung](./azure-ad-b2c/_static/b2csts.png)

Nach erfolgreich bei anmelden, leitet den Browser an die Web-app ein.

![Erfolgreich](./azure-ad-b2c/_static/success.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie gelernt, wie die folgenden Aufgaben ausgeführt werden:

> [!div class="checklist"]
> * Erstellen eines Azure Active Directory B2C-Mandanten
> * Registrieren einer app in Azure AD B2C
> * Verwenden Sie Visual Studio zum Erstellen von Core ASP.NET-Webanwendung so konfiguriert, dass die Azure AD B2C-Mandanten für die Authentifizierung verwenden
> * Konfigurieren von Richtlinien, die Steuerung des Verhaltens des Azure AD B2C-Mandanten

Nun, dass die ASP.NET Core-app für die Verwendung von Azure AD B2C für die Authentifizierung konfiguriert ist die [Authorize-Attribut](xref:security/authorization/simple) kann zum Sichern von Ihrer app verwendet werden. Weiterhin Learning, um die Entwicklung Ihrer app:

* [Anpassen der Benutzeroberfläche von Azure AD B2C](/azure/active-directory-b2c/active-directory-b2c-reference-ui-customization).
* [Konfigurieren von Anforderungen an die Kennwortkomplexität](/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity).
* [Aktivieren von Multi-Factor Authentication](/azure/active-directory-b2c/active-directory-b2c-reference-mfa).
* Konfigurieren Sie zusätzliche Identitätsanbieter, z. B. [Microsoft](/azure/active-directory-b2c/active-directory-b2c-setup-msa-app), [Facebook](/azure/active-directory-b2c/active-directory-b2c-setup-fb-app), [Google](/azure/active-directory-b2c/active-directory-b2c-setup-goog-app), [Amazon](/azure/active-directory-b2c/active-directory-b2c-setup-amzn-app), [Twitter ](/azure/active-directory-b2c/active-directory-b2c-setup-twitter-app), und andere.
* [Verwenden die Azure AD Graph-API](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet) zusätzliche Benutzerinformationen, wie z. B. Gruppenmitgliedschaft, aus dem Azure AD B2C-Mandanten abgerufen.
* [Sichern einer ASP.NET Core Web-API mithilfe von Azure AD B2C](xref:security/authentication/azure-ad-b2c-webapi).
* [Rufen Sie eine .NET Web-API aus einer .NET Web-app mit Azure AD B2C](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-web-api-dotnet).
