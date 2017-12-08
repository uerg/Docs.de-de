---
uid: signalr/overview/older-versions/introduction-to-security
title: "Einführung in die SignalR-Sicherheit (SignalR 1.x) | Microsoft Docs"
author: pfletcher
description: "Beschreibt die Sicherheitsprobleme, die Sie beim Entwickeln einer Anwendung SignalR berücksichtigen müssen."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 715a4059-d307-4631-abbb-c789c95d6eb4
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/introduction-to-security
msc.type: authoredcontent
ms.openlocfilehash: 04487614b219f8f6f8f0524c3b5f1aa42480c4d3
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="introduction-to-signalr-security-signalr-1x"></a>Einführung in die SignalR-Sicherheit (SignalR 1.x)
====================
durch [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)

> Dieser Artikel beschreibt die Sicherheitsprobleme, die Sie beim Entwickeln einer Anwendung SignalR berücksichtigen müssen.


## <a name="overview"></a>Übersicht

Dieses Dokument enthält folgende Abschnitte:

- [Schlüsselbegriffe der Sicherheit SignalR](#concepts)

    - [Authentifizierung und Autorisierung](#authentication)
    - [Verbindungstoken](#connectiontoken)
    - [Tritt Gruppen wieder, bei einer erneuten Verbindung](#rejoingroup)
- [Wie wird verhindert, dass SignalR Cross Anforderungsfälschung](#csrf)
- [Sicherheitsempfehlungen für SignalR](#recommendations)

    - [Secure Socket Layer (SSL)-Protokoll](#ssl)
    - [Verwenden Sie Gruppen nicht als Sicherheitsmechanismus](#groupsecurity)
    - [Behandlung von problemlos die Eingabe von clients](#input)
    - [Eine Änderung in den Status von User mit einer aktiven Verbindung abstimmen](#reconcile)
    - [Automatisch generierte Dateien der JavaScript-proxy](#autogen)
    - [Ausnahmen](#exceptions)

<a id="concepts"></a>

## <a name="signalr-security-concepts"></a>Schlüsselbegriffe der Sicherheit SignalR

<a id="authentication"></a>

### <a name="authentication-and-authorization"></a>Authentifizierung und Autorisierung

SignalR dient in der bestehenden Struktur der Authentifizierung für eine Anwendung integriert werden. Es stellt keine Funktionen bereit, zum Authentifizieren von Benutzern. Stattdessen wird Benutzer authentifizieren, wie Sie in der Anwendung normalerweise, und klicken Sie dann mit den Ergebnissen der Authentifizierung in Ihrem Code SignalR arbeiten. Beispielsweise können Sie authentifizieren Ihrer Benutzer mit ASP.NET-Formularauthentifizierung und dann in Ihrem Hub erzwingen, welche Benutzer oder Rollen sind berechtigt, eine Methode aufzurufen. In Ihrem Hub können auch übergeben Authentifizierungsinformationen, z. B. Benutzernamen oder eine Rolle an den Client, ob ein Benutzer angehört.

SignalR stellt die [autorisieren](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) Attribut, um anzugeben, welche Benutzer Zugriff auf einen Hub oder eine Methode haben. Die Authorize-Attribut auf einen Hub oder bestimmte Methoden in einem Hub angewendet. Ohne die Authorize-Attribut stehen alle öffentliche Methoden des Hubs ein Client, der mit dem Hub verbunden ist. Weitere Informationen zu Hubs finden Sie unter [Authentifizierung und Autorisierung für SignalR-Hubs](../security/hub-authorization.md).

Die `Authorize` Attribut wird nur mit Hubs verwendet. Autorisierungsregeln zu erzwingen, bei Verwendung einer `PersistentConnection` müssen Sie überschreiben die `AuthorizeRequest` Methode. Weitere Informationen zum permanenten Verbindungen finden Sie unter [Authentifizierung und Autorisierung für den permanenten SignalR-Verbindungen](../security/persistent-connection-authorization.md).

<a id="connectiontoken"></a>

### <a name="connection-token"></a>Verbindungstoken

SignalR verringert das Risiko von böswillige Befehle durch Überprüfen der Identität des Absenders ausführen. Ein Verbindungstoken, die die Verbindungs-Id und den Benutzernamen für authentifizierte Benutzer enthält, wird zwischen Client und Server für jede Anforderung übergeben. Die Verbindungs-Id ist ein eindeutiger Bezeichner, der vom Server nach dem Zufallsprinzip generiert wird, wenn eine neue Verbindung erstellt wird, und wird für die Dauer der Verbindung beibehalten. Der Benutzername wird von der Authentifizierungsmechanismus für die Webanwendung bereitgestellt. Das Verbindungstoken ist eine digitale Signatur und Verschlüsselung geschützt.

![](introduction-to-security/_static/image2.png)

Für jede Anforderung überprüft der Server den Inhalt des Tokens, um sicherzustellen, dass der angegebene Benutzer die Anforderung stammt. Der Benutzername muss die Verbindungs-Id entsprechen. Validieren Sie die Verbindungs-Id und der Benutzername, verhindert, dass SignalR einen böswilligen Benutzer problemlos Identität eines anderen Benutzers. Wenn der Server das Verbindungstoken nicht überprüfen kann, schlägt die Anforderung fehl.

![](introduction-to-security/_static/image4.png)

Da die Verbindungs-Id des Vorgangs der Überprüfung ist, sollten Sie nicht offenlegen Verbindungs-Id eines Benutzers an andere Benutzer oder den Wert auf dem Client, z. B. in einem Cookie speichern.

<a id="rejoingroup"></a>

### <a name="rejoining-groups-when-reconnecting"></a>Tritt Gruppen wieder, bei einer erneuten Verbindung

Standardmäßig wird die SignalR-Anwendung automatisch neu zuweisen ein Benutzers an den entsprechenden Gruppen bei einer erneuten Verbindung aus einer temporären Unterbrechung, z. B. wenn eine Verbindung gelöscht und neu eingerichtet werden, bevor die Verbindung ein Timeout eintritt. Beim Wiederherstellen der Verbindung übergibt der Client ein Token von Gruppe, die die Verbindungs-Id und die zugewiesenen Gruppen enthält. Das Token Gruppe digital signiert und verschlüsselt. Der Client behält die gleichen Verbindungs-Id nach eine erneute Verbindung; aus diesem Grund muss die Verbindungs-Id, die vom Client erneut verbundene übergeben die vorherige vom Client verwendeten Verbindungs-Id überein. Diese Überprüfung wird verhindert, dass einen böswilligen Benutzer Anforderungen, nicht autorisierten Gruppe beizutreten, bei einer erneuten Verbindung zu übergeben.

Allerdings ist es wichtig zu beachten Sie, dass das Token für die Gruppe nicht abläuft. Wenn ein Benutzer zu einer Gruppe, in der Vergangenheit liegen gehören, aber aus dieser Gruppe gesperrt wurde, kann dieser Benutzer werden ein Token für die Gruppe zu imitieren, die die unzulässige Gruppe enthält. Wenn Sie sicher zu verwalten, welche Benutzer auf welche Gruppen angehören müssen, müssen Sie diese Daten auf dem Server, z. B. in einer Datenbank zu speichern. Anschließend fügen Sie Logik Ihrer Anwendung, die auf dem Server überprüft, ob ein Benutzer zu einer Gruppe gehört. Ein Beispiel für das Überprüfen der Gruppenmitgliedschaft, finden Sie unter [arbeiten mit Gruppen](../guide-to-the-api/working-with-groups.md).

Automatisch ein Gruppen gilt nur, wenn nach einer temporären Unterbrechung erneut eine Verbindung hergestellt wird. Wenn ein Benutzer durch die Verbindung trennt muss die Weg von der Anwendung oder die Anwendung erneut gestartet wird, Ihre Anwendung Navigieren zum Hinzufügen dieser Benutzer den richtigen Gruppen verarbeiten. Weitere Informationen finden Sie unter [arbeiten mit Gruppen](../guide-to-the-api/working-with-groups.md).

<a id="csrf"></a>

## <a name="how-signalr-prevents-cross-site-request-forgery"></a>Wie wird verhindert, dass SignalR Cross Anforderungsfälschung

Cross-Site Request Fälschung Websiteübergreifender ist ein Angriff, in denen eine bösartige Website sendet eine Anforderung mit einem gefährdeten Standort, in dem der Benutzer zurzeit angemeldet ist. SignalR verhindert CSRF codewiederverwendung äußerst unwahrscheinlich, dass für eine bösartige Website, um eine gültige Anforderung für die SignalR-Anwendung zu erstellen.

### <a name="description-of-csrf-attack"></a>Beschreibung von CSRF-Angriffen

Hier ist ein Beispiel von CSRF-Angriffen:

1. Ein Benutzer sich anmeldet www.example.com, Formularauthentifizierung verwenden.
2. Der Server authentifiziert den Benutzer. Die Antwort vom Server enthält ein Authentifizierungscookie.
3. Ohne Abmelden, wechselt der Benutzer eine bösartige Website. Diese bösartige Website enthält die folgenden HTML-Formular: 

    [!code-html[Main](introduction-to-security/samples/sample1.html)]

 Beachten Sie, dass die formaktion an den Standort anfällig für nicht auf die bösartige Website sendet. Dies ist der "Cross-Site" Teil CSRF.
4. Der Benutzer klickt auf die Schaltfläche "Absenden". Der Browser umfasst das Authentifizierungscookie mit der Anforderung.
5. Die Anforderung auf dem Server "example.com" mit dem Kontext des Benutzers Authentifizierung ausgeführt und Aktionen möglich, die ein authentifizierter Benutzer tun darf.

Obwohl in diesem Beispiel wird den Benutzer auf die Formularschaltfläche erforderlich sind, konnte die böswillige Seite so fest, wie Sie problemlos ein Skript ausführen, das eine AJAX-Anforderung an die SignalR-Anwendung sendet. Darüber hinaus verhindert mit SSL eine CSRF-Angriffen nicht, da die bösartige Website "https://"-Anforderung senden kann.

In der Regel sind CSRF-Angriffen möglich für Websites, die Cookies für die Authentifizierung verwenden, da Browser alle relevanten Cookies an die Ziel-Website senden. CSRF-Angriffen sind jedoch nicht beschränkt auf Cookies ausnutzen. Beispielsweise Basis-und Digestauthentifizierung sind auch anfällig. Nachdem ein Benutzer mit Standard oder Digest-Authentifizierung anmeldet, sendet der Browser automatisch die Anmeldeinformationen an, bis die Sitzung beendet.

### <a name="csrf-mitigations-taken-by-signalr"></a>CSRF-Maßnahmen, die vom SignalR ausgeführt wird

SignalR akzeptiert die folgenden Schritte aus, um zu verhindern, dass eine bösartige Website gültige Anforderungen für die SignalR-Anwendung erstellen. Diese Schritte sind standardmäßig und erfordern keine Maßnahmen in Ihrem Code.

- **Deaktivieren Sie die domänenübergreifende Anforderungen**  
 Standardmäßig sind domänenübergreifende Anforderungen in einer SignalR-Anwendung, um zu verhindern, dass Benutzer Aufrufen eines SignalR-Endpunkts aus einer externen Domäne deaktiviert. Jede Anforderung, die aus einer externen Domäne stammen, wird automatisch als ungültig betrachtet und ist blockiert. Es wird empfohlen, dass Sie dieses Standardverhalten beibehalten; Andernfalls konnte eine bösartige Website Benutzer bringen, Senden von Befehlen an Ihrem Standort. Wenn Sie domänenübergreifende Anforderungen verwenden müssen, finden Sie unter [Gewusst wie: Herstellen einer Verbindung domänenübergreifende](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain) .
- **Übergeben Sie Verbindungstoken Abfragezeichenfolge nicht cookie**  
 SignalR übergibt das Verbindungstoken als Wert einer Abfragezeichenfolge, anstatt als Cookie. Speichern Sie das Verbindungstoken nicht als ein Cookie, das Verbindungstoken nicht versehentlich vom Browser weitergeleitet Wenn Schadsoftware erkannt wird. Darüber hinaus wird das Verbindungstoken nicht über die aktuelle Verbindung beibehalten. Ein böswilliger Benutzer kann nicht aus diesem Grund eine Anforderung unter Authentifizierungsinformationen des Benutzers vornehmen.
- **Vergewissern Sie sich Verbindungstoken**  
 Wie in beschrieben die [Verbindungstoken](#connectiontoken) Abschnitt, der Server weiß, welche Verbindungs-Id jeder authentifizierte Benutzer zugeordnet ist. Der Server verarbeitet keine Anforderung über eine Verbindungs-Id, die nicht mit den Benutzernamen übereinstimmt. Es ist unwahrscheinlich, dass ein böswilliger Benutzer eine gültige Anforderung erraten konnte, da ein böswilliger Benutzer den Benutzernamen und die aktuelle nach dem Zufallsprinzip generierte Verbindungs­ID kennen müsste. Dieser Verbindungs-Id wird ungültig, sobald die Verbindung beendet wird. Anonyme Benutzer sollten nicht auf vertrauliche Informationen zugreifen.

<a id="recommendations"></a>

## <a name="signalr-security-recommendations"></a>Sicherheitsempfehlungen für SignalR

<a id="ssl"></a>

### <a name="secure-socket-layers-ssl-protocol"></a>Secure Socket Layer (SSL)-Protokoll

Das SSL-Protokoll verwendet Verschlüsselung, um die Übertragung von Daten zwischen einem Client und Server sichern. Wenn Ihre Anwendung SignalR vertraulichen Informationen zwischen Client und Server übermittelt werden, verwenden Sie SSL für den Transport. Weitere Informationen zum Einrichten von SSL finden Sie unter [zum Einrichten von SSL in IIS 7](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).

<a id="groupsecurity"></a>

### <a name="do-not-use-groups-as-a-security-mechanism"></a>Verwenden Sie Gruppen nicht als Sicherheitsmechanismus

Gruppen sind eine einfache Möglichkeit im Zusammenhang stehenden Benutzer erfassen, aber sie sind dabei nicht um einen sicheren Mechanismus zum Beschränken des Zugriffs auf vertrauliche Informationen. Dies gilt insbesondere, wenn der Benutzer automatisch Gruppen während einer erneut hergestellten Verbindung erneut beitreten können. Erwägen Sie stattdessen eine Rolle von berechtigten Benutzern hinzugefügt und das Beschränken des Zugriffs auf eine hubmethode, die nur für Mitglieder dieser Rolle. Ein Beispiel zum Einschränken des Zugriffs basierend auf einer Rolle finden Sie unter [Authentifizierung und Autorisierung für SignalR-Hubs](../security/hub-authorization.md). Ein Beispiel der Benutzerzugriff auf Gruppen überprüfen, bei einer erneuten Verbindung finden Sie unter [arbeiten mit Gruppen](../guide-to-the-api/working-with-groups.md).

<a id="input"></a>

### <a name="safely-handling-input-from-clients"></a>Behandlung von problemlos die Eingabe von clients

Alle Eingaben von Clients, die für die Übertragung an andere Clients vorgesehen ist muss codiert werden, um sicherzustellen, dass ein böswilliger Benutzer keine Skripts für andere Benutzer gesendet werden. Es wird empfohlen, auf dem Server, statt die empfangende Clients Nachrichten zu codieren, da die SignalR-Anwendung kann viele verschiedene Arten von Clients. Aus diesem Grund funktioniert die HTML-Codierung für einen Webclient, jedoch nicht für andere Typen von Clients. Z. B. eine Client-Webmethode anzuzeigenden eine Chatnachricht genauso sicher behandelt den Benutzernamen und die Nachricht durch Aufrufen der `html()` Funktion.

[!code-html[Main](introduction-to-security/samples/sample2.html?highlight=3-4)]

<a id="reconcile"></a>

### <a name="reconciling-a-change-in-user-status-with-an-active-connection"></a>Eine Änderung in den Status von User mit einer aktiven Verbindung abstimmen

Wenn Authentifizierungsstatus eines Benutzers geändert wird, wenn eine aktive Verbindung vorhanden ist, der Benutzer wird eine Fehlermeldung, aus der hervorgeht, "die Identität des Benutzers kann nicht während einer aktiven SignalR-Verbindungs ändern". In diesem Fall sollte Ihre Anwendung erneut verbinden, mit dem Server, stellen Sie sicher, dass die Verbindungs-Id und den Benutzernamen koordiniert werden. Z. B. wenn die Anwendung kann der Benutzer abmelden, während eine aktive Verbindung vorhanden ist, wird der Benutzername für die Verbindung nicht mehr den Namen entspricht, der für die nächste Anforderung übergeben wird. Sie möchten, die Verbindung zu beenden, bevor der Abmeldung des Benutzers, und starten Sie ihn neu.

Allerdings ist es wichtig zu beachten, dass die meisten Anwendungen nicht benötigen, manuell beenden und starten die Verbindung. Wenn Ihre Anwendung Benutzer auf ein anderes Zeichenblatt nach der Anmeldung, z. B. das Standardverhalten in einer Web Forms-Anwendung oder MVC-Anwendung leitet oder die aktuelle Seite aktualisiert nach dem Abmelden, wird automatisch getrennt und nicht die aktive Verbindung ist weitere Aktion erforderlich.

Das folgende Beispiel zeigt, wie beenden und starten eine Verbindung aus, wenn der Benutzerstatus geändert hat.

[!code-html[Main](introduction-to-security/samples/sample3.html)]

Oder Authentifizierungsstatus des Benutzers möglicherweise ändern, wenn die Website gleitenden Ablauf bei der Formularauthentifizierung verwendet, und es keine Aktivität gibt, um das Authentifizierungscookie gültig bleiben. In diesem Fall wird der Benutzer wird abgemeldet, und der Benutzername entspricht nicht mehr den Benutzernamen in das Verbindungstoken. Sie können dieses Problem beheben, indem einige Skript, das in regelmäßigen Abständen eine Ressource auf dem Webserver, um das Authentifizierungscookie zu gültig halten fordert hinzufügen. Das folgende Beispiel zeigt, wie zum Anfordern einer Ressource alle 30 Minuten.

[!code-javascript[Main](introduction-to-security/samples/sample4.js)]

<a id="autogen"></a>

### <a name="automatically-generated-javascript-proxy-files"></a>Automatisch generierte Dateien der JavaScript-proxy

Wenn Sie nicht alle Hubs und Methoden in der JavaScript-Proxy-Datei für jeden Benutzer einschließen möchten, können Sie die automatische Generierung von der Datei deaktivieren. Sie können diese Option auswählen, wenn Sie mehrere Hubs und Methoden aufweisen, aber nicht jeden Benutzer, der alle Methoden berücksichtigen möchten. Deaktivieren Sie automatische Generierung, indem **EnableJavaScriptProxies** auf **"false"**.

[!code-csharp[Main](introduction-to-security/samples/sample5.cs)]

Weitere Informationen über die JavaScript-Proxy-Dateien finden Sie unter [des generierten Proxys und wozu Sie](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy). <a id="exceptions"></a>

### <a name="exceptions"></a>Ausnahmen

Vermeiden Sie, ob Sie Ausnahmeobjekte an Clients übergeben wird, da die Objekte für vertrauliche Informationen zu den Clients zur Verfügung stellen können. Rufen Sie stattdessen eine Methode auf dem Client, in dem die entsprechende Fehlermeldung angezeigt.

[!code-csharp[Main](introduction-to-security/samples/sample6.cs)]
