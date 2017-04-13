<properties
    pageTitle="Käytä MySQL-tietokantojen PaaS Azure pinossa | Microsoft Azure"
    description="Tietoja pikatoiminnot käyttöönotto MySQL resurssin tarjoajaan ja anna MySQL palveluna Azure pinossa."
    services="azure-stack"
    documentationCenter=""
    authors="Dumagar"
    manager="bradleyb"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="dumagar"/>

# <a name="use-mysql-databases-as-paas-on-azure-stack"></a>Käytä PaaS Azure pinossa MySQL-tietokannat

> [AZURE.NOTE] Seuraavat tiedot koskevat vain Azure pinon TP1 ominaisuuksissa.

Voit ottaa käyttöön MySQL resurssin palveluntarjoaja Azure pinossa. Kun resurssin tarjoajaan otetaan käyttöön, voit luoda MySQL-palvelimia ja tietokantoja Azure Resurssienhallinta käyttöönoton mallien ja antaa MySQL-tietokantojen palveluna. MySQL-tietokantoja, joita löytyviä sivustoista, tukevat monia sivuston ympäristöissä. Sen jälkeen, kun resurssin tarjoajaan otetaan käyttöön, voit luoda WordPress sivustojen Azure Web Apps-ympäristö kuin service (PaaS)-lisäosa Azure pinon varten.

## <a name="quick-steps-to-deploy-the-resource-provider"></a>Vaiheittaiset ohjeet ottamaan resurssin tarjoajaan
Noudattamalla näitä ohjeita, jos ole vielä tutustunut Azure pinon. Jos haluat lisätietoja, jokaisen osan linkeistä tai siirtyä suoraan [käyttöönotto-Azure pinon Käsitteiden MySQL tietokannan resurssin tarjoajan sovittimen](azure-stack-mysql-rp-deploy-long.md).

1.  Varmista, että [kaikki määritysvaiheet loppuun](azure-stack-mysql-rp-deploy-long.md#set-up-steps-before-you-deploy) ennen niiden käyttöönottoa resurssi-palvelu:

    - 3,5 .NET framework on jo määritetty Windows Server kuvaa. (Jos latasit Azure pinon bittien jälkeen helmikuu, 23, 2016, voit ohittaa tämän vaiheen.)
    - [PowerShellin Azure, joka on yhteensopiva Azure pinon versio on asennettu](http://aka.ms/azStackPsh).
    - Valitse Internet Explorerin suojausasetukset ClientVM [Internet Explorerin parannettu suojaus on poistettu käytöstä ja evästeet on otettu](azure-stack-mysql-rp-deploy-long.md#Turn-off-IE-enhanced-security-and-enable-cookies)käyttöön.

2. [Lataa MySQL resurssin tarjoajan binaaritiedostot tiedosto](http://aka.ms/masmysqlrp) ja Pura se Azure-pinon todiste käsitteiden vahvistamisen ClientVM käyttöön.

3. [Suorita bootstrap.cmd ja komentosarjat](azure-stack-mysql-rp-deploy-long.md#Bootstrap-the-resource-provider-deployment-PowerShell-and-Prepare-for-deployment).

    Komentosarjojen joukko, jotka on ryhmitelty kaksi tärkeintä välilehteä Avaa-PowerShell integroitu komentosarjat ympäristössä (ise:). Suorita ladata komentosarjoja järjestyksessä vasemmalta oikealle välilehtien.

    1. Voit suorittaa **Valmistele** -välilehdessä komentosarjoja vasemmalta oikealle:

        - Yleismerkkien varmenteen suojaamiseen resurssin tarjoajaan ja Azure Resurssienhallinta välisen luominen.
        - Hyväksy MySQL Käyttöoikeussopimuksen ehtoja ja lataa MySQL-binaaritiedostoja.
        - Lataa varmenteet ja kaikki muut palvelutiedot Azure pinon tallennustilan tilin.
        - Valikoima-pakettien julkaiseminen niin, että MySQL resurssien valikoiman avulla voit ottaa käyttöön.

        > [AZURE.IMPORTANT] Jos jokin komentosarjat jumittua ei jostakin syystä, kun olet lähettänyt Azure Active Directory-vuokraajan, tietoturva-asetukset voivat estää DLL, joka on pakollinen suorittaa käyttöönottamiseen. Voit ratkaista ongelman, Etsi Microsoft.AzureStack.Deployment.Telemetry.Dll resurssin tarjoajan kansiossa, hiiren kakkospainikkeella, valitse **Ominaisuudet**ja valitse **Yleiset** -välilehdessä **Salli** .

    2. Voit suorittaa **käyttöönotto** -välilehdessä komentosarjoja vasemmalta oikealle:

        - [Ota käyttöön virtual machine (AM)](azure-stack-mysql-rp-deploy-long.md#Deploy-the-MySQLResource-Provider-VM) , joka isännöi sekä resurssin tarjoajaan, MySQL-palvelimet että tietokannoissa, jotka vahvistaa. Tämä komentosarja viittaa JSON parametrin tiedosto, joka on päivitettävä joitakin arvoilla, ennen kuin suoritat komentosarjan.
        - [Rekisteröi paikallinen DNS-tietue](azure-stack-mysql-rp-deploy-long.md#Update-the-local-DNS) , joka yhdistetään resurssin palveluntarjoajan AM.
        - [Rekisteröi resurssi-palveluntarjoajan](azure-stack-mysql-rp-deploy-long.md#Register-the-MySQL-RP-Resource-Provider) kanssa paikallisen Azure Resurssienhallinta.

        > [AZURE.IMPORTANT] Kaikki komentosarjat oletetaan, että käyttöjärjestelmän kuva täyttää edellytykset (.NET 3.5 asennettu, JavaScript ja evästeet käytössä ClientVM ja PowerShellin Azure asennettu uusin versio). Jos saat virheitä, kun suoritat komentosarjat, varmista täyty edellytykset.

5. Voit [testata palveluntarjoajan uusi MySQL-resurssi](/azure-stack-MySql-rp-deploy-long.md#create-your-first-mysql-database-to=test-your-deployment)-käyttöönotto Azure pino-portaalista MySQL-tietokantaan. Valitse **Luo** &gt; **Mukautettu** &gt; **MySQL-palvelin ja tietokanta**.

Hanki MySQL resurssi-palvelun määrittäminen ja suorittamalla: noin 25 minuuttia.
