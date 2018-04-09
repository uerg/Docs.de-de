---
uid: web-pages/overview/api-reference/asp-net-web-pages-api-reference
title: ASP.NET Web Pages (Razor) Quick-API-Referenz | Microsoft Docs
author: tfitzmac
description: Diese Seite enthält eine Liste mit kurzen Beispielen für die am häufigsten verwendeten Objekten, Eigenschaften und Methoden zur Programmierung von ASP.NET Web Pages mit Razor-Syntax.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2014
ms.topic: article
ms.assetid: 4001cb9b-3bfd-4ace-8a89-1561d8421e2c
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/api-reference/asp-net-web-pages-api-reference
msc.type: authoredcontent
ms.openlocfilehash: 5f9d84f4d453583d7d4eae12e4fc510275255616
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-web-pages-razor-api-quick-reference"></a>ASP.NET Web Pages (Razor)-API-Kurzübersicht
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> Diese Seite enthält eine Liste mit kurzen Beispielen für die am häufigsten verwendeten Objekten, Eigenschaften und Methoden zur Programmierung von ASP.NET Web Pages mit Razor-Syntax.
> 
> Beschreibungen, die mit "(v2)" gekennzeichnet, die in ASP.NET Web Pages 2-Version eingeführt wurden.
> 
> API-Referenzdokumentation, finden Sie unter der [ASP.NET Web Pages-Referenzdokumentation](https://go.microsoft.com/fwlink/?LinkId=208659) auf MSDN.
> 
> ## <a name="software-versions"></a>Versionen der Software
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> Dieses Lernprogramm funktioniert auch mit ASP.NET Web Pages 2 und ASP.NET Web Pages 1.0 (mit Ausnahme von Funktionen, die markiert v2).


Diese Seite enthält Referenzinformationen für Folgendes:

- [Klassen](#Classes)
- [Data](#Data)
- [Hilfsmethoden](#Helpers)
- [Validierung](#Validation)

<a id="Classes"></a>
## <a name="classes"></a>Klassen

### `AppState[key], AppState[index],App`

Enthält Daten, die von Seiten in der Anwendung gemeinsam verwendet werden können. Sie können den dynamischen `App` Eigenschaft Zugriff auf die gleichen Daten, wie im folgenden Beispiel:

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample1.html)]

### `AsBool(), AsBool(true|false)`

Konvertiert einen Zeichenfolgenwert in einen booleschen Wert (True/False). Gibt "false" oder den angegebenen Wert, wenn die Zeichenfolge nicht "true" / "false" darstellt.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample2.cs)]

### `AsDateTime(), AsDateTime(value)`

Konvertiert einen Zeichenfolgenwert, der Datum/Uhrzeit an. Gibt `DateTime.MinValue` oder den angegebenen Wert, wenn die Zeichenfolge keinen Datums-/Uhrzeitwert darstellt.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample3.cs)]

### `AsDecimal(), AsDecimal(value)`

Einen Zeichenfolgenwert konvertiert in einen Dezimalwert. Gibt 0,0 oder den angegebenen Wert, wenn die Zeichenfolge keinen decimal-Wert darstellt.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample4.cs)]

### `AsFloat(), AsFloat(value)`

Konvertiert einen Zeichenfolgenwert in einen Gleitkommawert an. Gibt 0,0 oder den angegebenen Wert, wenn die Zeichenfolge keinen decimal-Wert darstellt.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample5.cs)]

### `AsInt(), AsInt(value)`

Einen Zeichenfolgenwert konvertiert in eine ganze Zahl. Gibt 0 zurück, oder der angegebene Wert, wenn die Zeichenfolge keine ganze Zahl darstellt.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample6.cs)]

### `Href(path [, param1 [, param2]])`

Erstellt eine URL-Browser-kompatiblen über einen lokalen Pfad, mit optionalen zusätzlichen Pfad teilen.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample7.cshtml)]

### `Html.Raw(value)`

Rendert *Wert* als HTML-Markup und Rendern Ausgabe als HTML-codiert.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample8.cshtml)]

### `IsBool(), IsDateTime(), IsDecimal(), IsFloat(), IsInt()`

Gibt "true" zurück, wenn der Wert aus einer Zeichenfolge in den angegebenen Typ konvertiert werden kann.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample9.cs)]

### `IsEmpty()`

Gibt "true" zurück, wenn das Objekt oder die Variable über keinen Wert verfügt.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample10.cs)]

### `IsPost`

Gibt "true" zurück, wenn die Anforderung einer POST-Anforderung ist. (Anfänglichen Anforderungen sind in der Regel einen GET-Befehl).

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample11.cs)]

### `Layout`

Gibt den Pfad einer Layoutseite auf dieser Seite anwenden.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample12.html)]

### `PageData[key], PageData[index],Page`

Enthält Daten, die die Seite, Layoutseiten und Teilseiten gemeinsam, in der aktuellen Anforderung. Sie können den dynamischen `Page` Eigenschaft Zugriff auf die gleichen Daten, wie im folgenden Beispiel:

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample13.html)]

### `RenderBody()`

(Layoutseiten) Rendert den Inhalt der Inhaltsseite, die nicht in eine beliebige benannte Abschnitte enthalten ist.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample14.cs)]

### `RenderPage(path, values)`  
`RenderPage(path[,param1 [, param2]])`

Rendert eine Inhaltsseite mit dem angegebenen Pfad und einen optionalen zusätzlichen Daten. Sie erhalten den Wert der zusätzlichen Parameter aus `PageData` durch Position (z. B. 1) oder Schlüssel (z. B. 2).

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample15.js)]

### `RenderSection(sectionName [, required = true|false])`

(Layoutseiten) Rendert eine Inhaltsabschnitt, deren Name. Legen Sie *erforderlichen* auf "false", um einen Abschnitt optional zu machen.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample16.js)]

### `Request.Cookies[key]`

Ruft ab oder legt den Wert eines HTTP-Cookies.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample17.cs)]

### `Request.Files[key]`

Ruft die Dateien, die in der aktuellen Anforderung hochgeladen wurden.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample18.js)]

### `Request.Form[key]`

Ruft die Daten, die in einem Formular (als Zeichenfolgen) zurückgesendet wurde. `Request[key]` überprüft die `Request.Form` und `Request.QueryString` Sammlungen.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample19.cs)]

### `Request.QueryString[key]`

Ruft die Daten, die in der URL-Abfragezeichenfolge angegeben wurde. `Request[key]` überprüft die `Request.Form` und `Request.QueryString` Sammlungen.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample20.cs)]

### `Request.Unvalidated(key)`  
`Request.Unvalidated().QueryString|Form|Cookies|Headers[key]`

Selektiv Anforderungsvalidierung deaktiviert für Form-Elements, Abfragezeichenfolgen-Wert, Cookie oder Headerwert. Anforderungsüberprüfung ist standardmäßig aktiviert und verhindert, dass Benutzer Posten von Beiträgen Markup oder andere potenziell gefährlichen Inhalt.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample21.cs)]

### `Response.AddHeader(name, value)`

Fügt einen HTTP-Server-Header der Antwort hinzu.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample22.cs)]

### `Response.OutputCache(seconds [, sliding] [, varyByParams])`

Speichert die Seitenausgabe innerhalb eines angegebenen Zeitraums an. Legen Sie optional *gleitende* das Timeout bei jedem Seitenzugriff zurücksetzen und *VaryByParams* verschiedene Versionen der Seite für jede andere Abfragezeichenfolge in der Seitenanforderung zwischenzuspeichern.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample23.js)]

### `Response.Redirect(path)`

Die Browseranforderung umgeleitet an einen neuen Speicherort.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample24.js)]

### `Response.SetStatus(httpStatusCode)`

Legt den HTTP-Statuscode an den Browser gesendet.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample25.cs)]

### `Response.WriteBinary(data [, mimetype])`

Schreibt den Inhalt der *Daten* an die Antwort mit einer optionalen MIME-Typ.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample26.js)]

### `Response.WriteFile(file)`

Schreibt den Inhalt einer Datei in die Antwort an.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample27.cs)]

### `@section(sectionName) {content }`

(Layoutseiten) Definiert eine Inhaltsabschnitt, deren Name.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample28.cshtml)]

### `Server.HtmlDecode(htmlText)`

Decodiert eine Zeichenfolge, die HTML-codiert ist.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample29.cs)]

### `Server.HtmlEncode(text)`

Codiert eine Zeichenfolge zum Rendern in das HTML-Markup.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample30.cs)]

### `Server.MapPath(virtualPath)`

Gibt den physischen Pfad der Server für den angegebenen virtuellen Pfad zurück.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample31.cs)]

### `Server.UrlDecode(urlText)`

Decodiert Text aus einer URL an.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample32.cs)]

### `Server.UrlEncode(text)`

Codiert Text, in einer URL eingefügt werden soll.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample33.cs)]

### `Session[key]`

Ruft ab oder legt einen Wert, der vorhanden ist, bis der Benutzer den Browser schließt.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample34.css)]

### `ToString()`

Eine Zeichenfolgendarstellung der Wert des Objekts wird angezeigt.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample35.html)]

### `UrlData[index]`

Ruft zusätzliche Daten aus der URL (z. B. */MyPage/ExtraData*).

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample36.cs)]

### `WebSecurity.ChangePassword(userName,currentPassword,newPassword)`

Ändert das Kennwort für den angegebenen Benutzer.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample37.cs)]

### `WebSecurity.ConfirmAccount(accountConfirmationToken)`

Wird ein Konto mit das Kontotoken für die Bestätigung bestätigt.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample38.cs)]

### `WebSecurity.CreateAccount(userName, password`  
 `[, requireConfirmationToken = true|false])`

Erstellt ein neues Benutzerkonto mit dem angegebenen Benutzernamen und Kennwort an. Um ein bestätigungstoken erforderlich ist, übergeben Sie "true" für *RequireConfirmationToken.*

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample39.cs)]

### `WebSecurity.CurrentUserId`

Ruft den ganzzahligen Bezeichner für den derzeit angemeldeten Benutzer ab.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample40.cs)]

### `WebSecurity.CurrentUserName`

Ruft den Namen für den derzeit angemeldeten Benutzer ab.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample41.cs)]

### `WebSecurity.GeneratePasswordResetToken(username`  
 `[, tokenExpirationInMinutesFromNow])`

Generiert ein Zurücksetzen des Kennworts-Token, die per e-Mail an einen Benutzer gesendet werden kann, damit der Benutzer das Kennwort zurücksetzen kann.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample42.cs)]

### `WebSecurity.GetUserId(userName)`

Gibt die Benutzer-ID aus dem Benutzernamen zurück.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample43.cs)]

### `WebSecurity.IsAuthenticated`

Gibt "true" zurück, wenn der aktuelle Benutzer angemeldet ist.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample44.cs)]

### `WebSecurity.IsConfirmed(userName)`

Gibt "true" zurück, wenn der Benutzer (z. B. über eine e-Mail zur kaufbestätigung) bestätigt wurde.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample45.cs)]

### `WebSecurity.IsCurrentUser(userName)`

Gibt "true" zurück, wenn der Name des aktuellen Benutzers auf dem angegebenen Benutzernamen übereinstimmt.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample46.cs)]

### `WebSecurity.Login(userName,password[, persistCookie])`

Meldet den Benutzer in durch ein Authentifizierungstoken im Cookie festlegen.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample47.cs)]

### `WebSecurity.Logout()`

Meldet den Benutzer, durch das Entfernen des token Authentifizierungscookies.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample48.css)]

### `WebSecurity.RequireAuthenticatedUser()`

Wenn der Benutzer nicht authentifiziert ist, legt den HTTP-Status auf 401 (nicht autorisiert) fest.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample49.css)]

### `WebSecurity.RequireRoles(roles)`

Wenn der aktuelle Benutzer nicht Mitglied einer der angegebenen Rollen ist, legt den HTTP-Status auf 401 (nicht autorisiert) fest.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample50.html)]

### `WebSecurity.RequireUser(userId)`  
`WebSecurity.RequireUser(userName)`

Wenn der aktuelle Benutzer nicht den vom angegebenen Benutzer ist *Benutzername*, legt den HTTP-Status auf 401 (nicht autorisiert) fest.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample51.css)]

### `WebSecurity.ResetPassword(passwordResetToken,newPassword)`

Wenn das kennwortzurücksetzungstoken gültig ist, ändert das Kennwort des Benutzers auf das neue Kennwort.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample52.css)]

<a id="Data"></a>
## <a name="data"></a>Daten

### `Database.Execute(SQLstatement [,parameters]`

Führt *SQLstatement* (mit optionalen Parametern) wie z. B. INSERT, DELETE oder UPDATE und gibt die Anzahl der betroffenen Datensätze zurück.

[!code-sql[Main](asp-net-web-pages-api-reference/samples/sample53.sql)]

### `Database.GetLastInsertId()`

Gibt die Identitätsspalte der zuletzt eingefügten Zeile zurück.

[!code-sql[Main](asp-net-web-pages-api-reference/samples/sample54.sql)]

### `Database.Open(filename)`  
`Database.Open(connectionStringName)`

Öffnet die angegebene Datenbankdatei oder die Datenbank mithilfe einer benannten Verbindungszeichenfolge aus der *"Web.config"* Datei.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample55.cs)]

### `Database.OpenConnectionString(connectionString)`

Öffnet eine Datenbank mithilfe der Verbindungszeichenfolge. (Dies steht im Gegensatz zu `Database.Open`, die Name einer Verbindungszeichenfolge verwendet.)

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample56.cs)]

### `Database.Query(SQLstatement[,parameters])`

Abfragen der Datenbank mit *SQLstatement* (wahlweise können Parameter übergeben) und gibt die Ergebnisse als Auflistung zurück.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample57.html)]

### `Database.QuerySingle(SQLstatement [, parameters])`

Führt *SQLstatement* (mit optionalen Parametern) und gibt einen einzelnen Datensatz zurück.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample58.cs)]

### `Database.QueryValue(SQLstatement [, parameters])`

Führt *SQLstatement* (mit optionalen Parametern) und einen einzelnen Wert zurückgibt.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample59.cs)]

<a id="Helpers"></a>
## <a name="helpers"></a>Hilfsmethoden

### `Analytics.GetGoogleHtml(webPropertyId)`

Rendert den Google Analytics-JavaScript-Code für die angegebene ID

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample60.js)]

### `Analytics.GetStatCounterHtml(project,security)`

Rendert den StatCounter Analytics JavaScript-Code für das angegebene Projekt.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample61.css)]

### `Analytics.GetYahooHtml(account)`

Rendert den Yahoo Analytics JavaScript-Code für das angegebene Konto an.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample62.js)]

### `Bing.SearchBox([boxWidth])`

Übergibt eine Suche mit Bing an. Sie können zum Angeben der Website zum Durchsuchen und einen Titel für das Suchfeld Festlegen der `Bing.SiteUrl` und `Bing.SiteTitle` Eigenschaften. Normalerweise legen Sie diese Eigenschaften der  *\_AppStart* Seite.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample63.html)]

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample64.cshtml)]

### `Chart(width,height [, template] [, templatePath])`

Initialisiert ein Diagramm an.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample65.cshtml)]

### `Chart.AddLegend([title] [, name])`

Ein Diagramm hinzugefügt eine Legende.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample66.cshtml)]

### `Chart.AddSeries([name] [, chartType] [, chartArea]`  
 `[, axisLabel] [, legend] [, markerStep] [, xValue]`  
 `[, xField] [, yValues] [, yFields] [, options])`

Fügt eine Reihe von Werten für das Diagramm an.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample67.cshtml)]

### `Crypto.Hash(string [, algorithm])`  
`Crypto.Hash(bytes [, algorithm])`

Gibt einen Hash für die angegebenen Daten zurück. Standardmäßig wird der Algorithmus `sha256`.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample68.html)]

### `Facebook.LikeButton(href [, buttonLayout] [, showFaces] [, width] [, height]`   
 `[, action] [, font] [, colorScheme] [, refLabel])`

Ermöglicht Benutzern das Herstellen einer Verbindung mit Seiten Facebook.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample69.js)]

### `FileUpload.GetHtml([initialNumberOfFiles] [, allowMoreFilesToBeAdded]`  
 `[, includeFormTag] [, addText] [, uploadText])`

Rendert Benutzeroberfläche für das Hochladen von Dateien an.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample70.html)]

### `GamerCard.GetHtml(gamerTag)`

Rendert das angegebene Spiele Xbox-Tag.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample71.js)]

### `Gravatar.GetHtml(email [, imageSize] [, defaultImage] [, rating]`  
 `[, imageExtension] [, attributes])`

Rendert das a. mit Gravatar-Image für die angegebene e-Mail-Adresse an.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample72.css)]

### `Json.Encode(object)`

Konvertiert ein Datenobjekt in eine Zeichenfolge im Format JavaScript Object Notation (JSON).

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample73.cs)]

### `Json.Decode(string)`

Konvertiert eine JSON-codierte Eingabezeichenfolge an ein Datenobjekt, das Sie durchlaufen oder in eine Datenbank einfügen können.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample74.cs)]

### `LinkShare.GetHtml(pageTitle[, pageLinkBack] [, twitterUserName]`  
 `[, additionalTweetText] [, linkSites])`

Rendert social networking Links, die mit dem angegebenen Titel und eine optionale URL an.

[!code-xml[Main](asp-net-web-pages-api-reference/samples/sample75.xml)]

### `ModelStateDictionary.AddError(key, errorMessage)`

Ordnet eine Fehlermeldung eines Formularfelds. Verwenden der `ModelState` Helper auf diesen Member zugreifen.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample76.cs)]

### `ModelStateDictionary.AddFormError(errorMessage)`

Ordnet eine Fehlermeldung mit einem Formular an. Verwenden der `ModelState` Helper auf diesen Member zugreifen.

[!code-powershell[Main](asp-net-web-pages-api-reference/samples/sample77.ps1)]

### `ModelStateDictionary.IsValid`

Gibt "true" zurück, wenn keine gültigkeitsprobleme bestehen. Verwenden der `ModelState` Helper auf diesen Member zugreifen.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample78.cs)]

### `ObjectInfo.Print(value [, depth] [, enumerationLength])`

Rendert die Eigenschaften und Werte von einem Objekt und alle untergeordneten Objekte.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample79.css)]

### `Recaptcha.GetHtml([, publicKey] [, theme] [, language] [, tabIndex])`

Rendert die ReCAPTCHA-Überprüfungstest.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample80.css)]

### `ReCaptcha.PublicKey`  
 `ReCaptcha.PrivateKey`

Legt die öffentlichen und privaten Schlüssel für den ReCAPTCHA-Dienst. Normalerweise legen Sie diese Eigenschaften der  *\_AppStart* Seite.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample81.css)]

### `ReCaptcha.Validate([, privateKey])`

Gibt das Ergebnis des Tests ReCAPTCHA zurück.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample82.cs)]

### `ServerInfo.GetHtml()`

Rendert die Statusinformationen zu ASP.NET Web Pages.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample83.cshtml)]

### `Twitter.Profile(twitterUserName)`

Rendert einen Twitter-Datenstrom für den angegebenen Benutzer.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample84.js)]

### `Twitter.Search(searchQuery)`

Rendert einen Twitter-Datenstrom für den angegebenen Suchtext ein.

[!code-xml[Main](asp-net-web-pages-api-reference/samples/sample85.xml)]

### `Video.Flash(filename [, width, height])`

Rendert einen video Flash Player für die angegebene Datei mit optionalen Breite und Höhe.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample86.cshtml)]

### `Video.MediaPlayer(filename [, width, height])`

Rendert einen Windows Media Player für die angegebene Datei mit optionalen Breite und Höhe.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample87.cshtml)]

### `Video.Silverlight(filename, width, height)`

Rendert einen Silverlight-Player für den angegebenen *XAP* Datei mit erforderliche Breite und Höhe.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample88.cshtml)]

### `WebCache.Get(key)`

Gibt das Objekt vom angegebenen *Schlüssel*, oder null, wenn das Objekt nicht gefunden wird.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample89.cs)]

### `WebCache.Remove(key)`

Entfernt das Objekt vom angegebenen *Schlüssel* aus dem Cache.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample90.cs)]

### `WebCache.Set(key, value [, minutesToCache] [, slidingExpiration])`

Setzt *Wert* in den Cache unter dem Namen gemäß *Schlüssel*.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample91.html)]

### `WebGrid(data)`

Erstellt ein neues `WebGrid` -Objekt mithilfe der Daten aus einer Abfrage.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample92.cs)]

### `WebGrid.GetHtml()`

Rendert Markup zum Anzeigen von Daten in einer HTML-Tabelle.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample93.html)]

### `WebGrid.Pager()`

Rendert einen Pager für die `WebGrid` Objekt.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample94.html)]

### `WebImage(path)`

Lädt ein Bild aus dem angegebenen Pfad.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample95.cs)]

### `WebImage.AddImagesWatermark(image)`

Fügt das angegebene Bild als Wasserzeichen.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample96.cs)]

### `WebImage.AddTextWatermark(text)`

Fügt den angegebenen Text zu dem Bild.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample97.cs)]

### `WebImage.FlipHorizontal()`  
`WebImage.FlipVertical()`

Kippt das Bild, horizontal oder vertikal.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample98.css)]

### `WebImage.GetImageFromRequest()`

Lädt ein Bild an, wenn ein Bild auf einer Seite, während das Hochladen einer Datei zurückgesendet wird.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample99.cs)]

### `WebImage.Resize(width,height)`

Ändert die Größe ein Bild.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample100.css)]

### `WebImage.RotateLeft()`  
`WebImage.RotateRight()`

Dreht das Bild nach links oder rechts.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample101.css)]

### `WebImage.Save(path [, imageFormat])`

Speichert das Bild in den angegebenen Pfad.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample102.js)]

### `WebMail.Password`

Legt das Kennwort für den SMTP-Server an. Normalerweise legen Sie diese Eigenschaft der  *\_AppStart* Seite.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample103.cs)]

### `WebMail.Send(to, subject, body [, from] [, cc] [, filesToAttach] [, isBodyHtml]`  
 `[, additionalHeaders])`

Sendet eine E-Mail.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample104.css)]

### `WebMail.SmtpServer`

Legt den Namen des SMTP-Server. Normalerweise legen Sie diese Eigenschaft der<em>\_AppStart</em> Seite.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample105.html)]

### `WebMail.UserName`

Legt den Benutzernamen für den SMTP-Server fest. Normalerweise sollten Sie diese Eigenschaft festlegen, der  *\_AppStart* Seite.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample106.html)]

<a id="Validation"></a>
## <a name="validation"></a>Validierung

### `Html.ValidationMessage(field)`

(v2) Rendert eine Überprüfungsfehlermeldung für das angegebene Feld.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample107.cshtml)]

### `Html.ValidationSummary([message])`

(v2) Zeigt eine Liste aller Validierungsfehler.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample108.cshtml)]

### `Validation.Add(field, validationType)`

(v2) Registriert ein benutzereingabeelement für den angegebenen Typ der Überprüfung an.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample109.js)]

### `Validation.ClassFor(field)`

(v2) Rendert dynamisch CSS-Klassenattribute für die clientseitige Validierung auf, sodass Sie Überprüfungsfehlermeldungen formatieren können. (Erfordert, dass Sie die entsprechenden Clientskriptbibliotheken verweisen, sodass Sie CSS-Klassen definieren.)

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample110.html)]

### `Validation.For(field)`

(v2) Ermöglicht die clientseitige Validierung für das Eingabefeld für die Benutzer an. (Erfordert, dass Sie die entsprechenden Clientskriptbibliotheken verweisen.)

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample111.html)]

### `Validation.IsValid()`

(v2) Gibt "true" zurück, wenn alle benutzereingabeelemente, die für die Validierung Registred sind gültige Werte enthalten.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample112.cs)]

### `Validation.RequireField(field[, errorMessage])`

(v2) Gibt an, dass Benutzer einen Wert für das benutzereingabeelement bereitstellen müssen.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample113.cs)]

### `Validation.RequireFields(field1[, field12, field3, ...])`

(v2) Gibt an, dass Benutzer Werte für die einzelnen benutzereingabeelemente bereitstellen müssen. Diese Methode ermöglicht nicht zu, dass Sie eine benutzerdefinierte Fehlermeldung angeben.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample114.html)]

### `Validator.DateTime ([error message])`  
`Validator.Decimal([error message])`  
`Validator.EqualsTo(otherField,[error message])`  
`Validator.Float([error message])`  
`Validator.Integer([error message])`  
`Validator.Range(min,max [, error message])`  
`Validator.RegEx(pattern[, error message])`  
`Validator.Required([error message])`  
`Validator.StringLength(length)`  
`Validator.Url([error message])`

(v2) Gibt einen Überprüfungstest, bei der Verwendung der `Validation.Add` Methode.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample115.js)]
