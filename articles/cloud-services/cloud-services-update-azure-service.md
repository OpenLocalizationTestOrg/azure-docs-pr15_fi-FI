<properties
pageTitle="Päivittämisestä pilvipalveluun | Microsoft Azure"
description="Opi päivittämään pilvipalveluihin Azure-tietokannassa. Katso, miten päivitetyt tiedot pilvipalveluun etenee käytettävyyden varmistamiseksi."
services="cloud-services"
documentationCenter=""
authors="Thraka"
manager="timlt"
editor=""/>
<tags
ms.service="cloud-services"
ms.workload="tbd"
ms.tgt_pltfrm="na"
ms.devlang="na"
ms.topic="article"
ms.date="08/10/2016"
ms.author="adegeo"/>

# <a name="how-to-update-a-cloud-service"></a>Päivittämisestä pilvipalveluun

## <a name="overview"></a>Yleiskatsaus
10 000 jalat päivittäminen pilvipalveluun, kuten sen roolien ja Vieras-Käyttöjärjestelmää, on kolme vaihetta. Ensin binaaritiedostot ja asetustiedostot uusi pilvipalvelussa tai käyttöjärjestelmäversio on ladattava. Seuraavaksi Azure varaa suorittaminen ja verkon resursseille pilvipalvelussa uuden cloud service-version järjestelmävaatimukset. Lopuksi Azure suorittaa vaiheittaisen päivityksen, Päivitä vaiheittainen vuokraajan uusi versio tai Vieras-Käyttöjärjestelmää, säilyttämällä käytettävyyden. Tässä artikkelissa kerrotaan tässä viimeisessä vaiheessa – vaiheittaisen päivityksen tietoja.

## <a name="update-an-azure-service"></a>Päivitä Azure palvelu

Azure järjestää roolin esiintymät loogisesti ryhmiteltyjä kutsutaan päivityksen domains (UD). Päivityksen domains (UD) ovat loogisen roolin esiintymät, jotka päivittyvät ryhmänä.  Cloud Azure päivittää palvelun yhden UD kerrallaan, joka sallii esiintymät muiden UDs Jatka käsittelevä liikenne.

Päivitä toimialueen oletusnumero on 5. Voit määrittää eri päivityksen toimialuemäärän sisällyttämällä upgradeDomainCount määrite palvelun määritystiedostossa (.csdef). Saat lisätietoja upgradeDomainCount-määritteen [WebRole rakennetta](https://msdn.microsoft.com/library/azure/gg557553.aspx) tai [WorkerRole rakenne](https://msdn.microsoft.com/library/azure/gg557552.aspx).

Suoritettaessa paikallaan tapahtuva päivitys vähintään yksi roolien palvelun Azure päivittää määrittää roolin esiintymät, johon ne kuuluvat päivityksen toimialueen mukaan. Kaikki esiintymät annetun päivityksen toimialueen – pysäyttäminen ne päivitetään, joka tuo ne takaisin online – Azure päivitykset siirtyy seuraavaan toimialueen sivulle. Mukaan pysäyttäminen vain nykyisen päivityksen toimialueen esiintymiä Azure varmistetaan, että päivitys esiintyy mahdollisimman vaikutus käynnissä-palveluun. Lisätietoja on tämän artikkelin kohdassa [miten päivitys etenee](#howanupgradeproceeds) .

> [AZURE.NOTE] Kun ehdot **Päivitä** ja **Päivitä** on hieman eri merkitys kontekstissa Azure, ne voidaan tarkoittavat prosessit ja tämän asiakirjan ominaisuuksien kuvaukset.

Palvelussa on määritettävä vähintään kaksi esiintymät, on päivitettävä roolin rooli paikallaan ilman käyttökatkot. Jos palvelun koostuu vain yhden esiintymän yksi rooli-palvelussa ei ole käytettävissä, kunnes paikallaan tapahtuva päivitys on valmis.

Tässä artikkelissa käsitellään Azure päivitykset seuraavat tiedot:

-   [Sallittu palvelumuutoksista päivityksen aikana](#AllowedChanges)
-   [Miten päivitys etenee](#howanupgradeproceeds)
-   [Päivityksen peruuttaminen](#RollbackofanUpdate)
-   [Aloitetaan useita mutating toimintoja jatkuvaa ympäristöön](#multiplemutatingoperations)
-   [Jakauman päivityksen toimialueilla roolit](#distributiondfroles)

<a name="AllowedChanges"></a>
## <a name="allowed-service-changes-during-an-update"></a>Sallittu palvelumuutoksista päivityksen aikana
Seuraavassa taulukossa on sallittu muutokset palveluun päivityksen aikana:

|Muutokset sallittua isännöinnin, palveluihin ja roolit|Paikallaan tapahtuva päivitys|Vaiheistettu (VIP Vaihda)|Poistaminen ja ottaminen uudelleen käyttöön|
|---|---|---|---|
|Käyttöjärjestelmäversio|Kyllä|Kyllä|Kyllä
|.NET luottamustaso|Kyllä|Kyllä|Kyllä|
|Virtuaalikoneen koko<sup>1</sup>|Kyllä,<sup>2</sup>|Kyllä|Kyllä|
|Paikallinen tallennus asetukset|Suurenna vain<sup>2</sup>|Kyllä|Kyllä|
|Lisää tai poista roolit-palvelu|Kyllä|Kyllä|Kyllä|
|Tietyn roolin esiintymien määrän|Kyllä|Kyllä|Kyllä|
|Luku- tai päätepisteet palvelun tyyppi|Kyllä,<sup>2</sup>|Ei|Kyllä|
|Nimet ja arvot asetukset|Kyllä|Kyllä|Kyllä|
|Arvot (mutta ei nimet) asetukset.|Kyllä|Kyllä|Kyllä|
|Lisää uusi varmenteet|Kyllä|Kyllä|Kyllä|
|Muuta aiemmin varmenteet|Kyllä|Kyllä|Kyllä|
|Ottaa käyttöön uuden koodia|Kyllä|Kyllä|Kyllä|
<sup>1</sup> Rajoitettu käytettävissä pilvipalvelussa koot alijoukko koon muuttaminen.

<sup>2</sup> Edellyttää Azure SDK 1,5 tai uudempi versio.

> [AZURE.WARNING] Virtuaalikoneen koon muuttaminen tuhoa paikalliset tiedot.


Päivityksen aikana ei tue seuraavia kohteita:

-   Roolin nimen muuttaminen. Poista ja lisää sitten rooli uudella nimellä.
-   Päivitä toimialueen määrän muuttaminen.
-   Pienentää kokoa paikalliset resurssit.

Jos teet muita päivityksiä yhteyttä palvelun määritystä, kuten paikallinen resurssi koon pienentäminen sinun täytyy suorittaa VIP swap-päivitys. Lisätietoja on artikkelissa [Vaihda käyttöönotto](https://msdn.microsoft.com/library/azure/ee460814.aspx).

<a name="howanupgradeproceeds"></a>
## <a name="how-an-upgrade-proceeds"></a>Miten päivitys etenee
Voit päättää, haluatko päivittää kaikki palvelun roolien tai rooliin-palvelussa. Kummassakin tapauksessa kaikki esiintymät kunkin rooli päivitetään ja ensimmäinen päivityksen toimialue kuuluu on pysäytetty, päivittää ja palauttaa online-tilaan. Kun ne ovat online-tilaan, toisen päivityksen toimialueen esiintymät on pysäytetty, päivittää ja palauttaa online-tilaan. Pilvipalvelun voi olla enintään yhden päivityksen aktiivinen kerrallaan. Päivitys suoritetaan aina vastaan pilvipalvelussa uusimman version.

Seuraavassa kaaviossa on kuvattu, miten päivitys etenee, jos päivität kaikki roolit-palvelussa:

![Palvelun päivittäminen] (media/cloud-services-update-azure-service/IC345879.png "Palvelun päivittäminen")

Seuraava Tässä kaaviossa on kuvattu, miten päivitys etenee, jos päivität vain yhteen rooliin:

![Päivitä rooli] (media/cloud-services-update-azure-service/IC345880.png "Päivitä rooli")  

> [AZURE.NOTE] Jos päivität yhden esiintymän palvelun useita kertoja palvelun otetaan käyttöön samalla, kun päivitys suoritetaan vuoksi tapa Azure päivitykset-palvelut. Palvelun tärkeyden guaranteeing palvelun saatavuus koskee vain palvelut, jotka on otettu käyttöön useita esiintymää. Seuraavassa luettelossa kuvataan, kuinka tiedot kussakin asemassa vaikuttaa kutakin Azure palvelun päivitystä skenaariota:
>
>AM uudelleenkäynnistys:
>
-   C: säilyvät
-   D: säilyvät
-   E säilyvät
>
>Portaalin uudelleenkäynnistys:
>
-   C: säilyvät
-   D: säilyvät
-   E hävitetään
>
>Portaalin Reimage:
>
-   C: säilyvät
-   D: hävitetään
-   E hävitetään

>Paikallaan tapahtuva päivitys:
>
-   C: säilyvät
-   D: säilyvät
-   E hävitetään
>
>Solmun siirto:
>
-   C: hävitetään
-   D: hävitetään
-   E hävitetään

>Huomaa, että, edellä olevassa luettelossa, e-asema roolin pääkansion asema, ei saa olla koodattu. Käytä sen sijaan % RoleRoot %-ympäristömuuttuja edustamaan asemaa.

>Pienennä käyttökatkot yhden esiintymän palvelun päivitystä, uuden usean esiintymä-palvelun käyttöön väliaikaisen palvelimessa ja suorita VIP Vaihda.

Automaattisen päivityksen yhteydessä Azure kangasta ohjauskoneen arvioi säännöllisesti määrittääksesi, kun se on turvallista Suorita seuraava UD pilvipalvelussa kunto. Kuntotietojen arviointi suoritetaan roolin kohti välein ja katsoo vain esiintymiä uusimman version (eli UDs, joka on jo walked esiintymiä). Se varmistaa, että rooli esiintymien kunkin roolin vähimmäismäärä on saavutettu tyydyttävästi pääte tila.

### <a name="role-instance-start-timeout"></a>Roolin esiintymän Käynnistä aikakatkaisu
Kangasta ohjauskoneen odottaa 30 minuuttia kunkin roolin esiintymän saavuttamiseksi aloittaminen-tilaan. Jos aikakatkaisun kesto kuluu, kangasta ohjauskoneen säilyvät ominaisuussäilöjen seuraava rooli-esiintymässä.

<a name="RollbackofanUpdate"></a>
## <a name="rollback-of-an-update"></a>Päivityksen peruuttaminen
Azure joustavasti palveluiden hallinta päivityksen aikana ilmoittamalla voit aloittaa lisätoiminnot-palvelun, kun ensimmäisen päivityksen pyyntö hyväksytään Azure kangasta valvoja. Peruutus voidaan suorittaa vain, kun päivitys (määritysten muuta) tai päivitys on **käynnissä** -tilassa ympäristöön. Päivitä tai päivitä pidetään on käynnissä on vähintään yksi esiintymä-palvelun, joka ei ole vielä päivitetty uuteen versioon. Testaa, onko peruutus sallittu, tarkista RollbackAllowed merkintää, [Hae käyttöönotto](https://msdn.microsoft.com/library/azure/ee460804.aspx) -ja [Hae Cloud palveluominaisuudet](https://msdn.microsoft.com/library/azure/ee460806.aspx) palauttama arvo, on määritetty tosi.

> [AZURE.NOTE] Vain kannattaa soittaminen palauttaminen **paikallaan tapahtuva** päivitys tai päivittää, sillä VIP Vaihda päivitykset liittyä yhteen koko suoritettavan palvelun korvaaminen toisella.

Käynnissä olevat päivityksen peruuttaminen on seuraavista tehosteista ympäristöön:

-   Kaikki roolin esiintymät, joka ei ole vielä oli päivitetään tai päivitetty uuteen versioon ei päivitetä tai päivittää, koska tilanteita, on jo käytössä palvelun kohde-version.
-   Kaikki roolin esiintymät, joka oli jo päivitetty tai päivittää uuden version service-paketin (\*.cspkg) tiedoston tai palvelun (\*.cscfg)-tiedosto (tai molemmat tiedostot) palautetaan päivitystä edeltävän version nämä tiedostot.

Tämä toiminnallisesti tarjoaa seuraavia ominaisuuksia:

-   [Peruutus Päivitä tai päivitä](https://msdn.microsoft.com/library/azure/hh403977.aspx) toiminto, joka voidaan kutsua määritys-päivityksen (saatu puheluja [Muuta käyttöönoton määritys](https://msdn.microsoft.com/library/azure/ee460809.aspx)) tai (käynnistämä puheluja [Päivittäminen käyttöönoton](https://msdn.microsoft.com/library/azure/ee460793.aspx)) päivityksen, kunhan on vähintään yksi esiintymä-palvelussa, jossa ei vielä ole päivitetty uuteen versioon.
-   Lukittu-osa ja RollbackAllowed elementti, joka palautetaan osana vastauksen jotakin [Hae käyttöönotto](https://msdn.microsoft.com/library/azure/ee460804.aspx) - ja [Hae Cloud palveluominaisuudet](https://msdn.microsoft.com/library/azure/ee460806.aspx) -toimintoa:
    1.  Lukittu-osan avulla voit tunnistaa, kun mutating toiminto voidaan kutsua annetun käyttöönotto.
    2.  RollbackAllowed-osan avulla voit tunnistaa, kun [Peruutus Päivitä tai päivitä](https://msdn.microsoft.com/library/azure/hh403977.aspx) -toimintoa voidaan kutsua annetun käyttöönotto.

    Jotta voit suorittaa peruutus, ei tarvitse lukittu-ja RollbackAllowed osat. Se suffices vahvistamaan, että RollbackAllowed on määritetty tosi. Näitä elementtejä palautetaan vain, jos näistä menetelmistä on kutsuttu käyttämällä asettaminen pyynnön otsikko "x-ms-versio: 2011-10-01" tai sitä uudempi versio. Saat lisätietoja versiotietojen otsikot [Service Management versiotietojen](https://msdn.microsoft.com/library/azure/gg592580.aspx).

Joissain tilanteissa Jos päivityksen peruuttaminen tai päivitys ei ole tuettu, ne ovat seuraavat:

-   Vähenemisen paikalliset resurssit - Jos päivitys kasvaa paikalliset resurssit roolin Azure-ympäristö ei salli peruutetaan. 
-   Kiintiön rajoitukset - Jos päivitys on asteikolla alaspäin toimintoa voi enää on riittävä peruutus-toiminnon suorittaminen kiintiön. Kunkin Azure tilauksella on liitetty kiintiön, joka määrittää, kuinka monta sydämiä, jotka on käytetty isännöityä palvelut, jotka kuuluvat kyseiseen tilaukseen. Jos suorittamiseen annetun päivityksen peruuttaminen siirtävät tilauksen päälle kiintiön sitten, joka palauttaminen ole käytössä.
-   Kilpa ehto – Jos alkuperäinen päivitys on valmis peruutus ei ole mahdollista.

Jos käytät [Päivittäminen käyttöönotto](https://msdn.microsoft.com/library/azure/ee460793.aspx) -toiminto, jolla oman Azure päivitys on pää paikallaan pitäminen palvelun korko on esiin ohjausobjektiin manuaalinen tilassa on esimerkiksi kun päivityksen peruuttaminen voivat olla hyödyllisiä.

Rahoittaja päivityksen aikana Soita [Päivittäminen käyttöönoton](https://msdn.microsoft.com/library/azure/ee460793.aspx) manuaalinen-tilassa ja aloita kerrotaan päivityksen toimialueet. Jos jossakin vaiheessa tavalliseen tapaan valvoa päivitys, Huomaa, että ensimmäinen päivityksen toimialueiden, voit tarkastella roolin tietyissä tilanteissa on tullut ei vastaa, voit soittaa käyttöönotosta, joka jää sellaisekseen, joka oli ei vielä ole päivitetty esiintymät ja peruutus esiintymät, jotka oli päivitetty edellisen palvelupakettiin- ja määritystietoja [Palauttaminen Päivitä tai päivitä](https://msdn.microsoft.com/library/azure/hh403977.aspx) -toimintoa.

<a name="multiplemutatingoperations"></a>
## <a name="initiating-multiple-mutating-operations-on-an-ongoing-deployment"></a>Aloitetaan useita mutating toimintoja jatkuvaa ympäristöön
Joissakin tilanteissa haluat ehkä aloittaa useita samanaikaisesti mutating toimintoja jatkuvaa käytössä. Esimerkiksi saattaa toimia service-päivitys ja kyseinen päivitys on palautetaan palvelun kautta, kun haluat tehdä muutoksia, kuten voit koota päivitys takaisin eri päivityksen tai jopa poistaa käyttöönotto. Palvelupyynnön, joissa tämä voi olla tarpeen on, jos jälkeinen versio sisältää buggy koodia, joka aiheuttaa päivitetyn roolin esiintymä kaatuvan toistuvasti. Tässä tapauksessa Azure kangasta ohjauskoneen eivät voi tehdä edistymistä otetaan käyttöön, päivittää, sillä vähän esiintymien päivitetyn toimialueessa on kunnossa. Tässä tilassa kutsutaan nimellä *jumissa käyttöönotto*. Voit voima käyttöönoton sen peruutetaan päivitys tai käyttämällä ajan tasalla päivityksen päälle epäonnistunut alkuun jokin.

Kun alkuperäisen pyynnön Päivitä tai päivitä palvelu on vastaanottanut Azure kangasta ohjauskoneen, voit aloittaa mutating toimintojen. Ei ole on odotettava alkuperäinen toiminto on valmis, ennen kuin aloitat toinen mutating toiminto.

Aloitetaan toisen päivitystoiminto, kun ensimmäinen päivitys on meneillään oleviin suorittaa vastaavat peruuttaminen-toimintoon. Jos toinen päivitys on automaattinen-tilassa, ensimmäinen päivityksen toimialue päivitetään välittömästi, alussa mahdollisesti esiintymissä useita päivityksen toimialueita on offline-tilassa samaan kohtaan samanaikaisesti.

Mutating toiminnot ovat seuraavat: [Muuta käyttöönoton määritys](https://msdn.microsoft.com/library/azure/ee460809.aspx), [Päivittäminen käyttöönoton](https://msdn.microsoft.com/library/azure/ee460793.aspx), [Päivityksen käyttöönoton tilan](https://msdn.microsoft.com/library/azure/ee460808.aspx), [Poista käyttöönoton](https://msdn.microsoft.com/library/azure/ee460815.aspx)ja [Peruuta päivitys ja päivittää](https://msdn.microsoft.com/library/azure/hh403977.aspx).

[Hae käyttöönotto](https://msdn.microsoft.com/library/azure/ee460804.aspx) - ja [Hae Cloud palveluominaisuudet](https://msdn.microsoft.com/library/azure/ee460806.aspx)-toimintojen palauttaa joka voidaan tutkia määrittääksesi, onko mutating toiminto voidaan kutsua annetun käyttöönoton lukittu-merkinnän.

Jotta voit soittaa näistä tavoista versiota, joka palauttaa lukittu-merkintä, sinun on määritettävä pyynnön otsikossa arvoksi "x-ms-versio: 2011-10-01" tai uudempi versio. Saat lisätietoja versiotietojen otsikot [Service Management versiotietojen](https://msdn.microsoft.com/library/azure/gg592580.aspx).

<a name="distributiondfroles"></a>
## <a name="distribution-of-roles-across-upgrade-domains"></a>Jakauman päivityksen toimialueilla roolit
Azure jakaa rooli esiintymät tasaisesti päivityksen toimialueet, joka on määritetty osana palvelun määritystiedoston (.csdef) tietyn ajan. Päivityksen toimialueiden enimmäismäärä on 20 ja oletusarvo on 5. Katso lisätietoja siitä, miten voit muokata palvelun määritystiedoston, [Azure määritelmä malli (.csdef tiedosto)](cloud-services-model-and-package.md#csdef).

Esimerkiksi jos tehtäväsi on kymmenen esiintymät, oletusarvon mukaan jokaisen päivityksen toimialueen sisältää kaksi esiintymää. Jos tehtäväsi on 14 esiintymät, valitse neljästä päivitystä toimialueiden sisältävät kolmessa tilanteessa ja viidennen toimialue on kaksi.

Päivityksen toimialueiden tunnistetaan nollasta indeksi: ensimmäinen päivityksen toimialue on tunnus on 0, ja toisen päivityksen toimialueen on tunnus on 1 ja niin edelleen.

Seuraavassa kaaviossa on kuvattu, miten palvelu kuin sisältää kaksi roolia jaetaan, kun palvelua määritetään kaksi Päivitä toimialuetta. Palvelu on käynnissä, web-roolin kahdeksan esiintymät ja Työntekijä-roolin yhdeksän esiintymät.

![Päivitä toimialueen jakauman.] (media/cloud-services-update-azure-service/IC345533.png "Päivitä toimialueen jakauman.")

> [AZURE.NOTE] Huomautus Azure määrittää, miten esiintymät kohdistetaan päivityksen toimialueilla. Ei ole mahdollista määrittää toimialueen jaetaan esiintymät.

## <a name="next-steps"></a>Seuraavat vaiheet
[Voit hallita pilvipalveluihin](cloud-services-how-to-manage.md)  
[Voit valvoa pilvipalveluihin](cloud-services-how-to-monitor.md)  
[Pilvipalveluihin määrittäminen](cloud-services-how-to-configure.md)  
