<properties
    pageTitle="Azure avaimen säilö kirjaaminen | Microsoft Azure"
    description="Tässä opetusohjelmassa käyttäminen helpottaa aloittaminen Azure avaimen säilö lokiin kirjaaminen."
    services="key-vault"
    documentationCenter=""
    authors="cabailey"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="08/31/2016"
    ms.author="cabailey"/>

# <a name="azure-key-vault-logging"></a>Azure avaimen säilö kirjaaminen #
Azure avaimen säilö on käytettävissä useimmilla alueilla. Lisätietoja on artikkelissa [avaimen säilö hinnat sivun](https://azure.microsoft.com/pricing/details/key-vault/).

## <a name="introduction"></a>Johdanto  
Kun olet luonut vähintään yksi tärkeimmistä vaults, haluat todennäköisesti seurannassa milloin ja miten tärkeimmät vaults on käyttää, ja kuka. Voit tehdä tämän avaimen säilö, joka tallentaa tiedot, jotka saadaan Azure-tallennustilan tilin kirjaamisen käyttöönottaminen. Uusi säilö **tiedot-lokit-auditevent** luodaan automaattisesti määritetyn tallennustilan tilin ja voit käyttää tätä saman tallennustilan tilin useita keskeisiä vaults lokien kerääminen.

Voit käyttää lokiin kirjaaminen on enintään-avain 10 minuutin säilöön-toiminto. Useimmissa tapauksissa se on nopeampaa kuin tämä.  Asiat, voit hallita tallennustilan tilisi lokit:

- Vakio Azure access menetelmät avulla voit suojata lokeista rajoittamalla sitä, kuka voi käyttää niitä.
- Poista lokit, et enää halua säilyttää tallennustilan tilisi.

Tässä opetusohjelmassa avulla pääset alkuun Azure avaimen säilö kirjaaminen, Luo uusi tallennustilan ja ottaa kirjaamisen käyttöön tulkita kirjaaminen kerättyjä tietoja.  


>[AZURE.NOTE]  Tässä opetusohjelmassa ei ole ohjeita avaimen vaults, avaimet tai tietoja luomiseen. Tämä on artikkelissa [Azure avaimen säilö käytön aloittaminen](key-vault-get-started.md). Tai katso ohjeet Office kaikissa ympäristöissä käyttöliittymä [vastaavat tässä opetusohjelmassa](key-vault-manage-with-cli.md).
>
>Tällä hetkellä et voi määrittää Azure avaimen säilö Azure-portaalissa. Käytä näitä Azure PowerShell-ohjeita.

Lokit, jotka olet kerännyt voidaan näyttää lokin analytics-toimintojen hallinta-ohjelmistopaketin avulla. Lisätietoja on artikkelissa [Azure avaimen säilö (esikatselu)-ratkaisun Log Analytics](../log-analytics/log-analytics-azure-key-vault.md).

Yleistietoja Azure avaimen säilö, katso [Azure avaimen säilö ominaisuudet?](key-vault-whatis.md)

## <a name="prerequisites"></a>Edellytykset

Jotta voit suorittaa tässä opetusohjelmassa, sinulla on oltava seuraavasti:

- Aiemmin luodun avaimen säilö, jota käytät.  
- Azure PowerShell- **version minimivaatimus, 1.0.1**. Voit asentaa PowerShellin Azure ja liittää sen Azure-tilaus, katso, [miten voit asentaa ja määrittää PowerShellin Azure](../powershell-install-configure.md). Jos olet jo asentanut PowerShellin Azure ja et tiedä versioksi PowerShellin Azure-konsolin, kirjoita `(Get-Module azure -ListAvailable).Version`.  
- Riittävän tallennustilan Azure-avain säilö lokitiedot.


## <a id="connect"></a>Tilausten yhdistäminen ##

PowerShellin Azure-istunnon aloittaminen ja kirjaudu sisään Azure tiliisi seuraavalla komennolla:  

    Login-AzureRmAccount

Kirjoita ponnahdusikkunoiden selainikkunassa Azure tilin käyttäjänimi ja salasana. Azure PowerShell saavat kaikki haluamasi tilaukset, jotka liittyvät tällä tilillä ja oletusarvoisesti, käyttää ensimmäistä.

Jos sinulla on useita tilauksia, voit joutua määrittämään tarkasti, joka luotiin Azure-avain säilö. Kirjoita näet tilisi tilaukset:

    Get-AzureRmSubscription

Tämän jälkeen voit määrittää tilaus, johon on liitetty oman avaimen säilö logging kirjoittamalla

    Set-AzureRmContext -SubscriptionId <subscription ID>

Lisätietoja PowerShellin Azure Katso, [miten asennetaan ja määritetään PowerShellin Azure](../powershell-install-configure.md).


## <a id="storage"></a>Luo uuden tallennustilan tilin lokit ##

Vaikka voit käyttää käytössä olevan tallennustilan tilin lokeista, emme Luo uusi tallennustilan-tili, joka on varattu avaimen säilö lokit. Asiasta, kun haluat määrittää myöhemmin on tiedot tallennetaan nimeltä **sa**muuttujaksi.

Lisää hallinnan helpottamiseksi emme käytetään myös saman resurssiryhmän jota, joka sisältää sekä avaimen säilö. [Aloitusopas](key-vault-get-started.md)tämän resurssiryhmän nimeltä **ContosoResourceGroup** ja on edelleen käyttää Itä-Aasian sijaintia. Korvaa arvot omaan tilanteen mukaan:

    $sa = New-AzureRmStorageAccount -ResourceGroupName ContosoResourceGroup -Name ContosoKeyVaultLogs -Type Standard_LRS -Location 'East Asia'


>[AZURE.NOTE]  Jos päätät käyttää käytössä olevan tallennustilan tilin, se on käytettävä samaa tilauksen avaimen säilö ja se on käytettävä resurssien hallinnan käyttöönottomalli perinteinen käyttöönotto-malli sijaan.

## <a id="identify"></a>Määritä avaimen säilö lokeista varten ##

Tutustu hakeminen aloittaminen opetusohjelmassa avaimen säilö nimeä ollut **ContosoKeyVault**, joten on edelleen käyttää samaa nimeä ja tallentaa tiedot nimeltä **kv**muuttujaksi:

    $kv = Get-AzureRmKeyVault -VaultName 'ContosoKeyVault'


## <a id="enable"></a>Kirjaamisen ottaminen käyttöön ##

Voit ottaa lokiin kirjaamisen käyttöön avaimen säilö, Käytämme määrittäminen AzureRmDiagnosticSetting cmdlet-ja tutustu uuden tallennustilan tilin ja tutustu avaimen säilö luomaasi muuttujat. Microsoft myös määrittää **-käytössä** **$true** merkintä ja Määritä luokka AuditEvent (vain luokka avaimen säilö kirjaaminen),:


    Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent

Tämä tulos sisältää:

    StorageAccountId   : /subscriptions/<subscription-GUID>/resourceGroups/ContosoResourceGroup/providers/Microsoft.Storage/storageAccounts/ContosoKeyVaultLogs
    ServiceBusRuleId   :
    StorageAccountName :
        Logs
        Enabled           : True
        Category          : AuditEvent
        RetentionPolicy
        Enabled : False
        Days    : 0


Tämä tarkoittaa, että kirjaaminen on otettu käyttöön avaimen säilö, tietojen tallentaminen tallennustilan-tiliisi.

Halutessasi voit myös määrittää säilytyskäytäntö lokeista siten, että vanhat lokit poistetaan automaattisesti. Esimerkiksi säilytyskäytännön käyttäminen **$true** **- RetentionEnabled** -merkinnän määrittäminen ja **- RetentionInDays** parametrin arvoksi **90** , niin, että 90 päivää vanhemmat lokit poistetaan automaattisesti.

    Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent -RetentionEnabled $true -RetentionInDays 90

Mikä on kirjautunut:

- Kaikki todennetut REST API-pyynnöt kirjautunut, joka sisältää epäonnistuneiden pyyntöjen käyttöoikeudet, järjestelmävirheiden tai virheelliset pyynnöt.
- Toimintojen avaimen vault itse, joka sisältää luominen, poistaminen, asetus avaimen säilö käytäntöjen, ja avaimen säilö määritteiden päivittäminen kuten tunnisteita.
- Toimenpiteet näppäimet ja avaimen säilö, joka sisältää luominen, muokkaaminen tai poistaminen nämä avaimet tai tietoja, tietoja Toiminnot, kuten merkki, tarkista salaa salauksen, Rivitä ja poistaa rivityksen näppäimet, saat tietoja, luettelossa näppäimiä ja tietoja ja niiden versiot.
- Todentamattomille pyynnöt, joka johtaa 401 vastausta. Esimerkiksi pyynnöt, joka ei ole haltijatunnukseen, tai on virheellinen tai vanhentunut tai on virheellinen tunnus.  


## <a id="access"></a>Käyttää lokit ##

Avaimen säilö lokit tallennetaan antamasi tallennustilan tilin **tiedot-lokit-auditevent** -säilö. Kirjoita kaikki tämän säilön BLOB-luettelosta:

    Get-AzureStorageBlob -Container 'insights-logs-auditevent' -Context $sa.Context

Tulos näyttää jotakin tämän tapaista:

**Säilön Uri: https://contosokeyvaultlogs.blob.core.windows.net/insights-logs-auditevent**


**Nimi**

**----**

**resourceId = / tilaukset/361DA5D4-A47A – 4C 79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/TARJOAJIEN/MICROSOFT. KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=05/h=01/m=00/PT1H.json**

**resourceId = / tilaukset/361DA5D4-A47A – 4C 79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/TARJOAJIEN/MICROSOFT. KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=04/h=02/m=00/PT1H.json**

** resourceId = / tilaukset/361DA5D4-A47A – 4C 79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/TARJOAJIEN/MICROSOFT. KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=04/h=18/m=00/PT1H.json***


Kuten näet tämän tuloste BLOB-objektit noudattaa tiettyä nimeämiskäytäntöä: **resourceId =<ARM resource ID>/y =<year>/m =<month>/d =<day of month>/h =<hour>/m =<minute>/filename.json**

Päivämäärä ja kellonaika-arvoja käytetään UTC-aika.

Saman tallennustilan tilin voidaan käyttää useiden resurssien lokien kerääminen koko Resurssitunnus Blob-objektien nimissä on erittäin hyödyllinen tai ladata vain BLOB, jotka on. Mutta ennen on, että käsittelemme ensin kaikki Blob-objektien lataamisesta.

Luo kansio, johon ladataan BLOB-objektit. Esimerkki:

    New-Item -Path 'C:\Users\username\ContosoKeyVaultLogs' -ItemType Directory -Force

Saat kaikki Blob-objektien luettelo:  

    $blobs = Get-AzureStorageBlob -Container $container -Context $sa.Context

Pipe tämän luettelon "Get-AzureStorageBlobContent' lataaminen BLOB-objektit Microsoftin kohdekansio kautta:

    $blobs | Get-AzureStorageBlobContent -Destination 'C:\Users\username\ContosoKeyVaultLogs'

Kun suoritat tämän toisen komennon **/** erottimen Blob-objektien nimissä luominen koko kansiorakenne Valitse kohdekansio ja rakenteen käytetään voit ladata ja tallentaa BLOB-tiedostoina.

Voit ladata valikoivasti BLOB-yleismerkkejä. Esimerkki:

- Jos on useita keskeisiä vaults ja haluat ladata yhden avaimen säilö lokit-niminen CONTOSOKEYVAULT3:

        Get-AzureStorageBlob -Container $container -Context $sa.Context -Blob '*/VAULTS/CONTOSOKEYVAULT3

- Jos sinulla on useita resurssiryhmiä ja haluat ladata yhden resurssiryhmä lokit- `-Blob '*/RESOURCEGROUPS/<resource group name>/*'`:

        Get-AzureStorageBlob -Container $container -Context $sa.Context -Blob '*/RESOURCEGROUPS/CONTOSORESOURCEGROUP3/*'

- Jos haluat ladata kaikki lokit tammikuussa 2016 kuukauden, käytä `-Blob '*/year=2016/m=01/*'`:

        Get-AzureStorageBlob -Container $container -Context $sa.Context -Blob '*/year=2016/m=01/*'

Olet nyt valmiina aloittamaan katsoo lokit ominaisuudet. Mutta ennen siirtämistä, kaksi Get-AzureRmDiagnosticSetting, sinun tarvitsee tietää lisää parametrit sivulle:

- Kyselyn diagnostiikan asetusten avaimen säilö resurssin tila:`Get-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId`

- Voit poistaa käytöstä kirjaaminen, jotta avaimen säilö resurssi:`Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $false -Categories AuditEvent`


## <a id="interpret"></a>Tässä artikkelissa avain säilöön-lokit ##

Yksittäisten BLOB tallennetaan JSON Blob-objektien muotoiltu teksti. Tämä on esimerkki-lokitapahtuman suoritus `Get-AzureRmKeyVault -VaultName 'contosokeyvault'`:

    {
        "records":
        [
            {
                "time": "2016-01-05T01:32:01.2691226Z",
                "resourceId": "/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSOGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT",
                "operationName": "VaultGet",
                "operationVersion": "2015-06-01",
                "category": "AuditEvent",
                "resultType": "Success",
                "resultSignature": "OK",
                "resultDescription": "",
                "durationMs": "78",
                "callerIpAddress": "104.40.82.76",
                "correlationId": "",
                "identity": {"claim":{"http://schemas.microsoft.com/identity/claims/objectidentifier":"d9da5048-2737-4770-bd64-XXXXXXXXXXXX","http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn":"live.com#username@outlook.com","appid":"1950a258-227b-4e31-a9cf-XXXXXXXXXXXX"}},
                "properties": {"clientInfo":"azure-resource-manager/2.0","requestUri":"https://control-prod-wus.vaultcore.azure.net/subscriptions/361da5d4-a47a-4c79-afdd-XXXXXXXXXXXX/resourcegroups/contosoresourcegroup/providers/Microsoft.KeyVault/vaults/contosokeyvault?api-version=2015-06-01","id":"https://contosokeyvault.vault.azure.net/","httpStatusCode":200}
            }
        ]
    }


Seuraavassa taulukossa on lueteltu kenttien nimiä ja kuvauksia.


| Kenttänimi        | Kuvaus |
| ------------- |-------------|
| aika      | Päivämäärä ja kellonaika (UTC-ajan).|
| resourceId      | Azure Resurssienhallinta resurssin tunnus. Avaimen säilö lokitiedot ei aina avaimen säilö resurssin tunnus.|
| operationName      | Toiminnon suorittimessa seuraavan taulukon nimi.|
| operationVersion      | Tämä on asiakkaan REST API-versio.|
| luokka      | Avaimen säilö lokitiedot AuditEvent on yksi, käytettävissä-arvo.|
| resultType      | REST API pyynnön tulos.|
| resultSignature      | HTTP-tila.|
| resultDescription     | Lisää kuvaus tuloksen, kun niitä on saatavilla.|
| durationMs      | REST API-pyynnön millisekunteina kulunut aika. Tämä ei ole verkon latenssin, jotta mitta asiakaspuolen aika eivät ehkä vastaa tällä hetkellä.|
| callerIpAddress      | Asiakas, joka pyynnön IP-osoite.|
| correlationId      | Valinnainen GUID, asiakas voi välittää yhdistää asiakaspuolen lokien service (avaimen säilö) lokitietoihin.|
| käyttäjätiedot      | Tunnistetiedot, jotka on esitetty, kun REST API-pyynnön tunnuksesta. Tämä on yleensä "käyttäjä", "palvelun lyhennys" tai yhdistelmä "käyttäjän + sovelluksen tunnus" kuin palvelupyynnön tuloksena Azure PowerShell-cmdlet-pyynnön.|
| Ominaisuudet:      | Tämä kenttä sisältää muita tietoja toiminnon (operationName) perusteella. Useimmissa tapauksissa sisältää asiakastietojen (useragent merkkijonon asiakkaan lähettämän) tarkka REST API-pyynnön URI ja HTTP-tilakoodin. Lisäksi kun objektin palautetaan tuloksena pyyntö (esimerkiksi KeyCreate tai VaultGet) se myös sisältää avaimen URI (kuten "tunnus"), säilö URI tai salaisuus URI.|




**OperationName** kenttien arvot ovat ObjectVerb-muodossa. Esimerkki:

- Kaikkien avaimen säilö toimintojen ' säilö`<action>`' muotoilla, kuten `VaultGet` ja `VaultCreate`.

- Kaikkien avaimen toimintojen ' avain`<action>`' muotoilla, kuten `KeySign` ja `KeyList`.

- Kaikkien salainen toimintojen ' salaisuus`<action>`' muotoilla, kuten `SecretGet` ja `SecretListVersions`.

Seuraavassa taulukossa on lueteltu operationName ja REST API-komennon.

| operationName        | REST API-komento |
| ------------- |-------------|
| Todennus      | Azure Active Directory-päätepisteen kautta|
| VaultGet      | [Tietoja avaimen säilö](https://msdn.microsoft.com/en-us/library/azure/mt620026.aspx)|
| VaultPut      | [Luo tai päivitä avaimen säilö](https://msdn.microsoft.com/en-us/library/azure/mt620025.aspx)|
| VaultDelete      | [Poista avaimen säilö](https://msdn.microsoft.com/en-us/library/azure/mt620022.aspx)|
| VaultPatch      | [Päivitä avaimen säilö](https://msdn.microsoft.com/library/azure/mt620025.aspx)|
| VaultList      | [Luettele kaikki avaimen vaults resurssi-ryhmässä](https://msdn.microsoft.com/en-us/library/azure/mt620027.aspx)|
| KeyCreate      | [Luo avain](https://msdn.microsoft.com/en-us/library/azure/dn903634.aspx)|
| KeyGet      | [Tietoja avain](https://msdn.microsoft.com/en-us/library/azure/dn878080.aspx)|
| KeyImport      | [Avaimen tuominen säilöön](https://msdn.microsoft.com/en-us/library/azure/dn903626.aspx)|
| KeyBackup      | [Varmuuskopiointi-näppäintä](https://msdn.microsoft.com/en-us/library/azure/dn878058.aspx).|
| KeyDelete      | [Poista avain](https://msdn.microsoft.com/en-us/library/azure/dn903611.aspx)|
| KeyRestore      | [Palauttaa avain](https://msdn.microsoft.com/en-us/library/azure/dn878106.aspx)|
| KeySign      | [Kirjaudu avain](https://msdn.microsoft.com/en-us/library/azure/dn878096.aspx)|
| KeyVerify      | [Tarkista avaimella](https://msdn.microsoft.com/en-us/library/azure/dn878082.aspx)|
| KeyWrap      | [Rivitä avain](https://msdn.microsoft.com/en-us/library/azure/dn878066.aspx)|
| KeyUnwrap      | [Poistaa rivityksen avain](https://msdn.microsoft.com/en-us/library/azure/dn878079.aspx)|
| KeyEncrypt      | [Salaa avain](https://msdn.microsoft.com/en-us/library/azure/dn878060.aspx)|
| KeyDecrypt      | [Salauksen avaimella](https://msdn.microsoft.com/en-us/library/azure/dn878097.aspx)|
| KeyUpdate      | [Päivitä avain](https://msdn.microsoft.com/en-us/library/azure/dn903616.aspx)|
| KeyList      | [Luettelon säilöön näppäimet](https://msdn.microsoft.com/en-us/library/azure/dn903629.aspx)|
| KeyListVersions      | [Avaimen versioiden luettelo](https://msdn.microsoft.com/en-us/library/azure/dn986822.aspx)|
| SecretSet      | [Luo salaisuus](https://msdn.microsoft.com/en-us/library/azure/dn903618.aspx)|
| SecretGet      | [Hae salainen](https://msdn.microsoft.com/en-us/library/azure/dn903633.aspx)|
| SecretUpdate      | [Päivitä salaisuus](https://msdn.microsoft.com/en-us/library/azure/dn986818.aspx)|
| SecretDelete      | [Poista salaisuus](https://msdn.microsoft.com/en-us/library/azure/dn903613.aspx)|
| SecretList      | [Luettelon tietoja säilöön](https://msdn.microsoft.com/en-us/library/azure/dn903614.aspx)|
| SecretListVersions      | [Luettelossa salaisuus versiot](https://msdn.microsoft.com/en-us/library/azure/dn986824.aspx)|




## <a id="next"></a>Seuraavat vaiheet ##

Katso opetusohjelma, joka käyttää Azure avaimen säilö web-sovelluksen, [Käytä Azure avaimen säilö WWW-sovelluksesta](key-vault-use-from-web-application.md).

Ohjelmointi viittauksia, artikkelissa [Azure avaimen säilö kehittäjän oppaassa](key-vault-developers-guide.md).

Azure PowerShell 1.0 cmdlet-komennot Azure avaimen säilö luettelo on artikkelissa [Azure avaimen säilö cmdlet-komennot](https://msdn.microsoft.com/library/azure/dn868052.aspx).

Opetusohjelmaan avaimen kierto ja lokitiedostojen tarkistaminen Azure avaimen säilö kanssa Lue [avaimen säilö pääty avaimen kierto ja seurannan asetukset](key-vault-key-rotation-log-monitoring.md).
