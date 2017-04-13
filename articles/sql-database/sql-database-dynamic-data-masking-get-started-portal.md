<properties
   pageTitle="SQL tietokannan Dynamic Data Masking (perinteinen Azure-portaalin) käytön aloittaminen"
   description="Miten SQL tietokannan dynaaminen tietojen Masking perinteinen Azure-portaalin käytön aloittaminen"
   services="sql-database"
   documentationCenter=""
   authors="ronitr"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="07/10/2016"
   ms.author="ronitr; ronmat; v-romcal; sstein"/>

# <a name="get-started-with-sql-database-dynamic-data-masking-azure-classic-portal"></a>SQL tietokannan Dynamic Data Masking (perinteinen Azure-portaalin) käytön aloittaminen

> [AZURE.SELECTOR]
- [Dynamic Data naamioiminen - Azure Portal](sql-database-dynamic-data-masking-get-started.md)

## <a name="overview"></a>Yleiskatsaus

Luottamuksellisten tietojen näyttäminen SQL tietokanta Dynamic Data-Masking rajoittaa mukaan masking käyttöoikeudeton käyttäjille. Dynaamiset tiedot piilottamisen tuetaan Vipuventtiili V12 Azure SQL-tietokanta-versiota.

Dynaamiset tiedot piilottamisen auttaa estämään luvatonta pääsyä luottamuksellisia tietoja ottamalla asiakkaat voivat määrittää, kuinka suuri osa tulee näkyviin ja vaikuttaa mahdollisimman vähän sovelluskerroksen luottamuksellisia tietoja. Se on käytännön perustuva suojaus-ominaisuus, joka piilottaa luottamuksellisia tietoja kyselyn tulosjoukon nimettyjen Tietokantakentät päälle, tietokannan tiedot eivät muutu.

Esimerkiksi edustajaan puhelun keskiosassa voi tunnistaa soittajat useita numeroiden niiden sosiaaliturvatunnus tai luottokortin numeroa, mutta tietojen kohteiden pitäisi ei voi täysin tarjoamia edustajalle. Naamioiminen säännön voidaan määrittää, että kaikki muodot, mutta viimeiset neljä numeroa sosiaaliturvatunnus tai luottokortin numeroa tuloksessa useat kyselyä. Toinen esimerkki syöttörajoitetta tarvittavat tiedot voidaan määrittää henkilökohtaisia tietoja (PII) tietojen niin, että kehittäjä voi tehdä kyselyn tuotannon ympäristöissä ilman rikkomiseen yhteensopivuuden asetusten vianmääritystä varten.

## <a name="sql-database-dynamic-data-masking-basics"></a>SQL tietokanta Dynamic Data-Masking perusteet

Voit määrittää dynaamiset masking käytännön Azure perinteinen portaalissa tietokannan valvonta- ja tietoturva-välilehdessä tiedot.


> [AZURE.NOTE] Määrittää dynaamiset tiedot masking Azure-portaalissa, on artikkelissa [SQL tietokanta Dynamic Data Masking (Azure-portaalin) käytön aloittaminen](sql-database-dynamic-data-masking-get-started.md).


### <a name="dynamic-data-masking-permissions"></a>Dynaamiset tiedot piilottamisen käyttöoikeudet

Dynaamiset tiedot naamioiminen voi määrittää Azure-tietokanta-järjestelmänvalvoja, palvelimen järjestelmänvalvojan tai suojauksen hyväksyjän roolit.

### <a name="dynamic-data-masking-policy"></a>Dynaamiset tiedot naamioiminen käytäntö

* **SQL-käyttäjät ulkopuolelle piilottamisen** - SQL-käyttäjät tai AAD käyttäjätietoja, jotka saavat yhteysmerkkijonoon tietojen SQL-kyselyn tuloksissa. Huomaa, että käyttäjät, joilla on järjestelmänvalvojan oikeudet aina jätetään pois piilottamisen ja tulee näkyviin alkuperäiset tiedot ilman mitään syöttörajoitteen.

* **Masking säännöt** - sääntöjä, jotka määrittävät määritetyt kentät, jotka voidaan peitä ja peite-funktio, jota käytetään. Määritetyt kentät voidaan määrittää tietokannan rakenteen nimi, taulukkonimi ja sarakkeen nimi.

* **Masking Funktiot** - joukon tavoista, joilla hallitaan eri skenaarioissa tietojen näyttäminen.

| Naamioiminen-funktio | Masking logiikka |
|----------|---------------|
| **Oletusarvo**  |**Koko naamioiminen nimettyjen kenttien tietotyyppien mukaan**<br/><br/>• Käytä XXXX tai vähemmän Xs Jos kentän koko on alle 4 merkkejä merkkijonon tietotyyppien (nchar, ntext, nvarchar).<br/>• Käyttää nolla-arvojen numeerisista tietotyypeistä (bigint, bittinen, desimaalien, int, rahaa, numeerinen, smallint, smallmoney, tinyint, liukuluku, yhdessä).<br/>• Käyttää 01 01 – 1900 pvm. / klo-tietotyyppien (päivämäärä, datetime2, päivämäärä ja aika, DateTimeOffset-arvoa, smalldatetime, kellonaika).<br/>• SQL variantin nykyisen oletusarvo on käytössä.<br/>• XML tiedoston <masked/> käytetään.<br/>• Käyttää tyhjää arvoa määräten tietotyyppien (aikaleima taulukossa, hierarchyid, GUID-tunnus, binaarinen, kuva, varbinary paikkatietojen tyypit).
| **Luottokortti** |**Masking menetelmä, joka paljastaa neljä viimeistä numeroa määritetyt kentät** ja lisää vakion merkkijonon etuliitteenä luottokorttia muodossa.<br/><br/>XXXX-XXXX-XXXX-1234|
| **Sosiaaliturvatunnus** |**Masking menetelmä, joka paljastaa neljä viimeistä numeroa määritetyt kentät** ja lisää vakion merkkijonon etuliitteenä American sosiaaliturvatunnus muodossa.<br/><br/>XXX-XX-1234 |
| **Sähköpostin** | **Menetelmä, joka paljastaa ensimmäisen kirjaimen ja korvaa XXX.com toimialueeseen masking** käyttämällä vakio merkkijonon etuliite sähköpostiosoite muodossa.<br/><br/>aXX@XXXX.com |
| **SATUNNAISLUKU** | **Masking menetelmää, joka luo satunnaisen numeron** mukaan valitun rajat ja todellisten tietojen tietotyyppejä. Jos määritetyt rajat ovat yhtä suuret naamioiminen-funktio on vakioluvulla.<br/><br/>![Siirtymisruutu](./media/sql-database-dynamic-data-masking-get-started-portal/1_DDM_Random_number.png) |
| **Mukautetun tekstin** | **Menetelmä, joka paljastaa ensimmäisen ja viimeisen merkin masking** ja Lisää mukautettu täyttöä merkkijonon keskellä. Jos alkuperäisen merkkijonon on lyhyempi näkyviä etuliite ja jälkiliite, käytetään täyttö-merkkijono.<br/>etuliitteen [täyttöä] jälkiliite<br/><br/>![Siirtymisruutu](./media/sql-database-dynamic-data-masking-get-started-portal/2_DDM_Custom_text.png) |


<a name="Anchor1"></a>

## <a name="set-up-dynamic-data-masking-for-your-database-using-the-azure-classic-portal"></a>Dynaamiset tiedot piilottamisen Azure perinteinen portaalissa tietokannan määrittäminen

1. Käynnistä Azure perinteinen-portaalissa osoitteessa [https://manage.windowsazure.com](https://manage.windowsazure.com).

2. Valitse haluamasi peite tietokanta ja valitse sitten **valvonta ja suojaus** -välilehti.

3. **Dynaamiset masking tiedot**-kohdassa käyttöön dynaamiset tiedot masking ominaisuus **käytössä** .  

4. Kirjoita SQL-käyttäjät tai AAD käyttäjätietoja, jotka jättää pois piilottamisen ja käyttämään yhteysmerkkijonoon luottamuksellisia tietoja. Tämä on oltava käyttäjät puolipisteillä erotettu luettelo. Huomaa, että järjestelmänvalvojan oikeuksilla käyttäjät voivat aina alkuperäisen yhteysmerkkijonoon tietoihin pääsy.

    >[AZURE.TIP] Minkä ansiosta, jotta sovelluskerroksen voi näyttää luottamuksellisia tietoja etuoikeutettu sovelluksen käyttäjille, lisää SQL-käyttäjä tai sovellus käyttää tietokantakyselyn AAD tunnistetiedot. On suositeltavaa, että tämän luettelon sisältävät vähän sellaisten käyttäjien Pienennä luottamuksellisten tietojen näyttäminen.

    ![Siirtymisruutu](./media/sql-database-dynamic-data-masking-get-started-portal/4_ddm_policy_classic_portal.png)

5. Valikkorivin sivun alareunassa valitsemalla **Lisää SYÖTTÖRAJOITTEEN** voit avata piilottamisen säännön ikkunaan.

6. Valitse **rakenne**, **taulukoiden** ja **sarakkeiden** voidaan määrittää nimetyn kentät, jotka peitä avattavasta luettelosta.

7. Valitse **MASKING FUNKTION** luottamuksellisia tietoja masking luokat-luettelosta.

    ![Siirtymisruutu](./media/sql-database-dynamic-data-masking-get-started-portal/5_DDM_Add_Masking_Rule_Classic_Portal.png)

8. Valitse **OK** tietojen masking säännön ikkunaan päivittämään joukko masking dynaamiset tiedot masking käytännön säännöt.

9. Valitse Tallenna uusi tai päivitetty piilottamisen käytännön **Tallenna** .


## <a name="set-up-dynamic-data-masking-for-your-database-using-transact-sql-statements"></a>Dynaamiset tiedot käyttämällä Transact-SQL-lauseita tietokannan masking määrittäminen

Katso [Dynamic Data Masking](https://msdn.microsoft.com/library/mt130841.aspx).

## <a name="set-up-dynamic-data-masking-for-your-database-using-powershell-cmdlets"></a>Dynaamiset tiedot käyttämällä Powershell cmdlet-komennot tietokannan masking määrittäminen

Katso [Azure SQL-tietokannan cmdlet-komennot](https://msdn.microsoft.com/library/azure/mt574084.aspx).

## <a name="set-up-dynamic-data-masking-for-your-database-using-rest-api"></a>Dynaamiset tiedot piilottamisen käyttämällä REST API tietokannan määrittäminen

Katso [Azure SQL-tietokantoja-toimintoja](https://msdn.microsoft.com/library/dn505719.aspx).
