

1. Mac-tietokoneeseen Käynnistä **Avainnipun käyttö**. Avaa **Oma varmenteet** **luokassa** vasemman navigationn olevasta palkista. Etsi lataamaasi edellisessä osassa SSL-varmenne ja esittää sen sisältöä. Valitse vain varmenne (Älä valitse Yksityinen avain), ja [Vie se](https://support.apple.com/kb/PH20122?locale=en_US).

2. [Azure-portaali](https://portal.azure.com/)valitsemalla **Selaa kaikki** > **Sovelluksen Services** > Mobile-sovelluksen Taustajärjestelmä. **Asetukset**-kohdassa Valitse **Sovelluksen palvelun Push**ja valitse sitten nimesi ilmoitus-toiminnossa. Siirry **Apple Push-ilmoituksen palvelujen** > **Lataa sertifikaatti**. Lataa .p12-tiedosto valitsemalla **tila** (sen mukaan, onko asiakkaan SSL-varmenteen kuin aiemmin on tuotannon tai eristetyn). Tallenna muutokset.

Palvelussa on nyt määritetty toimimaan push-ilmoitukset iOS!

[1]: ./media/app-service-mobile-apns-configure-push/mobile-push-notification-hub.png
