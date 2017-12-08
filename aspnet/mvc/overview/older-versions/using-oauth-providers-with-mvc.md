---
uid: mvc/overview/older-versions/using-oauth-providers-with-mvc
title: Verwenden von OAuth-Anbieter mit MVC 4 | Microsoft Docs
author: tfitzmac
description: "In diesem Lernprogramm wird gezeigt, wie eine ASP.NET MVC 4-Webanwendung zu erstellen, die Benutzern ermöglicht, melden Sie sich mit den Anmeldeinformationen von einem externen Anbieter, wie z. B. Facebo..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/19/2013
ms.topic: article
ms.assetid: 7a87f16f-0e19-4f15-a88a-094ae866c4a2
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/using-oauth-providers-with-mvc
msc.type: authoredcontent
ms.openlocfilehash: 965d2e740cc76838b1b4e1c618a2a6d784672fcc
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="using-oauth-providers-with-mvc-4"></a>Verwenden von OAuth-Anbieter mit MVC 4
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> Diesem Lernprogramm erfahren Sie, wie Sie eine ASP.NET MVC 4-Webanwendung zu erstellen, die ermöglicht es Benutzern, melden Sie sich mit den Anmeldeinformationen von einem externen Anbieter, wie z. B. Facebook, Twitter, Microsoft oder Google, und integrieren einige der Funktionen von diesen Anbietern in Ihrem Webanwendung. Der Einfachheit halber konzentriert sich dieses Lernprogramms zum Arbeiten mit den Anmeldeinformationen von Facebook.
> 
> Externe Anmeldeinformationen in einer ASP.NET MVC 5-Webanwendung verwenden möchten, finden Sie unter [erstellen Sie eine ASP.NET MVC 5-App mit Facebook und Google OAuth2 und OpenID-Sign-on](../security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
> 
> Aktivieren diese Anmeldeinformationen auf Ihre Websites bietet einen erheblichen Vorteil Schreibberechtigung Millionen Benutzer bereits Konten mit diesen externen Anbietern. Diese Benutzer möglicherweise eher für Ihre Website registrieren können, wenn sie nicht zum Erstellen und speichern einen neuen Satz von Anmeldeinformationen verfügen. Nachdem ein Benutzer über einen dieser Anbieter angemeldet hat, können Sie auch, soziale Vorgänge vom Anbieter integrieren.


## <a name="what-youll-build"></a>Was müssen Sie erstellen

Dieses Lernprogramm umfasst zwei Hauptziele:

1. Aktivieren Sie einen Benutzer, sich mit den Anmeldeinformationen von einem OAuth-Anbieter anzumelden.
2. Abgerufen Sie Kontoinformationen vom Anbieter, und integrieren Sie diese Informationen mit der kontoregistrierung für Ihre Website.

Obwohl in den Beispielen in diesem Lernprogramm zur Verwendung von Facebook als Authentifizierungsanbieter zu konzentrieren, können Sie den Code zur Verwendung eines der Anbieter ändern. Die Schritte zur Implementierung von beliebigen Anbietern sind sehr ähnlich, mit den Schritten, die Sie in diesem Lernprogramm sehen werden. Beachten Sie nur signifikante Unterschiede bei der Anbieter-API direkt aufrufen, festgelegt.

## <a name="prerequisites"></a>Erforderliche Komponenten

- [Microsoft Visual Studio 2012](https://www.microsoft.com/visualstudio/eng/downloads#vs) oder [Microsoft Visual Studio Express 2012 für Web](https://www.microsoft.com/visualstudio/eng/downloads#d-2012-express)

Oder

- Microsoft Visual Studio 2010 SP1 oder [Visual Web Developer Express 2010 SP1](https://www.microsoft.com/visualstudio/eng/downloads#d-2010-express)
- [ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392)

In diesem Thema wird außerdem davon ausgegangen, dass Sie über grundlegende Kenntnisse über ASP.NET MVC und Visual Studio verfügen. Wenn Sie eine Einführung in ASP.NET MVC 4 benötigen, finden Sie unter [Einführung in ASP.NET MVC 4](getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md).

## <a name="create-the-project"></a>Erstellen eines Projekts

In Visual Studio, erstellen Sie eine neue ASP.NET MVC 4-Webanwendung, und nennen Sie sie &quot;OAuthMVC&quot;. Sie können .NET Framework 4.5 oder 4 abzielen.

![Projekt erstellen](using-oauth-providers-with-mvc/_static/image1.png)

Wählen Sie im Fenster Neues ASP.NET MVC 4-Projekt **Internetanwendung** und belassen Sie **Razor** als Ansichtsmodul.

![Wählen Sie die Internet-Anwendung](using-oauth-providers-with-mvc/_static/image2.png)

## <a name="enable-a-provider"></a>Aktivieren Sie einen Anbieter

Wenn Sie eine MVC 4-Webanwendung mit dem Internet Application-Vorlage erstellen, wird das Projekt erstellt, mit einer Datei namens AuthConfig.cs in der App\_Startordner.

![AuthConfig-Datei](using-oauth-providers-with-mvc/_static/image3.png)

Die AuthConfig-Datei enthält Code zum Registrieren von Clients für die externe Authentifizierungsanbieter. Standardmäßig ist dieser Code, auskommentiert daher keine externen Anbietern nicht aktiviert werden.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample1.cs)]

Sie müssen diesen Code, um externe-Authentifizierungsclient kommentieren. Kommentieren Sie Sie nur die Anbieter, die Sie an Ihrem Standort einschließen möchten. Für dieses Lernprogramm können Sie nur die Facebook-Anmeldeinformationen.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample2.cs)]

Beachten Sie im obigen Beispiel, dass die Methode für die Registrierungsparameter leere Zeichenfolgen enthält. Wenn Sie versuchen, die Anwendung jetzt ausführen, löst die Anwendung eine Ausnahme für das Argument aus, da leere Zeichenfolgen für die Parameter sind nicht zulässig. Um gültige Werte bereitzustellen, müssen Sie Ihre Website mit den externen Anbietern registrieren, wie im nächsten Abschnitt gezeigt.

## <a name="registering-with-an-external-provider"></a>Mit einem externen Anbieter registrieren

Um Benutzer mit Anmeldeinformationen von einem externen Anbieter zu authentifizieren, müssen Sie Ihre Website mit dem Anbieter registrieren. Wenn Sie Ihre Website registrieren, erhalten Sie Parameter (z. B. Schlüssel oder -Id und Schlüssel), um beim Registrieren des Clients enthalten. Sie müssen ein Konto mit den Anbietern verfügen, die Sie verwenden möchten.

In diesem Lernprogramm werden nicht alle Schritte angezeigt, die Sie ausführen müssen, um diese Anbieter zu registrieren. Die Schritte sind nicht in der Regel schwierig. Befolgen Sie die Anweisungen, die auf diesen Standorten, um Ihre Website erfolgreich zu registrieren. Zum Einstieg in die Registrierung einer Website, finden Sie in der Developer-Website:

- [Facebook](https://developers.facebook.com/)
- [Google](https://developers.google.com/)
- [Microsoft](http://manage.dev.live.com/)
- [Twitter](https://dev.twitter.com/)

Wenn Sie Ihre Website mit Facebook zu registrieren, bieten Sie &quot;"localhost"&quot; für die Domäne der Website und `&quot;http://localhost/&quot;` für die URL, wie in der folgenden Abbildung dargestellt. Verwenden "localhost" mit den meisten Anbietern funktioniert jedoch derzeit funktioniert nicht mit dem Microsoft-Anbieter. Für den Microsoft-Anbieter müssen Sie eine gültige Website-URL einschließen.

![Website registrieren](using-oauth-providers-with-mvc/_static/image4.png)

In der vorherigen Abbildung wurden die Werte für die app-Id, die app-Geheimnis und die Kontakt-e-Mail entfernt. Wenn Sie Ihre Website wirklich registrieren, werden diese Werte vorhanden sein. Sie sollten die Werte für app-Id und den geheimen beachten, da Sie sie der Anwendung hinzugefügt werden.

## <a name="creating-test-users"></a>Erstellen Testbenutzer

Wenn Sie mit einem vorhandenen Facebook-Konto so testen Sie Ihre Website nichts ausmacht, können Sie diesen Abschnitt überspringen.

Sie können problemlos Testbenutzer für Ihre Anwendung in der Verwaltungsseite des Facebook-app erstellen. Sie können diese Testkonten auf Ihrer Website anmelden. Erstellen Sie Testbenutzer, indem Sie auf die **Rollen** Link im linken Navigationsbereich und anschließend auf die **erstellen** Link.

![Erstellen Sie Testbenutzer](using-oauth-providers-with-mvc/_static/image5.png)

Die Facebook-Website erstellt automatisch die Anzahl der Testkonten, die Sie anfordern.

## <a name="adding-application-id-and-secret-from-the-provider"></a>Anwendungs-Id und den geheimen Schlüssel vom Anbieter hinzufügen

Nun, dass Sie die Id und den geheimen von Facebook erhalten haben, wechseln Sie zurück zur Datei AuthConfig, und fügen Sie sie als Parameterwerte. Die unten aufgeführten Werte sind keine echte Werte.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample3.cs)]

## <a name="log-in-with-external-credentials"></a>Melden Sie sich mit externen Anmeldeinformationen

Das ist alles tun, damit die externe Anmeldeinformationen in Ihre Website aktiviert haben. Führen Sie die Anwendung, und klicken Sie auf den Anmeldelink in der oberen rechten Ecke. Die Vorlage erkennt automatisch, dass Sie Facebook als Dienstanbieter registriert haben, und eine Schaltfläche für den Anbieter enthält. Wenn Sie mehrere Anbieter registrieren, ist eine Schaltfläche für jeden dieser automatisch mit eingeschlossen.

![externe Anmeldung](using-oauth-providers-with-mvc/_static/image6.png)

Gewusst wie: Anpassen des Protokolls in Schaltflächen für die externe Anbieter behandelt in diesem Lernprogramm nicht. Diese Informationen finden Sie unter [die Anmeldebenutzeroberfläche anpassen, bei Verwendung der OAuth-/OpenID](https://blogs.msdn.com/b/pranav_rastogi/archive/2012/08/24/customizing-the-login-ui-when-using-oauth-openid.aspx).

Klicken Sie auf die Facebook-Schaltfläche, um sich mit Facebook Anmeldeinformationen anzumelden. Wenn Sie einen externen Anbieter auswählen, werden Sie an diesem Standort umgeleitet und vom Dienst zum Anmelden aufgefordert.

Die folgende Abbildung zeigt die Anmeldeseite für Facebook. Er notiert sich, dass Sie zum Anmelden bei einer Website namens Oauthmvcexample Ihrem Facebook-Konto verwenden.

![Facebook-Authentifizierung](using-oauth-providers-with-mvc/_static/image7.png)

Nach der Anmeldung mit Facebook-Anmeldeinformationen informiert eine Seite dem Benutzer, dass der Standort auf grundlegende Informationen zugreifen kann.

![Anfrageberechtigungen](using-oauth-providers-with-mvc/_static/image8.png)

Nach der Auswahl **wechseln Sie zur App**, muss der Benutzer für den Standort registrieren. Die folgende Abbildung zeigt die Seite "Registrierung" aus, nachdem ein Benutzer mit Facebook-Anmeldeinformationen angemeldet hat. Der Benutzername ist bereits in der Regel mit einem Namen vom Anbieter ausgefüllt.

![register](using-oauth-providers-with-mvc/_static/image9.png)

Klicken Sie auf **registrieren** um die Registrierung abzuschließen. Schließen Sie den Browser.

Sie sehen, dass das neue Konto der Datenbank hinzugefügt wurde. Öffnen Sie im Server-Explorer die **DefaultConnection** Datenbank, und öffnen Sie die **Tabellen** Ordner.

![Datenbanktabellen](using-oauth-providers-with-mvc/_static/image10.png)

Mit der rechten Maustaste die **UserProfile** Tabelle, und wählen Sie **Tabellendaten anzeigen**.

![Anzeigen von Daten](using-oauth-providers-with-mvc/_static/image11.png)

Das neue Konto wird angezeigt, das Sie hinzugefügt haben. Betrachten Sie die Daten in **Webseite\_OAuthMembership** Tabelle. Sie sehen, mehr Daten im Zusammenhang mit der externen Anbieter für das Konto, das Sie gerade hinzugefügt haben.

Wenn Sie externe Authentifizierung aktivieren möchten, ist der Vorgang abgeschlossen. Allerdings können Sie weitere Informationen vom Anbieter in der neuen Registrierung Benutzerprozess integrieren, wie in den folgenden Abschnitten gezeigt.

## <a name="create-models-for-additional-user-information"></a>Erstellen von Modellen für zusätzliche Benutzerinformationen

Wie Sie in den vorherigen Abschnitten bemerkt haben, müssen Sie nicht abgerufen werden zusätzlichen Informationen für die Registrierung integriertes Konto arbeiten. Die meisten externen Anbieter übergeben jedoch wieder zusätzliche Informationen über den Benutzer. Die folgenden Abschnitte zeigen, wie diese Informationen beibehalten, und speichern Sie sie in einer Datenbank. Insbesondere sind behalten Sie Werte für den Benutzer vollständigen Namen, den URI des Benutzers persönliche Webseite und einen Wert, der angibt, ob Facebook das Konto überprüft wurde.

Verwenden Sie [Code First-Migrationen](https://msdn.microsoft.com/en-us/data/jj591621) zum Hinzufügen einer Tabelle zum Speichern von zusätzliche Benutzerinformationen. Sie werden in der Tabelle zu einer vorhandenen Datenbank hinzugefügt, daher Sie zunächst eine Momentaufnahme der aktuellen Datenbank zu erstellen müssen. Erstellen Sie eine Momentaufnahme der aktuellen Datenbank, können Sie später eine Migration erstellen, die nur für die neue Tabelle enthält. So erstellen Sie eine Momentaufnahme der aktuellen Datenbank

1. Öffnen der **Paket-Manager-Konsole**
2. Führen Sie den Befehl **Enable-Migrationen**
3. Führen Sie den Befehl **hinzufügen Migration anfängliche – IgnoreChanges**
4. Führen Sie den Befehl **Datenbank aktualisieren**

Fügen Sie nun die neuen Eigenschaften. Öffnen Sie im Ordner "Modelle" die Datei AccountModels.cs, und suchen Sie die RegisterExternalLoginModel-Klasse. Die Klasse RegisterExternalLoginModel enthält Werte, die aus den Authentifizierungsanbieter zurückkehren. Fügen Sie die Eigenschaften "FullName" und "Link, wie unten markiert.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample4.cs?highlight=9-13)]

Fügen Sie auch eine neue Klasse namens ExtraUserInformation in AccountModels.cs, hinzu. Diese Klasse stellt die neue Tabelle, die in der Datenbank erstellt werden soll.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample5.cs)]

Fügen Sie in der Klasse UsersContext des hervorgehobenen Codes unten, um ein ' DbSet '-Eigenschaft für die neue Klasse zu erstellen.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample6.cs?highlight=9)]

Sie sind jetzt bereit, um die neue Tabelle erstellen. Öffnen Sie die Paket-Manager-Konsole erneut, und dieses Mal:

1. Führen Sie den Befehl **AddExtraUserInformation hinzufügen-Migration**
2. Führen Sie den Befehl **Datenbank aktualisieren**

Die neue Tabelle ist jetzt in der Datenbank vorhanden.

## <a name="retrieve-the-additional-data"></a>Die zusätzlichen Daten abrufen

Es gibt zwei Möglichkeiten zum Abrufen von Benutzerdaten für zusätzliche. Die erste Möglichkeit besteht darin Benutzerdaten beizubehalten, die standardmäßig zurück, während die Authentifizierungsanforderung übergeben wird. Die zweite Möglichkeit besteht, insbesondere den Anbieter-API aufrufen und Weitere Informationen anfordern. Werte für FullName und Links werden automatisch wieder als Facebook übergeben. Ein Wert, der angibt, ob Facebook das Konto überprüft hat, wird durch einen Aufruf an die Facebook-API abgerufen. Zuerst, füllen Sie Werte für FullName und Link, und klicken Sie dann später, erhalten Sie den überprüften Wert.

Um die zusätzlichen Daten abzurufen, öffnen Sie die **AccountController.cs** in der Datei die **Controller** Ordner.

Diese Datei enthält die Logik für die Protokollierung, registrieren und Verwalten von Konten. Beachten Sie insbesondere die aufgerufenen Methoden **ExternalLoginCallback** und **ExternalLoginConfirmation**. In diesen Methoden können Sie Code, um externe Anmeldung Vorgänge für Ihre Anwendung anzupassen hinzufügen. Die erste Zeile der **ExternalLoginCallback** Methode enthält:

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample7.cs)]

Zusätzliche Benutzerdaten werden übergeben, wieder in die **ExtraData** Eigenschaft von der **AuthenticationResult** von zurückgegebene Objekt der **VerifyAuthentication** Methode. Die Facebook-Client enthält die folgenden Werte in der **ExtraData** Eigenschaft:

- ID
- Name
- link
- Geschlecht
- accesstoken

Andere Anbieter müssen ähnliche aber leicht abweichende Daten in der ExtraData-Eigenschaft.

Wenn der Benutzer auf Ihrer Website neu ist, Sie einige zusätzlichen Daten abrufen und diese Daten auf die Bestätigung Ansicht zu übergeben. Der letzte Block von Code in der Methode ausgeführt wird, nur, wenn der Benutzer noch nicht mit Ihrer Website ist. Ersetzen Sie die folgende Zeile:

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample8.cs)]

Durch diese Zeile:

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample9.cs)]

Diese Änderung umfasst lediglich die Werte für Eigenschaften FullName und Link.

In der **ExternalLoginConfirmation** -Methode, ändern Sie den Code als markierte unten, um die zusätzliche Benutzerinformationen zu speichern.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample10.cs?highlight=4,7-13)]

## <a name="adjusting-the-view"></a>Anpassen der Ansicht

Die zusätzlichen Daten, die Sie vom Anbieter abzurufen, werden in die Seite "Registrierung" angezeigt.

In der **Ansichten**/**Konto** Ordner **ExternalLoginConfirmation.cshtml**. Unterhalb des vorhandenen Felds für den Benutzernamen Hinzufügen von Feldern für FullName, Link und PictureLink.

[!code-cshtml[Main](using-oauth-providers-with-mvc/samples/sample11.cshtml)]

Sie können nun fast führen Sie die Anwendung, und registrieren einen neuen Benutzer mit zusätzlichen Informationen gespeichert. Sie müssen ein Konto verfügen, die mit dem Standort noch nicht registriert ist. Sie können entweder einen anderen Test-Konto verwenden oder Löschen von Zeilen in der **UserProfile** und **Webseiten\_OAuthMembership** Tabellen für das Konto, die Sie wiederverwenden möchten. Durch diese Zeilen löschen, werden Sie sicherstellen, dass das Konto erneut registriert wird.

Führen Sie die Anwendung, und registrieren Sie den neuen Benutzer. Beachten Sie, dass dieses Mal die Seite "Bestätigung" Weitere Werte enthält.

![register](using-oauth-providers-with-mvc/_static/image12.png)

Schließen Sie nach Abschluss der Registrierung den Browser. Suchen Sie in der Datenbank, beachten Sie die neuen Werte in der **ExtraUserInformation** Tabelle.

## <a name="install-nuget-package-for-facebook-api"></a>Installieren von NuGet-Paket für die Facebook-API

Facebook bietet eine [API](https://developers.facebook.com/docs/reference/apis/) , dass Sie aufrufen können, um Vorgänge auszuführen. Sie können die Facebook-API aufrufen, indem sendende HTTP-Anforderungen weitergeleitet oder mit der Installation von NuGet-Paket, das die vereinfacht diese Anforderungen zu senden. Mithilfe von NuGet-Paket in diesem Lernprogramm gezeigt wird, aber installieren NuGet-Paket ist nicht unbedingt erforderlich. Dieses Lernprogramm zeigt, wie das Facebook C# SDK-Paket. Es gibt andere NuGet-Pakete, die Ihnen dabei helfen zu Aufrufen der Facebook-API.

Aus der **NuGet-Pakete verwalten** Windows, wählen Sie das Facebook C# SDK-Paket.

![Paket installieren](using-oauth-providers-with-mvc/_static/image13.png)

Sie verwenden das Facebook C# SDK einen Vorgang aufrufen, der das Zugriffstoken für den Benutzer erforderlich sind. Im nächste Abschnitt wird gezeigt, wie das Zugriffstoken abgerufen wird.

## <a name="retrieve-access-token"></a>Abrufen von Zugriffstoken

Die meisten externen Anbieter geben ein Zugriffstoken zurück, nachdem die Anmeldeinformationen des Benutzers überprüft werden. Das Zugriffstoken ist sehr wichtig, weil dadurch, dass Sie zum Aufrufen von Vorgängen, die nur für authentifizierte Benutzer verfügbar sind. Daher unbedingt abrufen und das Speichern des Zugriffstokens, wenn Sie weitere Funktionen bereitstellen möchten.

Abhängig von den externen Anbieter kann das Zugriffstoken für einen begrenzten Zeitraum gültig sein. Um sicherzustellen, dass Sie einen gültigen Zugang-Token verfügen, werden Sie durch Aufrufen jedes Mal der Benutzer anmeldet, und speichern Sie sie als einen Sitzungswert anstatt mit einer Datenbank zu speichern.

In der **ExternalLoginCallback** -Methode, das Zugriffstoken auch wieder übergeben der **ExtraData** Eigenschaft von der **AuthenticationResult** Objekt. Hinzufügen den hervorgehobenen Code zum **ExternalLoginCallback** , speichern Sie das Zugriffstoken in der **Sitzung** Objekt. Jedes Mal, wenn der Benutzer mit einer Facebook-Konto anmeldet, wird dieser Code ausgeführt.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample12.cs?highlight=11-14)]

Obwohl in diesem Beispiel wird ein Zugriffstoken von Facebook abgerufen wird, können Sie das Zugriffstoken von einem externen Anbieter abzurufen, über den gleichen Schlüssel mit dem Namen &quot;Accesstoken&quot;.

## <a name="logging-off"></a>Abmelden

Die Standardeinstellung **Abmelden** Methode meldet den Benutzer aus der Anwendung, aber den Benutzer aus der externen Anbieter werden nicht protokolliert. Zum Anmelden des Benutzers aus Facebook und verhindern, dass das Token beibehalten, nachdem der Benutzer abgemeldet hat, können Sie die folgenden hervorgehobenen Code zum Hinzufügen der **Abmelden** Methode in der AccountController.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample13.cs?highlight=6-14)]

Der Wert, der Sie, in angeben der `next` Parameter ist die URL zu verwenden, nachdem der Benutzer außerhalb des Facebook angemeldet hat. Wenn auf dem lokalen Computer testen, würden Sie die richtige Portnummer für den lokalen Standort bereitstellen. In der Produktion würden Sie eine Standardseite, z. B. "contoso.com" angeben.

## <a name="retrieve-user-information-that-requires-the-access-token"></a>Abrufen von Benutzerinformationen, die das Zugriffstoken ist erforderlich

Jetzt, Sie haben das Zugriffstoken gespeichert und das Facebook C# SDK-Paket installiert, können Sie diese zusammen verwenden, um zusätzliche Benutzerinformationen von Facebook anzufordern. In der **ExternalLoginConfirmation** -Methode, erstellen Sie eine Instanz von der **FacebookClient** Klasse durch Übergeben des Werts des Zugriffstokens. Den Wert der Anforderung der **überprüft** -Eigenschaft für den aktuellen authentifizierten Benutzer. Die **überprüft** Eigenschaft angibt, ob Facebook das Konto mithilfe anderer Methoden wie z. B. das Senden einer Nachricht an ein Mobiltelefon überprüft wurde. Speichern Sie dieser Wert wird in der Datenbank.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample14.cs?highlight=7-18,25)]

Sie müssen erneut löschen Sie die Datensätze in der Datenbank für den Benutzer oder ein anderes Facebook-Konto verwenden.

Führen Sie die Anwendung, und registrieren Sie den neuen Benutzer. Betrachten Sie die **ExtraUserInformation** Tabelle, um den Wert für die Eigenschaft überprüft anzuzeigen.

## <a name="conclusion"></a>Schlussfolgerung

In diesem Lernprogramm erstellt Sie eine Website, die für die Benutzerauthentifizierung und Registrierungsdaten mit Facebook integriert ist. Sie haben gelernt, über das Standardverhalten, das für MVC 4-Webanwendung sowie zum Anpassen dieses Standardverhalten eingerichtet ist.

## <a name="related-topics"></a>Verwandte Themen

- [Eine ASP.NET MVC-app mit Authentifizierung und SQL-Datenbank erstellen und Bereitstellen von Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)
