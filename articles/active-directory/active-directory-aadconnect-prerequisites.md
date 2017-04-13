<properties
   pageTitle="Azure AD Connect: Edellytykset ja laite | Microsoft Azure"
   description="Tässä ohjeaiheessa kerrotaan, vaatimat ja Azure AD Connect laitteistovaatimukset"
   services="active-directory"
   documentationCenter=""
   authors="andkjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="10/12/2016"
   ms.author="billmath"/>

# <a name="prerequisites-for-azure-ad-connect"></a>Azure AD edellytyksistä yhdistäminen
Tässä ohjeaiheessa kerrotaan, vaatimat ja Azure AD Connect laitteistovaatimukset.

## <a name="before-you-install-azure-ad-connect"></a>Ennen kuin asennat Azure AD Connect
Ennen kuin asennat Azure AD Connect, on muutamia asioita, joita tarvitset.

### <a name="azure-ad"></a>Azure AD
- Azure-tilaus tai [Azure-kokeiluversioon](https://azure.microsoft.com/pricing/free-trial/). Tämä on vain pakollinen Azure-portaalin käyttöä, ei Azure AD Connect avulla. Jos käytössäsi on PowerShell- tai Office 365 Azure tilauksen käyttämään Azure AD Connect ei tarvitse. Jos sinulla on Office 365-käyttöoikeus, voit käyttää myös Office 365-portaalissa. Kun maksettua Office 365-käyttöoikeus voit myös käyttää Azure portaaliin Office 365-portaalista.
    - Voit käyttää myös Azure AD-Esikatselutoiminto [Azure portal](https://portal.azure.com). Portaalin ei edellytä Azure käyttöoikeus.
- [Lisää ja tarkista toimialue](active-directory-add-domain.md) aiot käyttää Azure AD. Esimerkiksi jos aiot käyttää contoso.com käyttäjien sitten Varmista, että tämä toimialue on vahvistettu ja käytössäsi ei ole vain contoso.onmicrosoft.com oletustoimialueen.
- Azure AD-vuokraajan avulla oletusarvon 50k objektit. Kun olet tarkistanut toimialueen, raja kasvaa 300 k objekteihin. Jos tarvitset vielä enemmän objekteja Azure AD, sinun on avattava tukipyynnön on lisätty täsmentää rajoitus. Jos sinun on yli 500 k objektien sinun on käyttöoikeus, kuten Office 365: ssä, Azure AD Basic, Azure AD Premium tai yrityksen Mobility Suite.

### <a name="prepare-your-on-premises-data"></a>Paikallisen tietojen valmisteleminen
- Tarkista [valinnaisten synkronointiominaisuudet, voit ottaa käyttöön Azure AD](active-directory-aadconnectsyncservice-features.md) ja arvioi kannattaa ottaa käyttöön ominaisuudet.

### <a name="on-premises-servers-and-environment"></a>Paikallisten palvelimien ja ympäristön
- AD rakenteen versio ja metsää tason on oltava Windows Server 2003 tai uudempi versio. Toimialueen ohjaimet suorittamisen mikä tahansa versio, kunhan rakenteen ja metsää tason vaatimukset täyttyvät.
- Jos aiot käyttää ominaisuutta **salasanan takaisinkirjoituksen** toimialueen ohjaimet on oltava Windows Server 2008: ssa (ja uusimmat SP) tai uudempi. Jos yhteyttä ohjauskoneet ovat 2008 (R2 vuotta) on myös käyttää [korjaustiedoston KB2386717](http://support.microsoft.com/kb/2386717).
- Azure AD käyttämän toimialueen ohjauskoneen on oltava kirjoitettava. Se ei ole tuettu käyttämään Ohjauskoneen (vain luku-toimialueen ohjauskoneen) ja Azure AD Connect ei noudata minkä tahansa kirjoitusoikeudet Uudelleenohjaa.
- Azure AD Connect ei voi asentaa Small Business Server tai Windows Server Essentials. Palvelin on oltava käytössä Windows Server standard- tai paremmin.
- Azure AD Connect palvelin on oltava koko Graafisen, joka on asennettu. Ei voi asentaa server core.
- Azure AD Connect on oltava asennettuna, Windows Server 2008: ssa tai uudempi. Tämä palvelin saattaa olla toimialueen ohjauskoneen tai jäsenpalvelimeen, jos pika-asetuksia. Jos käytät mukautettuja asetuksia, palvelin voi olla myös erillinen ja ei ole liitetty toimialueeseen.
- Jos olet asentanut Azure AD Connect Windows Server 2008, varmista, että uusin korjausten käyttää Windows Update. Asennus ei voi alkaa palvelimessa.
- Jos aiot käyttää ominaisuutta **salasanojen synkronoinnin**, Azure AD Connect palvelin on oltava Windows Server 2008 R2 SP1 tai uudempi.
- Azure AD Connect palvelin on oltava [.NET Framework 4.5.1](#component-prerequisites) tai uudempi versio ja [Microsoft PowerShell 3.0](#component-prerequisites) tai uudempi versio.
- Jos Active Directory Federation Services otetaan käyttöön-palvelimet, johon on asennettu AD FS tai Web-sovelluksen välityspalvelin on oltava Windows Server 2012 R2: n tai sitä uudemmalla versiolla. [Windowsin etähallinnan](#windows-remote-management) on otettava käyttöön seuraavissa palvelimissa etäasennuksen varten.
- Jos Active Directory Federation Services otetaan käyttöön, sinun on [SSL-varmenteita](#ssl-certificate-requirements).
- Jos Active Directory Federation Services otetaan käyttöön, voit joutua määrittämään [nimenselvitys](#name-resolution-for-federation-servers).
- Azure AD Connect edellyttää käyttäjätiedot tallennetaan SQL Server-tietokantaan. Oletusarvon mukaan SQL Server 2012 Express LocalDB (light-versio, SQL Server Express) on asennettu ja palvelun service-tili on luotu paikallisessa tietokoneessa. SQL Server Express on 10 Gigatavun kokorajoitus, jonka avulla voit hallita 100 000 objekteja. Jos haluat hallita suurempi määrä directory-objekteja, sinun on ohjattu asennus osoittamaan toinen SQL Serverin asennus.
- Jos käytät eri SQL Server, käytä näitä vaatimuksia:
    - Azure AD Connect tukee Microsoft SQL Server-SQL Server 2008: n (SP4) SQL Server 2014 kaikki flavors. Microsoft Azure SQL-tietokanta on tietokantaan **ei tueta** .
    - Sinun on käytettävä asennustavan SQL-lajittelu. Nämä tunnistetaan \_CI_ nimessä. Se on käytettävä kirjainkoko on merkitsevä lajittelu-merkittyä **ei tueta** \_CS_ nimessä.
    - Voit käyttää vain yhden synkronointi-moduulin tietokannan esiintymän kohden. Näin voit jakaa tietokannan esiintymän FIM/MIM synkronoinnin, DirSync tai Azure AD-synkronointi **ei tueta** .

### <a name="accounts"></a>Tilit
- Azure Active directory Azure AD-Yleinen järjestelmänvalvoja-tili, jonka haluat integroida. Tämä on oltava **oppilaitoksen tai organisaation tili** ja et voi olla **Microsoft-tili**.
- Jos käyttämällä pika-asetuksia tai päivittää Dirsync-työkalun, sinun on oltava yrityksen järjestelmänvalvojan tilin oman paikallisen Active Directory.
- Jos käytät mukautettuja asetuksia asennuspolku [Active Directory-tilit](./connect/active-directory-aadconnect-accounts-permissions.md) .

### <a name="azure-ad-connect-server-configuration"></a>Azure AD Connect server-määritys
- Jos yleinen järjestelmänvalvojilla on otettu käyttöön MFA, URL-osoite **https://secure.aadcdn.microsoftonline-p.com** on oltava luotettujen sivustojen luettelosta. Sinua kehotetaan Lisää tämä luotettujen sivustojen luetteloon, jos sitä ei lisätä ennen kehotusta MFA-todennus. Voit käyttää Internet Explorerin lisääminen luotettuihin sivustoihin.

### <a name="connectivity"></a>Yhteys
- Azure AD Connect palvelin on DNS-ratkaisun intranet-ja internet. DNS-palvelin on voitava muodostaa nimien sekä paikallisen Active Directory sekä Azure AD-päätepisteet.
- Jos sinulla on sallittava intranetissä ja sinun on avattava portit Azure AD Connect-palvelimia ja toimialueen ohjainten välillä, valitse Lisätietoja [Azure AD Connect portit](active-directory-aadconnect-ports.md) .
- Jos välityspalvelimen tai palomuurin rajan jota voi käyttää URL-osoitteet, valitse URL-osoitteet kuvattu [alueet, Office 365: n URL-osoitteet ja IP-osoite](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2) on avattava.
    - Jos käytössäsi on Microsoft Cloud Saksan tai Microsoft Azure Government pilveen, katso kohta [Azure AD Connect synkronoi palvelun esiintymät huomioon otettavia seikkoja](active-directory-aadconnect-instances.md) URL-osoitteet.
- Azure AD Connect on oletusarvoisesti Azure AD yhteydenpito TLS 1.0 avulla. Voit muuttaa tämä TLS 1.2 noudattamalla [Azure AD Connect TLS 1.2 käyttöön](#enable-tls-12-for-azure-ad-connect).
- Jos käytössäsi on lähtevän liikenteen välityspalvelimen Internet-yhteyden muodostamisesta **C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config** tiedoston seuraava asetus on lisättävä ohjattu asennus-ja Azure AD Connect synkronointi on pystyttävä muodostamaan yhteys Internetiin ja Azure AD. Tämä teksti on kirjoitettava tiedoston alareunassa. Tässä koodissa &lt;PROXYADRESS&gt; todellinen välityspalvelimen IP-osoite tai isännän nimi.

```
    <system.net>
        <defaultProxy>
            <proxy
            usesystemdefault="true"
            proxyaddress="http://<PROXYADDRESS>:<PROXYPORT>"
            bypassonlocal="true"
            />
        </defaultProxy>
    </system.net>
```

- Jos välityspalvelimen-palvelin edellyttää käyttöoikeuden tarkistusta, [palvelutilin](./connect/active-directory-aadconnect-accounts-permissions.md#azure-ad-connect-sync-service-accounts) on sijaittava toimialueen ja sinun on käytettävä mukautettuja asetuksia asennuspolku voit määrittää [mukautetun palvelutilin](./connect/active-directory-aadconnect-get-started-custom.md#install-required-components). Sinun on myös machine.config eri muuttaminen. Tämän muutoksen machine.config ohjattu asennus- ja sync engine vastata käyttöoikeuksien välityspalvelin. Kaikki ohjatun asennuksen käytetään sivuja, **Määritä** sivun allekirjoitetut lukuun käyttäjän tunnistetiedot. Valitse ohjatun asennuksen lopussa **määrittäminen** -sivulla yhteydessä kytketään [palvelutilin](./connect/active-directory-aadconnect-accounts-permissions.md#azure-ad-connect-sync-service-accounts) , joka on luotu voit. Machine.config-osassa pitäisi näyttää tältä.

```
    <system.net>
        <defaultProxy enabled="true" useDefaultCredentials="true">
            <proxy
            usesystemdefault="true"
            proxyaddress="http://<PROXYADDRESS>:<PROXYPORT>"
            bypassonlocal="true"
            />
        </defaultProxy>
    </system.net>
```

Lisätietoja [oletusarvoisen välityspalvelimen elementti](https://msdn.microsoft.com/library/kd3cf2ex.aspx)MSDN: ssä.

Jos sinulla on yhteys liittyviä ongelmia, katso [yhteysongelmien vianmääritys](active-directory-aadconnect-troubleshoot-connectivity.md).

### <a name="other"></a>Muut
- Valinnainen: Testaa käyttäjätili varmista synkronointi.

## <a name="component-prerequisites"></a>Osan edellytykset

### <a name="powershell-and-net-framework"></a>PowerShell- ja .net Framework
Azure AD Connect riippuu PowerShellin Microsoft .NET Framework 4.5.1. Tarvitset tätä versiota tai palvelimen uudempi versio. Käytössä Windows Server-version mukaan seuraavasti:

- Windows Server 2012R2
  - PowerShellin Microsoft asennetaan oletusarvoisesti, mitään toimia ei tarvita.
  - Windows Updaten tarjotaan .NET framework 4.5.1 ja myöhemmissä versioissa. Varmista, että olet asentanut uusimmat päivitykset Windows Server Ohjauspaneelissa.
- Windows Server 2008R2 ja Windows Server 2012
  - PowerShellin Microsoft uusin versio on saatavana **Windows Management Framework 4.0**käytettävissä [Microsoft Download Centeristä](http://www.microsoft.com/downloads).
  - .NET framework 4.5.1 ja myöhemmissä versioissa ovat saatavilla [Microsoft Download Centeristä](http://www.microsoft.com/downloads).
- Windows Server 2008
  - PowerShellin uusimmat tuettuun sisältyy **Windows Management Framework 3.0**-käytettävissä [Microsoft Download Centeristä](http://www.microsoft.com/downloads).
 - .NET framework 4.5.1 ja myöhemmissä versioissa ovat saatavilla [Microsoft Download Centeristä](http://www.microsoft.com/downloads).

### <a name="enable-tls-12-for-azure-ad-connect"></a>Ottaa käyttöön TLS 1.2 Azure AD Connect
Azure AD Connect käyttää TLS 1.0 oletusarvoisesti välisen synkronoinnin ohjelma palvelimen ja Azure AD salaamiseen. Voit muuttaa määrittämällä .net-sovellukset voivat käyttää TLS 1.2 oletusarvoisesti palvelimessa. Lisätietoja TLS 1.2 löytyy [Microsoftin Security neuvoa 2960358](https://technet.microsoft.com/security/advisory/2960358).

1. TLS 1.2 ei voi ottaa käyttöön Windows Server 2008: ssa. Tarvitset Windows Server 2008R2 tai uudempi versio. Varmista, että olet asentanut käyttöjärjestelmäkohtaisia .net 4.5.1 korjaus on artikkelissa [Microsoftin Security neuvoa 2960358](https://technet.microsoft.com/security/advisory/2960358). Voit joutua tämä tai myöhemmällä jo asennettu palvelimeen.
2. Jos käytät Windows Server 2008R2, varmista, että TLS 1.2 on otettu käyttöön. TLS 1.2 on jo käytössä Windows Server 2012-palvelin ja sitä uudemmissa versioissa.
```
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2]
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client] "DisabledByDefault"=dword:00000000 "Enabled"=dword:00000001
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server] "DisabledByDefault"=dword:00000000 "Enabled"=dword:00000001
```
3. Kaikissa käyttöjärjestelmissä Määritä tämän rekisteriavaimen ja Käynnistä palvelin.
```
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319
"SchUseStrongCrypto"=dword:00000001
```
4. Jos haluat ottaa käyttöön TLS 1.2 sync engine palvelimeen ja SQL-etäpalvelimeen myös, varmista, että sinulla on tarvittavat versioita asennettuna [TLS 1.2 tuki Microsoft SQL Server](https://support.microsoft.com/kb/3135244).

## <a name="prerequisites-for-federation-installation-and-configuration"></a>Edellytyksistä federation asentaminen ja määrittäminen

### <a name="windows-remote-management"></a>Windowsin etähallinta
Kun Azure AD Connect avulla voit ottaa käyttöön Active Directory Federation Services tai Web-sovelluksen välityspalvelinta, Tarkista yhteys- ja onnistuu, varmista seuraavat vaatimukset:

- Jos kohdepalvelin on liitetty toimialueeseen, varmista, että Windows Remote hallitun käytössä
    - Laajennettuja PSH ikkunassa komento-komennolla`Enable-PSRemoting –force`
- Jos kohdepalvelin on muu kuin toimialueen liitetyssä WAP tietokoneessa, on muutama lisävaatimukset
    - Kohde tietokoneeseen (WAP machine):
         - Varmista, winrm (Windowsin etähallinnan ja muista) palvelua käytetään palvelut-laajennuksen kautta
         - Laajennettuja PSH ikkunassa komento-komennolla`Enable-PSRemoting –force`
    - Tietokoneessa, jossa ohjattu toiminto on käynnissä (Jos kohdetietokoneen on muu kuin toimialueen liitettyjen tai ei-luotettujen toimialue):
        - Laajennettuja PSH ikkunassa komento-komennolla`Set-Item WSMan:\localhost\Client\TrustedHosts –Value <DMZServerFQDN> -Force –Concatenate`
        - Kirjoita palvelimen hallinta:
             - Lisää DMZ WAP isäntä koneen resurssivarantoon (palvelimen hallinta -> Hallitse -> Lisää palvelimia... DNS-välilehdessä)
             - Palvelimen hallinnan kaikki palvelimet-välilehti: Napsauta hiiren kakkospainikkeella WAP palvelimen ja hallinta nimellä, kirjoita paikallisen (ei toimialueen) creds WAP koneen
             - Vahvista PSH etäyhteyksien, palvelimen hallinnan kaikki palvelimet-välilehden: WAP palvelimen hiiren kakkospainikkeella ja valitse Windows PowerShell.  Jotta Etä-PowerShell-istunnot voidaan muodostaa kannattaa avata PSH etäistunnon.

### <a name="ssl-certificate-requirements"></a>SSL-varmenteen vaatimukset
**Tärkeä:** on erittäin suositeltavaa käyttää samaa SSL-varmenteen AD FS-klusterin kaikki solmut sekä kaikki verkkosovelluksen välityspalvelimet.

- Varmenne on oltava X509 varmenne.
- Voit käyttää itse allekirjoitettua varmennetta liittoutumispalvelimia kurssin testiympäristössä. Kuitenkin tuotantoympäristössä, suosittelemme saada varmenteiden myöntäjältä julkinen.
    - Jos käytät varmenne, jota ei ole julkisesti luotetun, varmista, että kunkin Web-sovelluksen välityspalvelimen asennettuihin varmenne on luotettava paikallisen palvelimen ja kaikki liittoutumispalvelimia
- Varmenteen tunnuksen on vastattava federation palvelunimi (esimerkiksi sts.contoso.com).
    - Tunnistetiedot on joko aihe vaihtoehtoinen nimi (SAN) tunniste on tyyppi dNSName, tai jos merkintöjä ei ole SAN, hakijan nimen määritettyä yleinen nimi.  
    - Useita SAN merkintöjä voi olla paikalla olevat varmenteen, jos jonkin niistä vastaa federation palvelunimi.
    - Jos aiot käyttää työpaikkasi liittyminen, Lisää SAN tarvitaan arvolla **enterpriseregistration.** Seuraa organisaation käyttäjien pääasiallista nimi (UPN) liitteen esimerkiksi **enterpriseregistration.contoso.com**.
- CryptoAPI seuraava luonti (CNG) näppäimet ja avaimen tallennusvälineiden palveluntarjoajien varmenteita ei tueta. Tämä tarkoittaa sitä, sinun on käytettävä varmenne CSP (salauksen palveluntarjoajan) ja ei KSP (avaimen tallennustilan-palvelu) perusteella.
- Yleismerkki varmenteet ovat tuettuja.

### <a name="name-resolution-for-federation-servers"></a>Liittoutumispalvelimia nimenselvitys
- Määritä DNS-tietueet AD FS federation palvelunimi (esimerkiksi sts.contoso.com): intranet (sisäiseen DNS-palvelimeen) ja ekstranet julkisen DNS (toimialueen rekisteröintipalvelu kautta). Intranetin DNS record varmistaa, että käytät A tietueet ja ei CNAME-tietueet. Tämä tarvitaan windows-todennuksen toimisi oikein toimialueen liitettyjen tietokoneesta.
- Jos otat useita AD FS ‑palvelin tai Web-sovelluksen välityspalvelinta, varmista, että olet määrittänyt kuormituksen tasauspalvelun ja kuormituksen osoittamalla AD FS federation palvelunimi (esimerkiksi sts.contoso.com) DNS-tietueet.
- Varmista windows-todennusta toimimaan selaimen sovellusten Internet Explorerissa intranet-, että AD FS federation palvelunimi (esimerkiksi sts.contoso.com) lisätään IE intranet-vyöhyke. Tämä voidaan ohjata ryhmäkäytännön ja ottaa kaikki toimialueelle liitetyissä tietokoneissa.

## <a name="azure-ad-connect-supporting-components"></a>Azure AD Connect tukevat osat
Seuraavassa on luettelo Azure AD Connect asentaa palvelimessa, johon on asennettu Azure AD Connect osista. Tämä luettelo on basic pika-asennus.  Jos haluat käyttää eri SQL Server Asenna synkronoinnin palvelut-sivulla, valitse SQL Express LocalDB ei ole asennettu paikallisesti.

- Azure AD Connect kunto
- Microsoft Online Services-kirjautuminen avustajan IT-ammattilaisille (mutta ei riippuvuus asennettuna)
- Microsoft SQL Server 2012 komentoriviä apuohjelmat
- Microsoft SQL Server 2012: n pika-LocalDB
- Microsoft SQL Server 2012 Native Client
- Microsoft Visual C++ 2013 uudelleenjakamista paketti

## <a name="hardware-requirements-for-azure-ad-connect"></a>Azure AD Connect laitteistovaatimukset
Alla olevassa taulukossa näkyy Azure AD Connect synkronointi tietokoneen vähimmäisvaatimukset.

| Active Directoryn objektien määrälle. | SUORITIN | Muisti | Kovalevyn kokoa |
| ------------------------------------- | --- | ------ | --------------- |
| Alle 10 000 | 1,6 GHz | VÄHINTÄÄN 4 GT | 70 GT |
| 10 000 – 50 000 | 1,6 GHz | VÄHINTÄÄN 4 GT | 70 GT |
| 50 000 – 100 000 | 1,6 GHz | 16 GT | 100 GT |
| 100 000 tai useita objekteja, SQL Serverin täydellinen versio vaaditaan|  |  |  |
| 100 000 – 300 000 | 1,6 GHz | 32 GIGATAVUA | 300 GT |
| 300 000 – 600,000 | 1,6 GHz | 32 GIGATAVUA | 450 GT |
| Yli 600,000 | 1,6 GHz | 32 GIGATAVUA | 500 GT |

AD FS- tai Web-sovelluksen palvelimet tietokoneita vähimmäisvaatimukset on seuraava:

- Suoritin: Kahden core vähintään 1,6 GHz tai uudempi versio
- Muisti: vähintään 2 gt tai uudempi versio
- Azure AM: A2 määritysten tai uudempi versio

## <a name="next-steps"></a>Seuraavat vaiheet
Lisätietoja [ympäristön integroinnissa paikallisen-käyttäjätietojen Azure Active Directory-hakemistosta](active-directory-aadconnect.md).
