<properties
   pageTitle="SQL-tietovarasto ohjaimet | Microsoft Azure"
   description="Yhteysmerkkijonot ja SQL-tietovarasto ohjaimet"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/24/2016"
   ms.author="sonyama;barbkess"/>


# <a name="drivers-for-azure-sql-data-warehouse"></a>Azure SQL-tietovarasto ohjaimet

Voit muodostaa yhteyden SQL-tietovarasto käyttämällä useita eri sovelluksessa protokollia, kuten [ADO.NET][]-, [ODBC][]-, [PHP][] ja [JDBC][]. Seuraavassa on esimerkkejä kunkin protokolla merkkijonojen yhteydet.  Voit myös luoda yhteysmerkkijono Azure portaalin.  Siirry Azure-portaalissa yhteysmerkkijono luonnissa tietokanta-sivu, valitse *Essentials* valitsemalla *Näytä tietokannan yhteysmerkkijonoja*.

## <a name="sample-adonet-connection-string"></a>Esimerkki ADO.NET-yhteysmerkkijonon

```C#
Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};User ID={your_user_name};Password={your_password_here};Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;
```

## <a name="sample-odbc-connection-string"></a>Esimerkki ODBC-yhteysmerkkijono

```C#
Driver={SQL Server Native Client 11.0};Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};Uid={your_user_name};Pwd={your_password_here};Encrypt=yes;TrustServerCertificate=no;Connection Timeout=30;
```

## <a name="sample-php-connection-string"></a>Esimerkki PHP yhteysmerkkijonon

```PHP
Server: {your_server}.database.windows.net,1433 \r\nSQL Database: {your_database}\r\nUser Name: {your_user_name}\r\n\r\nPHP Data Objects(PDO) Sample Code:\r\n\r\ntry {\r\n   $conn = new PDO ( \"sqlsrv:server = tcp:{your_server}.database.windows.net,1433; Database = {your_database}\", \"{your_user_name}\", \"{your_password_here}\");\r\n    $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );\r\n}\r\ncatch ( PDOException $e ) {\r\n   print( \"Error connecting to SQL Server.\" );\r\n   die(print_r($e));\r\n}\r\n\rSQL Server Extension Sample Code:\r\n\r\n$connectionInfo = array(\"UID\" => \"{your_user_name}\", \"pwd\" => \"{your_password_here}\", \"Database\" => \"{your_database}\", \"LoginTimeout\" => 30, \"Encrypt\" => 1, \"TrustServerCertificate\" => 0);\r\n$serverName = \"tcp:{your_server}.database.windows.net,1433\";\r\n$conn = sqlsrv_connect($serverName, $connectionInfo);
```

## <a name="sample-jdbc-connection-string"></a>Esimerkki JDBC yhteysmerkkijonon

```Java
jdbc:sqlserver://yourserver.database.windows.net:1433;database=yourdatabase;user={your_user_name};password={your_password_here};encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;
```

> [AZURE.NOTE] Harkitse yhteyden aikakatkaisu arvoksi 300 sekunnin, jotta yhteyden uutta siitä lyhyitä aikoja käyttöä varten.

## <a name="next-steps"></a>Seuraavat vaiheet

Käynnistä Visual Studio ja muita sovelluksia tietojen varastossa kyselyt, on artikkelissa [kyselyn Visual Studiossa][].

<!--Image references-->

<!--Azure.com references-->
 [Kysely, joka sisältää Visual Studio]: ./sql-data-warehouse-query-visual-studio.md
 
<!--MSDN references-->
[ADO.NET]: https://msdn.microsoft.com/library/e80y5yhx(v=vs.110).aspx
[ODBC]: https://msdn.microsoft.com/library/jj730314.aspx
[PHP]: https://msdn.microsoft.com/library/cc296172.aspx?f=255&MSPPError=-2147217396
[JDBC]: https://msdn.microsoft.com/library/mt484311(v=sql.110).aspx

<!--Other references-->
