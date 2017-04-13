<properties
    pageTitle="Tietojen tuominen Azure hakutuloksia hyödyntämällä Indeksoijilla Azure-portaalissa | Microsoft Azure | Isännöityjen pilvipalvelussa haku"
    description="Käyttäminen Azure haun Ohjattu tuominen Azure-portaalin Azure Blob storage, taulukon stroage, SQL-tietokantaan ja SQL Server Azure VMs indeksoinnin tietoihin."
    services="search"
    documentationCenter=""
    authors="HeidiSteen"
    manager="jhubbard"
    editor=""
    tags="Azure Portal"/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="heidist"/>

# <a name="import-data-to-azure-search-using-the-portal"></a>Tietojen tuominen Azure haku-portaalissa

Azure-portaalissa on **Tietojen tuominen** ohjatun Azure haun koontinäytössä tietojen lataamiseen indeksin kyselyjä. 

  ![Tietojen tuominen komentopalkki][1]

Sisäisesti ohjattu toiminto määrittää ja käynnistää *indeksointitoiminto*, automatisointi indeksointi useita vaiheita: 

- Valitse nykyinen Azure tilauksen ulkoiseen tietolähteeseen yhdistäminen
- Muodostetaan indeksi-rakenteeseen perustuvan lähde-tietorakenne
- Luo Rivijoukko, noudettu tietolähteen perustuvat asiakirjat
- Tiedostojen lisääminen search-palvelun indeksi

Voit kokeilla tämän työnkulun avulla mallitiedot DocumentDB. Siirry [Azure haun Azure-portaalin käytön aloittaminen](search-get-started-portal.md) ohjeet.

## <a name="data-sources-supported-by-the-import-data-wizard"></a>Ohjattu tuominen tukemat tietolähteet

Ohjattu tietojen tuominen tukee seuraaviin tietolähteisiin: 

- Azure SQL-tietokanta
- SQL Server Azure-AM relaatio tiedot
- Azure DocumentDB
- Azure-Blob-säiliö (-ennakkoversio)
- Azure-taulukkotallennus (-ennakkoversio)

Litistetty tietojoukko on pakollinen syöte. Voit tuoda vain yhden taulukon, tietokantanäkymä tai vastaavia tietorakenne. Sinun on luotava tämä tietorakenne ennen ohjatun toiminnon suorittaminen.

Huomaa, että Indeksoijilla muutaman ovat edelleen esikatselunäkymässä, mikä tarkoittaa, että indeksointitoiminto määritelmä on Ohjelmointirajapinnan preview-versiossa. Katso lisätietoja ja linkit [indeksointitoiminto yleiskatsaus](search-indexer-overview.md) .

## <a name="connect-to-your-data"></a>Tietojen yhdistäminen

1. Kirjaudu [Azure-portaalin](https://portal.azure.com) ja Avaa raporttinäkymät-ikkunan. Voit valita **Etsi palveluita** tekstin-palkki, joka näyttää olemassa olevia services-voimassa oleva tilaus. 

2. Valitse **Tuontitiedot** komentopalkista dian Avaa tuontitiedot-sivu.  

3. Valitse **Muodosta yhteys tietoihin** määrittää indeksointitoiminto käyttämän tietolähteen määritys. Sisäisessä tilauksen tietolähteiden ohjatun toiminnon voit yleensä tunnistaa ja lukea yhteystiedot, yleinen määritykset-vaatimusten pienentäminen.

| | |
|--------|------------|
|**Aiemmin luotuun tietolähteeseen** | Jos sinulla on jo määritetty search-palvelun Indeksoijilla, voit valita olemassa olevan tietojen lähteen määrityksen toiseen tuomista varten.|
|**Azure SQL-tietokanta** | Palvelun nimen, tietokannan käyttäjä, jolla on lukuoikeudet tunnistetietojen ja tietokannan nimi voidaan määrittää sivun tai ADO.NET-yhteysmerkkijonon kautta. Jos haluat tarkastella tai muokata ominaisuudet yhteysmerkkijonon asetuksen valitseminen <br/><br/>Taulukon tai näkymän, joka sisältää rivijoukko on määritettävä sivulla. Tämä asetus tulee näkyviin, kun yhteys on muodostettu, jossa avattava luettelo, niin, että voit tehdä valinnan.|
|**SQL Server Azure AM** | Määritä täydellinen palvelunimi, Käyttäjätunnus ja salasana ja tietokannan yhteysmerkkijonon. Jos haluat käyttää tätä tietolähdettä, aiemmin asennettuna on varmenne, joka salaa yhteys paikalliseen säilöön. <br/><br/>Taulukon tai näkymän, joka sisältää rivijoukko on määritettävä sivulla. Tämä asetus tulee näkyviin, kun yhteys on muodostettu, jossa avattava luettelo, niin, että voit tehdä valinnan.
|**DocumentDB** |Vaatimukset sisällä tilin, tietokanta ja sivustokokoelman. Sivustokokoelman kaikki tiedostot sisällytetään indeksi. Voit määrittää kyselyn Litistä tai suodattaa Rivijoukko tai muuttuneita asiakirjoja seuraavien tietojen päivityksen toimille vianmäärityksen avulla.|
|**Azure-Blob-säiliö** | Vaatimukset sisällä tallennustilan tilin ja säilön. Voit myös jos Blob-objektien nimet noudattavat virtual nimeämiskäytäntö ryhmittely tarkoituksiin, voit määrittää nimen näennäiskansio osa säilössä kansiona. Lisätietoja on kohdassa [Indeksoinnin Blob Storage (esikatselu)](search-howto-indexing-azure-blob-storage.md) . |
|**Azure-taulukkotallennus** | Vaatimukset sisällä tallennustilan tilin ja taulukkonimi. Vaihtoehtoisesti voit määrittää kyselyn, joka noutaa alijoukkoa taulukot. Lisätietoja on kohdassa [Indeksoinnin taulukkotallennus (esikatselu)](search-howto-indexing-azure-tables.md) . |

## <a name="customize-target-index"></a>Mukauta kohde indeksiä

Alustava indeksi johdetaan yleensä dataset. Lisätä, muokata tai poistaa kenttiä Viimeistele rakenne. Lisäksi voit määrittää määritteet kenttä määrittää sen myöhemmin haun toiminnan tasolla.

1. **Mukauta kohde indeksi**Määritä nimi ja **avaimen** avulla voidaan yksilöidä kunkin asiakirjan. Avain on oltava merkkijono. Jos kenttäarvojen sisällä välilyöntejä tai katkoviivat muista lisäasetusten määrittäminen **tietojen** kelpoisuuden tarkistamisen merkeistä estää.

2. Tarkista ja korjaa muut kentät. Kenttänimi ja laji on yleensä täyttää puolestasi. Voit muuttaa tietotyypin.

3. Määritä kunkin kentän indeksimääritteet:

 - Noudatettavan palauttaa kentän hakutuloksissa.
 - Suodatettavia avulla kentän suodatinlausekkeiden valitsinta.
 - Lajiteltava avulla lajittelussa käytettävän kentän.
 - Facetable ottaa käyttöön kohdistettua siirtymistä kentän.
 - Haettavat mahdollistaa teksti-haku.
  
4. Valitse **Analyzer** -välilehti, jos haluat määrittää kieli-analyzer kentän tasolla. Vain kielen analyzers voidaan määrittää tällä hetkellä. Mukautetun analyzer tai kieli analyzer, kuten avainsanan, kuviota ja niin edelleen vaadi koodi.

 - Valitse **Searchable** teksti-haku-kenttään ja ota käyttöön Analyzer avattavasta luettelosta.
 - Valitse haluamasi analyzer. [Luo indeksin monikielisen asiakirjojen](search-language-support.md) lisätietoja.

5. Valitse **Suggester** käyttöön täydentävä kyselyehdotukset käyttöön valitut kentät.


## <a name="import-your-data"></a>Tietojen tuominen

1. **Tuo tiedot**anna sille nimi indeksointitoiminto. Peruuta ohjattu tietojen tuominen tulo lasketaan indeksointitoiminto. Myöhemmin, jos haluat tarkastella tai muokata sitä, valitset sen-portaalista sijaan suorittamalla ohjattu toiminto. 

2. Määritä aikataulu, joka perustuu aluekohtaiset aikavyöhykkeen, jossa palvelu on valmisteltu.

3. Määritä lisäasetukset, jos haluat määrittää raja-arvot, onko indeksointi voit jatkaa asiakirjan kohteet poistetaan. Lisäksi voit määrittää, onko **avaimen** kentät voivat sisältää välilyöntejä ja vinoviivaa.  

## <a name="edit-an-existing-indexer"></a>Muokkaa aiemmin indeksointitoiminto

Kaksoisnapsauta palvelun Raporttinäkymät-ikkunan dia kaikki Indeksoijilla tilauksen luotu luettelo indeksointitoiminto-ruutu. Kaksoisnapsauta jotakin Indeksoijilla avulla voit muokata tai poistaa sen. Voit korvata aiemmin toisella hakemiston, tietolähteen muuttaminen ja indeksoinnin aikana virhe raja-arvot asetusten määrittäminen.

## <a name="edit-an-existing-index"></a>Muokkaa aiemmin luotua indeksiä

Azure hakutoiminnossa indeksin rakenteellista päivitykset edellyttävät muodosta, indeksi, joka koostuu poistaminen indeksin, luotaessa indeksi ja uudelleen tietoja. Rakenteelliset päivityksiin kuuluvat tietotyypin muuttaminen ja nimeäminen uudelleen tai poistaminen kentän.

Muutokset, jotka eivät edellytä muodosta Sisällytä lisäämällä uuden kentän muuttaminen tulosmalli profiilit-muuttaminen suggesters tai analyzers kielen muuttaminen. Lisätietoja on kohdassa [Päivitä hakemisto](https://msdn.microsoft.com/library/azure/dn800964.aspx) .

## <a name="next-step"></a>Seuraava vaihe

Lue lisätietoja Indeksoijilla linkit:

- [Indeksoinnin Azure SQL-tietokanta](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md)
- [Indeksoinnin DocumentDB](../documentdb/documentdb-search-indexer.md)
- [Indeksoinnin Blob Storage (ennakkoversio)](search-howto-indexing-azure-blob-storage.md)
- [Indeksoinnin taulukkotallennus (ennakkoversio)](search-howto-indexing-azure-tables.md)



<!--Image references-->
[1]: ./media/search-import-data-portal/search-import-data-command.png

