<properties
   pageTitle="Azure arkkitehtuuri viittaus - IaaS: Ulottuu Azure Active Directory | Microsoft Azure"
   description="Miten toteuttamisesta suojatun hybrid verkko-arkkitehtuuri, Active Directory-todennus ja Azure-tietokannassa."
   services="guidance,vpn-gateway,expressroute,load-balancer,virtual-network,active-directory"
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/19/2016"
   ms.author="telmos"/>

# <a name="extending-active-directory-directory-services-adds-to-azure"></a>Ulottuu Azure Active Directory-palveluihin (Lisää)

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

Tässä artikkelissa kerrotaan parhaita käytäntöjä laajentaminen ympäristössäsi Active Directory (AD) Azure tarjoamaan käyttämällä [Active Directory-toimialueen palveluista (AD DS)]hajautettu todennuspalvelujen[active-directory-domain-services]. Tämä arkkitehtuuri laajentaa on artikkeleissa [käyttöönoton suojatun hybrid-verkoston arkkitehtuuri Azure-tietokannassa] kuvattua[ implementing-a-secure-hybrid-network-architecture] ja [toteuttaminen suojatun hybrid verkko-arkkitehtuuri Internet-yhteyden Azure][implementing-a-secure-hybrid-network-architecture-with-internet-access].

> [AZURE.NOTE] Azure on kaksi eri käyttöönoton mallit: [Resurssienhallinta] [ resource-manager-overview] ja perinteinen. Viite-arkkitehtuuri käyttää Resurssienhallinta, johon Microsoft suosittelee uuden käyttöönotoissa.

Käyttäjätietojen todentaa AD DS: N avulla. Nämä käyttäjätietojen voivat kuulua käyttäjien, tietokoneiden, sovelluksiin tai muita resursseja, jotka ovat osa suojaus-toimialueeseen. Voit ylläpitää AD DS: N paikalliseen, mutta jos sovelluksen osat sijaitsevat Azure hybrid tilanne voi olla tehokkaampaa replikoida toiminto ja AD-tietovarasto pilveen. Tämän menetelmän voit vähentää viive aiheuttaa lähettämällä todennus ja AD DS: ÄÄN käynnissä paikallisen takaisin paikalliseen pyytää pilvestä. 

Azure-tietokannassa käytössäsi hakemistopalvelujen isännöimiseen kahdella tavalla:

1. Voit käyttää [Azure Active Directory (AAD)] [ azure-active-directory] voit luoda uuden AD-toimialueen pilveen ja linkittää sen paikallisen AD-toimialue. Määritä [Azure AD Connect] [ azure-ad-connect] --promises replikointia käyttäjätiedot säilytetään paikallisen säilön pilveen. Huomaa, että pilveen kansio ei **ole** tiedostotunnistetta paikalliseen järjestelmään, mieluummin kopio, joka sisältää saman käyttäjätietojen on. Nämä käyttäjätietojen pilveen, mutta cloud **ei** tehdyt muutokset kopioidaan paikallisen tehdyt muutokset replikoida takaisin paikallisen toimialueen. Salasanan vaihtamisesta on esimerkiksi voidaan suorittaa paikallisen ja kopioida muutoksen pilveen Azure AD Connect avulla. Huomaa myös, että AAD samassa esiintymässä voidaan linkittää useamman kuin yhden esiintymän AD DS: ÄÄN; AAD sisältää kunkin AD-säilön, johon se on linkitetty käyttäjätietoja.

    AAD on hyötyä tilanteissa, jossa paikallisen verkko- ja Azure virtual verkon isännöinnin cloud resurssit ei ole suoraan linkitetty VPN-tunnelin tai ExpressRoute piiri avulla. Tämä ratkaisu on yksinkertaista, mutta sitä ei välttämättä sopivan Systemsin missä osat voisit siirtää--paikallisen/cloud rajan yli, kun tämä saattaa edellyttää AAD uudelleenmääritys. Lisäksi AAD käsittelee vain käyttäjän Todentamisella tietokoneen todennus sijaan. Jotkin sovellukset ja -palvelut, kuten SQL Server saattaa edellyttää tietokoneen todennus jolloin tätä ratkaisua ei ole sopiva.

2. Voit ottaa käyttöön AM käynnissä toimialueen ohjauskoneen Azure AD DS: ÄÄN, ulottuu olemassa olevan AD-infrastruktuurin paikallisen palvelinkeskukseen. Tämä vaihtoehto on yleisimpiä tilanteissa, joissa VPN-ja/tai ExpressRoute yhteyden yhdistää paikallisen verkko- ja Azure virtual verkon. Tämä ratkaisu tukee myös kaksisuuntaisen replikoinnin, joten voit tehdä muutoksia cloud ja paikallisesta-on aina, kun se on sopivin. Suojaus, tarpeen mukaan pilveen AD-asennuksen voi olla:
    - osa kyseisen vastuuseen paikallisen samaan toimialueeseen
    - Uusi toimialueessa jaetun metsää
    - erillinen metsää

<!-- we might want to add info on how to choose from the three options above -->

Tämä arkkitehtuuri ohjeessa on saman AD DS-toimialue Azure ja paikallisen ratkaisun 2.

Tavallisesti tämä arkkitehtuuri laatikkomäärät ovat seuraavat:

- Hybrid sovellukset missä työmääriä Suorita osittain paikallisen ja osittain tekstimuodossa Azure.

- Sovellusten ja palveluiden, joka suorittaa todennusta käyttämällä Active Directory.

## <a name="architecture-diagram"></a>Arkkitehtuurikaavio

Seuraavassa kaaviossa korostaa tärkeitä osia tämän arkkitehtuuri (*Lähennä*). Lue lisätietoja harmaana ulos-osista [käyttöönoton suojatun hybrid-verkoston arkkitehtuuri Azure-tietokannassa] [ implementing-a-secure-hybrid-network-architecture] ja [toteuttaminen suojatun hybrid verkko-arkkitehtuuri Internet-yhteyden Azure][implementing-a-secure-hybrid-network-architecture-with-internet-access]:

[! [0]][0]

- **Paikalliseen verkkoon.** Paikallisen verkon sisältää paikalliset AD-palvelimet sallitut toiminnot todennus- ja osat, jotka sijaitsevat paikalliset.

- **AD-palvelimiin.** Nämä ovat toimialueen ohjaimet käyttöönoton hakemistopalvelujen (AD DS) käynnissä VMs pilveen. Seuraavissa palvelimissa voi todentaa Azure virtual verkostosi-osaa.

- **Active Directory-aliverkon.** Erillinen aliverkon isännöidään AD DS-palvelimiin. NSG säännöt suojaa AD DS-palvelimet, ja se voi tarjota palomuuri vastaan odottamattomia lähteistä.

- **Azure yhdyskäytävän ja AD-synkronointi.**. Azure yhdyskäytävä on paikalliseen verkkoon ja Azure VNet välinen yhteys. Tämä voi olla [VPN-yhteyden] [ azure-vpn-gateway] tai [Azure ExpressRoute][azure-expressroute]. Kaikki synkronointipyyntöihin pilveen AD-palvelimien väliltä paikallisen kulkevat yhdyskäytävän. Käyttäjän määrittämät tiet (UDRs) käsitellä reititys synkronoinnin tietoliikenteen, joka välittää suoraan pilvipalvelussa AD-palvelin ja läpäise olemassa olevan verkoston virtual laitteiden (NVAs) käytetään tässä skenaariossa.

    Katso lisätietoja määrittämisestä UDRs ja NVAs [käyttöönoton suojatun hybrid-verkoston arkkitehtuuri Azure-tietokannassa][implementing-a-secure-hybrid-network-architecture].

## <a name="recommendations"></a>Suosituksia

Tässä osassa on yhteenveto suosituksia AD DS: ÄÄN Azure-tietokannassa, joka kattaa:

- AM suosituksia.

- Verkko-suosituksia.

- Suojausta koskevia suosituksia. 

- Active Directory-sivuston suosituksia.

- Active Directory FSMO sijaintia suosituksia.

>[AZURE.NOTE] Asiakirjan [käyttöönotto Windows Server Active Directory-Azuren näennäiskoneiden ohjeet] [ ad-azure-guidelines] sisältää monia nämä tarkempia tietoja.

### <a name="vm-recommendations"></a>AM suositukset

Voit luoda VMs käsitellään liikenteen odotettu äänenvoimakkuuden riittävät resurssit. Käytä isännöinnin AD DS paikallinen lähtökohtana koneet kokoa. Resurssien; käytön valvominen Voit VMs kokoa ja skaalata, jos ne ovat liian suuri. Saat lisätietoja koon AD DS: N toimialueen ohjaimet [Kapasiteetin suunnitteleminen Active Directory-toimialueen palveluista][capacity-planning-for-adds].

Erillinen virtual tietojen DVD-levyllä projektiin tietokannan, lokit ja SYSVOL kuvasarjan luominen Älä tallenna nämä kohteet käyttöjärjestelmän levylle. Huomaa, että käytetään oletusarvon mukaan tiedot-levyjä, jotka on liitetty AM yksisuuntaisen kirjoituksen välimuisti. Kuitenkin tämän lomakkeen välimuistiin tallentamisen voivat olla ristiriidassa AD DS: ÄÄN vaatimusten mukainen. Tästä syystä määrittää *Host välimuistin* kieliasetus *ei mitään*tietoja levylle. Lisätietoja on artikkelissa [Windows Server AD DS tietokannan ja SYSVOL sijaintia][adds-data-disks].

Ota käyttöön vähintään kaksi VMs käynnissä AD DS: N toimialueen ohjaimet Azure virtual verkkoon [käytettävyys](#Availability-considerations) syistä.

### <a name="networking-recommendations"></a>Verkko-suositukset

Määrittää isännöinnin AD DS staattinen yksityinen IP-osoitteita VMs Verkkokäyttöliittymän. Tässä määrityksessä tukee DNS paremmin kunkin AD-VMs. Lisätietoja on artikkelissa [määrittämisestä staattinen yksityinen IP-osoite Azure-portaalissa][set-a-static-ip-address].

> [AZURE.NOTE] Anna AD DS VMs julkiseen IP-osoitteet. Katso [suojausasiat] [ security-considerations] lisätietoja.

Varmista, että liikenne flow pilveen AD-palvelimien väliltä paikallisen vapaasti:

- Lisää NSG säännöt, jotka sallia saapuvan liikenteen paikallisen AD aliverkon. Saat yksityiskohtaiset tiedot, jotka käyttävät AD DS: N portit, [Active Directory- ja Active Directoryn toimialueen palveluiden portin vaatimukset][ad-ds-ports].

- Varmista, että UDR taulukoiden ei reitittää käytetään tässä skenaariossa NVAs AD DS: ÄÄN liikenne. Oman käyttöönotoissa sen mukaan, mitä NVAs käytät haluat ehkä uudelleenohjaaminen liikenne.

### <a name="security-recommendations"></a>Suojausta koskevia suosituksia

AD DS palvelinten täyttökahva todennus ja ovat näin ollen erittäin kohteet. Estä suoraan näyttäminen AD DS-palvelimiin Internetissä. Aseta erillisessä aliverkon oma palomuuri AD DS-palvelimiin. Varmista, että tarvitse käyttää AD DS: N todennus- ja ja synchronzing palvelimien portit on avoinna, mutta kaikki muut portit Sulje. Lisätietoja on artikkelissa [Active Directory- ja Active Directoryn toimialueen palveluiden portin vaatimukset][ad-ds-ports]. [Ratkaisun osien] [ solution-components] näkyvät jäljempänä tässä asiakirjassa-osassa NSG säännöt, jota näyte käyttää Avaa portit.

NSG sääntöjen avulla voit luoda yksinkertaisen palomuuri. Jos tarvitset monipuolisemman suojaus voit ottaa käyttöön lisäsuojauksen kehyksen reunan palvelinten ympärille käyttämällä aliverkosta ja NVAs, [käyttöönoton suojatun hybrid verkko-arkkitehtuuri Internet-yhteyden Azure]asiakirja kuvatulla[implementing-a-secure-hybrid-network-architecture-with-internet-access].

Levyn isännöinnin AD DS-tietokannan salaaminen BitLocker tai Azure salauksen avulla.

### <a name="active-directory-site-recommendations"></a>Active Directory-sivuston suositukset

Voit käyttää AD DS: ään, ryhmän yhdessä toimialueen ohjaimet, jotka ovat yhteydessä nopea linkin sivustot. Samassa AD DS-sivustossa toimialueen ohjaimet replikoida directory muutoksensa automaattisesti ja vähän määritysten tarvitsemia replikointi.

VOIT määrittää replikoinnin liikenteen Azure ja paikallisen palvelinkeskuksen välillä, kannattaa erillisessä AD DS: ÄÄN sivuston edustavan Azure käyttämä osoitetilaa lisääminen. Voit määrittää linkin paikallisen oman välillä AD DS: ÄÄN sivustot ja hallita sivustojenvälisen replikoinnin entistä tehokkaammin.

Voit myös käyttää eri ryhmäkäytännön ryhmäkäytäntöobjekteja liittyneet tietokoneet Azure ja sovelluksia, jotka ovat "sivuston tiedä", kuten System Center määritysten hallinta hyödyntää sivuston erottaminen.

### <a name="active-directory-fsmo-placement-recommendations"></a>Active Directory FSMO sijaintia suositukset

Joustava toiminnon FSMO (Single Master)-palvelimia erityinen toimialueen ohjaimet, tietojen yhdenmukaisuuden koko erilaisia vaihtoehtoja AD DS: ään, alla reposnsible.

- **Perustyylin rakenne**. Tämä on metsää laajuinen rooli määrää ylläpitää AD DS: N käyttämän mallin rakenne.

- **Toimialueen nimeäminen perustyyli**. Tämä on metsää laajuinen rooli, joka hallitsee toimialuenimet metsää sisällä tietoja.

- **Ensisijainen toimialue pääohjauskonetta**. Tämä on toimialueen laajuinen rooli. PDC käsittelee salasanan päivitykset ja tilin lukitsemisten. Se on kuulla muut ohjauskoneet kun palvelupyyntöjä todennus on pariton salasanoja. PDC myös käsittelee ryhmäkäytännön päivitykset, ja kohde Ohjauskoneen vanhojen sovellusten ja jotkin hallintatyökalut, joka suorittaa joitakin kirjoitettavat.

- **Suhteellinen tunnus (RID) perustyyli**. RIDs käytetään yksilöivät hakemiston objektien avulla. Tämä palvelin on vastuussa luodaan koko toimialueen resurssivarantoon RIDs käytettäväksi kaikki AD-palvelimet toimialueella.

- **Infrastruktuurin rooli**. Objektin yhden toimialueen voi viitata objektin toisen toimialueen. Koko toimialueen tällä roolilla on vastuussa objektin SUOJAUSTUNNUS ja toimialueiden objektin viittauksessa DN-nimi. Huomaa, että palvelimen käyttöönoton tämä rooli ei voi myös edustajana Yleinen luettelo (yleisen-luettelon)-palvelimeen.

Lisätietoja [Active Directory-FSMO roolit Windowsin][AD-FSMO-roles-in-windows].

Tässä skenaariossa Suosittelemme vältät käyttöönotossa FSMO-roolien Azure toimialue-ohjaimet. 

## <a name="availability-considerations"></a>Käytettävyys huomioon otettavia seikkoja

Luo saatavuus määrittäminen AD-palvelimiin. Varmista, että joukon ovat vähintään kaksi palvelimiin. AD-palvelimet pilveen on oltava samaa toimialuetta toimialueen ohjaimiin. Tämä ottaa käyttöön automaattisen replikoinnin palvelinten välillä.

Ota huomioon myös nimeävät palvelimet kuin [valmiustilassa operations Master] [ standby-operations-masters] siltä varalta, että yhteys palvelimeen, visuaalisessa muodossa FSMO-roolin epäonnistuu.

## <a name="security-considerations"></a>Suojausasiat

Jos kaikki toimialueen hallintatehtävät eivät todennäköisesti suoritettavat paikallisen ohjauskoneet, harkitse tekemistä ohjauskoneita pilveen, vain luku-tilassa. Vain luku-Ohjauskoneen vain ylläpitää alijoukkoa käyttäjien tunnistetiedot (tarpeeksi todennuksella paikallisesti), ja voidaan määrittää välimuistin tietoja vain tietyille käyttäjille. Sen vuoksi, jos vain luku-Ohjauskoneen on ongelmia, välimuistissa käyttäjien tietoja vaikuttaako, sen sijaan, että jokaisen tilin toimialueen tietoja. Lisätietoja on artikkelissa [Vain luku-toimialueen ohjaimet][read-only-dc].

Yksittäisten käyttäjätilien haavoittuvuus ajaksi häiriöiden ja estää break-in yrittää noudattamalla parhaat käytännöt ja ylläpito käyttäjien salasanat AD. Lisätietoja on artikkelissa [Pakotetut salasanakäytännöt parhaat Käytännö][best_practices_ad_password_policy]. Varmista myös, ettet käyttäjäryhmät, voit määrittää käyttäjät. Esimerkiksi Älä tee tavalliset käyttäjät yrityksen Järjestelmänvalvojat-ryhmän, rakenteen valvojat-ryhmän ja Toimialueenvalvojat-ryhmän jäsenet.

## <a name="scalability-considerations"></a>Skaalattavuus huomioon otettavia seikkoja

AD on automaattisesti skaalattava toimialueen ohjaimet, jotka ovat osa samaa toimialuetta. Pyynnöt jakautuu kaikkien ohjainten toimialueella. Voit lisätä toisen toimialueen ohjauskoneen, ja se synkronoi automaattisesti toimialueen kanssa. Erillinen kuormituksen ohjaamaan liikenne paikalliseen ohjaimet toimialueessa ei määritetä. Varmista, että kaikki toimialueen ohjaimet on tarpeeksi muistia ja tallennustilaa resurssien käsittelemään toimialueen tietokantaan. muuttaa kaikki toimialueen ohjauskoneen VMs Ihannetapauksessa samankokoisiksi.

## <a name="management-considerations"></a>Hallinnan huomioon otettavia seikkoja

Kopioi näennäiskiintolevytiedostoja, toimialueen ohjaimet sijaan suorittamiseen säännöllisen varmuuskopioinnin suunnittelu, koska palauttamisesta voi aiheuttaa ristiriitoja tilaan toimialueen ohjainten välille.

Sulje ja Käynnistä uudelleen AM, joka suoritetaan toimialueen ohjauskoneen rooli Azure Vieras-käyttöjärjestelmässä Azure-portaalissa Sammuta-vaihtoehdon sijaan. Voit olla Varaus poistettu AM AM Sammuta Azure-portaalin avulla valitaan. Tämä toiminto palauttaa AM GenerationID, joka on ei-toivottujen toimialueen ohjauskoneen. Kun AM-GenerationID on palautettu, AD-tietovarasto invocationID myös Palauta, RID-varanto hylätään ja SYSVOL on merkitty vain ei-tärkeitten.

## <a name="monitoring-considerations"></a>Huomioon otettavia seikkoja seuranta

Valvonnan sekä ylläpidon AD-palvelimien verkon kaatuvat saattaa aiheuttaa ongelmia seuraavasti:

- **Kirjautumisvirhe**. Kirjautumisvirhe voi johtua siitä koko toimialueen tai metsää luota suhteen tai nimi selvitys epäonnistuu tai Yleinen luettelo-palvelin ei voi määrittää yleinen Ryhmäjäsenyyden.

- **Tilin lukitus**. Käyttäjä-ja voit lukita Jos ensisijaisen ohjauskoneen ei ole käytettävissä tai replikointi epäonnistuu useita toimialueen ohjainten välille.

- **Toimialueen ohjauskoneen virheen takia**. Jos levytila loppuu Ntds.dit tiedoston sisältävä asema, toimialueen ohjauskoneen lopettaa toiminnan.

- **Sovelluksen virhe**. Sovellukset, jotka ovat kriittisiä liiketoiminnallesi, kuten Microsoft Exchange tai muulla sähköpostisovelluksella saattaa epäonnistua, jos osoite kirjan kyselyt-kansioon.

- **Epäyhtenäinen kansion tiedot**. Jos replikointi epäonnistuu pitkäksi aikaa, objektien kaksoisarvot voidaan luoda hakemiston ja saattaa edellyttää laaja vianmäärityksen ja aikaa poistamista varten.

- **Tilin luominen epäonnistui**. Toimialueen ohjauskoneen ei voi luoda käyttäjän tai tietokoneen tilit, jos se kuluttaa kaikki kokeneelle suhteellinen tunnuksista ja RID perustyyli ei ole käytettävissä.

- **Suojauksen käytännön virhe**. Jos SYSVOL jaettua kansiota ei replikoida oikein, Ryhmäkäytäntö objektit ja suojauskäytäntöjä ei ole oikein käytetään asiakkaat.

Seurata AD-palvelimet huolellisesti ja Valmistaudu tulevat korjaavat toimenpiteet nopeasti. Seurannan tehtäviä, jotka suoritetaan tarvittavat välein tarkistusluettelon luomista varten. Esimerkiksi seuraavat tärkeät toimet voi ajoittaa päivittäin:

- Tutki ja ratkaista ilmoitusten ilmoittaa toimialueen ohjaimet 

- Varmista, että kaikki toimialueen ohjaimet voivat viestiä ja, että replikointi toimii.

- Varmistaa, että SYSVOL on jaettu.

- Itseään aika synkronointiongelmia.

- Tarkista AD käyttämä fyysiset asemat levytilasta.

Muita tavallisia tehtäviä vähemmän voinut suorittaa usein. Voi esimerkiksi Tarkista luottamussuhteet ja tarkista vanhentunut tai poistettu luottamussuhteet viikoittain ja varmista, että kaikki toimialueen ohjaimet suoritetaan käyttämällä samaa service Pack-paketit ja kuuma korjaus korjaukset kuukausittain.

Lisätietoja on artikkelissa [Seuranta Active Directory][monitoring_ad]. Voit asentaa työkaluilla, kuten [Microsoft Systems Centerin] [ microsoft_systems_center] seuranta-palvelimessa (katso [arkkitehtuuri kaavion][architecture]) avulla näistä tehtävistä. 

## <a name="solution-components"></a>Ratkaisun osat

Ratkaisu tarkoitetun tämän arkkitehtuuri Luo suojatun hybrid verkon [käyttöönoton suojatun hybrid-verkoston arkkitehtuuri Azure-tietokannassa] asiakirjoilla kuvatulla[ implementing-a-secure-hybrid-network-architecture] ja [toteuttaminen suojatun hybrid verkko-arkkitehtuuri Internet-yhteyden Azure][implementing-a-secure-hybrid-network-architecture-with-internet-access], mutta lisäksi seuraavat:

- Azure resurssiryhmä nimeltä *basename*- dns-rg, missä *basename* etuliite voit määrittää, kun otat ratkaisun.

- Kaksi Azure VMs kutsutaan *basename*-ad1-AM ja *basename*-ad2-AM, luotu *basename*- dns rg resurssiryhmä. Nämä VMs on määritetty hakemistopalvelujen ja asentanut ja määrittänyt DNS AD-palvelimiin.

- NSG nimeltä *basename*-ad- *basename*- ntwk rg Azure resurssiryhmä nsg. Tämä resurssiryhmä kuuluu, jotka muodostavat suojatun hybrid verkon infrastruktuuri, mutta uusi NSG on lisäys, joka määrittää saapuvien suojaussäännöt AD-palvelimia seuraavassa taulukossa kuvatulla tavalla:


    Priority (prioriteetti)|Nimi|Lähde|Kohde|Palvelun|Toiminto|
    --------|----|------|-----------|-------|------|
    170|vnet port53|10.0.0.0/16|Mikä tahansa|Custom(any/53)|Salli|
    180|vnet port88|10.0.0.0/16|Mikä tahansa|Custom(any/88)|Salli|
    190|vnet port135|10.0.0.0/16|Mikä tahansa|Custom(any/135)|Salli|
    200|vnet-,-port137-9|10.0.0.0/16|Mikä tahansa|Custom(any/137-139)|Salli|
    210|vnet port389|10.0.0.0/16|Mikä tahansa|Custom(any/389)|Salli|
    220|vnet port464|10.0.0.0/16|Mikä tahansa|Custom(any/464)|Salli|
    230|vnet-,-rpc-dynaaminen|10.0.0.0/16|Mikä tahansa|Custom(any/49152-65535)|Salli|
    240|onprem-ad-,-port53|192.168.0.0/24|Mikä tahansa|Custom(any/53)|Salli|
    250|onprem-ad-,-port88|192.168.0.0/24|Mikä tahansa|Custom(any/88)|Salli|
    260|onprem-ad-,-port135|192.168.0.0/24|Mikä tahansa|Custom(any/135)|Salli|
    270|onprem-ad-,-port389|192.168.0.0/24|Mikä tahansa|Custom(any/389)|Salli|
    280|onprem-ad-,-port464|192.168.0.0/24|Mikä tahansa|Custom(any/464)|Salli|
    290|rdp hallintoraportointi salliminen|10.0.0.128/25|Mikä tahansa|Custom(any/3389)|Salli|
    300|yhdyskäytävän salliminen|10.0.255.224/27|Mikä tahansa|Custom(any/any)|Salli|
    310|Salli itse|10.0.255.192/27|Mikä tahansa|Custom(any/any)|Salli|
    320|vnet-estä|Mikä tahansa|Mikä tahansa|Custom(any/any)|Salli|

    AD DS: ÄÄN Hyväksy saapuva replikoinnin ja tietoliikenteen portit 53, 89, 135, 389 ja 464 avulla. Tässä taulukossa paikallisen toimialueen ohjauskoneen on osoite tilaa 192.168.0.0/24 (osoitetilaa varten voivat vaihdella – voit määrittää tämän tiedon parametrina ratkaisun käyttämät malleja.

    NSG määrittää myös seuraavat lähtevien suojaussäännöt, jotka käyttöön synkronointi ja todennus-liikenne paikalliseen flow takaisin paikalliseen verkkoon:

    Priority (prioriteetti)|Nimi|Lähde|Kohde|Palvelun|Toiminto|
    --------|----|------|-----------|-------|------|
    100|ulos port53|Mikä tahansa|192.168.0.0/24|Custom(any/53)|Salli|
    110|ulos port88|Mikä tahansa|192.168.0.0/24|Custom(any/88)|Salli|
    120|ulos port135|Mikä tahansa|192.168.0.0/24|Custom(any/135)|Salli|
    130|ulos port389|Mikä tahansa|192.168.0.0/24|Custom(any/389)|Salli|
    140|ulos-portti 445|Mikä tahansa|192.168.0.0/24|Custom(any/445)|Salli|
    150|ulos port464|Mikä tahansa|192.168.0.0/24|Custom(any/464)|Salli|
    160|ulos-rpc-dynaaminen|Mikä tahansa|192.168.0.0/24|Custom(any/49152-65535)|Salli|

Komentosarja ratkaisun mukana myös suorittaa seuraavia tehtäviä:

- Se Lisää *basename*-ad1-AM ja *basename*-ad2-toimialueen ohjaimet toimialueen AM palvelimeen. Voit tarkastella paikallisen toimialueen ohjauskoneen *Active Directoryn käyttäjät ja tietokoneet* konsoli seuraavissa palvelimissa:

![[1]][1]

- Se luo uusi aliverkon (10.0.0.0/16) nimeltä Azure-VNet-Ad-sivustossa toimialueen AD-sivustossa. Tämä sivusto sisältää *basename*-ad1-AM ja *basename*-ad2-AM-palvelimiin. 

- Lisää IP-sivustojen välinen siirto asetukset, joka määrittää paikallisen sivuston ja toimialueen ohjaimet replikoinnin väliä pilvipalvelussa. Voit tarkastella asetuksia aliverkon, sivustojen ja paikallisen toimialueen ohjauskoneen *Active Directory-sivustojen ja palvelinten* konsoli transport asetukset:

![[2]][2]

## <a name="deployment"></a>Käyttöönotto

Esimerkki ratkaisu on seuraavassa prerequsites:

- Olet jo määrittänyt paikallisen toimialueen ja että olet määrittänyt DNS, ja asentanut reititys- ja -palvelut tukemaan VPN muodostaa Azure VPN-yhdyskäytävä.


- Olet asentanut uusimman version Azure-CLI. [Lisätietoja näiden ohjeiden][cli-install].

- Jos olet otat Windows-ratkaisun, sinun on asennettava työkalua, joka sisältää bash shell esimerkiksi [GitHub työpöytä][github-desktop].

>[AZURE.NOTE] Jos sinulla ei ole access paikallisen-toimialueeseen, voit luoda käyttämällä [onpremdeploy.sh] testiympäristössä[ onpremdeploy] bash komentosarjan. Tämä komentosarja luo verkko- ja AM hyvin basic paikallisen asennuksen tulokset vastaavat pilveen. Muokkaa tätä komentosarjaa ennen sen suorittamista ja määritä seuraavat muuttujat määritetty tiedoston alkuun:
>
> - **BASE_NAME**. Käyttäjän määrittämät etuliite resurssiryhmä ja AM luoma komentosarja. Kohteen pitäisi olla **enintään 5 merkkiä** muutoin komentosarja yrittää luoda AM, jonka nimi on virheellinen ja hylätty.
> 
> - **TILAUKSEN**. Azure tilauksen käyttämällä. Resurssiryhmä luodaan tämän suscription.
> 
> - **Sijainti**. Azure sijainti, johon luot resurssiryhmän, kuten *eastus* tai *westus*.
> 
> - **ADMIN_USER_NAME**. Käytettävä AM järjestelmänvalvojatilin nimi.
> 
> - **ADMIN_PASSWORD**. Järjestelmänvalvojatilin salasana.

Seuraavien toimien luonnissa näyte:

1. Lataa ja muokata [azuredeploy.sh] [ azuredeploy] komentosarja ja määritä seuraavat parametrit tiedoston alkuun:

    - **BASE_NAME**. Käyttäjän määrittämät etuliite resurssiryhmiä ja VMs luoma komentosarja. Kuten edellä kohteen pitäisi olla **enintään 5 merkkiä**.

    - **TILAUKSEN**. Azure tilauksen käyttämällä.

    - **OS_TYPE**. Käyttöjärjestelmä (*Windows* tai *Linux*) käyttämään web ja business-access tason VMs. Huomaa, että kaikki AD-palvelimet luoma komentosarja suorittaa Windows Server 2012: tämän asetuksen arvosta huolimatta.

    - **Toimialuenimi**. Paikallisen toimialueen nimi. Huomaa, että jos käytät onpremdeploy.sh komentosarja luoma ympäristössä, tämä on oltava *contoso.com*.

    - **Sijainti**. Azure sijainti, johon resurssiryhmien luominen.

    - **ADMIN_USER_NAME**. Käyttää eri VMs järjestelmänvalvojan tilin nimi.

    - **ADMIN_PASSWORD**. Järjestelmänvalvojatilin salasana.

    - **ON_PREMISES_PUBLIC_IP**. Paikallisen VPN-koneen julkiseen IP-osoite.

    - **ON_PREMISES_ADDRESS_SPACE**. Paikallisen verkon sisäinen osoitetilaa. Jos käytät onpremdeploy.sh komentosarja luoma ympäristössä, tämä on oltava 192.168.0.0/16.

    - **VPN_IPSEC_SHARED_KEY**. IP jaetun avaimen käytettäviä paikalliseen verkkoon ja Azure VPN-yhdyskäytävän VPN-yhteyden muodostamisesta.

    - **ON_PREMISES_DNS_SERVER_ADDRESS**. Paikallisen DNS-palvelimen IP-osoite. Jos käytössäsi on onpremdeploy.sh komentosarja luoma ympäristössä, tämä on oltava 192.168.0.4

    - **ON_PREMISES_DNS_SUBNET_PREFIX** Paikallisen aliverkon osoite etuliite. Jos käytät onpremdeploy.sh komentosarja luoma ympäristössä, tämä on oltava 192.168.0.0/24.

    >[AZURE.NOTE] Tallenna resurssit ja aika-komentosarja ei luoda business- tai access-tasoa. Jos asetat kohteita, voit kommentointi azuredeploy.sh komentosarjan seuraavassa osassa:
    >
    >
    > ```
    > #### # create biz tier
    > #### TEMPLATE_URI=${URI_BASE}/ARMBuildingBlocks/Templates/bb-ilb-backend-http-https.json
    > #### SUBNET_NAME_PREFIX=${DEPLOYED_BIZ_SUBNET_NAME_PREFIX}
    > #### ILB_IP_ADDRESS=${BIZ_ILB_IP_ADDRESS}
    > #### NUMBER_VMS=${BIZ_NUMBER_VMS}
    > #### 
    > #### RESOURCE_GROUP=${BASE_NAME}-${SUBNET_NAME_PREFIX}-tier-rg
    > #### VM_NAME_PREFIX=${SUBNET_NAME_PREFIX}
    > #### VM_COMPUTER_NAME_PREFIX=${SUBNET_NAME_PREFIX}
    > #### VNET_RESOURCE_GROUP=${NTWK_RESOURCE_GROUP}
    > #### VNET_NAME=${DEPLOYED_VNET_NAME}
    > #### SUBNET_NAME=${DEPLOYED_BIZ_SUBNET_NAME}
    > #### PARAMETERS="{\"baseName\":{\"value\":\"${BASE_NAME}\"},\"vnetResourceGroup\":{\"value\":\"${VNET_RESOURCE_GROUP}\"},\"vnetName\":{\"value\":\"${VNET_NAME}\"},\"subnetName\":{\"value\":\"${SUBNET_NAME}\"},\"adminUsername\":{\"value\":\"${ADMIN_USER_NAME}\"},\"adminPassword\":{\"value\":\"${ADMIN_PASSWORD}\"},\"subnetNamePrefix\":{\"value\":\"${SUBNET_NAME_PREFIX}\"},\"ilbIpAddress\":{\"value\":\"${ILB_IP_ADDRESS}\"},\"osType\":{\"value\":\"${OS_TYPE}\"},\"numberVMs\":{\"value\":${NUMBER_VMS}},\"vmNamePrefix\":{\"value\":\"${VM_NAME_PREFIX}\"},\"vmComputerNamePrefix\":{\"value\":\"${VM_COMPUTER_NAME_PREFIX}\"}}"
    > #### 
    > #### echo
    > #### echo
    > #### echo azure group create --name ${RESOURCE_GROUP} --location ${LOCATION} --subscription ${SUBSCRIPTION}
    > ####      azure group create --name ${RESOURCE_GROUP} --location ${LOCATION} --subscription ${SUBSCRIPTION}
    > #### echo
    > #### echo
    > #### echo azure group deployment create --template-uri ${TEMPLATE_URI} -g ${RESOURCE_GROUP} -p ${PARAMETERS} --subscription ${SUBSCRIPTION}
    > ####      azure group deployment create --template-uri ${TEMPLATE_URI} -g ${RESOURCE_GROUP} -p ${PARAMETERS} --subscription ${SUBSCRIPTION}
    > #### 
    > #### # create db tier
    > #### TEMPLATE_URI=${URI_BASE}/ARMBuildingBlocks/Templates/bb-ilb-backend-http-https.json
    > #### SUBNET_NAME_PREFIX=${DEPLOYED_DB_SUBNET_NAME_PREFIX}
    > #### ILB_IP_ADDRESS=${DB_ILB_IP_ADDRESS}
    > #### NUMBER_VMS=${DB_NUMBER_VMS}
    > #### 
    > #### RESOURCE_GROUP=${BASE_NAME}-${SUBNET_NAME_PREFIX}-tier-rg
    > #### VM_NAME_PREFIX=${SUBNET_NAME_PREFIX}
    > #### VM_COMPUTER_NAME_PREFIX=${SUBNET_NAME_PREFIX}
    > #### VNET_RESOURCE_GROUP=${NTWK_RESOURCE_GROUP}
    > #### VNET_NAME=${DEPLOYED_VNET_NAME}
    > #### SUBNET_NAME=${DEPLOYED_DB_SUBNET_NAME}
    > #### PARAMETERS="{\"baseName\":{\"value\":\"${BASE_NAME}\"},\"vnetResourceGroup\":{\"value\":\"${VNET_RESOURCE_GROUP}\"},\"vnetName\":{\"value\":\"${VNET_NAME}\"},\"subnetName\":{\"value\":\"${SUBNET_NAME}\"},\"adminUsername\":{\"value\":\"${ADMIN_USER_NAME}\"},\"adminPassword\":{\"value\":\"${ADMIN_PASSWORD}\"},\"subnetNamePrefix\":{\"value\":\"${SUBNET_NAME_PREFIX}\"},\"ilbIpAddress\":{\"value\":\"${ILB_IP_ADDRESS}\"},\"osType\":{\"value\":\"${OS_TYPE}\"},\"numberVMs\":{\"value\":${NUMBER_VMS}},\"vmNamePrefix\":{\"value\":\"${VM_NAME_PREFIX}\"},\"vmComputerNamePrefix\":{\"value\":\"${VM_COMPUTER_NAME_PREFIX}\"}}"
    > #### 
    > #### echo
    > #### echo
    > #### echo azure group create --name ${RESOURCE_GROUP} --location ${LOCATION} --subscription ${SUBSCRIPTION}
    > ####      azure group create --name ${RESOURCE_GROUP} --location ${LOCATION} --subscription ${SUBSCRIPTION}
    > #### echo
    > #### echo
    > #### echo azure group deployment create --template-uri ${TEMPLATE_URI} -g ${RESOURCE_GROUP} -p ${PARAMETERS} --subscription ${SUBSCRIPTION}
    > ####      azure group deployment create --template-uri ${TEMPLATE_URI} -g ${RESOURCE_GROUP} -p ${PARAMETERS} --subscription ${SUBSCRIPTION}
    > ```

2. Avaa bash shell-kehote ja Siirrä kansioon, joka sisältää azuredeploy.sh komentosarja.

3. Kirjaudu sisään Azure-tili. Bash-käyttöliittymän Kirjoita seuraava komento:

    ```
    azure login
    ```

    Yhteyden muodostaminen Azure ohjeiden mukaan.

4. Suorita-komento `./azuredeploy.sh`, ja odota sitten, kun komentosarja luo verkkoinfrastruktuuria.

5. *Tarkista, että VNet on luotu*kehotettaessa Azure portaalin avulla nimeltä *basename*- ntwk-rg resurssiryhmä on luotu ja että se sisältää kohteet kaltaisia seuraavan kuvan mukaisesti:

    ![[3]][3]

    >[AZURE.NOTE] Esimerkeissä näytetään *basename* määritettiin *pilveen* , kun komentosarja on suoritettu.

    Valitse *basename*- vnet VNet, valitse *aliverkosta*ja varmista, että alla aliverkosta on luotu:

    ![[4]][4]

6. Bash shell-ikkunassa kehotettaessa näppäintä painetaan ja odota, kunnes web taso ja kuormituksen luodaan.

7. *Varmista, että Web-taso on luotu oikein*kehotettaessa kutsutaan *basename*web-taso-rg resurssiryhmä on luotu ja että se sisältää kohteet kaltaisia alla Azure portaalin avulla:

    ![[5]][5]

8. Bash shell-ikkunassa kehotettaessa näppäintä painetaan ja odota, NVAs luodaan.

9. *Varmista, että NVA on luotu oikein*kehotettaessa kutsutaan *basename*- hallintoraportointi-rg resurssiryhmä luoda seuraavat sisällön Azure portaalin avulla:

    ![[6]][6]

10. Bash shell-ikkunassa kehotettaessa näppäintä painetaan ja odota, jumpbox luodaan.

11. *Varmista, että jumpbox on luotu oikein*kehotettaessa Tarkista, että seuraavien kohteiden on lisätty *basename*- hallintoraportointi rg resurssiryhmä Azure portaalin avulla:

    - Käytettävyys määrittäminen kutsutaan *basename*- jb-muodossa.

    - AM, nimeltä *basename*- jb-AM.

    - Verkkoliittymän kutsutaan *basename*- jb-NIC-sivustosta.

12. Bash shell-ikkunassa kehotettaessa näppäintä painetaan ja odota, kunnes Azure VPN-yhdyskäytävän ja yhteys luodaan. Huomaa, että tämä vaihe voi kestää enimmillään 30 minuuttia.

13. *Varmista, että VPN-yhdyskäytävän on luotu oikein*kehotettaessa Tarkista, että seuraavien kohteiden on lisätty *basename*- ntwk rg resurssiryhmä Azure portaalin avulla:

    - Paikalliseen verkkoon kirjauduttaessa yhdyskäytävän nimi--paikallisen-lgw.
    
    - VPN-yhdyskäytävän nimi *basename*- vpngw.

    - Yhdyskäytävän yhteys nimeltä *basename*- vnet-vpnconn. Huomaa, että tämä yhteyden tilan välttämättä *ole yhdistetty* Jos; yhteys paikalliseen loppuun ei ole vielä määrittänyt Tämä osoitteen myöhemmin.

14. Bash shell-ikkunassa kehotettaessa näppäintä painetaan ja odota, kunnes VMs ja muita resursseja voi käyttää Jos kyseessä on luotu.

15. *Varmista, että jos kyseessä on luotu oikein*kehotettaessa kutsutaan *basename*-dmz-rg resurssiryhmä luoda seuraavat sisällön Azure portaalin avulla:

    ![[7]][7]

16. Bash shell-ikkunassa tulee näkyviin paina näppäintä. Näkyy seuraavia ohjeita:

    ```text
    Manual Step...

    Please configure your on-premises network to connect to the Azure VNet

    Make sure that you can connect to the on-premises AD server from the Azure VMs
    ```

    Kirjaudu sisään paikalliseen tietokoneeseen, joka yhdistää Azure yhdyskäytävän ja määrittämää yhteyttä asianmukaisesti. Lisää staattinen tiet paikallisen yhdyskäytävän laitteella, joka ohjaa requestsfor 10.0.0.0/16 osoitealueita yhdyskäytävä VNet kautta. Näin vaiheet vaihtelevat mukaan, miten yhteys muodostetaan. Katso [käyttöönoton Hybrid-verkoston arkkitehtuuri Azure ja paikallisen VPN] [ implementing-a-hybrid-network-architecture-with-vpn] lisätietoja.

    Huomaa, että löydät Azure VPN-yhdyskäytävän julkiseen IP-osoitteen avulla Azure portaalin tutkia *basename*- vpngw yhdyskäytävän *basename*- ntwk rg resurssiryhmä:

    ![[8]][8]

    Voit määrittää, onko yhteys on muodostettu oikein katsomalla *basename*- vnet vpnconn yhteyden tilan. Se on määritettävä *yhteys on muodostettu*.

    Testaa yhteys, Avaa Etätyöpöytäyhteys jumpbox (10.0.0.254) tietokoneesta, joka sijaitsee kuin paikalliseen verkkoon kirjauduttaessa.

17. Bash shell-ikkunassa tulee näkyviin paina näppäintä. Seuraavassa Kysy, *mitä tahansa näppäintä päivittäminen osoittamaan paikallisen DNS VNet VNet-asetusta*, paina näppäintä ja odota, VNet DNS-asetusten päivitetään **ON_PREMISES_DNS_SERVER_ADDRESS** parametrina azuredeploy.sh komentosarjan määritetyn arvon.

18. Tulee näkyviin, *Varmista, että VNet DNS server-asetus on päivitetty*, käyttää Azure portal *basename*- vnet VNet *basename*- ntwk rg resurssiryhmän *DNS-palvelimet* -asetus. Se on asetettava *Mukautetun DNS*ja *ensisijainen DNS-palvelin* on oltava paikallisen DNS-palvelimen osoite:

    ![[9]][9]

19. *Minkä tahansa-näppäimellä voit luoda resurssiryhmän AD-palvelimia* kehottaessa bash shell-ikkunassa näppäintä painetaan ja odota, kunnes asettamisen AD-palvelimet pilveen resurssiryhmä on luotu.

20. *Minkä tahansa-näppäimellä voit luoda AD-palvelimia VMs* kehottaessa bash shell-ikkunassa näppäintä painetaan ja luoda ja lisätä resurssiryhmän VMs odota.

21. Kun *kaikki näppäintä liittymään paikallisen toimialueen VMs* tulee näkyviin, siirry Azure-portaaliin ja varmista, että ryhmän nimeltä *basename*- dns-rg on luotu ja että se sisältää kaksi VMS (*basename*-ad1-AM ja *basename*-ad2-AM):

    ![[10]][10]

22. *Basename*- ntwk rg resurssiryhmä NSG luotu valintaruudun nimi *basename*-ad-nsg. Tarkista saapuvan ja lähtevän suojaussäännöt tämän NSG varten. Ne pitäisi täsmätä [ratkaisun osien] taulukot ilmoitettuja[ solution-components] osa.

23. Bash shell-ikkunassa kehotettaessa näppäintä painetaan ja odota, VMs lisätään paikallisen toimialueen.

24. Milloin *paikallisen Siirry AD-palvelin, varmista, että toimialueet lisätty tietokoneet* Kysy, muodosta yhteys paikalliseen tietokoneeseen ja tarkista, että toimialue on lisätty sekä VMs *Active Directoryn käyttäjät ja tietokoneet* -konsolin avulla:

    ![[11]][11]

25. Bash shell-ikkunassa kehotettaessa näppäintä painetaan ja odota, kunnes AD replikoinnin sivusto luodaan toimialueen.

26. Milloin *paikallisen Siirry AD-palvelin, tarkista, että replikoinnin sivusto on luotu* Kysy, käyttämällä *Active Directory-sivustot ja -palvelut* -konsolin, tarkista, että replikoinnin sivuston nimeltä *Azure-Vnet-Ad-sivusto* on luotu onnistuu, IP sivustojen välinen siirto linkki on korostettuna *AzureToOnpremLink*ja aliverkon, joka viittaa VNet ja:

    ![[12]][12]

27. Bash shell-ikkunassa kehotettaessa näppäintä painetaan ja odota, kun komentosarja asentaa hakemistopalvelujen ja DNS-kunkin AD-VMs.

28. Kun näyttöön tulee kehote, *Kirjoita Kirjaudu sisään jokaiseen Azure AD-palvelimeen, voit varmistaa, että hakemistopalvelujen on määritetty onnistuneesti* , Etätyöpöytäyhteys avaaminen paikallisen tietokoneen jumpbox (*basename*- jb-AM) ja avaat toisen Etätyöpöytäyhteys jumpbox ensimmäisen AD-palvelin (*basename*-ad1-AM). Kirjaudu sisään **toimialueen_nimi**, **ADMIN_USER_NAME**ja **ADMIN_PASSWORD** , jonka olet määrittänyt azuredeploy.sh komentosarja. Käytä palvelimen hallinta ja varmista, että AD DS ja DNS-roolien molemmat lisäämisen. Tämä toimenpide on toistettava toisen AD-palvelin (*basename*-ad2-AM).

29. Bash shell-ikkunassa tulee näkyviin paina näppäintä. Kun näyttöön tulee kehote, *minkä tahansa näppäintä osoittavan DNS-Azure Azure VNet DNS-asetusten määrittämiseen* , paina näppäintä ja Salli komentosarja VNet DNS-asetusten päivittäminen.

30. Kun kehotteen *Tarkista, että VNet DNS: asetus on päivitetty viittaus Azure AM DNS servers* näkyy Azure portaalin tarkistus *basename*- vnet VNet *basename*- ntwk rg resurssiryhmän *DNS-palvelimet* -asetuksen avulla. Ensisijaisen ja toissijaisen DNS-palvelimet pitäisi nyt viitata kaksi AD VMs:

    ![[13]][13]

31. Käynnistä kukin AD-VMs ennen jatkamista. Tämä vaihe on tarpeen varmistaa, että jokainen Valitse oikea DNS-asetukset azuren. Odota, kunnes molemmat VMs on käytössä, ennen kuin jatkat.

32. Bash shell-ikkunassa tulee näkyviin paina näppäintä. Seuraava pyydettäessä *minkä tahansa-näppäimellä voit käyttää yhdyskäytävän UDR yhdyskäytävän aliverkon (se voi on poistettu)*, paina näppäintä ja avulla voit päivittää yhdyskäytävän UDR komentosarja.

33. Varmista, että komentosarja on suoritettu onnistuneesti.

## <a name="next-steps"></a>Seuraavat vaiheet

- Lue [Lisää-resurssin metsää luomisen] parhaat käytännöt[ adds-resource-forest] Azure-tietokannassa.

- Tutustu parhaisiin käytäntöihin [ADFS-infrastruktuuria] luomiseen[ adfs] Azure-tietokannassa.

<!-- links -->

[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[adfs]: ./guidance-identity-adfs.md
[guidance-vpn-gateway]: ./guidance-hybrid-network-vpn.md
[adds-resource-forest]: ./guidance-identity-adds-resource-forest.md
[script]: #sample-solution-script
[implementing-a-multi-tier-architecture-on-Azure]: ./guidance-compute-3-tier-vm.md
[implementing-a-secure-hybrid-network-architecture-with-internet-access]: ./guidance-iaas-ra-secure-vnet-dmz.md
[implementing-a-secure-hybrid-network-architecture]: ./guidance-iaas-ra-secure-vnet-hybrid.md
[implementing-a-hybrid-network-architecture-with-vpn]: ./guidance-hybrid-network-vpn.md
[active-directory-domain-services]: https://technet.microsoft.com/library/dd448614.aspx
[active-directory-federation-services]: https://technet.microsoft.com/windowsserver/dd448613.aspx
[azure-active-directory]: ../active-directory-domain-services/active-directory-ds-overview.md
[azure-ad-connect]: ../active-directory/active-directory-aadconnect.md
[architecture]: #architecture_diagram
[security-considerations]: #security-considerations
[recommendations]: #recommendations
[azure-vpn-gateway]: https://azure.microsoft.com/documentation/articles/vpn-gateway-about-vpngateways/
[azure-expressroute]: https://azure.microsoft.com/documentation/articles/expressroute-introduction/
[claims-aware applications]: https://msdn.microsoft.com/en-us/library/windows/desktop/bb736227(v=vs.85).aspx
[active-directory-federation-services-overview]: https://technet.microsoft.com/en-us/library/hh831502(v=ws.11).aspx
[capacity-planning-for-adds]: http://social.technet.microsoft.com/wiki/contents/articles/14355.capacity-planning-for-active-directory-domain-services.aspx
[ad-ds-ports]: https://technet.microsoft.com/library/dd772723(v=ws.11).aspx
[where-to-place-an-fs-proxy]: https://technet.microsoft.com/library/dd807048(v=ws.11).aspx
[powershell-ad]: https://technet.microsoft.com/en-us/library/ee617195.aspx
[ad_network_recommendations]: #network_configuration_recommendations_for_AD_DS_VMs
[domain_and_forests]: https://technet.microsoft.com/library/cc759073(v=ws.10).aspx
[best_practices_ad_password_policy]: https://technet.microsoft.com/magazine/ff741764.aspx
[monitoring_ad]: https://msdn.microsoft.com/library/bb727046.aspx
[microsoft_systems_center]: https://www.microsoft.com/server-cloud/products/system-center-2016/
[cli-install]: https://azure.microsoft.com/documentation/articles/xplat-cli-install
[github-desktop]: https://desktop.github.com/
[sssd-and-active-directory]: https://help.ubuntu.com/lts/serverguide/sssd-ad.html
[set-a-static-ip-address]: https://azure.microsoft.com/documentation/articles/virtual-networks-static-private-ip-arm-pportal/
[ad-azure-guidelines]: https://msdn.microsoft.com/library/azure/jj156090.aspx
[adds-data-disks]: https://msdn.microsoft.com/library/azure/jj156090.aspx#BKMK_PlaceDB
[AD-FSMO-recommendations]: #active_directory_FSMO_placement_recommendations
[AD-FSMO-roles-in-windows]: https://support.microsoft.com/en-gb/kb/197132
[standby-operations-masters]: https://technet.microsoft.com/library/cc794737(v=ws.10).aspx
[transfer-FSMO-roles]: https://technet.microsoft.com/library/cc816946(v=ws.10).aspx
[view-fsmo-roles]: https://technet.microsoft.com/library/cc816893(v=ws.10).aspx
[read-only-dc]: https://technet.microsoft.com/library/cc732801(v=ws.10).aspx
[AD-sites-and-subnets]: https://blogs.technet.microsoft.com/canitpro/2015/03/03/step-by-step-setting-up-active-directory-sites-subnets-site-links/
[sites-overview]: https://technet.microsoft.com/library/cc782048(v=ws.10).aspx
[implementing-adfs]: ./guidance-iaas-ra-secure-vnet-adfs.md
[onpremdeploy]: https://github.com/mspnp/blueprints/blob/master/ARMBuildingBlocks/guidance-iaas-ra-ad-extension/onpremdeploy.sh
[azuredeploy]: https://github.com/mspnp/blueprints/blob/master/ARMBuildingBlocks/guidance-iaas-ra-ad-extension/azuredeploy.sh
[solution-components]: #solution_components

[0]: ./media/guidance-iaas-ra-secure-vnet-ad/figure1.png "Suojattu hybrid verkoston arkkitehtuuri Active Directory-hakemistosta"
[1]: ./media/guidance-iaas-ra-secure-vnet-ad/figure2.png "Active Directoryn käyttäjät ja tietokoneiden konsolin kaksi Azure VMs, kuin palvelinten luettelo"
[2]: ./media/guidance-iaas-ra-secure-vnet-ad/figure3.png "Active Directory-sivustot ja -palvelut-konsolin sivuston replikoinnin asetuksia näkyvät pilveen"
[3]: ./media/guidance-iaas-ra-secure-vnet-ad/figure4.png "Basename ntwk-rg resurssiryhmä sisältö"
[4]: ./media/guidance-iaas-ra-secure-vnet-ad/figure5.png "Valitse basename vnet VNet aliverkosta"
[5]: ./media/guidance-iaas-ra-secure-vnet-ad/figure6.png "Web-tason kohteet"
[6]: ./media/guidance-iaas-ra-secure-vnet-ad/figure7.png "Basename hallintoraportointi-rg resurssiryhmän NVAs"
[7]: ./media/guidance-iaas-ra-secure-vnet-ad/figure8.png "Basename dmz-rg resurssiryhmä resurssit"
[8]: ./media/guidance-iaas-ra-secure-vnet-ad/figure9.png "Azure VPN-yhdyskäytävän julkiseen IP-osoitteen löytäminen"
[9]: ./media/guidance-iaas-ra-secure-vnet-ad/figure10.png "DNS-palvelinasetukset * basename *-vnet VNet"
[10]: ./media/guidance-iaas-ra-secure-vnet-ad/figure11.png "* Basename *-dns-rg resurssiryhmä sisältävä VMs AD-palvelin"
[11]: ./media/guidance-iaas-ra-secure-vnet-ad/figure12.png "Luettelo AD-palvelin VMs jäseniksi toimialueen Active Directoryn käyttäjät ja tietokoneiden konsolin"
[12]: ./media/guidance-iaas-ra-secure-vnet-ad/figure13.png "Active Directory-sivustot ja -palvelut-konsolin Azure AD-palvelimia replikoinnin-sivuston näkyvissä"
[13]: ./media/guidance-iaas-ra-secure-vnet-ad/figure14.png "Viittaaminen pilveen AD-palvelimien basename vnet VNet DNS-palvelinasetukset"