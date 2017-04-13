<properties
    pageTitle="Luo ja yhteyden muodostaminen MySQL-tietokantaan Azure-tietokannassa"
    description="Opettele Azure portaalin avulla voit luoda MySQL-tietokantaan ja liitä siihen PHP web Appista, Azure-tietokannassa."
    documentationCenter="php"
    services="app-service\web"
    authors="cephalin"
    manager="wpickett"
    editor=""
    tags="mysql"/>

<tags
    ms.service="multiple"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm;cephalin"/>

# <a name="create-and-connect-to-a-mysql-database-in-azure"></a>Luo ja yhteyden muodostaminen MySQL-tietokantaan Azure-tietokannassa

Tässä opetusohjelmassa näytetään, miten voit luoda MySQL-tietokantaan [Azure portal](https://portal.azure.com) (provider on [ClearDB](http://www.cleardb.com/)) ja muodostaa siihen PHP web Appista- [Sovelluksen Azure-palvelu](./app-service/app-service-value-prop-what-is.md)käytössä. 

> [AZURE.NOTE] Voit myös luoda [Marketplace-sovelluksen malli](./app-service-web/app-service-web-create-web-app-from-marketplace.md)osana MySQL-tietokantaan.

## <a name="create-a-mysql-database-in-azure-portal"></a>Luo MySQL-tietokantaan Azure-portaalissa

Jos haluat luoda MySQL-tietokantaan Azure-portaalissa, toimi seuraavasti:

1. Kirjaudu sisään [Azure portal](https://portal.azure.com).

2. Valitse vasemmanpuoleisessa valikossa **Uusi** > **tietojen + tallennustilan** > **MySQL-tietokantaan**.

    ![Luo Azure - Käynnistä MySQL-tietokantaan](./media/store-php-create-mysql-database/create-db-1-start.png)

2. Uusi MySQL-tietokanta- [sivu](azure-portal-overview.md), valitse Määritä uusi MySQL-tietokantaan seuraavasti (*sivu*: portaalisivu, joka avaa vaakasuunnassa):

    - **Tietokannan nimi**: Kirjoita tunnistettavissa yksilöivästi nimi
    - **Tilaus**: Valitse käyttämään tilaus
    - **Tietokannan tyyppi**: Valitse **jaettu** edullinen tai vapaan tasoa varten tai **erityisvarastopaikka** Hae erillinen resurssit. 
    - **Resurssiryhmä**: lisääminen aiemmin luotuun [resurssiryhmä](../azure-resource-manager/resource-group-overview.md) MySQL-tietokantaan tai valitse uusi. Resurssien saman ryhmän helposti hallittavissa yhdessä.
    - **Sijainti**: Voit lähellä sijainti. Kun lisäät resurssi-ryhmään, on lukittu resurssiryhmä-kohtaan.
    - **Hinnat taso**: **Hinnat taso**ja valitse hinnoitteluista vaihtoehto (**elohopeaa** taso on ilmainen), ja valitse sitten **Valitse**. 
    - **Ehdot**: Valitse **Ehdot**, tarkista oston tiedot ja valitse **Osta**.
    - **Raporttinäkymät-ikkunan kiinnittäminen**: Valitse tämä, jos haluat käyttää suoraan koontinäyttö. Tämä on erityisen hyödyllinen, jos et ole vielä portaalin siirtyminen tutustunut.
    
    Seuraava kuva on vain esimerkki siitä, miten voit määrittää MySQL-tietokantaan.  
    ![MySQL-tietokantaan luominen Azure - asetusten määrittäminen](./media/store-php-create-mysql-database/create-db-2-configure.png)

3. Kun olet valmis määrittäminen, valitse **Luo**.

    ![Luo MySQL-tietokantaan Azure - Luo](./media/store-php-create-mysql-database/create-db-3-create.png)

    Näkyviin tulee ponnahdusilmoituksen-salliminen tiedät, että käyttöönoton on alkanut.

    ![MySQL-tietokantaan luominen Azure - käynnissä](./media/store-php-create-mysql-database/create-db-4-started-status.png)

    Näkyviin tulee toinen ponnahdusikkunoiden kun käyttöönotto onnistui. Portaalin myös avata MySQL-tietokantaan sivu automaattisesti.

<a name="connect"></a>
## <a name="connect-to-your-mysql-database"></a>Yhteyden muodostaminen MySQL-tietokantaan

Voit tarkastella yhteystietoja uuteen MySQL-tietokantaan, valitse **Ominaisuudet** -web-sovelluksen sivu.
    
![MySQL-tietokantaan luominen Azure - MySQL-tietokanta-sivu](./media/store-php-create-mysql-database/create-db-5-finished-db-blade.png)

Voit nyt yhteyden tiedot minkä tahansa online-sovelluksessa. Malli, jossa näkyvät yhteystiedot yksinkertainen PHP-sovelluksen käyttämisestä on saatavilla [tähän](https://github.com/WindowsAzure/azure-sdk-for-php-samples/tree/master/tasklist-mysql).

## <a name="connect-a-laravel-web-app-from-the-php-get-started-tutorial"></a>Yhdistä Laravel verkkosovellukseen (mistä PHP get Aloittaminen opetusohjelman)

Oletetaan, että juuri päättynyt opetusohjelman [luominen, Määritä, ja ota PHP verkkosovellukseen Azure,](./app-service-web/app-service-web-php-get-started.md) ja on käynnissä Azure [Laravel](https://www.laravel.com/) verkkosovellukseen. Voit lisätä tietokantaan ominaisuudet helposti Laravel-sovellukseen. Noudata seuraavia ohjeita:

>[AZURE.NOTE] Seuraavissa vaiheissa oletetaan, että olet opetusohjelman [luominen, määritys ja PHP verkkosovellukseen Azure voit ottaa käyttöön](./app-service-web/app-service-web-php-get-started.md).

1. Määrittää Laravel sovelluksen paikallisen kehitysympäristö osoittamaan MySQL-tietokantaan. Voit tehdä tämän Avaa `.env` Laravel-sovelluksen pääkansio ja määritä MySQL-tietokanta-asetukset.

        DB_CONNECTION=mysql
        DB_HOST=<HOSTNAME_from_properties_blade>
        DB_PORT=<PORT_from_properties_blade>
        DB_DATABASE=<see_note_below>
        DB_USERNAME=<USERNAME_from_properties_blade>
        DB_PASSWORD=<PASSWORD_from_properties_blade>

    >[AZURE.NOTE] **Ominaisuudet** -sivu MySQL-tietokantaan nimi voi tai ei ehkä ole näkyvä **TIETOKANNAN nimi** -kenttään. Kannattaa tarkistaa tietokannan parametrin **YHTEYSMERKKIJONO** -kenttään. 
    >
    >![MySQL-tietokantaan luominen Azure - käynnissä](./media/store-php-create-mysql-database/connect-db-1-database-name.png)

2. Varmista, että sinulla on MySQL käyttöoikeudet nyt nopein tapa on käyttämään [Laravel on oletusarvoinen todennus rakennustelineet](https://laravel.com/docs/5.2/authentication#authentication-quickstart). Suorita seuraavat komennot komentorivin pääte Laravel-sovelluksen pääkansiosta:

         php artisan migrate
         php artisan make:auth

    Ensimmäisen komennon Luo taulukoiden perusteella ennalta määritettyjä siirrot Azure `database/migrations` hakemistosta ja toinen komento käsittelynä basic näkymät ja ohjaavat käyttäjän rekisteröinti ja todennusta varten.

3. Suorita kehittäminen palvelin:

        php artisan serve

4. Avaa selaimessa http://localhost:8000 ja rekisteröi uusi käyttäjä seuraavasti:

    ![Yhteyden muodostaminen MySQL-tietokantaan Azure - käyttäjän rekisteröiminen](./media/store-php-create-mysql-database/connect-db-2-development-server.png)

    Noudata valmis Käyttöliittymän kehotteen rekisteröinnin. Rekisteröinti on valmis, kun kirjaudut.
    
    ![Yhteyden muodostaminen MySQL-tietokantaan Azure - käyttäjän rekisteröiminen](./media/store-php-create-mysql-database/connect-db-3-registered-user.png)

    Voit nyt kehittävät sovelluksen vastaan Azure MySQL-tietokantaan.

5. Seuraavaksi haluat vain replikoida oman `.env` Azure web app-asetukset. Suorita seuraavat Azure CLI-komennot:

        azure site appsetting add DB_CONNECTION=mysql
        azure site appsetting add DB_HOST=<HOSTNAME_from_properties_blade>
        azure site appsetting add DB_PORT=<PORT_from_properties_blade>
        azure site appsetting add DB_DATABASE=<Database_param_from_CONNECTION_INFO_from_properties_blade>
        azure site appsetting add DB_USERNAME=<USERNAME_from_properties_blade>
        azure site appsetting add DB_PASSWORD=<PASSWORD_from_properties_blade>

    Katso, miten tämä toimii [Määritä Azure web Appissa](./app-service-web/app-service-web-php-get-started.md#configure).

6. Vahvista seuraavaksi ja siirtää Azure paikallisen muutoksia aiemmin suorituksen aikana `php artisan make:auth`.

        git add .
        git commit -m "scaffold auth views and routes"
        git push azure master

7. Siirry Azure online.

        azure site browse

8. Kirjaudu sisään aiemmin luomasi käyttäjän tunnistetietoja.

    ![Yhteyden muodostaminen MySQL-tietokantaan Azure - Siirry Azure web Appissa](./media/store-php-create-mysql-database/connect-db-4-browse-azure-webapp.png)

    Kirjauduttuasi sisään pitäisi näkyä helpossa muodossa jälkeinen kirjautumisnäyttö.
    
    ![Yhteyden muodostaminen MySQL-tietokantaan Azure - kirjautunut sisään](./media/store-php-create-mysql-database/connect-db-5-logged-in.png)

    Onnittelut, PHP koodiin Azure-tietokannassa on nyt tietojen käyttäminen MySQL-tietokannasta. 

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja on artikkelissa [PHP Developer Center](/develop/php/).
