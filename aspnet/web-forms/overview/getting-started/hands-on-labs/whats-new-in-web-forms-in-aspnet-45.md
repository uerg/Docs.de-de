---
uid: web-forms/overview/getting-started/hands-on-labs/whats-new-in-web-forms-in-aspnet-45
title: Neuigkeiten in Web Forms in ASP.NET 4.5 | Microsoft-Dokumentation
author: rick-anderson
description: Die neue Version von ASP.NET Web Forms führt eine Reihe von Verbesserungen umfassen hauptsächlich die Verbesserung der benutzerfreundlichkeit beim Arbeiten mit Daten. In früheren Versionen von...
ms.author: aspnetcontent
ms.date: 02/18/2013
ms.assetid: 0a1f88bd-97da-4ed1-86f1-605199dc75a4
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/whats-new-in-web-forms-in-aspnet-45
msc.type: authoredcontent
ms.openlocfilehash: f36c50b64ed2363ba648297a1424b638bf9d4af5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830411"
---
<a name="whats-new-in-web-forms-in-aspnet-45"></a>Neuerungen in Web Forms in ASP.NET 4.5
====================
durch [Web Camps Team](https://twitter.com/webcamps)

> Die neue Version von ASP.NET Web Forms führt eine Reihe von Verbesserungen umfassen hauptsächlich die Verbesserung der benutzerfreundlichkeit beim Arbeiten mit Daten.
> 
> In früheren Versionen von Web Forms, wenn die Datenbindung zu verwenden, um den Wert eines Objektmitglieds ausgeben müssen Sie die Datenbindungsausdrücke Bind() oder Eval() verwendet. In der neuen Version von ASP.NET sind Sie deklarieren, welche Art von Daten ein Steuerelements durch Verwendung einer neuen ItemType-Eigenschaft gebunden werden soll. Durch Festlegen dieser Eigenschaft können Sie eine stark typisierte Variable verwenden, um die Vorteile der Visual Studio-Entwicklungsoberfläche, z. B. IntelliSense, Navigation durch Member und kompilierzeitüberprüfung zu empfangen.
> 
> Mit den datengebundenen Steuerelementen können Sie jetzt auch eigene benutzerdefinierten Methoden für die Sie auswählen, aktualisieren, löschen und Einfügen von Daten, angeben die Interaktion zwischen der Steuerelemente der Seite und die Anwendungslogik vereinfachen. Darüber hinaus wurden Funktionen des Modell-Bindung an ASP.NET, hinzugefügt Dies bedeutet, dass Sie Daten von der Seite direkt in den Typ der Methodenparameter zuordnen können.
> 
> Überprüfen der Benutzereingabe sollte auch mit der neuesten Version von Web Forms einfacher sein. Sie können jetzt Ihren Modellklassen mit Validierungsattributen aus versehen die **System.ComponentModel.DataAnnotations** -Namespace und die Anforderung, die steuert, alle Ihre Website Überprüfen der Benutzereingabe mit diesen Informationen. Die clientseitige Validierung in Web Forms ist jetzt in jQuery, Bereitstellen von saubereren der clientseitige Code und unaufdringliches JavaScript-Funktionen integriert.
> 
> Im Bereich Anforderung Überprüfung wurden Verbesserungen vorgenommen, können Sie selektiv Anforderungsvalidierung für bestimmte Teile Ihrer Anwendungen deaktivieren, oder lesen die für ungültig erklärten Anforderungsdaten zu erleichtern.
> 
> Einige für Web Forms-Steuerelementen zu neuen Funktionen von HTML5 nutzen wurden Verbesserungen vorgenommen:
> 
> - Die TextMode-Eigenschaft der TextBox-Steuerelement wurde aktualisiert, um die neuen HTML5-Eingabetypen wie e-Mail, "DateTime" und So weiter unterstützen.
> - Das Steuerelement "FileUpload" unterstützt jetzt mehrere Dateiuploads in Browsern, die mit Unterstützung für diese HTML5-Funktion.
> - Validierungssteuerelement steuert nun Unterstützung überprüfen HTML5 input-Elemente.
> - Neuer HTML5-Elemente mit Attributen, die eine URL jetzt darstellen unterstützen Runat =&quot;Server&quot;. Daher können Sie ASP.NET Konventionen in der URL-Pfade, z. B. der ~-Operator zum Stammverzeichnis der Anwendung darstellen (z. B. &lt;video Runat =&quot;Server&quot; Src =&quot;~/myVideo.wmv&quot; &gt; &lt;/video&gt;).
> - Das UpdatePanel-Steuerelement wurde behoben, um Beiträge HTML5 Eingabefelder zu unterstützen.
> 
> In der offiziellen ASP.NET-Portal finden Sie weitere Beispiele für die neuen Funktionen in ASP.NET WebForms 4.5: [Neuigkeiten in ASP.NET 4.5 und Visual Studio 2012](../../../../whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012.md#_Toc318097385)
> 
> Alle Beispielcode und Ausschnitte sind im Web Camps Training Kit unter enthalten [ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).


<a id="Objectives"></a>
### <a name="objectives"></a>Ziele

In dieser praktischen Übungseinheit erfahren Sie, wie Sie:

- Verwenden von Ausdrücken für stark typisierte Datenbindung
- Verwenden Sie neue Features der Modell-Bindung in Web Forms
- Verwenden von Wertanbietern für die Zuordnung von Daten der Seite auf Code-Behind-Methoden
- Verwenden von Datenanmerkungen für die Validierung von Benutzereingaben
- Nutzen Sie Advange Unobstrusive die clientseitige Validierung mit jQuery in Web Forms
- Differenzierte Anforderungsvalidierung implementieren
- Implementieren der asynchronen seitenverarbeitung in Web Forms

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Erforderliche Komponenten

Sie benötigen Folgendes, um diese testumgebung abzuschließen:

- [Microsoft Visual Studio Express 2012 für Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) oder sogar eine höhere (Lesen [Anhang A](#AppendixA) Anleitungen zur Installation).

<a id="Setup"></a>
### <a name="setup"></a>Setup

**Installieren von Codeausschnitten**

Der Einfachheit halber ist Großteil des Codes, die entlang dieser Übungseinheit verwaltet werden soll als Codeausschnitte für Visual Studio verfügbar. So installieren Sie die Codeausschnitte ausführen **.\Source\Setup\CodeSnippets.vsi** Datei.

Wenn Sie nicht mit dem Visual Studio Code Snippets und zu erfahren, wie Sie deren Verwendung vertraut sind, sehen Sie sich im Anhang in diesem Dokument &quot; [Anhang C: Verwenden von Code Snippets](#AppendixC)&quot;.

<a id="Exercises"></a>
## <a name="exercises"></a>Übungen

Dieser praktischen Übungseinheit enthält die folgenden Übungen:

1. [Übung 1: Modellbindung in ASP.NET Web Forms](#Exercise1)
2. [Übung 2: Die Datenüberprüfung](#Exercise2)
3. [Übung 3: Asynchrone Seitenverarbeitung in der ASP.NET Web Forms](#Exercise3)

> [!NOTE]
> Jede Übung umfasst eine **End** Ordner mit der resultierenden Lösung, die Sie nach Abschluss der Übungen abrufen soll. Sie können diese Lösung als Leitfaden verwenden, bei Bedarf zusätzliche Hilfe bei der die Übungen durcharbeiten.


Geschätzte Zeit für diese testumgebung abzuschließen: **60 Minuten**.

<a id="Exercise1"></a>

<a id="Exercise_1_Model_Binding_in_ASPNET_Web_Forms"></a>
### <a name="exercise-1-model-binding-in-aspnet-web-forms"></a>Übung 1: Modellbindung in ASP.NET Web Forms

Die neue Version von ASP.NET Web Forms führt eine Reihe von Verbesserungen umfassen hauptsächlich die Verbesserung der benutzerfreundlichkeit, beim Arbeiten mit Daten. Sie werden in dieser Übung erfahren Sie mehr über stark typisierte Datensteuerelemente und modellbindung.

<a id="Task_1_-_Using_Strongly-Typed_Data-Bindings"></a>
#### <a name="task-1---using-strongly-typed-data-bindings"></a>Aufgabe 1: Verwenden eines stark typisierten Datenbindungen

In dieser Aufgabe werden Sie die neuen stark typisierte Bindungen in ASP.NET 4.5 verfügbaren feststellen.

1. Öffnen der **beginnen** Lösung controllerarbeitsverzeichnis **Quelle/Ex1-ModelBinding/Anfang/** Ordner.

   1. Sie müssen einige fehlende NuGet-Pakete herunterladen. bevor Sie fortfahren. Zu diesem Zweck klicken Sie auf die **Projekt** Menü **NuGet-Pakete verwalten**.
   2. In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.
   3. Abschließend erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.

      > [!NOTE]
      > Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken in Ihrem Projekt, Versand Verringern der Projektgröße. Mit NuGet Power Tools können werden durch Angabe von Versionen des Pakets in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, die, das Sie das Projekt ausführen, können. Deshalb müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit wird.
2. Öffnen der **Customers.aspx** Seite. Fügen Sie eine nicht nummerierte Liste in das Hauptsteuerelement und enthalten Sie ein Repeater-Steuerelement in für die Auflistung von einzelnen Kunden. Legen Sie den Namen der Repeater **CustomersRepeater** wie im folgenden Code gezeigt.

    In früheren Versionen von Web Forms Wenn die Datenbindung zu verwenden, um den Wert eines Elements für ein Objekt ausgeben, sind Sie Datenbindung, verwenden Sie eine XPath-Datenbindungsausdruck, zusammen mit einem Aufruf an die Eval-Methode übergibt den Namen der Member als Zeichenfolge.

    Zur Laufzeit werden diese Aufrufe Eval mithilfe von Reflektion für das derzeit gebundene Objekt um den Wert des Elements mit dem angegebenen Namen zu lesen, und das Ergebnis im HTML-Code angezeigt. Dieser Ansatz ist es sehr einfach, Daten für beliebige, unshaped Daten gebunden werden soll.

    Leider verlieren Sie viele hervorragende entwicklungserfahrung Features in Visual Studio, einschließlich IntelliSense für Elementnamen, die Unterstützung für die Navigation (z. B. "Gehe zu Definition), und Überprüfen während der Kompilierung.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample1.aspx)]
3. Öffnen der **Customers.aspx.cs** Datei.
4. Fügen Sie die folgenden using-Anweisung.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample2.cs)]
5. In der **Seite\_Load** -Methode, fügen Sie Code zum Auffüllen des Repeaters mit der Liste der Kunden.

    (Codeausschnitt - *Web Forms Lab - Ex01 - Bindung für Kunden-Datenquelle*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample3.cs)]

    Die Lösung nutzt EntityFramework zusammen mit CodeFirst erstellen und auf die Datenbank zugreifen. In den folgenden Code ist der CustomersRepeater auf eine materialisierte Abfrage gebunden, die alle Kunden aus der Datenbank zurückgibt.
6. Drücken Sie **F5** , führen Sie die Projektmappe, und fahren mit der **Kunden** um Repeater in Aktion anzuzeigen. Die Lösung CodeFirst verwendet wird, wird die Datenbank erstellt und in Ihrer lokalen SQL Express-Instanz aufgefüllt wird, wenn die Anwendung ausgeführt werden.

    ![Auflisten der Kunden mit einem wiederholungssteuerelement](whats-new-in-web-forms-in-aspnet-45/_static/image1.png "Codebeispiel die Kunden mit einem wiederholungssteuerelement")

    *Auflisten der Kunden mit einem wiederholungssteuerelement*

    > [!NOTE]
    > In Visual Studio 2012 ist IIS Express der Standardwebserver für die Entwicklung.
7. Schließen Sie den Browser, und wechseln Sie zurück zu Visual Studio.
8. Ersetzen Sie nun die Implementierung, um stark typisierte Bindungen verwenden. Öffnen der **Customers.aspx** Seite und verwenden Sie die neue **ItemType** Attribut im wiederholungsmodul Festlegen der **Kunden** den Typ der Bindung.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample4.aspx)]

    Die ItemType-Eigenschaft können Sie deklarieren, welche Art von Daten das Steuerelement gebunden werden soll und können Sie mit fester typbindung auf das datengebundene Steuerelement binden.
9. Ersetzen Sie die ItemTemplate-Inhalten mit den folgenden Code.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample5.aspx)]

    Ein Nachteil der oben genannten Methoden ist, dass die Aufrufe von Eval() und Bind() spät gebunden – was bedeutet, dass das Übergeben von Zeichenfolgen, um die Eigenschaftennamen darstellen. Dies bedeutet, dass Sie keine Intellisense für die Elementnamen, Unterstützung für die Navigation in Code (z. B. "Gehe zu Definition), noch während der Kompilierung überprüfen Unterstützung erhalten.

    Festlegen der ItemType-Eigenschaft führt zwei neue typisierte Variablen im Rahmen der Datenbindungsausdrücke generiert werden soll: **Element** und **BindItem**. Sie können verwenden Sie diese stark typisierten Variablen in den Ausdrücken für die Datenbindung und erhalten alle Vorteile der Visual Studio-Entwicklungsoberfläche.

    Die &quot; **:** &quot; im Ausdruck verwendeten wird automatisch ein HTML-Codierung die Ausgabe, um Sicherheitsprobleme (z. B. Cross-Site scripting-Angriffe) zu vermeiden. Diese Notation verfügbar seit .NET 4 für die Antwort geschrieben wurde, aber es ist jetzt auch in Datenbindungsausdrücken verfügbar.

    > [!NOTE]
    > Das Element-Element funktioniert für unidirektionale Bindung. Wenn Sie bidirektionale Bindung verwenden möchten. die **BindItem** Member.

    ![IntelliSense-Unterstützung in stark typisierte Bindung](whats-new-in-web-forms-in-aspnet-45/_static/image2.png "IntelliSense-Unterstützung in stark typisierte Bindung")

    *IntelliSense-Unterstützung in stark typisierte Bindung*
10. Drücken Sie **F5** , führen Sie die Projektmappe, und wechseln zu der Seite "Kunden", um sicherzustellen, dass die Änderungen wie erwartet funktionieren.

    ![Auflisten von Kundendetails](whats-new-in-web-forms-in-aspnet-45/_static/image3.png "Kundendetails auflisten")

    *Auflisten von Kundendetails*
11. Schließen Sie den Browser, und wechseln Sie zurück zu Visual Studio.

<a id="Task_2_-_Introducing_Model_Binding_in_Web_Forms"></a>
#### <a name="task-2---introducing-model-binding-in-web-forms"></a>Aufgabe 2: Einführung in Modellbindung in Web Forms

In früheren Versionen von ASP.NET Web Forms Wenn Sie bidirektionale Datenbindung, ausführen, sowohl abrufen und Aktualisieren von Daten möchten, mussten Sie ein Objekt für die Datenquelle verwendet wird. Dies kann einer Objektdatenquelle, die eine SQL-Datenquelle, die eine LINQ-Datenquelle usw. sein. Jedoch wenn Ihr Szenario benutzerdefinierten Code erforderlich, für die Verarbeitung der das, mussten Sie die Objekt-Datenquelle verwenden, und dadurch wurden einige Nachteile. Beispielsweise mussten Sie komplexe Typen zu vermeiden, und Sie benötigen, um Ausnahmen zu behandeln, beim Ausführen von Validierungslogik.

In der neuen Version von ASP.NET Web Forms unterstützt die von datengebundenen Steuerelementen modellbindung. Dies bedeutet, dass Sie Option angeben, zu aktualisieren, einfügen und löschen können Methoden direkt in die vom datengebundenen Steuerelement Logik aus der CodeBehind-Datei oder von einer anderen Klasse aufgerufen.

Informationen hierzu, werden Sie einer GridView-Ansicht verwenden, um die Produktkategorien, die mit der neuen Liste **SelectMethod** Attribut. Mit diesem Attribut können Sie eine Methode zum Abrufen der GridView-Daten angeben.

1. Öffnen der **beispielsweise "Products.aspx"** Seite eine **GridView**. Konfigurieren Sie die GridView, wie unten gezeigt, um stark typisierte Bindungen verwenden und Sortieren und paging zu ermöglichen.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample6.aspx)]
2. Verwenden Sie die neue **SelectMethod** Attribut so konfigurieren Sie die GridView zum Aufrufen einer **GetCategories** Methode, um die Daten auszuwählen.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample7.aspx)]
3. Öffnen der **Products.aspx.cs** CodeBehind-Datei, und fügen Sie die folgenden using-Anweisungen.

    (Codeausschnitt - *Web Forms Lab - Ex01 - Namespaces*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample8.cs)]
4. Fügen Sie einen privaten Member in der **Produkte** Klasse, und weisen Sie eine neue Instanz der **ProductsContext**. Diese Eigenschaft speichert die Entity Framework-Datenkontext, der Sie mit der Datenbank herstellen können.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample9.cs)]
5. Erstellen Sie eine **GetCategories** Methode zum Abrufen der Liste der Kategorien, die unter Verwendung von LINQ. Die Abfrage enthält die **Produkte** Eigenschaft, sodass die GridView, die Menge der Produkte in jeder Kategorie anzeigen kann. Beachten Sie, dass eine unformatierte "IQueryable"-Objekt an, die darstellen, die Abfrage kann später im Lebenszyklus der Seite ausgeführt, gibt die Methode zurück.

    (Codeausschnitt - *Web Forms Lab - Ex01 - GetCategories*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample10.cs)]

    > [!NOTE]
    > In früheren Versionen von ASP.NET Web Forms erforderlich, aktivieren sortieren und paging mit Ihrer eigenen Logik Repository im Kontext einer Objektdatenquelle, um Ihre eigenen benutzerdefinierten Code schreiben, und erhalten alle erforderlichen Parameter. Jetzt aber, wie die Datenbindung-Methoden können "IQueryable" zurückgeben, und dies eine Abfrage stellt immer noch um ausgeführt werden, ASP.NET kann dann ändern die Abfrage die richtige Sortierung hinzufügen und die Paging-Parameter.
6. Drücken Sie **F5** Debuggen Sie den Standort aus, und wechseln zu der Seite "Produkte". Sollte angezeigt werden, dass die GridView mit den Kategorien, die von der GetCategories-Methode zurückgegebene aufgefüllt wird.

    ![Auffüllen einer GridView-Ansicht mit der modellbindung](whats-new-in-web-forms-in-aspnet-45/_static/image4.png "Auffüllen einer GridView-Ansicht mit der modellbindung")

    *Auffüllen einer GridView-Ansicht mit der modellbindung*
7. Drücken Sie **UMSCHALT**+**F5** Beenden des Debuggens.

<a id="Task_3_-_Value_Providers_in_Model_Binding"></a>
#### <a name="task-3---value-providers-in-model-binding"></a>Aufgabe 3: Wertanbieter in der Modellbindung

Modellbindung nicht nur ermöglicht Ihnen die Angabe von benutzerdefinierten Methoden zum Arbeiten mit Ihren Daten direkt in das datengebundene Steuerelement, sondern ermöglicht auch das Zuordnen von Daten von der Seite in Parameter dieser Methoden. Auf der Methodenparameter können Sie Anbieter Attributen zur Angabe des Werts der Datenquelle. Zum Beispiel:

- Steuerelemente auf der Seite
- Abfragezeichenfolgen-Werte
- Anzeigen von Daten
- Sitzungszustand
- Cookies
- Gesendeten Formulardaten
- Ansichtszustand
- Benutzerdefinierte Wertanbieter werden ebenfalls unterstützt.

Wenn Sie ASP.NET MVC 4 verwendet haben, werden Sie feststellen, dass die bindungsunterstützung für das Modell ähnlich ist. In der Tat diese Features von ASP.NET MVC erstellt und in verschoben wurden die **"System.Web"** Assembly können sie auch in Web Forms verwendet.

In dieser Aufgabe aktualisieren Sie die GridView zum Filtern der Ergebnisse durch die Menge der Produkte in jeder Kategorie, empfangen den Filter-Parameter mit modellbindung.

1. Wechseln Sie zurück zu den **beispielsweise "Products.aspx"** Seite.
2. Fügen Sie am Anfang der GridView, eine **Bezeichnung** und **"ComboBox"** die Anzahl der Produkte für die einzelnen Kategorien auswählen, wie unten dargestellt.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample11.aspx)]
3. Hinzufügen einer **EmptyDataTemplate** an die GridView, die eine Meldung angezeigt, wenn es keine Kategorien mit der ausgewählten Anzahl von Produkten gibt.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample12.aspx)]
4. Öffnen der **Products.aspx.cs** Code-Behind, und fügen Sie die folgenden using-Anweisung.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample13.cs)]
5. Ändern der **GetCategories** Methode, um eine ganze Zahl empfangen **MinProductsCount** Argument und die zurückgegebenen Ergebnisse zu filtern. Zu diesem Zweck ersetzen Sie die Methode mit dem folgenden Code ein.

    (Codeausschnitt - *Web Forms Lab - Ex01 - GetCategories 2*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample14.cs)]

    Die neue **[STRG]** -Attribut für die **MinProductsCount** Argument ASP.NET wissen der Wert muss ein Steuerelement auf der Seite mit aufgefüllt werden können. ASP.NET wird suchen Sie nach jedem Steuerelement mit dem Namen des Arguments (MinProductsCount) übereinstimmt, und führen Sie die erforderliche Zuordnung und die Konvertierung in den Parameter mit dem Steuerelementwert zu füllen.

    Das Attribut stellt auch einen überladenen Konstruktor, der Ihnen ermöglicht, geben Sie das Steuerelement aus, wo Sie den Wert zu erhalten.

    > [!NOTE]
    > Eines der Ziele von den Datenbindungsfunktionen ist die Menge des Codes zu reduzieren, die für die Interaktion von Seite geschrieben werden muss. Abgesehen von den Wertanbieter [STRG] können Sie andere modellbindungs-Anbieter in Ihrer Methodenparameter. Einige davon werden in der Einführung der Aufgabe aufgeführt.
6. Drücken Sie **F5** Debuggen Sie den Standort aus, und wechseln zu der Seite "Produkte". Wählen Sie eine Reihe von Produkten in der Dropdown-Liste, und beachten Sie, wie GridView jetzt aktualisiert wird.

    ![Filtern der GridView mit dem Wert des Dropdown-Listenfeld](whats-new-in-web-forms-in-aspnet-45/_static/image5.png "GridView mit dem Wert des Dropdown-Liste filtern")

    *Filtern der GridView mit dem Wert des Dropdown-Liste*
7. Beenden des Debuggens.

<a id="Task_4_-_Using_Model_Binding_for_Filtering"></a>
#### <a name="task-4---using-model-binding-for-filtering"></a>Aufgabe 4: mit Modellbindung für die Filterung

In dieser Aufgabe fügen Sie ein zweites, untergeordneten GridView, die Produkte in der ausgewählten Kategorie angezeigt.

1. Öffnen der **beispielsweise "Products.aspx"** Seite, und Aktualisieren der Kategorien GridView die auswählen-Schaltfläche automatisch generiert.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample15.aspx)]
2. Fügen Sie eine zweite **GridView** mit dem Namen **ProductsGrid** am unteren Rand. Legen Sie die **ItemType** zu **WebFormsLab.Model.Product**, wird die **DataKeyNames** zu **"ProductID"** und die **SelectMethod**  zu **GetProducts**. Legen Sie **AutoGenerateColumns** zu **"false"** und fügen Sie die Spalten ProductId, ProductName, Beschreibung und UnitPrice.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample16.aspx)]
3. Öffnen der **Products.aspx.cs** Code-Behind-Datei. Implementieren der **GetProducts** Methode erhalten die Kategorie-ID aus der Kategorie "GridView" und die Produkte zu filtern. Modellbindung wird legen Sie den Wert des Parameters verwenden die ausgewählte Zeile in der **CategoriesGrid**. Da die Argumentnamen und Steuerelementname nicht übereinstimmen, sollten Sie den Namen des Steuerelements im Steuerelement Wertanbieter angeben.

    (Codeausschnitt - *Web Forms Lab - Ex01 - GetProducts*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample17.cs)]

    > [!NOTE]
    > Dieser Ansatz einfacher Komponententest testen diese Methoden. Auf einem Unit Test-Kontext, in denen Web Forms nicht ausgeführt wird, wird das Attribut [STRG] keine bestimmte Aktion ausgeführt.
4. Öffnen der **beispielsweise "Products.aspx"** Seite, und suchen Sie die GridView-Produkte. Aktualisieren Sie die Produkte GridView, um einen Link zum Bearbeiten des ausgewählten Produkts anzuzeigen.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample18.aspx)]
5. Öffnen der **ProductDetails.aspx** Code-Behind-Seite, und Ersetzen Sie die **SelectProduct** Methode durch den folgenden Code.

    (Codeausschnitt - *Forms Lab - Ex01 - SelectProduct-Webmethode*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample19.cs)]

    > [!NOTE]
    > Beachten Sie, dass die **[QueryString]** Attribut wird verwendet, um die Parameter der Methode aus einem "ProductID"-Parameter in der Abfragezeichenfolge zu füllen.
6. Drücken Sie **F5** Debuggen Sie den Standort aus, und wechseln zu der Seite "Produkte". Wählen Sie eine Kategorie aus den Kategorien GridView, und beachten Sie, dass die GridView-Produkte wird aktualisiert.

    ![Die Produkte aus der ausgewählten Kategorie](whats-new-in-web-forms-in-aspnet-45/_static/image6.png "die Produkte aus der ausgewählten Kategorie")

    *Die Produkte aus der ausgewählten Kategorie*
7. Klicken Sie auf die **Ansicht** Link zu einem Produkt auf die Seite "ProductDetails.aspx" zu öffnen.

    Beachten Sie, dass die Seite das Produkt mit die SelectMethod, mit dem Parameter "ProductID" aus der Abfragezeichenfolge abruft.

    ![Anzeigen der Produktdetails](whats-new-in-web-forms-in-aspnet-45/_static/image7.png "Anzeigen der Produktdetails")

    *Anzeigen der Produktdetails*

    > [!NOTE]
    > Die Möglichkeit, geben Sie eine HTML-Beschreibung wird in der nächsten Übung implementiert werden.

<a id="Task_5_-_Using_Model_Binding_for_Update_Operations"></a>
#### <a name="task-5---using-model-binding-for-update-operations"></a>Aufgabe 5: mit Modellbindung für Updatevorgänge

Klicken Sie in der vorherigen Aufgabe haben Sie haben die modellbindung verwendet, hauptsächlich zum Auswählen von Daten, in dieser Aufgabe erfahren Sie, wie modellbindung in Update-Vorgänge verwendet.

Aktualisieren Sie die Kategorien GridView ermöglichen den Benutzer, die Kategorien zu aktualisieren.

1. Öffnen der **beispielsweise "Products.aspx"** Seite, und Aktualisieren der Kategorien GridView, auf die Schaltfläche "Bearbeiten" automatisch generieren und verwenden Sie die neue **UpdateMethod** -Attribut zum Angeben einer **UpdateCategory**Methode, um das ausgewählte Element zu aktualisieren.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample20.aspx)]

    Das DataKeyNames-Attribut in der GridView-Ansicht zu definieren, die die Elemente sind, die das Modell gebundenen Objekt eindeutig identifiziert, und daher die sind die Parameter, die mindestens die Update-Methode erhalten soll.
2. Öffnen der **Products.aspx.cs** Code-Behind-Datei und Implementieren der **UpdateCategory** Methode. Die Methode sollte die Kategorie-ID, und laden die aktuelle Kategorie, die Werte aus der GridView zu füllen und aktualisieren Sie dann die Kategorie angezeigt.

    (Codeausschnitt - *Web Forms Lab - Ex01 - UpdateCategory*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample21.cs)]

    Die neue **TryUpdateModel** -Methode in der Page-Klasse ist zuständig für das Auffüllen der Model-Objekts unter Verwendung der Werte aus den Steuerelementen auf der Seite. In diesem Fall ersetzt ihn die aktualisierten Werte aus der aktuellen GridView-Zeile, die bearbeitet wird, in der **Kategorie** Objekt.

    > [!NOTE]
    > Die nächste Übung wird die Verwendung der ModelState.IsValid zum Überprüfen der Daten, die vom Benutzer eingegeben wird, wenn das Objekt bearbeiten erläutert.
3. Führen Sie die Website, und wechseln Sie zu der Seite "Produkte". Bearbeiten einer Kategorie an. Geben Sie einen neuen Namen ein, und klicken Sie dann auf **Update** , die Änderungen beizubehalten.

    ![Bearbeiten von Kategorien](whats-new-in-web-forms-in-aspnet-45/_static/image8.png "Bearbeiten von Kategorien")

    *Bearbeiten von Kategorien*

<a id="Exercise2"></a>

<a id="Exercise_2_Data_Validation"></a>
### <a name="exercise-2-data-validation"></a>Übung 2: Die Datenüberprüfung

In dieser Übung erfahren Sie mehr über die neue Datenfunktionen für die Validierung in ASP.NET 4.5. Sie werden die neuen Funktionen der unaufdringlichen Validierung in Web Forms Auschecken. Sie können datenanmerkungen in die Anwendung Modellklassen für die Validierung von Benutzereingaben und schließlich erfahren Sie, wie Sie zum Aktivieren oder Deaktivieren der Anforderungsvalidierung in einzelne Steuerelemente auf einer Seite.

<a id="Task_1_-_Unobtrusive_Validation"></a>
#### <a name="task-1---unobtrusive-validation"></a>Aufgabe 1: unaufdringliche Validierung

Formulare mit komplexen Daten, einschließlich der Validierungssteuerelemente tendenziell zu viele JavaScript-Code auf der Seite zu generieren, die CA. 60 % des Codes darstellen kann. Unaufdringliche Validierung aktiviert wird Ihre HTML-Code übersichtlicher und ordentlichere aussehen.

In diesem Abschnitt können Sie die unaufdringliche Validierung in ASP.NET für den HTML-Code, die von beiden Konfigurationen generierten verglichen werden soll.

1. Öffnen Sie **Visual Studio 2012** , und öffnen Sie die **beginnen** Lösung befindet sich in der **Source\Ex2-Validation\Begin** Ordner dieser Anleitung. Alternativ können Sie arbeiten in Ihre vorhandene Lösung aus der vorherigen Übung fortsetzen.

   1. Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung, Sie müssen einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren. Klicken Sie dazu im Projektmappen-Explorer klicken Sie auf die **WebFormsLab** Projekt **NuGet-Pakete verwalten**.
   2. In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.
   3. Abschließend erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.

      > [!NOTE]
      > Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken in Ihrem Projekt, Versand Verringern der Projektgröße. Mit NuGet Power Tools können werden durch Angabe von Versionen des Pakets in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, die, das Sie das Projekt ausführen, können. Deshalb müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit wird.
2. Drücken Sie **F5** zum Starten der Webanwendung. Wechseln Sie auf die Seite, und klicken Sie auf die **Hinzufügen eines neuen Kunden** Link.
3. Mit der rechten Maustaste auf die Seite "Browser", und wählen **Quelltext anzeigen** Option, um den von der Anwendung generiert HTML-Code zu öffnen.

    ![Anzeigen der Seite HTML-Code](whats-new-in-web-forms-in-aspnet-45/_static/image9.png "Anzeigen der Seite HTML-Code")

    *Anzeigen der Seite HTML-code*
4. Führen Sie den Quellcode der Seite einen Bildlauf aus, und beachten Sie, dass ASP.NET JavaScript-Code und Daten Validierungssteuerelemente auf der Seite um die Überprüfungen ausführen und Anzeigen der Fehlerliste eingefügt wurde.

    ![Überprüfung von JavaScript-Code auf CustomerDetails](whats-new-in-web-forms-in-aspnet-45/_static/image10.png "Überprüfung-JavaScript-Code in CustomerDetails-Seite")

    *Überprüfung von JavaScript-Code in CustomerDetails-Seite*
5. Schließen Sie den Browser, und wechseln Sie zurück zu Visual Studio.
6. Jetzt können Sie die unaufdringliche Validierung. Open **"Web.config"** und suchen Sie nach **ValidationSettings:UnobtrusiveValidationMode** -Schlüssel in der **"appSettings"** Abschnitt **.** Legen Sie den Wert auf **WebForms**.

    [!code-xml[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample22.xml)]

    > [!NOTE]
    > Sie können auch diese Eigenschaft festlegen, der &quot; **Seite\_Load** &quot; Ereignis in der Sie die unaufdringliche Validierung nur für einige Seiten aktivieren möchten.
7. Open **CustomerDetails.aspx** , und drücken Sie **F5** zum Starten der Web-Anwendung.
8. Drücken Sie die Taste F12 die Internet Explorer-Entwicklertools zu öffnen. Nachdem Sie die Developer Tools geöffnet ist, wählen Sie die Registerkarte "Skript". Wählen Sie **CustomerDetails.aspx** aus dem Menü und Take wurden in den Browser des lokalen Standorts Beachten Sie, dass die Skripts benötigt, um die Ausführung von jQuery auf der Seite geladen.

    ![Laden die jQuery JavaScript-Dateien direkt aus dem lokalen IIS-Server](whats-new-in-web-forms-in-aspnet-45/_static/image11.png "laden die jQuery JavaScript-Dateien direkt aus dem lokalen IIS-Server")

    *Laden die jQuery-JavaScript-Dateien direkt aus dem lokalen IIS-server*
9. Schließen Sie den Browser zu Visual Studio zurück. Öffnen der **Site.Master** -Datei erneut, und suchen Sie die **ScriptManager**. Fügen Sie das Attribut **EnableCdn** Eigenschaft mit dem Wert **"true"**. Dies zwingt jQuery aus, um aus den online-URL und nicht aus der lokalen Website-URL geladen werden.
10. Open **CustomerDetails.aspx** in Visual Studio. Drücken Sie F5, um die Website ausgeführt werden soll. Wenn Internet Explorer geöffnet wird, drücken Sie die Taste F12, um die Entwicklertools zu öffnen. Wählen Sie die **Skript** Registerkarte, und klicken Sie dann sehen Sie sich die Dropdown-Liste. Beachten Sie, dass die jQuery-JavaScript-Dateien nicht mehr vom lokalen Standort, sondern vielmehr die online jQuery CDN geladen werden.

    ![Laden die jQuery JavaScript-Dateien aus dem CDN](whats-new-in-web-forms-in-aspnet-45/_static/image12.png "laden die jQuery JavaScript-Dateien aus dem CDN")

    *Laden die jQuery-JavaScript-Dateien aus dem CDN*
11. Öffnen Sie den Quellcode der HTML-Seite erneut mit der Quelle Ansichtsoption im Browser. Beachten Sie, dass durch Aktivieren der unaufdringlichen Validierung ASP.NET den eingefügten JavaScript-Code durch Daten ersetzt hat \*Attribute.

    ![Unaufdringliche Validierungscode](whats-new-in-web-forms-in-aspnet-45/_static/image13.png "unaufdringliche Validierungscode")

    *Unaufdringliche Validierungscode*

    > [!NOTE]
    > In diesem Beispiel haben Sie gesehen, wie eine Validierung mit datenanmerkungen in nur wenigen HTML und JavaScript-Zeilen vereinfacht wurde. Zuvor ohne unaufdringliche Validierung, die mehrere Validierungssteuerelemente, die Sie hinzufügen möchten, je größer Ihre JavaScript-Validierungscode wächst.

<a id="Task_2_-_Validating_the_Model_with_Data_Annotations"></a>
#### <a name="task-2---validating-the-model-with-data-annotations"></a>Aufgabe 2: überprüfen das Modell mit Datenanmerkungen

ASP.NET 4.5 führt Anmerkungen der datenüberprüfung für Web Forms. Anstatt ein Validierungssteuerelement für jede Eingabe, können Sie jetzt Einschränkungen in Ihren Modellklassen definieren und diese auf alle Ihre Webanwendung verwenden. In diesem Abschnitt lernen Sie, wie Sie mit datenanmerkungen, für die Überprüfung ein Formular für das neu/bearbeiten.

1. Open **CustomerDetail.aspx** Seite. Beachten Sie, die der Kunde zunächst nennen und zweitens die **EditItemTemplate** und **InsertItemTemplate** Abschnitte werden überprüft, ein RequiredFieldValidator-Steuerelemente verwenden. Jede Bestätigung wird auf eine bestimmte Bedingung zugeordnet, daher Sie so viele Bestätigungen als Bedingung aus, um zu überprüfen sind müssen.
2. Hinzufügen von datenanmerkungen, um zu überprüfen, ob die Customer-Modell-Klasse. Open **Customer.cs** -Klasse in der **Modell** Ordner und *ergänzen* jede Eigenschaft, die Attribute für die datenanmerkung verwenden.

    (Codeausschnitt - *Web Forms-Lab - Ex02 - Datenanmerkungen*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample23.cs)]

    > [!NOTE]
    > .NET Framework 4.5 wurde erweitert, dass die vorhandenen Daten anmerkungsauflistung. Dies sind einige der die datenanmerkungen, die Sie verwenden können: [CreditCard], [Phone], [EmailAddress], [Bereich], [vergleichen], [Url], [FileExtensions], [Required], [Key], [RegularExpression].
    > 
    > Einige Beispiele zur Verwendung:
    > 
    > [Key]: Specifies that an attribute is the unique identifier
    > 
    > [Range(0.4, 0.5, ErrorMessage=&quot;{Write an error message}&quot;]: Double range
    > 
    > [EmailAddress(ErrorMessage=&quot;Invalid Email&quot;), MaxLength(56)]: Two annotations in the same line.
    > 
    > Sie können auch Ihre eigenen Fehlermeldungen innerhalb jedes Attribut definieren.
3. Open **CustomerDetails.aspx** und entfernen Sie alle RequiredFieldvalidators für die vor-und Nachnamen Felder in den in EditItemTemplate InsertItemTemplate Abschnitten und das FormView-Steuerelement.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample24.aspx)]

    > [!NOTE]
    > Ein Vorteil der Verwendung von datenanmerkungen ist, dass die Validierungslogik in Ihren Anwendungsseiten nicht dupliziert werden kann. Sie einmal im Modell definieren und verwenden Sie es auf alle Anwendungsseiten, die Daten bearbeiten können.
4. Open **CustomerDetails.aspx** Code-Behind, und suchen Sie die SaveCustomer-Methode. Diese Methode wird aufgerufen, wenn einen neuen Kunde einfügen und empfängt den Customer-Parameter aus den Werten der FormView-Steuerelement. Bei der Zuordnung zwischen der Steuerelemente der Seite und die verbindungsherstellung der Parameter-Objekt, auf ASP.NET ausgeführt wird, die modellvalidierung für die datenanmerkung Attribute und füllen Sie die ModelState-Wörterbuchs, mit dem aufgetretenen Fehler, ggf.

    Die ModelState.IsValid gibt nur true, wenn alle Felder für das Modell gültig sind, nach dem Ausführen der Validierung zurück.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample25.cs)]
5. Hinzufügen einer **ValidationSummary** Steuerelement am Ende der CustomerDetails-Seite, um die Liste der Modellfehler anzuzeigen.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample26.aspx)]

    Die **ShowModelStateErrors** ist eine neue Eigenschaft für die ValidationSummary-Steuerelement, die bei Festlegung auf **"true"**, das Steuerelement zeigt die Fehler aus den ModelState-Wörterbuchs. Diese Fehler stammen aus der datenüberprüfung für Anmerkungen.
6. Drücken Sie **F5** zum Ausführen der Web-Anwendung. Füllen Sie das Formular mit einigen fehlerhaften Werten, und klicken Sie auf **speichern** auszuführenden Überprüfung. Beachten Sie, dass der fehlerzusammenfassung am unteren Rand.

    ![Validierung mit Datenanmerkungen](whats-new-in-web-forms-in-aspnet-45/_static/image14.png "Validierung mit Datenanmerkungen")

    *Validierung mit Datenanmerkungen*

<a id="Task_3_-_Handling_Custom_Database_Errors_with_ModelState"></a>
#### <a name="task-3---handling-custom-database-errors-with-modelstate"></a>Aufgabe 3: Behandeln von Fehlern der benutzerdefinierten Datenbank mit ModelState

In früheren Version von Web Forms kann das Behandeln von Datenbankfehlern z. B. eine Zeichenfolge zu lang oder eine Verletzung des eindeutigen Schlüssels vorsehen, Auslösen von Ausnahmen in Ihrem repositorycode aus, und klicken Sie dann die Ausnahmebehandlung in der CodeBehind-Fehler angezeigt. Eine große Menge an Code ist erforderlich, relativ einfach etwas zu tun.

In Web Forms 4.5 kann das ModelState-Objekt verwendet werden, um Fehler auf der Seite aus dem Modell oder aus der Datenbank konsistent.

In dieser Aufgabe werden Sie Code aus, um ordnungsgemäß Behandeln von Datenbankausnahmen und die entsprechende Meldung wird angezeigt, für dem Benutzer, die mit dem ModelState-Objekt hinzufügen.

1. Während die Anwendung weiterhin ausgeführt wird, versuchen Sie, den Namen einer Kategorie, die mit einem doppelten Wert zu aktualisieren.

    ![Aktualisieren eine Kategorie mit einem doppelten Namen](whats-new-in-web-forms-in-aspnet-45/_static/image15.png "eine Kategorie mit einem doppelten Namen aktualisieren")

    *Aktualisieren eine Kategorie mit einem doppelten Namen*

    Beachten Sie, die eine Ausnahme, aufgrund ausgelöst wird der &quot;eindeutige&quot; Einschränkung der **"CategoryName"** Spalte.

    ![Ausnahme doppelte Namen für die](whats-new-in-web-forms-in-aspnet-45/_static/image16.png "Ausnahme doppelte Namen für die")

    *Ausnahme für doppelte Namen*
2. Beenden des Debuggens. In der **Products.aspx.cs** Code-Behind-Datei, die Aktualisierung der **UpdateCategory** Methode zum Behandeln von Ausnahmen, die von der Datenbank ausgelöst. SaveChanges() Methodenaufruf, und fügen Sie einen Fehler an die **ModelState** Objekt.

    Die neue **TryUpdateModel** Methode aktualisiert das Category-Objekt, das vom Benutzer angegebene Daten aus dem Formular mit aus der Datenbank abgerufen.

    (Codeausschnitt - *Web Forms Lab - Ex02 - UpdateCategory Behandeln von Fehlern*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample27.cs)]

    > [!NOTE]
    > Im Idealfall müssten Sie zum Identifizieren der Ursache für die "dbupdateexception", und überprüfen, ob die zugrunde liegende Ursache der Verstoß gegen eine Einschränkung auf eindeutige Schlüssel ist.
3. Open **beispielsweise "Products.aspx"** und Hinzufügen einer **ValidationSummary** Steuerelement unterhalb der Kategorien GridView, um die Liste der Modellfehler anzuzeigen.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample28.aspx)]
4. Führen Sie die Website, und wechseln Sie zu der Seite "Produkte". Versuchen Sie, den Namen einer Kategorie, die über einen doppelten Wert zu aktualisieren.

    Beachten Sie, die die Ausnahme behandelt wurde und die Fehlermeldung wird angezeigt, der **ValidationSummary** Steuerelement.

    ![Fehler der Kategorie dupliziert](whats-new-in-web-forms-in-aspnet-45/_static/image17.png "dupliziert Kategorie-Fehler")

    *Doppelte Kategorie-Fehler*

<a id="Task_4_-_Request_Validation_in_ASPNET_Web_Forms_45"></a>
#### <a name="task-4---request-validation-in-aspnet-web-forms-45"></a>Aufgabe 4: der Anforderungsvalidierung in ASP.NET Web Forms 4.5

Das Feature zur Anforderung Validierung in ASP.NET bietet ein gewisses Maß an standardmäßigen Schutz vor Angriffen mit siteübergreifenden Skripterstellung (XSS). In früheren Versionen von ASP.NET Request-Überprüfung wurde standardmäßig aktiviert und kann nur für eine gesamte Seite deaktiviert werden. Mit der neuen Version von ASP.NET Web Forms können jetzt die Anforderungsvalidierung für ein einzelnes Steuerelement deaktivieren, lazy-Request-Überprüfung ausführen oder Zugriff auf nicht überprüften Anforderungsdaten (Seien Sie vorsichtig, wenn Sie dies tun!).

1. Drücken Sie **STRG + F5** , starten den Standort ohne Debuggen aus, und wechseln zu der Seite "Produkte". Wählen Sie eine Kategorie aus, und klicken Sie dann auf die **bearbeiten** Link auf eines der Produkte.
2. Geben Sie eine Beschreibung, die potenziell gefährliche Inhalte enthält, darunter z. B. HTML-Tags ein. Beachten Sie die Ausnahme, die aufgrund der Anforderungsvalidierung ausgelöst.

    ![Bearbeiten eines Produkts mit potenziell gefährliche Inhalte](whats-new-in-web-forms-in-aspnet-45/_static/image18.png "ein Produkt mit potenziell gefährlichen Inhalt in Bearbeitung")

    *Bearbeiten eines Produkts mit potenziell gefährliche Inhalte*

    ![Ausnahme aufgrund von Anforderungsvalidierung](whats-new-in-web-forms-in-aspnet-45/_static/image19.png "Ausnahme aufgrund der Request-Überprüfung")

    *Ausnahme aufgrund der Request-Überprüfung*
3. Schließen Sie die Seite, und drücken Sie in Visual Studio **UMSCHALT + F5** debugging beenden.
4. Öffnen der **ProductDetails.aspx** Seite, und suchen Sie die **Beschreibung** Textfeld.
5. Hinzufügen des neuen **ValidateRequestMode** Eigenschaft, um das Textfeld ein, und setzen ihren Wert auf **deaktiviert**.

    Die neue **ValidateRequestMode** Attribut können Sie die Anforderungsvalidierung präzise auf jedes Steuerelement zu deaktivieren. Dies ist nützlich, wenn Sie möchten eine Eingabe zu verwenden, die HTML-Code erhalten können, aber die Validierung für den Rest der Seite beibehalten möchten.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample29.aspx)]
6. Drücken Sie **F5** zum Ausführen der Webanwendung. Öffnen Sie erneut auf der Produktseite von bearbeiten und führen Sie eine produktbeschreibung, einschließlich der HTML-Tags. Beachten Sie, dass Sie jetzt HTML-Inhalt in der Beschreibung hinzufügen können.

    ![Anforderungsvalidierung für die produktbeschreibung deaktiviert](whats-new-in-web-forms-in-aspnet-45/_static/image20.png "Anforderungsvalidierung für die produktbeschreibung deaktiviert")

    *Anforderungsüberprüfung für die produktbeschreibung deaktiviert*

    > [!NOTE]
    > In einer produktionsanwendung sollten Sie bereinigen den HTML-Code eingegeben haben, durch den Benutzer aus, um sicherzustellen, dass nur sichere HTML-Tags eingegeben werden (z. B. stehen keine &lt;Skript&gt; Tags). Zu diesem Zweck können Sie [Microsoft Web Protection Library](https://www.nuget.org/packages/AntiXSS).
7. Bearbeiten Sie das Produkt erneut. Geben Sie HTML-Code in das Feld Name ein, und klicken Sie auf **speichern**. Achten Sie darauf, die Anforderungsüberprüfung ist nur für das Feld "Beschreibung" deaktiviert und die restlichen Felder neu überprüft weiterhin anhand der potenziell gefährlichen Inhalts.

    ![Anforderungsvalidierung in den restlichen Feldern aktiviert](whats-new-in-web-forms-in-aspnet-45/_static/image21.png "Anforderungsvalidierung in den restlichen Feldern aktiviert")

    *Request-Überprüfung, die in den restlichen Feldern aktiviert*

    ASP.NET Web Forms 4.5 enthält eine neue Anforderung Validierungsmodus für die Anforderung verzögert Validierung. Mit der Anforderung Überprüfung-moduseinstellung **4.5**, wenn ein Stück Code greift auf *Request.Form [&quot;Schlüssel&quot;]*, ASP.NET 4.5 für die Überprüfung wird nur anforderungstrigger Anforderungsvalidierung für dieses bestimmte Element in der formularauflistung.

    Darüber hinaus umfasst ASP.NET 4.5 jetzt kerncodierungsroutinen von Anti-XSS-Bibliothek von Microsoft v4. 0. Die Anti-XSS-Codierungsroutinen, durch die neue implementiert werden *AntiXssEncoder* Typ gefunden wird, in der neuen **System.Web.Security.AntiXss** Namespace. Mit der **EncoderType** Parameter für die Verwendung konfiguriert *AntiXssEncoder*, alle Ausgabedateien Codierung in ASP.NET automatisch die neuen Codierungsroutinen verwendet.
8. ASP.NET 4.5-Request-Überprüfung unterstützt auch nicht überprüfte Zugriff auf Daten. ASP.NET 4.5 Fügt eine neue Auflistungseigenschaft, um die **HttpRequest** Objekt mit dem Namen **Unvalidated**. Wenn Sie Navigieren in **HttpRequest.Unvalidated** haben Sie Zugriff auf alle allgemeinen Teile der Anforderungsdaten, einschließlich Forms, Formularwerte, Cookies, URLs und So weiter.

    ![Request.Unvalidated Objekt](whats-new-in-web-forms-in-aspnet-45/_static/image22.png "Request.Unvalidated-Objekt")

    *Request.Unvalidated-Objekt*

    > [!NOTE]
    > **Verwenden Sie die HttpRequest.Unvalidated-Eigenschaft mit Bedacht vorgehen!** Stellen Sie sicher, dass Sie benutzerdefinierte Validierung für die raw-Anforderungsdaten, um sicherzustellen, dass gefährlicher Text wird nicht zurückgeleitet an ahnungslose Kunden gerendert sorgfältig ausführen!

<a id="Exercise3"></a>

<a id="Exercise_3_Asynchronous_Page_Processing_in_ASPNET_Web_Forms"></a>
### <a name="exercise-3-asynchronous-page-processing-in-aspnet-web-forms"></a>Übung 3: Asynchrone Seitenverarbeitung in der ASP.NET Web Forms

In dieser Übung wird die neue asynchrone seitenverarbeitung Funktionen in ASP.NET Web Forms vorgestellt werden.

<a id="Task_1_-_Updating_the_Product_Details_Page_to_Upload_and_Show_Images"></a>
#### <a name="task-1---updating-the-product-details-page-to-upload-and-show-images"></a>Aufgabe 1: Aktualisieren des Produkts Detailseite zum Hochladen und Anzeigen von Bildern

In dieser Aufgabe aktualisieren Sie die Seite für Produktdetails damit die Benutzer geben eine Bild-URL für das Produkt und in der schreibgeschützten Ansicht anzeigen. Erstellen Sie eine lokale Kopie des angegebenen Bilds synchron herunterladen. In der nächsten Aufgabe aktualisieren Sie diese Implementierung wird die asynchrone Arbeit zu erleichtern.

1. Open **Visual Studio 2012** und laden die **beginnen** Lösung befindet sich in **Source\Ex3-Async\Begin** aus Ordner "des Labs". Alternativ können Sie arbeiten in Ihre vorhandene Lösung aus den vorherigen Übungen fortsetzen.

   1. Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung, Sie müssen einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren. Klicken Sie dazu im Projektmappen-Explorer klicken Sie auf die **WebFormsLab** Projekt, und wählen **NuGet-Pakete verwalten**.
   2. In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.
   3. Abschließend erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.

      > [!NOTE]
      > Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken in Ihrem Projekt, Versand Verringern der Projektgröße. Mit NuGet Power Tools können werden durch Angabe von Versionen des Pakets in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, die, das Sie das Projekt ausführen, können. Deshalb müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit wird.
2. Öffnen der **ProductDetails.aspx** Seite Datenquelle, und fügen Sie ein Feld in das FormView ItemTemplate-Element das Produktbild angezeigt.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample30.aspx)]
3. Hinzufügen eines Felds, um die Bild-URL in das FormView EditTemplate angeben.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample31.aspx)]
4. Öffnen der **ProductDetails.aspx.cs** CodeBehind-Datei, und fügen Sie die folgenden namespacedirektiven hinzu.

    (Codeausschnitt - *Web Forms Lab - Ex03 - Namespaces*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample32.cs)]
5. Erstellen Sie eine **UpdateProductImage** Methode zum Speichern von remote-Images in der lokalen **Images** Ordner, und aktualisieren Sie die Product-Entität mit dem neuen Wert des Image-Speicherort.

    (Codeausschnitt - *Web Forms Lab - Ex03 - UpdateProductImage*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample33.cs)]
6. Update der **UpdateProduct** aufzurufende Methode der **UpdateProductImage** Methode.

    (Codeausschnitt - *Web Forms Lab - Ex03 - UpdateProductImage Aufruf*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample34.cs)]
7. Führen Sie die Anwendung aus, und versuchen Sie, ein Bild für ein Produkt hochladen. Beispielsweise können Sie die folgenden Bild-URL aus Office Clip Arts: [[http://officeimg.vo.msecnd.net/images/MB900437099.jpg](http://officeimg.vo.msecnd.net/images/MB900437099.jpg)](http://officeimg.vo.msecnd.net/images/MB900437099.jpg)

    ![Festlegen eines Bilds für ein Produkt](whats-new-in-web-forms-in-aspnet-45/_static/image23.png "Festlegen eines Bilds für ein Produkt")

    *Festlegen eines Bilds für ein Produkt*

<a id="Task_2_-_Adding_Asynchronous_Processing_to_the_Product_Details_Page"></a>
#### <a name="task-2---adding-asynchronous-processing-to-the-product-details-page"></a>Aufgabe 2: Hinzufügen von asynchronen Verarbeitung auf der Seite für Produktdetails

In dieser Aufgabe aktualisieren Sie die Seite für Produktdetails um die asynchrone Arbeit zu erleichtern. Sie werden eine langwierige Aufgabe – der Prozess für die Image-Download – erweitern, mithilfe der asynchronen seitenverarbeitung ASP.NET 4.5.

Asynchrone Methoden in Webanwendungen können verwendet werden, auf die Weise zu optimieren, die ASP.NET Threadpools verwendet werden. In ASP.NET gibt es sind eine begrenzte Anzahl von Threads im Threadpool für die Teilnahme anfordert, daher bei der alle Threads ausgelastet ist, sind ASP.NET wird gestartet, um neue Anforderungen abzulehnen, sendet Fehlermeldungen für die Anwendung und ist Ihre Website nicht mehr verfügbar.

Zeitaufwändige Operationen auf Ihrer Website sind gute Kandidaten für asynchrone Programmierung, da sie einen längeren Zeitraum die zugewiesenen Thread belegen. Dies schließt lang andauernder Anforderungen, die mit vielen unterschiedlichen Elementen und Seiten, die offline-Vorgänge, für solche Abfragen einer Datenbank oder den Zugriff auf einen externen Webserver erfordern. Der Vorteil ist, dass bei Verwendung von asynchronen Methoden für diese Vorgänge während der Verarbeitung der Seite wird der Thread freigegeben und an den Thread zurückgegeben pool und kann verwendet werden, um eine neue Seitenanforderung überwachen möchten. Das bedeutet, die Seite startet die Verarbeitung in einem Thread vom Threadpool der Warteschleife hinzu und Verarbeitung in eine andere möglicherweise abgeschlossen werden, nach dem Abschluss der asynchronen Verarbeitung.

1. Öffnen der **ProductDetails.aspx** Seite. Hinzufügen der **Async** -Attribut in der **Seite** Element, und legen Sie ihn auf **"true"**. Dieses Attribut teilt ASP.NET implementieren die IHttpAsyncHandler-Schnittstelle.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample35.aspx)]
2. Fügen Sie eine Bezeichnung am unteren Rand der Seite, um die Details der Threads Ausführen der Seite anzuzeigen.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample36.aspx)]
3. Öffnen Sie **ProductDetails.aspx.cs** und fügen Sie die folgenden namespacedirektiven hinzu.

    (Codeausschnitt - *Web Forms Lab - Ex03 - Namespaces 2*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample37.cs)]
4. Ändern der **UpdateProductImage** Methode, um das Image mit einer asynchronen Aufgabe herunterzuladen. Ersetzen Sie die **"Webclient"** **DownloadFile** -Methode mit der **DownloadFileTaskAsync** Methode und die **"await"** Schlüsselwort.

    (Codeausschnitt - *Web Forms Lab - Ex03 - UpdateProductImage Async*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample38.cs)]

    Die RegisterAsyncTask registriert eine neue Seite asynchrone Aufgabe, die in einem anderen Thread ausgeführt werden. Sie erhält einen Lambda-Ausdruck, mit der Aufgabe (t) ausgeführt werden. Die **"await"** -Schlüsselwort in der **DownloadFileTaskAsync** Methode konvertiert den Rest der Methode in einen Rückruf, der nach asynchron aufgerufen wird, die **DownloadFileTaskAsync** -Methode abgeschlossen wurde. ASP.NET wird die Ausführung der Methode fortgesetzt, indem Sie alle HTTP-Anforderung ursprüngliche Werte automatisch zu verwalten. Das neue asynchrone Programmiermodell in .NET 4.5 können Sie zum Schreiben von asynchronen Codes, der Wie synchroner Code aussieht und lassen Sie vom Compiler schwierigkeiten von Rückruffunktionen oder fortsetzungscode zu behandeln.
    > [!NOTE]
    > RegisterAsyncTask und PageAsyncTask wurden bereits seit .NET 2.0 zur Verfügung. Das Schlüsselwort "await" ist über das asynchrone Programmiermodell von .NET 4.5 neu, und kann verwendet werden, zusammen mit den neuen TaskAsync-Methoden aus der .NET WebClient-Objekt.
5. Fügen Sie Code, um die Threads anzuzeigen, auf denen der Code gestartet, und die Ausführung beendet. Aktualisieren Sie zu diesem Zweck die **UpdateProductImage** Methode durch den folgenden Code.

    (Codeausschnitt - *Web Forms-Lab - Ex03 - Show Threads*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample39.cs)]
6. Öffnen Sie die Website **"Web.config"** Datei. Fügen Sie die folgende AppSetting-Variable.

    [!code-xml[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample40.xml)]
7. Drücken Sie **F5** zum Ausführen der Anwendung und zum Hochladen von Images für das Produkt. Beachten Sie, dass die Threads-ID, in dem der Code gestartet und abgeschlossen abweichen. Dies ist, da asynchrone Aufgaben in einem separaten Thread aus dem ASP.NET-Threadpool ausgeführt. Wenn die Aufgabe abgeschlossen ist, wird ASP.NET legt die Aufgabe wieder in die Warteschlange und eine der verfügbaren Threads zuweist.

    ![Ein Bild asynchron herunterladen](whats-new-in-web-forms-in-aspnet-45/_static/image24.png "asynchron Herunterladen eines Images")

    *Ein Bild herunterladen asynchron*

> [!NOTE]
> Darüber hinaus können Sie diese Anwendung in Azure Folgendes bereitstellen [Anhang B: Veröffentlichen einer ASP.NET MVC 4-Anwendung mit Web Deploy](#AppendixB).


* * *

<a id="Summary"></a>
## <a name="summary"></a>Zusammenfassung

In dieser praktischen Übungseinheit haben die folgenden Konzepte behandelt veranschaulicht und wurde:

- Verwenden von Ausdrücken für stark typisierte Datenbindung
- Verwenden Sie neue Features der Modell-Bindung in Web Forms
- Verwenden von Wertanbietern für die Zuordnung von Daten der Seite auf Code-Behind-Methoden
- Verwenden von Datenanmerkungen für die Validierung von Benutzereingaben
- Nutzen Sie Advange Unobstrusive die clientseitige Validierung mit jQuery in Web Forms
- Differenzierte Anforderungsvalidierung implementieren
- Implementieren der asynchronen seitenverarbeitung in Web Forms

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Anhang A: Installieren von Visual Studio Express 2012 für Web

Sie installieren können **Microsoft Visual Studio Express 2012 für Web** oder einem anderen &quot;Express&quot; Version verwenden, die **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. Die folgenden Anweisungen führen Sie über die erforderlichen Schritte zum Installieren *Visual Studio Express 2012 für Web* mit *Microsoft Web Platform Installer*.

1. Wechseln Sie zu [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). Auch wenn Sie bereits Webplattform-Installer installiert haben, können Sie öffnen ihn, und suchen Sie nach dem Produkt &quot; <em>Visual Studio Express 2012 für das Web mit Azure SDK</em>&quot;.
2. Klicken Sie auf **jetzt installieren**. Wenn Sie keine **Webplattform-Installer** gelangen Sie zum Herunterladen und installieren Sie diese zuerst.
3. Einmal **Webplattform-Installer** geöffnet ist, klicken Sie auf **installieren** um das Setup zu starten.

    ![Installieren von Visual Studio Express](whats-new-in-web-forms-in-aspnet-45/_static/image25.png "Installieren von Visual Studio Express")

    *Installieren von Visual Studio Express*
4. Lesen Sie die Produkte Lizenzen und Begriffe, und klicken Sie auf **akzeptieren** um den Vorgang fortzusetzen.

    ![Akzeptieren der Lizenzbedingungen](whats-new-in-web-forms-in-aspnet-45/_static/image26.png)

    *Akzeptieren der Lizenzbedingungen*
5. Warten Sie, bis der Prozess zum Herunterladen und die Installation abgeschlossen ist.

    ![Installationsstatus](whats-new-in-web-forms-in-aspnet-45/_static/image27.png)

    *Installationsstatus*
6. Wenn die Installation abgeschlossen ist, klicken Sie auf **Fertig stellen**.

    ![Die Installation wurde abgeschlossen](whats-new-in-web-forms-in-aspnet-45/_static/image28.png)

    *Die Installation wurde abgeschlossen*
7. Klicken Sie auf **beenden** Webplattform-Installer zu schließen.
8. Um Visual Studio Express für Web zu öffnen, wechseln Sie zu der **starten** schreiben zu starten und Bildschirm &quot; **VS Express**&quot;, klicken Sie dann auf die **Visual Studio Express für Web** die Kachel.

    ![Visual Studio Express für Web-Kachel](whats-new-in-web-forms-in-aspnet-45/_static/image29.png)

    *Visual Studio Express für Web-Kachel*

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Anhang B: Veröffentlichen einer ASP.NET MVC 4-Anwendung mit Web Deploy

In diesem Anhang erfahren Sie, wie eine neue Website aus dem Azure-Portal erstellen und Veröffentlichen der Anwendung, die Sie erhalten haben, indem der testumgebung können die Nutzung der Web Deploy-publishing-Funktion von Azure bereitgestellten.

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a>Aufgabe 1: Erstellen einer neuen Website über das Azure-Portal

1. Wechseln Sie zu der [Azure-Verwaltungsportal](https://manage.windowsazure.com/) und melden Sie sich mit den Microsoft-Anmeldeinformationen, die Ihrem Abonnement zugeordnet.

    > [!NOTE]
    > Mit Azure können Sie 10 ASP.NET-Websites kostenlos hosten und dann zu skalieren, wenn Ihr Datenverkehr zunimmt. Sie können registrieren [hier](http://aka.ms/aspnet-hol-azure).

    ![Melden Sie sich bei Windows Azure-Portal](whats-new-in-web-forms-in-aspnet-45/_static/image30.png "melden Sie sich bei Windows Azure-Portal")

    *Anmelden beim Portal*
2. Klicken Sie auf **neu** auf der Befehlsleiste.

    ![Erstellen einer neuen Website](whats-new-in-web-forms-in-aspnet-45/_static/image31.png "Erstellen einer neuen Website")

    *Erstellen einer neuen Website*
3. Klicken Sie auf **Compute** | **Website**. Wählen Sie dann **Schnellerfassung** Option. Geben Sie eine verfügbare URL für die neue Website, und klicken Sie auf **Website erstellen**.

    > [!NOTE]
    > Azure ist der Host für eine Webanwendung, die in der Cloud, die Sie steuern und verwalten können. Die Option "Schnellerfassung" können Sie eine vollständige Webanwendung in Azure von außerhalb des Portals bereitstellen. Er umfasst keine Informationen zum Einrichten einer Datenbank.

    ![Erstellen einer neuen Website mithilfe der Schnellerfassung](whats-new-in-web-forms-in-aspnet-45/_static/image32.png "Erstellen einer neuen Website mithilfe der Schnellerfassung")

    *Erstellen einer neuen Website mithilfe der Schnellerfassung*
4. Warten Sie, bis die neue **Website** erstellt wird.
5. Nachdem die Website erstellt wurde, klicken Sie auf den Link unter der **URL** Spalte. Überprüfen Sie, dass die neue Website ausgeführt wird.

    ![Navigieren zu der neuen Website](whats-new-in-web-forms-in-aspnet-45/_static/image33.png "auf die neue Website durchsuchen")

    *Um die neue Website durchsuchen*

    ![Website](whats-new-in-web-forms-in-aspnet-45/_static/image34.png "Website ausgeführt wird")

    *Die Website ausgeführt wird*
6. Wechseln Sie zurück zum Portal, und klicken Sie auf den Namen der Website unter der **Namen** Spalte die Verwaltungsseiten angezeigt.

    ![Öffnen die Website-Verwaltungsseiten](whats-new-in-web-forms-in-aspnet-45/_static/image35.png "öffnen die Website-Verwaltungsseiten")

    *Öffnen die Website-Verwaltungsseiten*
7. In der **Dashboard** Seite die **Blick** auf die **Veröffentlichungsprofil herunterladen** Link.

    > [!NOTE]
    > Die *Veröffentlichungsprofil* enthält alle Informationen zum Veröffentlichen einer Webanwendung in Azure für die einzelnen aktivierten Veröffentlichungsmethoden erforderlich sind. Das Veröffentlichungsprofil enthält die URLs, Benutzeranmeldeinformationen und datenbankzeichenfolgen, die zum Herstellen einer Verbindung mit und die Authentifizierung bei jedem der Endpunkte für die eine Veröffentlichungsmethode aktiviert ist. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express für Web** und **Microsoft Visual Studio 2012** Unterstützung, die beim Lesen von veröffentlichungsprofilen Automatisieren der Konfiguration von diesen Programmen Veröffentlichung von Webanwendungen in Azure.

    ![Herunterladen der Website-Veröffentlichungsprofil](whats-new-in-web-forms-in-aspnet-45/_static/image36.png "herunterladen den Website-Veröffentlichungsprofil")

    *Herunterladen der Website-Veröffentlichungsprofil*
8. Laden Sie das Veröffentlichungsprofil an einem bekannten Speicherort herunter. In dieser Übung wird weiter wie Sie diese Datei verwenden, um das Veröffentlichen einer Webanwendung in Azure aus Visual Studio angezeigt.

    ![Speichern das Veröffentlichungsprofil](whats-new-in-web-forms-in-aspnet-45/_static/image37.png "speichern das Veröffentlichungsprofil")

    *Speichern das Veröffentlichungsprofil*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Aufgabe 2: Konfigurieren des Datenbank-Servers

Wenn Ihre Anwendung nutzt SQL Server Datenbanken, die Sie zum Erstellen eines SQL-Datenbank-Servers benötigen. Wenn zum Bereitstellen einer einfachen Anwendung, die keine SQL Server verwendet werden sollen, können Sie diese Aufgabe überspringen.

1. Sie benötigen einen SQL-Datenbank-Server zum Speichern der Datenbank. Sehen Sie die SQL-Datenbank-Server aus Ihrem Abonnement im Azure-Verwaltungsportal auf **Sql-Datenbanken** | **Server** | **Dashboard des Servers**. Wenn Sie nicht mit eine Server-Instanz verfügen, können Sie erstellen, mithilfe der **hinzufügen** auf der Befehlsleiste auf die Schaltfläche. Notieren Sie sich die **Servername und -URL, Administrator-Anmeldenamen und Kennwort**, wie Sie sie in den nächsten Aufgaben verwenden. Erstellen Sie die Datenbank nicht noch, wie es in einem späteren Zeitpunkt erstellt wird.

    ![SQL Server-Datenbank-Dashboard](whats-new-in-web-forms-in-aspnet-45/_static/image38.png "Dashboard der SQL-Datenbank-Server")

    *SQL Server-Datenbank-Dashboard*
2. In der nächsten Aufgabe testen Sie die Verbindung mit der Datenbank, in Visual Studio aus diesem Grund Sie Ihre lokale IP-Adresse in der Liste des Servers der einschließen müssen **zulässige IP-Adressen**. Zu diesem Zweck klicken Sie auf **konfigurieren**, wählen Sie die IP-Adresse von **Current Client IP Address** und fügen Sie ihn auf die **IP-Startadresse** und **IP-Endadresse** Textfelder, und klicken Sie auf die ![add-client-ip-address-ok-button](whats-new-in-web-forms-in-aspnet-45/_static/image39.png) Schaltfläche.

    ![Client-IP-Adresse hinzufügen](whats-new-in-web-forms-in-aspnet-45/_static/image40.png)

    *Client-IP-Adresse hinzufügen*
3. Einmal die **Client-IP-Adresse** wird hinzugefügt, um die zulässigen IP-Adressen aufzulisten, klicken Sie auf **speichern** um die Änderungen zu bestätigen.

    ![Bestätigen von Änderungen](whats-new-in-web-forms-in-aspnet-45/_static/image41.png)

    *Bestätigen von Änderungen*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Aufgabe 3: Veröffentlichen einer ASP.NET MVC 4-Anwendung mit Web Deploy

1. Wechseln Sie zurück, der ASP.NET MVC 4-Projektmappe. In der **Projektmappen-Explorer**mit der rechten Maustaste auf das Websiteprojekt, und wählen Sie **veröffentlichen**.

    ![Veröffentlichen der Anwendung](whats-new-in-web-forms-in-aspnet-45/_static/image42.png "Veröffentlichen der Anwendung")

    *Veröffentlichen der Website*
2. Importieren Sie das Veröffentlichungsprofil, die, das Sie in der ersten Aufgabe gespeichert.

    ![Importieren das Veröffentlichungsprofil](whats-new-in-web-forms-in-aspnet-45/_static/image43.png "Importieren des Veröffentlichungsprofils")

    *Importieren Sie das Veröffentlichungsprofil*
3. Klicken Sie auf **überprüft, ob Verbindung**. Klicken Sie nach Abschluss der Überprüfung auf **Weiter**.

    > [!NOTE]
    > Die Überprüfung ist abgeschlossen, wenn Sie sehen, dass ein grünes Häkchen neben der Schaltfläche "Verbindung überprüfen" angezeigt werden.

    ![Überprüfen der Verbindung](whats-new-in-web-forms-in-aspnet-45/_static/image44.png "Überprüfen der Verbindung")

    *Überprüfen der Verbindung*
4. In der **Einstellungen** Seite die **Datenbanken** auf die Schaltfläche neben dem Textfeld für die datenbankverbindung (d. h. **DefaultConnection**).

    ![Web deploy-Konfiguration](whats-new-in-web-forms-in-aspnet-45/_static/image45.png "Web deploy-Konfiguration")

    *Web deploy-Konfiguration*
5. Konfigurieren Sie die Verbindung mit der Datenbank wie folgt:

   - In der **Servernamen** Geben Sie Ihre SQL-Datenbankserver URL mit der *Tcp:* Präfix.
   - In **Benutzernamen** Geben Sie Ihre Server Administrator-Anmeldenamen.
   - In **Kennwort** Geben Sie Ihre Server Administrator-Anmeldekennwort.
   - Geben Sie einen neuen Datenbanknamen ein.

     ![Konfigurieren von Ziel-Verbindungszeichenfolge](whats-new-in-web-forms-in-aspnet-45/_static/image46.png "Zielverbindungszeichenfolge konfigurieren")

     *Konfigurieren von Ziel-Verbindungszeichenfolge*
6. Klicken Sie dann auf **OK**. Bei der Aufforderung zum Erstellen der Datenbank auf **Ja**.

    ![Erstellen der Datenbank](whats-new-in-web-forms-in-aspnet-45/_static/image47.png "erstellen die datenbankzeichenfolge")

    *Erstellen der Datenbank*
7. Die Verbindungszeichenfolge, die Sie für die Verbindung mit SQL-Datenbank in Azure verwendet werden, ist innerhalb der Verbindungstyp Standard Textfeld angezeigt. Klicken Sie dann auf **Weiter**.

    ![Auf der SQL-Datenbank-Verbindungszeichenfolge](whats-new-in-web-forms-in-aspnet-45/_static/image48.png "auf SQL-Datenbank-Verbindungszeichenfolge")

    *Auf der SQL-Datenbank-Verbindungszeichenfolge*
8. In der **Vorschau** auf **veröffentlichen**.

    ![Veröffentlichen der Webanwendung](whats-new-in-web-forms-in-aspnet-45/_static/image49.png "Veröffentlichen der Webanwendung")

    *Veröffentlichen der Webanwendung*
9. Sobald der Veröffentlichungsprozess abgeschlossen ist, öffnet Ihr Standardbrowser die veröffentlichte Website.

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Anhang C: Verwenden von Codeausschnitten

Mit Codeausschnitten müssen Sie den Code zur Hand benötigten. Das Lab-Dokument informiert Sie genau wann sie verwendet werden kann wie in der folgenden Abbildung dargestellt.

![Verwenden von Visual Studio-Codeausschnitten zum Einfügen von Code in Ihrem Projekt](whats-new-in-web-forms-in-aspnet-45/_static/image50.png "mithilfe von Visual Studio-Codeausschnitten, die Code in das Projekt einfügen.")

*Verwenden von Visual Studio-Codeausschnitten zum Einfügen von Code in Ihrem Projekt*

***Hinzufügen ein Codeausschnitts, die über die Tastatur (nur c#)***

1. Platzieren Sie den Cursor, wo Sie möchten den Code einfügen.
2. Starten Sie den codeausschnittsnamen (ohne Leerzeichen oder Bindestriche) eingeben.
3. Beobachten Sie, wie IntelliSense zeigt übereinstimmende Codeausschnitte Namen.
4. Wählen Sie den richtigen Codeausschnitt (oder halten Sie eingeben, bis die gesamte codeausschnittsnamen ausgewählt ist).
5. Drücken Sie die Tab-Taste zweimal auf Einfügen des Codeausschnitts an der Cursorposition ein.

![Geben Sie den Namen des Ausschnitts](whats-new-in-web-forms-in-aspnet-45/_static/image51.png "Geben Sie den Namen des Ausschnitts")

*Geben Sie den Namen des Ausschnitts*

![Tabstopp drücken, um den hervorgehobenen Codeausschnitt auswählen](whats-new-in-web-forms-in-aspnet-45/_static/image52.png "Tab drücken, um den hervorgehobenen Codeausschnitt auswählen")

*Drücken Sie Tabstopp, um den hervorgehobenen Codeausschnitt auswählen*

![Drücken Sie die Tabulatortaste erneut, und der Ausschnitt erweitert](whats-new-in-web-forms-in-aspnet-45/_static/image53.png "drücken Sie die Tabulatortaste erneut, und der Ausschnitt werden erweitert.")

*Drücken Sie die Tabulatortaste erneut, und der Ausschnitt werden erweitert.*

***Hinzufügen ein Codeausschnitts, die mit der Maus (c#, Visual Basic und XML)*** 1. Mit der rechten Maustaste, in dem den Codeausschnitt eingefügt werden soll.

1. Wählen Sie **Ausschnitt einfügen** gefolgt von **Meine Codeausschnitte**.
2. Wählen Sie die relevante Codeausschnitte in der Liste, indem Sie darauf klicken.

![Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten](whats-new-in-web-forms-in-aspnet-45/_static/image54.png "mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten,")

*Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten*

![Wählen Sie die relevante Codeausschnitte in der Liste, indem Sie darauf klicken](whats-new-in-web-forms-in-aspnet-45/_static/image55.png "die relevante Codeausschnitte in der Liste auswählen, indem Sie darauf klicken")

*Wählen Sie die relevante Codeausschnitte in der Liste, indem Sie darauf klicken*
