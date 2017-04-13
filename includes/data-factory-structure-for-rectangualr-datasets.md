## <a name="specifying-structure-definition-for-rectangular-datasets"></a>Suorakulmainen tietojoukkoja rakenteen määritys määrittäminen
Tietojoukkoja JSON rakenne-kohdassa (jossa rivit ja sarakkeet) suorakulmainen taulukoiden **valinnaisen** osan ja sisältää joukon sarakkeita taulukkoon. Voit käyttää rakenne-kohdassa joko tarjoavat tiedot tyyppi muuntamisen tai tekevät sarakkeiden yhdistämismäärityksiä. Seuraavissa osissa on nämä ominaisuudet yksityiskohtaiset tiedot. 

Kukin sarake sisältää seuraavat ominaisuudet:

| Ominaisuus | Kuvaus | Pakollinen |
| -------- | ----------- | -------- |
| Nimi | Sarakkeen nimi. | Kyllä |
| tyyppi | Sarakkeen tietotyypin. Katso tyyppi muunnokset osan Lisää tiedot koskevat milloin kannattaa määrittämäsi tiedot | Ei |
| maa-asetus | .NET pohjaiset culture, jota käytetään, kun tyyppi on määritetty ja .NET tyyppi on Datetime tai DateTimeOffset-arvoa. Oletusarvo-fi-yhteyttä ".  | Ei |
| Muotoile | Muotoile merkkijono, jota käytetään, kun tyyppi on määritetty ja .NET tyyppi on Datetime tai DateTimeOffset-arvoa. | Ei |

Seuraavassa esimerkissä näkyy taulukko, jossa on kolme saraketta käyttäjätunnus, nimi ja lastlogindate on rakenne-osassa JSON.

    "structure": 
    [
        { "name": "userid"},
        { "name": "name"},
        { "name": "lastlogindate"}
    ],

Käytä seuraavia ohjeita, milloin haluat sisällyttää "rakenteen-tiedot ja sisällön **rakenne** -osassa.

- **Rakenteellisia tietolähteitä** , joka tallentaa tiedot rakenteen ja kirjoita tietoja itse (lähteistä kuten SQL Server-Oracle-Azure-taulukosta jne.), "rakenteen-kohta tulee määrittää vain, jos haluat tietoja sekä tehdä tietyt sarakkeet mainitaan käsittelytoiminto tietyn lähdesarakkeiden sarakkeen yhdistäminen ja heidän nimensä eivät ole samoja (Katso tiedot sarakkeen yhdistäminen osan alla). 

    Kuten edellä mainittiin tiedot on valinnainen "rakenteen-osassa. Rakenteellisia tietolähteitä, tiedot on jo saatavana osana tietojoukko määritelmä tietovaraston niin Älä sisällytä tiedot Kun sisällytät "rakenteen-osassa.
- **Lue tietolähteiden (erityisesti Azure blob)-rakenteen** voit tallentaa tiedot ilman tallentaminen rakenteen tai tyypin liittyviä tietoja. Tietolähteiden tämäntyyppisille Sisällytä "rakenteen-2 seuraavissa tapauksissa:
    - Haluat tehdä sarakkeen yhdistäminen.
    - Tietojoukon ollessa kopioi tehtävän lähteen tiedot "rakenteessa" antaisit ja tietojen factory käyttää tätä tiedot muuntaminen käsittelytoiminto alkuperäiset tyypit. Lisätietoja on artikkelissa [tietojen ja sieltä pois Azure-Blob-objektien siirtäminen](../articles/data-factory/data-factory-azure-blob-connector.md) artikkelissa.

### <a name="supported-net-based-types"></a>Tuettu. VERKON perustuva tyypit 
Tietoja factory tukee seuraavia CLS yhteensopiva .NET mukaan tyyppi arvoja tarjoamiseksi tiedot "rakenteessa" luku tietolähteisiin, kuten Azure-blob-rakenne.

- Int16
- Int32 
- Int64
- Yksittäisen
- Kaksinkertainen
- Desimaaliluku
- Byte]
- Bool
- Merkkijono 
- GUID-tunnus
- Päivämäärä ja aika
- DateTimeOffset-arvoa
- Aikajakson 

Datetime & DateTimeOffset-arvoa voit myös määrittää "culture" & "muoto" merkkijonon helpottamaan jäsentää mukautettu Datetime-merkkijono. Katso otoksen muuntamisen alla.

