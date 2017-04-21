

Voit rekisteröidä push-ilmoitukset – Apple Push ilmoituksen Service (APN)-sovellukseen, sinun on luotava uusi push-varmenne, Sovellustunnus ja valmistelu profiilin projektin Applen developer-portaalissa. Sovelluksen tunnus sisältää asetukset, joiden avulla voit lähettää ja vastaanottaa push-ilmoitukset sovelluksen. Nämä asetukset sisällytetään push-ilmoituksen varmenteen kanssa Apple Push ilmoituksen Service (APN) Kun lähetät ja vastaanotat push-ilmoitukset tarkistamiseen tarvitaan. Lisätietoja näistä asioista on virallinen [Apple Push-ilmoituspalvelu](http://go.microsoft.com/fwlink/p/?LinkId=272584) ohjeissa.


####<a name="generate-the-certificate-signing-request-file-for-the-push-certificate"></a>Luo push-sertifikaatin sertifikaatin allekirjoitus pyytäminen-tiedosto

Näin käy läpi luominen sertifikaatin kirjautumispyyntöön. Tämä käytetään push-varmenne, jota käytetään APN luomiseen.

1. Mac-tietokoneeseen Suorita Avainnipun käyttö-työkalu. Se voidaan avata **Apuohjelmat** -kansiosta tai julkaisun Pad **toiseen** kansioon.

2. Valitse **Avainnipun käyttö**, laajenna **Varmenteen tilan**ja valitse sitten **... varmenteiden myöntäjältä sertifikaatin**.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-request-cert-from-ca.png)

3. Valitse **Käyttäjän sähköpostiosoite** ja **Kutsumanimi** , varmista, että **tallennettu levylle** on valittuna ja valitse sitten **Jatka**. Jätä **CA sähköpostiosoite** -kenttä tyhjäksi, kun sitä ei tarvita.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-csr-info.png)

4. Kirjoita varmenteen allekirjoitus pyytää (CSR)-tiedoston nimi **Tallenna nimellä**, valitse sijainti- **kohtaa, johon**ja valitse sitten **Tallenna**.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-save-csr.png)

    CSR-tiedosto tallennetaan valitussa kansiossa; oletussijainti on työpöytä. Muista valinnut tämän tiedoston sijainti.


####<a name="register-your-app-for-push-notifications"></a>Rekisteröi sovelluksen push-ilmoitukset

Luo eksplisiittinen Sovellustunnus sovelluksen Apple ja määritä se myös push-ilmoitukset.  

1. Siirry [iOS Provisioning Portal](http://go.microsoft.com/fwlink/p/?LinkId=272456) Apple Developer Centerissä, kirjaudu sisään Apple-Tunnuksella, **tunnisteita**, valitse sitten Valitse **Sovelluksen tunnukset**ja valitse lopuksi **+** Kirjaudu Rekisteröi uusi sovellus.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-ios-appids.png)

2. Päivitä seuraavat kolme kenttää, kun uusi sovellus ja valitse sitten **Jatka**:

    * **Nimi**: Kirjoita sovelluksen nimi **nimi** -kenttään **Sovelluksen tunnus kuvaus** -osassa.

    * **Pikaoppaista tunnus**: **Eksplisiittinen Sovellustunnus** -kohdassa Määritä **Pikaoppaista tunnus** lomakkeen `<Organization Identifier>.<Product Name>` [Sovelluksen jakauman oppaassa](https://developer.apple.com/library/mac/documentation/IDEs/Conceptual/AppDistributionGuide/ConfiguringYourApp/ConfiguringYourApp.html#//apple_ref/doc/uid/TP40012582-CH28-SW8)mainitut. Tämä on vastattava mitä käytetään myös XCode, Xamarin tai Cordova Projectissa, kun sovellus.

    * **Push-ilmoitukset**: **Push-ilmoitukset** vaihtoehto **Sovelluksen palvelut** -osassa.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-new-appid-info.png)

3.  Valitse Vahvista Sovellustunnus näytön Tarkista asetusta ja sen jälkeen, kun olet tarkistanut ne valitsemalla **Lähetä**

4.  Kun olet lähettänyt uuden Sovellustunnus- **rekisteröityminen on valmis** -näyttö tulee näkyviin. Valitse **Valmis**.

5. Etsi juuri luonut, ja valitse sen rivin Sovellustunnus Developer Centeriin, valitse sovelluksen tunnukset. Napsauttamalla sovelluksen tunnus rivillä näkyy sovelluksen tiedot. Napsauta alareunassa **Muokkaa** -painiketta.

6. Vieritä näytön alareunaan ja valitse **Luo sertifikaatti...** -painikkeen kohdassa **Development Push SSL-varmenne**.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-appid-create-cert.png)

    Tässä näkyvät "Lisää sertifikaatti iOS" avustajan.

    > [AZURE.NOTE] Tässä opetusohjelmassa käyttää kehittäminen varmennetta. Samoja ohjeita käytetään, kun rekisteröiminen tuotannon varmenne. Varmista vain käyttää varmenteen samantyyppisten ilmoituksia lähetettäessä.

7. **Valitse**tiedosto, etsi selaamalla sijainti, johon tallensit CSR push-sertifikaatin. Valitse **Luo**.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-appid-cert-choose-csr.png)

8. Kun varmenne on luotu portaali, napsauttamalla **Lataa** -painiketta.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-appid-download-cert.png)

    Tämä Lataa allekirjoitusvarmenteen ja tallentaa sen tietokoneeseesi tiedostot-kansiossa.

    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-cert-downloaded.png)

    > [AZURE.NOTE] Oletusarvon mukaan ladattu tiedosto kehittäminen varmenteen nimi on **aps_development.cer**.

9. Kaksoisnapsauta ladatut push-varmenteen **aps_development.cer**. Tämä toiminto asentaa uutta varmennetta avainnippuun, alla kuvatulla tavalla:

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-cert-in-keychain.png)

    > [AZURE.NOTE] Sertifikaatin nimi voi olla erilainen, mutta se etuliite **kehittäminen Apple iOS Push-palveluiden:**.

10. Napsauta hiiren kakkospainikkeella Avainnipun käyttö **Varmenteet** -luokan juuri luomasi uusi push-sertifikaatti. Valitse **Vie**, nimeä tiedosto, valitse **.p12** muoto ja valitse sitten **Tallenna**.

    Muista tiedostonimen ja sijainnin viedyn .p12 push-sertifikaatti. Sitä käytetään käyttöön APN todentaminen lataamalla se perinteinen Azure-portaalissa.



####<a name="create-a-provisioning-profile-for-the-app"></a>Sovelluksen valmistelu profiilin luominen

1. Valitse takaisin sisään <a href="http://go.microsoft.com/fwlink/p/?LinkId=272456" target="_blank">iOS Provisioning Portal</a> **Valmistelu profiilit**, valitse **kaikki**ja valitse sitten **+** Luo uusi profiili-painiketta. Tämä käynnistää ohjatun **Lisää iOS Provisiong profiili**

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-new-provisioning-profile.png)

2. Tyypiksi **iOS sovellusten kehittämiseen** **kehitteillä** provisiong profiili ja valitse **Jatka**.


3. Seuraavaksi luomaasi **Sovellustunnus** avattavasta luettelosta Sovellustunnus ja **Jatka** sitten

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-select-appid-for-provisioning.png)


4. **Valitse varmenteet** -näytössä Valitse koodin allekirjoittamiseen käytettävän kehittäminen varmenne ja valitse **Jatka**. Tämä on allekirjoitusvarmenteen ei juuri luomasi push-sertifikaatti.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-provisioning-select-cert.png)


5. Seuraavaksi testikäyttöön **laitteissa** , ja valitse **Jatka**

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-provisioning-select-devices.png)


6. Valitse profiilin nimi **Profiilinimi**-, valitse **Luo**.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-provisioning-name-profile.png)
