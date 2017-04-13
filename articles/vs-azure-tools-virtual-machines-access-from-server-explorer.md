<properties
   pageTitle="Azure-Virtuaalikoneissa avaaminen palvelimen Explorer | Microsoft Azure"
   description="Hae opit tarkastelemaan luominen ja hallinta Azure palvelimen Explorerissa Visual Studiossa näennäiskoneiden (VMs)."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="accessing-azure-virtual-machines-from-server-explorer"></a>Azure-Virtuaalikoneissa käyttäminen Server Explorerista

Visual Studio palvelimen Explorerin avulla voit näyttää tietoja siitä, että näennäiskoneiden Azure ylläpitämä.

## <a name="accessing-virtual-machines-in-server-explorer"></a>Palvelimen Explorerissa näennäiskoneiden käyttäminen

Jos sinulla on ylläpitämä Azuren näennäiskoneiden, voit käyttää niitä palvelimen Resurssienhallinnassa. Sinun on kirjauduttava Azure-tilaukseen haluat tarkastella mobiili-palveluita. Voit kirjautua sisään Avaa Azure solmu pikavalikon palvelimen Resurssienhallinnassa ja valitse **yhteyden muodostaminen Microsoft Azure**.

### <a name="to-get-information-about-your-virtual-machines"></a>Saat tietoja siitä, että näennäiskoneiden

1. Palvelimen Resurssienhallinnassa Valitse virtual machine ja valitse sitten Näytä sen ominaisuusikkuna F4-näppäintä.

    Seuraavassa taulukossa näkyy, mitkä ominaisuudet ovat käytettävissä, mutta ne ovat vain luku. Voit muuttaa niiden avulla [Azure perinteinen portal](http://go.microsoft.com/fwlink/?LinkID=213885).

  	|Ominaisuus|Kuvaus|
  	|---|---|
  	|DNS-nimi|Virtuaalikoneen Internet-osoite URL-osoite.|
  	|Ympäristön|Virtual-koneen tämän ominaisuuden arvo on aina tuotannon.|
  	|Nimi|Virtuaalikoneen nimi.|
  	|Kokoa|Virtuaalikoneen, joka vastaa muistia ja käytettävissä olevan tilan määrää kokoa. Lisätietoja on artikkelissa How To: Määritä virtuaalikoneen koot.|
  	|Tila|Arvot ovat aloitus, aloittaminen, pysäyttäminen, pysäytetty ja haetaan tila. Jos haetaan tila on näkyvissä, nykyinen tila on tuntematon. Tämän ominaisuuden arvoja poiketa [Azure perinteinen portal](http://go.microsoft.com/fwlink/?LinkID=213885)käytettävät arvot.|
  	|SubscriptionID|Tilauksen tunnus Azure-tili. Voit näyttää nämä tiedot [Azure perinteinen portal](http://go.microsoft.com/fwlink/?LinkID=213885) -tilauksen ominaisuuksia.|

1. Valitse päätepisteen-solmu ja tarkastele **Ominaisuudet** -ikkuna.

1. Seuraavassa taulukossa on kuvattu päätepisteet käytettävissä olevat ominaisuudet, mutta ne ovat vain luku-tilassa. Voit lisätä tai muokata virtual machine päätepisteet, käytä [Azure perinteinen portal](http://go.microsoft.com/fwlink/?LinkID=213885). 

  	|Ominaisuus|Kuvaus|
  	|---|---|
  	|Nimi|Päätepisteen tunniste.|
  	|Yksityinen portti|Verkkokäyttö sisäinen sovelluksen portti.|
  	|Protokolla|Protokolla, joka käyttää transport layer tämän päätepisteen, TCP tai UDP.|
  	|Julkinen portti|Portti, jota käytetään julkisen access-sovellukseen.|

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja Azure roolien avulla Visual Studiossa, katso [Käyttämällä Etätyöpöytä Azure roolien](vs-azure-tools-remote-desktop-roles.md).
