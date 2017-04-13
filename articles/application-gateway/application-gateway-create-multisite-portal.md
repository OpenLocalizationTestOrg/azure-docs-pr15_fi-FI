<properties
   pageTitle="Määrittää olemassa olevan sovelluksen-yhdyskäytävän isännöimiseen Azure portaalin sivustoissa | Microsoft Azure"
   description="Tällä sivulla on ohjeita voit määrittää aiemmin Azure sovelluksen-yhdyskäytävän isännöimiseen useita web-sovellusten samassa yhdyskäytävässä Azure-portaalissa."
   documentationCenter="na"
   services="application-gateway"
   authors="georgewallace"
   manager="carmonm"
   editor="tysonn"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="gwallace"/>


# <a name="configure-an-existing-application-gateway-for-hosting-multiple-web-applications"></a>Olemassa olevan sovelluksen-yhdyskäytävän isännöimiseen useita web-sovellusten määrittäminen

> [AZURE.SELECTOR]
- [Azure portal](application-gateway-create-multisite-portal.md)
- [Azure Resurssienhallinta PowerShell](application-gateway-create-multisite-azureresourcemanager-powershell.md)

Usean sivuston isännöintiä voit ottaa käyttöön useita web-sovelluksen sovelluksen samassa yhdyskäytävässä. Se on riippuvainen merkitsevä saapuvan HTTP-pyynnössä, voit selvittää, mitkä listener näytetä liikenne toimialuenimi. Kuuntelutoiminto ohjaa liikenne tarvittavat Taustajärjestelmä resurssivarantoon sitten yhdyskäytävän säännöt määrityksessä mukaisena. SSL käytössä verkkosovellusten sovelluksen yhdyskäytävän riippuvainen valitsemaan oikean listener web-tietoliikenteen palvelimen nimi merkintä (SNI)-tunniste. Yleisen käytön usean sivuston isännöintiä varten on eri sivustoon toimialueiden toisen taustatietokannan palvelimen jakavat haluat ladata. Saman päätoimialue useita alitoimialueita voi vastaavasti ylläpitää myös sovelluksen samassa yhdyskäytävässä.

## <a name="scenario"></a>Skenaario

Seuraavassa esimerkissä sovelluksen yhdyskäytävän toimiva liikenne contoso.com ja fabrikam.com ja kaksi taustatietokantaan palvelimen jakavat: contoso palvelimen resurssivaranto ja fabrikam palvelimen resurssivarantoon. Host alitoimialueita, kuten app.contoso.com ja blog.contoso.com avulla voidaan vastaavat asetukset.

![Multisite skenaario][multisite]

## <a name="before-you-begin"></a>Ennen aloittamista

Tässä skenaariossa Lisää usean sivuston tuen olemassa olevan sovelluksen-yhdyskäytävä. Jotta voit suorittaa tämän skenaarion skenaarion yhdyskäytävän olemassa olevan sovelluksen on oltava käytettävissä määrittäminen. Käy [Luo sovelluksen yhdyskäytävän portaalin avulla](./application-gateway-create-gateway-portal.md) opit luomaan basic sovelluksen yhdyskäytävän portaalissa.

## <a name="requirements"></a>Vaatimukset

- **Taustatietokantaan palvelimen resurssivarantoon:** Taustatietokannan-palvelimien IP-osoitteiden luettelo. IP-osoitteiden luettelossa tulee kuulua joko VPN-aliverkon tai pitäisi olla julkiseen IP-tai VIP. Voidaan myös täydellinen toimialuenimi.
- **Taustatietokantaan resurssivarantoon palvelinasetukset:** Jokaisen varanto on asetusten, kuten portin, protokolla ja evästeiden perustuva affiniteetti. Nämä asetukset on liitetty resurssivarantoon, ja niitä käytetään varannon kaikki palvelimiin.
- **Edusta portti:** Tämä portti on Julkinen portti, joka on avattu sovelluksen yhdyskäytävän. Liikenne käynnit portin ja sitten uudelleenohjataan taustatietokantaan-palvelimia.
- **Listener:** Kuuntelun edusta portti-protokolla (Http tai Https-arvot ovat kirjainkoko on merkitsevä), ja SSL-varmenteen nimi (Jos määrittäminen SSL purku). Usean sivuston käytössä sovelluksen yhdyskäytävien isäntänimi ja SNI ilmaisimet myös lisätään.
- **Säännön:** Säännön sitoo listener taustatietokantaan palvelimen resurssivarantoon, ja määrittää, mitkä taustatietokantaan palvelimen resurssivarantoon liikenne olisi ohjautuu, kun se käynnit tietyn listener.
- **Varmenteet:** Kunkin listener edellyttää yksilöllinen varmennetta, tässä esimerkissä 2 kuuntelijoita luodaan usean sivuston. Kaksi .pfx-varmenteet ja niiden salasanat on luotava.

## <a name="create-an-application-gateway"></a>Sovelluksen Gatewayn luominen

Sovelluksen yhdyskäytävän päivitys vaiheet ovat seuraavat:

1. Luoda taustatietokantaan kunkin sivuston.
2. Luo uusi kuuntelu, kunkin sivuston sovelluksen yhdyskäytävä tue.
3. Luo säännöt, voit yhdistää kunkin kuuntelijaa, jonka tarvittavat taustatietokannan.

## <a name="create-back-end-pools-for-each-site"></a>Luoda taustatietokantaan jokaisen

Taustatietokannan resurssivarantoon jokaisen sovelluksen, että yhdyskäytävä on tuki tarvita, tässä tapauksessa 2 luodaan, yksi contoso11.com varten ja toinen fabrikam11.com.

### <a name="step-1"></a>Vaihe 1

Siirry Azure-portaalissa (https://portal.azure.com) luodun sovelluksen-yhdyskäytävä. **Taustajärjestelmä jakavat** ja valitse **Lisää**

![Lisää Taustajärjestelmä jakavat][7]

### <a name="step-2"></a>Vaihe 2

Kirjoita-taustatietokannan resurssivarantoon **pool1**, lisäämällä ip-osoitteet tai täydelliset toimialuenimet taustatietokantaan palvelimia tiedot ja valitse **OK**

![Taustajärjestelmä sovellussarjan pool1 asetukset][8]

### <a name="step-3"></a>Vaihe 3

Valitse Taustajärjestelmä jakavat-sivu muita taustatietokantaan resurssivarantoon **pool2**, lisäämällä ip-osoitteet tai täydelliset TOIMIALUENIMET taustatietokantaan palvelimia Lisää **Lisää** ja valitse **OK**

![Taustajärjestelmä sovellussarjan pool2 asetukset][9]

### <a name="create-listeners-for-each-back-end"></a>Luo kullekin taustatietokantaan kuuntelijoita

Sovelluksen yhdyskäytävä on riippuvainen HTTP 1.1 toimialuenimiä isännöimiseen sama julkiseen IP-osoite ja portin useita sivustossa. Luotu portaalissa basic listener ei ole tätä ominaisuutta.

### <a name="step-1"></a>Vaihe 1

Olemassa olevan sovelluksen yhdyskäytävän **kuuntelijoita** ja valitse Lisää ensimmäisen listener **usean sivuston** .

![kuuntelijoita yhteenveto-sivu][1]

### <a name="step-2"></a>Vaihe 2

Täytä listener tässä esimerkissä SSL-tilauksen tiedot on määritetty, Luo uusi edusta-porttiin. Lataa .pfx-varmenne, jota käytetään SSL-tilauksen. Valitse tämä sivu verrattuna vakio basic listener sivu ainoa ero on isäntänimi.

![Listener ominaisuudet-sivu][2]

### <a name="step-3"></a>Vaihe 3

Valitse **usean sivuston** ja luo toinen listener toisen sivuston edellisessä vaiheessa kuvatulla tavalla. Varmista, että eri varmenteen käytettävä toinen kuuntelua. Valitse tämä sivu verrattuna vakio basic listener sivu ainoa ero on isäntänimi. Täytä kuuntelua tiedot ja valitse **OK**.

![Listener ominaisuudet-sivu][3]

> [AZURE.NOTE] Kuuntelijoita sovelluksen Gatewayn Azure-portaalissa on pitkä käynnissä olevan tehtävän, voi kestää jonkin aikaa, voit luoda kaksi kuuntelijoita tässä skenaariossa. Kun suorittanut kuuntelijoita Näytä portaalissa tarkastelu seuraavan kuvan mukaisesti.

![Listener yleiskatsaus][4]

### <a name="create-rules-to-map-listeners-to-backend-pools"></a>Voit yhdistää kuuntelijoita Taustajärjestelmä jakavat sääntöjen luominen

### <a name="step-1"></a>Vaihe 1

Siirry Azure-portaalissa (https://portal.azure.com) luodun sovelluksen-yhdyskäytävä. Valitse **säännöt** ja valitse aiemmin luotu oletusarvon säännön **rule1** ja valitse **Muokkaa**.

### <a name="step-2"></a>Vaihe 2

Täytä säännöt-sivu, joka näkyy seuraavassa kuvassa. Ensimmäinen listener ja ensimmäisen ryhmän valitseminen ja valitsemalla **Tallenna** , kun olet valmis.

![aiemmin luodun säännön muokkaaminen][6]

### <a name="step-3"></a>Vaihe 3

Valitse **Perus säännön** 2. säännön luominen. Täytä lomake toisen listener ja toisen taustatietokannan resurssivarantoon ja Tallenna valitsemalla **OK** .

![Lisää perustiedot sääntö-sivu][10]

Tämä suorittaa aiemmin sovelluksen yhdyskäytävän määrittäminen usean sivuston tukeen palvelun Azure-portaalissa.

## <a name="next-steps"></a>Seuraavat vaiheet

Lue, miten voit suojata artikkelissa [Application](application-gateway-webapplicationfirewall-overview.md) Gatewayn - Web-sovelluksen palomuuri-sivuston käyttäminen

<!--Image references-->
[1]: ./media/application-gateway-create-multisite-portal/figure1.png
[2]: ./media/application-gateway-create-multisite-portal/figure2.png
[3]: ./media/application-gateway-create-multisite-portal/figure3.png
[4]: ./media/application-gateway-create-multisite-portal/figure4.png
[5]: ./media/application-gateway-create-multisite-portal/figure5.png
[6]: ./media/application-gateway-create-multisite-portal/figure6.png
[7]: ./media/application-gateway-create-multisite-portal/figure7.png
[8]: ./media/application-gateway-create-multisite-portal/figure8.png
[9]: ./media/application-gateway-create-multisite-portal/figure9.png
[10]: ./media/application-gateway-create-multisite-portal/figure10.png
[multisite]: ./media/application-gateway-create-multisite-portal/multisite.png