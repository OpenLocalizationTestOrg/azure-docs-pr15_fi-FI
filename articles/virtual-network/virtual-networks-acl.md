<properties
   pageTitle="Mikä verkon luettelon Käyttöoikeusluettelon (Access Control)?"
   description="Lisätietoja käyttöoikeusluettelot"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="what-is-an-endpoint-access-control-list-acls"></a>Mikä on päätepisteen käyttöoikeusluettelo (käyttöoikeusluettelot)?

Päätepisteen luettelon Käyttöoikeusluettelon (Access Control) on tietoturvaa on parannettu käytettävissä Azure käyttöönottoa varten. Käyttöoikeusluettelon mahdollistaa valikoivasti Salli tai estä liikenne virtuaalikoneen päätepisteelle. Tämän paketin suodatus vain antaa suojauksen. Voit määrittää verkoston käyttöoikeusluettelot vain päätepisteiden. Et voi määrittää Käyttöoikeusluettelon virtual verkon tai tietyn aliverkon sisälsi virtual verkossa.

> [AZURE.IMPORTANT] On suositeltavaa käyttää verkon käyttöoikeusryhmät (NSGs) käyttöoikeusluettelot mahdollisuuksien sijaan. Lisätietoja NSGs on artikkelissa [verkon käyttöoikeusryhmän ominaisuudet?](virtual-networks-nsg.md).

Käyttöoikeusluettelot voi määrittää käyttämällä PowerShell tai hallinta-portaalin. Määrittämiseen verkon Käyttöoikeusluettelon PowerShell-toiminnolla kohdassa [hallinta Access ohjausobjektin (käyttöoikeusluettelot) päätepisteiden PowerShell-toiminnolla](virtual-networks-acl-powershell.md). Voivat määrittää verkon Käyttöoikeusluettelon hallinta-portaalissa, katso, [miten voit määrittää määrittäminen päätepisteet Virtual Machine](../virtual-machines/virtual-machines-windows-classic-setup-endpoints.md).

Käytä verkon käyttöoikeusluettelot, tee näin:

- Valikoivasti Salli tai estä saapuvan liikenteen remote aliverkon IPv4-osoitealueita virtuaalikoneen syötteen päätepistettä perusteella.

- Blacklist IP-osoitteet

- Useita sääntöjä virtuaalikoneen päätepisteen luominen

- Määrittää enintään 50 virtuaalikoneen päätepisteen Käyttöoikeusluettelon säännöt

- Käytä sääntöä järjestys varmistaa oikean sääntöryhmän käytetään tietyn virtuaalikoneen-päätepisteen (pienimmästä luvusta suurimpaan)

- Määritä Käyttöoikeusluettelon tietyn remote aliverkon IPv4-osoite.

## <a name="how-acls-work"></a>Käyttöoikeusluettelot maksaminen

Käyttöoikeusluettelon on objekti, joka sisältää luettelon säännöt. Kun luot Käyttöoikeusluettelon ja käyttää sitä virtuaalikoneen päätepisteen-paketin suodattaminen tapahtuu oman AM Host (isäntä)-solmun. Tämä tarkoittaa etä-IP-osoitteiden tietoliikenteen Suodatusperuste vastaavia Käyttöoikeusluettelon säännöt sen sijaan, että AM host solmu. Tämä estää oman AM tehtäviinsä arvokkaita suorittimen jaksot paketin suodattamisesta.

Virtual machine luomisen jälkeen voit estää kaiken saapuvan liikenteen sijoitetaan oletusarvoisesti Käyttöoikeusluettelon. Kuitenkin jos päätepisteen luodaan (portti 3389), valitse Käyttöoikeusluettelon on muutettava siten, että kaikki saapuva liikenne kyseisen päätepisteen oletusarvo. Saapuvan liikenteen minkä tahansa remote aliverkosta sallitaan sitten kyseisen päätepisteen ja ei ole palomuurin valmistelussa vaaditaan. Kaikki muut portit on estetty saapuvan liikenteen, ellei päätepisteet luodaan portit. Lähtevä liikenne sallitaan oletusarvoisesti.

**Esimerkki oletus Käyttöoikeusluettelon taulukko**

| **Sääntö** | **Remote aliverkon** | **Päätepisteen** | **Hyväksy/hylkää** |
|--------|---------------|----------|-------------|
| 100    | 0.0.0.0/0     | 3389     | Salli      |

## <a name="permit-and-deny"></a>Salli ja estä

Voit sallia valikoivasti tai estä verkkoliikennettä virtuaalikoneen syötteen päätepisteen luomalla sääntöjä, jotka määrittävät "Salli" tai "Estä". On tärkeää muistaa, että oletusarvoisesti päätepisteen luomisen jälkeen kaikki liikenne sallitaan päätepiste. Tästä syystä on tärkeää ymmärtää Salli tai estä sääntöjen luomisesta ja sijoittaa ne oikean järjestyksen halutessasi hajautetun päättää, sallit saavuttamiseksi virtuaalikoneen päätepisteen verkkoliikennettä.

Seikkoja huomioitavaksi tilanteisiin:

1. **Ei ole Käyttöoikeusluettelon –** Päätepisteen luomisen jälkeen on sallittava oletusarvoisesti kaikki päätepisteelle.

1. **Salli-** Kun lisäät yhden tai useamman "Salli" alueita, muut alueet ovat myönnä oletusarvoisesti. Vain sallitut IP-alueen paketit voivat pitää yhteyttä virtuaalikoneen päätepiste.

1. **Estä-** Kun lisäät yhden tai useamman "Estä" alueita, muut liikenteen alueet ovat riittää oletusarvoisesti.

1. **Yhdistelmä Salli ja estä-** Voit käyttää "Salli" ja "Estä", kun haluat tietyn IP-alueen sallittu tai estetty, carve yhdistelmän.

## <a name="rules-and-rule-precedence"></a>Säännöt ja sääntöjen käsittelyjärjestyksestä

Valitse tietyn virtuaalikoneen päätepisteet voidaan määrittää verkon käyttöoikeusluettelot. Voit esimerkiksi määrittää verkon Käyttöoikeusluettelon RDP-päätepisteen luotu mitä lukitukset alaspäin access tietyille IP-osoitteita virtual koneeseen. Alla olevassa taulukossa näkyy vielä käyttöoikeuden myöntäminen julkisen virtual IP-osoitteet (VIPs) tietyn alueen sallia access RDP. Kaikki muut remote IP-osoitteet on estetty. Olemme noudattamalla *huonoiten ohittaa* sääntöjen järjestystä.

### <a name="multiple-rules"></a>Useita sääntöjä

Seuraavassa esimerkissä, jos haluat sallia access RDP päätepisteelle vain kahden julkisen IPv4 osoitealueet (65.0.0.0/8 ja 159.0.0.0/8), voit toteuttaa tämä määrittämällä kaksi *sallia* sääntöjen. Tässä tapauksessa jälkeen RDP luodaan oletusarvoisesti virtual tietokoneeseen, voit lukita RDP portin remote aliverkon perusteella. Alla olevassa esimerkissä vielä käyttöoikeuden myöntäminen julkisen virtual IP-osoitteet (VIPs) tietyn alueen sallia access RDP. Kaikki muut remote IP-osoitteet on estetty. Tämä toimii, koska verkon käyttöoikeusluettelot voidaan määrittää tietyn virtuaalikoneen päätepisteen käyttö on estetty oletusarvon mukaan.

**Esimerkki – useita sääntöjä**

| **Sääntö** | **Remote aliverkon** | **Päätepisteen** | **Hyväksy/hylkää** |
|--------|---------------|----------|-------------|
| 100    | 65.0.0.0/8    | 3389     | Salli      |
| 200    | 159.0.0.0/8   | 3389     | Salli      |

### <a name="rule-order"></a>Sääntöjen järjestystä

Koska päätepisteen voidaan määrittää useita sääntöjä, on oltava tapaa järjestää säännöt, jotta voit selvittää, mitä sääntö on nähden. Sääntöjen järjestystä määrittää järjestys. Verkon käyttöoikeusluettelot noudattamalla *huonoiten ohittaa* sääntöjen järjestystä. Alla olevassa esimerkissä porttiin 80 päätepisteen valikoivasti myönnetään pääsy vain tietyt IP-osoitealueet. Määritä tämä on estä säännön (säännön \# 100) osoitteiden 175.1.0.1/24 tilaa. Toisen säännön määritetään sitten ohittaa 200, joka sallii pääsy kaikki muut 175.0.0.0/8-osoitteita.

**Esimerkki – sääntöjen käsittelyjärjestyksestä**

| **Sääntö** | **Remote aliverkon** | **Päätepisteen** | **Hyväksy/hylkää** |
|--------|---------------|----------|-------------|
| 100    | 175.1.0.1/24  | 80       | Estä        |
| 200    | 175.0.0.0/8   | 80       | Salli      |

## <a name="network-acls-and-load-balanced-sets"></a>Verkon käyttöoikeusluettelot ja lataa tasapainoinen joukot

Verkon käyttöoikeusluettelot voidaan määrittää kuormitus tasataan määrittäminen (kg määritetty)-päätepisteen. Jos kg-asetukseksi on määritetty Käyttöoikeusluettelon, verkon Käyttöoikeusluettelon otetaan käyttöön kaikissa näennäiskoneiden kyseisen kg joukko. Esimerkiksi jos kg määrittäminen luodaan "Portti 80" ja määrittää kg sisältää 3 VMs, verkon Käyttöoikeusluettelon luotu päätepiste "Portti 80" yhden AM koskevat automaattisesti muihin VMs.

![Verkon käyttöoikeusluettelot ja lataa tasapainoinen joukot](./media/virtual-networks-acl/IC674733.png)

## <a name="next-steps"></a>Seuraavat vaiheet

[Hallinta Access ohjausobjektin luettelot (käyttöoikeusluettelot) päätepisteiden PowerShell-toiminnolla](virtual-networks-acl-powershell.md)
