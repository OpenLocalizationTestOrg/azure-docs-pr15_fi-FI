<properties
   pageTitle="Suodattimen merkkijonot luomisesta, taulukon suunnitteluohjelman | Microsoft Azure"
   description="Taulukon suunnitteluohjelman rakentaa suodattimen merkkijonoja"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="storage"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="constructing-filter-strings-for-the-table-designer"></a>Suodattimen merkkijonojen luominen taulukon suunnittelua varten

## <a name="overview"></a>Yleiskatsaus

Voit suodattaa tietoja Azure-taulukon, joka näkyy Visual Studio **Taulukon suunnitteluohjelman**muodostaa suodattimen merkkijonon ja Syötä Suodatin-kenttään. Suodattimen merkkijonosyntaksi WCF-tietopalvelut määrittämiä ja muistuttaa SQL WHERE-lauseen, mutta taulukon-palvelun kautta HTTP-pyyntö lähetetään. **Taulukon suunnitteluohjelman** käsittelee oikea koodaus puolestasi, niin voit suodattaa haluamasi ominaisuuden arvon, tarvitsee vain kirjoittaa ominaisuuden nimi, vertailuoperaattori, ehtoarvo ja vaihtoehtoisesti totuusarvo operaattorin suodatus-kenttään. Sinun ei tarvitse sisällytettävien $filter kysely-vaihtoehdon, jos URL-Osoitteen kautta [Tallennustilan Services REST API-viittaus](http://go.microsoft.com/fwlink/p/?LinkId=400447)taulukkoa kysely on luomisesta tapaan.

WCF-tietopalvelut perustuvat [Open Data Protocol](http://go.microsoft.com/fwlink/p/?LinkId=214805) (OData). Lisätietoja suodattimen järjestelmän kysely-vaihtoehdon (**$filter**) on artikkelissa [OData URI nimeämiskäytännön määritys](http://go.microsoft.com/fwlink/p/?LinkId=214806).

## <a name="comparison-operators"></a>Vertailuoperaattorit

Seuraavat loogisilla operaattoreilla tuetaan ominaisuuden tyypeissä:

|Loogisten operaattorien|Kuvaus|Esimerkki suodattimen merkkijono|
|---|---|---|
|eq|Yhtä suuri kuin|Kaupungin eq "Redmond"|
|gt|Suurempi kuin|Gt hintaan 20|
|sivu|Suurempi tai yhtä suuri kuin|Hinta-sivu 10|
|lt|Pienempi kuin|Hinta lt 20|
|tiedostoon|Pienempi tai yhtä|Hinta 100-tiedostoon|
|Uusi|Eri suuri kuin|Kaupungin ne "Helsinki"|
|ja|Ja|Hinta-tiedostoon 200 ja gt hintaan 3.5|
|tai|Tai|Hinta-tiedostoon 3.5 tai gt hintaan 200|
|ei|Ei|ei isAvailable|

Suodatinmerkkijono luodessaan seuraavat säännöt ovat tärkeitä:

- Loogisten operaattorien avulla voit verrata ominaisuuden arvon. Huomaa, että ei ole mahdollista vertaileminen dynaaminen arvo; ominaisuus yhdeltä sivulta lausekkeen on oltava vakio.

- Suodatusmerkkijono kaikista osista kirjainkoko on merkitsevä.

- Vakion arvo on oltava samaa tietotyyppiä, jotta suodatin ominaisuutena palauttamaan kelvollinen tulokset. Saat lisätietoja tuetuista ominaisuuden tyypeistä [taulukon Service-tietomallin perusteet](http://go.microsoft.com/fwlink/p/?LinkId=400448).

## <a name="filtering-on-string-properties"></a>Suodattaminen merkkijonon ominaisuudet

Jos suodatat merkkijonon ominaisuuksien, kirjoita merkkijonovakio puolilainausmerkein.

Seuraavassa esimerkissä suodattimet **PartitionKey** ja **RowKey** ; ominaisuudet ei näppäintä lisäominaisuuksia myös lisätään suodatin-merkkijono:

    PartitionKey eq 'Partition1' and RowKey eq '00001'

Voit asettaa Kukin suodatin-lausekkeen sulkeissa, mutta sitä ei tarvita:

    (PartitionKey eq 'Partition1') and (RowKey eq '00001')

Huomaa, että taulukon-palvelu ei tue yleismerkin kyselyt, ja ne eivät tue taulukon suunnitteluohjelman joko. Voit kuitenkin suorittaa vastaavat käyttämällä vertailuoperaattorit haluamasi etuliite etuliite. Seuraava esimerkki palauttaa kohteita, kun Sukunimi-ominaisuuden alkaa kirjaimella "A":

    LastName ge 'A' and LastName lt 'B'

## <a name="filtering-on-numeric-properties"></a>Lukuarvojen suodattaminen

Jos haluat suodattaa kokonaisluku tai liukuluku, Määritä ilman lainausmerkkejä.

Tässä esimerkissä palauttaa kaikki kohteet, joiden arvo on suurempi kuin 30 ikä-ominaisuuden:

    Age gt 30

Tässä esimerkissä palauttaa kaikki kohteet, joiden arvo on pienempi tai yhtä suuri 100.25 AmountDue-ominaisuuden:

    AmountDue le 100.25

## <a name="filtering-on-boolean-properties"></a>Suodattaminen totuusarvo-ominaisuudet

Jos haluat suodattaa totuusarvo, Määritä **Tosi** tai **Epätosi** ilman lainausmerkkejä.

Seuraava esimerkki palauttaa kaikki kohteet, joissa IsActive-ominaisuudeksi on määritetty **Tosi**:

    IsActive eq true

Voit myös kirjoittaa ilman loogista operaattoria tässä suodatin-lausekkeen. Seuraavassa esimerkissä taulukko-palvelun myös palauttavat kaikki kohteet joissa IsActive on **Tosi**:

    IsActive

Palauttaa kaikki kohteet, jossa IsActive on EPÄTOSI, voit käyttää not operaattori:

    not IsActive

## <a name="filtering-on-datetime-properties"></a>Suodattaminen DateTime-ominaisuudet

Voit suodattaa DateTime-arvo, Määritä **datetime** -avainsana, perään kirjoitetaan puolilainausmerkkeihin pvm. / klo-vakio. Päivämäärä ja kellonaika-vakio on oltava yhdistetty UTC-muodossa, kuvatulla tavalla [Muotoilun DateTime ominaisuusarvoihin](http://go.microsoft.com/fwlink/p/?LinkId=400449).

Seuraava esimerkki palauttaa kohteita, missä CustomerSince-ominaisuuden arvoksi 10. heinäkuussa 2008:

    CustomerSince eq datetime'2008-07-10T00:00:00Z'
