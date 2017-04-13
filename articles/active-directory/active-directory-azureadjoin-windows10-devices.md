<properties
    pageTitle="Windows 10-laitteissa käyttäminen työpaikalle | Microsoft Azure"
    description="Tarjoaa käyttäjille tilannevedoksen ominaisuuksia ja IT-kontrasti eri tavoista, joilla laitteen voidaan valmisteltu ja käyttää yrityksen Windows 10: ssä."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""
    tags="azure-classic-portal"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="femila"/>

# <a name="using-windows-10-devices-in-your-workplace"></a>Käyttäminen työpaikalle Windows 10-laitteissa

Koskee: Windows 10 PC-tietokoneeseen

Windows 10: ssä on kolme organisaatiot, joiden avulla käyttäjät voivat käyttää työresurssien suojatun ja kätevä tapa mallit.

- **Azure Active Directory liittyminen** (Azure AD-liitos), työntekijöiden, joka käyttää resursseja, kuten Office 365: n ensisijaisesti pilvipalvelussa. Azure AD-liitos ei Omatoiminen valmistelu kokemus, Windows 10: n uudet ominaisuudet.
- **Toimialueen join**-ja organisaatioissa, joissa on paikallisen sovellukset ja resursseja. Toimialueen liitos on parannettu sisäänrakennetun Windows 10: Azure AD yhteydessä.
- **Uuden yksinkertaistettu BYOD-version**käyttäjille, jotka haluat lisätä työpaikan tai oppilaitoksen Windows ja helposti resurssien Omat laitteissa.

Seuraavassa taulukossa esitellään tilannevedoksen ominaisuuksia käyttäjille ja IT-järjestelmänvalvojille, kontrasti eri tavoista, joilla laitteen voidaan valmisteltu ja käyttää yrityksen Windows 10:

|                                                                                                                                                                 | Toimialueen liitos     | Azure AD-liitos | Omat laite |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------|---------------|-----------------|
| Windowsin laite-kirjautuminen työpaikan tai oppilaitoksen tilit.                                                                                                                      | Kyllä             | Kyllä           | Ei              |
| Käyttäjä single-kertakirjautumisportaaliin (SSO) Office 365: ssä ja Azure AD-sovellukset. SSO ei voi kirjautua kerran organisaation resursseihin. | Kyllä             | Kyllä           | Kyllä             |
| Käyttäjän SSO Kerberos/NTLM-sovelluksiin.                                                                                                                                  | Kyllä             | Rajoitettu       | Kyllä, VPN-verkon kautta         |
| Vahva todennus ja helposti-kirjautuminen Microsoft Passport ja Windows Hei työpaikan tai oppilaitoksen tilit.                                                                   | Kyllä             | Kyllä           | Kyllä             |
| Työpaikan tai oppilaitoksen tilillä (ei ole Microsoft-tilillä) Windows-kaupan yrityksen pääsyä.                                                                                    | Kyllä             | Kyllä           | Kyllä             |
| Enterprise-yhteensopiva roaming asetukset yli laitteet, joissa on työpaikan tai oppilaitoksen tilit.                                                                                 | Kyllä             | Kyllä           | Kyllä             |
| Mahdollisuus rajoittaa organisaation sovellusten laitteita, jotka ovat yhteensopivia organisaation laitteen käytännöt.                                                      | Kyllä             | Kyllä           | Kyllä             |
| Käyttäjien Omatoiminen valmistelu laitteet-työstä missä tahansa".                                                                                                | Ei              | Kyllä           | Kyllä             |
| Mahdollisuus laitteiden hallinta.                                                                                                                                       | Kyllä, Ryhmäkäytännön/SCCM kautta | Kyllä           | Kyllä             |

## <a name="use-work-owned-devices-with-azure-ad-join-and-domain-join-in-windows-10"></a>Käytä työn omistaa laitteet, joissa on Azure AD-Kutsu osallistujat ja toimialueen liittyä Windows 10: ssä

Windows 10: ssä on työn omistaa laitteiden käyttää työresurssit kahdella tavalla:

- Azure AD-liitos
- Toimialueen liitos

 Molemmat voi olla vaihtoehdot organisaation tarpeiden ja vaatimusten mukaan. Joissakin organisaatioissa hyötyä lopettamista käyttöönoton ottaminen käyttöön.

## <a name="when-to-use-azure-active-directory-join"></a>Milloin kannattaa käyttää Azure Active Directory-liittyminen

Azure AD-liitos on uusi Omatoiminen työ valmistelu Windows 10-ratkaisun.  Se on suunnattu työntekijät, jotka käyttävät työresurssit, kuten Office 365: n ensisijaisesti pilvipalvelussa. On kevyt voi määrittää tietokoneilla, tableteilla ja yrityksen puhelimet. Laitteiden hallitaan kautta mobiililaitteiden hallinta käyttämällä Windows-ympäristöissä eri yhdenmukaisia ohjausobjektit.

**Käytä Azure AD liittyä johtua seuraavista syistä**:

- Haluat ottaa käyttöön Omatoiminen valmistelun laitteiden työntekijöiden työpöydän äärellä.
- Työn omistaa mobiililaitteella kuten taulutietokoneisiin ja matkapuhelimiin tarjota käyttäjille.
- Haluat hallita käyttäjäryhmälle Azure AD sijaan Active Directoryssa, kuten kausiluonteisista työntekijät, alihankkijat tai opiskelijoiden.
- Haluat antaa liittävä ominaisuuksia työntekijöiden remote toimipisteiden rajoitetun paikallisen infrastruktuuriin.
- Sinulla ei ole paikallisen Active Directory.

Joissakin organisaatioissa käyttää Azure AD liittyä ensisijainen tapa käyttöönotto työn omistaja-laitteet, erityisesti silloin, kun ne siirtää pilveen useimmat tai kaikki resursseille. Hybrid organisaatiot, joiden Active Directory- ja Azure AD voit myös valita käyttöön yksi keino tai toisen käyttäjän tai osaston mukaan.

Koulun paikallisalueita ja korkeakoulujen, esimerkiksi voi hallita Active Directoryn henkilökunnan ja Azure AD opiskelijat. Jotkin yritykset kannattaa hallita remote toimistot tai myynnin osastot Azure AD. Azure AD liity ja toimialueen liity menetelmiä voidaan hybrid organisaatiossa. Azure AD-liity voi olla toimialueen liittymisen käyttöönotto käsittely-ympäristö, mikä hyvien täydentää.

**Yritysresurssit eniten tavallista käyttöoikeus on pilvipalveluun, jos organisaatiosi voi nauttivat muita etuja, jos**:

- Voit poistaa paikallisen tunnistetietojen infrastruktuurin riippuvuuksia.
- Voidaan yksinkertaistaa laitteet-käyttöönoton mallin käytön poispäin imaging ratkaisuja sallimalla Omatoiminen määritys.
- Mobiililaitteiden hallinta voit hallitsemaan eri ympäristöissä kaikissa laitteissa.

Saat lisätietoja Azure AD liittyä [Windows 10-laitteita Azure Active Directory-liittyä Extending cloud ominaisuuksia](active-directory-azureadjoin-overview.md).

## <a name="when-to-use-domain-join-or-keep-using-it"></a>Milloin kannattaa käyttää toimialueen join (tai pidä sitä käytännössä)

Viimeinen 15 vuoden Monet organisaatiot ovat käyttäneet toimialueen liity muodostaa työn laitteet. Sen avulla käyttäjät voivat kirjautua laitteensa Active Directory-työpaikan tai oppilaitoksen tilit. Toimialueen liity avulla IT-hallita keskitetysti ja täysin seuraaviin laitteisiin. Organisaatioiden yleensä riippuvaisia imaging menetelmiä säännöstä laitteet ja käytä usein hallita niitä System Center Configuration Manager (SCCM) tai ryhmän käytännön (Ryhmäkäytännön).

**Yrityksen pitäisi käyttää toimialueen join (tai pidä sitä käytännössä) johtua seuraavista syistä**:

- Sinun on Win32-sovellusten käyttöön näiden NTLM/Kerberos käyttävät laitteet.
- Tarvitset Ryhmäkäytäntö tai SCCM/DCM laitteiden hallinta.
- Haluat jatkaa imaging ratkaisujen avulla voit työntekijöiden laitteiden määrittäminen.

**Toimialueen liity Windows 10: ssä myös antaa Azure AD yhteydessä seuraavat edut**:

- Vahvan laitteen sidottujen todennus ja helposti-kirjautuminen Microsoft Passport ja Windows Hei työpaikan tai oppilaitoksen tilit.
- Yrityksen Windows-kaupan Access laitteita, jotka käyttävät työpaikan tai oppilaitoksen tilit (ei Microsoft-tiliä tarvitaan).
- Enterprise-yhteensopiva roaming asetukset kaikissa laitteissa, jotka käyttävät työpaikan tai oppilaitoksen tilit (ei Microsoft-tiliä tarvitaan).
- Mahdollisuus rajoittaa organisaation sovellusten laitteita, jotka ovat yhteensopivia organisaation laitteen käytännöt.

Saat lisätietoja Azure AD liittyä [Extending cloud ominaisuuksia Windows 10-laitteisiin kautta Azure Active Directory-liity](active-directory-azureadjoin-overview.md).

## <a name="enable-joining-of-personally-owned-devices-for-work-or-school"></a>Ota käyttöön henkilökohtaisesti omistaa laitteiden työpaikan tai oppilaitoksen liittyminen

Yrityksen BYOD tukemaan Windows 10: ssä antaa käyttäjälle mahdollisuus työpaikan tai oppilaitoksen tilin lisääminen tietokoneessa, tabletissa tai puhelimessa. Sen jälkeen, kun käyttäjä lisää työpaikan tai oppilaitoksen tiliä, laite on rekisteröity Azure AD ja halutessasi rekisteröity mobiililaitteen hallintajärjestelmän, organisaatio on määrittänyt. Hakemiston näkyy seuraaviin laitteisiin "Rekisteröity" ja Azure AD liittyneet. IT-administraors käyttää eri käytännöt tarjoaa vaaleampi touch henkilökohtaisesti omistaa laitteiden kuin työn omistaa laitteissa halutessasi tietojen perusteella.

Käyttäjien voit lisätä on työpaikan tai oppilaitoksen tilillä niiden Omat laitteeseen hyvin helposti. Käyttäjä voi tehdä tämä käytettäessä työ-sovelluksen ensimmäistä kertaa tai käyttäjä voi tehdä sen manuaalisesti asetukset-valikon kautta. Tällä tilillä on SSO organisaation resurssien.

Saat lisätietoja Azure AD liittyä [Windows 10: lle Azure AD Connect toimialueeseen liittyneet laitteiden kohtaa](active-directory-azureadjoin-devices-group-policy.md).

## <a name="enable-domain-join-or-azure-ad-join"></a>Toimialueen join- tai Azure AD Join ottaminen käyttöön

Kaikki menetelmät edempänä (toimialueen liity, Azure AD-Kutsu osallistujat ja lisää työpaikan tai oppilaitoksen tili) on pikakuvakkeet Windows 10: n käyttökokemusta. Kaikki edellyttävät kuitenkin käyttöön infrastruktuuri toimintoja, ennen kuin kokemus toimivat IT-järjestelmänvalvoja.

## <a name="requirements-for-deploying-azure-ad-join"></a>Vaatimukset käyttöönotto Azure AD liittyminen

Ottamaan kullekin käyttäjien Azure AD liittyminen edellyttää seuraavia:

- Azure AD-tilaus.
- Azure AD Premium-tilaus, kuten mobiililaitteiden hallinnan automaattisen rekisteröinnin, jos tarvitset lisää ominaisuuksia.
- Mobiililaitteiden hallinta – esimerkiksi, Microsoft Intune-tilaus, mobiililaitteiden hallinta Office 365: n tai jonkin kumppanin mobiililaitteiden hallinnan toimittajat, jotka integroida Azure AD. (Katso lisätietoja tämän artikkelin alareunassa [kysymykset](#frequently-asked-questions) ).

Jos yhteyttä tilat hybrid, Microsoft suosittelee käyttöönottoa Azure AD Connect laajentaa Azure AD paikallisesta hakemistosta.

## <a name="requirements-for-using-domain-join-with-azure-ad"></a>Toimialueen käyttövaatimukset Azure AD osallistuminen

Toimialueen liity säilyy toimivat, jos se on aina. Kuitenkin Azure AD-edut tarvitset seuraavat:

- Azure AD-tilaus.
- Azure AD Connect laajentaa paikallisesta hakemistosta Azure AD-asennukseen.
- Käytännön, joka sallii toimialueeseen liittyneet laitteiden Azure AD ehdollinen käyttää.
- Käytännön, joka sallii käytön "toimialueeseen liittyneet" laitteet, jos haluat, että voit rajoittaa joitakin laitteita.
- System Center määritysten hallinta versio 1509 teknisen ennakkoversion käyttöön säännöt edellyttävän yhteensopivat laitteet. (Katso TechNet-asiakirjat ja blogimerkinnässä).

Saat lisätietoja toimialueen liity Windows 10: ssä, [Windows 10: lle Azure AD Connect toimialueeseen liittyneet laitteiden ilmenee](active-directory-azureadjoin-devices-group-policy.md)

## <a name="requirements-for-using-byod-and-add-a-work-or-school-account"></a>BYOD ja "Lisää työpaikan tai oppilaitoksen tiliä" käyttövaatimukset

Voit ottaa käyttöön "tuoda Omat laitteen" (BYOD) ja työpaikan tai oppilaitoksen tilit, tarvitset seuraavat:

- Azure AD-tilaus.
- Azure AD Premium-tilaus, kuten mobiililaitteiden hallinnan automaattisen rekisteröinnin, jos tarvitset lisää ominaisuuksia.

## <a name="requirements-for-using-microsoft-passport"></a>Microsoft Passport käyttövaatimukset

Jotta Microsoft Passport, sinun on seuraavasti:

- Julkisen avaimen infrastruktuurin (PKI), joka käyttää Microsoft Passport varmenteen tuki.
- Varmenteen tuki, joka käyttää Microsoft Passport Azure AD liittyä ja työpaikan tai oppilaitoksen tilit Intune-tilaus.
- System Center määritysten hallinta teknisen ennakkoversion 1509 numerotyyli (Katso TechNet-asiakirjat ja blogimerkinnässä), joka käyttää Microsoft Passport toimialueen liitoksen varmenteen tuki.
- Käytäntöä Microsoft Passport ottaminen käyttöön organisaatiossa.

Vaihtoehtona PKI: N avulla voit ottaa avaimen perustuva Microsoft Passport toimimalla seuraavasti:

- Ota käyttöön Windows Server 2016 "Tuotannon esikatselu 1" ohjauskoneet (toimialueen tai metsää toiminnalliset tasot ei tarvita; useita ohjauskoneita redundancy kokouksena kunkin Active Directory-sivuston riittää varten).
- Määrittää käytännön, Microsoft Passport käyttöön organisaatiossa.

Saat lisätietoja Microsoft Passport ja Windows Hei Windows 10: ssä < link-to-MS-Passport-and-Windows-Hello-document >.

## <a name="frequently-asked-questions"></a>Usein kysytyt kysymykset
### <a name="which-partner-mobile-device-management-products-integrate-with-azure-ad"></a>Kumppanin mobiililaitteiden hallinnan mitkä tuotteet integroida Azure AD?

Toimittajan seuraavia tuotteita integroida Azure AD yhdistetty rekisteröinti ja ehdollisen käyttöoikeuden Windows 10: ssä:

- AirWatch VMware mukaan
- Citrix Xenmobile
- Lightspeed Mobile hallinta
- SOTI paikallisen mobiililaitteiden hallinta
- MobileIron

### <a name="what-about-workplace-join-in-windows-10"></a>Entä työpaikkasi liittyä Windows 10: ssä?
Työpaikan liitos on käytetty Windows 8.1 BYOD. Windows 10: ssä BYOD on käytössä "Lisää on työpaikan tai oppilaitoksen tilillä" kuvatulla tavalla aiemmin tässä asiakirjassa. Organisaatioissa, joissa niiden mobiililaitteiden hallinta ei integroida Azure AD-käyttäjät voivat Rekisteröi laite siirtäminen manuaalisesti kautta **asetusten**hallinta > **tilit** > **työn access**.


### <a name="can-users-connect-their-microsoft-account-to-their-domain-account-in-windows-10"></a>Voit käyttäjät muodostavat yhteyden Microsoft-tilin toimialueen tilissä Windows 10: ssä?
Ei ole Windows 10: ssä. Windows 8.1 käyttäjät toimialueeseen liittyneet laitteiden onnistunut "muodostaa" Microsoft-tilillä (kuten Hotmail, Live, Outlook, Xbox, jne.) toimialueen tilissä käyttöön tiettyjä kokemukset, kuten Kertakirjautumista Live palveluihin tai Windows-kaupan käyttö ja roaming asetukset kaikissa laitteissa. Windows 10: "yhteyden" toimintoja Microsoft-tili on jätetty pois. Käyttäjän voit lisätä muita sähköpostitilejä, jotta Kertakirjautumista kuluttaja-palveluihin, kuten Windows-kaupan Microsoft-tilit. Tämä tapahtuu **asetukset** > **tilit** > **tilisi**.

Käyttäjät, jotka päivittävät Windows 8.1 toimialueeseen liittyneet laitteet ja jotka oli Microsoft-tilin yhteydessä, on yhdistetty Microsoft-tilin tilejä käyttävät luetteloon automaattisesti.


## <a name="additional-information"></a>Lisätietoja
* [Windows 10: n yrityksen: tapoja käyttää laitteet](active-directory-azureadjoin-windows10-devices-overview.md)
* [Cloud ominaisuuksia Windows 10-laitteisiin kautta Azure Active Directory-liittyä laajentaminen](active-directory-azureadjoin-user-upgrade.md)
* [Lisätietoja käyttötapoja Azure AD liittyminen](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Toimialueen liittyneet laitteiden yhdistäminen Azure AD for Windows 10: ssä kokemukset](active-directory-azureadjoin-devices-group-policy.md)
* [Määritä Azure AD liittyminen](active-directory-azureadjoin-setup.md)
