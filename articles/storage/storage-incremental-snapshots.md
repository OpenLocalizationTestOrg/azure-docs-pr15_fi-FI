<properties
    pageTitle="Käytä vaiheittainen tilannevedoksia varmuuskopiointia ja palauttamista Azuren näennäiskoneiden | Microsoft Azure"
    description="Voit luoda mukautetun ratkaisun varmuuskopiointia ja palauttamista Azure virtuaalikoneen levyjen vaiheittainen tilannevedosten käyttäminen."
    services="storage"
    documentationCenter="na"
    authors="aungoo-msft"
    manager="tadb"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="aungoo"/>


# <a name="back-up-azure-virtual-machine-disks-with-incremental-snapshots"></a>Azure virtuaalikoneen levyjen vaiheittainen tilannevedoksia ja varmuuskopiointi

## <a name="overview"></a>Yleiskatsaus

Azure-tallennustilan avulla käyttäjät voivat ottaa tilannevedoksen BLOB-objektit. Tilannevedoksia siepata senhetkinen aika-blob-tilaan. Tässä artikkelissa on kuvataan, miten voit ylläpitää varmuuskopioiden virtuaalikoneen levyjen tilannevedosten käyttäminen tilanne. Tätä menetelmää voit käyttää, kun Käytä Azure varmuuskopiointi ja palauttaminen-palvelu ja haluat luoda mukautetun Varmuuskopioinnin toimintamallin virtuaalikoneen levyille.

Azure virtuaalikoneen levyjen tallennetaan sivun BLOB Azuren tallennustilaan. Olemme keskustelet Varmuuskopioinnin toimintamallin virtuaalikoneen levyjen tämän artikkelin tietoja, koska olemme voidaan viittaavat tilannevedoksia sivun BLOB kontekstissa. Saat lisätietoja tilannevedoksia viitata [Blob tilannevedoksen luominen](https://msdn.microsoft.com/library/azure/hh488361.aspx).

## <a name="what-is-a-snapshot"></a>Mikä on?

Blob-tilannevedoksen on blob, johon kirjataan vaiheessa ajassa vain luku-versio. Kun tilannevedoksen on luotu, se voi lukea, kopioitu, tai poistettu, mutta ei muokata. Tilannevedoksia lisäämistapaa varmuuskopioida blob sellaisena kuin se näkyy hetkellä. Ennen kuin muut versio 2015-04-05 oli voi kopioida koko tilannevedoksia. Muilta versio 2015 07-08 tai uudempi, voit myös kopioida vaiheittainen tilannevedoksia.

## <a name="full-snapshot-copy"></a>Kopioi koko tilannevedos

Tilannevedoksia voidaan kopioida toisen tallennustilan tilin nimellä Blob-objektien pitämään perus Blob-objektien varmuuskopiot. Voit myös kopioida tilannevedoksen perus kuten aiemman version palauttaminen blob eli blob päälle. Kun tilannevedoksen kopioidaan tallennustilan-tililtä toiselle, se ovat samassa paikassa kuin perussivun Blob-objektien. Tämän vuoksi kopioiminen koko tilannevedoksia, tallennustilan-tililtä toiselle on hidasta ja myös käyttää paljon tilaa kohde-tallennustilan tilin.

>[AZURE.NOTE] Jos peruste Blob-objektien kopioiminen toiseen kohteeseen, blob tilannevedosten ei kopioida mukana. Vastaavasti Jos korvaat perus Blob-objektien kopiolla, perus Blob-objektien liittyvät tilannevedoksia ei vaikuta ja pysyvät ennallaan perus blob nimellä.

### <a name="back-up-disks-using-snapshots"></a>Varmuuskopioi levyjen tilannevedosten käyttäminen

Kuin virtuaalikoneen levyjen Varmuuskopioinnin toimintamallin ottaa säännöllisiä tilannevedoksen levyn tai sivun Blob-objektien ja kopioi ne toiseen tallennustilan tilin työkaluilla, kuten [Kopioi Blob](https://msdn.microsoft.com/library/azure/dd894037.aspx) -toimintoa tai [AzCopy](storage-use-azcopy.md). Voit kopioida tilannevedoksen kohde sivun Blob-objektien eri nimellä. Tuloksena oleva kohde sivun Blob-objektien on kirjoitettava sivun Blob-objektien ja tilannevedoksen luominen. Tämän artikkelin on kerrotaan, miten toimia, varmuuskopioiden virtuaalikoneen levyjen tilannevedosten käyttäminen.

### <a name="restore-disks-using-snapshots"></a>Palauttaa levyjen tilannevedosten käyttäminen

Kun on aika, voit palauttaa levyn varmuuskopion tilannevedoksia tallentaa vakaata aiemmasta versiosta, voit kopioida tilannevedoksen perussivun Blob-objektien päälle. Kun tilannevedoksen on ylemmän tason perus-sivulle blob-tilannevedoksen, pysyy, mutta sen lähdesijainnissa korvautuu kopio, joka voidaan sekä lukea ja kirjoittaa. Tämän artikkelin on kuvataan levytilaa aiemman version palauttaminen sen tilannevedoksen vaiheet.

### <a name="implementing-full-snapshot-copy"></a>Kopioi koko tilannevedoksen soveltamisesta

Voit ottaa käyttöön koko tilannevedoksen kopio toimimalla seuraavasti

-   Ota perus Blob-objektien [Tilannevedoksen Blob](https://msdn.microsoft.com/library/azure/ee691971.aspx) -toiminnon tilannevedos ensin.
-   Kopioi sitten tilannevedoksen kohde tallennustilan tilin [Kopioi Blob-objektien](https://msdn.microsoft.com/library/azure/dd894037.aspx)käyttäminen.
-   Toista tämä pitämään perus Blob-objektien varmuuskopiot.

## <a name="incremental-snapshot-copy"></a>Vaiheittainen tilannevedoksen kopioiminen

Uusi [GetPageRanges](https://msdn.microsoft.com/library/azure/ee691973.aspx) API-toiminto on paljon paremman tavan sivun BLOB tai levyjä tilannevedosten varmuuskopioida. Ohjelmointirajapinnan palauttaa muutosluettelo perus Blob-objektien ja tilannevedosten välillä. Tämä pienentää tallennustilaa varmuuskopio-tilissä. Ohjelmointirajapinnan tukee sivun BLOB Premium tallennustilan sekä vakio tallennustilan. Tämä Ohjelmointirajapinnan käyttäminen voit nyt luoda nopeammin ja tehokkaan varmuuskopion ratkaisuja Azure VMs varten. Tämä on käytettävissä REST-versiolla 2015 07-08 tai uudempi versio.

Vaiheittainen tilannevedoksen kopioi avulla voit kopioida, tallennustilan-tililtä toiselle välistä eroa,

-   Perus-Blob-objektien ja sen tilannevedoksen tai
-   Minkä tahansa kahden tilannevedosten perus Blob-objektien

Jos seuraavat ehdot täyttyvät,

- Blob on luotu tammi – 1-2016 tai uudempi.
- Blob ei korvattu [PutPage](https://msdn.microsoft.com/library/azure/ee691975.aspx) tai [Kopioi Blob-objektien](https://msdn.microsoft.com/library/azure/dd894037.aspx) kaksi tilannevedoksen välillä.


**Huomautus**: Tämä ominaisuus on käytettävissä Premium ja vakio sivun Azure-BLOB.

Jos sinulla on mukautettu Varmuuskopioinnin toimintamallin, joka käyttää tilannevedoksia, kopioit tilannevedosten tallennustilan-tililtä toiselle voi olla hyvin hidasta ja tallennustilaa on paljon siinä käytetään. Sen sijaan, että kopioit koko tilannevedoksen varmuuskopion tallennustilan tilin, voit kirjoittaa peräkkäisten tilannevedoksia varmuuskopion sivun Blob-objektien välisen eron. Tällä tavalla kopio ja tallennustilaa varmuuskopioiden aika pienenee huomattavasti.

### <a name="implementing-incremental-snapshot-copy"></a>Vaiheittainen tilannevedoksen kopioi soveltamisesta

Voit ottaa käyttöön vaiheittainen tilannevedoksen kopioi toimimalla seuraavasti

-   Ota perus Blob-objektien käyttäminen [Tilannevedoksen Blob-objektien](https://msdn.microsoft.com/library/azure/ee691971.aspx)tilannevedos.
-   Kopioi kohteen varmuuskopioinnin tallennustilan tili käyttämällä [Kopioi Blob-objektien](https://msdn.microsoft.com/library/azure/dd894037.aspx)tilannevedoksen. Tämä on varmuuskopion sivun Blob-objektien. Ota tilannevedos tämän varmuuskopion sivun Blob-objektien ja Tallenna varmuuskopio tili.
-   Ota perus Blob-objektien tilannevedoksen Blob-objektien käyttäminen toisessa tilannevedoksen.
-   Pyydä perus Blob-objektien käyttäminen [GetPageRanges](https://msdn.microsoft.com/library/azure/ee691973.aspx)ensimmäisen ja toisen tilannevedosten välinen erotus. Uuden parametrin **prevsnapshot** avulla voit määrittää haluat lähetettävän ero tilannevedoksen DateTime-arvo. Kun tämä parametri ei sisällä tietoja, REST-vastauksen lisätä sivuja, jotka ovat muuttuneet kohde tilannevedoksen ja Poista sivut mukaan lukien edellisen tilannevedoksen välillä.
-   Nämä muutokset varmuuskopion sivun Blob-objektien [PutPage](https://msdn.microsoft.com/library/azure/ee691975.aspx) avulla.
-   Lopuksi Ota tilannevedos varmuuskopion sivun Blob-objektien ja tallentaa sen varmuuskopion tallennustilan tilin.

Seuraavassa osassa on kuvataan tarkemmin siitä, miten voit ylläpitää varmuuskopioiden levyjen vaiheittainen tilannevedoksen kopioimalla

## <a name="scenario"></a>Skenaario

Tässä osassa on kuvataan tilanne, jossa liittyy mukautetun Varmuuskopioinnin toimintamallin virtuaalikoneen levyjen tilannevedosten käyttäminen.

Harkitse DS-sarjan Azure-AM liitetty premium tallennustilan P30 DVD-levyllä kanssa. Kutsutaan *mypremiumdisk* P30 levy on tallennettu kutsutaan *mypremiumaccount*premium tallennustilan tilillä. Vakio tallennustilan-tiliä, nimeltä *mybackupstdaccount* käytetään *mypremiumdisk*varmuuskopion tallentamista varten. Haluamme pitämään tilannevedoksen *mypremiumdisk* 12 tunnin välein.

Saat lisätietoja tallennustilan tilin ja levyille luominen viitata [tietoja Azure-tallennustilan tilit](storage-create-storage-account.md).

Saat lisätietoja Azure VMs varmuuskopioiminen viitata [suunnitteleminen Azure AM varmuuskopiot](../backup/backup-azure-vms-introduction.md).

## <a name="steps-to-maintain-backups-of-a-disk-using-incremental-snapshots"></a>Ohjeita pitämään varmuuskopioita DVD-levyllä vaiheittainen tilannevedosten käyttäminen

Alla kuvatut vaiheet otettava tilannevedosten *mypremiumdisk* ja ylläpitää *mybackupstdaccount*varmuuskopioiden. Varmuuskopiointi on nimeltään *mybackupstdpageblob*vakiosivulla Blob-objektien. Varmuuskopion sivun Blob-objektien aina vaikuttavat *mypremiumdisk*viimeisen tilannevedoksena samaan tilaan.

1.  Luo varmuuskopio sivun Blob-objektien premium-tallennustilan levyn. Voit tehdä tämän Ota tilannevedos *mypremiumdisk* kutsutaan *mypremiumdisk_ss1*.
2.  Kopioi tämä tilannevedos mybackupstdaccount kutsutaan *mybackupstdpageblob*sivun Blob-objektien nimellä.
3.  Ota tilannevedos *mybackupstdpageblob* kutsutaan *mybackupstdpageblob_ss1*, käyttämällä [Tilannevedoksen Blob-objektien](https://msdn.microsoft.com/library/azure/ee691971.aspx) ja tallentaa sen *mybackupstdaccount*.
4.  Aikana varmuuskopio-ikkunassa Luo toinen *mypremiumdisk*tilannevedoksen, sano *mypremiumdisk_ss2*ja tallenna se *mypremiumaccount*.
5.  Hae vaiheittainen muutoksia kaksi tilannevedoksen *mypremiumdisk_ss2* ja *mypremiumdisk_ss1*välillä, käyttämällä [GetPageRanges](https://msdn.microsoft.com/library/azure/ee691973.aspx) *mypremiumdisk_ss2* **prevsnapshot** -parametrin arvoksi *mypremiumdisk_ss1*aikaleimaa. Kirjoittaa vaiheittainen muutokset varmuuskopion sivun Blob-objektien *mybackupstdpageblob* - *mybackupstdaccount*. Jos määritettynä on poistettu alueiden vaiheittainen muutokset, ne on poistetaan varmuuskopion sivun Blob-objektien. [PutPage](https://msdn.microsoft.com/library/azure/ee691975.aspx) avulla voit kirjoittaa vaiheittainen muutokset varmuuskopion sivun Blob-objektien.
6.  Ota tilannevedos varmuuskopion sivun Blob-objektien *mybackupstdpageblob*, kutsutaan *mybackupstdpageblob_ss2*. Poista edellinen tilannevedoksen *mypremiumdisk_ss1* premium-tallennustilan tilin.
7.  Toista vaiheet 4 – 6 jokaisen varmuuskopiointi-ikkuna. Näin voit ylläpitää *mypremiumdisk* vakio tallennustilan tilillä varmuuskopiot.

![Varmuuskopioi levyn vaiheittainen tilannevedosten käyttäminen](./media/storage-incremental-snapshots/storage-incremental-snapshots-1.png)

## <a name="steps-to-restore-a-disk-from-snapshots"></a>Palauttaa DVD-levyllä tilannevedoksia vaiheet

Alla kuvatut vaiheet palauttaa premium levyn *mypremiumdisk* aiemmissa tilannevedos-varmuuskopion tallennustilan tilin *mybackupstdaccount*.

1.  Määritä kohtaan samanaikaisesti haluat palauttaa premium-levylle. Oletetaan, että tämä on tilannevedoksen *mybackupstdpageblob_ss2*, joka on tallennettu varmuuskopion tallennustilan tilin *mybackupstdaccount*.
2.  Voit viedä tilannevedoksen *mybackupstdpageblob_ss2* mybackupstdaccount, uusi varmuuskopion perussivun Blob-objektien *mybackupstdpageblobrestored*muodossa.
3.  Ota tilannevedos tämän palauttaminen varmuuskopiosta sivun blob-kutsutaan *mybackupstdpageblobrestored_ss1*.
4.  Kopioi palautettu sivun Blob-objektien *mybackupstdpageblobrestored* *mybackupstdaccount* *mypremiumaccount* uusi premium-levyn *mypremiumdiskrestored*nimellä.
5.  Ota tilannevedos *mypremiumdiskrestored*, kutsutaan *mypremiumdiskrestored_ss1* , jolla varmistetaan, että uudet vaiheittainen varmuuskopiot.
6.  Pisteen Hakemistopalvelun sarjan AM palautettu levylle *mypremiumdiskrestored* ja irrottaminen vanha *mypremiumdisk* kohteesta AM.
7.  Aloita varmuuskopiointi palautettu levyn *mypremiumdiskrestored*, käytä *mybackupstdpageblobrestored* varmuuskopion sivun Blob-objektien edellisessä osassa kuvattua.

![Palauttaa levyn tilannevedokset](./media/storage-incremental-snapshots/storage-incremental-snapshots-2.png)

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja blob tilannevedosten luominen ja suunnittelun AM varmuuskopion infrastruktuurin käyttämällä alla olevia linkkejä.

- [Blob tilannevedoksen luominen](https://msdn.microsoft.com/library/azure/hh488361.aspx)
- [AM-varmuuskopion infrastruktuurin suunnitteleminen](../backup/backup-azure-vms-introduction.md)
