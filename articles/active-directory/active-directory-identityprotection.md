<properties
    pageTitle="Azure Active Directory-tunnistetietojen suojauksen | Microsoft Azure"
    description="Katso, miten Azure AD-tunnistetiedot avulla voit rajoittaa luvaton hyödyntää vaarantuneen tunnistetiedot tai laite ja suojaamiseen jäsenyyden tai laitteella, joka oli aiemmin epäilty tai tiedossa on käsiin."
    services="active-directory"
    keywords="Azure active Directoryn tunnistetietojen suojaus-cloud app etsiminen-sovellukset, suojaus, riski, riskin taso, heikkous, suojauskäytäntö hallinta"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/26/2016"
    ms.author="markvi"/>

#<a name="azure-active-directory-identity-protection"></a>Azure Active Directory-tunnistetietojen suojaus 

Azure Active Directory tunnistetietojen suojaus on ominaisuus, josta löytyy riski tapahtumien ja mahdolliset heikkouksien, vaikuttavia organisaation käyttäjätietojen kootut näkymän Azure AD Premium P2 edition. Microsoft on ollut suojaaminen pilvipohjainen jäsenyyksiä vuosikymmenen ja Azure AD-tunnistetietojen käyttöoikeudella Microsoft on saataville saman SUOJAJÄRJESTELMÄT yritysasiakkaille. Tunnistetiedot hyödyntää aiemmin Azure AD's poikkeavuuksista tunnistus ominaisuuksia (käytettävissä Azure AD erheellisiin käyttöraporttien) ja esitellään uusi riski tapahtuman Lisättävissä olevat tunnistaa poikkeamia reaaliajassa.



##<a name="getting-started"></a>Käytön aloittaminen

Suurin osa suojauksen ilmeisesti tehdään, kun hyökkääjät käsitellä ympäristössä varastamasta käyttäjän tunnistetiedot. Hyökkääjät on tullut yhä tehokas hyödyntäminen kolmannen osapuolen ilmeisesti ja kehittyneitä tietojen kalastelu avulla. Kun luvaton pääsee jopa pienen sellaisten käyttäjätilille, on suhteellisen yksinkertaista niiden saat käyttöösi tärkeitä yrityksen resurssien radan viereen varattu siirto kautta. Näin ollen suojaa kaikki käyttäjätiedot ja kun jäsenyyden on ongelmia, kun muutoksista estää vaarantuneen tunnistetiedot on väärin. 

Etsiminen vaarantuneen käyttäjätietoja ei ole helppoa. Lähettäjä-tunnistetiedot voivat auttaa: tunnistetiedot käyttää mukautuvat koneen learning algoritmien ja heuristiikkaa tunnistaa poikkeamia ja riskien tapahtumia, jotka ilmaisevat, että käyttäjätietojen käsiin.
 
Käytä näitä tietoja, tunnistetiedot Luo raportteja ja ilmoitukset, jonka avulla voit tutkia riskin näitä tapahtumia ja toteuttaa haluamasi korjaus tai rajoituksen toiminto.
 
Mutta Azure Active Directory tunnistetietojen suojaus on enemmän kuin valvonta- ja -työkalu. Riskin tapahtumiin perustuvan tunnistetiedot laskee riskin käyttäjätason kunkin käyttäjän kohdalla voit määrittää riskiin perustuva käytäntöjä suojaaminen automaattisesti organisaation tunnistetietoja.  Nämä riskiin perustuva käytännöt Azure Active Directory-ja EMS, ehdollisen käyttöoikeuden muiden ohjausobjektien lisäksi automaattisesti estää tai tarjota mukautuvat korjaus-toiminnot, jotka sisältävät salasanan vaihtamisesta ja monimenetelmäisen todentamisen käyttäminen.  

####<a name="explore-identity-protections-capabilities"></a>Selaa käyttäjätietoja suojauksen 

**Tunnistaminen riskin tapahtuma- ja riskialtis tilejä:**  

- Tunnistaminen 6 riskin tapahtumatyypit koneen oppiminen ja heuristinen sääntöjen avulla 

- Laskeminen käyttäjän riskin tasot

- Mukautetun suosituksia parantamiseksi yleinen suojaus reagoimisessa korostamalla heikkouksien antamisen



**Käsittelevälle riskin tapahtumat:**

- Riskin liittyviä tapahtumailmoituksia lähettäminen

- Käsittelevälle riskin tapahtumien asiaa ja kirjainparit asiayhteyden mukaan tietojen avulla

- Tarjoaa basic työnkulkuja, joilla voit seurata tutkinta

- Helppo käyttää korjaus-toimintoja, kuten salasanan palauttaminen



**Riskiin perustuva ehdollisen käyttöoikeuden käytännöt:**

- Käytäntö pienentämään riskialtis Kirjaudu apuohjelmien estäminen kirjautumiset tai edellyttävän monimenetelmäisen todentamisen haasteita.

- Käytännön estäminen tai suojatun riskialttiit käyttäjätilit

- Käytäntö edellyttää, että käyttäjät monimenetelmäisen todentamisen Rekisteröidy


## <a name="detection-and-risk"></a>Tunnistaminen ja Risk

### <a name="risk-events"></a>Riskin tapahtumat

Riskin tapahtumat ovat tapahtumia, jotka on merkitty epäilyttäväksi mukaan tunnistetiedot ja osoittaa, että jäsenyyden saattaa olla on käsiin. Riskin tapahtumien kattavaan luetteloon on artikkelissa [Tiedostotyypit, riskien tapahtumien havaitsemien Azure Active Directory tunnistetietojen suojaus](active-directory-identityprotection-risk-events-types.md). 


### <a name="risk-level"></a>Riskin taso

Riskin riskin tapahtuman on riski tapahtuman vakavuus tieto (suuri, Normaali tai pieni). Priorisoida ne on otettava riskien vähentämistä organisaation toiminnot tunnistetiedot-käyttäjät voivat käyttää riskin taso. Riskin tapahtuman vakavuus edustaa signaalin voimakkuuden predictor tunnistetietojen vuotamiseen, yhdistää vähentämiseksi, se yleensä esittelee määrää. 

- **Suuri**: luotettavan ja suuri vakavuus riskin tapahtuma. Näitä tapahtumia vahva ilmaisimet, joista käyttäjän tunnistetiedot on ongelma, ja kaikki käyttäjätilit, vaikuttaa olisi remediated heti.

- **Normaali**: hyvin vakavuus, mutta pienempi LUOTTAMUSVÄLI riskin tapahtuman, tai päinvastoin. Näitä tapahtumia ovat mahdollisesti riskialtis, ja kaikki käyttäjätilit, vaikuttaa olisi remediated.

- **Pieni**: pieni LUOTTAMUSVÄLI ja pienen vakavuus riskin tapahtuma. Tapahtuman heti toimintoa ei voi vaatia, mutta riski muita tapahtumia yhdistämällä voi antaa vahva tieto siitä, että käyttäjätietojen käsiin. 


![Riskin taso] (./media/active-directory-identityprotection/01.png "Riskin taso")

 

Riskin tapahtumat tunnistetaan joko **Reaaliaikainen**, tai jälkeinen käsittelyn jälkeen riskin tapahtuma on tehnyt sijoittaminen (offline). Tällä hetkellä useimmat riskin tapahtumat tunnistetiedot on laskettu offline-tilassa ja tunnistetiedot näkyvät 2 – 4 tunnin kuluessa. Kun lasketaan-reaaliaikainen reaaliaikainen riskin tapahtumat näkyvät tunnistetietojen suojaus-konsolin 5-10 minuutin kuluessa.

Useita vanha asiakkaat eivät tällä hetkellä tue reaaliaikainen riskin tapahtuman tunnistaminen ja estäminen. Kirjaudu apuohjelmien näiden asiakkaiden voi seurauksena havaittiin tai esti reaaliajassa.


## <a name="investigation"></a>Tutkimuksen
Ensiaskeleesi tunnistetiedot – alkaa yleensä tunnistetiedot Raporttinäkymät-ikkunan. 

![Korjaus] (./media/active-directory-identityprotection/1000.png "Korjaus")

Koontinäytön kautta pääset käsiksi avulla:
 
- Raporttien, kuten **käyttäjien merkitty riski**, **riskien tapahtumien** ja **heikkouksien**
- Asetukset, kuten **Suojauskäytäntöjä**, **ilmoituksia** ja **monimenetelmäisen todentamisen rekisteröinti** määritys
 

Se on yleensä aloituspiste on toimintojen, lokit ja muut haluamasi tiedot, liittyvien riskien tapahtumasta päättävän korjaus tai rajoituksen vaiheet tarvitaan tarkistaminen, tutkimuksen ja miten tunnistetiedot on käsiin ja ymmärrät vaarantuneen käyttäjätietoja käyttötavoista.

Voit liittää tutkimuksen toimintojen [ilmoitukset](active-directory-identityprotection-notifications.md) Azure Active Directory-suojauksen lähettää sähköpostia kohden.

Seuraavissa osissa on lisätietoja ja ohjeita, jotka liittyvät tutkimuksen.  



## <a name="what-is-a-user-risk-level"></a>Mikä on riski käyttäjätason?

Riskin käyttäjätason on käyttäjän tunnistetiedot käsiin todennäköisyys tieto (suuri, Normaali tai pieni). On laskettu perustuvat käyttäjän riskin tapahtumista, jotka liittyvät käyttäjän tunnistetiedot. 

Riskin tapahtuman tila on **käytössä** vai **Suljettu**. Vain riskin tapahtumia, jotka ovat **aktiivisia** edistää käyttäjän riskin laskemiseen. 

Riskin käyttäjätason lasketaan seuraavat syötteiden avulla:

- Aktiivinen riski tapahtumat vaikuttavat käyttäjä
- Näitä tapahtumia riskin taso 
- Onko korjaus toimintoja toteutettu 

![Käyttäjän riskit] (./media/active-directory-identityprotection/1001.png "Käyttäjän riskit")



Voit käyttää käyttäjän riskin tasot avulla voit luoda ehdollisen käyttöoikeuden käytäntöjä estämään riskialtis käyttäjiä kirjautuminen tai Pakota yleisöä turvallisesti vaihtaa oman salasanansa. 


## <a name="closing-risk-events-manually"></a>Riskin tapahtumien sulkeminen manuaalisesti

Useimmissa tapauksissa kestää korjaus-toimintoja, kuten suojattua salasanan Sulje automaattisesti riskin tapahtumat. Kuitenkin tämä ei aina ehkä voi.  
Näin, esimerkiksi kun:

- Käyttäjä, jolla on aktiivinen riski tapahtumat on poistettu
- Tutkimuksen paljastaa raportoidun riskin tapahtuman suorittaa oikeutettu käyttäjä

Koska riskin tapahtumia, jotka ovat **aktiivisia** vaikuttavat käyttäjä riskin laskentaan, saatat joutua Pienennä manuaalisesti sulkemalla riskin tapahtumat manuaalisesti riskin taso.  
Tutkimuksen aikana voit toteuttaa kaikki riski tapahtuman tilan muuttaminen seuraavat toimet:

![Toiminnot] (./media/active-directory-identityprotection/34.png "Toiminnot")

- **Ratkaise** - jälkeen tutkiminen riskin tapahtuman, joita noudatit haluamasi korjaus-toiminnon ulkopuolella tunnistetiedot ja epäilet, että riski tapahtuman pitää suljettu, merkitse tapahtuman ratkaistu. Ratkaistu tapahtumat Määritä riskin tapahtuman tila suljettu ja riskien tapahtuman enää osallistuu käyttäjän riski.

- **Merkitse positiivisiin** - joissakin tapauksissa voi tutkia tapahtumasta riski ja etsiminen, että se on virheellisesti merkitty riskialtis. Voit pienentää näiden esiintymien määrän merkitsemällä positiivisiin riskin tapahtuman. Tämä auttaa konepohjaisten oppimistekniikoiden algoritmien parantamiseksi vastaavat tapahtumat luokittelu tulevaisuudessa. Positiivisiin tapahtumien tila on **Suljettu** , ja ne eivät enää osallistuu käyttäjän riski.

- **Ohita** – Jos ole tehnyt korjaamisen toiminnon, mutta haluat riskin tapahtuma poistetaan aktiivisen luettelon voit merkitä riskin tapahtuma Ohita ja tapahtuman tila on suljettu. Ohitetut tapahtumat eivät vaikuta käyttäjä riski. Tämä vaihtoehto vain voi käyttää epätavallisia tilanteissa. 

- **Aktivoi** - riski tapahtumia, jotka on suljettu manuaalisesti (valitsemalla **Ratkaise**, **väärä hälytys**tai **Ohita**) voidaan aktivoida uudelleen, tapahtuma-tilan määrittäminen tulee **aktiivinen**. Uudelleen aktivoidut riskin tapahtumien osaltaan käyttäjän riskin tason laskentaan. Riskin tapahtumien suljettu kautta korjaus (kuten suojattua salasanan) voi aktivoida uudelleen. 




**Aiheeseen liittyvät määritykset-valintaikkunan avaaminen**:

1. Valitse **Azure AD-tunnistetiedot** , sivu **Investigate** **riski tapahtumat**.

    ![Manuaalinen salasanan] (./media/active-directory-identityprotection/1002.png "Manuaalinen salasanan")

2. Valitse riskin **riskin tapahtumat** -luettelosta.

    ![Manuaalinen salasanan] (./media/active-directory-identityprotection/1003.png "Manuaalinen salasanan")

2. Riski-sivu, napsauta hiiren kakkospainikkeella käyttäjä.

    ![Manuaalinen salasanan] (./media/active-directory-identityprotection/1004.png "Manuaalinen salasanan")



### <a name="closing-all-risk-events-for-a-user-manually"></a>Sulkemalla kaikki riski tapahtumat käyttäjän manuaalisesti

Sijaan suljet manuaalisesti riskin tapahtumien käyttäjän erikseen, Azure Active Directory tunnistetietojen suojaus sisältää myös menetelmän Sulje kaikki tapahtumat käyttäjän yhdellä hiiren napsautuksella.


![Toiminnot] (./media/active-directory-identityprotection/2222.png "Toiminnot")

Kun napsautat **Hylkää kaikki tapahtumat**, kaikki tapahtumat suljetaan ja tarvittavien käyttäjä ei ole enää vastuullasi.



## <a name="remediating-user-risk-events"></a>Idfixin käyttäjän riskin tapahtumat

Korjaus on toiminto suojaamiseen jäsenyyden tai laitteella, joka oli aiemmin epäilty tai tiedossa on käsiin. Korjaus-toiminto palauttaa tunnistetiedot tai laite on turvallista ja korjaa edellisen tunnistetiedot tai laitteen liittyvien riskien tapahtumien.

Riskien parantaminen käyttäjän riskin tapahtumia, voit:

- Suorittaa suojattua salasanan, riskien käyttäjän riskin tapahtumien parantaminen manuaalisesti 

- Määrittää käyttäjän riskin suojauskäytäntö pienentämään tai riskien käyttäjän riskin tapahtumat automaattisesti parantaminen

- Kuvan uudelleen tartunnan laite  


### <a name="manual-secure-password-reset"></a>Manuaalinen suojattua salasanan palauttaminen

Suojattua salasanan on tehokas korjaus, monta riskin tapahtumien ja suoritetaan, kun automaattisesti sulkee riskin näitä tapahtumia ja laskee riskin käyttäjätason. Voit aloittaa riskialtis käyttäjän salasanan tunnistetiedot Raporttinäkymät-ikkunan. 

Aiheeseen liittyvät valintaikkuna sisältää salasanan nollaaminen kahdella tavalla:

Monimenetelmäisen todentamisen **salasanan** – Valitse **Edellytä, että käyttäjä salasanan** voit antaa käyttäjän itse Palauta, jos käyttäjä on rekisteröity. Aikana käyttäjän seuraava-kirjautuminen-käyttäjä pakollinen ratkaisemaan monimenetelmäisen todentamisen hankala onnistuneesti ja pakko sitten Vaihda salasana. Tämä vaihtoehto ei ole käytettävissä, jos käyttäjätili ei ole jo rekisteröity monimenetelmäisen todentamisen.

**Tilapäinen salasana** - valitsemalla **Luo tilapäinen salasana** heti vahingoittaa nykyinen salasana ja luo uusi tilapäinen salasana käyttäjälle. Lähetä uusi tilapäinen salasana, että vaihtoehtoinen sähköpostiosoite käyttäjän ja hänen esimiehensä. Koska salasana on väliaikainen, käyttäjää pyydetään salasanan kirjautumisen yhteydessä.


![Käytäntö] (./media/active-directory-identityprotection/1005.png "Käytäntö")


**Aiheeseen liittyvät määritykset-valintaikkunan avaaminen**:

1. Valitse **Azure AD-tunnistetiedot** , sivu **käyttäjät, jotka on merkitty riskin**.

    ![Manuaalinen salasanan] (./media/active-directory-identityprotection/1006.png "Manuaalinen salasanan")


2. Valitse käyttäjät-luettelosta käyttäjä, jolla on vähintään yksi riskin tapahtumat.

    ![Manuaalinen salasanan] (./media/active-directory-identityprotection/1007.png "Manuaalinen salasanan")


2. Valitse käyttäjä-sivu **salasanan**.

    ![Manuaalinen salasanan] (./media/active-directory-identityprotection/1008.png "Manuaalinen salasanan")





## <a name="user-risk-security-policy"></a>Käyttäjän riskin suojauskäytäntö

Käyttäjän riskin suojauskäytäntö on ehdollisen käyttöoikeuden käytännön, joka arvioi tietyn käyttäjän riskin taso ja korjaus ja rajoituksen toiminnot ennalta määritettyjä ehtoja ja sääntöjen perusteella.


![Käyttäjän ridk käytäntöä] (./media/active-directory-identityprotection/1009.png "Käyttäjän ridk käytäntöä")


Azure AD-tunnistetietojen suojaus auttaa hallitsemaan rajoituksen ja korjaamisen käyttäjien riskin merkinnyt voit:

- Määritä käyttäjät ja ryhmät käytäntö: 

    ![Käyttäjän ridk käytäntöä] (./media/active-directory-identityprotection/1010.png "Käyttäjän ridk käytäntöä")


- Määrittää käyttäjän riskin tason raja-arvon (pieni, Normaali tai suuri), joka käynnistää käytäntö: 

    ![Käyttäjän ridk käytäntöä] (./media/active-directory-identityprotection/1011.png "Käyttäjän ridk käytäntöä")


- Määritä ohjausobjektit suoritettaviksi, kun käytäntö:

    ![Käyttäjän ridk käytäntöä] (./media/active-directory-identityprotection/1012.png "Käyttäjän ridk käytäntöä")


- Siirtyä käytäntöjen tilan seuraavasti:

    ![Käyttäjän ridk käytäntöä] (./media/active-directory-identityprotection/403.png "MFA-rekisteröinti")


- Tarkista ja arvioi ennen aktivointia sen muutoksen vaikutus:

    ![Käyttäjän ridk käytäntöä] (./media/active-directory-identityprotection/1013.png "Käyttäjän ridk käytäntöä")


Kuinka monta kertaa käytännön käynnistyy ja pienentää käyttäjiin vaikuttavia tekijöitä vähentää valitseminen **hyvin** raja-arvon.
Kuitenkin ulkopuolelle **Pieni** ja **Normaali** käyttäjät merkitty riskin käytännöstä, joka ei suojaa käyttäjätietojen tai laitteiden aiemmin epäilty tai tiedossa on käsiin.

Määrittäessäsi käytäntöä

- Poistaa käyttäjät, jotka todennäköisesti Luo useita false-positiivisten (kehittäjät, suojauksen analyytikot)

- Jätä pois käyttäjät, jossa ottaminen käyttöön käytäntöä ei ole käytännön kielialueilla (esimerkiksi ei voi käyttää tukipalvelu)

- Käytä **suuri** kynnysarvo aikana alkuperäisen käytännön kokoa, tai jos on Pienennä haasteita tarkastella käyttäjät.

- Käytä **Pieni** raja-arvon, jos organisaatio edellyttää paremman suojauksen. Lisää käyttäjä-kirjautuminen haasteisiin, mutta parantavat turvallisuutta valitsemalla **Pieni** kynnysarvo esitellään.

Useimmat organisaatiot suositellut oletusarvo on **Normaali** -kynnysarvo tasapaino käytettävyyttä ja tietoturvan välillä säännön määrittäminen.

Yleiskuvaus liittyvät käyttäjäkokemuksen on seuraavissa artikkeleissa:

- [Compromised tilin palautus työnkulku](active-directory-identityprotection-flows.md#compromised-account-recovery).  

- [Compromised tilin estetyt työnkulku](active-directory-identityprotection-flows.md#compromised-account-blocked).  


**Aiheeseen liittyvät määritykset-valintaikkunan avaaminen**:

1. Valitse **käyttäjän riskin käytäntöä** **Azure AD-tunnistetiedot** , sivu **määrittäminen** -osassa.

    ![Käyttäjän ridk käytäntöä] (./media/active-directory-identityprotection/1009.png "Käyttäjän ridk käytäntöä")






## <a name="mitigating-user-risk-events"></a>Pienentävät käyttäjä riskin tapahtumat
Järjestelmänvalvojat voivat määrittää käyttäjän riskin suojauskäytäntö yhteydessä Kirjautumisvirheitä aiheuttavien roskapostisuodattimeen määritetyn riskin käyttäjien estäminen. 

Esto kirjautumisvirheiden:
 
- Estää uuden käyttäjän riskin tapahtumat käyttäjän luominen

- Järjestelmänvalvojat voivat manuaalisesti riskien vaikuttavia käyttäjän tunnistetiedot riskin tapahtumat parantaminen ja palauttaa laitteen suojattu tila



## <a name="what-is-a-sign-in-risk-level"></a>Mikä on kirjautumisen riskin tason?

Kirjautumisen riskin taso on todennäköisyys, että tietyn kirjautumista varten, joku yrittää käyttäjän tunnistetiedot todentamismenetelmä tieto (suuri, Normaali tai pieni). Kirjaudu sisään riskin taso arvioidaan ajankohtana Kirjaudu sisään ja katsoo riskin tapahtumat, ja ilmaisimet havaittiin reaaliaikainen kyseisen tietyn kirjautumista varten. 

## <a name="mitigating-sign-in-risk-events"></a>Pienentävät kirjautumisen riskin tapahtumat 
Rajoitus on toiminto, voit rajoittaa luvaton voi hyödyntää vaarantuneen tunnistetiedot tai laitteen ilman palauttaminen tunnistetiedot tai laite on turvallista. Edellisen kirjautumisen riskin tapahtumien tunnistetiedot tai laitteen liittyvää ei ratkaise rajoituksen.

Voit Azure AD-tunnistetiedot ehdollisen käyttöoikeuden pienentämään kirjautumisen riskin tapahtumat automaattisesti. Käytä käytännöt, harkitse käyttäjä tai kirjautuminen-riskialttiit Kirjaudu apuohjelmien estosta tai käyttäjän on suoritettava monimenetelmäisen todentamisen riskin taso. Nämä toiminnot saattavat estää luvaton hyödyntää sitä käyttäjätietoja aiheuttaa vahinkoa ja voi antaa sinulle suojaamiseen käyttäjätietoja jonkin aikaa. 


## <a name="sign-in-risk-security-policy"></a>Kirjaudu sisään riskin suojauskäytäntö

Kirjaudu sisään riski-käytäntö on ehdollisen käyttöoikeuden käytännön, joka arvioi riskin tiettyyn sisäänkirjautumisongelmien ja koskee ongelman pienentämistavat ennalta määritettyjä ehtoja ja sääntöjen perusteella.

![Kirjaudu sisään riskin käytäntö] (./media/active-directory-identityprotection/1014.png "Kirjaudu sisään riskin käytäntö")


Azure AD-tunnistetietojen suojaus auttaa hallitsemaan riskialtis Kirjaudu apuohjelmien vähentäminen avulla voit:

- Määritä käyttäjät ja ryhmät käytäntö: 

    ![Kirjaudu sisään riskin käytäntö] (./media/active-directory-identityprotection/1015.png "Kirjaudu sisään riskin käytäntö")


- Määritä kirjautumisen riskin tason raja-arvon (pieni, Normaali tai suuri), joka käynnistää käytäntö: 

    ![Kirjaudu sisään riskin käytäntö] (./media/active-directory-identityprotection/1016.png "Kirjaudu sisään riskin käytäntö")


- Määritä ohjausobjektit suoritettaviksi, kun käytäntö:  

    ![Kirjaudu sisään riskin käytäntö] (./media/active-directory-identityprotection/1017.png "Kirjaudu sisään riskin käytäntö")
    

- Siirtyä käytäntöjen tilan seuraavasti:

    ![MFA-rekisteröinti] (./media/active-directory-identityprotection/403.png "MFA-rekisteröinti")

- Tarkista ja arvioi ennen aktivointia sen muutoksen vaikutus: 

    ![Kirjaudu sisään riskin käytäntö] (./media/active-directory-identityprotection/1018.png "Kirjaudu sisään riskin käytäntö")


### <a name="what-you-need-to-know"></a>Mitä on tiedettävä

Voit määrittää kirjautumisen riskin suojauskäytäntö edellyttämään monimenetelmäisen todentamisen:

![Kirjaudu sisään riskin käytäntö] (./media/active-directory-identityprotection/1017.png "Kirjaudu sisään riskin käytäntö")

Kuitenkin tietoturvasyistä tämä asetus toimii vain käyttäjät, joilla on jo rekisteröity monimenetelmäisen todentamisen. Jos edellyttämään monimenetelmäisen todentamisen ehto täyttyy käyttäjälle, joka ei ole vielä rekisteröity monimenetelmäisen todentamisen, käyttäjän on estetty. 

Paras käytäntö, jos haluat vaatia monimenetelmäisen todentamisen riskialtis merkki-laajennukset huomioon:

1. [Monimenetelmäisen todentamisen rekisteröinnin käytännön](#multi-factor-authentication-registration-policy) tarvittavien käyttäjien käyttöön.
2. Edellytä kirjautumista tarvittavien käyttäjien riskialttiit istunnossa MFA rekisteröintiä

Näiden vaiheiden suorittamisen varmistaa, että monimenetelmäisen todentamisen, jota tarvitaan riskialttiit kirjautumisnimi. 


### <a name="best-practices"></a>Parhaat käytännöt

 
Kuinka monta kertaa käytännön käynnistyy ja pienentää käyttäjiin vaikuttavia tekijöitä vähentää valitseminen **hyvin** raja-arvon.  
 
Kuitenkin ulkopuolelle **Pieni** ja **Normaali** kirjautumiset merkitty riskin käytännöstä, jotka saattavat estää ei voi saada-hyödyntää vaarantuneen tunnistetiedot. 

Määrittäessäsi käytäntöä 

- Käyttäjät, jotka eivät ole / ei voi olla monimenetelmäisen todentamisen jättäminen pois

- Jätä pois käyttäjät, jossa ottaminen käyttöön käytäntöä ei ole käytännön kielialueilla (esimerkiksi ei voi käyttää tukipalvelu)

- Poistaa käyttäjät, jotka todennäköisesti Luo useita false-positiivisten (kehittäjät, suojauksen analyytikot)

- Käytä **suuri** kynnysarvo aikana alkuperäisen käytännön kokoa, tai jos on Pienennä haasteita tarkastella käyttäjät.

- Käytä **Pieni** raja-arvon, jos organisaatio edellyttää paremman suojauksen. Lisää käyttäjä-kirjautuminen haasteisiin, mutta parantavat turvallisuutta valitsemalla **Pieni** kynnysarvo esitellään.

Useimmat organisaatiot suositellut oletusarvo on **Normaali** -kynnysarvo tasapaino käytettävyyttä ja tietoturvan välillä säännön määrittäminen.

 
Kirjaudu sisään riskin käytäntö on:

- Kaikki selaimen liikenteen ja kirjaudu apuohjelmien käyttäminen Nykyaikainen todentaminen käyttöön.
- Sovellusten WS luotettavaksi päätepiste kohteessa liitetyt IDP ADFS kuten poistamalla vanhat suojauksen protokollat ei käytetä.

Tunnistetiedot-konsolissa **Riskin tapahtumat** -sivulla on lueteltu kaikki tapahtumat:

- Tämän käytännön käyttöön
- Voit tarkastella tehtävän ja selvittää, onko toiminnon oli oikea, tai ei 

Yleiskuvaus liittyvät käyttäjäkokemuksen on seuraavissa artikkeleissa:

- [Riskialttiit kirjautumisen palauttaminen](active-directory-identityprotection-flows.md#risky-sign-in-recovery) 

- [Riskialttiit-kirjautuminen estetty](active-directory-identityprotection-flows.md#risky-sign-in-blocked)  

- [Monimenetelmäisen todentamisen rekisteröinti riskialtis kirjautumisvirheiden aikana](active-directory-identityprotection-flows.md#multi-factor-authentication-registration-during-a-risky-sign-in)  





**Aiheeseen liittyvät määritykset-valintaikkunan avaaminen**:

1. Valitse **Kirjaudu sisään riskin käytännön** **Azure AD-tunnistetiedot** , sivu **määrittäminen** -osassa.

    ![Käyttäjän ridk käytäntöä] (./media/active-directory-identityprotection/1014.png "Käyttäjän ridk käytäntöä")





## <a name="multi-factor-authentication-registration-policy"></a>Monimenetelmäisen todentamisen rekisteröinti käytäntö

Azure monimenetelmäisen todentamisen on menetelmä tarkistetaan henkilöt, joka edellyttää enemmän kuin vain käyttäjätunnusta ja salasanaa. Se on toisen kerroksen arvopaperin käyttäjän kirjautumiset ja tapahtumia.  
On suositeltavaa, että asetat Azure monimenetelmäisen todentamisen käyttäjän kirjautumiset koska sitä:

- Toimittaa vahva todennus helposti vahvistus asetukset-alue

- Toistaa roolissa valmistellaan organisaation suojaaminen ja tilin kompromisseja palauttaminen

![Käyttäjän ridk käytäntöä] (./media/active-directory-identityprotection/1019.png "Käyttäjän ridk käytäntöä")



Lisätietoja on artikkelissa [Mikä Azure Monimenetelmäisen todentamisen?](../multi-factor-authentication/multi-factor-authentication.md)


Azure AD-tunnistetietojen suojauksen avulla voit hallita monimenetelmäisen todentamisen rekisteröinnin lisensointi määrittämällä käytäntö, jonka avulla voit: 



- Määritä käyttäjät ja ryhmät käytäntö: 

    ![MFA-käytäntö] (./media/active-directory-identityprotection/1020.png "MFA-käytäntö")



- Käytössä on otettava käyttöön, kun käytäntö:  

    ![MFA-käytäntö] (./media/active-directory-identityprotection/1021.png "MFA-käytäntö")


- Siirtyä käytäntöjen tilan seuraavasti:

    ![MFA-käytäntö] (./media/active-directory-identityprotection/403.png "MFA-käytäntö")

- Rekisteröinti nykyisen tilan tarkasteleminen: 

    ![MFA-käytäntö] (./media/active-directory-identityprotection/1022.png "MFA-käytäntö")


Yleiskuvaus liittyvät käyttäjäkokemuksen on seuraavissa artikkeleissa:

- [Monimenetelmäisen todentamisen rekisteröinnin työnkulku](active-directory-identityprotection-flows.md#multi-factor-authentication-registration).  

- [Monimenetelmäisen todentamisen rekisteröinnin aikana riskialtis kirjautumisnimi](active-directory-identityprotection-flows.md#multi-factor-authentication-registration-during-a-risky-sign-in).  





**Aiheeseen liittyvät määritykset-valintaikkunan avaaminen**:

1. Valitse **monimenetelmäisen todentamisen rekisteröinti** **Azure AD-tunnistetiedot** , sivu **määrittäminen** -osassa.

    ![MFA-käytäntö] (./media/active-directory-identityprotection/1019.png "MFA-käytäntö")





## <a name="next-steps"></a>Seuraavat vaiheet

 - [Kanavan 9: Azure AD ja käyttäjätiedot näkyvät: Identity suojaus esikatselu](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)
 - [Riskin tapahtumien havaitsemien Azure Active Directory tunnistetietojen suojaus](active-directory-identityprotection-risk-events-types.md)
 - [Heikkouksien havaitsemien Azure Active Directory tunnistetietojen suojaus](active-directory-identityprotection-vulnerabilities.md)
 - [Azure Active Directory-tunnistetietojen suojaus-ilmoitukset](active-directory-identityprotection-notifications.md)
 - [Azure Active Directory tunnistetietojen suojauksen playbook](active-directory-identityprotection-playbook.md)
 - [Azure Active Directory tunnistetietojen suojaus-sanasto](active-directory-identityprotection-glossary.md)

 - [Kirjaudu sisään kokemukset Azure AD-tunnistetiedot](active-directory-identityprotection-flows.md)

 - [Azure Active Directory-tunnistetietojen suojauksen ottaminen käyttöön](active-directory-identityprotection-enable.md)
 - [Azure Active Directory tunnistetietojen suojaus - käyttäjien eston poistaminen](active-directory-identityprotection-unblock-howto.md)

 - [Azure Active Directory tunnistetietojen suojaus- ja Microsoft Graph käytön aloittaminen](active-directory-identityprotection-graph-getting-started.md)


