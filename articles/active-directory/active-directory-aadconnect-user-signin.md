<properties
    pageTitle="Azure AD Connect: Käyttäjän kirjautuminen | Microsoft Azure"
    description="Azure AD Connect käyttäjän kirjautunut sisään mukautettuja asetuksia."
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/16/2016"
    ms.author="billmath"/>



# <a name="azure-ad-connect-user-sign-on-options"></a>Azure AD yhteyden käyttäjän Kirjaudu käyttöönotto

Azure AD Connect sallii saman salasanojen cloud ja paikallisen resursseja kirjautumaan käyttäjille. Tässä artikkelissa kuvataan avaimen käsitteitä jokaisen identity-mallin avulla voit valita käyttäjätietoja haluat käyttää käyttäjän kirjautuminen Azure AD.

Jos tutuilla Azure AD tunnistetietojen mallin ja haluat lisätietoja tiettyä menetelmää, valitse alla tarvittavat aiheesta.

* [Salasanan synkronointi](#password-synchronization)
* [Liitetyt SSO (ja ADFS)](#federation-using-a-new-or-existing-ad-fs-in-windows-server-2012-r2-farm)


## <a name="choosing-a-user-sign-in-method"></a>Käyttäjän kirjautuminen menetelmän valitseminen
Varten useimmille organisaatioille, jotka haluat antaa käyttäjän kirjautua Office 365: een, SaaS sovellukset ja muut Azure AD-pohjaiset resurssit, suositellaan salasanan synkronointi oletusasetus.
Joissakin organisaatioissa on kuitenkin tietyn syyt käyttämällä esimerkiksi AD FS-asetus on liitetty allekirjoitus.  Näitä ovat:

- Organisaatiossa on AD FS tai 3rd, osapuolen federation tarjoajan otettu käyttöön
- Suojauskäytäntö estää synkronoidaan salasanan hajautusarvot pilveen
- Tarvitset käyttäjät kohdata saumattoman SSO (ilman muita salasana pyytää), kun cloud resurssien avaaminen toimialueen liitettyinä koneet yrityksen verkkoon
- Joitakin AD FS on tietyt ominaisuudet vaativat
    - Paikallisen monimenetelmäisen todentamisen kolmannen osapuolen palvelujen tai älykortit (Lisätietoja kolmannen osapuolen MFA tarjoajien, Windows Server 2012 R2 AD FS) käyttäminen
    - Active Directory asiakasintegraatio-ominaisuuksia esimerkiksi Pehmeä tilin lukitseminen tai AD salasana- ja työ tunnit-käytäntö
    - Ehdollisia käyttöoikeuksia sekä paikallisen ja cloud resurssien laitteen rekisteröinti, Azure AD-liity tai Intune-Mobiililaitteiden määrittäminen

### <a name="password-synchronization"></a>Salasanojen synkronoinnin
Salasanan synkronointi käyttäjän salasanojen hajautusarvot on synkronoitu paikallisen Active Directorysta Azure AD.  Kun salasanoja muutetaan tai palauta paikallinen, uudet salasanat synkronoituvat heti Azure AD niin, että käyttäjät voivat aina käyttää saman salasanan cloud resurssien tavalla kuin paikallisen.  Salasanat ovat koskaan lähettää Azure AD eikä tallennettu Azure AD pelkkänä tekstinä.
Salasanojen synkronoinnin voidaan ja salasanan takaisinkirjoituksen itse palvelun salasanan Azure AD-käyttöön.

<center>![Cloud](./media/active-directory-aadconnect-user-signin/passwordhash.png)</center>

[Lisätietoja salasanojen synkronoinnin](active-directory-aadconnectsync-implement-password-synchronization.md)


### <a name="federation-using-a-new-or-existing-ad-fs-in-windows-server-2012-r2-farm"></a>Uusi tai aiemmin luotu AD FS käyttäminen Windows Server 2012 R2-klusterin Federation
Kanssa liitetyssä etumerkin, että käyttäjät voivat kirjautua Azure AD-pohjaisten palvelujen paikallisen salasanansa ja yrityksen verkkoon, valitse tarvitsematta kirjoittaa salasanansa uudelleen.  AD FS federation-asetuksen avulla voit ottaa käyttöön uuden tai määrittää aiemmin AD FS-Liittoutumispalvelut Windows Server 2012 R2-klusterin.  Jos haluat määrittää aiemmin luodun klusterin Azure AD Connect määrittäminen niin, että käyttäjät voivat kirjautua asennuksen ja Azure AD välinen luottamussuhde.

<center>![Cloud](./media/active-directory-aadconnect-user-signin/federatedsignin.png)</center>

#### <a name="to-deploy-federation-with-ad-fs-in-windows-server-2012-r2-you-will-need-the-following"></a>Käyttöönotto Federation AD FS Windows Server 2012 R2, sinun on seuraavassa

Jos otat uuden klusterin:

- Windows Server 2012 R2-palvelin federation palvelimen.
- Windows Server 2012 R2-palvelin, Web-sovelluksen välityspalvelimen.
- .pfx tiedoston oman tarkoitettu federation-palvelunimi, kuten fs.contoso.com SSL-varmenteella.

Jos otat käyttöön uuden klusterin tai aiemmin luodun klusterin avulla:

- Paikallisen järjestelmänvalvojan tunnistetiedot liittäminen palvelimiin.
- Paikallisen järjestelmänvalvojan tunnistetietoja minkä tahansa työryhmän (muut kuin toimialueen liittyneet), jonka aiot ottaa Web-sovelluksen välityspalvelimen rooli palvelimiin.
- Tietokoneeseen, suorita ohjattu toiminto on voitava muodostaa yhteyden muodostaminen muihin koneet, johon haluat asentaa AD FS tai Web-sovelluksen välityspalvelimen kautta Windows etähallinnan.

[Kertakirjauksen määrittämisestä AD FS](./aad-connect/active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs) 

#### <a name="sign-on-using-an-earlier-version-of-ad-fs-or-a-third-party-solution"></a>Kirjaudu käyttämällä AD FS aiemmassa versiossa tai kolmannen osapuolen ratkaisu

Jos olet jo määrittänyt cloud Kirjaudu käyttämisestä AD FS-Liittoutumispalvelut (esimerkiksi AD FS 2.0) tai kolmannen osapuolen federation palveluntarjoaja aiemmassa versiossa, voit valita käyttäjän kirjautuminen määritysten Siirry Azure AD Connect kautta.  Tämän avulla voit Lataa uusin synkronointi ja muita ominaisuuksia Azure AD Connect samalla kun yhä käyttää aiemmin luotuja ratkaisu kirjauduttaessa käyttöön.

[Azure AD kolmannen osapuolen federation yhteensopivuuden luettelo](./active-directory-aadconnect-federation-compatibility.md)

## <a name="user-sign-in-and-user-principal-name-upn"></a>Käyttäjien sisäänkirjautumisessa ja käyttäjätunnus (UPN)

### <a name="understanding-user-principal-name"></a>Tietoja täydellinen käyttäjätunnus

Oletusarvon täydellisen Käyttäjätunnuksen jälkiliitteen on Active Directoryssa, jossa käyttäjätili on luotu toimialueen DNS-nimi. Useimmissa tapauksissa tämä on rekisteröity yrityksen toimialueen Internet-toimialuenimi. Voit lisätä useita UPN liitteet, käyttämällä Active Directory-toimialueet ja -luottamussuhteet.

Käyttäjän UPN on muodon username@domai. Esimerkiksi active Directoryn contoso.com käyttäjällä John saattaa olla UPN john@contoso.com. Käyttäjän UPN perustuu RFC 822. Täydellisen Käyttäjätunnuksen ja sähköpostin jakaa samassa muodossa, vaikka UPN arvo käyttäjän voi tai ei ehkä ole sama kuin käyttäjän sähköpostiosoite.

### <a name="user-principal-name-in-azure-ad"></a>Täydellinen käyttäjätunnus Azure AD-

Azure AD Connect ohjattu sisällytetään userPrincipalName-määritteen avulla tai avulla voit määrittää käytettävä-paikallisen täydellinen käyttäjätunnus Azure AD-määrite (kirjoita mukautettu asennus). Tämä on arvo, jota käytetään Azure AD sisäänkirjautumista. Jos Azure AD-käyttäjän käyttäjätunnuksen määritteen arvo vastaa vahvistettu toimialue, valitse Azure AD korvaa oletusarvoisen. onmicrosoft.com-arvo.

Jokaisen Azure Active Directory-hakemistossa sisältyy lomakkeen contoso.onmicrosoft.com, jolla voit valmiin verkkotunnus Azure tai muiden Microsoftin palvelujen käytön aloittamisessa. Voit parantaa ja yksinkertaistaa kirjautuminen mukautettujen toimialueiden. Lisätietoja mukautetun toimialueen Azure AD-nimet ja tarkistaminen toimialueen, lue [Lisää oman toimialuenimen Azure Active Directoryyn](active-directory-add-domain.md#add-your-custom-domain-name-to-azure-active-directory)

## <a name="azure-ad-sign-in-configuration"></a>Azure AD-määritys

### <a name="azure-ad-sign-in-configuration-with-azure-ad-connect"></a>Azure AD-kirjautuminen kokoonpanon Azure AD Connect
Azure AD-toiminto on riippuvainen onko Azure AD pysty vastaamaan käyttäjän käyttäjätunnuksen jälkiliite synkronoitavan johonkin mukautettujen toimialueiden vahvistettu Azure Active directory-käyttäjän. Azure AD Connect on Ohje-kirjautuminen Azure AD-asetusten määrittäminen niin, että käyttäjän kirjautuminen pilvipohjaisia on kuin mikä tahansa paikallisen. Azure AD Connect luettelot toimialueet % määritetty UPN liitteet ja yrittää vastaa ne mukautetun toimialueen Azure AD- ja auttaa tarvittavat toimenpiteet on otettava.
Azure AD-kirjautumissivulla määritetty paikallisen Active Directory UPN loppuliitteet, luettelot ja näyttää kunkin jälkiliite vastaan vastaavan tilan. Tila-arvoja voi olla jokin seuraavista alla:

| Tila | Kuvaus | Toimia ei tarvita |
|:----|:----|:----|
|Vahvistettu| Azure AD Connect löydy vastaavaa vahvistanut toimialueen Azure AD. Kaikki tämän toimialueen käyttäjät voivat kirjautua sisään käyttämällä paikallisen tunnistetietoja| Mitään toimia ei tarvita |
|Ei ole vahvistettu| Azure AD Connect löytänyt vastaavaa mukautetun toimialueen Azure AD, mutta se ei ole vahvistettu. Täydellisen Käyttäjätunnuksen jälkiliitteen toimialueessa käyttäjien muutetaan oletusasetukset. onmicrosoft.com jälkiliite Jos toimialue on vahvistettu ei synkronoinnin jälkeen. | Tarkista Azure AD mukautettua toimialuetta. [Opi lisää](./active-directory-add-domain.md#verify-the-domain-name-with-azure-ad) |  
|Ei lisätä| Azure AD Connect ei löydä vastaava täydellisen Käyttäjätunnuksen jälkiliitteen mukautettua toimialuetta. Täydellisen Käyttäjätunnuksen jälkiliitteen tämän toimialueen käyttäjät muutetaan oletusasetukset. Jos toimialuetta ei ole lisätty onmicrosoft.com- ja verifeid Azure-tietokannassa.| Lisää ja vastaavat täydellisen Käyttäjätunnuksen jälkiliitteen [Lisätietoja](./active-directory-add-domain.md) mukautetun toimialueen tarkistaminen|

Azure AD-sivulla näkyvät paikallisen Active Directory-ja vastaavan mukautetun toimialueen vahvistus tilassa Azure AD määritetty UPN suffix(s). Voit valita mukautetun asennuksen, nyt-määritteen täydellinen käyttäjätunnus **Azure AD - kirjautuminen** -sivulla.

![Azure AD-sivu](./media/active-directory-aadconnect-user-signin/custom_azure_sign_in.png)

Voit napsauttaa Päivitä-painike hakeaksesi mukautettujen toimialueiden uusimman tilan uudelleen Azure AD-painiketta.

###<a name="selecting-attribute-for-user-principal-name-in-azure-ad"></a>Valitsemalla täydellinen käyttäjätunnus Azure AD-määrite

UserPrincipalName - määritteen userPrincipalName on määrite käyttäjät käyttävät, kun ne kirjautuminen Azure AD- ja Office 365: ssä. Toimialueiden käytetään, kutsutaan myös UPN-liitteen, tarkistettava Azure AD ennen käyttäjät on synkronoitu. On suositeltavaa säilyttää oletusarvoisesti määrite userPrincipalName. Jos määritettä ei reititettävä ja ei voi vahvistaa, se on mahdollista valita toisen määritettä, esimerkiksi sähköpostin kuin määritettä tilalla on Kirjaudu sisään. Tätä kutsutaan vaihtoehtoinen tunnus. Vaihtoehtoinen tunnus-määritteen on seurattava RFC822-standardia. Vaihtoehtoinen tunnus voidaan käyttää salasanan Single Sign-On (SSO) ja federation SSO-kirjautuminen ratkaisuksi.

>[AZURE.NOTE] Vaihtoehtoinen tunnus käyttäminen ei ole yhteensopiva kaikkien Office 365-toiminnoista. Saat lisätietoja tutustumaan [Määrittäminen vaihtoehtoinen Kirjautumistunnus](https://technet.microsoft.com/library/dn659436.aspx).

#### <a name="different-custom-domain-states-and-effect-on-azure-sign-in-experience"></a>Toinen mukautettu toimialue: ssa tai vaikutus Azure kirjautuminen
On tärkeää ymmärtää mukautetun toimialueen tilat Azure Active Directoryn välisen suhteen ja UPN liitteet määritetty paikallisen. Kerrotaan uutiskirjeistä eri mahdollista Azure-kirjautuminen kokemukset, kun olet määrittämässä sycnhronization käyttämällä Azure AD Connnect.

Alla olevia tietoja uutiskirjeistä oletetaan, että olemme kyseisen täydellisen Käyttäjätunnuksen jälkiliite-contoso.com koon muuttamiseen tarkoitettu paikallisen hakemistossa osana UPN, kuten user@contoso.com.

###### <a name="express-settings--password-synchronization"></a>Express asetukset ja salasanan synkronointi
| Tila         | Vaikutus Azure kirjautuminen käyttäjäkokemus |
|:-------------:|:----------------------------------------|
| Ei lisätä | Tässä tapauksessa ei ole mukautettua toimialuettasi contoso.com on lisätty Azure AD-kansiossa. Käyttäjät, joilla on täydellisen Käyttäjätunnuksen jälkiliite kanssa paikallista @contoso.com, ei voi käyttää heidän paikallisen täydellisen Käyttäjätunnuksen kirjautuminen Azure. Ne sen sijaan on käytettävä uusi täydellisen Käyttäjätunnuksen toimittamien Azure AD kielletyt oletusarvon Azure Active Directory. Jos Azure Active directory azurecontoso.onmicrosoft.com sitten paikallisen käyttäjän käyttäjät ovat synkronoinnin esimerkiksi user@contoso.com kiinnitetään UPN.user@azurecontoso.onmicrosoft.com|
| Ei ole vahvistettu | Tässä tapauksessa on mukautettu toimialue-contoso.com lisätty Azure AD-kansioon, mutta se ei ole vielä vahvistanut. Jos siirryt suoraan synkronoinnin tarkistamatta toimialueen käyttäjät, valitse käyttäjät määritetään uusi täydellisen Käyttäjätunnuksen Azure AD vain samalta ei lisätä skenaarion mukaan.|
| Vahvistettu | Tässä tapauksessa on mukautetun toimialueen contoso.com jo lisännyt ja Azure Active Directory täydellisen Käyttäjätunnuksen jälkiliitteen vahvistettu. Käyttäjille voi käyttää heidän paikallisen täydellinen käyttäjätunnus, kuten user@contoso.com, sisään Azure, kun ne on synkronoitu Azure AD|

###### <a name="ad-fs-federation"></a>AD FS-Liittoutumispalvelinten
Et voi luoda oletusarvo federation. onmicrosoft.com-toimialuetta Azure AD- tai Azure AD-vahvistamaton mukautettua toimialuetta. Kun käytät Azure AD Connect ohjatun, jos valitset vahvistamaton toimialue, voit luoda ja sitten Azure AD Connect federation kehottaa sinua tarvittavat tietueet luodaan, jossa DNS: ää isännöidään toimialueen kanssa. Katso lisätietoja [tästä](active-directory-aadconnect-get-started-custom.md#verify-the-azure-ad-domain-selected-for-federation).

Jos valitsit käyttäjän kirjautuminen vaihtoehto "Federation AD FS", sinun on oltava mukautettua toimialuetta, voit jatkaa luomisesta federation Azure AD. Tutustu keskustelun siis on oltava Azure Active Directoryn lisätään mukautettu toimialue-contoso.com.

| Tila         | Vaikutus Azure kirjautuminen käyttäjäkokemus |
|:-------------:|:----------------------------------------|
| Ei lisätä | Tässä tapauksessa Azure AD Connect ei löydy vastaavaa mukautetun toimialueen täydellisen Käyttäjätunnuksen jälkiliite contoso.com Azure AD-kansiossa. Sinun on lisättävä mukautetun toimialueen contoso.com, jos tarvitset käyttäjiä kirjautumaan sisään AD FS käyttäminen niiden paikallisen täydellisen Käyttäjätunnuksen user@contoso.com.|
| Ei ole vahvistettu | Tässä tapauksessa Azure AD Connect kehottaa sinua, miten voit tarkistaa toimialueen myöhemmässä vaiheessa tarvittavat tiedot|
| Vahvistettu | Tässä tapauksessa voit siirtyä suoraan ilman jatkotoimia määrityksissä|  

## <a name="changing-user-sign-in-method"></a>Muuttaminen käyttäjän-menetelmällä

Voit muuttaa käyttäjän kirjautuminen menetelmä Federation salasanan synkronointi tehtävät-avaialble käyttäminen Azure AD Connect Azure AD Connect käyttämällä ohjattua ensimmäisen määrityksen jälkeen. Suorita ohjattu Azure AD Connect uudelleen ja voit valita jommankumman ja tehtäviä, joita voit suorittaa luettelo. Valitse **Muuta käyttäjien-sisäänkirjautumisessa** tehtäväluettelosta. 

![Muuta käyttäjien sisäänkirjautumisessa"](./media/active-directory-aadconnect-user-signin/changeusersignin.png)

Valitse seuraavalla sivulla sinua pyydetään Azure AD tunnistetietoja.

![Azure AD yhdistäminen](./media/active-directory-aadconnect-user-signin/changeusersignin2.png)

Valitse **käyttäjien sisäänkirjautumisessa** -sivulla **Salasanojen synkronoinnin**. Tämä muuttaa kansio, liitetty hallitun yksi.

![Azure AD yhdistäminen](./media/active-directory-aadconnect-user-signin/changeusersignin3.png)

>[AZURE.NOTE] Jos teet tilapäinen valitsin salasanan synkronointi, valitse **Älä muunna käyttäjätilit**. Lukemassa asetus johtaa muuntaminen kunkin käyttäjän liitetty ja se voi kestää useita tunteja.
  
## <a name="next-steps"></a>Seuraavat vaiheet
Lisätietoja [ympäristön integroinnissa paikallisen-käyttäjätietojen Azure Active Directory-hakemistosta](active-directory-aadconnect.md).

Lisätietoja [Azure AD Connect: suunnitella käsitteitä](active-directory-aadconnect-design-concepts.md)


