<properties
   pageTitle="Testauksessa ratkaisu mallin tarjous Marketplacen | Microsoft Azure"
   description="Voit testata ratkaisu mallin tarjous Azure Marketplacen osaavat."
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
   ms.date="12/04/2015"
   ms.author="hascipio; v-divte" />

# <a name="test-your-solution-template-offer-in-staging"></a>Testaa ratkaisu mallin tarjous vaiheet
Väliaikaisen tarkoittaa yksityisesti "eristetyn", jossa voit testata ja varmistaa sen toimintoja ennen valitseminen tuotannon tarjous käyttöönotto. Tarjous näkyy väliaikaisen samalla tavalla kuin se olisi asiakkaalle, joka on käyttöön. Miten väliaikaisen tarjous on hyväksytty.

Kun tarjous Vaiheistettu, voit tarkastella ja testaa tarjous [Azure-portaalissa](https://portal.azure.com/).

Noudata seuraavia ohjeita push väliaikaisen tarjouksen ja testaa se [Azure-portaalissa](https://portal.azure.com/):

1.  Valitse [Julkaisemisportaali](https://publish.windowsazure.com) > **Ratkaisu-mallit** -välilehti > tarjous > **Julkaise** > **Push vaiheet**.
2.  Anna Azure-tilaukset, jotka voit nähdä esikatselun ja kokeilla tarjous luettelo.
3.  Kirjaudu Azure esikatselu-portaaliin, jota käytit edellisessä vaiheessa Tilaustunnus avulla.
4.  Suorita vähintään yksi pyöreä testauksen alla mainitut animoiminen Azure esikatselu-portaalissa:
  - Varmista, että sisällön markkinoinnin näkyy oikein Azure Marketplacesta.
  - Lopusta loppuun-ympäristö, topologian.
  - Suorita suorituskyvyn testaus ja saatuja.
  - Varmista, että topologian noudattaa parhaita käytäntöjä.

## <a name="next-steps"></a>Seuraavat vaiheet
Jos olet tyytyväinen ja valitse voit siirtyä julkaisun lopullinen tarjous-vaiheen **Vaihe 4**: [käyttöönotto Marketplace tarjous](marketplace-publishing-push-to-production.md). Muussa tapauksessa muutosten tekeminen tarjous ja pyydät sertifikaatin uudelleen.

> [AZURE.NOTE] Markkinoinnin sisällön muutoksista sertifikaatin ei ole pakollinen.

Katso [käytön aloittaminen: julkaiseminen tarjouksen Azure Marketplacesta](marketplace-publishing-getting-started.md) lisätietoja ja kaikki publisher-tehtävät.
