Käyttäjätietojen hallinta on vain tärkeänä julkisen pilveen, kun se on paikallinen. Auttamaan tämän Azure tukee useita eri cloud tunnistetietojen tekniikoita. Tällöin käytettävissä ovat seuraavat:

- Voit suorittaa Windows Server Active Directory (tunnetaan yleisesti nimellä vain AD) avulla luotu Azure Virtual koneet näennäiskoneiden pilveen. Tämän menetelmän on järkevää, kun käytät Azure laajentaa paikallisen palvelinkeskuksen pilveen.


- Azure Active Directoryn avulla voit antaa oman käyttäjien kertakirjautumisen [palveluna (SaaS)](https://azure.microsoft.com/overview/what-is-saas/) sovellukset. Microsoft Office 365: n käyttää tätä tekniikkaa esimerkiksi ja Azure tai cloud muiden ympäristöjen sovellusten myös käyttää sitä.


- Cloud tai paikallisen sovellusten avulla käyttäjille Kirjaudu sisään käyttämällä käyttäjätietojen Facebook, Google, Microsoftin ja muiden tunnistetietojen toimittajat Azure Active Directory käyttöoikeuksien hallinta.


Tässä artikkelissa on kaikki kolme seuraavista vaihtoehdoista.

## <a name="table-of-contents"></a>Sisällysluettelo

- [Windows Server Active Directory käynnissä näennäiskoneiden](#adinvm)

- [Azure Active Directoryn avulla](#ad)

- [Käyttämällä Azure Active Directory käyttöoikeuksien valvonta](#ac)


## <a name="adinvm"></a>Windows Server Active Directory käynnissä näennäiskoneiden

Käytössä Windows Server AD Azuren näennäiskoneiden on lähes samalla tavalla kuin käynnissä paikallisen. [Kuvassa 1](#fig1) on tyypillinen Esimerkki tämä ulkoasun.

![Azure Active Directory-virtuaalikoneen](./media/identity/identity_01_ADinVM.png)


<a name="Fig1"></a>Kuva 1: Windows Server Active Directory suorittaa Azuren näennäiskoneiden organisaation paikallisen palvelinkeskuksen Azure Virtual verkon käytön yhteydessä.

Seuraavassa esimerkissä Windows Server AD suoritetaan VMs luotu Azuren näennäiskoneiden, ympäristö IaaS tekniikka. Nämä VMs ja joitakin muita on ryhmitelty virtual verkko on yhteydessä Azure Virtual verkon käyttäminen paikallisen-palvelinkeskukseen. Virtuaalinen verkon carves cloud näennäiskoneiden paikallisen verkon kautta näennäisen yksityisverkon (VPN)-yhteyden kanssa toimivat lukujoukon ulos. Toimimalla näin nämä Azuren näennäiskoneiden näyttää vain toisen aliverkon paikallisen palvelinkeskukseen. Kuten kuvassa näkyy, kaksi näiden VMs on käynnissä Windows Server AD toimialueen ohjaimet. Muiden näennäiskoneiden näennäinen verkossa on käynnissä sovellukset, kuten SharePointin tai käyttää jollakin muulla tavalla, kuten kehittämiseen. Paikallisen Palvelinkeskus toimii myös kahden Windows Server AD-toimialueen ohjauskoneen.

Yhteyden toimialueen ohjaimet pilveen kanssa, joissa on paikallinen useita vaihtoehtoja:

- Varmista, että ne kaikki yhteen Active Directory-toimialueen osa.

- Luo erillisen AD toimialueiden paikallisen ja pilvipalvelussa, jotka ovat samassa metsää osa.

- Luoda erilliset AD metsien cloud ja paikallisen ja valitse Yhdistä toimialueiden väliset luottamussuhteet tai Windows Server Active Directory Federation Services (AD FS), jossa voit myös suorittaa Azuren näennäiskoneiden metsien.

Jostakin valinta tehdään, järjestelmänvalvojan Varmista, että paikalliset käyttäjät todennus-pyynnöt Siirry cloud toimialueen ohjaimet vain silloin, kun se on tarpeen, koska linkki pilveen on todennäköisesti hitaammin paikallisen verkot. Yhdistäviä cloud ja paikallisen toimialueen ohjaimet toinen tekijä on replikoinnin luoma liikenne. Toimialueen ohjaimet pilveen ovat yleensä oman AD-sivusto, joka sallii järjestelmänvalvojan ajoittaa kuinka usein replikointi on valmis. Liikenne lähettää ulos Azure palvelinkeskukseen, vaikka ei tavujen lähetetty, joka voi olla vaikutusta järjestelmänvalvojan replikoinnin vaihtoehtojen Azure kulut. Kannattaa myös nykyarvo osoittavat että vaikka Azure (DNS, Domain Name System)-tuen, tämä palvelu ei ole ominaisuuksia vaatii Active Directory (kuten dynaaminen DNS- ja SRV-tietueet tuki). Tästä syystä käytössä Windows Server AD Azure edellyttää pilveen omia DNS-palvelimet on määritetty.

Windows Server AD käytössä Azure VMs voi tulkita useita eri tilanteissa. Seuraavassa on joitakin esimerkkejä:

- Jos käytät Azuren näennäiskoneiden tiedostotunnistetta oman palvelinkeskuksen, sovellusten saattaa toimia pilvipalvelussa, joka on paikallisen toimialueen ohjaimet käsittelee asioita, kuten Windows-todennusta pyynnöt tai LDAP-kyselyt. SharePoint-esimerkiksi toimii usein Active Directory-hakemistosta ja siten samalla, kun se on mahdollista suorittaa SharePoint-klusterin Azure avulla paikallisesta hakemistosta, toimialueen ohjaimet pilveen määrittäminen merkittävästi parantaa suorituskykyä. (Se on tärkeää, että ei välttämättä ole pakollinen, vaan paljon sovellusten suorittamisen onnistuneesti käyttämällä vain paikallisen toimialueen ohjaimet pilveen.)

- Oletetaan, että kaukaisista Tampereen toimistossa puuttuu suorittaa omassa toimialueen ohjaimet resurssit. Nykyään tietokannan käyttäjiä on todennetaan toimialueen ohjaimet maailman muiden reunassa - kirjautumiset on hidasta. Käytössä Azure Active Directory-tarkempi Microsoft-palvelinkeskuksen voit nopeuttaa tämä tarvitsematta palvelimien haaran Officessa.

- Tietojen palauttaminen Azure käyttävän organisaation voivat ylläpitää aktiivinen VMs pilvipalveluun, kuten toimialueen ohjauskoneen on pieni joukko. Se voidaan valmistaa sitten Laajenna tämän sivuston muualla virheiden tulevat tarvittaessa.

Saatavilla on myös muita vaihtoehtoja. Jos esimerkiksi et ole Windows Server AD pilveen muodostettava yhteys paikalliseen palvelinkeskukseen. Jos haluat suorittaa SharePoint-klusterin, served tietyt käyttäjät, esimerkiksi kaikki, jolle Kirjaudu ainoastaan pilvipohjainen käyttäjätietojen kanssa, voi luoda erillinen metsää Azure. Miten voit käyttää tätä tekniikkaa määräytyy sen mukaan, mitä tavoitteet ovat. (Saat tarkempia ohjeita käyttäminen Azure, [on kohdassa](http://msdn.microsoft.com/library/windowsazure/jj156090.aspx)Windows Server AD.)

## <a name="ad"></a>Azure Active Directoryn avulla

Kun enemmän yleisiä SaaS sovelluksia, ne Nosta ilmeisimmät kysymys: mitä hakemistopalvelu tulee pilvipohjainen sovellukset käyttää? Microsoftin vastauksesi on Azure Active Directory.

Ohjatun hakemistopalvelun pilveen tärkeimmät vaihtoehdot ovat:

- Henkilöiden ja vain SaaS sovellukset käyttävissä organisaatioissa voit luottaa Azure Active Directory-kuin niiden harkintansa hakemistopalvelu.

- Organisaatioissa, joissa Suorita Windows Server Active Directory voivat niiden paikallisesta hakemistosta yhdistäminen Azure Active Directory, ja anna niiden käyttäjien kertakirjautumisen SaaS sovellusten sen avulla.


[Kuva 2](#fig2) on kuvattu näistä vaihtoehdoista, jossa Azure Active Directory on kaikki, joka on pakollinen ensimmäinen.

![Azure Active Directory-virtuaalikoneen](./media/identity/identity_02_AD.png)

<a name="fig2"></a>Kuva 2: Azure Active Directory antaa organisaation käyttäjien kertakirjautumisen SaaS-sovellukset, kuten Office 365: ssä.

Kuten kuvassa näkyy, Azure AD on usean vuokraajan palvelu. Tämä tarkoittaa, että se samanaikaisesti tukee useita eri organisaatioissa, tallentaminen, jokainen niistä käyttäjät directory tietoja. Tässä esimerkissä organisaatio A käyttäjä yrittää käyttää SaaS-sovellusta. Tämän sovelluksen saattaa olla osa Office 365: ssä, kuten SharePoint Online- tai jotain muuta voi olla – -Microsoft sovellusten voit myös käyttää tätä tekniikkaa. Azure AD tukee SAML 2.0-protokollaa, kaikki haluamasi on pakollinen-sovelluksesta on voi olla vuorovaikutuksessa käyttämällä yleinen-standardi. (Mahdollista sovelluksissa, jotka käyttävät Azure AD suorittaa palvelinkeskukseen, ei pelkästään Azure palvelinkeskukseen.)

Prosessi alkaa, kun käyttäjä käyttää SaaS-sovellus (vaihe 1). Voit käyttää tätä sovellusta, käyttäjän on esittää Azure AD myöntämä tunnus.

Tunnuksessa sisältää tiedot, jotka yksilöivät käyttäjän, ja se on allekirjoitettu digitaalisesti Azure AD. Saat tunnuksen-käyttäjän suorittaa itsestään Azure AD antamalla käyttäjänimellä ja salasanalla (vaihe 2). Valitse Azure AD palauttaa tunnuksen hänellä on oltava (vaihe 3).

Tunnuksessa lähetetään SaaS sovellukseen (vaihe 4), joka vahvistaa tunnuksen allekirjoitus ja käyttää sen sisältö (vaihe 5). Yleensä sovellus käyttää tunnus sisältää päättää, mitä käyttäjä voi Accessiin ja mahdollisesti muita tietoja tunnistetietoja.

Jos sovellus on lisätietoja käyttäjän kuin mitä sisältyy tunnuksen, se voi pyytää suoraan Azure AD käyttämällä Azure AD-kaavio-Ohjelmointirajapinnan (vaihe 6). Alkuperäinen Azure AD-versiossa directory-rakenne on yksinkertainen: se sisältää vain käyttäjät ja ryhmät ja niiden suhteet. Sovellukset voivat käyttää näitä tietoja Lisätietoja käyttäjien väliset yhteydet. Oletetaan esimerkiksi, että sovellus on tietävät tämän käyttäjän esimies on päättää hän on sallinut joitakin tietojoukon käyttöoikeus. Se Lue tämä tekemällä kyselyn Azure AD Graph API-Liittymän kautta.

Graph-Ohjelmointirajapinta käyttää tavallisen RESTful protokolla, joka on helppoa, jos haluat käyttää useimmissa asiakkaiden, mukaan lukien mobiililaitteet. Ohjelmointirajapinnan tukee myös määrittämiä OData-asioita, kuten kyselykieli kertoaksesi hyödyllisempi tavoilla asiakkaiden access-tietojen lisääminen. (Saat lisätietoja OData-Katso [Esittely OData](http://download.microsoft.com/download/E/5/A/E5A59052-EE48-4D64-897B-5F7C608165B8/IntroducingOData.pdf).) Koska Graph-Ohjelmointirajapinnan voidaan käyttää käyttäjien suhteita lisätietoja avulla sovellusten ymmärtää sosiaalisen kaavio, joka on upotettu Azure AD-rakenteen määrätyn organisaation (joka on miksi sen nimi on Graph API). Ja todennusta Graph Ohjelmointirajapinnan pyyntöjen Azure AD-sovellus käyttää OAuth 2.0.

Jos organisaatio ei käytä Windows Server Active Directory - se ei ole paikallisen palvelimia tai toimialueet - ja luottaa cloud-sovelluksia, jotka käyttävät Azure AD-käyttämällä vain cloud kansion antaa yrityksen käyttäjien kertakirjautumisen ne kaikki. Vielä samalla, kun tämä skenaario saa tavallisimmista päivittäin, useimmat organisaatiot edelleen käyttää paikalliset toimialueet, joka on luotu käyttämällä Windows Server Active Directory. Azure AD on hyötyä rooli, jotta voit toistaa tähän, kuten [kuvassa 3](#fig3) näkyy.

![Azure Active Directory-virtuaalikoneen](./media/identity/identity_03_AD.png)
<a id="fig3"></a>kuva 3: organisaatiossa voidaan järjestäjäorganisaatiota Windows Server Active Directory Azure Active Directory voi antaa sen käyttäjät kertakirjautumisen SaaS sovellukset.

Tässä skenaariossa organisaation B käyttäjä haluaa käyttää SaaS-sovellusta. Ennen kuin hän tekee tämän, yrityksen Järjestelmänvalvojat on muodostettava federation yhteyden Azure AD käyttäen AD FS kuvassa. Näiden järjestelmänvalvojien on määritettävä tiedot organisaation paikallisen Windows Server AD ja Azure AD-synkronoinnin. Tämä kopioi automaattisesti käyttäjän ja ryhmätiedot paikallisesta hakemistosta Azure AD. Huomaa, että tämä sallii:, organisaatio on laajentaminen sen paikallisesta hakemistosta pilveen kyselyjä. Yhdistäminen Windows Server AD- ja Azure AD tällä tavalla antaa organisaation hakemistopalvelu, joka voidaan hallita yhden kohteen jatkuvat vaatiman tallennustilan sekä paikallisen ja pilveen.

Voit käyttää Azure AD-käyttäjän ensin kirjautuu sisään hänen paikallisen Active Directory-toimialueen tavalliseen tapaan (vaihe 1). Kun hän yrittää käyttää SaaS-sovellusta (vaihe 2), federation prosessin tuloksena Azure AD myöntää hänelle tunnuksen sovelluksen (vaihe 3). (Saat lisätietoja federation toiminta- [Windows väitepohjaista tunnistustietojen: Technologies-tuote ja skenaariot](http://www.davidchappell.com/writing/white_papers/Claims-Based_Identity_for_Windows_v3.0--Chappell.docx).) Kun, tunnuksessa on tietoja, joka määrittää käyttäjän, ja se on allekirjoitettu digitaalisesti Azure AD. Tunnuksessa lähetetään SaaS sovellukseen (vaihe 4), joka vahvistaa tunnuksen allekirjoitus ja käyttää sen sisältö (vaihe 5). Edellisen tilanteessa sovellus voi käyttää Graph-Ohjelmointirajapinnan Saat lisätietoja tämän käyttäjän Jos SaaS ja tarvittaessa (vaihe 6).

Nykyään Azure AD ei valmis korvaa paikallisen Windows Server AD. Kuten edellä cloud-kansio on paljon yksinkertaisempi rakenne ja myös puuttuu kohteita, kuten ryhmäkäytännön voi tallentaa tietoja koneet ja LDAP-tuki. (Itse asiassa Windows-tietokoneen ei voi määrittää takaa käyttäjille, kirjaudu sisään käyttämällä mitään mutta Azure AD - ei tueta.) Sen sijaan Azure AD alkuperäinen tavoitteisiin Sisällytä auttaa muita yrityksen käyttäjät pilvipalvelussa access-sovelluksia säilyttämättä erillisessä kirjautuminen ja vapauttaminen paikallisten järjestelmänvalvojat synkronoinnin manuaalisesti jokaisen SaaS sovelluksen niiden organisaatiossa niiden paikallisesta hakemistosta. Ajan myötä toiminta kuitenkin cloud hakemistopalvelun sähköpostiosoite monenlaisille skenaarioita.

## <a name="ac"></a>Käyttämällä Azure Active Directory käyttöoikeuksien valvonta

Pilvipohjainen tunnistetietojen tekniikoita voidaan erilaisia ongelmien ratkaisemiseen. Azure Active Directory antaa organisaation käyttäjien kertakirjautumisen useita SaaS sovelluksia, kuten. Mutta tunnistetiedot technologies pilveen voidaan myös muilla tavoilla.

Oletetaan esimerkiksi, että sovelluksen haluaa sen käyttäjät voivat kirjautua sisään käyttämällä tunnusten myöntämä useita- *tunnistetietojen toimittajat (IdPs)*. Eri tunnistetietojen toimittajat useille olemassa tänään, kuten Facebook, Google, Microsoftin ja muiden ja sovellusten usein avulla käyttäjät voivat kirjautua sisään käyttämällä jotakin näistä käyttäjätietoja. Miksi sovelluksen pitäisi tekee Jos haluat säilyttää sen sijaan voit luottaa käyttäjätietoja, jotka ovat jo omassa luettelo käyttäjät ja salasanat? Hyväksy käyttäjätietoja on aika yksinkertaisempi molemmat käyttäjät, joilla on yksi käyttäjänimi ja salasana, muista vähemmän, sekä henkilöt, jotka luovat sovellus, joka ei enää tarvitse säilyttää käyttäjänimet ja salasanat omia luetteloita.

Mutta kun jokaisen tunnistetietojen toimittaja ongelmat jokin lisätoimi tunnuksen, näiden tunnusten eivät ole vakio - kunkin IdP on oma muoto. Lisäksi näiden tunnusten tiedot myös ei ole vakio. Sovellus, joka haluaa Hyväksy tunnusten myöntämä, Yammer, Facebook, Google ja Microsoft on kärsivät haasteellista yksilöllisiä koodin kirjoittamisen käsittelemään näitä eri muotoja.

Mutta Miksi näin? Miksi ei sen sijaan luoda edustaja, voit luoda yksittäisen suojaustunnuksen muoto sisältää yleisiä esittäminen tunnistetietoja? Tämän menetelmän helpottaa yksinkertaisempi kehittäjille, jotka luoda sovelluksia, koska ne on nyt käsitellä vain yhden millaisen tunnuksen. Azure Active Directory käyttöoikeuksien valvonta tekee täsmälleen tämän antamisen edustaja pilveen monipuolisen tunnusten käsittelyyn. [Kuva 4](#fig4) näyttää, miten se toimii

![Azure Active Directory-virtuaalikoneen](./media/identity/identity_04_IdentityProviders.png)
<a id="fig4"></a>kuva 4: Azure Active Directory käyttöoikeuksien hallinta on helppo sovellusten Hyväksy tunnistetietojen tunnusten eri tunnistetietojen toimittajat myöntämä.

Prosessi alkaa, kun käyttäjä yrittää käyttää sovellusta selaimesta. Sovelluksen ohjaa, hän voi haluamaansa IdP (ja sovelluksen myös luottaa). Hän todentaa ottaa tämän IdP, kuten kirjoittamalla käyttäjänimellä ja salasanalla (vaihe 1) ja IdP palauttaa tunnuksen, joka sisältää tietoja hänen (vaihe 2).

Kuten kuvassa näkyy, käyttöoikeuksien valvonta tukee solualueen eri pilvipohjainen IdPs, mukaan lukien luoma Google, Yahoo, Facebook, Microsoft (aiemmalta nimeltään Windows Live ID) ja minkä tahansa OpenID palveluntarjoajan tilejä. Se tukee myös käyttäjätietojen Azure Active Directory-sovelluksella luodut ja AD FS, Windows Server Active Directory federation kautta. Tavoitteena on peittää yleisimmin käytettyjä käyttäjätietojen tänään, onko ne on myöntänyt IdPs cloud tai paikallisen.

Kun käyttäjän selain on IdP-tunnuksen-hänen valittu IdP, se lähettää tämä suojaustunnuksen käyttöoikeuksien hallinta (vaihe 3). Käyttöoikeuksien hallinta vahvistaa tunnuksen, varmista, että se todella on myöntänyt tämän IdP ja valitse Luo uusi salasana, jotka on määritetty tämän sovelluksen lajittelusääntöjen. Azure Active Directory, kuten käyttöoikeuksien hallinta on usean vuokraajan palvelu, mutta alihallinnat, jotka ovat sovellusten asiakkaan organisaatiot sijaan. Kunkin sovelluksen pääsevät omassa nimitilan kuvassa näkyy, ja voit määrittää eri sääntöjä tietoja luvan ja paljon muuta.

Sääntöjen avulla kunkin sovelluksen järjestelmänvalvoja määrittää, kuinka eri IdPs tunnusta muuntaa kyselyjä käyttöoikeuksien valvonta-tunnuksen. Esimerkiksi eri IdPs käyttää erilaista edustava käyttäjänimet, jos käytönvalvonta sääntöjä voit muuntaa kaikki nämä yleiset käyttäjänimi-tyypiksi. Käyttöoikeuksien hallinta lähettää sitten uusi tunnuksessa takaisin selaimessa (vaihe 4), joka lähettää sen sovelluksen (vaihe 5). Kun se on käyttöoikeuksien valvonta-tunnuksen, sovellus varmistaa, että tunnuksessa todella on myöntänyt käyttöoikeuksien hallinta ja valitse tiedot, se sisältää (vaihe 6).

Vaikka tämä toimenpide saattaa näyttää hieman monimutkaiselta, se todellisuudessa on suunniteltu aika merkittävästi yksinkertaisempi sovelluksen luoja. Sijaan käsittelemään monipuolisen tunnuksia, jotka sisältävät erilaisia tietoja, sovelluksen hyväksyä käyttäjätietojen myöntämä useita tunnistetietojen toimittajat vastaanotettaessa edelleen vain yhden tunnuksen tuttuja tiedoilla. Lisäksi sijaan edellyttävät kunkin sovelluksen on määritettävä, luotetaanko eri IdPs, nämä luottamussuhteet sijaan ylläpitämä käyttöoikeuksien valvonta - sovellus on vain Salli käyttöoikeus.

Se on osoittavat että mitään tietoja käyttöoikeuksien hallinta on yhdistetty Windows - sitä voi käyttää vain sekä Linux-sovellus, joka hyväksyy vain Google- ja Facebook-käyttäjätietoja. Ja vaikka käyttöoikeuksien valvonta kuuluu Azure Active Directory-tuoteperheen, voit ajatella se kokonaan eri palveluna-mikä on kuvattu edellisessä osassa. Vaikka molemmat tekniikat käsitellä käyttäjätietoja, ne osoitteen aivan eri ongelmia (vaikka Microsoft on said, odottaa integroida jossakin vaiheessa 2).

Käyttäjätietojen käsittelemisestä on tärkeää lähes kaikissa. Käyttöoikeuksien hallinta on helpompi luoda sovelluksia, jotka hyväksyvät käyttäjätietojen monipuolisen tunnistetietojen toimittajat-kehittäjille. Sijoittamalla tämän palvelun pilveen Microsoft tapahtuneen käytettävissä minkä tahansa sovelluksen minkä tahansa ympäristössä.

##<a name="about-the-author"></a>Tietoja kirjoittajasta

David Chappell on pääasiallista, Chappell ja osakkaiden [www.davidchappell.com](http://www.davidchappell.com) -San Francisco, Kalifornian.
