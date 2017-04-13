<properties
    pageTitle="Tallenna ja Näytä diagnostiikan tietojen Azuren tallennustilaan | Microsoft Azure"
    description="Azure diagnostiikka tietojen noutaminen Azure varastoon ja tarkastella sitä"
    services="cloud-services"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor="tysonn" />
<tags
    ms.service="cloud-services"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/01/2016"
    ms.author="robb" />

# <a name="store-and-view-diagnostic-data-in-azure-storage"></a>Tallenna ja Näytä diagnostiikan tietojen Azuren tallennustilaan

Diagnostiikkatiedot sitä tallenneta pysyvästi, ellet siirrä se Microsoft Azure-tallennustilan emulaattorin tai Azure-tallennustilan. Kerran tallennustilaan, se voi tarkastella mainitun käytettävissä työkaluilla.

## <a name="specify-a-storage-account"></a>Määritä tallennustilan-tili

Voit määrittää tallennustilan tili, jota haluat käyttää ServiceConfiguration.cscfg-tiedostossa. Tilin tiedot on määritetty yhteysmerkkijonon määritys-asetusta. Seuraavassa esimerkissä esitetään, ohjelma luo uuden pilvipalvelussa projektin Visual Studiossa oletusarvon yhteysmerkkijono:


```
    <ConfigurationSettings>
       <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
    </ConfigurationSettings>
```

Voit muuttaa tämän yhteysmerkkijonon antamaan Azure-tallennustilan tilin tilitiedot.

Azure diagnostiikka käyttää Blob-palvelun tai taulukon palvelun diagnostiikan tiedot, jotka on kerätty tyypin mukaan. Seuraavassa taulukossa, joka on samanlainen tietolähteet ja niiden muodossa.

|Tietolähde|Tallennusmuoto|
|---|---|
|Azure lokit|Taulukko|
|IIS 7.0 lokit|BLOB|
|Azure diagnostiikka infrastruktuurin lokit|Taulukko|
|Pyydä jäljitys lokit epäonnistui|BLOB|
|Windowsin tapahtumalokien|Taulukko|
|Suorituskyvyn laskureita|Taulukko|
|Kaatumisvedokset|BLOB|
|Mukautetun virhelokeja|BLOB|

## <a name="transfer-diagnostic-data"></a>Diagnostiikan tietojen siirtäminen

2,5 SDK: ssa ja uudemmassa versiossa Tiedostonsiirtopyyntö diagnostiikkatiedot voi ilmetä määritystiedosto kautta. Voit siirtää diagnostiikkatiedot tietyin väliajoin määritysten mukaisesti.

Voit pyytää SDK 2.4 ja edellisen diagnostiikan tietojen siirtäminen niin ohjelmallisesti-määritystiedoston avulla. Ohjelmallisesti menetelmän avulla voit tehdä tarvittaessa siirrot.


>[AZURE.IMPORTANT] Kun siirrät diagnostiikkatiedot Azure-tallennustilan tilin, voit maksamaan, joka käyttää diagnostiikan tietojen tallennustilan resurssien kustannukset.

## <a name="store-diagnostic-data"></a>Tallentaa diagnostiikkatiedot

Blob-objektien tai taulukon tallennustilan ja seuraavat nimet on tallennettu lokitiedot:

**Taulukot**

- **WadLogsTable** - koodia Jäljityksen kuuntelutoiminnon kirjoitetut lokitiedot.

- **WADDiagnosticInfrastructureLogsTable** - diagnostiikan näyttö- ja määritystietoja muuttuu.

- **WADDirectoriesTable** – kansioita, jotka diagnostiikan näytön seuranta.  Tämä vaihtoehto sisältää IIS-lokeja, IIS epäonnistui pyynnön lokit ja mukautettuja kansioita.  Blob-lokitiedoston sijainti on määritetty säilö-kentässä ja blob on relativePath antavat-kenttään.  AbsoluuttinenPolku-kenttä osoittaa sijainti ja tiedoston nimi Azure virtuaalikoneen mukaisessa muodossa.

- **WADPerformanceCountersTable** – suorituskyvyn laskureita.

- **WADWindowsEventLogsTable** – Windowsin tapahtumalokien.

**BLOB-objektit**

- **wad-ohjausobjekti-säilö** – (vain SDK 2.4 ja edellisen) sisältää XML-määritysten tiedostoja, joka ohjaa Azure Diagnostiikka.

- **wad iis-failedreqlogfiles** – sisältää tietoja IIS epäonnistui pyytää Kirjaa.

- **iis-publish logfiles wad** – sisältää tietoja IIS Kirjaa.

- **"Mukautettu"** – kansioita, jotka diagnostiikan näytön valvoo määrittäminen perustuu mukautetun säilön.  WADDirectoriesTable määritetään blob-säilö nimi.

## <a name="tools-to-view-diagnostic-data"></a>Voit tarkastella diagnostiikkatiedot Työkalut
Useita työkaluja ovat nähtävissä tiedot, kun se siirretään tallennustilan. Esimerkki:

- Palvelimen Explorer Visual Studiossa - Jos olet asentanut Azure-Työkalut Microsoft Visual Studio voit Azuren tallennustilaan solmu palvelimen Explorerissa voit tarkastella vain luku-Blob-objektien ja taulukkotiedot Azure tallennustilan tileistä. Voit näyttää tietoja paikallisen säilön emulaattorin tililtä ja myös tallennustilan tileistä olet luonut Azure. Lisätietoja on artikkeleissa [selaaminen ja tallennustilaa resurssien hallinta palvelimen Resurssienhallinnassa](../vs-azure-tools-storage-resources-server-explorer-browse-manage.md).

- [Microsoft Azure-tallennustilan Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) on erillinen sovellus, jonka avulla voit helposti käyttää Azuren tallennustilaan tiedoilla Windows, OS x- ja Linux.

- [Azure Management Studiossa](http://www.cerebrata.com/products/azure-management-studio/introduction) sisältää Azure diagnostiikka hallinnan, jonka avulla voit tarkastella, lataa ja diagnostiikka tietojen keräämistä Azure sovellusten hallinta.


## <a name="next-steps"></a>Seuraavat vaiheet

[Jäljitä kulun Azure vianmäärityksen Cloud Services-sovelluksessa](cloud-services-dotnet-diagnostics-trace-flow.md)
