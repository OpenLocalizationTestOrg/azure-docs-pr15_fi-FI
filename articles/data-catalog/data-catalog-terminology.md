<properties
   pageTitle="Azure tietoluettelon terminologiaa | Microsoft Azure"
   description="Tässä artikkelissa esitellään käsitteitä ja termit Azure tietoluettelon ohjeissa."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="09/21/2016"
   ms.author="maroche"/>

# <a name="azure-data-catalog-terminology"></a>Azure tietoluettelon sanasto

## <a name="catalog"></a>Luettelo

Azure-tietoluettelon on pilvipohjainen metatietojen säilö missä lähteiden ja tietojen varat voidaan rekisteröidä. Luettelo on keskitetyn tallennuspaikka rakenteellisia tietolähteitä poimittujen metatietojen ja kuvaava metatietojen käyttäjät lisätään.

## <a name="data-source"></a>Tietolähde

Tietolähde on järjestelmän tai säilö, joka hallitsee tietojen resurssit. Esimerkkejä tällaisista tiedostoista ovat SQL Server-tietokannat, Oracle-tietokantojen, SQL Server Analysis Services-tietokannat (taulukkomuotoinen tai moniulotteisia) ja SQL Server Reporting Services-palvelimiin.

## <a name="data-asset"></a>Tietoja resurssi

Tietoja varat ovat tietolähteistä, joita voi rekisteröidä luettelon objekteja. Esimerkkejä tällaisista tiedostoista ovat SQL Server-taulukot ja näkymät, Oracle-taulukot ja näkymät, SQL Server Analysis Services toimenpiteitä, dimensioista ja KPI: T ja SQL Server Reporting Services-raportteja.

## <a name="data-asset-location"></a>Kohteiden sijainti

Luettelon tallentaa tietolähteen tai tietojen resurssi, jonka avulla voidaan muodostaa käyttämällä asiakassovellus tietolähteen sijainnin. Muodon ja sijainnin tiedot määräytyvät tietolähteen tyyppi. Esimerkiksi SQL Server-taulukko voidaan tunnistaa sen neljä osaa nimi – palvelimen nimen, tietokannan nimi, rakenteen nimi objektinimi – kun SQL Server Reporting Services-raportista voidaan tunnistaa sen URL-osoite.

## <a name="structural-metadata"></a>Rakenteelliset metatiedot

Rakenteelliset metatiedot tietolähdettä, jossa kuvataan tietojen resurssi rakenteen poimittujen metatiedot. Tämä sisältää kalusto-sijainti, sen objektinimi ja tyyppi ja tyyppikohtaisia lisätiedot. Taulukot ja näkymät rakenteellisia metatiedot sisältää esimerkiksi nimet ja tietotyypit objektin sarakkeiden.

## <a name="descriptive-metadata"></a>Kuvaava metatiedot

Kuvaava metatiedot metatietoja, jotka kuvaavat tarkoituksen tai täsmentäminen tietojen resurssi. Yleensä kuvaava metatietojen lisätään luettelon käyttäjät Azure tietoluettelon-portaalissa, mutta sen voi myös purkaa tietolähteen rekisteröinnin yhteydessä. Esimerkiksi Azure tietoluettelon rekisteröinti työkalu purkaa kuvaukset SQL Server Analysis Services ja SQL Server Reporting Services Description-ominaisuus ja [laajennettuna ominaisuutena ms_description](https://technet.microsoft.com/library/ms190243.aspx) SQL Server-tietokannoissa, jos nämä ominaisuudet täytetty arvoilla.

## <a name="request-access"></a>Käyttöoikeuksien pyytäminen

Tietojen resurssi kuvaava metatiedot voivat sisältää tietoja siitä, miten tiedot resurssi tai tietolähteen käyttöoikeuden pyytäminen. Nämä tiedot annetaan tietojen kohteiden sijainti ja voi olla yksi tai useampi seuraavista vaihtoehdoista:

- Käyttäjän tai ryhmän vastaavan tietolähteen käyttöoikeuksien myöntäminen sähköpostiosoite.
- Kuvata prosessi, jossa käyttäjät on toimittava tietolähteen käyttämään URL-osoite.
- Tunnistetietojen ja käytön hallintatyökalu (Microsoftin Käyttäjätietojen hallinta kuten), jonka avulla voidaan käyttää tietolähteen URL-osoite.
- Tekstimuotoisen merkintä, joka kuvaa, miten käyttäjät voivat käyttää tietolähteeseen.

## <a name="preview"></a>Esikatselu

Azure-tietoluetteloon esikatselu on enintään 20 tietueet, jotka voidaan suodattaa tietolähteen rekisteröinnin yhteydessä ja tallennettuja tietoja resurssi metatietojen kanssa tuoteluettelossa. Esikatselun avulla käyttäjät, jotka Tutustu tietojen resurssi ymmärtää helpommin, sen-funktio ja tarkoitus. Toisin sanoen näe mallitiedot on enemmän hyötyä kuin näe vain sarakkeiden nimet ja tietotyypit.
Esikatselukuvat tuetaan vain taulukot ja näkymät ja erikseen valitsemista käyttäjän rekisteröinnin yhteydessä.

## <a name="data-profile"></a>Profiilin tiedot

Tietoja profiilin Azure-tietoluetteloon on taulukon tason ja sarakkeen tason metatietojen rekisteröidyt tiedot-resurssi voidaan poimittujen tietolähteen rekisteröinnin yhteydessä ja tallennettu luettelon tietojen resurssi metatietoihin liittyviä tietoja. Profiilin tiedot auttavat käyttäjät, jotka Tutustu tietojen resurssi ymmärtää helpommin, sen-funktio ja tarkoitus. Esikatselukuvat tavoin käyttäjäprofiileissa erikseen valitsemista käyttäjän rekisteröinnin yhteydessä.

> [AZURE.NOTE] Poimii profiilin tietoja voi olla kallista toiminnon suurten taulukoiden ja näkymien ja saattaa huomattavasti rekisteröitävä tietolähteen aika.

## <a name="user-perspective"></a>Käyttäjän näkökulmasta

Azure-tietoluetteloon kuka tahansa käyttäjä, voit antaa kuvaava metatietojen rekisteröidyt tiedot resurssi. Kullakin käyttäjällä on eri tiedot ja sen käyttö perspektiivin. Esimerkiksi vastuussa palvelimen järjestelmänvalvoja voi antaa tietoja sen tason palvelusopimus (SLA) tai varmuuskopion ikkunat. data steward voi antaa liiketoiminnan ohjeissa linkkejä käsittelee tietojen; tukee ja henkilön voi antaa kuvaus ehdot, jotka ovat tärkeimpiä muut analyytikot ja joka voi olla eniten hyötyä käyttäjille, joilla on Tutustu ja ymmärtää tiedot.

Kunkin perspektiivien ovat salliminen arvokkaita ja Azure tietoluettelon kunkin käyttäjän voit tarjota tietoja, jotka on havainnollinen, kun kaikki käyttäjät voivat käyttää tietoja ymmärtää tiedot ja tarkoitustaan.

## <a name="expert"></a>Asiantuntija

Asiantuntija on käyttäjä, joka on määritetty hallitsevansa, tiedot annetaan ajan tasalla "asiantuntija" näkökulmasta. Kuka tahansa käyttäjä, voit lisätä itse tai toiselle käyttäjälle, annetaan asiantuntija. Parhaillaan peruutuksesta asiantuntija aina lisäkäyttöoikeuksia Azure-tietoluetteloon; sen avulla käyttäjät voivat löydät helposti ne, jotka todennäköisesti voi olla hyötyä, kun tarkastelet resurssi kuvaava metatietojen perspektiivejä.

## <a name="owner"></a>Omistaja

Omistaja on käyttäjä, jolla on lisäoikeuksia tietojen resurssi, Azure-tietoluetteloon hallintaan. Käyttäjät voivat tehdä rekisteröidyt tiedot omistajuutta ja omistajat voit lisätä muiden käyttäjien kanssasi omistajiksi. Lisätietoja on artikkelissa [tietojen resurssien hallinta](data-catalog-how-to-manage.md)  
> [AZURE.NOTE] Omistus- ja ovat käytettävissä vain Standard Edition, Azure tietoluettelon.

## <a name="registration"></a>Rekisteröinti

Rekisteröinti on poimii tietoja resurssi metatietojen tietolähteestä ja kopioimalla sen Azure tietoluettelon-palveluun. Tietoja omaisuus, joka on rekisteröity on ollut kommentteja ja havaitsi.

## <a name="see-also"></a>Katso myös

- [Mikä on Azure tietoluettelon?](data-catalog-what-is-data-catalog.md) -Artikkelissa esitetään yleiskatsaus tietoluettelon Azure-palvelu, se sisältää arvon ja se tukee skenaarioita.

- Lopusta loppuun-opetusohjelma, joka näytetään, miten voit käyttää tietojen lähteen etsiminen Azure tietoluettelon [Azure tietoluettelon käytön aloittaminen](data-catalog-get-started.md) : Tässä artikkelissa on.  
