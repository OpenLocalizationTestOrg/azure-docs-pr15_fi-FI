<properties
    pageTitle="Joustava software development Azure-sovelluksen ominaisuudet"
    description="Opi luomaan monimutkaisia suuri asteikko-sovellusten Azure App palvelussa niin, että tukee joustava software development."
    services="app-service"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/01/2016"
    ms.author="cephalin"/>


# <a name="agile-software-development-with-azure-app-service"></a>Joustava software development Azure-sovelluksen ominaisuudet #

Tässä opetusohjelmassa kerrotaan suuri asteikko monimutkaisia sovellusten luominen [Azure App](/services/app-service/) palvelussa niin, että tukee [Joustava software development](https://en.wikipedia.org/wiki/Agile_software_development). Oletetaan, että tiedät [monimutkaisia sovellusten laadukkaampi Azure](app-service-deploy-complex-application-predictably.md)ottamisesta käyttöön.

Tekniset prosessit rajoitukset usein oma ehkäistä joustava menetelmiä onnistunut käyttöönotto. Ominaisuudet, kuten [Jatkuva julkaisemisen](app-service-continuous-deployment.md), [väliaikaisen ympäristöissä](web-sites-staged-publishing.md) (paikkojen) ja [Seuranta](web-sites-monitor.md), kun sekä viisaasti tiedonsiirron ja [Azure Resurssienhallinta](../azure-resource-manager/resource-group-overview.md)ja käyttöönoton hallinta Azure App palvelun voi olla osa sovelluskehittäjät, jotka sisältävät huoltorivien joustava software development erinomainen ratkaisu.

Seuraavassa taulukossa on lyhyt luettelo joustava kehittämisen ja miten Azure-palveluiden avulla ne liittyvät vaatimukset.

| Vaatimus | Miten Azure avulla |
|---------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| -Muodostaa jokaisen vahvistaminen<br>– Luo automaattisesti ja nopea | Kun määritetty jatkuva käyttöönoton, Azure App palvelun voi toimia live suorittaminen versiot keskihajonta-haara perusteella. Aina, kun koodi on miten sivuliike, on automaattisesti valmiita ja käyttöön live Azure-tietokannassa.|
| -Merkki muodostaa itse testaaminen | Lataa kokeet, web testit jne., voidaan ottaa käyttöön Azure Resurssienhallinta-malli.|
| -Tee testit Kloonaa tuotantoympäristössä. | Azure Resurssienhallinta malleja voidaan luoda Azure tuotantoympäristössä (mukaan lukien sovelluksen asetusten yhteyden merkkijonon mallit, skaalaus, jne.) testikäyttöön nopeasti ja laadukkaampi kopioita.|
| -Näyttää tuloksen uusimmassa versiossa helposti | Jatkuva käyttöönoton Azure säilöstä tarkoittaa, että voit testata uutta koodia live sovelluksessa heti, kun tallentaa muutokset. |
| -Vahvista tärkeimmät haaran joka päivä<br>-Automatisoida käyttöönotto | Jatkuva integrointi tuotannon sovelluksen voi tallentaa tärkeimpien haaran automaattisesti ottaa käyttöön jokaisen Vahvista/yhdistämisen tärkeimmät haaran tuotannon. |

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="what-you-will-do"></a>Mitä hyötyä ##

Käy läpi tyypillinen keskihajonta-testi-vaiheen-tuotannon työnkulun jotta uusi muutosten julkaiseminen sovelluksen [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) malli, joka sisältää kaksi [web apps](/services/app-service/web/)-toisaalta frontend (FE) ja toinen verkko-Ohjelmointirajapinnan taustassa (BE) ja [SQL-tietokantaan](/services/sql-database/). Toimivat käyttöönotto-arkkitehtuuri alla:

![](./media/app-service-agile-software-development/what-1-architecture.png)

Voit lisätä kuvan sanoiksi:

-   Käyttöönotto-arkkitehtuuri jakautuu kolme eri ympäristöissä eli Azure- [ryhmissä](../azure-resource-manager/resource-group-overview.md) , joissa oma [sovelluksen palvelusopimus](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md), [Skaalaus](web-sites-scale.md) -asetukset ja SQL-tietokantaan. 
-   Kussakin ympäristössä voidaan hallita erikseen. Ne voi olla jopa eri tilaukset.
-   Väliaikaisen ja tuotannon on toteutettu kaksi paikkojen saman sovelluksen palvelun sovelluksen. Perusmuodon haara on jatkuva integrointi väliaikaisen paikka asetukset.
-   Kun Vahvista perusmuodon haaraa on vahvistetaan (tuotannon tiedoilla) väliaikaisen paikka, vaihtaa paikkaa varmennettu väliaikaisen sovellus tuotannon paikka [ei ole käyttökatkot kanssa](web-sites-staged-publishing.md)-kyselyjä.

Tuotannon ja väliaikaisen-ympäristö on määritetty mallin [ * &lt;repository_root >*/ARMTemplates/ProdandStage.json](https://github.com/azure-appservice-samples/ToDoApp/blob/master/ARMTemplates/ProdAndStage.json).

Keskihajonta-Jos-ympäristöissä on määritetty mallin [ * &lt;repository_root >*/ARMTemplates/Dev.json](https://github.com/azure-appservice-samples/ToDoApp/blob/master/ARMTemplates/Dev.json).

Tyypillinen haarautumislogiikan strategia myös käytetään koodilla siirtäminen ylöspäin testi-haara keskihajonta päätasolta sitten perustyylin haara (siirtäminen laatu, joten speak).

![](./media/app-service-agile-software-development/what-2-branches.png) 

## <a name="what-you-will-need"></a>Tarvittavat komponentit ##

-   Azure-tili
-   [GitHub](https://github.com/) -tili
-   Git Shell (asennettu [GitHub for Windowsin](https://windows.github.com/)kanssa) – tämän avulla voit suorittaa saman istunnon Git ja PowerShell-komennot 
-   Uusimman [PowerShellin Azure](https://github.com/Azure/azure-powershell/releases/download/0.9.4-June2015/azure-powershell.0.9.4.msi) bittien
-   Tavallinen tietoja seuraavasti:
    -   [Azure Resurssienhallinta](../azure-resource-manager/resource-group-overview.md) mallin käyttöönottoa (myös Katso [Ota KOMPLEKSI sovelluksen laadukkaampi Azure](app-service-deploy-complex-application-predictably.md))
    -   [Git](http://git-scm.com/documentation)
    -   [PowerShellin](https://technet.microsoft.com/library/bb978526.aspx)

> [AZURE.NOTE] Sinun on suoritettava tässä opetusohjelmassa Azure tilin:
> + Voit [avata Azure-tili maksutta](/pricing/free-trial/) – hyvitykset Hae avulla voit kokeilla maksettu Azure services ja senkin jälkeen, kun niitä käytetään voit pitää tilin ja käyttää vapaa Azure-palvelut, kuten Web Apps.
> + Voit [aktivoida Visual Studio tilaajan edut](/pricing/member-offers/msdn-benefits-details/) - Your Visual Studio tilauksen käyttöösi hyvitykset joka kuukausi, jonka maksettu Azure-palveluiden avulla.
>
> Jos haluat aloittaa Azure App palvelun ennen rekisteröimässä Azure-tili, siirry [Yritä App palvelu](http://go.microsoft.com/fwlink/?LinkId=523751), jossa lyhytkestoinen starter verkkosovellukseen heti voit luoda sovelluksen-palvelussa. Ei ole pakollinen; luottokortit ei ole sitoumukset.

## <a name="set-up-your-production-environment"></a>Tuotannon ympäristön määrittäminen ##

>[AZURE.NOTE] Tässä opetusohjelmassa käytetty komentosarja määritetään automaattisesti jatkuva julkaiseminen GitHub-säilöstä. Tämä edellyttää, että GitHub tunnistetiedot on jo tallennettu Azure, muuten komentosarjalla toteutettavan käyttöönoton epäonnistuu yritettäessä web Apps-sovellusten tietolähteen ohjausobjektin asetusten määrittäminen. 
>
>Azure GitHub tunnistetietojen tallentamiseen Luo verkkosovellukseen [Azure-portaalin](https://portal.azure.com/) ja [määrittää GitHub-yhdistelmäympäristössä](app-service-continuous-deployment.md). Tarvitset vain vain yhden kerran. 

Tyypillinen DevOps skenaariossa sovellus, joka toimii live Azure-tietokannassa on ja haluat tehdä siihen muutoksia jatkuva julkaiseminen kautta. Tässä skenaariossa on malli, joka on kehitetty, Testaa ja käyttöönotossa tuotantoympäristössä käytetty. Haluat määrittää sen sisältö.

1.  Voit luoda oman rinnakkainen [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) tietovaraston. Lisätietoja oman rinnakkainen luomisesta on artikkelissa [Rinnakkainen Repo](https://help.github.com/articles/fork-a-repo/). Oman rinnakkainen luodaan, kun näet sen selaimessa.
 
    ![](./media/app-service-agile-software-development/production-1-private-repo.png)

2.  Avaa Git Shell-istunto. Jos sinulla ei ole vielä Git Shell, asenna [GitHub Windows](https://windows.github.com/) .

3.  Luo paikalliseen Kloonaa, että rinnakkainen suorittamalla seuraavan komennon:

        git clone https://github.com/<your_fork>/ToDoApp.git 

4.  Kun paikallinen Kloonaa, siirry * &lt;repository_root >*\ARMTemplates ja suorita deploy.ps1 komentosarjan seuraavasti:

        .\deploy.ps1 –RepoUrl https://github.com/<your_fork>/todoapp.git

4.  Kun sinulta kysytään, kirjoita haluamasi käyttäjänimi ja salasana tietokannan käytön.

    Raportissa pitäisi näkyä eri Azure resurssien valmistelu etenemisen. Kun käyttöönotto on valmis, komentosarja Käynnistä sovellus selaimessa ja antaa sinulle helpossa muodossa äänimerkki.

    ![](./media/app-service-agile-software-development/production-2-app-in-browser.png)
 
    >[AZURE.TIP] Tutustu * &lt;repository_root >*\ARMTemplates\Deploy.ps1, jos haluat nähdä, miten se luo resursseja on yksilölliset tunnukset. Samoin voit luoda saman käyttöönoton kopioita ilman katkeamisesta ristiriitaiset resurssien nimet.
 
6.  Takaisin Git Shell-istunnossa suorittaa:

        .\swap –Name ToDoApp<unique_string>master

    ![](./media/app-service-agile-software-development/production-4-swap.png)

7.  Kun komentosarja on valmis, palaa Selaa edusta-osoite (http://ToDoApp*&lt;unique_string >*master.azurewebsites.net/) nähdäksesi tuotannon sovelluksen.
 
5.  Kirjaudu sisään [Azure-portaaliin](https://portal.azure.com/) ja tutustu mitä luodaan.

    Sinun pitäisi nähdä saman resurssiryhmä, jossa kaksi verkkosovelluksissa `Api` nimi-liitettä. Jos tarkastelet ryhmän resurssinäkymään, myös näet SQL-tietokantaan ja palvelimen ja sovelluksen palvelusopimus väliaikaisen paikkojen web Apps-sovellukset. Selata eri resursseja ja vertaa niitä * &lt;repository_root >*\ARMTemplates\ProdAndStage.json näet, miten ne määritetään mallissa.

    ![](./media/app-service-agile-software-development/production-3-resource-group-view.png)

Voit nyt määrittää tuotantoympäristössä. Seuraavaksi aloituskokouksen uusi päivitys-sovellukseen.

## <a name="create-dev-and-test-branches"></a>Luo keskihajonta ja testaa haaroja ##

Nyt kun olet luonut monimutkaisen sovelluksen Azure tuotannon käynnissä, tekee päivitys sovelluksen joustava kehitysmenetelmistä mukaisesti. Tässä osassa luodaan keskihajonta ja testaa haaroja, sinun on tehtävä tarvittavat muutokset.

1.  Luo ensin testiympäristössä. Git Shell-istunnossa suorittamalla seuraavat komennot kutsutaan **NewUpdate**uusi haaran ympäristön luomiseen. 

        git checkout -b NewUpdate
        git push origin NewUpdate 
        .\deploy.ps1 -TemplateFile .\Dev.json -RepoUrl https://github.com/<your_fork>/ToDoApp.git -Branch NewUpdate

1.  Kun sinulta kysytään, kirjoita haluamasi käyttäjänimi ja salasana tietokannan käytön. 

    Kun käyttöönotto on valmis, komentosarja Käynnistä sovellus selaimessa ja antaa sinulle helpossa muodossa äänimerkki. Ja aivan kuin uuden haara, jossa on oma testiympäristössä on nyt. Tarkista tässä testiympäristössä tietoja muutama seikka hetki:

    -   Voit luoda sen jokin Azure tilaus. Tämä tarkoittaa tuotantoympäristössä voidaan hallita erikseen testiympäristössä.
    -   Testiympäristössä on käynnissä live Azure-tietokannassa.
    -   Testiympäristössä on sama kuin tuotantoympäristössä lukuun ottamatta väliaikaisen paikkojen ja skaalauksen asetukset. Näin voit tietää, koska nämä ovat ProdandStage.json ja Dev.json vain erot.
    -   Voit hallita testiympäristössä App palvelun omassa suunnitelman kanssa eri hinta taso (kuten **vapaa**).
    -   Tämä testiympäristössä poistaminen olla pelkästään resurssiryhmän poistaminen. Huomaat, miten voit tehdä tämän [myöhemmin](#delete).

2.  Siirry Luo keskihajonta-haara suorittamalla seuraavat komennot:

        git checkout -b Dev
        git push origin Dev
        .\deploy.ps1 -TemplateFile .\Dev.json -RepoUrl https://github.com/<your_fork>/ToDoApp.git -Branch Dev

3.  Kun sinulta kysytään, kirjoita haluamasi käyttäjänimi ja salasana tietokannan käytön. 

    Tutustu tarkistettava muutama seikka tietoja keskihajonta-ympäristöön: 

    -   Keskihajonta-ympäristösi on määritys, joka on sama kuin testiympäristössä, koska se on otettu käyttöön sama mallin avulla.
    -   Kehittäjän oman Azure tilauksen, jätä erikseen tuleeko testiympäristössä voidaan luoda kussakin keskihajonta-ympäristössä.
    -   Keskihajonta-ympäristösi on käynnissä live Azure-tietokannassa.
    -   Keskihajonta-ympäristön poistaminen onnistuu helposti vain resurssiryhmän poistaminen. Huomaat, miten voit tehdä tämän [myöhemmin](#delete).

>[AZURE.NOTE] Jos sinulla on useita kehittäjät uusi päivitys parissa, jokainen niistä voivat luoda helposti haaran ja erillinen keskihajonta-ympäristön seuraavasti:
>
>1. Luo omia rinnakkainen tietovaraston GitHub (katso [Rinnakkainen Repo](https://help.github.com/articles/fork-a-repo/)).
>2. Kloonaa rinnakkainen niiden paikallisessa tietokoneessa
>3. Suorita sama komennot omia keskihajonta haaran ja ympäristön luomiseen.

Kun olet valmis, GitHub rinnakkainen pitäisi olla kolme haaroja:

![](./media/app-service-agile-software-development/test-1-github-view.png)

Ja on oltava kuusi verkkosovelluksissa (kaksi kolme sääntöjoukot) kolmeen eri resurssin ryhmään:

![](./media/app-service-agile-software-development/test-2-all-webapps.png)
 
>[AZURE.NOTE] Huomaa, että ProdandStage.json määrittää käyttämään **Vakio** hinnoittelua taso, joka sopii skaalattavuus tuotannon sovelluksen tuotantoympäristössä.

## <a name="build-and-test-every-commit"></a>Muodosta ja Testaa jokaisen vahvistaminen ##

Mallitiedostot ProdAndStage.json ja Dev.json Määritä jo Ohjausobjektin lähde-parametrit, joka oletusarvoisesti määrittää jatkuva julkaiseminen web-sovelluksen. Tämän vuoksi jokaisen Vahvista GitHub haaraa käynnistää automaattinen käyttöönotto Azure, kyseisen päätasolta. Katsotaan, miten asetus toimii nyt.

1.  Varmista, että olet paikallisen säilön keskihajonta-haara. Voit tehdä tämän suorittamalla seuraavan komennon Git runko:

        git checkout Dev

2.  Sovelluksen Käyttöliittymän kerrokseen yksinkertaisten muutosten tekeminen muuttamalla koodin käyttämään [Automaattinen](http://getbootstrap.com/components/) luettelot. Avaa * &lt;repository_root >*\src\MultiChannelToDo.Web\index.cshtml tai tekevät alla korostetun muuttaminen:

    ![](./media/app-service-agile-software-development/commit-1-changes.png)

    >[AZURE.NOTE] Jos et pysty lukemaan yllä olevan kuvan: 
    >
    >- Muuta rivin 18 `check-list` , `list-group`.
    >- Muuta rivin 19 `class="check-list-item"` , `class="list-group-item"`.

3.  Tallenna muutokset. Takaisin-käyttöliittymän Git suorittamalla seuraavat komennot:

        cd <repository_root>
        git add .
        git commit -m "changed to bootstrap style"
        git push origin Dev
 
    Nämä git komennot muistuttavat toisen lähteen ohjausobjektin järjestelmän like TFS "tarkistuksen koodissa". Kun suoritat `git push`, uusi Vahvista käynnistää automaattisen koodi-push, Azure, joka muodostaa muuttuvat keskihajonta-ympäristössä sovellus sitten uudelleen.

4.  Voit varmistaa, että tämä koodi push keskihajonta-ympäristöön on tapahtunut, siirry keskihajonta-ympäristösi web app-sivu ja katsomalla **käyttöönotto** -osa. Sinun pitäisi nähdä uusimmat Vahvista viesti.

    ![](./media/app-service-agile-software-development/commit-2-deployed.png)

5.  Sieltä valitsemalla **Selaa** voit näyttää uuden Azure-tietokannassa live-sovelluksessa.

    ![](./media/app-service-agile-software-development/commit-3-webapp-in-browser.png)

    Tämä on melko pieni muutos-sovellukseen. Monta kertaa monimutkaisia verkkosovelluksen uusia muutoksia on kuitenkin tahattomia ja ei-toivotun vaikutukset. Ei voi esikatsella jokaisen Vahvista helposti live versiot mahdollistaa todellisen ongelmaan ennen asiakkaille näe niitä.

Nyt mukaan pitäisi olla perehtynyt siihen, että **NewUpdate** projektin kehittäjän, osaat helposti luoda keskihajonta-ympäristön itsellesi, valitse jokaisen Vahvista kokoaminen ja testaaminen jokaisen muodosta olevilla.

## <a name="merge-code-into-test-environment"></a>Koodin yhdistäminen testiympäristössä ##

Kun olet valmis push-koodisi ylöspäin NewUpdate haaran päätasolta keskihajonta, se on vakio git-prosessi:

1.  Yhdistä että uudet tallentaa NewUpdate keskihajonta haara GitHub, kuten tapahtumasarjan vahvistukset muiden kehittäjät luoma kyselyjä. Kaikki uudet Vahvista-GitHub käynnistäminen koodin push ja luoda keskihajonta-ympäristössä. Voit sitten varmistaa, että koodisi keskihajonta haaran toimii edelleen uusimman bittien NewUpdate päätasolta.

2.  Yhdistää kaikki oman uuden tapahtumasarjan vahvistukset keskihajonta päätasolta NewUpdate haara-GitHub. Tämä toiminto käynnistää koodin push ja testiympäristössä muodosta. 

Huomautus uudelleen, koska jatkuva käyttöönoton on jo git nämä haaroja asennus, sinun ei tarvitse tehdä mitään muita kuin käynnissä integrointi muodostaa. Sinun on suoritettava vakio tietolähteen ohjausobjektin käytännöt käyttämällä git yksinkertaisesti ja Azure suorittaa muodosta prosessit puolestasi.

Nyt push japanin **NewUpdate** haaraa koodisi. Käyttöliittymän Git suorittamalla seuraavat komennot:

    git checkout NewUpdate
    git pull origin NewUpdate
    git merge Dev
    git push origin NewUpdate

Joka on tämä! 

Siirry testiympäristössä näkevän tekemäsi uusi Vahvista (yhdistetään NewUpdate haaran) nyt testiympäristössä, miten web-sovelluksen sivu. Valitse **Selaa** voit tarkistaa, että tyylin muuttaminen nyt toimii live Azure-tietokannassa.

## <a name="deploy-update-to-production"></a>Ottaa päivityksen käyttöön tuotannon ##

Valitseminen koodin väliaikaisen ja tuotannon ympäristön olisi tuntuu sama asia kuin jo työn koodin valittuna testi-ympäristöön. On todella helppoa, että. 

Käyttöliittymän Git suorittamalla seuraavat komennot:

    git checkout master
    git pull origin master
    git merge NewUpdate
    git push origin master

Muista, että mukaan, kuinka testaus- ja -ympäristö on ProdandStage.json asennuksen, uusi koodi **väliaikaisen** paikka, miten ja on käynnissä. Jotta Jos siirryt väliaikaisen paikan URL-osoite, näkyviin tulee uusi koodi on käynnissä. Voit tehdä tämän suorittamalla `Show-AzureWebsite` cmdlet-Git käyttöliittymän.

    Show-AzureWebsite -Name ToDoApp<unique_string>master -Slot Staging
 
Ja nyt, kun olet tarkistanut väliaikaisen paikka päivitys, hahmottaa riittää on vaihdettava kyselyjä tuotannon. Käyttöliittymän Git suorittamalla seuraavat komennot:

    cd <repository_root>\ARMTemplates
    .\swap.ps1 -Name ToDoApp<unique_string>master

Onnittelen! Uusi päivitys on julkaistu tuotannon verkkosovellukseen. Mitä yhdistetyssä Lisää on, että vain tehdä sen luomalla keskihajonta helposti ja testaa ympäristöissä ja luomisesta ja testaamalla jokaisen Vahvista. Nämä ovat tärkeitä joustava software development rakenneosia.

<a name="delete"></a>
## <a name="delete-dev-and-test-enviroments"></a>Poista keskihajonta ja testaa enviroments ##

Koska olet tarkoituksellisesti sivun reunaan arkkitehtuuri keskihajonta-Jos-ympäristöissä on itsenäinen resurssiryhmät, on hyvin helppoa poista ne. Voit poistaa niistä luomasi Tässä opetusohjelmassa GitHub haaroja ja Azure palvelutiedot, Suorita vain seuraavat komennot Git runko:

    git branch -d Dev
    git push origin :Dev
    git branch -d NewUpdate
    git push origin :NewUpdate
    Remove-AzureRmResourceGroup -Name ToDoApp<unique_string>dev-group -Force -Verbose
    Remove-AzureRmResourceGroup -Name ToDoApp<unique_string>newupdate-group -Force -Verbose

## <a name="summary"></a>Yhteenveto ##

Joustava software development on on-ovat monta yrityksille, joilla haluat vahvistaa Azure kuin niiden sovelluksen ympäristö. Tässä opetusohjelmassa on opit luomisesta ja tear tarkka replikoiden alas tai lähellä tuotantoympäristössä replikoita vaivattomasti myös monimutkaisia sovelluksia varten. Opit on myös, miten voit hyödyntää tätä mahdollisuus luoda kehittäminen prosessi, jossa voit luoda ja esikatsella jokaisen yksittäisen Vahvista Azure. Tässä opetusohjelmassa on hopefully osoittanut parhaiten käyttämisestä Azure App palvelu ja Azure Resurssienhallinta yhdessä DevOps-ratkaisun, joka caters joustava menetelmiä. Seuraavaksi voit luoda-artikkelista suorittamalla Lisäasetukset DevOps tekniikoita, kuten [testaaminen tuotannon](app-service-web-test-in-production-get-start.md). Katso [Flighting käyttöönoton (Betatestaus) Azure-sovelluksen palvelun](app-service-web-test-in-production-controlled-test-flight.md)käytetty testaus-kohdassa-tuotannon-vaihtoehto.

## <a name="more-resources"></a>Lisää resursseja ##

-   [Monimutkaisen laadukkaampi Azure-sovelluksen käyttöönotto](app-service-deploy-complex-application-predictably.md)
-   [Joustava kehittäminen käytännössä: vihjeitä ja vinkkejä uudenaikaistettujen kehittäminen kehä](http://channel9.msdn.com/Events/Ignite/2015/BRK3707)
-   [Käyttöönottostrategioita Azure verkkosovelluksissa Resurssienhallinta mallien käyttäminen](http://channel9.msdn.com/Events/Build/2015/2-620)
-   [Authoring Azure Resurssienhallinta-mallit](../resource-group-authoring-templates.md)
-   [JSONLint - JSON tarkistajan](http://jsonlint.com/)
-   [ARMClient – GitHub julkaiseminen sivuston määrittäminen](https://github.com/projectKudu/ARMClient/wiki/Setup-GitHub-publishing-to-Site)
-   [Git haarautumista – Basic haarautuminen ja yhdistäminen](http://www.git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging)
-   [David Ebbo-blogi](http://blog.davidebbo.com/)
-   [Azure PowerShell](../powershell-install-configure.md)
-   [Azure Office kaikissa ympäristöissä komentorivin Työkalut](../xplat-cli-install.md)
-   [Luo tai Muokkaa käyttäjiä Azure AD](https://msdn.microsoft.com/library/azure/hh967632.aspx#BKMK_1)
-   [Projektin Kudu Wiki](https://github.com/projectkudu/kudu/wiki)
