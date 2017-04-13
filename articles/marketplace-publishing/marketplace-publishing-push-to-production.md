<properties
   pageTitle="Tarjous käyttöön Azure Marketplacesta | Microsoft Azure"
   description="Tietoja ja käy läpi ottamaan tarjous – ohjeita virtuaalikoneen kuva, developer-palvelu, tietopalvelu jne--Azure Marketplace."
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
   ms.date="08/02/2016"
   ms.author="hascipio" />

# <a name="deploy-your-offer-to-the-azure-marketplace"></a>Tarjous käyttöön Azure Marketplacesta
Kun olet tyytyväinen tarjous (eli olet testannut asiakkaiden skenaariot markkinointi sisältöä, jne.) ja haluat käynnistää, pyydä **Push tuotannon** **Julkaise** -välilehdessä.  

1. Neljä vaihetta vaiheittainen kuvaus-kohdassa sivun julkaisu portaalissa on oltava valmiit ja vihreä. Varmista virtuaalikoneen tarjouksia, noudata seuraavia ohjeita noudatetaan.

    ![piirustuksen][img-pubportal-walkthru-checked]

2. Valitse vasemman reunan luettelosta **Julkaise** -välilehti.

    ![piirustuksen][img-pubportal-menu-publish]

3. Napsauta **pyydettävä lupaa tuotannon push**-painiketta. Kun kutsu on tehty, hyväksyntä-ryhmän suorittaa lopullinen Tarkista ja sitten tarjous voi Azure Marketplacesta.

    ![piirustuksen][img-pubportal-publish-pushproduction]

>[AZURE.IMPORTANT] Näennäiskoneiden, jos painaa tuotannon hyväksyntä-painiketta napsautettaessa seuraavat vaiheet suoritetaan näkymän takana. Osaat JULKAISE-välilehdessä jokaisen vaiheen etenemisen tarkasteleminen julkaisu portal. Sinun on tarkistettava tämän sivun säännöllisin väliajoin (kunnes tilana näkyy "Listed") virheen tiedot, joka on kohdassa päästä korjaus.

> - Ensimmäinen tuotannon pyyntö siirtyy kuka Vahvista näennäiskiintolevyn sertifikaatin ryhmän. Jos päivität jo luettelossa tarjous ja pyyntö on käytössä vain markkinoinnin muuta, valitse todistus vaihe ohitetaan.
> - Seuraava vaihe pyyntö tulee sisällön kelpoisuuden tarkistaminen-ryhmän kuka Tarkista tarjouksen markkinoinnin sisällöstä.
> - Jos edellä kuvatut toimet onnistuvat, tarjous on hyväksytty tuotannon. Tällä hetkellä tila tulee "näy" Julkaisemisportaali. Kuitenkin "Listed" tämän tilan ei tarkoita, että prosessi on valmis. Seuraavat vaiheet on suoritettava, ennen kuin tarjous on käytettävissä Azure Marketplacesta.
> - Kun tarjous on hyväksynyt tuotannon yllä olevassa vaiheessa, replikoinnin tarjouksen aloittaa Azure palvelinkeskusten yli. Se yleensä kestää 24-48hours suorittamiseen replikoinnin mutta voi kestää viikon, näennäiskiintolevyn koon mukaan. Jos päivität jo luettelossa tarjous ja se on käytössä vain markkinoinnin muuta, valitse replikointi on kuitenkin nopeampaa.
> - Kun replikointi on valmis, valitse tarjous ovat käytettävissä Azure Marketplacesta.

> Voit poistaa tarjous aina kun se on **Luonnos** -tila (eli koskaan **Push väliaikaisen** tai **Push tuotannon**). **Historia** -välilehdessä Poista luonnos sivun alareunassa **Hylkää luonnos** -painike.


## <a name="production-checklist-for-all-virtual-machine-offers"></a>Kaikki virtuaalikoneen tarjoukset tuotannon tarkistusluettelo

- Varmista, että olet Microsoft Azure Certified-kumppani
- Tuotteissa-välilehdessä vaihtoehto "Piilota tämä tuote Marketplacesta koska se on aina ostanut kautta ratkaisu mallin" merkitään Kyllä vain, jos olevat versiot kuuluu ratkaisu-malli. Kaikissa muissa tapauksissa asetus aina merkitään ei.
- Muista: Älä muuta tuote näkyvyyden määrittäminen kun olevat versiot näkyy. Tämä toiminto ei ole suositeltavaa.
- Varmista, että logoja noudattaa alla Azure Marketplace-logo-ohjeista.
- Tarjouksen ja SKU kuvaus ei pitäisi olla sama.
- TUOTE on otsikko ja tarjota pitkä yhteenveto ei kannata olla sama.
- SKU otsikko ja tarjota yhteenveto ei pitäisi olla sama.
- TUOTE otsikoita ei saa olla sama varten tarjouksen useita tuotteissa.

**Ohjeet Azure Marketplace-logo**

- Azure rakenne on yksinkertainen värivalikoiman. Pidä määrä ensisijainen ja toissijainen värit-logo.
- Teemavärien Azure portaalin on valkoinen ja mustaa. Näin ollen Vältä värejä oman logon taustavärinä. Käytä väriä, jotka korostaa oman logon ääniä Azure-portaalissa. On suositeltavaa yksinkertainen Ensisijaiset värit. Jos käytössäsi on läpinäkyvä tausta, varmista, että logon tai teksti ei ole valkoiseksi tai mustaksi.
- Älä käytä liukuvärjätyn taustan logon.
- Vältä markkinoille tekstiä, vaikka yrityksen tai kuvitteellinen nimi, logo.
- Logon ulkoasun pitäisi olla 'tasainen' ja vältä liukuvärejä.
- Logon ei olisi venytetään.

**Pääkuva logon lisäohjeita:**

- Pääkuva logo on valinnainen. Julkaisija valita ei, jos haluat ladata Pääkuva logon. **Kuitenkin kerran ladatut pääkuva-kuvake ei voi poistaa julkaisu portal. Kumppanin on seurattava Pääkuva kuvakkeet muita-tarjous ei voi hyväksyä Azure Marketplace-ohjeet tuotannon samaan aikaan.**
- Publisher-näyttönimi ja SKU otsikko ja pitkä yhteenveto tarjous näkyvät valkoinen fonttiväri. Näin ollen Vältä pitäminen minkä tahansa vaalean Pääkuva kuvake taustalla. Musta, valkoinen ja Läpinäkyvä tausta ei sallita Pääkuva kuvakkeet.
- Julkaisija näyttää nimen, SKU otsikko, pitkään yhteenveto tarjous ja luo-painike on upottaa ohjelmallisesti Pääkuva logon kun tarjous siirtyy luettelossa. Niin Älä kirjoita tekstiä samalla, kun suunnittelet Pääkuva-logo. Jätä tyhjään kohtaan vain oikealla, koska teksti (eli Publisherin Näytä nimi, SKU otsikko pitkään yhteenveto tarjous) sisällytetään ohjelmallisesti mukaan us sieltä päälle. Tyhjään kohtaan tekstin pitäisi olla 415 x 100 oikealla (ja se on korvata 370px vasemmalta).


## <a name="additional-production-checklist-for-already-listed-virtual-machine-offers"></a>Lisää tarkistusluettelo jo luettelossa virtuaalikoneen tarjoaa

- Tarkista, onko jo samanniminen tarjouksen yrityksestäsi tarjouksen. Jos Kyllä, uusi versio olevat versiot olisi lisääminen aiemmin tarjous sijaan kaksoiskappaleiden uusi tarjous.
- Tietoja levyn Älä muuta saman SKU kahden version välisiä.
- Azure Marketplacesta ei tue luettelossa tuotteissa hinnoittelu muuta kuin se vaikuttaa olemassa olevat asiakkaat laskutus. Varmista, että et muuta luettelossa tuotteissa missä käytettävissä olevat versiot alueilla hinnat. Voit lisätä uuden tuotteissa tai kaksi uutta lisääminen aiemmin luotuun tuote avulla.


## <a name="next-steps"></a>Seuraavat vaiheet
Kun tarjous menee live, Testaa asiakkaan käyttötavoista Vahvista, että kaikki sopimuksia ja toiminnot toimivat oikein kuin tuotantoympäristössä testattu ja vahvistettu väliaikaisen-ympäristössä.

## <a name="see-also"></a>Katso myös
- [Aloittaminen: julkaiseminen tarjouksen Azure Marketplacesta](marketplace-publishing-getting-started.md)

[img-pubportal-walkthru-checked]:media/marketplace-publishing-push-to-production/pubportal-walkthru-checked.png
[img-pubportal-menu-publish]:media/marketplace-publishing-push-to-production/pubportal-menu-publish.png
[img-pubportal-publish-pushproduction]:media/marketplace-publishing-push-to-production/pubportal-publish-pushproduction.png
