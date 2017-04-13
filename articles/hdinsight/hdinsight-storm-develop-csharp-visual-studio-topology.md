<properties
   pageTitle="Visual Studio ja C# topologioissa Apache myrsky | Microsoft Azure"
   description="Opettele luomaan myrsky topologioissa C# luomalla yksinkertaisen word Laske-topologian Visual Studiossa Visual Studio HDInsight-työkaluilla."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
   tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="java"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/27/2016"
   ms.author="larryfr"/>

# <a name="develop-c-topologies-for-apache-storm-on-hdinsight-using-hadoop-tools-for-visual-studio"></a>Kehitä C# topologioissa Apache myrsky Hadoop työkaluilla Visual Studio HDInsight-

Opettele luomaan C# myrsky topologian Visual Studio HDInsight-painikkeilla. Tässä opetusohjelmassa käydään läpi myrsky uuden projektin luominen Visual Studiossa, testaaminen paikallisesti ja Apache-myrsky HDInsight klusterissa voidaan ottaa käyttöön.

Opit myös hybrid topologioissa, jotka käyttävät C#- ja Java-osien luominen.

> [AZURE.IMPORTANT] Kun tämän asiakirjan vaiheet ovat riippuvaisia Windows kehitysympäristö Visual Studiossa, käännetty projektin voidaan lähettää Linux- tai Windows-pohjaisesta HDInsight-klusterin. Linux-pohjaiset klustereiden luoda vain 10/28/2016 tuki jälkeen SCP.NET topologioissa.
>
> Jos haluat käyttää C#-topologian Linux-pohjaiset klusterin, sinun on päivitettävä Microsoft.SCP.Net.SDK NuGet paketin käyttämä projektin 0.10.0.6 versioon tai sitä uudemmassa versiossa. Paketin versio on myös vastattava myrsky HDInsight asennettuihin pääversion. Esimerkiksi myrsky HDInsight-versioiden 3.3 ja 3.4 käyttämällä myrsky versio 0.10.x HDInsight 3.5 käyttää myrsky 1.0.x.
> 
> Topologioissa C#-Linux-pohjaiset klustereiden Käytä .NET 4.5, ja käyttää Mono HDInsight-klusterin. Useimmat toimivat, mutta kannattaa tarkistaa mahdolliset yhteensopivuusongelmia [Mono yhteensopivuuden](http://www.mono-project.com/docs/about-mono/compatibility/) asiakirjan.

## <a name="prerequisites"></a>Edellytykset

- Jokin seuraavista versioista Visual Studio

    - Visual Studio 2012 kanssa [Päivitä 4](http://www.microsoft.com/download/details.aspx?id=39305)

    - Visual Studio 2013 [Päivitä 4](http://www.microsoft.com/download/details.aspx?id=44921) tai [Visual Studio 2013 yhteisön](http://go.microsoft.com/fwlink/?LinkId=517284)

    - Visual Studio 2015 tai [Visual Studio 2015 yhteisön](https://go.microsoft.com/fwlink/?LinkId=532606)

- Azure SDK 2.9.5 tai uudempi versio

- HDInsight Tools for Visual Studio: Katso asennetaan ja määritetään HDInsight-Työkalut Visual Studio [HDInsight Tools for Visual Studio käytön aloittamisessa](hdinsight-hadoop-visual-studio-tools-get-started.md) .

    > [AZURE.NOTE] HDInsight Tools for Visual Studio ei tue Visual Studio Express

-   Apache myrsky HDInsight klusterissa: Katso [aloittaminen Apache myrsky HDInsight-](hdinsight-apache-storm-tutorial-get-started.md) ohjeita voit luoda klusterin.

## <a name="templates"></a>Mallit

Visual Studio HDInsight-työkalut tarjoavat seuraavat mallit:

| Projektityyppi | Esittelee |
| ------------ | ------------- |
| Myrsky-sovellus | Tyhjä myrsky topologian projekti |
| Myrsky Azure SQL-Writer malli | Viestin kirjoittaminen Azure SQL-tietokanta |
| Myrsky DocumentDB lukija-Esimerkki | Voit lukea Azure DocumentDB |
| Myrsky DocumentDB Writer malli | Viestin kirjoittaminen Azure DocumentDB |
| Myrsky EventHub lukija-Esimerkki | Voit lukea Azure tapahtuman keskittimet |
| Myrsky EventHub Writer malli | Viestin kirjoittaminen Azure tapahtuman porttiin |
| Myrsky HBase lukija-Esimerkki | Voit lukea HBase-HDInsight klustereiden |
| Myrsky HBase Writer malli | Voit kirjoittaa HBase HDInsight klustereiden |
| Myrsky Hybrid malli | Java-osan käyttäminen |
| Myrsky malli | Perustiedot Wordin Laske-topologian |

> [AZURE.NOTE] HBase ja mallit HBase HDInsight-klusterissa, ei HBase Java-Ohjelmointirajapinnan yhteydenpito HBase REST-Ohjelmointirajapinnalla avulla.

Seuraavien vaiheiden tässä asiakirjassa Luo uusi topologian basic myrsky sovelluksen projektityypin avulla.

## <a name="create-a-c-topology"></a>Luo C#-topologian

1. Jos et ole asentanut uusimman version HDInsight-Työkalut Visual Studio, katso [käyttäminen HDInsight Tools for Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).

2. Avaa Visual Studiossa, valitse **Tiedosto** > **Uusi**ja valitsemalla sitten **Projekti**.

3. **Uusi projekti** -näytöstä, laajenna **asennetut** > **Mallit**ja valitse **Hdinsightista**. Valitse Mallit-luettelosta **Myrsky sovelluksen**. Näytön alalaidassa Lisää **WordCount** sovelluksen nimi.

    ![Kuva](./media/hdinsight-storm-develop-csharp-visual-studio-topology/new-project.png)

4. Kun projekti on luotu, taulukossa pitäisi olla seuraavat tiedostot:

    - **Program.cs**: Tämä määrittää projektin topologian. Huomaa, että oletusarvoisen Hakutopologian, jossa on yksi nokkaan ja yksi lukko luodaan oletusarvon mukaan.

    - **Spout.cs**: esimerkki-nokkaan, tietokoneesta kuuluu äänimerkki satunnaisia lukuja.

    - **Bolt.cs**: esimerkki-lukko, joka säilyttää nokkaan lähettämän lukujen määrän.

    Kun projektin luontia, uusimpien [SCP.NET pakettien](https://www.nuget.org/packages/Microsoft.SCP.Net.SDK/) ladataan NuGet.

    [AZURE.INCLUDE [scp.net version important](../../includes/hdinsight-storm-scpdotnet-version.md)]

Seuraavien osien muokataan projektin basic WordCount-sovellukseen.

### <a name="implement-the-spout"></a>Käyttöönotto nokkaan

1. Avaa **Spout.cs**. Spouts avulla voidaan lukea on verkkotopologia ulkoisesta tietolähteestä. Nokkaan tärkeimmät osat ovat seuraavat:

    - **NextTuple**: kutsuu myrsky, kun nokkaan on oikeus lähettää uusi monikot.

    - **ACK** (vain tapahtumien topologian): tämän nokkaan lähettämät monikot topologian muiden osien Mahdollisuuden löytäjä kuittaussanomien käsittelee. Tiedät, että se käsiteltiin edeltävät osien nokkaan tunnustavat monikon avulla.

    - **Epäonnistuu** (vain tapahtumien topologian): käsittelee monikot, jotka ovat virheiden käsittelyyn topologian muita osia. Tämä tarjoaa mahdollisuuden tulostaminen monikon uudelleen, jotta voit käsitellä uudelleen.

2. Korvaa **Spout** -luokan sisältö seuraavasti. Tämä luo nokkaan, satunnaisesti tietokoneesta kuuluu äänimerkki lauseen topologian kyselyjä.

        private Context ctx;
        private Random r = new Random();
        string[] sentences = new string[] {
            "the cow jumped over the moon",
            "an apple a day keeps the doctor away",
            "four score and seven years ago",
            "snow white and the seven dwarfs",
            "i am at two with nature"
        };

        public Spout(Context ctx)
        {
            // Set the instance context
            this.ctx = ctx;

            Context.Logger.Info("Generator constructor called");

            // Declare Output schema
            Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
            // The schema for the default output stream is
            // a tuple that contains a string field
            outputSchema.Add("default", new List<Type>() { typeof(string) });
            this.ctx.DeclareComponentSchema(new ComponentStreamSchema(null, outputSchema));
        }

        // Get an instance of the spout
        public static Spout Get(Context ctx, Dictionary<string, Object> parms)
        {
            return new Spout(ctx);
        }

        public void NextTuple(Dictionary<string, Object> parms)
        {
            Context.Logger.Info("NextTuple enter");
            // The sentence to be emitted
            string sentence;

            // Get a random sentence
            sentence = sentences[r.Next(0, sentences.Length - 1)];
            Context.Logger.Info("Emit: {0}", sentence);
            // Emit it
            this.ctx.Emit(new Values(sentence));

            Context.Logger.Info("NextTuple exit");
        }

        public void Ack(long seqId, Dictionary<string, Object> parms)
        {
            // Only used for transactional topologies
        }

        public void Fail(long seqId, Dictionary<string, Object> parms)
        {
            // Only used for transactional topologies
        }
    
    Hetki lukea kommenttien ymmärtää koodi mitä avulla.

### <a name="implement-the-bolts"></a>Toteuta Pultit

1. Poistaa projektin **Bolt.cs** aiemmin luotu tiedosto.

2. Napsauta **Ratkaisunhallinnassa**projektin hiiren kakkospainikkeella ja valitse **Lisää** > **Uusi kohde**. Luettelosta Valitse **Myrsky lukko**ja anna nimeksi **Splitter.cs** . Toista tämä, jos haluat luoda toisen bolt nimettyjä **Counter.cs**.

    - **Splitter.cs**: toteuttaa lukko, joka jakaa lauseiden yksittäisiksi sanoiksi ja uusi virta sanojen tietokoneesta kuuluu äänimerkki.

    - **Counter.cs**: toteuttaa lukko, joka laskee kunkin sanan ja tietokoneesta kuuluu äänimerkki uusi virta sanojen ja Laske jokaisen sanan.

    > [AZURE.NOTE] Nämä Pultit ainoastaan lukea ja kirjoittaa virtaa, mutta voit myös käyttää lukko lähteistä, kuten tietokannasta tai palvelun kanssa kommunikoimiseen.

3. Avaa **Splitter.cs**. Huomaa, että se on vain yksi keino oletusarvoisesti: **Suorita**. Tätä kutsutaan, kun laitteen vastaanottaa monikon käsittelyä varten. Tässä kohdassa voit lukea käsittelemään saapuvia monikot ja lähettää lähtevä monikot.

4. Korvaa **jako** -luokan sisältö seuraava koodi:

        private Context ctx;

        // Constructor
        public Splitter(Context ctx)
        {
            Context.Logger.Info("Splitter constructor called");
            this.ctx = ctx;

            // Declare Input and Output schemas
            Dictionary<string, List<Type>> inputSchema = new Dictionary<string, List<Type>>();
            // Input contains a tuple with a string field (the sentence)
            inputSchema.Add("default", new List<Type>() { typeof(string) });
            Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
            // Outbound contains a tuple with a string field (the word)
            outputSchema.Add("default", new List<Type>() { typeof(string) });
            this.ctx.DeclareComponentSchema(new ComponentStreamSchema(inputSchema, outputSchema));
        }

        // Get a new instance of the bolt
        public static Splitter Get(Context ctx, Dictionary<string, Object> parms)
        {
            return new Splitter(ctx);
        }

        // Called when a new tuple is available
        public void Execute(SCPTuple tuple)
        {
            Context.Logger.Info("Execute enter");

            // Get the sentence from the tuple
            string sentence = tuple.GetString(0);
            // Split at space characters
            foreach (string word in sentence.Split(' '))
            {
                Context.Logger.Info("Emit: {0}", word);
                //Emit each word
                this.ctx.Emit(new Values(word));
            }

            Context.Logger.Info("Execute exit");
        }

    Hetki lukea kommenttien ymmärtää koodi mitä avulla.

5. Avaa **Counter.cs** ja luokan sisältö korvaava seuraavasti:

        private Context ctx;

        // Dictionary for holding words and counts
        private Dictionary<string, int> counts = new Dictionary<string, int>();

        // Constructor
        public Counter(Context ctx)
        {
            Context.Logger.Info("Counter constructor called");
            // Set instance context
            this.ctx = ctx;

            // Declare Input and Output schemas
            Dictionary<string, List<Type>> inputSchema = new Dictionary<string, List<Type>>();
            // A tuple containing a string field - the word
            inputSchema.Add("default", new List<Type>() { typeof(string) });

            Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
            // A tuple containing a string and integer field - the word and the word count
            outputSchema.Add("default", new List<Type>() { typeof(string), typeof(int) });
            this.ctx.DeclareComponentSchema(new ComponentStreamSchema(inputSchema, outputSchema));
        }

        // Get a new instance
        public static Counter Get(Context ctx, Dictionary<string, Object> parms)
        {
            return new Counter(ctx);
        }

        // Called when a new tuple is available
        public void Execute(SCPTuple tuple)
        {
            Context.Logger.Info("Execute enter");

            // Get the word from the tuple
            string word = tuple.GetString(0);
            // Do we already have an entry for the word in the dictionary?
            // If no, create one with a count of 0
            int count = counts.ContainsKey(word) ? counts[word] : 0;
            // Increment the count
            count++;
            // Update the count in the dictionary
            counts[word] = count;

            Context.Logger.Info("Emit: {0}, count: {1}", word, count);
            // Emit the word and count information
            this.ctx.Emit(Constants.DEFAULT_STREAM_ID, new List<SCPTuple> { tuple }, new Values(word, count));
            Context.Logger.Info("Execute exit");
        }

    Hetki lukea kommenttien ymmärtää koodi mitä avulla.

### <a name="define-the-topology"></a>Topologian määrittäminen

Spouts ja pultit järjestetään kaaviossa, joka määrittää, kuinka tiedot jatkuu osien välillä. Tämä topologian kaavio on seuraavanlainen:

![kuvan osien lajittelujärjestyksen vaihtaminen](./media/hdinsight-storm-develop-csharp-visual-studio-topology/wordcount-topology.png)

Lauseiden on lähetetty-nokkaan, joka jaetaan osiin lukko esiintymät. Jakopalkki lukko jakaa virkkeet sanoiksi, joka jaetaan laskuri-lukko.

Sanamäärä pidetään paikallisesti laskuri-esiintymän, koska haluat varmistaa, että tiettyjä sanoja flow samalle laskuri lukko esiintymälle, joten vain yhden esiintymän pitäminen ja tietyn sanan. Mutta varten jako-lukko todella valintajärjestyksellä ei ole mitä lukko vastaanottaa mitä virke, niin yksinkertaisesti haluat ladata saldo lauseiden kaikissa tapauksissa.

Avaa **Program.cs**. Tärkeää tapa on **GetTopologyBuilder**, jolla voidaan määrittää, joka lähetetään topologian myrsky. Korvaa **GetTopologyBuilder** sisällön toteuttamisesta kuvatuilla topologian seuraava koodi:

        // Create a new topology named 'WordCount'
        TopologyBuilder topologyBuilder = new TopologyBuilder("WordCount" + DateTime.Now.ToString("yyyyMMddHHmmss"));

        // Add the spout to the topology.
        // Name the component 'sentences'
        // Name the field that is emitted as 'sentence'
        topologyBuilder.SetSpout(
            "sentences",
            Spout.Get,
            new Dictionary<string, List<string>>()
            {
                {Constants.DEFAULT_STREAM_ID, new List<string>(){"sentence"}}
            },
            1);
        // Add the splitter bolt to the topology.
        // Name the component 'splitter'
        // Name the field that is emitted 'word'
        // Use suffleGrouping to distribute incoming tuples
        //   from the 'sentences' spout across instances
        //   of the splitter
        topologyBuilder.SetBolt(
            "splitter",
            Splitter.Get,
            new Dictionary<string, List<string>>()
            {
                {Constants.DEFAULT_STREAM_ID, new List<string>(){"word"}}
            },
            1).shuffleGrouping("sentences");

        // Add the counter bolt to the topology.
        // Name the component 'counter'
        // Name the fields that are emitted 'word' and 'count'
        // Use fieldsGrouping to ensure that tuples are routed
        //   to counter instances based on the contents of field
        //   position 0 (the word). This could also have been
        //   List<string>(){"word"}.
        //   This ensures that the word 'jumped', for example, will always
        //   go to the same instance
        topologyBuilder.SetBolt(
            "counter",
            Counter.Get,
            new Dictionary<string, List<string>>()
            {
                {Constants.DEFAULT_STREAM_ID, new List<string>(){"word", "count"}}
            },
            1).fieldsGrouping("splitter", new List<int>() { 0 });

        // Add topology config
        topologyBuilder.SetTopologyConfig(new Dictionary<string, string>()
        {
            {"topology.kryo.register","[\"[B\"]"}
        });

        return topologyBuilder;

Hetki lukea kommenttien ymmärtää koodi mitä avulla.

## <a name="submit-the-topology"></a>Lähetä topologian

1. **Ratkaisunhallinnassa**projektin hiiren kakkospainikkeella ja valitse **Lähetä myrsky HDInsight**.

    > [AZURE.NOTE] Kirjoita pyydettäessä Azure tilauksen kirjautumisen tunnistetiedot. Jos sinulla on useita tilauksia, kirjaudu haluamasi vaihtoehto, joka sisältää oman myrsky HDInsight-klusterissa.

2. Valitse oman myrsky HDInsight-klusterissa **Myrsky klusterin** avattavasta luettelosta ja valitse sitten **Lähetä**. Voit valvoa Jos lähettäminen onnistuu käyttämällä **kohde** -ikkunassa.

3. Kun topologian on lähetetty, **Myrsky topologioissa** klusterin pitäisi näkyä. Valitse **WordCount** topologian liittyviä tietoja käynnissä topologian luettelosta.

    > [AZURE.NOTE] Voit myös tarkastella **Myrsky topologioissa** **Server**Explorerista: Laajenna **Azure** > **HDInsight**myrsky HDInsight klusterissa hiiren kakkospainikkeella ja valitse sitten **Näytä myrsky topologioissa**.

    Spouts tai Pultit linkkien avulla voit tarkastella tietoja komponentit. Uusi ikkuna avataan kunkin osa on valittuna.

4. **Topologian yhteenveto** -näkymästä valitsemalla Lopeta topologian **lopettaa** .

    > [AZURE.NOTE] Myrsky topologioissa edelleen käytössä, kunnes ne poistetaan käytöstä tai klusterin poistetaan.

## <a name="transactional-topology"></a>Tapahtumien topologian

Edellisen topologian on muiden tapahtumien. Topologian osia Toteuta kaikki toiminnot estotoimintoja viestejä, jos käsittely epäonnistuu topologian komponentti mukaan. Esimerkki tapahtumien topologian uuden projektin luominen ja projektin tyypiksi **Myrsky malli** .

Tapahtumien topologioissa Toteuta tukemaan uusinnan tiedot seuraavasti:

- **Metatietojen välimuistin**: nokkaan täytyy tallentaa metatietoja lähettämän niin, että tiedot voit hakea ja voimakkuus uudelleen virheen ilmetessä tiedoista. Koska otosten lähettämän tiedot ovat pieniä, kunkin monikon raaka tiedot tallennetaan uusinnan sanastoa.

- **ACK**: topologian kunkin lukko soittaa `this.ctx.Ack(tuple)` ack, että se on onnistuneesti käsittelee monikon. Kun kaikki Pultit on Jäljitetyt monikon, `Ack` nokkaan menetelmä käynnistetään. Näin voit poistaa uusinnan välimuistiin tallennetut tiedot, koska tiedot kokonaan käsiteltiin nokkaan.

- **Epäonnistui**: kunkin lukko soittaa `this.ctx.Fail(tuple)` osoittamaan, että käsittely ei onnistu monikon. Virheen lisätään `Fail` nokkaan, jossa monikon sieppaamaa käyttämällä maksutavan välimuistiin metatiedot.

- **Numerosarjan tunnuksen**: kun lähettävä monikon määritetty järjestys-tunnus. Tämä on oltava arvo, joka määrittää monikon toista (Ack ja virheiden) käsittelyä varten. Esimerkiksi **Myrsky otoksen** projektin nokkaan käyttää seuraavasti, kun lähettää tietoja:

        this.ctx.Emit(Constants.DEFAULT_STREAM_ID, new Values(sentence), lastSeqId);

    Uusi kyselyä, joka sisältää lauseen oletusarvo-muodossa, jossa sarjan tunnus arvon **lastSeqId**tietokoneesta kuuluu äänimerkki. Tässä esimerkissä **lastSeqId** suurenee riittää, että jokaisen kyselyllä voimakkuus.

Osoittanut **Myrsky otoksen** Projectissa, onko osa tapahtumien voidaan määrittää suorituksen aikana määrittäminen perustuu.

## <a name="hybrid-topology"></a>Hybrid topologian

HDInsight tools for Visual Studio voidaan myös topologioissa, jossa jotkin osat ovat C# ja toiset ovat Java hybrid luomiseen.

Uuden projektin luominen Esimerkki hybrid topologian, ja valitse **Myrsky Hybrid malli**. Tämä luo on täysin kommentit otoksen, joka sisältää useita topologioissa, jotka kuvaavat seuraavasti:

- **Java spout** ja **C# lukko**: **HybridTopology_javaSpout_csharpBolt** määritelty

    - Tapahtumien versio on määritetty **HybridTopologyTx_javaSpout_csharpBolt**

- **C# spout** ja **Java lukko**: **HybridTopology_csharpSpout_javaBolt** määritelty

    - Tapahtumien versio on määritetty **HybridTopologyTx_csharpSpout_javaBolt**

        > [AZURE.NOTE] Tämä versio käsittelee myös käyttää Clojure koodin tekstitiedostosta Java-komponentti.

Vaihtaa topologian, jota käytetään, kun projektin lähetetään siirtyy `[Active(true)]` topologian, jota haluat käyttää ennen sen lähettämistä klusterin-lauseen.

> [AZURE.NOTE] Kaikki tarvittavat Java-tiedostot ovat osana projektin **JavaDependency** -kansiossa.

Ota huomioon seuraavat asiat, kun luominen ja lähettäminen hybrid topologian:

- Voit luoda uuden esiintymän Java-luokan nokkaan tai lukko on käytettävä **JavaComponentConstructor** .

- **microsoft.scp.storm.multilang.CustomizedInteropJSONSerializer** käytetään onnistu tietojen tai pois Java osat Java-objektien JSON-muotoon.

- Lähetettäessä topologian palvelimeen on määritettävä **Java tiedostopolkujen** **lisämäärityksiä** -vaihtoehdon avulla. Määritetyssä polussa on oltava hakemiston, joka sisältää PURKKI-tiedostoja, jotka sisältävät Java-luokat.

### <a name="azure-event-hubs"></a>Azure tapahtuman keskittimet

SCP.Net versio 0.9.4.203 esitellään uusi luokka ja menetelmä erityisesti käsitteleminen-tapahtuman keskittimeen Spout (Java nokkaan, jossa lukee tapahtuma-toiminnosta.) Kun luot Hakutopologian, jossa käytetään tätä nokkaan, tekemällä seuraavat toimet:

- **EventHubSpoutConfig** luokan: Luo objekti, joka sisältää nokkaan osan määritys

- **TopologyBuilder.SetEventHubSpout** menetelmä: tapahtumaa-toiminnossa Spout osa lisätään topologian

> [AZURE.NOTE] Kun nämä helpompi käsitellä tapahtuman keskittimeen Spout kuin muut Java-osat, sinun on käytettävä CustomizedInteropJSONSerializer edelleen onnistu nokkaan tuottamat tiedot.

## <a id="configurationmanager"></a>ConfigurationManager käyttäminen

Älä käytä ConfigurationManager haettaessa määritysten arvoihin lukko- ja spout osia. johtaa null-osoitin poikkeuksen. Sen sijaan projektin määrityskohde välitetään myrsky topologian kuin avain/arvo-pari topologian kontekstissa. Kukin osa, joka on riippuvainen määritysten arvot on hakea ne yhteydessä aikana.

Seuraava koodi esittelee noutamisesta seuraavat arvot:

    public class MyComponent : ISCPBolt
    {
        // To hold configuration information loaded from context
        Configuration configuration;
        ...
        public MyComponent(Context ctx, Dictionary<string, Object> parms)
        {
            // Save a copy of the context for this component instance
            this.ctx = ctx;
            // If it exists, load the configuration for the component
            if(parms.ContainsKey(Constants.USER_CONFIG))
            {
                this.configuration = parms[Constants.USER_CONFIG] as System.Configuration.Configuration;
            }
            // Retrieve the value of "Foo" from configuration
            var foo = this.configuration.AppSettings.Settings["Foo"].Value;
        }
        ...
    }

Jos käytät `Get` menetelmä palauttaa-komponentin esiintymä, varmista, että se kulkee molemmat `Context` ja `Dictionary<string, Object>` konstruktoria parametrit. Seuraavassa esimerkissä on käyttää basic `Get` menetelmä, joka välittää oikein seuraavat arvot:

    public static MyComponent Get(Context ctx, Dictionary<string, Object> parms)
    {
        return new MyComponent(ctx, parms);
    }

## <a name="how-to-update-scpnet"></a>SCP.NET päivittäminen

SCP.NET uusimmat versiot tukevat paketin päivitys NuGet kautta. Kun uusi päivitys on saatavilla, saat päivitysilmoitus. Voit tarkistavan on päivitettävä manuaalisesti toimimalla seuraavasti:

1. **Ratkaisunhallinnassa**projektin hiiren kakkospainikkeella ja valitse **NuGet pakettien hallinta**.

2. Valitse paketti manager- **päivitykset**. Jos on saatavilla päivitys, se merkitään. Valitse paketti, voit asentaa sen **Päivitä** -painiketta.

> [AZURE.IMPORTANT] Jos projektiin on luotu toisella SCP.NET, joka ei ole käyttänyt NuGet paketin päivitykset aiemmissa versioissa, on suoritettava päivittää uuteen versioon seuraavasti:
>
> 1. **Ratkaisunhallinnassa**projektin hiiren kakkospainikkeella ja valitse **NuGet pakettien hallinta**.
> 2. Käytä **hakukentän** , Etsi ja lisää, **Microsoft.SCP.Net.SDK** projektiin.

## <a name="troubleshooting"></a>Vianmääritys

### <a name="null-pointer-exceptions"></a>Null-osoitin poikkeukset

Kun käytät C#-topologian Linux-pohjaiset HDInsight-klusterin kanssa, lukita ja spout osat, jotka käyttävät ConfigurationManager lukemaan määritysasetukset suorituksen voi palauttaa null-osoitin poikkeukset. Tämä johtuu siitä ladattu toimialueen määritystä ei ole kokoonpano, joka sisältää projektisi.

Projektin määritys on siirretty myrsky topologian kuin avain/arvo-pari topologian kontekstissa ja voi hakea sanasto-objekti, joka siirretään komponentit, kun ne ovat alustaa.

Seuraavassa esimerkissä on kuvattu ladataan määritysten arvot topologian yhteydessä on tämän artikkelin [ConfigurationManager](#configurationmanager) -osio.

### <a name="systemtypeloadexception"></a>System.TypeLoadException

Kun käytät C#-topologian Linux-pohjaiset HDInsight-klusterin kanssa, t seuraavan virheilmoituksen:

    System.TypeLoadException: Failure has occurred while loading a type.

Tässä yleensä occurrs, kun käytössäsi on binaarinen, joka ei ole yhteensopiva, joka tukee mono .NET-version kanssa.

Varmista varten Linux-pohjaiset HDInsight klustereiden projektin käyttää binaaritiedostot käännetty .NET 4.5 varten.


### <a name="test-a-topology-locally"></a>Testaa topologian paikallisesti

Vaikka on helppoa topologian käyttöön klusterin, joissakin tapauksissa joudut ehkä Testaa topologian paikallisesti. Seuraavien vaiheiden avulla voit suorittaa ja testaa Esimerkki topologian Tässä opetusohjelmassa paikallisesti development-ympäristössäsi.

> [AZURE.WARNING] Paikallinen testaaminen toimii vain perustiedot, C# vain topologioissa. Älä käytä paikallisen testaus hybrid topologioissa tai topologioissa, jotka käyttävät useita virtaa, kun näyttöön tulee virhesanomia.

1. **Ratkaisunhallinnassa**projektin hiiren kakkospainikkeella ja valitse **Ominaisuudet**. Muuta projektin ominaisuuksien **tulosteen tyyppi** **Console**-sovellukseen.

    ![tulosteen tyyppi](./media/hdinsight-storm-develop-csharp-visual-studio-topology/outputtype.png)

    > [AZURE.NOTE] Muista **tulosteen tyypin** muuttaminen takaisin **Luokkakirjasto** ennen niiden käyttöönottoa topologian klusteriin.

2. **Ratkaisunhallinnassa**projektin hiiren kakkospainikkeella ja valitse **Lisää** > **Uusi kohde**. Valitse **luokka** ja kirjoita **LocalTest.cs** kuin luokkanimi. Valitsemalla **Lisää**.

3. Avaa **LocalTest.cs** ja lisää seuraavan **käyttäminen** lauseen yläreunassa:

        using Microsoft.SCP;

4. Käyttää **LocalTest** -luokan sisältö seuraavasti:

        // Drives the topology components
        public void RunTestCase()
        {
            // An empty dictionary for use when creating components
            Dictionary<string, Object> emptyDictionary = new Dictionary<string, object>();

            #region Test the spout
            {
                Console.WriteLine("Starting spout");
                // LocalContext is a local-mode context that can be used to initialize
                // components in the development environment.
                LocalContext spoutCtx = LocalContext.Get();
                // Get a new instance of the spout, using the local context
                Spout sentences = Spout.Get(spoutCtx, emptyDictionary);

                // Emit 10 tuples
                for (int i = 0; i < 10; i++)
                {
                    sentences.NextTuple(emptyDictionary);
                }
                // Use LocalContext to persist the data stream to file
                spoutCtx.WriteMsgQueueToFile("sentences.txt");
                Console.WriteLine("Spout finished");
            }
            #endregion

            #region Test the splitter bolt
            {
                Console.WriteLine("Starting splitter bolt");
                // LocalContext is a local-mode context that can be used to initialize
                // components in the development environment.
                LocalContext splitterCtx = LocalContext.Get();
                // Get a new instance of the bolt
                Splitter splitter = Splitter.Get(splitterCtx, emptyDictionary);


                // Set the data stream to the data created by the spout
                splitterCtx.ReadFromFileToMsgQueue("sentences.txt");
                // Get a batch of tuples from the stream
                List<SCPTuple> batch = splitterCtx.RecvFromMsgQueue();
                // Process each tuple in the batch
                foreach (SCPTuple tuple in batch)
                {
                    splitter.Execute(tuple);
                }
                // Use LocalContext to persist the data stream to file
                splitterCtx.WriteMsgQueueToFile("splitter.txt");
                Console.WriteLine("Splitter bolt finished");
            }
            #endregion

            #region Test the counter bolt
            {
                Console.WriteLine("Starting counter bolt");
                // LocalContext is a local-mode context that can be used to initialize
                // components in the development environment.
                LocalContext counterCtx = LocalContext.Get();
                // Get a new instance of the bolt
                Counter counter = Counter.Get(counterCtx, emptyDictionary);

                // Set the data stream to the data created by splitter bolt
                counterCtx.ReadFromFileToMsgQueue("splitter.txt");
                // Get a batch of tuples from the stream
                List<SCPTuple> batch = counterCtx.RecvFromMsgQueue();
                // Process each tuple in the batch
                foreach (SCPTuple tuple in batch)
                {
                    counter.Execute(tuple);
                }
                // Use LocalContext to persist the data stream to file
                counterCtx.WriteMsgQueueToFile("counter.txt");
                Console.WriteLine("Counter bolt finished");
            }
            #endregion
        }

    Lue läpi koodin hetki. Koodi käyttää **LocalContext** Suorita osat kehitysympäristö, ja se toistuu tietovirta tiedostot paikallisesta asemasta osien välillä.

5. Avaa **Program.cs** ja lisää seuraava teksti **Main** -menetelmää:

        Console.WriteLine("Starting tests");
        System.Environment.SetEnvironmentVariable("microsoft.scp.logPrefix", "WordCount-LocalTest");
        // Initialize the runtime
        SCPRuntime.Initialize();

        //If we are not running under the local context, throw an error
        if (Context.pluginType != SCPPluginType.SCP_NET_LOCAL)
        {
            throw new Exception(string.Format("unexpected pluginType: {0}", Context.pluginType));
        }
        // Create test instance
        LocalTest tests = new LocalTest();
        // Run tests
        tests.RunTestCase();
        Console.WriteLine("Tests finished");
        Console.ReadKey();

6. Tallenna muutokset ja valitse **F5-näppäintä** tai valitse **Virheenkorjaus** > Aloita projektin**Käynnistä virheenkorjaus** . Konsoli-ikkuna tulee näkyvät ja kirjaudu tila testien edistymisen. Kun **testit suoritettu** tulee näkyviin, paina jotakin näppäintä Sulje ikkuna.

7. Etsi kansio, joka sisältää projektisi, kuten **Windowsin Resurssienhallinnan** avulla **C:\Users\<your_user_name > \Documents\Visual Studio 2013\Projects\WordCount\WordCount**. Tässä kansiossa Avaa **luokka**ja valitse sitten **Korjaa**. Näkyviin tulee tekstitiedostoja, jotka on valmistettu, kun testejä suoritettiin: sentences.txt, counter.txt ja splitter.txt. Avaa jokaisen tekstitiedosto ja tarkista tiedot.

    > [AZURE.NOTE] Merkkijonon tietotyyppi on samanlainen matriisina desimaaleja arvojen nämä tiedostot. Esimerkiksi \[[97,103,111]] **splitter.txt** tiedosto on sana "ja".

Perustiedot Wordin testaaminen laskeminen sovelluksen paikallisesti on melko trivial todellinen arvo on, kun käytössäsi on monimutkaisia verkkotopologia, joka on yhteydessä ulkoisiin tietolähteisiin tai suorittaa monimutkaisessa tietojen analysoinnissa. Kun käsittelet kyseistä projektia, voit joutua keskeytyskohtia ja käydä läpi koodin määrittämisestä komponentit eristää ongelmat.

> [AZURE.NOTE] Muista määrittää **projektityypin** takaisin **Luokkakirjasto** ennen kuin otat käyttöön myrsky HDInsight-klusterissa.

### <a name="log-information"></a>Lokitieto

Voit helposti kirjautua tiedot topologian-komponenteilta käyttämällä `Context.Logger`. Seuraavassa esimerkiksi luoda tiedoksi lokitapahtuman:

    Context.Logger.Info("Component started");

Kirjautuneena tietoja voidaan tarkastella **Hadoop loki**, joka löytyy **Palvelimen**Resurssienhallinnassa. Laajenna oman myrsky HDInsight-klusterissa tapahtuma ja valitse Laajenna **Hadoop loki**. Valitse lopuksi lokitiedosto tarkastelemiseen.

> [AZURE.NOTE] Lokit tallennetaan Azure-tallennustilan tilin, jota käytetään yhteyttä klusterin. Jos kyseessä on eri kuin se, jonka olet kirjautunut Visual Studiossa tilauksen, sinun on kirjauduttava tilaukseen, joka sisältää tallennustilan tili, jota haluat tarkastella tietoja.

### <a name="view-error-information"></a>Virhetietojen tarkasteleminen

Jos haluat tarkastella on käynnissä topologian tapahtuneista virheistä, tekemällä seuraavat toimet:

1. **Palvelimen Explorer**HDInsight-klusterissa myrsky hiiren kakkospainikkeella ja valitse **näkymän myrsky topologioissa**.

2. **Spout** ja **Bolts** **Viimeisin virhe** -sarakkeessa on lisätietoja viimeisen virheestä, joka on tapahtunut.

3. Valitse osa, joka on lueteltu virhe **Spout** tai **Lukko tunnus** . Valitse tiedot-sivulla, joka tulee näkyviin virheen lisätietoja näkyvät sivun alareunassa **virheet** -kohdassa.

4. Saadaksesi lisätietoja Valitse **Port (portti)** sivulle nähdäkseen myrsky työntekijä lokin viimeisen minuuttien **pesänselvittäjät** -osa.

## <a name="next-steps"></a>Seuraavat vaiheet

Nyt kun olet oppinut kehittäminen ja käyttöönotto Visual Studio myrsky topologioissa HDInsight-työkaluista, katso, miten [tapahtumien Azure tapahtuma-toiminnosta ja myrsky HDInsight](hdinsight-storm-develop-csharp-event-hub-topology.md).

Esimerkki C#-topologian, joka jakaa stream tietojen tuominen useita virtaa Katso [C# myrsky esimerkkiä](https://github.com/Blackmist/csharp-storm-example).

Saat tietää C# topologioissa luontiin liittyviä lisätietoja seuraavasta [SCP.NET GettingStarted.md](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/SCPNet-GettingStarted.md).

Lisää esimerkkejä HDInsight ja lisää myrsky-HDinsight esimerkkejä, joiden on seuraavissa artikkeleissa:

**Microsoft SCP.NET**

* [SCP ohjelmoinnin opas](hdinsight-storm-scp-programming-guide.md)

**Valitse HDInsight Apache myrsky**

- [Ottaa käyttöön ja topologioissa Apache myrsky HDInsight-ja seuranta](hdinsight-storm-deploy-monitor-topology.md)

- [Esimerkki topologioissa myrsky HDInsight-varten](hdinsight-storm-example-topology.md)

**Apache Hadoop-Hdinsightiin**

- [Hadoop HDInsight-rakenteen käyttäminen](hdinsight-use-hive.md)

- [Possu käyttäminen Hadoop-Hdinsightiin](hdinsight-use-pig.md)

- [Hadoop-HDInsight MapReduce käyttäminen](hdinsight-use-mapreduce.md)

**Valitse HDInsight Apache HBase**

- [HBase HDInsight-käytön aloittaminen](hdinsight-hbase-tutorial-get-started.md)
