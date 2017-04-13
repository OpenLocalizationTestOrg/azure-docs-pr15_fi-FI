<properties
    pageTitle="SQL-tietokantaan alatason tarkistamista ja IP-päätepisteen muutosten seuranta | Microsoft Azure"
    description="Lisätietoja SQL-tietokantaan alatason asiakkaiden tuki- ja IP päätepisteen muutosten seuranta."
    services="sql-database"
    documentationCenter=""
    authors="ronitr"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/10/2016"
    ms.author="ronitr"/>

# <a name="sql-database----downlevel-clients-support-and-ip-endpoint-changes-for-auditing"></a>SQL-tietokanta - alatason asiakkaiden tuki- ja IP-päätepisteen muutosten seuranta


[Valvonta](sql-database-auditing-get-started.md) toimii automaattisesti SQL-asiakkaat, jotka tukevat TKJ uudelleenohjausta.


##<a id="subheading-1"></a>Alatason asiakkaiden tuki

Asiakas, joka toteuttaa TKJ 7.4 pitäisi myös tukevat uudelleenohjausta. Poikkeusta Sisällytä JDBC 4.0, jossa uudelleenohjaus-ominaisuutta ei tueta täysin ja mitkä uudelleenohjaus Node.JS Tedious ole otettu käyttöön.

"Alatason asiakkaille" eli mitkä tuki TKJ 7.3 ja alapuolella - yhteys palvelimeen FQDN merkkijono on muutettava:

Alkuperäisen palvelimen FQDN yhteysmerkkijonon: <*palvelimen nimi*>. database.windows.net

Muokattu palvelimen FQDN yhteysmerkkijonon: <*palvelimen nimi*> .database. **suojatun**. windows.net

Osittainen "Alatason asiakkaille"-luettelo sisältää:

- .NET 4.0 ja alla
- ODBC-10.0 ja alapuolelle.
- JDBC (vaikka JDBC tue TKJ 7.4, TKJ uudelleenohjaus ominaisuutta ei täysin tueta)
- Ylimääräiseltä (for Node.JS)

**Huomautus:** Yllä palvelimen FDQN muokkaus voi olla hyötyä myös ilman tarvetta tietokantojen (tilapäinen rajoituksen) määrittäminen vaiheessa SQL Server tason tarkistaminen käytäntö otetaan käyttöön.

##<a id="subheading-2"></a>IP-päätepiste muuttuu, kun valvonta ottaminen käyttöön

Huomaa, että kun otat valvonta, tietokannan IP-päätepiste muuttuu. Jos sinulla on tarkka palomuuriasetukset, päivitä kyseiset palomuuriasetukset vastaavasti.

Uuden tietokannan IP-päätepisteen riippuu tietokannan sisältävä alue:

| Tietokanta-alue | Mahdolliset IP-päätepisteet |
|----------|---------------|
| Kiinan Pohjois  | 139.217.29.176, 139.217.28.254 |
| Kiinan Itä  | 42.159.245.65, 42.159.246.245 |
| Australia Itä  | 104.210.91.32 40.126.244.159, 191.239.64.60, 40.126.255.94 |
| Australia varaaja | 191.239.184.223 40.127.85.81, 191.239.161.83, 40.127.81.130 |
| Brasilia Etelä  | 104.41.44.161 104.41.62.230, 23.97.99.54, 104.41.59.191 |
| Keskitetyn USA  | 104.43.255.70 40.83.14.7, 23.99.128.244, 40.83.15.176 |
| Itä-Aasian   | 23.99.125.133 13.75.40.42, 23.97.71.138, 13.94.43.245 |
| Yhdysvaltojen Itä 2 | 104.209.141.31 104.208.238.177, 191.237.131.51, 104.208.235.50 |
| Yhdysvaltojen Itä   | 23.96.107.223 104.41.150.122, 23.96.38.170, 104.41.146.44 |
| Keskitetyn Intia  | 104.211.98.219, 104.211.103.71 |
| Etelä Intia   | 104.211.227.102, 104.211.225.157 |
| Länsi Intia  | 104.211.161.152, 104.211.162.21 |
| Japani Itä   | 104.41.179.1 40.115.253.81, 23.102.64.207, 40.115.250.196 |
| Japani Länsi    | 104.214.140.140 104.214.146.31, 191.233.32.34, 104.214.146.198 |
| Pohjois-keskitetyn USA  | 191.236.155.178 23.96.192.130, 23.96.177.169, 23.96.193.231 |
| Pohjois-Eurooppa  | 104.41.209.221 40.85.139.245, 137.116.251.66, 40.85.142.176 |
| Etelä keskitetyn USA  | 191.238.184.128 40.84.190.84, 23.102.160.153, 40.84.186.66 |
| Kaakkoisaasialaiset Aasian  | 104.215.198.156 13.76.252.200, 23.97.51.109, 13.76.252.113 |
| Länsi Europe  | 104.40.230.120, 13.80.23.64, 137.117.171.161, 13.80.8.37, 104.47.167.215, 40.118.56.193, 104.40.176.73, 40.118.56.20 |
| Länsi USA  | 191.236.123.146 138.91.163.240, 168.62.194.148, 23.99.6.91 |
| Kanada Keski  | 13.88.248.106 |
| Kanada Itä  |  40.86.227.82 |
