<properties
   pageTitle="Azure log integrointi usein kysytyt kysymykset | Microsoft Azure"
   description="Nämä usein kysytyt kysymykset vastauksia kysymyksiin Azure log integrointi."
   services="security"
   documentationCenter="na"
   authors="TomShinder"
   manager="MBaldwin"
   editor="TerryLanfear"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/23/2016"
   ms.author="TomSh"/>

# <a name="azure-log-integration-frequently-asked-questions-faq"></a>Usein kysyttyjä kysymyksiä Azure log-integrointi

Nämä usein kysytyt kysymykset vastauksia kysymyksiin Azure log-integrointi, palvelu, joka mahdollistaa raaka lokit Azure resurssien integroida paikallisen suojaustietojen ja tapahtuman Management (SIEM) järjestelmien. Integrointi on yhdistetty Raporttinäkymät-ikkunan kaikki oman varat paikallisen tai pilvipalvelussa, niin, että voit koostaa, yhdistää, analysoida ja ilmoita suojauksen tapahtumien liittyvät sovellukset.

## <a name="how-can-i-see-the-storage-accounts-from-which-azure-log-integration-is-pulling-azure-vm-logs-from"></a>Miten voin nähdä tallennustilan tilit, josta Azure log integrointi on tietojen Azure AM lokit?

Suorita-komennon **azlog lähde-luettelosta**.

## <a name="how-can-i-update-the-proxy-configuration"></a>Miten voin päivittää välityspalvelimen määritykset?

Jos välityspalvelimen asetuksia ei salli tallennustilan Azure access suoraan, Avaa **AZLOG. KOMENTORIVIVALITSIMET. CONFIG** **c:\Program Files\Microsoft Azure Log integrointi**tiedoston. Päivitä tiedosto, joka sisältää organisaation välityspalvelimen osoite **defaultProxy** -osassa. Kun päivitys on valmis, Lopeta ja Käynnistä komennot **nettonykyarvon stop azlog** ja **nettonykyarvon Käynnistä azlog**palvelu.

    <?xml version="1.0" encoding="utf-8"?>
    <configuration>
      <system.net>
        <connectionManagement>
          <add address="*" maxconnection="400" />
        </connectionManagement>
        <defaultProxy>
          <proxy usesystemdefault="true"
          proxyaddress=http://127.0.0.1:8888
          bypassonlocal="true" />
        </defaultProxy>
      </system.net>
      <system.diagnostics>
        <performanceCounters filemappingsize="20971520" />
      </system.diagnostics>   

## <a name="how-can-i-see-the-subscription-information-in-windows-events"></a>Miten voin nähdä Windowsin tapahtumien tilauksen tiedot?

Liitä **subscriptionid** kutsumanimi samalla, kun lähteen lisääminen.

    Azlog source add <sourcefriendlyname>.<subscription id> <StorageName> <StorageKey>  

Tapahtuman XML on metatiedot alla kuvatulla tavalla, mukaan lukien tilauksen tunnus.

![Tapahtuman XML][1]

## <a name="error-messages"></a>Virhesanomat

### <a name="when-running-command-azlog-createazureid-why-do-i-get-the-following-error"></a>Kun käynnissä komento **azlog createazureid**, miksi näyttöön tulee seuraava virhe?

Virhe:

  *AAD - sovelluksen luominen epäonnistui vuokraaja 72f988bf-86f1-41af-91ab-2d7cd011db37 syy = "Käyttö estetty" - viesti = "Oikeudet eivät riitä toiminnon suorittamiseksi."*

**Azlog createazureid** yrittää palvelun lyhennys luominen kaikki Azure AD-alihallinnat tilaukset, joina Azure sisäänkirjautuminen on käyttöoikeus. Jos Azure sisäänkirjautuminen on vain kyseisen Azure AD-vuokraajan Vieras-käyttäjän, valitse komento epäonnistuu "Oikeudet eivät riitä toiminnon suorittamiseksi." Pyydä vuokraajan järjestelmänvalvoja lisää tilisi vuokraajan käyttäjänä.

### <a name="when-running-command-azlog-authorize-why-do-i-get-the-following-error"></a>Kun käynnissä komento **azlog sallivat**, miksi näyttöön tulee seuraava virhe?

Virhe:

  *Varoitus luominen roolimääritys - AuthorizationFailed: asiakkaan janedo@microsoft.com' objektin tunnus "fe9e03e4-4dad-4328-910f-fd24a9660bd2" ei ole lupa, toiminto 'Microsoft.Authorization/roleAssignments/write' laajuus '/ tilaukset/70 95299-d689-4c d 97-b971-0d8ff0000000' päälle.*

**Azlog sallivat** komento määrittää lukijan rooli pääasiallista Azure AD-palveluun (luotu **Azlog createazureid**) annettu tilaukset. Jos Azure kirjautuminen ei ole muiden järjestelmänvalvojan tai omistajan tilauksen, se epäonnistuu luvan epäonnistui-viestin. Tämä toiminto suoritetaan tarvitaan tiedostojen järjestelmänvalvojan tai omistajan Azure Roolipohjainen käyttöoikeuksien hallinnan (RBAC).

## <a name="where-can-i-find-the-definition-of-the-properties-in-audit-log"></a>Mistä löydän ominaisuuksien määritys-valvontaloki?

Lisätietoja:

- [Valvonta-toimintojen resurssien hallinta](../resource-group-audit.md)
- [Luettelo Azure näytön REST API-tilauksen hallinta-tapahtumat](https://msdn.microsoft.com/library/azure/dn931934.aspx)

## <a name="where-can-i-find-details-on-azure-security-center-alerts"></a>Mistä löydän lisätietoja Azure Tietoturvakeskus ilmoituksille?

Katso [hallinta ja niihin vastaaminen suojausvaroitusten Azure Tietoturvakeskuksessa](../security-center/security-center-managing-and-responding-alerts.md).

## <a name="how-can-i-modify-what-is-collected-with-vm-diagnostics"></a>Miten voin muokata mitä kerätään AM vianmäärityksen?

[PowerShellin Azure diagnostiikka-käyttöjärjestelmässä virtual koneen käyttöön](../virtual-machines/virtual-machines-windows-ps-extensions-diagnostics.md) lisätietoja siitä, miten voit Get-, Muokkaa, ja määritä *(WAD)* Windows Azure-diagnostiikka määritys. Seuraavassa on esimerkki:

### <a name="get-the-wad-config"></a>Hae WAD config

    -AzureRmVMDiagnosticsExtension -ResourceGroupName AzLog-Integration -VMName AzlogClient
    $publicsettings = (Get-AzureRmVMDiagnosticsExtension -ResourceGroupName AzLog-Integration -VMName AzlogClient).PublicSettings
    $encodedconfig = (ConvertFrom-Json -InputObject $publicsettings).xmlCfg
    $xmlconfig = [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($encodedconfig))
    Write-Host $xmlconfig

    $xmlconfig | Out-File -Encoding utf8 -FilePath "d:\WADConfig.xml"

### <a name="modify-the-wad-config"></a>Muokkaa WAD Config

Seuraavassa esimerkissä on kokoonpano, jossa EventID 4624 ja EventId 4625 kerätään suojauksen tapahtumalokin. Microsoft Antimalware tapahtumien kerätään järjestelmän tapahtumalokiin. Katso [muissa tapahtumien] (https://msdn.microsoft.com/library/windows/desktop/dd996910 (v=vs.85) lisätietoja XPath-lausekkeiden käyttö.

    <WindowsEventLog scheduledTransferPeriod="PT1M">
        <DataSource name="Security!*[System[(EventID=4624 or EventID=4625)]]" />
        <DataSource name="System!*[System[Provider[@Name='Microsoft Antimalware']]]"/>
    </WindowsEventLog>

### <a name="set-the-wad-configuration"></a>WAD määrittäminen

    $diagnosticsconfig_path = "d:\WADConfig.xml"
    Set-AzureRmVMDiagnosticsExtension -ResourceGroupName AzLog-Integration -VMName AzlogClient -DiagnosticsConfigurationPath $diagnosticsconfig_path -StorageAccountName log3121 -StorageAccountKey <storage key>

Kun olet tehnyt muutokset, tarkista varmistaa, että oikea tapahtumat kerätään tallennustilan-tili.

Jos sinulla on kysyttävää Azure Log integrointi, Lähetä sähköpostia [AzSIEMteam@microsoft.com](mailto:AzSIEMteam@microsoft.com)

<!--Image references-->
[1]: ./media/security-azure-log-integration-faq/event-xml.png
