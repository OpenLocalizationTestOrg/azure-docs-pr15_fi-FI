<properties
   pageTitle="Testauksessa tietopalvelu tarjous Marketplacen | Microsoft Azure"
   description="Voit testata tietopalvelu tarjous Azure Marketplacen osaavat."
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

<tags
   ms.service="marketplace"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/26/2016"
   ms.author="hascipio; avikova" />

# <a name="testing-your-data-service-offer-in-staging"></a>Väliaikaisen tietopalvelu tarjous testaaminen

>[AZURE.IMPORTANT] **Tällä hetkellä onboarding olemme eivät ole enää mitään uusi tietopalvelu julkaisijat. Uusi dataservices ei Hae hyväksyä luettelo.** Jos haluat julkaista AppSource SaaS yrityssovelluksen löydät lisätietoja [tähän](https://appsource.microsoft.com/partners). Jos sinulla IaaS sovellusten tai haluat julkaista Azure Marketplace-Kehitystyökalut-palvelun löytyy lisätietoja [tästä](https://azure.microsoft.com/marketplace/programs/certified/).

Kun olet suorittanut kaksi ensimmäistä vaihetta [Microsoft Developer-tilin luominen](marketplace-publishing-accounts-creation-registration.md) ja [Lisääminen tietojen palvelun tarjoaa Julkaisemisportaali-](marketplace-publishing-data-service-creation.md) on valmis, jolla varmistetaan tarjous Azure Marketplacesta. Tässä ohjeaiheessa edetään ohjatusti ensin keskitason vaiheen nimeltä "Väliaikaisen"

Väliaikaisen tarkoittaa yksityisesti "eristetyn", jossa voit testata ja varmistaa sen toimintoja ennen valitseminen tuotannon tarjous käyttöönotto. Tarjous näkyvät väliaikaisen samalla tavalla kuin se olisi asiakkaalle, joka on käyttöön.

## <a name="step-1-pushing-your-offer-to-staging"></a>Vaihe 1. Väliaikaisen tarjouksen valitseminen
Valitseminen väliaikaisen tarjouksen voit testata tarjous, ennen kuin se on käytettävissäsi tulevien tilaajille.  Näet, miten tarjous lajittelutapa ja toimi niille tilaamisen tietoihin.  

  ![piirustuksen](media/marketplace-publishing-data-service-test-in-staging/step-1.1.png)

1.  Kirjaudu sisään kyselyjä [julkaiseminen Portal](https://publish.windowsazure.com)
2.  Valitse vasemmassa siirtymisruudussa **Tietopalvelut**
3.  Valitse haluat siirtää väliaikaisen tarjous. Yllä näyttö tulee näkyviin.
4.  **Push-väliaikaisen** -painiketta.  
5.  Jos määritettynä on ongelmat, joita on tarkoitus valmistua ennen valitseminen väliaikaisen tarjous, näet luettelon näkyviin.  Korjaa kohteita napsauttamalla luettelon jokaisen kohteen. Kun kaikki korjaukset tehtävä, napsauta **väliaikaisen Push** -painiketta uudelleen.

Jos ei ole ongelmia tarjous näet alla ponnahdusikkunasta.  

Jos olet ei suunnittelu/ei hyväksynyt siten, että tarjous Azure-portaalissa (tällä hetkellä on rajoitettu kapasiteetti), valitse Sulje ponnahdusikkunan.

Voit testata tietojen palvelua Azure-portaalissa (lisäksi DataMarket-portaalin), sinun on Azure-Tilaustunnus Testaa.  Tilauksen tunnus tunnistaa tili, jolla on oikeus tarjota testata.  

Leikkaa ja liitä tilauksen tunnus ja valitse Jatka valintamerkki.

  ![piirustuksen](media/marketplace-publishing-data-service-test-in-staging/step-1.2.png)

> [AZURE.NOTE] Nämä Azure tilaukset tunnukset ovat vain testaaminen ja väliaikaisen [Azure hallinta-portaalin](https://manage.windowsazure.com)edellyttämät. Voit esikatsella Azure Marketplacesta ei tarvitse.

Seuraavassa näytössä, joka näkyy, että julkaiseminen on meneillään näyttämällä "käynnissä"-kuvake näkyy korostettuna keltainen alla. Väliaikaisen valitseminen on 10 – 15 minuuttia välillä.  Jos kestää kauemmin, ensin Päivitä selain (paina F5-IE).  Napsauta kohtaa, johon tarjous edelleen valitseminen väliaikaisen tunnin jälkeen voit harvinaisissa tapauksissa contact us linkittäminen kerro meille, että on ongelma.

  ![piirustuksen](media/marketplace-publishing-data-service-test-in-staging/step-1.3.png)

Kun haluat väliaikaisen Push on valmis "käynnissä"-kuvake Lopeta siirtäminen ja tila päivitetään "Vaiheistettu".  Olet nyt valmis kokeilemaan tarjous.  

## <a name="step-2-test-your-staged-offer-in-datamarket"></a>Vaihe 2. Testaa vaiheistettu tarjous DataMarket

Napsauttamalla seuraavaa **-kohdassa palvelussa tarjota kierrosluvulla..."** teksti Näyttää tilaajan tulee näkyviin, kun tarjous siirtyy tuotannon ja DataMarket näkyy näytössä.

  ![piirustuksen](media/marketplace-publishing-data-service-test-in-staging/step-2.2.png)

Testaa tai Tarkista 12 kohteiden merkitty edellä varmistamiseksi kaikki logot hinnat ja tapahtumia tekstiä, kuvia, ohjeet ja linkit ovat oikein ja että mikrofoni toimii oikein.  Tämä on hyvä varmistaa kirjoittamasi tarjous luotaessa testi arvoja on nyt korvattu todellisten arvojen kanssa.

1. Tarjous-logo
2. Tarjouksen nimi
3. Yrityksen verkkosivuston julkaisijan nimi ja linkki
4. Tarjous aiheita
5. Oman tarjouksen tuen linkin auttaa tilaajille
6. Tarjous tilannekohtainen kuvaus
7. Tarjouksen suunnitelma, jossa näkyvät laskutustiedot
8. Toteutuksen jälkeisen koodin linkittäminen
9. Esimerkki kuvat, jotka kuvaavat käyttäminen tarjouksen tiedot
10. Jokaisen palvelun sisällä tarjous i/metatiedot
11. Tarjouksen 's käyttöehdot
12. Tarjouksen tietojen esikatselu


Valitse lopuksi palvelun toimivat kautta Datamarket napsauttamalla sen linkkiä "TUTUSTU tähän TIETOJOUKKO".  Avaa uudessa ikkunassa-työkalussa on Soita-palvelun Explorer"niin voit esikatsella kyselyn tulosten vastaan palvelussa.  Tässä ikkunassa voit syöttää tarvittavat parametrit ja palvelun kysely näyttää tulokset.   Näkyvissä on myös URL-Osoitteen kyselyn.  

> [AZURE.NOTE] Muista tarkistaa näyttäminen yläpuolella palvelun tekstiä kuvaus.  Ja jos tarjous koostuu useamman kuin yhden palvelun kutsu valitsemalla Siirry seuraavaan palveluun tarkistetaan sekä testata alareunassa välilehdet.



## <a name="next-step"></a>Seuraava vaihe
Jos sinulla on vaikeuksia ja Tarvitsetko apua ratkaisemista Ota yhteyttä [Azure Publisher-tukeen]( http://go.microsoft.com/fwlink/?LinkId=272975).

Jos olet tyytyväinen ja valmis julkaisemaan tarjous Lue [Push tuotannon pyynnön hyväksymisen](marketplace-publishing-push-to-production.md) ohjeissa.

## <a name="see-also"></a>Katso myös
- [Aloittaminen: Miten voit julkaista tarjouksen Azure Marketplacesta](marketplace-publishing-getting-started.md)
