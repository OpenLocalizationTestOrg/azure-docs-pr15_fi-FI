<properties
   pageTitle="Tietoja ja BizTalk säännöt API-sovelluksen luominen logiikan sovelluksen | Microsoft Azure"
   description="Tässä ohjeaiheessa käsitellään BizTalk säännöt Connectorin ominaisuuksien ja ohjeita sen käyttö"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="anuragdalmia"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="04/20/2016"
   ms.author="andalmia"/>

#<a name="biztalk-rules"></a>BizTalk säännöt

[AZURE.INCLUDE [app-service-logic-version-message](../../includes/app-service-logic-version-message.md)]

Liiketoiminnan sääntöjen kapseloi päätöksiä, jotka ohjaavat liiketoimintaprosessien ja käytännöt. Käytännöt mahdollisesti määritellään varsinaisesti toimintosarjan käyttöoppaat, sopimuksia tai sopimuksia tai saattaa olla knowledge tai ilmentämää työntekijöiden osaamisalueet. Käytännöt ovat dynaamisia ja saatetaan muuttaa ajan asianmukaisesti voi muokata business-palvelupakettia, asetukset ja muista syistä.

Käyttöönoton käytännöt perinteinen ohjelmoinnin kielillä edellyttää aikaa ja koordinointi ja ota käyttöön osallistumaan luomisen ja ylläpidon yrityksen käytäntöjä ei ohjelmoijille. BizTalk liiketoiminnan sääntöjen avulla on helppo toteuttaa kyseiset käytännöt ja irrottamisen Liiketoimintaprosessi loput nopeasti. Näin tehdä tarvittavat muutokset yrityksen käytäntöjä ilman vaikuttavat muiden Liiketoimintaprosessi.

##<a name="why-rules"></a>Miksi sääntöjä

Syitä 3 avaimen BizTalk liiketoiminnan sääntöjen käyttäminen Liiketoimintaprosessi:

* Irrottamisen liiketoimintalogiikan sovelluksen koodista
- Salli liiketoiminnan analyytikot Lisää päättää yrityksen logiikan hallinta
+ Liiketoimintalogiikan muutokset Siirry tuotannon nopeammin

##<a name="rules-concepts"></a>Sääntöjen käsitteitä

##<a name="vocabulary"></a>Sanasto

_Sanasto_ on joukko koostuu helpossa muodossa käytetään säännön ehtojen ja toimintojen tietojenkäsittely objektien nimet määritykset. Sanaston määritelmät helpottaa sääntöjä lukeminen ja ymmärtäminen sekä jakaa tietyn liiketoiminta henkilöt.

Vocabularies on suunniteltu kuroa business semantiikkaan liittyvien ja käyttöönoton välillä. Tietojen sidonta, saat hyväksyntätila voi esimerkiksi osoittamalla tiettyjä sarakkeessa tietyn rivin tiettyjä tietokannan, vastaa SQL-kysely. Sen sijaan, että lisäät esitys monimutkainen tällaiset säännön, voi sen sijaan luoda sanaston määrityksen, liittyvät kyseisen tietojen sidonta, helpossa muodossa nimellä "Tilan." Sen jälkeen voit sisällyttää "Tila" jokin muu luku sääntöjen ja säännön ohjelma vastaavien tietojen hakemiseen taulukon.

##<a name="rule"></a>Sääntö

Liiketoiminnan säännöt ovat määritettäviä lausekkeita, jotka ohjaavat liiketoimintaprosessien järjestäminen. Säännön koostuu ehto ja toiminnot. Ehto arvioidaan ja sen arvo on TOSI, jos sääntö-ohjelma aloittaa seuraavia toimia.
Säännöissä Business säännöt määritetään käyttämällä seuraavanlainen:

_IF_ _ehto_ _Valitse_ _toiminto_

Esimerkki:

*Jos summa on pienempi tai yhtä käytettävissä varoihin*  
*Valitse Järjestä tapahtuma- ja tulostuksen vastaanotto*

Tämä sääntö määrittää, onko tapahtuman suoritetaan käyttämällä liiketoimintalogiikan kaksi raha-arvot, tapahtuman summa ja käytettävissä olevat varat vertailu-lomakkeessa.
Voit luoda, muokata ja otetaan käyttöön sääntöjä liiketoimintasääntö. Vaihtoehtoisesti voit suorittaa edeltävät tehtävät ohjelmallisesti.

###<a name="conditions"></a>Ehdot

Ehto on TOSI tai EPÄTOSI (totuusarvo) lauseke, joka sisältää vähintään yhden predikaatit.
Tässä esimerkissä predikaatin pienempi tai yhtä suuri kuin käytetään summa- ja käytettävissä varoja. Tämä ehto arvioi aina true tai false.
Predikaatit voidaan yhdistää loogisilla operaattoreilla AND, OR niin, ETTEIVÄT ne muodostavat looginen lauseke, joka on mahdollisesti suuria, mutta se aina arvo on TOSI tai EPÄTOSI.

###<a name="actions"></a>Toiminnot

Toiminnot ovat ehdon arviointi toimintojen vaikutukset. Jos sääntöehto täyttyy, myös vastaavan toiminnon tai toimintojen aloitetaan.
Tässä esimerkissä "järjestäminen tapahtuman" ja "Tulosta vastaanotto" on toimintoja, jotka suoritetaan, kun ja vain silloin, kun (Tässä tapauksessa "Jos summa on pienempi tai yhtä käytettävissä varoihin"), jos ehto on TOSI.
Liiketoiminnan sääntöjen puitteissa edustaa toiminnot joukko toimintoja XML-tiedostoja.

##<a name="policy"></a>Käytäntö

Käytäntö on looginen ryhmittely säännöt. Voit luoda käytännön, tallentaa sen, testaa se ja, kun olet tyytyväinen, käyttää sitä tuotantoympäristössä.

###<a name="policy-composition"></a>Käytännön yhdistelmä

Voit kirjoittaa käytännöt säännöt-portaalissa. Käytännön voi sisältää satunnaisesti suuri sääntöryhmän, mutta yleensä kun kirjoitat käytäntö säännöt, jotka koskevat yrityksen toimialueen sovellus, joka käyttää käytännön kontekstissa.

###<a name="policy-testing"></a>Käytännön testaaminen
Voit suorittaa kulku käytäntöön tehokkaasti ennen sen käyttämistä tuotantoympäristössä. Säännöt-portaalin avulla voit antaa syötteiden käytännön, suorita käytännön ja tarkastella sen Tulosta. Tulos on lokit, säännön suorittaminen, ehdon arviointi, ja tuloksena tulostaa.

##<a name="sample-scenario---insurance-claims"></a>Esimerkkitilanne - vakuutussaatavien
Otetaan esimerkkitilanne ja käydä läpi sen, kun olemme laatia liiketoimintalogiikan samaan.

![Vaihtoehtoinen teksti][1]

Hakijan lähettää really simple vakuutussaatavien skenaariossa hänen Selainyhteensopivuusongelmia (joko mikä tahansa asiakas, kuten sivuston, phone App). Tämä vaatimus pyytää saa lähetetty yrityksen varaa käsittelyn yksikkö ja käsittelyn tuloksen perusteella, vaatimus voi olla joko hyväksytty hylätty tai lähettää pitkin manuaalinen käsittelyä varten.
Varaa käsittelyn yksikkö Microsoftin skenaarion olisi osana järjestelmän liiketoimintalogiikan yksi. Yksikön tarkemmin ottaen näkyvissä seuraavasti:

![Vaihtoehtoinen teksti][2]

Anna meidän nyt Business sääntöjen avulla Toteuta tämän liiketoimintalogiikan.


##<a name="creation-of-rules-api-app"></a>Sääntöjen Api-sovelluksen luominen


1. Kirjaudu sisään Azure-portaaliin
2. Valitse -> Uusi Marketplace sitten *BizTalk säännöt* etsiminen
3. Valitse tulosluettelon BizTalk säännöt. BizTalk säännöt-sivu avautuu
4. Valitse *Luo* -painike ![vaihtoehtoinen teksti][3]
1. Uusi sivu, joka avautuu Anna seuraavat tiedot:  
    1. Nimi – antaa säännöt API sovelluksen nimi
    1. Sovelluksen palvelusopimus – Valitse tai Luo uusi sovellus-palvelu-suunnitelma
    1. Hinnoittelua taso – Valitse haluamasi tämä sovellus sijaitsevat hinnoittelu taso
    1. Resurssiryhmä – Valitse tai luo resurssiryhmä, jossa sovellus olisi sijaitsevat
    2. Tilaus – Valitse tilaus, jota haluat käyttää
    1. Sijainti – Valitse maantieteellisen sijainnin, jossa haluat ottaa käyttöön sovelluksen.
4.  Valitse *Luo*. Muutaman minuutin kuluessa BizTalk säännöt API sovelluksen luodaan.

##<a name="vocabulary-creation"></a>Sanaston luominen
Kun olet luonut BizTalk säännöt API-sovelluksen, seuraava vaihe on vocabularies luomiseen. Se, että kehittäjä on yleisimpiä henkilön on aikaisemmin tämän Harjoitus. Näin saat tämän valmis:


1. Oman BizTalk säännöt API App valitsemalla Selaa-portaalista käynnistys > API sovellukset - ><Your Rules API App>. Näin pääset avulla säännöt API sovelluksen koontinäyttö oheisen:

   ![Vaihtoehtoinen teksti][4]

2. Valitse "Sanaston määritelmät". Tämä näyttää, sanaston yhtä aikaa muiden kanssa näytön 3 valitsemalla "Lisää" Aloita uusi sanasto-määritysten lisääminen.
Sanaston määritelmät tällä hetkellä tueta – literaalimerkit sekä XML ovat 2.

##<a name="literal-definition"></a>Literal määritys
1.  Sen jälkeen valitsemalla "Lisää" uusi "Lisää määritys"-sivu avautuu. Seuraavat arvot
  1.    Nimi – vain aakkosnumeerisia merkkejä ovat oikein ilman muita erikoismerkkejä. Tämä myös on oltava yksilöivä otsikko aiemmin sanaston määritelmä luetteloon.
  2.    Kuvaus: valinnainen kenttä.
  3.    Tyypin – on 2 tuetut tietotyypit. Valitse tässä esimerkissä päivämääräliteraali
  4.    Tietotyyppi – tämä avulla käyttäjät voivat määrityksen tietotyyppi. Tällä hetkellä tueta 4 tietotyyppejä: voin.  Merkkijono – nämä arvot on kirjoitettava lainausmerkkeihin ("esimerkki merkkijonon")  
    II. Boolean – Tämä voi olla joko TOSI tai EPÄTOSI  
    III.    Numero – voi olla mikä tahansa desimaaliluvuksi  
    IV. DateTime – Tämä tarkoittaa, että tyyppi päivämäärätyyppi on def. Tiedot on kirjoitettava muodossa – KK/PP/VVVV HH AM\PM  
  5. Kirjoita – tällä Jos kirjoitat arvon määrityksen. Tähän kohtaan lisätty arvojen on oltava valitsemasi tietotyypin. Voit joko kirjoittaa yksittäinen arvo, pilkuilla tai arvoalue-avainsana *,*arvojoukon. Jos esimerkiksi kirjoitat yksilöllinen arvo 1. joukon 1; 2; 3; tai alueen 1-5. Huomaa, että alueella on tuettu vain lukuja.
  6. Valitse *OK*.

![Vaihtoehtoinen teksti][5]
##<a name="xml-definition"></a>XML-määritys
Jos sanasto tyyppi on valittu XML-tiedostona, seuraavat syötteiden on määritettävä  
  a.    Rakenteen – Clicking tästä avautuu uusi sivu salliminen käyttäjä joko valittavana ladattuja malleja tai sallimalla Lataa uusi luettelo.
b.    XPATH – tämä syöte saa lukitus vain, kun olet valinnut mallin edellisessä vaiheessa. Valitsemalla tämän näkyy rakenne, joka on valittuna ja antaa käyttäjälle oikeuden, jonka sanaston määritelmä on luotava solmu.  
  c-näppäinyhdistelmää.    KERTOMA – tämän syötteen tunnistaa tietojen objektissa fed säännöt-moduulin käsittelyä varten. Tämä on Lisäasetukset-ominaisuus ja oletusarvoisesti on määritetty ylätason valittu XPath-lauseke. KERTOMA on erityisen tärkeää ketjutus ja sivustokokoelman skenaarioita.

![Vaihtoehtoinen teksti][6]

### <a name="add-bulk"></a>Lisää samalla kertaa
Edellä kuvatut toimet on siepata käyttökokemusta sanaston määritelmien luominen. Luomiasi luotu sanaston määritelmät näkyvät lomake. Lisäksi edellytetään voivat luoda useita määrityksiä samaan rakenteeseen peräkkäisten edellä kuvatut toimet yksittäisen aina sijaan. Tämä on missä lisääminen joukkona ominaisuuksien on erittäin hyödyllinen.
Valitsemalla "Lisää käyttäjäjoukko" vievät sinut uusi sivu. Ensimmäiseksi on malli, jonka useita määrityksiä luotavien. Valitsemalla tämän avautuu uusi sivu, voit joko valitseminen luettelosta ladattuja rakenteiden hyväksyminen tai Lataa uusi salliminen.
Nyt XPath-POLUT-ominaisuuden saa lukitus. Valitsemalla tämän Avaa rakenteen tarkastelu, joissa voidaan valita useita solmujen.
Nimeä valittu solmu oletusarvo on luonut useita määrityksiä nimet. Nämä voi aina muokata luonnin jälkeen.

![Vaihtoehtoinen teksti][7]

##<a name="policy-creation"></a>Käytännön luominen
Kun kehittäjä on luonut tarvittavat vocabularies, sen on yritysanalyytikko olisi yksi luominen Business-käytäntöjä, Azure-portaalin kautta.  
    1. Valitse säännöt-sovellus on luotu on käytännön linssin napsauttamalla, jossa käyttäjä voi siirtyä käytännön luominen-sivulle.  
    2. Tämä sivu näkyy tietyn säännöt sovelluksen on käytäntöjen luettelon. Määrityksen, voit lisätä uuden käytännön käytännön nimeä ja pallolla välilehti kahdesti. Useita käytäntöjä voi olla yksittäinen säännöt API-sovelluksessa.  
    3. valitsemalla luotu käytäntö otetaan käytännön tiedot-sivulla käyttäjän missä jokin näkee säännöt, jotka ovat käytännön.  
    ![Vaihtoehtoisen tekstin][8]
 4.  Valitse "Lisää", jos haluat lisätä uuden säännön. Tällöin näyttöön uusi sivu.

##<a name="rule-creation"></a>Säännön luominen
Sääntö on kokoelma ehto ja toiminto lauseita. Toiminnot suoritetaan, jos ehto on TOSI. Anna Luo sääntö-sivu (ja kyseisen käytännön) yksilöivä säännön nimi ja kuvaus (valinnainen).
Ehto (Jos)-ruutuun voidaan luoda monimutkaisia ehdollinen lauseita. Seuraavassa on lueteltu tuetut avainsanoja:  
1.  Ja – ehdollinen operaattori  
2.  Tai – ehdollinen operaattori  
3.  onko\_ei\_olemassa  
4.  on olemassa  
5.  EPÄTOSI  
6.  on\_yhtä\_avulla  
7.  on\_suurempi\_kuin  
8.  on\_suurempi\_kuin\_yhtä\_avulla  
9.  on\_:  
10. on\_vähemmän\_kuin  
11. on\_vähemmän\_kuin\_yhtä\_avulla  
12. on\_ei\_:  
13. on\_ei\_yhtä\_avulla  
14. Mod  
15. TOSI  

Action(THEN)-ruutuun voi olla useita lauseita, omille riveilleen, voit luoda toiminnot, jotka suoritetaan. Seuraavassa on lueteltu tuetut avainsanoja:  
1.  yhtä suuri kuin  
2.  EPÄTOSI  
3.  TOSI  
4.  Pysäytä  
5.  Mod  
6.  Null  
7.  päivittäminen  

Ehto ja toiminto niiden sisältävät Intellisense auttaa säännön luominen nopeasti. Näin voit käynnistää osumalla ctrl + VÄLINÄPPÄIN tai vain vaihtoehtoisesti kirjoittaa. Avainsanojen vastaavat merkkien automaattisesti suodatettujen alas ja näytetään. Intellisense-ikkunassa näkyvät kaikki avainsanoja ja sanaston määritelmät.
![Vaihtoehtoinen teksti][9]

##<a name="explicit-forward-chaining"></a>Eksplisiittinen ketjutus eteenpäin
BizTalk säännöt tukee eksplisiittinen ketjutus eteenpäin, jos käyttäjät haluat arvioida uudelleen sääntöjä vastaukseksi tiettyjen toimintojen, ne käynnistäminen tämä käyttämällä tiettyjä avainsanoja. Tuetut avainsanat ovat seuraavat:  
   1.   Päivitä <vocabulary definition> – avainsanan arvioi uudelleen kaikki säännöt, jotka käyttävät määritettyä sanaston määritelmän kunto.  
   2.   Pysäyttää – avainsanan lopettaa kaikki säännön suorituskerran

##<a name="enabledisable-rules"></a>Enable\Disable säännöt
Käytännön sääntöä voi ottaa käyttöön tai poistaa käytöstä. Oletusarvon mukaan kaikki säännöt otetaan käyttöön. Käytöstä poistetut säännöt ei voi suorittaa käytännön suorituksen aikana. Enable\Disable säännöt voidaan toteuttaa joko säännön sivu suoraan – komennot ovat käytettävissä sivu yläreunassa olevia painikkeita tai käytännöstä-pikavalikon (hiiren kakkospainikkeella säännön) on enable\disable asetuksen.

##<a name="rule-priority"></a>Säännön prioriteetti
Käytännön säännöt suoritetaan järjestyksessä. Siinä järjestyksessä, jossa ne sijaitsevat käytäntö määritetään suorittamisen prioriteetti. Tämä prioriteetti voidaan muuttaa vetämällä ja pudottamalla säännön yksinkertaisesti.

##<a name="test-policy"></a>Testaa käytäntö
Voit testata oman käytännöt Testaa käytännön sivu "Testaa käytännön"-komennolla. Tämä sivu näet sanaston määritykset, joita käytetään käytäntö, jotka edellyttävät käyttäjän syötteiden luettelo. Käyttäjät voivat lisätä manuaalisesti näiden syötteiden niiden testi skenaarion arvot. Voit vaihtoehtoisesti myös käyttäjät voivat tuoda testata XMLs syötteiden. Kun kaikkien syötteiden ovat, testi voidaan suorittaa ja tulostaa jokaisen sanaston määrityksen näkyvät esitystapa-sarakkeen vertailun helpottamiseksi. Voit tarkastella yritysanalyytikko helpossa muodossa lokit, valitsemalla "Näytä lokit" tarkastella suorittamisen lokeja. Lokien tallentaminen "Tallenna tulosteen"-vaihtoehto on käytettävissä tallentaa kaikki testata riippumattoman analyysin liittyvät tiedot.

## <a name="using-rules-in-logic-apps"></a>Logiikan sovellusten sääntöjen avulla
Kun käytäntö on luotu ja testaa, on nyt valmis käytettäväksi. Voit luoda uuden logiikan sovelluksen valitsemalla logiikan sovellukset portaalin kotisivulle vasemmalla puolella. Kun logiikan-sovellus on luotu, käynnistä se ja valitse sitten *Käynnistimet ja toiminnot*. Voit valita mallin *luominen alusta alkaen* . Ohjeiden lisääminen BizTalk säännöt API sovelluksen logiikan-sovellukseen. Tämän jälkeen voit valita mitkä säännöt API App (toiminto) kohteeseen vaihtoehto ole. Toiminnot ovat käytäntöjä, jotka suoritetaan luettelo. Valitse tietyn käytäntö, minkä jälkeen syötteiden vaaditaan käytäntö on annettava. Käyttäjät voivat käyttää myötävirtaan säännöt-Ohjelmointirajapinnan sovelluksen tulostus edelleen päätöksentekoa.

## <a name="using-rules-via-apis"></a>Sääntöjen kautta ohjelmointirajapinnan käyttäminen
Sääntöjen API-sovellus voi käynnistää myös ohjelmointirajapinnan monipuoliset avulla. Tätä tapaa, jolla käyttäjät eivät ole rajoittanut vain sovelluksilla logiikan ja käyttää sääntöjä minkä tahansa sovelluksen tekemällä REST-puhelut. Käytettävissä olevat tarkka REST API voi tarkastella napsauttamalla "API Definiton" linssin säännöt raporttinäkymät-ikkunassa.

![Vaihtoehtoinen teksti][10]

Seuraavassa on esimerkki siitä, miten jokin voi käyttää tätä API c#

            // Constructing the JSON message to use in API call to Rules API App

            // xmlInstance is the XML message instance to be passed as input to our Policy
            string xmlInstance = "<ns0:Patient xmlns:ns0=\"http://tempuri.org/XMLSchema.xsd\">  <ns0:Name>Name_0</ns0:Name>  <ns0:Email>Email_0</ns0:Email>  <ns0:PatientID>PatientID_0</ns0:PatientID>  <ns0:Age>10.4</ns0:Age>  <ns0:Claim>    <ns0:ClaimDate>2012-05-31T13:20:00.000-05:00</ns0:ClaimDate>    <ns0:ClaimID>10</ns0:ClaimID>    <ns0:TreatmentID>12</ns0:TreatmentID>    <ns0:ClaimAmount>10000.0</ns0:ClaimAmount>    <ns0:ClaimStatus>ClaimStatus_0</ns0:ClaimStatus>    <ns0:ClaimStatusReason>ClaimStatusReason_0</ns0:ClaimStatusReason>  </ns0:Claim></ns0:Patient>";

            JObject input = new JObject();

            // The JSON object is to be of form {"<XMLSchemName>_<RootNodeName>":"<XML Instance String>".
            // In the below case, we are using XML Schema - "insruanceclaimsschema" and the root node is "Patient".
            // This is CASE SENSITIVE.
            input.Add("insuranceclaimschema_Patient", xmlInstance);
            string stringContent = JsonConvert.SerializeObject(input);


            // Making REST call to Rules API App
            HttpClient httpClient = new HttpClient();

            // The url is the Host URL of the Rules API App
            httpClient.BaseAddress = new Uri("https://rulesservice77492755b7b54c3f9e1df8ba0b065dc6.azurewebsites.net/");
            HttpContent httpContent = new StringContent(stringContent);
            httpContent.Headers.ContentType = MediaTypeHeaderValue.Parse("application/json");

            // Invoking API "Execute" on policy "InsruranceClaimPolicy" and getting response JSON object. The url can be gotten from the API Definition Lens
            var postReponse = httpClient.PostAsync("api/Policies/InsuranceClaimPolicy?comp=Execute", httpContent).Result;

Huomaa, että edellä säännöt API-sovellus on määritetty "Julkisen Anon" suojausasetuksia. Näin voit määrittää API-sovelluksen käyttäminen - kaikki asetukset -> Sovelluksen asetukset -> käyttöoikeustaso.

![Vaihtoehtoinen teksti][11]

## <a name="editing-vocabulary-and-policy"></a>Sanaston ja käytännön muokkaaminen
Yksi Business sääntöjen avulla eduista on, että muutokset liiketoimintalogiikan voidaan miten tuotannon on usein nopeampaa. Tuotannon mukautuu automaattisesti sanasto ja käytännöistä tehtyjä muutoksia. Käyttäjän on yksinkertaisesti vastaaviin sanaston määritystä tai käytännön selaamalla ja muutat sen tulee voimaan.

<!--Image references-->
[1]: ./media/app-service-logic-use-biztalk-rules/InsuranceScenario.PNG
[2]: ./media/app-service-logic-use-biztalk-rules/InsuranceBusinessLogic.png
[3]: ./media/app-service-logic-use-biztalk-rules/CreateRuleApiApp.png
[4]: ./media/app-service-logic-use-biztalk-rules/RulesDashboard.png
[5]: ./media/app-service-logic-use-biztalk-rules/LiteralVocab.PNG
[6]: ./media/app-service-logic-use-biztalk-rules/XMLVocab.PNG
[7]: ./media/app-service-logic-use-biztalk-rules/BulkAdd.PNG
[8]: ./media/app-service-logic-use-biztalk-rules/PolicyList.PNG
[9]: ./media/app-service-logic-use-biztalk-rules/RuleCreate.PNG
[10]: ./media/app-service-logic-use-biztalk-rules/APIDef.PNG
[11]: ./media/app-service-logic-use-biztalk-rules/PublicAnon.PNG
