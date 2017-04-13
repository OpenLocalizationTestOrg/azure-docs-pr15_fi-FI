<properties
    pageTitle="Azure CDN reaaliaikaisia ilmoituksia | Microsoft Azure"
    description="Microsoft Azure CDN reaaliaikainen ilmoitukset. Reaaliaikainen ilmoitukset tarjoavat CDN profiilin päätepisteet suorituskyvyn ilmoituksia."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/12/2016"
    ms.author="casoper"/>

# <a name="real-time-alerts-in-microsoft-azure-cdn"></a>Microsoft Azure CDN reaaliaikainen ilmoitukset

[AZURE.INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]


## <a name="overview"></a>Yleiskatsaus

Tässä asiakirjassa kerrotaan Microsoft Azure CDN reaaliaikaisia ilmoituksia. Tämä toiminto on reaaliaikaiset ilmoitukset CDN profiilin päätepisteet suorituskyvystä.  Voit määrittää sähköpostin tai HTTP ilmoitusten perusteella:

* Kaistanleveyden
* Tilakoodin
* Välimuistin tilat
* Yhteydet

## <a name="creating-a-real-time-alert"></a>Reaaliaikainen ilmoituksen luominen

1. Siirry [Azure Portal](https://portal.azure.com)CDN profiilin.

    ![CDN profiili-sivu](./media/cdn-real-time-alerts/cdn-profile-blade.png)

2. Valitse **hallinta** -painike CDN profiili-sivu.

    ![CDN profiilin sivu hallinta-painike](./media/cdn-real-time-alerts/cdn-manage-btn.png)

    CDN hallinta-portaalin avautuu.

3. Vie hiiren osoitin **Analytics** -välilehti ja valitse Vie hiiren osoitin **Reaaliaikaisen tilasto** -valikon avauspainike.  Valitse **reaaliaikaisia ilmoituksia**.

    ![CDN hallinta-portaalin](./media/cdn-real-time-alerts/cdn-premium-portal.png)

    Olemassa olevan ilmoitusten käyttömahdollisuudet (jos saatavilla) luettelo tulee näkyviin.

4. Napsauta **Lisää ilmoitus** -painiketta.

    ![Lisää ilmoitus-painike](./media/cdn-real-time-alerts/cdn-add-alert.png)

    Lomakkeen luominen uusi ilmoitus tulee näkyviin.

    ![Uusi ilmoitus-lomake](./media/cdn-real-time-alerts/cdn-new-alert.png)

5. Jos haluat tämän ilmoituksen on aktiivinen, kun valitset **Tallenna**, valitse **Ilmoitus käytössä** -valintaruutu.

6. Kirjoita ilmoituksen kuvaava nimi **nimi** -kenttään.

7. Valitse **Mediatyypin** avattavasta valikosta **Suuret HTTP-objekti**.

    ![Mediatyypin HTTP suuri objekti on valittuna](./media/cdn-real-time-alerts/cdn-http-large.png)

    > [AZURE.IMPORTANT] Sinun on valittava **HTTP objekti** **Mediatyypin**mukaan.  Muut vaihtoehdot eivät ole käytössä **Azure CDN-Verizon**mukaan.  Valitse **HTTP suuri objekti** epäonnistumisen aiheuttaa ilmoituksen koskaan käynnistettäväksi.

8. Luo **lauseke** valitsemalla **arvo**, **operaattori**ja **käynnistimen arvo**seurannassa.

    - **Arvo**Valitse valvottu ehto.  **Kaistanleveyden Mbps** on kaistanleveyden käytön, jotka-määrä.  **Yhteyksien kokonaismäärän** on Microsoftin reunapalvelimet samanaikainen HTTP yhteyksien määrä.  Katso määritelmät eri välimuistin tilat ja tilakoodin [Azure CDN välimuistin tilakoodin](https://msdn.microsoft.com/library/mt759237.aspx) ja [Azure CDN HTTP-tilakoodin](https://msdn.microsoft.com/library/mt759238.aspx)
    - **Operaattori** on matemaattinen operaattori, joka muodostaa lisätiedot ja käynnistimen arvon välinen suhde.
    - **Käynnistimen** arvo on täytyttävä, ennen kuin ilmoitus lähetetään raja-arvo.

    Valitse seuraavassa esimerkissä, lauseke, joka on luotu ilmaisee, että haluan ilmoituksen, kun 404 tilakoodin määrä on suurempi kuin 25.

    ![Reaaliaikainen ilmoitusten esimerkkilauseke](./media/cdn-real-time-alerts/cdn-expression.png)

9. Kirjoita **aikaväli**, kuinka usein haluat lauseke lasketaan.

10. Valitse **Ilmoita-** avattavan luettelon, kun haluat saada ilmoituksen, kun lauseke on TOSI.
    
    - **Ehto Käynnistä** ilmaisee, että ilmoitus lähetetään, kun määritetyn ehdon havaitaan.
    - **Ehto Lopeta** ilmaisee, että ilmoitus lähetetään, kun määritetyn ehdon enää havaita. Tämä ilmoitus voi käynnistää sen jälkeen, kun meidän verkon valvonta järjestelmän havaita ehtoa tapahtui.
    - **Jatkuva** ilmaisee, että ilmoitus lähetetään aina, kun verkon seuranta järjestelmä tunnistaa ehtoa. Ota huomioon, verkon seuranta järjestelmä tarkistaa vain kerran aikavälillä määritetyn ehdon.
    - **Ehto Käynnistä ja Lopeta** ilmaisee, että ilmoitus lähetetään ensimmäistä kertaa, että määritetyn ehdon havaitaan ja uudelleen kun ehto ei enää havaitaan.

11. Jos haluat saada ilmoituksen sähköpostitse, valitse **Ilmoita sähköpostitse** -valintaruutu.  

    ![Ilmoita sähköpostia lomakkeen mukaan](./media/cdn-real-time-alerts/cdn-notify-email.png)
    
    Kirjoita **Vastaanottaja** -kentässä voit haluamaasi ilmoitukset lähetetään sähköpostiosoite. **Aihe** -ja **leipätekstin**ehkä käytä oletusnimeä tai voit mukauttaa **käytettävissä olevat avainsanat** -luettelon avulla voit lisätä dynaamisesti ilmoitusten tiedot, kun viesti on lähetetty viesti.

    > [AZURE.NOTE] Voit testata sähköposti-ilmoituksen napsauttamalla **Testaa ilmoitus** -painiketta, mutta vain, kun loput määritykset on tallennettu.

12. Jos haluat, että ilmoitukset lähetetään verkkopalvelimeen, valitse **Ilmoita postitse HTTP** -valintaruutu.

    ![Ilmoita HTTP Post lomakkeen mukaan](./media/cdn-real-time-alerts/cdn-notify-http.png)

    Kirjoita **Url** -kenttään haluamaasi HTTP-sanoman kirjaamisen URL-osoite. Kirjoita **ylä** -tekstiruutuun lähetetään pyyntö HTTP-otsikot.  **Body** voit mukauttaa **käytettävissä olevat avainsanat** -luettelon avulla voit lisätä dynaamisesti ilmoitusten tiedot, kun viesti on lähetetty viesti.  XML-paketti, jossa on samankaltainen kuin ** **ylä** - ja** oletusarvo seuraavassa esimerkissä.

    ```
    <string xmlns="http://schemas.microsoft.com/2003/10/Serialization/">
        <![CDATA[Expression=Status Code : 404 per second > 25&Metric=Status Code : 404 per second&CurrentValue=[CurrentValue]&NotificationCondition=Condition Start]]>
    </string>
    ```

    > [AZURE.NOTE] Voit testata HTTP Post ilmoituksessa, joka **Testaa ilmoitus** -vaihtoehdon, mutta vain, kun loput määritykset on tallennettu.

13. Ilmoitusten asetusten tallentaminen valitsemalla **Tallenna** .  Jos vaiheessa 5 valittuna **Ilmoitus käytössä** ilmoituksen on nyt aktiivinen.

## <a name="next-steps"></a>Seuraavat vaiheet

- Analysoi [-Azure CDN reaaliaikainen tilasto](cdn-real-time-stats.md)
- Tarkastella tarkemmin – [Lisäasetukset HTTP](cdn-advanced-http-reports.md) -raportit
- Analysoi [käyttötavat](cdn-analyze-usage-patterns.md)

