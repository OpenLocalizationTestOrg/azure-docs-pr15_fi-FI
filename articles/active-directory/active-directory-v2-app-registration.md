<properties
    pageTitle="v2.0 sovelluksen rekisteröinnin | Microsoft Azure"
    description="Sovelluksen rekisteröitymisestä Microsoft kirjautumisen ottaminen käyttöön ja Microsoft-palveluita käyttämällä v2.0 päätepisteen käyttäminen"
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>

# <a name="how-to-register-an-app-with-the-v20-endpoint"></a>Sovelluksen rekisteröitymisestä v2.0 päätepiste

Voit luoda sovelluksen, joka hyväksyy MSA ja Azure AD kirjautuminen, ensin tarvitset sovelluksen rekisteröitymään Microsoftille.  Tällä hetkellä, et voi käyttää aiemmin luotuja sovellusten joudut Azure AD tai MSA - sinun on Luo uusi tunnus.

> [AZURE.NOTE]
    V2.0 päätepisteen tukee kaikkia Azure Active Directory-skenaariot ja toiminnot.  Voit selvittää, jos sinun on käytettävä v2.0 päätepisteen, lue lisätietoja [v2.0 rajoitukset](active-directory-v2-limitations.md).

## <a name="visit-the-microsoft-app-registration-portal"></a>Käy Microsoft sovelluksen rekisteröinti
Ensimmäinen asioita, siirry ensin - [https://apps.dev.microsoft.com/?deeplink=/appList](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).  Tämä on uusi sovellus rekisteröinti portaali kohtaa, johon voit hallita Microsoft-sovelluksia.

Kirjaudu sisään joko oman tai työpaikan tai oppilaitoksen Microsoft-tili.  Jos sinulla ei ole joko uuden oman tilin rekisteröiminen. Siirry eteenpäin, ei kestää kauan - olemme odotettava tähän.

Valmis? Pitäisi nyt etsit Microsoft sovellusluettelo, joka on todennäköisesti tyhjä.  Muuttaa japanin.

Lisää **sovellus**ja anna sille nimi.  Portaalin määrittää sovelluksen GUID sovelluksen Id, jolla käytät myöhemmin koodissa.  Jos sovellus on palvelinpuolen osa, joka on puheluja ohjelmointirajapinnan access tunnusten (Ajattele: Office ja Azure omia verkko-Ohjelmointirajapinnan), kannattaa luoda **Sovelluksen toiminta** tässä sekä.
<!-- TODO: Link for app secrets -->

Lisää seuraavaksi ympäristöissä, jotka käyttävät sovelluksen.

- Web-pohjaisten sovellusten antaa **Uudelleenohjata URI** , jossa kirjautumisen viestejä voidaan lähettää.
- Kopioi mobile-sovellukset alaspäin oletusarvoinen uudelleenohjaus uri automaattisesti puolestasi.

Vaihtoehtoisesti voit mukauttaa profiilin kirjautumisen sivun ulkoasun.  Varmista ennen siirtymistä valitsemalla **Tallenna** .

> [AZURE.NOTE] Kun luot [https://apps.dev.microsoft.com/?deeplink=/appList](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)-sovellus, tili, jolla voit kirjautua portaaliin koti vuokraajan rekisteröity sovellus.  Tämä tarkoittaa, että sinun ei rekisteröidä sovelluksen Azure AD-vuokraajan henkilökohtainen Microsoft-tilillä.  Jos haluat erikseen sovellus rekisteröidään tietyn vuokraajan kirjautua sisään tilille alun perin luotu alihallintaan.

## <a name="build-a-quick-start-app"></a>Pika-aloitusopas-sovelluksen luominen
Nyt kun olet luonut Microsoft-sovelluksen, voit täyttää Microsoftin v2.0 pikaopas opetusohjelmat.  Seuraavassa on muutamia suositukset:

[AZURE.INCLUDE [active-directory-v2-quickstart-table](../../includes/active-directory-v2-quickstart-table.md)]
