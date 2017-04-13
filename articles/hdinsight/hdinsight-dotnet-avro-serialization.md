<properties
    pageTitle="Muuntaa sarjaksi Microsoft Avro Library tietojasi | Microsoft Azure"
    description="Katso, miten Azure Hdinsightiin käyttää Avro onnistu big datasta."
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian" 
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="jgao"/>


# <a name="serialize-data-in-hadoop-with-the-microsoft-avro-library"></a>Muuntaa sarjaksi tietojen Hadoop Microsoft Avro Library kanssa

Tässä ohjeaiheessa esitellään opit käyttämään <a href="https://hadoopsdk.codeplex.com/wikipage?title=Avro%20Library" target="_blank">Microsoft Avro kirjaston</a> onnistu objektit ja muut rakenteet jotta jatkuvat ne muistiin, tietokannan tai tiedoston tuominen virtaa ja miten voit poistaa ne, Palauta alkuperäiset objektit.

[AZURE.INCLUDE [windows-only](../../includes/hdinsight-windows-only.md)]

##<a name="apache-avro"></a>Apache Avro
<a href="https://hadoopsdk.codeplex.com/wikipage?title=Avro%20Library" target="_blank">Microsoft Avro kirjaston</a> toteuttaa Apache Avro tietojen Sarjatoiminto järjestelmän Microsoft.NET-ympäristössä. Apache Avro tarjoaa Sarjatoiminto compact binaaritietoja Interchange Format-muoto. Se käyttää <a href="http://www.json.org" target="_blank">JSON</a> Määritä kieli-ympäristöstä riippumattomalla tavalla rakenteen, joka vahinkosuhdetta kielen yhteensopivuus. Muuntaa sarjaksi yhden kielen tiedot voidaan lukea toiseen. Tällä hetkellä tueta, C, C++, C#, Java, PHP, Python ja Ruby. Muodon yksityiskohtaiset tiedot löytyvät <a href="http://avro.apache.org/docs/current/spec.html" target="_blank">Apache Avro määritykset</a>. Huomaa, että Microsoft Avro Library nykyinen versio ei tue määrityksen Etäproseduurikutsu (RPC-kutsujen) puhelut-osa.

Objektin Avro järjestelmän sarjoitettu esittäminen koostuu kahdesta: rakenteen ja todellinen arvo. Avro-rakenne kuvaa kielen riippumattomalla tietomallin JSON sarjoitettu tiedot. On olemassa-rinnakkais binaarimuotoinen tietojen kanssa. Kullekin objektille ei ole arvo yleiskustannusten, että Sarjatoiminto nopea ja esitys pieni kirjoitetaan ottaa erillään binaarimuotoinen rakenteen sallii.

##<a name="the-hadoop-scenario"></a>Hadoop-skenaario
Apache Avro Sarjatoiminto muotoilua käytetään laajasti Azure Hdinsightiin ja Apache Hadoop muiden ympäristöjen. Avro avulla kätevästi edustavan monimutkaisten tietojen rakenteet Hadoop MapReduce projektissa. Muodon muuttaminen Avro tiedostot (Avro objektin säilötiedosto) on suunniteltu tukemaan hajautettu MapReduce ohjelmoinnin malli. Tiedostot ovat "jaettavia" siten, että jokin haku jotakin tiedoston kohtaa ja aloita lukeminen tietyn estosta on avaimen toiminto, joka mahdollistaa jakauman.

##<a name="serialization-in-avro-library"></a>Sarjatoiminto Avro-kirjastossa
.NET-kirjastosta Avro tukee sarjoittamista objektien kahdella tavalla:

- **Heijastus** - JSON rakenteen sisältötyypeille muodostanut tiedoista automaattisesti .NET-tyyppiä vastaamaan voi muuntaa sarjaksi sopimuksen määritteet.
- **Yleinen tietue** - A JSON rakenne on nimenomaisesti määritetty tietueen vastaavan [**AvroRecord**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) luokan kun ei ole .NET-tietotyypit ovat näkyvissä kuvaamaan voi muuntaa sarjaksi tietojen rakennetta.

Kun tietomallin tiedetään writer ja virta lukija-tiedot voi lähettää sen rakenne ilman. Tapauksissa Avro objektin säilö äänitiedoston käytettäessä rakenne on tallennettu tiedosto. Muut parametrit-pakkauksenhallinta pakkaamiseen, kuten voidaan määrittää. Näissä tilanteissa kuvatut tarkemmin ja esitelty koodin alla olevissa esimerkeissä.


## <a name="install-avro-library"></a>Asenna Avro kirjastoon

Seuraavassa on tarvittavat ennen kuin asennat kirjastoon:

- <a href="http://www.microsoft.com/download/details.aspx?id=17851" target="_blank">Microsoft .NET Framework 4</a>
- <a href="http://james.newtonking.com/json" target="_blank">Newtonsoft Json.NET</a> (6.0.4 tai uudempi)

Huomautus Newtonsoft.Json.dll riippuvuuden ladattu Microsoft Avro Library asennuksessa automaattisesti. Tämä menetelmä annetaan seuraavassa osassa.


Microsoft Avro Library on jaettu NuGet pakettina, joka voidaan asentaa Visual Studio kautta seuraavasti:

1. Valitse **Projekti** -välilehden -> **Hallitse NuGet pakettien...**
2. Etsi "Microsoft.Hadoop.Avro" **Etsi Online** -ruutuun.
3. Napsauta **Microsoft Azure Hdinsightiin Avro kirjaston**vieressä **Asenna** -painiketta.

Huomaa, että Newtonsoft.Json.dll (> = 6.0.4) riippuvuuden ladataan myös automaattisesti Microsoft Avro Library kanssa.

Voit lukea nykyisen julkaisutiedot <a href="https://hadoopsdk.codeplex.com/wikipage?title=Avro%20Library" target="_blank">Microsoft Avro kirjaston kotisivulle</a> .


Microsoft Avro Library-lähdekoodi on saatavilla kohdassa <a href="https://hadoopsdk.codeplex.com/wikipage?title=Avro%20Library" target="_blank">Microsoft Avro kirjaston kotisivulle</a>.

##<a name="compile-schemas-using-avro-library"></a>Käännä Avro kirjaston rakenteet

Microsoft Avro Library sisältää koodin luonti-apuohjelma, jonka avulla luodaan C# tyypit automaattisesti aiemmin määritettyä JSON-rakenteeseen perustuvan. Koodin luonti-apuohjelma on ei jaettu binaarinen asennettu, mutta voit helposti rakentaa kautta seuraavasti:

1. Lataa <a href="http://hadoopsdk.codeplex.com/SourceControl/latest#" target="_blank">Microsoft .NET SDK Hadoop</a>HDInsight SDK-lähdekoodi uusimman version kanssa .zip-tiedosto. (Valitse **Lataa** kuvake ei **Lataa** välilehti).

2. Pura HDInsight SDK tietokoneessa kansioon .NET Framework 4 asennettu ja ladata tarvittavat riippuvuuden NuGet pakettien Internet-yhteys. Alla oletetaan, että lähdekoodin puretaan C:\SDK.

3. Siirry kansioon C:\SDK\src\Microsoft.Hadoop.Avro.Tools ja suorita build.bat. (Tiedosto kutsua MSBuild .NET Framework 32-bittinen jakautumisen. Jos haluat käyttää 64-bittinen versio, Muokkaa build.bat, seuraavien kommentit-tiedoston sisälle.) Varmista, että Luo on onnistunut. (Joidenkin järjestelmien MSBuild voi tuottaa varoitukset. Nämä varoitukset eivät vaikuta apuohjelma, kunhan muodosta ei ole virheitä.)

4. Käännetty apuohjelma sijaitsee C:\SDK\Bin\Unsigned\Release\Microsoft.Hadoop.Avro.Tools.


Komentorivin syntaksi esitellään, suorita seuraava komento-kansiosta koodin luonti-apuohjelman sijainti:`Microsoft.Hadoop.Avro.Tools help /c:codegen`

Voit testata apuohjelma, voit luoda C# luokkien mallitiedostossa JSON rakenteen lähdekoodin mukana. Suorita seuraava komento:

    Microsoft.Hadoop.Avro.Tools codegen /i:C:\SDK\src\Microsoft.Hadoop.Avro.Tools\SampleJSON\SampleJSONSchema.avsc /o:

Tämä on tarkoitus tuottaa kaksi C# tiedostot nykyiseen kansioon: SensorData.cs ja Location.cs.

Logiikan, joka koodin luonti-apuohjelma on käytössä, kun JSON rakenteen muunnetaan C# tyypit on artikkelissa C:\SDK\src\Microsoft.Hadoop.Avro.Tools\Doc GenerationVerification.feature, sijaitsevaa tiedostoa.

Huomaa, että nimitilan poimittujen JSON-rakenne-tiedostossa, edellä mainituista logiikan avulla. Nimitilan rakenteen poimittujen ohittavat /n parametrin apuohjelman komentorivillä mukana riippumatta. Jos haluat ohittaa sisältämät rakenteen nimitilan, käytä /nf parametrin. Jos haluat muuttaa kaikki nimitilan SampleJSONSchema.avsc my.own.nspace, suorita seuraava komento:

    Microsoft.Hadoop.Avro.Tools codegen /i:C:\SDK\src\Microsoft.Hadoop.Avro.Tools\SampleJSON\SampleJSONSchema.avsc /o:. /nf:my.own.nspace

## <a name="samples"></a>Esimerkkejä, joiden
Kuusi annettu tämän artikkelin esimerkeissä kuvataan eri skenaarioissa Microsoft Avro Library tukemat. Microsoft Avro Library on suunniteltu toimimaan minkä tahansa muodossa. Näissä esimerkeissä tietojen toiminnat muistin virtaa sijaan tiedoston virtaa tai tietokantojen yksinkertaisuuden ja yhtenäisyyden kautta. Koskevassa tuotantoympäristössä riippuu tarkka skenaarion vaatimukset, tietolähteen ja aseman, suorituskyvyn rajoitukset ja muut tekijät.

Kaksi ensimmäistä esimerkeissä näytetään, voit muuntaa sarjaksi ja poistaa tietoja muistin stream puskuri kyselyjä käyttämällä heijastus ja Yleinen tietueita. Kummassakin tapauksessa rakenteen oletetaan voidaan jakaa lukijat ja kirjoittajat Luokittele.

Kolmannen ja neljännen esimerkeissä näytetään, voit muuntaa sarjaksi ja poistaa tietoja käyttämällä Avro objektin säilö tiedostot. Kun tiedot on tallennettu Avro säilö-tiedoston, sen rakenne tallennetaan aina sitä, koska rakenne on jaettava sarjoituksen.

<a href="http://code.msdn.microsoft.com/windowsazure/Serialize-data-with-the-86055923" target="_blank">Azure MALLIKOODEJA</a> -sivustosta voi ladata näyte, joka sisältää ensin neljä esimerkkiä.

Viidennen esimerkissä käyttäminen mukautettujen pakkauksenhallintaa Avro objektin säilö tiedostojen poistamisesta. <a href="http://code.msdn.microsoft.com/windowsazure/Serialize-data-with-the-67159111" target="_blank">Azure MALLIKOODEJA</a> -sivustosta voi ladata otoksen, joka sisältää tässä esimerkissä koodi.

Kuudennesta otosten näytetään, miten tietojen lataaminen Azure-Blob-säiliö ja analysoi käyttämällä rakenteen HDInsight (Hadoop)-klusterin Avro Sarjatoiminto avulla. Se voi ladata <a href="https://code.msdn.microsoft.com/windowsazure/Using-Avro-to-upload-data-ae81b1e3" target="_blank">Azure MALLIKOODEJA</a> -sivustosta.

Seuraavassa on linkkejä kuusi esimerkkejä, joiden aiheena on ohjeaiheessa:

 * <a href="#Scenario1">**Sarjatoiminto ja heijastus**</a> - tyypit voi muuntaa sarjaksi JSON rakenteen muodostanut tiedoista automaattisesti sopimuksen määritteet.
 * <a href="#Scenario2">**Sarjatoiminto yleinen tietue**</a> - JSON rakenteen nimenomaisesti määritetty tietueen tyyppiä ei .NET ollessa käytettävissä heijastus.
 * <a href="#Scenario3">**Sarjatoiminto käyttämällä objektin säilö-tiedostoja, joissa heijastus**</a> - JSON rakenteen sisäisten ja jaetut Avro objektin säilö-tiedostolla sarjoitettu tiedot automaattisesti.
 * <a href="#Scenario4">**Sarjatoiminto käyttämällä objektin säilö tiedostot, joiden yleinen tietue**</a> - JSON rakenne on erikseen ennen Sarjatoiminto määritetyn ja Avro objektin säilö-tiedostolla tiedot jaetaan.
 * <a href="#Scenario5">**Sarjatoiminto käyttämällä mukautetun pakkauksenhallintaa objektin säilö tiedostot**</a> - esimerkissä esitetään, kuinka Avro objektin säilötiedoston luominen mukautetun .NET-soveltaminen Deflate tietojen pakkauksenhallintaa kanssa.
 * <a href="#Scenario6">**Voit ladata Microsoft Azure HDInsight-palvelun tiedot käyttämällä Avro**</a> - Esimerkki havainnollistaa, miten Avro Sarjatoiminto toimii HDInsight-palvelun kanssa. Voit suorittaa tämän esimerkin tarvitaan aktiivinen Azure tilaus ja Azure HDInsight-klusterin käyttöoikeus.

###<a name="Scenario1"></a>Esimerkki 1: Sarjatoiminto ja heijastus

JSON-rakenne-kohdat, voit voi automaattisesti muodostama Microsoft Avro Library kautta heijastus tiedoista sopimuksen määritteiden C#-objekteja, voi muuntaa sarjaksi. Luo Microsoft Avro Library [**IAvroSeralizer<T> **](http://msdn.microsoft.com/library/dn627341.aspx) tunnistavan kentät, jotka voi muuntaa sarjaksi.

Tässä esimerkissä objektit ( **SensorData** luokan jäsenen **sijainti** Struct) on esittää sarjana, muistin virta ja vuosta puolestaan poistaa. Tulos verrataan Valitse Vahvista, että palauttaa **SensorData** objekti on sama kuin alkuperäinen ensimmäisen esiintymän.

Tässä esimerkissä rakenteen oletetaan voi jakaa lukijat – kirjoittajat, jotta Avro objektin container-muoto ei ole pakollinen. Katso esimerkki onnistu ja poistaa tietoja muistin puskuri kyselyjä käyttämällä heijastus säilö piirrosobjektimuoto kun rakenne on jakanut tiedot <a href="#Scenario3">käyttämällä objektin säilö-tiedostoja, joissa heijastus Sarjatoiminto</a>.

    namespace Microsoft.Hadoop.Avro.Sample
    {
        using System;
        using System.Collections.Generic;
        using System.IO;
        using System.Linq;
        using System.Runtime.Serialization;
        using Microsoft.Hadoop.Avro.Container;
        using Microsoft.Hadoop.Avro;

        //Sample class used in serialization samples
        [DataContract(Name = "SensorDataValue", Namespace = "Sensors")]
        internal class SensorData
        {
            [DataMember(Name = "Location")]
            public Location Position { get; set; }

            [DataMember(Name = "Value")]
            public byte[] Value { get; set; }
        }

        //Sample struct used in serialization samples
        [DataContract]
        internal struct Location
        {
            [DataMember]
            public int Floor { get; set; }

            [DataMember]
            public int Room { get; set; }
        }

        //This class contains all methods demonstrating
        //the usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serialize and deserialize sample data set represented as an object using reflection.
            //No explicit schema definition is required - schema of serialized objects is automatically built.
            public void SerializeDeserializeObjectUsingReflection()
            {

                Console.WriteLine("SERIALIZATION USING REFLECTION\n");
                Console.WriteLine("Serializing Sample Data Set...");

                //Create a new AvroSerializer instance and specify a custom serialization strategy AvroDataContractResolver
                //for serializing only properties attributed with DataContract/DateMember
                var avroSerializer = AvroSerializer.Create<SensorData>();

                //Create a memory stream buffer
                using (var buffer = new MemoryStream())
                {
                    //Create a data set by using sample class and struct
                    var expected = new SensorData { Value = new byte[] { 1, 2, 3, 4, 5 }, Position = new Location { Room = 243, Floor = 1 } };

                    //Serialize the data to the specified stream
                    avroSerializer.Serialize(buffer, expected);


                    Console.WriteLine("Deserializing Sample Data Set...");

                    //Prepare the stream for deserializing the data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Deserialize data from the stream and cast it to the same type used for serialization
                    var actual = avroSerializer.Deserialize(buffer);

                    Console.WriteLine("Comparing Initial and Deserialized Data Sets...");

                    //Finally, verify that deserialized data matches the original one
                    bool isEqual = this.Equal(expected, actual);

                    Console.WriteLine("Result of Data Set Identity Comparison is {0}", isEqual);

                }
            }

            //
            //Helper methods
            //

            //Comparing two SensorData objects
            private bool Equal(SensorData left, SensorData right)
            {
                return left.Position.Equals(right.Position) && left.Value.SequenceEqual(right.Value);
            }



            static void Main()
            {

                string sectionDivider = "---------------------------------------- ";

                //Create an instance of AvroSample Class and invoke methods
                //illustrating different serializing approaches
                AvroSample Sample = new AvroSample();

                //Serialization to memory using reflection
                Sample.SerializeDeserializeObjectUsingReflection();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key to exit.");
                Console.Read();
            }
        }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING REFLECTION
    //
    // Serializing Sample Data Set...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // Result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.


###<a name="sample-2-serialization-with-a-generic-record"></a>Esimerkki 2: Sarjatoiminto yleinen tietueeseen

JSON-rakenne on erikseen määritettävissä yleinen tietueen kun heijastus ei voi käyttää, koska tietoja ei voi esittää .NET-luokat, joiden tiedot sopimuksen kautta. Tämä menetelmä on yleensä hitaammin heijastuksen avulla. Tässä tapauksessa rakenteen tiedot myös voidaan dynaaminen, eli tiedetään kääntämisen yhteydessä. Pilkuilla erotetut arvot (CSV)-tiedostoina, jonka rakenne on tuntematon, kunnes se muuttuu Avro muotoon suorituksen aikana tiedot on esimerkki dynaaminen skenaarion tällaiset.

Tässä esimerkissä näytetään luomisesta ja [**AvroRecord**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) avulla voit määrittää erikseen JSON rakenteen, voit täyttää tiedot ja sitten muuntaa sarjaksi ja poistaa sen. Tulos verrataan Valitse Vahvista, että palauttaa tietue on sama kuin alkuperäinen ensimmäisen esiintymän.

Tässä esimerkissä rakenteen oletetaan voi jakaa lukijat – kirjoittajat, jotta Avro objektin container-muoto ei ole pakollinen. Esimerkki onnistu ja poistaa tietoja muistin puskuri kyselyjä käyttämällä yleinen tietueen säilö piirrosobjektimuoto kun rakenne on oltava sarjoitettu tiedoilla Katso <a href="#Scenario4">Sarjatoiminto käyttämällä objektin säilö tiedostot, joiden yleinen tietueen</a> esimerkkiä.


    namespace Microsoft.Hadoop.Avro.Sample
    {
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Linq;
    using System.Runtime.Serialization;
    using Microsoft.Hadoop.Avro.Container;
    using Microsoft.Hadoop.Avro.Schema;
    using Microsoft.Hadoop.Avro;

    //This class contains all methods demonstrating
    //the usage of Microsoft Avro Library
    public class AvroSample
    {

        //Serialize and deserialize sample data set by using a generic record.
        //A generic record is a special class with the schema explicitly defined in JSON.
        //All serialized data should be mapped to the fields of the generic record,
        //which in turn will be then serialized.
        public void SerializeDeserializeObjectUsingGenericRecords()
        {
            Console.WriteLine("SERIALIZATION USING GENERIC RECORD\n");
            Console.WriteLine("Defining the Schema and creating Sample Data Set...");

            //Define the schema in JSON
            const string Schema = @"{
                                ""type"":""record"",
                                ""name"":""Microsoft.Hadoop.Avro.Specifications.SensorData"",
                                ""fields"":
                                    [
                                        {
                                            ""name"":""Location"",
                                            ""type"":
                                                {
                                                    ""type"":""record"",
                                                    ""name"":""Microsoft.Hadoop.Avro.Specifications.Location"",
                                                    ""fields"":
                                                        [
                                                            { ""name"":""Floor"", ""type"":""int"" },
                                                            { ""name"":""Room"", ""type"":""int"" }
                                                        ]
                                                }
                                        },
                                        { ""name"":""Value"", ""type"":""bytes"" }
                                    ]
                            }";

            //Create a generic serializer based on the schema
            var serializer = AvroSerializer.CreateGeneric(Schema);
            var rootSchema = serializer.WriterSchema as RecordSchema;

            //Create a memory stream buffer
            using (var stream = new MemoryStream())
            {
                //Create a generic record to represent the data
                dynamic location = new AvroRecord(rootSchema.GetField("Location").TypeSchema);
                location.Floor = 1;
                location.Room = 243;

                dynamic expected = new AvroRecord(serializer.WriterSchema);
                expected.Location = location;
                expected.Value = new byte[] { 1, 2, 3, 4, 5 };

                Console.WriteLine("Serializing Sample Data Set...");

                //Serialize the data
                serializer.Serialize(stream, expected);

                stream.Seek(0, SeekOrigin.Begin);

                Console.WriteLine("Deserializing Sample Data Set...");

                //Deserialize the data into a generic record
                dynamic actual = serializer.Deserialize(stream);

                Console.WriteLine("Comparing Initial and Deserialized Data Sets...");

                //Finally, verify the results
                bool isEqual = expected.Location.Floor.Equals(actual.Location.Floor);
                isEqual = isEqual && expected.Location.Room.Equals(actual.Location.Room);
                isEqual = isEqual && ((byte[])expected.Value).SequenceEqual((byte[])actual.Value);
                Console.WriteLine("Result of Data Set Identity Comparison is {0}", isEqual);
            }
        }

        static void Main()
        {

            string sectionDivider = "---------------------------------------- ";

            //Create an instance of AvroSample class and invoke methods
            //illustrating different serializing approaches
            AvroSample Sample = new AvroSample();

            //Serialization to memory using generic record
            Sample.SerializeDeserializeObjectUsingGenericRecords();

            Console.WriteLine(sectionDivider);
            Console.WriteLine("Press any key to exit.");
            Console.Read();
        }
    }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING GENERIC RECORD
    //
    // Defining the Schema and creating Sample Data Set...
    // Serializing Sample Data Set...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // Result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.


###<a name="sample-3-serialization-using-object-container-files-and-serialization-with-reflection"></a>Esimerkki 3: Sarjatoiminto objektin säilö tiedostojen ja Sarjatoiminto käyttäminen ja heijastus

Tässä esimerkissä on samankaltainen kuin skenaarion <a href="#Scenario1">Ensimmäinen esimerkki</a>, jossa malli on implisiittisesti määritetty ja heijastus. Ero on, tässä, rakenne ei oletetaan tunnetuksi lukijaa, deserializes sitä. Voi muuntaa sarjaksi **SensorData** objektien ja niiden implisiittisesti määritetty rakenteen tallennetaan [**AvroContainer**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.avrocontainer.aspx) -luokan vastaavan Avro objektin säilö tiedoston.

Tässä esimerkissä, on muuntaa sarjaksi tiedot [**SequentialWriter<SensorData> **](http://msdn.microsoft.com/library/dn627340.aspx) ja sarjoitus kanssa [**SequentialReader<SensorData>**](http://msdn.microsoft.com/library/dn627340.aspx). Tulos verrataan sitten alkuperäisen esiintymät varmistamiseksi tunnistetiedot.

Objektin säilö-tiedoston tietojen pakkaus on oletusarvoinen [**Deflate**] kautta[ deflate-100] pakkauksenhallintaa .NET Framework 4. <a href="#Scenario5">Viidennen Esimerkki</a> tämän artikkelin Opettele käyttämään [**Deflate**] Lisää uusimpia ja ylätason version[ deflate-110] sisältyy .NET Framework 4.5 pakkauksenhallintaa.

    namespace Microsoft.Hadoop.Avro.Sample
    {
        using System;
        using System.Collections.Generic;
        using System.IO;
        using System.Linq;
        using System.Runtime.Serialization;
        using Microsoft.Hadoop.Avro.Container;
        using Microsoft.Hadoop.Avro;

        //Sample class used in serialization samples
        [DataContract(Name = "SensorDataValue", Namespace = "Sensors")]
        internal class SensorData
        {
            [DataMember(Name = "Location")]
            public Location Position { get; set; }

            [DataMember(Name = "Value")]
            public byte[] Value { get; set; }
        }

        //Sample struct used in serialization samples
        [DataContract]
        internal struct Location
        {
            [DataMember]
            public int Floor { get; set; }

            [DataMember]
            public int Room { get; set; }
        }

        //This class contains all methods demonstrating
        //the usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serializes and deserializes the sample data set by using reflection and Avro object container files.
            //Serialized data is compressed with the Deflate codec.
            public void SerializeDeserializeUsingObjectContainersReflection()
            {

                Console.WriteLine("SERIALIZATION USING REFLECTION AND AVRO OBJECT CONTAINER FILES\n");

                //Path for Avro object container file
                string path = "AvroSampleReflectionDeflate.avro";

                //Create a data set by using sample class and struct
                var testData = new List<SensorData>
                        {
                            new SensorData { Value = new byte[] { 1, 2, 3, 4, 5 }, Position = new Location { Room = 243, Floor = 1 } },
                            new SensorData { Value = new byte[] { 6, 7, 8, 9 }, Position = new Location { Room = 244, Floor = 1 } }
                        };

                //Serializing and saving data to file.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Serializing Sample Data Set...");

                    //Create a SequentialWriter instance for type SensorData, which can serialize a sequence of SensorData objects to stream.
                    //Data will be compressed using the Deflate codec.
                    using (var w = AvroContainer.CreateWriter<SensorData>(buffer, Codec.Deflate))
                    {
                        using (var writer = new SequentialWriter<SensorData>(w, 24))
                        {
                            // Serialize the data to stream by using the sequential writer
                            testData.ForEach(writer.Write);
                        }
                    }

                    //Save stream to file
                    Console.WriteLine("Saving serialized data to file...");
                    if (!WriteFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }
                }

                //Reading and deserializing data.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Reading data from file...");

                    //Reading data from object container file
                    if (!ReadFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }

                    Console.WriteLine("Deserializing Sample Data Set...");

                    //Prepare the stream for deserializing the data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Create a SequentialReader instance for type SensorData, which will deserialize all serialized objects from the given stream.
                    //It allows iterating over the deserialized objects because it implements the IEnumerable<T> interface.
                    using (var reader = new SequentialReader<SensorData>(
                        AvroContainer.CreateReader<SensorData>(buffer, true)))
                    {
                        var results = reader.Objects;

                        //Finally, verify that deserialized data matches the original one
                        Console.WriteLine("Comparing Initial and Deserialized Data Sets...");
                        int count = 1;
                        var pairs = testData.Zip(results, (serialized, deserialized) => new { expected = serialized, actual = deserialized });
                        foreach (var pair in pairs)
                        {
                            bool isEqual = this.Equal(pair.expected, pair.actual);
                            Console.WriteLine("For Pair {0} result of Data Set Identity Comparison is {1}", count, isEqual);
                            count++;
                        }
                    }
                }

                //Delete the file
                RemoveFile(path);
            }

            //
            //Helper methods
            //

            //Comparing two SensorData objects
            private bool Equal(SensorData left, SensorData right)
            {
                return left.Position.Equals(right.Position) && left.Value.SequenceEqual(right.Value);
            }

            //Saving memory stream to a new file with the given path
            private bool WriteFile(MemoryStream InputStream, string path)
            {
                if (!File.Exists(path))
                {
                    try
                    {
                        using (FileStream fs = File.Create(path))
                        {
                            InputStream.Seek(0, SeekOrigin.Begin);
                            InputStream.CopyTo(fs);
                        }
                        return true;
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during creation and writing to the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                        return false;
                    }
                }
                else
                {
                    Console.WriteLine("Can not create file \"{0}\". File already exists", path);
                    return false;

                }
            }

            //Reading a file content by using the given path to a memory stream
            private bool ReadFile(MemoryStream OutputStream, string path)
            {
                try
                {
                    using (FileStream fs = File.Open(path, FileMode.Open))
                    {
                        fs.CopyTo(OutputStream);
                    }
                    return true;
                }
                catch (Exception e)
                {
                    Console.WriteLine("The following exception was thrown during reading from the file \"{0}\"", path);
                    Console.WriteLine(e.Message);
                    return false;
                }
            }

            //Deleting file by using given path
            private void RemoveFile(string path)
            {
                if (File.Exists(path))
                {
                    try
                    {
                        File.Delete(path);
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during deleting the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                    }
                }
                else
                {
                    Console.WriteLine("Can not delete file \"{0}\". File does not exist", path);
                }
            }

            static void Main()
            {

                string sectionDivider = "---------------------------------------- ";

                //Create an instance of AvroSample class and invoke methods
                //illustrating different serializing approaches
                AvroSample Sample = new AvroSample();

                //Serialization using reflection to Avro object container file
                Sample.SerializeDeserializeUsingObjectContainersReflection();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key to exit.");
                Console.Read();
            }
        }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING REFLECTION AND AVRO OBJECT CONTAINER FILES
    //
    // Serializing Sample Data Set...
    // Saving serialized data to file...
    // Reading data from file...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // For Pair 1 result of Data Set Identity Comparison is True
    // For Pair 2 result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.


###<a name="sample-4-serialization-using-object-container-files-and-serialization-with-generic-record"></a>Esimerkki 4: Sarjatoiminto objektin säilö tiedostot ja Sarjatoiminto käyttäminen yleinen tietue

Tässä esimerkissä on samankaltainen kuin skenaarion <a href="#Scenario2">Toinen esimerkki</a>, jossa rakenne on erikseen määritetty JSON kanssa. Ero on, tässä, rakenne ei oletetaan tunnetuksi lukijaa, deserializes sitä.

Testi tietojoukon kerätään varustetuksi luetteloksi [**AvroRecord**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) objektien eksplisiittisesti määritetyt JSON-rakenteen kautta ja sitten tallennetaan tiedostoon objektin säilö vastaavan [**AvroContainer**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.avrocontainer.aspx) -luokka. Tämän säilötiedoston Luo writer, jota käytetään onnistu tietojen puretaan muistin virtaa, joka on tallennettu tiedosto. Käytetään luomiseen lukija [**Codec.Null**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.codec.null.aspx) -parametri ilmaisee, että näitä tietoja ei pakata.

Tiedot sitten lukea tiedostoa ja poistaa objektien kokoelmaan. Tämän sivustokokoelman verrataan alkuperäisen luettelon Avro tietueita, varmista, että ne ovat samat.


    namespace Microsoft.Hadoop.Avro.Sample
    {
        using System;
        using System.Collections.Generic;
        using System.IO;
        using System.Linq;
        using System.Runtime.Serialization;
        using Microsoft.Hadoop.Avro.Container;
        using Microsoft.Hadoop.Avro.Schema;
        using Microsoft.Hadoop.Avro;

        //This class contains all methods demonstrating
        //the usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serializes and deserializes a sample data set by using a generic record and Avro object container files.
            //Serialized data is not compressed.
            public void SerializeDeserializeUsingObjectContainersGenericRecord()
            {
                Console.WriteLine("SERIALIZATION USING GENERIC RECORD AND AVRO OBJECT CONTAINER FILES\n");

                //Path for Avro object container file
                string path = "AvroSampleGenericRecordNullCodec.avro";

                Console.WriteLine("Defining the Schema and creating Sample Data Set...");

                //Define the schema in JSON
                const string Schema = @"{
                                ""type"":""record"",
                                ""name"":""Microsoft.Hadoop.Avro.Specifications.SensorData"",
                                ""fields"":
                                    [
                                        {
                                            ""name"":""Location"",
                                            ""type"":
                                                {
                                                    ""type"":""record"",
                                                    ""name"":""Microsoft.Hadoop.Avro.Specifications.Location"",
                                                    ""fields"":
                                                        [
                                                            { ""name"":""Floor"", ""type"":""int"" },
                                                            { ""name"":""Room"", ""type"":""int"" }
                                                        ]
                                                }
                                        },
                                        { ""name"":""Value"", ""type"":""bytes"" }
                                    ]
                            }";

                //Create a generic serializer based on the schema
                var serializer = AvroSerializer.CreateGeneric(Schema);
                var rootSchema = serializer.WriterSchema as RecordSchema;

                //Create a generic record to represent the data
                var testData = new List<AvroRecord>();

                dynamic expected1 = new AvroRecord(rootSchema);
                dynamic location1 = new AvroRecord(rootSchema.GetField("Location").TypeSchema);
                location1.Floor = 1;
                location1.Room = 243;
                expected1.Location = location1;
                expected1.Value = new byte[] { 1, 2, 3, 4, 5 };
                testData.Add(expected1);

                dynamic expected2 = new AvroRecord(rootSchema);
                dynamic location2 = new AvroRecord(rootSchema.GetField("Location").TypeSchema);
                location2.Floor = 1;
                location2.Room = 244;
                expected2.Location = location2;
                expected2.Value = new byte[] { 6, 7, 8, 9 };
                testData.Add(expected2);

                //Serializing and saving data to file.
                //Create a MemoryStream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Serializing Sample Data Set...");

                    //Create a SequentialWriter instance for type SensorData, which can serialize a sequence of SensorData objects to stream.
                    //Data will not be compressed (Null compression codec).
                    using (var writer = AvroContainer.CreateGenericWriter(Schema, buffer, Codec.Null))
                    {
                        using (var streamWriter = new SequentialWriter<object>(writer, 24))
                        {
                            // Serialize the data to stream by using the sequential writer
                            testData.ForEach(streamWriter.Write);
                        }
                    }

                    Console.WriteLine("Saving serialized data to file...");

                    //Save stream to file
                    if (!WriteFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }
                }

                //Reading and deserializing the data.
                //Create a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Reading data from file...");

                    //Reading data from object container file
                    if (!ReadFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }

                    Console.WriteLine("Deserializing Sample Data Set...");

                    //Prepare the stream for deserializing the data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Create a SequentialReader instance for type SensorData, which will deserialize all serialized objects from the given stream.
                    //It allows iterating over the deserialized objects because it implements the IEnumerable<T> interface.
                    using (var reader = AvroContainer.CreateGenericReader(buffer))
                    {
                        using (var streamReader = new SequentialReader<object>(reader))
                        {
                            var results = streamReader.Objects;

                            Console.WriteLine("Comparing Initial and Deserialized Data Sets...");

                            //Finally, verify the results
                            var pairs = testData.Zip(results, (serialized, deserialized) => new { expected = (dynamic)serialized, actual = (dynamic)deserialized });
                            int count = 1;
                            foreach (var pair in pairs)
                            {
                                bool isEqual = pair.expected.Location.Floor.Equals(pair.actual.Location.Floor);
                                isEqual = isEqual && pair.expected.Location.Room.Equals(pair.actual.Location.Room);
                                isEqual = isEqual && ((byte[])pair.expected.Value).SequenceEqual((byte[])pair.actual.Value);
                                Console.WriteLine("For Pair {0} result of Data Set Identity Comparison is {1}", count, isEqual.ToString());
                                count++;
                            }
                        }
                    }
                }

                //Delete the file
                RemoveFile(path);
            }

            //
            //Helper methods
            //

            //Saving memory stream to a new file with the given path
            private bool WriteFile(MemoryStream InputStream, string path)
            {
                if (!File.Exists(path))
                {
                    try
                    {
                        using (FileStream fs = File.Create(path))
                        {
                            InputStream.Seek(0, SeekOrigin.Begin);
                            InputStream.CopyTo(fs);
                        }
                        return true;
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during creation and writing to the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                        return false;
                    }
                }
                else
                {
                    Console.WriteLine("Can not create file \"{0}\". File already exists", path);
                    return false;

                }
            }

            //Reading a file content by using the given path to a memory stream
            private bool ReadFile(MemoryStream OutputStream, string path)
            {
                try
                {
                    using (FileStream fs = File.Open(path, FileMode.Open))
                    {
                        fs.CopyTo(OutputStream);
                    }
                    return true;
                }
                catch (Exception e)
                {
                    Console.WriteLine("The following exception was thrown during reading from the file \"{0}\"", path);
                    Console.WriteLine(e.Message);
                    return false;
                }
            }

            //Deleting file by using the given path
            private void RemoveFile(string path)
            {
                if (File.Exists(path))
                {
                    try
                    {
                        File.Delete(path);
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during deleting the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                    }
                }
                else
                {
                    Console.WriteLine("Can not delete file \"{0}\". File does not exist", path);
                }
            }

            static void Main()
            {

                string sectionDivider = "---------------------------------------- ";

                //Create an instance of the AvroSample class and invoke methods
                //illustrating different serializing approaches
                AvroSample Sample = new AvroSample();

                //Serialization using generic record to Avro object container file
                Sample.SerializeDeserializeUsingObjectContainersGenericRecord();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key to exit.");
                Console.Read();
            }
        }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING GENERIC RECORD AND AVRO OBJECT CONTAINER FILES
    //
    // Defining the Schema and creating Sample Data Set...
    // Serializing Sample Data Set...
    // Saving serialized data to file...
    // Reading data from file...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // For Pair 1 result of Data Set Identity Comparison is True
    // For Pair 2 result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.




###<a name="sample-5-serialization-using-object-container-files-with-a-custom-compression-codec"></a>Esimerkki 5: Sarjatoiminto objektin säilö tiedostojen käyttäminen mukautettujen pakkauksenhallintaa

Viidennen esimerkissä käyttäminen mukautettujen pakkauksenhallintaa Avro objektin säilö tiedostojen poistamisesta. [Azure MALLIKOODEJA](http://code.msdn.microsoft.com/windowsazure/Serialize-data-with-the-67159111) -sivustosta voi ladata otoksen, joka sisältää tässä esimerkissä koodi.

[Avro määrityksen](http://avro.apache.org/docs/current/spec.html#Required+Codecs) avulla käyttö valinnainen pakkauksenhallintaa (lisäksi **Null** ja **Deflate** oletusasetukset). Tässä esimerkissä on toteuttaminen ei ole täysin uuden pakkauksenhallinta, kuten Snappy (mainitaan tuetut valinnainen pakkauksenhallinta [Avro määritys](http://avro.apache.org/docs/current/spec.html#snappy)). Se näyttää käyttämisestä [**Deflate**] .NET Framework 4.5 käyttöönoton[ deflate-110] pakkauksenhallinnan, joka tarjoaa paremman pakkaamisen-algoritmin [zlib](http://zlib.net/) pakkaamisen kirjaston kuin .NET Framework 4 oletusversio perusteella.


    //
    // This code needs to be compiled with the parameter Target Framework set as ".NET Framework 4.5"
    // to ensure the desired implementation of the Deflate compression algorithm is used.
    // Ensure your C# project is set up accordingly.
    //

    namespace Microsoft.Hadoop.Avro.Sample
    {
        using System;
        using System.Collections.Generic;
        using System.Diagnostics;
        using System.IO;
        using System.IO.Compression;
        using System.Linq;
        using System.Runtime.Serialization;
        using Microsoft.Hadoop.Avro.Container;
        using Microsoft.Hadoop.Avro;

        #region Defining objects for serialization
        //Sample class used in serialization samples
        [DataContract(Name = "SensorDataValue", Namespace = "Sensors")]
        internal class SensorData
        {
            [DataMember(Name = "Location")]
            public Location Position { get; set; }

            [DataMember(Name = "Value")]
            public byte[] Value { get; set; }
        }

        //Sample struct used in serialization samples
        [DataContract]
        internal struct Location
        {
            [DataMember]
            public int Floor { get; set; }

            [DataMember]
            public int Room { get; set; }
        }
        #endregion

        #region Defining custom codec based on .NET Framework V.4.5 Deflate
        //Avro.NET codec class contains two methods,
        //GetCompressedStreamOver(Stream uncompressed) and GetDecompressedStreamOver(Stream compressed),
        //which are the key ones for data compression.
        //To enable a custom codec, one needs to implement these methods for the required codec.

        #region Defining Compression and Decompression Streams
        //DeflateStream (class from System.IO.Compression namespace that implements Deflate algorithm)
        //cannot be directly used for Avro because it does not support vital operations like Seek.
        //Thus one needs to implement two classes inherited from stream
        //(one for compressed and one for decompressed stream)
        //that use Deflate compression and implement all required features.
        internal sealed class CompressionStreamDeflate45 : Stream
        {
            private readonly Stream buffer;
            private DeflateStream compressionStream;

            public CompressionStreamDeflate45(Stream buffer)
            {
                Debug.Assert(buffer != null, "Buffer is not allowed to be null.");

                this.compressionStream = new DeflateStream(buffer, CompressionLevel.Fastest, true);
                this.buffer = buffer;
            }

            public override bool CanRead
            {
                get { return this.buffer.CanRead; }
            }

            public override bool CanSeek
            {
                get { return true; }
            }

            public override bool CanWrite
            {
                get { return this.buffer.CanWrite; }
            }

            public override void Flush()
            {
                this.compressionStream.Close();
            }

            public override long Length
            {
                get { return this.buffer.Length; }
            }

            public override long Position
            {
                get
                {
                    return this.buffer.Position;
                }

                set
                {
                    this.buffer.Position = value;
                }
            }

            public override int Read(byte[] buffer, int offset, int count)
            {
                return this.buffer.Read(buffer, offset, count);
            }

            public override long Seek(long offset, SeekOrigin origin)
            {
                return this.buffer.Seek(offset, origin);
            }

            public override void SetLength(long value)
            {
                throw new NotSupportedException();
            }

            public override void Write(byte[] buffer, int offset, int count)
            {
                this.compressionStream.Write(buffer, offset, count);
            }

            protected override void Dispose(bool disposed)
            {
                base.Dispose(disposed);

                if (disposed)
                {
                    this.compressionStream.Dispose();
                    this.compressionStream = null;
                }
            }
        }

        internal sealed class DecompressionStreamDeflate45 : Stream
        {
            private readonly DeflateStream decompressed;

            public DecompressionStreamDeflate45(Stream compressed)
            {
                this.decompressed = new DeflateStream(compressed, CompressionMode.Decompress, true);
            }

            public override bool CanRead
            {
                get { return true; }
            }

            public override bool CanSeek
            {
                get { return true; }
            }

            public override bool CanWrite
            {
                get { return false; }
            }

            public override void Flush()
            {
                this.decompressed.Close();
            }

            public override long Length
            {
                get { return this.decompressed.Length; }
            }

            public override long Position
            {
                get
                {
                    return this.decompressed.Position;
                }

                set
                {
                    throw new NotSupportedException();
                }
            }

            public override int Read(byte[] buffer, int offset, int count)
            {
                return this.decompressed.Read(buffer, offset, count);
            }

            public override long Seek(long offset, SeekOrigin origin)
            {
                throw new NotSupportedException();
            }

            public override void SetLength(long value)
            {
                throw new NotSupportedException();
            }

            public override void Write(byte[] buffer, int offset, int count)
            {
                throw new NotSupportedException();
            }

            protected override void Dispose(bool disposing)
            {
                base.Dispose(disposing);

                if (disposing)
                {
                    this.decompressed.Dispose();
                }
            }
        }
        #endregion

        #region Define Codec
        //Define the actual codec class containing the required methods for manipulating streams:
        //GetCompressedStreamOver(Stream uncompressed) and GetDecompressedStreamOver(Stream compressed).
        //Codec class uses classes for compressed and decompressed streams defined above.
        internal sealed class DeflateCodec45 : Codec
        {

            //We merely use different IMPLEMENTATIONS of Deflate, so CodecName remains "deflate"
            public static readonly string CodecName = "deflate";

            public DeflateCodec45()
                : base(CodecName)
            {
            }

            public override Stream GetCompressedStreamOver(Stream decompressed)
            {
                if (decompressed == null)
                {
                    throw new ArgumentNullException("decompressed");
                }

                return new CompressionStreamDeflate45(decompressed);
            }

            public override Stream GetDecompressedStreamOver(Stream compressed)
            {
                if (compressed == null)
                {
                    throw new ArgumentNullException("compressed");
                }

                return new DecompressionStreamDeflate45(compressed);
            }
        }
        #endregion

        #region Define modified Codec Factory
        //Define modified codec factory to be used in the reader.
        //It will catch the attempt to use "Deflate" and provide  a custom codec.
        //For all other cases, it will rely on the base class (CodecFactory).
        internal sealed class CodecFactoryDeflate45 : CodecFactory
        {

            public override Codec Create(string codecName)
            {
                if (codecName == DeflateCodec45.CodecName)
                    return new DeflateCodec45();
                else
                    return base.Create(codecName);
            }
        }
        #endregion

        #endregion

        #region Sample Class with demonstration methods
        //This class contains methods demonstrating
        //the usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serializes and deserializes sample data set by using reflection and Avro object container files.
            //Serialized data is compressed with the custom compression codec (Deflate of .NET Framework 4.5).
            //
            //This sample uses memory stream for all operations related to serialization, deserialization and
            //object container manipulation, though file stream could be easily used.
            public void SerializeDeserializeUsingObjectContainersReflectionCustomCodec()
            {

                Console.WriteLine("SERIALIZATION USING REFLECTION, AVRO OBJECT CONTAINER FILES AND CUSTOM CODEC\n");

                //Path for Avro object container file
                string path = "AvroSampleReflectionDeflate45.avro";

                //Create a data set by using sample class and struct
                var testData = new List<SensorData>
                        {
                            new SensorData { Value = new byte[] { 1, 2, 3, 4, 5 }, Position = new Location { Room = 243, Floor = 1 } },
                            new SensorData { Value = new byte[] { 6, 7, 8, 9 }, Position = new Location { Room = 244, Floor = 1 } }
                        };

                //Serializing and saving data to file.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Serializing Sample Data Set...");

                    //Create a SequentialWriter instance for type SensorData, which can serialize a sequence of SensorData objects to stream.
                    //Here the custom codec is introduced. For convenience, the next commented code line shows how to use built-in Deflate.
                    //Note that because the sample deals with different IMPLEMENTATIONS of Deflate, built-in and custom codecs are interchangeable
                    //in read-write operations.
                    //using (var w = AvroContainer.CreateWriter<SensorData>(buffer, Codec.Deflate))
                    using (var w = AvroContainer.CreateWriter<SensorData>(buffer, new DeflateCodec45()))
                    {
                        using (var writer = new SequentialWriter<SensorData>(w, 24))
                        {
                            // Serialize the data to stream using the sequential writer
                            testData.ForEach(writer.Write);
                        }
                    }

                    //Save stream to file
                    Console.WriteLine("Saving serialized data to file...");
                    if (!WriteFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }
                }

                //Reading and deserializing data.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Reading data from file...");

                    //Reading data from object container file
                    if (!ReadFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }

                    Console.WriteLine("Deserializing Sample Data Set...");

                    //Prepare the stream for deserializing the data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Because of SequentialReader<T> constructor signature, an AvroSerializerSettings instance is required
                    //when codec factory is explicitly specified.
                    //You may comment the line below if you want to use built-in Deflate (see next comment).
                    AvroSerializerSettings settings = new AvroSerializerSettings();

                    //Create a SequentialReader instance for type SensorData, which will deserialize all serialized objects from the given stream.
                    //It allows iterating over the deserialized objects because it implements the IEnumerable<T> interface.
                    //Here the custom codec factory is introduced.
                    //For convenience, the next commented code line shows how to use built-in Deflate
                    //(no explicit Codec Factory parameter is required in this case).
                    //Note that because the sample deals with different IMPLEMENTATIONS of Deflate, built-in and custom codecs are interchangeable
                    //in read-write operations.
                    //using (var reader = new SequentialReader<SensorData>(AvroContainer.CreateReader<SensorData>(buffer, true)))
                    using (var reader = new SequentialReader<SensorData>(
                        AvroContainer.CreateReader<SensorData>(buffer, true, settings, new CodecFactoryDeflate45())))
                    {
                        var results = reader.Objects;

                        //Finally, verify that deserialized data matches the original one
                        Console.WriteLine("Comparing Initial and Deserialized Data Sets...");
                        bool isEqual;
                        int count = 1;
                        var pairs = testData.Zip(results, (serialized, deserialized) => new { expected = serialized, actual = deserialized });
                        foreach (var pair in pairs)
                        {
                            isEqual = this.Equal(pair.expected, pair.actual);
                            Console.WriteLine("For Pair {0} result of Data Set Identity Comparison is {1}", count, isEqual.ToString());
                            count++;
                        }
                    }
                }

                //Delete the file
                RemoveFile(path);
            }
        #endregion

            #region Helper Methods

            //Comparing two SensorData objects
            private bool Equal(SensorData left, SensorData right)
            {
                return left.Position.Equals(right.Position) && left.Value.SequenceEqual(right.Value);
            }

            //Saving memory stream to a new file with the given path
            private bool WriteFile(MemoryStream InputStream, string path)
            {
                if (!File.Exists(path))
                {
                    try
                    {
                        using (FileStream fs = File.Create(path))
                        {
                            InputStream.Seek(0, SeekOrigin.Begin);
                            InputStream.CopyTo(fs);
                        }
                        return true;
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during creation and writing to the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                        return false;
                    }
                }
                else
                {
                    Console.WriteLine("Can not create file \"{0}\". File already exists", path);
                    return false;

                }
            }

            //Reading file content by using the given path to a memory stream
            private bool ReadFile(MemoryStream OutputStream, string path)
            {
                try
                {
                    using (FileStream fs = File.Open(path, FileMode.Open))
                    {
                        fs.CopyTo(OutputStream);
                    }
                    return true;
                }
                catch (Exception e)
                {
                    Console.WriteLine("The following exception was thrown during reading from the file \"{0}\"", path);
                    Console.WriteLine(e.Message);
                    return false;
                }
            }

            //Deleting file by using given path
            private void RemoveFile(string path)
            {
                if (File.Exists(path))
                {
                    try
                    {
                        File.Delete(path);
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during deleting the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                    }
                }
                else
                {
                    Console.WriteLine("Can not delete file \"{0}\". File does not exist", path);
                }
            }
            #endregion

            static void Main()
            {

                string sectionDivider = "---------------------------------------- ";

                //Create an instance of AvroSample Class and invoke methods
                //illustrating different serializing approaches
                AvroSample Sample = new AvroSample();

                //Serialization using reflection to Avro object container file using custom codec
                Sample.SerializeDeserializeUsingObjectContainersReflectionCustomCodec();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key to exit.");
                Console.Read();
            }
        }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING REFLECTION, AVRO OBJECT CONTAINER FILES AND CUSTOM CODEC
    //
    // Serializing Sample Data Set...
    // Saving serialized data to file...
    // Reading data from file...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // For Pair 1 result of Data Set Identity Comparison is True
    //For Pair 2 result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.

###<a name="sample-6-using-avro-to-upload-data-for-the-microsoft-azure-hdinsight-service"></a>Esimerkki 6: Lataa Microsoft Azure HDInsight-palvelun tiedot Avro avulla

Kuudennesta esimerkissä havainnollistetaan joitakin ohjelmoinnin tekniikoita liittyvät Azure HDInsight-palvelun käyttäminen. [Azure MALLIKOODEJA](https://code.msdn.microsoft.com/windowsazure/Using-Avro-to-upload-data-ae81b1e3) -sivustosta voi ladata otoksen, joka sisältää tässä esimerkissä koodi.

Otosten tekee seuraavat toimet:

* Muodostaa yhteyden aiemmin luotuun HDInsight-palvelu-klusteriin.
* Serializes useita CSV-tiedostoja ja lataa tulos Azure-Blob-säiliö. (CSV-tiedostoja yhdessä otosten jaetaan ja edustavat ote tarvikkeiden Stock historiatietoja jaettuna [Infochimps](http://www.infochimps.com/) kauden 1970 2010. Otosten lukee CSV-tiedostotietoja, tietueet muunnetaan **kalusto** -luokan esiintymät ja serializes ne käyttämällä heijastus. Varaston laji määritelmä luodaan JSON rakenteen kautta Microsoft Avro Library koodin luonti-apuohjelma.
* Luo uuden ulkoisen taulukon nimeltä **Osakekaivos** rakenne ja sen tiedot on ladattu edellisessä vaiheessa linkkejä.
* Suorittaa kyselyn käyttämällä **Osakekaivos** taulukon rakennetta.

Lisäksi otosten suorittaa SIIVOA toimet ennen ja sen jälkeen pää toimintoja. Aikana puhdistus kaikki liittyvät Azure-Blob-tiedot ja kansiot poistetaan ja taulukon rakenne on katkaistu. Voit myös käynnistää SIIVOA-toimintosarjan otoksen komentoriviltä.

Malli on seuraavat edellytykset:

* Aktiivinen Microsoft Azure-tilaus ja sen tilauksen tunnus.
* Hallinta-varmenteen tilaukseen, jossa on vastaava yksityinen avain. Nykyisen käyttäjän yksityinen muistiin otosten käyttöä varten tietokoneeseen on asennettava varmenne.
* Aktiivinen HDInsight-klusterin.
* Azure-tallennustilan tilin linkitetty HDInsight-klusterin edellisen edellytyksenä vastaavan ensisijaisen tai toissijaisen access-näppäimen kanssa.

Kaikki edellytykset tiedot kannattaa kirjoittaa, määritys mallitiedostossa ennen otosten suoritetaan. Kahdella mahdollista käsiteltävä:

* Esimerkki pääkansion app.config-tiedoston muokkaaminen ja luoda sitten otosten
* Luo ensin malli, ja valitse Muokkaa AvroHDISample.exe.config muodosta-kansiossa

Kummassakin tapauksessa kaikki muokkaukset pitäisi tehdä myös **<appSettings>** asetukset-osassa. Toimi tiedoston kommentit.
Otosten suoritetaan komentoriviltä suorittamalla seuraavan komennon (jossa otosten .zip tiedoston oletetaan olevan purkaa C:\AvroHDISample; Jos, käytä asiaa tiedostopolku):

    AvroHDISample run C:\AvroHDISample\Data

Tyhjentääksesi klusterin, suorita seuraava komento:

    AvroHDISample clean

[deflate-100]: http://msdn.microsoft.com/library/system.io.compression.deflatestream(v=vs.100).aspx
[deflate-110]: http://msdn.microsoft.com/library/system.io.compression.deflatestream(v=vs.110).aspx
