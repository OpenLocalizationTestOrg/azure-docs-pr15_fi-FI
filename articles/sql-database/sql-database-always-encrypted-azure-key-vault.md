<properties
    pageTitle="Aina salattu: Luottamuksellisten tietojen Azure SQL-tietokantaan tietokannan salaus suojaaminen | Microsoft Azure"
    description="Suojaa luottamukselliset tiedot SQL-tietokantaan minuutteina."
    keywords="salauksen, salausavaimen cloud salaus"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor="cgronlun"/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/18/2016"
    ms.author="sstein"/>

# <a name="always-encrypted-protect-sensitive-data-in-sql-database-and-store-your-encryption-keys-in-azure-key-vault"></a>Aina salattu: Luottamuksellisten tietojen SQL-tietokantaan ja tallentaa salausavaimet Azure avain säilöön

> [AZURE.SELECTOR]
- [Azure avaimen säilö](sql-database-always-encrypted-azure-key-vault.md)
- [Varmenteen Windows-kaupan](sql-database-always-encrypted.md)


Tässä artikkelissa kerrotaan, miten luottamuksellisia tietoja SQL-tietokannan suojaaminen salauksen käyttämällä [Aina salattu ohjattu](https://msdn.microsoft.com/library/mt459280.aspx) [SQL Server Management Studiossa (SSMS)](https://msdn.microsoft.com/library/hh213248.aspx). Se on myös ohjeet, joissa kerrotaan, kuinka kunkin salausavaimen tallennuspaikoista Azure avaimen säilö.

Salattu on aina uutta tietojen salaustekniikkaa Azure SQL-tietokantaan ja SQL Server, joka auttaa suojaamaan arkaluontoisia tietoja muiden palvelimessa, aikana asiakkaan ja palvelimen välillä, ja kun tiedot on käytössä. Salattu aina varmistaa, että pelkkänä tekstinä tietokannan järjestelmän sisällä arkaluontoisia tietoja koskaan näkyy. Kun määrität salauksen, vain asiakassovelluksissa tai sovelluksen palvelimet, joilla on oikeus näppäimet voi käyttää tekstimuotoisia tietoja. Lisätietoja on artikkelissa [Salattu aina (tietokantaohjelma)](https://msdn.microsoft.com/library/mt163865.aspx).


Kun olet määrittänyt käytettävä aina salattu tietokanta, luodaan asiakassovellus C# Visual Studiossa salattujen tietojen käyttöä varten.

Noudattamalla tämän artikkelin ja lue, miten voit määrittää aina salattu Azure SQL-tietokantaan. Tässä artikkelissa kerrotaan, miten voit suorittaa seuraavia tehtäviä:

- Luo [aina salattu avaimet](https://msdn.microsoft.com/library/mt163865.aspx#Anchor_3)SSMS aina salattu ohjatun toiminnon avulla.
    - Luo [sarakkeen avaimen (CMK)](https://msdn.microsoft.com/library/mt146393.aspx).
    - Luo [sarakkeen salausavaimen (CEK)](https://msdn.microsoft.com/library/mt146372.aspx).
- Luoda tietokantataulukon ja salata sarakkeet.
- Luo sovellus, joka lisää, valitsee ja näyttää salattuja sarakkeiden tiedot.


## <a name="prerequisites"></a>Edellytykset

Tässä opetusohjelmassa, sinun on:

- Azure-tili ja tilauksen. Jos sinulla ei ole yhtä tilaamaan [maksuttoman kokeiluversion käyttäjäksi](https://azure.microsoft.com/pricing/free-trial/).
- [SQL Server Management Studiossa](https://msdn.microsoft.com/library/mt238290.aspx) versio 13.0.700.242 tai uudempi versio.
- [.NET framework 4.6](https://msdn.microsoft.com/library/w0x726c2.aspx) tai uudempi (asiakastietokoneen).
- [Visual Studio](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).
- [PowerShellin azure](../powershell-install-configure.md)-1.0 tai uudempi versio. Kirjoita **(Get-moduulin azure - ListAvailable). Version** näet, mitä PowerShell-versio sinulla on käytössä.



## <a name="enable-your-client-application-to-access-the-sql-database-service"></a>SQL-tietokanta-palvelua asiakassovellus ottaminen käyttöön

Sinun on otettava asiakassovellus käyttää SQL-tietokanta-palvelun vaadittavaa käyttöoikeuksien määrittämisestä ja *ClientId* ja *salainen* , joita tarvitset seuraava koodi sovelluksen tarkistamiseen.

1. Avaa [Azure perinteinen portal](http://manage.windowsazure.com).
2. **Active Directory** ja valitse sitten, sovellus käyttää Active Directory-esiintymä.
3. Valitse **sovellukset**ja valitse sitten **Lisää**.
4. Kirjoita sovelluksesi nimi (esimerkiksi: *myClientApp*), valitse **VERKKOSOVELLUS**ja Jatka napsauttamalla nuolta.
5. **KIRJAUDU edelleen URL-osoite** ja **Sovelluksen tunnus URI** kelvollinen URL-osoite (esimerkiksi *http://myClientApp*) ja jatka.
6. Valitse **Määritä**.
7. Kopioi omaan **Asiakastunnus**. (Tarvitset tätä arvoa koodissa myöhemmin.)
8. Valitse **näppäimet** -kohdan **1 vuosi** **Valitse kesto** avattavasta luettelosta. (Voit kopioida avaimen Tallennettuasi vaiheessa 14.)
11. Vieritä alaspäin ja valitse **Lisää sovellus**.
12. Jätä **Näytä** arvoksi **Microsoft-sovellukset** ja valitse sitten **Microsoft Azure hallinta**. Valitse Jatka valintamerkki.
13. Valitse **Access Azure hallinnan** **Valtuutetun käyttöoikeuksien** avattavasta luettelosta.
14. Valitse **Tallenna**.
15. Kun tallennus on tehty, kopioi avainarvon **näppäimet** -osassa. (Tarvitset tätä arvoa koodissa myöhemmin.)



## <a name="create-a-key-vault-to-store-your-keys"></a>Luo tallentamiseen avaimien avaimen säilö

Asiakas-sovellus on määritetty ja sinulla on Asiakastunnus, on aika, voit luoda avaimen säilö ja määrittää sen käyttöoikeuskäytäntö, jotta sinä ja sovelluksen voit käyttää tietoja säilö (salattu aina näppäimet). *Luo*, *Saat* *luettelon*, *Kirjaudu*, *Tarkista*, *wrapKey*tai *unwrapKey* käyttöoikeudet ovat uuden sarakkeen perustyyli avaimen luomiseen ja salaus, SQL Server Management Studiossa määrittämisestä.

Voit luoda nopeasti avaimen säilö suorittamalla seuraavaa komentosarjaa. Lisätietoja näiden cmdlet-komennot ja haluat lisätietoja luominen ja määrittäminen avaimen säilö tarkan [Azure avaimen säilö käytön aloittaminen](../Key-Vault/key-vault-get-started.md).



    $subscriptionName = '<your Azure subscription name>'
    $userPrincipalName = '<username@domain.com>'
    $clientId = '<client ID that you copied in step 7 above>'
    $resourceGroupName = '<resource group name>'
    $location = '<datacenter location>'
    $vaultName = 'AeKeyVault'


    Login-AzureRmAccount
    $subscriptionId = (Get-AzureRmSubscription -SubscriptionName $subscriptionName).SubscriptionId
    Set-AzureRmContext -SubscriptionId $subscriptionId

    New-AzureRmResourceGroup –Name $resourceGroupName –Location $location
    New-AzureRmKeyVault -VaultName $vaultName -ResourceGroupName $resourceGroupName -Location $location

    Set-AzureRmKeyVaultAccessPolicy -VaultName $vaultName -ResourceGroupName $resourceGroupName -PermissionsToKeys create,get,wrapKey,unwrapKey,sign,verify,list -UserPrincipalName $userPrincipalName
    Set-AzureRmKeyVaultAccessPolicy  -VaultName $vaultName  -ResourceGroupName $resourceGroupName -ServicePrincipalName $clientId -PermissionsToKeys get,wrapKey,unwrapKey,sign,verify,list




## <a name="create-a-blank-sql-database"></a>Tyhjä SQL-tietokannan luominen
1. Kirjautuminen [Azure portal](https://portal.azure.com/).
2. Valitse **Uusi** > **tietojen + tallennustilan** > **SQL-tietokantaan**.
3. Luo **Tyhjä** tietokanta nimeltä **kurssi** uuteen tai aiemmin luotuun palvelimessa. Saat yksityiskohtaiset ohjeet siitä, miten voit luoda tietokannan Azure-portaalissa, [Luo minuutteina SQL-tietokanta](sql-database-get-started.md).

    ![Luo tyhjä tietokanta](./media/sql-database-always-encrypted-azure-key-vault/create-database.png)

Sinun on yhteys myöhemmin-opetusohjelman merkkijono, niin kun olet luonut tietokannan, siirry kurssi uuden tietokannan ja kopioi yhteysmerkkijono. Voit hankkia yhteysmerkkijonon milloin tahansa, mutta se on helppo kopioida Azure-portaalissa.

1. Valitse **SQL-tietokantoja** > **kurssi** > **Näytä tietokannan yhteysmerkkijonoja**.
2. Kopioi yhteysmerkkijono **ADO.NET**.

    ![Kopioi yhteysmerkkijono](./media/sql-database-always-encrypted-azure-key-vault/connection-strings.png)


## <a name="connect-to-the-database-with-ssms"></a>Muodosta yhteys tietokantaan SSMS kanssa

Avaa SSMS ja muodostaa yhteyttä palvelimeen kurssi tietokannassa.


1. Avaa SSMS. (Siirry **Yhdistä** > **Tietokantamoduuli** Avaa **Muodosta yhteys palvelimeen** -ikkuna, jos se ei ole avoinna.)
2. Kirjoita palvelimen nimi ja tunnistetiedot. Palvelimen nimi löytyy SQL-tietokanta-sivu ja yhteysmerkkijonon aiemmin kopioitu. Kirjoita palvelimen täydellinen nimi, kuten *database.windows.net*.

    ![Kopioi yhteysmerkkijono](./media/sql-database-always-encrypted-azure-key-vault/ssms-connect.png)

Jos **Palomuurin uusi sääntö** -ikkuna avautuu Azure kirjautuminen ja anna SSMS uuden palomuurisäännön luominen puolestasi.


## <a name="create-a-table"></a>Taulukon luominen

Tässä osiossa luot taulukko sisältää Potilas tietoja. Se ei ole alun perin salattu--määrität salaus seuraavan osion.

1. Laajenna **tietokannat**.
1. Napsauta **kurssi** tietokanta ja valitse **Uusi kysely**.
2. Liitä seuraava Transact-SQL (T-SQL) uusi kyselyikkuna ja **Suorita** se.


        CREATE TABLE [dbo].[Patients](
         [PatientId] [int] IDENTITY(1,1),
         [SSN] [char](11) NOT NULL,
         [FirstName] [nvarchar](50) NULL,
         [LastName] [nvarchar](50) NULL,
         [MiddleName] [nvarchar](50) NULL,
         [StreetAddress] [nvarchar](50) NULL,
         [City] [nvarchar](50) NULL,
         [ZipCode] [char](5) NULL,
         [State] [char](2) NULL,
         [BirthDate] [date] NOT NULL
         PRIMARY KEY CLUSTERED ([PatientId] ASC) ON [PRIMARY] );
         GO


## <a name="encrypt-columns-configure-always-encrypted"></a>Salaa sarakkeet (määrittäminen aina salattu)

SSMS sisältää ohjatun, jonka avulla voit määrittää helposti sarakkeen avaimen, sarakkeen salausavaimen ja salattuja sarakkeiden määrittäminen puolestasi aina salattu.

1. Laajenna **Databases** > **kurssi** > **taulukot**.
2. **Potilaille** taulukkoa hiiren kakkospainikkeella ja valitse **Salaa sarakkeet** ja Avaa ohjattu aina salattu:

    ![Salaa sarakkeet](./media/sql-database-always-encrypted-azure-key-vault/encrypt-columns.png)

Salattu aina ohjattua sisältää seuraavat osat: **Sarakkeen valinta**, **Master Key määritykset**, **kelpoisuustarkistus**ja **Yhteenveto**.

### <a name="column-selection"></a>Sarakkeen valitseminen##

**Johdanto** sivulla Avaa **Sarakkeen valinta** -sivu valitsemalla **Seuraava** . Tällä sivulla voit valita näytettävät sarakkeet, jonka haluat salata, [Suojaus-tyypin ja mitä sarakkeen salausavain (CEK)](https://msdn.microsoft.com/library/mt459280.aspx#Anchor_2) käyttämään.

Salaa kunkin potilaan **Henkilötunnusta** ja **Syntymäpv-kenttien** tiedot. Sosiaaliturvatunnus-sarakkeessa käytetään deterministic salausta, joka tukee tasa haut, liitokset ja Ryhmittele. Syntymäpäivä-sarakkeessa käytetään satunnaisesti salausta, joka ei tue toimintoja.

Määritä **Salaustyyppi** Henkilötunnusta sarakkeen **Deterministic** ja Syntymäpv-sarake kohtaan **Randomized**. Valitse **Seuraava**.

![Salaa sarakkeet](./media/sql-database-always-encrypted-azure-key-vault/column-selection.png)

### <a name="master-key-configuration"></a>Avaimen määritys###

**Master Key määritys** -sivulla ei, jos oman CMK määrittäminen ja valitse avaimen säilöpalvelu mihin CMK tallennetaan. Tällä hetkellä voi tallentaa CMK sertifikaatin Windows-kaupasta, Azure avaimen säilö tai laitteiston (:: HSM)-moduulissa.

Tässä opetusohjelmassa näytetään, miten voit tallentaa avaimien Azure avaimen säilö.

1.     Valitse **Azure avaimen säilö**.
1.     Valitse avattavasta luettelosta haluamasi avaimen säilö.
1.     Valitse **Seuraava**.

![Avaimen määritys](./media/sql-database-always-encrypted-azure-key-vault/master-key-configuration.png)


### <a name="validation"></a>Kelpoisuustarkistus###

Voit salata sarakkeet nyt tai tallentaa PowerShell-komentosarjaa myöhemmin. Tässä opetusohjelmassa, valitse **Jatka Viimeistele nyt** ja valitse **Seuraava**.

### <a name="summary"></a>Yhteenveto ###

Varmista, että asetukset ovat kaikki korjaa ja Viimeistele määritykset saat aina salattu **Valmis** .


![Yhteenveto](./media/sql-database-always-encrypted-azure-key-vault/summary.png)


### <a name="verify-the-wizards-actions"></a>Varmista ohjatun toiminnon toiminnot

Kun ohjattu toiminto on valmis, tietokannan määritetään aina salattu. Ohjatun toiminnon suorittaa seuraavia toimintoja:

- Luotu sarakkeen perustyyli-näppäintä ja se tallennetaan Azure avaimen säilö.
- Luotu sarakkeen salausavaimen ja se tallennetaan Azure avaimen säilö.
- Määritetty salauksen valitut sarakkeet. Potilaille taulukon tällä hetkellä ei ole tietoja, mutta valituissa sarakkeissa mahdollisesti olevat tiedot ovat nyt salattuja.

Voit tarkistaa SSMS avaimet luominen laajentamalla **kurssi** > **suojauksen** > **Aina salattu avaimet**.


## <a name="create-a-client-application-that-works-with-the-encrypted-data"></a>Luo asiakassovellus, joka toimii salatut tiedot

Nyt kun aina salattu on määritetty, voit luoda sovellus, joka suorittaa *Lisää* ja *valitsee* salattuja sarakkeet.  

> [AZURE.IMPORTANT] Sovelluksen on käytettävä [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx) objekteja, kun siirtämällä tekstimuotoisia tietoja aina salattu sarakkeiden palvelimeen. Poikkeuksen johtaa kulkeva literaaliarvot käyttämättä SqlParameter objekteja.

1. Avaa Visual Studio ja luo uusi C# console-sovellus. Varmista, että projektin on määritetty **.NET Framework 4.6** tai uudempi versio.
2. Projektin **AlwaysEncryptedConsoleAKVApp** nimeä ja valitse **OK**.
![Uusi console-sovellus](./media/sql-database-always-encrypted-azure-key-vault/console-app.png)
3. Asenna seuraavat NuGet paketit siirtymällä kohtaan **Työkalut** > **NuGet pakettien hallinta** > **Paketin hallinta-konsolin**.

Suorita nämä kaksi riviä koodin pakettien hallinta-konsolissa.

    Install-Package Microsoft.SqlServer.Management.AlwaysEncrypted.AzureKeyVaultProvider
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory



## <a name="modify-your-connection-string-to-enable-always-encrypted"></a>Muokkaa yhteysmerkkijono salattu aina käyttöön

Tässä osassa kerrotaan ottamisesta käyttöön aina salattu tietokanta yhteysmerkkijonossa.


Salattu aina käyttöön tarvitset lisää **Sarake salaus asetus** -avainsana yhteysmerkkijono ja määritä se **otetaan käyttöön**.

Voit määrittää tämän suoraan yhteysmerkkijonossa tai voit määrittää sen [SqlConnectionStringBuilder](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.aspx)avulla. Sovelluksen seuraavan osion malli näkyy **SqlConnectionStringBuilder**käyttämisestä.



### <a name="enable-always-encrypted-in-the-connection-string"></a>Ota yhteysmerkkijonon aina salattu

Lisää seuraava avainsana yhteysmerkkijono.

    Column Encryption Setting=Enabled


### <a name="enable-always-encrypted-with-sqlconnectionstringbuilder"></a>Ota aina salattu SqlConnectionStringBuilder

Seuraava koodi näkyy ottamisesta käyttöön määrittämällä [SqlConnectionStringBuilder.ColumnEncryptionSetting](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.columnencryptionsetting.aspx) [käytössä](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectioncolumnencryptionsetting.aspx)aina salattu.

    // Instantiate a SqlConnectionStringBuilder.
    SqlConnectionStringBuilder connStringBuilder =
       new SqlConnectionStringBuilder("replace with your connection string");

    // Enable Always Encrypted.
    connStringBuilder.ColumnEncryptionSetting =
       SqlConnectionColumnEncryptionSetting.Enabled;

## <a name="register-the-azure-key-vault-provider"></a>Rekisteröi Azure avain säilöön-palvelu

Seuraava koodi esitetään, kuinka voit rekisteröidä Azure avaimen säilö toimittajan ADO.NET-ohjain.

    private static ClientCredential _clientCredential;

    static void InitializeAzureKeyVaultProvider()
    {
       _clientCredential = new ClientCredential(clientId, clientSecret);

       SqlColumnEncryptionAzureKeyVaultProvider azureKeyVaultProvider =
          new SqlColumnEncryptionAzureKeyVaultProvider(GetToken);

       Dictionary<string, SqlColumnEncryptionKeyStoreProvider> providers =
          new Dictionary<string, SqlColumnEncryptionKeyStoreProvider>();

       providers.Add(SqlColumnEncryptionAzureKeyVaultProvider.ProviderName, azureKeyVaultProvider);
       SqlConnection.RegisterColumnEncryptionKeyStoreProviders(providers);
    }



## <a name="always-encrypted-sample-console-application"></a>Aina salattu konsolisovelluksen malli

Tässä esimerkissä näytetään, miten voit:

- Muokkaa yhteysmerkkijono käyttöön aina salattu.
- Rekisteröi sovelluksen tärkeimmistä säilöpalvelun Azure avaimen säilö.  
- Lisää tiedot salattuja sarakkeisiin.
- Valitse tietue suodattamalla salattuja sarakkeen tietyn arvon.

Korvaa **Program.cs** sisällön seuraava koodi. Korvaa rivi, joka edeltää suoraan kelvollista yhteysmerkkijonoa Azure-portaalista Main menetelmää yleinen connectionString muuttujaa yhteysmerkkijono. Tämä on ainoa muutos täytyy tehdä koodiksi.

Suorita sovellus Tutustu aina salattu toimintaan.

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    using System.Data;
    using System.Data.SqlClient;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.SqlServer.Management.AlwaysEncrypted.AzureKeyVaultProvider;

    namespace AlwaysEncryptedConsoleAKVApp
    {
    class Program
    {
        // Update this line with your Clinic database connection string from the Azure portal.
        static string connectionString = @"<connection string from the portal>";
        static string clientId = @"<client id from step 7 above>";
        static string clientSecret = "<key from step 13 above>";


        static void Main(string[] args)
        {
            InitializeAzureKeyVaultProvider();

            Console.WriteLine("Signed in as: " + _clientCredential.ClientId);

            Console.WriteLine("Original connection string copied from the Azure portal:");
            Console.WriteLine(connectionString);

            // Create a SqlConnectionStringBuilder.
            SqlConnectionStringBuilder connStringBuilder =
                new SqlConnectionStringBuilder(connectionString);

            // Enable Always Encrypted for the connection.
            // This is the only change specific to Always Encrypted
            connStringBuilder.ColumnEncryptionSetting =
                SqlConnectionColumnEncryptionSetting.Enabled;

            Console.WriteLine(Environment.NewLine + "Updated connection string with Always Encrypted enabled:");
            Console.WriteLine(connStringBuilder.ConnectionString);

            // Update the connection string with a password supplied at runtime.
            Console.WriteLine(Environment.NewLine + "Enter server password:");
            connStringBuilder.Password = Console.ReadLine();


            // Assign the updated connection string to our global variable.
            connectionString = connStringBuilder.ConnectionString;


            // Delete all records to restart this demo app.
            ResetPatientsTable();

            // Add sample data to the Patients table.
            Console.Write(Environment.NewLine + "Adding sample patient data to the database...");

            InsertPatient(new Patient()
            {
                SSN = "999-99-0001",
                FirstName = "Orlando",
                LastName = "Gee",
                BirthDate = DateTime.Parse("01/04/1964")
            });
            InsertPatient(new Patient()
            {
                SSN = "999-99-0002",
                FirstName = "Keith",
                LastName = "Harris",
                BirthDate = DateTime.Parse("06/20/1977")
            });
            InsertPatient(new Patient()
            {
                SSN = "999-99-0003",
                FirstName = "Donna",
                LastName = "Carreras",
                BirthDate = DateTime.Parse("02/09/1973")
            });
            InsertPatient(new Patient()
            {
                SSN = "999-99-0004",
                FirstName = "Janet",
                LastName = "Gates",
                BirthDate = DateTime.Parse("08/31/1985")
            });
            InsertPatient(new Patient()
            {
                SSN = "999-99-0005",
                FirstName = "Lucy",
                LastName = "Harrington",
                BirthDate = DateTime.Parse("05/06/1993")
            });


            // Fetch and display all patients.
            Console.WriteLine(Environment.NewLine + "All the records currently in the Patients table:");

            foreach (Patient patient in SelectAllPatients())
            {
                Console.WriteLine(patient.FirstName + " " + patient.LastName + "\tSSN: " + patient.SSN + "\tBirthdate: " + patient.BirthDate);
            }

            // Get patients by SSN.
            Console.WriteLine(Environment.NewLine + "Now lets locate records by searching the encrypted SSN column.");

            string ssn;

            // This very simple validation only checks that the user entered 11 characters.
            // In production be sure to check all user input and use the best validation for your specific application.
            do
            {
                Console.WriteLine("Please enter a valid SSN (ex. 999-99-0003):");
                ssn = Console.ReadLine();
            } while (ssn.Length != 11);

            // The example allows duplicate SSN entries so we will return all records
            // that match the provided value and store the results in selectedPatients.
            Patient selectedPatient = SelectPatientBySSN(ssn);

            // Check if any records were returned and display our query results.
            if (selectedPatient != null)
            {
                Console.WriteLine("Patient found with SSN = " + ssn);
                Console.WriteLine(selectedPatient.FirstName + " " + selectedPatient.LastName + "\tSSN: "
                    + selectedPatient.SSN + "\tBirthdate: " + selectedPatient.BirthDate);
            }
            else
            {
                Console.WriteLine("No patients found with SSN = " + ssn);
            }

            Console.WriteLine("Press Enter to exit...");
            Console.ReadLine();
        }


        private static ClientCredential _clientCredential;

        static void InitializeAzureKeyVaultProvider()
        {

            _clientCredential = new ClientCredential(clientId, clientSecret);

            SqlColumnEncryptionAzureKeyVaultProvider azureKeyVaultProvider =
              new SqlColumnEncryptionAzureKeyVaultProvider(GetToken);

            Dictionary<string, SqlColumnEncryptionKeyStoreProvider> providers =
              new Dictionary<string, SqlColumnEncryptionKeyStoreProvider>();

            providers.Add(SqlColumnEncryptionAzureKeyVaultProvider.ProviderName, azureKeyVaultProvider);
            SqlConnection.RegisterColumnEncryptionKeyStoreProviders(providers);
        }

        public async static Task<string> GetToken(string authority, string resource, string scope)
        {
            var authContext = new AuthenticationContext(authority);
            AuthenticationResult result = await authContext.AcquireTokenAsync(resource, _clientCredential);

            if (result == null)
                throw new InvalidOperationException("Failed to obtain the access token");
            return result.AccessToken;
        }

        static int InsertPatient(Patient newPatient)
        {
            int returnValue = 0;

            string sqlCmdText = @"INSERT INTO [dbo].[Patients] ([SSN], [FirstName], [LastName], [BirthDate])
     VALUES (@SSN, @FirstName, @LastName, @BirthDate);";

            SqlCommand sqlCmd = new SqlCommand(sqlCmdText);


            SqlParameter paramSSN = new SqlParameter(@"@SSN", newPatient.SSN);
            paramSSN.DbType = DbType.AnsiStringFixedLength;
            paramSSN.Direction = ParameterDirection.Input;
            paramSSN.Size = 11;

            SqlParameter paramFirstName = new SqlParameter(@"@FirstName", newPatient.FirstName);
            paramFirstName.DbType = DbType.String;
            paramFirstName.Direction = ParameterDirection.Input;

            SqlParameter paramLastName = new SqlParameter(@"@LastName", newPatient.LastName);
            paramLastName.DbType = DbType.String;
            paramLastName.Direction = ParameterDirection.Input;

            SqlParameter paramBirthDate = new SqlParameter(@"@BirthDate", newPatient.BirthDate);
            paramBirthDate.SqlDbType = SqlDbType.Date;
            paramBirthDate.Direction = ParameterDirection.Input;

            sqlCmd.Parameters.Add(paramSSN);
            sqlCmd.Parameters.Add(paramFirstName);
            sqlCmd.Parameters.Add(paramLastName);
            sqlCmd.Parameters.Add(paramBirthDate);

            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    sqlCmd.ExecuteNonQuery();
                }
                catch (Exception ex)
                {
                    returnValue = 1;
                    Console.WriteLine("The following error was encountered: ");
                    Console.WriteLine(ex.Message);
                    Console.WriteLine(Environment.NewLine + "Press Enter key to exit");
                    Console.ReadLine();
                    Environment.Exit(0);
                }
            }
            return returnValue;
        }


        static List<Patient> SelectAllPatients()
        {
            List<Patient> patients = new List<Patient>();


            SqlCommand sqlCmd = new SqlCommand(
              "SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients]",
                new SqlConnection(connectionString));


            using (sqlCmd.Connection = new SqlConnection(connectionString))

            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    SqlDataReader reader = sqlCmd.ExecuteReader();

                    if (reader.HasRows)
                    {
                        while (reader.Read())
                        {
                            patients.Add(new Patient()
                            {
                                SSN = reader[0].ToString(),
                                FirstName = reader[1].ToString(),
                                LastName = reader["LastName"].ToString(),
                                BirthDate = (DateTime)reader["BirthDate"]
                            });
                        }
                    }
                }
                catch (Exception ex)
                {
                    throw;
                }
            }

            return patients;
        }


        static Patient SelectPatientBySSN(string ssn)
        {
            Patient patient = new Patient();

            SqlCommand sqlCmd = new SqlCommand(
                "SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE [SSN]=@SSN",
                new SqlConnection(connectionString));

            SqlParameter paramSSN = new SqlParameter(@"@SSN", ssn);
            paramSSN.DbType = DbType.AnsiStringFixedLength;
            paramSSN.Direction = ParameterDirection.Input;
            paramSSN.Size = 11;

            sqlCmd.Parameters.Add(paramSSN);


            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    SqlDataReader reader = sqlCmd.ExecuteReader();

                    if (reader.HasRows)
                    {
                        while (reader.Read())
                        {
                            patient = new Patient()
                            {
                                SSN = reader[0].ToString(),
                                FirstName = reader[1].ToString(),
                                LastName = reader["LastName"].ToString(),
                                BirthDate = (DateTime)reader["BirthDate"]
                            };
                        }
                    }
                    else
                    {
                        patient = null;
                    }
                }
                catch (Exception ex)
                {
                    throw;
                }
            }
            return patient;
        }


        // This method simply deletes all records in the Patients table to reset our demo.
        static int ResetPatientsTable()
        {
            int returnValue = 0;

            SqlCommand sqlCmd = new SqlCommand("DELETE FROM Patients");
            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    sqlCmd.ExecuteNonQuery();

                }
                catch (Exception ex)
                {
                    returnValue = 1;
                }
            }
            return returnValue;
        }
    }

    class Patient
    {
        public string SSN { get; set; }
        public string FirstName { get; set; }
        public string LastName { get; set; }
        public DateTime BirthDate { get; set; }
    }
    }



## <a name="verify-that-the-data-is-encrypted"></a>Varmista, että tiedot salataan

Voit nopeasti tarkistaa palvelimen todellisten tietojen salataan kysely potilaille tiedot SSMS (käyttämällä nykyistä yhteyttä kohtaa, johon **Sarake salaus asetus** ei ole vielä käytössä).

Suorita seuraava kysely kurssi-tietokantaan.

    SELECT FirstName, LastName, SSN, BirthDate FROM Patients;

Näet, että salattuja sarakkeet eivät sisällä tekstimuotoisia tietoja.

   ![Uusi console-sovellus](./media/sql-database-always-encrypted-azure-key-vault/ssms-encrypted.png)


SSMS avulla voit käyttää tekstimuotoisia tietoja, voit lisätä *sarakkeen salaus asetus = käytössä* parametri yhteys.

1. SSMS- **Objektin Explorer** palvelinta hiiren kakkospainikkeella ja valitse **Katkaise yhteys**.
2. Valitse **Yhdistä** > **Tietokantamoduulin** Avaa **Muodosta yhteys palvelimeen** -ikkuna ja sitten **asetukset**.
3. Valitse **Lisää parametreja** ja kirjoita **sarakkeen salaus asetus = käytössä**.

    ![Uusi console-sovellus](./media/sql-database-always-encrypted-azure-key-vault/ssms-connection-parameter.png)

4. Suorita seuraava kysely kurssi-tietokantaan.

        SELECT FirstName, LastName, SSN, BirthDate FROM Patients;

     Näet nyt salattuja sarakkeen tekstimuotoisia tietoja.


    ![Uusi console-sovellus](./media/sql-database-always-encrypted-azure-key-vault/ssms-plaintext.png)


## <a name="next-steps"></a>Seuraavat vaiheet
Kun olet luonut aina salattu käyttävään tietokantaan, haluat ehkä seuraavasti:

- [Kierrä ja Puhdista avaimien](https://msdn.microsoft.com/library/mt607048.aspx).
- [Tietojen siirtäminen, joka on jo salattu aina salattu](https://msdn.microsoft.com/library/mt621539.aspx).


## <a name="related-information"></a>Muita aiheeseen liittyviä tietoja

- [Salattu aina (asiakkaan kehitys)](https://msdn.microsoft.com/library/mt147923.aspx)
- [Läpinäkyvä tiedon salaus](https://msdn.microsoft.com/library/bb934049.aspx)
- [SQL Server-salaus](https://msdn.microsoft.com/library/bb510663.aspx)
- [Aina salattu ohjattu toiminto](https://msdn.microsoft.com/library/mt459280.aspx)
- [Aina salattuja blogi](http://blogs.msdn.com/b/sqlsecurity/archive/tags/always-encrypted/)
