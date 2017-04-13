<properties 
    pageTitle="Ottaa käyttöön Azure ensimmäisen Java koodiin viisi minuuttia | Microsoft Azure" 
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
    
# <a name="deploy-your-first-java-web-app-to-azure-in-five-minutes"></a>Ottaa käyttöön Azure ensimmäisen Java koodiin viisi minuuttia

Tässä opetusohjelmassa auttaa yksinkertainen Java web-sovelluksen käyttöönotto [Azure](../app-service/app-service-value-prop-what-is.md)-sovelluksen palveluun.
Sovelluksen palvelun voit luoda web Apps-sovelluksista, [mobiilisovelluksen takaisin päättyy](/documentation/learning-paths/appservice-mobileapps/)ja [API sovellusten](../app-service-api/app-service-api-apps-why-best-platform.md).

Menettelet seuraavasti: 

- Luo Azure App palvelun verkkosovellukseen.
- Esimerkki Java sovelluksen käyttöönotto.
- Katso käynnissä live tuotannon koodi.

## <a name="prerequisites"></a>Edellytykset

- Pyydä FTP/FTPS-asiakas, kuten [FileZilla](https://filezilla-project.org/).
- Hanki Microsoft Azure-tili. Jos sinulla ei ole tiliä, voit [Rekisteröidy maksuttoman kokeiluversion](/pricing/free-trial/?WT.mc_id=A261C142F) tai [aktivoida Visual Studio tilaajan etuja](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

>[AZURE.NOTE] Voit [Kokeilla sovelluksen palvelun](http://go.microsoft.com/fwlink/?LinkId=523751) ilman Azure-tili. Starter-sovelluksen luominen ja toistaminen saat tunnin--ylöspäin, ei ole luottokortti, ei ole sitoumukset.

<a name="create"></a>
## <a name="create-a-web-app"></a>Web-sovelluksen luominen

1. Kirjautuminen [Azure portal](https://portal.azure.com) Azure-tilisi kanssa.

2. Valitse vasemmanpuoleisessa valikossa **Uusi** > **Web + Mobile** > **Web Appissa**.

    ![](./media/app-service-web-get-started-languages/create-web-app-portal.png)

3. -Sovelluksen luomisen sivu Käytä seuraavia asetuksia, kun uusi sovellus:

    - **Sovelluksen nimi**: Kirjoita yksilöllinen nimi.
    - **Resurssiryhmä**: Valitse **Luo uusi** ja kirjoita resurssiryhmän nimi.
    - **Sovelluksen palvelun suunnitelman/sijainti**: Voit määrittää napsauttamalla sitä ja valitse sitten **Luo uusi** nimi, sijainti ja hinnoittelu App palvelusopimus taso. Vapaasti käyttää **vapaa** hinnoittelua taso.

    Kun olet valmis, sovelluksen luonti-sivu pitäisi näyttää tältä:

    ![](./media/app-service-web-get-started-languages/create-web-app-settings.png)

3. Valitse **Luo** alaosassa. Voit valita **ilmoituskuvaketta edistymisen Nähdäksesi yläosassa** .

    ![](./media/app-service-web-get-started-languages/create-web-app-started.png)

4. Käyttöönoton päätyttyä, näkyy ilmoitussanoma. Napsauta viestiä, voit avata koko käyttöönotto-sivu.

    ![](./media/app-service-web-get-started-languages/create-web-app-finished.png)

5. Valitse **resurssi** -linkki avaa oman uuden online-sivu **käyttöönoton** -sivu.

    ![](./media/app-service-web-get-started-languages/create-web-app-resource.png)

## <a name="deploy-a-java-app-to-your-web-app"></a>Java-sovelluksen käyttöönotto web App-sovellukseen

Nyt Java-sovelluksen käyttöön japanin Azure FTPS avulla.

5. Web app-sivu-Vieritä **Sovellusasetukset** tai etsi se ja valitse sitten se. 

    ![](./media/app-service-web-get-started-languages/set-java-application-settings.png)

6. **Java-versio**Valitse **Java 8** ja valitse **Tallenna**.

    ![](./media/app-service-web-get-started-languages/set-java-application-settings.png)

    Kun saat ilmoituksen **päivitetty web app-asetukset**, siirry http://*&lt;appname >*. azurewebsites.net Nähdäksesi oletusarvon JSP servlet käytössä.

7. Edellinen web app-sivu- **käyttöönoton tunnistetiedot** tai etsiä sen Siirry ja valitse sitten se.

8. Määritä käyttöönoton tunnistetiedot ja valitse **Tallenna**.

7. Valitse **yleiskuvaus**takaisin sisään web app-sivu. Vierestä **FTP/käyttöönoton käyttäjänimi** ja **FTPS isäntänimi**, **Kopioi** -painike, kopioi seuraavat arvot.

    ![](./media/app-service-web-get-started-languages/get-ftp-url.png)

    Voit nyt Java-sovelluksen kanssa FTPS käyttöönotto.

8. Azure-web-sovelluksen FTP-palvelimen viimeisessä vaiheessa kopioimasi arvot kirjautuminen FTP/FTPS-asiakasohjelmassa. Käytä käyttöönoton salasanaa, jolla aiemmin luomasi.

    Seuraavassa näyttökuvassa näkyy kirjautumisesta FileZilla avulla.

    ![](./media/app-service-web-get-started-languages/filezilla-login.png)

    Näkyviin voi tulla Tuntematon SSL-varmenteen azuren suojausvaroitukset. Siirry eteenpäin ja jatka.

9. Napsauttamalla [tätä linkkiä](https://github.com/Azure-Samples/app-service-web-java-get-started/raw/master/webapps/ROOT.war) SOTA-tiedoston lataaminen paikallisessa tietokoneessa.

9. Siirry **/site/wwwroot/webapps** remote-sivuston FTP/FTPS asiakkaalle ja vedä SODAN ladattu tiedosto paikallisessa tietokoneessa remote kyseiseen kansioon.

    ![](./media/app-service-web-get-started-languages/transfer-war-file.png)

    Valitse **OK** , jos haluat ohittaa tiedoston Azure-tietokannassa.

    >[AZURE.NOTE] Tomcat's oletustoiminnan mukaisesti /site/wwwroot/webapps tiedostonimeä **ROOT.war** tutustutaan pääkansion web app (http://*&lt;appname >*. azurewebsites.net), ja tiedostonimi ** * &lt;anyname >*.war** tutustutaan nimetty verkkosovellukseen (http://*&lt;appname >*.azurewebsites.net/*&lt;anyname >*).

Joka on tämä! Java-sovellus on nyt käynnissä live Azure-tietokannassa. Siirry selaimella http://*&lt;appname >*. azurewebsites.net, katso, miten sitä käytetään. 

## <a name="make-updates-to-your-app"></a>Tee sovelluksen päivitykset

Aina, kun haluat tehdä päivityksen, lataa uusi SODAN tiedosto vain sama remote hakemiston FTP/FTPS asiakkaan kanssa.

## <a name="next-steps"></a>Seuraavat vaiheet

[Luo Java verkkosovellukseen mallista Azure Marketplacesta](web-sites-java-get-started.md#marketplace). Voit saada oman täysin mukautettavissa Tomcat säilö ja saada tuttuja hallinta-Käyttöliittymä. 

Virheenkorjaus Azure web-sovelluksen suoraan [IntelliJ](app-service-web-debug-java-web-app-in-intellij.md) tai [Pimennys](app-service-web-debug-java-web-app-in-eclipse.md).

Voit myös Älä lisää ensimmäisen web-sovelluksen kanssa. Esimerkki:

- Kokeile [muita tapoja käyttöönotto Azure koodi](../app-service-web/web-sites-deploy.md). 
- Ota Azure sovelluksen seuraavalle tasolle. Käyttäjien todentamiseen. Mittakaava tarpeeseen perustuvan. Määritä joitakin suorituskyky-ilmoitukset. Liikkeellä muutamalla napsautuksella. Katso [Lisää toimintoja ensimmäisen web App-sovellukseen](app-service-web-get-started-2.md).

