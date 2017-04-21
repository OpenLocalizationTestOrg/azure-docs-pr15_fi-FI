Voit luoda Azure tallennustilan olevien Visual Studio **Palvelimen Explorerin**avulla.

![Palvelimen Explorer BLOB-objektit][Image1]

1. Valitse **Näytä** -valikosta **Server Explorer**.
2. Palvelimen Resurssienhallinnassa Laajenna tilauksen **Azure** -solmu, laajenna **tallennustilan** -solmu ja Azure-tallennustilan yhdistetty service-määritetyn tallennustilan tilin solmu.
3. Valitse **olevien** -solmu ja valitse pikavalikosta **Jonon luomisen** .
4. Säilön nimi ja valitse **OK**.   

Oletusarvon mukaan uusi säilö on yksityinen ja sinun on määritettävä tallennustilan access-näppäintä, voit ladata tämän säilön BLOB-objektit. Jos haluat julkisiksi säilö tiedostot, valitse säilön **Palvelimen Explorerissa** ja paina `F4` näytettävä **Ominaisuudet** -ikkuna. Määritä **julkinen lukuoikeudet** **Blob**. Kuka tahansa Internetissä näkevät yleisen säilön BLOB, mutta voit muokata tai poistaa ne vain, jos sinulla on riittävät käyttöoikeudet-näppäintä.


[Image1]: ./media/vs-create-blob-container-in-server-explorer/vs-storage-create-blob-containers-in-Server-Explorer.png