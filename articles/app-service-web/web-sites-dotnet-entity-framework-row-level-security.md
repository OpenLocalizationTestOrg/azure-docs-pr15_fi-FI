<properties
    pageTitle="Opetusohjelma: Web Appissa ja usean vuokraajan tietokanta käyttämällä kohteen Framework ja rivin käyttäjätason suojauksen"
    description="Lue, miten voit tehdä ASP.NET MVC 5 web app, jossa usean vuokraajan SQL-tietokantaan backent, kohteen Framework ja rivin käyttäjätason suojauksen."
  metaKeywords="azure asp.net mvc entity framework multi tenant row level security rls sql database"
    services="app-service\web"
    documentationCenter=".net"
    manager="jeffreyg"
  authors="tmullaney"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="04/25/2016"
    ms.author="thmullan"/>

# <a name="tutorial-web-app-with-a-multi-tenant-database-using-entity-framework-and-row-level-security"></a>Opetusohjelma: Web Appissa ja usean vuokraajan tietokanta käyttämällä kohteen Framework ja rivin käyttäjätason suojauksen

Tässä opetusohjelmassa näytetään, miten voit luoda usean vuokraajan verkkosovellukseen "[jaettuun tietokantaan, jaettu rakenteen](https://msdn.microsoft.com/library/aa479086.aspx)" vuokraajan malli-kohteen Framework ja [Rivin käyttäjätason suojauksen](https://msdn.microsoft.com/library/dn765131.aspx). Tämän mallin yhden tietokannan sisältää tietoja monta omistajien ja kunkin taulukon kunkin rivin liittyy "vuokraajan tunnus." Rivin tason Suojaustoiminnon, Azure SQL-tietokanta, uusi ominaisuus käytetään alihallinnat estää muiden henkilöiden tiedot. Tämä edellyttää vain yhden pienen muutoksen sovellukseen. Keskittäminen tietokannan itse vuokraajan access-logiikkaa RLS yksinkertaistaa sovelluksen-koodin ja vähentää riskiä tietoja vahingossa vuoto omistajien välillä.

Aloitetaan yksinkertaisen Contact Manager-sovelluksen [auth ja SQL DB ASP.NET MVP-sovelluksen luominen](web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md)ja ottaa käyttöön sovelluksen Azure-palvelu. Oikea nyt sovellus sallii kaikkien käyttäjien (alihallinnat), jos haluat nähdä kaikki yhteystiedot:

![Ennen kuin otat käyttöön RLS Contact Manager-sovellus](./media/web-sites-dotnet-entity-framework-row-level-security/ContactManagerApp-Before.png)

Vain muutamalla pientä muutoksilla on Lisää usean vuokraajan tuki niin, että käyttäjät voivat tarkastella vain yhteystietoja, jotka kuuluvat niihin.

## <a name="step-1-add-an-interceptor-class-in-the-application-to-set-the-sessioncontext"></a>Vaihe 1: Lisää keräilyaltaan-luokka-sovelluksessa voit määrittää SESSION_CONTEXT

On annettava tehdä sovelluksen muutoksen. Koska kaikki sovelluksen käyttäjät muodostavat yhteyden tietokantaan käyttäen samaa yhteysmerkkijonoa (eli saman SQL kirjautuminen), ei tällä hetkellä ei voi RLS-käytännön tietää, mitä käyttäjä, se kannattaa suodattaa. Tämä vaihtoehto on hyvin yleisiä verkkosovellusten, koska se mahdollistaa tehokkaan yhteysvarannon, mutta se tarkoittaa sitä, voit tunnistaa tietokannan nykyisen sovelluksen käyttäjän annettava. Ratkaisu on, että sovellus avain-arvo-pari määrittäminen nykyisen käyttäjätunnus- [SESSION_CONTEXT](https://msdn.microsoft.com/library/mt590806) heti, kun olet avannut yhteyden, ennen kuin se suorittaa kaikki kyselyt. SESSION_CONTEXT on istunnon kohdistettu avain-arvo-kaupan ja RLS tämän käytännön avulla tunnistaa nykyisen käyttäjän käyttäjätunnus, se tallennetaan.

Lisäämme [keräilyaltaan](https://msdn.microsoft.com/data/dn469464.aspx) (erityisesti [DbConnectionInterceptor](https://msdn.microsoft.com/library/system.data.entity.infrastructure.interception.idbconnectioninterceptor)), uusi ominaisuus-kohteen Framework (EF) 6, jos haluat määrittää automaattisesti nykyisen käyttäjätunnus SESSION_CONTEXT T-SQL-lause suoritetaan aina, kun EF Avaa yhteyden.

1.  Avaa projekti, ContactManager Visual Studiossa.
2.  Solution Explorerissa Mallit-kansion hiiren kakkospainikkeella ja valitse Lisää > luokka.
3.  "SessionContextInterceptor.cs" uuden luokan nimi ja valitse Lisää.
4.  Korvaa SessionContextInterceptor.cs sisällön seuraava koodi.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Web;
using System.Data.Common;
using System.Data.Entity;
using System.Data.Entity.Infrastructure.Interception;
using Microsoft.AspNet.Identity;

namespace ContactManager.Models
{
    public class SessionContextInterceptor : IDbConnectionInterceptor
    {
        public void Opened(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
            // Set SESSION_CONTEXT to current UserId whenever EF opens a connection
            try
            {
                var userId = System.Web.HttpContext.Current.User.Identity.GetUserId();
                if (userId != null)
                {
                    DbCommand cmd = connection.CreateCommand();
                    cmd.CommandText = "EXEC sp_set_session_context @key=N'UserId', @value=@UserId";
                    DbParameter param = cmd.CreateParameter();
                    param.ParameterName = "@UserId";
                    param.Value = userId;
                    cmd.Parameters.Add(param);
                    cmd.ExecuteNonQuery();
                }
            }
            catch (System.NullReferenceException)
            {
                // If no user is logged in, leave SESSION_CONTEXT null (all rows will be filtered)
            }
        }
        
        public void Opening(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
        }

        public void BeganTransaction(DbConnection connection, BeginTransactionInterceptionContext interceptionContext)
        {
        }

        public void BeginningTransaction(DbConnection connection, BeginTransactionInterceptionContext interceptionContext)
        {
        }

        public void Closed(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
        }

        public void Closing(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
        }

        public void ConnectionStringGetting(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void ConnectionStringGot(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void ConnectionStringSet(DbConnection connection, DbConnectionPropertyInterceptionContext<string> interceptionContext)
        {
        }

        public void ConnectionStringSetting(DbConnection connection, DbConnectionPropertyInterceptionContext<string> interceptionContext)
        {
        }

        public void ConnectionTimeoutGetting(DbConnection connection, DbConnectionInterceptionContext<int> interceptionContext)
        {
        }

        public void ConnectionTimeoutGot(DbConnection connection, DbConnectionInterceptionContext<int> interceptionContext)
        {
        }

        public void DataSourceGetting(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void DataSourceGot(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void DatabaseGetting(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void DatabaseGot(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void Disposed(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
        }

        public void Disposing(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
        }

        public void EnlistedTransaction(DbConnection connection, EnlistTransactionInterceptionContext interceptionContext)
        {
        }

        public void EnlistingTransaction(DbConnection connection, EnlistTransactionInterceptionContext interceptionContext)
        {
        }

        public void ServerVersionGetting(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void ServerVersionGot(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void StateGetting(DbConnection connection, DbConnectionInterceptionContext<System.Data.ConnectionState> interceptionContext)
        {
        }

        public void StateGot(DbConnection connection, DbConnectionInterceptionContext<System.Data.ConnectionState> interceptionContext)
        {
        }
    }

    public class SessionContextConfiguration : DbConfiguration
    {
        public SessionContextConfiguration()
        {
            AddInterceptor(new SessionContextInterceptor());
        }
    }
}
```

Tämä on pakollinen vain sovelluksen muuttaminen. Siirry eteenpäin ja luoda ja julkaista sovellus.

## <a name="step-2-add-a-userid-column-to-the-database-schema"></a>Vaihe 2: Lisää tietokantarakenteen käyttäjätunnus-sarake

Seuraavaksi annettava käyttäjätunnus-sarakkeen lisääminen yhteystiedot-taulukon kunkin rivin liitettävä käyttäjä (Alihallinta). Olemme muuttavat rakenteen suoraan tietokannassa, niin, että ei tarvitse Sisällytä tämä kenttä EF Microsoftin tietomallissa.

Muodosta yhteys tietokantaan suoraan, SQL Server Management Studiossa tai Visual Studio ja suorita seuraavassa T-SQL:

```
ALTER TABLE Contacts ADD UserId nvarchar(128)
    DEFAULT CAST(SESSION_CONTEXT(N'UserId') AS nvarchar(128))
```

Tämä lisää käyttäjätunnus sarakkeen Yhteystiedot-taulukkoon. Käytämme nvarchar(128) tietotyypin vastaamaan USERID AspNetUsers-taulukkoon ja luodaan oletusrajoitus, joka määrittää automaattisesti juuri lisättyjen rivien käyttäjätunnus on tallennettu SESSION_CONTEXT käyttäjätunnus.

Taulukko näyttää nyt tältä:

![SSMS Yhteystiedot-taulukkoon](./media/web-sites-dotnet-entity-framework-row-level-security/SSMS-Contacts.png)

Luotaessa uusia yhteyshenkilöitä heidät automaattisesti on määritetty oikea käyttäjätunnus. Esittely tarkoituksiin kuitenkin japanin määrittää nämä olemassa olevat yhteystiedot suoritettavien jo olemassa olevalle käyttäjälle.

Jos olet luonut muutamalla käyttäjällä jo sovelluksessa (esimerkiksi paikallisia, Google- tai Facebook tilit), ne näkyvät AspNetUsers taulukon. Alla olevassa näyttökuvassa on vain yksi käyttäjä tähän mennessä.

![SSMS AspNetUsers taulukko](./media/web-sites-dotnet-entity-framework-row-level-security/SSMS-AspNetUsers.png)

Kopioi tunnuksen user1@contoso.com, ja liitä se alla T-SQL-lause. Suorita tämä lause kolme yhteystiedot liitettävä käyttäjätunnus.

```
UPDATE Contacts SET UserId = '19bc9b0d-28dd-4510-bd5e-d6b6d445f511'
WHERE ContactId IN (1, 2, 5)
```

## <a name="step-3-create-a-row-level-security-policy-in-the-database"></a>Vaihe 3: Luo rivin tason suojauskäytäntö tietokannassa

Viimeinen vaihe on luotava suojauskäytäntö, joka käyttää SESSION_CONTEXT käyttäjätunnus palauttama kyselyitä tulosten suodattamiseen automaattisesti.

Kun edelleen yhteydessä tietokantaan, suorita seuraavat T-SQL:

```
CREATE SCHEMA Security
go

CREATE FUNCTION Security.userAccessPredicate(@UserId nvarchar(128))
    RETURNS TABLE
    WITH SCHEMABINDING
AS
    RETURN SELECT 1 AS accessResult
    WHERE @UserId = CAST(SESSION_CONTEXT(N'UserId') AS nvarchar(128))
go

CREATE SECURITY POLICY Security.userSecurityPolicy
    ADD FILTER PREDICATE Security.userAccessPredicate(UserId) ON dbo.Contacts,
    ADD BLOCK PREDICATE Security.userAccessPredicate(UserId) ON dbo.Contacts
go

```

Tämä koodi ei kolme asiaa. Ensimmäisen kerran se luo uuden rakenteen parhaana käytäntönä keskittäminen ja pääsyä RLS objektit. Seuraavaksi Luo predikaatin funktiota, joka palauttaa '1', kun rivin käyttäjätunnus vastaa SESSION_CONTEXT-käyttäjätunnus. Lopuksi Luo suojauskäytäntö, joka lisää tämän funktion suodatus- ja estä lause yhteystiedot-taulukon. Suodatin-lause aiheuttaa kyselyjä palauttaa vain ne rivit, jotka kuuluvat nykyisen käyttäjän ja estä-lause toimii suojaavat estää sovelluksen lisääminen koskaan vahingossa väärän käyttäjän rivin.

Suorita sovellus ja kirjaudu user1@contoso.com. Tämä käyttäjä näkee nyt vain on määritetty käyttäjätunnus aiemmin yhteystiedot:

![Ennen kuin otat käyttöön RLS Contact Manager-sovellus](./media/web-sites-dotnet-entity-framework-row-level-security/ContactManagerApp-After.png)

Voit tarkistaa tämän edelleen rekisteröiminen uusi käyttäjä. He näkevät yhteystietoja ei ole, koska ei ole määritetty niihin. Jos hän luo uusi yhteystieto-määritetään niihin ja vain he voivat nähdä tiedot.

## <a name="next-steps"></a>Seuraavat vaiheet

Joka on tämä! Yksinkertainen Contact Managerin web Appissa on muunnettu usean vuokraajan jotakin kohtaa, johon kullakin käyttäjällä on oma yhteystietoluetteloon. Rivin käyttäjätason suojauksen käyttämällä Microsoft olet välttää monimutkaisuutta pakotetut vuokraajan access-funktioiden sovelluksen koodia. Tämä läpinäkyvyys sallii sovelluksen keskitytään todellisiin business ongelman mukaan, ja se myös pienentää riskiä vuoda vahingossa tiedot, kun sovellus on codebase kasvaa.

Tässä opetusohjelmassa on vain naarmu pinta tarjoamiin mahdollisuuksiin RLS. Esimerkiksi ei ole Lisää kehittyneitä tai hajautetun access-funktioiden ja käyttäjän voi tallentaa enemmän kuin vain nykyisen käyttäjätunnus SESSION_CONTEXT. Se on mahdollista [integroida RLS joustavasti tietokannan Työkalut asiakas-kirjastoja](../sql-database/sql-database-elastic-tools-multi-tenant-row-level-security.md) myös tukemaan usean vuokraajan shards skaalaus-kohtaa tietojen taso.

Lisäksi seuraavia vaihtoehtoja pyrimme myös parhaillaan tehdä RLS entistä paremmin. Jos sinulla on kysyttävää, ideoita tai kohteita, jotka haluat nähdä, kerro meille kommenttien. Arvostamme antamaasi palautetta!
