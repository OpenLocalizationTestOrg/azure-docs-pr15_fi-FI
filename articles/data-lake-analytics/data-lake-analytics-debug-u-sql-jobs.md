<properties 
   pageTitle="Virheenkorjaus U-SQL-työt | Microsoft Azure" 
   description="Opettele virheenkorjaus U SQL epäonnistui kärkipiste Visual Studiossa. " 
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
   ms.date="09/02/2016"
   ms.author="jgao"/>



#<a name="debug-c-code-in-u-sql-for-data-lake-analytics-jobs"></a>C#-koodin virheenkorjaus U SQL tietojen järvi Analytics töiden 

Opettele virheenkorjaus epäonnistui U-SQL-työt vuoksi virheet sisällä Käyttäjäkoodi Azure tietojen järvi Visual Studio työkalujen avulla. 

Visual Studio-työkalun avulla voit Lataa klusterin jäljittää ja korjaamisessa epäonnistui työt käännetty koodi ja kärkipiste tarvittavat tiedot.

Big datasta järjestelmien on yleensä laajennettavuus mallia kautta kieliä, kuten Java, C#, Python jne. Näiden järjestelmien on rajoitettu runtime virheiden tiedot, joka tekee vaikea mukautettua koodia runtime virheiden korjaaminen. Uusimman Visual Studio-työkalut sisältyy nimeltä "epäonnistui kärkipiste virheenkorjaus-ominaisuutta. Tämän ominaisuuden avulla voit ladata runtime tiedot azuren paikallisen työasemaan niin, että voit korjata virheet epäonnistui mukautettua C# koodia käyttämällä samaa runtime ja tarkka syöttötiedot pilvestä.  Kun ongelmista on kiinteä, voit käyttää muokattu koodin Azure-työkaluista.

Katso video esitykseen tätä ominaisuutta, [Virheenkorjaus Azure tietojen järvi Analytics mukautetun koodin](https://mix.office.com/watch/1bt17ibztohcb).

>[AZURE.NOTE] Visual Studio saattaa jumittua tai kaatua, jos sinulla ei ole kaksi ikkunaa seuraavat päivitykset: [Microsoft Visual C++ 2015 Redistributable päivitys 2](https://www.microsoft.com/download/details.aspx?id=51682), [Yleinen C Runtime for Windows](https://www.microsoft.com/download/details.aspx?id=50410&wa=wsignin1.0).


##<a name="prerequisites"></a>Edellytykset
-   On poistettu – [aloittaminen](data-lake-analytics-data-lake-tools-get-started.md) -artikkelissa.

## <a name="create-and-configure-debug-projects"></a>Luo ja määritä virheenkorjaus projektit

Avattaessa epäonnistunut työ tietojen järvi Visual Studio-työkalun saavat ilmoituksen. Yksityiskohtaiset virhetiedot näytetään virhesanoma-välilehti ja ikkunan ylälaidassa ilmoitusten keltaista palkkia. 

![Azure tietojen järvi Analytics U-SQL virheenkorjaus visual Studiossa Lataa kärkipiste](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-download-vertex.png)

**Voit ladata kärkipistettä ja virheenkorjaus-ratkaisun luominen**

1.  Avaa Visual Studiossa epäonnistunut U-SQL-työ.
2.  Valitse Lataa kaikki tarvittavat resurssit ja syötteen virtaa **Lataa** . Valitse **uudelleen** , jos lataaminen epäonnistui.
3.  Kun lataaminen on valmis, voit luoda paikallisen virheenkorjaus projektin, valitse **Avaa** . Uusi Visual Studio-ratkaisun nimeltä **VertexDebug** kutsutaan **LocalVertexHost** tyhjä projekti luodaan.

Jos käyttäjän määrittämiä operaattoreita käytetään U-SQL-koodia (Script.usql.cs), Luo projekti luokan kirjaston C# käyttäjän määrittämät operaattorit koodilla ja sisällyttää projektin VertexDebug-ratkaisuun.

Jos olet rekisteröinyt .dll kokoonpanot tietojen järvi Analytics-tietokantaan, kokoonpanon lähdekoodi on lisättävä VertexDebug-ratkaisuun.
 
Jos olet luonut erillisen C# luokan kirjaston U-SQL-koodin ja rekisteröity .dll kokoonpanot tietojen järvi Analytics-tietokantaa, sinun on lisättävä kokoonpanon lähteen C# projektin VertexDebug-ratkaisun.

Harvinaisten joskus käyttää käyttäjän määrittämiä operaattoreita U-SQL-koodia (Script.usql.cs)-tiedoston alkuperäinen ratkaisu. Jos haluat tehdä siihen, miten, joudut luomisen C# lähdekoodin sisältävän ja muuta klusterin rekisteröity johonkin kokoonpanon nimi. Saat rekisteröity klusterin valitsemalla komentosarjan, joka saatiin käynnissä klusterin kokoonpanon nimi. Voit tehdä avaamalla U-SQL-työ ja valitse "komentosarja-työ-ruudussa. 

**Voit määrittää ratkaisu**

1.  Ratkaisu Explorerista juuri luomasi C#-projektin hiiren kakkospainikkeella ja valitse sitten **Ominaisuudet**.
2.  Määritä tulostus-polku LocalVertexHost projektin toimi kansiopolun. Saat LocalVertexHost projektin kansio polku LocalVertexHost ominaisuuksien avulla.
3.  Koota projektin C#, jotta .pdb tiedosto liikkeelle LocalVertexHost projektin kansio tai voit kopioida .pdb-tiedosto kansioon manuaalisesti.
4.  **Poikkeusasetukset**Tarkista Yleiset kielen Runtime poikkeukset:

![Azure tietojen järvi Analytics U-SQL-virheenkorjaus visual studio-asetus](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-clr-exception-setting.png)
 
##<a name="debug-the-job"></a>Työn korjaaminen

Kun olet luonut virheenkorjaus-ratkaisun lataamalla kärkipistettä ja ympäristö on määritetty, voit aloittaa virheenkorjaus U-SQL-koodi.

1.  Ratkaisu Explorerista juuri luomasi **LocalVertexHost** projektin hiiren kakkospainikkeella, osoita **Virheenkorjaus**ja valitse sitten **Käynnistä uuden esiintymän**. Projektin aloitus LocalVertexHost on määritettävä. Voit nähdä seuraavan sanoman ensimmäistä kertaa, jossa voit jättää huomiotta. Voi kestää jopa minuutin virheenkorjaus-näyttö.
 
    ![Azure tietojen järvi Analytics U-SQL virheenkorjaus visual Studiossa varoitus](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-visual-studio-u-sql-debug-warning.png)

4.  Käytä Visual Studio perusteella muistin kokemus (Katso, muuttujat, jne.) vianmäärityksen. 
5.  Kun olet määrittänyt ongelman, korjaa koodin ja C#-projektin Muodosta uudelleen ennen testaaminen se uudelleen, kunnes kaikki ongelmat on ratkaistu. Kun virheenkorjaus on suoritettu onnistuneesti, näyttää seuraavan sanoman tulostusikkunassa 

        The Program ‘LocalVertexHost.exe’ has exited with code 0 (0x0).
 
##<a name="resubmit-the-job"></a>Lähetä uudelleen työ

Kun olet suorittanut virheenkorjaus U-SQL-koodi, voit tiedot epäonnistunut työ.

1. Rekisteröi uusi .dll kokoonpanojen ADLA-tietokantaan.

    1.  Palvelimen Explorer/Cloud Resurssienhallinnasta tietojen järvi Visual Studio-työkalun Laajenna **Databases** -solmu 
    2.  Napsauta kokoonpanot Rekisteröi laitekokonaisuuksiin. 
    3.  Voit rekisteröidä oman uusi .dll kokoonpanojen ADLA-tietokantaan.
 
2.  Tai kopioi C#-koodin script.usql.cs--C#-koodin tiedoston takana.
3.  Lähetä uudelleen työtäsi.

##<a name="next-steps"></a>Seuraavat vaiheet

- [Opetusohjelma: Azure tietojen järvi Analytics U-SQL-kielen käytön aloittaminen](data-lake-analytics-u-sql-get-started.md)
- [Opetusohjelma: U-SQL-komentosarjojen käyttämällä järvi Datatyökalut Visual Studio kehittäminen](data-lake-analytics-data-lake-tools-get-started.md)
- [Kehittää Azure tietojen järvi Analytics työt U-SQL-käyttäjän määrittämät operaattorit](data-lake-analytics-u-sql-develop-user-defined-operators.md)

