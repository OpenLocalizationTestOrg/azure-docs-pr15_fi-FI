<properties   
    pageTitle="BizTalkin Muokkaa ratkaisuja siirtämisestä BizTalk Services tekninen opas | Microsoft Azure"
    description="Muokkaa siirtäminen MAb; Microsoft Azure BizTalk palvelut"
    services="biztalk-services"
    documentationCenter="na"
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


# <a name="migrating-biztalk-server-edi-solutions-to-biztalk-services-technical-guide"></a>Siirtämisestä BizTalk Services BizTalkin Muokkaa ratkaisut: tekninen opas

Tekijä: Tim Wieman ja Nitin Mehrotra

Tarkistajat: Karthik Bharthy

Kirjoitettu: Microsoft Azure BizTalk Services – helmikuussa 2014 vapauttamista.

## <a name="introduction"></a>Johdanto

Sähköisen Data Interchange Format (Muokkaa) on yleisin keino yritysten exchange tiedot sähköisessä muodossa, myös termillä Business Business tai B2B tapahtumiksi. BizTalkin on ollut Muokkaa tuki vuosikymmenen alkuperäinen BizTalk Server-version jälkeen. BizTalk-palvelujen kanssa Microsoft säilyy Muokkaa ratkaisuja tuki Microsoft Azure-ympäristössä. Organisaation ulkopuolisten henkilöiden enimmäkseen B2B tapahtumia ja näin ollen epäyhtenäiset tiedot on helppo toteuttamisesta, jos se on otettu käyttöön cloud-ympäristössä. Microsoft Azure on BizTalk suoratoistamisen tätä ominaisuutta.

Kun asiakkaita katsella BizTalk-palvelut "greenfield"-ympäristö uusia Muokkaa ratkaisuja, monet asiakkaat on nykyisen BizTalk Server Muokkaa-ratkaisuja, jotka he haluavat siirtää Azure. Koska BizTalk Services Muokkaa arkkitehtuuri saman avaimen kohteet kuin BizTalk Server Muokkaa arkkitehtuuri (myynti kumppanit, objektit, sopimusten) perusteella on mahdollista BizTalk Server Muokkaa palvelutiedot siirtäminen BizTalk palvelut.

Tämän asiakirjan käsitellään joitakin BizTalk palveluihin liittyvistä siirretään BizTalk Server Muokkaa palvelutiedot erot. Tämän asiakirjan olettaa kokemusta BizTalk Server Muokkaa käsittelyn ja Kaupankäyntipäivä kumppanin sopimuksia. Katso lisätietoja BizTalk Server Muokkaa [Kumppanin hallinta käyttämällä BizTalk palvelin myynti](https://msdn.microsoft.com/library/bb259970.aspx).

## <a name="which-version-of-biztalk-server-edi-artifacts-can-be-migrated-to-biztalk-services"></a>BizTalk Server Muokkaa palvelutiedot version voidaan siirtää BizTalk Services?

BizTalk Server Muokkaa moduulin merkittävästi parannetun BizTalk Server 2010: n, kun se on Uudistetuista kumppanit, profiilit ja sopimuksia. BizTalk Services käyttää samaan tietomalliin kaupan kumppanien ja näiden kaupan kumppanien business osastoja järjestämiseen. Tämän vuoksi siirtänyt Muokkaa palvelutiedot BizTalk Server 2010: n ja sitä uudemmissa versioissa BizTalk palveluja, on paljon suoraan eteenpäin prosessi. Siirtää Muokkaa palvelutiedot BizTalk Server 2010: n versioissa liittyvät BizTalk Server 2010: een päivittämistä ja siirtää sitten Muokkaa palvelutiedot BizTalk palvelut.

## <a name="scenariosmessage-flow"></a>Skenaariot/viestien kulku

Kuin BizTalk Serverin kanssa Muokkaa käsittely BizTalk Services muodostanut myynti kumppanin Management (TPM)-ratkaisun ympärille. TPM-ratkaisu on avaimen seuraavat osat:

- Myynti kumppanien vastaavat organisaation B2B tapahtumaa.
- Profiilit, joka edustaa kaupan kumppanin osastoja.
- Kumppanin toimeenpano (tai sopimusten) kaksi kumppanien/profiilit business sopimus vastaavat.

Seuraava kuva esittää yhteiset ominaisuudet sekä BizTalk Server Muokkaa ratkaisu ja BizTalk Services Muokkaa ratkaisu erot:

![][EDImessageflow]

Tärkeimmät erot ja yhtäläisyydet Muokkaa ratkaisu-työnkulku-BizTalkin ja BizTalk-palvelut ovat seuraavat:

- Samalla tavalla kuin BizTalkin käytetään EDIReceive myyntijakso vastaanottaa Muokkaa viestin ja EDISend myyntijakso lähettämisestä Muokkaa, BizTalk Services käyttää Muokkaa vastaanottaa silta vastaanottamaan ja Muokkaa Lähetä silta Muokkaa viestien lähettämiseen. BizTalk Serverissä putkistot liittyvät sopimuksia käyttämällä Lähetä puhelinnumeroon ja vastaanottaa portit. BizTalk-palveluiden itse sopimukseen ilmaisee, Lähetä puhelinnumeroon ja vastaanottaa silta.
- BizTalk-palvelimella, kun EDIReceive putkijohto käsittelee Muokkaa viestin viestin tekstieditorissa SQL Server-tietokantaan. EdiSend putkijohto sitten vastataan viestin SQL Server-tietokannasta, käsittelee sen ja lähettää sen sitten kaupan kumppaniin.

    BizTalk palveluja, kun Muokkaa vastaanottaa silta käsittelee Muokkaa viestin, se reitittää viestin ulkoinen prosessi. Ulkoinen prosessi voidaan käyttää Microsoft Azure tai paikalliseen. Ulkoinen prosessi reitittää viestin Muokkaa Lähetä siltaan; Lähetä silta ei tuoda viestin salliminen. Viestin käsittelyn jälkeen Muokkaa Lähetä siltaan reitittää kaupan Kumppanin viesti.

BizTalk palvelut sisältävät helposti käytettävällä määritysten nopeasti luoda ja ottaa käyttöön B2B sopimus myynti kumppanien ilman määrittäminen minkä tahansa Microsoft Azure Laske esiintymät (verkossa tai työntekijä roolit), minkä tahansa Microsoft Azure SQL-tietokannat tai minkä tahansa Microsoft Azure-tallennustilan tilit. Monimutkaisempia skenaariot edellyttävät sitominen työnkulut tai palvelun käsittelyyn "ympärillä rajatut" Myynti kumppanin sopimuksen, eli ennen tai jälkeen myynti kumppanin sopimuksen Muokkaa silta käsittely. Yksityiskohtaiset tiedot BizTalk Services-palveluissa Muokkaa-viestin aikana ilmenee tapahtumien seuraavat sarjaa.

1. Muokkaa kun viesti on saatu kumppani, Fabrikam myynti.  Muokkaa viestien vastaanottaminen kaupan kumppanit, BizTalk Services tukee transport protokollia, kuten FTP, SFTP, AS2 ja HTTP/s-kirjainta.

2. Kaupan kumppanin sopimuksen vastaanota side käsittely purkaa Muokkaa viestin XML-muotoon.  Voit ohjata palvelun Bus päätepisteet esimerkiksi palvelun Bus välitys päätepisteen, palvelun Bus aiheen, palvelun Bus jonossa tai BizTalk Services sillan kokoamaton tavara Muokkaa viestin (XML-muodossa).

3. Kokoamaton tavara XML-viestien sai sitten päätepisteestä mukautetun käsittelyä varten.  Nämä päätepisteet voinut käsitellä paikallisen-osa-tai Microsoft Azure Laske esiintymä edelleen prosessin viestin Windows työnkulun (Windowsin Palomuuri) tai Windows Communication Foundation (WCF)-palvelussa, kuten.

4. "Lähetä Include käsittely" kaupan kumppanin sopimuksen sitten kokoaa XML-viestin Muokkaa muotoon ja lähettää sen kaupan kumppani, Contoso.  Muokkaa viestien lähettämiseen kaupan kumppaneille, BizTalk Services tukee saman protokollia kuin käytetyt Muokkaa viestien vastaanottamiseen.

Tämä asiakirja on käsitteellisiä ohjeita joitakin eri BizTalk Server Muokkaa palvelutiedot siirtämisestä edelleen BizTalk palvelut.

## <a name="sendreceive-ports-to-trading-partners"></a>Lähetä tai vastaanota portit kohteeksi kumppaneille

BizTalk Serverissä määrityksen tulla sijainteihin ja vastaanota portit, Muokkaa ja XML-viestien vastaanottamiseen kaupan yhteistyökumppanien ja Määritä lähetys portit Muokkaa ja XML-viestien lähettäminen kaupan kumppani. Voit sitten vaadi kaupan kumppanin sopimuksen, käyttämällä BizTalk hallintakonsoli seuraavat portit. BizTalk palveluja, jossa saapuvista viesteistä myynti kumppanien sijainnit ja missä lähettämäsi viestit kohteeksi kumppanien on määritetty osana kaupan kumppanin sopimuksen itse (Transport asetukset osana) BizTalk palvelut-portaalissa.  Niin todella et "Lähetä portit" ja "tulla sijainteihin", joka kertoo, BizTalk Services-palveluissa. Lisätietoja on artikkelissa [Luominen sopimuksia](https://msdn.microsoft.com/library/windowsazure/hh689908.aspx).

## <a name="pipelines-bridges"></a>Putkistot (siltoja)

BizTalk Server Muokkaa putkistot ovat viestin käsittely-kohteita, voit myös lisätä mukautetun logiikan käsittelyä ominaisuuksia vaaditulla sovellus. BizTalk-palveluiden vastaavat tapaan Muokkaa silta. Kuitenkin BizTalk palveluja, nyt Muokkaa siltoja "suljetaan".  Et voi lisätä omia mukautettuja toimintoja Muokkaa silta. Mukautettu käsittely on tehtävä ulkopuolella Muokkaa sillalta sovelluksen ennen tai sen jälkeen, kun viesti saapuu myynti kumppanin sopimuksen osana määritetty siltaan. EAI siltoja on suoritettava mukautettujen käsittelyn asetus. Mukautettu käsittely halutessasi voit ennen tai jälkeen viesti käsitellään Muokkaa silta EAI siltoja. Katso lisätietoja, [miten voit sisällyttää mukautetut koodi siltoja](https://msdn.microsoft.com/library/azure/dn232389.aspx).

Voit lisätä Julkaise ja tilaa vuo mukautettua koodia ja/tai palvelun Bus messaging olevien ja aiheet, ennen kuin kaupan kumppanin sopimuksen vastaanottaa viestiä tai sopimuksen käsittelee viestin ja reitittää sen palvelun Bus päätepisteen avulla.

Tämän ohjeaiheen viestin työnkulku kuvion kohdassa **Skenaariot/viestien kulku** .

## <a name="agreements"></a>Toimeenpano

Jos tunnet BizTalk Server 2010: n Trading kumppanin sopimusten käytetään Muokkaa käsittelyä varten, BizTalk palveluiden myynti kumppanin toimeenpano näyttää varmasti tutulta. Useimmat sopimuksen asetuksista ovat samat ja käyttää samaa termejä. Joissakin tapauksissa sopimus-asetukset on helpompaa verrattuna BizTalkin samat asetukset. Microsoft Azure BizTalk Services tukee X12 EDIFACT ja AS2 transport.

Microsoft Azure BizTalk Services sisältää myös **TPM tietojen** siirtotyökalua kaupan kumppanit ja sopimuksia BizTalk Server myynti Partner moduulista BizTalk palvelut-portaaliin. TPM tietojen siirtäminen-työkalu on saatavana osana Tools-paketti, jonka voi ladata [MAb SDK-paketissa](http://go.microsoft.com/fwlink/p/?LinkId=235057). Paketti sisältää myös Lueminut-tiedosto, joka sisältää ohjeita siitä, miten voit käyttää työkalu ja vianmäärityksen perustiedot-työkalun.

## <a name="schemas"></a>Rakenteet

BizTalk Services sisältää Muokkaa rakenteita, joita voidaan käyttää BizTalk Services-palveluiden ratkaisujen.  Lisäksi BizTalk Server Muokkaa malleja voi myös BizTalk-palvelujen kanssa koska Muokkaa rakenteen pääkansion-solmu on sama BizTalkin sekä BizTalk palveluiden välillä. Näin ollen osaat suoraan BizTalk Server Muokkaa rakenteita ja käyttää niitä Muokkaa-ratkaisuja, jotka voit kehittää BizTalk Services-palvelujen avulla. Voit ladata rakenteet myös [MAb SDK](http://go.microsoft.com/fwlink/p/?LinkId=235057).

## <a name="maps-transforms"></a>Kartat (muunnoksia)

Kartat BizTalkin kutsutaan muunnokset BizTalk Services-palveluissa. Luotujen karttoja BizTalkin BizTalk palveluihin voi olla yksi monimutkaisia tehtäviä tavoitteet (sen mukaan, kartan monimutkaisuus). Yhdistäminen-työkalu, jolla BizTalk Services eroaa BizTalk mapper. Vaikka mapper näyttää lähinnä saman, pohjana kartta-muoto ei ole. Functoids (eli **Kartan toimintojen** BizTalk Services) käyttäjien käytettävissä ovat eri paikan päällä.  Et voi käyttää BizTalk kartan suoraan BizTalk palvelut. Myös kaikki käytettävissä olevat BizTalkin functoids ovat käytettävissä kartan toimintoina BizTalk Services-palveluissa.

### <a name="new-transform-operations"></a>Uusi muunnos-toiminnot

Kun muunnos kartan toimintojen käytettävissä luettelo saattaa näyttää aivan eroaa BizTalkin mapper, BizTalk Services muunnokset on uusilla tavoilla, joiden avulla voit suorittaa tehtäviä. Esimerkiksi BizTalk Services muunnokset on **Luettelon toiminnot** käytettävissä. Ei ole käytettävissä BizTalk mapper.  **Luettelon toimintojen** avulla voit luoda ja toimivat "Luettelon", jossa luettelo on joukko kohteita (tunnetaan myös nimellä "rivien") ja kunkin kohteen voi olla useita jäseniä (tunnetaan myös nimellä "sarakkeet").  Voit lajitella luettelon, valitse kohteet, jotka perustuvat ehto, jne.

Toinen esimerkki BizTalk Services muunnokset uudet toiminnot ovat **Silmukan toimintoja**.  On vaikeaa sisäkkäisiä silmukoita luominen BizTalkin mapper.  Näin ollen silmukan kartta-toimintojen lisätään BizTalk-palvelut-muunnoksia.

Toinen esimerkki vielä on **Jos-niin-muuten** lausekkeen kartta-toimintoa.  Jos-niin-muuten toiminnon tekeminen on mahdollista BizTalk mapper, mutta se edellyttää useita functoids näennäisesti yksinkertaisen tehtävän suorittamiseen.

### <a name="migrating-biztalk-server-maps"></a>Luotujen BizTalkin määritykset

Microsoft Azure BizTalk Services sisältää työkalun siirtämään BizTalkin yhdistää BizTalk Services muunnoksia. **BTMMigrationTool** on käytettävissä **Työkalut** -ohjelmistopaketin mukana [BizTalk Services SDK Lataa](http://go.microsoft.com/fwlink/p/?LinkId=235057)osana. Saat lisätietoja työkalu [muuntaa BizTalk-palvelut-Transform BizTalk kartta](https://msdn.microsoft.com/library/windowsazure/hh949812.aspx).

Voit myös tarkastella Sandro Pereira BizTalk MVP otoksen siitä, miten voit [siirtää BizTalkin kartat BizTalk Services muunnosten](http://social.technet.microsoft.com/wiki/contents/articles/23220.migrating-biztalk-server-maps-to-windows-azure-biztalk-services-wabs-maps.aspx).

## <a name="orchestrations"></a>Orchestrations

Jos haluat siirtää BizTalkin tiedonsiirron käsittelyn Microsoft Azure orchestrations on kirjoitettava, koska Microsoft Azure ei tue käynnissä BizTalkin orchestrations.  Voi kirjoittaa tiedonsiirron toimintoja Windows työnkulun Foundation 4.0 (WF4)-palvelussa.  Tämä on oltava valmiina suorittamaan ei tällä hetkellä ei siirtymistä BizTalkin orchestrations WF4. Seuraavien resurssien Windows työnkulun:

- [*Miten voit integroida WCF työnkulkupalvelu, jolla palvelun Bus ja aiheet*](https://msdn.microsoft.com/library/azure/hh709041.aspx) Paolo Salvatori mukaan. 

- [ *Rakennuksen-sovellukset Windows Workflow Foundation ja Azure* istunnon](http://go.microsoft.com/fwlink/p/?LinkId=237314) muodosta 2011 neuvottelusta.

- MSDN-sivuston [*Windows työnkulun Foundation Developer Centerissä*](http://go.microsoft.com/fwlink/p/?LinkId=237315) .

- MSDN-sivuston [*Windows työnkulun Foundation 4 (WF4) ohjeissa*](https://msdn.microsoft.com/library/dd489441.aspx) .

## <a name="other-considerations"></a>Muita huomioon otettavia seikkoja

Seuraavassa on muutamia seikat, jotka sinun on tehtävä BizTalk palveluita käyttäessään.

### <a name="fallback-agreements"></a>Varakyselyjen sopimusten

BizTalk Server Muokkaa käsittely on "Varmistusta sopimusten" käsite.  BizTalk Services onko **ei** ole varmistusta sopimus-käsite tähän mennessä.  Näkyviin BizTalk dokumentaatio [Rooli, sopimusten Muokkaa käsittelyn](http://go.microsoft.com/fwlink/p/?LinkId=237317) ja [määrittämisestä Yleinen tai varmistusta sopimuksen ominaisuudet](https://msdn.microsoft.com/library/bb245981.aspx) tiedot-varmistusta toimeenpano käyttämisestä BizTalkin.

### <a name="routing-to-multiple-destinations"></a>Useita kohteita reitittämisestä

BizTalk Services siltoja nykyisestä tilasta ei tue useita kohteita käyttämällä julkaisemisen reititys viestien-tilaa malli. Sen sijaan voi reitittää sanomia BizTalk Services siltauksesta palvelun Bus aiheeseen, jonka voit sitten on useita tilauksia on useampi kuin yksi päätepiste sanoman.

## <a name="conclusion"></a>Tekemistä

Microsoft Azure BizTalk Services on päivittänyt säännöllisesti välitavoitteiksi, jotta voit lisätä useita ominaisuuksia ja toimintoja. Kunkin päivityksessä Odotamme tukevat toimintoja helpottamiseksi lopusta loppuun-ratkaisujen BizTalk Services ja Azure muita tekniikoita luomiseen.

## <a name="see-also"></a>Katso myös

[Enterprise-sovellusten Azure kehittäminen](https://msdn.microsoft.com/library/azure/hh674490.aspx)

[EDImessageflow]: ./media/biztalk-migrating-to-edi-guide/IC719455.png
