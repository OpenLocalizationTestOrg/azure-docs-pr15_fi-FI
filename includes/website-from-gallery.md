Azure Marketplacesta on käytettävissä monien suosittujen web Apps-sovellusten kehittämää Microsoft kolmansien osapuolien ja Avaa lähde ohjelmiston hankkeita. Web-sovellukset, jotka on luotu Azure Marketplacesta ei tarvitse asennettuun kuin selaimen [Azure esikatselu Portal](http://go.microsoft.com/fwlink/?LinkId=529715)-yhteyden kanssa käytettäviä ohjelmistoja. 

Tässä opetusohjelmassa opit:

- Voit luoda uuden web-sovelluksen kautta Azure Marketplacesta.

- Vaiheittaiset ohjeet palvelun Azure esikatselu-portaalissa web-sovelluksen käyttöönotto.
 
Luot WordPress blogia, joka käyttää oletusmallia. Seuraavassa kuvassa näkyy valmiin sovelluksen:


![Wordpress-blogi][13]

>[AZURE.NOTE] Jos haluat aloittaa Azure App palvelun ennen rekisteröimässä Azure-tili, siirry [Yritä App palvelu](http://go.microsoft.com/fwlink/?LinkId=523751), jossa lyhytkestoinen starter verkkosovellukseen heti voit luoda sovelluksen-palvelussa. Ei ole pakollinen; luottokortit ei ole sitoumukset.

## <a name="create-a-web-app-in-the-portal"></a>Web-sovelluksen luominen-portaalissa

1. Kirjaudu sisään Azure esikatselu-portaaliin.

2. Avaa Azure Marketplacesta joko napsauttamalla **Marketplace** -kuvaketta:

    ![Marketplace-kuvake][marketplace]

    Tai napsauttamalla Raporttinäkymät-ikkunan oikeassa yläkulmassa **Uusi** -kuvaketta ja valitsemalla **Marketplace** osoitteessa bottow luettelon.
    
    ![Luo uusi][5]
    
3. Valitse **Web + Mobile**. Etsi **WordPress** ja **WordPress** -kuvaketta.

    ![WordPress luettelosta][7]
    
5. Luettuasi WordPress sovelluksen kuvausta valitsemalla **Luo**.

6. Valitse **Web**Appissa ja anna tarvittavat arvot web-sovelluksen määrittäminen.
    
    ![sovelluksen määrittäminen][8]

7. Valitse **tietokanta**ja anna tarvittavat arvot määrittäminen MySQL-tietokantaan. 

    ![tietokannan asetusten määrittäminen][database]

8. Anna sille nimi uusi resurssiryhmä.

    ![Määritä resurssiryhmä][groupname]

8. Tarvittaessa Valitse **TILAUS**ja määritä tilauksen käyttäminen. 

7. Kun olet web-sovelluksen määrittäminen, valitse **Luo**ja odota, kunnes uusi web-sovellus on luotu.

   Kun sovellus on luotu, näet sisältävä web app- ja tietokannan resurssiryhmä.

   ![Näytä-ryhmä][resourcegroup]

## <a name="launch-and-manage-your-wordpress-web-app"></a>Käynnistä ja hallita WordPress koodiin
    
1. Jos haluat lisätietoja sovelluksen uusi web-sovellus napsauttamalla.

    ![Käynnistä Raporttinäkymät-ikkunan][10]

2. Valitse **Essentials** -sivulla **Selaa** tai linkin **URL-osoitteen** Avaa web-sovelluksen aloitussivu-kohdassa.

    ![sivuston URL-osoite][browse]

3. Jos et ole asentanut WordPress, kirjoita WordPress vaadittujen tarvittavat määritykset ja valitse **Asenna WordPress** Viimeistele määritys ja avaa web app-kirjautumissivulle.

4. Valitse **Kirjaudu sisään** ja anna tunnistetiedot.  

5. Sinun on uusi WordPress web App-sovelluksesta, joka näyttää samalta web-sovellukseen.    

    ![WordPress-sivustoon][13]






[5]: ./media/website-from-gallery/start-marketplace.png
[6]: ./media/website-from-gallery/wordpressgallery-02.png
[7]: ./media/website-from-gallery/search-web-app.png
[8]: ./media/website-from-gallery/set-web-app.png
[9]: ./media/website-from-gallery/wordpressgallery-05.png
[10]: ./media/website-from-gallery/select-web.png
[13]: ./media/website-from-gallery/wordpressgallery-09.png
[webapps]: ./media/website-from-gallery/selectwebapps.png
[database]: ./media/website-from-gallery/set-db.png
[resourcegroup]: ./media/website-from-gallery/show-rg.png
[browse]: ./media/website-from-gallery/browse-web.png
[marketplace]: ./media/website-from-gallery/marketplace-icon.png
[groupname]: ./media/website-from-gallery/set-rg.png
