<properties
   pageTitle="Upotetun Microsoft Power BI Preview vianmääritys"
   description="Upotetun Microsoft Power BI Preview vianmääritys"
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="microsoft-power-bi-embedded-preview-troubleshooting"></a>Upotetun Microsoft Power BI Preview vianmääritys
Tässä artikkelissa on vastauksia vianmääritys **Power BI upotetun**.

<a name="connection-string"/>
## <a name="setting-sql-server-connection-strings"></a>SQL Server-yhteyden merkkijonot määrittäminen
Voit määrittää yhteyden merkkijonon SQL Server-haluat seurata tietyssä muodossa. Alla on esimerkki-yhteysmerkkijonon SQL Serveriä varten.

```
"Persist Security Info=False;Integrated Security=true;Initial Catalog=Northwind;server=(local)"
```

Saat lisätietoja SQL Serverin yhteyden merkkijonot on seuraavissa artikkeleissa:

-   [SQL Server yhteyden merkkijonoja](https://msdn.microsoft.com/library/jj653752.aspx)
-   [SqlConnection.ConnectionString](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.connectionstring.aspx)

<a name="credentials"/>
## <a name="setting-credentials"></a>Tunnistetietojen määrittäminen
Tunnistetietojen kehittämistä tai väliaikaisen ympäristössä, kuten käyttäjänimi ja salasana, joissa kirjoitusmuotoon voit joutua päivittämään tunnistetiedot, jotka vastaavat tuotannon ratkaista.

## <a name="see-also"></a>Katso myös
- [Esimerkki käytön aloittaminen](power-bi-embedded-get-started-sample.md)
- [Mikä on Power BI upotetun](power-bi-embedded-what-is-power-bi-embedded.md)
