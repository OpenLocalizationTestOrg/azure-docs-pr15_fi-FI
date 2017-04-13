<properties
   pageTitle="Parhaat käytännöt ohjelmistopäivitysten Microsoft Azure IaaS | Microsoft Azure"
   description="Tässä artikkelissa parhaita käytäntöjä Microsoft Azure IaaS ympäristössä ohjelmistopäivitysten kokoelma.  Se on tarkoitettu IT-ammattilaisille ja suojaus-analyytikot kuka käsitellä muuttaa ohjausobjektin, päivittäin, mukaan lukien vastuussa organisaation tietosuojaan ja vaatimustenmukaisuuteen ohjelmiston päivitys ja kohteiden hallinta."
   services="security"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/18/2016"
   ms.author="yurid"/>

# <a name="best-practices-for-software-updates-on-microsoft-azure-iaas"></a>Ohjelmistopäivitysten Microsoft Azure IaaS parhaat käytännöt

Ennen Sukeltava, mitä tahansa keskustelun kyselyjä-ympäristössä Azure [IaaS](https://azure.microsoft.com/overview/what-is-iaas/) parhaita käytäntöjä, on tärkeää ymmärtää käyttötavoista oikein, joilla voit hallinta ohjelmistopäivitykset ja vastuualueet. Seuraavassa kuvassa pitäisi auttavat ymmärtämään rajoja:

![Cloud-malleja ja asesulkua](./media/azure-security-best-practices-software-updates-iaas/sec-cloudstack-new.png)

Vasemmanpuoleisimmasta sarakkeesta näyttää seitsemän vastuut (määritetty tapoja, joilla), että organisaatiot kannattaa harkita, jotka kaikki vaikuttavat tietoturvaa ja yksityisyyttä tietojenkäsittely-ympäristöön.
 
Tietojen luokittelu ja vastuu ja asiakkaan ja läpikuultavuutta suojaus on Yleiset tehtävät, jotka ovat ainoastaan asiakkaiden toimialueen ja fyysisiä, Host (isäntä)- ja Yleiset tehtävät ovat cloud palveluntarjoajien PaaS ja SaaS-malleissa toimialueen. 

Jäljellä olevat tehtävät ovat yhteisiä asiakkaat ja cloud palveluntarjoajia. Jotkin vastuut edellyttävät CSP ja asiakkaan ja hallita vastuu yhteen, mukaan lukien niiden toimialueen tarkistaminen. Esimerkiksi harkitse käyttäjätietojen ja käyttää hallinta käytettäessä Azure Active Directory-palveluiden; monimenetelmäisen todentamisen palveluihin määritysten asiakkaan ylöspäin, mutta varmistaa, että tehokkaita toimintoja kuuluu Microsoft Azure.

> [AZURE.NOTE] Lisätietoja jaettujen vastuut pilveen lukea [Jaetun vastuu Cloud tietojenkäsittely](https://gallery.technet.microsoft.com/Shared-Responsibilities-81d0ff91/file/153019/1/Shared%20responsibilities%20for%20cloud%20computing.pdf) 

Näiden samojen periaatteiden käyttää hybrid-tilanteessa, jossa yrityksessäsi on käytössä, joka ilmoittaa Azure IaaS VMs paikallisen tiedoilla seuraavassa kuvassa esitetyllä tavalla.

![Tyypillinen hybrid skenaarion Microsoft Azure](./media/azure-security-best-practices-software-updates-iaas/sec-azconnectonpre.png)

## <a name="initial-assessment"></a>Alustava arviointi

Vaikka yritykselläsi on jo päivityksen hallinnan käyttämisestä koituvia ja sinulla on jo ohjelmiston päivityksen käytännöt paikassa, on tärkeää usein palata edelliseen käytännön arvioinnit tähän määritykseen ja päivittää ne nykyisen tarpeen mukaan. Tämä tarkoittaa, että sinun on tunnettava yrityksesi resurssien nykyisen tilan. Saavuttaa tässä tilassa, sinun on tiedettävä:

-   Yrityksen fyysinen ja virtuaalinen-tietokoneissa.

-   Käyttöjärjestelmät ja versioita käytössä kuhunkin fyysinen ja virtuaalinen näistä tietokoneista.

-   Saatavilla olevat ohjelmistopäivitykset asennettuna kussakin tietokoneessa (service pack-versioita, ohjelmistopäivitykset ja muut muutokset).

-   Funktion kussakin tietokoneessa, suorittaa yrityksen.

-   Sovellusten ja käyntiin kussakin tietokoneessa.

-   Jokaisessa tietokoneessa omistus- ja yhteyshenkilön tiedot.

-   Varat esittäminen ympäristön ja niiden suhteellinen arvo, voit selvittää, mitkä alueet on eniten huomiota ja suojaus.

-   Tunnetut suojaukseen liittyviä ongelmia ja prosessit yrityksessä on yksilöivä uusi suojaus-ongelmat tai suojaustaso muutokset paikassa.

-   Countermeasures, jotka on otettu suojata ympäristöäsi.

Päivitä tiedot säännöllisesti, ja se on helposti saatavilla ohjelmiston päivityksen hallinnan prosessin yhdistävää.

## <a name="establish-a-baseline"></a>Muodostaa perusaikataulun

Ohjelmiston päivitysprosessin hallinta tärkeä osa luodaan ensimmäisen vakio asennusten käyttöjärjestelmien versiot, sovellusten ja laitteiston tietokoneissa yrityksen; Näitä kutsutaan perusaikatauluja. Perusaikataulun on tuotteen tai muodostettu tiettynä ajankohtana järjestelmän kokoonpano. Sovelluksen tai käyttöjärjestelmä perusaikataulun, esimerkiksi tarjoaa mahdollisuuden tietokoneessa tai tietyn tila-palvelu uudelleen.

Perusaikataulujen perustan etsiminen ja korjaaminen mahdollisista ongelmista ja yksinkertaistaa ohjelmiston päivityksen hallinta, sekä vähentämällä ohjelmistopäivitykset, sinun on otettava käyttöön yrityksen järjestelmänvalvoja voi valvoa kasvattamalla.

Suorituksen yrityksen alkuperäinen valvonta jälkeen käytettävä tietoja, jotka saadaan tarkastuksen määrittämään toiminnallisia perusaikataulun, IT-komponentteja tuotannon-ympäristössä. Perusaikataulujen useita voivat olla tarpeen mukaan erityyppiset laitteiston ja ohjelmiston kyselyjä tuotannon käyttöön.

Esimerkiksi jotkin palvelimet edellyttävät ohjelmistopäivitys, jotta ne eivät kaatuu, kun he kirjoittavat sulkemisen, kun käytössä Windows Server 2012. Seuraavissa palvelimissa perusaikataulun Sisällytä tämä ohjelmistopäivitys.

Suurissa organisaatioissa on usein hyödyllistä säilyttää kussakin luokassa vakio perusaikataulun ohjelmistojen ja ohjelmistopäivitykset saman versioita käyttämällä ja jakamiseksi yrityksen tietokoneisiin resurssi luokkiin. Voit käyttää näitä resurssi luokat priorisointi ohjelmiston päivityksen jakauman.

## <a name="subscribe-to-the-appropriate-software-update-notification-services"></a>Tarvittavan ohjelman päivitystä ilmoituksen palvelujen tilaa

Kun olet suorittanut ensimmäisen tarkastus ohjelmiston käytössä yrityksen, Määritä kullekin ohjelmistotuotteita ja-version uudet ohjelmistopäivitykset ilmoitusten vastaanottaminen paras siirtomenetelmä. Paras tapa ilmoitus voi ohjelmistotuotteita, sähköposti-ilmoituksia, sivustoja tai tietokoneen julkaisuja.

Esimerkiksi Microsoftin Security vastauksen Center (MSRC) vastaa kaikkien Microsoftin tuotteiden tietoturvaan liittyvät epävarma ja tarjoaa Microsoft Security tiedotteen palvelun, vapaa sähköposti-ilmoituksen juuri tunnistettujen heikkouksien ja ohjelmistopäivitykset, jotka on julkaistu sähköpostiosoite näiden heikkouksien. Voit tilata tämän palvelun http://www.microsoft.com/technet/security/bulletin/notify.mspx.

## <a name="software-update-considerations"></a>Ohjelmiston päivitys huomioon otettavia seikkoja

Kun olet suorittanut ensimmäisen tarkastus ohjelmiston käytössä yrityksen, Määritä määrittäminen oman ohjelmiston päivityksen hallintajärjestelmän, joka määräytyy ohjelmiston päivityksen hallintajärjestelmän, jota käytät vaatimuksia. System Center jäljempänä WSUS lukea [Ja Windows Server Update Services parhaita käytäntöjä](https://technet.microsoft.com/library/Cc708536), [ohjelmistopäivitysten määritysten hallinnan suunnittelu](https://technet.microsoft.com/library/gg712696).

Välillä on kuitenkin joitakin yleistä ja parhaita käytäntöjä, joilla voit käyttää ratkaisu, jota käytät osat esitetyllä tavalla, joka seuraa riippumatta.

### <a name="setting-up-the-environment"></a>Ympäristön määrittäminen

Ota huomioon seuraavat käytännöt suunniteltaessa Määritä ohjelmiston asetukset Päivitä hallinta-ympäristöön:

-   **Luo tuotannon ohjelmiston päivityksen sivustokokoelmat vakaata ehtojen perusteella**: yleensä vakaata ehtojen käyttämisestä kokoelmien ohjelmiston päivityksen varaston ja jakelu luomiseen auttaa yksinkertaistamaan kaikissa vaiheissa ohjelmiston päivitysprosessin hallinta. Vakaata ehto voi olla asennettu käyttöjärjestelmäversio ja service pack-taso, järjestelmä roolin tai kohdeorganisaation.

-   **Luo edeltävien sivustokokoelmat, jotka sisältävät viittaus-tietokoneissa**: edeltävien sivustokokoelman projektityypin käyttöjärjestelmässä versioita, viivan business-ohjelmiston ja muiden ohjelmien käytössä yrityksen edustavan määrityksiä.

Ota huomioon myös, jos ohjelmiston päivityksen palvelimen sijoitetaan, jos se on Azure IaaS infrastruktuuri pilveen tai jos se on paikallisen. Tämä on tärkeää päätös, koska haluat arvioida liikennettä paikallisen resurssit ja Azure infrastruktuurin välillä. [Muodosta yhteys paikalliseen-verkon Microsoft Azure virtual verkkoon](https://technet.microsoft.com/library/Dn786406.aspx) lukea lisätietoja paikallisen infrastruktuurin yhdistämisestä Azure.

Vaihtoehtoja, jotka määrittävät, mistä päivitys-palvelin on myös vaihtelevat mukaan nykyisen infrastruktuurin ja ohjelmiston päivitys järjestelmän, jota käytät parhaillaan. Lue WSUS [Ottaminen käyttöön Windows Server Update Services-organisaatioon](https://technet.microsoft.com/library/hh852340.aspx) ja saat System Center määritysten hallinta Lue [suunnitteleminen sivustoja ja hierarkiat määritysten hallinta](https://technet.microsoft.com/library/Gg712681.aspx).

### <a name="backup"></a>Varmuuskopiointi

Säännöllisen varmuuskopioinnin suunnittelu ovat tärkeitä vain ei-ohjelmiston päivityksen hallinta ympäristön itse myös palvelimissa, jotka päivity. Organisaatiot, joilla on lisätty [muuttaminen hallinta](https://technet.microsoft.com/library/cc543216.aspx) vaatii IT-Tasaa syitä, miksi palvelin on päivitettävä, arvioitu käyttökatkot ja mahdolliset vaikutukset. Voit varmistaa, että peruutus-määritysten paikassa siltä varalta, että päivitys epäonnistuu, varmista, että järjestelmä varmuuskopioida säännöllisesti.

Azure IaaS varmuuskopion vaihtoehtoja ovat:

-   [Azure IaaS kuormituksen suojaus tietojen suojauksen hallinnan avulla](https://azure.microsoft.com/blog/2014/09/08/azure-iaas-workload-protection-using-data-protection-manager/)

-   [Azuren näennäiskoneiden varmuuskopiointi](../backup/backup-azure-vms.md)

### <a name="monitoring"></a>Seuranta

Suorita säännöllisesti raporttien seurannassa määrän puuttuu tai päivitykset ja päivitykset asennetaan keskeneräiseen tilan, kunkin ohjelmistopäivitys, joka on hyväksytty. Vastaavasti raportoinnin ohjelmistopäivitysten, joka ei ole vielä hyväksytty voi helpottaa helpompaa käyttöönoton päätöksiä.

Ota huomioon myös seuraavat toimet:

-   Järjestä tarkastus yrityksesi kaikissa tietokoneissa käytettävissä ja asennettava turvapäivitykset.

-   Määritä ja ota käyttöön haluamasi tietokoneet päivitykset.

-   Varaston seuranta ja Päivitä asennuksen tilan ja edistymisen kaikki yrityksesi tietokoneissa.

Lisäksi yleiset seikat, jotka on tässä artikkelissa kerrotaan, ota huomioon myös jokaisen tuotteen on paras harjoitella, esimerkiksi: Jos sinulla AM Azure SQL Serverin kanssa, varmista, että seuraavat ohjelmiston päivitykset suositus kyseistä tuotetta.

## <a name="next-steps"></a>Seuraavat vaiheet

Tässä artikkelissa kuvattuja ohjeita avulla voit helpottaa määrittämiseen parhaat vaihtoehdot ohjelmistopäivitysten näennäiskoneiden Azure IaaS kuluessa. On paljon yhtäläisyyksiä ohjelmiston päivityksen parhaita käytäntöjä perinteinen joten ja Azure IaaS välillä, on suositeltavaa, että arvioit oman nykyinen päivitys ohjelmiston-käytännöt, lisätä Azure VMs ja sisällyttää tästä artikkelista asiaa parhaita käytäntöjä yleinen ohjelmiston päivitysprosessin.
