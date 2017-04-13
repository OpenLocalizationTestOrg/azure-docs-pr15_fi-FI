<properties
    pageTitle="Voit luoda, hallita tai poistaa tallennustilaa-tilin Azure perinteinen portaalissa | Microsoft Azure"
    description="Luoda uuden tallennustilan tilin hallinta tilin pikanäppäimet, poistaminen ja tallennustilan-tilin Azure-portaalissa. Lisätietoja vakio- ja premium tallennustilan tilit."
    services="storage"
    documentationCenter=""
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/26/2016"
    ms.author="robinsh"/>


# <a name="about-azure-storage-accounts"></a>Tietoja Azure-tallennustilan

[AZURE.INCLUDE [storage-selector-portal-create-storage-account](../../includes/storage-selector-portal-create-storage-account.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools](../../includes/storage-try-azure-tools.md)]

## <a name="overview"></a>Yleiskatsaus

Azure-tallennustilan tilin kautta pääset käsiksi Azuren tallennustilaan Azure-Blob, jonossa taulukon tai tiedosto-palveluihin. Tallennustilan-tilisi on yksilöllinen nimitilan Azuren tallennustilaan tieto-objektit. Oletusarvon mukaan-tilisi tiedot ovat käytettävissä vain tilin omistajan.

Tallennustilan tilien kahdenlaisia:

- Vakio-tallennustilan tilin sisältää Blob, taulukko tai jonon tiedoston tallennustilan.
- Premium-tallennustilan tilin tukee tällä hetkellä vain Azure virtuaalikoneen levyille. Katso [Premium tallennustilan: tehokas tallennustila Azure virtuaalikoneen työmääriä](storage-premium-storage.md) Premium tallennustilan perusteellisempaa yleiskatsaus.

## <a name="storage-account-billing"></a>Tallennustilan tilin Laskutus

Sinun on laskuttaa Azure-tallennustilan käyttöäsi tallennustilan tilin perusteella. Tallennustilan kustannukset perustuvat neljä huomioon otettavia seikkoja: tallennustilaa, replikoinnin värimallin tai tallennustilan tapahtumien tiedot juniin.

- Kuinka paljon tallennustilaa tili-palvelun olet viittaa tallennustilaa tietojen tallentamiseen. Kustannusten tallentaminen vain tiedot määritetään tietojen mukaan tallennat ja miten se on replikoida.
- Replikoinnin määrittää, kuinka monta kopiota tietoja ylläpidetään kerralla ja mitä sijainneissa.
- Tapahtumien viitata kaikki lukeminen ja kirjoittaminen toimintojen Azure-tallennustilan.
- Tietoja juniin viittaa tiedonsiirron Azure alueen ulkopuolella. Kun tallennustilan tilisi tiedot käytetään sovellus, joka ei ole käynnissä, samalla alueella, onko sovellus pilvipalveluun tai jonkin muun vastaavan sovelluksen, valitse perittävän tietojen juniin. (Azure-palveluiden voit ryhtyä ryhmitellä tietoja ja saman tietojen keskukset, vähentämiseksi tai poistamiseksi juniin tiedonsiirtomaksuihin palvelujen.)  

[Azure-tallennustilan hinnoittelu](https://azure.microsoft.com/pricing/details/storage) -sivulla on yksityiskohtaiset hintatiedot tallennustilaa, replikoinnin ja tapahtumia. [Tietojen siirrot hinnat tiedot](https://azure.microsoft.com/pricing/details/data-transfers/) -sivu sisältää yksityiskohtaiset tiedot juniin hinnoittelu tietoja.

Lisätietoja tallennustilan tilin kapasiteetin ja suorituskyvyn kohteet on artikkelissa [Azure-tallennustilan skaalattavuus ja suorituskyvyn kohteet](storage-scalability-targets.md).

> [AZURE.NOTE] Luodessasi Azure virtuaalikoneen tallennustilan tilin luodaan sinulle automaattisesti käyttöönoton sijainti Jos sinulla ei ole vielä tallennustilan tilin kyseisessä sijainnissa. Näin ei ole tarpeen virtuaalikoneen levyille tallennustilan tilin luominen seuraavia ohjeita noudattamalla. Tallennustilan tilin nimi perusteella virtuaalikoneen nimi. On lisätietoja [Azuren näennäiskoneiden ohjeissa](https://azure.microsoft.com/documentation/services/virtual-machines/) .

## <a name="create-a-storage-account"></a>Tallennustilan tilin luominen

1. Kirjautuminen [Azure perinteinen Portal](https://manage.windowsazure.com).

2. Valitse **Uusi** sivun alareunassa tehtäväpalkissa. Valitse **Tietopalvelut** | **tallennustilan**, ja valitse sitten **Nopea luominen**.

    ![NewStorageAccount](./media/storage-create-storage-account-classic-portal/storage_NewStorageAccount.png)

3. Kirjoita **URL**-tallennustilan tilin nimi.

    > [AZURE.NOTE] Tallennustilan tilin nimi on oltava 3 – 24 merkkiä ja voivat sisältää numeroita ja pieniä kirjaimia vain.
    >  
    > Tallennustilan tilin nimen on oltava yksilöllinen Azure. Azure perinteinen portaalin osoittavat, jos valitset tallennustilan tilin nimi on varattu.

    Katso lisätietoja siitä, miten tallennustilan tilin nimi käytetään osoite objektit Azuren tallennustilaan [tallennustilan tilin päätepisteet](#storage-account-endpoints) alapuolella.

4. **Sijainti tai affiniteetti ryhmän**Valitse sijainti tallennustilan tilin, joka on lähellä sinun tai asiakkaillesi. Jos tallennustilan tilin tietoja käytetään toisen Azure-palvelusta, kuten Azure virtuaalikoneen tai pilvipalveluun, voit halutessasi voit valita affiniteetti-ryhmän luettelosta ja ryhmitellä Azure palvelut, joita käytät parantaa suorituskykyä ja pienempi kustannukset saman tietokeskuksen tallennustilan-tilin.

    Huomaa, että sinun on valittava affiniteetti-ryhmä, kun tallennustilan-tili on luotu. Et voi siirtää aiemmin luotu tili affiniteetti ryhmään. Lisätietoja affiniteetti ryhmät-kohdassa [palvelun rinnakkain affiniteetti-ryhmästä](#service-co-location-with-an-affinity-group) alla.

    >[AZURE.IMPORTANT] Voit selvittää, mitkä sijainnit ovat käytettävissä tilauksen, voit soittaa [luettelon kaikkien resurssien palveluntarjoajien](https://msdn.microsoft.com/library/azure/dn790524.aspx) -toimintoa. Luettelo PowerShell-palveluja, soita [Get-AzureLocation](https://msdn.microsoft.com/library/azure/dn757693.aspx). Käytä .NET-ProviderOperationsExtensions luokan [luettelo](https://msdn.microsoft.com/library/azure/microsoft.azure.management.resources.provideroperationsextensions.list.aspx) -menetelmää.
    >
    >Saat lisätietoja siitä, mitkä palvelut ovat käytettävissä millä alueella lisäksi [Azure alueet](https://azure.microsoft.com/regions/#services) .


5. Jos sinulla on useampi kuin yksi Azure tilaus, **tilauksen** -kentässä näkyy. Kirjoita **tilauksen**Azure tilaus, jota haluat käyttää tallennustilan tiliä.

6. **Toistoa**Valitse haluamasi taso replikoinnin tallennustilan tilin. Suositeltu replikoinnin-asetus on geo tarpeettomat replikoinnin, joka sisältää enintään kestävyyttä tiedoillesi. Lisätietoja Azure-tallennustilan Replikointiasetukset on artikkelissa [Azure-tallennustilan replikoinnin](storage-redundancy.md).

6. Valitse **Luo tallennustilan tilin**.

    Voi kestää muutaman minuutin kuluttua tallennustilan tilin luominen. Tarkista tila, voit valvoa Azure perinteinen portaalin alareunassa ilmoitukset. Kun tallennustilan-tili on luotu, uusi tallennustilan-tilisi on **Online** -tilaan ja on valmis käytettäväksi.

![StoragePage](./media/storage-create-storage-account-classic-portal/Storage_StoragePage.png)


### <a name="storage-account-endpoints"></a>Tallennustilan tilin päätepisteet

Kaikki objektit, joiden avulla voit tallentaa Azuren tallennustilaan on yksilöllinen URL-osoite. Tallennustilan tilin nimi-lomakkeiden osoite alitoimialueen. Alitoimialueen ja toimialueen nimi, joka on kunkin palvelun-yhdistelmä lomakkeiden *päätepisteen* tallennustilan tilissäsi.

Esimerkiksi jos tallennustilan tilin nimi on *mystorageaccount*, valitse Oletus päätepisteet tallennustilan tilin ovat:

- BLOB-palvelu: http://*mystorageaccount*. blob.core.windows.net

- Taulukon palvelun: http://*mystorageaccount*. table.core.windows.net

- Palvelun jonon: http://*mystorageaccount*. queue.core.windows.net

- Tiedoston palvelun: http://*mystorageaccount*. file.core.windows.net

Näet päätepisteet tallennustilan koontinäytössä [Azure perinteinen Portal](https://manage.windowsazure.com) -tallennustilan tilin, kun tili on luotu.

URL-Osoitteen käyttäminen objektin tallennustilan tilin muodostanut lisäämällä objektin sijainti-tallennustilan tilin päätepisteelle. Esimerkiksi blob-osoite on ehkä tässä muodossa: http://*mystorageaccount*.blob.core.windows.net/*mycontainer*/*myblob*.

Voit myös määrittää tallennustilan-tilisi käyttää mukautettua toimialuenimeä. Lisätietoja on kohdassa [Configure blob storage päätepiste mukautettua toimialuenimeä](storage-custom-domain-name.md) .

### <a name="service-co-location-with-an-affinity-group"></a>Palvelun rinnakkain affiniteetti-ryhmän kanssa

Aiemmin *affiniteetti ryhmä* on maantieteelliset ryhmittely Azure palveluiden ja VMs Azure-tallennustilan tilin kanssa. Affiniteetti-ryhmässä voit paranna suorituskykyä etsimällä tietokoneen työmääriä lähellä käyttäjän kohdeyleisön tai saman tietokeskuksen. Lisäksi ei laskutuksen veloituksia ovat kertyneet juniin luettaessa palvelusta, joka on saman affiniteetti ryhmän tallennustilan tilin tiedot.

> [AZURE.NOTE]  Voit luoda affiniteetti ryhmän Avaa [Azure perinteinen Portal](https://manage.windowsazure.com)- <b>asetukset</b> -alue, <b>affiniteetti</b>-ryhmät ja valitse sitten <b>Lisää affiniteetti-ryhmän</b> tai <b>Lisää</b> -painike. Voit myös luoda ja hallita affiniteetti ryhmiä käyttämällä Azure Service Management API. Lisätietoja on kohdassa <a href="http://msdn.microsoft.com/library/azure/ee460798.aspx">affiniteetti ryhmät-toimintoja</a> .

## <a name="view-copy-and-regenerate-storage-access-keys"></a>Tarkastele, kopioiminen ja luo tallennustilan pikanäppäimet

Kun luot tallennustilan tilin, Azure Luo kaksi 512-bittinen tallennustilan pikanäppäimet, joita käytetään todennusta varten, kun tallennustilan tilin käytetään. Antamalla kaksi tallennustilan pikanäppäinten Azure mahdollistaa näppäimet palvelukatkoksia tallennustilan-palvelussa tai käyttää kyseisen palvelun uudelleen.

> [AZURE.NOTE] On suositeltavaa välttää tallennustilan access avaimien jakaminen kenellekään. Salli tallennustilan resurssien käytön tuomatta access-näppäimiä ulos, voit käyttää *jaettua käyttöä allekirjoitus*. Jaetun access-allekirjoitus on tilisi resurssin käyttöoikeutta, voit määrittää aikavälin ja käyttöoikeuksilla, jotka määrität. Lisätietoja on kohdassa [Käyttämällä jaettu Access allekirjoitukset (SAS)](storage-dotnet-shared-access-signature-part-1.md) .

[Azure perinteinen Portal](https://manage.windowsazure.com)-näppäimillä **Hallinta** koontinäyttö tai **tallennustilan** -sivulla voit tarkastella, kopioida ja luo tallennustilan pikanäppäimet, joita käytetään Blob, taulukon ja jono-palveluiden käyttämiseen.

### <a name="copy-a-storage-access-key"></a>Kopioi tallennustilan pikanäppäin  

Voit **Hallita näppäimet** voit kopioida yhteysmerkkijonon käyttäminen tallennustilan access-näppäintä. Yhteysmerkkijonon edellyttää tallennustilan tilin nimi ja todennus käyttämään avain. Lisätietoja yhteyden merkkijonot Azure tallennustilan palveluiden käyttämiseen artikkelissa [Määrittäminen Azure tallennustilan yhteyden merkkijonoja](storage-configure-connection-string.md).

1. [Azure perinteinen Portal](https://manage.windowsazure.com) **tallennustilan**ja valitse sitten Avaa koontinäyttö-tallennustilan tilin nimi.

2. Valitse **Hallitse avaimet**.

    **Hallitse pikanäppäinten** avautuu.

    ![Managekeys](./media/storage-create-storage-account-classic-portal/Storage_ManageKeys.png)


3. Voit kopioida tallennustilan pikanäppäin, valitse avaimen teksti. Valitse Napsauta hiiren kakkospainikkeella ja valitse **Kopioi**.

### <a name="regenerate-storage-access-keys"></a>Luo tallennustilan pikanäppäimet
On suositeltavaa, muuttaa pikanäppäimet tallennustilan tilin säännöllisesti, jos haluat suojata tallennustilan yhteydet. Kaksi pikanäppäinten määritetään niin, että voit säilyttää yhteyden tallennustilan tilin käyttämällä yhtä access-näppäintä samalla, kun access-näppäintä uudelleen.

> [AZURE.WARNING] Access-näppäimiä uudelleen voivat vaikuttaa Azure sekä omia sovelluksia, jotka ovat riippuvaisia tallennustilan tilin palvelut. Kaikkien asiakkaiden, jotka käyttävät pikanäppäin tallennustilan-tili on päivitettävä uusi avain.

**Media services** - Jos media-palveluja, jotka ovat riippuvaisia tallennustilan-tilisi on uudelleen synkronoit pikanäppäimet media-palvelun kanssa kun näppäimet uudelleen.

**Sovellusten** – Jos verkkosovellusten tai cloud Services-palvelut, jotka käyttävät tallennustilan tilin menetät yhteydet Jos uudelleen näppäimet, ellei avaimien kokoa. 

**Tallennustilan hallinnasta** – Jos käytössäsi on [tallennustilan explorer sovellukset](storage-explorers.md)todennäköisesti haluat päivittää näiden sovellusten käyttämä tallennustila-näppäintä.

Näin prosessi, jossa kiertäminen tallennustilan pikanäppäimet:

1. Päivitä yhteysmerkkijonot sovelluksen koodissa tallennustilan tilin toissijainen access-avain.

2. Luo ensisijainen pikanäppäin tallennustilan tilissäsi. Valitse [Azure perinteinen Portal](https://manage.windowsazure.com)-koontinäytön tai **Määritä** sivun **Hallinta avaimet**. Valitse Valitse ensisijainen pikanäppäin **Luo** ja sitten Vahvista valitsemalla **Kyllä** , johon haluat luoda uuden tunnuksen.

3. Päivitä yhteysmerkkijonot koodisi ensisijainen uusi avain.

4. Luo toissijainen pikanäppäin.

## <a name="delete-a-storage-account"></a>Tallennustilan tilin poistaminen

Voit poistaa tallennustilan-tili, jota käytät enää avulla koontinäyttö tai **määrittäminen** -sivulla **Poista** . **Poista** poistaa koko tallennustilan tilin, myös kaikki BLOB-objektit, taulukoita ja olevien tili.

> [AZURE.WARNING] Ei ole mahdollista Palauta poistetut tallennustilan tilin tai hakea sisältöä, se säilytetään ennen poistamista. Muista varmuuskopioida mitään haluat tallentaa, ennen kuin voit poistaa tilin. Tämä myös pitää paikkansa resurssien tilille, kun poistat blob, taulukkoon, jonossa tai tiedoston, se poistetaan pysyvästi.
>
> Jos tilisi tallennustilan sisältää Azure virtuaalikoneen näennäiskiintolevytiedostoja, valitse sinun on poistettava kuvia ja levyjä, jotka käyttävät näiden näennäiskiintolevytiedostoja, ennen kuin voit poistaa tallennustilaa-tili. Ensin Lopeta virtuaalikoneen, jos se on käynnissä ja poistamalla sen. Poista levyjä, siirry **levyjen** -välilehti ja poista kaikki levyjä. Jos haluat poistaa kuvia, siirry **kuvat** -välilehti ja poista kuvia, jotka on tallennettu tili.

1. Valitse [Azure perinteinen Portal](https://manage.windowsazure.com)- **tallennustilan**.

2. Napsauta mitä tahansa tallennustilan tilin tapahtuma lukuun ottamatta nimi ja valitse sitten **Poista**.

     - Tai -

    Valitse Avaa koontinäyttö-tallennustilan tilin nimi ja valitse sitten **Poista**.

3. Valitse Vahvista, että haluat poistaa tallennustilan tilin **Kyllä** .

## <a name="next-steps"></a>Seuraavat vaiheet

- Saat lisätietoja Azure-tallennustilan on [Azuren tallennustilaan ohjeissa](https://azure.microsoft.com/documentation/services/storage/).
- Siirry [Azure tallennustilan tiimin blogia](http://blogs.msdn.com/b/windowsazurestorage/).
- [Siirrä tiedot ja AzCopy komentorivivalitsimet-apuohjelma](storage-use-azcopy.md)
