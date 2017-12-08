---
uid: web-api/overview/security/working-with-ssl-in-web-api
title: Arbeiten mit SSL in Web-API | Microsoft Docs
author: MikeWasson
description: "Veranschaulicht die Verwendung von SSL mit ASP.NET Web-API, einschließlich der Verwendung von SSL-Clientzertifikate."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/12/2012
ms.topic: article
ms.assetid: 97f6164f-59cf-45c0-b820-e4aa29b45396
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/working-with-ssl-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 8c631900c8c5ab6097e0cb9fd4a71abbcba1c88b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="working-with-ssl-in-web-api"></a>Arbeiten mit SSL in Web-API
====================
durch [Mike Wasson](https://github.com/MikeWasson)

Mehrere allgemeine Authentifizierungsschemas sind über einfaches HTTP nicht sicher. Insbesondere senden Standardauthentifizierung und Formularauthentifizierung unverschlüsselte Anmeldeinformationen. Sicher, diese Authentifizierungsschemas *müssen* SSL verwenden. Darüber hinaus können der SSL-Clientzertifikate verwendet werden, um Clients zu authentifizieren.

## <a name="enabling-ssl-on-the-server"></a>Aktivieren von SSL auf dem Server

So richten SSL in IIS 7 oder höher

- Erstellen oder Abrufen eines Zertifikats. Zu Testzwecken können Sie ein selbstsigniertes Zertifikat erstellen.
- Fügen Sie eine HTTPS-Bindung hinzu.

Weitere Informationen finden Sie unter [zum Einrichten von SSL in IIS 7](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).

Bei lokalen Tests können Sie SSL in IIS Express aus Visual Studio aktivieren. Legen Sie im Fenster Eigenschaften **SSL aktiviert** auf **"true"**. Beachten Sie den Wert der **SSL-URL**; verwenden Sie diese URL für das HTTPS-Verbindungen zu testen.

![](working-with-ssl-in-web-api/_static/image1.png)

### <a name="enforcing-ssl-in-a-web-api-controller"></a>Erzwingen von SSL in einem Web-API-Controller

Wenn Sie eine HTTPS und eine HTTP-Bindung haben, können Clients weiterhin HTTP verwenden, auf die Website zugreifen. Sie können einige Ressourcen, die über HTTP verfügbar sein, während andere Ressourcen SSL erforderlich ist. In diesem Fall verwenden Sie einen Aktionsfilter So fordern Sie SSL für die geschützten Ressourcen. Der folgende Code zeigt eine Web-API-Authentifizierungsfilter, die prüft, ob SSL:

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample1.cs)]

Fügen Sie diesen Filter für alle Web-API-Aktionen, die SSL erfordern:

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample2.cs)]

## <a name="ssl-client-certificates"></a>SSL-Clientzertifikate

SSL ermöglicht die Authentifizierung mithilfe von Zertifikaten der Public Key-Infrastruktur. Der Server muss ein Zertifikat bereitstellen, die der Server an den Client authentifiziert. Es ist weniger gängig, damit der Client ein Zertifikat mit dem Server nicht bereitgestellt, aber dies ist eine Möglichkeit zum Authentifizieren von Clients. Um Clientzertifikate mit SSL zu verwenden, benötigen Sie eine Möglichkeit, signierte Zertifikate für Ihre Benutzer zu verteilen. Für viele Anwendungstypen Dies ist eine gute Benutzeroberfläche nicht, aber in einigen Umgebungen (z. B. Enterprise) kann es praktikabel sein.

| Vorteile | Nachteile |
| --- | --- |
| -Zertifikats-Anmeldeinformationen sind stärker als Benutzername/Kennwort. -SSL stellt einen vollständigen sicheren Kanal mit Authentifizierung, Integrität und Verschlüsselung von Nachrichten bereit. | -Sie müssen erhalten und Verwalten von PKI-Zertifikaten. -Clientplattform muss SSL-Clientzertifikate unterstützen. |

Zum Konfigurieren von IIS, um Clientzertifikate zu akzeptieren, öffnen Sie IIS-Manager, und führen Sie die folgenden Schritte aus:

1. Klicken Sie auf der Websiteknoten in der Strukturansicht.
2. Doppelklicken Sie auf die **SSL-Einstellungen** Funktion in der Mitte.
3. Klicken Sie unter **Clientzertifikate**, wählen Sie eine der folgenden Optionen: 

    - **Akzeptieren Sie**: IIS ein Zertifikat vom Client akzeptiert, aber es ist nicht erforderlich sind.
    - **Erfordern**: ein Clientzertifikat erforderlich. (Wenn Sie diese Option aktivieren, müssen Sie auch "SSL erforderlich" auswählen)

Sie können diese Optionen auch in der Datei "applicationHost.config" festlegen:

[!code-xml[Main](working-with-ssl-in-web-api/samples/sample3.xml)]

Die **häufig "sslnegotiatecert"** Flag bedeutet, dass IIS ein Zertifikat vom Client akzeptiert, aber es ist nicht erforderlich sind (entspricht der Option "Annehmen" im IIS-Manager). Um ein Zertifikat erforderlich ist, legen Sie die **"sslrequirecert"** Flag. Zu Testzwecken können Sie auch diese Optionen in IIS Express in der lokalen Applicationhost festlegen. Config-Datei im Verzeichnis "Documents\IISExpress\config".

### <a name="creating-a-client-certificate-for-testing"></a>Erstellen ein Clientzertifikat zu Testzwecken

Zu Testzwecken können Sie [MakeCert.exe](https://msdn.microsoft.com/en-US/library/bfsktky3.aspx) ein Clientzertifikat erstellen. Erstellen Sie zunächst eine Test-Stammzertifizierungsstelle:

[!code-console[Main](working-with-ssl-in-web-api/samples/sample4.cmd)]

MakeCert fordert Sie zur Eingabe eines Kennworts für den privaten Schlüssel.

Fügen Sie anschließend das Zertifikat mit dem Test, des Servers "Vertrauenswürdige Stammzertifizierungsstellen" gespeichert werden, wie folgt:

1. Öffnen Sie die MMC.
2. Klicken Sie unter **Datei**Option **Snap-In hinzufügen/entfernen**.
3. Wählen Sie **Computerkonto**.
4. Wählen Sie **Sicherheitszertifikate** und schließen Sie den Assistenten ab.
5. Erweitern Sie unter im Navigationsbereich den Knoten "Vertrauenswürdige Stammzertifizierungsstellen" aus.
6. Auf der **Aktion** Sie im Menü **alle Aufgaben**, und klicken Sie dann auf **Import** um den Zertifikatimport-Assistenten zu starten.
7. Navigieren Sie zu der Zertifikatdatei, die Datei TempCA.cer.
8. Klicken Sie auf **öffnen**, klicken Sie dann auf **Weiter** und schließen Sie den Assistenten ab. (Sie werden aufgefordert werden, um das Kennwort erneut eingeben.)

Nun erstellen Sie ein Clientzertifikat, das durch das erste Zertifikat signiert ist:

[!code-console[Main](working-with-ssl-in-web-api/samples/sample5.cmd)]

### <a name="using-client-certificates-in-web-api"></a>Verwenden Clientzertifikate in Web-API

Auf der Serverseite erhalten Sie das Clientzertifikat durch Aufrufen von [GetClientCertificate](https://msdn.microsoft.com/en-us/library/system.net.http.httprequestmessageextensions.getclientcertificate.aspx) für die Anforderungsnachricht. Die Methode gibt null, wenn kein Clientzertifikat vorhanden ist. Andernfalls gibt es eine **X509Certificate2** Instanz. Verwenden Sie dieses Objekt zum Abrufen von Informationen aus dem Zertifikat, z. B. dem Aussteller und Antragsteller. Anschließend können Sie diese Informationen für die Authentifizierung und/oder Autorisierung verwenden.

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample6.cs)]
