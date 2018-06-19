---
uid: web-forms/overview/older-versions-security/admin/building-an-interface-to-select-one-user-account-from-many-cs
title: Erstellen eine Schnittstelle zum Auswählen eines Benutzerkontos aus vielen (c#) | Microsoft Docs
author: rick-anderson
description: In diesem Lernprogramm wird eine Benutzeroberfläche mit einem ausgelagerten, filterable Raster erstellen. Insbesondere wird eine Reihe von LinkButtons für unsere Benutzeroberfläche bestehen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2008
ms.topic: article
ms.assetid: 9e4e687c-b4ec-434f-a4ef-edb0b8f365e4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/admin/building-an-interface-to-select-one-user-account-from-many-cs
msc.type: authoredcontent
ms.openlocfilehash: 304505b18e330425ea1dc8df87a552f3d8cd15f3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890670"
---
<a name="building-an-interface-to-select-one-user-account-from-many-c"></a>Erstellen eine Schnittstelle zum Auswählen eines Benutzerkontos aus vielen (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Herunterladen von Code](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/CS.12.zip) oder [PDF herunterladen](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial12_SelectUser_cs.pdf)

> In diesem Lernprogramm wird eine Benutzeroberfläche mit einem ausgelagerten, filterable Raster erstellen. Insbesondere wird unsere Benutzeroberfläche zum Filtern der Ergebnisse basierend auf dem ersten Buchstaben des Benutzernamens und eines GridView-Steuerelements können Sie die übereinstimmenden Benutzer Anzeigen einer Reihe von LinkButtons bestehen. Wir beginnen mit den in der alle Benutzerkonten in einem GridView aufgelistet. In Schritt 3, werden wir Sie dann den Filter LinkButtons hinzufügen. Schritt 4 prüft die gefilterten Ergebnissen paging. Die Schnittstelle, die in den Schritten 2 bis 4 konstruiert wird in den nachfolgenden Lernprogrammen für administrative Aufgaben für ein bestimmtes Benutzerkonto verwendet werden.


## <a name="introduction"></a>Einführung

In der <a id="_msoanchor_1"> </a> [ *Zuweisen von Rollen an Benutzer* ](../roles/assigning-roles-to-users-cs.md) Lernprogramm, erstellt es eine rudimentäre Schnittstelle für einen Administrator, wählen Sie einen Benutzer und ihre Rollen verwalten. Die Schnittstelle werden insbesondere den Administrator eine Dropdown-Liste mit allen Benutzern angezeigt. Eine solche Schnittstelle ist geeignet, wenn vorhanden sind, aber einem Dutzend Benutzer Konten, aber wird unhandlich für Sites mit Hunderten oder Tausenden von Konten. Ein Raster ausgelagerten, filterbar ist besser geeignet Benutzeroberfläche für Websites, die große Anzahl von Benutzern.

In diesem Lernprogramm werden solche eine Benutzeroberfläche erstellen. Insbesondere wird unsere Benutzeroberfläche zum Filtern der Ergebnisse basierend auf dem ersten Buchstaben des Benutzernamens und eines GridView-Steuerelements können Sie die übereinstimmenden Benutzer Anzeigen einer Reihe von LinkButtons bestehen. Wir beginnen mit den in der alle Benutzerkonten in einem GridView aufgelistet. In Schritt 3, werden wir Sie dann den Filter LinkButtons hinzufügen. Schritt 4 prüft die gefilterten Ergebnissen paging. Die Schnittstelle, die in den Schritten 2 bis 4 konstruiert wird in den nachfolgenden Lernprogrammen für administrative Aufgaben für ein bestimmtes Benutzerkonto verwendet werden.

Fangen wir an!

## <a name="step-1-adding-new-aspnet-pages"></a>Schritt 1: Hinzufügen von neuen ASP.NET-Seiten

In diesem Lernprogramm und die nächsten beiden werden wir verschiedene Funktionen im Zusammenhang mit Verwaltung und Funktionen untersuchen. Wir benötigen eine Reihe von ASP.NET-Seiten in den Themen in der gesamten Ausführung dieser Lernprogramme untersucht implementieren. Wir diese Seiten erstellen und Aktualisieren der Siteübersicht.

Starten, indem Sie einen neuen Ordner erstellen, in das Projekt mit dem Namen `Administration`. Fügen Sie zwei neue ASP.NET-Seiten zu dem Ordner, verknüpfen jede Seite mit den `Site.master` Masterseite. Benennen Sie die Seiten:

- `ManageUsers.aspx`
- `UserInformation.aspx`

Auch zwei Seiten zum Stammverzeichnis der Website hinzuzufügen: `ChangePassword.aspx` und `RecoverPassword.aspx`.

Diese vier Seiten, an diesem Punkt müssen zwei Inhaltssteuerelementen stellt jeweils einen für die Gestaltungsvorlage ContentPlaceHolders: `MainContent` und `LoginContent`.

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample1.aspx)]

Wir Standardmarkup für die Gestaltungsvorlage anzuzeigenden der `LoginContent` ContentPlaceHolder für diese Seiten. Daher sollten Sie entfernen das deklarative Markup für die `Content2` Inhaltssteuerelement. Nachdem Sie auf diese Weise sollte Markup der Seiten nur ein Inhaltssteuerelement enthalten.

Der ASP-Seiten in der `Administration` Ordner sind ausschließlich für Administratoren vorgesehen. Wir das System in einer Administratorrolle hinzugefügt der <a id="_msoanchor_2"> </a> [ *erstellen und Verwalten von Rollen* ](../roles/creating-and-managing-roles-cs.md) Lernprogramm; Beschränken des Zugriffs auf diese beiden Seiten, die dieser Rolle. Um dies zu erreichen, fügen eine `Web.config` Datei wird in der `Administration` Ordner und Konfigurieren der `<authorization>` Element um sämtlichen Benutzern in der "Administratoren" und alle anderen verweigern.

[!code-xml[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample2.xml)]

An diesem Punkt sollte Projektmappen-Explorer des Projekts im Screenshot, der in Abbildung 1 dargestellt aussehen.


[![Vier neue Seiten und eine Datei "Web.config" wurden der Website hinzugefügt](building-an-interface-to-select-one-user-account-from-many-cs/_static/image2.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image1.png)

**Abbildung 1**: vier neue Seiten und eine `Web.config` Datei wurden hinzugefügt, um die Website ([klicken Sie hier, um das Bild in voller Größe angezeigt](building-an-interface-to-select-one-user-account-from-many-cs/_static/image3.png))


Zum Schluss aktualisieren die Siteübersicht (`Web.sitemap`) auf einen Eintrag enthalten die `ManageUsers.aspx` Seite. Die folgenden XML-Code nach dem Hinzufügen der `<siteMapNode>` wir für die Lernprogramme Rollen hinzugefügt.

[!code-xml[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample3.xml)]

Suchen Sie mit der Siteübersicht aktualisiert die Website über einen Browser aus. Wie in Abbildung 2 gezeigt, enthält die Navigation auf der linken Seite Elemente für die Verwaltung Lernprogramme.


[![Die Siteübersicht enthält einen Knoten mit dem Titel Benutzerverwaltung](building-an-interface-to-select-one-user-account-from-many-cs/_static/image5.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image4.png)

**Abbildung 2**: die Siteübersicht enthält einen Knoten mit dem Titel Benutzerverwaltung ([klicken Sie hier, um das Bild in voller Größe angezeigt](building-an-interface-to-select-one-user-account-from-many-cs/_static/image6.png))


## <a name="step-2-listing-all-user-accounts-in-a-gridview"></a>Schritt 2: Auflisten aller Benutzerkonten in einer GridView

Unser Endziel "für dieses Lernprogramm ist einem ausgelagerten, filterable Raster erstellen, über die ein Administrator ein Benutzerkonto verwalten auswählen kann. Beginnen wir mit dem Angebot *alle* Benutzer in einem GridView. Sobald dies abgeschlossen ist, werden wir die Filterung und paging Schnittstellen und Funktionen hinzufügen.

Öffnen der `ManageUsers.aspx` auf der Seite der `Administration` Ordner und fügen Sie eine GridView Festlegen seiner `ID` auf `UserAccounts`. Warten Sie einen Augenblick, Schreiben Sie Code aus, um den Satz von Benutzerkonten an die GridView mit Binden der `Membership` Klasse `GetAllUsers` Methode. Wie in früheren Lernprogrammen erläutert wird, gibt die GetAllUsers-Methode eine `MembershipUserCollection` -Objekt, das eine Auflistung von `MembershipUser` Objekte. Jede `MembershipUser` in der Auflistung enthält Eigenschaften, z. B. `UserName`, `Email`, `IsApproved`und so weiter.

Um die gewünschten Benutzerkontoinformationen in der GridView anzuzeigen, legen Sie der GridView `AutoGenerateColumns` Eigenschaft auf "false" und fügen Sie BoundFields für die `UserName`, `Email`, und `Comment` Eigenschaften und CheckBoxFields für die `IsApproved`, `IsLockedOut`, und `IsOnline` Eigenschaften. Diese Konfiguration kann durch deklaratives Markup, das Steuerelement oder über das Dialogfeld Felder angewendet werden. Abbildung 3 zeigt einen Screenshot der Felder (Dialogfeld), nachdem die Felder automatisch generiert Kontrollkästchen deaktiviert wurde und die BoundFields und CheckBoxFields hinzugefügt und konfiguriert wurden.


[![Hinzufügen von drei BoundFields und drei CheckBoxFields an die GridView](building-an-interface-to-select-one-user-account-from-many-cs/_static/image8.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image7.png)

**Abbildung 3**: drei BoundFields hinzufügen und drei CheckBoxFields an die GridView ([klicken Sie hier, um das Bild in voller Größe angezeigt](building-an-interface-to-select-one-user-account-from-many-cs/_static/image9.png))


Nach dem Konfigurieren der GridView, stellen Sie sicher, dass seine deklarative Markup der folgenden ähnelt:

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample4.aspx)]

Als Nächstes müssen es, Code zu schreiben, die die Benutzerkonten an die GridView gebunden. Erstellen Sie eine Methode mit dem Namen `BindUserAccounts` führen Sie diese Aufgabe aus, und rufen Sie anschließend aus der `Page_Load` Ereignishandler ersten Besuch der Seite.

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample5.cs)]

Nehmen Sie einen Moment Zeit, um die Seite über einen Browser zu testen. Wie in Abbildung 4 gezeigt, die `UserAccounts` GridView Listet den Benutzernamen, e-Mail-Adresse und andere relevante Informationen für alle Benutzer im System.


[![Die Benutzerkonten werden in die GridView aufgeführt.](building-an-interface-to-select-one-user-account-from-many-cs/_static/image11.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image10.png)

**Abbildung 4**: der Benutzerkonten sind aufgeführt, in der GridView ([klicken Sie hier, um das Bild in voller Größe angezeigt](building-an-interface-to-select-one-user-account-from-many-cs/_static/image12.png))


## <a name="step-3-filtering-the-results-by-the-first-letter-of-the-username"></a>Schritt 3: Filtern der Ergebnisse nach dem ersten Buchstaben des Benutzernamens

Derzeit die `UserAccounts` GridView zeigt *alle* der Benutzerkonten. Websites mit Hunderten oder Tausenden von Benutzerkonten ist es erforderlich, dass dieser Benutzer kann schnell nach unten angezeigten Konten zu kürzen. Dies kann durch Hinzufügen von Filtern LinkButtons zur Seite erfolgen. Fügen Sie auf der Seite 27 LinkButtons: eine mit dem Titel alle zusammen mit einem LinkButton für jeden Buchstaben des Alphabets. Wenn ein Besucher alle LinkButton klickt, wird die GridView aller Benutzer angezeigt. Wenn sie einen bestimmten Buchstaben klicken, werden nur die Benutzer, deren Benutzername mit dem ausgewählten Buchstaben beginnt, angezeigt.

Unsere erste Aufgabe besteht darin die 27 LinkButton Steuerelemente hinzuzufügen. Eine Möglichkeit wäre die 27 LinkButtons deklarativ erstellen jeweils einzeln. Ein noch flexibler Ansatz ist die Verwendung einer Wiederholungsmodul-Steuerelement mit einer `ItemTemplate` , die LinkButton rendert und klicken Sie dann die Filteroptionen an Repeater als bindet ein `string` Array.

Beginnen Sie, indem eine Wiederholungsmodul-Steuerelement auf der Seite "oben" Hinzufügen der `UserAccounts` GridView. Legen Sie die Repeater `ID` Eigenschaft `FilteringUI`. Die Repeater Vorlagen konfigurieren, damit seine `ItemTemplate` LinkButton rendert, deren `Text` und `CommandName` Eigenschaften für das aktuelle Arrayelement gebunden sind. Wie wir gesehen, in haben der <a id="_msoanchor_3"> </a> [ *Zuweisen von Rollen an Benutzer* ](../roles/assigning-roles-to-users-cs.md) Tutorial, dies geschieht mithilfe der `Container.DataItem` Databinding-Syntax. Verwenden Sie die Repeater `SeparatorTemplate` eine vertikale Linie zwischen jeder Link angezeigt.

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample6.aspx)]

Um diese Repeater mit den gewünschten Filteroptionen aufzufüllen, erstellen Sie eine Methode namens `BindFilteringUI`. Achten Sie darauf, dass Sie zum Aufrufen dieser Methode aus der `Page_Load` Ereignishandler beim ersten Laden der Seite.

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample7.cs)]

Diese Methode gibt die Filteroptionen als Elemente in der `string` Array `filterOptions`. Für jedes Element im Array, rendert die Repeater LinkButton mit seiner `Text` und `CommandName` Eigenschaften, die dem Wert des Arrayelements zugewiesen.

Abbildung 5 zeigt die `ManageUsers.aspx` Seite, wenn Sie über einen Browser angezeigt.


[![Die Repeater listet 27 Filtern LinkButtons](building-an-interface-to-select-one-user-account-from-many-cs/_static/image14.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image13.png)

**Abbildung 5**: die Repeater listet 27 Filtern LinkButtons ([klicken Sie hier, um das Bild in voller Größe angezeigt](building-an-interface-to-select-one-user-account-from-many-cs/_static/image15.png))


> [!NOTE]
> Benutzernamen können mit einem beliebigen Zeichen, einschließlich Ziffern und Satzzeichen beginnen. Um diese Konten anzuzeigen, müssen der Administrator die Option alle LinkButton verwenden. Alternativ konnte eine LinkButton, um alle Benutzerkonten zurück, die mit einer Zahl beginnen hinzugefügt werden. Ich lassen Sie dieses als Übung für den Leser.


Durch Klicken auf die Filterung LinkButtons einen Postback verursacht, und löst die Repeater `ItemCommand` Ereignis, aber es keine Änderung im Raster, gibt da wir noch zu haben schreiben Sie Code um die Ergebnisse zu filtern. Die `Membership` Klasse enthält eine [ `FindUsersByName` Methode](https://technet.microsoft.com/library/system.web.security.membership.findusersbyname.aspx) , die diese Benutzerkonten, deren Benutzername einem angegebenen Suchmuster entspricht, zurückgibt. Wir können diese Methode verwenden, um nur die Benutzerkonten abzurufen, dessen Benutzernamen zu, mit dem Buchstaben gemäß starten, der `CommandName` der gefilterten LinkButton, auf die geklickt wurde.

Starten mit dem Aktualisieren der `ManageUser.aspx` Seite des Code-Behind-Klasse, enthalten eine Eigenschaft namens `UsernameToMatch`. Diese Eigenschaft weiterhin die Filterzeichenfolge Benutzernamen über Postbacks besteht:

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample8.cs)]

Die `UsernameToMatch` Eigenschaft speichert den Wert, in zugewiesen wird der `ViewState` Auflistung unter Verwendung des Schlüssels UsernameToMatch. Wenn der Wert dieser Eigenschaft gelesen wird, er überprüft, um festzustellen, ob ein Wert vorhanden, in ist der `ViewState` Auflistung ist; falls nicht, es der Standardwert, eine leere Zeichenfolge zurückgegeben. Die `UsernameToMatch` Eigenschaft weist ein allgemeines Muster, nämlich Beibehaltung einen Wert zum Anzeigen des Status, sodass alle Änderungen an die Eigenschaft über Postbacks hinweg beibehalten werden. Weitere Informationen zu diesem Muster finden Sie unter [Verständnis ASP.NET-Ansichtszustand](https://msdn.microsoft.com/library/ms972976.aspx).

Aktualisieren Sie als Nächstes die `BindUserAccounts` Methode, damit der Aufruf von `Membership.GetAllUsers`, ruft er `Membership.FindUsersByName`, und übergeben Sie den Wert des der `UsernameToMatch` Eigenschaft mit dem SQL-Platzhalterzeichen angefügt %.

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample9.cs)]

Nur für diese Benutzer anzeigen, deren Benutzername mit dem Buchstaben A beginnt, legen Sie die `UsernameToMatch` Eigenschaft, um ein, und rufen Sie anschließend `BindUserAccounts`. Dies führt zu einem Aufruf von `Membership.FindUsersByName("A%")`, dem alle Benutzer zurückgegeben wird, deren Benutzername mit A. ebenso zurückzugebenden startet *alle* Zuweisen von Benutzern, eine leere Zeichenfolge, die `UsernameToMatch` Eigenschaft, damit die `BindUserAccounts` Methode wird Aufrufen `Membership.FindUsersByName("%")`, wodurch alle Benutzerkonten zurückgeben.

Erstellen Sie einen Ereignishandler für das Repeater `ItemCommand` Ereignis. Dieses Ereignis wird ausgelöst, wenn eine des Filters LinkButtons geklickt wird; die angeklickte LinkButton übergeben `CommandName` Wert über die `RepeaterCommandEventArgs` Objekt. Wir müssen die entsprechenden Wert zuzuweisen der `UsernameToMatch` -Eigenschaft, und rufen Sie dann die `BindUserAccounts` Methode. Wenn die `CommandName` alle "," ist eine leere Zeichenfolge zum Zuweisen `UsernameToMatch` so, dass alle Benutzerkonten angezeigt werden. Ordnen Sie andernfalls die `CommandName` Wert `UsernameToMatch`.

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample10.cs)]

Testen Sie mit diesem Code werden die Filterfunktionen zur Verfügung. Wenn die Seite zuerst aufgerufen wird, werden alle Benutzerkonten angezeigt (siehe Abbildung 5). Klicken auf ein LinkButton einen Postback verursacht und filtert die Ergebnisse, die nur die Benutzerkonten, die mit A beginnen anzeigen.


[![Verwenden Sie die Filterung LinkButtons diese Benutzer angezeigt, deren Benutzername mit einem bestimmten Buchstaben beginnt](building-an-interface-to-select-one-user-account-from-many-cs/_static/image17.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image16.png)

**Abbildung 6**: Verwenden Sie die LinkButtons filtern, um diese Benutzer, deren Benutzername mit einem bestimmten Buchstaben beginnt anzuzeigen ([klicken Sie hier, um das Bild in voller Größe angezeigt](building-an-interface-to-select-one-user-account-from-many-cs/_static/image18.png))


## <a name="step-4-updating-the-gridview-to-use-paging"></a>Schritt 4: Aktualisieren der GridView Verwendung Paging

Die Zahlen 5 und 6 gezeigt GridView zeigt eine Liste aller vom zurückgegebenen Datensätze der `FindUsersByName` Methode. Wenn es Hunderte oder Tausende von Benutzerkonten gibt, kann dies zu Informationen Überladung führen, wenn Sie alle Konten (wie die Groß-/Kleinschreibung, wenn alle LinkButton klicken oder anfänglich auf die Seite besuchen) anzeigen. Hosten Sie die Benutzerkonten in einem besser verwaltbare Segmente dargestellt, konfigurieren wir die GridView um 10 Benutzerkonten zu einem Zeitpunkt anzuzeigen.

GridView-Steuerelements bietet zwei Arten von Paging:

- **Standardnavigation** – einfach zu implementieren, aber ineffizient. Kurz gesagt, erwartet hat Standardwert GridView paging *alle* Datensätze aus der Datenquelle. Es zeigt dann nur die entsprechenden Seite mit Datensätzen.
- **Benutzerdefiniertes Paging** -erfordert mehr Arbeit zu implementieren, aber ist effizienter als die Standardnavigation, da mit benutzerdefinierten Auslagern der Daten Quelle nur die präzise Satz von anzuzeigenden Datensätze zurückzugeben.

Der Leistungsunterschied zwischen Standard- und benutzerdefinierte Paging kann ziemlich beträchtlich sein, wenn paging durch Tausende von Datensätzen. Da wir erstellen kann diese Schnittstelle, die davon ausgegangen, dass Hunderte oder Tausende von Benutzerkonten, verwenden wir das benutzerdefiniertes Paging.

> [!NOTE]
> Eine ausführlichere Erläuterung zu den Unterschieden zwischen standardmäßige und benutzerdefinierte Paging sowie Herausforderung Implementieren von benutzerdefiniertem Paging, finden Sie unter [effizient Paging durch große Mengen von Daten](https://asp.net/learn/data-access/tutorial-25-cs.aspx). Einige Analysen von der Leistungsunterschied zwischen Standard- und benutzerdefinierte Paging, finden Sie unter [benutzerdefiniertes Paging in ASP.NET mit SQL Server 2005](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx).


Benutzerdefiniertes Paging implementieren, das wir zunächst einige Mechanismus, mit dem abzurufenden die präzise Teilmenge der Datensätze, die durch die GridView angezeigt wird. Die gute Nachricht ist, die die `Membership` Klasse `FindUsersByName` Methode weist eine Überladung, die wir der Seitenindex "und" Seitengröße angeben kann, und gibt nur die Benutzerkonten, die innerhalb dieses Bereichs liegt, der Datensätze liegen.

Diese Überladung verfügt insbesondere über die folgende Signatur: [ `FindUsersByName(usernameToMatch, pageIndex, pageSize, totalRecords)` ](https://msdn.microsoft.com/library/fa5st8b2.aspx).

Die *PageIndex* Parameter gibt an, die Seite von Benutzerkonten für zurückgegeben. *PageSize* gibt an, wie viele Datensätze pro Seite angezeigt werden sollen. Die *TotalRecords* Parameter ist ein `out` Parameter, der die Anzahl der insgesamt von Benutzerkonten in den Speicher des Benutzers zurückgegeben.

> [!NOTE]
> Die zurückgegebene Daten `FindUsersByName` wird sortiert nach Benutzername; die Sortierkriterien können nicht angepasst werden.


Die GridView kann konfiguriert werden, um das benutzerdefiniertes Paging nutzen, jedoch nur bei einem ObjectDataSource-Steuerelement gebunden. Für das benutzerdefiniertes Paging implementieren ObjectDataSource-Steuerelement, zwei Methoden benötigt: eine, die eine Zeile startIndex und die maximale Anzahl von Datensätzen, um anzuzeigen, übergeben wird, und gibt die genaue Teilmenge der Datensätze, die innerhalb dieser Spanne liegen und eine Methode, die Gesamtanzahl von Datensätzen zurückgibt, über ausgelagert wird. Die `FindUsersByName` Überladung akzeptiert eine Seitenindex "und" Seitengröße und gibt die Gesamtanzahl von Datensätzen durch ein `out` Parameter. Es gibt also eine Schnittstelle-Konflikt.

Eine Möglichkeit wäre eine Proxyklasse zu erstellen, die die Schnittstelle verfügbar macht, das ObjectDataSource erwartet, und ruft dann intern, den `FindUsersByName` Methode. Eine andere Möglichkeit- und eine für diesen Artikel verwendet wird. - ist unser eigenes Paging-Schnittstelle erstellen und verwenden, anstatt die GridView integrierte Paging-Schnittstelle.

### <a name="creating-a-first-previous-next-last-paging-interface"></a>Erstellen einer ersten, vorherigen, als Nächstes zuletzt Paging-Schnittstelle

Erstellen Sie eine Auslagerungsdatei-Schnittstelle mit dem ersten, zurück, nächsten und letzten LinkButtons. Erste LinkButton geklickt haben, wird der Benutzer zur ersten Seite der Daten, geleitet, während vorherige ihm zur vorherigen Seite zurückzukehren. Ebenso wird weiter und einen Nachnamen der Benutzer verschieben auf der nächsten und letzten Seite bzw. Fügen Sie die vier LinkButton Steuerelemente unterhalb der `UserAccounts` GridView.

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample11.aspx)]

Als Nächstes erstellen Sie einen Ereignishandler für jeden der LinkButton `Click` Ereignisse.

Abbildung 7 zeigt die vier LinkButtons, wenn Sie über die Visual Web Developer-Design-Sicht angezeigt.


[![Fügen Sie ersten, vorherigen hinzu, nächsten und letzten Sie LinkButtons unterhalb der GridView](building-an-interface-to-select-one-user-account-from-many-cs/_static/image20.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image19.png)

**Abbildung 7**: erste hinzufügen, zurück, nächsten und letzten LinkButtons unterhalb der GridView ([klicken Sie hier, um das Bild in voller Größe angezeigt](building-an-interface-to-select-one-user-account-from-many-cs/_static/image21.png))


### <a name="keeping-track-of-the-current-page-index"></a>Nachverfolgen der Index der aktuellen Seite

Wenn ein Benutzer zuerst besucht die `ManageUsers.aspx` Seiten- oder Klicks die Filterung eines Schaltflächen, wir die erste Seite der Daten in die GridView angezeigt werden soll. Wenn der Benutzer eine der Navigation LinkButtons klickt, müssen wir jedoch den Seitenindex zu aktualisieren. Zum Verwalten der Seitenindex und die Anzahl der Datensätze pro Seite angezeigt werden sollen, fügen Sie die folgenden beiden Eigenschaften auf der Seite Code-Behind-Klasse hinzu:

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample12.cs)]

Wie die `UsernameToMatch` -Eigenschaft, die `PageIndex` Eigenschaft weiterhin besteht, dessen Wert Ansichtszustand. Die schreibgeschützte `PageSize` Eigenschaft gibt einen hartcodierten Wert 10. Ich einladen den gewünschten Reader, aktualisieren Sie diese Eigenschaft, um dem gleichen Muster als `PageIndex`, und dann erweitern, die `ManageUsers.aspx` Seite so, dass die Person, die Zugriff auf die Seite angeben kann, wie viele Benutzerkonten für die anzuzeigenden pro Seite.

### <a name="retrieving-just-the-current-pages-records-updating-the-page-index-and-enabling-and-disabling-the-paging-interface-linkbuttons"></a>Nur die aktuelle Seite Datensätze abrufen, Aktualisieren der Seitenindex aktivieren und deaktivieren die Auslagerung Schnittstelle LinkButtons

Mit der Paginierung-Schnittstelle vorhanden und der `PageIndex` und `PageSize` Eigenschaften hinzugefügt werden, werden zur Aktualisierung der `BindUserAccounts` Methode, damit die entsprechenden verwendet `FindUsersByName` überladen. Darüber hinaus benötigen wir diese Methode aktiviert oder deaktiviert die Auslagerungsschnittstelle, je nachdem, welche Seite angezeigt wird. Bei die erste Seite der Daten anzeigen zu können, sollte die Links "First" und "zurück deaktiviert werden. Nächsten und letzten sollte deaktiviert werden, wenn die letzte Seite anzeigen.

Aktualisieren Sie die `BindUserAccounts`-Methode mit folgendem Code:

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample13.cs)]

Beachten Sie, dass der letzte Parameter von der Gesamtzahl der Datensätze, die ausgelagerte über bestimmt wird die `FindUsersByName` Methode. Dies ist ein `out` Parameter, daher muss zuerst eine Variable für diesen Wert zu deklarieren (`totalRecords`), und klicken Sie dann mit Präfix der `out` Schlüsselwort.

Nachdem die angegebene Seite von Benutzerkonten werden zurückgegeben, werden, je nachdem, ob die erste oder letzte Seite der Daten angezeigt wird die vier LinkButtons entweder aktiviert oder deaktiviert.

Im letzten Schritt wird zum Schreiben von Code für die vier LinkButtons' `Click` Ereignishandler. Müssen Sie diese Ereignishandler zum Aktualisieren der `PageIndex` Eigenschaft und binden Sie dann die Daten an die GridView über einen Aufruf an `BindUserAccounts`. Ersten, vorherigen und nächsten Ereignishandler sind sehr einfach. Die `Click` -Ereignishandler für die letzten LinkButton ist jedoch ein wenig komplexer, da wir benötigen, um zu bestimmen, wie viele Datensätze angezeigt werden, um den Index der letzten Seite zu bestimmen.

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample14.cs)]

Abbildung 8 und 9 zeigen das benutzerdefinierte Paging-Schnittstelle in Aktion an. Abbildung 8 zeigt die `ManageUsers.aspx` Startseite bei die erste Seite der Daten für alle Benutzerkonten anzeigen. Beachten Sie, dass nur 10 13 Konten angezeigt werden. Der nächste bzw. den letzten Link führt dazu, dass ein Postback handeln, Updates der `PageIndex` 1, und die zweite Seite des Benutzer-Konten mit dem Raster gebunden (siehe Abbildung 9).


[![Die ersten 10 Benutzerkonten werden angezeigt.](building-an-interface-to-select-one-user-account-from-many-cs/_static/image23.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image22.png)

**Abbildung 8**: die ersten 10 Benutzerkonten werden angezeigt ([klicken Sie hier, um das Bild in voller Größe angezeigt](building-an-interface-to-select-one-user-account-from-many-cs/_static/image24.png))


[![Nach Klicken auf den nächsten Link wird die zweite Seite des Benutzerkonten](building-an-interface-to-select-one-user-account-from-many-cs/_static/image26.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image25.png)

**Abbildung 9**: weiter auf die zweite Seite der Benutzerkonten angezeigt ([klicken Sie hier, um das Bild in voller Größe angezeigt](building-an-interface-to-select-one-user-account-from-many-cs/_static/image27.png))


## <a name="summary"></a>Zusammenfassung

Administratoren müssen häufig einen Benutzer aus der Liste der Konten auswählen. In vorherigen Lernprogrammen erläutert mithilfe einer Dropdownliste mit Benutzern gefüllt, aber dieser Ansatz ist nicht gut skalierbar. In diesem Lernprogramm wird eine bessere Alternative untersucht: eine filterbaren-Schnittstelle, deren Ergebnisse in einem ausgelagerten GridView angezeigt werden. Mit dieser Benutzeroberfläche können Administratoren immer dann schnell und effizient suchen und markieren ein Benutzerkonto zwischen Tausenden.

Viel Spaß beim Programmieren!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Lernprogramm erläutert finden Sie in den folgenden Ressourcen:

- [Benutzerdefiniertes Paging in ASP.NET mit SQLServer 2005](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx)
- [Effizient Paging durch große Mengen von Daten](https://asp.net/learn/data-access/tutorial-25-cs.aspx)
- [Paralleles eigene Websiteverwaltungs-Tool](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)

### <a name="about-the-author"></a>Informationen zum Autor

Scott Mitchell, Autor von mehreren ASP/ASP.NET-Büchern und Gründer von 4GuysFromRolla.com, bereits seit 1998 mit Microsoft-Web-Technologien gearbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird  *[Sams Schulen selbst ASP.NET 2.0 in 24 Stunden](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott erreicht werden kann, zur [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) oder über seinen Blog unter [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Besonderen Dank an

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Lead Prüfer für dieses Lernprogramm wurde Alicja Maziarz. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Nächste](recovering-and-changing-passwords-cs.md)
