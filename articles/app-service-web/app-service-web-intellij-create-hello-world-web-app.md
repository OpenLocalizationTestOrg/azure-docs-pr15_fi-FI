<properties 
    pageTitle="Luo Hei maailma verkkosovellukseen Azure IntelliJ | Microsoft Azure" 
    description="Tässä opetusohjelmassa kerrotaan, miten saat IntelliJ Azure-työkalujen avulla voit luoda Hei maailma verkkosovellukseen Azure varten." 
    services="app-service\web" 
    documentationCenter="java" 
    authors="selvasingh" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="asirveda;robmcm"/>

# <a name="create-a-hello-world-web-app-for-azure-in-intellij"></a>Luo Hei maailma verkkosovellukseen IntelliJ Azure

Tässä opetusohjelmassa näytetään, miten voit luoda ja ottaa käyttöön basic Hei maailma sovelluksen Azure kuin Web App [For IntelliJ Azure-työkalujen]avulla. Perustietoja JSP esimerkissä näkyy yksinkertaisuuden, mutta erittäin vastaavat vaiheet on vastaava Java-servlet Azure käyttöönoton osalta.

Kun olet tehnyt Tässä opetusohjelmassa, sovelluksen näyttää seuraavassa kuvassa samalla, kun tarkastelet selaimessa:

![][01]
 
## <a name="prerequisites"></a>Edellytykset

* Java Developer Kit (JDK), v 1,8 tai uudempi versio.
* IntelliJ VERRATA Ultimate Edition. Tämä voi ladata <https://www.jetbrains.com/idea/download/index.html>.
* Java-pohjainen web-palvelimeen tai sovelluspalvelin, kuten Apache Tomcat tai jota jakauman.
* Azure tilauksen, joka on hankittu <https://azure.microsoft.com/free/> tai <http://azure.microsoft.com/pricing/purchase-options/>.
* IntelliJ Azure työkalut. Lisätietoja on artikkelissa [asentamista varten IntelliJ Azure-Työkalut].

## <a name="to-create-a-hello-world-application"></a>Hei maailma sovelluksen luominen

Ensin asetetaan ensin Java projektin luomisesta.

1. Aloita IntelliJ, ja valitse valikosta **Tiedosto**ja valitse **Uusi**ja valitse sitten **Projekti**.

   ![][02]

1. Uusi projekti-valintaikkunassa valitse **Java**, valitse **Web-sovelluksen**, ja valitse sitten **Seuraava**.

   ![][03a]

   **Jatka ei ole määritetty SDK kehotettaessa Kyllä.**

   ![][03b]

1. Tässä opetusohjelmassa tarkoituksiin projektin **Java-Web-sovelluksen – Valitse-Azure**nimi ja valitse sitten **Valmis**.

   ![][04]

1. IntelliJ's Project Explorer-näkymässä Laajenna **Java-Web-sovelluksen – Valitse-Azure**sitten Laajenna **web**ja kaksoisnapsauta sitten **index.jsp**.

   ![][05]

1. Kun index.jsp-tiedosto avautuu IntelliJ, Lisää teksti dynaamisesti **Hei maailma!** sisällä nykyisen `<body>` elementti. Oman päivitetyt `<body>` sisällön pitäisi muistuttaa seuraavassa esimerkissä:

   `<body><b><% out.println("Hello World!"); %></b></body>` 

1. Tallenna index.jsp.

## <a name="to-deploy-your-application-to-an-azure-web-app-container"></a>Ottaa käyttöön sovelluksen Azure Web-sovelluksen säilöön

Jolla voit ottaa Azure Java verkkosovelluksen useilla tavoilla. Tässä opetusohjelmassa kuvataan jokin yksinkertaisin: sovellus otetaan käyttöön Azure Web App-säilö - ei erityistä projektityypin eikä muita työkaluja tarvitaan. JDK ja web-säilön ohjelmisto toimitetaan puolestasi mukaan Azure, niin ei ole tarpeen lataaminen oman; tarvitset on Java koodiin. Tämän vuoksi julkaisuprosessin sovelluksen kestää sekuntia, ei minuuttia.

1. Napsauta hiiren kakkospainikkeella IntelliJ's Project Explorer **Java-Web-sovelluksen – Valitse-Azure** -projekti. Kun pikavalikon tulee näkyviin, valitse **Azure**ja valitse sitten **Julkaise Azure Web Appissa …**

   ![][06]

1. Jos sinulla ei ole jo kirjautunut sisään Azure-IntelliJ, sinun tulee kehotus kirjautua Azure-tili:

   ![][07]

   Huomautus: Jos käytössäsi on useita Azure tilejä, joitakin ohjeita kirjautumisprosessin aikana voi näkyä useammin kuin kerran, vaikka ne näkyvät on sama. Jos näin käy, jatkaa seuraavat ohjeet kirjautuminen.

1. Kun olet kirjautunut sisään Azure-tiliin, **Tilausten hallinta** -valintaikkunassa näkyy luettelon tilauksista, jotka liittyvät tunnistetiedot. Jos määritettynä on useita tilauksia luettelossa ja haluat käsitellä vain tietyn osan niistä, niistä, jota haluat käyttää voi myös poistaa kohdan. Kun olet valinnut tilauksistasi, valitse **Sulje**.

   ![][08]

1. **Ota käyttöön Azure Web App-säilöön** -valintaikkuna tulee näkyviin, kun se näkyy Web App säilöjen, jotka olet aiemmin luonut; Jos et ole luonut säilöjen-luettelo on tyhjä.   

   ![][09]

1. Jos et ole luonut vain Azure Web App säilö ennen tai jos haluat julkaista sovelluksesi uuteen säilöön, käytä seuraavia ohjeita. Muussa tapauksessa valitse aiemmin luotu Web-sovelluksen säilö ja siirry vaiheeseen 6.

  1. Valitse**+**

        ![][10]

  1. **Uusi Web App-säilö** -valintaikkunassa näkyvät, jota käytetään eri vaiheisiin.

        ![][11]

  1. Kirjoita **DNS-otsikon** lisääminen Web-sovelluksen säilön; web-sovelluksen isännän URL-osoite lehti DNS otsikko muodostavat Azure-tietokannassa. Huomautus: Nimen on oltava käytettävissä ja Azure Web Appiin nimeämisvaatimukset mukaisia.

  1. Valitse sovelluksen tarvittavan ohjelman **Web-säilö** -pudotusvalikosta.

        Voit valita tällä hetkellä Tomcat 8, Tomcat 7 tai jota 9. Azure toimittamien viimeisimmät jakauman valitun ohjelmiston ja se suoritetaan JDK 8 Oracle luotu ja Azure myöntämä viimeisimmät jakauman.

  1. Valitse **tilaus** -pudotusvalikosta tilauksen, jota haluat käyttää tätä käyttöönottoa varten.

  1. Valitse pudotusvalikosta **Resurssiryhmä** , johon haluat liittää koodiin resurssiryhmä.

        Huomautus: Azure Resurssiryhmien avulla voit ryhmitellä liittyvät resurssit yhteen niin, että esimerkiksi ne voidaan poistaa yhdessä.

        Voit valita olemassa olevan resurssiryhmä (Jos sinulla on jokin) ja Ohita g alla olevilla tai käyttää näitä vaiheita Luo uusi resurssiryhmä seuraavasti:

      * Valitse **Uusi...**

      * **Uusi resurssiryhmä** -valintaikkunassa näkyvät:

            ![][12]

      * Kirjoita **nimi** -tekstiruutuun nimi uusi resurssiryhmä.

      * Valitse **alueen** avattavan valikon, valitse Azure tiedot keskittää resurssiryhmän sijainti.

      * Valitse **OK**.

  1. **Sovelluksen palvelun suunnitelma** avattavasta valikosta luettelo, jotka liittyvät resurssiryhmän, jonka valitsit app palvelusopimusten vaihtoehdot.

        Huomautus:-Sovelluksen-palvelun suunnittelu määrittää tietoja, kuten sijainti Web App, hinnoittelutiedot taso ja Laske esiintymän koon. Yhden sovelluksen-palvelu-suunnittelu voi käyttää useita verkkosovelluksissa, joka on miksi sitä ylläpidetään tietyn Web App-käyttöönotosta erikseen.

        Voit valita aiemmin luodun sovelluksen palvelun suunnitteleminen (Jos sinulla on jokin) ja Ohita h alla olevilla tai Käytä näitä vaiheita Luo uusi sovellus-palvelu-palvelupaketti:

      * Valitse **Uusi...**

      * **Uuden sovelluksen-palvelun suunnittelu** -valintaikkunassa näkyvät:

            ![][13]

      * Valitse **nimi** -tekstiruutuun nimi oman uuden sovelluksen palvelun suunnitteleminen.

      * - **Sijainti** avattavasta valikosta, valitse Azure tiedot keskittää suunnitelman sijainti.

      * - **Hinnoittelua taso** avattavasta valikosta, valitse haluamasi hinnoittelua palvelupaketin. Voit valita **vapaa**testausta varten.

      * Valitse **Esiintymän koon** avattavasta valikosta, valitse haluamasi esiintymän koko palvelupaketin. Testausta varten voit valita **Pieni**.

  1. Kun olet suorittanut kaikki edellä mainitut vaiheet, uusi Web App-säilö-valintaikkunan pitäisi muistuttaa seuraavassa kuvassa:

        ![][14]

  1. Valitse Viimeistele uuden Web App-säilö luominen **OK** .

        Odota hetki Web App-säilöjen luettelo on päivitettävä ja luomasi web app-säilö pitäisi nyt olla valittuna luettelossa.

1. Olet nyt valmiina suorittamiseen ensimmäisen käyttöönoton Web-sovelluksesi Azure; Valitse **OK** Java-sovellus valitun Web App-säilöön.

    ![][15]

    Huomautus: Oletusarvoisesti sovellus otetaan käyttöön sovelluspalvelin alikansio nimellä. Jos haluat sen pääkansio-sovelluksen käyttöön, valitse **päätasolle käyttöönotto** -valintaruutu ennen kuin valitset **OK**.

1. Seuraavaksi pitäisi näkyä **Azure tehtävän Log** -näkymää, jossa kertovat käyttöönoton tilan koodiin.

    ![][16]

    Web-sovelluksen ottaminen käyttöön ja Azure olisi kestää vain muutaman sekunnin. Kun sovellus on valmis, näyttöön tulee nimeltä **julkaistu** **tila** -sarakkeen linkkiä. Kun olet napsauttanut linkkiä, se siirtää sinut oman käyttöön online-aloitussivulle tai voit käyttää vaiheet seuraavassa osassa selaamalla web App-sovellukseen.

## <a name="browsing-to-your-web-app-on-azure"></a>Web App-sovellukseen, valitse Azure selaaminen

Voit brows Web App-sovellukseen, valitse Azure **Azure** Resurssienhallintanäkymä.

Jos **Azure** Resurssienhallintanäkymä ei ole avoinna, voit avata sen napsauttamalla sitten **Näytä** -valikon IntelliJ, ja valitse sitten **Työkalu Windows**ja valitse sitten **Palvelun Explorer**. Jos ole aiemmin kirjautunut, se kehottaa sinua tekemään niin.

**Azure** Resurssienhallintanäkymä näkyessä Käytä toimi lopettavat Web App seuraavasti: 

1. Laajenna **Azure** -solmu.

1. Laajenna **Web Apps** -solmu. 

1. Napsauta hiiren kakkospainikkeella haluamaasi Web Appissa.

1. Kun pikavalikon tulee näkyviin, valitse **Avaa selaimessa**.

    ![][17]

## <a name="updating-your-web-app"></a>Web-sovelluksen päivittäminen

Aiemmin luodun Azure Web Appia päivittämisestä on nopea ja helppo prosessi ja sinulla on kaksi vaihtoehtoa päivittämistä varten:

* Voit päivittää aiemmin Java-Web Appin käyttöönotto.
* Voit julkaista muita Java-sovelluksen saman Web App-säilöön.

Kummassakin tapauksessa prosessi on sama kuin ja kestää vain muutaman sekunnin:

1. Napsauta hiiren kakkospainikkeella IntelliJ project explorer Java-sovellus, jonka voit päivittää tai lisätä vain aiemmin Web App-säilö.

1. Kun pikavalikon tulee näkyviin, valitse **Azure** ja valitse sitten **Julkaise Azure Web Appissa …**

1. Koska ole vielä kirjautunut aiemmin, näet luettelon aiemmin Web App-säilöjä. Valitse haluat julkaista tai julkaista uudelleen Java-sovellus ja valitse **OK**.

Joitakin sekunteja myöhemmin **Azure tehtävän Log** -näkymässä näytetään **julkaistu** kuin päivitetyt käyttöönoton ja osaat vahvistamiseksi päivitetyt sovelluksen suoraan selaimessa.

## <a name="starting-or-stopping-an-existing-web-app"></a>Aloittaminen tai lopettaminen aiemmin Web App-sovelluksessa

Voit aloittaa tai lopettaa aiemmin Azure Web App-säilö (myös ei otettu käyttöön Java sovellukset), voit käyttää **Azure** Resurssienhallintanäkymä.

Jos **Azure** Resurssienhallintanäkymä ei ole avoinna, voit avata sen napsauttamalla sitten **Näytä** -valikon IntelliJ, ja valitse sitten **Työkalu Windows**ja valitse sitten **Palvelun Explorer**. Jos ole aiemmin kirjautunut, se kehottaa sinua tekemään niin.

**Azure** Resurssienhallintanäkymä näkyessä käyttöä toimimalla seuraavasti voit käynnistää tai pysäyttää Web-sovelluksen: 

1. Laajenna **Azure** -solmu.

1. Laajenna **Web Apps** -solmu. 

1. Napsauta hiiren kakkospainikkeella haluamaasi Web Appissa.

1. Kun pikavalikon tulee näkyviin, valitse **käynnistää** tai **pysäyttää**. Huomaa, että valikkovaihtoehdot ovat konteksti-aware, jotta voit vain käynnissä verkkosovellukseen pysäyttää tai käynnistää web-sovellus, joka ei ole käynnissä.

    ![][18]

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja Java IDEs Azure työkalupakettien on seuraavissa linkeissä:

- [Azure työkalujen Pimennys varten]
  - [Azure-työkalut asennetaan Pimennys]
  - [Luo Hei maailma verkkosovellukseen Pimennys Azure]
  - [Mitä uusia ominaisuuksia, Pimennys Azure Työkalut]
- [Azure työkalujen IntelliJ varten]
  - [Azure-työkalut asennetaan IntelliJ]
  - *Luo Hei maailma verkkosovellukseen Azure-IntelliJ (tämä artikkeli)*
  - [Mitä uusia ominaisuuksia Azure työkalujen IntelliJ varten]

Saat lisätietoja Azure käyttäminen Java [Azure Java Developer Center].

Lisätietoja Azure Web Apps-sovellusten luomisesta on artikkelissa [Yleistä Web Apps].

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[Azure työkalujen Pimennys varten]: ../azure-toolkit-for-eclipse.md
[Azure työkalujen IntelliJ varten]: ../azure-toolkit-for-intellij.md
[Luo Hei maailma verkkosovellukseen Pimennys Azure]: ./app-service-web-eclipse-create-hello-world-web-app.md
[Create a Hello World Web App for Azure in IntelliJ]: ./app-service-web-intellij-create-hello-world-web-app.md
[Azure-työkalut asennetaan Pimennys]: ../azure-toolkit-for-eclipse-installation.md
[Azure-työkalut asennetaan IntelliJ]: ../azure-toolkit-for-intellij-installation.md
[Mitä uusia ominaisuuksia, Pimennys Azure Työkalut]: ../azure-toolkit-for-eclipse-whats-new.md
[Mitä uusia ominaisuuksia Azure työkalujen IntelliJ varten]: ../azure-toolkit-for-intellij-whats-new.md

[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Web-sovellusten yleiskatsaus]: ./app-service-web-overview.md

<!-- IMG List -->

[01]: ./media/app-service-web-intellij-create-hello-world-web-app/01-Web-Page.png
[02]: ./media/app-service-web-intellij-create-hello-world-web-app/02-File-New-Project.png
[03a]: ./media/app-service-web-intellij-create-hello-world-web-app/03-New-Project-Dialog.png
[03b]: ./media/app-service-web-intellij-create-hello-world-web-app/03-No-SDK-Specified.png
[04]: ./media/app-service-web-intellij-create-hello-world-web-app/04-New-Project-Dialog.png
[05]: ./media/app-service-web-intellij-create-hello-world-web-app/05-Open-Index-Page.png
[06]: ./media/app-service-web-intellij-create-hello-world-web-app/06-Azure-Publish-Context-Menu.png
[07]: ./media/app-service-web-intellij-create-hello-world-web-app/07-Azure-Log-In-Dialog.png
[08]: ./media/app-service-web-intellij-create-hello-world-web-app/08-Manage-Subscriptions.png
[09]: ./media/app-service-web-intellij-create-hello-world-web-app/09-App-Containers.png
[10]: ./media/app-service-web-intellij-create-hello-world-web-app/10-Add-App-Container.png
[11]: ./media/app-service-web-intellij-create-hello-world-web-app/11-New-App-Container.png
[12]: ./media/app-service-web-intellij-create-hello-world-web-app/12-New-Resource-Group.png
[13]: ./media/app-service-web-intellij-create-hello-world-web-app/13-New-App-Service-Plan.png
[14]: ./media/app-service-web-intellij-create-hello-world-web-app/14-New-App-Container.png
[15]: ./media/app-service-web-intellij-create-hello-world-web-app/15-Deploy-To-Azure.png
[16]: ./media/app-service-web-intellij-create-hello-world-web-app/16-Progress-Indicator.png
[17]: ./media/app-service-web-intellij-create-hello-world-web-app/17-Browse-Web-App.png
[18]: ./media/app-service-web-intellij-create-hello-world-web-app/18-Stop-Web-App.png
