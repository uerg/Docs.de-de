---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
title: 'Teil 9: Registrierung und Kasse | Microsoft Docs'
author: jongalloway
description: Diese Reihe von Lernprogrammen details aller die Schritte zum Erstellen von ASP.NET MVC-Music Store-beispielanwendung. Registrierung und Auschecken, werden Teil 9 behandelt.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: d65c5c2b-a039-463f-ad29-25cf9fb7a1ba
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
msc.type: authoredcontent
ms.openlocfilehash: e7e83b70f2508b6dfc0c078b992747a76e4d0ff2
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870114"
---
<a name="part-9-registration-and-checkout"></a>Teil 9: Registrierung und Kasse
====================
durch [Jon Galloway](https://github.com/jongalloway)

> Das MVC-Music Store ist eine lernprogrammanwendung, die führt und erklärt schrittweise, wie mithilfe von ASP.NET MVC und Visual Studio für die Webentwicklung.  
>   
> Das MVC-Music Store handelt es sich um einfache Beispiel Store Implementierung, die online Musikalben verkauft und implementiert grundlegende standortverwaltung Benutzer anmelden und shopping Cart-Funktionalität.  
>   
> Diese Reihe von Lernprogrammen details aller die Schritte zum Erstellen von ASP.NET MVC-Music Store-beispielanwendung. Registrierung und Auschecken, werden Teil 9 behandelt.


In diesem Abschnitt wird eine CheckoutController erstellen wir werden die Adresse des Käufers und Zahlungsinformationen gesammelt werden. Wir müssen Benutzer mit unserer Website vor dem Auschecken, registrieren Sie sich damit diesen Controller Autorisierung erforderlich ist.

Benutzer werden zu des Auscheckvorgangs aus Einkaufswagen navigieren, indem Sie auf die Schaltfläche "Kasse".

![](mvc-music-store-part-9/_static/image1.jpg)

Wenn der Benutzer nicht angemeldet ist, wird er auf aufgefordert werden.

![](mvc-music-store-part-9/_static/image1.png)

Nach der erfolgreichen Anmeldung wird der Benutzer dann die Adresse "und" Payment-Ansicht angezeigt.

![](mvc-music-store-part-9/_static/image2.png)

Nachdem sie das Formular ausgefüllt und übermittelt die Reihenfolge, werden sie dem Bestätigungsbildschirm Reihenfolge angezeigt werden.

![](mvc-music-store-part-9/_static/image3.png)

Versuchen, eine nicht existierende Bestellung oder eine Bestellung, die Ihnen gehört anzuzeigen, wird der Fehler-Sicht angezeigt.

![](mvc-music-store-part-9/_static/image4.png)

## <a name="migrating-the-shopping-cart"></a>Migrieren den Einkaufswagen

Während des Einkaufsvorgangs anonym ist, wird der Benutzer klickt auf die Schaltfläche "Auschecken", werden sie beim Registrieren benötigt und melden Sie sich. Benutzer erwarten, dass, dass wir ihre Einkaufswagendaten zwischen Besuchen bewahren, damit ein Benutzer die Einkaufswagendaten zuordnen, wenn sie die Registrierung oder Anmeldung abgeschlossen werden muss.

Dies ist tatsächlich sehr einfach zum Zweck, wie unsere ShoppingCart-Klasse bereits über eine Methode verfügt, die alle Elemente in der aktuellen Einkaufswagen mit einem Benutzernamen zuordnen. Wir benötigen nur diese Methode aufgerufen, wenn ein Benutzer auf Registrierung oder Anmeldung abgeschlossen ist.

Öffnen der **AccountController** -Klasse, die wir hinzugefügt, wenn wir Mitgliedschaft einrichten und der Autorisierung wurden. Hinzufügen einer Anweisung verweisen auf MvcMusicStore.Models, dann fügen Sie die folgenden MigrateShoppingCart-Methode:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample1.cs)]

Im nächsten Schritt ändern Sie die Aktion nach der Anmeldung um MigrateShoppingCart aufgerufen wird, nachdem der Benutzer überprüft wurde, wie unten dargestellt:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample2.cs)]

Stellen Sie die gleiche Änderung registriert wird, die Aktion, post, sofort, nachdem das Benutzerkonto erfolgreich erstellt wurde:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample3.cs)]

Das ist alles -, jetzt eine anonyme Einkaufswagen ein Benutzerkonto nach erfolgreicher Registrierung oder Anmeldung automatisch übertragen werden sollen.

## <a name="creating-the-checkoutcontroller"></a>Erstellen die CheckoutController

Mit der rechten Maustaste auf den Ordner Controller, und fügen Sie einen neuen Domänencontroller für das Projekt mit dem Namen CheckoutController mithilfe der Vorlage für leere Controller hinzu.

![](mvc-music-store-part-9/_static/image5.png)

Fügen Sie zunächst die Authorize-Attribut über die Klassendeklaration Controller Benutzer registrieren, bevor Sie Auschecken erforderlich ist:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample4.cs)]

*Hinweis: Dies ist vergleichbar mit der Änderung, die wir zuvor die StoreManagerController vorgenommen haben, aber in diesem Fall wird in Authorize-Attribut erforderlich sind, dass der Benutzer Mitglied einer Administratorrolle sein. Im Controller auschecken möchten wir erfordern, der Benutzer angemeldet sein, jedoch sind nicht erfordern, dass diese Administratoren.*

Der Einfachheit halber wird nicht wir Umgang mit Zahlungsinformationen in diesem Lernprogramm werden. Stattdessen werden wir Benutzer sehen Sie sich mithilfe eines Werbe-Codes ermöglicht. Wir werden diesen Angebotscode eine Konstante mit dem Namen PromoCode gespeichert.

Ein Feld an eine Instanz der MusicStoreEntities-Klasse, die mit dem Namen StoreDB halten müssen wie in der StoreController deklariert werden. Damit auch der MusicStoreEntities-Klasse verwenden, müssen wir hinzufügen, eine using-Anweisung für den MvcMusicStore.Models-Namespace. Am Anfang unserer Checkout-Controller wird unten angezeigt.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample5.cs)]

Die CheckoutController müssen die folgenden Controlleraktionen:

**AddressAndPayment (GET-Methode)** zeigt ein Formular, um den Benutzer zur Eingabe ihrer Informationen zu ermöglichen.

**AddressAndPayment (POST-Methode)** wird die Überprüfung der Eingabe- und die Reihenfolge zu verarbeiten.

**Vollständige** wird angezeigt, nachdem ein Benutzer des Auscheckvorgangs erfolgreich beendet wurde. In dieser Ansicht wird als Bestätigung des Benutzers Reihenfolgennummer enthalten.

Erstens wir benennen die Index-Controlleraktion (die generiert wurde, wenn wir den Controller erstellt), AddressAndPayment. Diese Controlleraktion zeigt nur das Formular Auschecken alle Modellinformationen benötigen Sie nicht.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample6.cs)]

Unsere AddressAndPayment POST-Methode wird folgen dem gleichen Muster, die wir in der StoreManagerController verwendet: Es wird versucht, die Übermittlung des Formulars übernehmen und die Reihenfolge zu beenden und das Formular erneut angezeigt, wenn ein Fehler auftritt.

Nach unserer Prüfungsanforderungen für einen Auftrag überprüfen die Formulareingabe erfüllt werden, werden wir die PromoCode Formularwert direkt überprüfen. Vorausgesetzt, dass alles richtig ist, wird es speichert die aktualisierte Informationen mit der Reihenfolge, sagen Sie das ShoppingCart-Objekt, das den Auftrag abzuschließen, und leiten Sie die Aktion abgeschlossen.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample7.cs)]

Nach erfolgreichem Abschluss des Prozesses Auschecken werden Benutzer auf die Aktion der controllerrolle abschließen umgeleitet. Diese Aktion wird einer einfachen Überprüfung, um zu überprüfen, dass der Auftrag tatsächlich für den angemeldeten Benutzer gehört, bevor Sie mit der Bestellnummer als Bestätigung.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample8.cs)]

*Hinweis: Die Fehleransicht wurde automatisch erstellt für uns im Ordner "/Views/Shared", wenn wir das Projekt wurde gestartet.*

Der vollständige CheckoutController Code lautet wie folgt:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample9.cs)]

## <a name="adding-the-addressandpayment-view"></a>Hinzufügen der AddressAndPayment-Ansicht

Jetzt erstellen wir die AddressAndPayment-Sicht. Mit der rechten Maustaste auf eine der Aktionen AddressAndPayment Controller, und fügen Sie eine Ansicht mit dem Namen AddressAndPayment stark typisiert ist, als eine Bestellung und verwendet die Vorlage bearbeiten, wie unten dargestellt.

![](mvc-music-store-part-9/_static/image6.png)

In dieser Ansicht wird stellen zwei Techniken erläutert, während der Erstellung der Sicht StoreManagerEdit verwenden:

- Wir verwenden Html.EditorForModel(), um Felder für das Modell Reihenfolge anzeigen
- Wir werden Validierungsregeln Validierungsattribute Order-Klasse mit nutzen.

Wir beginnen mit den Formularcode, um Html.EditorForModel(), gefolgt von einer zusätzlichen Textfeld für die Promo-Code aktualisieren. Der vollständige Code für die Sicht AddressAndPayment ist unten dargestellt.

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample10.cshtml)]

## <a name="defining-validation-rules-for-the-order"></a>Definieren von Validierungsregeln für den Auftrag

Nun, dass unsere Ansicht eingerichtet ist, wird die Validierungsregeln für unsere Reihenfolge Modell richten wir wir zuvor für das Modell Album ausgeführt haben. Mit der rechten Maustaste auf den Ordner Models, und fügen Sie eine Klasse mit dem Namen Reihenfolge. Neben den Attributen der Überprüfung, die wir zuvor für das Album verwendet, werden auch einen regulären Ausdruck verwendet. um die e-Mail-Adresse des Benutzers zu überprüfen.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample11.cs)]

Versuch, das Formular aus, mit fehlendem oder ungültige Informationen wird jetzt die clientseitige Validierung mit Fehlermeldung angezeigt.

![](mvc-music-store-part-9/_static/image7.png)

Zugegeben, haben wir die meisten die harte Arbeit für des Auscheckvorgangs; Wir haben nur wenige Odds und endet, um den Vorgang abzuschließen. Es müssen zwei Ansichten für das einfache hinzufügen, und wir die Übergabe der Einkaufswagen Informationen während des Anmeldevorgangs kümmern müssen.

## <a name="adding-the-checkout-complete-view"></a>Hinzufügen der Ansicht für die vollständige Auschecken

Die Auschecken vollständige Ansicht ist ganz einfach, da es lediglich die Auftrags-ID anzeigen muss Mit der rechten Maustaste auf die Aktion der controllerrolle abschließen, und fügen Sie eine Ansicht mit dem Namen Fertig stellen die stark typisiert als ein "int".

![](mvc-music-store-part-9/_static/image8.png)

Jetzt werden wir den Code anzeigen, um die Bestell-ID anzeigen aktualisieren, wie unten dargestellt.

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample12.cshtml)]

## <a name="updating-the-error-view"></a>Aktualisieren der Fehleransicht

Die Standardvorlage enthält ein Fehleransicht im Ordner Views freigegeben, sodass wiederverwendeten an anderer Stelle in der Website möglich. Dieser Fehler-Ansicht enthält einen sehr einfachen Fehler und unserer Website Layout, nicht verwendet werden, damit wir aktualisiert werden müssen.

Da dies eine generische Fehlerseite ist, ist der Inhalt sehr einfach. Es wird eine Nachricht und einen Link, um zur vorherigen Seite im Verlauf zu navigieren, wenn der Benutzer möchte ihre Aktion erneut versuchen, das enthalten.

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample13.cshtml)]


> [!div class="step-by-step"]
> [Zurück](mvc-music-store-part-8.md)
> [Weiter](mvc-music-store-part-10.md)
