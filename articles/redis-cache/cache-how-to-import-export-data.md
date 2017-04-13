<properties 
    pageTitle="Azure Redis välimuistin tietojen tuominen ja vieminen | Microsoft Azure" 
    description="Lue, miten voit tuoda ja viedä tietoja ja sieltä pois Blob-objektien tallennustilaan premium Azure Redis välimuisti-esiintymät" 
    services="redis-cache" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/15/2016" 
    ms.author="sdanie"/>

# <a name="import-and-export-data-in-azure-redis-cache"></a>Azure Redis välimuistin tietojen tuominen ja vieminen

Tuo tai Vie on Azure Redis välimuisti-tietojen hallinnan toiminto, joka mahdollistaa Azure Redis välimuistin tietojen tuominen tai vieminen Azure Redis välimuistin tietojen tuomisesta ja viemisestä Redis välimuistin tietokannan (RDB) tilannevedoksen premium välimuistista sivun Blob-objektien Azure-tallennustilan tilin. Tuo tai Vie avulla voit siirtää Azure Redis välimuistin eri esiintymien välillä tai täyttää välimuistin tietoja ennen käyttöä.

Tässä artikkelissa annetaan opas ja Azure Redis välimuistin tietojen tuomisen ja viemisen ja vastauksia usein kysyttyihin kysymyksiin.

>[AZURE.IMPORTANT] Tuo tai Vie esikatselu on, ja se on käytettävissä [premium taso](cache-premium-tier-intro.md) tallentaa vain.

## <a name="import"></a>Tuo

Tuo avulla voidaan tuoda Redis.txt yhteensopiva RDB tiedostot minkä tahansa Redis.txt palvelimeen cloud tai ympäristössä, mukaan lukien Redis.txt Linux tai minkä tahansa cloud-palvelun Amazon Web Services ja muiden Windows-käyttöjärjestelmässä. Tietojen tuonti on helppo tapa luoda välimuistin täyttää tiedoilla. Tuonnin aikana Azure Redis välimuistin RDB tiedostoja ladataan Azure tallennustilan muistiin ja lisää sitten välimuistiin näppäimet.

>[AZURE.NOTE] Ennen tuontitoiminnon aloittamista, varmista, että Redis tietokannan (RDB)-tiedosto tai tiedostot ladataan kyselyjä sivun BLOB Azuren tallennustilaan, sama alue-ja Azure Redis välimuistin esiintymä-tilauksen. Lisätietoja on artikkelissa [Azure-Blob-säiliö käytön aloittaminen](../storage/storage-dotnet-how-to-use-blobs.md). Jos olet vienyt RDB tiedoston [Azure Redis välimuistin vieminen](#export) -toiminnon avulla, RDB-tiedosto on jo tallennettu sivun Blob-objektien ja on valmis tuontia varten.

1. Yhden tai useamman viedyn Välimuistin BLOB- [välimuistin Siirry](cache-configure.md#configure-redis-cache-settings) Azure-portaalissa ja valitse **tuontitiedot** välimuistin esiintymän **asetukset** -sivu.

    ![Tietojen tuominen][cache-import-data]

2. **Valitse Blob(s)** ja valitse tallennustilan-tili, joka sisältää tuotavat tiedot.

    ![Valitse tallennustilan tili][cache-import-choose-storage-account]

3. Napsauta kansiota, joka sisältää tuotavat tiedot.

    ![Valitse säilö][cache-import-choose-container]

4. Valitse vähintään yksi BLOB tuo valitsemalla alueen blob-nimen vasemmalla puolella ja valitse sitten **Valitse**.

    ![Valitse BLOB-objektit][cache-import-choose-blobs]

5. Valitse Aloita tuonti **Tuo** .

    >[AZURE.IMPORTANT] Välimuistin ei ole käytettävissä välimuistin asiakkaat tuonnin aikana ja välimuistin mahdollisesti olevat tiedot poistetaan.

    ![Tuo][cache-import-blobs]

    Voit valvoa edistymistä tuontitoiminnon seuraamalla ilmoitukset Azure-portaalista tai [Kirjaudu valvontalokin](cache-configure.md#support-amp-troubleshooting-settings)tapahtumien tarkasteleminen.

    ![Tuonnin edistyminen][cache-import-data-import-complete] 


## <a name="export"></a>Vie

Vie voit viedä tiedot tallennetaan Azure Redis välimuistin Redis yhteensopiva RDB tiedostot. Voit käyttää tätä toimintoa voit siirtää tietoja Azure Redis välimuistin esiintymästä toiseen tai johonkin muuhun Redis.txt palvelimeen. Vie aikana tilapäinen tiedosto luodaan, joka isännöi Azure Redis välimuistin server-esiintymän AM ja tiedosto ladataan nimettyjen tallennustilan-tilille. Kun vienti on valmis joko tila onnistumisen tai virhe, tilapäinen tiedosto poistetaan.

1. Välimuistin muodostaminen tallennustilaan, [välimuistin Siirry](cache-configure.md#configure-redis-cache-settings) Azure-portaalissa nykyisen sisällön ja sitten **vientitiedot** -välimuistin esiintymän **asetukset** -sivu.

    ![Valitse tallennustilan säilö][cache-export-data-choose-storage-container]

2. **Valitse tallennustilan säilö** ja valitse haluamasi tallennustilan tilin. Tallennustilan tilin on oltava samassa tilaus ja alueen välimuistin.

    >[AZURE.IMPORTANT] Tuo tai Vie sivun BLOB-objektit, jotka perinteinen ja ARM tallennustilan tilit ovat tuettuja, mutta ei tue [Blob storage tilit](../storage/storage-blob-storage-tiers.md#blob-storage-accounts) tällä hetkellä toimii.

    ![Tallennustilan tilin][cache-export-data-choose-account]

3. Valitse haluamasi blob-säilö ja **Valitse**. Jos haluat käyttää uutta säilön, valitse **Lisää säilö** ja lisää se ensin ja valitse se luettelosta.

    ![Valitse tallennustilan säilö][cache-export-data-container]

4. **Blob-objektien etuliite** ja valitse Aloita Vie **Vie** . Blob-etuliite käytetään etuliite tämän vientitoiminnon luotujen tiedostojen nimet.

    ![Vie][cache-export-data]

    Voit valvoa edistymistä vientitoiminnon seuraamalla ilmoitukset Azure-portaalista tai [Kirjaudu valvontalokin](cache-configure.md#support-amp-troubleshooting-settings)tapahtumien tarkasteleminen.

    ![][cache-export-data-export-complete]

    Tallentaa ovat käytettävissä viemisen aikana.


## <a name="importexport-faq"></a>Tuo tai Vie usein kysytyt kysymykset

Tässä osassa on usein kysyttyjä kysymyksiä Tuo/Vie-ominaisuus.

-   [Mitä hinnat tasoa, voit käyttää Tuo/Vie?](#what-pricing-tiers-can-use-importexport)
-   [Tietojen tuominen mistä tahansa Redis.txt-palvelimesta](#can-i-import-data-from-any-redis-server)
-   [Oma välimuistin ovat käytettävissä tuominen ja vieminen-toiminnon aikana?](#will-my-cache-be-available-during-an-importexport-operation)
-   [Voit käyttää Tuo/Vie Redis.txt klusterin?](#can-i-use-importexport-with-redis-cluster)
-   [Miten Tuo/Vie toimii mukautetun tietokannat-asetuksen?](#how-does-importexport-work-with-a-custom-databases-setting)
-   [Miten eroaa Tuo/Vie Redis.txt pysyvyyttä?](#how-is-importexport-different-from-redis-persistence)
-   [Tuo tai Vie käyttämällä PowerShell, CLI tai muiden hallinta-asiakkaiden automatisointi](#can-i-automate-importexport-using-powershell-cli-or-other-management-clients)
-   [Sain aikakatkaisu-virheen Tuo/Vie-toiminnon aikana. Mitä se tarkoittaa?](#i-received-a-timeout-error-during-my-importexport-operation.-what-does-it-mean)
-   [Sain virhe, kun tietojen vieminen Azure-Blob-objektien tallennustilaan. Mitä tapahtui?](#i-got-an-error-when-exporting-my-data-to-azure-blob-storage.-what-happened)


### <a name="what-pricing-tiers-can-use-importexport"></a>Mitä hinnat tasoa, voit käyttää Tuo/Vie?

Tuo tai Vie on käytettävissä vain premium hinnat taso.

### <a name="can-i-import-data-from-any-redis-server"></a>Tietojen tuominen mistä tahansa Redis.txt-palvelimesta

Kyllä, lisäksi tuomista viety Azure Redis välimuistin esiintymät, voit tuoda RDB tiedostoja mistä tahansa Redis.txt palvelimesta käynnissä cloud tai ympäristössä, kuten Linux-Windows- tai cloud tarjoajien, kuten Amazon verkkopalvelut. Voit tehdä tämän haluamasi Redis.txt palvelimesta RDB-tiedoston lataaminen sivulle Blob-objektien Azure-tallennustilan tilin tuominen ja tuomalla ne premium Azure Redis välimuisti-esiintymä. Haluat ehkä viedä tiedot tuotannon välimuistin ja tuoda sen testaamista tai siirron väliaikaisen ympäristön osana käytettävä välimuistin. 

### <a name="will-my-cache-be-available-during-an-importexport-operation"></a>Oma välimuistin ovat käytettävissä tuominen ja vieminen-toiminnon aikana?

-   **Vie** - tallentaa ovat kuitenkin käytettävissä ja voit edelleen käyttää välimuistin vientitoiminnon aikana.
-   **Tuo** - tallentaa käytettävissä, kun tuontitoimintoa käynnistyy ja tulee näkyviin käyttöä varten, kun tuonti on valmis.


### <a name="can-i-use-importexport-with-redis-cluster"></a>Voit käyttää Tuo/Vie Redis.txt klusterin?

Kyllä, ja voit voit Tuo/Vie liitetty välimuistin ja muiden liitetty välimuistin välillä. Koska Redis klusterin [tukee vain tietokannan 0](cache-how-to-premium-clustering.md#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering), tietokantojen kuin 0 mitään tietoja ei voi tuoda. Kun liitetty välimuistitiedot on tuotu, näppäimet on uudelleen klusterin shards. 

### <a name="how-does-importexport-work-with-a-custom-databases-setting"></a>Miten Tuo/Vie toimii mukautetun tietokannat-asetuksen?

Jotkin hinnoittelu tasoa on eri [tietokantojen rajoitukset](cache-configure.md#databases), jotta on joitakin tapoja tuotaessa, jos olet määrittänyt mukautettu arvo `databases` määrittäminen välimuistin luonnin aikana.

-   Kun tuot hinnoittelu taso, jossa pienempi `databases` taso, josta olet vienyt yläraja:
    -   Jos käytössäsi on oletusmäärän `databases` eli kaikki hinnat tasoa 16-tietoja ei häviä.
    -   Jos käytössäsi on mukautettu määrä `databases` , että sisältö kuuluu sallittujen rajojen sisällä, johon olet tuomassa, tason tietoja ei häviä.
    -   Jos viedyt tiedot sisältyvät tietokannassa, jossa ylittää uuden tason tietoja, kyseiset uudemmissa tiedot ei tuoda.

### <a name="how-is-importexport-different-from-redis-persistence"></a>Miten eroaa Tuo/Vie Redis.txt pysyvyyttä?

Azure Redis välimuistin pysyvyyttä voit jatkuvat Redis.txt Azuren tallennustilaan tallennettuja tietoja. Kun pysyvyys on määritetty, Azure Redis välimuistin jatkuu tilannevedoksen Redis.txt välimuistin Redis.txt binaarimuodossa määritettäviä varmuuskopion korkojakso levylle. Jos kriittinen tapahtuman ilmetessä, joka estää perus- ja replikan välimuistin välimuistitiedot automaattisesti käyttämällä viimeisimmän tilannevedoksen palautetaan. Lisätietoja on artikkelissa [tietojen pysyvyyttä Premium Azure Redis välimuistin määrittämisestä](cache-how-to-premium-persistence.md).

Tuo / Vie avulla voit tuoda tietojen tuominen tai vieminen Azure Redis välimuistista. Se ei Määritä varmuuskopiointi ja palauttaminen Redis.txt pysyvyyttä avulla.


### <a name="can-i-automate-importexport-using-powershell-cli-or-other-management-clients"></a>Tuo tai Vie käyttämällä PowerShell, CLI tai muiden hallinta-asiakkaiden automatisointi

Kyllä, PowerShell ohjeita kohdassa [Redis.txt välimuistin tuo](cache-howto-manage-redis-cache-powershell.md#to-import-a-redis-cache) ja [Vie Redis.txt välimuistin](cache-howto-manage-redis-cache-powershell.md#to-export-a-redis-cache).



### <a name="i-received-a-timeout-error-during-my-importexport-operation-what-does-it-mean"></a>Sain aikakatkaisu-virheen Tuo/Vie-toiminnon aikana. Mitä se tarkoittaa?

Jos sinulla on edelleen yli 15 minuuttia ennen toiminnon aloitetaan **tietojen tuominen** tai **vieminen tiedot** -sivu, näyttöön tulee virhe seuraavankaltaiselta.

    The request to import data into cache 'contoso55' failed with status 'error' and error 'One of the SAS URIs provided could not be used for the following reason: The SAS token end time (se) must be at least 1 hour from now and the start time (st), if given, must be at least 15 minutes in the past.

Voit ratkaista ongelman, Aloita tuonti tai vienti 15 minuuttia ennen on kulunut.

### <a name="i-got-an-error-when-exporting-my-data-to-azure-blob-storage-what-happened"></a>Sain virhe, kun tietojen vieminen Azure-Blob-objektien tallennustilaan. Mitä tapahtui?

Tuo tai Vie toimii vain sivun BLOB tallennettujen RDB tiedostot. Muuntyyppisten blob ei tueta tällä hetkellä, mukaan lukien blob storage tili ja kuuman ja hienoja tasoa.


## <a name="next-steps"></a>Seuraavat vaiheet
Opettele käyttämään Lisää erikoisominaisuuksia välimuistin.

-   [Johdanto Azure Redis välimuistin Premium-taso](cache-premium-tier-intro.md)    

  
<!-- IMAGES -->
[cache-settings-import-export-menu]: ./media/cache-how-to-import-export-data/cache-settings-import-export-menu.png
[cache-export-data-choose-account]: ./media/cache-how-to-import-export-data/cache-export-data-choose-account.png
[cache-export-data-choose-storage-container]: ./media/cache-how-to-import-export-data/cache-export-data-choose-storage-container.png
[cache-export-data-container]: ./media/cache-how-to-import-export-data/cache-export-data-container.png
[cache-export-data-export-complete]: ./media/cache-how-to-import-export-data/cache-export-data-export-complete.png
[cache-export-data]: ./media/cache-how-to-import-export-data/cache-export-data.png
[cache-import-data]: ./media/cache-how-to-import-export-data/cache-import-data.png
[cache-import-choose-storage-account]: ./media/cache-how-to-import-export-data/cache-import-choose-storage-account.png
[cache-import-choose-container]: ./media/cache-how-to-import-export-data/cache-import-choose-container.png
[cache-import-choose-blobs]: ./media/cache-how-to-import-export-data/cache-import-choose-blobs.png
[cache-import-blobs]: ./media/cache-how-to-import-export-data/cache-import-blobs.png
[cache-import-data-import-complete]: ./media/cache-how-to-import-export-data/cache-import-data-import-complete.png








