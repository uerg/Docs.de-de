---
uid: web-api/overview/testing-and-debugging/troubleshooting-http-405-errors-after-publishing-web-api-applications
title: Problembehandlung bei HTTP 405 Fehler nach der Veröffentlichung Web API 2 Anwendungen | Microsoft Docs
author: rmcmurray
description: In diesem Lernprogramm wird beschrieben, wie zur Problembehandlung nach dem Veröffentlichen einer Web-API-Anwendung auf einem Produktionsserver-Web HTTP 405 wird.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/01/2014
ms.topic: article
ms.assetid: 07ec7d37-023f-43ea-b471-60b08ce338f7
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/troubleshooting-http-405-errors-after-publishing-web-api-applications
msc.type: authoredcontent
ms.openlocfilehash: b87ae7420e1295030e90c30e97b1e331413ce263
ms.sourcegitcommit: f1436107b4c022b26f5235dddef103cec5aa6bff
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/15/2017
---
<a name="troubleshooting-http-405-errors-after-publishing-web-api-2-applications"></a>Problembehandlung bei HTTP 405 Fehler nach der Veröffentlichung Web-API 2-Anwendungen
====================
durch [Robert McMurray](https://github.com/rmcmurray)

> In diesem Lernprogramm wird beschrieben, wie zur Problembehandlung nach dem Veröffentlichen einer Web-API-Anwendung auf einem Produktionsserver-Web HTTP 405 wird.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>In diesem Lernprogramm verwendeten Versionen der Software
> 
> 
> - [Internetinformationsdienste (Internet Information Services, IIS)](https://www.iis.net/) (Version 7 oder höher)
> - [Web-API](../../index.md) (Version 1 oder 2)


Web-API-Anwendungen verwenden in der Regel mehrere allgemeine HTTP-Verben: GET, POST, PUT, DELETE, und manchmal GEPATCHT. Nichtsdestotrotz Entwickler in Situationen, in denen diese Verben werden durch ein anderes Modul von IIS auf den Produktionsserver, was zu einer Situation führt ein Web-API-Controller, der in Visual Studio oder auf einem Entwicklungsserver ordnungsgemäß funktioniert, in denen zurück implementiert, führen möglicherweise eine HTTP 405 Fehler, wenn es auf einem Produktionsserver bereitgestellt wird. Dieses Problem ist Glücklicherweise leicht gelöst, aber die Auflösung gewährleistet eine Erläuterung, warum das Problem auftritt.

## <a name="what-causes-http-405-errors"></a>Welche Ursachen HTTP 405 Fehler

Der erste Schritt zum Erlernen der HTTP-405-Fehler zu behandeln ist, zu verstehen, welche HTTP-Fehler 405 tatsächlich bedeutet. Dokumentieren Sie die primäre Steuerung für HTTP ist [RFC 2616](http://www.ietf.org/rfc/rfc2616.txt), definiert der Statuscode "HTTP 405" als ***Method Not Allowed***, und weitere beschreibt dieser Statuscode als eine Situation, in denen &quot;der Methode angegebene in die Anforderungszeile ist für die durch den Anforderungs-URI identifizierte Ressource nicht zulässig.&quot; Das HTTP-Verb ist also nicht für die bestimmte URL zulässig, die ein HTTP-Client angefordert wurde.

Eine kurze Überprüfung sind hier einige der am häufigsten verwendeten HTTP-Methoden gemäß Definition in RFC 2616, RFC 4918 und RFC 5789:

| HTTP-Methode | Beschreibung |
| --- | --- |
| **ERHALTEN** | Diese Methode wird verwendet, um Daten aus einem URI, und es wahrscheinlich die am häufigsten verwendeten HTTP-Methode abzurufen. |
| **HEAD** | Diese Methode ist ähnlich wie die GET-Methode, außer dass es nicht die Daten tatsächlich vom Anforderungs-URI Abrufen – einfach Ruft den HTTP-Status ab. |
| **BEREITSTELLEN** | Diese Methode wird in der Regel verwendet, um neue Daten an den URI senden; POST wird häufig verwendet, zum Übermitteln von Formulardaten. |
| **PUT** | Diese Methode wird normalerweise verwendet, um die Rohdaten an den URI senden; PUT wird häufig verwendet, um JSON oder XML-Daten für Web-API-Anwendungen senden. |
| **LÖSCHEN** | Diese Methode wird verwendet, um Daten aus einem URI zu entfernen. |
| **OPTIONEN** | Diese Methode wird normalerweise verwendet, um die Liste der HTTP-Methoden abzurufen, die für einen URI unterstützt werden. |
| **KOPIEREN VERSCHIEBEN** | Diese beiden Methoden werden mit WebDAV verwendet, und ihr Zweck ist selbsterklärend. |
| **MKCOL** | Diese Methode wird mit WebDAV verwendet, und es wird verwendet, um eine Sammlung (z. B. ein Verzeichnis) am angegebenen URI zu erstellen. |
| **PROPFIND PROPPATCH** | Diese beiden Methoden werden mit WebDAV verwendet, und sie werden zum Abfragen oder zum Festlegen von Eigenschaften für einen URI verwendet. |
| **SPERREN ENTSPERREN** | Diese beiden Methoden werden mit WebDAV verwendet, und sie werden verwendet, um die Ressource, die durch die Anforderungs-URI identifiziert wird, beim Erstellen von Sperren/Entsperren. |
| **PATCH** | Diese Methode wird verwendet, um eine vorhandene HTTP-Ressource zu ändern. |

Wenn eine der folgenden HTTP-Methoden für die Verwendung auf dem Server konfiguriert ist, antwortet der Server mit der HTTP-Status und andere Daten, die für die Anforderung geeignet ist. (Z. B. eine GET-Methode wird möglicherweise eine HTTP 200 ***OK*** Antwort und eine PUT-Methode wird möglicherweise eine HTTP 201 ***erstellt*** Antwort.)

Wenn die HTTP-Methode für die Verwendung auf dem Server nicht konfiguriert ist, der Server antwortet mit einer HTTP-501 ***nicht implementiert*** Fehler.

Jedoch, wenn eine HTTP-Methode für die Verwendung auf dem Server konfiguriert ist, aber es für einen angegebenen URI deaktiviert wurde, der Server antwortet mit einer HTTP-405 ***Method Not Allowed*** Fehler.

## <a name="example-http-405-error"></a>Beispiel für HTTP 405 Fehler

Das folgende Beispiel HTTP-Anforderung und Antwort veranschaulichen eine Situation, in dem ein HTTP-Client versucht, die in einer Web-API-Anwendung auf einem Webserver PUT-Wert, und der Server gibt einen HTTP-Fehler angibt, dass die PUT-Methode nicht zulässig:


HTTP-Anforderung:


[!code-console[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample1.cmd)]


HTTP-Antwort:


[!code-console[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample2.cmd)]


In diesem Beispiel wird der HTTP-Client eine gültige JSON-Anforderung an die URL für eine Web-API-Anwendung auf einem Webserver gesendet, aber der Server gab eine HTTP-405-Fehlermeldung gibt an, dass die PUT-Methode für die URL nicht zugelassen wurde. Im Gegensatz dazu, wenn im Anforderungs-URI nicht mit eine Route für die Web-API-Anwendung übereinstimmt, der Server würden zurück HTTP 404 ***Nichtgefunden*** Fehler.

## <a name="resolving-http-405-errors"></a>Beheben von HTTP 405 Fehler

Es gibt verschiedene Gründe, warum ein bestimmtes HTTP-Verb möglicherweise nicht zulässig, aber es ist ein wichtiges Szenario, der die führenden Ursache dieses Fehlers in IIS ist: mehrere Handler für die gleiche Verb/Methode definiert sind, und ist einer der Handler den erwarteten Handler aus blockieren Verarbeiten der Anforderung. Durch Erklärung verarbeitet IIS Handler aus der ersten, letzten basierend auf der Reihenfolge Handlereinträge in den Dateien "applicationHost.config" und "Web.config", in dem die erste übereinstimmende Kombination aus Pfad, Verb, Ressourcen usw. soll, zur Verarbeitung der Anforderung verwendet werden.

Im folgende Beispiel ist ein Auszug aus einer Datei "applicationHost.config" für einen IIS-Server, der HTTP-Fehler 405 zurückgeben wurde, bei Verwendung der PUT-Methode zum Senden von Daten an eine Web-API-Anwendung. In diesem Auszug mehrere HTTP-Handler definiert sind, und jeder Ereignishandler besitzt einen anderen Satz von HTTP-Methoden, die für die er konfiguriert wird – der letzte Eintrag in der Liste ist der statische Inhaltshandler, der Standard-Handler ist, der verwendet wird, nachdem die anderen Handler einer Chanc generiert hat e, um die Anforderung zu überprüfen:

[!code-xml[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample3.xml)]

Im Beispiel oben werden die WebDAV-Handler und den erweiterungslose URL-Handler für ASP.NET (die für die Web-API verwendet wird) eindeutig für separate Listen mit HTTP-Methoden definiert. Beachten Sie, dass für alle HTTP-Methoden, die ISAPI DLL-Handler konfiguriert ist, obwohl diese Konfiguration nicht unbedingt einen Fehler verursachen. Jedoch wie Einstellungen für diese Anforderung angesehen, bei der Problembehandlung von HTTP 405-Fehler.

Im obigen Beispiel war der ISAPI-DLL-Handler nicht das Problem. in der Tat das Problem nicht in der Datei "applicationHost.config" für den IIS-Server definiert wurde: das Problem verursacht wurde, indem Sie einen Eintrag, der in der Datei "Web.config" vorgenommen wurde, wenn die Web-API-Anwendung in Visual Studio erstellt wurde. Der folgende Auszug aus der Anwendungsdatei "Web.config" zeigt den Speicherort des Problems:

[!code-xml[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample4.xml)]

In diesem Auszug wird erweiterungslose-URL-Handler für ASP.NET neu definiert, um zusätzliche HTTP-Methoden enthalten, die mit der Web-API-Anwendung verwendet werden soll. Da eine ähnliche Reihe von HTTP-Methoden für die WebDAV-Ereignishandler definiert ist, tritt jedoch ein Konflikt auf. In diesem speziellen Fall wird der WebDAV-Handler definiert und von IIS geladen wird, obwohl WebDAV für die Website deaktiviert ist, die die Web-API-Anwendung enthält. Während der Verarbeitung einer HTTP PUT-Anforderung ruft IIS das WebDAV-Modul, da es das Verb PUT definiert wurde. Wenn das WebDAV-Modul aufgerufen wird, überprüft die Konfiguration und erkennt, dass er deaktiviert ist, damit sie eine HTTP-405 zurückgeben, wird ***Method Not Allowed*** Fehler für jede Anforderung, die eine WebDAV-Anforderung ähnelt. Um dieses Problem zu beheben, sollten Sie WebDAV aus der Liste der HTTP-Module für die Website entfernen, in dem Ihre Anwendung Web-API definiert ist. Im folgende Beispiel wird die Vorgehensweise, aussehen kann:

[!code-xml[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample5.xml)]

Dieses Szenario wird häufig gefunden, nachdem eine Anwendung aus einer Entwicklungsumgebung in einer produktionsumgebung veröffentlicht wird, und dies geschieht, weil die Liste der Handler-Module Ihrer Entwicklungs- und produktionsumgebungen Umgebungen unterschiedlich ist. Wenn Sie Visual Studio 2012 oder 2013 verwenden Sie eine Web-API-Anwendung entwickeln, ist IIS Express 8 z. B. der Standardwebserver für Testzwecke. Diese Entwicklungswebserver ist eine nach unten skalierte Version der vollständige IIS-Funktionen, die in einer Server-Produkts ausgeliefert wird und dieser Entwicklungswebserver enthält einige Änderungen, die für Entwicklungsszenarien hinzugefügt wurden. Beispielsweise ist das WebDAV-Modul häufig installiert auf einem Produktions-Web-Server, der die vollständige Version von IIS ausgeführt wird, obwohl er tatsächlich möglicherweise nicht. Die Entwicklungsversion von IIS (IIS Express), wird das WebDAV-Modul installiert, aber die Einträge für das WebDAV-Modul absichtlich auskommentiert werden, damit das WebDAV-Modul nie unter IIS Express geladen wird, es sei denn, Sie insbesondere die IIS Express-Konfiguration ändern. Einstellungen, die WebDAV-Funktionen der IIS Express-Installation hinzufügen. Daher möglicherweise Ihre Webanwendung ordnungsgemäß auf dem Entwicklungscomputer ausgeführt, jedoch können Sie HTTP 405 Fehler auftreten, wenn Sie Ihre Anwendung Web-API auf Ihren Webserver für die Produktion veröffentlichen.

## <a name="summary"></a>Zusammenfassung

HTTP 405 Fehler sind darauf zurückzuführen, dass eine HTTP-Methode nicht von einem Webserver für einer angeforderten URL zugelassen wird. Diese Bedingung wird häufig angezeigt, wenn ein bestimmter Handler für ein bestimmtes Verb definiert wurde, und dieser Handler wird den Handler, den Sie erwarten, beim Verarbeiten der Anforderung dass überschreiben.

Wenn Sie eine Situation auftreten, in dem Sie eine HTTP 501, d. die spezifische Funktionalität nicht auf dem Server implementiert wurde Fehlermeldung h., häufig bedeutet, dass es ist kein Handler definiert, die in den IIS-Einstellungen die der HTTP-Anforderung entspricht dem Wahrscheinlich gibt an, dass etwas nicht ordnungsgemäß auf Ihrem System installiert wurde oder etwas die IIS-Einstellungen geändert werden wurde, damit es sind keine Handler dieser Unterstützung von bestimmten HTTP-Methode definiert. Um dieses Problem zu beheben, müssten Sie jede Anwendung zu installieren, die versucht, eine HTTP-Methode verwenden, für die er keine entsprechende Modul oder Handler Definitionen enthält.
