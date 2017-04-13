
<properties 
    pageTitle="Käytön Azure RemoteApp ja lisäksi | Microsoft Azure"
    description="Lisätietoja siitä, miten suojattu käyttö Azure RemoteApp käyttämällä ehdollisen käyttöoikeuden Azure Active Directory"
    services="remoteapp"
    documentationCenter="" 
    authors="piotrci" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />

# <a name="securing-access-to-azure-remoteapp-and-beyond"></a>Käytön Azure RemoteApp, ja sen jälkeen

> [AZURE.IMPORTANT]
> Azure RemoteApp on on lopetettu. Lue lisätietoja [ilmoitus](https://go.microsoft.com/fwlink/?linkid=821148) .

Tässä artikkelissa on avulla yleiskatsaus siitä, miten järjestelmänvalvoja määrittää suojattu käyttö kanavan peruskäyttäjän kautta Azure RemoteApp alkamis- ja suojatun resurssi, kuten SQL-tietokanta tai toisen sovelluksen taustatietokantaan kanssa. Varmista, että haluamasi ehdot täyttävä vain valtuutettujen käyttäjien käyttää sovellusten ja että suojatun taustatietokantaan voi käyttää vain valvottu Azure RemoteApp-ympäristöstä ja muista sijainneista on.

3 järjestelmänvalvojan tarvitsemien katsomalla pääaluetta on:

![Azure RemoteApp ehdollisen käyttöoikeuden huomioon otettavia seikkoja](./media/remoteapp-secureaccess/ra-conditionalenvironment.png)

Valitse luku ja vastauksia seuraaviin kysymyksiin.

## <a name="who-can-access-the-collection"></a>Kuka voi käyttää kokoelman?
Järjestelmänvalvoja valitsee käyttäjät, jotka voivat käyttää sivustokokoelman sovellusten. Voit käyttää Azure Active Directory (Azure AD) työpaikan tai oppilaitoksen tilit (aiemmalta nimeltään, "organisaation tili") tai Microsoft-tili (kuten @outlook.com). Useimmat paranee Azure AD-tilien; käyttäminen niiden avulla voit käyttää ehdollisen käyttöoikeuden käsitellyt myöhemmin ja ovat myös vain valinta toimialueen liittyneet loppu. Loput on artikkelissa oletetaan, että käytät Azure AD-tilien kanssa Azure RemoteApp.

**Mikä on oltava tehdä:**

Azure AD-tilin käyttäminen hallintaa Azure RemoteApp avulla us kaksi asiaa:

1.  Olemme aina tietää, kuka voi käyttää sovelluksia syy on julkaistu ja käyttää kaikkia Edellinen päättyy näiden sovellusten yhdistäminen.
2.  Olemme hallita pohjana Azure AD, jotta voit luoda ja poista käyttäjätilejä, käytäntöjen määrittäminen salasanan, käytä monimenetelmäisen todentamisen jne. 

## <a name="how-is-the-collection-accessed-from-where"></a>Miten kokoelman käytetään? Mistä?
Tavallisesti järjestelmänvalvojat haluat määrittää jäsenyydelle käyttäminen julkisessa Internetiin yhteydessä oleva-ympäristössä, kuten Azure RemoteApp. Esimerkiksi he haluavat varmistaa, että käyttäjät ympäristön yritysverkon ulkopuolelta on monimenetelmäisen todentamisen (MFA) avulla voit käyttää; tai ehkä ne pitäisi olla estetty kokonaan.

Azure RemoteApp-järjestelmänvalvojat voivat käyttää kautta Azure AD Premium toimintoja ehdollisen käyttöoikeuden käytännöt niiden Azure RemoteApp-ympäristössä. He voivat myös käyttää monipuolisia raportointi ja ilmoitat ominaisuuksien seurannassa, miten ympäristön käytetään.

### <a name="how-to-set-up-conditional-access-for-azure-remoteapp"></a>Azure RemoteApp ehdollisen käyttöoikeuden määrittämisestä
Tarkastellaan käydä läpi esimerkkiskenaario – Azure-RemoteApp järjestelmänvalvoja haluaa ympäristön käytön estäminen, kun käyttäjät ovat yritysverkon ulkopuolelta.

>[AZURE.NOTE] Olemme oletetaan, että olet päivittänyt Azure AD Premium taso ja että olet luonut ainakin yksi Azure RemoteApp-sivustokokoelman.

1.  Valitse Azure portal **Active Directory** -välilehti. Valitse sitten kansio, jonka haluat määrittää.

    Muista, että Ehdollisen käyttöoikeuden on hakemistossa ja ei Azure RemoteApp-ominaisuus, jotta kaikki määritykset on tehty sivustotasolla. Tämä tarkoittaa sitä haluat hakemisto-järjestelmänvalvojat voivat tehdä nämä muutokset.

2.  Valitse **sovellukset**ja valitse sitten **Microsoft Azure RemoteApp** ehdollisen käyttöoikeuden määrittäminen. Huomaa, että voit määrittää ehdollisen käyttöoikeuden "ohjelmisto palveluna" kunkin sovelluksen hakemistossa erikseen.
![Azure RemoteApp ehdollisen käyttöoikeuden määrittäminen](./media/remoteapp-secureaccess/ra-conditionalaccessscreen.png)
 

3.  **Määritä** -välilehdessä Siirrä **Käyttöön käyttösäännöt** kohtaan käytössä.
![Käyttösäännöt käyttöön Azure RemoteApp](./media/remoteapp-secureaccess/ra-enableaccessrules.png)
 

4.  Voit määrittää eri säännöt ja valitse keneltä voi käyttää niitä:

    1. Valitse **Kun ei töissä käytön estäminen** kokonaan estää käyttäjien pääsyn Azure RemoteApp määrittämäsi Verkkoympäristössä ulkopuolella.
    2. Valitse seuraavista vaihtoehdoista Määritä, jotka muodostavat "luotettu verkon" IP-osoitealueet. Kaikki ulkopuolella ne hylätään.

5.  Testaa kokoonpanoa käynnistetään Azure RemoteApp-asiakas ulkopuolelle määritetty IP-osoite. Kun olet kirjautunut sisään Azure AD-tunnistetiedoilla pitäisi näkyä seuraavanlaisen sanoman:

![Azure RemoteApp pääsy estetty](./media/remoteapp-secureaccess/ra-accessdenied.png)
 

### <a name="future-conditional-access-features"></a>Tulevien ehdollisen käyttöoikeuden ominaisuudet 
Azure Active Directory-ryhmän suorittamaa enimmäistyömäärää ehdollisen käyttöoikeuden uudet ominaisuudet. Järjestelmänvalvojat voivat luoda uuden sääntötyyppien verkon ulkopuolella sijainnin perusteella sääntöjä. Uudet toiminnot julkisen esikatselun pitäisi olla käytettävissä pian.

### <a name="how-to-monitor-access-to-azure-remoteapp"></a>Voit valvoa Azure RemoteApp käyttöä
Hyvien ominaisuus käyttää ehdollisen käyttöoikeuden rinnalla on Azure Active Directory-Premium raportointi-toiminnon. Voit käyttää raportteja näyttö, joka on ympäristössäsi ja tunnistaa epäilyttävistä tapahtumista.

Kuten näet käyttää Azure RemoteApp, kuinka monta kertaa, se ollut niiden käyttäjien nimet ja milloin.

1.  Azure-portaalissa **Active Directory**ja valitse sitten hakemistossa.

2.  Siirry **Raportit** -välilehti.

3.  Valitse **sovellusten käyttö** -kohdassa **integroitujen sovellusten**raportit-luettelosta.

    Näyttöön tulee joitakin koostetun tilastoja Azure RemoteApp. 
![Koostetun Azure RemoteApp access tilasto](./media/remoteapp-secureaccess/ra-accessstats.png)
 
5.  Valitse sovellus, joka paljastaa tietoja käyttäville Azure RemoteApp.
![Käyttäjän access tilasto Azure RemoteApp varten](./media/remoteapp-secureaccess/ra-userstats.png)
 
### <a name="summary"></a>Yhteenveto
Azure Active Directory-Premium avulla voit määrittää käyttösäännöt Azure RemoteApp (ja muiden ohjelmien palvelusovellukset on saatavana Azure AD nimellä). Säännöt ovat tällä hetkellä rajoitettu verkon sijainnin perusteella käytännöt, mutta tulevaisuudessa koskemaan yrityksen johto muita ominaisuuksia.

Azure AD-Premium on myös raportointi ja seuranta ominaisuuksia, jotka ulottuvat edelleen ohjausobjektin järjestelmänvalvojalla on niiden Azure RemoteApp-ympäristössä.

## <a name="how-do-i-make-sure-my-secure-resource-is-accessible-only-from-my-azure-remoteapp-environment"></a>Miten voin varmistaa, etteivät Omat suojatun resurssi on käytettävissä vain Azure RemoteApp-ympäristössä?
Edellisten kohtien on tämän artikkelin kohdassa on kohdistettu käytön Azure RemoteApp-ympäristöön. Emme ole toteuttaa, valitseminen käyttäjät, jotka saavat access ja määrittämällä käyttösäännöt voit hallita, miten niitä voi käyttää palvelua.

Azure RemoteApp-versioiden käytetty vaihtoehto on, että sovellusten yhteydessä taustatietokantaan resurssi, kuten SQL-tietokantaan. Tämän resurssin nykyisessä joko paikallisesti (esimerkiksi yrityksen verkkoon) tai pilvipalveluun (esimerkiksi-Azure IaaS). Järjestelmänvalvojat haluta Varmista, että taustatietokantaa resurssi voi käyttää vain sovellusten käyttöön kautta Azure RemoteApp ja ei esimerkiksi sovelluksen käyttäjän tietokoneessa käynnissä ja käyttää julkisen Internetin kautta. Azure RemoteApp usein nähnyt keskitetysti hallittuja ja suojatun ympäristön ja näin ollen hyödyntämällä käyttäjien olisi käsitellä taustatietokantaan resurssin vain polku.

Ratkaisu on sijoittaa Azure RemoteApp-ympäristön ja suojatun resurssi-sama Azure Virtual verkon (VNET). Jos resurssi on eri sivustolle, voit muodostaa sivuston sivuston VPN-yhteyden, esimerkiksi VNet, jonka kesto on Azure tietokeskuksen ja asiakkaan paikallisen ympäristön luomiseen.

Azure RemoteApp tukee kahdentyyppisiä sivustokokoelman käyttöönotoissa, johon voidaan lisätä omia VNET:

-   Ei-toimialueeseen liittymistä: sovellukset, on VNET "rivin ovat näkyvissä" muita resursseja. Esimerkiksi Tämä voidaan muodostaa sovellusten SQL-tietokantaan, joka käyttää SQL-todennus (sovellusten todentaessaan käyttäjän suoraan tietokannan vastaan)

-   Toimialueen liittyneet: Azure RemoteApp käyttämä näennäiskoneiden liitettyinä toimialueen ohjauskoneen VNET. Tästä on hyötyä, kun sovellukset on todennetaan Windows toimialueen ohjauskoneen, jotta saat taustatietokantaan resurssin käyttöoikeutta.
![Toimialueen liittyneet kokoelman Azure RemoteApp](./media/remoteapp-secureaccess/ra-domainjoined.png)
 
### <a name="how-to-create-a-secure-connection-between-azure-and-my-on-premises-environment"></a>Azure- ja Omat paikallisen ympäristön välinen suojatun yhteyden luominen
Seuraavassa on kuvattu erilaisia määritysten yhteyden Azuren ja paikallisten ympäristöjen. Hyvä yleiskatsaus asetukset on saatavilla täällä.

Azure RemoteApp haluat määrittää oman VNet ensin ja käyttää sitä kokoelmaa luonnin aikana. 

## <a name="the-complete-solution"></a>Täydellinen ratkaisu
Seuraavassa kuvassa näkyy täydellinen ratkaisu kohtaa, johon on on valmiina suojattu käyttö kanavan peruskäyttäjän kautta Azure RemoteApp (ARA), Taustajärjestelmä resurssi.
![Suojattu Azure RemoteApp](./media/remoteapp-secureaccess/ra-secureoverview.png) -vaihe 1 on valittu käyttäjät ja käyttösäännöt, jotka määrittävät, miten niitä voi käyttää ARA luotu. Seuraavassa esimerkissä on vain Salli Accessin käyttäjille, jotka käyttävät yrityksen verkosta. Ei-yhteensopivan käyttäjät eivät pysty käyttämään ARA-ympäristössä.
"Vaiheessa 2" on on näkyvät Taustajärjestelmä resurssin vain VNet/VPN-määritys, joka on hallita kautta. Azure RemoteApp on sijoitettu samaan VNet. Lopputulos on se, että resurssi voi käyttää vain ARA-ympäristön avulla.


