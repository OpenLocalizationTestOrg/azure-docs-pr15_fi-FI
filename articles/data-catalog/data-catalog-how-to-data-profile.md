<properties
    pageTitle="Kuinka profiilin tietolähteisiin"
    description="Toimintaohjeet artikkelissa korostaminen voit sisällyttää taulukon ja sarakkeiden tason käyttäjäprofiileissa rekisteröiminen tietolähteiden Azure tietoluettelon ja Opi käyttämään käyttäjäprofiileissa ymmärtää tietolähteet."
    services="data-catalog"
    documentationCenter=""
    authors="spelluru"
    manager="NA"
    editor=""
    tags=""/>
<tags
    ms.service="data-catalog"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="data-catalog"
    ms.date="09/13/2016"
    ms.author="spelluru"/>

# <a name="data-profile-data-sources"></a>Profiilin tietolähteet

## <a name="introduction"></a>Johdanto

**Microsoft Azure tietoluettelon** on täysin hallitun pilvipalvelussa, joka on järjestelmän rekisteröinnin ja järjestelmä on enterprise tietolähteiden etsiminen. Toisin sanoen **Azure tietoluettelon** 's all about toimintamallia löytää, toimintaperiaatteet ja käyttäminen tietolähteiden ja auttaa organisaatioita tulee niiden annetuista tiedoista. Kun tietolähde on rekisteröitynyt **Azure tietoluettelon**, sen metatietoja kopioidaan ja indeksoitu-palvelu, mutta Tarinan ei lopeta siellä.

**Azure tietoluettelon** **Tietojen profilointi** -toiminnon tutkii tietolähteiden luettelon tiedot ja kerää Tilasto- ja tiedot tietoja. On helppoa sisällytettävien tietojen varat profiili. Kun olet rekisteröinyt tietojen resurssi, valitse **Sisällytä tietoja profiilin** tietojen lähteen rekisteröinti-työkalu.

## <a name="what-is-data-profiling"></a>Mitä tietoja profilointi

Tietoja profilointi tutkii on rekisteröity tietolähteen tiedot ja kerää Tilasto- ja tiedot tietoja. Tietojen lähteen etsiminen aikana tilastotiedot auttavat selvittämään sopivuus tiedot niiden business-ongelman ratkaisemiseen.

<!-- In [How to discover data sources](data-catalog-how-to-discover.md), you learn about **Azure Data Catalog's** extensive search capabilities including searching for data assets that have a profile. See [How to include a data profile when registering a data source](#howto). -->

Seuraaviin tietolähteisiin tukevat tietojen profilointi:

- SQL Server (kuten SQL Azure-DB ja Azure SQL-tietovarasto) taulukot ja näkymät
- Oracle-taulukot ja näkymät
- Teradata-taulukot ja näkymät
- Rakenne-taulukot

Mukaan lukien tiedot-profiileista, kun tietojen varat rekisteröiminen auttaa käyttäjiä kysymyksiin tietolähteistä, mukaan lukien:

-   Se voidaan käyttää business-ongelman ratkaisemiseen?
-   Tietoja mukainen tietyn standardien tai kuviot
-   Mitä tietolähteen poikkeamia?
-   Mitä mahdollista haasteiden tietojen integroiminen sovelluksessa?

> [AZURE.NOTE] Voit lisätä resurssi kuvataan, miten tietoja voi integroida sovellukseen myös ohjeissa. Katso, [kuinka asiakirjaan tietolähteet](data-catalog-how-to-documentation.md).


<a name="howto"/>
## <a name="how-to-include-a-data-profile-when-registering-a-data-source"></a>Profiilin tietojen lisäämisestä, kun tietolähteen rekisteröiminen

On helppoa sisältämään tietolähteen profiili. Kun rekisteröidyt tietolähteeksi tietojen lähteen rekisteröinti-työkalun **voi rekisteröidä objektit** -ruudussa, valitse **Sisällytä tietojen profiili**.

![](media\data-catalog-data-profile\data-catalog-register-profile.png)

Lisätietoja siitä, miten voit rekisteröidä tietolähteet on artikkelissa [rekisteröimään tietolähteet](data-catalog-how-to-register.md) ja [Azure tietoluettelon käytön aloittaminen](data-catalog-get-started.md).


## <a name="filtering-on-data-assets-that-include-data-profiles"></a>Tietoja resurssit, jotka sisältävät tietoja-profiileista suodattaminen
Saat tietää, jotka sisältävät tietoja profiilin tiedot varat, voit lisätä `has:tableDataProfiles` tai `has:columnsDataProfiles` yhtenä hakusanojen.

> [AZURE.NOTE] Valitsemalla **Sisällytä tietoja profiilin** tietojen lähteen rekisteröinti-työkalu sisältää taulukon ja sarakkeiden tason profiilitiedot. Data Catalog Ohjelmointirajapinnan sallii kuitenkin tietojen varat rekisteröitävä vain profiilitiedot joukkona.

## <a name="viewing-data-profile-information"></a>Profiilitietojen tarkasteleminen

Kun olet löytänyt sopivan tietolähteen käyttämällä profiilia, voit tarkastella tietoja profiilitietoja. Voit tarkastella tietoja profiilin, valitse tietojen resurssi ja valitse **Profiilin tiedot** tietoluettelon portal-ikkunassa.

![](media\data-catalog-data-profile\data-catalog-view.png)

Tietoja profiilin **Azure** -tietoluetteloon näyttää taulukon ja sarakkeiden profiilin tiedot myös:

### <a name="object-data-profile"></a>Objektin tiedot profiili

-   Rivien määrä
-   Taulukon kokoa
-   Kun objekti on päivitetty

### <a name="column-data-profile"></a>Sarakkeen tietojen profiili

- Sarakkeen tietotyypin
- Eri arvojen määrä
- NULL-arvoja sisältävien rivien määrä
- Vähintään, enintään tai keskiarvon sarakkeen arvojen keskihajonta

## <a name="summary"></a>Yhteenveto
Tietoja profilointi on tilastoja ja rekisteröidyt tiedot resurssien avulla voit määrittää tietojen liiketoiminnan ongelmien sopivuus tietoja. Sekä lisätä huomautuksia ja dokumentoida tietolähteitä, käyttäjäprofiileissa antaa käyttäjille tietojen tarkempaa tietoa.


## <a name="see-also"></a>Katso myös
-   [Voit rekisteröidä tietolähteet](data-catalog-how-to-register.md)
-   [Azure tietoluettelon käytön aloittaminen](data-catalog-get-started.md)
