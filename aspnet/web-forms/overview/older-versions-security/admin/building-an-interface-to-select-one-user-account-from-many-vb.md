---
uid: web-forms/overview/older-versions-security/admin/building-an-interface-to-select-one-user-account-from-many-vb
title: Erstellen eine Schnittstelle zum Auswählen eines Benutzerkontos aus vielen (VB) | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial werden wir eine Benutzeroberfläche mit einem ausgelagerten, filterbar Raster erstellen. Insbesondere besteht aus unserer Benutzeroberfläche aus einer Reihe von LinkButtons für...
ms.author: riande
ms.date: 04/01/2008
ms.assetid: da53380c-a16b-41c7-a20d-24343c735c52
msc.legacyurl: /web-forms/overview/older-versions-security/admin/building-an-interface-to-select-one-user-account-from-many-vb
msc.type: authoredcontent
ms.openlocfilehash: bb30c5d3ce6e04f60d8192e8ed0404b89031b4b9
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826743"
---
<a name="building-an-interface-to-select-one-user-account-from-many-vb"></a>Erstellen eine Schnittstelle zum Auswählen eines Benutzerkontos aus vielen (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/VB.12.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial12_SelectUser_vb.pdf)

> In diesem Tutorial werden wir eine Benutzeroberfläche mit einem ausgelagerten, filterbar Raster erstellen. Insbesondere wird unserer Benutzeroberfläche aus einer Reihe von LinkButtons bestehen, zum Filtern der Ergebnisse basierend auf den Buchstaben ab, der den Benutzernamen und ein GridView-Steuerelement, um die übereinstimmenden Benutzer anzuzeigen. Wir beginnen, durch die Liste aller der Benutzerkonten in einer GridView-Ansicht. Klicken Sie dann in Schritt 3 Fügen wir den Filter LinkButtons hinzu. Schritt 4 untersucht die gefilterten Ergebnissen paging. Die Schnittstelle, die in den Schritten 2 bis 4 erstellt wird in den nachfolgenden Lernprogrammen verwendet werden, um administrative Aufgaben für ein bestimmtes Benutzerkonto auszuführen.


## <a name="introduction"></a>Einführung

In der <a id="_msoanchor_1"> </a> [ *Zuweisen von Rollen an Benutzer* ](../roles/assigning-roles-to-users-vb.md) Tutorial, erstellt es eine einfache Schnittstelle für die ein Administrator einen Benutzer auswählen und ihre Rollen verwalten. Die Schnittstelle werden insbesondere den Administrator mit einer Dropdown-Liste aller Benutzer angezeigt. Eine solche Schnittstelle ist geeignet, wenn vorhanden sind, aber einem Dutzend Benutzer Konten, aber für Sites mit Hunderten oder Tausenden von Konten unhandlich ist. Ein Raster ausgelagerten, filterbar ist besser geeignet für Websites mit großer Benutzergruppen-Benutzeroberfläche.

In diesem Tutorial werden wir diese eine Benutzeroberfläche erstellen. Insbesondere wird unserer Benutzeroberfläche aus einer Reihe von LinkButtons bestehen, zum Filtern der Ergebnisse basierend auf den Buchstaben ab, der den Benutzernamen und ein GridView-Steuerelement, um die übereinstimmenden Benutzer anzuzeigen. Wir beginnen, durch die Liste aller der Benutzerkonten in einer GridView-Ansicht. Klicken Sie dann in Schritt 3 Fügen wir den Filter LinkButtons hinzu. Schritt 4 untersucht die gefilterten Ergebnissen paging. Die Schnittstelle, die in den Schritten 2 bis 4 erstellt wird in den nachfolgenden Lernprogrammen verwendet werden, um administrative Aufgaben für ein bestimmtes Benutzerkonto auszuführen.

Fangen wir an!

## <a name="step-1-adding-new-aspnet-pages"></a>Schritt 1: Hinzufügen von ASP.NET-Seiten

In diesem Tutorial und die nächsten beiden werden wir verschiedene Funktionen im Zusammenhang mit Verwaltung und Funktionen untersuchen. Wir benötigen eine Reihe von ASP.NET-Seiten in den Themen in diesen Tutorials untersucht implementieren. Diese Seiten zu erstellen, und die Sitemap zu aktualisieren.

Zunächst erstellen Sie einen neuen Ordner im Projekt mit dem Namen `Administration`. Fügen Sie zwei neue ASP.NET-Seiten in den Ordner, und verknüpfen jede Seite mit den `Site.master` Masterseite. Benennen Sie die Seiten ein:

- `ManageUsers.aspx`
- `UserInformation.aspx`

Hinzufügen von zwei Seiten auch zum Stammverzeichnis der Website: `ChangePassword.aspx` und `RecoverPassword.aspx`.

Diese vier Seiten sollten an diesem Punkt haben zwei Inhaltssteuerelementen stellt eine für jede von der Masterseite ContentPlaceHolder-Steuerelemente: `MainContent` und `LoginContent`.

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample1.aspx)]

Standard-Markup für die Masterseite anzeigen soll die `LoginContent` ContentPlaceHolder für diese Seiten. Daher entfernen deklarative Markup für die `Content2` Inhaltssteuerelement. Danach sollte die Seiten-Markup nur einem Inhaltssteuerelement enthalten.

Der ASP-Seiten in der `Administration` Ordner sind ausschließlich für Administratoren vorgesehen. Wir haben eine Rolle "Administratoren" hinzugefügt, auf das System in die <a id="_msoanchor_2"> </a> [ *erstellen und Verwalten von Rollen* ](../roles/creating-and-managing-roles-vb.md) Tutorials; Beschränken des Zugriffs auf diese beiden Seiten, die dieser Rolle. Zu diesem Zweck fügen eine `Web.config` -Datei in die `Administration` Ordner und konfigurieren Sie die `<authorization>` Element, um Benutzer in der Rolle "Administratoren" zugeben und alle anderen verweigern.

[!code-xml[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample2.xml)]

An diesem Punkt sollte Ihr Projekts des Projektmappen-Explorer im Screenshot dargestellt in Abbildung 1 ähneln.


[![Vier neue Seiten und eine Web.config-Datei wurden auf der Website hinzugefügt](building-an-interface-to-select-one-user-account-from-many-vb/_static/image2.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image1.png)

**Abbildung 1**: vier neue Seiten und ein `Web.config` Datei wurde auf der Website ([klicken Sie, um das Bild in voller Größe anzeigen](building-an-interface-to-select-one-user-account-from-many-vb/_static/image3.png))


Zum Schluss aktualisieren die Sitemap (`Web.sitemap`) auf einen Eintrag enthalten die `ManageUsers.aspx` Seite. Die folgenden XML-Code nach dem Hinzufügen der `<siteMapNode>` wir für die Lernprogramme Rollen hinzugefügt.

[!code-xml[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample3.xml)]

Besuchen Sie die Website über einen Browser, mit der sitezuordnung aktualisiert. Wie in Abbildung 2 gezeigt, enthält die Navigation auf der linken Seite nun Elemente für die Verwaltung Lernprogramme.


[![Die Sitemap enthält einen Knoten mit dem Titel Benutzerverwaltung](building-an-interface-to-select-one-user-account-from-many-vb/_static/image5.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image4.png)

**Abbildung 2**: die Sitemap enthält einen Knoten mit dem Titel Benutzerverwaltung ([klicken Sie, um das Bild in voller Größe anzeigen](building-an-interface-to-select-one-user-account-from-many-vb/_static/image6.png))


## <a name="step-2-listing-all-user-accounts-in-a-gridview"></a>Schritt 2: Auflisten aller Benutzerkonten in einer GridView-Ansicht

Unser Ziel für dieses Tutorial ist die Erstellung ein Rasters ausgelagertes, filterbar, über dem ein Administrator ein Benutzerkonto zum Verwalten von auswählen kann. Beginnen wir über eine Listung *alle* Benutzer in einer GridView-Ansicht. Sobald dies abgeschlossen ist, werden wir die Filterung und paging-Schnittstellen und Funktionen hinzufügen.

Öffnen der `ManageUsers.aspx` auf der Seite die `Administration` Ordner und das Hinzufügen einer GridView-Ansicht, Festlegen der `ID` zu `UserAccounts` In einem kurzen Moment schreiben Sie Code aus, um die Gruppe von Benutzerkonten mit GridView zu binden der `Membership` -Klasse `GetAllUsers` Methode. Wie in den vorherigen Tutorials erläutert die `GetAllUsers` Methode gibt eine `MembershipUserCollection` -Objekt, das eine Auflistung von `MembershipUser` Objekte. Jede `MembershipUser` in der Auflistung enthält Eigenschaften, z. B. `UserName`, `Email`, `IsApproved`und so weiter.

Um die gewünschten Informationen zum Benutzerkonto in der GridView anzuzeigen, legen Sie des GridView `AutoGenerateColumns` Eigenschaft auf "false" und fügen Sie BoundFields für die `UserName`, `Email`, und `Comment` Eigenschaften und CheckBoxFields für die `IsApproved`, `IsLockedOut`, und `IsOnline` Eigenschaften. Diese Konfiguration kann über deklaratives Markup des Steuerelements oder über das Dialogfeld Felder angewendet werden. Abbildung 3 zeigt einen Screenshot der Felder (Dialogfeld), nachdem die Felder automatisch generieren Kontrollkästchen deaktiviert wurde, und die BoundFields und CheckBoxFields hinzugefügt und konfiguriert wurden.


[![Hinzufügen von drei BoundFields und drei CheckBoxFields an die GridView](building-an-interface-to-select-one-user-account-from-many-vb/_static/image8.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image7.png)

**Abbildung 3**: drei BoundFields hinzufügen und drei CheckBoxFields an die GridView ([klicken Sie, um das Bild in voller Größe anzeigen](building-an-interface-to-select-one-user-account-from-many-vb/_static/image9.png))


Nach dem Konfigurieren Ihrer GridView, stellen Sie sicher, dass die deklarative Markup der folgenden ähnelt:

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample4.aspx)]

Als Nächstes müssen wir Code schreiben, die die Benutzerkonten an die GridView gebunden wird. Erstellen Sie eine Methode mit dem Namen `BindUserAccounts` diese Aufgabe auszuführen, und rufen Sie sie über die `Page_Load` Ereignishandler auf der Seite zum ersten Mal besuchen.

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample5.vb)]

Nehmen Sie einen Moment Zeit, um die Seite über einen Browser zu testen. Wie in Abbildung 4 gezeigt, die `UserAccounts` GridView Listet den Benutzernamen, e-Mail-Adresse und andere relevante Kontoinformationen für alle Benutzer im System.


[![In den GridView-Ansicht werden die Benutzerkonten aufgeführt.](building-an-interface-to-select-one-user-account-from-many-vb/_static/image11.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image10.png)

**Abbildung 4**: die Benutzerkonten finden Sie in den GridView-Ansicht ([klicken Sie, um das Bild in voller Größe anzeigen](building-an-interface-to-select-one-user-account-from-many-vb/_static/image12.png))


## <a name="step-3-filtering-the-results-by-the-first-letter-of-the-username"></a>Schritt 3: Filtern der Ergebnisse nach dem ersten Buchstaben des Benutzernamens

Derzeit den `UserAccounts` GridView zeigt *alle* der Benutzerkonten. Für Websites mit Hunderten oder Tausenden von Benutzerkonten ist es zwingend erforderlich, dass dieser Benutzer schnell kürzen Sie die angezeigten Konten können. Dies kann durch Hinzufügen der Filterung LinkButtons auf der Seite erfolgen. Fügen Sie auf der Seite 27 LinkButtons: eine mit dem Titel, alle zusammen mit einem LinkButton für jeden Buchstaben des Alphabets. Wenn ein Besucher alle LinkButton klickt, wird die GridView aller Benutzer angezeigt. Wenn sie einen bestimmten Buchstaben klicken, werden nur die Benutzer, deren Benutzernamen mit dem ausgewählten Buchstaben, angezeigt.

Unsere erste Aufgabe besteht darin, die 27 LinkButton-Steuerelemente hinzufügen. Eine Möglichkeit wäre die 27 LinkButtons deklarativ erstellen jeweils einzeln. Ein noch flexibler Ansatz ist die Verwendung von ein Repeater-Steuerelement mit einem `ItemTemplate` , rendert eine LinkButton und bindet dann die Filteroptionen, an der Repeater als eine `String` Array.

Beginnen Sie, indem ein Repeater-Steuerelement hinzufügen, um die zuvor auf der Seite die `UserAccounts` GridView. Festlegen des Repeaters `ID` Eigenschaft `FilteringUI` des Repeaters Vorlagen konfigurieren, damit die `ItemTemplate` rendert eine LinkButton, deren `Text` und `CommandName` Eigenschaften gebunden sind, auf das aktuelle Arrayelement. Wie in beschrieben der <a id="_msoanchor_3"> </a> [ *Zuweisen von Rollen an Benutzer* ](../roles/assigning-roles-to-users-vb.md) Tutorial, kann dies erreicht werden mithilfe der `Container.DataItem` Databinding-Syntax. Verwenden des Repeaters `SeparatorTemplate` eine vertikale Linie zwischen den einzelnen Links angezeigt.

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample6.aspx)]

Um diesen Repeater mit den gewünschten Filteroptionen aufzufüllen, erstellen Sie eine Methode namens `BindFilteringUI`. Achten Sie darauf, dass Sie zum Aufrufen dieser Methode aus der `Page_Load` Ereignishandler auf dem ersten Laden einer Seite.

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample7.vb)]

Diese Methode gibt die Filteroptionen als Elemente in der `String` Array `filterOptions` für jedes Element im Array, rendert der Repeater LinkButton mit seiner `Text` und `CommandName` Eigenschaften, die dem Wert des Arrays zugewiesen Element.

Abbildung 5 zeigt die `ManageUsers.aspx` Seite, wenn Sie über einen Browser angezeigt.


[![Repeater sind 27 Filtern LinkButtons aufgeführt.](building-an-interface-to-select-one-user-account-from-many-vb/_static/image14.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image13.png)

**Abbildung 5**: der Repeater listet 27 Filtern LinkButtons ([klicken Sie, um das Bild in voller Größe anzeigen](building-an-interface-to-select-one-user-account-from-many-vb/_static/image15.png))


> [!NOTE]
> Benutzernamen können mit einem beliebigen Zeichen, Zahlen und Interpunktionszeichen beginnen. Um diese Konten anzuzeigen, wird der Administrator verfügen, die alle LinkButton-Option verwenden. Alternativ können Sie durch Hinzufügen eine LinkButton, um alle Benutzerkonten zurück, die mit einer Zahl beginnen. Ich lassen Sie dieses als Übung für den Leser.


Klicken auf die Filterung LinkButtons ein Postback auslöst, und löst des Repeaters `ItemCommand` -Ereignis, aber es keine Änderung im Raster, gibt da wir noch zum haben Schreiben von Code zum Filtern der Ergebnisse. Die `Membership` Klasse enthält eine [ `FindUsersByName` Methode](https://technet.microsoft.com/library/system.web.security.membership.findusersbyname.aspx) , die diese Benutzerkonten, deren Benutzername mit einem angegebenen Suchmuster übereinstimmt, zurückgibt. Wir können diese Methode verwenden, um nur die Benutzerkonten abzurufen, deren Benutzernamen zu, mit dem Buchstaben gemäß starten, der `CommandName` von gefilterten LinkButton, auf die geklickt wurde.

Aktualisieren Sie zunächst die `ManageUser.aspx` Seite des Code-Behind-Klasse, sodass es sich um eine Eigenschaft, die mit dem Namen enthält `UsernameToMatch` dieser Eigenschaft beibehalten der Benutzernamen-Zeichenfolge nach dem zurücksenden:

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample8.vb)]

Die `UsernameToMatch` Eigenschaft speichert dessen Wert, der er, in zugewiesen ist der `ViewState` Auflistung unter Verwendung des Schlüssels UsernameToMatch. Wenn der Wert dieser Eigenschaft gelesen wird, überprüft er, um festzustellen, ob ein Wert vorhanden, in ist der `ViewState` Auflistung; Falls nicht, es gibt zurück, den Standardwert eine leere Zeichenfolge. Die `UsernameToMatch` Eigenschaft weist ein allgemeines Muster, d. h. Beibehaltung einen Wert zum Anzeigen des Status, sodass alle Änderungen an die Eigenschaft über Postbacks hinweg beibehalten werden. Finden Sie weitere Informationen zu diesem Muster, [Verständnis ASP.NET-Ansichtszustand](https://msdn.microsoftn-us/library/ms972976.aspx).

Aktualisieren Sie als Nächstes die `BindUserAccounts` Methode so, dass anstelle eines Aufrufs `Membership.GetAllUsers`, ruft `Membership.FindUsersByName`, und übergeben Sie den Wert des der `UsernameToMatch` -Eigenschaft mit dem SQL-Platzhalterzeichen %.

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample9.vb)]

Um nur die Benutzer anzuzeigen, deren Benutzernamen mit dem Buchstaben A, legen die `UsernameToMatch` Eigenschaft, um ein und rufen Sie dann `BindUserAccounts` führt dies zu einem Aufruf von `Membership.FindUsersByName("A%")`, die alle Benutzer zurückgibt, deren Benutzername mit A. ebenso zurückzugebenden beginnt*alle* eine leere Zeichenfolge zum Zuweisen von Benutzern, die `UsernameToMatch` Eigenschaft, damit die `BindUserAccounts` Methode wird aufgerufen `Membership.FindUsersByName("%")`, wodurch alle Benutzerkonten zurückgeben.

Erstellen Sie einen Ereignishandler für das Repeater `ItemCommand` Ereignis. Dieses Ereignis wird ausgelöst, wenn eine des Filters LinkButtons geklickt wird; Es wird das Klicken auf LinkButton übergeben `CommandName` Wert über die `RepeaterCommandEventArgs` Objekt. Wir müssen den entsprechenden Wert zuweisen der `UsernameToMatch` -Eigenschaft, und rufen Sie dann die `BindUserAccounts` Methode. Wenn die `CommandName` ist, weisen Sie eine leere Zeichenfolge `UsernameToMatch` , damit alle Benutzerkonten angezeigt werden. Ordnen Sie andernfalls die `CommandName` Wert `UsernameToMatch`

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample10.vb)]

Testen Sie mit diesem Code werden die Funktion zum Filtern. Wenn die Seite zuerst aufgerufen wird, werden alle Benutzerkonten angezeigt (siehe Abbildung 5). Klicken auf LinkButton ein ein Postback auslöst und filtert die Ergebnisse, die Anzeige nur der Benutzerkonten, die mit A zu starten.


[![Verwenden Sie die Filterung LinkButtons, um die Benutzer anzuzeigen, deren Benutzernamen mit einem bestimmten Buchstaben](building-an-interface-to-select-one-user-account-from-many-vb/_static/image17.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image16.png)

**Abbildung 6**: Verwenden Sie die LinkButtons filtern, um diese Benutzer, deren Benutzername mit einem bestimmte Buchstaben beginnt anzuzeigen ([klicken Sie, um das Bild in voller Größe anzeigen](building-an-interface-to-select-one-user-account-from-many-vb/_static/image18.png))


## <a name="step-4-updating-the-gridview-to-use-paging"></a>Schritt 4: Aktualisieren der GridView zur Verwendung von Paging

In den Abbildungen 5 und 6 gezeigten GridView zeigt eine Liste aller vom zurückgegebenen Datensätze der `FindUsersByName` Methode. Wenn es Hunderte oder Tausende von Benutzerkonten gibt kann dies Informationsflut führen, wenn Sie alle Konten anzeigen (wie der Fall, wenn Sie alle LinkButton klicken, oder beim Zugriff auf die Seite anfänglich ist). Damit können die Benutzerkonten in besser verwaltbare Segmente vorhanden, konfigurieren die GridView um 10 Benutzerkonten zu einem Zeitpunkt anzuzeigen.

Das GridView-Steuerelement bietet zwei Arten von Paging:

- **Das Standardpaging** – leicht zu implementieren, jedoch ineffizient. Kurz gesagt, Standardwert GridView paging erwartet *alle* Datensätze aus der Datenquelle. Es zeigt dann nur die entsprechende Seite mit Datensätzen.
- **Benutzerdefiniertes Paging** -erfordert mehr Arbeit zu implementieren, jedoch ist effizienter als die Standardnavigation, da das Auslagern der Daten benutzerdefinierte Quelle nur den genauen Satz von anzuzeigenden Datensätze zurückgibt.

Der Leistungsunterschied zwischen Standard- und benutzerdefinierte Paginierung kann recht umfangreich sein, wenn paging durch Tausenden von Datensätzen. Da wir erstellen diese Schnittstelle, vorausgesetzt, dass es möglicherweise Hunderte oder Tausende von Benutzerkonten, verwenden Sie benutzerdefiniertes Paging.

> [!NOTE]
> Eine ausführlichere Erläuterung zu den Unterschieden zwischen der standardmäßige und benutzerdefinierte Paginierung als auch die Herausforderungen beim Implementieren von benutzerdefiniertem Paging, finden Sie unter [effizient Paging durch große Mengen von Daten](https://asp.net/learn/data-access/tutorial-25-vb.aspx). Eine Analyse der Leistungsunterschied zwischen Standard- und benutzerdefinierte Paginierung, finden Sie unter [benutzerdefiniertes Paging in ASP.NET mit SQL Server 2005](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx).


Benutzerdefiniertes Paging implementieren, das wir einen Mechanismus zunächst, um die genaue Teilmenge der Datensätze, die vom GridView angezeigten abzurufen. Die gute Nachricht ist, die die `Membership` Klasse `FindUsersByName` Methode verfügt über eine Überladung, die zum Angeben der Seitenindex und "Seitengröße" ermöglicht und gibt nur die Benutzerkonten, die innerhalb dieses Bereichs von Datensätzen.

Insbesondere diese Überladung hat die folgende Signatur: [ `FindUsersByName(usernameToMatch, pageIndex, pageSize, totalRecords)` ](https://msdn.microsoft.com/library/fa5st8b2.aspx).

Die *PageIndex* Parameter gibt an, die Seite von Benutzerkonten für zurückgegeben wird. *PageSize* gibt an, wie viele Datensätze pro Seite angezeigt werden sollen. Die *TotalRecords* -Parameter ist ein `ByRef` Parameter, der die Anzahl der insgesamt von Benutzerkonten in den Speicher des Benutzers zurückgegeben.

> [!NOTE]
> Vom zurückgegebenen Daten `FindUsersByName` Username; sortiert ist die Sortierkriterien können nicht angepasst werden.


GridView kann konfiguriert werden, um benutzerdefiniertes Paging verwendet, aber nur bei der Bindung an ein ObjectDataSource-Steuerelement. Für das ObjectDataSource-Steuerelement benutzerdefinierte Paginierung implementieren, ist es erforderlich, zwei Methoden: eine, die übergeben wird, ein startIndex für die Zeile und die maximale Anzahl von Datensätzen angezeigt werden, und gibt die genaue Teilmenge der Datensätze, die in dieser Spanne liegen und eine Methode, die die Gesamtzahl der Datensätze zurückgibt, wird über ausgelagert. Die `FindUsersByName` Überladung akzeptiert ein Seitenindex "und" Seitengröße und gibt die Gesamtzahl der Datensätze über eine `ByRef` Parameter. Es gibt hier eine Schnittstelle-Konflikt.

Eine Möglichkeit wäre eine Proxyklasse zu erstellen, die die Schnittstelle verfügbar macht, das "ObjectDataSource" erwartet, und ruft dann intern, den `FindUsersByName` Methode. Eine weitere Option- und eine, die wir für diesen Artikel verwende - ist unserer eigenen Paging-Schnittstelle erstellen und verwenden diesen anstatt GridView integrierte Paging-Schnittstelle.

### <a name="creating-a-first-previous-next-last-paging-interface"></a>Erstellen einer ersten, vorherigen, als Nächstes letzte Paging-Schnittstelle

Erstellen Sie eine Paging-Schnittstelle mit dem ersten "," zurück "," Weiter "und" letzte LinkButtons. Erste LinkButton geklickt haben, dauert den Benutzer die erste Seite der Daten, während er zurück zur vorherigen Seite zurückgegeben wird. Ebenso wird weiter "und" Last der Benutzer verschieben zur nächsten und letzten Seite bzw. Fügen Sie die vier LinkButton-Steuerelemente unter dem `UserAccounts` GridView.

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample11.aspx)]

Als Nächstes erstellen Sie einen Ereignishandler für die einzelnen des LinkButton `Click` Ereignisse.

Abbildung 7 zeigt die vier LinkButtons, wenn Sie über die Visual Web Developer-Design-Sicht angezeigt.


[![Fügen Sie der ersten, vorherigen hinzu, weiter, und der letzten Sie LinkButtons unterhalb der GridView](building-an-interface-to-select-one-user-account-from-many-vb/_static/image20.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image19.png)

**Abbildung 7**: erste hinzufügen, zurück, nächsten und letzten LinkButtons unterhalb der GridView ([klicken Sie, um das Bild in voller Größe anzeigen](building-an-interface-to-select-one-user-account-from-many-vb/_static/image21.png))


### <a name="keeping-track-of-the-current-page-index"></a>Nachverfolgen der Index der aktuellen Seite

Wenn ein Benutzer erstmals besucht die `ManageUsers.aspx` Seite oder -Klicks die Filterung eines Schaltflächen, die wir die erste Seite der Daten in den GridView-Ansicht angezeigt werden soll. Klickt der Benutzer eine der Navigation LinkButtons, müssen wir jedoch den Seitenindex zu aktualisieren. Um den Seitenindex und die Anzahl der pro Seite anzuzeigenden Datensätze zu gewährleisten, fügen Sie die folgenden beiden Eigenschaften auf der Seite Code-Behind-Klasse hinzu:

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample12.vb)]

Wie die `UsernameToMatch` -Eigenschaft, die `PageIndex` Eigenschaft beibehalten, dessen Wert Ansichtszustand. Die schreibgeschützte `PageSize` Eigenschaft gibt einen hartcodierten Wert 10. Ich lade den interessierten Leser, aktualisieren Sie diese Eigenschaft, um das gleiche Muster wie verwenden `PageIndex`, und klicken Sie dann zum Erweitern der `ManageUsers.aspx` Seite so, dass die Person, die auf der Seite angeben kann, wie viele Benutzerkonten pro anzuzeigenden Seite.

### <a name="retrieving-just-the-current-pages-records-updating-the-page-index-and-enabling-and-disabling-the-paging-interface-linkbuttons"></a>Nur die aktuelle Seite Datensätze abzurufen, den Seitenindex, aktualisieren und aktivieren und Deaktivieren der Auslagerung Schnittstelle LinkButtons

Mit vorhandener Schnittstelle Paging und die `PageIndex` und `PageSize` Eigenschaften hinzugefügt, wir sind bereit zum Aktualisieren der `BindUserAccounts` Methode, sodass die It die entsprechende verwendet `FindUsersByName` überladen. Darüber hinaus benötigen wir diese Methode aktiviert oder deaktiviert die Paging-Schnittstelle, je nachdem auf welcher Seite angezeigt wird. Die erste ' und ' Vorheriger Links sollte deaktiviert werden, wenn Sie die erste Seite der Daten anzeigen zu können, Als Nächstes und zuletzt sollte deaktiviert werden, wenn die letzte Seite anzeigen.

Aktualisieren Sie die `BindUserAccounts`-Methode mit folgendem Code:

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample13.vb)]

Beachten Sie, dass die Gesamtzahl der Datensätze, die über ausgelagerte, durch den letzten Parameter von bestimmt wird der `FindUsersByName` Methode. Nachdem die angegebene Seite von Benutzerkonten werden zurückgegeben, werden, je nachdem, ob der ersten bzw. letzten Datenseite angezeigt wird die vier LinkButtons entweder aktiviert oder deaktiviert.

Der letzte Schritt ist, Code zu schreiben, für die vier LinkButtons' `Click` -Ereignishandler. Müssen Sie diese Ereignishandler zum Aktualisieren der `PageIndex` Eigenschaft und dann binden Sie die Daten an die GridView über einen Aufruf an `BindUserAccounts` der ersten zurück und den nächsten Ereignishandler sind sehr einfach. Die `Click` -Ereignishandler für die letzten LinkButton, allerdings etwas komplexer ist, da wir benötigen, um zu bestimmen, um den Index der letzten Seite zu ermitteln, wie viele Datensätze angezeigt werden.

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample14.vb)]

Abbildungen 8 und 9 zeigen die benutzerdefinierte Paginierung-Schnittstelle in Aktion. Abbildung 8 zeigt die `ManageUsers.aspx` Seite, wenn Sie die erste Seite der Daten für alle Benutzerkonten anzeigen. Beachten Sie, dass nur 10 13 Konten angezeigt werden. Klicken auf den Link weiter oder letzte bewirkt, dass ein Postback, Updates der `PageIndex` 1 und Bindungen, die in das Raster die zweite Seite des Benutzer-Konten (siehe Abbildung 9).


[![Die ersten 10 Benutzerkonten werden angezeigt.](building-an-interface-to-select-one-user-account-from-many-vb/_static/image23.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image22.png)

**Abbildung 8**: die ersten 10 Benutzerkonten werden angezeigt ([klicken Sie, um das Bild in voller Größe anzeigen](building-an-interface-to-select-one-user-account-from-many-vb/_static/image24.png))


[![Klicken Sie auf den nächsten Link zeigt die zweite Seite des Benutzerkonten](building-an-interface-to-select-one-user-account-from-many-vb/_static/image26.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image25.png)

**Abbildung 9**: Klicken Sie auf den nächsten Link zeigt die zweite Seite von Benutzerkonten ([klicken Sie, um das Bild in voller Größe anzeigen](building-an-interface-to-select-one-user-account-from-many-vb/_static/image27.png))


## <a name="summary"></a>Zusammenfassung

Administratoren müssen häufig einen Benutzer aus der Liste der Konten auswählen. In vorherigen Tutorials erläutert, mit einer Dropdown-Liste mit den Benutzern aufgefüllt, aber dieser Ansatz ist nicht gut skalierbar. In diesem Tutorial haben wir untersucht, eine bessere Alternative: eine filterbar-Schnittstelle, deren Ergebnisse in einem ausgelagerten GridView-Ansicht angezeigt werden. Mit dieser Benutzeroberfläche können Administratoren immer dann schnell und effizient suchen und wählen ein Benutzerkonto, unter den Tausenden.

Viel Spaß beim Programmieren!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Tutorial erläutert finden Sie in den folgenden Ressourcen:

- [Benutzerdefiniertes Paging in ASP.NET mit SQLServer 2005](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx)
- [Effizientes Auslagern von großen Mengen von Daten](https://asp.net/learn/data-access/tutorial-25-vb.aspx)
- [Paralleles eigene Websiteverwaltungs-Tool](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)

### <a name="about-the-author"></a>Der Autor

Scott Mitchell, Autor von mehreren Büchern zu ASP/ASP.NET und Gründer von 4GuysFromRolla.com, ist seit 1998 mit Microsoft-Web-Technologien gearbeitet. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird *[Sams Schulen selbst ASP.NET 2.0 in 24 Stunden](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott erreicht werden kann, zur [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) oder über seinen Blog unter [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Besonderen Dank an

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führendes Prüfer für dieses Tutorial wurde Alicja Maziarz. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an

> [!div class="step-by-step"]
> [Zurück](unlocking-and-approving-user-accounts-cs.md)
> [Weiter](recovering-and-changing-passwords-vb.md)
