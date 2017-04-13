<properties
    pageTitle="Analytics ympäristöissä: Apache myrsky vertailu Stream Analytics | Microsoft Azure"
    description="Hanki ohjeita valitsemalla cloud analytics-ympäristö käyttämällä Stream Analytics Apache myrsky vertailu. Tietoja ominaisuudet ja erot."
    keywords="Analytics-ympäristö, analytics ympäristöjen, cloud analytics-ympäristö, myrsky vertailu"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>

# <a name="help-choosing-a-streaming-analytics-platform-apache-storm-comparison-to-azure-stream-analytics"></a>Ohjeen valitseminen streaming analytics-ympäristössä: Azure Stream Analytics Apache myrsky vertailu

Hanki ohjeita valitsemalla cloud analytics-ympäristö käyttämällä Azure Stream Analytics Apache myrsky tämän vertailu. Tietoja Stream Analytics Apache myrsky Azure Hdinsightiin, jotta osaat valita oikean ratkaisun yrityksesi hallitun palveluna vai arvo-propositioita pysyisi käyttötapauksiin.

Sekä analytics ympäristöjen on hyötyä PaaS ratkaisun, on muutamia pää myöntäneen ominaisuuksista, joita erottaa toisistaan. Ominaisuuksia sekä näiden palveluiden rajoituksista on lueteltu alla avulla voit Power View ratkaisussa, sinun on kohdassa tavoitteet.

## <a name="storm-comparison-to-stream-analytics-general-features"></a>Myrsky Stream Analytics vertailu: Yleiset toiminnot ##
<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Azure Stream Analytics</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>Valitse HDInsight Apache myrsky</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Avaa lähde</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Ei, Azure Stream Analytics on Microsoftin yleinen ojentamassa.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Kyllä, Apache myrsky on Apache käyttöoikeus-tekniikka.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Tuettu Microsoftin</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Kyllä </p>
            </td>
            <td width="246" valign="top">
                <p>
Kyllä </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Laitteistovaatimukset</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Ei ole laitteistovaatimukset. Azure Stream Analytics on Azure-palvelu.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Ei ole laitteistovaatimukset. Apache myrsky on Azure-palvelu.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Ylimmän tason yksikkö</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Azure Stream Analytics-asiakkaiden ottaminen käyttöön ja valvoa streaming töitä.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Apache myrsky HDInsight-asiakkaiden kanssa käyttöönotto ja seurata koko klusterin, joka voi olla useita myrsky työt sekä muita työmääriä (mukaan lukien erä).
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Hinta</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Tietojen käsitteleminen on hinta on Stream Analytics ja streaming yksiköiden (työ on käynnissä tunnissa) pakollinen.
                </p>
                <p>
                    <a href="http://azure.microsoft.com/en-us/pricing/details/stream-analytics/">Muita hintatiedot tässä luettelossa.</a>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Apache myrsky HDInsight-Osta muuttaminen perustuu klusterin ja veloitetaan klusterin on käynnissä, riippumaton työt käyttöön ajan perusteella.
                </p>
                <p>
                    <a href="http://azure.microsoft.com/en-us/pricing/details/hdinsight/">Muita hintatiedot tässä luettelossa.</a>
                </p>
            </td>
        </tr>
    </tbody>
</table>
##Sisällön tuottaminen kullekin analytics-ympäristössä##
<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Azure Stream Analytics</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>Valitse HDInsight Apache myrsky</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Ominaisuudet: SQL DSL</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Helppokäyttöinen SQL-kielituki on käytettävissä.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Käyttäjien on ei, koodin kirjoittaminen-Java C# tai käytä Trident API.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Ominaisuudet: Ajallinen operaattorit</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Erillisessä ikkunassa koosteet ja ajallinen liitokset tuetaan ulos ruutuun.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Ajallinen operaattoreita on pantava täytäntöön käyttäjä.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Kehittäminen kokemus</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Vuorovaikutteinen yhtä aikaa muiden kanssa ja virheenkorjaus kokemuksen mallitiedot palvelun Azure-portaalissa.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Kehittäminen ja virheenkorjaus seurantaan ja valvontaan kokemus on kautta Visual Studio .NET-käyttäjille, kun for Java ja muiden kielten kehittäjät saa käyttää haluamaansa IDE.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Virheenkorjaus tuki</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Virta Analytics tarjoaa basic tilan ja toimintojen lokitiedot tapana virheenkorjaus, mutta tällä hetkellä tarjoa joustavuutta mitä/kuinka paljon sisältyy lokit ie: yksityiskohtaisessa tilassa.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Yksityiskohtainen lokit ovat käytettävissä vianmääritystä varten. Kahdella eri tavalla, pinta lokit käyttäjälle, visual Studio tai käyttäjä voi RDP käyttämään lokit klusterin kyselyjä.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>UDF-tuki (käyttäjän määrittämät funktiot)</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Tällä hetkellä ei ole UDF tukea.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
UDF voidaan kirjoittaa C#, Java tai valittua kieli.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Laajennettava - mukautettua koodia</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Extensible Stream Analytics-koodi ei tueta.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Kyllä, ei käytettävyys kirjoittaa mukautettua koodia myrsky C#, Java tai muita tuetuilla kielillä.
                </p>
            </td>
        </tr>
    </tbody>
</table>
##Tietolähteiden ja tulostaa##
<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Azure Stream Analytics</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>Valitse HDInsight Apache myrsky</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                 <strong>Syötteen tietolähteet</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>Tuetut lähteitä ovat Azure tapahtuman keskittimet ja Azure-BLOB-objektit.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Yhdistimet ovat käytettävissä tapahtuman keskittimet, palvelun Bus, Kafka jne. Ei tueta yhdistimet voidaan toteuttaa mukautettua koodia kautta.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Kirjoita tietomuotojen</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Tuetut syötteen muodot ovat Avro, JSON, CSV.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Mitä tahansa muotoa voidaan toteuttaa mukautettua koodia kautta.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Tulostus</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Streaming työn voi olla useita tulostaa. Tuetut tulostaa: Azure tapahtuman keskittimet, Azure-Blob-säiliö Azure taulukot, Azure SQL-DB tai PowerBI.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Tuen monta tulostus-on verkkotopologia, kunkin tulosteen saattaa olla mukautetun logiikan edeltävät käsittelyä varten. Loitontaminen ruutuun myrsky sisältää yhdistimet PowerBI, Azure tapahtuman keskittimet, Azure-Blob-säilö, Azure DocumentDB, SQL ja HBase. Ei tueta yhdistimet voidaan toteuttaa mukautettua koodia kautta.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Tietomuotojen koodaus</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Virta Analytics edellyttää UTF-8 tietomuoto voi käyttää.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Tietoja koodauksen muoto voidaan toteuttaa mukautettua koodia kautta.
                </p>
            </td>
        </tr>
    </tbody>
</table>
##Hallinta ja toiminnot##
<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Azure Stream Analytics</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>Valitse HDInsight Apache myrsky</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Projektin mallista</strong>
                </p>
                <p>
                    - <strong>Azure Portal</strong>
                </p>
                <p>
                    - <strong>Visual Studio</strong>
                </p>
                <p>
                    - <strong>PowerShellin</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Käyttöönoton on toteutettu Azure-portaalissa, PowerShell ja REST API kautta.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Depolyment on toteutettu Azure-portaalissa, PowerShell, Visual Studio ja REST API kautta.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Seuranta (Tilasto, laskureita jne.)</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Seuranta on toteutettu Azure-portaalin ja REST API kautta.
                </p>
                <p>
Käyttäjä voi määrittää myös Azure ilmoitukset.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Seuranta on toteutettu myrsky Käyttöliittymä ja REST API kautta.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Skaalattavuus</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Kunkin projektin Streaming yksiköt numero. Kunkin Streaming yksikön käsittelee enintään 1 Megatavu/s-näppäinyhdistelmää. Enintään 50 yksiköiden oletusarvoisesti. Kutsu niin, että rajoitus.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
HDI myrsky klusterin näkyvien solmujen määrän. Ei rajaa enimmäismäärään solmujen (Azure kiintiön määrittämiä yläreunan rajoitus). Kutsu niin, että rajoitus.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Tietojen käsittely rajoitukset</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Käyttäjien skaalata ylös tai alas Streaming yksiköiden Suurenna tietojen käsittely tai optimoida kustannukset.
                </p>
                <p>
Skaalaa enintään 1 gt/s </p>
            </td>
            <td width="246" valign="top">
                <p>
Käyttäjä voi skaalata ylös tai alas klusteri tarpeiden mukaan.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Lopeta/ansioluettelo</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Lopeta ja jatka pysäyttää edellisen paikasta.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Lopeta ja jatkaa viimeksi, sijoita pysäytetty vesileiman perusteella.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Palvelun ja framework päivitys</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Automaattista korjausta ei ole käyttökatkot kanssa.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Automaattista korjausta ei ole käyttökatkot kanssa.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Liiketoiminnan jatkuvuus taattua SLA erittäin käytettävissä palvelu kautta</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
99,9 % käytettävyyttä SLA </p>
                <p>
Automaattinen palautus-virheet </p>
                <p>
Tilallisten ajallinen toimijoiden palautus on valmis.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
99,9 % käytettävyyttä myrsky klusterin SLA. Apache myrsky on vika varmatoimisempia streaming ympäristö, mutta se ei niiden asiakkaiden varmistamisesta streaming työnsä suoritettavaa.
                </p>
            </td>
        </tr>
    </tbody>
</table>
##Lisäominaisuudet##
<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Azure Stream Analytics</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>Valitse HDInsight Apache myrsky</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Myöhässä olevat saapuvien ja väärässä järjestyksessä tapahtumankäsittely</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Valmiin määritettäviä käytäntöjä, voit järjestää uudelleen pudota tai muuttaa tapahtuma-aika.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Käyttäjän on otettava käyttöön logiikan käsittelemään Tämä skenaario.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Viitetiedot</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Viitetiedot käytettävissä Azure-BLOB-enimmäiskoon ladatun haku välimuistin 100 megatavua. Viite-tietojen päivittäminen hallitsee palvelu.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Rajoja tiedot koon. Yhdistimien HBase ja DocumentDB, SQL Server Azure käytettävissä. Ei tueta yhdistimet voidaan toteuttaa mukautettua koodia kautta.
                </p>
                <p>
Viite-tietojen päivittäminen täytyy käsitellä mukautettua koodia.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Koneen Learning integrointi</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Määrittämällä julkaista Azure koneen Learning mallien toimintojen aikana ASA työn luominen <a href="http://blogs.msdn.com/b/streamanalytics/archive/2015/05/24/real-time-scoring-of-streaming-data-using-machine-learning-models.aspx">(yksityinen esikatselu)</a>.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Käytettävissä myrsky Pultit kautta.
                </p>
            </td>
        </tr>
    </tbody>
</table>
