<properties
    pageTitle="Azure BLOB hienoja tallennustila | Microsoft Azure"
    description="Tallennustilan tasoa, Azure-Blob-säiliö tarjouksen kustannukset säästävän tallennustila objektitiedot access-kuvioiden perusteella. Hieno tallennustilan taso on tarkoitettu tietojen, jota käytetään kovin usein."
    services="storage"
    documentationCenter=""
    authors="michaelhauss"
    manager="vamshik"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="mihauss"/>


# <a name="azure-blob-storage-hot-and-cool-storage-tiers"></a>Azure-Blob-säiliö: Kuuman ja tallennustilaa tasoa jäähdytetään

## <a name="overview"></a>Yleiskatsaus

Azuren tallennustila on nyt kaksi tallennustilan-tasoa Blob storage (objektin tallennus), niin, että voit tallentaa tiedot eniten eteenpäin riippuen siitä, miten voit käyttää sitä. Azure **kuuma tallennustilan taso** on tarkoitettu, jota käytetään usein tietojen tallentaminen. Azure **hienoja tallennustilan taso** on tarkoitettu tiedot, jotka ovat harvoin käytetyt ja pitkäikäiset tallentamiseen. Tietojen hienoja tallennustilan tason hyväksyt hieman pienempi saatavuus, mutta vaatii edelleen hyvin kestävyyttä ja samalla, kun haluat käyttää ja siirtonopeuden ominaisuudet kuuma tietoina. Hieno tietojen hieman pienempi käytettävyys SLA ja access kustannuksia on hyväksyttävä valinnat paljon alempi tallennustilan kustannusten.

Nykyään pilveen tallennettujen tietojen kasvaa eksponentiaalisen tahdissa. Kustannusten laajennettavassa tallennustilan tarpeisiin hallinnan on hyödyllinen, kuten korkojakso ja suunnitellun säilytysaika määritteiden perusteella tietojen järjestämiseen. Pilveen tallennettujen tietojen voi olla aivan eri myös siitä, miten se on luotu, käsitellä ja käyttää aikana kautta. Tietoja on aktiivisesti käyttää ja muokata koko aikana. Tietoja käytetään hyvin usein etuajassa aikana, valitse pudottaminen merkittävästi iät tietoja Accessissa. Tietoja käyttämättömänä pilveen ja vain harvoin, jos koskaan, käyttää kerran tallennettu.

Kaikkien tietojen käytön tilanteisiin kuvattuun eriteltyjen taso tallennustilaa, joka on optimoitu tietyn käyttötapa eduista. Myötä kuuman ja hienoja tallennustilan tasoa Azure Blob storage korjaa nyt eriteltyjen tallennustilan tasoa kanssa tarvetta erota hinnoittelua mallit.

## <a name="blob-storage-accounts"></a>BLOB storage tilit

**BLOB storage-tilit** ovat erityisiä tallennustilan tilit rakenteeton tietojen tallentaminen nimellä Azuren tallennustilaan BLOB-objektit (objekteja). Blob storage tilien nyt voit tallentaa kuuman ja hienoja tallennustilan tasojen välissä kovin usein käytetyt hienoja tietojen etsiminen alempi tallennustilan kustannusten ja tallentaa usein käytetyt kuuma tietoja alemman access, kustannukset. BLOB storage tilit ovat olemassa yleinen tallennustilan tilit ja jakaa kaikki hyvien kestävyyttä, käytettävyyttä, skaalattavuus, ja suorituskykyominaisuudet, joita käytät tänään, 100 %: n API yhdenmukaisuuden estä BLOB-objektit, mukaan lukien ja liitä BLOB-objektit.

> [AZURE.NOTE] BLOB storage tilit tukee vain estäminen ja liittää BLOB-objektit ja sivun BLOB.

BLOB storage tilit Näytä **Accessin taso** -määrite, jonka avulla voit määrittää tallennustilan tason **kuuman** tai **hienoja** mukaan tili tallennettuja tietoja. Jos näkyvissä on muuttunut käyttö tietojen rakenteessa, voit vaihtaa myös nämä tallennustilan tasojen milloin tahansa välissä.

> [AZURE.NOTE] Tallennustilan tason muuttaminen voi aiheuttaa muita kulut. Katso lisätietoja [hinnoittelu ja laskutus](storage-blob-storage-tiers.md#pricing-and-billing) -osan.

Esimerkki käyttötapoja kuuma tallennustilan tason ovat seuraavat:

- Tiedot, jotka on käytössä tai voi käyttää (luku- ja kirjattu) usein tarkoitus.
- Tiedot, jotka on vaiheistettu käsittely ja potentiaalisen siirto hienoja tallennustilan taso.

Esimerkki käyttötapoja hienoja tallennustilan tason ovat seuraavat:

- Varmuuskopiointi-arkistointia ja tietojen palauttaminen tietojoukkoja.
- Vanhat mediasisältöä ei usein enää tarkastella, vaan se on tarkoitus olla käytettävissä heti, kun käyttää.
- Suurten tietojoukkojen, joka on oltava tallennetut kustannukset tehokkaasti, kun enemmän tietoja kerätään myöhempää käsittelyä varten. (*kuten*pitkään tieteellisen tietojen, tuotannon tilan raaka telemetriatietojen tietojen säilyttäminen)
- Alkuperäinen (alustamaton) tiedot, jotka on säilytettävä, vaikka se käsitelty lopullinen käytettävä lomakkeeseen. (*kuten*raaka mediatiedostojen koodauksen siirtäminen toiseen muotoon muuntamisen jälkeen)
- Yhteensopivuus ja arkistointia tiedot, jotka on tallennettava pitkään ja niitä käytetään tuskin koskaan. (*kuten*suojauksen kameran videomateriaali vanha X-Rays/MRIs terveydenhuollon organisaatiot, äänitallenteiden ja tallenteet asiakkaan soittaa taloudellisen tilanteen Services)

Saat lisätietoja tallennustilan tilien [tietoja Azure-tallennustilan tilit](storage-create-storage-account.md) .

Sovellukset edellyttävät vain estää tai liittäminen Blob-objektien tallennustilaan, suosittelemme Blob storage tilit avulla voit hyödyntää Porrastettu tallennustilan eriteltyjen hinnoittelu malli. Kuitenkin Ymmärrämme, että ei ehkä ole mahdollista kaikissa tilanteissa Jos yleinen tallennustilan tilien olisi tapa siirtyä, kuten:

- Haluat käyttää taulukoita ja olevien tiedostojen ja haluat oman BLOB tallennettu saman tallennustilan tilin. Huomaa, että mitään tekniset etua tallentaminen näitä samaa tiliä, joilla on sama kuin on jaettu avaimet.
- Haluat käyttää perinteinen käyttöönotto-malli. BLOB storage tilit ovat vain kautta Azure resurssien hallinnan käyttöönottomalli.
- Sinun on käytettävä sivun BLOB-objektit. BLOB storage tilit eivät tue sivun BLOB-objektit. Yleensä suositella käyttämällä estä BLOB-objektit, ellei sinulla ole tietyn tarvetta sivun BLOB-objektit.
- [Tallennustilan Services REST API](https://msdn.microsoft.com/library/azure/dd894041.aspx) , joka on aiempi kuin 2014-02 – 14 tai asiakkaan kirjaston version käyttäminen versio, joka on pienempi kuin 4.x ja sovelluksen päivittäminen ei onnistu.

> [AZURE.NOTE] BLOB storage tilit ovat tällä hetkellä tukemien Azure alueet, joiden Lisää useimpia seuraa. Voit etsiä päivitetyn luettelon käytettävissä alueista [Alueittain Azure-palvelut](https://azure.microsoft.com/regions/#services) -sivulla.

## <a name="comparison-between-the-storage-tiers"></a>Tallennustilan tasoa vertailu

Seuraavassa taulukossa on esitelty tallennustilan kaksi tasoa vertailu:

<table border="1" cellspacing="0" cellpadding="0" style="border: 1px solid #000000;">
<col width="250">
<col width="250">
<col width="250">
<tbody>
<tr>
    <td><strong><center></center></strong></td>
    <td><strong><center>Kuuma tallennustilan taso</center></strong></td>
    <td><strong><center>Hieno tallennustilan taso</center></strong></td
</tr>
<tr>
    <td><strong><center>Käytettävyys</center></strong></td>
    <td><center>99,9 %</center></td>
    <td><center>99 %</center></td>
</tr>
<tr>
    <td><strong><center>Käytettävyys<br>(RA GRS lukee)</center></strong></td>
    <td><center>99,99 %</center></td>
    <td><center>99,9 %</center></td>
</tr>
<tr>
    <td><strong><center>Käyttökustannukset</center></strong></td>
    <td><center>Tallennustilan kustannuksia<br>Pienet access ja tapahtuman kustannukset</center></td>
    <td><center>Alempi tallennustilan kustannukset<br>Käyttöoikeudet ja tapahtuman kustannuksia</center></td>
</tr>
<tr>
    <td><strong><center>Pienin objektin koko<center></strong></td>
    <td colspan="2"><center>PUUTTUU</center></td>
</tr>
<tr>
    <td><strong><center>Tallennustilan vähimmäiskoko kesto<center></strong></td>
    <td colspan="2"><center>PUUTTUU</center></td>
</tr>
<tr>
    <td><strong><center>Viive<br>(Ensimmäinen tavu aika)<center></strong></td>
    <td colspan="2"><center>millisekunteina</center></td>
</tr>
<tr>
    <td><strong><center>Skaalattavuus ja suorituskyvyn kohteisiin<center></strong></td>
    <td colspan="2"><center>Sama kuin Yleinen tallennustilan tilit</center></td>
</tr>
</tbody>
</table>

> [AZURE.NOTE] BLOB storage tilit tukevat sama suorituskyky ja skaalattavuus kohteet yleinen tallennustilan tilit. Lisätietoja on kohdassa [Azure-tallennustilan skaalattavuus ja suorituskyvyn kohteet](storage-scalability-targets.md) .

## <a name="pricing-and-billing"></a>Hinnat ja laskutus

BLOB storage tilien käyttäminen uuden hinnoittelu mallin Blob-objektien tallennustilaan tallennustilan tason perusteella. Blob storage-tiliä käytettäessä ottaa laskutuksen huomioon seuraavat asiat:

- **Tallennustilan kustannuksia**: lisäksi tallennettujen tietojen määrä kustannusten tietojen tallentaminen vaihtelee riippuen tallennustilan taso. Kustannus gigatavua kohden on pienempi kuin hienoja tallennustilan tason kuuma tallennustilan aikayksikön.
- **Data access kustannuksia**: Jos tiedot ovat hienoja tallennustilan taso, jota veloitetaanko varausta kohden gigatavun tiedot access lukee ja kirjoittaa.
- **Tapahtuman kustannukset**: on sekä tasoa kohden tapahtuman maksu. Hienoja tallennustilan tason tapahtuman kohti kustannukset on suurempi kuin kuuma tallennustilan tason.
- **GEO replikoinnin tiedonsiirron kustannuksia**: Tämä koskee vain tilit toistot geo-määritetty, mukaan lukien GRS ja RA GRS. GEO replikoinnin tiedonsiirto veloitetaan gigatavua kohden maksu.
- **Lähtevien tietojen siirtäminen kustannuksia**: lähtevä tiedonsiirron (tiedot, jotka on siirretty pois Azure alue) maksamaan kaistanleveyden käytön gigatavua kohden väliajoin johdonmukaisia verrattuna yleinen tallennustilan tilien laskutus.
- **Tallennustilan tason muuttaminen**: muuttaminen tallennustilan tason jäähdytetään kuuman selvittää varausta yhtä luetaan kaikki tallennustilan tilin jokaisen siirtymän olevia tietoja. Toisaalta vaihdetaan tallennustilan tason kuuma jäähtyä ovat maksuttomia kustannukset.

> [AZURE.NOTE] Jotta käyttäjät voivat kokeilla uusi tallennustilan tasoa ja vahvista toimintoja julkaisemisen jälkeen maksu tallennustilan muuttaminen taso-jäähdytetään kuuman poiketa käytöstä 30 kesäkuussa 2016 asti. 1st heinäkuussa 2016 alkaen kulun käytetään kaikkien siirtymät-jäähdytetään kuuman. Lisätietoja Blob-objektien tallennustilaan tilit näkyviin [Azure-tallennustilan hinnat](https://azure.microsoft.com/pricing/details/storage/) sivun hinnoittelutiedot mallista. Lisätietoja lähtevän tietojen siirto ovat näkyvissä, [Tietojen siirrot hinnat tiedot](https://azure.microsoft.com/pricing/details/data-transfers/) -sivu.

## <a name="quick-start"></a>Pika-aloitusopas

Tässä osassa on esitetty Azure-portaalissa seuraavissa tilanteissa:

- Voit luoda Blob storage tilin.
- Miten Blob-tallennustilan tilin hallinta.

### <a name="using-the-azure-portal"></a>Azure-portaalissa

#### <a name="create-a-blob-storage-account-using-the-azure-portal"></a>Azure-portaalissa Blob storage tilin luominen

1. Kirjautuminen [Azure portal](https://portal.azure.com).

2. Valitse toiminto-valikosta **Uusi** > **tietojen + tallennustilan** > **tallennustilan tilin**.

3. Kirjoita tallennustilan tilin nimi.

    Tämän nimen on oltava yksilöivä; sitä käytetään osana käyttää objektit-tallennustilan tilin URL-osoite.  

4. Valitse **Resurssienhallinta** käyttöönoton mallina.

    Porrastettu tallennustilan voidaan käyttää vain Resurssienhallinta tallennustilan tilit; Tämä on uusi resurssien suositellut käyttöönottomalli. Lisätietoja on Katso [Azure resurssin hallinnassa: yleiskatsaus](../azure-resource-manager/resource-group-overview.md).  

5. Valitse tilin tyyppi avattavasta luettelosta, **Blob-objektien tallennustilaan**.

    Tämä on missä tallennustilan tilin tyypin valitseminen. Porrastettu tallennustila ei ole käytettävissä yleinen tallennustilan; se on käytettävissä vain Blob storage tyyppi-tilille.    

    Huomaa, että kun valitset tämän vaihtoehdon suorituskyvyn taso on määritetty vakio. Porrastettu tallennustilan ei ole käytettävissä Premium suorituskyvyn taso.

6. Replikoinnin vaihtoehto tallennustilan tilin: **LRS**, **GRS**tai **RA GRS**. Oletusarvo on **RA GRS**.

    LRS = paikallisesti tarpeettomat tallennustilan; GRS = geo tarpeettomat storage (2 alueet); RA GRS on lukuoikeudet geo tarpeettomat tallennus (2 alueet, joiden toinen lukuoikeudet).

    Lisätietoja Azure-tallennustilan Replikointiasetukset Tutustu [Azuren tallennustilaan replikoinnin](storage-redundancy.md).

7. Valitse oikea tallennustilan tason tarpeisiin: **Access-tason** asettaminen **hienoja** tai **kuuman**. Oletusarvo on **kuumalla**.

8. Valitse tilaus, johon haluat luoda uuden tallennustilan tilin.

9. Määritä uusi resurssiryhmä tai valitse aiemmin luotu resurssiryhmä. Lisätietoja resurssiryhmät on artikkelissa [Azure Resurssienhallinta yleiskatsaus](../azure-resource-manager/resource-group-overview.md).

10. Valitse alue tallennustilan tilissäsi.

11. Valitse **Luo** tallennustilan-tilin luominen.

#### <a name="change-the-storage-tier-of-a-blob-storage-account-using-the-azure-portal"></a>Muuta tallennustilan tason Blob storage tilin Azure-portaalissa

1. Kirjautuminen [Azure portal](https://portal.azure.com).

2. Siirry tilisi tallennustilan kaikki resurssit ja valitse tallennustilan tilin.

3. Valitse asetukset-sivu **määritysten** tarkasteleminen ja/tai muuttaa tilin määrittäminen.

4. Valitse oikea tallennustilan tason tarpeisiin: **Access-tason** asettaminen **hienoja** tai **kuuman**.

5. Valitse Tallenna sivu yläreunassa.

> [AZURE.NOTE] Tallennustilan tason muuttaminen voi aiheuttaa muita kulut. Katso lisätietoja [hinnoittelu ja laskutus](storage-blob-storage-tiers.md#pricing-and-billing) -osan.

## <a name="evaluating-and-migrating-to-blob-storage-accounts"></a>Arvioimisen ja Blob storage tilit siirtyminen

Tässä osassa on avulla käyttäjät voivat tehdä sujuva siirtyminen Blob storage tilien kautta. On kaksi käyttäjäskenaariot:

- On yleinen tallennustilan tili ja haluat arvioida muutoksen Blob storage tilin kanssa oikean tallennustilan taso.
- Olet päättänyt Blob storage-tilin tai jo kirjoittamisen ja haluat arvioida, sinun on käytettävä kuuman tai hienoja tallennustilan taso.

Kummassakin tapauksessa ensimmäisen tilauksen of business on arvioimiseen tallentamiseen ja käyttämiseen Blob storage tilillä tallennetut tiedot ja verrata, nykyisen kustannuksiin.

### <a name="evaluating-blob-storage-account-tiers"></a>Arvioimisen Blob storage tilin tasoa

Jotta arvioimiseen tallentamiseen ja käyttämiseen tilillä Blob-objektien tallennustilaan tallennettuja tietoja, sinun on käyttö kuviota tai arvioida lähentää oletettu käyttö kuvio. Yleensä haluat tietää:

- Tallennustilan-kulutus - kuinka paljon tiedot tallentuvat ja kuinka Muuttaako tämä kuukausittain?
- Tallennustilan access - kuvion parhaillaan tietojen lukeminen ja (mukaan lukien uudet tiedot)-tilille kirjoitetaan? Kuinka monta tapahtumaa käytettäviä tietojen käytön ja millaisia tapahtumat ovat ne?

#### <a name="monitoring-existing-storage-accounts"></a>Olemassa olevan tallennustilan tilit seuranta

Olemassa olevan tallennustilan-tilisi ja kokoa tarvitsemasi tiedot, voit tehdä Azure-tallennustilan Analytics joka tekee kirjaaminen ja mahdollistaa tietojen arvot tallennustilan tilin käyttäminen.
Tallennustilan Analytics voi tallentaa arvot, jotka sisältävät sekä yleinen tallennustilan tilit sekä Blob storage tilit Blob storage-palveluun koostetun tapahtuman Tilasto- ja kapasiteetin tietoja.
Tiedot tallennetaan tunnetun taulukoiden saman tallennustilan tilin.

Lisätietoja on artikkelissa [Tietoja tallennustilan Analytics arvot](https://msdn.microsoft.com/library/azure/hh343258.aspx) ja [Tallennustilaa Analytics arvot taulukko rakenne](https://msdn.microsoft.com/library/azure/hh343264.aspx)

> [AZURE.NOTE] BLOB storage tilit Näytä taulukon Palvelupäätepisteen tallentamista ja arvot tietojen tilin käyttäminen.

Tarvitset seurannassa tallennustilan kulutus Blob storage-palvelun käyttöön kapasiteetin arvot.
Tämä asetus on käytössä, jossa kapasiteetin tiedot on tallennettu päivittäinen tallennustilan tilin Blob-palvelun ja tallennettuja taulukon tapahtumana, joka on kirjoitettu saman tallennustilan tilin *$MetricsCapacityBlob* -taulukkoon.

Tarvitset seurannassa Blob storage palvelun tiedot access-kuvion käyttöön tunnin välein tapahtuman mittarit, API tasolla.
Tämä asetus on käytössä kohti API tapahtumat ovat koostetun tunnissa ja tallentanut taulukon tapahtumana, joka on kirjoitettu saman tallennustilan tilin *$MetricsHourPrimaryTransactionsBlob* -taulukkoon. *$MetricsHourSecondaryTransactionsBlob* taulukon tietueiden toissijainen päätepisteen Jos RA GRS tallennustilan tilien tapahtumat.

> [AZURE.NOTE] Siltä varalta, että sinulla on yleinen tallennustilan-tili, jossa on tallennettu sivun BLOB-objektit ja virtuaalikoneen levyjen rinnalla lohko ja Blob-objektien tietoihin, arvio prosessia ei ole käytettävissä. Tämä johtuu siitä voi esiintyä ei voi myöntäneen kapasiteetin ja tapahtuman arvot blob lajin perusteella vain estäminen ja liitä BLOB-objektit, jotka voidaan siirtää Blob storage tiliin.

Saat hyvän kulman tietojen kulutus ja käyttötapa, suosittelemme Valitse arvot, joka on Normaali käyttö edustaja säilytysaika ja ekstrapoloida.
Yksi vaihtoehto on säilytettävä arvot tiedot seitsemän päivää ja kerää tietoja viikko analysointiin kuukauden lopussa.
Toinen vaihtoehto on säilyttää viimeisten 30 päivän aikana arvot tietojen kerääminen ja analysoida tietoja 30 päivän kauden lopussa.

Lisätietoja ottamisesta käyttöön on mittarit tietojen keräämistä ja tarkastelemista, katso, [ottaminen käyttöön Azure-tallennustilan arvot ja tarkastelemisessa arvot](storage-enable-and-view-metrics.md).

> [AZURE.NOTE] Tallentamiseen, käyttämiseen ja lataaminen analytics-tiedot myös veloitetaan aivan kuin tavallinen käyttäjätiedot.

#### <a name="utilizing-usage-metrics-to-estimate-costs"></a>Käyttävien käyttö mittarit kustannusarvioita

##### <a name="storage-costs"></a>Tallennustilan kustannukset

Uusimman tapahtuman kapasiteetin arvot taulukon *$MetricsCapacityBlob* rivin avaimen *"tiedot"* näyttää käyttämä käyttäjätiedot tallennustilaa.
Uusimman tapahtuman kapasiteetin arvot taulukon *$MetricsCapacityBlob* kanssa rivin avaimen *"analytics"* näyttää käyttämä analytics-lokit tallennustilaa.

Tämä kokonaiskapasiteetti kulutettu mukaan sekä käyttäjätiedot ja analyysitiedot lokit (Jos käytössä) voidaan käyttää tietojen tallentaminen-tallennustilan tilin kustannusten arvioiminen.
Samalla tavalla myös voi käyttää tallennustilan lohkon kustannusten arvioiminen ja liitä BLOB yleinen tallennustilan tilin.

##### <a name="transaction-costs"></a>Tapahtuman kustannukset

*'TotalBillableRequests'*summa kaikissa tietueissa API tapahtuman arvot taulukon ilmaisee, että tietty API tapahtumien kokonaismäärä. *kuten* *"GetBlob"* tapahtumien tiettynä ajanjaksona kokonaismäärä voidaan laskea kaikki merkinnät laskutettavan pyyntöjen kokonaismäärä summan mukaan rivin avaimella *' käyttäjän; GetBlob'*.

Jotta Blob storage tilit tapahtuman kustannusarvioita, tarvitset eritellä tapahtumat kolmeen ryhmään, koska ne laitteiden hinnoittelu eri tavalla.

- Kirjoita tapahtumia, kuten *'PutBlob'*, *'PutBlock'*, *'PutBlockList'*, *'AppendBlock'*, *'ListBlobs'*, *'ListContainers'*, *'CreateContainer'*, *'SnapshotBlob'*ja *'CopyBlob'*.
- Poista tapahtumat, kuten *'DeleteBlob'* ja *'DeleteContainer'*.
- Muita tapahtumia.

Jotta yleinen tallennustilan tilit tapahtuman kustannusarvioita, sinun on koosta riippumatta toiminnon/Ohjelmointirajapinnan kaikki tapahtumat.

##### <a name="data-access-and-geo-replication-data-transfer-costs"></a>Tietojen käytön ja geo replikoinnin tietojen siirtäminen kustannukset

Kun tallennustilan analytics ei tarjoa tietojen lukeminen ja tallennustilaa tilin kirjoitetaan määrää, se voidaan karkeasti arvioida katsomalla tapahtuman arvot taulukon.
*'TotalIngress'* kaikissa tietueissa API tapahtuman arvot taulukon summa ilmaisee, että tietty API tunkeutumisen tietojen tavujen määrä.
Vastaavasti *'TotalEgress'* summa ilmaisee juniin tietojen tavuina kokonaismäärä.

Arvioida Blob storage tilien tiedot access-kustannukset, tarvitset eritellä tapahtumat kahteen ryhmään.

- Tallennustilan tilin tiedot määrä on arvioitu katsomalla ensisijaisesti *'GetBlob'* ja *'CopyBlob'* toimille *'TotalEgress'* summa.
- Tallennustilan tilille tietojen määrää on arvioitu *'TotalIngress'* summa katsomalla ensisijaisesti *'PutBlob'*, *'PutBlock'*, *'CopyBlob'* ja *'AppendBlock'* toiminnot.

Geo replikoinnin tiedonsiirto Blob storage tilien kustannukset voi myös laskea arvion tietomäärää varten kirjoitettu Jos GRS tai RA GRS tallennustilan-tilin avulla.

> [AZURE.NOTE] Yksityiskohtaisempia esimerkiksi tietoja laskemisesta kustannuksiin kuuman tai hienoja tallennustilan tason avulla Tutustu Kysyttyjen *mitä kuuman ja jäähdytetään access tasoa ja miten use? kumpaa kannattaa määrittää* katsaus [Azure tallennustilan hinnat sivun](https://azure.microsoft.com/pricing/details/storage/).

### <a name="migrating-existing-data"></a>Olemassa olevien tietojen siirtäminen

Blob storage-tili on suunniteltu projektiin vain lohko ja liitä BLOB-objektit. Aiemmin luotuja yleinen tallennustilan-tilejä, joiden avulla voit tallentaa taulukoiden, olevien, tiedostoja ja levyjen sekä blob ei voi muuntaa Blob storage tilit. Haluat käyttää tallennustilan tasoa, sinun on luoda uusia Blob storage tilejä ja siirtää tiedot juuri luomasi tilit.
Voit siirtää olemassa olevia tietoja Blob storage tilit paikallinen tallennuslaitteet, kolmannen osapuolen pilvitallennussijainnista tallennustilan tarjoajat tai aiemmin yleinen tallennustilan tileistä Azure-tietokannassa seuraavista tavoista:

#### <a name="azcopy"></a>AzCopy

AzCopy on tarkoitettu tietojen kopioiminen tehokas ja sieltä pois Azuren tallennustilaan Windows komentorivin apuohjelma. Voit käyttää AzCopy tietojen kopioimiseen Blob storage tilille aiemmin yleinen tallennustilan tileistä tai ladata tietoja paikallisen tallennuslaitteet Blob storage-tilillesi.

Lisätietoja on artikkelissa [tiedonsiirron AzCopy komentorivivalitsimet-apuohjelman avulla](storage-use-azcopy.md).

#### <a name="data-movement-library"></a>Tietojen siirto-kirjasto

Azure tallennustilan tietojen siirtämistä kirjasto .NET perustuu core tietojen siirtämistä puitteissa, joka tehostaa AzCopy. Kirjasto on suunniteltu tehokas, luotettavaa ja helposti siirtojen samalla AzCopy. Voit ottaa ominaisuuksia AzCopy sovelluksen grafiikkatiedostomuotoja eikä sinun tarvitse huolehtia käynnissä ja seuranta AzCopy ulkoisen esiintymät koko edut.

Lisätietoja on artikkelissa [Azure tallennustilan tietojen siirtämistä kirjasto .net](https://github.com/Azure/azure-storage-net-data-movement)

#### <a name="rest-api-or-client-library"></a>REST API tai asiakas-kirjasto

Voit luoda mukautetun sovelluksen, voit siirtää tietoja jollakin Azure asiakas-kirjastoja tai Azure liittyviä palveluja REST API Blob storage tilille. Azure-tallennustilan tarjoaa useita kieliä ja ympäristöjen, kuten .NET, Java, C++, Node.JS, PHP, Ruby ja Python rich client-kirjastot. Asiakas-kirjastojen tarjouksen lisäasetusten ominaisuudet, kuten uudelleen logiikan, kirjaaminen ja rinnakkaisia lataukset. Voit myös kehittää suoraan vastaan REST-Ohjelmointirajapinta, joita voidaan kutsua kielen, joka tekee HTTP/HTTPS-pyyntöjen perusteella.

Lisätietoja on artikkelissa [Azure-Blob-säiliö käytön aloittaminen](storage-dotnet-how-to-use-blobs.md).

> [AZURE.NOTE] Salattu asiakkaan salauksen BLOB tallentaa salaus liittyviä metatietoja tallennettujen blob. On ehdottoman tärkeää, että kaikki kopio-järjestelmä on varmistettava blob-metatiedot ja erityisesti salaus liittyviä metatietoja, säilytetään. Jos kopioit ilman metatiedot BLOB-blob-sisällön eivät ole enää käytettävissä uudelleen. Saat lisätietoja salauksen liittyviä metatietoja koskevien [Azuren tallennustilaan asiakaspuolen salausta](storage-client-side-encryption.md).

## <a name="faqs"></a>Usein kysyttyjä kysymyksiä

1. **Tallennustilan tilit on edelleen käytettävissä on olemassa?**

    Kyllä, olemassa olevan tallennustilan-tilit ovat käytettävissä ja hinnat tai toimintoja ennalleen.  Ne ei ole mahdollisuus valita tallennustilan taso ja ei ole tiering ominaisuuksia.

2. **Miksi ja milloin kannattaa aloittaa Blob storage tilien kautta?**

    BLOB storage tilit ovat erilaisia projektiin BLOB-objektit ja esittele blob keskitettyä uusia ominaisuuksia, jotta. Exceliä Blob storage-tilit ovat suositeltu tapaa projektiin BLOB-kuin tulevien ominaisuudet, kuten hierarkkisia tallennustilan ja tiering se esitellään tämän tilin tyypin mukaan. On kuitenkin enintään, kun haluat siirtää business tarpeen mukaan.

3. **Olemassa olevan tallennustilan tilini muuntaa Blob storage tiliin**

    Ei. BLOB storage tilin on muusta tallennustilan tilin ja luo uusi ja siirtää tietoja, kuten edellä on selitetty tarvitset.

4. **Objektien tallentaa sekä tallennustilan tasoa saman tilin**

    Mikä tarkoittaa tallennustilan tason *' Access taso-* määrite on määritetty tilin tasolla ja koskee kaikkia objekteja kyseisessä tilissä. Et voi määrittää access taso-määritteen objektin tasolla.

5. **Voit muuttaa tallennustilan tason Blob storage tilin?**

    Kyllä. Voit voi muuttaa tallennustilan tason määrittämällä tallennustilan tilin *' Access taso-* määrite. Tallennustilan tason muuttaminen koskevat kaikki objektit, jotka on tallennettu tili. Muuta tallennustilan taso-kuuma jäähtyä ei selvittää minkä tahansa kulujen muutettaessa-jäähdytetään kuuman selvittää lukee kaikki tilin tiedot kustannus Gigatavua kohden.

6. **Kuinka usein tallennustilan tason Blob storage tilin vaihdetaan?**

    Vaikka emme Älä pakota tiheyden tallennustilan tason voi muuttaa rajoitus, huomioi, vaihtaa tallennustilan taso-jäähdytetään kuuman selvittää merkittäviä kulut. Emme suosittele muuttaminen tallennustilan tason usein.

7. **On hienoja tallennustilan taso-BLOB toimivat eri tavalla kuin mitä kuuma tallennustilan tason?**

    Kuuma tallennustilan tason BLOB on sama kuin BLOB viive yleinen tallennustilan tilit. Hieno tallennustilan tason BLOB on samanlainen viive (millisekunteina) kuin BLOB yleinen tallennustilan tilit.

    Hieno tallennustilan tason BLOB on hieman käytettävyys palvelun alemmaksi (SLA) kuin kuuma tallennustilan tason tallennetaan BLOB-objektit. Lisätietoja on artikkelissa [tallennustilan SLA](https://azure.microsoft.com/support/legal/sla/storage).

8. **Voin tallentaa sivun BLOB-objektit ja virtuaalikoneen levyjen Blob storage tilien?**

    BLOB storage tilit tukee vain estäminen ja liittää BLOB-objektit ja sivun BLOB. Azure virtuaalikoneen levyjen varmuuskopioidaan mukaan sivun BLOB-objektit ja tuloksena Blob storage tilit ei voi käyttää virtuaalikoneen levyjen tallentamiseen. On kuitenkin mahdollista tallentaa varmuuskopiot virtuaalikoneen levyjä estä BLOB Blob storage tilillä.

9. **Minulla on muutettava Omat sovellukset Blob storage tilejä?**

    BLOB storage tilit ovat 100 %: n API yleinen tallennustilan tilit estä mukainen, ja liitä BLOB-objektit. Kun sovellus on käyttämällä estäminen BLOB-objektit ja liittää BLOB- ja käytät [Tallennustilan Services REST API](https://msdn.microsoft.com/library/azure/dd894041.aspx) tai suurempi 2014-02-14-versiota sitten sovellus toimii vain. Jos käytössäsi on protokolla vanhempi versio, täytyy päivittää sovelluksen käyttämään uutta versiota siten, että se toimii saumattomasti kanssa sekä tallennustilan tilityypit. Yleensä aina suositellun käytössä ovat uusimmat riippumatta siitä, mitä tallennustilan tilin lajin Käytämme.

10. **Käyttäjäkokemuksen muutoksen on olemassa?**

    BLOB storage tilit ovat hyvin samankaltaisia kuin Yleinen tallennustilan-tilien projektiin lohko ja liittää BLOB-objektit ja tukevat kaikkia ominaisuuksia Azure-tallennustilan, mukaan lukien hyvin kestävyyttä ja käytettävyyden, skaalattavuus, suorituskyky ja suojaus. Kuin ominaisuudet ja rajoitukset tietyn Blob storage tileille ja sen tallennustilan tasoa, joka on kutsuttu yläpuolella monipuolisista säilyy ennallaan.

## <a name="next-steps"></a>Seuraavat vaiheet

### <a name="evaluate-blob-storage-accounts"></a>Arvioi Blob storage tilit

[Käytettävyyden tarkistaminen Blob storage tilit alueittain](https://azure.microsoft.com/regions/#services)

[Arvioi nykyistä tallennustilan asiakkaiden käyttö ottamalla Azure-tallennustilan arvot](storage-enable-and-view-metrics.md)

[Tarkista Blob storage hinnat alueittain](https://azure.microsoft.com/pricing/details/storage/)

[Tarkista tiedonsiirron hinnat](https://azure.microsoft.com/pricing/details/data-transfers/)

### <a name="start-using-blob-storage-accounts"></a>Blob storage tilien käytön aloittaminen

[Azure-Blob-säiliö käytön aloittaminen](storage-dotnet-how-to-use-blobs.md)

[Tietojen siirtäminen ja Azuren tallennustilaan](storage-moving-data.md)

[Siirrä tiedot ja AzCopy komentorivivalitsimet-apuohjelma](storage-use-azcopy.md)

[Selaa ja tallennustilaa asiakkaiden tutkiminen](http://storageexplorer.com/)
