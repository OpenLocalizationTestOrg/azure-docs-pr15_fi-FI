<properties
    pageTitle="Yleistä logiikan sovellusten yhdistimet | Microsoft Azure"
    description="Yleistä yhdistimiä, jotka voidaan logiikan-sovelluksessa"
    services=""
    documentationCenter="" 
    authors="jeffhollan"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="logic-apps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na" 
   ms.date="07/15/2016"
   ms.author="jehollan"/>

# <a name="using-connectors-in-a-logic-app"></a>Yhdistimien käyttämisestä monitasoisten logiikan-sovelluksessa

Yhdistimien ottaa nopeasti käyttöön tapahtumien tiedot ja toiminnot services, protokollat ja ympäristöissä.  Yhdistimiä, jotka logiikan sovellusten tukee täydellisestä luettelosta voi [olla täällä](apis-list.md).  Yhdistimien voidaan käyttää käynnistimen tai toiminnon logiikan-sovelluksessa ja määritetty *yhteyden* käyttäminen saattaa edellyttää (esimerkiksi: sallimisesta Twitter-tili tai julkaista puolestasi).

## <a name="basics"></a>Perusteet

Yhdistimet ovat isännöityjen palveluiden on saatavana osana logiikan-sovellusta, jos haluat liittää muiden palvelujen, kuten Dynamics Azure-Salesforce- [ja lisää](apis-list.md).  Ne on otettu käyttöön ja hallitsee Microsoft, jotta voit luoda työnkulkuja integroinnin mittakaava, liikenteen ja otettava huomioon suojaus.  Voit lisätä yhdistimen logiikan-sovelluksen hakeminen ja valitsemalla yhdistimen toiminnon tai käynnistin, valitse **Näytä Microsoft hallitun ohjelmointirajapinnan**.

![Toimintovalikko käynnistimen valitsemiseen][1]

Kunkin yhdistimen toiminnon tai käynnistin on sen joukko ominaisuuksien avulla.  Valitse lisätietoja toiminnon tiedot-painikkeen, tai viittaus sen ohjeissa [kerrotaan](apis-list.md).

Jos haluat integroida palvelun tai Ohjelmointirajapinta, joka ei ole vielä yhdistimen, voit myös laajentaa logiikan sovellusten [Mukautetun yhdistimen](../app-service-logic/app-service-logic-create-api-app.md) kautta tai vain kutsu suoraan palvelun kuten HTTP protokollan kautta.

## <a name="triggers"></a>Käynnistimien

Yhdysviivoja on käynnistimen, mikä tarkoittaa, että tapahtumaa yhdistintä tulee Kyselysäännön logiikan-sovellus ja välittää tietoja käynnistin osana.  Käynnistin on aina ensimmäiseksi logiikan-sovelluksessa.  Suositut käynnistimien sisältävät toimintoja, kuten:
 
 * Toistuminen - Suorita tunnissa
 * Kun HTTP-pyyntö on vastaanotettu
 * Kun kohde on lisätty jonossa
 * Kun sähköpostia saapuu
 
Jotkin käynnistimien Kyselysäännön tapahtuman tapahtuu – logiikan sovellukseen ilmoituksen pikaviestikeskustelun ja muiden on toistovälin, kuinka usein logiikan sovelluksen Tarkista tapahtuman (enintään 15 sekunnin välein) palvelu määritetty.  

Kun tapahtuman vastaanotetaan, suorita logiikan sovellus Kyselysäännön ja työnkulku käynnistyy.  Osaat myös käyttää mitään tietoja (esimerkiksi "Valitse uusi tweetin' käynnistin välittää tweetin Suorita kyselyjä) työnkulun koko käynnistimestä.

## <a name="actions"></a>Toiminnot

Useimmat yhdistimet on yksi tai useita toimintoja, jotka voidaan suorittaa työnkulun osana.  Toiminnot ovat vaiheet, jotka käynnistyvät, kun Suorita on käynnistyy käynnistimestä.  Toiminto valitsemalla Lisää **Uusi vaihe** -painike ja Etsi yhdistimen haluat käyttää.  Kun valittuna (ja sen jälkeen, kun kaikki [yhteydet](#connections) , joita voidaan vaatia määrittäminen) voit määrittää toiminnon kortin tulee näkyviin.  Voit valita tiedot aiemmissa vaiheissa valitsemalla jonkin tunnuksia, tulostaa tai kirjoita muiden asetusten tarpeen mukaan.

![Yhdistimen toiminnon määrittäminen][2]

## <a name="connections"></a>Yhteydet

Useimmat yhdistimet edellyttää, voit määrittää *yhteyden* , ennen kuin voit käyttää yhdistimen.  *Yhteys* on tarvittavat käyttää yhdistimen kirjautuminen tai yhteys määritykset.  Tarkoitetaan yhdistimiä, jotka käyttää OAuth-yhteyden luominen (kuten Office 365: ssä, Salesforce tai GitHub) palvelu kohtaa, johon access-tunnuksen voit salataan ja tallennetaan suojatusti Azure salainen kaupan kirjauduttaessa.  Muut yhdysviivat (kuten FTP- ja SQL) vaativat yhteyden, joka sisältää määritykset, kuten palvelimen osoite, käyttäjänimi ja salasana.  Näiden määritysten yhteystiedot ovat myös salataan ja tallennetaan suojatusti.  Yhteyksiä voi käyttää palvelua, kunhan palvelun avulla.  Azure Active Directory-OAuth-yhteyksien (kuten Office 365: ssä ja Dynamics) on edelleen päivittäminen käyttöoikeustietue jatkuvasti.  Muiden palvelujen voi siirtää rajoitukset Microsoft kuinka kauan käyttää tunnusta ilman sitä päivittämisen.  Yleinen tiettyjen toimintojen, kuten salasanan vaihtaminen poistaa access tunnusten.  

Yhteyksiä voi tarkastella ja hallita Azure valitsemalla **Selaa** ja sitten **API yhteydet**.  API yhteydet resurssin tarkasteleminen, muokkaaminen, Päivitä tai määritä uudelleen mahdolliset yhteydet, jotka olet luonut.

## <a name="next-steps"></a>Seuraavat vaiheet

- [Ensimmäinen logiikan-sovelluksen luominen](../app-service-logic/app-service-logic-create-a-logic-app.md)
- [Lue yleisiä käyttää ja esimerkkejä logiikan sovellukset](../app-service-logic/app-service-logic-examples-and-scenarios.md)
- [Yrityksen integrointi Käynnistimet ja toiminnot käytön aloittaminen](../app-service-logic/app-service-logic-enterprise-integration-overview.md)

<!--Image References -->
[1]: ./media/connectors-overview/addAction.png
[2]: ./media/connectors-overview/configureAction.png