<properties
    pageTitle="Voit tehdä hallintatehtävät, kuten järjestelmänvalvojan salasanan | Microsoft Azure"
    description="Tässä artikkelissa käsitellään yleisten hallinnollisten tehtävien suorittamiseen SQL-tietokantaan. Esimerkiksi järjestelmänvalvojan salasanan, myöntämistä ja käyttöoikeuksien poistaminen."
    services="sql-database"
    documentationCenter=""
    authors="v-shysun"
    manager="felixwu"
    editor=""
    keywords="järjestelmänvalvojan salasanan"/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="v-shysun"/>

# <a name="how-to-perform-common-administrative-tasks-such-as-resetting-admin-password-in-azure-sql-database"></a>Voit suorittaa yleisiä hallintatehtäviä, kuten järjestelmänvalvojan salasanan Azure SQL-tietokantaan
Vaiheittaiset ohjeet tämän aiheen avulla voit myöntää ja poistaa Azure SQL-tietokannan. Monipuolisemman Lisätietoja on artikkelissa:

- [Tietokantojen ja Azure SQL-tietokantaan kirjautumiset hallinta](sql-database-manage-logins.md)
- [SQL-tietokannan suojaaminen](sql-database-security.md)
- [Tietoturvakeskus SQL Server-tietokantamoduuli ja Azure SQL-tietokanta](https://msdn.microsoft.com/library/bb510589)


[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="to-reset-admin-password-for-a-logical-server"></a>Loogisen palvelimen järjestelmänvalvojan salasanan

- [Azure-portaalissa](https://portal.azure.com) **SQL-palvelimet**, valitse palvelin luettelosta ja valitse sitten **Vaihda salasana**.

## <a name="to-help-make-sure-only-authorized-ip-addresses-are-allowed-to-access-the-server"></a>Voit parantaa Varmista, että vain valtuutettujen IP-osoitteet on sallittu palvelimen käyttämiseen
- Katso [Toimintaohje: SQL-tietokantaan palomuurin asetusten määrittäminen](sql-database-configure-firewall-settings.md).

## <a name="to-create-contained-database-users-in-the-user-database"></a>Käyttäjän tietokannan sisältämät tietokannan käyttäjien luominen
- [Luo käyttäjä](https://msdn.microsoft.com/library/ms173463.aspx) -lauseella ja katso [Sisälsi tietokannan käyttäjiä - että Your tietokannan kannettavat](https://msdn.microsoft.com/library/ff929188.aspx).

## <a name="to-authenticate-contained-database-users-by-using-your-azure-active-directory"></a>Tarkista sisältämät tietokannan käyttöoikeudet käyttämällä Azure Active Directory
- On artikkelissa [yhteyden muodostaminen SQL-tietokantaan käyttämällä Azure Active Directory-todennusta](sql-database-aad-authentication.md).

## <a name="to-create-additional-logins-for-high-privileged-users-in-the-virtual-master-database"></a>Luo muita kirjautumiset suuri toimittajien käyttäjille virtual perusmuodon tietokannan
- [Luo kirjautuminen](https://msdn.microsoft.com/library/ms189751.aspx) -lauseella ja lisätietoja [tietokantojen hallinta](sql-database-manage-logins.md) ja Azure SQL-tietokantaan kirjautumiset hallinta kirjautumiset-osiossa.
