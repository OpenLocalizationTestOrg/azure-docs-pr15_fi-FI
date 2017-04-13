
1. **Hybrid yhteydet** -sivu: Valitse juuri luomasi hybrid yhteys ja valitse sitten **Listener asetukset**.
    
    ![Listener asetukset](./media/app-service-hybrid-connections-manager-install/D04ClickListenerSetup.png)
    
4. **Hybrid yhteyden ominaisuudet** -sivu avautuu. **Paikallisen Hybrid Yhteyksienhallinnan**-Valitse **Lataa ja määrittää manuaalisesti**, tallentaa ladatut HybridConnectionManager.msi paketin ja kopioi yhdyskäytävän yhteysmerkkijonon.
    
    ![Asenna napsauttamalla tätä](./media/app-service-hybrid-connections-manager-install/D05ClickToInstallHCM.png)
    
5. Järjestelmänvalvojan komentoriviltä kirjoittamalla Käynnistä asennusohjelma seuraava komento:

        start HybridConnectionManager.msi
 
7. Asennusohjelma suoritetaan, kun valitse **ei Kiitos**, ja valitse %ProgramFiles%\Microsoft\HybridConnectionManager kansio, suorita HCMConfigWizard.exe ja ** **Käyttäjätilien valvonta** -valintaikkunassa Kyllä** .
        
7. Liitä hybrid yhteysmerkkijonon, joka on kopioitu aiemmin ja valitse **OK**. 
    
    ![Asentaminen](./media/app-service-hybrid-connections-manager-install/D08aHCMInstallManual.png)
    
8. Kun asennus on valmis, valitse **Sulje**.
    
    ![Sulje](./media/app-service-hybrid-connections-manager-install/D09HCMInstallComplete.png)
    
    **Hybrid yhteydet** -sivu, valitse **tila** -sarakkeessa näkyy nyt **yhdistetty**. 
    
    ![Yhdistetyn tila](./media/app-service-hybrid-connections-manager-install/D10HCStatusConnected.png)