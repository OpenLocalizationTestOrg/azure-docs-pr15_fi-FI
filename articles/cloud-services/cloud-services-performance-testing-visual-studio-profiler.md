<properties 
    pageTitle="Profilointi pilvipalvelussa paikallisesti Laske emulaattori | Microsoft Azure" 
    services="cloud-services"
    description="Tutkia ja Visual Studio Profilerin suorituskykyongelmia cloud Services-palveluissa" 
    documentationCenter=""
    authors="TomArcher" 
    manager="douge" 
    editor=""
    tags="" 
    />

<tags 
    ms.service="cloud-services" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="07/30/2016" 
    ms.author="tarcher"/>

# <a name="testing-the-performance-of-a-cloud-service-locally-in-the-azure-compute-emulator-using-the-visual-studio-profiler"></a>Testaus pilvipalveluun suorituskyvyn paikallisesti käyttämällä Visual Studio Profilerin Azure Laske emulaattori

Erilaisten työkalujen ja tekniikoita ovat käytettävissä testikäyttöön pilvipalveluihin suorituskykyä.
Kun julkaiset Azure pilvipalveluun, voit määrittää Visual Studio profiloinnin tietojen kerääminen ja analysoida se paikallisesti sellaisena kuvatun [profilointi Azure-sovelluksen][1].
Voit käyttää myös diagnostiikka voit seurata suorituskykylaskureita erilaisia kuvatulla tavalla [käyttäminen suorituskyvyn laskureita Azure-tietokannassa][2].
Voit myös profiilin paikallisesti Laske emulaattori-sovelluksen ennen kuin otat pilveen.

Tässä artikkelissa käsitellään suorittimen esimerkkejä menetelmä, profilointi, jotka voidaan tehdä paikallisesti emulaattori. Suorittimen esimerkkejä on keinot profilointi, joka ei ole hyvin tunkeutuva. Nimetty esimerkkejä määritetyn profiloija kestää tilannevedoksen puhelun pinossa. Tietojen kerääminen ajanjakson aikana ja raportissa näkyvät. Tätä menetelmää profilointi yleensä ilmaisemaan, mihin laskennallisesti tehostettu sovelluksessa suurimman osan suorittimen tehdään.  Näin mahdollisuuden keskittyminen "kuuma polku-kohtaa, johon sovelluksesi tehtäviinsä eniten aikaa.



## <a name="1-configure-visual-studio-for-profiling"></a>1: profilointi Visual Studio määrittäminen

Ensin on muutama Visual Studio määritysvaihtoehtoja, joita voi olla hyötyä, kun profilointi. Selventämään profiloinnin raportteja, sinun on merkkejä (.pdb-tiedostot) sovellusta ja myös järjestelmän kirjastojen symbolit. Haluat varmistaaksesi, että viittaus käytettävissä symboli-palvelimiin. Visual Studio **Työkalut** -valikon toiminto valitsemalla **asetukset**ja valitse sitten **Virheenkorjaus**, valitse **symbolit**. Varmista, että Microsoft symboli-palvelimet näkyy **symbolin oletuskansiot (.pdb)**.  Voit luoda viittauksen http://referencesource.microsoft.com/symbols, joka voi olla symboli-tiedostoja.

![Merkin asetukset][4]

Halutessasi voit yksinkertaistaa raportteja, jotka profiloija Luo määrittämällä vain oman koodin. Vain oma koodia sisältävien käytössä funktio puhelun pinoa on yksinkertaistettu niin, että puhelut kokonaan sisäinen kirjastoista ja .NET Framework piilotetaan raportit. Valitse **Työkalut** -valikosta **asetukset**. Laajenna **Suorituskyvyn Työkalut** -solmu ja valitse **Yleiset**. Valitse valintaruutu, jos **käyttöön vain oman**koodin kuvaajan raporteille.

![Vain oman koodin asetukset][17]

Voit käyttää näitä ohjeita aiemmin luodun projektin tai uuden projektin kanssa.  Jos luot uuden projektin, kokeile jäljempänä tekniikoita, valitse C# **Azure pilvipalvelussa** projekti ja valitse **Verkko-roolin** ja **Työntekijän rooli**.

![Azure pilvipalvelussa projektiroolit][5]

Esimerkiksi tarkoituksiin, Lisää projekti, jossa kellonajan keventää ja esitellään joitakin ilmeisimmät suorituskykyongelma lisäkoodin. Lisää esimerkiksi työntekijän rooli projektin seuraava koodi:

    public class Concatenator
    {
        public static string Concatenate(int number)
        {
            int count;
            string s = "";
            for (count = 0; count < number; count++)
            {
                s += "\n" + count.ToString();
            }
            return s;
        }
    }

Kutsua koodi Työntekijä-roolin RoleEntryPoint johdetun luokan RunAsync-menetelmää. (Ohita käynnissä synkronoidusti menetelmä varoittaa.)

        private async Task RunAsync(CancellationToken cancellationToken)
        {
            // TODO: Replace the following with your own logic.
            while (!cancellationToken.IsCancellationRequested)
            {
                Trace.TraceInformation("Working");
                Concatenator.Concatenate(10000);
            }
        }

Luoda ja suorittaa pilvipalvelussa sijaitsevaan paikallisesti ilman virheenkorjaus (Ctrl + F5)-ratkaisun kokoonpanon määrittäminen **versioon**. Näin varmistat, että kaikki tiedostot ja kansiot luodaan käytössä sovelluksen paikallisesti ja varmistaa, että kaikki emulointeja on käynnistetty. Aloita Laske emulaattorin Käyttöliittymän tehtäväpalkista varmistaaksesi, että työntekijä tehtäväsi on käynnissä.

## <a name="2-attach-to-a-process"></a>2: prosessin liittäminen

Sen sijaan, että profilointi sovelluksen aloittamalla se Visual Studio 2010 IDE, sinun on liitettävä profiloija käynnissä oleva prosessi. 

Voit liittää profiloija prosessi, valitse **Analysoi** -valikossa Valitse **Profilerin** ja **Liitä tai irrota**.

![Liitä profiili-vaihtoehto][6]

Etsi Työntekijä-roolin WaWorkerHost.exe prosessi.

![WaWorkerHost prosessi][7]

Jos projektikansio on verkkoasemassa, profiloija pyytää sinua antamaan toiseen sijaintiin, voit tallentaa profiloinnin raportteja.

 Voit liittää web-roolin WaIISHost.exe liittämällä.
Jos sovellus on useita työntekijän rooli prosesseja, sinun on käytettävä prosessin tunnus voidaan erottaa. Voit tehdä kyselyn prosessin tunnus ohjelmallisesti käyttämällä prosessi-objekti. Esimerkiksi jos koodi on lisätty Run-menetelmän RoleEntryPoint johdetun luokan rooli, voit hakea lokista käyttöliittymässä Laske emulaattorin tietää, mitä prosessi, jos haluat muodostaa yhteyden.

    var process = System.Diagnostics.Process.GetCurrentProcess();
    var message = String.Format("Process ID: {0}", process.Id);
    Trace.WriteLine(message, "Information");

Tarkastele lokia, Aloita Laske emulaattorin Käyttöliittymän.

![Käynnistä Laske emulaattorin Käyttöliittymä][8]

Avaa työntekijän rooli log konsoli-ikkunan Laske emulaattorin käyttöliittymä valitsemalla console-ikkunan otsikkorivillä. Näet lokissa Prosessitunnus.

![Näytä tunnus][9]

Yksi olet liittänyt, Suorita vaiheet sovelluksen käyttöliittymässä (tarvittaessa) toistamisen skenaariota.

Kun haluat lopettaa profilointi, valitse **Lopeta profilointi** -linkki.

![Lopeta profilointi vaihtoehto][10]

## <a name="3-view-performance-reports"></a>3: Näytä raportteja

Sovelluksen suorituskykyraportti näkyy.

Tässä vaiheessa profiloija suorittaminen lopetetaan, tiedot tallennetaan .vsp tiedoston ja näyttää raportin, joka näkyy analyysi tiedoista.

![Profilerin raportti][11]


Jos näet String.wstrcpy kuuma polun, valitse vain omat koodin voit muuttaa näkymän näyttämään vain Käyttäjäkoodi.  Jos näet String.Concat, yritä Näytä kaikki koodi-painiketta.

Pitäisi näkyä KETJUTA menetelmä ja vie ylimääräistä suoritusaika suuri osa String.Concat.

![Raportin analyysi][12]

Jos olet lisännyt koodin merkkijonon yhdistäminen tämän artikkelin, tämän ominaisuuden pitäisi näkyä varoitus tehtäväluettelossa. Voit myös nähdä varoituksen olevan muistista, joka on vuoksi merkkijonoja, jotka on luotu ja myyty määrä liiallinen määrä.

![Suorituskyvyn varoitukset][14]

## <a name="4-make-changes-and-compare-performance"></a>4: muutosten tekeminen ja vertaa suorituskyky

Voit myös vertailla suorituskyvyn ennen ja sen jälkeen koodin muutos.  Pysäyttää käynnissä ja korvaa merkkijonon yhdistäminen-toiminto on käytettävä StringBuilder-koodin muokkaaminen:

    public static string Concatenate(int number)
    {
        int count;
        System.Text.StringBuilder builder = new System.Text.StringBuilder("");
        for (count = 0; count < number; count++)
        {
             builder.Append("\n" + count.ToString());
        }
        return builder.ToString();
    }

Tee toinen suorituskyvyn Suorita ja vertaa sitten suorituskykyä. Suorituskyky-Resurssienhallinnassa Jos toimii samalla istunnon aikana, voit vain valita molempiin, Avaa pikavalikko, ja valitse **Vertaa raportteja**. Jos haluat verrata toisessa suorituskyvyn istunnossa asennuksen kanssa, Avaa **Analysoi** -valikko ja valitse **Vertaa raportteja**. Määrittää molempien tiedostojen tulevasta valintaikkunasta.

![Vertaa suorituskyvyn raportit-vaihtoehto][15]

Raporttien korostettu kaksi suoritetaan välisiä eroja.

![Raportin vertailu][16]

Onnittelen! Olet saanut aloittanut profiloija kanssa.

## <a name="troubleshooting"></a>Vianmääritys

- Varmista, että ovat profilointi Release muodosta ja Käynnistä virheenkorjaus ilman.

- Jos Liitä tai irrota-vaihtoehto ei ole otettu käyttöön Profiler-valikon ohjatun suorituskykyä.

- Laske emulaattorin Käyttöliittymän avulla voit tarkastella sovelluksesi tila. 

- Jos ongelmia sovellusten käynnistäminen emulaattori tai liittämällä Profilerin, sulje suorittaminen emulaattori alas ja käynnistä se uudelleen. Jos tämä ei ratkaise ongelmaa, kokeile uudelleenkäynnistyksen. Tämä ongelma voi ilmetä, jos käytät Laske emulaattori keskeyttää ja Poista käytössä ominaisuuksissa.

- Jos olet käyttänyt profiloinnin komentoja komentoriviltä, erityisesti yleisten asetusten, varmista, että VSPerfClrEnv /globaloff on kutsuttu ja että VsPerfMon.exe on suljettu.

- Jos näyte, kun näyttöön tulee sanoma "PRF0025: allekirjoitukseen ei ole tietoja," Tarkista, että, liitetty prosessi on suorittimen tehtävän. Sovellukset, jotka tekevät ei laskennallinen työ ei voi tuottaa esimerkkejä mitään tietoja.  On myös mahdollista, että prosessi lopetettu minkä tahansa esimerkkejä määritetty ennen tehtävien. Tarkista, että rooli, joka on profilointi Run-menetelmän ei pääty.

## <a name="next-steps"></a>Seuraavat vaiheet

Azure-emulaattori binaaritiedostot instrumenting ei tueta Visual Studio Profilerin, mutta jos haluat testata muistin varaamista, voit valita vaihtoehdon kun profilointi. Voit valita myös samanaikainen profilointi, joiden avulla voit selvittää, ovatko viestiketjuissa siirtyminen näivetystilasta pikaluistelukilpailuissa lukitukset tai taso vuorovaikutus profilointi aika, jonka avulla voit etsiä suorituskykyongelmia kun vuorovaikutteinen sovelluksen useimmin tietojen taso ja Työntekijä-roolin välillä tasojen välissä.  Voit tarkastella, joka luo sovelluksen tietokantakyselyt ja profiloinnin tietojen avulla voit parantaa tietokannan käytön. Lisätietoja taso vuorovaikutus profilointi blogimerkinnässä [vaiheittaisissa ohjeissa: taso vuorovaikutus Profilerin käyttäminen Visual Studio ryhmän järjestelmän 2010][3].



[1]: http://msdn.microsoft.com/library/azure/hh369930.aspx
[2]: http://msdn.microsoft.com/library/azure/hh411542.aspx
[3]: http://blogs.msdn.com/b/habibh/archive/2009/06/30/walkthrough-using-the-tier-interaction-profiler-in-visual-studio-team-system-2010.aspx
[4]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally09.png
[5]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally10.png
[6]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally02.png
[7]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally05.png
[8]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally010.png
[9]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally07.png
[10]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally06.png
[11]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally03.png
[12]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally011.png
[14]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally04.png 
[15]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally013.png
[16]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally012.png
[17]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally08.png
 