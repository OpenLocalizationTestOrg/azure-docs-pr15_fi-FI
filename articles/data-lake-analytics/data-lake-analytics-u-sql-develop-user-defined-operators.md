<properties 
   pageTitle="Kehittää U-SQL-käyttäjän määrittämät operaattorit Azure tietojen järvi Analytics töiden | Azure" 
   description="Opettele kehittää käyttäjän määrittämiä operaattoreita voidaan käyttää ja tietojen järvi Analytics työt uudelleen. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="edmacauley" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="05/16/2016"
   ms.author="edmaca"/>


# <a name="develop-u-sql-user-defined-operators-for-azure-data-lake-analytics-jobs"></a>Kehittää Azure tietojen järvi Analytics työt U-SQL-käyttäjän määrittämät operaattorit

Opettele kehittää käyttäjän määrittämiä operaattoreita voidaan käyttää ja tietojen järvi Analytics työt uudelleen. Kehität mukautettuja operaattori muuntaa maan nimet.

##<a name="prerequisites"></a>Edellytykset

- Visual Studio 2015, Visual Studio 2013 Päivitä 4 tai Visual Studio 2012 Visual C++ asennettuna 
- Microsoft Azure SDK .NET 2.5-version tai yläpuolella.  Asenna se käyttämällä WWW-ympäristö asennusohjelma.
- Tietoja järvi Analytics-tili.  Katso [Azure tietojen järvi Analytics Azure-portaalissa käytön aloittaminen](data-lake-analytics-get-started-portal.md).
- Siirry [Azure tietojen järvi Analytics U SQL Studio käytön aloittaminen](data-lake-analytics-u-sql-get-started.md) -opetusohjelma kautta.
- Yhteyden muodostaminen Azure on artikkelissa [Azure tietojen järvi Analytics U SQL Studio käytön aloittaminen](data-lake-analytics-u-sql-get-started.md#connect-to-azure). 
- Lataa lähdetietojen on artikkelissa [Azure tietojen järvi Analytics U SQL Studio käytön aloittaminen](data-lake-analytics-u-sql-get-started.md#upload-source-data-files). 

## <a name="define-and-use-user-defined-operator-in-u-sql"></a>Määrittäminen ja käyttäminen käyttäjän määrittämä operaattori U SQL

**Voit luoda ja lähettää U-SQL-työ** 

1. **Tiedosto** -valikosta **Uusi**ja valitse sitten **Projekti**.
2. Valitse **U-SQL-projektin** tyyppi.

    ![Uusi Visual Studio U-SQL-projekti](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-new-project.png)

3. Valitse **OK**. Visual studio Luo ratkaisun Script.usql-tiedoston.
4. Laajenna Script.usql **Ratkaisunhallinnassa**ja kaksoisnapsauta **Script.usql.cs**.
5. Liitä tiedosto seuraava koodi:

        using Microsoft.Analytics.Interfaces;
        using System.Collections.Generic;
        
        namespace USQL_UDO
        {
            public class CountryName : IProcessor
            {
                private static IDictionary<string, string> CountryTranslation = new Dictionary<string, string>
                {
                    {
                        "Deutschland", "Germany"
                    },
                    {
                        "Schwiiz", "Switzerland"
                    },
                    {
                        "UK", "United Kingdom"
                    },
                    {
                        "USA", "United States of America"
                    },
                    {
                        "中国", "PR China"
                    }
                };
        
                public override IRow Process(IRow input, IUpdatableRow output)
                {
        
                    string UserID = input.Get<string>("UserID");
                    string Name = input.Get<string>("Name");
                    string Address = input.Get<string>("Address");
                    string City = input.Get<string>("City");
                    string State = input.Get<string>("State");
                    string PostalCode = input.Get<string>("PostalCode");
                    string Country = input.Get<string>("Country");
                    string Phone = input.Get<string>("Phone");
        
                    if (CountryTranslation.Keys.Contains(Country))
                    {
                        Country = CountryTranslation[Country];
                    }
                    output.Set<string>(0, UserID);
                    output.Set<string>(1, Name);
                    output.Set<string>(2, Address);
                    output.Set<string>(3, City);
                    output.Set<string>(4, State);
                    output.Set<string>(5, PostalCode);
                    output.Set<string>(6, Country);
                    output.Set<string>(7, Phone);
        
                    return output.AsReadOnly();
                }
            }
        }

5. Avaa Script.usql ja liitä seuraava U-SQL-komentosarja:

        @drivers =
            EXTRACT UserID      string,
                    Name        string,
                    Address     string,
                    City        string,
                    State       string,
                    PostalCode  string,
                    Country     string,
                    Phone       string
            FROM "/Samples/Data/AmbulanceData/Drivers.txt"
            USING Extractors.Tsv(Encoding.Unicode);
        
        @drivers_CountryName =
            PROCESS @drivers
            PRODUCE UserID string,
                    Name string,
                    Address string,
                    City string,
                    State string,
                    PostalCode string,
                    Country string,
                    Phone string
            USING new USQL_UDO.CountryName();    
        
        OUTPUT @drivers_CountryName
            TO "/Samples/Outputs/Drivers.csv"
            USING Outputters.Csv(Encoding.Unicode);

6. **Ratkaisunhallinnassa** **Script.usql**napsauttamalla hiiren kakkospainikkeella ja valitse sitten **Luo komentosarjan**.
6. **Ratkaisunhallinnassa** **Script.usql**napsauttamalla hiiren kakkospainikkeella ja valitse sitten **Lähetä komentosarjan**.
7. Jos muodostat eivät ole Azure-tilaukseen, on kehote tunnistetietosi Azure-tili.
7. Valitse **Lähetä**. Lähettämisen tulokset ja työn linkki ovat käytettävissä tulokset-ikkunassa kun lähetys on valmis.
8. Sinun on valittava uusimman projektin tilaa ja näytön päivittäminen Päivitä-painike.

**Jos haluat nähdä projektin tulos**

1. **Palvelimen Explorer**Laajenna **Azure**, laajenna **Tietojen järvi Analytics**, laajenna tietojen järvi Analytics-tilisi, laajenna **Tallennustilan tilit**, oletus-tallennustilan hiiren kakkospainikkeella ja valitse **Explorer**. 
2. Laajenna mallit, laajenna tulostus ja kaksoisnapsauta sitten **Drivers.csv**.


##<a name="see-also"></a>Katso myös

- [Aloita tietojen järvi Analytics PowerShellin avulla](data-lake-analytics-get-started-powershell.md)
- [Aloita tietojen järvi Analytics Azure-portaalissa](data-lake-analytics-get-started-portal.md)
- [U-SQL-sovellusten kehittämiseen Visual Studio järvi Datatyökalut käyttäminen](data-lake-analytics-data-lake-tools-get-started.md)
