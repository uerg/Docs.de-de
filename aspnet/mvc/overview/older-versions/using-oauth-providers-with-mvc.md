---
uid: mvc/overview/older-versions/using-oauth-providers-with-mvc
title: Verwenden von OAuth-Anbietern mit MVC 4 | Microsoft-Dokumentation
author: tfitzmac
description: In diesem Tutorial erfahren Sie, wie Sie eine ASP.NET MVC 4-Webanwendung erstellen, die Benutzern ermöglicht, melden Sie sich mit den Anmeldeinformationen von einem externen Anbieter, z. B. Facebo...
ms.author: aspnetcontent
ms.date: 06/19/2013
ms.assetid: 7a87f16f-0e19-4f15-a88a-094ae866c4a2
msc.legacyurl: /mvc/overview/older-versions/using-oauth-providers-with-mvc
msc.type: authoredcontent
ms.openlocfilehash: 15f6b45706c0711d68b0780a7474d4c939a85fba
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37823325"
---
<a name="using-oauth-providers-with-mvc-4"></a>Verwenden von OAuth-Anbietern mit MVC 4
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> In diesem Tutorial erfahren Sie, wie Sie eine ASP.NET MVC 4-Webanwendung zu erstellen, können die Benutzer melden Sie sich mit Anmeldeinformationen aus einem externen Anbieter, z. B. Facebook, Twitter, Microsoft oder Google, und integrieren anschließend einige der Funktionen von diesen Anbietern in, Ihre Web-Anwendung. Der Einfachheit halber konzentriert sich in diesem Tutorial zum Arbeiten mit Anmeldeinformationen von Facebook.
> 
> Um die externe Anmeldeinformationen in einer ASP.NET MVC 5-Webanwendung verwenden zu können, finden Sie unter [erstellen Sie eine ASP.NET MVC 5-App mit Facebook und Google OAuth2 und OpenID-Sign-on](../security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
> 
> Aktivieren diese Anmeldeinformationen in Ihrer Web Sites bietet entscheidende Vorteile, da Sie Millionen Benutzer bereits Konten mit dieser externen Anbietern haben. Diese Benutzer möglicherweise eher für Ihre Website registrieren können, wenn sie nicht zum Erstellen und speichern einen neuen Satz von Anmeldeinformationen verfügen. Nachdem ein Benutzer über einen dieser Anbieter angemeldet hat, können Sie auch soziale Vorgänge vom Anbieter integrieren.


## <a name="what-youll-build"></a>Sie lernen Folgendes

In diesem Tutorial sind zwei Hauptziele:

1. Einen Benutzer sich mit den Anmeldeinformationen eines OAuth-Anbieters anmelden zu aktivieren.
2. Abrufen von Informationen vom Anbieter, und integrieren Sie diese Informationen, mit der kontoregistrierung für Ihre Website.

Obwohl in die Beispielen in diesem Tutorial zur Verwendung von Facebook als Authentifizierungsanbieter zu konzentrieren, können Sie den Code, um einen der Anbieter verwenden, ändern. Die Schritte zum Implementieren von beliebigen Anbietern sind sehr ähnlich, mit den Schritten, die Sie in diesem Lernprogramm sehen werden. Sie sehen nur die erhebliche Unterschiede bei direkten Aufrufen an den Anbieter-API festgelegt.

## <a name="prerequisites"></a>Erforderliche Komponenten

- [Microsoft Visual Studio 2012](https://www.microsoft.com/visualstudio/eng/downloads#vs) oder [Microsoft Visual Studio Express 2012 für Web](https://www.microsoft.com/visualstudio/eng/downloads#d-2012-express)

Or

- Microsoft Visual Studio 2010 SP1 oder [Visual Web Developer Express 2010 SP1](https://www.microsoft.com/visualstudio/eng/downloads#d-2010-express)
- [ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392)

In diesem Thema wird außerdem davon ausgegangen, dass Sie über grundlegende Kenntnisse über ASP.NET MVC und Visual Studio verfügen. Wenn Sie eine Einführung in ASP.NET MVC 4 benötigen, finden Sie unter [Einführung in ASP.NET MVC 4](getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md).

## <a name="create-the-project"></a>Erstellen eines Projekts

Klicken Sie in Visual Studio eine neue ASP.NET MVC 4-Webanwendung erstellen, und nennen sie &quot;OAuthMVC&quot;. Sie können entweder .NET Framework 4.5 oder 4 abzielen.

![Projekt erstellen](using-oauth-providers-with-mvc/_static/image1.png)

Wählen Sie im Fenster Neues ASP.NET MVC 4-Projekt **Internetanwendung** und lassen Sie **Razor** als die Ansichts-Engine.

![Wählen Sie die Internet-Anwendung](using-oauth-providers-with-mvc/_static/image2.png)

## <a name="enable-a-provider"></a>Aktivieren Sie einen Anbieter

Wenn Sie eine MVC 4-Webanwendung mit Internet Application-Vorlage erstellen, wird das Projekt erstellt, mit einer Datei mit dem Namen AuthConfig.cs in der App\_Startordner.

![AuthConfig-Datei](using-oauth-providers-with-mvc/_static/image3.png)

Die AuthConfig-Datei enthält Code zum Registrieren von Clients für die externe Authentifizierungsanbieter. Standardmäßig ist dieser Code auskommentiert, damit kein externer Anbieter aktiviert sind.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample1.cs)]

Sie müssen diesen Code, um die externe Authentifizierung-Client verwenden die auskommentierung aufheben. Kommentieren Sie Sie nur die Anbieter, die auf Ihrer Website enthalten sein sollen. In diesem Tutorial können Sie nur die Facebook-Anmeldeinformationen.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample2.cs)]

Beachten Sie, dass im obigen Beispiel, dass die Methode leere Zeichenfolgen für die Registrierungsparameter enthält. Wenn Sie versuchen, die die Anwendung nun ausführen, löst die Anwendung eine Argumentausnahme aus, da leere Zeichenfolgen für die Parameter sind nicht zulässig. Um gültige Werte bereitzustellen, müssen Sie Ihre Website mit den externen Anbietern, registrieren, wie im nächsten Abschnitt gezeigt.

## <a name="registering-with-an-external-provider"></a>Über einen externen Anbieter registrieren

Um Benutzer mit Anmeldeinformationen aus einem externen Anbieter authentifizieren zu können, müssen Sie Ihre Website mit dem Anbieter registrieren. Wenn Sie Ihre Website registrieren, erhalten Sie die Parameter (z. B. Key oder Id und Geheimnis), um einschließen, wenn den Client registrieren. Sie müssen ein Konto mit den Anbietern verfügen, die Sie verwenden möchten.

In diesem Tutorial werden nicht alle Schritte angezeigt, die Sie ausführen müssen, um für diese Anbieter zu registrieren. Diese Schritte sind in der Regel nicht schwierig. Um Ihre Website erfolgreich registriert haben, befolgen Sie die Anweisungen für diese Websites aus. Finden Sie Informationen zum Einstieg in die Registrierung einer Website, für die Developer-Website aus:

- [Facebook](https://developers.facebook.com/)
- [Google](https://developers.google.com/)
- [Microsoft](http://manage.dev.live.com/)
- [Twitter](https://dev.twitter.com/)

Wenn Sie Ihre Website mit Facebook zu registrieren, können Sie angeben &quot;"localhost"&quot; für die Domäne der Website und `&quot;http://localhost/&quot;` für die URL, wie in der folgenden Abbildung dargestellt. Verwenden "localhost" arbeitet mit den meisten Anbietern zusammen, aber derzeit funktioniert nicht mit dem Microsoft-Anbieter. Für den Microsoft-Anbieter müssen Sie eine gültige Website-URL einschließen.

![Registrieren der site](using-oauth-providers-with-mvc/_static/image4.png)

In der vorherigen Abbildung wurden die Werte für die app-Id, app-Geheimnis und e-Mail-Kontaktadresse entfernt. Wenn Sie tatsächlich auf Ihrer Website registrieren, werden diese Werte vorhanden sein. Sie sollten die Werte für die app-Id und app-Geheimnis zu beachten, da Sie sie Ihrer Anwendung hinzufügen möchten.

## <a name="creating-test-users"></a>Erstellen von Benutzern von Webleistungstests

Wenn Sie nicht erledigen, verwenden ein vorhandenes Facebook-Konto auf Ihre Website testen, können Sie diesen Abschnitt überspringen.

Sie können Benutzern von Webleistungstests problemlos für Ihre Anwendung in der Facebook-app-Management-Seite erstellen. Mit diesen Befehlen können Konten für die Anmeldung auf Ihrer Website zu testen. Sie Testbenutzer erstellen, indem Sie auf die **Rollen** Link im linken Navigationsbereich und anschließend auf die **erstellen** Link.

![Erstellen Sie Testbenutzer](using-oauth-providers-with-mvc/_static/image5.png)

Die Facebook-Website erstellt automatisch die Anzahl der Testkonten, die Sie anfordern.

## <a name="adding-application-id-and-secret-from-the-provider"></a>Anwendungs-Id und geheimen Schlüssel vom Anbieter hinzufügen

Nun, dass Sie die Id und den geheimen Clientschlüssel aus Facebook erhalten haben, wechseln Sie zurück zur AuthConfig, und fügen Sie sie als Parameterwerte. Die unten aufgeführten Werte sind nicht die tatsächlichen Werte.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample3.cs)]

## <a name="log-in-with-external-credentials"></a>Melden Sie sich mit externen Anmeldeinformationen

Das ist alles, was man tun, um die externe Anmeldeinformationen in Ihrer Website zu aktivieren. Führen Sie die Anwendung, und klicken Sie auf den Anmeldelink in der oberen rechten Ecke. Die Vorlage erkennt automatisch, dass Sie Facebook als Dienstanbieter registriert haben, und eine Schaltfläche für den Anbieter enthält. Wenn Sie mehrere Anbieter registrieren, ist eine Schaltfläche für jeden dieser automatisch mit eingeschlossen.

![externe Anmeldung](using-oauth-providers-with-mvc/_static/image6.png)

Gewusst wie: Anpassen die Anmeldung über die Schaltflächen für die externe Anbieter behandelt in diesem Tutorial nicht. Diese Informationen finden Sie unter [die Anmeldebenutzeroberfläche anpassen, bei Verwendung der OAuth-/OpenID](https://blogs.msdn.com/b/pranav_rastogi/archive/2012/08/24/customizing-the-login-ui-when-using-oauth-openid.aspx).

Klicken Sie auf die Schaltfläche "Facebook", um sich mit Facebook-Anmeldeinformationen anmelden. Wenn Sie einen der externe Anbieter auswählen, werden Sie an diesem Standort umgeleitet und aufgefordert, die von diesen Diensten anmelden.

Die folgende Abbildung zeigt den Anmeldebildschirm für Facebook an. Es enthält einen Hinweis, dass Sie Ihr Facebook-Konto für die Anmeldung an einem Standort mit dem Namen Oauthmvcexample verwenden.

![Facebook-Authentifizierung](using-oauth-providers-with-mvc/_static/image7.png)

Nach der Anmeldung mit Facebook-Anmeldeinformationen informiert der eine Seite den Benutzer, dass die Website zu grundlegenden Informationen zugreifen kann.

![Berechtigung](using-oauth-providers-with-mvc/_static/image8.png)

Nach der Auswahl **wechseln Sie zur App**, muss der Benutzer für die Website registrieren. Die folgende Abbildung zeigt die Registrierungsseite auf, nachdem ein Benutzer sich mit Facebook-Anmeldeinformationen angemeldet hat. Der Benutzername ist mit einem Namen des Anbieters in der Regel bereits vorhanden.

![register](using-oauth-providers-with-mvc/_static/image9.png)

Klicken Sie auf **registrieren** zum Abschließen der Registrierung. Schließen Sie den Browser.

Sie sehen, dass das neue Konto der Datenbank hinzugefügt wurde. Öffnen Sie im Server-Explorer die **DefaultConnection** Datenbank, und öffnen Sie die **Tabellen** Ordner.

![Datenbanktabellen](using-oauth-providers-with-mvc/_static/image10.png)

Mit der rechten Maustaste die **UserProfile** Tabelle, und wählen Sie **Tabellendaten anzeigen**.

![Anzeigen von Daten](using-oauth-providers-with-mvc/_static/image11.png)

Das neue Konto wird angezeigt, die, das Sie hinzugefügt haben. Sehen Sie sich die Daten in **Webseite\_OAuthMembership** Tabelle. Sie sehen, mehr Daten, die im Zusammenhang mit der der externe Anbieter für das Konto, das Sie gerade hinzugefügt haben.

Wenn Sie nur die externe Authentifizierung aktivieren möchten, sind Sie fertig. Allerdings können Sie weitere Informationen vom Anbieter in der neuen User Registration-Prozess integrieren, wie in den folgenden Abschnitten gezeigt.

## <a name="create-models-for-additional-user-information"></a>Erstellen von Modellen für zusätzliche Benutzerinformationen

Wie Sie in den vorherigen Abschnitten bemerkt haben, müssen Sie nicht zum Abrufen zusätzlichen Informationen für die Registrierung integriertes Konto arbeiten. Die meisten externen Anbieter aber auch wieder Weitere Informationen über den Benutzer übergeben. Die folgenden Abschnitte zeigen, wie Sie diese Informationen beibehalten, und speichern Sie ihn in einer Datenbank. Speziell, behält Sie Werte für den vollständigen Benutzernamen, den URI des Benutzers persönliche-Webseite und ein Wert, der angibt, ob es sich bei Facebook das Konto bestätigt wurde.

Verwenden Sie [Code First-Migrationen](https://msdn.microsoft.com/data/jj591621) zum Hinzufügen einer Tabelle zum Speichern von zusätzlichen Benutzerinformationen. Sie fügen die Tabelle zu einer vorhandenen Datenbank, daher Sie zunächst eine Momentaufnahme der aktuellen Datenbank zu erstellen müssen. Erstellen Sie eine Momentaufnahme der aktuellen Datenbank, können Sie später eine Migration erstellen, die nur die neue Tabelle enthält. So erstellen Sie eine Momentaufnahme der aktuellen Datenbank:

1. Öffnen der **-Paket-Manager-Konsole**
2. Führen Sie den Befehl **Enable-Migrations**
3. Führen Sie den Befehl **anfängliche hinzufügen-Migration – IgnoreChanges**
4. Führen Sie den Befehl **Datenbank aktualisieren**

Fügen Sie nun die neuen Eigenschaften. Öffnen der Datei AccountModels.cs im Ordner "Models" und suchen Sie die RegisterExternalLoginModel-Klasse. Die Klasse RegisterExternalLoginModel enthält Werte, die vom Authentifizierungsanbieter zurückkehren. Fügen Sie Eigenschaften, die mit dem Namen "FullName" und den Link unten hervorgehoben.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample4.cs?highlight=9-13)]

Fügen Sie auch eine neue Klasse namens ExtraUserInformation in AccountModels.cs, hinzu. Diese Klasse stellt die neue Tabelle, die in der Datenbank erstellt wird.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample5.cs)]

Fügen Sie in der Klasse UsersContext des hervorgehobenen Codes unten, um eine "DbSet"-Eigenschaft für die neue Klasse zu erstellen.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample6.cs?highlight=9)]

Abrufen von Zugriffstoken Die meisten externen Anbieter übergeben ein Zugriffstoken für den wieder, nachdem die Anmeldeinformationen des Benutzers überprüft werden.

1. Dieses Zugriffstoken ist sehr wichtig, weil dadurch, dass Sie zum Aufrufen von Vorgängen, die nur für authentifizierte Benutzer verfügbar sind.**
2. Führen Sie den Befehl **Datenbank aktualisieren**

Aus diesem Grund unbedingt abrufen und speichern das Zugriffstoken, wenn Sie weitere Funktionen bereitstellen möchten.

## <a name="retrieve-the-additional-data"></a>Abhängig von der externe Anbieter kann das Zugriffstoken für nur einen begrenzten Zeitraum gültig sein.

Um sicherzustellen, dass Sie ein gültiges Zugangstoken verfügen, werden Sie abrufen, es jedes Mal, die der Benutzer anmeldet, und speichern ihn als einen Sitzungswert statt in einer Datenbank speichern. In der ExternalLoginCallback -Methode, das Zugriffstoken auch wieder in übergeben die ExtraData Eigenschaft der AuthenticationResult Objekt. Fügen Sie den hervorgehobenen Code ExternalLoginCallback , speichern Sie das Zugriffstoken in der Sitzung Objekt. Dieser Code ausgeführt wird, jedes Mal, wenn der Benutzer mit einer Facebook-Konto anmeldet. Obwohl in diesem Beispiel ein Zugriffstoken von Facebook abgerufen werden, können Sie das Zugriffstoken von einem externen Anbieter abrufen, über den gleichen Schlüssel, die mit dem Namen Accesstoken. Der Standardwert Abmeldung -Methode meldet den Benutzer von Ihrer Anwendung, aber den Benutzer bei der externe Anbieter werden nicht protokolliert.

Um den Benutzer bei Facebook anmelden und zu verhindern, dass das Token beibehalten, nachdem der Benutzer abgemeldet hat, Sie können den folgenden hervorgehobenen Code zum Hinzufügen der **Abmeldung** -Methode in der AccountController-Komponente.

Der Wert in der  Parameter ist die URL verwenden, nachdem der Benutzer aus Facebook angemeldet hat. Wenn Sie auf dem lokalen Computer testen möchten, würden Sie die richtige Portnummer für den lokalen Standort bereitstellen. In einer produktionsumgebung würden Sie eine Standardseite, z. B. "contoso.com" angeben. Abrufen von Benutzerinformationen, mit denen das Zugriffstoken ist erforderlich.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample7.cs)]

Nun, dass Sie das Zugriffstoken gespeichert und das Paket C# Facebook-SDK installiert, können diese zusammen Sie zusätzliche Benutzerinformationen von Facebook angefordert. In der **ExternalLoginConfirmation** -Methode, erstellen Sie eine Instanz von der FacebookClient Klasse übergibt den Wert des Zugriffstokens.

- ID
- Name
- aus
- Den Wert der Anforderung der überprüft -Eigenschaft für den aktuellen authentifizierten Benutzer.
- Die überprüft Eigenschaft gibt an, ob es sich bei Facebook, das Konto über andere Weise, wie z. B. das Senden einer Nachricht an ein Mobiltelefon überprüft wurde.

Speichern Sie diesen Wert in der Datenbank.

Sie müssen es löschen Sie die Datensätze in der Datenbank für den Benutzer oder ein anderes Facebook-Konto verwenden. Führen Sie die Anwendung, und registrieren Sie den neuen Benutzer. Sehen Sie sich die ExtraUserInformation Tabelle, um den Wert für die überprüfte Eigenschaft anzuzeigen.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample8.cs)]

In diesem Tutorial haben Sie eine Website, die in Facebook, für die Benutzerauthentifizierung und Registrierungsdaten integriert ist erstellt.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample9.cs)]

Sie haben über die das Standardverhalten, das für MVC 4-Webanwendung, und wie Sie das Standardverhalten anpassen eingerichtet ist.

In der **ExternalLoginConfirmation** -Methode, ändern Sie den Code wie unten, um die zusätzliche Benutzerinformationen zu speichern.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample10.cs?highlight=4,7-13)]

## <a name="adjusting-the-view"></a>Anpassen der Ansicht

Die zusätzliche Benutzerdaten, die Sie vom Anbieter abrufen werden auf der Registrierungsseite angezeigt.

In der **Ansichten**/**Konto** Ordner **ExternalLoginConfirmation.cshtml**. Fügen Sie unterhalb des vorhandenen Felds für den Benutzernamen an für die "FullName", Link und PictureLink Felder hinzu.

[!code-cshtml[Main](using-oauth-providers-with-mvc/samples/sample11.cshtml)]

Sie können nun fast bereit, die Anwendung auszuführen, und registrieren einen neuen Benutzer mit den zusätzlichen Informationen gespeichert. Sie müssen ein Konto verfügen, die mit dem Standort noch nicht registriert ist. Sie können entweder verwenden Sie ein Konto anderen Test oder Löschen von Zeilen in der **UserProfile** und **Webseiten\_OAuthMembership** Tabellen für das Konto, die Sie wiederverwenden möchten. Diese Zeilen löschen, werden Sie sicherstellen, dass das Konto erneut registriert wird.

Führen Sie die Anwendung, und registrieren Sie den neuen Benutzer. Beachten Sie, dass dieses Mal die Seite "Bestätigung" mehrere Werte enthält.

![register](using-oauth-providers-with-mvc/_static/image12.png)

Schließen Sie nach Abschluss der Registrierung den Browser. Suchen Sie in der Datenbank, beachten Sie, dass die neuen Werte in der **ExtraUserInformation** Tabelle.

## <a name="install-nuget-package-for-facebook-api"></a>Installieren von NuGet-Paket für die Facebook-API

Facebook bietet einen [API](https://developers.facebook.com/docs/reference/apis/) können Sie aufrufen, um Vorgänge auszuführen. Sie können die Facebook-API aufrufen, Senden von HTTP-Anforderungen weitergeleitet, oder mit der Installation eines NuGet-Pakets, das ermöglicht, diese Anforderungen zu senden. In diesem Tutorial wird mithilfe eines NuGet-Pakets dargestellt, aber Installieren von NuGet-Paket ist nicht wichtig. Dieses Tutorial zeigt, wie das C# Facebook-SDK-Paket verwenden wird. Es gibt andere NuGet-Pakete, die beim Aufrufen der Facebook-API unterstützen.

Von der **NuGet-Pakete verwalten** Windows, wählen Sie das Facebook-C# SDK-Paket.

![Paket installieren](using-oauth-providers-with-mvc/_static/image13.png)

Sie werden das Facebook C# SDK verwenden, um einen Vorgang aufrufen, der das Zugriffstoken für den Benutzer erforderlich sind. Im nächsten Abschnitt erfahren, wie Sie das Zugriffstoken abzurufen.

## <a name="retrieve-access-token"></a>Abrufen von Zugriffstoken

Die meisten externen Anbieter übergeben ein Zugriffstoken für den wieder, nachdem die Anmeldeinformationen des Benutzers überprüft werden. Dieses Zugriffstoken ist sehr wichtig, weil dadurch, dass Sie zum Aufrufen von Vorgängen, die nur für authentifizierte Benutzer verfügbar sind. Aus diesem Grund unbedingt abrufen und speichern das Zugriffstoken, wenn Sie weitere Funktionen bereitstellen möchten.

Abhängig von der externe Anbieter kann das Zugriffstoken für nur einen begrenzten Zeitraum gültig sein. Um sicherzustellen, dass Sie ein gültiges Zugangstoken verfügen, werden Sie abrufen, es jedes Mal, die der Benutzer anmeldet, und speichern ihn als einen Sitzungswert statt in einer Datenbank speichern.

In der **ExternalLoginCallback** -Methode, das Zugriffstoken auch wieder in übergeben die **ExtraData** Eigenschaft der **AuthenticationResult** Objekt. Fügen Sie den hervorgehobenen Code **ExternalLoginCallback** , speichern Sie das Zugriffstoken in der **Sitzung** Objekt. Dieser Code ausgeführt wird, jedes Mal, wenn der Benutzer mit einer Facebook-Konto anmeldet.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample12.cs?highlight=11-14)]

Obwohl in diesem Beispiel ein Zugriffstoken von Facebook abgerufen werden, können Sie das Zugriffstoken von einem externen Anbieter abrufen, über den gleichen Schlüssel, die mit dem Namen &quot;Accesstoken&quot;.

## <a name="logging-off"></a>Abmelden

Der Standardwert **Abmeldung** -Methode meldet den Benutzer von Ihrer Anwendung, aber den Benutzer bei der externe Anbieter werden nicht protokolliert. Um den Benutzer bei Facebook anmelden und zu verhindern, dass das Token beibehalten, nachdem der Benutzer abgemeldet hat, Sie können den folgenden hervorgehobenen Code zum Hinzufügen der **Abmeldung** -Methode in der AccountController-Komponente.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample13.cs?highlight=6-14)]

Der Wert in der `next` Parameter ist die URL verwenden, nachdem der Benutzer aus Facebook angemeldet hat. Wenn Sie auf dem lokalen Computer testen möchten, würden Sie die richtige Portnummer für den lokalen Standort bereitstellen. In einer produktionsumgebung würden Sie eine Standardseite, z. B. "contoso.com" angeben.

## <a name="retrieve-user-information-that-requires-the-access-token"></a>Abrufen von Benutzerinformationen, mit denen das Zugriffstoken ist erforderlich.

Nun, dass Sie das Zugriffstoken gespeichert und das Paket C# Facebook-SDK installiert, können diese zusammen Sie zusätzliche Benutzerinformationen von Facebook angefordert. In der **ExternalLoginConfirmation** -Methode, erstellen Sie eine Instanz von der **FacebookClient** Klasse übergibt den Wert des Zugriffstokens. Den Wert der Anforderung der **überprüft** -Eigenschaft für den aktuellen authentifizierten Benutzer. Die **überprüft** Eigenschaft gibt an, ob es sich bei Facebook, das Konto über andere Weise, wie z. B. das Senden einer Nachricht an ein Mobiltelefon überprüft wurde. Speichern Sie diesen Wert in der Datenbank.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample14.cs?highlight=7-18,25)]

Sie müssen es löschen Sie die Datensätze in der Datenbank für den Benutzer oder ein anderes Facebook-Konto verwenden.

Führen Sie die Anwendung, und registrieren Sie den neuen Benutzer. Sehen Sie sich die **ExtraUserInformation** Tabelle, um den Wert für die überprüfte Eigenschaft anzuzeigen.

## <a name="conclusion"></a>Schlussbemerkung

In diesem Tutorial haben Sie eine Website, die in Facebook, für die Benutzerauthentifizierung und Registrierungsdaten integriert ist erstellt. Sie haben über die das Standardverhalten, das für MVC 4-Webanwendung, und wie Sie das Standardverhalten anpassen eingerichtet ist.

## <a name="related-topics"></a>Verwandte Themen

- [Erstellen einer ASP.NET MVC-app mit Authentifizierung und SQL-Datenbank und Bereitstellen in Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)
