<properties
   pageTitle="Luo sovelluksen yhdyskäytävän portaalissa | Microsoft Azure"
   description="Opi luomaan sovelluksen yhdyskäytävän mukaan-portaalissa"
   services="application-gateway"
   documentationCenter="na"
   authors="georgewallace"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="gwallace" />

# <a name="create-an-application-gateway-by-using-the-portal"></a>Luo sovelluksen yhdyskäytävän-portaalissa

> [AZURE.SELECTOR]
- [Azure portal](application-gateway-create-gateway-portal.md)
- [Azure Resurssienhallinta PowerShell](application-gateway-create-gateway-arm.md)
- [Azure perinteinen PowerShell](application-gateway-create-gateway.md)
- [Azure Resurssienhallinta-malli](application-gateway-create-gateway-arm-template.md)
- [Azure CLI](application-gateway-create-gateway-cli.md)

Azure sovelluksen yhdyskäytävä on kerroksen 7 kuormituksen. Se sisältää automaattisesti, suorituskyvyn reititys-pyyntöjen eri palvelinten välillä, olivatpa ne sitten cloud tai paikallisen. Sovelluksen yhdyskäytävän sisältää useita sovelluksen toimituksen ohjauskoneen (ADC) mukaan lukien HTTP kuormituksen tasaamisen, eväste perustuva istunnon affiniteetti, Secure Sockets Layer (SSL) purku mukautetun kunto keräysputkien usean sivuston tuki ja monista muista. Luettelo kaikista tuetut ominaisuudet etsimällä käy [Sovelluksen Gateway yleiskatsaus](application-gateway-introduction.md)

## <a name="scenario"></a>Skenaario

Tässä skenaariossa voit Opi luomaan sovelluksen yhdyskäytävän Azure-portaalissa.

Tässä skenaariossa on:

- Luo kaksi esiintymää Normaali sovelluksen yhdyskäytävä.
- Luo virtuaalisia verkko nimeltä AdatumAppGatewayVNET varattu CIDR tekstialueen 10.0.0.0/16 kanssa.
- Luo nimi, joka käyttää 10.0.0.0/28 sen CIDR palkkina Appgatewaysubnet aliverkon.
- Määritä SSL purku varmennetta.

![Skenaario-Esimerkki][scenario]

>[AZURE.IMPORTANT] Sovelluksen yhdyskäytävän, mukaan lukien mukautetun kunto lisämäärityksiä probes, Taustajärjestelmä resurssivarantoon osoitteet ja lisäsääntöjä määritetään sen jälkeen, kun sovellus yhdyskäytävä on määritetty ja ei ensimmäisen käyttöönoton yhteydessä.

## <a name="before-you-begin"></a>Ennen aloittamista

Azure sovelluksen yhdyskäytävä on oltava oma aliverkon. Virtuaalinen verkoston luominen varmistaa, että jätät on useita aliverkosta tarpeeksi osoitetilaa. Kun sovellus-yhdyskäytävän aliverkon otetaan käyttöön, vain Lisää sovellus yhdyskäytävien voivat aliverkon lisättäväksi.

## <a name="create-the-application-gateway"></a>Sovelluksen yhdyskäytävän luominen

### <a name="step-1"></a>Vaihe 1

Siirry Azure-portaaliin, valitse **Uusi** > **Verkko** > **Sovelluksen yhdyskäytävän**

![Sovelluksen yhdyskäytävän luominen][1]

### <a name="step-2"></a>Vaihe 2

Täytä seuraava sovelluksen yhdyskäytävän perustietoja. Kun olet valmis, valitse **OK**

Perusasetuksia tarvittavat tiedot:

- **Nimi** - sovelluksen yhdyskäytävän nimi.
- **Taso** - tämä on sovelluksen yhdyskäytävän taso. Kaksi tasoa on saatavilla, **WAF** ja **Vakio**. WAF mahdollistaa web-sovelluksen palomuuri.
- **Tuote kokoa** - asetusta on haluamasi kokoinen sovelluksen yhdyskäytävän, käytettävissä olevat vaihtoehdot (**Pieni**, **Normaali**ja **suuri**). *Pieni ei ole käytettävissä, kun WAF taso on valittu*
- **Esiintymän Laske** - esiintymien määrän, tämä arvo on oltava luku 2 – 10.
- **Resurssiryhmä** - sovelluksen yhdyskäytävää, pidä resurssiryhmän voi olla aiemmin resurssiryhmä tai uusi tunnus.
- **Sijainti** - sovelluksen yhdyskäytävää, alue on osoitteessa resurssiryhmän samaan sijaintiin. *Sijainti on tärkeää kuin virtual verkon ja julkiseen IP on oltava samassa sijainnissa kuin yhdyskäytävän*.

![sivu näkyy perusasetukset][2]

>[AZURE.NOTE] 1 esiintymä-määrä on valittava testausta varten. On tärkeää tietää, minkä tahansa esiintymän määrä-kohdassa kaksi esiintymää ei koske SLA ja näin ollen ei suositella. Pieni yhdyskäytävät on käytettävä keskihajonta-ehtoa ja ei tuotannon varten.

### <a name="step-3"></a>Vaihe 3

Kun perusasetuksia on määritetty, seuraava vaihe on määrittää virtual verkon, jota käytetään. Virtuaalinen verkon ovat suosikkiraporttisi sovellus, jossa sovellus yhdyskäytävän kuormituksen tasaaminen varten.

Valitse **Valitse virtual verkon** määrittäminen virtual verkon.

![sivu, jossa sovellus yhdyskäytävän asetusten][3]

### <a name="step-4"></a>Vaihe 4

*Valitse virtuaaliverkko* -sivu Valitse **Luo uusi**

Kun selitetään ei ole tässä tilanteessa, voi valittuna olemassa olevan Virtual verkoston tässä vaiheessa.  Jos käytössä on aiemmin virtual verkkoon, on tärkeää tietää virtual verkko on tyhjä aliverkon tai aliverkon vain sovelluksen yhdyskäytävän resursseja voi käyttää.

![Valitse VPN-sivu][4]

### <a name="step-5"></a>Vaihe 5

Täytä **Luoda virtuaalisen verkko** -sivu verkkotiedot kuvatulla tavalla edellisen [skenaarion](#scenario) kuvaus.

![Luo VPN-sivu, jossa tiedot][5]

### <a name="step-6"></a>Vaihe 6

Kun virtual verkko on luotu, seuraava vaihe on Määritä edusta IP sovelluksen Gateway. Tässä vaiheessa valinta on julkisen ja yksityisen IP-osoitteen määrittäminen edusta. Valinnan riippuu siitä, onko sovellus internet aukeaman tai sisäinen vain. Tässä tilanteessa oletetaan julkisen IP-osoitetta. Jos haluat valita yksityinen IP-osoite, voi napsauttaa **Yksityinen** -painike. Valitaan automaattisesti IP-osoite tai **Valitse yksityinen IP-osoite** -valintaruutu, jos haluat kirjoittaa jonkin napsauttamalla.

### <a name="step-7"></a>Vaihe 7: ssä

Valitse **julkinen IP-osoite**. Jos aiemmin julkiseen IP-osoite on käytettävissä tässä vaiheessa valitaan, tässä skenaariossa voit luoda uuden julkiseen IP-osoite. Valitse **Luo uusi**

![Valitse julkisen IP-osoite-sivu][6]

### <a name="step-8"></a>Vaihe 8

Seuraavaksi julkiseen IP-osoitteeseen helpossa muodossa nimeä ja valitse **OK**

![Luo julkinen IP-osoite-sivu][7]

### <a name="step-9"></a>Vaihe 9

Voit määrittää sovelluksen yhdyskäytävän luotaessa viimeisen asetus on kuuntelun määritykset.  Jos käytössä on **http** , määrittäminen ei jää mitään ja **OK** voi napsauttaa. Jos haluat käyttää **https** edelleen määritystä ei tarvita.

Käyttämään **https**tarvitaan sertifikaatti. Yksityinen avain varmenteen tarvitaan, jotta .pfx vienti varmenne- ja salasana on annettava.

### <a name="step-10"></a>Vaihe 10

Valitse **HTTPS**- **Lataa PFX varmenteen** tekstiruudun vieressä olevaa **kansion** kuvaketta.
Siirry tiedostojärjestelmän .pfx-todistus-tiedosto. Kun se on valittuna, anna sertifikaatin kutsumanimi ja salasana .pfx-tiedosto.

Kun valmiiksi valitsemalla **OK** sovelluksen yhdyskäytävän asetusten tarkistaminen.

![Listener Configuration-osan asetukset-sivu][9]

### <a name="step-11"></a>Vaihe 11

Tarkista yhteenveto-sivulle ja valitse **OK**.  Sovelluksen yhdyskäytävä on nyt jonossa ja luoda.

### <a name="step-12"></a>Vaihe 12

Kun sovellus yhdyskäytävä on luotu, siirry siihen jatka määritystä sovelluksen yhdyskäytävän portaalissa.

![Sovelluksen yhdyskäytävän resurssinäkymä][10]

Nämä vaiheet Luo basic sovelluksen yhdyskäytävän oletusasetusten listener, Taustajärjestelmä resurssivarantoon, Taustajärjestelmä http asetukset ja säännöt. Voit muuttaa näitä asetuksia haluamallasi käyttöönoton, kun valmistelun onnistuu.

## <a name="next-steps"></a>Seuraavat vaiheet

Tässä skenaariossa luo oletusarvon sovelluksen yhdyskäytävä. Seuraavat vaiheet ovat voivat määrittää sovelluksen yhdyskäytävän ryhmän jäsenten lisääminen ja asetusten muokkaaminen muuttamalla sääntöjen yhdyskäytävän se toimii oikein.

Tietokantakyselyjen luominen mukautetun kunto keräysputkien ohjesisältöä, [luoda mukautetun kunto näytteenottimen](application-gateway-create-probe-portal.md)

Lue, miten voit määrittää SSL purkaminen ja ota kallista SSL-salauksen käytöstä WWW-palvelimien ohjesisältöä [Määrittäminen SSL purku](application-gateway-ssl-portal.md)

Opettele suojaa sovellustesi [Web-sovelluksen palomuurin](application-gateway-webapplicationfirewall-overview.md) ominaisuus sovelluksen yhdyskäytävän.

<!--Image references-->
[1]: ./media/application-gateway-create-gateway-portal/figure1.png
[2]: ./media/application-gateway-create-gateway-portal/figure2.png
[3]: ./media/application-gateway-create-gateway-portal/figure3.png
[4]: ./media/application-gateway-create-gateway-portal/figure4.png
[5]: ./media/application-gateway-create-gateway-portal/figure5.png
[6]: ./media/application-gateway-create-gateway-portal/figure6.png
[7]: ./media/application-gateway-create-gateway-portal/figure7.png
[8]: ./media/application-gateway-create-gateway-portal/figure8.png
[9]: ./media/application-gateway-create-gateway-portal/figure9.png
[10]: ./media/application-gateway-create-gateway-portal/figure10.png
[scenario]: ./media/application-gateway-create-gateway-portal/scenario.png
