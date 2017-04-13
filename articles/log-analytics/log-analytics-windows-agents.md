<properties
    pageTitle="Yhdistäminen Log Analytics Windows-tietokoneissa | Microsoft Azure"
    description="Tässä artikkelissa näytetään, miten yhteyden muodostaminen paikalliseen infrastruktuuri Windows-tietokoneissa suoraan OMS käyttämällä mukautetun version, Microsoft seuranta agentti (MMA)."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="banders"/>


# <a name="connect-windows-computers-to-log-analytics"></a>Windows-tietokoneissa yhdistäminen Log Analytics

Tässä artikkelissa näytetään, miten yhteyden muodostaminen paikalliseen infrastruktuuri Windows-tietokoneissa suoraan OMS työtilat käyttämällä mukautetun version, Microsoft seuranta agentti (MMA). Sinun on asennettava ja tekijöiden kaikkien tietokoneissa, joihin haluat muodostaa yhteyden, jotta ne tietojen lähettäminen OMS ja voit tarkastella ja toimia OMS-portaalissa, jonka tietotyypiksi OMS määrän. Kunkin agentti raportoida useita työtiloihin.

Voit asentaa asennustoiminnolla, komentoriville käyttäjäagenttien tai kanssa toivottuja tilan määrittäminen (DSC) Azure automaatio.  

>[AZURE.NOTE] Näennäiskoneiden käytössä Azure voidaan yksinkertaistaa asennuksen käyttäen [virtuaalikoneen tunniste](log-analytics-azure-vm-extension.md).

Agentti käyttää tietokoneissa, joissa on Internet-yhteys, tietojen lähettäminen OMS Internet-yhteys. Tietokoneissa, joissa ei ole Internet-yhteys voit käyttää välityspalvelinta tai OMS Log Analytics Forwarder.

Windows-tietokoneiden yhdistäminen OMS on yksinkertaista 3 helpon vaiheen avulla:

1. Lataa agentti asennustiedosto
2. Asenna-menetelmällä voit valita agentti
3. Määritä agentti tai lisätä uusia työtiloja tarvittaessa

Seuraavassa kaaviossa näkyvät Windows-tietokoneissa ja OMS välisen suhteen, olet asentanut ja määrittänyt agenttien vuoksi.

![OMS-suoraan-agentti-kaavio](./media/log-analytics-windows-agents/oms-direct-agent-diagram.png)


## <a name="system-requirements-and-required-configuration"></a>Järjestelmävaatimukset ja vaadittu
Ennen kuin asennat tai agenttien vuoksi käyttöön, tarkista seuraavat asiat, jotta tarvittavat ehtojen mukaan.

- Voit asentaa OMS MMA vain tietokoneiden Windows Server 2008: n SP 1 tai uudempi tai Windows 7 SP1 tai uudempi versio.
- Tarvitset OMS-tilaus.  Lisätietoja on artikkelissa [Log Analytics käytön aloittaminen](log-analytics-get-started.md).
- Windows-tietokoneissa on voitava muodostaa yhteyden HTTPS. Yhteys voi olla suora välityspalvelinta, tai OMS Log Analytics Forwarder kautta.
- Voit asentaa OMS MMA erillisten tietokoneiden ja palvelinten näennäiskoneiden. Jos haluat muodostaa yhteyden OMS Azure isännöimä näennäiskoneiden, katso [yhteyden Azuren näennäiskoneiden Log-analyysin avulla](log-analytics-azure-vm-extension.md).
- Agentti on käyttää eri resursseille TCP-porttiin 443. Lisätietoja [Määritä välityspalvelin ja palomuuri loki Analytics asetukset](log-analytics-proxy-firewall.md).

## <a name="download-the-agent-setup-file-from-oms"></a>Lataa agent-asennustiedosto OMS
1. Valitse OMS-portaalissa **Yleiskatsaus** -sivulla **asetukset** -ruutu.  Valitse yläreunassa **Yhdistetty tietolähteet** -välilehti.  
    ![Yhdistetyn lähteet-välilehti](./media/log-analytics-windows-agents/oms-direct-agent-connected-sources.png)
2. Valitse **Liitä tietokoneiden suoraan**kohdassa **Lataa Windows-agentti** koskevat tietokoneen suoritin tyypin Lataa asennustiedosto.
3. Oikealla puolella **Työtilan tunnus**napsauta Kopioi-kuvaketta ja Liitä tunnus Muistioon.
4. Oikealla puolella **Perusavaimen**kopio-kuvaketta ja liitä sitten-näppäintä Muistioon.     
    ![Kopioi työtila-tunnus ja perusavain](./media/log-analytics-windows-agents/oms-direct-agent-primary-key.png)

## <a name="install-the-agent-using-setup"></a>Asenna agent-vaihtoehdon avulla
1. Asennusohjelman agentti asennetaan tietokoneeseen, jossa haluat hallita.
2. Valitse Tervetuloa-sivulla **Seuraava**.
3. Valitse käyttöoikeussopimuksen ehdot-sivulla Lue käyttöoikeus ja valitse sitten **hyväksyn**.
4. Valitse kohdekansio-sivun muuta tai Säilytä asennuksen oletuskansio ja valitse sitten **Seuraava**.
5. Agentti asennuksen asetukset-sivulla voit muodostaa agentti, Azure Log Analytics (OMS), Operations Manager tai voi jättää vaihtoehdot tyhjäksi, jos haluat määrittää agentti myöhemmin. Valitse **Seuraava**.   
    - Jos valitsit Azure Log Analytics (OMS) voit muodostaa, Liitä **Työtila-tunnus** ja **Työtilan Key (perusavain)** , joka on kopioitu Muistioon edellisessä ja valitse sitten **Seuraava**.  
        ![Liitä työtilan tunnuksen ja perusavain](./media/log-analytics-windows-agents/connect-workspace.png)
    - Jos haluat muodostaa yhteyden Operations Manager, kirjoita **Hallinta ryhmänimi**, **Hallinta-palvelimen** nimi ja **Hallinnan palvelimen portti**ja valitse sitten **Seuraava**. Agentti toiminnon tili-sivulla Valitse Paikallinen järjestelmä-tili tai paikallisen toimialuetili ja valitse sitten **Seuraava**.  
        ![hallinnan määrityksiä](./media/log-analytics-windows-agents/oms-mma-om-setup01.png)![toiminto tilille](./media/log-analytics-windows-agents/oms-mma-om-setup02.png)

6. Valitse Asenna-sivun valitsemalla Oletko valmis Tarkista valinnat ja valitse sitten **Asenna**.
7. Määritysten onnistui-sivulla **Valmis**.
8. Kun olet valmis, **Ohjauspaneelissa**näkyy **Microsoft Agent seuranta** . Voit tarkastella kokoonpanosi siellä ja tarkista, että agentti on yhdistetty, toiminnalliset tiedot (OMS). Kun OMS, agentti näyttää viestin, jossa sanotaan: **Microsoft seuranta Agent on yhdistetty Microsoft toimintojen hallinta Suite-palveluun.**

## <a name="install-the-agent-using-the-command-line"></a>Asenna komentoriviltä agentti
- Muokkaa ja asenna komentoriviltä agentti seuraavan esimerkin avulla.

    >[AZURE.NOTE] Jos haluat päivittää agentti, joudut komentosarjan API Log-analyysin avulla. Lisätietoja on kohdassa päivittäminen agentti.

    ```
    MMASetup-AMD64.exe /Q:A /R:N /C:"setup.exe /qn ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_ID=<your workspace id> OPINSIGHTS_WORKSPACE_KEY=<your workspace key> AcceptEndUserLicenseAgreement=1"
    ```

## <a name="upgrade-the-agent-and-add-a-workspace-using-a-script"></a>Päivitä agentti ja lisätä työtilan komentosarjan avulla
Voit päivittää agentti ja lisätä työtilan komentosarjojen API PowerShell seuraavassa esimerkissä Log-analyysin avulla.

```
$mma = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'
$mma.AddCloudWorkspace($workspaceId, $workspaceKey)
$mma.ReloadConfiguration()
```

>[AZURE.NOTE] Jos olet käyttänyt komentoriviltä tai komentosarjan aiemmin asentaminen ja määrittäminen agentti, `EnableAzureOperationalInsights` on korvattu ohjatulla `AddCloudWorkspace`.

## <a name="install-the-agent-using-dsc-in-azure-automation"></a>Asenna DSC käyttäminen Azure automaatio agentti

>[AZURE.NOTE] Esimerkki tämän menettelyn ja komentosarja ei päivitä aiemmin agentti.

1. Tuoda xPSDesiredStateConfiguration DSC moduulin [http://www.powershellgallery.com/packages/xPSDesiredStateConfiguration](http://www.powershellgallery.com/packages/xPSDesiredStateConfiguration) Azure automaatio.  
2.  Luo Azure automaatio muuttujan resurssien *OPSINSIGHTS_WS_ID* ja *OPSINSIGHTS_WS_KEY*. Määritä *OPSINSIGHTS_WS_ID* OMS Log Analytics työtilan ID ja määritä *OPSINSIGHTS_WS_KEY* työtilan perusavaimeen.
3.  Käytä alla olevaa komentosarjaa ja tallenna se MMAgent.ps1
4.  Muokkaa ja asenna DSC käyttäminen Azure automaatio agentti seuraavan esimerkin avulla. Tuo MMAgent.ps1 Azure automaatio Azure Automation-liittymän tai cmdlet-komennon avulla.
5.  Määritä solmu määritykset. 15 minuutin kuluessa solmu tarkistaa sen asetukset ja MMA sijaita solmu.

```
Configuration MMAgent
{
    $OIPackageLocalPath = "C:\MMASetup-AMD64.exe"
    $OPSINSIGHTS_WS_ID = Get-AutomationVariable -Name "OPSINSIGHTS_WS_ID"
    $OPSINSIGHTS_WS_KEY = Get-AutomationVariable -Name "OPSINSIGHTS_WS_KEY"


    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    Node OMSnode {
        Service OIService
        {
            Name = "HealthService"
            State = "Running"
            DependsOn = "[Package]OI"
        }

        xRemoteFile OIPackage {
            Uri = "http://download.microsoft.com/download/0/C/0/0C072D6E-F418-4AD4-BCB2-A362624F400A/MMASetup-AMD64.exe"
            DestinationPath = $OIPackageLocalPath
        }

        Package OI {
            Ensure = "Present"
            Path  = $OIPackageLocalPath
            Name = "Microsoft Monitoring Agent"
            ProductId = "8A7F2C51-4C7D-4BFD-9014-91D11F24AAE2"
            Arguments = '/C:"setup.exe /qn ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_ID=' + $OPSINSIGHTS_WS_ID + ' OPINSIGHTS_WORKSPACE_KEY=' + $OPSINSIGHTS_WS_KEY + ' AcceptEndUserLicenseAgreement=1"'
            DependsOn = "[xRemoteFile]OIPackage"
        }
    }
}  


```


## <a name="configure-an-agent-manually-or-add-additional-workspaces"></a>Määrittää agentti manuaalisesti tai lisätä uusia työtiloja
Jos olet asentanut agenttien vuoksi, mutta määrittänyt sitä tai jos haluat agentti ja raportoi useita työtilat, voit tehdä seuraavat tiedot agentti ottaminen käyttöön ja määrittää sen uudelleen. Kun olet määrittänyt agentti, Rekisteröi agent-palvelussa ja saavat tarvittavat tiedot ja hallinta-paketteja, jotka sisältävät ratkaisutiedot.

1. Kun olet asentanut Microsoft Agent seuranta, Avaa **Ohjauspaneeli**.
2. Avaa **Microsoft Agent seuranta** ja valitse sitten **Azure Log Analytics (OMS)** -välilehti.   
3. Valitse **Lisää** ja avaa sitten **Lisää Log Analytics työtila** -ruutu.
4. Liitä **Työtila-tunnus** ja **Työtilan Key (perusavain)** , joka on kopioitu Muistioon edellisessä toiminnossa työtilan, johon haluat lisätä, ja valitse sitten **OK**.  
    ![Määritä toiminnallisia tiedot](./media/log-analytics-windows-agents/add-workspace.png)

Kun agentti valvoo tietokoneista kerätä tietoja, valvoo OMS tietokoneiden määrä näkyvät OMS portaalissa **Yhdistetty lähteet** -välilehden **asetukset** - **Palvelimet yhdistetty**muodossa.


## <a name="to-disable-an-agent"></a>Jos haluat poistaa käytöstä agentti
1. Kun olet asentanut agentti, Avaa **Ohjauspaneeli**.
2. Avaa Microsoft Agent seuranta ja valitse sitten **Azure Log Analytics (OMS)** -välilehti.
3. Valitse työtila ja valitse sitten **Poista**. Toista tämä vaihe kaikkien muiden työtilojen.


## <a name="optionally-configure-agents-to-report-to-an-operations-manager-management-group"></a>Voit myös määrittäminen tekijöiden ja raportoi Operations Manager-hallinta-ryhmä

Jos käytät Operations Manager IT-infrastruktuurin, voit käyttää myös MMA-agentti Operations Manager edustajana.

### <a name="to-configure-mma-agents-to-report-to-an-operations-manager-management-group"></a>Voit määrittää MMA tekijöiden ja raportoi Operations Manager-hallinta-ryhmä
1.  Tietokoneessa, johon agentti on asennettu Avaa **Ohjauspaneeli**.
2.  Avaa **Microsoft Agent seuranta** ja valitse sitten **Operations Manager** -välilehti.
    ![Microsoft seuranta agentti Operations Manager-välilehti](./media/log-analytics-windows-agents/om-mg01.png)
3.  Jos Operations Manager-palvelimet on Active Directory-integrointi, valitse **Päivitä automaattisesti hallinta ryhmän varaukset-AD DS: ÄÄN**.
4.  Valitse Avaa **Lisää Management Group** -valintaikkunassa **Lisää** .  
    ![Microsoft Agent seuranta Lisää hallinta-ryhmä](./media/log-analytics-windows-agents/oms-mma-om02.png)
5.  Kirjoita **hallinta ryhmänimi** -ruutuun hallinta-ryhmän nimi.
6.  Kirjoita **ensisijainen hallintapalvelin** -ruutuun ensisijainen asiakirjanhallinnan palvelimeen tietokonenimi.
7.  Kirjoita TCP-porttinumero **hallinta palvelinportti** -ruutuun.
8.  Valitse **Toiminto tilille**, paikallinen järjestelmä-tili tai paikallisen toimialuetiliä.
9.  Valitse Sulje **Lisää Management Group** -valintaikkunassa ja valitse **OK** ja sulje **Microsoft seuranta agenttiominaisuudet** -valintaikkunassa **OK** .

## <a name="optionally-configure-agents-to-use-the-oms-log-analytics-forwarder"></a>Voit myös määrittäminen käyttämään OMS Log Analytics Forwarder agenttien vuoksi

Jos sinulla on palvelimia tai ohjelmat, joita ei ole Internet-yhteys, voit edelleen on lähettää tietoja OMS OMS Log Analytics Forwarder avulla.  Kun forwarder, agenttien vuoksi kaikki tiedot lähetetään ja yksittäisen Serveriä, joka on yhteydessä Internetiin. Forwarder siirtää analysoiminen tiedoista, jotka siirretään suoraan ilman OMS tiedot niistä.

Katso lisätietoja forwarder, mukaan lukien asennus ja määritys [OMS Log Analytics Forwarder](https://blogs.technet.microsoft.com/msoms/2016/03/17/oms-log-analytics-forwarder) .

Saat tietoja siitä, miten voit määrittää edustajasi käyttämään välityspalvelinta, joka on tässä tapauksessa OMS Forwarder, [loki Analytics välityspalvelin ja palomuurin asetusten määrittäminen](log-analytics-proxy-firewall.md).

## <a name="optionally-configure-proxy-and-firewall-settings"></a>Voit myös välityspalvelin ja palomuurin asetusten määrittäminen
Jos sinulla on välityspalvelimet tai palomuurit ympäristössä, joka rajoittaa Internet, katso [välityspalvelin ja palomuurin asetusten määrittäminen Log Analytics](log-analytics-proxy-firewall.md) edustajasi viestintä OMS-palvelun käyttöön.

## <a name="next-steps"></a>Seuraavat vaiheet

- [Lisää Log Analytics-ratkaisuja ratkaisuvalikoimasta](log-analytics-add-solutions.md) Lisää toimintoja ja kerätä tietoja.
- Jos organisaatiosi käyttää palomuurin tai välityspalvelimen niin, että agenttien vuoksi viestiä Log Analytics-palvelussa [Määritä välityspalvelin ja palomuurin lokin Analytics asetukset](log-analytics-proxy-firewall.md) .
