<properties
    pageTitle="Lisää DB2 yhdistimen logiikan-sovelluksissa | Microsoft Azure"
    description="Yleistä REST API parametreilla DB2-yhdistin"
    services=""
    documentationCenter="" 
    authors="gplarsen"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="logic-apps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="09/26/2016"
   ms.author="plarsen"/>


# <a name="get-started-with-the-db2-connector"></a>Aloita DB2-yhdistin
Microsoft yhdistimen DB2 muodostaa logiikan sovellusten resurssit, jotka on tallennettu IBM DB2-tietokantaan. Tämä yhdistin on Microsoft-asiakas pitää yhteyttä DB2-palvelin etätietokoneiden TCP/IP-verkon kautta. Tämä koskee cloud-tietokannat, kuten IBM Bluemix dashDB tai IBM DB2 Windows Azure virtualisointi käytössä, ja paikallisen tietokantoja, joissa paikallisen tietojen yhdyskäytävän. Artikkelissa [Tuetut luettelon](connectors-create-api-db2.md#supported-db2-platforms-and-versions) IBM DB2-ympäristöissä ja versiot (tässä kohdassa).

>[AZURE.NOTE] Tässä versiossa on artikkelissa koskee logiikan sovellusten yleiseen käyttöön (GA). 

DB2 yhdistimen tukee tietokannan seuraavat toimenpiteet:

- Luettelo tietokannan taulukoihin
- Lue yhden rivin SELECT-komennon käyttäminen
- Lue kaikki rivit SELECT-komennon käyttäminen
- Käyttämällä Lisää yhden rivin lisääminen
- Muuta yhden rivin UPDATEN avulla
- Poista käyttämällä yhden rivin poistaminen

Tässä ohjeaiheessa esitellään käyttämisestä yhdistimen prosessin tietokannan toimintoihin logiikan-sovelluksessa.

Lisätietoja logiikan sovellukset on artikkelissa [logiikan sovelluksen luominen](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="available-actions"></a>Käytettävissä olevat toiminnot
DB2 yhdistimen tukee logiikan sovelluksen-toiminnot:

- GetTables
- GetRow
- GetRows
- InsertRow
- UpdateRow
- DeleteRow


## <a name="list-tables"></a>Luettelo-taulukot
Logiikan-sovelluksen kaikki-toiminnon koostuu monessa vaiheessa luonnista Microsoft Azure-portaalissa.

Logiikan sovelluksen voit lisätä toiminnon luettelon taulukoihin DB2-tietokantaan. Toiminnon ohjaa yhdistimen käsittelemään DB2 rakenne-lauseessa, kuten `CALL SYSIBM.SQLTABLES`.

### <a name="create-a-logic-app"></a>Logiikan sovelluksen luominen
1.  Valitse **Azure Käynnistä taulun** **+** (plusmerkki) **Web + Mobile**ja sitten **Logiikan sovelluksen**.
2.  Kirjoita **nimi**, kuten `Db2getTables`- **tilaus**, **resurssiryhmä**, **sijainti**ja **Sovelluksen palvelun suunnitteleminen**. Valitse **Kiinnitä Raporttinäkymät-ikkunan**ja valitse sitten **Luo**.

### <a name="add-a-trigger-and-action"></a>Lisää käynnistin ja toiminto
1.  **Logiikan sovellusten suunnittelu**Valitse **Tyhjä LogicApp** **Mallit** -luettelosta.
2.  Valitse **Toistuminen** **käynnistimien** -luettelosta. 
3.  Valitse **Toistuminen** -käynnistimen Valitse **Muokkaa**, **korkojakso** avattavasta Valitse **päivä**ja Aseta **aikavälin** tyyppi **7**.  
4.  Valitse **+ Uusi vaihe** -ruutuun ja valitse sitten **Lisää toiminnon**.
5.  Kirjoita **toimet** -luettelosta `db2` **Etsi lisää toimintoja** -ruutu ja valitse sitten **DB2 - Hae taulukot (esikatselu)**.

    ![](./media/connectors-create-api-db2/Db2connectorActions.png)  

6.  Määritysten **DB2 - Hae taulukot** -ruudussa **valintaruutu** Ota käyttöön valitsemalla **Yhdistä paikallisen tietojen yhdyskäytävän kautta**. Huomaa, että pilvestä paikallisen Muuta asetuksia.
    - Kirjoita **palvelimen**osoite tai alias kaksoispiste porttinumero lomake. Kirjoita esimerkiksi `ibmserver01:50000`.
    - Kirjoita **tietokannan**. Kirjoita esimerkiksi `nwind`.
    - Valitse arvo **todennusta**varten. Valitse esimerkiksi **Perustiedot**.
    - Kirjoita **käyttäjänimi**-arvo. Kirjoita esimerkiksi `db2admin`.
    - Kirjoita **salasana**. Kirjoita esimerkiksi `Password1`.
    - Valitse arvo **Gatewayn**. Valitse esimerkiksi **datagateway01**.
7. Valitse **Luo**ja valitse sitten **Tallenna**. 

    ![](./media/connectors-create-api-db2/Db2connectorOnPremisesDataGatewayConnection.png)

8.  Valitse **Db2getTables** -sivu, **kaikki suoritetaan** luettelossa, valitse **Yhteenveto**-luettelon ensimmäisestä-kohteen (uusin asennus).
9.  Valitse **Suorita tiedot** **logiikan sovelluksen Suorita** -sivu. Valitse **toiminto** -luettelossa **Get_tables**. Katso arvo **tila**, joiden pitäisi olla **onnistui**. Valitse Näytä syötteiden **syötteiden linkki** . Valitse **Luo linkki**ja tarkastella, tulostaa jossa projektityypin taulukkoluettelon.

    ![](./media/connectors-create-api-db2/Db2connectorGetTablesLogicAppRunOutputs.png)

## <a name="create-the-connections"></a>Yhteyksien luominen
Tämä yhdistin tukee yhteyksiä tietokantaa paikallisen ja pilvipalveluun käyttämällä seuraavia yhteyden ominaisuudet. 

Ominaisuus | Kuvaus
--- | ---
palvelin | Pakollinen. Hyväksyy merkkijonoarvo, joka edustaa TCP/IP-osoite tai alias IPv4 tai IPv6-muodossa, ja sen jälkeen (kaksoispiste erotetun) TCP/IP-portin numeroa. 
tietokannan | Pakollinen. Hyväksyy merkkijonoarvon, joka saadaan DRDA relaatio tietokannan nimi (RDBNAM). Z/OS DB2 hyväksyy 16-tavuinen merkkijono (tietokanta on nimeltään IBM DB2 z/OS sijainnin). I5/OS DB2 hyväksyy 18-Byte Character Set-merkkijono (tietokannan kutsutaan IBM DB2 i relaatio tietokannan). DB2 LUW hyväksyy 8-tavuinen merkkijonona.
todennus | Valinnainen. Hyväksyy luettelokohteen arvo, Basic- tai Windows (kerberos). 
käyttäjänimi | Pakollinen. Hyväksyy merkkijonoarvo. Z/OS DB2 hyväksyy 8-tavuinen merkkijonona. DB2, i hyväksyy 10-tavuinen merkkijonon. DB2 Linux tai UNIX hyväksyy 8-tavuinen merkkijonona. Windowsin DB2 hyväksyy 30-tavuinen merkkijonon.
salasana | Pakollinen. Hyväksyy merkkijonoarvo.
yhdyskäytävän | Pakollinen. Hyväksyy luettelokohteen arvo, edustava paikallisen tietojen yhdyskäytävän määritetty logiikan sovelluksiin tallennustilan ryhmän.  

## <a name="create-the-on-premises-gateway-connection"></a>Paikallisen-yhdyskäytävä-yhteyden luominen
Tämä yhdistin voit käyttää paikalliseen DB2-tietokantaan käyttämällä paikallisen yhdyskäytävän. Yhdyskäytävän ohjeaiheissa on lisätietoja. 

1. **Yhdyskäytävät** määritys-ruudussa **valintaruutu** Ota käyttöön valitsemalla **Yhdistä yhdyskäytävän kautta**. Huomaa, että pilvestä paikallisen Muuta asetuksia.
2. Kirjoita **palvelimen**osoite tai alias kaksoispiste porttinumero lomake. Kirjoita esimerkiksi `ibmserver01:50000`.
3. Kirjoita **tietokannan**. Kirjoita esimerkiksi `nwind`.
4. Valitse arvo **todennusta**varten. Valitse esimerkiksi **Perustiedot**.
5. Kirjoita **käyttäjänimi**-arvo. Kirjoita esimerkiksi `db2admin`.
6. Kirjoita **salasana**. Kirjoita esimerkiksi `Password1`.
7. Valitse arvo **Gatewayn**. Valitse esimerkiksi **datagateway01**.
8. Valitse **Luo** Jatka. 

    ![](./media/connectors-create-api-db2/Db2connectorOnPremisesDataGatewayConnection.png)

## <a name="create-the-cloud-connection"></a>Luo yhteys pilveen
Tämä yhdistin käyttää cloud DB2-tietokantaan. 

1. Jätä **valintaruutu** käytöstä määritysten **yhdyskäytävät** -ruudussa (napsauttamattomien) **Yhdistä yhdyskäytävän kautta**. 
2. Kirjoita arvo **yhteyden nimi**. Kirjoita esimerkiksi `hisdemo2`.
3. Kirjoita arvo **DB2 palvelimen nimi**-osoite tai alias kaksoispiste porttinumero muodossa. Kirjoita esimerkiksi `hisdemo2.cloudapp.net:50000`.
3. Kirjoita arvo **DB2 tietokannan nimi**. Kirjoita esimerkiksi `nwind`.
4. Kirjoita **käyttäjänimi**-arvo. Kirjoita esimerkiksi `db2admin`.
5. Kirjoita **salasana**. Kirjoita esimerkiksi `Password1`.
6. Valitse **Luo** Jatka. 

    ![](./media/connectors-create-api-db2/Db2connectorCloudConnection.png)

## <a name="fetch-all-rows-using-select"></a>Hae kaikki rivit SELECT-komennon käyttäminen
Voit määrittää logiikan-sovelluksen toiminto, joka noutaa DB2 taulukon kaikkien rivien. Tämän tiedon yhdistimen käsittelemään DB2 SELECT-lauseen, kuten `SELECT * FROM AREA`.

### <a name="create-a-logic-app"></a>Logiikan sovelluksen luominen
1.  Valitse **Azure Käynnistä taulun** **+** (plusmerkki) **Web + Mobile**ja sitten **Logiikan sovelluksen**.
2.  Kirjoita **nimi**, kuten `Db2getRows`- **tilaus**, **resurssiryhmä**, **sijainti**ja **Sovelluksen palvelun suunnitteleminen**. Valitse **Kiinnitä Raporttinäkymät-ikkunan**ja valitse sitten **Luo**.

### <a name="add-a-trigger-and-action"></a>Lisää käynnistin ja toiminto
1.  **Logiikan sovellusten suunnittelu**Valitse **Tyhjä LogicApp** **Mallit** -luettelosta.
2.  Valitse **Toistuminen** **käynnistimien** -luettelosta. 
3.  **Toistuminen** käynnistin Valitse **Muokkaa**, valitse **korkojakso** avattavasta Valitse **päivä**ja valitse sitten **aikavälin** tyyppi **7**. 
4.  Valitse **+ Uusi vaihe** -ruutuun ja valitse sitten **Lisää toiminnon**.
5.  Kirjoita **toimet** -luettelosta `db2` **Etsi lisää toimintoja** -ruutu ja valitse sitten **DB2 - Hae rivit (esikatselu)**.
6. Valitse **Hae rivit (esikatselu)** -toimintoa, **Muuta yhteys**.
7. Valitse **Luo uusi**määritys **yhteydet** -ruudussa. 

    ![](./media/connectors-create-api-db2/Db2connectorNewConnection.png)
  
8. Jätä **valintaruutu** käytöstä määritysten **yhdyskäytävät** -ruudussa (napsauttamattomien) **Yhdistä yhdyskäytävän kautta**.
    - Kirjoita arvo **yhteyden nimi**. Kirjoita esimerkiksi `HISDEMO2`.
    - Kirjoita arvo **DB2 palvelimen nimi**-osoite tai alias kaksoispiste porttinumero muodossa. Kirjoita esimerkiksi `HISDEMO2.cloudapp.net:50000`.
    - Kirjoita arvo **DB2 tietokannan nimi**. Kirjoita esimerkiksi `NWIND`.
    - Kirjoita **käyttäjänimi**-arvo. Kirjoita esimerkiksi `db2admin`.
    - Kirjoita **salasana**. Kirjoita esimerkiksi `Password1`.
9. Valitse **Luo** Jatka.

    ![](./media/connectors-create-api-db2/Db2connectorCloudConnection.png)

10. **Taulukkonimi** -luettelossa valitse **kohdan alanuolta**ja valitse sitten **alue**.
11. Voit myös valitsemalla **Näytä lisäasetukset** voit määrittää kyselyasetukset.
12. Valitse **Tallenna**. 

    ![](./media/connectors-create-api-db2/Db2connectorGetRowsTableName.png)

13. Valitse **Db2getRows** -sivu, **kaikki suoritetaan** luettelossa, valitse **Yhteenveto**-luettelon ensimmäisestä-kohteen (uusin asennus).
14. Valitse **Suorita tiedot** **logiikan sovelluksen Suorita** -sivu. Valitse **toiminto** -luettelossa **Get_rows**. Katso arvo **tila**, joiden pitäisi olla **onnistui**. Valitse Näytä syötteiden **syötteiden linkki** . Valitse **Luo linkki**ja tarkastella, tulostaa jossa pitäisi sisältää luettelon rivien määrä.

    ![](./media/connectors-create-api-db2/Db2connectorGetRowsOutputs.png)

## <a name="add-one-row-using-insert"></a>Käyttämällä Lisää yhden rivin lisääminen
Voit määrittää logiikan-sovelluksen toiminto, joka yhden rivin lisääminen DB2-taulukkoon. Tämä toiminto ohjaa yhdistimen käsittelemään DB2 Lisää ilmoituksen, kuten `INSERT INTO AREA (AREAID, AREADESC, REGIONID) VALUES ('99999', 'Area 99999', 102)`.

### <a name="create-a-logic-app"></a>Logiikan sovelluksen luominen
1.  Valitse **Azure Käynnistä taulun** **+** (plusmerkki) **Web + Mobile**ja sitten **Logiikan sovelluksen**.
2.  Kirjoita **nimi**, kuten `Db2insertRow`- **tilaus**, **resurssiryhmä**, **sijainti**ja **Sovelluksen palvelun suunnitteleminen**. Valitse **Kiinnitä Raporttinäkymät-ikkunan**ja valitse sitten **Luo**.

### <a name="add-a-trigger-and-action"></a>Lisää käynnistin ja toiminto
1.  **Logiikan sovellusten suunnittelu**Valitse **Tyhjä LogicApp** **Mallit** -luettelosta.
2.  Valitse **Toistuminen** **käynnistimien** -luettelosta. 
3.  **Toistuminen** käynnistin Valitse **Muokkaa**, valitse **korkojakso** avattavasta Valitse **päivä**ja valitse sitten **aikavälin** tyyppi **7**. 
4.  Valitse **+ Uusi vaihe** -ruutuun ja valitse sitten **Lisää toiminnon**.
5.  Valitse **Toiminnot** -luettelosta Kirjoita **db2** **Etsi lisää toimintoja** -tekstiruutuun ja valitse sitten **DB2 - Lisää rivi (ennakkoversio)**.
6. Valitse **Hae rivit (esikatselu)** -toimintoa, **Muuta yhteys**. 
7. **Yhteyksien** määrittäminen-ruudussa Valitse yhteys. Valitse esimerkiksi **hisdemo2**.

    ![](./media/connectors-create-api-db2/Db2connectorChangeConnection.png)

8. **Taulukkonimi** -luettelossa valitse **kohdan alanuolta**ja valitse sitten **alue**.
9. Kirjoita arvot kaikki pakolliset sarakkeet (Katso punainen tähti). Kirjoita esimerkiksi `99999` **AREAID**, kirjoita `Area 99999`, ja kirjoita `102` **REGIONID**varten. 
10. Valitse **Tallenna**.

    ![](./media/connectors-create-api-db2/Db2connectorInsertRowValues.png)
 
11. Valitse **Db2insertRow** -sivu, **kaikki suoritetaan** luettelossa, valitse **Yhteenveto**-luettelon ensimmäisestä-kohteen (uusin asennus).
12. Valitse **Suorita tiedot** **logiikan sovelluksen Suorita** -sivu. Valitse **toiminto** -luettelossa **Get_rows**. Katso arvo **tila**, joiden pitäisi olla **onnistui**. Valitse Näytä syötteiden **syötteiden linkki** . Valitse **Luo linkki**ja tarkastella, tulostaa jossa projektityypin uusi rivi.

    ![](./media/connectors-create-api-db2/Db2connectorInsertRowOutputs.png)

## <a name="fetch-one-row-using-select"></a>Nouda yhden rivin SELECT-komennon käyttäminen
Voit määrittää logiikan sovelluksen-toiminto, joka noutaa yhden rivin DB2-taulukossa. Tämä toiminto ohjaa yhdistimen käsittelemään missä DB2 Valitse ilmoitus siitä, kuten `SELECT FROM AREA WHERE AREAID = '99999'`.

### <a name="create-a-logic-app"></a>Logiikan sovelluksen luominen
1.  Valitse **Azure Käynnistä taulun** **+** (plusmerkki) **Web + Mobile**ja sitten **Logiikan sovelluksen**.
2.  Anna **nimi** (esimerkiksi "**Db2getRow**"), **tilauksen**, **resurssiryhmä**, **sijainti**ja **Sovelluksen palvelun suunnitteleminen**. Valitse **Kiinnitä Raporttinäkymät-ikkunan**ja valitse sitten **Luo**.

### <a name="add-a-trigger-and-action"></a>Lisää käynnistin ja toiminto
1.  **Logiikan sovellusten suunnittelu**Valitse **Tyhjä LogicApp** **Mallit** -luettelosta. 
2.  Valitse **Toistuminen** **käynnistimien** -luettelosta. 
3.  **Toistuminen** käynnistin Valitse **Muokkaa**, valitse **korkojakso** avattavasta Valitse **päivä**ja valitse sitten **aikavälin** tyyppi **7**. 
4.  Valitse **+ Uusi vaihe** -ruutuun ja valitse sitten **Lisää toiminnon**.
5.  Valitse **Toiminnot** -luettelosta Kirjoita **db2** **Etsi lisää toimintoja** -tekstiruutuun ja valitse sitten **DB2 - Hae rivit (ennakkoversio)**.
6. Valitse **Hae rivit (esikatselu)** -toimintoa, **Muuta yhteys**. 
7. Valitse olemassa oleva yhteys käyttömahdollisuudet **yhteydet** -ruudussa. Valitse esimerkiksi **hisdemo2**.

    ![](./media/connectors-create-api-db2/Db2connectorChangeConnection.png)

8. **Taulukkonimi** -luettelossa valitse **kohdan alanuolta**ja valitse sitten **alue**.
9. Kirjoita arvot kaikki pakolliset sarakkeet (Katso punainen tähti). Kirjoita esimerkiksi `99999` **AREAID**varten. 
10. Voit myös valitsemalla **Näytä lisäasetukset** voit määrittää kyselyasetukset.
11. Valitse **Tallenna**. 

    ![](./media/connectors-create-api-db2/Db2connectorGetRowValues.png)

12. Valitse **Db2getRow** -sivu, **kaikki suoritetaan** luettelossa, valitse **Yhteenveto**-luettelon ensimmäisestä-kohteen (uusin asennus).
13. Valitse **Suorita tiedot** **logiikan sovelluksen Suorita** -sivu. Valitse **toiminto** -luettelossa **Get_rows**. Katso arvo **tila**, joiden pitäisi olla **onnistui**. Valitse Näytä syötteiden **syötteiden linkki** . Valitse **Luo linkki**ja tarkastella, tulostaa jossa projektityypin rivi.

    ![](./media/connectors-create-api-db2/Db2connectorGetRowOutputs.png)

## <a name="change-one-row-using-update"></a>Muuta yhden rivin UPDATEN avulla
Voit määrittää logiikan app-toimintoa, voit muuttaa yhden rivin DB2-taulukossa. Tämä toiminto ohjaa yhdistimen käsittelemään DB2 UPDATE-komento, kuten `UPDATE AREA SET AREAID = '99999', AREADESC = 'Area 99999', REGIONID = 102)`.

### <a name="create-a-logic-app"></a>Logiikan sovelluksen luominen
1.  Valitse **Azure Käynnistä taulun** **+** (plusmerkki) **Web + Mobile**ja sitten **Logiikan sovelluksen**.
2.  Kirjoita **nimi**, kuten `Db2updateRow`- **tilaus**, **resurssiryhmä**, **sijainti**ja **Sovelluksen palvelun suunnitteleminen**. Valitse **Kiinnitä Raporttinäkymät-ikkunan**ja valitse sitten **Luo**.

### <a name="add-a-trigger-and-action"></a>Lisää käynnistin ja toiminto
1.  **Logiikan sovellusten suunnittelu**Valitse **Tyhjä LogicApp** **Mallit** -luettelosta.
2.  Valitse **Toistuminen** **käynnistimien** -luettelosta. 
3.  **Toistuminen** käynnistin Valitse **Muokkaa**, valitse **korkojakso** avattavasta Valitse **päivä**ja valitse sitten **aikavälin** tyyppi **7**. 
4.  Valitse **+ Uusi vaihe** -ruutuun ja valitse sitten **Lisää toiminnon**.
5.  Valitse **Toiminnot** -luettelosta Kirjoita **db2** **Etsi lisää toimintoja** -tekstiruutuun ja valitse sitten **DB2 - rivillä Update (ennakkoversio)**.
6. Valitse **Hae rivit (esikatselu)** -toimintoa, **Muuta yhteys**. 
7. Valitse **yhteydet** määritykset-ruudussa aiemmin luotua yhteyttä. Valitse esimerkiksi **hisdemo2**.

    ![](./media/connectors-create-api-db2/Db2connectorChangeConnection.png)

8. **Taulukkonimi** -luettelossa valitse **kohdan alanuolta**ja valitse sitten **alue**.
9. Kirjoita arvot kaikki pakolliset sarakkeet (Katso punainen tähti). Kirjoita esimerkiksi `99999` **AREAID**, kirjoita `Updated 99999`, ja kirjoita `102` **REGIONID**varten. 
10. Valitse **Tallenna**. 

    ![](./media/connectors-create-api-db2/Db2connectorUpdateRowValues.png)

11. Valitse **Db2updateRow** -sivu, **kaikki suoritetaan** luettelossa, valitse **Yhteenveto**-luettelon ensimmäisestä-kohteen (uusin asennus).
12. Valitse **Suorita tiedot** **logiikan sovelluksen Suorita** -sivu. Valitse **toiminto** -luettelossa **Get_rows**. Katso arvo **tila**, joiden pitäisi olla **onnistui**. Valitse Näytä syötteiden **syötteiden linkki** . Valitse **Luo linkki**ja tarkastella, tulostaa jossa projektityypin uusi rivi.

    ![](./media/connectors-create-api-db2/Db2connectorUpdateRowOutputs.png)

## <a name="remove-one-row-using-delete"></a>Poista käyttämällä yhden rivin poistaminen
Voit määrittää logiikan-sovelluksen toiminto, joka poistaa yhden rivin DB2 taulukon. Tämä toiminto ohjaa yhdistimen käsittelemään DB2 DELETE-lauseen, kuten `DELETE FROM AREA WHERE AREAID = '99999'`.

### <a name="create-a-logic-app"></a>Logiikan sovelluksen luominen
1.  Valitse **Azure Käynnistä taulun** **+** (plusmerkki) **Web + Mobile**ja sitten **Logiikan sovelluksen**.
2.  Kirjoita **nimi**, kuten `Db2deleteRow`- **tilaus**, **resurssiryhmä**, **sijainti**ja **Sovelluksen palvelun suunnitteleminen**. Valitse **Kiinnitä Raporttinäkymät-ikkunan**ja valitse sitten **Luo**.

### <a name="add-a-trigger-and-action"></a>Lisää käynnistin ja toiminto
1.  **Logiikan sovellusten suunnittelu**Valitse **Tyhjä LogicApp** **Mallit** -luettelosta. 
2.  Valitse **Toistuminen** **käynnistimien** -luettelosta. 
3.  **Toistuminen** käynnistin Valitse **Muokkaa**, valitse **korkojakso** avattavasta Valitse **päivä**ja valitse sitten **aikavälin** tyyppi **7**. 
4.  Valitse **+ Uusi vaihe** -ruutuun ja valitse sitten **Lisää toiminnon**.
5.  Valitse **toimet** -luettelosta **Etsi lisää toimintoja** -muokkausruutuun **db2** ja valitse **DB2 - Poista rivi (ennakkoversio)**.
6. Valitse **Hae rivit (esikatselu)** -toimintoa, **Muuta yhteys**. 
7. Valitse olemassa oleva yhteys käyttömahdollisuudet **yhteydet** -ruudussa. Valitse esimerkiksi **hisdemo2**.

    ![](./media/connectors-create-api-db2/Db2connectorChangeConnection.png)

8. **Taulukkonimi** -luettelossa valitse **kohdan alanuolta**ja valitse sitten **alue**.
9. Kirjoita arvot kaikki pakolliset sarakkeet (Katso punainen tähti). Kirjoita esimerkiksi `99999` **AREAID**varten. 
10. Valitse **Tallenna**. 

    ![](./media/connectors-create-api-db2/Db2connectorDeleteRowValues.png)

11. Valitsee luettelon ensimmäisestä-kohteen (uusin asennus) **Db2deleteRow** -sivu **Yhteenveto**-kohdassa **kaikki suoritetaan** -luettelossa.
12. Valitse **Suorita tiedot** **logiikan sovelluksen Suorita** -sivu. Valitse **toiminto** -luettelossa **Get_rows**. Katso arvo **tila**, joiden pitäisi olla **onnistui**. Valitse Näytä syötteiden **syötteiden linkki** . Valitse **Luo linkki**ja tarkastella, tulostaa jossa projektityypin poistettu rivi.

    ![](./media/connectors-create-api-db2/Db2connectorDeleteRowOutputs.png)

## <a name="technical-details"></a>Teknisiä tietoja

## <a name="actions"></a>Toiminnot
Toiminto on määritetty logiikan-sovelluksessa työnkulun suorittamiin toiminto. DB2-tietokantayhteyden sisältää seuraavat toimet. 

|Toiminto|Kuvaus|
|--- | ---|
|[GetRow](connectors-create-api-db2.md#get-row)|Yhden rivin hakee DB2-taulukosta|
|[GetRows](connectors-create-api-db2.md#get-rows)|Hakee DB2-taulukosta rivit|
|[InsertRow](connectors-create-api-db2.md#insert-row)|Lisää uuden rivin DB2 lisääminen taulukkoon|
|[DeleteRow](connectors-create-api-db2.md#delete-row)|Poistaa rivin DB2-taulukosta|
|[GetTables](connectors-create-api-db2.md#get-tables)|Taulukoiden hakee DB2-tietokantaan|
|[UpdateRow](connectors-create-api-db2.md#update-row)|Päivittää aiemmin määritetty DB2 taulukon rivi|

### <a name="action-details"></a>Toiminnon tiedot

Tässä osassa on tarkkoja tietoja kunkin toiminnon, mukaan lukien kaikki pakolliset ja valinnaiset syötteen ominaisuudet ja yhdistimen liittyvät vastaavan tuloksen.

#### <a name="get-row"></a>Hae rivi 
Hakee yksirivisen DB2-taulukosta.  

| Ominaisuuden nimi| Näyttönimi |Kuvaus|
| ---|---|---|
|taulukon * | Taulukkonimi |DB2 taulukon nimi|
|tunnus * | Rivin tunnus |Yksilöllinen tunnus rivin noutaminen|

Tähtimerkkiä (*) tarkoittaa, että ominaisuus on pakollinen.

##### <a name="output-details"></a>Tulostustiedot
Kohteen

| Ominaisuuden nimi | Tietotyyppi |
|---|---|
|ItemInternalId|merkkijono|


#### <a name="get-rows"></a>Hae rivit 
Hakee DB2 taulukon rivejä.  

|Ominaisuuden nimi| Näyttönimi|Kuvaus|
| ---|---|---|
|taulukon *|Taulukkonimi|DB2 taulukon nimi|
|$skip|Ohita määrä|Voit ohittaa merkintöjen määrä (oletus = 0)|
|$top|Suurin Get määrä|Noutaa kohteiden enimmäismäärä (oletus = 256)|
|$filter|Voit suodattaa kyselyn|ODATA suodatinkyselyn, joka rajoittaa tapahtumien määrä|
|$orderby|Lajittelu|ODATA lajittelu-kyselyn argumenteille tapahtumien järjestys|

Tähtimerkkiä (*) tarkoittaa, että ominaisuus on pakollinen.

##### <a name="output-details"></a>Tulostustiedot
ItemsList

| Ominaisuuden nimi | Tietotyyppi |
|---|---|
|arvo|matriisi|


#### <a name="insert-row"></a>Lisää rivi 
Lisää uuden rivin DB2-taulukkoon.  

|Ominaisuuden nimi| Näyttönimi|Kuvaus|
| ---|---|---|
|taulukon *|Taulukkonimi|DB2 taulukon nimi|
|kohteen *|Rivin|Voit lisätä DB2 määritetyn taulukon rivi|

Tähtimerkkiä (*) tarkoittaa, että ominaisuus on pakollinen.

##### <a name="output-details"></a>Tulostustiedot
Kohteen

| Ominaisuuden nimi | Tietotyyppi |
|---|---|
|ItemInternalId|merkkijono|


#### <a name="delete-row"></a>Poista rivi 
Poistaa rivin DB2-taulukosta.  

|Ominaisuuden nimi| Näyttönimi|Kuvaus|
| ---|---|---|
|taulukon *|Taulukkonimi|DB2 taulukon nimi|
|tunnus *|Rivin tunnus|Yksilöllinen tunnus, jos haluat poistaa rivin|

Tähtimerkkiä (*) tarkoittaa, että ominaisuus on pakollinen.

##### <a name="output-details"></a>Tulostustiedot
Ei mitään.

#### <a name="get-tables"></a>Hae taulukot 
Hakee taulukoiden DB2-tietokantaan.  

Ei ole parametreja puhelun. 

##### <a name="output-details"></a>Tulostustiedot 
TablesList

| Ominaisuuden nimi | Tietotyyppi |
|---|---|
|arvo|matriisi|

#### <a name="update-row"></a>Päivitä rivi 
Päivittää olemassa olevaa riviä DB2-taulukossa.  

|Ominaisuuden nimi| Näyttönimi|Kuvaus|
| ---|---|---|
|taulukon *|Taulukkonimi|DB2 taulukon nimi|
|tunnus *|Rivin tunnus|Yksilöllinen tunnus, Päivitä rivi|
|kohteen *|Rivin|Rivi, jonka päivitetyt arvot|

Tähtimerkkiä (*) tarkoittaa, että ominaisuus on pakollinen.

##### <a name="output-details"></a>Tulostustiedot  
Kohteen

| Ominaisuuden nimi | Tietotyyppi |
|---|---|
|ItemInternalId|merkkijono|


### <a name="http-responses"></a>HTTP-vastaukset

Kun kutsut erilaisia toimintoja, näkyviin voi tulla tiettyjä vastaukset. Seuraavassa taulukossa kuvataan vastaukset ja niiden kuvaukset:  

|Nimi|Kuvaus|
|---|---|
|200|Okei|
|202|Hyväksytty|
|400|Pyyntö ei kelpaa|
|401|Luvattomia|
|403|Käyttö estetty|
|404|Ei löydy|
|500|Sisäinen palvelinvirhe. Tuntematon virhe|
|Oletusarvo|Epäonnistui.|


## <a name="supported-db2-platforms-and-versions"></a>Tuetut DB2 alustojen ja eri versiot
Tämä yhdistin tukee seuraavissa IBM DB2-ympäristöissä ja versiot sekä IBM DB2 yhteensopiva tuotteiden (kuten IBM Bluemix dashDB), jotka tukevat Distributed relaatio tietokannan arkkitehtuuri (DRDA) SQL Access Manager (SQLAM) 10 ja 11:

- IBM DB2 z/OS 11.1Kytkentälaitteen(-laitteiden) varten
- IBM DB2 z/OS 10.1 varten
- IBM DB2, i 7.3
- IBM DB2, i 7.2
- IBM DB2, i 7.1
- IBM DB2 LUW 11
- IBM DB2 LUW 10.5 varten


## <a name="next-steps"></a>Seuraavat vaiheet

[Logiikan-sovelluksen luominen](../app-service-logic/app-service-logic-create-a-logic-app.md). Tutustu muiden käytettävissä yhdistimet logiikan sovellusten etsiminen [API-luettelosta](apis-list.md).

