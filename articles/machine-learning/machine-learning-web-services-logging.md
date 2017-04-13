<properties 
    pageTitle="Lokiin kirjaaminen koneen Learning web Services | Microsoft Azure" 
    description="Lue, miten voit ottaa lokiin kirjaamisen käyttöön tietokoneen Learning web services. Lokiin kirjaaminen on Lisätietoja ongelmien API." 
    services="machine-learning" 
    documentationCenter="" 
    authors="raymondlaghaeian" 
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data" 
    ms.date="10/05/2016"
    ms.author="raymondl;garye"/>

# <a name="enable-logging-for-machine-learning-web-services"></a>Ottaa lokiin kirjaamisen käyttöön tietokoneen Learning verkkopalvelut  

Tässä asiakirjassa on tietoja kirjaaminen käyttöön perinteinen WWW-palveluista. Kirjaamisen ottaminen käyttöön verkkopalvelun on lisätietoja, heti virhenumero ja viestin, joka auttaa tietokoneen Learning ohjelmointirajapinnan puhelut vianmääritys lisäksi.  

Voit ottaa kirjaamisen verkkopalvelun Azure perinteinen portaalissa:   

1.  Kirjaudu [Azure perinteinen-portaaliin](https://manage.windowsazure.com/)
2.  Valitse **Tietokoneen LEARNING**vasemman ominaisuudet-sarakkeessa.
3.  Valitse työtilan, valitse **VERKKOPALVELUIHIN**.
4.  Valitse Web services-luettelosta WWW-palvelun nimi.
5.  Valitse päätepisteet-luettelosta päätepisteen nimi.
6.  Valitse **Määritä**.
7.  Määrittää **DIAGNOSTIIKKA JÄLJITÄ** *Virhe* tai *kaikki*ja valitse sitten **Tallenna**.

Voit ottaa kirjaamisen Azure koneen Learning Web Services-portaalissa.

1. Kirjaudu sisään [Azure koneen Learning Web Services](https://services.azureml.net) -portaalissa.
2. Valitse perinteinen verkkopalveluihin.
3.  Valitse Web services-luettelosta WWW-palvelun nimi.
4.  Valitse päätepisteet-luettelosta päätepisteen nimi.
5.  Valitse **Määritä**.
6.  Määrittää **lokiin kirjaaminen** *Virhe* tai *kaikki*ja valitse sitten **Tallenna**.

## <a name="the-effects-of-enabling-logging"></a>Kirjaamisen käyttöönottaminen vaikutukset

Kun kirjaaminen on otettu käyttöön, kaikki diagnostiikka- ja virheet valitun päätepisteestä kirjautunut Azure-tallennustilan tilin linkitetyt käyttäjän Workspacen kanssa. Näet tämän tallennustilan tilin Azure perinteinen portaalissa niiden työtilan Dashboard-näkymä (Quick Glance-osion alaosassa).  

Lokeja voi tarkastella useita työkaluja, tutustu Azure-tallennustilan tilin avulla. Helpoin voi olla vain Siirry Azure perinteinen portaalissa tallennustilan-tili ja valitse sitten **SÄILÖT**. Nähdä säilö **ml diagnostiikka**. Tämä säilö pitää kaikki kaikki web Palvelupäätepisteet kaikki tallennustilan tähän tiliin liittyvät työtilat diagnostiikka-tiedot. 
 
## <a name="log-blob-detail-information"></a>Log-Blob-objektien tiedot

Kunkin blob-säilö pitää diagnostiikka-tiedot, täsmälleen jokin seuraavista:

-   Eräkäsittely menetelmä suorittaminen  
-   Vastaus menetelmä suorittaminen  
-   Pyynnön vastaus säilön alustus
  
Nimeä kukin blob on seuraavassa muodossa etuliite: 

{Työtilan Id} – {WWW-palvelun tunnus} – {päätepisteen tunnus} / {Kirjaudu tyyppi}  

Jos lokin laji on jokin seuraavista arvoista:  

- erän  
- tulos/pyynnöt  
- tulos/alusta  