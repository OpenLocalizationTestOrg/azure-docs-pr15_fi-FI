<properties 
    pageTitle="Tavallisia toimintoja koneen Learning suositukset-Ohjelmointirajapinnan | Microsoft Azure" 
    description="Azure ML suositus sovelluksen malli" 
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


# <a name="recommendations-api-sample-application-walkthrough"></a>Suosituksia API otoksen sovelluksen vaiheittainen kuvaus

>[AZURE.NOTE]Käynnistä suosituksia API kognitiiviset Service-Esityspalvelua käyttämällä tämän version sijaan. Kognitiiviset suositukset-palvelu korvaaminen palvelua ja kaikkien uusien ominaisuuksien kehittämisestä siellä. Siinä on uusia ominaisuuksia, kuten jonottaminen tuki, paremmin API Explorer, selkeämpi API pinta, yhdenmukaisempi kirjautuminen/Laskutus kokemus ja niin edelleen.
> Lisätietoja [uuden kognitiiviset palvelun siirtyminen](http://aka.ms/recomigrate)

##<a name="purpose"></a>Tarkoitus

Tässä asiakirjassa näkyy [esimerkkisovelluksen](https://code.msdn.microsoft.com/Recommendations-144df403)kautta Azure koneen Learning suosituksia Ohjelmointirajapinnan käyttö.

Tätä sovellusta ei ole tarkoitus sisällyttää täysi toiminnallisuus eikä käyttää kaikkien API. Se esitellään joitakin yleisiä toimintoja suorittamaan, kun haluat ensin toistaa koneen Learning suositus-palvelussa. 

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

##<a name="introduction-to-machine-learning-recommendation-service"></a>Johdanto koneen Learning suosituspalvelu

Suosituksia koneen Learning suosittelupalvelun kautta ovat käytössä, kun luot suositus mallin perusteella seuraavat tiedot:

* Säilön kohteiden haluat suositella tunnetaan myös nimellä luetteloon
* Tietoja, joka edustaa kohdetta kussakin käyttäjän tai istunnon (tämän voi hankkia ajan kuluessa kautta hankintaa, malli-sovelluksen osana) käyttö

Kun suositus mallin muodostanut, voit käyttää sitä ennustaa kohteet, jotka käyttäjä voi olla kiinnostuneita, joukon kohteiden (tai yksittäisen kohteen) mukaan käyttäjä valitsee.

Jotta edellisen tilanteen, tietokoneen Learning suosittelupalvelun seuraavasti:

* Mallin luominen: Tämä on looginen säilö, joka sisältää tiedot (luettelo ja käyttö) ja ennakoiva tekstinsyöttö mallit. Kunkin mallin säilön tunnistetaan yksilöllisen tunnuksen, joka on varattu, kun se luodaan kautta. Tämä tunnus kutsutaan Mallitunnus, ja niitä käytetään useimmat API. 
* Lataa luetteloon: mallin säilön luomisen jälkeen voit yhdistää siihen luetteloon.

**Huomautus**: mallin luominen ja luettelon lataaminen on yleensä suoritetaan kerran mallin elinkaari varten.

* Lataa käyttö: Tämä lisää käyttötiedot mallin säilö.
* Suositus mallin luominen: kun sinulla on riittävästi tietoja, voit luoda suositus-malli. Tämä toiminto käyttää ylimmän kone Learning algoritmit suositus mallin luominen. Jokaisen muodosta liittyy yksilöllinen tunnus. Sinun täytyy tallentaa tunnus, koska se on tarpeen joitakin ohjelmointirajapinnan toimintoja.
* Valvoa rakennuksen prosessi: suositus-mallin luominen on asynkroninen toiminto ja voi kestää useita minuutteja-useita tunteja riippuen määrä (luettelo ja käyttö) ja muodosta parametrit. Tämän vuoksi haluat valvoa Luo. Suositus mallin luodaan vain, jos sen liittyvät muodosta on valmis.
* (Valinnainen) Valitse aktiivinen suositus-mallin muodosta: Tämä vaihe on vain tarpeen, jos sinulla on useampi kuin yksi suositus mallin valmis malli-säilö. Saat suosituksia ilman ilmaisee, että aktiivinen suositus mallin pyyntö ohjataan automaattisesti oletusarvon aktiivinen Kehitä järjestelmä. 

**Huomautus**: aktiivinen suositus-malli on valmis tuotannon ja se on muodostettu tuotannon kuormituksen. Tämä eroaa-aktiivinen suositus mallin, joka pysyy testi kaltaisessa ympäristössä (kutsutaan joskus väliaikaisen).

* Nouda suositukset: Kun käytössäsi on suositus, voit käynnistää suosituksia yksittäiseen kohteeseen tai kohteet, jotka olet valinnut luettelo. 

Voit käynnistää Hae suositus yleensä tietyn ajanjakson aikana. Kyseisen ajanjakson aikana Voit uudelleenohjata käyttötiedot koneen Learning suositus järjestelmään, mikä lisää nämä tiedot määritetyn mallin säilö. Jos sinulla on tarpeeksi käyttötietoja, voit luoda uuden mallin suositus, joka kattaa ylimääräisten tietojen. 

##<a name="prerequisites"></a>Edellytykset

* Visual Studio 2013
* Internet-yhteys 
* Tilauksen suositukset API (https://datamarket.azure.com/dataset/amla/recommendations).

##<a name="azure-machine-learning-sample-app-solution"></a>Azure koneen Learning otoksen sovellusratkaisuksi

Tämä ratkaisu sisältää lähdekoodin, malli, luettelo ja direktiivejä ladata kääntämistä varten tarvittavat paketit.

##<a name="the-apis-used"></a>Käyttää API

Sovellus käyttää tietokoneen Learning suositus toimintoja alijoukkoa käytettävissä ohjelmointirajapinnan kautta. Seuraavat ohjelmointirajapinnan on osoittanut sovelluksessa:

* Luo malli: Luo loogisen säilön säilyttämään tietojen ja suositus mallit. Mallin tunnistetaan nimi ja useita malleja voi luoda saman niminen.
* Lataa luettelotiedosto: ladata luettelotiedot.
* Lataa tiedosto käyttö: ladata käyttötiedot.
* Muodosta käynnistäminen: Luo suositus mallin avulla.
* Muodosta suorittamisen valvonta: avulla voit valvoa suositus-mallin muodosta tila.
* Valitse valmiita malli on suositus: avulla, mitkä suositus, jota haluat käyttää oletusarvoisesti tiettyjä mallin säilön. Tämä vaihe on tarpeen vain, jos sinulla on useampi kuin yksi suositus mallin ja haluat aktivoida-aktiivinen-muodosta aktiivinen suositus mallina.
* Hae suositus: avulla voit hakea tietyn yksittäiseen kohteeseen tai joukko kohteita mukaan suositellut kohteet. 

Katso täydellinen kuvaus API Microsoft Azure Marketplace-ohjeista. 

**Huomautus**: mallin voi olla useita versiot ajan kuluessa (samanaikaisesti). Jokaisen muodosta luodaan sama tai päivitetty luettelon ja muita käyttötiedot.

## <a name="common-pitfalls"></a>Yleisiä käsitellään

* Sinun on annettava käyttäjänimi ja Microsoft Azure Marketplacesta ensisijainen tiliavain suorittaa otoksen sovelluksen.
* Esimerkki sovelluksen käynnissä peräkkäin epäonnistuu. Sovelluksen työnkulku sisältää luomisesta, lataamisesta, rakentaminen näyttö ja suositusten saaminen esimääritettyjä; vuoksi se epäonnistuu peräkkäiset suorittamisen, jos et muuta välillä mallinimi.
* Suosituksia saattaa tuottaa ilman tietoja. Esimerkki sovellus käyttää vähän luettelon ja käyttö-tiedostoa. Tämän vuoksi ja luettelon kohteita on suositeltu kohteita ei ole.

## <a name="disclaimer"></a>POISSULKEMINEN
Esimerkki sovellus ei ole tarkoitettu tuotantoympäristössä suorittamiseen. Luettelon tiedot on hyvin pieni, ja siinä ei ole kuvaava suositus mallin. Tiedot on taulukkona osoitus. 
 
