<!--author=SharS last changed: 01/15/2016-->

#### <a name="to-install-update-12-from-the-azure-classic-portal"></a>Asenna päivitys 1.2 perinteinen Azure-portaalista

1. StorSimple palvelun sivulla Valitse laite. Siirry **laitteet** > **ylläpito**.

2. Sivun, valitse **Tarkista päivitykset**. Työn luodaan päivityksiä saatavilla. Saat ilmoituksen, kun työ on valmis.

3. Samalla sivulla **Ohjelmistopäivitykset** -osassa näkyy, että uusi ohjelmistopäivitykset ovat käytettävissä. On suositeltavaa, että luet julkaisutiedot, ennen kuin asennat päivityksen 1.2 laitteessasi.

    ![Asenna saatavilla olevat ohjelmistopäivitykset](./media/storsimple-install-update-via-portal/InstallUpdate12_11M.png)

4. Sivun, valitse **Asenna päivitykset**.

5. Voit pyydetään vahvistamaan. Valitse **OK**.

6. **Asenna päivitykset** -valintaikkuna avautuu. Laitteen on täytettävä tässä valintaikkunassa tarkistukset. Nämä vaiheet on suoritettu ennen päivitystä. Valitse **voin yllä vaatimus ymmärtäminen ja päivittäminen laitteeseeni voi alkaa**. Valitse Tarkista-kuvake.

    ![Vahvistussanoman](./media/storsimple-install-update-via-portal/InstallUpdate12_2M.png)

7. Automaattinen vanhat tarkistukset joukko nyt alkaa. Näitä ovat:

    - Varmista, että laitteen-ohjaimet ovat kunnossa ja online **ohjauskoneen kuntotietojen tarkistukset** .
    
    - Varmista, että kaikki StorSimple laitteen laitteita ovat kunnossa **laitteiston osan kuntotietojen tarkistukset** .
    
    - Varmista, että tietojen 0 on käytössä laitteen **tiedot 0 tarkistaa** . Jos tätä liittymää ei ole otettu käyttöön, sinun on ottaa sen käyttöön ja yritä sitten uudelleen.
    
    - Varmista, että tietojen 2 ja 3 tietojen verkkoliittymät eivät ole käytössä **tietojen 2 ja 3 tietojen tarkistaa** . Jos nämä liityntäkohdat ovat käytössä, valitse tarvitset poistaa ne käytöstä ja yritä päivittää laitteen. Tässä tarkistuksessa suoritetaan vain, jos päivität GA käyttö laitteesta. Laitteet, joissa versiot 0,1, 0,2 tai 0,3 ei on tämä tarkistus.
    
    - **Tarkista yhdyskäytävän** millä tahansa laitteella käynnissä päivitys 1: a vanhemmalla versiolla. Tässä tarkistuksessa suoritetaan kaikki päivitystä edeltävässä 1 käyttö laitteeseen, mutta epäonnistuu laitteissa, joissa on määritetty verkko-liittymän kuin tietojen 0 yhdyskäytävä.
 
    Päivitä 1.2 käytetään vain, jos edellä päivitystä edeltävässä tarkistukset on suoritettu. Saat ilmoituksen, että päivitystä edeltävässä tarkistukset ovat käynnissä.
  
    ![Tarkista valmiiksi ilmoitus](./media/storsimple-install-update-via-portal/InstallUpdate12_3M.png)

    Seuraavassa on esimerkki, jossa vanhasta tarkistus epäonnistui. Sinun on varmistaa, että laitteen-ohjaimet ovat kunnossa ja online-tilassa. Sinun on myös laitteita kuntotietojen tarkistus. Tässä esimerkissä Controller 0 ja 1 ohjauskoneen osia tarvitse toimenpiteitä. Joudut ehkä ottaa yhteyttä Microsoftin Support, jos ei korjaa ongelmat itse.

     ![Tarkista valmiiksi epäonnistui](./media/storsimple-install-update-via-portal/HCS_PreUpgradeChecksFailed-include.png)

    > [AZURE.NOTE] Kun olet asentanut päivityksen 1.2 StorSimple laitteen, tietojen 2 ja 3 tietojen tarkistus ja yhdyskäytävä-valintaruudun voi tarvittavat tulevia päivityksiä varten. Muut vanhat tarkastukset on pakollinen.


8. Kun vanhasta tarkistukset on suoritettu onnistuneesti, päivitystyö luodaan. Saat ilmoituksen, kun päivitystyö on luotu.
 
    ![Päivitä työpaikkojen](./media/storsimple-install-update-via-portal/InstallUpdate12_44M.png)

    Päivityksen käytetään laitteessasi.
 
9. Voit valvoa päivityksen työn edistymistä, valitsemalla **Näytä työ**. Valitse **Projektit** -sivulla näet edistymisen päivittäminen. 

    ![Projektin edistymisen päivittäminen](./media/storsimple-install-update-via-portal/InstallUpdate12_5M.png)

10. Päivittäminen kestää muutaman tunnin suorittamiseen. Voit tarkastella projektin tietoja milloin tahansa.

    ![Työn tietojen päivittäminen](./media/storsimple-install-update-via-portal/InstallUpdate12_6M.png)

11. Kun työ on valmis, **ylläpito** -sivulle ja vieritä **Ohjelmistopäivitykset**.

12. Varmista, että laite toimii **StorSimple 8000 sarjan päivitys 1.2 (6.3.9600.17584)**. **Päivitetty viimeksi päivämäärän** myös kannattaa muokata.

    ![Ylläpitosivun](./media/storsimple-install-update-via-portal/InstallUpdate12_10M.png)

13. Nyt näet, että ylläpidon tila-päivitykset ovat käytettävissä. Nämä päivitykset ovat häiritsevä päivityksiä, jotka aiheuttavat laitteen käyttökatkot ja voi käyttää vain laitteesi Windows PowerShell-liittymän kautta. Noudata [ylläpidon tila-päivitysten asentaminen](storsimple-update-device.md#install-maintenance-mode-updates-via-windows-powershell-for-storsimple) , asenna nämä päivitykset Windows PowerShellin kautta StorSimple.

> [AZURE.NOTE] Joissakin tapauksissa sanoma ylläpidon tila-päivitykset ovat saatavana saattaa näkyä määrittäminen 24 tuntia, kun ylläpidon tila-päivitykset on otettu käyttöön onnistuneesti laitteeseen.  


