<properties 
    pageTitle="Luo Ohjelmointirajapinnan logiikan sovellukset" 
    description="Mukautetun Ohjelmointirajapinnan käyttäminen logiikan sovellusten luominen" 
    authors="jeffhollan" 
    manager="dwrede" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na" 
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="jehollan"/>
    
# <a name="creating-a-custom-api-to-use-with-logic-apps"></a>Mukautetun Ohjelmointirajapinnan käyttäminen logiikan sovellusten luominen

Jos haluat laajentaa logiikan sovellukset-ympäristö, sinun on useilla eri tavoilla voit soittaa ohjelmointirajapinnan tai järjestelmiin, jotka eivät ole käytettävissä yhtenä Microsoftin monta ulos,-valmiilla yhdistimiä.  Yksi näistä tavoista API-sovelluksen, joihin voi soittaa logiikan App työnkulun luominen.

## <a name="helpful-tools"></a>Hyödyllisiä työkaluja

API toimii parhaiten logiikan sovelluksilla Suosittelemme luodaan [swagger](http://swagger.io) asiakirjaan, joka käsittelee oman API tuetut toiminnot ja parametrit.  On useita kirjastoja (kuten [Swashbuckle](https://github.com/domaindrivendev/Swashbuckle)), joka luo automaattisesti swagger puolestasi.  Voit käyttää myös [TRex](https://github.com/nihaue/TRex) avulla huomautuksia swagger toimimaan hyvin logiikan Apps (Näytä nimet, ominaisuuden tyypit jne.).  Joitakin esimerkkejä laadittuihin ratkaisuihin logiikan sovellusten API-sovellukset muista kuitata ulos meidän [GitHub säilöön](http://github.com/logicappsio) tai [blogiin](http://aka.ms/logicappsblog).

## <a name="actions"></a>Toiminnot

Logiikan sovelluksen basic-toiminto on ohjauskoneen, joka hyväksyy HTTP-pyynnön ja palauttaa vastaus (yleensä 200).  Mutta liittämisasetuksia on erimuotoisten vaiheittaisten ohjeiden avulla voit laajentaa tarvittavat toiminnot.

Oletusarvoisesti logiikan App ohjelma etsii aikakatkaisu pyyntö 1 minuutin kuluttua.  Kuitenkaan voi olla oman API suorittaa toimintoja, jotka kestää kauemmin ja että sinulla on suoritettu, odota seuraamalla alla mainitut asynkroninen- tai webhook kuvio ohjelma.

Vakio toimintojen kirjoittaa HTTP-pyyntömenetelmä vain oman API, joka on määritetty swagger kautta.  Näet esimerkkejä API-sovellukset, jotka toimivat Microsoftin [GitHub säilöön](https://github.com/logicappsio)logiikan sovellukset.  Seuraavassa on yleisiä kuvioita mukautetun yhdistimen seuraavista tavoista.

### <a name="long-running-actions---async-pattern"></a>Pitkä käynnissä olevat toiminnot - asynkroninen kuvio

On pitkä vaihe tai tehtävä, sinun on suoritettava ensimmäinen vaihe on Varmista, että moduulin tietää et vielä aikakatkaisu. Sinun on myös yhteydenpito moduulin, miten se tietää, kun olet valmis, kun tehtävä ja lopuksi haluat palata moduulin tarvittavat tiedot, jotta voit jatkaa työnkulun. Voit suorittaa, API-Liittymän kautta toimimalla alla työnkulku. Piste-,-näkymästä mukautetun Ohjelmointirajapinnan on seuraavasti:

1. Kun pyyntö on vastaanotettu, Palauta vastaus heti (ennen määritettyä työtä). Tämä vastaus on `202 ACCEPTED` vastauksen, auttaa muita tietää olet saanut tietoja, ohjelma ei hyväksytä paketti, ja nyt käsitellään. 202 vastauksen pitäisi olla seuraavat otsikot: 
 * `location`otsikko (pakollinen): Tämä on absoluuttisen polku URL-Osoitteen logiikan-sovellusten avulla voit tarkistaa työn tila.
 * `retry-after`(valinnainen, oletusarvo on 20 toiminnot tulevat). Tämä on sekunteina moduulin odottaa, ennen kuin kysely tilan tarkistaminen sijainnin otsikon URL-osoite.

2. Kun tila on valittuna, suorita seuraavat tarkistukset: 
 * Jos työ on valmis: palauttaa `200 OK` vastauksen, vastaus-paketti.
 * Jos projektin käsittelee yhä: palauttaa toiseen `202 ACCEPTED` vastaus samaan ylätunnisteet vastauksesta kuin kanssa

Tämän mallin avulla voit suorittaa erittäin pitkä tehtäviä oman mukautetun API viestiketjussa, mutta säilyttää aktiivisen yhteyden toiminnassa logiikan sovellukset-ohjelma niin, että se ei aikakatkaisu tai Jatka ennen valmiina. Kun lisäät logiikan Appiin, on tärkeää muistaa, sinun ei tarvitse tehdä mitään sen Siirry äänestyksen järjestäminen ja tilan logiikan-sovelluksen. Moduulin näkee 202 hyväksytyt vastauksen kelvollinen sijainti-otsikkoon, kun se tukee asynkroninen kuvio ja jatkaa sijainti-otsikon lisääminen kunnes ei 202 palautetaan.

Näet GitHub tätä mallia näyte [tähän](https://github.com/jeffhollan/LogicAppsAsyncResponseSample)

### <a name="webhook-actions"></a>Webhook toiminnot

Aikana työnkulun voit määrittää logiikan sovelluksen keskeyttää ja odota, kunnes "takaisinkutsun" Jatka.  Tämän takaisinkutsun tulee HTTP POST muodossa.  Toteuttamisesta tätä mallia, sinun on annettava päätepisteet opaspainiketta: tilaa ja tilauksen peruuttaminen.

Valitse "tilata" logiikan-sovelluksen luominen ja voit rekisteröidä takaisinsoitto-URL-osoite, johon voidaan tallentaa oman API ja takaisinsoitto kuin HTTP-viesti on valmis.  Kaikki sisältö ja otsikot välitetään logiikan-sovellukseen ja jakojäännöksen, kun työnkulku voidaan käyttää.  Logiikan App ohjelma Soita tilaa-kohdan suorittamisen käyttöön heti, kun se käynnit vaiheessa.

Jos Suorita peruutettiin, logiikan App ohjelma tekee tilauksen' päätepisteen kutsu.  Oman API voit sitten unregister takaisinsoitto URL-osoite, tarpeen mukaan.

Logiikan App Designer ei tällä hetkellä tue etsiminen webhook-päätepisteen kautta swagger, jotta voit käyttää tämäntyyppistä toiminnon "Webhook"-toiminnon lisääminen ja määritä URL-osoite, otsikoiden ja pyyntö leipätekstiin.  Voit käyttää `@listCallbackUrl()` työnkulun funktion johonkin näistä kentistä tarvittaessa siirtää takaisinsoitto URL-osoite.

Näet GitHub webhook muodostaa kuvion näyte [tähän](https://github.com/jeffhollan/LogicAppTriggersExample/blob/master/LogicAppTriggers/Controllers/WebhookTriggerController.cs)

## <a name="triggers"></a>Käynnistimien

Lisäksi toiminnot tietokoneeseen voi asentaa mukautetun API-act käynnistimen logiikan-sovellukseen.  On kaksi kuviot, voit noudattaa alla käynnistettävän logiikan sovelluksen:

### <a name="polling-triggers"></a>Kysely Käynnistimet

Kysely käynnistimien toimii samankaltaisesti kuin yllä pitkä käynnissä asynkroninen toiminnot.  Logiikan App ohjelma soittaa käynnistimen päätepisteen tietyn ajan kuluttua (määräytyy tuote-15 sekunnin Premium 1 minuutti vakio- ja vapaa 1 tunti).

Jos sijaintitietoja ei ole käytettävissä, käynnistin palauttaa `202 ACCEPTED` vastauksen, kanssa `location` ja `retry-after` otsikko.  Kuitenkin käynnistimien on suositeltavaa, `location` otsikko sisältää kyselyparametri `triggerState`.  Tämä on joitakin tunnus oman API tietää, kun viimeksi logiikan sovellus käynnistyy.  Jos sijaintitietoja ei käytettävissä, käynnistin palauttaa `200 OK` vastauksen sisällön tiedot.  Tämä Kyselysäännön logiikan-sovellus.

Esimerkiksi jos minulla on kysely nähdäksesi, jos tiedosto on käytettävissä voit luoda kysely käynnistimen, joka seuraavasti:

* Jos pyyntö on vastaanotettu ja ei ole triggerState Ohjelmointirajapinnan palauttavat `202 ACCEPTED` kanssa `location` otsikon, joka on triggerState nykyinen aika ja `retry-after` 15.
* Jos pyyntö on vastaanotettu triggerState kanssa:
 * Tarkista, jos tiedostoista, jotka on lisätty triggerState DateTime jälkeen. 
  * Jos on 1 tiedosto `200 OK` vastauksen sisällön tunnistukseen kasvattaa tiedoston palautettu ja määritä päivämäärä ja aika, triggerState `retry-after` 15.
  * Jos määritettynä on useita tiedostoja, voin palauttaa 1 kerrallaan, `200 OK`, kasvattaa Omat triggerState- `location` ylä- ja määritä `retry-after` 0.  Tämän avulla ohjelma tietää enemmän tietoja on käytettävissä ja sitä heti pyytää kerralla `location` määritetty otsikko.
  * Jos käytettävissä ei ole tiedostoja, Palauta `202 ACCEPTED` vastauksen ja jätä `location` triggerState sama.  Määritä `retry-after` 15.

Näet kysely käynnistimen-GitHub näyte [tähän](https://github.com/jeffhollan/LogicAppTriggersExample/tree/master/LogicAppTriggers)

### <a name="webhook-triggers"></a>Webhook Käynnistimet

Webhook käynnistimien toimii samankaltaisesti kuin yllä Webhook toiminnot.  Logiikan App ohjelma soittaa tilaa' päätepisteen aina, kun webhook käynnistin lisätään ja tallentaa.  Oman API voit rekisteröidä webhook URL-osoite ja kutsu sitä kautta HTTP POST aina, kun tiedot ovat käytettävissä.  Sisällön paketti ja otsikot välitetään Suorita logiikan sovellukseen.

Jos webhook käynnistimen koskaan poistetaan (joko logiikan sovellus kokonaan tai vain webhook käynnistin)-moduulin soittaminen tilauksen"URL-osoitteen kohtaa, johon voit unregister oman API takaisinsoitto URL-osoite ja lopettaa prosessit tarpeen mukaan.

Logiikan App Designer ei tällä hetkellä tueta etsiminen webhook käynnistimen kautta swagger, jotta tällaista-toiminnon käyttäminen lisää "Webhook" käynnistin ja määritä URL-osoite, otsikoiden ja pyyntö tekstiosaan.  Voit käyttää `@listCallbackUrl()` työnkulun funktion johonkin näistä kentistä tarvittaessa siirtää takaisinsoitto URL-osoite.

Näet webhook käynnistimen-GitHub näyte [tähän](https://github.com/jeffhollan/LogicAppTriggersExample/tree/master/LogicAppTriggers)