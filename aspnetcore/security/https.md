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
ms.openlocfilehash: e06f4194d496b5b11aa867e66563bec317e735ff
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/11/2017
---
# <a name="setting-up-https-for-development-in-aspnet-core"></a>Einrichten der HTTPS-Entwicklung in ASP.NET Core

> [!NOTE] 
> Dieses Thema gilt für ASP.NET Core 2.0 Preview 1

Sie können Ihre Anwendung zum Verwenden von HTTPS zu HTTPS in Ihrer produktionsumgebung simulieren während der Entwicklung konfigurieren. Aktivieren von HTTPS möglicherweise zum Aktivieren der Integration verschiedener Identitätsanbieter erforderlich (z. B. [Azure AD](https://azure.microsoft.com/services/active-directory) und [Azure AD B2C](https://azure.microsoft.com/services/active-directory-b2c)).

<a name="iisxpress"></a>

Wenn Sie Visual Studio oder IIS Express installiert haben, wird IIS Express Entwicklung-Zertifikat unter Windows in Ihrem Zertifikatspeicher LocalMachine sein. Sie können die Projekteigenschaften in Visual Studio für dieses Zertifikat verwenden, wenn IIS Express zurückliegt aktualisieren.

   * Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste des Projekts, und wählen **Eigenschaften**.
   * Wählen Sie im linken Bereich **Debuggen**.
   * Überprüfen Sie **SSL aktivieren**
   * Kopieren Sie die SSL-URL, und fügen Sie ihn in die **App-URL**

![Debuggen Sie auf der Registerkarte Eigenschaften der Webanwendung](enforcing-ssl/_static/ssl.png)

Für die Entwicklung können Sie IIS Express Entwicklungszertifikat verwenden, sofern dieser verfügbar ist oder erstellen ein neues Zertifikat zu Entwicklungszwecken. Das Entwicklungszertifikat sollten konfiguriert werden, der `appsettings.Development.json` Datei, sodass sie nicht in der Produktion verwendet wird:

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

Eine app mit dieser Konfiguration wird in der produktionsumgebung ausgeführt wird, löst eine Ausnahme, die besagt "Kein Zertifikat mit dem Namen"HTTPS"in der Konfiguration für die aktuelle Umgebung (Produktion) gefunden". Wechseln der [Umgebung](xref:fundamentals/environments) auf `Development`legen die `ASPNETCORE_ENVIRONMENT` Umgebungsvariablen `Development`.

Wenn Sie nicht die IIS Express Entwicklungszertifikat installiert haben, können Sie ein Entwicklungszertifikat selbst erstellen. Unter Windows können Sie erstellen Sie ein Entwicklungszertifikat und dem vertrauenswürdigen Stammspeicher für den aktuellen Benutzer hinzufügen, durch die folgenden PowerShell-Befehle in einem Eingabeaufforderungsfenster mit erhöhten Rechten ausführen:

```powershell
$cert = New-SelfSignedCertificate -Subject localhost -DnsName localhost -FriendlyName "ASP.NET Core Development" -KeyUsage DigitalSignature -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.1") 
Export-Certificate -Cert $cert -FilePath cert.cer
Import-Certificate -FilePath cert.cer -CertStoreLocation cert:/CurrentUser/Root
```

<a name="OpenSSL"></a>

## <a name="kestrel-on--macos-and-linux"></a>Kestrel unter Mac OS und Linux

Sie können Kestrel Lauschen über HTTPS konfigurieren einen Endpunkt mit der gewünschten IP-Adresse, Port und Zertifikat konfigurieren. Das Zertifikat kann konfigurierten Inline sein oder in der obersten Ebene `Certificates` Abschnitt, und klicken Sie dann anhand des Namens verwiesen:

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

Unter Mac OS und Linux können Sie ein selbstsigniertes Zertifikat für die Verwendung von HTTPS erstellen [OpenSSL](https://www.openssl.org/):

```bash
openssl req -new -x509 -newkey rsa:2048 -keyout localhost.key -out localhost.cer -days 365 -subj /CN=localhost
openssl pkcs12 -export -out certificate.pfx -inkey localhost.key -in localhost.cer
```

Sobald die `certificate.pfx` Datei generiert wurde, konfigurieren Sie das HTTPS-Zertifikat in Ihre `appsettings.Development.json` Datei:

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

Sie müssen auch die Passphrase für das Zertifikat angeben, indem Sie die Konfigurationseigenschaft "Zertifikate: HTTPS:Password" festlegen. Kennwörter sollten nicht in Klartext gespeichert werden. Finden Sie unter [sicher Speicher der geheime Schlüssel während der Entwicklung von Apps](app-secrets.md) für angemessene Behandlung des das Zertifikats-Passphrase.

Auf MacOS können Sie [Hinzufügen des Zertifikats zu Schlüsselbund](https://support.apple.com/kb/PH20129?locale=en_US) und [ändern Sie die Einstellungen für die Vertrauensstellung](https://support.apple.com/kb/PH20127?locale=en_US&viewlocale=en_US) , damit er während der Entwicklung für HTTPS als vertrauenswürdig ist. Beim Hinzufügen des Zertifikats auf Ihrem Schlüsselbund (das Äquivalent der `CurrentUser/My` speichern unter Windows) führen Sie den folgenden Befehl:

```bash
security import certificate.pfx -k ~/Library/Keychains/login.keychain-db
```

Und anschließend das Zertifikat als vertrauenswürdig einstufen:

```bash
security add-trusted-cert localhost.cer
```

Anschließend können Sie Ihre app zur Verwendung dieses Zertifikats in der Entwicklung wie folgt konfigurieren:

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
