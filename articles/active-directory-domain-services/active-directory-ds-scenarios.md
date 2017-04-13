<properties
    pageTitle="Azure Active Directory-toimialueen palveluista: Käyttöönottoskenaariot | Microsoft Azure"
    description="Käyttöönoton tilanteita, joissa Azure AD-toimialueen palveluista"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/21/2016"
    ms.author="maheshu"/>


# <a name="deployment-scenarios-and-use-cases"></a>Käyttöönottoskenaariot ja käytä tapauksissa
Tässä osassa on Tarkista muutamia skenaarioita ja käytä-palvelupyynnöt, hyötyvät Azure Active Directory (AD)-toimialueen palveluista.

## <a name="secure-easy-administration-of-azure-virtual-machines"></a>Suojatun Azuren näennäiskoneiden asetusten hallinta
Azure Active Directory-toimialueen palveluista avulla voit hallita Azuren näennäiskoneiden virtaviivaistettu tavalla. Azuren näennäiskoneiden on liitetty hallitun toimialueeseen käyttöönottoon näin Kirjaudu sisään yrityksen AD tunnistetietojen avulla. Tämän menetelmän auttaa välttämään tunnistetietojen hallinta ongelmia, kuten ylläpito paikallisen Järjestelmänvalvojatileille kunkin Azuren näennäiskoneiden.

Palvelimen näennäiskoneiden, jotka on liitetty hallitun toimialueen voi hallita ja suojata ryhmäkäytännön avulla. Voit käyttää vaadittava perusaikataulujen Azuren näennäiskoneiden ja lukita ne yrityksen suojauksen ohjeiden mukaisesti. Voit esimerkiksi, millaisia sovelluksia, jotka voidaan käynnistää nämä näennäiskoneiden-ryhmäkäytännön hallintatoiminnot.

![Azuren näennäiskoneiden virtaviivaistettu hallinta](./media/active-directory-domain-services-scenarios/streamlined-vm-administration.png)

Kun palvelimia ja muut infrastruktuurin saavuttaa aika, Contoso siirtäminen tällä hetkellä isännöimät paikallisen pilveen useita sovelluksia. Niiden nykyisen IT-standardin valtuuttaa palvelinten isännöinnin yrityksen sovellukset on oltava toimialueen liittynyt ja hallittujen ryhmäkäytännön avulla. Contoson IT-järjestelmänvalvoja haluaa toimialueen liity näennäiskoneiden käyttöön Azure-tietokannassa, voit helpottaa hallinta. Tuloksena Järjestelmänvalvojat ja käyttäjät voivat Kirjaudu sisään yrityksen tunnistetietoja. Yhtä aikaa koneet voi määrittää edellyttämät pakolliset suojauksen perusaikatauluihin ryhmäkäytännön avulla. Contoson mieluummin ei tarvitse ottaa käyttöön, valvoa ja hallita toimialueen ohjaimet suojaamiseen Azuren näennäiskoneiden Azure-tietokannassa. Azure AD-toimialueen palveluista siksi tämän Käyttötapaus hyvien sopiva.

**Käyttöönoton muistiinpanot**

Ota tämä käyttöönottotapa seuraavat seikat huomioon:

- Hallitut toimialueiden Azure AD-toimialueen palveluista myöntämä muodostavat yhden tasainen OU (organisaatioyksikkö) rakenteen oletusarvoisesti. Kaikissa toimialueen liittyneet tietokoneissa sijaitsevat yksittäisen kiinteänä OU. Voit halutessasi sijaan voit luoda mukautetun organisaatioyksiköiden.

- Azure AD-toimialueen palveluista tukee yksinkertainen Ryhmäkäytäntö valmiin Ryhmäkäytäntöobjekti kullekin käyttäjien ja tietokoneiden muodossa säilöjen. Ei voi kohdistaa Ryhmäkäytännön OU tai osaston mukaan, suorittaa WMI-suodatus tai luoda mukautetun ryhmäkäytäntöobjekteja.

- Azure AD-toimialueen palveluista tukee perus AD tietokoneen objektin rakenne. Et voi laajentaa tietokoneen objektin rakenne.


## <a name="lift-and-shift-an-on-premises-application-that-uses-ldap-bind-authentication-to-azure-infrastructure-services"></a>Hissin-ja-vaihto Azure-infrastruktuuripalvelut LDAP sitominen todennusta käyttävä paikallisen sovellus

![LDAP-sitominen](./media/active-directory-domain-services-scenarios/ldap-bind.png)

Contoso on paikallisen sovellus, joka on ostettu ISV useita vuosia sitten. Sovellus on mukaan ISV ylläpito-tilassa ja pyytää sovelluksen tehdyt muutokset on korkeaksi Contoso. Tämän sovelluksen on verkkopohjainen edusta, joka kerää käyttäjän tunnistetietoja käyttäen verkkolomakkeen ja todentaa sitten suorittamalla yrityksen Active Directorysta LDAP-sitominen käyttäjät. Contoson haluat siirtää Azure-infrastruktuuripalvelut tämän sovelluksen. On suositeltavaa, että sovellus toimii on tarvitsematta muutokset. Lisäksi käyttäjien pitäisi onnistua todennuksen olemassa olevan yrityksen tunnistetietoja ja eikä sinun tarvitse suuntaamista käyttäjät voivat tehdä seuraavia toimintoja eri tavalla. Toisin sanoen loppukäyttäjät pitäisi olla oblivious, jossa sovellus on käynnissä ja siirron läpinäkyväksi tietyille käyttäjille.

**Käyttöönoton muistiinpanot**

Ota tämä käyttöönottotapa seuraavat seikat huomioon:

- Varmista, että sovellus ei tarvitse muokata/kirjoitus-kansioon. LDAP-kirjoitusoikeudet hallitun myöntämä Azure AD-toimialueen palvelut toimialueeseen ei tueta.

- Et voi muuttaa salasanoja suoraan hallitun toimialueen vastaan. Käyttäjät voivat vaihtaa oman salasanansa joko käyttämällä Azure AD käyttäjien Omatoiminen salasanan muuttaminen tai vastaan paikallisesta hakemistosta. Nämä muutokset ovat automaattisesti synkronoidut ja käytössä olevan hallitun toimialueen.


## <a name="lift-and-shift-an-on-premises-application-that-uses-ldap-read-to-access-the-directory-to-azure-infrastructure-services"></a>Hissin ja vaihto paikallisen-sovellus, joka käyttää LDAP lukea käyttämään Azure-infrastruktuuripalvelut kansio
Contoso on paikallisen-liiketoiminta (LOB)-sovelluksen, joka on kehitetty lähes vuosikymmenellä sitten. Sovellus on hakemiston aware ja on suunniteltu toimimaan Windows Server AD. Sovellus käyttää LDAP (Lightweight Directory Access Protocol) lukemaan käyttäjien tiedot tai määritteet Active Directorysta. Sovellus ei Muokkaa ominaisuuksia tai muussa kirjoittaa hakemiston. Contoson haluat siirtää tämän sovelluksen Azure-infrastruktuuripalvelut ja Keskeytä isännöinnin tällä hetkellä tämän sovelluksen erääntyminen paikallisen laitteen. Sovellus ei kirjoitettava Moderni directory API, kuten REST-pohjainen Azure AD-kaavio-Ohjelmointirajapinnan käyttäminen. Tämän vuoksi hissin ja vaihto-vaihtoehto halutun sovellus voidaan siirtää toimimaan ilman muokkaamalla koodi tai uudelleenkirjoitus sovelluksen pilvipalvelussa, joiden.

**Käyttöönoton muistiinpanot**

Ota tämä käyttöönottotapa seuraavat seikat huomioon:

- Varmista, että sovellus ei tarvitse muokata/kirjoitus-kansioon. LDAP-kirjoitusoikeudet hallitun myöntämä Azure AD-toimialueen palvelut toimialueeseen ei tueta.

- Varmista, että sovellus ei tarvitse mukautettu/laajennettu Active Directory-rakenne. Rakenteen tunnisteet ei tue Azure AD-toimialueen palveluista.


## <a name="migrate-an-on-premises-service-or-daemon-application-to-azure-infrastructure-services"></a>Siirtää Azure-infrastruktuuripalvelut paikallisen palvelun tai daemon sovellus
Jotkin sovellukset koostuvat useita tasoa, jossa tasoissa on suoritettava todennetut puhelut Taustajärjestelmä taso, kuten tietokannan taso. Active Directory-palvelutilejä käytetään yleensä Käytä tällaisissa tapauksissa. Voit tehdä hissin ja vaihto hakemukseen Azure-infrastruktuuripalvelut ja Azure AD-toimialueen palveluista nämä sovellukset identity-tarpeisiin. Voit käyttää samaa palvelutilin, jotka synkronoidaan paikallisen hakemistosta Azure AD. Voit vaihtoehtoisesti luo ensin mukautettu OU ja luo sitten kyseiseen OU: sta näiden sovellusten erillisessä palvelutilin.

![Palvelutilin avulla WIA](./media/active-directory-domain-services-scenarios/wia-service-account.png)

Contoso on custom-built ohjelmiston säilöön-sovellus, joka sisältää web-edusta, SQL server ja Taustajärjestelmä FTP-palvelimeen. Windows-integroitua todennusta palvelutilejä käytetään todentamiseen web-edusta FTP-palvelimeen. Web-edusta on määritetty palvelutili suorittamista. Sallivat web-edusta-palvelutili käytöltä on määritetty palvelimeen. Contoson ei haluaa on otettava käyttöön toimialueen ohjauskoneen virtual kone Siirry Azure-infrastruktuuripalvelut tämän sovelluksen pilveen. Contoson IT-järjestelmänvalvoja voi ottaa käyttöön isännöivän WWW-edusta ja SQL server Azure-virtuaalikoneissa FTP-palvelimen palvelimet. Nämä koneet liitettyinä sitten Azure AD-toimialueen palveluista hallitun toimialueeseen. Valitse he voivat käyttää samaa palvelutilin niiden paikallisesta hakemistosta sovelluksen todennusta varten. Tämä palvelutilin synkronoidaan Azure AD-toimialueen palveluista hallitun toimialueen ja on valmis käytettäväksi.

**Käyttöönoton muistiinpanot**

Ota tämä käyttöönottotapa seuraavat seikat huomioon:

- Varmista, että sovellus käyttää todennusta varten käyttäjänimi ja salasana. Todistus/älykortin perusteella todennus ei tue Azure AD-toimialueen palveluista.

- Et voi muuttaa salasanoja suoraan hallitun toimialueen vastaan. Käyttäjät voivat vaihtaa oman salasanansa joko käyttämällä Azure AD käyttäjien Omatoiminen salasanan muuttaminen tai vastaan paikallisesta hakemistosta. Nämä muutokset ovat automaattisesti synkronoidut ja käytössä olevan hallitun toimialueen.


## <a name="azure-remoteapp"></a>Azure RemoteApp
Azure RemoteApp ansiosta Contoson järjestelmänvalvojat voivat luoda toimialueen liittyneet kokoelman. Tämän ominaisuuden avulla remote sovellusten served mukaan Azure RemoteApp käyttäminen toimialueen liittyneet tietokoneissa ja muita resursseja käyttämällä Windows-integroitua todennusta. Contoson voit käyttää Azure AD-toimialueen palveluiden käyttämän Azure RemoteApp toimialueeseen liittyneet sivustokokoelmat hallitun toimialue.

![Azure RemoteApp](./media/active-directory-domain-services-scenarios/azure-remoteapp.png)

Lisätietoja tämä käyttöönottotapa artikkelista Remote Desktop Services-blogi [hissin ja vaihto oman työmääriä Azure RemoteApp ja Azure AD-toimialueen palveluista](http://blogs.msdn.com/b/rds/archive/2016/01/19/lift-and-shift-your-workloads-with-azure-remoteapp-and-azure-ad-domain-services.aspx).
