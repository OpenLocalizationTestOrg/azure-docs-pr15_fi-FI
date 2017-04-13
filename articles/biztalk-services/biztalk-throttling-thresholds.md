<properties 
    pageTitle="Lisätietoja rajoittimen BizTalk Services-palveluissa | Microsoft Azure" 
    description="Tietoja rajoittimen raja-arvot ja runtime toiminnan tuloksena BizTalk Services. Rajoittaminen perustuu muistinkäytön ja viestien määrä. MAB-WABS" 
    services="biztalk-services" 
    documentationCenter="" 
    authors="MandiOhlinger" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="biztalk-services" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="mandia"/>





# <a name="biztalk-services-throttling"></a>BizTalk Services: rajoittaminen

Azure BizTalk Services toteuttaa palvelun rajoitusten perusteella kaksi ehtoa: muistinkäytön ja käsittelyn samanaikaisesti viestien määrä. Tässä artikkelissa luetellaan rajoittava raja-arvot ja kuvataan Runtime toiminnan, kun rajoittava ehto toteutuu.

## <a name="throttling-thresholds"></a>Rajoittimen raja-arvot

Seuraavassa taulukossa on lueteltu rajoittava lähde- ja kynnysarvot:

||Kuvaus|Alaraja|Suuri kynnysarvo|
|---|---|---|---|
|Muisti|järjestelmän muistia käytettävissä/PageFileBytes %. <p><p>Yhteensä käytettävissä PageFileBytes on noin 2 kerrotaan luvulla järjestelmän RAM.|60 %|70 %|
|Viestin käsittely|Viestien käsitteleminen samanaikaisesti määrä|40 * määrä sydämiä|100 * määrä sydämiä|

Kun suuri raja-arvo on saavutettu, Azure BizTalk Services alkaa rajoita. Rajoittimen pysähtyy, kun pienen raja-arvo on saavutettu. Esimerkiksi palvelun käyttää 65 muistia. Tässä tilanteessa palvelu ei ole rajoita. Palvelu käynnistyy käyttämällä 70 % muistia. Tässä tilanteessa palvelun throttles ja jatkaa rajoita, kunnes palvelu käyttää 60 % (alaraja) muistia.

Azure BizTalk Services seuraa rajoittava tila (normaali tila ja kyselyn tila) ja rajoittava kesto.


## <a name="runtime-behavior"></a>Suorituksenaikainen toiminta

Kun Azure BizTalk Services siirtyy rajoittava tila, tapahtuu seuraavaa:

- Rajoitus on roolin esiintymän kohden. Esimerkki:<br/>
RoleInstanceA on rajoitin. RoleInstanceB ei rajoitusta. Tässä tilanteessa RoleInstanceB viestit käsitellään odotetulla tavalla. Viestien RoleInstanceA jätetään pois, ja epäonnistua seuraavan virheen vuoksi:<br/><br/>
**Palvelin on varattu. Yritä uudelleen.**<br/><br/>
- Erotettu kaikki lähteet eivät äänestyksen järjestäminen tai Lataa viestin. Esimerkki:<br/>
Putkijohto hakee viestien FTP ulkoisesta tietolähteestä. Roolin esiintymän tekeminen erotettu saa rajoittava vaiheeseen. Tässä tilanteessa putkijohto lopettaa lataaminen uusien viestien, kunnes pysäytetään roolin esiintymän rajoitusta.
- Vastaus lähetetään asiakkaalle, jolloin asiakas voi lähettää viestin.
- Odota, kunnes rajoitus on ratkennut. Tarkemmin sanottuna on odotettava, kunnes pienen raja-arvo on saavutettu.

## <a name="important-notes"></a>Tärkeitä muistiinpanoja
- Rajoittaminen ei voi poistaa käytöstä.
- Raja-arvot rajoittaminen ei voi muokata.
- Rajoitus on toteutettu järjestelmää.
- Azure SQL-tietokantapalvelimen on myös valmiin rajoitusta.

## <a name="additional-azure-biztalk-services-topics"></a>Azure BizTalk Services aiheisiin

-  [Azure BizTalk Services SDK asentaminen](http://go.microsoft.com/fwlink/p/?LinkID=241589)<br/>
-  [Oppaat: Azure BizTalk palvelut](http://go.microsoft.com/fwlink/p/?LinkID=236944)<br/>
-  [Miten voin aloittaa käyttämällä Azure BizTalk Services SDK-paketissa](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
-  [Azure BizTalk palvelut](http://go.microsoft.com/fwlink/p/?LinkID=303664)<br/>

## <a name="see-also"></a>Katso myös
- [BizTalk Services: Kehittäjä, Basic, Vakio ja Premium versiot kaavio](http://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
- [BizTalk Services: Valmistelu käyttämällä Azure perinteinen portal](http://go.microsoft.com/fwlink/p/?LinkID=302280)<br/>
- [BizTalk Services: Tila-kaavion valmistelu](http://go.microsoft.com/fwlink/p/?LinkID=329870)<br/>
- [BizTalk Services: Raporttinäkymät-ikkunan, näyttö ja skaalaus-välilehdet](http://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
- [BizTalk Services: Varmuuskopiointi ja palauttaminen](http://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
- [BizTalk Services: Myöntäjän nimi ja myöntäjä avain](http://go.microsoft.com/fwlink/p/?LinkID=303941)<br/>
 
