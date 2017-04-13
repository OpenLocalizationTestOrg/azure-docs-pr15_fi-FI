<properties
   pageTitle="HTTP-kuuntelutoiminnon ja yhdistimen käyttäminen logiikan sovellusten | Sovelluksen Microsoft Azure-palvelu "
   description="Voit luoda ja HTTP-kuuntelutoiminnon ja HTTP-toiminnon yhdistimen tai API sovelluksen määrittäminen ja käyttäminen logiikan-sovellus App Azure-palvelussa"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="anuragdalmia"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="08/31/2016"
   ms.author="prkumar"/>


# <a name="get-started-with-the-http-listener-and-http-action-and-add-it-to-your-logic-app"></a>HTTP-kuuntelutoiminnon ja HTTP-toiminnon käytön aloittaminen ja lisää se logiikan-sovellukseen

> [AZURE.NOTE]Microsoft on pääte Tämä yhdistin tuki, koska sen toiminnot nyt sisältyy oletusarvoisesti kuin **manuaalinen käynnistäminen** kun luot uuden logiikan sovellukset.  Suosittelemme, että kaikki logiikan sovellukset, jotka käyttävät tätä connector päivittää.  
> Tässä versiossa on artikkelissa koskee logiikan sovellusten 2014 – 12-01 – esikatselu rakenteen versio.

Yhdistä suoraan HTTP resurssien kuunnella pyyntöjen ja määritä HTTP-web-pyynnöt. Joissakin tilanteissa, joissa voit joutua toimimaan HTTP suorat yhteydet, mukaan lukien on:

1.  Voit tehdä logiikan, joka tukee matkapuhelinkäyttäjää vuorovaikutteinen edusta- tai web App.
2.  Voit hakea ja käsitellä tietoja verkkopalvelun, joka ei ole poissa-ruutuun yhdistin.
3.  Voit suorittaa toimintoja, jotka ovat jo näkyviä web-palvelu, mutta ei ole käytettävissä API sovelluksena.

Näissä tilanteissa on kaksi vaihtoehtoa:

1. **HTTP-kuuntelutoiminnon**: Tämä yhdistin toimii käynnistin ja määrittänyt päätepisteen HTTP pyyntöjen seuraa. Kun puhelu on vastaanotettu määritetyn päätepisteen, käynnistää kulun uuden esiintymän ja välittää pyynnön käsittelyä varten pystysuuntaiseksi vastaanotetut tiedot. Se voidaan määrittää myös vastaa automaattisesti saapuvan pyynnön, kun työnkulku on aloitettu tai avulla voit luoda työnkulun suorittamisen perusteella vastauksen ja lähettää kutsuja.
2. **HTTP-toiminto**: Tämän avulla voit määrittää WWW-pyynnön käytettävissä minkä tahansa päätepistettä Internetissä, saa takaisin vastauksen ja mahdollistaa sen käytön lisätoimintoja tarjoaman työnkulussa.

Logiikan sovellukset voivat aiheuttaa erilaisia tietolähteitä ja voit hakea ja käsitellä tietoja osana kulun yhdistimet tarjouksen perusteella. Voit lisätä HTTP-yhdistin yrityksen työnkulku ja prosessin tietoja tämän työnkulun logiikan-sovelluksen osana. 

## <a name="creating-an-http-listener-for-your-logic-app"></a>HTTP-kuuntelutoiminnon logiikan-sovelluksen luominen
Yhdistimen logiikan-sovelluksen sisällä voidaan luoda tai luodaan suoraan Azure Marketplacesta. Voit luoda yhdistimen Marketplace-sivulla seuraavasti:  

1. Valitse Azure startboard **Marketplacesta**.
2. Etsi "HTTP", valitse HTTP-kuuntelutoiminnon ja valitse **Luo**.
3.  Määritä HTTP-kuuntelutoiminnon seuraavasti:  
![][1]

4.  Järjestäessäsi pakettiasetukset näkyviin tulee seuraava asetus onko kuuntelua tulee vastata automaattisesti tai edellyttää lähettämään eksplisiittinen vastauksen. Aseta arvoksi **False** Lähetä oma vastauksesi:  
![][2]

5.  Luo valitsemalla **OK** .
6.  Kun API-sovelluksen esiintymää on luotu, Avaa asetukset suojauksen määrittäminen. HTTP-kuuntelutoiminnon tukee tällä hetkellä Perustodentamista. Voit määrittää suojaus-vaihtoehtoa käytettäessä avattaessa HTTP-kuuntelutoiminnon:  
![][3]
  
    **Tunnettu ongelma** *Tietoturva-asetukset näytetään "Ei", jos oletusarvoksi, mutta se ei ole määritetty. Sinun on muutettava asetusta, Basic ja takaisin ei mitään, ennen kuin tallennat sen varmistamiseksi, että HTTP-kuuntelutoiminnon on määritetty oikein.*  

7. API-sovelluksen suojausasetusten määrittäminen lopuksi yleiseen (anonyymi), jos haluat sallia ulkoisten asiakkaat voivat käyttää päätepiste. Tämä asetus on käytettävissä "kaikki asetukset > Asetukset" HTTP Listener API-sovelluksen:![][10]

Sen jälkeen voit nyt luoda logiikan-sovelluksen käyttämään HTTP-kuuntelutoiminnon.

## <a name="using-the-http-listener-in-your-logic-app"></a>HTTP-kuuntelutoiminnon käyttäminen logiikan sovelluksen
Kun API-sovellus on luotu, voit nyt käyttää HTTP-kuuntelutoiminnon käynnistämisen logiikan-sovelluksen. Voit tehdä tämän on:

4.  Logiikan uuden sovelluksen luominen.
5.  Avaa "Käynnistimien toimintojen" Avaa logiikan sovellusten suunnittelu ja tapa, että työnkulku. HTTP-kuuntelutoiminnon näkyy valikoimasta. Valitse se.
6.  Voit nyt määrittää HTTP-menetelmä ja suhteellinen URL-osoite, tarvitset käynnistettävän kulun listener:  
![][4]  
![][5]

7.  Saat valmis URI-ruudussa sen Hakumääritysten asetusten tarkasteleminen ja kopioi URL-osoite "Isännän" API-sovelluksesi HTTP-kuuntelutoiminnon:  
![][6]
8.  Voit nyt käyttää data vastaanotettu kulun muita toimintoja HTTP-pyynnössä seuraavasti:  
![][7]  
![][8]
9.  Lopuksi lähetettävä vastaus Lisää toinen HTTP-kuuntelutoiminnon ja valitse Lähetä HTTP-vastaus-toiminto. Määrittää pyytää tunnus HTTP-kuuntelutoiminnon saatu Pyyntötunnus ja täytä vastauksen leipätekstistä ja haluat palata takaisin HTTP-tila:  
![][9]

## <a name="using-the-http-action"></a>HTTP-toiminnon avulla
HTTP-toiminto tukee grafiikkatiedostomuotoja logiikan sovellukset ja ei edellytä API-sovelluksen luominen ensimmäisen voivat käyttää sitä. Voit lisätä HTTP-toiminnon logiikan sovelluksen milloin tahansa ja valita URI-, ylä- ja leipätekstin puheluun.
HTTP-toiminto tukee useita vaihtoehtoja asiakkaan puoli suojaus. Katso [asiakkaan puoli tietoturva-asetukset](../scheduler/scheduler-outbound-authentication.md).

HTTP-toiminnon tulos on ylä- ja leipätekstin, jonka avulla voidaan edelleen myötävirtaan samalla kuinka muut toimet ja yhdysviivat tulosteen kulutettu työnkulussa.

## <a name="do-more-with-your-connector"></a>Älä lisää ja sitten yhdistimen
Yhdistimen on luotu, voit lisätä business työnkulkuun logiikan sovellusta käytettäessä. Katso [mitä logiikan sovelluksia?](app-service-logic-what-are-logic-apps.md).

Tarkastele Swagger REST API-viittaus [yhdistimiä](http://go.microsoft.com/fwlink/p/?LinkId=529766)ja API sovellusten viittaus.

Voit myös tarkastella suorituskyvyn Tilasto- ja tietoturva yhdistin. Katso [hallinta ja valvonta valmiin API-sovellusten ja yhdistimet](app-service-logic-monitor-your-connectors.md).

> [AZURE.NOTE] Jos haluat aloittaa Azure logiikan sovellukset ennen rekisteröimässä Azure-tili, siirry [Yritä logiikan-sovellukseen](https://tryappservice.azure.com/?appservice=logic). Voit luoda lyhytkestoinen starter logiikan-sovelluksen heti sovelluksen-palvelussa. Ei ole pakollinen; luottokortit ei ole sitoumukset.

<!--Image references-->
[1]: ./media/app-service-logic-connector-http/1.png
[2]: ./media/app-service-logic-connector-http/2.png
[3]: ./media/app-service-logic-connector-http/3.png
[4]: ./media/app-service-logic-connector-http/4.png
[5]: ./media/app-service-logic-connector-http/5.png
[6]: ./media/app-service-logic-connector-http/6.png
[7]: ./media/app-service-logic-connector-http/7.png
[8]: ./media/app-service-logic-connector-http/8.png
[9]: ./media/app-service-logic-connector-http/9.png
[10]: ./media/app-service-logic-connector-http/10.png
