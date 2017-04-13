<properties 
    pageTitle="Luominen ja palauttaminen varmuuskopiosta BizTalk Services-palveluissa | Microsoft Azure" 
    description="BizTalk Services sisältää varmuuskopiointi ja palauttaminen. Lue, miten voit luoda ja palauttaminen varmuuskopiosta ja selvittää, mitä saa varmuuskopioida. MAB-WABS" 
    services="biztalk-services" 
    documentationCenter="" 
    authors="MandiOhlinger" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="biztalk-services" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="mandia"/>


# <a name="biztalk-services-backup-and-restore"></a>BizTalk Services: Varmuuskopiointi ja palauttaminen

Azure BizTalk Services sisältää varmuuskopiointi ja palauttaminen. Tässä ohjeaiheessa kerrotaan, miten varmuuskopioiminen ja palauttaminen BizTalk Services Azure perinteinen-portaalissa.

Voit myös varmuuskopioida BizTalk-palveluita käyttämällä [BizTalk Services REST API](http://go.microsoft.com/fwlink/p/?LinkID=325584). 

> [AZURE.NOTE] Hybrid yhteydet ei varmuuskopioidaan, riippumatta siitä, Edition. Sinun on luotava uudelleen hybrid yhteydet.

## <a name="before-you-begin"></a>Ennen aloittamista

- Varmuuskopiointi ja palauttaminen ei ole käytettävissä kaikki versiot. Katso [BizTalk Services: versiot kaavion](biztalk-editions-feature-chart.md).

- Käytä Azure perinteinen portaalin, voit luoda pyydettäessä varmuuskopiointi tai ajoitetun varmuuskopion luominen. 

- Varmuuskopion sisältöä voi palauttaa saman BizTalk-palveluun tai BizTalk-palveluun. Jos haluat palauttaa BizTalk-palvelu on sama nimi, nykyinen BizTalk-palvelu on poistettava ja nimen on oltava käytettävissä. Kun olet poistanut BizTalk palvelu, voi kestää kauemmin kuin haluaisit käyttää voi käyttää samaa nimeä. Jos odotat ei voi käyttää nimi, valitse Palauta uusi BizTalk-palvelu.

- BizTalk Services voi palauttaa saman tai uudempi versio edition. Palauttaminen BizTalk Services alemman edition, kun varmuuskopio on tehty, valitse ei tueta.

    Esimerkiksi käyttämällä perusversio varmuuskopion voidaan palauttaa mihin Premium-versioon. Varmuuskopion Premium-versioon ei voi palauttaa Standard Edition.

- Muokkaa ohjausobjektin numerot varmuuskopioidaan jatkuvuuden ohjausobjektin lukuja. Jos viestit käsitellään varmuuskopion jälkeen, palauttaminen varmuuskopiosta sisällön voi aiheuttaa kaksoiskappaleiden ohjausobjektin numeroita.

- Jos erä on aktiivinen viestejä, käsitellä erä **ennen** varmuuskopiointi. Luotaessa varmuuskopion (kuten tarvitaan tai ajoitetun) erissä viestit tallennetaan koskaan. 

    **Jos varmuuskopio on aktiivinen viesteillä erän, kyseisiä viestejä ei varmuuskopioida ja osia katoaa.**

- Valinnainen: BizTalk palvelut-portaalissa lopettaa kaikki hallintatoiminnot.


## <a name="create-a-backup"></a>Luo varmuuskopio

Varmuuskopion voidaan ottaa milloin tahansa ja voit kokonaan ohjaa. Tässä jaksossa kerrotaan vaihe vaiheelta, miten varmuuskopioita Azure perinteinen-portaalissa, mukaan lukien:

[Valitse tarvittaessa varmuuskopiointi](#backupnow)

[Aikataulun varmuuskopiointi](#backupschedule)

#### <a name="backupnow"></a>Valitse tarvittaessa varmuuskopiointi
1. Azure perinteinen-portaalissa, valitse **BizTalk palvelut**ja valitse sitten BizTalk palvelun haluat varmuuskopioida.
2. Valitse sivun alareunassa **varmuuskopioida** **koontinäyttö** -välilehti.
3. Kirjoita varmuuskopion nimi. Kirjoita esimerkiksi *myBizTalkService*BU*päivämäärä*.
4. Blob storage-tili ja valitse Aloita varmuuskopiointi valintamerkki.

Varmuuskopioinnin, kun kirjoitat varmuuskopio nimellä säilön luodaan tallennustilan tilin. Tämä säilö sisältää BizTalk palvelun varmuuskopioinnin määritykset.

#### <a name="backupschedule"></a>Aikataulun varmuuskopiointi

1. Azure perinteinen-portaalissa Valitse **BizTalk palvelut**, valitse haluamasi varmuuskopioinnin BizTalk palvelun nimi ja valitse sitten **määrittäminen** -välilehti.
2. Määritä **automaattisen** **varmuuskopioinnin tila** . 
3. **Tallennustilan tili** , jota haluat tallentaa varmuuskopion, kirjoita varmuuskopioita, **korkojakso** ja kuinka kauan Säilytä varmuuskopiot (**Säilytys päivää**):

    ![][AutomaticBU]

    **Huomautuksia**   
    - **Säilytys päivän**säilytysaika on oltava suurempi kuin varmuuskopion korkojakso.
    - Valitse **vähintään yksi varmuuskopio aina pitää**, vaikka se on jo mennyt säilytysaika.
    

4. Valitse **Tallenna**.


Kun ajoitetun varmuuskopiointityön suoritetaan, se luo kirjoittamasi tallennustilan tilin säilön (tallentamiseen varmuuskopiotiedot). Säilön nimi on nimeltään *BizTalk palvelun nimi-päivämäärän ja kellonajan*. 

Jos BizTalk palvelun Raporttinäkymät-ikkunan näkyy **epäonnistui** -tila:

![Viimeksi ajoitettu varmuuskopioinnin tila][BackupStatus] 

Linkki avaa ongelmien Management Services toiminto-lokit. Katso [BizTalk Services: vianmääritystä toimintojen lokit](http://go.microsoft.com/fwlink/p/?LinkId=391211).

## <a name="restore"></a>Palauttaminen

Voit palauttaa varmuuskopioiden perinteinen Azure-portaalista tai [Palauta BizTalk palvelun REST API](http://go.microsoft.com/fwlink/p/?LinkID=325582). Tässä jaksossa kerrotaan vaihe vaiheelta, miten palauttaa perinteinen-portaalissa.

#### <a name="before-restoring-a-backup"></a>Ennen kuin palauttaminen varmuuskopiosta

- Aikana BizTalk-palvelun palautus voidaan kirjoittaa uuden seuranta, arkistointi ja seuranta stores.

- Muokkaa Runtime samat tiedot palautetaan. Muokkaa Runtime varmuuskopioinnin tallentaa ohjausobjektin luvut. Palautetun ohjausobjektin ovat järjestyksessä varmuuskopioinnin aikana. Jos viestit käsitellään varmuuskopion jälkeen, palauttaminen varmuuskopiosta sisällön voi aiheuttaa kaksoiskappaleiden ohjausobjektin numeroita.

#### <a name="restore-a-backup"></a>Palauta varmuuskopio

1. Azure perinteinen-portaalissa, valitse **Uusi** > **Sovelluksen Services** > **BizTalk palvelun** > **palauttaa**:

    ![Palauta varmuuskopio][Restore]

2. **Varmuuskopion URL-osoite**Valitse kansiokuvake ja laajenna Azure-tallennustilan tilin, joka tallentaa BizTalk palvelun määritysten varmuuskopiointi. Laajenna kansio ja valitse oikeanpuoleisessa ruudussa vastaavan Varmuuskopioi .txt-tiedosto. 
<br/><br/>
Valitse **Avaa**.

3. **BizTalk palauttamisen** sivulla **BizTalk palvelunimi** ja varmista **Toimialueen URL-osoite**-, **vuosi**- ja **alueasetusten** palautettu BizTalk-palveluun. **Luo uusi SQL-tietokannan esiintymä** seuranta-tietokantaan:

    ![][RestoreBizTalkService]

    Valitse seuraava-nuoli.

4.  Tarkista SQL-tietokannan nimeä, kirjoita palvelimen fyysinen palvelin, johon SQL-tietokantaan luodaan ja käyttäjänimellä ja salasanalla.


    Jos haluat määrittää SQL-tietokanta-version, kokoa ja muita ominaisuuksia, valitse **Lisäasetukset tietokannan asetusten määrittäminen**. 

    Valitse seuraava-nuoli.

5. Luo uusi tallennustilan-tili tai kirjoita käytössä olevan tallennustilan tilin BizTalk-palveluun.

7. Valitse Aloita Palauta valintamerkki.

Kun palauttaminen onnistuu BizTalk uusi palvelu näkyy keskeytystilassa BizTalk palvelut-sivulla Azure perinteinen portaalissa.



### <a name="postrestore"></a>Kun olet palauttaminen varmuuskopiosta

BizTalk-palvelun palautetaan aina **Suspended** -tilaan. Tässä vaiheessa voit tehdä muutoksia määritysten ennen uuteen ympäristöön toimivuuden, mukaan lukien:

- Jos olet luonut BizTalk Palvelusovellusten Azure BizTalk Services SDK: N avulla, joudut ehkä päivittää Access-ohjausobjektin (ACS) tunnistetiedot näiden sovellusten palautettu ympäristön käyttöä varten.

- Voit palauttaa BizTalk palvelu replikointia aiemmin BizTalk Service-ympäristössä. Jos on määritetty alkuperäinen BizTalk palvelut-portaalissa toimeenpano, jotka käyttävät lähde FTP-kansion voit joutua tässä tilanteessa päivittämään sopimusten juuri palautettu ympäristössä käyttämään eri tietolähteen FTP-kansio. Muussa tapauksessa voi olla kaksi eri sopimusten yrität tuoda saman viestin.

- Jos palautettu on useita BizTalk Service-ympäristössä, varmista, että Visual Studio sovellukset, PowerShellin cmdlet-komennot, REST API tai myynti kumppanin hallinta OM-Ohjelmointirajapinnat oikeat ympäristön kohdeluettelo.

- Se on hyvä automaattisen varmuuskopioinnin määrittämiseksi juuri palautettu BizTalk Service-ympäristössä.

Voit käynnistää BizTalk-palvelun Azure perinteinen portaalissa palautettu BizTalk-palvelu ja valitse **Jatka** tehtäväpalkista. 



## <a name="what-gets-backed-up"></a>Mitä sivustomalliin varmuuskopioida

Varmuuskopion luomisen jälkeen seuraavat kohteet varmuuskopioidaan:

<table border="1"> 
<tr bgcolor="FAF9F9">
<th> </th>
<TH>Varmuuskopioida kohteet</TH> 
</tr> 
<tr>
<td colspan="2">
 <strong>Azure BizTalk palveluiden portaali</strong></td>
</tr> 
<tr>
<td>Määritys ja Runtime</td> 
<td>
<ul>
<li>Kumppanin ja profiilin tiedot</li>
<li>Kumppanin toimeenpano</li>
<li>Mukautetut kokoonpanot käyttöön</li>
<li>Siltoja, jotka on otettu käyttöön</li>
<li>Varmenteet</li>
<li>Muunnokset, jotka on otettu käyttöön</li>
<li>Putkistot</li>
<li>Mallit luodaan ja tallennetaan BizTalk palveluiden portaali</li>
<li>X12 ST01 ja GS01 yhdistämismääritysten</li>
<li>Ohjausobjektin numerot (Muokkaa)</li>
<li>AS2 viestin Mikrofoni arvot</li>
</ul>
</td>
</tr> 
 
<tr>
<td colspan="2">
 <strong>Azure BizTalk-palvelu</strong></td>
</tr> 
<tr>
<td>SSL-varmenne</td> 
<td>
<ul>
<li>SSL-varmenteen tiedot</li>
<li>SSL-varmenteen salasana</li>
</ul>
</td>
</tr> 
<tr>
<td>BizTalk palvelun asetukset</td> 
<td>
<ul>
<li>Asteikko yksikön määrä</li>
<li>Edition</li>
<li>Tuoteversio</li>
<li>Alue/Palvelinkeskukseen</li>
<li>Access-ohjausobjektin Service (ACS) nimitilan ja avaimen avulla</li>
<li>Tietokannan yhteysmerkkijono seuranta</li>
<li>Tallennustilan tilin yhteysmerkkijonon arkistointi</li>
<li>Tallennustilan tilin yhteysmerkkijonon seuranta</li>
</ul>
</td>
</tr> 
<tr>
<td colspan="2">
 <strong>Muut kohteet</strong></td>
</tr> 
<tr>
<td>Seurantatietokanta</td> 
<td>BizTalk-palvelun luomisen jälkeen seuranta tietokannan tiedot on syötetty, mukaan lukien Azure SQL-tietokantaan ja seuranta tietokannan nimi. Seuranta-tietokantaan ei automaattisesti varmuuskopioida.
<br/><br/>
<strong>Tärkeää</strong><br/>
Jos tietokanta on palautettu seuranta-tietokanta on poistettu, on oltava olemassa varmuuskopio. Jos varmuuskopion ei ole, seuranta-tietokannan ja sen tiedot eivät ole palautettavissa. Tässä tilanteessa tietokannan samannimistä uusi seuranta-tietokannan luominen. GEO replikoinnin suositellaan.</td>
</tr> 
</table>

## <a name="next"></a>Seuraava

Siirry Azure BizTalk Services luominen Azure perinteinen portaalin, [BizTalk Services: valmistelu käyttämällä Azure perinteinen portal](http://go.microsoft.com/fwlink/p/?LinkID=302280). Aloita sovellusten luominen, siirry [Azure BizTalk palvelut](http://go.microsoft.com/fwlink/p/?LinkID=235197).

## <a name="see-also"></a>Katso myös
- [Voit varmuuskopioida BizTalk-palvelu](http://go.microsoft.com/fwlink/p/?LinkID=325584)
- [BizTalk palvelun palauttaminen varmuuskopiosta](http://go.microsoft.com/fwlink/p/?LinkID=325582)
- [BizTalk Services: Kehittäjä, Basic, Vakio ja Premium versiot kaavio](http://go.microsoft.com/fwlink/p/?LinkID=302279)
- [BizTalk Services: Valmistelu käyttämällä Azure perinteinen portal](http://go.microsoft.com/fwlink/p/?LinkID=302280)
- [BizTalk Services: Tila-kaavion valmistelu](http://go.microsoft.com/fwlink/p/?LinkID=329870)
- [BizTalk Services: Raporttinäkymät-ikkunan, näyttö ja skaalaus-välilehdet](http://go.microsoft.com/fwlink/p/?LinkID=302281)
- [BizTalk Services: rajoittaminen](http://go.microsoft.com/fwlink/p/?LinkID=302282)
- [BizTalk Services: Myöntäjän nimi ja myöntäjä avain](http://go.microsoft.com/fwlink/p/?LinkID=303941)
- [Miten voin aloittaa käyttämällä Azure BizTalk Services SDK-paketissa](http://go.microsoft.com/fwlink/p/?LinkID=302335)

[BackupStatus]: ./media/biztalk-backup-restore/status-last-backup.png
[Restore]: ./media/biztalk-backup-restore/restore-ui.png
[AutomaticBU]: ./media/biztalk-backup-restore/AutomaticBU.png
[RestoreBizTalkService]: ./media/biztalk-backup-restore/RestoreBizTalkServiceWindow.png
 
