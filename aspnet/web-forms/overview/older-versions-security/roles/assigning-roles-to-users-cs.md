---
uid: web-forms/overview/older-versions-security/roles/assigning-roles-to-users-cs
title: "Zuweisen von Rollen für Benutzer (c#) | Microsoft Docs"
author: rick-anderson
description: "In diesem Lernprogramm werden zwei ASP.NET-Seiten zur Unterstützung beim Verwalten, welche Benutzer gehören, welche Rollen erstellen. Die erste Seite enthält Funktionen zum sehen..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/24/2008
ms.topic: article
ms.assetid: d522639a-5aca-421e-9a76-d73f95607f57
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/roles/assigning-roles-to-users-cs
msc.type: authoredcontent
ms.openlocfilehash: 15d2b427e6fccfc82eab535200ba6878ab41b72e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="assigning-roles-to-users-c"></a>Zuweisen von Rollen an Benutzer (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Herunterladen von Code](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/CS.10.zip) oder [PDF herunterladen](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial10_AssigningRoles_cs.pdf)

> In diesem Lernprogramm werden zwei ASP.NET-Seiten zur Unterstützung beim Verwalten, welche Benutzer gehören, welche Rollen erstellen. Die erste Seite umfasst Funktionen, um festzustellen, welche Benutzer zu einer bestimmten Rolle gehören enthaltenen Rollen, die ein bestimmter Benutzer gehört, sowie die Möglichkeit zum Zuweisen oder einen bestimmten Benutzer aus einer bestimmten Rolle zu entfernen. In der zweiten Seite wird wir das CreateUserWizard-Steuerelement erweitern, damit es enthält einen Schritt zum angeben, welche Rollen der neu erstellte Benutzer gehört. Dies ist nützlich in Szenarien, in denen ein Administrator neue Benutzerkonten erstellen kann.


## <a name="introduction"></a>Einführung

Die <a id="_msoanchor_1"> </a> [vorherigen Lernprogramm](creating-and-managing-roles-cs.md) untersucht das Rollen-Framework und die `SqlRoleProvider`; wurde erläutert, wie Sie die `Roles` Klasse erstellen, abrufen und Löschen von Rollen. Zusätzlich zum Erstellen und Löschen von Rollen, müssen wir möglicherweise zuweisen oder Entfernen von Benutzern aus einer Rolle. ASP.NET ist leider nicht enthalten, mit allen Websteuerelementen für die Verwaltung, welche Benutzer welche Rollen angehören. Stattdessen müssen wir unser eigenes ASP.NET-Seiten zum Verwalten dieser Zuordnungen erstellen. Die gute Nachricht ist, dass das Hinzufügen und Entfernen von Benutzern zu Rollen ist recht einfach. Die `Roles` Klasse enthält eine Reihe von Methoden für einen oder mehrere Benutzer eine oder mehrere Rollen hinzufügen.

In diesem Lernprogramm werden zwei ASP.NET-Seiten zur Unterstützung beim Verwalten, welche Benutzer gehören, welche Rollen erstellen. Die erste Seite umfasst Funktionen, um festzustellen, welche Benutzer zu einer bestimmten Rolle gehören enthaltenen Rollen, die ein bestimmter Benutzer gehört, sowie die Möglichkeit zum Zuweisen oder einen bestimmten Benutzer aus einer bestimmten Rolle zu entfernen. In der zweiten Seite wird wir das CreateUserWizard-Steuerelement erweitern, damit es enthält einen Schritt zum angeben, welche Rollen der neu erstellte Benutzer gehört. Dies ist nützlich in Szenarien, in denen ein Administrator neue Benutzerkonten erstellen kann.

Fangen wir an!

## <a name="listing-what-users-belong-to-what-roles"></a>Auflisten von welchen Benutzern gehören, welche Rollen

Die erste Order Of Business für dieses Lernprogramm besteht darin, eine Webseite erstellen, von der Benutzern Rollen zugewiesen werden können. Bevor wir uns mit Gewusst wie: Zuweisen von Benutzern zu Rollen betreffen, wir konzentrieren zum bestimmen, welche Benutzer auf welche Rollen gehören. Es gibt zwei Möglichkeiten, diese Informationen angezeigt: "Rolle" oder "von"Benutzer angezeigt. Bieten wir den Besucher Rolle auswählen und dann zeigen sie alle Benutzer, die der Rolle (der "von der Rolle" Display) gehören, oder es kann z. B. aufgefordert des Besuchers, wählen Sie einen Benutzer aus, und klicken Sie dann die für den Benutzer (der "von Benutzer" Display) zugewiesenen Rollen anzuzeigen.

Die Ansicht "von der Rolle" ist hilfreich in Fällen, bei denen möchte des Besuchers die Gruppe von Benutzern zu kennen, die zu einer bestimmten Rolle gehören; die Ansicht "von Benutzer" ist ideal, wenn Besucher Rollen für einen bestimmten Benutzer bekannt sein muss. Wir haben unsere Seite "Role" und "von Benutzer" sind Schnittstellen.

Zunächst wird mit dem Erstellen der Schnittstelle "von Benutzer". Diese Schnittstelle besteht aus einem Dropdown-Liste und eine Liste von Kontrollkästchen. Die Dropdownliste wird mit der Gruppe von Benutzern im System aufgefüllt. die Kontrollkästchen werden die Rollen aufgelistet werden. Durch Auswahl eines Benutzers aus der Dropdown-Liste prüft dieser Rollen, denen der Benutzer angehört. Die Person, die Zugriff auf die Seite kann dann aktivieren oder deaktivieren Sie die Kontrollkästchen zum Hinzufügen oder entfernen den ausgewählten Benutzer aus den entsprechenden Rollen.

> [!NOTE]
> Verwenden eine Dropdown-Liste in die Benutzerkonten ist nicht perfekt für Websites, gibt es möglicherweise Hunderte von Benutzerkonten. Eine Dropdown-Liste wurde entwickelt, damit der Benutzer ein Element aus einer relativ kurzen Liste der Optionen auswählen kann. Es wird schnell unhandlich mit steigender Anzahl der Listenelemente. Wenn Sie eine Website erstellen, die potenziell großen Anzahl von Benutzerkonten verfügen sollen, sollten Sie in Betracht, eine alternative Benutzeroberfläche, z. B. den Besucher ein Buchstabe wählen werden aufgefordert, auslagerbaren GridView oder eine filterbaren-Schnittstelle, die auflistet und dann nur Zeigt die Benutzer, deren Benutzername mit dem ausgewählten Buchstaben beginnt.


## <a name="step-1-building-the-by-user-user-interface"></a>Schritt 1: Erstellen der Benutzeroberfläche "Von Benutzer"

Öffnen der `UsersAndRoles.aspx` Seite. Fügen Sie am oberen Rand der Seite, ein Web Label-Steuerelement namens `ActionStatus` und löschen, dessen `Text` Eigenschaft. Diese Bezeichnung zum Bereitstellen von Feedback zu den Aktionen, die ausgeführt wird, Anzeigen von Nachrichten verwendet werden, "Benutzer Tito hinzugefügt wurde der" Administratoren"" oder "Benutzer Jisun entzogen wurde die Rolle" Abteilungsleiter"." Damit diese auch Nachrichten hervorzuheben, legen Sie die Bezeichnung `CssClass` -Eigenschaft auf "Wichtig".

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample1.aspx)]

Fügen Sie den folgenden CSS-Klassendefinition, um die `Styles.css` Stylesheet:

[!code-css[Main](assigning-roles-to-users-cs/samples/sample2.css)]

Diese CSS-Definition der Browser angewiesen, die Bezeichnung, die große, roter Schrift angezeigt werden. Abbildung 1 zeigt die Auswirkung dieser über die Visual Studio-Designer.


[![Der Bezeichnung CssClass Eigenschaft führt eine große, Rot Schriftart](assigning-roles-to-users-cs/_static/image2.png)](assigning-roles-to-users-cs/_static/image1.png)

**Abbildung 1**: die Bezeichnung `CssClass` Eigenschaftenergebnisse im eine große und Rot-Schriftart ([klicken Sie hier, um das Bild in voller Größe angezeigt](assigning-roles-to-users-cs/_static/image3.png))


Als Nächstes fügen Sie eine DropDownList auf der Seite festlegen seiner `ID` Eigenschaft `UserList`, und legen Sie dessen `AutoPostBack` Eigenschaft auf "true". Wir verwenden diese DropDownList zum Auflisten aller Benutzer im System. Diese DropDownList wird an eine Auflistung von Objekten MembershipUser gebunden werden. Da der DropDownList die UserName-Eigenschaft des Objekts MembershipUser anzuzeigen (und verwenden es als Wert der Listenelemente) werden soll, Festlegen der DropDownList `DataTextField` und `DataValueField` Eigenschaften "UserName".

Fügen Sie unterhalb der DropDownList einen Repeater mit dem Namen `UsersRoleList`. Diese Repeater werden alle Rollen im System als eine Reihe von Kontrollkästchen aufgeführt. Definieren der Repeater `ItemTemplate` verwenden deklarative folgende Markup:

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample3.aspx)]

Die `ItemTemplate` Markup enthält ein einzelnes Kontrollkästchen Web-Steuerelement mit dem Namen `RoleCheckBox`. Des Kontrollkästchen `AutoPostBack` Eigenschaft auf "true" festgelegt ist und die `Text` Eigenschaft gebunden ist `Container.DataItem`. Der Grund hierfür die Databinding-Syntax ist einfach `Container.DataItem` ist, da das Framework Rollen gibt die Liste von Rollennamen als Zeichenfolgenarray zurück, und dieses Array von Zeichenfolgen, die Bindung an die Repeater werden wird. Eine ausführliche Beschreibung der warum diese Syntax verwendet wird, zeigt den Inhalt eines Arrays, die an ein Webserver-Steuerelement gebunden ist, den Rahmen dieses lehrprogramms sprengen. Weitere Informationen zu diesem Thema finden Sie unter [binden ein Skalar-Array an ein Daten-Websteuerelement](http://aspnet.4guysfromrolla.com/articles/082504-1.aspx).

An diesem Punkt sollte Ihre "von Benutzer" Schnittstelle deklarativem Markup etwa wie folgt aussehen:

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample4.aspx)]

Wir können nun den Code, um die Gruppe von Benutzerkonten für DropDownList und den Satz von Rollen zum Repeater binden zu schreiben. Fügen Sie in der Seite Code-Behind-Klasse, eine Methode namens `BindUsersToUserList` und eine andere mit dem Namen `BindRolesList`, mit dem folgenden Code:

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample5.cs)]

Die `BindUsersToUserList` Methode ruft alle Benutzerkonten im System über die [ `Membership.GetAllUsers` Methode](https://msdn.microsoft.com/library/dy8swhya.aspx). Dies gibt eine [ `MembershipUserCollection` Objekt](https://msdn.microsoft.com/library/system.web.security.membershipusercollection.aspx), dies ist eine Auflistung von [ `MembershipUser` Instanzen](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx). Klicken Sie dann an diese Auflistung gebunden ist die `UserList` DropDownList. Die `MembershipUser` Instanzen dieser Zusammensetzung die Auflistung enthalten eine Reihe von Eigenschaften, z. B. `UserName`, `Email`, `CreationDate`, und `IsOnline`. Damit weisen Sie der DropDownList zum Anzeigen des Werts der `UserName` -Eigenschaft, sicher, dass die `UserList` DropDownLists `DataTextField` und `DataValueField` Eigenschaften festgelegt wurden, "UserName".

> [!NOTE]
> Die `Membership.GetAllUsers` Methode verfügt über zwei Überladungen: eine, die keine Eingabeparameter akzeptiert und gibt alle Benutzer, und eine, die in Ganzzahlwerte für die Seite "Index" und "Seitengröße und gibt nur die angegebene Teilmenge der Benutzer zurück. Bei große Datenmengen Benutzerkonten, die in einem navigierbare Element der Benutzeroberfläche angezeigt wird, kann die zweite Überladung effizienter Seite über die Benutzer verwendet, da präzise Untermenge von Benutzerkonten anstelle aller Textknoten zurückgegeben.


Die `BindRolesToList` Methode startet durch Aufrufen der `Roles` -Klasse [ `GetAllRoles` Methode](https://msdn.microsoft.com/library/system.web.security.roles.getallroles.aspx), womit ein Zeichenfolgenarray mit den Rollen im System. Dieses Array von Zeichenfolgen wird dann an die Repeater gebunden.

Schließlich müssen wir diese beiden Methoden aufgerufen werden, wenn die Seite erstmals geladen wird. Fügen Sie dem `Page_Load`-Ereignishandler folgenden Code hinzu:

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample6.cs)]

Nehmen Sie einen Moment Zeit, um die Seite über einen Browser besuchen, mit diesem Code werden; Ihr Bildschirm sollte ähnlich wie in Abbildung 2 aussehen. Alle Benutzerkonten werden in der Dropdown-Liste und darunter, aufgefüllt jede Rolle wird als ein Kontrollkästchen angezeigt. Da wir setzen die `AutoPostBack` Eigenschaften der DropDownList und Kontrollkästchen auf "true", ändern den ausgewählten Benutzer oder aktivieren aus, oder Deaktivieren einer Rolle bewirkt, dass einen Postback. Keine Aktion wird jedoch ausgeführt, da wir noch über die zum Schreiben von Code aus, um diese Aktionen zu behandeln. Wir werden diese Aufgaben in den nächsten beiden Abschnitten konfigurieren.


[![Die Seite zeigt die Benutzer und Rollen](assigning-roles-to-users-cs/_static/image5.png)](assigning-roles-to-users-cs/_static/image4.png)

**Abbildung 2**: die Seite zeigt die Benutzer und Rollen ([klicken Sie hier, um das Bild in voller Größe angezeigt](assigning-roles-to-users-cs/_static/image6.png))


### <a name="checking-the-roles-the-selected-user-belongs-to"></a>Überprüfen die Rollen gehört der ausgewählten Benutzer zu

Beim ersten Laden der Seite ", oder wenn Besucher einen neuen Benutzer aus der Dropdown-Liste auswählt, müssen wir aktualisieren das `UsersRoleList`des Kontrollkästchen, damit eine bestimmte Rolle Kontrollkästchen aktiviert ist, nur dann, wenn der ausgewählte Benutzer, die dieser Rolle gehört. Um dies zu erreichen, erstellen Sie eine Methode namens `CheckRolesForSelectedUser` durch den folgenden Code:

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample7.cs)]

Der obige Code wird durch die bestimmt wird, wer der ausgewählte Benutzer wird gestartet. Es verwendet dann der Rollen Klasse [ `GetRolesForUser(userName)` Methode](https://msdn.microsoft.com/library/system.web.security.roles.getrolesforuser.aspx) auf den angegebenen Benutzer Rollen als ein Array von Zeichenfolgen zurück. Als Nächstes das DataRepeater-Elemente aufgelistet sind, und jedes Element `RoleCheckBox` Kontrollkästchen programmgesteuert verwiesen wird. Das Kontrollkästchen aktiviert ist, nur dann, wenn die Rolle "entspricht" sich innerhalb befindet der `selectedUsersRoles` Array von Zeichenfolgen.

> [!NOTE]
> Die `selectedUserRoles.Contains<string>(...)` Syntax wird nicht kompiliert, wenn Sie ASP.NET Version 2.0 verwenden. Die `Contains<string>` Methode ist Teil der [LINQ-Bibliothek](http://en.wikipedia.org/wiki/Language_Integrated_Query), also noch nicht mit ASP.NET 3.5. Wenn Sie noch ASP.NET Version 2.0 verwenden, verwenden Sie die [ `Array.IndexOf<string>` Methode](https://msdn.microsoft.com/library/eha9t187.aspx) stattdessen.


Die `CheckRolesForSelectedUser` Methode muss in zwei Fällen aufgerufen werden: beim ersten der Seite laden und bei jedem der `UserList` DropDownLists ausgewählten Index geändert wird. Daher rufen Sie diese Methode aus der `Page_Load` Ereignishandler (nach dem Aufrufen der `BindUsersToUserList` und `BindRolesToList`). Außerdem erstellen Sie einen Ereignishandler für die DropDownList `SelectedIndexChanged` Ereignis und Aufrufen dieser Methode von dort aus.

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample8.cs)]

Mit diesem Code werden können Sie die Seite über den Browser testen. Da jedoch die `UsersAndRoles.aspx` Seite fehlt derzeit die Möglichkeit zum Zuweisen von Benutzern zu Rollen, hat kein Benutzer Rollen. Erstellen wir die Schnittstelle für das Zuweisen von Benutzern zu Rollen in wenigen Augenblicken Dadurch können Sie dauern my Word, die dieser Code funktioniert und stellen Sie sicher, dass dies der Fall höher ist, oder Sie können Rollen manuell Benutzer hinzufügen, durch das Einfügen von Datensätzen in der `aspnet_UsersInRoles` Tabelle, um diese Functi testen jetzt Onality.

### <a name="assigning-and-removing-users-from-roles"></a>Zuweisen und Entfernen von Benutzern aus Rollen

Wenn der Besucher überprüft oder deaktiviert ein Kontrollkästchen in der `UsersRoleList` Repeater müssen wir hinzufügen oder entfernen den ausgewählten Benutzer aus der entsprechenden Rolle. Des Kontrollkästchen `AutoPostBack` Eigenschaftensatz wird derzeit auf "true", wodurch einen Postback wird jedes Mal, wenn ein Kontrollkästchen im Wiederholungsmodul aktiviert oder deaktiviert wird. Kurz gesagt, es müssen so erstellen Sie einen Ereignishandler für des Kontrollkästchen `CheckChanged` Ereignis. Da das Kontrollkästchen im Wiederholungsmodul-Steuerelement ist, müssen wir die Ereignis-Handler-Basisaufgaben manuell hinzufügen. Starten, indem Sie den Ereignishandler für die Code-Behind-Klasse als eine `protected` -Methode, wie folgt:

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample9.cs)]

Es wird zurückgegeben, um den Code im Anschluss für diesen Ereignishandler schreiben. Zunächst sehen wir Abschließen der Aufgaben, die für die Ereignisbehandlung. Über das Kontrollkästchen in der Repeater `ItemTemplate`, hinzufügen `OnCheckedChanged="RoleCheckBox_CheckChanged"`. Diese Syntax verbindet die `RoleCheckBox_CheckChanged` -Ereignishandler, um die `RoleCheckBox`des `CheckedChanged` Ereignis.

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample10.aspx)]

Unsere letzte Aufgabe ist die Ausführung der `RoleCheckBox_CheckChanged` -Ereignishandler. Wir uns zunächst durch Verweisen auf das CheckBox-Steuerelement, das das Ereignis ausgelöst hat, da diese Kontrollkästchen Instanz mitteilt welche Rolle checked oder unchecked über wurde die `Text` und `Checked` Eigenschaften. Mithilfe dieser Informationen zusammen mit dem Benutzernamen des ausgewählten Benutzers wir hinzufügen oder entfernen Sie den Benutzer aus der Rolle über die `Roles` Klasse [ `AddUserToRole` ](https://msdn.microsoft.com/library/system.web.security.roles.addusertorole.aspx) oder [ `RemoveUserFromRole` Methode](https://msdn.microsoft.com/library/system.web.security.roles.removeuserfromrole.aspx).

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample11.cs)]

Der obige Code startet durch Programmgesteuertes Verweisen auf das Kontrollkästchen, das das Ereignis ausgelöst hat, also über die `sender` Eingabeparameter. Wenn das Kontrollkästchen aktiviert ist, wird der ausgewählte Benutzer auf die angegebene Rolle ist, andernfalls hinzugefügt, die sie aus der Rolle entfernt werden. In beiden Fällen die `ActionStatus` Bezeichnung wird eine Meldung angezeigt, die gerade ausgeführte Aktion zusammenfassen.

Nehmen Sie einen Moment Zeit, um zu dieser Seite über einen Browser testen. Wählen Sie Benutzer Tito und anschließend Rollen "Administratoren" und "Abteilungsleiter Tito hinzugefügt.


[![Tito wurde hinzugefügt, die Administratoren und Abteilungsleiter-Rollen](assigning-roles-to-users-cs/_static/image8.png)](assigning-roles-to-users-cs/_static/image7.png)

**Abbildung 3**: Tito wurde hinzugefügt, die Administratoren und Abteilungsleiter-Rollen ([klicken Sie hier, um das Bild in voller Größe angezeigt](assigning-roles-to-users-cs/_static/image9.png))


Wählen Sie als Nächstes Benutzer Bruce aus der Dropdown-Liste. Es wird ein Postback und die Repeater Kontrollkästchen werden aktualisiert, über die `CheckRolesForSelectedUser`. Da keine Rollen noch nicht Bruce gehört, sind die zwei Kontrollkästchen deaktiviert. Fügen Sie Bruce danach die Rolle "Abteilungsleiter" hinzu.


[![Bruce wurde die Rolle "Abteilungsleiter" hinzugefügt](assigning-roles-to-users-cs/_static/image11.png)](assigning-roles-to-users-cs/_static/image10.png)

**Abbildung 4**: Bruce wurde um die Rolle "Abteilungsleiter" ([klicken Sie hier, um das Bild in voller Größe angezeigt](assigning-roles-to-users-cs/_static/image12.png))


Um die Funktionalität des Überprüfen der `CheckRolesForSelectedUser` wählen Sie einen anderen Benutzer als Tito oder Bruce-Methode. Beachten Sie, wie die Kontrollkästchen automatisch deaktiviert werden, wobei angegeben wird, dass sie keine Rollen zugeordnet. An Tito zurück. Sowohl Administratoren als auch Abteilungsleiter Kontrollkästchen sollte überprüft werden.

## <a name="step-2-building-the-by-roles-user-interface"></a>Schritt 2: Erstellen der Benutzeroberfläche "Durch die Rollen"

An diesem Punkt abgeschlossen haben die Schnittstelle "von Benutzer" und sind mit bewältigt Fortschritts die Schnittstelle "durch die Rollen" beginnen. Die Schnittstelle "durch die Rollen" fordert den Benutzer auf eine Rolle aus einer Dropdownliste auswählen, und zeigt dann die Gruppe der Benutzer, die zu dieser Rolle in einer GridView gehören.

Fügen Sie ein anderes DropDownList-Steuerelement, um die `UsersAndRoles.aspx` Seite. Positionieren Sie dieses Element unterhalb Wiederholungsmodul-Steuerelement, nennen Sie sie `RoleList`, und legen Sie dessen `AutoPostBack` Eigenschaft auf "true". Darunter, fügen Sie eine GridView hinzu, und nennen Sie sie `RolesUserList`. Diese GridView werden die Benutzer aufgelistet, die der ausgewählten Rolle angehören. Legen Sie der GridView `AutoGenerateColumns` Eigenschaft auf "false", ein TemplateField dem Raster hinzufügen `Columns` -Auflistung, und legen dessen `HeaderText` Eigenschaft auf "Benutzer". Definieren der TemplateField `ItemTemplate` , damit sie den Wert des Ausdrucks Databinding zeigt `Container.DataItem` in der `Text` Eigenschaft einer Bezeichnung, die mit dem Namen `UserNameLabel`.

Nach dem Hinzufügen und konfigurieren die GridView, sollte Ihre "von der Rolle" Schnittstelle deklarativem Markup etwa wie folgt aussehen:

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample12.aspx)]

Wir müssen zum Auffüllen der `RoleList` DropDownList mit dem Satz von Rollen im System. Um dies zu erreichen, Aktualisieren der `BindRolesToList` Methode, damit bindet ist das Zeichenfolgenarray zurückgegeben, durch die `Roles.GetAllRoles` Methode, um die `RolesList` DropDownList (als auch die `UsersRoleList` Repeater).

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample13.cs)]

Die letzten beiden Zeilen in der `BindRolesToList` Methode wurden hinzugefügt, um den Satz von Rollen zum Binden der `RoleList` DropDownList-Steuerelement. Abbildung 5 zeigt das Endergebnis, wenn Sie über einen Browser – eine Dropdown-Liste aufgefüllt wird, mit dem System-Rollen angezeigt.


[![Die Rollen werden in der RoleList DropDownList angezeigt.](assigning-roles-to-users-cs/_static/image14.png)](assigning-roles-to-users-cs/_static/image13.png)

**Abbildung 5**: die Rollen werden angezeigt, der `RoleList` DropDownList ([klicken Sie hier, um das Bild in voller Größe angezeigt](assigning-roles-to-users-cs/_static/image15.png))


### <a name="displaying-the-users-that-belong-to-the-selected-role"></a>Anzeigen von den Benutzern, die der ausgewählten Rolle angehören

Beim ersten Laden der Seite, oder wenn eine neue Rolle ausgewählt ist, aus der `RoleList` DropDownList, müssen wir die Liste der Benutzer anzuzeigen, die zu dieser Rolle in der GridView gehören. Erstellen Sie eine Methode mit dem Namen `DisplayUsersBelongingToRole` mit dem folgenden Code:

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample14.cs)]

Diese Methode startet durch Abrufen der ausgewählten Rolle aus der `RoleList` DropDownList. Es verwendet dann die [ `Roles.GetUsersInRole(roleName)` Methode](https://msdn.microsoft.com/library/system.web.security.roles.getusersinrole.aspx) ein Zeichenfolgenarray mit den Benutzernamen der Benutzer abgerufen, die dieser Rolle angehören. Dieses Array wird dann gebunden die `RolesUserList` GridView.

Diese Methode muss in zwei Fällen aufgerufen werden: beim erstmaligen der Seite laden, und wenn die ausgewählte Rolle in der `RoleList` DropDownList Änderungen. Aktualisieren Sie deshalb die `Page_Load` -Ereignishandler, damit diese Methode aufgerufen wird, nach dem Aufruf von `CheckRolesForSelectedUser`. Als Nächstes erstellen Sie einen Ereignishandler für das `RoleList`des `SelectedIndexChanged` Ereignis, und rufen Sie diese Methode von dort aus zu.

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample15.cs)]

Mit dem folgenden Code direkt in die `RolesUserList` GridView sollte angezeigt werden die Benutzer, die der ausgewählten Rolle angehören. Wie in Abbildung 6 gezeigt, besteht die Rolle "Abteilungsleiter" zwei Member: Bruce und Tito.


[![Die GridView Listet die Benutzer, die der ausgewählten Rolle angehören](assigning-roles-to-users-cs/_static/image17.png)](assigning-roles-to-users-cs/_static/image16.png)

**Abbildung 6**: die GridView Listet die Benutzer, gehören die Rolle "ausgewählt" ([klicken Sie hier, um das Bild in voller Größe angezeigt](assigning-roles-to-users-cs/_static/image18.png))


### <a name="removing-users-from-the-selected-role"></a>Entfernen von Benutzern aus der ausgewählten Rolle

Wir ergänzen die `RolesUserList` Schaltflächen GridView, enthalten eine Spalte mit "entfernen". Klicken auf die Schaltfläche "Entfernen" für einen bestimmten Benutzer entfernt sie aus dieser Rolle.

Starten Sie, indem Sie ein Feld löschen an die GridView hinzufügen. Stellen Sie dieses Feld als am häufigsten archivierten links angezeigt, und ändern Sie seine `DeleteText` -Eigenschaft wurde von "Delete" (Standard) auf "Entfernen".


[![Hinzufügen der](assigning-roles-to-users-cs/_static/image20.png)](assigning-roles-to-users-cs/_static/image19.png)

**Abbildung 7**: Fügen Sie die Schaltfläche "Entfernen", an die GridView ([klicken Sie hier, um das Bild in voller Größe angezeigt](assigning-roles-to-users-cs/_static/image21.png))


Beim Klicken auf die Schaltfläche "Entfernen" ein Postback erfolgt und der GridView `RowDeleting` Ereignis wird ausgelöst. Wir müssen einen Ereignishandler für dieses Ereignis erstellen und Code schreiben, der die Benutzer aus der ausgewählten Rolle entfernt. Erstellen Sie den Ereignishandler, und fügen Sie den folgenden Code hinzu:

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample16.cs)]

Der Code wird gestartet, durch den Namen der ausgewählten Rolle zu bestimmen. Es wird dann programmgesteuert Verweise der `UserNameLabel` Steuerelement aus der Zeile, deren "Remove"-Schaltfläche geklickt wurde, um zu bestimmen, den Benutzernamen des Benutzers entfernen. Der Benutzer ist dann entfernt, aus der Rolle über einen Aufruf an die `Roles.RemoveUserFromRole` Methode. Die `RolesUserList` GridView dann aktualisiert, und eine Meldung wird angezeigt, über die `ActionStatus` Label-Steuerelement.

> [!NOTE]
> Die Schaltfläche "Entfernen" ist keine Art von Bestätigung durch den Benutzer vor dem Entfernen des Benutzers aus der Rolle erforderlich. Ich einladen Maß Bestätigung durch den Benutzer hinzufügen. Einer der einfachsten Möglichkeiten, eine Aktion zu bestätigen erfolgt über einen clientseitigen bestätigen (Dialogfeld). Weitere Informationen zu dieser Technik finden Sie unter [Hinzufügen von clientseitigen Bestätigung beim Löschen von](https://asp.net/learn/data-access/tutorial-42-cs.aspx).


Abbildung 8 zeigt die Seite an, nach dem Benutzer Tito aus der Gruppe "Abteilungsleiter" entfernt wurde.


[![Leider ist Tito nicht mehr Supervisor](assigning-roles-to-users-cs/_static/image23.png)](assigning-roles-to-users-cs/_static/image22.png)

**Abbildung 8**: Leider Tito ist nicht mehr Supervisor ([klicken Sie hier, um das Bild in voller Größe angezeigt](assigning-roles-to-users-cs/_static/image24.png))


### <a name="adding-new-users-to-the-selected-role"></a>Hinzufügen neuer Benutzer zu der ausgewählten Rolle

Zusammen mit Benutzern aus der ausgewählten Rolle entfernen, sollte der Besucher zu dieser Seite auch der ausgewählten Rolle einen Benutzer hinzufügen können. Optimale Schnittstelle für das Hinzufügen eines Benutzers zu der ausgewählten Rolle hängt von der Anzahl von Benutzerkonten, die Sie erwarten. Wenn Ihre Website nur ein paar Dutzend Benutzerkonten gespeichert werden soll oder weniger beträgt, können Sie hier eine DropDownList verwenden. Wenn es möglicherweise Tausende von Benutzerkonten, möchten Sie eine Benutzeroberfläche einschließen, die den Besucher Seite über die Konten, suchen Sie nach einem bestimmten Konto oder Filtern die Benutzerkonten auf irgendeine andere zulässt.

Für diese Seite verwenden wir eine sehr einfache Schnittstelle, die unabhängig von der Anzahl von Benutzerkonten in das System arbeitet. D. h., verwenden wir ein Textfeld, und den Besucher den Benutzernamen des Benutzers eingeben, den sie der ausgewählten Rolle hinzufügen möchte aufgefordert. Wenn kein Benutzer mit diesem Namen vorhanden ist oder wenn der Benutzer bereits ein Mitglied der Rolle ist, es wird eine Meldung, in angezeigt `ActionStatus` Bezeichnung. Aber wenn der Benutzer vorhanden ist, und es kein Mitglied der Rolle ist, wir der Serverrolle hinzugefügt und das Raster zu aktualisieren.

Fügen Sie einem Textfeld und einer Schaltfläche unterhalb der GridView hinzu. Legen Sie des Textfelds `ID` auf `UserNameToAddToRole` und legen Sie der Schaltfläche `ID` und `Text` Eigenschaften `AddUserToRoleButton` und "Hinzufügen zu Benutzerrolle" bzw.

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample17.aspx)]

Als Nächstes erstellen Sie eine `Click` -Ereignishandler für die `AddUserToRoleButton` und fügen Sie den folgenden Code hinzu:

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample18.cs)]

Der Großteil des Codes in der `Click` -Ereignishandler führt verschiedene Überprüfungen. Damit sichergestellt, dass der Besucher einen Benutzernamen in bereitgestellten der `UserNameToAddToRole` Textfeld, das der Benutzer im System vorhanden ist, und sie nicht bereits in die ausgewählte Rolle gehören. Wenn eines dieser Überprüfung ein Fehler auftritt, wird eine entsprechende Meldung angezeigt, `ActionStatus` und der Ereignishandler beendet wird. Wenn alle Überprüfungen übergeben, wird der Benutzer hinzugefügt, in die Rolle über die `Roles.AddUserToRole` Methode. Nach, dass das Textfeld des `Text` Eigenschaft gelöscht wird, die GridView wird aktualisiert, und die `ActionStatus` Bezeichnung zeigt eine Meldung, dass der angegebene Benutzer der ausgewählten Rolle erfolgreich hinzugefügt wurde.

> [!NOTE]
> Verwenden, um sicherzustellen, dass der angegebene Benutzer nicht bereits in die ausgewählte Rolle gehört, wird die [ `Roles.IsUserInRole(userName, roleName)` Methode](https://msdn.microsoft.com/library/system.web.security.roles.isuserinrole.aspx), ein boolescher Wert, der zurückgegeben, ob *Benutzername* ist ein Mitglied *RoleName*. Wir verwenden diese Methode in der <a id="_msoanchor_2"> </a> [nächsten Lernprogramm](role-based-authorization-cs.md) beim Untersuchen wir rollenbasierte Autorisierung.


Besuchen Sie die Seite über einen Browser, und wählen Sie die Rolle "Abteilungsleiter" aus der `RoleList` DropDownList. Versuchen Sie, einen ungültigen Benutzernamen einzugeben: Daraufhin sollte eine Meldung angezeigt, dass der Benutzer im System nicht vorhanden ist.


[![Einen nicht existierende Benutzer kann nicht zu einer Rolle hinzugefügt werden.](assigning-roles-to-users-cs/_static/image26.png)](assigning-roles-to-users-cs/_static/image25.png)

**Abbildung 9**: Sie können ein nicht vorhandener Benutzer einer Rolle hinzufügen ([klicken Sie hier, um das Bild in voller Größe angezeigt](assigning-roles-to-users-cs/_static/image27.png))


Hinzufügen eines gültigen Benutzers wird nun versucht. Fahren Sie fort, und fügen Sie Tito erneut auf die Rolle "Abteilungsleiter" hinzu.


[![Tito wird noch einmal Supervisor!](assigning-roles-to-users-cs/_static/image29.png)](assigning-roles-to-users-cs/_static/image28.png)

**Abbildung 10**: Tito wird noch einmal Supervisor!  ([Klicken Sie hier, um das Bild in voller Größe angezeigt](assigning-roles-to-users-cs/_static/image30.png))


## <a name="step-3-cross-updating-the-by-user-and-by-role-interfaces"></a>Schritt 3: Aktualisieren von Cross der "Von Benutzer" und "Rolle" Schnittstellen

Die `UsersAndRoles.aspx` Seite bietet zwei unterschiedliche Benutzeroberflächen für die Verwaltung von Benutzern und Rollen. Derzeit werden diese beiden Benutzeroberflächen unabhängig voneinander fungieren, daher ist es möglich, dass eine Änderung in einer Schnittstelle wird nicht sofort in den anderen wiedergegeben werden. Angenommen, dass der Besucher auf der Seite "die Rolle" Abteilungsleiter "vom wählt die `RoleList` DropDownList, dem Bruce und Tito als Mitglieder aufgeführt sind. Anschließend wählt der Besucher Tito aus der `UserList` DropDownList, das die Kontrollkästchen für Administratoren und Abteilungsleiter überprüft wird, in der `UsersRoleList` Repeater. Wenn Besucher klicken Sie dann die Rolle des Vorgesetzten aus Repeater deaktiviert, Tito wird von der Rolle "Abteilungsleiter" entfernt, aber diese Änderung nicht vorgenommen wurde in der Schnittstelle "von der Rolle". Die GridView werden Tito weiterhin als ein Mitglied der Rolle "Abteilungsleiter" wird angezeigt.

Um dieses Problem zu beheben, müssen wir die GridView zu aktualisieren, wenn eine Rolle aktiviert oder deaktiviert aus wird, die `UsersRoleList` Repeater. Ebenso müssen wir Repeater aktualisieren, wenn ein Benutzer entfernt oder von der Schnittstelle "von der Rolle" zu einer Rolle hinzugefügt wird.

Die Repeater in der Schnittstelle "von Benutzer" wird aktualisiert, durch Aufrufen der `CheckRolesForSelectedUser` Methode. Die Schnittstelle "von der Rolle" kann geändert werden, der `RolesUserList` GridView `RowDeleting` -Ereignishandler und die `AddUserToRoleButton` Schaltfläche `Click` -Ereignishandler. Aus diesem Grund müssen wir rufen die `CheckRolesForSelectedUser` Methode von einer dieser Methoden.

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample19.cs)]

Auf ähnliche Weise wird die GridView in der Schnittstelle "von der Rolle" aktualisiert, durch Aufrufen der `DisplayUsersBelongingToRole` -Methode und die Schnittstelle "von Benutzer" wird geändert, durch die `RoleCheckBox_CheckChanged` -Ereignishandler. Aus diesem Grund müssen wir rufen die `DisplayUsersBelongingToRole` Methode aus diesem Ereignishandler.

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample20.cs)]

Mit diesen kleinere Änderungen am Code der "von Benutzer" und "Rolle" jetzt Schnittstellen richtig Cross-Update. Um dies zu überprüfen, besuchen Sie die Seite über einen Browser, und wählen Sie Tito und Abteilungsleiter aus der `UserList` und `RoleList` DropDownLists, bzw. Beachten Sie, wie Sie die Rolle "Abteilungsleiter" für Tito aus Repeater in der Schnittstelle "von Benutzer" deaktivieren, Tito automatisch aus der GridView in der Schnittstelle "von der Rolle" entfernt werden. Der Vorgesetzte Kontrollkästchen in der Schnittstelle "von Benutzer" Hinzufügen von Tito zurück an die Rolle "Abteilungsleiter" von der Schnittstelle "von der Rolle" automatisch erneut überprüft werden.

## <a name="step-4-customizing-the-createuserwizard-to-include-a-specify-roles-step"></a>Schritt 4: Anpassen der CreateUserWizard Einbeziehung von einem Schritt "geben Sie Rollen"

In der <a id="_msoanchor_3"> </a> [ *Erstellen von Benutzerkonten* ](../membership/creating-user-accounts-cs.md) Lernprogramm wurde erläutert, wie das Websteuerelement CreateUserWizard zu verwenden, um eine Schnittstelle, die zum Erstellen eines neuen Benutzerkontos bereitzustellen. Das Steuerelement CreateUserWizard kann auf zwei Arten verwendet werden:

- Als Mittel für Besucher erstellen ihr eigenes Benutzerkonto auf der Website, und
- Als eine Möglichkeit für Administratoren, um neue Konten zu erstellen.

In der ersten Anwendungsfall ein Besucher funktioniert, stammen von der Website und füllt die CreateUserWizard, ihre Informationen eingeben, um auf der Website zu registrieren. Im zweiten Fall erstellt ein Administrator ein neues Konto einer anderen Person.

Wenn Sie ein Konto von einem Administrator für einige andere Person erstellt wird, kann es hilfreich sein, kann der Administrator angeben, welche Rollen das neue Benutzerkonto gehört. In der <a id="_msoanchor_4"> </a> [ *das Speichern von* *zusätzliche Benutzerinformationen* ](../membership/storing-additional-user-information-cs.md) Lernprogramm wurde erläutert, wie die CreateUserWizard durch das Hinzufügen zusätzlicher anpassen `WizardSteps`. Sehen wir uns auf wie die CreateUserWizard einen zusätzlichen Schritt hinzugefügt, um den neuen Benutzer Rollen angeben.

Öffnen der `CreateUserWizardWithRoles.aspx` Seite, und fügen Sie ein CreateUserWizard-Steuerelement namens `RegisterUserWithRoles`. Legen Sie das Steuerelement `ContinueDestinationPageUrl` -Eigenschaft auf "~ /" default.aspx "". Da der Grundgedanke besteht, dass ein Administrator dieses Steuerelement CreateUserWizard mit dem neue Benutzerkonten erstellen, legen Sie des Steuerelements `LoginCreatedUser` Eigenschaft auf "false". Dies `LoginCreatedUser` Eigenschaft gibt an, ob der Besucher automatisch als die gerade erstellte Benutzer angemeldet ist, und wird auf "true standardmäßig". Wir legen Sie sie auf "false", da, wenn ein Administrator ein neues Konto erstellt, die wir ihn als selbst angemeldet beibehalten möchten.

Wählen Sie als Nächstes die "hinzufügen/entfernen `WizardSteps`..." aus der CreateUserWizard Smart Tag aus, und fügen Sie einen neuen `WizardStep`wird durch das Festlegen der `ID` auf `SpecifyRolesStep`. Verschieben Sie die `SpecifyRolesStep WizardStep` , damit sie nach dem Schritt "Anmelden für Ihr neues Konto" und vor der Ausführung des Schritts "Abschließen" stammen. Festlegen der `WizardStep`des `Title` -Eigenschaft auf "Geben Sie Roles", seine `StepType` Eigenschaft, um `Step`, und die zugehörige `AllowReturn` Eigenschaft auf "false".


[![Hinzufügen der](assigning-roles-to-users-cs/_static/image32.png)](assigning-roles-to-users-cs/_static/image31.png)

**Abbildung 11**: Hinzufügen von "Geben Sie Roles" `WizardStep` auf die CreateUserWizard ([klicken Sie hier, um das Bild in voller Größe angezeigt](assigning-roles-to-users-cs/_static/image33.png))


Nach dieser Änderung sollte Ihre CreateUserWizard deklarative Markup wie folgt aussehen:

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample21.aspx)]

"Geben Sie Mitglied der Rollen" `WizardStep`, hinzufügen eine CheckBoxList mit dem Namen `RoleList`. Diese CheckBoxList werden die verfügbaren Rollen aufgelistet und aktivieren die Person, die Besuchen der Seite, welche Rollen Überprüfen der neu erstellte Benutzer gehört.

Wir bleiben mit zwei Codierungsaufgaben: Zunächst müssen wir Auffüllen der `RoleList` CheckBoxList mit den Rollen im System; Zweitens müssen wir des Benutzers auf die ausgewählten Rollen hinzufügen, wenn der Benutzer aus dem Schritt "Geben Sie Roles" mit dem Schritt "Vollständig" bewegt. Wir können die erste Aufgabe beim Erreichen der `Page_Load` -Ereignishandler. Der folgende code programmgesteuert Verweise der `RoleList` Kontrollkästchen auf der ersten finden Sie auf der Seite und die Rollen im System an es gebunden wird.

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample22.cs)]

Der obige Code aussehen vertraut. In der <a id="_msoanchor_5"> </a> [ *das Speichern von* *zusätzliche Benutzerinformationen* ](../membership/storing-additional-user-information-cs.md) Lernprogramm wir verwendet zwei `FindControl` Anweisungen, die ein Websteuerelement verweisen von innerhalb einer benutzerdefinierten `WizardStep`. Und der Code, der die Rollen CheckBoxList gebunden wurde zuvor in diesem Lernprogramm entnommen.

Um die zweite Aufgabe als Programmierer auszuführen, den müssen wir wissen, wenn der Schritt "Angeben der Rollen" abgeschlossen wurde. Wie bereits erwähnt, die die CreateUserWizard hat ein `ActiveStepChanged` Ereignis, das jedes Mal ausgelöst wird, mit der Besucher, die von einer Stufe zu einem anderen navigiert. Hier können wir ermittelt, wenn der Benutzer im Schritt "Abgeschlossen" erreicht hat; Wenn dies der Fall ist, muss der Benutzer auf die ausgewählten Rollen hinzufügen.

Erstellen Sie einen Ereignishandler für das `ActiveStepChanged` Ereignis und fügen Sie den folgenden Code hinzu:

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample23.cs)]

Wenn der Benutzer nur den Schritt "Abgeschlossen" erreicht hat, wird der Ereignishandler Listet die Elemente von der `RoleList` CheckBoxList und den gerade erstellten Benutzer auf die ausgewählten Rollen zugewiesen ist.

Besuchen Sie diese Seite über einen Browser. Der erste Schritt bei der CreateUserWizard ist der standard "Anmelden für Ihr neues Konto" Schritt, der für den neuen Benutzernamen, Kennwörter, e-Mail und andere wichtige Informationen aufgefordert werden. Geben Sie die Informationen zum Erstellen eines neuen Benutzers mit dem Namen Wanda aus.


[![Erstellen eines neuen Benutzers mit dem Namen Wanda](assigning-roles-to-users-cs/_static/image35.png)](assigning-roles-to-users-cs/_static/image34.png)

**Abbildung 12**: Erstellen Sie einen neuen Benutzer mit dem Namen Wanda ([klicken Sie hier, um das Bild in voller Größe angezeigt](assigning-roles-to-users-cs/_static/image36.png))


Klicken Sie auf die Schaltfläche "Benutzer erstellen". Ruft die CreateUserWizard intern die `Membership.CreateUser` Methode und erstellt das neue Benutzerkonto ein, und klicken Sie dann im Verlauf der Arbeit mit dem nächsten Schritt "Geben Sie Rollen." Hier werden die Standortsystemrollen aufgeführt. Aktivieren Sie das Kontrollkästchen Abteilungsleiter, und klicken Sie auf Weiter.


[![Stellen Sie ein Mitglied der Rolle "Abteilungsleiter" Wanda](assigning-roles-to-users-cs/_static/image38.png)](assigning-roles-to-users-cs/_static/image37.png)

**Abbildung 13**: Stellen Sie ein Mitglied der Rolle "Abteilungsleiter" Wanda ([klicken Sie hier, um das Bild in voller Größe angezeigt](assigning-roles-to-users-cs/_static/image39.png))


Klicken weiter bewirkt, dass ein Postback und Updates der `ActiveStep` mit dem Schritt "Fertig stellen". In der `ActiveStepChanged` Ereignishandler, d. h. das zuletzt erstellte Benutzerkonto ist die Rolle "Abteilungsleiter" zugewiesen. Um dies zu überprüfen, zurückgeben, um die `UsersAndRoles.aspx` Seite und wählen Sie Abteilungsleiter aus der `RoleList` DropDownList. Wie in Abbildung 14 gezeigt, die Abteilungsleiter jetzt bestehen aus drei Benutzer: Bruce Tito und Wanda.


[![Bruce Tito und Wanda sind alle Abteilungsleiter](assigning-roles-to-users-cs/_static/image41.png)](assigning-roles-to-users-cs/_static/image40.png)

**Abbildung 14**: Bruce Tito und Wanda sind alle Abteilungsleiter ([klicken Sie hier, um das Bild in voller Größe angezeigt](assigning-roles-to-users-cs/_static/image42.png))


## <a name="summary"></a>Zusammenfassung

Die Rollen-Framework bietet Methoden zum Abrufen von Informationen zu eines bestimmten Benutzers Rollen und Methoden zum bestimmen, welche Benutzer zu einer bestimmten Rolle gehören. Darüber hinaus sind eine Reihe von Methoden zum Hinzufügen und entfernen einen oder mehrere Benutzer auf eine oder mehrere Rollen. In diesem Lernprogramm wir konzentrierten uns auf nur zwei der folgenden Methoden: `AddUserToRole` und `RemoveUserFromRole`. Es gibt zusätzliche Varianten konzipiert, dass mehrere Benutzer zu einer einzigen Rolle hinzufügen und einen einzelnen Benutzer mehreren Rollen zuweisen.

In diesem Lernprogramm enthalten auch einen Blick auf das Steuerelement CreateUserWizard einschließen Erweitern einer `WizardStep` Rollen des neu erstellten Benutzers angeben. Eine solche Maßnahme können einen Administrator den Prozess zum Erstellen von Benutzerkonten für neue Benutzer zu optimieren.

Zu diesem Zeitpunkt haben wir gesehen, wie zum Erstellen und Löschen von Rollen und wie Benutzer hinzufügen oder Entfernen von Rollen. Jedoch können wir noch Uhrzeitstempel rollenbasierte Autorisierung anwenden. In der <a id="_msoanchor_6"> </a> [folgenden Lernprogramm](role-based-authorization-cs.md) betrachten wir definieren URL-Autorisierungsregeln auf der Grundlage einer Rolle von Rolle ebenfalls wie Seitenebene Funktionalitäten des aktuell angemeldeten Benutzers Rollen basierend auf.

Viel Spaß beim Programmieren!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Lernprogramm erläutert finden Sie in den folgenden Ressourcen:

- [ASP.NET Web Site Administration Tool-Übersicht](https://msdn.microsoft.com/library/ms228053.aspx)
- [ASP wird untersucht. NET Mitgliedschaft, Rollen und Profile](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Paralleles eigene Websiteverwaltungs-Tool](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)

### <a name="about-the-author"></a>Informationen zum Autor

Scott Mitchell, Autor von mehreren ASP/ASP.NET-Büchern und Gründer von 4GuysFromRolla.com, bereits seit 1998 mit Microsoft-Web-Technologien gearbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird  *[Sams Schulen selbst ASP.NET 2.0 in 24 Stunden](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott erreicht werden kann, zur [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) oder über seinen Blog unter [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Besonderen Dank an...

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Lead Prüfer für dieses Lernprogramm wurde Teresa Murphy. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise[mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Zurück](creating-and-managing-roles-cs.md)
[Weiter](role-based-authorization-cs.md)
