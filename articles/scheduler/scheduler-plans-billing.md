<properties
 pageTitle="Suunnitelmien ja Azure ajoituksen Laskutus"
 description="Suunnitelmien ja Azure ajoituksen Laskutus"
 services="scheduler"
 documentationCenter=".NET"
 authors="derek1ee"
 manager="kevinlam1"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="article"
 ms.date="08/18/2016"
 ms.author="deli"/>

# <a name="plans-and-billing-in-azure-scheduler"></a>Suunnitelmien ja Azure ajoituksen Laskutus

## <a name="job-collection-plans"></a>Työn sivustokokoelman suunnitelmat

Työn sivustokokoelmat ovat Azure ajoituksen laskutettavan kohteen. Työn sivustokokoelmat sisältävät työt määrän ja annettava kolme suunnitelmat – vapaa, Vakio ja Premium –, joka on kuvattu seuraavassa.

|**Työn sivustokokoelman suunnitelma**|**Työn sivustokokoelman työt Max #**|**Maks-toisto**|**Maks-työ sivustokokoelmat tilausta kohti**|**Rajoitukset**|
|:---|:---|:---|:---|:---|
|**Vapaa**|työn sivustokokoelman 5 työt|Kerran tunnissa. Ei voi suorittaa työt useammin kuin kerran tunnissa|Tilauksen sallitaan enintään 1 vapaa työn sivustokokoelman|[HTTP lähtevä luvan objektia](scheduler-outbound-authentication.md) ei voi käyttää
|**Vakio**|työn sivustokokoelman 50 työt|Kerran minuutissa. Ei voi suorittaa työt useammin kuin minuutin välein|Tilauksen sallitaan enintään 100 normaalin työn sivustokokoelmat|Käyttää valmiita ominaisuuksia ajoitus|
|**P10 Premium**|työn sivustokokoelman 50 työt|Kerran minuutissa. Ei voi suorittaa työt useammin kuin minuutin välein|Tilauksen sallitaan enintään 10 000 P10 Premium työn sivustokokoelmat. Lisätietoja <a href="mailto:wapteams@microsoft.com">contact us</a> .|Käyttää valmiita ominaisuuksia ajoitus|
|**Ñ20 = Premium**|työn sivustokokoelman 1000 työt|Kerran minuutissa. Ei voi suorittaa työt useammin kuin minuutin välein|Tilauksen sallitaan enintään 10 000 ñ20 = Premium työn sivustokokoelmat. Lisätietoja <a href="mailto:wapteams@microsoft.com">contact us</a> .|Käyttää valmiita ominaisuuksia ajoitus|

## <a name="upgrades-and-downgrades-of-job-collection-plans"></a>Päivitykset ja Downgrades työn sivustokokoelman suunnitelmien

Voit päivittää tai alentaa työn sivustokokoelman suunnitelma milloin tahansa vapaa, Vakio ja Premium-palvelupaketin välillä. Kun päivitysprosessin vapaa työ-kokoelmaan, artikkelissa epäonnistuu johonkin seuraavista syistä:

- Vapaa työn kokoelma jo olemassa-tilaus
- Projektin työn kokoelmassa on suurempi toistuva kuin sallittu vapaa työn sivustokokoelmat työt. Suurin sallittu vapaa työn kokoelman toistuminen on kerran tunnissa
- Työn sivustokokoelman on enemmän kuin 5 töitä
- Projektin työn kokoelmassa on HTTP tai HTTPS-toiminto, joka käyttää [HTTP lähtevä luvan objekti](scheduler-outbound-authentication.md)

## <a name="billing-and-azure-plans"></a>Laskutus- ja Azure-Palvelupaketit

Tilauksia ei perittävän maksutta työn sivustokokoelmat. Jos sinulla on yli 100 normaalin työn sivustokokoelmat (10 vakio laskutuksen yksiköt), on parempi Jaa, jos haluat, että kaikki työ sivustokokoelmat premium-palvelupaketin.

Jos sinulla on yksi normaalin työn kerääminen ja yksi premium työn kokoelma, olet laskutettu yhden vakio laskutuksen yksikkö _ja_ yksi premium laskutuksen yksikön. Työtehtävän sivustokokoelmat, joka on määritetty vakio- tai premium, montako ajoitus-palvelun laskut Tämä selitetään tarkemmin seuraavissa kahdessa osiossa.

## <a name="standard-billable-units"></a>Laskutettavat yksikköjen

Vakio laskutettavan yksikkö voi olla enintään 10 normaalin työn sivustokokoelmat. Koska normaalin työn sivustokokoelman voi olla enintään 50 projektit työn sivustokokoelman kohti, yhden vakio laskutuksen yksikön avulla tilannut on enintään 500 töitä – ylöspäin lähes 22 miljoonaa työn suorituskerran kuukaudessa.

Jos sinulla on 1 – 10 normaalin työn välinen, laskuteta 1 laskutuksen standard yksiköstä. Jos sinulla on 11-20 normaalin työn välinen, laskuteta 2 vakio laskutuksen yksikköä. Jos sinulla on 21 väliltä 30 normaalin työn sivustokokoelmat, laskuteta 3 laskutuksen yksikköjen ja niin edelleen.

## <a name="p10-premium-billable-units"></a>P10 Premium laskutettavat yksiköt

P10 premium laskutettavan yksikkö voi olla enintään 10 000 P10 premium työn sivustokokoelmat. Koska P10 premium työn kokoelman voi olla enintään 50 projektit työn sivustokokoelman kohti, yhden premium laskutuksen yksikön avulla tilannut on enintään 500 000 töitä – ylöspäin 22 miljardia työn suorituskerran kuukaudessa.

Jos sinulla on 1 – 10 000 premium työn sivustokokoelmat välillä, laskuteta 1 P10 premium laskutuksen yksikön. Jos sinulla on 10,001 ja 20 000 premium työn sivustokokoelmat välillä, laskuteta 2 P10 premium laskutuksen yksiköt ja niin edelleen.

Näin ollen P10 premium työn sivustokokoelmat on samat toiminnot kuin normaalin työn kokoelmien, mutta voi hinta tauko, siltä varalta, että sovelluksesi tarvitsee paljon työtä sivustokokoelmat.

## <a name="p20-premium-billable-units"></a>Ñ20 = Premium laskutettavat yksiköt

Laskutettavat ñ20 = premium-yksikkö voi olla enintään 5 000 ñ20 = premium työn sivustokokoelmat. Koska ñ20 = premium työn kokoelman voi olla enintään 1 000 projektin sivustokokoelman työt, yhden premium laskutuksen yksikön avulla tilannut on enintään 5,000,000 töitä – ylöspäin 220 miljardia työn suorituskerran kuukaudessa.

Ñ20 = premium työn sivustokokoelmat tarjoaa samat ominaisuudet kuin P10 premium työn sivustokokoelmat, mutta se tukee myös työn kokoelman suurempi luku projektien ja enemmän kokonaismäärän työt yleistä kuin P10 premium, jonka avulla voi olla useampi skaalattavuus.

## <a name="billing-and-active-status"></a>Laskutuksen ja käytössä-tila

Työn sivustokokoelmat ovat aina aktiivinen, ellei koko tilauksesi on suoriutunut joitakin tilapäinen käytöstä poistettuun vaiheeseen Laskutus ongelmien vuoksi. Ainoa tapa varmistaa, että projektin sivustokokoelman ei ole laskutettu on joko määrittää sen _vapaa_ -suunnitelma tai poistaa työ-sivustokokoelman.

Vaikket voi poistaa käytöstä kaikista projekteista työ-kokoelmassa yhdellä kertaa, se ei vaikuta työn sivustokokoelman laskutuksen tila – työn sivustokokoelman on _edelleen_ laskuteta. Vastaavasti tyhjä projekti sivustokokoelmat on aktiivinen ja laskutetaan.

## <a name="pricing"></a>Hinnat

Hinnat tiedot on [Ajoituksen hinnat](https://azure.microsoft.com/pricing/details/scheduler/).

## <a name="see-also"></a>Katso myös


 [Ajoituksen ominaisuudet](scheduler-intro.md)

 [Azure ajoitus-käsitteistä, terminologiaan ja kohteen hierarkia](scheduler-concepts-terms.md)

 [Aloita käyttö ajoituksen Azure-portaalissa](scheduler-get-started-portal.md)

 [Azure ajoituksen REST API-viittaus](https://msdn.microsoft.com/library/mt629143)

 [Azure ajoituksen PowerShell cmdlet-viittaus](scheduler-powershell-reference.md)

 [Azure ajoituksen suuren käytettävyyden ja luotettavuutta](scheduler-high-availability-reliability.md)

 [Azure ajoituksen rajoitukset, oletusarvot ja virhekoodit](scheduler-limits-defaults-errors.md)

 [Azure ajoituksen lähtevien yhteyksien todennusta](scheduler-outbound-authentication.md)
