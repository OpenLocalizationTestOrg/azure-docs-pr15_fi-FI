
1. (Tämä on vanha portaalin) [Azure hallinta-portaalin](http://manager.windowsazure.com) kirjautua paikalliseen tietokoneeseen.

2. Valitse siirtymisruudun alareunassa **+ Uusi** > **Sovelluksen palvelut** > **BizTalk palvelun** > **Mukautettu luominen**.

3. **BizTalk palvelunimi** ja valitse **Edition**. 

    Tässä opetusohjelmassa käytetään **mobile1**. Sinun on lisättävä uusi BizTalk palvelu yksilöllinen nimi.

4. Kun BizTalk-palvelu on luotu, valitse **Hybrid yhteydet** -välilehti ja valitse sitten **Lisää**.

    ![Hybrid yhteyden lisääminen](./media/hybrid-connections-create-new/3.png)

    Tämä luo uuden hybrid yhteyden.

5. Anna **nimi** ja **Isäntänimi** hybrid-yhteyden ja määritä **portin** `1433`. 
  
    ![Hybrid-yhteyden määrittäminen](./media/hybrid-connections-create-new/4.png)

    Isäntänimi on paikallisen palvelimen nimi. Tämä määrittää käyttämään SQL Serveriä porttia 1433 hybrid-yhteys. Jos käytössäsi on nimetty SQL Server ‑esiintymä, käytä sen sijaan aiemmin määritetty staattinen portti.

6. Kun uusi yhteys on luotu, tila uusi yhteyden näyttää **paikallisen asennuksen puutteellinen**.

7. Palata mobile-palvelu, **Määritä**, vieritä **Hybrid yhteydet** ja valitsemalla **Lisää hybrid yhteys**, valitse juuri luomasi hybrid yhteys ja valitse **OK**.

    Näin mobile-palvelu uusi hybrid-yhteyttä.

Seuraavaksi tarvitset Hybrid Yhteyksienhallinnan asentaminen paikalliseen tietokoneeseen.