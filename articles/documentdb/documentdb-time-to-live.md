<properties
   pageTitle="Päättyy DocumentDB tietojen kanssa oletusaika | Microsoft Azure"
   description="TTL-Microsoft Azure DocumentDB avulla voit automaattisesti tyhjentää järjestelmästä tietyn ajan kuluttua asiakirjat."
   services="documentdb"
   documentationCenter=""
   keywords="oletusaika"
   authors="kiratp"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="documentdb"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/28/2016"
   ms.author="kipandya"/>

# <a name="expire-data-in-documentdb-collections-automatically-with-time-to-live"></a>Voimassa tietojen DocumentDB sivustokokoelmat automaattisesti oletusaika kanssa

Sovellusten voi tuottaa ja tallentaa suuria määriä tietoja. Osa tiedoista, kuten tietokoneen luotu tapahtuman tiedot, lokit ja käyttäjän istunnon tiedot on hyödyllinen vain rajallinen ajan kuluessa. Kun tiedot on ylimääräinen sovelluksen tarpeisiin on turvallista Tyhjennä tiedoista ja vähentää sovelluksen tallennustilan tarpeisiin.

"Aika Live" tai TTL Microsoft Azure DocumentDB tarjoaa mahdollisuuden asiakirjat automaattisesti tyhjentää tietokannasta tietyn ajan kuluttua. Oletusarvoinen time to live voidaan määrittää sivustokokoelman tasolla ja ohittaa asiakirjan kohti välein. Kun TTL on määritetty, sivustokokoelman oletusarvoisesti tai asiakirjan tasolla DocumentDB poistetaan automaattisesti tiedostot, jotka ole kuluttua sekunteina, koska ne on viimeksi muokattu.

Oletusaika DocumentDB käyttää siirtymä vastaan, kun asiakirja on viimeksi muokattu. Voit tehdä tämän se käyttää _ts kentäksi, jota on olemassa kaikista asiakirjoista. _Ts-kenttä on unix-pohjaiset kausi aikaleima edustava päivämäärä ja kellonaika. _Ts-kenttä päivittyy aina, kun asiakirjaa muutetaan. 

## <a name="ttl-behavior"></a>TTL toiminta

TTL-ominaisuus ohjataan kahdella tasolla - sivustokokoelman tasolla ja asiakirjan tason TTL-ominaisuudet. Arvot määritetään sekunteina ja käsitellään sama.arvo-_ts, asiakirja on viimeksi muokattu.

 1.  DefaultTTL kokoelman
  * Jos puuttuu (tai arvo on null)-tiedostoja ei poisteta automaattisesti.
  
  * Jos nykyinen ja arvo on-"1" = äärettömän – tiedostojen voimassa oletusarvon mukaan
  
  * Jos nykyinen ja arvo on jokin numero ("n") – asiakirjojen vanhenemista "n" sekunnin kuluttua edellisen muokkauksen

 2.  Asiakirjojen TTL: 
  * Ominaisuus on käytettävissä vain, jos DefaultTTL ei sisällä tietoja pääkohde-kokoelmaan.
  
  * Ohittaa ylemmän tason kokoelmassa DefaultTTL arvo.

Heti, kun asiakirja on päättynyt (ttl + _ts > = nykyisen palvelimen kellon), asiakirja on merkitty "vanhentunut". Ei ole toiminnon salliminen näiden tiedostojen määräajan jälkeen ja niiden tulosten minkä tahansa suorittaa jätetään pois. Asiakirjojen poistetaan fyysisesti järjestelmässä ja poistetaan taustalla tilanteen voi tehdä myöhemmin. Tämä ei käytä mitä tahansa sivustokokoelman budjetti- [Pyytää yksiköt (RUs)](documentdb-request-units.md) .

Yllä logiikan voidaan näyttää seuraavat matriisissa:

|       | DefaultTTL kokoelma asettaa puuttuu tai ei | DefaultTTL =-1 käyttöön sivustokokoelmassa | DefaultTTL = "o" käyttöön sivustokokoelmassa|
| ------------- |:-------------|:-------------|:-------------|
| Tiedostoa ei löydy TTL| Ei mitään korvaavat asiakirjan tasolla, koska asiakirjan ja sivustokokoelman ei TTL-käsite. | Tässä kokoelmassa asiakirjoja ei päättyy. | Tämän sivustokokoelman tiedostoissa vanhenee väli n kuluu. |
| TTL =-1-asiakirjaan | Ei mitään korvaavat asiakirjan tasolla, koska kokoelman ei määritä DefaultTTL-ominaisuus, joka ohittaa asiakirjan. Asiakirjan TTL on yhdistelmävisualisoinnin tulkittava järjestelmä. | Tässä kokoelmassa asiakirjoja ei päättyy. | TTL =-1 Tässä kokoelmassa tiedoston koskaan päättyy. Kaikki muut asiakirjat vanhentuu "o" aikaväli. |
|  TTL = n tiedostoa | Ei mitään korvaavat asiakirjan tasolla. Asiakirjan TTL yhdistelmävisualisoinnin tulkittava järjestelmä. | Tiedoston TTL = n vanhentuu väli n sekunnin kuluttua. Muiden asiakirjojen tulee perivät väli 1 ja aina voimassa. | Tiedoston TTL = n vanhentuu väli n sekunnin kuluttua. Muiden asiakirjojen perivät "o" väli-valikoimasta. |


## <a name="configuring-ttl"></a>TTL määrittäminen

Oletusarvon mukaan oletusaika on poissa käytöstä oletusarvoisesti kaikki DocumentDB sivustokokoelmat ja kaikki tiedostot.

## <a name="enabling-ttl"></a>TTL ottaminen käyttöön

Haluat TTL-kokoelma tai kokoelma tiedostojen käyttöön kokoelman DefaultTTL-ominaisuuden arvoksi 1 tai muuta kuin nolla positiivinen luku. Asettaminen DefaultTTL 1 tarkoittaa, oletusarvoisen sivustokokoelman kaikki asiakirjat live jatkuvasti, mutta DocumentDB palvelun pitäisi seurata tämän sivustokokoelman tiedostoille, jotka on ohitettu oletusarvoa.

## <a name="configuring-default-ttl-on-a-collection"></a>Määrittäminen oletusarvoista TTL-kokoelma

Olet voi määrittää oletusaika Live sivustokokoelman tasolla. 

Määritä TTL kokoelma, haluat säätää nollasta positiivinen luku, joka ilmaisee ajanjakson sekunteina voimassa asiakirjan (_ts) jälkeen viimeksi muokattua aikaleimaa sivustokokoelman kaikki asiakirjat.

Tai voit määrittää oletusarvon 1, joka tarkoittaa, että kaikki tiedostot, jotka on lisätty kokoelmaan live toistaiseksi oletusarvoisesti.

## <a name="setting-ttl-on-a-document"></a>Asiakirjan TTL-asetusta

Määrittämisen oletusarvoista TTL-kokoelma lisäksi voit määrittää tietyn TTL asiakirjan tasolla. Näin ohittaa kokoelman oletusarvo.

Määritä TTL asiakirjan, haluat säätää nollasta positiivinen luku, joka ilmaisee ajanjakson sekunteina asiakirja vanhenee viimeksi muokattua aikaleimaa asiakirjan (_ts).

Voit määrittää tämän määräajan siirtymä Määritä asiakirjan TTL-kenttä.

Jos asiakirjaa ei ole TTL-kenttää, oletusarvo kokoelman käytetään.

Jos Elinaika on poistettu käytöstä sivustokokoelman tasolla, TTL-kentässä, ohitetaan, kunnes TTL on otettu käyttöön kokoelman uudelleen.


## <a name="extending-ttl-on-an-existing-document"></a>Valitse aiemmin luotu tiedosto TTL laajentaminen

Voit palauttaa TTL asiakirjan tekemällä minkä tahansa kirjoitus asiakirjan. Näin määrität _ts nykyinen aika ja asiakirjan määräajan, kuten ttl-on määritetty ajastus alkaa uudelleen.

Jos haluat muuttaa tiedoston ttl, voit päivittää kentän asetettavan kentän tapaan.


## <a name="removing-ttl-from-a-document"></a>TTL poistaminen asiakirjasta

Jos et halua enää voimassa asiakirjan Arvoksi on määritetty asiakirjan, valitse voit hakea asiakirjan, poista TTL-kenttä ja korvata asiakirjan palvelimeen.

Kun TTL-kenttä poistetaan asiakirjasta, otetaan käyttöön kokoelman oletusarvo.

Voit lopettaa asiakirjan päättyvän ja käytetä valikoimasta sitten haluat määrittää TTL-arvoa 1.


## <a name="disabling-ttl"></a>TTL-asetuksen poistaminen käytöstä

Täysin on kokoelma TTL poistaminen käytöstä ja pysäyttää tausta-Etsitkö kokoelman DefaultTTL-ominaisuus on poistettava vanhentuneet asiakirjat.

Jos tämä ominaisuus on erilainen kuin määrittäminen 1. 1 tarkoittaa uusia asiakirjoja lisätään kokoelmaan-asetukseksi live jatkuvasti, mutta voit ohittaa tiettyjen tiedostojen kokoelma.

Tämä ominaisuus poistetaan kokonaan kokoelmasta tarkoittaa, että asiakirjoja ei päättyy, vaikka on otettava huomioon tiedostot, jotka on nimenomaisesti ohittaa aiemman oletusarvon.


## <a name="faq"></a>USEIN KYSYTYT KYSYMYKSET

**Mikä TTL maksaa minulle?**

Ei, asetus TTL asiakirjan ilman lisäkustannuksia.

**Kuinka kauan poistaa asiakirjan, kun TTL on määrittäminen kestää?**

Asiakirjat on merkitty ei käytettävissä, kun asiakirja on päättynyt (ttl + _ts > = nykyisen palvelimen kellon). Ei ole toiminnon salliminen näiden tiedostojen määräajan jälkeen ja niiden tulosten minkä tahansa suorittaa jätetään pois. Asiakirjojen poistetaan fyysisesti järjestelmä taustalla. Tämä ei käyttää mitä tahansa RUs eikä kokoelman budjetista.

**Jos kestää tietyn ajanjakson aikana, voit poistaa tiedostoja, on ne laskea kohti Omat kiintiön (ja tuli.), kunnes ne poisteta?**

Ei, kun asiakirja on päättynyt sinua ei laskuteta tiedostot säilyttämiseen ja tiedostojen kokoa Laske kokoelma tallennuskiintiön kohti.

**Vaikuttaa asiakirjan TTL mitään vaikutusta RU ovat?**

Ei, ei ole vaikutusta RU kulujen minkä tahansa asiakirjan sisällä DocumentDB suorittaa toimintoja varten.

**Tulee tiedostojen vaikutus minulla on valmisteltu Omat sivustokokoelman siirtonopeuden poistetaan?**

Ei, että tarjoillaan kokoelmaa pyynnöt saavat prioriteettia päälle suorittaminen taustalla tiedostojen poistaminen. TTL lisääminen minkä tahansa tiedoston eivät vaikuta tämä.

**Kun asiakirja vanhenee, kuinka kauan se säilyy oma kokoelma, kunnes se poistetaan?**

Kun tiedosto vanhenee se on käytettävissä. Täsmällisen kellonajan asiakirjan säilyy kokoelmaa ennen todella poistamisesta on ei-deterministic ja perustuu kun tausta on voi poistaa asiakirjan.

**Vanhentuneet asiakirjat poistetaan kaikki solmut yli tai onko se "ilmestyy consistent?"**

Tiedosto ei ole käytettävissä samanaikaisesti kaikissa solmuissa ja kaikilla alueilla.

**Onko RU-kustannukset TTL seuranta taustan tehtävien?**

Ei, ei ole RU hinta.

**Kuinka usein TTL erääntymisten tarkistetaan?**

Tarkistuksen TTL erääntymisten ei tapahdu, taustalla. Pyyntö vastattaessa Taustajärjestelmä-palvelu Tee tarkistukset tekstiin ja jättää pois tiedostot, jotka ovat vanhentuneet. Fyysinen asiakirjan poisto on vain prosessi, joka suoritetaan asynkronisesti taustalla. Tämä toimenpide taajuus määräytyy sivustokokoelman käytettävissä RUs.

**TTL-toiminto vain koske koko asiakirjoja tai voin päättyy yksittäisen asiakirjan ominaisuusarvoihin?**

TTL koskee koko asiakirja. Jos haluat voimassa vain asiakirjan osa, sitten on suositeltavaa, että voit poimia pääasiakirjaan-osan toiseen "linkitetty" tiedostoon ja käyttää TTL poimitun tiedostoa.

**Onko TTL-ominaisuus tietyn indeksoinnin vaatimuksia?**

Kyllä. Kokoelma on oltava viive tai Consistent [indeksoinnin käytännön määrittäminen](documentdb-indexing-policies.md) . Yrittää määrittää DefaultTTL kokoelma, jossa indeksointi ei mitään määrittäminen aiheuttaa virheen, kun se yrittää käytöstä indeksointi käyttöön sivustokokoelmassa, jossa on DefaultTTL, joka on jo määritetty.


## <a name="next-steps"></a>Seuraavat vaiheet

Saat lisätietoja Azure DocumentDB viittaavat palvelun [*asiakirjat*](https://azure.microsoft.com/documentation/services/documentdb/) -sivu.




