Utvärdera webbplatsers laddningstid och användbarhet
=======================

Till denna uppgift skulle vi välja ut ett antal olika webbplatser, mäta hur snabbt de laddas och diskutera kring vad som kan göras för att förbättra dem.

Urval
-----------------------

Jag valde tre stycken hemsidor med konst som gemensam grund då jag ville inspektera sidor med mycket bilder och se hur deras developers hanterat detta. Sidorna jag valt ut är:

**Artstation.com** - Den mest populära hemsidan för digital konst för tillfället.
De undersidor jag också undersökt är <a href="http://artstation.com/jobs/all">Jobs</a> och <a href="https://www.artstation.com/learning">Learning</a>.

**Deviantart.com** - En äldre klassisk hemsida för olika alster.
De undersidor jag också undersökt är <a href="http://deviantart.com/shop">Shop</a> och en slumpvis vald <a href="http://deviantart.com/bradwright">Profile</a>.

**Inprnt.com** - En professionel sida för att köpa och sälja prints. 
De undersidor jag också undersökt är <a href="https://www.inprnt.com/browse/">Browse</a> och en slumpvis vald <a href="https://www.inprnt.com/gallery/nataliazaitseva/">Profile</a>.

Metod
-----------------------

För att mäta och samla data har jag använt dessa verktyg:

**Devtools (Networks) i Firefox** - Detta har använts för att mäta DOM Content Loaded och Page Load. Mätningarna har gjorts 3 gånger för varje sida och resultatet som presenteras är snittet av dessa tre försök.

**Google PageSpeed Insight** - Detta har använts för att undersöka sidans prestanda och för att få insikt i olika problem och lösningar angående sidornas laddningstid.

**Google Sheets** - Används för att presentera resultatet av mätningar. 

Resultat
-----------------------

Mätningar:
<div class="sheet-container">
    <iframe src="https://docs.google.com/spreadsheets/d/e/2PACX-1vSDTx3AiY4JDIjDaGNsOgzKOpF_SGkNunXWHEGkqQz0aJZDH-dmtT9dN3bgRwR-E4klQIJxHDuPYCkH/pubhtml?gid=0&amp;single=true&amp;widget=true&amp;headers=false"></iframe>
</div>

<br>

**Artstation**

![artstation](%assets_url%/img/artstation.jpg)

Denna sida är extremt bild-tung, men det är ändå inte en god anledning till att mätvärdena ska vara så dåliga. 
Några förbättringar som kan genomföras:
* Passande storlek på bilder 
* Preloada LCP (Det största elementet, i nuvarande stund en black friday popup)
* Visa bilder i next-gen format som WebP eller AVIF.
* Reducera JavaScript som inte används
* Ta bort render-blockerande resurser som hindrar FCP
* Unvik överdriven DOM storlek

<br>
För undersidorna <a href="http://artstation.com/jobs/all">Jobs</a> och <a href="https://www.artstation.com/learning">Learning</a> skulle man även kunna reducera oanvänt CSS och försöka unvika att ge legacy javascript till moderna webbläsare. Jag funderar även kring om deras infinite scroll verkligen är så optimerad med lazy-loading då det verkar laddas in många offscreen bilder långt innan man scrollat ned till dem. 

**Deviantart**

![deviantart](%assets_url%/img/deviantart.jpg)

Detta är också en bild-tung sida, men den har lite bättre mätvärden än artstation. 
Några förbättringar som kan föreslås:

* Reducera JavaScript som inte används
* Visa bilder i next-gen format som WebP eller AVIF
* Passande storlek på bilder 
* Reducera CSS som inte används
* Ta bort render-blockerande resurser som hindrar FCP

<br>
För desktop borde man även ta bort dublett-moduler i Javascript bundles och för <a href="http://deviantart.com/shop">Shop</a> och <a href="http://deviantart.com/bradwright">Profile</a> sidorna bör man även försöka minska tiden för initial server respons och köra lazy-loading på offscreen och dolda bilder.

**InPrnt**

![inprnt](%assets_url%/img/inprnt.jpg)

Denna sida fungerar bäst av de jag testat, några förbättringar som ändå skulle kunna genomföras:
* Visa bilder i next-gen format som WebP eller AVIF
* Passande storlek på bilder 
* Encodea bilder mer effektivt
* Reducera JavaScript som inte används
* Reducera CSS som inte används
* Ta bort render-blockerande resurser som hindrar FCP

<br>
För undersidorna <a href="https://www.inprnt.com/browse/">Browse</a> och <a href="https://www.inprnt.com/gallery/nataliazaitseva/">Profile</a> borde man även försöka minska tiden för initial server response.


Slutligen testade jag även att throttla startsidorna med Regular 3G för mobil där resultaten blev:

* Artstation: DOMContentLoaded: 813 ms, load: 3,06 min
* Deviantart: DOMContentLoaded: 15,57 s, load: 48,96 s
* Inprnt: DOMContentLoaded: 27,85 s, load: 30,86 s

<br>
Analys
-----------------------

Bäst till sämst enligt mätningar blir:
1. InPrnt
2. DeviantArt
3. Artstation

<br>
Även om sidorna är väldigt bild/resource-tunga tycker jag att de överlag har dålig Performance score. Alla sidor hade liknande problem som deras developers skulle kunna ta itu med som t.ex. att optimera bilder och reducera javascript som inte används.
InPrnt är den enda av sidorna som har en relativt OK score, medan Deviantart och Artstation är på gränsen, vilket jag tycker är uppseendeväckande när det är såpass populära sidor. Dock anser jag att när man själv testar att besöka dessa sidor så upplevs de ändå relativt snabbladdade; det enda som känns oacceptabelt sakta är när man går in på Artstation med sämre internet-uppkoppling.

Något man dock måste ha i åtanke är att bilderna på dessa sidor till största del laddas upp av användare, vilket betyder att man inte kan kontrollera alla parametrar direkt som developer utan måste normalisera allt i efterhand med compression/conversion/resizing och så vidare beroende på vad som laddats up. På t.ex. Artstations hemsida kan man läsa att många användare vill att sidan ska visa upp okomprimerade PNG-filer i full storlek, vilket givetvis inte känns rimligt när det görs hundratals uppladdningar per dag. Då bildkvalité ändå är av högsta vikt för användare ger sidorna förslag på vilka filformat, storlek och hur bilderna ska exporteras för att visas i bästa kvalité trots att de måste ändras. Med tanke på att användarna är så angelägna om att bibehålla hög kvalité på bilderna tror jag att de ändå kan acceptera en lite lägre performance score, men personligen anser jag att det ska inte behöva göra sådan stor inverkan på performance som det gör i nuläget.

Summeringen är alltså att trots att alla sidor ändå upplevs som OK att besöka som användare kan det göras väldigt mycket förbättringar på alla tre för att hjälpa performance, accessability och sökranking.


Referenser
-----------------------

<a href="pagespeed.web.dev">PageSpeed Insights</a>

<a href="https://moz.com/learn/seo/page-speed">Moz Page Speed</a>

<a href="https://help.artstation.com/hc/en-us/articles/360056654272-How-do-I-get-the-best-quality-of-images-on-ArtStation-">Artstation - How To Get Best Quality Images</a>

Övrigt
-----------------------

Rapport skriven av Isabel Westling