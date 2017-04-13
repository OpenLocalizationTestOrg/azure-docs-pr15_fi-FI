## <a name="download-install-and-register-the-azure-backup-agent"></a>Lataa, asenna ja rekisteröi Azure Backup agent

Kun olet luonut Azure varmuuskopiointi säilö, agentti on asennettava kunkin oman Windows koneet (Windows Server, Windows-asiakas, System Center tietojen suojauksen hallinta server tai Azure varmuuskopiointi palvelimen), joka mahdollistaa Varmuuskopioi tiedot ja Azure-sovellukset.

1. Kirjautuminen [hallinta-portaalissa](https://manage.windowsazure.com/)

2. Valitse **Palautus palvelut**ja valitse sitten varmuuskopion säilö, jonka haluat rekisteröidä palvelimen kanssa. Kyseisen varmuuskopion säilö Pika-aloitus-sivu tulee näkyviin.

    ![Pika-aloitusopas](./media/backup-install-agent/quickstart.png)

3. Pika-aloitus-sivulla **Lataa Agent**-kohdasta **Windows Server- tai System Center tietojen suojauksen hallinta- tai Windows-asiakas** -vaihtoehto. Valitse kopioitavat Paikallinen kone **Tallenna** .

    ![Tallenna agentti](./media/backup-install-agent/agent.png)

4. Kun agentti on asennettu, kaksoisnapsauta MARSAgentInstaller.exe käynnistää Azure Backup agent asennusta. Valitse asennuksen kansio ja agentti vaaditaan työtilan kansion. Välimuistin määritetyssä on oltava tilaa, jossa on vähintään 5 prosenttia palautettavat tiedot.

5.  Jos käytät välityspalvelinta internet,-yhteys **välityspalvelimen määritys** -näytössä ja anna välityspalvelimen palvelimen tiedot. Jos käytät edellyttävän välityspalvelimen kautta, kirjoita käyttäjän nimi ja salasana tiedot tämän näytön.

6.  Azure Backup agent asentaa .NET Framework 4.5 ja Windows PowerShellin (jos se ei ole käytettävissä jo) ja viimeistele asennus.

7.  Kun agentti on asennettu, napsauta Jatka työnkulun **rekisteröintiä Jatka** -painiketta.

    ![Rekisteröidy](./media/backup-install-agent/register.png)

8. Tunnistetietojen säilöön-näytön selaamalla ja valitse säilöön tunnistetiedot tiedosto, jonka aiemmin ladattu.

    ![Säilö tunnistetiedot](./media/backup-install-agent/vc.png)

    Säilö tunnistetiedot-tiedosto on voimassa vain 48 tuntia (sen jälkeen, kun se ladataan-portaalista). Jos ilmenee tämän näytössä (esimerkiksi "säilö tunnistetietojen tiedosto antaa on päättynyt"), kirjaudu sisään Azure-portaaliin virheiden ja lataa tiedosto säilöön tunnistetiedot uudelleen.

    Varmista, että säilö tunnistetiedot-tiedosto on käytettävissä sijainnissa, joita voit käyttää sovelluksen. Jos käytössä ilmenee käyttää liittyvät virheet, säilö tunnistetiedot-tiedoston kopioiminen tämän tietokoneen tilapäinen sijainti ja yritä uudelleen.

    Jos saat virheilmoituksen virheellisestä säilö tunnistetiedon (esimerkiksi "Virheellinen säilö tunnistetietojen annettu") tiedosto on vahingoittunut tai ei ole on uusin tunnistetiedot palautus-palveluun liitetyn. Yritä jälkeen uusi säilö tunnistetiedon tiedoston lataamisen-portaalista. Tämä virhe yleensä tarkastella, jos käyttäjä valitsee Azure-portaalissa nopeasti peräkkäin **Lataa säilö tunnistetiedot** -asetus. Tässä tapauksessa vain toinen säilö tunnistetiedon tiedosto on voimassa.

9. **Salauksen asetukset** -näytön Luo salasana tai Anna salasana (vähintään 16 merkkiä). Muista tallentaa salasana turvalliseen paikkaan.

    ![Salaus](./media/backup-install-agent/encryption.png)

    > [AZURE.WARNING] Jos salasana katoaa tai unohtanut; Microsoft voi auttaa palauttamisessa palautettavat tiedot. Peruskäyttäjän omistaa salauksen salasana ja Microsoft ei voi käyttää käyttäjän salasana näkyvyys. Tallenna tiedosto turvalliseen paikkaan, kun se on tarpeen palautumistoiminnon aikana.

10. Kun valitset **Valmis** -painike, tietokoneen on rekisteröity onnistuneesti säilö ja voit nyt jatkaa Microsoft Azure varmuuskopion.

11. Microsoft Azure varmuuskopiointi erillinen käytettäessä voit muokata asetuksissa määritettyä rekisteröinti työnkulun aikana valitsemalla Azure varmuuskopiointi mmc kohdistuksen **Ominaisuuksien muuttaminen** -asetus.

    ![Ominaisuuksien muuttaminen](./media/backup-install-agent/change.png)

    Voit myös käytettäessä tietojen suojauksen hallinta, voit muokata asetuksissa määritettyä rekisteröinti työnkulun aikana valitsemalla valitsemalla **online-palvelun** **hallinta** -välilehden **Määritä** -asetus.

    ![Määritä Azure varmuuskopiointi](./media/backup-install-agent/configure.png)
