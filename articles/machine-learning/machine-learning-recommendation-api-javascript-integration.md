<properties 
    pageTitle="Koneen Learning suosituksia: JavaScript-integroinnin | Microsoft Azure" 
    description="Azure koneen Learning suositukset - JavaScript-integrointi - asiakirjat" 
    services="machine-learning" 
    documentationCenter="" 
    authors="LuisCabrer" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="javascript" 
    ms.topic="article" 
    ms.date="09/08/2016" 
    ms.author="luisca"/>

# <a name="azure-machine-learning-recommendations---javascript-integration"></a>Azure Konepohjaisten Oppimistekniikoiden suositukset - JavaScript-integrointi

>[AZURE.NOTE]Käynnistä suosituksia API kognitiiviset Service-Esityspalvelua käyttämällä tämän version sijaan. Kognitiiviset suositukset-palvelu korvaaminen palvelua ja kaikkien uusien ominaisuuksien kehittämisestä siellä. Siinä on uusia ominaisuuksia, kuten jonottaminen tuki, paremmin API Explorer, selkeämpi API pinta, yhdenmukaisempi kirjautuminen/Laskutus kokemus ja niin edelleen.
> Lisätietoja [uuden kognitiiviset palvelun siirtyminen](http://aka.ms/recomigrate)


Tämän asiakirjan kuvaavat, miten voit integroida sivustoosi käyttäen JavaScript. JavaScript-koodia avulla voit lähettää hankintaa tapahtumien ja tarjoaman suositukset, kun luot suositus malli. Kaikki toiminnot, jotka on tehty JS kautta voit tehdä myös palvelinpuolen.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

##<a name="1-general-overview"></a>1 yleiskatsaus
Sivuston integraation Azure ML suosituksia koostuvat-2 vaiheet:

1.  Lähetä tapahtumien Azure ML suosituksia kyselyjä. Tämä ottaa käyttöön luonnissa suositus malli.
2.  Kuluttavat suositukset. Kun valmista mallia voit käyttää suositukset. (Saat lisätietoja siitä, miten pikaopas tässä asiakirjassa ei kerrotaan, kuinka voit luoda mallin, lue).


<ins>Vaiheen I</ins>

Ensimmäisessä vaiheessa voit lisätä html-sivuja small JavaScript-kirjastoon, joka mahdollistaa sivun tapahtumien lähettää kyselyjä Azure ML suosituksia servers (joko Data Market) ilmenee html-sivulla:

![Drawing1][1]

<ins>Vaiheen II</ins>

Toinen vaihe, kun haluat näyttää suositukset-sivulla Valitse luettelosta jokin seuraavista vaihtoehdoista:

1. palvelimen (sivuhahmonnuksen vaihe) kutsuja Azure ML suosituksia Server (joko Data Market) Nouda suositukset. Tulokset ovat luettelon kohteiden tunnus. Palvelin on palvelee tulokset kohteilla metatiedot (kuten kuvien, kuvaus) ja lähettää luotu sivun selaimessa.

![Drawing2][2]

2. vaihtoehto on yksinkertainen luettelo suositellut kohteet pieni JavaScript-tiedoston vaiheen avulla. Vastaanotettu tähän tiedot joustavasti entistä useammissa kuin se, valitse ensimmäinen vaihtoehto.

![Drawing3][3]

##<a name="2-prerequisites"></a>2. edellytykset

1. Luo uusi malli ohjelmointirajapinnan käyttäminen. Artikkelissa pikaopas tehtävän suorittaminen.
2. Koodata oman &lt;dataMarketUser&gt;:&lt;dataMarketKey&gt; base64 kanssa. (Tämä käytetään basic todennuksen käyttöön Soita API JS-koodi).


##<a name="3-send-data-acquisition-events-using-javascript"></a>3. Lähetä hankintaa tapahtumien JavaScript käyttäminen
Seuraavat vaiheet helpottamiseksi lähettämisen tapahtumia:

1.  Sisällytä JQuery kirjaston koodisi. Voit ladata sen nuget, kirjoita seuraava URL-osoite.

        http://www.nuget.org/packages/jQuery/1.8.2
2.  Sisällytä suosituksia Java-komentosarjojen kirjastossa seuraava URL-osoite: http://aka.ms/RecoJSLib1

3.  Alusta Azure ML suositukset-kirjastossa, jossa on tarvittavat parametrit.

        <script>
            AzureMLRecommendationsStart("<base64encoding of username:key>",
            "<model_id>");
        </script>

4.  Lähetä tapahtumatoimintosarja. Yksityiskohtainen jäljempänä kohdassa näkyvät kaikki tapahtumat tyyppi (esimerkiksi valitsemalla tapahtumalokiin)

        <script>
            if (typeof AzureMLRecommendationsEvent=="undefined") {      
                        AzureMLRecommendationsEvent = [];
                    }
            AzureMLRecommendationsEvent.push({ event: "click", item: "18321116" });
        </script>


###<a name="31-limitations-and-browser-support"></a>3.1. Rajoitukset ja selaintuki
Tämä on viittaus käyttöönotto ja se on määritetty ei. Se olisi tuen pää kaikissa selaimissa.

###<a name="32-type-of-events"></a>3.2. Tapahtumien tyyppi
Tapahtuman, joka tukee kirjaston 5 perustyyppiä: Napsauta, suositus, Lisää kahvilassa ostoskorin, jos haluat poistaa kahvilassa ostoskori ja Osta. Ei muita tapahtuma, joka määritetään nimeltä kirjautuminen käyttäjäkontekstissa.

####<a name="321-click-event"></a>3.2.1. Valitse tapahtuma
Tapahtuman käytetään aina, kun käyttäjä napsauttaa kohteen. Yleensä silloin, kun käyttäjä napsauttaa kohteen uusi sivu avataan kohteen; tiedot Tällä sivulla tämä pitäisi olla tapaus.

Parametrit:
- tapahtuman (merkkijono, pakollinen) - "napsauttamalla"
- kohde (merkkijono, pakollinen) - kohteen yksilöivä tunnus
- itemName (merkkijono, valinnainen): kohteen nimi
- itemDescription (merkkijono, valinnainen) - kohteen kuvaus
- itemCategory (merkkijono, valinnainen) - kohteen luokka
        
        <script>
            if (typeof AzureMLRecommendationsEvent == "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: "click", item: "3111718" });
        </script>

Tai valinnaiset tiedot:

        <script>
            if (typeof AzureMLRecommendationsEvent === "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "click", item: "3111718", itemName: "Plane", itemDescription: "It is a big plane", itemCategory: "Aviation"});
        </script>


####<a name="322-recommendation-click-event"></a>3.2.2. Suositus Valitse tapahtuma
Tapahtuman voidaan käyttää minkä tahansa aika, jonka käyttäjä valitsee kohteessa, joka saatiin Azure ML suosituksia suositellut kohteeksi. Yleensä silloin, kun käyttäjä napsauttaa kohteen uusi sivu avataan kohteen; tiedot Tällä sivulla tämä pitäisi olla tapaus.

Parametrit:
- tapahtuman (merkkijono, pakollinen) - "recommendationclick"
- kohde (merkkijono, pakollinen) - kohteen yksilöivä tunnus
- itemName (merkkijono, valinnainen): kohteen nimi
- itemDescription (merkkijono, valinnainen) - kohteen kuvaus
- itemCategory (merkkijono, valinnainen) - kohteen luokka
- alustaa (merkkijono matriisi, valinnainen) - siemenet, jotka on luotu suositus kysely.
- recoList (merkkijono matriisi, valinnainen) - kohdetta, joka on napsautettu luoneen suositus pyynnön tulos.
        
        <script>
            if (typeof AzureMLRecommendationsEvent=="undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "recommendationclick", item: "18899918" });
        </script>

Tai valinnaiset tiedot:

        <script>
            if (typeof AzureMLRecommendationsEvent == "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: eventName, item: "198", itemName: "Plane2", itemDescription: "It is a big plane2", itemCategory: "Default2", seeds: ["Seed1", "Seed2"], recoList: ["199", "198", "197"]               });
        </script>


####<a name="323-add-shopping-cart-event"></a>3.2.3. Lisää ostoskoriin ostoskorin tapahtuma
Tapahtuman tulisi käyttää kun käyttäjä lisää kohde ostoskoriin.
Parametrit:
* tapahtuman (merkkijono, pakollinen) - "addshopcart"
* kohde (merkkijono, pakollinen) - kohteen yksilöivä tunnus
* itemName (merkkijono, valinnainen): kohteen nimi
* itemDescription (merkkijono, valinnainen) - kohteen kuvaus
* itemCategory (merkkijono, valinnainen) - kohteen luokka
        
        <script>
            if (typeof AzureMLRecommendationsEvent == "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "addshopcart", item: "13221118" });
        </script>

####<a name="324-remove-shopping-cart-event"></a>3.2.4. Poista ostoskorin tapahtuman ostaminen
Tapahtuman käytetään, kun käyttäjä poistaa kohteen ostoskoriin.

Parametrit:
* tapahtuman (merkkijono, pakollinen) - "removeshopcart"
* kohde (merkkijono, pakollinen) - kohteen yksilöivä tunnus
* itemName (merkkijono, valinnainen): kohteen nimi
* itemDescription (merkkijono, valinnainen) - kohteen kuvaus
* itemCategory (merkkijono, valinnainen) - kohteen luokka
        
        <script>
            if (typeof AzureMLRecommendationsEvent=="undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: "removeshopcart", item: "111118" });
        </script>

####<a name="325-purchase-event"></a>3.2.5. Osta tapahtuma
Tapahtuman käytetään, kun käyttäjä ostaa hänen ostoskori.

Parametrit:
* tapahtuman (merkkijono) - "ostaa"
* kohteiden (ostettu []) - merkinnän tilan kunkin kohteen ostettu matriisi.<br><br>
Ostettujen muoto:
    * kohde (merkkijono) - kohteen yksilöllinen.
    * kohteet, jotka on ostettu (kokonaisluku tai merkkijono) - määrän laskeminen
    * Hinta (liukuluku tai merkkijonon) - valinnainen kenttä - kohteen hinta.

Alla olevassa esimerkissä 3 hankkiminen kohteiden (33, 34, 35) kaksi ja kaikki kentät täydennetty (kohteen, määrä, hinta) ja toinen (kohde 34) ilman hinta.

        <script>
            if ( typeof AzureMLRecommendationsEvent == "undefined"){ AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: "purchase", items: [{ item: "33", count: "1", price: "10" }, { item: "34", count: "2" }, { item: "35", count: "1", price: "210" }] });
        </script>

####<a name="326-user-login-event"></a>3.2.6. Käyttäjän kirjautuminen tapahtuma
Azure ML suosituksia tapahtuman kirjaston Luo ja käyttää eväste tunnistaa tapahtumat, jotka ovat peräisin samassa selaimessa. Jotta voit parantaa Azure ML suosituksia avulla voit määrittää käyttäjän yksilöivä tunnus, joka ohittaa evästeiden käyttö mallin tulokset.

Tapahtuman käytetään-sivustoon kirjautuminen jälkeen.

Parametrit:
* tapahtuman (merkkijono) - "userlogin"
* käyttäjä (merkkijono) - käyttäjän yksilöllinen tunnus.
        <script>
  Jos (typeof AzureMLRecommendationsEvent == "Määrittämätön") {AzureMLRecommendationsEvent = [;]} AzureMLRecommendationsEvent.push ({tapahtuman: "userlogin"-käyttäjä: "ABCD10AA"});      </script>

##<a name="4-consume-recommendations-via-javascript"></a>4. tarjoaman suosituksia JavaScript kautta
Asiakkaan WWW-sivu on käynnistämä koodi, joka käyttää suositus joitakin JavaScript-tapahtuman mukaan. Suositus vastaus sisältää suositellut kohteet tunnukset, heidän nimensä ja niiden luokituksen. Käytä tätä vaihtoehtoa vain suositellut kohteet luettelon ulkoasun kannattaa – monimutkaisia käsittely (esimerkiksi lisäämällä kohteen metatiedoista) tehdyn server puoli-integrointi.

###<a name="41-consume-recommendations"></a>4.1 tarjoaman suositukset
Tarjoaman suositukset, sinun on ovat tarvittavat JavaScript-kirjastot-sivulle ja soita AzureMLRecommendationsStart. Kohdassa 2.

Suosituksia yhden tai useamman kohteen, sinun on soitettava menetelmän tarjoaman: AzureMLRecommendationsGetI2IRecommendation.

Parametrit:
* kohteiden (merkkijonoja matriisi) - yhden tai useamman kohteen saat suosituksia. Jos muuttaa FBT-maksuerien muodosta tarjoaman jälkeen voit määrittää tässä vain yhden kohteen.
* numberOfResults (int) - tarvittavat tuloksia.
* includeMetadata (totuusarvo, valinnainen) – Jos "true" määrittäminen ilmaisee metatietojen-kenttä on täytettävä tuloksessa.
* Toiminto - funktiota, joka käsittelee suositukset käsittelyn palautetaan. Palauttaa matriisin nimellä:
    * Kohde - kohteen yksilöllinen tunnus
    * nimi - kohteen nimi (jos olemassa luettelo)
    * luokitus - suositus luokitus
    * metatieto - merkkijono, joka edustaa kohteen metatiedot

Esimerkki: Seuraava koodi pyytää 8 suosituksia kohteen "64f6eb0d-947a-4c18-a16c-888da9e228ba" (ja määrittämällä ei includeMetadata - implisiittisesti lukee ei ole metatietoja tarvitaan), se sitten yhdistää tulokset puskurin kyselyjä.

        <script>
            var reco = AzureMLRecommendationsGetI2IRecommendation(["64f6eb0d-947a-4c18-a16c-888da9e228ba"], 8, false, function (reco) {
                var buff = "";
                for (var ii = 0; ii < reco.length; ii++) {
                    buff += reco[ii].item + "," + reco[ii].name + "," + reco[ii].rating + "\n";
                }
                alert(buff);
            });
        </script>


[1]: ./media/machine-learning-recommendation-api-javascript-integration/Drawing1.png
[2]: ./media/machine-learning-recommendation-api-javascript-integration/Drawing2.png
[3]: ./media/machine-learning-recommendation-api-javascript-integration/Drawing3.png
 
