---
uid: web-api/overview/security/external-authentication-services
title: Externen Authentifizierungsdienste mit ASP.NET Web-API (c#) | Microsoft Docs
author: rmcmurray
description: Beschreibt die Verwendung von externen Authentifizierungsdienste in ASP.NET Web-API.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2013
ms.topic: article
ms.assetid: 3bb8eb15-b518-44f5-a67d-a27e051aedc6
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/external-authentication-services
msc.type: authoredcontent
ms.openlocfilehash: 406a85db7055910cb7a4e15fec8ef68dff5a19dc
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="external-authentication-services-with-aspnet-web-api-c"></a>Externen Authentifizierungsdienste mit ASP.NET Web-API (c#)
====================
durch [Robert McMurray](https://github.com/rmcmurray)

Visual Studio 2013 und ASP.NET 4.5.1 erweitern Sie die Sicherheitsoptionen für [Single Page Applications](../../../single-page-application/index.md) (SPA) und [Web-API](../../index.md) Services für die Integration von externen Authentifizierungsdienste, darunter mehrere OAuth-/OpenID und den Authentifizierungsdiensten für soziale Medien: Microsoft Accounts, Twitter, Facebook und Google.

### <a name="in-this-walkthrough"></a>In dieser exemplarischen Vorgehensweise

- [Verwenden von externen Authentifizierungsdienste](#USING)
- [Erstellen die Beispielwebanwendung](#SAMPLE)
- [Facebook-Authentifizierung aktivieren](#FACEBOOK)
- [Google-Authentifizierung aktivieren](#GOOGLE)
- [Microsoft-Authentifizierung aktivieren](#MICROSOFT)
- [Twitter-Authentifizierung aktivieren](#TWITTER)
- [Weitere Informationen](#MOREINFO)

    - [Kombinieren von externen Authentifizierungsdienste](#COMBINE)
    - [Konfigurieren von IIS Express verwendet einen vollständig qualifizierten Domänennamen](#FQDN)
    - [So erhalten Sie die Einstellungen Ihrer Anwendung für Microsoft-Authentifizierung](#OBTAIN)
    - [Optional: Lokale Registrierung deaktivieren](#DISABLE)

### <a name="prerequisites"></a>Erforderliche Komponenten

Um die Beispiele in dieser exemplarischen Vorgehensweise folgen zu können, müssen Sie über Folgendes verfügen:

- Visual Studio 2013
- Ein Konto für mindestens eine der folgenden externen Authentifizierungsdienste:

    - Ein Google-Benutzerkonto
    - Ein Entwicklerkonto mit dem Anwendungsbezeichner und den geheimen Schlüssel für eines der folgenden Dienste für soziale Authentifizierung:

        - Microsoft-Konten ([https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070))
        - Twitter ([https://dev.twitter.com/](https://dev.twitter.com/))
        - Facebook ([https://developers.facebook.com/](https://developers.facebook.com/))

<a id="USING"></a>
## <a name="using-external-authentication-services"></a>Verwenden von externen Authentifizierungsdienste

Die Fülle von externen Authentifizierungsdienste, die für Entwickler Webhilfe Entwicklung reduzieren aktuell verfügbar sind Zeit, wenn neue Webanwendungen erstellen. Webbenutzern in der Regel haben mehrere vorhandene Konten für beliebte Webdienste und Websites für soziale Medien, daher, wenn eine Web-Anwendung implementiert den Authentifizierungsdiensten aus einem externen Webdienst oder soziale Website sie die Entwicklungszeit, speichert würde angelangen wurde eine Implementierung für die Authentifizierung erstellen. Endbenutzer mit einem externen Authentifizierungsdienst gespeichert werden, müssen Sie ein anderes Konto für Ihre Webanwendung zu erstellen und zu einem anderen Benutzernamen und Kennwort merken müssen.

In der Vergangenheit mussten Entwickler zwei Optionen: Erstellen Sie eine eigene Implementierung der Authentifizierung, oder erfahren Sie, wie einen externer Authentifizierungsdienst in ihre Anwendungen integrieren. Auf der grundlegendsten Ebene veranschaulicht das folgende Diagramm ein einfachen anforderungsflusses für einen Benutzer-Agent (Webbrowser), der Informationen aus einer Webanwendung anfordert, die einen externen Authentifizierungsdienst konfiguriert ist:

[![](external-authentication-services/_static/image2.png "Klicken Sie hier, um das Bild zu erweitern.")](external-authentication-services/_static/image1.png)

Die Benutzer-Agent (oder einen Webbrowser in diesem Beispiel) sendet eine Anforderung im vorigen Diagramm um eine Web-Anwendung, mit die Webbrowser an einen externen Authentifizierungsdienst weitergeleitet. Der Benutzer-Agent sendet seine Anmeldeinformationen mit dem externen Authentifizierungsdienst, und wenn der Benutzer-Agent erfolgreich authentifiziert hat, der externen Authentifizierungsdienst weitergeleitet wird des Benutzer-Agents, die ursprünglichen Webanwendung eine Form der token, das die Benutzer-Agent sendet an die Webanwendung. Die Webanwendung wird das Token verwenden, um sicherzustellen, dass der Benutzer-Agent wurde erfolgreich vom externen Authentication-Dienst authentifiziert, und die Webanwendung das Token verwenden kann, um weitere Informationen über die Benutzer-Agent zu erfassen. Sobald die Anwendung erfolgt Verarbeitungsinformationen für den Benutzer-Agent, gibt die Webanwendung die richtige Reaktion auf die Benutzer-Agent basierend auf seiner autorisierungseinstellungen zurück.

In diesem zweiten Beispiel der Benutzer-Agent handelt, mit der Webanwendung und externen autorisierungsserver und die Webanwendung führt zusätzliche Kommunikation mit dem Server externe Autorisierung, um zusätzliche Informationen über den Benutzer abrufen -Agent:

[![](external-authentication-services/_static/image4.png "Klicken Sie hier, um das Bild zu erweitern.")](external-authentication-services/_static/image3.png)

Visual Studio 2013 und ASP.NET 4.5.1 vereinfachen Integration mit externen Authentifizierungsdienste für Entwickler durch integrierte Integration für die folgenden Authentifizierungsdienste bereitstellen:

- Facebook
- Google
- Microsoft-Accounts (Windows Live ID-Konten)
- Twitter

Die Beispiele in dieser exemplarischen Vorgehensweise zeigen, wie jede der unterstützten externen Authentifizierungsdienste mithilfe der neuen ASP.NET Web Application-Vorlage, die geliefert wird mit Visual Studio 2013 konfigurieren.

> [!NOTE]
> Ggf. müssen Sie möglicherweise Ihren FQDN ein an den Einstellungen für den Dienst für die externe Authentifizierung hinzufügen. Diese Anforderung basiert auf sicherheitseinschränkungen bei einigen Diensten externe Authentifizierung erforderlich ist den FQDN in den Anwendungseinstellungen Ihrer mit dem FQDN übereinstimmen, die von den Clients verwendet wird. (Die Schritte für diese werden für jeden Dienst externe Authentifizierung stark variieren, müssen Sie in der Dokumentation für jeden Dienst externe Authentifizierung, um festzustellen, ob dies erforderlich ist und wie Sie diese Einstellungen zu konfigurieren.) So konfigurieren Sie IIS Express zur Verwendung eines FQDNs für das Testen dieser Umgebung finden ggf. die [Konfigurieren von IIS Express verwenden Sie einen vollständig qualifizierten Domänennamen](#FQDN) Abschnitt weiter unten in dieser exemplarischen Vorgehensweise.


<a id="SAMPLE"></a>
## <a name="creating-a-sample-web-application"></a>Zum Erstellen einer Web-Beispielanwendung

Die folgenden Schritte führen Sie durch die zum Erstellen einer beispielanwendung mithilfe der ASP.NET Web Application-Vorlage, und verwenden Sie diese beispielanwendung für die externe Authentifizierung Dienste weiter unten in dieser exemplarischen Vorgehensweise.

Starten Sie Visual Studio 2013 wählen **neues Projekt** von der Startseite. Oder von der **Datei** klicken Sie im Menü **neu** und dann **Projekt**.

[![](external-authentication-services/_static/image6.png "Klicken Sie hier, um das Bild zu erweitern.")](external-authentication-services/_static/image5.png)

Wenn die **neues Projekt** im Dialogfeld angezeigt wird, wählen Sie **installiert** **Vorlagen** und erweitern Sie **Visual C#-**. Klicken Sie unter **Visual C#-** Option **Web**. Wählen Sie in der Liste der Projektvorlagen **ASP.NET-Webanwendung**. Geben Sie einen Namen für das Projekt, und klicken Sie auf **OK**.

[![](external-authentication-services/_static/image8.png "Klicken Sie hier, um das Bild zu erweitern.")](external-authentication-services/_static/image7.png)

Wenn die **neues ASP.NET-Projekt** gleichzeitig angezeigt werden, wählen Sie die **SPA** Vorlage, und klicken Sie auf **Projekt erstellen**.

[![](external-authentication-services/_static/image10.png "Klicken Sie hier, um das Bild zu erweitern.")](external-authentication-services/_static/image9.png)

Warten, wie Visual Studio 2013 erstellt das Projekt.

[![](external-authentication-services/_static/image12.png "Klicken Sie hier, um das Bild zu erweitern.")](external-authentication-services/_static/image11.png)

Wenn Visual Studio 2013 Erstellen eines Projekts abgeschlossen ist, öffnen Sie die *Startup.Auth.cs* in die Datei der **App\_starten** Ordner.

[![](external-authentication-services/_static/image14.png "Klicken Sie hier, um das Bild zu erweitern.")](external-authentication-services/_static/image13.png)

Wenn Sie das Projekt erstellen, sind keine externen Authentifizierungsdienste aktiviert *Startup.Auth.cs* Datei; die folgenden wird veranschaulicht, was der Code ähneln möglicherweise, mit, wo hervorgehobenen Abschnitte aktivieren Sie eine externe Authentifizierung-Dienst und alle relevanten Einstellungen, um Microsoft Accounts, Twitter, Facebook oder Google-Authentifizierung für Ihre ASP.NET-Anwendung zu verwenden:

[!code-csharp[Main](external-authentication-services/samples/sample1.cs)]

Wenn Sie F5 drücken, um Ihre Webanwendung Debuggen, werden Anmeldebildschirm angezeigt, wo Sie angezeigt werden, dass keine externen Authentifizierungsdienste definiert wurden.

[![](external-authentication-services/_static/image16.png "Klicken Sie hier, um das Bild zu erweitern.")](external-authentication-services/_static/image15.png)

In den folgenden Abschnitten erfahren Sie, wie der externen Authentifizierungsdienste aktivieren, die von ASP.NET in Visual Studio 2013 bereitgestellt werden.

<a id="FACEBOOK"></a>
## <a name="enabling-facebook-authentication"></a>Facebook-Authentifizierung aktivieren

Mithilfe von Facebook Authentifizierung erfordert, dass Sie eine Facebook-Entwicklerkonto erstellen und Ihr Projekt benötigen eine Anwendungs-ID und den geheimen Schlüssel von Facebook um zu funktionieren. Informationen zum Erstellen einer Facebook-Entwicklerkonto und Abrufen der Anwendungs-ID und geheimer Schlüssel finden Sie unter [ https://go.microsoft.com/fwlink/?LinkID=252166 ](https://go.microsoft.com/fwlink/?LinkID=252166).

Nach der Anwendungs-ID und geheimer Schlüssel abgerufen haben, verwenden die folgenden Schritte aus, um die Facebook-Authentifizierung für Ihre Webanwendung zu aktivieren:

1. Wenn Ihr Projekt in Visual Studio 2013 geöffnet ist, öffnen Sie die *Startup.Auth.cs* Datei:

    [![](external-authentication-services/_static/image18.png "Klicken Sie hier, um das Bild zu erweitern.")](external-authentication-services/_static/image17.png)
2. Suchen Sie den markierten Abschnitt des Codes:

    [!code-csharp[Main](external-authentication-services/samples/sample2.cs)]
3. Entfernen Sie die &quot; // &quot; zu kommentieren Sie die hervorgehobenen Codezeilen, und fügen Sie dann die Anwendungs-ID und geheimer Schlüssel Zeichen. Nachdem Sie diese Parameter hinzugefügt haben, können Sie das Projekt neu kompilieren:

    [!code-csharp[Main](external-authentication-services/samples/sample3.cs)]
4. Wenn Sie F5, um Ihre Webanwendung im Webbrowser öffnen drücken, sehen Sie sich, dass Facebook als externe Authentifizierungsdienst definiert wurde:

    [![](external-authentication-services/_static/image20.png "Klicken Sie hier, um das Bild zu erweitern.")](external-authentication-services/_static/image19.png)
5. Beim Klicken auf die **Facebook** Schaltfläche, Ihren Browser an die Facebook-Anmeldeseite umgeleitet werden:

    [![](external-authentication-services/_static/image22.png "Klicken Sie hier, um das Bild zu erweitern.")](external-authentication-services/_static/image21.png)
6. Nachdem Sie Ihre Facebook-Anmeldeinformationen eingeben, und klicken Sie auf **melden Sie sich**, Ihren Webbrowser wird zurück an Ihre Webanwendung umgeleitet werden Sie für die Eingabe der **Benutzername** , die Sie zuordnen möchten Ihre Facebook-Konto:

    [![](external-authentication-services/_static/image24.png "Klicken Sie hier, um das Bild zu erweitern.")](external-authentication-services/_static/image23.png)
7. Nachdem Sie Ihren Benutzernamen eingegeben und geklickt haben die **registrieren** Schaltfläche, Ihre Webanwendung zeigt die Standardeinstellung **Startseite** für Ihr Facebook-Konto:

    [![](external-authentication-services/_static/image26.png "Klicken Sie hier, um das Bild zu erweitern.")](external-authentication-services/_static/image25.png)

<a id="GOOGLE"></a>
## <a name="enabling-google-authentication"></a>Google-Authentifizierung aktivieren

Google ist weitem am besten zu aktivieren, denn es nicht, dass ein Entwicklerkonto erfordern, und zusätzliche Informationen wie die Anwendungs-ID oder den geheimen Schlüssel als die anderen externen Authentifizierungsdienste erfordert Dienste externe Authentifizierung erforderlich.

Verwenden Sie zum Aktivieren der Google-Authentifizierung für Ihre Webanwendung die folgenden Schritte aus:

1. Wenn Ihr Projekt in Visual Studio 2013 geöffnet ist, öffnen Sie die *Startup.Auth.cs* Datei:

    [![](external-authentication-services/_static/image28.png "Klicken Sie hier, um das Bild zu erweitern.")](external-authentication-services/_static/image27.png)
2. Suchen Sie den markierten Abschnitt des Codes:

    [!code-csharp[Main](external-authentication-services/samples/sample4.cs)]
3. Entfernen Sie die &quot; // &quot; Zeichen, kommentieren Sie die hervorgehobene Zeile des Codes, und kompilieren Sie das Projekt neu:

    [!code-csharp[Main](external-authentication-services/samples/sample5.cs)]
4. Wenn Sie F5, um Ihre Webanwendung im Webbrowser öffnen drücken, sehen Sie sich, dass Google als externe Authentifizierungsdienst definiert wurde:

    [![](external-authentication-services/_static/image30.png "Klicken Sie hier, um das Bild zu erweitern.")](external-authentication-services/_static/image29.png)
5. Beim Klicken auf die **Google** Schaltfläche, Ihren Browser an der Google-Anmeldeseite umgeleitet werden:

    [![](external-authentication-services/_static/image32.png "Klicken Sie hier, um das Bild zu erweitern.")](external-authentication-services/_static/image31.png)
6. Nachdem Sie Ihre Google-Anmeldeinformationen eingeben, und klicken Sie auf **anmelden**, Google werden Sie aufgefordert, überprüfen Sie, ob die Webanwendung auf Ihr Google-Konto zuzugreifen:

    [![](external-authentication-services/_static/image34.png "Klicken Sie hier, um das Bild zu erweitern.")](external-authentication-services/_static/image33.png)
7. Beim Klicken auf **Accept**, Ihren Webbrowser wird zurück an Ihre Webanwendung umgeleitet werden Sie für die Eingabe der **Benutzername** , die mit Ihrem Google-Konto zugeordnet werden soll:

    [![](external-authentication-services/_static/image36.png "Klicken Sie hier, um das Bild zu erweitern.")](external-authentication-services/_static/image35.png)
8. Nachdem Sie Ihren Benutzernamen eingegeben und geklickt haben die **registrieren** Schaltfläche, Ihre Webanwendung zeigt die Standardeinstellung **Startseite** für Ihr Google-Konto:

    [![](external-authentication-services/_static/image38.png "Klicken Sie hier, um das Bild zu erweitern.")](external-authentication-services/_static/image37.png)

<a id="MICROSOFT"></a>
## <a name="enabling-microsoft-authentication"></a>Microsoft-Authentifizierung aktivieren

Microsoft-Authentifizierung erfordert, dass Sie ein Entwicklerkonto erstellen, und erfordert eine Client-ID und geheimer Clientschlüssel ordnungsgemäßes funktionieren. Weitere Informationen zu einem Microsoft-Entwicklerkonto erstellen und Abrufen von Ihrem Client-ID und geheimer Clientschlüssel, finden Sie unter [ https://go.microsoft.com/fwlink/?LinkID=144070 ](https://go.microsoft.com/fwlink/?LinkID=144070).

Sobald Ihre consumerschlüssel und dem geheimen consumerschlüssel abgerufen wurden, gehen Sie folgendermaßen vor, um Microsoft-Authentifizierung für Ihre Webanwendung zu aktivieren:

1. Wenn Ihr Projekt in Visual Studio 2013 geöffnet ist, öffnen Sie die *Startup.Auth.cs* Datei:

    [![](external-authentication-services/_static/image40.png "Klicken Sie hier, um das Bild zu erweitern.")](external-authentication-services/_static/image39.png)
2. Suchen Sie den markierten Abschnitt des Codes:

    [!code-csharp[Main](external-authentication-services/samples/sample6.cs)]
3. Entfernen Sie die &quot; // &quot; zu kommentieren Sie die hervorgehobenen Codezeilen, und fügen Sie dann Ihre Client-ID und geheimer Clientschlüssel Zeichen. Nachdem Sie diese Parameter hinzugefügt haben, können Sie das Projekt neu kompilieren:

    [!code-csharp[Main](external-authentication-services/samples/sample7.cs)]
4. Wenn Sie F5, um Ihre Webanwendung im Webbrowser öffnen drücken, sehen Sie sich, dass Microsoft als externe Authentifizierungsdienst definiert wurde:

    [![](external-authentication-services/_static/image42.png "Klicken Sie hier, um das Bild zu erweitern.")](external-authentication-services/_static/image41.png)
5. Beim Klicken auf die **Microsoft** Schaltfläche, Ihren Browser an den Microsoft-Anmeldeseite umgeleitet werden:

    [![](external-authentication-services/_static/image44.png "Klicken Sie hier, um das Bild zu erweitern.")](external-authentication-services/_static/image43.png)
6. Nachdem Sie Ihre Microsoft-Anmeldeinformationen eingeben und auf **anmelden**, werden Sie aufgefordert, stellen Sie sicher, dass Ihre Webanwendung Zugriffsberechtigungen für Ihr Microsoft-Konto hat:

    [![](external-authentication-services/_static/image46.png "Klicken Sie hier, um das Bild zu erweitern.")](external-authentication-services/_static/image45.png)
7. Beim Klicken auf **Ja**, Ihren Webbrowser wird zurück an Ihre Webanwendung umgeleitet werden Sie für die Eingabe der **Benutzername** , die mit Ihrem Microsoft-Konto zugeordnet werden soll:

    [![](external-authentication-services/_static/image48.png "Klicken Sie hier, um das Bild zu erweitern.")](external-authentication-services/_static/image47.png)
8. Nachdem Sie Ihren Benutzernamen eingegeben und geklickt haben die **registrieren** Schaltfläche, Ihre Webanwendung zeigt die Standardeinstellung **Startseite** für Ihr Microsoft-Konto:

    [![](external-authentication-services/_static/image50.png "Klicken Sie hier, um das Bild zu erweitern.")](external-authentication-services/_static/image49.png)

<a id="TWITTER"></a>
## <a name="enabling-twitter-authentication"></a>Twitter-Authentifizierung aktivieren

Twitter-Authentifizierung erfordert, dass Sie ein Entwicklerkonto erstellen, und er erfordert ein consumerschlüssel und dem geheimen consumerschlüssel aus, um die Funktion. Informationen zu einem Twitter-Entwicklerkonto erstellen und Ihre consumerschlüssel und dem geheimen consumerschlüssel abrufen, finden Sie unter [ https://go.microsoft.com/fwlink/?LinkID=252166 ](https://go.microsoft.com/fwlink/?LinkID=252166).

Sobald Ihre consumerschlüssel und dem geheimen consumerschlüssel abgerufen wurden, verwenden die folgenden Schritte aus, um die Twitter-Authentifizierung für Ihre Webanwendung zu aktivieren:

1. Wenn Ihr Projekt in Visual Studio 2013 geöffnet ist, öffnen Sie die *Startup.Auth.cs* Datei:

    [![](external-authentication-services/_static/image52.png "Klicken Sie hier, um das Bild zu erweitern.")](external-authentication-services/_static/image51.png)
2. Suchen Sie den markierten Abschnitt des Codes:

    [!code-csharp[Main](external-authentication-services/samples/sample8.cs)]
3. Entfernen Sie die &quot; // &quot; zu kommentieren Sie die hervorgehobenen Codezeilen, und fügen Sie dann Ihre consumerschlüssel und dem geheimen consumerschlüssel Zeichen. Nachdem Sie diese Parameter hinzugefügt haben, können Sie das Projekt neu kompilieren:

    [!code-csharp[Main](external-authentication-services/samples/sample9.cs)]
4. Wenn Sie F5, um Ihre Webanwendung im Webbrowser öffnen drücken, sehen Sie sich, dass Twitter als externe Authentifizierungsdienst definiert wurde:

    [![](external-authentication-services/_static/image54.png "Klicken Sie hier, um das Bild zu erweitern.")](external-authentication-services/_static/image53.png)
5. Beim Klicken auf die **Twitter** Schaltfläche, Ihren Browser an der Twitter-Anmeldeseite umgeleitet werden:

    [![](external-authentication-services/_static/image56.png "Klicken Sie hier, um das Bild zu erweitern.")](external-authentication-services/_static/image55.png)
6. Nachdem Sie Ihre Twitter-Anmeldeinformationen eingeben, und klicken Sie auf **autorisieren app**, Ihren Webbrowser wird zurück an Ihre Webanwendung umgeleitet werden Sie für die Eingabe der **Benutzername** , die Sie zuordnen möchten Ihr Twitter-Konto:

    [![](external-authentication-services/_static/image58.png "Klicken Sie hier, um das Bild zu erweitern.")](external-authentication-services/_static/image57.png)
7. Nachdem Sie Ihren Benutzernamen eingegeben und geklickt haben die **registrieren** Schaltfläche, Ihre Webanwendung zeigt die Standardeinstellung **Startseite** für Ihr Twitter-Konto:

    [![](external-authentication-services/_static/image60.png "Klicken Sie hier, um das Bild zu erweitern.")](external-authentication-services/_static/image59.png)

<a id="MOREINFO"></a>
## <a name="additional-information"></a>Zusätzliche Informationen

Weitere Informationen zum Erstellen von Anwendungen, die OAuth- und OpenID verwenden, finden Sie unter den folgenden URLs:

- [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166)
- [https://go.microsoft.com/fwlink/?LinkID=243995](https://go.microsoft.com/fwlink/?LinkID=243995)

<a id="COMBINE"></a>
### <a name="combining-external-authentication-services"></a>Kombinieren von externen Authentifizierungsdienste

Höheres Maß an Flexibilität Sie können mehrere externen Authentifizierungsdienste definieren, zur gleichen Zeit - dadurch Ihre Web Anwendungsbenutzer ein Konto aus den aktivierter externen Authentifizierungsdienste verwenden:

[![](external-authentication-services/_static/image62.png "Klicken Sie hier, um das Bild zu erweitern.")](external-authentication-services/_static/image61.png)

<a id="FQDN"></a>
### <a name="configuring-iis-express-to-use-a-fully-qualified-domain-name"></a>Konfigurieren von IIS Express verwendet einen vollständig qualifizierten Domänennamen

Einige externe Authentifizierungsanbieter unterstützen keine Testen Ihrer Anwendung mithilfe einer HTTP-Adresse wie `http://localhost:port/`. Um dieses Problem zu umgehen, können Sie eine statische Zuordnung für den vollständig qualifizierten Domänennamen (FQDN) der HOSTS-Datei hinzufügen und konfigurieren die Optionen für Ihr Projekt in Visual Studio 2013, verwenden Sie den FQDN für das Testen/Debuggen. Verwenden Sie hierzu die folgenden Schritte aus:

- Fügen Sie einen statischen FQDN Zuordnen der HOSTS-Datei hinzu:

  1. Öffnen Sie ein Eingabeaufforderungsfenster mit erhöhten Rechten in Windows.
  2. Geben Sie folgenden Befehl ein:

      <kbd>notepad %WinDir%\system32\drivers\etc\hosts</kbd>
  3. Fügen Sie einen Eintrag wie folgt zur Hostdatei hinzu:

      <kbd>127.0.0.1 www.wingtiptoys.com</kbd>
  4. Speichern Sie und schließen Sie die Hostdatei.

- Konfigurieren von Visual Studio-Projekt zur Verwendung des FQDN:

  1. Wenn Ihr Projekt in Visual Studio 2013 geöffnet ist, klicken Sie auf die **Projekt** Menü, und wählen Sie dann die Eigenschaften des Projekts. Sie können z. B. auswählen **WebApplication1 Eigenschaften**.
  2. Wählen Sie die **Web** Registerkarte.
  3. Geben Sie Ihren FQDN für den <strong>Projekt Url</strong>. Geben Sie z. B. <kbd> <http://www.wingtiptoys.com> </kbd> war, die die FQDN-Zuordnung, die Sie der HOSTS-Datei hinzugefügt.

- Konfigurieren von IIS Express, um den vollqualifizierten Domänennamen für Ihre Anwendung verwenden:

    1. Öffnen Sie ein Eingabeaufforderungsfenster mit erhöhten Rechten in Windows.
    2. Geben Sie den folgenden Befehl in Ihrem Ordner "IIS Express" ändern:

        <kbd>cd /d &quot;%ProgramFiles%\IIS Express&quot;</kbd>
    3. Geben Sie den folgenden Befehl aus, um den FQDN für Ihre Anwendung hinzufügen an:

        <kbd>appcmd.exe set config -section:system.applicationHost/sites /+&quot;[name='WebApplication1'].bindings.[protocol='http',bindingInformation='*:80:www.wingtiptoys.com']&quot; /commit:apphost</kbd>

  Wobei **WebApplication1** ist der Name des Projekts und **BindingInformation** enthält die Portnummer und den vollqualifizierten Domänennamen, die Sie für die Tests verwenden möchten.

<a id="OBTAIN"></a>
### <a name="how-to-obtain-your-application-settings-for-microsoft-authentication"></a>So erhalten Sie die Einstellungen Ihrer Anwendung für Microsoft-Authentifizierung

Verknüpfen eine Anwendung für Windows Live für Microsoft Authentication ist ein einfacher Vorgang. Wenn Sie nicht bereits eine Anwendung für Windows Live verknüpft haben, können Sie die folgenden Schritte aus:

1. Navigieren Sie zu [ https://go.microsoft.com/fwlink/?LinkID=144070 ](https://go.microsoft.com/fwlink/?LinkID=144070) Geben Sie Ihren Microsoft-Kontonamen und das Kennwort ein, und klicken Sie auf **anmelden**:

    [![](external-authentication-services/_static/image64.png "Klicken Sie hier, um das Bild zu erweitern.")](external-authentication-services/_static/image63.png)
2. Geben Sie den Namen und die Sprache Ihrer Anwendung, wenn Sie aufgefordert werden, und klicken Sie dann auf **ich stimme**:

    [![](external-authentication-services/_static/image66.png "Klicken Sie hier, um das Bild zu erweitern.")](external-authentication-services/_static/image65.png)
3. Auf der **API-Einstellungen** Seite für Ihre Anwendung, geben Sie die umleitungsdomäne für Ihre Anwendung und die Kopie der **Client-ID** und **geheimen Clientschlüssel** für das Projekt, und klicken Sie dann Klicken Sie auf **speichern**:

    [![](external-authentication-services/_static/image68.png "Klicken Sie hier, um das Bild zu erweitern.")](external-authentication-services/_static/image67.png)

<a id="DISABLE"></a>
### <a name="optional-disable-local-registration"></a>Optional: Lokale Registrierung deaktivieren

Die aktuelle lokale Registrierung-Funktionalität von ASP.NET wird nicht verhindert, dass automatisierte Programme (Bots) Erstellen von Elementeigenschaften Konten; Mithilfe von Bot Prevention und Validierung-Technologie wie z. B. [CAPTCHA](../../../web-pages/overview/security/16-adding-security-and-membership.md). Aus diesem Grund sollten Sie die lokale Anmeldung Formular und die Registrierung der Link auf der Anmeldeseite entfernen. Öffnen Sie hierzu die  *\_Login.cshtml* Seite in Ihrem Projekt, und klicken Sie dann die Zeilen für den Bereich für die lokale Anmeldung und den Link auskommentieren. Ergebnisseite sollte wie im folgenden Codebeispiel aussehen:

[!code-html[Main](external-authentication-services/samples/sample10.html)]

Sobald im Bereich für die lokale Anmeldung und den Registrierungslink deaktiviert wurde, zeigt die Anmeldeseite nur die externe Authentifizierungsanbieter, die Sie aktiviert haben:

[![](external-authentication-services/_static/image70.png "Klicken Sie hier, um das Bild zu erweitern.")](external-authentication-services/_static/image69.png)
