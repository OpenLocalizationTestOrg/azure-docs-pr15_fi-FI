
1. Napsauta hiiren kakkospainikkeella Visual Studio ratkaisunhallinnassa Windows-kaupan sovelluksen projektin, valitse **Tallenna** > **Liittää App Store...**.

    ![Windows-kaupan sovelluksen liittäminen](./media/app-service-mobile-register-wns/notification-hub-associate-win8-app.png)

2. Ohjatun Valitse **Seuraava**, kirjaudu sisään käyttämällä Microsoft-tiliä, kirjoita sovelluksesi nimi **Varaa uuden sovelluksen nimi**ja valitse sitten **Varaa**.

3. Sen jälkeen, kun sovellus rekisteröinti on onnistunut, uuden sovelluksen nimi, valitse **Seuraava**ja valitse sitten **Liitä**. Tämä lisää tarvittavat Windows-kaupan rekisteröintitietoja sovelluksen luettelo.

7. Toista vaiheet 1 – 3 aiemmin luotu saman rekisteröinnin käyttäminen Windows-kaupan sovellus Windows Phone-kaupan sovelluksen projektin.  

7. Siirry [Windows keskihajonta Center](https://dev.windows.com/en-us/overview), kirjaudu sisään Microsoft-tiliä, valitse uusi sovellus rekisteröinti, **sovellusten**ja sitten Laajenna **palvelut** > **Push-ilmoitukset**.

8. **Push-ilmoitukset** -sivulla **Windows Push-ilmoituksen Services (WNS) ja Microsoft Azure-mobiilisovellukset** **Live-palveluiden sivuston** kohdasta ja Merkitse muistiin **Paketin SUOJAUSTUNNUS** arvot ja *nykyisen* arvon- **Sovelluksen toiminta**. 

    ![Developer center-asetus](./media/app-service-mobile-register-wns/mobile-services-win8-app-push-auth.png)

    > [AZURE.IMPORTANT] Sovelluksen salaisuus ja pakkaus SUOJAUSTUNNUS ovat tärkeitä suojausvaltuudet. Älä jaa nämä arvot kenen kanssa tai jakaa ne sovelluksen.
