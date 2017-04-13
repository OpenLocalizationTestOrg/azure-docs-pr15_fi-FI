<properties 
    pageTitle="Suojauksen parhaat käytännöt Azure MFA"
    description="Tässä tiedostossa on parhaita käytäntöjä ympärille käyttämällä Azure MFA Azure-tilien kanssa"
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtland"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="security-best-practices-for-using-azure-multi-factor-authentication-with-azure-ad-accounts"></a>Suojauksen parhaat käytännöt Azure Monimenetelmäisen todentamisen Azure AD-tilien kanssa

Jos helpottamiseen todennusprosessi, monimenetelmäisen todentamisen on ensisijainen valinta useimmille organisaatioille. Azure multi-factor Authentication (MFA) avulla niiden tietosuojaan ja vaatimustenmukaisuuteen vaatimukset täyttävän käyttämisessä yksinkertainen kirjautuminen niiden käyttäjien yritykset. Tässä artikkelissa neuvotaan parhaista käytännöistä, jotka tulee ottaa huomioon, kun suunnittelet Azure MFA hyväksyminen.

## <a name="best-practices-for-azure-multi-factor-authentication-in-the-cloud"></a>Azure Monimenetelmäisen todentamisen pilveen parhaat käytännöt
Jotta antaa kaikki käyttäjät monimenetelmäisen todentamisen ja ota laajennetut ominaisuudet, joita Azure Monimenetelmäisen todentamisen tarjoaa edut, sinun on Azure Monimenetelmäisen todentamisen kaikkien käyttäjien käyttöön.  Tämä tehdään käyttämällä jotakin seuraavista toimista:

- Azure AD Premium- tai Enterprise-liikkuvuus Suite
- Multi-factor Auth toimittaja

### <a name="azure-ad-premiumenterprise-mobility-suite"></a>Azure AD Premium tai yrityksen Mobility Suite

![EMS](./media/multi-factor-authentication-security-best-practices/ems.png)

Hyväksymällä Azure MFA Azure AD Premium-tai Enterprise Mobility Suite pilveen suositellut ensimmäiseksi on käyttöoikeuksien määrittäminen käyttäjille.  Azure Monimenetelmäisen todentamisen kuuluu seuraavat tuotepakettien ja sellaisenaan organisaation ei tarvitse mitään muita siirtämisestä monimenetelmäisen todentamisen ominaisuuksien kaikille käyttäjille.

Kun Monimenetelmäisen todentamisen määrittäminen Ota huomioon seuraavat:

- Sinun ei tarvitse luoda multi-factor Auth toimittajaa.  Azure AD-Premium- ja Enterprise-liikkuvuus Suite sisältyy Azure Monimenetelmäisen todentamisen.  Jos luot Auth tarjoajan voi hakea kaksinkertainen laskutettu.
- Azure AD Connect on vain vaatimus, jos haluat synkronoida paikallisen Active Directory-ympäristön Azure Active Directory-hakemistosta. Jos käytät vain Azure AD-kansio, joka ei ole synkronoitu paikallisen Active Directory esiintymään, Azure AD Connect ei tarvitse.


### <a name="multi-factor-auth-provider"></a>Multi-factor Auth toimittaja

![Multi-factor Auth toimittaja](./media/multi-factor-authentication-security-best-practices/authprovider.png)

Jos sinulla ei ole Azure AD Premium- tai Enterprise-liikkuvuus Suite hyväksymällä Azure MFA pilveen suositeltu ensimmäiseksi on luotava MFA Auth tarjoajan. Vaikka MFA on oletusarvoisesti yleinen järjestelmänvalvojille, jotka on Azure Active Directory, kun otat MFA organisaation haluat laajentaa kaikki käyttäjät ja tee multi-factor authentication-ominaisuus on multi-factor Auth palveluntarjoaja.
Valittaessa Auth-palvelu on Valitse kansio ja toimi seuraavasti:

- Sinun ei tarvitse Azure AD-directory multi-factor Auth palveluntarjoaja luomiseen.
- Sinun on liitettävä Azure Active directory multi-factor Auth-palvelu, jos haluat laajentaa monimenetelmäisen todentamisen kaikki käyttäjät ja/tai haluat oman Yleiset järjestelmänvalvojat voivat käyttää ominaisuuksia, kuten hallinta-portaalin, mukautetut tervehdykset ja raporttien.
- Dirsync-työkalun tai AAD Sync ovat vain pyyntö, jos haluat synkronoida paikallisen Active Directory-ympäristön Azure Active Directory-hakemistosta. Jos käytät vain Azure AD-kansio, joka ei ole synkronoitu paikallisen Active Directory esiintymän kanssa, sinun ei tarvitse Dirsync-työkalun tai AAD Sync.
- Jos sinulla on Azure AD Premium- tai Enterprise-liikkuvuus Suite, sinun ei tarvitse luoda multi-factor Auth toimittajaa. Haluat määrittää käyttäjän käyttöoikeudet ja sitten voivat aloittaa MFA ottaminen käyttäjille.
- Varmista, että voit valita oikean käyttö-mallin yrityksen (auth tai käytössä käyttäjäkohtainen), kun olet valinnut käyttö-malli ei voi muuttaa sitä.

### <a name="user-account"></a>Käyttäjätilin
Kun otat käyttöön MFA käyttäjiä, käyttäjätilejä voi olla jokin kolmesta core tilasta: käytöstä, käytössä tai voimassa.
Alla olevia ohjeita avulla voit varmistaa, että käytät käyttöönoton parhaiten sopiva vaihtoehto:

- Kun käyttäjän tila on määritetty ei käytössä, käyttäjä ei käytetä monimenetelmäisen todentamisen. Tämä on oletustilaan.
- Kun käyttäjän tila on käytössä, se tarkoittaa sitä, että käyttäjä on käytössä, mutta ei ole valmis rekisteröinnin loppuun. Ne pyydetään suorittamaan seuraavan kirjauduttaessa sisään. Tämä asetus ei vaikuta muiden selaimen sovellukset. Kaikki sovellukset säilyvät toimi, ennen kuin Rekisteröintiprosessi on valmis.
- Kun käyttäjän tila on pakotettu, se tarkoittaa, että käyttäjä voi tai ehkä ole suorittanut rekisteröinti. Jos ne on valmis rekisteröitymisen osallistujat käyttävät monimenetelmäisen todentamisen. Muutoin käyttäjää pyydetään rekisteröinti Viimeistele seuraava kirjauduttaessa sisään. Tämä asetus vaikuta muiden selaimen sovellukset. Nämä sovellukset eivät toimi, kunnes ohjelmasalasanat Luo ja niitä käytetään.
- Käytettävissä on artikkelissa [Azure Monimenetelmäisen todentamisen pilveen aloittaminen](multi-factor-authentication-get-started-cloud.md) käyttäjän ilmoitus-mallin avulla voit lähettää sähköpostia koskeva MFA käyttöönoton käyttäjille.

### <a name="supportability"></a>Supportability

Koska käyttäjät suurin osa on tottuneille todentaa vain salasanojen avulla, on tärkeää yrityksesi tuo tilatieto koskeva Tämä toimenpide kaikille käyttäjille. Tämä tilatieto pienentää todennäköisyys, että käyttäjien Ota yhteyttä tukipalveluun vähäisiä MFA seurantakohteita.
On kuitenkin joitakin poistaminen käytöstä tilapäisesti MFA missä tarvittavat skenaarioita. Käytä alla olevia ohjeita ymmärtää käsittelemisestä tilanteisiin:

- Varmista, että teknisen tuen edustajat ovat koulutus kahvaa skenaariot, jossa mobiilisovelluksen tai puhelinnumero on saa ilmoituksen tai puhelu ja käyttäjä ei voi kirjautua sisään tästä syystä. He voivat ottaa käyttöön erikseen ohitus-vaihtoehto, jos haluat, että käyttäjä todennetaan "ohittaminen" monimenetelmäisen todentamisen yhdellä kertaa. Ohituksen ovat väliaikaisia ja päättyy määritetyn ajan kuluttua.
- Tarvittaessa voit hyödyntää Azure MFA Luotetut IP-osoitteet-ominaisuus. Tämän ominaisuuden avulla järjestelmänvalvojat hallitun tai liitetyt vuokraajan voi ohittaa monimenetelmäisen todentamisen käyttäjille, jotka ovat sovellukseen on kirjauduttu sisään yrityksen Paikallinen intranet. Ominaisuudet ovat käytettävissä Azure AD-alihallinnat, jotka on Azure AD Premium, yrityksen Mobility Suite tai Azure Monimenetelmäisen todentamisen käyttöoikeuksia.


## <a name="best-practices-for-azure-multi-factor-authentication-on-premises"></a>Azure Monimenetelmäisen todentamisen paikallisen parhaat käytännöt
Jos yrityksesi päättänyt hyödyntää omassa infrastruktuurin käyttöön MFA, se on tarvittavat ottamaan Azure multi-factor Authentication Server paikallisen. MFA-palvelin-osat näkyvät seuraavassa kuvassa:

![Multi-factor Auth tarjoajan](./media/multi-factor-authentication-security-best-practices/server.png)
*asennu oletusarvoisesti ** asennettu, mutta ei käytössä oletusarvoisesti


Azure multi-factor Authentication Server voidaan suojata cloud ja paikallisen resursseja, joita voi käyttää Azure AD-tilin.  Tämä voi kuitenkin vain kirjautumalla liitetyn viestinnän avulla.  On eli on AD FS ja sen Azure AD-vuokraajan äänipuheluita.
Kun multi-factor Authentication-palvelimen asetusten määrittämisestä Ota huomioon seuraavat:

- Jos olet suojaaminen Azure AD-resursseja, käyttämällä Active Directory-liittoutumispalvelut sitten todennus 1st kerrointa suoritetaan paikallisen käyttämällä AD FS ja 2. kerroin on tehtävä paikallisen honoring vaatimus.
- Se ei ole tarpeen, että AD FS-liittoutumispalvelinten palvelimeen on asennettu Azure multi-factor Authentication Server kuitenkin multi-factor Authentication sovittimen AD FS: lle on oltava asennettuna Windows Server 2012 R2 AD FS käynnissä. Voit asentaa palvelimen toiseen tietokoneeseen, kunhan se on tuettu versio ja asentaa AD FS-sovittimen erikseen AD FS-liittoutumispalvelinten palvelimeen. Katso alla kuvattuja ohjeita sovittimen asentamista erikseen.
- Monimenetelmäisen todentamisen AD FS sovittimen ohjattu asennus luo kutsutaan PhoneFactor järjestelmänvalvojat Active Directoryn käyttöoikeusryhmän ja lisää sitten AD FS-palvelutilin federation palvelun tässä ryhmässä. On suositeltavaa, että tarkistat toimialueen ohjauskoneen, että PhoneFactor Järjestelmänvalvojat-ryhmän varmasti luodaan ja AD FS palvelun tili on ryhmän jäsen. Tarvittaessa lisätä AD FS-palvelutilin manuaalisesti toimialueen ohjauskoneen PhoneFactor Järjestelmänvalvojat-ryhmään.

### <a name="user-portal"></a>Käyttäjän Portal
Portaalin toimii Internet Information Server (IIS) sivuston, jonka avulla Omatoiminen ominaisuuksia ja on käyttäjien hallinnan ominaisuuksia koko sarja. Voit määrittää tämän osan käyttämällä alla olevia ohjeita:

- IIS 6 tai uudempi tarvitaan
- ASP.NET-v2.0.507207 on asennettu ja rekisteröity
- Tämä palvelin voidaan ottaa käyttöön ympäröivän verkon.



### <a name="app-passwords"></a>Sovelluksen salasanat
Jos organisaatiosi on liitetty käyttäminen Azure AD SSO ja aiot käyttää Azure MFA, valitse huomioon seuraavat ohjelmasalasanat käytettäessä (muista, että tämä koskee vain liittoutuneille (SSO) käytetään):

- Ohjelman salasana on tarkistanut Azure AD ja ohittaa tämän vuoksi federation. Liitetty viestintä käytetään vain silloin, kun sovelluksen salasanan määrittäminen.
- Käyttäjien salasanat tallennetaan liitetyt (SSO) organisaation tunnus. Jos käyttäjä jättää yrityksen, näitä tietoja on flow organisaation tunnus reaaliajassa Dirsync-työkalun avulla. Tilin poistaminen käytöstä tai poistaminen saattaa kestää synkronointia varten on enintään 3 tuntia viivästyy Azure AD-sovelluksen salasanan poistaminen käytöstä tai poistaminen.
- Paikallisen asiakkaan käyttöoikeuksien valvonta-asetukset eivät ole voimassa App salasanalla
- Paikallisen tarkistusta kirjaaminen / valvonta-ominaisuus on käytettävissä sovelluksen salasana
- Lisää käyttäjän education tarvitaan Microsoft Lync 2013-asiakasohjelmassa.
- Tietyt kehittyneet suunnitelmat voi vaatia organisaation käyttäjänimi ja salasana ja sovelluksen salasanat yhdistelmän avulla käytettäessä monimenetelmäisen todentamisen sen mukaan, missä ne todentavat asiakkaiden kanssa. Asiakkaat, jotka todennetaan paikallisen infrastruktuuri käyttäisi organisaation käyttäjänimi ja salasana. Asiakkaille, jotka todennetaan Azure AD-käyttäisi sovelluksen salasana.
- Oletusarvon mukaan käyttäjät eivät voi luoda sovelluksen salasanat-, jos yrityksesi edellyttää, että tai jos haluat sallia käyttäjien luoda ohjelman salasana joissakin tilanteissa Varmista, että vaihtoehto Salli käyttäjien luominen sovelluksen salasanat kirjautumalla selaimen sovellukset on valittuna.

## <a name="additional-considerations"></a>Muita huomioon otettavia seikkoja
Alla olevassa luettelossa avulla voit selvittää muutamia muita huomioon otettavia seikkoja ja parhaita käytäntöjä kullekin osalle, joka on otettu käyttöön-ympäristöön:

Menetelmä|Kuvaus
:------------- | :------------- |
[Active Directory Federation-palvelu](multi-factor-authentication-get-started-adfs.md)|Tietoja Azure Monimenetelmäisen todentamisen AD FS määrittämisestä.
[SÄDE Authenticaton](multi-factor-authentication-get-started-server-radius.md)|  Tietoja määritys ja jonka SÄDE Azure MFA-palvelimen määrittäminen.
[IIS-todennus](multi-factor-authentication-get-started-server-iis.md)|Tietoja määritys ja Azure MFA-palvelimen määrittäminen IIS: N kanssa.
[Windows-Authenticaton](multi-factor-authentication-get-started-server-windows.md)|  Tietoja määritys ja Azure MFA-palvelimen määrittäminen Windows-todennuksen kanssa.
[LDAP-todennus](multi-factor-authentication-get-started-server-ldap.md)|Tietoja määritys ja Azure MFA-palvelimen määrittäminen LDAP-todennuksen kanssa.
[Työpöydän yhdyskäytävän ja Azure multi-factor Authentication Server RADIUS käyttäminen](multi-factor-authentication-get-started-server-rdg.md)|  Tietoja määritys ja Azure MFA-palvelimen määrittäminen Remote Desktop Gatewayn RADIUS kanssa.
[Windows Server Active Directory-synkronointiin](multi-factor-authentication-get-started-server-dirint.md)|Tietoja asetukset ja määrittämisestä Active Directory ja Azure MFA-palvelimen välisen synkronoinnin.
[Käyttöönotto Azure Monimenetelmäisen todentamisen Server Mobile sovelluksen Web-palvelu](multi-factor-authentication-get-started-server-webservice.md)|Valitse asetukset ja Azure MFA palvelimen WWW-palvelun määrittäminen tiedot.
[Azure Monimenetelmäisen todentamisen kokoonpanon Lisäasetukset VPN](multi-factor-authentication-advanced-vpn-configurations.md)|LDAP-tai RADIUS Cisco ASA Citrix Netscaler ja Juniper/Pulse suojatun VPN laitteiden määrittämisestä.


## <a name="additional-resources"></a>Lisäresursseja
Tässä artikkelissa korostukset parhaista käytännöistä Azure MFA, liittyy muita resursseja, voit käyttää myös MFA-käyttöönoton suunnittelun aikana. Seuraavassa luettelossa on auttaa sinua vaihdon avaimen seuraavissa artikkeleissa:

- [Azure Monimenetelmäisen todentamisen raporteissa](multi-factor-authentication-manage-reports.md)
- [Määritä Azure Monimenetelmäisen todentamisen toiminta](multi-factor-authentication-end-user-first-time.md)
- [Azure Monimenetelmäisen todentamisen usein kysytyt kysymykset](multi-factor-authentication-faq.md)
