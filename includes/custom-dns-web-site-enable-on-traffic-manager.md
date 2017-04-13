Kun toimialuenimen tietueet on välitetty, sinun pitäisi Tarkista mukautettua toimialuenimeä voidaan käyttää Azure-sovelluksen palvelun koodiin selaimen avulla.

> [AZURE.NOTE] Voi kestää jonkin aikaa oman CNAME leviäminen – DNS-järjestelmään. Palvelun, kuten <a href="http://www.digwebinterface.com/">http://www.digwebinterface.com/</a> avulla voit varmistaa, että CNAME-TIETUE on käytettävissä.

Jos et ole lisännyt koodiin kuin liikenteen hallinta päätepisteen, toimi samoin ennen nimenselvitys toimii kuin mukautetun toimialueen nimeä tiet liikenteen hallinta. Liikenteen hallinta reitittää web App-sovellukseen. [Lisää](../articles/traffic-manager/traffic-manager-endpoints.md) tai poista päätepisteet tietojen avulla voit lisätä koodiin päätepisteen liikenteen hallinta-profiilissasi.

> [AZURE.NOTE] Jos web app ei näy luettelossa, kun lisäät päätepisteen, varmista, että se on määritetty **Vakio** App palvelun suunnitelman tila. Sinun on käytettävä **vakiotilassa web App-sovelluksen** , jotta voit käyttää liikenteen hallinta.

1. Avaa [Azure Portal](https://portal.azure.com)selaimella.

1. **Web Apps** -välilehdessä web-sovelluksen nimi, valitse **asetukset**ja valitse sitten **Mukautetut toimialueet**

    ![](./media/custom-dns-web-site/dncmntask-cname-6.png)

1. Valitse **Lisää hostname** **Mukautetut toimialueet** -sivu.
    
1. **Hostname** tekstiruutujen avulla voit yhdistää tämän web-sovelluksen kanssa liikenteen hallinta toimialueen nimi.

    ![](./media/custom-dns-web-site/dncmntask-cname-8.png)

1. Valitse **Vahvista** Tallenna toimialueen nimi-määritys.

7.  Kun **Validate** Azure valitsemalla projektin toimialueen vahvistus työnkulun. Tämä tarkistaa toimialueen omistajuuden sekä Hostname saatavuudesta ja raportin onnistumisesta tai ominaisuuksien guidence siitä, miten voit korjata virheen yksityiskohtaiset virhe.    

8.  Onnistuneen vahvistuksen **Lisää hostname** yhteydessä painike tulee aktiivinen ja osaat, Määritä isäntänimi. Siirry nyt mukautettua toimialuenimeä selaimessa. Pitäisi tulla näkyviin oman Sovelluksen suorittaminen käyttämällä mukautettua toimialuenimeä. 

    Kun määritys on valmis, web-sovelluksen **toimialuenimet** -osassa näkyvät mukautettua toimialuenimeä.

Tässä vaiheessa sinun pitäisi liikenteen hallinta verkkotunnuksen nimi selaimessa ja nähdä, että se onnistuu siirryt web Appissa.
