---
uid: identity/overview/getting-started/introduction-to-aspnet-identity
title: "Einführung in ASP.NET Identity | Microsoft Docs"
author: jongalloway
description: "Das ASP.NET-Mitgliedschaftssystem wurde mit ASP.NET 2.0 Back in 2005 und seit eingeführt und viele Änderungen in der Weise Web Applications Typicall stattgefunden haben..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 38717fc1-5989-43cf-952d-4007cc1dd923
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/getting-started/introduction-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: a66e2a80668dbf291b9cc34f205b546b72d92bcc
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="introduction-to-aspnet-identity"></a>Einführung in ASP.NET Identity
====================
durch [Jon Galloway](https://github.com/jongalloway), [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

> Das ASP.NET-Mitgliedschaftssystem wurde mit ASP.NET 2.0 Back in 2005 und seit eingeführt und dann Änderungen vorgenommen wurden viele Möglichkeiten, die Webanwendungen in der Regel Authentifizierung und Autorisierung behandeln. ASP.NET Identity ist eine neue Betrachtung wie das Mitgliedschaftssystem sein soll, wenn Sie moderne Anwendungen für Web-, Telefon oder Tablet erstellen.
> 
> In diesem Artikel wurde von Pranav Rastogi geschrieben ([@rustd](https://twitter.com/rustd)), Jon Galloway ([@jongalloway](https://twitter.com/jongalloway)), Tom Dykstra und Rick Anderson ([ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ).


## <a name="background-membership-in-aspnet"></a>Hintergrund: Mitgliedschaft in ASP.NET

### <a name="aspnet-membership"></a>ASP.NET-Mitgliedschaft

[ASP.NET-Mitgliedschaft](https://msdn.microsoft.com/en-us/library/yh26yfzy(v=VS.100).aspx) wurde entworfen, um die Website mitgliedschaftsanforderungen zu lösen, die häufig in 2005 wurden die formularbasierte Authentifizierung und SQL Server-Datenbank für den Benutzernamen, Kennwörtern und Profildaten beteiligt. Heute einen viel größeren Array von Datenspeicheroptionen für Webanwendungen vorhanden ist, und die meisten Entwickler ihre Websites Identitätsanbieter sozialer Netzwerke für die Authentifizierung und Autorisierung Funktionalität verwenden aktiviert werden soll. Die Einschränkungen des Entwurfs des ASP.NET-Mitgliedschaft erschweren. dieser Übergang:

- Das Datenbankschema wurde entwickelt, für SQL Server, und es kann nicht geändert werden. Sie können Informationen zum Profil hinzufügen, aber die zusätzlichen Daten in eine andere Tabelle, die für den Zugriff auf irgendeine Weise außer durch die Profil-Anbieter-API erschwert gepackt ist.
- Das anbietersystem ermöglicht es Ihnen, die dem Sicherungsspeicher Daten ändern, aber das System ist darauf ausgelegt, um Annahmen für eine relationale Datenbank geeignet. Sie müssen dann den relationalen Entwurf umgehen, indem Sie das Schreiben von viel Code und viele aber können Sie einen Anbieter zum Speichern von Informationen zur Mitgliedschaft in einer nicht-relationalen Speichermechanismus, z. B. Azure-Speichertabellen, schreiben `System.NotImplementedException` für Methoden, die keine Ausnahmen gelten Sie für NoSQL-Datenbanken.
- Da das Protokoll/Log-Out-Funktionalität auf Formularauthentifizierung basiert, kann nicht das Mitgliedschaftssystem verwenden [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md). OWIN umfasst Middleware-Komponenten für die Authentifizierung, einschließlich der Unterstützung für Anmeldungen mit externen Identitätsanbietern (z. B. Microsoft Accounts, Facebook, Google, Twitter) und Anmeldungen mithilfe von Unternehmenskonten aus einer lokalen Active Directory oder Azure Active Directory. OWIN bietet auch Unterstützung für OAuth 2.0, JWT und CORS.

### <a name="aspnet-simple-membership"></a>Einfache ASP.NET Membership

[Einfache ASP.NET-Mitgliedschaft](../../../web-pages/overview/security/16-adding-security-and-membership.md) als eine Mitgliedschaftssystem für ASP.NET Web Pages entwickelt wurde. Es wurde mit WebMatrix und Visual Studio 2010 SP1 veröffentlicht. Das Ziel des einfache Mitgliedschaft wurde zu Web Pages-anwendungsstartseiten Mitgliedschaftsfunktionen hinzufügen zu erleichtern.

Einfache Mitgliedschaft es einfacher, Benutzerprofilinformationen, anpassen, aber, die noch andere Probleme bei ASP.NET-Mitgliedschaft geteilt, und es gelten einige Einschränkungen:

- Es war schwer zu Mitgliedschaft Systemdaten in einem nicht relationalen Datenspeicher beizubehalten.
- Sie können nicht mit OWIN verwenden.
- Es funktioniert nicht gut mit vorhandenen ASP.NET-Mitgliedschaft-Anbieter, und ist nicht erweiterbar.

### <a name="aspnet-universal-providers"></a>ASP.NET Universal Providers

[ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx) wurden entwickelt, um zu ermöglichen, Informationen zur Mitgliedschaft in Microsoft Azure SQL-Datenbank, und sie funktionieren auch mit SQL Server Compact beizubehalten. Universal Providers wurden basierend auf Entity Framework Code First erstellt, was bedeutet, dass die Universal Providers zum Beibehalten von Daten in einen beliebigen Datenspeicher von EF unterstützt verwendet werden kann. Mit dem Universal Providers wurde zahlreicher sowie das Schema der Datenbank bereinigt.

Universal Providers bauen auf die ASP.NET-Mitgliedschaft-Infrastruktur, damit sie weiterhin dieselben Einschränkungen wie bei der SqlMembership Anbieter ausführen. D. h., sie wurden entwickelt, für relationale Datenbanken und schwierig, Profile und die Benutzerinformationen anpassen. Diese Anbieter zu Formularauthentifizierung für die Anmeldung und Abmeldung Funktionalität weiterhin verwenden.

## <a name="aspnet-identity"></a>ASP.NET Identity

Wie die Mitgliedschaft hat Textabschnitt in ASP.NET im Laufe der Jahre entwickelt, das ASP.NET-Team hat viel basierend auf Feedback von Kunden von gelernt.

Die Annahme, die Benutzer protokolliert werden, indem Sie eingeben, einen Benutzernamen und Kennwort, die sie in Ihrer eigenen Anwendung registriert haben, ist nicht mehr gültig. Das Web ist mehr sozialen geworden. Benutzer interagieren in Echtzeit über soziale Kanäle wie Facebook, Twitter und anderen soziale Websites miteinander. Entwickler möchten Benutzer sich mit ihrer sozialen Identitäten anmelden können, damit sie eine komfortable auf ihren Websites haben können. Eine moderne Mitgliedschaftssystem muss Umleitung-basierte Anmeldungen, z. B. Facebook, Twitter und andere Authentifizierungsanbieter aktivieren.

Wie Webentwicklung weiterentwickelt, hat also die Muster der Webentwicklung. Einheit des Anwendungscodes testen Core relevant für Anwendungsentwickler wurde. In 2008 hinzugefügt ASP.NET ein neues Framework basierend auf der Model-View-Controller (MVC)-Muster, Teil, um Entwickler bei Komponententests erstellen testbare ASP.NET-Anwendungen unterstützen. Entwickler, die die Einheit wollten Testen ihrer Anwendungslogik zudem soll demonstriert werden, damit das Mitgliedschaftssystem verwenden werden.

Unter Berücksichtigung dieser Änderungen in der Web-Anwendungsentwicklung wurde entwickelt, ASP.NET Identity mit den folgenden Zielen:

- **Eine ASP.NET Identity-system**

    - ASP.NET Identity kann mit allen die ASP.NET-Frameworks wie ASP.NET MVC, Web Forms, Webseiten, Web-API und SignalR verwendet werden.
    - ASP.NET Identity kann verwendet werden, wenn Sie Web "," Phone "," Store "oder" Hybrid-Anwendungen erstellen.
- **Erleichterte Bedienung unter Verwendung der Profildaten bezüglich des Benutzers**

    - Sie haben die Kontrolle über das Schema der Benutzer- und Profilinformationen. Beispielsweise können Sie problemlos aktivieren das System zum Speichern von Geburtsdaten, die vom Benutzer eingegeben werden, wenn sie ein Konto in Ihrer Anwendung registrieren.

- **Persistenz-Steuerelement**

    - Standardmäßig speichert die ASP.NET Identity System alle Benutzerinformationen in einer Datenbank. Zum Implementieren aller seiner Persistenz-Mechanismen verwendet ASP.NET Identity Entity Framework Code First.
    - Da Sie steuern das Datenbankschema sowie allgemeine Aufgaben wie das Ändern von Tabellennamen, Spaltennamen oder zu ändern ist der Datentyp der Primärschlüssel sehr einfach.
    - Es ist einfach in andere Speichermechanismen wie SharePoint, Azure-Speicherdienst-Tabelle, NoSQL-Datenbanken, usw., zu integrieren, ohne Auslösen `System.NotImplementedExceptions` Ausnahmen.
- **Einheit Prüfbarkeit**

    - ASP.NET Identity macht die Webanwendung Weitere Einheit getestet werden können. Sie können Komponententests für die Teile der Anwendung schreiben, die ASP.NET Identity zu verwenden.
- **Rollenanbieter**

    - Es gibt ein Rollenanbieter Hier können Sie den Zugriff auf Teile der Anwendung durch Rollen zu beschränken. Sie können problemlos Rollen wie z. B. "Admin" erstellen und Hinzufügen von Benutzern zu Rollen.
- **Anspruchsbasierte**

    - ASP.NET Identity unterstützt die anspruchsbasierte Authentifizierung verwendet werden, wo die Identität des Benutzers einen Satz von Ansprüchen dargestellt wird. Ansprüche können Entwickler werden viel ausdrucksstärker beim Beschreiben der Identität eines Benutzers als Rollen ermöglichen. Während der Mitgliedschaft in Datenbankrolle nur einen booleschen Wert (Element oder nicht-memberoperators) ist, kann ein Anspruch umfassende Informationen über die Identität und die Mitgliedschaft des Benutzers enthalten.
- **Soziale Anmeldeanbietern**

    - Sie können einfach Ihre Anwendung sozialen Anmeldungen z. B. Microsoft Account, Facebook, Twitter, Google usw. hinzu, und speichern Sie die benutzerspezifischen Daten in der Anwendung.
- **Azure Active Directory**

    - Sie können auch Protokoll-Funktionen mithilfe von Azure Active Directory hinzufügen und speichern Sie die benutzerspezifischen Daten in der Anwendung. Weitere Informationen finden Sie unter [Organisationskonten](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#orgauth) im Erstellen von ASP.NET-Webprojekten in Visual Studio 2013
- **OWIN-Integration**

    - ASP.NET-Authentifizierung basiert jetzt auf OWIN-Middleware, die auf eine beliebige OWIN-basierten Host verwendet werden kann. ASP.NET Identity muss keine Abhängigkeit auf System.Web. Es ist ein vollständig kompatibel OWIN-Framework und in jede owin-gehosteten Anwendung verwendet werden kann.
    - ASP.NET Identity verwendet OWIN-Authentifizierung für Protokoll-in/Abmeldung von Benutzern der Website an. Dies bedeutet, dass anstatt FormsAuthentication, um Cookies zu generieren, die Anwendung OWIN CookieAuthentication verwendet, nachholen.
- **NuGet-Paket**

    - ASP.NET Identity wird als ein NuGet-Paket neu verteilt, die in den Vorlagen für ASP.NET MVC, Web Forms und Web-API installiert ist, die im Lieferumfang von Visual Studio 2013. Sie können dieses NuGet-Paket in der NuGet Gallery herunterladen.
    - Freigeben von ASP.NET Identity als NuGet-erleichtert Paket für das Team ASP.NET beurteilen Sie den neuen Funktionen und Fehlerkorrekturen und übermittelt diese für Entwickler auf flexible Weise.

## <a name="getting-started-with-aspnet-identity"></a>Erste Schritte mit ASP.NET Identity

ASP.NET Identity wird in der Visual Studio 2013-Projektvorlagen für ASP.NET MVC, Web Forms und Web-API SPA verwendet. In dieser exemplarischen Vorgehensweise veranschaulichen, wie die Projektvorlagen ASP.NET Identity zu verwenden, um Funktionen zu registrieren, melden Sie sich hinzufügen und wir einen Benutzer abmelden.

ASP.NET Identity wird mithilfe des folgenden Verfahrens implementiert. Der Zweck dieses Artikels ist, der Ihnen einer grobe Übersicht der ASP.NET Identity; Sie führen Sie ihn schrittweise oder nur die Details für das Lesen. Ausführlichere Anweisungen zum Erstellen von apps mithilfe von ASP.NET Identity, einschließlich der Verwendung der neuen API zum Hinzufügen von Benutzern, Rollen und Profilinformationen, finden Sie im Abschnitt "Nächste Schritte" am Ende dieses Artikels.

1. Erstellen einer ASP.NET MVC-Anwendung mit individuellen Konten an. Sie können ASP.NET Identity in ASP.NET MVC, Web Forms, Web API SignalR usw. verwenden. In diesem Artikel wird es mit einer ASP.NET MVC-Anwendung gestartet.  
  
    ![](introduction-to-aspnet-identity/_static/image1.png)
2. Das Projekt enthält die folgenden drei Pakete für ASP.NET Identity.

    - [`Microsoft.AspNet.Identity.EntityFramework`](http://www.nuget.org/packages/Microsoft.AspNet.Identity.EntityFramework/)  
 Dieses Paket ist das Entity Framework-Implementierung von ASP.NET Identity, die die ASP.NET Identity-Daten und das Schema mit SQL Server beibehalten wird.
    - [`Microsoft.AspNet.Identity.Core`](http://www.nuget.org/packages/Microsoft.AspNet.Identity.Core/)  
 Dieses Paket enthält die Core-Schnittstellen für ASP.NET Identity. Dieses Paket kann verwendet werden, um einer Implementierung für ASP.NET Identity schreiben können, die Ziele unterschiedliche Persistenz z. B. Azure Table Storage NoSQL-Datenbanken usw. gespeichert.
    - [`Microsoft.AspNet.Identity.OWIN`](http://www.nuget.org/packages/Microsoft.AspNet.Identity.Owin/)  
 Dieses Paket enthält Funktionen, die verwendet wird, um die OWIN-Authentifizierung mit ASP.NET Identity in ASP.NET-Anwendungen einbinden. Wird verwendet, wenn Sie Protokoll Funktionalität Ihrer Anwendung und rufen Sie Cookieauthentifizierung OWIN-Middleware zum Generieren eines Cookies hinzufügen.
3. Erstellen eines Benutzers an.  
 Starten Sie die Anwendung, und klicken Sie dann auf die **registrieren** Link, um einen Benutzer zu erstellen. Die folgende Abbildung zeigt die Seite "Register" dem gesammelt, den Benutzernamen und das Kennwort werden an.  
  
    ![](introduction-to-aspnet-identity/_static/image2.png)  
  
 Wenn der Benutzer klickt der **registrieren** Schaltfläche, die `Register` des kontocontrollers wird den Benutzer durch Aufrufen der mit ASP.NET Identity-API, wie unten markiert:

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample1.cs?highlight=8-9)]
4. Anmelden.  
 Wenn der Benutzer erfolgreich erstellt wurde wird, er vom protokolliert wird der `SignInAsync` Methode.  

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample2.cs?highlight=12)]

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample3.cs?highlight=5-6)]

 Der hervorgehobene Code oben in der `SignInAsync` Methode generiert eine ["ClaimsIdentity"](https://msdn.microsoft.com/en-us/library/system.security.claims.claimsidentity.aspx). Da ASP.NET Identity und OWIN Cookieauthentifizierung anspruchsbasiertes System sind, erfordert das Framework die app, eine "ClaimsIdentity" für den Benutzer zu generieren. "ClaimsIdentity" hat Informationen über alle Ansprüche für den Benutzer, wie z. B. welche Rollen, die der Benutzer angehört. Sie können auch weitere Ansprüche für den Benutzer zu diesem Zeitpunkt hinzufügen.  
  
 Der hervorgehobene Code unten in der `SignInAsync` Methode meldet sich der Benutzer mithilfe der AuthenticationManager aus aufrufen und OWIN `SignIn` und die "ClaimsIdentity" übergeben.  

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample4.cs?highlight=8-11)]
5. Melden Sie sich ab.  
 Klicken auf die **Abmelden** Link Ruft die Abmeldung Aktion in den Kontocontroller. 

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample5.cs?highlight=6)]

 Der hervorgehobene code oben zeigt das OWIN `AuthenticationManager.SignOut` Methode. Dies ist analog zu [FormsAuthentication.SignOut](https://msdn.microsoft.com/en-us/library/system.web.security.formsauthentication.signout.aspx) Methode, mit der die [FormsAuthentication](https://msdn.microsoft.com/en-us/library/system.web.security.formsauthenticationmodule.aspx) Modul in Web Forms.

## <a name="components-of-aspnet-identity"></a>Komponenten des ASP.NET Identity

Das folgende Diagramm zeigt die Komponenten des Systems ASP.NET Identity (klicken Sie auf [dies](introduction-to-aspnet-identity/_static/image3.png) oder auf das Diagramm, um es zu vergrößern). Die Pakete in Grün bilden zusammen die ASP.NET Identity-System. Die anderen Pakete sind die Abhängigkeiten, die benötigt werden, das ASP.NET Identity-System in ASP.NET-Anwendungen verwenden.

[![](introduction-to-aspnet-identity/_static/image5.png)](introduction-to-aspnet-identity/_static/image4.png)

Im folgenden finden eine kurze Beschreibung des NuGet-Pakete, die nicht zuvor erwähnt:

- [Microsoft.Owin.Security.Cookies](http://www.nuget.org/packages/Microsoft.Owin.Security.Cookies/)  
 Middleware, ermöglicht der Anwendung für die Verwendung von Cookie-basierte Authentifizierung, ASP ähnelt. NET Formularauthentifizierung.
- [EntityFramework](http://www.nuget.org/packages/EntityFramework/)  
 Entity Framework ist Microsofts empfohlene datenzugriffstechnologie für relationale Datenbanken.

## <a name="migrating-from-membership-to-aspnet-identity"></a>Migrieren aus ihrer Zugehörigkeit zu ASP.NET Identity

Wir hoffen, bald bieten eine Anleitung zum Migrieren Ihrer vorhandenen apps, die ASP.NET-Mitgliedschaft oder einfache Mitgliedschaft auf das neue ASP.NET Identity-System zu verwenden.

## <a name="next-steps"></a>Nächste Schritte

- [Erstellen einer ASP.NET MVC 5-App mit Facebook und Google "oauth2" und OpenID-Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)  
 Das Lernprogramm verwendet die ASP.NET Identity-API Hinzufügen von Profilinformationen mit der Benutzerdatenbank und wie für die Authentifizierung bei Google und Facebook.
- [Eine ASP.NET MVC-app mit Authentifizierung und SQL-Datenbank erstellen und Bereitstellen von Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)  
 Dieses Lernprogramm veranschaulicht, wie die Identity-API zu verwenden, um Benutzer und Rollen hinzuzufügen.
- [Einzelne Benutzerkonten](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#indauth) erstellen ASP.NET Web-Projekte in Visual Studio 2013
- [Organisationskonten](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#orgauth) erstellen ASP.NET Web-Projekte in Visual Studio 2013
- [Anpassen von Profilinformationen in ASP.NET Identity in Visual Studio 2013-Vorlagen](https://blogs.msdn.com/b/webdev/archive/2013/10/16/customizing-profile-information-in-asp-net-identity-in-vs-2013-templates.aspx)
- [Abrufen von Informationen von Anbietern sozialer verwendet in der Visual Studio 2013-Projektvorlagen](https://blogs.msdn.com/b/webdev/archive/2013/10/16/get-more-information-from-social-providers-used-in-the-vs-2013-project-templates.aspx)
- [https://github.com/rustd/AspnetIdentitySample](https://github.com/rustd/AspnetIdentitySample)  
 Eine beispielanwendung, die zeigt, wie grundlegende Rollen und Benutzer hinzufügen und wie Rollen und die benutzerverwaltung ausgeführt werden.
