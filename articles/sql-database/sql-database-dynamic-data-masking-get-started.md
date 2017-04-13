<properties
   pageTitle="SQL tietokanta Dynamic Data Masking (Azure-portaalin) käytön aloittaminen"
   description="Miten SQL tietokannan Dynamic Data Masking Azure-portaalin käytön aloittaminen"
   services="sql-database"
   documentationCenter=""
   authors="ronitr"
   manager="jhubbard"
   editor="v-romcal"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="07/10/2016"
   ms.author="ronitr; ronmat; v-romcal; sstein"/>


# <a name="get-started-with-sql-database-dynamic-data-masking-azure-portal"></a>SQL tietokanta Dynamic Data Masking (Azure-portaalin) käytön aloittaminen

> [AZURE.SELECTOR]
- [Dynamic Data naamioiminen - Azure perinteinen Portal](sql-database-dynamic-data-masking-get-started-portal.md)

## <a name="overview"></a>Yleiskatsaus

Luottamuksellisten tietojen näyttäminen SQL tietokanta Dynamic Data-Masking rajoittaa mukaan masking käyttöoikeudeton käyttäjille. Dynaamiset tiedot piilottamisen tuetaan Vipuventtiili V12 Azure SQL-tietokanta-versiota.

Dynaamiset tiedot piilottamisen auttaa estämään luvatonta pääsyä luottamuksellisia tietoja ottamalla asiakkaat voivat määrittää, kuinka suuri osa tulee näkyviin ja sovelluskerroksen vaikuttaa mahdollisimman vähän luottamuksellisia tietoja. Se on käytännön perustuva suojaus-ominaisuus, joka piilottaa luottamuksellisia tietoja kyselyn tulosjoukon nimettyjen Tietokantakentät päälle, tietokannan tiedot eivät muutu.

Esimerkiksi edustajaan puhelun keskiosassa voi tunnistaa soittajat useita numeroiden niiden sosiaaliturvatunnus tai luottokortin numeroa, mutta tietojen kohteiden pitäisi ei voi täysin tarjoamia edustajalle. Naamioiminen säännön voidaan määrittää, että kaikki muodot, mutta viimeiset neljä numeroa sosiaaliturvatunnus tai luottokortin numeroa tuloksessa useat kyselyä. Toinen esimerkki syöttörajoitetta tarvittavat tiedot voidaan määrittää henkilökohtaisia tietoja (PII) tietojen niin, että kehittäjä voi tehdä kyselyn tuotannon ympäristöissä ilman rikkomiseen yhteensopivuuden asetusten vianmääritystä varten.

## <a name="sql-database-dynamic-data-masking-basics"></a>SQL tietokanta Dynamic Data-Masking perusteet

Voit määrittää dynaamiset tiedot, masking käytännön Azure-portaalissa Dynamic Data Masking-toiminto valitsemalla SQL-tietokantaan määritys-sivu tai asetukset-sivu.


### <a name="dynamic-data-masking-permissions"></a>Dynaamiset tiedot piilottamisen käyttöoikeudet

Dynaamiset tiedot naamioiminen voi määrittää Azure-tietokanta-järjestelmänvalvoja, palvelimen järjestelmänvalvojan tai suojauksen hyväksyjän roolit.

### <a name="dynamic-data-masking-policy"></a>Dynaamiset tiedot piilottamisen käytäntö

* **SQL-käyttäjät ulkopuolelle piilottamisen** - SQL-käyttäjät tai AAD käyttäjätietoja, jotka saavat yhteysmerkkijonoon tietojen SQL-kyselyn tuloksissa. Huomaa, että käyttäjät, joilla on järjestelmänvalvojan oikeudet aina jätetään pois piilottamisen ja tulee näkyviin alkuperäiset tiedot ilman mitään syöttörajoitteen.

* **Masking säännöt** - sääntöjä, jotka määrittävät määritetyt kentät, jotka voidaan peitä ja peite-funktio, jota käytetään. Määritetyt kentät voidaan määrittää tietokannan rakenteen nimi, taulukkonimi ja sarakkeen nimi.

* **Masking Funktiot** - joukon tavoista, joilla hallitaan eri skenaarioissa tietojen näyttäminen.

| Naamioiminen-funktio | Masking logiikka |
|----------|---------------|
| **Oletusarvo**  |**Koko naamioiminen nimettyjen kenttien tietotyyppien mukaan**<br/><br/>• Käytä XXXX tai vähemmän Xs Jos kentän koko on alle 4 merkkejä merkkijonon tietotyyppien (nchar, ntext, nvarchar).<br/>• Käyttää nolla-arvojen numeerisista tietotyypeistä (bigint, bittinen, desimaalien, int, rahaa, numeerinen, smallint, smallmoney, tinyint, liukuluku, yhdessä).<br/>• Käyttää 01 01 – 1900 pvm. / klo-tietotyyppien (päivämäärä, datetime2, päivämäärä ja aika, DateTimeOffset-arvoa, smalldatetime, kellonaika).<br/>• SQL variantin nykyisen oletusarvo on käytössä.<br/>• XML tiedoston <masked/> käytetään.<br/>• Käyttää tyhjää arvoa määräten tietotyyppien (aikaleima taulukossa, hierarchyid, GUID-tunnus, binaarinen, kuva, varbinary paikkatietojen tyypit).
| **Luottokortti** |**Masking menetelmä, joka paljastaa neljä viimeistä numeroa määritetyt kentät** ja lisää vakion merkkijonon etuliitteenä luottokorttia muodossa.<br/><br/>XXXX-XXXX-XXXX-1234|
| **Sosiaaliturvatunnus** |**Masking menetelmä, joka paljastaa neljä viimeistä numeroa määritetyt kentät** ja lisää vakion merkkijonon etuliitteenä American sosiaaliturvatunnus muodossa.<br/><br/>XXX-XX-1234 |
| **Sähköpostin** | **Menetelmä, joka paljastaa ensimmäisen kirjaimen ja korvaa XXX.com toimialueeseen masking** käyttämällä vakio merkkijonon etuliite sähköpostiosoite muodossa.<br/><br/>aXX@XXXX.com |
| **SATUNNAISLUKU** | **Masking menetelmä, joka luo satunnaisen numeron** mukaan valitun rajat ja todellisten tietojen tietotyyppejä. Jos määritetyt rajat ovat yhtä suuret piilottamisen-funktio on vakioluvulla.<br/><br/>![Siirtymisruutu](./media/sql-database-dynamic-data-masking-get-started/1_DDM_Random_number.png) |
| **Mukautetun tekstin** | **Masking menetelmää, joka paljastaa ensimmäisen ja viimeisen merkin** ja Lisää mukautettu täyttöä merkkijonon keskellä. Jos alkuperäisen merkkijonon on lyhyempi näkyviä etuliite ja jälkiliite, käytetään täyttö-merkkijono. <br/>etuliitteen [täyttöä] jälkiliite<br/><br/>![Siirtymisruutu](./media/sql-database-dynamic-data-masking-get-started/2_DDM_Custom_text.png) |


<a name="Anchor1"></a>
### <a name="recommended-fields-to-mask"></a>Suositellut kentät peite

DDM suositukset-ohjelma merkitsee mahdollisesti luottamukselliset kenttinä, jotka voivat olla piilottamisen käytettävyys tiettyjen kenttien tietokannasta. Portaalin Dynamic Data Masking-sivu tulee näkyviin suositeltu sarakkeet tietokannan. Kaikki sinun on suoritettava on käyttää syöttörajoitteen kyseisille kentille valitsemalla yhden tai usean sarakkeen **Lisätä syöttörajoitteen** ja valitse sitten **Tallenna** .

## <a name="set-up-dynamic-data-masking-for-your-database-using-the-azure-portal"></a>Dynaamiset tiedot piilottamisen Azure-portaalissa tietokannan määrittäminen

1. Käynnistä Azure-portaalissa osoitteessa [https://portal.azure.com](https://portal.azure.com).

2. Siirry asetukset-sivu, joka sisältää arkaluontoisia tietoja haluat peite tietokannan.

3. Napsauta **Dynamic Data Masking** ruutua joka käynnistää **Dynamic Data Masking** määritys-sivu.

    * Voit vaihtoehtoisesti **Toiminnot** -osassa kohtaan ja valitse **Dynamic Data Masking**.

    ![Siirtymisruutu](./media/sql-database-dynamic-data-masking-get-started/4_ddm_settings_tile.png)<br/><br/>


4. **Dynamic Data Masking** määritys-sivu näkyy joitakin tietokannan sarakkeita, jotka suositukset-ohjelma on merkitty piilottamisen. Hyväksy suositukset, jotta yhden tai usean sarakkeen, napsauta **Lisää syöttörajoitteen** ja peite luodaan oletuslajin tämän sarakkeen mukaan. Voit muuttaa naamioiminen-funktion myös valitsemalla piilottamisen sääntö ja muokkaamista piilottamisen kentän muoto valittua toiseen muotoon. Muista **tallentaa** Tallenna asetukset.

    ![Siirtymisruutu](./media/sql-database-dynamic-data-masking-get-started/5_ddm_recommendations.png)<br/><br/>


5. Voit lisätä syöttörajoitteen sarakkeen tietokannan yläreunaan **Dynamic Data Masking** määritys-sivu valitsemalla **Lisää syöttörajoitteen** Avaa **Masking säännön lisääminen** määritys-sivu

    ![Siirtymisruutu](./media/sql-database-dynamic-data-masking-get-started/6_ddm_add_mask.png)<br/><br/>

6. Valitse **rakenne**, **taulukoiden** ja **sarakkeiden** määrittää nimetyn kentän, joka peitä.

7. Valitse **Kentän muoto Masking** luottamuksellisia tietoja masking luokat-luettelosta.

    ![Siirtymisruutu](./media/sql-database-dynamic-data-masking-get-started/7_ddm_mask_field_format.png)<br/><br/>     

8. Tietojen masking säännön sivu päivittämään joukko masking sääntöjä masking käytännön dynaamiset tiedot valitsemalla **Tallenna** .

9. Kirjoita SQL-käyttäjät tai AAD käyttäjätietoja, jotka jättää pois piilottamisen ja käyttämään yhteysmerkkijonoon luottamuksellisia tietoja. Tämä on oltava käyttäjät puolipisteillä erotettu luettelo. Huomaa, että järjestelmänvalvojan oikeuksilla käyttäjät voivat aina alkuperäisen yhteysmerkkijonoon tietoihin pääsy.

    ![Siirtymisruutu](./media/sql-database-dynamic-data-masking-get-started/8_ddm_excluded_users.png)

    >[AZURE.TIP] Minkä ansiosta, jotta sovelluskerroksen voi näyttää luottamuksellisia tietoja etuoikeutettu sovelluksen käyttäjille, lisää SQL-käyttäjä tai sovellus käyttää tietokantakyselyn AAD tunnistetiedot. On suositeltavaa, että tämän luettelon sisältävät vähän sellaisten käyttäjien Pienennä luottamuksellisten tietojen näyttäminen.

10. Tietojen masking Tallenna uusi tai päivitetty piilottamisen käytännön määritys-sivu valitsemalla **Tallenna** .

## <a name="set-up-dynamic-data-masking-for-your-database-using-powershell-cmdlets"></a>Dynaamiset tiedot käyttämällä Powershell cmdlet-komennot tietokannan masking määrittäminen

Katso [Azure SQL-tietokannan cmdlet-komennot](https://msdn.microsoft.com/library/azure/mt574084.aspx).


## <a name="set-up-dynamic-data-masking-for-your-database-using-rest-api"></a>Dynaamiset tiedot piilottamisen käyttämällä REST API tietokannan määrittäminen

Katso [Azure SQL-tietokantoja-toimintoja](https://msdn.microsoft.com/library/dn505719.aspx).
