Azure asiakkaat voivat lukituksen 25,000 vapaa sähköpostit kuukausittain. Nämä 25,000 vapaa kuukausittain sähköpostit avulla voit käyttää Lisäasetukset raportoinnin ja analysoinnin ja [kaikki API][] (Web, SMTP-tapahtuman, jäsennä ja muiden tietojen). Lisätietoja SendGrid muita palveluja on artikkelissa [SendGrid ominaisuudet][] -sivulla.

### <a name="to-sign-up-for-a-sendgrid-account"></a>Jos haluat SendGrid tilin rekisteröiminen

1. Kirjaudu sisään [Azure hallinta-portaalin][].

2. Valitse hallinta-portaalin alaruudussa valitsemalla **Uusi**.

    ![komento-palkin uusi][command-bar-new]

3. Valitse **Marketplacesta**.

    ![sendgrid-säilö][sendgrid-store]

4. **Valitse sovellus- ja palveluluettelon** -valintaikkunassa valitse **SendGrid** ja valitse sitten oikealle osoittavaa nuolta.

5. Valitse **Mukauta sähköpostisovelluksen ja -palvelu** -valintaikkunan rekisteröitymään **SendGrid** haluamasi palvelupaketti.

6. Tunnistaa Azure asetusten **SendGrid** palvelun nimi tai käytä **SendGridEmailDelivery.Simplified.SMTPWebAPI**oletusarvon. Nimet täytyy olla väliltä 1 – 100 merkkiä ja sisältää vain aakkosnumeerisia merkkejä, katkoviivat, pisteet ja alaviivoja. Nimen on oltava yksilöllinen merkityn Azure säilön kohteiden luettelon.

    ![myymälä-näyttö-2][store-screen-2]

7. Valitse alue; arvo esimerkiksi Länsi US.

8. Napsauta sitten oikealle osoittavaa nuolta.

9. **Tarkista oston** -välilehden Tarkista suunnitelman ja hintatiedot ja juridiset ehdot. Jos hyväksyt ehdot, valitse valintamerkkiä. Kun valitset valintaruudun, SendGrid tilisi alkaa [SendGrid valmistelu prosessi].

    ![myymälä-näyttö-3][store-screen-3]

10. Ostotapahtuma vahvistamisen jälkeen sinut ohjataan lisäosia Raporttinäkymät-ikkunan ja näyttöön tulee sanoma **Ostaminen SendGrid**.

    ![sendgrid-ostaminen viesti][sendgrid-purchasing-message]

    SendGrid-tilisi on valmisteltu välittömästi ja **ostanut lisäosa SendGrid**sanoma tulee näkyviin. Tilin ja tunnistetiedot on nyt luotu. Olet valmis lähettämään sähköpostit tässä vaiheessa. 

    Tilauksen palvelupaketin muokkaamiseen tai Katso SendGrid Ota asetukset, valitse Avaa SendGrid Marketplace Raporttinäkymät-ikkunan SendGrid palvelun nimi. 

    ![sendgrid-Lisää-,-koontinäyttö][sendgrid-add-on-dashboard]

    Voit lähettää sähköpostia käyttämällä SendGrid, sinun on annettava tilin tunnistetiedot (käyttäjänimi ja salasana).

### <a name="to-find-your-sendgrid-credentials"></a>Voit etsiä SendGrid tunnistetiedot ###

1. Valitse **yhteyden tiedot**.

    ![sendgrid-yhteyden-tiedot-painike][sendgrid-connection-info-button]

2. Valitse *yhteyden tiedot* -valintaikkunassa kopioi **salasana** - ja käyttäjänimi käytetään jäljempänä tässä opetusohjelmassa.

    ![sendgrid-yhteyden-tiedot][sendgrid-connection-info]

    Jos haluat määrittää sähköpostin deliverability asetukset, valitse **hallinta** -painike. Tämä ohjaa SendGrid Ohjauspaneeli. 

    ![sendgrid--Ohjauspaneeli][sendgrid-control-panel]

    Lisätietoja SendGrid käytön aloittaminen-artikkelissa [SendGrid käytön aloittaminen][].

<!--images-->

[command-bar-new]: ./media/sendgrid-sign-up/sendgrid_BAR_NEW.PNG
[sendgrid-store]: ./media/sendgrid-sign-up/sendgrid_offerings_store.png
[store-screen-2]: ./media/sendgrid-sign-up/sendgrid_store_scrn2.png
[store-screen-3]: ./media/sendgrid-sign-up/sendgrid_store_scrn3.png
[sendgrid-purchasing-message]: ./media/sendgrid-sign-up/sendgrid_purchasing_message.png
[sendgrid-add-on-dashboard]: ./media/sendgrid-sign-up/sendgrid_add-on_dashboard.png
[sendgrid-connection-info]: ./media/sendgrid-sign-up/sendgrid_connection_info.png
[sendgrid-connection-info-button]: ./media/sendgrid-sign-up/sendgrid_connection_info_button.png
[sendgrid-control-panel]: ./media/sendgrid-sign-up/sendgrid_control_panel.png

<!--Links-->

[SendGrid ominaisuudet]: http://sendgrid.com/features
[Azure hallinta-portaalissa]: https://manage.windowsazure.com
[SendGrid käytön aloittaminen]: http://sendgrid.com/docs
[SendGrid valmistelu prosessi]: https://support.sendgrid.com/hc/articles/200181628-Why-is-my-account-being-provisioned-
[kaikki API]: https://sendgrid.com/docs/API_Reference/index.html

