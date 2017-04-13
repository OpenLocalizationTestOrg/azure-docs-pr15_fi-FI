<properties
    pageTitle="Azure AD yhteyden kunto-agentti asennuksen | Microsoft Azure"
    description="Tämä on Azure AD yhteyden kunto-sivulla, joka kuvaa AD FS ja synkronoi agent-asennuksen."
    services="active-directory"
    documentationCenter=""
    authors="karavar"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="vakarand"/>


# <a name="azure-ad-connect-health-agent-installation"></a>Azure AD Connect kunto agentti asennus

Tämän asiakirjan opastaa asennuksesta ja määrityksestä Azure AD yhteyden kunto-käyttäjäagenttien. Voit ladata niistä [täältä](active-directory-aadconnect-health.md#download-and-install-azure-ad-connect-health-agent).

##  <a name="requirements"></a>Vaatimukset
Seuraavassa taulukossa on luettelo Azure AD yhteyden kunto käyttövaatimukset.

| Vaatimus | Kuvaus|
| ----------- | ---------- |
|Azure AD-Premium| Azure AD yhteyden kunto on Azure AD Premium-ominaisuus ja edellyttää Azure AD Premium. </br></br>Lisätietoja on artikkelissa [Azure AD Premium käytön aloittaminen](active-directory-get-started-premium.md) </br>Jos haluat aloittaa 30 päivän maksuton kokeiluversio, katso [Käynnistä kokeiluversion.](https://azure.microsoft.com/trial/get-started-active-directory/)|
|Sinun on oltava Yleinen järjestelmänvalvoja, että Azure AD Aloita Azure AD yhteyden kunto|Oletusarvon mukaan vain Yleiset järjestelmänvalvojat voit asentaa ja määrittää kunto tekijöiden aloittaminen, portaalin ja suorittaa minkä tahansa Azure AD yhteyden kunto kuluessa. Lisätietoja on ohjeaiheessa [käyttäjien hallinta Azure AD-kansio](active-directory-administer.md). <br><br> Roolin perusteella Access-ohjausobjektin avulla voit sallia Azure AD yhteyden kunto muiden käyttäjien pääsyn organisaatiossa. Lisätietoja on artikkelissa [roolin perusteella käyttöoikeuksien valvonta Azure AD yhteyden kunto.](active-directory-aadconnect-health-operations.md#manage-access-with-role-based-access-control) </br></br>**Tärkeä:** Tili, jota käytetään, kun asennat niistä on oltava työpaikan tai oppilaitoksen tiliä. Se ei voi olla Microsoft-tili. Lisätietoja on artikkelissa [Azure organisaationa rekisteröityminen](sign-up-organization.md)
|Azure AD yhteyden kunto-agentti asennettu kunkin kohdennettujen palvelimeen| Azure AD yhteyden kunto edellyttää kohdennettujen palvelimissa tietoja, jotka näkyvät portaalissa agent-asennusta. </br></br>Esimerkiksi saat tietoja AD FS paikallisen infrastruktuurin agentti on oltava asennettuna AD FS, AD FS ‑välityspalvelin ja Web-sovelluksen välityspalvelimen-palvelimiin. AD DS: ÄÄN infrastruktuurin agentti vastaavasti paikallisen oman tietojen käyttämistä varten on asennettava toimialueen ohjauskoneen. </br></br>**Tärkeä:** Tili, jota käytetään, kun asennat niistä on oltava työpaikan tai oppilaitoksen tiliä. Se ei voi olla Microsoft-tili. Lisätietoja on artikkelissa [Azure organisaationa rekisteröityminen](sign-up-organization.md)|
|Azure Palvelupäätepisteet lähtevä yhteys|Asennus- ja suorituksen aikana agentti vaativat Azure AD yhteyden kunto Palvelupäätepisteet. Jos lähtevien yhteyksien on estetty, varmista, että seuraavat päätepisteet lisätään sallittujen luetteloon: </br></br><li>& #42;. BLOB.Core.Windows.NET </li><li>& #42;. Queue.Core.Windows.NET</li><li>adhsprodwus.servicebus.Windows.NET - portin: 5671 </li><li>https://Management.Azure.com </li><li>https://S1.adhybridhealth.Azure.com/</li><li>https://policykeyservice.DC.AD.MSFT.NET/</li><li>https://Login.Windows.NET</li><li>https://Login.microsoftonline.com</li><li>https://Secure.aadcdn.microsoftonline-p.com</li> |
|Palomuurin porttien agentti-palvelimessa.| Agentti edellyttää seuraavia palomuurin porttien, oltava käynnissä, jotta agentti pitää yhteyttä Azure AD kunto-Palvelupäätepisteet.</br></br><li>TCP/UDP-porttiin 443</li><li>TCP/UDP-portin 5671</li>
|Mahdollistaa seuraavat sivustot, jos Internet Explorerin parannettu suojaus on käytössä| Jos Internet Explorerin parannettu suojaus on käytössä, seuraavat sivustot saa palvelimeen, joka on asennettu agentti suorittaminen.</br></br><li>https://Login.microsoftonline.com</li><li>https://Secure.aadcdn.microsoftonline-p.com</li><li>https://Login.Windows.NET</li><li>Organisaation luottaa Azure Active Directory federation-palvelin. Esimerkki: https://sts.contoso.com</li>




## <a name="installing-the-azure-ad-connect-health-agent-for-ad-fs"></a>Azure AD asentaminen yhteyden AD FS kunto-agentti
Käynnistä agent-asennus, kaksoisnapsauta lataamasi .exe-tiedosto. Valitse ensimmäisessä näytössä Valitse Asenna.

![Tarkista Azure AD Connect kunto](./media/active-directory-aadconnect-health-requirements/install1.png)

Kun asennus on valmis, valitse Määritä nyt.

![Tarkista Azure AD Connect kunto](./media/active-directory-aadconnect-health-requirements/install2.png)

Komentorivi käynnistetään, jotkin PowerShell, joka suorittaa Rekisteröi AzureADConnectHealthADFSAgent perään. Azure Kirjaudu pyydettäessä Siirry eteenpäin ja kirjaudu sisään.

![Tarkista Azure AD Connect kunto](./media/active-directory-aadconnect-health-requirements/install3.png)


Kun olet kirjautunut sisään, PowerShell säilyvät. Kun se on valmis, voit sulkea PowerShell ja määritys on valmis.

Tässä vaiheessa palvelut tulisi aloittaa salliminen automaattisesti ja kerätä tietoja agentti. Jos olet ei täyty kaikki kuvatut edellisten kohtien vaatimat, varoitukset näkyvät PowerShell-ikkunassa. Muista suorittaa [vaatimukset](active-directory-aadconnect-health-agent-install.md#requirements) ennen asennusta agentti. Seuraava kuva on esimerkki virheet.

![Tarkista Azure AD Connect kunto](./media/active-directory-aadconnect-health-requirements/install4.png)

Voit tarkistaa agentti on asennettu, Etsi seuraavat palvelut-palvelimeen. Jos olet suorittanut määritykset, ne jo olisi ole käynnissä. Muussa tapauksessa ne pysäytetään, kunnes määritys on valmis.

- Azure AD Connect kunto AD FS diagnostiikka-palvelu
- Azure AD Connect kunto AD FS havainnollistamisen palvelu
- Azure AD Connect seuranta palvelun kunto AD FS

![Tarkista Azure AD Connect kunto](./media/active-directory-aadconnect-health-requirements/install5.png)


### <a name="agent-installation-on-windows-server-2008-r2-servers"></a>Windows Server 2008 R2-palvelimien Agent-asennuksen

Windows Server 2008 R2-palvelinten vaiheet:

1. Varmista, että palvelin on käynnissä Service Pack 1 tai suurempi.
1. Käytöstä Internet Explorerin ESC agentti asennusta varten:
1. Asenna Windows PowerShell 4.0 kaikki palvelimet ennen asennuksen AD kunto-agentti. Asenna Windows PowerShell 4.0 seuraavasti:
 - Asenna [Microsoft .NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=40779) Lataa offline-tilassa asennusohjelma napsauttamalla seuraavaa linkkiä avulla.
 - Asenna PowerShell ise: (-Windows-ominaisuudet)
 - Asenna [Windows Management Framework 4.0.](https://www.microsoft.com/download/details.aspx?id=40855)
 - Asenna Internet Explorer-version 10 tai uudempi palvelimessa. (Vaatii kunto-palvelun todentamiseen Azure järjestelmänvalvojan tunnistetiedoilla.)
1. Lisätietoja saat lisätietoja Windows PowerShell 4.0 asentamisesta Windows Server 2008 R2, wiki-artikkelissa [tähän](http://social.technet.microsoft.com/wiki/contents/articles/20623.step-by-step-upgrading-the-powershell-version-4-on-2008-r2.aspx).

### <a name="enable-auditing-for-ad-fs"></a>AD FS seurannan ottaminen käyttöön

> [AZURE.NOTE] Tässä osassa koskee vain AD FS-liittoutumispalvelimia.

Azure AD yhteyden kunto-agentti on AD FS valvontalokien tiedot, jotta voit kerätä ja analysoida tietoja käyttötietoja tunnin-toiminnon. Lokitiedostot eivät ole käytössä oletusarvoisesti. Seuraavien ohjeiden avulla voit valvoa AD FS ja Etsi AD FS-valvontalokien AD FS-palvelimiin.

#### <a name="to-enable-auditing-for-ad-fs-20"></a>Jos haluat ottaa käyttöön seurannan AD FS 2.0

1. Napsauta **Käynnistä-painiketta**, valitse **Ohjelmat**, **Valvontatyökalut**ja valitse sitten **Paikallinen suojauskäytäntö**.
2. Siirry **Suojauksen asetukset\Suojausasetukset\Paikalliset Policies\User Rights Management** -kansioon ja kaksoisnapsauta sitten Luo suojauksen auditointeja.
3. Varmista, että AD FS 2.0 palvelutilin näkyy **Paikallinen suojausasetus** -välilehdessä. Jos sitä ei ole, valitse **Lisää käyttäjä tai ryhmä** ja lisääminen luetteloon ja valitse sitten **OK**.
4. Voit valvoa, Avaa komentorivi oikeuksin ja suorittamalla seuraavan komennon:<code>auditpol.exe /set /subcategory:"Application Generated" /failure:enable /success:enable</code>
5. Paikallinen suojauskäytäntö Sulje ja avaa sitten hallinta-laajennus. Avaa hallinta-laajennus, **Käynnistä-painiketta**, valitse **Ohjelmat**, **Valvontatyökalut**ja valitse sitten AD FS 2.0-hallinta.
6. Valitse Toiminnot-ruudusta Muokkaa Federation palvelun ominaisuuksia.
7. Valitse **tapahtumat** -välilehti **Federation palveluominaisuudet** -valintaikkunassa.
8. Valitse **onnistuneiden** ja **virheen valvoo** -valintaruudut.
9. Valitse **OK**.

#### <a name="to-enable-auditing-for-ad-fs-on-windows-server-2012-r2"></a>Voit valvoa Windows Server 2012 R2-AD FS: lle

1. Avaa **Paikallinen suojauskäytäntö** avaamalla työpöydän tehtäväpalkissa **Palvelimen hallinnan** aloitusnäytön tai palvelimen hallinta ja valitse sitten **Työkalut/Paikallinen suojauskäytäntö**.
2. Siirry **Suojauksen \Suojausasetukset\Paikalliset** -kansioon ja kaksoisnapsauta sitten **suojauksen valvoo**.
3. Varmista, että AD FS-palvelutilin näkyy **Paikallinen suojausasetus** -välilehdessä. Jos sitä ei ole, valitse **Lisää käyttäjä tai ryhmä** ja lisääminen luetteloon ja valitse sitten **OK**.
4. Voit valvoa, Avaa komentorivi oikeuksin ja suorittamalla seuraavan komennon:<code>auditpol.exe /set /subcategory:"Application Generated" /failure:enable /success:enable.</code>
5. **Paikallinen suojauskäytäntö**Sulje ja avaa sitten **AD FS hallinta** -laajennus (palvelimen hallinnan työkalut ja valitse sitten AD FS-hallinta).
6. Valitse Toiminnot-ruudusta **Muokkaa Federation palvelun ominaisuuksia**.
7. Valitse **tapahtumat** -välilehti Federation palveluominaisuudet-valintaikkunassa.
8. **Onnistuneiden ja virheen valvoo** -valintaruudut ja valitse sitten **OK**.






#### <a name="to-locate-the-ad-fs-audit-logs"></a>Voit etsiä AD FS-valvonta lokit


1. Avaa **Tapahtumienvalvonta**.
2. Siirry Windows-lokit ja valitse **Suojaus**.
3. Valitse oikeanpuoleisesta **Suodattimen nykyisessä lokien**.
4. Valitse Tapahtumalähde-kohdassa **AD FS tarkistaminen**.

![AD FS valvontalokit](./media/active-directory-aadconnect-health-requirements/adfsaudit.png)

> [AZURE.WARNING] Jos on olemassa Ryhmäkäytäntö, joka on käytöstä AD FS tarkistaminen, Azure AD yhteyden kunto-agentti ei voi kerätä tietoja. Varmista, että sinulla ei ole Ryhmäkäytäntö, joka voi olla käytöstä tarkistaminen.

[//]: # (Start of Agent Proxy Configuration Section)

## <a name="installing-the-azure-ad-connect-health-agent-for-sync"></a>Synkronoi Azure AD yhteyden kunto-agentti asentaminen
Synkronoi Azure AD yhteyden kunto-agentti asennetaan automaattisesti Azure AD Connect uusimmassa versiossa. Käytä Azure AD Connect-synkronointia varten, tarvitset Azure AD Connect uusimman version Lataa ja asenna se. Voit ladata uusin versio [tästä](http://www.microsoft.com/download/details.aspx?id=47594).

Voit tarkistaa agentti on asennettu, Etsi seuraavat palvelut-palvelimeen. Jos olet suorittanut määritykset, ne jo olisi ole käynnissä. Muussa tapauksessa ne pysäytetään, kunnes määritys on valmis.

- Azure AD Connect kunto synkronoi tietoja palvelun
- Azure AD Connect seuranta palvelun kunto synkronointi

![Tarkista Azure AD Connect kunto-synkronointia varten](./media/active-directory-aadconnect-health-sync/services.png)

> [AZURE.NOTE] Muista, että Azure AD yhteyden kunto käyttäminen edellyttää Azure AD Premium. Jos sinulla ei ole Azure AD Premium, et voi Viimeistele määritys Azure-portaalissa. Lisätietoja on artikkelissa [vaatimukset-sivu](active-directory-aadconnect-health-agent-install.md#requirements).


## <a name="manual-azure-ad-connect-health-for-sync-registration"></a>Synkronoi rekisteröinti manuaalinen Azure AD yhteyden kunto
Jos Azure AD yhteyden kuntotietojen synkronointi agentti rekisteröinnin epäonnistuu asennettuasi Azure AD Connect onnistuneesti, voit rekisteröidä manuaalisesti agentti PowerShell-komentoa.

>[AZURE.IMPORTANT] PowerShell-komennolla on vain pakollinen, jos agentti rekisteröinti epäonnistuu Azure AD Connect asentamisen jälkeen.

Seuraavat PowerShell-komento on pakollinen vain kun kunto-agentti rekisteröinti epäonnistuu, vaikka asentamiseen ja Azure AD Connect määrittäminen. Azure AD yhteyden kunto-palveluiden alkaa, kun agentti on rekisteröity.

Voit rekisteröidä manuaalisesti Azure AD yhteyden kunto-agentti Synkronoi seuraavan PowerShell-komennon avulla:

`Register-AzureADConnectHealthSyncAgent -AttributeFiltering $false -StagingMode $false`

Seuraavat parametrit komennolla:

- AttributeFiltering: $true (oletus) – Jos Azure AD Connect ei synkronoidu oletusarvo-määrite määrittäminen ja käyttäminen suodatetut määrite on mukautettu. $false muulla tavalla.
- StagingMode: $false (oletus) – Jos Azure AD Connect-palvelin ei ole väliaikaisen $true-tilassa, jos palvelin on määritetty väliaikaisen tila on.

Pyydettäessä todennusta varten sinun on käytettävä samaa yleisen järjestelmänvalvojan tililtä (kuten admin@domain.onmicrosoft.com) , joka on käytetty Azure AD Connect määrittämistä varten.

## <a name="installing-the-azure-ad-connect-health-agent-for-ad-ds"></a>Azure AD asentaminen yhteyden AD DS: N kunto-agentti
Käynnistä agent-asennus, kaksoisnapsauta lataamasi .exe-tiedosto. Valitse ensimmäisessä näytössä Valitse Asenna.

![Tarkista Azure AD Connect kunto](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install1.png)

Kun asennus on valmis, valitse Määritä nyt.

![Tarkista Azure AD Connect kunto](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install2.png)

Komentorivi käynnistetään, jotkin PowerShell, joka suorittaa Rekisteröi AzureADConnectHealthADDSAgent perään. Azure Kirjaudu pyydettäessä Siirry eteenpäin ja kirjaudu sisään.

![Tarkista Azure AD Connect kunto](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install3.png)

Kun olet kirjautunut sisään, PowerShell säilyvät. Kun se on valmis, voit sulkea PowerShell ja määritys on valmis.

Tässä vaiheessa palvelut tulisi aloittaa salliminen automaattisesti ja kerätä tietoja agentti. Jos olet ei täyty kaikki kuvatut edellisten kohtien vaatimat, varoitukset näkyvät PowerShell-ikkunassa. Muista suorittaa [vaatimukset](active-directory-aadconnect-health-agent-install.md#requirements) ennen asennusta agentti. Seuraava kuva on esimerkki virheet.

![Tarkista Azure AD Connect kunto AD DS: ÄÄN varten](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install4.png)

Voit tarkistaa agentti on asennettu, Etsi seuraavat palvelut toimialueen ohjauskoneen.

- Azure AD Connect kunto AD DS: ÄÄN havainnollistamisen palvelu
- Azure AD Connect seuranta palvelun kunto AD DS

Jos suoritit määritykset, palveluista on jo käynnissä. Muussa tapauksessa ne pysäytetään, kunnes määritys on valmis.

![Tarkista Azure AD Connect kunto](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install5.png)

## <a name="installing-the-azure-ad-connect-health-agent-for-ad-ds-on-server-core"></a>Asentaminen Azure AD yhteyden AD DS-Server Core kunto-agentti.
Asennettuasi .exe-tiedoston, voit suorittaa rekisteröinnin loppuun PowerShell-komennon avulla:

`Register-AzureADConnectHealthADDSAgent -Credential $cred`

## <a name="configure-azure-ad-connect-health-agents-to-use-http-proxy"></a>Määritä Azure AD yhteyden kunto agenttien vuoksi käyttämään HTTP-välityspalvelin
Voit määrittää Azure AD yhteyden kunto agenttien vuoksi toimimaan HTTP-välityspalvelin.

>[AZURE.NOTE]
- Käyttämällä "Netsh WinHttp Aseta ProxyServerAddress" ei tueta, kun agentti käyttää System.Net sen sijaan, että Microsoft Windows HTTP Services web-pyynnöt.
- Määritettyä Http-välityspalvelin-osoitetta käytetään läpivienti salattuja Https-viestejä.
- Todennettu välityspalvelimet (joko HTTPBasic) ei tueta.

### <a name="change-health-agent-proxy-configuration"></a>Muuta kunto agentti välityspalvelimen määritykset
Sinulla on seuraavat vaihtoehdot määrittäminen Azure AD yhteyden kunto edustaja, joka käyttää HTTP-välityspalvelin.

>[AZURE.NOTE]Kaikki Azure AD yhteyden kunto Agent-palvelu on käynnistettävä päivitettävät välityspalvelimen järjestyksessä. Suorita seuraava komento:<br>
-Palvelu AdHealth *

#### <a name="import-existing-proxy-settings"></a>Tuo aiemmin välityspalvelimen asetukset

##### <a name="import-from-internet-explorer"></a>Tietojen tuominen Internet Explorerissa
Internet Explorerin HTTP-välityspalvelimen voi tuoda käytettävän Azure AD yhteyden kunto-tekijöiden. Jokaisessa palvelimessa on kunto-agentti suorittaa seuraavan PowerShell-komennon:

    Set-AzureAdConnectHealthProxySettings -ImportFromInternetSettings

##### <a name="import-from-winhttp"></a>Tietojen tuominen WinHTTP
WinHTTP välityspalvelimen voi tuoda käytettävän Azure AD yhteyden kunto-tekijöiden. Jokaisessa palvelimessa on kunto-agentti suorittaa seuraavan PowerShell-komennon:

    Set-AzureAdConnectHealthProxySettings -ImportFromWinHttp

#### <a name="specify-proxy-addresses-manually"></a>Välityspalvelimen osoitteet manuaalinen määrittäminen
Voit määrittää välityspalvelimen manuaalisesti jokaisen palvelimiin kunto-agentti suorittamalla seuraavan PowerShell-komennon:

    Set-AzureAdConnectHealthProxySettings -HttpsProxyAddress address:port

Esimerkki: *määrittäminen AzureAdConnectHealthProxySettings - HttpsProxyAddress myproxyserver: 443*

- "address" voi olla DNS ratkaistava palvelimen nimi tai IPv4-osoite
- "portin" jätetään pois. Jos pois sitten 443 on valittu oletusportti.

#### <a name="clear-existing-proxy-configuration"></a>Poista välityspalvelimen määritys
Voit poistaa välityspalvelimen määritys suorittamalla seuraavan komennon:

    Set-AzureAdConnectHealthProxySettings -NoProxy


### <a name="read-current-proxy-settings"></a>Lue nykyiset välityspalvelimen asetukset
Voit lukea määritettyjen välityspalvelimen asetukset suorittamalla seuraavan komennon:

    Get-AzureAdConnectHealthProxySettings


## <a name="test-connectivity-to-azure-ad-connect-health-service"></a>Testaa yhteys Azure AD yhteyden palvelun kunto
On mahdollista, että ongelmia voi syntyä, joka aiheuttaa Azure AD yhteyden kunto-edustaja, joka Azure AD yhteyden kunto-palvelussa yhteys katkeaa. Näitä ovat verkko, oikeudet ongelmia tai eri syistä.

Jos agentti ei voi lähettää tietoja yli kaksi tuntia Azure AD yhteyden kunto-palveluun, se näkyy portaalissa seuraava ilmoitus: "palvelun kunto-tietoja ei ole ajan tasalla." Voit vahvistaa, jos kyseessä olevaan Azure AD Connect kunnon ei voi ladata palveluun, tietojen Azure AD yhteyden kunto-palveluun suorittamalla seuraavan PowerShell-komennon:

    Test-AzureADConnectHealthConnectivity -Role ADFS

Rooli-parametri on tällä hetkellä seuraavat arvot:

- ADFS
- Synkronointi
- LISÄÄ

Voit - ShowResults merkintä komento tarkastella yksityiskohtaisia lokeja. Käytä seuraavassa esimerkissä:

    Test-AzureADConnectHealthConnectivity -Role Sync -ShowResult

>[AZURE.NOTE]Yhteys-työkalun käyttäminen sinun on suoritettava agentti rekisteröinnin. Jos et pysty suorittamaan agent-rekisteröinnin, varmista, että täyttyvät Azure AD yhteyden kunto kaikki [vaatimuksia](active-directory-aadconnect-health-agent-install.md#requirements) . Tämä Yhteystesti suoritetaan oletusarvoisesti agentti rekisteröinnin yhteydessä.



## <a name="related-links"></a>Aiheeseen liittyvät linkit

* [Azure AD Connect kunto](active-directory-aadconnect-health.md)
* [Azure AD Connect kunto-toiminnot](active-directory-aadconnect-health-operations.md)
* [Käyttämällä Azure AD yhteyden kunto AD FS](active-directory-aadconnect-health-adfs.md)
* [Azure AD yhteyden kunto käytön synkronointi](active-directory-aadconnect-health-sync.md)
* [Kuntotietojen käyttämällä Azure AD yhteydessä AD DS: ÄÄN](active-directory-aadconnect-health-adds.md)
* [Azure AD Connect kunto usein kysytyt kysymykset](active-directory-aadconnect-health-faq.md)
* [Azure AD Connect kunto versiohistoria](active-directory-aadconnect-health-version-history.md)
