---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-vb
title: Unterschiede zwischen IIS und ASP.NET Development Server (VB) Core | Microsoft-Dokumentation
author: rick-anderson
description: Wenn Sie eine ASP.NET-Anwendung lokal zu testen, ist es wahrscheinlich, dass Sie den ASP.NET Development Web Server verwenden. Der Produktionswebsite ist jedoch wahrscheinlich pow...
ms.author: aspnetcontent
ms.date: 04/01/2009
ms.assetid: 090e9205-52f3-4d72-ae31-44775b8b8421
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-vb
msc.type: authoredcontent
ms.openlocfilehash: 586bc45a44e773c3097de0959411a2d27098459a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37821089"
---
<a name="core-differences-between-iis-and-the-aspnet-development-server-vb"></a>Wichtige Unterschiede zwischen IIS und ASP.NET Development Server (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_06_VB.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial06_WebServerDiff_vb.pdf)

> Wenn Sie eine ASP.NET-Anwendung lokal zu testen, ist es wahrscheinlich, dass Sie den ASP.NET Development Web Server verwenden. Der Produktionswebsite ist jedoch wahrscheinlich ausgeschaltet IIS. Es gibt einige Unterschiede zwischen wie diese Webserver Anforderungen verarbeiten, und diese Unterschiede können wichtige Folgen haben. Dieses Tutorial wird beschrieben, einige der Unterschiede mehr von Belang.


## <a name="introduction"></a>Einführung

Wenn ein Benutzer eine ASP.NET-Anwendung besucht sendet sendet der Browser eine Anforderung an die Website an. Diese Anforderung wird durch die Webserver-Software, die mit der ASP.NET-Laufzeit zum Generieren und Zurückgeben des Inhalts für die angeforderte Ressource koordiniert übernommen. Die[**ich** Assistenten **ich** Nformation **S** Ervices (IIS)](http://en.wikipedia.org/wiki/Internet_Information_Services) sind eine Sammlung von Diensten, die allgemeine Funktionalität, die für die internetbasierte bereitstellen Windows Server. IIS ist der am häufigsten verwendete Webserver für ASP.NET-Anwendungen in produktionsumgebungen. Es ist wahrscheinlich die Webserver-Software, die von Ihrem Web-Host-Anbieter verwendet wird, um Ihre ASP.NET-Anwendung zu verarbeiten. IIS kann auch als die Webserver-Software in der Entwicklungsumgebung verwendet werden, obwohl dies beinhaltet die Installation von IIS und ordnungsgemäß konfigurieren.


Der ASP.NET Development Server ist eine alternative Web-Server-Option für die Entwicklungsumgebung. im Lieferumfang von dabei, die in Visual Studio integriert ist. Wenn die Webanwendung so konfiguriert wurde, um IIS zu verwenden, den ASP.NET Development Server automatisch gestartet und der ersten Besuch einer Webseite von Visual Studio als Webserver verwendet. Die Demo-Webanwendungen, die wir erstellt, wieder in haben die [ *bestimmen was Dateien müssen bereitgestellt werden* ](determining-what-files-need-to-be-deployed-vb.md) Tutorial wurden beide Datei System-basierte Webanwendungen, die nicht konfiguriert wurden, um IIS zu verwenden. Aus diesem Grund wird beim Zugriff auf eine der folgenden Websites in Visual Studio den ASP.NET Development Server verwendet.

In einer perfekten Welt würden die Umgebungen für Entwicklungs- und produktionsumgebungen identisch sein. Wie im vorherigen Lernprogramm erläutert ist es jedoch nicht ungewöhnlich, dass für die Umgebungen auf unterschiedliche Konfigurationseinstellungen haben. Mit anderen Webserver-Software in den Umgebungen Fügt eine weitere Variable, die berücksichtigt werden muss beim Bereitstellen einer Anwendung hinzu. Dieses Tutorial behandelt die wichtigsten Unterschiede zwischen IIS und ASP.NET Development Server. Aufgrund dieser Unterschiede werden Szenarien, in dem Code, der sich problemlos in die Entwicklungsumgebung ausgeführt wird. eine Ausnahme auslöst oder verhält sich anders, wenn in der Produktion ausgeführt.

## <a name="security-context-differences"></a>Security-Kontext-Unterschiede

Wenn die Webserver-Software auf eine eingehende Anforderung verarbeitet ordnet es die Anforderung mit einem bestimmten Sicherheitskontext. Diese Kontextinformationen für die Sicherheit wird vom Betriebssystem verwendet, um zu bestimmen, welche Aktionen zulässig sind von der Anforderung. Beispielsweise könnte eine ASP.NET-Seite Code enthalten, die eine Nachricht in eine Datei auf dem Datenträger protokolliert. Damit diese ASP.NET-Seite ohne Fehler ausgeführt werden kann müssen der Sicherheitskontext die entsprechenden Dateiberechtigungen auf Systemebene, d. h. Schreibberechtigungen für diese Datei.

Der ASP.NET Development Server ordnet eingehende Anforderungen mit dem Sicherheitskontext des angemeldeten Benutzers. Wenn Sie auf Ihrem Desktop als Administrator angemeldet sind, müssen die ASP.NET-Seiten, die von der ASP.NET Development Server bedient die gleichen Zugriffsrechte als Administrator. ASP.NET-Anforderungen von IIS behandelt sind jedoch mit einem bestimmten Computer-Konto verknüpft. Standardmäßig wird das Computerkonto für den Netzwerkdienst von IIS, Version 6 und 7 verwendet, obwohl Ihre Webhostinganbieter ein eindeutiges Konto für jeden Kunden konfiguriert haben, kann. Ihre Webhostinganbieter erhält, wahrscheinlich eingeschränkte Berechtigungen auf diese Computerkonten. Das Ergebnis ist, dass möglicherweise Webseiten, die ohne Fehler in der Entwicklungsumgebung ausgeführt, aber die Authorization-bezogenen Ausnahmen beim Hosten in der produktionsumgebung zu generieren.

Auf diese Art von Fehler in Aktion zeigen, habe ich der Book Reviews-Website, die eine Datei auf Datenträger, die das aktuelle Datum und die Uhrzeit speichert erstellt ein Benutzer eine Seite angezeigt. die *bringen Sie sich ASP.NET 3.5 in 24 Stunden* überprüfen. Um folgen zu können, öffnen Sie die `~/Tech/TYASP35.aspx` Seite, und fügen Sie den folgenden Code der `Page_Load` -Ereignishandler:

[!code-vb[Main](core-differences-between-iis-and-the-asp-net-development-server-vb/samples/sample1.vb)]

> [!NOTE]
> Die [ `File.WriteAllText` Methode](https://msdn.microsoft.com/library/system.io.file.writealltext.aspx) erstellt eine neue Datei, wenn es ist nicht vorhanden, und klicken Sie dann den angegebenen Inhalt, in die Datei schreibt. Wenn die Datei bereits vorhanden ist, wird der vorhandene Inhalt überschrieben.


Als Nächstes finden Sie auf die *bringen Sie sich ASP.NET 3.5 in 24 Stunden* Buch Seite "Überprüfen" in der Entwicklungsumgebung, die mithilfe von ASP.NET Development Server. Vorausgesetzt, dass Sie angemeldet sind auf Ihrem Computer mit einem Konto mit ausreichenden Berechtigungen zum Erstellen und ändern eine Textdatei, in der Web-Stammverzeichnis der Anwendung der buchbewertung angezeigt wird der gleiche wie zuvor, aber jedes Mal, wenn die Seite wird besucht, das Datum und Uhrzeit und des Benutzers  IP-Adresse befindet sich in der `LastTYASP35Access.txt` Datei. Zeigen Sie Ihren Browser zu dieser Datei; Daraufhin sollte eine Meldung wie in Abbildung 1 dargestellt.


[![Die Textdatei enthält, das letzte Datum und die Uhrzeit, die der Buchbewertung besucht&lt;](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image2.png)](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image1.png)

**Abbildung 1**: die Textdatei enthält, das Datum der letzten und die Uhrzeit der Buchbewertung war aufgerufen ([klicken Sie, um das Bild in voller Größe anzeigen](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image3.png))


Bereitstellen der Webanwendung für die Produktion, und besuchen Sie das gehostete *bringen Sie sich ASP.NET 3.5 in 24 Stunden* Buch Seite "Überprüfen". An diesem Punkt sollte die Seite "Überprüfen Buch" entweder als Normal oder die Fehlermeldung, die in Abbildung 2 dargestellt angezeigt werden. Einige Anbieter von Host erteilen Sie Schreibberechtigungen für das anonyme ASP.NET-Computerkonto, in dem Fall die Seite ohne Fehler funktionieren. Wenn Sie jedoch Ihre Webhostinganbieter Schreibzugriff für das anonyme Konto verhindert, dass ein [ `UnauthorizedAccessException` Ausnahme](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx) ausgelöst wird, wenn die `TYASP35.aspx` Seite versucht, auf das aktuelle Datum und Zeit für Schreiben der `LastTYASP35Access.txt` Datei.


[![Das von IIS verwendete Standard-Computerkonto ist nicht berechtigt, auf das Dateisystem schreiben](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image5.png)](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image4.png)

**Abbildung 2**: die Standard-Computer Konto verwendet von IIS wird nicht haben Berechtigungen zum Schreiben in das Dateisystem ([klicken Sie, um das Bild in voller Größe anzeigen](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image6.png))


Die gute Nachricht ist, dass die meisten Web-Host-Anbieter eine Art von Tool Berechtigungen, die Ihnen ermöglicht, Dateisystemberechtigungen auf Ihrer Website anzugeben. Gewähren Sie dem anonymen ASP.NET Konto Schreibzugriff auf das Stammverzeichnis, und klicken Sie dann noch einmal die Buch-Seite "Überprüfen". (Bei Bedarf wenden Sie sich an Ihre Web-Host-Anbieter um Unterstützung zu erhalten, auf wie Schreibberechtigungen für das Standardkonto für ASP.NET gewährt.) Dieses Mal sollte die Seite ohne Fehler geladen und die `LastTYASP35Access.txt` sollte die Datei erfolgreich erstellt werden.

Die Lektion ist hierbei, dass da ASP.NET Development Server unter einem anderen Sicherheitskontext als dem IIS ausgeführt wird, es möglich ist, dass Ihre ASP.NET-Seiten, die lesen oder Schreiben in das Dateisystem auslesen oder in das Windows-Ereignisprotokoll zu schreiben oder lesen oder Schreiben in die Windows  Registrierung wird erwartungsgemäß zur Entwicklung von aber generieren Ausnahmen, wenn in der Produktion. Beim Erstellen einer Webanwendung, die auf eine freigegebene Webhosting-Umgebung bereitgestellt wird, nicht lesen oder Schreiben in das Ereignisprotokoll geschrieben oder die Windows-Registrierung. Notieren Sie auch sich alle ASP.NET-Seiten, die lesen oder Schreiben in das Dateisystem, wie Sie möglicherweise zum Gewähren von Lese- und Schreibberechtigungen für die entsprechenden Ordner, in der produktionsumgebung benötigen.

## <a name="differences-on-serving-static-content"></a>Unterschiede für das Verarbeiten von statischen Inhalten

Eine andere Hauptunterschied zwischen IIS und ASP.NET Development Server ist, wie sie Anforderungen für statische Inhalte behandeln. Jede Anforderung, der in ASP.NET Development Server, sei es für eine ASP.NET-Seite, ein Bild oder einer JavaScript-Datei enthalten ist, wird von der ASP.NET-Laufzeit verarbeitet. Standardmäßig ruft IIS nur die ASP.NET-Laufzeit, wenn eine Anforderung für eine ASP.NET-Ressource, z. B. einer ASP.NET-Webseite, ein Webdienst usw. eingeht. Anforderungen für statische Inhalte – Bilder, CSS-Dateien, JavaScript-Dateien, PDF-Dateien, ZIP-Dateien und ähnliches – werden von IIS abgerufen, ohne die Beteiligung der ASP.NET-Laufzeit. (Es ist möglich, weisen Sie IIS so arbeiten mit der ASP.NET-Laufzeit beim Bereitstellen von statischen Inhalts, wenden Sie sich an den Abschnitt "Performing formularbasierte Authentifizierung und Authentifizierung der URL auf statische Dateien mit IIS 7" in diesem Tutorial Weitere Informationen.)

Die ASP.NET-Laufzeit führt eine Reihe von Schritten zum Generieren des angeforderten Inhalts, einschließlich (zur Identifizierung des anforderers)-Authentifizierung und Autorisierung (festlegen, wenn der anforderer die Berechtigung zum Anzeigen des angeforderten Inhalts wurde). Eine gängige Form der Authentifizierung ist *formularbasierte Authentifizierung*, in denen ein Benutzer durch Eingabe ihrer Anmeldeinformationen – in der Regel einen Benutzernamen und Kennwort – in Textfelder ein auf einer Webseite identifiziert wird. Beim Überprüfen ihre Anmeldeinformationen ein, die Website speichert eine *Authentifizierungsticket* Cookie auf den Browser des Benutzers, wird bei jeder nachfolgenden Anfrage an die Website gesendet und wird verwendet, um den Benutzer zu authentifizieren. Darüber hinaus ist es möglich, geben Sie *URL-Autorisierung* Regeln, die festlegen, welche Benutzer zugreifen kann oder keinen bestimmten Ordner. Viele ASP.NET-Websites verwenden formularbasierte Authentifizierung und URL-Autorisierung, um Benutzerkonten zu unterstützen und zu teilen der Site zu definieren, die nur für authentifizierte Benutzer oder Benutzer, die zu einer bestimmten Rolle gehören zugänglich sind.

> [!NOTE]
> Für eine gründliche Prüfung von ASP zu gewährleisten. NET formularbasierte Authentifizierung, URL-Autorisierung und andere Benutzer-Konto-Funktionen werden möchten, lesen Sie meine [Website-Lernprogramme zur ASP.NET-Sicherheit](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).


Erwägen Sie eine Website, die Benutzerkonten mithilfe von Forms basierende Autorisierung unterstützt und verfügt über einen Ordner aus, der mithilfe von URL-Autorisierung, damit, dass nur authentifizierte Benutzer konfiguriert ist. Stellen Sie sich, dass dieser Ordner enthält die ASP.NET-Seiten und PDF-Dateien und die Absicht, dass ist, die nur authentifizierte Benutzer können diese PDF-Dateien anzeigen.

Was geschieht, wenn ein Besucher versucht wird, um eine PDF-Dateien anzeigen, indem Sie die URL direkt in die Adressleiste des Browsers eingeben? Um herauszufinden, wir erstellen Sie einen neuen Ordner am Standort Book Reviews, eine PDF-Dateien hinzufügen und Konfigurieren des Standorts für die URL-Autorisierung verwenden, um zu verhindern, dass anonyme Benutzer Zugriff auf diesen Ordner. Wenn Sie die demoanwendung herunterladen werden Sie sehen, dass ich einen Ordner namens erstellt `PrivateDocs` und PDF-Datei aus meinem [Website-Lernprogramme zur ASP.NET-Sicherheit](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md) (wie schließendem!). Die `PrivateDocs` Ordner enthält auch eine `Web.config` -Datei, die URL-Autorisierungsregeln zum Verweigern von anonymen Benutzer angibt:

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-vb/samples/sample2.xml)]

Schließlich konfiguriert die Webanwendung, die formularbasierte Authentifizierung verwendet wird, durch die Aktualisierung der Datei "Web.config" im Stammverzeichnis, und Ersetzen Sie dabei:

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-vb/samples/sample3.xml)]

Durch:

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-vb/samples/sample4.xml)]

Mithilfe von ASP.NET Development Server, von der Website, und geben Sie die direkte URL einer PDF-Dateien in der Adressleiste des Browsers. Wenn Sie heruntergeladen die Website verknüpft ist, in diesem Tutorial aus, die die URL, die etwa so aussehen sollte haben: `http://localhost:portNumber/PrivateDocs/aspnet_tutorial01_Basics_vb.pdf`

Diese URL in die Adressleiste eingeben bewirkt, dass den Browser eine Anforderung an den ASP.NET Development Server für die Datei senden. Der ASP.NET Development Server die Anforderung an die ASP.NET-Laufzeitumgebung zur Verarbeitung übergibt. Da wir noch nicht angemeldet haben und die `Web.config` in die `PrivateDocs` Ordner ist so konfiguriert, dass Sie um den anonymen Zugriff zu verweigern, die ASP.NET-Laufzeit leitet uns automatisch an die Anmeldeseite, `Login.aspx` (siehe Abbildung 3). Wenn den Benutzer auf der Anmeldeseite umleiten, enthält ASP.NET eine `ReturnUrl` Querystring-Parameter, der die Seite angibt. der Benutzer hat versucht, anzeigen. Nach der erfolgreichen Anmeldung der Benutzer kann auf diese Seite zurückgegeben werden.


[![Nicht autorisierte Benutzer werden automatisch zur Anmeldeseite umgeleitet.](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image8.png)](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image7.png)

**Abbildung 3**: nicht autorisierte Benutzer werden automatisch zur Anmeldeseite weitergeleitet ([klicken Sie, um das Bild in voller Größe anzeigen](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image9.png))


Jetzt sehen wir uns an, wie sich dies auf Produktions-verhält. Bereitstellen Ihrer Anwendung, und geben Sie die direkte URL einer PDF-Dateien in die `PrivateDocs` Ordner in der Produktion. Dadurch wird Ihr Browser zum Senden einer Anforderung IIS für die Datei. Da eine statische Datei angefordert wird, wird IIS abgerufen und gibt die Datei zurück, ohne die ASP.NET-Laufzeit aufzurufen. Daher wurde keine URL-Autorisierung-Überprüfung ausgeführt. die Inhalte der angeblich private PDF-Datei sind für alle Benutzer, die weiß, die direkte URL in die Datei zugegriffen werden kann.


[![Anonyme Benutzer können die Private PDF-Dateien herunterladen, indem Sie die direkte URL und die Datei eingeben](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image11.png)](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image10.png)

**Abbildung 4**: anonyme Benutzer können die privaten PDF-Dateien durch Eingabe der direkte URL in die Datei ([klicken Sie, um das Bild in voller Größe anzeigen](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image12.png))


### <a name="performing-forms-based-authentication-and-url-authentication-on-static-files-with-iis-7"></a>Ausführen von formularbasierte Authentifizierung und die URL-Authentifizierung für statische Dateien mit IIS 7

Es gibt eine Reihe von Techniken, die Sie verwenden können, um statische Inhalte vor unberechtigten Benutzern schützen. IIS 7 eingeführt, die *integrierten Pipeline*, die IIS-Workflow mit der ASP.NET-Laufzeit Workflow heiratet. Kurz gesagt, können Sie IIS für die ASP.NET-Laufzeitumgebung-Authentifizierung und Autorisierungsmodule rufen Sie alle eingehenden Anforderungen (einschließlich statische Inhalte wie PDF-Dateien) anweisen. Wenden Sie sich an Ihre Web-Host-Anbieter finden Sie heraus, wie Sie die Website zum Verwenden der integrierten Pipeline konfigurieren.

Nach der Verwendung von IIS konfiguriert wurde, hinzufügen die integrierte Pipeline das folgende Markup zu der `Web.config` Datei im Stammverzeichnis befindet:

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-vb/samples/sample5.xml)]

Dieses Markup wird angewiesen, IIS 7, die ASP.NET basierende Authentifizierung und Autorisierung-Module verwenden. Stellen Sie Ihre Anwendung erneut bereit, und besuchen dann die PDF-Datei erneut. Dieses Mal, wenn IIS eine Anforderung verarbeitet, erhalten die ASP.NET-Laufzeit Authentifizierung und Autorisierung Logik eine Möglichkeit, die Anforderung zu überprüfen. Da nur authentifizierte Benutzer, zum Anzeigen des Inhalts in autorisiert sind der `PrivateDocs` Ordner, der anonyme Besucher automatisch an die Anmeldeseite umgeleitet (siehe Abbildung 3).

> [!NOTE]
> Wenn Ihre Webhostinganbieter noch IIS 6 verwendet können nicht Sie die integrierte Pipeline-Funktion verwenden. Eine problemumgehung besteht darin, die private Dokumente in einem Ordner abgelegt, die verhindert, HTTP-Zugriff dass (z. B. `App_Data`) und erstellen Sie dann auf eine Seite, um diese Dokumente zu verarbeiten. Diese Seite aufgerufen werden könnte `GetPDF.aspx`, und der Name der PDF-Datei über einen Abfragezeichenfolgeparameter übergeben wird. Die `GetPDF.aspx` Seite würden zuerst stellen Sie sicher, dass der Benutzer verfügt über die Berechtigung zum Anzeigen der Datei, wenn dies der Fall ist, verwenden die [ `Response.WriteFile(filePath)` ](https://msdn.microsoft.com/library/system.web.httpresponse.writefile.aspx) Methode, um den Inhalt der angeforderten PDF-Datei an den anfordernden Client zu senden. Dieses Verfahren funktioniert auch für IIS 7, wenn Sie nicht die integrierte Pipeline aktivieren möchten.


## <a name="summary"></a>Zusammenfassung

Die gehostete Webanwendungen in einer produktionsumgebung werden mithilfe von Microsoft IIS Webserver-Software. In der Entwicklungsumgebung möglicherweise jedoch die Anwendung gehostet werden mithilfe von IIS oder ASP.NET Development Server. Im Idealfall sollte der gleiche Webserver-Software in beiden Umgebungen verwendet werden, da eine andere Variable mit anderer Software in der testmischung hinzugefügt werden. Er stellt jedoch die benutzerfreundlichkeit des ASP.NET Development Server eine attraktive Möglichkeit in der Entwicklungsumgebung. Die gute Nachricht ist, dass es nur ein paar grundlegende Unterschiede zwischen IIS und ASP.NET Development Server, wenn Sie sich dieser Unterschiede bewusst sind. Sie sorgen dafür können, um sicherzustellen, dass die Anwendung funktioniert und die gleiche Funktion wie unabhängig von der Umgebung.

Viel Spaß beim Programmieren!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Tutorial erläutert finden Sie in den folgenden Ressourcen:

- [Integration von ASP.NET in IIS 7.0](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis)
- [Alle Arten von Inhalten auf IIS 7-Foren ASP.NET-Authentifizierung mit](https://blogs.iis.net/bills/archive/2007/05/19/using-asp-net-forms-authentication-with-all-types-of-content-with-iis7-video.aspx) (Video)
- [Webserver in Visual Web Developer](https://msdn.microsoft.com/library/58wxa9w5.aspx)

> [!div class="step-by-step"]
> [Zurück](common-configuration-differences-between-development-and-production-vb.md)
> [Weiter](deploying-a-database-vb.md)
