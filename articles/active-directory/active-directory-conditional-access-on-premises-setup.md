<properties
    pageTitle="Paikallisen ehdollinen käytön Azure Active Directory laitteen rekisteröinti määrittäminen | Microsoft Azure"
    description="Vaiheittaiset ohjeet käyttöön ehdollisen käyttämään paikallisen Active Directory Federation Service (AD FS) käyttäminen Windows Server 2012 R2."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>


# <a name="setting-up-on-premises-conditional-access-using-azure-active-directory-device-registration"></a>Paikallisen ehdollinen käytön Azure Active Directory laitteen rekisteröinti määrittäminen

Henkilökohtaisesti omistama laitteiden käyttäjät voidaan merkitä tiedossa organisaation edellyttäen työpaikalta liity muiden laitteiden Azure Active Directory laitteen rekisteröinti-palveluun. Alla on vaiheittaiset ohjeet käyttöön ehdollisen käyttämään paikallisen Active Directory Federation Service (AD FS) käyttäminen Windows Server 2012 R2.

> [AZURE.NOTE]
> Office 365-käyttöoikeus tai Azure AD Premium käyttöoikeuden vaaditaan, kun käytät laitetta rekisteröity Azure Active Directory laitteen rekisteröinti palvelun ehdollisen käyttöoikeuden käytännöt. Tämä sisältää käytännöt toimialuerekisteröintipalvelun resurssien paikallisen Active Directory Federation Services (AD FS).

Lisätietoja paikallisen ehdollisen käyttöoikeuden käyttötavoista on artikkelissa [työpaikan millä tahansa laitteella SSO ja saumaton toinen tekijä todennus yli yrityksen sovellusten liittäminen](https://technet.microsoft.com/library/dn280945.aspx).

Nämä ominaisuudet ovat käytettävissä asiakkaille, että ostaa Azure Active Directory-Premium-käyttöoikeus.

<a name="supported-devices"></a>Tuetut laitteet
-------------------------------------------------------------------------
* Windows 7: n toimialueen liitetyt laitteet.
* Windows 8.1 oma ja toimialueen liitetyt laitteet.
* iOS 6 tai uudempi versio, Safari-selaimessa
* Android 4.0 tai uudempi Samsung GS3 tai puhelimet, Samsung Huomautus 2 ylä- tai tabletit yläpuolella.


<a name="scenario-prerequisites"></a>Skenaario edellytykset
------------------------------------------------------------------------
* Office 365-tilaus tai Azure Active Directory-Premium
* Azure Active Directory-vuokraajan
* Windows Server Active Directory (Windows Server 2008: n tai uudemman)
* Windows Server 2012 R2 päivitettyä rakennetta
* Käyttöoikeuden määrittäminen Azure Active Directory-Premium
* Windows Server 2012 R2 Federation Services, SSO Azure AD määritetty
* Windows Server 2012 R2: n Web Application välityspalvelimen Microsoft Azure Active Directory-yhteyden (Azure AD Connect). [Lataa Azure AD Connect tähän](http://www.microsoft.com/en-us/download/details.aspx?id=47594).
* Vahvistama.

<a name="known-issues-in-this-release"></a>Tämän version tunnetut ongelmat
-------------------------------------------------------------------------------
* Laitteen mukaan ehdollisen käyttöoikeuden käytännöt edellyttävät laitteen objektin takaisinkirjoituksen Active Directory Azure Active Directorysta. Voi kestää jopa 3 tuntia laitteen objektit voidaan kirjoittaa takaisin muodostaminen Active Directoryyn
* iOS 7-laitteita aina kysyy käyttäjältä valittava varmenne Asiakasvarmenteen todennus aikana.
* IOS8, ennen kuin iOS 8.3 eivät toimi joissain-versioissa.

## <a name="scenario-assumptions"></a>Skenaario perustiedot
Tässä tilanteessa oletetaan, että Azure AD-vuokraajan ja paikallisen Active Directory yhdistelmäympäristön. Nämä alihallinnat pitäisi olla yhteydessä Azure AD Connect vahvistettu toimialue ja AD FS SSO varten. Alla tarkistusluettelon avulla voit määrittää ympäristösi edellä kuvatun vaiheen.

<a name="checklist-prerequisites-for-conditional-access-scenario"></a>Tarkistusluettelon: Edellytyksistä ehdollinen skenaario
--------------------------------------------------------------
Yhdistä Azure AD-vuokraajan paikallisen Active Directory.

## <a name="configure-azure-active-directory-device-registration-service"></a>Azure Active Directory-laitteen rekisteröinti kertakirjauspalvelun asetusten määrittäminen
Tämän oppaan avulla voit ottaa käyttöön ja määrittää organisaation Azure Active Directory laitteen rekisteröinti-palvelun.

Tässä oppaassa oletetaan, että olet määrittänyt Windows Server Active Directory ja tilannut Microsoft Azure Active Directory. Katso yllä edellytykset.

Ottamaan Azure Active Directory laitteen rekisteröinti-palvelun Azure Active Directory-vuokraajan seuraavan tarkistusluettelon järjestyksessä tehtävien suorittamiseen. Kun viite-linkeistä pääset käsitteellinen aiheen, palaa valmistelutyöt, kun olet tarkistanut yleisohje niin, että voit jatkaa valmistelutyöt jäljellä olevat tehtävät. Jotkin tehtävät sisällytetään skenaarion vahvistus vaihetta, jonka avulla voit vahvistaa vaihe on valmis.

## <a name="part-1-enable-azure-active-directory-device-registration"></a>Osa 1: Ota käyttöön Azure Active Directory laitteen rekisteröinti

Noudata seuraavia ohjeita ottaminen käyttöön ja määrittäminen Azure Active Directory laitteen rekisteröintipalvelua tarkistusluettelon.

| Tehtävä                                                                                                                                                                                                                                                                                                                                                                                             | Viittaus                                                       |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------|
| Ottaa käyttöön laitteen rekisteröinti Azure Active Directory-vuokraajan sallimaan liittymään työpaikan laitteet. Oletusarvon mukaan monimenetelmäisen todentamisen ei ole-palvelun käytössä. Monimenetelmäisen todentamisen kuitenkin suositellaan, kun laitteen rekisteröinti. Ennen kuin otat käyttöön multi-factor authentication-ADRS, varmista, että AD FS on määritetty monimenetelmäisen todentamisen palveluntarjoajasi. | [Azure Active Directory-laitteen rekisteröinti ottaminen käyttöön](active-directory-conditional-access-device-registration-overview.md)               |
| Laitteet löydä Azure Active laitteen rekisteröinti hakemistopalvelun hakemalla tunnetun DNS-tietueet. Sinun on määritettävä yrityksesi DNS, jotta laitteet löytävät Azure Active laitteen rekisteröinti hakemistopalvelun.                                                                                                                                                   | [Määritä Azure Active Directory laitteen rekisteröinti etsiminen.](active-directory-conditional-access-device-registration-overview.md) |

##<a name="part-2-deploy-and-configure-windows-server-2012-r2-active-directory-federation-services-and-set-up-a-federation-relationship-with-azure-ad"></a>Osa 2: Käyttöönotto ja määrittäminen Windows Server 2012 R2 Active Directory Federation Services ja Azure AD federation yhteyteen määrittäminen

| Tehtävä                                                                                                                                                                                                                                                                                                                                                                                             | Viittaus                                                       |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------|
| Ota käyttöön Active Directory-toimialueen palveluista toimialueen kanssa Windows Server 2012 R2 rakenteen tunnisteet. Sinun ei tarvitse tahansa toimialueen ohjaimet päivittämiseksi Windows Server 2012 R2. Rakenne-päivitys on vain taso. | [Päivitä Active Directory-toimialueen palvelut rakenteeseen](#upgrade-your-active-directory-domain-services-schema)               |
| Laitteet löydä Azure Active Directory laitteen rekisteröinti palvelusi hakemalla tunnetun DNS-tietueet. Sinun on määritettävä yrityksesi DNS, jotta laitteet löytävät Azure Active laitteen rekisteröinti hakemistopalvelun.                                                                                                                                                   | [Valmistele laitteissasi Active Directory-tuki](#prepare-your-active-directory-to-support-devices) |


##<a name="part-3-enable-device-writeback-in-azure-ad"></a>Osa 3: Ota käyttöön laitteen takaisinkirjoituksen Azure AD-

| Tehtävä                                                                                                                                                                                                                                                                                                                                                                                             | Viittaus                                                       |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------|
| Suorita ottaminen käyttöön laitteen takaisinkirjoituksen Azure AD Connect-osa 2. Valmistumisen Palauta tämän oppaan. | [Laitteen takaisinkirjoituksen Azure AD Connect-ottaminen käyttöön](#upgrade-your-active-directory-domain-services-schema)               |


##<a name="optional-part-4-enable-multi-factor-authentication"></a>[Valinnainen] Osa 4: Ota käyttöön monimenetelmäisen todentamisen

On suositeltavaa, että määrität jokin monimenetelmäisen todentamisen erilaisia vaihtoehtoja. Jos haluat vaatia MFA, on artikkelissa [multi-factor suojaus-ratkaisuun puolestasi](../multi-factor-authentication/multi-factor-authentication-get-started.md). Se sisältää kunkin ratkaisu ja linkkejä, joiden avulla voit määrittää valittua ratkaisun kuvaus.

## <a name="part-5-verification"></a>Osa 5: todentaminen

Käyttöönoton on nyt valmis. Voit nyt kokeilla joissakin tilanteissa. Kokeilla palvelua ja tutustu ominaisuudet alla olevista linkeistä


| Tehtävä                                                                                                                                                                                                                         | Viittaus                                                                       |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| Liity joitakin laitteita työpaikan Azure Active Directory laitteen rekisteröinti. Voit liittyä iOS-, Windows- ja Android-laitteille                                                                                         | [Laitteiden liittäminen työpaikan Azure Active Directory laitteen rekisteröinti](#join-devices-to-your-workplace-using-azure-active-directory-device-registration) |
| Voit tarkastella ja ottaa käyttöön/poistaa käytöstä rekisteröity laitteiden järjestelmänvalvoja-portaalissa. Tässä tehtävässä tarkastelet joitakin rekisteröity laitteita järjestelmänvalvoja-portaalissa.                                                        | [Azure Active Directory laitteen rekisteröinti yleiskatsaus](active-directory-conditional-access-device-registration-overview.md)                             |
| Varmista, että laitteen objektien kirjoitetaan takaisin Azure Active Directorysta Windows Server Active Directory.                                                                                                                  | [Tarkista rekisteröity laitteet on kirjoitettu Edellinen muodostaminen Active Directoryyn](#verify-registered-devices-are-written-back-to-active-directory)                  |
| Käyttäjille voi rekisteröidä laitteensa, voit luoda sovelluksen access käytännöt AD FS, joka mahdollistaa vain rekisteröity laitteita. Tässä tehtävässä luodaan sovelluksen-käyttösäännöt ja mukautetun käyttö estetty viesti | [Sovelluksen käyttöoikeuskäytäntö ja mukautetun käyttö estetty viestin luominen](#create-an-application-access-policy-and-custom-access-denied-message)            |



## <a name="integrate-azure-active-directory-with-on-premises-active-directory"></a>Integroi Azure Active Directory paikallisen Active Directory-hakemistosta
Tämä auttaa Azure AD-vuokraajan integroida paikallisen active directory-Azure AD Connect avulla. Vaikka vaiheet ovat käytettävissä Azure perinteinen-portaalissa, muistiin tässä osassa luetellut erityisiä ohjeita.

1.  Kirjaudu sisään käyttämällä tiliä, jolla on yleisen järjestelmänvalvojan Azure AD Azure perinteinen portaalin.
2.  Valitse vasemmanpuoleisessa ruudussa **Active Directorysta**.
3.  Valitse **kansio** -välilehdessä hakemistossa.
4.  Valitse **Yhteystietojen integrointi** -välilehti.
5.  **Käyttöönotto ja hallinta** -osassa vaiheiden 1 – 3, jos haluat liittää Azure Active Directory paikallisesta hakemistosta.
  1.    Lisää toimialueet.
  2.    Asentaminen ja suorittaminen Azure AD Connect: Asenna Azure AD Connect avulla ohjeita [Azure AD Connect mukautettu asennus](./connect/active-directory-aadconnect-get-started-custom.md).
  3. Tarkista ja directory-synkronoinnin hallinta. Sign-ohjeita on käytettävissä tämä vaihe.

  > [AZURE.NOTE]
  > Määritä Federation AD FS kuvatulla tavalla edellä linkitetyn asiakirjan. Sinun ei tarvitse esikatselu-ominaisuuksien määrittäminen.


## <a name="upgrade-your-active-directory-domain-services-schema"></a>Active Directory-toimialueen palveluista rakenteen päivittäminen

> [AZURE.NOTE]
> Active Directory-rakenteen päivittäminen ei voi kumota. On suositeltavaa, että ensin suorittaa tämän testiympäristössä.

1. Kirjaudu sisään toimialueen ohjauskoneen tilille, jolla on Enterprise-järjestelmänvalvoja ja rakenteen järjestelmänvalvojan oikeudet.
2. Kopioi **[media] \support\adprep** kansio ja alikansiot johonkin Active Directory-toimialueen ohjaimet.
3. Missä [media] Windows Server 2012 R2-asennustietoväline polku.
4. Komentoriviltä, siirry Adprepin kansioon ja suorita: **adprep.exe forestprepin**. Noudattamalla näytön ohjeita rakenteen asennusta.

## <a name="prepare-your-active-directory-to-support-devices"></a>Valmistele Active Directory tukevat laitteet

>[AZURE.NOTE] Tämä on erikseen toiminto, joka on suoritettava valmistelemisesta Active Directory-metsää tukevat laitteet. Sinun on oltava kirjautuneena yrityksen järjestelmänvalvojan oikeuksin ja Active Directory-metsää on oltava Windows Server 2012 R2-rakenteen jäsenyyttä.


##<a name="prepare-your-active-directory-forest-to-support-devices"></a>Valmistele Active Directory-metsää tukevat laitteet

> [AZURE.NOTE]
>Tämä on erikseen toiminto, joka on suoritettava valmistelemisesta Active Directory-metsää tukevat laitteet. Sinun on oltava kirjautuneena yrityksen järjestelmänvalvojan oikeuksin ja Active Directory-metsää on oltava Windows Server 2012 R2-rakenteen jäsenyyttä.

### <a name="prepare-your-active-directory-forest"></a>Active Directory-metsää valmisteleminen

1.  Liitetty viestintä-palvelimeen, Avaa Windows PowerShell-komentoikkunassa ja kirjoita: alusta ADDeviceRegistration
2.  Kun ohjelma pyytää **ServiceAccountName**, kirjoita olet valinnut palvelutili AD FS: lle palvelutilin nimi. Jos se on gMSA-tili, Anna tilin **domain\accountname$** -muodossa. Toimialueen tilin käyttää muodossa **domain\accountname**.



### <a name="enable-device-authentication-in-ad-fs"></a>AD FS laite-todennuksen käyttöön ottaminen

1. Liitetty viestintä-palvelimeen, Avaa AD FS-hallintakonsoli ja siirry **AD FS** > **Todennusta käytännöt**.
2. Valitse **Muokkaa yleinen ensisijainen käyttöoikeuden...** **Toiminnot** -ruudusta.
3. Tarkista **laite-todennuksen käyttöön ottaminen** ja valitse sitten**OK**.
4. Oletusarvon mukaan AD FS säännöllisesti poistaa käyttämättömiä laitteiden Active Directorysta. Tämän tehtävän on poistettava käytöstä, kun käytät Azure Active Directory laitteen rekisteröinti niin, että laitteet voidaan hallita Azure.


### <a name="disable-unused-device-cleanup"></a>Käyttämättömien laitteen uudelleenjärjestäminen poistaminen käytöstä
1. Liitetty viestintä-palvelimeen, Avaa Windows PowerShell-komentoikkunassa ja kirjoita: Set-AdfsDeviceRegistration - MaximumInactiveDays 0

### <a name="prepare-azure-ad-connect-for-device-writeback"></a>Azure AD Connect valmisteleminen laitteen takaisinkirjoituksen

1.  Mikä osa 1: Valmisteleminen Azure AD yhteyden.


## <a name="join-devices-to-your-workplace-using-azure-active-directory-device-registration"></a>Laitteiden liittäminen työpaikan Azure Active Directory laitteen rekisteröinti

### <a name="join-an-ios-device-using-azure-active-directory-device-registration"></a>Liittyminen käyttämällä Azure Active Directory laitteen rekisteröinti iOS-laitteessa

Azure Active Directory-laitteen rekisteröinti käyttää iOS-laitteita Over-the-Air profiilin laitteen käyttöönottoprosessin yhteydessä. Tämä toimenpide alkaa käyttäjän yhteyden profiilin rekisteröinti URL-osoitteen käyttäminen Safari-selaimessa. URL-muoto on seuraavanlainen:

    https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/"yourdomainname"

Jos `yourdomainname` on toimialuenimi, jonka olet määrittänyt Azure Active Directory-hakemistosta. Esimerkiksi jos toimialuenimi on contoso.com-URL-osoite on seuraava:

    https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/contoso.com

Monella eri viestintä tätä URL-Osoitetta käyttäjille. Yksi suositeltu tapa on julkaista tätä URL-Osoitetta mukautettujen sovellusten käyttö estetty AD FS-sanoma. Tämä koskee tulevista osan: [Luo sovelluksen käyttöoikeuskäytäntö ja mukautetun käyttö estetty-sanoman](#create-an-application-access-policy-and-custom-access-denied-message).

###<a name="join-a-windows-81-device-using-azure-active-directory-device-registration"></a>Liittyminen Azure Active Directory laitteen rekisteröinti-Windows 8.1-laitteella

1. Siirry Windows 8.1-laitteesi **Tietokoneen asetukset** > **verkon** > **työpaikkasi**.
2. Kirjoita käyttäjänimesi täydellisen Käyttäjätunnuksen muodossa. Esimerkiksi dan@contoso.com..
3. Valitse **Liity**.
4. Kun sinulta kysytään, kirjaudu sisään tunnistetiedot. Laite on nyt yhdistetty.

###<a name="join-a-windows-7-device-using-azure-active-directory-device-registration"></a>Liittyminen käyttämällä Azure Active Directory laitteen rekisteröinti Windows 7-laitteeseen
Voit rekisteröidä Windows 7: n toimialueen liitetty laitteita, sinun täytyy ottaa käyttöön laitteen rekisteröinti-ohjelmistopaketin. Ohjelmistopaketti kutsutaan työpaikkasi liittyä Windows 7 ja on ladattavissa [Microsoft Connect-sivustossa](https://connect.microsoft.com/site1164). Ohjeet paketin käyttämisestä on osoitteessa [Määritä automaattinen laitteen rekisteröinti Windows 7: n toimialueen liittyneet laitteet](active-directory-conditional-access-automatic-device-registration-windows7.md).

### <a name="join-an-android-device-using-azure-active-directory-device-registration"></a>Liittyminen käyttämällä Azure Active Directory laitteen rekisteröinti Android-laitteessa

[Azure tarkistuksessa Android aiheessa](active-directory-conditional-access-azure-authenticator-app.md) on ohjeet Azure todentaja-sovelluksen asentaminen Android-laitteen ja työpaikan tilin lisääminen. Työ-tili on luotu onnistuneesti Android-laitteessa, laite on yhdistetty organisaation työpaikkasi.

## <a name="verify-registered-devices-are-written-back-to-active-directory"></a>Tarkista rekisteröity laitteet on kirjoitettu Edellinen muodostaminen Active Directoryyn
Voit tarkastella ja varmista, että laitteen objektit on kirjoitettu takaisin muodostaminen Active Directoryyn LDP.exe tai ADSI Edit. Molemmat ovat käytettävissä Active Directory-järjestelmänvalvoja-työkaluja.

Oletusarvon mukaan laitteen objekteja, jotka on kirjoitettu Edellinen Azure Active Directorysta sijoitetaan AD FS-klusterin samaan toimialueeseen.

    CN=RegisteredDevices,defaultNamingContext

## <a name="create-an-application-access-policy-and-custom-access-denied-message"></a>Sovelluksen käyttöoikeuskäytäntö ja mukautetun käyttö estetty viestin luominen
Harkitse seuraava tilanne: Relying osapuolen luottamusta AD FS-sovelluksen luominen ja määrittäminen Liittoutumispalvelujen käyttöoikeuksien sääntö, jonka avulla vain rekisteröity laitteita. Nyt vain laitteita, joka on rekisteröity voivat käyttää sovellusta. Jotta helposti käyttäjiä käyttämään sovellusta, voit määrittää mukautetun estetty viesti, jossa on ohjeet niiden laitteen liittymään. Käyttäjien on nyt saumaton voi rekisteröidä laitteensa sovelluksen käyttämiseen.

Seuraavat vaiheet näkyy ottamisesta käyttöön tässä skenaariossa.

>[AZURE.NOTE]
Tässä osassa oletetaan, että olet jo määrittänyt että osapuolen luota AD FS-sovelluksen.

1. AD FS MMC-työkalun avaaminen ja AD FS Siirry > luota yhteydet > käyttäisit osapuolen luottaa.
2. Etsi sovellus, jonka säännöllä access käyttää. Napsauta sovellusta hiiren kakkospainikkeella ja valitse Muokkaa ryhmän sääntöjä...
3. Valitse **Liittoutumispalvelujen käyttöoikeuksien myöntämissääntöjä** -välilehti ja valitse sitten **Lisää sääntö...**
4. **Ryhmän säännön** mallin avattavasta luettelosta Valitse **Salli tai estä käyttäjiä perusteella saapuvan varaa**. Valitse **Seuraava**.
5. Varaa säännön nimi:-kenttään: **Salli Accessin rekisteröity laitteilla**
6. Kohteesta saapuvan postin väittää tyyppi: avattava luettelo, joka **on rekisteröity**käyttäjä.
7. Varaa saapuvat-arvo:-kenttään: **Tosi**
8. Valitse **Salli käyttäjille, joilla saapuvien vaatimus access** -valintanappi.
9. Valitse **Valmis** ja valitse sitten **Käytä**.
10. Poista säännöt, jotka on enemmän kuin juuri luomasi säännön makrosuojausasetusten. Poista esimerkiksi oletusarvon **Salli kaikkien käyttäjien pääsyn** sääntö.

Sovellus on nyt määritetty sallimaan käyttö vain silloin, kun käyttäjä on pian saatavilla laitteissa, joissa ne rekisteröity ja liitetty työpaikalla. Edistynyt käyttö käytäntöjä, lue [Hallinta riskin multi-factor käyttöoikeuksien hallinta](https://technet.microsoft.com/library/dn280949.aspx).

Seuraavaksi voit määrittää mukautetun virheilmoituksen sovelluksen. Virhesanoma avulla käyttäjät tietävät, että ne on liitettävä työpaikan niiden laite ennen kuin he voivat käyttää sovelluksen. Voit luoda mukautetun sovelluksen käyttöoikeutta viestin mukautettu HTML- ja Windows PowerShellin avulla.

Avaa Windows PowerShell-komentoikkunassa liittoutumispalvelimen, ja kirjoita seuraava komento. Korvaa kohteet, jotka ovat järjestelmän osat-komennon:

    Set-AdfsRelyingPartyWebContent -Name "relying party trust name" -ErrorPageAuthorizationErrorMessage
Sinun on rekisteröitävä laite, ennen kuin voit käyttää tätä sovellusta.

**Jos käytössäsi on iOS-laitteen valitsemalla laitteen liittymään tätä linkkiä**:

    a href='https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/yourdomain.com

Liity iOS-laite työpaikalle.


**Jos käytössäsi on Windows 8.1-laitteen**, voit liittyä laitteen **Tietokoneen asetukset **siirtymällä >  **verkon** > **työpaikkasi**.


Jossa "**käyttäisit osapuolen luota nimi**" on AD FS sovellusten käyttäisit osapuolen luota objektin nimi.
Jossa **yourdomain.com** on toimialuenimi, jonka olet määrittänyt Azure Active Directory-hakemistosta. Esimerkiksi contoso.com.
Muista poistaa kaikki rivinvaihdot (jos saatavilla) **Määrittäminen AdfsRelyingPartyWebContent** cmdlet-komento siirtää html-sisältöä.


Nyt kun käyttäjät käyttävät sovelluksen laitteella, joka ei ole rekisteröity-palvelussa Azure Active Directory laitteen rekisteröinti, he saavat sivun, joka näyttää seuraavankaltaiselta näyttökuvan alla.

![Virhe, kun käyttäjät on vielä rekisteröityjä niiden laitteen Azure AD Screeshot](./media/active-directory-conditional-access/error-azureDRS-device-not-registered.gif)

##<a name="related-articles"></a>Aiheeseen liittyviä artikkeleita

- [Artikkelissa indeksin Azure Active Directory-sovellusten hallinta](active-directory-apps-index.md)
