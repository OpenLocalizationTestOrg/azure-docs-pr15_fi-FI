<properties
    pageTitle="Aina salattu: Luottamuksellisten tietojen Azure SQL-tietokantaan tietokannan salaus suojaaminen | Microsoft Azure"
    description="Suojaa luottamukselliset tiedot SQL-tietokantaan minuutteina."
    keywords="salaa tiedot sql-salaus tietokannan salaus, luottamuksellisia tietoja aina salattu"
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

# <a name="always-encrypted-protect-sensitive-data-in-sql-database-and-store-your-encryption-keys-in-the-windows-certificate-store"></a>Aina salattu: Luottamuksellisten tietojen SQL-tietokantaan ja tallentaa salausavaimet sertifikaatin Windows-kaupasta

> [AZURE.SELECTOR]
- [Azure avaimen säilö](sql-database-always-encrypted-azure-key-vault.md)
- [Varmenteen Windows-kaupan](sql-database-always-encrypted.md)


Tässä artikkelissa kerrotaan, miten suojaamiseen luottamuksellisia tietoja tietokannan salaus ja SQL-tietokantaan ohjatulla [Aina salattu ohjattu](https://msdn.microsoft.com/library/mt459280.aspx) [SQL Server Management Studiossa (SSMS)](https://msdn.microsoft.com/library/hh213248.aspx). Se myös näytetään, miten voit tallentaa salausavaimet sertifikaatin Windows-kaupasta.

Aina salattu on uudet tiedot salaustekniikkaa Azure SQL-tietokantaan ja SQL Server, joka auttaa suojaamaan luottamuksellisia tietoja muiden palvelimessa, aikana asiakkaan ja palvelimen välillä siirtyminen ja tietojen ollessa käytössä varmistaminen luottamuksellisia tietoja koskaan näkyy pelkkänä tekstinä tietokannan järjestelmän sisällä. Salaa tiedot, kun asiakassovelluksissa tai sovelluksen palvelimet, joilla on oikeus näppäimet voi käyttää tekstimuotoisia tietoja. Lisätietoja on artikkelissa [Salattu aina (tietokantaohjelma)](https://msdn.microsoft.com/library/mt163865.aspx).

Kun määrittäminen käyttämään aina salattu tietokanta luodaan asiakassovellus C# Visual Studiossa salattujen tietojen käyttöä varten.

Noudata tässä artikkelissa kerrotaan määrittämisestä aina salattu Azure SQL-tietokantaan. Tässä artikkelissa kerrotaan, miten voit suorittaa seuraavia tehtäviä:

- Luo [Aina salattu avaimet](https://msdn.microsoft.com/library/mt163865.aspx#Anchor_3)SSMS aina salattu ohjatun toiminnon avulla.
    - Luo [Sarakkeen Master Key (CMK)](https://msdn.microsoft.com/library/mt146393.aspx).
    - Luo [sarakkeen salausavaimen (CEK)](https://msdn.microsoft.com/library/mt146372.aspx).
- Luoda tietokantataulukon ja salata sarakkeet.
- Luo sovellus, joka lisää, valitsee ja näyttää salattuja sarakkeiden tiedot.

## <a name="prerequisites"></a>Edellytykset

Tässä opetusohjelmassa, sinun on:

- Azure-tili ja tilauksen. Jos sinulla ei ole yhtä tilaamaan [maksuttoman kokeiluversion käyttäjäksi](https://azure.microsoft.com/pricing/free-trial/).
- [SQL Server Management Studiossa](https://msdn.microsoft.com/library/mt238290.aspx) versio 13.0.700.242 tai uudempi versio.
- [.NET framework 4.6](https://msdn.microsoft.com/library/w0x726c2.aspx) tai uudempi (asiakastietokoneen).
- [Visual Studio](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).



## <a name="create-a-blank-sql-database"></a>Tyhjä SQL-tietokannan luominen
1. Kirjautuminen [Azure portal](https://portal.azure.com/).
2. Valitse **Uusi** > **tietojen + tallennustilan** > **SQL-tietokantaan**.
3. Luo **Tyhjä** tietokanta nimeltä **kurssi** uuteen tai aiemmin luotuun palvelimessa. Lisätietoja Azure-portaalissa tietokannan luomisesta on artikkelissa [luominen minuutteina SQL-tietokantaan](sql-database-get-started.md).

    ![Luo tyhjä tietokanta](./media/sql-database-always-encrypted/create-database.png)

Sinun on yhteysmerkkijonon myöhemmin-opetusohjelman. Kun tietokanta on luotu, siirry uuden kurssi tietokannan ja kopioi yhteysmerkkijono. Saat yhteysmerkkijonon milloin tahansa, mutta se on helppo kopioida sen, kun olet Azure-portaalissa.

1. Valitse **SQL-tietokantoja** > **kurssi** > **Näytä tietokannan yhteysmerkkijonoja**.
2. Kopioi yhteysmerkkijono **ADO.NET**.

    ![Kopioi yhteysmerkkijono](./media/sql-database-always-encrypted/connection-strings.png)


## <a name="connect-to-the-database-with-ssms"></a>Muodosta yhteys tietokantaan SSMS kanssa

Avaa SSMS ja muodostaa yhteyttä palvelimeen kurssi tietokannassa.


1. Avaa SSMS. (Valitse **Yhdistä** > **Tietokantamoduuli** Avaa **Muodosta yhteys palvelimeen** -ikkuna, jos se ei ole avoinna).
2. Kirjoita palvelimen nimi ja tunnistetiedot. Palvelimen nimi löytyy SQL-tietokanta-sivu ja yhteysmerkkijonon aiemmin kopioitu. Kirjoita palvelimen täydellinen nimi, kuten *database.windows.net*.

    ![Kopioi yhteysmerkkijono](./media/sql-database-always-encrypted/ssms-connect.png)

Jos **Palomuurin uusi sääntö** -ikkuna avautuu Azure kirjautuminen ja anna SSMS uuden palomuurisäännön luominen puolestasi.


## <a name="create-a-table"></a>Taulukon luominen

Tässä osiossa luot taulukko sisältää Potilas tietoja. Tämä on Normaali taulukko aluksi--määrität salaus seuraavan osion.

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

SSMS sisältää ohjatun voivat määrittää helposti CMK, CEK ja salattuja sarakkeiden määrittäminen puolestasi aina salattu.

1. Laajenna **Databases** > **kurssi** > **taulukot**.
2. **Potilaille** taulukkoa hiiren kakkospainikkeella ja valitse **Salaa sarakkeet** ja Avaa ohjattu aina salattu:

    ![Salaa sarakkeet](./media/sql-database-always-encrypted/encrypt-columns.png)

Salattu aina ohjattua sisältää seuraavat osat: **Sarakkeen valinnan**, **Master Key määritys** (CMK), **vahvistus**ja **Yhteenveto**.

### <a name="column-selection"></a>Sarakkeen valitseminen ###

**Johdanto** sivulla Avaa **Sarakkeen valinta** -sivu valitsemalla **Seuraava** . Tällä sivulla voit valita näytettävät sarakkeet, jonka haluat salata, [Suojaus-tyypin ja mitä sarakkeen salausavain (CEK)](https://msdn.microsoft.com/library/mt459280.aspx#Anchor_2) käyttämään.

Salaa kunkin potilaan **Henkilötunnusta** ja **Syntymäpv-kenttien** tiedot. **Sosiaaliturvatunnus** -sarakkeessa käytetään deterministic salausta, joka tukee tasa haut, liitokset ja Ryhmittele. **Syntymäpäivä** -sarakkeessa käytetään satunnaisesti salausta, joka ei tue toimintoja.

Määritä **Salaustyyppi** **Henkilötunnusta** sarakkeen **Deterministic** ja **Syntymäpv** -sarake kohtaan **Randomized**. Valitse **Seuraava**.

![Salaa sarakkeet](./media/sql-database-always-encrypted/column-selection.png)

### <a name="master-key-configuration"></a>Avaimen määritys###

**Master Key määritys** -sivulla ei, jos oman CMK määrittäminen ja valitse avaimen säilöpalvelu mihin CMK tallennetaan. Tällä hetkellä voi tallentaa CMK sertifikaatin Windows-kaupasta, Azure avaimen säilö tai laitteiston (:: HSM)-moduulissa. Tässä opetusohjelmassa näytetään, miten voit tallentaa avaimien sertifikaatin Windows-kaupasta.

Varmista, että **sertifikaatti Windows-kaupan** on valittuna ja valitse **Seuraava**.

![Avaimen määritys](./media/sql-database-always-encrypted/master-key-configuration.png)


### <a name="validation"></a>Kelpoisuustarkistus###

Voit salata sarakkeet nyt tai tallentaa PowerShell-komentosarjaa myöhemmin. Tässä opetusohjelmassa, valitse **Jatka Viimeistele nyt** ja valitse **Seuraava**.

### <a name="summary"></a>Yhteenveto###

Varmista, että asetukset ovat kaikki korjaa ja Viimeistele määritykset saat aina salattu **Valmis** .

![Yhteenveto](./media/sql-database-always-encrypted/summary.png)


### <a name="verify-the-wizards-actions"></a>Varmista ohjatun toiminnon toiminnot

Kun ohjattu toiminto on valmis, tietokannan määritetään aina salattu. Ohjatun toiminnon suorittaa seuraavia toimintoja:

- Luoda CMK.
- Luoda CEK.
- Määritetty salauksen valitut sarakkeet. **Potilaille** taulukossa on tällä hetkellä ei ole tietoja, mutta valituissa sarakkeissa mahdollisesti olevat tiedot ovat nyt salattuja.

Voit tarkistaa SSMS avaimet luominen siirtymällä **kurssi** > **suojauksen** > **Aina salattu avaimet**. Nyt näet, ohjattu toiminto luo uuden näppäimet.


## <a name="create-a-client-application-that-works-with-the-encrypted-data"></a>Luo asiakassovellus, joka toimii salatut tiedot

Nyt kun aina salattu on määritetty, voit luoda sovellus, joka suorittaa *Lisää* ja *valitsee* salattuja sarakkeet. Esimerkkisovelluksen käyttämiseen onnistuneesti, suorittamalla se samassa tietokoneessa Jos suoritit aina salattu ohjatun toiminnon. Suorita sovellus toisessa tietokoneessa, on otettava käyttöön aina salattu sertifikaattien tarkasteleminen tietokoneen asiakas-sovellusta.  

> [AZURE.IMPORTANT] Sovelluksen on käytettävä [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx) objekteja, kun siirtämällä tekstimuotoisia tietoja aina salattu sarakkeiden palvelimeen. Poikkeuksen johtaa kulkeva literaaliarvot käyttämättä SqlParameter objekteja.


1. Avaa Visual Studio ja luo uusi C# console-sovellus. Varmista, että projektin on määritetty **.NET Framework 4.6** tai uudempi versio.
2. Projektin **AlwaysEncryptedConsoleApp** nimeä ja valitse **OK**.

![Uusi console-sovellus](./media/sql-database-always-encrypted/console-app.png)



## <a name="modify-your-connection-string-to-enable-always-encrypted"></a>Muokkaa yhteysmerkkijono salattu aina käyttöön

Tässä osassa kerrotaan ottamisesta käyttöön aina salattu tietokanta yhteysmerkkijonossa. Muokkaat console-sovelluksen luomaasi seuraavan osion, "Aina salattu console-sovelluksen malli."


Salattu aina käyttöön tarvitset lisää **Sarake salaus asetus** -avainsana yhteysmerkkijono ja määritä se **otetaan käyttöön**.

Voit määrittää tämän suoraan yhteysmerkkijonossa tai voit määrittää sen [SqlConnectionStringBuilder](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.aspx)avulla. Sovelluksen seuraavan osion malli näkyy **SqlConnectionStringBuilder**käyttämisestä.

> [AZURE.NOTE] Tämä on ainoa muutos asiakassovellus tietyn aina salauksen pakollinen. Jos sinulla on aiemmin luotua sovellusta, joka tallentaa ulkoisesti sen yhteysmerkkijonon (eli config tiedostossa), välttämättä voi aina salattu käyttöön muuttamatta koodia.


### <a name="enable-always-encrypted-in-the-connection-string"></a>Ota yhteysmerkkijonon aina salattu

Lisää seuraava avainsana yhteysmerkkijono:

    Column Encryption Setting=Enabled


### <a name="enable-always-encrypted-with-a-sqlconnectionstringbuilder"></a>Ota aina salattu SqlConnectionStringBuilder

Seuraava koodi näkyy ottamisesta käyttöön määrittämällä [SqlConnectionStringBuilder.ColumnEncryptionSetting](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.columnencryptionsetting.aspx) [käytössä](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectioncolumnencryptionsetting.aspx)aina salattu.

    // Instantiate a SqlConnectionStringBuilder.
    SqlConnectionStringBuilder connStringBuilder =
       new SqlConnectionStringBuilder("replace with your connection string");

    // Enable Always Encrypted.
    connStringBuilder.ColumnEncryptionSetting =
       SqlConnectionColumnEncryptionSetting.Enabled;



## <a name="always-encrypted-sample-console-application"></a>Aina salattu konsolisovelluksen malli

Tässä esimerkissä näytetään, miten voit:

- Muokkaa yhteysmerkkijono käyttöön aina salattu.
- Lisää tiedot salattuja sarakkeisiin.
- Valitse tietue suodattamalla salattuja sarakkeen tietyn arvon.

Korvaa **Program.cs** sisällön seuraava koodi. Korvaa rivi yläpuolelle Main-menetelmä yleinen connectionString muuttujan yhteysmerkkijono kelvollista yhteysmerkkijonoa Azure-portaalista. Tämä on ainoa muutos täytyy tehdä koodiksi.

Suorita sovellus Tutustu aina salattu toimintaan.

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    using System.Data;
    using System.Data.SqlClient;

    namespace AlwaysEncryptedConsoleApp
    {
    class Program
    {
        // Update this line with your Clinic database connection string from the Azure portal.
        static string connectionString = @"Replace with your connection string";

        static void Main(string[] args)
        {
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

            InsertPatient(new Patient() {
                SSN = "999-99-0001", FirstName = "Orlando", LastName = "Gee", BirthDate = DateTime.Parse("01/04/1964") });
            InsertPatient(new Patient() {
                SSN = "999-99-0002", FirstName = "Keith", LastName = "Harris", BirthDate = DateTime.Parse("06/20/1977") });
            InsertPatient(new Patient() {
                SSN = "999-99-0003", FirstName = "Donna", LastName = "Carreras", BirthDate = DateTime.Parse("02/09/1973") });
            InsertPatient(new Patient() {
                SSN = "999-99-0004", FirstName = "Janet", LastName = "Gates", BirthDate = DateTime.Parse("08/31/1985") });
            InsertPatient(new Patient() {
                SSN = "999-99-0005", FirstName = "Lucy", LastName = "Harrington", BirthDate = DateTime.Parse("05/06/1993") });


            // Fetch and display all patients.
            Console.WriteLine(Environment.NewLine + "All the records currently in the Patients table:");

            foreach (Patient patient in SelectAllPatients())
            {
                Console.WriteLine(patient.FirstName + " " + patient.LastName + "\tSSN: " + patient.SSN + "\tBirthdate: " + patient.BirthDate);
            }

            // Get patients by SSN.
            Console.WriteLine(Environment.NewLine + "Now let's locate records by searching the encrypted SSN column.");

            string ssn;

            // This very simple validation only checks that the user entered 11 characters.
            // In production be sure to check all user input and use the best validation for your specific application.
            do
            {
                Console.WriteLine("Please enter a valid SSN (ex. 123-45-6789):");
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

Voit nopeasti tarkistaa palvelimen todellisten tietojen salataan kyselyt SSMS **potilaille** tiedot. (Käytä nykyistä yhteyttä kohtaa, johon sarake salaus-asetus ei ole vielä käytössä.)

Suorita seuraava kysely kurssi-tietokantaan.

    SELECT FirstName, LastName, SSN, BirthDate FROM Patients;

Näet, että salattuja sarakkeet eivät sisällä tekstimuotoisia tietoja.

   ![Uusi console-sovellus](./media/sql-database-always-encrypted/ssms-encrypted.png)


SSMS avulla voit käyttää tekstimuotoisia tietoja, voit lisätä **sarakkeen salaus asetus = käytössä** parametri yhteys.

1. Valitse SSMS **Object Explorer**palvelinta hiiren kakkospainikkeella ja valitse sitten **Katkaise yhteys**.
2. Valitse **Yhdistä** > **Tietokantamoduuli** Avaa **Muodosta yhteys palvelimeen** -ikkuna ja valitse sitten **asetukset**.
3. Valitse **Lisää parametreja** ja kirjoita **sarakkeen salaus asetus = käytössä**.

    ![Uusi console-sovellus](./media/sql-database-always-encrypted/ssms-connection-parameter.png)

4. Suorita seuraava kysely **kurssi** -tietokantaan.

        SELECT FirstName, LastName, SSN, BirthDate FROM Patients;

     Näet nyt salattuja sarakkeen tekstimuotoisia tietoja.


    ![Uusi console-sovellus](./media/sql-database-always-encrypted/ssms-plaintext.png)



> [AZURE.NOTE] Jos muodostat SSMS (tai mikä tahansa asiakas) toisella tietokoneella, se ei ole access salausavaimet ja voi salauksen tiedot.



## <a name="next-steps"></a>Seuraavat vaiheet
Kun olet luonut aina salattu käyttävään tietokantaan, haluat ehkä seuraavasti:

- Suorita tässä esimerkissä toisella tietokoneella. Sillä ei ole pääsy salausavaimet, jotta se ei ole vain teksti-tietojen käytön ja ei suoriteta onnistuneesti.
- [Kierrä ja Puhdista avaimien](https://msdn.microsoft.com/library/mt607048.aspx).
- [Tietojen siirtäminen, joka on jo salattu aina salattu](https://msdn.microsoft.com/library/mt621539.aspx).
- [Muut asiakaskoneiden aina käyttöön salattu varmenteet](https://msdn.microsoft.com/library/mt723359.aspx#Anchor_1) (Katso "Tekeminen varmenteet käytettävissä sovellukset ja käyttäjille"-osio).

## <a name="related-information"></a>Muita aiheeseen liittyviä tietoja

- [Salattu aina (asiakkaan kehitys)](https://msdn.microsoft.com/library/mt147923.aspx)
- [Läpinäkyvä tiedon salaus](https://msdn.microsoft.com/library/bb934049.aspx)
- [SQL Server-salaus](https://msdn.microsoft.com/library/bb510663.aspx)
- [Aina salattuja ohjattu toiminto](https://msdn.microsoft.com/library/mt459280.aspx)
- [Aina salattuja blogi](http://blogs.msdn.com/b/sqlsecurity/archive/tags/always-encrypted/)
