<properties
    pageTitle="Azure AD Connect: Mukautettu asennus | Microsoft Azure"
    description="Tämän asiakirjan tiedot Azure AD Connect mukautetut asennusvaihtoehdot. Näiden ohjeiden avulla voit asentaa Azure AD Connect Active Directoryyn."
    services="active-directory"
    keywords="Mikä on Azure AD Connect, asenna Active Directory-Azure AD tarvittavat komponentit"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"  
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/13/2016"
    ms.author="billmath"/>

# <a name="custom-installation-of-azure-ad-connect"></a>Mukautetun asennuksen Azure AD Connect
Azure AD Connect **Mukautetut asetukset** käytetään, kun haluat asennuksen Lisäasetukset. Sitä käytetään, jos sinulla on useita metsien tai jos haluat määrittää valinnaisia ominaisuuksia, jotka eivät koske pika-asennus. Sitä käytetään kaikissa tapauksissa, kun [**Pika-asennus**](active-directory-aadconnect-get-started-express.md) -vaihtoehto ei täytä käyttöönoton tai topologian.

Ennen kuin aloitat asennuksen Azure AD Connect, varmista, että voit [ladata Azure AD Connect](http://go.microsoft.com/fwlink/?LinkId=615771) ja valmis ennen tarvittavat ohjeet ovat kohdassa [Azure AD Connect: laitteisto ja edellytykset](../active-directory-aadconnect-prerequisites.md). Varmista myös on pakollinen tilit, jotka ovat käytettävissä kuvatulla tavalla [Azure AD Connect asiakkaat ja käyttöoikeudet](active-directory-aadconnect-accounts-permissions.md).

Jos mukautettuja asetuksia ei vastaa verkkotopologia, esimerkiksi Dirsync-työkalun päivittäminen on artikkelissa muissa tilanteissa [liittyvät asiakirjat](#related-documentation) .

## <a name="custom-settings-installation-of-azure-ad-connect"></a>Azure AD Connect asennuksen mukautetut asetukset

### <a name="express-settings"></a>Express asetukset
Tällä sivulla valitsemalla **Mukauta** Käynnistä mukautetut asetukset-asennus.

### <a name="install-required-components"></a>Asenna tarvittavat osat
Kun asennat synkronointipalvelut, voit jättää valinnainen Tietolähdemääritykset-osasta valitsematta ja Azure AD Connect määrittää kaikki automaattisesti. Se määrittää SQL Server 2012 Express LocalDB esiintymän tarvittavat ryhmien luominen ja käyttöoikeuksien. Jos haluat muuttaa oletusasetuksia, voit ymmärtää valinnainen määritysvaihtoehtoja, joita ovat käytettävissä seuraavassa taulukossa.

![Tarvittavat komponentit](./media/active-directory-aadconnect-get-started-custom/requiredcomponents.png)

Valinnainen määritys  | Kuvaus
------------- | -------------
Olemassa olevan SQL-palvelimen käyttäminen | Voit määrittää SQL-palvelimen nimi ja esiintymänimi. Valitse tämä vaihtoehto, jos sinulla on jo tietokannan palvelin, jossa haluat käyttää. Anna seuraa pilkku ja portin numeroa- **Esiintymänimi** , jos SQL Server ei ole käytössä selaaminen esiintymän nimi.
Käytä aiemmin luotua service-tiliä | Azure AD Connect luo oletusarvoisesti paikallisen palvelutilin synkronointi-palveluiden käyttäminen. Salasana on luotu automaattisesti ja Tuntematon asentaminen Azure AD Connect henkilölle. Jos SQL-etäpalvelimeen tai käytät välityspalvelin, joka edellyttää käyttöoikeuden tarkistusta, sinun on palvelun tili toimialueen ja tiedät salasanan. Kirjoita tapauksessa palvelutili, jota käytetään. Varmista, että käyttäjän asennus on SQL SA, joten kirjautuminen palvelutilin voidaan luoda. Katso [Azure AD Connect asiakkaat ja käyttöoikeudet](active-directory-aadconnect-accounts-permissions.md#custom-settings-installation)
Määrittää mukautetun synkronointi | Oletusarvoisesti Azure AD Connect Luo neljä ryhmiä paikalliseen palvelimeen kun synkronointipalvelut on asennettu. Nämä ryhmät ovat: Järjestelmänvalvojat-ryhmään, operaattorit-ryhmän, Selaa-ryhmä ja salasanan palauttaminen-ryhmä. Voit määrittää Omat ryhmät. Ryhmät on oltava paikallinen palvelimessa, joten sitä ei löydy toimialueen.

### <a name="user-sign-in"></a>Käyttäjien sisäänkirjautumisessa
Asennettuasi tarvittavat osat, sinua pyydetään valitsemaan käyttäjät yksi Sign-menetelmä. Seuraavassa taulukossa on lyhyt kuvaus käytettävissä olevista vaihtoehdoista. Kirjaudu sisään menetelmistä koko kuvaus on kohdassa [käyttäjien sisäänkirjautumisessa](../active-directory-aadconnect-user-signin.md).

![Käyttäjä](./media/active-directory-aadconnect-get-started-custom/usersignin.png)

Merkki-vaihtoehto | Kuvaus
------------- | -------------
Salasanan synkronointi | Käyttäjät voivat kirjautua sisään Microsoftin pilvipalveluihin, kuten Office 365: ssä, niiden paikalliseen verkkoon kirjauduttaessa saman salasanalla. Käyttäjien salasanat synkronoidaan Azure AD kuin salasana hash ja todennusta pilveen. Lisätietoja [salasanojen synkronoinnin](../active-directory-aadconnectsync-implement-password-synchronization.md) .
AD FS liitetty viestintä | Käyttäjät voivat kirjautua sisään Microsoftin pilvipalveluihin, kuten Office 365: ssä, niiden paikalliseen verkkoon kirjauduttaessa saman salasanalla.  Käyttäjät on ohjattu paikalliseen niiden AD FS esiintymän kirjautua sisään ja paikallisen todennusta.
Ei määritetä | Kumpikaan ominaisuus asennetaan ja määritetään. Valitse tämä vaihtoehto, jos sinulla on jo 3 osapuolen liittoutumispalvelimen tai aiemmin ratkaisu on lisätty.

### <a name="connect-to-azure-ad"></a>Azure AD yhdistäminen
Anna Muodosta yhteys näytön Azure AD-yleisen järjestelmänvalvojan tililtä ja salasana. Jos valitsit **Federation AD FS** edellisen sivun, ei kirjautua sisään tilille toimialueen aiot federation käyttöön. Suositus on tilin käyttäminen oletusarvoinen **onmicrosoft.com** -toimialue, joka sisältää Azure AD-kansio.

Tämä tili käytetään vain palvelutili luominen Azure AD ja ei ole käytössä, kun ohjattu toiminto on valmis.  
![Käyttäjä](./media/active-directory-aadconnect-get-started-custom/connectaad.png)

Jos yleisen järjestelmänvalvojan tilillä on käytössä MFA, sinun on annettava salasana uudelleen-kirjautuminen-valikko ja viimeistele MFA-todennus. Haasteellista voi olla antamisen vahvistus-koodia tai puhelun.  
![MFA käyttäjän kirjautuminen](./media/active-directory-aadconnect-get-started-custom/connectaadmfa.png)

Yleisen järjestelmänvalvojan tililtä voi olla myös [Sellaisten jäsenyyksien hallinta](../active-directory-privileged-identity-management-getting-started.md) käytössä.

Jos virhesanoma ja yhteyden ilmenee ongelmia, katso kohta [yhteysongelmien vianmääritys](../active-directory-aadconnect-troubleshoot-connectivity.md).

## <a name="pages-under-the-section-sync"></a>Valitse synkronointi-osan sivut

### <a name="connect-your-directories"></a>Muodostaa yhteyttä hakemistojen selaaminen
Muodosta yhteys palvelussa Active Directory-toimialueen Azure AD Connect on riittävä käyttöoikeuksin varustetun tilin tunnistetiedot. Voit kirjoittaa toimialueen osa NetBios tai FQDN muodossa, eli FABRIKAM\syncuser tai fabrikam.com\syncuser. Tämän tilin voi olla tavallisen käyttäjätilin, koska sen vain luku oletuskäyttöoikeudet. Kuitenkin että toimintamallin joudut ehkä Lisää käyttöoikeuksia. Lisätietoja on artikkelissa [Azure AD yhteyden asiakkaat ja käyttöoikeudet](../active-directory-aadconnect-accounts-permissions.md#create-the-ad-ds-account)

![Yhdistä hakemisto](./media/active-directory-aadconnect-get-started-custom/connectdir.png)

### <a name="azure-ad-sign-in-configuration"></a>Azure AD-määritys
Tällä sivulla voit tarkastella esitä paikallisen täydellisen Käyttäjätunnuksen toimialueet AD DS ja joka on tarkistettu Azure AD. Tämän sivun avulla voit määrittää käytettävät userPrincipalName määrite.

![Vahvistamaton toimialueet](./media/active-directory-aadconnect-get-started-custom/aadsigninconfig.png)  
Tarkista jokaisen toimialueen, joka on merkitty **Ei lisätä** , ja **Ei ole vahvistettu**. Varmista, että näistä toimialueista, käytössäsi on tarkistettu Azure AD. Kun olet tarkistanut toimialueen, valitse Päivitä-symboli. Lisätietoja on kohdassa [Lisää ja tarkista toimialue](../active-directory-add-domain.md)

**UserPrincipalName** - määritteen userPrincipalName on määrite he voivat käyttää, kun ne kirjautuminen Azure AD- ja Office 365: ssä. Toimialueiden käytetään, kutsutaan myös UPN-liitteen, tarkistettava Azure AD ennen käyttäjät on synkronoitu. Microsoft suosittelee pitämään oletusarvo-määritteen userPrincipalName. Jos määrite ei reititettävä ei voi vahvistaa, on mahdollista valita toisen määrite. Voit esimerkiksi valita sähköpostin määrite tilalla on kirjautumisnimi. **Vaihtoehtoinen tunnus**kutsutaan toiseen kuin userPrincipalName-määritteen avulla. Vaihtoehtoinen tunnus-määritteen on seurattava RFC822-standardia. Vaihtoehtoinen tunnus voidaan käyttää salasanan synkronointi ja federation.

>[AZURE.WARNING]
Vaihtoehtoinen tunnus käyttäminen ei ole yhteensopiva kaikkien Office 365-toiminnoista. Saat lisätietoja viitata [Määrittäminen vaihtoehtoinen Kirjautumistunnus](https://technet.microsoft.com/library/dn659436.aspx).

### <a name="domain-and-ou-filtering"></a>Toimialueen ja OU suodatus
Oletusarvon mukaan kaikki toimialueiden ja organisaatioyksiköiden synkronoidaan. Jos luettelossa on joitakin toimialueita tai et halua synkronoida Azure AD organisaatioyksiköiden, voit poistaa nämä toimialueiden ja organisaatioyksiköiden valinnan.  
![DomainOU suodattaminen](./media/active-directory-aadconnect-get-started-custom/domainoufiltering.png) tämän ohjatun toiminnon sivulla on määrittäminen toimialueelta suodatus. Lisätietoja on artikkelissa [toimialueen suodatus](../active-directory-aadconnectsync-configure-filtering.md#domain-based-filtering).

On myös mahdollista, että jotkin toimialueita ei voi käyttää palomuurin rajoitusten vuoksi. Nämä toimialueet ovat oletusarvoisesti ei valittu, ja näyttöön tulee varoitus.  
![Saavuttamattomissa toimialueet](./media/active-directory-aadconnect-get-started-custom/unreachable.png)  
Jos näyttöön tulee varoitus, varmista, että nämä toimialueiden ovat varmasti saavuttamattomissa varoitus on oikein.

### <a name="uniquely-identifying-your-users"></a>Yksilöivä käyttäjille
Matching yli metsien ominaisuuden avulla voit määrittää, kuinka käyttäjät AD DS: ÄÄN saatavista esitetään Azure AD. Käyttäjä voi joko edustaa vain kerran kaikki puuryhmissä tai on käytössä ja poissa käytöstä tilien yhdistelmä. Käyttäjä voi esittää myös joitakin metsien yhteyshenkilöksi.

![Yksilöivä](./media/active-directory-aadconnect-get-started-custom/unique.png)

Asetus | Kuvaus
------------- | -------------
[Käyttäjien esitetään vain kerran kaikki puuryhmissä](../active-directory-aadconnect-topologies.md#multiple-forests-separate-topologies) | Kaikki käyttäjät luodaan Azure AD yksittäisiä objekteina. Objekteja ei ole liitetty metaverse.
[Posti-määrite](../active-directory-aadconnect-topologies.md#multiple-forests-full-mesh-with-optional-galsync) | Tämä vaihtoehto yhdistää käyttäjät ja yhteystiedot, jos sähköposti-määrite on sama arvo eri toimialuepuuryhmiin. Käytä tätä vaihtoehtoa, kun yhteystietosi on luotu käyttämällä GALSync.
[ObjectSID ja msExchangeMasterAccountSID / msRTCSIP OriginatorSid](../active-directory-aadconnect-topologies.md#multiple-forests-account-resource-forest) | Tämä vaihtoehto yhdistää käytössä käyttäjän tilin metsää resurssin metsää ei käytössä-käyttäjän kanssa. Exchange-määritysten kutsutaan linkitetyn postilaatikkoon. Tämä vaihtoehto voidaan myös, jos käytät vain Lync ja Exchange ei ole resurssin metsää.
sAMAccountName että MailNickName | Tämä vaihtoehto yhdistää määritteissä, jossa on odotettavissa käyttäjän kirjautuminen tunnus löytyy.
Tietyn määritteen. | Tämän asetuksen avulla voit valita oman määrite. **Rajoitus:** Varmista, että valitse määrite, joka on jo löytyy metaverse. Jos valitset Mukautettu määrite (eivät sisälly metaverse), ohjattu toiminto ei voi suorittaa loppuun.

**Tietolähteen ankkuri** - määritteen sourceAnchor on määrite, joka on muuttumaton user-objekti elinkaaren aikana. Perusavaimen linkittämisestä Azure AD-ympäristöön-käyttäjä, jolla käyttäjä on. Koska määrite ei voi muuttaa, täytyy suunnitella hyvä määritteen käyttämään. Hyvällä perusavaimella on objectGUID. Tämän määritteen ei muutu, paitsi jos käyttäjätili on siirretty metsien/toimialueiden välillä. Usean metsää ympäristössä, johon siirrät tilit puuryhmien toinen määrite on käytettävä, kuten määrite, jolla työntekijöillä. Vältä määritteitä, jotka muuttaa, kun henkilö marries tai osalta. Et voi käyttää määritteitä @-sign, niin sähköpostin ja userPrincipalName ei voi käyttää. Määrite on myös kirjainkoko on merkitsevä niin siirrät puuryhmien objektin, varmista, että Säilytä isot ja pienet kirjaimet. Binaarinen määritteet ovat base64-koodattu, mutta määrite muuntyyppisten säilyvät koodaamattomana tilasta. Federation skenaariot ja jotkin Azure AD-liittymät tämän määritteen käytetään myös nimitystä immutableID. Lisätietoja tietolähteen ankkurin löytyvät [suunnittelu](../active-directory-aadconnect-design-concepts.md#sourceAnchor).

### <a name="sync-filtering-based-on-groups"></a>Synkronoi suodatuksen perusteella ryhmät
Suodattaminen ryhmät-toiminnon avulla voit synkronoida vain pienen osan objektien koekäytön varten. Jos haluat käyttää tätä toimintoa, Luo ryhmä tähän tarkoitukseen paikallisen Active Directoryssa. Lisää käyttäjät ja ryhmät, jotka synkronoidaan sitten Azure AD suoraan jäseniksi. Voit lisätä ja poistaa käyttäjiä ryhmään objekteja, jotka esitetään Azure AD ylläpitämiseen myöhemmin. Kaikki objektit, jotka haluat synkronoida on oltava suora ryhmän jäsen. Käyttäjät, ryhmät, yhteystiedot ja tietokoneiden ja laitteiden oltava kaikki suora jäseniä. Sisäkkäisen Ryhmäjäsenyyden ei ole ratkennut. Kun lisäät ryhmän jäsen vain ryhmän lisätylle ja ei sen jäseniin.

![Synkronoi suodattaminen](./media/active-directory-aadconnect-get-started-custom/filter2.png)

>[AZURE.WARNING]
Tämä ominaisuus on tarkoitettu vain tukemaan Pilottivaiheen käyttöönotto. Älä käytä sen full-blown tuotannon käyttöönotto.

Valitse full-blown tuotantoympäristössä sen suorittaminen on vaikea säilyttää yhden ryhmän Synkronoi kaikkien objektien kanssa. Sen sijaan käytettävä tavoilla [määrittäminen](../active-directory-aadconnectsync-configure-filtering.md)suodattamisessa.

### <a name="optional-features"></a>Valinnaisia ominaisuuksia
Tässä näytössä voit valita tietyissä skenaarioissa valinnaisia ominaisuuksia.

![Valinnaisia ominaisuuksia](./media/active-directory-aadconnect-get-started-custom/optional.png)

>[AZURE.WARNING]
Jos sinulla on tällä hetkellä DirSync tai Azure AD-synkronointi aktiivinen, Aktivoi Azure AD Connect takaisinkirjoituksen-ominaisuuksien.

Valinnaisia ominaisuuksia | Kuvaus
------------------- | -------------
Exchange-Yhdistelmäratkaisu | Exchange-Yhdistelmäratkaisu-ominaisuuden avulla rinnakkainen Exchange-postilaatikoiden sekä paikallisen ja Office 365: ssä. Azure AD Connect synkronoidaan tietyt Azure AD- [määritteitä](../active-directory-aadconnectsync-attributes-synchronized.md#exchange-hybrid-writeback) takaisin paikallisesta hakemistosta.
Azure AD-sovellus ja määritteen suodatus | Ottamalla Azure AD-sovellus ja määritteen suodatus on räätälöityjä synkronoidun määritteet määrittäminen. Tämä vaihtoehto lisää ohjattuun kaksi määritys-sivuja. Lisätietoja on artikkelissa [Azure AD-sovellus ja määritteen suodatus](#azure-ad-app-and-attribute-filtering).
Salasanojen synkronoinnin | Jos olet valinnut Kirjaudu sisään-ratkaisun federaatio, voit ottaa tämän asetuksen. Salasanojen synkronoinnin voidaan varmuuskopio-asetus. Lisätietoja on artikkelissa [salasanojen synkronoinnin](../active-directory-aadconnectsync-implement-password-synchronization.md).
Salasanan takaisinkirjoituksen | Ottamalla salasanan takaisinkirjoituksen salasanan muutokset, jotka ovat peräisin Azure AD kirjoitetaan takaisin paikallisesta hakemistosta. Lisätietoja [salasanojen hallinta käytön aloittaminen](../active-directory-passwords-getting-started.md).
Ryhmän takaisinkirjoituksen | Jos käytät **Office 365-ryhmiä** -ominaisuutta, voit määrittää paikallisen Active Directoryssa edustaa ryhmiin. Tämä asetus on käytettävissä vain, jos sinulla on Exchange esitä paikallisen Active Directoryssa. Lisätietoja on artikkelissa [ryhmän takaisinkirjoituksen](../active-directory-aadconnect-feature-preview.md#group-writeback).
Laitteen takaisinkirjoituksen | Mahdollistaa takaisinkirjoituksen laitteen objektien ehdollisen käyttöoikeuden skenaarioissa Azure AD paikallisen Active Directory. Lisätietoja on artikkelissa [ottaminen käyttöön laitteen takaisinkirjoituksen: Azure AD Connect](../active-directory-aadconnect-feature-device-writeback.md).
Tunniste-määritteen hakemistosynkronointia | Ottamalla tunnisteet määrite hakemistosynkronointia määritteen on synkronoitu Azure AD. Lisätietoja on artikkelissa [Directory tunnisteet](../active-directory-aadconnectsync-feature-directory-extensions.md).

### <a name="azure-ad-app-and-attribute-filtering"></a>Azure AD-sovellus ja määritteen suodatus
Jos haluat rajoittaa mitkä attribuutit synkronoidaan Azure AD-Käynnistä valitsemalla mitä palveluja käytät. Jos teet määritysmuutoksia tällä sivulla, uusi palvelu on valittava eksplisiittisesti suorittamalla ohjattu asennus.

![Valinnaisia ominaisuuksia sovellukset](./media/active-directory-aadconnect-get-started-custom/azureadapps2.png)

Perusteella edellisessä vaiheessa valitut palvelut-sivulla näkyvät kaikki määritteitä, jotka on synkronoitu. Tämä luettelo on synkronoitu kaikki objektityypit yhdistelmä. Jos luettelossa on joitakin erityisesti määritteitä, jotka haluat synkronoida, voit poistaa valinnan nämä määritteet.

![Valinnaisia ominaisuuksia määritteet](./media/active-directory-aadconnect-get-started-custom/azureadattributes2.png)

>[AZURE.WARNING]
Määritteet poistaminen saattaa olla vaikutusta toimintoja. Saat parhaat käytännöt ja suositukset, [Attribuutit synkronoidaan](../active-directory-aadconnectsync-attributes-synchronized.md#attributes-to-synchronize).

### <a name="directory-extension-attribute-sync"></a>Tunniste-määritteen hakemistosynkronointia
Voit laajentaa rakenteen Azure AD-organisaation tai määritteet Active Directoryssa lisäämät mukautetut määritteet kanssa. Voit käyttää tätä toimintoa, valitse **kansio-tunniste määrite Synkronoi** **Valinnaisia ominaisuuksia** -sivulla. Voit valita useita määritteitä synkronoimaan tällä sivulla.

![Hakemisto-laajennukset](./media/active-directory-aadconnect-get-started-custom/extension2.png)

Lisätietoja on artikkelissa [Directory tunnisteet](../active-directory-aadconnectsync-feature-directory-extensions.md).

## <a name="configuring-federation-with-ad-fs"></a>AD FS federation määrittäminen
AD FS määrittäminen Azure AD Connect on yksinkertainen vain muutamalla napsautuksella. Seuraavassa on pakollinen ennen määritystä.

- Windows Server 2012 R2-palvelimen federation palvelimen kanssa käytössä etähallinta
- Windows Server 2012 R2-palvelimen Web Application välityspalvelimen käytössä etähallinnan kanssa
- SSL-varmenteen federation palvelun nimi, jota aiot käyttää (kuten sts.contoso.com)

### <a name="ad-fs-configuration-pre-requisites"></a>AD FS määritys-vaatimukset
Määrittäminen käyttämällä Azure AD Connect AD FS klusterin, varmista WinRM on käytössä remote-palvelimiin. Lisäksi käymällä läpi portit vaatimus [taulukon](../active-directory-aadconnect-ports.md#table-3---azure-ad-connect-and-federation-serverswap)3 - Azure AD Connect ja Federation palvelinten/WAP luettelossa.

### <a name="create-a-new-ad-fs-farm-or-use-an-existing-ad-fs-farm"></a>Luo uusi AD FS-klusterin tai Käytä aiemmin luotuun AD FS-klusteriin
Voit käyttää aiemmin luotuun AD FS-klusteriin tai voit luoda uuden AD FS-klusterin. Jos haluat luoda uuden tarvitaan antamaan SSL-varmenne. Jos SSL-varmenne on suojattu salasanalla, sinua kehotetaan salasana.

![AD FS-klusterin](./media/active-directory-aadconnect-get-started-custom/adfs1.png)

Jos haluat käyttää aiemmin luotuun AD FS-klusteriin on otettava siirtyä määrittäminen AD FS ja Azure AD-näytön välinen luottamussuhde.

### <a name="specify-the-ad-fs-servers"></a>Määritä AD FS-palvelimiin
Kirjoita, johon haluat asentaa AD FS palvelimiin. Voit lisätä yhden tai usean palvelimen yhteyttä kapasiteetin suunnittelu tarpeiden perusteella. Liity palvelimilta Active Directory, ennen kuin voit tehdä tämän määrityksen. Microsoft suosittelee yksittäisen AD FS-palvelimen numero- ja kokeilla-versioiden asentaminen. Valitse Lisää ja ota käyttöön palvelimien omien skaalauksen tarpeiden suorittamalla Azure AD Connect alkuperäinen määritysten jälkeen uudelleen.

>[AZURE.NOTE]
Varmista, että jokaisessa palvelimessa liitettyinä AD-toimialue, ennen kuin teet tämän määrityksen.

![AD FS-palvelimet](./media/active-directory-aadconnect-get-started-custom/adfs2.png)

### <a name="specify-the-web-application-proxy-servers"></a>Määrittää WWW-sovelluksen välityspalvelinten
Kirjoita palvelimissa, jotka tarvitset verkkosovelluksen välityspalvelimet. Web-sovelluksen välityspalvelimen on otettu käyttöön oman DMZ (ekstranet yhteydessä) ja tukee todennus ekstranet pyynnöt. Voit lisätä yhden tai usean palvelimen yhteyttä kapasiteetin suunnittelu tarpeiden perusteella. Microsoft suosittelee yhden sovelluksen Web-välityspalvelin numero- ja kokeilla-versioiden asentaminen. Valitse Lisää ja ota käyttöön palvelimien tarpeittesi skaalauksen suorittamalla Azure AD Connect alkuperäinen määritysten jälkeen uudelleen. On suositeltavaa ottaa vastaava määrä välityspalvelimen todennus intranetistä tarpeiden tyydyttämiseksi.

>[AZURE.NOTE]
<li> Jos tiliä ei ole paikallisen järjestelmänvalvojan AD FS-palvelimissa, valitse pyydettäessä järjestelmänvalvojan tunnistetietoja.</li>
<li> Varmista, että on Azure AD Connect-palvelin ja Web-sovelluksen välityspalvelimen välinen yhteys HTTP/HTTPS ennen tämän vaiheen suorittamista.</li>
<li> Varmista, että on HTTP/HTTPS sovelluksen verkkopalvelin ja AD FS ‑palvelin Salli todennus pyynnöt kautta kulkevan väliset yhteydet.</li>

![Web App](./media/active-directory-aadconnect-get-started-custom/adfs3.png)

Sinua kehotetaan Anna tunnistetiedot, jotta sovelluksen verkkopalvelin muodostaa suojatun yhteyden ADFS-palvelimeen. Nämä tunnistetiedot on oltava paikallisen järjestelmänvalvojan ADFS-palvelimeen.

![Välityspalvelin](./media/active-directory-aadconnect-get-started-custom/adfs4.png)

### <a name="specify-the-service-account-for-the-ad-fs-service"></a>Määritä palvelutilin AD FS-palveluun
AD FS-palvelu edellyttää todennusta käyttäjät ja haku käyttäjätiedot Active Directoryn toimialueen palvelutili. Se tukee kahdentyyppisiä palvelutilejä:

- **Ryhmän hallitun palvelutilin** - Active Directory-toimialueen palveluista, Windows Server 2012: ssa. Tämän tilin tyyppi on palvelut, kuten AD FS, eikä hänen nimeään tarvitse päivittää tilin säännöllisesti tilistä. Käytä tätä vaihtoehtoa, jos sinulla on jo Windows Server 2012 toimialueen ohjaimet toimialueeseen, joka kuuluu AD FS-palvelimiin.
- **Toimialueen käyttäjätili** - Tämä edellyttää, että salasanaa ja säännöllisesti salasanan, kun salasana muuttuvat tai päättyy. Käytä tätä vaihtoehtoa, vain, jos sinulla ei ole Windows Server 2012 toimialueen ohjaimet toimialueeseen, joka kuuluu AD FS-palvelimiin.

Jos olet valinnut ryhmään hallitun palvelutilin ja tämä ominaisuus ei ole koskaan käytetty Active Directoryssa, sinua kehotetaan yrityksen järjestelmänvalvojan tunnistetietoja. Nämä käytetään Active Directory-toiminnon käyttöön ottaminen ja aloittaa avaimen säilö.

![AD FS palvelun tili](./media/active-directory-aadconnect-get-started-custom/adfs5.png)

### <a name="select-the-azure-ad-domain-that-you-wish-to-federate"></a>Azure AD-toimialue, jota haluat järjestäjäorganisaatiota
Tässä määrityksessä käytetään AD FS ja Azure AD federation suhteen määritys. AD FS ongelman suojauksen tunnusten Azure AD, voit määrittää, ja se määrittää Azure AD luotetaanko tähän AD FS tunnusta. Tämän sivun avulla voit määrittää yhden toimialueen asennus vain. Voit määrittää lisää toimialueita myöhemmin suorittamalla Azure AD Connect uudelleen.

![Azure AD-toimialueesta](./media/active-directory-aadconnect-get-started-custom/adfs6.png)

### <a name="verify-the-azure-ad-domain-selected-for-federation"></a>Vahvista valitun yhdistämiseen Azure AD-toimialue
Kun toimialue on liitetty, Azure AD Connect voi tarvittavat tiedot Vahvista vahvistamaton toimialue. Katso [Lisää ja tarkista toimialue](../active-directory-add-domain.md) siitä, miten näitä tietoja.

![Azure AD-toimialueesta](./media/active-directory-aadconnect-get-started-custom/verifyfeddomain.png)

>[AZURE.NOTE]
Tarkista toimialueen määritys vaiheessa yrittää AD Connect. Jos jatkat määrittämiseen tarvittavat DNS-tietueiden lisäämättä, ohjattu toiminto ei voi Viimeistele määritys.

## <a name="configure-and-verify-pages"></a>Määritä ja tarkista sivut
Tällä sivulla tapahtuu määritykset.

>[AZURE.NOTE]
Ennen asennuksen jatkamista ja olet määrittänyt federaatio, varmista, että olet määrittänyt [liittoutumispalvelimia nimenselvitys](../active-directory-aadconnect-prerequisites.md#name-resolution-for-federation-servers).

![Voit määrittää asetukset](./media/active-directory-aadconnect-get-started-custom/readytoconfigure2.png)

### <a name="staging-mode"></a>Väliaikaisen tila
On mahdollista määrittäminen uuden synkronointi-palvelimen kanssa väliaikaisen tilassa rinnakkain. Se on tuettu vain yhden kansion pilveen vieminen yksi synkronointi-palvelin on. Mutta jos haluat siirtää toiseen palvelimeen, esimerkiksi yksi käynnissä ohjelmistovaatimuksista, valitse voit ottaa Azure AD Connect väliaikaisen tilassa. Käytössä, kun sync engine tuominen ja synkronoiminen tietojen normaalisti, mutta se ei vie mitään Azure AD- tai AD. Ominaisuuksien salasanan synkronointi ja salasanan takaisinkirjoituksen eivät ole käytettävissä samalla, kun väliaikaisen tilassa.

![Väliaikaisen tila](./media/active-directory-aadconnect-get-started-custom/stagingmode.png)

Väliaikaisen-tilassa, on mahdollista tehdä tarvittavat muutokset sync engine ja tarkista, mikä on tarkoitus viedä. Kun määritykset näyttää hyvältä, suorita ohjattu asennus uudelleen ja väliaikaisen tilan poistaminen käytöstä. Tiedot viedään nyt Azure AD tästä palvelimesta. Varmista, että toisen palvelimen käytöstä on vain yksi palvelin aktiivisesti vieminen samanaikaisesti.

Lisätietoja on artikkelissa [väliaikaisen tilassa](../active-directory-aadconnectsync-operations.md#staging-mode).

### <a name="verify-your-federation-configuration"></a>Tarkista liitetyn viestinnän määrittäminen
Azure AD Connect tarkistaa puolestasi DNS-asetukset, kun napsautat Vahvista-painiketta.

![Viimeistele](./media/active-directory-aadconnect-get-started-custom/completed.png)

![Tarkista](./media/active-directory-aadconnect-get-started-custom/adfs7.png)

Lisäksi suorittaa vahvistusta seuraavasti:

- Vahvista, että voit kirjautua sisään selaimella toimialueen liitettyjen tietokoneesta intranet: yhdistäminen https://myapps.microsoft.com ja tarkista kirjautuneena-tilisi kirjautumisnimi. Valmiin AD DS: N järjestelmänvalvojan tilillä ei ole synkronoitu, eikä sitä voi käyttää vahvistusta varten.
- Vahvista, että voit kirjautua sisään ekstranet-laitteesta. Home tietokoneeseen tai mobiililaitteeseen muodostaa https://myapps.microsoft.com ja anna tunnistetiedot.
- Vahvista rich client-kirjautuminen. Yhteyden muodostaminen https://testconnectivity.microsoft.com, valitse **Office 365** -välilehti ja valitse sitten **Office 365: n yksittäisen Sign-testi**.

## <a name="next-steps"></a>Seuraavat vaiheet
Kun asennus on valmis, kirjaudu ulos ja kirjaudu uudelleen Windows ennen kuin käytät synkronoinnin palvelun hallinta tai synkronoinnin säännön editorilla.

Kun olet tutustunut Azure AD Connect asennettuna, voit [Tarkista asennus ja määrittää käyttöoikeuksia](../active-directory-aadconnect-whats-next.md).

Lisätietoja näistä toiminnoista, jotka on otettu käyttöön asennuksessa: [Estä vahingossa poistaa](../active-directory-aadconnectsync-feature-prevent-accidental-deletes.md) ja [Azure AD yhteyden kunto](../active-directory-aadconnect-health-sync.md).

Lisätietoja näiden yleisiä aiheita: [ajoitus ja käynnistämisestä Synkronoi](../active-directory-aadconnectsync-feature-scheduler.md).

Lisätietoja [ympäristön integroinnissa paikallisen-käyttäjätietojen Azure Active Directory-hakemistosta](../active-directory-aadconnect.md).

## <a name="related-documentation"></a>Aiheeseen liittyvät ohjeet

Aihe |  
--------- | ---------
Azure AD Connect yleiskatsaus | [Azure Active Directory-integraation paikallisen käyttäjätietoja](../active-directory-aadconnect.md)
Asenna Express-asetusten avulla | [Pika-asennus, Azure AD Connect](active-directory-aadconnect-get-started-express.md)
Dirsync-työkalun päivittäminen | [Päivittää Azure AD-synkronointityökalu (DirSync)](active-directory-aadconnect-dirsync-upgrade-get-started.md)
Asennus tilit | [Lisätietoja Azure AD Connect asiakkaat ja käyttöoikeudet](active-directory-aadconnect-accounts-permissions.md)
