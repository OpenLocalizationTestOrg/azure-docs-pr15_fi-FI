<properties
   pageTitle="Tietolähteen yhteydet | Microsoft Azure"
   description="Tässä artikkelissa kuvataan tietolähdeyhteydet tietomallien Azure Analysis Services-palveluissa."
   services="analysis-services"
   documentationCenter=""
   authors="minewiskan"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="analysis-services"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="na"
   ms.date="10/25/2016"
   ms.author="owend"/>

# <a name="datasource-connections"></a>Tietolähteen yhteydet

Tietomallien Azure Analysis Services-palveluissa saattaa tarvita eri tietopalveluiden tietolähteiden yhteyden. Joissakin tapauksissa muodostamisesta tietolähteiden alkuperäisen tarjoajien, kuten SQL Server Native Client (SQLNCLI11) Taulukkomalleissa voi palauttaa virheen.

Esimerkiksi; Jos sinulla ladatun tai suoraan kyselyn tietomallin, joka muodostaa yhteyden cloud-tietolähteestä, kuten Azure SQL-tietokanta, jos käytössäsi on alkuperäinen tarjoajiin kuin SQLOLEDB, voi tulla virhesanoma: **"" SQLNCLI11.1"ei ole rekisteröity toimittaja"**.

Tai jos käytössäsi on DirectQuery yhteyden muodostaminen paikalliseen tietolähteisiin, jos käytössäsi on alkuperäinen tarjoajat voi tulla virhesanoma: **"Virhe luotaessa OLE DB-rivin määrittäminen. Virheellinen syntaksi lähellä 'LIMIT' "**.

## <a name="data-source-providers"></a>Tietolähteen tietopalveluiden

Seuraavat tietolähteen tarjoajat tuetaan ladatun tai suora kyselyn tietomallien paikallisen yhdistettäessä tai cloud tietolähteisiin:

|               | **Tietolähde**                     | **Ladatun**                            |  **Suora kysely**                                           |
|---------------------------|-------------------------------|---------------------------------------------|---------------------------------------------|
| **Cloud**                     | Azure SQL-tietovarasto      | SQL Serverin .NET framework-tietopalvelu | SQL Serverin .NET framework-tietopalvelu |
|                           | Azure SQL-tietokanta            | SQL Serverin .NET framework-tietopalvelu | SQL Serverin .NET framework-tietopalvelu |
| **Paikallisen** (joko yhdyskäytävän) | SQL Server                    | SQL Server Native Client 11.0               | SQL Serverin .NET framework-tietopalvelu |
|                           |  SQL Server                             | Microsoft OLE DB-palvelu SQL Serveriä varten    |   SQL Serverin .NET framework-tietopalvelu                                          |
|                           |  SQL Server                             | SQL Serverin .NET framework-tietopalvelu |  SQL Serverin .NET framework-tietopalvelu                                           |
|                           | Oracle                        | Oracle Microsoft OLE DB-palvelu        | .NET Frameworkin Oracle-tietopalvelu               |
|                           |  Oracle                             | .NET Frameworkin Oracle-tietopalvelu               | .NET Frameworkin Oracle-tietopalvelu                                            |
|                           | Teradata                      | Teradata OLE DB-palvelu                | .NET Teradata-tietopalvelu             |
|                           |  Teradata                             | .NET Teradata-tietopalvelu             |  .NET Teradata-tietopalvelu                                            |
|                           | Analytics järjestelmä | SQL Serverin .NET framework-tietopalvelu | SQL Serverin .NET framework-tietopalvelu |


> [AZURE.NOTE] Varmista 64-bittinen tarjoajien on asennettu paikallisen yhdyskäytävän käytettäessä.

Siirrettäessä paikallisen SQL Server Analysis Servicesin taulukkomallisia Azure Analysis Services voi olla tarpeen muuttaa toimittaja.

**Tietolähteen tarjoajan määrittäminen**

1. Valitse SSDT > **Taulukkomuotoinen Model Explorer** > **Tietolähteitä**, tietolähdeyhteys hiiren kakkospainikkeella ja valitse sitten **Muokkaa tietolähdettä**.

2. Valitse **Muokkaa yhteyttä**-ikkunassa Avaa etukäteen ominaisuudet-ikkunassa **Lisäasetukset** .

3. **Määritä lisäominaisuudet**- > **tarjoajien**ja valitse sitten haluamasi palvelu.

## <a name="impersonation"></a>Tekeytyminen
Joissakin tapauksissa saattaa olla tarpeen Määritä eri tekeytyminen tili. Tekeytymistaso tili voidaan määrittää SSDT tai SSMS.

Paikallisen tietolähteiden:

- Jos käytät SQL-todennusta, tekeytyminen on oltava palvelutilin.
- Jos käytät Windows-todennusta, Määritä Windowsin käyttäjä ja salasanalla. SQL Server Windows-todennuksen tietyn tekeytyminen tilillä tuetaan vain ladatun tietomallien.

Cloud tietolähteiden:

- Jos käytät SQL-todennusta, tekeytyminen on oltava palvelutilin.


## <a name="next-steps"></a>Seuraavat vaiheet

Jos sinulla on paikallisen tietolähteitä, muista asentaa [paikallisen yhdyskäytävä](analysis-services-gateway.md). Lisätietoja palvelimellesi SSDT tai SSMS hallinta-kohdassa [Manage palvelimellesi](analysis-services-manage.md).
