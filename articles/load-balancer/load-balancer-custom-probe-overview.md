<properties
  pageTitle="Lataa tasaustoiminto mukautetun keräysputkien seurantaan ja valvontaan kunnon tilan | Microsoft Azure"
  description="Opettele käyttämään seurannassa esiintymät takana kuormituksen mukautetun keräysputkien Azure kuormituksen varten"
  services="load-balancer"
  documentationCenter="na"
  authors="sdwheeler"
  manager="carmonm"
  editor=""
  tags="azure-resource-manager"
/>
<tags
  ms.service="load-balancer"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="na"
  ms.workload="infrastructure-services"
  ms.date="10/24/2016"
  ms.author="sewhee" />

# <a name="understand-load-balancer-probes"></a>Kuormituksen tasauspalvelun keräysputkien ymmärtäminen

Azure kuormituksen on ominaisuuksien tarkkailla server esiintymät keräysputkien avulla. Kun näytteenottimen lakkaa vastaamasta, kuormituksen lopettaa uudet yhteydet lähettäminen perusasemassa esiintymä. Muodostetut yhteydet näin ei käy ja uudet yhteydet lähetetään kunnossa esiintymät.

Cloud palvelun roolit (työntekijän roolit ja web roolit) Käytä Vieras-agentti näytteenottimen seurantaa varten. TCP- tai HTTP mukautetun keräysputkien on oltava määritettynä, kun käytät näennäiskoneiden kuormituksen takana.

## <a name="understand-probe-count-and-timeout"></a>Tietoja näytteenottimen Laske- ja aikakatkaisu

Näytteenottimen toiminta määräytyy:

- Onnistuneiden keräysputkien, joka mahdollistaa erillisen lukee määrä kuin ylös.
- Epäonnistuneiden keräysputkien, joka aiheuttaa erillisen lukee määrä kuin alaspäin.

SuccessFailCount aikakatkaisu ja korkojakso arvo määrittävät, onko esiintymää on vahvistettu on tai ei ole käynnissä. Azure-portaalissa aikakatkaisun on määritetty kahdesti taajuus arvo.

Kaikki kuormituksen esiintymät, päätepisteen (eli kuormituksen joukon) näytteenottimen-määritys on oltava sama. Tämä tarkoittaa eri näytteenottimen määrityksen jokaisen roolin esiintymän tai virtuaalikoneen ei saa olla sama isännöityä palvelua tietyn päätepisteen yhdistelmä. Esimerkiksi jokaiselle esiintymälle on oltava samat paikalliset portit ja aikakatkaisu.

>[AZURE.IMPORTANT] Kuormituksen näytteenottimen käyttää IP-osoite 168.63.129.16. Tämä julkiseen IP-osoite helpottaa sisäinen ympäristö resurssit tuo your-omistaja-IP-Azure Virtual verkon skenaarion tietoliikenne. Virtuaalinen julkiseen IP-osoite 168.63.129.16 käytetään kaikilla alueilla ja ei muutu. Suosittelemme, että annat IP-osoitteen paikallinen palomuuri käytännöt. Se ei tule pitää tietoturvariskin koska vain sisäinen Azure ympäristö voi tietolähteen osoitteesta osoite. Jos et tee näin, ole skenaarioita, kuten samaa IP-osoitealueita 168.63.129.16 ja ottaa kaksoiskappale IP-osoitteiden määrittäminen odottamatonta toimintaa.

## <a name="learn-about-the-types-of-probes"></a>Lisätietoja keräysputkien tyypit

### <a name="guest-agent-probe"></a>Vieras agentti näytteenottimen

Tämä sondi on käytettävissä Azure pilvipalveluihin vain. Kuormituksen, käytä Vieras-agentti sisällä virtuaalikoneen, ja seuraa ja vastaa HTTP 200 OK-vastauksen vain, kun esiintymä on valmis-tilassa (eli kuin toisen valtion esimerkiksi varattu kierrättäminen tai pysäyttää).

Lisätietoja on artikkelissa [palvelun määritystiedosto (csdef) määrittäminen kunto keräysputkien](https://msdn.microsoft.com/library/azure/ee758710.aspx) tai [luomisesta Internetiin yhteydessä oleva-kuormituksen cloud Services](load-balancer-get-started-internet-classic-cloud.md#check-load-balancer-health-status-for-cloud-services).

### <a name="what-makes-a-guest-agent-probe-mark-an-instance-as-unhealthy"></a>Minkälainen Merkitse erillisen merkitään virheelliseksi Vieras agent-näytteenottimen?

Jos Vieras-agentti ei vastaa HTTP 200 OK, kuormituksen merkitsee esiintymän kuin vastaamisen ja lopettaa liikenne lähettää kyseisen esiintymän. Kuormituksen säilyy ping-esiintymä. Jos Vieras-agentti vastaa HTTP-200, kuormituksen lähettää liikenne kyseisen esiintymän uudelleen.

Kun web-rooli, sivuston koodin käytetään yleensä w3wp.exe, jossa on ei Azure valvoo kangasta tai Vieras agentti. Tämä tarkoittaa, että Vieras-agentti ei raportoitava w3wp.exe (esimerkiksi HTTP 500 vastaukset) virheiden ja kuormituksen eivät tule kyseisen esiintymän ulos kierto.

### <a name="http-custom-probe"></a>HTTP mukautetun näytteenottimen

Mukautetun HTTP kuormituksen näytteenottimen ohittaa oletusarvon Vieras agent-näytteenottimen, mikä tarkoittaa, että voit luoda oman mukautetun logiikan terveydentilasta roolin esiintymän. Kuormituksen probes päätepiste 15 sekunnin välein oletusarvoisesti. Esiintymän pidetään olevan kuormituksen tasauspalvelun kiertoa, jos se vastaa HTTP-200 ajassa (31 sekuntia oletusarvoisesti).

Tämä voi olla hyödyllinen, jos haluat ottaa oman logiikan poistaa esiintymät kuormituksen tasauspalvelun kierto. Esimerkiksi päätät voi poistaa esiintymä, jos se on suurempi kuin 90 prosenttia suorittimen ja palauttaa-200 tila. Jos sinulla on web-roolit, jotka käyttävät w3wp.exe, tämä tarkoittaa myös saat automaattinen seuranta sivuston, koska sivuston koodin virheiden palauttaa kuormituksen tasauspalvelun näytteenottimen-200 tila.

>[AZURE.NOTE] HTTP mukautetun keräysputken tukee suhteellisten polkujen ja HTTP-protokolla. HTTPS ei tueta.

### <a name="what-makes-an-http-custom-probe-mark-an-instance-as-unhealthy"></a>Mikä HTTP mukautetun näytteenottimen Merkitse erillisen merkitään virheelliseksi tekee?

- HTTP-sovellus palauttaa HTTP vastaus-koodin kuin 200 (esimerkiksi 403, 404 tai 500). Tämä on positiivinen acknowledgment, sovelluksen esiintymää on otettava poissa saman tien.
- HTTP-palvelin ei vastaa lainkaan aikakatkaisuajan jälkeen. Sen mukaan, aikakatkaisuarvo, joka on määritetty, voi tällä, useita näytteenottimen pyytää ilman vastausta ennen näytteenottimen saa merkitty ei ole käynnissä olevat go (eli ennen SuccessFailCount näytteenottimet lähetetään).
- Palvelimen sulkee yhteyden TCP-Palauta kautta.

### <a name="tcp-custom-probe"></a>TCP mukautetun näytteenottimen

TCP keräysputkien yhteyden muodostamiseen suorittamalla kolme tapaa kättelyn määritetyn porttiin.

### <a name="what-makes-a-tcp-custom-probe-mark-an-instance-as-unhealthy"></a>Minkälainen TCP mukautetun näytteenottimen, merkitse erillisen merkitään virheelliseksi?

- TCP-palvelin ei vastaa lainkaan aikakatkaisuajan jälkeen. Kun näytteenottimen on merkitty ei ole käynnissä määräytyy epäonnistui tutkimuspyyntöjä, joka on määritetty Siirry ennen merkintä keräysputken kuin suoriteta vastaamattomat määrä.
- Näytteenottimen vastaanottaa TCP, Palauta roolin esiintymästä.

Lisätietoja HTTP-kunto näytteenottimen tai TCP-näytteenottimen määrittämisestä on artikkelissa [luomisesta Internetiin yhteydessä oleva kuormituksen resurssien hallinta PowerShellin avulla](load-balancer-get-started-internet-arm-ps.md#create-lb-rules-nat-rules-a-probe-and-a-load-balancer).

## <a name="add-healthy-instances-back-into-load-balancer-rotation"></a>Yhteen kunnossa takaisin kuormituksen tasauspalvelun kierto

TCP ja HTTP keräysputkien pidetään kunnossa ja Merkitse siis kelvollinen, kun rooli-esiintymä:

- Kuormituksen saa positiivinen näytteenottimen, ensimmäistä kertaa AM käynnistyy.
- Luvun SuccessFailCount (edempänä) määrittää onnistuneen keräysputkien, joita tarvitaan Merkitse kuin kunnossa rooli-esiintymän arvo. Jos roolin esiintymän poistettiin, onnistuu, peräkkäiset keräysputkien määrä on yhtä suuri kuin tai ylittää SuccessFailCount Merkitse kuin suorittaminen rooli-esiintymä-arvoa.

>[AZURE.NOTE] Jos vaihteleva luku roolin esiintymän kunto-kuormituksen odottaa enää ennen osoittamassa roolin esiintymän takaisin kunnossa tila. Tehdä tämän käytännön käyttäjän ja infrastruktuurin suojaaminen kautta.

## <a name="use-log-analytics-for-load-balancer"></a>Käytä log analytics kuormituksen

[Log analysointitietoja kuormituksen](load-balancer-monitor-log.md) avulla voit tarkistaa näytteenottimen terveydentilasta ja näytteenottimen määrä. Kirjaaminen voidaan Power BI-tai Azure toiminnallisia havainnollistamisen antamaan tilastotietoja kuormituksen kunnon tilaa.
