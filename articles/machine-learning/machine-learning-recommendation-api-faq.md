<properties 
    pageTitle="Määrittäminen ja käyttäminen koneen Learning suositukset-Ohjelmointirajapinnan | Microsoft Azure" 
    description="Microsoft SUOSITUKSIA API luotu Azure koneen Learning usein kysytyt kysymykset" 
    services="machine-learning" 
    documentationCenter="" 
    authors="LuisCabrer" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/08/2016" 
    ms.author="luisca"/> 

#<a name="setting-up-and-using-machine-learning-recommendations-api-faq"></a>Määrittämisestä ja käyttämisestä tietokoneen Learning suosituksia API usein kysytyt kysymykset


**Mikä on SUOSITUKSIA?**

>[AZURE.NOTE]Käynnistä suosituksia API kognitiiviset Service-Esityspalvelua käyttämällä tämän version sijaan. Kognitiiviset suositukset-palvelu korvaaminen palvelua ja kaikkien uusien ominaisuuksien kehittämisestä siellä. Siinä on uusia ominaisuuksia, kuten jonottaminen tuki, paremmin API Explorer, selkeämpi API pinta, yhdenmukaisempi kirjautuminen/Laskutus kokemus ja niin edelleen.
> Lisätietoja [uuden kognitiiviset palvelun siirtyminen](http://aka.ms/recomigrate)

Organisaatiot ja yritykset, jotka perustuvat rajat Tilausasiakas suositukset ja ylös myynti-tuotteiden ja palvelujen asiakkaille SUOSITUKSET Azure koneen Learning tarjoaa Omatoiminen suositukset-ohjelma. Toteutus yhteiskäytön suodattaminen, joka käyttää matriisin factorization sen core algoritmin on. Sovellusten kehittäjille käyttää SUOSITUKSIA REST API avulla. 

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

**Mitä voin tehdä suositusten?**

SUOSITUKSIA tulee syötteen kohdetta tai joukko kohteita, ja palauttaa suositusten luettelon. Esimerkki: online jälleenmyyjältä asiakkaalle napsauttaa tuote. Online jälleenmyyjältä lähettää tuotteen syötteeksi SUOSITUKSET, saa luettelon tilauksista, jotka ja päättää, mitä tuotteiden näkyy asiakkaan. Haluat ehkä käyttää SUOSITUKSIA optimointi verkkokauppaan tai jopa ilmoittaa, että sisäpuolelle myynnin osaston tai puhelun keskelle.

**Onko käyttö rajoitettu?**

Suosituksia on käyttö seuraavat rajoitukset:
* Mallien tilauskohtaisten enimmäismäärä: 10
* Kohteet, jotka voivat sisältää luettelon enimmäismäärä: 100,000
* Käyttö pisteet, joita pidetään enimmäismäärä on noin 5,000,000. Vanhin poistetaan, jos uusia ladattu tai raportoi.
* Tiedot, jotka voidaan lähettää sähköpostitse (esimerkiksi luettelon tietojen tuominen-valintaikkunassa Tuo käyttötiedot) enimmäiskoko on 200 Mt
* Suosituksia mallin muodosta, joka ei ole aktiivinen sekunnissa (TP merkinnät) tapahtumien määrä on noin 2 TP merkinnät. Suosituksia mallin muodosta, joka on aktiivinen voivat sisältää enintään 20 TP merkinnät.

##<a name="purchase-and-billing"></a>Osta ja laskutus 


**Kuinka paljon suosituksia kustannukset käynnistyksen aikana?**

Suosituksia on tilauspohjaista palvelu. Charging kuukaudessa tapahtumien määrän perusteella. Voit tarkistaa [tarjouksen sivu] (https://datamarket.azure.com/dataset/amla/recommendations)-hintatiedot Microsoft Azure Marketplacesta.

**Onko ottaa suosituksia liittyvät kustannukset seurata ja tallentaa käyttäjän minulle?**

Ei hetkellä.

**Onko suosituksia maksuttoman kokeiluversion käyttäjäksi?**

On ilmainen kirjausketju on rajoitettu 10 000 tapahtumat kuukaudessa.

**Kun voin laskutetaan suositukset?**

Maksetun tilauksen on jokin tilaus, jossa on kuukausittainen maksu. Kun ostat maksulliseen tilaukseen, heti perittävän ensimmäisenä kuukautena käyttöä varten. Peritään summa, joka on liitetty tarjous tilauksen sivulla (sekä verot). Tämän kuukauden maksu tehdään kuukausittain kalenterin alkuperäisen ostettu samana päivänä, ennen kuin peruutat tilauksen. 

**Miten päivitän korkeampi taso-palveluun?**

Voit ostaa tai päivittää tilauksen [tarjouksen sivu] (https://datamarket.azure.com/dataset/amla/recommendations)-sivulla Microsoft Azure Marketplacesta.

Kun päivität tilauksen:

* Tapahtumat, jotka ovat jäljellä olevan vanhan tilauksen ei lisätään uuteen tilaukseen. 
* Voit maksaa koko hinnan uuteen tilaukseen, vaikka käytössäsi käyttämättömät tapahtumat vanhan tilauksen.

Prosessi, jos haluat päivittää tilauksen:

* Nevigate [tarjouksen sivu] (https://datamarket.azure.com/dataset/amla/recommendations).
* Kirjaudu Marketplacesta Jos et ole jo kirjautunut sisään.
* Oikeanpuoleisessa ruudussa näkyvät kaikki käytettävissä olevat suunnitelmat. Valitse valintanappi, jonka haluat päivittää.
* Jos haluat päivittää, valitse **OK**. Jos et halua päivityksen, valitse **Peruuta**.

**Tärkeää** Lue valintaikkunan huolellisesti ennen päivitystä, koska laskutus- ja käytä vaikutukset.

**Kun suositukset tilaukseni päättyy?**

Tilauksesi päättyy, kun se peruutetaan. Jos haluat peruuttaa tilauksistasi, noudata seuraavia ohjeita.

**Miten suositukset tilaus peruutetaan?**

Jos haluat peruuttaa tilauksen, noudattamalla seuraavia ohjeita. Jos nykyisen tilauksesi on maksulliseen tilaukseen, tilauksen pysyy voimassa nykyisen laskutusjakson loppuun saakka. Jos tarvitset peruutus ovat tehokas heti, yhteystiedot, [Microsoft-tuen](https://support.microsoft.com/oas/default.aspx?gprid=17024&st=1&wfxredirect=1&sd=gn).

**Huomautus** Ei tukea annetaan, jos peruutat tilauksen ennen päättymispäivä laskutusjaksolla tai käyttämättömät tapahtumien laskutuksen kaudella.

* Siirry [tarjouksen sivu] (https://datamarket.azure.com/dataset/amla/recommendations).
* Kirjaudu Marketplace Jos et ole jo kirjautunut sisään.
* Valitse oikealla puolella tietojoukon nimi ja tila **Peruuta** . Voit käyttää tätä tilausta nykyisen laskutusjakson loppuun saakka tai tapahtuman enimmäismäärä on saavutettu (sen mukaan, kumpi toteutuu ensin).

Jos haluat peruuttaa tilauksen heti, joten voit Osta uusi tilaus, tiedosto on [Microsoft-tuen](https://support.microsoft.com/oas/default.aspx?gprid=17024&st=1&wfxredirect=1&sd=gn)lippu.

##<a name="getting-started-with-recommendations"></a>Suosituksia käytön aloittaminen

**Suosituksia on minulle?** 

Koneen Learning suosituksia on organisaatiot ja yritykset, jotka perustuvat rajat Tilausasiakas suositukset ja ylöspäin myynti-tuotteita ja palveluita asiakkaille. Jos sinulla on asiakkaan verkkosivustoon, myynnin voimassa, jonka myyjien tai puhelinkeskus, ja jos yli muutaman dozen tuotteita ja palveluita luettelo voi käyttää, alimman rivin voi hyötyä käyttämisestä suosituksia. 

Suosituksia kokeileminen on suunniteltu on melko yksinkertainen. Nykyisen API-pohjaisen version on oltava basic-ohjelmoinnin taidot. Jos tarvitset apua, ota myyjä, joka on kehittänyt sivustoon. Jos sinulla on sisäinen IT-osaston tai sisäiset kehittäjä, ne pitäisi Nouda suositukset auttaa sinua. 

**Mitkä ovat edellytyksistä suosituksia määrittäminen?**

Suosituksia edellyttää, että käyttäjän valintoja loki luettelon viittaavan. Jos et t on esimerkiksi lokia ja asiakkaan aukeaman sivusto on, suositukset voi kerätä käyttäjän puolestasi. 

Suosituksia edellyttää myös luettelo tuotteita ja palveluita. Jos et t on luettelon, suositukset, voit käyttää varsinainen asiakas-käyttötietoja ja distill luetteloon. Epäsuora luettelo ei sisällä kohteet, jotka raportoidut ei osana käyttäjän tapahtumat.

**Miten määritän suosituksia ensimmäistä kertaa?**

Kun [tilaamisen] (https://datamarket.azure.com/dataset/amla/recommendations) suositukset, avulla API-ohjeissa [Azure koneen Learning suosituksia pikaopas](machine-learning-recommendation-api-quick-start-guide.md) palvelun määrittäminen.

**Mistä löydän API asiakirjat?** 

API-ohjeissa on [Azure koneen Learning suosituksia pikaopas](machine-learning-recommendation-api-quick-start-guide.md).

**Asetuksista tarvitsen luettelo-ja käyttötiedot lataaminen suosituksia?**

Käytettävissä on kaksi vaihtoehtoista ladataan luettelon ja käyttö tietojen: Voit viedä tiedot CRM-järjestelmän tai muiden lokit ja lataa se suosituksia tai voit lisätä sivustoon, joka seuraa käyttäjän toiminnot tunnisteet. Jos käytät jälkimmäisessä tapaa, tiedot tallennetaan Azure-tietokannassa.

##<a name="maintenance-and-support"></a>Ylläpito ja tuki

**Kuinka suuria Omat tietojoukon voidaan?**

Kunkin tietojoukon voi sisältää enintään 100 000 luettelokohteet ja ylös käyttötiedot 2048 megatavua.
Lisäksi tilauksen voi olla enintään 10 tietojoukot (malleja).

**Mistä saan suosituksia teknistä tukea?**

Tekninen tuki on käytettävissä [Microsoft Azure tuki](https://social.msdn.microsoft.com/forums/azure/home?forum=MachineLearning) -sivustosta.

**Mistä löydän käyttöehdot?**

[Microsoft Azure Konepohjaisten Oppimistekniikoiden suosituksia API palveluehdot](https://datamarket.azure.com/dataset/amla/recommendations#terms).



 
