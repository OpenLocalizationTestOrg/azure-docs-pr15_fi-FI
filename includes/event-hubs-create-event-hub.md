## <a name="create-an-event-hub"></a>Luo tapahtumaa-toiminnossa

1. Kirjaudu sisään [Azure portal][]ja sitten **Uusi** yläreunassa näytön vasemmassa.

2. Valitse **tietojen + Analytics**ja valitse sitten **Tapahtuma keskittimet**.

    ![](./media/event-hubs-create-event-hub/create-event-hub9.png)

3. Kirjoita **Luo nimitilan** , sivu nimitilan nimi. Järjestelmä tarkistaa heti, jos nimi on käytettävissä.

    ![](./media/event-hubs-create-event-hub/create-event-hub1.png)

4. Kun varmistetaan, että nimitilan nimi on käytettävissä, valitse hinnoittelu tason (Basic tai vakio). Lisäksi Azure tilauksen, resurssiryhmä ja sijainti, johon voit luoda resurssin valitseminen. 

2. Valitse **Luo** nimitilan luomiseen.

6. Valitse tapahtuma keskittimet nimitilan-luettelosta luomasi nimitilan.      

    ![](./media/event-hubs-create-event-hub/create-event-hub2.png)

7. Valitse **Tapahtuma keskittimet**nimitilan-sivu.

    ![](./media/event-hubs-create-event-hub/create-event-hub3.png)

8. Yläreunassa olevalla sivu napsauttamalla **Lisää tapahtumaa-toiminnossa**.

    ![](./media/event-hubs-create-event-hub/create-event-hub4.png)

3. Tapahtuma-toiminnossa nimi ja valitse sitten **Luo**.

    ![](./media/event-hubs-create-event-hub/create-event-hub5.png)

4. Valitse tapahtuma keskittimet luettelo on juuri luomasi tapahtuman keskittimeen nimi. 

    ![](./media/event-hubs-create-event-hub/create-event-hub6.png)

5. Nimitilan sivu (ei tiettyyn tapahtumaa-toiminnossa-sivu)-kohdassa **jaettu käytäntöjen**ja valitse sitten **RootManageSharedAccessKey**.

    ![](./media/event-hubs-create-event-hub/create-event-hub7.png)

5. **RootManageSharedAccessKey** yhteysmerkkijonon Kopioi Leikepöydälle valitsemalla Kopioi. Tallenna tämän opetusohjelman myöhemmin käyttäminen yhteysmerkkijonon.

    ![](./media/event-hubs-create-event-hub/create-event-hub8.png)

Tapahtuma-toiminnossa on nyt luotu ja sinulla on yhteysmerkkijonot haluat lähettää ja vastaanottaa tapahtumat.

[Azure portal]: https://portal.azure.com/