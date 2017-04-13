<properties 
    pageTitle="Lopeta ja Lopeta vianmääritys Azure-tallennustilan arvot ja kirjaaminen, AzCopy ja viestin Analyzer | Microsoft Azure" 
    description="Lopusta loppuun vianmääritys Azure-tallennustilan Analytics, AzCopy ja Microsoft viestin Analyzer osoittaa opetusohjelman" 
    services="storage" 
    documentationCenter="dotnet" 
    authors="robinsh" 
    manager="carmonm"/>

<tags 
    ms.service="storage" 
    ms.workload="storage" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/03/2016" 
    ms.author="robinsh"/>

# <a name="end-to-end-troubleshooting-using-azure-storage-metrics-and-logging-azcopy-and-message-analyzer"></a>Azure-tallennustilan arvot ja kirjaaminen, AzCopy ja viestin Analyzer lopusta loppuun-vianmääritys 

[AZURE.INCLUDE [storage-selector-portal-e2e-troubleshooting](../../includes/storage-selector-portal-e2e-troubleshooting.md)]

## <a name="overview"></a>Yleiskatsaus

Ohjelmistossa ja vianmääritys on avaimen osaamisalueiden luomisesta ja asiakassovelluksissa Microsoft Azuren tallennustilaan kanssa. Vuoksi Azure sovelluksen hajautettu laatu voi olla monimutkaisempi kuin perinteisen ympäristöissä ohjelmistossa esiintyvien virheiden ja suorituskykyongelmien vianmääritys.

Tässä opetusohjelmassa on esitetty, miten tunnistaa asiakkaan tiettyjä virheitä, jotka saattavat vaikuttaa suorituskykyyn ja näiden vianmääritys lopusta loppuun-työkaluilla Microsoft ja Azure-tallennustilan, jotta voit optimoida asiakassovellus. 

Tässä opetusohjelmassa on vianmäärityksen lopusta loppuun-skenaarion käytännön tarkasteluun. Yksityiskohtaisempia käsitteellinen oppaan vianmääritystä Azure tallennustilan sovellukset Katso [näytön vianmäärityksen, ja vianmääritys Microsoft Azuren tallennustilaan](storage-monitoring-diagnosing-troubleshooting.md). 

## <a name="tools-for-troubleshooting-azure-storage-applications"></a>Azuren tallennustilaan sovelluksia vianmää Työkalut

Vianmääritys käyttämällä Microsoft Azuren tallennustilaan asiakassovelluksissa määrittää, kun ongelma on tapahtunut ja ongelman syy voi olla Työkalut yhdistelmän avulla. Työkaluja ovat:

- **Azure-tallennustilan Analytics**. [Azure-tallennustilan Analytics](http://msdn.microsoft.com/library/azure/hh343270.aspx) sisältää arvot ja kirjaaminen, jotta Azure-tallennustilan.
    - **Tallennustilan arvot** seuraa tapahtuman arvot ja tallennustilan tilin mittaukset kapasiteetti. Käytä arvot, voit määrittää miten sovelluksesi toimii eri mitat erilaisia mukaan. Saat lisätietoja tallennustilan Analytics jäljittää arvot tyypit [Tallennustilan Analytics arvot Taulukkorakenteen](http://msdn.microsoft.com/library/azure/hh343264.aspx) . 

    - **Tallennustilan kirjaaminen** kirjaa sivupyynnön palvelinpuolen lokiin Azuren tallennustilaan-palveluihin. Kirjaudu seuraa sivupyynnön, mukaan lukien suorittaa toiminnon, toiminto ja viive tietoja tilan yksityiskohtaiset tiedot. Saat lisätietoja pyynnön ja vastauksen tiedot, jotka on kirjoitettu lokit mukaan tallennustilan Analytics [Analytics Log Tallennusmuoto](http://msdn.microsoft.com/library/azure/hh343259.aspx) .

> [AZURE.NOTE] Tallennustilan tili ja replikoinnin tyyppi, vyöhykkeen tarpeettomat Storage (ZRS) ei ole arvot tai virheloki-ominaisuus on otettu käyttöön tällä hetkellä. 

- **Azure perinteinen Portal**. Voit määrittää arvot ja kirjaaminen [Azure perinteinen Portal](https://manage.windowsazure.com)-tallennustilan tilin. Voit myös tarkastella kaavioita, joka näyttää, miten sovelluksesi toimii ajan kuluessa ja ilmoittaa, jos sovellus suorittaa odotettu tietyn mittarin ilmoitusten määrittäminen. 
    
    Saat lisätietoja määrittäminen seurantaa Azure perinteinen portaalissa [näytön tallennustilan-tilin Azure-portaalissa](storage-monitor-storage-account.md) .

- **AzCopy**. Server Azure-tallennustilan lokit tallennetaan BLOB-objektit, jotta voit käyttää AzCopy log-BLOB kopioiminen paikallisessa kansiossa Microsoft viestin analysoimisen avulla analyysia varten. Saat lisätietoja AzCopy [tiedonsiirron AzCopy komentorivivalitsimet-apuohjelman avulla](storage-use-azcopy.md) .

- **Microsoft Message Analyzer**. Viestin Analyzer-työkalua, joka käyttää lokitiedostot ja näyttää lokitiedot visuaalisessa muodossa, joka on helppo suodatin, Etsi ja ryhmän lokitiedot hyödyllisiä joukkoihin, jonka avulla voit analysoida virheitä ja suorituskykyongelmia. On [Microsoft viestin Analyzer toimivien Guide](http://technet.microsoft.com/library/jj649776.aspx) lisätietoja viestin Analyzer.

## <a name="about-the-sample-scenario"></a>Lisätietoja esimerkkitilanne

Tässä opetusohjelmassa on tutkia tilanne, jossa Azure-tallennustilan arvot ilmaisee pienen prosentti success korvaus sovelluksen, joka kutsuu Azure-tallennustilan. Pieni prosentti success korko-arvo (näkyy **PercentSuccess** Azure perinteinen portaalin ja arvot taulukoista) seuraa toimintoja, jotka onnistuu, mutta, jotka palauttavat HTTP-tilakoodi on suurempi kuin 299. Palvelinpuolen tallennustilan-lokitiedostojen näitä toimintoja tallennetaan **ClientOtherErrors**tapahtuman tilassa. Saat lisätietoja pienen prosentti onnistumista metrijärjestelmä [arvot näyttää pienen PercentSuccess tai analytics lokimerkintöjä on toimintoja, joiden ClientOtherErrors tila](storage-monitoring-diagnosing-troubleshooting.md#metrics-show-low-percent-success).

Azure tallennustilan toiminnot voi palauttaa HTTP-tilakoodin suurempi kuin 299 niiden Normaali toimintoja osana. Mutta joissakin tapauksissa nämä virheet osoittaa, että voit ehkä optimointi asiakassovellus parannettu suorituskyky. 

Tässä skenaariossa on harkitse pienen prosentti success korvaus voi olla mikä tahansa alle 100 %. Voit valita eri metrisillä tasolle kuitenkin tarpeidesi mukaan. On suositeltavaa, että sovelluksesi testauksen aikana voit vahvistaa perusaikataulun poikkeama suorituskyvyn mittarit. Esimerkiksi voi selvittää, perusteella testauksen sovelluksen pitäisi olla yhdenmukaisia prosentti onnistumista korvaus 90 % tai 85 %. Jos arvot tietojen osoittaa, että sovellus on mitoista luku-ja valitse voit tutkia, mikä voi aiheuttaa lisäys. 

Microsoftin esimerkkitilanne kun Microsoft on muodostettu, että prosentti success korko-arvo on alle 100 prosenttia, Microsoft Tarkista voit etsiä virheitä, joka yhdistää arvot, ja niiden avulla voit selvittää, mikä aiheuttaa pienempi prosentti success nopeus lokit. Tarkastellaan erityisesti 400 alueen virheet. Valitse Microsoft tutkia tarkemmin 404 (ei löydy) virheitä.

### <a name="some-causes-of-400-range-errors"></a>Joitakin syitä 400 alueen tarkistus

Seuraavissa esimerkeissä näytetään esimerkkejä, Azure-Blob-säiliö ja niiden mahdollisia syitä pyynnöt 400-alue-virheitä. Jokin nämä virheet sekä 300 alueen ja 500 alueen voivat osallistua pienen prosentti onnistumista korvaus. 

Huomaa, että alla olevista luetteloista huomattavasti valmis. Lisätietoja [tila- ja virhekoodit](http://msdn.microsoft.com/library/azure/dd179382.aspx) Yleiset Azuren tallennustilaan virheisiin ja virheisiin kullekin liittyviä palveluja.

**Tila (ei löydy) koodin 404-esimerkkejä**

Tapahtuu, kun säilö tai blob lukemiseen toiminnan epäonnistuu, koska blob-säilö ei löydy.

- Tapahtuu, jos säilön tai blob on poistettu toisen asiakkaan ennen kehotusta. 
- Tapahtuu, jos käytössäsi on API-kutsu, joka luo blob-säilö tarkistettuaan, onko olemassa. CreateIfNotExists-ohjelmointirajapinnan soittaminen HEAD ensimmäisen kerran, jos haluat tarkistaa, onko säilö tai Blob-objektien; Jos sitä ei ole, 404-virhe palautetaan, ja valitse toinen HYLLYTETTY-puhelu soitetaan kirjoittaa blob-säilö.

**Tilakoodi 409 (ristiriita)-Esimerkkejä**

- Tapahtuu, jos luominen API avulla voit luoda uuden säilön tai blob-tarkistamatta ensin olemassaolo ja säilö tai blob nimi on jo olemassa. 
- Tapahtuu, jos säilön poistetaan ja yrität luoda uuden säilön saman niminen, ennen kuin poistotoiminto on valmis.
- Tapahtuu, jos määrittämäsi varaus säilö tai Blob-objektien ja on jo olemassa varaus.
 
**Tilakoodin 412 (ennakkoehto epäonnistui) Esimerkkejä**

- Tapahtuu, kun ehdollinen ylätunnisteen määrittämä ehto ei täyty.
- Tapahtuu, kun määritetty varauksen tunnus vastaa varauksen tunnus-blob-säilö.

## <a name="generate-log-files-for-analysis"></a>Luo lokitiedostojen analysointia varten

Tässä opetusohjelmassa Käytämme viestin Analyzer toimimaan kolmenlaisia lokitiedostot, vaikka voi valita jonkin seuraavista käyttöä varten.

- **Palvelimen lokitiedoston**, joka luodaan, kun otat Azuren tallennustilaan kirjaaminen. Palvelimen lokista sisältää tietoja kunkin toiminnon vastaan Azuren tallennustilaan-palvelut - Blob-objektien, jonossa, taulukon ja tiedoston nimi. Palvelimen lokista osoittaa, mitä toiminto kutsuttiin ja mitä tilakoodi pyynnön ja vastauksen palautetut sekä muita tietoja.
- **.NET asiakkaan lokitiedoston**, joka luodaan, kun otat asiakaspuolen kirjaaminen-.NET-sovelluksesta. Asiakas-loki on tarkkoja tietoja siitä, miten asiakas valmistelee pyynnön ja vastaanottaa ja käsittelee vastaus. 
- **HTTP-verkon jäljitysloki**, joka kerää tietoja HTTP/HTTPS pyynnön ja vastauksen tiedoista, kuten toimille vastaan Azure-tallennustilan. Tässä opetusohjelmassa on Luo verkkoseurannan viestin Analyzer kautta.

### <a name="configure-server-side-logging-and-metrics"></a>Palvelinpuolen kirjaaminen ja arvot määrittäminen

Ensin annettava Määritä Azure-tallennustilan kirjaaminen ja arvot, niin, että olemme asiakassovellus analysoida tietoja. Voit määrittää kirjaaminen ja arvot useilla tavoilla – [Perinteinen Azure-portaalin](https://manage.windowsazure.com)kautta käyttämällä PowerShell- tai ohjelmallisesti. Lisätietoja [tallennustilan arvot ottaminen käyttöön ja arvot tietojen tarkasteleminen](http://msdn.microsoft.com/library/azure/dn782843.aspx) ja [ottaminen käyttöön tallennustilan kirjaaminen ja käytettäessä lokitiedot](http://msdn.microsoft.com/library/azure/dn782840.aspx) määrittämisestä kirjaaminen ja arvot.

**Azure perinteinen portaalin kautta**

Määritä kirjaaminen ja mittaukset portaalissa tallennustilan tilin noudattamalla [näytön tallennustilan-tilin Azure-portaalissa](storage-monitor-storage-account.md)osoitteessa olevia ohjeita.

> [AZURE.NOTE] Ei ole mahdollista määrittää minuutit arvot perinteinen Azure-portaalissa. Suosittelemme kuitenkin, että voit määrittää ne tarkoitetaan tässä opetusohjelmassa ja tutkiminen suorituskykyongelmia sovelluksen. Voit määrittää minuutit arvot PowerShellin avulla, kuten alla tai ohjelmallisesti tai Azure perinteinen portaalin kautta.
>
> Huomaa, että Azure perinteinen portaalin ei voi näyttää minuutit mittarit, vain tunnin välein arvot. 

**PowerShellin kautta**

Aloita Azure PowerShellin avulla, katso, [miten asennetaan ja määritetään PowerShellin Azure](../powershell-install-configure.md).

1. Azure käyttäjätilin lisääminen PowerShell-ikkunassa [Lisää AzureAccount](http://msdn.microsoft.com/library/azure/dn722528.aspx) cmdlet-komennon avulla:

    ```
    Add-AzureAccount
    ```

2. Kirjoita sähköpostiosoite ja salasana tilisi **Kirjaudu Microsoft Azure** -ikkunaan. Azure todentaa ja tallentaa tunnistetietoja ja sulkee ikkunan.
3. Tallennustilan oletustilin määrittäminen tallennustilan tilille käytät opetusohjelman suorittamalla seuraavat komennot PowerShell-ikkunassa:

    ```
    $SubscriptionName = 'Your subscription name'
    $StorageAccountName = 'yourstorageaccount' 
    Set-AzureSubscription -CurrentStorageAccountName $StorageAccountName -SubscriptionName $SubscriptionName 
    ```

4. Ota käyttöön Blob-palvelun tallennustilan kirjaaminen: 
 
    ```
    Set-AzureStorageServiceLoggingProperty -ServiceType Blob -LoggingOperations Read,Write,Delete -PassThru -RetentionDays 7 -Version 1.0 
    ```
5. Tallennustilan arvot Blob-palvelua varmistetaan, että **- MetricsType** asetukseksi Ota käyttöön `Minute`:

    ```
    Set-AzureStorageServiceMetricsProperty -ServiceType Blob -MetricsType Minute -MetricsLevel ServiceAndApi -PassThru -RetentionDays 7 -Version 1.0 
    ```

### <a name="configure-net-client-side-logging"></a>Määritä .NET asiakaspuolen kirjaaminen

Määritä asiakkaan kirjaaminen .NET-sovelluksen käyttöön .NET vianmääritys sovelluksen määritystiedostossa (web.config tai app.config). Lisätietoja [asiakkaan kirjaaminen .NET tallennustilan asiakas-kirjaston kanssa](http://msdn.microsoft.com/library/azure/dn782839.aspx) ja [asiakaspuolen kirjaaminen Microsoft Azure tallennustilan SDK for Javan kanssa](http://msdn.microsoft.com/library/azure/dn782844.aspx) .

Asiakkaan loki sisältää yksityiskohtaisia tietoja siitä, miten asiakas valmistelee pyynnön ja vastaanottaa ja käsittelee vastaus.

Tallennustilan asiakas-kirjasto tallentaa asiakkaan lokitiedot sovelluksen määritystiedostossa (web.config tai app.config) määritettyyn sijaintiin. 

### <a name="collect-a-network-trace"></a>Kerää verkkoseurannan

Viestin Analyzer avulla voit kerätä HTTP/HTTPS-verkkoseurannan asiakassovellus on käynnissä. Viestin Analyzer käyttää [Fiddler](http://www.telerik.com/fiddler) uudelleen. Ennen kuin olet kerännyt verkon seurantatiedoista, on suositeltavaa, että määrität Fiddler tallentavan salaamaton HTTPS-liikenne:

1. Asenna [Fiddler](http://www.telerik.com/download/fiddler).
2. Käynnistä Fiddler.
2. Valitse **Työkalut | Fiddler asetukset**.
3. Valitse asetukset-valintaikkunan varmistaa, että **Siepata HTTPS-muodostaa yhteyden** ja **Salauksen HTTPS-liikenne** ovat valittuina, alla kuvatulla tavalla.

![Fiddler asetusten määrittäminen](./media/storage-e2e-troubleshooting-classic-portal/fiddler-options-1.png)

Opetusohjelmaan kerätä ja tallentaa verkkoseurannan ensin viesti analysoiminen ja valitse sitten Luo analysis-istunto voit analysoida seurannasta ja lokit. Kerää verkkoseurannan viestin analysoinnissa:

1. Valitse viestin Analyzer **tiedoston | Nopea jäljitys | Salaamaton HTTPS**.
2. Jäljitys käynnistyy heti. Valitse **Lopeta** , jos haluat lopettaa jäljityksen niin, että olemme voit määrittää jäljitys tallennustilan liikennettä vain.
3. Valitse **Muokkaa** , jos haluat muokata jäljitysistunnon.
4. Valitse oikealla puolella **Microsoft-Pef-WebProxy** tapahtumien seuranta-palvelun **määrittäminen** -linkki.
5. Valitse **Lisäasetukset** -valintaikkunan **palvelu** -välilehti.
6. Määritä **Hostname** suodatinkentän tallennustilan päätepisteet, ja erottaminen välilyönnillä. Esimerkiksi voit määrittää oman päätepisteet seuraavasti: Muuta `storagesample` tallennustilan tilin nimi:
    
    ``` 
    storagesample.blob.core.windows.net storagesample.queue.core.windows.net storagesample.table.core.windows.net 
    ```

7. Sulje valintaikkuna ja valitse Aloita kerääminen seurannasta hostname suodattimella paikassa, **Käynnistä uudelleen** niin, että vain Azuren tallennustilaan verkkoliikennettä sisältyy jäljitys.

>[AZURE.NOTE] Sen jälkeen, kun olet kerääminen verkon seurantatiedoista, on suositeltavaa palata asetuksia, jotka olet ehkä muuttanut-Fiddler salauksen purkaminen HTTPS-liikennettä. Valitse Fiddler asetukset-valintaikkunan valinnan **Siepata HTTPS-muodostaa yhteyden** ja **Salauksen HTTPS-liikenne** valintaruudut.

[Verkon jäljitys-ominaisuuksilla](http://technet.microsoft.com/library/jj674819.aspx) TechNetin artikkelissa lisätietoja.

## <a name="review-metrics-data-in-the-azure-classic-portal"></a>Perinteinen Azure-portaalissa arvot tietojen tarkasteleminen

Kun sovellus on ollut käynnissä tietyn ajanjakson aikana, voit tarkastella arvot kaavioita, joissa näkyvät Azure perinteinen portaalissa voit tarkastella, kuinka palvelusi on ollut suorittamiseen. Lisätään ensin **Success prosentuaalisen** itseisarvon seuranta-sivulle:

1. Siirry [Azure perinteinen Portal](https://manage.windowsazure.com)-tallennustilan tilin koontinäyttöön ja valitse sitten **näytön** seuranta-sivua.
2. Valitse **Lisää arvot** , **Valitse arvot** -valintaikkunan näyttämiseen.
3. Vieritä alaspäin kohtaan **Success prosentti** -ryhmästä Laajenna sitä, valitse sitten **Kooste**-alla olevassa kuvassa esitetyllä tavalla. Tämä arvo kokoaa success prosenttitiedot kaikki Blob-toiminnot.

![Valitse arvot](./media/storage-e2e-troubleshooting-classic-portal/choose-metrics-portal-1.png)

Azure perinteinen-portaalissa nyt näet **Success prosentti** seuranta-kaaviossa, sekä muita tietoja olet ehkä lisännyt (enintään kuusi voidaan näyttää kaaviossa samalla kertaa). Alla olevassa kuvassa näet prosentti onnistumisen on hieman alle 100 %, joka on olemme tutkia seuraava mukaan analysoiminen viestin Analyzer lokit skenaariota:

![Arvot-kaavio-portaalissa](./media/storage-e2e-troubleshooting-classic-portal/portal-metrics-chart-1.png)

Lisätietoja arvot lisääminen seuranta-sivun [Toimintaohje: arvot lisätään arvot-taulukkoon](storage-monitor-storage-account.md#how-to-add-metrics-to-the-metrics-table).

> [AZURE.NOTE] Voi kestää jonkin aikaa arvot tietojen näkyvän perinteinen Azure-portaalissa, kun olet ottanut käyttöön tallennustilan arvot. Tämä johtuu siitä edellisen tunnin tunnin välein mittaukset ei näy perinteinen Azure-portaalissa, kunnes nykyisen tunnin on kulunut. Lisäksi minuutit arvot eivät näy perinteinen Azure-portaalissa. Niin vastaavassa, kun otat mittarit, voi kestää jopa kaksi tuntia arvot tiedot.

## <a name="use-azcopy-to-copy-server-logs-to-a-local-directory"></a>Kopioi palvelimen lokit paikallisessa kansiossa AzCopy avulla

Azure-tallennustilan kirjoittaa palvelimen lokitiedot BLOB-objektit, kun arvot kirjoitetaan taulukoihin. Log-BLOB ovat käytettävissä tunnetun `$logs` tilin tallennustilan säilö. Log BLOB nimetään hierarkiassa vuoden, kuukauden, päivän ja tunti, jotta löydät helposti haluat tutkia ajanjakson. Esimerkiksi `storagesample` tili, saat 01/02/2015-8 – 9 am log-BLOB säilö on `https://storagesample.blob.core.windows.net/$logs/blob/2015/01/08/0800`. Yksittäisten BLOB tähän säilöön nimetään järjestyksessä alkavat `000000.log`.

Voit ladata palvelinpuolen lokitiedostoista valittua sijaintiin paikallisessa tietokoneessa AzCopy työkalun avulla. Esimerkiksi seuraava komento avulla voi ladata blob-toiminnoista, joita suoritettiin 2. tammikuussa 2015 kansioon lokitiedostojen `C:\Temp\Logs\Server`; Korvaa `<storageaccountname>` tallennustilan tilin nimi ja `<storageaccountkey>` access tiliavain kanssa:

    AzCopy.exe /Source:http://<storageaccountname>.blob.core.windows.net/$logs /Dest:C:\Temp\Logs\Server /Pattern:"blob/2015/01/02" /SourceKey:<storageaccountkey> /S /V

AzCopy on ladattavissa [Azure](https://azure.microsoft.com/downloads/) -lataussivulta. Lisätietoja AzCopy käyttämisestä on artikkelissa [tiedonsiirron AzCopy komentorivivalitsimet-apuohjelman avulla](storage-use-azcopy.md).

Saat lisätietoja lataaminen palvelinpuolen lokeja [lataaminen tallennustilan kirjaaminen lokitiedot](http://msdn.microsoft.com/library/azure/dn782840.aspx#DownloadingStorageLogginglogdata). 

## <a name="use-microsoft-message-analyzer-to-analyze-log-data"></a>Microsoft viestin Analyzer avulla voit analysoida tietoja

Microsoft viestin Analyzer-työkalua sieppaus, näyttäminen ja analysoiminen protokolla messaging liikenne, tapahtumia ja muut järjestelmän tai sovelluksen viestit vianmäärityksen ja diagnostiikan tilanteissa. Viestin Analyzer avulla voit ladata, koosta ja analysoida tietoja lokista myös ja tallentaa jäljitystiedot. Lisätietoja viestin Analyzer on [Microsoft viestin Analyzer toimivien Guide](http://technet.microsoft.com/library/jj649776.aspx).

Viestin Analyzer sisältää resurssit, joiden avulla voit analysoida server client ja verkon lokit Azure-tallennustilan. Tässä osassa käsittelemme pienen prosentti success tallennustilan lokien ongelman kyseisten työkalujen avulla.

### <a name="download-and-install-message-analyzer-and-the-azure-storage-assets"></a>Lataa ja asenna viestin Analyzer ja Azure-tallennustilan resurssit

1. Lataa [Viestin Analyzer](http://www.microsoft.com/download/details.aspx?id=44226) Microsoft Download Centeristä ja suorittamalla asennusohjelma.
2. Käynnistä viestin Analyzer.
3. Valitse **Työkalut** -valikosta **Kohteiden hallinta**. **Kohteiden hallinta** -valintaikkuna valitsemalla **Lataa**, ja suodattaa **Azure-tallennustilan**. Näet Azure-tallennustilan kohteiden, kuten alla olevassa kuvassa on esitetty.
4. Valitse **Synkronoi kaikki näyttää kohteet** asentaminen Azure-tallennustilan resurssit. Käytettävissä olevat resurssit ovat seuraavat:
    - **Azure tallennustilan väri sääntöjä:** Azure tallennustilan väri sääntöjen avulla voit määrittää erityisiä suodattimet, jotka käyttävät väri, tekstin ja fonttityylien Korosta jäljityksen tietoja sisältävien viestien.
    - **Azure tallennustilan kaavioiden:** Azure tallennustilan kaaviot ovat ennalta määritettyjä kaavioita, joissa kaavion palvelimen lokitiedot. Huomautus käyttämään Azuren tallennustilaan kaavioiden tällä hetkellä voi vain ladata palvelimen lokista analyysi ruudukkoon.
    - **Azure tallennustilan jäsentimet:** Azuren tallennustilaan jäsentimet jäsentää Azuren tallennustilaan asiakaskone, palvelimen ja HTTP-lokit näyttämiseksi Analysis-ruudukossa.
    - **Azure tallennustilan suodattimet:** Azure tallennustilan suodattimet ovat ennalta määritettyjä ehtoja, joita voit tehdä kyselyn tietojen analysointi-ruudukossa.
    - **Azure tallennustilan näkymän asettelut:** Azure tallennustilan näkymän asettelut ovat ennalta määritetyn sarakkeen asettelut ja ryhmittelyjen Analysis-ruudukossa.
4. Kun olet asentanut varat, Käynnistä uudelleen viestin Analyzer.

![Viestin Analyzer kohteiden hallinta](./media/storage-e2e-troubleshooting-classic-portal/mma-start-page-1.png)

> [AZURE.NOTE] Asenna kaikki Azuren tallennustilaan varat tarkoitetaan tässä opetusohjelmassa näytetään.

### <a name="import-your-log-files-into-message-analyzer"></a>Log-tiedostojen tuominen viesti analysoiminen

Voit tuoda kaikki lokin tallennetut tiedostot (palvelinpuolen, asiakkaan ja verkon) yksittäistä istuntoa varten Microsoft viestin Analyzer analyysia varten.

1. Microsoft viestin Analyzer **Tiedosto** -valikon **Uuden istunnon**ja valitse sitten **Tyhjä istunnon**. Kirjoita **Uuden istunnon** -valintaikkunassa nimi analysis-istunto. Napsauta **Istunnon** tietoruudussa **tiedostot** -painiketta. 
1. Lataamaan viestin Analyzer luomien verkon seuranta tietojen valitsemalla **Lisää tiedostoja**, etsi selaamalla sijainti, johon olet tallentanut .matp tiedoston web-jäljityksen istunnosta, valitse .matp-tiedosto ja valitse **Avaa**. 
1. Lataa palvelinpuolen lokitiedot, valitse **Lisää tiedostoja**, etsi selaamalla sijainti, johon olet ladannut palvelinpuolen lokeja, valitse analysoitavat aikavälin lokitiedostojen ja valitse **Avaa**. - **Istunnon tiedot** -ruudussa Aseta **Tekstin asetukset** kunkin palvelinpuolen lokitiedoston avattavasta **AzureStorageLog** varmistaa, että Microsoft viestin Analyzer voi jäsentää lokitiedosto oikein.
1. Asiakkaan lokitiedot lataamaan valitsemalla **Lisää tiedostoja**, etsi selaamalla sijainti, johon tallensit asiakaspuolen lokeista, valitse analysoitavat lokitiedostot ja valitse **Avaa**. - **Istunnon tiedot** -ruudussa Aseta **Tekstin asetukset** avattavasta kunkin asiakkaan lokitiedoston **AzureStorageClientDotNetV4** varmistaa, että Microsoft viestin Analyzer voi jäsentää lokitiedosto oikein.
1. Valitse **Aloita** lataamisen ja jäsentää lokitiedot **Uuden istunnon** -valintaikkunassa. Lokitiedot näyttää sanoman Analyzer Analysis-ruudukossa.

Alla olevassa kuvassa näkyy määritetty palvelimeen, asiakkaan ja verkon jäljityksen lokitiedostojen Esimerkki-istunto.

![Viestin Analyzer istunnon määrittäminen](./media/storage-e2e-troubleshooting-classic-portal/configure-mma-session-1.png)

Huomautus viestin Analyzer Lataa lokitiedostojen muistiin. Jos sinulla on suuri joukko lokitiedot, haluat suodattaa jotta saat parhaan suorituskyvyn-viestin Analyzer.

Ensin määrittää ajanjakson, jotka ovat kiinnostuneita tarkistaminen ja säilyttää mahdollisimman pieni tähän aikaväli. Monissa tapauksissa haluat tarkastella minuuttia tai yleensä tunnin ajan. Tuoda pienin lokit, jotka voivat vastaa tarpeitasi.

Jos sinulla on paljon tietoja, voivat haluat määrittää istunnon suodattimen lokin tietojen ennen kuin voit ladata sen. Valitse **Istunnon suodatin** -kentässä voit valita valmiiksi määritetyn suodattimen; **kirjasto** -painike esimerkiksi Valitse **Yleinen aika suodattimen I** Azuren tallennustilaan suodattimista Suodata aikaväli. Voit muokata sitten suodatusehtojen määrittäminen ensimmäinen ja viimeinen aikaleiman aikavälin, joiden haluat olevan näkyvissä. Voit myös suodattaa tilakoodi; Voit esimerkiksi ladata vain missä tilakoodin 404 lokimerkintöjä.

Lisätietoja lokitiedot tuominen Microsoft viestin Analyzer on TechNetin [Viestitiedot noudetaan](http://technet.microsoft.com/library/dn772437.aspx) .

### <a name="use-the-client-request-id-to-correlate-log-file-data"></a>Pyyntö Ostajantunnus avulla voit yhdistää lokitiedostotiedot

Azure-tallennustilan asiakkaan kirjaston luo automaattisesti yksilöllisen asiakkaan-pyynnön tunnus aina. Tämän arvon kirjoitetaan asiakkaan lokin, palvelimen lokista ja verkkoseurannan, jotta sen avulla voit yhdistää tietoja eri kaikki kolme lokit sisällä viestin Analyzer. Katso [pyynnön Ostajantunnus](storage-monitoring-diagnosing-troubleshooting.md#client-request-id) lisätietoja pyyntö tunnus.

Seuraavissa osissa kerrotaan, kuinka voit yhdistää ennalta määritetyt ja mukautetut asettelun näkymien avulla ja ryhmitellä tietoja perustuu asiakkaan pyynnön ID-tunnuksellasi.

### <a name="select-a-view-layout-to-display-in-the-analysis-grid"></a>Valitse Näytä asettelu analyysi Ruudukon näyttäminen

Viestin Analyzer tallennustilan-resurssit ovat Azure tallennustilan näkymän asetteluja, jotka ovat valmiiksi näkymiä, joita voit käyttää hyödyllistä Ryhmittelyt ja sarakkeiden eri skenaarioissa tietojen näyttämiseen. Voit luoda mukautetun näkymän asetteluja ja tallentaminen uudelleenkäyttöä varten. 

Alla olevassa kuvassa näkyy **Näytä asettelu** -valikon, valitsemalla **Näytä asettelu** työkalurivin valintanauhan käytettävissä. Näytä asetteluita Azuren tallennustilaan on ryhmitelty **Azuren tallennustilaan** solmu-valikossa. Voit etsiä `Azure Storage` hakuruutuun suodatettavan Azuren tallennustilaan tarkastella vain asetteluja. Voit myös valita Näytä rakennetta merkitä suosikiksi ja näyttää sen yläreunassa olevaa valikkoa vieressä oleva tähti.

![Näytä asetusvalikko](./media/storage-e2e-troubleshooting-classic-portal/view-layout-menu.png)

Valitse Aloita **ryhmitetyt ClientRequestID ja moduuli**. Ryhmien tarkasteleminen Tämä asettelu Kirjaudu tietojen kolme lokien ensin Ostajantunnus pyyntö ja sitten lokiin lähdetiedostoa (tai viestin Analyzer- **moduulin** ). Tässä näkymässä voit siirtyä tietyn asiakkaan pyynnön-tunnuksen alirakenteeseen ja tiedot asiakkaan pyynnön kaikki kolme lokitiedostoista tunnus.

Kuvan alapuolella näkyy tässä asettelunäkymä käyttöön malli-lokitiedot näkyvien sarakkeiden osajoukkoa. Näet, että tietyn asiakkaan pyynnön ID-analyysin ruudukko näyttää asiakkaan lokitiedoston, palvelimen lokista ja verkkoseurannan tietoja.

![Azure-tallennustilan Näytä asettelu](./media/storage-e2e-troubleshooting-classic-portal/view-layout-client-request-id-module.png)

>[AZURE.NOTE] Eri lokitiedostot on erilaisia sarakkeita, joten tietoja useista lokitiedostoista näkyy analyysi ruudukon, joitakin sarakkeita ei saa sisältää kaikki tietyn rivin tiedot. Esimerkiksi yllä olevassa kuvassa asiakkaan lokin rivit ei näy mitään tietoja **aikaleiman**, **TimeElapsed**, **lähde**ja **kohde** -saraketta ja koska nämä sarakkeet eivät ole asiakkaan lokiin, mutta ei ole verkon seurantatiedoista. Vastaavasti **aikaleima** -sarakkeessa näkyy palvelimen lokista aikaleima tietoja, mutta **TimeElapsed** **lähde**ja **kohde** sarakkeet, jotka eivät kuulu palvelimen lokista ei näy mitään tietoja. 

Lisäksi Azuren tallennustilaan näkymä-asetteluja, voit myös määrittää ja Tallenna näkymä-asettelun. Voit valita muita haluamasi kenttiä tietojen ryhmittely ja ryhmittelyn tallentaa mukautetun asettelun sekä osana. 

### <a name="apply-color-rules-to-the-analysis-grid"></a>Käytä väri sääntöjä Analysis-ruudukko

Tallennustilan varat myös väri säännöt, jotka antavat visualisoinnin tarkoittaa tunnistavan eri Virhetyyppien Analysis-ruudukossa. Ennalta määritetyn väriä koskevat HTTP-virheet, jotta ne näkyvät vain loki- ja Jäljitä.

Jos haluat käyttää väriä sääntöjä, valitse **Väri säännöt** työkalurivin valintanauhasta. Näet Azuren tallennustilaan väri säännöt-valikossa. Valitse opetusohjelman **Asiakkaan virheitä (StatusCode 400 ja 499 välillä)**, alla olevassa kuvassa esitetyllä tavalla.

![Azure-tallennustilan Näytä asettelu](./media/storage-e2e-troubleshooting-classic-portal/color-rules-menu.png)

Lisäksi Azuren tallennustilaan väri sääntöjen avulla, voit myös määrittää ja tallentaa väri-säännöt.

### <a name="group-and-filter-log-data-to-find-400-range-errors"></a>Ryhmittely- ja suodattimen lokitiedot 400 alueen virheiden etsiminen

Seuraavaksi on ryhmitellä ja suodattaa kaikki virheitä löytyy 400 alueen lokitiedot.

1. Etsi **StatusCode** -sarakkeen Analysis-ruudukossa, napsauttamalla sarakkeen otsikkoa hiiren kakkospainikkeella ja valitse **ryhmä**.
2. Seuraavaksi ryhmän **ClientRequestId** saraketta. Näkyviin tulee tietojen analysointi-ruudukossa järjestetään nyt tilakoodin ja asiakkaan pyynnön ID-tunnuksellasi.
1. Näytä suodatin tool-ikkunassa, jos se ei ole vielä näkyvissä. Valitse työkalurivillä-valintanauhasta **Työkalun Windows**, valitse **Näkymän suodattimen**.
2. Suodattaminen näyttämään vain 400-alueen virheet lokitiedot, Lisää seuraavat suodattimen ehdot **Suodatin** -ikkunassa ja valitse **Käytä**:

        (AzureStorageLog.StatusCode >= 400 && AzureStorageLog.StatusCode <=499) || (HTTP.StatusCode >= 400 && HTTP.StatusCode <= 499)

Alla olevassa kuvassa ryhmittely- ja suodattimen tulokset. Toiminnon, jota kyseisen tilakoodin tuloksena näkyy esimerkiksi laajentaminen **ClientRequestID** -kentän alapuolella tilakoodin 409 ryhmittely.

![Azure-tallennustilan Näytä asettelu](./media/storage-e2e-troubleshooting-classic-portal/400-range-errors1.png)

Sen jälkeen, kun tämä suodatin, näet, että rivit asiakkaan lokista jätetään pois, kuin asiakas loki ei ole **StatusCode** sarakkeen. Aloita on Tarkista palvelimen ja verkon seuranta lokit Etsi 404 virheet ja sitten palautettavien tutkia asiakkaan toiminnoista, joita johtanut niihin asiakas-loki.

>[AZURE.NOTE] Voit suodattaa **StatusCode** -saraketta ja kolme lokit, mahdollinen asiakas-loki lausekkeen lisääminen suodatin, joka sisältää lokimerkintöjä, missä tilakoodin null tietojen näyttäminen edelleen. Tässä suodatin-lausekkeen muodostaminen avulla:
>
> <code>&#42;StatusCode >= 400 or !&#42;StatusCode</code> 
>
> Tämä suodatin palauttaa kaikki rivit asiakkaan lokin ja vain palvelimen lokista ja HTTP Kirjaudu rivit missä tilakoodi on suurempi kuin 400. Jos haluat käyttää sitä ryhmitelty Ostajantunnus pyynnön ja moduulin näkymä asettelun, voit etsiä tai vieritä etsiminen nykyisten kohtaa, johon kaikki kolme lokit esitetään lokimerkintöjä kautta.   

### <a name="filter-log-data-to-find-404-errors"></a>Suodattaa tietoja 404 virheiden etsiminen

Tallennustilan varat sisältävät esimääritettyjä suodattimia, joiden avulla voit rajata tietoja löytää virheitä tai hakua trendejä. Seuraavaksi on käyttää kaksi ennalta määritettyjä suodattimia: yksi, joka suodattaa palvelimen nimen ja verkko Jäljityslokit 404 virheet ja yksi, joka suodattaa määritetyn aikavälin tiedot.

1. Näytä suodatin tool-ikkunassa, jos se ei ole vielä näkyvissä. Valitse työkalurivillä-valintanauhasta **Työkalun Windows**, valitse **Näkymän suodattimen**.
2. Valitse suodatin-ikkunassa **kirjasto**ja Etsi `Azure Storage` etsiminen Azure-tallennustilan suodattimet. Valitse suodatin **404 (ei löydy)**viestit-kohdassa kaikki lokitiedot.
3. **Kirjasto** -valikko näkyviin, ja Etsi ja valitse **Yleinen aika-suodatinta**.
4. Muokkaa alue, jota haluat katsella suodattimen näkyvät aikaleimat. Tämän avulla voit rajata tiedot, kun haluat analysoida.
5. Suodattimen pitäisi olla seuraavanlainen alla olevassa esimerkissä. Valitse **Käytä** Ota suodatin käyttöön Analysis ruudukkoon.

        ((AzureStorageLog.StatusCode == 404 || HTTP.StatusCode == 404)) And 
        (#Timestamp >= 2014-10-20T16:36:38 and #Timestamp <= 2014-10-20T16:36:39)

![Azure-tallennustilan Näytä asettelu](./media/storage-e2e-troubleshooting-classic-portal/404-filtered-errors1.png)

### <a name="analyze-your-log-data"></a>Lokitiedoston tietojen analysointi

Nyt kun olet ryhmitellyt ja suodattaa tietoja, voit tutkia tietoja yksittäisten pyyntöjen, joka on luotu 404 virheitä. Nykyinen näkymä-asettelu tiedot on ryhmitelty asiakkaan pyynnön tunnus lajitteluperuste loki lähde. Koska olemme ovat suodattaminen pyynnöt missä StatusCode-kenttä sisältää 404, emme tulee palvelimen nimen ja verkko-jäljitystiedot ei asiakkaan lokitiedot.

Alla olevassa kuvassa näkyy erityinen pyyntö, jossa Hae Blob-toiminnon antanut 404, koska blob ei ole. Huomaa, että sarakkeita on poistettu vakionäkymässä asianmukaisten tietojen näyttämiseen.

![Suodatettu palvelimen ja verkon seuranta-lokit](./media/storage-e2e-troubleshooting-classic-portal/server-filtered-404-error.png)

Seuraavaksi on yhdistää tämän pyynnön Ostajantunnus avulla voit tarkastella asiakkaan on ottaen, kun virhe on tapahtunut toiminnot asiakkaan lokitiedot. Voit näyttää uuden Analysis ruudukkonäkymässä tämän istunnon aikana, voit tarkastella asiakkaan lokitiedot, joka avaa toisen-välilehdessä:

1. Kopioi ensin **ClientRequestId** -kentän arvo Leikepöydälle. Voit tehdä tämän valitsemalla joko rivin, etsitään **ClientRequestId** -kentässä, tietoarvo hiiren kakkospainikkeella ja valitsemalla **Kopioi "ClientRequestId"**. 
1. Työkalurivin valintanauhan **Uuden Viewerin**ja valitse Avaa uudessa välilehdessä **Analyysi ruudukko** . Uusi välilehti näyttää kaikki tiedot lokitiedostojen ilman ryhmittely, suodatus tai väri sääntöjä. 
2. Työkalurivi-valintanauhan **Näytä asettelu**ja valitse Valitse **Kaikki .NET asiakkaan sarakkeet** **Azuren tallennustilaan** -osassa. Näytä tämä asettelu näyttää tiedot asiakaskoneesta loki sekä palvelimen nimen ja verkko-jäljityksen lokit. Oletusarvon mukaan tiedot on lajiteltu **MessageNumber** saraketta.
3. Etsi seuraavaksi asiakkaan lokin asiakkaan pyynnön ID-tunnuksellasi. Työkalurivin valintanauhan Valitse **Etsiä viestejä**ja määrittää mukautetun suodattimen pyynnön Asiakastunnus **Etsi** -kenttään. Käytä seuraavaa syntaksia suodattimen määrittäminen oman pyynnön Asiakastunnus:

        *ClientRequestId == "398bac41-7725-484b-8a69-2a9e48fc669a"

Viestin Analyzer paikantaa ja valitsee ensimmäisen lokitapahtuman hakuehdot, jossa vastaa asiakkaan pyynnön ID-tunnuksellasi. Asiakas-lokiin ei useita tapahtumille kukin Ostajantunnus pyynnön, joten haluat ryhmitellä tiedostojasi on helpompi tarkastella niitä kaikkia yhdessä **ClientRequestId** -kenttä. Alla olevassa kuvassa kaikki viestit asiakkaan lokiin määritetyn asiakkaan pyynnön tunnus. 

![Asiakkaan loki näyttäminen 404 virheet](./media/storage-e2e-troubleshooting-classic-portal/client-log-analysis-grid1.png)

Nämä kaksi välilehteä näkymä-asettelut näkyvät tiedot voit analysoida pyynnön tiedot voit selvittää, mikä saattaa aiheuttaa virheen. Voit myös tarkastella pyyntöjä, että niiden näytettyjä nähdäksesi, jos edelliseen tapahtumaan on johtanut 404-virhe. Voit tarkastella esimerkiksi asiakas-lokimerkintöjä ennen tämän pyynnön Asiakastunnus voit määrittää, onko blob on poistettu tai virhe kutsumista CreateIfNotExists API säilö tai blob asiakassovellus vuoksi. Asiakkaan lokiin voit etsiä Blob-objektien osoite **kuvaus** -kenttään; verkon seuranta-lokit ja palvelimen tiedot näkyvät **Yhteenveto** -kenttä.

Kun tiedät, joka on antanut 404-virhe Blob-objektien osoitteen, voit tutkia tarkemmin. Jos etsit muita laskutoimituksia saman Blob-objektien liittyvät viestit log merkintöjä, voit tarkistaa, onko asiakkaan aiemmin poistettu kohde. 

## <a name="analyze-other-types-of-storage-errors"></a>Analysoi muuntyyppisten tallennustilan virheet

Nyt kun olet käyttänyt viestin Analyzer lokin tietojen analysointia varten, voit analysoida muuntyyppisten virheet käyttämällä Näytä asettelujen, värien säännöt ja etsiminen ja suodattaminen. Seuraavissa taulukoissa on lueteltu joitakin liittyvistä ongelmista ja voit paikantaa ne suodatusehdot. Lisätietoja suodattimet ja suodatuksen kielen viestin Analyzer luomisesta on artikkelissa [Suodattaminen viestitiedot](http://technet.microsoft.com/library/jj819365.aspx).

|    Jos haluat tutkia...                                                                                               |    Käytä suodatin-lausekkeen...                                                                                                                                                                                                                                        |    Lausekkeen koskee Log (asiakas, Server ja verkkoon, kaikki)    |
|------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------|
|    Viestin toimituksen jonon odottamattomia viipeitä                                                              |    AzureStorageClientDotNetV4.Description sisältää "Retrying epäonnistui toiminnon."                                                                                                                                                                                |    Asiakkaan                                                        |
|    HTTP PercentThrottlingError suureneminen                                                                       |    HTTP. Response.StatusCode == 500 & #124; & #124; HTTP. Response.StatusCode == 503                                                                                                                                                                                          |    Verkon                                                       |
|    Suurenna PercentTimeoutError                                                                               |    HTTP. Response.StatusCode == 500                                                                                                                                                                                                                             |    Verkon                                                       |
|    Suurenna PercentTimeoutError (kaikki)                                                                         |    * StatusCode == 500                                                                                                                                                                                                                                          |    Kaikki                                                           |
|    Suurenna PercentNetworkError                                                                               |    AzureStorageClientDotNetV4.EventLogEntry.Level < 2                                                                                                                                                                                                          |    Asiakkaan                                                        |
|    HTTP 403 (kielletty) viestit                                                                                 |    HTTP. Response.StatusCode == 403                                                                                                                                                                                                                             |    Verkon                                                       |
|    HTTP (ei löydy) 404 viestit                                                                                 |    HTTP. Response.StatusCode == 404                                                                                                                                                                                                                             |    Verkon                                                       |
|    404 (kaikki)                                                                                                     |    * StatusCode == 404                                                                                                                                                                                                                                          |    Kaikki                                                           |
|    Jaettu Access allekirjoitus (SAS) luvan ongelma                                                             |    AzureStorageLog.RequestStatus == "SASAuthorizationError"                                                                                                                                                                                                     |    Verkon                                                       |
|    HTTP (ristiriita) 409 viestit                                                                                  |    HTTP. Response.StatusCode == 409                                                                                                                                                                                                                             |    Verkon                                                       |
|    409 (kaikki)                                                                                                     |    * StatusCode == 409                                                                                                                                                                                                                                          |    Kaikki                                                           |
|    Pieni PercentSuccess tai analytics lokimerkintöjä on toimintojen ClientOtherErrors tapahtuman tila    |    AzureStorageLog.RequestStatus == "ClientOtherError"                                                                                                                                                                                                         |    Palvelin                                                        |
|    Nagle varoitus                                                                                               |    ((AzureStorageLog.EndToEndLatencyMS-AzureStorageLog.ServerLatencyMS) > (AzureStorageLog.ServerLatencyMS * 1,5)) ja (AzureStorageLog.RequestPacketSize < 1460) ja (AzureStorageLog.EndToEndLatencyMS - AzureStorageLog.ServerLatencyMS > = 200)        |    Palvelin                                                        |
|    Aika-palvelimen nimen ja verkko-alueen lokit                                                                    |    #Aikaleima > = 2014-10-20T16:36:38 ja #Timestamp < = 2014-10-20T16:36:39                                                                                                                                                                                     |    Palvelin-verkossa                                               |
|    Ajanjakson palvelimen lokit                                                                                |    AzureStorageLog.Timestamp > = 2014-10-20T16:36:38 ja AzureStorageLog.Timestamp < = 2014-10-20T16:36:39                                                                                                                                                     |    Palvelin                                                        |


## <a name="next-steps"></a>Seuraavat vaiheet

Saat lisätietoja vianmäärityksen lopusta loppuun käyttötilanteita Azuren tallennustilaan nämä resurssit:

- [Valvonta, diagnosointi ja vianmääritys Microsoft Azuren tallennustilaan](storage-monitoring-diagnosing-troubleshooting.md)
- [Tallennustilan Analytics](http://msdn.microsoft.com/library/azure/hh343270.aspx)
- [Näytön tallennustilan-tilin Azure-portaalissa](storage-monitor-storage-account.md)
- [Siirrä tiedot AzCopy komentorivin-apuohjelman avulla](storage-use-azcopy.md)
- [Microsoft Message Analyzer toimivien opas](http://technet.microsoft.com/library/jj649776.aspx)
 
 
