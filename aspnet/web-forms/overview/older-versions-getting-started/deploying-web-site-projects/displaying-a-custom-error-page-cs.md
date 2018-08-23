---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/displaying-a-custom-error-page-cs
title: Anzeigen einer benutzerdefinierten Fehlerseite (c#) | Microsoft-Dokumentation
author: rick-anderson
description: Was sieht der Benutzer, tritt ein Laufzeitfehler in einer ASP.NET-Webanwendung? Die Antwort hängt wie der Website &lt;CustomErrors&gt; Konfiguration...
ms.author: riande
ms.date: 06/09/2009
ms.assetid: cb061642-faf3-41b2-9372-69e13444d458
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/displaying-a-custom-error-page-cs
msc.type: authoredcontent
ms.openlocfilehash: 449b8eb26f3f6018fdd6c6dcc1de1c8d58214ac3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837098"
---
<a name="displaying-a-custom-error-page-c"></a>Anzeigen einer benutzerdefinierten Fehlerseite (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_11_CS.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial11_CustomErrors_cs.pdf)

> Was sieht der Benutzer, tritt ein Laufzeitfehler in einer ASP.NET-Webanwendung? Die Antwort hängt wie der Website &lt;CustomErrors&gt; Konfiguration. Benutzer werden standardmäßig einen unschöner gelben Bildschirm Advantages, dass ein Laufzeitfehler aufgetreten angezeigt. In diesem Tutorial zeigt, wie Sie diese Einstellungen zur benutzerdefinierten Fehlerseite anzeigen, die eine ästhetisch ansprechendste anpassen, die Ihrer Website Aussehen und Verhalten entspricht.


## <a name="introduction"></a>Einführung

In einer perfekten Welt würden keine Laufzeitfehler vorhanden sein. Programmierer würde schreiben Sie Code mit nary einen Fehler aus, und mit stabilen Validierung von Benutzereingaben, und externe Ressourcen wie Datenbankservern und e-Mail-Server würde niemals offline geschaltet. Natürlich sind in der Praxis Fehler unvermeidlich. Die Klassen in .NET Framework signalisiert einen Fehler durch Auslösen einer Ausnahme. Z. B. eine "SqlConnection" Aufrufen des Objekts Open-Methode stellt eine Verbindung her, der durch eine Verbindungszeichenfolge angegebenen Datenbank. Wenn die Datenbank offline ist oder wenn die Anmeldeinformationen in die Verbindungszeichenfolge ungültig sind. Klicken Sie dann die Open-Methode löst jedoch eine `SqlException`. Ausnahmen können behandelt werden, durch die Verwendung von `try/catch/finally` Blöcke. Wenn code in einem `try` Blocks eine Ausnahme auslöst, wird die Steuerung an die entsprechenden Catch-Block, in denen der Entwickler versuchen, die Verarbeitung fortzusetzen. Wenn keine übereinstimmenden Catch-Block vorhanden ist oder wenn der Code, der die Ausnahme ausgelöst hat, nicht in einem Try-Block ist, wird die Ausnahme der Aufrufliste im search von percolates `try/catch/finally` Blöcke.

Wenn die Ausnahme zu der ASP.NET-Laufzeit ohne Behandlung wird übergeben, die [ `HttpApplication` Klasse](https://msdn.microsoft.com/library/system.web.httpapplication.aspx)des [ `Error` Ereignis](https://msdn.microsoft.com/library/system.web.httpapplication.error.aspx) wird ausgelöst, und dem konfigurierten *Fehler (Seite)*  wird angezeigt. Standardmäßig ASP.NET eine Fehlerseite angezeigt, die sich als bezeichnet wird, zeigt der [gelben Bildschirm of Death](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow) (YSOD). Es gibt zwei Versionen der YSOD: eine zeigt die Details der Ausnahme, die eine stapelüberwachung und andere Informationen für Entwickler Debuggen der Anwendung hilfreich (finden Sie unter **Abbildung1**); die andere gibt einfach an, dass ein Laufzeitfehler (siehe aufgetreten **Abbildung 2**).

Die Ausnahmedetails YSOD ist sehr hilfreich für Entwickler, die die Anwendung debuggen, aber mit einem YSOD für Endbenutzer ist klebrig und unprofessionell. Stattdessen sollten Benutzer zu einer Fehlerseite um ausgeführt werden, die der Website Aussehen und Verhalten mit Benutzerfreundlicher Prose beschreiben die Lage verwaltet. Die gute Nachricht ist, dass das Erstellen solcher einer benutzerdefinierten Fehlerseite recht einfach. Dieses Tutorial beginnt mit einem Blick auf ASP. NET Seiten, die anderen Fehler. Es zeigt dann, wie so konfigurieren Sie die Webanwendung, die Benutzern bei einem Fehler eine benutzerdefinierten Fehlerseite angezeigt.

### <a name="examining-the-three-types-of-error-pages"></a>Untersuchen die drei Typen von Fehlerseiten

Wenn eine nicht behandelte Ausnahme in einer ASP.NET-Anwendung aus einem von drei Typen der Fehlerseiten, die tritt auf, wird angezeigt:

- Die Ausnahme Details gelben Bildschirm of Death-Fehlerseite
- Die Common Language Runtime Fehler gelben Bildschirm of Death-Fehlerseite, oder
- Einer benutzerdefinierten Fehlerseite

Der Seitenentwickler Fehler sind ist am besten vertraut mit der Ausnahme Details YSOD. Standardmäßig wird diese Seite wird angezeigt, für Benutzer, die lokal besuchen und aus diesem Grund ist die Seite, die Sie sehen, wenn ein Fehler auftritt, wenn die Website in der Entwicklungsumgebung testen. Wie der Name schon sagt, stellt die Ausnahme Details YSOD Details zur Ausnahme – der Typ der Nachricht und die stapelüberwachung. Wird von Code in die ASP.NET-Seite Code-Behind-Klasse die Ausnahme ausgelöst wurde und für das Debuggen die Anwendung konfiguriert ist Klicken Sie dann die Ausnahme Details YSOD auch diese Codezeile (und einige Zeilen des oben angegeben Codes und darunter) angezeigt.

**Abbildung 1** zeigt die Seite die Details YSOD Ausnahme. Beachten Sie die URL in die Adresse des Browserfensters: `http://localhost:62275/Genre.aspx?ID=foo`. Bedenken Sie, dass die `Genre.aspx` Seite listet die Buchbesprechungen in einem bestimmten Genre. Es erfordert, `GenreId` Wert (eine `uniqueidentifier`) übergeben werden, über die Abfragezeichenfolge, die entsprechende URL ein, um die Überprüfungen Fiktion anzuzeigen ist z. B. `Genre.aspx?ID=7683ab5d-4589-4f03-a139-1c26044d0146`. Wenn es sich bei einer nicht -`uniqueidentifier` -Wert übergeben wird im über die Abfragezeichenfolge (z. B. "Foo") wird eine Ausnahme ausgelöst.

> [!NOTE]
> Zum Reproduzieren dieser Fehler in der Demo-Webanwendung zum Download zur Verfügung. Sie können entweder `Genre.aspx?ID=foo` direkt, oder klicken Sie auf den Link "Generieren einer Runtime-Fehler" in `Default.aspx`.


Beachten Sie die Ausnahmeinformationen im **Abbildung1**. Die Ausnahmemeldung, "Fehler beim Konvertieren einer Zeichenfolge in ' uniqueidentifier '" ist am oberen Rand der Seite vorhanden. Der Typ der Ausnahme `System.Data.SqlClient.SqlException`, wird ebenfalls aufgeführt. Es gibt auch die stapelüberwachung.

[![](displaying-a-custom-error-page-cs/_static/image2.png)](displaying-a-custom-error-page-cs/_static/image1.png)

**Abbildung 1**: die Ausnahmedetails YSOD enthält Informationen zur Ausnahme  
 ([Klicken Sie, um das Bild in voller Größe anzeigen](displaying-a-custom-error-page-cs/_static/image3.png))

Die andere Art von YSOD YSOD der Common Language Runtime-Fehler ist, und sehen Sie in **abbildung2**. Die Common Language Runtime-Fehler YSOD informiert den Besucher, den ein Laufzeitfehler aufgetreten ist, aber es werden keine Informationen über die Ausnahme, die ausgelöst wurde. (Es der Fall ist, allerdings bieten Anweisungen an, um den Fehlerdetails angezeigt werden kann durch Ändern der `Web.config` -Datei, die macht diese eine YSOD unprofessionell gehört.)

Standardmäßig wird die Common Language Runtime-Fehler YSOD für Benutzer, die Zugriff auf Remote angezeigt (über http://www.yoursite.com), wie durch die URL in die Adressleiste des Browsers belegt **Abbildung 2**: `http://httpruntime.web703.discountasp.net/Genre.aspx?ID=foo`. Die beiden anderen YSOD Bildschirme vorhanden sein, da Entwickler möchte wissen, die Fehlerdetails sind, aber solche Informationen nicht auf einer live-Website angezeigt werden, sollten wie sie anzeigen kann, potenzielle Sicherheitsrisiken oder andere sensible Daten für alle Benutzer besucht Ihre Website.

> [!NOTE]
> Wenn Sie befolgt werden und DiscountASP.NET wie auf dem Webhost verwenden, werden Sie feststellen, dass die Common Language Runtime-Fehler YSOD nicht angezeigt wird, wenn sich die live-Website besuchen. Dies ist da DiscountASP.NET ihre Server so konfiguriert, dass die Ausnahme Details YSOD anzeigen standardmäßig verfügt. Die gute Nachricht ist, dass Sie dieses Standardverhalten, durch das Hinzufügen überschreiben können einer `<customErrors>` -Abschnitt Ihrer `Web.config` Datei. Im Abschnitt "Konfigurieren der Fehler angezeigt" untersucht die `<customErrors>` Abschnitt ausführlich.


[![](displaying-a-custom-error-page-cs/_static/image5.png)](displaying-a-custom-error-page-cs/_static/image4.png)

**Abbildung 2**: Laufzeitfehler YSOD enthält kein Fehlerdetails  
 ([Klicken Sie, um das Bild in voller Größe anzeigen](displaying-a-custom-error-page-cs/_static/image6.png))

Die dritte Art von Fehler (Seite) wird die benutzerdefinierte Fehlerseite, wird eine Webseite, die Sie erstellen. Der Vorteil einer benutzerdefinierten Fehlerseite ist, dass Sie vollständige Kontrolle über die Informationen, die dem Benutzer zusammen mit Erscheinungsbild der Seite angezeigt wird; die benutzerdefinierte Fehlerseite kann die dieselbe Masterseite und Stile als alle anderen Seiten verwenden. Im Abschnitt "Verwenden einer benutzerdefinierten Fehlerseite" führt durch Erstellen einer benutzerdefinierten Fehlerseite und konfigurieren es im Falle einer nicht behandelten Ausnahme angezeigt. **Abbildung 3** bietet einen ersten Eindruck von dieser benutzerdefinierten Fehlerseite. Wie Sie sehen können, ist das Aussehen und Verhalten der Fehlerseite viel mehr professionell aussehende als eines der gelben Bildschirme of Death in den Abbildungen 1 und 2 dargestellt.

[![](displaying-a-custom-error-page-cs/_static/image8.png)](displaying-a-custom-error-page-cs/_static/image7.png)

**Abbildung 3**: eine benutzerdefinierten Fehlerseite bietet eine besser angepasste Aussehen und Verhalten  
 ([Klicken Sie, um das Bild in voller Größe anzeigen](displaying-a-custom-error-page-cs/_static/image9.png))

Überprüfen Sie in der Adressleiste des Browsers in Ruhe **Abbildung 3**. Beachten Sie, dass die Adressleiste die URL der benutzerdefinierten Fehlerseite zeigt (`/ErrorPages/Oops.aspx`). In den Abbildungen 1 und 2 werden gelb Bildschirme of Death angezeigt, in der gleichen Seite, die der Fehler entstanden (`Genre.aspx`). Die benutzerdefinierte Fehlerseite übergeben wird die URL der Seite, in dem der Fehler aufgetreten, über ist, die `aspxerrorpath` Querystring-Parameter.

## <a name="configuring-which-error-page-is-displayed"></a>Konfigurieren die Fehlerseite wird angezeigt.

Welche der drei möglichen Fehlerseiten angezeigt wird, basiert auf zwei Variablen:

- Die Konfigurationsinformationen in der `<customErrors>` Abschnitt und
- Gibt an, ob der Benutzer die Site lokal oder Remote besucht.

Die [ `<customErrors>` Abschnitt](https://msdn.microsoft.com/library/h0hfz6fc.aspx) in `Web.config` verfügt über zwei Attribute, die beeinflussen, welche Fehler (Seite) angezeigt wird: `defaultRedirect` und `mode`. Das `defaultRedirect`-Attribut ist optional. Wenn angegeben, gibt die URL des benutzerdefinierten Fehlerseite und gibt an, dass die benutzerdefinierte Fehlerseite anstelle der Common Language Runtime-Fehler YSOD angezeigt werden soll. Die `mode` Attribut ist erforderlich, und akzeptiert einen von drei Werten: `On`, `Off`, oder `RemoteOnly`. Diese Werte weisen folgendes Verhalten auf:

- `On` -Gibt an, dass die benutzerdefinierte Fehlerseite oder die Common Language Runtime-Fehler YSOD, für alle Besucher angezeigt wird, unabhängig davon, ob sie lokal oder remote sind.
- `Off` -Gibt an, dass die Ausnahme Details YSOD, für alle Besucher angezeigt wird, unabhängig davon, ob sie lokal oder remote sind.
- `RemoteOnly` -Gibt an, dass die benutzerdefinierte Fehlerseite oder die Common Language Runtime-Fehler YSOD für remote Besucher, angezeigt wird, während die Ausnahme Details YSOD lokalen Besucher angezeigt wird.

Wenn Sie nichts anderes angeben, den ASP.NET verhält, als ob Sie das Mode-Attribut, um festlegen mussten `RemoteOnly` und nicht angegeben hatten eine `defaultRedirect` Wert. Das heißt, ist das Standardverhalten an, dass die Ausnahme Details YSOD für lokale Besucher angezeigt wird, während der Laufzeit Fehler YSOD remote Besucher angezeigt wird. Sie können dieses Standardverhalten außer Kraft setzen, durch das Hinzufügen einer `<customErrors>` Abschnitt aus, um Ihrer Webanwendung `Web.config file.`

## <a name="using-a-custom-error-page"></a>Verwenden einer benutzerdefinierten Fehlerseite

Jede Webanwendung müssen sich auf eine benutzerdefinierten Fehlerseite. Es bietet eine Alternative mehr professionell aussehende, zu der Common Language Runtime-Fehler YSOD, es ist einfach zu erstellen und Konfigurieren der Anwendung verwendet die benutzerdefinierte Fehlerseite dauert nur wenige Minuten. Der erste Schritt ist die benutzerdefinierte Fehlerseite erstellen. Ich habe die mit dem Namen Book Reviews-Anwendung einen neuen Ordner hinzugefügt `ErrorPages` und hinzugefügt, eine neue ASP.NET-Seite mit dem Namen `Oops.aspx`. Haben Sie die Seite, die dieselbe Masterseite als die anderen Seiten auf Ihrer Website zu verwenden, sodass es automatisch die gleiche Aussehen und Verhalten erbt.

[![](displaying-a-custom-error-page-cs/_static/image11.png)](displaying-a-custom-error-page-cs/_static/image10.png)

**Abbildung 4**: erstellen eine benutzerdefinierten Fehlerseite

Als Nächstes dauert ein paar Minuten, die den Inhalt für die Fehlerseite erstellen. Ich habe eine ziemlich einfach, benutzerdefinierte Fehlerseite erstellt, mit einer Meldung, dass ein unerwarteter Fehler aufgetreten, und ein Link zu der Website-Homepage zurück.

[![](displaying-a-custom-error-page-cs/_static/image13.png)](displaying-a-custom-error-page-cs/_static/image12.png)

**Abbildung 5**: Entwerfen der Fehlerseite  
 ([Klicken Sie, um das Bild in voller Größe anzeigen](displaying-a-custom-error-page-cs/_static/image14.png))

Konfigurieren Sie mit der Fehlerseite abgeschlossen ist die Webanwendung verwendet die benutzerdefinierte Fehlerseite, durch die Common Language Runtime-Fehler YSOD. Dies erfolgt durch Angabe der URL der Fehlerseite in das `<customErrors>` des Abschnitts `defaultRedirect` Attribut. Fügen Sie das folgende Markup zu Ihrer Anwendungsverzeichnis `Web.config` Datei:

[!code-xml[Main](displaying-a-custom-error-page-cs/samples/sample1.xml)]

Das obenstehende Markup konfiguriert, die Anwendung die Ausnahme Details YSOD Benutzern Zugriff auf lokal, bei der Verwendung von für diese Benutzer remote auf der benutzerdefinierte Fehlerseite Oops.aspx angezeigt wird. Um dies in Aktion zu sehen, stellen Sie Ihre Website in der produktionsumgebung bereit, und besuchen Sie dann die Seite "Genre.aspx" auf der Website mit einem ungültigen Querystring-Wert. Daraufhin sollte die benutzerdefinierte Fehlerseite (siehe **Abbildung 3**).

Um sicherzustellen, dass die benutzerdefinierte Fehlerseite nur remote-Benutzern angezeigt wird, finden Sie auf die `Genre.aspx` Seite mit einer ungültigen Abfragezeichenfolge aus der Entwicklungsumgebung. Die Ausnahme Details YSOD sollte immer noch angezeigt werden (siehe **Abbildung 1**). Die `RemoteOnly` Einstellung wird sichergestellt, dass Besucher der Site in der produktionsumgebung die benutzerdefinierte Fehlerseite angezeigt, während Entwickler, die lokal arbeiten weiterhin, um die Details der Ausnahme anzuzeigen.

## <a name="notifying-developers-and-logging-error-details"></a>Benachrichtigen von Entwicklern und Protokollieren von Fehlerdetails

Fehler, die in der Entwicklungsumgebung auftreten, die durch den Entwickler, die auf ihrem Computer verursacht wurden. Er die Ausnahme, die Informationen in der Ausnahme Details YSOD angezeigt wird, und er weiß, welche Schritte sie beim Auftreten des Fehlers ausgeführt wurde. Wenn ein Fehler in der Produktion befindet, der Entwickler hat aber keine Kenntnis, dass ein Fehler aufgetreten ist, es sei denn, der Website für den Endbenutzer die Zeit, die den Fehler zu melden. Und selbst wenn der Benutzer seine Möglichkeit, die das Entwicklungsteam zu warnen, das ohne Kenntnis der Ausnahmetyp, die Nachricht und die stapelüberwachung, die es um die Ursache des Fehlers, geschweige denn Behebung es schwierig sein kann, ist ein Fehler aufgetreten, verlässt.

Die Entwickler werden für diesen Fehler benachrichtigt, aus diesen Gründen ist es entscheidend, dass ein Fehler in der produktionsumgebung zu einem dauerhaft bestehenden Speicherort (z. B. eine Datenbank), die protokolliert wird. Die benutzerdefinierte Fehlerseite mag sich wie ein guter Ausgangspunkt, um diese Protokollierung und Benachrichtigungen. Leider wird die benutzerdefinierte Fehlerseite hat keinen Zugriff auf die Fehlerdetails, und daher nicht verwendet werden, um diese Informationen zu protokollieren. Die gute Nachricht ist, dass es eine Reihe von Möglichkeiten, um die Fehlerdetails abfangen und den sie protokollieren, die nächsten drei Tutorials werden in diesem Thema noch ausführlicher.

## <a name="using-different-custom-error-pages-for-different-http-error-statuses"></a>Verwenden andere benutzerdefinierte Fehlerseiten für verschiedene HTTP-Fehler-Status

Wenn eine Ausnahme, durch eine ASP.NET-Seite ausgelöst wird und wird nicht verarbeitet, percolates die Ausnahme, bis die ASP.NET-Laufzeit, die den konfigurierten wird folgende Fehlermeldung angezeigt. Wenn eine Anforderung an die Engine für ASP.NET ist jedoch nicht verarbeitet werden, aus irgendeinem Grund – z. B. die angeforderte Datei nicht gefunden oder gelesen wurden Berechtigungen für die Datei - deaktiviert die Engine für ASP.NET und löst anschließend ein `HttpException`. Diese Ausnahme aus, wie von ASP.NET-Seiten aus aufzurufen, ausgelöste Ausnahmen ausgelöst, die Laufzeit, wodurch der entsprechende Fehler angezeigt werden soll.

Dies bedeutet für die Webanwendung in der Produktion ist, sofern ein Benutzer eine Seite, die nicht gefunden wird anfordert, und klicken Sie dann die benutzerdefinierte Fehlerseite angezeigt. **Abbildung 6** zeigt ein Beispiel dafür. Da die Anforderung für eine nicht existierende-Seite ist (`NoSuchPage.aspx`), wird eine `HttpException` wird ausgelöst, und die benutzerdefinierte Fehlerseite wird angezeigt (Beachten Sie den Verweis auf `NoSuchPage.aspx` in die `aspxerrorpath` Querystring-Parameter).

[![](displaying-a-custom-error-page-cs/_static/image16.png)](displaying-a-custom-error-page-cs/_static/image15.png)

**Abbildung 6**: die ASP.NET-Laufzeit Anzeigen der konfigurierten Fehler Seite In Antwort auf eine ungültige Anforderung ([klicken Sie, um das Bild in voller Größe anzeigen](displaying-a-custom-error-page-cs/_static/image17.png))

Standardmäßig führen alle Arten von Fehlern die gleiche benutzerdefinierte Fehlerseite angezeigt werden. Allerdings können Sie angeben, für einen bestimmten HTTP-Status Code mit eine anderen benutzerdefinierten Fehlerseite `<error>` untergeordnete Elemente innerhalb der `<customErrors>` Abschnitt. Um eine andere Fehlerseite angezeigt, die im Falle einer Seite nicht gefunden, die über HTTP-Statuscode 404 verfügt, z. B. Aktualisieren der `<customErrors>` Abschnitt aus, um das folgende Markup enthalten:

[!code-xml[Main](displaying-a-custom-error-page-cs/samples/sample2.xml)]

Mit dieser Änderung vorgenommen wurde, wenn ein Benutzer Zugriff auf Remote eine ASP.NET-Ressource, die nicht vorhanden ist anfordert, wird er umgeleitet die `404.aspx` benutzerdefinierte Fehlerseite anstelle von `Oops.aspx`. Als **abbildung7** veranschaulicht, die `404.aspx` Seite kann eine spezifische Nachricht als die allgemeine benutzerdefinierte Fehlerseite einschließen.

> [!NOTE]
> Sehen Sie sich [404-Fehlerseiten, mehr einmal](http://www.smashingmagazine.com/2009/01/29/404-error-pages-one-more-time/) Anleitungen zum Erstellen von Seiten für effektive 404-Fehler.


[![](displaying-a-custom-error-page-cs/_static/image19.png)](displaying-a-custom-error-page-cs/_static/image18.png)**Abbildung 7**: die Seite benutzerdefinierte 404-Fehler in einer Meldung besser abgestimmten als `Oops.aspx`  
 ([Klicken Sie, um das Bild in voller Größe anzeigen](displaying-a-custom-error-page-cs/_static/image20.png)) 

Da Sie wissen, dass die `404.aspx` Seite wird nur erreicht werden, wenn der Benutzer für eine Seite anfordert, die nicht gefunden wurde, können Sie erweitern dieses benutzerdefinierte Fehlerseite Funktionen können Sie den Benutzer, die diese bestimmte Art von Fehler zu beheben. Z. B. Sie eine Datenbanktabelle, der zugeordnet gute URLs bekanntermaßen fehlerhafte URLs erstellen und dann konnte der `404.aspx` benutzerdefinierte Fehlerseite führen Sie eine Abfrage, die Tabelle und Seiten, die der Benutzer zu erreichen versuchen möglicherweise vorschlagen.

> [!NOTE]
> Die benutzerdefinierte Fehlerseite wird nur angezeigt, wenn eine auf eine Ressource, die von der Engine für ASP.NET behandelt Anforderung. Wie wir unter den [Core Unterschiede zwischen IIS und ASP.NET Development Server](core-differences-between-iis-and-the-asp-net-development-server-cs.md) Tutorial, auf der Webserver unter Umständen bestimmte Anforderungen selbst behandeln. Standardmäßig web der IIS-Prozesse serveranforderungen für statische Inhalte wie Bilder und HTML-Dateien ohne das ASP.NET-Modul aufzurufen. Daher, wenn der Benutzer eine nicht vorhandene Abbilddatei anfordert sie ASP, anstatt IISs-Standardnachricht 404-Fehler erhalten zurück. NET konfiguriert Fehler (Seite).


## <a name="summary"></a>Zusammenfassung

Wenn eine nicht behandelte Ausnahme in einer ASP.NET-Anwendung auftritt, wird dem Benutzer angezeigt eine der drei Fehlerseiten: die Ausnahme Details gelben Bildschirm of Death; Laufzeitfehler Gelb Bildschirm fehl; oder einer benutzerdefinierten Fehlerseite. Die Seite "Fehler" angezeigt wird, hängt davon ab, der Anwendung `<customErrors>` Konfigurations- und gibt an, ob der Benutzer lokal oder Remote besucht. Standardmäßig wird die Ausnahme Details YSOD für Besucher von lokalen und der Common Language Runtime-Fehler YSOD remote Besuchern erläutert.

Während der Common Language Runtime-Fehler YSOD potenziell vertrauliche Informationen von der Website für den Benutzer ausblendet, unterbrochen, die von Ihrer Website Aussehen und Verhalten und macht Buggy Erscheinungsbild Ihrer Anwendung. Ein besserer Ansatz ist die Verwendung eine benutzerdefinierten Fehlerseite, dazu gehört die erstellen und Entwerfen der Fehlerseite angegeben wird und die URL in die `<customErrors>` des Abschnitts `defaultRedirect` Attribut. Sie können auch mehrere benutzerdefinierte Fehlerseiten für verschiedene HTTP-Fehler Statuswerte haben.

Die benutzerdefinierte Fehlerseite ist der erste Schritt bei einer umfassenden Strategie für eine Website in der Produktion zur Fehlerbehandlung. Warnungen vom Entwickler des Fehlers und protokollieren die Details sind auch wichtige Schritte. Die nächsten drei Tutorials werden die Verfahren für die fehlerbenachrichtigung und Protokollierung.

Viel Spaß beim Programmieren!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Tutorial erläutert finden Sie in den folgenden Ressourcen:

- [Fehlerseiten, noch einmal](http://www.smashingmagazine.com/2009/01/29/404-error-pages-one-more-time/)
- [Entwurfsrichtlinien für Ausnahmen](https://msdn.microsoft.com/library/ms229014.aspx)
- [Benutzerfreundliche Fehlerseiten](http://aspnet.4guysfromrolla.com/articles/090606-1.aspx)
- [Behandeln und Auslösen von Ausnahmen](https://msdn.microsoft.com/library/5b2yeyab.aspx)
- [Verwenden von benutzerdefinierten Fehlerseiten ordnungsgemäß in ASP.NET](http://professionalaspnet.com/archive/2007/09/30/Properly-Using-Custom-Error-Pages-in-ASP.NET.aspx)

> [!div class="step-by-step"]
> [Zurück](strategies-for-database-development-and-deployment-cs.md)
> [Weiter](processing-unhandled-exceptions-cs.md)
