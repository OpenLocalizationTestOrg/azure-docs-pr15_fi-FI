<properties
   pageTitle="Yleisiä LDAP-yhdistin | Microsoft Azure"
   description="Tässä artikkelissa käsitellään määrittäminen Microsoftin yleisiä LDAP-yhdistin."
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="08/30/2016"
   ms.author="billmath"/>

# <a name="generic-ldap-connector-technical-reference"></a>Yleisiä LDAP-yhdistin tekniset tiedot
Tässä artikkelissa on yleisiä LDAP-yhdistin. Artikkeli koskee seuraavia tuotteita:

- Microsoftin Käyttäjätietojen hallinta 2016 (MIM2016)
- Forefront käyttäjätietojen hallinta 2010 R2 (FIM2010R2)
    -   Käytä korjaustiedoston 4.1.3671.0 tai uudempi [KB3092178](https://support.microsoft.com/kb/3092178).

MIM2016 ja FIM2010R2 yhdistin on ladattavissa [Microsoft Download Centeristä](http://go.microsoft.com/fwlink/?LinkId=717495).

IETF RFC viitattaessa tämän asiakirjan käyttämällä muotoa (RFC [RFC numero] / [osan RFC asiakirjan]), esimerkiksi (RFC 4512/4.3).
Löydät lisätietoja http://tools.ietf.org/html/rfc4500 (haluat korvata 4500 oikeat RFC-numero).

## <a name="overview-of-the-generic-ldap-connector"></a>Yleistä yleisiä LDAP-yhdistin
Yleisiä LDAP-Connectorin mahdollistaa integroida synkronointipalvelu v3 LDAP-palvelimen kanssa.

Tiettyjen toimintojen ja rakenne-elementtejä, kuten tarpeen delta tuontia ei ole määritetty IETF RFC. Näitä toimintoja tuetaan vain LDAP kansioita, jotka on nimenomaisesti määritetty.

Ylätason näkökulmasta yhdistimen nykyinen versio tukee seuraavia ominaisuuksia:

Toiminto | Tuki
--- | --- |
Yhdistetty tietolähde | Yhdistin tue kaikki LDAP-v3 palvelimet (RFC 4510 yhteensopiva). Se on testattu kanssa seuraavasti: <li>Microsoft Active Directory Lightweight Directory Services (AD LDS)</li><li>Microsoft Active Directoryn Yleinen luetteloon AD</li><li>389 palvelin</li><li>Apache hakemisto-palvelin</li><li>IBM Tivoli DS</li><li>Isode hakemisto</li><li>NetIQ eDirectory</li><li>Novell eDirectory</li><li>Avaa DJ</li><li>Avaa DS</li><li>Avaa LDAP (openldap.org)</li><li>Oracle (aiemmin su) Directory Server Enterprise Edition</li><li>RadiantOne näennäiskansio Server (VDS)</li><li>Su yksi palvelin</li>**Huomattavat kansioita ei tueta:** <li>Microsoft Active Directory-toimialueen palveluista (AD DS) [käyttää valmiita Active Directory-Connectorin sijaan]</li><li>Oracle Internet-hakemisto (OID)</li>
Skenaariot   | <li>Objektin elinkaari hallinta</li><li>Ryhmän hallinta</li><li>Salasanojen hallinta</li>
Toiminnot |Kaikkien LDAP-kansioiden tuetaan seuraavat toimenpiteet: <li>Täysi tuominen</li><li>Vie</li>Seuraavat toimenpiteet tuetaan vain kansioita:<li>Sama.arvo-tuonti</li><li>Aseta salasana, salasanan vaihtaminen</li>
Rakenne | <li>Rakenteen havaitaan LDAP rakenteesta (RFC3673 ja RFC4512/4.2)</li><li>Tukee rakenteellisia luokkien, aux luokat ja extensibleObject objektiluokan (RFC4512/4.3)</li>

### <a name="delta-import-and-password-management-support"></a>Sama.arvo tuominen ja salasanan tiedostohallinnan tuki
Tuetut kansioiden Delta tuonti ja salasanojen hallinta:

- Microsoft Active Directory Lightweight Directory Services (AD LDS)
    - Tukee kaikkia toimintoja delta tuontia varten
    - Tukee määrittäminen
- Microsoft Active Directoryn Yleinen luetteloon AD
    - Tukee kaikkia toimintoja delta tuontia varten
    - Tukee määrittäminen
- 389 palvelin
    - Tukee kaikkia toimintoja delta tuontia varten
    - Tukee salasanan asettaminen ja salasanan muuttaminen
- Apache hakemisto-palvelin
    - Ei tue delta Tuo, koska tämä kansio ei ole pysyvä muutosloki
    - Tukee määrittäminen
- IBM Tivoli DS
    - Tukee kaikkia toimintoja delta tuontia varten
    - Tukee salasanan asettaminen ja salasanan muuttaminen
- Isode hakemisto
    - Tukee kaikkia toimintoja delta tuontia varten
    - Tukee salasanan asettaminen ja salasanan muuttaminen
- Novell eDirectory ja NetIQ eDirectory
    - Lisää, Päivitä ja nimeä toimintoja delta tuontia varten
    - Ei tue Poista toimintoja delta tuontia varten
    - Tukee salasanan asettaminen ja salasanan muuttaminen
- Avaa DJ
    - Tukee kaikkia toimintoja delta tuontia varten
    - Tukee salasanan asettaminen ja salasanan muuttaminen
- Avaa DS
    - Tukee kaikkia toimintoja delta tuontia varten
    - Tukee salasanan asettaminen ja salasanan muuttaminen
- Avaa LDAP (openldap.org)
    - Tukee kaikkia toimintoja delta tuontia varten
    - Tukee määrittäminen
    - Ei tue salasanan muuttaminen
- Oracle (aiemmin su) Directory Server Enterprise Edition
    - Tukee kaikkia toimintoja delta tuontia varten
    - Tukee salasanan asettaminen ja salasanan muuttaminen
- RadiantOne näennäiskansio Server (VDS)
    - On käytettävä 7.1.1 versio tai sitä uudemmassa versiossa
    - Tukee kaikkia toimintoja delta tuontia varten
    - Tukee salasanan asettaminen ja salasanan muuttaminen
-  Su yksi palvelin
    - Tukee kaikkia toimintoja delta tuontia varten
    - Tukee salasanan asettaminen ja salasanan muuttaminen

### <a name="prerequisites"></a>Edellytykset
Ennen kuin käytät yhdistimen, varmista, että sinulla on synkronointi-palvelimeen seuraavasti:

- Microsoft .NET 4.5.2 Framework tai uudempi versio

### <a name="detecting-the-ldap-server"></a>LDAP-palvelimen tunnistaminen
Yhdistimen käyttää erilaisia menetelmiä havaitsemaan ja määritä LDAP-palvelimen yhteydessä. Yhdistimen käyttää pääkansion DSE, toimittajan nimi/versio, ja se tarkistaa rakenteen etsiminen yksilöllinen kohteet ja määritteet löytääkseen olevia tiettyjä LDAP-palvelimiin. Nämä tiedot, jos löydy, käytetään Täytä valmiiksi yhdistimen määritykset-asetukset.

### <a name="connected-data-source-permissions"></a>Yhdistetty tietolähteeseen käyttöoikeudet
Tuo ja vie toimenpiteet yhdistetyn kansion objekteja, connector-tilin on oltava riittäviä käyttöoikeuksia. Yhdistimen on oltava kirjoitusoikeudet, jos haluat viedä ja Luku-käyttöoikeudet, voit tuoda. Käyttöoikeuden määritys on suorittaa kohdekansion itse hallinta-kokemukset.

### <a name="ports-and-protocols"></a>Portit ja protokollat
Yhdistimen käyttää määrityksestä, joka on 389 LDAP ja 636 LDAPS oletusarvoisesti määritetty porttinumero.

LDAPS sinun on käytettävä SSL 3.0- tai TLS. SSL 2.0 ei tueta, joten niitä ei voi aktivoida.

### <a name="required-controls-and-features"></a>Tarvittavat ohjausobjektit ja toiminnot
Seuraavat LDAP-ohjausobjektit ja ominaisuudet on oltava käytettävissä toimii oikein yhdistimen LDAP-palvelimeen:  
`1.3.6.1.4.1.4203.1.5.3`TOSI tai EPÄTOSI suodattimet

TOSI tai EPÄTOSI-suodatin on usein ei ole valmiiksi tukemat LDAP-kansioiden ja saattavat näyttää **Yleinen sivun** **Pakollinen ominaisuuksia ei löydy**. Sitä käytetään **suodattimien** luominen LDAP-kyselyt, esimerkiksi kun tuot useita objektityypit. Jos haluat tuoda useita objektityyppi, LDAP-palvelin tukee tätä ominaisuutta.

Jos käytät hakemiston missä yksilöllinen ankkurin seuraavat on oltava käytettävissä (Katso lisätietoja tämän artikkelin [Määrittäminen ankkurit](#configure-anchors) -kohta):  
`1.3.6.1.4.1.4203.1.5.1`Kaikki toiminnallisia määritteet

Jos kansio on enemmän kuin yksi kutsussa hakemiston mahtuu objekteja, valitse on suositeltavaa käyttää sivutus. Sivutuksen toimimaan, sinun on jokin seuraavista vaihtoehdoista:

**Vaihtoehto 1:**  
`1.2.840.113556.1.4.319`pagedResultsControl

**Vaihtoehto 2:**  
`2.16.840.1.113730.3.4.9`VLVControl  
`1.2.840.113556.1.4.473`SortControl

Jos connector-kokoonpanon määritys molemmat vaihtoehdot on otettu käyttöön, pagedResultsControl käytetään.

`1.2.840.113556.1.4.417`ShowDeletedControl

ShowDeletedControl käytetään vain USNChanged delta tuonti-menetelmällä voi tarkastella poistettujen objektien.

Yhdistimen yrittää tunnistaa palvelimessa esitä asetukset. Jos vaihtoehtoa ei löydy, Varoitus ei sisällä tietoja Yleiset-sivulla yhdistimen ominaisuuksiin. Kaikki LDAP-palvelimet esitä kaikki komponentit/ominaisuuksia ne tue ja vaikka tämä varoitus ei sisällä tietoja, yhdistimen voi työskennellä ilman ongelmat.

### <a name="delta-import"></a>Sama.arvo-tuonti
Sama.arvo tuo on käytettävissä vain, kun tuki-kansio on jo olemassa. Tällä hetkellä käytetään seuraavista tavoista:

- LDAP-Accesslog. Katso [http://www.openldap.org/doc/admin24/overlays.html#Access lokiin kirjaaminen](http://www.openldap.org/doc/admin24/overlays.html#Access Logging)
- LDAP-Changelog. Katso [http://tools.ietf.org/html/draft-good-ldap-changelog-04](http://tools.ietf.org/html/draft-good-ldap-changelog-04)
- Aikaleima. Yhdistimen käyttää Novell/NetIQ eDirectory viimeinen päivämäärä ja kellonaika saamiseen luodaan ja päivittää objekteja. Novell/NetIQ eDirectory ei tarjoa sitä vastaavassa tarkoittaa poistettujen objektien hakemiseen. Tämä vaihtoehto voidaan myös, jos LDAP-palvelimen käytössä ei ole muita delta tuo menetelmää. Tämä asetus ei voi poistaa Tuo objektit.
- USNChanged. Lisätietoja: [https://msdn.microsoft.com/library/ms677627.aspx](https://msdn.microsoft.com/library/ms677627.aspx)

### <a name="not-supported"></a>Ei tueta
LDAP seuraavia ominaisuuksia ei tueta:

- LDAP-viitteitä, palvelinten (RFC 4511/4.1.10)

## <a name="create-a-new-connector"></a>Luo uusi yhdistin
Voit luoda yleisiä LDAP-yhdistin **Synkronointipalvelu** Valitse **Hallinta-agentti** ja **Luo**. Valitse yhdistin **yleisiä LDAP (Microsoft)** .

![CreateConnector](./media/active-directory-aadconnectsync-connector-genericldap/createconnector.png)

### <a name="connectivity"></a>Yhteys
Yhteys-sivulla määritettävä Host porteista ja sidonta-tiedot. Sen mukaan, joka sidonta on valittuna, lisää tiedot voi toimittaa seuraavissa osissa.

![Yhteys](./media/active-directory-aadconnectsync-connector-genericldap/connectivity.png)

- Yhteyden aikakatkaisu-asetusta käytetään vain ensimmäisen yhteyden palvelimeen, kun tunnistaminen rakenne.
- Jos sidonta on anonyymi, valitse kumpaakaan käyttäjänimi tai salasana eikä varmennetta käytetään.
- Muut sidontojen, kirjoita tiedot joko tekstimuodossa käyttäjänimi ja salasana ja valitse varmenne.
- Jos käytössäsi on Kerberos tarkistamiseen myös Anna käyttäjän alueen/toimialue.

**Määritteen aliases** tekstiruudun käytetään määritteitä, jotka on määritetty rakenteessa RFC4522 syntaksin. Seuraavanlaisia määritteitä ei tunnisteta rakenteen tunnistuksen aikana ja yhdistimen on oltava auttavat tunnistamaan nämä määritteet. Esimerkiksi seuraavat tarvitaan voidaan kirjoittaa tunnistavan oikein binaarinen määritettä userCertificate määritteen määritettä aliases-ruudussa:

`userCertificate;binary`

Seuraavassa on esimerkkejä siitä, miten määritysten saattaa näyttää:

![Yhteys](./media/active-directory-aadconnectsync-connector-genericldap/connectivityattributes.png)

Valitse myös sisällyttää määritteitä, jotka on luotu palvelin **ovat toiminnallisia määritteitä rakenteessa** -valintaruutu. Nämä ovat määritteitä, esimerkiksi kun objekti on luotu ja Päivitä edellisen kerran.

Valitse **Sisällytä laajennettavat määritteet rakenteessa** , jos on käytetty extensible objekteja (RFC4512/4.3) ja tämän asetuksen avulla jokaisen määrite, jota käytetään kaikki objektissa. Tämä vaihtoehto mahdollistaa rakenteen erittäin suuri niin ellei yhdistetyn hakemiston Käytä tätä ominaisuutta suositus on estää ei valittu vaihtoehto.

### <a name="global-parameters"></a>Yleiset parametrit
Yleiset parametrit-sivulla voit määrittää LDAP-lisäominaisuudet ja delta muutosloki DN. Sivu on valmiiksi toimittamien tietojen perusteella LDAP-palvelimen kanssa.

![Yhteys](./media/active-directory-aadconnectsync-connector-genericldap/globalparameters.png)

Yläosassa näkyy tietoja antama palvelimen itse, kuten palvelimen nimi. Yhdistimen myös tarkistaa, että pakollinen ohjausobjektit ovat pääkansion DSE. Jos ohjausobjektit eivät näy, Varoitus esitetään. Jotkin LDAP-kansioita ei tehtäväluettelo pääkansion DSE kaikki toiminnot ja on mahdollista, että yhdistimen toimii ongelmaton, vaikka varoitus ei sisällä tietoja.

**Tuetut ohjausobjektien** valintaruudut ohjataan tiettyjä toimintoja varten:

- Jossa on valittuna puun Poista hierarkian poistetaan yksi LDAP-puheluun. Puun Poista ei valittu yhdistin ei rekursiivinen Poista tarvittaessa.
- Yhdistin ei ja sivutettu tulos on valittuna, sivutettu Tuo koko, joka on määritetty Suorita vaiheet.
- VLVControl ja SortControl on vaihtoehtoinen pagedResultsControl voi lukea tietoja LDAP-hakemisto.
- Jos kaikki kolme vaihtoehtoa (pagedResultsControl, VLVControl ja SortControl) eivät ole yhdistimen Tuo kaikki objektin yhdellä kertaa, joka saattaa epäonnistua, jos se on suuri kansio.
- ShowDeletedControl käytetään vain, kun Delta tuo menetelmä USNChanged.

Muutosloki DN on nimeämiskäytäntö yhteydessä käyttämien delta muutoslokin, kuten **cn = changelog**. Tämä arvo on määritettävä tehdä delta Tuo.

Seuraavassa on luettelo oletusarvon muutosloki DNs:

Hakemisto | Sama.arvo muutosloki
--- | ---
Microsoft AD LDS ja AD yleisen luettelon | Tunnistaa automaattisesti. USNChanged.
Apache hakemisto-palvelin | Ei ole käytettävissä.
Hakemiston 389 | Muutosloki. Oletusarvo käyttämään: **cn = changelog**
IBM Tivoli DS | Muutosloki. Oletusarvo käyttämään: **cn = changelog**
Isode hakemisto | Muutosloki. Oletusarvo käyttämään: **cn = changelog**
Novell/NetIQ eDirectory | Ei ole käytettävissä. Aikaleima. Yhdistimen käyttää päivitetty päivämäärä ja kellonaika, jos haluat lisätä ja päivittää tietueita.
Avaa DJ/DS | Muutosloki.  Oletusarvo käyttämään: **cn = changelog**
Avaa LDAP | Access-loki. Oletusarvo käyttämään: **cn = accesslog**
Oracle DSEE | Muutosloki. Oletusarvo käyttämään: **cn = changelog**
RadiantOne VDS | Näennäiskansio. Määräytyy yhteydessä VDS hakemiston.
Su yksi palvelin | Muutosloki. Oletusarvo käyttämään: **cn = changelog**

Salasana-määrite on yhdistimen olisi avulla voit määrittää salasanan salasanan vaihtaminen määritteen nimi ja salasana set-toiminnot.
Tämä arvo on oletusarvoisesti **userPassword** , mutta voi muuttaa tietyn LDAP-järjestelmän tarvittaessa.

Muita osioita-luettelossa on mahdollista lisätä muita nimitilan havaita automaattisesti. Esimerkiksi tämä asetus voidaan, jos useita palvelimet muodostavat looginen klusterin, jossa on kaikki tuodaan yhtä aikaa. Active Directory voi olla useita toimialueita yhden metsää, mutta kaikki toimialueet jakaminen rakenteita, sama voit Simuloitu kirjoittamalla muita nimitilan tähän ruutuun. Kunkin nimitilan eri palvelimiin tuoda, ja se on määritetty edelleen määrittää osiot ja hierarkiat-sivulla. Käyttää Ctrl + Enter avulla uusi rivi.

### <a name="configure-provisioning-hierarchy"></a>Määritä valmistelu hierarkia
Tällä sivulla käytetään DN-osa, kuten OU yhdistäminen objektityyppi, joka on valmisteltu, kuten organizationalUnit.

![Hierarkian valmistelu](./media/active-directory-aadconnectsync-connector-genericldap/provisioninghierarchy.png)

Määrittämällä valmistelu hierarkiaa voit määrittää yhdistimen automaattisesti tarvittaessa rakenteen luominen. Esimerkiksi jos nimitilan ohjauskoneen = contoso-ohjauskoneen = com ja uuden objektin cn = Esko, ou = Seattle, c = FI, ohjauskoneen = contoso-ohjauskoneen = com on valmisteltu ja sitten yhdistimen voit luoda objektin tyyppi maan USA ja organizationalUnit Seattle, jos ne eivät ole jo valmiina hakemistossa.

### <a name="configure-partitions-and-hierarchies"></a>Määritä osiot ja hierarkiat
Valitse osiot ja hierarkiat-sivulla kaikki nimitilan aiot tuominen ja vieminen-objektien kanssa.

![Osiot](./media/active-directory-aadconnectsync-connector-genericldap/partitions.png)

Kunkin nimitilan on myös mahdollista määrittää yhteysasetukset, jotka ohittaa Connectivity näytössä määritetyt arvot. Jos nämä arvot ovat lisänneet tyhjä oletusarvoon, yhteys näytössä tietoja käytetään.

On myös mahdollista valita, mitkä säilöjä ja organisaatioyksiköiden yhdistimen olisi tuominen ja vieminen yhteystiedoista.

### <a name="configure-anchors"></a>Määritä ankkurit
Tämä sivu on aina esimääritettyjä arvo, eikä niitä voi muuttaa. Jos palvelimen toimittaja on määritetty, sitten ankkuri saattaa täyttää pysyvä määrite, kuten objektin GUID. Jos se ei tunnisteta tai tiedetään ei ole pysyvä määrite, yhdistimen käyttää dn (DN-nimi) kuin ankkurin.

![ankkurit](./media/active-directory-aadconnectsync-connector-genericldap/anchors.png)

Seuraavassa on luettelo LDAP-palvelimia ja ankkurin käytössä:

Hakemisto | Ankkuri-määrite
--- | ---
Microsoft AD LDS ja AD yleisen luettelon | objectGUID
389 palvelin | DN
Apache hakemisto | DN
IBM Tivoli DS | DN
Isode hakemisto | DN
Novell/NetIQ eDirectory | GUID-TUNNUS
Avaa DJ/DS | DN
Avaa LDAP | DN
Oracle ODSEE | DN
RadiantOne VDS | DN
Su yksi palvelin | DN

## <a name="other-notes"></a>Muut muistiinpanot
Tässä osassa on tietoja, jotka ovat tämän yhdistimen tai muista syistä on tärkeää tietää ominaisuuksia.

### <a name="delta-import"></a>Sama.arvo-tuonti
Avaa LDAP delta vesileiman on UTC-ajan päivämäärä ja kellonaika. Tästä syystä on synkronoitava kellonaikoja FIM synkronointipalvelu ja avaa LDAP välillä. Muussa tapauksessa merkintöjä delta muutoslokin voi jättää pois.

Saat Novell eDirectory delta-tuonti ei tunnistaa minkä tahansa objektin poistaa. Tästä syystä on säännöllisesti, voit etsiä kaikki poistettujen objektien koko tuonnin suorittaminen.

Kansioiden ja delta muutoslokin, joka perustuu päivämäärä ja kellonaika on erittäin suositeltavaa suorittaa täysi tuominen kausittaisen aikoina. Tämän prosessin avulla sync engine etsimiseen ja liiketoimintasektoria LDAP-palvelimen ja mitä connector-tilaan.

## <a name="troubleshooting"></a>Vianmääritys

-   Saat lisätietoja siitä, miten voit ottaa kirjaamisen vianmääritys yhdistin, [Voit ottaa käyttöön tapahtumien seuranta jäljitys yhdistimet](http://go.microsoft.com/fwlink/?LinkId=335731).
