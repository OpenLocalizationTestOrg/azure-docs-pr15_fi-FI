<properties
    pageTitle="Lisätietoja ominaisuuksista BizTalk palvelut-versioissa | Microsoft Azure"
    description="BizTalk Services-versiot ominaisuuksien vertailu: vapaa, Developer, Basic, Vakio ja Premium. MAB-WABS."
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
    ms.topic="get-started-article"
    ms.date="08/15/2016"
    ms.author="mandia"/>


# <a name="biztalk-services-editions-chart"></a>BizTalk Services: Versiot kaavio

Azure BizTalk-palvelut on eri versiot. Tämän artikkelin avulla voit määrittää, mikä versio sopii skenaario ja business tarpeitasi.


## <a name="compare-the-editions"></a>Versioiden vertailu

**Vapaa (ennakkoversio)**

Voit luoda ja hallita Hybrid yhteyksiä. Hybrid yhteyden on helppo tapa muodostaa paikalliseen järjestelmään, kuten SQL Server Azure verkkosivustosta.

**Developer**

Sisältää Hybrid yhteyksiä, EAI ja Muokkaa käsittely helposti käytettävällä kaupan kumppanin hallinta-portaalin ja yleisiä Muokkaa rakenteita tuki- ja rich Muokkaa käsittely X12 ja AS2. Voit luoda yleisiä EAI tilanteita, joissa yhdistämällä services pilveen minkä tahansa HTTP/S, muille käyttäjille, FTP, WCF ja SFTP protokollat lukemiseen ja kirjoittamiseen viestejä.  Käytä yhteys paikalliseen LOB-järjestelmiin valmis avulla SAP, Oracle-verkkoliiketoiminnan, Oracle DB, Siebel ja SQL Server sovittimien. Käyttää keskitettyä Kehitystyökalut-ympäristön Visual Studio työkaluja helposti kehittämällä ja. Rajoitettu kehittäminen ja testaa tarkoituksiin vain kanssa ei palvelussa palvelutasosopimusta (SLA).

**Perustoiminnot**

Sisältää Hybrid yhteydet, EAI siltoja, muokkaa sopimusten ja BizTalk sovittimen Pack yhteydet yleensä Kehitystyökalut-ominaisuuksien avulla. On myös suuren käytettävyyden ja asetus, jos haluat skaalata kanssa palvelussa palvelutasosopimusta (SLA).

**Vakio**

Sisältää kaikki perusominaisuudet avulla Hybrid yhteydet, EAI siltoja, muokkaa sopimusten ja BizTalk sovittimen Pack yhteydet. On myös suuren käytettävyyden ja asetus, jos haluat skaalata kanssa palvelussa palvelutasosopimusta (SLA).

**Premium**

Sisältää Hybrid yhteydet, EAI siltoja, muokkaa sopimusten ja BizTalk sovittimen Pack yhteydet vakio ominaisuuksien avulla. Myös arkistointi, suuri käytettävyys ja asetus, jos haluat skaalata kanssa palvelussa palvelutasosopimusta (SLA).


## <a name="editions-chart"></a>Versiot-kaavio
Seuraavassa taulukossa on lueteltu erot.

<table border="1">
<tr bgcolor="FAF9F9">
        <th></th>
        <th>Vapaa (ennakkoversio)</th>
        <th>Developer</th>
        <th>Perustoiminnot</th>
        <th>Vakio</th>
        <th>Premium</th>
</tr>

<tr>
<td><strong>Aloitus-hinta</strong></td>
<td colspan="5"><a HREF="http://go.microsoft.com/fwlink/p/?LinkID=304011"> Azure BizTalk palvelujen hinnat</a> <br/><br/> <a HREF="http://azure.microsoft.com/pricing/calculator/?scenario=full">Azure hinnat laskuri</a></td>
</tr>
<tr>
<td><strong>Pienin oletusasetukset</strong></td>
<td>1 vapaa yksikkö</td>
<td>1 developer yksikkö</td>
<td>1 basic yksikkö</td>
<td>1 vakio yksikkö</td>
<td>1 premium yksikkö</td>
</tr>
<tr>
<td><strong>Asteikko</strong></td>
<td>Ei mittakaavaa</td>
<td>Ei mittakaavaa</td>
<td>Kyllä, 1 Basic yksikön kerrallaan.</td>
<td>Kyllä, 1 vakio yksikön kerrallaan.</td>
<td>Kyllä, 1 Premium yksikön kerrallaan.</td>
</tr>
<tr>
<td><strong>Suurin sallittu ulos</strong></td>
<td>Ei mittakaavaa</td>
<td>Ei mittakaavaa</td>
<td>Enintään 8 yksikköä</td>
<td>Enintään 8 yksikköä</td>
<td>Enintään 8 yksikköä</td>
</tr>
<tr>
<td><strong>EAI siltoja kohti</strong></td>
<td>Ei sisälly sovellukseen</td>
<td>25</td>
<td>25</td>
<td>125</td>
<td>500</td>
</tr>
<tr>
<td><strong>MUOKKAA AS2</strong>
<br/><br/>
Sisältää TPM toimeenpano</td>
<td>Ei sisälly sovellukseen</td>
<td>Levy. 10 toimeenpano kohti.</td>
<td>Ohjelmistopakettia. 50 toimeenpano kohti.</td>
<td>Ohjelmistopakettia. 250 toimeenpano kohti.</td>
<td>Ohjelmistopakettia. 1000 toimeenpano kohti.</td>
</tr>
<tr>
<td><strong>Hybrid yhteydet kohti</strong></td>
<td>5</td>
<td>5</td>
<td>10</td>
<td>50</td>
<td>100</td>
</tr>
<tr>
<td><strong>Hybrid yhteyden tiedonsiirto (gt) kohti</strong></td>
<td>5</td>
<td>5</td>
<td>50</td>
<td>250</td>
<td>500</td>
</tr>
<tr>
<td><strong>BizTalk sovittimen palvelun yhteydet paikallisen LOB-järjestelmiin</strong></td>
<td>Ei sisälly sovellukseen</td>
<td>1 yhteys</td>
<td>2 yhteydet</td>
<td>5 yhteydet</td>
<td>25 yhteydet</td>
</tr>
<tr>
<td align="left"><strong>Tuetut protokollat/järjestelmät:</strong>
<ul>
<li>HTTP</li>
<li>HTTPS</li>
<li>FTP</li>
<li>SFTP</li>
<li>WCF</li>
<li>Palvelun Bus (SB)</li>
<li>Azure-Blob</li>
<li>REST API</li>
</ul>
</td>
<td>Ei sisälly sovellukseen</td>
<td>Ohjelmistopakettia</td>
<td>Ohjelmistopakettia</td>
<td>Ohjelmistopakettia</td>
<td>Ohjelmistopakettia</td>
</tr>
<tr>
<td><strong>Suuri käytettävyys</strong>
<br/><br/>
Saat palvelussa palvelutasosopimusta (SLA), on artikkelissa <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=304011">Azure BizTalk palvelujen hinnat</a>.
</td>
<td>Ei sisälly sovellukseen</td>
<td>Ei sisälly sovellukseen</td>
<td>Ohjelmistopakettia</td>
<td>Ohjelmistopakettia</td>
<td>Ohjelmistopakettia</td>
</tr>
<tr>
<td><strong>Varmuuskopiointi ja palauttaminen</strong></td>
<td>Ei sisälly sovellukseen</td>
<td>Levy</td>
<td>Levy</td>
<td>Ohjelmistopakettia</td>
<td>Ohjelmistopakettia</td>
</tr>
<tr>
<td><strong>Seuranta</strong></td>
<td>Ei sisälly sovellukseen</td>
<td>Ohjelmistopakettia</td>
<td>Ohjelmistopakettia</td>
<td>Levy</td>
<td>Ohjelmistopakettia</td>
</tr>
<tr>
<td><strong>Arkistointi</strong><br/><br/>
Sisältää ei-hylkäämistä vastaanottoa (NRR) ja seurattujen viestien lataaminen</td>
<td>Ei sisälly sovellukseen</td>
<td>Ohjelmistopakettia</td>
<td>Ei sisälly sovellukseen</td>
<td>Ei sisälly sovellukseen</td>
<td>Ohjelmistopakettia</td>
</tr>
<tr>
<td><strong>Mukautetun koodin</strong></td>
<td>Ei sisälly sovellukseen</td>
<td>Ohjelmistopakettia</td>
<td>Ohjelmistopakettia</td>
<td>Ohjelmistopakettia</td>
<td>Ohjelmistopakettia</td>
</tr>
<tr>
<td><strong>Muunnokset, kuten mukautetun XSLT käyttäminen</strong></td>
<td>Ei sisälly sovellukseen</td>
<td>Ohjelmistopakettia</td>
<td>Ohjelmistopakettia</td>
<td>Ohjelmistopakettia</td>
<td>Ohjelmistopakettia</td>
</tr>
</table>

> [AZURE.NOTE] Vikasietoisuudelle vastaan laitteisto-suuren käytettävyyden merkitsee on useita VMs BizTalk yksikkönä kuluessa.


## <a name="faqs"></a>Usein kysyttyjä kysymyksiä

#### <a name="what-is-a-biztalk-unit"></a>Mikä on BizTalk yksikköä?
"Yksikköä" on Azure BizTalk palvelut-käyttöönoton atomisia taso. Kunkin version sisältyy yksikkö, jossa on eri Laske kapasiteetti ja muistin. Esimerkiksi Basic yksikkö on enemmän kuin Developer suorittaminen, vakio on enemmän kuin tavallinen suorittaminen ja niin edelleen. Kun BizTalk-palvelun skaalata, skaalata myyntialueella.

#### <a name="what-is-the-difference-between-biztalk-services-and-azure-biztalk-vm"></a>Mitä eroa BizTalk palvelut ja Azure BizTalk AM?
BizTalk-palvelut on tosi ympäristö nimellä--palvelun (PaaS)-arkkitehtuuri integrointi ratkaisujen pilveen. PaaS-malli keskittyä sovelluksen logiikkaa kokonaan ja jätä kaikista infrastruktuuriin hallinta Microsoftille, mukaan lukien:

- Voit hallita tai korjaustiedoston näennäiskoneiden ei tarvita.
- Microsoft varmistaa käytettävyyttä.
- Voit hallita asteikko pyydettäessä pyytämällä yksinkertaisesti enemmän tai vähemmän kapasiteetin palvelun Azure-portaalissa.

BizTalkin-Azuren näennäiskoneiden on infrastruktuurin nimellä--palvelun (IaaS)-arkkitehtuuri. Voit luoda näennäiskoneiden ja määrittää niitä täsmälleen kuten paikallisen ympäristön suorittaa pilveen koodin ilman muutoksia aiemmin sovelluksia on helpompaa. IaaS, jonka olet edelleen vastuussa näennäiskoneiden määrittäminen ja hallinta näennäiskoneiden (esimerkiksi asentanut ohjelmiston ja käyttöjärjestelmän korjaukset) suunnittelee suuren käytettävyyden hakeminen.

Jos kyseessä on uusi integrointi-ratkaisuja, jotka pienentäminen infrastruktuurin hallinta-työmäärään, käyttää BizTalk palveluja. Jos Haluatko siirtää nopeasti aiemmin BizTalk-ratkaisuja tai Etsitkö kehittämään ja BizTalkin sovellusten testaaminen tarvittaessa ympäristössä, käytä BizTalkin Azure Virtual Machine.

#### <a name="what-is-the-difference-between-biztalk-adapter-service-and-hybrid-connections"></a>Mitä eroa BizTalk sovittimen palvelu ja Hybrid yhteydet?
BizTalk sovittimen palvelua käytetään Azure-BizTalk palvelu. BizTalk sovittimen palvelun käyttää BizTalk sovittimen pakettiin yhteys paikalliseen-rivillä on Business ALUESOVELLUKSISTA järjestelmään. Hybrid-yhteyden avulla voit helposti ja helposti muodostaa paikallisen resurssin Azure-sovellukset, kuten Azure sovelluksen ja Azure Mobile-palveluihin, Web Apps-ominaisuus.

#### <a name="what-does-hybrid-connection-data-transfer-gb-per-unit-mean-is-this-per-minutehourdayweekmonth-what-happens-when-the-limit-is-reached"></a>Mitä "Hybrid yhteyden tiedonsiirto (gt) kohti" tarkoittaa? Tämä on minuutti/tunti/päivä/viikko tai kuukausi kohden? Mitä tapahtuu, kun on saavutettu?

Hybrid yhteyden yksikköhinta määräytyy BizTalk Services edition. Vain lisätä kustannukset riippuvat-tietojen siirtäminen. Esimerkiksi 10 Gigatavun tietojen siirtäminen päivittäin kustannuksia pienempi kuin 100 Gigatavua siirtäminen päivittäin. [Hinnat Laskimen](https://azure.microsoft.com/pricing/calculator/?scenario=full) BizTalk-palveluiden avulla voit määrittää haluamasi kustannukset. Raja-arvot ovat yleensä pakotettu päivittäin. Jos ylität rajoitus, yliannostus laskutetaan suuruuden 1 gigatavua.

#### <a name="when-i-create-an-agreement-in-biztalk-services-why-does-the-number-of-bridges-go-up-by-two-instead-of-just-one"></a>Kun sopimuksia luominen BizTalk Services, miksi siltoja määrän Siirry ylöspäin kahdella vain yhden sijaan?

Kunkin sopimuksen sisältää kaksi eri siltoja: Lähetä puoli viestintä sillan ja vastaanota side viestintä siltaa.

####  <a name="what-happens-when-i-hit-the-quota-limit-on-the-number-of-bridges-or-agreements"></a>Mitä tapahtuu, kun voin osumien kiintiö siltoja tai sopimuksia määrän?

Et voi ottaa kaikki uudet siltoja tai luoda uusia sopimuksia. Ottaa käyttöön lisää haluat skaalata BizTalk-palvelun on suurempi tai uudempi versio päivittäminen.

#### <a name="how-do-i-migrate-from-one-tier-of-biztalk-services-to-another"></a>Kuinka voin siirretään yhden tason BizTalk palvelujen toiseen?

Vapaa edition ei voi siirtää tai toisen tason ' sovitettu- ja ei voi varmuuskopioida ja palauttaa toisen tason. Jos tarvitset toisen tason, Luo uusi BizTalk palvelu käyttämällä uusi taso. Minkä tahansa palvelutiedot luotu vapaa-versioon, mukaan lukien hybrid yhteyksiä on luodaan uusi BizTalk-palvelussa. 

Jäljellä olevat versiot käyttää varmuuskopiointi ja palauttaminen siirtämistä oman palvelutiedot yksi taso. Varmuuskopioi esimerkiksi vakio taso oman palvelutiedot ja palauttaa ne sitten Premium taso. [BizTalk Services: varmuuskopioiminen ja palauttaminen](biztalk-backup-restore.md) kuvataan tuetut siirron polut ja näyttää, mitä palvelutiedot varmuuskopioidaan. Huomaa, että Hybrid yhteydet varmuuskopioidaan ei. Varmuuskopiointia ja palauttamista uusi taso, kun olet luo sitten hybrid yhteydet.  


#### <a name="is-the-biztalk-adapter-service-included-in-the-service-how-do-i-receive-the-software"></a>On BizTalk sovittimen-palvelu sisältää palvelun? Miten ohjelmistoa voi tulla?

Kyllä, BizTalk sovittimen Service Pack BizTalk-sovittimen ovat mukana Azure BizTalk Services SDK [Lataa](http://www.microsoft.com/download/details.aspx?id=39087).

## <a name="next-steps"></a>Seuraavat vaiheet

Jos haluat luoda Azure BizTalk Services Azure-portaalissa, siirry [BizTalk Services: valmistelu Azure-portaalissa](biztalk-provision-services.md). Aloita sovellusten luominen, siirry [Azure BizTalk palvelut](http://go.microsoft.com/fwlink/p/?LinkID=235197).

## <a name="additional-resources"></a>Lisäresursseja
- [BizTalk Services: Valmistelu Azure-portaalissa](biztalk-provision-services.md)<br/>
- [BizTalk Services: Tila-kaavion valmistelu](biztalk-service-state-chart.md)<br/>
- [BizTalk Services: Raporttinäkymät-ikkunan, näyttö ja skaalaus-välilehdet](biztalk-dashboard-monitor-scale-tabs.md)<br/>
- [BizTalk Services: Varmuuskopiointi ja palauttaminen](biztalk-backup-restore.md)<br/>
- [BizTalk Services: rajoittaminen](biztalk-throttling-thresholds.md)<br/>
- [BizTalk Services: Myöntäjän nimi ja myöntäjä avain](biztalk-issuer-name-issuer-key.md)<br/>
- [Miten voin aloittaa käyttämällä Azure BizTalk Services SDK-paketissa](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
