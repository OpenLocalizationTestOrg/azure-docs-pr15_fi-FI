<properties
   pageTitle="Logiikan sovellusten asentaminen paikallisen tietojen yhdyskäytävän | Microsoft Azure"
   description="Tietoja paikallisen tietojen yhdyskäytävän käytettäväksi asentaminen logiikan-sovelluksessa."
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="07/05/2016"
   ms.author="jehollan"/>

# <a name="install-the-on-premises-data-gateway-for-logic-apps"></a>Paikallisen tietojen yhdyskäytävän for logiikan-sovellusten asentaminen

## <a name="installation-and-configuration"></a>Asentaminen ja määrittäminen

### <a name="prerequisites"></a>Edellytykset

Pienin:

* .NET 4.5 framework
* Windows 7 tai Windows Server 2008 R2: n 64-bittinen versio (tai uudempi)

Suositus:

* 8 core Suoritin
* 8 Gigatavun muistia
* 64-bittinen versio Windows 2012 R2: n (tai uudempi)

Aiheeseen liittyvät seikat:

* Yhdyskäytävää ei voi asentaa toimialueen ohjauskoneen.
* Yhdyskäytävää ei kannata asentaminen tietokoneeseen, kannettavan, jotka voivat olla sammutettu, tai ei ole yhteydessä Internetiin, koska yhdyskäytävä ei voi suorittaa näitä tilanteissa. Lisäksi yhdyskäytävän suorituskyky voi kärsiä langattomassa verkossa.

### <a name="install-a-gateway"></a>Asenna yhdyskäytävän

Pääset [tähän paikallisen-tietojen yhdyskäytävän asennusohjelma](http://go.microsoft.com/fwlink/?LinkID=820931&clcid=0x409).

Määrittää **paikallisen tietojen yhdyskäytävän** tila, kirjaudu sisään käyttämällä työpaikan tai oppilaitoksen tiliä, ja valitse Määritä uusi yhdyskäytävä tai siirtää, palauttaa tai aiemmin yhdyskäytävän menee.

* Jos haluat määrittää yhdyskäytävän, kirjoita **nimi** ja **avaimen**, napsauta tai napauta **määrittäminen**.

    Määritä palautus-näppäintä, joka sisältää vähintään kahdeksaa merkkiä ja pidä se turvallisessa paikassa. Tarvitset tätä näppäintä, jos haluat siirtää, palauttaminen tai sen yhdyskäytävän menee.

* Voit siirtää, palauttaa tai aiemmin yhdyskäytävän menee, anna palautus-näppäintä, joka on määritetty yhdyskäytävän luonnin yhteydessä.

### <a name="restart-the-gateway"></a>Käynnistä yhdyskäytävän uudelleen

Yhdyskäytävän suoritetaan Windows-palveluna ja samalla tavalla kuin muiden Windows-palvelun, voit aloittaa ja lopettaa useilla eri tavoilla. Voit esimerkiksi Avaa komentorivi järjestelmänvalvojan oikeuksilla tietokoneeseen, jossa yhdyskäytävä on käynnissä ja suorittamalla jompikumpi näistä vaihtoehdoista:

* Palvelun pysäyttäminen suorittaa tämän komennon:

    `net stop PBIEgwService`

* Käynnistä palvelu, suorita tämä komento:

    `net start PBIEgwService`

### <a name="configure-a-firewall-or-proxy"></a>Palomuurin tai välityspalvelimen määrittäminen

Lisätietoja Anna välityspalvelimen tiedot käyttämäsi yhdyskäytävän, tutustu artikkeliin [välityspalvelimen asetusten määrittäminen](https://powerbi.microsoft.com/en-us/documentation/powerbi-gateway-proxy/).

Voit tarkistaa, onko palomuuri tai välityspalvelin, Estä yhteydet suorittamalla seuraava komento PowerShell-kehotteessa. Tämä Testaa yhteys Azure-palvelu Bus. Tässä vain Testaa verkkoyhteyden, mutta ei ole mitään palvelimen pilvipalveluun tai yhdyskäytävän tehdään. Sen avulla voit selvittää, onko tietokoneesi pääsevät todella käyttämään Internet.

`Test-NetConnection -ComputerName watchdog.servicebus.windows.net -Port 9350`

Tässä esimerkissä pitäisi muistuttaa tulokset. Jos **TcpTestSucceeded** ei ole tosi, voi olla estetty palomuuri.

```
ComputerName           : watchdog.servicebus.windows.net
RemoteAddress          : 70.37.104.240
RemotePort             : 5672
InterfaceAlias         : vEthernet (Broadcom NetXtreme Gigabit Ethernet - Virtual Switch)
SourceAddress          : 10.120.60.105
PingSucceeded          : False
PingReplyDetails (RTT) : 0 ms
TcpTestSucceeded       : True
```

Jos haluat antaa monipuolisen, Korvaa kohteella tämän artikkelin kohdassa [Määritä portit](#configure-ports) ilmoitettuja **tietokoneen nimi** ja **portin** arvot.

Palomuurin voi myös estäminen, että Azure-palvelu Bus tekee Azure tietojen keskikohdan mukaan yhteydet. Jos näin on, kannattaa whitelist (Salli) kaikki nämä tiedot keskikohdan mukaan alueesi IP-osoitteet. Pääset [tähän Azure IP-osoitteiden](https://www.microsoft.com/download/details.aspx?id=41653)luettelo.

### <a name="configure-ports"></a>Määritä portit

Yhdyskäytävän Luo Azure palvelun Bus lähtevä yhteys. Lähtevien porttiin yhteydessä: TCP 443 (oletusarvo), 5671, 5672, 9350 9354 käyttämisessä. Yhdyskäytävää ei edellytä saapuvien porttien.

Lisätietoja [hybrid ratkaisuja](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md).

| TOIMIALUEIDEN NIMET | LÄHTEVIEN PORTTI | KUVAUS |
| ----- | ------ | ------ |
| *. analysis.windows.net | 443 | HTTPS |
| *. login.windows.net | 443 | HTTPS |
| *. servicebus.windows.net |5671 5672 | Lisäasetukset sanomajonojärjestelmä Protocol (AMQP) |
| *. servicebus.windows.net | 443, 9350 9354 | Kuuntelijoita-palvelun Bus välitys TCP (edellyttää 443 käytönvalvonta suojaustunnuksen saamiseksi)-protokollaa |
| *. frontend.clouddatahub.net | 443 | HTTPS |
| *. core.windows.net | 443 | HTTPS |
| Login.microsoftonline.com | 443 | HTTPS |
| *. msftncsi.com | 443 | Testaa internet-yhteys, jos yhdyskäytävä ei ole käytettävissä Power BI-palvelun avulla. |

Jos tarvitset valkoinen luettelo IP-osoitteiden sijaan toimialueessa, voit ladata ja käyttää [Microsoft Azure palvelinkeskuksen IP-alueita luettelon](https://www.microsoft.com/download/details.aspx?id=41653). Joissakin tapauksissa Azure palvelun Bus yhteydet tehdään IP-osoite täydelliset toimialuenimet sijaan.

### <a name="sign-in-account"></a>Kirjautumistiliä käyttäen

Käyttäjien Kirjaudu sisään joko on työpaikan tai oppilaitoksen tiliä. Tämä on organisaation tili. Jos Office 365-tarjoaa rekisteröitynyt ja ei anna todellinen työmäärä-sähköpostiin, se voi näyttää samalta kuin jeff@contoso.onmicrosoft.com. Tilisi sisällä pilvipalveluun, on tallennettuna vuokraajan-Azure Active Directory (AAD). Useimmissa tapauksissa AAD-tilisi UPN vastaavat sähköpostiosoite.

### <a name="windows-service-account"></a>Windows-palvelun tili

Paikallisen tietojen yhdyskäytävä on määritetty käyttämään Windows-palvelun kirjautumisen tunnistetiedon NT SERVICE\PBIEgwService. Oletusarvon mukaan se on oikeus Log palvelu. Tämä on tietokoneeseen, johon asennat yhdyskäytävän kontekstissa.

Ei ole käytetty käyttöön paikallisten tietolähteiden tai työpaikan tai oppilaitoksen tiliä, jolla kirjaudut cloud services tili.

##<a name="frequently-asked-questions"></a>Usein kysytyt kysymykset

### <a name="general"></a>Yleiset

**Kysymys**: tietolähteiden yhdyskäytävän tukee?<br/>
**Vastaus**: saavuttaman tämän kirjoittaminen, SQL Server.

**Kysymys**: tarvitaan yhdyskäytävän pilvipalveluun, kuten SQL Azure-tietolähteiden? <br/>
**Vastaus**: ei. Yhdyskäytävän muodostaa vain paikallisen tietolähteisiin.

**Kysymys**: mitä todellinen Windows-palvelun, jonka nimi on?<br/>
**Vastaus**: palvelut-yhdyskäytävän kutsutaan Power BI yrityksen Gateway-palveluun.

**Kysymys**: mitä tahansa saapuvia yhteyksiä Gateway pilvestä? <br/>
**Vastaus**: ei. Yhdyskäytävän käyttää Azure palvelun Bus lähteviä yhteyksiä.

**Kysymys**: entä jos estän Lähtevät yhteydet? Mitä tarvitsen Avaa? <br/>
**Vastaus**: Katso portit ja isännöi, joka käyttää yhdyskäytävän.


**Kysymys**: ole yhdyskäytävä on asennettava tietolähteenä samaan tietokoneeseen? <br/>
**Vastaus**: ei. Yhdyskäytävän muodostaa yhteyden tietolähteeseen käyttämällä yhteystiedot, joka on annettu. Ajattele yhdyskäytävän asiakassovellus tältä. Se on vain voi yhdistää palvelimen nimi, joka on annettu.


**Kysymys**: mikä on viive suorittamaan kyselyjä tietolähteen Yhdyskäytävästä? Mikä on paras arkkitehtuuri? <br/>
**Vastaus**: verkon latenssin pienentämiseksi asentaa yhdyskäytävän tietolähteen mahdollisimman lähelle. Voit asentaa yhdyskäytävän todellinen tietolähteen, jos se Minimoi viive käyttöön. Ota huomioon myös keskikohdan mukaan tiedot. Esimerkiksi jos sinulla on SQL Server Azure-AM ylläpidettävä palvelusi on käytössä Länsi US tietokeskuksen, kannattaa on Länsi US Azure-AM paikan päällä. Tämä Minimoi viive ja välttää Azure AM juniin kulut.


**Kysymys**: mitä tahansa kaistanleveys vaatimukset? <br/>
**Vastaus**: on suositeltavaa on hyvä siirtonopeuden verkkoyhteydessä. Jokaisen ympäristö on eri ja lähetetty tietomäärän vaikuttaa tuloksiin. Käyttämällä ExpressRoute voi auttaa taata taso siirtonopeuden paikallisen ja Azure tietojen keskikohdan mukaan.

Kolmannen osapuolen työkalun Azure nopeuden testaaminen-sovelluksen avulla voit mitta oman siirtonopeuden ominaisuudet.


**Kysymys**: Voit suorittaa yhdyskäytävän Windows-palvelun Azure Active Directory-tilin kanssa? <br/>
**Vastaus**: ei. Windows-palvelu on oltava kelvollinen Windows-tili. Oletusarvon mukaan se suoritetaan Service-SUOJAUSTUNNUS NT SERVICE\PBIEgwService kanssa.


**Kysymys**: kuinka tulokset lähetetään takaisin pilveen? <br/>
**Vastaus**: Tämä on valmis Bus Azure-palvelun kautta. Lisätietoja on artikkelissa miten se toimii.


**Kysymys**: Omat tunnistetiedot tallennuspaikkaa? <br/>
**Vastaus**: tunnistetiedot, jotka syötät tietolähde on tallennettu salattu yhdyskäytävän pilvipalvelussa. Tunnistetietojen salaus puretaan osoitteessa-yhdyskäytävä-ympäristöön.

### <a name="high-availabilitydisaster-recovery"></a>Suuri käytettävyys ja palauttaminen

**Kysymys**: onko suunnitelmien käyttöönoton suuren käytettävyyden skenaarioita Gatewayn? <br/>
**Vastaus**: Tämä on mallia, mutta emme ole aikajanan vielä.


**Kysymys**: mitä asetuksia on käytettävissä palauttaminen? <br/>
**Vastaus**: palautus-näppäimellä palauttaminen tai siirtäminen yhdyskäytävä. Kun olet asentanut yhdyskäytävän, Määritä palautus-näppäintä.


**Kysymys**: mikä on hyötyjä palautus-näppäintä? <br/>
**Vastaus**: sen avulla voit siirtää tai palauta yhdyskäytävän asetusten huono jälkeen.

### <a name="troubleshooting"></a>Vianmääritys

**Kysymys**: missä yhdyskäytävän lokit ovat? <br/>
**Vastaus**: katso tämän artikkelin työkalut.


**Kysymys**: Miten näen, kyselyt kierretään paikallisen tietolähteen lähetetään? <br/>
**Vastaus**: Voit ottaa kyselyn jäljityksen, joka sisältää lähetetty kyselyt. Muista vaihtaa takaisin alkuperäiseen arvoon, kun valmis vianmääritys. Jätä kysely jäljitys käytössä aiheuttaa kooksi lokitiedot.

Voit myös tarkastella työkaluja, joilla tietolähde on jäljitys kyselyjen. Voit esimerkiksi käyttää laajennettu tapahtumia tai SQL-kuvaajan ja SQL Server Analysis Services.

## <a name="how-the-gateway-works"></a>Yhdyskäytävän toiminta

Kun käyttäjä toimii elementtiä, joka on yhteydessä paikalliseen tietolähteeseen kanssa:

1. Pilvipalvelussa sijaitsevaan luo kyselyyn sekä tietolähteen salattuja tunnistetietojen ja lähettää kyselyn yhdyskäytävän käsittelemään jonossa.
1. Palvelun analysoi kyselyn, ja Vie pyynnön Azure-palvelu Bus.
1. Paikallisen tietojen yhdyskäytävän tekee kyselyn Azure palvelun Bus varten pyyntöihin.
1. Yhdyskäytävän saa kyselyn, purkaa tunnistetietojen ja muodostaa yhteyden tietolähteet ja tunnistetiedot.
1. Yhdyskäytävän lähettää kyselyn suorittamista varten tietolähteeseen.
1. Tulokset lähetetään tietolähteen takaisin yhdyskäytävän ja aina pilvipalvelussa sijaitsevaan. Palvelun käyttää tulokset.

## <a name="troubleshooting"></a>Vianmääritys

### <a name="update-to-the-latest-version"></a>Päivitä uusimpaan versioon

Ongelmat paljon voi tuoda, kun yhdyskäytäväversio ei ole ajan tasalla.  On yleinen hyvä varmistaaksesi, että käytössäsi on uusin versio.  Jos ei ole päivitetty yhdyskäytävän kuukausi- tai pidempi, haluat ehkä avulla voit asentaa uusimman version yhdyskäytävän ja tarkista, jos voit toistaa ongelman.

### <a name="error-failed-to-add-user-to-group--2147463168-pbiegwservice-performance-log-users-"></a>Virhe: Käyttäjien lisääminen ryhmään epäonnistui. (-2147463168 PBIEgwService suorituskyvyn käyttäjät)

Näyttöön voi tulla seuraava virhesanoma, jos yrität asentaa yhdyskäytävän toimialueen ohjauskoneen, jota ei tueta. Sinun on käyttöön tietokoneessa, joka ei ole toimialueen ohjauskoneen yhdyskäytävän.

## <a name="tools"></a>Työkalut

### <a name="collecting-logs-from-the-gateway-configurator"></a>Keräämisen yhdyskäytävän määritystoiminnon lokit

Voit kerätä useita lokit yhdyskäytävän. Aloitetaan aina lokit!

#### <a name="installer-logs"></a>Installer lokit

`%localappdata%\Temp\Power_BI_Gateway_–Enterprise.log`

#### <a name="configuration-logs"></a>Määritysten lokit

`%localappdata%\Microsoft\Power BI Enterprise Gateway\GatewayConfigurator.log`

#### <a name="enterprise-gateway-service-logs"></a>Yrityksen yhdyskäytävän lokit

`C:\Users\PBIEgwService\AppData\Local\Microsoft\Power BI Enterprise Gateway\EnterpriseGateway.log`

#### <a name="event-logs"></a>Tapahtumalokien

Data Management Gateway ja PowerBIGateway lokit ovat näkyvissä, valitse **sovellus- ja palvelulokit**.

### <a name="fiddler-trace"></a>Fiddler seuranta

[Fiddler](http://www.telerik.com/fiddler) on ilmainen työkalu-Telerik, valvoo HTTP-liikenne.  Voit tarkastella taaksepäin ja tuotua Power BI palvelun asiakkaan tietokoneesta. Tämä näyttää virheet ja muut tarvittavat tiedot.

## <a name="next-steps"></a>Seuraavat vaiheet
- [Luoda paikallisen yhteys logiikan sovellukset](app-service-logic-gateway-connection.md)
- [Yrityksen asiakasintegraatio-ominaisuuksia](app-service-logic-enterprise-integration-overview.md)
- [Logiikan sovellusten yhdistimet](../connectors/apis-list.md)