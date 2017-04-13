<properties
    pageTitle="Jatkuva ja ryhmän-palveluita käyttämällä Azure resurssiryhmä projektit-integroinnin | Microsoft Azure"
    description="Tässä artikkelissa käsitellään muodostaa jatkuva Visual Studio Team Services-integroinnin avulla Azure resurssiryhmä käyttöönottoprojektien Visual Studiossa."
    services="visual-studio-online"
    documentationCenter="na"
    authors="mlearned"
    manager="erickson-doug"
    editor="" />

 <tags
    ms.service="azure-resource-manager"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/01/2016"
    ms.author="mlearned" />

# <a name="continuous-integration-in-visual-studio-team-services-using-azure-resource-group-deployment-projects"></a>Jatkuva integrointi Visual Studio Team Servicesissä käyttämällä Azure resurssiryhmä käyttöönoton projektit

Ottamaan Azuren mallin, sinun täytyy suorittaa tehtäviä voit siirtyä eri vaiheiden: Muodosta-testi, kopioi Azure (kutsutaan myös "Väliaikaisen") ja ota käyttöön-malli.  On kahdella eri tavalla, voit tehdä tämän Visual Studio Team Services (ja ryhmän palvelut). Molemmista menetelmistä on samat tulokset, joten valitse haluamasi vaihtoehto, joka vastaa parhaiten työnkulun.

-   Lisää yhdellä kertaa koontiversion määrityksen, joka suoritetaan PowerShell-komentosarja, joka sisältyy Azure resurssiryhmä käyttöönotto-projektin (AzureResourceGroup.ps1 käyttöönotto). Komentosarja kopioi palvelutiedot ja ottaa sitten mallin käyttöön.
-   Lisää ja ryhmän palveluihin muodostaa vaiheet jokaisen uuden vaiheen tehtävän suorittamiseen.

Tässä artikkelissa esitellään käyttämisestä ensimmäinen vaihtoehto (Käytä koontiversion määrityksen Suorita PowerShell-komentosarjaa). Tämä vaihtoehto etu siitä, että käytetty sovelluskehittäjät Visual Studiossa komentosarja on saman komentosarjan, jota käytetään ryhmän ja-palvelut. Tässä toimintosarjassa oletetaan, että sinulla on jo Visual Studio käyttöönottoprojektin kuitata sisään ja Team Services.

## <a name="copy-artifacts-to-azure"></a>Kopioi palvelutiedot Azure 

Jos sinulla on kaikki tiedot, joita tarvitaan mallin käyttöönottoa varten sinun on riippumatta skenaariota, voit antaa heille Azure Resurssienhallinta. Näitä tietoja voi olla tiedostot seuraavasti:

-   Sisäkkäisiä malleja
-   Määrityksessä ja DSC komentosarjoja
-   Sovelluksen binaaritiedostot

### <a name="nested-templates-and-configuration-scripts"></a>Sisäkkäisiä malleja ja määrityksessä
Kun käytät Visual Studio myöntämä mallit (tai Visual Studio katkelmat luotu) PowerShell-komentosarjaa vaiheisiin ei vain tietoja, myös parameterizes eri versioiden resurssien URI. Komentosarja ja valitse kopioi palvelutiedot suojatun säilön Azure-tietokannassa, Luo SaS-tunnuksen kyseisen säilön ja välittää tietoja voin mallin käyttöönottoa. Katso lisätietoja sisäkkäisiä malleja [luominen mallin käyttöönottoa](https://msdn.microsoft.com/library/azure/dn790564.aspx) .

## <a name="set-up-continuous-deployment-in-vs-team-services"></a>Jatkuva käyttöönoton ja työryhmän Services-palveluissa asetusten määrittäminen

Soita PowerShell-komentosarjaa ja Team Services, haluat päivittää koontiversion määrityksen. Lyhyesti vaiheet ovat: 

1.  Muodosta kuvauksen muokkaaminen.
1.  Määritä Azure todennus ja ryhmän Services-palveluissa.
1.  Lisää PowerShellin Azure muodosta vaiheeseen, joka viittaa Azure resurssiryhmä käyttöönotto-projektin PowerShell-komentosarjaa.
1.  Määritä projektin sisäisten ja Team Services-käyttöä varten *- ArtifactsStagingDirectory* -parametrin arvon.

### <a name="detailed-walkthrough"></a>Laskun

Seuraavat vaiheet edetään ohjatusti vaiheista määrittäminen jatkuva käyttöönoton ja työryhmän Services-palveluissa 

1.  Muokkaa ja Team Services koontiversion määrityksen ja lisää PowerShellin Azure-muodosta vaihe. Valitse koontiversion määrityksen **luominen määritykset** -luokassa, ja valitse sitten **Muokkaa** -linkkiä.

    ![][0]

1.  Lisää uusi **PowerShellin Azure** muodosta koontiversion määrityksen ja valitse sitten **Lisää luominen vaihe...** painike.

    ![][1]

1.  Valitse **Ota käyttöön tehtävän** luokan, valitse **PowerShellin Azure** tehtävä, ja valitse sitten sen **Lisää** -painiketta.

    ![][2]

1.  Valitse **PowerShellin Azure** muodosta vaihe ja täytä siihen arvot.

    1.  Jos sinulla on jo Azure Palvelupäätepisteen ja ryhmän palveluita lisätään, valitse ensin tilaus **Azure tilauksen** avattavassa luetteloruudussa ja siirry seuraavaan osioon. 

        Jos sinulla ei ole Azure Palvelupäätepisteen ja ryhmän Services-palveluissa, tarvitset lisätä sellaista. Tämä tästä kerrotaan prosessi. Jos Azure-tili on käytössä Microsoft-tilillä (kuten Hotmail), tarvitset saat palvelun lyhennys todennus seuraavasti.

    1.  Valitse **Azure tilauksen** **hallinta** valinnan avattava luetteloruutu.

        ![][3]

    1. Valitse **Uusi palvelupäätepiste** avattavassa luetteloruudussa **Azure** .

        ![][4]

    1.  Valitse **Lisää Azure tilaus** -valintaikkunassa **Palvelun lyhennys** -vaihtoehto.

        ![][5]

    1.  Azure Tilaustiedot lisääminen **Lisää Azure tilaus** -valintaikkuna. Sinun on annettava seuraavat tiedot:
        -   Tilauksen tunnus
        -   Tilauksen nimi
        -   Palvelun lyhennys tunnus
        -   Palvelun lyhennys avain
        -   Vuokraajan tunnus

    1.  Valittua nimen lisääminen **tilauksen** nimi-ruutuun. Tämä arvo näkyy myöhemmin **Azure tilauksen** avattavaa luetteloa ja ryhmän Services-palveluissa. 

    1.  Jos et tiedä Azure Tilaustunnus, voit jompikumpi seuraavista komennoista hankkiminen.
        
        PowerShell-komentosarjojen käyttää:

        `Get-AzureRmSubscription`

        Käytä Azure CLI:

        `azure account show`
    

    1.  Saat palvelun lyhennys tunnus-palvelun lyhennys-näppäintä painettuna ja vuokraajan tunnus noudattamalla [luominen Active Directory-sovelluksen ja palvelun lyhennys käyttämällä portal](resource-group-create-service-principal-portal.md) tai [todennustapa palvelun lyhennys Azure resurssien hallinta](resource-group-authenticate-service-principal.md).

    1.  Palvelun lyhennys tunnus, palvelun lyhennys avain ja vuokraajan tunnus-arvojen lisääminen **Lisää Azure tilaus** -valintaikkunassa ja valitse sitten **OK** -painiketta.

        Nyt sinulla kelvollinen palvelun lyhennys käyttää Azure PowerShell-komentosarjaa.

1.  Muodosta kuvauksen muokkaaminen ja valitse **PowerShellin Azure** muodosta vaihe. Valitse tilaus **Azure tilauksen** avattavasta luetteloruudusta. (Jos tilaus ei näy, valitse **Päivitä** -painike seuraava **hallinta** -linkki). 

    ![][8]

1.  Anna käyttöönotto AzureResourceGroup.ps1 PowerShell-komentosarjaa polku. Voit tehdä tämän **Komentosarjapolku** -ruudun vieressä olevaa kolmea pistettä (…)-painiketta, siirry projektin **komentosarjat** -kansiossa käyttöönotto AzureResourceGroup.ps1 PowerShell-komentosarjaa, valitse se ja valitse sitten **OK** -painiketta. 

    ![][9]

1. Kun olet valinnut komentosarja, Päivitä polku komentosarjaa niin, että se toimii Build.StagingDirectory (samaan kansioon, joka on määritetty *ArtifactsLocation* ). Voit tehdä tämän lisäämällä "$(Build.StagingDirectory)/"Komentosarjapolku alkuun.

    ![][10]

1.  Kirjoita (viiva) seuraavien parametrien **Komentosarjan argumentit** -ruutuun. Kun suoritat komentosarja Visual Studiossa, näet, miten ja käyttää parametreja **kohde** -ikkunassa. Voit käyttää tätä pohjana määrittämisestä parametriarvot muodosta vaiheessa.

  	| Parametri | Kuvaus|
  	|---|---|
  	| -ResourceGroupLocation           | Missä resurssiryhmän on, kuten **eastus** tai **"Itä USA"**geo sijainti-arvo. (Lisää puolilainausmerkkeihin, jos nimessä on välilyönti.) Lisätietoja on kohdassa [Azure-alueita](https://azure.microsoft.com/en-us/regions/) .|                                                                                                                                                                                                                              |
  	| -ResourceGroupName               | Käyttää tätä käyttöönottoa varten resurssiryhmän nimi.|                                                                                                                                                                                                                                                                                                                                                                                                                |
  	| -UploadArtifacts                 | Tämä parametri, mikäli ilmaisee, että palvelutiedot on ladattava Azure paikalliseen järjestelmään. Sinun on määritettävä tätä valitsinta mallia käyttöönoton vaatiiko ylimääräisiä palvelutiedot, jonka haluat vaiheen avulla (esimerkiksi määrityksessä tai sisäkkäisiä malleja) PowerShell-komentosarjaa vain.                                                                                                                                                                 |
  	| -StorageAccountName              | Käytettävä käyttöönottoon vaiheen palvelutiedot tallennustilan tilin nimi. Tämä parametri on pakollinen vain, jos kopioit palvelutiedot Azure. Tallennustilan tällä tilillä ei luodaan automaattisesti käyttöympäristön mukaan, se on jo olemassa.|                                                                                                                                                                                                                     |
  	| -StorageAccountResourceGroupName | Tallennustilan-tiliin liittyvät resurssiryhmän nimi. Tämä parametri on pakollinen vain, jos kopioit palvelutiedot Azure.|                                                                                                                                                                                                                                                                                                                               |
  	| -TemplateFile                    | Azure resurssiryhmä käyttöönotto-projektin mallitiedoston polku. Voit parantaa joustavuutta, käytä parametrin, joka on sen sijaan, että absoluuttinen polku PowerShell-komentosarjaa sijainnin suhteessa polkua.|
  	| -TemplateParametersFile          | Azure resurssiryhmä käyttöönotto-projektin parametrit-tiedoston polku. Voit parantaa joustavuutta, käytä parametrin, joka on sen sijaan, että absoluuttinen polku PowerShell-komentosarjaa sijainnin suhteessa polkua.|
  	| -ArtifactStagingDirectory        | Tämä parametri avulla PowerShell komentosarja tietää kansio-minne projektin binaaritiedostoja kopioidaan. Tämä arvo ohittaa PowerShell-komentosarjojen käyttää oletusarvoa. JA Team Services käyttää, aseta arvoksi: - ArtifactStagingDirectory $(Build.StagingDirectory)                                                                                                                                                                                              |

    Seuraavassa on komentosarja argumentit Esimerkki (rivi jaettu luettavuuden):

    ``` 
    -ResourceGroupName 'MyGroup' -ResourceGroupLocation 'eastus' -TemplateFile '..\templates\azuredeploy.json' 
    -TemplateParametersFile '..\templates\azuredeploy.parameters.json' -UploadArtifacts -StorageAccountName 'mystorageacct' 
    –StorageAccountResourceGroupName 'Default-Storage-EastUS' -ArtifactStagingDirectory '$(Build.StagingDirectory)' 
    ```

    Kun olet valmis, **Komentosarjan argumentit** -ruutuun pitäisi näyttää seuraavankaltaiselta.

    ![][11]

1.  Kun olet lisännyt kaikki tarvittavat kohteet PowerShellin Azure luominen vaihe, valitse **jonon** luominen projektin Muodosta-painiketta. **Luo** -kuvassa on PowerShell-komentosarjaa tuloste.

## <a name="next-steps"></a>Seuraavat vaiheet

Lue lisätietoja Azure Resurssienhallinta ja Azure resurssiryhmät [Azure resurssin hallinnassa: yleiskatsaus](azure-resource-manager/resource-group-overview.md) .


[0]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough1.png
[1]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough2.png
[2]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough3.png
[3]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough4.png
[4]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough5.png
[5]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough6.png
[8]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough9.png
[9]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough10.png
[10]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough11b.png
[11]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough12.png
