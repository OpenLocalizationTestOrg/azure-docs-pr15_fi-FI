<properties
    pageTitle="Operations Manager yhdistäminen Log Analytics | Microsoft Azure"
    description="System Center Operations Manager aiemmin kiinnostusta ylläpitämään ja laajennettu ominaisuuksien käyttäminen Log Analytics Operations Manager voidaan yhdistää OMS työtilassa."
    services="log-analytics"
    documentationCenter=""
    authors="MGoedtel"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="magoedte"/>

# <a name="connect-operations-manager-to-log-analytics"></a>Yhteyden muodostaminen Log Analytics Operations Manager

System Center Operations Manager aiemmin kiinnostusta ylläpitämään ja laajennettu ominaisuuksien käyttäminen Log Analytics Operations Manager voidaan yhdistää OMS työtilassa.  Näin OMS mahdollisuudet hyödyntää samalla, kun hallinnan toiminnot:

- Jatka IT-palveluiden kanssa Operations Manager kunnon valvonta
- Ylläpidä integrointi tukevat tapauksen ja ongelmien ITSM ratkaisujen kanssa
- Ottaa käyttöön paikallisen ja julkisen cloud IaaS näennäiskoneiden, joka seurata Operations Manager tekijöiden elinkaari hallinta

System Center Operations Manager ja lisää arvon yrityksen palvelun toimintojen määrittäminen mukaan hyödyntäminen nopeuden ja tehokkuuden OMS kerätään, tallentaminen ja analysoidaan tietoja Operations Manager.  OMS auttaa yhdistää ja tunnistaminen ongelmia virheitä ja pinnoituksen reoccurrences, jotka tukevat luotuun ongelmasta hallintaprosessin.   Hakukone tutkia suorituskykyä, tapahtuman ja ilmoitusten tiedot ja rich raporttinäkymien ja raportointiominaisuudet voit näyttää nämä tiedot selkeällä tavalla joustavuutta esitellään OMS kokoaa complimenting Operations Manager voimakkuus.

Raportoinnin Operations Manager hallinta-ryhmässä tekijöiden kerää tietoja palvelinten Log Analytics tietolähteet ja olet ottanut OMS tilauksen ratkaisuja perusteella.  Ratkaisu on käytössä, näitä ratkaisuja tietojen mukaan joko lähetetään suoraan Operations Manager-asiakirjanhallinnan palvelimeen OMS web-palveluun tai vuoksi agentti hallitun järjestelmän kerättyjä tietoja äänenvoimakkuuden suoraan agentti OMS web-palveluun lähetetään. Asiakirjanhallinnan palvelimeen välittää suoraan OMS WWW-palvelun OMS tiedot, se ei koskaan kirjoitetaan OperationsManager tai OperationsManagerDW-tietokantaan.  Kun asiakirjanhallinnan palvelimeen menettää yhteyden OMS-web-palvelun kanssa, siirtää tiedot paikallisesti, kunnes viestintä muodostetaan uudelleen OMS kanssa.  Jos asiakirjanhallinnan palvelimeen on suunniteltu ylläpito- tai käyttökatkosta suunnittelematon vuoksi offline-tilassa, toiseen asiakirjanhallinnan palvelimeen hallinta-ryhmässä jatkuu OMS yhteys.  

Seuraavassa kaaviossa on esitetty hallinta-palvelimia ja agenttien vuoksi System Center Operations Manager hallinta-ryhmä ja OMS, mukaan lukien suunta ja portit välinen yhteys.   

![OMS toiminnot-hallinta-integrointi-kaavio](./media/log-analytics-om-agents/oms-operations-manager-connection.png)

## <a name="system-requirements"></a>Järjestelmävaatimukset
Ennen kuin aloitat, tarkista seuraavat asiat vahvistamiseksi tarvittavat edellytykset täyttyvät.

- OMS tukee vain Operations Manager 2012 SP1 UR6 ja suurempi, ja Operations Manager 2012 R2 UR2 ja suurempia.  Välityspalvelimen tuki on lisätty Operations Manager 2012 SP1 UR7 ja Operations Manager 2012 R2 UR3.
- Kaikki Operations Manager tekijöiden on täytettävä pienin tuki vaatimukset. Varmista, että tekijöiden on pienin päivitys ylöspäin, muuten Windows agentti liikenne epäonnistuu eivätkä paljon virheitä saattaa täyttää Operations Manager tapahtumaloki.
- OMS-tilaus.  Lisätietoja Lue [Log Analytics käytön aloittaminen](log-analytics-get-started.md).

## <a name="connecting-operations-manager-to-oms"></a>Yhteyden muodostaminen OMS Operations Manager
Suorita Määritä Operations Manager hallinta-ryhmä, muodosta yhteys OMS työtilojen seuraavat sarjaa.

1. Valitse **hallinta** -työtilan Operations Manager-konsolissa.
2. Laajenna toimintojen hallinta Suite-solmu ja valitse **yhteys**.
3. Valitse **Rekisteröi toimintojen hallinta ohjelmistopakettiin** -linkki.
4. Valitse **Toimintojen hallinta Suite Onboarding ohjatun: todennus** sivun, kirjoita sähköpostiosoite tai puhelinnumero ja joka on liitetty OMS tilauksen järjestelmänvalvojatilin salasana ja valitse **Kirjaudu sisään**.
5. Kun olet onnistuneesti todennetaan, valitse **Toimintojen hallinta Suite Onboarding ohjatun: Valitse työtila** sivun, sinua pyydetään valitsemaan OMS työtilan.  Jos sinulla on useampi kuin yksi työtilan, valitse työtila rekisteröidä Operations Manager hallinta-ryhmän avattavasta luettelosta ja valitse sitten **Seuraava**.

    >[AZURE.NOTE] Operations Manager tukee vain yksi OMS työtila kerrallaan. Yhteys- ja tietokoneissa, joissa on rekisteröity OMS edellisen Workspacen poistetaan OMS.

6. Valitse **Toimintojen hallinta Suite Onboarding ohjatun: Yhteenveto** -sivulla asetusten vahvistaminen ja jos ne ovat oikeat, valitsemalla **Luo**.
7. Valitse **Toimintojen hallinta Suite Onboarding ohjatun: valmis** sivun, valitse **Sulje**.

### <a name="add-agent-managed-computers"></a>Lisää agentti hallitun tietokoneissa
Määritettäessä integrointi OMS-työtila, kun tämä muodostaa yhteyden OMS, -raportoinnin hallinta-ryhmässä tekijöiden kerätä tietoja. Näin ei käydä vasta, kun olet määrittänyt tietyn agentti hallitun tietokoneet kerää tietoja lokin analysoinnissa. Voit valita tietokoneen objektit joko yksitellen tai voit valita ryhmää, joka sisältää Windows-tietokoneen objekteja. Et voi valita ryhmän, joka sisältää toisen luokan, kuten loogiset levyjen tai SQL-tietokantoja esiintymät.

1. Avaa Operations Manager-konsolin ja valitse **hallinta** -työtilassa.
2. Laajenna toimintojen hallinta Suite-solmu ja valitse **yhteys**.
3. **Lisää tietokoneen/ryhmä** -linkin kohdasta toiminnot-ruudun oikeanpuoleisessa otsikko.
4. **Tietokoneen Etsi** -valintaikkunassa voit etsiä tietokoneita tai ryhmiä valvoo Operations Manager. Valitse tietokoneet tai ryhmiä määrän OMS, valitse **Lisää**ja valitse sitten **OK**.

Voit tarkastella tietokoneiden ja ryhmien määritetty tietojen keräämiseen hallitun tietokoneiden solmu toimintojen hallinta Suite toiminnot-konsolin **hallinta** -työtilassa.  Tässä voit lisätä tai poistaa tietokoneiden ja ryhmien tarpeen mukaan.

### <a name="configure-oms-proxy-settings-in-the-operations-console"></a>Toiminnot-konsolin OMS välityspalvelimen asetusten määrittäminen
Suorita seuraavat vaiheet, jos sisäinen välityspalvelin on välillä hallinta-ryhmä ja OMS web-palveluun.  Nämä asetukset ovat keskitetysti hallittujen hallinta-ryhmästä ja distributed agentti hallitun järjestelmiin, jotka sisältyvät laajuuden tietojen keräämiseen OMS varten.  Tämä on hyötyä, kun tiettyjen ratkaisujen ohittaa asiakirjanhallinnan palvelimeen ja Lähetä tiedot suoraan OMS WWW-palvelun.

1. Avaa Operations Manager-konsolin ja valitse **hallinta** -työtilassa.
2. Laajenna toimintojen hallinta-ohjelmistopaketti ja valitse sitten **yhteydet**.
3. Valitse OMS yhteys-näkymässä **Määrittäminen välityspalvelinta**.
4. Valitse **Ohjattu toimintojen hallinta Suite: välityspalvelimen** sivun, valitse **Käytä toimintojen hallinta Suite käyttämään välityspalvelinta**, ja Kirjoita porttinumero, esimerkiksi http://corpproxy:80 URL-osoite ja valitse sitten **Valmis**.

Jos välityspalvelimeen edellyttää todennusta, suorita seuraavat vaiheet Määritä tunnistetiedot ja asetuksia, jotka on hallitun tietokoneissa, joissa raportoi OMS hallinta-ryhmässä Välitä.

1. Avaa Operations Manager-konsolin ja valitse **hallinta** -työtilassa.
2. Valitse **RunAs määritys**- **profiileista**.
3. Avaa **System Center Advisor Suorita kuin profiilin välityspalvelimen** profiili.
4. Valitse Suorita kuin profiilin ohjatun valitsemalla Lisää Suorita nimellä-tilin käyttöä varten. Voit luoda uuden [tilin Suorita nimellä](https://technet.microsoft.com/library/hh321655.aspx) tai Käytä aiemmin luotua tiliä. Tällä tilillä on ole riittäviä käyttöoikeuksia välityspalvelimen kautta.
5. Voit määrittää tilin hallinta-kohdassa **valitun luokan, ryhmän tai objekti**, valitse **Valitse...** ja valitse sitten **ryhmä...** Voit avata **Ryhmän Etsi** -ruutuun.
6. Etsi ja valitse sitten **Microsoft System Center Advisor seuranta palvelimen ryhmä**.  Kun olet valinnut Sulje **Ryhmän Etsi** -ruutuun ryhmän valitsemalla **OK** .
7.  Valitse **OK** ja sulje **tilin Suorita nimellä** -ruutuun.
8.  Valitse Tallenna muutokset ja suorita ohjattu **Tallenna** .

Kun yhteys on luotu ja voit määrittää, mitkä tekijöiden kerääminen ja raportointi tietojen OMS, seuraavat määritykset käytetään hallinta-ryhmässä ei välttämättä tässä järjestyksessä:

- Suorita nimellä tilin **Microsoft.SystemCenter.Advisor.RunAsAccount.Certificate** luodaan.  Se on liitetty Suorita nimellä-profiilin **Microsoft System Center Advisor Suorita kuin profiilin Blob-objektien** ja on kohdistamisen kahteen luokkaan - **Sivustokokoelman-palvelimen nimen** ja **Operations Manager hallinta-ryhmässä**.
- Kaksi yhdistimet luodaan.  Ensimmäinen nimi on **Microsoft.SystemCenter.Advisor.DataConnector** ja tilauksessa, joka välittää kaikki ilmoitukset, jotka on luotu kaikkien luokkien hallinta-ryhmässä esiintymät OMS Log Analytics määritetään automaattisesti. Toisen yhdistimen on **Advisor yhdistin**, joka on vastuussa OMS verkkopalvelun yhteydenpito ja tietojen jakamiseen.
- Tekijöiden ja ryhmiä, jotka olet valinnut kerää tietoja hallinta-ryhmässä lisätään **Microsoft System Center Advisor seuranta palvelimen ryhmän**.

## <a name="management-pack-updates"></a>Management pack-päivitykset
Kun määritys on valmis, Operations Manager hallinta-ryhmässä muodostaa yhteyden OMS-palvelussa.  Asiakirjanhallinnan palvelimeen WWW-palvelun synkronointi ja vastaanottaa päivitetyt kokoonpanotietoja management Pack-paketit on otettu käyttöön ratkaisuja, jotka Operations Manager integroida lomake.   Operations Manager tarkistaa päivitykset nämä management Pack-paketit automaattinen lataaminen ja tuoda niitä, kun ne ovat käytettävissä.  Sääntöjen kaksi erityisesti käytettävän ohjausobjektin näin:

- **Microsoft.SystemCenter.Advisor.MPUpdate** - päivittää perus OMS management Pack-paketit. Suorittaa 12 (12) tunnin välein oletusarvoisesti.
- **Microsoft.SystemCenter.Advisor.Core.GetIntelligencePacksRule** - päivitykset ratkaisu management Pack-paketit käytössä työtilassa. Suorittaa oletusarvoisesti viisi (5) minuutin välein.

Voit ohittaa nämä kaksi sääntöä automaattisen lataamisen estäminen poistamalla ne tai muokata korkojakso, kuinka usein asiakirjanhallinnan palvelimeen synkronointi OMS voit tarkistaa uudet hallintapaketin on käytettävissä ja voi ladata.  Noudattamalla [Voit ohittaa säännön tai näytön](https://technet.microsoft.com/library/hh212869.aspx) **taajuus** -parametrin arvon sekunteina synkronoinnin aikataulun muuttaminen tai muokata **käytössä** -parametrin käytöstä sääntöjä.  Kohde kaikkien objektien luokan Operations Manager hallinta-ryhmässä korvaa.

Voit halutessasi jatkaa aiemmin ohjaukseen management pack-versioiden tuotannon hallinta-ryhmässä Muuta ohjausobjektin prosessin jälkeen voit sääntöjen käytöstä ja otat ne tietyn aikoina, kun päivitykset on sallittu. Jos siinä on Internet-yhteys: ole kehittäminen tai kysymysten ja vastausten hallinnan ryhmän, voit määrittää ryhmän hallinta OMS-työtilan tukemaan tässä skenaariossa kanssa.  Näin voit tarkastella ja arvioida iteratiivinen versioiden OMS management Pack ennen kuin vapautat ne tuotannon hallinta-ryhmään.

## <a name="switch-an-operations-manager-group-to-a-new-oms-workspace"></a>Siirry uuden OMS työtilan Operations Manager-ryhmä
1. Kirjaudu sisään OMS-tilauksesi ja uuden työtilan luominen [Microsoft toimintojen hallinta](http://oms.microsoft.com/)Suite.
2. Avaa Operations Manager-konsolin tilille, jolla on Operations Manager järjestelmänvalvojat-roolin jäsen ja valitse **hallinta** -työtilassa.
3. Laajenna toimintojen hallinta-ohjelmistopaketti ja valitse **yhteydet**.
4. Valitse ruudussa toinen-puolella **Määrittää uudelleen toiminnon Management Suite** -linkki.
5. Noudata **Ohjattu toimintojen hallinta Suite-Onboarding** ja kirjoita sähköpostiosoite tai puhelinnumero ja joka on liitetty työtilaa OMS järjestelmänvalvojatilin salasana.

    > [AZURE.NOTE] **Toimintojen hallinta Suite Onboarding ohjatun: Valitse työtila** sivun esittää luodun työtilan, joka on käytössä.


## <a name="validate-operations-manager-integration-with-oms"></a>Vahvista OMS Operations Manager-integrointi
Jakautuvat muutamalla eri tavalla, voit tarkistaa, että OMS Operations Manager integrointiin onnistuu.

### <a name="to-confirm-integration-from-the-oms-portal"></a>Vahvista OMS-portaalista integrointi

1.  Valitse OMS-portaalissa **asetukset** -ruutu
2.  Valitse **yhdistetty lähteistä**.
3.  System Center Operations Manager-kohdassa taulukon pitäisi näkyä luettelossa on monta agenttien ja tila, kun tiedot on vastaanotettu viimeksi hallinta-ryhmän nimi.

    ![OMS-asetukset-connectedsources](./media/log-analytics-om-agents/oms-settings-connectedsources.png)

4.  Huomautus asetukset-sivun vasemmasta **Työtilan tunnus** -arvoa.  Voit vahvistaa sen vastaan Operations Manager hallinta ryhmän alla.  

### <a name="to-confirm-integration-from-the-operations-console"></a>Vahvista toiminnot-konsolin integrointi

1.  Avaa Operations Manager-konsolin ja valitse **hallinta** -työtilassa.
2.  Valitse **Management Pack-paketit** ja **Etsi:** teksti-ruutuun **tukipalvelu** tai **Intelligence**.
3.  Sen mukaan, kun olet ottanut käyttöön ratkaisuja näet vastaavat hallintapaketin hakutulokset-luettelossa.  Esimerkiksi jos olet ottanut ilmoitusten Management-ratkaisun, management pack Microsoft System Center Advisor ilmoitusten hallinta on luettelossa.
4.  **Seuranta** -näkymässä Siirry **Toimintojen hallinnan Suite\Health tila** -näkymässä.  Valitse asiakirjanhallinnan palvelimeen, valitse **Hallinnan palvelimen tila** -ruudun ja **Tietonäkymästä** -ruudussa Tarkista **todennus palvelun URI** -ominaisuuden arvo vastaa OMS työtilan ID-tunnuksellasi.

    ![OMS-OpsMgr-mg-authsvcuri-Property-MS](./media/log-analytics-om-agents/oms-opsmgr-mg-authsvcuri-property-ms.png)


## <a name="remove-integration-with-oms"></a>Poista OMS integrointi
Kun et enää tarvitse Operations Manager hallinta-ryhmä ja OMS työtilan välinen integrointi, on useita vaiheita tarvitse poistaa oikein yhteyden ja määritysten hallinta-ryhmässä. Seuraavassa on voit päivittää OMS työtilan poistamalla hallinta-ryhmässä viittaus, poista OMS yhdistimet ja poista management Pack-paketit tukevat OMS.   

1.  Avaa Operations Manager komento-liittymän tilille, jolla on Operations Manager järjestelmänvalvojat-roolin jäsen.

    >[AZURE.WARNING] Tarkista kaikki mukautetut management Pack sanalla tukipalvelu tai IntelligencePack ennen jatkamista nimessä ei ole, muuten seuraavasti poistaa ne hallinta-ryhmästä.

2.  Shell komentoriviltä kirjoittamalla`Get-SCOMManagementPack -name "*advisor*" | Remove-SCOMManagementPack`

3.  Seuraava tyyppi`Get-SCOMManagementPack -name “*IntelligencePack*” | Remove-SCOMManagementPack`

4.  Avaa Operations Manager toimintojen console tilille, jolla on Operations Manager järjestelmänvalvojat-roolin jäsen.
5.  Valitse **hallinta**-osassa **Management Pack** -solmu ja **Etsi:** , kirjoita **Advisor** ja tarkista seuraavat management Pack-paketit on yhä tuotu hallinta-ryhmässä:

    - Microsoft System Center tukipalvelu
    - Microsoft System Center Advisor sisäinen

6. Valitse OMS-portaalissa **asetukset** -ruutu.
7.  Valitse **yhdistetty lähteistä**.
8.  System Center Operations Manager-kohdassa taulukon pitäisi näkyä haluat poistaa työtilasta hallinta-ryhmän nimi.  Valitse sarakkeen **Viimeksi tiedot**-kohdasta **Poista**.  

    >[AZURE.NOTE] **Poista** -linkki ei ole tyylivalikoimien 14 päivän jälkeen, jos ei havaita yhdistetyn hallinta-ryhmästä aktiviteettia.  
   
9.  Näyttöön tulee ikkuna, jossa pyydetään vahvistamaan, että haluat jatkaa poistamista.  Jatka valitsemalla **Kyllä** . 

Voit poistaa kaksi yhdysviivat - Microsoft.SystemCenter.Advisor.DataConnector ja Advisor yhdistimen alla PowerShell-komentosarjaa tallennettava tietokoneeseen ja suorittaa seuraavien esimerkkien avulla.

```
    .\OM2012_DeleteConnector.ps1 “Advisor Connector” <ManagementServerName>
    .\OM2012_DeleteConnectors.ps1 “Microsoft.SytemCenter.Advisor.DataConnector” <ManagementServerName>
```

>[AZURE.NOTE] Voit suorittaa tämän komentosarjaa asiakirjanhallinnan palvelimeen, jollei tietokoneen pitäisi olla Operations Manager 2012 SP1 tai R2-komennon shell, sen mukaan, hallinta-ryhmässä versio asennettuna.

```
    `param(
    [String] $connectorName,
    [String] $msName="localhost"
    )
    $mg = new-object Microsoft.EnterpriseManagement.ManagementGroup $msName
    $admin = $mg.GetConnectorFrameworkAdministration()
    ##########################################################################################
    # Configures a connector with the specified name.
    ##########################################################################################
    function New-Connector([String] $name)
    {
         $connectorForTest = $null;
         foreach($connector in $admin.GetMonitoringConnectors())
    {
    if($connectorName.Name -eq ${name})
    {
         $connectorForTest = Get-SCOMConnector -id $connector.id
    }
    }
    if ($connectorForTest -eq $null)
    {
         $testConnector = New-Object Microsoft.EnterpriseManagement.ConnectorFramework.ConnectorInfo
         $testConnector.Name = $name
         $testConnector.Description = "${name} Description"
         $testConnector.DiscoveryDataIsManaged = $false
         $connectorForTest = $admin.Setup($testConnector)
         $connectorForTest.Initialize();
    }
    return $connectorForTest
    }
    ##########################################################################################
    # Removes a connector with the specified name.
    ##########################################################################################
    function Remove-Connector([String] $name)
    {
        $testConnector = $null
        foreach($connector in $admin.GetMonitoringConnectors())
       {
        if($connector.Name -eq ${name})
       {
         $testConnector = Get-SCOMConnector -id $connector.id
       }
      }
     if ($testConnector -ne $null)
     {
        if($testConnector.Initialized)
     {
     foreach($alert in $testConnector.GetMonitoringAlerts())
     {
       $alert.ConnectorId = $null;
       $alert.Update("Delete Connector");
     }
     $testConnector.Uninitialize()
     }
     $connectorIdForTest = $admin.Cleanup($testConnector)
     }
    }
    ##########################################################################################
    # Delete a connector's Subscription
    ##########################################################################################
    function Delete-Subscription([String] $name)
    {
      foreach($testconnector in $admin.GetMonitoringConnectors())
      {
      if($testconnector.Name -eq $name)
      {
        $connector = Get-SCOMConnector -id $testconnector.id
      }
    }
    $subs = $admin.GetConnectorSubscriptions()
    foreach($sub in $subs)
    {
      if($sub.MonitoringConnectorId -eq $connector.id)
      {
        $admin.DeleteConnectorSubscription($admin.GetConnectorSubscription($sub.Id))
      }
     }
    }
    #New-Connector $connectorName
    write-host "Delete-Subscription"
    Delete-Subscription $connectorName
    write-host "Remove-Connector"
    Remove-Connector $connectorName
```

Myöhemmin Jos aiot muodostamista hallinta-ryhmässä OMS työtilaan, tarvitset Tuo uudelleen `Microsoft.SystemCenter.Advisor.Resources.\<Language>\.mpb` management pack tiedoston uusin päivityskokoelma käytetty hallinta-ryhmä.  Voit etsiä tämän tiedoston `%ProgramFiles%\Microsoft System Center 2012` tai `System Center 2012 R2\Operations Manager\Server\Management Packs for Update Rollups` kansio.

## <a name="next-steps"></a>Seuraavat vaiheet

- [Lisää Log Analytics-ratkaisuja ratkaisuvalikoimasta](log-analytics-add-solutions.md) Lisää toimintoja ja kerätä tietoja.
- Jos organisaatiosi käyttää palomuurin tai välityspalvelimen niin, että agenttien vuoksi viestiä Log Analytics-palvelussa [Määritä välityspalvelin ja palomuurin lokin Analytics asetukset](log-analytics-proxy-firewall.md) .
