---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/precompiling-your-website-vb
title: Vorkompilieren Ihrer Website (VB) | Microsoft-Dokumentation
author: rick-anderson
description: 'Visual Studio bietet ASP.NET-Entwicklern zwei Projekttypen: Web Application Projects (WAPs) und Websiteprojekten (WSPs). Eines der wichtigsten Unterschiede Betwe...'
ms.author: aspnetcontent
ms.date: 06/09/2009
ms.assetid: c285dc6f-a1c6-46e6-ac03-3830947f57e3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/precompiling-your-website-vb
msc.type: authoredcontent
ms.openlocfilehash: a5d7820bf99348ec9d1014264de5779bb7aead01
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37809562"
---
<a name="precompiling-your-website-vb"></a>Vorkompilieren Ihrer Website (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_15_VB.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial15_Precompiling_vb.pdf)

> Visual Studio bietet ASP.NET-Entwicklern zwei Projekttypen: Web Application Projects (WAPs) und Websiteprojekten (WSPs). Einer der wichtigsten Unterschiede zwischen den zwei Projekttypen ist, dass WAPs müssen den Code explizit vor der Bereitstellung kompiliert, während der Code in eine WSP-Datei automatisch auf dem Webserver kompiliert werden kann. Allerdings ist es möglich, eine WSP-Datei vor der Bereitstellung vorkompilieren. Dieses Tutorial erläutert die Vorteile der Vorkompilierung und zeigt, wie zum Vorkompilieren einer Website aus Visual Studio und über die Befehlszeile.


## <a name="introduction"></a>Einführung

Visual Studio bietet ASP.NET-Entwicklern zwei Projekttypen: Web Application Projects (WAP) und Website-Projekten (WSP). Einer der wichtigsten Unterschiede zwischen dieser Projekttypen ist, dass WAPs erfordern *explizite Kompilierung* während WSPs verwenden *automatische Kompilierung*, in der Standardeinstellung. Mit drahtlosen Zugriffspunkten, Sie Kompilieren der Webanwendung in eine einzelne Assembly, die in der Website erstellt wurde `Bin` Ordner. Bereitstellung umfasst das Kopieren den Inhalt des Markups (die `.aspx.ascx`, und `.master` Dateien) im Projekt zusammen mit der Assembly in der `Bin` Ordner, in der CodeBehind-Klassendateien selbst müssen nicht bereitgestellt werden. Stellen Sie auf der anderen Seite WSPs, indem Sie sowohl die Markupseiten und ihre entsprechenden CodeBehind-Klassen in der produktionsumgebung kopieren. Die CodeBehind-Klassen werden bei Bedarf auf dem Webserver kompiliert.

> [!NOTE]
> Finden Sie im Abschnitt "Explizite Kompilierung und automatische Kompilierung" in der [ *bestimmen was Dateien müssen bereitgestellt werden* Tutorial](determining-what-files-need-to-be-deployed-vb.md) Weitere Hintergrundinformationen zu den Unterschieden zwischen dem Projekt Modelle, explizit und automatisch Kompilierung und wie das Kompilierungsmodell Bereitstellung auswirkt.


Die Option für die automatische Kompilierung ist einfach zu verwenden. Es ist kein expliziter Kompilierungsschritt und nur die Dateien, die wurden geändert, müssen bereitgestellt werden, während die explizite Kompilierung erfordert die geänderten Markupseiten und die gerade kompilierte Assembly bereitstellen. Automatische Bereitstellung hat jedoch zwei mögliche Nachteile:

- Da die Seiten automatisch kompiliert werden müssen, wenn sie zuerst aufgerufen werden, kann es sein eine kurze, aber wichtigen Verzögerung, wenn zum ersten Mal nach der Bereitstellung eine ASP.NET-Seite angefordert wird.
- Automatische Kompilierung erfordert, dass es sich bei der deklarative Markup und Quellcode-Code auf dem Webserver vorhanden sein. Dies kann unattraktiv sein, wenn Sie planen, verkaufen den Web-Anwendung für Kunden, die auf ihren Webservern installiert werden.

Wenn entweder die beiden oben Nachteile sind viel wörtertrennungen, können Sie wechseln mit dem WAP-Modell oder *Vorkompilieren* WSP-Datei vor der Bereitstellung. In diesem Tutorial werden die am besten geeigneten für eine gehostete Website Vorkompilierungsoptionen und führt Sie durch die Vorkompilierung Prozess und die Bereitstellung einer vorkompilierten Website.

## <a name="an-overview-of-aspnet-code-generation-and-compilation"></a>Eine Übersicht über ASP.NET-Codegenerierung und-Kompilierung

Bevor Sie sich die verfügbaren Optionen für die Vorkompilierung befassen, zunächst sprechen wir über die codegenerierung und Kompilierung, das auftritt, wenn eine ASP.NET-Seite zum ersten Mal angefordert wird, da sie erstellt oder zuletzt aktualisiert wurde. Wie Sie wissen, ASP.NET-Seiten bestehen aus zwei Teilen: deklaratives Markup in der `.aspx` -Datei und ein Teil der Source-Code, in der Regel in einer separaten CodeBehind-Klassendatei (`.aspx.vb`). Die Schritte, die von der Runtime ausgeführt wird, wenn eine ASP.NET-Seite angefordert wird, hängt von der Anwendung Kompilierungsmodell ab.

Mit drahtlosen Zugriffspunkten muss die Seiten-Quellcode explizit in eine einzelne Assembly bereitgestellt wird, bevor Sie kompiliert werden. Während der Bereitstellung werden diese Assembly und den verschiedenen Markupseiten in der produktionsumgebung kopiert. Wenn eine Anforderung an den Webserver für eine ASP.NET-Seite eingeht, wird die Common Language Runtime erstellt eine Instanz der Seite Code-Behind-Klasse und ruft seine `ProcessRequest` Methode, die beginnt des Seitenlebenszyklus und letztendlich den Inhalt der Seite, die an zurückgegeben wird der anforderer. Die Laufzeit kann der ASP.NET-Seite Code-Behind-Klasse verwenden, da der Code-Behind-Klasse in einer Assembly vor der Bereitstellung bereits kompiliert wurde.

WSPs und die automatische Kompilierung gibt es keine explizite Kompilierungsschritt vor der Bereitstellung. Dagegen umfasst die Bereitstellung sowohl deklarativen als auch der Quellinhalt für Code in der produktionsumgebung kopieren. Wenn eine Anforderung an den Webserver für eine ASP.NET-Seite zum ersten Mal eingeht seit die Seite erstellt oder zuletzt aktualisiert wurde, muss die Laufzeit zuerst die Code-Behind-Klasse in eine Assembly kompilieren. Diese kompilierten Assembly im Ordner gespeichert ist `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files`, obwohl Sie der Speicherort dieses Ordners angepasst werden kann, über die `<pages>` Element im `Web.config`. Da die Assembly ist auf dem Datenträger gespeichert, es muss nicht bei nachfolgenden Anforderungen zur selben Seite neu kompiliert werden.

> [!NOTE]
> Wie zu erwarten ist, ist eine geringfügige Verzögerung beim Anfordern einer Seite zum ersten Mal (oder zum ersten Mal seit sie geändert wurde) an einem Standort, die automatische Kompilierung verwendet wird, wie es dauert einen Moment, während der Server zum Kompilieren von Code der Seite, und speichern die resultierende assembly der Datenträger.


Kurz gesagt, mit der expliziten Kompilierung müssen Sie zum Kompilieren von Quellcode für die Website vor der Bereitstellung, speichern die Runtime erspart, um diesen Schritt ausführen. Bei der automatischen Kompilierung behandelt die Runtime die Kompilierung von Quellcode für die Seiten, jedoch mit geringfügigen Initialisierung Kosten für den ersten Besuch auf der Seite, da erstellt oder zuletzt aktualisiert wurde.

Aber was ist das deklarative Teil von ASP.NET-Seiten (die `.aspx` Datei)? Es ist offensichtlich, dass es eine Beziehung zwischen ist der `.aspx` Dateien und der Code in ihre CodeBehind-Klassen, wie die Web-Steuerelemente, die im deklarativen Markup definiert sind in den Code zugegriffen werden kann. Es ist auch offensichtlich, die den Inhalt in die `.aspx` Dateien wirkt sich erheblich auf dem gerenderten Markup, das durch die Seite generiert. Wie funktioniert die Runtime mit dem Text, HTML und Syntax von Webserver-Steuerelement definiert, der `.aspx` Datei zum Generieren der angeforderten Seite gerenderten Inhalts?

Ich möchte nicht auch in den Implementierungsdetails auf niedriger Ebene, auslassen und lenke die zwischen WAPs und WSPs variieren, aber im Prinzip generiert die Laufzeit automatisch eine Klassendatei, die die verschiedenen Steuerelemente des Web-als geschützte Member und Methoden enthält. Diese Datei wird als implementiert eine *Teilklasse* auf die entsprechende CodeBehind-Klasse. ([Teilklassen](http://www.dotnet-guide.com/partialclasses.html) für den Inhalt einer einzelnen Klasse auf mehrere Dateien verteilt werden können.) Aus diesem Grund wird der Code-Behind-Klasse definiert, an zwei Orten: in der `.aspx.vb` -Datei, die Sie erstellen und in dieser Klasse automatisch generiert, die von der Runtime erstellt. Diese automatisch generierte Klasse befindet sich in der `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files` Ordner.

Die wichtigen einsenden ist hier, dass für eine ASP.NET-Seite, die von der Laufzeit gerendert werden sowohl die deklarative und Teile der Source-Code müssen in eine Assembly kompiliert werden. Mit drahtlosen Zugriffspunkten der Quellcode explizit in eine Assembly vor der Bereitstellung kompiliert wird, aber deklarative Markup muss weiterhin Code konvertiert und kompiliert die Runtime auf dem Webserver. Mit WSPs automatische Kompilierung verwenden müssen sowohl den Quellcode und deklarativen Markup vom Webserver kompiliert werden.

Es ist möglich, explizite Kompilierung mit der WSP-Modell verwenden. Sie können explizit den Codeteil der Quelle wie mit dem Modell WAP kompilieren. Können Sie auch deklarative Markup kompilieren.

## <a name="precompilation-options"></a>Vorkompilierungsoptionen

Im Lieferumfang von .NET Framework ein [ASP.NET-Kompilierungstool (`aspnet_compiler.exe`)](https://msdn.microsoft.com/library/ms229863.aspx) , mit der Sie zum Kompilieren von Quellcode (und auch den Inhalt), einer ASP.NET-Anwendung mithilfe des WSP-Modells erstellt. Dieses Tool wurde mit .NET Framework, Version 2.0 veröffentlicht und befindet sich in der `%WINDIR%\Microsoft.NET\Framework\v2.0.50727` Ordner; es über die Befehlszeile verwendet oder über im buildmenü-Option "Website veröffentlichen" in Visual Studio aus gestartet werden kann.

Das Kompilierungstool verfügt über zwei allgemeine Arten der Kompilierung: direkte Vorkompilierung und Vorkompilierung zur Bereitstellung. Führen Sie für die direkte Vorkompilierung der `aspnet_compiler.exe` tool über die Befehlszeile, und geben Sie den Pfad für das virtuelle Verzeichnis oder den physischen Pfad einer Website, die sich befindet, auf dem Computer. Das Kompilierungstool kompiliert anschließend jede ASP.NET-Seite in das Projekt, und speichern die kompilierte Version in der `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files` Ordner haben wie wenn Seiten jeweils zum ersten Mal in einem Browser besucht wurden. Direkte Vorkompilierung kann beschleunigen der ersten Anforderung an die neu bereitgestellte ASP.NET-Seiten auf Ihrer Website vorgenommen werden, da es verringert, dass die Laufzeit diesen Schritt ausführen müssen. Direkte Vorkompilierung ist jedoch nicht nützlich für die Mehrzahl der gehosteten Websites, da sie erfordert, dass Sie Programme in der Webservers Befehlszeile ausführen können. In gemeinsamen hosting-Umgebungen ist diese Zugriffsebene nicht zulässig.

> [!NOTE]
> Finden Sie weitere Informationen über die direkte Vorkompilierung [so wird's gemacht: Vorkompilieren von ASP.NET-Websites](https://msdn.microsoft.com/library/ms227972.aspx) und [Vorkompilierung in ASP.NET 2.0](http://www.odetocode.com/Articles/417.aspx).


Anstatt die Seiten der Website zum Kompilieren der `Temporary ASP.NET Files` Ordner durch die Vorkompilierung zur Bereitstellung kompiliert die Seiten in einem Verzeichnis Ihrer Wahl und in ein Format, das in der produktionsumgebung bereitgestellt werden kann.

Es gibt zwei Arten der Vorkompilierung zur Bereitstellung, die wir in diesem Tutorial untersuchen: mit einer Benutzeroberfläche für aktualisierbare Vorkompilierung und Vorkompilierung mit einer nicht aktualisierbaren-Benutzeroberfläche. Durch die Vorkompilierung einer aktualisierbaren Benutzeroberfläche verlässt deklarative Markup in der `.aspx`, `.ascx`, und `.master` -Dateien, wodurch Entwickler anzuzeigen und, falls gewünscht, ändern Sie das deklarative Markup auf dem Produktionsserver. Durch die Vorkompilierung mit einer nicht aktualisierbaren Benutzeroberfläche generiert `.aspx` Seiten, Inhalte und entfernt "void" sind `.ascx` und `.master` Dateien, wodurch Ausblenden von deklarativen Markup und Änderung von verbietet Entwickler die die produktionsumgebung.

### <a name="precompiling-for-deployment-with-an-updatable-user-interface"></a>Für die Bereitstellung mit einer Benutzeroberfläche für aktualisierbare Vorkompilierung

Die beste Möglichkeit, durch die Vorkompilierung zur Bereitstellung zu verstehen ist, finden Sie ein Beispiel in Aktion. Lassen Sie uns Vorkompilieren der Book Reviews WSP-Datei für die Bereitstellung über eine aktualisierbare Benutzeroberfläche. Die ASP.NET-Kompilierungstool kann von Visual Studio Menü "Build" oder über die Befehlszeile aufgerufen werden. In diesem Abschnitt wird untersucht, mit dem Tool von Visual Studio. im Abschnitt "Vorkompilieren über die Befehlszeile" untersucht das Compilertool von der Befehlszeile aus ausführen.

Öffnen Sie Buch Review WSP-Datei in Visual Studio, öffnen Sie das Build-Menü, und wählen Sie die Menüoption "-" Website veröffentlichen. Dadurch wird das Dialogfeld "Website veröffentlichen" (finden Sie unter **Abbildung1**), in dem Sie am Zielspeicherort an, ob die vorkompilierte Site Benutzeroberfläche aktualisiert werden kann und andere Compiler-Tool-Optionen angeben können. Am Zielstandort kann ein remote-Web-Server oder den FTP-Server sein, aber jetzt wählen einen Ordner auf der Festplatte Ihres Computers. Da zum Vorkompilieren der Site eine aktualisierbare Benutzeroberfläche soll, lassen Sie das Kontrollkästchen "Aktualisierbarkeit dieser vorkompilierten Site zulassen" aktiviert, und klicken Sie auf OK.

[![](precompiling-your-website-vb/_static/image2.png)](precompiling-your-website-vb/_static/image1.png)

**Abbildung 1**: die ASP.NET-Kompilierungstool wird Ihre Website auf den angegebenen Zielspeicherort Vorkompilieren  
 ([Klicken Sie, um das Bild in voller Größe anzeigen](precompiling-your-website-vb/_static/image3.png))

> [!NOTE]
> Option "Website veröffentlichen" im Menü "Build" ist nicht verfügbar, in Visual Web Developer. Wenn Sie Visual Web Developer verwenden, müssen Sie die Befehlszeilenversion des Tools zum ASP.NET Kompilierung verwenden, die im Abschnitt "Vorkompilieren über die Befehlszeile" beschrieben wird.


Navigieren Sie nach der Vorkompilierung der Websites an an den Zielspeicherort, die, den Sie im Dialogfeld "Website veröffentlichen" eingegeben haben. Nehmen Sie einen Moment Zeit, um den Inhalt dieses Ordners mit dem Inhalt der Website zu vergleichen. **Abbildung 2** zeigt den Ordner an Book Reviews-Website. Beachten Sie, dass sie beide enthält `.aspx` und `.aspx.cs` Dateien. Beachten Sie außerdem, dass die `Bin` enthält nur eine Datei, Verzeichnis `Elmah.dll`, die wir hinzugefügt, in der [vorherigen Tutorials](logging-error-details-with-elmah-vb.md)

[![](precompiling-your-website-vb/_static/image5.png)](precompiling-your-website-vb/_static/image4.png)

**Abbildung 2**: das Projektverzeichnis enthält `.aspx` und `.aspx.cs` Dateien; die `Bin` Ordner enthält nur `Elmah.dll`  
 ([Klicken Sie, um das Bild in voller Größe anzeigen](precompiling-your-website-vb/_static/image6.png))

**Abbildung 3** zeigt den Ziel-Ort-Ordner, deren Inhalt das ASP.NET-Kompilierungstool erstellt wurden. Dieser Ordner enthält keine Code-Behind-Dateien. Darüber hinaus des Ordners `Bin` Directory umfasst mehrere Assemblys und zwei `.compiled` Dateien zusätzlich zu den `Elmah.dll` Assembly.

[![](precompiling-your-website-vb/_static/image8.png)](precompiling-your-website-vb/_static/image7.png)

**Abbildung 3**: der Zielordner für den Speicherort enthält die Dateien für die Bereitstellung  
 ([Klicken Sie, um das Bild in voller Größe anzeigen](precompiling-your-website-vb/_static/image9.png))

Im Gegensatz zu explizite Kompilierung in drahtlosen Zugriffspunkten erstellt die Vorkompilierung zur Bereitstellung einer Assembly für den gesamten Standort nicht. Stattdessen batches es zusammen mehrere Seiten in jeder Assembly. Sie kompiliert, auch die `Global.asax` Datei (falls vorhanden) in der eigenen Assembly sowie alle Klassen in der `App_Code` Ordner. Die Dateien, die für das Speichern von deklarativen Markup für ASP.NET web Pages, Benutzersteuerelementen und Masterseiten (`.aspx`, `.ascx`, und `.master` bzw. Dateien) werden als kopiert-wird in das Zielverzeichnis für den Speicherort. Ebenso die `Web.config` Datei wird zusammen mit statischen Dateien, z. B. Bilder, CSS-Klassen und PDF-Dateien direkt über kopiert. Eine formale Beschreibung des wie das Kompilierungstool behandelt verschiedene Dateitypen, beziehen sich auf [File Handling während ASP.NET Precompilation](https://msdn.microsoft.com/library/e22s60h9.aspx).

> [!NOTE]
> Sie können das Kompilierungstool eine Assembly je Seite, Benutzersteuerelement oder Masterseite ASP.NET zu erstellen, indem Sie das Kontrollkästchen "Wird korrigiert, Benennung und Assemblys mit einzelnen Seiten" im Dialogfeld "Website veröffentlichen" anweisen. Jedes in seiner eigenen Assembly kompiliert ASP.NET-Seite ermöglicht eine präzisere Kontrolle über die Bereitstellung. Z. B. Wenn Sie eine einzelne ASP.NET-Webseite aktualisiert und erforderlich, um die Änderung bereitstellen, Sie müssen nur Bereitstellen dieser Seite des `.aspx` -Datei und die zugehörige Assembly in der produktionsumgebung bereit. Wenden Sie sich an [so wird's gemacht: festgelegten Namen zu generieren, mit dem ASP.NET-Kompilierungstool](https://msdn.microsoft.com/library/ms228040.aspx) für Weitere Informationen.


Das Zielverzeichnis für den Speicherort enthält außerdem eine Datei, die nicht Teil der vorkompilierten Webprojekt, d. h. `PrecompiledApp.config`. Diese Datei informiert, dass die Anwendung vorkompiliert wurde der ASP.NET-Laufzeit und gibt an, ob es mit einer Benutzeroberfläche aktualisiert werden, oder Mittag aktualisierbar vorkompiliert wurde.

Schließlich können Sie eine der öffnen die `.aspx` Dateien am Zielspeicherort an, die mit Visual Studio oder Text-Editor Ihrer Wahl. Wenn für die Bereitstellung mit einer Benutzeroberfläche für aktualisierbare Vorkompilierung, enthalten die ASP.NET-Seiten Speicherort im Zielverzeichnis genaue dasselbe Markup wie die entsprechenden Dateien auf der Website.

### <a name="precompiling-for-deployment-with-a-non-updatable-user-interface"></a>Vorkompilieren für die Bereitstellung mit einer nicht aktualisierbaren-Benutzeroberfläche

Der ASP.NET Compiler-Tool kann auch zum Vorkompilieren einer Website für die Bereitstellung mit einer nicht aktualisierbaren Benutzeroberfläche verwendet werden. Vorkompilieren der Site mit einer nicht aktualisierbaren Benutzeroberfläche funktioniert ähnlich wie die Vorkompilierung mit einer aktualisierbaren Benutzeroberfläche, der wesentliche Unterschied darin, dass die ASP.NET-Seiten, Benutzersteuerelementen und Masterseiten in das Zielverzeichnis von deren Markup entfernt werden. Vorkompilieren eine Website für die Bereitstellung mit einer nicht aktualisierbaren Benutzeroberfläche, wählen Sie im Menü erstellen die Option "Website veröffentlichen", aber deaktivieren Sie die Option "Aktualisierbarkeit dieser vorkompilierten Site zulassen" (siehe **Abbildung 4**).

[![](precompiling-your-website-vb/_static/image11.png)](precompiling-your-website-vb/_static/image10.png)

**Abbildung 4**: Deaktivieren Sie die "Aktualisierbarkeit dieser vorkompilierten Site zulassen" Option zum Vorkompilieren mit einer nicht aktualisierbare-Benutzeroberfläche  
 ([Klicken Sie, um das Bild in voller Größe anzeigen](precompiling-your-website-vb/_static/image12.png))

**Abbildung 5** zeigt den Zielordner für den Standort nach der Vorkompilierung mit einer nicht aktualisierbaren-Benutzeroberfläche.

[![](precompiling-your-website-vb/_static/image14.png)](precompiling-your-website-vb/_static/image13.png)

**Abbildung 5**: der Zielordner Speicherort für die Bereitstellung mit einer nicht aktualisierbaren Benutzeroberfläche  
 ([Klicken Sie, um das Bild in voller Größe anzeigen](precompiling-your-website-vb/_static/image15.png))

Vergleichen Sie **Abbildung 3** zu **Abbildung 5**. Während die beiden Ordner identisch aussehen können, beachten Sie, dass es sich bei nicht aktualisierbaren UI Ordner verfügt nicht über die die Masterseite `Site.master`. Und während **Abbildung 5** enthält die verschiedenen ASP.NET-Seiten aus, wenn Sie den Inhalt dieser Dateien anzeigen, Sie sehen, dass sie ihre deklarativen Markup entfernt und durch den Platzhaltertext ersetzt wurde haben: "Dies ist eine vom Markerdatei Das Tool Vorkompilierung und sollten nicht gelöscht werden. "

[![](precompiling-your-website-vb/_static/image17.png)](precompiling-your-website-vb/_static/image16.png)

**Abbildung 5**: deklarative Markup aus die ASP.NET-Seiten entfernt wurde

Die `Bin` Ordner im **Abbildungen 3** und **5** mehr erheblich unterscheiden. Zusätzlich zu den Assemblys die `Bin` Ordner **Abbildung 5** umfasst eine `.compiled` -Datei für jeden ASP.NET Seite Benutzersteuerelement und Masterseite.

Vorkompilieren einer Website mit einer nicht aktualisierbaren Benutzeroberfläche ist hilfreich in Situationen, in denen Sie nicht möchten die ASP.NET-Seiten Inhalt geändert werden, von der Person oder Firma, die installiert wird oder die Website in der produktionsumgebung verwaltet. Wenn Sie eine ASP.NET-Webanwendung, die Sie verkaufen, Kunden auf ihre eigenen Webserver installieren erstellen, Sie möchten sicherstellen, dass sie nicht das Aussehen und Verhalten Ihres Standorts durch direktes Bearbeiten Ändern der `.aspx` Seiten, die Sie versenden. Durch Vorkompilieren Ihrer Website mit einer nicht aktualisierbaren Benutzeroberfläche, senden Sie den Platzhalter `.aspx` Seiten als Teil der Installation, und verhindert so Ihren Kunden untersuchen, oder ändern ihre Inhalte.

### <a name="precompiling-from-the-command-line"></a>Vorkompilieren von der Befehlszeile aus

Hinter den Kulissen ruft Visual Studio Website veröffentlichen Dialogfeld die ASP.NET-Kompilierungstool (`aspnet_compiler.exe`), die Website vorzukompilieren. Alternativ können Sie dieses Tool über die Befehlszeile aufrufen. In der Tat, wenn Sie Visual Web Developer verwenden, müssen Sie das Compilertool über die Befehlszeile ausführen Visual Web Developer-Menü "Build" keine Option "Website veröffentlichen".

Starten Sie zum Verwenden der Compilertool über die Befehlszeile an die Befehlszeile löschen und navigieren Sie in das Frameworkverzeichnis, `%WINDIR%\Microsoft.NET\Framework\v2.0.50727`. Geben Sie anschließend die folgende Anweisung in die Befehlszeile ein:

`aspnet_compiler -p "physical_path_to_app" -v / -f -u "target_location_folder"`

Der obige Befehl startet das ASP.NET-Compilertool (`aspnet_compiler.exe`) und über die `-p` wechseln, und weist ihn an, führen Sie eine Vorkompilierung die Website, deren Stamm *physischen\_Pfad\_zu\_app*; Dieser Wert ist, etwa `C:\MySites\BookReviews`, und sollten durch Anführungszeichen begrenzt werden.

Die `-v` Option gibt an, das virtuelle Verzeichnis der Website. Wenn Ihre Website als der Standardwebsite in IIS-Metabasis registriert ist, dann können Sie weglassen der `-p` wechseln, und geben Sie einfach das virtuelle Verzeichnis der Anwendung. Bei Verwendung der `-p` zu wechseln, den Wert fortfahren die `-v` Switch gibt das Stammverzeichnis der Website, und wird verwendet, um die Anwendungsstamm Verweise auflösen. Z. B. Wenn Sie einen Wert angeben `-v /MySite` verweist in der Anwendung, klicken Sie dann auf `~/path/file` werden als aufgelöst `~/MySite/path/file`. Ich habe den Switch als verwendet, da die Book Reviews-Website im Stammverzeichnis auf Meine Webhostingunternehmen befindet `-v /`.

Die `-f` Switch vorhanden ist, weist das Kompilierungstool, überschreiben die *Ziel\_Speicherort\_Ordner* Verzeichnis bereits vorhanden ist. Wenn Sie weglassen der `-f` Switch und den Zielordner für den Speicherort bereits vorhanden ist, wird das Kompilierungstool mit dem Fehler beendet: "Fehler ASPRUNTIME: das Zielverzeichnis ist nicht leer. Löschen Sie sie manuell ein oder wählen Sie ein anderes Ziel."

Die `-u` wechseln, falls vorhanden, informiert Sie das Tool, um eine aktualisierbare Benutzeroberfläche zu erstellen. Lassen Sie diesen Schalter zum Vorkompilieren der Site mit einer nicht aktualisierbaren-Benutzeroberfläche.

Und schließlich die *Ziel\_Speicherort\_Ordner* ist der physische Pfad in das Zielverzeichnis Speicherort; dieser Wert ist, etwa `C:\MySites\Output\BookReviews`, und sollten durch Anführungszeichen begrenzt werden.

## <a name="deploying-the-precompiled-website"></a>Bereitstellen der vorkompilierten Website

An diesem Punkt haben wir gesehen, wie Sie mit, dass die ASP.NET-Kompilierungstool Vorkompilieren eine Website mit sowohl den aktualisierbare und nicht aktualisierbare Optionen der Benutzeroberfläche. Unseren Beispielen vorkompilierte jedoch bisher die Website in einen lokalen Ordner, und nicht in der produktionsumgebung bereit. Die gute Nachricht ist, dass die Bereitstellung der vorkompilierten Website ist ein Kinderspiel und kann erfolgen über Visual Studio oder einem anderen Datei Kopiermechanismus wie z. B. auf einem eigenständigen FTP-Client.

Das Dialogfeld "Website veröffentlichen" (dargestellte **Abbildung1**) verfügt über eine Ziel-Speicherort-Option, die angibt, wohin die vorkompilierte Websitedateien kopiert werden. Dieser Speicherort kann es sich um eine remote-Web-Server oder FTP-Server sein. Eingeben von einem Remoteserver in dieses Textfeld vorkompiliert und die Website bereitgestellt, mit dem angegebenen Server in einem Schritt. Alternativ können Sie die Vorkompilierung der Website in einen lokalen Ordner und manuell kopieren Sie den Inhalt dieses Ordners in der produktionsumgebung über FTP oder einem anderen Ansatz.

Die vorkompilierte Website, die automatisch über Visual Studio Website veröffentlichen Dialogfeld bereitgestellt ist hilfreich für einfache Websites, es keine Konfigurationsunterschiede zwischen den Umgebungen für Entwicklungs- und produktionsumgebungen gibt. Wie bereits erwähnt, in der [ *Common Configuration Unterschiede zwischen Entwicklungs- und Produktionsumgebungen* Tutorial](common-configuration-differences-between-development-and-production-vb.md) es ist nicht ungewöhnlich, dass solche Unterschiede vorhanden sein. Beispielsweise verwendet die Book Reviews-Web-Anwendung eine andere Datenbank in der produktionsumgebung als in der Entwicklungsumgebung. Wenn Visual Studio die Website auf einem Remoteserver veröffentlicht, die blind Sie die Konfigurationsinformationen für die Datei in der Entwicklungsumgebung kopiert.

Für Standorte mit der Konfigurationsunterschiede zwischen den Umgebungen für Entwicklungs- und produktionsumgebungen sein es empfehlenswert, in ein lokales Verzeichnis Site vorkompilieren, kopieren Sie die produktionsumgebung spezifische Konfigurationsdateien und kopieren Sie den Inhalt der vorkompilierten Ausgabe auf die Produktion.

Eine Auffrischung zum Kopieren von Dateien aus der Entwicklungsumgebung in die produktionsumgebung finden Sie in der [ *Bereitstellen Ihrer Website mithilfe von FTP-Client* ](deploying-your-site-using-an-ftp-client-vb.md) und [  *Bereitstellen Ihrer Website mit Visual Studio* ](determining-what-files-need-to-be-deployed-vb.md) Tutorials.

## <a name="summary"></a>Zusammenfassung

ASP.NET unterstützt zwei Modi der Kompilierung: automatisch und explizit. Wie in vorherigen Tutorials erwähnt, verwenden Sie Web Application Projects (WAPs) explizite Kompilierung während Web Site Projects (WSPs) automatische Kompilierung wird standardmäßig verwendet. Allerdings ist es möglich, explizit eine WSP-Datei vor der Bereitstellung mit dem ASP.NET-Kompilierungstool kompilieren.

Dieses Tutorial konzentriert sich auf die des Kompilierungstools durch die Vorkompilierung für die Unterstützung für die Bereitstellung. Beim Vorkompilieren zur Bereitstellung, das Compilation-Tool erstellt einen Zielordner für den Speicherort, kompiliert die angegebene Webanwendung Quellcode und kopiert diese kompilierten Assemblys und die Inhaltsdateien in den Zielordner für den Standort. Kompilierungstools kann konfiguriert werden, um eine aktualisiert werden oder nicht aktualisierbaren Benutzeroberfläche zu erstellen. Beim Vorkompilieren mit einer nicht aktualisierbaren Benutzeroberflächenoption, wird der deklarative Markup in den Inhaltsdateien entfernt. Kurz gesagt, ermöglicht durch die Vorkompilierung-Websiteprojekt-basierte Bereitstellen der Anwendung ohne alle Quellcodedateien, und klicken Sie mit der deklarativen Markup entfernt, falls gewünscht.

Viel Spaß beim Programmieren!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Tutorial erläutert finden Sie in den folgenden Ressourcen:

- [ASP.NET Web Sitevorkompilierung](https://msdn.microsoft.com/library/ms228015.aspx)
- [CodeBehind und Kompilierung in ASP.NET 2.0](https://msdn.microsoft.com/magazine/cc163675.aspx)
- [Durch die Vorkompilierung in ASP.NET](http://www.odetocode.com/Articles/417.aspx)
- [Optionen für vorkompilierte Site in ASP.NET](http://www.dotnetperls.com/precompiled)

> [!div class="step-by-step"]
> [Zurück](logging-error-details-with-elmah-vb.md)
> [Weiter](users-and-roles-on-the-production-website-vb.md)
