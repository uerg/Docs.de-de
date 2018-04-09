---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs
title: Unterschiede zwischen IIS und ASP.NET Development Server (c#) Core | Microsoft Docs
author: rick-anderson
description: Wenn Sie eine ASP.NET-Anwendung lokal zu testen, sind wahrscheinlich, dass Sie den ASP.NET Development Web Server verwenden. Der Produktionswebsite ist jedoch sehr wahrscheinlich pow...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: 13a5a423-9235-4dde-b408-2fd10f791d63
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs
msc.type: authoredcontent
ms.openlocfilehash: e343a6eac39d7959718cb791012cfa3b931ae33f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="core-differences-between-iis-and-the-aspnet-development-server-c"></a>Core-Unterschiede zwischen IIS und ASP.NET Development Server (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Herunterladen von Code](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_06_CS.zip) oder [PDF herunterladen](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial06_WebServerDiff_cs.pdf)

> Wenn Sie eine ASP.NET-Anwendung lokal zu testen, sind wahrscheinlich, dass Sie den ASP.NET Development Web Server verwenden. Der Produktionswebsite ist jedoch sehr wahrscheinlich ausgeschaltet IIS. Es gibt einige Unterschiede zwischen den Umgang mit diesen Webserver Anforderungen, und diese Unterschiede können wichtige Auswirkungen haben. In diesem Lernprogramm wird erklärt, einige der Unterschiede mehr von Belang.


## <a name="introduction"></a>Einführung

Wenn ein Benutzer eine ASP.NET-Anwendung besucht sendet seinen Browser eine Anforderung an die Website an. Diese Anforderung wird von der Web-Server-Software übernommen, die Koordination mit die ASP.NET-Laufzeit zum Generieren und den Inhalt für die angeforderte Ressource zurückgibt. Die[**ich** Assistenten **ich** Nformation **S** Ervices (IIS)](http://en.wikipedia.org/wiki/Internet_Information_Services) sind eine Sammlung von Diensten, die allgemeine Funktionalität, die für die internetbasierte bereitstellen Windows-Servern. IIS ist die am häufigsten verwendeten Webserver für ASP.NET-Anwendungen in produktionsumgebungen. Es ist sehr wahrscheinlich die Webserver-Software wird von Ihrem Web-Host-Anbieter verwendet, um Ihre ASP.NET-Anwendung zu dienen. IIS kann auch als die Webserver-Software in der Entwicklungsumgebung verwendet werden, obwohl dabei müssen Sie IIS installieren und konfigurieren sie ordnungsgemäß.


Der ASP.NET Development Server ist eine alternative Web-Server-Option für die Entwicklungsumgebung; im Lieferumfang von, und in Visual Studio integriert ist. Wenn die Webanwendung für die Verwendung von IIS konfiguriert wurde, wird der ASP.NET Development Server automatisch gestartet und verwendet wie der Webserver beim ersten einer Webseite in Visual Studio Besuch. Die Demo-Webanwendungen, die wir erstellt, wieder in haben die [ *bestimmen was-Dateien müssen bereitgestellt werden* ](determining-what-files-need-to-be-deployed-cs.md) Lernprogramm wurden beide Datei systembasierten Webanwendungen, die nicht zur Verwendung von IIS konfiguriert wurden. Aus diesem Grund wird beim entweder diese Websites von innerhalb von Visual Studio Zugriff auf den ASP.NET Development Server verwendet.

Idealerweise würde die Entwicklungs- und produktionsumgebungen Umgebungen identisch sein. Wie im vorherigen Lernprogramm erläutert ist es jedoch nicht ungewöhnlich, dass die Umgebungen, die unterschiedliche Konfigurationseinstellungen haben. Mit anderen Web-Server-Software in den Umgebungen Fügt eine andere Variable, die berücksichtigt werden muss, wenn eine Anwendung bereitstellen. In diesem Lernprogramm werden die Hauptunterschiede zwischen IIS und ASP.NET Development Server behandelt. Aufgrund dieser Unterschiede gibt es Szenarien, in dem Code, in der Entwicklungsumgebung fehlerlos ausgeführt wird, löst eine Ausnahme aus, oder verhält sich anders, wenn in der produktionsumgebung ausgeführt, werden.

## <a name="security-context-differences"></a>Sicherheit Kontext Unterschiede

Wenn die Web-Server-Software auf eine eingehende Anforderung verarbeitet ordnet es diese Anforderung mit einem bestimmten Sicherheitskontext. Diese Sicherheitskontextinformationen wird vom Betriebssystem verwendet, um zu bestimmen, welche Aktionen zulässig sind von der Anforderung. Eine ASP.NET-Seite kann z. B. Code enthalten, die eine Nachricht in eine Datei auf dem Datenträger protokolliert. Damit diese ASP.NET-Seite ohne Fehler ausgeführt muss der Sicherheitskontext müssen die entsprechenden Dateiberechtigungen auf Systemebene, nämlich Schreibberechtigungen für diese Datei.

Der ASP.NET Development Server ordnet eingehende Anforderungen mit dem Sicherheitskontext des angemeldeten Benutzers. Wenn Sie auf Ihrem Desktop als Administrator angemeldet sind, müssen die von den ASP.NET Development Server bedient ASP.NET-Seiten dieselben Zugriffsrechte als Administrator. Allerdings werden von IIS behandelt ASP.NET-Anforderungen ein bestimmtes Computerkonto zugeordnet. Standardmäßig ist das NETZWERKDIENST-Konto "Machine" von IIS, Version 6 und 7, verwendet, obwohl Ihre Webhostinganbieter ein eindeutiges Konto für jeden Kunden konfiguriert haben, kann. Ihre Webhostinganbieter hat, wahrscheinlich eingeschränkte Berechtigungen für dieses Konto "Machine" erteilt. Das Ergebnis ist, dass möglicherweise Webseiten, die ohne Fehler in der Entwicklungsumgebung ausgeführt, aber die Autorisierung bezogenen Ausnahmen beim Hosten in der produktionsumgebung zu generieren.

Auf diese Art von Fehlern in Aktion zeigen ich erstellt habe eine Seite im Buch Reviews Website an, die erstellt eine Datei auf dem Datenträger, der das Datum und die Uhrzeit speichert eine Person angezeigt der *Schulen selbst ASP.NET 3.5 in 24 Stunden* überprüfen. Um nachzuvollziehen, öffnen Sie die `~/Tech/TYASP35.aspx` Seite, und fügen Sie den folgenden Code hinzu der `Page_Load` Ereignishandler:

[!code-csharp[Main](core-differences-between-iis-and-the-asp-net-development-server-cs/samples/sample1.cs)]

> [!NOTE]
> Die [ `File.WriteAllText` Methode](https://msdn.microsoft.com/library/system.io.file.writealltext.aspx) erstellt eine neue Datei, wenn er nicht vorhanden, und klicken Sie dann den angegebenen Inhalt schreibt dort hinein. Wenn die Datei bereits vorhanden ist, wird der vorhandene Inhalt überschrieben.


Als Nächstes finden Sie auf der *Schulen selbst ASP.NET 3.5 in 24 Stunden* Buch Seite "Überprüfen" in der Entwicklungsumgebung, die mithilfe von ASP.NET Development Server. Vorausgesetzt, dass Sie angemeldet sind auf Ihrem Computer mit einem Konto, verfügt über die erforderlichen Berechtigungen zum Erstellen und ändern eine Textdatei im Web, Stammverzeichnis der Anwendung die Buch-Überprüfung wird der gleiche wie zuvor, aber jedes Mal, wenn die Seite ist besucht hat, das Datum und Uhrzeit und des Benutzers  IP-Adresse befindet sich in der `LastTYASP35Access.txt` Datei. Zeigen Sie Ihren Browser auf diese Datei; Daraufhin sollte eine Meldung wie in Abbildung 1 dargestellt.


[![Die Textdatei enthält, das letzte Datum und Zeit der Überprüfung des Buchs besucht wurde](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image2.png)](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image1.png)

**Abbildung 1**: die Textdatei enthält, das letzte Datum und Uhrzeit die Buch-Überprüfung wurde besucht ([klicken Sie hier, um das Bild in voller Größe angezeigt](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image3.png))


Bereitstellen der Webanwendung bis hin zur Produktion und besuchen dann die gehosteten *Schulen selbst ASP.NET 3.5 in 24 Stunden* Buch Seite "Überprüfen". An diesem Punkt sollte die Buch-Seite "Überprüfen" entweder als Normal oder die Fehlermeldung, die in Abbildung 2 veranschaulichte angezeigt werden. Einige Web Hosten von Anbietern erteilen Sie Schreibberechtigungen für das anonyme Konto zur ASP.NET-Computer, in dem Fall die Seite ohne Fehler ausgeführt wird. Wenn Sie jedoch Ihre Webhostinganbieter Schreibzugriff für das anonyme Konto verhindert ein [ `UnauthorizedAccessException` Ausnahme](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx) ausgelöst wird, wenn die `TYASP35.aspx` Seite wird versucht, das aktuelle Datum und die Uhrzeit zum Schreiben der `LastTYASP35Access.txt` Datei.


[![Das standardmäßige Konto "Machine" von IIS verwendete verfügt nicht über Berechtigungen zum Schreiben in das Dateisystem](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image5.png)](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image4.png)

**Abbildung 2**: The Default Machine Konto verwendet von IIS ist nicht haben Berechtigungen zum Schreiben in das Dateisystem ([klicken Sie hier, um das Bild in voller Größe angezeigt](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image6.png))


Die gute Nachricht ist, dass die meisten Web Hostanbieter irgendeine Tool Berechtigungen verfügen, die Sie in Ihrer Website Dateisystemberechtigungen angeben können. Gewähren Sie den anonymen ASP.NET Konto Schreibzugriff auf das Stammverzeichnis, und rufen Sie die Seite "Buch überprüfen" erneut. (Bei Bedarf, wenden Sie sich an Ihre Webhostinganbieter um Unterstützung zu erhalten, auf wie Schreibberechtigungen für das Standardkonto für ASP.NET gewährt.) Dieses Mal die Seite sollte ohne Fehler geladen und die `LastTYASP35Access.txt` Datei erfolgreich erstellt werden soll.

Die Take sieht unterwegs, da es sich bei den ASP.NET Development Server unter einem anderen Sicherheitskontext als dem IIS ausgeführt wird, es möglich ist, dass Ihre ASP.NET-Seiten, die lesen oder Schreiben in das Dateisystem auslesen oder in das Windows-Ereignisprotokoll zu schreiben oder lesen und Schreiben in die Windows  Registrierung wird auf die Entwicklung erwartungsgemäß arbeiten jedoch generieren Ausnahmen, wenn Sie sich auf Produktion. Beim Erstellen einer Webanwendung, die auf eine freigegebene Webhosting-Umgebung bereitgestellt werden, nicht lesen und Schreiben in das Ereignisprotokoll oder der Windows-Registrierung. Außerdem sollten Sie alle ASP.NET-Seiten, die lesen oder Schreiben in das Dateisystem, wie Sie benötigen möglicherweise zum Gewähren von Lese- und Schreibberechtigungen für die entsprechenden Ordner in der produktionsumgebung.

## <a name="differences-on-serving-static-content"></a>Unterschiede auf statische Inhalte

Ein weiterer Core Unterschied zwischen IIS und ASP.NET Development Server ist, wie sie die Anforderungen für den statischen Inhalt behandeln. Jede Anforderung, die in den ASP.NET Development Server für eine ASP.NET-Seite, ein Bild oder eine JavaScript-Datei stammen, wird durch die ASP.NET-Laufzeit verarbeitet. Standardmäßig wird IIS nur die ASP.NET-Laufzeit aufgerufen, wenn eine Anforderung für eine ASP.NET-Ressource, z. B. eine ASP.NET-Webseite, ein Webdienst usw. eingehen. Bei statischen Inhalten - Bilder, CSS-Dateien, JavaScript-Dateien, PDF-Dateien, ZIP-Dateien und der Like - Anforderungen werden ohne Beteiligung der ASP.NET-Laufzeit von IIS abgerufen. (Es ist möglich, weisen Sie IIS für die mit die ASP.NET-Laufzeit beim Bereitstellen von statischen Inhalts, finden Sie im Abschnitt "Performing formularbasierte Authentifizierung und Authentifizierung der URL für statische Dateien mit IIS 7" in diesem Lernprogramm für Weitere Informationen.)

Die ASP.NET-Laufzeit führt eine Reihe von Schritten zum Generieren des angeforderten Inhalts, einschließlich (Identifizieren des anforderers)-Authentifizierung und Autorisierung (bestimmen, ob der anforderer die Berechtigung zum Anzeigen des angeforderten Inhalts hat). Eine gängige Form der Authentifizierung besteht *formularbasierte Authentifizierung*, in denen ein Benutzer durch Eingabe ihrer Anmeldeinformationen – in der Regel einen Benutzernamen und Kennwort – in Textfelder ein auf einer Webseite identifiziert wird. Beim Überprüfen ihre Anmeldeinformationen ein, die Website speichert eine *Authentifizierungsticket* Cookie auf den Browser des Benutzers, der mit jeder nachfolgende Anforderung an die Website gesendet und wird verwendet, um den Benutzer zu authentifizieren. Darüber hinaus ist es möglich, anzugeben *URL-Autorisierung* Regeln, die festlegen, welche Benutzer oder einen bestimmten Ordner kann nicht zugegriffen werden kann. Viele ASP.NET-Websites verwenden formularbasierte Authentifizierung und URL-Autorisierung, um Benutzerkonten zu unterstützen und die Teile des Standorts zu definieren, die nur für authentifizierte Benutzer oder Benutzer, die zu einer bestimmten Rolle gehören zugänglich sind.

> [!NOTE]
> Für eine gründliche Prüfung von ASP. NET formularbasierte Authentifizierung, URL-Autorisierung und andere Benutzer im Zusammenhang mit-Funktionen, achten Sie darauf, sehen Sie sich meine [Website Sicherheit Lernprogramme](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).


Erwägen Sie eine Website, die Benutzerkonten mithilfe von Formularen basierende Autorisierung unterstützt und verfügt über einen Ordner, der so konfiguriert ist, dass nur authentifizierte Benutzer zulassen, mit der URL-Autorisierung. Stellen Sie sich vor, dass dieser Ordner ASP.NET-Seiten enthält und PDF-Dateien und, dass die Absicht ist, die nur authentifizierte Benutzer können diese PDF-Dateien anzeigen.

Was geschieht, wenn ein Besucher versucht, eine dieser PDF-Dateien anzeigen, indem Sie die URL direkt in die Adressleiste des Browsers eingeben? Um herauszufinden, wir erstellen Sie einen neuen Ordner am Standort Buch Reviews einige PDF-Dateien hinzufügen und konfigurieren Sie die Website, um die URL-Autorisierung verwenden, um zu verhindern, dass anonyme Benutzer Zugriff auf diesen Ordner. Wenn Sie die demoanwendung herunterladen werden Sie sehen, dass ich erstellt habe einen Ordner namens `PrivateDocs` und PDF-Datei aus hinzugefügt meine [Website-Lernprogramme](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md) (wie geeignete!). Die `PrivateDocs` Ordner enthält auch eine `Web.config` -Datei, die URL-Autorisierungsregeln zum Verweigern von anonymer Benutzern angibt:

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-cs/samples/sample2.xml)]

Schließlich konfiguriert die Webanwendung aktualisieren, formularbasierte Authentifizierung mithilfe der `Web.config` -Datei in das Stammverzeichnis ersetzen:

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-cs/samples/sample3.xml)]

Durch:

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-cs/samples/sample4.xml)]

Mithilfe von ASP.NET Development Server, von der Website, und geben Sie die direkte URL auf einen der PDF-Dateien in der Adressleiste des Browsers angezeigt. Wenn Sie heruntergeladen haben, die Website verknüpft sind mit diesem Lernprogramm die URL sollte etwa wie folgt aus: `http://localhost:portNumber/PrivateDocs/aspnet_tutorial01_Basics_vb.pdf`

Diese URL in die Adressleiste eingeben bewirkt, dass der Browser eine Anforderung an den ASP.NET Development Server für die Datei senden. Der ASP.NET Development Server die Anforderung an die ASP.NET-Laufzeit zur Verarbeitung übergibt. Da es sich noch nicht angemeldet haben und die `Web.config` in der `PrivateDocs` Ordner für den anonymen Zugriff zu verweigern konfiguriert ist, den die ASP.NET-Laufzeit leitet uns automatisch zu der Anmeldeseite `Login.aspx` (siehe Abbildung 3). Wenn den Benutzer die Anmeldeseite umgeleitet, ASP.NET umfasst einen `ReturnUrl` Querystring-Parameter, der die Seite angezeigt wird der Benutzer wurde versucht, anzeigen. Nach der erfolgreichen Anmeldung der Benutzer kann auf dieser Seite zurückgegeben werden.


[![Nicht autorisierte Benutzer werden automatisch an die Anmeldeseite umgeleitet.](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image8.png)](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image7.png)

**Abbildung 3**: nicht autorisierte Benutzer werden automatisch an die Anmeldeseite umgeleitet ([klicken Sie hier, um das Bild in voller Größe angezeigt](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image9.png))


Jetzt sehen wir, wie dies in der Produktion verhält. Bereitstellen der Anwendung, und geben Sie die direkte URL zu einer PDF-Dateien in den `PrivateDocs` Ordner in der Produktion. Dadurch werden aufgefordert, Ihren Browser zum Senden einer Anforderung IIS für die Datei. Da eine statische Datei angefordert wird, wird IIS abgerufen und zurück, die Datei ohne die ASP.NET-Laufzeit aufrufen. Daher wurde keine URL-Autorisierung-Überprüfung ausgeführt. der Inhalt der eigentlich privaten PDF sind für jede Person, die direkte URL zur Datei kennt, zugänglich.


[![Anonyme Benutzer können die Private PDF-Dateien herunterladen, indem Sie die direkte URL zur Datei eingeben](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image11.png)](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image10.png)

**Abbildung 4**: anonyme Benutzer können das Private PDF-Dateien durch Eingeben der direkte URL in die Datei ([klicken Sie hier, um das Bild in voller Größe angezeigt](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image12.png))


### <a name="performing-forms-based-authentication-and-url-authentication-on-static-files-with-iis-7"></a>Ausführen von formularbasierte Authentifizierung und die URL-Authentifizierung für statische Dateien mit IIS 7

Es gibt eine Reihe von Techniken, die Sie verwenden können, um statische Inhalte vor unberechtigten Benutzern schützen. IIS 7 eingeführten der *integrierten Pipeline*, dem heiratet IISs-Workflow mit der ASP.NET-Laufzeit Workflow. Kurz gesagt, können Sie anweisen, IIS für die ASP.NET-Laufzeit Authentifizierung und Autorisierungsmodule alle eingehenden Anforderungen (einschließlich statischen Inhalt wie PDF-Dateien) aufrufen. Wenden Sie sich an Ihre Webhostinganbieter aus, um herauszufinden, wie so konfigurieren Sie Ihre Website Verwendung die integrierte Pipeline.

Nach der Verwendung von IIS konfiguriert wurde die integrierte Pipeline das folgende Markup zum Hinzufügen der `Web.config` Datei im Stammverzeichnis befindet:

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-cs/samples/sample5.xml)]

Dieses Markup weist IIS 7 der Module ASP.NET-basierten, Authentifizierung und Autorisierung verwendet werden. Stellen Sie die Anwendung erneut bereit, und besuchen Sie die PDF-Datei dann erneut. Dieses Mal, wenn IIS für die Anforderung verarbeitet, hat es die ASP.NET-Laufzeit Authentifizierung und Autorisierung Logik Gelegenheit, die Anforderung zu überprüfen. Da nur durch authentifizierte Benutzer, zum Anzeigen des Inhalts in autorisiert sind der `PrivateDocs` Ordner, der anonyme Besucher wird automatisch auf die Anmeldeseite umgeleitet (siehe Abbildung 3).

> [!NOTE]
> Wenn Ihre Webhostinganbieter noch IIS 6 verwendet, können nicht Sie die integrierten Pipeline-Funktion verwenden. Eine Lösung besteht darin, Ihre private Dokumente in einem Ordner abgelegt, die HTTP-Zugriff untersagt (z. B. `App_Data`), und erstellen Sie eine Seite, um diese Dokumente dienen. Auf dieser Seite aufgerufen werden könnte `GetPDF.aspx`, und der Name der PDF-Datei über eine Querystring-Parameter übergeben wird. Die `GetPDF.aspx` Seite würde zunächst prüfen, ob der Benutzer verfügt über die Berechtigung zum Anzeigen der Datei, und wenn dies der Fall ist, verwenden die [ `Response.WriteFile(filePath)` ](https://msdn.microsoft.com/library/system.web.httpresponse.writefile.aspx) Methode, um den Inhalt der angeforderten PDF-Datei an den anfordernden Client zu senden. Dieses Verfahren funktioniert auch für IIS 7, wenn Sie nicht, aktivieren die integrierte Pipeline möchten.


## <a name="summary"></a>Zusammenfassung

Webanwendungen in einer produktiven Umgebung gehostet werden, mithilfe des Microsoft IIS Web-Serversoftware. In der Entwicklungsumgebung möglicherweise jedoch die Anwendung gehostet werden mithilfe von IIS oder ASP.NET Development Server. Im Idealfall sollte die gleiche Webserver-Software in beiden Umgebungen verwendet werden, da in der Mischung andere Software mit einer anderen Variablen hinzugefügt werden. Allerdings vereinfacht die Einfachheit der Verwendung von ASP.NET Development Server eine attraktive Möglichkeit in der Entwicklungsumgebung. Die gute Nachricht ist, dass nur einige grundlegende Unterschiede zwischen IIS und ASP.NET Development Server vorhanden sind, und wenn Sie diese Unterschiede kennen Sie Schritte können können Sie sicherstellen, dass die Anwendung funktioniert und genauso funktioniert wie unabhängig von der Umgebung.

Viel Spaß beim Programmieren!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Lernprogramm erläutert finden Sie in den folgenden Ressourcen:

- [Integration von ASP.NET in IIS 7.0](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis)
- [Alle Typen von Inhalten in IIS 7-Foren ASP.NET-Authentifizierung mit](https://blogs.iis.net/bills/archive/2007/05/19/using-asp-net-forms-authentication-with-all-types-of-content-with-iis7-video.aspx) (Video)
- [Webserver in Visual Web Developer](https://msdn.microsoft.com/library/58wxa9w5.aspx)

> [!div class="step-by-step"]
> [Zurück](common-configuration-differences-between-development-and-production-cs.md)
> [Weiter](deploying-a-database-cs.md)
