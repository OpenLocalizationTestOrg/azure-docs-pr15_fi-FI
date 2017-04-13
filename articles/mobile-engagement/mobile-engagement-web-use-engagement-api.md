<properties
    pageTitle="Azure Mobile välitys Web SDK-ohjelmointirajapinnan | Microsoft Azure"
    description="Uusimmat päivitykset ja Azure Mobile välitys Web SDK ohjeet"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="web"
    ms.devlang="js"
    ms.topic="article"
    ms.date="06/07/2016"
    ms.author="piyushjo" />

# <a name="use-the-azure-mobile-engagement-api-in-a-web-application"></a>Web-sovelluksen Azure Mobile välitys Ohjelmointirajapinnan käyttäminen

Tässä asiakirjassa on lisäys asiakirja, jossa kerrotaan, miten voit [integroida Mobile välitys WWW-sovelluksessa](mobile-engagement-web-integrate-engagement.md). Se sisältää Azure Mobile välitys Ohjelmointirajapinnan käyttäminen ja raportoi sovellusta tilastotiedot tarkempia tietoja.

Mobile välitys Ohjelmointirajapinnan tarjoaa `engagement.agent` objekti. Azure Mobile välitys Web SDK alias on oletusarvoinen `engagement`. Voit määrittää tämän tunnuksen SDK-määrityksestä uudelleen.

## <a name="mobile-engagement-concepts"></a>Mobile välitys käsitteitä

Seuraavien osien Tarkenna yhteiset [Mobile välitys käsitteitä](mobile-engagement-concepts.md) web-ympäristössä.

### <a name="session-and-activity"></a>`Session`ja`Activity`

Jos käyttäjä pysyy käyttämättömänä yli muutaman sekunnin kaksi toimien välillä, toimenpiteiden järjestys käyttäjän on jaettu kaksi eri istuntoa. Nämä hetkinen kutsutaan istunnon aikakatkaisun.

Jos web-sovelluksen ei määritellä käyttäjän toiminnot loppuun erillisenä (soittamalla `engagement.agent.endActivity` funktion), Mobile välitys palvelimen vanhenee automaattisesti käyttäjäistunto kolme minuuttia sen jälkeen, kun sovellus-sivulla on suljettu. Tätä kutsutaan palvelin istunnon aikakatkaisu.

### `Crash`

Automaattinen raporttien aiheutetaan JavaScript-poikkeusten luodaan oletusarvon mukaan. Voit kuitenkin raportoida kaatuu manuaalisesti käyttämällä `sendCrash` toimi (Lisätietoja on osassa raportoinnin kaatuu).

## <a name="reporting-activities"></a>Raportoinnin toiminnot

Käyttäjän toimintojen raportoinut sisältää kun käyttäjä aloittaa uuden tehtävän ja käyttäjän päättyessä valitun tehtävän.

### <a name="user-starts-a-new-activity"></a>Käyttäjä aloittaa uusi tehtävä

    engagement.agent.startActivity("MyUserActivity");

Soita `startActivity()` kerran kunkin käyttäjän toiminnan muutokset. Tämän funktion ensimmäinen kutsu aloittaa uuden käyttäjäistunnon.

### <a name="user-ends-the-current-activity"></a>Käyttäjän päättyy nykyisen tehtävän

    engagement.agent.endActivity();

Soita `endActivity()` vähintään kerran, kun käyttäjä loppuu viimeisen toimintaansa. Näin tiedät Mobile välitys Web SDK-paketissa, että käyttäjä ei tällä hetkellä käyttämättömänä sekä käyttäjäistunto on suljettava istunnon aikakatkaisun päätyttyä. Jos soitat `startActivity()` istunnon aikakatkaisun vanhenee, istunnon yksinkertaisesti jatketaan.

Koska ei ole luotettavaa puhelun kun etsintäikkuna on suljettu, on usein vaikeaa tai mahdotonta todellisen käyttäjän toimintoja Verkkoympäristössä sisällä loppuun. Minkä vuoksi Mobile välitys palvelimen vanhenee automaattisesti käyttäjäistunto kolme minuuttia sen jälkeen, kun sovellus-sivulla on suljettu.

## <a name="reporting-events"></a>Tapahtumien raportointi

Tapahtumien raportoinut kattaa istunnon tapahtuma- ja erillinen tapahtumat.

### <a name="session-events"></a>Istunnon tapahtumat

Istunnon tapahtumat käytetään yleensä ilmoittamaan käyttäjän istunnon aikana käyttäjän toimintoja.

**Esimerkki ilman ylimääräisiä tietoja:**

    loginButton.onclick = function() {
      engagement.agent.sendSessionEvent('login');
      // [...]
    }

**Esimerkki, jossa lisätiedot:**

    loginButton.onclick = function() {
      engagement.agent.sendSessionEvent('login', {user: 'alice'});
      // [...]
    }

### <a name="standalone-events"></a>Erillisestä tapahtumat

Toisin kuin istunnon tapahtumat erillinen tapahtumat voi ilmetä muualla kuin istunnon.

Käytä siis ``engagement.agent.sendEvent`` sijaan ``engagement.agent.sendSessionEvent``.

## <a name="reporting-errors"></a>Virheiden raportoiminen

Virheet raportoinut kattaa istunnon virheet ja erillinen virheet.

### <a name="session-errors"></a>Istunnon virheet

Istunnon virheitä käytetään yleensä ilmoittamaan virheitä, jotka vaikuttavat käyttäjän käyttäjän istunnon aikana.

**Esimerkki ilman ylimääräisiä tietoja:**

    var validateForm = function() {
      // [...]
      if (password.length < 6) {
        engagement.agent.sendSessionError('password_too_short');
      }
      // [...]
    }

**Esimerkki, jossa lisätiedot:**

    var validateForm = function() {
      // [...]
      if (password.length < 6) {
        engagement.agent.sendSessionError('password_too_short', {length: 4});
      }
      // [...]
    }

### <a name="standalone-errors"></a>Erillisestä virheet

Toisin kuin istunnon virheet erillinen virheitä voi ilmetä muualla kuin istunnon.

Käytä siis `engagement.agent.sendError` sijaan `engagement.agent.sendSessionError`.

## <a name="reporting-jobs"></a>Töiden raportointi

Raportoinut työt kattaa virheistä ja tapahtumat, jotka esiintyvät työn ja raportointi kaatuu.

**Esimerkki:**

Jos haluat seurata AJAX-pyyntö, voit käyttää seuraavasti:

    // [...]
    xhr.onreadystatechange = function() {
      if (xhr.readyState == 4) {
      // [...]
        engagement.agent.endJob('publish');
      }
    }
    engagement.agent.startJob('publish');
    xhr.send();
    // [...]

### <a name="reporting-errors-during-a-job"></a>Virheiden raportoiminen projektin aikana

Virheet voi liittyä käyttäjäistunnon käynnissä työn sijaan.

**Esimerkki:**

Jos haluat ilmoittaa virheestä, jos AJAX-pyyntö epäonnistuu:

    // [...]
    xhr.onreadystatechange = function() {
      if (xhr.readyState == 4) {
        // [...]
        if (xhr.status == 0 || xhr.status >= 400) {
          engagement.agent.sendJobError('publish_xhr', 'publish', {status: xhr.status, statusText: xhr.statusText});
        }
        engagement.agent.endJob('publish');
      }
    }
    engagement.agent.startJob('publish');
    xhr.send();
    // [...]

### <a name="reporting-events-during-a-job"></a>Raportoinnin tapahtumia projektin aikana

Tapahtumien liitettävissä käynnissä työn sen sijaan, että käyttäjäistunnon tukijoillemme `engagement.agent.sendJobEvent` funktio.

Tämä toiminto toimii täsmälleen kuten `engagement.agent.sendJobError`.

### <a name="reporting-crashes"></a>Raportoinnin kaatuu

Käytä `sendCrash` -funktion avulla raportin kaatuu manuaalisesti.

`crashid` -Argumentti on merkkijono, joka määrittää kaatumisen.
`crash` -Argumentti on yleensä pinon jäljitys kaatumisen merkkijonona.

    engagement.agent.sendCrash(crashid, crash);

## <a name="extra-parameters"></a>Ylimääräisen parametrit

Voit liittää satunnaisia tietoja tapahtuman, virhe, tehtävän tai projektin.

Tietoja voi olla JSON-objektin (mutta ei matriisin tai yksinkertaisen tyypin).

**Esimerkki:**

    var extras = {"video_id": 123, "ref_click": "http://foobar.com/blog"};
    engagement.agent.sendEvent("video_clicked", extras);

### <a name="limits"></a>Rajoitukset

Näppäimet, arvon tyypeistä ja koon säännöllisiä lausekkeita alueilla koskevat rajoitukset, jotka koskevat ylimääräiset parametrit.

#### <a name="keys"></a>Näppäimet

Jokainen objektin avain on vastattava seuraavalla lausekkeella:

    ^[a-zA-Z][a-zA-Z_0-9]*

Tämä tarkoittaa, että näppäimet on alettava kirjaimella vähintään yksi perään kirjaimia, numeroita ja alaviivoja (\_).

#### <a name="values"></a>Arvot

Merkkijono, ja totuusarvon lajit kokorajoituksena arvot.

#### <a name="size"></a>Kokoa

Lisäominaisuudet kokorajoituksena puhelun 1 024 merkkiä (kun Mobile välitys Web SDK käyttää JSON-muodossa).

## <a name="reporting-application-information"></a>Sovelluksen raportointiin

Voit manuaalisesti raportoida seurannan tiedot (tai muita tietoja sovelluksen kielikohtaiset) avulla `sendAppInfo()` funktio.

Huomaa, että nämä tiedot voidaan lähettää soluissa. Tietyn laitteen säilytetään vain tietyn avaimen uusimman arvo.

Tapahtuman lisäominaisuudet, kuten abstrakti hakemuksen tiedot JSON-objektin avulla. Huomaa, että matriiseja tai aliraportti objektit käsitellään tasainen merkkijonot (joko JSON Sarjatoiminto).

**Esimerkki:**

Tässä on koodin Esimerkki käyttäjän sukupuolen ja Syntymäpäivä lähettämistä varten:

    var appInfos = {"birthdate":"1983-12-07","gender":"female"};
    engagement.agent.sendAppInfo(appInfos);

### <a name="limits"></a>Rajoitukset

Rajoitukset, jotka koskevat hakemuksen tiedot ovat säännöllisiä lausekkeita näppäimet ja koon alueet.

#### <a name="keys"></a>Näppäimet

Jokainen objektin avain on vastattava seuraavalla lausekkeella:

    ^[a-zA-Z][a-zA-Z_0-9]*

Tämä tarkoittaa, että näppäimet on alettava kirjaimella vähintään yksi perään kirjaimia, numeroita ja alaviivoja (\_).

#### <a name="size"></a>Kokoa

Hakemuksen tiedot on rajoitettu puhelun 1 024 merkkiä (kun Mobile välitys Web SDK käyttää JSON-muodossa).

Edellisessä esimerkissä palvelimeen lähetetään JSON on 44 merkkiä:

    {"birthdate":"1983-12-07","gender":"female"}
