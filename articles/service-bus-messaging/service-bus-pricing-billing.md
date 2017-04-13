<properties 
    pageTitle="Palvelun Bus hinnat ja laskutuksen | Microsoft Azure"
    description="Palvelun Bus hinnat rakenteen yleiskatsaus."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/06/2016"
    ms.author="sethm" />

# <a name="service-bus-pricing-and-billing"></a>Palvelun Bus hinnat ja laskutus

Palvelun Bus tarjotaan Basic, Vakio ja [Premium](service-bus-premium-messaging.md) tasoa. Voit valita kunkin palvelun Bus palvelun nimitilan, jonka luot palvelutaso ja kaikki kohteet, jotka on luotu, nimitilan soveltaa tämä taso-valinta.

>[AZURE.NOTE] Lisätietoja nykyinen palvelun Bus hinnoittelu artikkelissa [Azure palvelun Bus hinnat sivun](https://azure.microsoft.com/pricing/details/service-bus/)ja [Palvelun Bus usein kysytyt kysymykset](service-bus-faq.md#service-bus-pricing).

Palvelun Bus käyttää seuraavat kaksi metriä olevien ja aiheet/tilaukset:

1. **Viestintä toiminnot**: API-kutsujen vastaan jonossa tai aiheen/tilauksen päätepisteiden lasketaan. Tämä mittari korvaa saanut laskutettavan käyttö olevien ja aiheet/tilaukset ensisijainen yksikkö tai lähetetyt viestit.

2. **Se yhteydet**: määritetty pysyvät yhteydet piikin määrän Avaa olevien, aiheet tai tilaukset annetun tunnin esimerkkejä jaksolla. Tämä mittari koskevat vain vakio taso, jonka voit avata muita yhteydet-(aiemmin yhteydet on rajoitettu tilauskohtaisten jonossa/aiheen/100) korko.vuosi yhteyttä kohti maksua vastaan.

**Vakio** taso esitellään toimintoja suoritetaan olevien ja aiheet/tilaukset, tuloksena on aseman perustuva alennuksia 80 % suurin käyttö tasoilla: n hinnat. On myös vakio taso perus maksu 10 kuukaudessa, jonka avulla voit suorittaa 12,5 miljoonaa kuukaudessa ilman lisäkustannuksia.

**Premium** taso sisältää resurssin eristystaso suorittimen ja muistin tasolla niin, että kunkin asiakkaan työmäärää suoritetaan yksinään. Tämän resurssin säilön kutsutaan *messaging yksikkö*. Kunkin premium nimitila on varattu vähintään yhden tekstiviesti yksikön. Voit hankkia 1, 2 tai 4 tekstiviesti yksiköt kunkin palvelun Bus Premium nimitilan. Kohteen tai yksittäisen kuormituksen voi olla useita tekstiviesti yksiköt ja tekstiviesti yksiköiden määrän, voi muuttaa milloin, vaikka laskutus on 24 tunnin tai päivittäin maksut. Tulos on ennakoitavissa ja toistettavien suorituskyvyn palvelun Bus-ratkaisuun. Paitsi on tämän suorituskyvyn ennakoitavissa ja käytettävissä, mutta myös on nopeampaa. Azure palvelun Bus Premium messaging perustuu Azure tapahtuman keskittimet käyttöön tallennustilan ohjelma. Premium messaging, Huippu suorituskyky on paljon aiempaa nopeammin kuin Vakio taso.

Huomaa, että vakio perus maksu veloitetaan kuukaudessa Azure tilauskohtaisten vain kerran. Tämä tarkoittaa, että kun olet luonut yhden vakio- tai Premium taso palvelun Bus nimitilan, osaat luoda niin monta muita vakio- tai Premium taso nimitilan sellaisena kuin haluat, että sama Azure tilauksen, valitse ilman muita perus lisäkustannuksia.

Kaikki aiemmin palvelun Bus nimitilan ennen 1. marraskuussa 2014 luodaan automaattisesti poistettavan kyselyjä vakio taso. Näin varmistat, että voit edelleen käyttää kaikkia ominaisuuksia, jotka ovat tällä hetkellä käytettävissä palvelun Bus. Voit myöhemmin muuntaminen Basic taso halutessasi [Azure perinteinen portal][] .

Seuraavassa taulukossa on yhteenveto Basic- ja standardin/Premium tasoa toiminnalliset erot.

|Ominaisuus|Perustoiminnot|Vakio/Premium|
|---|---|---|
|Olevien|Kyllä|Kyllä|
|Aikataulun mukainen viestit|Kyllä|Kyllä|
|Aiheet/tilaukset|Ei|Kyllä|
|Releitä|Ei|Kyllä|
|Tapahtumat|Ei|Kyllä|
|Jään monistaminen|Ei|Kyllä|
|Istunnot|Ei|Kyllä|
|Suuret viestit|Ei|Kyllä|
|ForwardTo|Ei|Kyllä|
|SendVia|Ei|Kyllä|
|Se yhteydet (sisältyy)|100 euroa palvelun Bus nimitila|1 000 Azure tilausta kohti|
|Se yhteydet (kattavuus sallittuja)|Ei|Kyllä (laskutettava)|

## <a name="messaging-operations"></a>Tekstiviesti-toiminnot

Laskutus olevien ja aiheet/tilaukset muuttuu osana hinnoittelu uusi malli. Nämä objektit siirtymässä kohti viestin Laskutus-toimintoa kohden laskutus. "Toiminto" viittaa minkä tahansa API-kutsu vastaan jonossa tai aiheen/tilauksen palvelupäätepiste. Tämä sisältää hallintaa, Lähetä tai vastaanota ja istunnon tila-toimintoja.

|Toiminto|Kuvaus|
|---|---|
|Hallinta|Luominen, lukeminen, päivitys, poistaminen (CRUD) olevien tai aiheet/tilaukset.|
|Viestintä|Viestien lähettäminen ja vastaanottaminen olevien tai aiheet/tilaukset.|
|Istunnon tila|Käytön tai jonossa tai aiheen/tilauksen käyttöoikeuksien määrittäminen istunnon tila.|

Seuraavat hinnat on tehokas 1. marraskuussa 2014 alkaen:

|Perustoiminnot|Kustannukset|
|---|---|
|Toiminnot|0,05 miljoonaa toimintojen kohden|

|Vakio|Kustannukset|
|---|---|
|Perus maksu|10/ kuukausi|
|Ensin 12,5 miljoonaa toiminnot-, kuukausi|Levy|
|12,5 – 100 miljoonaa toimintoja, kuukausi|0,80 miljoonaa toimintojen kohden|
|100 miljoonaa - 2 500 miljoonaa toimintoja, kuukausi|0,50 miljoonaa toimintojen kohden|
|Yli 2 500 miljoonaa toimintoja, kuukausi|0,20 miljoonaa toimintojen kohden|

|Premium|Kustannukset|
|---|---|
|Päivittäinen|$11.13 kiinteä korvauksen viestin yksikkö|

## <a name="brokered-connections"></a>Se yhteydet

*Brokered yhteydet* mahtuu asiakkaan käyttötavat, joihin sisältyy suuri määrä "pysyvästi yhdistetty" lähettäjien/vastaanottajia vastaan olevien, aiheet ja tilaukset. Pysyvästi yhdistetyn lähettäjien ja vastaanottajien ovat ne, jotka muodostaa yhteyden joko AMQP tai HTTP ja muuta kuin nolla vastaanottaa aikakatkaisu (esimerkiksi HTTP pitkä kyselyt). HTTP-lähettäjien ja vastaanottajien heti aikakatkaisu ja luo se yhteydet.

Enintään 100 samanaikaista yhteyttä kohti URL-osoite oli aiemmin, olevien ja aiheet/tilaukset. Nykyisen laskutuksen valikoiman poistaa-URL rajoitus olevien ja aiheet/tilaukset ja toteuttaa kiintiöiden ja niin, että se yhteyden palvelun Bus nimitilan ja Azure tilauksen tasoilla.

Perustiedot taso sisältää ja on ehdottoman rajoitettu, 100 se yhteydet palvelun Bus nimitilan kohden. Yhteyksien määrä edellä hylätään Basic taso. Vakio taso poistaa nimitilan kohti rajan ja laskee kooste se yhteyden käyttö Azure tilauksen yli. Vakio taso 1 000 se yhteydet Azure tilauskohtaisten sallitaan osoitteessa ei ole ylimääräisiä kustannukset (lisäksi perus kulu). Käyttö yli 1 000 se yhteydet korkeintaan Standard tason palvelun Bus Azure tilauksen nimitilan laskutetaan: n aikataulussa, seuraavassa taulukossa kuvatulla tavalla.

|Se yhteydet (vakio taso)|Kustannukset|
|---|---|
|Ensimmäinen 1 000, kuukausi|Perus maksu mukana|
|1 000 – 100 000, kuukausi|0.03 yhteyden/kk|
|500 000-100 000, kuukausi|0.025 yhteyden/kk|
|500 000 tai kuukauden kohdalle|0.015 yhteyden/kk|

>[AZURE.NOTE] 1 000 se yhteydet kuuluvat vakio tekstiviesti taso (joko perus kulu), ja ne voidaan jakaa olevien, aiheet ja liittyvän Azure-tilauksen piiriin kuuluvien tilaukset.

<br />

>[AZURE.NOTE] Laskutuksen samanaikaisia yhteyksiä piikin määrän perusteella ja on jaettu kuukauden 744 tuntien perusteella kerran tunnissa.

|Premium taso
|---|
|Se yhteydet peritään ei Premium taso.|

Lisätietoja se yhteydet tämän artikkelin kohdassa [usein kysyttyjä Kysymyksiä](#faq) .

## <a name="relay"></a>Välitys

Releitä ovat käytettävissä vain vakio taso nimitilan. Muussa tapauksessa hinnat ja yhteyden kiintiöt releitä pysyy muuttumattomana. Tämä tarkoittaa, että releitä jatkaa veloitetaan viestien (ei toimintoja) määrän ja välittää tuntia.

|Välittää hinnat|Kustannukset|
|---|---|
|Välitys tunnit|0,10 $ 100 välitys tunnin välein|
|Viestit|$0,01, joka 10 000 viestistä|

## <a name="faq"></a>USEIN KYSYTYT KYSYMYKSET

### <a name="how-is-the-relay-hours-meter-calculated"></a>Miten välitys tuntia mittarin lasketaan?

[Tässä](service-bus-faq.md#how-is-the-relay-hours-meter-calculated)aiheessa.

### <a name="what-are-brokered-connections-and-how-do-i-get-charged-for-them"></a>Mitä se yhteydet ja kuinka voin Hae perittävän ne?

Se yhteyden määritellään seuraavasti:

1. AMQP yhteys asiakaskoneesta palvelun Bus jonossa tai aiheen tai tilaus.

2. HTTP kutsu haluat vastaanottaa viestin palvelun Bus aiheen tai jono, joka on Vastaanota aikakatkaisuarvo on suurempi kuin nolla.

Palvelun Bus kulut piikin määrälle samanaikaisia se yhteyksiä, jotka ylittävät mukana määrä (vakio taso 1 000). Päät mitattuna tunnissa, jaettu jakamalla 744 tuntia kuukaudessa ja yhteenlaskettu kuukausittain laskutusjaksolla päälle. Sisältää määrä (1 000 se yhteyksiä kk) otetaan käyttöön vastaan summan mukaan jaettu tunnin välein päät laskutusjaksolla lopussa.

Esimerkki:

1. Kunkin 10 000 laitteiden muodostaa yhden AMQP-yhteyden kautta ja niistä komennot palvelun Bus aiheen. Laitteet Lähetä telemetriatietojen tapahtumien tapahtumaa-toiminnossa. Jos kaikki laitteet 12 tuntia joka päivä, seuraavat yhteys-ovat voimassa (lisäksi muita palvelun Bus aiheen maksuja): 10 000 yhteydet *12 tuntia* 31 päivän ajalta / 744 = 5 000 se yhteydet. 1 000 se yhteydet kuukausittain alennuksen jälkeen voit veloitetaan 4 000 se yhteyksien otettujen 0.03 se yhteyden 120 euroa yhteensä kohden.

2. 10 000 laitteiden vastaanottaa viestejä palvelun Bus jonon HTTP-määrittäminen nollasta aikakatkaisu. Jos kaikki laitteet 12 tuntia joka päivä, näet seuraavat yhteyden maksut (lisäksi muita palvelun Bus-kulut): 10 000 HTTP tulee yhteydet *12 tuntia päivässä* 31 päivän ajalta / 744 tuntia = 5 000 se yhteydet.

### <a name="do-brokered-connection-charges-apply-to-queues-and-topicssubscriptions"></a>Se yhteyden ovat voimassa olevien ja aiheet/tilaukset?

Kyllä. Veloituksia ei ole yhteyttä lähettämiseen tapahtumien http:n määrän lähettäminen järjestelmien tai laitteiden riippumatta. Tapahtumien vastaanottaminen HTTP käyttämällä aikakatkaisu, joka on suurempi kuin nolla, niin sanottu "pitkä kyselyt", luo se yhteyden ovat. AMQP yhteydet luo se yhteyden ovat riippumatta siitä, onko yhteys käytetään lähettämiseen tai vastaanottamiseen. Huomaa, että 100 se yhteydet sallitaan Basic nimitilan maksutta. Tämä on myös se Azure-tilausta yhteyksien enimmäismäärä. Ensimmäiset 1 000 se yhteyksiä eri kaikki vakio nimitilan Azure tilauksen-sisältyvät ylimääräiset maksutta (lisäksi perus kulu). Nämä korvaukset on riittävän peittää palvelun tekstiviesti skenaarioita, koska se yhteyden ovat yleensä vain muuttuvat merkitystä, jos aiot käyttää AMQP- tai HTTP long-kysely suuri määrä asiakkaita. Jos esimerkiksi toteuttaa tehokkaammin tapahtumaa streaming tai ottaa käyttöön kaksisuuntaisen useiden laitteiden tai sovellusten esiintymiä kanssa.

## <a name="next-steps"></a>Seuraavat vaiheet

- Saat lisätietoja palvelun Bus hinnat [Bus Azure-palvelun hinnat sivun](https://azure.microsoft.com/pricing/details/service-bus/).

- Artikkelissa [Palvelun Bus usein kysytyt kysymykset](service-bus-faq.md#service-bus-pricing) joitakin yleisiä usein kysyttyjä kysymyksiä palvelun bus hinnat ja laskutuksen ympärille.

[Azure perinteinen portal]: http://manage.windowsazure.com