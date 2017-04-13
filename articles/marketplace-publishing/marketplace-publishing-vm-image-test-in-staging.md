<properties
   pageTitle="Testaa AM tarjous Marketplacen | Microsoft Azure"
   description="Tietoja siitä, miten AM Testikuva Azure Marketplacen."
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
   ms.date="08/01/2016"
   ms.author="hascipio" />

# <a name="test-your-vm-offer-for-the-azure-marketplace-in-staging"></a>Testaa AM tarjous Azure Marketplacen vaiheet

Väliaikaisen tarkoittaa käyttöönotto yksityiseksi "eristetyn", jossa voit testata ja vahvistaa sen toimintoja ennen kuin otat Marketplace oman tuote. Olevat versiot näkyy väliaikaisen samalla tavalla kuin se olisi asiakkaalle, joka on käyttöön. Väliaikaisen sijaita AM kuva on hyväksytty.

## <a name="step-1-push-your-offer-to-staging"></a>Vaihe 1: Push väliaikaisen tarjouksen

1. Valitse **Julkaise** -välilehdessä **Push väliaikaisen**.

    ![piirustuksen](media/marketplace-publishing-vm-image-test-in-staging/vm-image-push-to-staging.png)

2. Jos Julkaisemisportaali sanoma, jonka virheitä, korjaa ne.
3.  : **Käyttöoikeuden vaiheistettu tarjous?** valintaikkunan-ruutuun Azure-tilaukset, jotka voit esikatsella tarjous [Azure esikatselu portal](https://portal.azure.com)-luettelo.

    >[AZURE.NOTE] Näennäiskoneiden ja ratkaisu mallit, ota **eivät** whitelist tilaukset CSP, DreamSpark tai Azure Avaa-tyypin.


    > Näennäiskoneiden, jos **PUSH väliaikaisen**-painiketta napsautettaessa seuraavat vaiheet suoritetaan näkymän takana. Osaat JULKAISE-välilehdessä jokaisen vaiheen etenemisen tarkasteleminen julkaisu portal. Sinun on tarkistettava tämän sivun säännöllisin väliajoin (kunnes tilana näkyy VAIHEISTETTU) virheen tiedot, joka on kohdassa päästä korjaus.

    > - Ensimmäinen väliaikaisen pyyntö siirtyy kuka Vahvista näennäiskiintolevyn sertifikaatin ryhmän. Kuitenkin jos pyyntö on käytössä vain markkinoinnin muuta, valitse todistus vaihe ohitetaan.
    > - Kun varmentaminen on valmis, replikoinnin tarjouksen aloittaa Azure palvelinkeskusten yli. Se yleensä kestää 24-48hours suorittamiseen replikoinnin mutta voi kestää viikon, näennäiskiintolevyn koon mukaan. Jos kuitenkin pyyntö on käytössä vain markkinoinnin muuta, sitten replikointi on nopeampaa.
    > - Kun replikointi on valmis, tarjous ovat käytettävissä [Azure portal](http:/portal.azure.com). AT, tila, kun muuttuvat VAIHEISTETTU julkaisu portal. Vaiheistettu tarjouksen näkyy vain käyttämällä sähköposti-tunnus, jolla tarjous vaiheistettu-tilaukseen liittyvää [Azure portal](http:/portal.azure.com) .

4. Kirjaudu [Azure esikatsella portal](https://portal.azure.com) jollakin Azure tilaukset, jotka on lueteltu edellisessä vaiheessa.
5. Etsi tarjous ja vahvista AM kuva pisteiden:
  - Varmista, että sisällön markkinoinnin näkyy oikein Marketplacesta.
  - Lopusta loppuun-ympäristö, AM kuvan.

      ![IMG-kartta-portaalissa](media/marketplace-publishing-push-to-staging/pubportal-mapping-azure-portal.jpg)

> [AZURE.IMPORTANT] Tarjous säilyy, kunnes Ilmoita Microsoft kautta Julkaisemisportaali väliaikaisen [**Julkaise** -välilehti > napsauttamalla painiketta **"pyytää hyväksyntää, Push, tuotannon"**], että olet valmis, jos haluat siirtää tuotannon. Tämä on paras mahdollinen ajan kohdalla kaikki valmistelussa tarjous siirtymällä luettelossa kaikki jäsenet ryhmän-valintaruutu.

> Väliaikaisen ympäristö on suunniteltu julkaisijan testikäyttöön tarjous esikatselu-tilassa. On erittäin avaavan käyttämällä tämän platofrm commerical tarkoituksiin.

## <a name="next-steps"></a>Seuraavat vaiheet
Tarjous on "vaiheistettu" ja olet testannut toiminnoista ja sisällön markkinoinnin, voit siirtyä viimeinen julkaisun vaihe **Vaihe 4**: [käyttöönotto Marketplace tarjous](marketplace-publishing-push-to-production.md).

## <a name="see-also"></a>Katso myös
- [Aloittaminen: julkaiseminen tarjouksen Azure Marketplacesta](marketplace-publishing-getting-started.md)
