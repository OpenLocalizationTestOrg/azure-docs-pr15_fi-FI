<properties
   pageTitle="Luo sovelluksen yhdyskäytävän web-sovelluksen palomuuri-portaalissa | Microsoft Azure"
   description="Opi luomaan sovelluksen yhdyskäytävän web-sovelluksen palomuuri mukaan-portaalissa"
   services="application-gateway"
   documentationCenter="na"
   authors="georgewallace"
   manager="carmonm"
   editor="tysonn"
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

# <a name="create-an-application-gateway-with-web-application-firewall-by-using-the-portal"></a>Sovelluksen yhdyskäytävän luominen web-sovelluksen palomuuri-portaalissa

> [AZURE.SELECTOR]
- [Azure portal](application-gateway-web-application-firewall-portal.md)
- [Azure Resurssienhallinta PowerShell](application-gateway-web-application-firewall-powershell.md)

Web application palomuuri (WAF) Azure sovelluksen yhdyskäytävän suojaa verkkosovellusten yleisiä verkkopohjaisia kalastelu, kuten SQL lisäämisen ja komentosarjoja sivustojen-istunnon hijacks. Verkkosovelluksen suojaa monia OWASP Ylin 10 yleisiä web heikkouksien.

Azure sovelluksen yhdyskäytävä on kerroksen 7 kuormituksen. Se sisältää automaattisesti, suorituskyvyn reititys-pyyntöjen eri palvelinten välillä, olivatpa ne sitten cloud tai paikallisen.
Sovelluksen sisältää useita sovelluksen toimituksen ohjauskoneen (ADC) mukaan lukien HTTP kuormituksen tasaamisen, eväste perustuva istunnon affiniteetti, Secure Sockets Layer (SSL) purku mukautetun kunto keräysputkien usean sivuston tuki ja monista muista.
Luettelo kaikista tuetut ominaisuudet etsimällä käy [Sovelluksen yhdyskäytävän yleiskatsaus](application-gateway-introduction.md)

## <a name="scenarios"></a>Skenaariot

Tässä artikkelissa on kaksi skenaariota:

Ensimmäinen skenaario opit [Lisää olemassa olevan sovelluksen yhdyskäytävän web application palomuurin](#add-web-application-firewall-to-an-existing-application-gateway).

Toisen tilanteessa opit luomaan [sovelluksen yhdyskäytävän web-sovelluksen palomuuri](#create-an-application-gateway-with-web-application-firewall)

![Skenaario-Esimerkki][scenario]

>[AZURE.NOTE] Sovelluksen yhdyskäytävän, mukaan lukien mukautetun kunto lisämäärityksiä probes, Taustajärjestelmä resurssivarantoon osoitteet ja lisäsääntöjä määritetään sen jälkeen, kun sovellus yhdyskäytävä on määritetty ja ei ensimmäisen käyttöönoton yhteydessä.

## <a name="before-you-begin"></a>Ennen aloittamista

Azure sovelluksen yhdyskäytävä on oltava oma aliverkon. Virtuaalinen verkoston luominen varmistaa, että jätät on useita aliverkosta tarpeeksi osoitetilaa. Kun sovellus-yhdyskäytävän aliverkon otetaan käyttöön, vain Lisää sovellus yhdyskäytävät voivat aliverkon lisättäväksi.

## <a name="add-web-application-firewall-to-an-existing-application-gateway"></a>Web-sovelluksen palomuurin lisääminen olemassa olevan sovelluksen yhdyskäytävän

Tässä skenaariossa päivittää aiemmin sovelluksen-yhdyskäytävän tukea web-sovelluksen palomuuri estäminen.

### <a name="step-1"></a>Vaihe 1

Siirry Azure-portaaliin, valitse aiemmin luotu sovelluksen-yhdyskäytävä.

![Sovelluksen yhdyskäytävän luominen][1]

### <a name="step-2"></a>Vaihe 2

Valitse **määritys** ja Päivitä yhdyskäytävän sovellusasetukset. Kun olet valmis valitsemalla **Tallenna**

Päivitä aiemmin sovelluksen-yhdyskäytävän tukea web-sovelluksen palomuurin asetukset ovat seuraavat:

- **Taso** - tason valittuna on oltava **WAF** tukemaan web-sovelluksen palomuuri
- **Tuote kokoa** - asetusta on haluamasi kokoinen sovelluksen yhdyskäytävän web-sovelluksen palomuuri, käytettävissä olevat vaihtoehdot (**Normaali** ja **suuri**).
- **Palomuurin tila** - asetus poistetaan käytöstä tai ottaa käyttöön web-sovelluksen palomuuri.
- **Palomuuritila** - asetus on, miten web-sovelluksen palomuurin käsitellään haittaohjelmien liikenne. **Tunnistamisen** tilassa Kirjaa vain tapahtumat, jossa **estäminen** Kirjaa tapahtumat ja lopettaa haittaohjelmien liikenne.

![sivu näkyy perusasetukset][2]

>[AZURE.NOTE] Web-sovelluksen palomuurin lokit katselemista Diagnostiikka on otettava käyttöön ja ApplicationGatewayFirewallLog valitaan. 1 esiintymä-määrä on valittava testausta varten. On tärkeää tietää, minkä tahansa esiintymän määrä-kohdassa kaksi esiintymää ei koske SLA ja näin ollen ei suositella. Pieni yhdyskäytävät eivät ole käytettävissä, web-sovelluksen palomuurin käytettäessä.

## <a name="create-an-application-gateway-with-web-application-firewall"></a>Luo sovelluksen yhdyskäytävän web-sovelluksen palomuuri

Tässä skenaariossa on:

- Luo kaksi esiintymää Normaali web application palomuurin sovelluksen yhdyskäytävän.
- Luo virtuaalisia verkko nimeltä AdatumAppGatewayVNET varattu CIDR tekstialueen 10.0.0.0/16 kanssa.
- Luo nimi, joka käyttää 10.0.0.0/28 sen CIDR palkkina Appgatewaysubnet aliverkon.
- Määritä SSL purku varmennetta.

### <a name="step-1"></a>Vaihe 1

Siirry Azure-portaaliin, valitse **Uusi** > **Verkko** > **Sovelluksen yhdyskäytävän**

![Sovelluksen yhdyskäytävän luominen][1-1]

### <a name="step-2"></a>Vaihe 2

Täytä seuraava sovelluksen yhdyskäytävän perustietoja. Muista valita **WAF** tason nimellä. Kun olet valmis, valitse **OK**

Perusasetuksia tarvittavat tiedot:

- **Nimi** - sovelluksen yhdyskäytävän nimi.
- **Taso** - sovelluksen yhdyskäytävää, taso valittavissa olevat vaihtoehdot ovat (**Vakio** ja **WAF**). Web-sovelluksen palomuuri on käytettävissä vain WAF taso.
- **Tuote kokoa** - asetusta on haluamasi kokoinen sovelluksen yhdyskäytävän, käytettävissä olevat vaihtoehdot (**Normaali** ja **suuri**).
- **Esiintymän Laske** - esiintymien määrän, tämä arvo on oltava luku **2** – **10**.
- **Resurssiryhmä** - sovelluksen yhdyskäytävää, pidä resurssiryhmän voi olla aiemmin resurssiryhmä tai uusi tunnus.
- **Sijainti** - sovelluksen yhdyskäytävää, alue on osoitteessa resurssiryhmän samaan sijaintiin. *Sijainti on tärkeää kuin virtual verkon ja julkiseen IP on oltava samassa sijainnissa kuin yhdyskäytävän*.

![sivu näkyy perusasetukset][2-2]

>[AZURE.NOTE] 1 esiintymä-määrä on valittava testausta varten. On tärkeää tietää, minkä tahansa esiintymän määrä-kohdassa kaksi esiintymää ei koske SLA ja näin ollen ei suositella. Pieni yhdyskäytävien ei tue web-sovelluksen palomuurin skenaarioissa.

### <a name="step-3"></a>Vaihe 3

Kun perusasetuksia on määritetty, seuraava vaihe on määrittää virtual verkon, jota käytetään. Virtuaalinen verkon ovat suosikkiraporttisi sovellus, jossa sovellus yhdyskäytävän kuormituksen tasaaminen varten.

Valitse **Valitse virtual verkon** määrittäminen virtual verkon.

![sivu, jossa sovellus yhdyskäytävän asetusten][3]

### <a name="step-4"></a>Vaihe 4

**Valitse virtuaaliverkko** -sivu Valitse **Luo uusi**

Kun selitetään ei ole tässä tilanteessa, voi valittuna olemassa olevan Virtual verkoston tässä vaiheessa.  Jos käytössä on aiemmin virtual verkkoon, on tärkeää tietää virtual verkko on tyhjä aliverkon tai aliverkon vain sovelluksen yhdyskäytävän resursseja voi käyttää.

![Valitse VPN-sivu][4]

### <a name="step-5"></a>Vaihe 5

Täytä **Luoda virtuaalisen verkko** -sivu verkkotiedot kuvatulla tavalla edellisen [skenaarion](#scenario) kuvaus.

![Luo VPN-sivu, jossa tiedot][5]

### <a name="step-6"></a>Vaihe 6

Kun virtual verkko on luotu, seuraava vaihe on voit määrittää sovelluksen yhdyskäytävän edusta IP. Tässä vaiheessa valinta on julkisen ja yksityisen IP-osoitteen määrittäminen edusta. Valinnan riippuu siitä, onko sovellus internet aukeaman tai sisäinen vain. Tässä tilanteessa oletetaan julkisen IP-osoitetta. Jos haluat valita yksityinen IP-osoite, voi napsauttaa **Yksityinen** -painike. Valitaan automaattisesti IP-osoite tai **Valitse yksityinen IP-osoite** -valintaruutu, jos haluat kirjoittaa jonkin napsauttamalla.

Valitse **julkinen IP-osoite**. Jos aiemmin julkiseen IP-osoite on käytettävissä tässä vaiheessa valitaan, tässä skenaariossa voit luoda uuden julkiseen IP-osoite. Valitse **Luo uusi**

![Valitse julkisen IP-osoite-sivu][6]

### <a name="step-7"></a>Vaihe 7: ssä

Seuraavaksi julkiseen IP-osoitteeseen helpossa muodossa nimeä ja valitse **OK**

![Luo julkinen IP-osoite-sivu][7]

### <a name="step-8"></a>Vaihe 8

Seuraavaksi voit määrittää kuuntelun määritykset.  Jos käytössä on **http** , määrittäminen ei jää mitään ja **OK** voi napsauttaa. Jos haluat käyttää **https** edelleen määritystä ei tarvita.

Käyttämään **https**tarvitaan sertifikaatti. Sertifikaatin yksityinen avain tarvitaan, jotta .pfx Vie toimitettavat varmenteen tarpeet ja tiedostolle salasana.

Valitse **HTTPS**- **Lataa PFX varmenteen** tekstiruudun vieressä olevaa **kansion** kuvaketta.
Siirry tiedostojärjestelmän .pfx-todistus-tiedosto. Kun se on valittuna, anna sertifikaatin kutsumanimi ja salasana .pfx-tiedosto.

Kun valmiiksi valitsemalla **OK** sovelluksen yhdyskäytävän asetusten tarkistaminen.

![Listener Configuration-osan asetukset-sivu][8]

### <a name="step-9"></a>Vaihe 9

Määritä **WAF** tiettyjä asetuksia.

- **Palomuurin tila** - asetus ottaa WAF käyttöön tai poistaminen käytöstä.
- **Palomuuritila** - asetus määrittää toiminnot WAF kestää haittaohjelmien liikenne. Jos **tunnistus** on valittu, liikenne vain lokiin.  Jos **estäminen** valitaan, liikenne kirjautunut ja pysäytetty kanssa 403 ei oikeuksia.

![Web-sovelluksen palomuuriasetukset][9]

### <a name="step-10"></a>Vaihe 10

Tarkista yhteenveto-sivulle ja valitse **OK**.  Sovelluksen yhdyskäytävä on nyt jonossa ja luoda.

### <a name="step-12"></a>Vaihe 12

Kun sovellus yhdyskäytävä on luotu, siirry siihen jatka määritystä sovelluksen yhdyskäytävän portaalissa.

![Sovelluksen yhdyskäytävän resurssinäkymä][10]

Nämä vaiheet Luo basic sovelluksen yhdyskäytävän oletusasetusten listener, Taustajärjestelmä resurssivarantoon, Taustajärjestelmä http asetukset ja säännöt. Voit muuttaa näitä asetuksia haluamallasi käyttöönoton, kun valmistelun on tarkistettu

## <a name="next-steps"></a>Seuraavat vaiheet

Opettele määrittämään Diagnostiikan kirjaus lokiin tapahtumat, jotka ovat havaita tai esti Web-sovelluksen palomuuri ohjesisältöä [Sovelluksen yhdyskäytävän diagnostiikka](application-gateway-diagnostics.md)

Tietokantakyselyjen luominen mukautetun kunto keräysputkien ohjesisältöä, [luoda mukautetun kunto näytteenottimen](application-gateway-create-probe-portal.md)

Lue, miten voit määrittää SSL purkaminen ja ota kallista SSL-salauksen käytöstä WWW-palvelimien ohjesisältöä [Määrittäminen SSL purku](application-gateway-ssl-portal.md)

<!--Image references-->
[1]: ./media/application-gateway-web-application-firewall-portal/figure1.png
[2]: ./media/application-gateway-web-application-firewall-portal/figure2.png
[1-1]: ./media/application-gateway-web-application-firewall-portal/figure1-1.png
[2-2]: ./media/application-gateway-web-application-firewall-portal/figure2-2.png
[3]: ./media/application-gateway-web-application-firewall-portal/figure3.png
[4]: ./media/application-gateway-web-application-firewall-portal/figure4.png
[5]: ./media/application-gateway-web-application-firewall-portal/figure5.png
[6]: ./media/application-gateway-web-application-firewall-portal/figure6.png
[7]: ./media/application-gateway-web-application-firewall-portal/figure7.png
[8]: ./media/application-gateway-web-application-firewall-portal/figure8.png
[9]: ./media/application-gateway-web-application-firewall-portal/figure9.png
[10]: ./media/application-gateway-web-application-firewall-portal/figure10.png
[scenario]: ./media/application-gateway-web-application-firewall-portal/scenario.png