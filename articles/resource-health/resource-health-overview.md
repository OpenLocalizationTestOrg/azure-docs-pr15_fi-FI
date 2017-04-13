<properties
   pageTitle="Azure kunto Resurssikatsaus | Microsoft Azure"
   description="Yleistä Azure resurssin kunto"
   services="Resource health"
   documentationCenter="dev-center-name"
   authors="BernardoAMunoz"
   manager=""
   editor=""/>

<tags
   ms.service="resource-health"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="Supportability"
   ms.date="06/01/2016"
   ms.author="BernardoAMunoz"/>

# <a name="azure-resource-health-overview"></a>Azure kunto Resurssikatsaus

Azure resurssin kunto on palvelu, joka paljastaa yksittäisten Azure resurssien kunto ja suoritettavia ohjeita ongelmien vianmääritys. Cloud-ympäristössä, jossa ei voi käyttää suoraan palvelimia tai infrastruktuurin osia tavoitteen määrittäminen resurssi on asiakkaiden käyttävät vianmäärityksen, vähentää erityisesti tarkistamalla, jos ongelman ylimmällä säädetään sovelluksen sisällä tai tapahtuman sisällä Azure ympäristö johtuu aikaa aikaa.

## <a name="what-is-considered-a-resource-and-how-does-resource-health-decides-if-the-resource-is-healthy-or-not"></a>Mitä pidetään resurssin ja mistä resurssin kunto päättää, onko resurssi kunnossa? 
Resurssi on Resurssityyppi, myöntämä palvelun, kuten käyttäjien luomissa esiintymä: virtual machine, verkkosovellukseen tai SQL-tietokantaan. 

Resurssin kunto on riippuvainen signaalit lähettämän resurssi ja/tai palvelun määrittääksesi, onko resurssi kunnossa. On tärkeää huomata, että tällä hetkellä resurssin kunto vain tilit tiettyä resurssia kunto kirjoittamalla ja ei myöskään muita elementtejä, jotka voivat kohtaan yleinen kunto. Esimerkiksi raportoitaessa virtual machine, vain Laske infrastruktuurin osa tilan pidetään eli verkon ongelmat ei näytetä resurssin kunto-ellei ole ilmoitettu service-käyttökatkosta, se voidaan esiin kautta ilmoituspalkissa yläreunaan sivu. Lisätietoja palvelun käyttökatkosta tarjotaan tämän artikkelin. 

## <a name="how-is-resource-health-different-from-service-health-dashboard"></a>Miten eroaa resurssin kunto palvelun kunnon koontinäyttö?

Resurssin kunto toimittamien tietojen on eritellympiä kuin mikä tarjoaa palvelun kunnon koontinäyttö. Kun SHD kommunikoi tapahtumista, jotka vaikuttavat palvelu alueen käytettävyyttä, resurssin kunto paljastaa tietyn resurssin tiedot, kuten se näyttää tapahtumista, jotka vaikuttavat virtual machine, verkkosovellukseen tai SQL-tietokannan käytettävyyttä. Esimerkiksi jos solmu odottamatta käynnistyy, asiakkaat, joiden näennäiskoneiden on käytössä solmun saa oikeuden hankkia syy, miksi niiden AM ei ole käytettävissä ajan kuluessa.   

## <a name="how-to-access-resource-health"></a>Resurssin kunto käyttäminen
Resurssin kunto kautta palveluja on 2 tapoja käyttää resurssin kunto.

### <a name="azure-portal"></a>Azure Portal
Azure-portaalissa resurssin kunto-sivu sisältää yksityiskohtaisia tietoja resurssin kuntotietojen sekä suositellut toiminnot, jotka vaihtelevat sen mukaan, resurssin nykyinen kunto. Tämä sivu on saadaan paras käyttötuntuma kyselyt resurssin kunto, kun se Versioning portaalin sisällä muita resursseja. Ennen kuin mainittu joukko toimintoja suositeltu resurssin kunto-sivu vaihtelee nykyinen kunto:

* Kunnossa resurssit: koska ei ole ongelma, joka voi olla vaikutusta resurssin kunto havaitsi toiminnot kohdistettu tukea vianmääritystä varten. Esimerkiksi on suora pääsy vianmääritys-sivu, jossa on ohjeita siitä, miten voit korjata yleisimmät ongelmia asiakkaat-kuvaketta.
* Perusasemassa resurssi: Azure aiheuttamia ongelmia, sivu näkyy toiminnot Microsoft kestää (tai on tehnyt) palauttaa resurssi. Käyttäjän aiheuttamia ongelmia omaan toiminnot-sivu avautuu toiminnot asiakasluettelosta voi kestää niin osoite ongelman ja palauttaa resurssi.  

Kun olet kirjautunut Azure-portaaliin, on kaksi tapaa käyttää resurssin kunto-sivu: 

###<a name="open-the-resource-blade"></a>Avaa resurssi-sivu
Avaa määritetyn resurssin resurssi-sivu. Valitse asetukset-sivu, joka avaa vieressä resurssi-sivu, resurssi terveyteen Avaa resurssin kunto-sivu. 

![Resurssin kunto-sivu](./media/resource-health-overview/resourceBladeAndResourceHealth.png)

### <a name="help-and-support-blade"></a>Ohje ja tuki-sivu
Avaa Ohje ja tuki-sivu valitsemalla ohjeen + tuki sitten oikean yläkulman kysymysmerkki valitsemalla. 

**Valitse yläreunan siirtymispalkista**

![Ohjeen + tuki](./media/resource-health-overview/HelpAndSupport.png)

Ruudun napsauttaminen avaa resurssin kunto tilauksen sivu jossa luetellaan kaikki resurssit-tilaukseesi. Kullekin resurssille vieressä on kuvake ilmaisee, että sen kunto. Kullekin resurssille napsauttaminen avaa resurssin kunto-sivu.

**Resurssin kunto-ruutu**

![Resurssin kunto-ruutu](./media/resource-health-overview/resourceHealthTile.png)

## <a name="what-does-my-resource-health-status-mean"></a>Mitä tilani kunto resurssin tarkoittaa?
On 4, näyttöön saattaa tulla resurssin eri kunto-tila.

### <a name="available"></a>Käytettävissä
Palvelu ei ole havaitsi ongelmia alustaksi, joka voi olla vaikuttavat resurssin käytettävyyttä. Tämä ilmaistaan vihreä valintamerkki. 

![Resurssi on käytettävissä](./media/resource-health-overview/Available.png)

### <a name="unavailable"></a>Ei ole käytettävissä

Tässä tapauksessa palvelu on havainnut jatkuvaa ongelma alustaksi, joka on vaikuttavat esimerkiksi solmu jossa on käynnissä AM odottamatta käynnistettävä tämän resurssin käytettävyyttä. Tämä on merkitty punaisella varoitus-kuvake. Lisätietoja ongelman on annettu keskikohtaa sivu, mukaan lukien: 

1.  Mitä Microsoft kestää palauttamaan resurssi 
2.  Yksityiskohtainen aikajana ongelmasta, mukaan lukien odotettu tarkkuus-aika
3.  Luettelon suositeltuja toimia käyttäjille 

![Resurssi on ei käytettävissä](./media/resource-health-overview/Unavailable.png)

### <a name="unavailable--customer-initiated"></a>Ei käytettävissä – asiakkaan aloitettu
Resurssi on käytettävissä vuoksi asiakkaan pyynnön, kuten resurssin pysäyttäminen tai pyytää uudelleen. Tämä ilmaistaan sinistä tiedoksi kuvaketta. 

![Resurssi ei ole käytettävissä käyttäjän omaan toiminnon vuoksi](./media/resource-health-overview/userInitiated.png)

### <a name="unknown"></a>Tuntematon
Palvelu ei ole vastaanottanut yli 5 minuuttia tämän resurssin tietoja. Tämä ilmaistaan harmaa kysymysmerkkikuvake. 

On tärkeää muistaa, että tämä ei ole lopullinen tieto siitä, että on ongelmia liittyvien, jotta asiakkaat noudattamalla näitä suosituksia:

* Jos resurssi toimii odotetulla tavalla, mutta sen kunto on määritetty Tuntematon resurssin kunto, ei ole ongelmia ja voit odottaa tila resurssin päivittämään kunnossa muutaman minuutin kuluttua.
* Jos määritettynä on ongelmia resurssi ja sen kuntoa on määritetty Tuntematon resurssin kunto, tämä saattaa aikainen maininta voi olla ongelma ja muita tutkimuksia on tehtävä ennen kuin kuntotietojen päivitetään kunnossa tai perusasemassa

![Resurssin kunto on tuntematon](./media/resource-health-overview/unknown.png)

## <a name="service-impacting-events"></a>Palvelun vaikuttavat tapahtumat
Jos resurssin saattaa heikentyä jatkuvaa palvelun vaikuttavat tapahtuman mukaan, nauhan näkyvät yläreunaan resurssin kunto-sivu. Nauhan napsauttaminen avaa valvontalokin tapahtumien-sivu, jossa näkyy lisätietoja käyttökatkosta.

![Resurssin kunto saattaa heikentyä SIE](./media/resource-health-overview/serviceImpactingEvent.png)

## <a name="what-else-do-i-need-to-know-about-resource-health"></a>Mikä muu on lisätietoja resurssin kunto?

### <a name="signal-latency"></a>Signaalin viive
Signaalit syötteen resurssin kunto, voi olla enintään 15 min lykätä, jotka voivat aiheuttaa resurssin nykyinen kunto-tilaa sekä sen todellinen välillä. On tärkeää kannattaa ottaa tämä mielessä, kun se auttaa poistamaan tarpeettomia aika käsittelevälle mahdolliset ongelmat. 

### <a name="special-case-for-sql"></a>SQL erityinen tilanne 
Resurssin kuntoraportit SQL-tietokanta ei ole SQL Serverin tila. Siirtyminen tätä vaihtoehtoa, toimi tarjoaa realistisesta kunto kuvan, se vaatii, että useita osat ja -palvelut otetaan huomioon tietokannan terveydentilasta. Nykyinen signaali on riippuvainen Kirjautumiset tietokantaan, mikä tarkoittaa, että tietokannoille, jotka saavat säännöllisesti kirjautumiset (joka sisältää muun muassa: vastaanottaminen kyselyn suorituksen pyynnöt) kuntotietojen tila säännöllisesti näytetään. Jos tietokanta ei ole käytetty vähintään 10 minuutin ajan, se siirretään Tuntematon tila. Tämä ei tarkoita, että tietokanta ei ole käytettävissä, vain ei on on lähetetty, koska ei ole kirjautumiset on suoritettu. Tietokantayhteyden muodostamisessa ja kyselyn suorituksen aikana välittää signaalit tarvitsee selvittää ja Päivitä tietokannan kunto-tila.

## <a name="feedback"></a>Palaute
Emme aina Avaa palaute ja ehdotukset! Lähetä meille [ehdotuksia](https://feedback.azure.com/forums/266794-support-feedback). Lisäksi voit valtuutettua kanssamme [Twitter-](https://twitter.com/azuresupport) tai [MSDN-keskustelupalstoilla](https://social.msdn.microsoft.com/Forums/azure).
