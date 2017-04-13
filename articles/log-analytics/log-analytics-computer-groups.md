<properties
    pageTitle="Tietokoneen ryhmien Log Analytics Kirjaudu haut | Microsoft Azure"
    description="Tietokoneen ryhmien Log Analytics mahdollistavat tietokoneiden tietyt laajuus log hakuja.  Tässä artikkelissa kuvataan avulla voit luoda tietokoneen ryhmiä ja käyttämisestä log haun muilla tavoilla."
    services="log-analytics"
    documentationCenter=""
    authors="bwren"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="bwren"/>

# <a name="computer-groups-in-log-analytics-log-searches"></a>Tietokoneen ryhmien Log Analytics Kirjaudu haut
Tietokoneen ryhmien Log Analytics mahdollistavat laajuus [Kirjaudu haut](log-analytics-log-searches.md) tietyt tietokoneissa.  Kunkin ryhmän täytetään käyttämällä joko kyselyn, jossa voit määrittää tietokoneen kanssa tai tuomalla ryhmiä eri lähteistä.  Kun ryhmä sisältyy log haun, tulokset rajoittuvat ryhmän tietokoneet vastaavat tietueet.

## <a name="creating-a-computer-group"></a>Tietokoneen ryhmän luominen
Voit luoda tietokoneessa ryhmän Log Analytics käyttämällä tavoilla seuraavassa taulukossa.  Kummassakin menetelmässä tiedot on tarkoitettu alla olevissa osioissa. 

| Menetelmä | Kuvaus |
|:---|:---|
| Log-haku       | Log-haun, joka palauttaa tietokoneiden luettelon luominen ja tulosten tallentaminen tietokoneeseen ryhmänä. |
| Kirjaudu haun Ohjelmointirajapinta   | Log-haun Ohjelmointirajapinta avulla voit luoda ohjelmallisesti tietokoneen ryhmän log etsinnän tulokset perusteella. |
| Active Directory | Tarkista automaattisesti agentti tietokoneiden, jotka ovat Active Directory-toimialueen jäseniä ja Luo ryhmä Log Analytics kunkin käyttöoikeusryhmän Ryhmäjäsenyyden.
| WSUS              | Etsi WSUS-palvelimia tai asiakkaasi kohdistamisen ryhmiä ja Luo ryhmä Log Analytics kunkin automaattisesti. |


### <a name="log-search"></a>Log-haku

Tietokoneryhmät luotu loki haun sisältää kaikki haun, voit määrittää kyselyn palauttamien tietokoneissa.  Tämä kysely suoritetaan aina, kun tietokoneen ryhmän käytetään niin, että ryhmän luomisen jälkeen muutokset näkyvät myös.

Seuraavien ohjeiden avulla voit luoda tietokoneen ryhmän log tuloksista.

1. [Luo lokitiedoston haun](log-analytics-log-searches.md) , joka palauttaa luettelon tietokoneista.  Haku on palautettava eri joukon tietokoneita käyttämällä jotain, kuten **Eri tietokoneeseen** tai **mittayksikön count() tietokoneen** kyselyssä.  
2. Valitse näytön yläreunassa **Tallenna** -painiketta.
3. Valitse **Kyllä** **tämän kyselyn tallentaminen tietokoneeseen ryhmän:**.
4. Kirjoita **nimi** ja **luokan** ryhmän.  Jos haku on sama nimi ja luokka on jo olemassa, valitse voit pyydetään sen päälle.  Voit määrittää useita hakuja saman niminen eri luokkiin. 

Seuraavassa on esimerkki hakuja, jotka on tallennettu tietokoneen ryhmänä.

    Computer="Computer1" OR Computer="Computer2" | distinct Computer 
    Computer=*srv* | measure count() by Computer

### <a name="log-search-api"></a>Kirjaudu haun Ohjelmointirajapinta

Loki haun Ohjelmointirajapinta luodut tietokoneryhmät ovat samat kuin luotu loki haun haut.

Lisätietoja lokin haku-Ohjelmointirajapinnan käyttäminen tietokoneen ryhmän luominen kohdassa [tietokoneen ryhmien Log Analytics Kirjaudu haun REST API](log-analytics-log-search-api.md#computer-groups).

### <a name="active-directory"></a>Active Directory

Määrittäessäsi Log Analytics tuo Active Directory-ryhmäjäsenyyksiä se analyysi Ryhmäjäsenyyden kaikki toimialueelle liitetyissä tietokoneissa OMS edustajan kanssa.  Tietokoneen ryhmän luodaan loki Analytics kunkin käyttöoikeusryhmän Active Directoryn ja tietokoneen ryhmien jäsenyyksien mukaan käyttöoikeusryhmät vastaavat lisätään jokaisessa tietokoneessa.  Tämä jäsenyys päivitetään jatkuvasti 4 tunnin välein.  

Voit määrittää lokiin Analytics Active Directory-käyttöoikeusryhmät tuomisesta **Tietokoneryhmät** -valikosta Log Analytics- **asetukset**.  Valitse **Automaattiset** ja **Tuo Active Directory-ryhmäjäsenyyksiä tietokoneista**.  Ei ole tarvitaan lisämäärityksiä.

![Tietokoneen ryhmiin Active Directorysta](media/log-analytics-computer-groups/configure-activedirectory.png)

Ryhmät on tuotu, kun valikko on luettelo tietokoneissa, joissa Ryhmäjäsenyyden havaita määrän ja tuoda ryhmien määrän.  Voit valita joko on seuraavissa linkeissä palauttaa nämä ohjeet **ComputerGroup** -tietueet.

### <a name="windows-server-update-service"></a>Windows Update-palvelun

Määrittäessäsi Log Analytics tuo WSUS ryhmäjäsenyyksiä se analysoida kohdistuksen Ryhmäjäsenyyden minkä tahansa tietokoneissa, joissa OMS-agentti.  Jos käytössäsi on asiakkaan kohdistamisen, tietokoneeseen, jossa on yhdistetty OMS ja ei mitään WSUS kohdistamisen ryhmät on sen Ryhmäjäsenyyden Log Analytics tuoda. Jos käytössäsi on palvelinpuolen kohdistamisen OMS agentti on asennettava, jotta ryhmän jäsenyyden tietoja tuodaan OMS WSUS-palvelimessa.  Tämä jäsenyys päivitetään jatkuvasti 4 tunnin välein. 

Voit määrittää lokiin Analytics Active Directory-käyttöoikeusryhmät tuomisesta **Tietokoneryhmät** -valikosta Log Analytics- **asetukset**.  Valitse **Active Directory** ja **Tuo Active Directory-ryhmäjäsenyyksiä tietokoneista**.  Ei ole tarvitaan lisämäärityksiä.

![Tietokoneen ryhmiin Active Directorysta](media/log-analytics-computer-groups/configure-wsus.png)

Ryhmät on tuotu, kun valikko on luettelo tietokoneissa, joissa Ryhmäjäsenyyden havaita määrän ja tuoda ryhmien määrän.  Voit valita joko on seuraavissa linkeissä palauttaa nämä ohjeet **ComputerGroup** -tietueet.

## <a name="managing-computer-groups"></a>Tietokoneen ryhmien hallinta

Voit tarkastella tietokoneen ryhmiä, jotka on luotu loki haun tai lokin haun Ohjelmointirajapinta **Tietokoneryhmät** -valikosta Log Analytics- **asetukset**.  Valitse **x** , voit poistaa tietokoneen-ryhmän **Poista** -sarakkeessa.  Napsauta Suorita ryhmän lokista etsintää, joka palauttaa sen jäsenet ryhmän **jäsenten tarkasteleminen** -kuvaketta. 

![Tietokoneeseen tallennetun ryhmät](media/log-analytics-computer-groups/configure-saved.png)

Jos haluat muokata ryhmän, on sama **luokka** ja **nimi** korvaa alkuperäisen ryhmän uuden ryhmän luominen.

## <a name="using-a-computer-group-in-a-log-search"></a>Tietokoneen ryhmän käyttäminen log-haku
Lokitiedoston haun tietokoneen ryhmän viitata seuraavaa syntaksia avulla.  Jos sinulla on tietokoneen-ryhmissä, joissa on sama nimi eri luokkien määrittäminen **luokka** on valinnainen pakollinen. 

    $ComputerGroups[Category: Name]

Haun suoritettaessa haussa mukana tietokoneen ryhmiä jäsenet ensin ratkaista.  Jos ryhmän perustuu log haun, että haku suoritetaan palauttaa ryhmän jäsenten ennen ylimmän tason log hakeminen.

Tietokoneen ryhmiä käytetään yleensä log haun seuraavan esimerkin **IN** -lause.

    Type=UpdateSummary Computer IN $ComputerGroups[My Computer Group]

## <a name="computer-group-records"></a>Tietokoneen ryhmitellä tietueita

Kunkin tietokoneen jäsenyys luotu Active Directorysta tai WSUS OMS säilö luodaan uusi tietue.  Nämä tietueet on eräänlainen **ComputerGroup** ja sen ominaisuudet ovat seuraavassa taulukossa.  Tietueet luodaan tietokoneen ryhmien log haut perusteella.

| Ominaisuus | Kuvaus |
|:--|:--|
| Tyyppi                | *ComputerGroup* |
| SourceSystem        | *SourceSystem*  |
| Tietokoneen            | Jäsenen nimi. |
| Ryhmän               | Ryhmän nimi. |
| GroupFullName       | Ryhmän, mukaan lukien lähde- ja tietolähteen nimi koko polku.
| GroupSource         | Tietolähteen ryhmään on koottu. <br><br>Active Directory<br>WSUS<br>WSUSClientTargeting |
| GroupSourceName     | Tietolähde, jota ryhmät on kerätty nimi.  Tämä on Active Directory-toimialuenimi. |
| ManagementGroupName | SCOM tekijöiden hallinta-ryhmän nimi.  Muiden tekijöiden tämä on AOI -\<työtilan tunnus\> |
| TimeGenerated       | Päivämäärän ja kellonajan tietokoneryhmä on luotu tai päivitetty. |



## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja [lokin haut](log-analytics-log-searches.md) tietolähteet ja ratkaisujen kerättyjen tietojen analysointia varten.  