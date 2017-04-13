<properties
    pageTitle="Aloita suorittamalla ottaminen käyttöön tietokantaa Venytys ohjatun | Microsoft Azure"
    description="Opettele määrittämään tietokannan Venytys tietokannan suorittamalla Venytys ohjatun ottaminen käyttöön tietokantaa."
    services="sql-server-stretch-database"
    documentationCenter=""
    authors="douglaslMS"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-server-stretch-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="08/05/2016"
    ms.author="douglasl"/>

# <a name="get-started-by-running-the-enable-database-for-stretch-wizard"></a>Aloita suorittamalla ottaminen käyttöön tietokantaa Venytys ohjatun

Tietokannan määrittämiseksi Venytys tietokannan, suorita Venytys ohjatun ottaminen käyttöön tietokantaa.  Tässä ohjeaiheessa kerrotaan, kirjoita tiedot ja vaihtoehdot, sinun on tehtävä ohjatun toiminnon.

Saat lisätietoja Venytys tietokannan artikkelissa [Venytys tietokannan](sql-server-stretch-database-overview.md).

 >   [AZURE.NOTE] Myöhemmin, jos poistat Venytys tietokannan, muista, että käytöstä Venytys tietokannan lisääminen taulukkoon tai tietokantaan ei poista remote objekti. Jos haluat poistaa remote taulukon tai etätietokantaan, sinun on pudota se käyttämällä Azure hallinta-portaalin. Remote-objekteille Siirry maksamaan Azure kustannukset, kunnes poistat ne manuaalisesti. 

## <a name="launch-the-wizard"></a>Käynnistä ohjattu toiminto

1.  Valitse tietokanta, jossa haluat ottaa käyttöön Venytys SQL Server Management Studiossa, objektin Resurssienhallinnassa.

2.  Oikealle\-napsauttamalla ja valitse **tehtävät**- ja valitse sitten **Venytys**ja valitse sitten käynnistää ohjatun toiminnon **käyttöön** .

## <a name="Intro"></a>Johdanto
Tarkista ohjattu toiminto ja edellytykset käyttötarkoituksen.

Tärkeää edellytykset ovat seuraavat:

-   Sinun on järjestelmänvalvoja voi muuttaa tietokanta-asetukset.
-   Sinulla on Microsoft Azure-tilauksia.
-   SQL Server on voi pitää yhteyttä Azure etäpalvelimeen.

![Venytä tietokantaan ohjatun toiminnon sivulla esittely][StretchWizardImage1]

## <a name="Tables"></a>Valitse taulukot
Valitse taulukot, jonka haluat ottaa käyttöön Venytys.

Taulukoita, joissa on paljon rivejä yläpuolelle lajitellussa luettelossa. Ennen kuin ohjattu toiminto näyttää taulukoiden luettelosta, se analysoi ne tietotyyppien, joita ei tällä hetkellä tueta Venytys tietokanta.

![Venytä tietokantaan ohjatun toiminnon Valitse taulukot-sivu][StretchWizardImage2]

|Sarakkeen|Kuvaus|
|----------|---------------|
|(ei otsikkoa)|Tässä sarakkeessa, jos haluat ottaa käyttöön valitun taulukon Venytys valintaruutu.|
|**Nimi**|Määrittää taulukossa sarakkeen nimi.|
|(ei otsikkoa)|Tässä sarakkeessa merkki voivat edustaa varoitus joka ei\'t estää ottaminen käyttöön Venytys valitun taulukon. Se myös esittää ongelman, joka estää valitun taulukon ottaminen käyttöön Venytys \- esimerkiksi koska taulukon käyttää tietotyyppi, jota ei tueta. Pitämällä hiiren osoitinta Näytä lisätietoja työkaluvihjeessä symboli. Saat lisätietoja, [Venytys tietokannan koskevat rajoitukset](sql-server-stretch-database-limitations.md).|
|**Venytetään**|Ilmaisee, onko taulukko on jo käytössä Venytys.|
|**Siirtää**|Voit siirtää koko taulukon (**Koko taulukon**) tai aiemmin luotuun sarakkeeseen taulukossa voit määrittää suodattimen. Jos haluat siirtää rivien valitseminen eri suodatinfunktion avulla, suorittaa Määritä filter-funktio, kun suljet ohjatun toiminnon ALTER TABLE-lause. Saat lisätietoja filter-funktio [Valitse rivit, voit siirtää suodatin-funktiolla](sql-server-stretch-database-predicate-function.md). Katso lisätietoja siitä, miten voit käyttää funktiota [Käyttöön Venytys tietokannan taulukkoon](sql-server-stretch-database-enable-table.md) tai [ALTER TABLE (Transact-SQL)](https://msdn.microsoft.com/library/ms190273.aspx).|
|**Rivit**|Määrittää taulukon rivien määrä.|
|**Koko (kt)**|Määrittää taulukon koon kilotavuina.|

## <a name="Filter"></a>Kirjoittaa rivi-suodatin

Jos haluat säätää suodatinfunktion avulla voit siirtää rivien valitseminen, älä **Valitse taulukot** -sivulla seuraavat kohdat.

1.  Valitse **Valitse Venytä taulukot** -luettelosta rivin taulukon **Koko taulukon** . **Valitse rivit venyttämällä** -valintaikkuna avautuu.

    ![Määritä suodatin-funktio][StretchWizardImage2a]

2.  Valitse **venyttämällä rivien valitseminen** -valintaikkunassa **Valitse rivit**.

3.  Anna suodatinfunktion nimi **nimi-kenttään**.

4.  Valitse taulukosta sarake, valitse operaattori ja arvo antaa **Where** -lause.

5. Valitse **Tarkista** voit testata funktiota. Jos funktio palauttaa tulokset - taulukosta, jos haluat siirtää - ehdon täyttävät rivit testi raportit **onnistui**.

    >   [AZURE.NOTE] Tekstiruutu, jossa näkyy suodatuskyselyn on vain luku-tilassa. Et voi muokata kyselyä, kirjoita tekstiruutuun.

6.  Valitse valmis, palaa **Valitse taulukot** -sivu.

Filter-funktiossa luodaan SQL Server vain silloin, kun ohjatun toiminnon. Vasta sitten voit palata **Valitse taulukot** -sivun muuttaminen tai uudelleennimeäminen filter-funktio.

![Valitse taulukot-sivun määritettyäsi suodatin-funktio][StretchWizardImage2b]

Jos haluat tyypin suodatin-funktion avulla voit valita rivien siirtää, tee jokin seuraavista toimista.  

-   Sulje ohjattu toiminto ja suorita ALTER TABLE-lause Venytys käyttöön taulukko ja määritä suodatin-funktio. Saat lisätietoja, [Käyttöön Venytys tietokannan taulukkoon](sql-server-stretch-database-enable-table.md).  

-   ALTER TABLE-lause Määritä filter-funktiossa, kun suljet ohjatun toiminnon suorittaminen Artikkelissa [suodatinfunktion, kun olet suorittanut ohjatun toiminnon Lisää](sql-server-stretch-database-predicate-function.md#addafterwiz)tarvittavat vaiheet.

## <a name="Configure"></a>Määritä Azure käyttöönotto

1.  Kirjaudu Microsoft Azure Microsoft-tilillä.

    ![Azure - tietokannan Venytys kirjautuminen][StretchWizardImage3]

2.  Valitse käytettävä Venytys tietokannan aiemmin Azure tilaus.

3.  Valitse Azure alue.
    -   Jos luot uuden palvelimen, palvelin on luotu tällä alueella.  
    -   Jos sinulla on aiemmin palvelinten valitun alueen, ohjattu toiminto näkyvät valitessasi **aiemmin palvelimeen**.

    Pienennä viive, valitse Azure alue, jossa SQL Server on. Saat lisätietoja alueiden [Azure-alueita](https://azure.microsoft.com/regions/).

4.  Määrittää, haluatko käyttää olemassa olevan palvelimen tai luoda uuden Azure palvelimen.

    Jos Active Directory SQL Server on liitetty Azure Active Directory-hakemistosta, voit halutessasi Azure etäpalvelimeen yhteydenpito liitetyt palvelutilin SQL Server avulla. Saat lisätietoja tämän asetuksen vaatimukset [Muuttaa TIETOKANNAN asetusten määrittäminen (Transact-SQL)](https://msdn.microsoft.com/library/bb522682.aspx).

    -   **Luo uusi palvelin**

        1.  Palvelimen järjestelmänvalvoja luo sisäänkirjautuminen- ja salasana.

        2.  Voit myös käyttää liitetyt palvelutilin SQL Server Azure etäpalvelimeen yhteydessä.

        ![Luo uusi Azure server - tietokannan Venytys][StretchWizardImage4]

    -   **Olemassa olevan palvelimen**

        1.  Valitse olemassa oleva Azure-palvelin.

        2.  Valita todentamismenetelmän.

            -   Jos valitset **SQL Server-todennus**, anna järjestelmänvalvojan käyttäjätunnus ja salasana.

            -   Valitse liitetyt palvelutilin SQL Serverin avulla voit pitää yhteyttä etäpalvelimeen Azure **Active Directory-todennusta** . Jos valittu palvelin ei ole integroitu Azure Active Directory-vaihtoehtoa ei näy.

        ![Valitse aiemmin luotu Azure server - tietokannan Venytys][StretchWizardImage5]

## <a name="Credentials"></a>Suojatun tunnistetiedot
Käytössä on oltava tietokannan perustyyli näppäintä suojaamiseen tunnistetiedot, jotka Venytys tietokanta käyttää Muodosta yhteys etätietokantaan.  

Jos avaimeksi perustyyli on jo, kirjoita se salasana.  

![Venytä tietokantaan ohjatun toiminnon sivulla suojatun tunnistetiedot][StretchWizardImage6b]

Jos tietokanta ei ole olemassa perustyyli-näppäintä, kirjoita vahvan salasanan luominen avaimeksi perustyyli.  

![Venytä tietokantaan ohjatun toiminnon sivulla suojatun tunnistetiedot][StretchWizardImage6]

Saat lisätietoja tietokannan perustyyli avain [luominen MASTER KEY (Transact-SQL)](https://msdn.microsoft.com/library/ms174382.aspx) ja [Luo avaimeksi perustyyli](https://msdn.microsoft.com/library/aa337551.aspx). Saat lisätietoja tunnistetiedot, jotka ohjattu toiminto luo [Luominen TIETOKANNAN SUODATETUT TUNNISTETIEDON (Transact-SQL)](https://msdn.microsoft.com/library/mt270260.aspx).

## <a name="Network"></a>Valitse IP-osoite
Luo palomuurisäännön, jonka avulla SQL Server viestiä, Azure etäpalvelimeen Azure aliverkon IP-osoitealueita (suositus) tai SQL Server, julkiseen IP-osoitteen avulla.

IP-osoite tai osoitteet, jotka tarjoavat tällä sivulla Ilmoita Azure-palvelimella, jotta saapuvat tiedot, kyselyt ja hallintatoiminnot aloittamia SQL Server Azure palomuurin läpi. Ohjattu toiminto ei muuta mitään SQL Serverin palomuuriasetukset.

![Valitse IP address sivu Venytys tietokantaan ohjatun toiminnon][StretchWizardImage7]

## <a name="Summary"></a>Yhteenveto
Tarkista arvot, joita käytit ja asetuksista, jotka olet valinnut ohjatun toiminnon ja Azure arvioitu kustannuksia. Valitse **Valmis** ottamaan Venytys.

![Yhteenveto Venytys tietokantaan ohjatun toiminnon sivulla][StretchWizardImage8]

## <a name="Results"></a>Tulokset
Tarkista tulokset.

Voit valvoa tietojen siirtäminen [näyttö ja vianmääritys tietojen siirtäminen (Venytys-tietokanta)](sql-server-stretch-database-monitor.md).

![Venytä tietokantaan ohjatun toiminnon sivulla tulokset][StretchWizardImage9]

## <a name="KnownIssues"></a>Ohjatun toiminnon vianmääritys
**Venytä tietokannan epäonnistui.**
Jos Venytys tietokanta ei ole vielä käytössä palvelimen tasolla ja järjestelmänvalvojan oikeudet, jotta se Suorita ohjattu käynnistämättä järjestelmää, ohjattu toiminto epäonnistuu. Pyydä järjestelmänvalvojaa Venytys tietokannan ottaminen käyttöön paikallisten server-esiintymän ja suorita sitten ohjattu toiminto uudelleen. Saat lisätietoja, [valmistelevat: Venytys tietokannan käyttöön palvelimen käyttöoikeus](sql-server-stretch-database-enable-database.md#EnableTSQLServer).

## <a name="next-steps"></a>Seuraavat vaiheet
Käyttöön Venytys tietokannan taulukoihin. Tietojen siirtäminen seurata ja hallita Venytys\-käytössä tietokannat ja taulukot.

-   [Ota käyttöön Venytys tietokannan taulukkoon](sql-server-stretch-database-enable-table.md) käyttöön taulukoita.

-   [Näyttö ja vianmääritys tietojen siirtäminen](sql-server-stretch-database-monitor.md) tietojen siirtäminen tilan tarkasteleminen.

-   [Keskeyttäminen ja jatkaminen Venytys tietokanta](sql-server-stretch-database-pause.md)

-   [Hallinta ja Venytys tietokannan vianmääritys](sql-server-stretch-database-manage.md)

-   [Varmuuskopion Venytys käyttävä tietokannat](sql-server-stretch-database-backup.md)

## <a name="see-also"></a>Katso myös

[Ota käyttöön Venytys tietokannan tietokantaan](sql-server-stretch-database-enable-database.md)

[Venytä tietokannan käyttöön taulukon](sql-server-stretch-database-enable-table.md)

[StretchWizardImage1]: ./media/sql-server-stretch-database-wizard/stretchwiz1.png
[StretchWizardImage2]: ./media/sql-server-stretch-database-wizard/stretchwiz2.png
[StretchWizardImage2a]: ./media/sql-server-stretch-database-wizard/stretchwiz2a.png
[StretchWizardImage2b]: ./media/sql-server-stretch-database-wizard/stretchwiz2b.png
[StretchWizardImage3]: ./media/sql-server-stretch-database-wizard/stretchwiz3.png
[StretchWizardImage4]: ./media/sql-server-stretch-database-wizard/stretchwiz4.png
[StretchWizardImage5]: ./media/sql-server-stretch-database-wizard/stretchwiz5.png
[StretchWizardImage6]: ./media/sql-server-stretch-database-wizard/stretchwiz6.png
[StretchWizardImage6b]: ./media/sql-server-stretch-database-wizard/stretchwiz6b.png
[StretchWizardImage7]: ./media/sql-server-stretch-database-wizard/stretchwiz7.png
[StretchWizardImage8]: ./media/sql-server-stretch-database-wizard/stretchwiz8.png
[StretchWizardImage9]: ./media/sql-server-stretch-database-wizard/stretchwiz9.png
