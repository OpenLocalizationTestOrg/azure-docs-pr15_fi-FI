<properties
   pageTitle="Tietojen tuominen Azure tapahtuman keskittimet SQL tiedot | Microsoft Azure"
   description="Yleistä tapahtuman keskittimet tuominen SQL-Esimerkki"
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

# <a name="pulling-data-from-sql-into-an-azure-event-hub"></a>Hakemalla tietoja SQL Azure-tapahtuman keskittimeen

Tyypillinen arkkitehtuuri käsittelyyn reaaliaikaiset tiedot sovelluksen kuuluu valitseminen se ensin Azure-tapahtumaa-toiminnossa. Lähettää IoT-skenaario, kuten yleisille, eri osuuksilla tietoliikenteestä seurantaa tai gaming tilanne, seuranta, frenzied contestants valtavasti toiminnot tai enterprise-skenaario, kuten seuranta rakennuksen käyttöönottoon asti. Tällaisissa tapauksissa tietojen tuottajat ovat yleensä ulkoisia objekteja, jotka tuottavat aikasarjalle tiedot, jotka haluat kerätä, analysoida, tallentaa ja act yhteydessä ja voivat on sijoitettu työmäärään paljon huomioon nämä prosessit infrastruktuuri, rakennus. Mitä Haluatko tehdä, jos haluat tuoda tietoja jotain, kuten lähteen streaming data sijaan tietokannan, ja käyttää sitä yhdessä muiden streaming tiedot? Tilanne, johon haluat Azure Stream Analytics, Remote Data Explorer (RDX) tai jokin muu työkalu avulla voit analysoida ja toimia hitaasti muuttaminen tietojen KIELIASETUS tai mukautetun tilat hallintajärjestelmän? Voit ratkaista tämän ongelman syy on kirjoitettu ja avaa Orpotermit pieni cloud otoksen, joita voit muokata ja ottaa käyttöön, joka noutaa tietoja SQL-taulukosta ja push Azure tapahtuman keskittimeen käyttämään syötteeksi edeltävät analytical-sovelluksissa. Huomaat, että se harvinaisissa tilanne ja päinvastainen toiminto kuin tavallisesti toiminnot ja tapahtumaa-toiminnossa. Jos tiedä tilanteessa, jos se on sinun on suoritettava, voit etsiä koodi Azure näytteiden valikoimassa [tähän](https://azure.microsoft.com/documentation/samples/event-hubs-dotnet-import-from-sql/).  

Huomaa, että koodi on tässä esimerkissä on vain tämän otoksen. Se on **ole** tarkoitettu tuotannon-sovelluksen ja ei ole yritykset on tehty, jotta se olisi sovellu järjestelmän ympäristöön - DIY-stricly on developer keskittyvässä, esimerkiksi. Tarvitset kaikenlaisia suojaus, suorituskyvyn, toiminnot ja kustannusten tekijät ennen lopusta loppuun-arkkitehtuuri-selvitetään.

## <a name="application-structure"></a>Sovelluksen rakenne

Sovellus on kirjoitettu C# ja otoksessa readme.md-tiedosto sisältää kaikki tiedot, joita haluat muokata, luoda ja julkaista sovellus. Seuraavissa osissa on korkean tason yleiskatsaus sovelluksen mitä.

Aluksi olettaen, että sinulla on pääsy SQL Azure-taulukosta. Voit myös on määrittänyt Azure-tapahtumaa-toiminnossa ja tiedät yhteyden merkkijonon, jota voit käyttää sitä tarvitaan.

Kun SqlToEventHub-ratkaisun käynnistyy, lukee kokoonpanotiedosto (App.config) useista syistä, saat kuvatulla tavalla readme.md-tiedosto. Nämä ovat melko selkeitä, kuten nimi arvotaulukko jne ja ei tarvitse rehash tähän selitykset. 

Kun sovellus on luku määritystiedoston, se siirtyy kyselyjä muodostaa silmukan lukeminen SQL-taulukon ja tietueiden valitseminen tapahtuma-keskittimeen ja valitse odottaa käyttäjän määrittämä nukkumaanmenoaikojen ajan ennen kuin teet sen alusta uudelleen. Muutama seikka on otettava huomioon:

1. Sovelluksen perustuu olettaen, että SQL-taulukkoon päivitetään joitakin ulkoisen prosessin ja haluat lähettää kaikki vain tapahtumaa-toiminnossa päivitykset.
2. SQL-taulukon on oltava yksilöllinen ja kasvava numero, esimerkiksi tietueen mainokset sisältävän kentän. Tämä voi olla pelkästään "Tunnus"-nimisen kentän tai muu kuin riippumatta siitä, mitä päivittää tietokannan suurenee, lisää tietueita, kuten "Creation_time" tai "Sequence_number". Sovelluksen muistiinpanoja ja tallentaa iteraation kentän arvon. Kunkin kautta tapahtuu seuraavien kerralla sovelluksen kyselyt olennaisesti taulukon kaikki tietueet, kun kyseisen kentän arvo on suurempi kuin arvo se tuli viimeisen silmukassa. Olemme soitat "siirtymä" tämän viimeisen arvon.
3. Sovelluksen luo taulukon "TableOffsets" käynnistyksen yhteydessä, siirtymät tallentamiseen. Taulukko luodaan "CreateOffsetTableQuery" määritetty määritystiedostossa kyselyn kanssa. 
4. On useita käsittelyyn offset-taulukko, "OffsetQuery", "UpdateOffsetQuery" ja "InsertOffsetQuery" määritystiedostossa määritellään käytetyt kyselyt. Älä muuta nämä.
5. -Kyselyn "DataQuery" määritetty määritystiedostossa on kysely, joka suoritetaan hakemaan SQL-taulukon tietueita. Siinä on tällä hetkellä vain kunkin vaiheen kautta optimointiin - silmukan ensimmäiset 1 000 tietueisiin Jos esimerkiksi 25,000 tietueet on lisätty tietokannan viimeisen kyselyn jälkeen voi kestää hetken kyselyn suorittaminen. Rajoittamalla 1 000 tietueet kyselyn aina, kun kyselyt on paljon aiempaa nopeammin. Peräkkäiset erissä 1 000 tietueiden valitseminen ensimmäiset 1 000 yksinkertainen Vie tapahtuma-keskittimeen.    

## <a name="next-steps"></a>Seuraavat vaiheet

Ottamaan ratkaisun Kloonaa tai Lataa SqlToEventHub-sovelluksen, Muokkaa App.config-tiedostoa, luoda ja lopuksi julkaisemaan. Kun olet julkaissut sovellus, se on kohdassa pilvipalveluihin Azure perinteinen-portaalissa ja seurata saapua tapahtuman pääsivuston tapahtumia. Huomaa, että taajuus määritetään kahden asian mukaan: päivitysväli SQL-taulukon ja olet määrittänyt sovelluksen määritystiedostossa nukkumaanmenoaikojen aikaväli.