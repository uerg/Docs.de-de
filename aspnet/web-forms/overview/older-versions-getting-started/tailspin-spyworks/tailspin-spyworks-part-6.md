---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
title: 'Teil 6: ASP.NET-Mitgliedschaft | Microsoft Docs'
author: JoeStagner
description: Diese Reihe von Lernprogrammen sind alle Schritte ausgeführt, um die beispielanwendung Tailspin Spyworks erstellen. Teil 6 fügt ASP.NET-Mitgliedschaft hinzu.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: f70a310c-9557-4743-82cb-655265676d39
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
msc.type: authoredcontent
ms.openlocfilehash: 83e9bc780ea8face3e0f55fdf8c00e13b60f80a7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="part-6-aspnet-membership"></a>Teil 6: ASP.NET-Mitgliedschaft
====================
durch [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks wird veranschaulicht, wie außergewöhnlich einfache ist die leistungsstarke, skalierbare Anwendungen für .NET-Plattform zu erstellen. Es wird gezeigt, aus wie die hervorragenden neuen Funktionen in ASP.NET 4 mit um einen Onlineshop, einschließlich Warenkorb, Auschecken und Verwaltung zu erstellen.
> 
> Diese Reihe von Lernprogrammen sind alle Schritte ausgeführt, um die beispielanwendung Tailspin Spyworks erstellen. Teil 6 fügt ASP.NET-Mitgliedschaft hinzu.


## <a id="_Toc260221672"></a>  Arbeiten mit ASP.NET Membership

![](tailspin-spyworks-part-6/_static/image1.png)

Klicken Sie auf die Sicherheit

![](tailspin-spyworks-part-6/_static/image1.jpg)

Stellen Sie sicher, dass wir die Formularauthentifizierung verwenden.

![](tailspin-spyworks-part-6/_static/image2.jpg)

Verwenden Sie den Link "Create User", um eine Reihe von Benutzern zu erstellen.

![](tailspin-spyworks-part-6/_static/image3.jpg)

Wenn fertig sind, finden Sie in Projektmappen-Explorer, und aktualisieren Sie die Ansicht.

![](tailspin-spyworks-part-6/_static/image2.png)

Beachten Sie, dass die ASPNETDB. Feine MDF wurde erstellt. Diese Datei enthält die Tabellen, um die Hauptdienste in ASP.NET wie Mitgliedschaft zu unterstützen.

Jetzt können wir die Implementierung des Auscheckvorgangs beginnen.

Beginnen Sie mit dem Erstellen einer CheckOut.aspx-Seite.

Die Seite "CheckOut.aspx" darf nur für Benutzer verfügbar sein, angemeldet sind, damit wir den Zugriff auf einschränken angemeldeten Benutzer und umleiten, die nicht auf der Anmeldeseite angemeldet sind.

Zu diesem Zweck fügen Sie den folgenden für den Konfigurationsabschnitt der Datei "Web.config" hinzu.

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample1.xml)]

Die Vorlage für ASP.NET Web Forms-Anwendungen automatisch einen Abschnitt für die Authentifizierung in unsere Datei "Web.config" hinzugefügt und die Standardanmeldeseite hergestellt.

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample2.xml)]

Wir müssen ändern "Login.aspx" CodeBehind-Datei, um eine anonyme Einkaufswagen migriert werden, wenn der Benutzer anmeldet. Die Seite "ändern"\_Load-Ereignis wie folgt.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample3.cs)]

Fügen Sie dann einen "LoggedIn"-Ereignishandler wie folgt den Sitzungsnamen auf neu angemeldeten Benutzers festlegen und ändern die temporäre Sitzungs-Id in den Einkaufswagen Sinn macht, des Benutzers durch Aufrufen der Methode MigrateCart in unsere MyShoppingCart-Klasse. (Implementiert in der CS-Datei)

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample4.cs)]

Implementieren Sie die Methode MigrateCart() wie folgt.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample5.cs)]

In checkout.aspx verwenden EntityDataSource und eine GridView in unserer Auschecken Seite wir fast so wie wir in unserer Warenkorb-Seite hat.

[!code-aspx[Main](tailspin-spyworks-part-6/samples/sample6.aspx)]

Beachten Sie, dass unsere GridView-Steuerelement einen "den Ondatabound"-Ereignishandler mit dem Namen MyList gibt\_RowDataBound implementieren wir also diesem Ereignishandler wie folgt.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample7.cs)]

Diese Methode hält eine laufende Summe der Warenkorb Einkaufswagen wie jede Zeile gebunden ist und die letzte Zeile der GridView aktualisiert.

Zu diesem Zeitpunkt haben wir eine Präsentation "Überprüfen", platziert werden der Reihenfolge implementiert.

Behandeln wir eine leere einkaufswagenszenario durch Hinzufügen von wenigen Codezeilen auf unserer Seite\_Load-Ereignis:

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample8.cs)]

Klickt der Benutzer auf die Schaltfläche "Absenden" führt wir im übermitteln Schaltfläche klicken-Ereignishandler den folgenden Code.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample9.cs)]

Der "Hauptbestandteil" des Bestellvorgangs Übermittlung wird in der Methode SubmitOrder() unsere MyShoppingCart-Klasse implementiert werden.

SubmitOrder unterstützt wird:

- Nehmen Sie alle Einzelposten in den Warenkorb, und verwenden Sie, um einen neuen Datensatz für die Reihenfolge und die zugehörigen OrderDetails Datensätze zu erstellen.
- Versanddatum zu berechnen.
- Deaktivieren Sie den Einkaufswagen Sinn macht.


[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample10.cs)]

Im Rahmen dieser beispielanwendung müssen wir ein Lieferdatum berechnen, indem einfach das aktuelle Datum zwei Tage hinzugefügt.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample11.cs)]

Die Anwendung jetzt ausführen lässt, den Einkaufswagen Prozess von Anfang bis Ende zu testen.

> [!div class="step-by-step"]
> [Zurück](tailspin-spyworks-part-5.md)
> [Weiter](tailspin-spyworks-part-7.md)
