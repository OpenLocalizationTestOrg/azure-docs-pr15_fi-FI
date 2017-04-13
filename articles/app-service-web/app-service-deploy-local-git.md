<properties
    pageTitle="Paikallinen Git käyttöönoton Azure sovelluksen-palveluun"
    description="Opettele käyttöön paikallisessa Git käyttöönoton Azure-sovelluksen palveluun."
    services="app-service"
    documentationCenter=""
    authors="dariagrigoriu"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/13/2016"
    ms.author="dariagrigoriu"/>
    
# <a name="local-git-deployment-to-azure-app-service"></a>Paikallinen Git käyttöönoton Azure sovelluksen-palveluun

Tässä opetusohjelmassa näytetään, miten sovelluksen käyttöön [Azure App palvelun] Git säilöstä paikalliseen tietokoneeseen. Sovelluksen palvelu tukee tätä tapaa **Paikallisen Git** käyttöönottoa ja [Azure-portaalissa].  
Monet tässä artikkelissa kuvattuja Git komennot suoritetaan automaattisesti, kun Käytä [Azure käyttöliittymä] kuvattu [seuraavassa](app-service-web-get-started.md)App Service-sovelluksen luominen.

## <a name="prerequisites"></a>Edellytykset

Jotta voit suorittaa tässä opetusohjelmassa, sinun on:

- Git. Voit ladata asennuksen binaarinen [tähän](http://www.git-scm.com/downloads).  
- Tavallinen tuntemus Git.
- Microsoft Azure-tili. Jos sinulla ei ole tiliä, voit [Rekisteröidy maksuttoman kokeiluversion](https://azure.microsoft.com/pricing/free-trial) tai [aktivoida Visual Studio tilaajan etuja](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details).

>[AZURE.NOTE] Jos haluat aloittaa Azure App palvelun ennen rekisteröimässä Azure-tili, siirry [Yritä App palvelu](http://go.microsoft.com/fwlink/?LinkId=523751), jossa lyhytkestoinen starter-sovelluksen heti voit luoda sovelluksen-palvelussa. Ei ole pakollinen; luottokortit ei ole sitoumukset.  

## <a name="Step1"></a>Vaihe 1: Luo paikalliset säilöön

Suorita seuraavat toimet voit luoda uuden Git säilö.

1. Käynnistä komentorivin työkalu, kuten **GitBash** (Windows) tai **Bash** (Unix-liittymän). OS X-järjestelmiin voit käyttää komentorivillä **Pääte** -sovelluksen kautta.

2. Siirry kansioon, johon sisällön käyttöönotto olisi.

3. Käytä seuraavaa komentoa alustaa uuden Git säilö:

        git init

## <a name="Step2"></a>Vaihe 2: Vahvista sisältöä

Sovelluksen palvelu tukee luotuja erilaisia ohjelmoinnin kieliä. 

1. Jos yhteyttä säilöön sisältää jo sisällön Ohita tämä osoittamalla ja Siirrä osoittavan 2 alla. Jos yhteyttä säilöön ei ole jo sisällön vain lisätä staattisia .html-tiedoston seuraavasti: 

    - Tekstieditorissa, Luo uusi tiedosto nimeltä **index.html** Git säilö ylimmällä tasolla
    - Lisää seuraava teksti index.html sisältö tiedosto ja tallenna se: *Hei Git!*
        
2. Valitse komentorivin Tarkista, että olet Git säilöön pääkansio-kohdassa. Voit lisätä tiedostoja oman-säilöön Käytä seuraava komento:

        git add -A 

4. Vahvista muutokset seuraavaksi säilö käyttämällä seuraava komento:

        git commit -m "Hello Azure App Service"

## <a name="Step3"></a>Vaihe 3: Ottaa käyttöön sovelluksen palvelun sovelluksen säilöön

Seuraavien toimien ottaa käyttöön sovelluksen palvelun sovelluksen Git säilö.

1. Kirjaudu sisään [Azure Portal].

2. Valitse sovelluksen Service-sovelluksen sivu **Asetukset > käyttöönoton lähteen**. **Valitse lähde**-ja sitten Valitse **Paikallinen Git säilöön**ja valitse sitten **OK**.  

    ![Paikallinen Git säilöön](./media/app-service-deploy-local-git/local_git_selection.png)

3. Jos kyseessä on ensimmäisen kerran määritettävän säilön Azure-tietokannassa, tarvitset kirjautumistietosi luomiseen. Niitä käytetään Azure tietovaraston ja push muutokset Kirjaudu oman paikallisen Git säilöstä. Valitse sinua sovelluksen sivu **Asetukset > käyttöönoton tunnistetiedot**, Määritä käyttöönoton käyttäjänimi ja salasana. Kun olet valmis, valitse **Tallenna**.

    ![](./media/app-service-deploy-local-git/deployment_credentials.png)

## <a name="Step4"></a>Vaihe 4: Ota käyttöön projektin

Seuraavien vaiheiden avulla voit julkaista sovelluksen App palvelu paikallisen Git.

1. Napsauta sinua sovelluksen sivu Azure-portaalissa, **Asetukset > ominaisuudet** **Git URL-osoite**.

    ![](./media/app-service-deploy-local-git/git_url.png)

    **Git URL-osoite** on remote viittaus ottaa käyttöön paikallisessa säilöön. Käytät tätä URL-Osoitteen seuraavasti.

2. Käytä komentorivillä ja varmista, että käytössä on paikallinen Git-tietovarasto ylimmällä.

3. Käytä `git remote` Lisää remote viittaus luetellut **Git URL-Osoitteen** vaiheen 1. Komento näyttää seuraavankaltaiselta:

        git remote add azure https://<username>@localgitdeployment.scm.azurewebsites.net:443/localgitdeployment.git         
    > [AZURE.NOTE] **Remote** -komento lisää nimetty viittauksen remote säilöön. Tässä esimerkissä se luo viittaus nimetyt 'azure-web-sovelluksen säilöön.

4. Siirtää sovelluksen palvelu uusi **azure** remote juuri luomasi sisältö.

        git push azure master

    Ohjelma kysyy salasanaa aiemmin luomasi kun Azure-portaalissa käyttöönoton tunnistetietojen nollaaminen. Kirjoita salasana (Huomaa, että Gitbash ei päivitä tähdet konsoliin samalla kun kirjoitat salasanan). 
       
5. Siirry Azure-portaalissa sovelluksen. Lokitapahtuman, viimeisimmän push pitäisi näkyä **käyttöönottoa** sivu. 

    ![](./media/app-service-deploy-local-git/deployment_history.png)

6. Valitse **Selaa yläreunaan sovelluksen sivu vahvistamiseksi sisältö on otettu käyttöön** . 
    
## <a name="Step5"></a>Vianmääritys

Seuraavassa on virheitä tai ongelmia yleisesti käytettäessä Git julkaiseminen Azure App Service-sovelluksen:

****

**Ongelma**: ei voi käyttää [siteURL]: [scmAddress] yhteys epäonnistui

**Syy**: Tämä virhe voi ilmetä, jos sovellus ei ole hyvin alkuun.

**Ratkaisu**: Käynnistä sovellus Azure-portaalissa. Git käyttöönoton ei toimi, jos sovellus on käynnissä. 


****

**Ongelma**: ei voitu ratkaista host isännän "nimi"

**Syy**: Tämä virhe voi ilmetä, jos annettu luomiseen 'azure' remote osoitetiedot on virheellinen.

**Ratkaisu**: Käytä `git remote -v` komento Luetteloi kaikki kaukosäätimet sekä liittyvä URL-osoite. Varmista, että 'azure' remote URL-osoite on oikein. Tarvittaessa, poistaa ja luo tämän etäyhteyksien blogisivun URL-Osoitteen avulla.

****

**Ongelma**: ei refs yhteisen ja ei määritetty; Näin ei mitään. Määritä ehkä haara, kuten "perustyyli".

**Syy**: Tämä virhe voi ilmetä, jos et määritä haaran suorittamiseen git push-toiminto ja käyttää Git push.default arvo ei ole määritetty.

**Ratkaisu**: push toiminnon tekeminen uudelleen määrittäminen perusmuodon haaran. Esimerkki:

    git push azure master

****

**Ongelma**: src refspec [branchname] ei vastaa mitään.

**Syy**: Tämä virhe voi ilmetä, jos yrität push haaraa kuin perustyyli-'azure' remote.

**Ratkaisu**: push toiminnon tekeminen uudelleen määrittäminen perusmuodon haaran. Esimerkki:

    git push azure master

****

**Ongelma**: Virhe - vahvistetun remote säilöön muutokset, mutta niitä ei päivitetä koodiin.

**Syy**: Tämä virhe voi ilmetä, jos otat package.json-tiedoston, joka määrittää lisää tarvittavat moduulit sisältävässä Node.js-sovellus.

**Ratkaisu**: Lisää viestit, jotka sisältävät "npm virhe!" on oltava kirjautuneena edellisen tämän virheen ja antaa lisäkontekstia virheen. Seuraavassa ovat tiedossa syitä tämän virheen ja vastaavan 'npm virhe!" sanoma:

* **Virheellinen package.json tiedosto**: Virhe npm! Riippuvuudet ei voitu lukea.

* **Alkuperäisen moduuli, jolla ei ole binaarinen jakauman Windows**:

    * npm virhe! \`cmd "/ c" "solmu gyp muodosta"\` epäonnistui 1

        TAI

    * npm virhe! [modulename@version]esiasentaa: \`tehdä || gmake\`


## <a name="additional-resources"></a>Lisäresursseja

* [Git dokumentaatio](http://git-scm.com/documentation)
* [Kudu asiakirjoja](https://github.com/projectkudu/kudu/wiki)
* [Jatkuva käyttöönoton Azure sovelluksen-palveluun](app-service-continuous-deployment.md)
* [PowerShellin käyttäminen Azure varten](../powershell-install-configure.md)
* [Opi käyttämään Azure käyttöliittymä](../xplat-cli-install.md)

[Azure sovelluksen-palvelu]: https://azure.microsoft.com/documentation/articles/app-service-changes-existing-services/
[Azure Developer Center]: http://www.windowsazure.com/en-us/develop/overview/
[Azure Portal]: https://portal.azure.com
[Git website]: http://git-scm.com
[Installing Git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[Azure käyttöliittymä]: https://azure.microsoft.com/en-us/documentation/articles/xplat-cli-azure-resource-manager/

[Using Git with CodePlex]: http://codeplex.codeplex.com/wikipage?title=Using%20Git%20with%20CodePlex&referringTitle=Source%20control%20clients&ProjectName=codeplex
[Quick Start - Mercurial]: http://mercurial.selenic.com/wiki/QuickStart
