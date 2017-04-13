<properties
    pageTitle="Esimerkki infrastruktuurin ongelmatilanteita | Microsoft Azure"
    description="Lisätietoja keskeisiä suunnittelu ja käyttöönotto ohjeita käyttöönoton Esimerkki infrastruktuuri Azure-tietokannassa."
    documentationCenter=""
    services="virtual-machines-windows"
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="iainfou"/>

# <a name="example-azure-infrastructure-walkthrough"></a>Esimerkki Azure infrastruktuurin ongelmatilanteita

[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)] 

Tässä artikkelissa käydään läpi rakennuksen Esimerkki sovelluksen infrastruktuuri ulos. Kerromme yksityiskohtaisesti infrastruktuuri yksinkertainen online-kaupan, joka yhdistää asioiden ohjeita ja päätösten ympärille nimeämiskäytännöt, käytettävyys joukot, virtual verkkojen ja kuormituksen tasoitusmääritykset suunnittelu ja käyttöönotto todella näennäiskoneiden (VMs).


## <a name="example-workload"></a>Esimerkki työmäärää

Adventure Worksin jaksot haluaa luomaan online store-sovelluksen Azure-tietokannassa, joka koostuu:

- Kaksi edusta-WWW-tason asiakasohjelmaa IIS-palvelimiin.
- Kaksi käsittelyn tiedot ja tilaukset-sovelluksen taso IIS-palvelimiin.
- Kaksi Microsoft SQL Server-esiintymää ryhmiin AlwaysOn käytettävyys (kaksi SQL-palvelimet ja suurin-solmu hallitun) tallennetaan tuotetietojen ja tilaukset tietokannan taso
- Kahden Active Directory-toimialueen ohjauskoneen, asiakas- ja käyttöoikeuksien taso toimittajat
- Jokaisessa palvelimessa on ohjeaiheessa kahden aliverkon:
    - edusta-WWW-palvelimien aliverkon 
    - taustatietokannan aliverkon sovelluksen palvelimissa, SQL-klusterin ja toimialueen ohjaimet

![Kaavio eri tasojen sovelluksen infrastruktuurin](./media/virtual-machines-common-infrastructure-service-guidelines/example-tiers.png)

Saapuvan postin suojatun web-liikenne on kuormituksen-tasattava web-palvelinten välillä, kun asiakkaat selata online-kauppaa. Tilauksen käsitellään liikenteen HTTP-lomakkeessa pyytää palvelinten on tasattava sovelluksen palvelinten välillä verkosta. Lisäksi infrastruktuuri on suunniteltava suuren.

Tuloksena suunnitteluun kuuluvat:

- Azure-tilauksen ja tilin
- Yksittäisen resurssiryhmä
- Tallennustilan tilit
- Kahden aliverkon virtual verkosta
- Käytettävyys määrittää samalla roolissa VMs
- Näennäiskoneiden

Kaikki edellä mainitut noudattamalla näitä nimeämiskäytännöt:

- Adventure Worksin jaksot käyttää **[IT työmäärää]-[sijainti]-[Azure resurssi]** etuliitteenä
    - Tässä esimerkissä "**azos**" (Azure online-kauppa) on IT työmäärää nimi ja sijainti on "**Käytä**" (Itä USA 2)
- Tallennustilan tilit käyttävät adventureazosusesa**[kuvaus]**
    - 'adventure' on lisätty antamaan yksilöllisyyden etuliite ja tallennustilan tilien nimet eivät tue yhdysmerkit käyttö.
- Virtuaalinen verkot käyttävät AZOS-käyttö-VN**[määrä]**
- Käytettävyys joukot käyttää azos-käyttäminen-muodossa-**[roolin]**
- Virtuaalikoneen nimissä käytetään azos-käyttäminen-AM -**[vmname]**


## <a name="azure-subscriptions-and-accounts"></a>Azure tilaukset ja -tilien

Adventure Worksin jaksot on käytössä yrityksen tilauksen-niminen Enterprise Adventure Works-tilaukseen antamaan IT-kuormituksen laskutus.


## <a name="storage-accounts"></a>Tallennustilan tilit

Adventure Worksin jaksot määritetään, että ne tarvitaan kaksi tallennustilan tiliä:

- **adventureazosusesawebapp** WWW-palvelimien, sovelluksen palvelimia ja toimialueen ohjaimet ja niiden tiedot levyjen vakio tallennustilaan.
- **adventureazosusesasql** SQL Server-VMs ja niiden tiedot levyjen Premium tallennustilaan.


## <a name="virtual-network-and-subnets"></a>VPN- ja aliverkosta

Virtuaalinen verkon ei tarvitse jatkuvaa connectivity Adventure työn jaksot paikalliseen verkkoon, koska ne päättänyt vain pilvipalveluita virtual verkossa.

Luomiaan vain pilvipalveluita virtual verkon Azure-portaalissa seuraavat asetukset:

- Nimi: AZOS-käyttö-VN01
- Sijainti: Itä USA 2
- VPN-osoitetilaa: 10.0.0.0/8
- Ensimmäinen aliverkon:
    - Nimi: FrontEnd
    - Tilaa osoite: 10.0.1.0/24
- Toinen aliverkon:
    - Nimi: taustaan
    - Tilaa osoite: 10.0.2.0/24


## <a name="availability-sets"></a>Käytettävyys joukot

Jos haluat säilyttää kaikki neljä tasoa, niiden online säilön suuren käytettävyyden, Adventure Works jaksot päättää neljän käytettävyys eri:

- web-palvelinten **azos-käyttö--web-sivuna**
- sovelluksen palvelimia **azos-käyttö-muodossa-sovellus**
- **azos-käyttö-muodossa-sql** -SQL-palvelimien
- **azos-käyttö-muodossa-ohjauskoneen** toimialueen ohjaimet


## <a name="virtual-machines"></a>Näennäiskoneiden

Adventure Worksin jaksot päättää niiden Azure VMs seuraavat nimet:

- **azos-käyttö-AM-web01** ensimmäisen palvelimeen
- **azos-käyttö-AM-web02** toisessa WWW-palvelin
- ensimmäisen sovelluksen palvelimen **azos-käyttö-AM-app01**
- toisen sovelluksen palvelimen **azos-käyttö-AM-app02**
- ensimmäinen klusterin SQL Server-palvelimen **azos-käyttö-AM-sql01**
- toisen klusterin SQL Server-palvelimen **azos-käyttö-AM-sql02**
- ensimmäinen toimialueen ohjauskoneen **azos-käyttö-AM-dc01**
- toisen toimialueen ohjauskoneen **azos-käyttö-AM-dc02**

Seuraavassa on tuloksena määritykset.

![Lopullinen sovelluksen infrastruktuurin käyttöön Azure-tietokannassa](./media/virtual-machines-common-infrastructure-service-guidelines/example-config.png)

Tämä määritys sisältyy:

- Kahden aliverkon (edusta- ja Taustajärjestelmä) vain pilvipalveluita virtual verkosta
- Kaksi tallennustilan tilit
- Neljän käytettävyys eri, yhteen kunkin tason online-kaupasta
- Neljä tasoa, näennäiskoneiden
- Ulkoinen kuormituksen saapuva HTTPS-pohjainen web-liikenteen Internetistä WWW-palvelimien määrittäminen
- Sisäinen kuormituksen saapuva sovelluksen palvelimiin WWW-palvelimien salaamaton web tietoliikenteen määrittäminen
- Yksittäisen resurssiryhmä


## <a name="next-steps"></a>Seuraavat vaiheet

[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)] 