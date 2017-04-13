<properties
    pageTitle="Yksityiskohtaiset SSH vianmääritys Azure-AM | Microsoft Azure"
    description="Yksityiskohtaisempia SSH vianmäärityksen Azure virtuaalikoneen ongelmista vaiheet"
    keywords="ssh yhteyden hylätty, ssh virhe, azure ssh, SSH yhteys epäonnistui"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="top-support-issue,azure-service-management,azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="support-article"
    ms.date="09/01/2016"
    ms.author="iainfou"/>

# <a name="detailed-ssh-troubleshooting-steps"></a>Yksityiskohtaisia SSH vianmääritysohjeita

Tähän on useita mahdollisia syitä, SSH asiakkaan välttämättä voi aina saavuttaa AM SSH-palvelun. Jos olet toiminut useita [yleisiä SSH vianmääritysohjeita](virtual-machines-linux-troubleshoot-ssh-connection.md), sinun on edelleen vianmääritys yhteysongelmat. Tässä artikkelissa opastaa yksityiskohtaiset vianmääritysvaiheiden avulla voit selvittää, missä SSH yhteys epäonnistui tuntemattomasta ja miten voit ratkaista ongelman.

## <a name="take-preliminary-steps"></a>Alustava korjausohjeet

Seuraavassa kaaviossa näkyvät osat, jotka ovat mukana.

![Kaaviossa näkyy SSH palvelun osat](./media/virtual-machines-linux-detailed-troubleshoot-ssh-connection/ssh-tshoot1.png)

Seuraavien ohjeiden avulla, eristää virheen lähteen ja niiden ratkaisumenetelmiä voi selvittää.

Tarkista ensin, tila portaalissa AM.

[Azure portaalissa](https://portal.azure.com):

1. Perinteinen käyttöönotto-mallin avulla luotu analyysinäkymä VMs, valitse **Selaa** > **näennäiskoneiden (perinteinen)** > *AM nimi*.

    -TAI –

    Resurssienhallinta-mallin avulla luotu analyysinäkymä VMs, valitse **Selaa** > **näennäiskoneiden** > *AM nimi*.

    Tila-ruutua AM näytetään **käytössä**. Vieritä Näytä viimeisimmät tehtävän suorittaminen, varasto ja verkkoresursseja.

2. Valitse **asetukset** tutkia päätepisteet, IP-osoitteet ja muut asetukset.

    Laji päätepisteet VMs, jotka on luotu käyttämällä Resurssienhallinta, varmista [verkon käyttöoikeusryhmän](../virtual-network/virtual-networks-nsg.md) on määritetty. Myös varmistaa, että sääntöjä on käytetty verkko-käyttöoikeusryhmän ja että ne on viitattu aliverkon.

[Azure perinteinen portal](https://manage.windowsazure.com)VMs, joka luotiin perinteinen käyttöönotto-mallin avulla:

1. Valitse **näennäiskoneiden** > *AM nimi*.
2. Valitse sen tilan tarkistaminen AM **raporttinäkymät-ikkunan** .
3. Valitse **näytön** saat näkyviin uusimmat tehtävän suorittaminen, varasto ja verkkoresursseja.
4. Valitse **päätepisteet** varmistaa, että päätepisteen SSH tietoliikenteen.

Tarkista verkkoyhteys, tarkista määritetyn päätepisteet ja katso, jos olet laskentataulukon AM jokin muu protokolla, kuten HTTP tai toisen palvelun kautta.

Yritä SSH yhteyttä uudelleen nämä vaiheet.


## <a name="find-the-source-of-the-issue"></a>Ongelman aiheuttaja etsiminen

SSH asiakkaan tietokoneessa voi epäonnistua saavuttamiseksi vuoksi ongelmia tai onko seuraavan Azure-AM SSH-palvelu:

- [SSH asiakastietokone](#source-1-ssh-client-computer)
- [Organisaation reuna-laite](#source-2-organization-edge-device)
- [Cloud palvelupäätepiste ja käyttää hallinta-luettelossa (Käyttöoikeusluettelon)](#source-3-cloud-service-endpoint-and-acl)
- [Verkon käyttöoikeusryhmät](#source-4-network-security-groups)
- [Linux-pohjaiset Azure AM](#source-5-linux-based-azure-virtual-machine)

## <a name="source-1-ssh-client-computer"></a>Lähteen 1: SSH asiakastietokone

Tarkista poistamiseksi tietokoneen virheen lähteen, voit tehdä SSH yhteydet paikalliseen toiseen, Linux-tietokoneella.

![Kaavio, joka korostaa SSH asiakkaan tietokoneen osat](./media/virtual-machines-linux-detailed-troubleshoot-ssh-connection/ssh-tshoot2.png)

Jos yhteys epäonnistuu, Tarkista tietokoneen seuraavasti:

- Paikallinen palomuuri-asetus, joka estää saapuvan ja lähtevän liikenteen SSH liikenne (TCP-22)
- Paikallisesti asennettu välityspalvelimen Asiakasohjelmistoa, joka estää SSH yhteydet
- Paikallisesti asennettu verkon seuranta ohjelmiston, joka estää SSH yhteydet
- Muuntyyppisten suojausohjelmisto toisella, joka liikennettä tai Salli/disallow tietyntyyppiset liikenne

Jos jokin seuraavista ehdoista täyttyy, tilapäisesti käytöstä ohjelmiston ja yritä SSH yhteys paikalliseen tietokoneeseen nähdäksesi, yhteys estetään tietokoneen syy. Toimi sitten verkonvalvoja korjaa Ohjelmistoasetukset sallimaan SSH yhteyksiä.

Jos käytössäsi on todennus, varmista, että näiden oikeuksien .ssh-kansioon-kotihakemistoosi:

- Chmod 700 ~/.ssh
- Chmod 644 ~/.ssh/\*.pub
- Chmod 600 ~/.ssh/id_rsa (tai muita tiedostoja, jotka on tallennettu ne yksityiset avaimet)
- Chmod 644 ~/.ssh/known_hosts (sisältää isännät, jotka olet aiemmin muodostanut yhteyden SSH kautta)

## <a name="source-2-organization-edge-device"></a>Lähteen 2: Organisaation reuna-laite

Poistamiseksi organisaation reunan laitteen virheen lähteen, tarkista, että tietokone, joka on suoraan yhteydessä Internetiin tehdä Azure AM SSH yhteydet. Jos käytät käyttöoikeuspalvelinta AM sivusto sivusto VPN tai Azure ExpressRoute-yhteyden kautta, siirry [lähde 4: verkon käyttöoikeusryhmät](#nsg).

![Kaavio, joka korostaa organisaation reuna-laite](./media/virtual-machines-linux-detailed-troubleshoot-ssh-connection/ssh-tshoot3.png)

Jos sinulla ei ole tietokoneeseen, jossa on suoraan yhteydessä Internetiin, Luo uusi Azure-AM oma resurssiryhmä tai cloud palvelun ja käyttää sitä. Lisätietoja on artikkelissa [Create virtual machine käynnissä Linux Azure-tietokannassa](virtual-machines-linux-quick-create-cli.md). Poista resurssiryhmä tai AM ja cloud-palvelu, kun enää tarvitse oman testaaminen.

Jos voit luoda SSH yhteyden tietokoneeseen, jossa on suoraan yhteydessä Internetiin, tarkista organisaation reunan laitteen:

- Sisäinen palomuuri, joka estää Internet-liikenteen SSH
- Välityspalvelin, joka estää SSH yhteydet
- Tunkeutumisen tunnistamisen tai verkon seuranta sovelluksen käytössä, mikä verkkoa reunan, joka estää SSH yhteydet

Käsitellä verkonvalvoja korjaa organisaation reunan laitteet asetuksia sallimaan SSH liikenne yhteydessä Internetiin.

## <a name="source-3-cloud-service-endpoint-and-acl"></a>Lähteen 3: Cloud palvelupäätepiste ja Käyttöoikeusluettelon

> [AZURE.NOTE] Tämä lähde koskee vain VMs, jotka on luotu käyttämällä perinteinen käyttöönottomalli. Voit ohittaa VMs, jotka on luotu käyttämällä Resurssienhallinta, [tietolähteen 4: verkon käyttöoikeusryhmät](#nsg).

Poistamiseksi cloud palvelupäätepiste ja Käyttöoikeusluettelon virheen lähteenä Varmista, että toiseen samassa virtual verkostossa Azure AM tehdä oman AM SSH yhteydet.

![Kaavio, joka korostaa cloud palvelupäätepiste ja Käyttöoikeusluettelon](./media/virtual-machines-linux-detailed-troubleshoot-ssh-connection/ssh-tshoot4.png)

Jos sinulla ei ole toisen AM virtual samassa verkostossa, voit helposti luoda uuden. Lisätietoja on artikkelissa [luominen Linux AM, valitse Azure CLI avulla](virtual-machines-linux-quick-create-cli.md). Poista ylimääräiset AM, kun olet valmis ja että testaaminen.

Jos voit luoda SSH yhteyden samassa virtual verkostossa AM, tarkista seuraavat asiat:

- **Päätepisteen määrityksen SSH tietoliikenteen aikataulussa AM.** Yksityinen porttinumeroa päätepisteen vastattava porttinumeroa, jossa seuraa AM SSH-palvelun. (Oletusportti on 22). Tarkista VMs luotu resurssien hallinnan käyttöönottomalli, SSH TCP-porttinumero Azure-portaalissa valitsemalla **Selaa** > **näennäiskoneiden (v2)** > *AM nimi* > **asetukset** > **päätepisteet**.

- **Kohteen virtuaalikoneen SSH liikenne-päätepisteelle Käyttöoikeusluettelon.** Käyttöoikeusluettelon avulla voit määrittää sallia tai estää saapuvan liikenteen Internetistä perusteella sen lähde-IP-osoite. Väärä käyttöoikeusluettelot estää SSH saapuvan liikenteen päätepisteelle. Tarkista, että käyttöoikeusluettelot varmistaa, että saapuvan liikenteen välityspalvelimen julkiseen IP-osoitteista tai muita edge Serverin on sallittu. Lisätietoja on artikkelissa [tietoja verkkokäyttö ohjausobjektin luettelot (käyttöoikeusluettelot)](../virtual-network/virtual-networks-acl.md).

Päätepisteen poistaminen lähteenä ongelman, poista nykyinen päätepiste, Luo uusi päätepiste ja SSH nimi (TCP-portti 22 julkisista ja yksityisistä porttinumero). Lisätietoja on artikkelissa [määrittäminen päätepisteet virtual tietokoneeseen Azure-tietokannassa](virtual-machines-windows-classic-setup-endpoints.md).

<a id="nsg"></a>
## <a name="source-4-network-security-groups"></a>Lähteen 4: Verkon käyttöoikeusryhmät

Verkon käyttöoikeusryhmät mahdollistavat on sallittu saapuva ja lähtevä liikenne eritellympiä oikeudet. Voit luoda sääntöjä, jotka ulottuvat aliverkosta ja cloud services Azure virtual verkossa. Tarkista verkon suojauksen ryhmän säännöt varmistaa, että SSH liikenne ja sieltä pois Internet on sallittua.
Lisätietoja on artikkelissa [verkon käyttöoikeusryhmät tietoja](../virtual-network/virtual-networks-nsg.md).

## <a name="source-5-linux-based-azure-virtual-machine"></a>Lähteen 5: Linux-pohjaiset Azure virtual-tietokone

Mahdollisia ongelmia viimeisen lähde on Azure virtuaalikoneen itse.

![Kaavio, joka korostaa Linux-pohjaiset Azure virtuaalikoneen](./media/virtual-machines-linux-detailed-troubleshoot-ssh-connection/ssh-tshoot5.png)

Jos et ole jo tehnyt niin, noudata ohjeita [vaihtamaan salasanan tai Linux-pohjaiset näennäiskoneiden SSH](virtual-machines-linux-classic-reset-access.md).

Yritä yhdistää tietokoneesta uudelleen. Jos epäonnistuu uudelleen, seuraavassa on joitakin mahdolliset ongelmat:

- SSH-palvelu ei ole käynnissä kohde virtuaalikoneen.
- SSH-palvelu ei ole Kuuntele porttinumeroa 22. Voit testata tämän telnet-asiakasohjelman asentaminen paikalliseen tietokoneeseen ja suorittaa "telnet *cloudServiceName*. cloudapp.net 22". Määrittää, jos virtuaalikoneen sallii saapuvan ja lähtevän tietoliikenteen SSH päätepisteelle.
- Kohteen virtuaalikoneen paikallinen palomuuri on säännöt, jotka estävät saapuvan ja lähtevän tietoliikenteen SSH.
- Tunkeutumisen tunnistamisen tai verkon seuranta ohjelmiston, jossa on käytössä Azure virtuaalikoneen estää SSH yhteydet.


## <a name="additional-resources"></a>Lisäresursseja
Saat lisätietoja sovellusten käyttö [vianmääritys access-sovellukseen Azure virtuaalikoneen käytössä](virtual-machines-linux-troubleshoot-app-connection.md)