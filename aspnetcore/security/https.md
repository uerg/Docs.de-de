---
title: Einrichten der HTTPS-Entwicklung in ASP.NET Core
author: Rick-Anderson
description: "Veranschaulicht das Einrichten von HTTPS für die Entwicklung in ASP.NET Core 2.0."
keywords: ASP.NET Core, SSL, HTTPS
ms.author: riande
manager: wpickett
ms.date: 05/10/2017
ms.topic: article
ms.assetid: 94f2f1a4-7d46-45e2-a085-a57916e41724
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/https
ms.openlocfilehash: 7c366ffbac71152c2f29901ff12bac2962e83e3e
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2017
---
# <a name="setting-up-https-for-development-in-aspnet-core"></a><span data-ttu-id="4639e-104">Einrichten der HTTPS-Entwicklung in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4639e-104">Setting up HTTPS for development in ASP.NET Core</span></span>

> [!NOTE] 
> <span data-ttu-id="4639e-105">Dieses Thema gilt für ASP.NET Core 2.0 Preview 1</span><span class="sxs-lookup"><span data-stu-id="4639e-105">This topic applies to ASP.NET Core 2.0 Preview 1</span></span>

<span data-ttu-id="4639e-106">Sie können Ihre Anwendung zum Verwenden von HTTPS zu HTTPS in Ihrer produktionsumgebung simulieren während der Entwicklung konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="4639e-106">You can configure your application to use HTTPS during development to simulate HTTPS in your production environment.</span></span> <span data-ttu-id="4639e-107">Aktivieren von HTTPS möglicherweise zum Aktivieren der Integration verschiedener Identitätsanbieter erforderlich (z. B. [Azure AD](https://azure.microsoft.com/services/active-directory) und [Azure AD B2C](https://azure.microsoft.com/services/active-directory-b2c/)).</span><span class="sxs-lookup"><span data-stu-id="4639e-107">Enabling HTTPS may be required to enable integration with various identity providers (like [Azure AD](https://azure.microsoft.com/services/active-directory) and [Azure AD B2C](https://azure.microsoft.com/services/active-directory-b2c/)).</span></span>

<a name="iisxpress"></a>

<span data-ttu-id="4639e-108">Wenn Sie Visual Studio oder IIS Express installiert haben, wird IIS Express Entwicklung-Zertifikat unter Windows in Ihrem Zertifikatspeicher LocalMachine sein.</span><span class="sxs-lookup"><span data-stu-id="4639e-108">On Windows if you’ve installed Visual Studio or IIS Express, the IIS Express Development Certificate will be in your LocalMachine certificate store.</span></span> <span data-ttu-id="4639e-109">Sie können die Projekteigenschaften in Visual Studio für dieses Zertifikat verwenden, wenn IIS Express zurückliegt aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="4639e-109">You can update your project properties in Visual Studio to use this certificate when running behind IIS Express.</span></span>

   * <span data-ttu-id="4639e-110">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste des Projekts, und wählen **Eigenschaften**.</span><span class="sxs-lookup"><span data-stu-id="4639e-110">In Solution Explorer, right-click the project and select **Properties**.</span></span>
   * <span data-ttu-id="4639e-111">Wählen Sie im linken Bereich **Debuggen**.</span><span class="sxs-lookup"><span data-stu-id="4639e-111">On the left pane, select **Debug**.</span></span>
   * <span data-ttu-id="4639e-112">Überprüfen Sie **SSL aktivieren**</span><span class="sxs-lookup"><span data-stu-id="4639e-112">Check **Enable SSL**</span></span>
   * <span data-ttu-id="4639e-113">Kopieren Sie die SSL-URL, und fügen Sie ihn in die **App-URL**</span><span class="sxs-lookup"><span data-stu-id="4639e-113">Copy the SSL URL and paste it into the **App URL**</span></span>

![Debuggen Sie auf der Registerkarte Eigenschaften der Webanwendung](enforcing-ssl/_static/ssl.png)

<span data-ttu-id="4639e-115">Für die Entwicklung können Sie IIS Express Entwicklungszertifikat verwenden, sofern dieser verfügbar ist oder erstellen ein neues Zertifikat zu Entwicklungszwecken.</span><span class="sxs-lookup"><span data-stu-id="4639e-115">For development you can use the IIS Express Development Certificate if it is available, or create a new certificate for development purposes.</span></span> <span data-ttu-id="4639e-116">Das Entwicklungszertifikat sollten konfiguriert werden, der `appsettings.Development.json` Datei, sodass sie nicht in der Produktion verwendet wird:</span><span class="sxs-lookup"><span data-stu-id="4639e-116">The development certificate should be configured in the `appsettings.Development.json` file so that it is not used in production:</span></span>

```json
{
  "Certificates": {
    "HTTPS": {
      "Source": "Store",
      "StoreLocation": "LocalMachine",
      "StoreName": "My",
      "Subject": "CN=localhost",
      "AllowInvalid": true
    }
  }
}
```

<span data-ttu-id="4639e-117">Eine app mit dieser Konfiguration wird in der produktionsumgebung ausgeführt wird, löst eine Ausnahme, die besagt "Kein Zertifikat mit dem Namen"HTTPS"in der Konfiguration für die aktuelle Umgebung (Produktion) gefunden".</span><span class="sxs-lookup"><span data-stu-id="4639e-117">An app with this configuration running in production will throw an exception saying "No certificate named 'HTTPS' found in configuration for the current environment (Production)".</span></span> <span data-ttu-id="4639e-118">Wechseln der [Umgebung](xref:fundamentals/environments) auf `Development`legen die `ASPNETCORE_ENVIRONMENT` Umgebungsvariablen `Development`.</span><span class="sxs-lookup"><span data-stu-id="4639e-118">To switch the [environment](xref:fundamentals/environments) to `Development`, set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development`.</span></span>

<span data-ttu-id="4639e-119">Wenn Sie nicht die IIS Express Entwicklungszertifikat installiert haben, können Sie ein Entwicklungszertifikat selbst erstellen.</span><span class="sxs-lookup"><span data-stu-id="4639e-119">If you do not have the IIS Express Development Certificate installed, you can create a development certificate yourself.</span></span> <span data-ttu-id="4639e-120">Unter Windows können Sie erstellen Sie ein Entwicklungszertifikat und dem vertrauenswürdigen Stammspeicher für den aktuellen Benutzer hinzufügen, durch die folgenden PowerShell-Befehle in einem Eingabeaufforderungsfenster mit erhöhten Rechten ausführen:</span><span class="sxs-lookup"><span data-stu-id="4639e-120">On Windows you can create a development certificate and add it to the trusted root store for the current user by running the following PowerShell commands in an elevated prompt:</span></span>

```powershell
$cert = New-SelfSignedCertificate -Subject localhost -DnsName localhost -FriendlyName "ASP.NET Core Development" -KeyUsage DigitalSignature -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.1") 
Export-Certificate -Cert $cert -FilePath cert.cer
Import-Certificate -FilePath cert.cer -CertStoreLocation cert:/CurrentUser/Root
```

<a name="OpenSSL"></a>

## <a name="kestrel-on--macos-and-linux"></a><span data-ttu-id="4639e-121">Kestrel unter Mac OS und Linux</span><span class="sxs-lookup"><span data-stu-id="4639e-121">Kestrel on  macOS and Linux</span></span>

<span data-ttu-id="4639e-122">Sie können Kestrel Lauschen über HTTPS konfigurieren einen Endpunkt mit der gewünschten IP-Adresse, Port und Zertifikat konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="4639e-122">You can  configure Kestrel to listen over HTTPS by configuring an endpoint with the desired IP address, port, and certificate.</span></span> <span data-ttu-id="4639e-123">Das Zertifikat kann konfigurierten Inline sein oder in der obersten Ebene `Certificates` Abschnitt, und klicken Sie dann anhand des Namens verwiesen:</span><span class="sxs-lookup"><span data-stu-id="4639e-123">The certificate can be configured inline, or in the top-level `Certificates` section and then referenced by name:</span></span>

```json
{
  "Kestrel": {
    "Endpoints": {
      "LocalhostHttps": {
        "Address": "127.0.0.1",
        "Port": "43434",
        "Certificate": "HTTPS"
      }
    }
  }
}

```

<span data-ttu-id="4639e-124">Unter Mac OS und Linux können Sie ein selbstsigniertes Zertifikat für die Verwendung von HTTPS erstellen [OpenSSL](https://www.openssl.org/):</span><span class="sxs-lookup"><span data-stu-id="4639e-124">On macOS and Linux you can create a self-signed certificate for HTTPS using [OpenSSL](https://www.openssl.org/):</span></span>

```bash
openssl req -new -x509 -newkey rsa:2048 -keyout localhost.key -out localhost.cer -days 365 -subj /CN=localhost
openssl pkcs12 -export -out certificate.pfx -inkey localhost.key -in localhost.cer
```

<span data-ttu-id="4639e-125">Sobald die `certificate.pfx` Datei generiert wurde, konfigurieren Sie das HTTPS-Zertifikat in Ihre `appsettings.Development.json` Datei:</span><span class="sxs-lookup"><span data-stu-id="4639e-125">Once the `certificate.pfx` file has been generated, configure the HTTPS certificate in your `appsettings.Development.json` file:</span></span>

```json
{
  "Certificates": {
    "HTTPS": {
      "Source": "File",
      "Path": "certificate.pfx"
    }
  }
}
```

<span data-ttu-id="4639e-126">Sie müssen auch die Passphrase für das Zertifikat angeben, indem Sie die Konfigurationseigenschaft "Zertifikate: HTTPS:Password" festlegen.</span><span class="sxs-lookup"><span data-stu-id="4639e-126">You will also need to specify the passphrase for the certificate by setting the “Certificates:HTTPS:Password” config property.</span></span> <span data-ttu-id="4639e-127">Kennwörter sollten nicht in Klartext gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="4639e-127">Passwords should not be stored in plain text.</span></span> <span data-ttu-id="4639e-128">Finden Sie unter [sicher Speicher der geheime Schlüssel während der Entwicklung von Apps](app-secrets.md) für angemessene Behandlung des das Zertifikats-Passphrase.</span><span class="sxs-lookup"><span data-stu-id="4639e-128">See [Safe Storage of App Secrets During Development](app-secrets.md) for appropriate handling of the certificate passphrase.</span></span>

<span data-ttu-id="4639e-129">Auf MacOS können Sie [Hinzufügen des Zertifikats zu Schlüsselbund](https://support.apple.com/kb/PH20129?locale=en_US) und [ändern Sie die Einstellungen für die Vertrauensstellung](https://support.apple.com/kb/PH20127?locale=en_US&viewlocale=en_US) , damit er während der Entwicklung für HTTPS als vertrauenswürdig ist.</span><span class="sxs-lookup"><span data-stu-id="4639e-129">On macOS you can [add the certificate to your keychain](https://support.apple.com/kb/PH20129?locale=en_US) and [change its trust settings](https://support.apple.com/kb/PH20127?locale=en_US&viewlocale=en_US) so that it is trusted for HTTPS during development.</span></span> <span data-ttu-id="4639e-130">Beim Hinzufügen des Zertifikats auf Ihrem Schlüsselbund (das Äquivalent der `CurrentUser/My` speichern unter Windows) führen Sie den folgenden Befehl:</span><span class="sxs-lookup"><span data-stu-id="4639e-130">To add the certificate to your keychain (the equivalent of the `CurrentUser/My` store on Windows) run the following command:</span></span>

```bash
security import certificate.pfx -k ~/Library/Keychains/login.keychain-db
```

<span data-ttu-id="4639e-131">Und anschließend das Zertifikat als vertrauenswürdig einstufen:</span><span class="sxs-lookup"><span data-stu-id="4639e-131">And then to trust the certificate:</span></span>

```bash
security add-trusted-cert localhost.cer
```

<span data-ttu-id="4639e-132">Anschließend können Sie Ihre app zur Verwendung dieses Zertifikats in der Entwicklung wie folgt konfigurieren:</span><span class="sxs-lookup"><span data-stu-id="4639e-132">You can then configure your app to use this certificate in development like this:</span></span>

```json
{
  "Certificates": {
    "HTTPS": {
      "Source": "Store",
      "StoreLocation": "CurrentUser",
      "StoreName": "My",
      "Subject": "CN=localhost",
      "AllowInvalid": true
    }
  }
}
```
