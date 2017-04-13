<properties 
    pageTitle="Luo Hei maailma verkkosovellukseen Pimennys Azure" 
    description="Tässä opetusohjelmassa kerrotaan, miten saat Pimennys Azure-työkalujen avulla voit luoda Hei maailma verkkosovellukseen Azure varten." 
    services="app-service\web" 
    documentationCenter="java" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

# <a name="create-a-hello-world-web-app-for-azure-in-eclipse"></a>Luo Hei maailma verkkosovellukseen Pimennys Azure

Tässä opetusohjelmassa näytetään, miten voit luoda ja ottaa käyttöön basic Hei maailma sovelluksen Azure kuin Web App [For Pimennys Azure-työkalujen]avulla. Perustietoja JSP esimerkissä näkyy yksinkertaisuuden, mutta erittäin vastaavat vaiheet on vastaava Java-servlet Azure käyttöönoton osalta.

Kun olet tehnyt Tässä opetusohjelmassa, sovelluksen näyttää seuraavassa kuvassa samalla, kun tarkastelet selaimessa:

![][01]
 
## <a name="prerequisites"></a>Edellytykset

* Java Developer Kit (JDK), v 1.7 tai uudempi.
* Pimennys IDE Java ss kehittäjille Indigo tai uudempi versio. Tämä voi ladata <http://www.eclipse.org/downloads/>.
* Java-pohjainen web-palvelimeen tai sovelluspalvelin, kuten Apache Tomcat tai jota jakauman.
* Azure tilauksen, joka on hankittu <https://azure.microsoft.com/en-us/free/> tai <http://azure.microsoft.com/pricing/purchase-options/>.
* Pimennys Azure työkalut. Lisätietoja on artikkelissa [asentamista varten Pimennys Azure-Työkalut].

## <a name="to-create-a-hello-world-application"></a>Hei maailma sovelluksen luominen

Ensin asetetaan ensin Java projektin luomisesta.

1. Käynnistä Pimennys, ja valitse **Tiedosto**valikosta, **Uusi**ja valitse sitten **Dynaamiset Web-projekti**. (Jos et näe **Dynaaminen Web-projekti** peruutuksesta käytettävissä projektin jälkeen valitsemalla **Tiedosto** - ja **Uusi**, toimi seuraavasti: Valitse **Tiedosto**, **Uusi**, valitse **projektia**, laajenna **Web**, **dynaaminen Web**-projekti ja valitse **Seuraava**.)

1. Tässä opetusohjelmassa tarkoituksiin nimeä projektin **MyHelloWorld**. Näyttöön tulee näkyviin seuraavankaltaiselta:

    ![][02]

1. Valitse **Valmis**.

1. Laajenna **MyHelloWorld**Pimennys 's Project Explorer-näkymässä. **WebContent**hiiren kakkospainikkeella, valitse **Uusi**ja valitse sitten **JSP tiedosto**.

1. Nimeä tiedosto **index.jsp** **JSP uusi tiedosto** -valintaikkunassa. Pidä pääkansion mahdollisimman **MyHelloWorld/WebContent**.

1. **Valitse JSP malli** -valintaikkunassa tarkoitetaan tässä opetusohjelmassa valitsemalla **Uusi JSP-tiedosto (html)**, ja valitse sitten **Valmis**.

1. Kun index.jsp-tiedosto avautuu Pimennys, Lisää teksti dynaamisesti **Hei maailma!** sisällä nykyisen `<body>` elementti. Oman päivitetyt `<body>` sisällön pitäisi muistuttaa seuraavassa esimerkissä:

    `<body><b><% out.println("Hello World!"); %></b></body>` 

1. Tallenna index.jsp.

## <a name="to-deploy-your-application-to-an-azure-web-app-container"></a>Ottaa käyttöön sovelluksen Azure Web-sovelluksen säilöön

Jolla voit ottaa Azure Java verkkosovelluksen useilla tavoilla. Tässä opetusohjelmassa kuvataan jokin yksinkertaisin: sovellus otetaan käyttöön Azure Web App-säilö - ei erityistä projektityypin eikä muita työkaluja tarvitaan. JDK ja web-säilön ohjelmisto toimitetaan puolestasi mukaan Azure, niin ei ole tarpeen lataaminen oman; tarvitset on Java koodiin. Tämän vuoksi julkaisuprosessin sovelluksen kestää sekuntia, ei minuuttia.

1. Napsauta hiiren kakkospainikkeella Pimennys 's Project Explorer **MyHelloWorld**.

1. Valitse pikavalikosta **Azure**ja valitse sitten **Julkaise Azure Web Appissa …**

    ![][03]
   
    Voit myös silloin, kun Project Explorer on valittuna web-sovelluksen projektiin, voit napsauttamalla työkalurivin **Julkaise** avattava valikko-painiketta ja valita **Azure Web App Julkaise** sieltä:
   
    ![][14]
   
1. Jos sinulla ei ole jo kirjautunut sisään Azure-Pimennys, sinun tulee kehotus kirjautua Azure-tili:

    ![][04]
   
    Huomautus: Jos käytössäsi on useita Azure tilejä, joitakin ohjeita kirjautumisprosessin aikana voi näkyä useammin kuin kerran, vaikka ne näkyvät on sama. Jos näin käy, jatkaa seuraavat ohjeet kirjautuminen.
1. Kun olet kirjautunut sisään Azure-tiliin, **Tilausten hallinta** -valintaikkunassa näkyy luettelon tilauksista, jotka liittyvät tunnistetiedot. Jos määritettynä on useita tilauksia luettelossa ja haluat käsitellä vain tietyn osan niistä, niistä, jota haluat käyttää voi myös poistaa kohdan. Kun olet valinnut tilauksistasi, valitse **Sulje**.

    ![][05]
   
1. **Ota käyttöön Azure Web App-säilöön** -valintaikkuna tulee näkyviin, kun se näkyy Web App säilöjen, jotka olet aiemmin luonut; Jos et ole luonut säilöjen-luettelo on tyhjä.

    ![][06]
   
1. Jos et ole luonut vain Azure Web App säilö ennen tai jos haluat julkaista sovelluksesi uuteen säilöön, käytä seuraavia ohjeita. Muussa tapauksessa valitse aiemmin luotu Web-sovelluksen säilö ja siirry vaiheeseen 7.

    1. Valitse **Uusi...**

    1. **Uusi Web App-säilö** -valintaikkunassa näkyvät:

        ![][07]

    1. Kirjoita **DNS-otsikon** lisääminen Web-sovelluksen säilön; web-sovelluksen isännän URL-osoite lehti DNS otsikko muodostavat Azure-tietokannassa. Huomautus: Nimen on oltava käytettävissä ja Azure Web Appiin nimeämisvaatimukset mukaisia.

    1. Valitse sovelluksen tarvittavan ohjelman **Web-säilö** -pudotusvalikosta.

        Voit valita tällä hetkellä Tomcat 8, Tomcat 7 tai jota 9. Azure toimittamien viimeisimmät jakautumisen valitun ohjelmisto ja se suoritetaan JDK 8 Oracle luotu ja Azure myöntämä viimeisimmät jakauman.

    1. Valitse **tilaus** -pudotusvalikosta tilauksen, jota haluat käyttää tätä käyttöönottoa varten.

    1. Valitse pudotusvalikosta **Resurssiryhmä** , johon haluat liittää koodiin resurssiryhmä.

        Huomautus: Azure Resurssiryhmien avulla voit ryhmitellä liittyvät resurssit yhteen niin, että esimerkiksi ne voidaan poistaa yhdessä.

        Voit valita olemassa olevan resurssiryhmä (Jos sinulla on jokin) ja Ohita g alla olevilla tai käyttää näitä vaiheita Luo uusi resurssiryhmä seuraavasti:

        * Valitse **Uusi...**

        * **Uusi resurssiryhmä** -valintaikkunassa näkyvät:

            ![][08]

        * Kirjoita **nimi** -tekstiruutuun nimi uusi resurssiryhmä.

        * Valitse **alueen** avattavan valikon, valitse Azure tiedot keskittää resurssiryhmän sijainti.

        * Valitse **OK**.

    1. **Sovelluksen palvelun suunnitelma** avattavasta valikosta luettelo, jotka liittyvät resurssiryhmän, jonka valitsit app palvelusopimusten vaihtoehdot.

        Huomautus:-Sovelluksen-palvelun suunnittelu määrittää tietoja, kuten sijainti Web App, hinnoittelutiedot taso ja Laske esiintymän koon. Yhden sovelluksen-palvelu-suunnittelu voi käyttää useita verkkosovelluksissa, joka on miksi sitä ylläpidetään tietyn Web App-käyttöönotosta erikseen.

        Voit valita aiemmin luodun sovelluksen palvelun suunnitteleminen (Jos sinulla on jokin) ja Ohita h alla olevilla tai Käytä näitä vaiheita Luo uusi sovellus-palvelu-palvelupaketti:

        * Valitse **Uusi...**

        * **Uuden sovelluksen-palvelun suunnittelu** -valintaikkunassa näkyvät:

            ![][09]

        * Valitse **nimi** -tekstiruutuun nimi oman uuden sovelluksen palvelun suunnitteleminen.

        * - **Sijainti** avattavasta valikosta, valitse Azure tiedot keskittää suunnitelman sijainti.

        * - **Hinnat taso** avattavasta valikosta, valitse haluamasi hinnat palvelupaketin. Voit valita **vapaa**testausta varten.

        * Valitse **Esiintymän koon** avattavasta valikosta, valitse haluamasi esiintymän koko palvelupaketin. Testausta varten voit valita **Pieni**.

    1. Kun olet suorittanut kaikki edellä mainitut vaiheet, uusi Web App-säilö-valintaikkunan pitäisi muistuttaa seuraavassa kuvassa:

        ![][10]

    1. Valitse Viimeistele uuden Web App-säilö luominen **OK** .

        Odota hetki Web App-säilöjen luettelo on päivitettävä ja luomasi web app-säilö pitäisi nyt olla valittuna luettelossa.

1. Olet nyt valmiina ensimmäisen käyttöönoton Web-sovelluksesi Azure suorittamiseen:

    ![][11]

    Valitse **OK** Java-sovellus valitun Web App-säilöön.

    Huomautus: Oletusarvoisesti sovellus otetaan käyttöön sovelluspalvelin alikansio nimellä. Jos haluat sen pääkansio-sovelluksen käyttöön, valitse **päätasolle käyttöönotto** -valintaruutu ennen kuin valitset **OK**.

1. Seuraavaksi pitäisi näkyä **Azure tehtävän Log** -näkymää, jossa kertovat käyttöönoton tilan koodiin.

    ![][12]

    Web-sovelluksen ottaminen käyttöön ja Azure olisi kestää vain muutaman sekunnin. Kun sovellus on valmis, näyttöön tulee nimeltä **julkaistu** **tila** -sarakkeen linkkiä. Kun olet napsauttanut linkkiä, se vievät sinut oman käyttöön online-aloitussivu.

## <a name="updating-your-web-app"></a>Web-sovelluksen päivittäminen

Aiemmin luodun Azure Web Appia päivittämisestä on nopea ja helppo prosessi ja sinulla on kaksi vaihtoehtoa päivittämistä varten:

* Voit päivittää aiemmin Java-Web Appin käyttöönotto.
* Voit julkaista muita Java-sovelluksen saman Web App-säilöön.

Kummassakin tapauksessa prosessi on sama kuin ja kestää vain muutaman sekunnin:

1. Napsauta hiiren kakkospainikkeella Pimennys project explorer Java-sovellus, jonka voit päivittää tai lisätä vain aiemmin Web App-säilö.

2. Kun pikavalikon tulee näkyviin, valitse **Azure** ja valitse sitten **Julkaise Azure Web Appissa …**

3. Koska ole vielä kirjautunut aiemmin, näet luettelon aiemmin Web App-säilöjä. Valitse haluat julkaista tai julkaista uudelleen Java-sovellus ja valitse **OK**.

Joitakin sekunteja myöhemmin **Azure tehtävän Log** -näkymässä näytetään **julkaistu** kuin päivitetyt käyttöönoton ja osaat vahvistamiseksi päivitetyt sovelluksen suoraan selaimessa.

## <a name="stopping-an-existing-web-app"></a>Olemassa olevan Web-sovelluksen pysäyttäminen

Jos haluat lopettaa aiemmin luodun Azure Web App säilöön, (myös ei otettu käyttöön Java sovellukset), voit käyttää **Azure** Resurssienhallintanäkymä.

Jos **Azure** Resurssienhallintanäkymä ei ole avoinna, voit avata sen napsauttamalla sitten Pimennys- **ikkuna** -valikko ja valitse **Näytä**ja sitten **Muut...**sitten **Azure**ja valitse sitten **Azure Explorer**. Jos ole aiemmin kirjautunut, se kehottaa sinua tekemään niin.

**Azure** Resurssienhallintanäkymä näkyessä Käytä toimi lopettavat Web App seuraavasti: 

1. Laajenna **Azure** -solmu.

1. Laajenna **Web Apps** -solmu. 

1. Napsauta hiiren kakkospainikkeella haluamaasi Web Appissa.

1. Kun pikavalikon tulee näkyviin, valitse **Lopeta**.

    ![][13]

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja on seuraavissa linkeissä:

* [Java Developer Center](/develop/java/).
* [Web-sovellusten yleiskatsaus](app-service-web-overview.md)

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[Azure App Service]: http://go.microsoft.com/fwlink/?LinkId=529714
[Azure työkalujen Pimennys varten]: http://go.microsoft.com/fwlink/?LinkID=699529
[Azure-työkalut asennetaan Pimennys]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[01]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/01-Web-Page.png
[02]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/02-Dynamic-Web-Project.png
[03]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/03-Context-Menu.png
[04]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/04-Log-In-Dialog.png
[05]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/05-Manage-Subscriptions-Dialog.png
[06]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/06-Deploy-To-Azure-Web-Container.png
[07]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/07-New-Web-App-Container-Dialog.png
[08]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/08-New-Resource-Group-Dialog.png
[09]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/09-New-Service-Plan-Dialog.png
[10]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/10-Completed-Web-App-Container-Dialog.png
[11]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/11-Completed-Deploy-Dialog.png
[12]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/12-Activity-Log-View.png
[13]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/13-Azure-Explorer-Web-App.png
[14]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/publishDropdownButton.png