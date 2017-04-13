## <a name="prerequisites"></a>Edellytykset

Emme voi kirjoittaa CDN hallinta koodia, tarvitsemme, voit tehdä joitakin valmistelu koodia kanssa Azure Resurssienhallinta ottaa käyttöön.  Tällöin sinun on:

* Resurssiryhmä sisältää tässä opetusohjelmassa luodaan CDN-profiilin luominen
* Azure Active Directory tarjoamaan tämän sovelluksen käyttöoikeuksien määrittäminen
* Käyttöoikeuksien resurssiryhmän, niin, että vain valtuutettujen käyttäjien Microsoftin Azure AD-vuokraajan käsitellä Microsoftin CDN-profiili

### <a name="creating-the-resource-group"></a>Resurssiryhmän luominen

1. Lokitiedoston [Azure Portal](https://portal.azure.com).

2. Valitse vasemmasta yläreunasta ja valitse sitten **hallinta**ja **Resurssiryhmä** **Uusi** -painiketta.
    
    ![Luodaan uusi resurssiryhmä](./media/cdn-app-dev-prep/cdn-new-rg-1-include.png)

3. Soita resurssiryhmä *CdnConsoleTutorial*.  Valitse tilaus ja valitse lähellä sijainti.  Jos haluat, että, valitset **raporttinäkymät-ikkunan kiinnittäminen** -valintaruutu, jos haluat kiinnittää koontinäyttö resurssiryhmä-portaalissa.  Tämä helpompi löytää myöhemmin.  Kun olet valinnut asetukset, valitse **Luo**.

    ![Resurssiryhmä nimeäminen](./media/cdn-app-dev-prep/cdn-new-rg-2-include.png)

4. Kun resurssiryhmän on luotu, jos se ei ole kiinnittäminen koontinäyttöön, löydät sen valitsemalla **Selaa**ja valitse **Resurssiryhmät**.  Valitse Avaa resurssiryhmä.  **Tilauksen tunnus**muistiin.  Se annettava myöhemmin.

    ![Resurssiryhmä nimeäminen](./media/cdn-app-dev-prep/cdn-subscription-id-include.png)

### <a name="creating-the-azure-ad-application-and-applying-permissions"></a>Azure AD-sovelluksen luomisen ja käyttöoikeuksien määrittämistä

Sovelluksen todentamiseen Azure Active Directory-hakemistosta kahdella tavalla: yksittäisille käyttäjille tai palvelun lyhennyksen. Lainan palvelu muistuttaa palvelutilin Windowsissa.  Sen sijaan, että CDN-profiileista vuorovaikutuksessa oikeuksien myöntämistä tietyn käyttäjän, emme myöntää sijaan pääasiallista palveluun oikeuksia.  Palvelun ansaitun käytetään yleensä automatisoitu, ei ole vuorovaikutteinen kannalta.  Tässä opetusohjelmassa kirjoittaa vuorovaikutteinen console-sovelluksen, mutta tarkastellaan palvelun pääasiallista lähestymistapa.

Palvelun lyhennys luominen sisältää useita vaiheita, mukaan lukien Azure Active Directory-sovelluksen luominen.  Voit tehdä tämän tutustutaan [noudattamalla tässä opetusohjelmassa](../articles/resource-group-create-service-principal-portal.md).

> [AZURE.IMPORTANT] Muista kaikki [linkitetyt opetusohjelma](../articles/resource-group-create-service-principal-portal.md)noudattamalla.  On *erittäin tärkeää* sen täsmälleen ohjeiden suorittamista.  Varmista, että Huomaa, että **vuokraajan tunnus** **vuokraajan toimialuenimi** (yleisesti *. onmicrosoft.com* toimialueen paitsi, jos olet määrittänyt mukautettua toimialuetta)- **Asiakastunnus**ja **asiakkaan todennuksen avain**kuin on tarpeen näiden myöhemmin.  Varo hyvin suojaa **Asiakastunnus** ja **asiakkaan todennuksen avain**, kun näitä tunnistetietoja voidaan kuka tahansa voi suorittaa toimintoja palvelun lyhennys. 
>   
> Kun olet edennyt vaiheeseen, [Määritä usean vuokraajan sovelluksen](../articles/resource-group-create-service-principal-portal.md#configure-multi-tenant-application)nimi, valitse **ei**.
> 
> Kun olet edennyt vaiheeseen, [Määritä rooli-sovelluksen](../articles/resource-group-create-service-principal-portal.md#assign-application-to-role), käyttää luomaasi aiemmin *CdnConsoleTutorial*resurssiryhmä, mutta sen sijaan, että **lukija** -rooliin **CDN profiilin avustaja** -roolin määrittäminen.  Kun olet määrittänyt sovelluksen **CDN profiilin osallistujan** rooli resurssiryhmä-, palaa Tässä opetusohjelmassa. 

Kun olet luonut palvelun pääasiallista ja määritetty **CDN profiilin osallistujan** rooli, resurssiryhmän **käyttäjät** -sivu pitäisi näyttää seuraavanlaiselta.

![Käyttäjät-sivu](./media/cdn-app-dev-prep/cdn-service-principal-include.png)


### <a name="interactive-user-authentication"></a>Vuorovaikutteinen käyttäjän todentaminen

Jos palvelun-lyhennys sijaan voit mieluummin käyttää vuorovaikutteisia yksittäisen käyttäjän käyttöoikeuden, prosessi on samankaltainen kyseisen palvelun lyhennyksen.  Sinun on itse asiassa samalla tavalla, mutta tehdä muutamia pieniä muutoksia.

> [AZURE.IMPORTANT] Tee seuraavien vaiheiden vain, jos valitset Käytä yksittäisten käyttöoikeuksien sijaan pääasiallista palvelu.

1. Luotaessa sovelluksesi sijaan **Web-sovelluksen**, valitse **alkuperäisen sovelluksen**. 
    
    ![Alkuperäisen sovelluksen](./media/cdn-app-dev-prep/cdn-native-application-include.png)
    
2. Valitse seuraavalla sivulla voit pyydetään **uudelleenohjata URI**.  URI ei voi vahvistaa, mutta muista, mitä olet kirjoittanut.  Tarvitset niitä myöhemmin. 

3. Ei tarvitse luoda **asiakkaan todennuksen avain**.

4. Sen sijaan, että määrität palvelun lyhennys **CDN profiilin osallistujan** rooli, seuraavaksi määrittäminen yksittäisille käyttäjille tai ryhmille.  Tässä esimerkissä näet, että olet liitetyt *CDN esittely käyttäjän* **CDN profiilin osallistujan** rooli.  
    
    ![Yksittäisten käyttäjien käyttöoikeuksien](./media/cdn-app-dev-prep/cdn-aad-user-include.png)

