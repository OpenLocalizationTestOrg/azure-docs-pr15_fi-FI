<properties
    pageTitle="Käyttöön Internetiin yhteydessä oleva kuormituksen ratkaisun IPv6: N kanssa mallin avulla | Microsoft Azure"
    description="IPv6-tuki Azure ladata tasaustoiminto ja kuormituksen VMs ottamisesta."
    services="load-balancer"
    documentationCenter="na"
    authors="sdwheeler"
    manager="carmonm"
    editor=""
    tags="azure-resource-manager"
    keywords="IPv6: ta, azure kuormituksen, kahden pinon, julkiseen ip, alkuperäisen ipv6, mobile, iot"
/>
<tags
    ms.service="load-balancer"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="09/14/2016"
    ms.author="sewhee"
/>

# <a name="deploy-an-internet-facing-load-balancer-solution-with-ipv6-using-a-template"></a>Käyttöönotto Internetiin yhteydessä oleva kuormituksen ratkaisussa, jossa IPv6 mallin avulla

> [AZURE.SELECTOR]
- [PowerShellin](./load-balancer-ipv6-internet-ps.md)
- [Azure CLI](./load-balancer-ipv6-internet-cli.md)
- [Malli](./load-balancer-ipv6-internet-template.md)

Azure kuormituksen on kerros-4 (TCP- ja UDP) kuormituksen. Kuormituksen on suuri käytettävyys jakamalla saapuvan liikenteen kunnossa palveluesiintymiä cloud Services-palveluissa tai näennäiskoneiden kuormituksen tasauspalvelun joukon. Azure kuormituksen myös näyttää useita portit, useita IP-osoitteita tai molempia näistä palveluista.

## <a name="example-deployment-scenario"></a>Käyttöönotto-Esimerkkitapaus

Seuraavassa kaaviossa on kuvattu ratkaisu kuormituksen otetaan käyttöön, joka on kuvattu tämän artikkelin Esimerkki mallin avulla.

![Kuormituksen tasauspalvelun-skenaario](./media/load-balancer-ipv6-internet-template/lb-ipv6-scenario.png)

Tässä skenaariossa luot Azure seuraavissa resursseissa:

- VPN-liittymän, kunkin AM määritetty IPv4- ja IPv6-osoitteet
- Internetiin yhteydessä oleva-kuormituksen IPv4 ja IPv6 julkiseen IP-osoite
- kaksi ladata julkisen VIPs yhdistäminen yksityinen päätepisteet Vastatilin säännöt
- Käytettävyys-määrittäminen, joka sisältää kaksi VMs
- kaksi näennäiskoneiden (VMs)

## <a name="deploying-the-template-using-the-azure-portal"></a>Azure-portaalissa mallin käyttöönotto

Tässä artikkelissa viittaa malliin, jotka on julkaistu [Azure pikaopas](https://azure.microsoft.com/documentation/templates/201-load-balancer-ipv6-create/) mallivalikoimasta. Voit ladata malli valikoimasta tai Käynnistä käyttöönoton Azure-tietokannassa suoraan valikoimasta. Tässä artikkelissa oletetaan, että olet ladannut mallin paikalliseen tietokoneeseen.

1. Avaa Azure portal ja kirjaudu sisään tilille, jolla on oikeus luoda VMs ja Azure-tilauksen piiriin kuuluvien verkko resurssit. Lisäksi paitsi, jos käytät aiemmin resursseja, tili on oikeus luoda resurssiryhmä ja tallennustilaa tilin.

2. Valitse "+ uusi"-valikosta sitten tyyppi-malli-hakuruutuun. Valitse "Mallin käyttöönottoa" hakutuloksista.

    ![g-ipv6-portal-vaiheen2 ohjeiden mukaisesti](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step2.png)

3. Onko kaikki-sivu, valitse "Mallin käyttöönottoa."

    ![g-ipv6-portal-vaihe 3](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step3.png)

4. Valitse "Luo".

    ![g-ipv6-portal-step4](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step4.png)

5. "Muokkaa mallia." Poista olemassa olevaa sisältöä- ja kopioi ja liitä-mallitiedoston (Jos haluat sisällyttää alkamis- ja loppumiskohdat {}) koko sisällön ja valitse sitten "Tallenna."

    > [AZURE.NOTE] Jos käytössäsi on Microsoft Internet Explorer, kun liität sinun tulee valintaikkuna, jossa kysytään annettavien Windowsin Leikepöydälle. Valitse "Salli Accessin."

    ![g-ipv6-portal-step5](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step5.png)

6. Valitse "Muokkaa parametreja." Parametrit-sivu, valitse Määritä arvot kohti ohjeet mallin parametrit-osassa ja valitse sitten "Tallenna" Sulje parametrit-sivu. Valitse mukautettu käyttöönottoa sivu-tilauksen aiemmin resurssiryhmä tai luoda. Jos olet luomassa resurssiryhmä, valitse resurssiryhmän sijainti. Seuraavaksi **juridiset**ehdot ja valitse sitten **Osta** koskevat ehdot. Azure alkaa käyttöönotto resurssit. Kestää useita minuutteja ottamaan kaikki resurssit.

    ![g-ipv6-portal-step6](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step6.png)

    Lisätietoja näiden parametrit-kohdassa [Malliparametrit ja muuttujat](#template-parameters-and-variables) tämän artikkelin.

7. Haluat nähdä resurssit luoma mallia, valitse Selaa, Vieritä luetteloa alaspäin, kunnes "resurssiryhmät-kohdassa ja valitse sitten se.

    ![g-ipv6-portal-step7](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step7.png)

8. Valitse resurssi ryhmät-sivu vaiheessa 6 määritetyn resurssiryhmän nimi. Näet luettelon kaikista resursseista, jotka on otettu käyttöön. Jos kaikki tapahtui hyvin, pitäisi näkyä versionumeron "onnistui-kohdassa"Edellisen käyttöönoton." Jos näin ei ole, varmista, että käytät tilillä on oikeus luoda tarvittavat resurssit.

    ![g-ipv6-portal-step8](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step8.png)

    > [AZURE.NOTE] Jos siirryt resurssiryhmien heti, kun olet suorittanut vaiheessa 6, "Edellisen käyttöönoton" Näytä "Käyttöönotto" tila, kun resursseja on otettu käyttöön.

9. Valitse "myIPv6PublicIP" resurssien luetteloa. Näet, että se on IPv6-osoitteen IP-osoite ja DNS-nimessä on vaiheessa 6 dnsNameforIPv6LbIP-parametrille määritetty arvo. Resurssi on julkinen IPv6-osoite ja isännän nimi, joita voit käyttää Internet-asiakkaille.

    ![g-ipv6-portal-step9](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step9.png)

## <a name="validate-connectivity"></a>Tarkista yhteys

Kun malli on otettu käyttöön onnistuneesti, voit tarkistaa yhteys suorittamalla seuraavat toimet:

1. Kirjaudu Azure-portaaliin ja Yhdistä eri mallin käyttöönottoa luoma VMs. Jos olet asentanut Windows Server-AM, Suorita ipconfig/kaikki komentoriviltä. Näet, että VMs IPv4- ja IPv6-osoitteet. Linux VMs on otettu käyttöön, jos haluat määrittää Linux OS vastaanottamaan dynaaminen IPv6-osoitteet käyttämällä Linux-jakauman annettuja ohjeita.
2. IPv6-Internet-yhteys-asiakasohjelma ja aloittaa kuormituksen julkisen IPv6 address yhteyttä. Vahvistamaan, että kuormituksen tasaamisen kaksi VMs välillä voi asentaa verkkopalvelimeen, kuten Microsoft Internet Information Services (IIS) kunkin VMs. Kussakin palvelimessa oletusarvoinen web-sivun voi sisältää tekstin "Server0" tai "Palvelin1" ainutlaatuisen. Avaa Internet-selaimen IPv6 Internet-yhteys asiakkaan ja siirry isäntänimi Vahvista lopusta loppuun IPv6-yhteyden avulla kunkin AM kuormituksen dnsNameforIPv6LbIP-parametrille määritetty. Jos näet vain verkkosivun vain yksi palvelimesta, joudut ehkä Tyhjennä selaimen välimuisti. Avaa useita yksityisen selausistunnon. Raportissa pitäisi näkyä kunkin palvelimen vastausta.
3. IPv4-Internet-yhteys-asiakasohjelma ja aloittaa yhteyden julkinen IPv4-osoite, kuormituksen. Vahvistamaan, että kuormituksen on kaksi VMs kuormituksen voi testata käyttämällä IIS-vaiheessa 2 esitetyllä tavalla.
4. Kunkin AM-aloittaa lähtevä yhteys, IPv6: ta tai IPv4-yhteys Internet-laitteeseen. Kummassakin tapauksessa tarkastella kohde-laitteen lähde-IP on kuormituksen julkisen IPv4 tai IPv6-osoite.

>[AZURE.NOTE]
ICMP IPv4 ja IPv6 on estetty Azure verkkoon. Tämän vuoksi ICMP työkaluja, kuten ping aina onnistu. Testaa yhteys käytetään TCP-vaihtoehto, kuten TCPing tai PowerShell testi-NetConnection cmdlet-komento. Huomaa, että IP-osoitteet, kaavion arvoja, joita saatat nähdä esimerkkejä. Koska IPv6-osoitteet määritetään dynaamisesti, näyttöön tulee osoitteet vaihtelevat ja voivat vaihdella alueittain. On myös yleisiä kuormituksen kuin yksityinen IPv6-osoitteet taustatietokantaan varannon etuliitteellä Aloita julkisen IPv6-osoitteen.

## <a name="template-parameters-and-variables"></a>Malliparametrit ja muuttujat

Azure Resurssienhallinta-malli sisältää useita muuttujia ja parametreja, jota voi mukauttaa tarpeen mukaan. Muuttujat käytettäviä kiinteitä arvoja, joita et halua käyttäjä voi muuttaa. Parametreja käytetään arvojen näkyvän, kun otat mallin käyttäjä. Esimerkki malli on määritetty tässä artikkelissa kuvattuja skenaarion. Voit mukauttaa ympäristön tarpeisiin.

Tässä artikkelissa käytetyt Esimerkki malli sisältää seuraavat muuttujat ja parametrit:

| Parametrin / muuttujan | Huomautuksia |
|-----------|-------|
| adminUsername | Määritä kirjautumaan näennäiskoneiden kanssa käytettävä järjestelmänvalvojan tilin nimi. |
| adminPassword | Määritä jolla kirjaudutaan sisään näennäiskoneiden järjestelmänvalvojatilin salasanan. |
| dnsNameforIPv4LbIP | Määritä DNS-isäntänimi haluat määrittää kuormituksen julkisen nimenä. Tätä nimeä ratkaisee kuormituksen julkinen IPv4-osoite. Nimi on kirjoitettava pienellä ja käytä regex: ^ [a-z][a-z0-9-]{1,61}[a-z0-9]$. |
| dnsNameforIPv6LbIP | Määritä DNS-isäntänimi haluat määrittää kuormituksen julkisen nimenä. Tätä nimeä ratkaisee kuormituksen julkisen IPv6-osoite. Nimi on kirjoitettava pienellä ja käytä regex: ^ [a-z][a-z0-9-]{1,61}[a-z0-9]$. Tämä voi olla sama nimi kuin IPv4-osoite. Kun asiakas lähettää saman niminen DNS kysely palauttaa Azure A- ja AAAA tallentaa kun nimi on jaettu. |
| vmNamePrefix | Määritä AM etuliite. Mallin Lisää numero (0, 1, jne.) nimen VMs luotaessa. |
| nicNamePrefix | Määrittää verkko-liittymän etuliite. Mallin Lisää numero (0, 1, jne.) nimeä verkkoliittymät luotaessa. |
| storageAccountName | Käytössä olevan tallennustilan tilin nimi tai uuden luominen mallin nimi. |
| availabilitySetName | Kirjoita sitten nimi asettaminen käytettäväksi VMs käytettävyydestä |
| addressPrefix | Käytettävä osoite arvojoukon Virtual Network osoitteen etuliite |
| subnetName | Aliverkon nimi luodaan VNet |
| subnetPrefix | Käytettävä osoite arvojoukon aliverkon osoite etuliite |
| vnetName | Määritä VMs käyttämä VNet nimi. |
| ipv4PrivateIPAddressType | Kohdistusmenetelmä, jolla yksityinen IP-osoite (staattinen tai dynaaminen) |
| ipv6PrivateIPAddressType | Kohdistus-tapa, jolla yksityinen IP-osoite (dynaaminen). IPv6 tukee vain dynaaminen kohdistus. |
| numberOfInstances | Mallin käyttöön kuormitus tasataan esiintymien määrän |
| ipv4PublicIPAddressName | Määritä julkinen IPv4-osoite kuormituksen yhteydessä käytettävä DNS-nimi. |
| ipv4PublicIPAddressType | Käytetyn julkiseen IP-osoite (staattinen tai dynaaminen) |
| Ipv6PublicIPAddressName | Määritä julkinen IPv6-osoite kuormituksen yhteydessä käytettävä DNS-nimi. |
| ipv6PublicIPAddressType | Kohdistus-tapa, jolla julkiseen IP-osoite (dynaaminen). IPv6 tukee vain dynaaminen kohdistus. |
| lbName | Määritä kuormituksen nimi. Tämä nimi näkyy portaalissa tai käytetään, kun siihen viittaava CLI tai PowerShell-komennolla. |

Jäljellä oleva muuttujat mallin sisältää arvot, jotka on määritetty, kun Azure Luo resursseja. Nämä muuttujat eivät muutu.
