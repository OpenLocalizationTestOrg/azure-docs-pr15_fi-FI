<properties
   pageTitle="Ratkaisu mallin luominen Marketplacen opas | Microsoft Azure"
   description="Lisätietoja siitä, miten voit luoda varmentaminen ja ottaa usean AM kuva ratkaisu mallin, osta Azure Marketplace-sivustossa."
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
      ms.date="07/27/2016"
      ms.author="hascipio; v-divte" />

# <a name="guide-to-create-a-solution-template-for-azure-marketplace"></a>Ratkaisu-mallin luominen Azure Marketplacen opas
Kun olet suorittanut vaiheen 1- [tilin luominen ja rekisteröintiä][link-acct-creation], emme ohjattu osoitteessa [tekniset edellytyksistä ratkaisu mallin luominen](marketplace-publishing-solution-template-creation-prerequisites.md)Azure-yhteensopiva ratkaisu-mallin luominen. Nyt on edetään ohjatusti vaiheet ratkaisu-mallin luominen useita VMs [Julkaisemisportaali] [ link-pubportal] Azure Marketplacen.

## <a name="create-your-solution-template-offer-in-the-publishing-portal"></a>Luo ratkaisun mallin tarjous Julkaisemisportaali
Siirry [https://publish.windowsazure.com](http://publish.windowsazure.com). Kun kirjaudut sisään ensimmäistä kertaa [Julkaisemisportaali](https://publish.windowsazure.com/), käytä samaa tiliä, jolla yrityksen myyjän profiili on rekisteröity. Myöhemmin voit lisätä minkä tahansa yrityksen työntekijä Julkaisemisportaali Mää-järjestelmänvalvojaksi.

### <a name="1-select-solution-templates"></a>1. Valitse "Ratkaisu mallit"

  ![piirustuksen][img-pubportal-menu-sol-templ]

### <a name="2-create-a-new-solution-template"></a>2. ratkaisu uuden mallin luominen

  ![piirustuksen][img-pubportal-sol-templ-new]

### <a name="3-start-with-topologies"></a>3. topologioissa aloittaminen
Ratkaisu mallin tarkoitetaan "", kaikki sen topologioissa. Voit määrittää useita topologioissa tarjous ja ratkaisu-mallissa. Tarjouksen on valittuna väliaikaisen, se on miten täyttää sen topologioissa. Noudata seuraavia ohjeita voit määrittää tarjous:     

- Luo on verkkotopologia: "Topologian tunnus" on yleensä topologian ratkaisu mallin nimi. Topologian tunnus käytetään URL-osoite alla kuvatulla tavalla:

  Azure Marketplace: http://azure.microsoft.com/marketplace/partners/ {PublisherNamespace} / {OfferIdentifier} {TopologyIdentifier}

  Azure portaalissa: https://portal.azure.com/#gallery/ {PublisherNamespace}. {OfferIdentifier} {TopologyIdentifier}

- Lisää uusi versio.

### <a name="4-get-your-topology-versions-certified"></a>4. Hae oman sertifioitu topologian-versiot
Lataa zip-tiedosto, joka sisältää kaikki tarvittavia tiedostoja valmistelu topologian versiota. Zip-tiedoston on oltava seuraavat:

- pääkansiossa *mainTemplate.json* ja *createUiDefinition.json* -tiedostoa.
- Linkitetyt mallit ja kaikki tarvittavat komentosarjoja.

  > [AZURE.TIP] Kehittäjille työskennellessäsi luomisesta ratkaisun mallin topologioissa ja tulostamisessa varmennettu business, markkinoinnin ja/tai oikeudellinen osastot yrityksesi käyttää markkinointi- ja sisältöä.

## <a name="next-steps"></a>Seuraavat vaiheet
Luotu ratkaisu mallin ja zip-tiedosto on ladattu, noudata ohjeita [Marketplace markkinoinnin sisällön opas](marketplace-publishing-push-to-staging.md) ennen valitseminen väliaikaisen tarjous. Voit tarkastella kaikkia käytettävissä olevia artikkelien julkaiseminen marketplace [käytön aloittaminen: julkaiseminen tarjouksen Azure Marketplacesta](marketplace-publishing-getting-started.md).

Voit myös ehkä Aiheeseen liittyvät artikkelit:

- AM kuvat: [Tietoja virtuaalikoneen kuvia Azure-tietokannassa](https://msdn.microsoft.com/library/azure/dn790290.aspx)
- AM tunnisteet: [AM agentti ja AM laajennukset yleiskatsaus](https://msdn.microsoft.com/library/azure/dn832621.aspx) ja [Azure AM tunnisteet ja toiminnot](https://msdn.microsoft.com/library/azure/dn606311.aspx)
- Azure Resurssienhallinta: [Authoring Azure ARM-mallit](../resource-group-authoring-templates.md) ja [Yksinkertainen ARM mallin esimerkkejä](https://github.com/rjmax/ArmExamples)
- Tallennustilan tilin throttles: [kuinka näytön varten tallennustilan tilin rajoittaminen](http://blogs.msdn.com/b/mast/archive/2014/08/02/how-to-monitor-for-storage-account-throttling.aspx) ja [Premium-tallennustilan](../storage/storage-premium-storage.md#scalability-and-performance-targets-when-using-premium-storage)

[img-pubportal-menu-sol-templ]:media/marketplace-publishing-solution-template-creation/pubportal-menu-solution-templates.png
[img-pubportal-sol-templ-new]:media/marketplace-publishing-solution-template-creation/pubportal-solution-template-new.png
[link-acct-creation]:marketplace-publishing-accounts-creation-registration.md
[link-pubportal]:https://publish.windowsazure.com
