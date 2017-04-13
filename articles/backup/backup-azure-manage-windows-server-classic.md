<properties
    pageTitle="Hallitse Azure varmuuskopiointi vaults ja perinteinen käyttöönoton mallin Azure-palvelimet | Microsoft Azure"
    description="Opi hallitsemaan Azure varmuuskopiointi vaults ja palvelinten Tässä opetusohjelmassa avulla."
    services="backup"
    documentationCenter=""
    authors="markgalioto"
    manager="jwhit"
    editor="tysonn"/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="jimpark;markgal"/>


# <a name="manage-azure-backup-vaults-and-servers-using-the-classic-deployment-model"></a>Azure varmuuskopiointi vaults ja perinteinen käyttöönoton mallin palvelinten hallinta

> [AZURE.SELECTOR]
- [Resurssien hallinta](backup-azure-manage-windows-server.md)
- [Perinteinen](backup-azure-manage-windows-server-classic.md)

Tässä artikkelissa löydät kautta Azure perinteinen portaalin ja Microsoft Azure Backup agentti varmuuskopion hallintatehtäviä yleiskatsaus.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Resurssien hallinnan käyttöönottomalli.

## <a name="management-portal-tasks"></a>Portaalin hallintatehtävät
1. Kirjautuminen [hallinta-portaalin](https://manage.windowsazure.com).

2. Valitse **Palautus palvelut**ja valitse sitten varmuuskopion säilöön, voit tarkastella Pika-aloitus-sivun nimi.

    ![Palautus-palvelut](./media/backup-azure-manage-windows-server-classic/rs-left-nav.png)

Näet käytettävissä hallintatehtäviä valitsemalla Pika-aloitus-sivun yläosassa asetukset.

![Välilehtien hallinta](./media/backup-azure-manage-windows-server-classic/qs-page.png)

### <a name="dashboard"></a>Raporttinäkymät-ikkunan
Valitse **raporttinäkymät-ikkunan** käyttö esiin palvelimen. **Käyttö yleiskatsaus** sisältää:

- Windows-palvelimien määrä rekisteröity pilveen
- Azuren näennäiskoneiden cloud suojattu määrä
- Kulutettu Azure-tallennustilan
- Viimeisimmät töiden tilan

Koontinäytön alareunassa voit suorittaa seuraavia tehtäviä:

- **Hallinta-varmenteen** – Jos varmenne on käytetty rekisteröidä palvelimen ja valitse tämän toiminnon avulla voit päivittää varmenne. Jos käytössäsi on säilö tunnistetiedot, älä käytä **hallinta-varmenteen**.
- **Poista** - poistaa nykyisen varmuuskopioinnin säilö. Jos varmuuskopion säilö käytetään enää, voit poistaa sen ja vapauttaa tilaa. **Poistaa** vain käytössä, kun rekisteröidyt jokaisessa palvelimessa on poistettu säilöstä.

![Varmuuskopion Raporttinäkymät-ikkunan tehtävät](./media/backup-azure-manage-windows-server-classic/dashboard-tasks.png)

## <a name="registered-items"></a>Rekisteröity kohteet
Valitse **Rekisteröity kohteiden** tarkasteleminen, joka on rekisteröity tämän säilöön-palvelimien nimet.

![Rekisteröity kohteet](./media/backup-azure-manage-windows-server-classic/registered-items.png)

**Tyyppi** -suodatin oletusarvoisesti Azure Virtual Machine. Jos haluat tarkastella, joka on rekisteröity tämän säilö palvelinten nimet, valitse **Windows server** avattavassa valikossa.

Täällä voit suorittaa seuraavia tehtäviä:

- **Salli uudelleen-rekisteröinti** – kun tämä asetus on valittu palvelimen avulla voit **Rekisteröinnistä** : paikallisen Microsoft Azure Backup agentti rekisteröidä palvelimen varmuuskopion säilö toisen kerran. Voit joutua muuttamaan rekisteröidä uudelleen varmenteen virheen vuoksi tai jos palvelin oli muodostetaan uudelleen.
- Palvelimen **poistaminen** - poistaa varmuuskopion säilöstä. Kaikki liittyvät palvelimeen tallennetut tiedot poistetaan heti.

    ![Rekisteröity kohteita tehtävät](./media/backup-azure-manage-windows-server-classic/registered-items-tasks.png)

## <a name="protected-items"></a>Suojatun kohteet
Valitse Näytä kohteet, jotka-palvelimet varmuuskopioida **Suojatut osat** .

![Suojatun kohteet](./media/backup-azure-manage-windows-server-classic/protected-items.png)

## <a name="configure"></a>Asetusten määrittäminen

**Määritä** -välilehdestä voit valita haluamasi redundancy tallennuspaikka. Löydät parhaan mahdollisen ajan tallennustilan redundancy-vaihtoehto on oikea säilöön luomisen jälkeen ja ennen kuin se on rekisteröity missään.

>[AZURE.WARNING] Kun kohde on rekisteröity säilö, redundancy tallennuspaikka on lukittu, eikä sitä voi muokata.

![Asetusten määrittäminen](./media/backup-azure-manage-windows-server-classic/configure.png)

Katso tästä artikkelista lisätietoja [tallennustilan redundanssin](../storage/storage-redundancy.md).

## <a name="microsoft-azure-backup-agent-tasks"></a>Microsoft Azure varmuuskopiointi agentti tehtävät

### <a name="console"></a>Konsolin

Avaa **Microsoft Azure Backup agentti** (löydät sen koneen hakemalla *Microsoft Azure varmuuskopiointi*).

![Backup-agentti](./media/backup-azure-manage-windows-server-classic/snap-in-search.png)

**Toiminnot** käytettävissä backup agentti-konsolin oikeassa voit suorittaa seuraavat hallintatehtäviä:

- Rekisteröi-palvelin
- Ajoita varmuuskopiointi
- Varmuuskopioi
- Ominaisuuksien muuttaminen

![Agentti console-toiminnot](./media/backup-azure-manage-windows-server-classic/console-actions.png)

>[AZURE.NOTE] **Palauta tiedot**-kohdassa [Palauta tiedostot Windows server- tai Windows-asiakaskoneeseen](backup-azure-restore-windows-server.md).

### <a name="modify-an-existing-backup"></a>Muokkaa olemassa oleva varmuuskopio

1. Valitse Microsoft Azure Backup agent **Ajoita varmuuskopiointi**.

    ![Windows Server varmuuskopion ajoittaminen](./media/backup-azure-manage-windows-server-classic/schedule-backup.png)

2. **Aikataulun Ohjattu varmuuskopiointi** Jätä **muutosten tekeminen varmuuskopioidut kohteet tai ajat** -asetus on valittuna ja valitse **Seuraava**.

    ![Muokkaa ajoitettu varmuuskopiointi](./media/backup-azure-manage-windows-server-classic/modify-or-stop-a-scheduled-backup.png)

3. Jos haluat lisätä tai muokata kohteita, valitse **Valitse kohteita** -näytössä **Lisää kohteita**.

    Voit myös määrittää **Pois jätettyjen asetukset** ohjatun tältä sivulta. Jos haluat jättää pois tiedostot tai tiedostotyypit lue ohjeet lisääminen [pois jätettyjen asetukset](#exclusion-settings).

4. Valitse tiedostot ja kansiot, jotka haluat varmuuskopioida ja valitse **OK**.

    ![Lisää kohteita](./media/backup-azure-manage-windows-server-classic/add-items-modify.png)

5. Määritä **varmuuskopioinnin aikataulu** ja valitse **Seuraava**.

    Voit ajoittaa (on enintään 3 kertaa päivässä) päivittäin tai viikoittain varmuuskopiot.

    ![Määritä varmuuskopiointiaikataulu](./media/backup-azure-manage-windows-server-classic/specify-backup-schedule-modify-close.png)

    >[AZURE.NOTE] Tässä [artikkelissa](backup-azure-backup-cloud-as-tape.md)kerrotaan varmuuskopioinnin ajoituksen määrittäminen.

6. **Säilytyskäytännön** varmuuskopion ja valitse **Seuraava**.

    ![Valitse oman säilytyskäytäntö](./media/backup-azure-manage-windows-server-classic/select-retention-policy-modify.png)

7. **Vahvistusnäytössä** Tarkista tiedot ja valitse **Valmis**.

8. Kun ohjattu toiminto päättyy, **aikataulun**luominen, valitse **Sulje**.

    Muokattuasi suojaus voit vahvistaa, että varmuuskopiot käynnistävät siirtymällä **työt** -välilehti ja varmistamalla, että muutokset näkyvät varmuuskopion työt oikein.

### <a name="enable-network-throttling"></a>Verkon rajoittimen käyttöönotto  
Azure Backup agent on Throttling-välilehdestä, jossa voit määrittää, miten kaistanleveys käytetään tiedonsiirrossa. Ohjausobjektin voi olla hyödyllinen, jos haluat varmuuskopioida tiedot aikana työaika, mutta et halua varmuuskopiointia voi häiritä muiden internet-liikenne. Tietojen rajoittaminen siirto koskee varmuuskopioiminen ja palauttaminen toimintoja.  

Jos haluat ottaa käyttöön rajoitusta:

1. Valitse **Backup agentti** **Ominaisuuksien muuttaminen**.

2. Valitse **internet-kaistanleveyden käytön rajoittaminen backup toimille käyttöön** -valintaruutu.

    ![Verkon rajoittaminen](./media/backup-azure-manage-windows-server-classic/throttling-dialog.png)

3. Kun olet ottanut rajoitusta, määritä sallitut kaistanleveyden varmuuskopiotiedot siirron **työtuntien** ja **Muut työtuntien**aikana.

    Kaistanleveyden arvot alkaa 512 kilotavua sekunnissa (Kbps), ja voit siirtyä 1023 megatavua sekunnissa (Mbps). Voit myös nimetä alku ja lopetuksen **työtuntien**ja mitkä viikonpäivien pidetään työn päivää. Nimetty työtuntien ulkopuolella aika pidetään työtunnit.

4. Valitse **OK**.

## <a name="exclusion-settings"></a>Pois jätettyjen asetukset

1. Avaa **Microsoft Azure Backup agentti** (löydät sen koneen hakemalla *Microsoft Azure varmuuskopiointi*).

    ![Avaa backup agentti](./media/backup-azure-manage-windows-server-classic/snap-in-search.png)

2. Valitse Microsoft Azure Backup agent **Ajoita varmuuskopiointi**.

    ![Windows Server varmuuskopion ajoittaminen](./media/backup-azure-manage-windows-server-classic/schedule-backup.png)

3. Aikataulun varmuuskopion ohjatun Jätä **muutosten tekeminen varmuuskopioidut kohteet tai ajat** -asetus on valittuna ja valitse **Seuraava**.

    ![Muokkaa aikataulua](./media/backup-azure-manage-windows-server-classic/modify-or-stop-a-scheduled-backup.png)

4. Valitse **Poikkeukset-asetukset**.

    ![Valitse kohteet jättäminen pois](./media/backup-azure-manage-windows-server-classic/exclusion-settings.png)

5. Valitse **Lisää pois jätettyjen**.

    ![Lisää poikkeukset](./media/backup-azure-manage-windows-server-classic/add-exclusion.png)

6. Valitse sijainti ja valitse **OK**.

    ![Pois jätettyjen sijainti](./media/backup-azure-manage-windows-server-classic/exclusion-location.png)

7. Lisää tiedostotunniste **Tiedostotyyppi** -kentässä.

    ![Jätä pois tiedostotyypin mukaan](./media/backup-azure-manage-windows-server-classic/exclude-file-type.png)

    Lisäämällä .mp3-tiedostotunnistetta

    ![Tiedoston tyyppi-Esimerkki](./media/backup-azure-manage-windows-server-classic/exclude-mp3.png)

    Lisää toinen tunniste, valitsemalla **Lisää poikkeus** ja Syötä toinen (lisääminen .jpeg-tunniste) tiedostotyyppiä.

    ![Toinen tiedoston tyyppi-Esimerkki](./media/backup-azure-manage-windows-server-classic/exclude-jpg.png)

8. Kun olet lisännyt kaikki tunnisteet, valitse **OK**.

9. Etene ohjatun aikataulun varmuuskopioinnin valitsemalla **Seuraava** , kunnes **Vahvistus-sivu**ja valitse sitten **Valmis**.

    ![Pois jätettyjen vahvistus](./media/backup-azure-manage-windows-server-classic/finish-exclusions.png)

## <a name="next-steps"></a>Seuraavat vaiheet
- [Palauttaa Azure Windows Server- tai Windows-asiakas](backup-azure-restore-windows-server.md)
- Lisätietoja Azure varmuuskopiointi-artikkelissa [Azure varmuuskopiointi yleiskatsaus](backup-introduction-to-azure-backup.md)
- Käy [Azure varmuuskopion keskustelupalsta](http://go.microsoft.com/fwlink/p/?LinkId=290933)
