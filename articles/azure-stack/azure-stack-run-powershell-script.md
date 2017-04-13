<properties
    pageTitle="Azure pinon Käsitteiden käyttöönotto | Microsoft Azure"
    description="Lue, miten Azure pinon Käsitteiden valmistelemisesta ja suorittamisesta PowerShell-komentosarja Azure-pino käsitteiden."
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/20/2016"
    ms.author="erikje"/>

# <a name="deploy-azure-stack-poc"></a>Azure pinon Käsitteiden käyttöönotto
Ottaa käyttöön Azure pinon Käsitteiden, sinun on [valmisteleminen käyttöönoton koneen](#prepare-the-deployment-machine) ja [Suorita PowerShell-käyttöönoton komentosarja](#run-the-powershell-deployment-script).

## <a name="download-and-extract-microsoft-azure-stack-poc-tp2"></a>Lataa ja purkaa Microsoft Azure pinon Käsitteiden TP2

Ennen kuin aloitat, varmista, että olet vähintään 85 Gigatavua tilaa.

1. Azure pinon Käsitteiden TP2 Lataa koostuu zip-tiedoston, joka sisältää seuraavat 12 tiedostot, yhteensä noin 20 gt:
    - 1 MicrosoftAzureStackPOC.EXE
2. Tarkista Käyttöoikeussopimus-näyttö ja Self-Extractor ohjatun toiminnon tiedot ja valitse sitten **Seuraava**.
3. Tarkista tietosuojatiedot näyttö ja Self-Extractor ohjatun toiminnon tiedot ja valitse sitten **Seuraava**.
4. Valitse kohde-tiedostojen purkaa, valitse **Seuraava**.
    - Oletusarvo on: <drive letter>:\<nykyisen kansion > \Microsoft Azure pinon Käsitteiden
5. Tarkista kohteen sijainti näytössä ja Self-Extractor ohjatun toiminnon tiedot ja valitse sitten Pura CloudBuilder.vhdx (~44.5 gt) ja ThirdPartyLicenses.rtf tiedostojen **Pura** .

> [AZURE.NOTE] Kun olet poiminut tiedostoja, voit poistaa palauttamaan tilaa tietokoneeseen zip-tiedosto. Vaihtoehtoisesti voit siirtää zip-tiedosto toiseen sijaintiin niin, että jos haluat käyttöön, sinun ei tarvitse zip-tiedostojen lataaminen uudelleen.

## <a name="prepare-the-deployment-machine"></a>Valmistele käyttöönoton koneen

1. Varmista, että voit fyysisesti muodostaa yhteyttä tietokoneeseen käyttöönoton tai fyysinen konsoli käyttää (kuten KVM). Sinun on pääsyä, kun käynnistät käyttöönoton tietokoneen vaiheessa 9.

2. Varmista, että koneen käyttöönoton täyttää [vähimmäisvaatimukset](azure-stack-deploy.md). Voit käyttää [Käyttöönoton tarkistaminen Azure pinon tekninen esikatselu 2](https://gallery.technet.microsoft.com/Deployment-Checker-for-50e0f51b) Vahvista tarpeen.

3. Kirjaudu sisään nimellä paikallisen järjestelmänvalvojan Käsitteiden tietokoneeseesi.

4. Kopioi C:\CloudBuilder.vhdx CloudBuilder.vhdx-tiedosto.

    > [AZURE.NOTE] Jos et halua suositellut komentosarjan avulla voit laatia Käsitteiden isäntätietokoneen (vaiheet 5 – vaihe 7), älä kirjoita mitään käyttöoikeus-näppäintä painettuna aktivointi-sivu. Windows Server 2016 kuva kokeiluversion sisältyy ja kirjoittamalla käyttöoikeuden näppäintä aiheuttaa vanheneminen varoituksia.

5. Käsitteiden tietokoneessa Suorita lataamaan Azure pinon TP2 tukitiedostot seuraavaa PowerShell-komentosarjaa:

    ```powershell
    # Variables
    $Uri = 'https://raw.githubusercontent.com/Azure/AzureStack-Tools/master/Deployment/'
    $LocalPath = 'c:\AzureStack_TP2_SupportFiles'

    # Create folder
    New-Item $LocalPath -type directory

    # Download files
    ( 'BootMenuNoKVM.ps1', 'PrepareBootFromVHD.ps1', 'Unattend.xml', 'unattend_NoKVM.xml') | foreach { Invoke-WebRequest ($uri + $_) -OutFile ($LocalPath + '\' + $_) } 
    ```

    Tämä komentosarja Lataa Azure pinon TP2 tukitiedostot $LocalPath-parametrin määrittämän kansioon.
    
6. Avaa laajennettuja PowerShell-konsolin ja Vaihda kansio, johon kopioit tiedostot.
    - 11 MicrosoftAzureStackPOC-N.BIN (jossa N on 1-11)
7. Napsauta hiiren kakkospainikkeella MicrosoftAzureStackPOC.EXE > Suorita järjestelmänvalvojana.

8. PrepareBootFromVHD.ps1-komentosarjan suorittaminen Tämä komentosarja ja automaattisia tiedostoja ovat käytettävissä muiden tuki komentosarjojen, sekä tämän koontiversion annettu kanssa.
    On viisi parametrit PowerShell-komentosarjaa:
    - CloudBuilderDiskPath (pakollinen) – isännän CloudBuilder.vhdx polku.
    - DriverPath (valinnainen) – voit lisätä ohjaimia virtual HD isännän.
    - (Valinnainen) – ApplyUnattend Määritä valitsin parametria voit automatisoida käyttöjärjestelmän määrittämisen. Jos määritetty, käyttäjän on määritettävä AdminPassword määrittäminen käyttöjärjestelmän käynnistyksen (edellyttää edellyttäen mukana tiedoston unattend_NoKVM.xml).
    Jos et käytä parametriä, yleinen unattend.xml-tiedoston käytetään ilman edelleen mukauttaminen. Tarvitset KVM valmis mukauttamiseen, kun se käynnistyy.
    - AdminPassword (valinnainen) – käyttää vain, kun ApplyUnattend-parametri on määritetty, edellyttää vähintään kuusi merkkiä.
    - (Valinnainen) VHDLanguage – määrittää Näennäiskiintolevyn kielen, oletusarvo-en-US".
    Komentosarja on kuvattu, ja se sisältää Esimerkki käyttöä, vaikka yleisimmät käyttö on:
    
        `.\PrepareBootFromVHD.ps1 -CloudBuilderDiskPath C:\CloudBuilder.vhdx -ApplyUnattend`
    
        Jos suoritat tarkka komento, sinun on annettava AdminPassword tulee näkyviin.

9. Kun komentosarja on valmis, sinun on vahvistettava uudelleenkäynnistyksen. Jos määritettynä on muita käyttäjiä on kirjautunut sisään, tämä komento epäonnistuu. Jos komento epäonnistuu, suorita seuraava komento:`Restart-Computer -force` 

10. ISÄNNÄN käynnistetään uudelleen CloudBuilder.vhdx, jolla käyttöönoton jatkuu OS kyselyjä.

> [AZURE.IMPORTANT] Azure pinon tarvitaan Internet-yhteys joko suoraan tai läpinäkyvä välityspalvelimen kautta. TP2 Käsitteiden käyttöönoton tukee täsmälleen yhden NIC tuomasta. Jos sinulla on useita NIC, varmista, että vain yksi on otettu käyttöön (ja kaikki muut on poistettu käytöstä) ennen kuin suoritat seuraavan osion käyttöönoton komentosarja.

## <a name="run-the-powershell-deployment-script"></a>Suorita käyttöönoton PowerShell-komentosarja

1. Kirjaudu sisään nimellä paikallisen järjestelmänvalvojan Käsitteiden tietokoneeseesi. Käytä edellisessä vaiheessa määritetyt tunnistetiedot.

2. Avaa laajennettuja PowerShell-konsolin.

3. Suorita PowerShell-komennon:`cd C:\CloudDeployment\Configuration`

4. Suorita käyttöönotto-komento:`.\InstallAzureStackPOC.ps1`

5. **Enter salasana** tulee näkyviin Kirjoita salasana ja vahvista se. Tämä on kaikki näennäiskoneiden salasana. Muista tallentaa sen.

6. Kirjoita Azure Active Directory-tilin tunnistetiedot. Käyttäjän on oltava hakemiston vuokraajan yleisen järjestelmänvalvojan.

7. Käyttöönotto voi kestää muutaman tunnin aikana automaattisesti uudelleenkäynnistyksen kerran.

    > [AZURE.IMPORTANT] Jos haluat seurata käyttöönoton, kirjaudu azurestack\AzureStackAdmin. Jos olet kirjautunut sisään paikallisen järjestelmänvalvojaksi jälkeen tietokone on liitetty toimialueeseen, et näe käyttöönoton edistymisen. Älä suorita käyttöönoton, vaan Kirjaudu azurestack\AzureStackAdmin Vahvista, että se on käynnissä.

    Kun asennus on valmis, PowerShell console näyttää: **VALMIINA: toiminto "Käyttöönotto"**.

    Jos asennus epäonnistuu, voit kokeilla [sitä uudelleen](azure-stack-rerun-deploy.md). Voit myös [ottaa uudelleen](azure-stack-redeploy.md) sen alusta alkaen.

### <a name="deployment-script-examples"></a>Käyttöönoton komentosarja-esimerkkejä

Jos AAD henkilöllisyytesi on vain yksi AAD Directory liittyvät:

    cd C:\CloudDeployment\Configuration
    $adminpass = ConvertTo-SecureString "<LOCAL ADMIN PASSWORD>" -AsPlainText -Force
    $aadpass = ConvertTo-SecureString "<AAD GLOBAL ADMIN ACCOUNT PASSWORD>" -AsPlainText -Force
    $aadcred = New-Object System.Management.Automation.PSCredential ("<AAD GLOBAL ADMIN ACCOUNT>", $aadpass)
    .\InstallAzureStackPOC.ps1 -AdminPassword $adminpass -AADAdminCredential $aadcred

Jos AAD henkilöllisyytesi liittyy suurempi kuin AAD kansiossa:

    cd C:\CloudDeployment\Configuration
    $adminpass = ConvertTo-SecureString "<LOCAL ADMIN PASSWORD>" -AsPlainText -Force
    $aadpass = ConvertTo-SecureString "<AAD GLOBAL ADMIN ACCOUNT PASSWORD>" -AsPlainText -Force
    $aadcred = New-Object System.Management.Automation.PSCredential ("<AAD GLOBAL ADMIN ACCOUNT> example: user@AADDirName.onmicrosoft.com>", $aadpass)
    .\InstallAzureStackPOC.ps1 -AdminPassword $adminpass -AADAdminCredential $aadcred -AADDirectoryTenantName "<SPECIFIC AAD DIRECTORY example: AADDirName.onmicrosoft.com>"

Jos ympäristössä ei ole käytössä DHCP, on oltava seuraavat YLIMÄÄRÄISET parametrit johonkin (esimerkiksi käyttöoikeudet annettu) yläpuolella olevista asetuksista:

    .\InstallAzureStackPOC.ps1 -AdminPassword $adminpass -AADAdminCredential $aadcred
    -NatIPv4Subnet 10.10.10.0/24 -NatIPv4Address 10.10.10.3 -NatIPv4DefaultGateway 10.10.10.1


### <a name="installazurestackpocps1-optional-parameters"></a>InstallAzureStackPOC.ps1 valinnaisten parametrien

| Parametri | Pakollinen/Valinnainen | Kuvaus |
| --------- | ----------------- | ----------- |
| AADAdminCredential | Valinnainen | Määrittää Azure Active Directory-käyttäjänimi ja salasana. Näitä Azure tunnistetietoja voi olla Org-ID- tai Microsoft-Account. Voit käyttää Microsoft-Account tunnistetietoja, Ohita tämä parametri cmdlet. Tämä parametri pois pyytää Azure todennus-valikko käyttöönoton aikana. Tämä luo käyttöönoton käytettyä todennus ja Päivitä tunnukset. |
| AADDirectoryTenantName | Pakollinen | Määrittää vuokraajan kansion. Tämä parametri avulla voit määrittää tiettyyn kansioon, jossa AAD-tilillä on useita kansioiden käyttöoikeuksia. Koko nimi AAD hakemisto-vuokraajan muoto <directoryName>. onmicrosoft.com. |
| AdminPassword | Pakollinen | Määrittää paikallisen järjestelmänvalvojatilin ja kaikki muut käyttäjätilit kaikki luotu Käsitteiden käyttöönoton osana näennäiskoneiden. Salasana on vastattava nykyisen paikallisen järjestelmänvalvojan salasanan, isännän. |
| AzureEnvironment | Valinnainen | Valitse Azure-ympäristössä, johon haluat rekisteröidä Azure pinon käyttöönottoon. Vaihtoehdot ovat *Julkisia Azure*- *Azure - Kiinan* *Azure - US Government*. |
| EnvironmentDNS | Valinnainen | DNS-palvelin luodaan Azure pinon käyttöönoton osana. Anna sisäpuolella ratkaisu nimien leiman ulkopuolella, on olemassa oleva infrastruktuuri DNS-palvelin. Leima-DNS-palvelin välittää Tuntematon nimi tarkkuus pyynnöt tähän palvelimeen. |
| NatIPv4Address | Pakollinen DHCP NAT-tuki | Määrittää MAS BGPNAT01 staattinen IP-osoite. Tämä parametri käyttää vain, jos DHCP voi määrittää kelvollinen IP-osoite Internet-yhteyden. |
| NatIPv4DefaultGateway | Pakollinen DHCP NAT-tuki | Määrittää oletusarvon yhdyskäytävän MAS BGPNAT01 käytettäviä staattinen IP-osoite. Tämä parametri käyttää vain, jos DHCP voi määrittää kelvollinen IP-osoite Internet-yhteyden.  |
| NatIPv4Subnet | Pakollinen DHCP NAT-tuki | IP-osoitteiden etuliite DHCP NAT tuki päälle. Tämä parametri käyttää vain, jos DHCP voi määrittää kelvollinen IP-osoite Internet-yhteyden.  |
| PublicVLan | Valinnainen | Määrittää VLAN ID-tunnuksellasi. Tämä parametri käyttää vain, jos isännän ja MAS BGPNAT01 on määritettävä VLAN-tunnus fyysinen verkon (ja Internet). Esimerkiksi`.\InstallAzureStackPOC.ps1 –Verbose –PublicVLan 305` |
| Suorita | Valinnainen | Käytä tämän merkinnän uudelleen käyttöönoton.  Kaikki edellisen syötteen käytetään. Kirjoita tietoja uudelleen aiemmin ei tueta, koska useita yksilölliset arvot luodaan ja käytetään käyttöönottamiseen. |
| TimeServer | Valinnainen | Käytä tätä parametria, jos haluat määrittää tietyn ajanjakson palvelimen. |

## <a name="next-steps"></a>Seuraavat vaiheet

[Yhteyden muodostaminen Azure pino](azure-stack-connect-azure-stack.md)
