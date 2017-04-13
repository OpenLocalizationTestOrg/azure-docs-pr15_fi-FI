<properties
   pageTitle="SQL Data Warehouse uhkien tunnistus käytön aloittaminen"
   description="Miten uhkien tunnistus käytön aloittaminen"
   services="sql-data-warehouse"
   documentationCenter=""
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/24/2016"
   ms.author="lodipalm;sonyama;barbkess"/>


# <a name="get-started-with-threat-detection"></a>Uhkien tunnistus käytön aloittaminen

> [AZURE.SELECTOR]
- [Valvonta](sql-data-warehouse-auditing-overview.md)
- [Uhkien tunnistus](sql-data-warehouse-security-threat-detection.md)

## <a name="overview"></a>Yleiskatsaus

Uhkien tunnistus havaitsee erheellisiin tietokannan toimintaa, joka osoittaa mahdolliset suojauksen uhkien tietokantaan. Uhkien tunnistus on esikatselu ja SQL-tietovarasto tuetaan.

Uhkien tunnistus tarjoaa uuden kerroksen suojauksen, jonka avulla käyttäjä voi tunnistaa ja viesteihin vastaaminen mahdollisten uhkien ilmenee antamalla suojausvaroitusten erheellisiin toimiin. Käyttäjien tutkia epäilyttävistä tapahtumien avulla voit selvittää, jos ne johtuvat yritys käyttää, rikkonut tai hyödyntää tietojen tietovarasto [Azure SQL tietojen varaston seuranta](sql-data-warehouse-auditing-overview.md) .
Uhkien tunnistus on yksinkertainen, osoite mahdollisten uhkien tietovarasto ilman, että arvopaperille asiantuntija tai Lisäasetukset seuranta järjestelmien tietoturvan hallintaa.

Esimerkiksi uhkien tunnistus tunnistaa, joka osoittaa mahdolliset SQL lisäämisen yritykset erheellisiin tietokannan tiettyjen toimintojen. SQL-lisäämisen on yleisiä Web application suojausongelmia Internetissä käytettävä hyökkäyksille tietopohjaisten. Hyökkääjät hyödyntää sovelluksen heikkouksien lisätäkseen haittaohjelmien SQL-lauseet sovelluksen tapahtuma kenttiin, jolla tai muokkaamalla tietokanta.


## <a name="set-up-threat-detection-for-your-database"></a>Määritä tietokannan uhkien tunnistus

1. Käynnistä Azure-portaalissa osoitteessa [https://portal.azure.com](https://portal.azure.com).

2. Siirry SQL-tietovarasto, joita haluat seurata määritys-sivu. Valitse asetukset-sivu **valvonta ja uhkien tunnistus**.

    ![Siirtymisruutu][1]

3. Poista **edelleen** valvonta, joka näyttää uhkien tunnistus asetusten **tarkistaminen ja uhkien tunnistus** määritys-sivu.

    ![Siirtymisruutu][2]

4. Ota **edelleen** uhkien tunnistus.

5. Määritä sähköpostit, joka vastaanottaa suojausvaroitusten tunnistus erheellisiin data warehouse toiminnan yhteydessä luettelo.

6. Valitse Tallenna uusi tai päivitetty tarkistaminen ja uhkien tunnistus käytännön **valvonta ja uhkien tunnistus** määritys-sivu valitsemalla **Tallenna** .

    ![Siirtymisruutu][3]


## <a name="explore-anomalous-data-warehouse-activities-upon-detection-of-a-suspicious-event"></a>Tutustu erheellisiin tietojen varastotoimintojen yhteydessä epäilyttävistä tapahtuman tunnistus

1. Saat siitä sähköposti-ilmoituksen yhteydessä tunnistus erheellisiin tietokannan toiminnan. <br/>
Sähköpostin antaa tietoja epäilyttävistä Suojaustapahtuma, mukaan lukien erheellisiin toimintoja, tietokannan nimi, palvelimen nimi ja tapahtuma-aika. Lisäksi se antaa tietoja mahdollisia syitä ja suositeltuja toimia tutkia ja pienentämään mahdollisten tietokantaan.<br/>

    ![Siirtymisruutu][4]

2. Napsauta sähköposti- **Azure SQL-tarkistaminen lokin** linkkiä, joka Käynnistä Azure perinteinen portaalin ja näyttää haluamasi valvonta tietueet, epäilyttävistä tapahtuma-ajan.

    ![Siirtymisruutu][5]

3. Voit tarkastella lisätietoja epäilyttävien tietokanta-toimintoja, kuten SQL-lause valvontatiedot valitsemalla virheen syy ja asiakkaan IP.

    ![Siirtymisruutu][6]

4. Valvonta-tietueet-sivu valitsemalla **Avaa Excelissä** Avaa valmiiksi määritetyn excel-malli ja suorita myyntilukujen valvontaloki epäilyttävistä tapahtuma-ajan mukaan.<br/>
**Huomautus:** Excel 2010: n tai sitä uudempi versio Power Query- ja **Nopea yhdistäminen** -asetusta tarvitaan

    ![Siirtymisruutu][7]

5. Voit määrittää **POWER QUERY** -valintanauhassa **Nopea yhdistäminen** - asetuksen, valitse Näytä asetukset-valintaikkunan **asetukset** . Valitse tietosuoja-osa ja valitse toinen vaihtoehto - Ohita yksityisyystasot ja mahdollisesti suorituskyvyssä:

    ![Siirtymisruutu][8]

6. Lataa SQL valvontalokien, että parametrit asetuksia-välilehti on määritetty oikein ja valitse sitten valintanauhan 'Tiedot' ja 'Päivitä kaikki-painiketta.

    ![Siirtymisruutu][9]

7. Tulokset näkyvät **SQL valvontalokien** taulukon, jonka avulla voit suorittaa myyntilukujen mukaan erheellisiin toimintoja, jotka havaittiin ja rajoittaa sovelluksen suojaus-tapahtuman vaikutus.


<!--Image references-->
[1]: ./media/sql-data-warehouse-security-threat-detection/1_td_click_on_settings.png
[2]: ./media/sql-data-warehouse-security-threat-detection/2_td_turn_on_auditing.png
[3]: ./media/sql-data-warehouse-security-threat-detection/3_td_turn_on_threat_detection.png
[4]: ./media/sql-data-warehouse-security-threat-detection/4_td_email.png
[5]: ./media/sql-data-warehouse-security-threat-detection/5_td_audit_records.png
[6]: ./media/sql-data-warehouse-security-threat-detection/6_td_audit_record_details.png
[7]: ./media/sql-data-warehouse-security-threat-detection/7_td_audit_records_open_excel.png
[8]: ./media/sql-data-warehouse-security-threat-detection/8_td_excel_fast_combine.png
[9]: ./media/sql-data-warehouse-security-threat-detection/9_td_excel_parameters.png
