<properties
   pageTitle="Ohjelmistossa logiikan sovellusten virheet | Microsoft Azure"
   description="Yleinen lähestymistavat tietoja missä logiikan sovellukset epäonnistuvat"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="10/18/2016"
   ms.author="jehollan"/>

# <a name="diagnosing-logic-app-failures"></a>Logiikan sovelluksen virheet ohjelmistossa

Jos kohtaat ongelmia tai Azure App palvelun logiikan sovellukset-ominaisuudella epäonnistuu, muutaman tavoista auttavat ymmärtämään mistä virheet ovat peräisin.  

## <a name="azure-portal-tools"></a>Azure portaalin Työkalut

Azure-portaalissa on useita työkaluja, vaiheittaisia logiikan kunkin sovelluksen vianmääritys.

### <a name="trigger-history"></a>Käynnistimen historia

Kunkin logiikan-sovelluksessa on vähintään yksi käynnistin. Jos huomaat, että sovellukset eivät ole käynnistettäessä, voit etsiä lisätietoja ensimmäiseksi on käynnistimen historia. Voit käyttää logiikan sovelluksen tärkeimmät sivu käynnistimen-historia.

![Etsiminen käynnistimen historia][1]

Tämä näyttää luettelon kaikista, jotka on tehnyt logiikan sovelluksen käynnistimen yritykset. Voit valita kunkin käynnistimen yrityksellä saat yksityiskohtaiset tiedot (erityisesti haluamasi syötteiden tai tulostaa, käynnistimen yritä luoda) edistynyt. Jos näet epäonnistui käynnistimet, valitse käynnistimen yritys ja Poraudu **tulostaa** linkittäminen virhesanomia, jotka saattavat on luotu (esimerkiksi FTP-tunnistetietoja ei kelpaa).

Näyttöön voi tulla eri tilat ovat seuraavat:

* **Ohitetaan**. Se kohdistaa päätepisteen tiedot tarkistavan pyyntöjä ja -vastauksen tietoja ei ole ollut käytettävissä.
* **Onnistui**. Käynnistin sai vastauksen tietojen ollut käytettävissä. Tämä voi johtua manuaalinen käynnistäminen, toistuva tapahtuma käynnistimen tai kysely käynnistintä. Tämä todennäköisesti liitetään **Fired**tilassa, mutta se saattaa ei, jos sinulla on ehdon tai SplitOn-komento, joka ei täyty koodinäkymän.
* **Epäonnistui**. Virhe on luotu.

#### <a name="starting-a-trigger-manually"></a>Käynnistimen käynnistäminen manuaalisesti

Jos haluat tarkistaa käytettävissä käynnistimen heti (ilman odottaa seuraava toisto) logiikan-sovelluksen, voit valita **Valitse käynnistimen** Valitse Pakota valintamerkki tärkeimmät sivu. Esimerkiksi napsauttamalla tätä linkkiä Dropbox käynnistimen kanssa johtavat työnkulun lisääminen heti Dropbox for uusia tiedostoja.

### <a name="run-history"></a>Suorita historia

Jokaisen käynnistimen, joka on käynnistyy, suorita tulokset. Voit käyttää Suorita tiedot tärkeimmät sivu, jossa on tietoja, jotka voivat olla hyödyllisiä ymmärtää, mitä on tapahtunut työnkulun aikana.

![Suorita historia etsiminen][2]

Suorita näkyy jokin seuraavista tiloista:

* **Onnistui**. Kaikki toiminnot onnistui, tai jos oli virhe, se on käsittelemien toiminnon myöhemmin työnkulussa on tapahtunut. Toisin sanoen se oli käsittelemien toiminnon, joka on määritetty suorittamaan epäonnistui toiminnon jälkeen.
* **Epäonnistui**. Vähintään yksi toiminto oli virhe, joka ei käsitellyt toiminnon myöhemmin työnkulussa.
* **Peruutettu**. Työnkulku on käytössä, mutta vastaanotti Peruuta.
* **Käynnissä**. Työnkulku on käynnissä. Näin voi käydä, työnkulut, jotka ovat parhaillaan rajoittanut tai nykyinen sovellus palvelusopimus vuoksi. [Sivun hinnoittelua](https://azure.microsoft.com/pricing/details/app-service/plans/) lisätietoja artikkelissa toiminnon rajoitukset. Diagnostiikan asetusten määrittäminen (kaavioita, suorita historia alla) myös voi saada tietoa rajoituksen tapahtumat pitkäksi aikaa.

Kun etsit Suorita tarkasteleminen, voit siirtyä määrityksiä.  

#### <a name="trigger-outputs"></a>Käynnistimen tulostus

Käynnistimen tulostaa Näytä tiedot, jotka on vastaanotettu käynnistimestä. Tämä voi auttaa määrittämään kaikki ominaisuudet, joka palauttaa odotetulla tavalla.

>[AZURE.NOTE] Sinun on ehkä Tutustu, miten logiikan-sovellusten ominaisuuksien [käsittelee erilaisia sisältötyyppejä](app-service-logic-content-type.md) Jos näet kaikki sisältö, et ymmärrä.

![Käynnistimen tulostus-esimerkkejä][3]

#### <a name="action-inputs-and-outputs"></a>Toiminnon syötteiden ja tulostaa

Voit siirtyä syötteiden ja tulostaa, toiminnon vastaanotettu. Tämä on hyödyllisiä tietoja koko ja muoto tulostus, sekä siten, että virhesanomia, jotka on jo muodostettu.

![Toiminnon syötteiden ja tulostaa][4]

## <a name="debugging-workflow-runtime"></a>Virheenkorjaus työnkulun suorituksen aikana

Lisäksi seuranta-syötteitä, tulostaa ja suorita käynnistimien, voi olla hyödyllistä Lisää muutama työnkulun virheenkorjaus avuksi. [RequestBin](http://requestb.in) on tehokas työkalu, voit lisätä työnkulun vaiheen. RequestBin avulla voit määrittää HTTP-pyyntö tarkastaminen tarkka koko, muoto ja Muotoile HTTP-pyynnön määrittämiseen. Voit luoda uuden RequestBin ja liitä logiikan app HTTP POST-toiminto sekä testattava pääsisältöä URL-osoite (esimerkiksi lausekkeen tai toinen vaihe tuloste). Kun olet suorittanut logiikan sovellus, voit päivittää oman RequestBin nähdäksesi, kuinka pyynnön muodostettiin, kun se on saatu logiikan sovellukset-ohjelma.




<!-- image references -->
[1]: ./media/app-service-logic-diagnosing-failures/triggerHistory.PNG
[2]: ./media/app-service-logic-diagnosing-failures/runHistory.PNG
[3]: ./media/app-service-logic-diagnosing-failures/triggerOutputsLink.PNG
[4]: ./media/app-service-logic-diagnosing-failures/ActionOutputs.PNG
