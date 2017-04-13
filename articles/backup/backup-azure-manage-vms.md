
<properties
    pageTitle="Hallitse Resurssienhallinta käyttöön virtual machine-varmuuskopiot | Microsoft Azure"
    description="Lue, miten voit valvoa Resurssienhallinta käyttöön virtual machine-varmuuskopiot"
    services="backup"
    documentationCenter=""
    authors="trinadhk"
    manager="shreeshd"
    editor=""/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="jimpark; markgal; trinadhk"/>

# <a name="manage-azure-virtual-machine-backups"></a>Azure virtual machine-varmuuskopiot hallinta

> [AZURE.SELECTOR]
- [Azure AM varmuuskopioiden hallinta](backup-azure-manage-vms.md)
- [Perinteinen AM varmuuskopioiden hallinta](backup-azure-manage-vms-classic.md)

Tässä artikkelissa on ohjeita hallinnasta AM varmuuskopiot ja kerrotaan portaalin Raporttinäkymät-ikkunan varmuuskopion ilmoitusten tietoihin. Tämän artikkelin ohjeet koskevat VMs käyttäminen palautus Services vaults. Tässä artikkelissa ei käsitellä näennäiskoneiden luominen eikä kerrotaan, miten voit suojata näennäiskoneiden. Katso askeleet, valitse suojata Azure Resurssienhallinta käyttöön VMs Azure-tietokannassa palautus Services säilöön, [ensin kohde: Varmuuskopioi VMs palautus palvelut-säilö](backup-azure-vms-first-look-arm.md).

## <a name="manage-vaults-and-protected-virtual-machines"></a>Vaults ja suojatun virtual koneet hallinta

Azure-portaalissa palautus Services säilö Raporttinäkymät-ikkunan pääsee tietoja säilö mukaan lukien:

- viimeisimmän varmuuskopioinnin tilannevedoksen, joka on myös uusimmat Palauta-kohdassa < br\>
- varmuuskopion käytännön < br\>
- kaikki varmuuskopion tilannevedoksia koko yhteensä < br\>
- luku, joka on suojattu säilö näennäiskoneiden < br\>

Monta hallintatehtäviä virtuaalikoneen varmuuskopioimalla alkavat avaaminen säilö koontinäytön. Koska vaults voidaan suojata useita kohteita (tai useita VMs) tietyn AM tietoja, Avaa säilö kohteen Raporttinäkymät-ikkunan. Seuraavien ohjeiden avulla voit avata *säilö Raporttinäkymät-ikkunan* ja jatka sitten *säilö kohteen Raporttinäkymät-ikkunan*. Sekä ohjeita, jotka osoittavat, miten voit lisätä säilö ja säilöön kohteen Azure koontinäyttö käyttämällä Raporttinäkymät-ikkunan komento PIN-koodi on "Vihjeitä". Raporttinäkymät-ikkunan kiinnittäminen on tapa säilöön tai kohteen pikakuvakkeen luominen. Voit suorittaa yleiset komennot pikakuvaketta.

>[AZURE.TIP] Jos sinulla on useita raporttinäkymien ja lavat Avaa, dian edestakaisin Azure Raporttinäkymät ikkunan alareunassa Tumma sininen liukusäätimen avulla.

![Koko näkymä, jossa on liukusäädin](./media/backup-azure-manage-vms/bottom-slider.png)

### <a name="open-a-recovery-services-vault-in-the-dashboard"></a>Avaa palautus Services säilö koontinäytön:

1. Kirjautuminen [Azure portal](https://portal.azure.com/).

2. Valitse toiminto-valikosta valitsemalla **Selaa** ja kirjoita resurssien luetteloa, **Palautus-palvelut**. Kun alat kirjoittaa, luettelon suodattimet kirjoittamiesi tietojen perusteella. Valitse **palautus-palveluiden säilö**.

    ![Luo palautus Services säilö vaihe 1](./media/backup-azure-manage-vms/browse-to-rs-vaults.png) <br/>

    Palautus-palveluiden vaults näkyvät luettelossa.

    ![Palautus palveluluetteloon vaults ](./media/backup-azure-manage-vms/list-o-vaults.png) <br/>

    >[AZURE.TIP] Jos kiinnität säilö Azure-koontinäyttö, että säilö on heti käytettävissä, kun avaat Azure portaalin. Jos haluat kiinnittää säilö koontinäyttö, säilöön-luettelossa, napsauttamalla sitä hiiren kakkospainikkeella säilö ja valitse **Kiinnitä Raporttinäkymät-ikkunan**.

3. Valitse Avaa sen Raporttinäkymät-ikkunan säilö vaults-luettelosta. Kun valitset säilö, Avaa säilö Raporttinäkymät-ikkunan ja **asetukset** -sivu. Seuraavassa kuvassa on korostettuna **Contoso-säilö** Raporttinäkymät-ikkunan.

    ![Avaa säilö Raporttinäkymät-ikkunan ja asetukset-sivu](./media/backup-azure-manage-vms/full-view-rs-vault.png)

### <a name="open-a-vault-item-dashboard"></a>Säilö kohteen Raporttinäkymät-ikkunan avaaminen

Valitse edellisessä vaiheessa avasit säilö Raporttinäkymät-ikkunan. Avaa säilö kohteen Raporttinäkymät-ikkunan seuraavasti:

1. Valitse **Azuren näennäiskoneiden**säilö raporttinäkymien, **Varmuuskopiointi kohteet** -ruutu.

    ![Avaa varmuuskopio kohteet-ruutu](./media/backup-azure-manage-vms/contoso-vault-1606.png)

    **Varmuuskopioidut kohteet** -sivu näyttää kunkin kohteen viimeisen varmuuskopiointityön. Tässä esimerkissä on yksi virtual machine-demovm-markgal suojattu säilö tässä.  

    ![Varmuuskopioidut kohteet-ruutu](./media/backup-azure-manage-vms/backup-items-blade.png)

    >[AZURE.TIP] Aputoiminnot voit kiinnittää Azure Raporttinäkymät-ikkunan säilöön-kohteeseen. Jos haluat kiinnittää säilöön-kohteen säilö kohdeluettelon, kohdetta hiiren kakkospainikkeella ja valitse **Kiinnitä Raporttinäkymät-ikkunan**.

2. Valitse Avaa säilö kohteen Raporttinäkymät-ikkunan kohde **Varmuuskopiointi kohteet** -sivu.

    ![Varmuuskopioidut kohteet-ruutu](./media/backup-azure-manage-vms/backup-items-blade-select-item.png)

    Säilö kohteen Raporttinäkymät-ikkunan ja avaa sen **asetukset** -sivu.

    ![Varmuuskopioidut kohteet raporttinäkymä, jossa on asetukset-sivu](./media/backup-azure-manage-vms/item-dashboard-settings.png)

    Säilö kohteen Koontinäytöstä voit tehdä monta tärkeimmät hallintatehtävät, kuten:

    - muuttaa käytännöt tai luoda uuden varmuuskopion käytännön < br\>
    - Näyttää palauttaminen pistettä ja tarkastella niiden yhdenmukaisuuden tilaan < br\>
    - tarvittaessa varmuuskopion virtual machine < br\>
    - Lopeta suojaaminen näennäiskoneiden < br\>
    - Jatka suojaus virtual koneen < br\>
    - Poista varmuuskopiotiedot (tai palautuspiste) < br\>
    - [Palauta varmuuskopio (tai palautuspiste)](./backup-azure-arm-restore-vms.md#restore-a-recovery-point) < br\>

Tämän osion toimenpiteistä aloituskohdan on säilö kohteen Raporttinäkymät-ikkunan.

## <a name="manage-backup-policies"></a>Varmuuskopion käytäntöjen hallinta

1. [Säilö kohteen Raporttinäkymät-ikkunan](backup-azure-manage-vms.md#open-a-vault-item-dashboard)Valitse **Kaikki asetukset** Avaa **asetukset** -sivu.

    ![Varmuuskopion käytännön sivu](./media/backup-azure-manage-vms/all-settings-button.png)

2. Valitse **asetukset** -sivu **Varmuuskopiointi käytännön** avaa kyseisen sivu.

    Valitse sivu-varmuuskopioinnin korkojakso ja säilytyskäytäntöjä alueen tiedot ovat näkyvissä.

    ![Varmuuskopion käytännön sivu](./media/backup-azure-manage-vms/backup-policy-blade.png)

3. **Valitse Varmuuskopioi käytäntö** -valikosta:
    - Jos haluat muuttaa käytäntöjä, valitse eri käytäntö ja valitse **Tallenna**. Uuden käytännön mukautuu automaattisesti säilö. < br\>
    - Käytännön luomisesta, valitse **Luo uusi**.

    ![Virtuaalikoneen varmuuskopiointi](./media/backup-azure-manage-vms/backup-policy-create-new.png)

    Ohjeita varmuuskopion käytännön luomisesta on artikkelissa [varmuuskopion käytännön määrittäminen](backup-azure-manage-vms.md#defining-a-backup-policy).

[AZURE.INCLUDE [backup-create-backup-policy-for-vm](../../includes/backup-create-backup-policy-for-vm.md)]


## <a name="on-demand-backup-of-a-virtual-machine"></a>Tarvittaessa varmuuskopion virtual machine
Voi tehdä tarvittaessa virtual konetta varmuuskopion, kun se on määritetty suojaus. Jos ensimmäinen varmuuskopioinnin odottaa, tarvittaessa varmuuskopiointi luo kopio virtuaalikoneen palautus Services säilö. Jos ensimmäinen varmuuskopiointi on valmis, tarvittaessa varmuuskopiointi vain lähettää muutokset edellisen tilannevedoksen, valitse palautus-palveluiden säilö. Toisin sanoen myöhemmin varmuuskopiot ovat aina vaiheittainen.

>[AZURE.NOTE] Tarvittaessa varmuuskopiointi säilytys-alue on määritetty käytännön päivittäisen varmuuskopioinnin pisteen säilytys-arvo. Jos mikään päivittäisen varmuuskopioinnin kohta on valittuna, viikoittain varmuuskopion pisteen käytetään.

Käynnistettävän virtual machine tarvittaessa-varmuuskopion:

- Valitse **Varmuuskopioi** [säilö kohteen Raporttinäkymät-ikkunan](backup-azure-manage-vms.md#open-a-vault-item-dashboard).

    ![Varmuuskopioimalla nyt-painike](./media/backup-azure-manage-vms/backup-now-button.png)

    Portaalin varmistetaan, että haluat aloittaa tarvittaessa varmuuskopiointityön. Käynnistä valitsemalla **Kyllä** varmuuskopiointityön.

    ![Varmuuskopioimalla nyt-painike](./media/backup-azure-manage-vms/backup-now-check.png)

    Varmuuskopiointityön Luo palautus-kohtaa. Palautus-kohdan säilytys-alue on sama kuin liittyvän virtuaalikoneen käytännön määritetyn säilytys alueen. Edistymisen työn säilö raporttinäkymät-ikkunassa, napsauta **Varmuuskopiointi työt** -ruutua.  


## <a name="stop-protecting-virtual-machines"></a>Lopeta näennäiskoneiden suojaaminen
Jos haluat lopettaa suojaaminen virtual machine, sinua pyydetään haluat säilyttää palautus kohdeosoite. Voit lopettaa suojaaminen näennäiskoneiden kahdella tavalla:
- Peruuta kaikki varmuuskopion tulevien projektien ja poista palautus pisteiden tai
- Lopeta kaikki tulevaisuudessa varmuuskopion työt, mutta jättää palautus pisteet <br/>

Tällä jättää palautus pisteet tallennustilan liittyvät kustannukset. Jätä palautus pisteet etuna on kuitenkin voit palauttaa virtuaalikoneen myöhemmin tarvittaessa. Tietoja, jätä palautus pisteet kustannus on artikkelissa [hinnat tiedot](https://azure.microsoft.com/pricing/details/backup/). Jos haluat poistaa palautus pisteiden, et voi palauttaa virtuaalikoneen.

Jos haluat lopettaa virtual machine suojaus:

1. [Säilö kohteen Raporttinäkymät-ikkunan](backup-azure-manage-vms.md#open-a-vault-item-dashboard)Valitse **Lopeta varmuuskopiointi**.

    ![Lopeta varmuuskopiointi-painike](./media/backup-azure-manage-vms/stop-backup-button.png)

    Lopeta varmuuskopiointi-sivu avautuu.

    ![Lopeta varmuuskopion sivu](./media/backup-azure-manage-vms/stop-backup-blade.png)

2. Valitse **Lopeta varmuuskopion** sivu, säilytetäänkö vai poistetaanko palautettavat tiedot. Tiedot-ruutu on valittua tietoja.

    ![Lopeta suojaus](./media/backup-azure-manage-vms/retain-or-delete-option.png)

3. Jos haluat säilyttää varmuuskopiotiedot, siirry vaiheeseen 4. Jos haluat poistaa varmuuskopiotiedot, varmista, että haluat lopettaa varmuuskopion työt ja poista palautus - kohtien Kirjoita kohteen nimi.

    ![Lopeta tarkistus](./media/backup-azure-manage-vms/item-verification-box.png)

    Jos et ole varma napsauttamalla kohteen nimen, osoitinta huutomerkki voit tarkastella nimeä. Kohteen nimi on myös kohdassa **Pysäytä varmuuskopiointi** sivu yläreunassa.

4. Voit myös antaa **syy** tai **Kommentti**.

5. Lopeta nykyisen kohteen varmuuskopiointityön napsauttamalla  ![varmuuskopion Pysäytä-painike](./media/backup-azure-manage-vms/stop-backup-button-blue.png)

    Ilmoitus kertoo varmuuskopion työt pysäyttämisestä.

    ![Vahvista Lopeta suojaus](./media/backup-azure-manage-vms/stop-message.png)


## <a name="resume-protection-of-a-virtual-machine"></a>Jatka suojausta virtual koneen
Jos **Säilyttää Varmuuskopioi tietoja** -asetus on valittu, kun virtuaalikoneen suojaus keskeytyi, on mahdollista, kun haluat jatkaa suojaus. Jos **Varmuuskopiointi tietojen poisto** on valittu, ei voi jatkaa virtuaalikoneen suojaus.

Kun haluat jatkaa virtuaalikoneen suojaus

1. [Säilö kohteen Raporttinäkymät-ikkunan](backup-azure-manage-vms.md#open-a-vault-item-dashboard)valitsemalla **Jatka varmuuskopion**.

    ![Jatka suojaus](./media/backup-azure-manage-vms/resume-backup-button.png)

    Varmuuskopiointi-käytäntö-sivu avautuu.

    >[AZURE.NOTE] Kun suojaat virtuaalikoneen uudelleen, voit valita eri käytäntö kuin käytäntö, jonka virtuaalikoneen on suojattu aluksi.

2. [Muuta käytännöt tai Luo uusi varmuuskopion käytäntö](backup-azure-manage-vms.md#change-policies-or-create-a-new-backup-policy), noudattamalla voit määrittää virtuaalikoneen käytännön.

    Kun varmuuskopioinnin käytäntö otetaan käyttöön virtuaalikoneen, näyttöön tulee seuraava sanoma.

    ![Onnistuneesti suojatun AM](./media/backup-azure-manage-vms/success-message.png)

## <a name="delete-backup-data"></a>Varmuuskopiointi-tietojen poistaminen
Voit poistaa virtual machine liittyvät varmuuskopiotiedot **lopettaa varmuuskopioinnin** aikana tai milloin tahansa varmuuskopioinnin jälkeen työ on valmis. Vaikka voi olla hyödyllistä odottamaan päivinä tai viikkoina ennen poistamista palautus kohdeosoite. Toisin kuin palauttaminen palautus pisteet, poistettaessa varmuuskopiotiedot, et voi valita tiettyjä palautus viittaa poistaminen. Jos haluat poistaa varmuuskopion tietoja, voit poistaa kaikki liittyvät kohteen palautuspisteet.

Oletetaan, virtuaalikoneen varmuuskopiointityön on pysäytetty tai poistettu käytöstä. Kun työ on poistettu käytöstä, **Jatka varmuuskopiointi** ja **Poista varmuuskopiointi** -asetukset ovat käytettävissä säilö kohteen raporttinäkymät-ikkunassa.

![Jatka ja poista-painikkeet](./media/backup-azure-manage-vms/resume-delete-buttons.png)

Voit poistaa varmuuskopiotiedot virtual tietokoneessa, jossa *varmuuskopion käytöstä*seuraavasti:

1. [Säilö kohteen Raporttinäkymät-ikkunan](backup-azure-manage-vms.md#open-a-vault-item-dashboard)Poista **varmuuskopion**.

    ![AM tyyppi](./media/backup-azure-manage-vms/delete-backup-buttom.png)

    **Poista varmuuskopiointi tiedot** -sivu avautuu.

    ![AM tyyppi](./media/backup-azure-manage-vms/delete-backup-blade.png)

2. Kirjoita Vahvista poistettavien palautus pisteet kohteen nimi.

    ![Lopeta tarkistus](./media/backup-azure-manage-vms/item-verification-box.png)

    Jos et ole varma napsauttamalla kohteen nimen, osoitinta huutomerkki voit tarkastella nimeä. Kohteen nimi on myös **Varmuuskopiointi tietojen poisto** sivu yläreunassa.

3. Voit myös antaa **syy** tai **Kommentti**.

4. Voit poistaa valitun kohteen varmuuskopioidut tiedot valitsemalla  ![varmuuskopion Pysäytä-painike](./media/backup-azure-manage-vms/delete-button.png)

    Ilmoitus kertoo varmuuskopiotiedot on poistettu.


## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja luomisesta uudelleen virtual koneen palautus pisteestä Tutustu [Palauttaa Azure VMs](backup-azure-restore-vms.md). Jos haluat lisätietoja siitä, että näennäiskoneiden suojaaminen, katso [ensin kohde: Varmuuskopioi VMs palautus Services säilö](backup-azure-vms-first-look-arm.md). Lisätietoja tapahtumien seuranta on artikkelissa [Azure virtual machine-varmuuskopiot ilmoitusten näyttö](backup-azure-monitor-vms.md).
