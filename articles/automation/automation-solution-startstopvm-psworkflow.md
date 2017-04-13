<properties 
    pageTitle="Käynnistäminen ja lopettaminen näennäiskoneiden Azure automaatio - PowerShell-työnkulun avulla | Microsoft Azure"
    description="Graafinen versio, mukaan lukien runbooks aloittaa tai lopettaa perinteinen näennäiskoneiden Azure automaatio-skenaario."
    services="automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="jwhit"
    editor="tysonn" />
<tags 
    ms.service="automation"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="07/06/2016"
    ms.author="bwren" />

# <a name="azure-automation-scenario---starting-and-stopping-virtual-machines"></a>Azure automaatio-skenaario - käynnistäminen ja näennäiskoneiden lopettaminen

Voit aloittaa ja lopettaa perinteinen näennäiskoneiden runbooks tässä Azure automaatio skenaariossa.  Tässä skenaariossa voit käyttää seuraavia toimia:  

- Käyttää sellaisenaan runbooks oman ympäristössä. 
- Muokkaa runbooks mukautettujen toimintojen suorittamiseen.  
- Kutsua toisen runbookin osana yleistä ratkaisu runbooks. 
- Määritä runbooks opetusohjelmat lisätietoja authoring käsitteitä runbookin. 

> [AZURE.SELECTOR]
- [Graafinen](automation-solution-startstopvm-graphical.md)
- [PowerShell-työnkulku](automation-solution-startstopvm-psworkflow.md)

Tämä on skenaario PowerShell työnkulun runbookin-versiota. Se on saatavilla myös käyttämällä [Graafinen runbooks](automation-solution-startstopvm-graphical.md).

## <a name="getting-the-scenario"></a>Aloittaminen skenaario

Tässä skenaariossa koostuu kahdesta PowerShell työnkulun runbooks, jonka voi ladata seuraavista linkeistä.  Tarkista tässä skenaariossa graafinen runbooks linkkejä [Graafinen versio](automation-solution-startstopvm-graphical.md) .

| Runbookin | Linkki | Tyyppi | Kuvaus |
|:---|:---|:---|:---|
| Käynnistä AzureVMs | [Käynnistä Azure perinteinen VMs](https://gallery.technet.microsoft.com/Start-Azure-Classic-VMs-86ef746b) | PowerShell-työnkulku | Aloittaa kaikki perinteinen näennäiskoneiden Azure subscriptionor kaikki näennäiskoneiden tietyn palvelun nimellä. |
| Lopeta AzureVMs | [Lopeta Azure perinteinen VMs](https://gallery.technet.microsoft.com/Stop-Azure-Classic-VMs-7a4ae43e) | PowerShell-työnkulku | Lopettaa kaikki näennäiskoneiden automaatio-tilin tai kaikki näennäiskoneiden tietyn palvelun nimellä.  |


## <a name="installing-and-configuring-the-scenario"></a>Asentaminen ja määrittäminen skenaario

### <a name="1-install-the-runbooks"></a>1. runbooks asentaminen

Lataa runbooks, voit tuoda ne ohjeiden tuonnissa [Runbookin](http://msdn.microsoft.com/library/dn643637.aspx#ImportRunbook).

### <a name="2-review-the-description-and-requirements"></a>2. Tarkista kuvaus ja vaatimukset
Runbooks ovat on kommentit ohjeteksti, joka sisältää kuvaus ja tarvittavat resurssit.  Saat myös samat tiedot tästä artikkelista. 

### <a name="3-configure-assets"></a>3. resurssien määrittäminen
Runbooks edellyttää seuraavia kalusteet, joiden täytyy luoda ja lisätä haluamasi arvot.

| Kohteen tyyppi | Kohteiden nimi | Kuvaus |
|:---|:---|:---|:---|
| Tunnistetietojen | AzureCredential | Sisältää tilille, jolla on oikeus aloittaa ja lopettaa näennäiskoneiden Azure tilauksen tunnistetiedot.  Vaihtoehtoisesti voit määrittää **Lisää AzureAccount** tehtävän **tunnistetiedon** -parametrin toiseen tunnistetiedon resurssi. |
| Muuttujan | AzureSubscriptionId | Sisältää Tilaustunnus Azure tilauksen. |

## <a name="using-the-scenario"></a>Skenaarion avulla

### <a name="parameters"></a>Parametrit

Runbooks kunkin on seuraavat parametrit.  Anna arvot pakollinen parametreja ja voit halutessasi kirjoittaa arvoja muiden parametrien tarpeen mukaan.

| Parametri | Tyyppi | Pakollinen | Kuvaus |
|:---|:---|:---|:---|
| Palvelun nimi | merkkijono | Ei | Jos arvo on annettu, kaikki kyseisen palvelun nimellä näennäiskoneiden on aloittaa tai pysäyttää.  Jos arvoa ei ole annettu, kaikki perinteinen näennäiskoneiden Azure tilauksen on aloittaa tai pysäyttää. |
| AzureSubscriptionIdAssetName | merkkijono | Ei | Sisältää [muuttujan resurssi](#installing-and-configuring-the-scenario) , joka sisältää Tilaustunnus Azure tilauksen nimi.  Jos et määritä arvo, *AzureSubscriptionId* käytetään.  |
| AzureCredentialAssetName | merkkijono | Ei | Sisältää [tunnistetiedon resurssi](#installing-and-configuring-the-scenario) , joka sisältää käyttämään runbookin tunnistetietojen nimen.  Jos et määritä arvo, *AzureCredential* käytetään.  |

### <a name="starting-the-runbooks"></a>Aloitus runbooks

Voit käyttää tavoilla aloittamisen [runbookin Azure automaatio-](automation-starting-a-runbook.md) käynnistämiseen joko runbooks tässä skenaariossa.

Esimerkki seuraavat komennot suoritetaan **StartAzureVMs** aloittaaksesi kaikki näennäiskoneiden palvelunimi *MyVMService*käyttämällä Windows PowerShell.

    $params = @{"ServiceName"="MyVMService"}
    Start-AzureAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Start-AzureVMs" –Parameters $params

### <a name="output"></a>Tulos

Runbooks on [tulosteen viestin](automation-runbook-output-and-messages.md) kunkin virtuaalikoneen, joka osoittaa, voit aloittaa tai lopettaa käsky lähetettiin onnistuneesti vai ei.  Voit etsiä tietyn merkkijonon kunkin runbookin tuloksen tulosteessa.  Seuraavassa taulukossa on lueteltu mahdollista tuloste merkkijonoja.

| Runbookin | Ehto | Viestin |
|:---|:---|:---|
| Käynnistä AzureVMs | Virtuaalikoneen on jo käytössä  | MyVM on jo käytössä |
| Käynnistä AzureVMs | Aloita pyyntö lähetettiin virtuaalikoneen | MyVM aloitettu |
| Käynnistä AzureVMs | Käynnistä-pyyntö virtuaalikoneen epäonnistui  | MyVM ei voitu käynnistää |
| Lopeta AzureVMs | Virtuaalikoneen jo pysäytetään  | MyVM jo pysäytetään |
| Lopeta AzureVMs | Pyyntö lähetettiin virtuaalikoneen lopettaminen | MyVM on keskeytetty |
| Lopeta AzureVMs | Lopeta-pyyntö virtuaalikoneen epäonnistui  | MyVM lopettaminen epäonnistui |

Esimerkiksi seuraavat koodikatkelman runbookin-yrittää käynnistää kaikki näennäiskoneiden palvelun nimellä *MyServiceName*.  Jos jokin Käynnistä pyynnöt epäonnistuvat voi ottaa virhe toiminnot. 

    $results = Start-AzureVMs -ServiceName "MyServiceName"
    foreach ($result in $results) {
        if ($result -like "* has been started" ) {
            # Action to take in case of success.
        }
        else {
            # Action to take in case of error.
        }
    }


## <a name="detailed-breakdown"></a>Erittelyn

Seuraavassa on tässä skenaariossa runbooks erittelyn.  Tämän toiminnon avulla voit mukauttaa runbooks tai juuri ja opi niiltä authoring automaatio-tekemät.

### <a name="parameters"></a>Parametrit

    param (
        [Parameter(Mandatory=$false)] 
        [String]  $AzureCredentialAssetName = 'AzureCredential',
        
        [Parameter(Mandatory=$false)]
        [String] $AzureSubscriptionIdAssetName = 'AzureSubscriptionId',

        [Parameter(Mandatory=$false)] 
        [String] $ServiceName
    )

Työnkulku käynnistyy hakemalla arvot [Syöteparametrit](#using-the-scenario).  Jos kohteiden nimiä ei anneta oletusnimet käytetään.

### <a name="output"></a>Tulos

    # Returns strings with status messages
    [OutputType([String])]

Tämä rivi ilmoittaa olevan: n runbookin tulosteen merkkijonon.  Tämä ei tarvita, mutta se on parhaat käytännöt, kun n runbookin käytetään [lapsen runbookin](automation-child-runbooks.md) niin, että ylemmän tason runbookin tietävät toiminta Tulostustyyppi.

### <a name="authentication"></a>Todennus

    # Connect to Azure and select the subscription to work against
    $Cred = Get-AutomationPSCredential -Name $AzureCredentialAssetName
    $null = Add-AzureAccount -Credential $Cred -ErrorAction Stop
    $SubId = Get-AutomationVariable -Name $AzureSubscriptionIdAssetName
    $null = Select-AzureSubscription -SubscriptionId $SubId -ErrorAction Stop

Seuraava rivit Määritä [tunnistetiedot](automation-configuring.md#configuring-authentication-to-azure-resources) ja Azure tilauksen, jota käytetään muiden: n runbookin.
Ensin Käytämme **Get-AutomationPSCredential** saat resurssi, jolla on pääsy aloittaa ja lopettaa näennäiskoneiden Azure tilauksen tunnistetiedot. **Lisää AzureAccount** käyttää sitten kohteen tunnistetietojen määrittäminen.  Tulos on määritetty tyhjä muuttujan niin, että se ei ole runbookin tulos.  

Tilauksen tunnus muuttujan resurssi haetaan **Get-AutomationVariable** ja **Valitse AzureSubscription**määrityksessä tilaus.

### <a name="get-vms"></a>Hae VMs

    # If there is a specific cloud service, then get all VMs in the service,
    # otherwise get all VMs in the subscription.
    if ($ServiceName) 
    { 
        $VMs = Get-AzureVM -ServiceName $ServiceName
    }
    else 
    { 
        $VMs = Get-AzureVM
    }

**Hae AzureVM** käytetään hakemiseen: n runbookin toimivat näennäiskoneiden.  Jos arvo on annettu **palvelun nimi** syötteen muuttuja, vain kyseisen palvelun nimellä näennäiskoneiden noudetaan.  Jos **palvelun nimi** on tyhjä, kaikki näennäiskoneiden noudetaan.

### <a name="startstop-virtual-machines-and-send-output"></a>Aloita/Lopeta näennäiskoneiden ja Lähetä tulostus

    # Start each of the stopped VMs
    foreach ($VM in $VMs)
    {
        if ($VM.PowerState -eq "Started")
        {
            # The VM is already started, so send notice
            Write-Output ($VM.InstanceName + " is already running")
        }
        else
        {
            # The VM needs to be started
            $StartRtn = Start-AzureVM -Name $VM.Name -ServiceName $VM.ServiceName -ErrorAction Continue

            if ($StartRtn.OperationStatus -ne 'Succeeded')
            {
                # The VM failed to start, so send notice
                Write-Output ($VM.InstanceName + " failed to start")
            }
            else
            {
                # The VM started, so send notice
                Write-Output ($VM.InstanceName + " has been started")
            }
        }
    }

Seuraava rivit käydä läpi kunkin virtuaalikoneen.  Ensin virtuaalikoneen **PowerState** on valittuna nähdäksesi, jos se on jo käytössä tai keskeyttää,: n runbookin mukaan.  Jos se on jo kohde-tilassa, viesti on lähetetty tulostus ja runbookin päättyy.  Jos et valitse **Käynnistä AzureVM** tai **Pysäytä AzureVM** käytetään yrittää käynnistää tai pysäyttää virtuaalikoneen tallennettu muuttujan pyynnön tuloksella.  Viesti on lähetetty Valitse siirrettävät pyynnön aloittaminen tai lopettaminen on lähetetty, joka määrittää.


## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja lapsen runbooks käyttämisestä on artikkelissa [ali runbooks Azure automaatio-](automation-child-runbooks.md) 
- Saat lisätietoja tulosteen viestien runbookin suorittamisen ja kirjaaminen ongelmien aikana, katso [Runbookin tulostus ja Azure Automaattiset viestien](automation-runbook-output-and-messages.md)
