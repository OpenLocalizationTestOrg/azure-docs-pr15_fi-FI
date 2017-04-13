## <a name="defining-a-backup-policy"></a>Varmuuskopion käytännön määrittäminen

Varmuuskopiointi-käytännöllä määritetään, kun tietojen tilannevedosten otetaan ja kuinka kauan näiden tilannevedoksia säilyvät matriisi. Voit käynnistää varmuuskopiointityön *kerran päivässä*AM varmuuskopioiminen käytäntöä määritettäessä. Kun luot uuden käytännön, se otetaan käyttöön säilö. Varmuuskopion käytäntö-käyttöliittymä näyttää seuraavanlaiselta:

![Varmuuskopion käytäntö](./media/backup-create-policy-for-vms/backup-policy.png)

Käytännön luominen:

1. Kirjoita nimi **käytännön nimi**.

2. Tietojen tilannevedosten voidaan ottaa päivittäin tai viikoittain väliajoin. **Varmuuskopiointi korkojakso** avattavasta valikosta avulla voit valita tietojen tilannevedosten päivittäin tehdyt tai viikoittain.

    - Jos valitset päivittäin, valitse kellonaika näyttökuvan korostetun ohjausobjektin avulla. Voit muuttaa tunnin, poista valinta tunnin ja valitse uusi tunti.

    ![Päivittäinen varmuuskopiointi käytäntö](./media/backup-create-policy-for-vms/backup-policy-daily.png) <br/>

    - Jos valitset Viikoittain, valitse viikonpäivät ja kellonaika toteuttamaan tilannevedoksen korostetun ohjausobjektien avulla. Valitse useita päiviä päivä-valikossa. Valitse tunnin tunti-valikossa. Voit muuttaa tunnin, poista valinta valitun tunti ja valitse uusi tunti.

    ![Viikoittainen varmuuskopiointi käytäntö](./media/backup-create-policy-for-vms/backup-policy-weekly.png)

3. Kaikki **Säilytys alueen** asetukset ovat oletusarvoisesti valittuna. Poista kaikki säilytys alueen yläraja et halua käyttää. Määritä käytettävä interval(s).

    Kuukausittain ja vuosittain säilytys alueiden avulla voit määrittää tilannevedoksia, viikoittain tai päivittäin lisäys perusteella.

    >[AZURE.NOTE] Kun suojaat AM, varmuuskopiointityön suoritetaan kerran päivässä. Kun varmuuskopiointi suoritetaan aika on sama säilytys kunkin alueen.

4. Kaikki vaihtoehdot käytännön määritettyäsi yläreunaan sivu valitsemalla **Tallenna**.

    Uuden käytännön mukautuu automaattisesti säilö.
