
<!--
includes/sql-database-include-connection-string-30-compare.md

Latest Freshness check:  2015-09-03 , GeneMi.

## Connection string
-->


### <a name="compare-the-connection-string"></a>Vertaa yhteysmerkkijono


Seuraavassa taulukossa on Vertailtu yhteyden merkkijonot C#-ohjelma on paikallisen SQL-palvelimeen ja Azure SQL-tietokanta pilveen. Erot ovat lihavoituna.


| Yhteysmerkkijono<br/>Azure SQL-tietokanta | Yhteysmerkkijono<br/>Microsoft SQL Server |
| :-- | :-- |
| Palvelimen =**tcp:**{your_serverName_here}**. database.windows.net,1433**;<br/>Käyttäjätunnus = {your_loginName_here}**@{your_serverName_here}**;<br/>Salasanan = {your_password_here};<br/>**Tietokannan = {your_databaseName_here};**<br/>**Yhteyden aikakatkaisu = 30**;<br/>**Salaa = True**;<br/>**TrustServerCertificate = False**; | Palvelimen = {your_serverName_here};<br/>Käyttäjätunnus = {your_loginName_here};<br/>Salasanan = {your_password_here}; |


**Tietokannan =** on SQL Server on valinnainen, mutta tarvitaan SQL-tietokantaan.


[.NET ADO SqlConnectionStringBuilder ominaisuudet](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder_properties.aspx) - kerrotaan yksityiskohtaisesti parametrit.


<!--
These three includes/ files are a sequenced set, but you can pick and choose:

includes/sql-database-include-connection-string-20-portalshots.md
includes/sql-database-include-connection-string-30-compare.md
includes/sql-database-include-connection-string-40-config.md
-->
