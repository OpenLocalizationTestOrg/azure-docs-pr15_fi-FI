<properties
   pageTitle="SQL-tietovarasto (portaalin) läpinäkyvä salauksen | Microsoft Azure"
   description="Läpinäkyvä salauksen (TDE)-SQL-tietovarasto"
   services="sql-data-warehouse"
   documentationCenter=""
   authors="ronortloff"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.workload="data-management"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="09/24/2016" 
   ms.author="rortloff;barbkess;sonyama"/>

# <a name="get-started-with-transparent-data-encryption-tde-in-sql-data-warehouse"></a>Aloita kanssa läpinäkyvä tietojen salaus (TDE)-SQL-tietovarasto

> [AZURE.SELECTOR]
- [Yleistä suojauksesta](sql-data-warehouse-overview-manage-security.md)
- [Todennus](sql-data-warehouse-authentication.md)
- [Salauksen (portaalin)](sql-data-warehouse-encryption-tde.md)
- [Salauksen (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)

## <a name="required-permssions"></a>Tarvittavat oikeudet

Jotta läpinäkyvä tietojen salaus (TDE) on oltava järjestelmänvalvoja tai dbmanager-roolin jäsen.

## <a name="enabling-encryption"></a>Salauksen ottaminen käyttöön

Ottaa käyttöön TDE SQL-tietovarasto, noudata seuraavia ohjeita:

1. Avaa tietokanta [Azure portal](https://portal.azure.com)
2. Valitse tietokanta-sivu **asetukset** -painike
3. Valitse **Läpinäkyvä salauksen** -vaihtoehto![][1]
4. **Valitse** -asetuksen valitseminen![][2]
5. Valitse **Tallenna**
![][3]  

## <a name="disabling-encryption"></a>Salauksen poistaminen käytöstä

Voit poistaa käytöstä TDE, SQL-tietovarasto, noudata seuraavia ohjeita:

1. Avaa tietokanta [Azure portal](https://portal.azure.com)
2. Valitse tietokanta-sivu **asetukset** -painike
3. Valitse **Läpinäkyvä salauksen** -vaihtoehto![][1]
4. Valitse **käytöstä** -asetus![][4]
5. Valitse **Tallenna**
![][5]  

## <a name="encryption-dmvs"></a>Salauksen DMVs

Salaus on vahvistettava seuraavat DMVs kanssa:

- [sys.Databases]
- [sys.dm_pdw_nodes_database_encryption_keys]

<!--MSDN references-->
[Transparent Data Encryption (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx
[sys.Databases]: http://msdn.microsoft.com/library/ms178534.aspx
[sys.dm_pdw_nodes_database_encryption_keys]: https://msdn.microsoft.com/library/mt203922.aspx

<!--Image references-->
[1]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings.png
[2]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-on.png
[3]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-save.png
[4]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-off.png
[5]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-save2.png

<!--Link references-->
