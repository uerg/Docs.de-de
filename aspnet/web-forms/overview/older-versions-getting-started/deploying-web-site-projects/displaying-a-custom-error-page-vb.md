---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/displaying-a-custom-error-page-vb
title: Anzeigen einer Fehlerseite (VB) | Microsoft Docs
author: rick-anderson
description: "Was sieht der Benutzer, tritt ein Laufzeitfehler in einer ASP.NET-Webanwendung? Die Antwort abhängig, wie der Website &lt;CustomErrors&gt; Konfiguration..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: 14873c5d-81a9-455b-bd71-30fb555583e7
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/displaying-a-custom-error-page-vb
msc.type: authoredcontent
ms.openlocfilehash: e8a2f88490de08f731f9737d15237ae445c5ec0d
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2018
---
<a name="displaying-a-custom-error-page-vb"></a>Anzeigen einer Fehlerseite (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Herunterladen von Code](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_11_VB.zip) oder [PDF herunterladen](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial11_CustomErrors_vb.pdf)

> Was sieht der Benutzer, tritt ein Laufzeitfehler in einer ASP.NET-Webanwendung? Die Antwort abhängig, wie der Website &lt;CustomErrors&gt; Konfiguration. Standardmäßig werden Benutzer einen bestehen gelben Bildschirm proclaiming, dass ein Laufzeitfehler aufgetreten ist, angezeigt. Dieses Lernprogramm zeigt, wie benutzerdefinierte Fehlerseite anzeigen, die eine unauffälligen ansprechende diese Einstellungen anpassen, das Erscheinungsbild Ihrer Website entspricht.


## <a name="introduction"></a>Einführung

Idealerweise wäre keine Fehler zur Laufzeit. Programmierer Schreiben von Code mit nary einen Fehler, und mit stabile Validierung von Benutzereingaben, und externe Ressourcen wie e-Mail-Servern und -Datenbankservern würden nie offline geschaltet. In Wirklichkeit sind natürlich Fehler unvermeidlich. Die Klassen in .NET Framework einen Fehler durch Auslösen einer Ausnahme zu signalisieren. Z. B. richtet eine SqlConnection Aufrufen des Objekts Open-Methode eine Verbindung mit der Datenbank, die durch eine Verbindungszeichenfolge angegeben. Wenn die Datenbank nicht ausgeführt wird oder wenn die Anmeldeinformationen in der Verbindungszeichenfolge ungültig sind. Klicken Sie dann die Open-Methode löst jedoch eine `SqlException`. Ausnahmen können behandelt werden, durch die Verwendung von `Try/Catch/Finally` blockiert. Wenn code in einem `Try` Block eine Ausnahme auslöst, wird die Steuerung an den entsprechenden Catch-Block kann der Entwickler Versuch, die Verarbeitung fortzusetzen. Wenn kein übereinstimmenden CatchBlock vorhanden ist oder wenn der Code, der die Ausnahme ausgelöst hat, nicht in einem Try-Block ist, wird die Ausnahme der Aufrufliste search von percolates `Try/Catch/Finally` blockiert.

Wenn ganz bis zu die ASP.NET-Laufzeit die Ausnahme ausgelöst, ohne verarbeitet wird, die [ `HttpApplication` Klasse](https://msdn.microsoft.com/library/system.web.httpapplication.aspx)des [ `Error` Ereignis](https://msdn.microsoft.com/library/system.web.httpapplication.error.aspx) wird ausgelöst, und die konfigurierte *Fehler (Seite)*  wird angezeigt. Standardmäßig zeigt ASP.NET eine Fehlerseite, die als affectionately bezeichnet wird, die [gelben Bildschirm Tod](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow) (YSOD). Es gibt zwei Versionen der YSOD: eine zeigt die Details der Ausnahme, die eine stapelüberwachung und andere Informationen für Entwickler, die die Anwendung Debuggen hilfreich (finden Sie unter **Abbildung 1**); die andere einfach gibt an, dass ein Laufzeitfehler ist (siehe aufgetreten **Abbildung 2**).

Die Ausnahmedetails YSOD ist sehr hilfreich für Entwickler, die die Anwendung debuggen, aber klebrig und unprofessionell ist eine YSOD für Endbenutzer angezeigt. Stattdessen müssen Endbenutzer eine Fehlerseite angezeigt. ergriffen werden, die den Standort Aussehen und Verhalten mit benutzerfreundlicheren Programmierkontext, beschreibt die Situation verwaltet. Die gute Nachricht ist, erstellen eine solche benutzerdefinierte Fehlerseite lässt sich ganz einfach. In diesem Lernprogramm wird mit einem Blick auf ASP gestartet. NET Seiten, die ein anderer Fehler. Es wird dargestellt, wie die Webanwendung, um Benutzern eine benutzerdefinierte Fehlerseite bei Fehler anzuzeigen, konfigurieren.

### <a name="examining-the-three-types-of-error-pages"></a>Untersuchen die drei Typen von Fehlerseiten

Wenn eine nicht behandelte Ausnahme in einer ASP.NET-Anwendung einen von drei Typen der Fehlerseiten, die tritt auf, wird angezeigt:

- Die Ausnahme Details gelben Bildschirm des Tod Fehler (Seite)
- Die Fehlerseite Runtime Fehler gelben Bildschirm des zum Tod oder
- Eine benutzerdefinierte Fehlerseite

Fehler-Seite-Entwickler sind, ist am besten vertraut mit der Ausnahme Details YSOD. Standardmäßig wird diese Seite wird angezeigt, bis der Benutzer, die lokal besuchen und aus diesem Grund wird die Seite, die angezeigt werden, wenn ein Fehler auftritt, wenn den Standort in der Entwicklungsumgebung testen. Wie der Name schon sagt, stellt die Ausnahme Details YSOD Details zur Ausnahme – der Typ der Nachricht und die stapelüberwachung. Wenn die Ausnahme von Code in die ASP.NET-Seite Code-Behind-Klasse ausgelöst wurde und die Anwendung für das Debuggen konfiguriert ist dann die Ausnahme Details YSOD ebenfalls diese Codezeile (und einige Zeilen Code oberhalb und unterhalb dieser) angezeigt.

**Abbildung 1** zeigt die Ausnahme Details YSOD ". Beachten Sie die URL in das Browserfenster Adresse: `http://localhost:62275/Genre.aspx?ID=foo`. Bedenken Sie, dass die `Genre.aspx` Seite führt die Buch-Überprüfungen in einer bestimmten "Genre". Es erfordert, dass `GenreId` Wert (eine `uniqueidentifier`) übergeben werden, über die Abfragezeichenfolge; z. B. die entsprechende URL anzeigen prüft der Idee ist `Genre.aspx?ID=7683ab5d-4589-4f03-a139-1c26044d0146`. Wenn eine nicht-`uniqueidentifier` übergebene Wert im über die Abfragezeichenfolge (z. B. "Foo") eine Ausnahme ausgelöst.

> [!NOTE]
> Reproduzieren Sie diesen Fehler in der Demo-Webanwendung zum Download zur Verfügung können Sie entweder Besuch `Genre.aspx?ID=foo` direkt oder klicken Sie auf den Link "Generieren eine Common Language Runtime-Fehler" im `Default.aspx`.


Beachten Sie die ausnahmefehlerinformationen im dargestellt **Abbildung 1**. Ausnahmemeldung: "Fehler beim Konvertieren einer Zeichenfolge in" uniqueidentifier "" ist am oberen Rand der Seite "vorhanden. Der Typ der Ausnahme `System.Data.SqlClient.SqlException`, wird ebenfalls aufgeführt. Es ist auch die stapelüberwachung.

[![](displaying-a-custom-error-page-vb/_static/image2.png)](displaying-a-custom-error-page-vb/_static/image1.png)

**Abbildung 1**: die Ausnahmedetails YSOD enthält Informationen zur Ausnahme  
 ([Klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-a-custom-error-page-vb/_static/image3.png))

Die andere Art von YSOD YSOD der Common Language Runtime-Fehler ist, und auf **Abbildung 2**. Die Common Language Runtime-Fehler YSOD informiert den Besucher, den ein Laufzeitfehler aufgetreten ist, aber es umfasst keine Informationen über die Ausnahme, die ausgelöst wurde. (Es der Fall ist, Anweisungen jedoch bereitstellen, wie die Fehlerdetails können durch Ändern der `Web.config` -Datei, die macht eine YSOD, die von unprofessionell stammt.)

Standardmäßig YSOD der Common Language Runtime-Fehler wird angezeigt, die Benutzer besuchen Remote (über http://www.yoursite.com), wie durch die URL in die Adressleiste des Browsers belegt **Abbildung 2**: `http://httpruntime.web703.discountasp.net/Genre.aspx?ID=foo`. Die zwei verschiedenen YSOD Bildschirme vorhanden sein, da Entwickler regionale die Fehlerdetails, sondern solche Informationen sollten auf einem live-Standort nicht angezeigt, wie sie potenzielle Sicherheitsrisiken oder andere sensible Informationen für alle Benutzer besucht Offenlegen möglicherweise Ihre Standort.

> [!NOTE]
> Wenn Sie sind die folgenden zusammen und DiscountASP.NET wie auf dem Webhost verwenden, werden Sie möglicherweise feststellen, dass die Common Language Runtime-Fehler YSOD beim Besuch der live-Standorts nicht angezeigt wird. Dies ist da DiscountASP.NET ihre Server so konfiguriert, dass die Ausnahme Details YSOD anzeigen in der Standardeinstellung verfügt. Die gute Nachricht ist, dass Sie dieses Standardverhalten, indem hinzufügen überschreiben können einer `<customErrors>` Abschnitt Ihrer `Web.config` Datei. Im Abschnitt "Konfigurieren der Fehler angezeigt wird" untersucht die `<customErrors>` Abschnitt detailliert.


[![](displaying-a-custom-error-page-vb/_static/image5.png)](displaying-a-custom-error-page-vb/_static/image4.png)

**Abbildung 2**: Laufzeitfehler YSOD schließt keine Details zum Systemfehler  
 ([Klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-a-custom-error-page-vb/_static/image6.png))

Der dritte Typ der Seite "Fehler" ist die benutzerdefinierte Fehlerseite, also eine Webseite, die Sie erstellen. Der Vorteil, dass eine benutzerdefinierte Fehlerseite ist die vollständige Kontrolle über die Informationen vorliegen, die dem Benutzer zusammen mit der Seite aussehen und Verhalten angezeigt wird. die benutzerdefinierte Fehlerseite können die gleichen Gestaltungsvorlage und Formatvorlagen als alle anderen Seiten. Im Abschnitt "Verwenden eines benutzerdefinierten Fehlerseite" führt Sie durch eine benutzerdefinierte Fehlerseite erstellen und konfigurieren, um im Falle einer nicht behandelten Ausnahme anzuzeigen. **Abbildung 3** bietet einen Vorgeschmack, der diese benutzerdefinierte Fehlerseite. Wie Sie sehen können, ist das Aussehen und Verhalten der Fehlerseite viel mehr aussehende als eines der gelben Bildschirme des Tod in Abbildung 1 und 2 dargestellt.

[![](displaying-a-custom-error-page-vb/_static/image8.png)](displaying-a-custom-error-page-vb/_static/image7.png)

**Abbildung 3**: eine benutzerdefinierte Fehlerseite bietet eine weitere angepasste Aussehen und Verhalten  
 ([Klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-a-custom-error-page-vb/_static/image9.png))

Erkundet, überprüfen Sie in der Adressleiste des Browsers in **abbildung3**. Beachten Sie, dass die Adressleiste die URL für die benutzerdefinierte Fehlerseite zeigt (`/ErrorPages/Oops.aspx`). In Abbildung 1 und 2 werden dem gelben Bildschirme Tod angezeigt, auf der gleichen Seite, die von der Fehler verursacht wurde (`Genre.aspx`). Die benutzerdefinierte Fehlerseite übergeben wird die URL der Seite, in dem der Fehler aufgetreten, über ist, die `aspxerrorpath` Querystring-Parameter.

## <a name="configuring-which-error-page-is-displayed"></a>Konfigurieren die Fehlerseite wird angezeigt.

Die drei möglichen Fehlern Seiten angezeigt wird, basiert auf zwei Variablen:

- Die Konfigurationsinformationen in der `<customErrors>` Abschnitt, und
- Gibt an, ob der Benutzer die Website lokal oder Remote besucht wird.

Die [ `<customErrors>` Abschnitt](https://msdn.microsoft.com/library/h0hfz6fc.aspx) in `Web.config` verfügt über zwei Attribute, die beeinflussen, welche Seite "Fehler" angezeigt wird: `defaultRedirect` und `mode`. Das `defaultRedirect`-Attribut ist optional. Wenn angegeben, gibt die URL für die benutzerdefinierte Fehlerseite an und gibt an, dass die benutzerdefinierte Fehlerseite soll, anstatt die Common Language Runtime-Fehler YSOD angezeigt werden. Die `mode` Attribut ist erforderlich und akzeptiert einen von drei Werten: `On`, `Off`, oder `RemoteOnly`. Diese Werte weisen folgendes Verhalten auf:

- `On`-Gibt an, dass die benutzerdefinierte Fehlerseite oder YSOD der Common Language Runtime-Fehler, um alle Besucher konfigurieren angezeigt wird, unabhängig davon, ob sie lokal oder remote.
- `Off`-Gibt an, dass die Ausnahme Details YSOD, um alle Besucher konfigurieren angezeigt wird, unabhängig davon, ob sie lokal oder remote.
- `RemoteOnly`-Gibt an, dass die benutzerdefinierte Fehlerseite oder der Common Language Runtime-Fehler YSOD remote Besucher angezeigt wird, während die Ausnahme Details YSOD lokalen Besucher angezeigt wird.

Sofern nicht anders angegeben, wird ASP.NET verhält, als ob Sie die Mode-Attribut, um festgelegt wurden `RemoteOnly` wurde nicht angegeben, und ein `defaultRedirect` Wert. Das heißt, ist das Standardverhalten, dass die Ausnahme Details YSOD lokalen Besucher angezeigt wird, während der Laufzeit Fehler YSOD remote Besucher angezeigt wird. Sie können dieses Standardverhalten außer Kraft setzen, durch Hinzufügen einer `<customErrors>` Abschnitt aus, um Ihre Web-Anwendungsverzeichnis`Web.config file.`

## <a name="using-a-custom-error-page"></a>Verwenden eine benutzerdefinierte Fehlerseite

Jede Webanwendung müssen eine benutzerdefinierte Fehlerseite. Er bietet eine weitere aussehende Alternative zum YSOD Common Language Runtime-Fehler, es ist einfach zu erstellen und konfigurieren die Anwendung verwendet die benutzerdefinierte Fehlerseite nimmt nur ein paar Minuten noch einmal. Der erste Schritt ist die benutzerdefinierte Fehlerseite erstellen. Ich habe einen neuen Ordner hinzugefügt, der mit dem Namen Book Reviews-Anwendung `ErrorPages` und hinzugefügt, dass eine neue ASP.NET-Seite namens `Oops.aspx`. Haben Sie die Seite, die dieselbe Masterseite wie der Rest der Seiten auf Ihrer Website zu verwenden, damit sie automatisch das gleiche Erscheinungsbild erbt.

[![](displaying-a-custom-error-page-vb/_static/image11.png)](displaying-a-custom-error-page-vb/_static/image10.png)

**Abbildung 4**: Erstellen Sie eine benutzerdefinierte Fehlerseite

Als Nächstes verbringen Sie einige Minuten, die den Inhalt für die Fehlerseite erstellen. Ich haben eine recht einfach, benutzerdefinierte Fehlerseite mit einer Meldung aufgefordert, ein unerwarteter Fehler aufgetreten, und sichern ein Link zur Startseite der Website erstellt.

[![](displaying-a-custom-error-page-vb/_static/image13.png)](displaying-a-custom-error-page-vb/_static/image12.png)

**Abbildung 5**: Entwerfen Sie Ihre benutzerdefinierte Fehlerseite  
 ([Klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-a-custom-error-page-vb/_static/image14.png))

Konfigurieren Sie mit die Fehlerseite, die abgeschlossen wird die Webanwendung, die benutzerdefinierte Fehlerseite anstelle von der Common Language Runtime-Fehler YSOD verwenden. Dies erfolgt durch Angabe der URL der Fehlerseite in der `<customErrors>` Bereich "Benutzerportal" `defaultRedirect` Attribut. Fügen Sie das folgende Markup für Ihre Anwendungsverzeichnis `Web.config` Datei:

[!code-xml[Main](displaying-a-custom-error-page-vb/samples/sample1.xml)]

Die oben genannten Markup konfiguriert die Anwendung aus, um die Ausnahme Details YSOD Besuchern lokal, bei der Verwendung der benutzerdefinierte Fehlerseite Oops.aspx für diese Benutzer besuchen Remote anzuzeigen. Um dies in Aktion sehen, Bereitstellen Sie Ihre Website in der produktionsumgebung, und besuchen Sie dann die Seite "Genre.aspx" auf der live-Website mit einem ungültigen Querystring-Wert. Daraufhin sollte die benutzerdefinierte Fehlerseite (verweisen zurück auf **Abbildung 3**).

Um sicherzustellen, dass die benutzerdefinierte Fehlerseite für Remotebenutzer nur angezeigt werden, finden Sie auf der `Genre.aspx` Seite mit einer ungültigen Abfragezeichenfolge aus der Entwicklungsumgebung. Daraufhin sollte immer noch die Ausnahme Details YSOD (verweisen zurück auf **Abbildung 1**). Die `RemoteOnly` Einstellung wird sichergestellt, dass Benutzer der Website in der produktionsumgebung die benutzerdefinierte Fehlerseite angezeigt, während Entwickler, die lokal arbeiten fortfahren, um die Details der Ausnahme anzuzeigen.

## <a name="notifying-developers-and-logging-error-details"></a>Entwickler zu benachrichtigen und Fehlerdetails protokollieren

In der Entwicklungsumgebung auftretende Fehler wurden durch den Entwickler, die sich direkt auf ihrem Computer verursacht. Sie Informationen zu der Ausnahme in der Ausnahme Details YSOD angezeigt wird, und er weiß, welche Schritte sie durchgeführt wurde, als der Fehler auftrat. Jedoch tritt ein Fehler auf, hat der Entwickler keine Kenntnis, dass ein Fehler aufgetreten ist, es sei denn, der Website für den Endbenutzer die Zeit, die den Fehler zu melden. Und auch wenn der Benutzer von seinem Möglichkeit, das Entwicklungsteam zu warnen, das ohne Kenntnis der Ausnahmetyp, die Meldung und die stapelüberwachung, die es schwierig sein kann, um die Ursache des Fehlers, ganz zu schweigen Behebung des Problems ist ein Fehler aufgetreten, verlässt.

Aus diesen Gründen ist es unabdinglich, dass alle Fehler in der produktionsumgebung zu einem persistenten Speicher (z. B. eine Datenbank), die protokolliert wird gewarnt werden die Entwicklern für diesen Fehler. Die benutzerdefinierte Fehlerseite mag wie einen guten Ausgangspunkt für diese Protokollierung auch die Benachrichtigung ausgeführt werden. Leider wird die benutzerdefinierte Fehlerseite hat keinen Zugriff auf die Fehlerdetails, und kann daher nicht verwendet werden, um diese Informationen zu protokollieren. Die gute Nachricht ist, eine Reihe von Möglichkeiten stehen, um die Fehlerdetails abzufangen und diese Anmeldung, und die nächsten drei Lernprogramme in diesem Thema im Detail zu untersuchen.

## <a name="using-different-custom-error-pages-for-different-http-error-statuses"></a>Verwenden unterschiedliche benutzerdefinierte Fehlerseiten für verschiedene Fehler der HTTP-Status

Wenn eine Ausnahme von einer ASP.NET-Seite ausgelöst und nicht behandelt wird, percolates Ausnahme bis zu die ASP.NET-Laufzeit, in dem die konfigurierten Fehlerseite angezeigt. Wenn eine Anforderung an das Modul ASP.NET stammen, aber nicht verarbeitet werden, aus irgendeinem Grund – vielleicht die angeforderte Datei wurde nicht gefunden oder gelesen wurden Berechtigungen für die Datei - deaktiviert das Modul für ASP.NET und löst anschließend ein `HttpException`. Diese Ausnahme aus, wie von ASP.NET-Seiten aus aufzurufen, ausgelöste Ausnahmen ausgelöst, bis zu der Common Language Runtime, wodurch der entsprechende Fehler angezeigt werden.

Dies bedeutet für die Webanwendung in der Produktion ist, dass ein Benutzer eine Seite anfordert, die nicht gefunden wird, und klicken Sie dann die benutzerdefinierte Fehlerseite angezeigt. **Abbildung 6** zeigt ein Beispiel für eine solche. Da eine nicht existierende Seite angefordert wird (`NoSuchPage.aspx`), wird ein `HttpException` wird ausgelöst, und die benutzerdefinierte Fehlerseite angezeigt (Beachten Sie den Verweis auf `NoSuchPage.aspx` in die `aspxerrorpath` Querystring-Parameter).

[![](displaying-a-custom-error-page-vb/_static/image16.png)](displaying-a-custom-error-page-vb/_static/image15.png)**Abbildung 6**: die ASP.NET-Laufzeit zeigt die konfigurierten Fehlerseite als Antwort auf eine ungültige Anforderung an.  
 ([Klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-a-custom-error-page-vb/_static/image17.png)) 

Standardmäßig bewirken dass alle Arten von Fehlern die gleiche benutzerdefinierte Fehlerseite angezeigt werden. Sie können jedoch angeben, eine andere benutzerdefinierte Fehlerseite für einen bestimmten HTTP-Status Code mit `<error>` untergeordneter Elemente innerhalb der `<customErrors>` Abschnitt. Um eine andere Fehlerseite angezeigt, die im Falle einer Seite nicht gefunden-Fehler, die HTTP-Statuscode 404 aufweist, z. B. Aktualisieren der `<customErrors>` Abschnitt aus, um das folgende Markup enthalten:

[!code-xml[Main](displaying-a-custom-error-page-vb/samples/sample2.xml)]

Durch diese Änderung vorhanden, wenn ein Benutzer Zugriff auf Remote eine ASP.NET-Ressource, die nicht vorhanden ist anfordert, wird er umgeleitet an die `404.aspx` benutzerdefinierte Fehlerseite anstelle von `Oops.aspx`. Als **Abbildung 7** veranschaulicht, die `404.aspx` Seite eine spezifische Nachricht als das allgemeine benutzerdefinierte Fehlerseite einschließen kann.

> [!NOTE]
> Auschecken [404 Fehlerseiten mehr einmal](http://www.smashingmagazine.com/2009/01/29/404-error-pages-one-more-time/) Anleitungen zum effektiven Fehler 404-Seiten erstellen.


[![](displaying-a-custom-error-page-vb/_static/image19.png)](displaying-a-custom-error-page-vb/_static/image18.png)**Abbildung 7**: die Seite "Benutzerdefiniert 404-Fehler" in einer Meldung spezielleren als`Oops.aspx`  
 ([Klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-a-custom-error-page-vb/_static/image20.png)) 

Da Sie wissen, dass die `404.aspx` Seite erreicht ist nur, wenn der Benutzer für eine Seite anfordert, die nicht gefunden wurde, können Sie dieses benutzerdefinierte Fehlerseite Einbeziehung von Funktionen bereit, mit dem Benutzer, die diese bestimmte Art von Fehler zu beheben helfen verbessern. Beispielsweise erstellen Sie eine Datenbanktabelle, die ordnet ungültige URLs, die gute URLs bekannt werden konnte, und lassen Sie dann die `404.aspx` benutzerdefinierte Fehlerseite Ausführen eine Abfrage, die Tabelle und Seiten, die der Benutzer zu erreichen versuchen möglicherweise vorschlagen.

> [!NOTE]
> Die benutzerdefinierte Fehlerseite wird nur angezeigt, wenn eine Anforderung auf eine Ressource, die durch das Modul für ASP.NET behandelt erfolgt. Wie in erläutert die [Core Unterschiede zwischen IIS und ASP.NET Development Server](core-differences-between-iis-and-the-asp-net-development-server-vb.md) Tutorial, der Webserver möglicherweise bestimmte Anforderungen selbst behandeln. Standardmäßig web IIS Prozesse serveranforderungen bei statischen Inhalten wie Bilder und HTML-Dateien, ohne das ASP.NET-Modul aufrufen. Deshalb, wenn der Benutzer eine nicht vorhandene Abbilddatei anfordert erhalten sie wieder 404 IISs-Standardfehlermeldung anstelle der ASP. NET konfigurierten Fehlerseite.


## <a name="summary"></a>Zusammenfassung

Wenn in einer ASP.NET-Anwendung eine nicht behandelte Ausnahme auftritt, wird dem Benutzer angezeigt eine der drei Fehlerseiten,: die Ausnahme Details gelben Bildschirm des Tod; Laufzeitfehler Gelb Absturz; oder eine benutzerdefinierte Fehlerseite. Die Seite "Fehler" angezeigt wird, hängt davon ab, der Anwendungsverzeichnis `<customErrors>` Konfigurations- und gibt an, ob der Benutzer lokal oder Remote besucht wird. Das Standardverhalten ist die Ausnahme Details YSOD Besucher der lokalen und der Common Language Runtime-Fehler YSOD remote Besucher.

Während der Laufzeit Fehler YSOD potenziell vertrauliche Fehlerinformationen von der Website für den Benutzer ausgeblendet, unterbricht aus Erscheinungsbild Ihrer Website und macht Ihre Anwendung fehlerhaftes zu suchen. Ein besserer Ansatz ist eine benutzerdefinierte Fehlerseite verwendet wird, die umfasst das Erstellen und entwerfen die benutzerdefinierte Fehlerseite angegeben wird und die URL in die `<customErrors>` Bereich "Benutzerportal" `defaultRedirect` Attribut. Sie können auch mehrere benutzerdefinierte Fehlerseiten für die verschiedenen Status der HTTP-Fehler haben.

Die benutzerdefinierte Fehlerseite ist der erste Schritt in einer umfassenden Strategie für eine Website in der Produktion für die Fehlerbehandlung an. Den Entwickler des Fehlers Warnungen und Protokollierung die Details sind auch wichtige Schritte. Die nächsten drei Lernprogramme untersuchen Techniken für die Benachrichtigung über einen Fehler und Protokollierung.

Viel Spaß beim Programmieren!

## <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Lernprogramm erläutert finden Sie in den folgenden Ressourcen:

- [Fehlerseiten, noch einmal](http://www.smashingmagazine.com/2009/01/29/404-error-pages-one-more-time/)
- [Entwurfsrichtlinien für Ausnahmen](https://msdn.microsoft.com/library/ms229014.aspx)
- [Benutzerfreundliche Fehlerseiten](http://aspnet.4guysfromrolla.com/articles/090606-1.aspx)
- [Behandeln und Auslösen von Ausnahmen](https://msdn.microsoft.com/library/5b2yeyab.aspx)
- [Verwenden benutzerdefinierte Fehlerseiten ordnungsgemäß in ASP.NET](http://professionalaspnet.com/archive/2007/09/30/Properly-Using-Custom-Error-Pages-in-ASP.NET.aspx)

>[!div class="step-by-step"]
[Zurück](strategies-for-database-development-and-deployment-vb.md)
[Weiter](processing-unhandled-exceptions-vb.md)
