<properties
    pageTitle="Asetukset ja tiedot roaming usein kysytyt kysymykset | Microsoft Azure"
    description="On vastauksia joihinkin kysymyksiin IT-järjestelmänvalvojien voi olla tietoja sovelluksen tietojen synkronointi."
    services="active-directory"
    keywords="yrityksen state sijaitsevaa asetukset windows pilvipalveluun, usein kysytyt kysymykset-yrityksen tilan roaming"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor="curtand"/>

<tags
    ms.service="active-directory"  
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>

# <a name="settings-and-data-roaming-faq"></a>Asetukset ja tiedot roaming usein kysytyt kysymykset

Tässä artikkelissa vastataan joihinkin kysymyksiin IT-järjestelmänvalvojille on ehkä sovellus tietojen synkronointi tietoja.

## <a name="what-data-roams"></a>Tietoja roams?
**Windows-asetukset**: PC-asetuksia, jotka ovat valmiina Windows-käyttöjärjestelmässä. Yleensä nämä ovat asetukset, Mukauta Tietokoneeltasi ja ne sisältävät laajan seuraaviin:

- *Teeman*, joka sisältää ominaisuuksia, kuten työpöydän teema ja tehtäväpalkin asetuksia.
- *Internet Explorer-asetukset*, mukaan lukien viimeksi avattu välilehdet ja Suosikit.
- *Reunan Selaimen asetukset*, kuten Suosikit ja lukuruudun luettelosta.
- *Salasanat*, esimerkiksi Internet-salasanat ja Wi-Fi-profiileista.
- *Kieliasetusten määrittäminen*, joka sisältää asetukset näppäimistöasetteluja, järjestelmän kieli, päivämäärä ja aika.
- *Helppokäyttötoiminnot Accessin ominaisuuksia*, kuten suurikontrastista teemaa, Narrator ja Suurennuslasi.
- *Muut Windows-asetukset*, kuten komentokehote asetukset ja sovellusluettelo.


**Sovelluksen tietoja**: Yleinen Windows-sovellusten kirjoittaa asetuksia tietojen sijaitsevaa kansioon ja tämän kansion kirjoitetut tiedot synkronoituvat automaattisesti. Kannattaa yksittäisiä sovelluksen kehittäjän suunnitteluun sovelluksen, jotta voit hyödyntää tätä ominaisuutta. Lisätietoja siitä, kuinka voit tehdä yleinen Windows-sovellusta, joka käyttää roaming-artikkelissa [appdata tallennustilan API](https://msdn.microsoft.com/library/windows/apps/mt299098.aspx) - ja [Windows 8: n appdata sijaitsevaa Sovelluskehittäjän blogi](http://blogs.msdn.com/b/windowsappdev/archive/2012/07/17/roaming-your-app-data.aspx).

## <a name="what-account-is-used-for-settings-sync"></a>Minkä tilin käytetään asetusten synkronointi?
Windows 8: ssa ja Windows 8.1-asetusten synkronointi käytetään aina kuluttaja Microsoft-tilit. Yrityksen edellytti mahdollisuus yhdistää niiden Active Directory-toimialuetilin asetusten synkronointi käyttämään Microsoft-tili. Windows 10: Tämä on yhdistetty toimintoja korvataan ensisijainen ja toissijainen tilin Framework Microsoft-tili.

Ensisijainen tili on määritetty tili, jota käytetään Windows kirjautuminen. Tämä voi olla Microsoft-tiliä, Azure Active Directory (Azure AD)-tilin, paikallisen Active Directory-tilin tai paikallinen tili. Lisäksi ensisijaisen tilin Windows 10-käyttäjät voivat lisätä vähintään yhden toissijaisen cloud tilit niiden laitteeseen. Toissijainen tili on yleensä Microsoft-tiliä, Azure AD-tili tai muu tili kuten Gmail- tai Facebook. Toissijainen tilit antaa lisäpalveluita, kuten kertakirjautumisen ja Windows-kaupan, mutta niitä ei voi ajan asetusten synkronointi.

Windows 10: vain laitteen ensisijainen tiliä voi käyttää asetusten synkronointi (katso [miten päivitetään-Microsoft-tilin asetusten synkronointi Windows 8: Azure AD asetusten synkronointi Windows 10: ssä?](active-directory-windows-enterprise-state-roaming-faqs.md#How-do-I-upgrade-from-Microsoft-account-settings-sync-in-Windows-8-to-Azure-AD-settings-sync-in Windows-10?)).

Tiedot on aina sekoitus laitteeseen toisen käyttäjän-tilien välillä. Asetukset-synkronointia varten kaksi sääntöjen:

- Windows-asetukset aina vierailla ensisijainen tilillä.
- Sovelluksen tiedot merkitty tili, jota käytetään hankkia sovelluksen. Vain sovellukset, jotka on merkitty ensisijaisen tilin Synkronoi. Sovelluksen omistajuus tunnisteita määritetään, kun sovellus on side ladata siirtymällä Windows-kaupan tai mobiililaitteiden hallinta (MDM).

Jos omistaja sovellus ei tunnisteta, se vierailla ensisijainen tilillä. Jos laite on päivitetty Windows 8: ssa tai Windows 8.1 Windows 10, kaikki sovellukset olla merkitty, kuten hankkii Microsoft-tiliä. Tämä johtuu siitä Useimmat käyttäjät hankkia sovelluksia Windows-kaupan kautta sekä ei ole Windows-kaupan tuki Azure AD-tilit ennen Windows 10: ssä. Jos sovellus on asennettu kautta on offline-käyttöoikeus, sovellus olla merkitty tilillä ensisijainen laitteeseen.

>[AZURE.NOTE]  
> Windows 10-laitteissa, jotka ovat yrityksen omistamien ja Azure AD yhdistettyjen voi enää muodostaa niiden Microsoft-tilien toimialueen tiliin. Microsoft-tiliin yhdistäminen toimialuetiliä ja on kaikkien käyttäjien tietojen synkronointi Microsoft-tilille (eli Microsoft-tiliä roaming yhdistetyn Microsoft-tilin ja Active Directory-toiminnon kautta) poistetaan Windows 10-laitteissa, jotka on liitetty liitetty Active Directory- tai Azure AD-ympäristöön.

## <a name="how-do-i-upgrade-from-microsoft-account-settings-sync-in-windows-8-to-azure-ad-settings-sync-in-windows-10"></a>Miten päivitetään-Microsoft-tilin asetusten synkronointi Windows 8: Azure AD asetusten synkronointi Windows 10: ssä?
Jos tietokone on liitetty Active Directory-toimialueen, joka on käynnissä Windows 8: ssa tai Windows 8.1 yhdistetyn Microsoft-tilillä, voit synkronoida asetukset Microsoft-tilisi kautta. Kun on päivitetty Windows 10: een, voit jatkaa synkronoida käyttäjäasetusten Microsoft-tilin kautta, kunhan toimialueeseen liittymistä-käyttäjänä ja Active Directory-toimialueen ei ole yhteydessä Azure AD.

Jos paikallisen Active Directory-toimialueen yhteyttä Azure AD-laitteen yrittää synkronoida asetusten avulla yhdistetyt Azure AD-tili. Jos Azure AD-järjestelmänvalvoja ole käyttöön yrityksen tilan Roaming, että yhdistetyn Azure AD-tilin ne eivät synkronoidu enää asetukset. Jos kirjaudut Azure AD-jäsenyyden Windows 10-käyttäjänä, alkaa synkronointi Windowsin asetukset, kun järjestelmänvalvoja on myöntänyt asetusten kautta Azure AD-synkronointi.

Jos kaikki henkilökohtaiset tiedot tallennettu yrityksen laitteessa, sinun olisi otettava huomioon, että Windows-Käyttöjärjestelmää ja sovelluksen tietojen aloittaa synkronoinnin, Azure AD. Tämä on seuraavat vaikutukset:

- Henkilökohtainen Microsoft-tilin asetukset alueen työsi ryömintä lukuun ottamatta asetuksia tai oppilaitoksen Azure AD-tilit. Tämä johtuu siitä, että synkronointi Microsoft-tili ja Azure AD-asetuksia käytetään nyt erilliset tilit.
- Henkilökohtaisia tietoja, esimerkiksi Wi-Fi salasanat, tunnistetiedot verkko ja Internet Explorerin Suosikit aiemmin yhdistetyn Microsoft-tilin kautta synkronoidut synkronoidaan Azure AD kautta.


## <a name="how-do-microsoft-account-and-azure-ad-enterprise-state-roaming-interoperability-work"></a>Miten Microsoft-tili ja Azure AD yrityksen tilan Roaming yhteensopivuus työn?
Windows 10 marraskuun 2015 tai sitä uudemmissa versioissa yrityksen tilan Roaming tuetaan vain yhteen asiakkaaseen kerrallaan. Jos kirjautuminen Windows käyttämällä on työpaikan tai oppilaitoksen Azure AD-tili, kaikki tiedot synkronoituvat Azure AD kautta. Jos kirjautuminen Windows käyttämällä omalla Microsoft-tilillä, kaikki tiedot synkronoituvat Microsoft-tilin kautta. Yleinen appdata vierailla, käyttämällä vain ensisijainen kirjautumisen tiliä laitteessa ja se vierailla vain, jos sovelluksen käyttöoikeuden omistaa ensisijaisen tilin. Yleinen appdata minkä tahansa toissijaisen tilin omistaa sovellusten ei voi synkronoida.

## <a name="do-settings-sync-for-azure-ad-accounts-from-multiple-tenants"></a>Asetukset Azure AD-tileissä-useita alihallinnat, jotka synkronoidaan?
Kun useita Azure AD tilejä eri Azure AD alihallinnat, jotka ovat samassa laitteessa, sinun on päivitettävä laitteen rekisterin pitää yhteyttä kunkin Azure AD-vuokraajan Azure Rights Management (Azure RMS).  

1. Etsi kunkin Azure AD-vuokraajan GUID-tunnus. Avaa Azure perinteinen portaalin ja Azure AD-vuokraajan. Vuokraajan GUID-tunnus on URL-osoite selaimen osoiteriville. Esimerkki: `https://manage.windowsazure.com/YourAccount.onmicrosoft.com#Workspaces/ActiveDirectoryExtension/Directory/Tenant GUID/directoryQuickStart`
2. Kun olet GUID-tunnus, sinun on lisättävä rekisteriavain **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\SettingSync\WinMSIPC\<vuokraaja tunnus GUID >**.
Luoda uusia usean merkkijonoarvo (REG-usean-SZ) **AllowedRMSServerUrls** **vuokraajan tunnus GUID-tunnus** -näppäintä. Määritä sen tietoja käyttöoikeuksien myöntämistä jakauman pisteen URL-osoitteet muut Azure alihallinnat, joka käyttää laite.
3. Löydät käyttöoikeuksien myöntämistä jakauman pisteen URL-osoitteet suorittamalla **Get-AadrmConfiguration** cmdlet-komento. Jos **LicensingIntranetDistributionPointUrl** ja **LicensingExtranetDistributionPointUrl** arvot ovat erilaiset, Määritä molemmat arvot. Jos arvot ovat samat, määritä arvo vain kerran.


## <a name="what-are-the-roaming-settings-options-for-existing-windows-desktop-applications"></a>Mitkä ovat olemassa olevan Windows-työpöytäsovellusten sijaitsevaa asetusten asetukset?
Roaming toimii vain Yleinen Windows-sovellusten. Ottaminen käyttöön Valitse olemassa olevan Windows-työpöytäsovellus roaming käytettävissä on kaksi vaihtoehtoa:

- [Työpöydän silta](http://aka.ms/desktopbridge) avulla voit tuoda Windows aiemmin Työpöytäsovelluksen yleinen Windows-ympäristössä. Tässä näkymässä mahdollisimman vähän muutokset on suoritettava hyödyntää Azure AD-sovelluksen tiedot-palvelimesta. Työpöydän siltaan on sovellukset app jäsenyyden, joka tarvitaan, voit ottaa käyttöön sovelluksen tietojen roaming aiemmin Työpöytäsovelluksen sovellusten kanssa.
- [Käyttäjän kokemus Virtualization (luokkaan UE ASTI-V)](https://technet.microsoft.com/library/dn458947.aspx) avulla voit luoda mukautettuja asetuksia olemassa olevan Windows-työpöytäsovellusten ja roaming Win32-sovellusten käyttöön. Tämä asetus ei edellytä sovelluksen kehittäjä muuttamisen sovelluksen. Luokkaan UE ASTI V on rajoitettu paikallisen Active Directory roaming asiakkaille, jotka on ostettu Microsoft Desktop optimointi Pack.

Järjestelmänvalvojat voivat määrittää luokkaan UE ASTI-V siirtymisen Windowsin työpöytäsovellus tietoja muuttamalla roaming Windows-Käyttöjärjestelmää asetukset-ja Yleinen sovelluksen tietojen [luokkaan UE ASTI V ryhmitellä käytäntöjä](https://technet.microsoft.com/itpro/mdop/uev-v2/configuring-ue-v-2x-with-group-policy-objects-both-uevv2), mukaan lukien:

- Windows-asetusten Ryhmäkäytäntö vierailla
- Windows-sovellusten Ryhmäkäytäntö ei synkronoida
- Internet Explorerin roaming sovellukset-osassa

Microsoft voi tutkia myöhemmin, tee luokkaan UE ASTI V monitasoisissa integroitu Windows-ja laajentaa luokkaan UE ASTI-V siirtymisen asetuksia Azure AD-pilven kautta.


## <a name="can-i-store-synced-settings-and-data-on-premises"></a>Voin tallentaa synkronoidussa tiliasetuksia ja datatiedoston paikallinen?
Yrityksen tilan Roaming tallentaa kaikki synkronoidut tiedot Azure pilveen. Luokkaan UE ASTI V tarjoaa paikallisen roaming ratkaisu.

## <a name="who-owns-the-data-thats-being-roamed"></a>Kuka omistaa tiedot, jotka on parhaillaan siirryttävästä?
Oman yritykset tiedot siirryttävästä yrityksen tilan Roaming kautta. Tiedot tallennetaan Azure palvelinkeskukseen. Kaikki käyttäjätiedot salataan sekä muut käyttämällä Azure RIGHTS pilveen siirron. Tämä on Microsoftin asiakaspohjaiset asetusten synkronointi, joka salaa vain tietyt luottamuksellisia tietoja esimerkiksi käyttäjän tunnistetietoja, ennen kuin se jättää laitteen verrattuna parantaminen.

Microsoft on sitoutunut suojaaminen asiakastiedot. Azure RIGHTS salattu enterprise-käyttäjän asetusten tiedot automaattisesti, ennen kuin se jättää Windows 10-laitteeseen, jotta muut käyttäjät voivat lukea nämä tiedot. Jos organisaatiolla on maksetun tilauksen Azure RIGHTS varten, voit Azure RIGHTS muita ominaisuuksia, kuten seuraa ja kumota asiakirjoja, automaattisesti Suojaa sähköpostiviestejä, jotka sisältävät luottamuksellisia tietoja, ja hallita omia näppäimet ("Tuo oma avain-ratkaisun, eli BYOK). Lisätietoja näistä toiminnoista ja Azure RIGHTS toiminnasta artikkelissa [Azure Rights Management ominaisuudet](https://technet.microsoft.com/jj585026.aspx).

## <a name="can-i-manage-sync-for-a-specific-app-or-setting"></a>Tietyn sovelluksen tai asetus synkronoinnin hallinta
Windows 10: ssä ei ole MDM tai Ryhmäkäytäntö asetusta roaming yksittäisiä sovelluksen käytöstä. Vuokraajan järjestelmänvalvojat voivat poistaa käytöstä appdata Synkronoi kaikki sovellukset hallitun laitteessa, mutta ei ole tarkempaan hallintaan app kohti tai sovelluksen sisällä tasolla.

## <a name="how-can-i-enable-or-disable-roaming"></a>Miten voin ottaa käyttöön tai poistaminen käytöstä roaming?
Siirry **Settings** -sovellukseen **tilit** > **synkronoinnin asetuksia**. Tällä sivulla näet, mitä tiliä käytetään siirtymisen asetukset, ja voit ottaa käyttöön tai poistaa käytöstä asetuksia, jotka voidaan siirryttävästä yksittäiset ryhmät.

## <a name="what-is-microsofts-recommendation-for-enabling-roaming-in-windows-10"></a>Mikä on Microsoftin toiminnon ottaminen käyttöön roaming Windows 10: ssä?
Microsoft on roaming ratkaisuja, jotka ovat saatavilla, mukaan lukien käyttäjäprofiileja, luokkaan UE ASTI V ja yrityksen tilan sijaitsevaa muutamalla eri asetukset.  Microsoft on sitoutunut tekemään sijoitus Enterprise esitettävä Roaming tulevissa versioissa Windows. Jos organisaatiosi ei ole valmis tai perehtynyt tietojen siirtämistä pilvipalveluun, sitten Suosittelemme käyttämään luokkaan UE ASTI V oman ensisijainen sijaitsevaa tekniikka. Jos organisaatiosi edellyttää roaming olemassa olevan Windows-työpöytäsovellusten tuki, mutta se Haluatko siirtyminen pilveen, on suositeltavaa, että käytät sekä yrityksen tilan sijaitsevaa luokkaan UE ASTI V. Vaikka luokkaan UE ASTI V ja yrityksen tilan Roaming ovat hyvin samankaltaisia tekniikoita, ne eivät ole toisensa poissulkevia. Ne täydentävät toisiaan varmistaa, että organisaation tarjoaa sijaitsevaa palveluita käyttäjät tarvitsevat.  

Kun käytät yrityksen tilan Roaming ja luokkaan UE ASTI V, voimassa ovat seuraavat säännöt:

- Yrityksen tilan Roaming on ensisijainen sijaitsevaa agentti laitteeseen. Luokkaan UE ASTI V käytetään täydentää "Win32 välin."
- Luokkaan UE ASTI V roaming Windows-asetukset ja nykyaikaisen UWP sovelluksen tiedot poistetaan käytöstä, kun käyttämällä luokkaan UE ASTI V-ryhmän käytännöt. Nämä jo jäävät yrityksen tilan Roaming.

## <a name="how-does-enterprise-state-roaming-support-virtual-desktop-infrastructure-vdi"></a>Miten yrityksen tilan Roaming tukee virtual desktop infrastructure (VDI)?
Yrityksen tilan Roaming tuetaan Windows 10: ssä asiakkaan tuotteissa, mutta se ei ole palvelimessa tuotteissa. Jos asiakkaan AM nykyisessä hypervisor tietokoneeseen ja etäyhteyden virtuaalikoneen kirjautumisen, tiedot vierailla. Jos käyttäjien etäyhteyden kirjauduttava palvelimeen työpöytäversiota takaamiseksi usea käyttäjä voi jakaa saman OS, roaming ei ehkä toimi. Jälkimmäisessä istunnon perustuva skenaariota ei tue virallisesti.


## <a name="what-happens-when-my-organization-purchases-azure-rms-after-using-roaming"></a>Mitä tapahtuu, kun organisaation ostaa Azure RIGHTS roaming käyttämisen jälkeen?
Jos organisaatiossa on jo käytössä roaming Windows 10: Azure RIGHTS rajoitettu käyttö vapaa tilaukseen, olet ostanut Azure RIGHTS maksetun tilauksen ei ole mitään vaikutusta sijaitsevaa-toiminnon toimintoja ja ei ole määritysmuutoksia tarvitaan IT-järjestelmänvalvoja.

## <a name="known-issues"></a>Tunnetut ongelmat

- Jos yrität kirjautua Windows-laitteen käyttämällä älykortin tai virtual älykorttia, asetusten synkronointi lakkaa toimimasta. Päivitykset Windows 10: een voi ratkaista ongelman.
- Tarvitset heinäkuussa kumulatiivisen päivityksen synkronointi toimii Internet Explorerin Suosikit for Windows 10: ssä (koontiversio 10586.494 tai uudempi).
- Tiedot, jotka on suojattu Windowsin tietojen suojaaminen ei synkronoida yrityksen tilan Roaming kautta. Lisäksi tietokoneissa, joissa on käytössä Windows tietojen suojaaminen ei ilmene teeman Synkronoi. 
- Tietyissä olosuhteissa yrityksen tilan Roaming voi epäonnistua synkronoi tietoja, jos Azure Monimenetelmäisen todentamisen on määritetty.
    - Jos laite on määritetty edellyttämään [Monimenetelmäisen todentamisen](multi-factor-authentication.md) Azure Active Directory-portaalissa, saattaa epäonnistua, voit synkronoida asetukset allekirjoittamisen salasanalla Windows 10-laitteeseen. Tällaista Monimenetelmäisen todentamisen määrittäminen tarkoitus on suojata Azure järjestelmänvalvojatilin. Järjestelmänvalvojille edelleen ehkä kirjautumalla niiden Windows 10-laitteisiin, joiden omaa [Microsoft Passport työkäyttöön](active-directory-azureadjoin-passport.md) PIN-tunnus tai täyttämällä Monimenetelmäisen todentamisen muita Office 365: n Azure palveluita käyttäessään synkronointia.
    - Synkronointi voi epäonnistua, jos järjestelmänvalvoja määrittää Active Directory Federation Services Monimenetelmäisen todentamisen ehdollinen käyttöoikeuskäytäntö ja käyttöoikeustietue laitteeseen vanhenee.  Varmista, kirjaudu sisään ja Kirjaudu käyttämällä [Microsoft Passport työn](active-directory-azureadjoin-passport.md) PIN-tunnus tai suorittaa muita Office 365: n Azure palveluita käyttäessään Monimenetelmäisen todentamisen.

- Jos kone on toimialueen liittyneet automaattisen rekisteröinnin Azure Active Directory-laitteet, se saattaa ilmetä synkronointi epäonnistuu, jos tietokone on sähköntuotannosta pitkän ajan ja toimialueen todennusta ei voi suorittaa loppuun. Voit ratkaista ongelman, Yhdistä tietokoneen yritysverkon niin, että sync voit jatkaa.


## <a name="related-topics"></a>Aiheeseen liittyviä ohjeita
- [Yrityksen tilan sijaitsevaa yleiskatsaus](active-directory-windows-enterprise-state-roaming-overview.md)
- [Roaming Azure Active Directoryn enterprise-tila käyttöön](active-directory-windows-enterprise-state-roaming-enable.md)
- [Ryhmän käytännön ja asetusten synkronointi Mobiililaitteiden asetukset](active-directory-windows-enterprise-state-roaming-group-policy-settings.md)
- [Windows 10: ssä sijaitsevan asetukset viittaus](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md)
