---
uid: mvc/overview/older-versions-1/nerddinner/secure-applications-using-authentication-and-authorization
title: Sichern von Anwendungen mithilfe von Authentifizierung und Autorisierung | Microsoft-Dokumentation
author: microsoft
description: Schritt 9 zeigt, wie Authentifizierung und Autorisierung für die NerdDinner-Anwendung schützen hinzufügen, sodass die Benutzer zum Registrieren müssen und melden Sie sich an den Standort erstellen...
ms.author: aspnetcontent
ms.date: 07/27/2010
ms.assetid: 9e4d5cac-b071-440c-b044-20b6d0c964fb
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/secure-applications-using-authentication-and-authorization
msc.type: authoredcontent
ms.openlocfilehash: 0005b99dbf7d59e96313f025880c46cdec4838b6
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37801391"
---
<a name="secure-applications-using-authentication-and-authorization"></a>Sichern von Anwendungen mithilfe von Authentifizierung und Autorisierung
====================
durch [Microsoft](https://github.com/microsoft)

[PDF herunterladen](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Dies ist Schritt 9, der ein kostenloses ["NerdDinner"-webanwendungstutorial](introducing-the-nerddinner-tutorial.md) , die führt – Exemplarische Vorgehensweise erstellen eine kleine, jedoch abgeschlossen haben, Web-Anwendung mithilfe von ASP.NET MVC-1.
> 
> Schritt 9 zeigt, wie Authentifizierung und Autorisierung für die NerdDinner-Anwendung schützen hinzufügen, sodass die Benutzer zum Registrieren müssen und Anmeldung auf der Website zum Erstellen von neuen Dinner und nur der Benutzer, die eine Dinner hostet können sie später bearbeiten.
> 
> Wenn Sie ASP.NET MVC 3 verwenden, sollten Sie Sie folgen den [erste Schritte mit MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) oder [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) Tutorials.


## <a name="nerddinner-step-9-authentication-and-authorization"></a>NerdDinner Schritt 9: Authentifizierung und Autorisierung

Jetzt Website unsere Anwendung, die jeder Benutzer gewährt NerdDinner für den die Möglichkeit zum Erstellen und bearbeiten die Details jeder Dinner. Wir ändern das also, dass Benutzer registrieren müssen und melden Sie sich an den Standort zum Erstellen von neuen Dinner und fügen Sie eine Einschränkung hinzu, sodass nur der Benutzer, die eine Dinner zu hosten, ist er später bearbeiten kann.

Um dies zu ermöglichen verwenden Authentifizierung und Autorisierung wir um zu unserer Anwendung zu schützen.

### <a name="understanding-authentication-and-authorization"></a>Grundlegendes zur Authentifizierung und Autorisierung

*Authentifizierung* ist der Prozess zur Identifizierung und Überprüfung der Identität eines Clients, die auf eine Anwendung zugreifen. Einfacher ausgedrückt ist es zum Identifizieren von "Wer der Endbenutzer wird, wenn sie eine Website besuchen". ASP.NET unterstützt mehrere Möglichkeiten, Browserbenutzer zu authentifizieren. Internet-Webanwendungen wird der am häufigsten verwendeten authentifizierungsansatz verwendet "Formularauthentifizierung" bezeichnet. Formular-Authentifizierung kann es sich um ein Entwickler zum Erstellen einer HTML-Anmeldeformular innerhalb ihrer Anwendung und überprüfen Sie anschließend das Benutzername/Kennwort, die, das ein Endbenutzer auf eine Datenbank oder andere Anmeldeinformationen Kennwortspeicher übermittelt. Wenn die Kombination von Benutzername und Kennwort richtig ist, kann der Entwickler anschließend ASP.NET ein verschlüsselte HTTP-Cookie zur Identifizierung des Benutzers für zukünftige Anforderungen ausgeben gefragt. Wir senden, durch die Verwendung der Formularauthentifizierung mit unserer NerdDinner-Anwendung.

*Autorisierung* ist der Prozess des bestimmens, ob ein authentifizierter Benutzer berechtigt ist auf eine bestimmte URL-Ressource zuzugreifen oder eine Aktion auszuführen. Beispielsweise sollten wir in unserer NerdDinner-Anwendung zu autorisieren, dass nur Benutzer, die angemeldet sind, zugreifen können die */Dinners/erstellen* URL, und erstellen Sie neue Dinner. Wir möchten auch Autorisierungslogik hinzufügen, damit nur der Benutzer, die eine Dinner gehostet wird – bearbeiten und Zugriff auf alle anderen Benutzer verweigern kann.

### <a name="forms-authentication-and-the-accountcontroller"></a>Formularauthentifizierung und AccountController-Komponente

Die standardmäßige Visual Studio-Projektvorlage für ASP.NET MVC ermöglicht die Formularauthentifizierung automatisch, wenn neue ASP.NET MVC-Anwendungen erstellt werden. Er auch automatisch eine vorab erstellte Konto Login-Seite-Implementierung dem Projekt hinzugefügt – Dadurch wird es wirklich einfach, Integration der Sicherheit innerhalb eines Standorts.

Die standardmäßige Stammwebsite zeigt einen Link "Anmelden" in der oberen rechten des Standorts, wenn der Benutzer darauf zugreifen, nicht authentifiziert ist:

![](secure-applications-using-authentication-and-authorization/_static/image1.png)

Den Link "Anmelden" gelangen Benutzer die */Account/LogOn* URL:

![](secure-applications-using-authentication-and-authorization/_static/image2.png)

Besucher, die nicht registriert haben dazu werden Sie auf den Link "Register" – in die sie gelangen die */Account/Register* URL und zur Eingabe der Details zu ermöglichen:

![](secure-applications-using-authentication-and-authorization/_static/image3.png)

Klicken Sie auf die Schaltfläche "Registrieren" wird einen neuen Benutzer in das Mitgliedschaftssystem von ASP.NET erstellen, und authentifiziert den Benutzer auf der Website unter Verwendung der Formularauthentifizierung.

Wenn ein Benutzer angemeldet ist, ändert sich Site.master rechts oben auf der Seite, um eine "Willkommen [Username]!" auszugeben. anstelle einer "Anmelden" einen verknüpfen Nachricht und rendert eine "Abmelden". Klicken Sie auf den Link "Abmelden" meldet den Benutzer:

![](secure-applications-using-authentication-and-authorization/_static/image4.png)

Die obige Funktionalität für Anmeldung, Abmeldung und Registrierung wird innerhalb der AccountController-Klasse implementiert, die von Visual Studio bei der Erstellung des Projekts zu Ihrem Projekt hinzugefügt wurde. Die Benutzeroberfläche für AccountController-Komponente wird mithilfe von Vorlagen anzeigen, innerhalb des Verzeichnisses \Views\Account implementiert:

![](secure-applications-using-authentication-and-authorization/_static/image5.png)

Die AccountController-Klasse verwendet die ASP.NET-Formularauthentifizierung-System zum Ausstellen von verschlüsselten Authentifizierungscookies und ASP.NET Membership-API zum Speichern und überprüfen Benutzernamen und Kennwörtern. ASP.NET Membership-API ist erweiterbar und können Anmeldeinformationen Kennwortspeicher verwendet werden. Im Lieferumfang von ASP.NET sind integrierte Mitgliedschaft providerimplementierungen, in denen ein Benutzername und Kennwort in einer SQL-Datenbank oder in Active Directory gespeichert.

Wir können festlegen, sollten die NerdDinner-Anwendung durch Öffnen der Datei "web.config" im Stammverzeichnis des Projekts, und Suchen nach verwenden, der Membership-Provider die &lt;Mitgliedschaft&gt; Abschnitt darin. Die standardmäßige Datei "Web.config" hinzugefügt, wenn das Projekt erstellt wurde die SQL-Mitgliedschaftsanbieters registriert und konfiguriert, um eine Verbindungszeichenfolge, mit dem Namen "ApplicationServices" zu verwenden, geben Sie den Speicherort der Datenbank.

Die Standardverbindungszeichenfolge für die "ApplicationServices" (die wird angegeben, in der &lt;ConnectionStrings&gt; -Abschnitt der Datei "Web.config") ist zur Verwendung von SQL Express konfiguriert. Es verweist auf eine SQL Express-Datenbank, die mit dem Namen "ASPNETDB. MDF-Datei"unter der Anwendung" App\_Daten "Verzeichnis. Wenn diese Datenbank beim ersten nicht vorhanden ist, die der Membership-API in der Anwendung verwendet wird, ASP.NET automatisch die Datenbank erstellen und Bereitstellen der entsprechenden mitgliedschaftsdatenbankschema, das in diesem:

![](secure-applications-using-authentication-and-authorization/_static/image6.png)

Wenn anstelle von SQL Express, wir verwenden einer vollständigen SQL Server-Instanz (oder eine Verbindung mit einer Remotedatenbank herstellen) möchten, alle Aufgaben brauchen wir da ist die "ApplicationServices" Verbindungszeichenfolge in der Datei "Web.config" aktualisieren, und stellen Sie sicher, dass das Schema für die entsprechende Mitgliedschaft wurde in der Datenbank hinzugefügt, die es auf zeigt. Können Sie ausführen, die "Aspnet\_regsql.exe" Hilfsprogramm im Verzeichnis \Windows\Microsoft.NET\Framework\v2.0.50727\ des entsprechenden Schemas für Mitgliedschaft und die andere ASP.NET-Anwendungsdienste in einer Datenbank hinzufügen.

### <a name="authorizing-the-dinnerscreate-url-using-the-authorize-filter"></a>Autorisieren die Dinner/Create-URL mithilfe des Filters [Authorize]

Wir mussten Schreiben beliebigen Code, um eine sichere Authentifizierung und die Implementierung der Konto-Verwaltung für die NerdDinner-Anwendung zu aktivieren. Benutzer können neue Konten mit unserer Anwendung, und klicken Sie auf Anmeldung/Abmeldung von der Website registrieren.

Jetzt können wir Autorisierungslogik zur Anwendung hinzufügen, und verwenden die Authentication-Status und den Benutzernamen der Besucher um zu steuern, welche Inhalte sie innerhalb des Standorts nicht möglich. Beginnen wir mit der Aktionsmethoden "Erstellen" unserer Klasse "dinnerscontroller" Autorisierungslogik hinzugefügt. Insbesondere werden wir benötigen, die Benutzern beim Zugriff auf die */Dinners/erstellen* URL muss angemeldet sein. Wenn sie angemeldet sind nicht werden wir sie zur Anmeldeseite umleiten, damit sie sich anmelden können.

Implementieren diese Logik ist ziemlich einfach. Alles, was wir benötigen die to-do ist unsere Aktionsmethoden erstellen ein Filterattribut [Authorize] hinzu wie folgt:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample1.cs)]

ASP.NET MVC unterstützt die Fähigkeit zum Erstellen von "Aktionsfilter", die verwendet werden können, um wiederverwendbare Logik zu implementieren, die deklarativ auf Aktionsmethoden angewendet werden können. Der [Authorize]-Filter ist einer der von ASP.NET MVC bereitgestellte Filter integrierte Aktion aus, und er ermöglicht dem Entwickler, die deklarativ gelten die Autorisierungsregeln auf Aktionsmethoden und Controllerklassen.

Bei ohne Parameter (wie weiter oben) erzwingt der Filter [Authorize] an, dass der Benutzer, der Anforderung der Aktionsmethode angemeldet sein muss – und es automatisch der Browser zur Anmelde-URL umgeleitet wird, wenn sie nicht sind. Beim Ausführen dieser Umleitung, die ursprünglich angeforderte URL als Querystring-Argument übergeben wird (z. B.: / Account/LogOn? ReturnUrl = % 2fDinners % 2fCreate). AccountController-Komponente leitet dann den Benutzer zurück zur ursprünglich angeforderten URL, nach der Anmeldung.

Der Filter [Authorize] unterstützt optional die Möglichkeit, eine Eigenschaft "Benutzer" oder "Roles" angeben, die verwendet werden kann, um festzulegen, dass der Benutzer mit sowohl in und in einer Liste von zulässigen Benutzern oder ein Mitglied einer Sicherheitsrolle für die zulässigen angemeldet ist. Der folgende Code ermöglicht beispielsweise nur zwei bestimmte Benutzer, "Scottgu" und "Billg," Zugriff auf die Dinner/Create-URL:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample2.cs)]

Einbetten von bestimmten Benutzernamen in Code ist jedoch ziemlich nicht verwaltbar sein. Ein besserer Ansatz ist, um auf höherer Ebene "Roles" zu definieren, die der Code überprüft, und klicken Sie dann zum Zuordnen von Benutzern in der Rolle, die mit einer Datenbank oder active Directory-System (ermöglicht die tatsächliche Zuordnung Benutzerliste aus dem Code extern gespeichert werden). ASP.NET umfasst eine integrierte Rolle-Verwaltungs-API als auch einen integrierten Satz von Rollenanbietern (einschließlich für SQL und Active Directory), mit die diese Benutzerrolle/Zuordnung vornehmen können. Wir könnten klicken Sie dann den Code damit, dass nur Benutzer innerhalb einer bestimmten "Admin" Rolle Zugriff auf die Dinner/Create-URL aktualisieren:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample3.cs)]

### <a name="using-the-useridentityname-property-when-creating-dinners"></a>Verwenden die Eigenschaft User.Identity.Name beim Erstellen von Dinner

Wir können den Benutzernamen des aktuell angemeldeten Benutzers der eine Anforderung mithilfe der User.Identity.Name-Eigenschaft, die für die Basisklasse für Controller verfügbar gemacht werden, abrufen.

Weiter oben, wenn wir die HTTP-POST-Version unserer Create() Aktionsmethode implementiert werden musste hartcodiert die Eigenschaft "HostedBy" das Dinner in eine statische Zeichenfolge. Wir können jetzt aktualisieren dieser Code stattdessen die User.Identity.Name-Eigenschaft verwendet, als auch automatisch eine Antwort für den Host mit dem Erstellen des Dinner hinzufügen:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample4.cs)]

Da wir ein [Authorize]-Attribut für die Create()-Methode hinzugefügt haben, wird Sie ASP.NET MVC sichergestellt, dass die Aktionsmethode nur ausgeführt, wenn der Benutzer Zugriff auf die Dinner/Create-URL auf der Website angemeldet ist. Daher wird der Eigenschaftswert User.Identity.Name immer einen gültigen Benutzernamen enthalten.

### <a name="using-the-useridentityname-property-when-editing-dinners"></a>Verwenden die User.Identity.Name-Eigenschaft beim Bearbeiten von Dinner

Nun fügen Sie einige Autorisierungslogik, die Benutzer beschränkt, sodass sie nur die Eigenschaften des Dinner bearbeiten können, die sie selbst gehostet werden.

Um dies zu vereinfachen, zunächst fügen wir eine Hilfsmethode "IsHostedBy(username)" auf unserer Dinner-Objekt (innerhalb der partiellen Dinner.cs-Klasse, die wir zuvor erstellt). Diese Hilfsmethode gibt True oder False, je nachdem, ob ein angegebenen Benutzernamen entspricht die Dinner-HostedBy-Eigenschaft und kapselt die Logik erforderlich, um einen Zeichenfolgenvergleich davon auszuführen:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample5.cs)]

Wir werden die Edit() Aktionsmethoden in unsere Klasse "dinnerscontroller" klicken Sie dann eine [Authorize]-Attribut hinzufügen. Dadurch wird sichergestellt, dass Benutzer auf Anforderung angemeldet sein müssen, eine *"/ dinners" / Edit / [Id]* URL.

Wir können Sie Code hinzufügen, um unsere Edit-Methoden, die die Dinner.IsHostedBy(username)-Hilfsmethode verwendet, um sicherzustellen, dass die Dinner-Host mit der angemeldeten Benutzer übereinstimmt. Wenn der Benutzer nicht der Host ist, wir zeigt ein "InvalidOwner" an und beendet die Anforderung. Der Code hierfür sieht wie folgt aus:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample6.cs)]

Wir können dann mit der rechten Maustaste auf das Verzeichnis \Views\Dinners und wählen Sie die Add -&gt;Menübefehl zum Erstellen einer neuen "InvalidOwner" Ansicht anzeigen. Wir werden befüllen Sie sie mit der folgenden Fehlermeldung angezeigt:

[!code-aspx[Main](secure-applications-using-authentication-and-authorization/samples/sample7.aspx)]

Und nun, wenn ein Benutzer versucht, eine Dinner zu bearbeiten, die sie besitzen nicht, sie erhalten eine Fehlermeldung angezeigt:

![](secure-applications-using-authentication-and-authorization/_static/image7.png)

Wir können die gleichen Schritte für die Aktionsmethoden Delete() wiederholen, in den Controller, Sperren Sie die Berechtigung zum Dinner auch löschen, und stellen Sie sicher, dass nur der Host von einem Dinner gelöscht werden kann.

### <a name="showinghiding-edit-and-delete-links"></a>Anzeigen/ausblenden bearbeiten "und" Links "löschen"

Wir sind von unseren Informationen-URL an die Aktionsmethode bearbeiten und Löschen von unserer Klasse "dinnerscontroller" verknüpfen:

![](secure-applications-using-authentication-and-authorization/_static/image8.png)

Gegenwärtig werden wir die Aktionslinks bearbeiten und löschen, unabhängig davon, ob der Besucher auf die URL für die Details der Host die Dinner angezeigt. Lassen Sie uns dies ändern und die Links nur angezeigt werden, wenn der Benutzer aufgerufen. der Besitzer der Dinner ist.

Details() Aktionsmethode in unsere "dinnerscontroller" Ruft ein Objekt Dinner ab und leitet diese dann als das Modellobjekt an unsere Vorlage anzeigen:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample8.cs)]

Wir können unsere Vorlage anzeigen, um bedingt ein-/ausblenden der Bearbeiten und löschen die Verknüpfungen mit der Hilfsmethode wie die folgende Dinner.IsHostedBy() aktualisieren:

[!code-aspx[Main](secure-applications-using-authentication-and-authorization/samples/sample9.aspx)]

#### <a name="next-steps"></a>Nächste Schritte

Jetzt sehen wir uns wie wir authentifizierte Benutzern RSVP für die Verwendung von AJAX Dinner aktivieren können.

> [!div class="step-by-step"]
> [Zurück](implement-efficient-data-paging.md)
> [Weiter](use-ajax-to-deliver-dynamic-updates.md)
