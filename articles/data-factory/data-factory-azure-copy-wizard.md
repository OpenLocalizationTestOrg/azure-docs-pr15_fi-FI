<properties
    pageTitle="Ohjattu tietojen Factory Azure kopioi | Microsoft Azure"
    description="Lue lisää tietojen kopioiminen tietolähteiden poistumia ohjatun tietojen Factory Azure kopioi avulla."
    services="data-factory"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="data-factory"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="spelluru"/>

# <a name="azure-data-factory---copy-wizard"></a>Azure tietojen Factory - Ohjattu kopiointi
Ohjattu Azure tietojen Factory-kopio on helpottaa ingesting tietojen, joka on yleensä ensimmäisessä vaiheessa tiedot lopusta loppuun-integrointiskenaario. Siirryttäessä ohjatussa Azure tietojen Factory kopioi sinun ei tarvitse ymmärtää minkä tahansa JSON määritykset linkitetyn services, tietojoukkoja ja putkistot. Kun olet suorittanut ohjatun toiminnon ohjeita, ohjattu toiminto luo automaattisesti putkijohto tietojen kopioiminen valitun tietolähteen valittuun kohteeseen. Lisäksi kopio-ohjatun toiminnon avulla voit tarkistaa parhaillaan nautittuina yhtä aikaa, milloin tiedot, joka tallentaa paljon aikaa erityisesti kun sinulla on ingesting tietojen ensimmäistä kertaa tietolähteestä. Käynnistä ohjattu kopio, valitsemalla tiedot factory aloitussivulla **Kopioi tiedot** -ruutu.

![Ohjattu kopiointi](./media/data-factory-copy-wizard/copy-data-wizard.png)


## <a name="an-intuitive-wizard-for-copying-data"></a>Intuitiivisen ohjatun tietojen kopioiminen
Tämän ohjatun toiminnon avulla voit helposti siirtää tietoja useista eri lähteistä kohteisiin minuutteina. Ohjatun toiminnon loppuun, kopioi aktiviteettiin putkijohto luodaan automaattisesti puolestasi sekä riippuvaiset Data Factory-objektit (linkitetyn services ja tietojoukkoja). Putkijohto luomiseen tarvitaan et lisätoimia.   

![Valitse tietolähde](./media/data-factory-copy-wizard/select-data-source-page.png)

> [AZURE.NOTE] Katso [Ohjattu kopiointi opetusohjelma](data-factory-copy-data-wizard-tutorial.md) artikkelista vaiheittaiset ohjeet luominen otoksen putkijohto kopioi tiedot Azure blob Azure SQL-tietokannan taulukkoon. 

Ohjattu toiminto on suunniteltu big datasta mielessä alusta alkaen. Se on yksinkertainen ja tehokas tekijän tiedot Factory putkistot, jotka siirtävät satoja kansioita, tiedostoja tai käyttämällä ohjattua Kopioi tietojen taulukot. Ohjattu toiminto tukee kolmea seuraavat ominaisuudet: automaattisen tietojen esikatselu, rakenteen sieppaus ja yhdistäminen ja tietojen suodattamisesta. 

## <a name="automatic-data-preview"></a>Automaattisen tietojen esikatselu 
Kopioi ohjatun toiminnon avulla voit tarkastella osa valitun tietolähteen voit tarkistaa, onko tiedot on oikeat tiedot, jotka haluat kopioida tiedot. Jos lähdetiedot tekstitiedostona, ohjattu kopiointi jäsentää lisäksi saat automaattisesti rivin ja sarakkeen erottimet ja rakenteen tekstitiedosto. 

![Tiedoston muotoiluasetukset](./media/data-factory-copy-wizard/file-format-settings.png)

## <a name="schema-capture-and-mapping"></a>Rakenteen sieppaus ja yhdistäminen 
Syöttötiedot rakennetta ei ehkä vastaa siirtää tietoja joissakin tapauksissa rakenne. Tässä skenaariossa haluat Yhdistä sarakkeet lähde rakenteesta sarakkeisiin kohde rakenteesta. 

Kopioi ohjattu toiminto yhdistää automaattisesti tietolähteen rakenne sarakkeiden sarakkeisiin määritelty kohde. Voit ohittaa yhdistämismääritykset käyttämällä avattavien luetteloiden (tai) Määritä, onko sarake on ohitettu kopioitaessa tiedot.   

![Mallin yhdistäminen](./media/data-factory-copy-wizard/schema-mapping.png)

## <a name="filtering-data"></a>Tietojen suodattaminen  
Ohjatun toiminnon avulla voit suodattaa lähdetiedot, valitse vain kopioidaan kohde/käsittelytoiminto tietovaraston tarvitsemat tiedot. Suodattaminen vähentää tiedot kopioidaan käsittelytoiminto tietovaraston minäkin ja parantaa näin ollen kopiointi siirtonopeuden. Se on joustava tapa suhteellisen tietokannan tietojen suodattaminen käyttämällä SQL kyselyn language (tai) tiedostot Azure-blob-kansion käyttämällä [Data Factory-funktioita ja muuttujia](data-factory-functions-variables.md).   

### <a name="filtering-of-data-in-a-database"></a>Tietokannan tietojen suodattaminen  
Esimerkissä SQL-kysely käyttää `Text.Format` funktio ja `WindowStart` muuttuja. 

![Vahvista lausekkeet](./media/data-factory-copy-wizard/validate-expressions.png)

### <a name="filtering-of-data-in-an-azure-blob-folder"></a>Azure-blob-kansion tietojen suodattaminen
Voit käyttää muuttujaa kansiopolku tietojen kopioiminen kansioon, joka määritetään suorituksen [järjestelmämuuttujien](data-factory-functions-variables.md#data-factory-system-variables)perusteella. Tuetut muuttujat: **{vuoden}**, **{kuukauden}**, **{päivän}**, **{Tunti}**, **{minuutti}**ja **{mukautetun}**. Esimerkki: inputfolder / {vuoden} / {kuukauden} / {päivän}.

Oletetaan, että olet kirjoittanut kansiot seuraavanlainen:

    2016/03/01/01
    2016/03/01/02
    2016/03/01/03
    ...

Valitse **Selaa** **tiedoston**tai kansion, valitse jokin näistä kansioista (esimerkiksi 2016 -> 03 -> 01 -> 02), ja sitten **Valitse**. Raportissa pitäisi näkyä `2016/03/01/02` tekstiruutuun. **{Vuoden}**, **03** , jossa **{kuukausi}**, **{day}**- **01** ja **02** **{Tunti}**kanssa nyt **2016** korvaaminen ja paina SARKAINTA. Avattavien luetteloiden Valitse neljä muuttujia tyylissä pitäisi näkyä seuraavat:

![Järjestelmämuuttujien käyttäminen](./media/data-factory-copy-wizard/blob-standard-variables-in-folder-path.png)   

Seuraavassa näyttökuvassa esitetyllä voit käyttää myös **mukautettuja** muuttujan ja [tuettu muotoilumerkkijonoa](https://msdn.microsoft.com/library/8kb3ddd4.aspx). Voit valita rakenteessa kansio käyttämällä **Selaa** -painiketta. Valitse korvaa arvon **{mukautetun}**ja paina SARKAINTA Nähdäksesi tekstiruutu, jossa voi kirjoittaa muodon merkkijono.     

![Mukautetun muuttujan käyttäminen](./media/data-factory-copy-wizard/blob-custom-variables-in-folder-path.png)


## <a name="support-for-diverse-data-and-object-types"></a>Monipuolisen tiedot ja objektityypit tuki
Käyttämällä ohjattua Kopioi voit siirtää tehokkaasti satoja kansioita, tiedostoja tai taulukoita.

![Valitse taulukot, josta haluat kopioida tiedot](./media/data-factory-copy-wizard/select-tables-to-copy-data.png)

## <a name="scheduling-options"></a>Ajoitusasetukset
Voit suorittaa kopiointi kerran tai aikataulun (kerran tunnissa päivä-, ja niin edelleen). Molempien asetusten voi käyttää yhdistimet kaikki paikallisen, cloud ja työpöydän paikallinen kopio.

Erikseen kopiointi mahdollistaa tietojen siirto lähteestä kohteeseen vain kerran. Toiminto on käytettävissä minkä kokoinen tahansa ja kaikki tuetut muodon tiedot. Ajoitettu Kopioi voit kopioida tietoja määrätyn toistuminen. Voit käyttää monipuolisia asetukset (kuten uudelleen, aikakatkaisu ja ilmoitusten) määrittämiseen ajoitetun kopio.

![Ajoituksen ominaisuudet](./media/data-factory-copy-wizard/scheduling-properties.png)


## <a name="next-steps"></a>Seuraavat vaiheet
Katso lyhyt vaiheista tietojen Factory kopioi luontitoiminnon avulla putkijohto kopioi aktiviteettiin, [Opetusohjelma: luominen käyttämällä ohjattua Kopioi putkijohto](data-factory-copy-data-wizard-tutorial.md).
