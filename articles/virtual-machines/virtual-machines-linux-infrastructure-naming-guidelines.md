<properties
    pageTitle="Infrastruktuurin nimeäminen ohjeet | Microsoft Azure"
    description="Lue lisää ohjeita suunnittelu ja käyttöönotto avaimen Azure infrastruktuuripalvelut nimeämisestä."
    documentationCenter=""
    services="virtual-machines-linux"
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="iainfou"/>

# <a name="infrastructure-naming-guidelines"></a>Infrastruktuurin nimeämiseen liittyviä ohjeita

[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)] 

Tässä artikkelissa keskitytään tietoja voit esitellä nimeämiskäytännöt kaikki eri Azure-resursseissa luonnissa resurssien loogisen tai helposti tunnistettavissa joukko ympäristön yli.

## <a name="implementation-guidelines-for-naming-conventions"></a>Toteutuksen jälkeisen ohjeet nimeämiskäytännöt

Päätökset:

- Mitkä ovat oman nimeämiskäytännöt Azure resurssien?

Tehtävät:

- Määritä jälkiliitteet yhtenäisyyden säilyttämiseksi resurssien avulla.
- Määritä tallennustilan tilin nimiin vaatimus niiden yksilöiviä.
- Asiakirjan nimeämiskäytäntöä, jotta voidaan käyttää ja jakaa varmistaa kaikissa ominaisuuksissa kaikille osapuolille.

## <a name="naming-conventions"></a>Nimeämiskäytännöt

Sinulla on oltava hyvä nimeämiskäytäntöä paikallaan ennen kuin luot mitään Azure-tietokannassa. Nimeämiskäytäntöä varmistaa, että kaikki resurssit helpompi alemmalle tasolle hallinnollisten rasitteiden resursseja hallintaan liittyvistä ennakoitavissa nimi.

Voit esimerkiksi määritetty koko organisaation tai tietyn Azure-tilauksen tai tilin nimeämiskäytännöt tietyt noudattamalla. On helppoa organisaatioiden henkilöt voivat laatia implisiittinen säännöt, kun käsittelet Azure resursseja, mutta haluat voi skaalata työryhmät yhteistyössä Azure-tietokannassa.

Sopivat etukäteen nimeämiskäytännöt joukko. On joitakin tapoja, nimeämiskäytännöt, Leikkaa yli, joka määrittää koskevat säännöt.

## <a name="affixes"></a>Kiinnitettävä

Miltä määrittämään nimeämiskäytännön mukaisesti, yksi päätös on onko kiinnitettävä on:

- Nimi (etuliite) alkuun
- Nimi (jälkiliite) loppuun

Esimerkiksi seuraavat kaksi mahdollista nimeä resurssiryhmä avulla `rg` kiinnitettävä:

- RG – Web App-sovelluksen (etuliite)
- Web App-sovelluksen-Rg (jälkiliite)

Jälkiliitteet voidaan viitata eri ominaisuuksia, jotka kuvaavat tarvittavat resurssit. Seuraavassa taulukossa on joitakin esimerkkejä tavanomaisesta käytöstä.

| Kuvasuhde                               | Esimerkkejä                                                               | Huomautuksia                                                                                                      |
|:-------------------------------------|:-----------------------------------------------------------------------|:-----------------------------------------------------------------------------------------------------------|
| Ympäristön                          | keskihajonta-stg, tuot                                                         | Sen mukaan, aihe ja kussakin ympäristössä nimi.                                                     |
| Sijainti                             | usw (Länsi Yhdysvallat), käytä (Itä US 2)                                         | Sen mukaan, palvelinkeskuksen alue tai organisaation alue.                               |
| Azure-osan, palveluun tai tuotteen | Resurssiryhmä-VPN VNet RG                        | Sen mukaan, johon resurssi tukee tuote.                                          |
| Rooli                                 | DB app, web                                                           | Sen mukaan, virtuaalikoneen rooli.                                                              |
| Esiintymän                             | 01, 02, 03, jne.                                                       | Resurssien, joissa on enemmän kuin yksi. Esimerkiksi ladata pilvipalvelussa tasapainoinen verkko-palvelimiin. |


Muodostettaessa yhteyttä nimeämiskäytännöt Varmista, että siinä selkeästi mitä jälkiliitteet käyttämään mistäkin, resurssien ja mitkä sijainti (etuliite ja jälkiliite).

## <a name="dates"></a>Päivämäärät

Usein on tärkeää selvittää luontipäivän resurssin nimen. On suositeltavaa VVVVKKPP päivämäärämuoto. Tässä muodossa, joka kertoo, joka ei ole vain on täydellinen päivämäärä on tallennettu, mutta myös merkkijonoon eroavat toisistaan vain kaksi resurssit lajitellaan aakkosjärjestykseen ja aikajärjestyksessä.

## <a name="naming-resources"></a>Resurssien nimeäminen

Määritä kunkin nimeämiskäytännön resurssin, jossa on oltava säännöt, jotka määrittävät määrittäminen nimet kullekin resurssille, joka on luotu. Sääntöjen on voimassa kaikenlaisia resurssit, kuten:

- Tilaukset
- Tilit
- Tallennustilan tilit
- Virtuaalinen verkot
- Aliverkosta
- Käytettävyys joukot
- Resurssiryhmät
- Näennäiskoneiden
- Päätepisteet
- Verkon käyttöoikeusryhmät
- Roolit

Voit varmistaa, että nimi on tarpeeksi tietoa, voit selvittää, mitä resurssille se viittaa, kannattaa käyttää kuvaavat nimet.

## <a name="computer-names"></a>Tietokoneen nimien

Kun luot virtual machine (AM), Azure edellyttää AM nimi enintään 64 merkkiä, jota käytetään resurssin nimen. Azure käyttää samaa nimeä AM käyttöjärjestelmiä. Seuraavat nimet ei aina voi kuitenkin sama.

Jos AM luodaan .vhd kuva tiedostosta, joka sisältää jo käyttöjärjestelmä, Azure AM nimi voi olla eri kuin AM käyttöjärjestelmän tietokonenimi. Näin voit lisätä/tai vaikeuksiin aste AM hallinta, joiden vuoksi ei ole suositeltavaa. Määritä Azure AM resurssin nimi on sama kuin määrität kyseisen AM käyttöjärjestelmän tietokonenimi.

On suositeltavaa, että Azure AM nimi on sama kuin pohjana käyttöjärjestelmän tietokonenimi.

## <a name="storage-account-names"></a>Tallennustilan tilin nimi

Tallennustilan-tileillä on erityisen säätelevät heidän nimensä. Voit käyttää vain pieniä kirjaimia ja numeroita. Lisätietoja on kohdassa [Luo tallennustilan tili](../storage/storage-create-storage-account.md#create-a-storage-account) . Lisäksi tallennustilan tilin nimi, core.windows.net, jossa on oltava yleisesti kelvollinen yksilöllinen DNS-nimi. Jos tallennustilan tilin nimi on mystorageaccount, tuloksena olevat DNS-nimet on esimerkiksi oltava yksilöivä otsikko:

- mystorageaccount.BLOB.Core.Windows.NET
- mystorageaccount.Table.Core.Windows.NET
- mystorageaccount.Queue.Core.Windows.NET


## <a name="next-steps"></a>Seuraavat vaiheet
[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)] 