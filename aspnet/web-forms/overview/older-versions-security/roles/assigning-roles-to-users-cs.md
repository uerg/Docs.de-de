---
uid: web-forms/overview/older-versions-security/roles/assigning-roles-to-users-cs
title: Zuweisen von Rollen für Benutzer (c#) | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial werden. Sie erstellen zwei ASP.NET-Seiten zur Unterstützung bei der Verwaltung, welche Benutzer welche Rollen angehören. Die erste Seite enthält Funktionen, durch was finden Sie unter...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/24/2008
ms.topic: article
ms.assetid: d522639a-5aca-421e-9a76-d73f95607f57
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-security/roles/assigning-roles-to-users-cs
msc.type: authoredcontent
ms.openlocfilehash: 2f65caf132e185b381093ee1a0b5dd5400ed8434
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374139"
---
<a name="assigning-roles-to-users-c"></a>Zuweisen von Rollen für Benutzer (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/CS.10.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial10_AssigningRoles_cs.pdf)

> In diesem Tutorial werden. Sie erstellen zwei ASP.NET-Seiten zur Unterstützung bei der Verwaltung, welche Benutzer welche Rollen angehören. Die erste Seite enthält Funktionen, um festzustellen, welche Benutzer zu einer bestimmten Rolle gehören welche Rollen, die ein bestimmter Benutzer gehört, und die Möglichkeit zum Zuweisen oder entfernen einen bestimmten Benutzer aus einer bestimmten Rolle. Wir werden in der zweiten Seite des Steuerelements CreateUserWizard erweitern, enthalten einen Schritt zum angeben, welche Rollen zu der neu erstellte Benutzer gehört. Dies ist nützlich in Szenarien, in denen ein Administrator neue Benutzerkonten erstellen kann.


## <a name="introduction"></a>Einführung

Die <a id="_msoanchor_1"> </a> [vorherigen Tutorial](creating-and-managing-roles-cs.md) untersucht das Rollen-Framework und die `SqlRoleProvider`; erläutert, wie Sie mit der `Roles` Klasse zu erstellen, abrufen und Löschen von Rollen. Zusätzlich zum Erstellen und Löschen von Rollen, müssen wir in der Lage, beim Zuweisen oder Entfernen von Benutzern aus einer Rolle sein. Leider wird ASP.NET nicht geliefert, mit allen Websteuerelementen für die Verwaltung, welche Benutzer welche Rollen angehören. Stattdessen müssen wir unsere eigene ASP.NET-Seiten, um diese Zuordnungen verwalten erstellen. Die gute Nachricht ist, dass das Hinzufügen und Entfernen von Benutzern zu Rollen ist recht einfach. Die `Roles` Klasse enthält eine Reihe von Methoden zum Hinzufügen von einem oder mehreren Benutzern zu einer oder mehreren Rollen.

In diesem Tutorial werden. Sie erstellen zwei ASP.NET-Seiten zur Unterstützung bei der Verwaltung, welche Benutzer welche Rollen angehören. Die erste Seite enthält Funktionen, um festzustellen, welche Benutzer zu einer bestimmten Rolle gehören welche Rollen, die ein bestimmter Benutzer gehört, und die Möglichkeit zum Zuweisen oder entfernen einen bestimmten Benutzer aus einer bestimmten Rolle. Wir werden in der zweiten Seite des Steuerelements CreateUserWizard erweitern, enthalten einen Schritt zum angeben, welche Rollen zu der neu erstellte Benutzer gehört. Dies ist nützlich in Szenarien, in denen ein Administrator neue Benutzerkonten erstellen kann.

Fangen wir an!

## <a name="listing-what-users-belong-to-what-roles"></a>Auflisten von welchen Benutzern Rollen angehören

Der erste Tagesordnungspunkt für dieses Tutorial ist die Erstellung einer Webseite, aus der Benutzern Rollen zugewiesen werden können. Bevor wir uns mit Benutzern zu Rollen zuweisen befassen, lassen Sie uns zuerst darauf konzentrieren bestimmen, welche Benutzer welche Rollen angehören. Es gibt zwei Möglichkeiten, diese Informationen angezeigt: "Rolle" oder "von"Benutzer angezeigt. Können wir den Besucher eine Rolle auszuwählen, und zeigen sie alle Benutzer, die die Rolle (der "by" Role"Display) angehören, oder konnte den Besucher, wählen Sie einen Benutzer aus, und zeigen sie die Rollen zugewiesen, die diesem Benutzer (die"von Benutzer"Display) aufgefordert.

Die Ansicht "von" Role"eignet sich in Situationen, in dem wissen möchte, der Besucher die Gruppe von Benutzern, die zu einer bestimmten Rolle gehören; die Ansicht "von Benutzer" ist ideal, wenn der Besucher, Rollen für einen bestimmten Benutzer wissen muss. Werfen wir unserer Seite, die sowohl "Role" und "von Benutzer" sind Schnittstellen.

Zunächst wird mit dem Erstellen der Schnittstelle "von Benutzer". Diese Schnittstelle besteht aus einem Dropdown-Liste und eine Liste von Kontrollkästchen. Die Dropdownliste wird mit der Gruppe von Benutzern im System aufgefüllt. die Kontrollkästchen werden die Rollen aufgelistet werden. Durch Auswahl eines Benutzers aus der Dropdown-Liste überprüft diese Rollen, die, denen der Benutzer gehört. Die Person, die auf der Seite dann aktivieren oder deaktivieren Sie die Kontrollkästchen zum Hinzufügen oder entfernen den ausgewählten Benutzer aus den entsprechenden Rollen.

> [!NOTE]
> Mit einer Dropdown-Liste in die Benutzerkonten ist nicht ideale Wahl für Websites, gibt es möglicherweise Hunderte von Benutzerkonten. Eine Dropdown-Liste wurde entwickelt, damit der Benutzer ein Element aus einer relativ kurzen Liste der Optionen auswählen kann. Es wird schnell unhandlich, wächst die Anzahl der Listenelemente. Wenn Sie eine Website erstellen, die potenziell großen Anzahl von Benutzerkonten verfügen sollen, Sie sollten in Betracht, eine alternative Benutzeroberfläche, wie z. B. einer navigierbaren GridView-Ansicht oder eine filterbar-Schnittstelle, die auflistet fordert den Besucher, mit einen Buchstaben zu wählen und dann nur Zeigt die Benutzer, deren Benutzername mit dem ausgewählten Buchstaben beginnt.


## <a name="step-1-building-the-by-user-user-interface"></a>Schritt 1: Erstellen der Benutzeroberfläche "Von Benutzer"

Öffnen der `UsersAndRoles.aspx` Seite. Fügen Sie am oberen Rand der Seite, ein Label-Steuerelement mit dem Namen `ActionStatus` und lösche die `Text` Eigenschaft. Wir zum Bereitstellen von Feedback zu den Aktionen, die ausgeführt wird, Anzeigen von Meldungen wie diese Bezeichnung verwenden, "Benutzer Tito hinzugefügt wurde die Rolle" Administratoren"" oder "Benutzer Jisun wurde aus entfernt die Rolle" Supervisor"." Um diese erstellen, Nachrichten hervor, legen Sie die Bezeichnung des `CssClass` Eigenschaft auf "Wichtig".

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample1.aspx)]

Fügen Sie die folgende CSS-Klassendefinition, um die `Styles.css` Stylesheet:

[!code-css[Main](assigning-roles-to-users-cs/samples/sample2.css)]

Diese CSS-Definition weist den Browser an die Bezeichnung, die umfangreiche und rote Schrift angezeigt. Abbildung 1 zeigt diesen Effekt über Visual Studio-Designer.


[![CssClass--Eigenschaft der Bezeichnung ergibt ein großes, rotes Schriftart](assigning-roles-to-users-cs/_static/image2.png)](assigning-roles-to-users-cs/_static/image1.png)

**Abbildung 1**: die Bezeichnung `CssClass` führt zu einer großen, Red-Schriftart ([klicken Sie, um das Bild in voller Größe anzeigen](assigning-roles-to-users-cs/_static/image3.png))


Fügen Sie einem DropDownList-Steuerelement auf der Seite legen Sie als Nächstes die `ID` Eigenschaft `UserList`, und legen Sie dessen `AutoPostBack` Eigenschaft auf "true". Wir verwenden diese DropDownList aller Benutzer im System aufgelistet. Diese Dropdownliste wird auf eine Auflistung von MembershipUser Objekte gebunden werden. Da soll der Dropdownliste die UserName-Eigenschaft des Objekts MembershipUser anzuzeigen (und als Wert für die Listenelemente verwenden), legen Sie die DropDownList `DataTextField` und `DataValueField` Eigenschaften auf "UserName".

Fügen Sie unterhalb der Dropdownliste einen Repeater, mit dem Namen `UsersRoleList`. Diesen Repeater werden alle Rollen im System als eine Reihe von Kontrollkästchen aufgelistet. Definieren des Repeaters `ItemTemplate` mithilfe der folgenden deklarativen Markups:

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample3.aspx)]

Die `ItemTemplate` Markup enthält ein einzelnes Kontrollkästchen-Steuerelement mit dem Namen `RoleCheckBox`. Des Kontrollkästchens `AutoPostBack` Eigenschaft auf "true" festgelegt ist und die `Text` Eigenschaft gebunden ist `Container.DataItem`. Der Grund dafür die Databinding-Syntax ist einfach `Container.DataItem` liegt daran, dass das Framework Rollen gibt die Liste von Rollennamen als Zeichenfolgenarray zurück, und es diesem Zeichenfolgenarray, das wir an der Repeater Bindung erfolgt ist. Eine ausführliche Beschreibung der warum diese Syntax verwendet wird, um den Inhalt eines Arrays an eine Web-Steuerelement gebunden anzuzeigen ist, würde den Rahmen dieses Tutorials. Weitere Informationen zu dieser Angelegenheit, finden Sie unter [binden ein Array von skalaren auf ein Websteuerelement Daten](http://aspnet.4guysfromrolla.com/articles/082504-1.aspx).

An diesem Punkt sollte Ihre "von Benutzer" Schnittstelle deklaratives Markup etwa wie folgt aussehen:

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample4.aspx)]

Wir können nun den Code zum Binden Sie die Gruppe von Benutzerkonten in der Dropdownliste und den Satz von Rollen, des Repeaters schreiben. Fügen Sie in der Seite des Code-Behind-Klasse eine Methode, die mit dem Namen `BindUsersToUserList` und eine weitere mit dem Namen `BindRolesList`, mit dem folgenden Code:

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample5.cs)]

Die `BindUsersToUserList` Methode ruft alle Benutzerkonten im System über die [ `Membership.GetAllUsers` Methode](https://msdn.microsoft.com/library/dy8swhya.aspx). Dies gibt eine [ `MembershipUserCollection` Objekt](https://msdn.microsoft.com/library/system.web.security.membershipusercollection.aspx), dies ist eine Auflistung von [ `MembershipUser` Instanzen](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx). Klicken Sie dann an diese Auflistung gebunden ist die `UserList` DropDownList. Die `MembershipUser` Instanzen dieser Zusammensetzung die Auflistung enthalten eine Vielzahl von Eigenschaften, z. B. `UserName`, `Email`, `CreationDate`, und `IsOnline`. Um der Dropdownliste den Wert der anzuzeigenden weisen die `UserName` -Eigenschaft, sicher, dass die `UserList` DropDownList `DataTextField` und `DataValueField` auf "UserName" festgelegt wurde.

> [!NOTE]
> Die `Membership.GetAllUsers` Methode verfügt über zwei Überladungen: eine, die keine Eingabeparameter akzeptiert und gibt alle Benutzer und eine, die ganzzahlige Werte für die Seite "Index" und "Seitengröße akzeptiert und nur die angegebene Teilmenge der Benutzer zurückgibt. Wenn werden große Mengen von Benutzerkonten, die in einer navigierbaren Element der Benutzeroberfläche angezeigt wird, kann die zweite Überladung verwendet werden, effizienter Seite über die Benutzer, da nur die genaue Teilmenge von Benutzerkonten anstelle aller Textknoten zurückgegeben.


Der `BindRolesToList` Methode beginnt mit dem Aufrufen der `Roles` Klasse [ `GetAllRoles` Methode](https://msdn.microsoft.com/library/system.web.security.roles.getallroles.aspx), die ein Zeichenfolgenarray mit den Rollen im System zurückgibt. Diese Zeichenfolgen-Array wird dann an den Repeater gebunden.

Abschließend müssen wir diese beiden Methoden aufrufen, beim ersten die Seite laden. Fügen Sie dem `Page_Load`-Ereignishandler folgenden Code hinzu:

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample6.cs)]

Können Sie mit diesem Code werden die Seite über einen Browser besuchen; Ihr Bildschirm sollte ähnlich wie in Abbildung 2 aussehen. Alle Benutzerkonten werden in der Dropdown Liste und darunter, aufgefüllt jede Rolle wird als ein Kontrollkästchen angezeigt. Da wir legen die `AutoPostBack` Eigenschaften der DropDownList und Kontrollkästchen auf "true", ändern den ausgewählten Benutzer oder das Überprüfen und Deaktivieren einer Rolle bewirkt, dass einen Postback. Keine Aktion wird jedoch ausgeführt, da wir noch Code schreiben, um diese Aktionen zu behandeln müssen. Wir werden diese Aufgaben in den nächsten beiden Abschnitten in Angriff nehmen.


[![Die Seite zeigt die Benutzer und Rollen](assigning-roles-to-users-cs/_static/image5.png)](assigning-roles-to-users-cs/_static/image4.png)

**Abbildung 2**: die Seite zeigt die Benutzer und Rollen ([klicken Sie, um das Bild in voller Größe anzeigen](assigning-roles-to-users-cs/_static/image6.png))


### <a name="checking-the-roles-the-selected-user-belongs-to"></a>Überprüfen die Rollen gehört von der ausgewählte Benutzer zu

Beim ersten Laden die Seite, oder wenn Besucher einen neuen Benutzer aus der Dropdown-Liste auswählt, müssen wir aktualisieren den `UsersRoleList`die Kontrollkästchen, damit eine bestimmte Rolle Kontrollkästchen aktiviert ist, nur dann, wenn der ausgewählte Benutzer, die dieser Rolle gehört. Um dies zu erreichen, erstellen Sie eine Methode namens `CheckRolesForSelectedUser` durch den folgenden Code:

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample7.cs)]

Der obige Code zunächst bestimmen, die der ausgewählte Benutzer ist. Klicken Sie dann der Rollen-Klasse verwendet [ `GetRolesForUser(userName)` Methode](https://msdn.microsoft.com/library/system.web.security.roles.getrolesforuser.aspx) des angegebenen Benutzers-Satz von Rollen als Zeichenfolgenarray zurückgegeben. Als Nächstes des Repeater-Elemente aufgelistet werden und die entsprechenden `RoleCheckBox` Kontrollkästchen wird programmgesteuert auf die verwiesen wird. Das Kontrollkästchen aktiviert ist, nur dann, wenn die Rolle, die es entspricht der in enthalten ist das `selectedUsersRoles` Zeichenfolgenarray.

> [!NOTE]
> Die `selectedUserRoles.Contains<string>(...)` Syntax wird nicht kompiliert werden, wenn Sie ASP.NET Version 2.0 verwenden. Die `Contains<string>` Methode ist Teil der [LINQ-Bibliothek](http://en.wikipedia.org/wiki/Language_Integrated_Query), dies ist neu in ASP.NET 3.5. Wenn Sie noch ASP.NET Version 2.0 verwenden, verwenden Sie die [ `Array.IndexOf<string>` Methode](https://msdn.microsoft.com/library/eha9t187.aspx) stattdessen.


Die `CheckRolesForSelectedUser` -Methode in zwei Fällen aufgerufen werden muss: beim ersten die Seite laden, und wenn die `UserList` DropDownLists ausgewählte Index geändert wird. Daher rufen diese Methode aus der `Page_Load` -Ereignishandler (nach dem Aufrufen der `BindUsersToUserList` und `BindRolesToList`). Darüber hinaus erstellen Sie einen Ereignishandler für die DropDownList `SelectedIndexChanged` Ereignis und rufen Sie diese Methode von dort aus.

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample8.cs)]

Mit diesem Code werden können Sie die Seite über den Browser testen. Da jedoch die `UsersAndRoles.aspx` Seite derzeit verfügt nicht über die Möglichkeit zum Zuweisen von Benutzern zu Rollen, die keine Benutzer Rollen haben. Erstellen wir die Schnittstelle für das Zuweisen von Benutzern zu Rollen in Kürze, damit Sie Mein Wort, das dieser Code funktioniert, und stellen Sie sicher, dass dies der Fall höher ist, oder Sie können Benutzer manuell zu Rollen hinzufügen, durch Einfügen von Datensätzen in der `aspnet_UsersInRoles` Tabelle, um diese Functi zu testen. jetzt Onality.

### <a name="assigning-and-removing-users-from-roles"></a>Zuweisen und Entfernen von Benutzern aus Rollen

Wenn der Besucher markiert oder ein Kontrollkästchen in der `UsersRoleList` Repeater müssen wir zum Hinzufügen oder entfernen den ausgewählten Benutzer aus der entsprechenden Rolle. Des Kontrollkästchens `AutoPostBack` Eigenschaftensatz wird zurzeit auf "true", die einen Postback verursacht jedes Mal, wenn ein Kontrollkästchen im Wiederholungsmodul aktiviert oder deaktiviert ist. Kurz gesagt: Wir müssen einen Ereignishandler für des Kontrollkästchen des erstellen `CheckChanged` Ereignis. Da das Kontrollkästchen in der ein Repeater-Steuerelement ist, müssen wir die Grundlagen der Ereignis-Handler manuell hinzufügen. Starten Sie durch Hinzufügen des ereignishandlers für die Code-Behind-Klasse als eine `protected` -Methode, wie folgt:

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample9.cs)]

Wir zurück zum Schreiben von Code für diese Ereignishandler gleich. Zuerst wird aber wir schließen die Infrastruktur für die Ereignisbehandlung. Über das Kontrollkästchen innerhalb des Repeaters `ItemTemplate`, fügen `OnCheckedChanged="RoleCheckBox_CheckChanged"`. Diese Syntax verbindet die `RoleCheckBox_CheckChanged` -Ereignishandler der `RoleCheckBox`des `CheckedChanged` Ereignis.

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample10.aspx)]

Unsere letzte Aufgabe ist zum Abschließen der `RoleCheckBox_CheckChanged` -Ereignishandler. Wir müssen zunächst durch Verweisen auf das CheckBox-Steuerelement, das das Ereignis ausgelöst hat, da diese Kontrollkästchen-Instanz an des Benutzers wurde von Rolle aktiviert oder deaktiviert ist, über die `Text` und `Checked` Eigenschaften. Mithilfe dieser Informationen zusammen mit dem Benutzernamen des ausgewählten Benutzers wir hinzufügen oder entfernen Sie den Benutzer aus der Rolle über die `Roles` Klasse [ `AddUserToRole` ](https://msdn.microsoft.com/library/system.web.security.roles.addusertorole.aspx) oder [ `RemoveUserFromRole` Methode](https://msdn.microsoft.com/library/system.web.security.roles.removeuserfromrole.aspx).

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample11.cs)]

Der obige Code beginnt, durch Programmgesteuertes Verweisen auf das Kontrollkästchen, das das Ereignis ausgelöst hat, verfügbar über den `sender` input-Parameters. Wenn das Kontrollkästchen aktiviert ist, wird der ausgewählte Benutzer der angegebenen Rolle ist, andernfalls hinzugefügt, die sie aus der Rolle entfernt werden. In beiden Fällen die `ActionStatus` Bezeichnung wird eine Meldung angezeigt, die Zusammenfassung der gerade ausgeführten Aktion.

Nehmen Sie einen Moment Zeit, um diese Seite über einen Browser zu testen. Wählen Sie Benutzer Tito, und fügen Sie Tito für Administratoren und Supervisor-Rollen.


[![Tito wurde die Administratoren und Rollen für Vorgesetzte hinzugefügt](assigning-roles-to-users-cs/_static/image8.png)](assigning-roles-to-users-cs/_static/image7.png)

**Abbildung 3**: Tito wurde hinzugefügt, die Administratoren und Supervisor-Rollen ([klicken Sie, um das Bild in voller Größe anzeigen](assigning-roles-to-users-cs/_static/image9.png))


Wählen Sie anschließend Benutzer Bruce aus der Dropdown-Liste ein. Es wird ein Postback und des Repeaters Kontrollkästchen werden aktualisiert, über die `CheckRolesForSelectedUser`. Da Rollen noch nicht Bruce gehört, werden die beiden Kontrollkästchen deaktiviert. Fügen Sie anschließend Bruce zur Rolle "Supervisor" ein.


[![Bruce wurde die Rolle "Supervisor" hinzugefügt](assigning-roles-to-users-cs/_static/image11.png)](assigning-roles-to-users-cs/_static/image10.png)

**Abbildung 4**: Bruce Funktion wurde in die Rolle "Supervisor" ([klicken Sie, um das Bild in voller Größe anzeigen](assigning-roles-to-users-cs/_static/image12.png))


Um die Funktionalität des überprüfen die `CheckRolesForSelectedUser` -Methode, ein Benutzer als Tito oder Bruce auswählen. Beachten Sie, wie die Kontrollkästchen automatisch deaktiviert werden, gibt an, dass sie keine Rollen gehören. An Tito zurück. Administratoren und Supervisor Kontrollkästchen sollte überprüft werden.

## <a name="step-2-building-the-by-roles-user-interface"></a>Schritt 2: Erstellen der Benutzeroberfläche "Durch die Rollen"

An diesem Punkt haben die Schnittstelle "von Benutzer" und können mit der Umgang mit der Schnittstelle "durch die Rollen" beginnen. Die Schnittstelle "durch die Rollen" fordert den Benutzer auf eine Rolle aus der Dropdown-Liste auswählen und zeigt dann die Gruppe von Benutzern, die zu dieser Rolle in einer GridView-Ansicht gehören.

Fügen Sie ein anderes DropDownList-Steuerelement, das `UsersAndRoles.aspx` Seite. In diesem Beispiel unter dem Repeater-Steuerelement platzieren, nennen Sie es `RoleList`, und legen Sie dessen `AutoPostBack` Eigenschaft auf "true". Klicken Sie unterhalb, das Hinzufügen einer GridView-Ansicht, und nennen Sie sie `RolesUserList`. Diese GridView werden die Benutzer aufgelistet, die die ausgewählte Rolle angehören. Legen Sie des GridView `AutoGenerateColumns` Eigenschaft auf "false", fügen Sie ein TemplateField zum Raster des `Columns` Sammlung, und legen Sie dessen `HeaderText` Eigenschaft auf "Benutzer". Definieren Sie das TemplateField des `ItemTemplate` , damit diese den Wert des Ausdrucks Databinding anzeigt `Container.DataItem` in die `Text` -Eigenschaft einer Bezeichnung, die mit dem Namen `UserNameLabel`.

Nach dem Hinzufügen und Konfigurieren von GridView, sollte Ihre "von" Role"Schnittstelle deklaratives Markup etwa wie folgt aussehen:

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample12.aspx)]

Wir müssen zum Auffüllen der `RoleList` DropDownList für den Satz von Rollen im System. Aktualisieren Sie zu diesem Zweck die `BindRolesToList` Methode bindet ist das Zeichenfolgenarray von zurückgegebenen der `Roles.GetAllRoles` Methode, um die `RolesList` DropDownList (als auch die `UsersRoleList` Repeater).

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample13.cs)]

Die letzten beiden Zeilen in der `BindRolesToList` Methode wurden hinzugefügt, um den Satz von Rollen zum Binden der `RoleList` DropDownList-Steuerelement. Abbildung 5 zeigt das Ergebnis bei der Anzeige über einen Browser – ein Dropdown-Liste, die mit dem System-Rollen aufgefüllt.


[![Die Rollen werden in der RoleList-Dropdownliste angezeigt.](assigning-roles-to-users-cs/_static/image14.png)](assigning-roles-to-users-cs/_static/image13.png)

**Abbildung 5**: die Rollen werden angezeigt, der `RoleList` DropDownList ([klicken Sie, um das Bild in voller Größe anzeigen](assigning-roles-to-users-cs/_static/image15.png))


### <a name="displaying-the-users-that-belong-to-the-selected-role"></a>Die Benutzer werden angezeigt, die der ausgewählten Rolle gehören.

Beim ersten Laden der Seite, oder wenn eine neue Rolle auswählen, wird die `RoleList` DropDownList, müssen wir die Liste der Benutzer anzuzeigen, die dieser Rolle in der GridView zu gehören. Erstellen Sie eine Methode mit dem Namen `DisplayUsersBelongingToRole` mithilfe des folgenden Codes:

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample14.cs)]

Diese Methode startet durch Abrufen der ausgewählten Rolle aus der `RoleList` DropDownList. Anschließend wird mithilfe der [ `Roles.GetUsersInRole(roleName)` Methode](https://msdn.microsoft.com/library/system.web.security.roles.getusersinrole.aspx) ein Zeichenfolgenarray der Benutzernamen der Benutzer abgerufen, die dieser Rolle angehören. Dieses Array wird dann gebunden, um die `RolesUserList` GridView.

Diese Methode muss in zwei Situationen aufgerufen werden: beim erstmaligen die Seite laden, und wenn die ausgewählte Rolle in der `RoleList` DropDownList-Änderungen. Aktualisieren Sie daher die `Page_Load` -Ereignishandler, damit diese Methode aufgerufen wird, nach dem Aufruf von `CheckRolesForSelectedUser`. Als Nächstes erstellen Sie einen Ereignishandler für die `RoleList`des `SelectedIndexChanged` -Ereignis, und rufen Sie diese Methode von dort aus zu.

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample15.cs)]

Mit diesem Code werden die `RolesUserList` GridView diese Benutzer, die die ausgewählte Rolle angehören, sollte angezeigt werden. Wie in Abbildung 6 gezeigt, besteht die Rolle "Supervisor" von zwei Elementen: Bruce und Tito.


[![GridView zeigt die Benutzer, die der ausgewählten Rolle gehören.](assigning-roles-to-users-cs/_static/image17.png)](assigning-roles-to-users-cs/_static/image16.png)

**Abbildung 6**: die GridView Listet die Benutzer, gehören die Rolle "ausgewählt" ([klicken Sie, um das Bild in voller Größe anzeigen](assigning-roles-to-users-cs/_static/image18.png))


### <a name="removing-users-from-the-selected-role"></a>Entfernen von Benutzern aus der ausgewählten Rolle

Wir erweitern die `RolesUserList` GridView, enthalten eine Spalte mit "Remove"-Schaltflächen. Klicken auf die Schaltfläche "Entfernen" für einen bestimmten Benutzer entfernt sie aus dieser Rolle.

Starten Sie durch das Hinzufügen eines Felds der löschen-Schaltfläche an die GridView. Stellen Sie dieses Feld als die meisten archivierten links angezeigt, und ändern Sie seine `DeleteText` Eigenschaft von "Delete" (Standard) auf "Entfernen".


[![Hinzufügen der](assigning-roles-to-users-cs/_static/image20.png)](assigning-roles-to-users-cs/_static/image19.png)

**Abbildung 7**: Fügen Sie die Schaltfläche "Entfernen" an die GridView ([klicken Sie, um das Bild in voller Größe anzeigen](assigning-roles-to-users-cs/_static/image21.png))


Wenn auf die Schaltfläche "Entfernen" geklickt, wird ein Postback Sicherheitsvorschriften und GridView `RowDeleting` Ereignis wird ausgelöst. Wir müssen einen Ereignishandler für dieses Ereignis erstellen und Schreiben von Code, der der Benutzer von der ausgewählten Rolle entfernt. Erstellen Sie den Ereignishandler, und fügen Sie den folgenden Code:

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample16.cs)]

Der Code beginnt, durch den Namen der ausgewählten Rolle bestimmen. Es wird dann programmgesteuert Verweise der `UserNameLabel` Steuerelement aus der Zeile, deren "Remove"-Schaltfläche geklickt wurde, um zu ermitteln, den Benutzernamen des Benutzers entfernen. Der Benutzer wird dann entfernt, aus der Rolle über einen Aufruf an die `Roles.RemoveUserFromRole` Methode. Die `RolesUserList` GridView wird dann aktualisiert und eine Meldung wird angezeigt, über die `ActionStatus` Beschriftungs-Steuerelement.

> [!NOTE]
> Die Schaltfläche "Entfernen" erfordert ein beliebiges Bestätigung durch den Benutzer vor dem Entfernen des Benutzers aus der Rolle nicht. Ich lade Sie gewisse Bestätigung durch den Benutzer hinzufügen. Eine der einfachsten Möglichkeiten, eine Aktion zu bestätigen ist über ein Dialogfeld für die clientseitige bestätigen. Weitere Informationen zu dieser Technik finden Sie unter [Hinzufügen der clientseitigen Bestätigung beim Löschen von](https://asp.net/learn/data-access/tutorial-42-cs.aspx).


Abbildung 8 zeigt die Seite, nachdem Benutzer Tito aus der Supervisor-Gruppe entfernt wurde.


[![Leider ist Tito nicht mehr Supervisor](assigning-roles-to-users-cs/_static/image23.png)](assigning-roles-to-users-cs/_static/image22.png)

**Abbildung 8**: Leider Tito ist nicht mehr einen Vorgesetzten ([klicken Sie, um das Bild in voller Größe anzeigen](assigning-roles-to-users-cs/_static/image24.png))


### <a name="adding-new-users-to-the-selected-role"></a>Hinzufügen neuer Benutzer zu der ausgewählten Rolle

Zusammen mit der Benutzer aus der ausgewählten Rolle entfernt haben, sollte der Besucher auf diese Seite auch der ausgewählten Rolle einen Benutzer hinzufügen können. Optimale Schnittstelle für das Hinzufügen eines Benutzers zu der ausgewählten Rolle hängt von der Anzahl von Benutzerkonten, die Sie erwarten, dass ab. Wenn Ihre Website mit nur wenigen Dutzend Benutzerkonten gespeichert werden soll oder weniger beträgt, können Sie hier einem DropDownList-Steuerelement. Wenn es möglicherweise Tausende von Benutzerkonten, möchten Sie eine Benutzeroberfläche enthalten, die den Besucher auf der Seite über die Konten, suchen Sie nach einem bestimmten Konto oder das Filtern der Benutzerkonten in eine andere Art zulässt.

Für diese Seite verwenden wir eine sehr einfache Schnittstelle, die unabhängig von der Anzahl von Benutzerkonten im System arbeitet. Insbesondere verwenden wir ein Textfeld, aufgefordert wird des Besuchers den Benutzernamen des Benutzers eingeben, die sie der ausgewählten Rolle hinzufügen möchte. Wenn kein Benutzer mit diesem Namen vorhanden ist oder wenn der Benutzer bereits Mitglied der Rolle ist, wir eine Nachricht in zeigen `ActionStatus` Bezeichnung. Aber wenn der Benutzer vorhanden ist, und es kein Mitglied der Rolle ist, wir sie der Rolle hinzufügen und das Raster zu aktualisieren.

Fügen Sie einem Textfeld und einer Schaltfläche unterhalb der GridView hinzu. Festlegen des Textfelds `ID` zu `UserNameToAddToRole` und legen Sie die `ID` und `Text` Eigenschaften `AddUserToRoleButton` und "Hinzufügen zu Benutzerrolle" bzw.

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample17.aspx)]

Als Nächstes erstellen Sie eine `Click` -Ereignishandler für die `AddUserToRoleButton` und fügen Sie den folgenden Code hinzu:

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample18.cs)]

Der Großteil des Codes in der `Click` -Ereignishandler führt verschiedene Überprüfungen. Dadurch wird sichergestellt, dass der Besucher mit einen Benutzernamen in angegeben die `UserNameToAddToRole` Textfeld, das der Benutzer im System vorhanden ist, und nicht bereits in die ausgewählte Rolle gehören. Wenn diese überprüft, ob ein Fehler auftritt, wird eine entsprechende Meldung angezeigt, `ActionStatus` und der Ereignishandler beendet ist. Wenn alle Überprüfungen erfolgreich sind, wird der Benutzer hinzugefügt, auf die Rolle über die `Roles.AddUserToRole` Methode. Danach die TextBox `Text` Eigenschaft deaktiviert ist, wird die GridView aktualisiert, und die `ActionStatus` Bezeichnung zeigt die Meldung, dass der angegebene Benutzer der ausgewählten Rolle erfolgreich hinzugefügt wurde.

> [!NOTE]
> Um sicherzustellen, dass der angegebene Benutzer nicht bereits in die ausgewählte Rolle gehört, verwenden wir die [ `Roles.IsUserInRole(userName, roleName)` Methode](https://msdn.microsoft.com/library/system.web.security.roles.isuserinrole.aspx), ein boolescher Wert, der zurückgegeben, ob *Benutzername* ist ein Mitglied *RoleName*. Wir verwenden diese Methode in der <a id="_msoanchor_2"> </a> [nächsten Tutorial](role-based-authorization-cs.md) Wenn sehen wir uns an rollenbasierte Autorisierung.


Finden Sie auf der Seite über einen Browser, und wählen Sie die Rolle der Vorgesetzte aus dem `RoleList` DropDownList. Versuchen Sie, einen ungültigen Benutzernamen einzugeben – Daraufhin sollte eine Meldung angezeigt, dass der Benutzer im System nicht vorhanden ist.


[![Sie können nicht zu einer Rolle einen nicht existierende Benutzer hinzufügen.](assigning-roles-to-users-cs/_static/image26.png)](assigning-roles-to-users-cs/_static/image25.png)

**Abbildung 9**: Sie können nicht vorhandener Benutzer nicht zu einer Rolle hinzufügen ([klicken Sie, um das Bild in voller Größe anzeigen](assigning-roles-to-users-cs/_static/image27.png))


Versuchen Sie nun das Hinzufügen eines gültigen Benutzers aus. Fahren Sie fort, und wieder Tito hinzu, der Supervisor-Rolle.


[![Hier ist der Tito Supervisor!](assigning-roles-to-users-cs/_static/image29.png)](assigning-roles-to-users-cs/_static/image28.png)

**Abbildung 10**: Hier ist ein Supervisor Tito!  ([Klicken Sie, um das Bild in voller Größe anzeigen](assigning-roles-to-users-cs/_static/image30.png))


## <a name="step-3-cross-updating-the-by-user-and-by-role-interfaces"></a>Schritt 3: Cross-Aktualisierung der "Von Benutzer" und "Von der Rolle" Schnittstellen

Die `UsersAndRoles.aspx` Seite bietet zwei unterschiedliche Benutzeroberflächen für die Verwaltung von Benutzern und Rollen. Derzeit werden diese beiden Schnittstellen unabhängig voneinander fungieren, daher ist es möglich, dass eine Änderung in einer Schnittstelle in der anderen nicht sofort wiedergegeben werden. Beispiel: Angenommen, dass der Besucher auf der Seite die Supervisor-Rolle von auswählt der `RoleList` DropDownList, der Bruce und Tito als Member enthält. Anschließend wählt der Besucher Tito aus der `UserList` DropDownList, die die Kontrollkästchen für Administratoren und Supervisor überprüft wird, in der `UsersRoleList` Repeater. Wenn der Besucher, klicken Sie dann die Rolle des Vorgesetzten aus der Repeater markiert, wird Tito aus der Supervisor-Rolle entfernt diese Änderung wird nicht widergespiegelt, in der Schnittstelle "von" Role". Das GridView zeigt weiterhin Tito als Mitglied der Rolle "Supervisor".

Um dies zu beheben, müssen wir GridView zu aktualisieren, wenn eine Rolle ist, aktiviert oder deaktiviert ist, aus, der `UsersRoleList` Repeater. Ebenso müssen wir Repeater zu aktualisieren, wenn ein Benutzer entfernt oder zu einer Rolle über die Schnittstelle "von der Rolle" hinzugefügt wird.

Der Repeater in der Schnittstelle "von Benutzer" wird aktualisiert, durch den Aufruf der `CheckRolesForSelectedUser` Methode. Die Schnittstelle "von der Rolle" kann geändert werden, der `RolesUserList` GridView `RowDeleting` -Ereignishandler und die `AddUserToRoleButton` Schaltfläche `Click` -Ereignishandler. Aus diesem Grund müssen wir rufen die `CheckRolesForSelectedUser` Methode aus einer dieser Methoden.

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample19.cs)]

Auf ähnliche Weise wird die GridView in der Schnittstelle "von der Rolle" durch den Aufruf aktualisiert die `DisplayUsersBelongingToRole` -Methode und die Schnittstelle "von Benutzer" wird geändert, durch die `RoleCheckBox_CheckChanged` -Ereignishandler. Aus diesem Grund müssen wir rufen die `DisplayUsersBelongingToRole` Methode aus diesem Ereignishandler.

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample20.cs)]

Mit diesen kleinere Änderungen am Code der Schnittstellen "von Benutzer" und "von der Rolle" nun korrekt Cross-Update. Klicken Sie zur Überprüfung finden Sie auf der Seite über einen Browser, und wählen Sie Tito und Supervisor aus der `UserList` und `RoleList` DropDownList-Steuerelementen, bzw. Beachten Sie, wie Sie die Rolle der Vorgesetzte für Tito aus der Repeater in der Schnittstelle "von Benutzer" deaktivieren, Tito automatisch aus der GridView in der Schnittstelle "von" Role"entfernt wird. Der Vorgesetzte Kontrollkästchen in der Schnittstelle "von Benutzer" Hinzufügen von Tito zurück an die Rolle "Supervisor" von der Schnittstelle "von der Rolle" automatisch erneut überprüft werden.

## <a name="step-4-customizing-the-createuserwizard-to-include-a-specify-roles-step"></a>Schritt 4: Anpassen der CreateUserWizard enthält einen Schritt "Rollen angeben"

In der <a id="_msoanchor_3"> </a> [ *Erstellen von Benutzerkonten* ](../membership/creating-user-accounts-cs.md) Tutorial erläutert, wie Sie mit, dass das Steuerelement CreateUserWizard bieten eine Schnittstelle zum Erstellen eines neuen Benutzerkontos. Das Steuerelement CreateUserWizard kann auf zwei Arten verwendet werden:

- Als Mittel zur Besucher, um ihr eigenes Benutzerkonto auf der Website zu erstellen und
- Als eine Möglichkeit für Administratoren zum Erstellen neuer Konten

Im ersten Anwendungsfall ein Besucher darum geht, den Standort und füllt die CreateUserWizard, ihre Informationen eingeben, um auf der Website zu registrieren. Im zweiten Fall erstellt ein Administrator ein neues Konto für eine andere Person.

Wenn Sie ein Konto von einem Administrator für eine andere Person erstellt wird, kann es hilfreich sein, ermöglichen dem Administrator, welche Rollen angeben, das neue Benutzerkonto gehört. In der <a id="_msoanchor_4"> </a> [ *speichern* *zusätzliche Benutzerinformationen* ](../membership/storing-additional-user-information-cs.md) Tutorial erläutert, wie die CreateUserWizard durch Hinzufügen zusätzlicher anpassen `WizardSteps`. Sehen wir uns an, wie einen zusätzlichen Schritt der CreateUserWizard hinzufügen, um die neue Rollen des Benutzers angeben.

Öffnen der `CreateUserWizardWithRoles.aspx` Seite, und fügen Sie ein Steuerelement CreateUserWizard, die mit dem Namen `RegisterUserWithRoles`. Legen Sie die `ContinueDestinationPageUrl` Eigenschaft "~ /" default.aspx "". Da die Idee dabei ist, dass ein Administrator dieses Steuerelement CreateUserWizard verwenden, neue Benutzerkonten erstellen, legen Sie die `LoginCreatedUser` Eigenschaft auf "false". Dies `LoginCreatedUser` Eigenschaft gibt an, ob der Besucher automatisch als des gerade erstellten Benutzers angemeldet ist, und wird auf "true standardmäßig". Wir legen ihn auf false fest, denn wenn ein Administrator ein neues Konto erstellt, die wir ihn als sich angemeldet bleiben möchten.

Wählen Sie als Nächstes die "hinzufügen/entfernen `WizardSteps`..." option die CreateUserWizards Smarttag, und fügen Sie einen neuen `WizardStep`wird durch das Festlegen der `ID` zu `SpecifyRolesStep`. Verschieben der `SpecifyRolesStep WizardStep` , damit es nach dem Schritt "SSO für Ihr neues Konto", aber vor dem Schritt "Abschließen" geht. Festlegen der `WizardStep`des `Title` Eigenschaft auf "Geben Sie Roles", die `StepType` Eigenschaft, um `Step`, und die zugehörige `AllowReturn` Eigenschaft auf "false".


[![Hinzufügen der](assigning-roles-to-users-cs/_static/image32.png)](assigning-roles-to-users-cs/_static/image31.png)

**Abbildung 11**: Hinzufügen von "Geben Sie Roles" `WizardStep` auf die CreateUserWizard ([klicken Sie, um das Bild in voller Größe anzeigen](assigning-roles-to-users-cs/_static/image33.png))


Nach dieser Änderung sollte Ihre CreateUserWizards deklarative Markup wie folgt aussehen:

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample21.aspx)]

"Geben Sie Mitglied der Rollen" `WizardStep`, Hinzufügen einer "CheckBoxList" mit dem Namen `RoleList`. Diese "CheckBoxList" werden die verfügbaren Rollen aufgelistet und aktivieren die Person, die auf der Seite, um Rollen zu überprüfen, der neu erstellte Benutzer gehört.

Wir bleiben mit zwei Codieraufgaben: zuerst müssen wir Auffüllen der `RoleList` "CheckBoxList" mit den Rollen im System; Zweitens müssen wir den erstellten Benutzer für die ausgewählten Rollen hinzuzufügen, wenn der Benutzer aus dem Schritt "Geben Sie Roles" mit dem Schritt "Abschließen". Wir können die erste Aufgabe beim Erreichen der `Page_Load` -Ereignishandler. Der folgende code programmgesteuert Verweise der `RoleList` Kontrollkästchen auf der ersten finden Sie auf der Seite und die Rollen im System an sie bindet.

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample22.cs)]

Der obige Code sollte bekannt vorkommen. In der <a id="_msoanchor_5"> </a> [ *speichern* *zusätzliche Benutzerinformationen* ](../membership/storing-additional-user-information-cs.md) Tutorial verwendet zwei `FindControl` Anweisungen, um auf ein Steuerelement zu verweisen. von innerhalb einer benutzerdefinierten `WizardStep`. Und der Code, der die Rollen "CheckBoxList" gebunden wurde zuvor in diesem Tutorial entnommen.

Um die zweite Aufgabe als Programmierer auszuführen, den müssen wir wissen, wenn der Schritt "Geben Sie Roles" abgeschlossen wurde. Beachten Sie, die die CreateUserWizard ein `ActiveStepChanged` Ereignis, das jedes Mal ausgelöst wird, der Besucher, die von einem Schritt zu einem anderen navigiert. Hier können wir bestimmen, ob der Benutzer im Schritt "Abgeschlossen" erreicht hat; Wenn dies der Fall ist, müssen wir den Benutzer der ausgewählten Rollen hinzuzufügen.

Erstellen Sie einen Ereignishandler für die `ActiveStepChanged` Ereignis und fügen Sie den folgenden Code hinzu:

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample23.cs)]

Wenn der Benutzer nur den Schritt "Abgeschlossen" erreicht hat, wird der Ereignishandler Listet die Elemente von der `RoleList` "CheckBoxList" und dem gerade erstellten Benutzer der ausgewählten Rollen zugewiesen ist.

Besuchen Sie diese Seite über einen Browser ein. Der erste Schritt bei der CreateUserWizard ist der standard "SSO für Ihr neues Konto" Schritt, der für den neuen Benutzernamen, Kennwort, e-Mail- und anderen wichtigen Informationen aufgefordert werden. Geben Sie die Informationen zum Erstellen eines neuen Benutzers mit dem Namen Wanda aus.


[![Erstellen eines neuen Benutzers mit dem Namen Wanda](assigning-roles-to-users-cs/_static/image35.png)](assigning-roles-to-users-cs/_static/image34.png)

**Abbildung 12**: Erstellen Sie einen neuen Benutzer mit dem Namen Wanda ([klicken Sie, um das Bild in voller Größe anzeigen](assigning-roles-to-users-cs/_static/image36.png))


Klicken Sie auf die Schaltfläche "Benutzer erstellen". Ruft die CreateUserWizard intern die `Membership.CreateUser` -Methode, erstellen das neue Benutzerkonto ein, und klicken Sie dann im Verlauf mit dem nächsten Schritt, "Rollen angeben." Hier werden die Standortsystemrollen aufgeführt. Aktivieren Sie das Kontrollkästchen der Supervisor aus, und klicken Sie auf Weiter.


[![Stellen Sie Wanda Mitglied der Rolle "Supervisor"](assigning-roles-to-users-cs/_static/image38.png)](assigning-roles-to-users-cs/_static/image37.png)

**Abbildung 13**: Stellen Sie Wanda Mitglied der Rolle "Supervisor" ([klicken Sie, um das Bild in voller Größe anzeigen](assigning-roles-to-users-cs/_static/image39.png))


Klicken Sie auf Weiter bewirkt, dass ein Postback und Updates der `ActiveStep` mit dem Schritt "Abschließen". In der `ActiveStepChanged` -Ereignishandler das zuletzt erstellte Benutzerkonto wird der Vorgesetzte-Rolle zugewiesen. Um dies zu überprüfen, zurück zu den `UsersAndRoles.aspx` Seite und wählen Sie Vorgesetzte aus der `RoleList` DropDownList. Wie in Abbildung 14 gezeigt, der Supervisor jetzt bestehen drei Benutzer: Bruce, Tito und Wanda.


[![Bruce, Tito und Wanda sind alle Supervisor](assigning-roles-to-users-cs/_static/image41.png)](assigning-roles-to-users-cs/_static/image40.png)

**Abbildung 14**: Bruce, Tito und Wanda sind alle Vorgesetzte ([klicken Sie, um das Bild in voller Größe anzeigen](assigning-roles-to-users-cs/_static/image42.png))


## <a name="summary"></a>Zusammenfassung

Die Rollen-Framework bietet Methoden zum Abrufen von Informationen zu eines bestimmten Benutzers Rollen und Methoden zum bestimmen, welche Benutzer zu einer bestimmten Rolle gehören. Darüber hinaus stehen eine Reihe von Methoden zum Hinzufügen und entfernen einen oder mehrere Benutzer auf eine oder mehrere Rollen zur Verfügung. In diesem Tutorial haben wir uns nur zwei der folgenden Methoden: `AddUserToRole` und `RemoveUserFromRole`. Es gibt weitere Varianten, die entwickelt, um mehrere Benutzer zu einer einzelnen Rolle hinzufügen und einen einzelnen Benutzer mehreren Rollen zuweisen.

In diesem Tutorial enthalten auch einen Blick auf das Erweitern des Steuerelements CreateUserWizard sollen eine `WizardStep` Rollen des neu erstellten Benutzers an. Solch ein Schritt können Administratoren den Prozess zum Erstellen von Benutzerkonten für neue Benutzer zu optimieren.

An diesem Punkt haben Sie erfahren, wie zum Erstellen und Löschen von Rollen und Benutzer hinzufügen und Entfernen von Rollen. Aber wir haben dafür noch zum Anwenden der rollenbasierten Autorisierung betrachten. In der <a id="_msoanchor_6"> </a> [nachfolgenden Tutorial](role-based-authorization-cs.md) betrachten wir definieren Regeln auf URL-Autorisierung pro Rolle-von-Rolle sowie wie die Rollen des aktuell angemeldeten Benutzers zur Einschränkung der Seite auf Funktionalität abhängig.

Viel Spaß beim Programmieren!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Tutorial erläutert finden Sie in den folgenden Ressourcen:

- [ASP.NET Web Site Administration Tool Overview](https://msdn.microsoft.com/library/ms228053.aspx)
- [Untersuchen ASP. NET Mitgliedschaft, Rollen und Profile](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Paralleles eigene Websiteverwaltungs-Tool](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)

### <a name="about-the-author"></a>Der Autor

Scott Mitchell, Autor von mehreren Büchern zu ASP/ASP.NET und Gründer von 4GuysFromRolla.com, ist seit 1998 mit Microsoft-Web-Technologien gearbeitet. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird *[Sams Schulen selbst ASP.NET 2.0 in 24 Stunden](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott erreicht werden kann, zur [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) oder über seinen Blog unter [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Besonderen Dank an...

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führendes Prüfer für dieses Tutorial wurde Teresa Murphy. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](creating-and-managing-roles-cs.md)
> [Weiter](role-based-authorization-cs.md)
