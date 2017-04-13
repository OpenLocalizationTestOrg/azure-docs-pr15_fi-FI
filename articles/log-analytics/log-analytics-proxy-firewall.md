<properties
    pageTitle="Välityspalvelimen ja palomuurin asetusten määrittäminen Log Analytics | Microsoft Azure"
    description="Määritä välityspalvelin-ja palomuuriasetukset, kun agenttien vuoksi tai OMS palveluja on käytettävä tietyt portit."
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
    ms.topic="get-started-article"
    ms.date="08/23/2016"
    ms.author="banders;magoedte"/>

# <a name="configure-proxy-and-firewall-settings-in-log-analytics"></a>Lokitiedoston Analytics välityspalvelin ja palomuurin asetusten määrittäminen

Määritä välityspalvelin tarvittavat toimenpiteet ja Log Analytics-OMS palomuuriasetukset eroavat käytettäessä Operations Manager ja sen edustajien ja Microsoft seuranta agenttien vuoksi, joka muodostaa palvelimiin. Tutustu seuraaviin osiin, jota käytetään agentti tyypin.

## <a name="configure-proxy-and-firewall-settings-with-the-microsoft-monitoring-agent"></a>Määritä välityspalvelin-ja palomuuriasetukset Microsoft Agent seuranta

Microsoft seuranta Agent muodostaa ja rekisteröidä OMS-palveluun sen on oltava pääsy porttinumero toimialueet ja URL-osoitteet. Jos käytät välityspalvelinta agentti ja OMS-palvelun väliseen viestintään, tarvitset varmistaa, että tarvittavat resurssit ovat käytettävissä. Jos käytät palomuuri käytön rajoittaminen Internetissä, haluat määrittää palomuurin sallimaan access OMS. Seuraavissa taulukoissa luetellaan OMS tarvitsemat portit.

|**Agentti resurssi**|**Portit**|**Ohita HTTPS tarkastus**|
|--------------|-----|--------------|
|\*. ods.opinsights.azure.com|443|Kyllä|
|\*. oms.opinsights.azure.com|443|Kyllä|
|\*. blob.core.windows.net|443|Kyllä|
|ods.systemcenteradvisor.com|443| |

Seuraavien ohjeiden avulla voit Ohjauspaneelista Microsoft seuranta Agent välityspalvelimen asetusten määrittäminen. Tarvitset toimimalla kullekin palvelimelle. Jos sinulla on useita palvelimissa, jotka sinun on määritettävä, voit ehkä helpompi komentosarjan avulla voit automatisoida tämän. Jos näin on, katso seuraava menettely [komentosarjan avulla Microsoft seuranta Agent välityspalvelimen asetusten määrittäminen](#to-configure-proxy-settings-for-the-microsoft-monitoring-agent-using-a-script).

### <a name="to-configure-proxy-settings-for-the-microsoft-monitoring-agent-using-control-panel"></a>Voit määrittää Ohjauspaneelin avulla Microsoft seuranta Agent välityspalvelimen asetukset

1. Avaa **Ohjauspaneeli**.

2. Avaa **Microsoft Agent seuranta**.

3. Valitse **Välityspalvelimen asetukset** -välilehti.<br>  
  ![välityspalvelimen asetukset-välilehti](./media/log-analytics-proxy-firewall/proxy-direct-agent-proxy.png)

4. Valitse **Käytä välityspalvelinta** URL-osoite ja portin numero, jos se on tarpeen mukaan, samalla tavalla kuin esimerkissä. Jos välityspalvelimen-palvelin edellyttää käyttöoikeuden tarkistusta, kirjoita käyttäjänimi ja salasana, välityspalvelimen.

Seuraavien ohjeiden avulla voit luoda PowerShell-komentosarjaa, joita voit suorittaa kunkin agentti, joka muodostaa palvelinten välityspalvelimen asetusten määrittämiseen.

### <a name="to-configure-proxy-settings-for-the-microsoft-monitoring-agent-using-a-script"></a>Komentosarjan avulla Microsoft seuranta Agent Välityspalvelinasetusten määrittäminen

Kopioi seuraava näyte, ympäristön tiedot päivitetään, tallentaa sen tiedostotunniste on PS1 ja suorittamalla komentosarja kussakin tietokoneessa, joka muodostaa OMS-palvelun.

        
    param($ProxyDomainName="http://proxy.contoso.com:80", $cred=(Get-Credential))

    # First we get the Health Service configuration object.  We need to determine if we
    #have the right update rollup with the API we need.  If not, no need to run the rest of the script.
    $healthServiceSettings = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'

    $proxyMethod = $healthServiceSettings | Get-Member -Name 'SetProxyInfo'

    if (!$proxyMethod)
    {
         Write-Output 'Health Service proxy API not present, will not update settings.'
         return
    }

    Write-Output "Clearing proxy settings."
    $healthServiceSettings.SetProxyInfo('', '', '')

    $ProxyUserName = $cred.username

    Write-Output "Setting proxy to $ProxyDomainName with proxy username $ProxyUserName."
    $healthServiceSettings.SetProxyInfo($ProxyDomainName, $ProxyUserName, $cred.GetNetworkCredential().password)
        

## <a name="configure-proxy-and-firewall-settings-with-operations-manager"></a>Määritä välityspalvelin-ja palomuuriasetukset Operations Manager

Yhdistäminen ja rekisteröidä OMS-palvelun Operations Manager hallinta-ryhmän sen on oltava pääsy porttinumerot toimialueet ja URL-osoitteet. Jos käytät välityspalvelinta Operations Manager management server ja OMS-palvelun väliseen viestintään, tarvitset varmistaa, että tarvittavat resurssit ovat käytettävissä. Jos käytät palomuuri käytön rajoittaminen Internetissä, haluat määrittää palomuurin sallimaan access OMS. Vaikka Operations Manager-asiakirjanhallinnan palvelimeen ei ole välityspalvelimen takana, voi olla sen edustajat. Tässä tapauksessa välityspalvelin on määritettävä samalla tavalla kuin tekijöitä ovat ottaminen käyttöön ja Salli suojaus ja hallinta ratkaisun tietojen Hae lähettää OMS WWW-palvelun.

Jotta Operations Manager tekijöiden OMS-palvelun kanssa kommunikoimiseen Operations Manager infrastruktuurin (mukaan lukien agenttien vuoksi) pitäisi olla oikea välityspalvelimen asetukset ja -version. Välityspalvelimen asetusten agenttien vuoksi on määritetty Operations Manager-konsolissa. Version on oltava jokin seuraavista:

- Operations Manager 2012 SP1 Update Rollup 7 tai uudempi versio
- Operations Manager 2012 R2: n Update Rollup 3 tai uudempi


Seuraavissa taulukoissa luetellaan tehtäviin liittyvät portit.

>[AZURE.NOTE] Osa on seuraavissa resursseissa mainita Advisor ja toiminnallisia tietoja, kumpikin OMS aiemmissa versioissa. Luettelon resursseja kuitenkin muuttaa myöhemmin.

Seuraavassa on luettelo agentti resurssit ja portit:<br>

|**Agentti resurssi**|**Portit**|
|--------------|-----|
|\*. ods.opinsights.azure.com|443|
|\*. oms.opinsights.azure.com|443|
|\*.BLOB.Core.Windows.NET/\*|443|
|ods.systemcenteradvisor.com|443|
<br>
Ohessa on luettelo hallinnan palvelimen resurssien ja portit:<br>

|**Palvelimen resurssien hallinta**|**Portit**|**Ohita HTTPS tarkastus**|
|--------------|-----|--------------|
|Service.systemcenteradvisor.com|443| |
|\*. service.opinsights.azure.com|443| |
|\*. blob.core.windows.net|443|Kyllä| 
|Data.systemcenteradvisor.com|443| | 
|ods.systemcenteradvisor.com|443| | 
|\*. ods.opinsights.azure.com|443|Kyllä| 
<br>
Ohessa on luettelo OMS ja Operations Manager konsolin resurssit ja porteista.<br>

|**OMS ja Operations Manager konsolin resurssi**|**Portit**|
|----|----|
|Service.systemcenteradvisor.com|443|
|\*. service.opinsights.azure.com|443|
|\*. live.com|Portti 80 ja 443|
|\*. microsoft.com-sivustosta|Portti 80 ja 443|
|\*. microsoftonline.com|Portti 80 ja 443|
|\*. mms.microsoft.com|Portti 80 ja 443|
|Login.Windows.NET|Portti 80 ja 443|
<br>

Ohjeiden avulla voit rekisteröidä Operations Manager hallinta-ryhmässä OMS-palvelussa. Jos sinulla on communication ongelmia hallinta-ryhmä ja OMS-palvelun välillä, vianmääritys tiedonsiirto OMS-palveluun vahvistus ohjeiden avulla.

### <a name="to-request-exceptions-for-the-oms-service-endpoints"></a>Voit pyytää OMS Palvelupäätepisteet poikkeukset

1. Käytä esitetään aiemmin varmistaa, että Operations Manager management Server tarvittavat resurssit ovat käytettävissä minkä tahansa palomuurit kautta ensimmäisen taulukon tietoja, sinun on ehkä.
2. Käytä tiedot esitetään aiemmin varmistaa, että tarvittavat toiminnot konsoli Operations Manager ja OMS resurssit ovat käytettävissä minkä tahansa palomuurit kautta toisesta taulukosta, sinun on ehkä.
3. Jos käytät Internet Explorerin välityspalvelinta, varmista, että se on määritetty ja toimii oikein. Jos haluat varmistaa, voit avata suojatun WWW-yhteyden (HTTPS), kuten [https://bing.com](https://bing.com). Jos selaimessa ei onnistu suojattuun WWW-yhteyden, sitä todennäköisesti eivät toimi Operations Manager management console-verkkopalvelut pilveen.

### <a name="to-configure-the-proxy-server-in-the-operations-manager-console"></a>Voit määrittää välityspalvelimen Operations Manager-konsolissa

1. Avaa Operations Manager-konsolin ja valitse **hallinta** -työtilassa.

2. Laajenna **Toiminnallisia tiedot**ja valitse sitten **Toiminnallisia tietoyhteyden**.<br>  
    ![Operations Manager OMS yhteys](./media/log-analytics-proxy-firewall/proxy-om01.png)
3. Valitse OMS yhteys-näkymässä **Määrittäminen välityspalvelinta**.<br>  
    ![Operations Manager OMS yhteyden Määritä välityspalvelin](./media/log-analytics-proxy-firewall/proxy-om02.png)
4. Tekstimuodossa toiminnallisia havainnollistamisen ohjatun asetusten: Välityspalvelinta, valitsemalla **Käytä välityspalvelinta käyttämään toiminnallisia tietoja WWW-palvelun**ja kirjoittamalla URL-Osoitteen ja portin numero, esimerkiksi **http://myproxy:80**.<br>  
    ![Operations Manager OMS välityspalvelimen osoite](./media/log-analytics-proxy-firewall/proxy-om03.png)


### <a name="to-specify-credentials-if-the-proxy-server-requires-authentication"></a>Määritä tunnistetiedot, jos välityspalvelimen edellyttää käyttöoikeuden tarkistusta
 Palvelimen käyttäjätietojen ja asetukset on hallitun tietokoneissa, joissa raportoi OMS Välitä. Näissä palvelimissa on oltava *Microsoft System Center Advisor seuranta palvelimen ryhmän*. Tunnistetiedot salataan kunkin ryhmän palvelimen rekisterissä.

1. Avaa Operations Manager-konsolin ja valitse **hallinta** -työtilassa.
2. Valitse **RunAs määritys**- **profiileista**.
3. Avaa **System Center Advisor Suorita kuin profiilin välityspalvelimen** profiili.  
    ![System Center Advisor Suorita kuin välityspalvelimen profiilin kuva](./media/log-analytics-proxy-firewall/proxy-proxyacct1.png)
4. Valitse Suorita kuin profiilin ohjatun toiminnon **Lisää** Suorita nimellä-tilin käyttöä varten. Voit luoda uuden Suorita nimellä-tilin tai Käytä aiemmin luotua tiliä. Tällä tilillä on ole riittäviä käyttöoikeuksia välityspalvelimen kautta.  
    ![Suorita kuin profiilin ohjatun toiminnon kuva](./media/log-analytics-proxy-firewall/proxy-proxyacct2.png)
5. Määritä tili, jota haluat hallita, valitse **valitun luokan, ryhmän tai objektin** Avaa objekti hakukenttään.  
    ![Suorita kuin profiilin ohjatun toiminnon kuva](./media/log-analytics-proxy-firewall/proxy-proxyacct2-1.png)
6. Etsi ja valitse sitten **Microsoft System Center Advisor seuranta palvelimen ryhmä**.  
    ![Objektin haku-ruudun kuva](./media/log-analytics-proxy-firewall/proxy-proxyacct3.png)
7. Valitse **OK** ja sulje Lisää Suorita nimellä tilin ruutuun.  
    ![Suorita kuin profiilin ohjatun toiminnon kuva](./media/log-analytics-proxy-firewall/proxy-proxyacct4.png)
8. Suorita ohjattu ja Tallenna muutokset.  
    ![Suorita kuin profiilin ohjatun toiminnon kuva](./media/log-analytics-proxy-firewall/proxy-proxyacct5.png)


### <a name="to-validate-that-oms-management-packs-are-downloaded"></a>Vahvista, että OMS hallinta paketteja ladataan

Jos olet lisännyt OMS ratkaisuja, voit tarkastella niitä Operations Manager-konsolissa management Pack-pakettien **hallinta**-kohdassa nimellä. Hae *System Center Advisor* löydät ne nopeasti.  
    ![Management Pack-paketit on ladattu](./media/log-analytics-proxy-firewall/proxy-mpdownloaded.png) tai voit myös tarkistaa OMS management Pack for Operations Manager management server Windows PowerShell-komentoa käyttämällä:

    ```
    Get-ScomManagementPack | where {$_.DisplayName -match 'Advisor'} | select Name,DisplayName,Version,KeyToken
    ```

### <a name="to-validate-that-operations-manager-is-sending-data-to-the-oms-service"></a>Vahvista, että Operations Manager lähettää tietoja OMS-palveluun

1. Avaa suorituskyvyn valvonta (perfmon.exe) Operations Manager asiakirjanhallinnan palvelimeen, ja valitse **Suorituskyvyn valvonta**.
2. Valitse **Lisää**ja valitse sitten **Kunto Service Management ryhmät**.
3. Lisää kaikki, jotka alkavat **HTTP**laskureita.  
    ![lisätä laskureita](./media/log-analytics-proxy-firewall/proxy-sendingdata1.png)
4. Jos Operations Manager-määritys on hyvä, näet tehtävän kuntotietojen hallinnan laskureita tapahtumien ja muut tiedot, perusteella, jotka olet lisännyt OMS ja määritetty lokin sivustokokoelman käytännön management Pack-paketit.  
    ![Suorituskyvyn näytön näyttäminen tehtävä](./media/log-analytics-proxy-firewall/proxy-sendingdata2.png)


## <a name="next-steps"></a>Seuraavat vaiheet

- [Lisää Log Analytics-ratkaisuja ratkaisuvalikoimasta](log-analytics-add-solutions.md) Lisää toimintoja ja kerätä tietoja.
- Tutustu [log haut](log-analytics-log-searches.md) haluat tarkastella yksityiskohtaisia tietoja keräämien ratkaisuja.
