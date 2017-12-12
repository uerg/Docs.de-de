---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/precompiling-your-website-vb
title: Vorkompilieren von Ihrer Website (VB) | Microsoft Docs
author: rick-anderson
description: 'Visual Studio ASP.NET-Entwickler bietet zwei Arten von Projekten: Webanwendungsprojekte (oder) und Websiteprojekte (WSPs). Eines der wichtigsten Unterschiede Betwe...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: c285dc6f-a1c6-46e6-ac03-3830947f57e3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/precompiling-your-website-vb
msc.type: authoredcontent
ms.openlocfilehash: e9f2e2d71815a2e8f17d3c505b48b69a23bceb1c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="precompiling-your-website-vb"></a>Vorkompilieren von Ihrer Website (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Herunterladen von Code](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_15_VB.zip) oder [PDF herunterladen](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial15_Precompiling_vb.pdf)

> Visual Studio ASP.NET-Entwickler bietet zwei Arten von Projekten: Webanwendungsprojekte (oder) und Websiteprojekte (WSPs). Einer der Hauptunterschiede zwischen den zwei Projekttypen besteht, dass drahtlose Zugriffspunkte müssen den Code explizit vor der Bereitstellung kompiliert, während der Code in eine WSP automatisch auf dem Webserver kompiliert werden kann. Allerdings ist es möglich, eine WSP vor der Bereitstellung vorkompilieren. In diesem Lernprogramm wird erklärt, die Vorteile der Vorkompilierung und gezeigt, wie zum Vorkompilieren einer Website von innerhalb von Visual Studio und von der Befehlszeile aus.


## <a name="introduction"></a>Einführung

Visual Studio bietet ASP.NET-Entwickler zwei Projekttypen: Web Webanwendungsprojekten (WAP) und Website-Projekte (WSP). Einer der wichtigsten Unterschiede zwischen diesen Projekttypen ist, dass drahtlose Zugriffspunkte erfordern *explizite Kompilierung* während WSPs verwenden *automatische Kompilierung*, in der Standardeinstellung. Mit der drahtlosen Zugriffspunkten, kompilieren Sie Code der Webanwendung in einer einzelnen Assembly, die auf der Website erstellt wird `Bin` Ordner. Bereitstellung umfasst das Kopieren den Inhalt des Markups (die `.aspx.ascx`, und `.master` Dateien) in das Projekt, und die Assembly in die `Bin` Ordner, den Code-Behind-Klassendateien selbst müssen nicht bereitgestellt werden. Andererseits, stellen Sie WSPs durch Kopieren der Markupseiten und die entsprechenden Code-Behind-Klassen in der produktionsumgebung bereit. Die Code-Behind-Klassen werden bei Bedarf auf dem Webserver kompiliert.

> [!NOTE]
> Verweisen zurück zum Abschnitt "Explizite Kompilierung und automatische Kompilierung" in der [ *bestimmen was-Dateien müssen bereitgestellt werden* Lernprogramm](determining-what-files-need-to-be-deployed-vb.md) Weitere Hintergrundinformationen zu den Unterschieden zwischen dem Projekt Modelle, explizite und automatische Kompilierung und wie das Kompilierungsmodell Bereitstellung auswirkt.


Die Option für die automatische Kompilierung ist einfach zu verwenden. Es ist keine explizite Kompilierungsschritt und nur die Dateien, die wurden geändert, müssen bereitgestellt werden, während die explizite Kompilierung aktualisiert werden, die geänderten Markupseiten und der JIT-kompilierten Assembly bereitstellen. Allerdings wurde die automatische Bereitstellung zwei mögliche Nachteile:

- Da die Seiten automatisch kompiliert werden müssen, wenn sie zuerst besucht werden, treten möglicherweise Wenn zum ersten Mal nach der Bereitstellung eine ASP.NET-Seite angefordert wird eine kurze, jedoch bemerkbar Verzögerung auf.
- Automatische Kompilierung erfordert, dass sowohl die deklarative Markup und Quellcode Code auf dem Webserver vorhanden sein. Dies kann uninteressant Option sein, wenn Sie planen, auf den Verkauf von der Webanwendung für Kunden, die auf ihre Webserver installiert werden.

Wenn entweder der beiden oben Mängel Geschäft wörtertrennungen, können Sie zu dem WAP-Modell wechseln oder *Vorkompilieren* die WSP vor der Bereitstellung. Dieses Lernprogramm die am besten geeigneten für eine gehostete Website Vorkompilierungsoptionen untersucht und führt Sie durch den Vorkompilierungsprozess und die Bereitstellung einer vorkompilierte Website.

## <a name="an-overview-of-aspnet-code-generation-and-compilation"></a>Eine Übersicht über ASP.NET Codegenerierung und-Kompilierung

Bevor wir die verfügbaren Optionen für die Vorkompilierung betrachten, zunächst sprechen wir über das Generieren und kompilieren, das auftritt, wenn zum ersten Mal eine ASP.NET-Seite angefordert wird, da es erstellt oder zuletzt aktualisiert wurde. Wie Sie wissen, ASP.NET-Seiten bestehen aus zwei Teilen: deklaratives Markup in der `.aspx` Datei, und ein Teil der Code, in der Regel in einer separaten Code-Behind-Klassendatei (`.aspx.vb`). Die Schritte, die von der Laufzeit ausgeführt wird, wenn eine ASP.NET-Seite angefordert wird, hängt von der Anwendung Kompilierungsmodell ab.

Mit drahtlosen Zugriffspunkten muss der Seiten-Quellcode explizit in einer einzelnen Assembly bereitgestellt wird, bevor Sie kompiliert werden. Während der Bereitstellung werden diese Assembly und den verschiedenen Markupseiten in der produktionsumgebung kopiert. Wenn eine Anforderung an den Webserver für eine ASP.NET-Seite eingeht, wird die Common Language Runtime erstellt eine Instanz von der Seite Code-Behind-Klasse und ruft seine `ProcessRequest` Methode, die den Seitenlebenszyklus beginnt und schließlich generiert der Inhalt der Seite, die an zurückgegeben wird der anforderer. Die Common Language Runtime kann mit der ASP.NET-Seite Code-Behind-Klasse arbeiten, da der Code-Behind-Klasse in eine Assembly vor der Bereitstellung bereits kompiliert wurde.

Bei WSPs und automatische Kompilierung ist keine explizite Kompilierungsschritt vor der Bereitstellung. Stattdessen konzentriert sich Bereitstellung deklarativen und der Quellinhalt von Code in der produktionsumgebung kopieren. Wenn eine Anforderung an den Webserver für eine ASP.NET-Seite zum ersten Mal eingeht seit die Seite erstellt oder zuletzt aktualisiert wurde, muss die Common Language Runtime zuerst die Code-Behind-Klasse in eine Assembly kompilieren. Diese kompilierte Assembly im Ordner gespeichert ist `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files`, obwohl der Speicherort dieses Ordners kann, über angepasst werden die `<pages>` Element im `Web.config`. Da die Assembly ist auf dem Datenträger gespeichert, er muss nicht auf nachfolgende Anforderungen zur selben Seite neu kompiliert werden.

> [!NOTE]
> Wie zu erwarten, besteht eine kurze Verzögerung beim Anfordern einer Seite zum ersten Mal (oder zum ersten Mal seit sie geändert wurde) an einem Standort, die automatische Kompilierung verwendet werden, wie es einen Moment, während der Server den Anzeigezustand der Seite Code kompilieren und Speichern der resultierenden Assembly benötigt Datenträger.


Kurz gesagt, mit der expliziten Kompilierung müssen Sie zum Kompilieren von Quellcode für die Website vor der Bereitstellung, speichern die Laufzeit aus diesen Schritt ausführen. Mit der automatische Kompilierung verarbeitet die Common Language Runtime die Kompilierung von Quellcode für die Seiten, aber mit einem geringfügigen Initialisierung Kosten für den ersten Besuch der Seite, da erstellt oder zuletzt aktualisiert wurde.

Aber was ist mit der deklarativen Teil ASP.NET-Seiten (die `.aspx` Datei)? Es ist offensichtlich, dass eine Beziehung zwischen besteht den `.aspx` Dateien und den Code in ihren Code-Behind-Klassen als den Websteuerelementen in deklarativen Markup definierten im Code zugegriffen werden. Es ist auch ersichtlich, die den Inhalt in die `.aspx` Dateien wirkt sich erheblich auf die gerenderten Markups, die von der Seite generiert. Wie funktioniert die Common Language Runtime mit dem Text, HTML, und die Syntax der Webserver-Steuerelement definiert, der `.aspx` Datei zum Generieren der angeforderten Seite gerenderten Inhalts?

Ich möchte nicht zu zu den Implementierungsdetails auf niedriger Ebene abzukommen der zwischen drahtlose Zugriffspunkte und WSPs variieren, aber kurz gesagt generiert die Laufzeit automatisch eine Klassendatei, die die verschiedenen Websteuerelementen als geschützte Member und Methoden enthält. Diese Datei wird als implementiert eine *Teilklasse* auf die entsprechenden Code-Behind-Klasse. ([Teilklassen](http://www.dotnet-guide.com/partialclasses.html) für den Inhalt einer einzelnen Klasse, die auf mehrere Dateien verteilt werden können.) Aus diesem Grund wird der Code-Behind-Klasse definiert, an zwei Orten: in der `.aspx.vb` -Datei, die Sie erstellen und in dieser automatisch generierte Klasse, die von der Laufzeit erstellt. Diese automatisch generierte Klasse befindet sich in der `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files` Ordner.

Die wichtigen einsenden ist hier, dass für eine ASP.NET-Seite an, die von der Laufzeit gerendert werden die deklarative und Teile der Source-Code müssen in eine Assembly kompiliert werden. Mit Webanwendungsprojekte Quellcode explizit in eine Assembly vor der Bereitstellung kompiliert wird, aber deklarative Markup muss weiterhin in Code konvertiert und von der Laufzeit auf dem Webserver kompiliert. Bei WSPs automatische Kompilierung verwenden müssen den Quellcode und deklaratives Markup vom Webserver kompiliert werden.

Es ist möglich, explizite Kompilierung mit der WSP-Modell zu verwenden. Sie können explizit mit dem WAP-Modell der Teil der Code, wie kompilieren. Dazu können Sie auch die deklarative Markup kompilieren.

## <a name="precompilation-options"></a>Vorkompilierungsoptionen

Lieferumfang von .NET Framework ein [ASP.NET Kompilierung-Tool (`aspnet_compiler.exe`)](https://msdn.microsoft.com/en-us/library/ms229863.aspx) mit der Sie zum Kompilieren von Quellcode (und auch den Inhalt) von einer ASP.NET-Anwendung mithilfe der WSP-Modell erstellt. Dieses Tool wurde mit .NET Framework, Version 2.0 veröffentlicht und befindet sich der `%WINDIR%\Microsoft.NET\Framework\v2.0.50727` Ordner können verwendet werden, über die Befehlszeile oder über Option "Website veröffentlichen" im Menü erstellen in Visual Studio gestartet.

Das Kompilierungstool bietet zwei allgemeine Arten der Kompilierung: direkte Vorkompilierung und Vorkompilierung für die Bereitstellung. Führen Sie für die direkte Vorkompilierung der `aspnet_compiler.exe` -tool über die Befehlszeile aus, und geben Sie den Pfad für das virtuelle Verzeichnis oder physische Pfad einer Website, die sich befindet, auf dem Computer. Kompiliert das Kompilierungstool anschließend jede ASP.NET-Seite in das Projekt, und speichern die kompilierte Version in der `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files` Ordner hatte wie wenn Seiten jeweils zum ersten Mal in einem Browser besucht wurde. Direkte Vorkompilierung kann beschleunigen der ersten Anforderung an neu bereitgestellten ASP.NET-Seiten auf Ihrer Website vorgenommen werden, da es die Laufzeit zum Ausführen dieses Schritts nicht verringert. Direkte Vorkompilierung ist jedoch nicht nützlich für die Mehrheit der gehosteten Websites, da sie erfordert, dass Sie Programme aus der Webservers Befehlszeile ausgeführt werden. In der gemeinsam genutzten Hostingumgebungen ist diese Zugriffsebene nicht zulässig.

> [!NOTE]
> Weitere Informationen über die direkte Vorkompilierung Auschecken [How To: Precompile ASP.NET Web Sites](https://msdn.microsoft.com/en-us/library/ms227972.aspx) und [Vorkompilierung in ASP.NET 2.0](http://www.odetocode.com/Articles/417.aspx).


Statt die Seiten der Website zum Kompilieren der `Temporary ASP.NET Files` Ordner Vorkompilierung für die Bereitstellung kompiliert die Seiten in einem Verzeichnis Ihrer Wahl in einem Format, das in der produktionsumgebung bereitgestellt werden kann.

Es gibt zwei Arten der Vorkompilierung für die Bereitstellung, die wir in diesem Lernprogramm nachgehen: Vorkompilierung mit eine aktualisierbare Benutzeroberfläche und mit einer nicht aktualisierbare Benutzeroberfläche Vorkompilierung. Vorkompilierung mit eine aktualisierbare Benutzeroberfläche verlässt den deklarative Markup in der `.aspx`, `.ascx`, und `.master` Dateien, sodass ein Entwickler anzeigen und, falls gewünscht, ändern die deklarative Markup auf dem Produktionsserver. Vorkompilierung mit einer nicht aktualisierbare Benutzeroberfläche generiert `.aspx` Seiten, die ein und entfernt alle Inhalte, die "void" sind `.ascx` und `.master` Dateien, und dadurch zum Ausblenden von deklarativem Markup und Änderung von verbietet einen Entwickler die die produktionsumgebung.

### <a name="precompiling-for-deployment-with-an-updatable-user-interface"></a>Vorkompilieren für die Bereitstellung mit einer aktualisierbaren Benutzeroberfläche

Die beste Methode zur Vorkompilierung für die Bereitstellung zu verstehen ist ein Beispiel, in Aktion zu sehen. Wir Vorkompilieren der Buch Reviews WSP für die Bereitstellung über eine aktualisierbare Benutzeroberfläche. Das ASP.NET Kompilierung-Tool kann von Visual Studio im Menü oder über die Befehlszeile aufgerufen werden. In diesem Abschnitt werden mithilfe des Tools von Visual Studio; im Abschnitt "Vorkompilieren in der Befehlszeile" prüft das Compilertool über die Befehlszeile ausgeführt wird.

Öffnen Sie das Buch Review WSP in Visual Studio, öffnen Sie das Build-Menü, und wählen Sie die Menüoption Website veröffentlichen. Dadurch wird das Dialogfeld "Website veröffentlichen" (siehe **Abbildung 1**), in dem Sie den Zielspeicherort an, und zwar unabhängig davon, ob die vorkompilierte Website-Benutzeroberfläche aktualisiert werden kann und anderen Tool Compileroptionen angeben können. Der Zielort kann einem Remotewebserver oder FTP-Server, aber wählen Sie jetzt einen Ordner auf der Festplatte Ihres Computers. Da wir die Website mit einer aktualisierbaren Benutzeroberfläche Vorkompilieren möchten, lassen Sie der Kontrollkästchen "Diese vorkompilierte Website als aktualisierbare zulassen" aktiviert, und klicken Sie auf OK.

[![](precompiling-your-website-vb/_static/image2.png)](precompiling-your-website-vb/_static/image1.png)

**Abbildung 1**: ASP.NET Compilation-Tool wird Ihrer Website, die am angegebenen Zielort Vorkompilieren  
 ([Klicken Sie hier, um das Bild in voller Größe angezeigt](precompiling-your-website-vb/_static/image3.png))

> [!NOTE]
> Die Option "Website veröffentlichen" im Menü "Build" ist nicht in Visual Web Developer verfügbar. Wenn Sie Visual Web Developer verwenden, müssen Sie die Befehlszeilenversion des Tools Kompilierung ASP.NET verwenden, die im Abschnitt "Vorkompilieren in der Befehlszeile" abgedeckt wird.


Navigieren Sie nach der Vorkompilierung der Website an zu den Zielspeicherort an, den Sie im Dialogfeld "Website veröffentlichen" eingegeben haben. Nehmen Sie einen Moment Zeit, um den Inhalt dieses Ordners mit dem Inhalt der Website zu vergleichen. **Abbildung 2** zeigt den Websiteordner Buch Reviews. Beachten Sie, dass sie sowohl enthält `.aspx` und `.aspx.cs` Dateien. Beachten Sie außerdem, dass die `Bin` Verzeichnis enthält nur eine Datei `Elmah.dll`, die wir hinzugefügt, in der [vorherigen Lernprogramm](logging-error-details-with-elmah-vb.md)

[![](precompiling-your-website-vb/_static/image5.png)](precompiling-your-website-vb/_static/image4.png)

**Abbildung 2**: enthält das Projektverzeichnis `.aspx` und `.aspx.cs` Dateien; die `Bin` Ordner enthält nur`Elmah.dll`  
 ([Klicken Sie hier, um das Bild in voller Größe angezeigt](precompiling-your-website-vb/_static/image6.png))

**Abbildung 3** zeigt den Zielort, deren Inhalt vom Tool ASP.NET Kompilierung erstellt wurden. Dieser Ordner enthält keine Code-Behind-Dateien. Darüber hinaus führt dieser Ordner `Bin` Verzeichnis enthält mehrere Assemblys und zwei `.compiled` Dateien zusätzlich zu den `Elmah.dll` Assembly.

[![](precompiling-your-website-vb/_static/image8.png)](precompiling-your-website-vb/_static/image7.png)

**Abbildung 3**: der Ziel-Ordner enthält die Dateien für die Bereitstellung  
 ([Klicken Sie hier, um das Bild in voller Größe angezeigt](precompiling-your-website-vb/_static/image9.png))

Im Gegensatz zu explizite Kompilierung in drahtlosen Zugriffspunkten erstellt der Vorkompilierung zur Bereitstellung einer Assembly für den gesamten Standort nicht. Stattdessen batches es zusammen mehrere Seiten in jeder Assembly. Kompiliert auch das `Global.asax` Datei (falls vorhanden) in der eigenen Assembly sowie alle Klassen in der `App_Code` Ordner. Die Dateien mit deklarativem Markup für die ASP.NET web-Seiten und Benutzersteuerelemente Gestaltungsvorlagen (`.aspx`, `.ascx`, und `.master` -Dateien, die jeweils) werden als kopiert-in das Zielverzeichnis der Speicherort ist. Entsprechend der `Web.config` Datei wird direkt über zusammen mit statischen Dateien, z. B. Bilder, CSS-Klassen und PDF-Dateien kopiert. Für eine formale Beschreibung, wie das Kompilierungstool behandelt verschiedene Dateitypen finden Sie [Datei Behandlung während ASP.NET-Vorkompilierung](https://msdn.microsoft.com/en-us/library/e22s60h9.aspx).

> [!NOTE]
> Sie können das Kompilierungstool, um eine Assembly pro Seite, Benutzersteuerelement oder Masterseite von ASP.NET zu erstellen, indem Sie das Kontrollkästchen "Verwendet, die feste benennen und einseitige Assemblys" im Dialogfeld "Website veröffentlichen" anweisen. Da jede ASP.NET-Seite in der eigenen Assembly kompiliert werden können für eine präzisere Kontrolle über die Bereitstellung. Z. B. Wenn Sie eine einzelne ASP.NET-Webseite aktualisiert und erforderlich, um die Änderung bereitstellen, Sie müssen nur Bereitstellen dieser Seite `.aspx` Datei und zugehörige Assembly in der produktionsumgebung. Wenden Sie sich an [How To: festen Namen zu generieren, mit dem ASP.NET Kompilierungstool](https://msdn.microsoft.com/en-us/library/ms228040.aspx) für Weitere Informationen.


Das Zielverzeichnis für den Speicherort enthält außerdem eine Datei, die nicht Teil des vorkompilierten Webprojekts, nämlich `PrecompiledApp.config`. Diese Datei informiert, dass die Anwendung vorkompiliert wurde die ASP.NET-Laufzeit und gibt an, ob es mit einer Benutzeroberfläche aktualisiert werden, oder Mittag aktualisierbare vorkompiliert wurde.

Schalten Sie schließlich einen Moment Zeit, öffnen Sie eines der `.aspx` Dateien am Zielspeicherort, der mit Visual Studio oder Texteditor Ihrer Wahl. Wenn für die Bereitstellung mit einer aktualisierbaren Benutzeroberfläche vorkompilieren, enthalten die ASP.NET-Seiten im Zielverzeichnis Speicherort genaue gleiche Markup als die entsprechenden Dateien auf der Website an.

### <a name="precompiling-for-deployment-with-a-non-updatable-user-interface"></a>Vorkompilieren für die Bereitstellung mit einer Benutzeroberfläche nicht aktualisierbar

Vom Compiler-Tool für ASP.NET kann auch zum Vorkompilieren einer Website für die Bereitstellung mit einer nicht aktualisierbare Benutzeroberfläche verwendet werden. Vorkompilieren von der Website mit einer nicht aktualisierbare Benutzeroberfläche funktioniert ähnlich wie das Vorkompilieren von aktualisierbaren Benutzeroberfläche der wesentliche Unterschied, dass die ASP.NET-Seiten, Benutzersteuerelemente und Masterseiten im Zielverzeichnis der Markup entfernt werden. Vorkompilieren eine Website für die Bereitstellung mit einer Benutzeroberfläche nicht aktualisierbar, wählen Sie im Menü erstellen auf die Website veröffentlichen-Option, aber deaktivieren Sie die Option "Zulassen" dieser vorkompilierte Website als aktualisierbare"(siehe **Abbildung 4**).

[![](precompiling-your-website-vb/_static/image11.png)](precompiling-your-website-vb/_static/image10.png)

**Abbildung 4**: Deaktivieren Sie die "Dieser vorkompilierte Website als aktualisierbare zulassen" Option zum Vorkompilieren mit einer nicht aktualisierbare Benutzeroberfläche  
 ([Klicken Sie hier, um das Bild in voller Größe angezeigt](precompiling-your-website-vb/_static/image12.png))

**Abbildung 5** zeigt den Zielort nach der Vorkompilierung mit einer Benutzeroberfläche nicht aktualisierbar.

[![](precompiling-your-website-vb/_static/image14.png)](precompiling-your-website-vb/_static/image13.png)

**Abbildung 5**: der Zielort für die Bereitstellung mit einer Benutzeroberfläche nicht aktualisierbar  
 ([Klicken Sie hier, um das Bild in voller Größe angezeigt](precompiling-your-website-vb/_static/image15.png))

Vergleichen Sie **abbildung3** auf **Abbildung 5**. Beachten Sie, dass nicht aktualisierbare UI-Ordner die Gestaltungsvorlage fehlt, während die beiden Ordner identisch aussehen können, `Site.master`. Dagegen **Abbildung 5** enthält die verschiedenen ASP.NET-Seiten, wenn Sie die Inhalte dieser Dateien anzeigen, Sie sehen, dass ihre deklaratives Markup entfernt und durch den Platzhaltertext ersetzt wurde haben: "This is eine Markerdatei generiert von der Vorkompilierung Tool und sollte nicht gelöscht werden. "

[![](precompiling-your-website-vb/_static/image17.png)](precompiling-your-website-vb/_static/image16.png)

**Abbildung 5**: deklarative Markup aus den ASP.NET-Seiten entfernt wurde

Die `Bin` Ordner im **Zahlen 3** und **5** mehr erheblich unterscheiden. Zusätzlich zu den Assemblys die `Bin` Ordner **Abbildung 5** umfasst eine `.compiled` -Datei für jeden ASP.NET Seite Benutzersteuerelement und Masterseite.

Vorkompilieren von einem Standort mit einer nicht aktualisierbare Benutzeroberfläche eignet sich für Situationen, in denen Sie nicht möchten die ASP.NET-Seiten Inhalt von der Person oder Firma, die installiert und verwaltet die Website in der produktionsumgebung geändert werden. Wenn Sie eine ASP.NET-Webanwendung, die Kunden auf ihre eigenen Webservern installieren angeboten erstellen, möchten Sie möglicherweise stellen Sie sicher, dass sie nicht das Erscheinungsbild Ihrer Website zu ändern, indem Sie direkt zu bearbeiten der `.aspx` Seiten, die Sie versenden. Durch das Vorkompilieren von Ihrer Website mit einer Benutzeroberfläche nicht aktualisierbar, liefern Sie den Platzhalter `.aspx` Seiten im Rahmen der Installation, um Ihren Kunden untersuchen oder ändern ihre Inhalte zu verhindern.

### <a name="precompiling-from-the-command-line"></a>Vorkompilieren von über die Befehlszeile

Im Hintergrund ruft Visual Studio-Website veröffentlichen (Dialogfeld) das ASP.NET Kompilierung-Tool (`aspnet_compiler.exe`) zum Vorkompilieren der Website. Alternativ können Sie dieses Tool von der Befehlszeile aus aufrufen. In der Tat bei Verwendung von Visual Web Developer müssen dann Sie die Compiler-Tool über die Befehlszeile ausgeführt wie Visual Web Developer im Menü die Option Veröffentlichen von Websites nicht enthalten ist.

Um vom Compiler-Tool über die Befehlszeile verwenden, zu starten, indem Sie an die Befehlszeile löschen und das Navigieren zu den Frameworkverzeichnis `%WINDIR%\Microsoft.NET\Framework\v2.0.50727`. Geben Sie anschließend die folgende Anweisung in der Befehlszeile ein:

`aspnet_compiler -p "physical_path_to_app" -v / -f -u "target_location_folder"`

Der obige Befehl startet das ASP.NET Compiler-Tool (`aspnet_compiler.exe`) und über die `-p` wechseln möchten, weist er zum Vorkompilieren der Website als Stammknoten *physischen\_Pfad\_auf\_app*; Dieser Wert ist, etwa `C:\MySites\BookReviews`, und von Anführungszeichen begrenzt werden soll.

Die `-v` Switch gibt das virtuelle Verzeichnis der Website an. Wenn Ihre Website als der Standardwebsite in IIS-Metabasis registriert ist, und klicken Sie dann-Eigenschaftenmethode der `-p` wechseln, und geben Sie einfach das virtuelle Verzeichnis der Anwendung. Bei Verwendung der `-p` wechseln, den Wert fortfahren die `-v` Switch gibt das Stammverzeichnis der Website, und wird verwendet, um die Anwendungsstamm Verweise auflösen. Für die Instanz, wenn Sie einen Wert angeben `-v /MySite` verweist in der Anwendung, klicken Sie dann auf `~/path/file` aufgelöst als `~/MySite/path/file`. Da das Buch Reviews-Website im Stammverzeichnis auf Meine Webhostinganbieter befindet haben den Switch verwendet `-v /`.

Die `-f` Switch, falls vorhanden, weist das Kompilierungstool, überschreiben die *Ziel\_Speicherort\_Ordner* Verzeichnis bereits vorhanden ist. Wenn Sie weglassen der `-f` Switch- und Ordner für das Ziel bereits vorhanden ist, wird das Kompilierungstool mit dem Fehler beendet: "Fehler ASPRUNTIME: das Zielverzeichnis ist nicht leer. Löschen Sie es manuell oder wählen Sie ein anderes Ziel."

Die `-u` Switch, sofern vorhanden, informiert das Tool, um eine aktualisierbare Benutzeroberfläche zu erstellen. Lassen Sie diesen Schalter, wenn die Website mit einer nicht aktualisierbare Benutzeroberfläche Vorkompilieren Weg.

Abschließend wird die *Ziel\_Speicherort\_Ordner* ist der physische Pfad in das Zielverzeichnis Speicherort; dieser Wert ist, etwa `C:\MySites\Output\BookReviews`, und von Anführungszeichen begrenzt werden soll.

## <a name="deploying-the-precompiled-website"></a>Vorkompilierte Website bereitstellen

Zu diesem Zeitpunkt haben wir gesehen, wie das ASP.NET Kompilierung-Tool zum Vorkompilieren einer Website mithilfe der beiden aktualisierbar und nicht aktualisierbar Benutzeroberflächenoptionen verwenden. Unsere Beispiele vorkompilierte jedoch bisher die Website in einen lokalen Ordner, und nicht in der produktionsumgebung. Die gute Nachricht ist, ein Kinderspiel ist und über Visual Studio oder über einen anderen Datei Kopiermechanismus, wie z. B. von einem eigenständigen FTP-Client erfolgen kann die vorkompilierte Website bereitstellen.

Das Dialogfeld "Website veröffentlichen" (zuerst in angezeigten **Abbildung 1**) verfügt über eine Ziel-Speicherort-Option, die angibt, in die vorkompilierte Websitedateien kopiert werden. Dieser Speicherort kann es sich um einen Remotewebserver oder FTP-Server sein. Eingeben von einem Remoteserver in dieses Textfeld vorkompiliert und die Website mit dem angegebenen Server in einem Schritt bereitgestellt. Alternativ können Sie die Website in einen lokalen Ordner vorkompilieren und manuell kopieren Sie den Inhalt des Ordners in der produktionsumgebung über FTP oder einem anderen nicht linearen Ansatz.

Die vorkompilierte Website automatisch bereitgestellt, über das Dialogfeld für Visual Studio-Website veröffentlichen ist hilfreich für einfache Websites, wo es keine Konfigurationsunterschiede zwischen der Entwicklung und Produktion. Allerdings als notiert wurden die [ *Common Configuration Unterschiede zwischen Entwicklungs- und Produktionsumgebungen* Lernprogramm](common-configuration-differences-between-development-and-production-vb.md) es ist nicht ungewöhnlich, dass solche Unterschiede vorhanden sein. Die Webanwendung Buch Reviews verwendet beispielsweise eine andere Datenbank in der produktionsumgebung als in der Entwicklungsumgebung. Wenn Visual Studio die Website auf einem Remoteserver veröffentlicht kopiert Blind oben die Konfigurationsinformationen für die Datei in der Entwicklungsumgebung.

Für Standorte mit Configuration Unterschiede zwischen der Entwicklungs- und produktionsumgebungen kann es sein sollten Vorkompilieren der Website in ein lokales Verzeichnis, kopieren Sie die Konfigurationsdateien Produktions-spezifischen Konfiguration und kopieren Sie den Inhalt der vorkompilierten Ausgabe auf Produktion.

Aufzufrischen zum Kopieren von Dateien aus der Entwicklungsumgebung in die produktionsumgebung finden Sie in der [ *Bereitstellung Ihrer Website mithilfe von FTP-Client* ](deploying-your-site-using-an-ftp-client-vb.md) und [  *Bereitstellen von der Website mit Visual Studio* ](determining-what-files-need-to-be-deployed-vb.md) Lernprogramme.

## <a name="summary"></a>Zusammenfassung

ASP.NET unterstützt zwei Bereitstellungsmodi Kompilierung: automatisch und explizit. Wie im vorherigen Lernprogrammen erläutert wird, verwenden Sie Webanwendungsprojekte (drahtlose Zugriffspunkte) explizite Kompilierung während Websiteprojekte (WSPs) automatische Kompilierung standardmäßig verwenden. Allerdings ist es möglich, eine WSP vor der Bereitstellung mithilfe des ASP.NET Kompilierungstools explizit zu kompilieren.

Dieses Lernprogramm konzentriert sich auf die Kompilierung des Programms Vorkompilierung für die Unterstützung für die Bereitstellung. Bei der Bereitstellung das Vorkompilieren von, die Kompilierungstool erstellt einen Zielordner für den Speicherort, kompiliert die angegebene Webanwendung Quellcode und kopiert diese Assemblys und die Inhaltsdateien in den Zielordner für die Speicherort kompiliert. Das Kompilierungstool kann konfiguriert werden, um eine aktualisierbare oder nicht aktualisierbare Benutzeroberfläche zu erstellen. Mit einem nicht aktualisierbare Benutzeroberflächenoption vorkompilieren, wird das deklarative Markup in die Inhaltsdateien entfernt. Kurz gesagt, können mit Vorkompilierung Websiteprojekt basierende Bereitstellen der Anwendung ohne alle Quellcodedateien und mit der deklarativem Markup entfernt, falls gewünscht.

Viel Spaß beim Programmieren!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Lernprogramm erläutert finden Sie in den folgenden Ressourcen:

- [ASP.NET Web Site Vorkompilierung](https://msdn.microsoft.com/en-us/library/ms228015.aspx)
- [CodeBehind und Kompilierung in ASP.NET 2.0](https://msdn.microsoft.com/en-us/magazine/cc163675.aspx)
- [Vorkompilierung in ASP.NET](http://www.odetocode.com/Articles/417.aspx)
- [Vorkompilierte Website Optionen in ASP.NET](http://www.dotnetperls.com/precompiled)

>[!div class="step-by-step"]
[Zurück](logging-error-details-with-elmah-vb.md)
[Weiter](users-and-roles-on-the-production-website-vb.md)
