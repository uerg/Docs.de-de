---
title: Aktivieren der QR-Code-Generierung für Authentifikator-apps in ASP.NET Core TOTP
author: rick-anderson
description: Erfahren Sie, wie QR-codegenerierung für TOTP-Authentifikator-apps zu aktivieren, die mit ASP.NET Core-zwei-Faktor-Authentifizierung verwendet.
ms.author: riande
ms.date: 08/14/2018
uid: security/authentication/identity-enable-qrcodes
ms.openlocfilehash: 4535efdde7340436c6a508848bff86e103df570e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826529"
---
# <a name="enable-qr-code-generation-for-totp-authenticator-apps-in-aspnet-core"></a><span data-ttu-id="b59aa-103">Aktivieren der QR-Code-Generierung für Authentifikator-apps in ASP.NET Core TOTP</span><span class="sxs-lookup"><span data-stu-id="b59aa-103">Enable QR Code generation for TOTP authenticator apps in ASP.NET Core</span></span>

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="b59aa-104">Qr-Codes ist ASP.NET Core 2.0 oder höher erforderlich.</span><span class="sxs-lookup"><span data-stu-id="b59aa-104">QR Codes requires ASP.NET Core 2.0 or later.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="b59aa-105">Im Lieferumfang von ASP.NET Core ist Support für Authenticator-Anwendungen für die einzelne Authentifizierung.</span><span class="sxs-lookup"><span data-stu-id="b59aa-105">ASP.NET Core ships with support for authenticator applications for individual authentication.</span></span> <span data-ttu-id="b59aa-106">Zwei Faktor-Authentifizierung (2FA) Authentifikator-apps, verwenden eine zeitbasierte Einmalkennwort Kennwort Algorithmus (TOTP), sind der empfohlene Ansatz für 2FA Branche.</span><span class="sxs-lookup"><span data-stu-id="b59aa-106">Two factor authentication (2FA) authenticator apps, using a Time-based One-time Password Algorithm (TOTP), are the industry recommended approach for 2FA.</span></span> <span data-ttu-id="b59aa-107">2FA TOTP mit SMS 2FA vorzuziehen ist.</span><span class="sxs-lookup"><span data-stu-id="b59aa-107">2FA using TOTP is preferred to SMS 2FA.</span></span> <span data-ttu-id="b59aa-108">Eine Authentifikator-app bietet einen 6 bis 8 Ziffern-Code, der die Benutzer nach der Bestätigung ihres Benutzernamens und Kennworts eingeben müssen.</span><span class="sxs-lookup"><span data-stu-id="b59aa-108">An authenticator app provides a 6 to 8 digit code which users must enter after confirming their username and password.</span></span> <span data-ttu-id="b59aa-109">In der Regel wird eine Authentifikator-app auf einem Smartphone installiert.</span><span class="sxs-lookup"><span data-stu-id="b59aa-109">Typically an authenticator app is installed on a smart phone.</span></span>

<span data-ttu-id="b59aa-110">Die ASP.NET Core-Web-app-Vorlagen unterstützen von Authentifikatoren, aber Sie bieten keine Unterstützung für QRCode Generation.</span><span class="sxs-lookup"><span data-stu-id="b59aa-110">The ASP.NET Core web app templates support authenticators, but don't provide support for QRCode generation.</span></span> <span data-ttu-id="b59aa-111">QRCode Generatoren vereinfachen die Einrichtung der 2FA.</span><span class="sxs-lookup"><span data-stu-id="b59aa-111">QRCode generators ease the setup of 2FA.</span></span> <span data-ttu-id="b59aa-112">Dieses Dokument führt Sie durch Hinzufügen von [QR-Code](https://wikipedia.org/wiki/QR_code) Generierung auf der Konfigurationsseite 2FA.</span><span class="sxs-lookup"><span data-stu-id="b59aa-112">This document will guide you through adding [QR Code](https://wikipedia.org/wiki/QR_code) generation to the 2FA configuration page.</span></span>

## <a name="adding-qr-codes-to-the-2fa-configuration-page"></a><span data-ttu-id="b59aa-113">Hinzufügen von QR-Codes auf der Konfigurationsseite für 2FA</span><span class="sxs-lookup"><span data-stu-id="b59aa-113">Adding QR Codes to the 2FA configuration page</span></span>

<span data-ttu-id="b59aa-114">Verwenden Sie diese Anweisungen *qrcode.js* aus der https://davidshimjs.github.io/qrcodejs/ Repository.</span><span class="sxs-lookup"><span data-stu-id="b59aa-114">These instructions use *qrcode.js* from the https://davidshimjs.github.io/qrcodejs/ repo.</span></span>

* <span data-ttu-id="b59aa-115">Herunterladen der [qrcode.js Javascript-Bibliothek](https://davidshimjs.github.io/qrcodejs/) auf die `wwwroot\lib` -Ordner des Projekts.</span><span class="sxs-lookup"><span data-stu-id="b59aa-115">Download the [qrcode.js javascript library](https://davidshimjs.github.io/qrcodejs/) to the `wwwroot\lib` folder in your project.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

* <span data-ttu-id="b59aa-116">Befolgen Sie die Anweisungen in [Gerüst Identität](xref:security/authentication/scaffold-identity) generieren */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="b59aa-116">Follow the instructions in [Scaffold Identity](xref:security/authentication/scaffold-identity) to generate */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*.</span></span>
* <span data-ttu-id="b59aa-117">In */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*, suchen Sie die `Scripts` Abschnitt am Ende der Datei:</span><span class="sxs-lookup"><span data-stu-id="b59aa-117">In */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*, locate the `Scripts` section at the end of the file:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

* <span data-ttu-id="b59aa-118">In *Pages/Account/Manage/EnableAuthenticator.cshtml* (Razor Pages) oder *Views/Manage/EnableAuthenticator.cshtml* (MVC), suchen Sie die `Scripts` Abschnitt am Ende der Datei:</span><span class="sxs-lookup"><span data-stu-id="b59aa-118">In *Pages/Account/Manage/EnableAuthenticator.cshtml* (Razor Pages) or *Views/Manage/EnableAuthenticator.cshtml* (MVC), locate the `Scripts` section at the end of the file:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")
}
```

* <span data-ttu-id="b59aa-119">Update der `Scripts` Abschnitt aus, um das Hinzufügen eines Verweises auf die `qrcodejs` Bibliothek, die Sie hinzugefügt haben und ein Aufruf zum Generieren des QR-Codes.</span><span class="sxs-lookup"><span data-stu-id="b59aa-119">Update the `Scripts` section to add a reference to the `qrcodejs` library you added and a call to generate the QR Code.</span></span> <span data-ttu-id="b59aa-120">Es sollte wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="b59aa-120">It should look as follows:</span></span>

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

* <span data-ttu-id="b59aa-121">Löschen Sie den Absatz, der Sie mit diesen Anweisungen verknüpft ist.</span><span class="sxs-lookup"><span data-stu-id="b59aa-121">Delete the paragraph which links you to these instructions.</span></span>

<span data-ttu-id="b59aa-122">Führen Sie die app aus, und stellen Sie sicher, dass Sie können den QR-Code Scannen und überprüfen den Code, den der Authentifikator beweist.</span><span class="sxs-lookup"><span data-stu-id="b59aa-122">Run your app and ensure that you can scan the QR code and validate the code the authenticator proves.</span></span>

## <a name="change-the-site-name-in-the-qr-code"></a><span data-ttu-id="b59aa-123">Ändern Sie den Namen der Website in den QR-Code</span><span class="sxs-lookup"><span data-stu-id="b59aa-123">Change the site name in the QR Code</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="b59aa-124">Der Websitename in den QR-Code stammt aus den Namen des Projekts, die Sie auswählen, wenn das Projekt zuerst erstellen.</span><span class="sxs-lookup"><span data-stu-id="b59aa-124">The site name in the QR Code is taken from the project name you choose when initially creating your project.</span></span> <span data-ttu-id="b59aa-125">Sie können ändern, indem Sie die Suche nach der `GenerateQrCodeUri(string email, string unformattedKey)` -Methode in der die */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="b59aa-125">You can change it by looking for the `GenerateQrCodeUri(string email, string unformattedKey)` method in the */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="b59aa-126">Der Websitename in den QR-Code stammt aus den Namen des Projekts, die Sie auswählen, wenn das Projekt zuerst erstellen.</span><span class="sxs-lookup"><span data-stu-id="b59aa-126">The site name in the QR Code is taken from the project name you choose when initially creating your project.</span></span> <span data-ttu-id="b59aa-127">Können Sie Sie ändern, indem Sie die Suche nach der `GenerateQrCodeUri(string email, string unformattedKey)` -Methode in der die *Pages/Account/Manage/EnableAuthenticator.cshtml.cs* (Razor Pages)-Datei oder das *Controllers/ManageController.cs* (MVC)-Datei.</span><span class="sxs-lookup"><span data-stu-id="b59aa-127">You can change it by looking for the `GenerateQrCodeUri(string email, string unformattedKey)` method in the *Pages/Account/Manage/EnableAuthenticator.cshtml.cs* (Razor Pages) file or the *Controllers/ManageController.cs* (MVC) file.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="b59aa-128">Der Standardcode aus der Vorlage sieht wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="b59aa-128">The default code from the template looks as follows:</span></span>

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

<span data-ttu-id="b59aa-129">Der zweite Parameter im Aufruf von `string.Format` der Standortname, der den Namen der Projektmappe entnommen wird.</span><span class="sxs-lookup"><span data-stu-id="b59aa-129">The second parameter in the call to `string.Format` is your site name, taken from your solution name.</span></span> <span data-ttu-id="b59aa-130">Sie können auf einen beliebigen Wert geändert werden, aber es muss immer ein URL-codiert sein.</span><span class="sxs-lookup"><span data-stu-id="b59aa-130">It can be changed to any value, but it must always be URL encoded.</span></span>

## <a name="using-a-different-qr-code-library"></a><span data-ttu-id="b59aa-131">Verwenden einer anderen QR-Code-Bibliothek</span><span class="sxs-lookup"><span data-stu-id="b59aa-131">Using a different QR Code library</span></span>

<span data-ttu-id="b59aa-132">Sie können den QR-Code-Bibliothek mit Ihrer bevorzugten Bibliothek ersetzen.</span><span class="sxs-lookup"><span data-stu-id="b59aa-132">You can replace the QR Code library with your preferred library.</span></span> <span data-ttu-id="b59aa-133">Der HTML-Code enthält eine `qrCode` Element in der Sie einen QR-Code, indem Mechanismus platzieren können Ihrer Bibliothek enthält.</span><span class="sxs-lookup"><span data-stu-id="b59aa-133">The HTML contains a `qrCode` element into which you can place a QR Code by whatever mechanism your library provides.</span></span>

<span data-ttu-id="b59aa-134">Die korrekt formatierte URL für den QR-Code finden Sie in der:</span><span class="sxs-lookup"><span data-stu-id="b59aa-134">The correctly formatted URL for the QR Code is available in the:</span></span>

* <span data-ttu-id="b59aa-135">`AuthenticatorUri` die Eigenschaft des Modells.</span><span class="sxs-lookup"><span data-stu-id="b59aa-135">`AuthenticatorUri` property of the model.</span></span>
* <span data-ttu-id="b59aa-136">`data-url` Eigenschaft in der `qrCodeData` Element.</span><span class="sxs-lookup"><span data-stu-id="b59aa-136">`data-url` property in the `qrCodeData` element.</span></span>

## <a name="totp-client-and-server-time-skew"></a><span data-ttu-id="b59aa-137">TOTP-Client und Server zeitabweichung auf.</span><span class="sxs-lookup"><span data-stu-id="b59aa-137">TOTP client and server time skew</span></span>

<span data-ttu-id="b59aa-138">TOTP (zeitbasierte Einmalkennwort) Authentifizierung hängt von dem Server und dem Authentifikator Gerät müssen die genaue Uhrzeit ab.</span><span class="sxs-lookup"><span data-stu-id="b59aa-138">TOTP (Time-based One-Time Password) authentication depends on both the server and authenticator device having an accurate time.</span></span> <span data-ttu-id="b59aa-139">Token, nur 30 Sekunden dauern.</span><span class="sxs-lookup"><span data-stu-id="b59aa-139">Tokens only last for 30 seconds.</span></span> <span data-ttu-id="b59aa-140">TOTP 2FA Anmeldungen fehlschlagen, sicher, dass die Serverzeit genaue und vorzugsweise mit einer genauen NTP-Dienst synchronisiert wird.</span><span class="sxs-lookup"><span data-stu-id="b59aa-140">If TOTP 2FA logins are failing, check that the server time is accurate, and preferably synchronized to an accurate NTP service.</span></span>

::: moniker-end
