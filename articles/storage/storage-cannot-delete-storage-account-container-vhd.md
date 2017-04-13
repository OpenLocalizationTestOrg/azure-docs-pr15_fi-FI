<properties
    pageTitle="Vianmääritys Azure tallennustilan tilit, säilöjen tai näennäiskiintolevyjen poistaminen perinteinen käyttöönoton | Microsoft Azure"
    description="Vianmääritys Azure tallennustilan tilit, säilöjen tai näennäiskiintolevyjen poistaminen perinteinen käyttöönotto"
    services="storage"
    documentationCenter=""
    authors="genlin"
    manager="felixwu"
    editor="tysonn"
    tags="storage"/>

<tags
    ms.service="storage"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="genli"/>

# <a name="troubleshoot-deleting-azure-storage-accounts-containers-or-vhds-in-a-classic-deployment"></a>Vianmääritys Azure tallennustilan tilit, säilöjen tai näennäiskiintolevyjen poistaminen perinteinen käyttöönotto

[AZURE.INCLUDE [storage-selector-cannot-delete-storage-account-container-vhd](../../includes/storage-selector-cannot-delete-storage-account-container-vhd.md)]

Näyttöön voi tulla virheitä, kun yrität poistaa Azure-tallennustilan tilin, säilön tai Näennäiskiintolevyn [Azure portal](https://portal.azure.com/) tai [Azure perinteinen portal](https://manage.windowsazure.com/). Ongelmat voivat johtua seuraavissa tilanteissa:

-   Kun poistat AM, levyn ja Näennäiskiintolevyn ei poisteta automaattisesti. Joka voi olla-tallennustilan tilin poistaminen epäonnistumisen syystä. Microsoft ei poista levy, jolloin levyn avulla voit ottaa käyttöön toisen AM.

-   Varaus DVD-levyllä tai blob, joka liittyy levy on edelleen.

Jos tässä artikkelissa ei käsitellä Azure ongelmaa, käy Azure [MSDN](https://azure.microsoft.com/support/forums/)ja Pinon ylivuoto. Voit julkaista ongelmaa nämä keskustelupalstalle tai @AzureSupport Twitter. Voit myös jättää Azure tukipyyntö valitsemalla **tuen saaminen** [Azure tuki](https://azure.microsoft.com/support/options/) -sivustossa.

## <a name="symptoms"></a>Ongelmia

Seuraavassa osassa on luettelo yleisiä virheitä, näyttöön voi tulla, kun yrität poistaa Azure-tallennustilan tilit, säilöjen tai näennäiskiintolevyjen.

### <a name="scenario-1-unable-to-delete-a-storage-account"></a>Tapaus 1: Tallennustilan tilin poistaminen ei onnistu

Kun siirryt tallennustilan tilin [Azure portal](https://portal.azure.com/) tai [Azure perinteinen portaalin](https://manage.windowsazure.com/) ja valitse **Poista**, näet ehkä seuraavan virhesanoman:

*Tallennustilan tilin StorageAccountName on AM kuvia. Varmista ennen poistamista tallennustilan tilin AM kuvien on siirretty.*

Myös saatat saada seuraavan virheilmoituksen:

**Valitse Azure-portaalissa**:

*Tallennustilan tilin < AM-tallennustilan-tili-name > poisto epäonnistui. Ei voi poistaa tallennustilan tilin < AM-tallennustilan-tili-name >: "tallennustilan tilin < AM-tallennustilan-tili-name > on joitakin aktiivinen kuvat ja/tai levyt. Varmista ennen poistamista tallennustilan tilin poistetaan nämä kuvat ja/tai levyt. ".*

**Valitse Azure perinteinen portaalissa**:

*Tallennustilan tilin < AM-tallennustilan-tili-name > on joitakin aktiivinen kuvat ja/tai levyt, kuten xxxxxxxxx-xxxxxxxxx-O-209490240936090599. Varmista nämä kuvat ja/tai levyt poistetaan ennen poistamista tallennustilan tähän tiliin.*

Tai

**Valitse Azure-portaalissa**:

*Tallennustilan tilin < AM-tallennustilan-tili-name > on 1 riittävästä on aktiivinen kuvan ja/tai levy palvelutiedot. Varmista näiden palvelutiedot poistetaan kuva tietovarasto ennen poistamista tallennustilan tilin*.

**Valitse Azure perinteinen portaalissa**:

*Lähetä epäonnistui tallennustilan tilin < AM-tallennustilan-tili-name > on 1 riittävästä on aktiivinen kuvan ja/tai levy palvelutiedot. Varmista näiden palvelutiedot poistetaan kuva tietovarasto ennen poistamista tallennustilan tähän tiliin. Kun yrität poistaa tallennustilan tilin ja liittyy yhä aktiivinen levyjä, näyttöön tulee viesti, jossa on aktiivinen levyjä, jotka haluat poistaa*.

### <a name="scenario-2-unable-to-delete-a-container"></a>Tapaus 2: Säilön poistaminen ei onnistu

Kun yrität poistaa tallennustilan säilö, näkyviin voi tulla seuraava virhe:

*Ei voi poistaa tallennustilan säiliön <container name>. Virhe: "-säilöä ei tällä hetkellä varaus ja varauksen ID-tunnusta ei määritetty pyynnön*.

### <a name="scenario-3-unable-to-delete-a-vhd"></a>Tapaus 3: Näennäiskiintolevyn poistaminen ei onnistu

Kun poistaminen AM ja yritä poistaa liittyvät näennäiskiintolevyjen BLOB-objektit, näyttöön voi tulla seuraava sanoma:

*Blob-objektien poistaminen epäonnistui "polku/XXXXXX-XXXXXX-os-1447379084699.vhd'. Virhe: "-blob ei tällä hetkellä varaus ja varauksen ID-tunnusta ei määritetty pyynnön.*

## <a name="solution"></a>Ratkaisu
Voit korjata Yleisimmät ongelmat hakua seuraavalla tavalla:

### <a name="step-1-delete-any-os-disks-that-are-preventing-deletion-of-the-storage-account-container-or-vhd"></a>Vaihe 1: Poista kaikki OS-levyjä, jotka estävät poiston tallennustilan tilin, säilön tai Näennäiskiintolevyn

1. Siirry [Azure perinteinen portal](https://manage.windowsazure.com/).
2. Valitse **VIRTUAALIKONEEN** > **LEVYJÄ**.

    ![Kuva levyille näennäiskoneiden Azure perinteinen portaalissa.](./media/storage-cannot-delete-storage-account-container-vhd/VMUI.png)

3. Etsi levyjä, jotka liittyvät tallennustilan tilin, säilön tai Näennäiskiintolevyn, jonka haluat poistaa. Kun olet kuittaamassa levyn sijainti, löydät liittyvät tallennustilan tilin, säilön tai Näennäiskiintolevyn.

    ![Kuva, jossa näkyy sijaintitiedot levyjen Azure perinteinen-portaalissa](./media/storage-cannot-delete-storage-account-container-vhd/DiskLocation.png)

4. Varmista, että mitään AM ei näy levyjä **liitetty** -kenttään ja poistamalla levyjä.

    > [AZURE.NOTE] Jos levy on liitetty AM, voit voi poistaa sen. Levyjen on irrotettu poistetut AM asynkronisesti. Saattaa kestää muutaman minuutin kuluttua AM tälle kentälle selvittää poistamisen jälkeen.

### <a name="step-2-delete-any-vm-images-that-are-preventing-deletion-of-the-storage-account-or-container"></a>Vaihe 2: Poista AM kuvia, jotka estävät poiston tallennustilan asiakas- tai säilö

1. Siirry [Azure perinteinen portal](https://manage.windowsazure.com/).
2. Valitse **VIRTUAALIKONEEN** > **kuvat**, ja poista sitten kuvat, jotka liittyvät tallennustilan tilin, säilön tai Näennäiskiintolevyn.

    Tämän jälkeen yrittää poistaa tallennustilan tilin, säilön tai Näennäiskiintolevyn uudelleen.

> [AZURE.WARNING] Muista varmuuskopioida mitään haluat tallentaa, ennen kuin voit poistaa tilin. Ei ole mahdollista Palauta poistetut tallennustilan tilin tai hakea sisältöä, se säilytetään ennen poistamista. Tämä myös pitää paikkansa resurssien tilin: Kun poistat Näennäiskiintolevyn, blob, taulukkoon, jonossa tai tiedoston, se poistetaan pysyvästi. Varmista, että resurssi ei ole käytössä.

## <a name="about-the-stopped-deallocated-status"></a>Pysäytetty (Varaus poistettu)-tilasta

VMs, jotka on luotu perinteinen käyttöönoton mallin ja, joka säilyttää on [Azure portal](https://portal.azure.com/) tai [Azure perinteinen portal](https://manage.windowsazure.com/) **pysäytetty (Varaus poistettu)** -tila.

**Azure perinteinen portaalissa**:

![Pysäytetty (Deallocated)-tila, VMs Azure-portaalissa.](./media/storage-cannot-delete-storage-account-container-vhd/moreinfo2.png)


**Azure portaalissa**:

![Pysäytetty (Varaus poistettu) tila VMs Azure perinteinen portaalissa.](./media/storage-cannot-delete-storage-account-container-vhd/moreinfo1.png)

"Pysäytetty (Varaus poistettu)-tila julkaisee tietokoneen resursseja, kuten suorittimen, muistin ja verkkoon. Levyjä, edelleen säilytetään kuitenkin niin, että voit nopeasti luoda uudelleen AM tarvittaessa. Näiden levyjen luodaan näennäiskiintolevyjen, jossa Azure tallennustilan palautettua päälle. Tallennustilan tilillä nämä näennäiskiintolevyjen ja levyjen ole vuokrasopimukset näiden näennäiskiintolevyjen.

## <a name="next-steps"></a>Seuraavat vaiheet

- [Tallennustilan tilin poistaminen](storage-create-storage-account.md#delete-a-storage-account)
- [Voit katkaista Blob-objektien tallennustilaan lukittu varauksen Microsoft Azure (PowerShell)](https://gallery.technet.microsoft.com/scriptcenter/How-to-break-the-locked-c2cd6492)
