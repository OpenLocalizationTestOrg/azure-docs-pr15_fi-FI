<properties 
    pageTitle="Luo Hei maailma verkkosovellukseen Pimennys Azure | Microsoft Azure" 
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
    ms.date="08/26/2016" 
    ms.author="robmcm"/>

# <a name="create-a-hello-world-web-app-for-azure-in-eclipse"></a>Luo Hei maailma verkkosovellukseen Pimennys Azure

Tässä opetusohjelmassa näytetään, miten voit luoda ja ottaa käyttöön basic Hei maailma sovelluksen Azure kuin Web App [For Pimennys Azure-työkalujen]avulla. Perustietoja JSP esimerkissä näkyy yksinkertaisuuden, mutta erittäin vastaavat vaiheet on vastaava Java-servlet Azure käyttöönoton osalta.

Kun olet tehnyt Tässä opetusohjelmassa, sovelluksen näyttää seuraavassa kuvassa samalla, kun tarkastelet selaimessa:

![Esikatselu Hei maailma app][01]
 
## <a name="prerequisites"></a>Edellytykset

* Java Developer Kit (JDK), v 1,8 tai uudempi versio.
* Pimennys IDE Java ss kehittäjille Luna tai uudempi versio. Tämä voi ladata <http://www.eclipse.org/downloads/>.
* Java-pohjainen web-palvelimeen tai sovelluspalvelin, kuten Apache Tomcat tai jota jakauman.
* Azure tilauksen, joka on hankittu <https://azure.microsoft.com/free/> tai <http://azure.microsoft.com/pricing/purchase-options/>.
* Pimennys Azure työkalut. Lisätietoja on artikkelissa [asentamista varten Pimennys Azure-Työkalut].

## <a name="to-create-a-hello-world-application"></a>Hei maailma sovelluksen luominen

Ensin asetetaan ensin Java projektin luomisesta.

1. Käynnistä Pimennys, ja valitse **Tiedosto**valikosta, **Uusi**ja valitse sitten **Dynaamiset Web-projekti**. (Jos et näe **Dynaaminen Web-projekti** peruutuksesta käytettävissä projektin jälkeen valitsemalla **Tiedosto** - ja **Uusi**, toimi seuraavasti: Valitse **Tiedosto**, **Uusi**, valitse **projektia**, laajenna **Web**, **dynaaminen Web**-projekti ja valitse **Seuraava**.)

1. Tässä opetusohjelmassa tarkoituksiin nimeä projektin **MyWebApp**. Näyttöön tulee näkyviin seuraavankaltaiselta:

    ![Uuden dynaaminen Web-projektin luominen][02]

1. Valitse **Valmis**.

1. Laajenna **MyWebApp**Pimennys 's Project Explorer-näkymässä. **WebContent**hiiren kakkospainikkeella, valitse **Uusi**ja valitse sitten **JSP tiedosto**.

1. **Uusi JSP tiedosto** -valintaikkunassa nimi tiedoston **index.jsp**, pitää pääkansion **MyWebApp/WebContent**nimellä ja valitse sitten **Seuraava**.

1. **Valitse JSP malli** -valintaikkunassa tarkoitetaan tässä opetusohjelmassa valitsemalla **Uusi JSP-tiedosto (html)**, ja valitse sitten **Valmis**.

1. Kun index.jsp-tiedosto avautuu Pimennys, Lisää teksti dynaamisesti **Hei maailma!** sisällä nykyisen `<body>` elementti. Oman päivitetyt `<body>` sisällön pitäisi muistuttaa seuraavassa esimerkissä:

    `<body><b><% out.println("Hello World!"); %></b></body>` 

1. Tallenna index.jsp.

## <a name="to-deploy-your-application-to-an-azure-web-app-container"></a>Ottaa käyttöön sovelluksen Azure Web-sovelluksen säilöön

Jolla voit ottaa Azure Java verkkosovelluksen useilla tavoilla. Tässä opetusohjelmassa kuvataan jokin yksinkertaisin: sovellus otetaan käyttöön Azure Web App-säilö - ei erityistä projektityypin eikä muita työkaluja tarvitaan. JDK ja web-säilön ohjelmisto toimitetaan puolestasi mukaan Azure, niin ei ole tarpeen lataaminen oman; tarvitset on Java koodiin. Tämän vuoksi julkaisuprosessin sovelluksen kestää sekuntia, ei minuuttia.

1. Napsauta hiiren kakkospainikkeella Pimennys 's Project Explorer **MyWebApp**.

1. Valitse pikavalikosta **Azure**ja valitse sitten **Julkaise Azure Web Appissa …**

    ![Julkaise kuin Azure Web Appissa][03]
   
    Voit myös silloin, kun Project Explorer on valittuna web-sovelluksen projektiin, voit napsauttamalla työkalurivin **Julkaise** avattava valikko-painiketta ja valita **Azure Web App Julkaise** sieltä:
   
    ![Julkaise kuin Azure Web Appissa][14]
   
1. Jos sinulla ei ole jo kirjautunut sisään Azure-Pimennys, sinun tulee kehotus kirjautua Azure-tili:

    ![Azure Kirjaudu sisään-valintaikkuna][04]
   
    Jos sinulla on useita Azure-tilejä, joitakin ohjeita kirjautumisprosessin aikana voi näkyä useammin kuin kerran, vaikka ne näkyvät on sama. Jos näin käy, jatkaa seuraavat ohjeet kirjautuminen.

1. Kun olet kirjautunut sisään Azure-tiliin, **Tilausten hallinta** -valintaikkunassa näkyy luettelon tilauksista, jotka liittyvät tunnistetiedot. Jos määritettynä on useita tilauksia luettelossa ja haluat käsitellä vain tietyn osan niistä, niistä, jota haluat käyttää voi myös poistaa kohdan. Kun olet valinnut tilauksistasi, valitse **Sulje**.

    ![Hallitse tilaukset-valintaikkuna][05]
   
1. **Ota käyttöön Azure Web App-säilöön** -valintaikkuna tulee näkyviin, kun se näkyy Web App säilöjen, jotka olet aiemmin luonut; Jos et ole luonut säilöjen-luettelo on tyhjä.

    ![Ottaa käyttöön Azure Web App-säilö-valintaikkuna][06]
   
1. Jos et ole luonut vain Azure Web App säilö ennen tai jos haluat julkaista sovelluksesi uuteen säilöön, käytä seuraavia ohjeita. Muussa tapauksessa valitse aiemmin luotu Web-sovelluksen säilö ja siirry vaiheeseen 7.

    1. Valitse **Uusi...**

        ![Ottaa käyttöön Azure Web App-säilö-valintaikkuna][15]

    1. **Uusi Web App-säilö** -valintaikkunassa näkyvät:

        ![Uusi Web App-säilö-valintaikkuna][07a]

    1. Kirjoita **DNS-otsikon** lisääminen Web-sovelluksen säilön; web-sovelluksen isännän URL-osoite lehti DNS otsikko muodostavat Azure-tietokannassa. (Huomaa, että nimi on oltava käytettävissä ja Azure Web Appiin nimeämisvaatimukset mukaisia.)

    1. Valitse sovelluksen tarvittavan ohjelman **Web-säilö** -pudotusvalikosta.

        Voit valita tällä hetkellä Tomcat 8, Tomcat 7 tai jota 9. Azure toimittamien viimeisimmät jakautumisen valitun ohjelmisto ja se suoritetaan JDK 8 Oracle luotu ja Azure myöntämä viimeisimmät jakauman.

    1. Valitse **tilaus** -pudotusvalikosta tilauksen, jota haluat käyttää tätä käyttöönottoa varten.

    1. Valitse pudotusvalikosta **Resurssiryhmä** , johon haluat liittää koodiin resurssiryhmä. (Azure Resurssiryhmien avulla voit ryhmitellä liittyvät resurssit yhteen niin, että esimerkiksi ne voidaan poistaa yhdessä.)

        Voit valita olemassa olevan resurssiryhmä (Jos sinulla on jokin) ja Ohita g alla olevilla tai käyttää näitä vaiheita Luo uusi resurssiryhmä seuraavasti:

        * Valitse **Uusi...**

        * **Uusi resurssiryhmä** -valintaikkunassa näkyvät:

            ![Uusi resurssiryhmä-valintaikkuna][08]

        * Kirjoita **nimi** -tekstiruutuun nimi uusi resurssiryhmä.

        * Valitse **alueen** avattavan valikon, valitse Azure tiedot keskittää resurssiryhmän sijainti.

        * Valinnainen: Oletusarvoisesti viimeisimmät jakautumisen Java 8 otetaan käyttöön Azure mukaan automaattisesti web app-säilö oman JVM nimellä. Voit kuitenkin määrittää eri versiota ja jakelu JVM Jos Web Appissa edellyttää sitä. Voit määrittää JDK Web App- **JDK** -välilehti ja valitse jokin seuraavista vaihtoehdoista:

            * **Käyttöönotto oletusarvo JDK tarjoamia Azure Web Apps-palvelu**: Tämä asetus otetaan käyttöön viimeisimmät jakautumisen Java 8.

            * **Käyttöönotto 3rd osapuolen JDK käytettävissä Azure**: tämän asetuksen avulla voit valita JDKs, joka tarjoaa Microsoft Azure-luettelosta.

            * **Käyttöönotto omaa JDK Lataa täältä**: tämän asetuksen avulla voit määrittää oman JDK jakauman, joka on pakattu ZIP-tiedostona ja ladata yleiseen käyttöön Lataa sijainti tai Azure-tallennustilan tilin, jonka voit käyttää.

            ![Uusi Web App-säilö-valintaikkuna][07b]

    * Valitse **OK**.

    1. **Sovelluksen palvelun suunnitelma** avattavasta valikosta luettelo, jotka liittyvät resurssiryhmän, jonka valitsit app palvelusopimusten vaihtoehdot. (Sovelluksen palvelusopimusten vaihtoehdot määrittää tietoja, kuten sijainti Web App, hinnoittelutiedot taso ja Laske esiintymän koon. Yhden sovelluksen-palvelu-suunnittelu voi käyttää useita verkkosovelluksissa, joka on miksi sitä ylläpidetään erikseen tietyn Web App-käyttöönotosta.)

        Voit valita aiemmin luodun sovelluksen palvelun suunnitteleminen (Jos sinulla on jokin) ja Ohita h alla olevilla tai Käytä näitä vaiheita Luo uusi sovellus-palvelu-palvelupaketti:

        * Valitse **Uusi...**

        * **Uuden sovelluksen-palvelun suunnittelu** -valintaikkunassa näkyvät:

            ![Uusi sovellus-palvelun suunnittelu-valintaikkuna][09]

        * Valitse **nimi** -tekstiruutuun nimi oman uuden sovelluksen palvelun suunnitteleminen.

        * - **Sijainti** avattavasta valikosta, valitse Azure tiedot keskittää suunnitelman sijainti.

        * - **Hinnoittelua taso** avattavasta valikosta, valitse haluamasi hinnoittelua palvelupaketin. Voit valita **vapaa**testausta varten.

        * Valitse **Esiintymän koon** avattavasta valikosta, valitse haluamasi esiintymän koko palvelupaketin. Testausta varten voit valita **Pieni**.

    1. Kun olet suorittanut kaikki edellä mainitut vaiheet, uusi Web App-säilö-valintaikkunan pitäisi muistuttaa seuraavassa kuvassa:

        ![Uusi Web App-säilö-valintaikkuna][10]

    1. Valitse Viimeistele uuden Web App-säilö luominen **OK** .

        Odota hetki Web App-säilöjen luettelo on päivitettävä ja luomasi web app-säilö pitäisi nyt olla valittuna luettelossa.

1. Olet nyt valmiina ensimmäisen käyttöönoton Web-sovelluksesi Azure suorittamiseen:

    ![Ottaa käyttöön Azure Web App-säilö-valintaikkuna][11]

    Valitse **OK** Java-sovellus valitun Web App-säilöön.

    Oletusarvon mukaan sovellus otetaan käyttöön sovelluspalvelin alikansio nimellä. Jos haluat sen pääkansio-sovelluksen käyttöön, valitse **päätasolle käyttöönotto** -valintaruutu ennen kuin valitset **OK**.

1. Seuraavaksi pitäisi näkyä **Azure tehtävän Log** -näkymää, jossa kertovat käyttöönoton tilan koodiin.

    ![Azure tehtävän loki][12]

    Web-sovelluksen ottaminen käyttöön ja Azure olisi kestää vain muutaman sekunnin. Kun sovellus on valmis, näyttöön tulee nimeltä **julkaistu** **tila** -sarakkeen linkkiä. Kun olet napsauttanut linkkiä, se vievät sinut oman käyttöön online-aloitussivu.

## <a name="updating-your-web-app"></a>Web-sovelluksen päivittäminen

Aiemmin luodun Azure Web Appia päivittämisestä on nopea ja helppo prosessi ja sinulla on kaksi vaihtoehtoa päivittämistä varten:

* Voit päivittää aiemmin Java-Web Appin käyttöönotto.
* Voit julkaista muita Java-sovelluksen saman Web App-säilöön.

Kummassakin tapauksessa prosessi on sama kuin ja kestää vain muutaman sekunnin:

1. Napsauta hiiren kakkospainikkeella Pimennys project explorer Java-sovellus, jonka voit päivittää tai lisätä vain aiemmin Web App-säilö.

1. Kun pikavalikon tulee näkyviin, valitse **Azure** ja valitse sitten **Julkaise Azure Web Appissa …**

1. Koska ole vielä kirjautunut aiemmin, näet luettelon aiemmin Web App-säilöjä. Valitse haluat julkaista tai julkaista uudelleen Java-sovellus ja valitse **OK**.

Joitakin sekunteja myöhemmin **Azure tehtävän Log** -näkymässä näytetään **julkaistu** kuin päivitetyt käyttöönoton ja osaat vahvistamiseksi päivitetyt sovelluksen suoraan selaimessa.

## <a name="stopping-an-existing-web-app"></a>Olemassa olevan Web-sovelluksen pysäyttäminen

Jos haluat lopettaa aiemmin luodun Azure Web App säilöön, (myös ei otettu käyttöön Java sovellukset), voit käyttää **Azure** Resurssienhallintanäkymä.

Jos **Azure** Resurssienhallintanäkymä ei ole avoinna, voit avata sen napsauttamalla sitten Pimennys- **ikkuna** -valikko ja valitse **Näytä**ja sitten **Muut...**sitten **Azure**ja valitse sitten **Azure Explorer**. Jos ole aiemmin kirjautunut, se kehottaa sinua tekemään niin.

**Azure** Resurssienhallintanäkymä näkyessä Käytä toimi lopettavat Web App seuraavasti: 

1. Laajenna **Azure** -solmu.

1. Laajenna **Web Apps** -solmu. 

1. Napsauta hiiren kakkospainikkeella haluamaasi Web Appissa.

1. Kun pikavalikon tulee näkyviin, valitse **Lopeta**.

    ![Olemassa olevan Web-sovelluksen pysäyttäminen][13]

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja Java IDEs Azure työkalupakettien on seuraavissa linkeissä:

- [Azure työkalujen Pimennys varten]
  - [Azure-työkalut asennetaan Pimennys]
  - *Luo Hei maailma verkkosovellukseen Azure-Pimennys (tämä artikkeli)*
  - [Mitä uusia ominaisuuksia, Pimennys Azure Työkalut]
- [Azure työkalujen IntelliJ varten]
  - [Azure-työkalut asennetaan IntelliJ]
  - [Luo Hei maailma verkkosovellukseen IntelliJ Azure]
  - [Mitä uusia ominaisuuksia Azure työkalujen IntelliJ varten]

Saat lisätietoja Azure käyttäminen Java [Azure Java Developer Center].

Lisätietoja Azure Web Apps-sovellusten luomisesta on artikkelissa [Yleistä Web Apps].

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[Azure työkalujen Pimennys varten]: ../azure-toolkit-for-eclipse.md
[Azure työkalujen IntelliJ varten]: ../azure-toolkit-for-intellij.md
[Create a Hello World Web App for Azure in Eclipse]: ./app-service-web-eclipse-create-hello-world-web-app.md
[Luo Hei maailma verkkosovellukseen IntelliJ Azure]: ./app-service-web-intellij-create-hello-world-web-app.md
[Azure-työkalut asennetaan Pimennys]: ../azure-toolkit-for-eclipse-installation.md
[Azure-työkalut asennetaan IntelliJ]: ../azure-toolkit-for-intellij-installation.md
[Mitä uusia ominaisuuksia, Pimennys Azure Työkalut]: ../azure-toolkit-for-eclipse-whats-new.md
[Mitä uusia ominaisuuksia Azure työkalujen IntelliJ varten]: ../azure-toolkit-for-intellij-whats-new.md

[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Web-sovellusten yleiskatsaus]: ./app-service-web-overview.md

<!-- IMG List -->

[01]: ./media/app-service-web-eclipse-create-hello-world-web-app/01-Web-Page.png
[02]: ./media/app-service-web-eclipse-create-hello-world-web-app/02-Dynamic-Web-Project.png
[03]: ./media/app-service-web-eclipse-create-hello-world-web-app/03-Context-Menu.png
[04]: ./media/app-service-web-eclipse-create-hello-world-web-app/04-Log-In-Dialog.png
[05]: ./media/app-service-web-eclipse-create-hello-world-web-app/05-Manage-Subscriptions-Dialog.png
[06]: ./media/app-service-web-eclipse-create-hello-world-web-app/06-Deploy-To-Azure-Web-Container.png
[07a]: ./media/app-service-web-eclipse-create-hello-world-web-app/07a-New-Web-App-Container-Dialog.png
[07b]: ./media/app-service-web-eclipse-create-hello-world-web-app/07b-New-Web-App-Container-Dialog.png
[08]: ./media/app-service-web-eclipse-create-hello-world-web-app/08-New-Resource-Group-Dialog.png
[09]: ./media/app-service-web-eclipse-create-hello-world-web-app/09-New-Service-Plan-Dialog.png
[10]: ./media/app-service-web-eclipse-create-hello-world-web-app/10-Completed-Web-App-Container-Dialog.png
[11]: ./media/app-service-web-eclipse-create-hello-world-web-app/11-Completed-Deploy-Dialog.png
[12]: ./media/app-service-web-eclipse-create-hello-world-web-app/12-Activity-Log-View.png
[13]: ./media/app-service-web-eclipse-create-hello-world-web-app/13-Azure-Explorer-Web-App.png
[14]: ./media/app-service-web-eclipse-create-hello-world-web-app/14-publishDropdownButton.png
[15]: ./media/app-service-web-eclipse-create-hello-world-web-app/15-New-Azure-Web-Container.png
