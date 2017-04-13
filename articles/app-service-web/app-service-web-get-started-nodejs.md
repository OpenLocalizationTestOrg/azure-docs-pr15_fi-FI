<properties 
    pageTitle="Ottaa käyttöön Azure Node.js koodiin viisi minuuttia | Microsoft Azure" 
    description="Katso, miten helppoa on otoksen-sovelluksen ottaminen käyttöön App palvelun verkkosovelluksissa suoritetaan. Aloita tekemällä reaali kehittäminen nopeasti ja hakutuloksia heti." 
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""
/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="10/13/2016" 
    ms.author="cephalin"
/>
    
# <a name="deploy-your-first-nodejs-web-app-to-azure-in-five-minutes"></a>Ottaa käyttöön Azure ensimmäisen Node.js koodiin viisi minuuttia

Tässä opetusohjelmassa auttaa käyttöön [Azure App palvelun](../app-service/app-service-value-prop-what-is.md)ensimmäisen Node.js koodiin.
Sovelluksen palvelun voit luoda web Apps-sovelluksista, [mobiilisovelluksen takaisin päättyy](/documentation/learning-paths/appservice-mobileapps/)ja [API sovellusten](../app-service-api/app-service-api-apps-why-best-platform.md).

Menettelet seuraavasti: 

- Luo Azure App palvelun verkkosovellukseen.
- Ottaa käyttöön otoksen Node.js koodia.
- Katso käynnissä live tuotannon koodi.
- Päivitä koodiin [Git vahvistaa push](https://git-scm.com/docs/git-push)samaan tapaan.

## <a name="prerequisites"></a>Edellytykset

- [Git](http://www.git-scm.com/downloads).
- [Azure CLI](../xplat-cli-install.md).
- Microsoft Azure-tili. Jos sinulla ei ole tiliä, voit [Rekisteröidy maksuttoman kokeiluversion](/pricing/free-trial/?WT.mc_id=A261C142F) tai [aktivoida Visual Studio tilaajan etuja](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

>[AZURE.NOTE] Voit [Kokeilla sovelluksen palvelun](http://go.microsoft.com/fwlink/?LinkId=523751) ilman Azure-tili. Starter-sovelluksen luominen ja toistaminen saat tunnin--ylöspäin, ei ole luottokortti, ei ole sitoumukset.

## <a name="deploy-a-nodejs-web-app"></a>Node.js web-sovelluksen käyttöönotto

1. Avaa uusi Windows-komentokehote, PowerShell-ikkuna, Linux-liittymän tai OS X-päätteen. Suorita `git --version` ja `azure --version` voit varmistaa, että Git ja Azure CLI on asennettu tietokoneeseesi.

    ![Azure-tietokannassa CLI työkaluja, joiden ensimmäinen online-asennuksen testaaminen](./media/app-service-web-get-started/1-test-tools.png)

    Jos et ole vielä asentanut työkaluja, katso [edellytykset](#Prerequisites) Lataa.

3. Kirjaudu sisään Azure tältä:

        azure login

    Noudata voi jatkaa Kirjaudu sisään.

    ![Kirjaudu sisään ja Azure ensimmäisen web-sovelluksen luominen](./media/app-service-web-get-started/3-azure-login.png)

4. Muuta Azure CLI ASM tilaan ja valitse sitten Määritä käyttöönoton sovelluksen-palveluun. Voit ottaa käyttöön koodin tunnistetiedoilla myöhemmin.

        azure config mode asm
        azure site deployment user set --username <username> --pass <password>

1. Työn hakemistoon (`CD`) ja Kloonaa otoksen sovelluksen tältä:

        git clone https://github.com/Azure-Samples/app-service-web-nodejs-get-started.git

2. Muuta otoksen sovelluksen säilöön.

        cd app-service-web-nodejs-get-started

4. Luo sovelluksen palvelun sovelluksen resurssin Azure yksilöllisen sovelluksen nimi ja aiemmin määritetty käyttöönotto-käyttäjän kanssa. Kun sinua pyydetään, määrittää minuuttimäärän, jonka haluamasi alue.

        azure site create <app_name> --git --gitusername <username>

    ![Ensimmäisen online Azure resurssin luominen Azure](./media/app-service-web-get-started-languages/node-site-create.png)

    Sovellus on luotu Azure nyt. Nykyisen hakemiston on myös Git alustaa ja liitetty uuden sovelluksen Service-sovelluksen Git, joka on remote nimellä.
    Voit selata sovelluksen URL-osoite (http://&lt;app_name >. azurewebsites.net) Katso laadukkaita oletusarvoisesti HTML-sivulle, mutta itse asiassa aloitetaan koodi on nyt.

4. Ottaa käyttöön otoksen koodia Azure-sovellukseen, kuten muistiinpanon koodia Git kanssa. Kun sinulta kysytään, käytä salasanaa, aiemmin määritetty.

        git push azure master

    ![Push-koodi ensimmäisen koodiin Azure-tietokannassa](./media/app-service-web-get-started-languages/node-git-push.png)

    `git push`paitsi sijoittaa koodin Azure, mutta tuottaa myös käyttöönoton tehtävät-käyttöönotto-ohjelma. 
    Jos sinulla package.json projektin (säilöön) pääsivustoon, käyttöönoton komentosarja palauttaa tarvittavat paketit puolestasi. 

Onnittelut, olet asentanut sovelluksen Azure-sovelluksen palveluun.

## <a name="see-your-app-running-live"></a>Katso käynnissä live sovelluksen

Saat käynnissä live Azure-sovelluksen, suorita tämä komento kansiota, että säilössä:

    azure site browse

## <a name="make-updates-to-your-app"></a>Tee sovelluksen päivitykset

Voit nyt push-projekti (säilöön) pääkansion milloin tahansa halutaan päivitys sivustoihin Git. Voit tehdä sen samalla tavalla kuin kun ensimmäisen kerran käyttöön koodisi. Esimerkiksi aina, kun haluat push muuttunut, kun olet testannut paikallisesti, vain suorittaa seuraavat komennot projektin (säilöön) pääkansio:

    git add .
    git commit -m "<your_message>"
    git push azure master

## <a name="next-steps"></a>Seuraavat vaiheet

[Luo-määritys ja Node.js Express web-sovelluksen, Azure käyttöönotto](app-service-web-nodejs-get-started.md). Noudattamalla tässä opetusohjelmassa opit perustiedot osaamisalueet, sinun on suoritettava minkä tahansa Node.js web-sovelluksen Azure-tietokannassa, kuten:

- Luo ja määritä sovellusten Azure-PowerShell/Bash.
- Määritä Node.js versio.
- Käytä Käynnistä-tiedosto, joka ei ole sovelluksen pääkansioon.
- Automatisoida NPM kanssa.
- Saat virheen ja tulosteen lokit.

Voit myös Älä lisää ensimmäisen web-sovelluksen kanssa. Esimerkki:

- Kokeile [muita tapoja käyttöönotto Azure koodi](../app-service-web/web-sites-deploy.md). Esimerkiksi ottamaan yhteyttä GitHub säilöjen tietoihin olevista yksinkertaisesti Valitse sen sijaan, että **Paikallinen Git säilöön** **GitHub** **asennusvaihtoehdot**.
- Ota Azure sovelluksen seuraavalle tasolle. Käyttäjien todentamiseen. Mittakaava tarpeeseen perustuvan. Määritä joitakin suorituskyky-ilmoitukset. Liikkeellä muutamalla napsautuksella. Katso [Lisää toimintoja ensimmäisen web App-sovellukseen](app-service-web-get-started-2.md).
