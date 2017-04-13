Kun toimialuenimen tietueet on välitetty, sinun täytyy yhdistää Web App-ohjelmalla. Seuraavien vaiheiden avulla voit selaimella toimialuenimien käyttöön.

> [AZURE.NOTE] Voi kestää jonkin aikaa TXT-tietueet, jotka on luotu edellä kuvatut vaiheet läpi koko DNS-järjestelmään Välitä. Et voi lisätä toimialuenimi web App-sovellukseen, kunnes TXT-tietue on välitetty. Jos käytössäsi on A-tietue, et voi lisätä web App-sovellukseen A-tietueen toimialuenimi ennen kuin edellisessä vaiheessa luotu TXT-tietue on välitetty.
>
> Palvelun, kuten <a href="http://www.digwebinterface.com/">http://www.digwebinterface.com/</a> avulla voit varmistaa, että TXT-tietue on käytettävissä.

1. Avaa [Azure Portal](https://portal.azure.com)selaimella.

2. Valitse **Web Apps** -välilehdessä web-sovelluksen nimi ja valitse sitten **Mukautetut toimialueet**

    ![](./media/custom-dns-web-site/dncmntask-cname-6.png)

3. Valitse **Lisää hostname** **Mukautetut toimialueet** -sivu.
    
4. Web-sovelluksen liitettävä toimialuenimien **Hostname** tekstiruutujen avulla.

    ![](./media/custom-dns-web-site/add-custom-domain.png)

6.  Valitse **Vahvista**.

7.  Kun **Validate** Azure valitsemalla projektin toimialueen vahvistus työnkulun. Tämä tarkistaa toimialueen omistajuuden sekä Hostname saatavuudesta ja raportin onnistumisesta tai ominaisuuksien guidence siitä, miten voit korjata virheen yksityiskohtaiset virhe.    

Tässä vaiheessa sinun pitäisi mukautettua toimialuenimeä selaimessa ja nähdä, että se onnistuu siirryt web Appissa.
