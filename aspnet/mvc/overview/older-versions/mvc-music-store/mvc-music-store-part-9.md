---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
title: 'Teil 9: Registrierung und Bezahlvorgang | Microsoft-Dokumentation'
author: jongalloway
description: Dieser tutorialreihe werden alle Schritte ausgeführt, um die ASP.NET MVC Music Store-beispielanwendung zu erstellen. Teil 9 sind die Registrierung und Bezahlvorgang.
ms.author: aspnetcontent
ms.date: 04/21/2011
ms.assetid: d65c5c2b-a039-463f-ad29-25cf9fb7a1ba
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
msc.type: authoredcontent
ms.openlocfilehash: e521e30cb820d834d57c87538b869861a536657b
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37812872"
---
<a name="part-9-registration-and-checkout"></a>Teil 9: Registrierung und Bezahlvorgang
====================
durch [Jon Galloway](https://github.com/jongalloway)

> Die MVC Music Store ist ein lernprogrammanwendung, die eingeführt und erläutert Schritt für Schritt, wie ASP.NET MVC und Visual Studio für die Webentwicklung verwenden.  
>   
> Die MVC Music Store ist eine Implementierung eines einfachen Beispiels die Alben online verkauft und implementiert grundlegende Verwaltung, Benutzeranmeldung und shopping Cart-Funktionalität.  
>   
> Dieser tutorialreihe werden alle Schritte ausgeführt, um die ASP.NET MVC Music Store-beispielanwendung zu erstellen. Teil 9 sind die Registrierung und Bezahlvorgang.


In diesem Abschnitt werden wir eine CheckoutController erstellen die des Kunden-Adresse und die Zahlungsinformationen gesammelt werden. Wir müssen die Benutzer mit unserer Website vor dem Auschecken, zu registrieren, damit es sich bei diesem Controller Autorisierung erforderlich ist.

Benutzer werden zur des Auscheckvorgangs aus ihrem Einkaufswagen navigieren, auf die Schaltfläche "Kasse" klicken.

![](mvc-music-store-part-9/_static/image1.jpg)

Wenn der Benutzer nicht angemeldet ist, wird er zum aufgefordert werden.

![](mvc-music-store-part-9/_static/image1.png)

Nach der erfolgreichen Anmeldung wird dem Benutzer dann die Ansicht und die Zahlungsmethode angezeigt.

![](mvc-music-store-part-9/_static/image2.png)

Nachdem sie das Formular ausgefüllt und übermittelt die Reihenfolge, werden sie dem Order-Bestätigungsbildschirm angezeigt.

![](mvc-music-store-part-9/_static/image3.png)

Es wird versucht, entweder eine nicht existierende Bestellung oder eine Bestellung, die Ihnen gehören nicht, anzeigen, wird die Fehleransicht angezeigt.

![](mvc-music-store-part-9/_static/image4.png)

## <a name="migrating-the-shopping-cart"></a>Migrieren des Einkaufswagens

Während des Einkaufsvorgangs anonym ist, wird, wenn der Benutzer auf die Schaltfläche zum Auschecken klickt, sie müssen zum Registrieren und melden Sie sich. Benutzer erwarten, dass, dass wir die Einkaufswagendaten Abonnenten‟, daher müssen wir die Einkaufswagendaten für einen Benutzer zuordnen, wenn sie die Registrierung oder Anmeldung abgeschlossen beibehält.

Dies ist tatsächlich sehr einfach ist, an, wie unsere ShoppingCart-Klasse bereits eine Methode verfügt über die alle Elemente in der aktuellen Einkaufswagen mit einem Benutzernamen zuordnen. Wir benötigen nur diese Methode aufgerufen, wenn ein Benutzer auf Registrierung oder Anmeldung abgeschlossen ist.

Öffnen der **AccountController** -Klasse, die wir hinzugefügt, wenn wir Sie Mitgliedschaft und Autorisierung einrichten wurden. Hinzufügen einer using-Anweisung verweisen auf MvcMusicStore.Models, dann fügen Sie die folgende MigrateShoppingCart-Methode:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample1.cs)]

Als Nächstes ändern Sie die Aktion nach der Anmeldung zum MigrateShoppingCart aufrufen, nachdem der Benutzer überprüft wurde, wie unten dargestellt:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample2.cs)]

Stellen Sie die gleiche Änderung das Register-Aktion zu buchen, sofort, nachdem das Benutzerkonto erfolgreich erstellt wurde:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample3.cs)]

Das ist alles – nun eine anonyme Einkaufswagen eines Benutzerkontos nach erfolgreicher Registrierung oder Anmeldung automatisch übertragen werden sollen.

## <a name="creating-the-checkoutcontroller"></a>Erstellen die CheckoutController

Mit der rechten Maustaste auf den Ordner "Controllers", und fügen Sie einen neuen Controller zum-Projekt namens CheckoutController mithilfe der Vorlage der leeren Controller hinzu.

![](mvc-music-store-part-9/_static/image5.png)

Fügen Sie zunächst das Authorize-Attribut über der Controller-Klassendeklaration Benutzer registrieren, vor dem Auschecken erforderlich ist:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample4.cs)]

*Hinweis: Dies ist vergleichbar mit der Änderung, die wir zuvor an die StoreManagerController vorgenommen, aber in diesem Fall wird in das Authorize-Attribut erforderlich sind, dass der Benutzer Mitglied in einer Administratorrolle sein. Im Controller Auschecken sind wir erfordern, der Benutzer angemeldet sein, jedoch sind nicht erfordern, dass sie sich Administratoren.*

Der Einfachheit halber wird nicht wir mit Zahlungsinformationen in diesem Tutorial zu tun. Stattdessen lassen wir die Benutzer sehen Sie sich mit einem Angebotscode. Wir werden diese über eine Konstante, die mit dem Namen PromoCode Angebotscode gespeichert.

Wie in der StoreController werden wir ein Feld für eine Instanz der MusicStoreEntities-Klasse, mit dem Namen StoreDB deklarieren. Um Stellen der MusicStoreEntities-Klasse verwenden, müssen wir zum Hinzufügen einer using-Anweisung für den MvcMusicStore.Models-Namespace. Der oberste des Controllers Auschecken wird unten angezeigt.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample5.cs)]

Die CheckoutController verfügen die folgenden Controlleraktionen:

**AddressAndPayment (GET-Methode)** zeigt ein Formular, um dem Benutzer ermöglichen, ihre Informationen eingeben.

**AddressAndPayment (POST-Methode)** wird die Überprüfung der Eingabe- und die Reihenfolge zu verarbeiten.

**Vollständige** wird angezeigt, nachdem ein Benutzer den Kassenvorgang erfolgreich abgeschlossen wurde. In dieser Ansicht werden fortlaufende Nummer des Benutzers, als Bestätigung enthalten.

Zuerst benennen Sie die Index-Controlleraktion (die generiert wurde, beim Erstellen des Controllers) lassen Sie uns in AddressAndPayment. Diese Controlleraktion zeigt nur das Formular Auschecken, damit alle Informationen zum Modell erforderlich ist.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample6.cs)]

Unsere AddressAndPayment POST-Methode wird folgen dem gleichen Muster, die wir in den StoreManagerController verwendet: Es wird versucht, akzeptieren die Übermittlung des Formulars, und führen die Reihenfolge und das Formular wird erneut angezeigt werden, wenn ein Fehler auftritt.

Nach unserem überprüfungsanforderungen für einen Auftrag überprüfen die Formulareingabe erfüllt werden, werden wir den PromoCode-Formular-Wert direkt überprüfen. Vorausgesetzt, dass alles korrekt ist, speichern wir die aktualisierte Informationen mit der Reihenfolge, teilen Sie das ShoppingCart-Objekt, das den Auftrag abzuschließen, und leiten Sie an die Aktion abgeschlossen.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample7.cs)]

Nach dem erfolgreichen Abschluss des Prozesses Auschecken werden Benutzer auf die vollständige Controlleraktion umgeleitet. Diese Aktion wird eine einfache Überprüfung, um sicherzustellen, dass die Reihenfolge tatsächlich für den angemeldeten Benutzer gehört, bevor Sie mit der Bestellnummer als Bestätigung durchführen.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample8.cs)]

*Hinweis: Die Fehleransicht wurde automatisch erstellt, für uns im Ordner "/Views/Shared" als wir zu des Projekts Beginn.*

Der vollständige CheckoutController Code lautet wie folgt aus:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample9.cs)]

## <a name="adding-the-addressandpayment-view"></a>Hinzufügen der AddressAndPayment-Ansicht

Nun erstellen wir die AddressAndPayment-Ansicht. Mit der rechten Maustaste auf eine der Aktionen AddressAndPayment Controller aus, und fügen Sie eine Ansicht mit dem Namen AddressAndPayment ist stark typisiert, wie eine Bestellung und verwendet die Vorlage bearbeiten, wie unten dargestellt.

![](mvc-music-store-part-9/_static/image6.png)

In dieser Ansicht wird stellen zwei Verfahren erläutert, während der Erstellung der Sicht StoreManagerEdit verwenden:

- Wir verwenden Html.EditorForModel(), um Felder für das Modell Reihenfolge anzeigen
- Wir werden Validierungsregeln, die mit einer Order-Klasse mit Validierungsattributen nutzen.

Wir beginnen, durch die Aktualisierung der Formularcode, um Html.EditorForModel(), gefolgt von einer zusätzlichen Textfeld für die Promo-Code verwenden. Der vollständige Code für die Ansicht AddressAndPayment ist unten dargestellt.

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample10.cshtml)]

## <a name="defining-validation-rules-for-the-order"></a>Definieren von Validierungsregeln für den Auftrag

Nun, da unsere Ansicht eingerichtet ist, werden wir die Validierungsregeln für unser Modell der Reihenfolge eingerichtet wie schon zuvor für das Album-Modell. Mit der rechten Maustaste auf den Ordner "Models", und fügen Sie eine Klasse, die mit dem Namen Bestellung. Zusätzlich zu den Validierungsattributen, die wir zuvor für das Album verwendet wird, werden wir auch einen regulären Ausdruck verwenden, um die e-Mail-Adresse des Benutzers zu überprüfen.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample11.cs)]

Es wird versucht, senden Sie das Formular mit fehlenden oder ungültige Informationen werden nun die clientseitige Validierung mit Fehlermeldung angezeigt.

![](mvc-music-store-part-9/_static/image7.png)

Okay, haben wir die meisten die harte Arbeit für den Kassenvorgang ausgeführt; Wir müssen lediglich ein paar Wahrscheinlichkeiten und endet um den Vorgang abzuschließen. Wir müssen zwei einfache Ansichten hinzufügen, und wir die Übergabe der Warenkorb Informationen während des Anmeldevorgangs kümmern müssen.

## <a name="adding-the-checkout-complete-view"></a>Die vollständige Auschecken Ansicht hinzufügen

Die vollständige Auschecken Ansicht ist ziemlich einfach, wie er lediglich die Auftrags-ID anzeigen muss Mit der rechten Maustaste auf die vollständige Controlleraktion aus, und fügen Sie eine Ansicht mit dem Namen abschließen, die als ganze Zahl stark typisiert ist

![](mvc-music-store-part-9/_static/image8.png)

Jetzt aktualisieren wir den Code anzeigen, zum Anzeigen der Auftrags-ID, wie unten dargestellt.

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample12.cshtml)]

## <a name="updating-the-error-view"></a>Aktualisieren die Fehler anzeigen

Die Standardvorlage enthält eine Fehleransicht im freigegebenen Ordner "Views", so, dass es nicht erneut verwendet an anderer Stelle auf der Website werden kann. Dieser Fehler-Ansicht enthält einen sehr einfachen Fehler und der Layout-Website nicht verwendet werden, damit wir ihn aktualisieren müssen.

Da dies eine generische Fehlerseite ist, wird der Inhalt sehr einfach. Wir nehmen eine Nachricht und einen Link, um zur vorherigen Seite im Verlauf zu navigieren, wenn möchte, dass der Benutzer die Aktion erneut versucht.

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample13.cshtml)]


> [!div class="step-by-step"]
> [Zurück](mvc-music-store-part-8.md)
> [Weiter](mvc-music-store-part-10.md)
