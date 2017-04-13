<properties 
   pageTitle="Käytä järvi Datatyökalut kärkipiste suorittamisen näkymää Visual Studio | Microsoft Azure" 
   description="Opettele käyttämään kärkipiste suorittamisen näkymän koe tietojen järvi Analytics työt." 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="mumian" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="10/13/2016"
   ms.author="jgao"/>

# <a name="use-the-vertex-execution-view-in-data-lake-tools-for-visual-studio"></a>Käytä järvi Datatyökalut kärkipiste suorittamisen näkymää Visual Studio

Opettele käyttämään kärkipiste suorittamisen näkymän koe tietojen järvi Analytics työt.

## <a name="prerequisites"></a>Edellytykset

- Jotkin basic knowledge järvi Datatyökalut Visual Studio avulla voit kehittää U-SQL-komentosarjoja.  Katso [Opetusohjelma: kehittää U-SQL-komentosarjoja järvi Datatyökalut Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).

## <a name="open-the-vertex-execution-view"></a>Avaa kärkipiste suorittamisen näkymä

Tiettyyn työhön voit valita vasemmassa alakulmassa "Kärkipiste suorittamisen Näytä"-linkki. Sinulta saatetaan pyytää lataamaan profiilit ja se voi viedä jonkin aikaa verkkoyhteyden mukaan.

![Tietoja järvi Analytics Työkalut kärkipiste suorittamisen Näytä](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-open-vertex-execution-view.png)

## <a name="understand-vertex-execution-view"></a>Kärkipisteen suorittamisen näkymän ymmärtäminen

Kun olet lisännyt kärkipiste suorittamisen näkymä on kolme osaa:

![Tietoja järvi Analytics Työkalut kärkipiste suorittamisen Näytä](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view.png)

- Kärkipisteen valitsin: vasemmalla on kärkipiste valitseminen.  Voit valita kärkipisteet ominaisuuksista (kuten top 10-tietojen lukeminen, tai valitse vaiheen mukaan).

    Kriittisen polun kärkipisteet on yksi käytetyt yleisimmät suodattimet. Kriittinen polku on pisimmän U-SQL-työn. Se on hyödyllinen optimointi työt tarkistamalla, mitä kärkipiste kestää Pisin aika.

- Yläreunan keskiosassa-ruudussa seuraavasti:

    ![Tietoja järvi Analytics Työkalut kärkipiste suorittamisen Näytä](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view-pane2.png)

    Tässä näkymässä näkyy myös kaikki kärkipisteet käynnissä tila. Se muuntaa aika vastaavasti paikallisesta tietokoneesta ja näyttää eri värejä eri tila.

- Ala-keskimmäisessä ruudussa:

    ![Tietoja järvi Analytics Työkalut kärkipiste suorittamisen Näytä](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view-pane3.png)

    - Prosessin nimi: Kärkipiste esiintymän nimi. Se koostuu eri osien vaiheen nimi | VertexName | VertexRunInstance. Esimerkiksi SV7_Split [62] .v1 kärkipiste lyhenne (.v1, indeksi lähtien 0) toisen suoritettavan vaiheen SV7_Split numeron kärkipiste 62.
    - Yhteensä tietojen luku/kirjoitus: Tiedot on tämän kärkipiste luku tai kirjoittaa.
    - Tilan ja lopetuksen tila: Lopullinen tila Kun kärkipiste on päättynyt.
    - Lopeta koodin/virhe tyyppi: Virheen kärkipiste epäonnistui.
    - Luomisen syy: Miksi kärkipiste on luotu.
    - Resurssin viive/prosessin viive/on jonon viive: käytettävä aika kärkipiste odottamaan resursseja, voit käsitellä tietoja ja yhteydenpitoon jonossa.
    - Prosessin tai luoja GUID: GUID nykyisen käynnissä kärkipiste tai luoja.
    - Versio: N: nnen esiintymä käynnissä kärkipiste (järjestelmä ajoittaa kärkipisteen uusia esiintymiä varten monesta syystä, kuten automaattisesti Laske redundancy jne.)
    - Luotu aika.
    - Luo Käynnistä aika/prosessin jonossa aika/prosessin Käynnistä aika/prosessin valmiina käsitellä aika: kärkipiste prosessin käynnistyessä luominen; kärkipisteen prosessin käynnistyessä jonon; tiettyjen kärkipiste prosessin käynnistyessä; Kun tiettyjä kärkipiste on valmis.

## <a name="next-steps"></a>Seuraavat vaiheet

- Saat yleiskatsauksen tietojen järvi Analytics-artikkelissa [Azure tietojen järvi Analytics yleiskatsaus](data-lake-analytics-overview.md).
- Aloita U-SQL-sovellusten kehittäminen-kohdasta [kehittää U-SQL-komentosarjat käyttäminen tietojen järvi Tools for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
- U-SQL-kohdassa [Azure tietojen järvi Analytics U-SQL-kielen käytön aloittaminen](data-lake-analytics-u-sql-get-started.md).
- Katso hallintatehtäviä, [hallita Azure tietojen järvi Analytics käyttämällä Azure portal](data-lake-analytics-manage-use-portal.md).
- Diagnostiikan lokitiedot on artikkelissa [Azure tietojen järvi Analytics käytetään diagnostiikka lokit](data-lake-analytics-diagnostic-logs.md)
- Monimutkaisen kyselyn on ohjeartikkelissa [Analysoi sivuston lokit Azure tietojen järvi Analytics avulla](data-lake-analytics-analyze-weblogs.md).
- Voit tarkastella projektin tiedot-kohdassa [käyttäminen työn selaimessa ja Azure tietojen järvi Analytics työt työn tarkasteleminen](data-lake-analytics-data-lake-tools-view-jobs.md)