<properties 
    pageTitle="Cloud välityspalvelimen palveluiden sovelluksen etsiminen rekisteriasetukset | Microsoft Azure" 
    description="Tässä ohjeaiheessa tavoitteena on tarjota vaiheet, sinun on suoritettava voit määrittää tarvittavan portin Cloud App etsiminen agent-tietokoneissa." 
    services="active-directory" 
    documentationCenter="" 
    authors="markusvi" 
    manager="femila"/>

<tags 
    ms.service="active-directory" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/10/2016" 
    ms.author="markusvi"/>

# <a name="cloud-app-discovery-registry-settings-for-proxy-services"></a>Cloud App etsiminen rekisteriasetusten välityspalvelimen palvelut

Oletusarvon mukaan Cloud App etsintä-agentti on määritetty käyttämään vain portit 80 ja 443. Jos suunnittelet asentamisesta Cloud App etsiminen ympäristössä, jossa välityspalvelinta, joka käyttää mukautettuja portin (80 eikä 443), sinun on edustajasi käyttämään portin määrittäminen. Rekisteriavaimen perustuu määritykset.


Tässä ohjeaiheessa tavoitteena on tarjota vaiheet, sinun on suoritettava voit määrittää tarvittavan portin Cloud App etsiminen agent-tietokoneissa.



**Muokattava Cloud App etsintä-agentti tietokoneessa käyttämä portti toimimalla seuraavasti:**


1. Käynnistä Rekisterieditori. <br> ![Suorita](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy01.png)

2. Etsi tai Luo seuraava rekisteriavain: <br> **HKLM_LOCAL_MACHINE\Software\Microsoft\Cloud sovelluksen Discovery\Endpoint** 

3. Luo uusi **Usean merkkijonon** arvo nimeltä **portit**. ![Uusi](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy02.png)

4. Avaa **Usean merkkijonon muokkaaminen** -valintaikkuna kaksoisnapsauttamalla portit-arvo.


5. Kirjoita tiedot arvo-tekstiruutuun seuraavat arvot ja Lisää kaikki mukautetut portit, joita organisaatiossa käytetään: <br><br>
**80** <br>
**8080** <br>
**8118** <br>
**8888** <br>
**81** <br>
**12080** <br>
**6999** <br>
**30606** <br>
**31595** <br>
**4080** <br>
**443** <br>
**1110** <br><br>
![Usean merkkijonon muokkaaminen](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy03.png)

6. Valitse **OK** ja sulje **Usean merkkijonon muokkaaminen** -valintaikkuna.



**Lisäresursseja**


* [Miten unsanctioned cloud-sovellukset, joiden avulla voit tutustua oman organisaation sisällä](active-directory-cloudappdiscovery-whatis.md) 


