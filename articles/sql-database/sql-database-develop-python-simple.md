<properties
    pageTitle="Yhteyden muodostaminen SQL-tietokantaan Python avulla | Microsoft Azure"
    description="Esittää Python koodin otoksen avulla voit muodostaa yhteyden Azure SQL-tietokantaan."
    services="sql-database"
    documentationCenter=""
    authors="meet-bhagdev"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="drivers"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="meetb"/>


# <a name="connect-to-sql-database-by-using-python"></a>Yhteyden muodostaminen SQL-tietokantaan Python avulla


[AZURE.INCLUDE [sql-database-develop-includes-selector-language-platform-depth](../../includes/sql-database-develop-includes-selector-language-platform-depth.md)] 


Tässä ohjeaiheessa esitellään, miten yhteyden ja kyselyjen avulla Python Azure SQL-tietokantaan. Voit suorittaa tässä esimerkissä Windows-, Ubuntu Linux- tai Mac-ympäristöissä.


## <a name="step-1-create-a-sql-database"></a>Vaihe 1: SQL-tietokannan luominen

Katso [aloittaminen-sivu](sql-database-get-started.md) luominen mallitietokanta.  On tärkeää noudatat luominen **AdventureWorks-tietokantamallin**opas. Alla on esitetty esimerkkejä toimivat vain **AdventureWorks rakenne**. Kun olet luonut tietokannan muutoksista että otat käyttöön IP-osoitteeseen ottamalla palomuurisäännöt kuvatulla tavalla [aloittaminen-sivu](sql-database-get-started.md)

## <a name="step-2-configure-development-environment"></a>Vaihe 2: Määritä kehitysympäristö

### <a name="mac-os"></a>**Mac OS**   
### <a name="install-the-required-modules"></a>Asenna tarvittavat moduulit
Avaa päätteen ja asentaminen

    ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
    brew install FreeTDS
    sudo -H pip install pymssql=2.1.1

### <a name="linux-ubuntu"></a>**Linux (Ubuntu)**

Avaa päätteen ja siirry hakemistoon, minne luomisesta oman python komentosarjan. Kirjoita seuraavat komennot **FreeTDS** ja **pymssql**asentamiseen. pymssql FreeTDS avulla voit muodostaa yhteyden SQL-tietokannat.

    sudo apt-get --assume-yes update
    sudo apt-get --assume-yes install freetds-dev freetds-bin
    sudo apt-get --assume-yes install python-dev python-pip
    sudo pip install pymssql=2.1.1
    
### <a name="windows"></a>**Windows**

Asenna pymssql [**täältä**](http://www.lfd.uci.edu/~gohlke/pythonlibs/#pymssql). 

Varmista, että valitset oikean whl-tiedosto. Esimerkki: Jos käytät Python 2.7 64-bittiseen tietokoneeseen: pymssql‑2.1.1‑cp27‑none‑win_amd64.whl. Kun olet ladannut .whl tiedoston sijoittaa C:/Python27-kansioon.

Asenna nyt käyttämällä pip komentoriviltä pymssql ohjain. C:/Python27 ja suorita seuraavat CD-levylle
    
    pip install pymssql‑2.1.1‑cp27‑none‑win_amd64.whl

Ohjeet, jotta Käytä pip löytyy [tähän](http://stackoverflow.com/questions/4750806/how-to-install-pip-on-windows)

## <a name="step-3-run-sample-code"></a>Vaihe 3: Esimerkki koodin suorittaminen

Luo tiedosto nimeltä **sql_sample.py** ja liitä seuraava koodi sisällä. Voit suorittaa tämän komennon käyttämisen:
    
    python sql_sample.py

### <a name="connect-to-your-sql-database"></a>Yhteyden muodostaminen SQL-tietokantaan

[Pymssql.connect](http://pymssql.org/en/latest/ref/pymssql.html) -funktiota käytetään muodostaa yhteyden SQL-tietokantaan.

    import pymssql
    conn = pymssql.connect(server='yourserver.database.windows.net', user='yourusername@yourserver', password='yourpassword', database='AdventureWorks')


### <a name="execute-an-sql-select-statement"></a>Suorita SQL SELECT-lause

[Cursor.execute](http://pymssql.org/en/latest/ref/pymssql.html#pymssql.Cursor.execute) -funktion avulla voidaan noutaa kyselyn tulosjoukon vastaan SQL-tietokantaan. Tämä funktio hyväksyy olennaisesti kyselyn ja palauttaa tulosjoukon, joka on iteroitava päälle [cursor.fetchone()](http://pymssql.org/en/latest/ref/pymssql.html#pymssql.Cursor.fetchone)avulla.


    import pymssql
    conn = pymssql.connect(server='yourserver.database.windows.net', user='yourusername@yourserver', password='yourpassword', database='AdventureWorks')
    cursor = conn.cursor()
    cursor.execute('SELECT c.CustomerID, c.CompanyName,COUNT(soh.SalesOrderID) AS OrderCount FROM SalesLT.Customer AS c LEFT OUTER JOIN SalesLT.SalesOrderHeader AS soh ON c.CustomerID = soh.CustomerID GROUP BY c.CustomerID, c.CompanyName ORDER BY OrderCount DESC;')
    row = cursor.fetchone()
    while row:
        print str(row[0]) + " " + str(row[1]) + " " + str(row[2])   
        row = cursor.fetchone()


### <a name="insert-a-row-pass-parameters-and-retrieve-the-generated-primary-key"></a>Lisää rivi, välittää parametreja ja hakea luotu perusavain

SQL-tietokantaan [IDENTITY](https://msdn.microsoft.com/library/ms186775.aspx) -ominaisuutta ja [sarjan](https://msdn.microsoft.com/library/ff878058.aspx) objektin avulla voidaan luoda [perusavaimen](https://msdn.microsoft.com/library/ms179610.aspx) arvot. 


    import pymssql
    conn = pymssql.connect(server='yourserver.database.windows.net', user='yourusername@yourserver', password='yourpassword', database='AdventureWorks')
    cursor = conn.cursor()
    cursor.execute("INSERT SalesLT.Product (Name, ProductNumber, StandardCost, ListPrice, SellStartDate) OUTPUT INSERTED.ProductID VALUES ('SQL Server Express', 'SQLEXPRESS', 0, 0, CURRENT_TIMESTAMP)")
    row = cursor.fetchone()
    while row:
        print "Inserted Product ID : " +str(row[0])
        row = cursor.fetchone()


### <a name="transactions"></a>Tapahtumat


Koodin tässä esimerkissä kuvataan tapahtumia, jossa voit:

* Aloittaa tapahtuman
* Tietorivin lisääminen
* Peruutus tapahtuman lisääminen kumoaminen 

Liitä seuraava koodi sql_sample.py sisällä.
    
    import pymssql
    conn = pymssql.connect(server='yourserver.database.windows.net', user='yourusername@yourserver', password='yourpassword', database='AdventureWorks')
    cursor = conn.cursor()
    cursor.execute("BEGIN TRANSACTION")
    cursor.execute("INSERT SalesLT.Product (Name, ProductNumber, StandardCost, ListPrice, SellStartDate) OUTPUT INSERTED.ProductID VALUES ('SQL Server Express New', 'SQLEXPRESS New', 0, 0, CURRENT_TIMESTAMP)")
    cnxn.rollback()

## <a name="next-steps"></a>Seuraavat vaiheet

* Tarkista [SQL-tietokannan kehittäminen yleiskatsaus](sql-database-develop-overview.md)
* Lisätietoja [Microsoft SQL Server Python-ohjain](https://msdn.microsoft.com/library/mt652092.aspx)
* Käy [Python Developer Center](/develop/python/).

## <a name="additional-resources"></a>Lisäresursseja 

* [Rakenne-kuvioiden määrittäminen usean vuokraajan SaaS sovellusten Azure SQL-tietokanta](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* Selaa kaikkia [ominaisuuksia SQL-tietokantaan](https://azure.microsoft.com/services/sql-database/)
