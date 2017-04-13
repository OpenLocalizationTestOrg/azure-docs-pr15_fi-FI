<properties
    pageTitle="Yhdistä mukautetun toimialuenimen Azure-sovellukseen"
    description="Opettele käyttämään mukautettua toimialuenimeä (vanity toimialue) yhdistäminen Azure App palvelun sovelluksen."
    services="app-service"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor="jimbe"
    tags="top-support-issue"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/27/2016"
    ms.author="cephalin"/>

# <a name="map-a-custom-domain-name-to-an-azure-app"></a>Yhdistä mukautetun toimialuenimen Azure-sovellukseen

[AZURE.INCLUDE [web-selector](../../includes/websites-custom-domain-selector.md)]

Tässä artikkelissa näytetään, miten voit yhdistää manuaalisesti web Appissa, mobiilisovelluksen taustatietokannan tai [Azure App Service](../app-service/app-service-value-prop-what-is.md)API-sovellusta mukautettua toimialuenimeä. 

Sovelluksen sisältyy jo yksilöllinen alitoimialuetta azurewebsites.net. Esimerkiksi jos sovellus on **contoso**, valitse sen toimialuenimi on **contoso.azurewebsites.net**. Voit kuitenkin yhdistää mukautetun toimialueen nimi App niin, jonka sen URL-osoite, kuten `www.contoso.com`, vastaa omaa brändiä.

>[AZURE.NOTE] Pyydä apua Azure asiantuntijoiden [Azure keskustelupalstoilla](https://azure.microsoft.com/support/forums/). Siirry [Azure tukevat sivuston](https://azure.microsoft.com/support/options/) jopa ylemmälle tasolle tukea, ja valitse **Pyydä tuki**.

[AZURE.INCLUDE [introfooter](../../includes/custom-dns-web-site-intro-notes.md)]

## <a name="buy-a-new-custom-domain-in-azure-portal"></a>Ostaa uuden mukautetun toimialueen Azure-portaalissa

Jos et ole jo ostanut mukautettua toimialuenimeä, voit ostaa sen ja hallita suoraan sinua sovelluksen asetuksissa [Azure portal](https://portal.azure.com). Tämä asetus on helppo Yhdistä mukautetun toimialueen, sovelluksen sovelluksen käyttääkö [Azure liikenteen hallinta](web-sites-traffic-manager-custom-domain-name.md) vai ei. 

Lisätietoja on kohdassa [sovellus-palveluun mukautetun toimialuenimen ostaminen](custom-dns-web-site-buydomains-web-app.md).

## <a name="map-a-custom-domain-you-purchased-externally"></a>Yhdistä mukautetun toimialueen ostit ulkoisesti

Jos olet jo ostanut mukautetun toimialueen [Azure DNS](https://azure.microsoft.com/services/dns/) tai kolmannen osapuolen palveluntarjoajan, on yhdistettävä mukautetun toimialueen sovelluksen kolme päävaihetta:

1. [ *(Tietueen vain)* Hanki sovellus IP-osoite](#vip).
2. [, Joka yhdistää sovelluksen toimialueen DNS-tietueiden luonnissa](#createdns). 
    - **Jossa**: oman toimialueen Rekisteröintipalvelun oman hallintatyökalun (kuten Azure DNS, GoDaddy jne.).
    - **Miksi**: Jotta toimialueen rekisteröintipalvelu tietää on liitetty haluamasi mukautetun toimialueen Azure-sovellukseen.
1. [Ota käyttöön Azure sovelluksen mukautettua toimialuenimeä](#enable).
    - **Jossa**: [Azure portal](https://portal.azure.com).
    - **Miksi**: Jotta sovelluksesi tietää pyyntöihin tehdyt mukautettua toimialuenimeä.
3. [Tarkista DNS välitys](#verify).

### <a name="types-of-domains-you-can-map"></a>Voit yhdistää toimialueiden tyypit

Azure sovelluksen-palvelun avulla voit yhdistää mukautettujen toimialueiden seuraavanlaisia sovelluksen.

- **Päätoimialue** - toimialuerekisteröijän varattu muutettavan toimialuenimi (vastaavan `@` isännöidä tietueen yleensä). Esimerkki: **contoso.com**.
- **Alitoimialueen** - toimialueeseen, joka on kohdassa oman päätoimialue. Esimerkiksi **www.contoso.com** (vastaavan `www` isännöidä tietue).  Voit yhdistää saman päätoimialue eri alitoimialueita eri sovellusten Azure-tietokannassa.
- **Yleismerkkien toimialueen** - [minkä tahansa alitoimialueen, jonka vasemmanpuoleisin DNS-tarran `*` ](https://en.wikipedia.org/wiki/Wildcard_DNS_record) (kuten isännöidä tietueita `*` ja `*.blogs`). Esimerkiksi ** \*. contoso.com**.

### <a name="types-of-dns-records-you-can-use"></a>Voit määrittää DNS-tietueiden tyypit

Sen mukaan, sinun on ehkä voit käyttää kahdentyyppisiä standard DNS-tietueiden Yhdistä mukautetun toimialueen: 

- [A](https://en.wikipedia.org/wiki/List_of_DNS_record_types#A) - karttojen mukautettua toimialuenimeä, Azure app virtual IP-osoitteen suoraan. 
- [CNAME](https://en.wikipedia.org/wiki/CNAME_record) - määritykset mukautettua toimialuenimeä sinua sovelluksen Azure toimialuenimen, * *&lt;*appname*>. azurewebsites.net**. 

CNAME-TIETUE etuna on se, että se jatkuu IP-osoitteen muuttumisen yli. Jos poistat ja Luo sovelluksen tai muuttaa suurempi hinta taso takaisin **jaettu** taso, sinua sovelluksen virtual IP-osoite voivat muuttua. Tällainen muutos – CNAME-tietue on edelleen voimassa, A-tietue edellyttää päivitys. 

Opetusohjelman näyttää vaiheet koskevat tietueessa sekä käytettäessä CNAME-tietue.

>[AZURE.IMPORTANT] Älä luo CNAME-tietue päätoimialue (eli "pääkansio-tietue"). Lisätietoja on artikkelissa [Miksi ei CNAME-tietue voi käyttää juuri päätoimialue](http://serverfault.com/questions/613829/why-cant-a-cname-record-be-used-at-the-apex-aka-root-of-a-domain).
Voit yhdistää päätoimialue Azure-sovellukseen, käytä A-tietue.

<a name="vip"></a>
## <a name="step-1-a-record-only-get-apps-ip-address"></a>Vaihe 1. *(Tietueen vain)* Hae sovelluksen IP-osoite
Jos haluat yhdistää A-tietue käyttämällä mukautettua toimialuenimeä, sinun on Azure app IP-osoite. Jos yhdistät CNAME-tietue käyttämistä, Ohita tämä vaihe ja siirtyä seuraavaan osaan.

1.  Kirjaudu sisään [Azure portal](https://portal.azure.com).

2.  Valitse **Sovelluksen palvelut** vasemmalla olevasta valikosta.

4.  Valitse sovellus ja valitse sitten **Mukautetut toimialueet**.

6.  Ota seuraavat seikat huomioon IP-osoitteen isäntänimet osan yläpuolella...

    ![Kartan mukautettua toimialuenimeä tietueen: Azure App palvelun sovelluksen Hae IP-osoite](./media/web-sites-custom-domain-name/virtual-ip-address.png)

7.  Pidä tämän portaalin sivu auki. Sinun tulee takaisin sen kun luot DNS-tietueet.

<a name="createdns"></a>
## <a name="step-2-create-the-dns-records"></a>Vaihe 2. Luo DNS-tietueet

Kirjaudu sisään toimialueen rekisteröintipalvelu ja niiden työkalulla voit lisätä A tai CNAME-tietueen. Jokaisen Rekisteröintipalvelun Käyttöliittymän on hieman erilaiset, joten palveluntarjoajan pitäisi käyttöohjeista. Seuraavassa on kuitenkin joitakin yleisiä ohjeita.

1.  Siirry sivulle, DNS-tietueiden hallintaan. Etsi linkkejä tai **Toimialuenimi**, **DNS**tai **Nimi hallinta**, jossa sivuston alueille. Usein löytyvät tarkastelemalla tilin tiedot ja esittää sitten linkin, kuten **omien toimialueiden**linkkiä.
2.  Etsi linkin, jonka avulla voit lisätä tai muokata DNS-tietueita. Tämä saattaa olla **Zone file** tai **DNS-tietueet** , tai **Lisäasetukset** -määritysten linkittää.
3.  Luo tietue ja Tallenna muutokset.
    - [Ohjeita A-tietue on tähän](#a).
    - [Ohjeita CNAME-tietue on tähän](#cname).

<a name="a"></a>
### <a name="create-an-a-record"></a>Luo A-tietue

A-tietueen avulla voit yhdistää Azure app IP-osoite, todella haluat luoda A-tietue ja TXT-tietue. A-tietue on itse DNS-ratkaisun ja TXT-tietue on Azure omistajuuden mukautettua toimialuenimeä. 

Määritä A-tietueen seuraavasti (@ on yleensä päätoimialue):
 
<table cellspacing="0" border="1">
  <tr>
    <th>FQDN-Esimerkki</th>
    <th>Isäntä</th>
    <th>Arvo</th>
  </tr>
  <tr>
    <td>contoso.com (pääkansio)</td>
    <td>@</td>
    <td>IP-osoitteen suoraan <a href="#vip">vaihe 1</a></td>
  </tr>
  <tr>
    <td>WWW.contoso.com (ala)</td>
    <td>WWW-tunnistetta</td>
    <td>IP-osoitteen suoraan <a href="#vip">vaihe 1</a></td>
  </tr>
  <tr>
    <td>*. contoso.com (yleismerkkejä)</td>
    <td>*</td>
    <td>IP-osoitteen suoraan <a href="#vip">vaihe 1</a></td>
  </tr>
</table>

Lisää TXT-tietue, joka yhdistää-konferenssi kestää &lt; *alitoimialueen*>. &lt; *rootdomain*>, &lt; *appname*>. azurewebsites.net. Määritä TXT-tietueen seuraavasti:

<table cellspacing="0" border="1">
  <tr>
    <th>FQDN-Esimerkki</th>
    <th>TXT Host (isäntä)</th>
    <th>TXT-arvo</th>
  </tr>
  <tr>
    <td>contoso.com (pääkansio)</td>
    <td>@</td>
    <td>&lt;<i>AppName</i>>. azurewebsites.net</td>
  </tr>
  <tr>
    <td>WWW.contoso.com (ala)</td>
    <td>WWW-tunnistetta</td>
    <td>&lt;<i>AppName</i>>. azurewebsites.net</td>
  </tr>
  <tr>
    <td>*. contoso.com (yleismerkkejä)</td>
    <td>*</td>
    <td>&lt;<i>AppName</i>>. azurewebsites.net</td>
  </tr>
</table>

<a name="cname"></a>
###Luo CNAME-tietue

Jos käytät CNAME-tietueen yhdistäminen Azure sovelluksen oletustoimialuenimen, sinun ei tarvitse TXT tarvitseman tietueen kuten tekisit A-tietueen kanssa. 

>[AZURE.IMPORTANT] Älä luo CNAME-tietue päätoimialue (eli "pääkansio-tietue"). Lisätietoja on artikkelissa [Miksi ei CNAME-tietue voi käyttää juuri päätoimialue](http://serverfault.com/questions/613829/why-cant-a-cname-record-be-used-at-the-apex-aka-root-of-a-domain).
Voit yhdistää päätoimialue Azure-sovellukseen, käytä [tietueessa](#a) .

Määritä CNAME-tietue seuraavasti (@ on yleensä päätoimialue):

<table cellspacing="0" border="1">
  <tr>
    <th>FQDN-Esimerkki</th>
    <th>CNAME-TIETUE Host (isäntä)</th>
    <th>CNAME-arvo</th>
  </tr>
  <tr>
    <td>WWW.contoso.com (ala)</td>
    <td>WWW-tunnistetta</td>
    <td>&lt;<i>AppName</i>>. azurewebsites.net</td>
  </tr>
  <tr>
    <td>*. contoso.com (yleismerkkejä)</td>
    <td>*</td>
    <td>&lt;<i>AppName</i>>. azurewebsites.net</td>
  </tr>
</table>

<a name="enable"></a>
##Vaihe 3. Ottaa käyttöön mukautettua toimialuenimeä, kun sovellus

Takaisin sisään Azure-portaalissa **Mukautetut toimialueet** -sivu (katso [vaiheessa 1](#vip)), sinun on lisättävä täydellinen toimialuenimi (FQDN) mukautetun toimialueen luetteloon.

1.  Jos et ole tehnyt niin [Azure portal](https://portal.azure.com)-Kirjaudu sisään.

2.  Valitse **Sovelluksen Services** Azure-portaalissa vasemmalla olevasta valikosta.

3.  Sovellus ja valitse sitten **Mukautetut toimialueet** > **Lisää isäntänimi**.

4.  Mukautetun toimialueen FQDN lisääminen luetteloon (kuten **www.contoso.com**).

    ![Yhdistä mukautetun toimialuenimen Azure-sovellukseen: verkkotunnusten luetteloon](./media/web-sites-custom-domain-name/add-custom-domain.png)

    >[AZURE.NOTE] Azure yrittää vahvistaa toimialuenimeä, jota käytetään tähän. Varmista, että se on sama toimialuenimi, jonka loit [Vaiheessa](#createdns)2 DNS-tietue. 

5.  Valitse **Vahvista**.

6.  Kun **Validate** Azure valitsemalla projektin toimialueen vahvistus työnkulun. Tämä tarkistaa toimialueen omistajuuden sekä Hostname saatavuudesta ja raportin onnistumisen tai ominaisuuksien guidence siitä, miten voit korjata virheen yksityiskohtaiset virhe.    

7.  Onnistuneen vahvistuksen **Lisää hostname** yhteydessä painike tulee aktiivinen ja osaat, Määritä isäntänimi. 

8.  Kun Azure on valmis, uusi oman toimialuenimen määrittäminen, siirry mukautettua toimialuenimeä selaimessa. Selaimen kannattaa avata Azure sovelluksen, mikä tarkoittaa, että mukautettu toimialuenimi on määritetty oikein.

> [AZURE.NOTE] Jos DNS-tietue on jo käyttää (aktiivinen toimialueen kokouksena tietoliikenteen skenaarion) ja haluat sitoa synkronointiraja koodiin sen toimialueen vahvistusta varten, valitse yksinkertaisesti Luo TXT-tietueet esimerkkejä seuraavassa taulukossa. Lisää TXT-tietue, joka yhdistää-konferenssi kestää &lt; *alitoimialueen*>. &lt; *rootdomain*>, &lt; *appname*>. azurewebsites.net. 
> <table cellspacing="0" border="1">
  <tr>
    <th>FQDN-Esimerkki</th>
    <th>TXT Host (isäntä)</th>
    <th>TXT-arvo</th>
  </tr>
  <tr>
    <td>contoso.com (pääkansio)</td>
    <td>awverify.contoso.com</td>
    <td>&lt;<i>AppName</i>>. azurewebsites.net</td>
  </tr>
  <tr>
    <td>WWW.contoso.com (ala)</td>
    <td>awverify.WWW.contoso.com</td>
    <td>&lt;<i>AppName</i>>. azurewebsites.net</td>
  </tr>
    <tr>
    <td>*. contoso.com (ala)</td>
    <td>awverify.*.contoso.com</td>
    <td>&lt;<i>AppName</i>>. azurewebsites.net</td>
  </tr>
</table>
Kun DNS-tietue on luotu, siirry takaisin Azure-portaaliin ja mukautettua toimialuenimeä lisääminen web-sovelluksen.
 

<a name="verify"></a>
##Tarkista DNS välittäminen

Kun olet suorittanut määritysvaiheet, se voi viedä jonkin aikaa, jotta muutokset leviäminen käyttämäsi DNS-palveluntarjoajan. Voit varmistaa, että DNS-välitys toimii odotetulla tavalla käyttämällä [http://digwebinterface.com/](http://digwebinterface.com/). Kun selaat sivustoon, Määritä isäntänimet tekstiruutuun ja valitse **tarkastella**. Tarkista tulokset Varmista Jos viimeisimmät muutokset ovat tulleet voimaan.  

![Yhdistä mukautetun toimialuenimen Azure-sovellukseen: Tarkista DNS välitys](./media/web-sites-custom-domain-name/1-digwebinterface.png)

> [AZURE.NOTE] DNS-arvojen välitys voi kestää jopa 48 tuntia (joskus enää). Kaikki tiedot on määritetty oikein, jos haluat vielä Odota välittäminen onnistuu.

## <a name="next-steps"></a>Seuraavat vaiheet
Tietoja suojatun mukautettua toimialuenimeä kanssa HTTPS [ostamalla SSL-varmenteen Azure-tietokannassa](web-sites-purchase-ssl-web-site.md) tai [käyttämällä SSL-varmenteen muualta](web-sites-configure-ssl-certificate.md).

>[AZURE.NOTE] Jos haluat aloittaa Azure App palvelun ennen rekisteröimässä Azure-tili, siirry [Yritä App palvelu](http://go.microsoft.com/fwlink/?LinkId=523751), jossa lyhytkestoinen starter verkkosovellukseen heti voit luoda sovelluksen-palvelussa. Ei ole pakollinen; luottokortit ei ole sitoumukset.

[Azure DNS käytön aloittaminen](../dns/dns-getstarted-create-dnszone.md)  
[Mukautetun toimialueen DNS-tietueiden web-sovelluksen luominen](../dns/dns-web-sites-custom-domain.md)  
[Edustajan toimialueen Azure DNS](../dns/dns-domain-delegation.md)


<!-- Images -->
[subdomain]: media/web-sites-custom-domain-name/azurewebsites-subdomain.png
