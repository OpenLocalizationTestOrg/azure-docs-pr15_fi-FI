<properties
    pageTitle="Yleistä Azure Active Directory-toimialuepalveluista | Microsoft Azure"
    description="Azure Active Directory-toimialuepalveluista yleiskatsaus"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/07/2016"
    ms.author="maheshu"/>

# <a name="azure-ad-domain-services"></a>Azure AD-toimialueen palvelut

## <a name="overview"></a>Yleiskatsaus
Azure infrastruktuuripalvelut avulla voit ottaa käyttöön monien tietojenkäsittely ratkaisuja joustava tavalla. Kanssa Azuren näennäiskoneiden, voit ottaa lähes välittömästi ja maksat vain minuutit. Windows-Linux, SQL Server, Oracle, IBM, SAP ja BizTalk tuki avulla, voit ottaa minkä tahansa työmäärää, minkä tahansa kielen, millä tahansa käyttöjärjestelmä. Seuraavat edut avulla voit siirtää paikalliseen käyttöön vanhojen sovellusten Azure toiminnallisia kulujen Tallenna.

Azure siirtäminen paikalliseen sovellusten tarjoama käsittelee nämä sovellukset tunnistetietojen tarpeisiin. Hakemisto-aware sovellusten Käytä luku- tai kirjoitusoikeudet yrityshakemistosta LDAP- tai Windows-todennusta (Kerberos tai NTLM-todennus) tarkistamiseen loppukäyttäjät riippuvaisia. Liiketoiminta-(LOB)-sovelluksia Windows Serverissä yleensä käyttöön liitetty tietokoneissa, jotta ne voidaan hallita suojatusti ryhmäkäytännön avulla. 'Hissin-ja-vaihto' paikallisen sovellukset pilveen riippuvuussuhteita yrityksen tunnistetiedot-infrastruktuuria on ratkaistava.

Järjestelmänvalvojat ottaa usein johonkin verkkosovelluksistaan käyttöön Azure identity-tarpeisiin seuraavista toimista:

- Ota käyttöön välillä työmääriä käynnissä Azure-infrastruktuuripalvelut ja yrityshakemistosta paikallisen sivuston sivuston VPN-yhteyden.
- Laajentaa yrityksen AD-toimialueen ja metsää infrastruktuurin määrittämällä replikan toimialueen ohjaimet Azure-virtuaalikoneissa avulla.
- Ota käyttöön Azure käyttämällä toimialueen ohjaimet kuin Azuren näennäiskoneiden käyttöön erillinen toimialue.

Kaikki ‑ominaisuuksia kärsivät hyvin kustannus- ja hallinnan tarvetta. Järjestelmänvalvojat edellyttää toimialueen ohjaimet näennäiskoneiden käyttäminen Azure käyttöönotto. Lisäksi tarvitsemansa hallintaa, suojattu, korjaustiedoston, valvonta, voit varmuuskopioida ja nämä näennäiskoneiden vianmääritys. VPN-yhteydet paikallisesta hakemistosta riippuvuuden aiheuttaa toiminnoista, jotka on otettu käyttöön Azure sisältäväksi lyhytkestoisia verkko-ongelmia tai katkokset. Nämä verkon katkokset johtaa puolestaan alemman käytettävyyttä ja rajoitettu luotettavuutta näiden sovellusten.

On suunniteltu Azure AD-toimialueen palveluista antamaan helpompaa löytämiseen.


## <a name="introducing-azure-ad-domain-services"></a>Azure AD-toimialueen palveluista esittely
Azure AD-toimialueen palveluista on toimialueen liity ryhmään käytäntö, LDAP-Kerberos/NTLM todentaminen, jotka ovat täysin Windows Server Active Directory-yhteensopiva hallitun toimialuepalveluihin. Voit käyttää ilman, että voit ottaminen käyttöön ja hallita korjaustiedoston toimialueen ohjaimet pilveen toimialueen palveluista. Azure AD-toimialueen palveluista integroitu aiemmin Azure AD alihallintaan, siten, että käyttäjät voivat kirjautua sisään yrityksen tunnistetietoja. Lisäksi voit käyttää olemassa olevia ryhmiä ja käyttäjätilit suojaamiseen resurssien, varmistaa sujuvan 'hissin-ja-vuoron' Azure-infrastruktuuripalvelut paikallisen resursseista.

Azure AD-toimialueen palvelut-toiminto toimii saumattomasti riippumatta siitä, onko Azure AD-vuokraajan vain pilvipalveluita tai synkronoitu paikallisen Active Directory kanssa.

### <a name="azure-ad-domain-services-for-cloud-only-organizations"></a>Azure AD-toimialueen palveluista vain pilvipalveluita organisaatioissa
Pelkkää cloud Azure AD-vuokraajan (kutsutaan usein "hallitun alihallinnat") ei ole mitään paikallisen tunnistetietojen vaatiman-tallennustilan. Toisin sanoen käyttäjätilit, niiden salasanat ja ryhmän jäsenyys ovat kaikki alkuperäisen pilveen - eli luodaan ja hallitaan Azure AD. Mieti hetki Contoso on pelkkää cloud Azure AD-vuokraajan. Seuraavassa kuvassa esitetyllä Contoson järjestelmänvalvoja on määrittänyt virtual verkon Azure infrastruktuurin Services-palveluissa. Sovellusten ja palvelimen toiminnoista on otettu käyttöön virtual verkoston Azuren näennäiskoneiden. Contoso on vain pilvipalveluita palvelutili, kaikki käyttäjätietojen, niiden tunnistetiedot ja ryhmän jäsenyys luodaan ja Azure AD-hallinnasta.

![Azure AD-toimialueen palvelut yleiskatsaus](./media/active-directory-domain-services-overview/aadds-overview.png)

Contoson IT-järjestelmänvalvoja, voit ottaa Azure AD-toimialueen palveluista niiden Azure AD-vuokraajan ja valita käyttöön virtual verkoston toimialuepalveluita. Tämän jälkeen Azure AD-toimialueen palveluista valmistelee hallitun toimialueen ja työnkulkuna virtual verkossa. Kaikki käyttäjätilit, ryhmäjäsenyyksiä ja käyttäjän tunnistetietoja Contoson Azure AD-vuokraajan käytettävissä ovat myös käytettävissä juuri luomasi toimialueessa. Tämän ominaisuuden avulla voit kirjautua käyttämällä yrityksen käyttöoikeutensa – esimerkiksi yhdistettäessä etäyhteyden toimialueeseen liittyneet koneet etätyöpöydän kautta toimialueen organisaatioon kuuluvien käyttäjien. Järjestelmänvalvojat voivat valmistella resurssien avulla luodun ryhmäjäsenyyksiä toimialueen käyttöä. Näennäiskoneiden virtual verkon käyttöön sovellukset voivat käyttää ominaisuuksia, kuten toimialueen liity, LDAP luku, LDAP sidonta, NTLM ja Kerberos-todennus ja Ryhmäkäytäntö.

Hallitut toimialueen, joka on valmisteltu Azure AD-toimialueen palvelut joitakin tärkeitä ominaisuuksia ovat seuraavat:

- Contoson IT-järjestelmänvalvoja ei tarvitse hallinta, korjaustiedoston ja seurata toimialueessa tai minkä tahansa ohjauskoneen, jos hallitut toimialueessa.
- Ei ole tarpeen hallittavan AD replikoinnin toimialueessa. Käyttäjätilit, ryhmäjäsenyyksiä ja Contoson Azure AD-vuokraajasta tunnistetiedot ovat automaattisesti käytettävissä tällä hallitun toimialueella.
- Koska toimialueen hallitsee Azure AD toimialueen Servicesin, Contoso IT-järjestelmänvalvoja ei ole toimialueessa toimialueen järjestelmänvalvoja tai yrityksen järjestelmänvalvojan oikeudet.


### <a name="azure-ad-domain-services-for-hybrid-organizations"></a>Azure AD-toimialueen palveluista hybrid organisaatioissa
Organisaatiot, joiden hybrid IT-infrastruktuurin tarjoaman cloud resursseja sekä paikallisen resurssit. Järjestön Synkronoi tunnistetietoja niiden paikallisesta hakemistosta käyttäjän Azure AD-vuokraajan. Hybrid organisaatiot miltä siirtää Lisää pilvipalveluun, erityisesti vanhoja hakemisto-aware sovelluksia, niiden paikallisen sovellusten Azure AD-toimialueen palveluista voi olla hyötyä tietyille käyttäjille.

Litware Corporation on otettu käyttöön [Azure AD Connect](../active-directory/active-directory-aadconnect.md)ja synkronoida tunnistetietoja niiden paikallisesta hakemistosta käyttäjän Azure AD-vuokraajan. Tunnistetiedot, jotka synkronoidaan sisältää käyttäjätilejä, niiden tunnistetiedon hajautusarvot todennus (salasanan synkronointi) ja ryhmäjäsenyyksiä.

> [AZURE.NOTE] **Salasanojen synkronoinnin on pakollinen hybrid organisaatiot voivat käyttää Azure AD-toimialueen palveluista**. Tämä vaatimus koskee, koska käyttäjien tunnistetiedot tarvitaan Azure AD-toimialueen palveluista, hallitun toimialueen tarkistamiseen nämä käyttäjät NTLM tai Kerberos todennusmenetelmien kautta.

![Azure AD-toimialueen palveluiden Litware Corporation:](./media/active-directory-domain-services-overview/aadds-overview-synced-tenant.png)

Yllä olevassa kuvassa näytetään, miten organisaatiot, joiden hybrid IT-infrastruktuurin, kuten Litware Corporation, voit käyttää Azure AD-toimialueen palveluista. Azure-infrastruktuuripalvelut virtual verkon käyttöön litware's sovellukset ja palvelimen toiminnoista, jotka edellyttävät toimialuepalveluita. Litware on IT-järjestelmänvalvoja, voit ottaa Azure AD-toimialueen palveluista niiden Azure AD-vuokraajan ja valita käyttöön virtual verkoston hallitun toimialueen. Litware on liitetty hybrid IT-infrastruktuurin, käyttäjätilit, ryhmät ja tunnistetiedot synkronoidaan käyttäjän Azure AD-vuokraajan niiden paikallisen hakemistosta. Tämän ominaisuuden avulla käyttäjät kirjautumaan toimialueen avulla yrityksen käyttöoikeutensa – esimerkiksi, kun yhteyden etäyhteyden koneet liitetty toimialueeseen etätyöpöydän kautta. Järjestelmänvalvojat voivat valmistella resurssien avulla luodun ryhmäjäsenyyksiä toimialueen käyttöä. Näennäiskoneiden virtual verkon käyttöön sovellukset voivat käyttää ominaisuuksia, kuten toimialueen liity, LDAP luku, LDAP sidonta, NTLM ja Kerberos-todennus ja Ryhmäkäytäntö.

Hallitut toimialueen, joka on valmisteltu Azure AD-toimialueen palvelut joitakin tärkeitä ominaisuuksia ovat seuraavat:

- Hallitut toimialue on erillinen toimialue. Se ei ole tiedostotunnistetta Litware päivän paikallisen toimialueen.
- Litware on IT-järjestelmänvalvoja ei tarvitse hallita, korjaus- tai näytön toimialueen ohjaimet hallitun toimialueessa.
- Ei ole tarpeen hallittavan AD-replikoinnin toimialueessa. Käyttäjätilit, ryhmäjäsenyyksiä ja tunnistetiedot on Litware paikallisen hakemistosta synkronoidaan Azure AD Azure AD Connect kautta. Nämä käyttäjätilit, ryhmäjäsenyyksiä ja tunnistetiedot ovat automaattisesti käytettävissä hallitun toimialueella.
- Koska toimialueen hallitsee Azure AD toimialueen Servicesin, Litware IT-järjestelmänvalvoja ei ole toimialueessa toimialueen järjestelmänvalvoja tai yrityksen järjestelmänvalvojan oikeudet.


## <a name="benefits"></a>Edut
Azure AD-toimialueen palvelujen kanssa voit käyttää seuraavia etuja:

-   **Yksinkertainen** – voit täyttää käyttöön yksinkertainen muutamalla napsautuksella Azure-infrastruktuuripalvelut näennäiskoneiden tunnistetietojen tarpeisiin. Sinun ei tarvitse ottaa käyttöön ja hallita tunnistetietojen infrastruktuurin Azure tai asennuksen connectivity takaisin paikalliseen identity-infrastruktuuria.

-   Azure AD-vuokraajan monitasoisissa integroitu **integroitu** – Azure AD-toimialueen palveluista. Voit käyttää Azure AD nyt integroitu pilvipohjainen yrityksen-kansio, joka caters Moderni sovellukset ja perinteinen hakemisto-aware sovellusten tarpeisiin.

-   **Yhteensopivat** – Azure AD-toimialueen palveluista on muodostettu, Windows Server Active Directory osoitettu yrityksen luokka-infrastruktuuria. Tämän vuoksi sovellustesi voit luottaa suurempi aste yhteensopivuuden Windows Server Active Directory-ominaisuuksilla. Kaikkia ominaisuuksia, jotka ovat käytettävissä Windows Server AD ovat tällä hetkellä käytettävissä Azure AD-toimialueen palveluista. Kuitenkin käytettävissä olevat ominaisuudet ovat yhteensopivia paikallisen infrastruktuurin riippuvaisia vastaavan Windows Server AD-ominaisuuksia. LDAP, Kerberos, NTLM, Ryhmäkäytäntö ja toimialueen liitoksen ominaisuudet muodostavat kehittynyt tarjoaa, joka on testannut ja säätää eri Windows Server-versioiden päälle.

-   **Edullinen** – Azure AD-toimialueen palveluista, voit välttää infrastruktuuri- ja rasitteen, joka liittyy tunnistetietojen infrastruktuurin perinteinen hakemisto-aware sovellusten hallinta. Voit siirtää näiden sovellusten Azure-infrastruktuuripalvelut ja toiminnallisia kulujen suurempi säästöjen hyötyvät.
