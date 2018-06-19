---
title: Cloud-Authentifizierung in Web-APIs mit Azure Active Directory B2C in ASP.NET Core
author: camsoper
description: Erfahren Sie, wie Azure Active Directory B2C-Authentifizierung mit Web-API von ASP.NET Core einrichten. Testen Sie die authentifizierte Web-API mit Postman.
ms.author: casoper
manager: wpickett
ms.date: 01/25/2018
ms.topic: tutorial
ms.technology: aspnet
ms.prod: asp.net-core
ms.custom: mvc
uid: security/authentication/azure-ad-b2c-webapi
ms.openlocfilehash: 621290f7e303f9157577b5c1b32646b750ed5159
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30897803"
---
# <a name="cloud-authentication-in-web-apis-with-azure-active-directory-b2c-in-aspnet-core"></a>Cloud-Authentifizierung in Web-APIs mit Azure Active Directory B2C in ASP.NET Core

Von [Cam Soper](https://twitter.com/camsoper)

[Azure Active Directory B2C](/azure/active-directory-b2c/active-directory-b2c-overview) (Azure AD B2C) ist eine Cloud Identity Management-Lösung für Web- und mobilen apps. Der Dienst ermöglicht die Authentifizierung bei apps, die in der Cloud und lokal gehostet. Authentifizierungstypen einzelkonten, soziale Netzwerke-Konten gehören und verbundene Unternehmen-Konten. Darüber hinaus können Azure AD B2C Multi-Factor Authentication mit minimaler Konfiguration bereitstellen.

> [!TIP]
> Azure Active Directory (Azure AD) und Azure AD B2C sind separate Produktangeboten. Azure AD-Mandanten repräsentiert eine Organisation ein Azure AD B2C-Mandanten eine Auflistung von Identitäten, die mit den Anwendungen der vertrauenden Seite verwendet werden. Weitere Informationen finden Sie unter [Azure AD B2C: häufig gestellte Fragen (FAQ)](/azure/active-directory-b2c/active-directory-b2c-faqs).

Da Web-APIs verfügen über keine Benutzeroberfläche, können sie nicht zum Umleiten des Benutzers an einem sicheren Tokendienst wie Azure AD B2C. Stattdessen wird die API ein trägertoken, das über die aufrufende Anwendung übergeben werden, die bereits den Benutzer mit Azure AD B2C authentifiziert hat. Die API überprüft dann das Token ohne direkten Benutzereingriff.

In diesem Lernprogramm erfahren Sie, wie Sie:

> [!div class="checklist"]
> * Erstellen eines Azure Active Directory B2C-Mandanten.
> * Registrieren einer Webs-API in Azure AD B2C an.
> * Verwenden Sie Visual Studio zum Erstellen einer Web-API, die für die Verwendung des Azure AD B2C-Mandanten für die Authentifizierung konfiguriert.
> * Konfigurieren von Richtlinien, die Steuerung des Verhaltens des Azure AD B2C-Mandanten.
> * Verwendung Postman, eine Web-app zu simulieren, die ein Anmeldedialogfeld präsentiert ein Token abgerufen und verwendet, um eine Anforderung für die Web-API zu versehen.

## <a name="prerequisites"></a>Erforderliche Komponenten

Im folgenden sind für diese exemplarische Vorgehensweise erforderlich:

* [Microsoft Azure-Abonnement](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
* [Visual Studio-2017](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs) (beliebige Edition)
* [Postman](https://www.getpostman.com/postman)

## <a name="create-the-azure-active-directory-b2c-tenant"></a>Erstellen der Azure Active Directory B2C-Mandanten

Erstellen eines Azure AD B2C-Mandanten [in der Dokumentation beschriebenen](/azure/active-directory-b2c/active-directory-b2c-get-started). Wenn Sie aufgefordert werden, ist ein Azure-Abonnement Zuordnen des Mandanten optional für dieses Lernprogramm.

## <a name="configure-a-sign-up-or-sign-in-policy"></a>Konfigurieren Sie eine Richtlinie registrieren oder anmelden

Anhand der Schritte in der Azure AD B2C Dokumentation [erstellen Sie eine Richtlinie registrieren oder anmelden](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-sign-up-or-sign-in-policy). Benennen Sie die Richtlinie **SiUpIn**.  Verwenden Sie die Beispielwerte finden Sie in der Dokumentation für **Identitätsanbieter**, **Anmeldung Attribute**, und **Anwendung Ansprüche**. Mithilfe der **jetzt ausführen** , um die Richtlinie zu testen, wie in der Dokumentation beschrieben ist optional.

## <a name="register-the-api-in-azure-ad-b2c"></a>Registrieren Sie die API in Azure AD B2C

In der neu erstellte Azure AD B2C-Mandanten registrieren Ihre API mit [die Schritte in der Dokumentation](/azure/active-directory-b2c/active-directory-b2c-app-registration#register-a-web-api) unter der **registrieren eine Web-API** Abschnitt.

Verwenden Sie die folgenden Werte an:

| Einstellung                       | Wert               | Hinweise                                                                                  |
|-------------------------------|---------------------|----------------------------------------------------------------------------------------|
| **Name**                      | *&lt;Name der API&gt;*  | Geben Sie einen **Namen** für die app, die die app Consumer beschrieben wird.                     |
| **Einschließen von Web-app / web-API** | Ja                 |                                                                                        |
| **Impliziten Datenfluss zulassen**       | Ja                 |                                                                                        |
| **Antwort-URL**                 | `https://localhost` | Antwort-URLs sind die Endpunkte zurück, bei Azure AD B2C alle Token, die Ihre app anfordert. |
| **App-ID-URI**                | *api*               | Der URI muss in eine physische Adresse aufgelöst werden. Es muss eindeutig sein.     |
| **Native Client enthalten**     | Nein                  |                                                                                        |

Nachdem die API registriert ist, wird die Liste der apps und APIs im Mandanten angezeigt. Wählen Sie die API, die gerade registriert wurde. Wählen Sie die **Kopie** Symbol rechts neben der **Anwendungs-ID** Feld in die Zwischenablage kopieren. Wählen Sie **veröffentlicht Bereiche** und überprüfen Sie die Standardeinstellung *User_impersonation* Gültigkeitsbereich vorhanden ist.

## <a name="create-an-aspnet-core-app-in-visual-studio-2017"></a>Erstellen einer ASP.NET Core-app in Visual Studio 2017

Der Visual Studio Web Application-Vorlage kann für die Azure AD B2C-Mandanten für die Authentifizierung verwenden konfiguriert werden.

In Visual Studio:

1. Erstellen Sie eine neue ASP.NET Core-Webanwendung. 
2. Wählen Sie **Web-API** aus der Liste der Vorlagen.
3. Wählen Sie die **Authentifizierung ändern** Schaltfläche.

    ![Authentifizierung-Schaltfläche "ändern"](./azure-ad-b2c-webapi/change-auth-button.png)

4. In der **Authentifizierung ändern** wählen Sie im Dialogfeld **einzelne Benutzerkonten**, und wählen Sie dann **Herstellen einer Verbindung mit einer vorhandenen Speicher des Benutzers in der Cloud** in der Dropdownliste aus. 

    ![Authentifizierungsdialog ändern](./azure-ad-b2c-webapi/change-auth-dialog.png)

5. Füllen Sie das Formular mit den folgenden Werten:

    | Einstellung                       | Wert                                                 |
    |-------------------------------|-------------------------------------------------------|
    | **Domänenname**               | *&lt;der Domänenname Ihres B2C-Mandanten&gt;*          |
    | **Anwendungs-ID**            | *&lt;die Anwendungs-ID aus der Zwischenablage einfügen&gt;* |
    | **Registrieren oder anmelden Richtlinie** | `B2C_1_SiUpIn`                                        |

    Wählen Sie **OK** schließen die **Authentifizierung ändern** Dialogfeld. Wählen Sie **OK** zum Erstellen der Web-app.

Visual Studio erstellt die Web-API mit einem Controller namens *ValuesController.cs* , die hartcodierte Werte für GET-Anforderungen zurückgibt. Die Klasse wird mit ergänzt die [Authorize-Attribut](xref:security/authorization/simple), sodass alle Anforderungen Authentifizierung erforderlich ist.

## <a name="run-the-web-api"></a>Führen Sie die Web-API

Führen Sie die API, in Visual Studio. Visual Studio öffnet einen Browser, auf die API-Stamm-URL verwiesen wird. Beachten Sie die URL in die Adressleiste ein, und lassen Sie die API im Hintergrund ausgeführt.

> [!NOTE]
> Da es kein Domänencontroller für die Stamm-URL definiert ist, zeigt der Browser Fehler 404 (Seite nicht gefunden). Dabei handelt es sich um ein erwartetes Verhalten.

## <a name="use-postman-to-get-a-token-and-test-the-api"></a>Verwenden Sie Postman, um ein Token abrufen und die API testen

[Postman](https://getpostman.com/postman) ist ein Tool zum Testen von web-APIs. Für dieses Lernprogramm simuliert Postman eine Web-app, die die Web-API im Auftrag des Benutzers zugreift.

### <a name="register-postman-as-a-web-app"></a>Registrieren Sie Postman als Web-app

Da Postman eine Web-app, die Token aus dem Azure AD B2C-Mandanten erhalten können simuliert, muss es als Web-app im Mandanten registriert sein. Registrieren Sie Postman mit [die Schritte in der Dokumentation](/azure/active-directory-b2c/active-directory-b2c-app-registration#register-a-web-app) unter der **Registrieren einer WebApp** Abschnitt. Beenden Sie die **erstellen Sie eine Web-app-Clientschlüssel** Abschnitt. Ein geheimen Client ist nicht für dieses Lernprogramm erforderlich. 

Verwenden Sie die folgenden Werte an:

| Einstellung                       | Wert                            | Hinweise                           |
|-------------------------------|----------------------------------|---------------------------------|
| **Name**                      | Postman                          |                                 |
| **Einschließen von Web-app / web-API** | Ja                              |                                 |
| **Impliziten Datenfluss zulassen**       | Ja                              |                                 |
| **Antwort-URL**                 | `https://getpostman.com/postman` |                                 |
| **App-ID-URI**                | *&lt;Leer lassen&gt;*            | Für dieses Lernprogramm erforderlich nicht. |
| **Native Client enthalten**     | Nein                               |                                 |

Die neu registrierten Web-app benötigt eine Zugriffsberechtigung für die Web-API im Auftrag des Benutzers.  

1. Wählen Sie **Postman** in der Liste der apps, und wählen Sie dann **API-Zugriff** im Menü auf der linken Seite.
2. Wählen Sie **+ hinzufügen**.
3. In der **API wählen** Dropdownliste, wählen Sie den Namen der Web-API.
4. In der **wählen Bereiche** Dropdownliste, stellen Sie sicher, alle Bereiche ausgewählt sind.
5. Wählen Sie **Ok**.

Beachten Sie der Postman app Anwendungs-ID, wie es erforderlich ist, um ein trägertoken zu erhalten.

### <a name="create-a-postman-request"></a>Erstellen Sie eine Postman-Anforderung

Starten Sie Postman. Standardmäßig zeigt Postman der **neu erstellen** Dialogfeld beim Starten. Wenn das Dialogfeld nicht angezeigt wird, wählen Sie die **+ neu** Schaltfläche in der oberen linken Ecke.

Aus der **neu erstellen** Dialogfeld:

1. Wählen Sie **anfordern**.

    ![Schaltfläche für Anforderungen](./azure-ad-b2c-webapi/postman-create-new.png)

2. Geben Sie *Werte abrufen* in der **Anforderungsnamen** Feld.
3. Wählen Sie **+ erstellen Auflistung** zum Erstellen einer neuen Auflistung zum Speichern der Anforderungs. Namen für die Sammlung *ASP.NET Core Lernprogramme* und wählen Sie dann auf das Häkchen.

    ![Eine neue Sammlung erstellen](./azure-ad-b2c-webapi/postman-create-collection.png)

4. Wählen Sie die **speichern zu ASP.NET Core Lernprogrammen** Schaltfläche.

### <a name="test-the-web-api-without-authentication"></a>Die Web-API ohne Authentifizierung testen

Um sicherzustellen, dass die Web-API eine Authentifizierung erforderlich ist, stellen Sie zunächst eine Anforderung ohne Authentifizierung.

1. In der **Anforderungs-URL eingeben** Geben Sie die URL für `ValuesController`. Die URL ist identisch mit der im Browser mit **-api/Werte** angefügt. Ein Beispiel wäre `https://localhost:44375/api/values`.
2. Wählen Sie die **senden** Schaltfläche.
3. Beachten Sie der Status der Antwort ist *401 nicht autorisiert*.

    ![nicht autorisierte Antwort 401](./azure-ad-b2c-webapi/postman-401-status.png)

### <a name="obtain-a-bearer-token"></a>Ein trägertoken, das Abrufen

Um eine authentifizierte Anforderung an die Web-API vornehmen zu können, ist ein trägertoken erforderlich. Postman erleichtert es, melden Sie sich an den Azure AD B2C-Mandanten und ein Token zu erhalten.

1. Auf der **Autorisierung** Registerkarte die **Typ** Dropdownliste wählen **OAuth 2.0**. In der **hinzufügen Autorisierungsdaten zu** Dropdownliste wählen **Anforderungsheader**. Wählen Sie **neue Zugriffstoken abrufen**.

    ![Registerkarte "Autorisierung" mit Einstellungen](./azure-ad-b2c-webapi/postman-auth-tab.png)

2. Führen Sie die **Abrufen neuer ZUGRIFFSTOKEN** Dialogfeld wie folgt:


   |                Einstellung                 |                                             Wert                                             |                                                                                                                                    Hinweise                                                                                                                                     |
   |----------------------------------------|-----------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
   |      <strong>Tokenname</strong>       |                                  <em>&lt;Tokenname&gt;</em>                                  |                                                                                                                   Geben Sie einen beschreibenden Namen für das Token aus.                                                                                                                    |
   |      <strong>GRANT-Typ</strong>       |                                           Implizit                                            |                                                                                                                                                                                                                                                                              |
   |     <strong>Rückruf-URL</strong>      |                               `https://getpostman.com/postman`                                |                                                                                                                                                                                                                                                                              |
   |       <strong>Auth-URL</strong>        | `https://login.microsoftonline.com/<tenant domain name>/oauth2/v2.0/authorize?p=B2C_1_SiUpIn` |                                                                                                  Ersetzen Sie <em>&lt;name_der_mandantendomäne&gt;</em> mit Domänennamen des Mandanten.                                                                                                  |
   |       <strong>Client-ID</strong>       |                <em>&lt;Geben Sie das Postman App- <b>Anwendungs-ID</b>&gt;</em>                 |                                                                                                                                                                                                                                                                              |
   |     <strong>Geheimer Clientschlüssel</strong>     |                                 <em>&lt;Leer lassen&gt;</em>                                  |                                                                                                                                                                                                                                                                              |
   |         <strong>Bereich</strong>         |         `https://<tenant domain name>/<api>/user_impersonation openid offline_access`         | Ersetzen Sie <em>&lt;name_der_mandantendomäne&gt;</em> mit Domänennamen des Mandanten. Ersetzen Sie <em>&lt;api&gt;</em> mit dem Namen der Web-API-Projekt. Können Sie auch Anwendung-ID auf. Das Muster für die URL lautet: <em>https://{tenant}.onmicrosoft.com/{app_name_or_id}/{scope-Name}</em>. |
   | <strong>Clientauthentifizierung</strong> |                                Clientanmeldeinformationen im Text senden                                |                                                                                                                                                                                                                                                                              |


3. Wählen Sie die **Token anfordern** Schaltfläche.

4. Postman öffnet ein neues Fenster mit den Azure AD B2C-Mandanten Anmelden im Dialogfeld ". Melden Sie sich mit einem vorhandenen Konto, (wenn die Richtlinien testen erstellt wurde), oder wählen Sie **jetzt registrieren** um ein neues Konto zu erstellen. Die **haben Sie Ihr Kennwort vergessen?** Link wird verwendet, um ein vergessenes Kennwort zurückzusetzen.

5. Nach der Anmeldung erfolgreich können das Fenster wird geschlossen und die **ZUGRIFFSTOKEN verwalten** Dialogfeld wird angezeigt. Scrollen Sie zu den unten, und wählen die **verwenden Token** Schaltfläche.

    ![Wo finden Sie die Schaltfläche "Token verwenden"](./azure-ad-b2c-webapi/postman-access-token.png)

### <a name="test-the-web-api-with-authentication"></a>Testen Sie die Web-API mit Authentifizierung

Wählen Sie die **senden** Schaltfläche, um die Anforderung erneut zu senden. In diesem Fall der Antwortstatus ist *200 OK* und die JSON-Nutzlast wird angezeigt, in der Antwort **Text** Registerkarte.

![Status der Nutzlast und Erfolg](./azure-ad-b2c-webapi/postman-success.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie gelernt, wie die folgenden Aufgaben ausgeführt werden:

> [!div class="checklist"]
> * Erstellen eines Azure Active Directory B2C-Mandanten.
> * Registrieren einer Webs-API in Azure AD B2C an.
> * Verwenden Sie Visual Studio zum Erstellen einer Web-API, die für die Verwendung des Azure AD B2C-Mandanten für die Authentifizierung konfiguriert.
> * Konfigurieren von Richtlinien, die Steuerung des Verhaltens des Azure AD B2C-Mandanten.
> * Verwendung Postman, eine Web-app zu simulieren, die ein Anmeldedialogfeld präsentiert ein Token abgerufen und verwendet, um eine Anforderung für die Web-API zu versehen.

Entwickeln von Learning, um Ihre API fortgesetzt werden:

* [Sichern einer ASP.NET Core Web-app mit Azure AD B2C](xref:security/authentication/azure-ad-b2c).
* [Rufen Sie eine .NET Web-API aus einer .NET Web-app mit Azure AD B2C](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-web-api-dotnet).
* [Anpassen der Benutzeroberfläche von Azure AD B2C](/azure/active-directory-b2c/active-directory-b2c-reference-ui-customization).
* [Konfigurieren von Anforderungen an die Kennwortkomplexität](/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity).
* [Aktivieren von Multi-Factor Authentication](/azure/active-directory-b2c/active-directory-b2c-reference-mfa).
* Konfigurieren Sie zusätzliche Identitätsanbieter, z. B. [Microsoft](/azure/active-directory-b2c/active-directory-b2c-setup-msa-app), [Facebook](/azure/active-directory-b2c/active-directory-b2c-setup-fb-app), [Google](/azure/active-directory-b2c/active-directory-b2c-setup-goog-app), [Amazon](/azure/active-directory-b2c/active-directory-b2c-setup-amzn-app), [Twitter ](/azure/active-directory-b2c/active-directory-b2c-setup-twitter-app), und andere.
* [Verwenden die Azure AD Graph-API](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet) zusätzliche Benutzerinformationen, wie z. B. Gruppenmitgliedschaft, aus dem Azure AD B2C-Mandanten abgerufen.
