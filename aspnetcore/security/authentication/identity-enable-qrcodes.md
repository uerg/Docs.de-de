---
title: Aktivieren Sie qr-Code-Generierung für Authentifikator-apps in ASP.NET Core
author: rick-anderson
description: Ermitteln Sie zum Aktivieren der Generierung von QR-Code für Authentifikator-apps, die mit ASP.NET Core zweistufige Authentifizierung arbeiten.
manager: wpickett
ms.author: riande
ms.date: 09/24/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/identity-enable-qrcodes
ms.openlocfilehash: c61918d42b407b01484b67d740edc7a682c3a4b0
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/22/2018
---
# <a name="enable-qr-code-generation-for-authenticator-apps-in-aspnet-core"></a>Aktivieren Sie qr-Code-Generierung für Authentifikator-apps in ASP.NET Core

Hinweis: Dieses Thema bezieht sich auf ASP.NET Core 2.x

Im Lieferumfang von ASP.NET Core ist Unterstützung für Anwendungen, für die einzelnen Authentifizierung Authentifikator. Zwei Faktor-Authentifizierung (2FA) Authentifikator-apps, mit der eine zeitbasierte zum einmaligen Kennwort Algorithmus (TOTP), sind die empfohlene Vorgehensweise für 2FA Branche. 2FA TOTP mit SMS 2FA bevorzugt wird. Eine Authentifikator-app enthält einen 6 bis 8 Ziffern-Code, der die Benutzer nach der Bestätigung ihres Benutzernamens und Kennworts eingeben müssen. In der Regel wird eine Authentifikator-app auf einem Smartphone installiert.

ASP.NET Core Web-app-Vorlagen unterstützen Authentifikatoren, aber stellen keine Unterstützung für QRCode Generation. QRCode Generatoren vereinfachen die Einrichtung des 2FA. Dieses Dokument führt Sie durch Hinzufügen von [QR-Code](https://wikipedia.org/wiki/QR_code) Generation auf der Konfigurationsseite 2FA.

## <a name="adding-qr-codes-to-the-2fa-configuration-page"></a>Hinzufügen von QR-Codes auf der Konfigurationsseite 2FA

Verwenden Sie diese Anweisungen *qrcode.js* aus der https://davidshimjs.github.io/qrcodejs/ Repository.

* Herunterladen der [qrcode.js Javascript-Bibliothek](https://davidshimjs.github.io/qrcodejs/) auf die `wwwroot\lib` Ordner des Projekts.

* In *Pages\Account\Manage\EnableAuthenticator.cshtml* (Razor-Seiten) oder *Views\Manage\EnableAuthenticator.cshtml* (MVC), suchen Sie die `Scripts` Abschnitt am Ende der Datei:

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")
}
```

* Update der `Scripts` Abschnitt fügen einen Verweis auf die `qrcodejs` Bibliothek, die Sie hinzugefügt haben und ein Aufruf zum Generieren des QR-Codes. Es sollte wie folgt aussehen:

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")

    <script type="text/javascript" src="~/lib/qrcode.js"></script>
    <script type="text/javascript">
        new QRCode(document.getElementById("qrCode"),
            {
                text: "@Html.Raw(Model.AuthenticatorUri)",
                width: 150,
                height: 150
            });
    </script>
}
```

* Löschen Sie den Absatz Sie mit den vorliegenden Anleitungen verknüpft.

Ausführen der app, und stellen Sie sicher, dass Sie den QR-Code Scannen und überprüfen den Code, den der Authentifikator beweist können.

## <a name="change-the-site-name-in-the-qr-code"></a>Ändern Sie den Namen der Website in den QR-Code

Der Websitename in den QR-Code stammt aus den Namen des Projekts, die Sie auswählen, wenn das Projekt zunächst erstellen. Können Sie Sie ändern, indem Sie die Suche nach der `GenerateQrCodeUri(string email, string unformattedKey)` Methode in der *Pages\Account\Manage\EnableAuthenticator.cshtml.cs* (Razor-Seiten)-Datei oder das *Controllers\ManageController.cs* (MVC)-Datei. 

Der Standard-Code aus der Vorlage sieht wie folgt aus:

```c#
private string GenerateQrCodeUri(string email, string unformattedKey)
{
    return string.Format(
        AuthenicatorUriFormat,
        _urlEncoder.Encode("Razor Pages"),
        _urlEncoder.Encode(email),
        unformattedKey);
}
```

Der zweite Parameter im Aufruf von `string.Format` der Standortname, der den Namen Ihrer Projektmappe entnommen wird. Kann auf einen beliebigen Wert geändert werden, aber es muss immer ein URL-codiert sein.

## <a name="using-a-different-qr-code-library"></a>Verwenden einer anderen QR-Code-Bibliothek

Sie können den QR-Code-Bibliothek mit Ihrem bevorzugten Bibliothek ersetzen. Der HTML-Code enthält eine `qrCode` Element in die Sie einen QR-Code, indem Sie geeignete Mechanismus platzieren können Ihre Bibliothek enthält.

Die korrekt formatierte URL für den QR-Code finden Sie in der:

* `AuthenticatorUri` die Eigenschaft des Modells.
* `data-url` die Eigenschaft in der `qrCodeData` Element. 

## <a name="totp-client-and-server-time-skew"></a>TOTP Client- und Zeitversatz

TOTP Authentifizierung hängt sowohl die Server-als auch der Authentifikator Gerät müssen die genaue Uhrzeit aus. Token werden nur für 30 Sekunden dauern. Wenn die TOTP 2FA Anmeldungen fehlschlägt, überprüfen Sie, ob die Serverzeit genau und vorzugsweise eine genaue NTP-Dienst synchronisiert.
