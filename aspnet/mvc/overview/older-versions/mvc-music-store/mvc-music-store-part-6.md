---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
title: 'Teil 6: Mithilfe von Datenanmerkungen zur Modellvalidierung | Microsoft Docs'
author: jongalloway
description: Diese Reihe von Lernprogrammen details aller die Schritte zum Erstellen von ASP.NET MVC-Music Store-beispielanwendung. Teil 6 deckt mithilfe von Datenanmerkungen für Modell V...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: b3193d33-2d0b-4d98-9712-58bd897c62ec
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
msc.type: authoredcontent
ms.openlocfilehash: 328eccb4324bb10a7e8dec819a70129fc14c42c4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="part-6-using-data-annotations-for-model-validation"></a><span data-ttu-id="2f3ac-104">Teil 6: Mithilfe von Datenanmerkungen bei der Modellüberprüfung</span><span class="sxs-lookup"><span data-stu-id="2f3ac-104">Part 6: Using Data Annotations for Model Validation</span></span>
====================
<span data-ttu-id="2f3ac-105">durch [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="2f3ac-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="2f3ac-106">Das MVC-Music Store ist eine lernprogrammanwendung, die führt und erklärt schrittweise, wie mithilfe von ASP.NET MVC und Visual Studio für die Webentwicklung.</span><span class="sxs-lookup"><span data-stu-id="2f3ac-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="2f3ac-107">Das MVC-Music Store handelt es sich um einfache Beispiel Store Implementierung, die online Musikalben verkauft und implementiert grundlegende standortverwaltung Benutzer anmelden und shopping Cart-Funktionalität.</span><span class="sxs-lookup"><span data-stu-id="2f3ac-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="2f3ac-108">Diese Reihe von Lernprogrammen details aller die Schritte zum Erstellen von ASP.NET MVC-Music Store-beispielanwendung.</span><span class="sxs-lookup"><span data-stu-id="2f3ac-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="2f3ac-109">Teil 6 werden die mithilfe von Datenanmerkungen zur Modellvalidierung behandelt.</span><span class="sxs-lookup"><span data-stu-id="2f3ac-109">Part 6 covers Using Data Annotations for Model Validation.</span></span>


<span data-ttu-id="2f3ac-110">Wir haben ein ernstes Problem mit unserem erstellen und Bearbeiten von Formularen: machen sie keine Validierung.</span><span class="sxs-lookup"><span data-stu-id="2f3ac-110">We have a major issue with our Create and Edit forms: they're not doing any validation.</span></span> <span data-ttu-id="2f3ac-111">Staus z. B. für Pflichtfelder leer oder Typ Buchstaben im Feld "Preis" lassen und der erste Fehler, die, den wir sehen, aus der Datenbank ist.</span><span class="sxs-lookup"><span data-stu-id="2f3ac-111">We can do things like leave required fields blank or type letters in the Price field, and the first error we'll see is from the database.</span></span>

<span data-ttu-id="2f3ac-112">Wir können unsere Anwendung einfach durch Hinzufügen von Datenanmerkungen zu unserem Modellklassen Validierung hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="2f3ac-112">We can easily add validation to our application by adding Data Annotations to our model classes.</span></span> <span data-ttu-id="2f3ac-113">Datenanmerkungen ermöglichen es uns zur Beschreibung der Regeln, die wir möchten unsere Modelleigenschaften angewendet, und ASP.NET MVC erzwungen werden und Anzeige von entsprechenden Meldungen unseren Benutzern übernimmt.</span><span class="sxs-lookup"><span data-stu-id="2f3ac-113">Data Annotations allow us to describe the rules we want applied to our model properties, and ASP.NET MVC will take care of enforcing them and displaying appropriate messages to our users.</span></span>

## <a name="adding-validation-to-our-album-forms"></a><span data-ttu-id="2f3ac-114">Unsere Album Formularen Validierungen hinzugefügt</span><span class="sxs-lookup"><span data-stu-id="2f3ac-114">Adding Validation to our Album Forms</span></span>

<span data-ttu-id="2f3ac-115">Wir verwenden die folgenden Datenanmerkung Attribute:</span><span class="sxs-lookup"><span data-stu-id="2f3ac-115">We'll use the following Data Annotation attributes:</span></span>

- <span data-ttu-id="2f3ac-116">**Erforderliche** – gibt an, dass die Eigenschaft ein Pflichtfeld ist.</span><span class="sxs-lookup"><span data-stu-id="2f3ac-116">**Required** – Indicates that the property is a required field</span></span>
- <span data-ttu-id="2f3ac-117">**DisplayName** – definiert den Text, der es verwendet werden auf Formularfelder und validierungsmeldungen soll</span><span class="sxs-lookup"><span data-stu-id="2f3ac-117">**DisplayName** – Defines the text we want used on form fields and validation messages</span></span>
- <span data-ttu-id="2f3ac-118">**StringLength** – definiert die maximale Länge für ein Zeichenfolgenfeld eingegeben</span><span class="sxs-lookup"><span data-stu-id="2f3ac-118">**StringLength** – Defines a maximum length for a string field</span></span>
- <span data-ttu-id="2f3ac-119">**Bereich** – weist den maximalen und minimalen Wert für ein numerisches Feld</span><span class="sxs-lookup"><span data-stu-id="2f3ac-119">**Range** – Gives a maximum and minimum value for a numeric field</span></span>
- <span data-ttu-id="2f3ac-120">**Binden Sie** – Listet die Felder zum Ausschließen oder einschließen beim Binden der Parameter oder Form Werte an Modelleigenschaften</span><span class="sxs-lookup"><span data-stu-id="2f3ac-120">**Bind** – Lists fields to exclude or include when binding parameter or form values to model properties</span></span>
- <span data-ttu-id="2f3ac-121">**ScaffoldColumn** – ermöglicht das Ausblenden von Feldern aus Editor Formularen</span><span class="sxs-lookup"><span data-stu-id="2f3ac-121">**ScaffoldColumn** – Allows hiding fields from editor forms</span></span>

<span data-ttu-id="2f3ac-122">*Hinweis: Weitere Informationen zum Verwenden von Attributen-Datenanmerkung Modellvalidierung, finden Sie auf die MSDN-Dokumentation*[`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)</span><span class="sxs-lookup"><span data-stu-id="2f3ac-122">*Note: For more information on Model Validation using Data Annotation attributes, see the MSDN documentation at*[`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)</span></span>

<span data-ttu-id="2f3ac-123">Öffnen Sie die Album-Klasse, und fügen Sie die folgenden *mit* -Anweisungen an den Anfang.</span><span class="sxs-lookup"><span data-stu-id="2f3ac-123">Open the Album class and add the following *using* statements to the top.</span></span>

[!code-csharp[Main](mvc-music-store-part-6/samples/sample1.cs)]

<span data-ttu-id="2f3ac-124">Als Nächstes aktualisieren Sie die Eigenschaften, um die Anzeige und Validierung Attribute hinzufügen, wie unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="2f3ac-124">Next, update the properties to add display and validation attributes as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-6/samples/sample2.cs)]

<span data-ttu-id="2f3ac-125">Während wir vorhanden sind, haben wir die "Genre" und Interpreten virtuelle Eigenschaften geändert.</span><span class="sxs-lookup"><span data-stu-id="2f3ac-125">While we're there, we've also changed the Genre and Artist to virtual properties.</span></span> <span data-ttu-id="2f3ac-126">Dies kann Entity Framework lazy sie nach Bedarf laden.</span><span class="sxs-lookup"><span data-stu-id="2f3ac-126">This allows Entity Framework to lazy-load them as necessary.</span></span>

[!code-csharp[Main](mvc-music-store-part-6/samples/sample3.cs)]

<span data-ttu-id="2f3ac-127">Nach müssen, werden diese Attribute unsere Album-Modell hinzugefügt, unsere erstellen und bearbeiten-Bildschirm beginnt sofort mit dem Überprüfen von Feldern, und wir haben mit dem Anzeigenamen (z. B. Album Art Url statt AlbumArtUrl) ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="2f3ac-127">After having added these attributes to our Album model, our Create and Edit screen immediately begin validating fields and using the Display Names we've chosen (e.g. Album Art Url instead of AlbumArtUrl).</span></span> <span data-ttu-id="2f3ac-128">Führen Sie die Anwendung, und navigieren Sie zu /StoreManager/Create.</span><span class="sxs-lookup"><span data-stu-id="2f3ac-128">Run the application and browse to /StoreManager/Create.</span></span>

![](mvc-music-store-part-6/_static/image1.png)

<span data-ttu-id="2f3ac-129">Als Nächstes müssen wir einige Überprüfungsregeln unterbrochen.</span><span class="sxs-lookup"><span data-stu-id="2f3ac-129">Next, we'll break some validation rules.</span></span> <span data-ttu-id="2f3ac-130">Geben Sie einen Preis von 0, und lassen Sie den Titel leer.</span><span class="sxs-lookup"><span data-stu-id="2f3ac-130">Enter a price of 0 and leave the Title blank.</span></span> <span data-ttu-id="2f3ac-131">Wenn wir auf die Schaltfläche "erstellen" klicken, sehen wir das Formular angezeigt, wobei Überprüfungsfehlermeldungen anzeigt, welche Felder der Überprüfungsregeln nicht entsprochen haben, dass wir definiert haben.</span><span class="sxs-lookup"><span data-stu-id="2f3ac-131">When we click on the Create button, we will see the form displayed with validation error messages showing which fields did not meet the validation rules we have defined.</span></span>

![](mvc-music-store-part-6/_static/image2.png)

## <a name="testing-the-client-side-validation"></a><span data-ttu-id="2f3ac-132">Testen die clientseitige Validierung</span><span class="sxs-lookup"><span data-stu-id="2f3ac-132">Testing the Client-Side Validation</span></span>

<span data-ttu-id="2f3ac-133">Serverseitige Validierung ist sehr wichtig, aus der Perspektive einer Anwendung, da Benutzer die clientseitige Überprüfung umgehen können.</span><span class="sxs-lookup"><span data-stu-id="2f3ac-133">Server-side validation is very important from an application perspective, because users can circumvent client-side validation.</span></span> <span data-ttu-id="2f3ac-134">Allerdings weisen Formulare, die nur die serverseitige Validierung implementiert, drei wesentlichen Probleme auf.</span><span class="sxs-lookup"><span data-stu-id="2f3ac-134">However, webpage forms which only implement server-side validation exhibit three significant problems.</span></span>

1. <span data-ttu-id="2f3ac-135">Der Benutzer muss warten, für das Formular gebucht, auf dem Server überprüft werden, und für die Antwort an den Browser gesendet wird.</span><span class="sxs-lookup"><span data-stu-id="2f3ac-135">The user has to wait for the form to be posted, validated on the server, and for the response to be sent to their browser.</span></span>
2. <span data-ttu-id="2f3ac-136">Der Benutzer erhalten nicht unmittelbares Feedback, wenn sie ein Feld korrigieren, sodass sie jetzt den setupüberprüfungsregeln entspricht.</span><span class="sxs-lookup"><span data-stu-id="2f3ac-136">The user doesn't get immediate feedback when they correct a field so that it now passes the validation rules.</span></span>
3. <span data-ttu-id="2f3ac-137">Wir sind Serverressourcen zum Ausführen der Validierungslogik, statt den Browser des Benutzers nutzt verschwenden.</span><span class="sxs-lookup"><span data-stu-id="2f3ac-137">We are wasting server resources to perform validation logic instead of leveraging the user's browser.</span></span>

<span data-ttu-id="2f3ac-138">Glücklicherweise müssen die ASP.NET MVC 3-Vorlagen Gerüst clientseitige Validierung integriert, erfordern keine zusätzliche Arbeit übernehmen.</span><span class="sxs-lookup"><span data-stu-id="2f3ac-138">Fortunately, the ASP.NET MVC 3 scaffold templates have client-side validation built in, requiring no additional work whatsoever.</span></span>

<span data-ttu-id="2f3ac-139">Eingabe aus einen einzelnen Buchstaben in das Feld "Titel" erfüllt die überprüfungsanforderungen zu erfüllen, weshalb der validierungsmeldung sofort entfernt wird.</span><span class="sxs-lookup"><span data-stu-id="2f3ac-139">Typing a single letter in the Title field satisfies the validation requirements, so the validation message is immediately removed.</span></span>

![](mvc-music-store-part-6/_static/image3.png)


> [!div class="step-by-step"]
> <span data-ttu-id="2f3ac-140">[Zurück](mvc-music-store-part-5.md)
> [Weiter](mvc-music-store-part-7.md)</span><span class="sxs-lookup"><span data-stu-id="2f3ac-140">[Previous](mvc-music-store-part-5.md)
[Next](mvc-music-store-part-7.md)</span></span>
