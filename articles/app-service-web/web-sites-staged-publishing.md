<properties
    pageTitle="Väliaikaisen ympäristöissä Azure App palvelun web-sovellusten määrittäminen"
    description="Opettele käyttämään vaiheistettu julkaiseminen web Apps-sovellukset App Azure-palvelussa."
    services="app-service"
    documentationCenter=""
    authors="cephalin"
    writer="cephalin"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/09/2016"
    ms.author="cephalin"/>

# <a name="set-up-staging-environments-for-web-apps-in-azure-app-service"></a>Väliaikaisen ympäristöissä Azure App palvelun web-sovellusten määrittäminen
<a name="Overview"></a>

Kun otat käyttöön web app [Sovelluksen](http://go.microsoft.com/fwlink/?LinkId=529714)-palveluun, voit ottaa käyttöön erillinen käyttöönoton paikka sen sijaan, että oletusarvoinen tuotannon paikka **Vakio** - tai **Premium** App palvelun suunnittelu-tilassa. Käyttöönoton paikkojen ovat todella live web Apps-sovellusten kanssa omia isäntänimet. Web-sovelluksen sisältöä ja määritykset osia voi vaihtaa kaksi käyttöönoton paikkaa, kuten tuotannon paikan välillä. Käyttöönotto sovelluksesi käyttöönoton paikka on seuraavat edut:

- Voit tarkistaa väliaikaisen käyttöönoton paikka web app muutokset ennen vaihtaminen tuotannon paikka kanssa.

- Käyttöönoton verkkosovellukseen ensin paikka ja vaihtaminen kyselyjä tuotannon varmistaa, että kaikki esiintymät paikka on lämmitettävä ennen parhaillaan vaihtaa paikkaa kyselyjä tuotannon. Näin käyttökatkot, kun otat käyttöön web Appissa. Liikenne uudelleenohjaus on saumaton ja ei ole pyynnöt olevat kohteet poistetaan, Vaihda toimintojen tuloksena. Koko työnkulku voidaan automatisoida määrittämällä [Automaattisen Vaihda](#configure-auto-swap-for-your-web-app) edeltävän Vaihda vahvistus ei tarvita, kun.

- Jälkeen Vaihda aiemmin vaiheistettu web Appilla paikka on nyt edellisen tuotannon web Appissa. Jos vaihtaa paikkaa kyselyjä tuotannon paikka muutokset ovat ei vastaa odotuksiasi, voit tehdä saman Vaihda heti, jos haluat saada "viimeisen tunnetut hyvä sivuston" takaisin.

Sovelluksen palvelun suunnitelman moodin tukee käyttöönoton paikkojen eri määrän. Voit selvittää, kuinka monta lähtö saapumisajat web-sovelluksen tila tukee on artikkelissa [Sovelluksen palvelun hinnat](/pricing/details/app-service/).

- Kun Verkko-sovelluksessa on useita paikkaa, et voi muuttaa tila.

- Skaalaus ei ole käytettävissä tuotannon paikkaa.

- Linkitetyn Resurssienhallinta ei tueta tuotannon paikkaa. [Azure-portaalin](http://go.microsoft.com/fwlink/?LinkId=529715) vain voit välttää tämän mahdollinen vaikutus tuotannon paikka siirtämällä väliaikaisesti tuotannon paikka App palvelun suunnitelman tilaa. Huomaa, että tuotannon paikka on jälleen jakaminen samaa tuotannon paikka ennen kuin voit muuttaa kaksi paikkaa.


[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

<a name="Add"></a>
## <a name="add-a-deployment-slot-to-a-web-app"></a>Käyttöönoton paikka lisääminen verkkosovellukseen ##

Web-sovelluksen on oltava käynnissä, jotta voit ottaa käyttöön useita käyttöönoton paikkojen **Vakio** - tai **Premium** -tilassa.

1. [Azure-portaalissa](https://portal.azure.com/)Avaa web-sovelluksen sivu.
2. Valitse **asetukset**ja valitse sitten **käyttöönoton paikkaa**. Valitse sitten **Lisää paikka** **käyttöönoton paikkojen** -sivu.

    ![Lisää uusi käyttöönoton paikka][QGAddNewDeploymentSlot]

    > [AZURE.NOTE]
    > Jos web app ei ole **Vakio** - tai **Premium** -tilassa, näyttöön tulee sanoma tuettu tilat vaiheistettu julkaisemisen ottaminen käyttöön. Tässä vaiheessa voit halutessasi valita **päivittäminen** ja siirry koodiin **Mittakaava** -välilehti, ennen kuin jatkat.

2. Valitse **Lisää paikka** -sivu nimeä paikka ja valitse, haluatko Kloonaa web app kokoonpano toiseen aiemmin käyttöönoton paikka. Valitse Jatka valintamerkki.

    ![Määritysten lähde][ConfigurationSource1]

    Ensimmäisen kerran lisäät paikka, käytössäsi on vain kaksi vaihtoehtoa: Kloonaa kokoonpano oletusarvon paikka tuotannon tai eivät ollenkaan.

    Kun olet luonut useita paikkaa, osaat Kloonaa kuin tuotannon yksi paikka kokoonpano:

    ![Määritysten lähteistä][MultipleConfigurationSources]

5. Valitse Avaa vapaan-sivu niin, että arvot ja tavoin web-sovelluksen kokoonpanon käyttöönotto-paikka **käyttöönoton paikkojen** -sivu. **your-Web-App-name-Deployment-Slot-name** tulee näkyviin sivu muistuttaa sinua, että olet tarkastelemassa käyttöönoton paikan päällä.

    ![Käyttöönoton paikka otsikko][StagingTitle]

5. Valitse sovelluksen URL-osoite paikka-sivu. Ilmoitus käyttöönoton paikka on oma isäntänimi, ja se on myös live sovellus. Käyttöönotto-paikka julkisen rajoittaa, on artikkelissa [Sovelluksen palvelun online-web-käytön estäminen tuotannon käyttöönoton paikkojen](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/).

Käyttöönoton paikka luonnin jälkeen ei ole sisältöä. Voit ottaa käyttöön paikka eri säilö haaraa tai kokonaan eri säilöön. Voit myös muuttaa paikan määrittäminen. Käyttää päivitysten käyttöönoton paikka liittyvät Julkaise profiili tai käyttöönoton tunnistetietoja.  Voit esimerkiksi [tämän paikka, jossa git julkaiseminen](app-service-deploy-local-git.md).

<a name="AboutConfiguration"></a>
## <a name="configuration-for-deployment-slots"></a>Käyttöönoton paikkojen määritys ##
Kun Kloonaa tietokoneesta toiseen käyttöönoton paikka, kloonatun määritys on muokattavissa. Lisäksi määritysten joitakin osia seuraa sisällön yli swap (ei paikka tietyn) samalla, kun määritys muiden osien säilyvät saman paikan vaihtaminen (paikka tietyn) jälkeen. Seuraavassa on luettelo Näytä määrityksistä, jotka muuttuvat, kun vaihdat paikkaa.

**Asetukset, joka vaihtaa paikkaa**:

- Yleiset asetukset - framework versio 32/64-bittinen, kuten Web sockets
- Sovelluksen asetusten (voi määrittää Jos haluat, paikka)
- Yhteyden merkkijonot (voi määrittää Jos haluat, paikka)
- Tapahtumakäsittelijän yhdistäminen
- Valvonta ja asetukset
- WebJobs sisältö

**Asetukset, joka ei ole vaihtaa paikkaa**:

- Julkaisun päätepisteet
- Mukautettuja toimialuenimiä
- SSL-varmenteita ja sidontojen
- Skaalausasetukset
- WebJobs aikatauluttajien

Voit määrittää sovelluksen asetus tai yhteyden merkkijonona paikka (vaihtaa ei paikkaa) Jos haluat käyttää tiettyä vapaan **Sovelluksen asetukset** -sivu ja valitse sitten määritysten elementeille, jotka kannattaa helppokäyttöisiä paikka **Paikka-asetus** -ruudussa. Huomaa, että merkitseminen määrityselementin paikka tietyn vaikuttaa vahvistamiseksi osa nimellä ei vaihdettavan laitepaikan kaikki web-sovellukseen kytketty käyttöönoton paikkojen yli.

![Paikan asetukset][SlotSettings]

<a name="Swap"></a>
## <a name="to-swap-deployment-slots"></a>Jos haluat käyttöönoton paikkojen vaihtaminen ##

>[AZURE.IMPORTANT] Ennen kyselyjä tuotannon Vaihda online-käyttöönoton paikka, varmista, että kaikki paikka tiettyjä asetuksia on määritetty juuri sellaisena kuin haluat, että se on Vaihda kohde.

1. Vaihda käyttöönoton paikkaa napsauttamalla **Vaihda** -painiketta web Appin painikkeita tai käyttöönoton paikan painikkeita. Varmista, että Vaihda lähde- ja Vaihda kohde on määritetty oikein. Vaihda kohde on yleensä tuotannon paikka.  

    ![Vaihda-painike][SwapButtonBar]

3. Valitse **OK** toiminnon suorittamiseksi. Kun toiminto on valmis, käyttöönoton paikkojen ole vaihtanut.

## <a name="configure-auto-swap-for-your-web-app"></a>Määritä web-sovelluksen automaattinen vaihtaminen ##

Vaihda automaattinen tehostaa kohtaa, johon haluat ottaa käyttöön jatkuvasti web-sovelluksen kanssa nolla kylmän alkamis- ja nolla käyttökatkot Lopeta asiakkaiden web Appin DevOps skenaarioita. Kun käyttöönoton paikka on määritetty automaattinen Vaihda kyselyjä tuotannon, joka kerta, kun painat koodin päivityksen paikkaa, sovelluksen palvelun automaattisesti Vaihda web app kyselyjä tuotannon sen jälkeen, kun se on jo lämmitettävä-paikka.

>[AZURE.IMPORTANT] Kun otat automaattisen Vaihda paikka, varmista, että paikka määritysten täsmälleen tarkoitettu kohde paikka (yleensä tuotannon paikka) määrittäminen.

Vaihda automaattinen määrittäminen paikka on helppoa. Noudata seuraavia ohjeita:

1. Valitse **Käyttöönoton paikkaa** , sivu tuotannon – paikka, saat kyseisen paikka-sivu valitsemalla **Kaikki asetukset** .  

    ![][Autoswap1]

2. Valitse **sovelluksen asetukset**. Valitse **Valitse** **Automaattinen vaihtaminen**, valitse haluamasi kohde paikan **Automaattinen vaihtaminen paikka**ja valitse **Tallenna** komentopalkin. Varmista, että määritysten vapaan täsmälleen kohde paikka tarkoitettu määritykset.

    **Ilmoitukset** -välilehti flash vihreä **onnistui** , kun toiminto on valmis.

    ![][Autoswap2]

    >[AZURE.NOTE] Voit esikatsella automaattinen vaihtaminen web App-tuotannon kohteen paikka voit valita **Automaattinen vaihtaminen paikka** tutustutaan ominaisuus.  

3. Koodi-push käyttöönoton paikkaa haluat suorittaa. Automaattinen vaihtaminen tapahtuu lyhyen ajan kuluttua ja päivitys on otettu kohde-paikan URL-osoitteessa.

<a name="Multi-Phase"></a>
## <a name="use-multi-phase-swap-for-your-web-app"></a>Käytä usean vaiheen Vaihda web Appissa ##

Usean vaiheen Vaihda voivat käyttää yksinkertaistaa määritysten osien suunniteltu Jos haluat, kuten yhteysmerkkijonon paikka kelpoisuuden kontekstissa. Tällaisissa tapauksissa voi olla hyötyä käyttää näiden määritysten elementtien Vaihda kohde Vaihda lähde-ja vahvista Vaihda todella tulee voimaan. Kun Vaihda lähde käytettävissä olevat toiminnot ovat Viimeistellään swap tai palata alkuperäisiä määrityksiä, joka on myös vaikutus peruuttaminen swap Vaihda lähteen käytetään Vaihda kohde määritysten osat. Esimerkkejä, joiden Azure PowerShellin cmdlet-komennot usean vaiheen Vaihda käytettävissä sisältyvät käyttöönoton paikkojen osan Azure PowerShellin cmdlet-komennot.

<a name="Rollback"></a>
## <a name="to-rollback-a-production-app-after-swap"></a>Peruutus tuotannon-sovelluksen vaihdon jälkeen ##

Jos virheitä tunnistetaan tuotannon paikka vaihdon jälkeen, palata paikkaa vanhat Vaihda niiden tilan mukaan samaa kaksi paikkojen vaihtaminen välittömästi.

<a name="Warm-up"></a>
## <a name="custom-warm-up-before-swap"></a>Vaihda ennen mukautetun Lämmitysjaksolla ##

Jotkin sovellukset voivat vaatia mukautetun Lämmitysjaksolla toiminnot. Web.config applicationInitialization määritys-osan avulla voit määrittää mukautetun alustustoiminnot suoritettava, ennen kuin pyyntö on vastaanotettu. Vaihda toiminto odottaa tämän mukautetun Lämmitysjaksolla suorittamiseen. Tässä on esimerkki web.config-osa.

    <applicationInitialization>
        <add initializationPage="/" hostName="[web app hostname]" />
        <add initializationPage="/Home/About" hostname="[web app hostname]" />
    </applicationInitialization>

<a name="Delete"></a>
## <a name="to-delete-a-deployment-slot"></a>Jos haluat poistaa käyttöönoton paikka##

Valitse käyttöönoton vapaan sivu komentopalkin **poistaminen** .  

![Poista käyttöönoton paikka][DeleteStagingSiteButton]

<!-- ======== AZURE POWERSHELL CMDLETS =========== -->

<a name="PowerShell"></a>
##Käyttöönoton paikkojen Azure PowerShell cmdlet-komennot

Azure PowerShell on moduuli, joka sisältää cmdlet-komentojen Azure tuesta web app käyttöönoton paikkojen Azure App palvelun hallinta Windows PowerShellin kautta.

- Tietoja asennuksesta ja määrityksestä PowerShellin Azure ja todennustapa PowerShellin Azure Azure-tilaus Katso, [miten asennetaan ja määritetään PowerShellin Microsoft Azure](../powershell-install-configure.md).  

----------

### <a name="create-web-app"></a>Web-sovelluksen luominen

```
New-AzureRmWebApp -ResourceGroupName [resource group name] -Name [web app name] -Location [location] -AppServicePlan [app service plan name]
```

----------

### <a name="create-a-deployment-slot-for-a-web-app"></a>Käyttöönoton paikka web-sovelluksen luominen

```
New-AzureRmWebAppSlot -ResourceGroupName [resource group name] -Name [web app name] -Slot [deployment slot name] -AppServicePlan [app service plan name]
```

----------

### <a name="initiate-multi-phase-swap-and-apply-target-slot-configuration-to-source-slot"></a>Aloittaa usean vaiheen Vaihda ja käytä lähteen paikka kohde paikan määrittäminen

```
$ParametersObject = @{targetSlot  = "[slot name – e.g. “production”]"}
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [web app name]/[slot name] -Action applySlotConfig -Parameters $ParametersObject -ApiVersion 2015-07-01
```

----------

### <a name="revert-the-first-phase-of-multi-phase-swap-and-restore-source-slot-configuration"></a>Palauttaa ensimmäisen vaiheen usean vaiheen Vaihda ja palauttaa paikan määrittäminen

```
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [web app name]/[slot name] -Action resetSlotConfig -ApiVersion 2015-07-01
```

----------

### <a name="swap-deployment-slots"></a>Käyttöönoton paikkojen vaihtaminen

```
$ParametersObject = @{targetSlot  = "[slot name – e.g. “production”]"}
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [web app name]/[slot name] -Action slotsswap -Parameters $ParametersObject -ApiVersion 2015-07-01
```

----------

### <a name="delete-deployment-slot"></a>Poista käyttöönoton paikka

```
Remove-AzureRmResource -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots –Name [web app name]/[slot name] -ApiVersion 2015-07-01
```

----------

<!-- ======== Azure CLI =========== -->

<a name="CLI"></a>
##Käyttöönoton paikkojen Azure käyttöliittymä (Azure CLI)-komennot

Azure-CLI komennoilla Office kaikissa ympäristöissä käsittelyyn Azure, mukaan lukien tuki Web Appin käyttöönotto paikkojen hallintaan.

- Ohjeet asennuksesta ja määrityksestä Azure-CLI, mukaan lukien Azure CLI yhdistämisestä Azure tilauksen tiedot-kohdassa [Asenna ja Määritä Azure-CLI](../xplat-cli-install.md).

-  Luettelo käytettävissä olevista komennoista Azure-sovelluksen palvelun Azure-CLI, soita `azure site -h`.

----------
### <a name="azure-site-list"></a>Azure sivustoluettelon
Lisätietoja online-sovellusten käyttö nykyisen tilauksesi Soita **azure sivustoluettelon**, kuten seuraavassa esimerkissä.

`azure site list webappslotstest`

----------
### <a name="azure-site-create"></a>Azure sivuston luominen
Voit luoda käyttöönoton paikka, soita **azure sivuston luominen** ja määritä aiemmin web-sovelluksen nimi ja paikka, jos haluat luoda, kuten seuraavassa esimerkissä nimeä.

`azure site create webappslotstest --slot staging`

Voit ottaa, uusi paikka ja tietolähteen ohjausobjektin käyttöön **--git** -vaihtoehto, kuten seuraavassa esimerkissä.

`azure site create --git webappslotstest --slot staging`

----------
### <a name="azure-site-swap"></a>Azure sivuston vaihtaminen
Voit tehdä päivitetyt käyttöönotto tuotannon sovelluksen paikka, Vaihda suorittaminen, kuten seuraavassa esimerkissä **azure sivuston Vaihda** -komennon avulla. Tuotannon sovellus ei ilmene jonkin aikaa alaspäin, eikä se tehdään kylmän Käynnistä.

`azure site swap webappslotstest`

----------
### <a name="azure-site-delete"></a>Azure sivustojen poistaminen
Jos haluat poistaa käyttöönoton paikka, joka ei enää tarvita, käytä **azure sivuston Poista** -komennon, kuten seuraavassa esimerkissä.

`azure site delete webappslotstest --slot staging`

----------

>[AZURE.NOTE] Jos haluat aloittaa Azure App palvelun ennen rekisteröimässä Azure-tili, siirry [Yritä App palvelu](http://go.microsoft.com/fwlink/?LinkId=523751), jossa lyhytkestoinen starter verkkosovellukseen heti voit luoda sovelluksen-palvelussa. Ei ole pakollinen; luottokortit ei ole sitoumukset.

## <a name="next-steps"></a>Seuraavat vaiheet ##
[Azure App palvelun Web App-sovelluksen – tuotannon käyttöönoton paikkojen web käytön estäminen](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/)

[Microsoft Azure ilmainen kokeiluversio](/pricing/free-trial/)

## <a name="whats-changed"></a>Mikä on muuttunut
* Katso muutoksen opas verkkosivuilta App palveluun: [Azure App palvelu ja sen vaikutus aiemmin Azure-palvelut](http://go.microsoft.com/fwlink/?LinkId=529714)

<!-- IMAGES -->
[QGAddNewDeploymentSlot]:  ./media/web-sites-staged-publishing/QGAddNewDeploymentSlot.png
[AddNewDeploymentSlotDialog]: ./media/web-sites-staged-publishing/AddNewDeploymentSlotDialog.png
[ConfigurationSource1]: ./media/web-sites-staged-publishing/ConfigurationSource1.png
[MultipleConfigurationSources]: ./media/web-sites-staged-publishing/MultipleConfigurationSources.png
[SiteListWithStagedSite]: ./media/web-sites-staged-publishing/SiteListWithStagedSite.png
[StagingTitle]: ./media/web-sites-staged-publishing/StagingTitle.png
[SwapButtonBar]: ./media/web-sites-staged-publishing/SwapButtonBar.png
[SwapConfirmationDialog]:  ./media/web-sites-staged-publishing/SwapConfirmationDialog.png
[DeleteStagingSiteButton]: ./media/web-sites-staged-publishing/DeleteStagingSiteButton.png
[SwapDeploymentsDialog]: ./media/web-sites-staged-publishing/SwapDeploymentsDialog.png
[Autoswap1]: ./media/web-sites-staged-publishing/AutoSwap01.png
[Autoswap2]: ./media/web-sites-staged-publishing/AutoSwap02.png
[SlotSettings]: ./media/web-sites-staged-publishing/SlotSetting.png
 
