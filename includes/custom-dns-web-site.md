#<a name="configuring-a-custom-domain-name-for-an-azure-website"></a>Mukautetun toimialuenimen Azure sivuston määrittäminen

Kun luot sivuston, Azure tarjoaa helpossa muodossa alitoimialueen azurewebsites.net toimialueen siten voi käyttää sivuston URL-osoite, kuten http://&lt;oman sivuston >. azurewebsites.net. Jos määrität oman sivuston jaetut tai vakiotilassa, voit yhdistää sivuston oman toimialuenimen.

Vaihtoehtoisesti voit ladata saldo saapuvan liikenteen sivustoon Azure liikenteen hallinta. Katso lisätietoja liikenteen hallinta toiminnasta sivustojen [Hallinta Azure sivustojen liikenne Azure liikenteen hallinta][trafficmanager].

> [AZURE.NOTE] Tämän tehtävän ohjeet koskevat Azure sivustojen; Katso Cloud Services <a href="/develop/net/common-tasks/custom-dns/">määrittäminen mukautettu toimialuenimi Azure-tietokannassa</a>.

> [AZURE.NOTE] Tässä tehtävässä vaiheet edellyttävät, oman sivuston määrittäminen jaettu tai normaalitilassa, jotka saattavat muuttua, kuinka paljon ovat laskuttaa tilauksen. Lisätietoja on kohdassa <a href="/pricing/details/web-sites/">Sivustojen hinnat tiedot</a> .

Tässä artikkelissa:

-   [Tietoja A ja CNAME-tietueet](#understanding-records)
-   [Web-sivustojen jaettujen tai vakio-tilan määrittäminen](#bkmk_configsharedmode)
-   [Lisää verkkosivustolla liikenteen hallinta](#trafficmanager)
-   [Lisää mukautetun toimialueen CNAME](#bkmk_configurecname)
-   [Mukautetun toimialueen A-tietueen lisääminen](#bkmk_configurearecord)

<h2><a name="understanding-records"></a>Tietoja A ja CNAME-tietueet</h2>

CNAME-TIETUE (tai alias-tietueet) ja molemmat tietueet avulla voit liittää toimialuenimen sivustoa, mutta ne kaikki toimivat eri tavoin.

###<a name="cname-or-alias-record"></a>CNAME-tai Alias

CNAME-tietue yhdistää *tietyn* toimialueen, kuten **contoso.com** tai **www.contoso.com**canonical toimialuenimi. Tässä tapauksessa canonical toimialuenimi on joko ** &lt;OmaSovellus >. azurewebsites.net** toimialuenimen Azure sivuston tai ** &lt;OmaSovellus >. trafficmgr.com** liikenteen hallinta profiilin toimialuenimi. Kun luonut, CNAME-TIETUE Luo tunnusnimen ** &lt;OmaSovellus >. azurewebsites.net** tai ** &lt;OmaSovellus >. trafficmgr.com** toimialuenimi. CNAME-merkinnän ratkaisee IP-osoitteen lisääminen ** &lt;OmaSovellus >. azurewebsites.net** tai ** &lt;OmaSovellus >. trafficmgr.com** toimialuenimi automaattisesti, joten jos sivuston IP-osoite muuttuu, sinun ei tarvitse tehdä mitään.

> [AZURE.NOTE] Jotkin toimialuerekisteröijät Salli vain voidaan yhdistää alitoimialueita, kun käytät CNAME-tietue, kuten www.contoso.com ja ei pääkansion nimiä, esimerkiksi contoso.com. Lisätietoja CNAME-tietueet on rekisteröintipalvelustasi, <a href="http://en.wikipedia.org/wiki/CNAME_record">CNAME-tietue Wikipedia-tapahtuma</a>tai <a href="http://tools.ietf.org/html/rfc1035">IETF toimialuenimet - käyttöönotto ja määritys</a> -asiakirjan ohjeissa.

###<a name="a-record"></a>Tietueen

A-tietue yhdistää toimialueen, kuten **contoso.com** tai **www.contoso.com**, *tai yleismerkkien toimialueen* ** \*. contoso.com**, IP-osoitteeseen. Azure-sivustossa kyseessä virtual IP-palvelun tai tietyn IP-osoite, jonka sivuston hankittujen. Jotta päälle CNAME-tietue A-tietue tärkeimmät etuna on se, että sinulla on yksi merkintä, joka käyttää yleismerkkejä, kuten * **. contoso.com**, jotka käsitellä useita alitoimialueisiin pyyntö kuten * *mail.contoso.com**, * *login.contoso.com**, tai * *www.contso.com**.

> [AZURE.NOTE] A-tietue on nyt yhdistetty staattinen IP-osoite, koska muutoksia ei voi selvittää automaattisesti sivuston IP-osoite. Mukautetun toimialuenimen asetusten määrittäminen sivuston; sait tietueisiin käytettäväksi IP-osoite kuitenkin arvoksi voivat muuttua, jos poistat ja luo sivuston tai muuttaa sivuston tilan vapauttaminen tiedostojen.

> [AZURE.NOTE] Kuormituksen tasaamisen liikenteen hallinta ei voi käyttää tietueisiin. Lisätietoja on ohjeaiheessa [Hallinta Azure sivustojen liikenne Azure liikenteen hallinta][trafficmanager].

<a name="bkmk_configsharedmode"></a><h2>Määritä vakio-tai Web-sivustoista</h2>

Käyttöoikeuksien määrittäminen mukautettua toimialuenimeä käyttävä sivusto on käytettävissä vain jaettu- ja Standard tilat Azure sivustot. Ennen siirtymistä sivuston vapaa sivuston tilaa jaettu tai Standard-sivuston tila on poistettava kaupankäynnin kulujen caps paikassa sivustossa tilauksen. Lisätietoja jaettujen ja vakio tilassa hinnat on artikkelissa [Hinnat tiedot][PricingDetails].

1. Avaa selaimessa, [Hallinta-portaalin][portal].
2. Valitse **sivustot** -välilehdessä näkyvää oman sivustosi nimeä.

    ![][standardmode1]

3. Valitse **Mittakaava** -välilehti.

    ![][standardmode2]


4. Määritä **Yleiset** -osaan sivuston tila valitsemalla **JAETTU**.

    ![][standardmode3]

    > [AZURE.NOTE] Jos käytät liikenteen hallinta tämän sivuston kanssa, sinun on käytettävä Valitse vakiotilassa jaettu sijaan.

5. Valitse **Tallenna**.
6. Pyydettäessä tietoja kasvu kustannukset, jotka on jaettu tila (tai vakiotilassa, jos valitset vakio), valitse **Kyllä** , jos hyväksyt.

    <!--![][standardmode4]-->

    **Huomautus**<br />
   Jos näyttöön tulee virhesanoma "Määrittäminen asteikko sivuston sivuston nimi, johon epäonnistui", voit saada lisätietoja tietoja-painike.

<a name="trafficmanager"></a><h2>(Valinnainen) Lisää oman sivuston liikenteen hallinta</h2>

Jos haluat käyttää sivuston liikenteen hallinta, suorita seuraavat vaiheet.

1. Jos sinulla ei ole liikenteen hallinta profiilin, käyttää luominen [käyttämällä nopea luominen liikenteen hallinta profiilin] tietoja[ createprofile] luomaan. Huomautus **. trafficmgr.com** liikenteen hallinta profiilin liitetyn toimialuenimen. Tämä käytetään myöhemmin.

2. Tietojen käyttäminen [Lisää tai poista päätepisteet] [ addendpoint] voit lisätä sivustoon päätepisteen liikenteen hallinta-profiilissasi.

    > [AZURE.NOTE] Jos sivuston ei näy luettelossa, kun lisäät päätepisteen, varmista, että se on määritetty vakiotilassa. Sinun on käytettävä vakiotilassa sivuston, jotta voit käyttää liikenteen hallinta.

3. Kirjaudu sisään DNS-Rekisteröintipalvelun sivustossa, ja siirry hallintasivu DNS. Etsi linkkejä tai **Toimialuenimi**, **DNS**- tai **Name Server Management**merkintä sivuston alueille.

4. Etsi, jossa voit valita tai kirjoittaa CNAME-tietueet. Saatat joutua Valitse tietuetyyppi avattavasta alaspäin tai valitsemalla Lisäasetukset-sivulla. Pitäisi näyttää sanoja **CNAME**- **Alias**tai **alitoimialueita**.

5. Sinun on annettava toimialueen tai alitoimialueen alias myös CNAME-TIETUE. Esimerkiksi **WWW-tunnistetta** , jos haluat luoda tunnusnimen **www.customdomain.com**.

5. Sinun on myös määritettävä isäntänimi, joka on CNAME-alias canonical toimialuenimi. Tämä on **. trafficmgr.com** sivuston nimi.

Esimerkiksi seuraavat CNAME-tietueet välittää kaikki liikenne- **www.contoso.com** **contoso.trafficmgr.com**, toimialuenimi on sivusto:

<table border="1" cellspacing="0" cellpadding="5" style="border: 1px solid #000000;">
<tr>
<td><strong>Alias tai Host nimi/alitoimialueen</strong></td>
<td><strong>Kanoninen toimialueen</strong></td>
</tr>
<tr>
<td>WWW-tunnistetta</td>
<td>contoso.trafficmgr.com</td>
</tr>
</table>

Käyttäjä **www.contoso.com** ei koskaan näe tosi host (contoso.azurewebsite.net), jotta edelleenlähetys-prosessi on näkymätön käyttäjälle.

> [AZURE.NOTE] Jos käytössäsi on liikenteen hallinta, sivuston kanssa, sinun ei tarvitse ohjeiden mukaisesti seuraavissa osissa, "**Lisää mukautetun toimialueen CNAME**" ja "**Lisää mukautetun toimialueen A-tietue**". CNAME-tietue, joka on luotu edellä kuvatut vaiheet reitittää liikenteen, liikenteen hallinta, joka reitittää liikenteen sivuston endpoint(s).

<a name="bkmk_configurecname"></a><h2>Lisää mukautetun toimialueen CNAME</h2>

CNAME-tietueen luomiseen on lisättävä uusi merkintä DNS-taulukon mukautetun toimialueen rekisteröintipalvelustasi työkalujen avulla. Kunkin rekisteröintipalvelu on samanlainen, mutta se on hieman erilainen tapa CNAME-tietue, joka määrittää, mutta käsitteitä ovat samat.

1. Etsi jokin näistä tavoista avulla **. azurewebsite.net** sivuston toimialuenimi.

    * Kirjaudu sisään [Azure hallinta-portaalin][portal], valitse sivuston, valitse **raporttinäkymät-ikkunan**ja Etsi **Sivuston URL-osoite** tapahtuma **quick glance** -osassa.

    * Asenna ja määritä [PowerShellin Azure](/manage/install-and-configure-windows-powershell/)ja käytä sitten seuraava komento:

            get-azurewebsite yoursitename | select hostnames

    * Asenna ja määritä [Azure komentoliittymä rivi](/manage/install-and-configure-cli/)ja käytä sitten seuraava komento:

            azure site domain list yoursitename

    Tallenna tämä **. azurewebsite.net** nimi, kun sitä käytetään seuraavissa vaiheissa.

3. Kirjaudu sisään DNS-Rekisteröintipalvelun sivustossa, ja siirry hallintasivu DNS. Etsi linkkejä tai **Toimialuenimi**, **DNS**- tai **Name Server Management**merkintä sivuston alueille.

4. Etsi, jossa voit valita tai kirjoittaa CNAME-tietueet. Saatat joutua Valitse tietuetyyppi avattavasta alaspäin tai valitsemalla Lisäasetukset-sivulla. Pitäisi näyttää sanoja **CNAME**- **Alias**tai **alitoimialueita**.

5. Sinun on annettava toimialueen tai alitoimialueen alias myös CNAME-TIETUE. Esimerkiksi **WWW-tunnistetta** , jos haluat luoda tunnusnimen **www.customdomain.com**. Jos haluat luoda tunnuksen päätoimialueen, sitä voidaan luettelossa nimellä '**@**' käyttämäsi Rekisteröintipalvelun DNS-työkalujen symboli.

5. Sinun on myös määritettävä isäntänimi, joka on CNAME-alias canonical toimialuenimi. Tämä on **. azurewebsite.net** sivuston nimi.

Esimerkiksi seuraavat CNAME-tietueet välittää kaikki liikenne- **www.contoso.com** **contoso.azurewebsite.net**, toimialuenimi on sivusto:

<table border="1" cellspacing="0" cellpadding="5" style="border: 1px solid #000000;">
<tr>
<td><strong>Alias tai Host nimi/alitoimialueen</strong></td>
<td><strong>Kanoninen toimialueen</strong></td>
</tr>
<tr>
<td>WWW-tunnistetta</td>
<td>contoso.azurewebsite.NET</td>
</tr>
</table>

Käyttäjä **www.contoso.com** ei koskaan näe tosi host (contoso.azurewebsite.net), jotta edelleenlähetys-prosessi on näkymätön käyttäjälle.

> [AZURE.NOTE] __Www__ -alitoimialueen liikenteen koskee vain yllä olevassa esimerkissä. Koska et voi käyttää yleismerkkejä CNAME-tietueet, sinun on luotava yksi CNAME-TIETUE kunkin toimialueen/alitoimialueen varten. Jos haluat liikenne voidaan ohjata osoitteesta alitoimialueita, kuten *. contoso.com-azurewebsite.net-osoitteeseen, voit määrittää __URL-Osoitteen ohjata__ tai __URL-Osoitteen eteenpäin__ tapahtuma DNS-asetukset, tai luo A-tietue.

> [AZURE.NOTE] Voi kestää jonkin aikaa oman CNAME leviäminen – DNS-järjestelmään. Et voi määrittää sivuston CNAME-TIETUE, kunnes CNAME-TIETUE on välitetty. Palvelun, kuten <a href="http://www.digwebinterface.com/">http://www.digwebinterface.com/</a> avulla voit varmistaa, että CNAME-TIETUE on käytettävissä.

###<a name="add-the-domain-name-to-your-website"></a>Toimialuenimen lisääminen sivustoon

Kun toimialueelle CNAME-tietue on välitetty, se on liittämällä sivuston. Voit lisätä mukautettua toimialuenimeä, määrittää sivustoon CNAME-tietue käyttämällä joko Azure käyttöliittymä (Azure CLI) tai Azure hallinta-portaalissa.

**Jos haluat lisätä komentorivin työkaluilla toimialuenimi**

Asenna ja määritä [Azure käyttöliittymä](/manage/install-and-configure-cli/)ja sitten seuraavalla komennolla:

    azure site domain add customdomain yoursitename

Esimerkiksi seuraavat Lisää mukautettua toimialuenimeä, **www.contoso.com** **contoso.azurewebsite.net** -sivustossa:

    azure site domain add www.contoso.com contoso

Voit vahvistaa, että mukautettu toimialuenimi on lisätty sivustoon käyttämällä seuraava komento:

    azure site domain list yoursitename

Tämä komento palauttaa luettelossa pitäisi näkyä mukautettua toimialuenimeä sekä oletusarvo **. azurewebsite.net** tapahtuma.

**Voit lisätä toimialuenimen Azure hallinta-portaalissa**

1. Avaa selaimessa [Azure hallinta-portaalin][portal].

2. Valitse **sivustot** -välilehdessä sivuston nimi, valitse **raporttinäkymät-ikkunan**ja valitse **Toimialueiden hallinta** sivulla.

    ![][setcname2]

6. Kirjoita toimialuenimi, jonka olet määrittänyt **TOIMIALUENIMET** -tekstiruutuun.

    ![][setcname3]

6. Valitse Hyväksy muutettavan toimialuenimen vieressä.

Kun määritys on valmis, sivuston **määrittäminen** -sivulla **toimialuenimet** -osassa näkyvät mukautettua toimialuenimeä.

<a name="bkmk_configurearecord"></a><h2>Mukautetun toimialueen A-tietueen lisääminen</h2>

A-tietueen luomiseen on ensin etsittävä sivuston IP-osoite. Valitse Lisää merkinnän mukautetun toimialueen DNS-taulukon rekisteröintipalvelustasi työkalujen avulla. Kunkin rekisteröintipalvelu on samanlainen, mutta hieman eri keinoja niiden A-tietue, joka määrittää, mutta käsitteitä ovat samat. Lisäksi A-tietueen luomisessa, sinun on luotava CNAME-tietue, jonka avulla Azure Tarkista tietueessa.

1. Avaa selaimessa [Azure hallinta-portaalin][portal].

2. Valitse **sivustot** -välilehdessä sivustosi nimeä, valitse **raporttinäkymät-ikkunan**ja valitse sitten näytön alareunasta **Toimialueiden hallinta** .

    ![][setcname2]

5. **Mukautettujen toimialueiden hallinta** -valintaikkunassa Etsi **IP-osoite käyttämään määritettäessä tietueisiin**. Kopioi IP-osoite. Tämä käytetään, kun luot A-tietueen.

5. **Mukautettujen toimialueiden hallinta** -valintaikkunassa Huomaa awverify toimialuenimi valintaikkunan yläosassa tekstin lopussa. Se olisi **awverify.mysite.azurewebsites.net** **oman sivuston** missä sivuston nimi. Kopioi tämän ennalleen vahvistus CNAME-tietueen luomisessa käytetty toimialuenimi.

6. Kirjaudu sisään DNS-Rekisteröintipalvelun sivustossa, ja siirry hallintasivu DNS. Etsi linkkejä tai **Toimialuenimi**, **DNS**- tai **Name Server Management**merkintä sivuston alueille.

6. Etsi, jossa voit valita tai kirjoittaa A ja CNAME-tietueet. Saatat joutua Valitse tietuetyyppi avattavasta alaspäin tai valitsemalla Lisäasetukset-sivulla.

7. Seuraavien toimien Luo A-tietue:

    1. Valitse tai kirjoita toimialueen tai alitoimialueen, joka käyttää tietueessa. Valitse esimerkiksi **www** , jos haluat luoda tunnusnimen **www.customdomain.com**. Jos haluat luoda kaikki alitoimialueet yleismerkkien tekstin, kirjoita '__*__". Tämä kattaa kaikki alitoimialueisiin, kuten **mail.customdomain.com**, **login.customdomain.com**ja **www.customdomain.com**.

        Jos haluat luoda päätoimialueen A-tietue, sitä voidaan luettelossa nimellä '**@**' käyttämäsi Rekisteröintipalvelun DNS-työkalujen symboli.

    2. Kirjoita määritetty kentän cloud-palvelun IP-osoite. Tämä liittää toimialueen tapahtuma käytetään tietueessa cloud palvelun käyttöönoton IP-osoitteeseen.

        Esimerkiksi tietueen välittää kaikki liikenne **contoso.com** - **137.135.70.239**, tämän sovelluksen IP-osoitteen seuraavasti:

        <table border="1" cellspacing="0" cellpadding="5" style="border: 1px solid #000000;">
        <tr>
        <td><strong>Host name/alitoimialueen</strong></td>
        <td><strong>IP-osoite</strong></td>
        </tr>
        <tr>
        <td>@</td>
        <td>137.135.70.239</td>
        </tr>
        </table>

        Tässä esimerkissä näytetään luominen päätoimialueen A-tietue. Jos haluat kattaa kaikki alitoimialueet yleismerkkien tekstin luominen, syötä "__*__' alitoimialueen nimellä.

7. Seuraavaksi luodaan CNAME-tietue, jossa on sähköpostitunnus **awverify**ja **awverify.mysite.azurewebsites.net** , jotka olet hankkinut aiemmin canonical toimialueeseen.

    > [AZURE.NOTE] Kun sähköpostitunnus awverify saattaa toimia joitakin rekisteröintipalveluissa, muiden voi vaatia awverify.www.customdomainname.com tai awverify.customdomainname.com koko alias toimialuenimi.

    Esimerkiksi seuraavat Luo CNAME-tietue, joka Azure avulla voit tarkistaa A-tietueen määritys.

    <table border="1" cellspacing="0" cellpadding="5" style="border: 1px solid #000000;">
    <tr>
    <td><strong>Alias tai Host nimi/alitoimialueen</strong></td>
    <td><strong>Kanoninen toimialueen</strong></td>
    </tr>
    <tr>
    <td>awverify</td>
    <td>awverify.contoso.azurewebsites.NET</td>
    </tr>
    </table>

> [AZURE.NOTE] Voi kestää jonkin aikaa awverify CNAME leviäminen – DNS-järjestelmään. Et voi määrittää määrittämiä verkkosivuston A-tietuetta, ennen kuin awverify CNAME-TIETUE on välitetty mukautettua toimialuenimeä. Palvelun, kuten <a href="http://www.digwebinterface.com/">http://www.digwebinterface.com/</a> avulla voit varmistaa, että CNAME-TIETUE on käytettävissä.

###<a name="add-the-domain-name-to-your-website"></a>Toimialuenimen lisääminen sivustoon

Kun toimialueelle **awverify** CNAME-tietue on välitetty, voit yhdistää A-tietuetta, jonka sivuston määrittämiä mukautettua toimialuetta. Voit lisätä sivustoon tietueessa määrittämiä käyttämällä joko Azure CLI tai Azure hallinta-portaalissa mukautettua toimialuenimeä.

**Voit lisätä toimialuenimen Azure komentorivivalitsimet käyttöliittymässä (Azure CLI)**

Asenna ja määritä [Azure CLI](/manage/install-and-configure-cli/)ja sitten seuraavalla komennolla:

    azure site domain add customdomain yoursitename

Esimerkiksi seuraavat Lisää mukautettu toimialuenimi on **contoso.com** **contoso.azurewebsite.net** -sivustossa:

    azure site domain add contoso.com contoso

Voit vahvistaa, että mukautettu toimialuenimi on lisätty sivustoon käyttämällä seuraava komento:

    azure site domain list yoursitename

Tämä komento palauttaa luettelossa pitäisi näkyä mukautettua toimialuenimeä sekä oletusarvo **. azurewebsite.net** tapahtuma.

**Voit lisätä toimialuenimen Azure hallinta-portaalissa**

1. Avaa selaimessa [Azure hallinta-portaalin][portal].

2. Valitse **sivustot** -välilehdessä sivuston nimi, valitse **raporttinäkymät-ikkunan**ja valitse **Toimialueiden hallinta** sivulla.

    ![][setcname2]

6. Kirjoita toimialuenimi, jonka olet määrittänyt **TOIMIALUENIMET** -tekstiruutuun.

    ![][setcname3]

6. Valitse Hyväksy muutettavan toimialuenimen vieressä.

Kun määritys on valmis, sivuston **määrittäminen** -sivulla **toimialuenimet** -osassa näkyvät mukautettua toimialuenimeä.

> [AZURE.NOTE] Kun olet lisännyt mukautetun toimialuenimen määrittämiä sivustoon A-tietuetta, voit poistaa awverify CNAME-tietue rekisteröintipalvelustasi työkalujen avulla. Jos haluat lisätä toisen A tietueen myöhemmin, sinun on luotava uudelleen awverify tietueeseen, ennen kuin voit yhdistää uusi toimialuenimi määrittämiä uuden tietueen sivustoa.

## <a name="next-steps"></a>Seuraavat vaiheet

-   [Sivustojen hallinta](/manage/services/web-sites/how-to-manage-websites/)

-   [SSL-varmenteen määrittäminen verkkosivustojen](/develop/net/common-tasks/enable-ssl-web-site/)


<!-- Bookmarks -->

[Configure your web sites for shared mode]: #bkmk_configsharedmode
[Configure the CNAME on your domain registrar]: #bkmk_configurecname
[Configure a CNAME verification record on your domain registrar]: #bkmk_configurecname
[Configure an A record for the domain name]:#bkmk_configurearecord
[Set the domain name in management portal]: #bkmk_setcname

<!-- Links -->

[PricingDetails]: /pricing/details/
[portal]: http://manage.windowsazure.com
[digweb]: http://www.digwebinterface.com/
[cloudservicedns]: ../articles/custom-dns.md
[trafficmanager]: ../articles/app-service-web/web-sites-traffic-manager.md
[addendpoint]: ../articles/traffic-manager/traffic-manager-endpoints.md
[createprofile]: ../articles/traffic-manager/traffic-manager-manage-profiles.md

<!-- images -->

[setcname1]: ../media/dncmntask-cname-5.png


<!-- images -->
[standardmode1]: ./media/custom-dns-web-site/dncmntask-cname-1.png
[standardmode2]: ./media/custom-dns-web-site/dncmntask-cname-2.png
[standardmode3]: ./media/custom-dns-web-site/dncmntask-cname-3.png
[standardmode4]: ./media/custom-dns-web-site/dncmntask-cname-4.png


[setcname2]: ./media/custom-dns-web-site/dncmntask-cname-6.png
[setcname3]: ./media/custom-dns-web-site/dncmntask-cname-7.png
