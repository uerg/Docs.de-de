---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/processing-unhandled-exceptions-vb
title: Verarbeitung von nicht behandelten Ausnahmen (VB) | Microsoft Docs
author: rick-anderson
description: "Tritt ein Laufzeitfehler auf eine Webanwendung in der Produktion ist es wichtig, einen Entwickler benachrichtigt, damit den Fehler protokolliert werden, damit er an einen la diagnostiziert werden möglicherweise..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: 051296f0-9519-4e78-835c-d868da13b0a0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/processing-unhandled-exceptions-vb
msc.type: authoredcontent
ms.openlocfilehash: f2c7b1324e75584a80530620eea94d4ecd7a7044
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="processing-unhandled-exceptions-vb"></a>Verarbeitung von nicht behandelten Ausnahmen (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Herunterladen von Code](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_12_VB.zip) oder [PDF herunterladen](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial12_ErrorHandling_vb.pdf)

> Tritt ein Laufzeitfehler auf eine Webanwendung in der Produktion ist es wichtig, einen Entwickler benachrichtigt, damit den Fehler protokolliert werden, damit er zu einem späteren Zeitpunkt zeitlich diagnostiziert werden kann. Dieses Lernprogramm bietet einen Überblick darüber, wie ASP.NET Laufzeitfehler verarbeitet und untersucht eine Möglichkeit, benutzerdefinierten Code, wenn eine nicht behandelte Ausnahme Blasen bis zu die ASP.NET-Laufzeit ausgeführt haben.


## <a name="introduction"></a>Einführung

Tritt eine nicht behandelte Ausnahme in einer ASP.NET-Anwendung, übergeben Sie bis zu die ASP.NET-Laufzeit, wodurch ausgelöst wird die `Error` Ereignis und die entsprechenden Fehlerseite angezeigt. Es gibt drei verschiedene Typen von Fehlerseiten: die Common Language Runtime Fehler gelben Bildschirm des Tod (YSOD); die Ausnahmedetails YSOD; und benutzerdefinierte Fehlerseiten. In der [vorherigen Lernprogramm](displaying-a-custom-error-page-vb.md) wir konfiguriert, dass die Anwendung eine benutzerdefinierte Fehlerseite für Remotebenutzer und die Ausnahme Details YSOD für Besucher lokal verwenden.

Mithilfe einer Human benutzerfreundliche benutzerdefinierte Fehlerseite, die das Aussehen und Verhalten des Standorts entspricht wird auf den Standardwert Runtime Fehler YSOD bevorzugt, aber nur ein Teil einer umfassenden Lösung für die Fehlerbehandlung wird eine benutzerdefinierte Fehlerseite anzeigen. Tritt ein Fehler in einer Anwendung in der Produktion, ist es wichtig, dass die Entwickler des Fehlers benachrichtigt werden, damit sie die Ursache der Ausnahme entdecken können und die Adresse. Darüber hinaus sollte Details der Fehler protokolliert werden, damit der Fehler überprüft und zu einem späteren Zeitpunkt zeitlich diagnostiziert werden kann.

Dieses Lernprogramm zeigt die Details einer nicht behandelten Ausnahme Zugriff auf, damit sie protokolliert werden können und ein Entwickler benachrichtigt. Die zwei Lernprogramme, die nach dieser untersuchen Fehler protokollierungsbibliotheken, die nach einem gewissen Grad Konfiguration automatisch benachrichtigen Entwickler von Laufzeitfehler und ihre Details zu melden.

> [!NOTE]
> Die Informationen in diesem Lernprogramm untersucht ist besonders hilfreich, wenn nicht behandelte Ausnahmen in eine eindeutige oder angepasste Weise verarbeitet werden muss. In Fällen, in denen nur müssen Sie die Ausnahme protokollieren und einen Entwickler zu benachrichtigen, ist die Verwendung von einer Fehler Protokollierung der Bibliothek die beste Wahl. Die nächsten beiden Lernprogramme enthalten eine Übersicht über zwei solcher Bibliotheken.


## <a name="executing-code-when-theerrorevent-is-raised"></a>Ausführen von Code, wenn die`Error`Ereignis wird ausgelöst.

Ereignisse stellen ein Objekt einen Mechanismus für die signalisieren, dass etwas Interessantes aufgetreten ist und ein weiteres Objekt für die Ausführung von Code in der Antwort bereit. Als ASP.NET-Entwickler sind Sie daran gewöhnt, dem Aspekt im Hinblick auf Ereignisse. Wenn Sie Code ausführen, wenn der Besucher einer bestimmten klickt möchten, erstellen Sie einen Ereignishandler für der Schaltfläche `Click` Ereignis, und fügen Sie Ihrem Code vorhanden. Davon ausgehend, dass die ASP.NET-Laufzeit löst die [ `Error` Ereignis](https://msdn.microsoft.com/library/system.web.httpapplication.error.aspx) immer eine nicht behandelte Ausnahme auftritt, daraus folgt, dass der Code für die Protokollierung der Fehler-Details in einem Ereignishandler gehen würde. Aber wie erstellen Sie einen Ereignishandler für das `Error` Ereignis?

Die `Error` Ereignis ist eines der vielen Ereignisse in der [ `HttpApplication` Klasse](https://msdn.microsoft.com/library/system.web.httpapplication.aspx) , die in bestimmten Phasen in der HTTP-Pipeline während der Lebensdauer einer Anforderung ausgelöst werden. Z. B. die `HttpApplication` Klasse [ `BeginRequest` Ereignis](https://msdn.microsoft.com/library/system.web.httpapplication.beginrequest.aspx) wird zu Beginn jeder Anforderung ausgelöst, dessen [ `AuthenticateRequest` Ereignis](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx) wird ausgelöst, wenn ein Sicherheitsmodul des anforderers ermittelt wurde. Diese `HttpApplication` Ereignisse der Entwickler der Seite bieten eine Möglichkeit, benutzerdefinierten Logik zu verschiedenen Zeitpunkten während der Lebensdauer einer Anforderung ausführen.

Ereignishandler für das `HttpApplication` Ereignisse platziert werden können, in eine spezielle Datei namens `Global.asax`. Um diese Datei in Ihre Website zu erstellen, fügen Sie ein neues Element in das Stammverzeichnis der Website mithilfe der Globale Anwendungsklasse-Vorlage mit dem Namen `Global.asax`.

[![](processing-unhandled-exceptions-vb/_static/image2.png)](processing-unhandled-exceptions-vb/_static/image1.png)

**Abbildung 1**: Hinzufügen `Global.asax` für Ihre Webanwendung  
 ([Klicken Sie hier, um das Bild in voller Größe angezeigt](processing-unhandled-exceptions-vb/_static/image3.png))

Die Struktur und Inhalt der `Global.asax` von Visual Studio erstellte Datei unterscheiden sich geringfügig abhängig, ob Sie eine Web Application Project (WAP) oder die Website-Projekt (WSP) verwenden. Mit einem WAP der `Global.asax` wird als zwei separate Dateien - implementiert `Global.asax` und `Global.asax.vb`. Die `Global.asax` -Datei enthält keine aber ein `@Application` -Direktive, verweist der `.vb` Datei; das Ereignis Handler von Interesse, in definiert sind der `Global.asax.vb` Datei. Für WSPs, nur eine einzige Datei erstellt wird, `Global.asax`, und die Ereignishandler werden definiert, einem `<script runat="server">` Block.

Die `Global.asax` in einem drahtlosen Zugriffspunkt globale anwendungsklassenvorlage Visual Studio erstellte Datei beinhaltet Ereignishandler, die mit dem Namen `Application_BeginRequest`, `Application_AuthenticateRequest`, und `Application_Error`, wobei es sich um Ereignishandler für die `HttpApplication` Ereignisse `BeginRequest`, `AuthenticateRequest`, und `Error`zugeordnet. Es gibt auch Ereignishandler mit dem Namen `Application_Start`, `Session_Start`, `Application_End`, und `Session_End`, welche Ereignishandler, die ausgelöst werden, wenn die Webanwendung startet, wenn eine neue Sitzung gestartet wird, wird am Ende die Anwendung sind, und wenn eine Sitzung beendet wird, bzw. Die `Global.asax` Datei in eine WSP von Visual Studio erstellt wurden, enthält nur die `Application_Error`, `Application_Start`, `Session_Start`, `Application_End`, und `Session_End` -Ereignishandler.

> [!NOTE]
> Wenn Sie die ASP.NET-Anwendung bereitstellen, müssen Sie kopieren die `Global.asax` Datei in der produktionsumgebung. Die `Global.asax.vb` -Datei, die in der WAP erstellt wird, muss nicht in die Produktion kopiert werden, da dieser Code in der Assembly des Projekts kompiliert wird.


Die Ereignishandler, die durch Visual Studio Globale Anwendungsklasse Vorlage erstellt wurden, sind nicht vollständig. Sie können einen Ereignishandler hinzufügen, für eine beliebige `HttpApplication` Ereignis durch Benennen des ereignishandlers `Application_EventName`. Beispielsweise können Sie den folgenden Code hinzu Hinzufügen der `Global.asax` Datei so erstellen einen Ereignishandler für das [ `AuthorizeRequest` Ereignis](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx):

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample1.vb)]

Entsprechend können Sie alle Ereignishandler, die durch die Globale Anwendungsklasse-Vorlage erstellt entfernen, die nicht benötigt werden. Für dieses Lernprogramm benötigen wir nur einen Ereignishandler für das `Error` Ereignis; gerne So entfernen Sie die anderen Ereignishandler aus der `Global.asax` Datei.

> [!NOTE]
> *HTTP-Module* bieten eine weitere Möglichkeit zum Definieren von Ereignishandlern für `HttpApplication` Ereignisse. HTTP-Module werden in einer separaten Klassenbibliothek erstellt, als eine Klassendatei, die direkt in das Webanwendungsprojekt platziert oder ausgelagert werden kann. Da sie in einer Klassenbibliothek ausgelagert werden können, bieten HTTP-Module flexibler und wiederverwendbare Modell zum Erstellen `HttpApplication` -Ereignishandler. Während der `Global.asax` Datei bezieht sich auf die Webanwendung, in dem er sich befindet, können HTTP-Module in Assemblys, die an diesem Punkt vom HTTP-Modul zu einer Website hinzufügen so einfach ist wie das Löschen der Assemblys in kompiliert werden die `Bin` Ordner und die Registrierung der Modul `Web.config`. Dieses Lernprogramms sieht nicht zur Erstellung und Verwendung von HTTP-Module, aber die zwei fehlerprotokollierung-Bibliotheken, die in den folgenden zwei Lernprogrammen verwendet werden wie HTTP-Modul implementiert. Weitere Hintergrundinformationen zu den Vorteilen der HTTP-Modulen finden Sie unter [mithilfe von HTTP-Module und -Handler austauschbare ASP.NET Komponenten erstellen](https://msdn.microsoft.com/library/aa479332.aspx).


## <a name="retrieving-information-about-the-unhandled-exception"></a>Abrufen von Informationen über die nicht behandelte Ausnahme

An diesem Punkt haben wir eine Datei "Global.asax" mit einer `Application_Error` -Ereignishandler. Bei der Ausführung dieser Ereignishandler müssen wir benachrichtigen ein Entwickler des Fehlers, und melden Sie sich die Details. Für diese Aufgaben müssen Sie zuerst die Details der Ausnahme zu bestimmen, die ausgelöst wurde. Verwenden Sie des Serverobjekts [ `GetLastError` Methode](https://msdn.microsoft.com/library/system.web.httpserverutility.getlasterror.aspx) beim Abrufen der Details der nicht behandelten Ausnahme, die aufgrund der `Error` Ereignis ausgelöst.

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample2.vb)]

Die `GetLastError` Methode gibt ein Objekt vom Typ `Exception`, dies ist der Basistyp für alle Ausnahmen in .NET Framework. Der obige Code bin ich jedoch das Ausnahmeobjekt zurückgegebenes Umwandlung `GetLastError` in ein `HttpException` Objekt. Wenn die `Error` Ereignis wird ausgelöst wird, da bei der Verarbeitung einer Ressource ASP.NET eine Ausnahme ausgelöst wurde, und klicken Sie dann die ausgelöste Ausnahme umschlossen ist ein `HttpException`. Um die eigentliche Ausnahme abzurufen, die die Verwendung der Fehler Ereignis unmittelbar vorausging der `InnerException` Eigenschaft. Wenn die `Error` Ereignis ausgelöst wurde aufgrund einer Ausnahme HTTP-basierte z. B. eine Anforderung für eine nicht existierende-Seite, ein `HttpException` ausgelöst wird, aber sie verfügt nicht über eine interne Ausnahme.

Der folgende code verwendet die `GetLastErrormessage` zum Abrufen von Informationen über die Ausnahme, die ausgelöst der `Error` Ereignis, das Speichern von der `HttpException` in einer Variablen namens `lastErrorWrapper`. Es speichert der Typ, die Meldung und die stapelüberwachung der ursprünglichen Ausnahme in drei Zeichenfolgenvariablen, die überprüft wird, wenn die `lastErrorWrapper` die eigentliche Ausnahme, die ausgelöst wird die `Error` Ereignis (im Fall von HTTP-basierte Ausnahmen) oder wenn sie lediglich eine Wrapper für eine Ausnahme, die beim Verarbeiten der Anforderung ausgelöst wurde.

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample3.vb)]

An diesem Punkt haben Sie die Informationen, dass Sie Code schreiben, die die Details der Ausnahme in einer Datenbanktabelle protokolliert werden müssen. Sie können eine Datenbanktabelle mit Spalten aller relevanten - Typ, der Nachricht, die stapelüberwachung und usw. - sowie andere nützliche Informationen, z. B. die URL der angeforderten Seite und den Namen des aktuell angemeldeten Benutzers die Fehlerdetails erstellen. In der `Application_Error` Ereignishandler anschließend eine Verbindung mit der Datenbank herstellen und einen Datensatz in die Tabelle eingefügt. Ebenso können Sie Code, damit ein Entwickler des Fehlers per E-mail benachrichtigt werden, hinzufügen.

Der Fehler protokollierungsbibliotheken, die in den nächsten beiden Lernprogramme untersucht bieten solche Funktionen ausgegeben, daher keine Notwendigkeit besteht, diese Fehler protokollieren und die Benachrichtigung selbst erstellen. Allerdings an, die veranschaulichen die `Error` Ereignis ausgelöst wird und dass die `Application_Error` -Ereignishandler kann verwendet werden, melden Sie sich Details zum Systemfehler und einen Entwickler zu benachrichtigen, fügen Sie Code, der einen Entwickler benachrichtigt, wenn ein Fehler auftritt.

## <a name="notifying-a-developer-when-an-unhandled-exception-occurs"></a>Einen Entwickler benachrichtigen, wenn eine nicht behandelte Ausnahme auftritt.

Tritt eine nicht behandelte Ausnahme in der produktionsumgebung ist es wichtig, um das Entwicklungsteam informieren und sie können den Fehler zu bewerten und bestimmen, welche Aktionen ausgeführt werden müssen. Beispielsweise ist ein Fehler in Verbindung mit der Datenbank aufweist, müssen Sie nach double überprüfen Sie Ihre Verbindungszeichenfolge und, öffnen Sie ein Support-Ticket ggf. mit Ihrem Webhosting. Wenn die Ausnahme aufgrund einer Programmierfehler aufgetreten ist, kein zusätzlicher Code oder Validierungslogik müssen möglicherweise hinzugefügt werden, um solche Fehler in Zukunft zu vermeiden.

Die .NET Framework-Klassen der [ `System.Net.Mail` Namespace](https://msdn.microsoft.com/library/system.net.mail.aspx) können sie einfach eine e-Mail zu senden. Die [ `MailMessage` Klasse](https://msdn.microsoft.com/library/system.net.mail.mailmessage.aspx) eine e-Mail dar und verfügt über Eigenschaften, z. B. `To`, `From`, `Subject`, `Body`, und `Attachments`. Die `SmtpClass` dient zum Senden einer `MailMessage` -Objekt mit einem angegebenen SMTP-Server, die SMTP-servereinstellungen können programmgesteuert oder deklarativ in angegeben werden die [ `<system.net>` Element](https://msdn.microsoft.com/library/6484zdc1.aspx) in der `Web.config file`. Weitere Informationen zum Senden von e-Mails Nachrichten in einer ASP.NET-Anwendung sehen Sie sich meine Artikel [Senden von E-Mails in ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx), und die [System.Net.Mail FAQ](http://systemnetmail.com/).

> [!NOTE]
> Die `<system.net>` Element enthält die SMTP-servereinstellungen verwendet, durch die `SmtpClient` Klasse, wenn eine E-mail zu senden. Ihrem Webhosting wahrscheinlich verfügt über einen SMTP-Server, den Sie zum Senden von E-mail aus Ihrer Anwendung verwenden können. Wenden Sie sich an Ihre Webhost-Unterstützung im Abschnitt Informationen auf den SMTP-Server-Einstellungen, die Sie in der Webanwendung verwenden soll.


Fügen Sie folgenden Code, der `Application_Error` -Ereignishandler einem Entwickler eine E-mail gesendet, wenn ein Fehler auftritt:

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample4.vb)]

Während der obige Code ziemlich umfassend ist, erstellt es der Großteil den HTML-Code, der angezeigt wird in dem Entwickler gesendeten E-mail. Der Code durch Verweisen auf startet die `HttpException` zurückgegebenes der `GetLastError` Methode (`lastErrorWrapper`). Die eigentliche Ausnahme, die von der Anforderung ausgelöst wurde, wird über abgerufen `lastErrorWrapper.InnerException` und der Variablen zugewiesen `lastError`. Der Typ, die Nachricht und den Stack Ablaufverfolgungsinformationen entnommen `lastError` und in drei Zeichenfolgenvariablen gespeichert.

Als Nächstes wird ein `MailMessage` Objekt mit dem Namen `mm` wird erstellt. E-Mail-Nachrichtentext wird HTML-formatierte und zeigt die URL der angeforderten Seite, den Namen der derzeit angemeldete Benutzer und Informationen zur Ausnahme (Typ, Nachrichten und stapelüberwachung). Eine der Coolbar Aufgaben zu den `HttpException` Klasse ist, dass Sie den HTML-Code verwendet, um die Ausnahme Details gelben Bildschirm des Tod (YSOD) zu erstellen, indem generieren können die [GetHtmlErrorMessage-Methode](https://msdn.microsoft.com/library/system.web.httpexception.gethtmlerrormessage.aspx). Diese Methode wird hier verwendet, die Ausnahme Details YSOD Markup abrufen und an die e-Mail als Anlage hinzuzufügen. Vorsicht: Wenn die Ausnahme, die ausgelöst der `Error` Ereignis ist ein HTTP-basierte Ausnahmefehler (z. B. eine Anforderung für eine nicht existierende-Seite) und dann die `GetHtmlErrorMessage` Methode zurück `null`.

Der letzte Schritt ist zum Senden der `MailMessage`. Dies erfolgt durch Erstellen eines neuen `SmtpClient` -Methode und der Aufruf der `Send` Methode.

> [!NOTE]
> Bevor Sie diesen Code in der Webanwendung verwenden soll so ändern Sie die Werte in der `ToAddress` und `FromAddress` Konstanten aus support@example.com an beliebige e-Mail-Adresse der Fehler Benachrichtigung wird per E-mail gesendet werden soll, um und stammen aus. Sie müssen auch an SMTP-servereinstellungen in der `<system.net>` im Abschnitt `Web.config`. Wenden Sie sich an Ihre Webhostinganbieter aus, um zu bestimmen, die SMTP-servereinstellungen zu verwenden.


Mit diesem Code werden jedes Mal, wenn ein Fehler vorliegt wird der Entwickler eine e-Mail-Nachricht gesendet, die den Fehler zusammengefasst und enthält die YSOD. In dem vorherigen Lernprogramm veranschaulicht wir einen Laufzeitfehler durch Genre.aspx besuchen, und übergeben ein ungültiges `ID` Wert über die Abfragezeichenfolge, z. B. `Genre.aspx?ID=foo`. Besuchen die Seite mit den `Global.asax` Datei direkt erzeugt die gleiche benutzererfahrung, wie im vorherigen Lernprogramm - in der Entwicklungsumgebung auch Ausnahme Details gelben Bildschirm des Tod, weiterhin müssen während in der produktionsumgebung Sie müssen finden Sie die benutzerdefinierte Fehlerseite aus. Zusätzlich zu diesen bereits bestehendem Verhalten wird der Entwickler eine E-mail gesendet.

**Abbildung 2** zeigt die e-Mail-Nachricht empfangen wird, wenn das Unternehmen besuchende `Genre.aspx?ID=foo`. E-Mail-Nachrichtentext enthält eine Zusammenfassung der Ausnahmeinformationen während der `YSOD.htm` Anlage zeigt den Inhalt, der in der Ausnahme Details YSOD angezeigt wird (finden Sie unter **Abbildung 3**).

[![](processing-unhandled-exceptions-vb/_static/image5.png)](processing-unhandled-exceptions-vb/_static/image4.png)

**Abbildung 2**: der Entwickler wird eine e-Mail-Benachrichtigung gesendet, wenn eine nicht behandelte Ausnahme aufgetreten ist  
 ([Klicken Sie hier, um das Bild in voller Größe angezeigt](processing-unhandled-exceptions-vb/_static/image6.png))

[![](processing-unhandled-exceptions-vb/_static/image8.png)](processing-unhandled-exceptions-vb/_static/image7.png)

**Abbildung 3**: die e-Mail-Benachrichtigung enthält die Details der Ausnahme YSOD als Anlage  
 ([Klicken Sie hier, um das Bild in voller Größe angezeigt](processing-unhandled-exceptions-vb/_static/image9.png))

## <a name="what-about-using-the-custom-error-page"></a>Was können Sie verwenden die benutzerdefinierte Fehlerseite?

Dieses Lernprogramm wurde gezeigt, wie `Global.asax` und `Application_Error` Ereignishandler zum Ausführen von Code, wenn eine nicht behandelte Ausnahme auftritt. Insbesondere können wurde dieser Ereignishandler verwendet, um ein Entwickler einen Fehler zu informieren; Wir waren, um auch melden Sie sich die Fehlerdetails in einer Datenbank zu erweitern. Das Vorhandensein der `Application_Error` Ereignishandler wirkt sich nicht auf die Erfahrung des Endbenutzers. Sie sehen immer noch die konfigurierten Fehlerseite, es sich um den Fehler Details YSOD, YSOD der Common Language Runtime-Fehler oder die benutzerdefinierte Fehlerseite sein.

Es ist natürlich weiß, ob die `Global.asax` Datei und `Application_Error` Ereignis ist erforderlich, wenn Sie eine benutzerdefinierte Fehlerseite verwenden. Wenn ein Fehler auftritt, wird die benutzerdefinierte Fehlerseite für den Benutzer angezeigt also Warum kann nicht wir platzieren Sie den Code vom Entwickler zu benachrichtigen und protokollieren die Fehlerdetails in die CodeBehind-Klasse, der die benutzerdefinierte Fehlerseite? Während Sie die benutzerdefinierte Fehlerseite Code-Behind-Klasse sicherlich Code hinzufügen können Sie keinen Zugriff auf die Details der Ausnahme, die ausgelöst der `Error` Ereignis aus, wenn das Verfahren verwenden wir im vorherigen Lernprogramm untersucht. Aufrufen der `GetLastError` über die benutzerdefinierte Fehlerseite Methodenrückgabe `Nothing`.

Der Grund für dieses Verhalten ist, da die benutzerdefinierte Fehlerseite über eine Umleitung erreicht ist. Löst aus, wenn eine nicht behandelte Ausnahme der ASP.NET-Laufzeit die Engine ASP.NET erreicht ihre `Error` Ereignis (das ausgeführt wird die `Application_Error` Ereignishandler) und dann *leitet* der Benutzer die benutzerdefinierte Fehlerseite durch Ausgeben einer `Response.Redirect(customErrorPageUrl)`. Die `Response.Redirect` Methode sendet eine Antwort an den Client mit einem HTTP 302-Statuscode angewiesen wird, den Browser, um eine neue URL, d. h. die benutzerdefinierte Fehlerseite anfordern. Der Browser fordert dann automatisch diese neue Seite. Sie können feststellen, dass die benutzerdefinierte Fehlerseite separat angefordert wurde, auf der Seite, in dem der Fehler aufgetreten, da sich der Adressleiste des Browsers auf der Seite benutzerdefinierte Fehler-URL ändert (siehe **Abbildung 4**).

[![](processing-unhandled-exceptions-vb/_static/image11.png)](processing-unhandled-exceptions-vb/_static/image10.png)

**Abbildung 4**: Wenn ein Fehler auftritt, der Browser ruft umgeleitet, um die URL der benutzerdefinierten Fehler angezeigt  
 ([Klicken Sie hier, um das Bild in voller Größe angezeigt](processing-unhandled-exceptions-vb/_static/image12.png))

Im Endeffekt ist, antwortet der Server mit HTTP 302-Umleitung endet mit die Anforderung, in die nicht behandelte Ausnahme aufgetreten ist. Die nachfolgende Anforderung an die benutzerdefinierte Fehlerseite ist eine völlig neue Anforderung. zu diesem Zeitpunkt ASP.NET Datenbankmodul hat die Fehlerinformationen verworfen und darüber hinaus hat keine Möglichkeit, die neue Anforderung für die benutzerdefinierte Fehlerseite die nicht behandelte Ausnahme in der vorherigen Anforderung zugeordnet werden soll. Deswegen `GetLastError` gibt `null` beim Aufrufen durch die benutzerdefinierte Fehlerseite.

Allerdings ist es möglich, dass die benutzerdefinierte Fehlerseite ausgeführt, während der gleichen Anforderung, die den Fehler verursacht hat. Die [ `Server.Transfer(url)` ](https://msdn.microsoft.com/library/system.web.httpserverutility.transfer.aspx) Methode übergibt die Ausführung an der angegebenen URL und innerhalb der gleichen Anforderung verarbeitet. Konnte, verschieben Sie den Code in der `Application_Error` -Ereignishandler, um die benutzerdefinierte Fehlerseite CodeBehind-Klasse, und Ersetzen Sie dabei in `Global.asax` durch den folgenden Code:

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample5.vb)]

Wenn nun eine nicht behandelte Ausnahme auftritt der `Application_Error` Ereignishandler überträgt die Steuerung an die entsprechende benutzerdefinierte Fehlerseite basierend auf den HTTP-Statuscode. Da die Steuerung wurde, hat die benutzerdefinierte Fehlerseite Zugriff auf die Informationen nicht behandelte Ausnahme über `Server.GetLastError` können Entwickler den Fehler benachrichtigen und melden Sie sich die Details. Die `Server.Transfer` Aufruf beendet das Modul für ASP.NET aus Weiterleiten des Benutzers an die benutzerdefinierte Fehlerseite. Stattdessen wird die benutzerdefinierte Fehlerseite Inhalt als Antwort auf der Seite "zurückgegeben, die den Fehler generiert hat.

## <a name="summary"></a>Zusammenfassung

Tritt eine nicht behandelte Ausnahme in eine ASP.NET-Webanwendung löst die ASP.NET-Laufzeit die `Error` Ereignis und die konfigurierten Fehlerseite angezeigt. Es kann den Entwickler den Fehler benachrichtigen. Melden Sie sich die Details, oder auf andere Weise verarbeitet werden, indem Sie einen Ereignishandler für das Error-Ereignis erstellen. Es gibt zwei Möglichkeiten, erstellen Sie einen Ereignishandler für `HttpApplication` Ereignisse wie `Error`: in der `Global.asax` Datei oder aus einem HTTP-Modul. Dieses Lernprogramm wurde gezeigt, wie zum Erstellen einer `Error` -Ereignishandler in der `Global.asax` -Datei, die Entwicklern einen Fehler mithilfe einer e-Mail-Nachricht benachrichtigt.

Erstellen einer `Error` -Ereignishandler ist nützlich, wenn nicht behandelte Ausnahmen in eine eindeutige oder angepasste Weise verarbeitet werden muss. Allerdings Erstellung Ihres eigenen `Error` -Ereignishandler protokolliert die Ausnahme oder einen Entwickler zu benachrichtigen ist nicht die effizienteste Nutzung Ihrer Zeit in Anspruch, da es bereits vorhanden sind kostenlos und einfach zu verwendende Fehler protokollierungsbibliotheken, die von Setup in wenigen Minuten werden können. Die nächsten beiden Lernprogramme untersuchen zwei diese Bibliotheken.

Viel Spaß beim Programmieren!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Lernprogramm erläutert finden Sie in den folgenden Ressourcen:

- [ASP.NET HTTP-Module und HTTP-Handler (Übersicht)](https://support.microsoft.com/kb/307985)
- [Ordnungsgemäß reagieren auf nicht behandelte Ausnahmen - Verarbeitung von nicht behandelten Ausnahmen](http://aspnet.4guysfromrolla.com/articles/091306-1.aspx)
- [`HttpApplication`Klasse und das Anwendungsobjekt ASP.NET](http://www.eggheadcafe.com/articles/20030211.asp)
- [HTTP-Handler und HTTP-Module in ASP.NET](http://www.15seconds.com/Issue/020417.htm)
- [Senden von E-Mail in ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)
- [Grundlegendes zu den `Global.asax` Datei](http://aspalliance.com/1114_Understanding_the_Globalasax_file.all)
- [Mithilfe von HTTP-Module und Handler austauschbare ASP.NET Komponenten erstellen](https://msdn.microsoft.com/library/aa479332.aspx)
- [Arbeiten mit ASP.NET `Global.asax` Datei](http://articles.techrepublic.com.com/5100-10878_11-5771721.html)
- [Arbeiten mit `HttpApplication` Instanzen](https://msdn.microsoft.com/library/a0xez8f2.aspx)

>[!div class="step-by-step"]
[Zurück](displaying-a-custom-error-page-vb.md)
[Weiter](logging-error-details-with-asp-net-health-monitoring-vb.md)
