---
uid: mvc/overview/getting-started/database-first-development/publish-to-azure
title: Veröffentlichen von MVC Database First-Website in Azure | Microsoft-Dokumentation
author: Rick-Anderson
description: Verwenden MVC, Entity Framework und ASP.NET-Gerüstbau, können Sie eine Webanwendung erstellen, die eine Schnittstelle für eine vorhandene Datenbank bereitstellt. Dieses Tutorial Seri...
ms.author: riande
ms.date: 12/22/2014
ms.assetid: 7131f1c1-cef3-4396-ab44-ed4519676546
msc.legacyurl: /mvc/overview/getting-started/database-first-development/publish-to-azure
msc.type: authoredcontent
ms.openlocfilehash: 1d2c26c211c5c8d97076327d01fe59d5ba4dc9ac
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021637"
---
<a name="publish-mvc-database-first-site-to-azure"></a>Veröffentlichen Sie MVC Database First-Website in Azure.
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> Verwenden MVC, Entity Framework und ASP.NET-Gerüstbau, können Sie eine Webanwendung erstellen, die eine Schnittstelle für eine vorhandene Datenbank bereitstellt. Dieser tutorialreihe erfahren Sie, wie Sie automatisch generierter Code, der ermöglicht Benutzern das anzeigen, bearbeiten, erstellen und Löschen von Daten, die in einer Datenbanktabelle gespeichert. Der generierte Code entspricht die Spalten in der Datenbanktabelle.
> 
> Dieser Teil der Serie konzentriert sich auf die WebApp und Datenbank in Azure veröffentlichen. Sie können in diesem Thema erfahren Sie mehr über das Veröffentlichen einer WebApp und Datenbank, sondern tatsächlich der Schritte, die Sie am Anfang des Tutorials starten müssen, lesen. Finden Sie unter [Einstieg](setting-up-database.md).


## <a name="deploy-your-web-app-on-azure"></a>Bereitstellen Sie Ihrer Web-app in Azure

Sie benötigen ein Azure-Konto zum Durchführen dieses Tutorials:

- Sie können [öffnen Sie ein Azure-Konto kostenlos](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) – Sie erhalten ein Guthaben zum Ausprobieren Zahlungspflichtiger Azure-Dienste nutzen können, und auch nach dem sie verwendet werden, bis können Sie das Konto behalten und kostenlose Azure-Dienste.
- Sie können [MSDN-abonnentenvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) -Ihr MSDN-Abonnement ein monatliches Guthaben, die Sie für zahlungspflichtige Azure-Dienste verwenden können.

Klicken Sie zum Veröffentlichen Ihrer Web-app mit der rechten Maustaste in des Projekts, und wählen Sie **veröffentlichen**.

![Website veröffentlichen](publish-to-azure/_static/image1.png)

Wählen Sie die Microsoft Azure-Websites.

![Auswählen von Azure](publish-to-azure/_static/image2.png)

Wenn Sie nicht bei Azure angemeldet sind, geben Sie Ihre Azure-Konto-Anmeldeinformationen. Wählen Sie dann neu, um eine neue Web-app zu erstellen.

![neue Website](publish-to-azure/_static/image3.png)

Erstellen Sie einen eindeutigen Namen für Ihre Web-app. Lernen Sie, dass der Name eindeutig ist. Wenn Sie rechts neben dem Namensfeld ein grünes Häkchen angezeigt. Wählen Sie eine Region für Ihre Web-app. Wählen Sie **neuen Server erstellen** für die Datenbank, und geben Sie einen Benutzernamen und ein Kennwort für diesen neuen Datenbankserver. Klicken Sie abschließend auf **erstellen**.

![Website erstellen](publish-to-azure/_static/image4.png)

Die Werte für Ihre Verbindung sind nun festgelegt. Sie können diese Werte unverändert lassen.

![connection](publish-to-azure/_static/image5.png)

Klicken Sie auf **Weiter**.

Beachten Sie, dass zwei Datenbank-Verbindungen für die Einstellungen werden angegeben - ApplicationDBContext und ContosoUniversityDataEntities. ApplicationDBContext ist die Verbindung für Benutzer-Tabellen. Diese Werte zeigen nur die Verbindungszeichenfolgen für die Datenbanken an. Es bedeutet nicht, dass diese Datenbanken veröffentlicht werden, wenn Sie Ihre Website zu veröffentlichen. Veröffentlichen Sie Ihr Datenbankprojekt nach dem Veröffentlichen der Web-app.

![](publish-to-azure/_static/image6.png)

Die Auslassungspunkte (...) neben der Verbindung mit der zeigt Sie die Details der Verbindungszeichenfolge. Klicken Sie auf die Auslassungspunkte neben ContosoUniversityDataEntities.

![](publish-to-azure/_static/image7.png)

Notieren Sie den Namen des Datenbankservers und der Datenbank. Den Namen des Servers wird nach dem Zufallsprinzip generiert. Der Name der Datenbank ist einfach der Name Ihrer Website mit  **\_Db** angefügt. Sie benötigen beide Namen später, wenn Sie Ihre Datenbank veröffentlichen.

Klicken Sie auf **OK** um die Datenbank-Verbindung Zeichenfolge-Fenster zu schließen.

Klicken Sie in das Fenster "Web veröffentlichen" auf **Weiter** um die Vorschau anzuzeigen.

![](publish-to-azure/_static/image8.png)

Sie können die Vorschau starten, um eine Liste der Dateien zum Veröffentlichen finden Sie unter klicken. Da dies beim ersten Sie auf dieser Website veröffentlicht haben ist, ist die Liste alle relevanten Datei im Projekt.

Klicken Sie auf **Veröffentlichen**.

Der Ausgabebereich zeigt das Ergebnis der Veröffentlichung.

![](publish-to-azure/_static/image9.png)

Nach der Veröffentlichung ist die Website Immedialely in einem Webbrowser gestartet. Ihre Website bereitgestellt wurde, und Sie können einen neuen Benutzer auf die Website registrieren; Allerdings haben die Tabellen im Projekt ContosoUniversityData noch nicht veröffentlicht wurde. Wenn Sie auf die Liste der Schüler/Studenten-Link klicken, erhalten Sie einen Fehler.

## <a name="publish-database-to-sql-azure"></a>Veröffentlichen Sie die Datenbank zu SQL Azure

Vor der Veröffentlichung der Datenbank, müssen Sie sicherstellen, dass es sich bei Ihrem lokale Computer eine Verbindung mit dem Datenbankserver herstellen kann. Die Firewall für Ihren Datenbankserver schränkt die Computer mit der Datenbank herstellen können. Sie müssen die IP-Adresse des Computers den zulässigen IP-Adressen für die Firewall hinzufügen.

Melden Sie sich beim Azure-Konto über das Azure-Portal.

Wählen Sie Ihre neue Datenbank und **verwalten**.

![Verwalten von](publish-to-azure/_static/image10.png)

Sie müssen Ihren Datenbankserver, um Verbindungen von Ihrem Computer zu ermöglichen, konfigurieren. Wenn Sie auf "verwalten" auswählen, werden Sie aufgefordert, um die aktuelle IP-Adresse hinzuzufügen, wie mit dem Datenbankserver her. Wählen Sie Ja.

![IP-Adresse hinzufügen](publish-to-azure/_static/image11.png)

Es besteht die Möglichkeit, die die IP-Adresse, die Sie im vorherigen Schritt hinzugefügt haben nicht die einzige IP-Adresse ist, die Sie für Verbindungen zu konfigurieren müssen. Sie können versuchen, an die Datenbank aus, um festzustellen, ob die Verbindungen ordnungsgemäß eingerichtet wurden. Geben Sie dem Benutzernamen und Kennwort, die Sie zuvor erstellt haben.

![Anmeldung](publish-to-azure/_static/image12.png)

Wenn Sie eine Fehlermeldung erhalten, müssen Sie eine andere IP-Adresse hinzuzufügen. Klicken Sie auf die Fehlermeldung, um weitere Details zum Fehler anzuzeigen. In den Details sehen Sie die IP-Adresse, die Sie hinzufügen müssen. Beachten Sie diese IP-Adresse ein.

![nicht zulässig.](publish-to-azure/_static/image13.png)

Schließen Sie dieses Fenster für die Anmeldung, und kehren Sie zum Azure-Portal zurück.

Navigieren Sie zum Dashboard für Ihre Datenbank. Klicken Sie auf **zulässige IP-Adressen verwalten**.

![IP-Adressen verwalten](publish-to-azure/_static/image14.png)

Sie müssen nun die IP-Adresse aus der Fehlermeldung hinzufügen. Ändern Sie den Bereich der zulässigen IP-Adressen, die aus der Fehlermeldung enthält, oder fügen Sie die IP-Adresse als einen separaten Eintrag hinzu.

![neue Adresse hinzufügen](publish-to-azure/_static/image15.png)

Speichern Sie die Änderung in zulässige IP-Adressen ein.

Klicken Sie auf verwalten, und versuchen Sie erneut mit der Datenbank anzumelden. Möglicherweise müssen Sie warten einige Minuten, bevor die zulässigen IP-Adressen für die Firewall ordnungsgemäß konfiguriert sind. Wenn Sie in der Datenbank anmelden können, müssen Sie die Verbindung mit der Datenbank eingerichtet.

Sie können dieses Fenster Management geöffnet lassen, da Sie das Ergebnis der Bereitstellung der Datenbank in Kürze überprüft werden.

Geben Sie dem Datenbankprojekt zurück. Mit der rechten Maustaste in des Projekts, und wählen Sie **veröffentlichen**.

![Veröffentlichen der Datenbank](publish-to-azure/_static/image16.png)

Wählen Sie im Fenster Datenbank veröffentlichen **bearbeiten**.

![bearbeiten](publish-to-azure/_static/image17.png)

Geben Sie den Namen des Datenbankservers und die Anmeldeinformationen für die Authentifizierung für den Server ein. Wählen Sie die Datenbank, die Sie aus der Liste der verfügbaren Datenbanken erstellt haben, nach der Eingabe der Anmeldeinformationen. Standardmäßig legt Visual Studio den Namen der Datenbank-Felds, auf den Namen des Projekts, die nicht identisch mit der Datenbank möglicherweise, die Sie erstellt haben.

![](publish-to-azure/_static/image18.png)

Klicken Sie auf OK.

Wahrscheinlich möchten dieses Profil zu speichern, damit Sie Updates in der Zukunft veröffentlichen können, ohne Sie alle Verbindungsinformationen erneut eingeben. Klicken Sie auf **Profil erstellen**.

![Profil speichern](publish-to-azure/_static/image19.png)

Es erstellt eine Datei in Ihrem Projekt mit dem Namen **ContosoUniversityData.publish.xml**. Laden Sie das nächste Mal mit das Sie die Datenbank in Azure veröffentlichen möchten einfach dieses Profil.

Klicken Sie nun **veröffentlichen** zum Erstellen der Datenbank in Azure.

![publish](publish-to-azure/_static/image20.png)

Nach der Ausführung eine Weile werden die Veröffentlichen von Ergebnissen angezeigt.

![results](publish-to-azure/_static/image21.png)

Jetzt können Sie in das Verwaltungsportal für Ihre Datenbank zurückkehren. Aktualisieren Sie die Entwurfsansicht zu sehen, und beachten Sie, dass die 3 Tabellen mit vorab ausgefüllten Daten bereitgestellt wurden.

![neue Tabellen](publish-to-azure/_static/image22.png)

Jetzt können Sie sich zum Testen der Web-app, die in Azure bereitgestellt wird. Navigieren Sie zu der Web-app in Azure (z. B. http://contosositeexample.azurewebsites.net/). Klicken Sie auf den Link, um die Liste der Schüler/Studenten und die Indexansicht für Schüler/Studenten wird angezeigt.

![Sicht](publish-to-azure/_static/image23.png)

In einigen Fällen benötigen in die Datenbank und die Verbindung etwas Zeit, um ordnungsgemäß konfiguriert werden. Wenn Sie eine Fehlermeldung erhalten, warten Sie einige Minuten, und versuchen Sie es dann erneut.

## <a name="conclusion"></a>Schlussbemerkung

Diese Reihe bereitgestellt, ein einfaches Beispiel dafür, wie zum Generieren von Code aus einer vorhandenen Datenbank, in dem Benutzer bearbeiten, aktualisieren, erstellen und Löschen von Daten. Er verwendet ASP.NET MVC 5, Entity Framework und ASP.NET Scaffolding zum Erstellen des Projekts.

Ein einführendes Beispiel Code First-Entwicklung, finden Sie unter [erste Schritte mit ASP.NET MVC 5](../introduction/getting-started.md).

Ein komplexeres Beispiel, finden Sie unter [erstellen ein Entity Framework-Datenmodells für eine ASP.NET MVC 4-App](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Beachten Sie, dass die DbContext-API, mit denen Sie für die Arbeit mit Daten in der ersten Datenbank identisch mit der API Sie verwenden für die Arbeit mit Daten in Code First. Auch wenn Sie Database First verwenden möchten, erhalten Sie wie Sie die Verarbeitung komplexerer Szenarien wie z. B. das Lesen und aktualisieren die zugehörige Daten, Behandlung von nebenläufigkeitskonflikten führen, und so weiter aus einem Code First-Lernprogramm. Der einzige Unterschied besteht im wie die Datenbank, die Context-Klasse und die Entitätsklassen erstellt werden.

> [!div class="step-by-step"]
> [Vorherige](enhancing-data-validation.md)
