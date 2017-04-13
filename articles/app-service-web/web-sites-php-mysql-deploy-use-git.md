<properties
    pageTitle="PHP MySQL-web-sovelluksen luominen Azure App palvelun ja ota käyttöön Git käyttäminen"
    description="Opetusohjelma, joka osoittaa, joka tallentaa tiedot MySQL PHP web-sovelluksen luominen ja käyttäminen Git käyttöönoton Azure."
    services="app-service\web"
    documentationCenter="php"
    authors="rmcmurray"
    manager="wpickett"
    editor=""
    tags="mysql"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="create-a-php-mysql-web-app-in-azure-app-service-and-deploy-using-git"></a>PHP MySQL-web-sovelluksen luominen Azure App palvelun ja ota käyttöön Git käyttäminen

Tässä opetusohjelmassa näytetään, miten PHP MySQL-web-sovelluksen luominen ja ottaa käyttöön [Sovelluksen](http://go.microsoft.com/fwlink/?LinkId=529714) palveluun käyttämällä Git. Voit käyttää [PHP][install-php], komentorivivalitsimet MySQL-työkalu (osa [MySQL][install-mysql]), ja [Git] [ install-git] tietokoneessasi. Tässä opetusohjelmassa ohjeita voit seurata sellaisille käyttöjärjestelmille, kuten Windows, Mac tai Linux. Kun olet suorittanut tämän oppaan, on käynnissä Azure PHP/MySQL verkkosovellukseen.

Opit seuraavat asiat:

* Luomisesta verkkosovellukseen ja [Azure Portal]MySQL-tietokantaan[management-portal]. Koska PHP on oletusarvoisesti käytössä [App palvelun Web](http://go.microsoft.com/fwlink/?LinkId=529714) Apps-sovelluksissa, mitään erityistä tarvitaan PHP koodin suorittaminen.
* Voit julkaista ja julkaista uudelleen käyttämällä Git Azure sovellusta.
* Ottamisesta käyttöön automatisoida tunnus tunnus-tunniste tehtäviä joka `git push`.

Noudattamalla tässä opetusohjelmassa, voit luoda yksinkertaisen rekisteröinti verkkosovellukseen PHP. Sovelluksen isännöityjen verkkosovelluksissa. Näyttökuva valmiin sovelluksen on alla:

![Azure PHP web-sivusto][running-app]

## <a name="set-up-the-development-environment"></a>Määritä kehitysympäristö

Tässä opetusohjelmassa oletetaan, että [PHP][install-php], komentorivivalitsimet MySQL-työkalu (osa [MySQL][install-mysql]), ja [Git] [ install-git] tietokoneessasi.

<a id="create-web-site-and-set-up-git"></a>
## <a name="create-a-web-app-and-set-up-git-publishing"></a>Web-sovelluksen luominen ja määrittäminen Git julkaiseminen

Voit luoda verkkosovellukseen ja MySQL-tietokantaan seuraavasti:

1. Kirjautuminen [Azure Portal][management-portal].
2. Valitse **Uusi** -kuvaketta.

3. Valitse **Kohdassa kaikki** **Marketplace**-kohdan vieressä. 

4. Valitse **Web + Mobile**jälkeen **Web-sovelluksen + MySQL**. Valitse **Luo**.

4. Anna kelvollinen resurssin ryhmän nimi.

    ![Määritä resurssiryhmän nimi][resource-group]

5. Kirjoita arvot uusi web-sovellus.

    ![Web-sovelluksen luominen][new-web-app]

6. Kirjoita uuden tietokannan, mukaan lukien siitä ehdot arvot.

    ![Luo uusi MySQL-tietokanta][new-mysql-db]

7. Web-sovellus on luotu, näet uudet web app-sivu.

7. **Asetukset** Valitse **Jatkuva**ympäristöön ja valitse sitten Valitse _Määritä tarvittavat asetukset_.

    ![Määritä Git julkaiseminen][setup-publishing]

8. Valitse **Paikallinen Git säilöön** lähde.

    ![Määritä Git säilöön][setup-repository]


9. Git julkaiseminen käyttöön Anna käyttäjänimi ja salasana. Pane merkille käyttäjänimi ja salasana, voit luoda. (Jos olet määrittänyt Git säilön ennen tämän vaiheen ohitetaan.)

    ![Julkaisun tunnistetietojen][credentials]


## <a name="get-remote-mysql-connection-information"></a>Remote MySQL-yhteystiedot

Yhteyden muodostaminen MySQL-tietokantaan, jossa on käytössä Web Apps-kohdassa näkyy on yhteystiedot. Saat MySQL-yhteystietoja, toimi seuraavasti:

1. Valitse resurssi-ryhmässä tietokannan:

    ![Valitse tietokanta][select-database]

2. Tietokannasta **asetukset**Valitse **Ominaisuudet**.

    ![Valitse ominaisuudet][select-properties]

2. Kirjoita muistiin arvojen `Database`, `Host`, `User Id`, ja `Password`.

    ![Huomautus-ominaisuudet][note-properties]

## <a name="build-and-test-your-app-locally"></a>Muodosta ja testaa sovelluksen paikallisesti

Nyt kun olet luonut verkkosovellukseen, voit kehittää sovellusta paikallisesti ja sitten käyttöönotto testauksen jälkeen.

Rekisteröinti-sovellus on yksinkertainen PHP-sovellus, jonka avulla voit rekisteröidä tapahtuman antamalla nimen ja sähköpostiosoitteen. Tietoja edellisen Ilmoittautuneita näkyy taulukon. Rekisteröintitietoja tallennetaan MySQL-tietokantaan. Sovelluksen koostuu yhden tiedoston (kopioi ja liitä koodi käytettävissä alla):

* **Index.php**: Näyttää lomakkeen rekisteröimiseen ja rekisteröijän tiedot sisältävä taulukko.

Voit luoda ja suorittaa sovelluksen paikallisesti, noudata seuraavia ohjeita. Huomaa, että seuraavissa vaiheissa oletetaan PHP ja paikallisessa tietokoneessa määrittäminen MySQL komentorivivalitsimet-työkalu (MySQL osana) ja olet ottanut [San MySQL-laajennus][pdo-mysql].

1. Yhteyden muodostaminen MySQL etäpalvelimeen ja arvo, jota haluat käyttää `Data Source`, `User Id`, `Password`, ja `Database` , joka noutaa aiemmin:

        mysql -h{Data Source] -u[User Id] -p[Password] -D[Database]

2. MySQL komentorivi-ikkuna tulee näkyviin:

        mysql>

3. Liitä seuraavassa `CREATE TABLE` komento luomiseen `registration_tbl` tietokannan taulukkoon:

        CREATE TABLE registration_tbl(id INT NOT NULL AUTO_INCREMENT, PRIMARY KEY(id), name VARCHAR(30), email VARCHAR(30), date DATE);

4. Luo **index.php** tiedoston paikallisen sovelluksen-kansion.

5. Avaa **index.php** tiedostoa tekstieditorissa tai IDE ja lisää seuraava koodi ja tee tarvittavat muutokset merkitty `//TODO:` kommentit.


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
            // DB connection info
            //TODO: Update the values for $host, $user, $pwd, and $db
            //using the values you retrieved earlier from the Azure Portal.
            $host = "value of Data Source";
            $user = "value of User Id";
            $pwd = "value of Password";
            $db = "value of Database";
            // Connect to database.
            try {
                $conn = new PDO( "mysql:host=$host;dbname=$db", $user, $pwd);
                $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );
            }
            catch(Exception $e){
                die(var_dump($e));
            }
            // Insert registration info
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
            // Retrieve data
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
        ?>
        </body>
        </html>

4.  Siirry sovelluksen kansiossa päätteeseen ja kirjoita seuraava komento:

        php -S localhost:8000

Voit nyt selata **http://localhost:8000 /** Testaa sovellus.


## <a name="publish-your-app"></a>Sovelluksen julkaiseminen

Kun olet testannut sovelluksen paikallisesti, voit julkaista Web Apps-sovellusten käyttäminen Git. Alusta paikallisen Git-säilöön ja julkaise sovellus.

> [AZURE.NOTE]
> Nämä ovat samat vaiheet näkyvät web-sovelluksen luominen ja määrittäminen päättyessä Git julkaisun edellisen osan ylös Azure-portaalissa.

1. (Valinnainen)  Jos olet unohtanut tai kadonneet Git remote repostitory URL-osoite, siirry Azure-portaalissa web app-ominaisuuksia.

1. Avaa GitBash (tai päätteen, jos Git on kohdassa `PATH`), muuta kansioiden sovelluksen pääkansiossa ja suorittamalla seuraavat komennot:

        git init
        git add .
        git commit -m "initial commit"
        git remote add azure [URL for remote repository]
        git push azure master

    Voit pyydetään antamaan aiemmin luomasi salasana.

    ![Git kautta ensimmäisen Push Azure][git-initial-push]

2. Siirry **http://[site name].azurewebsites.net/index.php** Aloita (nämä tiedot tallennetaan tilin Raporttinäkymät-ikkunan)-sovelluksessa:

    ![Azure PHP web-sivusto][running-app]

Kun olet julkaissut sovelluksen, voit aloittaa muutosten tekeminen ja Git avulla voit julkaista ne.

## <a name="publish-changes-to-your-app"></a>Sovelluksen muutosten julkaiseminen

Sovelluksen muutosten julkaiseminen seuraavasti:

1. Tee muutokset sovelluksen paikallisesti.
2. Avaa GitBash (tai päätteeseen, it Git on oman `PATH`), muuta kansioiden sovelluksen pääkansiossa ja suorittamalla seuraavat komennot:

        git add .
        git commit -m "comment describing changes"
        git push azure master

    Voit pyydetään antamaan aiemmin luomasi salasana.

    ![Ennen sivuston muutokset Azure Git kautta][git-change-push]

3. Siirry **http://[site name].azurewebsites.net/index.php** sovelluksen ja tekemäsi muutokset:

    ![Azure PHP web-sivusto][running-app]

>[AZURE.NOTE] Jos haluat aloittaa Azure App palvelun ennen rekisteröimässä Azure-tili, siirry [Yritä App palvelu](http://go.microsoft.com/fwlink/?LinkId=523751), jossa lyhytkestoinen starter verkkosovellukseen heti voit luoda sovelluksen-palvelussa. Ei ole pakollinen; luottokortit ei ole sitoumukset.

<a name="composer"></a>
## <a name="enable-composer-automation-with-the-composer-extension"></a>Ota käyttöön tunnus automaatio tunnus-tunniste

Oletusarvon mukaan git käyttöönottoprosessin App palvelussa ei mitään kanssa composer.json, jos käytössäsi on jokin PHP projektin. Voit ottaa käsittelyn aikana composer.json `git push` ottamalla tunnus-tunniste.

1. Oman PHP-web-sovelluksen sivu [Azure portal][management-portal], valitse **Työkalut** > **tunnisteet**.

    ![Tunnus tunniste asetukset][composer-extension-settings]

2. Valitse **Lisää**ja valitse sitten **tunnus**.

    ![Tunnus-laajennuksen lisääminen][composer-extension-add]
    
3. Valitse **OK** , jos haluat hyväksyä juridiset ehdot. Valitse **OK** uudelleen, jos haluat lisätä tunnisteen.

    **Asennetut laajennukset** -sivu näkyy nyt tunnus-tunniste.  
    ![Tunnus-tunniste Näytä][composer-extension-view]
    
4. Nyt suorittaa `git add`, `git commit`, ja `git push` , kuten edellisessä osassa. Nyt näet, että tunnus asentaa composer.json määrittää riippuvuuksia.

    ![Tunnus tunniste onnistui][composer-extension-success]

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja on artikkelissa [PHP Developer Center](/develop/php/).

<!-- URL List -->

[install-php]: http://www.php.net/manual/en/install.php
[install-SQLExpress]: http://www.microsoft.com/download/details.aspx?id=29062
[install-Drivers]: http://www.microsoft.com/download/details.aspx?id=20098
[install-git]: http://git-scm.com/
[install-mysql]: http://dev.mysql.com/downloads/mysql/
[pdo-mysql]: http://www.php.net/manual/en/ref.pdo-mysql.php
[management-portal]: https://portal.azure.com
[sql-database-editions]: http://msdn.microsoft.com/library/windowsazure/ee621788.aspx

<!-- IMG List -->

[running-app]: ./media/web-sites-php-mysql-deploy-use-git/running_app_2.png
[new-website]: ./media/web-sites-php-mysql-deploy-use-git/new_website2.png
[custom-create]: ./media/web-sites-php-mysql-deploy-use-git/create_web_mysql.png
[new-mysql-db]: ./media/web-sites-php-mysql-deploy-use-git/create_db.png
[go-to-webapp]: ./media/web-sites-php-mysql-deploy-use-git/select_webapp.png
[setup-git-publishing]: ./media/web-sites-php-mysql-deploy-use-git/setup_git_publishing.png
[credentials]: ./media/web-sites-php-mysql-deploy-use-git/save_credentials.png
[resource-group]: ./media/web-sites-php-mysql-deploy-use-git/set_group.png
[new-web-app]: ./media/web-sites-php-mysql-deploy-use-git/create_wa.png
[setup-publishing]: ./media/web-sites-php-mysql-deploy-use-git/setup_deploy.png
[setup-repository]: ./media/web-sites-php-mysql-deploy-use-git/select_local_git.png
[select-database]: ./media/web-sites-php-mysql-deploy-use-git/select_database.png
[select-properties]: ./media/web-sites-php-mysql-deploy-use-git/select_properties.png
[note-properties]: ./media/web-sites-php-mysql-deploy-use-git/note-properties.png

[git-instructions]: ./media/web-sites-php-mysql-deploy-use-git/git-instructions.png
[git-change-push]: ./media/web-sites-php-mysql-deploy-use-git/php-git-change-push.png
[git-initial-push]: ./media/web-sites-php-mysql-deploy-use-git/php-git-initial-push.png

[composer-extension-settings]: ./media/web-sites-php-mysql-deploy-use-git/composer-extension-settings.png
[composer-extension-add]: ./media/web-sites-php-mysql-deploy-use-git/composer-extension-add.png
[composer-extension-view]: ./media/web-sites-php-mysql-deploy-use-git/composer-extension-view.png
[composer-extension-success]: ./media/web-sites-php-mysql-deploy-use-git/composer-extension-success.png
