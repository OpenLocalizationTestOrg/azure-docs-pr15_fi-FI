<properties 
    pageTitle="Myöntäjän nimi ja myöntäjän avaimen BizTalk Services-palveluissa | Microsoft Azure" 
    description="Lue, miten noutamiseen myöntäjän nimi ja myöntäjän avaimen palvelun Bus tai Access ohjausobjektin (ACS) BizTalk Services-palveluissa. MAB-WABS" 
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




# <a name="biztalk-services-issuer-name-and-issuer-key"></a>BizTalk Services: Myöntäjän nimi ja myöntäjä avain

Azure BizTalk Services käyttää palvelun Bus myöntäjän nimi ja myöntäjä-näppäintä, Access ohjausobjektin myöntäjän nimi ja myöntäjä-näppäintä. Tarkemmin:

Tehtävä | Mitä myöntäjän nimi ja myöntäjä avain
--- | ---
Visual Studio sovelluksen käyttöönotto | Ohjausobjektin myöntäjän nimi ja myöntäjä avain
Azure BizTalk palveluiden portaali määrittäminen | Ohjausobjektin myöntäjän nimi ja myöntäjä avain
LOB releitä luominen Visual Studiossa BizTalk sovittimen-palveluihin | Palvelun Bus myöntäjän nimi ja myöntäjä avain

Tässä artikkelissa on lueteltu vaiheet myöntäjän nimi ja myöntäjän avaimen hakemiseen. 

## <a name="access-control-issuer-name-and-issuer-key"></a>Ohjausobjektin myöntäjän nimi ja myöntäjä avain
Accessin ohjausobjektin myöntäjän nimi ja myöntäjän avaimen käytetään seuraavasti:

- Azure BizTalk palvelusovelluksen Visual Studiossa luodut: BizTalk palvelusovelluksen Visual Studiossa käyttöön Azure onnistuneesti, kirjoitat Access ohjausobjektin myöntäjän nimi ja myöntäjä-näppäintä. 
- Azure BizTalk palveluiden portaali: Kun luot BizTalk palvelu ja Avaa BizTalk-palveluiden portaali, Access ohjausobjektin myöntäjän nimi ja myöntäjän avaimen rekisteröidään automaattisesti saman käytönvalvonta arvoilla käyttöönoton varten.

### <a name="to-copy-and-paste-the-access-control-issuer-name-and-issuer-key"></a>Voit kopioida ja liittää Access ohjausobjektin myöntäjän nimi ja myöntäjä avain

1. Kirjautuminen [Azure perinteinen portal](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. Valitse vasemmassa siirtymisruudussa **BizTalk palvelut**.
3. Valitse BizTalk-palvelu. 
4. Valitse **Yhteystiedot** tehtäväpalkista. Access-ohjausobjekti, Namespace oletus myöntäjän (myöntäjän nimi) ja oletusarvoiset avainta (myöntäjä) on lueteltu ja voit kopioitavan ja liitettävän.  

Yhteenveto:  
Myöntäjän nimi = oletusarvon myöntäjä  
Myöntäjän avaimen = oletusarvo-näppäin


Voit myös valita **Avaa ACS hallinta-portaalin** hakee käytönvalvonta arvot:

1. Kirjautuminen [Azure perinteinen portal](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. Valitse vasemmassa siirtymisruudussa **BizTalk palvelut**.
3. Valitse BizTalk-palvelu.
4. Valitse Yhteystiedot-painiketta ja valitse **Avaa ACS hallinta-portaalin**.
5. Valitse **Palveluasetukset**-kohdassa portaalissa **Palvelun käyttäjätietojen**. Palvelun jäsenyyden, Access ohjausobjektin myöntäjän nimi-arvo eli näkyviin. Valitse palvelun Identity-linkki Nähdäksesi salasanan, joka on myöntäjä avain-arvo. Voit kopioida niiden arvot.<br/><br/>
Jos esimerkiksi **Palvelun käyttäjätietojen**, näet "omistaja". "Omistaja" on Access ohjausobjektin myöntäjän nimi. Kun napsautat "omistaja"-linkkiä, näyttöön tulee **salasana**. Kun napsautat "Salasana"-linkkiä, näet arvon. Salasana tämä arvo on Access ohjausobjektin myöntäjä Key-tuotetunnuksen.  

Yhteenveto:  
Myöntäjän nimi = palvelun käyttäjätietojen nimi  
Myöntäjän avaimen = salasanan arvo

Valitse vasemmassa siirtymisruudussa voit valita myös **Active Directory** käyttöoikeuksien valvonta-arvojen noutamiseen. 

> [AZURE.IMPORTANT]Kun Access-ohjausobjektin Namespace luodaan **Active Directory**-palvelun tunnistetietojen avulla **ei** luoda automaattisesti. Kun BizTalk-palvelun käytön hallinta Namespace valmistella palvelun käyttäjätietojen nimeltä "omistaja" (myöntäjän nimi), salasana (myöntäjä avain), ja symmetrisen avaimen luodaan automaattisesti.<br /> 
[Toimintaohje: Käytä ACS hallintapalvelun määrittäminen palvelun käyttäjätiedot](http://go.microsoft.com/fwlink/p/?LinkID=303942) tarjoaa lisätietoja Access ohjausobjektin palvelun käyttäjätietojen.


## <a name="service-bus-issuer-name-and-issuer-key"></a>Palvelun Bus myöntäjän nimi ja myöntäjä avain
Palvelun Bus myöntäjän nimi ja myöntäjän avaimen avulla BizTalk sovittimen palvelut. BizTalk Services projektin Visual Studiossa Käytä BizTalk sovittimen palvelut muodostaa paikalliseen järjestelmään, liiketoiminta-(LOB). Jos haluat yhdistää, Luo LOB välitys ja kirjoita LOB-järjestelmän tiedot. Kun teet tämän, voit kirjoittaa myös palvelun Bus myöntäjän nimi ja myöntäjä-näppäintä.

### <a name="to-retrieve-the-service-bus-issuer-name-and-issuer-key"></a>Palvelun Bus myöntäjän nimi ja myöntäjän avaimen hakemiseen

1. Kirjautuminen [Azure perinteinen portaaliin](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. Valitse vasemmassa siirtymisruudussa **Palvelun Bus**.
3. Valitse oman nimitilan. Valitse tehtäväpalkissa **Yhteyden tietoja**. Tämä näyttää **Oletus myöntäjän** (myöntäjän nimi) ja **Oletusarvoiset avain** (myöntäjä avain). Voit kopioida niiden arvot.  

Yhteenveto:  
Myöntäjän nimi = oletusarvon myöntäjä  
Myöntäjän avaimen = oletusarvo-näppäin

## <a name="next"></a>Seuraava
Azure BizTalk Services aiheisiin:

-  [Azure BizTalk Services SDK asentaminen](http://go.microsoft.com/fwlink/p/?LinkID=241589)<br/>
-  [Oppaat: Azure BizTalk palvelut](http://go.microsoft.com/fwlink/p/?LinkID=236944)<br/>
-  [Miten voin aloittaa käyttämällä Azure BizTalk Services SDK-paketissa](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
-  [Azure BizTalk palvelut](http://go.microsoft.com/fwlink/p/?LinkID=303664)<br/>


## <a name="see-also"></a>Katso myös
-  [Toimintaohje: ACS hallintapalvelun avulla voit määrittää palvelun käyttäjätietojen](http://go.microsoft.com/fwlink/p/?LinkID=303942)<br/>
- [BizTalk Services: Kehittäjä, Basic, Vakio ja Premium versiot kaavio](http://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
- [BizTalk Services: Valmistelu käyttämällä Azure perinteinen portal](http://go.microsoft.com/fwlink/p/?LinkID=302280)<br/>
- [BizTalk Services: Tila-kaavion valmistelu](http://go.microsoft.com/fwlink/p/?LinkID=329870)<br/>
- [BizTalk Services: Raporttinäkymät-ikkunan, näyttö ja skaalaus-välilehdet](http://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
- [BizTalk Services: Varmuuskopiointi ja palauttaminen](http://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
- [BizTalk Services: rajoittaminen](http://go.microsoft.com/fwlink/p/?LinkID=302282)<br/>
 
