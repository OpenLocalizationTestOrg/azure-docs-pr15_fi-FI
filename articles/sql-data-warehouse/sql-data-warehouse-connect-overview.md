<properties
   pageTitle="Yhteyden muodostaminen Azure SQL-tietovarasto | Microsoft Azure"
   description="Etsiminen palvelimen nimi ja yhteysmerkkijonon, että voit Azure SQL-tietovarasto"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/26/2016"
   ms.author="sonyama;barbkess"/>

# <a name="connect-to-azure-sql-data-warehouse"></a>Yhteyden muodostaminen Azure SQL-tietovarasto

Tämän artikkelin avulla voit yhteyden muodostaminen SQL-tietovarasto ensimmäisen kerran.

## <a name="find-your-server-name"></a>Palvelimen nimen selvittäminen

Yhteyden muodostaminen SQL-tietovarasto ensimmäinen askel on tietää, miten palvelimen nimen selvittäminen.  Seuraavassa esimerkissä palvelimen nimi on esimerkiksi sample.database.windows.net. Etsi palvelimen täydellinen nimi:

1. Siirry [Azure portal][].
2. Valitse **SQL-tietokannat** 
3. Valitse tämä vaihtoehto, jos haluat muodostaa yhteyden tietokantaan.
4. Etsi palvelimen täydellinen nimi.

    ![Palvelimen täydellinen nimi][1]

## <a name="supported-drivers-and-connection-strings"></a>Tuetut ohjaimet ja yhteyden merkkijonoja

Azure SQL-tietovarasto tukee [ADO.NET][], [ODBC][], [PHP][]ja [JDBC][]. Valitse jokin edellisen ohjaimet uusin versio ja ohjeiden etsiminen. Automaattiseen luomiseen ohjaimen, jota käytät yhteysmerkkijonoa Azure-portaalista, voit valita- **tietokannan yhteysmerkkijonot Näytä** edellisessä esimerkissä käytetty mallitietokanta.  Seuraavassa on joitakin esimerkkejä siitä, miltä yhteysmerkkijonon näyttää kunkin ohjaimen.

> [AZURE.NOTE] Harkitse yhteyden aikakatkaisu arvoksi 300 sekuntia, jotta yhteyden uutta siitä lyhyitä aikoja käyttöä varten.

### <a name="adonet-connection-string-example"></a>ADO.NET yhteyden merkkijono-Esimerkki

```C#
Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};User ID={your_user_name};Password={your_password_here};Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;
```

### <a name="odbc-connection-string-example"></a>ODBC-yhteyttä merkkijono-Esimerkki

```C#
Driver={SQL Server Native Client 11.0};Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};Uid={your_user_name};Pwd={your_password_here};Encrypt=yes;TrustServerCertificate=no;Connection Timeout=30;
```

### <a name="php-connection-string-example"></a>PHP yhteyden merkkijono-Esimerkki

```PHP
Server: {your_server}.database.windows.net,1433 \r\nSQL Database: {your_database}\r\nUser Name: {your_user_name}\r\n\r\nPHP Data Objects(PDO) Sample Code:\r\n\r\ntry {\r\n   $conn = new PDO ( \"sqlsrv:server = tcp:{your_server}.database.windows.net,1433; Database = {your_database}\", \"{your_user_name}\", \"{your_password_here}\");\r\n    $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );\r\n}\r\ncatch ( PDOException $e ) {\r\n   print( \"Error connecting to SQL Server.\" );\r\n   die(print_r($e));\r\n}\r\n\rSQL Server Extension Sample Code:\r\n\r\n$connectionInfo = array(\"UID\" => \"{your_user_name}\", \"pwd\" => \"{your_password_here}\", \"Database\" => \"{your_database}\", \"LoginTimeout\" => 30, \"Encrypt\" => 1, \"TrustServerCertificate\" => 0);\r\n$serverName = \"tcp:{your_server}.database.windows.net,1433\";\r\n$conn = sqlsrv_connect($serverName, $connectionInfo);
```

### <a name="jdbc-connection-string-example"></a>JDBC yhteyden merkkijono-Esimerkki

```Java
jdbc:sqlserver://yourserver.database.windows.net:1433;database=yourdatabase;user={your_user_name};password={your_password_here};encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;
```

## <a name="connection-settings"></a>Yhteysasetukset

SQL-tietovarasto yhdenmukaistaa joitakin asetuksia yhteyden ja käyttäjäobjektin luominen aikana. Näitä asetuksia ei voi korvata, eikä se sisältää:

| Tietokanta-asetus       | Arvo                        |
| :--------------------- | :--------------------------- |
| [EI OLE KÄYTÖSSÄ][]         | KÄYTÖSSÄ                           |
| [QUOTED_IDENTIFIERS][] | KÄYTÖSSÄ                           |
| [PÄIVÄMÄÄRÄMUOTO][]         | MDY                          |
| [DATEFIRST][]          | 7                            |

## <a name="next-steps"></a>Seuraavat vaiheet

Yhdistä ja kysely, jossa on Visual Studio on ohjeaiheessa [kyselyn Visual Studiossa][]. Lisätietoja todennusasetukset on artikkelissa [Azure SQL-tietovarasto todennusta][].

<!--Articles-->
[Kysely, joka sisältää Visual Studio]: ./sql-data-warehouse-query-visual-studio.md
[Azure SQL-tietovarasto-todennus]: ./sql-data-warehouse-authentication.md

<!--MSDN references-->
[ADO.NET]: https://msdn.microsoft.com/library/e80y5yhx(v=vs.110).aspx
[ODBC]: https://msdn.microsoft.com/library/jj730314.aspx
[PHP]: https://msdn.microsoft.com/library/cc296172.aspx?f=255&MSPPError=-2147217396
[JDBC]: https://msdn.microsoft.com/library/mt484311(v=sql.110).aspx
[EI OLE KÄYTÖSSÄ]: https://msdn.microsoft.com/library/ms188048.aspx
[QUOTED_IDENTIFIERS]: https://msdn.microsoft.com/library/ms174393.aspx
[PÄIVÄMÄÄRÄMUOTO]: https://msdn.microsoft.com/library/ms189491.aspx
[DATEFIRST]: https://msdn.microsoft.com/library/ms181598.aspx

<!--Other-->
[Azure portal]: https://portal.azure.com

<!--Image references-->
[1]: media/sql-data-warehouse-connect-overview/get-server-name.png


