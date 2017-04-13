<properties
    pageTitle="DocumentDB yleinen tietokannan replikoiminen | Microsoft Azure"
    description="Lue, miten voit hallita yleistä replikoinnin DocumentDB-tilisi kautta Azure portaaliin."
    services="documentdb"
    keywords="Yleinen tietokanta, replikointi"
    documentationCenter=""
    authors="mimig1"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="documentdb"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="mimig"/>

# <a name="how-to-perform-documentdb-global-database-replication-using-the-azure-portal"></a>Tietoja DocumentDB yleinen tietokannan replikoiminen Azure-portaalissa

Opettele käyttämään Azure portaalin replikointia tietojen yleinen Azure DocumentDB tietojen saatavuuden alueille.

Lisätietoja siitä, miten yleinen tietokannan replikoiminen tapahtuu DocumentDB on artikkelissa [välit tietojen yleisesti DocumentDB](documentdb-distribute-data-globally.md). Katso tietoja suorittamiseen yleinen tietokannan replikoiminen ohjelmallisesti [kehitysmaiden monille DocumentDB tilien kanssa](documentdb-developing-with-multiple-regions.md).

> [AZURE.NOTE] Yleinen jakautumisen DocumentDB tietokantoja on yleisesti saatavilla ja automaattisesti käyttöön juuri luomasi DocumentDB kaikki tilit. Yritämme yleinen käyttöönottaminen kaikkien asiakkaiden, mutta sillä välin voit halutessasi Yleinen osoitteisto käytössä tililläsi, [Ota yhteyttä tukeen](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) ja Microsoft ota se puolestasi nyt.

## <a id="addregion"></a>Lisää yleinen tietokanta-alueet

DocumentDB on käytettävissä useimmissa [Azure alueiden] [azureregions]. Kun olet valinnut tilin tietokannan yhdenmukaisuuden oletustaso, liittää yhteen tai useampaan (valintasi oletusarvon yhdenmukaisuuden taso ja Yleinen-jakauman tarpeita) mukaan.

1. [Azure portal](https://portal.azure.com/)-kohdassa Jumpbar Valitse **DocumentDB tilit**.
2. Valitse tietokanta-tili, jota haluat muokata **DocumentDB tili** -sivu.
3. Valitse tili-sivu-valikosta **tietoja replikoida yleisesti** .
4. Valitse **replikoida yleisesti tiedot** -sivu alueiden lisääminen tai poistaminen ja valitse sitten **Tallenna**. Ei lisäämisen alueiden kustannus on artikkelissa [hinnat sivun](https://azure.microsoft.com/pricing/details/documentdb/) tai [välit tietoja yleisesti DocumentDB](documentdb-distribute-data-globally.md) -artikkelista lisätietoja.

    ![Valitse Lisää tai poista ne määrityksen alueita][1]

### <a name="selecting-global-database-regions"></a>Yleinen tietokanta alueiden valitseminen

Kahden tai useamman alueen määritettäessä on suositeltavaa alueiden valitaan kuvattu alueen parit perusteella [liiketoiminnan jatkuvuuden ja tietojen palauttaminen (BCDR): Azure Parittainen alueiden]  [ bcdr] artikkelin.

Tarkemmin sanottuna määritettäessä alueille Varmista, että sama määrä (+/-1 parittomat ja parilliset) alueiden valitseminen kunkin pisteparin alue-sarakkeet. Esimerkiksi jos haluat ottaa käyttöön US neljä aluetta, valitset US kummallakin alueella vasemmassa sarakkeessa ja kaksi oikealta lähtien. Näin on, seuraavat olisi sopivat: Länsi US, Yhdysvaltojen Itä, Pohjois keskitetyn US ja Etelä keskitetyn US.

Nämä ohjeet on tärkeää kun tietojen palauttaminen skenaariot määritetään vain kummallakin alueella. Enemmän kuin kaksi alueiden ohjeiden tämä on hyvä, mutta ei kriittinen, kunhan joitakin valittuihin alueisiin noudattaa tätä laiteparin.

<!---
## <a id="selectwriteregion"></a>Select the write region

While all regions associated with your DocumentDB database account can serve reads (both, single item as well as multi-item paginated reads) and queries, only one region can actively receive the write (insert, upsert, replace, delete) requests. To set the active write region, do the following  


1. In the **DocumentDB Account** blade, select the database account to modify.
2. In the account blade, if the **All Settings** blade is not already opened, click **All Settings**.
3. In the **All Settings** blade, click **Write Region Priority**.
    ![Change the write region under DocumentDB Account > Settings > Add/Remove Regions][2]
4. Click and drag regions to order the list of regions. The first region in the list of regions is the active write region.
    ![Change the write region by reordering the region list under DocumentDB Account > Settings > Change Write Regions][3]
-->

## <a id="next"></a>Seuraavat vaiheet

Opi hallitsemaan yleisesti replikoitua tilisi yhtenäisyyden lukemalla [yhdenmukaisuuden tasojen DocumentDB](documentdb-consistency-levels.md).

Lisätietoja siitä, miten yleinen tietokannan replikoiminen toimii DocumentDB on artikkelissa [välit tietojen yleisesti DocumentDB](documentdb-distribute-data-globally.md). Tietoja replikoiminen ohjelmallisesti tietoja useista alueista on artikkelissa [kehitysmaiden monille DocumentDB tilien kanssa](documentdb-developing-with-multiple-regions.md).

<!--Image references-->
[1]: ./media/documentdb-portal-global-replication/documentdb-add-region.png
[2]: ./media/documentdb-portal-global-replication/documentdb_change_write_region-1.png
[3]: ./media/documentdb-portal-global-replication/documentdb_change_write_region-2.png

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[bcdr]: https://azure.microsoft.com/documentation/articles/best-practices-availability-paired-regions/
[consistency]: https://azure.microsoft.com/documentation/articles/documentdb-consistency-levels/
[azureregions]: https://azure.microsoft.com/en-us/regions/#services
[offers]: https://azure.microsoft.com/en-us/pricing/details/documentdb/
