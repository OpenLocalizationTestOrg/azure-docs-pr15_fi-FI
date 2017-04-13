<properties
    pageTitle="Voit luoda, hallita tai poistaa tallennustilaa-tilin Azure-portaalissa | Microsoft Azure"
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

Azure-tallennustilan tilin avulla voit tallentaa ja käyttää Azuren tallennustilaan tieto-objektit yksilöllisen nimitilan. Kaikissa objekteissa tallennustilan tilin laskutettu yhdessä yhtenä ryhmänä. Oletusarvon mukaan-tilisi tiedot ovat käytettävissä vain tilin omistajan.

[AZURE.INCLUDE [storage-account-types-include](../../includes/storage-account-types-include.md)]

## <a name="storage-account-billing"></a>Tallennustilan tilin Laskutus

[AZURE.INCLUDE [storage-account-billing-include](../../includes/storage-account-billing-include.md)]

> [AZURE.NOTE] Luodessasi Azure virtuaalikoneen tallennustilan tilin luodaan sinulle automaattisesti käyttöönoton sijainti Jos sinulla ei ole vielä tallennustilan tilin kyseisessä sijainnissa. Näin ei ole tarpeen virtuaalikoneen levyjen tallennustilan tilin luominen seuraavia ohjeita noudattamalla. Tallennustilan tilin nimi perusteella virtuaalikoneen nimi. On lisätietoja [Azuren näennäiskoneiden ohjeissa](https://azure.microsoft.com/documentation/services/virtual-machines/) .

## <a name="storage-account-endpoints"></a>Tallennustilan tilin päätepisteet

Kaikki objektit, joiden avulla voit tallentaa Azuren tallennustilaan on yksilöllinen URL-osoite. Tallennustilan tilin nimi-lomakkeiden osoite alitoimialueen. Alitoimialueen ja toimialueen nimi, joka on kunkin palvelun-yhdistelmä lomakkeiden *päätepisteen* tallennustilan tilissäsi.

Esimerkiksi jos tallennustilan tilin nimi on *mystorageaccount*, valitse Oletus päätepisteet tallennustilan tilin ovat:

- BLOB-palvelu: http://*mystorageaccount*. blob.core.windows.net

- Taulukon palvelun: http://*mystorageaccount*. table.core.windows.net

- Palvelun jonon: http://*mystorageaccount*. queue.core.windows.net

- Tiedoston palvelun: http://*mystorageaccount*. file.core.windows.net

> [AZURE.NOTE] Blob-tallennustilan tilin paljastaa vain Blob-palvelupäätepiste.

URL-Osoitteen käyttäminen objektin tallennustilan tilin muodostanut lisäämällä objektin sijainti-tallennustilan tilin päätepisteelle. Esimerkiksi blob-osoite on ehkä tässä muodossa: http://*mystorageaccount*.blob.core.windows.net/*mycontainer*/*myblob*.

Voit myös määrittää tallennustilan-tilisi käyttää mukautettua toimialuenimeä. Perinteinen tallennustilan-tileissä on [Määritä mukautettu toimialue Blob Storage-päätepisteen nimi](storage-custom-domain-name.md) . Tätä ominaisuutta ei ole lisätty [Azure portal](https://portal.azure.com) vielä Resurssienhallinta tallennustilan tilit, mutta voit määrittää sen PowerShellin avulla. Lisätietoja on kohdassa [Set-AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607146.aspx) cmdlet-komento.  

## <a name="create-a-storage-account"></a>Tallennustilan tilin luominen

1. Kirjautuminen [Azure portal](https://portal.azure.com).

2. Valitse toiminto-valikosta **Uusi** -> **tietojen + tallennustilan** -> **tallennustilan tilin**.

3. Kirjoita tallennustilan tilin nimi. Katso lisätietoja siitä, miten tallennustilan tilin nimi käytetään osoite objektit Azuren tallennustilaan [tallennustilan tilin päätepisteet](#storage-account-endpoints) .

    > [AZURE.NOTE] Tallennustilan tilin nimi on oltava 3 – 24 merkkiä ja voivat sisältää numeroita ja pieniä kirjaimia vain.
    >  
    > Tallennustilan tilin nimen on oltava yksilöllinen Azure. Azure portaalin osoittavat, jos valitset tallennustilan tilin nimi on jo käytössä.

4. Määritä käyttöönotto-mallin, jota käytetään: **Resurssienhallinta** tai **perinteinen**. **Resurssien hallinta** on suositellut käyttöönottomalli. Lisätietoja on artikkelissa [tietoja resurssien hallinnan käyttöönotto- ja perinteinen käyttöönotto](../resource-manager-deployment-model.md).

    > [AZURE.NOTE] BLOB storage tilejä voi luoda vain resurssien hallinnan käyttöönotto-mallin avulla.

5. Tallennustilan tilin tyypin valitseminen: **yleisiä tarkoitus** tai **Blob-objektien tallennustilaan**. **Yleisiä tarkoitus** on oletusarvo.

    Jos **yleisiä tarkoitus** on valittuna, Määritä suorituskyvyn taso: **Vakio** - tai **Premium**. Oletusarvo on **Vakio**. Lisätietoja vakio- ja premium tallennustilan tilit on artikkelissa [Johdanto Microsoft Azuren tallennustilaan](storage-introduction.md) ja [Premium tallennustilan: tehokas tallennustila Azure virtuaalikoneen työmääriä](storage-premium-storage.md).

    Jos **Blob-objektien tallennustilaan** on valittuna, Määritä access-tason: **kuuman** tai **hienoja**. Oletusarvo on **kuumalla**. Katso [Azure-Blob-säiliö: jäähdytetään ja oikotie tasoa](storage-blob-storage-tiers.md) lisätietoja.

6. Replikoinnin vaihtoehto tallennustilan tilin: **LRS**, **GRS**, **RA GRS**tai **ZRS**. Oletusarvo on **RA GRS**. Lisätietoja Azure-tallennustilan Replikointiasetukset on artikkelissa [Azure-tallennustilan replikoinnin](storage-redundancy.md).

7. Valitse tilaus, johon haluat luoda uuden tallennustilan tilin.

8. Määritä uusi resurssiryhmä tai valitse aiemmin luotu resurssiryhmä. Lisätietoja resurssiryhmät on artikkelissa [Azure Resurssienhallinta yleiskatsaus](../azure-resource-manager/resource-group-overview.md).

9. Tallennustilan tilin maantieteellisen sijainnin valitseminen Saat lisätietoja siitä, mitkä palvelut ovat käytettävissä millä alueella [Azure alueet](https://azure.microsoft.com/regions/#services) .

10. Valitse **Luo** tallennustilan-tilin luominen.

## <a name="manage-your-storage-account"></a>Tallennustilan tilin hallinta

### <a name="change-your-account-configuration"></a>Muuta tiliasetuksia

Kun olet luonut tallennustilan tilin, voit muokata sen kokoonpano, esimerkiksi käytettävän tilin replikoinnin-asetuksen muuttaminen tai muuttaminen access-tason Blob storage-tiliin. [Azure portal](https://portal.azure.com)-tallennustilan tilin Siirry, **kaikki** asetukset ja valitse sitten **määritysten** tarkasteleminen ja/tai muuttaa tilin määrittäminen.

> [AZURE.NOTE] Tallennustilan tilin luominen valitsit suorituskyvyn tason mukaan jotkin Replikointiasetukset eivät välttämättä ole käytettävissä.

Replikoinnin-asetuksen muuttaminen vaikuttaa omaa hinnoittelua. Lisätietoja on artikkelissa [Azure tallennustilan hinnat](https://azure.microsoft.com/pricing/details/storage/) sivun.

Blob storage tilien access-tason muuttaminen voi syntyä omaa hinnoittelua muuttumisen lisäksi muuta kulut. Katso lisätietoja [Blob storage tilit - hinnat ja laskutus](storage-blob-storage-tiers.md#pricing-and-billing) .

### <a name="manage-your-storage-access-keys"></a>Hallitse tallennustilan pikanäppäimet

Kun luot tallennustilan tilin, Azure Luo kaksi 512-bittinen tallennustilan pikanäppäimet, joita käytetään todennusta varten, kun tallennustilan tilin käytetään. Antamalla kaksi tallennustilan pikanäppäinten Azure mahdollistaa näppäimet palvelukatkoksia tallennustilan-palvelussa tai käyttää kyseisen palvelun uudelleen.

> [AZURE.NOTE] On suositeltavaa välttää tallennustilan access avaimien jakaminen kenellekään. Salli tallennustilan resurssien käytön tuomatta access-näppäimiä ulos, voit käyttää *jaettua käyttöä allekirjoitus*. Jaetun access-allekirjoitus on tilisi resurssin käyttöoikeutta, voit määrittää aikavälin ja käyttöoikeuksilla, jotka määrität. Lisätietoja on kohdassa [Käyttämällä jaettu Access allekirjoitukset (SAS)](storage-dotnet-shared-access-signature-part-1.md) .

#### <a name="view-and-copy-storage-access-keys"></a>Näyttäminen ja kopioiminen tallennustilan pikanäppäimet

[Azure portal](https://portal.azure.com)Siirry tallennustilan tilin, **kaikki** asetukset ja valitse sitten **pikanäppäimet** voit tarkastella, kopioida ja luo tili pikanäppäimet. **Pikanäppäimet** -sivu sisältää myös valmiiksi yhteyden merkkijonot ensisijainen ja toissijainen avaimien, voit kopioida käyttämään sovellusten avulla.

#### <a name="regenerate-storage-access-keys"></a>Luo tallennustilan pikanäppäimet

On suositeltavaa, muuttaa pikanäppäimet tallennustilan tilin säännöllisesti, jos haluat suojata tallennustilan yhteydet. Kaksi pikanäppäinten määritetään niin, että voit säilyttää yhteyden tallennustilan tilin käyttämällä yhtä access-näppäintä samalla, kun access-näppäintä uudelleen.

> [AZURE.WARNING] Access-näppäimiä uudelleen voivat vaikuttaa Azure sekä omia sovelluksia, jotka ovat riippuvaisia tallennustilan tilin palvelut. Kaikkien asiakkaiden, jotka käyttävät pikanäppäin tallennustilan-tili on päivitettävä uusi avain.

**Media services** - Jos media-palveluja, jotka ovat riippuvaisia tallennustilan-tilisi on uudelleen synkronoit pikanäppäimet media-palvelun kanssa kun näppäimet uudelleen.

**Sovellusten** – Jos verkkosovellusten tai cloud Services-palvelut, jotka käyttävät tallennustilan tilin menetät yhteydet Jos uudelleen näppäimet, ellei avaimien kokoa.

**Tallennustilan hallinnasta** – Jos käytössäsi on [tallennustilan explorer sovellukset](storage-explorers.md)todennäköisesti haluat päivittää näiden sovellusten käyttämä tallennustila-näppäintä.

Näin prosessi, jossa kiertäminen tallennustilan pikanäppäimet:

1. Päivitä yhteysmerkkijonot sovelluksen koodissa tallennustilan tilin toissijainen access-avain.

2. Luo ensisijainen pikanäppäin tallennustilan tilissäsi. Valitse **Pikanäppäimet** -sivu **Uudelleen Avain1**ja valitse Vahvista valitsemalla **Kyllä** , johon haluat luoda uuden tunnuksen.

3. Päivitä yhteysmerkkijonot koodisi ensisijainen uusi avain.

4. Toissijainen pikanäppäin luo samalla tavalla.

## <a name="delete-a-storage-account"></a>Tallennustilan tilin poistaminen

Poista tallennustilan-tilisi, joita et enää käytä Siirry [Azure portal](https://portal.azure.com)tallennustilan-tili ja valitse **Poista**. Tallennustilan tilin poistaminen poistaa koko tilin, mukaan lukien kaikki tiedot-tilillä.

> [AZURE.WARNING] Ei ole mahdollista Palauta poistetut tallennustilan tilin tai hakea sisältöä, se säilytetään ennen poistamista. Muista haluat tallentaa, ennen kuin voit poistaa tilin jotakin varmuuskopiointi. Tämä myös pitää paikkansa resurssien tilille, kun poistat blob, taulukkoon, jonossa tai tiedoston, se poistetaan pysyvästi.

Voit poistaa tallennustilaa-tili, joka on liitetty Azure virtuaalikoneen Varmista ensin, että kaikki virtuaalikoneen levyjen on poistettu. Jos et ensin Poista virtuaalikoneen-levyjä, sitten kun yrität poistaa tilisi tallennustilan näet virhesanoman, joka muistuttaa:

    Failed to delete storage account <vm-storage-account-name>. Unable to delete storage account <vm-storage-account-name>: 'Storage account <vm-storage-account-name> has some active image(s) and/or disk(s). Ensure these image(s) and/or disk(s) are removed before deleting this storage account.'.

Jos tallennustilan tilin käyttää perinteinen käyttöönotto-malli, voit poistaa virtuaalikoneen levyn [Azure portaalissa](https://manage.windowsazure.com)seuraavasti:

1. Siirry [perinteiseen Azure portal](https://manage.windowsazure.com).
2. Siirry näennäiskoneiden-välilehti.
3. Valitse levyjen-välilehti.
4. Valitse tiedot-levy ja valitse sitten Poista levy.
5. Jos haluat poistaa levyn kuvia, siirry kuvat-välilehti ja poista kuvia, jotka on tallennettu tilin.

Katso lisätietoja [Azure virtuaalikoneen ohjeissa](http://azure.microsoft.com/documentation/services/virtual-machines/).

## <a name="next-steps"></a>Seuraavat vaiheet

- [Azure-Blob-säiliö: Jäähdytetään ja kuuman tasoa](storage-blob-storage-tiers.md)
- [Azure tallennustilan sallittuja](storage-redundancy.md)
- [Määritä Azure tallennustilan yhteyden merkkijonoja](storage-configure-connection-string.md)
- [Siirrä tiedot ja AzCopy komentorivivalitsimet-apuohjelma](storage-use-azcopy.md)
- Siirry [Azure tallennustilan tiimin blogia](http://blogs.msdn.com/b/windowsazurestorage/).
