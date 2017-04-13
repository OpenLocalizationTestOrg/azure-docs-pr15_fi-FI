<properties
    pageTitle="Azure Government-ohjeet | Microsoft Azure"
    description="Tämä sisältää ominaisuuksia ja ohjeita vertailua Azure Government-sovellusten kehittäminen"
    services="Azure-Government"
    cloud="gov" 
    documentationCenter=""
    authors="ryansoc"
    manager="zakramer"
    editor=""/>

<tags
    ms.service="multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="azure-government"
    ms.date="09/29/2016"
    ms.author="ryansoc"/>


#  <a name="azure-government-compute"></a>Azure Government suorittaminen

##  <a name="virtual-machines"></a>Näennäiskoneiden

Lisätietoja tämän palvelun ja miten sitä käytetään on artikkelissa [Azure näennäiskoneiden koot](../virtual-machines/virtual-machines-windows-sizes.md).

### <a name="variations"></a>Variaatiot

Seuraavat AM tuotteissa on yleisesti saatavilla (GA) Azure Government:

AM SKU|Yhdysvaltain gov – VA|Yhdysvaltain gov – IA|Huomautuksia
---|---|---|---
A|GA|GA|Ei mitään
Dv1|GA|-|Ei mitään
DSv1|GA|-|Ei mitään
Dv2|GA|GA|15 on tulossa
F|GA|GA|Ei mitään
G|Suunniteltu|-|Ei mitään

###  <a name="data-considerations"></a>Tietojen huomioon otettavia seikkoja

Seuraavassa on esitetty Azure Government rajan Azuren näennäiskoneiden:

| Sallittu säännelty ja hallita tiedot | Säännelty ja hallita tietojen ei sallita |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Tietojen syötetty, tallentaa ja käsitellään AM sisältää ohjaa Vie tietoja. Binaaritiedostot käynnissä Azuren näennäiskoneiden kuluessa. Staattinen todentajia, joiden, esimerkiksi salasanat ja älykortin PIN Azure käyttöympäristön osien käytön. Yksityiset avaimet varmenteiden Azure käyttöympäristön osien hallinta. Yhteyden SQL-lauseita.  Muut suojaus tiedot ja tietoja, kuten varmenteet, salausavaimet, perustyyli näppäimet ja tallennustilan näppäimet tallennettu Azure Services-palveluissa.  | Metatietojen ei ole sallittua sisältävät ohjaa vientitiedot. Metatiedot sisältää kaikki kokoonpanotietoja, kun luominen ja ylläpito Azure virtuaalikoneen.  Älä kirjoita Regulated ja hallita tiedot seuraaviin kenttiin: vuokraaja roolin nimet, resurssiryhmät, käyttöönoton nimet, resurssien nimet, resurssi-tunnisteet  

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoa ja päivitykset tilaaminen <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure Government-blogi.</a>
