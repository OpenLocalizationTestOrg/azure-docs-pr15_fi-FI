<properties
    pageTitle="Azure pinossa MySQL resurssi-palvelun käyttöön | Microsoft Azure"
    description="Resurssin MySQL-palvelun käyttöön Azure pinon toimimalla seuraavasti."
    services="azure-stack"
    documentationCenter=""
    authors="Dumagar"
    manager="byronr"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="dumagar"/>


# <a name="deploy-the-mysql-resource-provider-on-azure-stack-to-use-with-webapps"></a>Ottaa käyttöön MySQL resurssin tarjoajaan Azure pinossa WebApps käyttäminen

> [AZURE.NOTE] Seuraavat tiedot koskevat vain Azure pinon TP1 ominaisuuksissa.

Käytä tämän artikkelin ohjeiden yksityiskohtaiset MySQL resurssi-palvelun määrittäminen käsitteiden vahvistamisen Azure pinon kuitti niin, että voit [käyttää MySQL-tietokantojen](azure-stack-mysql-rp-deploy-short.md) Azure pinon, mukaan lukien käyttäen MySQL taustaan WordPress sivustojen, joka on luotu käyttämällä [Azure verkkosovelluksissa](azure-stack-webapps-deploy.md).

## <a name="set-up-steps-before-you-deploy"></a>Määritä vaiheet ennen niiden käyttöönottoa

Ennen kuin otat resurssin tarjoajaan, sinun on:

- On Windows Server oletuskuvan .NET 3.5 kanssa
- Internet Explorer (IE) parannettu suojaus poistaminen käytöstä
- PowerShellin Azure uusimman version asentaminen

### <a name="create-an-image-of-windows-server-including-net-35"></a>Luo Windows Server, mukaan lukien .NET 3.5 kuva

Jos latasit Azure pinon bittien jälkeen 2/23/2016 koska perus Windows Server 2012 R2 oletuskuva sisältää .NET 3.5 framework tämän ladattavan ja uudempi versio, voit ohittaa tämän vaiheen.

Jos latasit ennen 2/23/2016, sinun on luotava Windows Server 2012 R2 palvelinkeskuksen Näennäiskiintolevyn .NET 3.5 kuvallinen ja määrittäminen on Platform kuva säilössä oletuskuva.

### <a name="turn-off-ie-enhanced-security-and-enable-cookies"></a>Internet Explorerin käytöstä parannettu suojaus ja ota käyttöön evästeet

Resurssin palveluntarjoaja ottamaan suoritat PowerShell integroitu komentosarjat ympäristössä (ise:) järjestelmänvalvojana, niin haluat sallia evästeet ja JavaScript Internet Explorer-profiili, jota käytät Azure Active Directory kirjautuminen järjestelmänvalvoja ja käyttäjä kirjautumiset.

**Jos haluat poistaa käytöstä Internet Explorerin parannettu suojaus:**

1. Azure-pino käsitteiden, (käsitteiden) tietokoneeseen järjestelmänvalvojana AzureStack/sisäänkirjautuminen ja avaa sitten palvelimen hallinta.

2. Poista käytöstä **Internet Explorerin suojaus-parannetun** järjestelmänvalvojia ja käyttäjiä.

3. Kirjaudu sisään järjestelmänvalvojana **ClientVM.AzureStack.local** virtuaalikoneen ja avaa sitten palvelimen hallinta.

4. Poista käytöstä **Internet Explorerin suojaus-parannetun** järjestelmänvalvojia ja käyttäjiä.

**Evästeiden ottaminen käyttöön:**

1. Windowsin aloitusnäyttöön **kaikki sovellukset**, **Windows**-apuohjelmat, **Internet Explorerin**hiiren kakkospainikkeella, valitse **Lisää**ja valitse **Suorita järjestelmänvalvojana**.

2. Jos näyttöön tulee kehote, tarkista **suositeltavaa käyttää suojaus**ja valitse sitten **OK**.

3. Valitse Internet Explorerissa **Työkalut (hammaspyöräkuvake)** &gt; **Internet-asetukset** &gt; **Tietosuoja** -välilehti.

4. **Lisäasetukset**, varmista, että molemmat **Hyväksy** -painikkeet ovat valittuina, valitse **OK**ja valitse sitten **OK** .

5. Sulje Internet Explorer ja Käynnistä PowerShell ise: järjestelmänvalvojana.

### <a name="install-an-azure-stack-compatible-release-of-azure-powershell"></a>Asenna Azure pinon PowerShellin Azure yhteensopiva julkaisua

1. Poista kaikki olemassa olevat PowerShellin Azure asiakkaan AM.

2. Kirjaudu Azure pinon Käsitteiden koneen AzureStack/järjestelmänvalvojan oikeuksilla.

3. Käytä etätyöpöydän kautta, kirjaudu **ClientVM.AzureStack.local** virtuaalikoneen järjestelmänvalvojana.

4. Avaa Ohjauspaneeli ja valitse **Poista ohjelman asennus** &gt; valitsemalla **PowerShellin Azure** &gt; **Poista asennus**.

5. [Lataa uusimmat PowerShellin Azure-, joka tukee Azure pinoa](http://aka.ms/azstackpsh) ja asenna se.

    Kun olet asentanut PowerShell, voit suorittaa tämän tarkastuksen PowerShell-komentosarjaa varmistaaksesi, että voit muodostaa yhteyden oman Azure pinon esiintymän (Kirjaudu sisään verkkosivulle näkyy).

## <a name="bootstrap-the-resource-provider-deployment-powershell"></a>Automaattinen resurssin palvelun käyttöönoton PowerShell

1. Azure pinon Käsitteiden etätyöpöydän yhteyden clientVm.AzureStack.Local ja kirjaudu azurestack\\azurestackuser.

2. [Lataa MySQL RP binaaritiedostoja](http://aka.ms/masmysqlrp) tiedoston ja Pura se D:\\MySQLRP.

3. Suorita D:\\MySQLRP\\Bootstrap.cmd tiedoston järjestelmänvalvojana (azurestack\administrator).

    Bootstrap.ps1-tiedosto avautuu PowerShell ise:.

4. Kun windows PowerShell ise: on valmis ladataan, valitse "toista"-painiketta tai painamalla F5-näppäintä.

    Kaksi tärkeintä välilehteä ladataan sisältävät komentosarjojen ja ottaa käyttöön palveluntarjoajan MySQL resurssin tarvitsemasi tiedostot.

## <a name="prepare-prerequisites"></a>Valmistele edellytykset

Valitse **Valmistele edellytykset** välilehteä:

- Luo tarvittavat varmenteet
- Lataa MySQL binaaritiedostot Azure-pino
- Lataa palvelutiedot tallennustilan tilin Azure pinossa
- Julkaise valikoima kohteet

### <a name="create-the-required-certificates"></a>Luo tarvittavat varmenteet
Lisää **Uusi SslCert.ps1** -komentosarja \_. AzureStack.local.pfx SSL-varmenne, D:\\MySQLRP\\edellytykset\\BlobStorage\\säilö-kansio. Varmenteen suojaa välisen resurssin tarjoajaan ja paikallisen esiintymä Azure Resurssienhallinta.

1. Valitse **Valmistele edellytykset** pää-välilehden **Uusi SslCert.ps1** -välilehti ja se toimii puhelimessasi.

2. Kirjoita kehote, joka tulee näkyviin, PFX salasanaa, jolla suojaa yksityinen avain ja **Merkitse muistiin tätä salasanaa**. Tarvitset niitä myöhemmin.

### <a name="download-mysql-binaries-to-your-azure-stack"></a>Lataa MySQL binaaritiedostot Azure-pino

1. Valitse **Lataa MySqlServer.ps1** -välilehti ja se toimii puhelimessasi.
2. Valitse kehotettaessa **Kyllä** Vahvista-valintaikkunassa Hyväksy käyttöoikeussopimus.

    Tämä komento lisää kahden zip-tiedoston D:\MySql\Prerequisites\BlobStorage\Container-kansioon.

### <a name="upload-all-artifacts-to-a-storage-account-on-azure-stack"></a>Lataa kaikki palvelutiedot tallennustilan tilin Azure pinossa

1. Valitse **Lataa Microsoft.MySql-RP.ps1** -välilehti ja se toimii puhelimessasi.

2. Kirjoita Windows PowerShellin tunnistetiedon pyynnön-valintaikkunaan Azure pinon palvelun järjestelmänvalvojan tunnistetietoja.

3. Azure Active Directory vuokraajan tunnuksen pyydettäessä Kirjoita Azure Active Directory vuokraajan täydellinen toimialuenimi: esimerkiksi microsoftazurestack.onmicrosoft.com.

    Ponnahdusikkunan pyytää tunnistetietoja.

    > [AZURE.TIP] Jos Ponnahdusvalikossa ei näy, voit joko eivät ole poistettu käytöstä Internet Explorerin parannettu suojaus JavaScriptin ottaminen käyttöön tämän tietokoneen ja käyttäjän tai IE evästeitä ole vielä hyväksynyt. Katso [vaiheet ennen niiden käyttöönottoa määrittäminen](#set-up-steps-before-you-deploy).

4. Kirjoita Azure pinon palvelun järjestelmänvalvojan tunnistetietoja ja valitse sitten **Kirjaudu sisään**.

### <a name="publish-gallery-items-for-later-resource-creation"></a>Julkaise valikoima kohteet myöhemmin resurssin luontia varten

Valitse **Julkaise GalleryPackages.ps1** -välilehti ja se toimii puhelimessasi. Tämä komentosarja lisää kaksi marketplace-kohdetta Azure pinon Käsitteiden portal marketplace, joiden avulla voit ottaa käyttöön tietokannan resurssit marketplace kohteet.

## <a name="deploy-the-mysql-resource-provider-vm"></a>MySQL resurssin tarjoajaan AM käyttöönotto

Nyt kun olet valmistellut Azure pinon käsitteiden tarvittavat varmenteet ja marketplace kohteita, voit ottaa käyttöön SQL Server-resurssi-palvelu. -Välilehdessä **käyttöön MySQL-palvelu** :

   - Anna arvot JSON-tiedostossa, joka viittaa käyttöönottoprosessin
   - Ottaa käyttöön resurssin tarjoajaan
   - Päivitä paikalliset DNS
   - SQL Server resurssin tarjoajan sovittimen rekisteröimistä

### <a name="provide-values-in-the-json-file"></a>Anna arvot JSON-tiedostossa

Valitse **Microsoft.MySqlprovider.Parameters.JSON**. Tämä tiedosto on Azure Resurssienhallinta-mallin käyttöön oikein Azure pinon korjattava parametreja.

1. Täytä **tyhjään** parametrit JSON-tiedostossa:

    - Varmista, että annat **adminusername** ja **adminpassword** MySQL resurssin tarjoajan AM.

    - Varmista, että annat salasanan **SetupPfxPassword** -parametri, jonka teit merkille [valmisteleminen prequisites](#prepare-prerequisites) vaiheessa.

    - Varmista, että annat **basicAuthUserName** ja **basicAuthPassword** parametrit. **Huomioi seuraavat arvot.** Tarvitset niitä myöhemmin, voit rekisteröidä resurssin toimittaja.

2. Valitse **Tallenna**.

### <a name="deploy-the-resource-provider"></a>Ottaa käyttöön resurssin tarjoajaan

1. Valitse **Ota käyttöön-Microsoft.Mysql-provider.PS1** -välilehti ja Suorita komentosarja.
2. Kirjoita nimesi vuokraajan Azure Active Directory pyydettäessä.
3. Ponnahdusikkuna, valitse Lähetä Azure pinon palvelun järjestelmänvalvojan tunnistetietoja.

Koko käyttöönotto voi kestää joitakin erittäin käytetään Azure pinon POCs-15 ja 45 minuuttia välillä.

### <a name="update-the-local-dns"></a>Päivitä paikalliset DNS

1. Valitse **Rekisteröi-Microsoft.MySQL-fqdn.ps1** -välilehti ja Suorita komentosarja.
2. Azure Active Directory vuokraajan tunnuksen pyydettäessä syötteen Azure Active Directory vuokraajan täydellinen toimialuenimi: esimerkiksi **microsoftazurestack.onmicrosoft.com**.

### <a name="register-the-sql-rp-resource-provider"></a>Rekisteröi SQL RP resurssin tarjoajaan

1. Valitse **Rekisteröi-Microsoft.My-provider.ps1** -välilehti ja Suorita komentosarja.

2. Kun pyydetään tunnuksia, käytä merkille **basicAuthUserName** ja **basicAuthPassword** parametreiksi.

## <a name="verify-the-deployment-using-the-azure-stack-portal"></a>Tarkista Azure pinon portaalissa käyttöönotto

1. Kirjaudu ulos ClientVM ja kirjaudu sisään uudelleen nimellä **AzureStack\User**.

2. Työpöydällä Valitse **Azure pinon Käsitteiden Portal** ja kirjaudu sisään-portaaliin järjestelmänvalvojana palvelu

3. Varmista, että käyttöönotto onnistui. Valitsemalla **Selaa** &gt; **Resurssiryhmät**, valitse käyttämäsi resurssiryhmä (oletusarvo on **MySQLRP**), ja varmista, että sivu (yläosassa) essentials-osa lukee **käyttöönoton onnistui**.


4. Varmista, että rekisteröinti onnistui. Valitse **Selaa** &gt; **resurssin tarjoajat**ja Etsi **MySQL paikallinen**.

## <a name="create-your-first-mysql-database-to-test-your-deployment"></a>Jos haluat testata käyttöönoton ensimmäisen MySQL-tietokannan luominen

1. Kirjaudu Azure pinon Käsitteiden portaaliin palvelu.

2. Valitse **+** painike &gt; **Mukautettu** &gt; **MySQL-palvelin ja tietokantojen**.

3. Täytä tietokannan tiedot.

    **Muistiin kirjoittamasi "palvelimen nimi".** Tietokannan yhteydet-merkkijono on "palvelimen nimi" käyttäjänimi osana: esimerkiksi ** "user@ <ServerName>"**. Sinun on syötetty käyttäjänimi tässä muodossa, kun muodostat yhteyden tietokannan: esimerkiksi, kun otat käyttöön MySQL-sivuston Azure sivuston resurssi-palvelun avulla


## <a name="next-steps"></a>Seuraavat vaiheet

Kokeile muita [PaaS palvelut](azure-stack-tools-paas-services.md) , kuten [SQL Server resurssin tarjoajaan](azure-stack-sql-rp-deploy-short.md) ja [verkkosovelluksissa resurssin toimittaja](azure-stack-webapps-deploy.md).
