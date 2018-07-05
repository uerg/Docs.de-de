---
uid: web-forms/overview/moving-to-aspnet-20/master-pages
title: Masterseiten | Microsoft-Dokumentation
author: microsoft
description: Eine der Hauptkomponenten für eine erfolgreiche Website ist ein einheitliches Erscheinungsbild. In ASP.NET 1.x, Entwickler verwendet Steuerelemente, um allgemeine Seite Elem zu replizieren...
ms.author: aspnetcontent
ms.date: 02/20/2005
ms.assetid: 9c0cce4d-efd9-4c14-b0e8-a1a140abb3f4
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/master-pages
msc.type: authoredcontent
ms.openlocfilehash: 631aa39c51601f1ae843079d8cd930f77b1093ab
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37819214"
---
<a name="master-pages"></a>Masterseiten
====================
durch [Microsoft](https://github.com/microsoft)

> Eine der Hauptkomponenten für eine erfolgreiche Website ist ein einheitliches Erscheinungsbild. In ASP.NET 1.x, Entwickler verwendet Steuerelemente, um allgemeine Elemente der Seite über eine Webanwendung zu replizieren. Sicherlich eine praktikable Lösung ist, muss die Benutzersteuerelemente mit, dass einige Nachteile. Eine Änderung an der Position eines Steuerelements erfordert beispielsweise eine Änderung an mehrere Seiten auf einer Website an. Benutzersteuerelemente werden auch nicht in der Entwurfsansicht nach eingefügt wird, auf einer Seite gerendert.


Eine der Hauptkomponenten für eine erfolgreiche Website ist ein einheitliches Erscheinungsbild. In ASP.NET 1.x, Entwickler verwendet Steuerelemente, um allgemeine Elemente der Seite über eine Webanwendung zu replizieren. Sicherlich eine praktikable Lösung ist, muss die Benutzersteuerelemente mit, dass einige Nachteile. Eine Änderung an der Position eines Steuerelements erfordert beispielsweise eine Änderung an mehrere Seiten auf einer Website an. Benutzersteuerelemente werden auch nicht in der Entwurfsansicht nach eingefügt wird, auf einer Seite gerendert.

ASP.NET 2.0 werden Master Seiten als eine Möglichkeit zum Verwalten eines konsistenten Aussehens und Verhaltens, und Sie werden bald feststellen, Master Seiten stellen eine erhebliche Verbesserung dar, über die Benutzer Control-Methode.

## <a name="why-master-pages"></a>Warum Masterseiten?

Sie Fragen sich vielleicht Warum Masterseiten in ASP.NET 2.0 erforderlich waren. Schließlich Entwickler von Websites verwenden bereits Steuerelemente in ASP.NET 1.x, Inhaltsbereiche zwischen Seiten freizugeben. Es gibt tatsächlich mehrere Gründe, warum die Benutzersteuerelemente eine weniger als optimale Lösung für das Erstellen eines allgemeinen Layouts sind.

Benutzersteuerelemente definieren nicht tatsächlich Seitenlayout. Stattdessen definieren sie das Layout und die Funktionalität für einen Teil einer Seite. Der Unterschied zwischen diesen beiden ist wichtig, da es Verwaltbarkeit der Lösung für einen Benutzer sehr viel schwieriger ist. Z. B. Wenn Sie die Position eines Steuerelements auf der Seite ändern möchten, müssen Sie die eigentliche Seite bearbeiten in der das Steuerelement angezeigt wird. Thats genau, wenn Sie nur ein paar Seiten haben, aber in großer Sites haben schnell ein Site-Management-Albtraum wird.

Ein weiterer Nachteil der Verwendung von Benutzersteuerelementen zur Definition eines allgemeinen Layouts wird in der Architektur von ASP.NET selbst als Stamm. Wenn keine öffentlicher Member eines Steuerelements geändert wird, müssen Sie alle Seiten neu zu kompilieren, die das Benutzersteuerelement verwenden. Im Gegenzug wird ASP.NET auf, und klicken Sie dann nochmals JIT Ihre Seiten, die bei Zugriff auf. Dies erzeugt erneut, eine nicht skalierbare Architektur und ein Site-Management-Problem bei größeren Sites.

Beide Probleme (und viele mehr) sind gut von Masterseiten in ASP.NET 2.0 behoben.

## <a name="how-master-pages-work"></a>Funktionsweise von Masterseiten

Eine Masterseite ist analog zu einer Vorlage für die anderen Seiten. Die Masterseite werden Elemente, die von anderen Seiten (z. B. Menüs, Rahmen, usw.) gemeinsam verwendet werden soll, hinzugefügt. Wenn neue Seiten mit dem Standort hinzugefügt werden, können Sie sie mit einer Masterseite zuordnen. Wird aufgerufen, eine Seite, die mit einer Masterseite zugeordnet ist eine **Inhaltsseite**. Standardmäßig gelangen eine Inhaltsseite auf die Darstellung der Masterseite. Bei der Erstellung einer Masterseite können Sie Teile der Seite definieren, die die Inhaltsseite mit eigenen Inhalt ersetzen können. Diese Teile sind definiert, verwenden ein neues Steuerelement in ASP.NET 2.0 eingeführt wurde. die **ContentPlaceHolder** Steuerelement.

Eine Masterseite kann eine beliebige Anzahl von ContentPlaceHolder-Steuerelemente (oder überhaupt) enthalten. Auf der Seite Inhalt angezeigt wird der Inhalt der ContentPlaceHolder-Steuerelemente innerhalb eines **Inhalt** -Steuerelemente, ein weiteres neues Steuerelement in ASP.NET 2.0. Standardmäßig sind die Inhaltsseiten, die Inhaltssteuerelemente leer, damit Sie Ihre eigenen Inhalte angeben können. Wenn Sie den Inhalt der Masterseite in die Inhaltssteuerelemente verwenden möchten, erreichen Sie so, wie Sie weiter unten in diesem Modul sehen. Das Inhaltssteuerelement wird dem ContentPlaceHolder-Steuerelement über das ContentPlaceHolderID-Attribut des Inhaltssteuerelements zugeordnet. Der Code unten Zuordnungen ein Inhaltssteuerelement an ein ContentPlaceHolder-Steuerelement namens MainBody auf einer Masterseite.

[!code-aspx[Main](master-pages/samples/sample1.aspx)]

> [!NOTE]
> Sie hören oft Personen, die Masterseiten als eine Basisklasse für andere Seiten zu beschreiben. Thats tatsächlich nicht "true". Die Beziehung zwischen der Masterseiten und Inhaltsseiten ist keiner der Vererbung.


**Abbildung 1** eine Gestaltungsvorlage und einer verknüpften Inhaltsseite zeigt, wie sie in Visual Studio 2005 angezeigt werden. Sie sehen das ContentPlaceHolder-Steuerelement in der Masterseite und dem entsprechenden Inhaltssteuerelement in der Seite Inhalt. Beachten Sie, dass der Inhalt von Masterseiten, der außerhalb der ContentPlaceHolder ist sichtbar, sind jedoch abgeblendet ist, auf der Inhaltsseite. Von der Seite Inhalt kann nur der Inhalt innerhalb der ContentPlaceHolder ersetzt werden. Alle anderen Inhalte, der von der Masterseite stammt ist unveränderlich.


![Eine Masterseite und ihre zugeordneten Inhaltsseite](master-pages/_static/image1.jpg)

**Abbildung 1**: eine Masterseite und ihre zugeordneten Inhaltsseite


## <a name="creating-a-master-page"></a>Erstellen einer Masterseite

So erstellen Sie eine neue Masterseite

1. Öffnen Sie Visual Studio 2005 und erstellen Sie eine neue Website.
2. Klicken Sie auf die neue Datei, die Datei.
3. Wählen Sie im Dialogfeld "Neues Element hinzufügen" die Masterdatei, siehe **Abbildung 2:**.
4. Klicken Sie auf Hinzufügen.


![Erstellen einer neuen Master-Seite](master-pages/_static/image2.jpg)

**Abbildung 2**: Erstellen einer neuen Master-Seite


Beachten Sie, dass die Dateierweiterung für eine Masterseite <em>.master</em>. Dies ist eine der Methoden, mit denen eine normale Seite eine Masterseite unterscheidet. Der andere Hauptunterschied besteht darin, die statt einer @Page Direktive, die Masterseite enthält eine @Master Richtlinie. Wechseln Sie zur Quellansicht für den Master Seite, die Sie soeben erstellt haben, und überprüfen Sie den Code.

Standardmäßig müssen eine neue Masterseite ein ContentPlaceHolder-Steuerelement. In den meisten Fällen ist es sinnvoller, erstellen zuerst die allgemeine Elemente der Seite, und klicken Sie dann einfügen ContentPlaceHolder-Steuerelemente, benutzerdefinierter Inhalte erwünscht ist. In diesen Fällen sollten Entwickler das standardmäßige ContentPlaceHolder-Steuerelement löschen und Einfügen neuer Datensätze, wie Sie die Seite entwickelt wird. ContentPlaceHolder-Steuerelemente sind nicht in der Größe veränderbaren trotz der Tatsache, dass sie die Ziehpunkte angezeigt werden. Die Größen von ContentPlaceHolder-Steuerelement automatisch basierend auf den Inhalt, den sie mit einer Ausnahme enthält; Wenn Sie z. B. einer Tabellenzelle ein ContentPlaceHolder-Steuerelement in einem Blockelement ablegen, wird sie gemäß der Größe des Elements Größe.

## <a name="lab-1-working-with-master-pages"></a>Übungseinheit 1 mit Masterseiten arbeiten

In dieser Übungseinheit erhalten Sie eine neue Masterseite zu erstellen und definieren drei ContentPlaceHolder-Steuerelemente. Sie klicken Sie dann eine neue Inhaltsseite erstellen und Ersetzen Sie den Inhalt von mindestens einem der ContentPlaceHolder-Steuerelemente.

1. Erstellen einer Masterseite, und fügen Sie ContentPlaceHolder-Steuerelemente. 

    1. Erstellen Sie eine neue Masterseite, wie oben beschrieben.
    2. Löschen Sie das standardmäßige ContentPlaceHolder-Steuerelement.
    3. Wählen Sie das ContentPlaceHolder-Steuerelement, indem Sie auf den schattierten oberen Rahmen des Steuerelements, und löschen Sie sie durch Drücken der ENTF-Taste auf der Tastatur.
    4. Einfügen einer neuen Tabelle mit den *Header und Seite* Vorlage, wie in Abbildung 3 dargestellt. Ändern Sie die Breite und Höhe auf 90 %, damit die gesamte Tabelle im Designer angezeigt wird.


![](master-pages/_static/image3.jpg)

**Abbildung 3**


1. Platzieren Sie den Cursor in die einzelnen Zellen der Tabelle, und legen Sie die *Valign* Eigenschaft *oben*.
2. Fügen Sie aus der Toolbox ein ContentPlaceHolder-Steuerelement in der obersten Zelle der Tabelle (die Headerzelle.)
3. Wenn Sie dieses ContentPlaceHolder-Steuerelement einfügen, werden Sie feststellen, dass die Zeilenhöhe fast die gesamte Seite gelangen, wie in Abbildung 4 dargestellt. Werden überlegen zu müssen, die an diesem Punkt.


![Der freie Speicherplatz wird in der gleichen Zelle als ContentPlaceHolder](master-pages/_static/image1.gif)

**Abbildung 4**: der freie Speicherplatz wird in der gleichen Zelle als ContentPlaceHolder


1. Platzieren Sie ein ContentPlaceHolder-Steuerelement in den anderen beiden Zellen. Nachdem die anderen ContentPlaceHolder-Steuerelemente eingefügt wurden, sollte die Größe der Tabellenzellen wie erwartet. Die Seite sollte jetzt aussehen, wie im angezeigten Seite **Abbildung 5:**.


![Der Master alle ContentPlaceHolder-Steuerelemente. Beachten Sie, dass die Zellenhöhe für die Headerzelle jetzt ist es liegen](master-pages/_static/image2.gif)

**Abbildung 5**: der Master alle ContentPlaceHolder-Steuerelemente. Beachten Sie, dass die Zellenhöhe für die Headerzelle jetzt ist es liegen


1. Geben Sie Text Ihrer Wahl in jedem der drei ContentPlaceHolder-Steuerelemente.
2. Speichern Sie die Masterseite als exercise1.master an.
3. Erstellen Sie ein neues Webformular, und ordnen sie die Masterseite exercise1.master.
4. Wählen Sie die neue Datei, die Datei in Visual Studio 2005.
5. Wählen Sie **Webformular** in das Dialogfeld "Neues Element hinzufügen".
6. Stellen Sie sicher, dass die Masterseite auswählen das Kontrollkästchen aktiviert ist, wie in Abbildung 6 dargestellt.


![Eine neue Seite hinzufügen](master-pages/_static/image3.gif)

**Abbildung 6**: eine neue Seite hinzufügen


1. Klicken Sie auf Hinzufügen.
2. Wählen Sie Dialogfeld aus "" exercise1.master in die SELECT-Anweisung eine Masterseite wie in Abbildung 7 dargestellt.
3. Klicken Sie auf OK, um die neue Seite hinzuzufügen.

Die neue Seite, die mit einem Content-Steuerelement für die einzelnen ContentPlaceHolder-Steuerelemente auf der Masterseite in Visual Studio angezeigt werden. Standardmäßig sind die Inhaltssteuerelemente leer, damit Sie Ihre eigene Inhalte hinzufügen können. Wenn Sie den Inhalt aus dem ContentPlaceHolder-Steuerelement auf der Masterseite verwenden eine zufrieden sind, einfach klicken Sie auf das Smarttag-Symbol (den kleinen schwarzen Pfeil in der oberen rechten Ecke des Steuerelements), und wählen Sie *Master-Inhalt standardmäßig* aus dem Smarttag Siehe **Abbildung 8:**. Wenn Sie dies tun, ändert sich das Menüelement in *erstellen benutzerdefinierte Content*. An diesem Punkt klicken, wird der Inhalt der Masterseite, sodass Sie zum Definieren von benutzerdefinierten Inhalts für diese bestimmte Inhaltssteuerelement.


![Der Inhalt der Master-Seiten standardmäßig ein ContentControl-Element festlegen](master-pages/_static/image4.gif)

**Abbildung 7**: Festlegen eines Inhaltssteuerelements standardmäßig den Inhalt der Master-Seiten


## <a name="connecting-master-page-and-content-pages"></a>Herstellen einer Verbindung Masterseite und Inhaltsseiten

Die Zuordnung zwischen einer Masterseite und einer Inhaltsseite kann in einem der vier verschiedene Arten konfiguriert werden:

- Die <strong>MasterPageFile</strong> Attribut der @Page Richtlinie
- Festlegen der **Page.MasterPageFile** Eigenschaft im Code.
- Die **&lt;Seiten&gt;** Element in der Konfigurationsdatei "Anwendungen" ("Web.config" im Stammordner der Anwendung)
- Die **&lt;Seiten&gt;** Element in einem Unterordner-Konfigurationsdatei ("Web.config" in einem Unterordner)

## <a name="masterpagefile-attribute"></a>MasterPageFile-Attribut

Die MasterPageFile-Attribut erleichtert es, eine bestimmte ASP.NET-Seite eine Masterseite zuweisen. Es ist auch die Methode verwendet, um die Masterseite anzuwenden, wenn Sie überprüfen die **Masterseite auswählen** Kontrollkästchen, wie Sie in Übung 1 hat.

## <a name="setting-pagemasterpagefile-in-code"></a>Festlegen von Page.MasterPageFile im Code

Wenn Sie die MasterPageFile-Eigenschaft im Code festlegen, können Sie eine bestimmte Masterseite auf Ihre Inhalte zur Laufzeit anwenden. Dies ist hilfreich in Fällen, in denen Sie möglicherweise eine bestimmte Masterseite, die basierend auf einer Benutzerrolle oder anderen Kriterien anwenden müssen. Die MasterPageFile-Eigenschaft muss in der PreInit-Methode festgelegt werden. Wenn sie nach der PreInit-Methode festgelegt wird, wird eine "InvalidOperationException" ausgelöst. Die Seite, auf dem diese Eigenschaft festgelegt wird, muss auch einen Inhalt aufweisen Steuerelement als das Steuerelement der obersten Ebene für die Seite. Andernfalls wird ein HttpException nastavena die MasterPageFile-Eigenschaft ausgelöst.

## <a name="using-the-ltpagesgt-element"></a>Mithilfe der &lt;Seiten&gt; Element

Sie können eine Masterseite für Ihre Seiten konfigurieren, indem Sie die MasterPageFile-Attribut der &lt;Seiten&gt; -Element der Datei "Web.config". Wenn Sie diese Methode verwenden zu können, Bedenken Sie, dass die web.config-Dateien in der Anwendungsstruktur für diese Einstellung außer Kraft setzen können. Legen Sie in jedem MasterPageFile-Attribut eine @Page Richtlinie wird diese Einstellung auch überschreiben. Mithilfe der &lt;Seiten&gt; Element können sie ganz einfach erstellen eine <em>master</em> Masterseite, die ggf. in bestimmten Ordnern oder Dateien überschrieben werden kann.

## <a name="properties-in-master-pages"></a>Eigenschaften in der Master-Seiten

Eine Masterseite kann Eigenschaften verfügbar machen, einfach, indem diese Eigenschaften innerhalb der Masterseite öffentlich. Der folgende Code definiert z. B. eine Eigenschaft namens SomeProperty:

[!code-csharp[Main](master-pages/samples/sample2.cs)]

Zugriff auf die SomeProperty-Eigenschaft auf der Seite Inhalt müssen Sie mit der Master Eigenschaft wie folgt:

[!code-csharp[Main](master-pages/samples/sample3.cs)]

## <a name="nesting-master-pages"></a>Schachtelung von Masterseiten

Masterseiten sind die ideale Lösung zum Sicherstellen einer konsistenten Aussehens über eine umfangreiche Webanwendung. Allerdings ist es nicht ungewöhnlich, dass bestimmte Teile einer großen Site Freigabe eine gemeinsame Schnittstelle, während andere Teile eine andere Schnittstelle verwendet. Zu diesem Zweck werden mehrere Masterseiten die ideale Lösung. Das Problem weiterhin berücksichtigt nicht jedoch die Tatsache, dass es sich bei eine große Anwendung möglicherweise bestimmte Komponenten (z. B. ein Menü, z. B.), die von allen Seiten gemeinsam verwendet werden und andere Komponenten, die nur für bestimmte Abschnitte des Standorts freigegeben werden. Geschachtelte Masterseiten geben müssen für diese Situation gut aus. Wie Sie gesehen haben, besteht aus eine normalen Masterseite von einer Masterseite und einer Inhaltsseite ein. Es gibt zwei Masterseiten, in einer geschachtelten Masterseite Situation; ein Master für die übergeordneten und untergeordneten Master. Die untergeordnete Masterseite ist auch eine Inhaltsseite, und der Master ist die übergeordnete Masterseite.

Hier ist der Code für eine typische Masterseite:

[!code-aspx[Main](master-pages/samples/sample4.aspx)]

In einem Szenario mit geschachtelten master wäre dies die übergeordnete Master. Einer anderen Masterseite würde als die Masterseite mithilfe dieser Seite, und dieser Code sieht wie folgt:

[!code-aspx[Main](master-pages/samples/sample5.aspx)]

Beachten Sie, dass in diesem Szenario der untergeordneten Master auch einer Inhaltsseite für den übergeordneten Master. Alle untergeordneten Masterinhalt angezeigt innerhalb eines Inhaltssteuerelements, das den Inhalt von ContentPlaceHolder-Steuerelement des übergeordneten Elements abruft.

> [!NOTE]
> Designer-Unterstützung ist nicht verfügbar, für die geschachtelte Masterseiten. Beim Verwenden von geschachtelten Master entwickeln, müssen Sie die Datenquellensicht zu verwenden.


Dieses Video zeigt eine exemplarische Vorgehensweise zur Verwendung von geschachtelten Masterseiten.


![](master-pages/_static/image1.png)


[Open Vollbild-Video](master-pages/_static/nested1.wmv)


![Eine Masterseite auswählen](master-pages/_static/image4.jpg)

**Abbildung 8**: eine Masterseite auswählen
