<properties
   pageTitle="Voit siirtää ja julkaista Web-sovelluksen Azure pilvipalveluun Visual Studio | Microsoft Azure"
   description="Lue, miten voit siirtää ja julkaista web-sovelluksen Azure pilvipalveluun Visual Studiossa."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="how-to-migrate-and-publish-a-web-application-to-an-azure-cloud-service-from-visual-studio"></a>Toimintaohje: Siirrä ja julkaista Web-sovelluksen Azure pilvipalveluun Visual Studio

Ottaa etuna isännöintipalveluina ja Azure skaalattavuus, haluat ehkä siirtää ja julkaise web-sovelluksen Azure pilvipalveluun. Voit suorittaa web-sovelluksen Azure mahdollisimman vähän muutoksia aiemmin luotuun sovellukseen.

>[AZURE.NOTE] Tässä ohjeaiheessa on tietoja käyttöönotossa pilvipalvelut, ei ole sivustoja. Saat tietoja käyttöönotosta sivustoihin [käyttöönotto Azure-sovelluksen palvelun verkkosovellukseen](./app-service-web/web-sites-deploy.md).

Luettelo tiettyjä malleja, joita tueta Visual C# sekä Visual Basic-kohdassa **Tukee Project-mallit** ohjeaiheen.

Sinun on ensin otettava Azure verkkosovelluksen Visual Studio. Seuraavassa kuvassa näkyy tärkeimmät vaiheet, voit julkaista aiemmin web-sovelluksen lisäämällä Azure projekti, jonka avulla käyttöönottoa varten. Tämä toimenpide lisää Azure projekti, jonka tarvittavien DNS-web-roolin ratkaisu. Web-projekti, joka on tyypin mukaan projektin ominaisuuksien kokoonpanoille päivitetään myös, jos service-paketti edellyttää muita kokoonpanoja käyttöönottoa varten.

![Julkaise Microsoft Azure WWW-sovellus](./media/vs-azure-tools-migrate-publish-web-app-to-cloud-service/IC748917.png)

>[AZURE.NOTE] **Muunna**- **muuntaminen Azure Cloud palvelun Project** -komento näkyy vain ratkaisu web-projektille. Esimerkiksi komento ei ole käytettävissä Silverlight projektin ratkaisu.
Kun luot service-paketin tai julkaista sovelluksesi Azure-varoituksia tai virheitä saattaa ilmetä. Nämä varoitukset ja virheet voi auttaa korjaaminen, ennen kuin otat Azure. Näyttöön voi tulla puuttuu kokoonpanon varoituksen. Lisätietoja Käsittele kaikki varoitukset virheinä käyttämisestä on artikkelissa [Azure Cloud palvelun projektiksi Visual Studiossa määrittäminen](vs-azure-tools-configuring-an-azure-project.md). Jos sovelluksen luominen, suorita se paikallisesti käyttämällä Laske-emulaattorin tai julkaiseminen Azure jotkut **Virheluettelo** -ikkunassa seuraava virhe: **polku, tiedostonimi tai molemmat ovat liian pitkiä**. Tämä virhe ilmenee, koska täydellinen Azure projektin nimi on liian pitkä. Projektinimi, kuten koko polku, pituus voi olla enintään 146 merkkiä. Esimerkiksi Tämä on koko projektinimi, kuten Azure projekti, joka on luotu Silverlight-sovelluksen tiedostopolku: `c:\users\<user name>\documents\visual studio 2015\Projects\SilverlightApplication4\SilverlightApplication4.Web.Azure.ccproj`. Voit joutua ratkaisu siirtäminen toiseen kansioon, jossa on lyhyempi polun Lyhennä täydellinen projektin nimeä.

Voit siirtää ja julkaista Azure verkkosovelluksen Visual Studio, toimimalla seuraavasti.

## <a name="enable-a-web-application-for-deployment-to-azure"></a>Web-sovelluksen käyttöön käyttöönoton Azure

### <a name="to-enable-a-web-application-for-deployment-to-azure"></a>Jos haluat ottaa käyttöön web-sovelluksen käyttöönottoa Azure

1. Web-sovelluksen käyttöönottoa Azure käyttöön web-projekti pikavalikon avaaminen ratkaisu ja valitse Lisää Azure käyttöönoton projekti.

    Seuraavia toimia:

    - Azure projekti nimeltä `<name of the web project>.Azure` lisätään sovelluksen ratkaisu.

    - Web-project web rooli lisätään Azure projektin.

    - **Paikallinen kopio** -ominaisuudeksi on määritetty tosi kokoonpanoille MVC 2, MVC 3 MVC varten tarvittavat 4 ja Silverlight-yrityssovellusten. Tämä lisää nämä kokoonpanot palvelupakettiin, jota käytetään käyttöönottoa varten.

  >[AZURE.IMPORTANT] Jos sinulla on muita kokoonpanoja tai tiedostot, joita tarvitaan tämän verkkosovelluksen, sinun on määritettävä manuaalisesti näiden tiedostojen ominaisuuksia. Lisätietoja näiden ominaisuuksien määrittämisestä on kohdassa **Sisällytä tiedostot palvelupakettiin** tämän artikkelin.

  >[AZURE.NOTE] Jos tietyn web project web rooli on jo Azure Projectissa, **Muunna**-ratkaisussa **Azure Cloud palvelun projektin muuntaminen** ei näy web projektin pikavalikon.

  Jos haluat luoda kunkin web-projektin roolit web WWW-sovelluksessa on useita web-projekteja, on suoritettava vaiheet haluat kunkin web-projekti. Tämä luo erillisen Azure projektin kunkin web-roolin. Kunkin web-projekti voi julkaista erikseen. Vaihtoehtoisesti voit lisätä manuaalisesti toiseen web-roolin aiemmin luotu Azure projekti WWW-sovelluksessa. Voit tehdä tämän Avaa Azure projektin **roolit** -kansion pikavalikko, valitse **Lisää**ja sitten **Ratkaisu Web-roolin projekti**, valitse Lisää WWW-roolissa projekti ja valitse sitten **OK** -painiketta.

## <a name="use-an-azure-sql-database-for-your-application"></a>Käytä sovelluksen Azure SQL-tietokanta

Jos sinulla on yhteysmerkkijonon web-sovelluksen, joka käyttää SQL Server-tietokantaan, joka on tiloissa, sinun on muutettava käyttämään SQL-tietokanta-esiintymän tämän yhteysmerkkijonon Azure isännöivä sijaan.

>[AZURE.IMPORTANT] Tilauksesi on otettava voit käyttää SQL-tietokantaan. Jos käytät [Azure perinteinen portal](http://go.microsoft.com/fwlink/?LinkID=213885)-tilauksen, voit määrittää mitä palveluja tilauksesi sisältää. Seuraavat ohjeet koskevat julkaistu [Azure perinteinen portal](http://go.microsoft.com/fwlink/?LinkID=213885). Jos käytössäsi on [Azure portal](http://portal.microsoft.com), siirry seuraavaan vaiheeseen.

### <a name="to-use-a-sql-database-instance-in-your-web-role-for-your-connection-string"></a>Jotta voit käyttää SQL-tietokanta-esiintymän web-roolin yhteysmerkkijono

1. SQL-tietokannan luominen [Azure perinteinen portal](http://go.microsoft.com/fwlink/?LinkID=213885)-noudata seuraavan artikkelin ohjeita: [Luo SQL-tietokantaan](http://go.microsoft.com/fwlink/?LinkId=225109).

    >[AZURE.NOTE] Kun määrität oman esiintymän SQL-tietokantaan palomuurisäännöt, valitset **Salli Azure muiden käyttää tätä palvelinta** -valintaruutu.

1. Voit luoda SQL-tietokantaan voit käyttää yhteysmerkkijono esiintymä noudattamalla seuraavan osion seuraavan artikkelin ohjeita: [Luo SQL-tietokanta](http://go.microsoft.com/fwlink/?LinkId=225110).

1. Kopioi ADO.NET-yhteysmerkkijono yhteysmerkkijono käytettävät seuraavien toimien [Azure perinteinen portal](http://go.microsoft.com/fwlink/?LinkID=213885).  

  1. Valitse **tietokanta** -painike ja avaa sitten solmu tilauksen, joka oman esiintymän SQL-tietokannan luomiseen.

  1. Näyttää käytettävissä olevat esiintymät SQL-tietokantaan, valitse **SQL-tietokantoja** -solmu.

  1. Näytä tietokannan ominaisuudet-kohdasta haluamasi tietokanta. Näyttöön tulee **Ominaisuudet** -näkymässä.

      >[AZURE.NOTE] Jos **Ominaisuudet** -näkymässä ei näy, voit joutua avata sitä erotinta.

  1. Yhteyden merkkijonot näkyviin valitsemalla Näytä vieressä olevaa kolmea pistettä (…)-painiketta.

    **Yhteysmerkkijonon** -valintaikkuna.

  1. Voit kopioida ADO.NET-yhteysmerkkijonon, Korosta teksti ja valitsemalla Ctrl + C-näppäimiä.

  1. Sulje valintaikkuna valitsemalla **Sulje** -painiketta.

1. Korvaa yhteysmerkkijono-seuraavan koodin korostetut käyttämään tämän esiintymän SQL-tietokanta, Avaa seuraavan koodin korostetut, korosta yhteyden merkkijonon osalta ja valitse sitten Ctrl + V-näppäimiä. SQL-tietokannan esiintymän ADO.NET-yhteysmerkkijonon korvaa aiemmin luotu yhteysmerkkijonon.

1. Sinun on myös lisättävä parametrin `MultipleActiveResultSets=True` yhteysmerkkijonon avulla. Yhteysmerkkijonon pitäisi olla seuraavanlainen:

    ```
    connectionString=”Server=tcp:<database_server>.database.windows.net,1433;Database=<database_name>;User ID=<user_name>@<database_server>;Password=<myPassword>;Trusted_Connection=False;Encrypt=True;MultipleActiveResultSets=True"
    ```

1. (Valinnainen) Vaihtoehtoinen menetelmä muuttamista suoraan seuraavan koodin korostetut-yhteysmerkkijonon on Lisää osa yhdeksi web.config-muunnos-tiedostoja, joiden avulla voit luoda oman palvelupakettiin muodosta asetuksista riippuen. Avaa tiedoston Web.Debug.Config tai Web.Release.Config-tiedosto. Lisää tähän tiedostoon on seuraavassa osassa:

    ```
    XMLCopy<connectionStrings><addname="DefaultConnection"connectionString="Server=tcp:<database_server>.database.windows.net,1433;Database=<database_name>;User ID=<user_name>@<database_server>;Password=<myPassword>;Trusted_Connection=False;Encrypt=True;MultipleActiveResultSets=True"xdt:Transform="SetAttributes"xdt:Locator="Match(name)"/></connectionStrings>
    ```

1. Tallenna tiedosto, joita olet muokannut ja julkaise sovellus.

### <a name="to-use-an-instance-of-sql-database-by-using-the-azure-classic-portal"></a>SQL-tietokannan esiintymä otetaan käyttöön perinteinen Azure-portaalissa

1. Valitse [Azure perinteinen portal](http://go.microsoft.com/fwlink/?LinkID=213885)SQL-tietokantoja-solmu.

  - Jos SQL-tietokanta, jota haluat käyttää esiintymä tulee näkyviin, valitse Avaa.

  - Jos et ole luonut kaikki esiintymät, valitse haluamasi linkki ja luo erillisen.

1. Kun avaat tai luot tietokannan esiintymän, valitse **Yhteysmerkkijonon** -linkki.

1. Sivun alareunassa valitsemalla palomuurin asetusten määrittäminen ja hyväksy oletusarvot tai määrittää arvot, joita tarvitset linkki.

1. Kopioi ADO.NET-yhteysmerkkijono ja liittämällä sen web.config-tiedoston vanha yhteysmerkkijonon paikallisen tietokannan kautta muista lisätä `MultipleActiveResultSets=True`.

## <a name="publish-a-web-application-to-azure"></a>Julkaise Azure WWW-sovellus

### <a name="to-publish-a-web-application-to-azure"></a>Jos haluat julkaista Azure WWW-sovellus

1. Voit esikatsella sovelluksen paikallista kehittämistä käyttämällä Azure ympäristön Laske emulaattorin, Avaa Azure project web-roolin pikavalikon ja valitse **Aseta projektin aloitus**. Valitse sitten **Virheenkorjaus**, **Käynnistä virheenkorjaus** (näppäimistön: **F5-näppäintä**).

    **Käynnistä ympäristön Azure virheenkorjaus** -valintaikkuna avautuu ja sovellus käynnistyy selaimessa. Lisätietoja erityyppisiin web-sovelluksen käynnistäminen Laske emulaattori kohdassa taulukon sisältö.

1. Sovelluksen julkaiseminen Azure-palvelujen määrittämiseen, tarvitset Microsoft-tili ja Azure tilauksen. Palvelujen määrittäminen seuraavassa aiheessa ohjeiden avulla: [valmisteleminen, julkaiseminen tai Visual Studio Azure sovelluksen ottaminen käyttöön](vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md).

1. Julkaisemaan Azure verkkosovelluksen Avaa pikavalikon web-projekti ja valitse **Julkaise Azure**.

    **Julkaise Azure-sovelluksen** -valintaikkuna avautuu ja Visual Studio käynnistyy käyttöönottoprosessin. Lisätietoja siitä, miten voit julkaista sovellus on kohdassa julkaisu- [Työkaluilla Azure pilvipalveluun](vs-azure-tools-publishing-a-cloud-service.md) **Azure-sovelluksen Visual Studio julkaiseminen** .

    >[AZURE.NOTE] Voit myös julkaista web-sovelluksen Azure projektista. Voit tehdä tämän Avaa pikavalikko Azure projektin ja sitten **Julkaise**.

1. Käyttöönoton etenemisen näkyviin voit tarkastella **Azure tehtävän loki** -ikkunaan. Lokia näytetään automaattisesti, kun käyttöönottoprosessin käynnistyy. Voit laajentaa rivin kohteen tehtävän lokissa näyttämään on seuraavassa kuvassa esitetyllä tavalla:

    ![VST_AzureActivityLog](./media/vs-azure-tools-migrate-publish-web-app-to-cloud-service/IC744149.png)

1. (Valinnainen) Voit peruuttaa käyttöönottoprosessin, viiva-kohteen pikavalikon avaaminen tehtävän lokiin ja valitsemalla **Peruuta ja poista**. Tämä lopettaa käyttöönottoprosessin ja poistaa Azure käyttöönotto-ympäristössä.

    >[AZURE.NOTE] Jos haluat poistaa käyttöönotto-ympäristössä, sen jälkeen, kun se on otettu käyttöön, sinun on käytettävä [Azure perinteinen portal](http://go.microsoft.com/fwlink/?LinkID=213885).

1. (Valinnainen) Kun rooli esiintymät on jo alkanut, Visual Studio näkyy automaattisesti käyttöönotto-ympäristön **Azure Laske** -solmu **Cloud Explorer** tai **Palvelimen Resurssienhallinnassa**. Täällä voit tarkastella yksittäisiä roolin esiintymien tila.

    Seuraavassa kuvassa näkyy **Palvelimen Explorer** roolin esiintymät, silloin, kun ne ovat edelleen alustaminen onnistuu tila:

    ![VST_DeployComputeNode](./media/vs-azure-tools-migrate-publish-web-app-to-cloud-service/IC744134.png)

1. Käyttää sovelluksen käyttöönoton jälkeen, valitse käyttöönoton vieressä olevaa nuolta, kun tila on **Valmis** näkyy **Azure tehtävän log**. Web-sovelluksen URL-osoite näkyy Azure. Katso lisätietoja siitä, miten tietynlaisia web-sovelluksen käynnistäminen Azure seuraavassa taulukossa.

    Seuraavassa taulukossa lisätietoja tietyn verkkosovellusten käynnistäminen Azure tai suorittaa tai paikallisesti käyttämällä Azure Laske emulaattorin web-sovelluksen korjaaminen:

  	|Web-sovelluksen tyyppi|Suorita/virheenkorjaus paikallisesti käyttämällä Laske-emulaattorin|Azure käynnissä|
  	|---|---|---|
  	|ASP.NET Web-sovelluksen|Valitse valikkoriviltä **Virheenkorjaus**, **Käynnistä virheenkorjaus** (näppäimistön: Valitse **F5** -näppäintä.).|Valitse Lataa aloitussivu selaimeen **Azure tehtävän log** **käyttöönotto** -välilehdessä näkyvien URL-osoite hyperlinkki.|
  	|ASP.NET-MVC 2-verkkosovellus|Valitse valikkoriviltä **Virheenkorjaus**, **Käynnistä virheenkorjaus** (näppäimistön: Valitse **F5** -näppäintä.).|Valitse Lataa aloitussivu selaimeen **Azure tehtävän log** **käyttöönotto** -välilehdessä näkyvien URL-osoite hyperlinkki.|
  	|ASP.NET-MVC 3-verkkosovellus|Valitse valikkoriviltä **Virheenkorjaus**, **Käynnistä virheenkorjaus** (näppäimistön: Valitse **F5** -näppäintä.).|Valitse Lataa aloitussivu selaimeen **Azure tehtävän log** **käyttöönotto** -välilehdessä näkyvien URL-osoite hyperlinkki.|
  	|ASP.NET-MVC 4-verkkosovellus|Valitse valikkoriviltä **Virheenkorjaus**, **Käynnistä virheenkorjaus** (näppäimistön: Valitse **F5** -näppäintä.).|Valitse Lataa aloitussivu selaimeen **Azure tehtävän log** **käyttöönotto** -välilehdessä näkyvien URL-osoite hyperlinkki.|
  	|Tyhjennä ASP.NET Web-sovelluksen|Sinun on lisättävä .aspx-sivu, jotka määrität aloitussivun projektin web-sovelluksessa. Valitse valikkoriviltä **Virheenkorjaus**, **Käynnistä virheenkorjaus** (näppäimistön: Valitse **F5** -näppäintä.).|Jos sovellus on oletusarvoinen .aspx-sivu, valitse **käyttöönotto** -välilehdessä näkyvien **Azure tehtävän log** ja sivun URL-osoite hyperlinkki on ladattu selaimessa. Jos sinulla on eri .aspx-sivu, sinun on tätä URL-osoite seuraavassa muodossa tietylle sivulle siirtyminen:`<url for deployment>/<name of page>.aspx`|
  	|Silverlight-sovelluksen|Valitse valikkoriviltä **Virheenkorjaus**, **Käynnistä virheenkorjaus** (näppäimistön: Valitse **F5** -näppäintä.).|Sinun täytyy siirtyä tietylle sivulle muodossa seuraava URL-sovelluksen:`<url for deployment>/<name of page>.aspx`|
  	|Silverlight Business-sovelluksen|Valitse valikkoriviltä **Virheenkorjaus**, **Käynnistä virheenkorjaus** (näppäimistön: Valitse **F5** -näppäintä.).|Sinun täytyy siirtyä tietylle sivulle muodossa seuraava URL-sovelluksen:`<url for deployment>/<name of page>.aspx`|
  	|Siirtyminen Silverlight-sovelluksen|Valitse valikkoriviltä **Virheenkorjaus**, **Käynnistä virheenkorjaus** (näppäimistön: Valitse **F5** -näppäintä.).|Sinun täytyy siirtyä tietylle sivulle muodossa seuraava URL-sovelluksen:`<url for deployment>/<name of page>.aspx`|
  	|WCF-palvelusovellus|WCF-palveluun projektin sinun on määritettävä .svc-tiedostossa aloitus-sivulla. Valitse valikkoriviltä **Virheenkorjaus**, **Käynnistä virheenkorjaus** (näppäimistön: Valitse **F5** -näppäintä.).|Tarvitset seuraavassa muodossa URL-sovelluksen svc-tiedosto:`<url for deployment>/<name of service file>.svc`|
  	|WCF-työnkulun palvelusovelluksen|WCF-palveluun projektin sinun on määritettävä .svc-tiedostossa aloitus-sivulla. Valitse valikkoriviltä **Virheenkorjaus**, **Käynnistä virheenkorjaus** (näppäimistön: Valitse **F5** -näppäintä.).|Tarvitset seuraavassa muodossa URL-sovelluksen svc-tiedosto:`<url for deployment>/<name of service file>.svc`|
  	|ASP.NET-dynaaminen yksiköt|Valitse valikkoriviltä **Virheenkorjaus**, **Käynnistä virheenkorjaus** (näppäimistön: Valitse **F5** -näppäintä.).|Sinun on päivitettävä yhteysmerkkijonon (katso seuraava kohta). Sinun on myös Siirry tietylle sivulle muodossa seuraava URL-sovelluksen:`<url for deployment>/<name of page>.aspx`|
  	|ASP.NET Dynamic Data Linq SQL|Valitse valikkoriviltä **Virheenkorjaus**, **Käynnistä virheenkorjaus** (näppäimistön: Valitse **F5** -näppäintä.).|Sinun on noudatettava vaiheet: Käytä SQL Azure-tietokannan sovelluksen (katso tämän ohjeaiheen aiemmassa kohdassa). Sinun on myös Siirry tietylle sivulle muodossa seuraava URL-sovelluksen:`<url for deployment>/<name of page>.aspx`|

## <a name="update-a-connection-string-for-aspnet-dynamic-entities"></a>Päivitä yhteysmerkkijonon ASP.NET dynaaminen kohteisiin

### <a name="to-update-a-connection-string-for-aspnet-dynamic-entities"></a>Voit päivittää yhteysmerkkijonon ASP.NET dynaaminen yksiköt

1. Voit luoda SQL Azure-tietokanta, jonka avulla voidaan ASP.NET dynaaminen kohteiden web-sovelluksen noudattamalla tämän ohjeaiheen **sovelluksen SQL Azure-tietokannan** kuvattuja ohjeita.

1. Lisää haluamasi taulukot ja kentät, jotka tarvitset tietokannalle [Azure perinteinen portal](http://go.microsoft.com/fwlink/?LinkID=213885).

1. Tällaista sovelluksen yhteysmerkkijono on seuraavan koodin korostetut seuraavanlainen:  

    ```
    <addname="tempdbEntities"connectionString="metadata=res://*/Model1.csdl|res://*/Model1.ssdl|res://*/Model1.msl;provider=System.Data.SqlClient;provider connection string=&quot;data source=<server name>\SQLEXPRESS;initial catalog=<database name>;integrated security=True;multipleactiveresultsets=True;App=EntityFramework&quot;"providerName="System.Data.EntityClient"/>
    ```

    Päivitä *connectionString* arvo SQL Azure-tietokannassa ADO.NET-yhteysmerkkijonon seuraavasti:

    ```
    XMLCopy<addname="tempdbEntities"connectionString="metadata=res://*/Model1.csdl|res://*/Model1.ssdl|res://*/Model1.msl;provider=System.Data.SqlClient;provider connection string=&quot;Server=tcp:<SQL Azure server name>.database.windows.net,1433;Database=<database name>;User ID=<user name>;Password=<password>;Trusted_Connection=False;Encrypt=True;multipleactiveresultsets=True;App=EntityFramework&quot;"providerName="System.Data.EntityClient"/>
    ```

1. Tallenna seuraavan koodin korostetut, yhteysmerkkijono-valikossa tekemäsi muutokset valitsemalla **Tiedosto**, **Tallenna web.config**.

## <a name="supported-project-templates"></a>Tuetut Project-mallit

Voit julkaista web-sovelluksen Azure-sovellus on jollakin project-mallit C# tai Visual Basic, jotka on lueteltu alla olevassa taulukossa.

|Mallin projektiryhmän|Projektimalli|
|---|---|
|WWW|ASP.NET Web-sovelluksen|
|WWW|ASP.NET-MVC 2-verkkosovellus|
|WWW|ASP.NET-MVC 3-verkkosovellus|
|WWW|ASP.NET-MVC4 verkkosovelluksen|
|WWW|Tyhjennä ASP.NET Web-sovelluksen|
|WWW|ASP.NET-MVC 2 tyhjä Web-sovelluksen|
|WWW|ASP.NET Dynamic Data kohteiden WWW-sovellus|
|WWW|ASP.NET Dynamic Data Linq SQL-Web-sovellukseen|
|Silverlight|Silverlight-sovelluksen|
|Silverlight|Silverlight Business-sovelluksen|
|Silverlight|Siirtyminen Silverlight-sovelluksen|
|WCF|WCF-palvelusovellus|
|WCF|WCF-työnkulun palvelusovelluksen|
|Työnkulun|WCF-työnkulun palvelusovelluksen|

## <a name="next-steps"></a>Seuraavat vaiheet
Lisätietoja julkaiseminen on artikkelissa [Julkaise tai Visual Studio Azure-sovelluksen käyttöönotto](vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md). Tutustu myös [Asetus määrittäminen nimeltä todentamisen tunnistetiedoilla](vs-azure-tools-setting-up-named-authentication-credentials.md).
