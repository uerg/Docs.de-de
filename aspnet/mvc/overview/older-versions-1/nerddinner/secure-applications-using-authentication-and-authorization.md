---
uid: mvc/overview/older-versions-1/nerddinner/secure-applications-using-authentication-and-authorization
title: Sichern von Anwendungen mithilfe von Authentifizierung und Autorisierung | Microsoft Docs
author: microsoft
description: Schritt 9 zeigt, wie Authentifizierung und Autorisierung für unsere Anwendung NerdDinner secure hinzufügen, damit Benutzer registrieren müssen und melden Sie sich an den Standort erstellen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 9e4d5cac-b071-440c-b044-20b6d0c964fb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/secure-applications-using-authentication-and-authorization
msc.type: authoredcontent
ms.openlocfilehash: 4a9b1e6d7d453bd8dc5a61b1f1cec4617af7d693
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874381"
---
<a name="secure-applications-using-authentication-and-authorization"></a>Sichern von Anwendungen mithilfe von Authentifizierung und Autorisierung
====================
durch [Microsoft](https://github.com/microsoft)

[PDF herunterladen](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Dies ist Schritt 9 mit einer kostenlosen ["NerdDinner" Anwendung Lernprogramm](introducing-the-nerddinner-tutorial.md) , die Durchläufe-durch die Schritte zum Erstellen einer kleinen, aber abgeschlossen haben, Webanwendung, die mithilfe von ASP.NET MVC-1.
> 
> Schritt 9 zeigt, wie Authentifizierung und Autorisierung für unsere Anwendung NerdDinner secure hinzufügen, damit Benutzer registrieren müssen und die Anmeldung auf der Website zum Erstellen von neuen Abendessen und nur der Benutzer, die eine Dinner hostet können sie später bearbeiten.
> 
> Bei Verwendung von ASP.NET MVC 3 empfehlen wir führen Sie die [erste Schritte mit MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) oder [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) Lernprogramme.


## <a name="nerddinner-step-9-authentication-and-authorization"></a>NerdDinner Schritt 9: Authentifizierung und Autorisierung

Jetzt Website unsere NerdDinner Anwendung jeder gewährt der die Fähigkeit zum Erstellen und bearbeiten die Details jeder Dinner. Wir ändern, damit Benutzer registrieren müssen und melden Sie sich an den Standort zum Erstellen von neuer Abendessen und eine Einschränkung hinzufügen, sodass nur der Benutzer, die eine Dinner dient diese später bearbeiten kann.

Hierzu müssen wir die Authentifizierung und Autorisierung verwenden, um unsere Anwendung gesichert werden.

### <a name="understanding-authentication-and-authorization"></a>Grundlegendes zur Authentifizierung und Autorisierung

*Authentifizierung* versteht man das Identifizieren und Überprüfen der Identität eines Clients, die Zugriff auf eine Anwendung. Einfach gesagt ist es zum Identifizieren "Wer der Endbenutzer wird, wenn sie eine Website besuchen". ASP.NET unterstützt mehrere Möglichkeiten zum Authentifizieren von Browserbenutzern. Für Internet-Webanwendungen wird der am häufigsten verwendeten authentifizierungsansatz verwendet "Formularauthentifizierung" aufgerufen. Formular-Authentifizierung kann der Entwickler eine HTML-Anmeldeformular innerhalb ihrer Anwendung erstellen und anschließend überprüft die Benutzername/Kennwort, das ein Endbenutzer für eine Datenbank oder andere Anmeldeinformationen Kennwortspeicher übermittelt. Wenn die Kombination Benutzername/Kennwort korrekt ist, kann der Entwickler ASP.NET zum Ausstellen eines verschlüsselten HTTP-Cookies zur Identifizierung des Benutzers bei zukünftigen Anfragen fordern. Wir senden durch Verwenden der Formularauthentifizierung mit unserem NerdDinner-Anwendung.

*Autorisierung* ist der Prozess, um zu bestimmen, ob ein authentifizierter Benutzer berechtigt ist, eine bestimmte URL-Ressource zuzugreifen oder eine Aktion durchzuführen. Beispielsweise sollten wir in unserer NerdDinner Anwendung autorisieren, nur in angemeldete Benutzer zugreifen können die */Abendessen/Create* URL, und erstellen Sie neue Abendessen. Wir möchten auch Autorisierungslogik hinzufügen, damit nur der Benutzer, die eine Dinner hostet bearbeiten – und alle anderen Benutzer Bearbeitungszugriff verweigern kann.

### <a name="forms-authentication-and-the-accountcontroller"></a>Formularauthentifizierung und die AccountController

Der standardmäßige Visual Studio-Projektvorlage für ASP.NET MVC ermöglicht automatische Formularauthentifizierung verwenden, wenn neue ASP.NET MVC-Anwendungen erstellt werden. Auch automatisch werden dem Projekt –, wodurch das wirklich sehr einfach, zur Integration der Sicherheit innerhalb eines Standorts eine Seite-Implementierung von vorgefertigten Konto Anmeldung hinzugefügt.

Die standardmäßige Stammwebsite zeigt einen Link "Anmelden" in der oberen rechten des Standorts, wenn der Benutzer den Zugriff auf nicht authentifiziert ist:

![](secure-applications-using-authentication-and-authorization/_static/image1.png)

Durch Klicken auf den Link "Anmelden" gelangen Benutzer die */Account/LogOn* URL:

![](secure-applications-using-authentication-and-authorization/_static/image2.png)

Besucher registriert haben dazu den Link "Register" – Was sie dauern wird die */Account/Register* URL und ermöglicht es ihnen, Kontodetails eingeben:

![](secure-applications-using-authentication-and-authorization/_static/image3.png)

Klicken auf die Schaltfläche "Registrieren" wird einen neuen Benutzer innerhalb des Systems ASP.NET-Mitgliedschaft erstellen, und authentifiziert den Benutzer auf dem Standort mithilfe der Formularauthentifizierung.

Wenn ein Benutzer angemeldet ist, ändert der Site.master der oberen rechten Rand der Seite zur Ausgabe einer "Willkommen [Username]!" anstelle einer "Anmelden" eine verknüpfen Nachricht und rendert eine "Abmelden". Durch Klicken auf den Link "Abmelden" meldet den Benutzer ab:

![](secure-applications-using-authentication-and-authorization/_static/image4.png)

Die obige Funktionalität für Anmeldung, Abmeldung und Registrierung wird innerhalb der AccountController-Klasse implementiert, die von Visual Studio bei der Erstellung des Projekts zum Projekt hinzugefügt wurde. Die Benutzeroberfläche für die AccountController wird mithilfe von Vorlagen anzeigen, in das Verzeichnis \Views\Account implementiert:

![](secure-applications-using-authentication-and-authorization/_static/image5.png)

Die AccountController-Klasse verwendet die ASP.NET-Formularauthentifizierung System zum Ausstellen von verschlüsselten Authentifizierungscookies und ASP.NET Membership-API zum Speichern und überprüfen Benutzernamen/Kennwörter. ASP.NET Membership-API ist erweiterbar und ermöglicht eine Kennwort-Anmeldeinformationenspeicher verwendet werden. Im Lieferumfang von ASP.NET sind integrierte Mitgliedschaft Implementierungen von Schlüsselspeicheranbietern, in denen Benutzernamen/Kennwörter in einer SQL-Datenbank oder in Active Directory gespeichert.

Wir können konfigurieren, welche Mitgliedschaftsanbieter unsere NerdDinner-Anwendung die zu verwendende durch Öffnen der Datei "web.config" im Stammverzeichnis des Projekts, und suchen die &lt;Mitgliedschaft&gt; Abschnitt einstuft. Die Standardeinstellung "Web.config" hinzugefügt, wenn das Projekt erstellt wurde die SQL-Mitgliedschaftsanbieters registriert und konfiguriert, um eine Verbindungszeichenfolge, mit dem Namen "ApplicationServices" zu verwenden, geben Sie den Speicherort der Datenbank.

Die Standardverbindungszeichenfolge für den "ApplicationServices" (den wird angegeben, in der &lt;ConnectionStrings&gt; Abschnitt der Datei "Web.config") ist die Verwendung von SQL Express konfiguriert werden. Er verweist auf eine SQL Express-Datenbank, die mit dem Namen "ASPNETDB. MDF-Datei"unter der Anwendungsverzeichnis" App\_Daten "Verzeichnis. Wenn diese Datenbank erstmalig nicht vorhanden ist, die die API für die Mitgliedschaft in der Anwendung verwendet wird, ASP.NET automatisch die Datenbank erstellen und Bereitstellen der entsprechenden mitgliedschaftsdatenbankschema, das darin:

![](secure-applications-using-authentication-and-authorization/_static/image6.png)

Wenn anstelle von SQL Express, wir möchten, verwenden eine vollständige SQL Server-Instanz (oder Herstellen einer Verbindung mit einer Remotedatenbank) alle wir Aufgabe müssten besteht darin, die Verbindungszeichenfolge "ApplicationServices" in der Datei "Web.config" aktualisieren, und stellen Sie sicher, dass das entsprechende Mitgliedschaft-Schema wurde in der Datenbank hinzugefügt, die am verweist. Können Sie ausführen, die "Aspnet\_regsql.exe" Utility im \Windows\Microsoft.NET\Framework\v2.0.50727\-Verzeichnis des entsprechenden Schemas für die Mitgliedschaft und die andere ASP.NET-Anwendungsdiensten zu einer Datenbank hinzuzufügen.

### <a name="authorizing-the-dinnerscreate-url-using-the-authorize-filter"></a>Autorisieren die Verwendung des Filters [Authorize] Abendessen/erstellen-URL

Wir haben nicht Code schreiben müssen, um eine sichere Authentifizierung und die Implementierung der Konto-Verwaltung für die NerdDinner-Anwendung zu aktivieren. Benutzer können neue Konten mit unseren Anwendung und die Anmeldung/Abmeldung von der Website registrieren.

Jetzt können wir Autorisierungslogik zur Anwendung hinzufügen und den Authentifizierungsstatus und den Benutzernamen der Besucher steuern, was sie anzeigen können und nicht innerhalb des Standorts verwenden. Beginnen wir mit der "Erstellen" Aktionsmethoden unsere DinnersController Klasse Autorisierungslogik hinzugefügt. Insbesondere wir erfordert, dass Benutzer den Zugriff auf die */Abendessen/Create* URL muss angemeldet sein. Wenn sie angemeldet sind müssen wir sie zur Anmeldeseite umleiten, damit sie anmelden können.

Implementieren diese Logik ist ziemlich einfach. Wir benötigen lediglich Aufgabe besteht darin ein Filterattribut [Authorize] unsere Aktionsmethoden erstellen hinzufügen wie folgt:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample1.cs)]

ASP.NET MVC unterstützt die Fähigkeit zum Erstellen von "Aktionsfilter", die verwendet werden können, um wieder verwendbare Logik zu implementieren, die an Aktionsmethoden deklarativ angewendet werden können. Der [Authorize]-Filter ist einer der von ASP.NET MVC bereitgestellte integrierte Aktionsfilter verwendet werden, und es kann ein Entwickler deklarativ Autorisierungsregeln für Aktionsmethoden und Controllerklassen gelten.

Bei ohne Parameter (wie weiter oben) erzwingt [Authorize] Filter an, dass der Benutzer, der Anforderung der Aktionsmethode – angemeldet sein muss, und es automatisch den Browser an die Anmelde-URL umleiten wird, wenn sie nicht vorhanden sind. Bei Aktionen diese Umleitung, die ursprünglich angeforderte URL als eine Querystring-Argument übergeben wird (z. B.: / Account/anmelden? ReturnUrl = % 2fDinners % 2fCreate). Die AccountController wird den Benutzer dann zurück an die ursprünglich angeforderte URL umleiten, sobald bei der Anmeldung.

Filter [Authorize] unterstützt optional die Möglichkeit, eine Eigenschaft "Benutzer" oder "Rollen" anzugeben, die verwendet werden können, um festzulegen, dass der Benutzer mit sowohl in und eine Liste der zulässigen Benutzer oder ein Mitglied einer zulässigen Sicherheitsrolle angemeldet ist. So kann z. B. der folgenden Code nur auf, zwei bestimmte Benutzer, "Scottgu" und "Billg", beim Zugriff auf die URL Abendessen/erstellen:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample2.cs)]

Einbetten von bestimmten Benutzernamen innerhalb des Codes ist jedoch ziemlich nicht verwaltet werden. Ein besserer Ansatz ist auf höherer Ebene "Rollen" zu definieren, die der Code überprüft, und die anschließende Zuordnung von Benutzern in der Rolle mit einer Datenbank oder die active Directory-Systemermittlung (Aktivieren der tatsächlichen Benutzer-Zuordnung Liste extern aus dem Code gespeichert werden). ASP.NET umfasst eine integrierte Rolle und Verwaltungs-API als auch einen integrierten Satz von Rollenanbieter (einschließlich beendeter für SQL und Active Directory), mit die diese Benutzerrolle/Zuordnung vornehmen können. Es konnte den Code, um nur Benutzer innerhalb einer bestimmten "Admin" Rolle beim Zugriff auf die URL Abendessen/erstellen können aktualisiert werden:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample3.cs)]

### <a name="using-the-useridentityname-property-when-creating-dinners"></a>Beim Erstellen der Eigenschaft User.Identity.Name mit Abendessen

Wir können den Benutzernamen des aktuell angemeldeten Benutzers einer Anforderung mithilfe der User.Identity.Name-Eigenschaft, die für die Basisklasse für Controller verfügbar gemacht werden abgerufen werden.

Frühere Wenn wir die HTTP-POST-Version von unseren Create()-Aktionsmethode implementiert mussten wir hart codiert die Eigenschaft "HostedBy" von der Dinner in eine statische Zeichenfolge. Wir können jetzt aktualisieren dieser Code, um stattdessen die User.Identity.Name-Eigenschaft als auch automatisch eine Antwort für den Host mit dem Erstellen der Dinner hinzufügen:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample4.cs)]

Da wir die Methode Create()-ein [Authorize]-Attribut hinzugefügt haben, wird sichergestellt, dass ASP.NET MVC, dass die Aktionsmethode nur ausgeführt, wenn der Benutzer Zugriff auf die URL Abendessen/erstellen auf der Website angemeldet ist. Daher wird der Eigenschaftswert User.Identity.Name immer einen gültigen Benutzernamen enthalten.

### <a name="using-the-useridentityname-property-when-editing-dinners"></a>Mithilfe der User.Identity.Name-Eigenschaft, bei der Bearbeitung Abendessen

Nun fügen Sie einige Autorisierungslogik, die die Benutzer zu beschränken, damit sie nur die Eigenschaften von Abendessen bearbeiten können, die sie selbst gehostet werden.

Um dabei zu helfen, zunächst fügen wir eine Hilfsmethode "IsHostedBy(username)" auf unserer Dinner-Objekt (in der partiellen Dinner.cs-Klasse, die wir zuvor erstellt). Diese Hilfsmethode gibt "true" oder "false", davon ab, ob eine angegebene Benutzername entspricht die Eigenschaft Dinner HostedBy, kapselt die Logik erforderlich, um von ihnen einen Zeichenfolgenvergleich auszuführen:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample5.cs)]

Wir müssen Edit() Aktionsmethoden in unsere DinnersController-Klasse dann ein [Authorize]-Attribut hinzufügen. Dadurch wird sichergestellt, dass Benutzer auf Anforderung müssen, in protokolliert werden einem */Dinners/bearbeiten / [Id]* URL.

Wir können Sie Code hinzufügen, um unsere bearbeiten-Methoden, die die Dinner.IsHostedBy(username)-Hilfsmethode verwendet, um sicherzustellen, dass der angemeldete Benutzer die Dinner des Hosts übereinstimmt. Wenn der Benutzer nicht der Host ist, wir eine "InvalidOwner" anzeigen und die Anforderung zu beenden. Der Code dazu sieht wie im folgenden:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample6.cs)]

Wir können dann mit der rechten Maustaste auf das Verzeichnis \Views\Dinners und wählen Sie die Add -&gt;Menübefehl zum Erstellen einer neuen "InvalidOwner"-Ansicht anzeigen. Fügen wir weisen Sie ihm mit der folgenden Fehlermeldung:

[!code-aspx[Main](secure-applications-using-authentication-and-authorization/samples/sample7.aspx)]

Und nun, wenn ein Benutzer versucht, eine Dinner bearbeiten, die sie besitzen nicht, sie erhalten eine Fehlermeldung angezeigt:

![](secure-applications-using-authentication-and-authorization/_static/image7.png)

Wir können die gleichen Schritte für die Aktionsmethoden Delete() wiederholen, in unserem Controller aus, um Sperren über die Berechtigung zum Abendessen auch löschen, und stellen Sie sicher, dass nur der Host eine Dinner gelöscht werden kann.

### <a name="showinghiding-edit-and-delete-links"></a>Einblenden/Ausblenden bearbeiten und Löschen von Verknüpfungen

Wir sind auf die Aktionsmethode bearbeiten und Löschen von unsere DinnersController-Klasse aus unserem Details URL verknüpfen:

![](secure-applications-using-authentication-and-authorization/_static/image8.png)

Gegenwärtig sind wir die Aktionslinks bearbeiten und löschen, unabhängig davon, ob der Besucher, mit der URL für die Details der Host die Dinner angezeigt. Wir ändern, damit die Links nur angezeigt werden, wenn der Benutzer über den Besitzer der Dinner ist.

Details() Aktionsmethode in unserer DinnersController Ruft ein Objekt Dinner und übergibt diese als das Modellobjekt zu unserem Vorlage anzeigen:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample8.cs)]

Wir können unsere Vorlage anzeigen, um bedingt ein-/ausblenden der Links bearbeiten und löschen die Hilfsmethode wie unten Dinner.IsHostedBy() mit aktualisieren:

[!code-aspx[Main](secure-applications-using-authentication-and-authorization/samples/sample9.aspx)]

#### <a name="next-steps"></a>Nächste Schritte

Jetzt betrachten wie wir authentifizierte Benutzern das Erstellen der Antwort für Abendessen mithilfe von AJAX aktivieren können.

> [!div class="step-by-step"]
> [Zurück](implement-efficient-data-paging.md)
> [Weiter](use-ajax-to-deliver-dynamic-updates.md)
