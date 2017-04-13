<properties
    pageTitle="Seurata ja hallita joustavasti tietokanta-ryhmän Azure-portaalissa | Microsoft Azure"
    description="Katso, miten Azure portaalin ja SQL-tietokantaan valmiin liiketoimintatietojen avulla hallita, valvoa ja oikealle-kokoiselle skaalattava joustavasti tietokannan resurssivarantoon tietokannan suorituskyvyn parantaminen ja hallita kustannukset."
    keywords=""
    services="sql-database"
    documentationCenter=""
    authors="ninarn"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="09/22/2016"
    ms.author="ninarn"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="monitor-and-manage-an-elastic-database-pool-with-the-azure-portal"></a>Seurata ja hallita joustavasti tietokanta-ryhmän Azure-portaalissa

> [AZURE.SELECTOR]
- [Azure portal](sql-database-elastic-pool-manage-portal.md)
- [PowerShellin](sql-database-elastic-pool-manage-powershell.md)
- [C#](sql-database-elastic-pool-manage-csharp.md)
- [T-SQL](sql-database-elastic-pool-manage-tsql.md)


Azure portaalin avulla voit seurata ja hallita joustavasti tietokannan resurssivarantoon ja varannon tietokannoissa. -Portaalista voit valvoa joustavasti resurssivarantoon ja kyseisen ryhmän sisällä tietokannat. Voit tehdä muutoksia joukko joustavasti resurssivarantoon ja Lähetä kaikki muutokset samalla kertaa. Nämä muutokset sisältävät lisäämällä tai poistamalla tietokantoja, joustavasti resurssivarantoon-asetusten muuttaminen tai tietokannan asetusten muuttaminen.

Alla olevassa kuvassa näkyy joustavasti Esimerkki-ryhmän. Näkymä sisältää:

*  Kaavioiden resurssien käyttö joustavasti resurssivarantoon ja ryhmän sisältämät tietokantojen seurantaa varten.
*  Voit tehdä muutoksia joustavasti resurssivarantoon **Määritä** resurssivarantoon-painiketta.
*  **Luo tietokanta** -painiketta, joka luo uuden tietokannan ja lisää sen nykyisen joustavasti ryhmän.
*  Joustavasti työt, joiden avulla voit hallita paljon tietokantojen suorittamalla Transact-SQL-komentosarjat luettelon kaikki tietokannoissa.

![Resurssivarannon näkymä][2]

Toimii vain tämän artikkelin vaiheet, sinun on SQL server Azure-tietokannassa, vähintään yksi tietokanta ja joustavasti resurssivarantoon. Jos sinulla ei ole joustavasti resurssivarantoon, katso [luoda varannon](sql-database-elastic-pool-create-portal.md); Jos sinulla ei ole tietokannan, katso [SQL-tietokanta-opetusohjelma](sql-database-get-started.md).

## <a name="elastic-pool-monitoring"></a>Joustavasti varannon seuranta

Voit avata tietyn resurssivarantoon nähdäksesi sen resurssien käyttö. Oletusarvon mukaan altaan on määritetty näyttämään edellisen tunnin säilytys- ja eDTU käyttö. Kaavion voi määrittää näyttämään eri arvot eri Windowsin kautta.

1. Valitse varanto käyttöä varten.
2. Valitse **Joustavasti resurssivarantoon seuranta** on kaavion, jossa **Resurssien käyttö**. Napsauta kaaviota.

    ![Joustavasti varannon seuranta][3]

    Jossa määritetyn arvot yksityiskohtainen näkymä näkyy määritetyn ajanjakson **metrijärjestelmä** -sivu avautuu.   

    ![Metrijärjestelmän sivu][9]

### <a name="to-customize-the-chart-display"></a>Voit mukauttaa kaavion-näyttö

Voit muokata kaaviota ja muita tietoja, kuten suorittimen prosentteina, tietojen IO prosentteina ja log IO prosenttiarvo näyttämään metrisillä sivu.

2. Valitse metrisillä sivu valitsemalla **Muokkaa**.

    ![Valitse Muokkaa][6]

- **Muokkaa kaavion** sivu, valitse uusi aika-alue (tunti, nykyään aiempia tai viime viikolla) tai valitsemalla **Mukautettu** voit valita minkä tahansa päivämääräalue viimeisen kahden viikon aikana. Valitse kaaviolaji (palkki tai rivillä) ja valitse sitten resurssit seurannassa.

> Huomautus: Vain saman mittayksikkö arvot voidaan näyttää kaaviossa samanaikaisesti. Esimerkiksi jos valitset "eDTU prosentteina" sitten vain osaat valita muita tietoja prosenteilla mittayksikön.

    ![Click edit](./media/sql-database-elastic-pool-manage-portal/edit-chart.png)

- Valitse **OK**.


## <a name="elastic-database-monitoring"></a>Joustavasti tietokannan seuranta

Yksittäiset tietokannat seurattavista myös mahdollisia ongelmia.

1. Valitse **Joustavasti tietokannan seuranta**on kaavio, jossa viisi tietokantojen mittaukset. Oletusarvon mukaan kaavio näkyy Ylin 5 tietokantojen altaan mukaan keskimääräinen eDTU käyttö kuluneen tunnin. Napsauta kaaviota.

    ![Joustavasti varannon seuranta][4]

2. **Tietokannan resurssien käyttö** -sivu tulee näkyviin. Tämä on yksityiskohtainen näkymä-ryhmän tietokannan käytön. Käytä ruudukon alaosassa sivu, voit valita kaikki tietokannat sen käyttötavan näyttäminen kaavion (enintään 5 tietokannat) sarjassa. Voit mukauttaa kaaviossa valitsemalla **muokata kaavion**arvot ja aika-ikkuna.

    ![Tietokannan resurssien käyttö-sivu][8]

### <a name="to-customize-the-view"></a>Näkymän mukauttaminen

1. Valitse **tietokannan resurssien käyttö** -sivu **muokata kaaviota**.

    ![Valitse Muokkaa-kaavio](./media/sql-database-elastic-pool-manage-portal/db-utilization-blade.png)

2. Valitse **Muokkaa** -kaavion sivu uusi aika-alue (aiempia tunnin tai 24 tunnin aiempia) tai valitsemalla **Mukautettu** toista päivää Valitse näytettävien edellisten kahden viikon.

    ![Valitse mukautettu](./media/sql-database-elastic-pool-manage-portal/editchart-date-time.png)

3. Valitse **vertailu-tietokannoista** avattavasta valikosta voit valita eri metrijärjestelmä käyttämään vertailtaessa tietokannat.

    ![Kaavion muokkaaminen](./media/sql-database-elastic-pool-manage-portal/edit-comparison-metric.png)

### <a name="to-select-databases-to-monitor"></a>Valitse tietokantojen seurannassa

Tietokannan luettelossa **Tietokannan resurssien käyttö** -sivu voit etsiä tietyn tietokantojen katsomalla sivut-luettelon kautta tai kirjoittamalla Kirjoita tietokannan nimi. Valitse tietokanta käyttämällä valintaruudun valinta.

![Tietokantojen seurannassa etsiminen][7]


## <a name="add-an-alert-to-a-pool-resource"></a>Ilmoituksen lisääminen resurssivarantoon resurssi

Voit lisätä resursseja, Lähetä sähköposti edelleen henkilöille tai URL-Osoitteen päätepisteet ilmoitusten merkkijonojen, kun resurssin käynnit käyttö raja-arvon, joka määritetään säännöt.

> [AZURE.IMPORTANT]Resurssien käytön valvonta joustavasti jakavat on vähintään 20 minuutin viive. Määrittää ilmoitusasetukset on alle 30 minuuttia joustavasti jakavat ei tällä hetkellä tueta. Minkä tahansa ilmoitusten määrittäminen joustavasti jakavat pisteeseen (parametrin nimeltä "-WindowSize"-PowerShell API) on alle 30 minuuttia saattaa ei voi käynnistää. Varmista, että kaikki ilmoitukset joustavasti jakavat määrittäminen käyttää ajan (puskuri) vähintään 30 minuuttia.

**Voit lisätä minkä tahansa resurssin ilmoituksen seuraavasti:**

1. Valitse Avaa **metrijärjestelmä** -sivu valitsemalla **Lisää ilmoitus** **Resurssien käyttö** -kaavio ja täytä tiedot **ilmoitusten säännön lisääminen** -sivu (**resurssi** on automaattisesti määritetty toimialueille varanto on).
2. Kirjoita **nimi** ja **kuvaus** , joka ilmaisee, että vastaanottajat ilmoitusta.
3. Valitse **arvo** , jonka haluat ilmoituksen-luettelosta.

    Kaaviossa näkyy dynaamisesti resurssien käytön, metrijärjestelmän avulla voit valita raja-arvon.

4. Valitse **ehto** (suurempi kuin, pienempi kuin, jne) ja **raja-arvon**.
5. Valitse **OK**.



## <a name="move-a-database-into-an-elastic-pool"></a>Tietokannan siirtäminen joustavasti resurssivarantoon

Voit lisätä tai poistaa tietokantojen aiemmin varannon. Tietokantojen voi olla muiden jakavat. Voit kuitenkin vain lisätä tietokannoissa, jotka ovat samassa looginen palvelimessa.

1. Valitse ryhmän, sivu **joustavasti tietokantojen** **määrittäminen resurssivarantoon**.

    ![Valitse Määritä resurssivarantoon][1]

2. Valitse **Lisää ryhmään** **määrittäminen resurssivarantoon** -sivu.

    ![Valitse Lisää ryhmään](./media/sql-database-elastic-pool-manage-portal/add-to-pool.png)


3. Valitse **Lisää tietokantoja** , sivu tietokannan tai tietokantoihin, voit lisätä ryhmään. Valitse **Valitse**.

    ![Valitse Lisää tietokannat](./media/sql-database-elastic-pool-manage-portal/add-databases-pool.png)

    **Määritä sovellussarjan** -sivu näyttää nyt valittu lisätään, joiden sen tila on **odottaa**tietokanta.

    ![Odottaa resurssivarantoon lisäykset](./media/sql-database-elastic-pool-manage-portal/pending-additions.png)

3. **Määritä ryhmän sivu**valitsemalla **Tallenna**.

    ![Valitse Tallenna](./media/sql-database-elastic-pool-manage-portal/click-save.png)

## <a name="move-a-database-out-of-an-elastic-pool"></a>Tietokannan siirtäminen pois joustavasti resurssivarantoon

1. Valitse **Määritä resurssivarantoon** , sivu tietokanta tai poista tietokannat.

    ![tietokantojen luettelo](./media/sql-database-elastic-pool-manage-portal/select-pools-removal.png)

2. Valitse **Poista varannon**.

    ![tietokantojen luettelo](./media/sql-database-elastic-pool-manage-portal/click-remove.png)

    **Määritä ryhmän** -sivu näyttää nyt sen tila on **odottaa**poistaa valitsemasi tietokanta.

    ![Esikatselu tietokannan lisääminen ja poistaminen](./media/sql-database-elastic-pool-manage-portal/pending-removal.png)

3. **Määritä ryhmän sivu**valitsemalla **Tallenna**.

    ![Valitse Tallenna](./media/sql-database-elastic-pool-manage-portal/click-save.png)

## <a name="change-performance-settings-of-a-pool"></a>Resurssivarantoon suorituskyvyn asetusten muuttaminen

Kun seurata resurssivarantoon resurssien-käyttöön, saatat huomata muutoksia tarvitaan. Altaan on ehkä suorituskykyä tai tallennustilan rajoituksia muutoksen. Haluat mahdollisesti altaan tietokanta-asetusten muuttaminen. Voit muuttaa ryhmän asetuksia milloin tahansa, saat parhaan suorituskyvyn ja kustannusten saldo. Katso [Kun joustavasti tietokannan resurssivarantoon käytetään?](sql-database-elastic-pool-guidance.md) lisätietoja.

**Voit muuttaa resurssivarantoon eDTUs tai tallennustilan rajoituksia ja eDTUs tietokantaa kohden:**

1. Avaa **Määritä resurssivarantoon** -sivu.

    Valitse **joustavasti tietokannan ryhmän asetukset**-ryhmän asetusten muuttaminen joko liukusäätimen avulla.

    ![Joustavasti resurssivarannon resurssien käyttö](./media/sql-database-elastic-pool-manage-portal/resize-pool.png)

2. Kun asetus muuttuu, näytössä näkyy arvioitu kuukausihinta muutoksesta.

    ![Resurssivarannon ja uusi kuukausihinta päivittäminen](./media/sql-database-elastic-pool-manage-portal/pool-change-edtu.png)


## <a name="create-and-manage-elastic-jobs"></a>Luoda ja hallita joustavasti töitä

Joustavasti työt avulla voit suorittaa Transact-SQL-komentosarjoja altaan tietokantojen määrää. Voit luoda uusia töitä tai hallita aiemmin työt-portaalissa.

![Luoda ja hallita joustavasti töitä][5]


Ennen kuin käytät työt, joustavasti työt-osien asentaminen ja anna tunnistetiedot. Lisätietoja on artikkelissa [joustavasti tietokannan töiden yleiskatsaus](sql-database-elastic-jobs-overview.md).

Katso [Skaalaus ulos Azure SQL-tietokantaan](sql-database-elastic-scale-introduction.md): joustavasti Tietokantatyökalut avulla asteikko-kohtaa, Siirrä tiedot, kyselyn tai luoda tapahtumia.



## <a name="additional-resources"></a>Lisäresursseja

- [SQL-tietokantaan joustavasti resurssivarantoon](sql-database-elastic-pool.md)
- [Luo joustavasti tietokannan resurssivarantoon-portaalissa](sql-database-elastic-pool-create-csharp.md)
- [Luo joustavasti tietokannan resurssivarantoon PowerShellin avulla](sql-database-elastic-pool-create-powershell.md)
- [Luo joustavasti tietokannan resurssivarantoon C#](sql-database-elastic-pool-create-csharp.md)
- [Joustavasti tietokannan jakavat hinnan ja suorituskyvyn Huomioitavaa](sql-database-elastic-pool-guidance.md)


<!--Image references-->
[1]: ./media/sql-database-elastic-pool-manage-portal/configure-pool.png
[2]: ./media/sql-database-elastic-pool-manage-portal/basic.png
[3]: ./media/sql-database-elastic-pool-manage-portal/basic-2.png
[4]: ./media/sql-database-elastic-pool-manage-portal/basic-3.png
[5]: ./media/sql-database-elastic-pool-manage-portal/elastic-jobs.png
[6]: ./media/sql-database-elastic-pool-manage-portal/edit-metric.png
[7]: ./media/sql-database-elastic-pool-manage-portal/select-dbs.png
[8]: ./media/sql-database-elastic-pool-manage-portal/db-utilization.png
[9]: ./media/sql-database-elastic-pool-manage-portal/metric.png
