<properties
    pageTitle="Suuntaamista Koulujen mallin koneeseen | Microsoft Azure"
    description="Opettele suuntaamista mallin ja Päivitä juuri koulutetun mallin käyttäminen Azure koneen Learning Web-palveluun."
    services="machine-learning"
    documentationCenter=""
    authors="vDonGlover"
    manager="raymondl"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="v-donglo"/>

# <a name="retrain-a-machine-learning-model"></a>Suuntaamista Konepohjaisten Oppimistekniikoiden malli

Osana operationalization machine learning mallien Azure Konepohjaisten Oppimistekniikoiden prosessin mallin koulutus ja tallennetaan. Valitse sen avulla luodaan predicative verkkopalvelun. Web-palvelu on kulutettu sitten sivustoista, raporttinäkymiä ja mobile-sovellukset. 

Käyttää tietokoneen Learning mallit eivät ole yleensä staattinen. Uusien tietojen tullessa käytettävissä-tilaan tai kun Ohjelmointirajapinnan kuluttaja on omia tietoja mallin on oltava retrained. 

Uudelleenkoulutusta saattaa ilmetä usein. Ohjelmallisesti uudelleenkoulutusta API-ominaisuudella voit ohjelmallisesti suuntaamista uudelleenkoulutusta-ohjelmointirajapinnan käyttäminen mallin ja päivittää WWW-palvelun juuri koulutetun mallia. 

Tämän asiakirjan kuvaa uudelleenkoulutuksen ja näytetään, miten voit käyttää uudelleenkoulutusta-ohjelmointirajapinnan.

## <a name="why-retrain-defining-the-problem"></a>Miksi suuntaamista: ongelman määrittäminen  

Osana konepohjaisten oppimistekniikoiden koulutusprosessin malli on koulutus käyttämällä tietojoukko. Käyttää tietokoneen Learning mallit eivät tavallisesti staattinen. Uusien tietojen tullessa käytettävissä-tilaan tai kun Ohjelmointirajapinnan kuluttaja on omia tietoja mallin on oltava retrained.

Näissä tilanteissa ohjelmallisesti Ohjelmointirajapinnan on kätevä tapa sallivat sinun tai oman ohjelmointirajapinnan kuluttaja asiakas, joka voi erikseen tai säännöllisin väliajoin suuntaamista käyttämällä omia tietoja mallin luominen. He tulosten uudelleenkoulutusta ja päivittää Web service API käyttämään juuri koulutetun malli.

>[AZURE.NOTE] Jos olet aiemmin koulutus kokeessa ja uusi Web-palvelu, haluat ehkä kuitata ulos uudelleenohjausten aiemmin ennakoivan Web-palveluun seuraavan seuraavassa osassa ongelmatilanteita sijaan.

## <a name="end-to-end-workflow"></a>Lopusta loppuun-työnkulku 

Prosessiin kuuluu seuraavat osat: A koulutus kokeessa ja ennakoivan kokeen julkaistaan verkkopalvelun. Jatkokoulutuksen koulutetun mallin käyttöön koulutus kokeen julkistaa verkkopalvelun koulutetun mallin tulosteen kanssa. Näin API käyttöoikeuden mallin uudelleenkoulutusta. 

Seuraavat vaiheet koskevat vain uudet ja perinteinen Web services:

Luo ensimmäinen ennakoivan Web-palvelu:

* Koulutus kokeen luominen
* Ennakoivan web kokeen luominen
* Ennakoivan verkkopalvelun käyttöönotto

Suuntaamista Web-palvelu:

* Päivitä koulutus kokeen sallimaan uudelleenkoulutusta
* Ottaa käyttöön uudelleenkoulutuksen web-palvelu
* Mallin suuntaamista erä suorittamisen Service-koodin avulla

Katso edellisten vaiheiden vaiheittainen [suuntaamista koneen Learning mallit ohjelmallisesti](machine-learning-retrain-models-programmatically.md).

Jos olet jo käyttöön perinteinen Web-palvelu:

* Luo uusi päätepiste ennakoivan Web-palveluun
* KORJAUSTIEDOSTON URL-osoite ja koodi
* Osoita uuden päätepisteen retrained mallin KORJAUSTIEDOSTON URL-Osoitteen avulla 

Katso edellisten vaiheiden vaiheittainen [suuntaamista perinteinen verkkopalvelun](machine-learning-retrain-a-classic-web-service.md).

Jos kohtaat ongelmia uudelleenkoulutusta perinteinen verkkopalvelun, tutustu [vianmääritys Azure koneen Learning perinteinen WWW-palvelun uudelleenkoulutusta](machine-learning-troubleshooting-retraining-models.md).

Jos olet asentanut uuden Web-palvelu:

* Azure Resurssienhallinta-tiliisi kirjautuminen
* Saat WWW-palvelun määritys
* Vie JSON WWW-palvelun määritys
* Päivitä viittaus `ilearner` blob JSON
* JSON tuominen Web-palvelu-määritys
* Päivitä uuden WWW-palvelumäärityksen Web-palvelu

Katso edellisten vaiheiden vaiheittainen [suuntaamista uusi verkkopalvelun avulla tietokoneen Learning hallinta PowerShellin cmdlet-komennot](machine-learning-retrain-new-web-service-using-powershell.md).

Määritä käsittää perinteinen verkkopalvelun prosessi etenee vaiheittain seuraavasti:

![Prosessin yleiskuvaus uudelleenkoulutusta][1]

Määritä uusi Web-palvelu käsittää prosessi etenee vaiheittain seuraavasti:

![Prosessin yleiskuvaus uudelleenkoulutusta][7]

## <a name="other-resources"></a>Muut resurssit

- [Azure Data Factory uudelleenkoulutukseen ja päivitetään Azure koneen Learning malleja](https://azure.microsoft.com/blog/retraining-and-updating-azure-machine-learning-models-with-azure-data-factory/)
- [Useimmat tietokoneen Learning mallit ja web päätepisteiden luominen yhden kokeen PowerShellin avulla](machine-learning-create-models-and-endpoints-with-powershell.md)
- [AML uudelleenkoulutusta mallien avulla ohjelmointirajapinnan](https://www.youtube.com/watch?v=wwjglA8xllg) videon avulla voit suuntaamista koneen Learning mallien luotu Azure koneen Learning uudelleenkoulutusta ohjelmointirajapinnan ja PowerShellin avulla.

<!--image links-->
[1]: ./media/machine-learning-retrain-machine-learning-model/machine-learning-retrain-models-programmatically-IMAGE01.png
[7]: ./media/machine-learning-retrain-machine-learning-model/machine-learning-retrain-models-programmatically-IMAGE07.png

