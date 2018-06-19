---
uid: web-forms/overview/moving-to-aspnet-20/master-pages
title: Masterseiten | Microsoft Docs
author: microsoft
description: Eine der Schlüsselkomponenten einer erfolgreichen Website ist ein einheitliches Erscheinungsbild. In ASP.NET 1.x, Entwickler, Benutzersteuerelemente mit allgemeinen Seite Elem repliziert...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 9c0cce4d-efd9-4c14-b0e8-a1a140abb3f4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/master-pages
msc.type: authoredcontent
ms.openlocfilehash: f45dd9704f665244d2a48ec000326f6e98984e4f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30885108"
---
<a name="master-pages"></a>Masterseiten
====================
durch [Microsoft](https://github.com/microsoft)

> Eine der Schlüsselkomponenten einer erfolgreichen Website ist ein einheitliches Erscheinungsbild. In ASP.NET 1.x, Entwickler, Benutzersteuerelemente mit allgemeinen Seitenelemente über eine Webanwendung repliziert. Ist das zwar sicherlich eine praktikable Lösung ist, weist die Benutzersteuerelemente verwenden einige Nachteile auf. Eine Änderung an der Position eines Benutzersteuerelements erfordert z. B. eine Änderung an mehreren Seiten über einen Standort an. Benutzersteuerelemente werden auch nicht in der Entwurfsansicht nach dem Einfügen auf einer Seite gerendert.


Eine der Schlüsselkomponenten einer erfolgreichen Website ist ein einheitliches Erscheinungsbild. In ASP.NET 1.x, Entwickler, Benutzersteuerelemente mit allgemeinen Seitenelemente über eine Webanwendung repliziert. Ist das zwar sicherlich eine praktikable Lösung ist, weist die Benutzersteuerelemente verwenden einige Nachteile auf. Eine Änderung an der Position eines Benutzersteuerelements erfordert z. B. eine Änderung an mehreren Seiten über einen Standort an. Benutzersteuerelemente werden auch nicht in der Entwurfsansicht nach dem Einfügen auf einer Seite gerendert.

ASP.NET 2.0 führt Master Seiten als eine Möglichkeit, ein einheitliches Erscheinungsbild zu verwalten und wie Sie sehen so schnell, Master Seiten eine deutliche Verbesserung über die Benutzer-Steuerelementmethode darstellen.

## <a name="why-master-pages"></a>Warum Masterseiten?

Sie vielleicht, warum Masterseiten in ASP.NET 2.0 erforderlich waren. Nachdem alle Entwickler von Websites bereits verwenden Benutzersteuerelemente in ASP.NET 1.x Inhaltsbereiche zwischen Seiten freigeben. Es gibt tatsächlich einige Gründe für die Benutzersteuerelemente eine kleiner als optimale Lösung für das Erstellen eines allgemeinen Layouts.

Benutzersteuerelemente definieren nicht tatsächlich Seitenlayout. Stattdessen definieren sie das Layout und die Funktionalität für einen Teil einer Seite. Der Unterschied zwischen diesen beiden ist wichtig, da Verwaltbarkeit von einem Benutzer Control-Lösung sehr viel schwieriger macht. Z. B. Wenn Sie die Position eines Benutzersteuerelements auf der Seite ändern möchten, müssen Sie die aktuelle Seite bearbeiten auf der das Benutzersteuerelement angezeigt wird. Thats Ordnung, wenn Sie nur einige Seiten verfügen, aber an großen Standorten schnell ein Standort Management des Verlusts wird!

Die Architektur von ASP.NET selbst ist ein weiterer Nachteil der Verwendung von Benutzersteuerelementen zur Definition eines allgemeinen Layouts abstammt. Wenn keine öffentlicher Member eines Benutzersteuerelements geändert wird, müssen Sie alle Seiten neu kompilieren, die das Benutzersteuerelement verwenden werden sollen. Dagegen wird ASP.NET dann Re-JIT-Seiten werden zuerst zugegriffen. Dies erzeugt noch einmal: eine nicht skalierbare Architektur und ein Standort Management Problem bei größeren Sites.

Beide dieser Probleme (und vieles mehr) werden von Masterseiten in ASP.NET 2.0 ordentlich adressiert.

## <a name="how-master-pages-work"></a>Funktionsweise von Masterseiten

Eine Masterseite ist analog zu einer Vorlage für die anderen Seiten. Elemente, die von anderen Seiten (d. h. Menüs, Rahmen, usw.) gemeinsam genutzt werden soll, werden die Gestaltungsvorlage hinzugefügt. Wenn neue Seiten, die dem Standort hinzugefügt werden, können Sie diese mit einer Masterseite zuordnen. Eine Seite, die mit einer Masterseite anfallen heißt ein **Inhaltsseite**. Standardmäßig führt eine Inhaltsseite auf die Darstellung der Masterseite. Bei der Erstellung einer Masterseite können Sie Teile der Seite definieren, die mit einem eigenen Inhalt Inhaltsseite ersetzen kann. Diese Teile sind definiert, verwenden ein neues Steuerelement in ASP.NET 2.0 eingeführt. die **ContentPlaceHolder** Steuerelement.

Eine Masterseite kann eine beliebige Anzahl von ContentPlaceHolder-Steuerelemente (oder überhaupt) enthalten. Auf der Seite Inhalt angezeigt wird der Inhalt von den ContentPlaceHolder-Steuerelementen innerhalb eines **Inhalt** Steuerelemente, ein anderes neues Steuerelement in ASP.NET 2.0. Standardmäßig sind die Inhaltsseiten, die Inhaltssteuerelemente leer, damit Sie Ihre eigenen Inhalte bereitstellen können. Wenn Sie den Inhalt der Masterseite in die Inhaltssteuerelemente verwenden möchten, können Sie dies also, wie Sie weiter unten in diesem Modul sehen werden. Das Inhaltssteuerelement wird an das Steuerelement ContentPlaceHolder über das ContentPlaceHolderID-Attribut des Inhaltssteuerelements zugeordnet. Der Code unten Zuordnungen ein Inhaltssteuerelement an ein ContentPlaceHolder Steuerelement MainBody auf einer Masterseite aufgerufen.

[!code-aspx[Main](master-pages/samples/sample1.aspx)]

> [!NOTE]
> Häufig hören Sie Personen Masterseiten als Basisklasse für andere Seiten zu beschreiben. Thats tatsächlich nicht "true". Die Beziehung zwischen Gestaltungsvorlagen und Inhaltsseiten ist keiner der Vererbung.


**Abbildung 1** einer Masterseite und einer zugeordneten Inhaltsseite zeigt, wie sie in Visual Studio 2005 angezeigt werden. Sehen Sie das ContentPlaceHolder-Steuerelement in der Masterseite und dem entsprechenden Inhaltssteuerelement in der Seite Inhalt. Beachten Sie, dass die Masterseiten Inhalt, der außerhalb der ContentPlaceHolder sichtbar, sind jedoch abgeblendet, in der Seite Inhalt. Nur der Inhalt innerhalb der ContentPlaceHolder kann von der Seite Inhalt ersetzt. Alle anderen Inhalte die Masterseite stammt ist unveränderlich.


![Eine Masterseite und seine zugehörigen Inhaltsseite](master-pages/_static/image1.jpg)

**Abbildung 1**: einer Masterseite und seine zugehörigen Inhaltsseite


## <a name="creating-a-master-page"></a>Erstellen einer Masterseite

So erstellen Sie eine neue Masterseite

1. Öffnen Sie Visual Studio 2005, und erstellen Sie eine neue Website.
2. Klicken Sie auf die neue Datei, die Datei.
3. Master-Datei über das Dialogfeld "Neues Element hinzufügen" auswählen, wie gezeigt in **Abbildung 2**.
4. Klicken Sie auf Hinzufügen.


![Erstellen einer neuen Masterseite](master-pages/_static/image2.jpg)

**Abbildung 2**: Erstellen einer neuen Masterseite


Beachten Sie, dass die Dateierweiterung für eine Gestaltungsvorlage <em>.master</em>. Dies ist eine der Methoden, die eine Masterseite von einer gewöhnlichen Seite abweicht. Der andere Hauptunterschied besteht darin, die statt einer @Page Richtlinie, die Gestaltungsvorlage enthält eine @Master Richtlinie. Wechseln Sie zur Quellansicht für die Master Seite, die Sie soeben erstellt haben, und überprüfen Sie den Code.

Eine neue Masterseite weisen ein Steuerelement eines ContentPlaceHolder standardmäßig. In den meisten Fällen ist es sinnvoller, erstellen Sie zunächst die gemeinsamen Seitenelemente, und fügen Sie dann ContentPlaceHolder-Steuerelemente, benutzerdefinierter Inhalt erwünscht ist. In diesen Fällen sollten Entwickler das Standardsteuerelement für ContentPlaceHolder löschen und Einfügen neuer Datensätze wie die Seite entwickelt wird. ContentPlaceHolder-Steuerelemente sind nicht in der Größe veränderbaren trotz der Tatsache, dass sie die Ziehpunkte angezeigt werden. Die Größen von ContentPlaceHolder Steuerelement automatisch basierend auf den Inhalt, den sie mit einer Ausnahme enthält; Wenn Sie z. B. einer Tabellenzelle ContentPlaceHolder-Steuerelement in einem Blockelement ablegen, wird die Größe entsprechend der Größe des Elements.

## <a name="lab-1-working-with-master-pages"></a>Übungseinheit 1 Arbeiten mit Masterseiten

In dieser Anleitung werden Sie erstellen eine neue Gestaltungsvorlage und drei ContentPlaceHolder Steuerelemente definieren. Dann erstellen Sie eine neue Seite und der Inhalt von mindestens eines der Steuerelemente ContentPlaceHolder ersetzt.

1. Erstellen einer Masterseite und Einfügen von ContentPlaceHolder-Steuerelementen. 

    1. Erstellen Sie eine neue Gestaltungsvorlage aus, wie oben beschrieben.
    2. Löschen Sie das standardmäßige ContentPlaceHolder-Steuerelement.
    3. Wählen Sie das Steuerelement ContentPlaceHolder, indem Sie auf die schattierten oberen Rand des Steuerelements, und löschen Sie es dann, indem Sie die ENTF-Taste auf der Tastatur drücken.
    4. Einfügen einer neuen Tabelle mit den *Header und Seite* Vorlage wie in Abbildung 3 gezeigt. Ändern Sie die Breite und Höhe auf 90 %, sodass die gesamte Tabelle im Designer angezeigt wird.


![](master-pages/_static/image3.jpg)

**Abbildung 3**


1. Platzieren Sie den Cursor in jeder Zelle der Tabelle, und legen Sie die *Valign* Eigenschaft *oben*.
2. Fügen Sie aus der Toolbox ein ContentPlaceHolder-Steuerelement in der obersten Zelle der Tabelle (die Headerzelle.)
3. Wenn Sie dieses Steuerelement ContentPlaceHolder einfügen, bemerken Sie, dass die Zeilenhöhe fast die gesamte Seite wirksam werden, wie in Abbildung 4 dargestellt. Werden besorgt, die an diesem Punkt.


![Der freie Speicherplatz wird in der gleichen Zelle als ContentPlaceHolder](master-pages/_static/image1.gif)

**Abbildung 4**: der freie Speicherplatz wird in der gleichen Zelle als ContentPlaceHolder


1. Platzieren Sie eine ContentPlaceHolder-Steuerelement in den anderen beiden Zellen an. Sobald die ContentPlaceHolder-Steuerelemente eingefügt wurden, sollte die Größe der Zellen der Tabelle sein, wie zu erwarten. Die Seite sollte jetzt wie im angezeigten Seite aussehen **Abbildung 5**.


![Der Master alle ContentPlaceHolder-Steuerelemente. Beachten Sie, dass die Zellenhöhe für die Headerzelle jetzt ist es liegen](master-pages/_static/image2.gif)

**Abbildung 5**: The Master alle ContentPlaceHolder-Steuerelemente. Beachten Sie, dass die Zellenhöhe für die Headerzelle jetzt ist es liegen


1. Geben Sie Text Ihrer Wahl in jede der drei ContentPlaceHolder-Steuerelemente.
2. Speichern Sie die Gestaltungsvorlage als exercise1.master.
3. Erstellen Sie eine neue WebForm und der Gestaltungsvorlage exercise1.master zuordnen.
4. Wählen Sie die neue Datei, die Datei in Visual Studio 2005.
5. Wählen Sie **Webformular** in das Dialogfeld "Neues Element hinzufügen".
6. Stellen Sie sicher, dass das Kontrollkästchen Masterseite auswählen aktiviert ist, wie in Abbildung 6 veranschaulicht.


![Eine neue Seite hinzufügen](master-pages/_static/image3.gif)

**Abbildung 6**: eine neue Seite hinzufügen


1. Klicken Sie auf Hinzufügen.
2. Dialogfeld zum Auswählen der exercise1.master in die SELECT-Anweisung einer Masterseite wie in Abbildung 7.
3. Klicken Sie auf OK, um die neue Seite hinzufügen.

Die neue Seite, die in Visual Studio mit einem Steuerelement für jedes Steuerelement ContentPlaceHolder auf der Masterseite angezeigt werden. Standardmäßig sind die Inhaltssteuerelemente leer, damit Sie Ihre eigenen Inhalte hinzufügen können. Wenn Youd sind, um den Inhalt aus dem ContentPlaceHolder-Steuerelement auf der Masterseite verwenden möchten, klicken Sie einfach auf das Smarttag-Symbol (den kleinen schwarzen Pfeil in der oberen rechten Ecke des Steuerelements) und wählen Sie *Master Inhalt standardmäßig* aus dem Smarttag wie **Abbildung 8**. Wenn Sie dies tun, ändert sich das Menüelement in *erstellen benutzerdefinierte Content*. Klicken sie zu diesem Zeitpunkt entfernt den Inhalt der Masterseite ermöglicht Ihnen, benutzerdefinierte Inhalte für dieses bestimmte Inhaltssteuerelement zu definieren.


![Festlegen eines Inhaltssteuerelements auf die Standardeinstellungen auf den Inhalt der Master-Seiten](master-pages/_static/image4.gif)

**Abbildung 7**: Festlegen eines Inhaltssteuerelements an Standardeinstellung auf den Inhalt der Master-Seiten


## <a name="connecting-master-page-and-content-pages"></a>Herstellen einer Verbindung Gestaltungsvorlage und Inhaltsseiten

Die Zuordnung zwischen einer Masterseite und einer Inhaltsseite kann in einem der vier verschiedene Arten konfiguriert werden:

- Die <strong>MasterPageFile</strong> Attribut von der @Page Richtlinie
- Festlegen der **Page.MasterPageFile** -Eigenschaft im Code.
- Die **&lt;Seiten&gt;** Element in der Konfigurationsdatei der Anwendung ("Web.config" im Stammverzeichnis der Anwendung)
- Die **&lt;Seiten&gt;** Element in einem Unterordner-Konfigurationsdatei ("Web.config" in einem Unterordner)

## <a name="masterpagefile-attribute"></a>MasterPageFile Attribute

Das Attribut MasterPageFile erleichtert es eine Masterseite für einen bestimmten ASP.NET-Seite gelten. Es ist auch die Methode verwendet, um die Gestaltungsvorlage anzuwenden, wenn Sie prüfen, ob die **Masterseite auswählen** Kontrollkästchen wie in Übung 1 haben.

## <a name="setting-pagemasterpagefile-in-code"></a>Festlegen von Page.MasterPageFile in Code

Die Eigenschaft MasterPageFile im Code festlegen, können Sie auf Ihre Inhalte zur Laufzeit eine bestimmte Masterseite anwenden. Dies ist hilfreich in Fällen, in denen müssen Sie möglicherweise eine bestimmte Masterseite basierend auf einer Rolle "Benutzer" und anderer Kriterien anwenden. Die Eigenschaft MasterPageFile muss in der PreInit-Methode festgelegt werden. Wenn sie nach der PreInit-Methode festgelegt ist, wird eine InvalidOperationException ausgelöst. Die Seite, auf dem diese Eigenschaft festgelegt wird, benötigen auch einen Inhalt Steuerelement als Steuerelement der obersten Ebene für die Seite. Andernfalls wird ein HttpException ausgelöst werden, wenn die Eigenschaft MasterPageFile festgelegt ist.

## <a name="using-the-ltpagesgt-element"></a>Mithilfe der &lt;Seiten&gt; Element

Sie können eine Masterseite für Ihren Seiten konfigurieren, indem Sie das MasterPageFile-Attribut der &lt;Seiten&gt; -Element der Datei "Web.config". Bei Verwendung dieser Methode sollten Sie bedenken, dass die web.config-Dateien, die weiter unten in der Anwendungsstruktur diese Einstellung überschreiben können. Alle MasterPageFile-Attributsatz einem @Page Richtlinie wird diese Einstellung auch überschreiben. Mithilfe der &lt;Seiten&gt; Element vereinfacht das Erstellen einer <em>master</em> Masterseite, die bei Bedarf in bestimmten Ordnern oder Dateien überschrieben werden kann.

## <a name="properties-in-master-pages"></a>Eigenschaften in Masterseiten

Eine Masterseite kann Eigenschaften verfügbar machen, einfach, indem diese Eigenschaften öffentlich innerhalb der Masterseite. Der folgende Code definiert z. B. eine Eigenschaft namens SomeProperty:

[!code-csharp[Main](master-pages/samples/sample2.cs)]

Zugreifen auf die SomeProperty-Eigenschaft von der Inhaltsseite, müssen Sie den Master verwenden Eigenschaft wie folgt:

[!code-csharp[Main](master-pages/samples/sample3.cs)]

## <a name="nesting-master-pages"></a>Schachtelung Masterseiten

Gestaltungsvorlagen werden die perfekte Lösung für eine allgemeine Aussehen und Verhalten über eine große Webanwendung sichergestellt. Allerdings ist es nicht ungewöhnlich, dass bestimmte Teile einer großen Site Freigabe eine gemeinsame Schnittstelle, obwohl andere Teile eine andere Schnittstelle gemeinsam. Zur Behebung dieser Anforderung sind mehrere Masterseiten die ideale Lösung. Jedoch nicht das Problem weiterhin der Tatsache Rechnung zu tragen, dass es sich bei eine große Anwendung möglicherweise bestimmte Komponenten (z. B. ein Menü, z. B.), die für alle Seiten freigegeben werden und andere Komponenten, die nur für bestimmte Abschnitte des Standorts, freigegeben werden. Für diesen Typ der Fall füllen verschachtelte Gestaltungsvorlagen ordentlich erforderlich. Wie Sie gesehen haben, besteht aus eine normale Masterseite von einer Masterseite und einer Inhaltsseite ein. Es gibt zwei Masterseiten, in einer Situation geschachtelte Masterseite; einem Masterauftrag für den übergeordneten und untergeordneten Master. Die untergeordnete Masterseite ist auch eine Inhaltsseite und seinem Master ist die übergeordnete Masterseite.

Hier ist der Code für eine typische Gestaltungsvorlage:

[!code-aspx[Main](master-pages/samples/sample4.aspx)]

In einem geschachtelten master Szenario wäre dies die übergeordneten Master. Eine andere Masterseite würde verwenden Sie diese Seite als Gestaltungsvorlage und dieses Codes sieht wie folgt:

[!code-aspx[Main](master-pages/samples/sample5.aspx)]

Beachten Sie, dass in diesem Szenario der untergeordneten Master auch einer Inhaltsseite für den übergeordneten-Master ist. Alle untergeordneten Masterinhalt wird innerhalb eines Inhaltssteuerelements, das den Inhalt von ContentPlaceHolder-Steuerelement des übergeordneten Elements abruft.

> [!NOTE]
> Designer-Unterstützung ist nicht verfügbar für verschachtelte Gestaltungsvorlagen. Wenn Sie mit geschachtelten Master entwickeln, müssen Sie die Datenquellensicht zu verwenden.


Dieses Video zeigt eine exemplarische Vorgehensweise zur Verwendung von verschachtelter Gestaltungsvorlagen.


![](master-pages/_static/image1.png)


[Open Vollbild-Video](master-pages/_static/nested1.wmv)


![Auswählen einer Masterseite](master-pages/_static/image4.jpg)

**Abbildung 8**: Masterseite auswählen
