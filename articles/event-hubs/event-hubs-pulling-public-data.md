<properties
    pageTitle="Tietojen tuominen Azure tapahtuman keskittimet julkisten tietojen | Microsoft Azure"
    description="Yleistä tapahtuman keskittimet tuominen web-malli"
    services="event-hubs"
    documentationCenter="na"
    authors="spyrossak"
    manager="timlt"
    editor=""/>

<tags 
    ms.service="event-hubs"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/25/2016"
    ms.author="spyros;sethm" />

# <a name="pulling-public-data-into-azure-event-hubs"></a>Julkisten tietojen tietojen tuominen Azure tapahtuman keskittimet

Tyypillinen asioita Internet (IoT) skenaarioissa on laitteita, että olet ohjelma push tietoihin Azure-joko Azure-tapahtumaa-toiminnossa tai IoT-toiminnossa. Näiden keskittimet ehdot pikakuvakkeet kyselyjä Azure tallentaminen, analysointia ja kesäolympialaisten visualisointi runsaasti Työkalut Microsoft Azure saataville kanssa. Kuitenkin ne edellyttävät push tietojen niihin muotoiltu JSON ja suojattu tietyllä tavalla. Tämä näyttää seuraavaan kysymykseen. Mitä teen, jos haluat tuoda tiedot julkinen tai yksityinen lähteestä, jossa tiedot näkyvät web-palvelu tai syötteen tarkoitus, mutta et voi muuttaa, kuinka tiedot on julkaistu? Harkitse sää tai liikenne tai osakkeen tarjoukset - ei tiedä NOAA, tai WSDOT tai NASDAQ määrittäminen push tapahtuma-keskittimeen. Voit ratkaista tämän ongelman syy on kirjoitettu ja avaa Orpotermit pieni cloud otoksen, joita voit muokata ja ottaa käyttöön, joka noutaa tiedot jostakin lähteestä ja siirtää tapahtumaa-toiminnossa. Sieltä voit tehdä, joista haluat, aihe, jos haluat käyttöoikeussopimuksen ehdot-tuottaja. Löydät sovelluksen [tähän](https://azure.microsoft.com/documentation/samples/event-hubs-dotnet-importfromweb/).

Huomautus koodi on tässä esimerkissä näkyy vain voit hakea tietoja tyypillinen web syötteitä ja viestin kirjoittaminen Azure-tapahtumaa-toiminnossa. Tämä ei ole tarkoitus olla tuotannon-sovellus ja ei ole yritykset on tehty, jotta se olisi sovellu järjestelmän ympäristöön - strictfly DIY-developer keskittyvässä Esimerkki vain on. Lisäksi, onko tässä esimerkissä ei olisi **salaus puretaan** kyselyjä Azure **push** sijaan suositus tantamount to sitä. Tarkista suojaus, suorituskyvyn, toiminnot ja kustannusten tekijät ennen selvitetään lopusta loppuun-arkkitehtuuri.

## <a name="application-structure"></a>Sovelluksen rakenne

Sovellus on kirjoitettu C# ja [otoksen kuvaus](https://azure.microsoft.com/documentation/samples/event-hubs-dotnet-importfromweb/) sisältää kaikki tiedot, joita haluat muokata, luoda ja julkaista sovellus. Seuraavissa osissa on yleiskatsaus sovelluksen mitä.

Aluksi olettaen, että sinulla on pääsy tietosyötteestä. Voit esimerkiksi haluta hakemaan Washingtonin osavaltion osaston, kuljetus liikennetiedot tai tiedot-NOAA näytettävä mukautettuja raportteja tai yhdistää tiedot sovelluksen muiden tietojen kanssa. Voit myös on määrittänyt Azure-tapahtumaa-toiminnossa ja tiedät yhteyden merkkijonon, jota voit käyttää sitä tarvitaan.

Kun GenericWebToEH-ratkaisun käynnistyy, se lukee kokoonpanotiedosto (App.config) Saat useista syistä:

1. URL-Osoitteen tai URL-osoitteet, luettelo sivuston julkaisemalla tiedot. Ihannetapauksessa tämä on sivusto, joka julkaisee tietoja JSON, kuten viittauksia WSDOT [tähän](http://www.wsdot.wa.gov/Traffic/api/). 
2. URL-osoite, tarvittaessa tunnistetiedot. Monta julkiset lähteet eivät edellytä tunnistetietoja, tai voit sijoittaa tunnistetiedot URL-merkkijono. Muiden edellyttää, että annat erikseen. (Huomaa, että voit määrittää vain tunnistetietoja sovelluksessa, niin se toimii vain, jos vain yksi URL-osoite, eivät URL-osoitteet).
3. Yhteysmerkkijonon ja kyseisen tapahtuman keskittimet nimitilan, johon painat tiedot tapahtumaa-toiminnossa nimi. Löydät nämä tiedot Azure-portaalissa.
4. Nukkumaanmenoaikojen aikaväli millisekunteina väli kyselyt julkisten tietojen-sivustossa. Jotkin haluat näyttää tämän määrittämistä edellyttää. Kyselyn liian harvoin voi jäädä huomaamatta tiedot. toisaalta, jos kyselyn liian usein, näkyviin voi tulla toistuvien tietojen tarkastelun tai huonoin vielä, jonka voi olla estetty nefarious robotti nimellä. Harkitse tietolähteen päivittämisväliä - sää tai liikenne tietoja voi päivittää 15 minuutin välein, mutta stock tarjoukset ehkä muutaman sekunnin välein, riippuen siitä, mistä ne. 
5. Onko sovellus, onko tiedot on tulossa JSON tai XML merkinnän. Jälkeen, kun haluat siirtää tietoja tapahtumaa-toiminnossa, sovellus on moduuli XML muuntaa JSON ennen lähettämistä.

Luettuasi määritystiedosto sovelluksen siirtyy silmukka - käyttäminen julkista sivustoa, muunnetaan tietoja tarvittaessa kirjoittaminen tapahtuma-keskittimeen ja odottaa nukkumaanmenoaikojen ajan ennen kuin teet sen alusta uudelleen. Tarkemmin:

  * Luettaessa julkisen verkkosivuston. Vastaanottava valmis Lähetä tietojen Azure/GenericWebToEH/ApiReaders/RawXMLWithHeaderToJsonReader.cs käytetään RawXMLWithHeaderToJsonReader luokan esiintymä. Lukee lähde stream GetData()-menetelmällä ja jakaa sen käyttämällä GetXmlFromOriginalText pienempi kappaletta (eli tietueita). 
  Tämä menetelmä lukee XML sekä muotoiltu JSON tai JSON matriisi. Valitse käsittely on käynnistetty MergeToXML kokoonpano App.config (oletus = tyhjä).
  * Vastaanotto- ja lähettää tiedot on toteutettu silmukan Program.cs Process()-menetelmää. 
  Saatuaan Tulosta GetData() menetelmä enqueues erotetut arvot tapahtuma-keskittimeen.

## <a name="next-steps"></a>Seuraavat vaiheet

Ottamaan ratkaisun Kloonaa tai Lataa [GenericWebToEH](https://azure.microsoft.com/documentation/samples/event-hubs-dotnet-importfromweb/) -sovelluksen, Muokkaa App.config-tiedostoa, luoda ja lopuksi julkaisemaan. Kun olet julkaissut sovellus, näet sen toiminnassa Azure perinteinen portaalissa Cloud Services-kohdassa ja voit muuttaa joitakin asetukset (kuten tapahtumaa-toiminnossa kohde ja nukkumaanmenoaikojen väli) **Määritä** -välilehdessä.

On useita tapahtuman keskittimet esimerkkejä [Azure näytteiden valikoima](https://azure.microsoft.com/documentation/samples/?service=event-hubs) ja [MSDN](https://code.msdn.microsoft.com/site/search?query=event%20hubs&f%5B0%5D.Value=event%20hubs&f%5B0%5D.Type=SearchText&ac=5)-sivuston.
