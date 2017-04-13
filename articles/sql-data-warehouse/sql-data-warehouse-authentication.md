<properties
   pageTitle="Azure SQL-tietovarasto todennusta | Microsoft Azure"
   description="Azure Active Directory (AAD) ja SQL Server todennusta Azure SQL-tietovarasto."
   services="sql-data-warehouse"
   documentationCenter=""
   authors="byham"
   manager="barbkess"
   editor=""
   tags=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="09/24/2016"
   ms.author="rickbyh;barbkess;sonyama"/>

# <a name="authentication-to-azure-sql-data-warehouse"></a>Azure SQL-tietovarasto-todennus

> [AZURE.SELECTOR]
- [Yleistä suojauksesta](sql-data-warehouse-overview-manage-security.md)
- [Todennus](sql-data-warehouse-authentication.md)
- [Salauksen (portaalin)](sql-data-warehouse-encryption-tde.md)
- [Salauksen (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)

Voit muodostaa yhteyden SQL-tietovarasto, sinun täytyy Välitä suojausvaltuudet todennusta varten. Yhteydessä yhteyden muodostamisesta tietyt asetukset on määritetty osana vahvistamiseksi query-istunnon.  

Saat lisätietoja suojausta ja yhteyksiä data warehouse ottaminen käyttöön [suojatun tietovarasto SQL-tietokantaan][].

## <a name="sql-authentication"></a>SQL-todennus
Voit muodostaa yhteyden SQL-tietovarasto, sinun on annettava seuraavat tiedot:

- Palvelimen täydellinen nimi
- Määritä SQL-todennus
- Käyttäjänimi
- Salasana
- Oletustietokanta (valinnainen)

Oletusarvon mukaan yhteys muodostaa yhteyden *perusmuodon* tietokanta ja ei käyttäjän tietokantaa. Käyttäjä-tietokantayhteyden muodostamisessa, voit tehdä jommankumman seuraavista:

- Määritä oletusarvoinen tietokanta, rekisteröinnin yhteydessä SQL Server Object Explorerissa SSDT, SSMS, tai sovelluksen yhteysmerkkijonon palvelimellesi. Voit lisätä esimerkiksi InitialCatalog parametrin ODBC-yhteyttä.
- Korosta käyttäjän tietokanta ennen SSDT istunnon luominen.

> [AZURE.NOTE] Transact-SQL-lauseen **Käyttäminen OmaTietokanta;** muuttaminen yhteyden tietokantaan ei tueta. Ohjeita yhteyden muodostaminen SQL-tietovarasto SSDT kanssa Lue artikkeli [kyselyn Visual Studiossa][] .

## <a name="azure-active-directory-aad-authentication"></a>Azure Active Directory (AAD)-todennus

[Azure Active Directory] [ What is Azure Active Directory] todennus on puitteissa yhteyden muodostaminen Microsoft Azure SQL-tietovarasto avulla käyttäjätietojen Azure Active Directory (Azure AD). Azure Active Directory-todennuksen avulla voit hallita keskitetysti tietokannan käyttäjien käyttäjätietojen ja muiden Microsoftin palvelujen yhdessä keskitetyssä paikassa. Tunnus keskitetysti keskitetysti SQL-tietovarasto käyttäjien hallintaan ja yksinkertaistaa käyttöoikeuksien hallinta. 

### <a name="benefits"></a>Edut

Azure Active Directory-etuja ovat

- Sisältää SQL Server-todennus löytämiseen.
- Auttaa lopettaa käyttäjätietojen koskevia tietokannan palvelinten.
- Yhteen paikkaan kierto salasanan avulla
- Tietokannan käyttöoikeuksien ulkoisen (AAD)-ryhmien käyttäminen hallinta.
- Windows-todennusta ja muilta Azure Active Directory tukemista ottamalla jättää pois tallentamista salasanat.
- Käyttää sisälsi tietokannan käyttäjien todentamiseen käyttäjätietojen tietokanta tasolla.
- Tukee tunnuksen todennuksen sovellusten yhteyden muodostaminen SQL-tietovarasto.
- Tukee SQL Server Management Studiossa Monimenetelmäisen todentamisen Active Directoryn Yleinen todennuksen avulla. Monimenetelmäisen todentamisen kuvaus-kohdassa [SSMS tuki Azure AD-MFA SQL-tietokantaan ja SQL-tietovarasto](../sql-database/sql-database-ssms-mfa-authentication.md).

> [AZURE.NOTE] Azure Active Directory on edelleen suhteellisen uusi, ja se on joitakin rajoituksia. Varmista, että Azure Active Directory on hyvin ratkaistavaan ympäristössä, on kohdassa [Azure AD-ominaisuudet ja rajoitukset][], erityisesti huomioitava.

### <a name="configuration-steps"></a>Määritysvaiheet

Azure Active Directory käyttöoikeuksien määrittämiseen seuraavasti.

1. Luo ja täytä Azure Active Directory
2. Valinnainen: Liitä tai muuta, joka on liitetty tilauksen Azure active directory
3. Azure SQL-tietovarasto Azure Active Directory-järjestelmänvalvojan luominen
4. Asiakastietokoneiden määrittäminen
5. Käyttäjätietojen Azure AD määritetty tietokannan sisältämät tietokannan käyttäjien luominen
6. Muodosta yhteys data warehouse käyttämällä Azure AD-käyttäjätietoja

Tällä hetkellä, Azure Active Directory-käyttäjät eivät näy SSDT Object Explorer. Voit kiertää tämän ongelman tarkastella käyttäjät [sys.database_principals](https://msdn.microsoft.com/library/ms187328.aspx).
  
### <a name="find-the-details"></a>Etsi tietoja
- Suorita toimimalla seuraavasti. Yksityiskohtaisia ohjeita ja Azure Active Directory-todennusta samanlaiset lähes Azure SQL-tietokantaan ja SQL Azure-tietovarasto. Yksityiskohtainen noudattamalla artikkelissa [yhteyden muodostaminen SQL-tietokanta tai SQL tietojen varaston mukaan käyttämällä Azure Active Directory-todennusta](../sql-database/sql-database-aad-authentication.md).
- Luo mukautettu tietokannan rooleja ja käyttäjien lisääminen roolit. Valitse myöntää hajautetun roolit. Lisätietoja on artikkelissa [Tietokannan ohjelma käyttöoikeuksien käytön aloittaminen](https://msdn.microsoft.com/library/mt667986.aspx).

## <a name="next-steps"></a>Seuraavat vaiheet

Käynnistä Visual Studio ja muita sovelluksia tietojen varastossa kyselyt, on artikkelissa [kyselyn Visual Studiossa][].

<!-- Article references -->
[SQL-tietovarasto-tietokannan suojaaminen]: ./sql-data-warehouse-overview-manage-security.md
[Kysely, joka sisältää Visual Studio]: ./sql-data-warehouse-query-visual-studio.md
[What is Azure Active Directory]: ../active-directory/active-directory-whatis.md
[Azure AD-ominaisuudet ja rajoitukset]: ../sql-database/sql-database-aad-authentication.md#azure-ad-features-and-limitations
