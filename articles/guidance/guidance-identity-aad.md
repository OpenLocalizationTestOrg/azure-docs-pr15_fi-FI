<properties
   pageTitle="Käyttöönoton Azure Active Directory | Microsoft Azure"
   description="Miten suojatun hybrid verkoston arkkitehtuuri-Azure Active Directoryn avulla."
   services="guidance,virtual-network,active-directory"
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
   ms.date="09/28/2016"
   ms.author="telmos"/>

# <a name="implementing-azure-active-directory"></a>Azure Active Directory toteuttaminen

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

Tässä artikkelissa kerrotaan parhaita käytäntöjä paikallisen Active Directory (AD) toimialueita ja puuryhmiä integraation Azure Active Directory pilvipohjainen tunnistetietojen käyttöoikeuden.

> [AZURE.NOTE] Azure on kaksi eri käyttöönoton mallit: [Resurssienhallinta] [ resource-manager-overview] ja perinteinen. Viite-arkkitehtuuri käyttää Resurssienhallinta, johon Microsoft suosittelee uuden käyttöönotoissa.

Voit käyttää hakemiston ja käyttäjätiedot palvelut, kuten Active Directory-palveluiden (AD DS) tarkistamiseen käyttäjätietojen tulostusvaihtoehtoja. Nämä käyttäjätietojen voivat kuulua käyttäjien, tietokoneiden, sovelluksiin tai muita resursseja, jotka ovat osa suojauksen rajan. Voit ylläpitää directory ja käyttäjätiedot services paikallisen, mutta jos sovelluksen osat sijaitsevat Azure hybrid tilanne voi olla tehokkaampaa Laajenna toiminto kyselyjä pilveen. Tämän menetelmän voit vähentää viive aiheuttaa lähettämällä todennus ja paikallisen pyytää pilvestä takaisin hakemistosta ja käyttäjätiedot palvelut-ympäristöön.

Azure on kaksi ratkaisujen käyttöönoton directory ja tunnistetiedot pilvessä:

1. Voit käyttää [Azure Active Directory (AAD)] [ azure-active-directory] luominen pilvipalvelussa AD-toimialue ja linkittää paikallisen AD-toimialue. AAD avulla voit määrittää kertakirjautuminen (SSO) pilven kautta sovellukset-käyttäjille. AAD käyttää [Azure AD Connect] [ azure-ad-connect] käynnissä paikalliseen replikointia objektit ja käyttäjätiedot säilytetään paikallisen säilössä pilveen.

    Voit myös ottaa AAD käyttämättä paikallisesta hakemistosta. Tässä tapauksessa AAD toimii ensisijainen lähde kaikki käyttäjätiedot sijaan joka sisältää tiedot, kopioida paikallisen hakemistosta.

    Huomaa, että pilveen kansio ei **ole** tiedostotunnistetta paikallisesta hakemistosta. Sen sijaan on kopio, joka sisältää olevat objektit ja käyttäjätiedot. Nämä kohteet paikallisen tehdyt muutokset kopioidaan pilveen, mutta tehdyt muutokset cloud **eivät** voi replikoida takaisin paikallisen toimialueen.

    >[AZURE.NOTE] Azure Active Directory-Premium-versiot tukevat käyttäjien salasanat, ottaminen käyttöön paikallisen käyttäjien Omatoiminen salasanan vaihtamisesta suorittaa pilveen takaisinkirjoituksen. Tämä on ainoa tieto AAD Synkronoi takaisin paikallisesta hakemistosta. Saat lisätietoja AAD ja niiden ominaisuuksista eri versioista [Azure Active Directory-versiot][aad-editions].

2. Voit ottaa AM, käynnissä toimialueen ohjauskoneen Azure-laajentaminen olemassa olevan AD-infrastruktuurin (sekä AD DS ja AD FS) AD DS: N paikallisen palvelinkeskukseen. Tämä vaihtoehto on yleisimpiä tilanteissa, joissa VPN-ja/tai ExpressRoute yhteyden yhdistää paikallisen verkko- ja Azure virtual verkon. Tämä ratkaisu tukee myös kaksisuuntaisen replikoinnin, joten voit tehdä muutoksia cloud ja paikallisesta-on aina, kun se on sopivin.

Tämä arkkitehtuuri keskitytään ratkaisu 1. Saat lisätietoja toisen ratkaisun [Laajentaminen Azure Active Directoryn][guidance-adds].

Tavallisesti tämä arkkitehtuuri laatikkomäärät ovat seuraavat:

- Tarjoaa käyttäjille käynnissä SaaS ja muiden sovellusten pilvipalvelussa, käyttämällä samaa tunnistetiedot, jotka he määrittää paikallisen sovellusten SSO.

- Julkaiseminen paikalliset web-sovellusten käyttö tarjota remote organisaatioon kuuluvien käyttäjien pilven kautta.

- Käyttöönoton loppukäyttäjille, kuten niiden salasanojen ja ryhmän hallinta delegoiminen Omatoiminen ominaisuuksia. 

    >[AZURE.NOTE] Nämä ominaisuudet voidaan hallita järjestelmänvalvojan oikeuksia ryhmäkäytännön kautta.

- Tilanteet, jossa paikallisen verkko- ja Azure virtual verkon isännöintipalvelu cloud-sovellukset eivät suoraan liity VPN-tunnelin tai ExpressRoute piiri avulla.

Huomaa AAD ei tarjoa AD toiminnallisuuteen. Esimerkiksi AAD tällä hetkellä vain käsittelee käyttäjän Todentamisella tietokoneen todennus sijaan. Jotkin sovellukset ja -palvelut, kuten SQL Server saattaa edellyttää tietokoneen todennus jolloin tätä ratkaisua ei ole sopiva. Lisäksi AAD ei ehkä sovellu järjestelmien, jossa osat voisit siirtää--paikallisen/cloud rajan yli sijaan vain kopioitava.

> [AZURE.NOTE] Azure Active Directory toiminnasta tarkan, katso [Microsoft Active Directory-kuvaus][aad-explained].

## <a name="architecture-diagram"></a>Arkkitehtuurikaavio

Seuraavassa kaaviossa korostaa tärkeitä osia tämän arkkitehtuuri. Lisätietoja Azure työmäärää elementtien lukea [Käynnissä VMs, N-taso-arkkitehtuuri Azure-][implementing-a-multi-tier-architecture-on-Azure]:

> [AZURE.NOTE] Yksinkertaisuuden, tässä kaaviossa vain näyttää suoranaisesti liity AAD yhteyksien ja kuvaa web selaimen pyynnön Uudelleenohjaa tai muu protokolla liittyvät liikenne, joita voi ilmetä todennuksen ja tunnistetiedot liittäminen yhteydessä. Esimerkiksi käyttäjä (paikallisen tai etäyhteyksien) käyttää verkkosovellukseen yleensä selaimen kautta ja web-sovelluksen läpinäkyvä saattaa ohjata selaimen kautta AAD pyynnön tarkistamiseen. Kun todennus-pyynnön voidaan siirtää takaisin web-sovelluksen kanssa tarvittavat tunnistetiedot.

[! [0]][0]

- **Azure Active Directory-vuokraajan**. Tämä on organisaatiosi luoma AAD esiintymä. Se toimii cloud-sovelluksissa yksinkertaisen hakemistopalvelun (se on kopioitu paikallisen objektien AD), ja tarjoaa tunnistetiedot.

- **Web-tason aliverkon**. Tämän aliverkon pitää VMs, jotka toteuttavat mukautettu pilvipohjainen sovellus kehittänyt oman organisaation ja jotka AAD voi toimia tunnistetietojen broker.

- **Paikallisen AD DS: ÄÄN palvelimen**. Organisaatiossa todennäköisesti on jo olemassa hakemistopalvelu, esimerkiksi AD DS: ÄÄN. Voit synkronoida kohteita (kuten käyttäjän ja ryhmätiedot) AD DS: ÄÄN hakemistossa AAD, jotta AAD tämän toiminnon avulla voit todentaa käyttäjätietojen kanssa.

- **Azure AD Connect synkronointi palvelimeen**. Tämä on paikallisen-tietokone, jossa Azure AD Connect synkronointi-palvelun. Tämä palvelu asentaa Azure AD Connect-ohjelmiston avulla. Azure AD Connect synkronointi-palvelu synkronoi tietoja (käyttäjät, ryhmät, yhteystiedot, jne.) paikallista AD AAD pilveen. Esimerkiksi voit valmistella tai ryhmiä ja käyttäjiä paikallinen deprovision ja AAD välittyvät muutokset. Azure AD Connect synkronointi-palvelun välittää tietoja AAD vuokraajan.

    >[AZURE.NOTE] Tietoturvasyistä käyttäjän salasanan ei tallenneta suoraan AAD. Sen sijaan AAD pitää hash kunkin salasana. Tämä on riittävä vahvistamiseksi käyttäjän salasanan vaihtaminen. Jos käyttäjä vaatii salasanan palauttaminen, täytyy olla suoritetaan paikallisen ja uusi hash AAD lähetetään. AAD Premium-versioissa ominaisuudet, joita voit automatisoida tehtävä, jotta käyttäjät voivat oman järjestelmänvalvojan salasanan.

## <a name="recommendations"></a>Suosituksia

Tässä osassa on yhteenveto suosituksia AAD seuraavasti.

- AD Connect
- Tietoturva

### <a name="azure-ad-connect-sync-service-recommendations"></a>Azure AD Connect synkronoi palvelun suositukset

Synkronoinnin koskee varmistetaan, että käyttäjät tunnistetiedot pilvessä on kuin paikallinen säilytetään. Azure AD Connect synkronointi-palvelun tarkoituksena on tämän yhtenäisyyden säilyttämiseksi. Seuraavat seikat yhteenvedon suosituksia Azure AD Connect synkronointi-palvelu:

- Ennen käyttöönoton Azure AD Connect synkronointi tulee määrittää organisaation vaatimusten synkronoinnin (mitä haluat synkronoida, mitkä toimialueista ja kuinka usein. Artikkelin [määrittäminen directory-synkronoinnin vaatimukset] [ aad-sync-requirements] kuvataan pisteet, ota huomioon.

- Voit suorittaa Azure AD Connect synkronointi-palvelun avulla AM tai tietokoneen isännöidään paikallisen. Tietoja Active Directoryn haihtuvuus mukaan kun AAD alkuperäisen synkronointi on suoritettu Azure AD Connect synkronointi-palvelun kuormitus ei todennäköisesti ole suuri. AM avulla voit helpommin skaalata server (näytön tehtävän [Seuranta huomioon otettavia seikkoja](#monitoring-considerations) -kohdassa kuvatulla tavalla voit selvittää, onko se on tarpeen).

- Jos sinulla metsää paikallisen useita toimialueita, voit tallentaa ja synkronoida koko metsää yksittäisen AAD vuokraajalle tiedot (tämä on suositellaan). Voit suodattaa tietoja käyttäjätietoja, jotka esiintyvät useampi kuin yksi toimialue, niin, että eri käyttäjätiedot näkyy vain kerran AAD sen sijaan, että käytössä kaksoiskappale, sillä se voi johtaa epäyhtenäisyydet kun tiedot on synkronoitu. Tämän menetelmän edellyttää käyttöönoton useisiin Azure AD Connect synkronointi-palvelimiin. Lisätietoja on artikkelissa useita AAD skenaarion [topologian huomioon otettavia seikkoja](#topology-considerations) -osassa. 

- Voit rajoittaa AAD vain tähän tallennettuja tietoja, joita tarvitaan käyttää suodatusta. Jos esimerkiksi organisaatiosi ei kannata AAD passiiviset tai muiden kuin henkilökohtaisten tietojen tallentamiseen. Suodatus voi olla ryhmän perustuva, toimialueelta, OU-pohjainen tai määritteiden perusteella ja voit yhdistää suodattimia voit luoda monimutkaisia sääntöjä. Voit valita esimerkiksi Synkronoi vain objekteja pidetään toimialueeseen, joka on valittu määrite tietyn arvon. Lisätietoja on artikkelissa [Azure AD Connect synkronointi: Määritä suodatus][aad-filtering].

- Toteuttamaan suuren käytettävyyden AD Connect synkronointi-palvelun toissijaisen väliaikaisen palvelimen suorittaminen Lisätietoja on kohdassa [topologian huomioon otettavia seikkoja](#topology-considerations) .

### <a name="security-recommendations"></a>Suojausta koskevia suosituksia

Seuraavien kohteiden yhteenveto ensisijainen suojauskäytäntö suosituksia AAD, yrityksen tarpeiden mukaan:

- Käyttäjien salasanojen hallinta

    Jos käytössäsi on AAD premium-versio, voit ottaa käyttöön salasanan takaisinkirjoituksen-AAD paikallisesta hakemistosta, Azure AD Connect mukautetun asennuksen suorittamiseen:

    [! [9]][9]

    Tämän ominaisuuden avulla käyttäjä voi palauttaa omia salasanoja Azure portaalin, mutta se on käytössä vain organisaation salasanan suojauskäytäntö tarkistamisen jälkeen. Esimerkiksi voivat rajoittaa, mitä käyttäjät voivat vaihtaa salasanoja ja salasanan hallintatoimintojen voi mukauttaa. Lisätietoja [Salasanojen hallinta mukauttaminen organisaation tarpeiden][aad-password-management].

- Ylläpito Suojaus paikallisen sovelluksissa, jotka voivat käyttää ulkoisesti.

    Azure AD-sovelluksen välityspalvelimen avulla voit antaa hallittu paikallisen verkkosovellusten ulkoisten käyttäjien AAD kautta. Tämän menetelmän suojaa sovellustesi ei paljastaa yhteys Internetiin. Vain käyttäjät, joilla on kelvollinen tunnistetiedot Azure hakemistossa pystyvät sovellustesi saavuttamiseksi. Lisätietoja on artikkelissa [Azure-portaalissa ottaa käyttöön sovelluksen välityspalvelimen][aad-application-proxy].

- Seurannan aktiivisesti AAD epäilyttävistä tehtävän merkkejä.

    Harkitse AAD Premium P2 versio, joka sisältää AAD tunnistetietojen suojaus. Tunnistetiedot käyttää mukautuvat koneen learning algoritmit ja heuristiikkaa tunnistaa poikkeamia ja riskien tapahtumia, jotka ilmaisevat, että käyttäjätietojen käsiin. Esimerkiksi se havaitsee mahdollisesti erheellisiin tehtävän, kuten epäsäännöllinen kirjautumisen toiminnot-merkki-laajennukset tuntemattomista lähteistä tai IP-osoitteiden epäilyttävistä tehtävän tai Kirjaudu apuohjelmat, jotka voivat olla tartunnan laitteilla. Käytä näitä tietoja, tunnistetiedot Luo raportteja ja ilmoitukset, jonka avulla voit tutkia riskin näitä tapahtumia ja ota haluamasi korjaus tai rajoituksen toiminto. Lisätietoja on artikkelissa [Azure Active Directory tunnistetietojen suojauksen][aad-identity-protection].

    Voit myös seurata epäilyttävistä ja tietoturvaan liittyvät toiminnot järjestelmäsi, jotka ilmenevät Azure-portaalissa AAD raportointi-toiminnon. AAD voivat luoda Yhteenvetoraporttien sarja:

    [! [17]][17]

    Lisätietoja raporttien käyttämisestä on artikkelissa [Azure Active Directory raportoinnin Guide][aad-reporting-guide].

## <a name="topology-considerations"></a>Topologian huomioon otettavia seikkoja

Jos paikallisesta hakemistosta on integroitu AAD, Määritä Azure AD Connect toteuttamisesta topologian, joka vastaa parhaiten organisaatiosi vaatimuksia. Topologioissa, joka tukee Azure AD Connect ovat seuraavat:

- **Yksittäisen metsää, yksi AAD-kansio**. Tämän topologian Azure AD Connect avulla voit synkronoida objektit ja käyttäjätiedot vähintään yhden toimialueiden paikallisen metsää yhtä AAD vuokraajan kanssa. Tämä on oletusarvo topologian täytäntöön Azure AD Connect pika-asennus.

    [! [1]][1]

    Älä luo useita Azure AD Connect synkronointi palvelimia eri toimialueiden saman paikallisen muodostaa AAD samassa alihallinnassa paitsi, jos käytössäsi on Azure AD Connect-synkronointi palvelimen väliaikaisen tila (Katso alla väliaikaisen Server).

- **Useita metsien, yksi AAD-kansio**. Käytä tätä topologian, jos sinulla on useampi kuin yksi paikallisen metsää. Voit yhdistää tunnistetietoja niin, että kunkin yksilöllisiä käyttäjän esitetään kerran AAD kansioon, vaikka sama käyttäjä on useampi kuin yksi metsää. Kaikki metsien käyttää samaan Azure AD Connect synkronointi-palvelimeen. Azure AD Connect synkronointi palvelin ei ole osa minkä tahansa toimialueen, mutta sen on oltava tavoitettavissa kaikki saatavista:

    [! [2]][2]

    Tämä verkkotopologia, älä käytä erillisessä Azure AD Connect synkronointi paikallisen kunkin metsää yhdistäminen yhteen AAD palvelutili-palvelinten. Tämä saattaa johtaa AAD kaksoiskappaleet tunnistetietoja, jos käyttäjiä on enemmän kuin yksi metsää.

- **Useat metsien, erillisessä topologioissa**. Tämän menetelmän avulla voit tunnistetietoja erillisessä saatavista yhdistäminen yhteen AAD palvelutili. Tämä menetelmä on hyödyllinen, jos liittää eri organisaatioissa puuryhmien (fuusion tai hankinta, esimerkiksi) jälkeen ja kunkin käyttäjän tunnistetiedot säilytetään vain yksi metsää:

    Jos kunkin metsää-GALs synkronoidaan, yksi metsää käyttäjän saattaa olla käytössä toisessa yhteyshenkilöksi. Näin voi käydä, jos esimerkiksi organisaatiossasi on käytössä GALSync Forefront käyttäjätietojen hallinta 2010 tai Microsoft käyttäjätietojen hallinta 2016. Tässä skenaariossa voit määrittää, että käyttäjät yksilöityä niiden *Posti* -määrite. Voit myös verrata käyttäjätietojen käyttämällä *ObjectSID* ja *msExchMasterAccountSID* määritteet. Tämä on kätevä, jos sinulla on vähintään yksi resurssi metsien ei käytössä-tilien kanssa.

- **Väliaikaisen palvelimeen**. Tässä määrityksessä suorittaa toisen Azure AD Connect synkronointi-server-esiintymän ensimmäisen rinnakkain. Tämä rakenne tukee skenaariot

    - Suuren käytettävyyden.

    - Testaaminen ja käyttöönottaminen Azure AD Connect synkronointi palvelimen uudet asetukset.

    - Uusi server esittely ja vanha vanha määritys voidaan poistaa käytöstä. 

    Näissä tilanteissa toinen suorittaa väliaikaisen *tilassa*. Palvelimen tietueita tietokannasta tuotujen objektien ja synkronointitietojen, mutta ei välitä AAD tiedot. Vain silloin, kun väliaikaisen tilan poistaminen käytöstä palvelimen ala kirjoittaa tietoja AAD ja aloittaa menestyneet salasanan kirjoitus-toisen puolen paikallisen kansioihin tarvittaessa.

    [! [4]][4]

    Lisätietoja on artikkelissa [Azure AD Connect synkronointi: toiminnallisia tehtäviä ja huomioon otettavia seikkoja][aad-connect-sync-operational-tasks].

- **Useita AAD kansioita**. On suositeltavaa, että voit luoda yksittäisen AAD kansion organisaation, mutta voi liittyä tilanteita, joissa tarvitset osion tietoja eri AAD kansioiden välillä. Tässä tapauksessa sinun tulee varmistaa, että kunkin objektin paikallisen metsää näkyy vain yksi AAD directory-synkronointi ja salasanan takaisinkirjoituksen ongelmien välttämiseksi.

    Tässä skenaariossa toteuttamaan määrittäminen erillisessä Azure AD Connect Synkronoi palvelinten AAD kunkin hakemiston ja käyttää suodatusta, jotta jokaisen Azure AD Connect synkronointi palvelimen toimii toisensa poissulkevia objektijoukon: 

    [! [5]][5]

Saat lisätietoja näiden topologioissa [Azure AD Connect topologioissa][aad-topologies].

## <a name="user-sign-in-considerations"></a>Käyttäjän huomioitavista-kirjautuminen

Oletusarvon mukaan AAD palvelun oletetaan, että käyttäjät kirjautuvat antamalla salasana, että ne käyttävät paikallisen ja Azure AD Connect synkronointi server määrittää paikallisen toimialueen ja AAD salasanan synkronointi. Monissa organisaatioissa Käytä tätä vaihtoehtoa, mutta kannattaa organisaation käytännöistä ja infrastruktuurin. Esimerkki:

- Organisaation suojauskäytäntö saattaa kieltää synkronointi pilveen hajautusarvot salasana.

- Saattaa edellyttää, että käyttäjät kohdata saumaton SSO (ilman muita salasana pyytää) kun cloud resurssien avaaminen toimialueen liitettyinä koneet yrityksen verkkoon.

- Organisaatiosi on ehkä jo ADFS tai kolmannen osapuolen federation palvelun käyttöön. Voit määrittää AAD käyttämään tämän infrastruktuurin toteuttaminen todennus ja SSO sijaan pilveen salasanatietoja.

Lisätietoja on artikkelissa [Azure AD yhteyden käyttäjän Kirjaudu käyttöönotto][aad-user-sign-in].

## <a name="azure-ad-application-proxy-considerations"></a>Azure AD sovelluksen välityspalvelimen huomioon otettavia seikkoja

Azure AD avulla voit antaa paikallisen-sovellukset.

- Näytä yhdysviivat hallita Azure AD-sovelluksen välityspalvelimen osan sovelluksen välityspalvelimen paikallisen web-sovellusten. Sovelluksen välityspalvelimen yhdistimen avautuu lähtevä verkkoyhteyden Azure AD-sovelluksen välityspalvelinta. Etäkäyttäjien pyynnöt reititetään takaisin-yhteyden kautta AAD web Apps-sovelluksissa. Tämä järjestelmä poistaa saapuvien porttien avaaminen Paikallinen palomuuri, vähentää organisaation näyttämiä uhanalaisten ei tarvitse.

Lisätietoja on kohdassa [Julkaise sovellusten Azure AD-sovelluksen välityspalvelimen][aad-application-proxy].

## <a name="object-synchronization-considerations"></a>Objektin synkronoinnin huomioon otettavia seikkoja

Azure AD Connect oletusarvon määrittäminen synkronointi paikallisen AD-kansion säännöt määritellyt on artikkelissa määrittäminen perustuu objektit [Azure AD Connect synkronointi: tietoja oletusarvon määrittäminen][aad-connect-sync-default-rules]. Vain objekteja, jotka täyttävät seuraavat säännöt synkronoidaan, muiden ohitetaan. Esimerkiksi käyttäjän objekteissa on oltava yksilöllinen *sourceAnchor* -määrite ja *accounEnabled* -määrite on täytettävä. Käyttäjän objektit, jotka eivät ole *sAMAccountName* -määrite tai, jotka alkavat tekstin *AAD_* tai *MSOL_* ei ole synkronoitu. Azure AD Connect käyttää useita sääntöjä käyttäjän, yhteyshenkilön, ryhmän, ForeignSecurityPrincipal ja tietokoneen objekteissa. Jos haluat muokata sääntöjä oletusarvoiset editorilla synkronoinnin säännöt asennetulla Azure AD Connect (myös kuvattu [Azure AD Connect synkronointi: tietoja oletusarvon määrittäminen][aad-connect-sync-default-rules]).

Voit määrittää oman suodattimet rajoittamaan synkronoitavaksi toimialueen tai OU objektit. Tai toteuta monimutkaisia mukautettuja suodattaminen, kuvatulla tavalla [Azure AD Connect synkronointi: Määritä suodatus][aad-filtering].

## <a name="security-considerations"></a>Suojausasiat

Ehdollinen käytönvalvonta avulla voit estää käyttöoikeuksien odottamattomia lähteistä:

- Käynnistää [Azure multi-factor Authentication (MFA)] [ azure-multifactor-authentication] Jos käyttäjä yrittää muodosta-luotettu sijainti (kuten Internetissä) sen sijaan, että luotettu verkkoon.

- Laitteen ympäristötyyppi käyttäjän (iOS, Android-Windows Mobile Windows) avulla voit määrittää access-käytännön sovellukset ja ominaisuudet.

- Selvitä käyttäjien laitteiden käytössä tai ei käytössä-tilaan, ja liittää tiedot access-käytäntö tarkistukset. Esimerkiksi jos käyttäjän puhelinnumero katoaa tai varastamiselta se olisi tallennetaan käytöstä ei käytössä, saat käyttöösi.

- Hallita Ryhmäjäsenyyden perusteella käyttäjälle haluamasi käyttöoikeustaso. Käytä [AAD dynaaminen jäsenyyden sääntöjä] [ aad-dynamic-membership-rules] yksinkertaistaa ryhmän hallinta. Lyhyt yleiskatsaus siitä, miten tämä toimii, katso lisätietoja artikkelista [Johdanto dynaaminen ryhmien jäsenyyksien][aad-dynamic-memberships].

- AAD tunnistetietojen käyttöoikeudella ehdollisen käyttöoikeuden riskin käytäntöjen avulla voit suojata epätavallisia kirjautumisen toimintoja tai muita tapahtumia.

Lisätietoja on artikkelissa [Azure Active Directory ehdollisen käyttöoikeuden][aad-conditional-access].

## <a name="scalability-considerations"></a>Skaalattavuus huomioon otettavia seikkoja

Skaalattavuus täyttämään AAD-palvelu ja Azure AD Connect synkronointi palvelimen:

- AAD-palvelua ei tarvitse määrittää tarvittavat asetukset toteuttamisesta skaalattavuus. AAD-palvelu tukee skaalattavuus replikoiden perusteella. AAD toteuttaa yksi ensisijainen replikan, mitä kahvoja kirjoitus-toimintoja, ja useita vain luku-toissijainen replikoiden. AAD ohjaa läpinäkyvä toissijainen replikoiden vastaan tehdyt ensisijainen se yritettiin kirjoituksia. AAD on potentiaalisen yhdenmukaisuuden. Kaikki ensisijainen se tehdyt muutokset välittyvät toissijainen replikoihin. Useimmat toiminnot vastaan AAD ovat lukujen kirjoituksia sijaan, tämä arkkitehtuuri Skaalaa paikan päällä.

    Lisätietoja on artikkelissa [Azure AD: geo tarpeettomat, helposti saatavilla, eri aikajaksoille cloud hakemistoa sovellusmalleista][aad-scalability].

- Azure AD Connect synkronointi-palvelin-Määritä, kuinka monta objektia, olet todennäköisesti synkronointi paikallisen hakemistosta. Jos sinulla on pienempi kuin 100 000 objekteja, voit käyttää oletusarvon SQL Server Express LocalDB ohjelmiston mukana Azure AD Connect. Jos sinulla on useita objekteja, voit asentaa tuotannon SQL Server-version ja Azure AD Connect määrittämällä, se on käytettävä aiemmin luodun SQL Server-esiintymän mukautetun asennuksen suorittamiseen.

## <a name="availability-considerations"></a>Käytettävyys huomioon otettavia seikkoja

Skaalattavuus huolenaiheita, jossa käytettävyys ulottuu AAD-palvelu ja Azure AD Connect määritykset:

- AAD palvelun tarkoituksena on suuri käytettävyys. Käyttäjän määrittämät käytettävyys ei vaihtoehtoja on. Se on geo jaettu ja suorittaa useita tietojen keskittää levitä eri puolilla maailmaa automaattinen automaattisesti kanssa. Jos data Centerissä ei ole käytettävissä, AAD varmistaa kansion tiedot ovat käytettävissä esiintymän käytön vähintään kaksi Lisää ohjelmistoteollisuuden vahingollisesta tietojen keskikohdan mukaan.

    >[AZURE.NOTE] AAD perus- ja Premium services oikeudet vähintään 99,9 % käytettävyyden SLA. Ei ole SLA AAD vapaa tason. Lisätietoja on artikkelissa [Azure Active Directory SLA][sla-aad].

- Voit laajentaa Azure AD Connect synkronointi palvelimen käytettävyys voit suorittaa toisen väliaikaisen tilassa [topologian huomioon otettavia seikkoja](#topology-considerations) -kohdassa kuvatulla tavalla. 

    Lisäksi, jos et käytä SQL Server Express LocalDB esiintymän Azure AD Connect mukana tulevaa, valitse Ota huomioon suuren käytettävyyden SQL Server. Huomaa, että tuettu vain suuren käytettävyyden ratkaisu on SQL-klusterointi. Azure AD Connect ei tue ratkaisuja, kuten peilaukseen ja aina.

    Muita huomioon otettavia seikkoja ylläpitoon Azure AD Connect synkronointi palvelimeen ja kuinka voit palauttaa virheen jälkeen saatavuus, katso [Azure AD Connect synkronointi: toiminnallisia tehtäviä ja huomioon otettavia seikkoja - palauttaminen][aad-sync-disaster-recovery].

## <a name="management-considerations"></a>Hallinnan huomioon otettavia seikkoja

Hallinnan AAD kaksi eri:

1. Käyttäjien hallinta AAD pilveen.

2. Ylläpito Azure AD Connect synkronointi-palvelimiin.

AAD on hallinta-toimialueet ja -kansioiden pilveen seuraavista vaihtoehdoista:

- [PowerShellin Azure Active Directory-moduulin][aad-powershell]. Käytä moduuli, jos haluat komentosarjan yleisiä Azure AD hallintatehtäviä kuten käyttäjien hallinta-toimialueen hallintaan ja kertakirjautumisen määrittämistä varten.

- Azure AD hallinnan sivu Azure-portaalissa. Tämä sivu sisältää vuorovaikutteinen hallinta-näkymän hakemiston ja avulla voit hallita ja määrittää useimmat AAD.

    [! [10]][10]

Azure AD Connect asentaa seuraavia työkaluja, joilla voit säilyttää Azure AD Connect synkronointipalvelut-paikallisen koneet:

- Microsoft Azure Active Directory-yhteyden konsoli. Tämän työkalun avulla voit muokata palvelimen Azure AD-synkronointi, mukauttaminen, miten synkronointi tapahtuu, ottaminen käyttöön tai väliaikaisen tilan poistaminen käytöstä ja vaihtaa käyttäjän kirjautuminen tila (Voit ottaa AD FS-kirjautuminen käyttämällä paikallisen infrastruktuuri).

- Synkronointi-palvelun hallinta. Tämä työkalu *Toiminnot* -välilehden avulla voit hallita synkronointi ja tunnistaa, onko minkään prosessin epäonnistui. Voit käynnistää synkronoinnin manuaalisesti tämän työkalun avulla. 

    [! [12]][12]

    *Yhdysviivat* -välilehden avulla voit hallita toimialueita yhteyksiä (paikallisen ja pilvipalvelussa), johon on liitetty synkronointiohjelma:

    [! [13]][13]

-  Synkronoinnin säännöt-editori. Tämän työkalun avulla voit mukauttaa, jossa objektit muutetaan kopioitaessa paikallista hakemistoa ja AAD pilveen välillä. Tämän työkalun avulla voit määrittää muita määritteet ja objektien synkronoinnin ja suodattimien avulla, mitkä esiintymät on tai ei voi synkronoida.

    Lisätietoja on kohdassa synkronoinnin säännön editorin asiakirjan [Azure AD Connect synkronointi: tietoja oletusarvon määrittäminen][aad-connect-sync-default-rules].

Sivun [Azure AD Connect synkronointi: Parhaat käytännöt muuttaminen oletusarvon määrittäminen] [ aad-sync-best-practices] on muita tietoja ja vihjeitä Azure AD Connect hallinta.

## <a name="monitoring-considerations"></a>Huomioon otettavia seikkoja seuranta

Kunnon seuranta suoritetaan käyttämällä tekijöiden asennettu paikalliseen sarjaa:

- Azure AD Connect asentaa agentti, joka kuvaa synkronoinnin tietoja. Azure-portaalissa Azure Active Directory yhteyden kunto-sivu avulla voit seurata kunto ja Azure AD Connect suorituskykyä. Lisätietoja on artikkelissa [Käyttämällä Azure AD yhteyden kunto-synkronointia varten][aad-health].

- Tarkkailla AD DS: N toimialueet ja kansioiden azuren, asenna tietokoneeseen paikallisen toimialueen Azure AD yhteyden kuntotietojen AD DS-agentti varten. Käytä Azure AD DS: ÄÄN näytön portaalin Azure Active Directory yhteyden kunto-sivu. Lisätietoja on artikkelissa [Käyttämällä Azure AD yhteyden kunto AD DS: N kanssa][aad-health-adds]

- Asenna Azure AD yhteyden kuntotietojen AD FS-agentti AD FS paikallisen käytössä kunto-ja Azure Active Directory yhteyden kunto-sivu käyttö Azure-portaalissa seurannassa AD DS: ÄÄN varten. Lisätietoja on artikkelissa [Käyttämällä Azure AD yhteyden kunto AD FS][aad-health-adfs]

Lisätietoja AD yhteyden kunto-agenttien ja yhtiön asentamisesta on kohdassa [Azure AD yhteyden kunto agentti asennus][aad-agent-installation].

## <a name="sample-solution"></a>Esimerkki ratkaisu

Tässä osassa asiakirjat etsimisen malli-ratkaisun, jotka voit tehdä AAD määritysten testaaminen vaiheet. Ratkaisu sisältää seuraavat osat:

- N-taso-web-sovelluksen, seuraavien ohjeiden mukaan [Käynnissä VMs, N-taso-arkkitehtuuri Azure-]rakenne[implementing-a-multi-tier-architecture-on-Azure].

- Testaa asiakastietokoneelle. On suositeltavaa käyttää toisen AM tämän tietokoneen. Tämä AM määritys ei ole merkitystä, kunhan sinulla on järjestelmänvalvojan oikeudet ja se voi muodostaa yhteyden n tason web-sovelluksen.

- Simuloitu paikalliseen ympäristöön, joka sisältää oman toimialueen AD DS: N avulla.

Tilanteita, joissa nämä vaiheet ovat seuraavat:

- Ottaminen käyttöön käynnissä pilveen ulkoisten käyttäjien kanssa AAD antamisen salasanan todennusta n-taso-web-sovelluksen käyttöä.

- Ottaminen käyttöön käynnissä pilveen käyttäjille AAD antamisen salasanan todennusta organisaatiossa, jossa n-taso-web-sovelluksen käyttöä.

### <a name="prerequisites"></a>Edellytykset

Vaiheet, jotka oletetaan, että seuraavat edellytykset:

- Sinulla on aiemmin Azure tilaus, jossa voit luoda resurssiryhmiä.

- Olet asentanut [Azure käyttöliittymä][azure-cli].

- Olet ladannut ja asentanut uusimman muodosta. Katso [tähän] [ azure-powershell-download] ohjeita.

- Olet asentanut kopion [makecert] [ makecert] testi asiakastietokoneen-apuohjelma.

- Voit käyttää kehittäminen tietokonetta, johon on asennettu Visual Studio.

### <a name="create-the-n-tier-web-application-architecture"></a>N-taso-web-sovelluksen arkkitehtuuri luominen

1. Luo kansio nimeltä `Scripts` , joka sisältää alikansion `Parameters`.

2. Lataa [Käyttöönotto ReferenceArchitecture.ps1] [ solution-script] PowerShell-komentosarjaa komentosarjat-kansioon.

3. Luoda toisen alikansion Windows parametrit-kansioon.

4. Lataa parametrit/Windows-kansioon seuraavat tiedostot:

    - [virtualNetwork.parameters.json][vnet-parameters-windows]

    - [networkSecurityGroup.parameters.json][nsg-parameters-windows]

    - [webTierParameters.json][webtier-parameters-windows]

    - [businessTierParameters.json][businesstier-parameters-windows]

    - [dataTierParameters.json][datatier-parameters-windows]

    - [managementTierParameters.json][managementtier-parameters-windows]

5. Muokkaa komentosarjat-kansiossa **Käyttöönotto ReferenceArchitecture.ps**1-tiedostoa ja muuta seuraava rivi Määritä resurssiryhmä, jotka on luotu tai käyttää AM ja resurssien luoma komentosarja:

        ```powershell
        # PowerShell
        $resourceGroupName = "ra-aad-ntier-rg"
        ```

6. Muokkaa seuraavat **Parametrit/ikkunan**s-kansiossa olevat tiedostot ja Aseta `resourceGroup` arvo `virtualNetworkSettings` kohdassa sama kuin nämä tiedostot, jonka olet määrittänyt **Käyttöönotto ReferenceArchitecture.ps1** -komentosarjatiedosto.

    - networkSecurityGroup.parameters.json

    - webTierParameters.json

    - businessTierParameters.json

    - dataTierParameters.json

    - managementTierParameters.json

7. Avaa PowerShellin Azure-ikkuna, komentosarjat-kansioon ja suorita seuraava komento:

        ```powershell
        .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> Windows ntier
        ```

    Korvaa **<subscription id>** käyttämällä Azure tilauksen.

    Saat **<location>**, Määritä Azure alue, kuten *eastus* tai *westus*.

8. Kun komentosarja on valmis, käytä Azure portaalin web tason kuormituksen (*ra aad-ntier-web-kg*) julkiseen IP-osoitteen hankkiminen:

    [! [18]][18]

9. Kirjaudu sisään testi asiakkaan tietokoneeseen (tai AM) ja varmista, että voit käyttää web tason selaamalla web tason kuormituksen julkiseen IP-osoite Internet Explorerin avulla. IIS oletussivun pitäisi näkyä.

### <a name="simulate-configuration-of-a-public-web-site"></a>Simuloida julkisen sivuston määrittäminen

>[AZURE.NOTE] Seuraavat vaiheet simuloida web tason liittäminen DNS-nimen www.contoso.com muokkaamalla testi asiakastietokoneen isännät-tiedostoa. Lisäksi web tason VMs on määritetty itse allekirjoitettua varmenteet ja isännöinnin myöntäjä väärennetty. Tämä koskee ainoastaan ja **näistä menetelmistä tuotantoympäristössä Älä käytä**.

1. Asiakastietokoneen testi Muokkaa tiedostoa **C:\Windows\System32\drivers\etc\hosts** Muistiossa ja lisätä merkinnän, joka yhdistää web tason kuormituksen julkiseen IP-osoitteen www.contoso.com nimi:

        ```text
        # Copyright (c) 1993-2009 Microsoft Corp.
        #
        # This is a sample HOSTS file used by Microsoft TCP/IP for Windows.
        #
        # This file contains the mappings of IP addresses to host names. Each
        # entry should be kept on an individual line. The IP address should
        # be placed in the first column followed by the corresponding host name.
        # The IP address and the host name should be separated by at least one
        # space.
        #
        # Additionally, comments (such as these) may be inserted on individual
        # lines or following the machine name denoted by a '#' symbol.
        #
        # For example:
        #
        #      102.54.94.97     rhino.acme.com          # source server
        #       38.25.63.10     x.acme.com              # x client host
        
        # localhost name resolution is handled within DNS itself.
        #   127.0.0.1       localhost
        #   ::1             localhost
        
        52.165.38.64    www.contoso.com
        ```

2. Varmista, että voit nyt selata www.contoso.com testi asiakastietokoneesta. IIS saman sivun pitäisi näkyä kuin aiemmin.

3. Testi-asiakastietokone Avaa komentokehote järjestelmänvalvojana ja makecert-apuohjelmalla c
4. Luo Väärennetyt pääkansion varmenteiden myöntäjä varmenteen:

        ```
        makecert -sky exchange -pe -a sha256 -n "CN=MyFakeRootCertificateAuthority" -r -sv MyFakeRootCertificateAuthority.pvk MyFakeRootCertificateAuthority.cer -len 2048
        ```

    Varmista, että seuraavat tiedostot luodaan:

    - MyFakeRootCertificateAuthority.cer

    - MyFakeRootCertificateAuthority.pvk

4. Suorita testi päämyöntäjän asentamiseksi seuraava komento:

        ```
        certutil.exe -addstore Root MyFakeRootCertificateAuthority.cer
        ```

5. Testaa varmenteiden myöntäjä avulla voit luoda www.contoso.com varmennetta:

        ```
        makecert -sk pkey -iv MyFakeRootCertificateAuthority.pvk -a sha256 -n "CN=www.contoso.com" -ic MyFakeRootCertificateAuthority.cer -sr localmachine -ss my -sky exchange -pe
        ```

6. **Mmc** -komennon suorittaminen ja lisää Varmenteet-laajennuksen paikallisen tietokoneen tietokoneen-tili.

7. Valitse */Certificates (paikallinen tietokone) / henkilökohtainen/varmenteen/* tallentaa, voit viedä www.contoso.com varmenteen ja siihen yksityinen avain www.contoso.com.pfx-tiedostoon:

    [! [20]][20]

8. Palauttaa Azure-portaaliin ja muodosta yhteys hallinta tason AM (*ra aad-ntier-hallintoraportointi-vm1*). Käyttäjän oletusnimi on *testuser* salasanalla *AweS0me@PW*:

    [! [21]][21]
    
9. Hallinnan tason AM, muodosta yhteys kunkin web-tason VMs puolestaan käyttäjänimi *testuser* ja salasanan kanssa *AweS0me@PW*, ja suorita seuraavat toimet. Huomaa, että VMs yksityisten osoitteiden IP 10.0.1.4, 10.0.1.5 ja 10.0.1.6:

    >[AZURE.NOTE] Web-tason VMs on vain yksityinen IP-osoitteet. Voit muodostaa yhteyden niihin vain käyttämällä hallinnan tason AM.

    1. Kopioi tiedostot *www.contoso.com.pfx* ja *MyFakeRootCertificateAuthority.cer* testi asiakastietokoneesta.
    
    2. Avaa komentokehote järjestelmänvalvojana, pitämällä kopioimasi tiedostot-kansioon ja suorita seuraavat komennot:
    
        ```
        certutil.exe -privatekey -importPFX my www.contoso.com.pfx NoExport

        certutil.exe -addstore Root MyFakeRootCertificateAuthority.cer
        ```

    3. Suorita `mmc` komento, Lisää Varmenteet-laajennuksen paikallisen tietokoneen tietokoneen-tili ja varmistaa, että seuraavat varmenteet on asennettu:

        - \Certificates (paikallinen tietokone) \Personal\Certificates\ www.contoso.com

        - \Certificates (paikallinen tietokone) \Trusted pääkansion varmenteiden Authorities\Certificates\MyFakeRootCertificateAuthority

    4. Käynnistä Internet Information Services (IIS) hallinta-konsolin ja siirry *Sites\Default sivuston* tietokoneeseen.

    5. Valitse *Toiminnot* -ruudusta *sidontojen*ja https-sidonta, käyttämällä www.contoso.com SSL-varmenteen lisääminen:

        [! [22]][22]

10. Palaa testi asiakastietokoneen ja varmista, että voit nyt selata https://www.contoso.com.

### <a name="create-an-azure-active-directory-tenant"></a>Azure Active Directory-vuokraajan luominen

1. Azure-portaalissa luominen Azure Active Directory-vuokraajan, kuten *myaadname*. onmicrosoft.com-sivustolla, jossa *myaadname* on valittu kansionimi.

2. Lisää käyttäjä nimellä *järjestelmänvalvojan* hakemiston GlobalAdmin-roolin kanssa. Pane merkille juuri luotu salasana.

3. Internet Explorerissa Siirry https://account.activedirectory.windowsazure.com/ ja kirjaudu sisään admin@ *myaadname*. onmicrosoft.com. Voit vaihtaa salasanan pyydettäessä.

### <a name="create-and-deploy-a-test-web-application"></a>Luo ja testaa web-sovelluksen käyttöönotto

1. Visual Studio käyttämällä kehittäminen tietokoneessa ja luoda ASP.NET-Web-sovelluksen nimi ContosoWebApp1 (Käytä .NET Framework 4.5.2):

    [! [23]][23]

2. *MVC* -malli, muuta todennus *työ- ja oppilaitoksen tilit*ja määritä AAD vuokraajan nimi. Älä luo sovelluksen-palvelun pilveen.

    [! [24]][24]

3. Muodosta ja testaa todennus sovelluksen käyttämiseen. Anna Kirjaudu sisään-sivulla sen tilinimen admin@ *myaadname*. onmicrosoft.com, Anna salasana ja valitse sitten *Kirjaudu sisään*:

    [! [25]][25]

4. Varmista, että AAD pyytää oikeutta kirjautuminen ja lukea profiilin ja aloittaa web-sovelluksen käytön tunnus järjestelmänvalvoja.

5. Sulje Internet Explorer ja palaa Visual Studio.

6. Avaa seuraavan koodin korostetut, ja `<appSettings>` *HVT: PostLogoutRedirectUri* avaimen arvo-osassa *https://www.contoso.com:443 /*. Tallenna tiedosto.

7. *Ratkaisunhallinta* -ikkunassa ContosoWebApp1 projektin hiiren kakkospainikkeella, valitse *Julkaise*.

8. *Julkaise* -ikkunassa Valitse *Mukautettu*. Luo mukautettu profiilia *ContosoWebApp1*.

9. *Yhteyden* -sivulla määrittää *Julkaise menetelmä* *Tiedostojärjestelmässä* ja määritä *ContosoWebApp*, helposti sijainnissa kehittäminen tietokoneeseen-nimiseen kansioon.

10. Määritä *asetukset* -sivun *määritysten* *versioon*.

11. *Esikatselun* sivulla Valitse *Julkaise*.

12. Yhteyden muodostaminen kunkin verkkopalvelin puolestaan (joko hallinnan tason AM) ja suorita seuraavat toimet:

    1. Kopioi *ContosoWebApp* -kansio ja sen sisältö development tietokoneesta *C:\inetpub* -kansioon.

    2. Käytä Internet Information Services (IIS) hallinta-konsolin, siirry *Sites\Default sivuston* tietokoneeseen.

    3. Valitse *Toiminnot* -ruudun *Peruste asetuksia*ja muuttaa sivuston fyysinen polku *%SystemDrive%\inetpub\ContosoWebApp*:

        [! [26]][26]

### <a name="publish-the-test-web-application-through-aad"></a>Testi-web-sovelluksen kautta AAD julkaiseminen

1. Kirjaudu sisään Azure-portaaliin ja siirry AAD hakemistoon.

2. Valitse *sovellukset* -välilehdessä ContosoWebApp1-sovellus.

3. Varmista, että sovelluksesi lisätään onnistuneesti kansio ja valitse sitten *Määritä* -välilehti.

4. Muuta https://www.contoso.com:443 *KIRJAUDU edelleen URL-osoite* ja määritä *Vastaa URL-Osoitteen* https://www.contoso.com:443 (URL-Osoitetta).

5. Tallenna määritykset.

6. Siirry testi asiakastietokoneen https://www.contoso.com. Varmista, että AAD pyytää tunnistetietosi ja kirjaudu sitten.

>[AZURE.NOTE] Tämä on ensimmäinen skenaarion päätepiste: n tason verkkosovelluksen käynnissä pilveen ulkoisten käyttäjien kanssa AAD antamisen salasanan todennus käyttämään ottaminen käyttöön.

### <a name="create-a-simulated-on-premises-environment-with-active-directory"></a>Luo Simuloitu paikalliseen ympäristöön Active Directory-hakemistosta

Paikallisen ympäristön sisältää kahden toimialueen ohjauskoneen, `contoso.com` toimialueen ja kaksi palvelinten isännöimiseen Azure AD Connect synkronoida palvelu. Palvelimet isännöimiseen Azure AD Connect ei toimialue-liitettyinä.

1. Palaa Resurssienhallinnassa komentosarjat-kansio, jossa on N-taso-verkkosovelluksen luomiseen käytetty komentosarja.

2. Parametrit-kansioon, Lisää toinen sub-kansio nimeltä `Onpremise`.

3. Lataa parametrit/paikallisesti käytettävät versiot kansioon seuraavat tiedostot:

    - [Lisää-Lisää-toimialue-controller.parameters.json][add-adds-domain-controller-parameters]

    - [Luo-Lisää-metsää-extension.parameters.json][create-adds-forest-extension-parameters]

    - [virtualMachines adc.parameters.json][virtualMachines-adc-parameters]

    - [virtualMachines adc-joindomain.parameters.json][virtualMachines-adc-joindomain-parameters]

    - [virtualMachines adds.parameters.json][virtualMachines-adds-parameters]

    - [virtualNetwork.parameters.json][virtualNetwork-parameters]

    - [virtualNetwork-Lisää dns.parameters.json][virtualNetwork-adds-dns-parameters]

5. Muokkaa komentosarjat-kansiossa käyttöönotto ReferenceArchitecture.ps1-tiedostoa ja muuta seuraava rivi Määritä resurssiryhmä, jotka on luotu tai käyttää AM ja resurssien luoma komentosarja:

        ```powershell
        # PowerShell
        $resourceGroupName = "ra-aad-onpremise-rg"
        ```

6. Muokkaa seuraavat parametrit/paikallisesti käytettävät versiot-kansiossa olevat tiedostot ja Aseta `resourceGroup` arvo `virtualNetworkSettings` kohdassa sama kuin nämä tiedostot, jonka olet määrittänyt käyttöönotto ReferenceArchitecture.ps1-komentosarjatiedosto.

    - virtualMachines adds.parameters.json

    - virtualMachines adc.parameters.json

    - virtualNetwork.parameters.json

    - virtualNetwork-Lisää dns.parameters.json

7. Avaa PowerShellin Azure-ikkuna, komentosarjat-kansioon ja suorita seuraava komento:

        ```powershell
        .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> Windows onpremise
        ```

    Korvaa `<subscription id>` käyttämällä Azure tilauksen.

    Saat `<location>`, Määritä Azure alue, kuten `eastus` tai `westus`.

### <a name="install-and-configure-the-azure-ad-connect-sync-service"></a>Asenna ja Määritä Azure AD Connect synkronointi-palvelu

Esitelty seuraavasti määritys koostuu Azure AD Connect synkronointi-palvelun kaksi esiintymää. Ensimmäinen on aktiivinen, toinen ollessa määritettynä väliaikaisen tilassa antamaan nopean automaattisesti ja suuren käytettävyyden, jos ensimmäisen palvelimen epäonnistuu.

1. Käytä Azure portaalin, resurssiryhmän pitämällä VMs Azure AD Connect-synkronointipalvelut (*ra-aad-paikallisesti käytettävät versiot-rg* oletusarvon mukaan), siirry ja muodosta yhteys *ra aad-paikallisesti käytettävät versiot-adc-vm1* AM. Kirjaudu sisään *testuser* salasanalla *AweS0me@PW*.

2. Lataa Azure AD Connect [tähän][aad-connect-download].

3. Suorita Azure AD Connect installer ja *Mukauta* -vaihtoehtoa käytettäessä asennusoikeutta.

    >[AZURE.NOTE] Et voi käyttää *Pika-asetuksia* -vaihtoehto, koska AM isännöintipalvelu Azure AD Connect synkronointi ei ole toimialueeseen liittymistä.

4. Jätä *Asenna tarvittavat osat* -sivulla valinnaiset asetukset tyhjäksi, Hyväksy oletusasetukset.

5. Valitse *käyttäjien sisäänkirjautumisessa* -sivulla *Salasanojen synkronoinnin*.

6. Anna *Muodosta yhteys Azure AD* -sivulla admin@ *myaadname*. onmicrosoft.com-sivustolla, jossa *myaadname* on AAD alihallintaan. Kirjoita järjestelmänvalvojatilin salasana.

7. Määritä *muodostaa yhteyttä hakemistoja* -sivulla metsää contoso.com (Kirjoita arvo, koska se ei näy avattavasta luettelosta), kirjoita CONTOSO\testuser käyttäjänimi, Määritä AweS0me@PW salasanan, ja valitse sitten *Lisää hakemisto*.

8. *Azure AD - kirjautuminen määritys* -sivulla Hyväksy täydellinen käyttäjätunnus oletusarvo. Valitse *Jatka ilman mitään tarkistetut toimialueet*.

    >[AZURE.NOTE] Contoso.com hakemiston lueteltujen *Ei ole vahvistettu*. Voit vahvistaa toimialuenimeä, sinun on luotava toimialuenimen Rekisteröintipalvelun TXT-tietue. Tässä esimerkissä paikallisen toimialueen ei ole rekisteröity ulkoisesti. Lisätietoja on kohdassa [lisääminen mukautettua toimialuenimeä Azure Active Directory][aad-custom-directory].

9. Valitse *toimialue ja OU suodatus* -sivulla *Synkronoi kaikki toimialueiden ja organisaatioyksiköiden* (oletusarvo).

10. *Yksilöivä käyttäjät* -sivulla hyväksy oletusarvot.

11. Valitse *Suodata käyttäjille ja laitteille* -sivulla *Synkronoi kaikille käyttäjille ja laitteille* (oletusarvo).

12. Valitse *salasana takaisinkirjoituksen* *valinnaisia ominaisuuksia* -sivulla.

13. *Voit määrittää asetukset* -sivulla Valitse *Aloita synkronointi kun määritys on valmis*, mutta jätä *väliaikaisen tila käyttöön* valitsematta.

14. Varmista, että määritysprosessi päättyy virheettömät ja sulje sitten asennusohjelma.

15. Azure-portaalista muodostaa *ra aad-paikallisesti käytettävät versiot-adc-vm2* AM. Kirjaudu sisään *testuser* salasanalla *AweS0me@PW*.

16. Lataa Azure AD Connect ja suorittamalla asennusohjelma.

17. Ohjatun ja vastata kuvatulla tavalla vaiheet 3 – 12.

18. Valitse *Valmis määrittäminen* -sivulla *Aloita synkronointi kun määritys on valmis*ja **valita myös** *väliaikaisen tila käyttöön*. Tämä aiheuttaa Azure AD Connect synkronointi-palvelu noutaa tietoja tileistä ja objektien toimialueen contoso.com ja tallentaa ne sen tietokannan, mutta se ei lähettää nämä tiedot AAD asiakasympäristöön.

14. Varmista, että määritysprosessi päättyy virheettömät ja sulje sitten asennusohjelma.

### <a name="test-the-aad-configuration"></a>AAD määritysten testaaminen

1. Käytä Azure portaalin, siirry AAD hakemistossa, Avaa Azure Active Directory-sivu, *käyttäjät*ja ryhmät ja valitse sitten Näytä käyttäjät ja ryhmät synkronoida kansion *kaikki käyttäjät* . Seuraavien tilien käyttäjien pitäisi näkyä seuraavat:

    - järjestelmänvalvojan (admin@ *myaadname*. onmicrosoft.com)

    - Paikallisen Hakemistopalvelutilin synkronoinnin (Sync_ADC1_*nnnnnnnnnnnn*@*myaadname*. onmicrosoft.com)

    - Paikallisen Hakemistopalvelutilin synkronoinnin (Sync_ADC2_*nnnnnnnnnnnn*@*myaadname*. onmicrosoft.com)

2. Azure-portaalissa Siirry pitämällä AD DS: N toimialueen ohjaimet (*ra-aad-paikallisesti käytettävät versiot-rg* oletusarvon mukaan) VMs resurssiryhmä ja Yhdistä *ra aad-paikallisesti käytettävät versiot-ad-vm1* AM. Kirjaudu sisään *testuser* salasanalla *AweS0me@PW*.

3. *Active Directoryn käyttäjät ja tietokoneet* -konsolin avulla lisätä uuden toimialuekäyttäjän nimeltä Selma Tibbot kirjautumisnimi dianet@contoso.com. Määritä valittua salasanan. Tee käyttäjän Järjestelmänvalvojat-ryhmän jäsen (tämä on vain, jotta voit kirjautua paikallisesti tämän käyttäjänä myöhemmin - vain koneet toimialueessa on ohjauskoneita).

4. Siirry *ra aad-paikallisesti käytettävät versiot-adc-vm1* AM, Avaa PowerShell-ikkuna ja suorittamalla seuraavat komennot:

        ```[powershell]
        Import-Module ADSync
        Start-ADSyncSyncCycle -PolicyType Delta
        ```

    Varmista, että komento palauttaa *onnistui*.

    >[AZURE.NOTE] Tämä vaihe on tarpeen, koska oletusarvoisesti synkronointi on suunniteltu toimimaan 30 minuutin välein. Nämä komennot pakottaa synkronoidaan välittömästi.

5. Palauttaa Azure-portaaliin, siirry Azure Active Directory-sivu AAD-kansioon, Avaa, valitse *käyttäjät ja ryhmät*ja valitse *kaikki käyttäjät*. Pitäisi tulla näkyviin Selma Tibbot (dianet@ *myaadname*. onmicrosoft.com) näkyvät käyttäjien luettelo.

6. Azure Active Directory-sivu- *Yrityksen*ja valitse sitten *kaikki sovellukset*. Valitse *ContosoWebApp1* -sovellus ja valitse sitten *käyttäjät ja ryhmät*. Valitse työkalurivillä valitsemalla *Lisää*. Valitse *käyttäjät ja ryhmät*ja valitse *Selma Tibbot*. Valitse *Määritä*.

7. Internet Explorerissa Siirry sivuston https://account.activedirectory.windowsazure.com. Kirjaudu sisään dianet@ *myaadname*. onmicrosoft.com entisellään salasanalla.

    >[AZURE.NOTE] Valitse sovellusten luettelosta ContosoWebApp-kuvaketta. Etsi web-sovelluksen julkisesti luettelossa DNS-osoitteessa www.contoso.com, joka on eri web tason kuormituksen osoitteesta pyritään AAD.

8. Valitse *profiili* -välilehti. (Jos olet määrittänyt mitään, kun se on luotu) käyttäjän tiedot näytetään.

    >[AZURE.NOTE] Jos valitset *Muuta salasana*, sinun ei sallita voivat suorittaa tämän tehtävän, kun tämä toiminto on otettava käyttöön järjestelmänvalvojan ensin. Lisätietoja on ohjeaiheessa [käytön aloittaminen salasanojen hallinta][aad-password-management].

### <a name="enable-on-premises-users-to-run-the-application-by-using-authentication-through-aad"></a>Paikallisen käyttäjät voivat suorittaa todennusta ja AAD käyttämällä sovelluksen

1. Palaa *ra aad-paikallisesti käytettävät versiot-ad-vm1* toimialueen ohjauskoneen AM.

2. Avaa *DNS-hallinta* -konsolin ja Lisää uusi Host (isäntä)-tietue www.contoso.com. Määritä web tason kuormituksen julkiseen IP-osoite.

3. Kopioi tiedosto *MyFakeRootCertificateAuthority.cer* testi asiakastietokone (luomasi tähän kuvattuja [Simulate julkisen sivuston määrittäminen](#simulate-configuration-of-a-public-web-site)
    
4. Avaa komentokehote järjestelmänvalvojana, pitämällä tiedoston, jonka kopioit juuri-kansioon ja suorittamalla seuraavan komennon:

        ```
        certutil.exe -addstore Root MyFakeRootCertificateAuthority.cer
        ```

5. Internet Explorerissa Siirry https://www.contoso.com. Tarkista, että ContosoWebApp1 web-sovelluksen AAD kirjautumisen sivu tulee näkyviin.

6. Kirjaudu sisään dianet@ *myaadname*. onmicrosoft.com. Suorita sovellus ja kirjautuminen oikein.

## <a name="next-steps"></a>Seuraavat vaiheet

- Lue [Lisää paikallisen toimialueen Azure laajentaminen] parhaat käytännöt[adds-extend-domain]

- Lue [Lisää-resurssin metsää luomisen] parhaat käytännöt[ adds-resource-forest] Azure-tietokannassa

<!-- links -->
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[script]: #sample-solution-script
[implementing-a-multi-tier-architecture-on-Azure]: ./guidance-compute-n-tier-vm.md
[active-directory-domain-services]: https://technet.microsoft.com/library/dd448614.aspx
[active-directory-federation-services]: https://technet.microsoft.com/windowsserver/dd448613.aspx
[azure-active-directory]: ../active-directory-domain-services/active-directory-ds-overview.md
[azure-ad-connect]: ../active-directory/active-directory-aadconnect.md
[ad-azure-guidelines]: https://msdn.microsoft.com/library/azure/jj156090.aspx
[aad-explained]: https://youtu.be/tj_0d4tR6aM
[aad-editions]: ../active-directory/active-directory-editions.md
[guidance-adds]: ./guidance-iaas-ra-secure-vnet-ad.md
[sla-aad]: https://azure.microsoft.com/support/legal/sla/active-directory/v1_0/
[azure-multifactor-authentication]: ../multi-factor-authentication/multi-factor-authentication.md
[aad-conditional-access]: ../active-directory//active-directory-conditional-access.md
[aad-dynamic-membership-rules]: ../active-directory/active-directory-accessmanagement-groups-with-advanced-rules.md
[aad-dynamic-memberships]: https://youtu.be/Tdiz2JqCl9Q
[aad-user-sign-in]: ../active-directory/active-directory-aadconnect-user-signin.md
[aad-sync-requirements]: ../active-directory/active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md
[aad-topologies]: ../active-directory/active-directory-aadconnect-topologies.md
[aad-filtering]: ../active-directory/active-directory-aadconnectsync-configure-filtering.md
[aad-scalability]: https://blogs.technet.microsoft.com/enterprisemobility/2014/09/02/azure-ad-under-the-hood-of-our-geo-redundant-highly-available-distributed-cloud-directory/
[aad-connect-sync-default-rules]: ../active-directory/active-directory-aadconnectsync-understanding-default-configuration.md
[aad-identity-protection]: ../active-directory/active-directory-identityprotection.md
[aad-password-management]: ../active-directory/active-directory-passwords-customize.md
[aad-application-proxy]: ../active-directory/active-directory-application-proxy-enable.md
[aad-connect-sync-operational-tasks]: ../active-directory/active-directory-aadconnectsync-operations.md#staging-mode
[aad-custom-domain]: ../active-directory/active-directory-add-domain.md
[aad-powershell]: https://msdn.microsoft.com/library/azure/mt757189.aspx
[aad-sync-disaster-recovery]: ../active-directory/active-directory-aadconnectsync-operations.md#disaster-recovery
[aad-sync-best-practices]: ../active-directory/active-directory-aadconnectsync-best-practices-changing-default-configuration.md
[aad-adfs]: ../active-directory/active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs
[aad-health]: ../active-directory/active-directory-aadconnect-health-sync.md
[aad-health-adds]: ../active-directory/active-directory-aadconnect-health-adds.md
[aad-health-adfs]: ../active-directory/active-directory-aadconnect-health-adfs.md
[aad-agent-installation]: ../active-directory/active-directory-aadconnect-health-agent-install.md
[aad-reporting-guide]: ../active-directory/active-directory-reporting-guide.md
[azure-cli]: ../virtual-machines-command-line-tools.md
[azure-powershell-download]: ../powershell-install-configure.md
[solution-script]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/Deploy-ReferenceArchitecture.ps1
[vnet-parameters-windows]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/windows/virtualNetwork.parameters.json
[nsg-parameters-windows]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/windows/networkSecurityGroups.parameters.json
[webtier-parameters-windows]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/windows/webTier.parameters.json
[businesstier-parameters-windows]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/windows/businessTier.parameters.json
[datatier-parameters-windows]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/windows/dataTier.parameters.json
[managementtier-parameters-windows]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/windows/managementTier.parameters.json
[makecert]: https://msdn.microsoft.com/library/windows/desktop/aa386968(v=vs.85).aspx
[add-adds-domain-controller-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/add-adds-domain-controller.parameters.json
[create-adds-forest-extension-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/create-adds-forest-extension.parameters.json
[virtualMachines-adds-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/virtualMachines-adds.parameters.json
[virtualNetwork-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/virtualNetwork.parameters.json
[virtualNetwork-adds-dns-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/virtualNetwork-adds-dns.parameters.json
[virtualMachines-adc-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/virtualMachines-adc.parameters.json
[virtualMachines-adc-joindomain-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/virtualMachines-adc-joindomain.parameters.json
[aad-connect-download]: http://www.microsoft.com/download/details.aspx?id=47594
[aad-custom-directory]: ../active-directory/active-directory-add-domain.md
[aad-password-management]: ../active-directory/active-directory-passwords-getting-started.md#enable-users-to-reset-their-azure-ad-passwords
[adds-extend-domain]: ./guidance-identity-adds-extend-domain.md
[adds-resource-forest]: ./guidance-identity-adds-resource-forest.md
[0]: ./media/guidance-identity-aad/figure1.png "Cloud tunnistetietojen arkkitehtuuri Azure Active Directoryn avulla"
[1]: ./media/guidance-identity-aad/figure2.png "Yksi metsää, yksi AAD directory topologian"
[2]: ./media/guidance-identity-aad/figure3.png "Useiden puuryhmien yksittäisen AAD directory topologian"
[4]: ./media/guidance-identity-aad/figure5.png "Väliaikaisen palvelimen topologian"
[5]: ./media/guidance-identity-aad/figure6.png "Useita AAD kansioiden topologian"
[6]: ./media/guidance-identity-aad/figure7.png "Mukautetun asennuksen Azure AD yhteyden synkronointiin tietyn SQL Server-esiintymän valitseminen"
[7]: ./media/guidance-identity-aad/figure8.png "SSO menetelmä käyttäjien sisäänkirjautumisessa määrittäminen"
[8]: ./media/guidance-identity-aad/figure9.png "Toimialueen ja OU suodatusasetukset"
[9]: ./media/guidance-identity-aad/figure10.png "Salasana: takaisinkirjoituksen ottaminen käyttöön"
[10]: ./media/guidance-identity-aad/figure11.png "Azure AD-hallinnan sivu-portaalissa"
[11]: ./media/guidance-identity-aad/figure12.png "Azure AD Connect-konsolin"
[12]: ./media/guidance-identity-aad/figure13.png "Synkronoinnin palvelun hallinta-kohdassa Toiminnot-välilehti"
[13]: ./media/guidance-identity-aad/figure14.png "Yhdysviivat-välilehdestä synkronoinnin palvelun hallinta"
[14]: ./media/guidance-identity-aad/figure15.png "Synkronoinnin säännöt-editori"
[15]: ./media/guidance-identity-aad/figure16.png "Azure Active Directory yhteyden kunto-sivu näyttää synkronoinnin kunto Azure-portaalissa"
[16]: ./media/guidance-identity-aad/figure17.png "Azure AD DS: N kunto näkyy portaalissa Azure Active Directory yhteyden kunto-sivu"
[17]: ./media/guidance-identity-aad/figure18.png "Suojauksen raportteja käytettävissä Azure-portaalissa"
[18]: ./media/guidance-identity-aad/figure19.png "Azure portaalin korostaminen web tason kuormituksen julkiseen IP-osoite"
[19]: ./media/guidance-identity-aad/figure20.png "Siirry web tason kuormituksen julkiseen IP-osoite Internet Explorerissa"
[20]: ./media/guidance-identity-aad/figure21.png "Varmenteet-laajennuksen www.contoso.com varmenteen näyttäminen"
[21]: ./media/guidance-identity-aad/figure22.png "Yhteyden muodostaminen hallinnan tason AM"
[22]: ./media/guidance-identity-aad/figure23.png "Oletussivuston HTTPS sidonta luominen"
[23]: ./media/guidance-identity-aad/figure24.png "ContosoWebApp1 web-sovelluksen luominen"
[24]: ./media/guidance-identity-aad/figure25.png "ContosoWebApp1 web-sovelluksen todennus-ominaisuuksien määrittäminen"
[25]: ./media/guidance-identity-aad/figure26.png "Kirjautuminen Azure AAD ContosoWebApp1 WWW-sovelluksesta"
[26]: ./media/guidance-identity-aad/figure27.png "Oletussivuston kansiota muuttaminen"