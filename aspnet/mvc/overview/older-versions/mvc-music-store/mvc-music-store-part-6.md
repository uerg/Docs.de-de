---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
title: 'Teil 6: Verwenden von Datenanmerkungen zur Modellvalidierung | Microsoft-Dokumentation'
author: jongalloway
description: Dieser tutorialreihe werden alle Schritte ausgeführt, um die ASP.NET MVC Music Store-beispielanwendung zu erstellen. Teil 6 wird die Verwendung von Datenanmerkungen für Modell V behandelt...
ms.author: aspnetcontent
ms.date: 04/21/2011
ms.assetid: b3193d33-2d0b-4d98-9712-58bd897c62ec
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
msc.type: authoredcontent
ms.openlocfilehash: 10884f569f0f5d95517b73daab31fbd269a4726a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37825952"
---
<a name="part-6-using-data-annotations-for-model-validation"></a><span data-ttu-id="4a6d4-104">Teil 6: Verwenden von Datenanmerkungen bei der Modellüberprüfung</span><span class="sxs-lookup"><span data-stu-id="4a6d4-104">Part 6: Using Data Annotations for Model Validation</span></span>
====================
<span data-ttu-id="4a6d4-105">durch [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="4a6d4-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="4a6d4-106">Die MVC Music Store ist ein lernprogrammanwendung, die eingeführt und erläutert Schritt für Schritt, wie ASP.NET MVC und Visual Studio für die Webentwicklung verwenden.</span><span class="sxs-lookup"><span data-stu-id="4a6d4-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="4a6d4-107">Die MVC Music Store ist eine Implementierung eines einfachen Beispiels die Alben online verkauft und implementiert grundlegende Verwaltung, Benutzeranmeldung und shopping Cart-Funktionalität.</span><span class="sxs-lookup"><span data-stu-id="4a6d4-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="4a6d4-108">Dieser tutorialreihe werden alle Schritte ausgeführt, um die ASP.NET MVC Music Store-beispielanwendung zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="4a6d4-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="4a6d4-109">Teil 6 wird die Verwendung von Datenanmerkungen zur Modellvalidierung behandelt.</span><span class="sxs-lookup"><span data-stu-id="4a6d4-109">Part 6 covers Using Data Annotations for Model Validation.</span></span>


<span data-ttu-id="4a6d4-110">Wir haben ein schwerwiegendes Problem mit unserem erstellen und Bearbeiten von Formularen: sie noch keine Überprüfung nicht möglich.</span><span class="sxs-lookup"><span data-stu-id="4a6d4-110">We have a major issue with our Create and Edit forms: they're not doing any validation.</span></span> <span data-ttu-id="4a6d4-111">Können wir z. B. für Pflichtfelder leer "und" Typ Buchstaben in das Feld für den Preis zu lassen, und der erste Fehler, die, den wir sehen werden, aus der Datenbank ist.</span><span class="sxs-lookup"><span data-stu-id="4a6d4-111">We can do things like leave required fields blank or type letters in the Price field, and the first error we'll see is from the database.</span></span>

<span data-ttu-id="4a6d4-112">Wir können unsere Anwendung einfach durch Hinzufügen von Datenanmerkungen auf unserem Modellklassen Validierung hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="4a6d4-112">We can easily add validation to our application by adding Data Annotations to our model classes.</span></span> <span data-ttu-id="4a6d4-113">Datenanmerkungen können wir beschreiben die gewünschten auf unsere Modelleigenschaften angewendet, und ASP.NET MVC übernimmt erzwungen werden und die entsprechende Nachrichten für unsere Benutzer anzeigen.</span><span class="sxs-lookup"><span data-stu-id="4a6d4-113">Data Annotations allow us to describe the rules we want applied to our model properties, and ASP.NET MVC will take care of enforcing them and displaying appropriate messages to our users.</span></span>

## <a name="adding-validation-to-our-album-forms"></a><span data-ttu-id="4a6d4-114">Hinzufügen einer Validierung zu unseren Album-Formularen</span><span class="sxs-lookup"><span data-stu-id="4a6d4-114">Adding Validation to our Album Forms</span></span>

<span data-ttu-id="4a6d4-115">Wir verwenden die folgenden Attribute für die Datenanmerkung:</span><span class="sxs-lookup"><span data-stu-id="4a6d4-115">We'll use the following Data Annotation attributes:</span></span>

- <span data-ttu-id="4a6d4-116">**Erforderliche** – gibt an, dass die Eigenschaft ein erforderliches Feld.</span><span class="sxs-lookup"><span data-stu-id="4a6d4-116">**Required** – Indicates that the property is a required field</span></span>
- <span data-ttu-id="4a6d4-117">**"DisplayName"** – definiert den Text, der es verwendet werden auf Formularfelder und validierungsmeldungen soll</span><span class="sxs-lookup"><span data-stu-id="4a6d4-117">**DisplayName** – Defines the text we want used on form fields and validation messages</span></span>
- <span data-ttu-id="4a6d4-118">**StringLength** – definiert die maximale Länge für ein Zeichenfolgenfeld eingegeben</span><span class="sxs-lookup"><span data-stu-id="4a6d4-118">**StringLength** – Defines a maximum length for a string field</span></span>
- <span data-ttu-id="4a6d4-119">**Bereich** – weist den maximalen und minimalen Wert für ein numerisches Feld</span><span class="sxs-lookup"><span data-stu-id="4a6d4-119">**Range** – Gives a maximum and minimum value for a numeric field</span></span>
- <span data-ttu-id="4a6d4-120">**Binden Sie** – Listet die Felder zum Ausschließen oder einschließen, wenn die Werte für Parameter oder Formular eine Bindung an Eigenschaften des Modells</span><span class="sxs-lookup"><span data-stu-id="4a6d4-120">**Bind** – Lists fields to exclude or include when binding parameter or form values to model properties</span></span>
- <span data-ttu-id="4a6d4-121">**ScaffoldColumn** – ermöglicht das Ausblenden von Feldern von Editor-Formularen</span><span class="sxs-lookup"><span data-stu-id="4a6d4-121">**ScaffoldColumn** – Allows hiding fields from editor forms</span></span>

<span data-ttu-id="4a6d4-122">*Hinweis: Weitere Informationen zu Modellvalidierung Datenanmerkung Attribute verwenden, finden Sie auf die MSDN-Dokumentation*[`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)</span><span class="sxs-lookup"><span data-stu-id="4a6d4-122">*Note: For more information on Model Validation using Data Annotation attributes, see the MSDN documentation at*[`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)</span></span>

<span data-ttu-id="4a6d4-123">Öffnen Sie die Album-Klasse, und fügen Sie die folgenden *mit* -Anweisungen an den Anfang.</span><span class="sxs-lookup"><span data-stu-id="4a6d4-123">Open the Album class and add the following *using* statements to the top.</span></span>

[!code-csharp[Main](mvc-music-store-part-6/samples/sample1.cs)]

<span data-ttu-id="4a6d4-124">Als Nächstes aktualisieren Sie die Eigenschaften, um die Anzeige und Validierung-Attribute hinzufügen, wie unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="4a6d4-124">Next, update the properties to add display and validation attributes as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-6/samples/sample2.cs)]

<span data-ttu-id="4a6d4-125">Während wir damit beschäftigt sind, haben wir auch das Genre und Interpreteninformationen virtuelle Eigenschaften geändert.</span><span class="sxs-lookup"><span data-stu-id="4a6d4-125">While we're there, we've also changed the Genre and Artist to virtual properties.</span></span> <span data-ttu-id="4a6d4-126">Dies kann Entity Framework lazy sie bei Bedarf laden.</span><span class="sxs-lookup"><span data-stu-id="4a6d4-126">This allows Entity Framework to lazy-load them as necessary.</span></span>

[!code-csharp[Main](mvc-music-store-part-6/samples/sample3.cs)]

<span data-ttu-id="4a6d4-127">Klicken Sie nach dem müssen, werden diese Attribute unseres Modells Album hinzugefügt, unsere erstellen und bearbeiten-Bildschirm beginnt sofort mit dem Überprüfen von Feldern, und verwenden die Anzeigenamen haben wir uns entschieden, (z. B. Album Art Url anstelle von AlbumArtUrl).</span><span class="sxs-lookup"><span data-stu-id="4a6d4-127">After having added these attributes to our Album model, our Create and Edit screen immediately begin validating fields and using the Display Names we've chosen (e.g. Album Art Url instead of AlbumArtUrl).</span></span> <span data-ttu-id="4a6d4-128">Führen Sie die Anwendung, und navigieren Sie zu /StoreManager/Create.</span><span class="sxs-lookup"><span data-stu-id="4a6d4-128">Run the application and browse to /StoreManager/Create.</span></span>

![](mvc-music-store-part-6/_static/image1.png)

<span data-ttu-id="4a6d4-129">Als Nächstes müssen wir einige Validierungsregeln unterbrochen.</span><span class="sxs-lookup"><span data-stu-id="4a6d4-129">Next, we'll break some validation rules.</span></span> <span data-ttu-id="4a6d4-130">Geben Sie einen Preis von 0, und lassen Sie den Titel leer.</span><span class="sxs-lookup"><span data-stu-id="4a6d4-130">Enter a price of 0 and leave the Title blank.</span></span> <span data-ttu-id="4a6d4-131">Wenn wir auf die Schaltfläche "erstellen" klicken, sehen wir das Formular mit Meldungen für Validierungsfehler angezeigt, welche Felder die Validierungsregeln nicht erfüllt, dass wir definiert haben.</span><span class="sxs-lookup"><span data-stu-id="4a6d4-131">When we click on the Create button, we will see the form displayed with validation error messages showing which fields did not meet the validation rules we have defined.</span></span>

![](mvc-music-store-part-6/_static/image2.png)

## <a name="testing-the-client-side-validation"></a><span data-ttu-id="4a6d4-132">Testen die clientseitige Validierung</span><span class="sxs-lookup"><span data-stu-id="4a6d4-132">Testing the Client-Side Validation</span></span>

<span data-ttu-id="4a6d4-133">Eine serverseitige Validierung ist sehr wichtig ist, aus Anwendungssicht, da der Benutzer die clientseitige Überprüfung umgehen können.</span><span class="sxs-lookup"><span data-stu-id="4a6d4-133">Server-side validation is very important from an application perspective, because users can circumvent client-side validation.</span></span> <span data-ttu-id="4a6d4-134">Allerdings eine Webseite Formulare, die nur eine serverseitige Validierung implementieren drei schwerwiegende Probleme auf.</span><span class="sxs-lookup"><span data-stu-id="4a6d4-134">However, webpage forms which only implement server-side validation exhibit three significant problems.</span></span>

1. <span data-ttu-id="4a6d4-135">Der Benutzer muss warten, gesendet werden, auf dem Server überprüft das Formular, und klicken Sie für die Antwort an den Browser gesendet werden.</span><span class="sxs-lookup"><span data-stu-id="4a6d4-135">The user has to wait for the form to be posted, validated on the server, and for the response to be sent to their browser.</span></span>
2. <span data-ttu-id="4a6d4-136">Der Benutzer erhalten nicht sofort ein Feedback, wenn sie ein Feld, damit sie jetzt die Validierungsregeln erfolgreich zu beheben.</span><span class="sxs-lookup"><span data-stu-id="4a6d4-136">The user doesn't get immediate feedback when they correct a field so that it now passes the validation rules.</span></span>
3. <span data-ttu-id="4a6d4-137">Wir müssen die Serverressourcen zum Ausführen von Validierungslogik, statt den Browser des Benutzers nutzen verschwenden.</span><span class="sxs-lookup"><span data-stu-id="4a6d4-137">We are wasting server resources to perform validation logic instead of leveraging the user's browser.</span></span>

<span data-ttu-id="4a6d4-138">Sie müssen jedoch die ASP.NET MVC 3-gerüstvorlagen die clientseitige Validierung integriert, erfordert keine zusätzliche Arbeit jeglicher Art.</span><span class="sxs-lookup"><span data-stu-id="4a6d4-138">Fortunately, the ASP.NET MVC 3 scaffold templates have client-side validation built in, requiring no additional work whatsoever.</span></span>

<span data-ttu-id="4a6d4-139">Geben einen einzelnen Buchstaben in das Feld "Titel" erfüllt die überprüfungsanforderungen zu erfüllen, damit der validierungsmeldung sofort entfernt wird.</span><span class="sxs-lookup"><span data-stu-id="4a6d4-139">Typing a single letter in the Title field satisfies the validation requirements, so the validation message is immediately removed.</span></span>

![](mvc-music-store-part-6/_static/image3.png)


> [!div class="step-by-step"]
> <span data-ttu-id="4a6d4-140">[Zurück](mvc-music-store-part-5.md)
> [Weiter](mvc-music-store-part-7.md)</span><span class="sxs-lookup"><span data-stu-id="4a6d4-140">[Previous](mvc-music-store-part-5.md)
[Next](mvc-music-store-part-7.md)</span></span>
