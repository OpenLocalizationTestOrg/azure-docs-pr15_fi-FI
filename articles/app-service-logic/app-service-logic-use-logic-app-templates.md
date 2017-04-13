<properties
 pageTitle="Logiikan sovellusmallit | Microsoft Azure"
 description="Lue, miten voit käyttää valmiiksi luotuja logiikan sovellusmallit avulla pääset alkuun"
 authors="kevinlam1"
 manager="dwrede"
 editor=""
 services="app-service\logic"
 documentationCenter=""/>

<tags
    ms.service="app-service-logic"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/24/2016"
    ms.author="klam"/>

# <a name="logic-app-templates"></a>Logiikan sovellusmallit

## <a name="what-are-logic-app-templates"></a>Mitä logiikan sovellusmallit ovat

Logiikan-sovelluksen malli on valmiita logiikan sovellus, joiden avulla voit nopeasti pääset alkuun oman työnkulun luominen. 

Nämä mallit ovat erinomainen tapa Tutustu eri kuviot, jotka on luotu logiikan sovellukset. Voit käyttää näitä mallit- tai voit muokata niitä käyttämässäsi skenaariossa.

## <a name="overview-of-available-templates"></a>Yleiskatsaus käytettävissä olevat mallit

On monia julkaistu app logiikan ympäristön käytettävissä olevat mallit. Esimerkki luokkia sekä käyttää niitä, yhdistimet tyypin näkyvät jäljempänä.

### <a name="enterprise-cloud-templates"></a>Cloud yritysmallit
Malleilla, jotka integroida Dynamics CRM, Salesforce, ruutuun, Azure-Blob-objektien ja muut yhdysviivat yrityksen cloud tarpeisiin. Esimerkkejä, mitä voit tehdä näihin mallit sisältävät järjestäminen liidejä ja yrityksen tiedoston tietojen varmuuskopioinnin.

### <a name="enterprise-integration-pack-templates"></a>Integrointi pack yritysmallit
VETER määrityksiä (Vahvista Pura transform, täydentää, reitittää) putkistot, vastaanotto X12 Muokkaa asiakirjan päälle AS2 sekä muodonmuutoksen XML-X12 ja AS2 viestinä käsittely.

### <a name="protocol-pattern-templates"></a>Protokollan kuvio-mallit
Nämä mallit koostuvat logiikan sovellukset, jotka sisältävät protokolla kuviot-vastaus esimerkiksi yli HTTP sekä integroinnit FTP ja SFTP. Käytä näitä olemassa tai perustana monimutkaisia protokolla kuviot luomiseen.  

### <a name="personal-productivity-templates"></a>Tuottavuutta mallit
Kuvioiden avulla ja parantaa tuottavuutta sisältää malleja, joita päivittäinen muistutusten, tärkeitä töitä muuttuvat tehtäväluetteloita ja pitkiä automatisointiin kohtaan kertakäyttäjä hyväksynnän vaiheen.

### <a name="consumer-cloud-templates"></a>Kuluttaja cloud mallit
Yksinkertainen malleilla, jotka integroida Twitter-, liukuma- ja sähköposti-kädessä voi vahvistaminen markkinoinnin hankkeita Yhteisöpalvelut Yhteisöpalvelut palveluihin. Näitä ovat myös pilvistä kopioiminen, kuten malleja, jotka auttavat parantamaan tuottavuutta tallentamalla aika perinteisesti toistuvia tehtäviä. 

## <a name="how-to-create-a-logic-app-using-a-template"></a>Logiikan sovellusta mallin luominen 

Logiikan app mallin avulla pääset alkuun, siirry logiikan sovelluksen suunnittelu. Jos lisäät suunnittelija avaamalla aiemmin logiikan-sovelluksen, logiikan sovelluksen lataa automaattisesti suunnittelutyökalun näkymässä. Jos olet luomassa uutta logiikan-sovellusta, näkyviin tulee viesti alla.  
 ![](../../includes/media/app-service-logic-templates/template7.png)  

Tässä näytössä joko voit alkaa tyhjä logiikan sovelluksen tai valmiita mallina. Jos valitset mallit, ovat mukana lisätiedot. Tässä esimerkissä Käytämme *luotaessa uuden tiedoston dropbox, kopioi se onedrive* -malli.  
 ![](../../includes/media/app-service-logic-templates/template2.png)  

Jos haluat käyttää mallia, valitse *Käytä tätä mallia* -painiketta. Sinua pyydetään kirjautumaan mukaan asiakkaiden mitä yhdistimien mallia käyttämällä. Tai jos olet aiemmin muodostanut yhteyden nämä yhdistimillä, voit valita samalla tavalla kuin jäljempänä.  
 ![](../../includes/media/app-service-logic-templates/template3.png)  

Kun muodostaa yhteys ja valitsemalla *Jatka*, logiikan sovellus avautuu suunnittelutyökalun näkymässä.  
 ![](../../includes/media/app-service-logic-templates/template4.png)  

Yllä olevassa esimerkissä on useita malleja kirjoitusmuotoon osa pakollinen ominaisuuskentät saattaa täyttää sisällä yhdistimiä. Jotkin kuitenkin yhä määritettävä arvo ennen ei voi logiikan-sovelluksen käyttöönotto oikein. Jos yrität käyttöön ilman, että kirjoitat puuttuu kenttiä, saat ilmoituksen, ja näyttöön tulee virhesanoma.

Jos haluat palata mallin Viewer-ohjelman, valitse *Mallit* -painike siirtymispalkissa. Siirtyminen malli-katseluohjelman menetät kaikki tallentamattomat käynnissä. Ennen siirtymistä takaisin mallin tarkastelu, näet tämän ilmoituksen varoitussanoma.  
 ![](../../includes/media/app-service-logic-templates/template5.png)  

## <a name="how-to-deploy-a-logic-app-created-from-a-template"></a>Voit luoda mallin pohjalta logiikan sovelluksen käyttöönotto

Kun olet mallin lataaminen ja tehnyt haluamasi muutokset, valitse Tallenna-painike näytön vasemmassa yläkulmassa. Tämä toiminto tallentaa ja julkaisee logiikan sovelluksen.  
 ![](../../includes/media/app-service-logic-templates/template6.png)  

Jos haluat lisätietoja Lisää vaiheita aiemmin logiikan sovellusmallin kyselyjä ja varmista Muokkaa, Lisätietoja on osoitteessa [logiikan sovelluksen luominen](app-service-logic-create-a-logic-app.md).