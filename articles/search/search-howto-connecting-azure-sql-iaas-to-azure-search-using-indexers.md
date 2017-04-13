<properties 
    pageTitle="SQL Server Azure haun indeksointitoiminto yhteyden määrittämistä Azure virtuaalikoneen | Microsoft Azure | Indeksoijilla" 
    description="Salatun tietoyhteyksien ottaminen käyttöön ja palomuurin Salli yhteydet SQL Server Azure virtuaalikoneen (AM) käyttöön indeksointitoiminto Azure-haun avulla." 
    services="search" 
    documentationCenter="" 
    authors="jack4it" 
    manager="pablocas" 
    editor=""/>

<tags 
    ms.service="search" 
    ms.devlang="rest-api" 
    ms.workload="search" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.date="09/26/2016" 
    ms.author="jackma"/>

# <a name="configure-a-connection-from-an-azure-search-indexer-to-sql-server-on-an-azure-vm"></a>Määrittää Azure haun indeksointitoiminto yhteyden SQL Server Azure-AM

[Azure SQL](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md#frequently-asked-questions)-tietokantaan yhteyden Azure hakutuloksia hyödyntämällä Indeksoijilla mainittuja Azure-haku voi luoda Indeksoijilla vastaan **SQL Server Azure VMs** (tai **SQL Azure-VMs** lyhyen varten), mutta on muutama tietoturvaan liittyvät edellytykset huolehtia ensimmäisen. 

**Tehtävän kesto:** Noin 30 minuuttia, jos olet jo asennettu varmennetta AM.

## <a name="enable-encrypted-connections"></a>Salatun tietoyhteyksien ottaminen käyttöön

Azure haku edellyttää salatun kanavan kaikki indeksointitoiminto pyyntöjä julkinen internet-yhteyden kautta. Tässä osassa on esitetty vaiheet, jotta Kirjautumistapa saadaan toimimaan.

1. Tarkista Vahvista aihe on Azure AM täydellinen toimialuenimi (FQDN) varmenteen ominaisuudet. Voit käyttää työkalua, kuten CertUtils tai Varmenteet-laajennuksen ominaisuuksien näyttämiselle. Saat FQDN AM palvelun sivu Essentials osan **julkiseen IP-osoite ja DNS nimi otsikko** -kenttään [Azure portal](https://portal.azure.com/).

    - Muotoillaan VMs luodaan uudempaan **Resurssienhallinta** mallia käyttämällä, FQDN `<your-VM-name>.<region>.cloudapp.azure.com`. 

    - Muotoillaan vanhempia VMs luodaan **perinteinen** AM, FQDN `<your-cloud-service-name.cloudapp.net>`. 

2. Määritä SQL Server käyttää varmenteen Rekisterieditorin (regedit). 

    Vaikka toimintoa käytetään usein SQL Serverin määritysten hallinta, et voi käyttää tässä tapauksessa. Se ei löydä tuotua varmennetta, koska Azure-AM FQDN ei vastaa täydellinen toimialuenimi (se tunnistaa toimialueen paikalliseen tietokoneeseen tai verkon toimialueeseen, johon se on yhdistetty) AM mukaisesti. Kun nimet eivät vastaa, voit määrittää varmenteen regedit avulla.

    - Kirjoita regedit, siirry tämän rekisteriavaimen: `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\[MSSQL13.MSSQLSERVER]\MSSQLServer\SuperSocketNetLib\Certificate`.
     
    `[MSSQL13.MSSQLSERVER]` Osan vaihtelee versio ja esiintymän nimi. 

    - **Varmenteen** avaimen arvoksi SSL-varmenne, voit tuoda AM **allekirjoitus** .

    Saat allekirjoitus, jotkin parempi vaihtoehto muiden useilla tavoilla. Jos kopioit **Varmenteet** -laajennuksen MMC: stä, ensimmäinen merkki [Tässä tukiartikkelissa kuvatulla tavalla](https://support.microsoft.com/kb/2023869/), joka aiheuttaa virheen, kun yrität yhteyden näkymätön Nosta todennäköisesti. Usealla tavalla olemassa tämän ongelman korjaaminen. Helpoin on ASKELPALAUTINTA päälle ja vahvista poistaminen ensimmäistä merkkiä regedit avaimen arvokenttää allekirjoitus ensimmäisen merkin. Voit myös kopioida allekirjoitus toista työkalua.

3. Myöntää palvelutili. 

    Varmista, että SQL Server-tilin on myönnetty tarvittavat käyttöoikeudet, valitse Yksityinen avain SSL-varmenteen. Jos tämä vaihe unohtuvat helposti, SQL Server ei käynnisty. Voit käyttää **Varmenteet** laajennuksen tai **CertUtils** tämän tehtävän.

4. Käynnistä SQL Server-palvelu uudelleen.

## <a name="configure-sql-server-connectivity-in-the-vm"></a>Määritä SQL Server-yhteyttä AM

Kun olet määrittänyt Azure haku edellyttää salattua yhteyttä-on lisämäärityksiä vaihetta SQL Server Azure VMs sisäinen. Jos et ole jo tehnyt niin, seuraava vaihe on valmis konfigurointi käyttämällä jompaakumpaa seuraavissa artikkeleissa:

- Artikkelissa **Resurssienhallinta** AM- [yhteyden, SQL Server Virtual tietokoneen Azure resurssien hallinnan avulla](../virtual-machines/virtual-machines-windows-sql-connect.md). 

- **Perinteinen** AM ohjeaiheessa [yhteyden, SQL Server Virtual tietokoneen Azure perinteinen](../virtual-machines/virtual-machines-windows-classic-sql-connect.md).

Tarkista erityisesti kunkin artikkelin "yhteyden Internetin välityksellä-osassa.

## <a name="configure-the-network-security-group-nsg"></a>Määrittää verkko-käyttöoikeusryhmän (NSG)

Ei ole epätavallisia Määritä NSG ja vastaavan Azure päätepisteen tai luettelon Käyttöoikeusluettelon (Access Control), jotta Azure AM pääsee muille osapuolille. Olet tehnyt tämän ennen sallimaan oman sovelluksen logiikkaa muodostaa yhteyden SQL Azure-AM hyvinkin. On sama asia Azure haku-yhteyden SQL Azure-AM. 

Alla olevia linkkejä antaa ohjeet NSG määritysten AM käyttöönotoissa. Käytä Käyttöoikeusluettelon Azure haun päätepisteen IP-osoitetta perusteella seuraavien ohjeiden mukaisesti.

> [AZURE.NOTE] Tausta-kohdassa [verkon käyttöoikeusryhmän ominaisuudet?](../virtual-network/virtual-networks-nsg.md)

- **Resurssienhallinta** AM Katso, [miten voit luoda ARM-versioiden NSGs](../virtual-network/virtual-networks-create-nsg-arm-pportal.md). 

- **Perinteinen** AM Katso, [miten voit luoda Classic-versioiden NSGs](../virtual-network/virtual-networks-create-nsg-classic-ps.md).

IP-osoitteiden voivat tuoda esiin muutama haasteisiin, joita ovat helposti ratkaista, jos olet tietoinen ongelmasta ja mahdollisista vaihtoehtoisista menetelmistä. Jäljellä oleva osissa suosituksia käsittely IP-osoitteet Käyttöoikeusluettelon liittyvät ongelmat.

#### <a name="restrict-access-to-the-search-service-ip-address"></a>Käytön rajoittaminen search-palvelun IP-osoite

On suositeltavaa käytön rajoittaminen tehdä SQL Azure-VMs nähtäväksi yhteyden pyynnöt vaan Käyttöoikeusluettelon oman search-palvelun IP-osoite. Voit helposti tarkistaa, IP-osoite mukaan ping täydellinen toimialuenimi (esimerkiksi `<your-search-service-name>.search.windows.net`) search-palvelun.

#### <a name="managing-ip-address-fluctuations"></a>IP-osoite vaihteluista hallinta

Jos search-palvelun on vain yksi haun yksikkö (eli replikaan ja yksi osio), IP-osoite muuttuu aikana säännöllisestä palvelu käynnistyy, poistavat aiemmin Käyttöoikeusluettelon search-palvelun IP-osoite.

Yksi tapa välttää virhe myöhemmin yhteyden on useampi kuin yksi replikan ja yksi osio käyttäminen Azure haku. Tällöin kasvaa kustannukset, mutta se ratkaisee ongelman IP-osoite. Azure hakutoiminnossa IP-osoitteet Älä muuta, kun sinulla on useampi kuin yksi haun yksikkö.

Toinen tapa on Salli epäonnistua ja määritä käyttöoikeusluettelot NSG-yhteyden. Keskimäärin odotat IP-osoitteet, voit muuttaa joitakin viikon välein. Asiakkaat, jotka Tee valvottu indeksoinnin epäsäännölliset välein tätä tapaa kannattaa ehkä elinkykyisten.

Kolmas elinkykyisten (mutta erityisen suojatun)-vaihtoehto on Määritä IP-osoitealueita Azure alueen, jossa search-palvelun valmistelun yhteydessä. IP-alueita, josta Azure resurssit jaetaan julkisten IP-osoitteiden luettelo on julkaistu [Azure palvelinkeskuksen IP-alueita](https://www.microsoft.com/download/details.aspx?id=41653). 

#### <a name="include-the-azure-search-portal-ip-addresses"></a>Sisällytä Azure haun portaalin IP-osoitteet

Jos käytössäsi on Azure portaalin luominen indeksointitoiminto Azure haun portaalin logiikan on myös access SQL Azure-AM luomisen aikana. Azure haku-portaalin IP-osoitteiden löytävät perusteella ping `stamp2.search.ext.azure.com`.

## <a name="next-steps"></a>Seuraavat vaiheet

Pois tieltä kokoonpanoa voit nyt määrittää SQL Server Azure AM Azure haun indeksointitoiminto tietolähteenä. Lisätietoja on kohdassa [Azure SQL-tietokannan yhdistäminen Azure hakutuloksia hyödyntämällä Indeksoijilla](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md) .
