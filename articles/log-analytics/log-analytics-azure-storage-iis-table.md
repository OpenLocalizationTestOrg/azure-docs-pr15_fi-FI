<properties
    pageTitle="Blob-objektien tallennustilaan käyttäminen IIS- ja tallennustilaa tapahtumien | Microsoft Azure"
    description="Log Analytics lukea lokit Azure Services, joka kirjoittaa diagnostiikka taulukkotallennus tai kirjoittaa blob storage IIS-lokeja."
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
    ms.date="10/25/2016"
    ms.author="banders"/>


# <a name="using-blob-storage-for-iis-and-table-storage-for-events"></a>IIS-blob-säiliö ja taulukkotallennus käyttäminen tapahtumille

Log Analytics voivat lukea seuraavista palveluista, kirjoita taulukkotallennus diagnostiikka lokien tai kirjoittaa blob storage IIS-lokeja:

+ Palvelun kangasta klusterit (ennakkoversio)
+ Näennäiskoneiden
+ Web/työntekijä roolit

Ennen kuin lokin Analytics voi kerätä tietoja näiden resurssien, Azure Diagnostiikka on otettava käyttöön.

Kun Diagnostiikka on otettu käyttöön, voit käyttää Azure portaalin tai PowerShell määrittäminen Log Analytics lokien kerääminen.

Azure Diagnostiikka on Azure-laajennus, jonka avulla voit kerätä diagnostiikkatiedot työntekijän rooli, web-roolin tai virtual-tietokoneessa, jossa Azure-tietokannassa. Tiedot on tallennettu Azure-tallennustilan tilin ja sitten voit keräämiä Log Analytics.

Log Azure diagnostiikka näiden lokien kerääminen Analytics-lokit on oltava johonkin seuraavista sijainneista:

| Lokitiedoston tyyppi | Resurssilaji | Sijainti |
| -------- | ------------- | -------- |
| IIS-lokit | Näennäiskoneiden <br> Web-roolit <br> Työntekijän roolit | wad-iis-publish logfiles (Blob Storage) |
| Syslog | Näennäiskoneiden | LinuxsyslogVer2v0 (Table Storage) |
| Palvelun kangasta toiminnallisia tapahtumat | Palvelun kangasta solmujen | WADServiceFabricSystemEventTable |
| Palvelun kangasta luotettava toimija tapahtumat | Palvelun kangasta solmujen | WADServiceFabricReliableActorEventTable |
| Palvelun kangasta luotettavaa palvelun tapahtumat | Palvelun kangasta solmujen | WADServiceFabricReliableServiceEventTable |
| Windowsin tapahtumalokien | Palvelun kangasta solmujen <br> Näennäiskoneiden <br> Web-roolit <br> Työntekijän roolit | WADWindowsEventLogsTable (taulukkotallennus) |
| Windowsin tapahtumien seuranta-lokit | Palvelun kangasta solmujen <br> Näennäiskoneiden <br> Web-roolit <br> Työntekijän roolit | WADETWEventTable (taulukkotallennus) |

>[AZURE.NOTE] IIS-lokeja Azure verkkosivuilta eivät ole tuettuja.

Näennäiskoneiden voit halutessasi asentaa [Log Analytics-agentti](log-analytics-azure-vm-extension.md) kyselyjä virtuaalikoneen käyttöön muita tietoja. Lisäksi, että voit analysoida IIS-lokit ja tapahtumalokien, voit tehdä muita analysointi esimerkiksi määritysten muutosten jäljityksen SQL arviointi ja Päivitä arviointi.

## <a name="enable-azure-diagnostics-in-a-virtual-machine-for-event-log-and-iis-log-collection"></a>Ota käyttöön Azure diagnostiikka virtual machine-varten tapahtumaloki ja IIS Kirjaudu sivustokokoelman

Käyttöön Azure diagnostiikka-käyttöympäristön tapahtumaloki ja IIS log-sivustokokoelman Microsoft Azure-portaalissa seuraavien ohjeiden avulla.

### <a name="to-enable-azure-diagnostics-in-a-virtual-machine-with-the-azure-portal"></a>Jos haluat ottaa käyttöön Azure diagnostiikka-virtual koneen Azure-portaalissa

1. Asenna AM-agentti, kun luot virtual machine. Jos virtuaalikoneen on jo olemassa, tarkista, että AM-agentti on jo asennettu.
    - Azure-portaalissa virtuaalikoneen Siirry valitsemalla **Valinnainen määrittäminen**ja **vianmääritys** ja Aseta **tilaksi** **käyttöön**.

    Valmistumisen AM Azure diagnostiikka-tunniste asennettu ja käytössä on. Tämä tunniste on vastuussa diagnostiikka tietojen keräämisen.

2. Seurannan ottaminen käyttöön ja määrittäminen tapahtumien kirjaamisen käyttöön aiemmin AM. Voit ottaa diagnostiikka AM tasolla. Diagnostiikan ottaminen käyttöön ja määrittänyt tapahtumalokiin kirjaaminen, toimi seuraavasti:
    1. Valitse AM.
    2. Valitse **Seuranta**.
    3. Valitse **Diagnostiikka**.
    4. Aseta **tila** **on**.
    5. Valitse kunkin diagnostiikka lokin, jotka haluat kerätä.
    7. Valitse **OK**.

Azure-PowerShellin avulla voit määrittää tarkemmin tapahtumat, jotka on kirjoitettu Azure-tallennustilan. Lisätietoja on [kirjoitettu taulukkotallennus tai kirjoittaa blob IIS-lokeja Azure Diagnostiikan avulla tietojen kerääminen](log-analytics-azure-storage-json.md).


## <a name="enable-azure-diagnostics-in-a-web-role-for-iis-log-and-event-collection"></a>Ota käyttöön Azure Diagnostiikka-Web-roolin IIS log ja tapahtuman keräämistä varten

Lisätietoja [Siitä, miten voit ottaa käyttöön diagnostiikka pilvipalvelussa](../cloud-services/cloud-services-dotnet-diagnostics.md) n yleisohjeita Azure diagnostiikka ottamisesta käyttöön. Alla olevia ohjeita käyttää näitä tietoja, ja mukauta sitä Log Analytics käytettäväksi.

Azure vianmäärityksen käytössä:

- IIS-lokeja tallennetaan oletusarvoisesti lokitiedot siirretty scheduledTransferPeriod siirto mukaisin väliajoin.
- Windowsin tapahtumalokien eivät säily oletusarvoisesti.

### <a name="to-enable-diagnostics"></a>Jos haluat ottaa käyttöön vianmääritys

Käyttöön Windowsin tapahtumalokien tai muuttaa scheduledTransferPeriod, Määritä Azure diagnostiikka XML-määritysten käyttäminen (diagnostics.wadcfg) mukaisesti [Vaihe 4: Luo diagnostiikka-kokoonpanotiedosto ja asenna laajennus](../cloud-services/cloud-services-dotnet-diagnostics.md)

Esimerkki kokoonpanotiedosto kerää sovelluksen ja järjestelmän lokit IIS-lokit ja kaikki tapahtumat:

```
    <?xml version="1.0" encoding="utf-8" ?>
    <DiagnosticMonitorConfiguration xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration"
          configurationChangePollInterval="PT1M"
          overallQuotaInMB="4096">

      <Directories bufferQuotaInMB="0"
         scheduledTransferPeriod="PT10M">  
        <!-- IISLogs are only relevant to Web roles -->
        <IISLogs container="wad-iis" directoryQuotaInMB="0" />
      </Directories>

      <WindowsEventLog bufferQuotaInMB="0"
         scheduledTransferLogLevelFilter="Verbose"
         scheduledTransferPeriod="PT10M">
        <DataSource name="Application!*" />
        <DataSource name="System!*" />
      </WindowsEventLog>

    </DiagnosticMonitorConfiguration>
```

Varmista, että ConfigurationSettings määrittää tallennustilan tilin, kuten seuraavassa esimerkissä:

```
    <ConfigurationSettings>
       <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="DefaultEndpointsProtocol=https;AccountName=<AccountName>;AccountKey=<AccountKey>"/>
    </ConfigurationSettings>
```

**AccountName** ja **AccountKey** arvoja löytyy tallennustilan tilin koontinäytön-kohdassa Hallitse pikanäppäinten Azure-portaalissa. Yhteysmerkkijonon protokolla on oltava **https**.

Kun päivitetty diagnostiikan asetuksia käytetään pilvipalvelussa sijaitsevaan ja se on kirjoittaminen diagnostiikka Azure-tallennustilan, valitse olet valmis määrittäminen Log Analytics.


## <a name="use-the-azure-portal-to-collect-logs-from-azure-storage"></a>Lokien kerääminen Azuren tallennustilaan Azure portaalin avulla

Voit käyttää Azure portaalin määrittäminen Log Analytics Azure seuraavat palvelut lokien kerääminen:

+ Palvelun kangasta klustereiden
+ Näennäiskoneiden
+ Web/työntekijä roolit

Azure-portaalissa Siirry Log Analytics-työtilan ja suorittaa seuraavia tehtäviä:

1. Valitse *tallennustilan tilit lokit*
2. *Lisää* tehtävä
3. Valitse tallennustilan tili, joka sisältää diagnostiikka-lokit
  - Tämän tilin voi olla perinteinen tallennustilan tilin tai Azure Resurssienhallinta-tallennustilan tilin
4. Haluat kerätä lokit tietotyyppi
  - Vaihtoehdot ovat IIS-lokeja; Tapahtumien; Syslog (Linux); Tapahtumien seuranta lokit; Palvelun kangasta tapahtumat
5. Tietolähteen arvo täytetään automaattisesti tietotyypin mukaan eikä niitä voi muuttaa
6. Tallenna määritykset valitsemalla OK

Toista vaiheet 2 – 6 lisätallennustilaa asiakkaat ja tietotyyppejä, jotka haluat kerätä Analytics Log.

Noin 30 minuutin kuluttua olet näe Log Analytics tallennustilan tilin tiedot. Näkyviin tulee vain tietoja, jotka on kirjoitettu säilöön, kun määritykset on otettu käyttöön. Lokitiedoston Analytics ei lue aiemmin luotuja tietoja tallennustilan-tililtä.

>[AZURE.NOTE] Portaalin ei tarkista, että lähde on tallennustilan-tili tai jos uudet tiedot kirjoitetaan.

## <a name="enable-azure-diagnostics-in-a-virtual-machine-for-event-log-and-iis-log-collection-using-powershell"></a>Ota käyttöön Azure diagnostiikka virtual machine-varten tapahtumaloki ja IIS Kirjaudu sivustokokoelman PowerShellin avulla

[Log Analytics määrittäminen indeksoimaan Azure diagnostiikka](log-analytics-powershell-workspace-configuration.md#configuring-log-analytics-to-index-azure-diagnostics) vaiheiden avulla voit lukea Azure vianmääritys, joka on kirjoitettu taulukkotallennus PowerShellin avulla.

Azure-PowerShellin avulla voit määrittää tarkemmin tapahtumat, jotka on kirjoitettu Azure-tallennustilan.
Lisätietoja on artikkelissa [Ottaminen käyttöön diagnostiikka Azuren näennäiskoneiden](../virtual-machines-dotnet-diagnostics.md).

Voit ottaa käyttöön ja päivittää Azure diagnostiikka käyttämällä seuraavaa PowerShell-komentosarjaa.
Voit käyttää myös tämän komentosarjan mukautetun kirjaaminen kokoonpanon.
Muokkaa komentosarja-tallennustilan tilin, palvelunimi ja virtuaalikoneen nimi.
Komentosarja käyttää cmdlet-komennot perinteinen näennäiskoneiden.

Tarkista seuraava komentosarja-malli, kopioi se, muokkaa sitä haluamallasi tavalla, otosten tallentaminen PowerShell-komentosarjatiedosto ja suorittamalla komentosarja.

```
    #Connect to Azure
    Add-AzureAccount

    # settings to change:
    $wad_storage_account_name = "myStorageAccount"
    $service_name = "myService"
    $vm_name = "myVM"

    #Construct Azure Diagnostics public config and convert to config format

    # Collect just system error events:
    $wad_xml_config = "<WadCfg><DiagnosticMonitorConfiguration><WindowsEventLog scheduledTransferPeriod=""PT1M""><DataSource name=""System!* "" /></WindowsEventLog></DiagnosticMonitorConfiguration></WadCfg>"

    $wad_b64_config = [System.Convert]::ToBase64String([System.Text.Encoding]::UTF8.GetBytes($wad_xml_config))
    $wad_public_config = [string]::Format("{{""xmlCfg"":""{0}""}}",$wad_b64_config)

    #Construct Azure diagnostics private config

    $wad_storage_account_key = (Get-AzureStorageKey $wad_storage_account_name).Primary
    $wad_private_config = [string]::Format("{{""storageAccountName"":""{0}"",""storageAccountKey"":""{1}""}}",$wad_storage_account_name,$wad_storage_account_key)

    #Enable Diagnostics Extension for Virtual Machine

    $wad_extension_name = "IaaSDiagnostics"
    $wad_publisher = "Microsoft.Azure.Diagnostics"
    $wad_version = (Get-AzureVMAvailableExtension -Publisher $wad_publisher -ExtensionName $wad_extension_name).Version # Gets latest version of the extension

    (Get-AzureVM -ServiceName $service_name -Name $vm_name) | Set-AzureVMExtension -ExtensionName $wad_extension_name -Publisher $wad_publisher -PublicConfiguration $wad_public_config -PrivateConfiguration $wad_private_config -Version $wad_version | Update-AzureVM
```


## <a name="next-steps"></a>Seuraavat vaiheet

- [Käytä JSON tiedostot Blob-objektien tallennustilaan](log-analytics-azure-storage-json.md) lokit lukea Azure services, kirjoita vianmääritys ja Blob-objektien tallennustilaan JSON-muodossa.
- [Ota käyttöön ratkaisuja](log-analytics-add-solutions.md) selventäminen tiedot.
- [Käytä hakukyselyt](log-analytics-log-searches.md) tietojen analysoimista varten.
