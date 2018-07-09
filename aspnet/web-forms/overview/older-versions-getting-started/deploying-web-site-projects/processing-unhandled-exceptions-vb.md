---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/processing-unhandled-exceptions-vb
title: Verarbeiten von Ausnahmefehlern (VB) | Microsoft-Dokumentation
author: rick-anderson
description: Tritt ein Laufzeitfehler auf eine Webanwendung in der Produktion ist es wichtig, die einen Entwickler zu benachrichtigen und den Fehler protokollieren, damit es auf eine la diagnostiziert werden kann...
ms.author: aspnetcontent
ms.date: 06/09/2009
ms.assetid: 051296f0-9519-4e78-835c-d868da13b0a0
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/processing-unhandled-exceptions-vb
msc.type: authoredcontent
ms.openlocfilehash: f925a2e2a8cf2785aa2df89c82d2a29965a543d9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37835229"
---
<a name="processing-unhandled-exceptions-vb"></a>Verarbeiten von Ausnahmefehlern (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnet/web-forms/overview/older-versions-getting-started/deploying-web-site-projects/processing-unhandled-exceptions-vb/samples) ([Vorgehensweise zum Herunterladen](/aspnet/core/tutorials/index#how-to-download-a-sample))

> Tritt ein Laufzeitfehler auf eine Webanwendung in der Produktion ist es wichtig, die einen Entwickler zu benachrichtigen und den Fehler protokollieren, damit es zu einem späteren Zeitpunkt rechtzeitig diagnostiziert werden kann. Dieses Lernprogramm bietet einen Überblick darüber, wie ASP.NET Laufzeitfehler verarbeitet und untersucht eine Möglichkeit, benutzerdefinierten Code, wenn eine nicht behandelte Ausnahme Blasen bis zu der ASP.NET-Laufzeit ausgeführt haben.


## <a name="introduction"></a>Einführung

Tritt eine nicht behandelte Ausnahme in einer ASP.NET-Anwendung, Positionen bis zu der ASP.NET-Laufzeit, die löst die `Error` Ereignis und zeigt die Seite für die entsprechenden Fehler. Es gibt drei verschiedene Typen von Fehlerseiten: die Common Language Runtime Fehler gelben Bildschirm der Tod (YSOD); die Ausnahmedetails YSOD; und benutzerdefinierte Fehlerseiten. In der [vorherigen Lernprogramm](displaying-a-custom-error-page-vb.md) wir die Anwendung zur Verwendung einer benutzerdefinierten Fehlerseite für Remotebenutzer und die Ausnahme Details YSOD für Benutzer, die Zugriff auf lokal konfiguriert.

Eine Benutzerfreundlicher benutzerdefinierte Fehlerseite, die das Aussehen und Verhalten des Standorts entspricht ist die Verwendung auf den Standardwert YSOD der Common Language Runtime-Fehler, aber Anzeigen einer benutzerdefinierten Fehlerseite wird nur ein Teil einer umfassenden Lösung für die Fehlerbehandlung. Tritt ein Fehler in einer Anwendung in der Produktion, ist es wichtig, dass die Entwickler den Fehler benachrichtigt werden, damit sie die Ursache der Ausnahme entdecken und es beheben können. Darüber hinaus sollte die Fehlerdetails des protokolliert werden, so, dass der Fehler untersucht und zu einem späteren Zeitpunkt diagnostiziert werden kann.

In diesem Tutorial wird gezeigt, wie Sie die Details einer nicht behandelten Ausnahme Zugriff, sodass sie protokolliert werden können und ein Entwickler, die benachrichtigt. Die zwei befolgen diese Tutorials werden Fehler Bibliotheken, die nach einer gewissen Konfiguration automatisch benachrichtigen, Entwickler von Laufzeitfehlern und melden Sie sich ihre Details.

> [!NOTE]
> Die Informationen, die untersucht, die in diesem Tutorial sind besonders hilfreich, wenn nicht behandelte Ausnahmen auf eindeutige oder benutzerdefinierte Weise verarbeitet werden muss. In Fällen, in dem Sie müssen nur die Ausnahme protokollieren und einen Entwickler zu benachrichtigen, ist eine Fehler Protokollierung Bibliotheken die beste Wahl. Die nächsten beiden Lernprogramme bieten einen Überblick über diese Bibliotheken.


## <a name="executing-code-when-theerrorevent-is-raised"></a>Ausführen von Code, wenn die`Error`Ereignis wird ausgelöst.

Ereignisse stellen ein Objekt einen Mechanismus, um anzugeben, dass etwas Interessantes aufgetreten ist, und klicken Sie für ein anderes Objekt zum Ausführen von Code in der Antwort bereit. Als ASP.NET-Entwickler sind Sie daran gewöhnt, zu Überlegungen in Bezug auf Ereignisse. Sollten Sie Code ausführen, wenn der Besucher eine bestimmte Schaltfläche klickt, erstellen Sie einen Ereignishandler für die Schaltfläche `Click` Ereignis und fügen Sie Ihren Code vorhanden. Angesichts der Tatsache, dass die ASP.NET-Laufzeit löst die [ `Error` Ereignis](https://msdn.microsoft.com/library/system.web.httpapplication.error.aspx) , wenn eine nicht behandelte Ausnahme auftritt, daraus folgt, dass der Code für die Protokollierung des Fehlers Details in einem Ereignishandler gehen würde. Aber wie erstellen Sie einen Ereignishandler für die `Error` Ereignis?

Die `Error` -Ereignis ist eines von vielen Ereignissen in der [ `HttpApplication` Klasse](https://msdn.microsoft.com/library/system.web.httpapplication.aspx) , die während der Lebensdauer einer Anforderung in bestimmten Phasen der HTTP-Pipeline ausgelöst werden. Z. B. die `HttpApplication` Klasse [ `BeginRequest` Ereignis](https://msdn.microsoft.com/library/system.web.httpapplication.beginrequest.aspx) wird am Anfang jeder Anforderung ausgelöst, die [ `AuthenticateRequest` Ereignis](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx) wird ausgelöst, wenn ein Sicherheitsmodul den anforderer identifiziert hat. Diese `HttpApplication` Ereignisse geben Sie dem Seitenentwickler eine Möglichkeit zum Ausführen von benutzerdefinierten Logik zu verschiedenen Zeitpunkten während der Lebensdauer einer Anforderung.

Ereignishandler für die `HttpApplication` Ereignisse in einer speziellen Datei namens platziert werden `Global.asax`. Um diese Datei auf Ihrer Website zu erstellen, fügen Sie ein neues Element in das Stammverzeichnis Ihrer Website mithilfe der Vorlage für die Globale Anwendungsklasse mit dem Namen `Global.asax`.

[![](processing-unhandled-exceptions-vb/_static/image2.png)](processing-unhandled-exceptions-vb/_static/image1.png)

**Abbildung 1**: Hinzufügen `Global.asax` zu Ihrer Webanwendung  
 ([Klicken Sie, um das Bild in voller Größe anzeigen](processing-unhandled-exceptions-vb/_static/image3.png))

Der Inhalt und Struktur der `Global.asax` von Visual Studio erstellte Datei unterscheiden sich geringfügig abhängig, ob Sie eine Web Application Project (WAP) oder die Website-Projekt (WSP) verwenden. Mit einem drahtlosen Zugriffspunkt der `Global.asax` wird als zwei separate Dateien - implementiert `Global.asax` und `Global.asax.vb`. Die `Global.asax` -Datei enthält keine jedoch `@Application` -Direktive, verweist der `.vb` ; das Ereignis Handler von Interesse, in definiert sind der Datei die `Global.asax.vb` Datei. Für WSPs, nur eine einzige Datei erstellt wird, `Global.asax`, und die Ereignishandler in definiert eine `<script runat="server">` Block.

Die `Global.asax` in einem drahtlosen Zugriffspunkt von Visual Studio globale anwendungsklassenvorlage erstellte Datei schließt Ereignishandler, die mit dem Namen `Application_BeginRequest`, `Application_AuthenticateRequest`, und `Application_Error`, denen es sich um Ereignishandler für die `HttpApplication` Ereignisse `BeginRequest`, `AuthenticateRequest`, und `Error`bzw. Es gibt auch Ereignishandler mit dem Namen `Application_Start`, `Session_Start`, `Application_End`, und `Session_End`, Ereignishandler, die ausgelöst werden, wenn die Webanwendung, wenn eine neue Sitzung gestartet wird, startet, wenn von der Anwendung beendet werden, und wenn eine Sitzung beendet wird, bzw. Die `Global.asax` in eine WSP-Datei von Visual Studio erstellte Datei enthält nur die `Application_Error`, `Application_Start`, `Session_Start`, `Application_End`, und `Session_End` -Ereignishandler.

> [!NOTE]
> Beim Bereitstellen der ASP.NET-Anwendung müssen Sie kopieren die `Global.asax` Datei in der produktionsumgebung bereit. Die `Global.asax.vb` -Datei, die in der WAP erstellt wird, muss es sich nicht in der Produktion kopiert werden, da dieser Code in der Assembly des Projekts kompiliert wird.


Die Ereignishandler, die von Visual Studio Globale Anwendungsklasse Vorlage erstellt wurde, sind nicht vollständig. Sie können einen Ereignishandler hinzufügen, für alle `HttpApplication` Ereignisses durch den Namen des ereignishandlers `Application_EventName`. Beispielsweise können Sie den folgenden Code hinzu Hinzufügen der `Global.asax` Datei zum Erstellen eines ereignishandlers für die [ `AuthorizeRequest` Ereignis](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx):

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample1.vb)]

Ebenso können Sie alle Ereignishandler, die von der Vorlage für die Globale Anwendungsklasse erstellten entfernen, die nicht benötigt werden. Für dieses Tutorial benötigen wir nur einen Ereignishandler für die `Error` Ereignis; gerne entfernen Sie die anderen Ereignishandler aus der `Global.asax` Datei.

> [!NOTE]
> *HTTP-Module* bieten eine weitere Möglichkeit zum Definieren von Ereignishandlern für `HttpApplication` Ereignisse. HTTP-Module werden in einer separaten Klassenbibliothek erstellt, als eine Klassendatei, die direkt innerhalb des Webanwendungsprojekts platziert oder ausgelagert werden kann. Da sie in eine Klassenbibliothek verteilt werden können, bieten HTTP-Module ein flexibler und wieder verwendbaren Modell zum Erstellen von `HttpApplication` -Ereignishandler. Während der `Global.asax` Datei bezieht sich auf die Webanwendung, in dem er sich befindet, kann HTTP-Module in Assemblys, die an diesem Punkt das HTTP-Modul auf einer Website hinzufügen so einfach ist wie das Löschen der Assemblys in kompiliert werden die `Bin` Ordner und Registrieren der Modul im `Web.config`. In diesem Tutorial scheint sich nicht in die Erstellung und Verwendung von HTTP-Modulen, aber die beiden fehlerprotokollierung-Bibliotheken, die in den folgenden beiden Tutorials verwendet werden als HTTP-Module implementiert. Weitere Hintergrundinformationen zu den Vorteilen der HTTP-Modulen finden Sie unter [mithilfe von HTTP-Module und Handler, um Komponenten von austauschbaren ASP.NET erstellen](https://msdn.microsoft.com/library/aa479332.aspx).


## <a name="retrieving-information-about-the-unhandled-exception"></a>Abrufen von Informationen zu nicht behandelten Ausnahme

An diesem Punkt haben wir eine Datei "Global.asax" mit einem `Application_Error` -Ereignishandler. Bei der Ausführung dieser Ereignishandler müssen wir Entwickler den Fehler zu benachrichtigen, und melden Sie sich die Details. Für diese Aufgaben müssen wir zuerst auf die Details der Ausnahme zu bestimmen, die ausgelöst wurde. Verwenden Sie des Serverobjekts [ `GetLastError` Methode](https://msdn.microsoft.com/library/system.web.httpserverutility.getlasterror.aspx) beim Abrufen der Details der nicht behandelten Ausnahme, die aufgrund der `Error` Ereignis ausgelöst.

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample2.vb)]

Die `GetLastError` Methode gibt ein Objekt vom Typ `Exception`, dies ist der Basistyp für alle Ausnahmen in .NET Framework. Der obige Code habe ich jedoch das Ausnahmeobjekt, das vom umgewandelt `GetLastError` in einer `HttpException` Objekt. Wenn die `Error` Ereignis wird ausgelöst werden, da eine Ausnahme während der Verarbeitung einer ASP.NET-Ressource ausgelöst wurde, und klicken Sie dann die ausgelöste Ausnahme, innerhalb umschlossen wird einer `HttpException`. Um die eigentliche Ausnahme erhalten, die die Verwendung der Fehler Ereignis unmittelbar vorausging der `InnerException` Eigenschaft. Wenn die `Error` -Ereignisses aufgrund einer Ausnahme von HTTP-basierte, wie z. B. eine Anforderung für eine nicht existierende-Seite, ein `HttpException` ausgelöst wird, aber nicht über eine innere Ausnahme verfügt.

Der folgende code verwendet die `GetLastErrormessage` zum Abrufen von Informationen über die Ausnahme, die ausgelöst der `Error` -Ereignis, das Speichern von der `HttpException` in einer Variablen namens `lastErrorWrapper`. Es speichert dann den Typ, Nachricht und die stapelüberwachung der ursprünglichen Ausnahme in drei Zeichenfolgenvariablen, es wird überprüft, wenn die `lastErrorWrapper` die tatsächliche Ausnahme, die ausgelöst wird die `Error` -Ereignis (im Fall von HTTP-basierte Ausnahmen) oder wenn sie lediglich eine Wrapper für eine Ausnahme, die beim Verarbeiten der Anforderung ausgelöst wurde.

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample3.vb)]

An diesem Punkt müssen Sie alle Informationen, dass Code schreiben, der die Ausnahme, die Informationen in einer Datenbanktabelle protokolliert werden sollen. Sie können eine Datenbanktabelle mit Spalten für jeden die Fehlerdetails, die von Interesse – der Typ, der Nachricht, die stapelüberwachung und usw. - sowie andere nützliche Informationen, z. B. die URL der angeforderten Seite und den Namen des aktuell angemeldeten Benutzers erstellen. In der `Application_Error` Ereignishandler, die Sie dann eine Verbindung mit der Datenbank herstellen und einen Datensatz in die Tabelle einfügen. Ebenso können Sie Code aus, um Entwickler den Fehler per e-Mail Warnungen hinzufügen.

Der Fehler protokollierungsbibliotheken untersucht, die in den nächsten beiden Tutorials bieten solche Funktionen standardmäßig, daher keine Notwendigkeit besteht, diese fehlerprotokollierung und die Benachrichtigung selbst erstellen. Allerdings auf, die veranschaulichen die `Error` Ereignis ausgelöst wird und dass der `Application_Error` Ereignishandler dienen zum Protokollieren von Fehlerdetails, Entwickler zu benachrichtigen, und fügen Sie Code, der einen Entwickler benachrichtigt, wenn ein Fehler auftritt.

## <a name="notifying-a-developer-when-an-unhandled-exception-occurs"></a>Einen Entwickler benachrichtigen, wenn eine nicht behandelte Ausnahme auftritt.

Tritt eine nicht behandelte Ausnahme in der produktionsumgebung ist es wichtig, um das Entwicklungsteam informieren und sie können den Fehler zu bewerten und bestimmen, welche Aktionen ausgeführt werden müssen. Beispielsweise liegt ein Fehler beim Herstellen einer Verbindung mit der Datenbank müssen Sie nach double überprüfen Sie die Verbindungszeichenfolge und, vielleicht ein supportticket mit Ihrem Webhosting. Wenn die Ausnahme aufgrund eines Programmierung ein Fehler aufgetreten ist, müssen zusätzlichen Code oder Validierungslogik hinzugefügt werden, um solche Fehler in der Zukunft zu vermeiden.

Die .NET Framework-Klassen der [ `System.Net.Mail` Namespace](https://msdn.microsoft.com/library/system.net.mail.aspx) erleichtern Ihnen eine e-Mail zu senden. Die [ `MailMessage` Klasse](https://msdn.microsoft.com/library/system.net.mail.mailmessage.aspx) stellt eine e-Mail-Nachricht und verfügt über Eigenschaften wie `To`, `From`, `Subject`, `Body`, und `Attachments`. Die `SmtpClass` dient zum Senden einer `MailMessage` -Objekt mit einem angegebenen SMTP-Server, die SMTP-servereinstellungen können programmgesteuert oder deklarativ angegeben werden, die [ `<system.net>` Element](https://msdn.microsoft.com/library/6484zdc1.aspx) in die `Web.config file`. Weitere Informationen zum Senden von e-Mails Nachrichten in einer ASP.NET-Anwendung finden Sie meinen Artikel [Senden von e-Mail-Adresse in ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx), und die [System.Net.Mail – häufig gestellte Fragen](http://systemnetmail.com/).

> [!NOTE]
> Die `<system.net>` Element enthält, die SMTP-servereinstellungen ein, die die `SmtpClient` -Klasse beim Senden einer e-Mail. Ihre Webhostingunternehmen wahrscheinlich hat es sich um einen SMTP-Server, den Sie zum Senden von e-Mails von Ihrer Anwendung verwenden können. Informationen zu den SMTP-Server-Einstellungen, die Sie in Ihrer Webanwendung verwenden sollten finden Sie in Abschnitt für Ihre Web-Host-Support.


Fügen Sie den folgenden Code der `Application_Error` -Ereignishandler einem Entwickler eine e-Mail senden, wenn ein Fehler auftritt:

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample4.vb)]

Während der obige Code ziemlich umfassend ist, erstellt es der Großteil den HTML-Code, der angezeigt wird in die e-Mail-Adresse für den Entwickler gesendet. Der Code beginnt mit Verweisen auf die `HttpException` zurückgegebenes der `GetLastError` Methode (`lastErrorWrapper`). Die tatsächliche Ausnahme, die von der Anforderung ausgelöst wurde, wird über abgerufen `lastErrorWrapper.InnerException` und der Variablen zugewiesen `lastError`. Der Typ, Nachricht und Stack Ablaufverfolgungsinformationen aus abgerufen `lastError` und in drei String-Variablen gespeichert.

Als Nächstes wird ein `MailMessage` Objekt mit dem Namen `mm` erstellt wird. Der e-Mail-Text HTML-Format und zeigt die URL der angeforderten Seite, die den Namen der aktuell angemeldeten Benutzers und Informationen zur Ausnahme (der Typ, Nachrichten und stapelüberwachung). Eines der coolen Dinge bei der `HttpException` Klasse ist, dass Sie, den HTML-Code verwendet generieren können, um die Ausnahme Details gelben Bildschirm der Tod (YSOD) zu erstellen, indem die [GetHtmlErrorMessage Methode](https://msdn.microsoft.com/library/system.web.httpexception.gethtmlerrormessage.aspx). Diese Methode wird hier verwendet, können Sie das Markup für die Ausnahme Details YSOD abzurufen und an die e-Mail-Adresse als Anlage hinzufügen. Vorsicht: Wenn die Ausnahme, die ausgelöst der `Error` Ereignis ist eine HTTP-basierte-Ausnahme (z. B. eine Anforderung für eine nicht existierende-Seite) die `GetHtmlErrorMessage` Methode zurück `null`.

Der letzte Schritt ist zum Senden der `MailMessage`. Dies erfolgt durch Erstellen eines neuen `SmtpClient` -Methode ab, und rufen die `Send` Methode.

> [!NOTE]
> Zur Verwendung dieses Codes in Ihrer Webanwendung sollten Sie so ändern Sie die Werte in der `ToAddress` und `FromAddress` Konstanten aus support@example.com Ihrem e-Mail-Adresse die e-Mail-Benachrichtigung Fehler gesendet werden sollen und stammen. Sie müssen außerdem Geben Sie den SMTP-servereinstellungen in der `<system.net>` im Abschnitt `Web.config`. Wenden Sie sich an Ihre Webhostinganbieter aus, um zu bestimmen, die SMTP-servereinstellungen zu verwenden.


Mit diesem Code werden jedes Mal, wenn ein Fehler vorliegt wird der Entwickler eine e-Mail-Nachricht gesendet, die den Fehler zusammenfasst und den YSOD. Im vorherigen Tutorial dargelegt einen Laufzeitfehler durch Genre.aspx besuchen und die Übergabe eines ungültigen `ID` Wert über die Abfragezeichenfolge, wie z. B. `Genre.aspx?ID=foo`. Besuchen die Seite mit den `Global.asax` Datei direkt erzeugt dieselbe benutzererfahrung wie im vorherigen Tutorial – in der Entwicklungsumgebung Sie die Ausnahme Details gelben Bildschirm weiterhin of Death, angezeigt werden soll, während in der produktionsumgebung wird die benutzerdefinierte Fehlerseite angezeigt. Zusätzlich zu diesem vorhandenen Verhalten ist der Entwickler eine e-Mail gesendet.

**Abbildung 2** zeigt die e-Mail empfangen wird, wenn das Unternehmen besuchen `Genre.aspx?ID=foo`. Die e-Mail-Text werden zusammengefasst, die Informationen zur Ausnahme, während die `YSOD.htm` Anlage wird der Inhalt angezeigt, die in der Ausnahme Details YSOD angezeigt wird (finden Sie unter **Abbildung 3**).

[![](processing-unhandled-exceptions-vb/_static/image5.png)](processing-unhandled-exceptions-vb/_static/image4.png)

**Abbildung 2**: der Entwickler wird eine e-Mail-Benachrichtigung gesendet, wenn bei eine nicht behandelte Ausnahme  
 ([Klicken Sie, um das Bild in voller Größe anzeigen](processing-unhandled-exceptions-vb/_static/image6.png))

[![](processing-unhandled-exceptions-vb/_static/image8.png)](processing-unhandled-exceptions-vb/_static/image7.png)

**Abbildung 3**: die e-Mail-Benachrichtigung enthält die Details der Ausnahme YSOD als Anlage  
 ([Klicken Sie, um das Bild in voller Größe anzeigen](processing-unhandled-exceptions-vb/_static/image9.png))

## <a name="what-about-using-the-custom-error-page"></a>Verwenden Sie die benutzerdefinierte Fehlerseite was?

In diesem Tutorial wurde gezeigt, wie Sie mit `Global.asax` und `Application_Error` -Ereignishandler, um Code auszuführen, wenn eine unbehandelte Ausnahme auftritt. Insbesondere können wir dieser Ereignishandler verwendet, um die ein Entwickler einen Fehler zu benachrichtigen. Wir könnten, um auch melden Sie sich die Fehlerdetails in einer Datenbank erweitern. Das Vorhandensein der `Application_Error` Ereignishandler wirkt sich nicht auf der Endbenutzer Erfahrung. Sie sehen immer noch die konfigurierten Fehlerseite, es sich um den Fehler Details YSOD YSOD der Common Language Runtime-Fehler oder die benutzerdefinierte Fehlerseite sein.

Es ist natürlich die Frage, ob die `Global.asax` Datei und `Application_Error` Ereignis ist erforderlich, wenn es sich bei Verwendung eine benutzerdefinierten Fehlerseite. Wenn ein Fehler auftritt, dem Benutzer wird die benutzerdefinierte Fehlerseite angezeigt Warum also nicht möglich wir werden den Code so benachrichtigen Sie den Entwickler und die Fehlerdetails in die CodeBehind-Klasse, der die benutzerdefinierte Fehlerseite? Während Sie sicherlich Code auf Code-Behind-Klasse die benutzerdefinierte Fehlerseite hinzufügen können Sie keinen Zugriff auf die Details der Ausnahme, die ausgelöst der `Error` Ereignis aus, wenn das Verfahren verwenden wir in den vorherigen Tutorials behandelt. Aufrufen der `GetLastError` Methodenrückgabe über die benutzerdefinierte Fehlerseite `Nothing`.

Der Grund für dieses Verhalten ist, da die benutzerdefinierte Fehlerseite über eine Umleitung erreicht wird. Löst aus, wenn eine nicht behandelte Ausnahme der ASP.NET-Laufzeitumgebung das ASP.NET-Modul erreicht die `Error` Ereignis (der ausgeführt wird, die `Application_Error` -Ereignishandler) und dann *leitet* der Benutzer die benutzerdefinierte Fehlerseite durch Ausgeben einer `Response.Redirect(customErrorPageUrl)`. Die `Response.Redirect` Methode sendet eine Antwort an den Client mit einem HTTP 302-Statuscode, weist den Browser, um eine neue URL, d. h. die benutzerdefinierte Fehlerseite anfordern. Der Browser fordert dann automatisch an diese neue Seite. Sie können feststellen, dass die benutzerdefinierte Fehlerseite separat angefordert wurde, auf der Seite, in dem der Fehler aufgetreten, da sich der Adressleiste des Browsers auf die URL der benutzerdefinierten Fehler ändert (finden Sie unter **Abbildung 4**).

[![](processing-unhandled-exceptions-vb/_static/image11.png)](processing-unhandled-exceptions-vb/_static/image10.png)

**Abbildung 4**: Wenn ein Fehler auftritt, wird der Browser auf die URL der benutzerdefinierten Fehler umgeleitet  
 ([Klicken Sie, um das Bild in voller Größe anzeigen](processing-unhandled-exceptions-vb/_static/image12.png))

Das Endergebnis ist, antwortet der Server mit dem HTTP 302-Umleitung endet mit die Anforderung, in die nicht behandelte Ausnahme aufgetreten ist. Die nachfolgende Anforderung an die benutzerdefinierte Fehlerseite ist eine neue Anforderung. zu diesem Zeitpunkt die ASP.NET Engine hat die Fehlerinformationen verworfen und darüber hinaus hat keine Möglichkeit, die neue Anforderung für die benutzerdefinierte Fehlerseite nicht behandelte Ausnahme in der vorherigen Anforderung zugeordnet werden soll. Dies ist deshalb `GetLastError` gibt `null` beim Aufrufen durch die benutzerdefinierte Fehlerseite.

Allerdings ist es möglich, dass die benutzerdefinierte Fehlerseite, die ausgeführt werden, während die gleiche Anforderung, die den Fehler verursacht hat. Die [ `Server.Transfer(url)` ](https://msdn.microsoft.com/library/system.web.httpserverutility.transfer.aspx) -Methode überträgt die Ausführung an der angegebenen URL und innerhalb der gleichen Anforderung verarbeitet. Konnte, verschieben Sie den Code in die `Application_Error` -Ereignishandler, um die benutzerdefinierte Fehlerseite CodeBehind-Klasse, und Ersetzen sie im `Global.asax` durch den folgenden Code:

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample5.vb)]

Wenn jetzt eine nicht behandelte Ausnahme auftritt der `Application_Error` Ereignishandler überträgt die Steuerung an die entsprechende benutzerdefinierte Fehlerseite basierend auf den HTTP-Statuscode. Da Steuerelement übertragen wurde, wird die benutzerdefinierte Fehlerseite hat Zugriff auf die nicht behandelte Ausnahmeinformationen über `Server.GetLastError` können Entwickler den Fehler zu benachrichtigen und melden Sie sich die Details. Die `Server.Transfer` Aufruf beendet wird, die ASP.NET-Engine Weiterleiten des Benutzers an die benutzerdefinierte Fehlerseite. Stattdessen wird die benutzerdefinierte Fehlerseite-Inhalt als Antwort auf der Seite "zurückgegeben, die den Fehler generiert hat.

## <a name="summary"></a>Zusammenfassung

Tritt eine nicht behandelte Ausnahme in einer ASP.NET-Webanwendung über die ASP.NET-Laufzeit löst die `Error` Ereignis und die konfigurierte Fehlerseite angezeigt. Wir können die Entwickler, der den Fehler benachrichtigen. Melden Sie sich die Details, oder erstellen Sie einen Ereignishandler für das Fehlerereignis in eine andere Art verarbeiten. Es gibt zwei Möglichkeiten zum Erstellen eines ereignishandlers für `HttpApplication` wie `Error`: in der `Global.asax` Datei oder ein HTTP-Modul. In diesem Tutorial wurde gezeigt, wie zum Erstellen einer `Error` -Ereignishandler in der `Global.asax` -Datei, die Entwickler Fehler mithilfe einer e-Mail-Nachricht benachrichtigt.

Erstellen einer `Error` -Ereignishandler ist nützlich, wenn nicht behandelte Ausnahmen auf eindeutige oder benutzerdefinierte Weise verarbeitet werden muss. Allerdings Erstellen eigener `Error` -Ereignishandler protokolliert die Ausnahme oder einen Entwickler zu benachrichtigen ist nicht die effizienteste Nutzung Ihrer Zeit, wie es bereits vorhanden sind kostenlos und benutzerfreundlich Fehler Bibliotheken, die Einrichtung in wenigen Minuten werden können. Die nächsten beiden Tutorials untersuchen Sie diese Bibliotheken.

Viel Spaß beim Programmieren!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Tutorial erläutert finden Sie in den folgenden Ressourcen:

- [ASP.NET HTTP-Module und HTTP-Handler-Übersicht](https://support.microsoft.com/kb/307985)
- [Ordnungsgemäßes reagieren auf Unbehandelte Ausnahmen – verarbeiten von Ausnahmefehlern](http://aspnet.4guysfromrolla.com/articles/091306-1.aspx)
- [`HttpApplication` Klasse und das ASP.NET Application-Objekt](http://www.eggheadcafe.com/articles/20030211.asp)
- [HTTP-Handler und HTTP-Modulen in ASP.NET](http://www.15seconds.com/Issue/020417.htm)
- [Senden von E-Mails in ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)
- [Grundlegendes zu den `Global.asax` Datei](http://aspalliance.com/1114_Understanding_the_Globalasax_file.all)
- [Verwenden von HTTP-Module und Handler zum Erstellen von ASP.NET-Plug-in-Komponenten](https://msdn.microsoft.com/library/aa479332.aspx)
- [Arbeiten mit ASP.NET `Global.asax` Datei](http://articles.techrepublic.com.com/5100-10878_11-5771721.html)
- [Arbeiten mit `HttpApplication` Instanzen](https://msdn.microsoft.com/library/a0xez8f2.aspx)

> [!div class="step-by-step"]
> [Zurück](displaying-a-custom-error-page-vb.md)
> [Weiter](logging-error-details-with-asp-net-health-monitoring-vb.md)
