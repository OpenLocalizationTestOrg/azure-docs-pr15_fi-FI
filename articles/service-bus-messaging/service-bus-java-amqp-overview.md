<properties 
    pageTitle="Palvelun Bus AMQP esittely ja Java | Microsoft Azure" 
    description="Opi käyttämään Java kanssa Advanced viestin Queuing Protocol (AMQP) 1.0 Azure-tietokannassa." 
    services="service-bus" 
    documentationCenter="java" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="10/04/2016" 
    ms.author="sethm"/>


# <a name="amqp-10-support-in-service-bus"></a>Palvelun Bus AMQP 1.0-tuki

Azure-palvelu Bus pilvipalvelussa ja paikallinen [Palvelun Bus for Windows Server (Service Bus 1.1)](https://msdn.microsoft.com/library/dn282144.aspx) tuki laajennettu viesti asetetaan jonoon Protocol (AMQP) 1.0. AMQP avulla voit luoda ympäristöstä, hybrid sovellusten Avaa Vakio-protokollan avulla. Voit luoda sovellusten osat, jotka on suunniteltu eri kielillä ja kehysten käyttäminen ja, joka käyttää eri käyttöjärjestelmissä. Kaikki komponentit voivat muodostaa yhteyden palvelun Bus ja saumattomasti vaihtaa rakenteellisia business viestejä tehokkaasti ja luotettavaa.

## <a name="introduction-what-is-amqp-10-and-why-is-it-important"></a>Johdanto: Mikä on AMQP 1.0 ja miksi on tärkeää?

Perinteisesti viestin välitysohjelmistoksi tuotteet ovat käyttäneet protokollat asiakassovelluksissa ja vakuutuksenvälittäjän väliseen viestintään. Tämä tarkoittaa, että kun olet valinnut tietyn toimittajan tekstiviesti broker, avulla, että Tavarantoimittajan palvelun kirjastojen yhteyden kyseisen broker asiakkaan sovellukset. Tämä aiheuttaa aste riippuvuus toimittajan, koska sovittaminen sovelluksen eri tuotteen vaatii koodin muutoksia yhdistetyn sovelluksissa. 

Lisäksi yhteyden tekstiviesti vakuutuksenvälittäjän eri toimittajilta on hankalaa. Tämä vaatii yleensä sovellustason siltauksen, voit siirtää viestit järjestelmästä toiseen ja niiden erot omistusoikeuksia kohdekieli. Tämä on yleinen vaatimus; esimerkiksi kun on tarjoaa uuden yhdistetty käyttöliittymän, vanhemmat erillisiä järjestelmät, tai IT-järjestelmien seuraavan fuusion integroida.

Ohjelmiston alan on nopea siirtäminen business; uudet ohjelmoinnin kielet ja sovelluksen kehysten esitellään joskus bewildering tahdissa. Vastaavasti IT vaatimukset kehityttävä ajan kuluessa ja kehittäjät haluat hyödyntää uusin käyttöympäristö ominaisuuksia. Kuitenkin joskus valitun tekstiviesti toimittajan ei tue näissä ympäristöissä. Koska tekstiviesti protokollat omistusoikeuksia, ei ole mahdollista muiden antaa kirjastot näiden uusi ympäristöjen. Tämän vuoksi sinun on käytettävä tavoista, kuten yhdyskäytävien tai siltoja, joiden avulla voit edelleen käyttää tekstiviesti tuotteen.

Kehitystä, Advanced viestin Queuing Protocol (AMQP) 1.0 on motivoitunut näitä ongelmia. Se on peräisin osoitteessa JP Morgan vähentämiseksi, kuka on eniten taloudellisen tilanteen services yrityksille, kuten viestin välitysohjelmistoksi paksu käyttäjät. Tavoitteena on yksinkertainen: Luo Avaa standardiksi tekstiviesti protokolla, joka tekemisen luonnissa viesti-pohjaisten sovellusten laadittuihin kielten, kehysten ja-käyttöjärjestelmissä komponenttien käyttäminen kaikki käyttävät toimittajat solualueen rotu parhaiten osat.

## <a name="amqp-10-technical-features"></a>AMQP 1.0 teknisiä ominaisuuksia

AMQP 1.0 on tehokasta, luotettavia, wire tason tekstiviesti protokolla, jonka avulla voit luoda tehokkaat, Office kaikissa ympäristöissä, messaging sovellukset. Protokolla on yksinkertainen tavoite: Määritä turvallinen, luotettava ja tehokas siirtää viestit välillä kahden osapuolen suunnittelu. Viestien itse on koodattu käyttämällä siirrettävä tietotyyppi-esitys, jonka avulla erilaisten lähettäjien ja vastaanottajien Exchange rakenteellisia business viestien etsiminen koko tarkkuuden. Seuraavassa taulukossa on yhteenveto tärkeimmistä ominaisuuksista:

*    **Efficient**: AMQP 1.0 on yhteys aloittaminen protokolla, joka hyödyntää binaarinen koodaus protokolla-ohjeet ja business viestit siirretään päälle. Se kattaa kehittyneitä-virtaussäätimiä järjestelmien Suurenna verkon ja yhdistetyn osat. Toisaalta-protokolla on suunniteltu tasapaino tehokkuuden, joustavuutta ja yhteentoimivuus välillä.
*    **Luotettava**: AMQP 1.0-protokollan avulla voidaan vaihtaa solualueen luotettavuutta oikeudet-palomuuri-ja-unohda luotettavia, jossa täsmälleen viestit-kuitata kerran toimitusta.
*    **Joustava**: AMQP 1.0 on joustava protokolla, joita voidaan käyttää eri topologioissa tukemaan. Samaa protokollaa voidaan asiakkaan asiakas, asiakkaan broker ja broker broker yhteyksiä varten.
*    **Broker mallin riippumattoman**: AMQP 1.0 määritys ei tee vaatimuksia tekstiviesti broker käyttämän mallin. Tämä tarkoittaa, että on mahdollista lisätä AMQP 1.0 tuki aiemmin tekstiviesti vakuutuksenvälittäjän helposti.

## <a name="amqp-10-is-a-standard-with-a-capital-s"></a>AMQP 1.0 on standardi (kanssa pääoman ")

AMQP 1.0 ole ollut kehittäminen 2008 core-ryhmän enintään 20 yritysten jälkeen tekniikka toimittajien ja loppukäyttäjien yrityksille. Tänä aikana käyttäjän yritykset, joiden vuoksi niiden todellisen yrityksen tarpeet ja tekniikka toimittajista on kehittynyt protokolla täytä näitä vaatimuksia. Koko prosessin toimittajat ovat osallistuneet työpajat, jossa ne yhdessä vahvistamiseen niiden käyttöotot yhteensopivuuden.

Lokakuu 2011: n tekninen käsiteltäväksi organisaatiossa siirtymisen ja jäsennettyjä tietoja standardeja (OASIS) ja OASIS AMQP 1.0-standardin transitioned kehitystyön julkaistiin lokakuussa 2012. Seuraavat yritysten osallistuneet teknisen komitean standardin kehittämisen aikana:

*    **Tekniikka toimittajien**: Axway ohjelmiston, Huawei tekniikoita, IIT ohjelmiston, INETCO järjestelmien, Kaazing, Microsoft-Mitre Corporation, Primeton Technologies-edistymisen ohjelmiston, punainen on, SITA-ohjelmiston AG, Solace järjestelmien, VMware, WSO2, Zenika.
*    **Käyttäjän yrityksille**: Amerikka pankin, luottokortin Suisse, Saksan Boerse, Goldman Sachs, JPMorgan vähentämiseksi.

Avaa standardeja yleisesti puolustaa etuja ovat esimerkiksi seuraavat:

*    Vähemmän toimittajan Lukitse-mahdollisuus
*    Yhteensopivuus
*    Kirjastot ja sillä laaja saatavuus
*    Vanheneminen suojautumista
*    Halukkaiden henkilökunnan saatavuus
*    Ala- ja helpommin hallittaviin riski

## <a name="amqp-10-and-service-bus"></a>AMQP 1.0- ja Bus

Azure-palvelu Bus AMQP 1.0-tuki tarkoittaa, että voit nyt hyödyntää palvelun Bus queuing ja Julkaise ja tilaa se tekstiviesti ominaisuuksia solualueen ympäristöjen tehokas binaarinen-protokollan avulla. Voit lisäksi luoda osat, jotka on luotu käyttäen kielet, kehysten sekä käyttöjärjestelmät koostuu sovellukset.

Seuraavassa kuvassa on esimerkki-käyttöönoton, jossa Java asiakkaiden Linux, kirjoitettu vakio Java viestin Service (JMS) API ja Windows-.NET-asiakkaiden vaihtaa viestejä kautta käyttämällä AMQP 1.0 palvelun Bus.

![][0]

**Kuva 1: Esimerkki käyttöönottotapa ympäristöstä messaging palvelun Bus ja AMQP 1.0-näyttäminen**

Tällä hetkellä seuraavat asiakas-kirjastot kutsutaan palvelun Bus-käyttöä varten:

| Kieli | Kirjasto                                                                       |
|----------|-------------------------------------------------------------------------------|
| Java     | Asiakkaan Apache Qpid Java viestin Service (JMS)<br/>IIT ohjelmiston SwiftMQ Java-asiakas |
| C        | Apache Qpid Proton-C                                                          |
| PHP      | Apache Qpid Proton-PHP                                                        |
| Python   | Apache Qpid Proton-Python                                                     |


**Kuva 2: Sisällysluettelon AMQP 1.0 asiakkaan kirjastot**

Saat lisätietoja hankkiminen ja nämä kirjastot käyttäminen palvelun Bus [palvelun Bus AMQP Developer's Guide][]. [Seuraavat vaiheet](service-bus-java-amqp-overview.md#next-steps) on kohdassa Lisätietoja linkkejä.

## <a name="summary"></a>Yhteenveto

*    AMQP 1.0 on avoinna, luotettavia tekstiviesti protokolla, joiden avulla voit luoda ympäristöstä, hybrid sovellukset. AMQP 1.0 ei OASIS-standardia.
*    AMQP 1.0 tuki on nyt saatavilla Azure palvelun Bus sekä palvelun Bus for Windows Server (Service Bus 1.1). Hinnat on sama kuin aiemmin protokollat.

## <a name="next-steps"></a>Seuraavat vaiheet

Daxista on seuraavissa linkeissä lisätietoja palvelun Bus AMQP tuki.

*    [Voit käyttää AMQP 1.0 palvelun Bus .NET-Ohjelmointirajapinta](service-bus-dotnet-advanced-message-queuing.md)
*    [Palvelun Bus & AMQP 1.0 Java viestin Service (JMS) Ohjelmointirajapinnan käyttäminen](service-bus-java-how-to-use-jms-api-amqp.md)
*    [Palvelun Bus AMQP Sovelluskehittäjän opas][]
*    [OASIS laajennettu viesti Queuing Protocol (AMQP) versio 1.0 määritys](http://docs.oasis-open.org/amqp/core/v1.0/os/amqp-core-complete-v1.0-os.pdf)

[0]: ./media/service-bus-java-amqp-overview/Example1.png
[Palvelun Bus AMQP Sovelluskehittäjän opas]: service-bus-amqp-dotnet.md

 
