<properties 
    pageTitle="PHP MySQL-web-sovelluksen luominen Azure App palvelun ja ota käyttöön FTP: N avulla" 
    description="Opetusohjelma, joka näytetään, miten, joka tallentaa tiedot MySQL ja käytä FTP käyttöönoton Azure PHP web-sovelluksen luominen." 
    services="app-service\web" 
    documentationCenter="php" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="PHP" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>


#<a name="create-a-php-mysql-web-app-in-azure-app-service-and-deploy-using-ftp"></a>PHP MySQL-web-sovelluksen luominen Azure App palvelun ja ota käyttöön FTP: N avulla

Tässä opetusohjelmassa näytetään, miten PHP MySQL-web-sovelluksen luominen ja sen FTP ottamisesta. Tässä opetusohjelmassa oletetaan, että [PHP][install-php], [MySQL][install-mysql]-web-palvelin ja FTP-asiakas, joka on asennettu tietokoneeseen. Tässä opetusohjelmassa ohjeita voit seurata sellaisille käyttöjärjestelmille, kuten Windows, Mac tai Linux. Kun olet suorittanut tämän oppaan, on käynnissä Azure PHP/MySQL verkkosovellukseen.
 
Opit seuraavat asiat:

* Voit luoda verkkosovellukseen ja Azure-portaalissa MySQL-tietokantaan. Koska PHP on oletusarvoisesti käytössä Web Apps-sovelluksissa, mitään erityistä tarvitaan PHP koodin suorittaminen.
* Voit julkaista sovelluksesi Azure FTP: N avulla.
 
Noudattamalla tässä opetusohjelmassa, voit luoda yksinkertaisen rekisteröinti verkkosovellukseen PHP. Sovelluksen sovelluksessa verkkosovellukseen. Näyttökuva valmiin sovelluksen on alla:

![Azure PHP Web-sivusto][running-app]

>[AZURE.NOTE] Jos haluat aloittaa Azure App palvelun ennen tilin käyttäjäksi, siirry [Yritä App palvelu](http://go.microsoft.com/fwlink/?LinkId=523751), jossa lyhytkestoinen starter verkkosovellukseen heti voit luoda sovelluksen-palvelussa. Ei ole luottokortit pakollinen, ei ole sitoumukset. 


##<a name="create-a-web-app-and-set-up-ftp-publishing"></a>Web-sovelluksen luominen ja julkaiseminen FTP määrittäminen

Voit luoda verkkosovellukseen ja MySQL-tietokantaan seuraavasti:

1. Kirjautuminen [Azure Portal][management-portal].
2. Napsauta vasemmassa yläkulmassa **+ Uusi** kuvake Azure-portaalissa.

    ![Luo uusi Azure Web-sivusto][new-website]

3. Kirjoita Etsi **Online- + MySQL** ja valitse sitten **Web Appin + MySQL**.

    ![Mukautetun Luo uuden sivuston][custom-create]

4. Valitse **Luo**. Kirjoita yksilöllisen sovelluksen palvelunimi, resurssiryhmän kelvollinen nimi uusi palvelu suunnitelma.

    ![Määritä resurssiryhmän nimi][resource-group]


6. Kirjoita uuden tietokannan, mukaan lukien siitä ehdot arvot.

    ![Luo uusi MySQL-tietokanta][new-mysql-db]
    
7. Web-sovellus on luotu, näet uuden sovelluksen service-sivu.


6. Valitse **asetukset** > **käyttöönoton tunnistetiedot**. 

    ![Määritä käyttöönoton tunnistetiedot][set-deployment-credentials]

7. Jotta FTP-julkaisemista, sinun on määritettävä käyttäjänimi ja salasana. Tallenna tunnistetiedot ja Merkitse muistiin käyttäjänimi ja salasana, voit luoda.

    ![Julkaisun tunnistetietojen][portal-ftp-username-password]

##<a name="build-and-test-your-app-locally"></a>Muodosta ja testaa sovelluksen paikallisesti

Rekisteröinti-sovellus on yksinkertainen PHP-sovellus, jonka avulla voit rekisteröidä tapahtuman antamalla nimen ja sähköpostiosoitteen. Tietoja edellisen Ilmoittautuneita näkyy taulukon. Rekisteröintitietoja tallennetaan MySQL-tietokantaan. Sovelluksen kuuluu kaksi tiedostoa:

* **Index.php**: Näyttää lomakkeen rekisteröimiseen ja rekisteröijän tiedot sisältävä taulukko.
* **createtable.php**: Luo sovelluksen MySQL-taulukon. Tätä tiedostoa käytetään vain kerran.

Voit luoda ja suorita sovellus paikallisesti, noudata seuraavia ohjeita. Huomaa, että seuraavissa vaiheissa oletetaan, sinulla on PHP MySQL- ja verkkopalvelimeen, Määritä paikallisessa tietokoneessa ja joihin sinulla on käytössä [San MySQL-laajennus][pdo-mysql].

1. Luo MySQL-tietokantaan, jota kutsutaan `registration`. Voit tehdä tämän MySQL komentokehotteen tällä komennolla:

        mysql> create database registration;

2. Web-palvelimesi pääkansiosta, Luo kansio nimeltä `registration` ja luo kaksi tiedostoa ei - jokin kutsutaan `createtable.php` ja yksi kutsutaan `index.php`.

3. Avaa `createtable.php` tiedoston tekstieditorissa tai IDE ja lisää seuraava koodi. Tämä koodi käytetään luomaan `registration_tbl` taulukkoon `registration` tietokannan.

        <?php
        // DB connection info
        $host = "localhost";
        $user = "user name";
        $pwd = "password";
        $db = "registration";
        try{
            $conn = new PDO( "mysql:host=$host;dbname=$db", $user, $pwd);
            $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );
            $sql = "CREATE TABLE registration_tbl(
                        id INT NOT NULL AUTO_INCREMENT, 
                        PRIMARY KEY(id),
                        name VARCHAR(30),
                        email VARCHAR(30),
                        date DATE)";
            $conn->query($sql);
        }
        catch(Exception $e){
            die(print_r($e));
        }
        echo "<h3>Table created.</h3>";
        ?>

    > [AZURE.NOTE] 
    > Sinun on päivitettävä arvot <code>$user</code> ja <code>$pwd</code> paikallisen MySQL-käyttäjänimi ja salasana.

4. Avaa selain ja siirry [http://localhost/registration/createtable.php][localhost-createtable]. Tämä vaihtoehto luo `registration_tbl` tietokannan taulukkoon.

5. Avaa **index.php** tiedostoa tekstieditorissa tai IDE ja lisää basic HTML- ja CSS-koodin (PHP koodi lisätään myöhemmin vaiheet)-sivulle.

        <html>
        <head>
        <Title>Registration Form</Title>
        <style type="text/css">
            body { background-color: #fff; border-top: solid 10px #000;
                color: #333; font-size: .85em; margin: 20; padding: 20;
                font-family: "Segoe UI", Verdana, Helvetica, Sans-Serif;
            }
            h1, h2, h3,{ color: #000; margin-bottom: 0; padding-bottom: 0; }
            h1 { font-size: 2em; }
            h2 { font-size: 1.75em; }
            h3 { font-size: 1.2em; }
            table { margin-top: 0.75em; }
            th { font-size: 1.2em; text-align: left; border: none; padding-left: 0; }
            td { padding: 0.25em 2em 0.25em 0em; border: 0 none; }
        </style>
        </head>
        <body>
        <h1>Register here!</h1>
        <p>Fill in your name and email address, then click <strong>Submit</strong> to register.</p>
        <form method="post" action="index.php" enctype="multipart/form-data" >
              Name  <input type="text" name="name" id="name"/></br>
              Email <input type="text" name="email" id="email"/></br>
              <input type="submit" name="submit" value="Submit" />
        </form>
        <?php

        ?>
        </body>
        </html>

6. Lisää PHP koodi tietokantayhteyden muodostamisessa PHP, tunnisteiden sisällä.

        // DB connection info
        $host = "localhost";
        $user = "user name";
        $pwd = "password";
        $db = "registration";
        // Connect to database.
        try {
            $conn = new PDO( "mysql:host=$host;dbname=$db", $user, $pwd);
            $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );
        }
        catch(Exception $e){
            die(var_dump($e));
        }

    > [AZURE.NOTE]
    > Uudelleen, sinun on päivitettävä arvot <code>$user</code> ja <code>$pwd</code> paikallisen MySQL-käyttäjänimi ja salasana.

7. Seurattavat kohteet-tietokannan yhteys-koodin lisääminen koodin lisääminen rekisteröintitietoja tietokantaan.

        if(!empty($_POST)) {
        try {
            $name = $_POST['name'];
            $email = $_POST['email'];
            $date = date("Y-m-d");
            // Insert data
            $sql_insert = "INSERT INTO registration_tbl (name, email, date) 
                           VALUES (?,?,?)";
            $stmt = $conn->prepare($sql_insert);
            $stmt->bindValue(1, $name);
            $stmt->bindValue(2, $email);
            $stmt->bindValue(3, $date);
            $stmt->execute();
        }
        catch(Exception $e) {
            die(var_dump($e));
        }
        echo "<h3>Your're registered!</h3>";
        }

8. Lopuksi seuraavat yllä olevan koodin lisääminen koodi tietojen noutaminen tietokannasta.

        $sql_select = "SELECT * FROM registration_tbl";
        $stmt = $conn->query($sql_select);
        $registrants = $stmt->fetchAll(); 
        if(count($registrants) > 0) {
            echo "<h2>People who are registered:</h2>";
            echo "<table>";
            echo "<tr><th>Name</th>";
            echo "<th>Email</th>";
            echo "<th>Date</th></tr>";
            foreach($registrants as $registrant) {
                echo "<tr><td>".$registrant['name']."</td>";
                echo "<td>".$registrant['email']."</td>";
                echo "<td>".$registrant['date']."</td></tr>";
            }
            echo "</table>";
        } else {
            echo "<h3>No one is currently registered.</h3>";
        }

Voit nyt selata [http://localhost/registration/index.php] [ localhost-index] Testaa sovellus.

##<a name="get-mysql-and-ftp-connection-information"></a>MySQL- ja FTP-yhteystiedot

Yhteyden muodostaminen MySQL-tietokantaan, jossa on käytössä Web Apps-kohdassa näkyy on yhteystiedot. Saat MySQL-yhteystietoja, toimi seuraavasti:

1. Sovelluksen-palvelusta web app-sivu napsauttamalla resurssien ryhmä-linkin:

    ![Valitse resurssi-ryhmä][select-resourcegroup]

1. Valitse resurssi-ryhmässä tietokannan:

    ![Valitse tietokanta][select-database]

2. Yhteenveto tietokannasta, valitse **asetukset** > **Ominaisuudet**.

    ![Valitse ominaisuudet][select-properties]
    
2. Kirjoita muistiin arvojen `Database`, `Host`, `User Id`, ja `Password`.

    ![Huomautus-ominaisuudet][note-properties]

3. Verkkosovelluksen Valitse sivun oikeassa alakulmassa olevaa **Lataa Julkaise profiili** -linkkiä:

    ![Lataa Julkaise profiili][download-publish-profile]

4. Avaa `.publishsettings` tiedoston XML-editoriin. 

3. Etsi `<publishProfile >` elementti, jonka `publishMethod="FTP"` , joka näyttää seuraavankaltaiselta:

        <publishProfile publishMethod="FTP" publishUrl="ftp://[mysite].azurewebsites.net/site/wwwroot" ftpPassiveMode="True" userName="[username]" userPWD="[password]" destinationAppUrl="http://[name].antdf0.antares-test.windows-int.net" 
            ...
        </publishProfile>
    
Kirjoita muistiin `publishUrl`, `userName`, ja `userPWD` määritteet.

##<a name="publish-your-app"></a>Sovelluksen julkaiseminen

Kun olet testannut sovelluksen paikallisesti, voit julkaista koodiin FTP: N avulla. Sinun on päivitettävä tietokannan yhteystiedot-sovelluksessa. Olet hankkinut yhteystiedot aiemmin ( **Hae MySQL- ja FTP-yhteystiedot** -osan avulla), Päivitä seuraavat tiedot **sekä** `createdatabase.php` ja `index.php` tiedostoja, joissa kyseiset arvot:

    // DB connection info
    $host = "value of Data Source";
    $user = "value of User Id";
    $pwd = "value of Password";
    $db = "value of Database";

Kun olet valmis julkaisemaan sovelluksen FTP: N avulla.

1. Avaa FTP-asiakas, valinta.

2. Määritä *Host (isäntä)-osaan, nimi* - `publishUrl` määrite merkitsemäsi yläpuolella kyselyjä FTP-asiakas.

3. Kirjoita `userName` ja `userPWD` ole mainittu edellä määritteet muuttamatta kyselyjä FTP-asiakas.

4. Muodosta yhteys.

Kun olet yhdistänyt osaat lataaminen ja ladata tiedostoja haluamallasi tavalla. Varmista, että lataat tiedostot pääkansion, joka on `/site/wwwroot`.

Ladattuasi molemmat `index.php` ja `createtable.php`, siirry **http://[site name].azurewebsites.net/createtable.php** sovelluksen MySQL-taulukon luominen ja valitse Siirry **http://[site name].azurewebsites.net/index.php** , kun haluat käyttää sovellusta.
 
## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja on artikkelissa [PHP Developer Center](/develop/php/).

[install-php]: http://www.php.net/manual/en/install.php
[install-mysql]: http://dev.mysql.com/doc/refman/5.6/en/installing.html
[pdo-mysql]: http://www.php.net/manual/en/ref.pdo-mysql.php
[localhost-createtable]: http://localhost/tasklist/createtable.php
[localhost-index]: http://localhost/tasklist/index.php
[running-app]: ./media/web-sites-php-mysql-deploy-use-ftp/running_app_2.png
[new-website]: ./media/web-sites-php-mysql-deploy-use-ftp/new_website2.png
[custom-create]: ./media/web-sites-php-mysql-deploy-use-ftp/create_web_mysql.png
[website-details]: ./media/web-sites-php-web-site-mysql-deploy-use-ftp/website_details.jpg
[new-mysql-db]: ./media/web-sites-php-mysql-deploy-use-ftp/create_db.png
[go-to-webapp]: ./media/web-sites-php-mysql-deploy-use-ftp/select_webapp.png
[set-deployment-credentials]: ./media/web-sites-php-mysql-deploy-use-ftp/set_credentials.png
[portal-ftp-username-password]: ./media/web-sites-php-mysql-deploy-use-ftp/save_credentials.png
[resource-group]: ./media/web-sites-php-mysql-deploy-use-ftp/set_group.png
[new-web-app]: ./media/web-sites-php-mysql-deploy-use-ftp/create_wa.png
[select-database]: ./media/web-sites-php-mysql-deploy-use-ftp/select_database.png
[select-resourcegroup]: ./media/web-sites-php-mysql-deploy-use-ftp/select_resourcegroup.png
[select-properties]: ./media/web-sites-php-mysql-deploy-use-ftp/select_properties.png
[note-properties]: ./media/web-sites-php-mysql-deploy-use-ftp/note-properties.png

[connection-string-info]: ./media/web-sites-php-web-site-mysql-deploy-use-ftp/connection_string_info.png
[management-portal]: https://portal.azure.com
[download-publish-profile]: ./media/web-sites-php-mysql-deploy-use-ftp/download_publish_profile_3.png
 
