<properties
   pageTitle="SSMS tuki Azure AD-MFA SQL-tietokantaan ja SQL-tietovarasto | Microsoft Azure"
   description="Usean Monimenetelmäinen todentaminen käyttäminen SSMS SQL-tietokantaan ja SQL-tietovarasto."
   services="sql-database"
   documentationCenter=""
   authors="BYHAM"
   manager="jhubbard"
   editor=""
   tags=""/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="10/04/2016"
   ms.author="rick.byham@microsoft.com"/>

# <a name="ssms-support-for-azure-ad-mfa-with-sql-database-and-sql-data-warehouse"></a>Azure AD-MFA SQL-tietokantaan ja SQL-tietovarasto SSMS tuki

Azure SQL-tietokantaan ja SQL Azure-tietovarasto tukevat SQL Server Management Studiossa (SSMS)-yhteyksien *Active Directory*-yleinen todennusta. Active Directory yleinen todennus on vuorovaikutteinen työnkulku, joka tukee *Azure Monimenetelmäisen todentamisen* (MFA). Azure MFA auttaa suojaavat käyttämään tietoja ja sovelluksia käyttäjän tarve-kirjautuminen yksinkertaista kokouksen aikana. Se toimittaa vahva todennus helposti vahvistus asetukset-alue, puhelu, tekstiviesti, älykortit PIN-tunnus tai mobiilisovelluksessa ilmoituksen – sallia käyttäjien valita menetelmä haluamansa. Monimenetelmäisen todentamisen kuvaus-kohdassa [Monimenetelmäisen todentamisen](../multi-factor-authentication/multi-factor-authentication.md).

SSMS tukee nyt:

- Vuorovaikutteinen MFA Azure AD kanssa mahdollisuuksia avautuvassa valintaikkunassa kelpoisuustarkistus, kanssa.
- Ei-vuorovaikutteinen Active Directory-salasanaa ja Active Directory-todennusta menetelmiä, joita voi käyttää useita eri sovelluksissa (ADO.NET, JDBC, ODBC jne.). Nämä kaksi menetelmää johtaa koskaan ponnahdusikkunat.

Jos käyttäjätili on määritetty MFA vuorovaikutteinen todennus työnkulku edellyttää muita käyttäjän toimia ponnahdusikkunat, älykortin käyttämistä, jne. Jos käyttäjätili on määritetty MFA, käyttäjän on valittava Azure yleinen todennus muodostaa. Jos käyttäjätili ei edellytä MFA-käyttäjän edelleen käyttää muita kaksi Azure Active Directory-todennus-vaihtoehtoa.

## <a name="universal-authentication-limitations-for-sql-database-and-sql-data-warehouse"></a>Yleinen SQL-tietokantaan ja SQL-tietovarasto käyttöoikeuksien rajoitukset

- SSMS on MFA Active Directoryn Yleinen todennus – tällä hetkellä käytössä vain työkalu.
- Vain yhden Azure Active Directory-tilin voit kirjata esiintymän SSMS yleinen todennusta. Kirjaudu sisään toisen Azure AD-tilin, sinun on käytettävä SSMS esiintymässä. (Tämä rajoitus on rajoitettu Active Directoryn Yleinen todennusta, voit kirjautua eri palvelimissa, käyttämällä Active Directory-salasanan todennus, Active Directory-todennusta tai SQL Server-todennus).
- SSMS tukee Active Directoryn Yleinen todennus Object Explorer kyselyeditori ja kyselyn kaupan visualisointi.
- DacFx eikä rakenteen suunnittelu tue yleinen todennusta.
- MSA tilit ei tue Active Directoryn Yleinen todennusta varten.
- Active Directory yleinen todennusta ei tueta SSMS käyttäjille, jotka tuodaan nykyisen Active Directory muiden Azure Active kansioista. Nämä käyttäjät ei tueta, koska se edellyttäisi vuokraajan tunnus, Vahvista tilit ja antamaan, mikään toiminto ei.
- On muita ohjelmistovaatimukset, Active Directoryn Yleinen todennuksen lukuun ottamatta sitä, että sinun on käytettävä tuettua SSMS-versiota.

## <a name="configuration-steps"></a>Määritysvaiheet

Monimenetelmäisen todentamisen toteuttaminen edellyttää neljästä vaiheesta.

1. **Määritä Azure Active Directory** – Katso lisätietoja- [integrointi paikallisen-käyttäjätietojen Azure Active Directory-hakemistosta](../active-directory/active-directory-aadconnect.md), [Lisää oman toimialuenimen, Azure AD](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/), [tukee nyt Microsoft Azure Windows Server Active Directory federation](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/), [hallinta Azure AD-kansio](https://msdn.microsoft.com/library/azure/hh967611.aspx)ja [hallinta Windows PowerShellin Azure AD](https://msdn.microsoft.com/library/azure/jj151815.aspx).

2. **Määritä MFA** – vaiheittaiset ohjeet ovat kohdassa [Määrittäminen Azure Monimenetelmäisen todentamisen](../multi-factor-authentication/multi-factor-authentication-whats-next.md). 

3. **Määritä SQL-tietokanta tai SQL Azure AD-todennuksessa tietovarasto** – vaiheittaiset ohjeet on artikkelissa [yhteyden muodostaminen SQL-tietokanta tai SQL tietojen varaston mukaan käyttämällä Azure Active Directory-todennusta](sql-database-aad-authentication.md).

4. **Lataa SSMS** – asiakastietokoneen, lataa uusin SSMS (vähintään elokuussa 2016), valitse [Lataa SQL Server Management Studiossa (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx).

## <a name="connecting-by-using-universal-authentication-with-ssms"></a>Yhteyden muodostaminen käyttämällä SSMS yleinen todennus

Seuraavissa vaiheissa kuvataan yhdistämisestä SQL-tietokanta tai SQL-tietovarasto uusimman SSMS avulla.

1. Yhteyden muodostaminen yleinen käyttöoikeuksien **liittäminen Serveriin** -valintaikkunassa, valitse **Active Directoryn Yleinen todennusta**.
![Yleinen 1mfa yhdistäminen][1]

2. SQL-tietokantaan ja SQL-tietovarasto tavalliseen tapaan, valitse **asetukset** ja määritä tietokanta- **asetukset** -valintaikkunan. Valitse **Yhdistä**.
3. **Tiliisi kirjautuminen** -valintaikkuna tulee näkyviin, kun on tili ja salasana Azure Active Directory-henkilöllisyytesi.
![2mfa-kirjautuminen][2]

    > [AZURE.NOTE] Yleinen todennustavaksi tilillä, joka ei edellytä MFA Yhdistä tässä vaiheessa. Käyttäjien edellyttävän MFA Jatka seuraavien ohjeiden mukaisesti.
 
4. Kaksi MFA asetukset-valintaikkunaa ehkä näy. Tämän kerran toiminnon määrittäminen MFA järjestelmänvalvojan määräytyy ja siksi voidaan valinnainen. MFA käytössä toimialueen tämä vaihe on joskus ennalta määritetyt (esimerkiksi toimialueen edellyttää käyttäjät voivat käyttää älykortin ja PIN-tunnus).  
![3mfa asetukset][3]

5. Toinen mahdollista kerran valintaikkunan avulla voit valita, että todentamismenetelmän tietoja. Mahdolliset asetukset on määritetty järjestelmänvalvoja.
![4mfa-Tarkista 1][4]
 
6. Azure Active Directory lähettää sinulle vahvistuksen tiedot. Kun saat tarkistuskoodi, kirjoita se ruutuun **Enter tarkistuskoodi** ja valitse **Kirjaudu sisään**.
![5mfa-Tarkista 2][5]

Kun tarkistus on tehty, SSMS muodostaa aihe tavallisesti kelvolliset tunnistetiedot ja palomuuri käytön.

##<a name="next-steps"></a>Seuraavat vaiheet  

Tietokannan käyttöoikeuden myöntäminen muiden: [SQL-tietokannan todennus- ja: myöntämistä Access](sql-database-manage-logins.md)  
Varmista, että muut yhteyden palomuurin läpi: [Määritä Azure-portaalissa Azure SQL-tietokanta on palvelintason palomuurisääntö](sql-database-configure-firewall-settings.md)


[1]: ./media/sql-database-ssms-mfa-auth/1mfa-universal-connect.png
[2]: ./media/sql-database-ssms-mfa-auth/2mfa-sign-in.png
[3]: ./media/sql-database-ssms-mfa-auth/3mfa-setup.png
[4]: ./media/sql-database-ssms-mfa-auth/4mfa-verify-1.png
[5]: ./media/sql-database-ssms-mfa-auth/5mfa-verify-2.png

