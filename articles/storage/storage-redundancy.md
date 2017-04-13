<properties
    pageTitle="Azure tallennustilan replikoinnin | Microsoft Azure"
    description="Microsoft Azure-tallennustilan tilin tietojen replikoinnin kestävyyttä ja suuren käytettävyyden. Replikointiasetukset ovat paikallisesti tarpeettomat storage (LRS), vyöhykkeen tarpeettomat storage (ZRS), geo tarpeettomat storage (GRS) ja lukuoikeudet geo tarpeettomat storage (RA GRS)."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="tamram"/>

# <a name="azure-storage-replication"></a>Azure tallennustilan sallittuja

Microsoft Azure-tallennustilan tilin tiedot aina replikoida kestävyyttä ja suuren käytettävyyden varmistamiseksi. Replikoinnin Kopioi tietosi saman tietokeskuksen sisällä tai toinen tiedot-käyttöoikeudet, riippuen replikoinnin minkä vaihtoehdon valitset. Replikoinnin suojaa tietoja ja säilyttää sovelluksen ylös-ajankäytön lyhytkestoisia laitteisto yhteydessä. Jos tiedoissa on replikoida toisen tietokeskuksen, joka suojaa vastaan kriittinen virhe, valitse ensisijainen sijainti tiedoissa.

Replikoinnin varmistaa, tallennustilan tilin vastaa [Service-palvelutasosopimusta (SLA) tallennuksessa](https://azure.microsoft.com/support/legal/sla/storage/) jopa menettämisen tapahtuessa virheet. Katso lisätietoja Azure-tallennustilan SLA takaa kestävyyttä ja käytettävyys. 

Kun luot tallennustilan tilin, voit valita jompikumpi seuraavista Replikointiasetukset:  

- [Paikallisesti tarpeettomat storage (LRS)](#locally-redundant-storage)
- [Vyöhykkeen tarpeettomat storage (ZRS)](#zone-redundant-storage)
- [GEO tarpeettomat storage (GRS)](#geo-redundant-storage)
- [Lukuoikeudet geo tarpeettomat storage (RA GRS)](#read-access-geo-redundant-storage)

Lukuoikeudet geo tarpeettomat tallennus (RA GRS) on oletusarvo, kun luot uuden tallennustilan tilin.

Seuraavassa taulukossa kuvataan lyhyesti LRS, ZRS, GRS ja RA-GRS eroista samalla, kun seuraavien osien osoite erityyppisiin replikoinnin tarkemmin.

| Replikoinnin määrittäminen                                                               | LRS | ZRS | GRS | RA GRS |
|:----------------------------------------------------------------------------------|:---|:---|:---|:------|
| Tietojen replikoinnin useita palvelinkeskusten yli.                                     | Ei  | Kyllä | Kyllä | Kyllä    |
| Tietoja voi lukea toissijainen sijainnista ja ensisijainen sijainnista. | Ei  | Ei  | Ei  | Kyllä    |
| Tietoja ylläpidetään eri solmuissa kopioiden lukumäärä.                             | 3   | 3   | 6   | 6      |

Alla on tietoja eri redundancy niiden asetusten kohdassa [Azure-tallennustilan hinnat](https://azure.microsoft.com/pricing/details/storage/) .

>[AZURE.NOTE] Premium tallennustilan tukee vain paikallisesti tarpeettomat tallentamista (LRS). Lisätietoja tallennustilan Premium on [Premium tallennustilan: tehokas tallennustila Azure virtuaalikoneen työmääriä](storage-premium-storage.md).

## <a name="locally-redundant-storage"></a>Paikallisesti ylimääräinen

Paikallisesti tarpeettomat storage (LRS) kopioi tietojen tallennustilan asteikko joka isännöidään palvelinkeskukseen, jonka loit tallennustilan tilin alueen sisällä kolme kertaa. Kirjoituspyyntö palauttaa onnistuu vain, kun se on kirjoitettu kolme replikoita. Nämä kolme replikoiden kunkin sijaitsevat eri vika toimialueet ja Päivitä toimialueiden yhden tallennustilan asteikko. 

Tallennustilan asteikko on kokoelma tavara tallennustilan solmujen. Vian toimialue (Suodatuspalvelu) on ryhmä, joka edustaa virheen fyysinen yksikkö ja kuuluvat samaan fyysinen Teline solmujen katsotaan solmujen. Päivitä toimialueen (UD) on ryhmä solmujen, joka on päivitetty yhdessä palvelupäivityksen (rahoittaja) aikana. Kolme replikoiden leviävät UDs ja FDs yhden tallennustilan asteikko, varmista, että tiedot ovat käytettävissä, vaikka laitteistovirhe vaikuttaa yhden Teline tai solmut ovat päivityksen aikana rahoittaja kuluessa. 

LRS pienimmät kustannus-vaihtoehto ja tarjoaa vähintään kestävyyttä verrattuna muut asetukset. Jos palvelinkeskuksen tason huono (fire, samantyyppisten jne) kaikkien kolmen replikoiden ehkä kadonneet tai palauttaa. Riskin pienentämään useimpien sovellusten suositellaan Geo tarpeettomat Storage (GRS).

Paikallisesti ylimääräinen olla olisi tietyissä tilanteissa: 

- On suurin Azuren tallennustilaan Replikointiasetukset suurin kaistanleveys.

- Jos sovellus tallentaa tiedot, jotka helposti uudelleen, käyttäjä voi halutessaan LRS.

- Jotkin sovellukset on rajoitettu replikoiminen tietojen vain maan vuoksi tietojen hallinnon mukaan. Pisteparin alueen voi olla jossain muussa maassa; alueen paria Lisätietoja on artikkelissa [Azure alueet](https://azure.microsoft.com/regions/) .

## <a name="zone-redundant-storage"></a>Vyöhykkeen ylimääräinen

Vyöhykkeen tarpeettomat storage (ZRS) kopioi tiedot asynkronisesti palvelinkeskusten lisäksi tallentaminen kolme replikoiden samalla LRS, mikä antaa suurempi kuin LRS kestävyyttä yksi tai kaksi alueiden välillä. Tiedot tallennetaan ZRS on kestävät, vaikka ensisijainen palvelinkeskuksen ei ole käytettävissä tai palauttaa.
Asiakkaat, joiden aiot käyttää ZRS olisi otettava huomioon: 

- ZRS on vain saatavilla yleisiä tarkoitus-tallennustilan tilin estä BLOB-objektit ja tukee vain versioissa tallennustilan palvelun 2014-02 – 14 ja uudemmissa versioissa. 

- Asynkroninen replikoinnin liittyy viiveen, paikallisten tietojen jos se on mahdollista, että muutokset, jotka on ei ole vielä replikoida toissijaisen menetetään, jos tietoja ei voi palauttaa peräisin.

- Se ei ehkä ole käytettävissä, kunnes Microsoft aloittaa toissijaisen automaattisesti.

- ZRS tilejä ei voi muuntaa myöhemmin LRS tai GRS. Vastaavasti aiemmin LRS tai GRS tiliä ei voi muuntaa ZRS-tilille.

- Arvot tai virheloki-ominaisuus ei ole ZRS tilit. 

## <a name="geo-redundant-storage"></a>GEO ylimääräinen

GEO tarpeettomat storage (GRS) kopioi tietojen toissijainen alue, joka on satoja Mailia ensisijainen alueen ulkopuolella. Tallennustilan-tililläsi on käytössä GRS, jos tiedot ovat kestävät jopa kyseessä valmis alueellisen käyttökatkosta tai tietojen, jossa ensisijaisen alue ei voi palauttaa.

Tallennustilan tilin kanssa käytössä GRS päivitys on ensin sitoutunut ensisijainen alue, johon se on replikoida kolme kertaa. Valitse päivitys on replikoida asynkronisesti toissijainen alue, johon se on myös replikoida kolme kertaa. 

GRS kanssa ensisijainen ja toissijainen-alueiden hallinta replikoiden erillisessä vika toimialueilla ja Päivitä toimialueiden tallennustilan asteikko ohjeiden LRS kanssa.

Huomioon otettavia seikkoja:

- Asynkroninen replikoinnin liittyy viiveen, alueellisen huono jos se on mahdollista, että muutokset, joita ei vielä ole replikoida toissijainen alueen menetetään, jos ensisijainen alueelta ei voi palauttaa tietoja.

- Se ei ole käytettävissä, jos Microsoft aloittaa toissijainen alueen automaattisesti.

- Jos jokin sovellus haluaa lukea toissijainen alue, käyttäjän on otettava käyttöön RA GRS. 

Kun luot tallennustilan tilin, valitse tili ensisijainen alue. Toissijainen alueen määritetään ensisijainen alueen perusteella, eikä niitä voi muuttaa. Seuraavassa taulukossa on ensisijainen ja toissijainen alueen parit.

| Ensisijainen             | Toissijainen           |
|---------------------|---------------------|
| Pohjois-keskitetyn USA    | Etelä keskitetyn USA    |
| Etelä keskitetyn USA    | Pohjois-keskitetyn USA    |
| Yhdysvaltojen Itä             | Länsi USA             |
| Länsi USA             | Yhdysvaltojen Itä             |
| Yhdysvaltojen Itä 2           | Keskitetyn USA          |
| Keskitetyn USA          | Yhdysvaltojen Itä 2           |
| Pohjois-Eurooppa        | Länsi Europe         |
| Länsi Europe         | Pohjois-Eurooppa        |
| Etelä Itä-Aasian     | Itä-Aasian           |
| Itä-Aasian           | Etelä Itä-Aasian     |
| Itä kiina          | Pohjois-kiina         |
| Pohjois-kiina         | Itä kiina          |
| Japani Itä          | Japani Länsi          |
| Japani Länsi          | Japani Itä          |
| Brasilia Etelä        | Etelä keskitetyn USA    |
| Australia Itä      | Australia varaaja |
| Australia varaaja | Australia Itä      |
| Intia Etelä         | Intia Keski       |
| Intia Keski       | Intia Etelä         |
| Yhdysvaltain gov – Iowa         | Yhdysvaltain gov – Virginia     |
| Yhdysvaltain gov – Virginia     | Yhdysvaltain gov – Iowa         |
| Kanada Keski      | Kanada Itä          |
| Kanada Itä         | Kanada Keski      |
| Iso-Britannia Länsi             | Iso-Britannia Etelä            |
| Iso-Britannia Etelä            | Iso-Britannia Länsi             |
| Saksa Keski     | Saksa koillisen   |
| Saksa koillisen   | Saksa Keski     |
| Länsi US 2           | Keski-Länsi USA     |
| Keski-Länsi USA     | Länsi US 2           |

Alueiden Azure tukemat uusimpia tietoja artikkelissa [Azure-alueita](https://azure.microsoft.com/regions/).
 
## <a name="read-access-geo-redundant-storage"></a>Lukuoikeudet geo tarpeettomat tallennustila

Lukuoikeudet geo tarpeettomat storage (RA GRS) suurentaa käytettävyys tallennustilan tilissäsi tarjoamalla toissijaiseen sijaintiin lisäksi yli kummallakin alueella GRS myöntämä replikoinnin tietojen vain luku-tilassa. 

Kun otat käyttöön vain luku-tilassa toissijainen alueen tietoihin, tiedot ovat käytettävissä toissijainen päätepisteen-tallennustilan tilin ensisijainen päätepisteen lisäksi. Toissijainen päätepisteen muistuttaa ensisijainen päätepisteen, mutta Lisää liitteen `–secondary` tilin nimeä. Jos ensisijainen päätepiste Blob-palvelun on esimerkiksi `myaccount.blob.core.windows.net`, valitse toissijainen päätepiste on `myaccount-secondary.blob.core.windows.net`. Tallennustilan tilin pikanäppäimet ovat samat kuin ensisijaisen ja toissijaisen-päätepisteet.

Huomioon otettavia seikkoja:

- Sovellus on hallinta mikä se on vuorovaikutteinen RA GRS käytettäessä päätepiste. 

- RA GRS on tarkoitettu suuren käytettävyyden tarkoituksiin. Skaalattavuus ohjeita Tarkista [suorituskyvyn tarkistusluettelon](storage-performance-checklist.md).

## <a name="next-steps"></a>Seuraavat vaiheet

- [Azure-tallennustilan hinnat](https://azure.microsoft.com/pricing/details/storage/)
- [Tietoja Azure-tallennustilan](storage-create-storage-account.md)
- [Azure-tallennustilan skaalattavuus ja suorituskyvyn kohteet](storage-scalability-targets.md)
- [Microsoft Azure Redundancy tallennusasetukset- ja lukuoikeudet Geo tarpeettomat tallennustila](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx)  
- [SOSP paperi - Azure tallennustilan: Erittäin käytettävissä Pilvitallennuspalveluun vahva yhdenmukaisuuden kanssa](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)  
