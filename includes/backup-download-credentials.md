## <a name="using-vault-credentials-to-authenticate-with-the-azure-backup-service"></a>Säilö tunnistetiedoilla tarkistamiseen Azure varmuuskopiointi-palvelussa

Paikallisen-palvelimessa (Windows-asiakasohjelman tai Windows Server tai tietojen suojauksen hallinta) on voi todentaa varmuuskopion säilöön, ennen kuin se voit varmuuskopioida tiedot Azure. Todennus saavutetaan käyttämällä "säilö tunnistetietojen". Käsitteellä "Julkaisuasetukset-tiedosto, jossa käytetään PowerShellin Azure säilö tunnistetietoja joka muistuttaa.

### <a name="what-is-the-vault-credential-file"></a>Mikä on säilö tunnistetiedon-tiedosto?

Säilö tunnistetiedot-tiedosto on kunkin varmuuskopion säilö portaalin luoma varmenne. Portaalin lataa sitten julkisella avaimella, Access ohjausobjektin Service (ACS). Varmenteen yksityinen avain on saatavana osana työnkulun, joka on määritetty tietokoneen rekisteröinti työnkulun syötteeksi käyttäjä. Tämä todentaa koneen varmuuskopiotiedot lähettäminen tunnistettujen säilöön, Azure varmuuskopio-palvelussa.

Säilö tunnistetieto käytetään vain rekisteröinti työnkulun aikana. On käyttäjän varmistamisesta, että säilö tunnistetiedot-tiedostoa ei käsiin. Jos se osuu päivittämättömien kuka tahansa käyttäjä kädet, tiedosto säilöön tunnistetiedot voidaan rekisteröidä muiden laitteiden vastaan samaan säilö. Kuitenkin kuin varmuuskopiotiedot on salattu salasana, johon kuuluu asiakkaalle, ei käsiin aiemmin palautettavat tiedot. Tämä huolta pienentämään säilö tunnistetiedot on määritetty päättyy 48hrs. Voit ladata varmuuskopiointi säilö säilö tunnistetiedot minkä tahansa määrän kertoja –, mutta vain uusimmat säilö tunnistetiedon tiedosto on käytettävissä rekisteröinti työnkulun aikana.

### <a name="download-the-vault-credential-file"></a>Lataa tiedosto säilöön tunnistetiedon

Säilö tunnistetiedon tiedosto ladataan Azure-portaalista suojatun kanavan kautta. Azure varmuuskopiointi-palvelu ei tunnista sertifikaatin yksityinen avain ja yksityinen avain ei säilytetä portaalin tai palvelu. Seuraavien vaiheiden avulla voit ladata säilöön tunnistetiedon tiedoston paikalliseen tietokoneeseen.

1.  Kirjautuminen [hallinta-portaalissa](https://manage.windowsazure.com/)
2.  Napsauta **Palautus** Services vasemmassa siirtymisruudussa ja valitse varmuuskopioinnin säilöön, jotka olet luonut. Valitse pilvikuvaketta voit siirtyä varmuuskopion säilö Pika-aloitus-näkymään.

    ![Pikakatselu](./media/backup-download-credentials/quickview.png)

3.  Napsauta Pika-aloitus-sivulla **Lataa säilö tunnistetiedot**. Portaalin Luo säilöön tunnistetiedon tiedosto, joka tehdään ladattavaksi.

    ![Lataa](./media/backup-download-credentials/downloadvc.png)

4.  Portaalin Luo säilöön-tunnistetietojen avulla yhdistelmä säilö nimi ja nykyisen päivämäärän. **Tallenna** säilöön-tunnistetietojen lataamisesta paikallisen tilin tiedostot-kansiossa tai valitse Määritä sijainti säilö tunnistetietoja Tallenna-valikon Tallenna nimellä.

### <a name="note"></a>Huomautus
- Varmista, että säilö käyttäjätiedot tallennetaan sijaintiin, jossa niitä voi käyttää tietokoneesta. Jos se on tallennettu tiedosto Jaa/SMB, Tarkista käyttöoikeudet.
- Säilö tunnistetietojen tiedostoa käytetään vain rekisteröinti työnkulun aikana.
- Säilö tunnistetietojen tiedoston 48hrs vanhentuu ja voi ladata portaalin.
- Lisätietoja työnkulun kysymykset Azure varmuuskopiointi- [usein kysytyt kysymykset](../articles/backup/backup-azure-backup-faq.md) .
