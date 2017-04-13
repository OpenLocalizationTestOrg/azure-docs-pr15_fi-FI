<properties
    pageTitle="Suuren käytettävyyden rajat maantieteelliset AD FS käyttöönoton Azure-tietokannassa Azure liikenteen hallinta | Microsoft Azure"
    description="Tässä asiakirjassa kerrotaan AD FS Azure-tietokannassa ottamisesta hyvin laiduntamismahdollisuuksien."
    keywords="AD fs Azure liikenteen hallinta, adfs Azure liikenteen hallinta, maantieteelliset, usean palvelinkeskukseen, maantieteelliset palvelinkeskusten, usean maantieteelliset palvelinkeskusten käyttöönotto-azure AD FS, käyttöönotto azure adfs, azure adfs, azure ad fs, ADFS: n käyttöön, käyttöön ad fs adfs Azure-tietokannassa, käyttöönotto adfs Azure-tietokannassa, käyttöönotto AD FS azure, adfs azure AD FS, Azure AD FS Azure-tietokannassa, iaas esittely , ADFS, siirry azure adfs"
    services="active-directory"
    documentationCenter=""
    authors="anandyadavmsft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/01/2016"
    ms.author="anandy;billmath"/>
    
#<a name="high-availability-cross-geographic-ad-fs-deployment-in-azure-with-azure-traffic-manager"></a>Suuren käytettävyyden rajat maantieteelliset AD FS käyttöönoton Azure-tietokannassa Azure liikenteen hallinta

[Azure AD FS käyttöönottoa](active-directory-aadconnect-azure-adfs.md) on vaiheittainen siitä, miten voit ottaa yksinkertaisen AD FS-infrastruktuurin Azure organisaatiollesi. Tässä artikkelissa on seuraavat vaiheet luominen Azure AD FS rajat maantieteelliset käyttöönoton [Azure liikenteen hallinnan](../traffic-manager/traffic-manager-overview.md)avulla. Azure liikenteen hallinta auttaa luomaan maantieteellisesti levitä suuren käytettävyyden ja tehokas AD FS infrastruktuurin organisaation tekemällä reititys menetelmiä eri tarpeita vastaavaksi infrastruktuurin-alueen käyttäminen.

Ottaa käyttöön laajasti käytettävissä rajat maantieteelliset AD FS-infrastruktuuria:

* **Korjauksen yksittäisen kohdan virhe:** Automaattisesti ominaisuuksia Azure liikenteen hallinta voit toteuttaa käytettävyyden AD FS-infrastruktuurin myös silloin, kun jokin tietojen keskikohdan mukaan-osassa maapallon siirtyy
* **Entistä parempi suorituskyky:** Voit tämän artikkelin ehdotettu käyttöönoton antamaan tehokas AD FS-infrastruktuuria, jotka auttavat käyttäjiä todennetaan nopeammin. 

##<a name="design-principles"></a>Rakenne-periaatteet

![Yleiskuvaus](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/blockdiagram.png)

Raporttien luomisen periaatteet on sama rakenne periaatteet on artikkelissa Azure AD FS käyttöönottoa luetellut. Yllä olevassa kaaviossa näkyy tunniste on yksinkertainen basic käyttöönoton toiseen maantieteellisen alueen. Seuraavassa on joitakin laajentaminen uusi maantieteellisen alueen käyttöönottoa huomioon otettavia seikkoja

* **Virtual verkon:** Luo uuden virtual verkon haluat ottaa käyttöön AD FS lisäinfrastruktuuri maantieteellisen alueen. Yllä olevassa kaaviossa näet Geo1 VNET ja Geo2 VNET kaksi virtual verkkojen kunkin maantieteellisen alueen.

* **Toimialueohjaimia ja uusi maantieteellinen VNET AD FS-palvelimet:** On suositeltavaa ottaa käyttöön toimialueen ohjaimet uusi maantieteellisen alueen niin, että uuden alueen AD FS-palvelimia ei ole yhteyttä toimialueen ohjauskoneen toisessa kaukana verkko todennusta ja siten parantaa suorituskykyä.

* **Tallennustilan tilejä:** Tallennustilan tilit liittyy alueen. Koska sisällytät uusi maantieteellisen alueen koneet, sinun on luoda uusi tallennustilan tili, jota käytetään alueen.  

* **Verkon käyttöoikeusryhmät:** Kun jonkin muun maantieteellisen alueen ei voi käyttää tallennustilan tilit-alueella luotu verkon käyttöoikeusryhmät. Sen vuoksi sinun on luoda uutta verkostoa käyttöoikeusryhmät kaltaisia INT- ja DMZ aliverkon ensimmäisen maantieteellisen uusi maantieteellinen alue.

* **Julkiseen IP-osoitteiden tarrojen DNS:** Azure liikenteen hallinta voidaan viitata päätepisteet vain DNS otsikot kautta. Tämän vuoksi tarvitaan Luo DNS ulkoisen kuormituksen tasoitusmääritykset julkiseen IP-osoitteet.

* **Azure liikenteen hallinta:** Voit määrittää, että eri puolilla maailmaa palvelinkeskusten käynnissä päätepisteiden käyttäjien tietoliikenne jakautumisen Microsoft Azure liikenteen hallinta. Azure liikenteen hallinta toimii DNS-tasolla. Se käyttää DNS-vastauksia yleisesti distributed päätepisteiden loppukäyttäjän liikenne voidaan ohjata. Asiakkaiden liitä ne päätepisteet suoraan. Eri reititys vaihtoehtoja suorituskyvyn, painotettu ja prioriteetin voit helposti parhaiten soveltuu organisaation tarpeiden reititystä. 

* **V verkkoa, V-verkon kummallakin alueella väliset yhteydet:** Sinun ei tarvitse on yhteys virtual verkkojen välillä itse. Koska kunkin virtual verkon on pääsy toimialueen ohjaimet ja AD FS ja WAP palvelin on itse, ne toimivat ilman mitään virtual verkkojen eri alueiden väliset yhteydet. 

##<a name="steps-to-integrate-azure-traffic-manager"></a>Jos haluat integroida Azure liikenteen hallinta vaiheet

###<a name="deploy-ad-fs-in-the-new-geographical-region"></a>AD FS uusi maantieteellisen alueen käyttöönotto
Seuraa ohjeita ja ohjeita [Azure AD FS käyttöönottoa](active-directory-aadconnect-azure-adfs.md) käyttöönotto saman topologian uusi maantieteellisen alueen.

###<a name="dns-labels-for-public-ip-addresses-of-the-internet-facing-public-load-balancers"></a>Julkinen Internet vastakkaisten (julkinen) kuormituksen tasoitusmääritykset IP-osoitteiden DNS otsikot
Edellä mainittua Azure liikenteen hallinta voi viitata DNS otsikot päätepisteet kuin vain ja siksi on tärkeää Luo DNS ulkoisen kuormituksen tasoitusmääritykset julkiseen IP-osoitteet. Näyttökuva alapuolella näkyy siitä, miten voit määrittää DNS-otsikon julkiseen IP-osoite. 

![DNS-otsikko](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/eastfabstsdnslabel.png)

###<a name="deploying-azure-traffic-manager"></a>Käyttöönotto Azure liikenteen hallinta

Noudata seuraavia ohjeita voit luoda profiilin liikenteen hallinta. Jos haluat lisätietoja, voit myös katsoa [Azure liikenteen hallinta-profiilin](../traffic-manager/traffic-manager-manage-profiles.md)hallinta.

1. **Liikenteen hallinta profiilin luominen:** Anna liikenteen hallinta profiilin yksilöivä nimi. Tämän profiilin nimi on DNS-nimeen ja etuliite liikenteen hallinta toimialueen nimen otsikkona. Nimen / etuliite lisätään. trafficmanager.net luoda tietoliikenteestä DNS tarran hallinta. Seuraavassa näyttökuvassa näkyy liikenteen hallinta DNS etuliite on määritelty mysts ja tuloksena DNS-otsikon päivitetään mysts.trafficmanager.net. 

    ![Liikenteen hallinta profiilin luominen](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/trafficmanager01.png)
 
2. **Liikenteen reititys menetelmää:** Kolme reititys asetukset ovat käytettävissä liikenteen hallinta:

    * Priority (prioriteetti) 
    * Suorituskyky
    * Painotettu
    
    **Suorituskykyä** on suositellut saavuttamiseksi erittäin vastaa AD FS-infrastruktuuria. Voit kuitenkin käyttöönoton tarpeitasi parhaiten soveltuu reititys menetelmää. AD FS-toiminto ei ole vaikuttaa reititys asetus on valittuna. Lisätietoja on kohdassa [liikenteen hallinta liikenne reititys tavoista](../traffic-manager/traffic-manager-routing-methods.md) . Näyttökuva yläpuolella voit näkee otoksessa valitun **suorituskyvyn** menetelmän.
   
3.  **Määrittäminen päätepisteet:** Napsauta liikenteen hallinta-sivulla Valitse päätepisteet ja valitse Lisää. Tämä avaa Lisää päätepisteen sivulla samanlainen kuin seuraavassa näyttökuvassa
 
    ![Määritä päätepisteet](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/eastfsendpoint.png)
 
    Noudata alla olevia yleisohjeet eri syötteiden:

    **Tyyppi:** Valitse Azure päätepisteen, kuten Microsoft valitsemalla Azure julkiseen IP-osoitteeseen.

    **Nimi:** Luo nimi, jonka haluat yhdistää päätepiste. Tämä ei ole DNS-nimen ja ei liity DNS-tietueet.

    **Kohteen resurssityyppi:** Valitse tämän ominaisuuden arvoksi julkiseen IP-osoite. 

    **Kohteen resurssi:** Näin aloitat valittavana DNS tarrat, sinulla on tilaus-kohdassa haluamasi vaihtoehto. Valitse DNS selitteen avulla.

    Lisää kunkin haluat Azure liikenne esimiehesi reitti-liikenne paikalliseen maantieteellisen alueen päätepiste.
    Lisätietoja ja yksityiskohtaisia ohjeita siitä, miten voit lisätä / määrittäminen päätepisteet liikenteen hallinta viitata, [lisätä, poistaa käytöstä, ota käyttöön tai poistaa päätepisteet](../traffic-manager/traffic-manager-endpoints.md)
    
4. **Määrittäminen näytteenottimen:** Valitse liikenteen hallinta-sivulla määritysten. Sinun on määritys-sivulla tutkia HTTP-portin 80 ja suhteellinen polku /adfs/probe valvonta-asetusten muuttaminen

    ![Määritä näytteenottimen](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/mystsconfig.png) 

    >[AZURE.NOTE] **Varmista, että päätepisteet tilana on online-TILASSA, kun määritys on valmis**. Jos kaikki päätepisteet ovat tila heikentää', Azure liikenteen hallinta tehdä parhaat yritys ja haluat reitittää liikenteen olettaen, että diagnostiikkaa on virheellinen, ja kaikki päätepisteet ovat tavoitettavissa.

5. **Muokkaaminen DNS-tietueiden Azure liikenteen hallinta:** Liitetty viestintä-palvelu on oltava CNAME Azure liikenteen hallinta DNS-nimeä. Luo CNAME julkisen DNS-tietueet niin, että kulloinkin yrittää saavuttamiseksi federation palvelun todella saavuttaa Azure liikenteen hallinta.

    Esimerkiksi osoittavan federation palvelun fs.fabidentity.com liikenteen hallinta, haluat päivittää oman DNS-resurssitietue ovat seuraavat:

    <code>fs.fabidentity.com IN CNAME mysts.trafficmanager.net</code>

##<a name="test-the-routing-and-ad-fs-sign-in"></a>Testaa reititys ja AD FS-kirjautuminen   

###<a name="routing-test"></a>Reititys testi

Hyvin basic testin reititys on kokeilla ping jokaiselle tietyn maantieteellisen alueen tietokoneesta federation palvelun DNS-nimi. Valitun reititys menetelmän mukaan ping-näytössä näkyvät se todellisuudessa Testaa päätepisteelle. Esimerkiksi jos valitsit suorituskyvyn reititys, valitse lähinnä asiakkaan alueen päätepiste saavutetaan. Alla on kaksi Testaa-kaksi eri kohtaan asiakaskoneiden tilannevedoksen, EastAsia alue- ja toinen Länsi US. 

![Reititys testi](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/pingtest.png)

###<a name="ad-fs-sign-in-test"></a>AD FS-kirjautuminen testi

Helpoin tapa testata AD FS on IdpInitiatedSignon.aspx-sivulla. Jotta voit valita, että se on tarpeen, jotta IdpInitiatedSignOn AD FS-ominaisuuksia. Noudata seuraavia ohjeita vahvistamiseksi AD FS-asetukset
 
1. Suorita alapuolella cmdlet-komento, AD FS-palvelimessa, käyttämällä PowerShell-asetukseksi käytössä. Määritä AdfsProperties - EnableIdPInitiatedSignonPage $true
2. Kaikki ulkoisen tietokoneen access https://-<yourfederationservicedns>/adfs/ls/IdpInitiatedSignon.aspx
3. Näyttöön tulee AD FS-sivu, kuten alla:

    ![ADFS-testi - todennus-todennus](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/adfstest1.png)

    ja onnistuneen Kirjaudu sisään, se antaa sinulle onnistui-sanoma alla kuvatulla tavalla:

    ![ADFS-testi - todennus onnistui](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/adfstest2.png)
 
##<a name="related-links"></a>Aiheeseen liittyvät linkit
* [Basic-Azure AD FS käyttöönotto](active-directory-aadconnect-azure-adfs.md)
* [Microsoft Azure liikenteen hallinta](../traffic-manager/traffic-manager-overview.md)
* [Liikenteen hallinta liikenne reititys menetelmät](../traffic-manager/traffic-manager-routing-methods.md)

##<a name="next-steps"></a>Seuraavat vaiheet
* [Azure liikenteen hallinta-profiilin hallinta](../traffic-manager/traffic-manager-manage-profiles.md)
* [Lisääminen, poistaminen käytöstä, ottaminen käyttöön ja poistaminen päätepisteet](../traffic-manager/traffic-manager-endpoints.md) 

