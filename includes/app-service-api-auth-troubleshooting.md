Useimmat aika todennusta virheet johtuvat virheellinen tai epäyhtenäinen asetukset. Seuraavassa on tiettyjä toimia tavalla, voit tarkistaa.

* Varmista, että kaipaat ei **Tallenna** -painiketta missä tahansa. Tämä on usein helppoa ja tulos on etsit on oikeat arvot portaalin sivulla, mutta ne todella et vielä tallennettu Azure ympäristön tai Azure AD-sovelluksessa.
* Määritettyjen Azure-portaalin **Sovelluksen asetukset** -sivu Varmista, että oikea API-sovelluksen tai web Appissa on valittuna, kun asetukset on lisätty.  Varmista myös, että asetukset on lisätty **sovelluksen asetusten** ja ei **yhteyden merkkijonoja**, kaksi osaa muoto on samanlainen kuin.
* Todennustavaksi JavaScript-edusta-lataaminen uudelleen ja varmista, luettelo `oauth2AllowImplicitFlow` on vaihdettu `true`.
* Varmista, että olet käyttänyt HTTPS aina, kun olet määrittänyt URL-osoitteet:

    * Project-koodissa
    * Valitse CORS
    * Azure-ympäristön sovelluksen asetuksissa kunkin API-sovellus ja web Appissa
    * Azure AD-sovelluksen asetukset.
    
    Huomaa, että jos kopioit API-sovelluksen URL-osoite-portaalista, usein `http://` ja sinun on muutettava manuaalisesti sen `https://`.

* Varmista, että koodin muutokset on otettu. Usean projektin ratkaisussa on esimerkiksi mahdollista muuttaa projektin koodi ja valitse jokin muiden vahingossa, kun haluat ottaa muutokset käyttöön.
* Varmista, että aiot HTTPS URL-osoitteet selaimen HTTP URL. Visual Studio luo oletusarvoisesti julkaista käyttäjäprofiileissa HTTP URL-osoitteet, ja mitä avautuu selaimessa, kun otat käyttöön projektin.
* Todennustavaksi JavaScript-edusta-Varmista, että CORS on määritetty oikein, JavaScript-koodia soittaa API-sovellukseen. Jos olet epävarma siitä, onko ongelma CORS liittyvät, kokeile "*" sallittujen origin URL-osoitteena. 
* Avaa selaimen Developer Työkalut Console-välilehti, josta haluat lisätietoja Virhe JavaScript edusta- ja tarkastella pyyntöjen verkossa. Konsolin Virheilmoitukset voivat olla harhaanjohtavia. Jos saat CORS virhe-sanoma, reaali ongelma voi olla todennusta. Voit tarkistaa, jos se on kirjainkoon käynnistämällä sovelluksen käyttöoikeuksien tilapäisesti tilapäisesti käytöstä.
* .NET API-sovelluksen Varmista, että olet uudistamassa mahdollisimman paljon tietoja virhesanomissa mahdollisimman määrittämällä [customErrors tila ei ole käytössä](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#remoteview).
* .NET API-sovelluksen Aloita [muistin etäistunnon](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug)ja tutkia, jotka siirretään ADAL hankkia haltijatunnukseen käyttävän koodin tai koodi, joka tarkistaa saatavia odotettu palvelun pääasiallista tunnuksen muuttujan arvot Huomaa, että koodisi valita määritysten arvoja useista eri lähteistä, joten voi olla mahdollista löytää paljon tällä tavalla. Jos kirjoitat voit esimerkiksi `ida:ClientId` kuin `ida:ClientID` määritettäessä Azure App palvelun ympäristöasetuksia koodin saada `ida:ClientId` , se etsii seuraavan koodin korostetut, Azure App Service-asetus ohittaa-arvon. 
* Jos asiat eivät toimi Normaali Internet Explorer-ikkuna, aiemmin luotuun sisään saattaa aiheuttaa; InPrivate ja yritä Chrome- tai Firefox.
